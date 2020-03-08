---
title: Uygulamayı Azure sanal ağı ile tümleştirme
description: Uygulamaları Azure sanal ağlarla Azure App Service tümleştirme.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 02/27/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 89aa78e0d26598eacf436ca88cc6c5549f91d2fc
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673221"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı bir Azure sanal ağıyla tümleştirin
Bu belgede Azure App Service sanal ağ tümleştirme özelliği ve [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714)uygulamalarla nasıl ayarlanacağı açıklanmaktadır. [Azure sanal ağları][VNETOverview] (VNet), internet 'ten yönlendirilebilir olmayan bir ağa Azure kaynaklarınızın çoğunu yerleştirme izni verir.  

Azure App Service iki çeşitliliğe sahiptir.

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>VNet tümleştirmesini etkinleştir 

1. App Service portalında ağ kullanıcı arabirimine gidin. VNet tümleştirmesi altında *"yapılandırmak için buraya tıklayın"* seçeneğini belirleyin. 

1. **VNET Ekle**' yi seçin.  

   ![VNet tümleştirmesini seçin][1]

1. Açılan listede, aynı bölgedeki ve diğer tüm bölgelerdeki tüm Kaynak Yöneticisi sanal ağların bir listesi olan, aboneliğinizdeki tüm Kaynak Yöneticisi sanal ağları yer alacak. Tümleştirilecek sanal ağı seçin.

   ![VNet 'i seçin][2]

   * VNet aynı bölgedeyse, yeni bir alt ağ oluşturun veya önceden varolan boş bir alt ağ seçin. 

   * Başka bir bölgedeki VNet seçmek için, sitenin etkin olduğu noktadan bir sanal ağ geçidinizin gerekir.

   * Klasik VNET ile tümleştirme için, VNet açılan listesine tıklanmak yerine, **Klasik VNET 'e bağlanmak için buraya tıklayın ' ı**seçin. İstediğiniz klasik sanal ağı seçin. Hedef VNet 'in, site etkin noktadan sonra sağlanan bir sanal ağ geçidi zaten olmalıdır.

    ![Klasik VNet seçin][3]
    
Tümleştirme sırasında uygulamanız yeniden başlatılır.  Tümleştirme tamamlandığında, ile tümleştirildiği VNet 'teki ayrıntıları görürsünüz. 

Uygulamanız VNet 'iniz ile tümleşik olduktan sonra, Azure DNS Özel Bölgeleri olmadığı müddetçe sanal ağınızın yapılandırıldığı aynı DNS sunucusunu kullanır. Şu anda Azure DNS Özel Bölgeleri ile VNet tümleştirmesini kullanamazsınız.

## <a name="regional-vnet-integration"></a>Bölgesel VNet tümleştirmesi

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

### <a name="how-regional-vnet-integration-works"></a>Bölgesel VNet tümleştirmesinin çalışması

App Service uygulamalar çalışan rollerinde barındırılır. Temel ve daha yüksek fiyatlandırma planları, aynı çalışanlar üzerinde çalışan başka müşteri iş yükleri bulunmayan ayrılmış barındırma planlardır. Bölgesel VNet tümleştirmesi, yetkilendirilmiş alt ağdaki adreslerle sanal arabirimler bağlayarak işe yarar. Kimden adresi sanal ağınız içinde olduğundan, VNET 'iniz içindeki bir VM 'de olduğu gibi, sanal ağınız içindeki veya bu sanal nesnelerin çoğuna erişebilir. Ağ uygulamasının VNet 'iniz üzerinde bir VM çalıştırmasının dışında ve bu özellik kullanılırken bazı ağ özellikleri henüz kullanılamaz.

![Bölgesel VNet tümleştirmesinin çalışması][5]

Bölgesel VNet tümleştirmesi etkinleştirildiğinde, uygulamanız yine de normal olarak aynı kanallar üzerinden İnternet 'e giden çağrılar yapmaya devam edecektir. Uygulama özellikleri portalında listelenen giden adresler hala uygulamanız tarafından kullanılan adreslerdir. Uygulamanız için değişiklikler, hizmet uç noktası güvenliği sağlanmış hizmetler veya RFC 1918 adresleri için de sanal ağınıza gider. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanırsa, tüm giden trafik sanal ağınıza gönderilebilir. 

Özelliği, çalışan başına yalnızca bir sanal arabirimi destekler.  Çalışan başına bir sanal arabirim, App Service plan başına bir bölgesel VNet tümleştirmesi anlamına gelir. Aynı App Service planındaki tüm uygulamalar aynı VNet tümleştirmesini kullanabilir, ancak ek bir sanal ağa bağlanmak için bir uygulamaya ihtiyacınız varsa, başka bir App Service planı oluşturmanız gerekir. Kullanılan sanal arabirim, müşterilerin doğrudan erişimine sahip olduğu bir kaynak değildir.

Bu teknolojinin nasıl çalıştığı doğası nedeniyle, VNet tümleştirmeyle kullanılan trafik ağ Izleyicisi veya NSG akış günlüklerinde gösterilmez.  

## <a name="gateway-required-vnet-integration"></a>Ağ Geçidi gerekli VNet tümleştirmesi

Ağ Geçidi gerekli VNet tümleştirmesi, başka bir bölgedeki VNet 'e veya klasik VNet 'e bağlanmayı destekler. Ağ Geçidi gerekli VNet tümleştirmesi: 

* Bir uygulamanın tek seferde yalnızca 1 VNet 'e bağlanmasını sağlar
* App Service bir plan dahilinde tümleştirilecek en fazla beş VNET sağlar 
* Bir App Service planı tarafından kullanılabilecek toplam sayıyı etkilemeden bir App Service planında birden çok uygulama tarafından kullanılmasını sağlar.  Aynı App Service planında aynı VNet 'i kullanan altı uygulamanız varsa, bu, kullanılmakta olan 1 VNet olarak sayılır. 
* Ağ geçidinde SLA nedeniyle% 99,9 SLA 'sını destekler
* Uygulamalarınızın VNet 'in yapılandırıldığı DNS 'yi kullanmasına olanak sağlar
* , Uygulamaya bağlanmadan önce SSTP Noktadan siteye VPN ile yapılandırılmış bir sanal ağ yolu tabanlı ağ geçidi gerektirir. 

Ağ Geçidi gerekli VNet tümleştirmesini kullanamazsınız:

* Linux uygulamaları ile
* ExpressRoute ile bağlantılı VNet ile 
* Hizmet uç noktalarına güvenli kaynaklara erişmek için
* Hem ExpressRoute hem de site/siteden siteye VPN 'lere işaret eden bir birlikte bulunma ağ geçidiyle

### <a name="set-up-a-gateway-in-your-vnet"></a>Sanal ağınız için bir ağ geçidi ayarlama ###

Bir ağ geçidi oluşturmak için:

1. Sanal ağınız için [bir ağ geçidi alt ağı oluşturun][creategatewaysubnet] .  

1. [VPN ağ geçidini oluşturun][creategateway]. Rota tabanlı VPN türü seçin.

1. [Noktayı site adresleri olarak ayarlayın][setp2saddresses]. Ağ Geçidi temel SKU 'da değilse, ıKEV2 'nin site yapılandırması noktasında devre dışı bırakılması ve SSTP 'nin seçilmesi gerekir. Noktadan siteye adres alanı, RFC 1918 adres blokları, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 biçiminde olmalıdır

Yalnızca App Service VNet tümleştirmesiyle kullanmak üzere ağ geçidini oluşturuyorsanız, bir sertifikayı karşıya yüklemeniz gerekmez. Ağ geçidinin oluşturulması 30 dakika sürebilir. Ağ Geçidi sağlanıncaya kadar uygulamanızı VNet ile tümleştirimeyeceksiniz. 

### <a name="how-gateway-required-vnet-integration-works"></a>Ağ Geçidi gerekli VNet tümleştirmesinin çalışması

Ağ Geçidi, Noktadan siteye VPN teknolojisine yerleşik olarak oluşturulan VNet tümleştirmesini gerektirir. Noktadan siteye VPN 'Ler, yalnızca uygulamayı barındıran sanal makineye ağ erişimini sınırlandırır. Uygulamalar, Karma Bağlantılar veya VNet tümleştirmesi aracılığıyla yalnızca İnternet 'e trafik gönderecek şekilde kısıtlanır. Uygulamanız, ağ geçidi gerekli VNet tümleştirmesi kullanmak üzere portalla yapılandırıldığında, ağ geçidinde ve uygulama tarafında sertifika oluşturmak ve atamak için sizin adınıza karmaşık bir anlaşma yönetilir. Nihai sonuç, uygulamalarınızı barındırmak için kullanılan çalışanların seçili VNet 'teki sanal ağ geçidine doğrudan bağlanmasını sağlar. 

![Ağ Geçidi gerekli VNet tümleştirmesinin çalışması][6]

### <a name="accessing-on-premises-resources"></a>Şirket içi kaynaklara erişme

Uygulamalar, siteden siteye bağlantıları olan VNET 'ler ile tümleştirerek şirket içi kaynaklara erişebilir. Ağ Geçidi gerekli VNet tümleştirmesini kullanıyorsanız, şirket içi VPN ağ geçidi yollarınızı Noktadan siteye adres bloklarında güncelleştirmeniz gerekir. Siteden siteye VPN ilk kez ayarlandığında, yapılandırmak için kullanılan betikler yolları doğru şekilde ayarlayabilmelidir. Siteden siteye VPN 'nizi oluşturduktan sonra Noktadan siteye adresleri eklerseniz, rotaları el ile güncelleştirmeniz gerekir. Bu şekilde, ağ geçidi başına farklılık gösteren ve burada açıklanmayan Ayrıntılar açıklanmaktadır. Siteden siteye VPN bağlantısıyla BGP ile yapılandırılmış olamaz.

Bölgesel VNet tümleştirme özelliğinin sanal ağınıza ve şirket içi ağa ulaşması için ek bir yapılandırma gerekmez. Sanal ortamınızı ExpressRoute veya siteden siteye VPN kullanarak şirket içi olarak bağlamanız yeterlidir. 

> [!NOTE]
> Ağ Geçidi gerekli VNet tümleştirme özelliği, bir uygulamayı ExpressRoute ağ geçidi olan bir VNet ile tümleştirmez. ExpressRoute ağ geçidi, birlikte [bulunma modunda][VPNERCoex] yapılandırılmış olsa bile VNET tümleştirmesi çalışmaz. Kaynaklara bir ExpressRoute bağlantısı aracılığıyla erişmeniz gerekiyorsa, bölgesel VNet tümleştirme özelliğini veya sanal ağınız üzerinde çalışan bir [App Service ortamı][ASE]kullanabilirsiniz. 
> 
> 

### <a name="peering"></a>Eşleme

Bölgesel VNet tümleştirmesiyle eşleme kullanıyorsanız, ek yapılandırma yapmanız gerekmez. 

Ağ Geçidi gereken VNet tümleştirmesini eşleme ile kullanıyorsanız, birkaç ek öğe yapılandırmanız gerekir. Eşleme 'yi uygulamanızla çalışacak şekilde yapılandırmak için:

1. Uygulamanızın bağlandığı VNet 'e bir eşleme bağlantısı ekleyin. Eşleme bağlantısı eklenirken **sanal ağ erişimine Izin ver** ' i etkinleştirin ve **iletilen trafiğe** izin ver ' i işaretleyin ve **ağ geçidi aktarımına izin verin**.
1. Bağlı olduğunuz VNet 'e eşlenmekte olan VNet üzerinde bir eşleme bağlantısı ekleyin. Hedef VNet 'e eşleme bağlantısı eklenirken **sanal ağ erişimine Izin ver** ' i etkinleştirin ve **iletilen trafiğe** izin ver ' i işaretleyin ve **uzak ağ geçitlerine izin**verin.
1. Portalda App Service plan > Ağ > VNet tümleştirme Kullanıcı arabirimine gidin.  Uygulamanızın bağlandığı sanal ağı seçin. Yönlendirme bölümünde, uygulamanızın bağlı olduğu VNet ile ilişkilendirilen VNet 'in adres aralığını ekleyin.  

## <a name="managing-vnet-integration"></a>VNet tümleştirmesini yönetme 

VNet ile bağlanma ve bağlantı kesilmesi uygulama düzeyindedir. Birden çok uygulama genelinde VNet tümleştirmesini etkileyebilecek işlemler App Service plan düzeyidir. App > Ağ > VNet tümleştirme Portalı ' ndan VNet 'iniz hakkında ayrıntılı bilgi edinebilirsiniz. ASP > Ağ > VNet tümleştirme portalındaki ASP düzeyinde benzer bilgiler görebilirsiniz.

VNet tümleştirmenize ilişkin uygulama görünümünde gerçekleştirebileceğiniz tek işlem, uygulamanızın bağlı olduğu VNet 'ten bağlantısını kesmesidir. Uygulamanızın VNet bağlantısını kesmek için **bağlantıyı kes**' i seçin. VNet bağlantısı kesildiğinde uygulamanız yeniden başlatılacak. Bağlantı kesilmesi VNet 'i değiştirmez. Alt ağ veya ağ geçidi kaldırılmaz. Daha sonra sanal ortamınızı silmek istiyorsanız, öncelikle uygulamanızın VNet bağlantısını kesmeniz ve ağ geçitleri gibi kaynakları silmeniz gerekir. 

ASP VNet tümleştirmesi Kullanıcı arabirimi, ASP 'inizdeki uygulamalar tarafından kullanılan tüm VNet tümleştirmelerini gösterir. Her VNet hakkındaki ayrıntıları görmek için ilgilendiğiniz VNet 'e tıklayın. Ağ Geçidi gerekli VNet tümleştirmesi için burada gerçekleştirebileceğiniz iki eylem vardır.

* **Ağ eşitleme**. Ağ eşitleme işlemi yalnızca ağ geçidine bağımlı VNet tümleştirme özelliği için geçerlidir. Bir eşitleme ağı işleminin gerçekleştirilmesi, sertifikalarınızın ve ağ bilgilerinizin eşitlenmiş olmasını sağlar. VNet 'nizin DNS 'sini ekler veya değiştirirseniz, bir **eşitleme ağı** işlemi gerçekleştirmeniz gerekir. Bu işlem, bu VNet 'i kullanan tüm uygulamaları yeniden başlatacak.
* **Rota Ekle** Yolların eklenmesi, giden trafiği sanal ağınıza yönlendirir. 

**Ağ Geçidi gerekli VNET tümleştirme yönlendirmesi** VNet 'iniz üzerinde tanımlanan yollar uygulamanızdan sanal ağınıza giden trafiği yönlendirmek için kullanılır. VNet 'e ek giden trafik göndermeniz gerekiyorsa, bu adres bloklarını buraya ekleyebilirsiniz. Bu özellik yalnızca ağ geçidi gerekli VNet tümleştirmesi ile kullanılabilir. Ağ Geçidi gerekli VNet tümleştirmesinin bölgesel VNet tümleştirmeyle yaptıkları şekilde kullanılması durumunda yol tabloları uygulama trafiğinizi etkilemez.

**Ağ Geçidi gerekli VNET tümleştirme sertifikaları** Ağ Geçidi gereken VNet tümleştirmesi etkinleştirildiğinde, bağlantının güvenliğini sağlamak için gerekli bir sertifika alışverişi vardır. Sertifikaların yanı sıra, DNS yapılandırması, rotalar ve ağı tanımlayan diğer benzer şeylerdir.
Sertifikalar veya ağ bilgileri değiştirilirse "ağı Eşitle" seçeneğine tıklamanız gerekir. "Ağ Eşitle" seçeneğine tıkladığınızda, uygulamanız ve VNet 'iniz arasındaki bağlantıda kısa bir kesinti oluşmasına neden olursunuz. Uygulamanız yeniden başlatılana karşın bağlantı kaybı sitenizin düzgün şekilde çalışmamasına neden olabilir. 

## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Bölgesel VNet tümleştirme özelliğinin, ASP fiyatlandırma katmanı ücretleri dışında kullanılmak üzere ek ücret yoktur.

Ağ Geçidi gerekli VNet tümleştirme özelliğinin kullanımına yönelik üç ilgili ücret vardır:

* ASP fiyatlandırma katmanı ücretleri-uygulamalarınızın standart, Premium veya PremiumV2 App Service bir planda olması gerekir. Bu maliyetler hakkında daha fazla ayrıntı için şu adımları görebilirsiniz: [App Service fiyatlandırması][ASPricing]. 
* Veri aktarımı maliyetleri-VNet aynı veri merkezinde olsa bile veri çıkışı için ücret uygulanır. Bu ücretler [veri aktarımı fiyatlandırma ayrıntılarında][DataPricing]açıklanmaktadır. 
* VPN Gateway maliyetler-Noktadan siteye VPN için gereken VNet ağ geçidinin maliyeti vardır. Ayrıntılar [VPN Gateway fiyatlandırma][VNETPricing] sayfasıdır.

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>Otomasyon

Bölgesel VNet tümleştirmesi için CLı desteği vardır. Aşağıdaki komutlara erişmek için [Azure CLI 'Yı yüklemelisiniz][installCLI]. 

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

Ağ Geçidi gerekli VNet tümleştirmesi için, PowerShell kullanarak App Service bir Azure sanal ağı ile tümleştirebilirsiniz. Bir çalıştırma için, bkz. bir [uygulamayı Azure sanal ağına bağlama Azure App Service](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).


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
