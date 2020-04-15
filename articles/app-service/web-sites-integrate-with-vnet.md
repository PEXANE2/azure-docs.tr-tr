---
title: Uygulamayı Azure Sanal Ağı ile tümleştir
description: Azure Uygulama Hizmeti'ndeki uygulamayı Azure sanal ağlarıyla tümleştirin.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 04/15/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: cb5747c6780da134dfb2f5ab088348b848c5f04a
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312844"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı azure sanal ağıyla tümleştirme

Bu makalede, Azure App Service VNet Tümleştirme özelliği ve [Azure Uygulama Hizmeti'ndeki](https://go.microsoft.com/fwlink/?LinkId=529714)uygulamalarla nasıl ayarlanır. [Azure Sanal Ağı][VNETOverview] (VNets) ile Azure kaynaklarınızın çoğunu Internet'e uygun olmayan bir ağa yerleştirebilirsiniz.

Azure Uygulama Hizmeti'nin iki çeşidiyi vardır:

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>VNet Tümleştirmesini Etkinleştir

1. App Service portalındaki **Ağ** AraBirimi'ne gidin. **VNet Tümleştirmesi**altında, **yapılandırmak için buraya tıklayın'ı**seçin.

1. **VNet Ekle'yi**seçin.

   ![VNet Tümleştirme'yi seçin][1]

1. Açılan liste, aynı bölgedeki aboneliğinizdeki Tüm Azure Kaynak Yöneticisi sanal ağlarını içerir. Bunun altında, diğer tüm bölgelerdeki Kaynak Yöneticisi sanal ağların listesi yer alır. Tümleştirmek istediğiniz VNet'i seçin.

   ![VNet'i seçin][2]

   * VNet aynı bölgedeyse, yeni bir alt ağ oluşturun veya önceden varolan boş bir alt ağ seçin.
   * Başka bir bölgede bir VNet seçmek için, siteye nokta etkinleştirilmiş bir VNet ağ geçidine sahip olmalısınız.
   * Klasik bir VNet ile entegre olmak için, **Sanal Ağ** açılır listesini seçmek yerine, **Klasik VNet'e bağlanmak için buraya tıklayın'ı**seçin. İstediğiniz klasik sanal ağı seçin. Hedef VNet'in zaten yer-to-site etkinleştirilmiş bir Sanal Ağ ağ geçidi olmalıdır.

    ![Klasik VNet'i Seçin][3]

Tümleştirme sırasında uygulamanız yeniden başlatılır. Tümleştirme tamamlandığında, entegre olduğunuz VNet'in ayrıntılarını görürsünüz.

Uygulamanız VNet'inizle entegre edildikten sonra, Azure DNS Özel Bölgeleri olmadığı sürece VNet'inizin yapılandırıldığında aynı DNS sunucusunu kullanır. Şu anda Azure DNS Özel Bölgeleri ile VNet Tümleştirme'yi kullanamazsınız.

## <a name="regional-vnet-integration"></a>Bölgesel VNet Entegrasyonu

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

### <a name="how-regional-vnet-integration-works"></a>Bölgesel VNet Tümleştirme nasıl çalışır?

Uygulama Hizmeti'ndeki uygulamalar çalışan rollerinde barındırılır. Temel ve daha yüksek fiyatlandırma planları, aynı işçiler üzerinde çalışan başka hiçbir müşterinin iş yüklerinin olmadığı barındırma planlarına adanmıştır. Bölgesel VNet Tümleştirmesi, temsilci alt netindeki adreslerle sanal arayüzler oluşturarak çalışır. Adres VNet'inizde olduğundan, VNet'inizdeki bir VM gibi VNet'iniz deki veya vNet'iniz aracılığıyla birçok şeye erişebilir. Ağ uygulaması, VNet'inizde VM çalıştırmaktan farklıdır. Bu nedenle bazı ağ özellikleri henüz bu özellik için kullanılamıyor.

![Bölgesel VNet Tümleştirme nasıl çalışır?][5]

Bölgesel VNet Tümleştirmesi etkinleştirildiğinde, uygulamanız normal kanallar aracılığıyla internete giden aramaları yapar. Uygulama özellikleri portalında listelenen giden adresler, uygulamanız tarafından hala kullanılan adreslerdir. Uygulamanız için hangi değişiklikler, hizmet uç noktası güvenli hizmetlere yapılan çağrılar veya RFC 1918 adresleri VNet'inize gider. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa, tüm giden trafik VNet'inize gönderilebilir.

Özellik, çalışan başına yalnızca bir sanal arabirimi destekler. Çalışan başına bir sanal arabirim, Uygulama Hizmeti planı başına bir bölgesel VNet Tümleştirmesi anlamına gelir. Aynı Uygulama Hizmeti planındaki tüm uygulamalar aynı VNet Tümleştirmesini kullanabilir. Ek bir VNet'e bağlanmak için bir uygulamaya ihtiyacınız varsa, başka bir Uygulama Hizmeti planı oluşturmanız gerekir. Kullanılan sanal arabirim, müşterilerin doğrudan erişebilen bir kaynak değildir.

Bu teknolojinin çalışma şekli doğası gereği, VNet Tümleştirmesi ile kullanılan trafik Azure Ağ İzleyicisi'nde veya NSG akış günlüklerinde görünmüyor.

## <a name="gateway-required-vnet-integration"></a>Ağ geçidi gerekli VNet Entegrasyonu

Ağ geçidi gerekli VNet Tümleştirme başka bir bölgedeki bir VNet'e veya klasik bir sanal ağa bağlanmayı destekler. Ağ geçidi gerekli VNet Entegrasyonu:

* Bir uygulamanın aynı anda yalnızca bir VNet'e bağlanmasını sağlar.
* Bir Uygulama Hizmeti planına en fazla beş VNet'in entegre edilmesini sağlar.
* Aynı VNet'in, bir Uygulama Hizmeti planı tarafından kullanılabilecek toplam sayıyı etkilemeden, bir Uygulama Hizmeti planında birden çok uygulama tarafından kullanılmasına izin verir. Aynı Uygulama Hizmeti planında aynı VNet'i kullanan altı uygulamanız varsa, bu kullanılan bir VNet olarak sayılır.
* Ağ geçidindeki SLA nedeniyle %99,9'luk bir SLA'yı destekler.
* Uygulamalarınızın VNet'in yapılandırıldığının DNS'sini kullanmasını sağlar.
* Bir uygulamaya bağlanabilmek için SSTP noktadan siteye VPN ile yapılandırılan Sanal Ağ rotası tabanlı bir ağ geçidi gerektirir.

Ağ geçidi gerekli VNet Tümleştirme kullanamazsınız:

* Linux uygulamaları ile.
* Azure ExpressRoute'a bağlı bir VNet ile.
* Hizmet uç noktası güvenli kaynaklara erişmek için.
* Hem ExpressRoute'u hem de siteden siteye veya siteden siteye VPN'leri destekleyen bir birlikte yaşam ağ geçidi yle.

### <a name="set-up-a-gateway-in-your-azure-virtual-network"></a>Azure sanal ağınızda bir ağ geçidi ayarlama ###

Ağ geçidi oluşturmak için:

1. VNet'inizde [bir ağ geçidi alt ağı oluşturun.][creategatewaysubnet]  

1. [VPN ağ geçidini oluşturun.][creategateway] Rota tabanlı BIR VPN türü seçin.

1. [Noktadan siteye adresleri ayarlayın.][setp2saddresses] Ağ geçidi temel SKU'da değilse, IKEV2'nin noktadan siteye yapılandırmada devre dışı bırakılması ve SSTP'nin seçilmesi gerekir. Noktadan siteye adres alanı RFC 1918 adres blokları 10.0.0.0/8, 172.16.0.0/12 ve 192.168.0.0/16 olmalıdır.

App Service VNet Tümleştirmesi ile kullanılmak üzere ağ geçidi oluşturursanız, sertifika yüklemeniz gerekmez. Ağ geçidi oluşturma 30 dakika sürebilir. Ağ geçidi sağlanana kadar uygulamanızı VNet'inizle entegre edemezsiniz.

### <a name="how-gateway-required-vnet-integration-works"></a>Ağ geçidi gerekli VNet Tümleştirme nasıl çalışır?

Ağ geçidi gereksinimli VNet Tümleştirme, noktadan noktaya VPN teknolojisinin üzerine inşa edilmiştir. Noktadan siteye VPN'ler, ağa erişimi uygulamayı barındıran sanal makineye sınırlandırıyor. Uygulamaların trafiği yalnızca Hibrit Bağlantılar veya VNet Tümleştirmesi aracılığıyla internete göndermesi kısıtlanır. Uygulamanız ağ geçidi gerekli VNet Tümleştirmesi kullanmak için portalile yapılandırıldığında, ağ geçidi ve uygulama tarafında sertifika oluşturmak ve atamak için sizin adınıza karmaşık bir anlaşma yönetilir. Sonuç olarak, uygulamalarınızı barındırmak için kullanılan çalışanlar, seçilen VNet'teki sanal ağ ağ geçidine doğrudan bağlanabiliyor.

![Ağ geçidi gerekli VNet Tümleştirme nasıl çalışır?][6]

### <a name="access-on-premises-resources"></a>Şirket içi kaynaklara erişim

Uygulamalar, siteden siteye bağlantıları olan VNet'lerle tümleştirerek şirket içi kaynaklara erişebilir. Ağ geçidi için gerekli VNet Tümleştirmesini kullanıyorsanız, şirket içi VPN ağ geçidi yollarınızı noktadan siteye adres bloklarınızla güncelleyin. Siteden siteye VPN ilk ayarlandığında, yapılandırmak için kullanılan komut dosyaları yolları düzgün şekilde ayarlamalıdır. Siteden siteye VPN'inizi oluşturduktan sonra noktadan siteye adresleri eklerseniz, yolları el ile güncelleştirmeniz gerekir. Bunun nasıl yapılacağının ayrıntıları ağ geçidine göre değişir ve burada açıklanmaz. BGP'yi siteden siteye VPN bağlantısıyla yapılandıramazsınız.

Bölgesel VNet Tümleştirme özelliğinin VNet'iniz üzerinden şirket içi kaynaklara ulaşması için ek yapılandırma gerekmez. ExpressRoute veya siteden siteye VPN kullanarak VNet'inizi şirket içi kaynaklara bağlamanız yeterlidir.

> [!NOTE]
> Ağ geçidi gerekli VNet Tümleştirme özelliği, expressroute ağ geçidi olan bir VNet ile bir uygulama entegre etmez. ExpressRoute ağ geçidi [birlikte yaşam modunda][VPNERCoex]yapılandırılsa bile, VNet Tümleştirmesi çalışmaz. Kaynaklara ExpressRoute bağlantısı üzerinden erişmeniz gerekiyorsa, VNet'inizde çalışan bölgesel VNet Tümleştirme özelliğini veya [Uygulama Hizmet Ortamını][ASE]kullanın.
> 
> 

### <a name="peering"></a>Eşleme

Bölgesel VNet Tümleştirmesi ile eşleme kullanıyorsanız, ek yapılandırma yapmanız gerekmez.

Ağ geçidi gereksinimi olan VNet Tümleştirmesini eşleme yle birlikte kullanıyorsanız, birkaç ek öğe yapılandırmanız gerekir. Uygulamanızla çalışacak şekilde eşlemi yapılandırmak için:

1. Uygulamanızın bağlandığı VNet'e bir bakış bağlantısı ekleyin. Eşleme bağlantısını eklediğinizde, **sanal ağ erişimine izin ver'i** etkinleştirin ve **iletilen trafiğe İzin Ver** ve ağ geçidi **geçişine izin ver'i**seçin.
1. Bağlı olduğunuz VNet'e bakan VNet'e bir bakış bağlantısı ekleyin. Hedef VNet'e eşleme bağlantısını eklediğinizde, **sanal ağ erişimine izin ver'i** etkinleştirin ve **iletilen trafiğe İzin Ver'i** ve uzak ağ **geçitlerine izin ver'i**seçin.
1. Portaldaki Ağ**Networking** > **VNet Tümleştirme** UI'sine uygulama **hizmeti planına** > gidin. Uygulamanızın bağlandığı VNet'i seçin. Yönlendirme bölümünün altında, uygulamanızın bağlı olduğu VNet ile bakan VNet'in adres aralığını ekleyin.

## <a name="manage-vnet-integration"></a>VNet Tümleştirmeyi Yönet

Bir VNet ile bağlantı kurmak ve bağlantıyı kesmek bir uygulama düzeyindedir. Birden çok uygulama da VNet Tümleştirmesini etkileyebilecek işlemler Uygulama Hizmeti planı düzeyindedir. **Networking** > **VNet Integration** portalı> uygulamadan VNet'inizle ilgili ayrıntıları alabilirsiniz. Benzer bilgileri App Service planı düzeyinde, **App Service planı** > **Networking** > **VNet Integration** portalında görebilirsiniz.

VNet Tümleştirme örneğinizin uygulama görünümünde alabileceğiniz tek işlem, uygulamanızın bağlantısını şu anda bağlı olduğu VNet ile bağlantısını kesmektir. Uygulamanızın bir VNet bağlantısını kesmek için **Bağlantıyı Kesme'yi**seçin. Bir VNet ile bağlantınızı kestiğinizde uygulamanız yeniden başlatılır. Bağlantının kesilmesi VNet'inizi değiştirmez. Alt ağ veya ağ geçidi kaldırılmadı. Daha sonra VNet'inizi silmek istiyorsanız, önce uygulamanızın bağlantısını VNet'ten ve ağ geçitleri gibi kaynakları silin.

Uygulama Hizmeti planı VNet Entegrasyon UI, Uygulama Hizmeti planınızdaki uygulamalar tarafından kullanılan tüm VNet entegrasyonlarını gösterir. Her VNet'in ayrıntılarını görmek için ilgilendiğiniz VNet'i seçin. Ağ geçidi gerekli VNet Tümleştirmesi için burada gerçekleştirebileceğiniz iki eylem vardır:

* **Eşitleme ağı**: Eşitleme ağı işlemi yalnızca ağ geçidine bağımlı VNet Tümleştirme özelliği için kullanılır. Eşitleme ağı işlemi, sertifikalarınızın ve ağ bilgilerinizin eşit olmasını sağlar. VNet'inizin DNS'sini ekler veya değiştirirseniz, eşitleme ağı işlemi gerçekleştirin. Bu işlem, bu VNet'i kullanan tüm uygulamaları yeniden başlatır.
* **Rota ekleme**: Rota ekleme, VNet'inize giden trafiği yönlendirir.

### <a name="gateway-required-vnet-integration-routing"></a>Ağ geçidi gerekli VNet Tümleştirme yönlendirme
VNet'inizde tanımlanan rotalar, uygulamanızdan VNet'inize trafiği yönlendirmek için kullanılır. VNet'e ek giden trafik göndermek için bu adres bloklarını buraya ekleyin. Bu özellik yalnızca ağ geçidi gerekli VNet Tümleştirmesi ile çalışır. Geçiş geçidi gerekli VNet Tümleştirmesini bölgesel VNet Tümleştirmesi'nde olduğu gibi kullandığınızda rota tabloları uygulama trafiğinizi etkilemez.

### <a name="gateway-required-vnet-integration-certificates"></a>Ağ geçidi gerekli VNet Tümleştirme sertifikaları
Ağ geçidi gerekli VNet Tümleştirme seve etkinleştirildiğinde, bağlantının güvenliğini sağlamak için gerekli bir sertifika değişimi vardır. Sertifikalarla birlikte DNS yapılandırması, rotalar ve ağı tanımlayan diğer benzer şeyler de vardır.

Sertifikalar veya ağ bilgileri değiştirilirse, **Eşitleme Ağı'nı**seçin. **Eşitleme Ağı'nı**seçtiğinizde, uygulamanızla VNet'iniz arasındaki bağlantıda kısa bir kesintiye neden olursunuz. Uygulamanız yeniden başlatılmamasa da, bağlantı kaybı sitenizin düzgün çalışmamasına neden olabilir.

## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Bölgesel VNet Tümleştirme özelliği, App Service planı fiyatlandırma katmanı ücretleridışında kullanım için ek bir ücret eve sahiptir.

Ağ geçidi gerekli VNet Tümleştirme özelliğinin kullanımı ile ilgili üç ücret:

* **Uygulama Hizmeti planı fiyatlandırma katmanı ücretleri**: Uygulamalarınızın standart, premium veya PremiumV2 Uygulama Hizmeti planında olması gerekir. Bu maliyetler hakkında daha fazla bilgi [için, Uygulama Hizmeti fiyatlandırması][ASPricing]için bkz.
* **Veri aktarım maliyetleri**: VNet aynı veri merkezinde olsa bile veri çıkış ücreti vardır. Bu ücretler [Veri Aktarımı fiyatlandırma ayrıntılarında][DataPricing]açıklanmıştır.
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
