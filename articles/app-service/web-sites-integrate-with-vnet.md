---
title: Uygulamayı Azure Sanal Ağı ile tümleştir
description: Azure Uygulama Hizmeti'ndeki uygulamaları Azure Sanal Ağlar ile tümleştirin.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 02/27/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 89aa78e0d26598eacf436ca88cc6c5549f91d2fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673221"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı bir Azure Sanal Ağı ile tümleştirme
Bu belge, Azure App Service sanal ağ tümleştirme özelliğini ve [Azure Uygulama Hizmeti'ndeki](https://go.microsoft.com/fwlink/?LinkId=529714)uygulamalarla nasıl ayarlanınan açıklanmaktadır. [Azure Sanal Ağlar][VNETOverview] (VNet'ler), Azure kaynaklarınızın çoğunu internet olmayan bir ağa yerleştirmenize olanak tanır.  

Azure Uygulama Hizmeti'nin iki varyasyonu vardır.

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>VNet Tümleştirmesini Etkinleştir 

1. App Service portalındaki Ağ AraBirimi'ne gidin. VNet Tümleştirmesi altında *"Yapılandırmak için buraya tıklayın" seçeneğini belirleyin.* 

1. **VNet Ekle'yi**seçin.  

   ![VNet Tümleştirme'yi seçin][1]

1. Açılan liste, aboneliğinizdeki tüm Kaynak Yöneticisi VNet'lerini aynı bölgedeki ve aşağıda diğer tüm bölgelerdeki Tüm Kaynak Yöneticisi VNet'lerinin listesini içerecektir. Entegre etmek istediğiniz VNet'i seçin.

   ![VNet'i seçin][2]

   * VNet aynı bölgedeyse, yeni bir alt ağ oluşturun veya önceden varolan boş bir alt ağ seçin. 

   * Başka bir bölgede bir VNet seçmek için, siteye nokta etkinleştirilmiş bir Sanal Ağ ağ geçidine sahip olmalısınız.

   * Klasik VNet ile entegre olmak için, VNet açılır düşüşünü tıklatmak yerine, **Klasik VNet'e bağlanmak için buraya tıklayın'ı**seçin. İstediğinklasik VNet'i seçin. Hedef VNet'in zaten site etkinleştirilmiş bir noktaya işaretle birlikte bir Sanal Ağ ağ geçidi olması gerekir.

    ![Klasik VNet'i Seçin][3]
    
Tümleştirme sırasında uygulamanız yeniden başlatılır.  Tümleştirme tamamlandığında, entegre olduğunuz VNet'in ayrıntılarını görürsünüz. 

Uygulamanız VNet'inizle entegre edildikten sonra, Azure DNS Özel Bölgeleri olmadığı sürece VNet'inizin yapılandırıldığında aynı DNS sunucusunu kullanır. Şu anda Azure DNS Özel Bölgeleri ile VNet Tümleştirme'yi kullanamazsınız.

## <a name="regional-vnet-integration"></a>Bölgesel VNet Entegrasyonu

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

### <a name="how-regional-vnet-integration-works"></a>Bölgesel VNet Entegrasyonu nasıl çalışır?

Uygulama Hizmeti'ndeki uygulamalar çalışan rollerinde barındırılır. Temel ve daha yüksek fiyatlandırma planları, aynı işçiler üzerinde çalışan başka müşteri iş yüklerinin olmadığı barındırma planlarına adanmıştır. Bölgesel VNet Tümleştirmesi, temsilci alt netindeki adreslerle sanal arayüzler oluşturarak çalışır. Adres VNet'inizde olduğundan, VNet'inizdeki bir VM gibi VNet'iniz deki veya vNet'iniz aracılığıyla birçok şeye erişebilir. Ağ uygulaması VNet'inizde VM çalıştırmaktan farklıdır ve bu nedenle bu özelliği kullanırken bazı ağ özellikleri henüz kullanılamamaktadır.

![Bölgesel VNet Tümleştirme nasıl çalışır?][5]

Bölgesel VNet Tümleştirmesi etkinleştirildiğinde, uygulamanız yine de normal kanallar aracılığıyla internete giden aramalar yapar. Uygulama özellikleri portalında listelenen giden adresler hala uygulamanız tarafından kullanılan adreslerdir. Uygulamanız için ne gibi değişiklikler, hizmet uç noktası güvenli hizmetlere yapılan aramalar veya RFC 1918 adresleri VNet'inize gider. WEBSITE_VNET_ROUTE_ALL 1 olarak ayarlanmışsa, tüm giden trafik VNet'inize gönderilebilir. 

Özellik, çalışan başına yalnızca bir sanal arabirimi destekler.  Çalışan başına bir sanal arabirim, Uygulama Hizmeti planı başına bir bölgesel VNet Tümleştirmesi anlamına gelir. Aynı Uygulama Hizmeti planındaki tüm uygulamalar aynı VNet Tümleştirmesini kullanabilir, ancak ek bir VNet'e bağlanmak için bir uygulamaya ihtiyacınız varsa, başka bir Uygulama Hizmeti planı oluşturmanız gerekir. Kullanılan sanal arabirim, müşterilerin doğrudan erişebilen bir kaynak değildir.

Bu teknolojinin çalışma şekli nedeniyle, VNet Tümleştirmesi ile kullanılan trafik Network Watcher veya NSG akış günlüklerinde görünmüyor.  

## <a name="gateway-required-vnet-integration"></a>Ağ Geçidi gerekli VNet Entegrasyonu

Ağ Geçidi gerekli VNet Tümleştirme başka bir bölgedeki bir VNet'e veya Klasik VNet'e bağlanmayı destekler. Ağ Geçidi gerekli VNet Entegrasyonu: 

* Bir uygulamanın aynı anda yalnızca 1 VNet'e bağlanmasını sağlar
* Bir Uygulama Hizmet Planına en fazla beş VNet'in entegre edilmesini sağlar 
* Aynı VNet'in, Bir Uygulama Hizmeti planı tarafından kullanılabilecek toplam sayıyı etkilemeden, bir Uygulama Hizmet Planı'nda birden fazla uygulama tarafından kullanılmasına izin verir.  Aynı Uygulama Hizmeti planında aynı VNet'i kullanan altı uygulamanız varsa, bu 1 VNet'in kullanıldığı sayılır. 
* Ağ geçidindeki SLA nedeniyle %99,9'luk bir SLA'yı destekler
* Uygulamalarınızın VNet'in yapılandırıldığının DNS'sini kullanmasını sağlar
* Uygulamaya bağlanabilmek için SSTP noktadan siteye VPN ile yapılandırılan Sanal Ağ rota tabanlı bir ağ geçidi gerektirir. 

Gerekli VNet Tümleştirmeağlarını kullanamazsınız:

* Linux uygulamaları ile
* ExpressRoute'a bağlı bir VNet ile 
* Hizmet Bitiş Noktaları'na erişmek için güvenli kaynaklar
* Hem ExpressRoute'u destekleyen hem de site/siteye işaret eden bir birlikte yaşam ağ geçidi yle VPN'ler

### <a name="set-up-a-gateway-in-your-vnet"></a>VNet'inizde bir ağ geçidi ayarlama ###

Ağ geçidi oluşturmak için:

1. VNet'inizde [bir ağ geçidi alt ağı oluşturun.][creategatewaysubnet]  

1. [VPN ağ geçidini oluşturun.][creategateway] Rota tabanlı BIR VPN türü seçin.

1. [Noktayı site adreslerine ayarlayın.][setp2saddresses] Ağ geçidi temel SKU'da değilse, IKEV2 site yapılandırması noktasında devre dışı bırakılmalı ve SSTP seçilmelidir. Site adres alanı noktası RFC 1918 adres blokları, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 olmalıdır

Uygulama Hizmeti VNet Tümleştirmesi ile kullanım için ağ geçidi oluşturuyorsanız, sertifika yüklemeniz gerekmez. Ağ geçidi oluşturma 30 dakika sürebilir. Ağ geçidi sağlanana kadar uygulamanızı VNet'inizle entegre edemeyeceksiniz. 

### <a name="how-gateway-required-vnet-integration-works"></a>Ağ geçidi gerekli VNet Tümleştirme nasıl çalışır?

Gateway, site VPN teknolojisinin üzerine inşa edilmiş VNet Tümleştirmesi'ni gerekli kılması gerekiyordu. Site VPN'leri için işaret, ağ erişimini uygulamayı barındıran sanal makineyle sınırlar. Uygulamalar yalnızca Internet'e, Hibrit Bağlantılar aracılığıyla veya VNet Tümleştirmesi aracılığıyla trafik göndermekle sınırlıdır. Uygulamanız ağ geçidi gerekli VNet Entegrasyonu kullanmak için portal ile yapılandırıldığında, ağ geçidi ve uygulama tarafında sertifika oluşturmak ve atamak için sizin adınıza karmaşık bir anlaşma yönetilir. Sonuç olarak, uygulamalarınızı barındırmak için kullanılan çalışanlar, seçili VNet'teki sanal ağ ağ geçidine doğrudan bağlanabiliyor. 

![Ağ geçidi gerekli VNet Tümleştirme nasıl çalışır?][6]

### <a name="accessing-on-premises-resources"></a>Şirket içi kaynaklara erişme

Uygulamalar, siteden siteye bağlantıları olan VNet'lerle tümleştirerek şirket içi kaynaklara erişebilir. Gerekli VNet Tümleştirmesi ağ geçidini kullanıyorsanız, şirket içi VPN ağ geçidi yollarınızı yerinde adres bloklarınızla güncelleştirmeniz gerekir. Siteden siteye VPN ilk ayarlandığında, yapılandırmak için kullanılan komut dosyaları yolları düzgün şekilde ayarlamalıdır. Siteden siteye VPN'inizi oluşturduktan sonra noktadan siteye adresleri eklerseniz, yolları el ile güncelleştirmeniz gerekir. Bunun nasıl yapılacağının ayrıntıları ağ geçidine göre değişir ve burada açıklanmaz. BGP'yi siteden siteye VPN bağlantısıyla yapılandıramazsınız.

Bölgesel VNet Tümleştirme özelliğinin VNet'iniz aracılığıyla ve şirket içinde ulaşması için ek yapılandırma gerekmez. VNet'inizi ExpressRoute veya siteden siteye VPN'i kullanarak şirket içi bağlantınıza bağlamanız yeterlidir. 

> [!NOTE]
> Gerekli ağ geçidi VNet Tümleştirme özelliği, expressroute ağ geçidi olan bir VNet ile bir uygulamayı tümleştirmez. ExpressRoute Ağ Geçidi [birlikte yaşam modunda][VPNERCoex] yapılandırılsa bile VNet Tümleştirmesi çalışmaz. Kaynaklara ExpressRoute bağlantısı üzerinden erişmeniz gerekiyorsa, VNet'inizde çalışan bölgesel VNet Tümleştirme özelliğini veya [Uygulama Hizmet Ortamını][ASE]kullanabilirsiniz. 
> 
> 

### <a name="peering"></a>Eşleme

Bölgesel VNet Tümleştirmesi'ni eşleme kullanıyorsanız, ek yapılandırma yapmanız gerekmez. 

Eşleme ile gerekli VNet Tümleştirmesi ağ geçidini kullanıyorsanız, birkaç ek öğe yapılandırmanız gerekir. Uygulamanızla çalışacak şekilde eşlemi yapılandırmak için:

1. Uygulamanızın bağlandığı VNet'e bir bakış bağlantısı ekleyin. Eşleme bağlantısı eklerken, **sanal ağ erişimine izin ver'i** etkinleştirin ve iletilen trafiğe İzin ver ve ağ geçidi geçişine izin ver'i işaretleyin'i **işaretleyin'i** **etkinleştirin.**
1. Bağlı olduğunuz VNet'e bakan VNet'e bir peering bağlantısı ekleyin. Hedef VNet'e eşleme bağlantısı eklerken, **sanal ağ erişimine izin ver'i** etkinleştirin ve **iletilen trafiğe İzin Ver'i** ve uzak ağ **geçitlerine izin ver'i**denetleyin.
1. Portaldaki Ağ > VNet Integration UI'> Uygulama Hizmeti planına gidin.  Uygulamanızın bağlandığı VNet'i seçin. Yönlendirme bölümünün altında, uygulamanızın bağlı olduğu VNet ile bakan VNet'in adres aralığını ekleyin.  

## <a name="managing-vnet-integration"></a>VNet Entegrasyonunu Yönetme 

Bir VNet ile bağlantı kurmak ve bağlantıyı kesmek bir uygulama düzeyindedir. Birden çok uygulama arasında VNet Tümleştirmesini etkileyebilecek işlemler Uygulama Hizmeti planı düzeyindedir. Networking > VNet Integration portalı> uygulamadan VNet'inizle ilgili ayrıntıları alabilirsiniz. Benzer bilgileri ASP > Ağ > VNet Tümleştirme portalında ASP düzeyinde görebilirsiniz.

VNet Tümleştirmenizin uygulama görünümünde alabileceğiniz tek işlem, uygulamanızın bağlantısını şu anda bağlı olduğu VNet ile bağlantısını kesmektir. Uygulamanızın bir VNet bağlantısını kesmek için **Bağlantıyı Kesme'yi**seçin. Bir VNet ile bağlantınızı kestiğinizde uygulamanız yeniden başlatılır. Bağlantının kesilmesi VNet'inizi değiştirmez. Alt ağ veya ağ geçidi kaldırılmaz. Daha sonra VNet'inizi silmek istiyorsanız, öncelikle uygulamanızın VNet'in bağlantısını kesmeniz ve ağ geçitleri gibi kaynakları silmeniz gerekir. 

ASP VNet Entegrasyon UI, ASP'nizdeki uygulamalar tarafından kullanılan tüm VNet entegrasyonlarını gösterir. Her VNet'in ayrıntılarını görmek için ilgilendiğiniz VNet'e tıklayın. Ağ geçidi için gerekli VNet Tümleştirmesi için burada gerçekleştirebileceğiniz iki eylem vardır.

* **Eşitleme ağı**. Eşitleme ağı işlemi yalnızca ağ geçidine bağımlı VNet Tümleştirme özelliği içindir. Eşitleme ağı işlemi, sertifikalarınızın ve ağ bilgilerinizin eşit olmasını sağlar. VNet'inizin DNS'sini ekler veya değiştirirseniz, bir **Eşitleme ağı** işlemi gerçekleştirmeniz gerekir. Bu işlem, bu VNet'i kullanarak tüm uygulamaları yeniden başlatacaktır.
* **Rota ekleme** Rotalar eklemek, giden trafiği VNet'inize yönlendirir. 

**Ağ Geçidi gerekli VNet Tümleştirme Yönlendirme** VNet'inizde tanımlanan rotalar, uygulamanızdan VNet'inize trafiği yönlendirmek için kullanılır. VNet'e ek giden trafik göndermeniz gerekiyorsa, bu adres bloklarını buraya ekleyebilirsiniz. Bu özellik yalnızca gerekli ağ geçidi VNet Tümleştirmesi ile çalışır. Yol tabloları, ağ geçidi ni kullanarak bölgesel VNet Tümleştirmesi ile olduğu gibi Gerekli VNet Tümleştirmesi'ni kullanırken uygulama trafiğinizi etkilemez.

**Ağ Geçidi gerekli VNet Entegrasyon Sertifikaları** Ağ geçidi gerektiğinde VNet Tümleştirmesi etkinleştirildiğinde, bağlantının güvenliğini sağlamak için gerekli bir sertifika değişimi vardır. Sertifikalarla birlikte DNS yapılandırması, rotalar ve ağı tanımlayan diğer benzer şeyler de vardır.
Sertifikalar veya ağ bilgileri değiştirilirse, "Eşitleme Ağı"nı tıklatmanız gerekir. "Eşitleme Ağı"nı tıklattığınızda, uygulamanızla VNet'iniz arasındaki bağlantıda kısa bir kesintiye neden olursunuz. Uygulamanız yeniden başlatılmamasa da, bağlantı kaybı sitenizin düzgün çalışmamasına neden olabilir. 

## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Bölgesel VNet Tümleştirme özelliği, ASP fiyatlandırma katmanı ücretlerinin ötesinde kullanım için ek bir ücret eve sahiptir.

Gerekli ağ geçidi nin kullanımıyla ilgili üç ücret vardır:

* ASP fiyatlandırma katmanı ücretleri - Uygulamalarınızın Standart, Premium veya PremiumV2 Uygulama Hizmet Planı'nda olması gerekir. Bu maliyetler hakkında daha fazla ayrıntıyı burada görebilirsiniz: [Uygulama Hizmeti Fiyatlandırması.][ASPricing] 
* Veri aktarım maliyetleri - VNet aynı veri merkezinde olsa bile veri çıkış için bir ücret vardır. Bu ücretler [Veri Aktarımı Fiyatlandırma Ayrıntıları'nda][DataPricing]açıklanmıştır. 
* VPN Ağ Geçidi maliyetleri - Noktaya-to-site VPN için gerekli olan VNet ağ geçidi için bir maliyet vardır. Ayrıntılar [VPN Ağ Geçidi Fiyatlandırma][VNETPricing] sayfasında dır.

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>Automation

Bölgesel VNet Entegrasyonu için CLI desteği vardır. Aşağıdaki komutlara erişmek için [Azure CLI'yi yükleyin.][installCLI] 

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

Ağ geçidi için gerekli VNet Tümleştirmesi için, PowerShell'i kullanarak Uygulama Hizmetini bir Azure Sanal Ağı ile tümleştirebilirsiniz. Çalışmaya hazır bir komut dosyası için Azure [Uygulama Hizmeti'ndeki bir uygulamayı Azure Sanal Ağına Bağlayın'a](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3)bakın.


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
