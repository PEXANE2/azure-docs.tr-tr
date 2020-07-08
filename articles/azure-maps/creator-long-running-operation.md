---
title: Azure haritalar uzun süren Işlem API 'SI
description: Azure haritalar 'da uzun süre çalışan zaman uyumsuz arka plan işleme hakkında bilgi edinin
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 360fc4af688e393bb8639ee773f0bf0de603a425
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83598391"
---
# <a name="creator-long-running-operation-api"></a>Oluşturucu uzun süredir çalışan Işlem API 'SI

Azure Maps 'taki bazı API 'leri [zaman uyumsuz istek-yanıt modelini](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply)kullanır. Bu model, Azure haritalar 'ın yüksek oranda kullanılabilir ve hızlı yanıt veren hizmetler sağlamasına olanak sağlar Bu makalede, Azure Map 'in uzun süreli zaman uyumsuz arka plan işlemenin belirli bir uygulanması açıklanmaktadır.

## <a name="submitting-a-request"></a>İstek gönderiliyor

İstemci uygulaması, bir HTTP API 'sine zaman uyumlu bir çağrı aracılığıyla uzun süre çalışan bir işlem başlatır. Genellikle, bu çağrı bir HTTP POST isteği biçiminde olur. Zaman uyumsuz bir iş yükü başarıyla oluşturulduğunda, API, `202` isteğin kabul edildiğini belirten BIR http durum kodu döndürür. Bu yanıt `Location` , istemcinin uzun süre çalışan işlemin durumunu denetlemek için yoklama süresiyle bir uç noktayı işaret eden bir üst bilgi içerir.

### <a name="example-of-a-success-response"></a>Başarılı yanıt örneği

```HTTP
Status: 202 Accepted
Location: https://atlas.microsoft.com/service/operations/{operationId}

```

Çağrı doğrulamadan geçemezse, API bunun yerine `400` Hatalı bir istek IÇIN http yanıtı döndürür. Yanıt gövdesi, isteğin neden geçersiz olduğu konusunda istemciye daha fazla bilgi sağlayacaktır.

### <a name="monitoring-the-operation-status"></a>İşlem durumunu izleme

Kabul edilen yanıt üst bilgilerinde belirtilen konum uç noktası, uzun süreli işlemin durumunu denetlemek için yoklanabilir. İşlem durumu isteğinden yanıt gövdesi her zaman `status` ve `createdDateTime` özelliklerini içerir. `status`Özelliği, uzun süre çalışan işlemin geçerli durumunu gösterir. Olası durumlar `"NotStarted"` ,, `"Running"` ve içerir `"Succeeded"` `"Failed"` . `createdDateTime`Özelliği, uzun süre çalışan işlemi başlatmak için ilk isteğin yapıldığı zamanı gösterir. Durum ya da olduğunda `"NotStarted"` `"Running"` , `Retry-After` Yanıtla birlikte da bir üst bilgi sağlanacaktır. `Retry-After`Saniye cinsinden ölçülen başlık, işlem durumu API 'sine yönelik sonraki yoklama çağrısının ne zaman yapılması gerektiğini belirlemede kullanılabilir.

### <a name="example-of-running-a-status-response"></a>Durum yanıtı çalıştırma örneği

```HTTP
Status: 200 OK
Retry-After: 30
{
    "operationId": "c587574e-add9-4ef7-9788-1635bed9a87e",
    "createdDateTime": "3/11/2020 8:45:13 PM +00:00",
    "status": "Running"
}
```

## <a name="handling-operation-completion"></a>İşlem tamamlamayı işleme

Uzun süre çalışan işlemi tamamladıktan sonra, yanıtın durumu ya da olur `"Succeeded"` `"Failed"` . Uzun süre çalışan bir işlemden yeni bir kaynak oluşturulduğunda, başarı yanıtı bir HTTP `201 Created` durum kodu döndürür. Yanıt, `Location` kaynak hakkındaki meta verileri gösteren bir üst bilgi de içerecektir. Yeni kaynak oluşturulmadıysa, başarı yanıtı bir HTTP `200 OK` durum kodu döndürür. Bir hata durumunda yanıt durum kodu da `200 OK` kod olacaktır. Ancak, yanıtın `error` gövdede bir özelliği olur. Hata verileri OData hata belirtimine uyar.

### <a name="example-of-success-response"></a>Başarı yanıtı örneği

```HTTP
Status: 201 Created
Location: "https://atlas.microsoft.com/tileset/{tileset-id}"
 {
    "operationId": "c587574e-add9-4ef7-9788-1635bed9a87e",
    "createdDateTime": "3/11/2020 8:45:13 PM +00:00",
    "status": "Succeeded",
    "resourceLocation": "https://atlas.microsoft.com/tileset/{tileset-id}"
}
```

### <a name="example-of-failure-response"></a>Hata yanıtı örneği

```HTTP
Status: 200 OK

{
    "operationId": "c587574e-add9-4ef7-9788-1635bed9a87e",
    "createdDateTime": "3/11/2020 8:45:13 PM +00:00",
    "status": "Failed",
    "error": {
        "code": "InvalidFeature",
        "message": "The provided feature is invalid.",
        "details": {
            "code": "NoGeometry",
            "message": "No geometry was provided with the feature."
        }
    }
}
```
