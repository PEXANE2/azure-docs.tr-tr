---
title: Kaynak günlüklerini toplama & analiz edin
description: Azure Kapsayıcı Kayıt Defteri için kimlik doğrulama, görüntü itme ve görüntü çekme gibi kaynak günlüğü olaylarını kaydedin ve analiz edin.
ms.topic: article
ms.date: 01/03/2020
ms.openlocfilehash: 00f9468721126bd166051df47cec1596356e9b54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409652"
---
# <a name="azure-container-registry-logs-for-diagnostic-evaluation-and-auditing"></a>Tanılama değerlendirme ve denetim için Azure Konteyner Kayıt Defteri günlükleri

Bu makalede, [Azure Monitor](../azure-monitor/overview.md)özelliklerini kullanarak bir Azure kapsayıcı kayıt defteri için günlük verilerinin nasıl toplandığı açıklanmaktadır. Azure Monitor, kayıt defterinizdeki kullanıcı tarafından yönlendirilen olaylar için [kaynak günlükleri](../azure-monitor/platform/platform-logs-overview.md) (eski adıyla *tanılama günlükleri*olarak adlandırılır) toplar. Şu gibi gereksinimleri karşılamak için bu verileri toplayın ve tüketin:

* Güvenlik ve uyumluluğu sağlamak için denetim kayıt defteri kimlik doğrulama olayları 

* Kayıt defterinizle operasyonel sorunları tanılayabilmeniz için çekme ve çekme olayları gibi kayıt defteri yapıtlarında tam bir etkinlik izi sağlayın 

Azure Monitor kullanarak kaynak günlüğü verilerinin toplanması ek maliyetlere neden olabilir. Bkz. [Azure Monitör fiyatlandırması.](https://azure.microsoft.com/pricing/details/monitor/) 

## <a name="repository-events"></a>Depo olayları

Görüntüler ve diğer yapılar için aşağıdaki depo düzeyindeki olaylar şu anda günlüğe kaydedilir:

* **Itme olayları**
* **Olayları çekme**
* **Etiketi zıdama olayları**
* **Olayları silme** (depo silme olayları dahil)

Şu anda günlüğe kaydolmayan depo düzeyindeki olaylar: Olayları temizleme.

## <a name="registry-resource-logs"></a>Kayıt defteri kaynak günlükleri

Kaynak günlükleri, azure kaynakları tarafından yayılan ve kendi iç işleyişini açıklayan bilgiler içerir. Bir Azure kapsayıcı kayıt defteri için günlükler, aşağıdaki tablolarda depolanan kimlik doğrulama ve depo düzeyinde olaylar içerir. 

* **ContainerRegistryLoginEvents** - Gelen kimlik ve IP adresi de dahil olmak üzere kayıt defteri kimlik doğrulama olayları ve durumu
* **ContainerRegistryRepositoryEvents** - Kayıt defteri depolarında görüntüler ve diğer yapılar için itme ve çekme gibi işlemler
* **AzureMetrics** - Toplu itme ve çekme sayıları gibi[kayıt defteri ölçümleri.](../azure-monitor/platform/metrics-supported.md#microsoftcontainerregistryregistries)

İşlemler için günlük verileri şunları içerir:
  * Başarı veya başarısızlık durumu
  * Başlangıç ve bitiş zamanı damgaları

Azure, kaynak günlüklerine ek olarak, bir kapsayıcı kayıt defteri oluşturma veya silme gibi Azure yönetim olaylarının tek bir abonelik düzeyi kaydı olan bir [etkinlik günlüğü](../azure-monitor/platform/platform-logs-overview.md)sağlar.

## <a name="enable-collection-of-resource-logs"></a>Kaynak günlüklerinin toplanmasını etkinleştirme

Kapsayıcı kayıt defteri için kaynak günlüklerinin toplanması varsayılan olarak etkinleştirilen değildir. İzlemek istediğiniz her kayıt defteri için tanılama ayarlarını açıkça etkinleştirin. Tanılama ayarlarını etkinleştirme seçenekleri için, [Azure'da platform günlüklerini ve ölçümlerini toplamak için tanı ayarını oluştur'a](../azure-monitor/platform/diagnostic-settings.md)bakın.

Örneğin, Azure Monitor'da bir konteyner kayıt defterinin günlüklerini ve ölçümlerini gerçek zamanlı olarak görüntülemek için, bir Log Analytics çalışma alanında kaynak günlüklerini toplayın. Azure portalını kullanarak bu tanılama ayarını etkinleştirmek için:

1. Zaten bir çalışma alanınız yoksa, [Azure portalını](../azure-monitor/learn/quick-create-workspace.md)kullanarak bir çalışma alanı oluşturun. Veri toplamadaki gecikmeyi en aza indirmek için, çalışma alanının kapsayıcı kayıt defterinizle **aynı bölgede** olduğundan emin olun.
1. Portalda, kayıt defterini seçin ve **Tanılama ayarlarını > İzleme > Tanılama ayarlarını**seçin.
1. Ayarın adını girin ve **Günlük Analitiğine Gönder'i**seçin.
1. Kayıt defteri tanıgünlükleri için çalışma alanını seçin.
1. Toplamak istediğiniz günlük verilerini seçin ve **Kaydet'i**tıklatın.

Aşağıdaki resim, portalı kullanarak bir kayıt defteri için tanılama ayarı oluşturulmasını gösterir.

![Tanılama ayarlarını etkinleştirme](media/container-registry-diagnostics-audit-logs/diagnostic-settings.png)

> [!TIP]
> Maliyeti ve izleme ihtiyaçlarınızı dengeleyerek yalnızca ihtiyacınız olan verileri toplayın. Örneğin, yalnızca kimlik doğrulama olaylarını denetlemeniz gerekiyorsa, yalnızca **ContainerRegistryLoginEvents** günlüğünü seçin. 

## <a name="view-data-in-azure-monitor"></a>Azure Monitör'de verileri görüntüleme

Log Analytics'te tanılama günlüklerinin toplanmasını etkinleştirdikten sonra, verilerin Azure Monitor'da görünmesi birkaç dakika sürebilir. Portaldaki verileri görüntülemek için kayıt defterini seçin ve **İzleme > Günlükleri'ni**seçin. Kayıt defteri için veri içeren tablolardan birini seçin. 

Verileri görüntülemek için sorguları çalıştırın. Birkaç örnek sorgu sağlanır veya kendi sorgunuzu çalıştırın. Örneğin, aşağıdaki sorgu **ContainerYRegistryRepositoryEvents** tablosundan en son 24 saatlik verileri alır:

```Kusto
ContainerRegistryRepositoryEvents
| where TimeGenerated > ago(1d) 
```

Aşağıdaki resimde örnek çıktı gösterilmektedir:

![Günlük verilerini sorgulama](media/container-registry-diagnostics-audit-logs/azure-monitor-query.png)

Azure portalında Log Analytics'i kullanma hakkında bir eğitim için Azure [MonitörÜ Log Analytics'e başlayın](../azure-monitor/log-query/get-started-portal.md)veya Log Analytics [Demo ortamını](https://portal.loganalytics.io/demo)deneyin. 

Günlük sorguları hakkında daha fazla bilgi için [Azure Monitor'daki günlük sorgularına genel bakış](../azure-monitor/log-query/log-query-overview.md)bölümüne bakın.

### <a name="additional-query-examples"></a>Ek sorgu örnekleri

#### <a name="100-most-recent-registry-events"></a>100 en son kayıt defteri olayları

```Kusto
ContainerRegistryRepositoryEvents
| union ContainerRegistryLoginEvents
| top 100 by TimeGenerated
| project TimeGenerated, LoginServer , OperationName , Identity , Repository , DurationMs , Region , ResultType
```

## <a name="additional-log-destinations"></a>Ek günlük hedefleri

Günlükleri Log Analytics'e göndermenin yanı sıra veya alternatif olarak, ortak bir senaryo günlük hedefi olarak bir Azure Depolama hesabı seçmektir. Azure Depolama'da günlükleri arşivlemek için tanılama ayarlarında arşivlemeyi etkinleştirmeden önce bir depolama hesabı oluşturun.

Tanılama günlüğü olaylarını bir [Azure Etkinlik Hub'ına](../event-hubs/event-hubs-what-is-event-hubs.md)da aktarabilirsiniz. Olay Hub'ları saniyede milyonlarca olay alabilir ve bu etkinlikleri herhangi bir gerçek zamanlı analiz sağlayıcısını kullanarak dönüştürebilir ve depolayabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Log Analytics'i](../azure-monitor/log-query/get-started-portal.md) kullanma ve [günlük sorguları](../azure-monitor/log-query/get-started-queries.md)oluşturma hakkında daha fazla bilgi edinin.
* Azure'un farklı katmanlarında kullanılabilen platform günlükleri hakkında bilgi edinmek için [Azure platform günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md) alameti.

