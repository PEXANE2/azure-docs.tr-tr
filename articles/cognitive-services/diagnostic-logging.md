---
title: Tanılama günlüğüne kaydetme
titleSuffix: Azure Cognitive Services
description: Bu kılavuzda bir Azure bilişsel hizmeti için tanılama günlüğünü etkinleştirmek üzere adım adım yönergeler sağlanmaktadır. Bu Günlükler, sorun tanımlama ve hata ayıklama için kullanılan bir kaynağın çalışması hakkında zengin, sık veriler sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: article
ms.date: 06/14/2019
ms.author: erhopf
ms.openlocfilehash: cd380b4e2a7c05f0beedc2ab102b268aa4068f66
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68516364"
---
# <a name="enable-diagnostic-logging-for-azure-cognitive-services"></a>Azure bilişsel hizmetler için tanılama günlüğünü etkinleştirme

Bu kılavuzda bir Azure bilişsel hizmeti için tanılama günlüğünü etkinleştirmek üzere adım adım yönergeler sağlanmaktadır. Bu Günlükler, sorun tanımlama ve hata ayıklama için kullanılan bir kaynağın çalışması hakkında zengin, sık veriler sağlar. Devam etmeden önce, [Bing Web araması](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/overview), [konuşma Hizmetleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview)veya [Luo](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis)gibi en az bir bilişsel hizmetin aboneliğine sahip bir Azure hesabınızın olması gerekir.

## <a name="prerequisites"></a>Önkoşullar

Tanılama günlüğünü etkinleştirmek için, günlük verilerinizi depolamak üzere bir yere ihtiyacınız vardır. Bu öğretici, Azure depolama ve Log Analytics kullanır.

* [Azure depolama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) -ilke denetimi, statik analiz veya yedekleme için tanılama günlüklerini saklar. Ayarı yapılandıran kullanıcının her iki aboneliğe de uygun RBAC erişimi olduğu sürece, depolama hesabının kaynak yayma günlükleriyle aynı abonelikte olması gerekmez.
* [Log Analytics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics) -bir Azure kaynağı tarafından oluşturulan ham günlüklerin analizine izin veren esnek bir günlük araması ve Analiz Aracı.

> [!NOTE]
> Ek yapılandırma seçenekleri kullanılabilir. Daha fazla bilgi edinmek için bkz. [Azure kaynaklarınızdan günlük verilerini toplama ve](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview)kullanma.

## <a name="enable-diagnostic-log-collection"></a>Tanılama günlüğü toplamayı etkinleştir  

Azure portal kullanarak tanılama günlüğünü etkinleştirerek başlayalım.

> [!NOTE]
> Bu özelliği PowerShell veya Azure CLı kullanarak etkinleştirmek için [Azure kaynaklarınızdan günlük verilerini toplama ve](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#diagnostic-settings)kullanma bölümünde sunulan yönergeleri kullanın.

1. Azure portal gidin. Ardından bilişsel hizmetler kaynağını bulup seçin. Örneğin, aboneliğiniz Bing Web Araması.   
2. Ardından, sol taraftaki gezinti menüsünden **izleme** ' yi bulun ve **Tanılama ayarları**' nı seçin. Bu ekran, bu kaynak için daha önce oluşturulmuş tüm tanılama ayarlarını içerir.
3. Kullanmak istediğiniz önceden oluşturulmuş bir kaynak varsa, bunu şimdi seçebilirsiniz. Aksi takdirde **+ Tanılama ayarı Ekle**' yi seçin.
4. Ayar için bir ad girin. Ardından, **bir depolama hesabına arşiv** ' i seçin ve **Log Analytics 'e gönderin**.
5. Yapılandırma istendiğinde, tanılama günlüklerini depolamak için kullanmak istediğiniz depolama hesabı ve OMS çalışma alanını seçin. **Not**: Bir depolama hesabınız veya OMS çalışma alanınız yoksa, bir tane oluşturmak için istemleri izleyin.
6. **Denetim**, **RequestResponse**ve **allölçümleri**' ni seçin. Ardından tanılama günlüğü verilerinize ait saklama süresini ayarlayın. Bir bekletme ilkesi sıfır olarak ayarlandıysa, bu günlük kategorisinin olayları süresiz olarak depolanır.
7. **Kaydet**’e tıklayın.

Günlüğe kaydetme verilerinin sorgu ve analiz için kullanılabilir olması iki saate kadar sürebilir. Hemen hiç şey görmüyorsanız endişelenmeyin.

## <a name="view-and-export-diagnostic-data-from-azure-storage"></a>Azure depolama 'dan tanılama verilerini görüntüleme ve dışarı aktarma

Azure depolama, büyük miktarlarda yapılandırılmamış verileri depolamak için optimize edilmiş sağlam bir nesne depolama çözümüdür. Bu bölümde, depolama hesabınızı 30 günlük zaman dilimi boyunca toplam işlemler için sorgulamayı ve verileri Excel 'e aktarmayı öğreneceksiniz.

1. Azure portal, son bölümde oluşturduğunuz Azure depolama kaynağını bulun.
2. Sol taraftaki gezinti menüsünde, **izleme** ' yi bulun ve **ölçümler**' i seçin.
3. Sorgunuzu yapılandırmak için kullanılabilir açılan listeleri kullanın. Bu örnekte, zaman aralığını **son 30 güne** ve **işleme**ölçüsüne ayarlayalim.
4. Sorgu tamamlandığında, son 30 gün içinde bir işlem görselleştirmesi görürsünüz. Bu verileri dışarı aktarmak için sayfanın üst kısmında bulunan **Excel 'e aktar** düğmesini kullanın.

[Azure depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)'da Tanılama verileri ile yapabilecekleriniz hakkında daha fazla bilgi edinin.

## <a name="view-logs-in-log-analytics"></a>Log Analytics’te günlükleri görüntüleme

Kaynağınızın Log Analytics verilerini araştırmak için bu yönergeleri izleyin.

1. Azure portal, sol taraftaki gezinti menüsünden **Log Analytics** bulun ve seçin.
2. Tanılamayı etkinleştirirken oluşturduğunuz kaynağı bulun ve seçin.
3. **Genel**altında **günlükleri**bulun ve seçin. Bu sayfadan, günlüklerinizi karşılaştırarak sorgular çalıştırabilirsiniz.

### <a name="sample-queries"></a>Örnek sorgular

Günlük verilerinizi araştırmak için kullanabileceğiniz birkaç temel kusto sorgusu aşağıda verilmiştir.

Belirli bir süre boyunca Azure bilişsel hizmetler 'deki tüm tanılama günlükleri için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
```

En son 10 günlüğü görmek için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| take 10
```

İşlemleri **kaynağa**göre gruplamak için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES" |
summarize count() by Resource
```
Bir işlemi gerçekleştirmek için gereken ortalama süreyi bulmak için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize avg(DurationMs)
by OperationName
```

Zaman içinde her 10 s için ayrılmış olan sayımlar tarafından bölünen işlem hacmi görüntülemek için bu sorguyu çalıştırın.

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize count()
by bin(TimeGenerated, 10s), OperationName
| render areachart kind=unstacked
```

## <a name="next-steps"></a>Sonraki adımlar

* Günlük kaydını etkinleştirmeyi ve ayrıca çeşitli Azure hizmetleri tarafından desteklenen ölçümleri ve günlük kategorilerini öğrenmek için, Microsoft Azure içindeki [ölçümlere genel bakışı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) ve [Azure tanılama günlükleri makalelerine genel bakış](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview) makalesini okuyun.
* Event hubs hakkında bilgi edinmek için bu makaleleri okuyun:
  * [Azure Event Hubs nedir?](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs)
  * [Event Hubs kullanmaya başlayın](https://docs.microsoft.com/azure/event-hubs/event-hubs-csharp-ephcs-getstarted)
* Okuma [Azure Depolama'dan ölçümleri ve tanılama günlüklerini indirin](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#download-blobs).
* [Azure izleyici günlüklerinde günlük aramalarını anlayın](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-new).
