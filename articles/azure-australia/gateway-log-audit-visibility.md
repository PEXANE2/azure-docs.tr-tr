---
title: Azure Avustralya 'da ağ geçidi günlüğü, denetim ve görünürlük
description: Avustralya kamu ilkesinin, düzenlemelerinin ve yasalların belirli ihtiyaçlarını karşılamak için Avustralya bölgeleri içinde günlük, denetim ve görünürlük yapılandırma.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 153b9d503dfece404455fbb7e8cb51c51686ec57
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68824304"
---
# <a name="gateway-logging-auditing-and-visibility-in-azure-australia"></a>Azure Avustralya 'da ağ geçidi günlüğü, denetim ve görünürlük

Siber güvenlik tehditlerini algılama ve yanıtlama, bir sistemin çalışması ile ilgili verileri oluşturma, toplama ve analiz etme işlemlerini kullanır.

Microsoft, Azure 'da dağıtılan sistemlerinizin güvenliğini yönetmek için günlük, denetim ve görünürlük uygulamanıza yardımcı olmak üzere Azure 'daki yerleşik araçlara sahiptir. Ayrıca, Avustralya Cyber Güvenlik Merkezi (ACSC) tüketici kılavuzumuzu ve bilgi güvenliği el kitabı (ıSM) amacını içeren bir başvuru mimarisi de vardır.

Ağ geçitleri, Ağ katmanında bilgi akışı denetim mekanizmaları olarak davranır ve açık sistem bağlantısı (OSı) modelinin daha yüksek katmanlarındaki bilgileri de denetleyebilir. Ağ geçitleri, güvenlik etki alanları arasındaki veri akışlarını denetlemek ve dış ağlardan yetkisiz erişimi engellemek için gereklidir. Güvenlik alanları arasındaki bilgi akışını denetlerken ağ geçitlerinin önemi verildiğinde, özellikle de daha yüksek sınıflandırmalardaki herhangi bir hata ciddi sonuçlara sahip olabilir. Bu nedenle, bir personeli, siber güvenlik olaylarına neden olabilecek durumlara yönelik güçlü mekanizmalar, ağ geçitleri için özellikle önemlidir.

Ağ geçitleri için günlüğe kaydetme ve uyarı yeteneklerini uygulamak, siber güvenlik olaylarını tespit etme, denenen yetkisiz kullanım ve alışılmadık kullanım düzenlerini gidermeye yardımcı olabilir. Ayrıca, olay günlüklerinin ayrı bir güvenli günlük sunucusunda depolanması, hedeflenen bir siber izinsiz giriş kanıtını ortadan kaldırmak için, bir saldırgan tarafından günlüğe kaydetme bilgilerini silmenin zorluğunu artırır.

## <a name="australian-cyber-security-centre-acsc-requirements"></a>Avustralya Cyber Güvenlik Merkezi (ACSC) gereksinimleri

Uluslar sistemleri için genel güvenlik gereksinimleri ACSC Information Security Manual (ıSM) içinde tanımlanmıştır. Azure 'daki *ACSC tüketicisi Kılavuzu – Microsoft Azure, korunan* ve *ACSC sertifikasyon Microsoft Azure RAPORUNDAKI* acsc tüketici Kılavuzu – Günlüğe kaydetme, denetim ve görünürlük:

1. , Paylaşılan temel bulut kaynaklarını kullanmaktan kaynaklanan riskleri azaltmak için, Azure Güvenlik Merkezi, Azure Izleyici, Azure Ilkesi ve Azure Danışmanı dahil olmak üzere varlıkların gerçekleştirmesine yardımcı olması için sunulan olanakları Microsoft Azure kabul etmelidir. Azure iş yüklerinin gerçek zamanlı izleme

2. ACSC Ayrıca, Ulusal varlıkların tüm uygulanan güvenlik günlüklerini Avustralya kamu izleme 'nin tamamına yönelik ACSC 'ye iletmesini öneriyor

3. Risk azaltma konusunda yardımcı olmak için, bu varlıkların Azure abonelikleri içinde yapılandırılması gerekir:

    * Azure Güvenlik Merkezi 'ni etkinleştir
    * Standart katmana yükseltme
    * Desteklenen Azure VM 'lerine Microsoft Monitoring Agent otomatik sağlamayı etkinleştir
    * Güvenlik Merkezi panosunda güvenlik önerilerini ve uyarılarını düzenli olarak gözden geçirin, öncelikler yapın ve azaltın

4. Kamu varlıklarının, bu kılavuzlarla uyumsuz olarak görünebilirliği sağlamak üzere ACSC 'nin Azure aboneliğinden ACSC 'ye günlük ve olay iletmeyi etkinleştirmesi gerekir. Azure Event Hubs, kurumda veya şirket içinde bulunan ve şirket içi sistemlere harici günlük akışı yapma yeteneği sağlar

5. Bu varlıkların, Azure 'da etkinleşdikleri günlüğe kaydetme işleminin, ıSM 'de belirtilen gereksinimlere göre hizalanması gerekir

6. Microsoft, günlükleri Azure 'da 90 gün boyunca tutar. , NAA AFDA altında gereken yedi yıl boyunca günlüklerin korunabilmesi için Müşteri varlıklarının bir günlük Arşivi regime uygulaması gerekir

7. Şirket içi veya Azure tabanlı güvenlik bilgilerine ve olay yönetimi (SıEM) özelliklerine sahip olan kurumlar, günlükleri bu sistemlere de iletebilir

8. Kurumlar varlıkları ağ güvenlik grupları (NSG 'ler) ve sanal makineler için ağ Izleyicisi akış günlüklerini uygulamalıdır. Bu günlüklerin yalnızca güvenlik günlüklerini içeren ayrılmış bir depolama hesabında depolanması ve depolama hesabına erişimin rol tabanlı erişim denetimleriyle güvenli hale getirilmesi gerekir

9. Azure iş yüklerinin günlüğe kaydetme ve izleme amacını karşıladığından emin olmak için, kurumlar varlıklarının ACSC tüketici kılavuzunu uygulaması gerekir. Bu varlıkların, ACSC 'nin Azure 'un Avustralya kamu kullanımıyla ilişkili gerçek zamanlı izleme, uyarı ve Günlükler almasına yardımcı olan Azure yeteneklerini de kabul etmesi gerekir

## <a name="architecture"></a>Mimari

Azure ortamınızı giren ve çıkan ağ trafiğini güvenle anlamak için, gerekli günlük kaydının doğru bileşen kümesinde etkinleştirilmesi gerekir. Bu, ortamın tamamen görünürlüğünü sağlar ve analiz yapmak için gerekli verileri sağlar.

![Azure Izleme mimarisi](media/visibility.png)

## <a name="components"></a>Bileşenler

Yukarıda gösterilen mimari, günlük kaynakları, günlük koleksiyonu, günlük saklama, günlük analizi veya olay yanıtı işlevlerini sağlayan ayrı bileşenlerden oluşur. Bu mimari, genellikle internet erişimi olan Azure dağıtımlarına dahil olan ayrı bileşenleri içerir.

|İşlevler|Bileşenler|
|---|---|
|Günlük kaynakları|<ul><li>Application Gateway</li><li>VPN Gateway</li><li>Azure Güvenlik Duvarı</li><li>Ağ sanal cihazları</li><li>Azure Load Balancer</li><li>Virtual Machines</li><li>Etki alanı adlandırma sistemi (DNS) sunucuları</li><li>Syslog ve/veya günlük toplama sunucuları</li><li>NSG</li><li>Azure etkinlik günlüğü</li><li>Azure tanılama günlüğü</li><li>Azure İlkesi</li></ul>|
|Günlük Toplama|<ul><li>Event Hubs</li><li>Ağ İzleyicisi</li><li>Log Analytics</li></ul>|
|Günlük tutma|<ul><li>Azure Storage</li></ul>|
|Log Analytics|<ul><li>Azure Güvenlik Merkezi (ASC)</li><li>Azure Advisor</li><li>Log Analytics çözümleri<ul><li>Trafik Analizi</li><li>DNS Analizi (Önizleme)</li><li>Etkinlik Günlüğü Analizi</li></ul></li><li>SIEM</li><li>ACSC</li></ul>|
|Olay Yanıtı|<ul><li>Azure Uyarıları</li><li>Azure Otomasyonu</li></ul>|
|

Mimari öncelikle gerekli kaynaklardan günlükleri oluşturup sonra da bunları denetleyebileceği depolara topladıktan sonra işe yarar. Günlükleri topladıktan sonra şunları yapabilirsiniz:

* Azure Analysis Services tarafından Öngörüler elde etmek için kullanılır.
* Dış sistemlere iletilen veya
* uzun süreli saklama için depolamaya arşivlendi.

Analiz araçları tarafından tanımlanan önemli olaylara veya olaylara yanıt vermek için, uyarılar yapılandırılabilir ve otomasyon, proaktif yönetim ve yanıt için gerekli eylemleri gerçekleştirmek üzere geliştirilir.

## <a name="general-guidance"></a>Genel rehberlik

Bu makalede listelenen bileşenleri uygularken, aşağıdaki genel rehberlik geçerlidir:

* Tüm verilerin yetkili konumlar içinde kalmasını ve korunan iş yükleri için ilk tercih olarak AU Orta veya AU Orta 2 dağıtımını sağlamak için hizmetlerin bölge kullanılabilirliğini doğrulayın

* Tek tek hizmetlerin sertifika durumu için *Azure-ACSC sertifika raporu – Protected 2018* yayınına bakın ve *ACSC tüketici Kılavuzu 'na göre rapora dahil olmayan ilgili tüm bileşenler üzerinde kendi kendine değerlendirmeler gerçekleştirin – Microsoft Azure KORUMALı*

* Bu makalede başvurulmayan bileşenler için, uluslar topluluğu, günlükleri oluşturma, yakalama, analiz etme ve sürdürme ile ilgili ilkeleri izlemelidir

* Azure 'da barındırılan sistemlere yönelik tüm ağ giriş ve çıkış noktaları için günlük, denetim ve yüksek değerli sistemlerin görünürlüğünü ve bunların görünürlüğünü tanımla ve öncelikler

* Günlükleri birleştirin ve depolama hesapları, Log Analytics çalışma alanları ve Event Hubs gibi günlük araç örneklerinin sayısını en düşük düzeyde yapın

* Rol tabanlı erişim denetimleri aracılığıyla yönetici ayrıcalıklarını kısıtlama

* Azure 'da kaynakları yönetme veya yapılandırma hesapları için Multi-Factor Authentication (MFA) kullanma

* Birden çok abonelikte merkezi olarak kullanılan günlük toplamayı yaparken, yöneticilerin her abonelik için gerekli ayrıcalıklara sahip olduğundan emin olun

* Sanal makineler için ağ bağlantısı ve ağ sanal gereçleri (NVA 'lar), günlük toplama sunucuları ve DNS sunucuları dahil olmak üzere, Log Analytics çalışma alanları gibi gerekli Azure hizmetlerine bağlanmak için gereken tüm proxy yapılandırmalarını sağlayın Event Hubs ve depolama

* Microsoft Monitoring Agent (MMA) ile TLS sürüm 1,2 ' i yapılandırma

* Gereksinimlerle uyumluluğu izlemek ve zorlamak için Azure Ilkesini kullanın

* Depolama ve veritabanları gibi tüm veri depolarında şifrelemeyi zorunlu kıl

* Depolama hesaplarının ve ilişkili verilerin kullanılabilirliği için yerel olarak yedekli depolama (LRS) ve anlık görüntüleri kullanın

* Olağanüstü durum kurtarma stratejileri ile hizalamak için coğrafi olarak yedekli depolama (GRS) veya site dışı depolamayı değerlendirin

|Resource|URL|
|---|---|
|Avustralya mevzuata ve Ilke uyumluluk belgeleri|[https://aka.ms/au-irap](https://aka.ms/au-irap)|
|Azure ürünleri-Avustralya bölgeleri ve bölgesel olmayanlar|[https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional, Avustralya-orta, Avustralya-Orta-2, Avustralya-Doğu, Avustralya-Güneydoğu](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast)|
|Microsoft Azure Güvenlik ve Denetim günlük yönetimi teknik Incelemesi|[https://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf)|
|Microsoft Monitoring Agent yapılandırması|[https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent)|
|

## <a name="component-guidance"></a>Bileşen Kılavuzu

Bu bölümde, her bir bileşenin amacı ve genel günlük kaydı, denetim ve görünürlük mimarisindeki rolü hakkında bilgi verilmektedir. Başvuru belgeleri, kılavuzlar ve öğreticiler gibi faydalı kaynaklara erişmek için ek bağlantılar sağlanır.

## <a name="log-sources"></a>Günlük kaynakları

Herhangi bir analiz, uyarı veya raporlama tamamlanmadan önce gerekli günlüklerin oluşturulması gerekir. Azure günlükleri denetim/yönetim günlükleri, veri düzlemi günlükleri ve işlenmiş olaylara kategorize edilir.

|Type|Açıklama|
|---|---|
|Denetim/Yönetim günlükleri|Azure Resource Manager işlemler hakkında bilgi sağlayın|
|Veri düzlemi günlükleri|Bir sanal makinedeki Günlükler ve Azure Izleyici aracılığıyla kullanılabilen tanılama günlükleri gibi Azure Kaynak kullanımının bir parçası olarak oluşturulan olaylar hakkında bilgi sağlayın|
|İşlenen olaylar|Azure Güvenlik Merkezi 'nin güvenlik uyarılarını sağlamak üzere işlendiği ve analiz edildiği gibi Azure tarafından işlenmiş olan analiz olayları/uyarıları hakkında bilgi sağlayın|
|

### <a name="application-gateway"></a>Application Gateway

Azure Application Gateway, Azure ortamında olası giriş noktalarından biridir, böylece Web uygulamalarıyla iletişim kuran gelen bağlantılarla ilgili bilgileri yakalamanız gerekir. Application Gateway, Web uygulaması kullanımıyla ilgili önemli bilgiler ve siber güvenlik olaylarını algılamayla ilgili yardım sağlayabilir. Application Gateway, Azure Izleyici 'de Log Analytics kullanılabilecek veya bir olay hub 'ına ya da depolama hesabına dağıtılabilecek olan etkinlik günlüğü ve tanılama günlüklerine meta verileri gönderir.

|Kaynaklar|Bağlantı|
|---|---|
|Application Gateway Belgeleri|[https://docs.microsoft.com/azure/application-gateway/](https://docs.microsoft.com/azure/application-gateway/)|
|Application Gateway hızlı başlangıç kılavuzu|[https://docs.microsoft.com/azure/application-gateway/quick-create-portal](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)|
|

### <a name="vpn-gateway"></a>VPN Gateway

VPN Gateway, Azure ortamında, şirket içi bir ortama ve Yönetim trafiğine bağlantı gibi çok çeşitli iletişimler için olası bir giriş noktasıdır. VPN Gateway 'lerde günlüğe kaydetme, Azure ortamına yapılan bağlantılar için Öngörüler ve izlenebilirlik sağlar. Günlüğe kaydetme, denetim ve analiz sağlayabilir ve kötü amaçlı ya da anormal bağlantıları algılamada ya da araştırmada yardımcı olabilir. VPN Gateway Günlükler, Log Analytics kullanılabilecek veya bir olay hub 'ına ya da depolama hesabına dağıtılan Azure Izleyici etkinlik günlüğüne gönderilir.

|Kaynaklar|Bağlantı|
|---|---|
|VPN Gateway Belgeleri|[https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway)|
|Avustralya kamu özgü VPN Gateway Kılavuzu|[Azure VPN Gateway yapılandırması](vpn-gateway.md)|
|

### <a name="azure-firewall"></a>Azure Güvenlik Duvarı

Azure Güvenlik Duvarı, Azure ortamından kontrollü bir çıkış noktası sağlar ve denenen ve başarılı giden bağlantılar hakkında bilgi içeren Günlükler, günlük stratejinizde önemli bir öğedir. Bu Günlükler, sistemlerin tasarlandığı gibi çalışıyor olduğunu ve yetkisiz olmayan sistemlere bağlanmaya çalışan kötü amaçlı kod veya aktörlerin algılanmasıyla ilgili yardım sağlar. Azure Güvenlik Duvarı, Azure Izleyici 'de Log Analytics kullanılabildiği veya bir olay hub 'ına ya da depolama hesabına dağıtılan olayları günlüğe yazar.

|Kaynaklar|Bağlantı|
|---|---|
|Azure Güvenlik Duvarı Belgeleri|[https://docs.microsoft.com/azure/firewall/](https://docs.microsoft.com/azure/firewall)|
|Öğretici: Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme|[https://docs.microsoft.com/azure/firewall/tutorial-diagnostics](https://docs.microsoft.com/azure/firewall/tutorial-diagnostics)|
|

### <a name="network-virtual-appliances-nva"></a>Ağ sanal cihazları (NVA)

NVA 'lar, Azure 'da yerel olarak bulunan güvenlik yeteneklerini tamamlamak için kullanılabilir. NVA 'lar üzerinde oluşturulan Günlükler, siber güvenlik olaylarını tespit eden değerli kaynaklar olabilir ve toplam günlük kaydı, denetim ve görünürlük stratejisinin önemli bir parçasıdır. Günlükleri NVA 'lar 'tan yakalamak için Microsoft Monitoring Agent (MMA) kullanın. MMA 'yı yüklemeyi desteklemeyen NVA 'lar için, günlükleri geçiş için bir Syslog veya diğer günlük toplama sunucusu kullanmayı düşünün.

|Kaynaklar|Bağlantı|
|---|---|
|Ağ sanal gereçlerine genel bakış|[https://azure.microsoft.com/solutions/network-appliances](https://azure.microsoft.com/solutions/network-appliances)|
|NVA belgeleri|Azure 'da ilgili NVA 'nın uygulanması hakkındaki satıcı belgelerine bakın.|
|

### <a name="azure-load-balancer"></a>Azure Load Balancer

Azure Load Balancer Günlükler, Azure 'da dağıtılan sistemlerle ilgili bağlantılar ve kullanım hakkında yararlı bilgiler almak için kullanılır. Bu durum sistem durumu ve kullanılabilirlik izlemesi için kullanılabilir, ancak aynı zamanda, iletişim trafiğine gereken öngörüleri elde etmek ve kötü amaçlı veya anormal trafik desenleri algılamak için başka bir anahtar bileşeni de oluşturur. Azure Load Balancer, Azure Izleyici 'de Log Analytics kullanılabilecek veya bir olay hub 'ına ya da depolama hesabına dağıtılan etkinlik günlüğü ve tanılama günlüklerine kaydeder.

|Kaynaklar|Bağlantı|
|---|---|
|Azure Load Balancer belgeleri|[https://docs.microsoft.com/azure/load-balancer](https://docs.microsoft.com/azure/load-balancer)|
|Standart Load Balancer için ölçümler ve sistem durumu tanılaması|[https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics)|
|

### <a name="virtual-machines"></a>Virtual Machines

Sanal makineler, ağ iletişimleri gönderen ve alan, verileri işleyen ve hizmetleri sağlayan son noktalardır. Sanal makineler, verileri veya önemli sistem hizmetlerini barındırabildiklerinden, doğru şekilde çalıştıklarından ve siber güvenlik olaylarının saptanmasının kritik olması gerekir. Sanal makineler, sistemin çalışmasını ve bu sistemde gerçekleştirilen eylemleri izleyebileceği çeşitli olay ve denetim günlükleri toplar. Sanal makinelerde toplanan Günlükler, Azure Güvenlik Merkezi ve uygulanabilir Log Analytics çözümleri tarafından anallebileceği Microsoft Monitoring Agent kullanarak bir Log Analytics çalışma alanına iletilebilir. Sanal makineler ayrıca doğrudan veya bir günlük toplama sunucusu aracılığıyla doğrudan veya bir SıEM ile doğrudan Azure Event Hubs veya bir SıEM ile de tümleştirilebilir.

|Kaynaklar|Bağlantı|
|---|---|
|Virtual Machines|[https://docs.microsoft.com/azure/virtual-machines](https://docs.microsoft.com/azure/virtual-machines)|
|Sanal makinelerden veri toplama|[https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm)|
|Event Hubs sanal makine günlüklerini akışla|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics-streaming-event-hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics-streaming-event-hubs)|
|

### <a name="domain-name-services-dns-servers"></a>Etki alanı adı hizmetleri (DNS) sunucuları

DNS sunucusu günlükleri, sistemlerin dahili ya da harici olarak erişmeye çalıştığı hizmetlerle ilgili önemli bilgileri sağlar. DNS günlüklerinin yakalanması, bir siber güvenlik olayının belirlenmesine ve olay türüne ilişkin Öngörüler sağlamanıza ve etkilenmeyen sistemlere yardımcı olabilir. Microsoft Yönetim Aracısı (MMA), DNS Analizi (Önizleme) ' de kullanılmak üzere Log Analytics aracılığıyla günlükleri iletmek için DNS sunucularında kullanılabilir.

|Kaynaklar|Bağlantı|
|---|---|
|Sanal ağlar için Azure ad çözümlemesi|[https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)|
|

### <a name="syslog-and-log-collection-servers"></a>Syslog ve günlük toplama sunucuları

Ağ sanal gereçlerine veya bir SıEM içinde kullanılmak üzere diğer sistemlerden gelen özel güvenlik günlüklerine Günlükler almak için, adanmış sunucular Azure sanal ağları içinde dağıtılabilir. Syslog günlükleri bir Syslog sunucusunda toplanabilir ve analiz için Log Analytics 'e aktarılabilirler. Günlük toplama sunucusu, denetleyebileceği izleme sistemleri veya Sıems tarafından kullanılan herhangi bir günlük toplama ve dağıtım özelliği için genel bir terimdir. Bunlar, denetleyebileceği özelliğine dağıtılmadan önce ağ mimarisini ve güvenliğini basitleştirmek ve günlükleri filtreleyip toplayabilmek için kullanılabilir.

|Kaynaklar|Bağlantı|
|---|---|
|Syslog Log analytics'te veri kaynakları|[https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-syslog](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-syslog)|
|Günlük toplama sunucusu|İzleme ve SıEM mimarisine ilişkin ayrıntılar için satıcı belgelerine başvurun|
|

### <a name="network-security-groups-nsgs"></a>Ağ güvenlik grupları (NSG 'ler)

NSG 'ler, Azure 'daki sanal ağların içine ve dışına trafiği denetler. NSG 'ler, Azure ve şirket içi ya da Internet gibi dış ağlar arasındaki trafiği de içeren, izin verilen veya reddedilen trafik akışları için kurallar uygular. NSG 'ler bir sanal ağ içindeki alt ağlara veya tek tek ağ arabirimlerine uygulanır. Azure 'da sistemleri girme ve bırakma trafiği hakkındaki bilgileri yakalamak için NSG günlükleri ağ Izleyicisi NSG akış günlükleri özelliği aracılığıyla etkinleştirilebilir. Bu Günlükler, bir sistemin standart işlemi için bir taban çizgisi oluşturmak için kullanılır ve Azure 'da barındırılan sistemlerin trafik desenleri hakkında ayrıntılı Öngörüler sağlayan Trafik Analizi veri kaynağıdır.

|Kaynaklar|Bağlantı|
|---|---|
|Ağ güvenlik grubu belgeleri|[https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)|
|Ağ güvenlik grupları için akış günlüğüne giriş|[https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|
|Öğretici: Azure portal kullanarak bir sanal makineye gelen ve giden ağ trafiğini günlüğe kaydet|[https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)|
|

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü

Azure Izleyici 'nin bir parçası olan Azure etkinlik günlüğü, Azure 'da oluşan abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir abonelik Günlüğliğidir. Etkinlik günlüğü, bir abonelikte kaynaklar ***üzerinde*** herhangi bir yazma IŞLEMI (put, Post, silme) için ' ne, kim ve ne zaman ' alındığını belirlemesine yardımcı olabilir. Etkinlik günlüğü, Azure ortamında yapılan yapılandırma değişikliklerinin izlenmesi için önemlidir. Azure etkinlik günlükleri Log Analytics çözümlerinde kullanılmak üzere otomatik olarak kullanılabilir ve işleme veya bekletme için Event Hubs ya da Azure Storage 'a gönderilebilir.

|Kaynaklar|Bağlantı|
|---|---|
|Azure etkinlik günlüğü belgeleri|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|Azure etkinlik günlüğünü Event Hubs akışa almak|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs)|
|

### <a name="azure-diagnostic-log"></a>Azure tanılama günlüğü

Azure Izleyici tanılama günlükleri, bu hizmetin çalışması hakkında zengin ve sık veriler sağlayan bir Azure hizmeti tarafından oluşturulan günlüklerdir. Tanılama günlükleri, bir kaynağın işlem hakkında ayrıntılı bir düzeyde öngörü sağlar ve denetim veya sorun giderme gibi çeşitli gereksinimler için kullanılabilir. Azure tanılama günlükleri, Log Analytics çözümlerinde kullanılmak üzere otomatik olarak kullanılabilir ve işleme veya bekletme için Event Hubs ya da Azure Storage 'a gönderilebilir.

|Kaynaklar|Bağlantı|
|---|---|
|Azure tanılama günlüğü belgeleri|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|Tanılama günlükleri için destek hizmetleri|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-diagnostic-logs-schema](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-diagnostic-logs-schema)|
|

### <a name="azure-policy"></a>Azure İlkesi

Azure Ilkesi, kaynakların nasıl dağıtılacağı, örneğin tür, konum ve yapılandırma gibi kuralları zorlar. Azure Ilkesi, kaynakların yalnızca gereksinimleriyle uyumlu olmaları durumunda dağıtılabilmeleri için yapılandırılabilir. Azure Ilkesi, bir Azure ortamının bütünlüğünü sürdürmek için bir çekirdek bileşendir. Azure Ilkesiyle ilgili olaylar Azure etkinlik günlüğüne kaydedilir ve Log Analytics çözümlerinde otomatik olarak kullanılabilir veya işleme ya da bekletme için Event Hubs ya da Azure Storage 'a gönderilebilir.

|Kaynaklar|Bağlantı|
|---|---|
|Azure İlkesi Belgeleri|[https://docs.microsoft.com/azure/governance/policy](https://docs.microsoft.com/azure/governance/policy)|
|Azure Ilkelerini kullanarak Azure Ilkesi ve Kaynak Yöneticisi şablonları kullanma|[https://docs.microsoft.com/azure/governance/blueprints/overview](https://docs.microsoft.com/azure/governance/blueprints/overview)|
|

## <a name="log-collection"></a>Günlük koleksiyonu

Birden çok günlük kaynağından oluşturulduktan sonra, devam eden erişim ve analizler için günlüklerin denetleyebileceği bir konumda depolanması gerekir. Azure, günlük türü ve gereksinimlerine bağlı olarak kullanılabilecek günlük toplama için birden çok yöntem ve seçenek sağlar.

### <a name="event-hubs"></a>Event Hubs

Bir olay hub 'ının amacı, dağıtım için çeşitli kaynaklara ait günlük verilerini toplamanız. Olay Hub 'ında, günlük verileri bir SıEM 'ye, uyumluluk için ACSC ve uzun süreli saklama için depolama alanına gönderilebilir.

|Kaynaklar|Bağlantı|
|---|---|
|Event Hubs Belgeleri|[https://docs.microsoft.com/azure/event-hubs](https://docs.microsoft.com/azure/event-hubs)|
|Event Hubs ve dış araçlarla ilgili kılavuz|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs)|
|

### <a name="log-analytics"></a>Log Analytics

Log Analytics Azure Izleyici 'nin bir parçasıdır ve günlük analizi için kullanılır. Log Analytics, Azure 'da kullanılabilen çeşitli Analysis araçları ve çözümleri için günlük verilerinin kullanılabilir hale getirilbileceği depolama mekanizması olarak bir çalışma alanı kullanır. Log Analytics, Microsoft Monitoring Agent üzerinden sanal makinelerin yanı sıra, çok çeşitli Azure bileşenleriyle doğrudan tümleşir.

|Kaynaklar|Bağlantı|
|---|---|
|Log Analytics Belgeleri|[https://docs.microsoft.com/azure/azure-monitor](https://docs.microsoft.com/azure/azure-monitor)|
|Öğretici: Log Analytics verileri analiz etme|[https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata)|
|

### <a name="network-watcher"></a>Ağ İzleyicisi

Ağ Izleyicisi kullanımı, bir Azure aboneliğindeki ağ trafiğini anlama ve yakalama konusunda yardımcı olmak için ACSC tarafından önerilir. NSG akış günlükleri, Azure aracılığıyla yerel olarak daha fazla görünürlük, analiz ve raporlama sağlayan Log Analytics Trafik Analizi çözümüne giriş sağlar. Ağ Izleyicisi Ayrıca, sanal makinede oturum açmaya gerek olmadan doğrudan Azure portal bir paket yakalama özelliği de sağlar. Paket yakalama, bir sanal makineden gelen ve giden trafiği izlemek için paket yakalama oturumları oluşturmanıza olanak sağlar.

|Kaynaklar|Bağlantı|
|---|---|
|Ağ İzleyicisi|[https://docs.microsoft.com/azure/network-watcher](https://docs.microsoft.com/azure/network-watcher)|
|Paket yakalamaya genel bakış|[https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview)|
|

## <a name="log-retention"></a>Günlük tutma

Avustralya kamu kurumları için Azure içinde yakalanan günlüklerin, en fazla yedi yıla kadar olan günlükleri saklamayı belirten Avustralya [Yönetim Işlevleri aktiften çıkarma yetkilisinin (afda)](http://www.naa.gov.au/information-management/records-authorities/types-of-records-authorities/AFDA/index.aspx)Ulusal arşivleri doğrultusunda korunması gerekir.

|Günlük konumu|Saklama Dönemi|
|---|---|
|Azure etkinlik günlüğü|90 güne kadar|
|Log Analytics çalışma alanı|En fazla iki yıl|
|Olay Hub'ı|Yedi güne kadar|
|

Günlüklerin AFDA ve diğer yasama gereksinimlerine uyacak şekilde arşivlendiğinden emin olmak sizin sorumluluğunuzdadır.

### <a name="azure-storage"></a>Azure Storage

Azure depolama, Azure 'da uzun süreli saklama için günlüklere yönelik depodur. Azure depolama, Azure 'dan Event Hubs, Azure etkinlik günlüğü ve Azure tanılama günlükleri dahil olmak üzere günlüklerin arşivlenmesi için kullanılabilir. Depolama alanındaki verilerin tutulacağı süre sıfıra ayarlanabilir veya gün sayısı olarak belirtilebilir. Sıfır günlük bir bekletme, günlüklerin süresiz olarak tutulması anlamına gelir, aksi takdirde değer 1 ile 2147483647 arasında herhangi bir sayıda gün olabilir.

|Kaynaklar|Bağlantı|
|---|---|
|Azure Storage Belgeleri|[https://docs.microsoft.com/azure/storage](https://docs.microsoft.com/azure/storage)|
|Azure Blob Depolama veya Azure Data Lake Storage Azure Event Hubs ile olayları yakalama|[https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Öğretici: Azure Depolama’yı kullanarak Azure ölçümlerini ve günlük verilerini arşivleme|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data)|
|Azure depolama çoğaltma|[https://docs.microsoft.com/azure/storage/common/storage-redundancy](https://docs.microsoft.com/azure/storage/common/storage-redundancy)|
|Blob 'un anlık görüntüsünü oluşturma|[https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)|
|

## <a name="log-analysis"></a>Log Analytics

Oluşturulup bir denetleyebileceği konumunda depolandıktan sonra, denenenlerin veya başarılı güvenlik olaylarının algılanmasına yardımcı olmak için günlüklerin analiz olması gerekir. Güvenlik olayları algılandığında, bir ajankayı bu olaylara yanıt verebilme ve tehditleri izleme, içerme ve düzeltme olanağı vardır.

### <a name="azure-security-center-asc"></a>Azure Güvenlik Merkezi (ASC)

Azure Güvenlik Merkezi, Birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması sağlar. Azure Güvenlik Merkezi, iş yükleri genelinde güvenlik ilkeleri uygulayabilir, tehditlere maruz kalma olasılığını sınırlayabilir, saldırıları algılayıp yanıtlayabilir. Azure Güvenlik Merkezi, çok çeşitli Azure bileşenleri arasında panolar ve analiz sağlar. Azure Güvenlik Merkezi 'nin kullanımı ACSC tüketici kılavuzunda bir gereksinim olarak belirtilmiştir.

|Kaynaklar|Bağlantı|
|---|---|
|Azure Güvenlik Merkezi Belgeleri|[https://docs.microsoft.com/azure/security-center](https://docs.microsoft.com/azure/security-center)|
|Hızlı Başlangıç: Azure aboneliğinizi Güvenlik Merkezi standardına ekleme|[https://docs.microsoft.com/azure/security-center/security-center-get-started](https://docs.microsoft.com/azure/security-center/security-center-get-started)|
|

### <a name="traffic-analytics"></a>Trafik Analizi

Trafik Analizi, Azure 'da Kullanıcı ve uygulama etkinliğine görünürlük sağlayan bulut tabanlı bir çözümdür. Trafik Analizi analizler ağ Izleyicisi NSG akış günlükleri, Azure 'da trafik akışı hakkında öngörüler sağlar. Trafik Analizi, sanal ağlarda görülen ağ trafiğiyle ilgili panolar, raporlar, analizler ve olay yanıt özellikleri sağlamak için kullanılır. Trafik Analizi, siber güvenlik olaylarını tanımlamaya ve çözmeye yardımcı olmak için önemli öngörüler sağlar.

|Kaynaklar|Bağlantı|
|---|---|
|Trafik Analizi belgeleri|[https://docs.microsoft.com/azure/network-watcher/traffic-analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)|
|

### <a name="azure-advisor"></a>Azure Advisor

Azure Danışmanı, genel Azure harcamasını azaltmaya yönelik fırsatları ararken kaynakların performansını, güvenliğini ve yüksek oranda kullanılabilirliğini artırmaya yardımcı olmak için kaynak yapılandırması ve diğer verileri analiz etmek üzere çözümler önerir. Azure Advisor, ACSC tarafından önerilir ve Azure ortamının yapılandırmasında kolay erişilebilir ve ayrıntılı öneriler sağlar.

|Kaynaklar|Bağlantı|
|---|---|
|Azure Advisor Belgeleri|[https://docs.microsoft.com/azure/advisor](https://docs.microsoft.com/azure/advisor)|
|Azure Advisor’ı kullanmaya başlama|[https://docs.microsoft.com/azure/advisor/advisor-get-started](https://docs.microsoft.com/azure/advisor/advisor-get-started)|
|

### <a name="dns-analytics-preview"></a>DNS Analizi (Önizleme)

DNS Analizi, Windows DNS analitik ve denetim günlüklerini ve diğer ilgili verileri toplayan, analiz eden ve ilişkilendiren bir Log Analytics çözümüdür. DNS Analizi kötü amaçlı etki alanı adlarını, eski kaynak kayıtlarını, sık sorgulanan etki alanı adlarını ve korkative DNS istemcilerini çözmeyi deneyen istemcileri tanımlar. DNS Analizi Ayrıca, DNS sunucularında ve dinamik DNS kayıt hatalarında istek yükü hakkında öngörüler sağlar. DNS Analizi, bir Azure ortamında yapılan DNS sorgularıyla ilgili panolar, raporlar, analizler ve olay yanıt özellikleri sağlamak için kullanılır. DNS Analizi, siber güvenlik olaylarını tanımlamaya ve çözmeye yardımcı olmak için önemli öngörüler sağlar.

|Kaynaklar|Bağlantı|
|---|---|
|DNS Analiz Belgeleri|[https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics](https://docs.microsoft.com/azure/azure-monitor/insights/dns-analytics)|
|

### <a name="activity-log-analytics"></a>Etkinlik Günlüğü Analizi

Etkinlik Günlüğü Analizi, Azure etkinlik günlüğünde birden çok Azure aboneliği arasında analiz etme ve arama yapılmasına yardımcı olan bir Log Analytics çözümüdür. Etkinlik Günlüğü Analizi, tüm Azure ortamının kaynakları üzerinde gerçekleştirilen eylemlerle ilgili denetleyebileceği panolar, raporlar, analiz ve olay yanıt özellikleri sağlamak için kullanılır. Etkinlik Günlüğü Analizi, denetim ve araştırmaya yardımcı olabilir.

|Kaynaklar|Bağlantı|
|---|---|
|Toplamak ve Log analytics'te Azure etkinlik günlüklerini çözümleme|[https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs)|
|

### <a name="security-information-and-event-management-siem"></a>Güvenlik bilgileri ve olay yönetimi (SıEM)

SıEM, analiz, raporlama ve olay algılama ve yanıt için çok çeşitli günlük verilerini ve akıllı araçları almak için tanımlı mekanizmalarla, güvenlik günlüklerinin denetleyebileceği depolama, denetim ve analizini yapma olanağı sunan bir sistemdir. Azure 'da yerel olarak sunulan güvenlik yeteneklerini tamamlamak için Azure günlük bilgilerini içeren SıEM yeteneklerini kullanabilirsiniz. Uluslar arası varlıklar, belirli gereksinimlere bağlı olarak Azure 'daki sanal makinelerde, şirket içinde veya hizmet olarak yazılım (SaaS) özelliği olarak barındırılan bir SıEM kullanabilir.

|Kaynaklar|Bağlantı|
|---|---|
|Azure Sentinel (Önizleme)|[https://azure.microsoft.com/services/azure-sentinel](https://azure.microsoft.com/services/azure-sentinel)|
|SıEM belgeleri|SıEM mimarisi ve Kılavuzu için satıcı belgelerine başvurun|
|SıEM araçlarıyla tümleştirme için Azure Izleyicisini kullanın|[https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)|
|

### <a name="australian-cyber-security-centre"></a>Avustralya Cyber Güvenlik Merkezi

Avustralya Siber Güvenlik Merkezi (ACSC), Ulusal Siber Güvenlik lideri olan Avustralya kamu lideri. Avustralya topluluğunun siber esnekliği ve dijital yaş içinde Avustralya 'nın Ekonomik ve sosyal kullanım süresini desteklemek için Avustralya kamu genelindeki bir araya bir birlikte siber güvenlik özelliği sunar. ACSC, Ulusal varlıkların tüm uygulanan sistem tarafından oluşturulan günlük dosyalarını, olayları ve günlükleri Avustralya kamu izlemenin tamamına yönelik ACSC 'ye iletmesini önerir.

|Kaynaklar|Bağlantı|
|---|---|
|Avustralya Cyber Güvenlik Merkezi Web sitesi|[https://www.acsc.gov.au](https://www.acsc.gov.au)|
|

## <a name="incident-response"></a>Olay yanıtı

Uygun günlükleri oluşturma, bunları denetleyebileceği depolarına toplama ve analiz gerçekleştirme sistemleri, sistemlerin anlaşılmasını artırır ve siber güvenlik olaylarını algılamaya yönelik mekanizmalar sağlar. Olaylar veya olaylar algılandıktan sonra, bir sonraki adım, bu olaylara yanıt vermek ve sistem durumunu korumak ve hizmetleri ve verileri tehlikeye atmak için Eylemler gerçekleştirmenize neden olur. Azure, gerçekleşen tüm olaylara etkili bir şekilde yanıt vermeye yönelik bir hizmet kombinasyonu sağlar.

### <a name="azure-alerts"></a>Azure Uyarıları

Azure uyarıları, belirli olaylara yanıt olarak destek ve güvenlik personeline bildirmek için kullanılabilir. Bu, bir kurumdaki varlığın Bu makalede listelenen Analysis Services tarafından oluşturulan ilgili olayları algılamasına yanıt vermesini sağlar.

|Kaynaklar|Bağlantı|
|---|---|
|Microsoft Azure uyarılara genel bakış|[https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-alerts](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-alerts)|
|Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama|[https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)|
|Azure İzleyici Uyarıları ile olaylara yanıt verme|[https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)|
|

### <a name="azure-automation"></a>Azure Otomasyonu

Azure Otomasyonu, kurumlar varlıklarının olaylara yanıt olarak eylemleri tetiklemesine olanak sağlar. Bu, sanal makinelerde bir paket yakalama başlatmak, bir iş akışı çalıştırmak, sanal makineleri veya hizmetleri ya da başka bir görev aralığını başlatmak olabilir. Otomasyon, el ile müdahale olmadan uyarılara hızlı yanıt sağlar, böylece bir olay veya etkinliğin yanıt süresini ve önem derecesini azaltır.

|Kaynaklar|Bağlantı|
|---|---|
|Azure Otomasyonu belgeleri|[https://docs.microsoft.com/azure/automation](https://docs.microsoft.com/azure/automation)|
|Nasıl yapılır Kılavuzu: Bir Azure Otomasyonu runbook 'unu tetiklemek için uyarı kullanma|[https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook)|
|

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da ağ geçidi ortamınızı güvenli bir şekilde yönetme hakkında ayrıntılı bilgi edinmek için [ağ geçidi güvenli uzaktan yönetim](gateway-secure-remote-administration.md) makalesini inceleyin.
