---
title: Güvenli bir bağlantı için Özel Bitiş Noktası Oluşturma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama hizmetine güvenli bağlantı sağlamak için sanal ağda özel bir bitiş noktası ayarlama
manager: nitinme
author: mrcarter8
ms.author: mcarter
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 2664b1abd4131cf1dca186c7b044e338bf1efa84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945816"
---
# <a name="create-a-private-endpoint-for-a-secure-connection-to-azure-cognitive-search-preview"></a>Azure Bilişsel Arama'ya güvenli bağlantı için Özel Bitiş Noktası Oluşturma (Önizleme)

Bu makalede, genel bir IP adresi üzerinden erişilmeyen yeni bir Azure Bilişsel Arama hizmeti örneği oluşturmak için portalı kullanın. Ardından, aynı sanal ağda bir Azure sanal makineyi yapılandırın ve arama hizmetine özel bir bitiş noktası üzerinden erişmek için kullanın.

> [!Important]
> Azure Bilişsel Arama için Özel Bitiş Noktası desteği, [istek üzerine](https://aka.ms/SearchPrivateLinkRequestAccess) sınırlı erişimönizlemesi olarak kullanılabilir. Önizleme özellikleri hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. 
>
> Önizlemeye erişim izni verildikten sonra, Azure portalını veya [Management REST API sürümünü 2019-10-06-Preview'u](https://docs.microsoft.com/rest/api/searchmanagement/)kullanarak hizmetiniz için Özel Bitiş Noktalarını yapılandırabilirsiniz.
>   

## <a name="why-use-private-endpoint-for-secure-access"></a>Güvenli erişim için neden Private Endpoint kullanıyorsun?

Azure Bilişsel Arama için [Özel Bitiş Noktaları,](../private-link/private-endpoint-overview.md) sanal ağdaki bir istemcinin [Özel Bağlantı](../private-link/private-link-overview.md)üzerinden arama dizinindeki verilere güvenli bir şekilde erişmesine olanak tanır. Özel bitiş noktası, arama hizmetiniz için [sanal ağ adres alanından](../virtual-network/virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) bir IP adresi kullanır. İstemci ve arama hizmeti arasındaki ağ trafiği sanal ağ üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden geçerek genel internetten maruz kalmayı ortadan kaldırır. Private Link'i destekleyen diğer PaaS hizmetlerinin listesi için ürün belgelerindeki [kullanılabilirlik bölümünü](../private-link/private-link-overview.md#availability) kontrol edin.

Arama hizmetiniz için özel uç noktalar şunları yapmanızı sağlar:

- Arama hizmetiniz için ortak bitiş noktasındaki tüm bağlantıları engelleyin.
- Sanal ağdan veri sızmasını engellemenizi sağlayarak sanal ağ için güvenliği artırın.
- Özel bakışlı [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoutes](../expressroute/expressroute-locations.md) kullanarak sanal ağa bağlanan şirket içi ağlardan arama hizmetinize güvenli bir şekilde bağlanın.

> [!NOTE]
> Şu anda önizlemede dikkat edilmesi gereken bazı sınırlamalar vardır:
> * Yalnızca **Temel** katmandaki arama hizmetleri için kullanılabilir. 
> * Batı ABD 2, Batı Orta ABD, Doğu ABD, Güney Orta ABD, Avustralya Doğu ve Avustralya Güneydoğu bölgelerinde mevcuttur.
> * Hizmet bitiş noktası özel olduğunda, bazı portal özellikleri devre dışı bırakılır. Hizmet düzeyi bilgilerini görüntüleyebilir ve yönetebilirsiniz, ancak dizin verilerine ve dizini, dizin oluşturma ve beceri tanımları gibi hizmetteki çeşitli bileşenlere portal erişimi güvenlik nedenleriyle kısıtlanır.
> * Hizmet bitiş noktası özel olduğunda, belgeleri dizine yüklemek için [Search REST API'yi](https://docs.microsoft.com/rest/api/searchservice/) kullanmanız gerekir.
> * Azure portalında özel uç nokta desteği seçeneğini görmek için aşağıdaki bağlantıyı kullanmanız gerekir:https://portal.azure.com/?feature.enablePrivateEndpoints=true



## <a name="request-access"></a>Erişim izni isteme 

Bu önizleme özelliğine kaydolmak için [istek erişimine](https://aka.ms/SearchPrivateLinkRequestAccess) tıklayın. Form, siz, şirketiniz ve genel ağ topolojisi hakkında bilgi ister. Talebinizi inceledikten sonra, ek talimatlar içeren bir onay e-postası alırsınız.

## <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Bu bölümde, arama hizmetinizin özel bitiş noktasına erişmek için kullanılacak VM'yi barındıracak bir sanal ağ ve alt ağ oluşturacaksınız.

1. Azure portalı ana sekmesinden, **kaynak** > **Oluştur** > **Sanal ağ oluştur'u**seçin.

1. **Sanal ağ oluştur'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *MyVirtualNetwork'e* Girin |
    | Adres alanı | *10.1.0.0/16* girin |
    | Abonelik | Aboneliğinizi seçme|
    | Kaynak grubu | **Yeni Oluştur'u**seçin , *myResourceGroup'u*girin ve ardından **Tamam'ı** seçin |
    | Konum | **Batı ABD'yi** veya kullandığınız bölgeyi seçin|
    | Subnet - Adı | *mySubnet* girin |
    | Alt Ağ - Adres aralığı | *10.1.0.0/24* girin |
    |||

1. Geri sini varsayılan olarak bırakın ve **Oluştur'u**seçin.


## <a name="create-a-search-service-with-a-private-endpoint"></a>Özel bitiş noktası olan bir arama hizmeti oluşturma

Bu bölümde, Özel Bitiş Noktası olan yeni bir Azure Bilişsel Arama hizmeti oluşturursunuz. 

1. Azure portalında ekranın sol üst tarafında, **kaynak** > Oluştur**Web** > **Azure Bilişsel Arama'yı**seçin.

1. **Yeni Arama Hizmetinde - Temel Bilgiler**, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE DETAYLARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    | **ÖRNEK DETAYLAR** |  |
    | URL'si | Benzersiz bir ad girin. |
    | Konum | Bu önizleme özelliğine erişmek istediğinizde belirttiğiniz bölgeyi seçin. |
    | Fiyatlandırma katmanı | **Fiyatlandırma Katmanlarını Değiştir'i** seçin ve **Temel'i**seçin. Bu katman önizleme için gereklidir. |
    |||
  
1. **Sonraki'ni seçin: Ölçeklendirin.**

1. Değerleri varsayılan olarak bırakın ve **Sonraki: Ağ'** ı seçin.

1. **Yeni Arama Hizmetinde - Ağ,** **Endpoint bağlantısı(veri)** için **Özel'i** seçin.

1. **Yeni Arama Hizmetinde - Ağ**, select + Özel bitiş **noktası**altında **ekle** . 

1. **Özel Bitiş Noktası Oluştur'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.|
    | Konum | **Batı ABD'yi**seçin.|
    | Adı |  *MyPrivateEndpoint'i*girin.  |
    | Hedef alt kaynak | Varsayılan **searchService**bırakın. |
    | **Ağ** |  |
    | Sanal ağ  | MyVirtualNetwork'ünü kaynak grubundan *myResourceGroup'u*seçin. *MyVirtualNetwork* |
    | Alt ağ |  *mySubnet'i*seçin. |
    | **ÖZEL DNS ENTEGRASYON** |  |
    | Özel DNS bölgesiyle tümleştirme  | Varsayılan **Evet'i**bırakın. |
    | Özel DNS bölgesi  | Varsayılan ** (Yeni) privatelink.search.windows.net**. |
    |||

1. **Tamam'ı**seçin. 

1. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz. 

1. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin. 

1. Yeni hizmetinizin sağlanması tamamlandıktan sonra, yeni oluşturduğunuz kaynağa göz atın.

1. Sol daki içerik menüsünden **Tuşlar'ı** seçin.

1. Hizmete bağlanırken daha sonra birincil **yönetici anahtarını** kopyalayın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalında ekranın sol üst tarafında bir **kaynak** > **Oluştur** > **Sanal makine**oluştur'u seçin.

1. Sanal makine Oluştur 'da **- Temel bilgiler,** bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE DETAYLARI** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **MyResourceGroup'u**seçin. Bunu önceki bölümde oluşturdunuz.  |
    | **ÖRNEK DETAYLAR** |  |
    | Sanal makine adı | *myVm*girin. |
    | Bölge | **Batı ABD'yi** veya kullandığınız bölgeyi seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan **Bırak Altyapı artıklığı gerekli değil.** |
    | Görüntü | **Windows Server 2019 Datacenter'ı**seçin. |
    | Boyut | Varsayılan **Standart DS1 v2**bırakın. |
    | **YÖNETICI Hesabı** |  |
    | Kullanıcı adı | Seçtiğiniz bir kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTı NOKTASı Kuralları** |  |
    | Genel gelen bağlantı noktaları | Varsayılan **bırak Seçili bağlantı noktalarına izin ver.** |
    | Gelen bağlantı noktalarını seçme | Varsayılan **RDP(3389)** bırakın. |
    | **TASARRUF EDIN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan **No'yu**bırakın. |
    |||

1. **Sonraki'ni seçin: Diskler.**

1. **Sanal makine Oluştur 'da - Diskler,** varsayılanları bırakın ve **Sonraki: Ağ'** ı seçin.

1. **Sanal makine Oluştur 'da - Ağ oluşturma**, bu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan **MyVirtualNetwork**bırakın.  |
    | Adres alanı | Varsayılan **10.1.0.0/24**bırakın.|
    | Alt ağ | Varsayılan **mySubnet'i (10.1.0.0/24)** bırakın.|
    | Genel IP | Varsayılan **(yeni) myVm-ip**bırakın. |
    | Genel gelen bağlantı noktaları | **Seçili bağlantı noktalarına izin ver'i**seçin. |
    | Gelen bağlantı noktalarını seçme | **HTTP** ve **RDP'yi**seçin.|
    ||

1. **İncele ve oluştur**’u seçin. Azure'un yapılandırmanızı doğruladığı **Gözden Geçirme + oluşturma** sayfasına götürülürnüz.

1. **Validation geçirilen** iletiyi gördüğünüzde **Oluştur'u**seçin. 


## <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM *myVm'i* aşağıdaki gibi indirin ve bağlanın:

1. Portalın arama çubuğuna *myVm*girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP Dosyasını İndir'i**seçin. Azure uzak masaüstü protokolü (*.rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen.rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adı ve parolayı girin.

        > [!NOTE]
        > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullanın,** **daha fazla seçenek** > seçmeniz gerekebilir.

1. **Tamam'ı**seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Sertifika uyarısı alırsanız **Evet** veya **Devam et'i**seçin.

1. VM masaüstü göründükten sonra, yerel masaüstünüze geri dönmek için en aza indirin.  


## <a name="test-connections"></a>Test bağlantıları

Bu bölümde, arama hizmetine özel ağ erişimini doğrulayacak ve Özel Bitiş Noktası'nı kullanarak özel olarak bağlanabilirsiniz.

Arama hizmetiyle yapılan tüm etkileşimlerin Arama [REST API'sını](https://docs.microsoft.com/rest/api/searchservice/)gerektirdiğini girişten hatırlayın. Portal ve .NET SDK bu önizlemede desteklenmez.

1.  *myVM*Uzak Masaüstünde, PowerShell'i açın.

1. 'nslookup [arama hizmeti adı].search.windows.net' girin

    Buna benzer bir ileti alırsınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    [search service name].privatelink.search.windows.net
    Address:  10.0.0.5
    Aliases:  [search service name].search.windows.net
    ```

1. VM'den arama hizmetine bağlanın ve bir dizin oluşturun. REST API'sini kullanarak Postman'daki hizmetinizde yeni bir arama dizini oluşturmak için bu [hızlı başlatmayı](search-get-started-postman.md) takip edebilirsiniz. Postacı'dan gelen istekleri ayarlama için arama hizmeti bitiş noktası (https://[search service name].search.windows.net) ve önceki adımda kopyaladığınız yönetici api-key gerekir.

1. VM'den hızlı bir başlangıç tamamlamanız, hizmetin tam olarak çalışır durumda olduğunu onaylamanızdır.

1. Uzak masaüstü bağlantısını *myVM'e*kapatın. 

1. Hizmetinizin genel bir bitiş noktasında erişilemediğini doğrulamak için, yerel iş istasyonunuzda Postacı'yı açın ve hızlı başlatmada ilk birkaç görevi dene. Uzak sunucunun var olmadığı bir hata alırsanız, arama hizmetiniz için özel bir bitiş noktasını başarıyla yapılandırırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel Bitiş Noktası, arama hizmeti ve VM'yi kullanmayı bitirdiğinizde, kaynak grubunu ve içerdiği tüm kaynakları silin:
1. Portalın üst kısmındaki **Arama** kutusuna *myResourceGroup'u* girin ve arama sonuçlarından *myResourceGroup'u* seçin. 
1. **Kaynak grubunu sil**'i seçin. 
1. **KAYNAK GRUBU ADINI YAZIN** ve **Sil'i**seçmek için *MyResourceGroup'u* girin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, sanal ağda bir VM ve Özel Bitiş Noktası olan bir arama hizmeti oluşturdunuz. VM'ye internetten bağlandınız ve Private Link'i kullanarak arama hizmetine güvenli bir şekilde iletişim kurdunuz. Private Endpoint hakkında daha fazla bilgi edinmek için Azure [Özel Bitiş Noktası nedir?](../private-link/private-endpoint-overview.md)
