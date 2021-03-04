---
title: Mevcut Operations Manager müşterileri için Azure Izleyici
description: Belirli iş yüklerini buluta geçişin bir parçası olarak Azure Izleyici 'ye geçirmeye yönelik Operations Manager mevcut kullanıcıları için rehberlik.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/11/2021
ms.openlocfilehash: 6d92b7c2f01a7e9ef12bc2bb422cfb6ed0076f73
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039385"
---
# <a name="azure-monitor-for-existing-operations-manager-customers"></a>Mevcut Operations Manager müşterileri için Azure Izleyici
Bu makalede, şu anda [System Center Operations Manager](/system-center/scom/welcome) kullananlar ve iş uygulamalarını ve diğer kaynakları Azure 'A geçirirken [Azure izleyici](overview.md) 'ye geçiş planlıyor olan müşteriler için rehberlik sunulmaktadır. Son hedefiniz, Azure Izleyici ile mümkün olduğunca çok Operations Manager işlevselliği, işletmenizin ve BT operasyonel gereksinimlerinizle ödün vermeden değiştirerek buluta tam geçiş olduğunu varsayar. 

Bu makalede yapılan belirli öneriler Azure Izleyici olarak değişir ve Özellik Ekle Operations Manager. Temel strateji, tutarlı olmaya devam edecektir.

> [!IMPORTANT]
> Burada açıklanan çeşitli Azure Izleyici özelliklerinin uygulanması için bir maliyet vardır. bu nedenle, tüm ortamınız genelinde dağıtım yapmadan önce değerlerini değerlendirmelisiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, zaten [Operations Manager](/system-center/scom) kullandığınızı ve en azından [Azure izleyici](overview.md)'nin temel olarak anlaşıldığını varsaymaktadır. İkisi arasındaki tüm karşılaştırmalar için bkz. [bulut izleme Kılavuzu: platformları izlemeye genel bakış](/azure/cloud-adoption-framework/manage/monitor/platform-overview). Bu makalede, aralarında yapılan önerilerin bazılarını anlamanıza yardımcı olmak için iki ile arasındaki belirli özellik farklılıkları ayrıntılı olarak açıklanmaktadır. 


## <a name="general-strategy"></a>Genel strateji
Platformlar temelde farklı olduğundan, Operations Manager varlıkları Azure Izleyici 'ye dönüştürmeye yönelik geçiş araçları yoktur. Geçiş yapmanız bunun yerine, Operations Manager kullanmaya devam ederken [Standart bir Azure izleyici uygulamasını](deploy.md) oluşturur. Azure Izleyicisini farklı uygulamalar ve bileşenler için gereksinimlerinizi karşılayacak şekilde özelleştirdikten ve daha fazla özellik elde ettikten sonra, Operations Manager farklı yönetim paketlerini ve aracılarını devre dışı bırakmaya başlayabilirsiniz.

Bu makalede önerilen genel strateji, buluta aşamalı geçiş yapmanızı sağlayan bir [karma bulut izleme](/azure/cloud-adoption-framework/manage/monitor/cloud-models-monitor-overview#hybrid-cloud-monitoring) stratejisi öneren [bulut izleme Kılavuzu](/azure/cloud-adoption-framework/manage/monitor/)' nda olduğu gibidir. Bazı özellikler çakışsa da, bu strateji yeni platforma daha tanıdık geldiği sürece mevcut iş işlemlerinizi korumanıza olanak sağlar. Yalnızca Operations Manager işlevlerinden uzaklaştığından Azure Izleyici ile değiştirebilirsiniz. Birden çok izleme aracı kullanılması karmaşıklık ekler, ancak Azure Izleyici 'nin bir sonraki nesil bulut iş yüklerini izleyip, şirket içinde veya diğer bulutlarda olabilecek sunucu yazılımını ve altyapı bileşenlerini izleme Operations Manager korurken bir sonraki nesil bulut iş yüklerini izleme özelliğinden yararlanmanızı sağlar. 


## <a name="components-to-monitor"></a>İzlenecek bileşenler
Her biri için ayrı bir izleme stratejisi belirlemek üzere izlemeniz gereken farklı iş yükü türlerini kategorilere ayırmanıza yardımcı olur. [Bulut izleme Kılavuzu: izleme stratejisi](/azure/cloud-adoption-framework/strategy/monitoring-strategy#high-level-modeling) , ortamınızda, eski Kurumsal uygulamalardan bulutta modern uygulamalara kadar ilerlemede izlenmesi gereken farklı katmanların ayrıntılı bir dökümünü sağlar.

Buluttan önce, tüm katmanları izlemek için Operations Manager kullandınız. Bir hizmet olarak altyapı (IaaS) ile geçişinizi başlattığınızda, sanal makineleriniz için Operations Manager kullanmaya devam edersiniz, ancak Azure Izleyici 'yi bulut kaynaklarınız için kullanmaya başlayabilirsiniz. Hizmet olarak platform (PaaS) kullanarak modern uygulamalara daha fazla geçiş yaparken, Azure Izleyici 'de daha fazla odaklanarak Operations Manager işlevselliği devre dışı bırakmaya başlayabilirsiniz.


![Bulut modelleri](/azure/cloud-adoption-framework/strategy/media/monitoring-strategy/cloud-models.png)

Bu katmanlar, bu makalenin geri kalanında açıklanan aşağıdaki kategorilerde basitleştirilebilir. Ortamınızdaki tüm izleme iş yükleri bu kategorilerden birine uygun şekilde sığmayabilir, ancak genel önerilerin uygulanabilmesi için her birinin belirli bir kategoriye yeterince yakın olması gerekir.

**İş uygulamaları.** İşletmenize özgü işlevselliği sağlayan uygulamalar. Bunlar iç veya dış olabilirler ve genellikle özel kod kullanarak dahili olarak geliştirilir. Eski uygulamalarınız genellikle Windows veya Linux çalıştıran sanal veya fiziksel makinelerde barındırılır, daha yeni uygulamalarınız Azure Web Apps ve Azure Işlevleri gibi Azure 'da uygulama hizmetlerini temel alır.

**Azure hizmetleri.** Azure 'da, buluta geçirilmiş iş uygulamalarınızı destekleyen kaynaklar. Bu, Azure depolama, Azure SQL ve Azure IoT gibi hizmetleri içerir. Bu, diğer Azure hizmetleri gibi izlenmekte olduğu için Azure sanal makinelerini de içerir, ancak bu sanal makinelerin Konuk işletim sisteminde çalışan uygulamalar ve yazılımlar konağın ötesinde daha fazla izleme gerektirir.

**Sunucu yazılımı.** İşletmenizin genel işlevlerini sağlayan iş uygulamalarınızı veya paketlenmiş uygulamalarınızı destekleyen sanal ve fiziksel makinelerde çalışan yazılımlar. Internet Information Server (IIS), SQL Server, Exchange ve SharePoint örnekleri sayılabilir. Bu, sanal ve fiziksel makinelerinizdeki Windows veya Linux işletim sistemini de içerir.

**Yerel altyapı.** İzlemeyi gerektiren şirket içi ortamınıza özgü bileşenler. Bu, fiziksel sunucu, depolama ve ağ bileşenleri gibi kaynakları içerir. Bunlar, buluta geçtiğinizde sanallaştırılan bileşenlerdir.

## <a name="sample-walkthrough"></a>Örnek kılavuz
Aşağıda Azure Izleyici 'ye Operations Manager geçişe yönelik kuramsal bir anlatım verilmiştir. Bu, gerçek bir geçişin tam karmaşıklığını temsil etmek üzere tasarlanmamıştır, ancak en azından temel adımları ve sırayı sağlar. Aşağıdaki bölümlerde bu adımların her biri daha ayrıntılı olarak açıklanır.

Herhangi bir bileşeni Azure 'a taşımadan önce ortamınız, şirket içinde veya yönetilen bir hizmet sağlayıcısıyla bulunan sanal ve fiziksel makinelere dayanır. Bu, ortamınızdaki fiziksel sunucular ve ağlar gibi iş uygulamalarını, sunucu yazılımlarını ve diğer altyapı bileşenlerini izlemek için Operations Manager kullanır. IIS, SQL Server ve çeşitli satıcı yazılımları gibi sunucu yazılımları için standart yönetim paketleri kullanır ve bu yönetim paketlerini özel gereksinimlerinize göre ayarlayabilirsiniz. İş uygulamalarınız ve var olan yönetim paketleriyle izlenebilecek diğer bileşenler için özel yönetim paketleri oluşturun ve iş süreçlerinizi destekleyecek şekilde Operations Manager yapılandırabilirsiniz.

Azure 'a geçişiniz IaaS ile başlar ve iş uygulamalarını destekleyen sanal makineler Azure 'a taşınır. Bu uygulamalar ve bunlara bağlı oldukları sunucu yazılımı için izleme gereksinimleri değişmez ve mevcut yönetim paketlerinizdeki bu sunucular üzerinde Operations Manager kullanmaya devam edersiniz. 

Azure Izleyici, Azure hizmeti için Azure aboneliği oluşturandan hemen sonra etkinleştirilir. Platform ölçümlerini ve etkinlik günlüğünü otomatik olarak toplar ve günlük sorgularını kullanarak tüm kullanılabilir Telemetriyi etkileşimli bir şekilde analiz edebilmeniz için kaynak günlüklerini toplanacak şekilde yapılandırırsınız. Sanal makinelerinizde VM öngörülerini, tüm ortamınızda izleme verilerini analiz etmek ve makineler ile süreçler arasındaki ilişkileri bulmanız için etkinleştirirsiniz. Azure Izleyici 'nin etkin olduğu sunucuları etkinleştirerek şirket içi fiziksel ve sanal makinelerinize Azure Izleyici kullanımını genişletebilirsiniz. 

İş uygulamalarınızın her biri için Application Insights etkinleştirirsiniz. Her uygulamanın farklı bileşenlerini tanımlar, kullanım ve performans verilerini toplamaya başlar ve kodda oluşan tüm hataları tanımlar. Dış uygulamalarınızı önceden test etmek ve performans veya kullanılabilirlik sorunları için sizi uyarmak üzere kullanılabilirlik testleri oluşturursunuz. Application Insights, Operations Manager sahip olmadığınız güçlü özellikler sunurken, henüz Azure Izleyici kapsamında olmayan izleme senaryolarını içerdiğinden, iş uygulamalarınız için geliştirdiğiniz özel yönetim paketlerine güvenmeye devam edersiniz. 

Azure Izleyici hakkında daha fazla sahip olduğunuzda, bazı yönetim paketi işlevlerini değiştirecek ve yeni izleme platformunu kullanmak için iş süreçlerinizi gelişmeye başlayabilecek uyarı kuralları oluşturmaya başlayabilirsiniz. Bu, Operations Manager yönetim grubundan makineleri ve yönetim paketlerini kaldırmaya başlayabilmeniz için izin verir. Kritik sunucu yazılımı ve şirket içi altyapı için yönetim paketlerini kullanmaya devam edersiniz, ancak Azure Izleyici 'de ek işlevselliği devre dışı bırakabilmeniz için yeni özellikleri izlemeye devam edersiniz.

## <a name="monitor-azure-services"></a>Azure hizmetlerini izleme
Azure Hizmetleri, Azure Izleyici 'nin telemetri toplamasını gerektirir ve bir Azure aboneliği oluşturduğunuz anda etkindir. [Etkinlik günlüğü](essentials/activity-log.md) , abonelik için otomatik olarak toplanır ve [Platform ölçümleri](essentials/data-platform-metrics.md) oluşturduğunuz tüm Azure kaynaklarından otomatik olarak toplanır. Işlem konsolundaki performans görünümlerine benzer olan [Ölçüm Gezgini](essentials/metrics-getting-started.md)'ni hemen kullanmaya başlayabilirsiniz, ancak etkileşimli analiz ve [Gelişmiş veri toplamaları](essentials/metrics-charts.md) sağlar. Bir değer bir eşiği aştığında veya bir [Azure panosuna görünürlük için grafik eklerken](essentials/metrics-charts.md#pinning-to-dashboards) size bildirimde bulunulması için [bir ölçüm uyarısı oluşturun](alerts/alerts-metric.md) .

[![Ölçüm gezgini](media/azure-monitor-operations-manager/metrics-explorer.png)](media/azure-monitor-operations-manager/metrics-explorer.png#lightbox)

Her bir Azure kaynağı için, her bir kaynağın iç işlemiyle ilgili ayrıntıları bir Log Analytics çalışma alanına veren ölçümleri ve [kaynak günlüklerini](essentials/resource-logs.md)göndermek üzere [bir tanılama ayarı oluşturun](essentials/diagnostic-settings.md) . Bu, kaynaklarınız için tüm kullanılabilir telemetri sağlar ve Operations Manager eşdeğer olmayan gelişmiş bir sorgu dili kullanarak günlük ve performans verilerini etkileşimli olarak analiz etmek için [Log Analytics](logs/log-analytics-overview.md) kullanmanıza olanak sağlar. Ayrıca, uyarı koşullarını tespit etmek ve verileri birden çok kaynak genelinde ilişkilendirmek için karmaşık mantık kullanan [günlük sorgu uyarıları](alerts/alerts-log-query.md)oluşturabilirsiniz.

[![Günlük Analizi](media/azure-monitor-operations-manager/log-analytics.png)](media/azure-monitor-operations-manager/log-analytics.png#lightbox)

Azure Izleyici 'deki [Öngörüler](monitor-reference.md) , belirli bir Azure hizmeti için benzersiz izleme sağlamalarına göre yönetim paketlerine benzerdir. Öngörüler Şu anda ağ, depolama ve kapsayıcılar gibi çeşitli hizmetler için kullanılabilir ve diğerleri sürekli olarak ekleniyor.

[![Öngörü örneği](media/azure-monitor-operations-manager/insight.png)](media/azure-monitor-operations-manager/insight.png#lightbox)


Öngörüler, ölçümleri ve günlük sorgularını zengin etkileşimli raporlarda birleştiren Azure Izleyici 'deki [çalışma kitaplarına](visualize/workbooks-overview.md) dayalıdır. Işletim konsolunda özel görünümler ve raporlar oluşturma gibi çeşitli hizmetlerden gelen verileri birleştirmek için kendi çalışma kitaplarınızı oluşturun.

### <a name="azure-management-pack"></a>Azure yönetim paketi
[Azure yönetim paketi](https://www.microsoft.com/download/details.aspx?id=50013) , Operations Manager Azure kaynaklarını bulmasına ve belirli bir izleme senaryoları kümesine göre sistem durumlarını izlemelerine olanak tanır. Bu yönetim paketi, Azure 'daki her kaynak için ek yapılandırma gerçekleştirmenizi gerektirir, ancak iş işlemlerinizi Azure Izleyici 'ye odaklanmak üzere geliştirene kadar, Işletim konsolundaki Azure kaynaklarınızın bazı görünürlüğünü sağlamak yararlı olabilir.

[![Azure yönetim paketi](media/azure-monitor-operations-manager/operations-console.png)](media/azure-monitor-operations-manager/operations-console.png#lightbox)

 Işletim konsolundaki belirli Azure kaynakları için görünürlük istiyorsanız ve mevcut süreçlerinizle ilgili bazı temel uyarıları tümleştirmeniz durumunda Azure yönetim paketini kullanmayı seçebilirsiniz. Aslında Azure Izleyici tarafından toplanan verileri kullanır. Azure kaynaklarınızın uzun süreli olarak izlenmesi için Azure Izleyici 'ye bakmanız gerekir. 


## <a name="monitor-server-software-and-local-infrastructure"></a>Sunucu yazılımını ve yerel altyapıyı izleme
Makineleri buluta taşıdığınızda, yazılım için izleme gereksinimleri değişmez. Artık sanallaştırılan fiziksel bileşenlerini izlemeniz gerekmez, ancak Konuk işletim sistemi ve iş yükleri ortamlarından bağımsız olarak aynı gereksinimlere sahiptir.

[VM öngörüleri](vm/vminsights-overview.md) , sanal makineleri ve bunların Konuk işletim sistemlerini ve iş yüklerini Izlemek Için Azure izleyici 'deki birincil özelliktir. Operations Manager benzer şekilde, VM öngörüleri, sanal makinelerin Konuk işletim sisteminden veri toplamak için bir aracı kullanır. Bu, genellikle yönetim paketleri tarafından analiz ve uyarı için kullanılan performans ve olay verilerinin aynısını kullanır. Bu makinelerde çalışan iş uygulamaları ve sunucu yazılımlarıyla ilgili sorunları tespit etmek ve uyarmak için önceden mevcut kurallar yoktur. Algılanan sorunlar hakkında önceden bildirimde bulunulması için kendi uyarı kurallarınızı oluşturmanız gerekir.

[![VM öngörüleri performansı](media/azure-monitor-operations-manager/vm-insights-performance.png)](media/azure-monitor-operations-manager/vm-insights-performance.png#lightbox)

Azure Izleyici, bir sanal makinede çalışan farklı uygulama ve hizmetlerin sistem durumunu da ölçmez. Ölçüm uyarıları bir değer eşiğin altına düştüğünde otomatik olarak çözümleyebilir, ancak Azure Izleyici Şu anda makinede çalışan uygulamalar ve hizmetler için sistem durumu ölçütlerini tanımlama özelliğine sahip değilse veya ilgili bileşenlerin sistem durumunu gruplamak için sistem durumu toplaması sağlar.

> [!NOTE]
> [VM öngörüleri için yeni bir konuk sistem durumu özelliği](vm/vminsights-health-overview.md) artık genel önizlemeye sunuldu ve bir dizi performans ölçümlerinin sistem durumunu temel alarak uyarır. Bu, başlangıçta, Konuk işletim sistemiyle ilgili, sanal makinede çalışan uygulamalar veya diğer iş yükleri gibi belirli bir performans sayacı kümesiyle sınırlıdır.
> 
> [![VM öngörüleri Konuk durumu](media/azure-monitor-operations-manager/vm-insights-guest-health.png)](media/azure-monitor-operations-manager/vm-insights-guest-health.png#lightbox)

Bir karma ortamda makinelerinizdeki yazılımı izlemek, her bir makinenin gereksinimlerine ve Azure Izleyici 'de Azure Izleyici 'de oluşan işletimsel işlem süreçlerinize bağlı olarak, genellikle bir VM öngörüleri ve Operations Manager birleşimini kullanır. Microsoft Yönetim Aracısı (Azure Izleyici 'de Log Analytics aracı olarak adlandırılır) her iki platformda de kullanılır, böylece tek bir makine her ikisi tarafından aynı anda izlenebilir.

> [!NOTE]
> Gelecekte VM öngörüleri, şu anda genel önizleme aşamasında olan [Azure izleyici aracısına](agents/azure-monitor-agent-overview.md)geçiş yapacaktır. Aynı sanal makinenin her iki platformda de izlenmeye devam edebilmesi için Microsoft Monitoring Agent ile uyumlu olacaktır.

Henüz Azure Izleyici tarafından sağlansağlanmayan işlevsellik için Operations Manager kullanmaya devam edin. Bu, IIS, SQL Server veya Exchange gibi kritik sunucu yazılımları için yönetim paketleri içerir. Ayrıca, Azure Izleyici ile erişilemeyen şirket içi altyapı için geliştirilmiş özel yönetim paketlermiş olabilirsiniz. Ayrıca, Azure Izleyici ve diğer Azure hizmetlerinin daha fazla veya farklı olduğu hizmet işlemlerinizi modernize geçirebilmeniz için Operations Manager kullanmaya devam edin. 

Operations Manager hemen yerini almasa bile geçerli izlemeyi geliştirmek için Azure Izleyici 'yi kullanın. Azure Izleyici 'ye özgü özelliklere örnek olarak şunlar verilebilir:

- Sanal makineler ve bunların dış bağımlılıkları arasındaki ilişkileri bulur ve izler.
- Toplanan performans verilerini etkileşimli grafiklerde ve çalışma kitaplarında birden çok sanal makine arasında görüntüleyin.
- Diğer Azure kaynaklarınızdaki verilerle sanal makinelerinizdeki Telemetriyi etkileşimli olarak analiz etmek için [günlük sorgularını](logs/log-query-overview.md) kullanın.
- Birden çok sanal makine genelinde karmaşık mantık tabanlı [günlük uyarı kuralları](alerts/alerts-log-query.md) oluşturun.

[![VM öngörüleri Haritası](media/azure-monitor-operations-manager/vm-insights-map.png)](media/azure-monitor-operations-manager/vm-insights-map.png#lightbox)

VM öngörüleri, Azure sanal makinelerinin yanı sıra Şirket içindeki ve diğer bulutlardaki makineleri [Azure Arc etkin sunucularını](../azure-arc/servers/overview.md)kullanarak izleyebilir. Yay özellikli sunucular, Azure dışında, kurumsal ağınızda barındırılan Windows ve Linux makinelerinizi veya yerel Azure sanal makinelerini yönetme ile tutarlı diğer bulut sağlayıcınızı yönetmenizi sağlar.



## <a name="monitor-business-applications"></a>İş uygulamalarını izleme
Genellikle, her bir sanal makineye yüklenen aracılardan yararlanarak Operations Manager ile iş uygulamalarınızı izlemek için özel yönetim paketleri gerekir. Azure Izleyici 'de Application Insights, Azure 'a, diğer bulutlara veya şirket içinde olmalarından bağımsız olarak Web tabanlı uygulamaları izler, böylece Azure 'a geçirilmiş olup olmadıkları bağımsız olarak tüm uygulamalarınız için kullanılabilir.

Bir iş uygulamasını izlemenin, Operations Manager [.NET uygulama performansı şablonu]() tarafından belirtilen işlevlerle sınırlı olması halinde, büyük olasılıkla işlevsellik kaybı olmadan Application Insights geçirebilirsiniz. Aslında Application Insights, aşağıdakiler dahil olmak üzere önemli sayıda ek özellik içerir:

- Uygulama bileşenlerini otomatik olarak bulur ve izler.
- Ayrıntılı uygulama kullanımı ve yanıt süresi, hata oranları ve istek ücretleri gibi performans verilerini toplayın.
- Sayfa görünümleri ve yükleme performansı gibi tarayıcı verilerini toplayın.
- Özel durumları tespit edin ve yığın izlemenin ve ilgili isteklerin detayına gidin.
- [Dağıtılmış izleme](app/distributed-tracing.md) ve [akıllı algılama](app/proactive-diagnostics.md)gibi özellikleri kullanarak gelişmiş analiz gerçekleştirin.
- Performans verilerini etkileşimli olarak çözümlemek için [Ölçüm Gezgini](essentials/metrics-getting-started.md) 'ni kullanın.
- Toplanan telemetrileri Azure hizmetleri ve VM öngörüleri için toplanan verilerle birlikte etkileşimli olarak analiz etmek için [günlük sorgularını](logs/log-query-overview.md) kullanın.

[![Application Insights](media/azure-monitor-operations-manager/application-insights.png)](media/azure-monitor-operations-manager/application-insights.png#lightbox)

Gerekli işlevleri elde edinceye kadar Application Insights ek olarak Operations Manager kullanmaya devam etmeniz gerekebilecek bazı senaryolar vardır. Operations Manager devam etmeniz gerekebilecek örnekler şunlardır:

-  Uygulamalarınızın kullanılabilirlik ve yanıt verme hızını izlemenize ve bunlara göre uyarılabilmeniz için [kullanılabilirlik testleri](app/monitor-web-app-availability.md)Web TESTI aracılarının IP adreslerinden gelen istekleri gerektirir. İlkeniz bu tür erişime izin vermediğinden, Operations Manager [Web uygulaması kullanılabilirlik Izleyicilerini](/system-center/scom/web-application-availability-monitoring-template) kullanmaya devam etmeniz gerekebilir.
- Operations Manager, her 60-120 saniyede bir çok müşteri denetlemesiyle, kullanılabilirlik testleri için herhangi bir yoklama aralığı ayarlayabilirsiniz. Application Insights, bazı müşterilerin çok uzun sürebileceği 5 dakikalık en düşük yoklama aralığına sahiptir.
- Operations Manager ' de önemli miktarda izleme, uygulamalar tarafından oluşturulan olaylar toplanarak ve yerel aracıda komut dosyaları çalıştırılarak gerçekleştirilir. Bunlar Application Insights standart olmayan seçeneklerdir, bu nedenle iş gereksinimlerinizi elde etmek için özel çalışma gerekebilir. Bu, [karma Runbook Worker](../automation/automation-hybrid-runbook-worker.md)kullanan bir Log Analytics çalışma alanında depolanan olay verilerini ve sanal makinelerde başlatılan betiklerin kullanıldığı özel uyarı kuralları içerebilir.
- Uygulamanızın yazıldığı dile bağlı olarak, [Application Insights ile kullanabileceğiniz izleme ile](app/platforms.md)sınırlı olabilirsiniz.

Bu kılavuzun diğer bölümlerindeki temel stratejiyi izleyerek iş uygulamalarınız için Operations Manager kullanmaya devam edin, ancak Application Insights tarafından sunulan ek özelliklerden yararlanın. Kritik işlevselliği Azure Izleyici ile değiştirip, özel yönetim paketlerinizi devre dışı bırakmaya başlayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- Azure Izleyici ve System Center Operations Manager ayrıntılı bir karşılaştırması için [bulut Izleme kılavuzuna](/azure/cloud-adoption-framework/manage/monitor/) ve karma izleme ortamı tasarlama ve uygulama hakkında daha ayrıntılı bilgi için bkz..
- Azure [izleyici 'de Azure kaynaklarını izleme](essentials/monitor-azure-resource.md)hakkında daha fazla bilgi edinin.
- Azure [izleyici 'de Azure sanal makinelerini izleme](vm/monitor-vm-azure.md)hakkında daha fazla bilgi edinin.
- [VM öngörüleri](vm/vminsights-overview.md)hakkında daha fazla bilgi edinin.
- [Application Insights](app/app-insights-overview.md)hakkında daha fazla bilgi edinin.