---
title: Ölçümler, uyarılar ve tanılama günlükleri
description: Havuzlar ve görevler gibi Azure Batch hesabı kaynakları için tanılama günlüğü olaylarını kaydedin ve çözümleyin.
ms.topic: how-to
ms.date: 12/05/2018
ms.custom: seodec18
ms.openlocfilehash: 0a33f71cd185a327bfe6852b9acd7d7317b94c2c
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726749"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Tanılama değerlendirmesi ve izleme için toplu iş ölçümleri, uyarılar ve Günlükler

 
Bu makalede, [Azure izleyici](../azure-monitor/overview.md)'nin özelliklerini kullanarak bir Batch hesabının nasıl izleneceği açıklanır. Azure Izleyici, Batch hesabınızdaki kaynaklar için [ölçümleri](../azure-monitor/platform/data-platform-metrics.md) ve [tanılama günlüklerini](../azure-monitor/platform/platform-logs-overview.md) toplar. Batch hesabınızı izlemek ve sorunları tanılamak için kullanabileceğiniz çeşitli yollarla bu verileri toplayın ve kullanın. Ölçüm [uyarılarını](../azure-monitor/platform/alerts-overview.md) Ayrıca, bir ölçüm belirtilen bir değere ulaştığında bildirimler almanızı sağlayacak şekilde de yapılandırabilirsiniz. 

## <a name="batch-metrics"></a>Toplu iş ölçümleri

Ölçümler, Azure Izleyici hizmeti tarafından tüketilen Azure kaynaklarınız tarafından oluşturulan Azure telemetri verileri (performans sayaçları da denir). Batch hesabındaki örnek ölçümler şunlardır: havuz oluşturma olayları, düşük öncelikli düğüm sayısı ve görev tamamlanma olayları. 

[Desteklenen toplu iş ölçümleri listesine](../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)bakın.

Ölçümler şunlardır:

* Ek yapılandırma olmadan her Batch hesabında varsayılan olarak etkindir
* Her 1 dakikada bir oluşturulur
* Otomatik olarak kalıcı değildir, ancak 30 günlük bir geçmiş geçmişi vardır. Tanılama günlüğü 'nün bir parçası olarak etkinlik ölçümlerini kalıcı hale getirebilirsiniz.

### <a name="view-metrics"></a>Ölçümleri görüntüle

Batch hesabınızın ölçümlerini Azure portal görüntüleyin. Varsayılan olarak hesap için **genel bakış** sayfası anahtar düğümü, çekirdek ve görev ölçümlerini gösterir. 

Tüm Batch hesabı ölçümlerini görüntülemek için: 

1. Portalda, **tüm hizmetler**  >  **Batch hesapları**' na tıklayın ve ardından Batch hesabınızın adına tıklayın.
2. **İzleme**altında **ölçümler**' e tıklayın.
3. Bir veya daha fazla ölçüm seçin. İsterseniz, **abonelikleri**, **kaynak grubunu**, **kaynak türünü**ve **kaynak** açılan listelerini kullanarak ek kaynak ölçümleri ' ni seçin.
    * Count tabanlı ölçümler ("adanmış çekirdek sayısı" veya "düşük öncelikli düğüm sayısı" gibi) için "Ortalama" toplamayı kullanın. Olay tabanlı ölçümler ("havuz yeniden boyutlandırma tam olayları" gibi) için "say" toplamasını kullanın.

> [!WARNING]
> Grafiğin dönemi boyunca alınan tüm veri noktalarının değerlerini ekleyen "Sum" toplamasını kullanmayın
> 
> 

    ![Batch metrics](media/batch-diagnostics/metrics-portal.png)

Ölçümleri programlı bir şekilde almak için Azure Izleyici API 'Lerini kullanın. Örneğin, bkz. [.net Ile Azure izleyici ölçümlerini alma](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/).

## <a name="batch-metric-reliability"></a>Batch ölçüm güvenilirliği

Ölçümler, popüler ve veri analizi için kullanılmak üzere tasarlanmıştır. Ölçüm teslimi garanti edilmez ve sıra dışı teslim, veri kaybı ve/veya yineleme için de tabidir. İşlevleri uyarmak veya tetiklemek için tek olayların kullanılması önerilmez. Uyarı için eşikleri ayarlama hakkında daha fazla bilgi için [Batch ölçüm uyarıları](#batch-metric-alerts) bölümüne bakın.

Son 3 dakika içinde oluşturulan ölçümler yine de toplama olabilir. Bu zaman çerçevesinde, ölçüm değerleri eksik olabilir.

## <a name="batch-metric-alerts"></a>Toplu ölçüm uyarıları

İsteğe bağlı olarak, belirtilen bir ölçümün değeri atadığınız bir eşiği aştığında tetiklenen neredeyse gerçek zamanlı *ölçüm uyarılarını* yapılandırın. Uyarı, uyarı "etkinleştirildiğinde" (eşik geçildiğinde ve uyarı koşulu karşılandığında) ve "çözümlenmiş" olduğunda (eşiğin yeniden geçildiğinde ve koşul artık karşılanamadığında) seçtiğiniz bir [bildirim](../monitoring-and-diagnostics/insights-alerts-portal.md) oluşturur. Ölçümler, sipariş dışı teslim, veri kaybı ve/veya çoğaltmasıyla ilgili olduğundan, tek veri noktalarına dayalı uyarı önerilmez. Uyarı verme, bu tutarsızlıklar için hesap eşikleri kullanmalıdır.

Örneğin, düşük öncelikli çekirdek sayılarınız belirli bir düzeye düştüğünde bir ölçüm uyarısı yapılandırmak isteyebilirsiniz, bu sayede havuzlarınızın birleşimini ayarlayabilirsiniz. Ortalama düşük öncelikli çekirdek sayısı dönemin tamamına ait eşik değerinin altına düşerse, uyarıların tetiklenmesi için 10 veya daha fazla dakikalık bir süre ayarlamanız önerilir. Ölçümler yine de toplamak için 1-5 dakikalık bir dönemde uyarı kullanılması önerilmez.

Portalda bir ölçüm uyarısı yapılandırmak için:

1. **Tüm hizmetler**  >  **toplu iş hesapları**' na tıklayın ve ardından Batch hesabınızın adına tıklayın.
2. **İzleme**altında, **Uyarı kuralları**  >  **ölçüm uyarısı Ekle**' ye tıklayın.
3. Bir ölçüm, bir uyarı koşulu (bir ölçüm bir dönem sırasında belirli bir değeri aştığında olduğu gibi) ve bir veya daha fazla bildirim seçin.

[REST API](https://docs.microsoft.com/rest/api/monitor/)kullanarak neredeyse gerçek zamanlı bir uyarı da yapılandırabilirsiniz. Daha fazla bilgi için bkz. [uyarılara genel bakış](../azure-monitor/platform/alerts-overview.md). Uyarılarınızda işi, görevi veya havuza özel bilgileri eklemek için [Azure Izleyici uyarıları ile olaylara yanıt](../azure-monitor/learn/tutorial-response.md) veren arama sorguları hakkındaki bilgilere bakın

## <a name="batch-diagnostics"></a>Batch tanılama

Tanılama günlükleri, her bir kaynağın çalışmasını tanımlayan Azure kaynakları tarafından yayılan bilgileri içerir. Batch için aşağıdaki günlükleri toplayabilirsiniz:

* Hizmet, havuz veya görev gibi bireysel bir toplu Iş kaynağının ömrü boyunca Azure Batch hizmeti tarafından yayılan olayları **günlüğe kaydeder** . 

* Hesap düzeyinde **ölçümler** günlüğe kaydedilir. 

Tanılama günlüklerinin toplanmasını etkinleştirme ayarları varsayılan olarak etkin değildir. İzlemek istediğiniz her Batch hesabı için tanılama ayarlarını açık bir şekilde etkinleştirin.

### <a name="log-destinations"></a>Günlük hedefleri

Yaygın bir senaryo, günlük hedefi olarak bir Azure depolama hesabı seçmsudur. Günlükleri Azure depolamada depolamak için, günlüklerin toplanmasını etkinleştirmeden önce hesabı oluşturun. Batch hesabınızla bir depolama hesabı ilişkilendirdiyseniz, bu hesabı günlük hedefi olarak seçebilirsiniz. 

Tanılama günlükleri için diğer isteğe bağlı hedefler:

* Batch tanılama günlüğü olaylarını bir [Azure Olay Hub 'ına](../event-hubs/event-hubs-what-is-event-hubs.md)akış. Event Hubs, saniye başına milyonlarca olayı alabilir ve bu sayede herhangi bir gerçek zamanlı analiz sağlayıcısını kullanarak dönüştürebilir ve depolayabilirler. 

* Tanılama günlüklerini [Azure izleyici günlüklerine](../log-analytics/log-analytics-overview.md)göndererek bunları analiz edebilir veya Power BI veya Excel 'de Analize aktarabilirsiniz.

> [!NOTE]
> Azure hizmetleriyle tanılama günlüğü verilerini depolamak veya işlemek için ek ücret ödemeniz gerekebilir. 
>

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Batch tanılama günlüklerinin toplanmasını etkinleştir

1. Portalda, **tüm hizmetler**  >  **Batch hesapları**' na tıklayın ve ardından Batch hesabınızın adına tıklayın.
2. **İzleme**altında **tanılama günlükleri**  >  **Tanılama 'yı aç**' a tıklayın.
3. **Tanılama ayarları**' nda, ayar için bir ad girin ve bir günlük hedefi (mevcut depolama hesabı, Olay Hub 'ı veya Azure izleyici günlükleri) seçin. **ServiceLog** ve **allölçümleri**seçeneklerinden birini veya ikisini birden seçin.

    Bir depolama hesabı seçtiğinizde, isteğe bağlı olarak bir bekletme ilkesi ayarlayın. Bekletme için gün sayısı belirtmezseniz, depolama hesabının ömrü boyunca veriler korunur.

4. **Kaydet**’e tıklayın.

    ![Batch tanılama](media/batch-diagnostics/diagnostics-portal.png)

Günlük toplamayı etkinleştirmek için diğer seçenekler şunlardır: portalda Azure Izleyici 'yi kullanarak tanılama ayarlarını yapılandırın, bir [Kaynak Yöneticisi şablonu](../azure-monitor/platform/diagnostic-settings-template.md)kullanın veya Azure PowerShell ya da Azure CLI kullanın. bkz. [Azure kaynaklarınızdan günlük verilerini toplama ve](../azure-monitor/platform/platform-logs-overview.md)kullanma.


### <a name="access-diagnostics-logs-in-storage"></a>Depolama alanındaki tanılama günlüklerine erişin

Batch tanılama günlüklerini bir depolama hesabında arşivlerseniz, depolama hesabında ilgili bir olay meydana geldiğinde depolama kapsayıcısı oluşturulur. Blob 'lar aşağıdaki adlandırma düzenine göre oluşturulur:

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
Her `PT1H.json` BLOB dosyası, blob URL 'sinde belirtilen saat içinde gerçekleşen JSON biçimli olayları içerir (örneğin, `h=12` ). Mevcut saat boyunca olaylar, `PT1H.json` oluşan sırada dosyaya eklenir. `m=00` `00` Tanılama günlüğü olayları saat başına ayrı bloblara bölündüğü için, dakika değeri () her zaman olur. (Tüm zamanlar UTC 'de bulunur.)

Bir günlük dosyasındaki bir girdinin örneği aşağıda verilmiştir `PoolResizeCompleteEvent` `PT1H.json` . Bu, adanmış ve düşük öncelikli düğümlerin geçerli ve hedef sayısı ile işlemin başlangıç ve bitiş saati hakkında bilgiler içerir:

```
{ "Tenant": "65298bc2729a4c93b11c00ad7e660501", "time": "2019-08-22T20:59:13.5698778Z", "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/BATCHACCOUNTS/MYBATCHACCOUNT/", "category": "ServiceLog", "operationName": "PoolResizeCompleteEvent", "operationVersion": "2017-06-01", "properties": {"id":"MYPOOLID","nodeDeallocationOption":"Requeue","currentDedicatedNodes":10,"targetDedicatedNodes":100,"currentLowPriorityNodes":0,"targetLowPriorityNodes":0,"enableAutoScale":false,"isAutoPool":false,"startTime":"2019-08-22 20:50:59.522","endTime":"2019-08-22 20:59:12.489","resultCode":"Success","resultMessage":"The operation succeeded"}}
```

Depolama hesabındaki tanılama günlüklerinin şeması hakkında daha fazla bilgi için bkz. [Azure tanılama günlüklerini arşivleme](../azure-monitor/platform/resource-logs-collect-storage.md#schema-of-platform-logs-in-storage-account). Depolama hesabınızdaki günlüklere programlı bir şekilde erişmek için depolama API 'Lerini kullanın. 

### <a name="service-log-events"></a>Hizmet günlüğü olayları
Toplanırsa Azure Batch hizmet günlükleri, bir havuz veya görev gibi tekil bir toplu kaynağın ömrü boyunca Azure Batch hizmeti tarafından yayılan olayları içerir. Batch tarafından yayılan her olay JSON biçiminde günlüğe kaydedilir. Örneğin, bu örnek **Havuz oluşturma olayının**gövdesidir:

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

Batch hizmeti şu anda aşağıdaki hizmet günlüğü olaylarını yayar. Bu makale son güncelleştirilme sonrasında ek olaylar eklendiğinden, bu liste kapsamlı olmayabilir.

| **Hizmet günlüğü olayları** |
| --- |
| [Havuz oluşturma](batch-pool-create-event.md) |
| [Havuz silme başlangıcı](batch-pool-delete-start-event.md) |
| [Havuz silme Tamam](batch-pool-delete-complete-event.md) |
| [Havuz yeniden boyutlandırma başlangıcı](batch-pool-resize-start-event.md) |
| [Havuz yeniden boyutlandırma Tamam](batch-pool-resize-complete-event.md) |
| [Görev başlangıcı](batch-task-start-event.md) |
| [Görev tamamlanma](batch-task-complete-event.md) |
| [Görev başarısız](batch-task-fail-event.md) |



## <a name="next-steps"></a>Sonraki adımlar

* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* [Toplu iş çözümlerini izleme](monitoring-overview.md)hakkında daha fazla bilgi edinin.
