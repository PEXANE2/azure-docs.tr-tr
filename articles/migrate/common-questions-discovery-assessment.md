---
title: Azure Geçiş'te keşif, değerlendirme ve bağımlılık analizi ile ilgili sorular
description: Azure Geçiş'te bulma, değerlendirme ve bağımlılık analizi yle ilgili sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 7a63271811053ee2da79f134ac117559e31b0fed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460815"
---
# <a name="discovery-assessment-and-dependency-analysis---common-questions"></a>Keşif, değerlendirme ve bağımlılık analizi - Sık sorulan sorular

Bu makalede, Azure Geçiş'te bulma, değerlendirme ve bağımlılık çözümlemesi ile ilgili sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa, şu kaynakları denetleyin:

- Azure Geçiş hakkında [genel sorular](resources-faq.md)
- [Azure Geçiş cihazı](common-questions-appliance.md) yla ilgili sorular
- Sunucu [geçişi](common-questions-server-migration.md) ile ilgili sorular
- [Azure Geçiş forumunda](https://aka.ms/AzureMigrateForum) soruları yanıtlayın

## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Bir cihazla kaç VM keşfedebilirim?

Tek bir cihaz kullanarak 10.000 VM'ye kadar VM', 5.000'e kadar Hyper-V VM ve 250'ye kadar fiziksel sunucu keşfedebilirsiniz. Daha fazla makineniz varsa, [Bir Hyper-V değerlendirmesini ölçeklendirme,](scale-hyper-v-assessment.md) [VMware değerlendirmesini ölçeklendirme](scale-vmware-assessment.md)veya [fiziksel sunucu değerlendirmesini ölçeklendirme](scale-physical-assessment.md)hakkında bilgi edinin.

## <a name="the-size-of-my-vm-changed-can-i-run-an-assessment-again"></a>VM'min boyutu değişti. Tekrar bir değerlendirme yapabilir miyim?

Azure Geçir cihazı, şirket içi ortam hakkında sürekli olarak bilgi toplar.  Değerlendirme, şirket içi VM'lerin anlık görüntüsüdür. Değerlendirmek istediğiniz bir VM ayarlarını değiştirirseniz, değerlendirmeyi en son değişikliklerle güncelleştirmek için yeniden hesapla seçeneğini kullanın.

## <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Çok kiracılı bir ortamda VM'leri nasıl keşfedebilirim?

- **VMware**: Bir ortam kiracılar arasında paylaşılırsa ve başka bir kiracının aboneliğinde kiracının VM'lerini keşfetmek istemiyorsanız, yalnızca keşfetmek istediğiniz VMM'lere erişebilen VMware vCenter Server kimlik bilgilerini oluşturun. Ardından, Azure Geçiş cihazında keşfetmeye başladığınızda bu kimlik bilgilerini kullanın.
- **Hyper-V**: Discovery Hyper-V ana bilgisayar kimlik bilgilerini kullanır. VM'ler aynı Hyper-V ana bilgisayarını paylaşıyorsa, şu anda keşfi ayırmanın bir yolu yoktur.  

## <a name="do-i-need-vcenter-server"></a>vCenter Server'a ihtiyacım var mı?

Evet, Azure Geçiş i'ni bulma gerçekleştirmek için VMware ortamında vCenter Server gerektirir. Azure Geçir, vCenter Server tarafından yönetilmeyen ESXi ana bilgisayarlarının bulunmasını desteklemez.

## <a name="what-are-the-sizing-options"></a>Boyutlandırma seçenekleri nelerdir?

Şirket içi boyutlandırma ile Azure Geçir, değerlendirme için VM performans verilerini dikkate almaz. Azure Geçir, şirket içi yapılandırmaya göre VM boyutlarını değerlendirir. Performansa dayalı boyutlandırma ile boyutlandırma, kullanım verilerine dayanır.

Örneğin, şirket içi bir VM'de %50 CPU kullanımı ve %50 bellek kullanımında dört çekirdek ve 8 GB bellek varsa:
- Şirket içi boyutlandırma, dört çekirdek ve 8 GB belleğe sahip bir Azure VM SKU önerir.
- Performans tabanlı boyutlandırma, kullanım yüzdesi dikkate alındığından, iki çekirdek ve 4 GB belleğe sahip bir VM SKU önerir.

Benzer şekilde, disk boyutlandırma boyutlandırma ölçütlerine ve depolama türüne bağlıdır:
- Boyutlandırma ölçütleri performansa dayalı ysa ve depolama türü otomatikse, Azure Geçiri hedef disk türünü (Standart veya Premium) tanımlarken IOPS ve diskin üretim değerlerini hesaba katar.
- Boyutlandırma ölçütleri performansa dayalıysa ve depolama türü Premium ise, Azure Geçiş şirket içi diskin boyutuna bağlı olarak bir Premium disk SKU önerir. Boyutlandırma şirket içinde olduğunda ve depolama türü Standart veya Premium olduğunda aynı mantık disk boyutlandırmasına uygulanır.

## <a name="does-performance-history-and-utilization-affect-sizing"></a>Performans geçmişi ve kullanımı boyutlandırmayı etkiler mi?

Evet, performans geçmişi ve kullanımı Azure Geçiş'te boyutlandırmayı etkiler.

### <a name="performance-history"></a>Performans geçmişi

Yalnızca performanstabanlı boyutlandırma için Azure Geçir, şirket içi makinelerin performans geçmişini toplar ve azure'da VM boyutunu ve disk türünü önermek için kullanır:

1. Cihaz, her 20 saniyede bir gerçek zamanlı kullanım verileri toplamak için şirket içi ortamın profilini sürekli olarak kullanır.
1. Cihaz toplanan 20 saniyelik numuneleri toplar ve her 15 dakikada bir tek bir veri noktası oluşturmak için kullanır.
1. Cihaz, veri noktasını oluşturmak için 20 saniyelik örneklerin tümlerinden en yüksek değeri seçer.
1. Cihaz veri noktasını Azure'a gönderir.

### <a name="utilization"></a>Kullanımı

Azure'da performans süresine ve ayarlanan performans geçmişi yüzdelik değerine bağlı olarak bir değerlendirme oluşturduğunuzda, Azure Geçir etkin kullanım değerini hesaplar ve boyutlandırma için kullanır.

Örneğin, performans süresini bir güne ve yüzdelik değeri yüzde 95'e ayarlarsanız, Azure Geçir toplayıcı tarafından son gün için gönderilen 15 dakikalık örnek noktaları artan sırada sırayla sıralar. Etkin kullanım olarak yüzde 95'lik değeri seçer.

95. yüzdelik değerin kullanılması, aykırıların yoksayilmesini sağlar. Azure Geçiş'iniz yüzde 99'u kullanıyorsa, aykırı lar dahil edilebilir. Herhangi bir aykırılık kaçırmadan dönemin en yüksek kullanımını seçmek için Azure Geçir'i yüzde 99'u kullanacak şekilde ayarlayın.

## <a name="how-are-import-based-assessments-different-from-assessments-with-discovery-source-as-appliance"></a>İthalat tabanlı değerlendirmelerin, keşif kaynağının cihaz olarak değerlendirilmesiyle farkı nedir?

Alma tabanlı değerlendirmeler, Bir CSV dosyası kullanılarak Azure'a Geçiş'e aktarılan makinelerle oluşturulan değerlendirmelerdir. Yalnızca dört alanın içe aktarılaması zorunludur: Sunucu adı, çekirdek, bellek ve işletim sistemi. Dikkat edilmesi gereken bazı şeyler şunlardır: 
 - Önyükleme türü parametresi üzerindeki alma tabanlı değerlendirmelerde hazırlık ölçütleri daha az katıdır. Önyükleme türü sağlanmazsa, makinenin BIOS önyükleme türüne sahip olduğu ve makinenin **Koşullu Hazır**olarak işaretlenmediği varsayılır. Cihaz olarak keşif kaynağı ile yapılan değerlendirmelerde, önyükleme türü eksikse hazır olma **durumu Koşullu Olarak** işaretlenir. Hazırlık hesaplamasındaki bu fark, kullanıcıların içe aktarma tabanlı değerlendirmeler yapıldığında geçiş planlamasının erken aşamalarında makinelerle ilgili tüm bilgilere sahip olmayabileceğinden oluşur. 
 - Performansa dayalı alma değerlendirmeleri, doğru boyutlandırma hesaplamaları için kullanıcı tarafından sağlanan kullanım değerini kullanır. Kullanım değeri kullanıcı tarafından sağlandığı için, değerlendirme özelliklerinde **Performans geçmişi** ve **Yüzdelik kullanım** seçenekleri devre dışı bırakılır. Cihaz olarak keşif kaynağı ile yapılan değerlendirmelerde, seçilen yüzdelik değer, cihaz tarafından toplanan performans verilerinden seçilir.

## <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirme nedir?

Bağımlılık görselleştirmesi, daha fazla güvenle geçiş yapmak için VM gruplarını değerlendirmenize yardımcı olabilir. Bağımlılık görselleştirme, bir değerlendirme çalıştırmadan önce makine bağımlılıklarını çapraz denetler. Geride hiçbir şey kalmamasını sağlamaya yardımcı olur ve Azure'a geçiş yaptığınızda beklenmeyen kesintileri önlemeye yardımcı olur. Azure Geçir, bağımlılık görselleştirmesini etkinleştirmek için Azure Monitor'da Hizmet Haritası çözümünü kullanır. [Daha fazla bilgi edinin](concepts-dependency-visualization.md).

> [!NOTE]
> Bağımlılık görselleştirmesi Azure Kamu'da kullanılamaz.

## <a name="whats-the-difference-between-agent-based-and-agentless"></a>Ajan tabanlı ve aracısız arasındaki fark nedir?

Aracısız görselleştirme ile aracı tabanlı görselleştirme arasındaki farklar tabloda özetlenmiştir.

**Gereksinim** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
Destek | Bu seçenek şu anda önizlemededir ve yalnızca VMware VM'leri için kullanılabilir. Desteklenen işletim sistemlerini [gözden geçirin.](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) | Genel kullanılabilirlik (GA).
Aracı | Çapraz kontrol yapmak istediğiniz makinelere aracı yüklemenize gerek yoktur. | Analiz etmek istediğiniz her şirket içi makineye kurulacak aracılar: [Microsoft İzleme aracısı (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)ve [Bağımlılık aracısı.](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent) 
Ön koşullar | Ön koşulları ve dağıtım gereksinimlerini [gözden geçirin.](concepts-dependency-visualization.md#agentless-analysis) | Ön koşulları ve dağıtım gereksinimlerini [gözden geçirin.](concepts-dependency-visualization.md#agent-based-analysis)
Log Analytics | Gerek yok. | Azure Geçir, bağımlılık görselleştirme [süslerinde](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) [Hizmet Haritası](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) çözümünü kullanır. [Daha fazla bilgi edinin](concepts-dependency-visualization.md#agent-based-analysis).
Nasıl çalışır? | Bağımlılık görselleştirme için etkinleştirilen makinelerde TCP bağlantı verilerini yakalar. Keşiften sonra, verileri beş dakikalık aralıklarla toplar. | Bir makineye yüklenen Servis Haritası aracıları, her işlem için TCP işlemleri ve gelen/giden bağlantılar hakkında veri toplar.
Veri | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucusu adı, işlem, uygulama adı ve bağlantı noktası. | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucusu adı, işlem, uygulama adı ve bağlantı noktası.<br/><br/> Bağlantı sayısı, gecikme sonu ve veri aktarım bilgileri toplanır ve Log Analytics sorguları için kullanılabilir. 
Görselleştirme | Tek bir sunucunun bağımlılık haritası bir saat ile 30 gün arasında görüntülenebilir. | Tek bir sunucunun bağımlılık haritası.<br/><br/> Harita yalnızca bir saatten fazla görüntülenebilir.<br/><br/> Bir sunucu grubunun bağımlılık haritası.<br/><br/> Gruptaki sunucuları harita görünümünden ekleyin ve kaldırın.
Veri dışarı aktarma | Şu anda tabular biçiminde indirilemez. | Veriler Log Analytics ile sorgulanabilir.

## <a name="do-i-pay-for-dependency-visualization"></a>Bağımlılık görselleştirme için ödeme yapıyor muyum?

Hayır. [Azure Geçiş fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-migrate/)hakkında daha fazla bilgi edinin.

## <a name="what-do-i-install-for-agent-based-dependency-visualization"></a>Aracı tabanlı bağımlılık görselleştirmesi için ne yüklerim?

Aracı tabanlı bağımlılık görselleştirmesini kullanmak için, değerlendirmek istediğiniz her şirket içi makineye aracıları indirin ve yükleyin:

- [Microsoft İzleme Aracısı (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)
- [Bağımlılık aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent)
- İnternet bağlantısı olmayan makineleriniz varsa, Log Analytics ağ geçidini indirin ve yükleyin.

Bu aracıları yalnızca aracı tabanlı bağımlılık görselleştirmekullanıyorsanız ihtiyacınız vardır.

## <a name="can-i-use-an-existing-workspace"></a>Varolan bir çalışma alanını kullanabilir miyim?

Evet, aracı tabanlı bağımlılık görselleştirmesi için varolan bir çalışma alanını geçiş projesine ekleyebilir ve bağımlılık görselleştirmesi için kullanabilirsiniz. 

## <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, aracı tabanlı görselleştirmedeki bağımlılık görselleştirme raporu dışa aktarılamaz. Ancak Azure Geçir, Hizmet Haritası'nı kullanır ve JSON biçimindeki bağımlılıkları almak için [Hizmet Haritası REST API'sini](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) kullanabilirsiniz.

## <a name="can-i-automate-agent-installation"></a>Aracı yüklemeyi otomatikleştirebilir miyim?

Aracı tabanlı bağımlılık görselleştirmesi için:

- Bağımlılık [aracısını yüklemek için](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples)bir komut dosyası kullanın.
- MMA için [komut satırını veya otomasyonu kullanın](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration)veya bir komut [dosyası](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)kullanın.
- Komut dosyalarına ek olarak, aracıları dağıtmak için Microsoft Endpoint Configuration Manager ve [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) gibi dağıtım araçlarını kullanabilirsiniz.

## <a name="what-operating-systems-does-mma-support"></a>MMA hangi işletim sistemlerini destekler?

- [MMA'nın desteklediği Windows işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)listesini görüntüleyin.
- [MMA'nın desteklediği Linux işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)listesini görüntüleyin.

## <a name="can-i-visualize-dependencies-for-more-than-one-hour"></a>Bağımlılıkları bir saatten fazla görüntüleyebilir miyim?

Aracı tabanlı görselleştirme için, bağımlılıkları bir saate kadar görselleştirebilirsiniz. Tarihte belirli bir tarihe bir ay kadar geri gidebilirsiniz, ancak görselleştirme için maksimum süre bir saattir. Örneğin, bağımlılık haritasındaki süreyi, dünkü bağımlılıkları görüntülemek için kullanabilirsiniz, ancak bağımlılıkları yalnızca bir saatlik bir süre için görüntüleyebilirsiniz. Ancak, bağımlılık verilerini daha uzun bir süre için [sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için Azure Monitor günlüklerini kullanabilirsiniz.

Aracısız görselleştirme için, tek bir sunucunun bağımlılık haritasını bir saat ile 30 gün arasında bir süre içinde görüntüleyebilirsiniz.

## <a name="can-i-visualize-dependencies-for-groups-of-more-than-10-vms"></a>10'dan fazla VM'li gruplar için bağımlılıkları görselleştirebilir miyim?

10 VM'ye kadar olan gruplar için [bağımlılıkları görselleştirebilirsiniz.](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) 10'dan fazla VM'si olan bir grubunuz varsa, grubu daha küçük gruplara bölmenizi ve ardından bağımlılıkları görselleştirmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Azure [Geçiş genel görünümünü](migrate-services-overview.md)okuyun.
