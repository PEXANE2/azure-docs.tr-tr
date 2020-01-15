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
ms.openlocfilehash: 2664b1abd4131cf1dca186c7b044e338bf1efa84
ms.sourcegitcommit: 49e14e0d19a18b75fd83de6c16ccee2594592355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75945816"
---
# <a name="create-a-private-endpoint-for-a-secure-connection-to-azure-cognitive-search-preview"></a>Azure Bilişsel Arama güvenli bağlantısı için özel bir uç nokta oluşturma (Önizleme)

Bu makalede, genel bir IP adresi üzerinden erişilemeyen yeni bir Azure Bilişsel Arama hizmet örneği oluşturmak için portalını kullanın. Ardından, bir Azure sanal makinesini aynı sanal ağda yapılandırın ve özel bir uç nokta aracılığıyla arama hizmetine erişmek için kullanın.

> [!Important]
> Azure Bilişsel Arama için özel uç nokta desteği, [istek üzerine](https://aka.ms/SearchPrivateLinkRequestAccess) sınırlı erişim önizlemesi olarak sağlanır. Önizleme özellikleri bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
>
> Önizlemeye erişim verdikten sonra, Azure portal veya [yönetim REST API 2019-10-06-Preview sürümünü](https://docs.microsoft.com/rest/api/searchmanagement/)kullanarak hizmetinize yönelik özel uç noktaları yapılandırabileceksiniz.
>   

## <a name="why-use-private-endpoint-for-secure-access"></a>Güvenli erişim için özel uç nokta neden kullanılmalıdır?

Azure Bilişsel Arama [Özel uç noktaları](../private-link/private-endpoint-overview.md) , bir sanal ağdaki bir Istemcinin bir [özel bağlantı](../private-link/private-link-overview.md)üzerinden arama dizinindeki verilere güvenli bir şekilde erişmesini sağlar. Özel uç nokta, arama hizmetiniz için [sanal ağ adres alanından](../virtual-network/virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) bir IP adresi kullanır. İstemci ile arama hizmeti arasındaki ağ trafiği, sanal ağın ve Microsoft omurga ağındaki özel bir bağlantının üzerinde geçiş yaparken, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. Özel bağlantıyı destekleyen diğer PaaS hizmetlerinin bir listesi için ürün belgelerindeki [kullanılabilirlik bölümüne](../private-link/private-link-overview.md#availability) bakın.

Arama hizmetiniz için özel uç noktalar şunları yapmanızı sağlar:

- Arama hizmetiniz için genel uç noktasındaki tüm bağlantıları engelleyin.
- Sanal ağ üzerinden veri alımını engellemeyi etkinleştirerek sanal ağ güvenliğini artırın.
- Sanal ağa [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](../expressroute/expressroute-locations.md) kullanarak özel eşleme ile bağlanan şirket içi ağlardan arama hizmetinize güvenli bir şekilde bağlanın.

> [!NOTE]
> Önizlemede bilmeniz gereken bazı sınırlamalar vardır:
> * Yalnızca **temel** katmandaki arama hizmetleri için kullanılabilir. 
> * Batı ABD 2, Orta Batı ABD, Doğu ABD, Orta Güney ABD, Avustralya Doğu ve Avustralya Güneydoğu bölgelerinde kullanılabilir.
> * Hizmet uç noktası özel olduğunda, bazı Portal özellikleri devre dışı bırakılır. Hizmet düzeyi bilgilerini görüntüleyebilir ve yönetebileceksiniz, ancak dizin verilerine ve hizmette Dizin, Dizin Oluşturucu ve beceri tanımları gibi çeşitli bileşenlere erişim sağlamak için Portal erişimi güvenlik nedenleriyle kısıtlıdır.
> * Hizmet uç noktası özel olduğunda, dizine belge yüklemek için [arama REST API](https://docs.microsoft.com/rest/api/searchservice/) kullanmanız gerekir.
> * Azure portal özel uç nokta desteği seçeneğini görmek için aşağıdaki bağlantıyı kullanmanız gerekir: https://portal.azure.com/?feature.enablePrivateEndpoints=true



## <a name="request-access"></a>Erişim izni isteme 

Bu önizleme özelliği için kaydolmak üzere [erişim iste](https://aka.ms/SearchPrivateLinkRequestAccess) ' ye tıklayın. Form, siz, şirketiniz ve genel ağ topolojiniz hakkında bilgi ister. İsteğinizi gözden geçirdikten sonra ek yönergeler içeren bir onay e-postası alacaksınız.

## <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, arama hizmetinizin özel uç noktasına erişmek için kullanılacak VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız.

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


## <a name="create-a-search-service-with-a-private-endpoint"></a>Özel bir uç nokta ile arama hizmeti oluşturma

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

1. Hizmete bağlanırken, daha sonra **birincil yönetici anahtarını** kopyalayın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturun

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


## <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM *myvm* ' yi indirip şu şekilde bağlayın:

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


## <a name="test-connections"></a>Sınama bağlantıları

Bu bölümde, arama hizmetine özel ağ erişimini doğrulayacaksınız ve özel uç noktayı kullanarak özel olarak bağlanılacak şekilde bağlanıyorsunuz.

Arama hizmeti ile tüm etkileşimlerin [arama REST API](https://docs.microsoft.com/rest/api/searchservice/)gerektirdiğini giriş noktasından geri çekin. Portal ve .NET SDK bu önizlemede desteklenmez.

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

1. VM 'den arama hizmetine bağlanın ve bir dizin oluşturun. REST API kullanarak Postman 'da hizmetinize yeni bir arama dizini oluşturmak için bu [hızlı](search-get-started-postman.md) başlangıcı izleyebilirsiniz. İstekleri Postman 'dan ayarlamak için arama hizmeti uç noktası (https://[arama hizmeti adı]. Search. Windows. net) ve önceki adımda kopyaladığınız yönetici API anahtarı gerekir.

1. VM 'den hızlı başlangıç işlemini tamamlamak, hizmetin tam olarak çalışır durumda olduğunu onaylamadır.

1.  *Myvm*ile uzak masaüstü bağlantısını kapatın. 

1. Hizmetinizin genel bir uç noktada erişilebilir olmadığından emin olmak için, yerel iş istasyonunuzda Postman ' ı açın ve hızlı başlangıçta ilk birkaç görevi deneyin. Uzak sunucunun mevcut olmadığı bir hata alırsanız, arama hizmetiniz için bir özel uç nokta başarıyla yapılandırdınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel uç nokta, arama hizmeti ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:
1. Portalın üst kısmındaki **arama** kutusuna *myresourcegroup* girin ve arama sonuçlarından *myresourcegroup* öğesini seçin. 
1. **Kaynak grubunu sil**'i seçin. 
1. **Kaynak grubu adını yazmak** Için *myresourcegroup* girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir sanal ağ üzerinde bir VM ve özel bir uç nokta içeren bir arama hizmeti oluşturdunuz. VM 'ye internet 'ten bağlanır ve özel bağlantı kullanarak arama hizmetine güvenli bir şekilde iletilecaksınız. Özel uç nokta hakkında daha fazla bilgi edinmek için bkz. [Azure özel uç noktası nedir?](../private-link/private-endpoint-overview.md).
