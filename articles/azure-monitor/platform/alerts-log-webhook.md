---
title: Azure uyarılarındaki günlük uyarıları için Webhook eylemleri
description: Bu makalede, Log Analytics çalışma alanı veya Uygulama Öngörüleri kullanarak bir günlük uyarı kuralının nasıl oluşturuldurulması, uyarının verileri http webhook olarak nasıl ittiği ve mümkün olan farklı özelleştirmelerin ayrıntılarını açıklanmaktadır.
author: yanivlavi
ms.author: yalavi
services: monitoring
ms.topic: conceptual
ms.date: 06/25/2019
ms.subservice: alerts
ms.openlocfilehash: 7b1956ad2bf9bf38ba9edc4c7234078557564071
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667712"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Günlük uyarı kuralları için Webhook eylemleri
[Azure'da](alerts-log.md)bir günlük uyarısı oluşturulduğunda, bir veya daha fazla eylemi gerçekleştirmek için [eylem gruplarını kullanarak uyarıyı yapılandırma](action-groups.md) seçeneğiniz olur. Bu makalede, kullanılabilir farklı webhook eylemleri açıklanır ve özel bir JSON tabanlı webhook nasıl yapılandırılabildiğini gösterir.

> [!NOTE]
> Ayrıca webhook tümleştirmeler için [ortak uyarı şeması](https://aka.ms/commonAlertSchemaDocs) kullanabilirsiniz. Ortak uyarı şeması, Azure Monitor'daki tüm uyarı hizmetlerinde tek bir genişletilebilir ve birleşik uyarı yüküne sahip olmanın avantajını sağlar.Lütfen ortak uyarı şemasının günlük uyarıları için özel JSON seçeneğine uymadığını unutmayın. Uyarı kuralı düzeyinde yapmış olabileceğiniz özelleştirmeden bağımsız olarak seçilirse, ortak uyarı şeması yüküne erteler. [Ortak uyarı şeması tanımları hakkında bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)

## <a name="webhook-actions"></a>Web kancası eylemleri

Webhook eylemleri ile, tek bir HTTP POST isteği aracılığıyla harici bir işlem çağırabilirsiniz. Çağrılan hizmet webhooks'u desteklemeli ve aldığı yükünasıl kullanacağını belirlemelidir.

Webhook eylemleri aşağıdaki tablodaki özellikleri gerektirir.

| Özellik | Açıklama |
|:--- |:--- |
| **Webhook URL** |Webhook'un URL'si. |
| **Özel JSON yükü** |Uyarı oluşturma sırasında bu seçenek seçildiğinde webhook ile gönderilecek özel yük. Daha fazla bilgi için günlük [uyarılarını yönet'e](alerts-log.md)bakın.|

> [!NOTE]
> Günlük uyarısı için webhook seçeneği **için özel JSON yükü ekle** seçeneğinin yanında **Webhook'u görüntüle** düğmesi, sağlanan özelleştirme için örnek webhook yükünü görüntüler. Gerçek veri içermez, ancak günlük uyarıları için kullanılan JSON şema sını temsil eder. 

Webhooks bir URL ve harici hizmete gönderilen veri JSON biçimlendirilmiş bir yük içerir. Varsayılan olarak, yük aşağıdaki tablodaki değerleri içerir. Bu yükü kendi özel yükünüzle değiştirmeyi seçebilirsiniz. Bu durumda, parametrelerin her biri için tablodaki değişkenleri özel yükünüze değerlerini eklemek için kullanın.


| Parametre | Değişken | Açıklama |
|:--- |:--- |:--- |
| *AlertRuleName* |#alertrulename |Uyarı kuralının adı. |
| *Önem Derecesi* |#severity |Ateşlenen günlük uyarısı için önem kümesi. |
| *AlertThresholdOperator* |#thresholdoperator |Uyarı kuralı için eşik işleci, daha büyük veya daha az kullanır. |
| *AlertThresholdValue* |#thresholdvalue |Uyarı kuralı için eşik değeri. |
| *LinkToSearchSonuçları* |#linktosearchresults |Uyarıyı oluşturan sorgudaki kayıtları döndüren Analytics portalına bağlantı. |
| *Sonuç Sayısı* |#searchresultcount |Arama sonuçlarındaki kayıt sayısı. |
| *Arama Aralığı Bitiş zamanı* |#searchintervalendtimeutc |UTC'deki sorgunun bitiş saati, mm/dd/yyyy HH:mm:ss AM/PM biçimiyle. |
| *Arama Aralığı* |#searchinterval |HH:mm:ss biçimiyle uyarı kuralı için zaman penceresi. |
| *Arama Aralığı Başlangıç Saati* |#searchintervalstarttimeutc |UTC'deki sorgunun başlama saati, mm/dd/yyyy HH:mm:ss AM/PM biçimiyle. 
| *Arama Sorgusu* |#searchquery |Uyarı kuralı tarafından kullanılan günlük arama sorgusu. |
| *Arama Sonuçları* |"IncludeSearchResults": doğru|"IncludeSearchResults" aşağıdaki gibi özel json webhook tanımında üst düzey bir özellik olarak gerçek eklenirse, ilk 1.000 kayıtla sınırlı olan, sorgu tarafından JSON tablosu olarak döndürülen kayıtlar. |
| *Uyarı Türü*| #alerttype | [Metrik ölçüm](alerts-unified-log.md#metric-measurement-alert-rules) veya sonuç sayısı olarak yapılandırılan günlük uyarı [kuralının](alerts-unified-log.md#number-of-results-alert-rules)türü.|
| *Çalışma AlanıID* |#workspaceid |Log Analytics çalışma alanınızın kimliği. |
| *Başvuru Kimliği* |#applicationid |Application Insights uygulamanızın kimliği. |
| *Abonelik Kimliği* |#subscriptionid |Azure aboneliğinizin kimliği kullanılır. 

> [!NOTE]
> *LinkToSearchResults,* *Arama Sorgusu,* *Arama Aralığı Başlangıç Saati*ve Arama Aralığı Bitiş *saati* gibi parametreleri Analytics bölümünde görüntülemek için Azure portalına url'de geçirir. Azure portalı, uri boyut sınırına yaklaşık 2.000 karakterdir. Parametre değerleri sınırı aşarsa, portal uyarılarda sağlanan bağlantıları *açmaz.* Sonuçları Analytics portalında görüntülemek için ayrıntıları el ile giriş yapabilirsiniz. Veya sonuçları programlı olarak almak için [Application Insights ANALYTICs REST API](https://dev.applicationinsights.io/documentation/Using-the-API) veya [Log Analytics REST API'yi](/rest/api/loganalytics/) kullanabilirsiniz. 

Örneğin, *metin*adı verilen tek bir parametre içeren aşağıdaki özel yükü belirtebilirsiniz. Bu webhook'un aradığı hizmet bu parametreyi bekler.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Bu örnek yük, webhook'a gönderildiğinde aşağıdaki gibi bir çözüme kavuşturulir:

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Özel bir webhook'taki tüm değişkenlerin "#searchinterval" gibi bir JSON muhafazası içinde belirtilmesi gerektiğinden, ortaya çıkan webhook'un kasalarda "00:05:00" gibi değişken verileri de vardır.

Arama sonuçlarını özel bir yüke eklemek **için, IncludeSearchResults'in** JSON yükünde üst düzey bir özellik olarak ayarlandığından emin olun. 

## <a name="sample-payloads"></a>Örnek yükler
Bu bölümde, günlük uyarıları için webhooks için örnek yükler gösterilmektedir. Örnek yükler, yükün standart olduğu ve özel olduğunda örnekler içerir.

### <a name="standard-webhook-for-log-alerts"></a>Günlük uyarıları için standart webhook 
Bu örneklerin her ikisi de yalnızca iki sütun ve iki satır ile sahte bir yük var.

#### <a name="log-alert-for-log-analytics"></a>Log Analytics için günlük uyarısı
Aşağıdaki örnek yük, Log Analytics'e dayalı uyarılar için kullanılan *özel bir JSON seçeneği olmayan* standart bir webhook eylemi içindir:

```json
{
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": "log alert rule",
    "Severity": "Warning",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
 }
 ```

> [!NOTE]
> Log Analytics'teki günlük uyarıları için [API tercihinizi değiştirdiyseniz](alerts-log-api-switch.md) "Önem Derecesi" alan değeri değişebilir.


#### <a name="log-alert-for-application-insights"></a>Uygulama Öngörüleri için günlük uyarısı
Aşağıdaki örnek yük, Uygulama Öngörüleri'ne dayalı günlük uyarıları için kullanıldığında *özel json seçeneği olmayan* standart bir webhook içindir:
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"requests | where resultCode == \"500\"",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "3",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
    "AlertType": "Number of results"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Özel JSON yüküyle günlük uyarısı
Örneğin, yalnızca uyarı adını ve arama sonuçlarını içeren özel bir yük oluşturmak için aşağıdakileri kullanabilirsiniz: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Aşağıdaki örnek yük, herhangi bir günlük uyarısı için özel bir webhook eylemi içindir:
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResults":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Sonraki adımlar
- Azure [uyarılarındaki günlük uyarıları](alerts-unified-log.md)hakkında bilgi edinin.
- [Azure'da günlük uyarılarının](alerts-log.md)nasıl yönetilenini öğrenin.
- [Azure'da eylem grupları](action-groups.md)oluşturun ve yönetin.
- [Uygulama Öngörüleri](../../azure-monitor/app/analytics.md)hakkında daha fazla bilgi edinin.
- [Günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin. 

