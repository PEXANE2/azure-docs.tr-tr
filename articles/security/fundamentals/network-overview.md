---
title: Azure'da ağ güvenliği kavramları ve gereksinimleri | Microsoft Dokümanlar
description: Bu makalede, temel ağ güvenliği kavramları ve gereksinimleri hakkında temel açıklamalar ve Azure'un bu alanların her birinde neler sunduğu hakkında bilgiler verilmektedir.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: terrylan
ms.openlocfilehash: 496ee1bc97f6b72e09a62ae3491af7ccc7328583
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811093"
---
# <a name="azure-network-security-overview"></a>Azure ağ güvenliğine genel bakış

Ağ güvenliği, denetimleri ağ trafiğine uygulayarak kaynakları yetkisiz erişime veya saldırılara karşı koruma süreci olarak tanımlanabilir. Amaç, yalnızca yasal trafiğe izin verildiğini sağlamaktır. Azure, uygulama ve hizmet bağlantısı gereksinimlerinizi desteklemek için sağlam bir ağ altyapısı içerir. Ağ bağlantısı, Azure'da bulunan kaynaklar arasında, şirket içi ve Azure barındırılan kaynaklar arasında ve Internet ve Azure'dan ve Azure'dan mümkündür.

Bu makale, Azure'un ağ güvenliği alanında sunduğu seçeneklerden bazılarını kapsar. Şunlar hakkında bilgi edinebilirsiniz:

* Azure ağ
* Ağ erişim denetimi
* Azure Güvenlik Duvarı
* Güvenli uzaktan erişim ve tesisler arası bağlantı
* Kullanılabilirlik
* Ad çözümlemesi
* Çevre ağı (DMZ) mimarisi
* Azure DDoS koruması
* Azure Front Door
* Traffic manager
* İzleme ve tehdit algılama

## <a name="azure-networking"></a>Azure ağ

Azure, sanal makinelerin bir Azure Sanal Ağına bağlanmasını gerektirir. Sanal ağ, fiziksel Azure ağ dokusunun üzerine inşa edilmiş mantıksal bir yapıdır. Her sanal ağ diğer tüm sanal ağlardan izole edilmiştir. Bu, dağıtımlarınızdaki ağ trafiğinin diğer Azure müşterileri tarafından erişilememesini sağlamaya yardımcı olur.

Daha fazla bilgi edinin:

* [Sanal ağa genel bakış](../../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Ağ erişim denetimi

Ağ erişim denetimi, sanal ağdaki belirli aygıtlara veya alt ağlara bağlantı nın sınırlandırılması eylemidir. Ağ erişim denetiminin amacı, sanal makinelerinize ve hizmetlerinize erişimi onaylanmış kullanıcılar ve aygıtlarla sınırlamaktır. Erişim denetimleri, sanal makinenize veya hizmetinize ve bunlara ve bunlara bağlantı vermeme kararlarına dayanır.

Azure, şu gibi çeşitli ağ erişim denetimtürlerini destekler:

* Ağ katmanı denetimi
* Rota kontrolü ve zorunlu tünel
* Sanal ağ güvenlik cihazları

### <a name="network-layer-control"></a>Ağ katmanı denetimi

Herhangi bir güvenli dağıtım ağ erişim denetimi bir ölçü gerektirir. Ağ erişim denetiminin amacı, sanal makine iletişimini gerekli sistemlerle sınırlamaktır. Diğer iletişim girişimleri engellendi.

> [!NOTE]
> Depolama Güvenlik Duvarları [Azure depolama güvenliğine genel bakış](storage-overview.md) makalesinde yer almaktadır

#### <a name="network-security-rules-nsgs"></a>Ağ güvenliği kuralları (NSGs)

Temel ağ düzeyi erişim denetimine (IP adresine ve TCP veya UDP protokollerine dayalı olarak) ihtiyacınız varsa, Ağ Güvenlik Grupları'nı (NSGs) kullanabilirsiniz. NSG temel, durum lu, paket filtreleme güvenlik duvarıdır ve [5-tuple'a](https://www.techopedia.com/definition/28190/5-tuple)dayalı erişimi denetlemenizi sağlar. NSG'ler yönetimi basitleştirmek ve yapılandırma hatalarını azaltmak için işlevsellik içerir:

* **Artırılmış güvenlik kuralları** NSG kural tanımını basitleştirir ve aynı sonuca ulaşmak için birden çok basit kural oluşturmak yerine karmaşık kurallar oluşturmanıza olanak sağlar.
* **Hizmet etiketleri,** bir grup IP adresini temsil eden Microsoft tarafından oluşturulan etiketlerdir. Etikete eklenmesini tanımlayan koşulları karşılayan IP aralıklarını içerecek şekilde dinamik olarak güncellenirler. Örneğin, doğu bölgesindeki tüm Azure depolama alanı için geçerli bir kural oluşturmak istiyorsanız Storage.EastUS'u kullanabilirsiniz
* **Uygulama güvenlik grupları,** kaynakları uygulama gruplarına dağıtmanıza ve bu uygulama gruplarını kullanan kurallar oluşturarak bu kaynaklara erişimi denetlemenize olanak sağlar. Örneğin, 'Web sunucuları' uygulama grubuna dağıtılmış web sunucularınız varsa, Internet'ten 'Web sunucuları' uygulama grubundaki tüm sistemlere 443 trafik sağlayan bir NSG uygulayan bir kural oluşturabilirsiniz.

NSG'ler uygulama katmanı denetimi veya kimlik doğrulaması erişim denetimleri sağlamaz.

Daha fazla bilgi edinin:

* [Ağ Güvenlik Grupları](../../virtual-network/security-overview.md)

#### <a name="asc-just-in-time-vm-access"></a>ASC tam zamanında VM erişimi

[Azure güvenlik merkezi,](../../security-center/security-center-intro.md) UYGUN rol tabanlı erişim denetimine sahip bir kullanıcı [RBAC](/azure/role-based-access-control/overview) izinlerine sahip bir kullanıcı erişim isteyene kadar NSG'leri VM'lerde yönetebilir ve VM'ye erişimi kilitleyebilir. Kullanıcı başarıyla yetkialdığında ASC, belirtilen süre boyunca seçili bağlantı noktalarına erişim sağlamak için NSG'lerde değişiklikler yapar. Süre dolduğunda NSG'ler önceki güvenli durumuna geri yüklenir.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi Tam Zamanında Erişim](../../security-center/security-center-just-in-time.md)

#### <a name="service-endpoints"></a>Hizmet uç noktaları

Hizmet bitiş noktaları, trafiğiniz üzerinde denetim uygulamanın başka bir yoludur. Desteklenen hizmetlerle iletişimi doğrudan bağlantı üzerinden sadece VNet'lerinizle sınırlandırabilirsiniz. VNet'inizden belirtilen Azure hizmetine kadar olan trafik Microsoft Azure omurga ağında kalır.  

Daha fazla bilgi edinin:

* [Hizmet uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md#secure-azure-services-to-virtual-networks)

### <a name="route-control-and-forced-tunneling"></a>Rota kontrolü ve zorunlu tünel

Sanal ağlarınızdaki yönlendirme davranışını denetleme yeteneği çok önemlidir. Yönlendirme yanlış yapılandırılırsa, sanal makinenizde barındırılan uygulamalar ve hizmetler, olası saldırganlar tarafından sahip olunan ve işletilen sistemler de dahil olmak üzere yetkisiz aygıtlara bağlanabilir.

Azure ağ, sanal ağlarınızdaki ağ trafiği için yönlendirme davranışını özelleştirme olanağını destekler. Bu, sanal ağınızdaki varsayılan yönlendirme tablosu girişlerini değiştirmenize olanak tanır. Yönlendirme davranışının denetimi, belirli bir aygıttan veya aygıt grubundan gelen tüm trafiğin belirli bir konum üzerinden sanal ağınıza girmesini veya bölgeden ayrılmasını sağlamanıza yardımcı olur.

Örneğin, sanal ağınızda bir sanal ağ güvenlik cihazınız olabilir. Sanal ağınıza gelen ve sanal ağınızdan gelen tüm trafiğin bu sanal güvenlik cihazından geçtiğinden emin olmak istersiniz. Bunu, Azure'da [Kullanıcı Tanımlı Yolları](../../virtual-network/virtual-networks-udr-overview.md) (ÜDR) yapılandırarak yapabilirsiniz.

[Zorunlu tünelleme,](https://www.petri.com/azure-forced-tunneling) hizmetlerinizin internetüzerindeki aygıtlara bağlantı başlatmasına izin verilmemesini sağlamak için kullanabileceğiniz bir mekanizmadır. Bunun gelen bağlantıları kabul etmekten ve sonra bunlara yanıt vermekten farklı olduğunu unutmayın. Ön uç web sunucularının internet ana bilgisayarlarından gelen isteklere yanıt vermesi gerekir ve bu nedenle internet kaynaklı trafiğin bu web sunucularına girmesine izin verilir ve web sunucularının yanıt vermesine izin verilir.

İzin vermek istemediğiniz şey, giden bir istek başlatmak için bir ön uç web sunucusudur. Bu tür istekler, kötü amaçlı yazılım indirmek için kullanılabildiği için bir güvenlik riski teşkil edebilir. Bu ön uç sunucuların internete giden istekleri başlatmasını isteseniz bile, onları şirket içi web vekillerinizaracılığıyla geçmeye zorlamak isteyebilirsiniz. Bu, URL filtreleme ve günlüğe kaydetme avantajlarından yararlanmanızı sağlar.

Bunun yerine, bunu önlemek için zorunlu tünel kullanmak isteyebilirsiniz. Zorunlu tünel leri etkinleştirdiğinizde, internete tüm bağlantılar şirket içi ağ geçidinizden zorlanır. Zorlanmış tünelleri, ÜD'lerden yararlanarak yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Kullanıcı Tanımlı Rotalar ve IP Yönlendirme Nedir](../../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Sanal ağ güvenlik cihazları

NSG'ler, İşLetim'ler ve zorunlu tünelleme, [OSI modelinin](https://en.wikipedia.org/wiki/OSI_model)ağ ve aktarım katmanlarında bir güvenlik düzeyi sağlarken, ağdan daha yüksek düzeylerde güvenliği etkinleştirmek de isteyebilirsiniz.

Örneğin, güvenlik gereksinimleriniz şunları içerebilir:

* Başvurunuza erişime izin vermeden önce kimlik doğrulama ve yetkilendirme
* Saldırı algılama ve izinsiz giriş tepkisi
* Üst düzey protokoller için uygulama katmanı denetimi
* URL filtreleme
* Ağ düzeyinde antivirüs ve Antimalware
* Anti-bot koruması
* Uygulama erişim kontrolü
* Ek DDoS koruması (Azure kumaşın kendisi tarafından sağlanan DDoS korumasının üzerinde)

Azure iş ortağı çözümlerini kullanarak bu gelişmiş ağ güvenliği özelliklerine erişebilirsiniz. [Azure Marketi'ni](https://azure.microsoft.com/marketplace/)ziyaret ederek ve "güvenlik" ve "ağ güvenliği" araması yaparak en güncel Azure iş ortağı ağ güvenliği çözümlerini bulabilirsiniz.

## <a name="azure-firewall"></a>Azure Güvenlik Duvarı

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirlik oranı ve kısıtlamasız bulut ölçeklenebilirliğiyle hizmet olarak tam durum bilgisi olan bir güvenlik duvarıdır. Bazı özellikler şunlardır:

* Yüksek kullanılabilirlik
* Bulut ölçeklenebilirliği
* Uygulama FQDN filtreleme kuralları
* Ağ trafiği filtreleme kuralları

Daha fazla bilgi edinin:

* [Azure Güvenlik Duvarı'na genel bakış](/azure/firewall/overview)

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Güvenli uzaktan erişim ve tesisler arası bağlantı

Azure kaynaklarınızın kurulumu, yapılandırması ve yönetimi uzaktan yapılmalıdır. Ayrıca, bileşenleri şirket içinde ve Azure genel bulutuna sahip [karma BT](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) çözümlerini dağıtmak isteyebilirsiniz. Bu senaryolar güvenli uzaktan erişim gerektirir.

Azure ağ ağı aşağıdaki güvenli uzaktan erişim senaryolarını destekler:

* Tek tek iş istasyonlarını sanal ağa bağlama
* Şirket içi ağınızı VPN ile sanal ağa bağlayın
* Şirket içi ağınızı özel bir WAN bağlantısıyla sanal ağa bağlayın
* Sanal ağları birbirine bağlama

### <a name="connect-individual-workstations-to-a-virtual-network"></a>Tek tek iş istasyonlarını sanal ağa bağlama

Azure'da tek tek geliştiricilerin veya operasyon personelinin sanal makineleri ve hizmetleri yönetmesini etkinleştirmek isteyebilirsiniz. Örneğin, sanal ağdaki sanal bir makineye erişmeniz gerektiğini varsayalım. Ancak güvenlik politikanız RDP veya SSH'nin tek tek sanal makinelere uzaktan erişmesine izin vermez. Bu durumda, [noktadan siteye VPN](../../vpn-gateway/point-to-site-about.md) bağlantısı kullanabilirsiniz.

Noktadan siteye VPN bağlantısı, kullanıcı ile sanal ağ arasında özel ve güvenli bir bağlantı kurmanızı sağlar. VPN bağlantısı kurulduğunda, kullanıcı VPN üzerinden SANAL ağdaki herhangi bir sanal makineye RDP veya SSH bağlantısı yapabilir. (Bu, kullanıcının kimlik doğrulaması yapabileceğinizi ve yetkili olduğunu varsayar.) Noktadan siteye VPN destekler:

* Güvenli Soket Tünel Protokolü (SSTP), özel bir SSL tabanlı VPN protokolü. Bir SSL VPN çözümü güvenlik duvarlarına nüfuz edebilir, çünkü çoğu güvenlik duvarı TLS/SSL'nin kullandığı TCP portu 443'ü açar. SSTP yalnızca Windows aygıtlarında desteklenir. Azure, Windows'un SSTP (Windows 7 ve sonrası) tüm sürümlerini destekler.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.

* [OpenVPN](https://azure.microsoft.com/updates/openvpn-support-for-azure-vpn-gateways/)

Daha fazla bilgi edinin:

* [PowerShell'i kullanarak sanal ağa noktadan siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-vpn"></a>Şirket içi ağınızı VPN ile sanal ağa bağlayın

Tüm şirket ağınızı veya bir kısmını sanal ağa bağlamak isteyebilirsiniz. Bu, kuruluşların şirket içi veri [merkezlerini Azure'a genişlettikleri](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84)karma BT senaryolarında yaygındır. Çoğu durumda, kuruluşlar Azure'da bir hizmetin bölümlerini ve şirket içinde bölümleri barındırıyor. Örneğin, bir çözüm Azure'daki ön uç web sunucularını ve şirket içinde arka uç veritabanlarını içerdiğinde bunu yapabilirler. Bu tür "binalar arası" bağlantılar, Azure'da bulunan kaynakların yönetimini daha güvenli hale getirmekte ve Active Directory etki alanı denetleyicilerini Azure'a genişletme gibi senaryolara olanak tanır.

Bunu başarmanın bir yolu, [siteden siteye VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn)kullanmaktır. Siteden siteye VPN ile noktadan siteye VPN arasındaki fark, ikincisinin tek bir aygıtı sanal ağa bağlamasıdır. Siteden siteye VPN, tüm ağı (şirket içi ağınız gibi) sanal ağa bağlar. Sanal bir ağa site-to-site VPN'ler son derece güvenli IPsec tünel modu VPN protokolü kullanın.

Daha fazla bilgi edinin:

* [Azure portalını kullanarak siteden siteye VPN bağlantısı olan bir Kaynak Yöneticisi VNet oluşturma](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [VPN Gateway hakkında](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-dedicated-wan-link"></a>Şirket içi ağınızı özel bir WAN bağlantısıyla sanal ağa bağlayın

Noktadan siteye ve siteden siteye VPN bağlantıları, tesisler arası bağlantıyı etkinleştirmede etkilidir. Ancak, bazı kuruluşlar bunları aşağıdaki dezavantajları var düşünün:

* VPN bağlantıları verileri internet üzerinden taşır. Bu, bu bağlantıları, genel bir ağ üzerinden veri taşımayla ilgili olası güvenlik sorunlarına maruz bırakır. Buna ek olarak, internet bağlantıları için güvenilirlik ve kullanılabilirlik garanti edilemez.
* Sanal ağlara VPN bağlantıları, yaklaşık 200 Mbps hızları ile maksimum alabildiklerinden, bazı uygulamalar ve amaçlar için bant genişliğine sahip olmayabilir.

Binalar arası bağlantıları için en yüksek düzeyde güvenliğe ve kullanılabilirliğe ihtiyaç duyan kuruluşlar genellikle uzak sitelere bağlanmak için özel WAN bağlantılarını kullanır. Azure, şirket içi ağınızı sanal bir ağa bağlamak için kullanabileceğiniz özel bir WAN bağlantısını kullanma olanağı sağlar. Azure ExpressRoute, Express route direct ve Express route global erişimi bunu etkinleştirin.

Daha fazla bilgi edinin:

* [ExpressRoute teknik genel bakış](../../expressroute/expressroute-introduction.md)
* [ExpressRoute doğrudan](../../expressroute/expressroute-erdirect-about.md)
* [Ekspres rota küresel erişim](../../expressroute/expressroute-global-reach.md)

### <a name="connect-virtual-networks-to-each-other"></a>Sanal ağları birbirine bağlama

Dağıtımlarınız için birçok sanal ağ kullanmak mümkündür. Bunu yapmanızın çeşitli nedenleri vardır. Yönetimi basitleştirmek veya daha fazla güvenlik isteyebilirsiniz. Kaynakları farklı sanal ağlara koyma motivasyonundan bağımsız olarak, ağların her birinde kaynakların birbiriyle bağlanmasını istediğiniz zamanlar olabilir.

Seçeneklerden biri, bir sanal ağdaki hizmetlerin internet üzerinden "geri döngü" yaparak başka bir sanal ağdaki hizmetlere bağlanmasıdır. Bağlantı tek bir sanal ağda başlar, internet üzerinden geçer ve sonra hedef sanal ağa geri döner. Bu seçenek, herhangi bir internet tabanlı iletişimin doğasında bulunan güvenlik sorunlarına bağlantıyı ortaya çıkarır.

Daha iyi bir seçenek, iki sanal ağ arasında bağlanan bir siteden siteye VPN oluşturmak olabilir. Bu yöntem, yukarıda belirtilen tesisler arası siteden siteye VPN bağlantısıyla aynı [IPSec tünel modu protokolünü](https://technet.microsoft.com/library/cc786385.aspx) kullanır.

Bu yaklaşımın avantajı, VPN bağlantısının Internet üzerinden bağlanmak yerine Azure ağ dokusu üzerinden kurulmasıdır. Bu, internet üzerinden bağlanan siteden siteye VPN'lerle karşılaştırıldığında fazladan bir güvenlik katmanı sağlar.

Daha fazla bilgi edinin:

* [Azure Kaynak Yöneticisi ve PowerShell'i kullanarak VNet'e VNet Bağlantısı yapılandırın](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Sanal ağlarınızı bağlamanın bir diğer yolu da [VNET'in eşlenemesidir.](../../virtual-network/virtual-network-peering-overview.md) Bu özellik, iki Azure asına bağlanmanızı sağlar, böylece aralarındaki iletişim Microsoft omurga altyapısı üzerinden Internet üzerinden hiç girmeden gerçekleşir. VNET eşleme, aynı bölge içinde iki VNET veya Azure bölgeleri arasında iki VNET bağlayabilir. NSG'ler farklı alt ağlar veya sistemler arasındaki bağlantıyı sınırlamak için kullanılabilir.

## <a name="availability"></a>Kullanılabilirlik

Kullanılabilirlik, herhangi bir güvenlik programının önemli bir bileşenidir. Kullanıcılarınız ve sistemleriniz ağ üzerinden erişmek için ihtiyaç duydukları şeye erişemiyorsa, hizmet tehlikeye atılmış olarak kabul edilebilir. Azure,aşağıdaki yüksek kullanılabilirlik mekanizmalarını destekleyen ağ teknolojilerine sahiptir:

* HTTP tabanlı yük dengeleme
* Ağ seviyesi yük dengeleme
* Küresel yük dengeleme

Yük dengeleme, bağlantıları birden çok aygıt arasında eşit olarak dağıtmak için tasarlanmış bir mekanizmadır. Yük dengelemenin hedefleri şunlardır:

* Kullanılabilirliği artırmak için. Bakiye bağlantılarını birden çok cihaza yüklediğinizde, hizmetten ödün vermeden aygıtlardan biri veya daha fazlası kullanılamaz hale gelebilir. Kalan çevrimiçi cihazlarda çalışan hizmetler, hizmetteki içeriğe hizmet vermeye devam edebilir.
* Performansı artırmak için. Bakiye bağlantılarını birden çok cihaza yüklediğinizde, tek bir aygıtın tüm işlemleri işlemesi gerekmemektedir. Bunun yerine, içeriğe hizmet için işleme ve bellek talepleri birden çok aygıta yayılır.

### <a name="http-based-load-balancing"></a>HTTP tabanlı yük dengeleme

Web tabanlı hizmetleri çalıştıran kuruluşlar genellikle bu web hizmetlerinin önünde HTTP tabanlı bir yük dengeleyicisi olmasını ister. Bu, yeterli performans ve yüksek kullanılabilirlik düzeylerinin sağlanmasına yardımcı olur. Geleneksel, ağ tabanlı yük dengeleyicileri ağ ve aktarım katmanı protokollerine dayanır. Diğer taraftan, HTTP tabanlı yük dengeleyicileri, http protokolünün özelliklerine göre kararlar verir.

Azure Application Gateway, web tabanlı hizmetleriniz için HTTP tabanlı yük dengelemesağlar. Uygulama Ağ Geçidi destekler:

* Çerez tabanlı oturum yakınlığı. Bu özellik, yük dengeleyicisinin arkasındaki sunuculardan birine kurulan bağlantıların istemci ve sunucu arasında bozulmadan kalmasını sağlar. Bu, işlemlerin kararlılığını sağlar.
* TLS boşaltma. İstemci yük dengeleyicisi ile bağlandığında, bu oturum HTTPS (TLS) protokolü kullanılarak şifrelenir. Ancak, performansı artırmak için, yük dengeleyicisi ile yük dengeleyicisinin arkasındaki web sunucusu arasında bağlantı kurmak için HTTP (şifrelenmemiş) protokolünü kullanabilirsiniz. Yük dengeleyicisinin arkasındaki web sunucuları şifrelemeyle ilgili işlemci yükünü deneyimlemediği için buna "TLS boşaltma" adı verilir. Bu nedenle web sunucuları istekleri daha hızlı bir şekilde servis edebilir.
* URL tabanlı içerik yönlendirme. Bu özellik, yük dengeleyicisinin hedef URL'ye bağlı olarak bağlantıları nereye ileteceğimiz konusunda karar vermesini mümkün kılar. Bu, IP adreslerine dayalı yük dengeleme kararları veren çözümlerden çok daha fazla esneklik sağlar.

Daha fazla bilgi edinin:

* [Application Gateway’e genel bakış](/azure/application-gateway/application-gateway-introduction)

### <a name="network-level-load-balancing"></a>Ağ seviyesi yük dengeleme

HTTP tabanlı yük dengelemenin aksine, ağ düzeyi yük dengelemesi IP adresi ve bağlantı noktası (TCP veya UDP) sayılarına göre kararlar verir.
Azure Yük Dengeleyici'ni kullanarak Azure'da ağ düzeyinde yük dengeleme avantajlarından yararlanabilirsiniz. Yük Dengeleyici'nin bazı temel özellikleri şunlardır:

* IP adresi ve bağlantı noktası numaralarına göre ağ düzeyi yük dengelemesi.
* Herhangi bir uygulama katmanı protokolü için destek.
* Bakiyeleri Azure sanal makinelerine ve bulut hizmetlerine yükleyin rol örnekleri.
* Hem internete dönük (harici yük dengeleme) hem de internete dönük olmayan (dahili yük dengeleme) uygulamaları ve sanal makineler için kullanılabilir.
* Yük dengeleyicisinin arkasındaki hizmetlerden herhangi birinin kullanılamamış olup olmadığını belirlemek için kullanılan uç nokta izleme.

Daha fazla bilgi edinin:

* [Birden çok sanal makine veya hizmet arasında Internet'e bakan yük dengeleyicisi](/azure/load-balancer/load-balancer-internet-overview)
* [Dahili yük dengeleyicisi genel bakış](/azure/load-balancer/load-balancer-internal-overview)

### <a name="global-load-balancing"></a>Küresel yük dengeleme

Bazı kuruluşlar mümkün olan en yüksek kullanılabilirlik düzeyini ister. Bu hedefe ulaşmanın bir yolu, uygulamaları genel olarak dağıtılmış veri merkezlerinde barındırmaktır. Bir uygulama tüm dünyada bulunan veri merkezlerinde barındırıldığında, tüm bir jeopolitik bölgenin kullanılamaması ve uygulamanın çalışmaya devam etmesi mümkündür.

Bu yük dengeleme stratejisi de performans avantajları sağlayabilir. Hizmet isteklerini, isteği yapan aygıta en yakın veri merkezine yönlendirebilirsiniz.

Azure'da, Azure Trafik Yöneticisi'ni kullanarak genel yük dengeleme avantajlarından yararlanabilirsiniz.

Daha fazla bilgi edinin:

* [Traffic Manager nedir?](../../traffic-manager/traffic-manager-overview.md)

## <a name="name-resolution"></a>Ad çözümlemesi

Ad çözümlemesi, Azure'da barındırdığınız tüm hizmetler için kritik bir işlevdir. Güvenlik açısından bakıldığında, ad çözümleme işlevinin tehlikeye atlanması, bir saldırganın sitelerinizden istekleri bir saldırganın sitesine yönlendirmesine neden olabilir. Güvenli ad çözümlemesi, tüm bulut barındırılan hizmetleriniz için bir gerekliliktir.

Adreslemeniz gereken iki tür ad çözümlemesi vardır:

* Dahili ad çözünürlüğü. Bu, sanal ağlarınızdaki hizmetler, şirket içi ağlarınız veya her ikisi de tarafından kullanılır. Dahili ad çözümlemesi için kullanılan adlar internet üzerinden erişilemez. En iyi güvenlik için, dahili ad çözümleme şemanınızın harici kullanıcılar tarafından erişilememesi önemlidir.
* Dış ad çözünürlüğü. Bu, şirket içi ağlarınız ve sanal ağlarınız dışındaki kişiler ve aygıtlar tarafından kullanılır. Bunlar, Internet tarafından görülebilen ve bulut tabanlı hizmetlerinize doğrudan bağlantı sağlamak için kullanılan adlardır.

Dahili ad çözümlemesi için iki seçeneğiniz vardır:

* Sanal ağ DNS sunucusu. Yeni bir sanal ağ oluşturduğunuzda, sizin için bir DNS sunucusu oluşturulur. Bu DNS sunucusu, bu sanal ağda bulunan makinelerin adlarını çözebilir. Bu DNS sunucusu yapılandırılamaz, Azure kumaş yöneticisi tarafından yönetilir ve bu nedenle ad çözümünüzü güvenli hale getirebilirsiniz.
* Kendi DNS sunucunuzu getirin. Sanal ağınızda kendi seçtiğiniz bir DNS sunucusu koyma seçeneğiniz vardır. Bu DNS sunucusu Active Directory tümleşik DNS sunucusu veya Azure İş Ortağı tarafından sağlanan ve Azure Marketi'nden edinebileceğiniz özel bir DNS sunucu çözümü olabilir.

Daha fazla bilgi edinin:

* [Sanal ağa genel bakış](../../virtual-network/virtual-networks-overview.md)
* [Sanal ağ tarafından kullanılan DNS Sunucularını yönetme](../../virtual-network/manage-virtual-network.md#change-dns-servers)

Dış ad çözümlemesi için iki seçeneğiniz vardır:

* Kendi harici DNS sunucunuzu şirket içinde barındırın.
* Kendi harici DNS sunucunuzu bir servis sağlayıcısıyla barındırın.

Birçok büyük kuruluş kendi DNS sunucularını şirket içinde barındırıyor. Bunu onlar bunu yapmak için ağ uzmanlık ve küresel varlığı var, çünkü bunu yapabilirsiniz.

Çoğu durumda, DNS ad çözümleme hizmetlerinizi bir hizmet sağlayıcısıyla barındırmak daha iyidir. Bu hizmet sağlayıcılar, ad çözümleme hizmetleriniz için çok yüksek kullanılabilirlik sağlamak için ağ uzmanlığına ve küresel varlığa sahiptir. Kullanılabilirlik DNS hizmetleri için çok önemlidir, çünkü adınız çözümleme hizmetleriniz başarısız olursa, hiç kimse internete bakan hizmetlere erişemez.

Azure, Azure DNS biçiminde yüksek oranda kullanılabilir ve yüksek performanslı harici DNS çözümü sağlar. Bu dış ad çözümlemesi çözümü, dünya çapındaki Azure DNS altyapısından yararlanır. Diğer Azure hizmetlerinizle aynı kimlik bilgilerini, API'leri, araçları ve faturalandırmayı kullanarak etki alanınızı Azure'da barındırmanızı sağlar. Azure'un bir parçası olarak, platformda yerleşik güçlü güvenlik denetimlerini de devralır.

Daha fazla bilgi edinin:

* [Azure DNS'ye genel bakış](../../dns/dns-overview.md)
* [Azure DNS özel bölgeleri,](../../dns/private-dns-overview.md) özel bir DNS çözümü eklemeye gerek kalmadan otomatik olarak atanan adlar yerine Azure kaynakları için özel DNS adlarını yapılandırmanıza olanak tanır.

## <a name="perimeter-network-architecture"></a>Çevre ağı mimarisi

Birçok büyük kuruluş, ağlarını segmente etmek ve internet ve hizmetleri arasında bir arabellek alanı oluşturmak için çevre ağlarını kullanır. Ağın çevre bölümü düşük güvenlikli bölge olarak kabul edilir ve bu ağ kesimine yüksek değerli varlıklar yerleştirilmez. Genellikle çevre ağ segmentinde ağ arabirimi olan ağ güvenlik aygıtlarını görürsünüz. Başka bir ağ arabirimi, internetten gelen bağlantıları kabul eden sanal makinelere ve hizmetlere sahip bir ağa bağlıdır.

Çevre ağlarını çeşitli şekillerde tasarlayabilirsiniz. Bir çevre ağı dağıtma kararı ve daha sonra bir kullanmaya karar verirseniz ne tür bir çevre ağı kullanmak için, ağ güvenlik gereksinimleribağlıdır.

Daha fazla bilgi edinin:

* [Microsoft Bulut Hizmetleri ve Ağ Güvenliği](network-best-practices.md)

## <a name="azure-ddos-protection"></a>Azure DDoS koruması

Dağıtılmış hizmet engelleme (DDoS) saldırıları, uygulamalarını buluta taşıyan müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunlarından biridir. Bir DDoS saldırısı, uygulamanın kaynaklarını tüketerek uygulamayı yasal kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir.
Microsoft, Azure Platformu'nun bir parçası olarak **Temel** olarak bilinen DDoS koruması sağlar. Bu ücretsiz olarak gelir ve her zaman izleme ve ortak ağ düzeyinde saldırıların gerçek zamanlı azaltma içerir. DDoS koruma **Temel** ile birlikte verilen korumalara ek olarak **Standart** seçeneğini etkinleştirebilirsiniz. DDoS Koruma Standardı özellikleri şunlardır:

* **Yerel platform entegrasyonu:** Azure'a yerel olarak entegre edilmiştir. Azure portalı üzerinden yapılandırma içerir. DDoS Protection Standard kaynaklarınızı ve kaynak yapılandırmanızı anlar.
* **Anahtar teslim koruması:** Basitleştirilmiş yapılandırma, DDoS Koruma Standardı etkinleştirilir etkinleştirilir etkinleştirmez sanal ağdaki tüm kaynakları hemen korur. Hiçbir müdahale veya kullanıcı tanımı gereklidir. DDoS Koruma Standardı, algılandıktan sonra saldırıyı anında ve otomatik olarak azaltır.
* **Her zaman trafiğe karşı izleme:** Uygulama trafik desenleri, DDoS saldırılarının göstergeleri için haftanın 7 günü, günde 24 saat izlenir. Koruma ilkeleri aşıldığında azaltma gerçekleştirilir.
* **Saldırı Azaltma Raporları** Saldırı Azaltma Raporları, kaynaklarınızı hedefleyen saldırılar hakkında ayrıntılı bilgi sağlamak için toplu ağ akışı verilerini kullanır.
* **Saldırı Azaltma Akış Günlükleri** Saldırı Azaltma Akış Günlükleri, etkin bir DDoS saldırısı sırasında bırakılan trafiği, iletilen trafiği ve diğer saldırı verilerini neredeyse gerçek zamanlı olarak gözden geçirmenize olanak sağlar.
* **Uyarlanabilir atokalma:** Akıllı trafik profiloluşturma, uygulamanızın trafiğini zaman içinde öğrenir ve hizmetiniz için en uygun profili seçer ve günceller. Trafik zaman içinde değiştikçe profil ayarlanır. Katman 3'den katman 7 korumasına kadar koruma: Bir web uygulaması güvenlik duvarıyla kullanıldığında tam yığın DDoS koruması sağlar.
* **Kapsamlı azaltma ölçeği:** Bilinen en büyük DDoS saldırılarına karşı korumak için küresel kapasiteye sahip 60'tan fazla farklı saldırı türü azaltılabilir.
* **Saldırı ölçümleri:** Her saldırının özetlenmiş ölçümlerine Azure Monitor üzerinden erişilebilir.
* **Saldırı uyarısı:** Uyarılar, bir saldırının başlangıcında ve sonunda ve saldırının süresi boyunca yerleşik saldırı ölçümleri kullanılarak yapılandırılabilir. Uyarılar, Microsoft Azure Monitor günlükleri, Splunk, Azure Depolama, E-posta ve Azure portalı gibi operasyonel yazılımınıza entegre olur.
* **Maliyet garantisi:**  Belgelenmiş DDoS saldırıları için veri aktarımı ve uygulama ölçeklendirme hizmet kredileri.
* **DDoS Hızlı duyarlı** DDoS Protection Standard müşterileri artık etkin bir saldırı sırasında Hızlı Yanıt ekibine erişebilir. DRR, saldırı soruşturması, saldırı sırasında özel azaltımlar ve saldırı sonrası analizler konusunda yardımcı olabilir.


Daha fazla bilgi edinin:

* [DDOS koruma genel bakış](../../virtual-network/ddos-protection-overview.md)

## <a name="azure-front-door"></a>Azure Front Door

Azure Ön Kapı Hizmeti, web trafiğinizin genel yönlendirmesini tanımlamanızı, yönetmenize ve izlemenize olanak tanır. En iyi performans ve yüksek kullanılabilirlik için trafik yönlendirmenizi optimize eder. Azure Front Door, HTTP/HTTPS iş yükünüzü istemci IP adreslerine, ülke koduna ve http parametrelerine dayalı istismardan korumaya yönelik erişim denetimi için özel web uygulaması güvenlik duvarı (WAF) kuralları yazmanıza olanak tanır. Ayrıca, Ön Kapı da kötü niyetli bot trafiği savaş için oran sınırlayıcı kurallar oluşturmanıza olanak sağlar, bu TLS boşaltma ve başına HTTP/HTTPS isteği, uygulama katmanı işleme içerir.

Front Door platformunun kendisi Temel Azure DDoS Koruması ile korunur. Korumayı artırmak için, sanal ağlarınızda Azure DDoS Koruması Standart etkinleştirilebilir ve kaynakları ağ katmanı (TCP/UDP) saldırılarına karşı otomatik ayar ve risk azaltma yoluyla koruma altına alır. Ön Kapı bir katman 7 ters proxy, sadece web trafiğinin arka uç sunuculara geçmesine ve varsayılan olarak trafik diğer türleri engellemek için izin verir.

Daha fazla bilgi edinin:

* Azure Ön Kapı özelliklerinin tüm seti hakkında daha fazla bilgi için [Azure Ön Kapı genel görünümünü](../../frontdoor/front-door-overview.md) inceleyebilirsiniz

## <a name="azure-traffic-manager"></a>Azure Trafik yöneticisi

Azure Traffic Manager, trafiği farklı Azure bölgelerindeki hizmetlere en uygun şekilde dağıtırken yüksek kullanılabilirlik ve yanıtlama hızı sağlayan DNS tabanlı bir trafik yük dengeleyicidir. Traffic Manager, trafik yönlendirme yöntemine ve uç noktaların sistem durumuna bağlı olarak istemci isteklerini en uygun hizmet uç noktasına yönlendirmek için DNS hizmetini kullanır. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir. Trafik yöneticisi bitiş noktalarını izler ve trafiği kullanılamayan uç noktalara yönlendirmez.

Daha fazla bilgi edinin:

* [Azure Trafik yöneticisine genel bakış](../../traffic-manager/traffic-manager-overview.md)

## <a name="monitoring-and-threat-detection"></a>İzleme ve tehdit algılama

Azure, ağ trafiğini erken algılama, izleme ve toplama ve gözden geçirme ile bu önemli alanda size yardımcı olacak özellikler sağlar.

### <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi

Azure Ağ İzleyicisi sorun gidermenize yardımcı olabilir ve güvenlik sorunlarının tanımlanmasına yardımcı olacak yepyeni bir araç kümesi sağlar.

[Güvenlik Grubu Görünümü,](../../network-watcher/network-watcher-security-group-view-overview.md) Sanal Makinelerin denetim ve güvenlik uyumluluğuna yardımcı olur. Kuruluşunuz tarafından tanımlanan temel ilkeleri, VM'lerinizin her biri için etkili kurallarla karşılaştırarak programlı denetimler gerçekleştirmek için bu özelliği kullanın. Bu, herhangi bir yapılandırma kayması belirlemenize yardımcı olabilir.

[Paket yakalama](../../network-watcher/network-watcher-packet-capture-overview.md) ve sanal makineden ağ trafiğini yakalamak için izin verir. Ağ istatistiklerini toplayabilir ve ağ ihlallerinin araştırılmasında paha biçilmez olabilecek uygulama sorunlarını giderebilirsiniz. Belirli Azure uyarılarına yanıt olarak ağ yakalamalarını başlatmak için bu özelliği Azure İşlevleriyle birlikte de kullanabilirsiniz.

Ağ İzleyicisi hakkında daha fazla bilgi ve laboratuvarlarınızdaki bazı işlevleri test etmeye nasıl başlayacağınız hakkında azure [ağ izleyicisi izleme genel bakış](../../network-watcher/network-watcher-monitoring-overview.md)bölümüne bakın.

> [!NOTE]
> Bu hizmetin kullanılabilirliği ve durumu yla ilgili en güncel bildirimler için [Azure güncelleştirmeleri sayfasını](https://azure.microsoft.com/updates/?product=network-watcher)kontrol edin.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, tehditleri önlemenize, algılamanıza ve yanıt vermenize yardımcı olur ve Azure kaynaklarınızın güvenliği nde daha fazla görünürlük ve denetim sağlar. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur ve büyük bir güvenlik çözümleri kümesiyle çalışır.

Güvenlik Merkezi, ağ güvenliğini şu nedenlere göre optimize edip izlemenize yardımcı olur:

* Ağ güvenliği önerileri sağlama.
* Ağ güvenliği yapılandırmanızın durumunu izleme.
* Ağ tabanlı tehditlere karşı sizi hem uç noktada hem de ağ düzeylerinde uyarır.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../../security-center/security-center-intro.md)

### <a name="virtual-network-tap"></a>Sanal Ağ TAP

Azure sanal ağ TAP (Terminal Erişim Noktası), sanal makine ağ trafiğinizi sürekli olarak bir ağ paketi toplayıcısına veya analiz aracına aktarmanızı sağlar. Toplayıcı veya analiz aracı bir ağ sanal cihaz ortağı tarafından sağlanır. Aynı veya farklı aboneliklerde birden çok ağ arabiriminden gelen trafiği toplamak için aynı sanal ağ TAP kaynağını kullanabilirsiniz.

Daha fazla bilgi edinin:

* [Sanal ağ TAP](../../virtual-network/virtual-network-tap-overview.md)

### <a name="logging"></a>Günlüğe kaydetme

Ağ düzeyinde günlüğe kaydetme, herhangi bir ağ güvenlik senaryosu için önemli bir işlevdir. Azure'da, ağ düzeyinde günlük bilgileri almak için NSG'ler için elde edilen bilgileri günlüğe kaydedebilirsiniz. NSG günlüğü ile şu bilgileri alırsınız:

* [Etkinlik günlükleri.](../../azure-monitor/platform/platform-logs-overview.md) Azure aboneliklerinize gönderilen tüm işlemleri görüntülemek için bu günlükleri kullanın. Bu günlükler varsayılan olarak etkinleştirilir ve Azure portalı içinde kullanılabilir. Bunlar daha önce denetim veya operasyonel günlükleri olarak biliniyordu.
* Olay günlükleri. Bu günlükler, NSG kurallarının uygulandığı hakkında bilgi sağlar.
* Sayaç günlükleri. Bu günlükler, trafiği reddetmek veya izin vermek için her NSG kuralının kaç kez uygulandığını size bildirin.

Bu günlükleri görüntülemek ve çözümlemek için güçlü bir veri görselleştirme aracı olan [Microsoft Power BI'yi](https://powerbi.microsoft.com/what-is-power-bi/)de kullanabilirsiniz.
Daha fazla bilgi edinin:

* [Ağ Güvenlik Grupları (NSG' ler) için Azure Monitör günlükleri](../../virtual-network/virtual-network-nsg-manage-log.md)
