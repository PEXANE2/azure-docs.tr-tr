---
title: Uygulamaları Azure Sanal Ağı ile tümleştirme
description: Azure Uygulama Hizmeti'ndeki uygulamaları Azure sanal ağlarıyla tümleştirin.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 02/27/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: a1a9739c444db2e41d55b8876011c066f2e71ca3
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421385"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı azure sanal ağıyla tümleştirme

Bu makalede, Azure App Service VNet Tümleştirme özelliği ve [Azure Uygulama Hizmeti'ndeki](https://go.microsoft.com/fwlink/?LinkId=529714)uygulamalarla nasıl ayarlanır. [Azure Sanal Ağı][VNETOverview]ile Azure kaynaklarınızın çoğunu Internet'e uygun olmayan bir ağa yerleştirebilirsiniz.

Azure Uygulama Hizmeti'nin iki çeşidiyi vardır:

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>VNet Tümleştirmesini Etkinleştir

1. App Service portalındaki **Ağ** AraBirimi'ne gidin. **VNet Tümleştirmesi**altında, **yapılandırmak için buraya tıklayın'ı**seçin.

1. **VNet Ekle'yi**seçin.

   ![VNet Tümleştirme'yi seçin][1]

1. Açılan liste, aynı bölgedeki aboneliğinizdeki Tüm Azure Kaynak Yöneticisi sanal ağlarını içerir. Bunun altında, diğer tüm bölgelerdeki Kaynak Yöneticisi sanal ağların listesi yer alır. Tümleştirmek istediğiniz sanal ağı seçin.

   ![Sanal ağı seçin][2]

   * Sanal ağ aynı bölgedeyse, yeni bir alt ağ oluşturun veya önceden varolan boş bir alt ağ seçin.
   * Başka bir bölgedeki sanal ağ seçmek için, siteye nokta etkinleştirilmiş bir Sanal Ağ ağ geçidine sahip olmalısınız.
   * Klasik bir sanal ağla bütünleşmek için, **Sanal Ağ** açılır listesini seçmek yerine, **Klasik VNet'e bağlanmak için buraya tıklayın'ı**seçin. İstediğiniz klasik sanal ağı seçin. Hedef sanal ağ zaten bir Sanal Ağ ağ geçidi işaret-to-site etkin olarak sağlanmış olmalıdır.

    ![Klasik VNet'i Seçin][3]

Tümleştirme sırasında uygulamanız yeniden başlatılır. Tümleştirme tamamlandığında, entegre olduğunuz sanal ağdaki ayrıntıları görürsünüz.

Uygulamanız sanal ağınızla entegre edildikten sonra, Azure DNS Özel Bölgeleri olmadığı sürece sanal ağınızın yapılandırıldığında aynı DNS sunucusunu kullanır. Şu anda Azure DNS Özel Bölgeleri ile VNet Tümleştirme'yi kullanamazsınız.

## <a name="regional-vnet-integration"></a>Bölgesel VNet Entegrasyonu

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

### <a name="how-regional-vnet-integration-works"></a>Bölgesel VNet Tümleştirme nasıl çalışır?

Uygulama Hizmeti'ndeki uygulamalar çalışan rollerinde barındırılır. Temel ve daha yüksek fiyatlandırma planları, aynı işçiler üzerinde çalışan başka hiçbir müşterinin iş yüklerinin olmadığı barındırma planlarına adanmıştır. Bölgesel VNet Tümleştirmesi, temsilci alt netindeki adreslerle sanal arayüzler oluşturarak çalışır. Adres sanal aağınızda olduğundan, sanal ağınızdaki bir VM gibi sanal ağınızdaki veya aracılığıyla birçok şeye erişebilir. Ağ uygulaması, sanal abunuzlu bir VM çalıştırmaktan farklıdır. Bu nedenle bazı ağ özellikleri henüz bu özellik için kullanılamıyor.

![Bölgesel VNet Tümleştirme nasıl çalışır?][5]

Bölgesel VNet Tümleştirmesi etkinleştirildiğinde, uygulamanız normal kanallar aracılığıyla internete giden aramaları yapar. Uygulama özellikleri portalında listelenen giden adresler, uygulamanız tarafından hala kullanılan adreslerdir. Uygulamanız için ne gibi değişiklikler, hizmet uç noktası güvenli hizmetlere yapılan çağrılar veya RFC 1918 adresleri sanal ağınıza gider. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa, tüm giden trafik sanal ağınıza gönderilebilir.

Özellik, çalışan başına yalnızca bir sanal arabirimi destekler. Çalışan başına bir sanal arabirim, Uygulama Hizmeti planı başına bir bölgesel VNet Tümleştirmesi anlamına gelir. Aynı Uygulama Hizmeti planındaki tüm uygulamalar aynı VNet Tümleştirmesini kullanabilir. Ek bir sanal ağa bağlanmak için bir uygulamaya ihtiyacınız varsa, başka bir Uygulama Hizmeti planı oluşturmanız gerekir. Kullanılan sanal arabirim, müşterilerin doğrudan erişebilen bir kaynak değildir.

Bu teknolojinin çalışma şekli doğası gereği, VNet Tümleştirmesi ile kullanılan trafik Azure Ağ İzleyicisi'nde veya NSG akış günlüklerinde görünmüyor.

## <a name="gateway-required-vnet-integration"></a>Ağ geçidi gerekli VNet Entegrasyonu

Ağ geçidi gerekli VNet Tümleştirmebaşka bir bölgedeki bir sanal ağa veya klasik bir sanal ağa bağlanmayı destekler. Ağ geçidi gerekli VNet Entegrasyonu:

* Bir uygulamanın aynı anda yalnızca bir sanal ağa bağlanmasını sağlar.
* Bir Uygulama Hizmeti planına en fazla beş sanal anın entegre edilmesini sağlar.
* Aynı sanal ağın, bir Uygulama Hizmeti planı tarafından kullanılabilecek toplam sayıyı etkilemeden, bir Uygulama Hizmeti planında birden çok uygulama tarafından kullanılmasına izin verir. Aynı Uygulama Hizmeti planında aynı sanal ağı kullanan altı uygulamanız varsa, bu bir sanal ağ kullanılıyorsa sayılır.
* Ağ geçidindeki SLA nedeniyle %99,9'luk bir SLA'yı destekler.
* Uygulamalarınızın sanal ağın yapılandırıldığının DNS'sini kullanmasını sağlar.
* Bir uygulamaya bağlanabilmek için SSTP noktadan siteye VPN ile yapılandırılan Sanal Ağ rotası tabanlı bir ağ geçidi gerektirir.

Ağ geçidi gerekli VNet Tümleştirme kullanamazsınız:

* Linux uygulamaları ile.
* Azure ExpressRoute'a bağlı sanal bir ağ ile.
* Hizmet uç noktası güvenli kaynaklara erişmek için.
* Hem ExpressRoute'u hem de siteden siteye veya siteden siteye VPN'leri destekleyen bir birlikte yaşam ağ geçidi yle.

### <a name="set-up-a-gateway-in-your-virtual-network"></a>Sanal ağınızda bir ağ geçidi ayarlama ###

Ağ geçidi oluşturmak için:

1. Sanal ağınızda [bir ağ geçidi alt ağı oluşturun.][creategatewaysubnet]  

1. [VPN ağ geçidini oluşturun.][creategateway] Rota tabanlı BIR VPN türü seçin.

1. [Noktadan siteye adresleri ayarlayın.][setp2saddresses] Ağ geçidi temel SKU'da değilse, IKEV2'nin noktadan siteye yapılandırmada devre dışı bırakılması ve SSTP'nin seçilmesi gerekir. Noktadan siteye adres alanı RFC 1918 adres blokları 10.0.0.0/8, 172.16.0.0/12 ve 192.168.0.0/16 olmalıdır.

App Service VNet Tümleştirmesi ile kullanılmak üzere ağ geçidi oluşturursanız, sertifika yüklemeniz gerekmez. Ağ geçidi oluşturma 30 dakika sürebilir. Ağ geçidi sağlanana kadar uygulamanızı sanal ağınızla bütünleştiremezsiniz.

### <a name="how-gateway-required-vnet-integration-works"></a>Ağ geçidi gerekli VNet Tümleştirme nasıl çalışır?

Ağ geçidi gereksinimli VNet Tümleştirme, noktadan noktaya VPN teknolojisinin üzerine inşa edilmiştir. Noktadan siteye VPN'ler, ağa erişimi uygulamayı barındıran sanal makineye sınırlandırıyor. Uygulamaların trafiği yalnızca Hibrit Bağlantılar veya VNet Tümleştirmesi aracılığıyla internete göndermesi kısıtlanır. Uygulamanız ağ geçidi gerekli VNet Tümleştirmesi kullanmak için portalile yapılandırıldığında, ağ geçidi ve uygulama tarafında sertifika oluşturmak ve atamak için sizin adınıza karmaşık bir anlaşma yönetilir. Sonuç olarak, uygulamalarınızı barındırmak için kullanılan çalışanlar, seçilen sanal ağdaki sanal ağ ağ geçidine doğrudan bağlanabiliyor.

![Ağ geçidi gerekli VNet Tümleştirme nasıl çalışır?][6]

### <a name="access-on-premises-resources"></a>Şirket içi kaynaklara erişim

Uygulamalar, siteden siteye bağlantıları olan sanal ağlarla tümleştirerek şirket içi kaynaklara erişebilir. Ağ geçidi için gerekli VNet Tümleştirmesini kullanıyorsanız, şirket içi VPN ağ geçidi yollarınızı noktadan siteye adres bloklarınızla güncelleyin. Siteden siteye VPN ilk ayarlandığında, yapılandırmak için kullanılan komut dosyaları yolları düzgün şekilde ayarlamalıdır. Siteden siteye VPN'inizi oluşturduktan sonra noktadan siteye adresleri eklerseniz, yolları el ile güncelleştirmeniz gerekir. Bunun nasıl yapılacağının ayrıntıları ağ geçidine göre değişir ve burada açıklanmaz. BGP'yi siteden siteye VPN bağlantısıyla yapılandıramazsınız.

Bölgesel VNet Tümleştirme özelliğinin sanal ağınızdan şirket içi kaynaklara ulaşması için ek yapılandırma gerekmez. ExpressRoute veya siteden siteye VPN kullanarak sanal ağınızı şirket içi kaynaklara bağlamanız yeterlidir.

> [!NOTE]
> Ağ geçidi gerekli VNet Tümleştirme özelliği, bir uygulamayı ExpressRoute ağ geçidi olan sanal bir ağa entegre etmez. ExpressRoute ağ geçidi [birlikte yaşam modunda][VPNERCoex]yapılandırılsa bile, VNet Tümleştirmesi çalışmaz. Kaynaklara ExpressRoute bağlantısı üzerinden erişmeniz gerekiyorsa, sanal ağınızda çalışan bölgesel VNet Tümleştirme özelliğini veya [Uygulama Hizmet Ortamını][ASE]kullanın.
> 
> 

### <a name="peering"></a>Eşleme

Bölgesel VNet Tümleştirmesi ile eşleme kullanıyorsanız, ek yapılandırma yapmanız gerekmez.

Ağ geçidi gereksinimi olan VNet Tümleştirmesini eşleme yle birlikte kullanıyorsanız, birkaç ek öğe yapılandırmanız gerekir. Uygulamanızla çalışacak şekilde eşlemi yapılandırmak için:

1. Uygulamanızın bağlandığı sanal ağa bir bakış bağlantısı ekleyin. Eşleme bağlantısını eklediğinizde, **sanal ağ erişimine izin ver'i** etkinleştirin ve **iletilen trafiğe İzin Ver** ve ağ geçidi **geçişine izin ver'i**seçin.
1. Bağlı olduğunuz sanal ağa bakan sanal ağa bir bakış bağlantısı ekleyin. Hedef sanal ağa eşleme bağlantısı eklediğinizde, **sanal ağ erişimine izin ver'i** etkinleştirin ve **iletilen trafiğe İzin Ver'i** ve uzak ağ **geçitlerine izin ver'i**seçin.
1. Portaldaki Ağ**Networking** > **VNet Tümleştirme** UI'sine uygulama **hizmeti planına** > gidin. Uygulamanızın bağlanan sanal ağı seçin. Yönlendirme bölümüne, uygulamanızın bağlı olduğu sanal ağla bakan sanal ağın adres aralığını ekleyin.

## <a name="manage-vnet-integration"></a>VNet Tümleştirmeyi Yönet

Sanal ağla bağlantı kurmak ve bağlantıyı kesmek bir uygulama düzeyindedir. Birden çok uygulama da VNet Tümleştirmesini etkileyebilecek işlemler Uygulama Hizmeti planı düzeyindedir. **Networking** > **VNet Integration** portalı> uygulamadan sanal ağınızdaki ayrıntıları alabilirsiniz. Benzer bilgileri App Service planı düzeyinde, **App Service planı** > **Networking** > **VNet Integration** portalında görebilirsiniz.

VNet Tümleştirme örneğinizin uygulama görünümünde alabileceğiniz tek işlem, uygulamanızın bağlantısını şu anda bağlı olduğu sanal ağdan ayırmaktır. Uygulamanızın sanal ağdan bağlantısını kesmek için **Bağlantıyı Kesme'yi**seçin. Sanal ağla bağlantınızı kestiğinizde uygulamanız yeniden başlatılır. Bağlantının kesilmesi sanal ağınızı değiştirmez. Alt ağ veya ağ geçidi kaldırılmadı. Daha sonra sanal ağınızı silmek istiyorsanız, önce uygulamanızın bağlantısını sanal ağdan ve ağ geçitleri gibi kaynakları silin.

Uygulama Hizmeti planı VNet Entegrasyon UI, Uygulama Hizmeti planınızdaki uygulamalar tarafından kullanılan tüm sanal ağ entegrasyonlarını gösterir. Her sanal ağdaki ayrıntıları görmek için ilgilendiğiniz sanal ağı seçin. Ağ geçidi gerekli VNet Tümleştirmesi için burada gerçekleştirebileceğiniz iki eylem vardır:

* **Eşitleme ağı**: Eşitleme ağı işlemi yalnızca ağ geçidine bağımlı VNet Tümleştirme özelliği için kullanılır. Eşitleme ağı işlemi, sertifikalarınızın ve ağ bilgilerinizin eşit olmasını sağlar. Sanal ağınızın DNS'sini ekler veya değiştirirseniz, eşitleme ağı işlemi gerçekleştirin. Bu işlem, bu sanal ağı kullanan tüm uygulamaları yeniden başlatır.
* **Rota ekleme**: Rota eklemek, giden trafiği sanal ağınıza yönlendirir.

### <a name="gateway-required-vnet-integration-routing"></a>Ağ geçidi gerekli VNet Tümleştirme yönlendirme
Sanal ağınızda tanımlanan rotalar, uygulamanızdan trafiği sanal ağınıza yönlendirmek için kullanılır. Sanal ağa ek giden trafik göndermek için, bu adres bloklarını buraya ekleyin. Bu özellik yalnızca ağ geçidi gerekli VNet Tümleştirmesi ile çalışır. Geçiş geçidi gerekli VNet Tümleştirmesini bölgesel VNet Tümleştirmesi'nde olduğu gibi kullandığınızda rota tabloları uygulama trafiğinizi etkilemez.

### <a name="gateway-required-vnet-integration-certificates"></a>Ağ geçidi gerekli VNet Tümleştirme sertifikaları
Ağ geçidi gerekli VNet Tümleştirme seve etkinleştirildiğinde, bağlantının güvenliğini sağlamak için gerekli bir sertifika değişimi vardır. Sertifikalarla birlikte DNS yapılandırması, rotalar ve ağı tanımlayan diğer benzer şeyler de vardır.

Sertifikalar veya ağ bilgileri değiştirilirse, **Eşitleme Ağı'nı**seçin. **Eşitleme Ağı'nı**seçtiğinizde, uygulamanızla sanal ağınız arasındaki bağlantıda kısa bir kesintiye neden olursunuz. Uygulamanız yeniden başlatılmamasa da, bağlantı kaybı sitenizin düzgün çalışmamasına neden olabilir.

## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Bölgesel VNet Tümleştirme özelliği, App Service planı fiyatlandırma katmanı ücretleridışında kullanım için ek bir ücret eve sahiptir.

Ağ geçidi gerekli VNet Tümleştirme özelliğinin kullanımı ile ilgili üç ücret:

* **Uygulama Hizmeti planı fiyatlandırma katmanı ücretleri**: Uygulamalarınızın standart, premium veya PremiumV2 Uygulama Hizmeti planında olması gerekir. Bu maliyetler hakkında daha fazla bilgi [için, Uygulama Hizmeti fiyatlandırması][ASPricing]için bkz.
* **Veri aktarım maliyetleri**: Sanal ağ aynı veri merkezinde olsa bile veri çıkış ücreti vardır. Bu ücretler [Veri Aktarımı fiyatlandırma ayrıntılarında][DataPricing]açıklanmıştır.
* **VPN ağ geçidi maliyetleri**: Sanal ağ ağ geçidinin, noktaya göre VPN için gerekli olan bir maliyeti vardır. Daha fazla bilgi için [VPN ağ geçidi fiyatlandırması][VNETPricing]için bkz.

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>Otomasyon

Bölgesel VNet Entegrasyonu için CLI desteği mevcuttur. Aşağıdaki komutlara erişmek [için Azure CLI'yi yükleyin.][installCLI]

        az webapp vnet-integration --help

        Group
            az webapp vnet-integration : Methods that list, add, and remove virtual network integrations
            from a webapp.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            add    : Add a regional virtual network integration to a webapp.
            list   : List the virtual network integrations on a webapp.
            remove : Remove a regional virtual network integration from webapp.

        az appservice vnet-integration --help

        Group
            az appservice vnet-integration : A method that lists the virtual network integrations used in an
            appservice plan.
                This command group is in preview. It may be changed/removed in a future release.
        Commands:
            list : List the virtual network integrations used in an appservice plan.

Ağ geçidi için gerekli VNet Tümleştirmesi için PowerShell'i kullanarak App Service'i bir Azure sanal ağıyla tümleştirebilirsiniz. Çalışmaya hazır bir komut dosyası için Azure [Uygulama Hizmeti'ndeki bir uygulamayı Azure sanal ağına bağlayın'a](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3)bakın.


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-classic.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-regionalworks.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-gwworks.png


<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
[VNETRouteTables]: https://docs.microsoft.com/azure/virtual-network/manage-route-table/
[installCLI]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest/
