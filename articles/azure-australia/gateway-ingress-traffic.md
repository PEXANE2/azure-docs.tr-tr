---
title: Azure Avustralya 'da giriş trafiğini denetleme
description: Güvenli Internet ağ geçitleri için Avustralya kamu gereksinimlerini karşılamak üzere Azure Avustralya 'da giriş trafiğini denetlemeye yönelik kılavuz
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 3885b9fa346047a50a49c7b2f9b96b6a8f95e51f
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779360"
---
# <a name="controlling-ingress-traffic-in-azure-australia"></a>Azure Avustralya 'da giriş trafiğini denetleme

ICT sistemlerini güvenli hale getirmenin temel bir öğesi, ağ trafiğini denetliyor. Trafik yalnızca sistemin uzlaşmaya karşı riski azaltmak için gerekli olan ile sınırlandırılmalıdır.

Bu kılavuz, gelen (giriş) ağ trafiğinin Azure 'da nasıl çalıştığı hakkında ayrıntılar ve internet 'e bağlı bir sistem için ağ güvenliği denetimleri uygulamaya yönelik öneriler sağlar.

Ağ denetimleri, Avustralya Cyber Güvenlik Merkezi (ACSC) tüketici kılavuzlarıyla ve ACSC 'nin bilgi güvenliği El Ile (ıSM) amacı ile hizalanır.

## <a name="requirements"></a>Gereksinimler

Uluslar için genel güvenlik gereksinimleri, ıSM içinde tanımlanmıştır. Ağ güvenliği uygulama kapsamındaki varlıkların yardımcı olması için ACSC, [ACSC korumasını yayımladı: Ağ kesimlemesi ve ayrımı uygulama ve](https://www.acsc.gov.au/publications/protect/network_segmentation_segregation.htm)bulut ortamlarında sistemlerin güvenliğini sağlamaya yardımcı olmak ve ACSC, [kiracılar için bulut bilgi işlem güvenliği](https://www.acsc.gov.au/publications/protect/Cloud_Computing_Security_for_Tenants.pdf)yayımlaması.

Bu kılavuzlar ağ güvenliği uygulama ve trafiği denetleme bağlamını özetler ve ağ tasarımı ve yapılandırması için pratik öneriler içerir.

Hizmet güven portalının Avustralya sayfasındaki [Microsoft Azure Kılavuzu kiracılar Için Microsoft bulut bilgi Işlem güvenliği](https://aka.ms/au-irap) , ACSC yayımlarındaki önerileri karşılamanıza olanak tanıyan belirli Microsoft teknolojilerini vurgular.

ACSC 'deki yayınlarda tanımlanan aşağıdaki temel gereksinimler, Azure 'da giriş trafiğinin denetlenmesi için önemlidir:

|Açıklama|Source|
|---|---|
|**Ana bilgisayar tabanlı güvenlik duvarları ve CSP 'nin ağ erişim denetimlerini kullanarak, gelen ve giden VM ağ bağlantısını yalnızca gerekli bağlantı noktaları/protokolleriyle sınırlamak için ağ kesimlemesini ve ayırmayı, örneğin n katmanlı mimariyi uygulayın.**| _Kiracılar için bulut bilgi Işlem_|
|Kiracının kullanılabilirlik gereksinimlerini karşılamak için kiracı (kiracının uzak kullanıcıları dahil) ve bulut hizmeti arasında **yeterli yüksek bant genişliği, düşük gecikme süresi ve güvenilir ağ bağlantısı uygulayın**  | _Kiracılar için bulut bilgi Işlem_|
|**Teknolojileri yalnızca ağ katmanından daha fazlasına uygulayın**. Her ana bilgisayar ve ağ, mümkün olduğunda, mümkün olduğunda, mümkün olduğunca, en düşük düzeyde yönetilebilecek şekilde bölünmeli ve ayrılmış olmalıdır. Çoğu durumda, uygulama katmanını içeren ve dahil olmak üzere veri bağlantısı katmanından kesimlere ayırma ve ayırma uygulanır; Ancak, hassas ortamlarda fiziksel yalıtım uygun olabilir. Ana bilgisayar tabanlı ve ağ genelinde ölçümler, tamamlayıcı bir biçimde dağıtılmalıdır ve merkezi olarak izlenir. Tek güvenlik ölçüsü yeterli olmadığından güvenlik duvarı veya güvenlik gereci kullanılması yeterli değildir. |_ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama_|
|**En az ayrıcalık Ilkelerini kullanın ve verilere erişme to verilere erişme bildirmek için gerekir**. Bir konağın, hizmetin veya ağın başka bir konak, hizmet veya ağla iletişim kurması gerekmiyorsa, buna izin verilmez. Bir konağın, hizmetin veya ağın belirli bağlantı noktalarını veya protokolleri kullanarak başka bir ana bilgisayar, hizmet veya ağ ile iletişim kurmasına ihtiyacı varsa, diğer tüm bağlantı noktaları veya protokoller devre dışı bırakılmalıdır. Bu ilkeleri bir ağ genelinde benimseme, kullanıcı ayrıcalıklarının en düşük düzeyde kullanımını tamamlar ve ortamın genel güvenlik duruşunu önemli ölçüde artırır. |_ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama_|
|**Ana bilgisayarları ve ağları, iş işlemlerine duyarlılık veya önem derecesine göre ayırın**. Ayrım, farklı güvenlik sınıflandırmaları, güvenlik etki alanları veya belirli konaklar veya ağlar için kullanılabilirlik/bütünlük gereksinimlerine bağlı olarak farklı donanımlar veya platformlar kullanılarak elde edilebilir. Özellikle, ayrı yönetim ağları ve hassas ortamlar için bant dışı yönetim ağlarını fiziksel olarak yalıtmayı düşünün. |_ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama_|
|**Tüm varlıklar tarafından tüm diğer varlıklara göre erişimi belirler, kimlik doğrular ve yetkilendirin**. Tüm kullanıcılar, konaklar ve hizmetler, kendilerine atanan görevlerini veya işlevlerini yapmak için gereken diğer Kullanıcı, ana bilgisayar ve hizmetlerle sınırlı erişime sahip olmalıdır. Tanımlayıcı, kimlik doğrulama ve yetkilendirme hizmetlerinin gücünü atlayan veya indirgeen eski veya yerel hizmetler devre dışı bırakılmalıdır ve kullanımları yakından izlenmelidir. |_ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama_|
|**Engelleme listesi yerine ağ trafiği listesine izin ver**' i uygulayın. Bilinen hatalı ağ trafiğine erişimi reddetmek (örneğin, belirli bir adresi veya hizmeti engellemek) yerine yalnızca bilinen iyi ağ trafiğine (yani, kimliği doğrulanmış ve yetkili) erişime izin verir. Bir üstün güvenlik ilkesinde sonuçları kara listeye eklemek ve potansiyel ağ yetkisiz kullanım olasılığını tespit etmek ve değerlendirmek için kuruluşların kapasitesini önemli ölçüde artırır. |_ACSC koruma: Ağ kesimlemesini ve ayırmayı uygulama_|
|

Bu makalede, Azure 'da hizmet olarak altyapı (IaaS) ve hizmet olarak platform (PaaS) kullanılarak Azure 'da dağıtılan sistemler için bu gereksinimlerin karşılanabileceği hakkında bilgi ve öneriler sağlanır. Azure 'da ağ trafiğini denetlemeyi tam olarak anlamak için [Azure Avustralya 'daki çıkış trafiğini denetleme](gateway-egress-traffic.md) makalesini de okumalısınız.

## <a name="architecture"></a>Mimari

Ağ güvenliği ve giriş trafiği denetimlerinin tasarımına veya uygulamasına dahil ediyorsanız, önce gelen ağ trafiğinin hem IaaS hem de PaaS genelinde Azure 'da nasıl çalıştığını anlamanız gerekir. Bu bölüm, ağ trafiğinin Azure 'da barındırılan bir kaynağa ulaşabileceği olası giriş noktalarına ve bu trafiği kısıtlamak ve denetlemek için kullanılabilen güvenlik denetimlerine ilişkin bir genel bakış sağlar. Bu bileşenlerin her biri, bu kılavuzun kalan bölümlerinde ayrıntılı olarak ele alınmıştır.

### <a name="architecture-components"></a>Mimari bileşenleri

Burada gösterilen mimari diyagram, ağ trafiğinin Azure 'da barındırılan bir hizmete bağlanmak için ulaşabileceği olası yolları gösterir. Bu bileşenler, giriş trafiği için sağladıkları işleve bağlı olarak Azure, IaaS giriş, PaaS giriş ve güvenlik denetimine bölünmüştür.

![Mimari](media/ingress-traffic.png)

### <a name="azure-components"></a>Azure bileşenleri

|Bileşen | Açıklama|
|---|---|
|**DDoS Koruması** | Dağıtılmış hizmet reddi (DDoS) saldırıları bir uygulamanın kaynaklarını tüketmeye çalışır ve uygulamayı meşru kullanıcılar için kullanılamaz hale getirir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir. Azure, Azure platformu üzerinden otomatik olarak DDoS koruması içerir ve daha ayrıntılı denetim için belirli uygulamalar için etkinleştirilebilen ek risk azaltma özellikleri sağlar.|
| **Traffic Manager** | Azure Traffic Manager, yüksek kullanılabilirlik ve yanıt hızı sağlarken trafiği en iyi şekilde Azure bölgelerindeki hizmetlere dağıtabilen bir etki alanı adı sistemi (DNS) tabanlı trafik yük dengeleyicidir. Traffic Manager, istemci isteklerini bir trafik yönlendirme yöntemine ve uç noktaların sistem durumuna göre en uygun uç noktaya yönlendirmek için DNS kullanır.|
| **ExpressRoute** | ExpressRoute, Microsoft bulut hizmetleri 'ni tüketmeye yönelik adanmış bir ağ bağlantısıdır. Bir bağlantı sağlayıcısı aracılığıyla sağlanır ve Internet üzerinden tipik bağlantılardan daha fazla güvenilirlik, daha hızlı hız, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar. ExpressRoute devresi, bir bağlantı sağlayıcısı aracılığıyla şirket içi altyapı ve Microsoft bulut hizmetleri arasındaki mantıksal bağlantıyı temsil eder.|
| **ExpressRoute özel eşlemesi** | ExpressRoute özel eşlemesi, şirket içi ortam ve özel Azure sanal ağları arasında bir bağlantıdır. Özel eşleme, bir sanal ağ içinde dağıtılan sanal makineler gibi Azure hizmetlerine erişim sağlar. Özel eşleme aracılığıyla erişilen kaynaklar ve sanal ağlar, bir kuruluşun çekirdek ağının uzantısı olarak kabul edilir. Özel eşleme, şirket içi ağ ve Azure sanal ağları arasında özel IP adresleri kullanarak çift yönlü bağlantı sağlar.|
| **ExpressRoute Microsoft eşlemesi** | ExpressRoute Microsoft eşlemesi, şirket içi ortam ile Microsoft ile Azure genel hizmetleri arasında bir bağlantıdır. Bu, Office 365, Dynamics 365 ve Azure PaaS hizmetlerine bağlantı içerir. Eşleme, organizasyon veya bağlantı sağlayıcısına ait olan genel IP adresleri üzerinden oluşturulur. Varsayılan olarak ExpressRoute Microsoft eşlemesi aracılığıyla hiçbir hizmete erişilemeyecek ve bir kuruluşun gerekli hizmetleri kabul etmelidir. Bu işlem daha sonra Internet 'te bulunan uç noktalara bağlantı sağlar.|
|

### <a name="iaas-ingress-components"></a>IaaS giriş bileşenleri

|Bileşen | Açıklama|
|---|---|
|**Ağ arabirimi** | Ağ arabirimi, Azure 'da bulunan bir kaynaktır. Bir sanal makineye iliştirilir ve ilişkilendirildiği alt ağdan özel, Internet 'e ait olmayan bir IP adresi atanır. Bu IP adresi, Azure Resource Manager aracılığıyla dinamik veya statik olarak atanır.|
|**Alt ağ** | Bir alt ağ, VNet içinde oluşturulan bir IP adresi aralığıdır. Ağ kesimlemesi için bir VNet içinde birden çok alt ağ oluşturulabilir.|
| **Sanal ağ (VNet)** | VNet, Azure 'da kaynak dağıtmaya ve iletişimi etkinleştirmeye yönelik bir platform ve sınır sağlayan temel bir kaynaktır. VNet bir Azure bölgesi içinde bulunur ve sanal makineler gibi VNet tümleşik kaynakları için IP adresi alanını ve yönlendirme sınırlarını tanımlar.|
| **Sanal Ağ Eşlemesi** | VNet eşlemesi, bir sanal ağ geçidine gerek olmadan iki VNET arasında doğrudan iletişim sağlayan bir Azure yapılandırma seçeneğidir. Eşlendikten sonra iki VNET doğrudan iletişim kurabilir ve ek yapılandırma, sanal ağ geçitlerinin ve diğer aktarım seçeneklerinin kullanımını denetleyebilir.|
| **Genel IP** | Genel IP, Microsoft 'un sahip olduğu genel, Internet yönlendirilebilir IP adreslerinden sanal ağ içinde kullanılmak üzere belirli bir bölgeden ayrılmış bir kaynaktır. Bu, belirli bir ağ arabirimiyle ilişkilendirilebilir ve bu da kaynağa Internet, ExpressRoute ve PaaS sistemlerinden erişilebilmesine olanak sağlar.|
| **ExpressRoute ağ geçidi** | ExpressRoute ağ geçidi, bir sanal ağdaki bir nesnedir ve sanal ağdan şirket içi ağlara bir ExpressRoute devresi üzerinde özel eşleme üzerinden bağlantı ve yönlendirme sağlar.|
| **VPN Gateway** | VPN Gateway bir sanal ağ içindeki, bir sanal ağdan dış ağa şifreli bir tünel sağlayan bir nesnedir. Şifrelenmiş tünel, tek bir uç noktasıyla iletişim için şirket içi bir ortamla, diğer sanal ağ veya bulut ortamıyla veya Noktadan siteye kadar çift yönlü iletişim için siteden siteye olabilir.|
| **PaaS VNet tümleştirmesi** | Birçok PaaS özelliği bir sanal ağla birlikte dağıtılabilir veya ile tümleştirilebilir. Bazı PaaS özellikleri bir sanal ağ ile tamamen tümleştirilebilir ve yalnızca özel IP adresleri üzerinden erişilebilir. Azure Load Balancer ve Azure Application Gateway gibi diğer bir deyişle, genel IP adresi olan bir dış arabirime ve sanal ağ içinde özel IP adresi olan bir iç arabirime sahip olabilir. Bu örnekte, trafik PaaS özelliği aracılığıyla sanal ağa giriş yapabilir.|
|

### <a name="paas-ingress-components"></a>PaaS giriş bileşenleri

|Bileşen | Açıklama|
|---|---|
|**Ana Bilgisayar Adı** | Azure PaaS özellikleri, kaynak oluşturulduğunda atanan benzersiz bir genel ana bilgisayar adı tarafından tanımlanır. Bu konak adı daha sonra genel bir IP adresine çözümlenebileceği ortak bir DNS etki alanına kaydedilir.|
|**Genel IP** | VNet tümleşik yapılandırmasında dağıtılmadığı sürece Azure PaaS özelliklerine genel, Internet yönlendirilebilir bir IP adresi üzerinden erişilir. Bu adres, genel Load Balancer gibi belirli kaynaklara ayrılabilir veya depolama ya da SQL gibi birden çok örnek için paylaşılan bir giriş noktası olan belirli bir yetenek ile ilişkilendirilebilir. Bu genel IP 'ye Internet, ExpressRoute veya Azure omurga ağı aracılığıyla IaaS genel IP adreslerinden erişilebilir.|
|**Hizmet uç noktaları** | Hizmet uç noktaları, bir sanal ağdan belirli bir PaaS özelliğine doğrudan, özel bir bağlantı sağlar. PaaS yeteneklerinin yalnızca bir alt kümesi için kullanılabilen hizmet uç noktaları, PaaS 'ye erişen VNet 'teki kaynaklar için daha yüksek performans ve güvenlik sağlar.|
|

### <a name="security-controls"></a>Güvenlik denetimleri

|Bileşen | Açıklama|
|---|---|
|**Ağ güvenlik grupları (NSG 'ler)** | NSG 'ler, Azure 'daki sanal ağ kaynaklarının içine ve dışına trafiği denetler. NSG 'ler, Azure ve şirket içi ya da Internet gibi dış ağlar arasındaki trafiği de içeren, izin verilen veya reddedilen trafik akışları için kurallar uygular. NSG 'ler bir sanal ağ içindeki alt ağlara veya tek tek ağ arabirimlerine uygulanır.|
|**PaaS güvenlik duvarı** | Depolama ve SQL gibi birçok PaaS özelliği, belirli bir kaynağa gelen ağ trafiğini denetlemeye yönelik yerleşik bir güvenlik duvarına sahiptir. Belirli IP adreslerinden ve/veya sanal ağlardan gelen bağlantılara izin vermek veya bu bağlantıları reddetmek için kurallar oluşturulabilir.|
|**PaaS kimlik doğrulaması ve Access Control** | Güvenlik için katmanlı yaklaşımın bir parçası olarak PaaS özellikleri, kullanıcıların kimliğini doğrulamak ve ayrıcalıkları ve erişimi denetlemek için birden çok mekanizma sağlar.|
|**Azure İlkesi** | Azure Ilkesi, ilkeleri oluşturmak, atamak ve yönetmek için Azure 'da bir hizmettir. Bu ilkeler, dağıtılabilecek kaynak türlerini ve bu kaynakların yapılandırmasını denetlemek için kuralları kullanır. İlkeler, kaynakların gereksinimleri karşılamadığında veya uyumluluk durumunu raporlamak üzere izlenmesi için kullanılabilecekleri şekilde dağıtılmasını önleyecek şekilde uyumluluk sağlamak için kullanılabilir.|
|

## <a name="general-guidance"></a>Genel rehberlik

Azure 'da güvenli çözümler tasarlamak ve derlemek için, ağ trafiğini yalnızca tanımlı ve yetkili iletişimin gerçekleşebilmesi için anlamak ve denetlemek önemlidir. Bu kılavuzun amacı ve sonraki bölümlerde yer alan belirli bileşen Kılavuzu, _ACSC korumasında özetlenen ilkeleri uygulamak için kullanılabilecek araçları ve Hizmetleri anlatmaktadır: Azure iş yükleri_ arasında ağ kesimlemesini ve ayırmayı uygulama. Bu, bir şirket içi ortamda mümkün olan geleneksel fiziksel ve ağ denetimlerinin uygulanması mümkün olmadığında kaynakları güvenli hale getirmek için bir sanal mimari oluşturma hakkında ayrıntılı bilgiler içerir.

### <a name="specific-focus-areas"></a>Belirli odak alanı

* Giriş noktalarının sayısını sanal ağlarla sınırlayın
* Genel IP adresi sayısını sınırlayın
* Microsoft Virtual Data Center (VDC) belgelerinde anlatıldığı gibi sanal ağlar için bir hub ve bağlı bileşen ağ tasarımını kullanmayı düşünün
* Internet 'ten gelen bağlantılar için yerleşik güvenlik özelliklerine sahip ürünler (örneğin, Application Gateway, API Gateway, ağ sanal cihazları)
* İletişim akışlarını PaaS özelliklerine kısıtlama yalnızca sistem işlevselliği için gerekli olanlar
* Daha fazla ayrım ve denetim için VNet tümleşik yapılandırmasında PaaS dağıtma
* Sistemleri, ACSC tüketici kılavuzumuzu ve ıSM ile birlikte şifreleme mekanizmalarını kullanacak şekilde yapılandırın
* Geleneksel ağ denetimlerine ek olarak kimlik doğrulaması ve rol tabanlı erişim denetimi gibi kimlik tabanlı korumaları kullanın
* Şirket içi ağlarla bağlantı için ExpressRoute uygulama
* Yönetim trafiği için VPN uygulama ve dış ağlarla tümleştirme
* Bölgeleri ve kaynakları yalnızca sistem işlevselliği için gerekli olanlarla kısıtlamak için Azure Ilkesi 'ni kullan
* İnternet erişimli kaynaklar için temel güvenlik yapılandırmasını zorlamak üzere Azure Ilkesini kullan

### <a name="additional-resources"></a>Ek kaynaklar

|Resource | Bağlantı|
|---|---|
|Tüketici kılavuzu dahil olmak üzere Avustralya mevzuata ve Ilke uyumluluk belgeleri|[https://aka.ms/au-irap](https://aka.ms/au-irap)|
|Azure sanal veri merkezi|[https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter)|
|ACSC ağ kesimlemesi|[https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm](https://acsc.gov.au/publications/protect/network_segmentation_segregation.htm)|
|Kiracılar için ACSC Bulut güvenliği| [https://acsc.gov.au/publications/protect/cloud-security-tenants.htm](https://acsc.gov.au/publications/protect/cloud-security-tenants.htm)|
|ACSC Information Security el kitabı|[https://acsc.gov.au/infosec/ism/index.htm](https://acsc.gov.au/infosec/ism/index.htm)|

## <a name="component-guidance"></a>Bileşen Kılavuzu

Bu bölümde, Azure 'da dağıtılan sistemlere giriş yapmak için uygun olan tek tek bileşenler hakkında daha fazla rehberlik sunulmaktadır. Her bölümde, tasarım ve oluşturma etkinliklerine yardımcı olmak üzere kullanılabilecek belge ve yapılandırma kılavuzlarının bağlantıları ile ilgili bir bileşen amacı açıklanmaktadır.

## <a name="azure"></a>Azure

Azure 'daki kaynaklara yapılan tüm iletişimler, bağlantı ve güvenlik işlevselliği sağlayan Microsoft tarafından tutulan ağ altyapısı aracılığıyla geçer. Azure platformunu ve ağ altyapısını korumak için Microsoft tarafından otomatik olarak bir dizi koruma sağlanır ve ağ trafiğini denetlemek ve ağ kesimlenmesi ve ağ kesimlemesi oluşturmak için Azure 'da hizmetler olarak kullanılabilir ayrımı.

### <a name="ddos-protection"></a>DDOS Koruması

Internet erişimli kaynaklar, DDoS saldırılarına açıktır. Azure, bu saldırılara karşı korunmak için temel ve standart düzeyde DDoS korumaları sağlar.

Temel, her zaman açık trafik izleme, genel ağ düzeyi saldırıları ve gerçek zamanlı risk azaltma dahil olmak üzere Azure platformunun bir parçası olarak otomatik olarak etkinleştirilir ve bu da Microsoft 'un çevrimiçi hizmetler aynı erteçlerini sağlar. Azure 'un genel ağının tüm ölçeklendirilmesi, bölgeler arasında saldırı trafiğini dağıtmak ve azaltmak için kullanılabilir. IPv4 ve IPv6 Azure genel IP adresleri için koruma sağlanır

Standart, Azure sanal ağ kaynakları için özel olarak ayarlanan temel hizmet katmanı üzerinden ek risk azaltma özellikleri sağlar. Koruma ilkeleri adanmış trafik izleme ve makine öğrenimi algoritmalarıyla ayarlanır. IPv4 Azure genel IP adresleri için koruma sağlanır.

|Resource|Bağlantı|
|---|---|
|Azure DDoS korumasına genel bakış|[https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)|
|Azure DDoS En Iyi uygulamaları|[https://docs.microsoft.com/azure/security/fundamentals/ddos-best-practices](https://docs.microsoft.com/azure/security/fundamentals/ddos-best-practices)|
|DDoS korumasını yönetme|[https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)|
|

### <a name="traffic-manager"></a>Traffic Manager

Traffic Manager, bir uygulamanın hangi uç noktalarının bağlantı alacağını denetleyerek giriş trafiğini yönetmek için kullanılır. Iber güvenlik saldırısından kaynaklanan sistemlerin veya uygulamaların kullanılabilirliğine karşı koruma sağlamak ya da sistem güvenliğinin aşılmasına neden olan hizmetleri kurtarmak için Traffic Manager, trafiği çalışır ve kullanılabilir uygulama örneklerine yönlendirmek için kullanılabilir.

|Resource|Bağlantı|
|---|---|
|Traffic Manager’a Genel Bakış | [https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)|
|Azure DNS ve Traffic Manager kılavuzunu kullanarak olağanüstü durum kurtarma | [https://docs.microsoft.com/azure/networking/disaster-recovery-dns-traffic-manager](https://docs.microsoft.com/azure/networking/disaster-recovery-dns-traffic-manager)|
|

### <a name="expressroute"></a>ExpressRoute

ExpressRoute, şirket içi bir ortamdan Azure 'da barındırılan sistemlere özel bir yol oluşturmak için kullanılabilir. Bu bağlantı, ağ iletişimleri için gelişmiş gizliliğe sahip daha fazla güvenilirlik ve garantili performans sağlayabilir. Express Route, bir şirket içi ortamdan gelen trafiği denetlemesine ve gelen güvenlik duvarı kuralları ve erişim denetimi listelerinde kullanılacak kuruluşa özgü özel adresler tanımlamanızı sağlar.

|Resource | Bağlantı|
|---|---|
|ExpressRoute'a genel bakış | [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)|
|ExpressRoute bağlantı modelleri | [https://docs.microsoft.com/azure/expressroute/expressroute-connectivity-models](https://docs.microsoft.com/azure/expressroute/expressroute-connectivity-models)|
|

### <a name="expressroute-private-peering"></a>ExpressRoute özel eşlemesi

Özel eşleme, yalnızca özel IP adresleri kullanarak şirket içi bir ortamı Azure 'a genişletmeye yönelik bir mekanizma sağlar. Bu, Azure sanal ağlarını ve adres aralıklarını mevcut şirket içi sistemlerle ve hizmetlerle tümleştirmenize olanak sağlar. Özel eşleme, ExpressRoute üzerinden iletişimin yalnızca kuruluş tarafından yetki verilen sanal ağlara yönelik olduğunu güvence altına almaktadır. Özel eşleme kullanırsanız, kuruluşunuzun şirket içi ağlarınızla ACSC tüketici Kılavuzu tarafından istenen güvenli VPN iletişimini sağlamak için Azure VPN Gateway yerine ağ sanal gereçlerini (NVA) uygulaması gerekir.

|Resource | Bağlantı|
|---|---|
|ExpressRoute özel eşlemesine genel bakış | [https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains)|
|ExpressRoute özel eşleme nasıl yapılır Kılavuzu | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private](https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private)|
|

### <a name="expressroute-microsoft-peering"></a>ExpressRoute Microsoft eşlemesi

Microsoft eşleme, Internet 'e çapraz geçiş gerektirmeden Microsoft genel hizmetlerine yönelik yüksek hızlı, düşük gecikme süresine sahip bir bağlantı sağlar. Bu, bağlantılar için daha fazla güvenilirlik, performans ve gizlilik sağlar. Yönlendirme filtrelerini kullanarak, kurumlar, iletişimleri yalnızca istedikleri Azure bölgeleriyle kısıtlayabilir, ancak bu, diğer kuruluşlar tarafından barındırılan hizmetleri de içerir ve bu durum, Şirket içi ortam ve Microsoft.

Uluslar arası varlıklar, güvenlik duvarları ve PaaS özellikleri içindeki erişim denetimi listelerinde kullanılmak üzere şirket içi ortamı benzersiz şekilde tanımlamak için eşleme ilişkisi aracılığıyla sağlanan adanmış genel IP adreslerini kullanabilir.

Diğer bir deyişle, bir diğer seçenek olarak, Azure VPN Gateway aracılığıyla VPN bağlantısı kurmak için Microsoft 'un bir alt düzenleme ağı olarak ExpressRoute 'u kullanabilir. Bu modelde, şirket içi şirket ağından ExpressRoute üzerinden Azure genel hizmetlerine etkin bir iletişim yoktur, ancak ACSC tüketici kılavuzlarıyla uyumluluk sırasında özel sanal ağlara güvenli bağlantı sağlanır.

|Resource | Bağlantı|
|---|---|
|ExpressRoute Microsoft eşlemesine genel bakış | [https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#routingdomains)|
|ExpressRoute Microsoft eşleme nasıl yapılır Kılavuzu | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#msft](https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#msft)|
|

## <a name="iaas-ingress"></a>IaaS girişi

Bu bölümde, ınress trafiğini IaaS bileşenlerine denetlemek için bileşen Kılavuzu sağlanmaktadır. IaaS, sanal makineler ve Azure Ağa gelen Azure 'da dağıtılabilecek ve yönetilebilen diğer işlem kaynaklarını içerir. Trafiğin IaaS kullanılarak dağıtılan sistemlere gelmesi için, bir genel IP adresi, sanal ağ geçidi veya sanal ağ eşleme ilişkisiyle kurulabilen sanal ağa bir giriş noktası olması gerekir.

### <a name="network-interface"></a>Ağ arabirimi

Ağ arabirimleri, bir sanal makineye giden tüm trafiğin giriş noktalarıdır. Ağ arabirimleri IP adresleme yapılandırmasını etkinleştirir ve NSG 'ler uygulamak veya trafiği bir ağ sanal gereci üzerinden yönlendirmek için kullanılabilir. Sanal makineler için ağ arabirimleri, genel ağ kesimlemesi ve ayırma amaçları ile hizalamak için planlanmış ve uygun şekilde yapılandırılmalıdır.

|Resource | Bağlantı|
|---|---|
|Ağ arabirimi oluşturma, değiştirme veya silme | [https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface)|
|Ağ arabirimi IP adresleme | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)|
|

### <a name="subnet"></a>Subnet

Alt ağlar, Azure 'da ağ kesimlenmesi ve ayırma için önemli bir bileşendir. Alt ağlar, sistemler arasında ayrım sağlamak için benzer şekilde kullanılabilir. NSG 'ler, giriş iletişim akışlarını yalnızca sistem işlevselliği için gerekli olanlarla kısıtlamak üzere alt ağlara uygulanabilir. Alt ağlar, güvenlik duvarı kuralları ve erişim denetimi listeleri için hem kaynak hem de hedef adresler olarak kullanılabilir ve PaaS özelliklerine bağlantı sağlamak üzere hizmet uç noktaları için yapılandırılabilir.

|Resource | Bağlantı|
|---|---|
|Sanal ağ alt ağını ekleme, değiştirme veya silme | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)|
|

### <a name="virtual-network-vnet"></a>Sanal Ağ (VNet)

VNET 'ler, Azure 'da ağ için temel yapı taşlarından biridir. Sanal ağlar, çeşitli sistemler genelinde kullanılacak bir IP adresi alanı ve yönlendirme sınırı tanımlar. Sanal ağlar alt ağlara bölünür ve bir sanal ağ içindeki tüm alt ağların birbirlerine doğrudan bir ağ rotası vardır. Sanal ağ geçitleri (ExpressRoute veya VPN) kullanarak, bir sanal ağ içindeki sistemler şirket içi ve dış ortamlar tarafından erişilebilir hale getirilebilir. Sanal ağları ve ilişkili yapılandırma parametrelerini ve yönlendirmeyi anlamak, gelen ağ trafiğini anlamak ve denetlemek için önemlidir.

|Resource | Bağlantı|
|---|---|
|Sanal ağlara genel bakış | [https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)|
|Sanal ağlar için nasıl yapılır Kılavuzu planlayın | [https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)|
Sanal ağ hızlı başlangıcı oluşturma | [https://docs.microsoft.com/azure/virtual-network/quick-create-portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)|
|

### <a name="vnet-peering"></a>VNet Eşlemesi

VNet eşlemesi iki sanal ağ arasında doğrudan iletişim yolu sağlamak için kullanılır. Eşleme kurulduktan sonra, bir sanal ağdaki ana bilgisayarların doğrudan başka bir sanal ağdaki konaklara yüksek hızlı yönlendirme yolu vardır. NSG 'ler hala trafik için geçerlidir normal ve gelişmiş yapılandırma parametreleri, sanal ağ geçitleri veya diğer dış sistemlerden iletişim yapılmasına izin verilip verilmeyeceğini belirlemek için kullanılabilir.

|Resource | Bağlantı|
|---|---|
|Sanal ağ eşlemesine genel bakış |  [https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)|
|Sanal ağ eşlemesi oluşturma, değiştirme veya silme | [https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering)|
|

### <a name="public-ip-on-vnet"></a>VNET üzerinde genel IP

Ortak IP adresleri, bir sanal ağda dağıtılan hizmetlere giriş iletişim yolu sağlamak için kullanılır. Kurumlar varlıkları, genel IP adreslerinin ayırmayı dikkatlice planlıyor ve yalnızca orijinal bir gereksinim olduğu kaynaklara atamalıdır. Genel bir tasarım uygulaması olarak, ortak IP adresleri, bir sanal ağa güvenli, denetimli bir ortak giriş noktası sağlamak için Application Gateway veya ağ sanal gereçleri gibi yerleşik güvenlik özelliklerine sahip kaynaklara ayrılmalıdır.

|Resource | Bağlantı|
|---|---|
|Genel IP Adresleri genel bakış | [https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)|
|Genel IP adresi oluşturma, değiştirme veya silme | [https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)|
|

### <a name="expressroute-gateway"></a>ExpressRoute ağ geçidi

ExpressRoute ağ geçitleri, şirket içi ortamdan bir giriş noktası sağlar ve güvenlik, kullanılabilirlik, finansal ve performans gereksinimlerini karşılamak için dağıtılmalıdır. ExpressRoute ağ geçitleri, tanımlı bir ağ bant genişliği sağlar ve dağıtımdan sonra kullanım maliyetleri doğurur. Sanal ağlarda yalnızca bir ExpressRoute ağ geçidi olabilir, ancak bu birden fazla ExpressRoute devresine bağlanabilir ve VNet eşlemesi aracılığıyla birden çok sanal ağ tarafından yararlanılabilir olabilir ve birden çok sanal ağın bant genişliği ve bağlantı paylaşmasına izin verir. Şirket içi ortamlar ve sanal ağlar arasında ExpressRoute ağ geçitlerini kullanarak yönlendirmeyi yapılandırmak için, bilinen, denetlenen ağ giriş noktalarını kullanarak uçtan uca bağlantı sağlamak için dikkatli olunmalıdır. ExpressRoute ağ geçidini kullanan kurumlar varlıklarının, ACSC tüketici kılavuzuna uyum sağlamak üzere şirket içi ortama VPN bağlantısı kurmak için ağ sanal gereçlerini de dağıtması gerekir.

|Resource | Bağlantı|
|---|---|
|ExpressRoute Gateway 'e genel bakış | [https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)|
|ExpressRoute için sanal ağ geçidi yapılandırma | [https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager)|
|

### <a name="vpn-gateway"></a>VPN Gateway

Azure VPN Gateway, güvenli siteden siteye veya Noktadan siteye bağlantılar için bir dış ağdan bir giriş ağ noktası sağlar. VPN ağ geçitleri, tanımlı bir ağ bant genişliği sağlar ve dağıtımdan sonra kullanım maliyetlerini doğurur. VPN Gateway kullanan kurumlar varlıkları ACSC tüketici kılavuzlarıyla uyumlu olarak yapılandırıldığından emin olmalıdır. Sanal ağlarda yalnızca bir VPN Gateway olabilir, ancak bu birden çok tünelle yapılandırılabilir ve VNet eşlemesi aracılığıyla birden çok sanal ağ tarafından yararlanılabilir olabilir ve birden çok sanal ağın bant genişliği ve bağlantı paylaşmasına izin verir. VPN ağ geçitleri Internet üzerinden veya Microsoft eşlemesi üzerinden ExpressRoute üzerinden kurulabilir.

|Resource | Bağlantı|
|---|---|
|VPN Gateway genel bakış | [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)|
|VPN Gateway için planlama ve tasarım | [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)|
|Avustralya kamu kurumları için VPN Gateway yapılandırması|[Avustralya kamu kurumları için ıPSEC yapılandırması gerekir](vpn-gateway.md)|
|

### <a name="paas-vnet-integration"></a>PaaS VNet tümleştirmesi

PaaS 'nin kullanılması, gelişmiş işlevsellik ve kullanılabilirlik sağlayabilir ve yönetim yükünü azaltabilir, ancak güvenli bir şekilde güvende olmalıdır. Denetimi artırmak, ağ segmentlerini zorlamak veya uygulamalar ve hizmetler için güvenli bir giriş girişi noktası sağlamak için, birçok PaaS özelliği bir sanal ağ ile tümleştirilebilir.

Güvenli bir giriş noktası sağlamak için, Application Gateway gibi PaaS özellikleri, bir dış, genel kullanıma yönelik arabirim ve uygulama hizmetleriyle iletişim kurmak için dahili, özel bir arabirim ile yapılandırılabilir. Bu, uygulama sunucularını genel IP adresleriyle yapılandırma ve bunları dış ağlarda kullanıma sunma gereksinimini ortadan önler.

PaaS 'yi sistem veya uygulama mimarisinin tümleşik bir parçası olarak kullanmak için, Microsoft, PaaS 'yi bir sanal ağa dağıtmak için birden çok mekanizma sağlar. Dağıtım metodolojisi, iç sistemler ve uygulamalarla bağlantı sağlarken Internet gibi dış ağlardan gelen erişimi kısıtlar. App Service ortamları, SQL tarafından yönetilen örnekleri ve daha fazlasını içeren örnekler.

|Resource | Bağlantı|
|---|---|
|Azure hizmetleri için sanal ağ tümleştirmesi | [https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)|
|Uygulamanızı bir Azure sanal ağı ile tümleştirme nasıl yapılır Kılavuzu | [https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)|
|

## <a name="paas-ingress"></a>PaaS girişi

PaaS özellikleri, daha fazla yetenek ve basitleştirilmiş yönetim olanakları sağlar, ancak ağ kesimlemesi ve ayrımı için adres gereksinimlerinde karmaşıklıklar sunar. PaaS özellikleri genellikle genel IP adresleriyle yapılandırılır ve Internet 'ten erişilebilir.  PaaS yeteneklerini kullanarak sistem oluştururken, yalnızca bu iletişime izin vermek üzere oluşturulan sistem ve ağ güvenlik kuralları içindeki bileşenler arasındaki tüm gerekli iletişim akışlarını belirlemek için dikkatli olunması gerekir. Güvenlik konusunda derinlemesine savunma yaklaşımı kapsamında, PaaS özellikleri şifreleme, kimlik doğrulama ve uygun erişim denetimleri ve izinlerle yapılandırılmalıdır.  

### <a name="hostname"></a>Konak adı

PaaS özellikleri, aynı hizmetin birden çok örneğinin aynı genel IP adresinde barındırılmasına olanak tanımak için konak adları tarafından benzersiz şekilde tanımlanır. Benzersiz ana bilgisayar adları, kaynaklar oluşturulduğunda ve Microsoft 'un sahip olduğu DNS etki alanlarında mevcutsa belirtilir. Yetkili hizmetler için belirli ana bilgisayar adları, uygulama düzeyinde filtreleme özelliklerine sahip güvenlik araçları dahilinde kullanılabilir. Belirli hizmetler, gereken şekilde özel etki alanlarıyla de yapılandırılabilir.

|Resource | Bağlantı|
|---|---|
|Azure hizmetleri tarafından kullanılan birçok genel ad alanı, Get-AzureRMEnvironment komutu çalıştırılarak PowerShell aracılığıyla alınabilir. | [https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermenvironment](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermenvironment)|
|Azure bulut hizmeti için özel etki alanı adı yapılandırma | Uygulama hizmetleri ve başkalarının özel etki alanları olabilir[https://docs.microsoft.com/azure/cloud-services/cloud-services-custom-domain-name-portal](https://docs.microsoft.com/azure/cloud-services/cloud-services-custom-domain-name-portal)|
|

### <a name="public-ip-for-paas"></a>PaaS için genel IP

PaaS özellikleri için genel IP adresleri, hizmetin barındırıldığı veya dağıtıldığı bölgeye göre ayrılır. Ağ kesimlemeye ve Azure sanal ağlarını, PaaS ve ExpressRoute ile Internet bağlantısını kapsayan uygun ağ güvenliği kuralları ve yönlendirme topolojisi oluşturmak için, genel IP adresi ayırmayı ve bölgelerini anlama Gerekli. Azure, IP adreslerini her bir Azure bölgesine ayrılmış bir havuzdan ayırır. Microsoft, her bölgede kullanılan, düzenli ve denetimli bir şekilde güncelleştirilmiş olan adreslerin indirilmesini sağlar. Yeni hizmetler serbest bırakıldığında veya hizmetler daha yaygın olarak dağıtıldığında, her bölgede kullanılabilen hizmetler de sık değişir. Kurumlar varlıklarının bu malzemeleri düzenli olarak incelemesi gerekir ve gerektiğinde sistemleri sürdürmek için Otomasyon özelliğinden yararlanabilir. Her bölgede barındırılan bazı hizmetlere yönelik belirli IP adresleri, Microsoft desteğiyle iletişim kurarak elde edilebilir.

|Resource | Bağlantı|
|---|---|
|Microsoft Azure veri merkezi IP aralıkları | [https://www.microsoft.com/download/details.aspx?id=41653](https://www.microsoft.com/download/details.aspx?id=41653)|
|Bölge başına Azure hizmetleri | [https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional Avustralya-orta, Avustralya-Orta-2, Avustralya-Doğu, Avustralya-Güneydoğu & Ürünler = tümü](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast&products=all)|
|

### <a name="service-endpoints"></a>Hizmet uç noktaları

Sanal ağ hizmeti uç noktaları, belirli PaaS yeteneklerini tüketmek için bir sanal ağ içindeki alt ağlara yönelik yüksek hızlı, özel bir giriş ağı bağlantısı sağlar. PaaS özelliğinin tüm ağ kesimlenmesi ve ayrımı için, PaaS özelliği yalnızca gerekli sanal ağlardan gelen bağlantıları kabul edecek şekilde yapılandırılmalıdır. Tüm PaaS özellikleri, hizmet uç noktalarını ve geleneksel IP adresi tabanlı kuralları içeren Güvenlik Duvarı kurallarının bir birleşimini desteklemez, bu nedenle uygulama işlevselliği ve yönetimi için gereken iletişimin akışını anlamak için dikkatli olunmalıdır Bu güvenlik denetimlerinin uygulanması, hizmet kullanılabilirliğini etkilemez.

|Resource | Bağlantı|
|---|---|
|Hizmet uç noktalarına genel bakış | [https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)
|Öğretici |[https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)|
|

## <a name="security"></a>Güvenlik

IaaS ve PaaS özelliklerine ağ kesimleme ve ayırma denetimleri uygulamak, yeteneklerin kendilerine güvenilerek ve ile iletişim kuran sistemlerden denetimli iletişim yollarını uygulayarak elde edilir. yapma.

Azure 'da çözümler tasarlama ve oluşturma, tüm Azure varlığı genelinde ağ kaynaklarını anlamak, denetlemek ve izlemek için bir mantıksal mimari oluşturma işlemidir. Bu mantıksal mimari, Azure platformunda tanımlanan yazılımdır ve geleneksel ağ ortamlarında uygulanan bir fiziksel ağ topolojisinin yerini alır.

Oluşturulan mantıksal mimari, kullanılabilirlik için gereken işlevselliği sağlamalıdır, ancak güvenlik ve bütünlük için gereken görünürlük ve denetimi de sağlamalıdır.

Bu sonucun sağlanması, gerekli ağ kesimlemesi ve ayırma araçlarının uygulanmasına ve ayrıca ağ topolojisini koruma ve zorunlu tutma ve bu araçların uygulanmasını temel alır.

Bu kılavuzda sağlanan bilgiler, izin verilen giriş trafiği kaynaklarının ve trafiğin daha fazla denetlenebileceği ya da kısıtlanmasına yol göstermeye yardımcı olmak için kullanılabilir.

### <a name="network-security-groups-nsgs"></a>Ağ güvenlik grupları (NSG 'ler)

NSG 'ler, bir alt ağ veya belirli bir ağ arabirimi için izin verilen gelen ve giden trafiği belirtmek için kullanılır. NSG 'leri yapılandırırken, bir izin verme bildirimiyle eşleşmeyen tüm trafiği reddedecek şekilde yapılandırılmış bir varsayılan kuralla izin vermek üzere kuralların yapılandırıldığı bir beyaz listeleme yaklaşımı kullanmalıdır. Tüm gerekli gelen ve giden trafiğin uygun şekilde yakalandığından emin olmak için NSG 'ler planlarken ve yapılandırılırken dikkatli olunmalıdır. Bu, Azure sanal ağları ve şirket içi ortamda kullanılan tüm özel IP adresi aralıklarını tanımlama ve anlama ve Azure Load Balancer ve PaaS yönetimi gereksinimleri gibi belirli Microsoft hizmetleri içerir. Ağ güvenlik gruplarının tasarımına ve uygulanmasına dahil kişiler, ayrıntılı, hizmet ve uygulamaya özel güvenlik kuralları oluşturmak için hizmet etiketlerinin ve uygulama güvenlik gruplarının kullanımını da anlamalıdır.

|Resource | Bağlantı|
|---|---|
|Ağ güvenliğine genel bakış | [https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)
|Ağ güvenlik grubu oluşturma, değiştirme veya silme | [https://docs.microsoft.com/azure/virtual-network/manage-network-security-group](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)|
|

## <a name="paas-firewall"></a>PaaS güvenlik duvarı

PaaS güvenlik duvarı, belirli PaaS hizmetlerine uygulanabilen bir ağ erişim denetimi özelliğidir. Belirli sanal ağlardan gelen IP adresi filtrelemesinin veya filtrelemesinin belirli bir PaaS örneğine giriş trafiğini kısıtlamak üzere yapılandırılmasına izin verir. Bir güvenlik duvarı içeren PaaS özellikleri için, ağ erişim denetimi ilkeleri, uygulama gereksinimlerine göre yalnızca gerekli giriş trafiğine izin verecek şekilde yapılandırılmalıdır.  

|Resource | Bağlantı|
|---|---|
|Azure SQL veritabanı ve SQL veri ambarı IP güvenlik duvarı kuralları | [https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)|
|Depolama ağı güvenliği | [https://docs.microsoft.com/azure/storage/common/storage-network-security](https://docs.microsoft.com/azure/storage/common/storage-network-security)|
|

## <a name="paas-authentication-and-access-control"></a>PaaS kimlik doğrulaması ve erişim denetimi

PaaS özelliğine ve amacına bağlı olarak, erişimi kısıtlamak için ağ denetimlerinin kullanılması mümkün olmayabilir veya pratik olmayabilir. PaaS için katmanlı güvenlik modelinin bir parçası olarak Azure, ağ trafiğine izin verilse bile bir hizmete erişimi kısıtlamak için çeşitli kimlik doğrulama ve erişim denetimi mekanizmaları sağlar. PaaS özelliklerine yönelik tipik kimlik doğrulama mekanizmaları Azure Active Directory, uygulama düzeyi kimlik doğrulaması ve paylaşılan anahtarlar ya da erişim imzaları içerir. Bir Kullanıcı güvenli bir şekilde tanımlandıktan sonra, kullanıcıların gerçekleştirebileceği eylemleri denetlemek için roller kullanılabilir. Bu araçlar, hizmetlere erişimi kısıtlamak için alternatif olarak veya bir tamamlayıcı ölçü olarak kullanılabilir.

|Resource | Bağlantı|
|---|---|
|SQL veritabanı ve SQL veri ambarı 'na veritabanı erişimini denetleme ve verme | [https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins)|
|Azure depolama hizmetleri için yetkilendirme | [https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-Azure-Storage-Services](https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-Azure-Storage-Services)|
|

## <a name="azure-policy"></a>Azure İlkesi

Azure Ilkesi, Azure ortamının mantıksal mimarisinin bütünlüğünü zorlamaya ve korumaya yönelik bir temel bileşendir. Azure hizmetleri aracılığıyla kullanılabilen çeşitli hizmetler ve giriş ağ trafiği yolları göz önüne alındığında, kurumlar varlıklarının ortamlarında mevcut olan kaynakların ve kullanılabilir ağ giriş noktalarında farkında olması önemlidir. Yetkili olmayan ağ giriş noktalarının Azure ortamında oluşturulmadığından emin olmak için, dağılabilecek kaynak türlerini ve bu kaynakların yapılandırmasını denetlemek için, uluslar arasında Azure Ilkesi kullanılması gerekir. Pratik örnekler, kaynakları yalnızca yetkili ve kullanım açısından onaylanan, depolama üzerinde HTTPS şifrelemesini uygulayan ve alt ağlara eklenen NSG 'lerin kullanılmasını içerir.

|Resource | Bağlantı|
|---|---|
|Azure Ilkesine genel bakış | [https://docs.microsoft.com/azure/governance/policy/overview](https://docs.microsoft.com/azure/governance/policy/overview)|
|İzin verilen kaynak türleri örnek ilkesi | [https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types](https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types)
|HTTPS depolama hesabı örnek ilkesini doğrulayın|[https://docs.microsoft.com/azure/governance/policy/samples/ensure-https-storage-account](https://docs.microsoft.com/azure/governance/policy/samples/ensure-https-storage-account)_
|Alt ağ örnek ilkesinde NSG 'yi zorla| [https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet](https://docs.microsoft.com/azure/governance/policy/samples/nsg-on-subnet)|
|

## <a name="next-steps"></a>Sonraki adımlar

Azure ortamınızdaki ağ geçidi bileşenlerinizi kullanarak Azure ortamınızdan diğer ağlara trafik akışlarını yönetme hakkında ayrıntılı bilgi için [ağ geçidi çıkış trafiği yönetimi ve denetimindeki](gateway-egress-traffic.md) makaleyi gözden geçirin.
