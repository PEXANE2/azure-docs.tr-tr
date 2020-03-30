---
title: Azure DDoS Koruması ile esnek çözümler tasarlama
description: Uygulamanız hakkında derin bilgiler edinmek için günlük verilerini nasıl kullanabileceğiniz hakkında bilgi edinin.
services: security
author: barclayn
manager: RKarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2018
ms.author: barclayn
ms.openlocfilehash: 8d3fc809999508bf3d49c3765c90017e89e80fa7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624046"
---
# <a name="azure-ddos-protection---designing-resilient-solutions"></a>Azure DDoS Koruması - Esnek çözümler tasarlama

Bu makale, BT karar vericileri ve güvenlik personeli içindir. Azure, ağ ve güvenlik hakkında bilginiz olmasını bekler.
DDoS, uygulama kaynaklarını tüketmeye çalışan bir saldırı türüdür. Amaç, uygulamanın kullanılabilirliğini ve yasal istekleri işleme yeteneğini etkilemektir. Saldırılar boyut ve etki olarak daha sofistike ve daha büyük hale gelmektedir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir. Dağıtılmış hizmet reddi (DDoS) esnekliği için tasarım, çeşitli hata modları için planlama ve tasarım gerektirir. Azure, DDoS saldırılarına karşı sürekli koruma sağlar. Bu koruma varsayılan olarak azure platformuna entegre edilmiştir ve ek ücret ödemeden.

[Azure DDoS Koruma Standardı,](https://azure.microsoft.com/services/ddos-protection/) platformdaki temel DDoS korumasına ek olarak ağ saldırılarına karşı gelişmiş DDoS azaltma özellikleri sağlar. Belirli Azure kaynaklarınızı korumak için otomatik olarak ayarlanır. Yeni sanal ağların oluşturulması sırasında güvenliği sağlamak kolaydır. Ayrıca oluşturulduktan sonra yapılabilir ve hiçbir uygulama veya kaynak değişikliği gerektirir.

![Azure DDoS Koruması'nın müşterileri ve sanal ağı bir saldırgandan korumadaki rolü](./media/ddos-best-practices/image1.png)


## <a name="fundamental-best-practices"></a>Temel en iyi uygulamalar

Aşağıdaki bölümler, Azure'da DDoS'a dayanıklı hizmetler oluşturmak için açıklayıcı kılavuz sağlar.

### <a name="design-for-security"></a>Güvenlik için tasarlama

Tasarımın ve uygulamanın dağıtım ve işlemlerine kadar, bir uygulamanın tüm kullanım döngüsü boyunca güvenliğin bir öncelik olduğundan emin olun. Uygulamalar, dereceye kadar düşük sayıda istek kullanarak bir hizmet kesintisine neden olan hataları olabilir. 

Microsoft Azure'da çalışan bir hizmetin korunmasına yardımcı olmak için, uygulama mimarinizi iyi anlamalı ve [yazılım kalitesinin beş ayağına](/azure/architecture/guide/pillars)odaklanmalısınız.
Tipik trafik hacimlerini, uygulama ve diğer uygulamalar arasındaki bağlantı modelini ve genel internete maruz kalan hizmet bitiş noktalarını bilmeniz gerekir.

Bir uygulamanın, uygulamanın kendisini hedefleyen bir hizmet reddini işleyecek kadar esnek olmasını sağlamak en önemlidir. Güvenlik ve gizlilik, Güvenlik Geliştirme Yaşam [Döngüsü (SDL)](https://www.microsoft.com/sdl/default.aspx)ile başlayan Azure platformunda yerleşiktir. SDL, her geliştirme aşamasında güvenliğe hitap eder ve Azure'un güvenliği daha da güvenli hale getirmek için sürekli olarak güncelleştirilmesini sağlar.

### <a name="design-for-scalability"></a>Ölçeklenebilirlik için tasarım

Ölçeklenebilirlik, bir sistemin artan yükü ne kadar iyi idare edebileceğidir. Özellikle bir DDoS saldırısı durumunda, yükseltilmiş bir yük talebini karşılamak için uygulamalarınızı [yatay olarak ölçeklendirecek](/azure/architecture/guide/design-principles/scale-out) şekilde tasarla. Uygulamanız bir hizmetin tek bir örneğine bağlıysa, tek bir hata noktası oluşturur. Birden çok örnek sağlama, sisteminizi daha esnek ve daha ölçeklenebilir hale getirir.

[Azure Uygulama Hizmeti](/azure/app-service/app-service-value-prop-what-is)için, birden çok örnek sunan bir Uygulama Hizmeti [planı](/azure/app-service/overview-hosting-plans) seçin. Azure Bulut Hizmetleri için, rollerinizi [birden çok örnek](/azure/cloud-services/cloud-services-choose-me)kullanacak şekilde yapılandırın. [Azure Sanal Makineler](/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)için, sanal makine (VM) mimarinizin birden fazla VM içerdiğinden ve her VM'nin bir [kullanılabilirlik kümesine](/azure/virtual-machines/virtual-machines-windows-manage-availability)dahil olduğundan emin olun. Otomatik ölçeklendirme özellikleri için [sanal makine ölçeği kümeleri](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) kullanmanızı öneririz.

### <a name="defense-in-depth"></a>Derinlemesine savunma

Derinlemesine savunma arkasındaki fikir çeşitli savunma stratejileri kullanarak riski yönetmektir. Bir uygulamada güvenlik savunmasını katmanlama, başarılı bir saldırı olasılığını azaltır. Azure platformunun yerleşik özelliklerini kullanarak uygulamalarınız için güvenli tasarımlar uygulamanızı öneririz.

Örneğin, uygulamanın boyutu *(yüzey alanı)* ile saldırı riski artar. Açıkta kalan IP adres alanını kapatmak için beyaz liste kullanarak yüzey alanını ve yük dengeleyicilerinde[(Azure Yük Dengeleyicisi](/azure/load-balancer/load-balancer-get-started-internet-portal) ve [Azure Uygulama Ağ Geçidi)](/azure/application-gateway/application-gateway-create-probe-portal)gerekli olmayan dinleme bağlantı noktalarını kullanarak yüzey alanını azaltabilirsiniz. [Ağ güvenlik grupları (NSG'ler),](/azure/virtual-network/security-overview) saldırı yüzeyini azaltmanın başka bir yoludur.
Bir uygulamayapısının doğal bir uzantısı olarak, güvenlik kuralları oluşturmak ve ağ güvenliğini yapılandırmak için karmaşıklığı en aza indirmek için [hizmet etiketleri](/azure/virtual-network/security-overview#service-tags) ve [uygulama güvenlik grupları](/azure/virtual-network/security-overview#application-security-groups) kullanabilirsiniz.

Azure hizmetlerini mümkün olduğunca [sanal ağda](/azure/virtual-network/virtual-networks-overview) dağıtmanız gerekir. Bu uygulama, hizmet kaynaklarının özel IP adresleri üzerinden iletişim kurmasına olanak tanır. Sanal ağdaki Azure hizmet trafiği, varsayılan olarak genel IP adreslerini kaynak IP adresleri olarak kullanır. [Hizmet bitiş noktalarını](/azure/virtual-network/virtual-network-service-endpoints-overview) kullanmak, Sanal ağdan Azure hizmetine erişirken kaynak IP adresleri olarak sanal ağ özel adreslerini kullanmak için servis trafiğine geçiş yapacaktır.

Azure'daki kaynaklarıyla birlikte müşterilerin şirket içi kaynaklarının saldırıya uğradığını sık sık görüyoruz. Şirket içi bir ortamı Azure'a bağlıyorsanız, şirket içi kaynakların genel internete maruz kalmasını en aza indirmenizi öneririz. Tanınmış kamu kuruluşlarınızı Azure'da dağıtarak Azure'un ölçek ve gelişmiş DDoS koruma özelliklerini kullanabilirsiniz. Bu genel olarak erişilebilen bu varlıklar genellikle DDoS saldırıları için bir hedef olduğundan, bunları Azure'a koymak şirket içi kaynaklarınız üzerindeki etkisini azaltır.

## <a name="azure-offerings-for-ddos-protection"></a>DDoS koruması için Azure teklifleri

Azure'da ağ saldırılarına karşı koruma sağlayan iki DDoS hizmet teklifi vardır (Katman 3 ve 4): DDoS Protection Basic ve DDoS Protection Standard. 

### <a name="ddos-protection-basic"></a>DDoS Koruma Temel

Temel koruma, ek ücret ödemeden varsayılan olarak Azure'a entegre edilir. Küresel olarak dağıtılan Azure ağının ölçeği ve kapasitesi, her zaman çevrimiçi trafik izleme ve gerçek zamanlı azaltma yoluyla yaygın ağ katmanı saldırılarına karşı savunma sağlar. DDoS Protection Basic kullanıcı yapılandırması veya uygulama değişikliği gerektirmez. DDoS Protection Basic, Azure DNS gibi PaaS hizmetleri de dahil olmak üzere tüm Azure hizmetlerinin korunmasına yardımcı olur.

!["Global DDoS azaltma varlığı" ve "Lider DDoS azaltma kapasitesi" metniyle Azure ağının harita gösterimi](./media/ddos-best-practices/image3.png)

Azure'daki temel DDoS koruması hem yazılım hem de donanım bileşenlerinden oluşur. Bir yazılım kontrol uçağı, saldırı trafiğini analiz eden ve kaldıran donanım cihazlarından ne zaman, nerede ve ne tür bir trafik yönlendirilmesi gerektiğine karar verir. Kontrol uçağı bu kararı altyapı çapında bir DDoS Koruma *politikasına*göre verir. Bu ilke statik olarak ayarlanır ve tüm Azure müşterileri için evrensel olarak uygulanır.

Örneğin, DDoS Koruma ilkesi, korumanın hangi trafik hacminin *tetikleneceğini belirtir.* (Diğer bir şekilde, kiracının trafiği ovma aletleri ile yönlendirilmelidir.) İlke daha sonra ovma cihazlarının saldırıyı nasıl *azaltması* gerektiğini belirtir.

Azure DDoS Protection Basic hizmeti, Azure platformunun altyapısının ve korumasının korunmasını hedeflemektedir. Çok kiracılı bir ortamda birden çok müşteriyi etkileyebilecek kadar önemli bir oranı aştığında trafiği azaltır. Uyarı veya müşteri başına özelleştirilmiş ilkeler sağlamaz.

### <a name="ddos-protection-standard"></a>DDoS Koruma Standardı

Standart koruma gelişmiş DDoS azaltma özellikleri sağlar. Sanal ağdaki belirli Azure kaynaklarınızı korumaya yardımcı olacak şekilde otomatik olarak ayarlanır. Koruma, yeni veya varolan sanal ağda etkinleştirmek kolaydır ve uygulama veya kaynak değişikliği gerektirmez. Bu günlük, uyarı ve telemetri de dahil olmak üzere temel hizmet, çeşitli avantajları vardır. Aşağıdaki bölümlerde Azure DDoS Koruma Standardı hizmetinin temel özellikleri sıralanmaktadır.

#### <a name="adaptive-real-time-tuning"></a>Uyarlanabilir gerçek zamanlı atokalma

Azure DDoS Protection Basic hizmeti, müşterilerin korunmasına ve diğer müşterilerin etkilerini önlemeye yardımcı olur. Örneğin, bir hizmet, altyapı genelindeki DDoS Koruma ilkesinin tetikleyici *hızından* daha küçük olan tipik bir yasal gelen trafik hacmi için sağlanmışsa, bu müşterinin kaynaklarına yönelik bir DDoS saldırısı fark edilmeden gidebilir. Daha genel olarak, son saldırıların karmaşıklığı (örneğin, çok vektörlü DDoS) ve kiracıların uygulamaya özgü davranışları müşteri başına, özelleştirilmiş koruma ilkeleri için çağrıda bulunuyor. Hizmet, iki kavrayış kullanarak bu özelleştirmeyi gerçekleştirir:

- Katman 3 ve 4 için müşteri başına (IP başına) trafik desenlerinin otomatik olarak öğrenilir.

- Azure ölçeğinin önemli miktarda trafiği absorbe etmesine olanak sağladığını göz önünde bulundurarak yanlış pozitifleri en aza indirmek.

!["İlke Oluşturma" daire içine alınmış DDoS Koruma Standardının nasıl çalıştığına ait diyagram](./media/ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring-and-alerting"></a>DDoS Koruma telemetri, izleme ve uyarı

DDoS Koruma Standardı, Bir DDoS saldırısı süresince [Azure Monitor](/azure/azure-monitor/overview) üzerinden zengin telemetriyi ortaya çıkarır. DDoS Protection'ın kullandığı Azure Monitör ölçümleri için uyarıları yapılandırabilirsiniz. Azure Monitör Tanılama arabirimi üzerinden gelişmiş analiz ler için günlüğe kaydetmeyi Splunk (Azure Etkinlik Hub'ları), Azure Monitör günlükleri ve Azure Depolama ile tümleştirebilirsiniz.

##### <a name="ddos-mitigation-policies"></a>DDoS azaltma ilkeleri

Azure portalında, **Monitör** > **Ölçümleri'ni**seçin. **Ölçümler** bölmesinde kaynak grubunu seçin, **Genel IP Adresi**kaynak türünü seçin ve Azure genel IP adresinizi seçin. DDoS ölçümleri Kullanılabilir **ölçümler** bölmesinde görülebilir.

DDoS Koruma Standardı, DDoS etkinleştirilmiş sanal ağda, korunan kaynağın her ortak IP'si için üç otomatik ayarlanmış azaltma ilkesi (TCP SYN, TCP ve UDP) uygular. **DDoS azaltmayı tetiklemek için**metrik Gelen paketleri seçerek ilke eşlemlerini görüntüleyebilirsiniz.

![Kullanılabilir ölçümler ve ölçümler grafiği](./media/ddos-best-practices/image7.png)

İlke eşikleri makine öğrenimi tabanlı ağ trafiği profiloluşturma yoluyla otomatik olarak yapılandırılır. DDoS azaltma yalnızca ilke eşiği aşıldığında saldırı altındaki bir IP adresi için oluşur.

##### <a name="metric-for-an-ip-address-under-ddos-attack"></a>DDoS saldırısı altındaki bir IP adresi için metrik

Genel IP adresi saldırı altındaysa, DDoS koruması saldırı trafiğinde azaltma gerçekleştirirken **DDoS saldırısı altındaki** metrik veya 1 olarak değişmez.

!["DDoS saldırısı altında veya değil" metrik ve grafik](./media/ddos-best-practices/image8.png)

Bu metrikte bir uyarı yapılandırmanızı öneririz. Daha sonra, genel IP adresinizde etkin bir DDoS azaltma işlemi yapıldığında size bildirilir.

Daha fazla bilgi için Azure [portalını kullanarak Azure DDoS Koruma Standardını Yönet'e](/azure/virtual-network/ddos-protection-manage-portal)bakın.

#### <a name="web-application-firewall-for-resource-attacks"></a>Kaynak saldırıları için Web uygulaması güvenlik duvarı

Uygulama katmanındaki kaynak saldırılarına özgü olarak, web uygulamalarının güvenliğini sağlamaya yardımcı olmak için bir web uygulaması güvenlik duvarı (WAF) yapılandırmanız gerekir. WAF, SQL enjeksiyonlarını, siteleri arası komut dosyası komut dosyasını, DDoS'u ve diğer Katman 7 saldırılarını engellemek için gelen web trafiğini denetler. Azure, web uygulamalarınızın yaygın açıklardan ve güvenlik açıklarından merkezi olarak korunması için [Uygulama Ağ Geçidi'nin bir özelliği olarak WAF](/azure/application-gateway/application-gateway-web-application-firewall-overview) sağlar. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1)aracılığıyla ihtiyaçlarınıza daha uygun olabilecek diğer WAF teklifleri de Azure iş ortaklarından edinilebilir.

Hatta web uygulaması güvenlik duvarları hacimsel ve devlet yorgunluk saldırılarına duyarlıdır. Hacimsel ve protokol saldırılarına karşı korunmaya yardımcı olmak için WAF sanal ağında DDoS Koruma Standardı'nı etkinleştirmenizi önemle öneririz. Daha fazla bilgi için [DDoS Protection başvuru mimarileri](#ddos-protection-reference-architectures) bölümüne bakın.

### <a name="protection-planning"></a>Koruma planlaması

Planlama ve hazırlık, bir sistemin DDoS saldırısı sırasında nasıl performans göstereceğini anlamak için çok önemlidir. Bir olay yönetimi yanıt planı tasarlamak bu çabanın bir parçasıdır.

DDoS Koruma Standard'ınvarsa, internete bakan uç noktaların sanal ağında etkinleştirildiğinden emin olun. DDoS uyarılarını yapılandırmak, altyapınıza yönelik olası saldırıları sürekli olarak izlemenize yardımcı olur. 

Uygulamalarınızı bağımsız olarak izleyin. Bir uygulamanın normal davranışını anlayın. Uygulama bir DDoS saldırısı sırasında beklendiği gibi davranmıyorsa harekete hazırlanın.

#### <a name="testing-through-simulations"></a>Simülasyonlar aracılığıyla test etme

Hizmetlerinizin periyodik simülasyonlar yaparak bir saldırıya nasıl tepki vereceğine dair varsayımlarınızı test etmek iyi bir uygulamadır. Sınama sırasında, hizmetlerinizin veya uygulamalarınızın beklendiği gibi çalışmaya devam ettiğini ve kullanıcı deneyiminde herhangi bir aksama olmadığını doğrulayın. Hem teknoloji hem de süreç açısından boşlukları belirleyin ve Bunları DDoS yanıt stratejisine dahil edin. Üretim ortamına etkisini en aza indirmek için bu tür testleri evreleme ortamlarında veya yoğun olmayan saatlerde gerçekleştirmenizi öneririz.

Azure müşterilerinin simülasyonlar için DDoS Koruması özellikli ortak uç noktalarına karşı trafik oluşturabilecekleri bir arayüz oluşturmak için [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) ile ortaklık kurduk. [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) simülasyonunu şu şekilde kullanabilirsiniz:

- Azure DDoS Koruması'nın Azure kaynaklarınızı DDoS saldırılarına karşı korumaya nasıl yardımcı olduğunu doğrulayın.

- DDoS saldırısı altındayken olay yanıt sürecinizi optimize edin.

- Belge DDoS uyumluluğu.

- Ağ güvenlik ekiplerinizi eğitin.

Siber güvenlik savunmada sürekli yenilik gerektirir. Azure DDoS Standart koruması, giderek karmaşıklaşan DDoS saldırılarını azaltmak için etkili bir çözüm sunan en son teknolojiürünü sunar.

## <a name="components-of-a-ddos-response-strategy"></a>DDoS yanıt stratejisinin bileşenleri

Azure kaynaklarını hedefleyen bir DDoS saldırısı genellikle kullanıcı açısından en az müdahale gerektirir. Yine de, bir olay yanıt stratejisinin bir parçası olarak DDoS azaltma dahil iş sürekliliği üzerindeki etkisini en aza indirmeye yardımcı olur.

### <a name="microsoft-threat-intelligence"></a>Microsoft tehdit zekası

Microsoft'un kapsamlı bir tehdit istihbarat ağı vardır. Bu ağ, Microsoft çevrimiçi hizmetlerini, Microsoft iş ortaklarını ve internet güvenliği topluluğu içindeki ilişkileri destekleyen genişletilmiş bir güvenlik topluluğunun ortak bilgilerini kullanır. 

Kritik bir altyapı sağlayıcısı olarak Microsoft, tehditler hakkında erken uyarılar alır. Microsoft, çevrimiçi hizmetlerinden ve küresel müşteri tabanından tehdit istihbaratı toplar. Microsoft, tüm bu tehdit zekasını Azure DDoS Protection ürünlerine geri dahil eder.

Ayrıca, Microsoft Dijital Suçlar Birimi (DCU) botnet'lere karşı saldırgan stratejiler uygular. Botnetler, DDoS saldırıları için ortak bir komuta ve kontrol kaynağıdır.

### <a name="risk-evaluation-of-your-azure-resources"></a>Azure kaynaklarınızın risk değerlendirmesi

Sürekli olarak bir DDoS saldırısından kaynaklanan riskinizin kapsamını anlamak zorunludur. Periyodik olarak kendinize sorun:

- Kamuya açık yeni hangi Azure kaynaklarının korunmaya ihtiyacı vardır?

- Hizmette tek bir hata noktası var mı? 

- Hizmetleri geçerli müşterilere sunmayı sağlarken bir saldırının etkisini sınırlamak için hizmetler nasıl yalıtılabilir?

- DDoS Koruma Standardının etkinleştirilmesi gereken ancak etkinleştirilmediği sanal ağlar var mı? 

- Hizmetlerim birden çok bölgede başarısız olarak etkin mi/etkin mi?

### <a name="customer-ddos-response-team"></a>Müşteri DDoS yanıt ekibi

DDoS yanıt ekibi oluşturmak, bir saldırıya hızlı ve etkili bir şekilde yanıt vermede önemli bir adımdır. Kuruluşunuzda hem planlama hem de yürütmeyi denetleyecek kişileri tanımlayın. Bu DDoS yanıt ekibi Azure DDoS Koruma Standardı hizmetini iyice anlamalıdır. Microsoft destek ekibi de dahil olmak üzere dahili ve harici müşterilerle koordine ederek takımın bir saldırıyı tanımlayadığından ve azaltabildiğinden emin olun.

DDoS yanıt ekibiniz için, simülasyon alıştırmalarını hizmet kullanılabilirliği ve süreklilik planlamanızın normal bir parçası olarak kullanmanızı öneririz. Bu alıştırmalar ölçek testini içermelidir.

### <a name="alerts-during-an-attack"></a>Saldırı sırasında uyarılar

Azure DDoS Koruma Standardı, herhangi bir kullanıcı müdahalesi olmadan DDoS saldırılarını tanımlar ve azaltır. Korumalı bir genel IP için etkin bir azaltma olduğunda haberdar olmak için, **DDoS saldırısı altında**metrik [üzerinde bir uyarı yapılandırabilirsiniz.](/azure/virtual-network/ddos-protection-manage-portal) Saldırının ölçeğini, bırakılan trafiği ve diğer ayrıntıları anlamak için diğer DDoS ölçümleri için uyarılar oluşturmayı seçebilirsiniz.

#### <a name="when-to-contact-microsoft-support"></a>Microsoft desteğine ne zaman başvurun

- Bir DDoS saldırısı sırasında, korumalı kaynağın performansının ciddi şekilde düşürüldüğünü veya kaynağın kullanılamadığını fark esiniz.

- DDoS Koruma hizmetinin beklendiği gibi davranmadığını düşünüyorsunuz. 

  DDoS Koruma hizmeti, yalnızca **DDoS azaltmayı tetikleyen metrik değer Politikası (TCP/TCP SYN/UDP)** korunan kamu IP kaynağında alınan trafikten daha düşükse azaltma başlar.

- Ağ trafiğinizi önemli ölçüde artıracak viral bir etkinlik planlıyorsunuz.

- Bir aktör kaynaklarınıza karşı bir DDoS saldırısı başlatmakla tehdit etti.

- Azure DDoS Koruma Standardı'ndan bir IP veya IP aralığının listelanmasına izin vermeniz gerekiyorsa. Trafiğin harici bir bulut WAF'tan Azure'a yönlendirilmişolması durumunda liste IP'sine izin veren yaygın bir senaryodur. 

Kritik bir iş etkisi olan saldırılar için önem derecesi-A [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)oluşturun.

### <a name="post-attack-steps"></a>Saldırı sonrası adımlar

Bir saldırıdan sonra otopsi yapmak ve Gerektiğinde DDoS yanıt stratejisini ayarlamak her zaman iyi bir stratejidir. Göz önünde bulundurulması gerekenler:

- Ölçeklenebilir mimari eksikliği nedeniyle hizmet veya kullanıcı deneyiminde herhangi bir aksaklık oldu mu?

- En çok hangi uygulama veya hizmetlerden zarar gördü?

- DDoS yanıt stratejisi ne kadar etkiliydi ve nasıl geliştirilebilir?

Bir DDoS saldırısı altında olduğunuzdan şüpheleniyorsanız, normal Azure Destek kanallarınızda tırmanın.

## <a name="ddos-protection-reference-architectures"></a>DDoS Protection referans mimarileri

DDoS Koruma Standardı, [sanal ağda dağıtılan hizmetler için](/azure/virtual-network/virtual-network-for-azure-services)tasarlanmıştır. Diğer hizmetler için varsayılan DDoS Protection Basic hizmeti uygulanır. Aşağıdaki başvuru mimarileri senaryolara göre düzenlenir ve mimari desenler birlikte gruplanır.

### <a name="virtual-machine-windowslinux-workloads"></a>Sanal makine (Windows/Linux) iş yükleri

#### <a name="application-running-on-load-balanced-vms"></a>Yük dengeli VM'lerde çalışan uygulama

Bu başvuru mimarisi, kullanılabilirliği ve ölçeklenebilirliği artırmak için bir yük dengeleyicisinin arkasındaki ölçekte birden çok Windows VM çalıştırmak için kanıtlanmış bir dizi uygulama gösterir. Bu mimari, web sunucusu gibi herhangi bir durum suz iş yükü için kullanılabilir.

![Yük dengeli VM'lerde çalışan bir uygulama için başvuru mimarisinin diyagramı](./media/ddos-best-practices/image9.png)

Bu mimaride, bir iş yükü birden çok VM örneğine dağıtılır. Tek bir genel IP adresi vardır ve internet trafiği bir yük dengeleyicisi aracılığıyla VM'lere dağıtılır. DDoS Koruma Standardı, azure (internet) yük bakiyesi ile ilişkili genel IP'ye sahip sanal ağda etkinleştirilir.

Yük dengeleyicisi gelen internet isteklerini VM örneklerine dağıtır. Sanal makine ölçek kümeleri, VM sayısının el ile veya dışarı ölçeklendirilmesine veya önceden tanımlanmış kurallara göre otomatik olarak ölçeklendirilmesine olanak sağlar. Kaynak DDoS saldırısı altındaysa bu önemlidir. Bu başvuru mimarisi hakkında daha fazla bilgi için [bu makaleye](/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm)bakın.

#### <a name="application-running-on-windows-n-tier"></a>Windows N katmanında çalışan uygulama

N katmanlı mimari uygulamanın birçok yolu vardır. Aşağıdaki diyagram tipik bir üç katmanlı web uygulaması gösterir. Bu [mimari, ölçeklenebilirlik ve kullanılabilirlik için yük dengeli VM'leri çalıştır](/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm)makalesine dayanmaktadır. Web ve iş katmanları yük dengeli VM’leri kullanır.

![Windows N katmanında çalışan bir uygulama için başvuru mimarisinin diyagramı](./media/ddos-best-practices/image10.png)

Bu mimaride, Sanal ağda DDoS Koruma Standardı etkindir. Sanal ağdaki tüm genel IP'ler Katman 3 ve 4 için DDoS koruması alır. Katman 7 koruması için WAF SKU'da Uygulama Ağ Geçidi'ni dağıtın. Bu başvuru mimarisi hakkında daha fazla bilgi için [bu makaleye](/azure/architecture/reference-architectures/virtual-machines-windows/n-tier)bakın.

#### <a name="paas-web-application"></a>PaaS web uygulaması

Bu başvuru mimarisi, tek bir bölgede bir Azure Uygulama Hizmeti uygulamasını çalıştıran gösterir. Bu mimari, [Azure Uygulama Hizmeti](https://azure.microsoft.com/documentation/services/app-service/) ve [Azure SQL Veritabanı](https://azure.microsoft.com/documentation/services/sql-database/)kullanan bir web uygulaması için kanıtlanmış bir dizi uygulama gösterir.
Başarısız senaryolar için bir bekleme bölgesi ayarlanır.

![PaaS web uygulaması için referans mimarisinin diyagramı](./media/ddos-best-practices/image11.png)

Azure Trafik Yöneticisi, gelen istekleri bölgelerden birinde Uygulama Ağ Geçidi'ne yönlendirir. Normal işlemler sırasında, istekleri etkin bölgedeki Uygulama Ağ Geçidi'ne yönlendirir. Bu bölge kullanılamaz hale gelirse, Trafik Yöneticisi bekleme bölgesindeki Uygulama Ağ Geçidi'nde başarısız olur.

Internet'ten web uygulamasına giden tüm trafik, Traffic Manager aracılığıyla [Application Gateway genel IP adresine](/azure/application-gateway/application-gateway-web-app-overview) yönlendirilir. Bu senaryoda, uygulama hizmetinin (web uygulaması) kendisi doğrudan dışarıdan karşı karşıya değildir ve Uygulama Ağ Geçidi tarafından korunur. 

Katman 7 (HTTP/HTTPS/WebSocket) saldırılarına karşı korunmaya yardımcı olacak şekilde Uygulama Ağ Geçidi WAF SKU (önleme modu) yapılandırmanızı öneririz. Ayrıca, web uygulamaları yalnızca Uygulama Ağ Geçidi IP [adresinden gelen trafiği kabul](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) etmek üzere yapılandırılmıştır.

Bu başvuru mimarisi hakkında daha fazla bilgi için [bu makaleye](/azure/architecture/reference-architectures/app-service-web-app/multi-region)bakın.

### <a name="mitigation-for-non-web-paas-services"></a>Web dışı PaaS hizmetleri için azaltma

#### <a name="hdinsight-on-azure"></a>Azure'da HDInsight

Bu başvuru mimarisi, bir [Azure HDInsight kümesi](/azure/hdinsight/)için DDoS Koruma Standardı'nı yapılandırıcı gösterir. HDInsight kümesinin sanal ağa bağlı olduğundan ve Sanal ağda DDoS Koruması etkin olduğundan emin olun.

![Sanal ağ ayarlarıyla "HDInsight" ve "Gelişmiş ayarlar" bölmeleri](./media/ddos-best-practices/image12.png)

![DDoS Korumasını etkinleştirmek için seçim](./media/ddos-best-practices/image13.png)

Bu mimaride, internetten HDInsight kümesine giden trafik, HDInsight ağ geçidi yük dengeleyicisiyle ilişkili genel IP'ye yönlendirilir. Ağ geçidi yük dengeleyicisi daha sonra trafiği baş düğümlerine veya işçi düğümlerine doğrudan gönderir. HDInsight sanal ağında DDoS Koruma Standardı etkin olduğundan, sanal ağdaki tüm genel IP'ler Katman 3 ve 4 için DDoS koruması alır. Bu başvuru mimarisi, N-Tier ve çok bölgeli başvuru mimarileri ile birleştirilebilir.

Bu başvuru mimarisi hakkında daha fazla bilgi için Azure Sanal Ağ belgelerini [kullanarak Azure HDInsight'ı Genişlet'e](/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) bakın.


> [!NOTE]
> PowerApps için Azure Uygulama Hizmet Ortamı veya genel BIR IP'ye sahip sanal ağdaki API yönetimi için her ikisi de yerel olarak desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Bulutta paylaşılan sorumluluk](shared-responsibility.md)

* [Azure DDoS Koruması ürün sayfası](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS Koruma belgeleri](/azure/virtual-network/ddos-protection-overview)
