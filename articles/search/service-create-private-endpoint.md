---
title: Güvenli bağlantı için özel uç nokta oluşturma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama hizmetine güvenli bağlantı için bir sanal ağda özel uç nokta ayarlama
manager: nitinme
author: mrcarter8
ms.author: mcarter
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: cfa8db0d00f351f5ab2bda96744305ca83cccb19
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75922463"
---
# <a name="create-a-private-endpoint-for-a-secure-connection-to-azure-cognitive-search-preview"></a>Azure Bilişsel Arama güvenli bağlantısı için özel bir uç nokta oluşturma (Önizleme)

Azure Bilişsel Arama [Özel uç noktaları](../private-link/private-endpoint-overview.md) , bir sanal ağdaki bir Istemcinin bir [özel bağlantı](../private-link/private-link-overview.md)üzerinden arama dizinindeki verilere güvenli bir şekilde erişmesini sağlar. Özel uç nokta, arama hizmetiniz için [sanal ağ adres alanından](../virtual-network/virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) bir IP adresi kullanır. İstemci ile arama hizmeti arasındaki ağ trafiği, sanal ağın ve Microsoft omurga ağındaki özel bir bağlantının üzerinde geçiş yaparken, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. Özel bağlantıyı destekleyen diğer PaaS hizmetlerinin bir listesi için ürün belgelerindeki [kullanılabilirlik bölümüne](../private-link/private-link-overview.md#availability) bakın.

> [!Important]
> Azure Bilişsel Arama için özel uç nokta desteği, sınırlı erişim önizlemesi olarak kullanılabilir ve şu anda üretim kullanımı için tasarlanmamıştır. Önizlemeye erişmek istiyorsanız lütfen [erişim isteği formunu](https://aka.ms/SearchPrivateLinkRequestAccess) doldurun ve iletin. Form, siz, şirketiniz ve genel uygulama mimariniz hakkında bilgi ister. İsteğinizi gözden geçirdikten sonra ek yönergeler içeren bir onay e-postası alacaksınız.
>
> Önizlemeye erişim izni verdikten sonra, Azure portal ve sürüm [2019-10-06-önizleme](search-api-preview.md)REST API kullanarak hizmetinize yönelik özel uç noktaları yapılandırabileceksiniz.
>   

Arama hizmetiniz için özel uç noktalar şunları yapmanızı sağlar:

- Arama hizmetiniz için genel uç noktasındaki tüm bağlantıları engelleyin.
- Sanal ağ üzerinden veri alımını engellemeyi etkinleştirerek sanal ağ güvenliğini artırın.
- Sanal ağa [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](../expressroute/expressroute-locations.md) kullanarak özel eşleme ile bağlanan şirket içi ağlardan arama hizmetinize güvenli bir şekilde bağlanın.

> [!NOTE]
> Önizlemede bilmeniz gereken bazı sınırlamalar vardır:
> * Yalnızca **temel** katmandaki arama hizmetleri için kullanılabilir. 
> * Batı ABD 2, Orta Batı ABD, Doğu ABD, Orta Güney ABD, Avustralya Doğu ve Avustralya Güneydoğu bölgelerinde kullanılabilir.
> * Hizmet uç noktası özel olduğunda, bazı Portal özellikleri devre dışı bırakılır. Hizmet düzeyi bilgilerini görüntüleyebilir ve yönetebileceksiniz, ancak dizin verilerine ve hizmette Dizin, Dizin Oluşturucu ve beceri tanımları gibi çeşitli bileşenlere erişim sağlamak için Portal erişimi güvenlik nedenleriyle kısıtlıdır.
> * Hizmet uç noktası özel olduğunda, dizine belge yüklemek için Search API 'sini kullanmanız gerekir.
> * Azure portal özel uç nokta desteği seçeneğini görmek için aşağıdaki bağlantıyı kullanmanız gerekir: https://portal.azure.com/?feature.enablePrivateEndpoints=true

Bu makalede, bir genel IP adresi aracılığıyla erişilemeyen yeni bir Azure Bilişsel Arama hizmet örneği oluşturmak, aynı sanal ağ içinde bir Azure sanal makinesini yapılandırmak ve bunu özel bir işlem aracılığıyla arama hizmetine erişmek için kullanmak üzere portalını nasıl kullanacağınızı öğreneceksiniz. bkz.


## <a name="create-a-vm"></a>VM oluşturma
Bu bölümde, arama hizmetinizin özel uç noktasına erişmek için kullanılacak VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

1. Azure portal giriş sekmesinde, > **ağ** > **sanal ağ**' **a kaynak oluştur** ' u seçin.

1. **Sanal ağ oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | *MyVirtualNetwork* girin |
    | Adres alanı | *10.1.0.0/16* girin |
    | Abonelik | Aboneliğinizi seçme|
    | Kaynak grubu | **Yeni oluştur**' u seçin, *Myresourcegroup*yazın ve **Tamam** ' ı seçin. |
    | Konum | **Batı ABD** veya hangi bölgeyi kullandığınızı seçin|
    | Alt ağ adı | *Mysubnet* girin |
    | Alt Ağ - Adres aralığı | *10.1.0.0/24* girin |
    |||

1. Rest 'i varsayılan olarak bırakın ve **Oluştur**' u seçin.


## <a name="create-your-search-service-with-a-private-endpoint"></a>Özel bir uç nokta ile arama hizmetinizi oluşturma

Bu bölümde, özel bir uç nokta ile yeni bir Azure Bilişsel Arama hizmeti oluşturacaksınız. 

1. Azure portal ekranın sol üst tarafında, **Web** > **Azure bilişsel arama** > **kaynak oluştur** ' u seçin.

1. **Yeni arama hizmeti temel kavramları**için bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRıNTıLARı** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | **ÖRNEK AYRıNTıLARı** |  |
    | URL | Benzersiz bir ad girin. |
    | Konum | Bu Önizleme özelliğine erişim istenirken belirttiğiniz bölgeyi seçin. |
    | Fiyatlandırma katmanı | **Fiyatlandırma katmanını Değiştir** ' i seçin ve **temel**öğesini seçin. Önizleme için bu katman gereklidir. |
    |||
  
1. **İleri ' yi seçin: ölçek**.

1. Değerleri varsayılan olarak bırakın ve Ileri ' **yi seçin: ağ**.

1. **Yeni arama hizmeti-ağ**' da, **uç nokta bağlantısı (veriler)** için **özel** ' i seçin.

1. **Yeni arama hizmeti-ağ**' da, **Özel uç nokta**altında **+ Ekle** ' yi seçin. 

1. **Özel uç nokta oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | Konum | **Batı ABD**seçin.|
    | Ad |  *Myprivateendpoint*girin.  |
    | Hedef alt kaynak | Varsayılan **SearchService**' i bırakın. |
    | **IŞLEMLERI** |  |
    | Sanal ağ  | *Myresourcegroup*kaynak grubundan *MyVirtualNetwork* öğesini seçin. |
    | Alt ağ |  *Mysubnet*öğesini seçin. |
    | **ÖZEL DNS TÜMLEŞTIRMESI** |  |
    | Özel DNS bölgesiyle tümleştirin  | Varsayılan **Evet**' i bırakın. |
    | Özel DNS bölgesi  | Varsayılan * * (yeni) privatelink.search.windows.net * * ' i bırakın. |
    |||

1. **Tamam**’ı seçin. 

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz. 

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin. 

1. Yeni hizmetinizin sağlanması tamamlandıktan sonra, az önce oluşturduğunuz kaynağa gidin.

1. Sol içerik menüsünde **anahtarlar** ' ı seçin.

1. **Birincil yönetici anahtarını** daha sonra kopyalayın.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturun

1. Azure portal ekranın sol üst kısmında, **sanal makine** > **Işlem** > **kaynak oluştur** ' u seçin.

1. **Sanal makine oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRıNTıLARı** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.  |
    | **ÖRNEK AYRıNTıLARı** |  |
    | Sanal makine adı | *Myvm*' i girin. |
    | Bölge | **Batı ABD** veya kullandığınız bölgeyi seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan **altyapı yedekliliği gerekli değildir**. |
    | Resim | **Windows Server 2019 Datacenter**öğesini seçin. |
    | Boyut | Varsayılan **Standart DS1 v2**' i bırakın. |
    | **YÖNETICI HESABı** |  |
    | Kullanıcı adı | Seçmekten bir Kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTı NOKTASı KURALLARı** |  |
    | Genel gelen bağlantı noktası | Varsayılan olarak **Seçili bağlantı noktalarına Izin ver**' i bırakın. |
    | Gelen bağlantı noktalarını seçin | Varsayılan RDP 'yi **(3389)** bırakın. |
    | **TASARRUF EDIN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan **Hayır**olarak bırakın. |
    |||

1. **İleri ' yi seçin: diskler**.

1. **Sanal makine oluşturma-diskler**' de, varsayılan değerleri bırakın ve **İleri ' yi seçin: ağ**.

1. **Sanal makine oluşturma-ağ oluşturma**bölümünde şu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan **MyVirtualNetwork**bırakın.  |
    | Adres alanı | Varsayılan **10.1.0.0/24**' i bırakın.|
    | Alt ağ | Varsayılan **Mysubnet (10.1.0.0/24)** olarak bırakın.|
    | Genel IP | Varsayılan **(yeni) myVm-ip**' i bırakın. |
    | Genel gelen bağlantı noktası | **Seçili bağlantı noktalarına Izin ver**' i seçin. |
    | Gelen bağlantı noktalarını seçin | **Http** ve **RDP**' yi seçin.|
    ||

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin. 


## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi, internet *'ten gelen VM VM* 'sine bağlanın:

1. Portalın arama çubuğunda *Myvm*' i girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. Seçin **RDP dosyasını indir**. Azure bir Uzak Masaüstü Protokolü ( *. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen. rdp * dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

        > [!NOTE]
        > VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için **farklı bir hesap kullanmak** > **daha fazla seçenek** belirlemeniz gerekebilir.

1. **Tamam**’ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.  


## <a name="access-the-search-service-privately-from-the-vm"></a>Arama hizmetine özel olarak VM 'den erişin

Bu bölümde, arama hizmetine özel ağ erişimini doğrulayacaksınız ve özel uç noktayı kullanarak özel olarak bağlanılacak şekilde bağlanıyorsunuz.

1.  *Myvm*uzak masaüstünde PowerShell ' i açın.

1. ' Nslookup [arama hizmeti adı]. Search. Windows. net ' yazın

    Şuna benzer bir ileti alacaksınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    [search service name].privatelink.search.windows.net
    Address:  10.0.0.5
    Aliases:  [search service name].search.windows.net
    ```
1. REST API kullanarak Postman 'da hizmetinize yeni bir arama dizini oluşturmak için VM 'deki bu [hızlı](search-get-started-postman.md) başlangıcı izleyin.  Hizmette kimlik doğrulaması yapmak için önceki bir adımda kopyaladığınız anahtarı kullanın.

1. Yerel iş istasyonunuzda Postman ile aynı isteklerin birkaçını deneyin.

1. Sanal makineyi VM 'den tamamlayabiliyor, ancak yerel iş istasyonunuzda uzak sunucunun mevcut olmadığı bir hata alırsanız, arama hizmetiniz için bir özel uç noktayı başarıyla yapılandırdınız.

1.  *Myvm*ile uzak masaüstü bağlantısını kapatın. 


## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel uç nokta, arama hizmeti ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:
1. Portalın üst kısmındaki **arama** kutusuna *myresourcegroup* girin ve arama sonuçlarından *myresourcegroup* öğesini seçin. 
1. **Kaynak grubunu sil**'i seçin. 
1. **Kaynak grubu adını yazmak** Için *myresourcegroup* girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir sanal ağ üzerinde bir VM ve özel bir uç nokta içeren bir arama hizmeti oluşturdunuz. VM 'ye internet 'ten bağlanır ve özel bağlantı kullanarak arama hizmetine güvenli bir şekilde iletilecaksınız. Özel uç nokta hakkında daha fazla bilgi edinmek için bkz. [Azure özel uç noktası nedir?](../private-link/private-endpoint-overview.md).
