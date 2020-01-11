---
title: Güvenli bağlantılar için özel uç nokta oluşturma
titleSuffix: Azure Cognitive Search
description: Şu anda önizleme aşamasında olan erişimi, Özel uç nokta ve güvenli VNet bağlantısı kullanarak bir arama hizmeti uç noktasına kısıtlayabilirsiniz.
manager: nitinme
author: mrcarter8
ms.author: mcarter
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 0ed6319a33fc3db0b3144d91f4be10b7ab6121d7
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75865018"
---
# <a name="restrict-access-to-azure-cognitive-search-using-private-endpoint-and-a-virtual-network-connection"></a>Özel uç nokta ve bir sanal ağ bağlantısı kullanarak Azure Bilişsel Arama erişimini kısıtlayın.

> [!IMPORTANT] 
> Özel uç nokta desteği şu anda sınırlı erişimli önizleme olarak kullanılabilir. Bu Önizleme yalnızca **Temel katmanda**arama hizmetleri için kullanılabilir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-10-01-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

Bu makalede, genel IP adreslerinden erişim olmadan güvenli bağlantılarla erişilebilen yeni bir arama hizmeti oluşturmayı öğrenin. İstemci bağlantılarına, hizmet ile aynı sanal ağda dağıtılan Azure sanal makinelerinden izin verilir.

## <a name="about-private-endpoint-support"></a>Özel uç nokta desteği hakkında

Azure Bilişsel Arama [Özel uç noktaları](../private-link/private-endpoint-overview.md) , bir sanal ağ üzerindeki istemcinin [özel bir bağlantı](../private-link/private-link-overview.md)üzerinden verilere güvenli bir şekilde erişmesini sağlar. Özel uç nokta, arama hizmetiniz için [sanal ağ adres alanından](../virtual-network/virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) bir IP adresi kullanır. İstemci ile arama hizmeti arasındaki ağ trafiği, sanal ağın üzerine geçer ve Microsoft omurga ağı üzerinde bir özel bağlantı aracılığıyla genel İnternet üzerinden etkilenme olasılığını ortadan kaldırır. Özel bağlantıyı destekleyen PaaS hizmetlerinin bir listesi için ürün belgelerindeki [kullanılabilirlik bölümüne](../private-link/private-link-overview.md#availability) bakın.

Arama hizmetiniz için özel bir uç nokta şunları yapmanızı sağlar:

+ Arama hizmetiniz için genel uç noktasındaki tüm bağlantıları engelleyin.
+ Sanal ağ üzerindeki verilerin ayıklanma engellenmesini sağlayarak Sanal ağ güvenliğini artırın.
+ Sanal ağa [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](../expressroute/expressroute-locations.md) kullanarak özel eşleme ile bağlanan şirket içi ağlardan arama hizmetinize güvenli bir şekilde bağlanın.

> [!NOTE]
> Hizmet uç noktası özel olduğunda, bazı Portal özellikleri devre dışı bırakılır. Hizmet düzeyi bilgilerini görüntüleyebilir ve yönetebileceksiniz, ancak dizin verilerine ve hizmette Dizin, Dizin Oluşturucu ve beceri tanımları gibi çeşitli bileşenlere erişim sağlamak için Portal erişimi güvenlik nedenleriyle kısıtlıdır.

## <a name="request-access"></a>Erişim izni isteme 

Bu önizleme özelliği için kaydolmak üzere [erişim iste](https://aka.ms/SearchPrivateLinkRequestAccess) ' ye tıklayın. Form, siz, şirketiniz ve genel ağ topolojiniz hakkında bilgi ister. İsteğinizi gözden geçirdikten sonra ek yönergeler içeren bir onay e-postası alacaksınız.

## <a name="create-a-vm"></a>VM oluşturma
Bu bölümde, arama hizmetinizin özel uç noktasına erişmek için kullanılacak VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız.

### <a name="set-up-the-virtual-network"></a>Sanal ağı ayarlama
1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Sol üst tarafta, **sanal ağ** ** >  > ** **kaynak oluştur** ' u seçin.
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


### <a name="create-a-virtual-machine"></a>Sanal makine oluşturun

1. Ana portalın sol üst kısmında, **sanal makine** > **Işlem** > **kaynak oluştur** ' u seçin.

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
    | Genel gelen bağlantı noktası | Varsayılanı **yok**olarak bırakın. |
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


## <a name="create-your-search-service-with-a-private-endpoint"></a>Özel bir uç nokta ile arama hizmetinizi oluşturma

Bu bölümde, özel bir uç nokta ile yeni bir Azure Bilişsel Arama hizmeti oluşturacaksınız. 

1. Ana portalın sol üst kısmında, **Web** > **Azure bilişsel arama** > **kaynak oluştur** ' u seçin.

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
1. Hizmet oluşturulduktan sonra, yeni oluşturduğunuz kaynağa gidin.
1. Sol içerik menüsünde **anahtarlar** ' ı seçin.
1. Sonraki adımda kullanmak üzere **birincil yönetici anahtarını** kopyalayın.

 
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

1. Oturum açarken bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.  


## <a name="access-the-search-service-privately-from-the-vm"></a>Arama hizmetine özel olarak VM 'den erişin

Bu bölümde, arama hizmetine özel ağ erişimini doğrulayacaksınız ve özel uç nokta kullanarak depolama hesabına özel olarak bağlanacak şekilde bağlanıyorsunuz.

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
1. REST API kullanarak Postman 'da hizmetinize yeni bir arama dizini oluşturmak için VM 'deki bu [hızlı](search-get-started-postman.md) başlangıcı izleyin.

1. Yerel iş istasyonunuzda Postman ile aynı isteklerin birkaçını deneyin.

1. Sanal makineyi VM 'den tamamlayabiliyor, ancak yerel iş istasyonunuzda uzak sunucunun mevcut olmadığı bir hata alırsanız, arama hizmetiniz için bir özel uç noktayı başarıyla yapılandırdınız.

1.  *Myvm*ile uzak masaüstü bağlantısını kapatın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Özel uç nokta, arama hizmeti hesabı ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin: 

1. Portalın üst kısmındaki **arama** kutusuna *myresourcegroup* girin ve arama sonuçlarından *myresourcegroup* öğesini seçin. 
1. **Kaynak grubunu sil**'i seçin. 
1. **Kaynak grubu adını yazmak** Için *myresourcegroup* girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağ üzerinde bir VM ve özel bir uç nokta içeren bir arama hizmeti oluşturdunuz. VM 'ye internet 'ten bağlanır ve özel bağlantı kullanarak arama hizmetine güvenli bir şekilde iletilecaksınız. 

> [!div class="nextstepaction"]
> [Azure özel uç noktası nedir?](../private-link/private-endpoint-overview.md).
