---
title: Ölçümler, uyarılar ve tanılama günlükleri - Azure Toplu İş | Microsoft Dokümanlar
description: Havuzlar ve görevler gibi Azure Toplu Iş hesabı kaynakları için tanılama günlüğü olaylarını kaydedin ve analiz edin.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 12/05/2018
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 68d5976a5a79dbde88b7f80b02b39793ffc86de9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254852"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Tanısal değerlendirme ve izleme için toplu işlem ölçümleri, uyarılar ve günlükler

 
Bu makalede, [Azure Monitor](../azure-monitor/overview.md)özelliklerini kullanarak toplu iş hesabının nasıl izlendiği açıklanmaktadır. Azure Monitor, Toplu İş hesabınızdaki kaynaklar için [ölçümler](../azure-monitor/platform/data-platform-metrics.md) ve [tanılama günlükleri](../azure-monitor/platform/platform-logs-overview.md) toplar. Toplu iş hesabınızı izlemek ve sorunları tanılamak için bu verileri çeşitli şekillerde toplayın ve tüketin. Bir metrik belirli bir değere ulaştığında bildirimleri almak için [metrik uyarıları](../azure-monitor/platform/alerts-overview.md) da yapılandırabilirsiniz. 

## <a name="batch-metrics"></a>Toplu iş ölçümleri

Ölçümler, Azure Monitor hizmeti tarafından tüketilen Azure kaynaklarınız tarafından yayılan Azure telemetri verileridir (performans sayaçları olarak da adlandırılır). Toplu İş hesabındaki örnek ölçümler şunlardır: Havuz Oluşturma Olayları, Düşük Öncelikli Düğüm Sayısı ve Görev Tam Etkinlikleri. 

Desteklenen [Toplu İşlem ölçümlerinin listesine](../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)bakın.

Ölçümler şunlardır:

* Ek yapılandırma olmadan her Toplu Iş hesabında varsayılan olarak etkinleştirildi
* Her 1 dakikada bir oluşturulan
* Otomatik olarak kalıcı değil, ancak 30 günlük bir haddeleme geçmişi var. Tanısal günlüğe kaydetmenin bir parçası olarak etkinlik ölçümlerini devam ettirebilirsiniz.

### <a name="view-metrics"></a>Ölçümleri görüntüle

Toplu Iş hesabınıziçin ölçümleri Azure portalında görüntüleyin. Varsayılan olarak hesabın **Genel Bakış** sayfası anahtar düğüm, çekirdek ve görev ölçümlerini gösterir. 

Tüm Toplu Iş hesabı ölçümlerini görüntülemek için: 

1. Portalda, **Tüm Hizmetler** > **Toplu Hesapları'nı**tıklatın ve ardından Toplu İşlem hesabınızın adını tıklatın.
2. **İzleme**altında, **Ölçümler'i**tıklatın.
3. Ölçümlerden birini veya birkaçını seçin. İsterseniz, **Abonelikler,** **Kaynak grubu,** **Kaynak türü**ve **Kaynak** açılır düşüşlerini kullanarak ek kaynak ölçümleri seçin.
    * Sayım tabanlı ölçümler için ("Özel Çekirdek Sayısı" veya "Düşük Öncelikli Düğüm Sayısı" gibi), "Ortalama" toplamayı kullanın. Olay tabanlı ölçümler için ("Havuz Yeniden Boyutlandırma Tamamlandı Olayları" gibi), "Count" toplamasını kullanın.

> [!WARNING]
> Grafik döneminde alınan tüm veri noktalarının değerlerini bir araya getiren "Toplam" toplama
> 
> 

    ![Batch metrics](media/batch-diagnostics/metrics-portal.png)

Ölçümleri programlı olarak almak için Azure Monitor API'lerini kullanın. Örneğin, [.NET ile Azure Monitörünü Al ölçütlerine](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/)bakın.

## <a name="batch-metric-reliability"></a>Toplu metrik güvenilirlik

Ölçümler eğilim ve veri analizi için kullanılmak üzere tasarlanmıştır. Metrik teslimat garanti edilmez ve sipariş dışı teslimat, veri kaybı ve/veya yinelemeye tabidir. Uyarıları veya tetikleme işlevleri için tek olayların kullanılması önerilmez. Uyarı için eşikleri ayarlama hakkında daha fazla bilgi için [Toplu İşlem metrik uyarıları](#batch-metric-alerts) bölümüne bakın.

Son 3 dakika içinde yayılan ölçümler hala toplanıyor olabilir. Bu süre zarfında, metrik değerler eksik bildirilebilir.

## <a name="batch-metric-alerts"></a>Toplu metrik uyarıları

İsteğe bağlı olarak, belirli bir metnin değeri atadığınız bir eşiği geçtiğinde tetikleyen gerçek zamanlı *metrik uyarıların* yakınında yapılandırın. Uyarı, uyarı "Etkinleştirildiğinde" (eşik geçildiğinde ve uyarı koşulu karşılandığında) ve "Çözüldüğünde" (eşik yeniden aşıldığında ve koşul artık karşılanmadığında) seçtiğiniz bir [bildirim](../monitoring-and-diagnostics/insights-alerts-portal.md) oluşturur. Ölçümler sipariş dışı teslime, veri kaybına ve/veya yinelemeye tabi olduğundan, tek veri noktalarına dayalı uyarı önerilmez. Uyarı, bu tutarsızlıkları hesaba katmak için eşiklerden yararlanmalıdır.

Örneğin, havuzlarınızın kompozisyonunu ayarlamak için düşük öncelikli çekirdek sayınız belirli bir düzeye düştüğünde bir metrik uyarı yapılandırmak isteyebilirsiniz. Ortalama düşük öncelikli çekirdek sayısı tüm dönem için eşik değerinin altına düşerse uyarıların tetiklendiği 10 veya daha fazla dakikalık bir süre ayarlamanız önerilir. Ölçümler hala biraraya gelmek üzere olabileceğiiçin 1-5 dakikalık bir süre içinde uyarı yapılması önerilmez.

Portalda bir metrik uyarı yapılandırmak için:

1. **Tüm Hizmetler** > **Toplu Hesapları'nı**tıklatın ve ardından Toplu İşlem hesabınızın adını tıklatın.
2. **İzleme**altında, **Uyarı kurallarını** > tıklatın**Metrik uyarı ekleyin.**
3. Bir metrik, bir uyarı koşulu (örneğin, bir metrik bir dönem içinde belirli bir değeri aştığında) ve bir veya daha fazla bildirim seçin.

[Ayrıca REST API](https://docs.microsoft.com/rest/api/monitor/)kullanarak yakın bir gerçek zamanlı uyarı yapılandırabilirsiniz. Daha fazla bilgi için [Uyarılara Genel Bakış'a](../azure-monitor/platform/alerts-overview.md)bakın. Uyarılarınıza iş, görev veya havuza özgü bilgileri eklemek [için Azure Monitör Uyarıları ile etkinliklere yanıt verme'deki](../azure-monitor/learn/tutorial-response.md) arama sorgularıyla ilgili bilgilere bakın

## <a name="batch-diagnostics"></a>Batch tanılama

Tanılama günlükleri, her kaynağın çalışmasını açıklayan Azure kaynakları tarafından yayılan bilgileri içerir. Toplu İşlem için aşağıdaki günlükleri toplayabilirsiniz:

* **Hizmet Günlükleri,** bir havuz veya görev gibi tek bir Toplu İşlem kaynağının ömrü boyunca Azure Toplu İş hizmeti tarafından yayılan olayları kaydeder. 

* Ölçümler hesap düzeyinde **günlükleri.** 

Tanılama günlüklerinin toplanmasını etkinleştirmek için ayarlar varsayılan olarak etkinleştirilir. İzlemek istediğiniz her Toplu Iş hesabı için tanılama ayarlarını açıkça etkinleştirin.

### <a name="log-destinations"></a>Günlük hedefleri

Yaygın bir senaryo, günlük hedefi olarak bir Azure Depolama hesabı seçmektir. Günlükleri Azure Depolama'da depolamak için günlüklerin toplanmasını etkinleştirmeden önce hesabı oluşturun. Toplu Iş hesabınızla bir depolama hesabı ilişkilendirirken, bu hesabı günlük hedefi olarak seçebilirsiniz. 

Tanılama günlükleri için diğer isteğe bağlı hedefler:

* Toplu iş tanı günlüğü olaylarını bir [Azure Etkinlik Hub'ına](../event-hubs/event-hubs-what-is-event-hubs.md)aktarın. Olay Hub'ları saniyede milyonlarca olay alabilir ve bu etkinlikleri herhangi bir gerçek zamanlı analiz sağlayıcısını kullanarak dönüştürebilir ve depolayabilirsiniz. 

* Tanılama günlüklerini, Power BI veya Excel'de çözümlemek üzere analiz edebileceğiniz veya dışa aktarabileceğiniz [Azure Monitor günlüklerine](../log-analytics/log-analytics-overview.md)gönderin.

> [!NOTE]
> Tanılama günlüğü verilerini Azure hizmetleriyle depolamak veya işlemek için ek maliyetlere tabi olabilirsiniz. 
>

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Toplu tanı günlüklerinin toplanmasını etkinleştirme

1. Portalda, **Tüm Hizmetler** > **Toplu Hesapları'nı**tıklatın ve ardından Toplu İşlem hesabınızın adını tıklatın.
2. **İzleme**altında, **Tanılama günlüklerini** > tıklatın**Tanılama yı açın.**
3. **Tanılama ayarlarında,** ayar için bir ad girin ve bir günlük hedefi (varolan Depolama hesabı, Olay Hub'ı veya Azure Monitor günlükleri) seçin. **ServiceLog** ve **AllMetrics'lerden**birini veya her ikisini seçin.

    Bir depolama hesabı seçtiğinizde, isteğe bağlı olarak bir bekletme ilkesi ayarlayın. Bekletme için birkaç gün belirtmezseniz, veriler depolama hesabının ömrü boyunca tutulur.

4. **Kaydet**'e tıklayın.

    ![Batch tanılama](media/batch-diagnostics/diagnostics-portal.png)

Günlük koleksiyonunu etkinleştirmek için diğer seçenekler şunlardır: tanılama ayarlarını yapılandırmak için portalda Azure Monitor'u kullanın, [Kaynak Yöneticisi şablonu](../azure-monitor/platform/diagnostic-settings-template.md)kullanın veya Azure PowerShell veya Azure CLI'yi kullanın. bkz. [Azure kaynaklarından günlük verilerini toplayın ve tüketin.](../azure-monitor/platform/platform-logs-overview.md)


### <a name="access-diagnostics-logs-in-storage"></a>Depolama alanında tanılama günlüklerine erişin

Toplu iş tanı günlüklerini bir depolama hesabında arşivlerseniz, ilgili bir olay oluşur oluşmaz depolama hesabında bir depolama kapsayıcısı oluşturulur. Blobs aşağıdaki adlandırma deseni göre oluşturulur:

```
insights-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/
RESOURCEGROUPS/{resource group name}/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/{Batch account name}/y={four-digit numeric year}/
m={two-digit numeric month}/d={two-digit numeric day}/
h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
Örnek:

```
insights-metrics-pt1m/resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/
RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/MYBATCHACCOUNT/y=2018/m=03/d=05/h=22/m=00/PT1H.json
```
Her `PT1H.json` blob dosyası, blob URL'sinde belirtilen saat içinde gerçekleşen JSON biçimli olaylar içerir (örneğin,). `h=12` Şu anda, olaylar oluştuğu nda `PT1H.json` dosyaya eklenir. Tanılama günlüğü`m=00`olayları `00`saatte tek tek blobs bölündüğünden dakika değeri ( ) her zaman. (Tüm zamanlar UTC'dedir.)

Aşağıda, `PoolResizeCompleteEvent` `PT1H.json` günlük dosyasındaki bir giriş örneği verilmiştir. Adanmış ve düşük öncelikli düğümlerin geçerli ve hedef sayısı ile operasyonun başlangıç ve bitiş saati hakkında bilgiler içerir:

```
{ "Tenant": "65298bc2729a4c93b11c00ad7e660501", "time": "2019-08-22T20:59:13.5698778Z", "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/BATCHACCOUNTS/MYBATCHACCOUNT/", "category": "ServiceLog", "operationName": "PoolResizeCompleteEvent", "operationVersion": "2017-06-01", "properties": {"id":"MYPOOLID","nodeDeallocationOption":"Requeue","currentDedicatedNodes":10,"targetDedicatedNodes":100,"currentLowPriorityNodes":0,"targetLowPriorityNodes":0,"enableAutoScale":false,"isAutoPool":false,"startTime":"2019-08-22 20:50:59.522","endTime":"2019-08-22 20:59:12.489","resultCode":"Success","resultMessage":"The operation succeeded"}}
```

Depolama hesabındaki tanılama günlüklerinin şeması hakkında daha fazla bilgi için [Azure Tanı Günlüklerini Arşivle'ye](../azure-monitor/platform/resource-logs-collect-storage.md#schema-of-platform-logs-in-storage-account)bakın. Depolama hesabınızdaki günlüklere programlı olarak erişmek için Depolama API'lerini kullanın. 

### <a name="service-log-events"></a>Hizmet Günlüğü etkinlikleri
Azure Toplu İş Günlükleri, toplandığı takdirde, havuz veya görev gibi tek bir Toplu İşlem kaynağının ömrü boyunca Azure Toplu İş hizmeti tarafından yayılan olayları içerir. Batch tarafından yayılan her olay JSON biçiminde günlüğe kaydedilir. Örneğin, bu bir örnek **havuz oluşturma olay**gövdesi:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "5",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Toplu İşlem hizmeti şu anda aşağıdaki Hizmet Günlüğü olaylarını yayır. Bu makale en son güncelleştirildiğinden ek olaylar eklenebileceğinden, bu liste kapsamlı olmayabilir.

| **Hizmet Günlüğü etkinlikleri** |
| --- |
| [Havuz oluşturma](batch-pool-create-event.md) |
| [Havuz silme başlat](batch-pool-delete-start-event.md) |
| [Havuz silme tamamlandı](batch-pool-delete-complete-event.md) |
| [Havuz yeniden boyutlandırma başlat](batch-pool-resize-start-event.md) |
| [Havuz yeniden boyutlandırma tamamlandı](batch-pool-resize-complete-event.md) |
| [Görev başlangıcı](batch-task-start-event.md) |
| [Görev tamamlandı](batch-task-complete-event.md) |
| [Görev başarısız](batch-task-fail-event.md) |



## <a name="next-steps"></a>Sonraki adımlar

* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* [Toplu Iş çözümlerini izleme](monitoring-overview.md)hakkında daha fazla bilgi edinin.
