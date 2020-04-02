---
title: Ağ güvenliği için en iyi uygulamalar - Microsoft Azure
description: Bu makalede, Azure özelliklerinde yerleşik olarak ağ güvenliği için en iyi uygulamalar kümesi sağlanmaktadır.
services: security
author: TerryLanfear
manager: barbkess
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/02/2019
ms.author: TomSh
ms.openlocfilehash: 5e155758d19b45d977fcd087bff0ceb85898f8f8
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548296"
---
# <a name="azure-best-practices-for-network-security"></a>Ağ güvenliği için Azure en iyi uygulamaları
Bu makalede, ağ güvenliğinizi artırmak için azure en iyi uygulamaları koleksiyonu anlatılmaktadır. Bu en iyi uygulamalar, Azure ağı yla ilgili deneyimlerimizden ve sizin gibi müşterilerin deneyimlerinden elde edilir.

Her en iyi uygulama için, bu makalede açıklar:

* En iyi uygulama nedir
* Neden en iyi uygulamayı etkinleştirmek istiyorsunuz?
* En iyi uygulamayı etkinleştiremezseniz sonuç ne olabilir?
* En iyi uygulamaya olası alternatifler
* En iyi uygulamayı etkinleştirmeyi nasıl öğrenebilirsiniz?

Bu en iyi uygulamalar, bu makalenin yazıldığı sırada mevcut olan bir fikir birliğine ve Azure platformu yeteneklerine ve özellik kümelerine dayanır. Görüşler ve teknolojiler zaman içinde değişir ve bu makale, bu değişiklikleri yansıtacak şekilde düzenli olarak güncellenir.

## <a name="use-strong-network-controls"></a>Güçlü ağ denetimleri kullanma
[Azure sanal makinelerini (VM'leri)](https://azure.microsoft.com/services/virtual-machines/) ve cihazları Azure sanal [ağlarına](../../virtual-network/index.yml)yerleştirerek diğer ağa bağlı aygıtlara bağlayabilirsiniz. Diğer bir süre, ağ özellikli aygıtlar arasında TCP/IP tabanlı iletişime izin vermek için sanal ağ arabirim kartlarını sanal ağa bağlayabilirsiniz. Bir Azure sanal ağına bağlı sanal makineler aynı sanal ağdaki aygıtlara, farklı sanal ağlara, internete veya kendi şirket içi ağlarınıza bağlanabilir.

Ağınızı ve ağınızın güvenliğini planlarken, aşağıdakileri merkezileştirmenizi öneririz:

- ExpressRoute, sanal ağ ve alt ağ sağlama ve IP adresi gibi temel ağ işlevlerinin yönetimi.
- ExpressRoute, sanal ağ ve alt ağ sağlama ve IP adresi gibi ağ sanal cihaz işlevleri gibi ağ güvenliği öğelerinin yönetimi.

Ağınızı ve ağınızın güvenliğini izlemek için ortak bir yönetim araçları kümesi kullanırsanız, her ikisini de net bir şekilde görünürlüğe duyarsınız. Basit ve birleşik bir güvenlik stratejisi, insan anlayışını ve otomasyonun güvenilirliğini artırdığı için hataları azaltır.

## <a name="logically-segment-subnets"></a>Mantıksal olarak segment alt ağları
Azure sanal ağları şirket içi ağınızdaki LAN'lere benzer. Bir Azure sanal ağının arkasındaki fikir, tüm Azure sanal makinelerinizi yerleştirebileceğiniz tek bir özel IP adresi alanına dayalı bir ağ oluşturmanızdır. Kullanılabilir özel IP adresi alanları A (10.0.0.0/8), B sınıfı (172.16.0.0/12) ve C sınıfı (192.168.0.0/16) aralıklarında dır.

Alt ağları mantıksal olarak bölümlere ayırmak için en iyi uygulamalar şunlardır:

**En iyi yöntem**: Geniş aralıklı izin kuralları atamayın (örneğin, 0.0.0.0 ile 255.255.255.255 arasında izin verin).  
**Ayrıntı**: Sorun giderme yordamlarının bu tür kuralların ayarlanmasının cesaretini kırdığından veya yasaklanmasını sağlayın. Bunlar, kuralların yanlış bir güvenlik duygusuna yol açmasına izin verir ve sık sık kırmızı ekipler tarafından bulunur ve sömürülür.

**En iyi uygulama**: Büyük adres alanını alt ağlara ayırın.   
**Ayrıntı**: Alt ağlarınızı oluşturmak için [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)tabanlı alt ağ oluşturma ilkelerini kullanın.

**En iyi yöntem**: Alt ağlar arasında ağ erişim denetimleri oluşturun. Alt ağlar arasında yönlendirme otomatik olarak gerçekleşir ve yönlendirme tablolarını el ile yapılandırmanız gerekmez. Varsayılan olarak, bir Azure sanal ağında oluşturduğunuz alt ağlar arasında ağ erişim denetimi yoktur.   
**Ayrıntı**: Azure alt ağlarına yönelik istenmeyen trafiğe karşı korumak için bir [ağ güvenlik grubu](/azure/virtual-network/virtual-networks-nsg) kullanın. Ağ güvenlik grupları, ağ trafiği için izin verme/reddetme kuralları oluşturmak için 5 tuple yaklaşımını (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve katman 4 protokolü) kullanan basit ve durumlu paket denetim aygıtlarıdır. Tek bir IP adresine, birden çok IP adresine veya tüm alt ağlara gelen ve gelen trafiğe izin verir veya reddedersiniz.

Alt ağlar arasındaki ağ erişimi denetimi için ağ güvenlik gruplarını kullandığınızda, aynı güvenlik bölgesine ait kaynakları veya rolü kendi alt ağlarına koyabilirsiniz.

**En iyi uygulama**: Basitlik ve esneklik sağlamak için küçük sanal ağlardan ve alt ağlardan kaçının.   
**Ayrıntı**: Çoğu kuruluş başlangıçta planlanandan daha fazla kaynak ekler ve adresleri yeniden tahsis etmek emek yoğundur. Küçük alt ağları kullanmak sınırlı güvenlik değeri ekler ve her alt ağa bir ağ güvenlik grubu eşleme ek yükü ekler. Büyüme için esnekliğe sahip olduğundan emin olmak için alt ağları geniş olarak tanımlayın.

**En iyi uygulama**: Uygulama Güvenlik Grupları tanımlayarak ağ güvenliği grubu kural yönetimini basitleştirin. [Application Security Groups](https://azure.microsoft.com/blog/applicationsecuritygroups/)  
**Ayrıntı**: Gelecekte değişebileceğini veya birçok ağ güvenlik grubunda kullanılabileceğini düşündüğünüz IP adreslerinin listeleri için bir Uygulama Güvenlik Grubu tanımlayın. Başkalarının içeriklerini ve amaçlarını anlayabilmeleri için Uygulama Güvenlik Grupları'nı açıkça adlandırdığından emin olun.

## <a name="adopt-a-zero-trust-approach"></a>Sıfır Güven yaklaşımını benimseme
Çevre tabanlı ağlar, ağ içindeki tüm sistemlere güvenilebileceği varsayımıyla çalışır. Ancak günümüzün çalışanları, çevre güvenlik denetimlerini alakasız kılan çeşitli aygıt ve uygulamalarda kuruluşlarının kaynaklarına her yerden erişiyor. Yalnızca kaynağa kimlerin erişebileceğine odaklanan denetim ilkelerine erişmek yeterli değildir. Güvenlik ve üretkenlik arasındaki dengeyi sağlamak için, güvenlik yöneticilerinin bir kaynağa *nasıl* erişildiğini de hesaba katması gerekir.

Ağlar ihlallere karşı savunmasız olabileceğinden, ağların geleneksel savunmalardan evrimgeçirmesi gerekir: saldırgan güvenilen sınır içinde tek bir uç noktayı tehlikeye atabilir ve ardından tüm ağ daki dayanak noktasını hızla genişletebilir. [Sıfır Güven](https://www.microsoft.com/security/blog/2018/06/14/building-zero-trust-networks-with-microsoft-365/) ağları, bir çevredeki ağ konumuna dayalı güven kavramını ortadan kaldırır. Bunun yerine, Zero Trust mimarileri aygıt ve kullanıcı güven iddiaları nın kuruluş verilerine ve kaynaklarına erişime geçit kullanmasını kullanır. Yeni girişimler için, erişim sırasında güveni doğrulayan Sıfır Güven yaklaşımlarını benimseyin.

En iyi uygulamalar şunlardır:

**En iyi uygulama**: Aygıt, kimlik, güvence, ağ konumu ve daha fazlasına dayalı kaynaklara Koşullu Erişim verin.  
**Ayrıntı**: [Azure AD Koşullu Erişim,](/azure/active-directory/conditional-access/overview) gerekli koşullara göre otomatik erişim denetimi kararları uygulayarak doğru erişim denetimlerini uygulamanızı sağlar. Daha fazla bilgi için bkz. [Koşullu Erişim ile Azure yönetimine erişimi yönet.](../../role-based-access-control/conditional-access-azure-management.md)

**En iyi yöntem**: Bağlantı noktası erişimini yalnızca iş akışı onayı ndan sonra etkinleştirin.  
**Ayrıntı**: Azure VM'lerinize gelen trafiği kilitlemek ve gerektiğinde VM'lere bağlanmak için kolay erişim sağlarken saldırılara maruz kalmayı azaltmak için [Azure Güvenlik Merkezi'nde tam zamanında VM erişimi](../../security-center/security-center-just-in-time.md) kullanabilirsiniz.

**En iyi uygulama**: Kötü amaçlı veya yetkisiz kullanıcıların izinlerin süresi dolduktan sonra erişim kazanmasını engelleyen ayrıcalıklı görevleri gerçekleştirmek için geçici izinler verir. Erişim yalnızca kullanıcıların ihtiyacı olduğunda verilir.  
**Ayrıntı**: Ayrıcalıklı görevleri gerçekleştirmek için izin vermek için Azure AD Ayrıcalıklı Kimlik Yönetimi'nde veya üçüncü taraf çözümünde tam zamanında erişim kullanın.

Zero Trust ağ güvenliğinde bir sonraki evrimdir. Siber saldırıların durumu, kuruluşları "ihlal" zihniyetini ele almaya itiyor, ancak bu yaklaşım sınırlayıcı olmamalı. Zero Trust ağları kurumsal verileri ve kaynakları korurken, kuruluşların çalışanları her zaman, her yerde ve her şekilde üretken olmalarını sağlayan teknolojileri kullanarak modern bir işyeri oluşturabilmelerini sağlar.

## <a name="control-routing-behavior"></a>Yönlendirme davranışını denetleme
Bir Azure sanal ağına sanal bir makine koyduğunuzda, diğer VM'ler farklı alt ağlarda olsa bile VM aynı sanal ağdaki diğer VM'lere bağlanabilir. Varsayılan olarak etkinleştirilen sistem yolları koleksiyonu bu tür bir iletişime izin verdiği için bu mümkündür. Bu varsayılan rotalar, aynı sanal ağdaki VM'lerin birbirleriyle ve internetle (yalnızca internete giden iletişimiçin) bağlantı başlatmasına olanak tanır.

Varsayılan sistem yolları birçok dağıtım senaryosu için yararlı olsa da, dağıtımlarınız için yönlendirme yapılandırmasını özelleştirmek istediğiniz zamanlar vardır. Sonraki atlama adresini belirli hedeflere ulaşmak için yapılandırabilirsiniz.

Sanal ağ için bir güvenlik cihazı dağıttığınızda [kullanıcı tanımlı rotaları](../../virtual-network/virtual-networks-udr-overview.md) yapılandırmanızı öneririz. Bu [konuyu, kritik Azure hizmet kaynaklarınızı yalnızca sanal ağlarınıza güvenli hale](network-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks)getirmek başlıklı daha sonraki bir bölümde konuşuyoruz.

> [!NOTE]
> Kullanıcı tanımlı yollar gerekli değildir ve varsayılan sistem yolları genellikle çalışır.
>
>

## <a name="use-virtual-network-appliances"></a>Sanal ağ cihazlarını kullanma
Ağ güvenlik grupları ve kullanıcı tanımlı yönlendirme, AĞ'da belirli bir ağ güvenliği ve [OSI modelinin](https://en.wikipedia.org/wiki/OSI_model)aktarım katmanları sağlayabilir. Ancak bazı durumlarda, yığının yüksek düzeylerinde güvenliği etkinleştirmek ister veya gerekir. Bu gibi durumlarda, Azure iş ortakları tarafından sağlanan sanal ağ güvenlik cihazlarını dağıtmanızı öneririz.

Azure ağ güvenlik cihazları, ağ düzeyindeki denetimlerin sağladığından daha iyi güvenlik sağlayabilir. Sanal ağ güvenlik cihazlarının ağ güvenliği özellikleri şunlardır:


* Güvenlik Duvarı
* Saldırı tespiti/izinsiz giriş önleme
* Güvenlik açığı yönetimi
* Uygulama denetimi
* Ağ tabanlı anomali tespiti
* Web filtreleme
* Virüsten Koruma
* Botnet koruması

Kullanılabilir Azure sanal ağ güvenlik cihazlarını bulmak için [Azure Marketi'ne](https://azure.microsoft.com/marketplace/) gidin ve "güvenlik" ve "ağ güvenliği" araması yapın.

## <a name="deploy-perimeter-networks-for-security-zones"></a>Güvenlik bölgeleri için çevre ağlarını dağıtma
[Çevre ağı](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) (DMZ olarak da bilinir), varlıklarınız ve internet arasında ek bir güvenlik katmanı sağlayan fiziksel veya mantıksal bir ağ kesimidir. Çevre ağının kenarındaki özel ağ erişim kontrol aygıtları, sanal ağınıza yalnızca istenen trafiğe izin verir.

Çevre ağları yararlıdır, çünkü Azure sanal ağınızın kenarındaki aygıtlarda ağ erişim denetim yönetimi, izleme, günlüğe kaydetme ve raporlama odaklanabilirsiniz. Çevre ağı, genellikle dağıtılmış hizmet reddi (DDoS) önleme, izinsiz giriş algılama/izinsiz giriş önleme sistemleri (IDS/IPS), güvenlik duvarı kuralları ve ilkeleri, web filtresi, ağ antimalware ve daha fazlasını etkinleştirdiğiniz yerdir. Ağ güvenlik aygıtları Internet ve Azure sanal ağınız arasında yer almakta ve her iki ağda da bir arayüze sahiptir.

Bu bir çevre ağının temel tasarımı olmasına rağmen, arka arkaya, üç ev ve çok ev gibi birçok farklı tasarımlar vardır.

Daha önce bahsedilen Sıfır Güven kavramına dayanarak, Azure kaynaklarınız için ağ güvenliği ve erişim denetimi düzeyini artırmak için tüm yüksek güvenlikli dağıtımlar için bir çevre ağı kullanmayı düşünmenizi öneririz. Varlıklarınız ve Internet arasında ek bir güvenlik katmanı sağlamak için Azure veya üçüncü taraf çözümünün kullanılması:

- Azure yerel denetimleri. [Azure Güvenlik Duvarı](/azure/firewall/overview) ve [Application Gateway'deki web uygulama güvenlik duvarı,](../../application-gateway/features.md#web-application-firewall) hizmet olarak tam olarak durum lu güvenlik duvarı, yerleşik yüksek kullanılabilirlik, sınırsız bulut ölçeklenebilirliği, FQDN filtreleme, OWASP temel kural kümeleri için destek ve basit kurulum ve yapılandırma ile temel güvenlik sunar.
- Üçüncü taraf teklifleri. Azure [Marketi'nde](https://azuremarketplace.microsoft.com/) yeni nesil güvenlik duvarı (NGFW) ve tanıdık güvenlik araçları ve önemli ölçüde gelişmiş ağ güvenliği düzeyleri sağlayan diğer üçüncü taraf teklifleri için arama yapın. Yapılandırma daha karmaşık olabilir, ancak üçüncü taraf teklifi varolan yetenekleri ve becerileri kullanmanıza izin verebilir.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Özel WAN bağlantılarıyla internete maruz kalmaktan kaçının
Birçok kuruluş karma BT rotasını seçti. Karma BT ile şirketin bazı bilgi varlıkları Azure'dadır ve diğerleri şirket içinde kalır. Çoğu durumda, bir hizmetin bazı bileşenleri Azure'da çalışırken, diğer bileşenler şirket içinde kalır.

Karma BT senaryosunda, genellikle bir tür binalar arası bağlantı vardır. Şirket içi bağlantı, şirketin şirket içi ağlarını Azure sanal ağlarına bağlamasına olanak tanır. İki tesisler arası bağlantı çözümü mevcuttur:

* [Siteden siteye VPN](../../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md). Bu güvenilir, güvenilir ve kurulmuş bir teknoloji, ancak bağlantı internet üzerinden gerçekleşir. Bant genişliği maksimum 1,25 Gbps ile sınırlandırılmıştır. Siteden siteye VPN bazı senaryolarda arzu edilen bir seçenektir.
* **Azure ExpressRoute**. ExpressRoute'u [ExpressRoute](../../expressroute/expressroute-introduction.md) tesisler arası bağlantınız için kullanmanızı öneririz. ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute ile Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetlerine bağlantılar kurabilirsiniz. ExpressRoute, şirket içi konumunuz la Microsoft Exchange barındırma sağlayıcısı arasında özel bir WAN bağlantısıdır. Bu bir telekomünikasyon bağlantısı olduğundan, verileriniz internet üzerinden seyahat etmez, bu nedenle internet iletişiminin potansiyel risklerine maruz değildir.

ExpressRoute bağlantınızın konumu güvenlik duvarı kapasitesini, ölçeklenebilirliği, güvenilirliği ve ağ trafiği görünürlüğünü etkileyebilir. Varolan (şirket içi) ağlarda ExpressRoute'u nerede sonlandıracağınızı belirlemeniz gerekir. Şunları yapabilirsiniz:

- Trafikte görünürlük gerekiyorsa, veri merkezlerini yalıtma uygulamanızı sürdürmeniz gerekiyorsa veya yalnızca Azure'a ekstranet kaynakları koyuyorsanız, güvenlik duvarının (çevre ağı paradigması) dışında sonlandırın.
- Güvenlik duvarı (ağ uzantısı paradigması) içinde sonlandırmak. Bu varsayılan öneridir. Diğer tüm durumlarda, Azure'u bir nth veri merkezi olarak ele almanızı öneririz.

## <a name="optimize-uptime-and-performance"></a>Çalışma süresini ve performansı optimize edin
Bir hizmet inip, bilgilere erişilemez. Performans, verilerin kullanılamaz hale gelir durumda olduğu kadar düşükse, verilerin erişilemez olduğunu düşünebilirsiniz. Güvenlik açısından bakıldığında, hizmetlerinizin en iyi çalışma süresi ne olursa olsun ve performansa sahip olduğundan emin olmak için her şeyi yapmanız gerekir.

Kullanılabilirliği ve performansı artırmak için popüler ve etkili bir yöntem yük dengelemedir. Yük dengeleme, ağ trafiğini hizmetin parçası olan sunucular arasında dağıtma yöntemidir. Örneğin, hizmetinizin bir parçası olarak ön uç web sunucularınız varsa, trafiği birden çok ön uç web sunucunuza dağıtmak için yük dengelemesini kullanabilirsiniz.

Bu trafik dağıtımı kullanılabilirliği artırır, çünkü web sunucularından biri kullanılamıyorsa, yük dengeleyicisi o sunucuya trafik göndermeyi durdurur ve onu hala çevrimiçi olan sunuculara yönlendirir. Hizmet istekleriiçin işlemci, ağ ve bellek ek yükü tüm yük dengeli sunucular arasında dağıtıldığı için yük dengeleme de performansa yardımcı olur.

Yükleme dengelemeyi istediğiniz zaman ve hizmetleriniz için uygun olduğunca kullanmanızı öneririz. Aşağıda, hem Azure sanal ağ düzeyinde hem de genel düzeydeki senaryoların yanı sıra her biri için yük dengeleme seçenekleri yer almaktadır.

**Senaryo**: Bir uygulama var ki:

- Aynı arka uç sanal makineye ulaşmak için aynı kullanıcı/istemci oturumundan istekler gerektirir. Buna örnek olarak alışveriş sepeti uygulamaları ve web posta sunucuları verilebilir.
- Yalnızca güvenli bir bağlantıyı kabul eder, bu nedenle sunucuya şifrelenmemiş iletişim kabul edilebilir bir seçenek değildir.
- Aynı uzun süreli TCP bağlantısında birden fazla HTTP isteğinin yönlendirilmesi veya farklı arka uç sunucularına yüklenmesi gerekir.

**Yük dengeleme seçeneği**: HTTP web trafik yük dengeleyicisi olan [Azure Application Gateway'i](/azure/application-gateway/application-gateway-introduction)kullanın. Uygulama Ağ Geçidi, ağ geçidinde uçtan uca TLS şifrelemesini ve [TLS sonlandırmayı](/azure/application-gateway/application-gateway-introduction) destekler. Web sunucuları daha sonra şifreleme ve şifre çözme yükü nden ve arka uç sunucularına şifrelenmemiş akan trafikten çıkarılabilir.

**Senaryo**: Bir Azure sanal ağında bulunan sunucularınız arasında internetten gelen bakiye bağlantılarını yüklemeniz gerekir. Senaryolar şunlardır:

- Internet'ten gelen istekleri kabul eden devletsiz uygulamalara sahip.
- Yapışkan oturumlar veya TLS boşaltma gerektirmez. Yapışkan oturumları sunucu afiyeti elde etmek için, Uygulama Yük Dengeleme ile kullanılan bir yöntemdir.

**Yük dengeleme seçeneği**: Daha yüksek bir kullanılabilirlik düzeyi sağlamak için gelen istekleri birden çok VM'ye yayan [harici bir yük dengeleyicisi oluşturmak](../../load-balancer/quickstart-load-balancer-standard-public-portal.md) için Azure portalını kullanın.

**Senaryo**: Internet'te olmayan VM'lerden bakiye bağlantıları yüklemeniz gerekir. Çoğu durumda, yük dengeleme için kabul edilen bağlantılar, SQL Server örnekleri veya dahili web sunucuları gibi bir Azure sanal ağındaki aygıtlar tarafından başlatılır.   
**Yük dengeleme seçeneği**: Daha yüksek bir kullanılabilirlik düzeyi sağlamak için gelen istekleri birden çok VM'ye yayan [bir dahili yük dengeleyicisi oluşturmak](../../load-balancer/quickstart-load-balancer-standard-public-portal.md) için Azure portalını kullanın.

**Senaryo**: Küresel yük dengelemesi gerekir, çünkü:

- Birden çok bölgede yaygın olarak dağıtılan ve mümkün olan en yüksek çalışma süresi (kullanılabilirlik) düzeyini gerektiren bir bulut çözümüne sahip olun.
- Tüm veri merkezi kullanılamamış olsa bile hizmetinizin kullanılabilir olduğundan emin olmak için mümkün olan en yüksek çalışma süresine ihtiyacınız vardır.

**Yük dengeleme seçeneği**: Azure Trafik Yöneticisi'ni kullanın. Trafik Yöneticisi, kullanıcının konumuna bağlı olarak hizmetlerinize bakiye bağlantıları yüklemenizi mümkün kılar.

Örneğin, kullanıcı HIZMETInize AB'den bir istekte bulunursa, bağlantı bir AB veri merkezinde bulunan hizmetlerinize yönlendirilir. Trafik Yöneticisi global yük dengelemesinin bu bölümü, en yakın veri merkezine bağlanmak uzaktaki veri merkezlerine bağlanmaktan daha hızlı olduğundan performansı artırmaya yardımcı olur.

## <a name="disable-rdpssh-access-to-virtual-machines"></a>SANAL makinelere RDP/SSH erişimini devre dışı bırakma
[Uzak Masaüstü Protokolü](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) ve Güvenli [Kabuk](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protokolünü kullanarak Azure sanal makinelerine ulaşmak mümkündür. Bu protokoller VM'lerin uzak konumlardan yönetilmesine olanak tanır ve veri merkezi bilgi işleminde standarttır.

Bu protokolleri internet üzerinden kullanmanın olası güvenlik sorunu, saldırganların Azure sanal makinelerine erişmek için [kaba kuvvet](https://en.wikipedia.org/wiki/Brute-force_attack) tekniklerini kullanabilmesidir. Saldırganlar erişim kazandıktan sonra VM'nizi başlangıç noktası olarak kullanıp sanal ağınızdaki diğer makinelerin gizliliğini bozabilir, hatta Azure dışındaki ağa bağlı cihazlara bile saldırabilir.

Azure sanal makinelerinize internetten doğrudan RDP ve SSH erişimini devre dışı bırakmanızı öneririz. Internet'ten doğrudan RDP ve SSH erişimi devre dışı bırakıldıktan sonra, uzaktan yönetim için bu VM'lere erişmek için kullanabileceğiniz başka seçeneklerde de vardır.

**Senaryo**: Tek bir kullanıcının internet üzerinden azure sanal ağına bağlanmasını etkinleştirin.   
**Seçenek**: [Noktadan siteye VPN](/azure/vpn-gateway/vpn-gateway-point-to-site-create) uzaktan erişim VPN istemcisi / sunucu bağlantısı için başka bir terimdir. Noktadan siteye bağlantı kurulduktan sonra, kullanıcı, kullanıcının noktadan siteye VPN üzerinden bağlandığı Azure sanal ağında bulunan tüm VM'lere bağlanmak için RDP veya SSH kullanabilir. Bu, kullanıcının bu VM'lere ulaşma yetkisine sahip olduğunu varsayar.

Kullanıcı bir VM'ye bağlanmadan önce iki kez kimlik doğrulaması yaptığı ndan, site noktasındavpn doğrudan RDP veya SSH bağlantılarından daha güvenlidir. İlk olarak, kullanıcının noktadan siteye VPN bağlantısını kurmak için kimlik doğrulaması (ve yetkili olması) gerekir. İkinci olarak, kullanıcının RDP veya SSH oturumunu kurmak için kimliğini doğrulaması (ve yetkilendirilmesi) gerekir.

**Senaryo**: Şirket içi ağınızdaki kullanıcıların Azure sanal ağınızdaki VM'lere bağlanmasını sağlar.   
**Seçenek**: [Siteden siteye VPN,](/azure/vpn-gateway/vpn-gateway-site-to-site-create) tüm ağı internet üzerinden başka bir ağa bağlar. Şirket içi ağınızı bir Azure sanal ağına bağlamak için siteden siteye VPN kullanabilirsiniz. Şirket içi ağınızdaki kullanıcılar, siteden siteye VPN bağlantısı üzerinden RDP veya SSH protokolünü kullanarak bağlanır. İnternet üzerinden doğrudan RDP veya SSH erişimine izin vermek zorunda değilsiniz.

**Senaryo**: Siteden siteye VPN'e benzer işlevsellik sağlamak için özel bir WAN bağlantısı kullanın.   
**Seçenek**: [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/)kullanın. Siteden siteye VPN'e benzer işlevsellik sağlar. Temel farklılıklar şunlardır:

- Özel WAN bağlantısı internetten geçmiyor.
- Özel WAN bağlantıları genellikle daha kararlı dır ve daha iyi performans gösterir.

## <a name="secure-your-critical-azure-service-resources-to-only-your-virtual-networks"></a>Kritik Azure hizmet kaynaklarınızı yalnızca sanal ağlarınızla güvence altına almak
Sanal ağ özel adres alanınızı ve sanal ağınızın kimliğini doğrudan bağlantı üzerinden Azure hizmetlerine genişletmek için sanal ağ hizmeti uç noktalarını kullanın. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Sanal aağınızdan Azure hizmetine kadar olan trafik her zaman Microsoft Azure omurga ağında kalır.

Hizmet uç noktaları aşağıdaki avantajları sağlar:

- **Azure hizmet kaynaklarınız için geliştirilmiş güvenlik**: Hizmet uç noktaları sayesinde, Azure hizmet kaynakları sanal ağınızla sınırlandırılarak güvenli hale getirilebilir. Hizmet kaynaklarını bir sanal ağ ile sınırlandırmak, ilgili kaynaklara genel İnternet erişimini tamamen kaldırıp yalnızca sanal ağınızdan gelen trafiğe izin vererek güvenliği artırır.
- **Sanal ağınızdan Azure servis trafiği için en uygun yönlendirme**: Sanal abunuzdaki internet trafiğini şirket içi ve/veya sanal cihazlarınız için zorlayan ve zorunlu tünelleme olarak bilinen tüm rotalar, Azure hizmet trafiğini internet trafiğiyle aynı rotayı almaya da zorlar. Hizmet uç noktaları Azure trafiği için en uygun rotayı sunar.

  Uç noktalar her zaman doğrudan sanal aağınızdan Azure omurga ağındaki hizmete servis trafiğini alır. Azure omurga ağındaki trafiği tutmak, servis trafiğini etkilemeden zorunlu tünel leme yoluyla sanal ağlarınızdan giden internet trafiğini denetlemeye ve izlemeye devam etmenizi sağlar. [Kullanıcı tanımlı rotalar ve zorunlu tünelleme](../../virtual-network/virtual-networks-udr-overview.md)hakkında daha fazla bilgi edinin.

- **Daha az yönetim yüküyle ayarlanması kolay**: Bir IP güvenlik duvarı aracılığıyla Azure kaynaklarını güvence altına almak için artık sanal ağlarınızda ayrılmış, herkese açık IP adreslerine ihtiyacınız yoktur. Hizmet uç noktalarını ayarlamak için herhangi bir NAT veya ağ geçidi cihazı gerekmez. Hizmet uç noktaları alt ağda tek tıklamayla yapılandırılabilir. Uç noktaları korumak için ek ek yükü yoktur.

Hizmet bitiş noktaları ve hizmet bitiş noktalarının kullanılabildiği Azure hizmetleri ve bölgeleri hakkında daha fazla bilgi edinmek için [Sanal ağ hizmeti bitiş noktalarına](../../virtual-network/virtual-network-service-endpoints-overview.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanabileceğiniz daha fazla güvenlik en iyi uygulamaları için Azure güvenlik en iyi uygulamaları ve [desenleri](best-practices-and-patterns.md) görün.
