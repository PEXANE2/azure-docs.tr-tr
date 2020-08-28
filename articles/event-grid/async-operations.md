---
title: Event Grid zaman uyumsuz işlemlerin durumu
description: Azure 'da Event Grid zaman uyumsuz işlemlerin nasıl izleneceğini açıklar. Uzun süre çalışan bir işlemin durumunu almak için kullandığınız değerleri gösterir.
ms.topic: conceptual
ms.date: 07/07/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: baae7b097a0b696d405c0e7ea3d3bdeb326f23b1
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89011693"
---
# <a name="track-event-grid-asynchronous-azure-operations"></a>Zaman uyumsuz Azure işlemlerini izleme Event Grid
İşlem hızlı bir şekilde tamamlanamadığından bazı Azure REST işlemleri zaman uyumsuz olarak çalışır. Bu makalede, yanıtta döndürülen değerler aracılığıyla zaman uyumsuz işlemlerin durumunun nasıl izleneceği açıklanır.  

## <a name="status-codes-for-asynchronous-operations"></a>Zaman uyumsuz işlemler için durum kodları
Zaman uyumsuz bir işlem başlangıçta bir HTTP durum kodu döndürür:

* 201 (oluşturuldu)
* 202 (kabul edildi) 

İşlem başarıyla tamamlandığında, aşağıdakilerden birini döndürür:

* 200 (TAMAM)
* 204 (Içerik yok) 

Yürütmekte olduğunuz işlemin yanıtlarını görmek için [REST API belgelerine](/rest/api/) bakın.

## <a name="monitor-status-of-operation"></a>İşlemin durumunu izle
Zaman uyumsuz REST işlemleri, işlemin durumunu belirlemede kullandığınız üst bilgi değerlerini döndürür. İnceleyecek olabilecek üç üst bilgi değeri vardır:

* `Azure-AsyncOperation` -İşlemin devam eden durumunu denetlemek için URL. İşlem bu değeri döndürürse, işlemin durumunu izlemek için her zaman kullanın (konum yerine).
* `Location` -Bir işlemin ne zaman tamamlandığını belirlemek için URL. Bu değeri yalnızca Azure-AsyncOperation döndürülmediğinde kullanın.
* `Retry-After` -Zaman uyumsuz işlemin durumu denetlenmeden önce beklenecek saniye sayısı.

Ancak, her zaman uyumsuz işlem bu değerlerin tümünü döndürmez. Örneğin, bir işlem için Azure-AsyncOperation üst bilgi değerini ve başka bir işlemin konum üst bilgi değerini değerlendirmeniz gerekebilir. 

Bir istek için herhangi bir üst bilgi değerini alarak üst bilgi değerlerini alırsınız. Örneğin, C# ' de, başlık değerini `HttpWebResponse` aşağıdaki kodla adlı bir nesneden alırsınız `response` :

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a>Azure-AsyncOperation isteği ve yanıtı

Zaman uyumsuz işlemin durumunu almak için Azure-AsyncOperation üstbilgi değerindeki URL 'ye bir GET isteği gönderin.

Bu işlemden yanıt gövdesi, işlem hakkındaki bilgileri içerir. Aşağıdaki örnek, işlemden döndürülen olası değerleri gösterir:

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}", 
    "status" : "Succeeded | Failed | Canceled | {resource provider values}", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : { 
        "code": "{error code}",  
        "message": "{error description}" 
    }
}
```

Yalnızca `status` tüm yanıtlar için döndürülür. Hata nesnesi, durum başarısız olduğunda veya Iptal edildiğinde döndürülür. Diğer tüm değerler isteğe bağlıdır; Bu nedenle, aldığınız yanıt bu örnekteki değerden farklı görünebilir.

## <a name="provisioningstate-values"></a>provisioningState değerleri

Kaynak oluşturan, güncelleştiren veya silen (PUT, PATCH, SIL) işlemler genellikle bir `provisioningState` değer döndürüyor. Bir işlem tamamlandığında, aşağıdaki üç değerden biri döndürülür: 

* Başarılı
* Başarısız
* İptal edildi

Diğer tüm değerler işlemin hala çalıştığını gösterir. Kaynak sağlayıcı, durumunu gösteren özelleştirilmiş bir değer döndürebilir. Örneğin, istek alındığında ve çalıştırıldığında **kabul** edilebilir.


## <a name="example-requests-and-responses"></a>Örnek istekleri ve yanıtları

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Sanal makineyi başlatın (Azure-AsyncOperation ile 202)
Bu örnek, sanal makineler için **başlatma** işleminin durumunun nasıl belirleneceğini gösterir. İlk istek aşağıdaki biçimdedir:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

202 durum kodunu döndürür. Üst bilgi değerleri arasında şunu görürsünüz:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

Bu URL 'ye başka bir istek göndererek zaman uyumsuz işlemin durumunu denetlemek için.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

Yanıt gövdesi işlemin durumunu içerir:

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Kaynakları dağıtma (Azure ile zaman uyumsuz coperation ile 201)

Bu örnekte, Azure 'a kaynak dağıtmaya yönelik **dağıtımlar** işleminin durumunun nasıl belirleneceği gösterilmektedir. İlk istek aşağıdaki biçimdedir:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

201 durum kodunu döndürür. Yanıtın gövdesi şunları içerir:

```json
"provisioningState":"Accepted",
```

Üst bilgi değerleri arasında şunu görürsünüz:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

Bu URL 'ye başka bir istek göndererek zaman uyumsuz işlemin durumunu denetlemek için.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

Yanıt gövdesi işlemin durumunu içerir:

```json
{"status":"Running"}
```

Dağıtım tamamlandığında, yanıt şunları içerir:

```json
{"status":"Succeeded"}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a>Depolama hesabı oluştur (konum ve yeniden dene ile 202)

Bu örnek, depolama hesapları için **oluşturma** işleminin durumunun nasıl belirleneceğini gösterir. İlk istek aşağıdaki biçimdedir:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

Ve istek gövdesi depolama hesabı için özellikler içerir:

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

202 durum kodunu döndürür. Üst bilgi değerleri arasında aşağıdaki iki değeri görürsünüz:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

Yeniden deneme sırasında belirtilen saniye sayısını bekledikten sonra, bu URL 'ye başka bir istek göndererek zaman uyumsuz işlemin durumunu denetleyin.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

İstek hala çalışıyorsa 202 bir durum kodu alırsınız. İstek tamamlandıysa, 200 durum kodunu alır ve yanıtın gövdesi oluşturulan depolama hesabının özelliklerini içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Her REST işlemi hakkındaki belgeler için [REST API belgelerine](/rest/api/)bakın.
* Kaynak Yöneticisi REST API üzerinden şablon dağıtma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonlarıyla kaynakları dağıtma ve Kaynak Yöneticisi REST API](../azure-resource-manager/templates/deploy-rest.md).