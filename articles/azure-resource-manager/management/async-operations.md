---
title: Eşzamanlı operasyonların durumu
description: Azure'da eşzamanlı işlemlerin nasıl izlenir olduğunu açıklar. Uzun süren bir işlemin durumunu almak için kullandığınız değerleri gösterir.
ms.topic: conceptual
ms.date: 12/09/2018
ms.custom: seodec18
ms.openlocfilehash: 1cf8898e5fd63e35447f6580e13347ba6d7fc413
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485448"
---
# <a name="track-asynchronous-azure-operations"></a>Eşzamanlı Azure işlemlerini izleme
Bazı Azure REST işlemleri, işlem hızlı bir şekilde tamamlanamadığından eş senkronize bir şekilde çalışır. Bu makalede, yanıt döndürülen değerler üzerinden eşsenkronize işlemlerin durumunu izlemek için nasıl açıklanmaktadır.  

## <a name="status-codes-for-asynchronous-operations"></a>Eşzamanlı işlemler için durum kodları
Bir eşzamanlı işlem başlangıçta ya bir HTTP durum kodu döndürür:

* 201 (Oluşturuldu)
* 202 (Kabul) 

İşlem başarıyla tamamlandığında, aşağıdakilerden biri döndürür:

* 200 (Tamam)
* 204 (İçerik Yok) 

Yürüttüğün işlemin yanıtlarını görmek için [REST API belgelerine](/rest/api/) bakın.

## <a name="monitor-status-of-operation"></a>Operasyonun monitör durumu
Eşzamanlı REST işlemleri, işlemin durumunu belirlemek için kullandığınız üstbilgi değerlerini döndürür. İncelenecek üç üstbilgi değeri vardır:

* `Azure-AsyncOperation`- Operasyonun devam eden durumunu kontrol etmek için URL. İşleminiz bu değeri döndürürse, işlemin durumunu izlemek için her zaman (Konum yerine) kullanın.
* `Location`- Bir işlemin ne zaman tamamlandığını belirlemek için URL. Bu değeri yalnızca Azure-AsyncOperation döndürülmediğinde kullanın.
* `Retry-After`- Eşzamanlı işlemin durumunu kontrol etmeden önce bekleyecek saniye sayısı.

Ancak, her eşzamanlı işlem tüm bu değerleri döndürür. Örneğin, bir işlem için Azure-AsyncOperation üstbilgi değerini ve başka bir işlem için Konum üstbilgi değerini değerlendirmeniz gerekebilir. 

Bir istek için herhangi bir üstbilgi değeri almak gibi üstbilgi değerlerini alırsınız. Örneğin, C#'da üstbilgi değerini aşağıdaki `HttpWebResponse` kodla `response` birlikte adlı bir nesneden alırsınız:

```cs
response.Headers.GetValues("Azure-AsyncOperation").GetValue(0)
```

## <a name="azure-asyncoperation-request-and-response"></a>Azure-Asyncİşlem isteği ve yanıtı

Eşzamanlı işlemin durumunu almak için, Azure-AsyncOperation üstbilgi değerindeki URL'ye GET isteği gönderin.

Bu işlemden gelen yanıtın gövdesi, işlem hakkında bilgi içerir. Aşağıdaki örnek, işlemden döndürülen olası değerleri gösterir:

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

Yalnızca `status` tüm yanıtlar için döndürülür. Durum Başarısız olduğunda veya İptal Edildiğinde hata nesnesi döndürülür. Diğer tüm değerler isteğe bağlıdır; bu nedenle, aldığınız yanıt örnekten farklı görünebilir.

## <a name="provisioningstate-values"></a>sağlamaDevlet değerleri

Bir kaynak oluşturan, güncelleyen veya silen işlemler (PUT, `provisioningState` PATCH, DELETE) genellikle bir değer döndürür. Bir işlem tamamlandığında, aşağıdaki üç değerden biri döndürülür: 

* Başarılı oldu
* Başarısız
* İptal edildi

Diğer tüm değerler işlemin hala çalıştığını gösterir. Kaynak sağlayıcısı durumunu gösteren özelleştirilmiş bir değer döndürebilir. Örneğin, istek alınıp çalıştırıldığında **Kabul** edilebilirsiniz.

## <a name="example-requests-and-responses"></a>Örnek istekler ve yanıtlar

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Sanal makineyi başlatın (Azure-AsyncOperation ile 202)
Bu örnek, sanal makineler için **çalıştırmayı başlatma** durumunun nasıl belirlenebildiğini gösterir. İlk istek aşağıdaki biçimdedir:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2016-03-30
```

Durum kodu 202'yi döndürür. Üstbilgi değerleri arasında şunları görürsünüz:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2016-03-30
```

Eşzamanlı işlemin durumunu denetlemek için, bu URL'ye başka bir istek gönderme.

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

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Kaynakları dağıtma (Azure-AsyncOperation ile 201)

Bu örnek, kaynakları Azure'a dağıtmak için **dağıtım** işleminin durumunu nasıl belirleyeceklerini gösterir. İlk istek aşağıdaki biçimdedir:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2016-09-01
```

Durum kodu 201'i döndürür. Yanıtın gövdesi şunları içerir:

```json
"provisioningState":"Accepted",
```

Üstbilgi değerleri arasında şunları görürsünüz:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2016-09-01
```

Eşzamanlı işlemin durumunu denetlemek için, bu URL'ye başka bir istek gönderme.

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

### <a name="create-storage-account-202-with-location-and-retry-after"></a>Depolama hesabı oluşturma (Konum ve Yeniden Deneme-Sonra ile 202)

Bu örnek, depolama hesapları için **oluşturma** işleminin durumunu nasıl belirleyeceklerini gösterir. İlk istek aşağıdaki biçimdedir:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2016-01-01
```

İstek gövdesi depolama hesabının özelliklerini içerir:

```json
{ "location": "South Central US", "properties": {}, "sku": { "name": "Standard_LRS" }, "kind": "Storage" }
```

Durum kodu 202'yi döndürür. Üstbilgi değerleri arasında aşağıdaki iki değer e görebilirsiniz:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
Retry-After: 17
```

Yeniden Deneme-Sonra'da belirtilen saniye sayısını bekledikten sonra, bu URL'ye başka bir istek göndererek eşzamanlı işlemin durumunu denetleyin.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2016-01-01
```

İstek hala çalışıyorsa, bir durum kodu 202 alırsınız. İstek tamamlandıysa, bir durum kodu 200 alırsınız ve yanıtın gövdesi oluşturulan depolama hesabının özelliklerini içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Her REST işlemi yle ilgili belgeler için [REST API belgelerine](/rest/api/)bakın.
* Kaynak Yöneticisi REST API aracılığıyla şablonları dağıtma hakkında bilgi için kaynak [yöneticisi şablonları ve Kaynak Yöneticisi REST API ile kaynakları](../templates/deploy-rest.md)dağıtın'a bakın.