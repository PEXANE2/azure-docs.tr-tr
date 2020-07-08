---
title: Ölçümler, uyarılar ve tanılama günlükleri
description: Havuzlar ve görevler gibi Azure Batch hesabı kaynakları için tanılama günlüğü olaylarını kaydedin ve çözümleyin.
ms.topic: how-to
ms.date: 05/29/2020
ms.custom: seodec18
ms.openlocfilehash: 6e10a4fc6cd13854682f094274c975931b056365
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85960733"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Tanılama değerlendirmesi ve izleme için toplu iş ölçümleri, uyarılar ve Günlükler
 
Bu makalede, [Azure izleyici](../azure-monitor/overview.md)'nin özelliklerini kullanarak bir Batch hesabının nasıl izleneceği açıklanır. Azure Izleyici, Batch hesabınızdaki kaynaklar için [ölçümleri](../azure-monitor/platform/data-platform-metrics.md) ve [tanılama günlüklerini](../azure-monitor/platform/platform-logs-overview.md) toplar. Batch hesabınızı izlemek ve sorunları tanılamak için kullanabileceğiniz çeşitli yollarla bu verileri toplayın ve kullanın. Ölçüm [uyarılarını](../azure-monitor/platform/alerts-overview.md) Ayrıca, bir ölçüm belirtilen bir değere ulaştığında bildirimler almanızı sağlayacak şekilde de yapılandırabilirsiniz.

## <a name="batch-metrics"></a>Toplu iş ölçümleri

Ölçümler, Azure kaynaklarınız tarafından yayınlanan ve Azure Izleyici hizmeti tarafından tüketilen Azure telemetri verileri (performans sayaçları da denir). Batch hesabındaki ölçümlere örnek olarak havuz oluşturma olayları, düşük öncelikli düğüm sayısı ve görev tamamlanma olayları verilebilir.

[Desteklenen toplu iş ölçümleri listesine](../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)bakın.

Ölçümler şunlardır:

- Ek yapılandırma olmadan her Batch hesabında varsayılan olarak etkindir
- Her 1 dakikada bir oluşturulur
- Otomatik olarak kalıcı değildir, ancak 30 günlük bir geçmiş geçmişi vardır. Tanılama günlüğü 'nün bir parçası olarak etkinlik ölçümlerini kalıcı hale getirebilirsiniz.

## <a name="view-batch-metrics"></a>Toplu ölçümleri görüntüleme

Azure portal, hesap için **genel bakış** sayfasında anahtar düğüm, çekirdek ve görev ölçümleri varsayılan olarak gösterilir.

Azure portal tüm Batch hesabı ölçümlerini görüntülemek için:

1. Azure Portal, **tüm hizmetler**  >  **Batch hesapları**' nı seçin ve ardından Batch hesabınızın adını seçin.
2. **İzleme**altında **ölçümler**' i seçin.
3. **Ölçüm Ekle** ' yi seçin ve ardından açılır listeden bir ölçüm seçin.
4. Ölçüm için bir **toplama** seçeneği belirleyin. Count tabanlı ölçümler için ("adanmış çekirdek sayısı" veya "düşük öncelikli düğüm sayısı" gibi), **Ortalama** toplamayı kullanın. Olay tabanlı ölçümler ("havuz yeniden boyutlandırma tam olayları" gibi) için, "toplama" **sayısını**kullanın.

   > [!WARNING]
   > Grafiğin dönemi boyunca alınan tüm veri noktalarının değerlerini ekleyen "Sum" toplamasını kullanmayın.

5. Ek ölçümler eklemek için 3 ve 4 numaralı adımları tekrarlayın.

Ölçümleri Azure Izleyici API 'Leri ile programlı olarak da alabilirsiniz. Bir örnek için bkz. [.net Ile Azure izleyici ölçümlerini alma](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/).

### <a name="batch-metric-reliability"></a>Batch ölçüm güvenilirliği

Ölçümler, eğilimleri belirlemenize yardımcı olabilir ve veri analizi için kullanılabilir. Ölçüm tesliminin garanti edilmediğini ve sipariş dışı teslim, veri kaybı ve/veya yineleme açısından tabi olabileceğini aklınızda bulundurulmak önemlidir. Bu nedenle, uyarı veya tetikleme işlevleri için tek olayların kullanılması önerilmez. Uyarı için eşikleri ayarlama hakkında daha fazla bilgi için sonraki bölüme bakın.

Son 3 dakika içinde oluşturulan ölçümler yine de oluşturulabilir, bu nedenle ölçüm değerleri bu zaman çerçevesi sırasında eksik olabilir.

## <a name="batch-metric-alerts"></a>Toplu ölçüm uyarıları

Belirtilen bir ölçümün değeri atadığınız bir eşiği aştığında tetiklenecek neredeyse gerçek zamanlı *ölçüm uyarılarını* yapılandırabilirsiniz. Uyarı, uyarı "etkinleştirildiğinde" (eşik geçildiğinde ve uyarı koşulu karşılandığında) ve "çözümlenmiş" olduğunda (eşik yeniden geçildiğinde ve koşul artık karşılanamadığında) bir bildirim oluşturur.

Ölçümler, sipariş dışı teslim, veri kaybı ve/veya çoğaltmasıyla ilgili olduğundan, tek bir veri noktasında tetiklenen uyarılar önerilmez. Uyarılarınızı oluştururken, bu tutarsızlıkları hesaba eklemek için eşikleri kullanabilirsiniz.

Örneğin, düşük öncelikli çekirdek sayılarınız belirli bir düzeye düştüğünde bir ölçüm uyarısı yapılandırmak isteyebilirsiniz, bu sayede havuzlarınızın birleşimini ayarlayabilirsiniz. En iyi sonuçlar için 10 veya daha fazla dakikalık bir süre ayarlayın, burada uyarılar, ortalama düşük öncelikli çekirdek sayısının dönemin tamamına ait eşik değerinin altına düşerse tetiklenir. Bu, daha doğru sonuçlar elde etmek için ölçümlerin toplaması için daha fazla zaman sağlar. 

Azure portal bir ölçüm uyarısı yapılandırmak için:

1. **Tüm hizmetler**  >  **Batch hesapları**' nı seçin ve ardından Batch hesabınızın adını seçin.
2. **İzleme**altında **Uyarılar**' ı seçin ve ardından **Yeni uyarı kuralı**' nı seçin.
3. **Koşul Seç**' e tıklayın ve ardından bir ölçüm seçin. **Grafik dönemi**, **eşik türü**, **işleç**ve **toplama türü**değerlerini onaylayın ve bir **eşik değeri**girin. Ardından **Bitti**'yi seçin.
4. Var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir eylem grubu ekleyin.
5. **Uyarı kuralı ayrıntıları** bölümünde, bir **Uyarı kuralı adı** ve **açıklaması** girin ve **önem derecesini** seçin
6. **Uyarı kuralı oluştur**’u seçin.

Ölçüm uyarıları oluşturma hakkında daha fazla bilgi için bkz. [ölçüm uyarılarının Azure izleyici 'de nasıl çalıştığını anlama](../azure-monitor/platform/alerts-metric-overview.md) ve [Azure izleyici kullanarak ölçüm uyarıları oluşturma, görüntüleme ve yönetme](../azure-monitor/platform/alerts-metric.md).

Azure Izleyici [REST API](/rest/api/monitor/)kullanarak neredeyse gerçek zamanlı bir uyarı da yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Microsoft Azure uyarılara genel bakış](../azure-monitor/platform/alerts-overview.md). Uyarılarınızda işi, görevi veya havuza özel bilgileri eklemek için [Azure Izleyici uyarıları ile olaylara yanıt veren](../azure-monitor/learn/tutorial-response.md)arama sorguları hakkındaki bilgilere bakın.

## <a name="batch-diagnostics"></a>Batch tanılama

Tanılama günlükleri, her bir kaynağın çalışmasını tanımlayan Azure kaynakları tarafından yayılan bilgileri içerir. Batch için aşağıdaki günlükleri toplayabilirsiniz:

- Hizmet, havuz veya görev gibi bireysel bir toplu Iş kaynağının ömrü boyunca Azure Batch hizmeti tarafından yayılan olayları **günlüğe kaydeder** .
- Hesap düzeyinde **ölçümler** günlüğe kaydedilir.

Tanılama günlüklerinin toplanmasını etkinleştirme ayarları varsayılan olarak etkin değildir. İzlemek istediğiniz her Batch hesabı için tanılama ayarlarını açık bir şekilde etkinleştirin.

### <a name="log-destinations"></a>Günlük hedefleri

Yaygın bir senaryo, günlük hedefi olarak bir Azure depolama hesabı seçmsudur. Günlükleri Azure depolamada depolamak için, günlüklerin toplanmasını etkinleştirmeden önce hesabı oluşturun. Batch hesabınızla bir depolama hesabı ilişkilendirdiyseniz, bu hesabı günlük hedefi olarak seçebilirsiniz.

Alternatif olarak şunları yapabilirsiniz:

- Batch tanılama günlüğü olaylarını bir [Azure Olay Hub 'ına](../event-hubs/event-hubs-what-is-event-hubs.md)akış. Event Hubs, saniye başına milyonlarca olayı alabilir ve bu sayede herhangi bir gerçek zamanlı analiz sağlayıcısını kullanarak dönüştürebilir ve depolayabilirler. 
- Tanılama günlüklerini [Azure izleyici günlüklerine](../log-analytics/log-analytics-overview.md)göndererek bunları analiz edebilir veya Power BI veya Excel 'de Analize aktarabilirsiniz.

> [!NOTE]
> Azure hizmetleriyle tanılama günlüğü verilerini depolamak veya işlemek için ek ücret ödemeniz gerekebilir. 

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Batch tanılama günlüklerinin toplanmasını etkinleştir

Azure portal yeni bir tanılama ayarı oluşturmak için aşağıdaki adımları izleyin.

1. Azure Portal, **tüm hizmetler**  >  **Batch hesapları**' nı seçin ve ardından Batch hesabınızın adını seçin.
2. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.
3. **Tanılama ayarları**' nda, **Tanılama ayarı Ekle**' yi seçin.
4. Ayar için bir ad girin.
5. Bir hedef seçin: **Log Analytics gönder**, **depolama hesabına Arşivle**veya **Olay Hub 'ına akış**. Bir depolama hesabı seçerseniz, isteğe bağlı olarak bir bekletme ilkesi ayarlayabilirsiniz. Bekletme için gün sayısı belirtmezseniz, depolama hesabının ömrü boyunca veriler korunur.
6. **ServiceLog**, **allölçümlerini**veya her ikisini birden seçin.
7. Tanılama ayarını oluşturmak için **Kaydet** ' i seçin.

Ayrıca, [Kaynak Yöneticisi bir şablon](../azure-monitor/platform/diagnostic-settings-template.md)kullanarak veya Azure PowerShell ya da Azure CLI ile tanılama ayarlarını yapılandırmak Için [Azure Portal Azure izleyici aracılığıyla toplamayı etkinleştirebilirsiniz](../azure-monitor/platform/diagnostic-settings.md) . Daha fazla bilgi için bkz. [Azure platform günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md).

### <a name="access-diagnostics-logs-in-storage"></a>Depolama alanındaki tanılama günlüklerine erişin

Batch tanılama günlüklerini bir depolama hesabında arşivlerseniz, depolama hesabında ilgili bir olay meydana geldiğinde depolama kapsayıcısı oluşturulur. Blob 'lar aşağıdaki adlandırma düzenine göre oluşturulur:

```json
insights-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/
RESOURCEGROUPS/{resource group name}/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/{Batch account name}/y={four-digit numeric year}/
m={two-digit numeric month}/d={two-digit numeric day}/
h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Örneğin:

```json
insights-metrics-pt1m/resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/
RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/MYBATCHACCOUNT/y=2018/m=03/d=05/h=22/m=00/PT1H.json
```

Her `PT1H.json` BLOB dosyası, blob URL 'sinde belirtilen saat içinde gerçekleşen JSON biçimli olayları içerir (örneğin, `h=12` ). Mevcut saat boyunca olaylar, `PT1H.json` oluşan sırada dosyaya eklenir. `m=00` `00` Tanılama günlüğü olayları saat başına ayrı bloblara bölündüğü için, dakika değeri () her zaman olur. (Tüm zamanlar UTC 'de bulunur.)

Bir günlük dosyasındaki bir girdinin örneği aşağıda verilmiştir `PoolResizeCompleteEvent` `PT1H.json` . Bu, adanmış ve düşük öncelikli düğümlerin geçerli ve hedef sayısı ile işlemin başlangıç ve bitiş saati hakkında bilgiler içerir:

```json
{ "Tenant": "65298bc2729a4c93b11c00ad7e660501", "time": "2019-08-22T20:59:13.5698778Z", "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/BATCHACCOUNTS/MYBATCHACCOUNT/", "category": "ServiceLog", "operationName": "PoolResizeCompleteEvent", "operationVersion": "2017-06-01", "properties": {"id":"MYPOOLID","nodeDeallocationOption":"Requeue","currentDedicatedNodes":10,"targetDedicatedNodes":100,"currentLowPriorityNodes":0,"targetLowPriorityNodes":0,"enableAutoScale":false,"isAutoPool":false,"startTime":"2019-08-22 20:50:59.522","endTime":"2019-08-22 20:59:12.489","resultCode":"Success","resultMessage":"The operation succeeded"}}
```

Depolama hesabındaki tanılama günlüklerinin şeması hakkında daha fazla bilgi için bkz. [Azure Kaynak günlüklerini depolama hesabına arşivleme](../azure-monitor/platform/resource-logs-collect-storage.md#schema-of-platform-logs-in-storage-account). Depolama hesabınızdaki günlüklere programlı bir şekilde erişmek için depolama API 'Lerini kullanın.

### <a name="service-log-events"></a>Hizmet günlüğü olayları

Toplanırsa Azure Batch hizmet günlükleri, havuz veya görev gibi ayrı bir toplu Iş kaynağının ömrü boyunca Azure Batch hizmeti tarafından oluşturulan olayları içerir. Batch tarafından yayılan her olay JSON biçiminde günlüğe kaydedilir. Örneğin, bu örnek **Havuz oluşturma olayının**gövdesidir:

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

Batch hizmeti tarafından yayılan hizmet günlüğü olayları şunları içerir:

- [Havuz oluşturma](batch-pool-create-event.md)
- [Havuz silme başlangıcı](batch-pool-delete-start-event.md)
- [Havuz silme Tamam](batch-pool-delete-complete-event.md)
- [Havuz yeniden boyutlandırma başlangıcı](batch-pool-resize-start-event.md)
- [Havuz yeniden boyutlandırma Tamam](batch-pool-resize-complete-event.md)
- [Görev başlangıcı](batch-task-start-event.md)
- [Görev tamamlanma](batch-task-complete-event.md)
- [Görev başarısız](batch-task-fail-event.md)

## <a name="next-steps"></a>Sonraki adımlar

- Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
- [Toplu iş çözümlerini izleme](monitoring-overview.md)hakkında daha fazla bilgi edinin.

