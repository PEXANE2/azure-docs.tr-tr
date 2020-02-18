---
title: Azure geçişi 'nde sık sorulan sorular-bulma, değerlendirme ve bağımlılık Analizi
description: Azure geçişi 'nde bulma, değerlendirme ve bağımlılık analizi hakkında sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 2ae534e50cbee3ebd17d703f957aa7200460dbfe
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77426301"
---
# <a name="common-questions-about-discovery-assessment-and-dependency-analysis"></a>Bulma, değerlendirme ve bağımlılık analizi hakkında sık sorulan sorular

Bu makalede, Azure geçişi 'nde bulma, değerlendirme ve bağımlılık analizleri hakkında sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa, şu makaleleri gözden geçirin:

- Azure geçişi hakkında [genel sorular](resources-faq.md) .
- Azure geçişi gereci hakkında [sorular](common-questions-appliance.md) .
- Sunucu geçişi hakkında [sorular](common-questions-server-migration.md) .
- [Azure geçişi forumuna](https://aka.ms/AzureMigrateForum) soru gönderin



## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Bir gereç ile kaç tane VM keşfedebilirim?

En fazla 10.000 VMware VM, en fazla 5.000 Hyper-V VM ve tek bir gereç ile en fazla 250 sunucusu bulabilirsiniz. Şirket içi ortamınızda daha fazla makineniz varsa, [Hyper-V](scale-hyper-v-assessment.md), [VMware](scale-vmware-assessment.md) ve [fiziksel](scale-physical-assessment.md) değerlendirmesi ölçekleme hakkında bilgi edinin.



## <a name="vm-size-changed-can-i-run-an-assessment-again"></a>VM boyutu değişti. Bir değerlendirmeyi yeniden çalıştırabilir miyim?

Azure geçişi gereci, şirket içi ortam hakkındaki bilgileri sürekli olarak toplar. Ancak değerlendirme, şirket içi VM 'lerin bir zaman noktası anlık görüntüsüdür. Değerlendirmek istediğiniz bir VM 'deki ayarları değiştirirseniz, en son değişikliklerle değerlendirmesi güncelleştirmek için yeniden hesapla seçeneğini kullanın.

### <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Çok kiracılı bir ortamda VM 'Leri Nasıl yaparım?.

- VMware için, ortamınız kiracılar arasında paylaşılmışsa ve başka bir kiracının aboneliğindeki bir kiracının VM 'lerini öğrenmek istemiyorsanız, yalnızca keşfetmesini istediğiniz VM 'Lere erişebilen vCenter Server kimlik bilgileri oluşturun. Daha sonra, Azure geçiş gereci 'nda bulmaya başladığınızda bu kimlik bilgilerini kullanın.
- Hyper-V için bulma, Hyper-V ana bilgisayar kimlik bilgilerini kullanır. VM 'Ler aynı Hyper-V konağını paylaşıyorsa, şu anda bulmayı ayırmanın bir yolu yoktur.  


### <a name="do-i-need-vcenter-server-for-vmware-vm-discovery"></a>VMWare VM keşfi için vCenter Server gerekiyor mu?

Evet, Azure geçişi 'nin bir VMware ortamında bulmayı gerçekleştirmesi vCenter Server gerekir. VCenter Server tarafından yönetilmeyen ESXi konaklarının bulunmasını desteklemez.


## <a name="whats-the-difference-sizing-options"></a>Fark boyutlandırma seçenekleri nelerdir?

Şirket içi olarak boyutlandırılması sayesinde Azure geçişi, değerlendirme için VM performans verilerini kabul etmez. Şirket içi yapılandırmaya bağlı olarak VM boyutlarını değerlendirir. Performans tabanlı boyutlandırma ile boyutlandırma, kullanım verilerini temel alır.

- Örneğin, bir şirket içi VM 'nin %50 CPU kullanımı ve %50 bellek kullanımında 4 çekirdeğe ve 8 GB belleğe sahip olması halinde aşağıdakiler olur:
    - Şirket içi boyutlandırma, 4 çekirdek ve 8 GB bellek içeren bir Azure VM SKU 'SU önerir.
    - Kullanım yüzdesi göz önünde bulundurulduğundan, performans tabanlı boyutlandırma 2 çekirdek ve 4 GB belleğe sahip bir VM SKU 'SU önerir.

- Benzer şekilde, disk boyutlandırma iki değerlendirme özelliklerine bağlıdır: boyutlandırma ölçütleri ve depolama türü.
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

## <a name="do-i-pay-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için ücret ödemem gerekiyor mu?
Hayır. Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="what-do-i-install-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için ne yükleyebilirim?

Bağımlılık görselleştirmesini kullanmak için, değerlendirmek istediğiniz her şirket içi makineye aracıları indirip yüklemeniz gerekir.

Her makineye aşağıdaki aracıları yüklemeniz gerekir:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).
- [Bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent).
- İnternet bağlantısı olmayan makineleriniz varsa Log Analytics ağ geçidini indirip yüklemeniz gerekir.

Bağımlılık görselleştirmesi kullanmadığınız takdirde bu aracılara ihtiyacınız yoktur.

## <a name="can-i-use-an-existing-workspace"></a>Mevcut bir çalışma alanını kullanabilir miyim?

Evet, var olan bir çalışma alanını geçiş projesine iliştirebilir ve bunu bağımlılık görselleştirmesi için kullanabilirsiniz. [Daha fazla bilgi edinin](concepts-dependency-visualization.md#how-does-it-work).

## <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, bağımlılık görselleştirmesi verilemiyor. Bununla birlikte, Azure geçişi Hizmet Eşlemesi kullanır ve bağımlılıkları JSON biçiminde almak için [Hizmet Eşlemesi REST API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) de kullanabilirsiniz.

## <a name="can-i-automate--mmadependency-agent-installation"></a>MMA/bağımlılık Aracısı yüklemesini otomatik hale getirebilir miyim?

[Bağımlılık aracısını yüklemek için bu betiği](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples)kullanın. Komut satırını veya Otomasyonu kullanarak [MMA 'yı yüklemek için bu yönergeleri](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) izleyin. MMA için [bu betiği](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)kullanın.

Betiklerin yanı sıra, aracıları dağıtmak için Microsoft uç nokta Configuration Manager ve [ıntigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) gibi dağıtım araçlarını da kullanabilirsiniz.


## <a name="what-operating-systems-does-mma-support"></a>MMA hangi işletim sistemlerini destekler?

- [MMA tarafından desteklenen Windows işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)listesini görüntüleyin.
- [MMA tarafından desteklenen Linux işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)listesini görüntüleyin.

## <a name="can-i-visualize-dependencies-for-more-than-an-hour"></a>Bir saatten uzun bir süre boyunca bağımlılıkları görselleştirebilir miyim?
Hayır. Bağımlılıkları bir saate kadar görselleştirebilirsiniz. Geçmiş olarak belirli bir tarihe kadar geri dönerek, bir ay olarak geri dönebilirsiniz, ancak görselleştirme için en uzun süre bir saat olabilir. Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresini kullanabilirsiniz, ancak yalnızca bir saatlik pencerenin bağımlılıklarını görüntüleyebilirsiniz. Ancak, Azure Izleyici günlüklerini daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için kullanabilirsiniz.

## <a name="can-visualize-dependencies-for-groups-of-more-than-10-vms"></a>10 ' dan fazla VM grubu için bağımlılıklar görselleştirebilir mi?
10 adede kadar VM içeren gruplar için [bağımlılıkları görselleştirebilirsiniz](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) . 10 ' dan fazla VM 'ye sahip bir grubunuz varsa, grubu daha küçük gruplara bölmeniz ve sonra bağımlılıkları görselleştirmenizi öneririz.




## <a name="next-steps"></a>Sonraki adımlar
[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.
