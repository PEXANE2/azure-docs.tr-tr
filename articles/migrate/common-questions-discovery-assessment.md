---
title: Azure geçişi 'nde sık sorulan sorular-bulma, değerlendirme ve bağımlılık Analizi
description: Azure geçişi 'nde bulma, değerlendirme ve bağımlılık analizi hakkında sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 7edc73742b61e4e5e94431c50d14d263bbbea641
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78362253"
---
# <a name="common-questions-about-discovery-assessment-and-dependency-analysis"></a>Bulma, değerlendirme ve bağımlılık analizi hakkında sık sorulan sorular

Bu makalede, Azure geçişi 'nde bulma, değerlendirme ve bağımlılık analizleri hakkında sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa, şu makaleleri gözden geçirin:

- Azure geçişi hakkında [genel sorular](resources-faq.md) .
- Azure geçişi gereci hakkında [sorular](common-questions-appliance.md) .
- Sunucu geçişi hakkında [sorular](common-questions-server-migration.md) .
- [Azure geçişi forumuna](https://aka.ms/AzureMigrateForum) soru gönderin



## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Bir gereç ile kaç tane VM keşfedebilirim?

En fazla 10.000 VMware VM, en fazla 5.000 Hyper-V VM ve tek bir gereç ile en fazla 250 fiziksel sunucu bulabilirsiniz. Daha fazla makineniz varsa, [Hyper-V](scale-hyper-v-assessment.md), [VMware](scale-vmware-assessment.md)ve [fiziksel sunucu](scale-physical-assessment.md) değerlendirmesini ölçeklendirmeyle ilgili makaleleri gözden geçirin.



## <a name="vm-size-changed-can-i-run-an-assessment-again"></a>VM boyutu değişti. Bir değerlendirmeyi yeniden çalıştırabilir miyim?

Azure geçişi gereci şirket içi makinelerle ilgili bilgileri sürekli olarak toplar ve bir değerlendirme, bir zaman noktası anlık görüntüsüdür. Değerlendirmek istediğiniz bir VM 'deki ayarları değiştirirseniz, en son değişikliklerle değerlendirmesi güncelleştirmek için yeniden hesapla seçeneğini kullanın.

## <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Çok kiracılı bir ortamda VM 'Leri Nasıl yaparım?.

- **VMware**: bir ortam kiracılar arasında paylaşılmışsa ve bir kiracının başka bir kiracının aboneliğindeki VM 'leri keşfetmeniz gerekmiyorsa, yalnızca keşfetmeyi Istediğiniz VM 'lere erişebilen vCenter Server kimlik bilgileri oluşturun. Daha sonra, Azure geçiş gereci 'nda bulmaya başladığınızda bu kimlik bilgilerini kullanın.
- **Hyper-v**: bulma Işlemi, Hyper-v ana bilgisayar kimlik bilgilerini kullanır. VM 'Ler aynı Hyper-V konağını paylaşıyorsa, şu anda bulmayı ayırmanın bir yolu yoktur.  


## <a name="do-i-need-vcenter-server"></a>VCenter Server ihtiyacım var mı?

Evet, Azure geçişi 'nin bir VMware ortamında bulmayı gerçekleştirmesi vCenter Server gerekir. VCenter Server tarafından yönetilmeyen ESXi konaklarının bulunmasını desteklemez.


## <a name="whats-are-the-sizing-options"></a>Boyutlandırma seçenekleri nelerdir?

Şirket içi olarak boyutlandırılması sayesinde Azure geçişi, değerlendirme için VM performans verilerini kabul etmez. Şirket içi yapılandırmaya bağlı olarak VM boyutlarını değerlendirir. Performans tabanlı boyutlandırma ile boyutlandırma, kullanım verilerini temel alır.

- Örneğin, bir şirket içi VM 'nin %50 CPU kullanımı ve %50 bellek kullanımı için 4 çekirdeğe ve 8 GB belleğe sahip olması durumunda:
    - Şirket içi boyutlandırma, dört çekirdeğe ve 8 GB belleğe sahip bir Azure VM SKU 'SU önerir.
    - Kullanım yüzdesi göz önünde bulundurulduğundan, performans tabanlı boyutlandırmanın iki çekirdeği ve 4 GB belleğe sahip bir VM SKU 'SU önerilir.

- Benzer şekilde, disk boyutlandırma, boyutlandırma ölçütlerine ve depolama türüne bağlıdır.
    - Boyutlandırma ölçütü performans tabanlıdır ve depolama türü otomatik ise, Azure geçişi, hedef disk türünü (Standart veya Premium) belirlediğinde, diskin ıOPS ve aktarım hızı değerlerini hesaba göre alır.
    - Boyutlandırma ölçütü performans tabanlıdır ve depolama türü Premium ise, Azure geçişi, şirket içi diskin boyutuna bağlı olarak bir Premium disk SKU 'SU önerir. Aynı mantık disk boyutlandırılmasına, boyutlandırma şirket içi olarak olduğunda ve depolama türü standart veya Premium olduğunda uygulanır.

## <a name="does-performance-historyutilization-impact-sizing"></a>Performans geçmişi/kullanım etkisi boyutlandırması mi?

Bu özellikler yalnızca performans tabanlı boyutlandırma için geçerlidir.

- Azure geçişi, şirket içi makinelerin performans geçmişini toplar ve Azure 'da VM boyutunu ve disk türünü önermek için kullanır.
- Gereç, her 20 saniyede bir gerçek zamanlı kullanım verileri toplamak için şirket içi ortamı sürekli olarak profillerdir.
- Gereç, 20 saniyelik örnekleri kaydeder ve 15 dakikada bir tek bir veri noktası oluşturur.
- Veri noktasını oluşturmak için gereç tüm 20 saniyelik örneklerden tepe değeri seçer.
- Gereç bu veri noktasını Azure 'a gönderir.

Azure 'da performans süresi ve performans geçmişi yüzdelik değeri temelinde bir değerlendirme oluşturduğunuzda, Azure geçişi etkin kullanım değerini hesaplar ve boyutlandırmak için kullanır.

- Örneğin, performans süresini bir güne ve yüzdebirlik değerini 95 yüzdebirlik olarak ayarlarsanız Azure geçişi, son gün için toplayıcı tarafından gönderilen 15 dakikalık örnek noktalarını artan düzende sıralar ve 95. yüzdebirlik değerini etkin olarak seçer kullanımı.
- 95. yüzdebirlik değerinin kullanılması, aykırı değerlerin yoksayılmasını sağlar. 99. yüzdebirlik 'i kullanırsanız, dış değerler dahil edilebilir. Herhangi bir aykırı değer olmadan dönem için en yoğun kullanımı seçmek istiyorsanız, 99. yüzdebirlik ' u seçin.

## <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirmesi nedir?

Daha fazla güvenle geçiş için VM gruplarını değerlendirmek üzere bağımlılık görselleştirmesini kullanın. Bağımlılık görselleştirmesi, değerlendirme çalıştırmadan önce makine bağımlılıklarını çapraz denetler. Bu, herhangi bir şeyin gerisinde olmamasını sağlamaya yardımcı olur ve bu nedenle Azure 'a geçiş yaparken beklenmedik kesintilerden kaçınmaya yardımcı olur. Azure geçişi, bağımlılık görselleştirmesini etkinleştirmek için Azure Izleyici 'de Hizmet Eşlemesi çözümünü kullanır. [daha fazla bilgi edinin](concepts-dependency-visualization.md).

> [!NOTE]
> Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.

## <a name="whats-the-difference-between-agent-based-and-agentless"></a>Aracı tabanlı ve aracısız arasındaki fark nedir?

Aracısız görselleştirme ve aracı tabanlı görselleştirme arasındaki farklılıklar tabloda özetlenmiştir.

**Gereksinim** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
Destek | Bu seçenek şu anda önizleme aşamasındadır ve yalnızca VMware VM 'Leri için kullanılabilir. Desteklenen işletim sistemlerini [gözden geçirin](migrate-support-matrix-vmware.md#agentless-dependency-visualization) . | Genel kullanılabilirlik (GA).
Aracı | Çapraz denetlemek istediğiniz makinelere aracı yüklemeye gerek yoktur. | Çözümlemek istediğiniz her şirket içi makineye yüklenecek aracılar: [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)ve [bağımlılık Aracısı](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
Önkoşullar | Önkoşulları ve dağıtım gereksinimlerini [gözden geçirin](concepts-dependency-visualization.md#agentless-visualization) . | Önkoşulları ve dağıtım gereksinimlerini [gözden geçirin](concepts-dependency-visualization.md#agent-based-visualization) .
Log Analytics | Gerekli değildir. | Azure geçişi, bağımlılık görselleştirmesi için [Azure izleyici günlüklerinde](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) [hizmet eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) çözümünü kullanır. [Daha fazla bilgi edinin](concepts-dependency-visualization.md#agent-based-visualization).
Nasıl çalışır? | Bağımlılık görselleştirmesi için etkinleştirilen makinelerde TCP bağlantı verilerini yakalar. Bulmadan sonra, verileri beş dakikalık aralıklarla toplar. | Bir makineye yüklü Hizmet Eşlemesi aracılar, her bir işlem için TCP işlemleri ve gelen/giden bağlantılarla ilgili verileri toplar.
Veriler | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası. | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası.<br/><br/> Bağlantı sayısı, gecikme süresi ve veri aktarımı bilgilerinin toplanması ve Log Analytics sorguları için kullanılabilir olması. 
Görselleştirme | Tek bir sunucunun bağımlılık eşlemesi, bir saat ile 30 güne kadar bir süre içinde görüntülenebilir. | Tek bir sunucunun bağımlılık eşlemesi.<br/><br/> Eşleme, yalnızca bir saat boyunca görüntülenebilir.<br/><br/> Bir sunucu grubunun bağımlılık eşlemesi.<br/><br/> Harita görünümünden bir gruptaki sunucuları ekleyin ve kaldırın.
Veri dışarı aktarma | Şu anda tablolu biçimde indirilemez. | Veriler, Log Analytics ile sorgulanabilir.




## <a name="do-i-pay-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için ücret ödemem gerekiyor mu?
Hayır. Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="what-do-i-install-for-agent-based-dependency-visualization"></a>Aracı tabanlı bağımlılık görselleştirmesi için ne yükleyebilirim?

Aracı tabanlı bağımlılık görselleştirmesini kullanmak için, değerlendirmek istediğiniz her şirket içi makineye aracıları indirip yüklemeniz gerekir.

Her makineye aşağıdaki aracıları yükler:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).
- [Bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent).
- İnternet bağlantısı olmayan makineleriniz varsa Log Analytics ağ geçidini indirip yüklemeniz gerekir.

Aracı tabanlı bağımlılık görselleştirmesi kullanmadığınız takdirde bu aracılara ihtiyacınız yoktur.

## <a name="can-i-use-an-existing-workspace"></a>Mevcut bir çalışma alanını kullanabilir miyim?

Evet, aracı tabanlı bağımlılık görselleştirmesi için, var olan bir çalışma alanını geçiş projesine iliştirebilir ve bunu bağımlılık görselleştirmesi için kullanabilirsiniz. 

## <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, aracı tabanlı görselleştirmede bağımlılık görselleştirme raporu verilemez. Bununla birlikte, Azure geçişi Hizmet Eşlemesi kullanır ve bağımlılıkları JSON biçiminde almak için [Hizmet Eşlemesi REST API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) de kullanabilirsiniz.

## <a name="can-i-automate-agent-installation"></a>Aracı yüklemesini otomatikleştirebilirim miyim?
Aracı tabanlı bağımlılık görselleştirmesi için aşağıdaki gibi otomatikleştirin:

- [Bağımlılık aracısını yüklemek için bu betiği](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples)kullanın.
- MMA için, komut satırını veya Otomasyonu kullanmak için bu [yönergeleri](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration) izleyin veya [bu betiği](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)kullanın.
- Betiklerin yanı sıra, aracıları dağıtmak için Microsoft uç nokta Configuration Manager ve [ıntigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) gibi dağıtım araçlarını da kullanabilirsiniz.


## <a name="what-operating-systems-does-mma-support"></a>MMA hangi işletim sistemlerini destekler?

- [MMA tarafından desteklenen Windows işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)listesini görüntüleyin.
- [MMA tarafından desteklenen Linux işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)listesini görüntüleyin.

## <a name="can-i-visualize-dependencies-for-more-than-an-hour"></a>Bir saatten uzun bir süre boyunca bağımlılıkları görselleştirebilir miyim?
Aracı tabanlı görselleştirme için, bağımlılıkları bir saate kadar görselleştirebilirsiniz. Geçmiş olarak belirli bir tarihe kadar geri dönerek, bir ay olarak geri dönebilirsiniz, ancak görselleştirme için en uzun süre bir saat olabilir. Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresini kullanabilirsiniz, ancak yalnızca bir saatlik pencerenin bağımlılıklarını görüntüleyebilirsiniz. Ancak, Azure Izleyici günlüklerini daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için kullanabilirsiniz.

Aracısız görselleştirme için, tek bir sunucunun bağımlılık haritasını bir saat ile 30 gün arasında bir süre içinde görüntüleyebilirsiniz.


## <a name="can-visualize-dependencies-for-groups-of-more-than-10-vms"></a>10 ' dan fazla VM grubu için bağımlılıklar görselleştirebilir mi?
10 adede kadar VM içeren gruplar için [bağımlılıkları görselleştirebilirsiniz](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) . 10 ' dan fazla VM 'ye sahip bir grubunuz varsa, grubu daha küçük gruplara bölmeniz ve sonra bağımlılıkları görselleştirmenizi öneririz.




## <a name="next-steps"></a>Sonraki adımlar
[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.
