---
title: IoT Edge'de Azure Akış Analizi için CI/CD yapmak için REST API'lerini kullanın
description: REST API'lerini kullanarak Azure Akışı Analitiği için sürekli bir tümleştirme ve dağıtım ardışık akışını nasıl uygulayacağınızı öğrenin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.openlocfilehash: 328ca7cd2c6f76095c8334ae6fdb4aa75fbb867d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80291996"
---
# <a name="implement-cicd-for-stream-analytics-on-iot-edge-using-apis"></a>API'leri kullanarak IoT Edge'de Stream Analytics için CI/CD'yi uygulayın

REST API'lerini kullanarak Azure Akışı Analizi işleri için sürekli tümleştirme ve dağıtım sağlayabilirsiniz. Bu makalede, hangi API'ların kullanılacağı ve nasıl kullanılacağı nasıI örnekler verilmektedir. REST API'leri Azure Bulut BulutU'da desteklenmez.

## <a name="call-apis-from-different-environments"></a>Farklı ortamlardan API'leri arayın

REST API'leri hem Linux hem de Windows'tan çağrılabilir. Aşağıdaki komutlar, API çağrısı için uygun sözdizimini gösterir. Belirli API kullanımı bu makalenin sonraki bölümlerinde özetlenecektir.

### <a name="linux"></a>Linux

Linux için şunları `Curl` kullanabilir `Wget` veya komutlar alabilirsiniz:

```bash
curl -u { <username:password> }  -H "Content-Type: application/json" -X { <method> } -d "{ <request body> }" { <url> }   
```

```bash
wget -q -O- --{ <method> } -data="<request body>" --header=Content-Type:application/json --auth-no-challenge --http-user="<Admin>" --http-password="<password>" <url>
```
 
### <a name="windows"></a>Windows

Windows için Powershell'i kullanın: 

```powershell 
$user = "<username>" 
$pass = "<password>" 
$encodedCreds = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $user,$pass))) 
$basicAuthValue = "Basic $encodedCreds" 
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]" 
$headers.Add("Content-Type", 'application/json') 
$headers.Add("Authorization", $basicAuthValue) 
$content = "<request body>" 
$response = Invoke-RestMethod <url> -Method <method> -Body $content -Headers $Headers 
echo $response 
```
 
## <a name="create-an-asa-job-on-edge"></a>Edge'de ASA işi oluşturma 
 
Akış Analizi işi oluşturmak için, Stream Analytics API'sini kullanarak PUT yöntemini arayın.

|Yöntem|İstek URL'si|
|------|-----------|
|PUT|`https://management.azure.com/subscriptions/{\**subscription-id**}/resourcegroups/{**resource-group-name**}/providers/Microsoft.StreamAnalytics/streamingjobs/{**job-name**}?api-version=2017-04-01-preview`|
 
**Curl**kullanarak komut örneği :

```curl
curl -u { <username:password> } -H "Content-Type: application/json" -X { <method> } -d "{ <request body> }" https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/Microsoft.StreamAnalytics/streamingjobs/{jobname}?api-version=2017-04-01-preview  
``` 
 
JSON'daki istek gövdesi örneği:

```json
{ 
  "location": "West US", 
  "tags": { "key": "value", "ms-suppressjobstatusmetrics": "true" }, 
  "sku": {  
      "name": "Standard" 
    }, 
  "properties": { 
    "sku": {  
      "name": "standard" 
    }, 
       "eventsLateArrivalMaxDelayInSeconds": 1, 
       "jobType": "edge", 
    "inputs": [ 
      { 
        "name": "{inputname}", 
        "properties": { 
         "type": "stream", 
          "serialization": { 
            "type": "JSON", 
            "properties": { 
              "fieldDelimiter": ",", 
              "encoding": "UTF8" 
            } 
          }, 
          "datasource": { 
            "type": "GatewayMessageBus", 
            "properties": { 
            } 
          } 
        } 
      } 
    ], 
    "transformation": { 
      "name": "{queryName}", 
      "properties": { 
        "query": "{query}" 
      } 
    }, 
    "package": { 
      "storageAccount" : { 
        "accountName": "{blobstorageaccountname}", 
        "accountKey": "{blobstorageaccountkey}" 
      }, 
      "container": "{blobcontaine}]" 
    }, 
    "outputs": [ 
      { 
        "name": "{outputname}", 
        "properties": { 
          "serialization": { 
            "type": "JSON", 
            "properties": { 
              "fieldDelimiter": ",", 
              "encoding": "UTF8" 
            } 
          }, 
          "datasource": { 
            "type": "GatewayMessageBus", 
            "properties": { 
            } 
          } 
        } 
      } 
    ] 
  } 
} 
```
 
Daha fazla bilgi için [API belgelerine](/rest/api/streamanalytics/stream-analytics-job)bakın.  
 
## <a name="publish-edge-package"></a>Kenar paketini yayımla 
 
IoT Edge'de bir Stream Analytics işi yayınlamak için Edge Package Publish API'sini kullanarak POST yöntemini arayın.

|Yöntem|İstek URL'si|
|------|-----------|
|POST|`https://management.azure.com/subscriptions/{\**subscriptionid**}/resourceGroups/{**resourcegroupname**}/providers/Microsoft.StreamAnalytics/streamingjobs/{**jobname**}/publishedgepackage?api-version=2017-04-01-preview`|

Bu eşzamanlı işlem, iş başarıyla yayınlanana kadar 202 durumunu döndürür. Konum yanıt üstbilgisi, işlemin durumunu almak için kullanılan URI'yi içerir. İşlem çalışırken, konum üstbilgisinde URI'ye yapılan bir çağrı 202 durumunu döndürür. İşlem bittiğinde, konum üstbilgisindeki URI 200 durumu döndürür. 

Bir Edge paketi **kıvırma**kullanarak arama yayımlama örneği: 

```bash
curl -d -X POST https://management.azure.com/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/Microsoft.StreamAnalytics/streamingjobs/{jobname}/publishedgepackage?api-version=2017-04-01-preview
```
 
POST arama yaptıktan sonra, boş bir vücut ile bir yanıt beklemelisiniz. Yanıtın BAŞKANı'nda bulunan URL'ye bakın ve daha fazla kullanım için kaydedin.
 
Yanıt Başkanı'ndan URL örneği:

```
https://management.azure.com/subscriptions/{**subscriptionid**}/resourcegroups/{**resourcegroupname**}/providers/Microsoft.StreamAnalytics/StreamingJobs/{**resourcename**}/OperationResults/023a4d68-ffaf-4e16-8414-cb6f2e14fe23?api-version=2017-04-01-preview 
```
Yanıtın başında bulduğunuz URL ile api araması yapmak için aşağıdaki komutu çalıştırmadan önce bir ila iki dakika bekleyin. 200 yanıt alamazsanız komutu yeniden deneyin.
 
**Kıvrılmış**iade URL'si ile API araması yapma örneği:

```bash
curl -d –X GET https://management.azure.com/subscriptions/{subscriptionid}/resourceGroups/{resourcegroupname}/providers/Microsoft.StreamAnalytics/streamingjobs/{resourcename}/publishedgepackage?api-version=2017-04-01-preview 
```

Yanıt, Kenar dağıtım komut dosyasına eklemeniz gereken bilgileri içerir. Aşağıdaki örnekler, hangi bilgileri toplamanız gerektiğini ve dağıtım bildirimine nereye eklemeniz gerektiğini gösterir.
 
Başarılı bir şekilde yayımladıktan sonra örnek yanıt gövdesi:

```json
{ 
  edgePackageUrl : null 
  error : null 
  manifest : "{"supportedPlatforms":[{"os":"linux","arch":"amd64","features":[]},{"os":"linux","arch":"arm","features":[]},{"os":"windows","arch":"amd64","features":[]}],"schemaVersion":"2","name":"{jobname}","version":"1.0.0.0","type":"docker","settings":{"image":"{imageurl}","createOptions":null},"endpoints":{"inputs":["\],"outputs":["{outputnames}"]},"twin":{"contentType":"assignments","content":{"properties.desired":{"ASAJobInfo":"{asajobsasurl}","ASAJobResourceId":"{asajobresourceid}","ASAJobEtag":"{etag}","PublishTimeStamp":"{publishtimestamp}"}}}}" 
  status : "Succeeded" 
} 
```

Dağıtım Bildirimi Örneği: 

```json
{ 
  "modulesContent": { 
    "$edgeAgent": { 
      "properties.desired": { 
        "schemaVersion": "1.0", 
        "runtime": { 
          "type": "docker", 
          "settings": { 
            "minDockerVersion": "v1.25", 
            "loggingOptions": "", 
            "registryCredentials": {} 
          } 
        }, 
        "systemModules": { 
          "edgeAgent": { 
            "type": "docker", 
            "settings": { 
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0", 
              "createOptions": "{}" 
            } 
          }, 
          "edgeHub": { 
            "type": "docker", 
            "status": "running", 
            "restartPolicy": "always", 
            "settings": { 
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0", 
              "createOptions": "{}" 
            } 
          } 
        }, 
        "modules": { 
          "<asajobname>": { 
            "version": "1.0", 
            "type": "docker", 
            "status": "running", 
            "restartPolicy": "always", 
            "settings": { 
              "image": "<settings.image>", 
              "createOptions": "<settings.createOptions>" 
            } 
            "version": "<version>", 
             "env": { 
              "PlanId": { 
               "value": "stream-analytics-on-iot-edge" 
          } 
        } 
      } 
    }, 
    "$edgeHub": { 
      "properties.desired": { 
        "schemaVersion": "1.0", 
        "routes": { 
            "route": "FROM /* INTO $upstream" 
        }, 
        "storeAndForwardConfiguration": { 
          "timeToLiveSecs": 7200 
        } 
      } 
    }, 
    "<asajobname>": { 
      "properties.desired": {<twin.content.properties.desired>} 
    } 
  } 
} 
```

Dağıtım bildiriminin yapılandırması ndan sonra, dağıtım için [Azure CLI ile Azure IoT Edge modüllerini dağıt'a](../iot-edge/how-to-deploy-modules-cli.md) bakın.


## <a name="next-steps"></a>Sonraki adımlar 
 
* [IoT Edge üzerinde Azure Stream Analytics](stream-analytics-edge.md)
* [ASA Üzerinde IoT Edge öğretici](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics)
* [Visual Studio araçlarını kullanarak Stream Analytics Edge işlerini geliştirin](stream-analytics-tools-for-visual-studio-edge-jobs.md)
