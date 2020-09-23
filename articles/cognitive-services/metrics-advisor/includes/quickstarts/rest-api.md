---
title: Ölçüm Izleyicisi REST API hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: metrics-advisor
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 82eec57b30e177f75a3ac689dc096dfea54c6717
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947948"
---
## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Azure aboneliğiniz olduktan sonra, ölçüm Danışmanı <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> örneğinizi dağıtmak için Azure Portal ölçüm Danışmanı kaynağı oluşturun.  
* Geçerli [kıvrın](https://curl.haxx.se/)sürümü. Bu makalede, [kıvrımlı belgelerde](https://curl.haxx.se/docs/manpage.html)belirtilen birkaç komut satırı anahtarı kullanılır.
    * Aşağıdaki BASH örnekleri, `\` satır devamlılık karakterini kullanır. Konsol veya Terminal farklı bir satır devamlılık karakteri kullanıyorsa, bu karakteri kullanın.

> [!TIP]
> * [GitHub](https://github.com/Azure-Samples/cognitive-services-rest-api-samples/)üzerinde REST API kullanan bir Python örneği bulabilirsiniz.
> * Ölçüm Danışmanı kaynağınızın dağıtılması 10 ila 30 dakika olabilir. Başarılı bir şekilde dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
> * Dağıtımdan sonra, Web portalı ve REST API ölçüm Danışmanı örneğinizi kullanmaya başlayabilirsiniz. Oluşturduğunuz kaynakta her iki URL 'yi de bulabilirsiniz.
> * Kaynak içindeki **anahtarlar ve uç noktada**bulunan hizmeti kullanmaya başlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.

## <a name="add-a-data-feed-from-a-sample-or-data-source"></a>Örnek veya veri kaynağından veri akışı ekleme

Zaman serisi verilerinizi izlemeye başlamak için bir veri akışı eklemeniz gerekir. Veri akışı eklemek için veri kaynağı türü ve parametrelerine göre bir veri şeması sağlamanız gerekir. Aşağıdaki JSON istek gövdesini * üzerindebody.js*adlı bir dosyaya kaydedin ve kıvrımlı komutunu çalıştırın.

```json
{
        "dataSourceType": "SqlServer",
        "dataFeedName": "test_data_feed_00000001",
        "dataFeedDescription": "",
        "dataSourceParameter": {
            "connectionString": "Server=ad-sample.database.windows.net,1433;Initial Catalog=ad-sample;Persist Security Info=False;User ID=adreadonly;Password=Readonly@2020;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
            "query": "select * from adsample3 where Timestamp = @StartTime"
        },
        "granularityName": "Daily",
        "granularityAmount": 0,
        "metrics": [
            {
                "metricName": "revenue",
                "metricDisplayName": "revenue",
                "metricDescription": ""
            },
            {
                "metricName": "cost",
                "metricDisplayName": "cost",
                "metricDescription": ""
            }
        ],
        "dimension": [
            {
                "dimensionName": "city",
                "dimensionDisplayName": "city"
            },
            {
                "dimensionName": "category",
                "dimensionDisplayName": "category"
            }
        ],
        "timestampColumn": "timestamp",
        "dataStartFrom": "2020-06-01T00:00:00.000Z",
        "startOffsetInSeconds": 0,
        "maxConcurrency": -1,
        "minRetryIntervalInSeconds": -1,
        "stopRetryAfterInSeconds": -1,
        "needRollup": "NoRollup",
        "fillMissingPointType": "SmartFilling",
        "fillMissingPointValue": 0,
        "viewMode": "Private",
        "admins": [
            "xxx@microsoft.com"
        ],
        "viewers": [
        ],
        "actionLinkTemplate": ""
}
```

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla ve JSON değerlerinizle düzenleyin.

```bash
curl -i https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/datafeeds \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-d @body.json
```

#### <a name="example-response"></a>Örnek yanıt

```
HTTP/1.1 201 Created
Content-Length: 0
Location: https://gualala-beta-0617.cognitiveservices.azure.com/anomalydetector-ee/v1.0/datafeeds/b5921405-8001-42b2-8746-004ddeeb780d
x-envoy-upstream-service-time: 564
apim-request-id: 4e4fe70b-d663-4fb7-a804-b9dc14ba02a3
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Thu, 03 Sep 2020 18:29:27 GMT
```
Yukarıdaki yanıtta, **konum** üstbilgisi oluşturduğunuz veri akışı URL 'Sidir ve **Datafeedid**'yi içerir. 

Yukarıdaki URL 'yi kullanarak, önceki adımda oluşturduğunuz veri akışı hakkında ayrıntılı bilgileri sorgulayabilirsiniz. (Aşağıdaki adımlarda, veri akışı bilgilerinde **metricID** kullanacağız)

```bash
curl https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/datafeeds/REPLACE-WITH-YOUR-DATA-FEED-ID \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

#### <a name="example-response"></a>Örnek yanıt

```
{
   "dataFeedId":"90919c03-be13-4efa-86e5-aa9dc72764ce",
   "dataFeedName":"test_data_feed_00000007",
   "metrics":[
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "metricName":"cost",
         "metricDisplayName":"cost",
         "metricDescription":""
      },
      {
         "metricId":"82bbc63d-3739-4d57-b190-accb69721b6a",
         "metricName":"revenue",
         "metricDisplayName":"revenue",
         "metricDescription":""
      }
   ],
   "dimension":[
      {
         "dimensionName":"category",
         "dimensionDisplayName":"category"
      },
      {
         "dimensionName":"city",
         "dimensionDisplayName":"city"
      }
   ],
   "dataStartFrom":"2020-06-01T00:00:00Z",
   "dataSourceType":"SqlServer",
   "timestampColumn":"timestamp",
   "startOffsetInSeconds":0,
   "maxQueryPerMinute":30.0,
   "granularityName":"Daily",
   "granularityAmount":null,
   "allUpIdentification":null,
   "needRollup":"NoRollup",
   "fillMissingPointType":"SmartFilling",
   "fillMissingPointValue":0.0,
   "rollUpMethod":"None",
   "rollUpColumns":[
      
   ],
   "dataFeedDescription":"",
   "stopRetryAfterInSeconds":-1,
   "minRetryIntervalInSeconds":-1,
   "maxConcurrency":-1,
   "viewMode":"Private",
   "admins":[
      "zhli2@microsoft.com"
   ],
   "viewers":[
      
   ],
   "creator":"bowgong@microsoft.com",
   "status":"Active",
   "createdTime":"2020-09-08T08:39:28Z",
   "isAdmin":true,
   "actionLinkTemplate":"",
   "dataSourceParameter":{
      "connectionString":"Server=ad-sample.database.windows.net,1433;Initial Catalog=ad-sample;Persist Security Info=False;User ID=adreadonly;Password=Readonly@2020;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
      "query":"select * from adsample3 where Timestamp = @StartTime"
   }
}
```



## <a name="check-ingestion-status"></a>Alma durumunu denetle

Veri akışı eklendikten sonra, bir alım işinin ilerlemesini denetlemek isterseniz, onun durumunu kontrol edebilirsiniz. Aşağıdaki JSON istek gövdesini * üzerindebody.js*adlı bir dosyaya kaydedin ve kıvrımlı komutunu çalıştırın.

```json
{
  "startTime": "2020-01-01T00:00:00.0000000+00:00",
  "endTime": "2020-01-04T00:00:00.0000000+00:00"
}
```

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla, JSON değerlerinizle ve JSON boyutunda düzenleyin.


```bash
curl https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/datafeeds/REPLACE-WITH-YOUR-DATA-FEED-ID/ingestionStatus/query \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-d @body.json
```


#### <a name="example-response"></a>Örnek yanıt

```json
{
  "@nextLink": "https://demo.example.com/datafeeds/01234567-8901-2345-6789-012345678901/ingestionStatus/query?$skip=3&$top=1",
  "value": [
    {
      "timestamp": "2020-09-03T00:00:00.0000000+00:00",
      "status": "Running",
      "message": ""
    },
    {
      "timestamp": "2020-09-02T00:00:00.0000000+00:00",
      "status": "Succeeded",
      "message": ""
    },
    {
      "timestamp": "2020-09-01T00:00:00.0000000+00:00",
      "status": "Failed",
      "message": "No valid record pulled from source for current timestamp 2020-01-01T00:00:00Z"
    }
  ]
}
```

##  <a name="configure-anomaly-detection-configuration"></a>Anomali algılama yapılandırmasını Yapılandır

Varsayılan yapılandırma her ölçüm için otomatik olarak uygulandığında, verilerinizde kullanılan algılama modlarını ayarlayabilirsiniz. Aşağıdaki JSON istek gövdesini * üzerindebody.js*adlı bir dosyaya kaydedin ve kıvrımlı komutunu çalıştırın.

```json
{
        "name": "test_detection_config0000000001",
        "description": "string",
        "metricId": "8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
        "wholeMetricConfiguration": {
            "smartDetectionCondition": {
                "sensitivity": 100,
                "anomalyDetectorDirection": "Both",
                "suppressCondition": {
                    "minNumber": 1,
                    "minRatio": 1
                }
            }
        },
        "dimensionGroupOverrideConfigurations": [
        ],
        "seriesOverrideConfigurations": [
        ]
}
```

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla, JSON değerlerinizle ve JSON boyutunda düzenleyin.

```bash
curl -i https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/enrichment/anomalyDetection/configurations \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-d @body.json
```

#### <a name="example-response"></a>Örnek yanıt

```
HTTP/1.1 201 Created
Content-Length: 0
Location: https://gualala-beta-0617.cognitiveservices.azure.com/anomalydetector-ee/v1.0/enrichment/anomalyDetection/configurations/6a977d61-f0f5-488a-a162-2feb4643ae09
x-request-id: 17752fcc-9085-46d5-ad37-c4e9e9ba6a5a
x-envoy-upstream-service-time: 253
apim-request-id: 17752fcc-9085-46d5-ad37-c4e9e9ba6a5a
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Tue, 08 Sep 2020 09:50:38 GMT
```

Yukarıdaki **konum** üst bilgisinde, yeni oluşturulan kaynağın (algılama yapılandırması) URL 'sini içerir. 

**Konum** üst bilgisinde yukarıdaki URL 'yi kullanarak, oluşturduğunuz algılama yapılandırmasını sorgulayabilir (aşağıdaki adımlarda yanıt Içeriği için **anomalyDetectionConfigurationId** kullanacağız)

```
curl https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/enrichment/anomalyDetection/configurations/REPLACE-WITH-YOUR-DETECTION-CONFIGURATION-ID \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

#### <a name="example-response"></a>Örnek yanıt

```json
{
   "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
   "name":"test_detection_config0000000001",
   "description":"string",
   "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
   "wholeMetricConfiguration":{
      "smartDetectionCondition":{
         "sensitivity":100.0,
         "anomalyDetectorDirection":"Both",
         "suppressCondition":{
            "minNumber":1,
            "minRatio":1.0
         }
      }
   },
   "dimensionGroupOverrideConfigurations":[
      
   ],
   "seriesOverrideConfigurations":[
      
   ]
}
```



### <a name="configure-alert-configuration"></a>Uyarı yapılandırmasını Yapılandır

Uyarıyı yapılandırmadan önce, uyarıyı bilgilendirmek için kullanılacak bir kanca oluşturmanız gerekir. Web kancası ve e-posta olan bir uyarı tetiklendiğinde bildirim almanın iki yolu vardır. Kanca oluştururken kanca türü olarak bu bunlardan birini kanca içinde belirtebilirsiniz.

Aşağıdaki JSON istek gövdesini * üzerindebody.js*adlı bir dosyaya kaydedin ve kıvrımlı komutunu çalıştırın.

```json
{
        "hookType": "Webhook",
        "hookName": "test_web_hook000001",
        "description": "",
        "externalLink": "",
        "hookParameter": {
            "endpoint": "https://www.xxx.com/aaa",
            "username": "",
            "password": ""
        }
}
```

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla, JSON değerlerinizle ve JSON boyutunda düzenleyin.

```bash
curl -i https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/hooks \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-d @body.json
```

#### <a name="example-response"></a>Örnek yanıt

```
HTTP/1.1 201 Created
Content-Length: 0
Location: https://gualala-beta-0617.cognitiveservices.azure.com/anomalydetector-ee/v1.0/hooks/34d677bd-0875-4760-8bf6-24d48abde7c3
x-request-id: 7b6cc1a6-02cb-405b-bee3-174fdae0a7d2
x-envoy-upstream-service-time: 1640
apim-request-id: 7b6cc1a6-02cb-405b-bee3-174fdae0a7d2
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Tue, 08 Sep 2020 10:37:59 GMT
```

**Konum** üst bilgisinde yukarıdaki URL 'yi kullanarak, oluşturduğunuz Web kancasını sorgulayabilirsiniz.

```
curl https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/hooks/REPLACE-WITH-YOUR-HOOK-ID \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

#### <a name="example-response"></a>Örnek yanıt

```json
{
   "hookId":"34d677bd-0875-4760-8bf6-24d48abde7c3",
   "hookName":"test_web_hook000001",
   "hookType":"Webhook",
   "externalLink":"",
   "description":"",
   "admins":[
      "bowgong@microsoft.com"
   ],
   "hookParameter":{
      "endpoint":"https://www.xxx.com/aaa",
      "username":"",
      "password":"",
      "headers":{
         

      },
      "certificateKey":"",
      "certificatePassword":""

   }
}
```

Uyarı yapılandırmasını yapılandırarak, uyarının tetiklenmesi için kullanılabilecek algılama koşulu belirtebilirsiniz. Aşağıdaki JSON istek gövdesini * üzerindebody.js*adlı bir dosyaya kaydedin ve kıvrımlı komutunu çalıştırın.

```json
{
        "name": "test_alert_config00000001",
        "description": "",
        "crossMetricsOperator": "AND",
        "hookIds": [
           "34d677bd-0875-4760-8bf6-24d48abde7c3" 
        ],
        "metricAlertingConfigurations": [
            {
                "anomalyDetectionConfigurationId": "6a977d61-f0f5-488a-a162-2feb4643ae09",
                "anomalyScopeType": "All",
                "severityFilter": {
                    "minAlertSeverity": "Low",
                    "maxAlertSeverity": "High"
                },
                "snoozeFilter": {
                    "autoSnooze": 0,
                    "snoozeScope": "Metric",
                    "onlyForSuccessive": true
                },
            }
        ]
}
```

Kıvral komutu bir BASH kabuğundan yürütülür. Bu komutu kendi kaynak adınızla, kaynak anahtarınızla, JSON değerlerinizle ve JSON boyutunda düzenleyin.

```bash
curl -i https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/alert/anomaly/configurations \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-d @body.json
```

#### <a name="example-response"></a>Örnek yanıt

```
HTTP/1.1 201 Created
Content-Length: 0
Location: https://gualala-beta-0617.cognitiveservices.azure.com/anomalydetector-ee/v1.0/alert/anomaly/configurations/40004c91-6996-47c0-b8c8-fd20a8f4f0ab
x-request-id: 17752fcc-9085-46d5-ad37-c4e9e9ba6a5a
x-envoy-upstream-service-time: 253
apim-request-id: 17752fcc-9085-46d5-ad37-c4e9e9ba6a5a
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Tue, 08 Sep 2020 09:50:38 GMT
```

Yukarıdaki **konum** üst bilgisinde, yeni oluşturulan kaynağın (algılama yapılandırması) URL 'sini içerir. 

**Konum** üst bilgisinde yukarıdaki URL 'yi kullanarak, oluşturduğunuz uyarı yapılandırmasını sorgulayabilirsiniz. (Aşağıdaki adımlarda, uyarı yapılandırmasında **Anoyalertingconfigurationıd** kullanacağız)

```
curl https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/alert/anomaly/configurations/REPLACE-WITH-YOUR-ANOMALY-ALERTING-CONFIGURATION-ID \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

#### <a name="example-response"></a>Örnek yanıt

```json
{
   "anomalyAlertingConfigurationId":"40004c91-6996-47c0-b8c8-fd20a8f4f0ab",
   "name":"test_alert_config00000001",
   "description":"",
   "hookIds":[
      "34d677bd-0875-4760-8bf6-24d48abde7c3"
   ],
   "metricAlertingConfigurations":[
      {
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "anomalyScopeType":"All",
         "negationOperation":false,
         "severityFilter":{
            "minAlertSeverity":"Low",
            "maxAlertSeverity":"High"
         },
         "snoozeFilter":{
            "autoSnooze":0,
            "snoozeScope":"Metric",
            "onlyForSuccessive":true
         }
      }
   ]
}
```

### <a name="query-anomaly-detection-results"></a>Sorgu anomali algılama sonuçları

Algılama sonucunu almanın farklı yolları vardır. Örneğin, algılama işlemini oluşturduğunuz algılama yapılandırmasını kullanarak düzenli aralıklarla etkin bir şekilde sorgulayabilir veya uyarı aracılığıyla size bildirimde bulunabilir ve ardından bu uyarıyı kullanarak ilgili bozukluklar sorgulayabilirsiniz.

Aşağıdaki örnekte, uyarının nasıl sorgulandığından ve Bu uyarıyla ilgili anormallikleri sorgulamak için bu uyarının nasıl kullanılacağı gösterilmektedir.

#### <a name="query-alert"></a>Sorgu uyarısı

Uyarıyı sorgulamak için yukarıdaki adımda oluşturulan uyarı yapılandırması ' nı kullanabilirsiniz.

```
curl https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/anomalydetector-ee/v1.0/alert/anomaly/configurations/REPLACE-WITH-YOUR-ANOMALY-ALERTING-CONFIGURATION-ID/alerts/query \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

#### <a name="example-response"></a>Örnek yanıt

```json
{
   "value":[
      {
         "alertId":"17465dcc000",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.532Z",
         "modifiedTime":"2020-09-08T19:12:46.588Z"
      }
   ],
   "@nextLink":null
}
```

Yukarıdaki yanıtta bir uyarı aldık. Bu **AlertId**'yi kullanarak, ilgili tüm anormallikleri bu uyarıya neden olacak şekilde sorgulayabilir.

(Uyarı almanın başka bir yolu da Web kancasını yapılandırmaktır ve bulunduğunda kapsamlı olarak Uyarı alındı)

#### <a name="query-anomalies-using-alertid"></a>AlertId kullanarak anomali sorgula

```
curl https://REPLACE-WITH-YOUR-ENDPOINT/anomalydetector-ee/v1.0/alert/anomaly/configurations/REPLACE-WITH-YOUR-ANOMALY-ALERTING-CONFIGURATION-ID/alerts/REPLACE-WITH-YOUR-ALERTID/anomalies \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "x-api-key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

#### <a name="example-response"></a>Örnek yanıt

```json
{
   "value":[
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.566Z",
         "modifiedTime":"2020-09-08T19:12:46.566Z",
         "dimension":{
            "city":"Amphibian",
            "category":"Caucasian Fir"
         },
         "property":{
            "anomalySeverity":"High",
            "anomalyStatus":"Active"
         }
      },
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.566Z",
         "modifiedTime":"2020-09-08T19:12:46.566Z",
         "dimension":{
            "city":"Animals by number of neurons",
            "category":"Crack willow"
         },
         "property":{
            "anomalySeverity":"High",
            "anomalyStatus":"Active"
         }
      },
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.566Z",
         "modifiedTime":"2020-09-08T19:12:46.566Z",
         "dimension":{
            "city":"Amphibian",
            "category":"Crack willow"
         },
         "property":{
            "anomalySeverity":"High",
            "anomalyStatus":"Active"
         }
      },
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.566Z",
         "modifiedTime":"2020-09-08T19:12:46.566Z",
         "dimension":{
            "city":"Animals by number of neurons",
            "category":"Common Lime"
         },
         "property":{
            "anomalySeverity":"High",
            "anomalyStatus":"Active"
         }
      },
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.566Z",
         "modifiedTime":"2020-09-08T19:12:46.566Z",
         "dimension":{
            "city":"Chickadee",
            "category":"Common Lime"
         },
         "property":{
            "anomalySeverity":"High",
            "anomalyStatus":"Active"
         }
      },
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.566Z",
         "modifiedTime":"2020-09-08T19:12:46.566Z",
         "dimension":{
            "city":"Amphibian",
            "category":"Common Lime"
         },
         "property":{
            "anomalySeverity":"High",
            "anomalyStatus":"Active"
         }
      },
      {
         "metricId":"8d03e541-a56d-4c28-8d9c-09ce91c6d95f",
         "anomalyDetectionConfigurationId":"6a977d61-f0f5-488a-a162-2feb4643ae09",
         "timestamp":"2020-09-07T00:00:00Z",
         "createdTime":"2020-09-08T19:12:46.566Z",
         "modifiedTime":"2020-09-08T19:12:46.566Z",
         "dimension":{
            "city":"Chickadee",
            "category":"Crack willow"
         },
         "property":{
            "anomalySeverity":"High",
            "anomalyStatus":"Active"
         }
      }
   ],
   "@nextLink":null
}
```
