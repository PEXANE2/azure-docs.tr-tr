---
title: Tanılama günlüğüne kaydetme
titleSuffix: Azure Cognitive Services
description: Bu kılavuz, bir Azure Bilişsel Hizmeti için tanıgünlüğe kaydetmeyi etkinleştirmek için adım adım yönergeler sağlar. Bu günlükler, sorun tanımlama ve hata ayıklama için kullanılan bir kaynağın çalışması hakkında zengin, sık veri sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: article
ms.date: 06/14/2019
ms.author: erhopf
ms.openlocfilehash: 539a35f170b2ee0c94762a30ed9376ca4a416210
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71827908"
---
# <a name="enable-diagnostic-logging-for-azure-cognitive-services"></a>Azure Bilişsel Hizmetler için tanısal günlüğe kaydetmeyi etkinleştirme

Bu kılavuz, bir Azure Bilişsel Hizmeti için tanıgünlüğe kaydetmeyi etkinleştirmek için adım adım yönergeler sağlar. Bu günlükler, sorun tanımlama ve hata ayıklama için kullanılan bir kaynağın çalışması hakkında zengin, sık veri sağlar. Devam etmeden önce, [Bing Web Arama,](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/overview) [Konuşma Hizmetleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview)veya [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis)gibi en az bir Bilişsel Hizmet aboneliğine sahip bir Azure hesabınız olmalıdır.

## <a name="prerequisites"></a>Ön koşullar

Tanılama günlüğünü etkinleştirmek için günlük verilerinizi depolayacak bir yere ihtiyacınız vardır. Bu öğretici, Azure Depolama ve Günlük Analizi'ni kullanır.

* [Azure depolama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) - İlke denetimi, statik çözümleme veya yedekleme için tanılama günlüklerini korur. Depolama hesabı, ayarı yapılandıran kullanıcının her iki aboneye de uygun RBAC erişimine sahip olması nedeniyle kaynak yayan günlüklerle aynı abonelikte olması gerekmez.
* [Log Analytics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics) - Bir Azure kaynağı tarafından oluşturulan ham günlüklerin analizine olanak tanıyan esnek bir günlük arama ve analiz aracı.

> [!NOTE]
> Ek yapılandırma seçenekleri mevcuttur. Daha fazla bilgi edinmek için azure [kaynaklarından günlük verilerini topla ve tüketme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview)bilgisini görün.

## <a name="enable-diagnostic-log-collection"></a>Tanılama günlüğü toplamayı etkinleştirme  

Azure portalını kullanarak tanılama günlüğe kaydetmeyi etkinleştirerek başlayalım.

> [!NOTE]
> PowerShell veya Azure CLI'yi kullanarak bu özelliği etkinleştirmek için, [Azure kaynaklarından veri topla ve bunları tüket'](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview)te sağlanan yönergeleri kullanın.

1. Azure portalına gidin. Ardından, Bilişsel Hizmetler kaynağını bulun ve seçin. Örneğin, Bing Web Arama aboneliğiniz.   
2. Ardından, sol daki gezinme menüsünden **İzleme'yi** bulun ve **Tanılama ayarlarını**seçin. Bu ekran, bu kaynak için daha önce oluşturulmuş tüm tanılama ayarlarını içerir.
3. Kullanmak istediğiniz daha önce oluşturulmuş bir kaynak varsa, şimdi seçebilirsiniz. Aksi takdirde , **seçin + Tanıayar ekleyin**.
4. Ayar için bir ad girin. Ardından **bir depolama hesabına Arşiv'i** seçin ve **Analytics'i günlüğe gönderin.**
5. Yapılandırmanız istendiğinde, tanı günlüklerini depolamak için kullanmak istediğiniz depolama hesabı ve OMS çalışma alanını seçin. **Not**: Depolama hesabınız veya OMS çalışma alanınız yoksa, oluşturmak için istemleri izleyin.
6. **Denetim**, **RequestResponse**ve **AllMetrics'i**seçin. Ardından tanılama günlüğü verilerinizin bekletme süresini ayarlayın. Bekletme ilkesi sıfıra ayarlanırsa, bu günlük kategorisine ait olaylar süresiz olarak depolanır.
7. **Kaydet**'e tıklayın.

Verileri sorgulamak ve çözümlemek için kullanılabilir hale gelmeleri iki saat kadar sürebilir. Eğer hemen bir şey görmüyorsan endişelenme.

## <a name="view-and-export-diagnostic-data-from-azure-storage"></a>Azure Depolama'dan tanılama verilerini görüntüleme ve dışa aktarma

Azure Depolama, büyük miktarda yapılandırılmamış veri depolamak için optimize edilmiş sağlam bir nesne depolama çözümüdür. Bu bölümde, 30 günlük bir zaman dilimi içinde toplam hareketler için depolama hesabınızı sorgulamayı ve verileri excel'e aktarmayı öğreneceksiniz.

1. Azure portalından, son bölümde oluşturduğunuz Azure Depolama kaynağını bulun.
2. Sol daki gezinme menüsünden **İzleme'yi** bulun ve **Ölçümler'i**seçin.
3. Sorgunuzu yapılandırmak için kullanılabilir açılır bırakma işlemlerini kullanın. Bu örnek için, zaman aralığını **Son 30 gün** ve **Hareket**için metrik olarak ayarlayalım.
4. Sorgu tamamlandığında, son 30 gün içinde hareketin görselleştirilmesini görürsünüz. Bu verileri dışa aktarmak için, sayfanın üst kısmında bulunan **Excel'e Dışa** Aktar düğmesini kullanın.

[Azure Depolama'da](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)tanılama verileriyle neler yapabileceğiniz hakkında daha fazla bilgi edinin.

## <a name="view-logs-in-log-analytics"></a>Log Analytics’te günlükleri görüntüleme

Kaynağınız için günlük analitiği verilerini keşfetmek için bu yönergeleri izleyin.

1. Azure portalından, sol daki gezinme menüsünden **Log Analytics'i** bulun ve seçin.
2. Tanılamayı etkinleştirirken oluşturduğunuz kaynağı bulun ve seçin.
3. **Genel**altında , bulun ve **Günlükleri**seçin. Bu sayfadan, günlüklerinize karşı sorguları çalıştırabilirsiniz.

### <a name="sample-queries"></a>Örnek sorgular

Burada, günlük verilerinizi keşfetmek için kullanabileceğiniz birkaç temel Kusto sorgusu verebilirsiniz.

Azure Bilişsel Hizmetleri'nden gelen tüm tanılama günlükleri için bu sorguyan bu sorguyun belirli bir süre boyunca çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
```

En son 10 günlükleri görmek için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| take 10
```

Bu sorguydan **Kaynak'a**göre grup işlemlerine çalıştırın:

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

Her 10'lu için binned sayıları ile OperationName tarafından bölünmüş zaman içinde işlem hacmini görüntülemek için bu sorguyu çalıştırın.

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize count()
by bin(TimeGenerated, 10s), OperationName
| render areachart kind=unstacked
```

## <a name="next-steps"></a>Sonraki adımlar

* Günlüğe kaydetmeyi ve çeşitli Azure hizmetleri tarafından desteklenen ölçümleri ve günlük kategorilerini nasıl etkinleştireceklerini anlamak için, hem Microsoft Azure'daki [ölçümlere Genel Bakış'ı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) hem de [Azure Tanı Günlükleri makalelerine Genel Bakış'ı](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview) okuyun.
* Etkinlik merkezleri hakkında bilgi edinmek için bu makaleleri okuyun:
  * [Azure Event Hubs nedir?](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs)
  * [Event Hubs kullanmaya başlayın](https://docs.microsoft.com/azure/event-hubs/event-hubs-csharp-ephcs-getstarted)
* [Azure Depolama'dan İndirme ölçümleri ve tanılama günlüklerini](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#download-blobs)okuyun.
* [Azure Monitor günlüklerinde günlük aramalarını anla'yı](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-new)okuyun.
