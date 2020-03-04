---
title: Ölçümler, uyarılar ve tanılama günlükleri-Azure Batch | Microsoft Docs
description: Kayıt ve tanılama günlüğü olaylarını havuzlar ve görevler gibi Azure Batch hesabı kaynakları için analiz edin.
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
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78254852"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Toplu ölçümleri, uyarılar ve değerlendirme tanılama ve izleme günlükleri

 
Bu makalede, [Azure izleyici](../azure-monitor/overview.md)'nin özelliklerini kullanarak bir Batch hesabının nasıl izleneceği açıklanır. Azure Izleyici, Batch hesabınızdaki kaynaklar için [ölçümleri](../azure-monitor/platform/data-platform-metrics.md) ve [tanılama günlüklerini](../azure-monitor/platform/platform-logs-overview.md) toplar. Toplama ve Batch hesabınızı izleme ve sorunlarını tanılamak için yol çeşitli bu verileri kullanır. Ölçüm [uyarılarını](../azure-monitor/platform/alerts-overview.md) Ayrıca, bir ölçüm belirtilen bir değere ulaştığında bildirimler almanızı sağlayacak şekilde de yapılandırabilirsiniz. 

## <a name="batch-metrics"></a>Toplu ölçümleri

(Performans sayaçları olarak da bilinir) Azure telemetri verilerini Azure İzleyici hizmeti tarafından kullanılan, Azure kaynaklarınızı tarafından yayılan ölçümleridir. Bir Batch hesabında örnek ölçümlerini dahil et: havuzu oluşturma olayları, düşük öncelikli düğüm sayısı ve görevin tam olayları. 

[Desteklenen toplu iş ölçümleri listesine](../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)bakın.

Ölçümler şunlardır:

* Her Batch hesabında bir ek yapılandırma olmadan varsayılan olarak etkin
* Her 1 dakikada oluşturulan
* Otomatik olarak kalıcı değil, ancak 30 günlük çalışırken geçmişi bulunur. Tanılama günlüğü 'nün bir parçası olarak etkinlik ölçümlerini kalıcı hale getirebilirsiniz.

### <a name="view-metrics"></a>Ölçümleri görüntüle

Azure portalında Batch hesabı için ölçümleri görüntüleyin. Varsayılan olarak hesap için **genel bakış** sayfası anahtar düğümü, çekirdek ve görev ölçümlerini gösterir. 

Tüm Batch hesabı ölçümleri görüntülemek için: 

1. Portalda, **tüm hizmetler** > **Batch hesapları**' na ve ardından Batch hesabınızın adına tıklayın.
2. **İzleme**altında **ölçümler**' e tıklayın.
3. Bir veya daha fazla ölçüm seçin. İsterseniz, **abonelikleri**, **kaynak grubunu**, **kaynak türünü**ve **kaynak** açılan listelerini kullanarak ek kaynak ölçümleri ' ni seçin.
    * Count tabanlı ölçümler ("adanmış çekirdek sayısı" veya "düşük öncelikli düğüm sayısı" gibi) için "Ortalama" toplamayı kullanın. Olay tabanlı ölçümler ("havuz yeniden boyutlandırma tam olayları" gibi) için "say" toplamasını kullanın.

> [!WARNING]
> Grafiğin dönemi boyunca alınan tüm veri noktalarının değerlerini ekleyen "Sum" toplamasını kullanmayın
> 
> 

    ![Batch metrics](media/batch-diagnostics/metrics-portal.png)

Program aracılığıyla ölçümleri almak için Azure İzleyici API'lerini kullanın. Örneğin, bkz. [.net Ile Azure izleyici ölçümlerini alma](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/).

## <a name="batch-metric-reliability"></a>Batch ölçüm güvenilirlik

Ölçümler, eğilimleri belirlemek ve veri analizi için kullanılacak yöneliktir. Ölçüm teslimi garanti edilmez ve sırası teslim, veri kaybı ve/veya çoğaltma tabidir. Uyarı veya tetikleyici işlevleri tek olayları kullanılması önerilmez. Uyarı için eşikleri ayarlama hakkında daha fazla bilgi için [Batch ölçüm uyarıları](#batch-metric-alerts) bölümüne bakın.

Son 3 dakika içinde gösterilen ölçümleri hala toplama. Bu zaman çerçevesi sırasındaki ölçüm değerleri eksik.

## <a name="batch-metric-alerts"></a>Batch ölçüm uyarıları

İsteğe bağlı olarak, belirtilen bir ölçümün değeri atadığınız bir eşiği aştığında tetiklenen neredeyse gerçek zamanlı *ölçüm uyarılarını* yapılandırın. Uyarı, uyarı "etkinleştirildiğinde" (eşik geçildiğinde ve uyarı koşulu karşılandığında) ve "çözümlenmiş" olduğunda (eşiğin yeniden geçildiğinde ve koşul artık karşılanamadığında) seçtiğiniz bir [bildirim](../monitoring-and-diagnostics/insights-alerts-portal.md) oluşturur. Ölçümleri sırası teslim, veri kaybı ve/veya çoğaltma tabi olarak tek bir veri noktasının temel uyarı önerilmez. Uyarı olmalısınız eşiklerini bu tutarsızlıkları hesap için kullanın.

Örneğin, düşük öncelikli çekirdek sayısı, belirli bir düzeyde, düştüğünde havuzları, oluşumunu ayarlayabilmeniz için ölçüm uyarısı yapılandırmak isteyebilirsiniz. Burada, ortalama düşük öncelikli çekirdek sayısı tüm dönem için Eşik değerin altına düşerse uyarı tetikleyen bir süre en az 10 dakika ayarlamak için önerilir. 1-5 dakikalık bir süre üzerinde ölçümleri hala toplama gibi uyarmak için önerilmez.

Ölçüm uyarısı portalında yapılandırmak için:

1. **Tüm hizmetler** > **Batch hesapları** seçeneğine ve sonra da Batch hesabınızın adına tıklayın.
2. **İzleme**altında, **ölçüm uyarısı Ekle** > **Uyarı kuralları** ' na tıklayın.
3. Bir ölçüm, bir uyarı durumu (örneğin, bir ölçüm belirli bir değerin bir süre boyunca aştığında) ve bir veya daha fazla bildirim seçin.

[REST API](https://docs.microsoft.com/rest/api/monitor/)kullanarak neredeyse gerçek zamanlı bir uyarı da yapılandırabilirsiniz. Daha fazla bilgi için bkz. [uyarılara genel bakış](../azure-monitor/platform/alerts-overview.md). Uyarılarınızda işi, görevi veya havuza özel bilgileri eklemek için [Azure Izleyici uyarıları ile olaylara yanıt](../azure-monitor/learn/tutorial-response.md) veren arama sorguları hakkındaki bilgilere bakın

## <a name="batch-diagnostics"></a>Batch tanılama

Tanılama günlükleri, işlemi her bir kaynağın açıklayan Azure kaynakları tarafından gösterilen bilgileri içerir. Batch için aşağıdaki günlüklere toplayabilirsiniz:

* Hizmet, havuz veya görev gibi bireysel bir toplu Iş kaynağının ömrü boyunca Azure Batch hizmeti tarafından yayılan olayları **günlüğe kaydeder** . 

* Hesap düzeyinde **ölçümler** günlüğe kaydedilir. 

Tanılama günlüklerinin toplanmasını etkinleştirmek için ayarlar varsayılan olarak etkin değildir. Açıkça izlemek istediğiniz her bir Batch hesabı tanılama ayarlarını etkinleştirme.

### <a name="log-destinations"></a>Günlük hedefleri

Günlük hedefi bir Azure depolama hesabını seçin yaygın bir senaryodur. Azure Depolama'da günlüklerini depolamak için günlüklerin toplanmasını etkinleştirmeden önce hesabı oluşturun. Batch hesabınızla bir depolama hesabı ilişkilendirirseniz günlük hedefi bu hesabı seçebilirsiniz. 

Tanılama günlükleri için isteğe bağlı diğer hedefleri:

* Batch tanılama günlüğü olaylarını bir [Azure Olay Hub 'ına](../event-hubs/event-hubs-what-is-event-hubs.md)akış. Event hubs'ı, sonra dönüştürebilir ve herhangi bir gerçek zamanlı analiz sağlayıcısı kullanarak depolama, saniye başına milyonlarca olayı içe alabilir. 

* Tanılama günlüklerini [Azure izleyici günlüklerine](../log-analytics/log-analytics-overview.md)göndererek bunları analiz edebilir veya Power BI veya Excel 'de Analize aktarabilirsiniz.

> [!NOTE]
> Depolama veya Azure hizmetleriyle tanılama günlük verilerini işlemek için ek ücrete neden. 
>

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Batch tanılama günlükleri koleksiyonunu etkinleştir

1. Portalda, **tüm hizmetler** > **Batch hesapları**' na ve ardından Batch hesabınızın adına tıklayın.
2. **İzleme**altında tanılama **günlükleri** ' ne tıklayın > **tanılamayı açın**.
3. **Tanılama ayarları**' nda, ayar için bir ad girin ve bir günlük hedefi (mevcut depolama hesabı, Olay Hub 'ı veya Azure izleyici günlükleri) seçin. **ServiceLog** ve **allölçümleri**seçeneklerinden birini veya ikisini birden seçin.

    İsteğe bağlı olarak bir depolama hesabını seçtiğinizde bir bekletme ilkesi ayarlayın. Veriler, birkaç gün bekletme için belirtmezseniz, depolama hesabının ömrü tutulur.

4. **Kaydet** düğmesine tıklayın.

    ![Batch tanılama](media/batch-diagnostics/diagnostics-portal.png)

Günlük toplamayı etkinleştirmek için diğer seçenekler şunlardır: portalda Azure Izleyici 'yi kullanarak tanılama ayarlarını yapılandırın, bir [Kaynak Yöneticisi şablonu](../azure-monitor/platform/diagnostic-settings-template.md)kullanın veya Azure PowerShell ya da Azure CLI kullanın. bkz. [Azure kaynaklarınızdan günlük verilerini toplama ve](../azure-monitor/platform/platform-logs-overview.md)kullanma.


### <a name="access-diagnostics-logs-in-storage"></a>Depolama erişim tanılama günlükleri

Batch tanılama günlükleri bir depolama hesabında arşivleyin, ilgili olay oluşur oluşmaz depolama hesabında bir depolama kapsayıcısı oluşturulur. Bloblar, aşağıdaki adlandırma modele göre oluşturulur:

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
Her `PT1H.json` blob dosyası, blob URL 'sinde belirtilen saat içinde gerçekleşen JSON biçimli olayları içerir (örneğin, `h=12`). Mevcut saat boyunca olaylar, `PT1H.json` dosyasına eklendiği sırada eklenir. Tanılama günlüğü olayları saat başına ayrı bloblara bölündüğü için, dakika değeri (`m=00`) her zaman `00`. (Her zaman UTC biçimindedir.)

Aşağıda bir `PT1H.json` günlük dosyasında `PoolResizeCompleteEvent` girişi örneği verilmiştir. Bu, adanmış ve düşük öncelikli düğümlerin geçerli ve hedef sayısı ile işlemin başlangıç ve bitiş saati hakkında bilgiler içerir:

```
{ "Tenant": "65298bc2729a4c93b11c00ad7e660501", "time": "2019-08-22T20:59:13.5698778Z", "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/BATCHACCOUNTS/MYBATCHACCOUNT/", "category": "ServiceLog", "operationName": "PoolResizeCompleteEvent", "operationVersion": "2017-06-01", "properties": {"id":"MYPOOLID","nodeDeallocationOption":"Requeue","currentDedicatedNodes":10,"targetDedicatedNodes":100,"currentLowPriorityNodes":0,"targetLowPriorityNodes":0,"enableAutoScale":false,"isAutoPool":false,"startTime":"2019-08-22 20:50:59.522","endTime":"2019-08-22 20:59:12.489","resultCode":"Success","resultMessage":"The operation succeeded"}}
```

Depolama hesabındaki tanılama günlüklerinin şeması hakkında daha fazla bilgi için bkz. [Azure tanılama günlüklerini arşivleme](../azure-monitor/platform/resource-logs-collect-storage.md#schema-of-platform-logs-in-storage-account). Depolama hesabınızdaki günlüklerini programlı olarak erişmek için depolama API'leri kullanın. 

### <a name="service-log-events"></a>Hizmet günlüğü olayları
Azure Batch hizmeti toplanan, günlükleri ayrı bir Batch kaynak havuzu veya göreviyle gibi kullanım ömrü süresince Azure Batch hizmeti tarafından oluşturulan olayları içerir. Batch tarafından yayılan her olay, JSON biçiminde kaydedilir. Örneğin, bu örnek **Havuz oluşturma olayının**gövdesidir:

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

Batch hizmeti, şu anda aşağıdaki hizmet günlüğü olaylarını gösterir. Bu makalenin son güncelleştirildiği olduğundan ek olaylar eklenmiş olabilecek olduğundan bu liste kapsamlı, olmayabilir.

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
