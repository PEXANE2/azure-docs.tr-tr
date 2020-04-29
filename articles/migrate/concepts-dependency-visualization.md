---
title: Azure geçişi sunucu değerlendirmesi 'nde bağımlılık Analizi
description: Azure geçişi sunucu değerlendirmesi kullanılarak değerlendirme için bağımlılık analizinin nasıl kullanılacağını açıklar.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: f0b956620895ae2264b53916015d440f5e586eb2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82024770"
---
# <a name="dependency-analysis"></a>Bağımlılık Analizi

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ' nde bağımlılık Analizi açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Bağımlılık analizi, değerlendirmek ve Azure 'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. 

- Azure geçişi: Sunucu değerlendirmesi ' nde, makineleri bir gruba toplayın ve sonra grubu değerlendirin. Bağımlılık analizi, makineleri değerlendirme için yüksek güvenilirlikle daha doğru bir şekilde gruplandırmanıza yardımcı olur.
- Bağımlılık analizi, birlikte geçirilmesi gereken makineleri tanımlamanızı sağlar. Makinelerin kullanımda olup olmadığını veya geçişi yerine kullanımdan engellenmeyeceğini belirleyebilirsiniz.
- Bağımlılıkların çözümlenmesi, hiçbir şeyin geri ayrılmaması ve geçiş sırasında beklenmedik kesintilerden kaçınılmasını sağlar.
- Analiz, makinelerin Azure 'a geçirmek istediğiniz bir uygulama dağıtımının parçası olup olmadığından emin olmadığınız durumlarda faydalıdır.
- Bağımlılık analizi hakkında sık sorulan soruları [gözden geçirin](common-questions-discovery-assessment.md#what-is-dependency-visualization) .

Bağımlılık analizini dağıtmaya yönelik iki seçenek vardır

- **Aracı**tabanlı: aracı tabanlı bağımlılık analizi, aracıların çözümlemek istediğiniz her şirket içi makinede yüklü olmasını gerektirir.
- **Aracısız**: aracısız analizler, çapraz denetlemek istediğiniz makinelere aracılar yüklemeniz gerekmez. Bu seçenek şu anda önizleme aşamasındadır ve yalnızca VMware VM 'Leri için kullanılabilir.

> [!NOTE]
> Aracı tabanlı bağımlılık analizi, Azure Kamu 'da kullanılamaz. Aracısız bağımlılık analizini kullanabilirsiniz.

## <a name="agentless-analysis"></a>Aracısız analiz

Aracısız bağımlılık analizi, etkin olduğu makinelerden gelen TCP bağlantısı verilerini yakalayarak işe yarar. Çözümlemek istediğiniz makinelere yüklü aracı yok.

### <a name="collected-data"></a>Toplanan veriler

Bağımlılık keşfi başladıktan sonra, Gereç verileri toplamak için her beş dakikada bir makineden verileri yoklar. Bu veriler, vSphere API 'Leri kullanılarak vCenter Server aracılığıyla konuk VM 'lerden toplanır. Toplanan veriler, Azure geçişi gereci üzerinde işlenir ve kimlik bilgilerini, her altı saatte bir Azure 'a gönderilir.

Yoklama bu verileri makinelerden toplar: 
- Etkin bağlantıları olan işlemlerin adı.
- Etkin bağlantıları olan işlemlerin çalıştırıldığı uygulamanın adı.
- Etkin bağlantılardaki hedef bağlantı noktası.

## <a name="agent-based-analysis"></a>Aracı tabanlı analiz

Sunucu değerlendirmesi, aracı tabanlı analizler için, bağımlılık görselleştirmesini ve analizini etkinleştirmek üzere Azure Izleyici 'de [hizmet eşlemesi çözümünü](../azure-monitor/insights/service-map.md) kullanır. [Microsoft Monitoring Agent/Log Analytics Aracısı](../azure-monitor/platform/agents-overview.md#log-analytics-agent) ve [bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent), çözümlemek istediğiniz her makinede yüklü olmalıdır.

### <a name="collected-data"></a>Toplanan veriler

Aracı tabanlı analizler için aşağıdaki veriler toplanır:

- Kaynak makine sunucu adı, işlem, uygulama adı.
- Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası.
- Bağlantı sayısı, gecikme süresi ve veri aktarımı bilgilerinin toplanması ve Log Analytics sorguları için kullanılabilir olması. 


## <a name="compare-agentless-and-agent-based"></a>Aracısız ve aracı tabanlı karşılaştırması karşılaştırın

Aracısız görselleştirme ve aracı tabanlı görselleştirme arasındaki farklılıklar tabloda özetlenmiştir.

**Gereksinim** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
Destek | Bu seçenek şu anda önizleme aşamasındadır ve yalnızca VMware VM 'Leri için kullanılabilir. Desteklenen işletim sistemlerini [gözden geçirin](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) . | Genel kullanılabilirlik (GA).
Aracı | Çapraz denetlemek istediğiniz makinelere aracı yüklemeye gerek yoktur. | Çözümlemek istediğiniz her şirket içi makineye yüklenecek aracılar: [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)ve [bağımlılık Aracısı](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
Log Analytics | Gerekli değildir. | Azure geçişi, bağımlılık analizi için [Azure izleyici günlüklerinde](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) [hizmet eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) çözümünü kullanır. 
Nasıl çalışır? | Bağımlılık görselleştirmesi için etkinleştirilen makinelerde TCP bağlantı verilerini yakalar. Bulmadan sonra, verileri beş dakikalık aralıklarla toplar. | Bir makineye yüklü Hizmet Eşlemesi aracılar, her bir işlem için TCP işlemleri ve gelen/giden bağlantılarla ilgili verileri toplar.
Veriler | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası. | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası.<br/><br/> Bağlantı sayısı, gecikme süresi ve veri aktarımı bilgilerinin toplanması ve Log Analytics sorguları için kullanılabilir olması. 
Görselleştirme | Tek bir sunucunun bağımlılık eşlemesi, bir saat ile 30 güne kadar bir süre içinde görüntülenebilir. | Tek bir sunucunun bağımlılık eşlemesi.<br/><br/> Eşleme, yalnızca bir saat boyunca görüntülenebilir.<br/><br/> Bir sunucu grubunun bağımlılık eşlemesi.<br/><br/> Harita görünümünden bir gruptaki sunucuları ekleyin ve kaldırın.
Veri dışarı aktarma | Şu anda tablolu biçimde indirilemez. | Veriler, Log Analytics ile sorgulanabilir.



## <a name="next-steps"></a>Sonraki adımlar
- [VMware VM 'leri](migrate-support-matrix-vmware.md#agent-based-dependency-analysis-requirements), [fiziksel sunucular](migrate-support-matrix-physical.md#agent-based-dependency-analysis-requirements)ve [Hyper-V VM 'leri](migrate-support-matrix-hyper-v.md#agent-based-dependency-analysis-requirements)için aracı tabanlı analizler ayarlama gereksinimlerini gözden geçirin.
- VMware VM 'lerinin aracısız analizine yönelik gereksinimleri [gözden geçirin](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) .
- Aracı tabanlı bağımlılık görselleştirmesini [ayarlama](how-to-create-group-machine-dependencies.md)
- VMware VM 'Leri için aracısız bağımlılık görselleştirmesini [deneyin](how-to-create-group-machine-dependencies-agentless.md) .
- Bağımlılık görselleştirmesine ilişkin [genel soruları](common-questions-discovery-assessment.md#what-is-dependency-visualization) gözden geçirin.


