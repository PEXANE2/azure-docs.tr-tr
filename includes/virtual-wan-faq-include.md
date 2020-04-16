---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 03/24/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a2d79391832bbb5424c6d4096eb5c1a597623367
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81422217"
---
### <a name="does-the-user-need-to-have-hub-and-spoke-with-sd-wanvpn-devices-to-use-azure-virtual-wan"></a>Azure Virtual WAN'ı kullanabilmek için kullanıcının hub'a sahip olması ve SD-WAN/VPN aygıtlarıyla konuşması gerekiyor mu?

Virtual WAN, Site/Siteden siteye VPN bağlantısı, User/P2S bağlantısı, ExpressRoute bağlantısı, Sanal Ağ bağlantısı, VPN ExpressRoute Ara bağlantı, VNET vnet vnet geçişli bağlantı, Merkezi Yönlendirme, Azure güvenlik duvarı ve güvenlik duvarı yöneticisi güvenliği, İzleme, ExpressRoute Şifreleme ve diğer birçok özellik gibi tek bir cam bölmesine yerleşik birçok işlev sağlar. Sanal WAN kullanmaya başlamak için bu kullanım örneklerinin tümüne sahip olmak zorunda değildir. Sadece tek bir kullanım örneği ile başlamak olabilir. Virtual WAN mimarisi, şubelerin (VPN/SD-WAN aygıtları), kullanıcıların (Azure VPN Istemcileri, openVPN istemcileri veya IKEv2 Istemcileri), ExpressRoute devreleri, Sanal Ağlar'ın Sanal Hub(lar) sözcüsü olarak hizmet verdiği ölçek ve performans yerleşik bir hub ve kollu mimaridir. Tüm hub'lar Standart Sanal WAN'da tam ağ la bağlanır ve kullanıcının Microsoft omurgasını herhangi bir (herhangi bir konuşmalı) bağlantı için kullanmasını kolaylaştırır. Hub ve SD-WAN/VPN aygıtları için kullanıcılar azure Sanal WAN portalında el ile ayarlayabilir veya Azure bağlantısı ayarlamak için Virtual WAN Partner CPE'yi (SD-WAN/VPN) kullanabilir. Sanal WAN iş ortakları, aygıt bilgilerini Azure'a dışa aktarabilme, Azure yapılandırmasını indirme ve Azure Virtual WAN hub'ına bağlantı kurma olanağı olan bağlantı otomasyonu sağlar. Noktaya Sayfa/Kullanıcı VPN bağlantısı için Azure [VPN istemcisi](https://go.microsoft.com/fwlink/?linkid=2117554), OpenVPN veya IKEv2 istemcisini destekliyoruz. 

### <a name="what-client-does-the-azure-virtual-wan-user-vpn-point-to-site-support"></a>Azure Virtual WAN Kullanıcı VPN (Noktaya Sayfa) hangi istemciyi destekliyor?

Virtual [WAN, Azure VPN istemcisini,](https://go.microsoft.com/fwlink/?linkid=2117554)OpenVPN Istemcisini veya herhangi bir IKEv2 istemcisini destekler. Azure AD kimlik doğrulaması Azure VPN İstemci ile desteklenir.En az Windows 10 istemci işletim sistemi sürümü 17763.0 veya üstü gereklidir.  OpenVPN istemcisi(ler) sertifika tabanlı kimlik doğrulamasını destekleyebilir. Ağ geçidinde cert tabanlı auth seçildikten sonra cihazınıza indirilecek .ovpn dosyasını görürsünüz. Hem sertifika hem de RADIUS auth IKEv2 ile desteklenir. 

### <a name="for-user-vpn-point-to-site--why-is-the-p2s-client-pool-split-into-two-routes"></a>Kullanıcı VPN 'i için (Noktadan siteye)- Neden P2S istemci havuzu iki rotaya bölünür?

Her ağ geçidinin iki örneği vardır, bölme, böylece her ağ geçidi örneği bağlı istemciler için istemci IP'lerini bağımsız olarak tahsis edebilir ve sanal ağdaki trafik, ağ geçidi örnek atlamayı önlemek için doğru ağ geçidi örneğine yönlendirilir.

### <a name="how-do-i-add-dns-servers-for-p2s-clients"></a>P2S istemcileri için DNS sunucularını nasıl ekleyebilirim?

P2S istemcileri için DNS sunucuları eklemek için iki seçenek vardır.

1. Microsoft ile bir destek bileti açın ve DNS sunucularınızı hub'a eklemelerini sağlayın
2. Veya Windows 10 için Azure VPN İstemci'sini kullanıyorsanız, indirilen profil XML dosyasını değiştirebilir ve ** \<dnsserver \<> \< \</dnsserver>/dnsserver>** etiketleri>içe aktarabilirsiniz.

```
<azvpnprofile>
<clientconfig>

    <dnsservers>
        <dnsserver>x.x.x.x</dnsserver>
        <dnsserver>y.y.y.y</dnsserver>
    </dnsservers>
    
</clientconfig>
</azvpnprofile>
```

### <a name="for-user-vpn-point-to-site--how-many-clients-are-supported"></a>Kullanıcı VPN (Noktaya)- kaç istemci desteklenir?

Her Kullanıcı VPN P2S ağ geçidinin iki örneği vardır ve ölçek birimi değiştikçe her örnek belirli kullanıcılara kadar destekler. Ölçek ünitesi 1-3 500 bağlantıyı destekler, Ölçek birimi 4-6 1000 bağlantıyı destekler, Ölçek birimi 7-10 5000 bağlantıyı destekler ve Ölçek birimi 11+ 10.000 bağlantıyı destekler. Örnek olarak, kullanıcının 1 ölçek birimi seçtiğini söyleyelim. Her ölçek birimi etkin etkin ağ geçidinin dağıtılmış olduğunu ve örneklerin her birinin (bu durumda 2) 500'e kadar bağlantıyı destekleyeceğini ima eder. Ağ geçidi başına 500 bağlantı * 2 alabilirsiniz bu yana, örnekleri önerilen bağlantı sayısını aşarsanız ekstra 500 için bağlantı kesintiye uğrayabilir sırasında servis gerekebilir gibi bu ölçek birimi için 500 yerine 1000 planı anlamına gelmez.

### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpn-gateway"></a>Azure sanal ağ ağ geçidi (VPN Ağ Geçidi) ile Azure Virtual WAN VPN ağ geçidi arasındaki fark nedir?

Sanal WAN geniş ölçekli Siteden Siteye bağlantı sağlar; aktarım hızı, ölçeklendirilebilirlik ve kullanım kolaylığı için oluşturulmuştur. Bir siteyi Sanal WAN VPN ağ geçidine bağladığınızda, ağ geçidi türü 'VPN' kullanan normal bir sanal ağ ağ geçidinden farklıdır. Benzer şekilde, bir ExpressRoute devresini Sanal WAN hub'ına bağladığınızda, ExpressRoute ağ geçidi için ağ geçidi için ağ geçidi 'ExpressRoute' türü kullanan normal sanal ağ ağ geçidinden farklı bir kaynak kullanır. Virtual WAN, hem VPN hem de ExpressRoute için 20 Gbps'ye kadar toplam iş büyümesini destekler. Virtual WAN ayrıca CPE şube cihaz ortaklarının ekosistemi ile bağlantı otomasyonuna sahiptir. CPE şube aygıtları, Azure Virtual WAN'a otomatik hükümler sağlayan ve bağlanan yerleşik otomasyona sahiptir. Bu cihazlar büyüyen bir SD-WAN ve VPN iş ortakları ekosisteminden sağlanır. Tercih [Edilen İş Ortağı Listesi'ne](../articles/virtual-wan/virtual-wan-locations-partners.md)bakın.

### <a name="how-is-virtual-wan-different-from-an-azure-virtual-network-gateway"></a>Virtual WAN'ın Azure sanal ağ ağ geçidinden farkı nedir?

Sanal ağ ağ geçidi VPN 30 tünelle sınırlıdır. Bağlantılar için, büyük ölçekli VPN’lere yönelik Sanal WAN kullanmanız gerekir. Hub başına toplam 20 Gbps ile bölge başına en fazla 1.000 şube bağlantısı (sanal hub) bağlayabilirsiniz. Bağlantı şirket içi VPN cihazından sanal hub’a giden bir etkin-etkin tüneldir. Bölge başına bir hub'A sahip olabilirsiniz, bu da hub'lar arasında 1.000'den fazla şube bağlayabileceğiniz anlamına gelir.

### <a name="what-is-a-virtual-wan-gateway-scale-unit"></a>Sanal WAN Ağ Geçidi Ölçeği Birimi Nedir
Ölçek birimi, Sanal hub'daki ağ geçidinin toplam iş birliğini seçmek için tanımlanan birimdir. VPN 1 ölçek birimi = 500 Mbps . ExpressRoute 1 ölçek birimi = 2 Gbps. Örnek : VPN 10 ölçek birimi 500 Mbps * 10 = 5 Gbps anlamına gelir

### <a name="which-device-providers-virtual-wan-partners-are-supported"></a>Hangi aygıt sağlayıcıları (Virtual WAN ortakları) desteklenir?

Şu anda birçok iş ortağı tümüyle otomatik Sanal WAN deneyimini desteklemektedir. Daha fazla bilgi için bkz. [Sanal WAN iş ortakları](../articles/virtual-wan/virtual-wan-locations-partners.md). 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>Sanal WAN iş ortağı otomasyonu adımları nedir?

İş ortağı otomasyonu adımları için bkz. [Sanal WAN iş ortağı otomasyonu](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Tercih edilen bir iş ortağı cihazını kullanmam gerekiyor mu?

Hayır. IKEv2/IKEv1 IPsec desteğinin Azure gereksinimlerini karşılayan, VPN özellikli herhangi bir cihaz kullanabilirsiniz.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Sanal WAN iş ortakları Azure Virtual WAN ile bağlantıyı nasıl otomatik hale getirir?

Yazılım tanımlı bağlantı çözümleri normalde dal cihazlarını yönetmek için bir denetleyici veya cihaz sağlama merkezi kullanır. Denetleyici, Azure Sanal WAN'a otomatik bağlantı sağlamak için Azure API'lerini kullanabilir. Otomasyon, şube bilgilerini yüklemeyi, Azure yapılandırmasını indirmeyi, IPSec tünellerini Azure Sanal hub'larına ayarlamayı ve şube aygıtını Azure Virtual WAN'a otomatik olarak bağlama yı içerir. Yüzlerce şubeniz olduğunda, Sanal WAN CPE İş Ortakları'nı kullanarak bağlanmak kolaydır, çünkü biniş deneyimi büyük ölçekli IPsec bağlantısını ayarlama, yapılandırma ve yönetme gereksinimini ortadan kaldırsın. Daha fazla bilgi için [Virtual WAN iş ortağı otomasyonuna](../articles/virtual-wan/virtual-wan-configure-automation-providers.md)bakın.


### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>Sanal WAN SD-WAN aygıtlarını nasıl destekliyor?

Sanal WAN iş ortakları, Azure VPN bitiş noktalarına IPsec bağlantısını otomatikleştirin. Sanal WAN iş ortağı bir SD-WAN sağlayıcısıysa, SD-WAN denetleyicisinin Azure VPN bitiş noktalarına otomasyon ve IPsec bağlantısını yönettiği ima edilir. SD-WAN aygıtı, herhangi bir özel SD-WAN işlevi için Azure VPN yerine kendi bitiş noktasını gerektiriyorsa, SD-WAN bitiş noktasını bir Azure VNet'te dağıtabilir ve Azure Virtual WAN ile birlikte varoşlayabilirsiniz.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Sanal WAN mevcut bağlantı özelliklerinden herhangi birini değiştiriyor mu?

Mevcut Azure bağlantı özelliklerinde hiçbir değişiklik yoktur.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sanal WAN için yeni Resource Manager kaynakları sağlanıyor mu?
  
Evet, Sanal WAN yeni Resource Manager kaynakları getirir. Daha fazla bilgi için bkz. [Genel Bakış](../articles/virtual-wan/virtual-wan-about.md).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>Tek bir hub'a kaç VPN aygıtı bağlanabilir?

Sanal hub başına en fazla 1.000 bağlantı desteklenir. Her bağlantı dört bağlantıdan oluşur ve her bağlantı etkin-etkin yapılandırmada bulunan iki tüneli destekler. Tüneller bir Azure sanal hub vpnağ geçidinde sonlandırır.

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>Şirket içi VPN cihazı birden çok Hub’a bağlanabilir mi?

Evet. Trafik akışı, başlatılırken şirket içi aygıttan en yakın Microsoft ağ kenarına ve ardından sanal hub'a akar.

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Azure Sanal WAN ile tercih ettiğim ağ sanal cihazını (bir NVA sanal ağı içinde) dağıtabilir ve kullanabilir miyim?

Evet, en sevdiğiniz ağ sanal gereci(NVA) VNet’ini Azure Sanal WAN’a bağlayabilirsiniz. İlk olarak ağ sanal gereci VNet’ini Hub Sanal Ağ bağlantısının olduğu hub’a bağlayın. Ardından, Sanal Cihaz'ı işaret eden bir sonraki atlamayla sanal hub rotası oluşturun. Sanal hub Rota Tablosu'na birden çok rota uygulayabilirsiniz. Uç VNet yollarının şirket içi sistemlere yayıldığından emin olunması için NVA VNet’e bağlı olan tüm uçların ek olarak sanal hub’a bağlı olması gerekmektedir.

### <a name="can-i-create-a-network-virtual-appliance-inside-the-virtual-hub"></a>Sanal hub içinde bir Ağ Sanal Cihaz oluşturabilir miyim?

Ağ Sanal Cihaz (NVA) sanal hub içinde dağıtılamaz. Ancak, sanal hub'a bağlı bir kollu VNet'te oluşturabilir ve hub'daki bir rotanın NVA IP adresi (NIC) üzerinden hedef VNet trafiğini yönlendirmesini sağlayabilirsiniz.

### <a name="can-a-spoke-vnet-have-a-virtual-network-gateway"></a>Bir vNet'in sanal ağ ağ geçidi olabilir mi?

Hayır. Sanal hub'a bağlıysa, konuşan VNet'in sanal ağ ağ geçidi olamaz.

### <a name="is-there-support-for-bgp"></a>BGP için destek var mı?

Evet, BGP desteklenir. Bir VPN sitesi oluşturduğunuzda, içinde BGP parametrelerini sağlayabilirsiniz. Bu, bu site için Azure'da oluşturulan bağlantıların BGP için etkinleştirileceği anlamına gelecektir. Ayrıca, NVA'lı bir VNet'iniz varsa ve bu NVA VNet sanal wan hub'ına bağlıysa, bir NVA VNet'ten gelen yolların uygun şekilde duyurulduğundan emin olmak için, NVA VNet'e bağlı sözcülerin BGP'yi devre dışı bması gerekir. Ayrıca, bu kollu VNet'leri sanal hub VNet'e bağlayarak kollu VNet yollarının şirket içi sistemlere yayılmasını sağlayın.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Sanal hub'da UDR kullanarak trafiği yönlendirebilir miyim?

Evet, sanal hub rota tablosunu kullanarak trafiği VNet'e yönlendirebilirsiniz. Bu, belirli bir IP adresi (genellikle NVA NIC) aracılığıyla Azure'daki hedef VNet'ler için rotalar ayarlamanıza olanak tanır.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sanal WAN ile ilgili lisans ve fiyatlandırma bilgileri var mı?

Evet. Bkz. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-wan/) sayfası.

### <a name="how-do-i-calculate-price-of-a-hub"></a>Bir hub'ın fiyatını nasıl hesaplayabilirim?

* Merkezdeki hizmetlerin parasını ödeyeceksiniz. Örneğin, Azure Virtual WAN'a bağlanmak için gereken 10 şubeniz veya şirket içi aygıtınız olduğunu ve hub'daki VPN bitiş noktalarına bağlanmanız anlamına geleceğini varsasınız. Bu 1 ölçek birim = 500 Mbps VPN olduğunu söylüyorlar, bu 0,361 $ / saat tahsil edilir. Her bağlantı $0.05/sa ücretlendirilir. 10 bağlantı için, hizmet/saat toplam ücreti $0.361 + $.5/saat olacaktır. Azure'dan ayrılan trafik için veri ücretleri uygulanır.

* Ek hub ücreti vardır. Bkz. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-wan/) sayfası.

* Virtual Hub'a bağlanan ExpressRoute devreleri nedeniyle ExpressRoute ağ geçidiniz varsa, ölçek birim fiyatı için ödeme yapardınız. ER'deki her ölçek birimi 2 Gbps'dir ve her bağlantı birimi VPN Bağlantı ünitesi ile aynı oranda ücretlendirilir.

* Hub'a bağlı Spoke VNET'lerin varsa, Spoke VNETs'deki izleme ücretleri hala geçerlidir. 

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>İlk iş ortağı listenizde yer almayan yeni iş ortakları nasıl eklenir?

Tüm sanal WAN API'leri açık API'dir. Teknik fizibiliteyi değerlendirmek için belgelerin üzerinden geçebilirsiniz. Herhangi bir sorunuz varsa, azurevirtualwan@microsoft.combir e-posta gönderin. İdeal bir iş ortağı, IKEv1 veya IKEv2 IPsec bağlantısına yönelik sağlanabilen bir cihaza sahip olandır.

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>Kullandığım bir aygıt Sanal WAN iş ortağı listesinde değilse ne olur? Azure Virtual WAN VPN'e bağlanmak için kullanmaya devam edebilir miyim?

Evet sürece cihaz IPsec IKEv1 veya IKEv2 destekler. Sanal WAN iş ortakları, aygıttan Azure VPN bitiş noktalarına bağlantı otomatikleştirin. Bu, 'şube bilgileri yükleme', 'IPsec ve yapılandırma' ve 'bağlantı' gibi adımları otomatikleştirmek anlamına gelir. Cihazınız Sanal WAN iş ortağı ekosisteminden olmadığı için, IPsec bağlantısını ayarlamak için Azure yapılandırmasını el ile alma ve cihazınızı güncelleme gibi ağır kaldırma işlemi yapmanız gerekir.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Azure Sanal WAN’ı bir Resource Manager şablonu ile yapılandırmak mümkün mü?

Bir hub ve bir vpnsitesi ile bir Sanal WAN basit bir yapılandırma [quickstart şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network)kullanılarak oluşturulabilir. Sanal WAN öncelikle bir REST veya portal odaklı bir hizmettir.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Azure Sanal WAN ile Genel Sanal Ağ Eşleme destekleniyor mu? 

Bir VNet'i sanal WAN'ınızdan farklı bir bölgeye bağlayabilirsiniz.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other-v2v-transit"></a>Sanal bir hub'a bağlı kollu VNet'ler birbirleriyle iletişim kurabilir mi (V2V Transit)?

Evet. Standart Sanal WAN, Vnet'lerin bağlı olduğu Virtual WAN hub'ı aracılığıyla Vnet'e geçişli bağlantı sağlar. Sanal WAN terminolojisinde, bu yolları tek bir bölgedeki Sanal Wan Hub'a bağlı VNet'ler için "yerel Sanal WAN VNet geçişi" ve iki veya daha fazla bölgede birden fazla Sanal WAN Hub'ı aracılığıyla bağlanan VNet'ler için "küresel Virtual WAN VNet geçişi" olarak adlandırıyoruz. VNet transit, genel önizleme sırasında 3 Gbps'ye kadar iş elde etmeyi destekler. Küresel geçiş GA gittiğinde iş lenecektir.

NOT: Şu anda V2V transit önizleme başlatılacak yönlendirme öğelerini tetiklemek için sanal hub'da bir VPN GW'sinin dağıtılmasını gerektirir. Bu VPN GW V2V geçiş yolu için kullanılmaz. Bu bilinen bir sınırlamadır ve V2V GA sırasında kaldırılacaktır. V2V geçiş işlevselliği için gerekli olmadığı için vpn ağ geçidini tam olarak başlatıldıktan sonra hub(lar)taki VPN Ağ Geçidi'ni silebilirsiniz. 

Bazı senaryolar için, yerel veya küresel Virtual WAN VNet geçişine ek olarak [Sanal Ağ Eşlemenliği](../articles/virtual-network/virtual-network-peering-overview.md) kullanılarak doğrudan birbirleri ile konuşan vnet'ler de görülebilir. Bu durumda, Vnet Peering Sanal WAN hub üzerinden geçişli bağlantıdan önce gelir. 

### <a name="what-is-a-branch-connection-to-azure-virtual-wan"></a>Azure Virtual WAN'a şube bağlantısı nedir?

Bir dal aygıtından Azure Virtual WAN'a bağlantı en fazla dört bağlantıyı destekler. Bağlantı, şube konumundaki fiziksel bağlantıdır (örneğin: ATT, Verizon vb.). Her bağlantı iki etkin/etkin IPsec tünelinden oluşur.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Sanal WAN’da daldan dala bağlantıya izin verilir mi?

Evet, daldan dala bağlantı VPN için Sanal WAN’da ve VPN ile ExpressRoute arasında kullanılabilir.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Şubeden şubeye trafik Azure Sanal WAN'dan geçiyor mu?

Evet.

### <a name="does-virtual-wan-require-expressroute-from-each-site"></a>Virtual WAN her siteden ExpressRoute gerektirir mi?

Hayır, Sanal WAN her siteden ExpressRoute alınmasını gerektirmez. Cihazdan Azure Virtual WAN hub'ına internet bağlantıları aracılığıyla standart IPsec site-site bağlantısı kullanır. Siteleriniz ExpressRoute bağlantı hattı kullanılarak bir sağlayıcı ağına bağlanabilir. Sanal bir hub'da ExpressRoute kullanılarak bağlanan siteler için, vpn ve ExpressRoute arasındaki trafik akışını şubelemek için şubeye sahip olabilir.

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Azure Sanal WAN kullanırken bir ağ aktarım hızı sınırı var mı?

Şube sayısı hub/bölge başına 1000 bağlantı ve hub'da toplam 20 Gbps ile sınırlıdır. Bölge başına 1 hub'A sahip olabilirsiniz.

### <a name="how-many-vpn-connections-does-a-virtual-wan-hub-support"></a>Virtual WAN hub'ı kaç VPN bağlantısını destekler?

Azure Virtual WAN hub'ı 1.000'e kadar S2S bağlantısını, 10.000 P2S bağlantısını ve 4 ExpressRoute bağlantısını aynı anda destekleyebilir.

### <a name="what-is-the-total-vpn-throughput-of-a-vpn-tunnel-and-a-connection"></a>VPN tünelinin ve bağlantının toplam VPN çıkışı nedir?

Bir hub'ın toplam VPN iş bölümü, seçilen ölçek birimine göre 20 Gbps'ye kadardır. İş paylaşımı varolan tüm bağlantılar tarafından paylaşılır. Bağlantıdaki her tünel 1 Gbps'ye kadar destekleyebilir.

### <a name="i-dont-see-the-20-gbps-setting-for-the-virtual-hub-in-the-portal-how-do-i-configure-that"></a>Ben portalda sanal hub için 20 Gbps ayarı görmüyorum. Bunu nasıl yapılandıracağım?

Portaldaki bir hub'ın içindeki VPN ağ geçidine gidin ve uygun ayara değiştirmek için ölçek birimine tıklayın.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Virtual WAN, şirket içi aygıtın birden fazla ISS'yi paralel olarak kullanmasına izin veriyor mu, yoksa her zaman tek bir VPN tüneli midir?

Sanal BIR WAN VPN'e gelen bir bağlantı, şubede bulunan bir bağlantıyı kullanarak her zaman etkin bir tüneldir (aynı hub/bölge içinde esneklik için). Bu bağlantı, şirket içi şubedeki bir ISS bağlantısı olabilir. Virtual WAN 'VPNSite' siteye bağlantı bilgileri ekleme olanağı sağlar. Şubede birden fazla ISS varsa ve HER ISS bir bağlantı sağlamışsa, bu bilgiler Azure'daki VPN site bilgilerinde ayarlanabilir. Ancak, şubedeki ISS'ler arasında başarısız olmak tamamen dal merkezli bir yönlendirme işlemidir.

### <a name="what-is-global-transit-architecture"></a>Küresel transit mimarisi nedir?

Küresel transit mimarisi hakkında daha fazla bilgi için [Global transit ağı mimarisi ve Virtual WAN](../articles/virtual-wan/virtual-wan-global-transit-network-architecture.md)bölümüne bakın.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Azure temelinde trafik nasıl yönlendirilir?

Trafik deseni izler: şube aygıtı ->ISS >Microsoft ağ kenarı >Microsoft DC (hub VNet)->Microsoft ağ kenarı->ISS->dal aygıtı

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Bu modelde her sitede neye ihtiyacınız var? Yalnızca interneti bağlantısına mı?

Evet. Tercihen entegre [Virtual WAN iş ortaklarımızdan,](../articles/virtual-wan/virtual-wan-locations-partners.md)IPsec destekleyen bir internet bağlantısı ve fiziksel cihaz. İsteğe bağlı olarak, yapılandırmayı ve Azure bağlantısını tercih ettiğiniz cihazdan el ile yönetebilirsiniz.

### <a name="how-do-i-enable-default-route-00000-in-a-connection-vpn-expressroute-or-virtual-network"></a>Bir bağlantıda (VPN, ExpressRoute veya Virtual Network) varsayılan rotayı (0.0.0.0/0) nasıl etkinleştirebilirim:

Sanal hub, bayrak bağlantıda 'Etkinleştirilmiş' ise öğrenilen varsayılan rotayı sanal ağa/siteden siteye VPN/ExpressRoute bağlantısına yayabilir. Kullanıcı sanal ağ bağlantısını, VPN bağlantısını veya ExpressRoute bağlantısını yeniden elediğinde bu bayrak görünür. Varsayılan olarak, bir site veya ExpressRoute devresi bir hub'a bağlandığında bu bayrak devre dışı bırakılır. Sanal bir hub'a vnet bağlamak için sanal ağ bağlantısı eklendiğinde varsayılan olarak etkinleştirilir. Varsayılan rota Sanal WAN hub'ında kaynaklanmaz; hub'a bir güvenlik duvarı nın dağıtılması sonucunda Sanal WAN hub'ı tarafından zaten öğrenilmişse veya bağlı başka bir site zorunlu tünelleme etkinleştirilmişse varsayılan rota yayılır.

### <a name="how-does-the-virtual-hub-in-a-virtual-wan-select-the-best-path-for-a-route-from-multiple-hubs"></a>Sanal WAN'daki sanal hub, birden çok hub'dan bir rota için en iyi yolu nasıl seçer?

Sanal Hub aynı rotayı birden çok uzak hub'dan öğrenirse, karar verdiği sıra aşağıdaki gibidir
1) Rota Kaynağı a) Ağ yolları – Sanal Hub ağ geçitleri tarafından doğrudan öğrenilen VNET önekleri b) BGP c) Hub RouteTable (statik yapılandırılan rotalar) d) InterHub rotaları
2)  Rota ölçümü : Virtual WAN, VPN üzerinden ExpressRoute'u tercih eder. ExpressRoute eş VPN eş göre daha yüksek bir ağırlık var
3)  AS yol uzunluğu

### <a name="what-are-the-differences-between-the-virtual-wan-types-basic-and-standard"></a>Sanal WAN türleri (Temel ve Standart) arasındaki farklar nelerdir?

'Basic' WAN türü temel bir hub (SKU = Basic) oluşturmanıza olanak tanır. 'Standart' WAN türü standart hub (SKU = Standart) oluşturmanıza olanak sağlar. Temel hub'lar siteden siteye VPN işleviyle sınırlıdır. Standart hub'lar ExpressRoute, User VPN (P2S), tam ağ hub'ı ve hub'lar aracılığıyla VNet-to-VNet geçişine sahip olabilirsiniz. Standart hub'lar için $0,25/saat taban ücret ve VNet-to-VNet bağlantısı sırasında hub'lardan geçiş için veri işleme ücretinin yanı sıra hub'dan hub trafiğine veri işleme ücreti ödenebilirsiniz. Daha fazla bilgi için [Temel ve Standart sanal WAN'lara](../articles/virtual-wan/virtual-wan-about.md#basicstandard)bakın. Fiyatlandırma için [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-wan/) sayfasına bakın.
