---
title: Azure Geçir Sunucu Değerlendirmesinde Bağımlılık analizi
description: Azure Geçir Sunucu Değerlendirmesi'ni kullanarak değerlendirme için bağımlılık çözümlemesi nasıl kullanılacağını açıklar.
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: f96496b66d6bcfd397fb0a7303d3dbfb4fd6f6b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455647"
---
# <a name="dependency-analysis"></a>Bağımlılık analizi

Bu makalede, Azure Geçir:Sunucu Değerlendirmesi'nde bağımlılık çözümlemesi açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Bağımlılık çözümlemesi, değerlendirmek ve Azure'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. 

- Azure Geçir:Sunucu Değerlendirmesi'nde makineleri bir gruba toplar ve grubu değerlendirirsiniz. Bağımlılık çözümlemesi, değerlendirme için yüksek güvenle makineleri daha doğru bir şekilde gruplandırmanıza yardımcı olur.
- Bağımlılık çözümlemesi, birlikte geçirilmesi gereken makineleri belirlemenize olanak tanır. Makinelerin kullanımda olup olmadığını veya geçirilme yerine kullanımdan kaldırılıp kaldırılamayacağını belirleyebilirsiniz.
- Bağımlılıkları çözümleme, geride hiçbir şey kalmamasını sağlamaya ve geçiş sırasında sürpriz kesintileri önlemeye yardımcı olur.
- Analiz, özellikle makinelerin Azure'a geçirmek istediğiniz bir uygulama dağıtımının parçası olup olmadığından emin değilseniz yararlıdır.
- Bağımlılık çözümlemesi ile ilgili sık sorulan soruları [gözden geçirin.](common-questions-discovery-assessment.md#what-is-dependency-visualization)

Bağımlılık çözümlemesi dağıtmak için iki seçenek vardır

- **Aracı tabanlı**: Aracı tabanlı bağımlılık çözümlemesi, analiz etmek istediğiniz her şirket içi makineye aracıların yüklenmesini gerektirir.
- **Agentless**: Aracısız analiz ile çapraz kontrol yapmak istediğiniz makinelere aracı yüklemenize gerek yoktur. Bu seçenek şu anda önizlemededir ve yalnızca VMware VM'leri için kullanılabilir.

> [!NOTE]
> Bağımlılık çözümlemesi Azure Kamu'da kullanılamaz.

## <a name="agentless-analysis"></a>Aracısız analiz

Aracısız bağımlılık çözümlemesi, etkin olduğu makinelerden TCP bağlantı verilerini yakalayarak çalışır. Çözümlemek istediğiniz makinelere aracı yüklenmez.

### <a name="collected-data"></a>Toplanan veriler

Bağımlılık bulma başladıktan sonra, cihaz verileri toplamak için her beş dakikada bir makinelerden veri anketleri. Bu veriler vCenter Server üzerinden konuk VM'lerden vSphere API'leri kullanılarak toplanır. Toplanan veriler, kimlik bilgilerini çıkarmak için Azure Geçir cihazında işlenir ve her altı saatte bir Azure Geçir'e gönderilir.

Yoklama bu verileri makinelerden toplar: 
- Etkin bağlantıları olan işlemlerin adı.
- Etkin bağlantıları olan işlemleri çalıştıran uygulamanın adı.
- Etkin bağlantılardaki hedef bağlantı noktası.

## <a name="agent-based-analysis"></a>Ajan tabanlı analiz

Aracı tabanlı analiz için Sunucu Değerlendirmesi, bağımlılık görselleştirme ve çözümlemesini etkinleştirmek için Azure Monitor'da [Hizmet Haritası çözümünü](../azure-monitor/insights/service-map.md) kullanır. [Microsoft Monitoring Agent/Log Analytics aracısı](../azure-monitor/platform/agents-overview.md#log-analytics-agent) ve [Bağımlılık aracısı,](../azure-monitor/platform/agents-overview.md#dependency-agent)analiz etmek istediğiniz her makineye yüklenmelidir.

### <a name="collected-data"></a>Toplanan veriler

Aracı tabanlı görselleştirme için aşağıdaki veriler toplanır:

- Kaynak makine sunucu adı, işlem, uygulama adı.
- Hedef makine sunucusu adı, işlem, uygulama adı ve bağlantı noktası.
- Bağlantı sayısı, gecikme sonu ve veri aktarım bilgileri toplanır ve Log Analytics sorguları için kullanılabilir. 


## <a name="compare-agentless-and-agent-based"></a>Aracısız ve aracı tabanlı karşılaştırın

Aracısız görselleştirme ile aracı tabanlı görselleştirme arasındaki farklar tabloda özetlenmiştir.

**Gereksinim** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
Destek | Bu seçenek şu anda önizlemededir ve yalnızca VMware VM'leri için kullanılabilir. Desteklenen işletim sistemlerini [gözden geçirin.](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) | Genel kullanılabilirlik (GA).
Aracı | Çapraz kontrol yapmak istediğiniz makinelere aracı yüklemenize gerek yoktur. | Analiz etmek istediğiniz her şirket içi makineye kurulacak aracılar: [Microsoft İzleme aracısı (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)ve [Bağımlılık aracısı.](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent) 
Log Analytics | Gerek yok. | Azure Geçir, bağımlılık çözümlemesi için [Azure Monitor günlüklerinde](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) [Hizmet Haritası](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) çözümünü kullanır. 
Nasıl çalışır? | Bağımlılık görselleştirme için etkinleştirilen makinelerde TCP bağlantı verilerini yakalar. Keşiften sonra, verileri beş dakikalık aralıklarla toplar. | Bir makineye yüklenen Servis Haritası aracıları, her işlem için TCP işlemleri ve gelen/giden bağlantılar hakkında veri toplar.
Veri | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucusu adı, işlem, uygulama adı ve bağlantı noktası. | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucusu adı, işlem, uygulama adı ve bağlantı noktası.<br/><br/> Bağlantı sayısı, gecikme sonu ve veri aktarım bilgileri toplanır ve Log Analytics sorguları için kullanılabilir. 
Görselleştirme | Tek bir sunucunun bağımlılık haritası bir saat ile 30 gün arasında görüntülenebilir. | Tek bir sunucunun bağımlılık haritası.<br/><br/> Harita yalnızca bir saatten fazla görüntülenebilir.<br/><br/> Bir sunucu grubunun bağımlılık haritası.<br/><br/> Gruptaki sunucuları harita görünümünden ekleyin ve kaldırın.
Veri dışarı aktarma | Şu anda tabular biçiminde indirilemez. | Veriler Log Analytics ile sorgulanabilir.



## <a name="next-steps"></a>Sonraki adımlar
- [VMware VM'ler,](migrate-support-matrix-vmware.md#agent-based-dependency-analysis-requirements)fiziksel [sunucular](migrate-support-matrix-physical.md#agent-based-dependency-analysis-requirements)ve [Hyper-V VM'ler](migrate-support-matrix-hyper-v.md#agent-based-dependency-analysis-requirements)için aracı tabanlı analiz ayarlama gereksinimlerini gözden geçirin.
- VMware VM'lerin aracısız analizi için gereksinimleri [gözden geçirin.](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements)
- Aracı tabanlı bağımlılık görselleştirmeyi [ayarlama](how-to-create-group-machine-dependencies.md)
- VMware VM'ler için aracısız bağımlılık görselleştirmeyi [deneyin.](how-to-create-group-machine-dependencies-agentless.md)
- Bağımlılık görselleştirme [sıkça sorulan soruları](common-questions-discovery-assessment.md#what-is-dependency-visualization) gözden geçirin.


