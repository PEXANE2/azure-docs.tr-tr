---
title: Azure geçişi sunucu değerlendirmesi 'nde bağımlılık Analizi
description: Azure geçişi sunucu değerlendirmesi kullanılarak değerlendirme için bağımlılık analizinin nasıl kullanılacağını açıklar.
ms.topic: conceptual
ms.date: 06/14/2020
ms.openlocfilehash: ff563668666207f35fa2ea796d6c909a59df245f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84771351"
---
# <a name="dependency-analysis"></a>Bağımlılık Analizi

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ' nde bağımlılık Analizi açıklanmaktadır.


Bağımlılık Analizi keşfedilen şirket içi makineler arasındaki bağımlılıkları tanımlar. Bu avantajları sağlar: 

- Daha iyi güvenle, daha doğru değerlendirme için makineleri gruplara toplayabilirsiniz.
- Birlikte geçirilmesi gereken makineleri belirleyebilirsiniz. Bu özellikle, hangi makinelerin Azure 'a geçirmek istediğiniz bir uygulama dağıtımının parçası olduğundan emin olmadığınız durumlarda faydalıdır.
- Makinelerin kullanımda olup olmadığını ve geçirilmesi yerine hangi makinelerin kullanımdan hangilerinin kullanılabilir olduğunu belirleyebilirsiniz.
- Bağımlılıkların çözümlenmesi, hiçbir şeyin geri ayrılmadığından ve geçişten sonra beklenmedik kesintilerden kaçınmasının sağlanmasına yardımcı olur.
- Bağımlılık analizi hakkında sık sorulan soruları [gözden geçirin](common-questions-discovery-assessment.md#what-is-dependency-visualization) .


## <a name="analysis-types"></a>Analiz türleri

Bağımlılık analizini dağıtmaya yönelik iki seçenek vardır

**Seçeneği** | **Ayrıntılar** | **Genel bulut** | **Azure Devlet Kurumları**
----  |---- | ---- 
**Aracısız** | VSphere API 'Lerini kullanarak VMware VM 'Lerinden verileri yoklar.<br/><br/> Sanal makinelere aracılar yüklemeniz gerekmez.<br/><br/> Bu seçenek şu anda yalnızca VMware VM 'Leri için önizleme aşamasındadır. | Destekleniyor. | Destekleniyor.
**Aracı tabanlı analiz** | , Bağımlılık görselleştirmesini ve analizini etkinleştirmek için Azure Izleyici 'de [hizmet eşlemesi çözümünü](../azure-monitor/insights/service-map.md) kullanır.<br/><br/> Çözümlemek istediğiniz her şirket içi makineye aracılar yüklemeniz gerekir. | Destekleniyor | Desteklenmiyor.


## <a name="agentless-analysis"></a>Aracısız analiz

Aracısız bağımlılık analizi, etkin olduğu makinelerden gelen TCP bağlantısı verilerini yakalayarak işe yarar. VM 'Lere yüklü aracı yok. Aynı kaynak sunucu ve işlemle bağlantı, hedef sunucu, işlem ve bağlantı noktası, mantıksal olarak bir bağımlılık halinde gruplandırılır. Yakalanan bağımlılık verilerini bir harita görünümünde görselleştirebilir veya bir CSV olarak dışarı aktarabilirsiniz. Çözümlemek istediğiniz makinelere yüklü aracı yok.

### <a name="dependency-data"></a>Bağımlılık verileri

Bağımlılık verilerinin bulunması başladıktan sonra yoklama başlar:

- Azure geçişi gereci, verileri toplamak için makinelerden her beş dakikada bir TCP bağlantı verilerini yoklar.
- Veriler, vSphere API 'Leri kullanılarak vCenter Server aracılığıyla konuk VM 'lerden toplanır.
- Yoklama bu verileri toplar:

    - Etkin bağlantıları olan işlemlerin adı.
    - Etkin bağlantıları olan işlemlerin çalıştırıldığı uygulamanın adı.
    - Etkin bağlantılardaki hedef bağlantı noktası.

- Toplanan veriler, Azure geçişi gereci üzerinde işlenir ve kimlik bilgilerini, her altı saatte bir Azure geçişi 'ne gönderilir


## <a name="agent-based-analysis"></a>Aracı tabanlı analiz

Sunucu değerlendirmesi, aracı tabanlı analizler için Azure Izleyici 'de [hizmet eşlemesi](../azure-monitor/insights/service-map.md) çözümünü kullanır. Çözümlemek istediğiniz her makineye [Microsoft Monitoring Agent/Log Analytics aracısını](../azure-monitor/platform/agents-overview.md#log-analytics-agent) ve [bağımlılık aracısını](../azure-monitor/platform/agents-overview.md#dependency-agent)yüklersiniz.

### <a name="dependency-data"></a>Bağımlılık verileri

Aracı tabanlı analiz bu verileri sağlar:

- Kaynak makine sunucu adı, işlem, uygulama adı.
- Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası.
- Bağlantı sayısı, gecikme süresi ve veri aktarımı bilgilerinin toplanması ve Log Analytics sorguları için kullanılabilir olması. 



## <a name="compare-agentless-and-agent-based"></a>Aracısız ve aracı tabanlı karşılaştırması karşılaştırın

Aracısız görselleştirme ve aracı tabanlı görselleştirme arasındaki farklılıklar tabloda özetlenmiştir.

**Gereksinim** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
**Destek** | Yalnızca VMware VM 'Leri için önizleme aşamasındadır. Desteklenen işletim sistemlerini [gözden geçirin](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) . | Genel kullanılabilirlik (GA).
**Aracısı** | Çözümlemek istediğiniz makinelerde aracı gerekmez. | Çözümlemek istediğiniz her şirket içi makinede aracılar gereklidir.
**Log Analytics** | Gerekli değildir. | Azure geçişi, bağımlılık analizi için [Azure izleyici günlüklerinde](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) [hizmet eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) çözümünü kullanır. 
**İşleme** | TCP bağlantı verilerini yakalar. Bulmadan sonra, verileri beş dakikalık aralıklarla toplar. | Bir makineye yüklenen Hizmet Eşlemesi aracılar, her bir işlem için TCP işlemleri ve gelen/giden bağlantılar hakkında veri toplar.
**Veriler** | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası. | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası.<br/><br/> Bağlantı sayısı, gecikme süresi ve veri aktarımı bilgilerinin toplanması ve Log Analytics sorguları için kullanılabilir olması. 
**Ğiyle** | Tek bir sunucunun bağımlılık eşlemesi, bir saat ile 30 güne kadar bir süre içinde görüntülenebilir. | Tek bir sunucunun bağımlılık eşlemesi.<br/><br/> Bir sunucu grubunun bağımlılık eşlemesi.<br/><br/>  Eşleme, yalnızca bir saat boyunca görüntülenebilir.<br/><br/> Harita görünümünden bir gruptaki sunucuları ekleyin ve kaldırın.
Veri dışarı aktarma | Son 30 günlük veri, CSV biçiminde indirilebilir. | Veriler, Log Analytics ile sorgulanabilir.



## <a name="next-steps"></a>Sonraki adımlar

- Aracı tabanlı bağımlılık görselleştirmesini [ayarlayın](how-to-create-group-machine-dependencies.md) .
- VMware VM 'Leri için aracısız bağımlılık görselleştirmesini [deneyin](how-to-create-group-machine-dependencies-agentless.md) .
- Bağımlılık görselleştirmesine ilişkin [genel soruları](common-questions-discovery-assessment.md#what-is-dependency-visualization) gözden geçirin.


