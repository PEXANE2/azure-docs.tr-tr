---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 08/06/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8a4bbe92cc1b34801abffa1e905d873d4382025a
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912523"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Azure sanal ağ geçidi (VPN Gateway) ile Azure Sanal WAN vpngateway arasında ne fark vardır?

Sanal WAN geniş ölçekli Siteden Siteye bağlantı sağlar; aktarım hızı, ölçeklendirilebilirlik ve kullanım kolaylığı için oluşturulmuştur. Sanal WAN bağlantısı için ExpressRoute Şu anda önizleme aşamasındadır. CPE dal cihazları oto sağlama ve Azure sanal WAN 'a bağlanma. Bu cihazlar büyüyen bir SD-WAN ve VPN iş ortakları ekosisteminden sağlanır. [Tercih edilen Iş ortağı listesine](https://go.microsoft.com/fwlink/p/?linkid=2019615)bakın.

### <a name="what-is-a-branch-connection-to-azure-virtual-wan"></a>Azure sanal WAN ile dal bağlantısı nedir?

İki etkin/etkin IPSec tünellerinden oluşan Şube cihazından Azure sanal WAN 'a bağlantı.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Başlangıçta hangi cihaz sağlayıcıları (Sanal WAN iş ortakları) desteklenir?

Şu anda birçok iş ortağı tümüyle otomatik Sanal WAN deneyimini desteklemektedir. Daha fazla bilgi için bkz. [Sanal WAN iş ortakları](https://go.microsoft.com/fwlink/p/?linkid=2019615). 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>Sanal WAN iş ortağı otomasyonu adımları nedir?

İş ortağı otomasyonu adımları için bkz. [Sanal WAN iş ortağı otomasyonu](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Tercih edilen bir iş ortağı cihazını kullanmam gerekiyor mu?

Hayır. IKEv2/IKEv1 IPsec desteğinin Azure gereksinimlerini karşılayan, VPN özellikli herhangi bir cihaz kullanabilirsiniz.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Sanal WAN iş ortakları Azure Virtual WAN ile bağlantıyı nasıl otomatik hale getirir?

Yazılım tanımlı bağlantı çözümleri normalde dal cihazlarını yönetmek için bir denetleyici veya cihaz sağlama merkezi kullanır. Denetleyici, Azure Sanal WAN'a otomatik bağlantı sağlamak için Azure API'lerini kullanabilir. Daha fazla bilgi için bkz. Sanal WAN iş ortağı otomasyonu.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Sanal WAN mevcut bağlantı özelliklerinden herhangi birini değiştiriyor mu?   

Mevcut Azure bağlantı özelliklerinde hiçbir değişiklik yoktur.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sanal WAN için yeni Resource Manager kaynakları sağlanıyor mu?
  
Evet, Sanal WAN yeni Resource Manager kaynakları getirir. Daha fazla bilgi için bkz. [Genel Bakış](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>Tek bir Hub’a kaç adet VPN cihazı bağlanabilir?

Sanal hub başına en fazla 1.000 bağlantı desteklenir. Her bağlantı, etkin-etkin yapılandırmasına sahip iki tünelden oluşur. Tüneller bir Azure Sanal Hub vpngateway'de sonlandırılır.

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>Şirket içi VPN cihazı birden çok Hub’a bağlanabilir mi?

Evet. Başlarken trafik akışı şirket içi cihazdan en yakın Microsoft edge’e ve ardından Sanal Hub’a doğru gerçekleştirilir.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Azure Sanal WAN ile Genel Sanal Ağ Eşleme destekleniyor mu? 

 Hayır.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Sanal hub'a bağlı uç sanal ağları birbiriyle iletişim kurabiliyor mu?

Evet. Bağlı olan VNET 'ler, sanal ağ eşlemesi aracılığıyla doğrudan iletişim kurabilir. Ancak hub aracılığıyla geçişli olarak iletişim kuran VNET 'leri desteklemiyoruz. Daha fazla bilgi için bkz. [Sanal Ağ Eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Azure Sanal WAN ile tercih ettiğim ağ sanal cihazını (bir NVA sanal ağı içinde) dağıtabilir ve kullanabilir miyim?

Evet, en sevdiğiniz ağ sanal gereci(NVA) VNet’ini Azure Sanal WAN’a bağlayabilirsiniz. İlk olarak ağ sanal gereci VNet’ini Hub Sanal Ağ bağlantısının olduğu hub’a bağlayın. Ardından sonraki atlamanın Sanal Gerece işaret ettiği bir Sana Hub yönlendirmesi oluşturun. Sanal Hub Rota Tablosuna birden fazla rota uygulayabilirsiniz. Uç VNet yollarının şirket içi sistemlere yayıldığından emin olunması için NVA VNet’e bağlı olan tüm uçların ek olarak sanal hub’a bağlı olması gerekmektedir.

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Bir NVA sanal ağının sanal ağ geçidi olabilir mi?

Hayır. NVA sanal ağı sanal hub'a bağlıysa, bunun sanal ağ geçidi olamaz. 

### <a name="is-there-support-for-bgp"></a>BGP için destek var mı?

Evet, BGP desteklenir. Bir VPN sitesi oluşturduğunuzda BGP parametrelerini buna sağlayabilirsiniz. Bu, bu site için Azure 'da oluşturulan tüm bağlantıların BGP için etkinleştirileceğini kapsıyor. Ayrıca, bir NVA ile VNet 'iniz varsa ve bu NVA VNet bir sanal WAN hub 'ına eklenmişse, bir NVA VNET 'ten gelen yolların uygun şekilde tanıtıldığından emin olmak için NVA VNET 'e bağlı olan ışınsal 'ler BGP 'yi devre dışı bırakmalıdır. Ayrıca, bağlı olan VNet yollarının şirket içi sistemlere yayıldığından emin olmak için bu bağlı ağ sanal ağlarını sanal hub VNet 'e bağlayın.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Sanal hub'da UDR kullanarak trafiği yönlendirebilir miyim?

Evet, Sanal Hub Yol Tablosunu kullanarak trafiği bir VNet’e yönlendirebilirsiniz.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sanal WAN ile ilgili lisans ve fiyatlandırma bilgileri var mı?
 
Evet. Bkz. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-wan/) sayfası.

### <a name="how-do-i-calculate-price-of-a-hub"></a>Bir hub 'ın fiyatını hesaplamak Nasıl yaparım? mı?
 
Hub 'da hizmet için ödeme yaparsınız. Örneğin, Azure sanal WAN 'a bağlanması gereken 10 dal veya şirket içi cihazlar, hub 'daki VPN uç noktalarına bağlanmayı kapsıyor. Bunun 1 ölçekli birim = 500 Mbps hızında VPN olduğunu varsayalım. Bu, $0.361/HR üzerinden ücretlendirilir. Her bağlantı $0.08/HR üzerinden ücretlendirilir. 10 bağlantı için toplam hizmet ücreti/hr $0,361 + $. 8/ HR. Azure 'dan çıkmadan trafik için veri ücretleri. 

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>İlk iş ortağı listenizde yer almayan yeni iş ortakları nasıl eklenir?

azurevirtualwan@microsoft.com adresine e-posta gönderin. İdeal bir iş ortağı, IKEv1 veya IKEv2 IPsec bağlantısına yönelik sağlanabilen bir cihaza sahip olandır.

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>Kullandığım bir cihaz sanal WAN iş ortağı listesinde yoksa ne olur? Azure sanal WAN VPN 'e bağlanmak için kullanmaya devam edebilir miyim?

Bu, cihaz IPSec IKEv1 veya Ikev2 'yi desteklediği sürece evet. Sanal WAN iş ortakları cihazdan Azure VPN uç noktalarına bağlantıyı otomatik hale getirir. Bu, ' dal bilgileri yüklemesi ', ' IPSec ve yapılandırma ' ve ' bağlantı ' gibi adımları otomatikleştirmeye yönelik bir ifade gerektirir. Cihazınız bir sanal WAN iş ortağı ekosisteminden olmadığından, Azure yapılandırmasını el ile alma ve cihazınızı IPSec bağlantısı kurmak üzere güncelleştirme konusunda ağır bir kaldırma yapmanız gerekir. 

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Azure Sanal WAN’ı bir Resource Manager şablonu ile yapılandırmak mümkün mü?

Tek bir hub ve bir vpnsite içeren bir sanal WAN 'ın basit bir yapılandırması, bir [Azure hızlı başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network)kullanılarak oluşturulabilir. Sanal WAN birincil olarak bir REST veya Portal odaklı bir hizmettir.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Sanal WAN’da daldan dala bağlantıya izin verilir mi?

Evet, daldan dala bağlantı VPN için Sanal WAN’da ve VPN ile ExpressRoute arasında kullanılabilir. VPN siteden siteye GA olduğunda ExpressRoute Şu anda önizleme aşamasındadır.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Azure sanal WAN aracılığıyla daldan dala trafiği çapraz geçiş yapar mi?

Evet.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Sanal WAN, mevcut Azure Sanal Ağ’dan nasıl farklıdır?

Sanal Ağ Geçidi VPN’i 30 tünelle sınırlıdır. Bağlantılar için, büyük ölçekli VPN’lere yönelik Sanal WAN kullanmanız gerekir. Tüm bölgeler için hub 'da 20 Gbps 'e kadar 1.000 şube bağlantısı bağlayabilirsiniz. Bağlantı şirket içi VPN cihazından sanal hub’a giden bir etkin-etkin tüneldir. Her bölge için bir hub 'ınız olabilir, bu, hub 'larda 1.000 ' den fazla dalı bağlayabilmeniz anlamına gelir.

### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>SD-WAN cihazlarını sanal WAN 'ı destekleme

Sanal WAN ortakları, Azure VPN uç noktalarıyla IPSec bağlantısını otomatik hale getirir. Sanal WAN iş ortağı bir SD-WAN sağlayıcısıysanız, SD-WAN denetleyicisi 'nin Azure VPN uç noktalarıyla otomasyonu ve IPSec bağlantısını yönetmesi yerinde olur. SD-WAN cihazı, herhangi bir özel SD-WAN işlevselliği için Azure VPN yerine kendi uç noktasını gerektiriyorsa, SD-WAN uç noktasını bir Azure VNet 'te dağıtabilir ve Azure sanal WAN ile bir arada bulunabilir.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Bu Sanal WAN her siteden ExpressRoute alınmasını gerektirir mi?

Hayır, Sanal WAN her siteden ExpressRoute alınmasını gerektirmez. Cihazdan Azure Sanal WAN merkezine kurulan İnternet bağlantıları aracılığıyla standart IPsec siteden siteye bağlantı kullanır. Siteleriniz ExpressRoute bağlantı hattı kullanılarak bir sağlayıcı ağına bağlanabilir. Sanal Hub’da ExpressRoute kullanılarak bağlanan Siteler için (Önizlemede) VPN ile ExpressRoute arasında daldan dala trafik akışı olabilir. 

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Azure Sanal WAN kullanırken bir ağ aktarım hızı sınırı var mı?

Dal sayısı hub/bölge başına 1000 bağlantıyla ve hub 'da toplam 20 Gbps ile sınırlıdır.

### <a name="i-dont-see-the-20-gbps-setting-for-the-virtual-hub-in-the-portal-how-do-i-configure-that"></a>Portalda sanal hub için 20 Gbps ayarı görmüyorum. Nasıl yaparım? yapılandırmak mı istiyorsunuz?

Şu anda, [Update-AzVpnGateway](https://docs.microsoft.com/powershell/module/az.network/update-azvpngateway) cmdlet 'ini kullanarak 20 Gbps için ağ geçidi ölçek birimlerini yapılandırabilirsiniz. Bu ayar, portalda kullanılabilir olacak yol haritasında bulunur.

### <a name="how-many-vpn-connections-does-a-virtual-wan-hub-support"></a>Bir sanal WAN hub 'ı kaç VPN bağlantısı destekliyor?

Azure sanal WAN hub, en fazla 1.000 S2S bağlantısı ve 10.000 P2S bağlantısını aynı anda destekleyebilir.

### <a name="what-is-the-total-vpn-throughput-of-a-vpn-tunnel-and-a-connection"></a>VPN tünelinin toplam VPN işleme ve bağlantı nedir?

Bir hub 'ın toplam VPN verimlilik, seçilen ölçek birimine bağlı olarak 20 Gbps 'e kadar olur. Aktarım hızı, var olan tüm bağlantılar tarafından paylaşılır.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Sanal WAN, şirket içi cihazın paralel olarak birden fazla ISS’den yararlanmasına izin verir mi, yoksa her zaman tek bir VPN tüneli mi var?

Sanal WAN VPN 'ye gelen bir bağlantı, dalda bulunan bir bağlantı kullanılarak her zaman etkin-etkin bir tüneldir (aynı hub/bölge dahilinde esneklik için). Bu bağlantı, şirket içi dalda bir ISS bağlantısı olabilir. Sanal WAN, paralel olarak birden çok ISS 'yi kurmak için özel bir mantık sağlamaz; dalda ISS genelinde yük devretmeyi yönetmek, tamamen şube merkezli bir ağ işlemidir. En sevdiğiniz SD-WAN çözümünüzü, dalda yol seçimi yapmak için kullanabilirsiniz.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Azure temelinde trafik nasıl yönlendirilir?

Trafik şu şekilde izler: şube cihazı > ISP-> Microsoft Edge-> Microsoft DC (hub VNet)-> Microsoft Edge-> ISS-> Şube cihazı

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Bu modelde her sitede neye ihtiyacınız var? Yalnızca interneti bağlantısına mı?

Evet. Tercihen tümleşik [iş ortaklarımız](https://go.microsoft.com/fwlink/p/?linkid=2019615)tarafından IPSec 'i destekleyen bir Internet bağlantısı ve fiziksel cihaz. İsteğe bağlı olarak Azure bağlantısını ve yapılandırmasını tercih ettiğiniz cihazdan el ile yönetebilirsiniz.
