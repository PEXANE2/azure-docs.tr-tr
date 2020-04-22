---
title: Azure IoT Hub işlerini anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT hub'ınıza bağlı birden çok cihazda çalışacak işleri zamanlama. İşler etiketleri ve istenen özellikleri güncelleyebilir ve birden çok cihazda doğrudan yöntemleri çağırabilir.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: mqtt
ms.openlocfilehash: 5c14e8cfcbf8df86b0f71d6b12025594d2e648c4
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730098"
---
# <a name="schedule-jobs-on-multiple-devices"></a>Birden fazla cihazda işleri zamanlama

Azure IoT Hub, aygıt ikiz [özellikleri ve etiketleri ve](iot-hub-devguide-device-twins.md) doğrudan yöntemler gibi bir dizi yapı taşını etkinleştirir. [direct methods](iot-hub-devguide-direct-methods.md) Genellikle, arka uç uygulamaları aygıt yöneticilerinin ve operatörlerin IoT aygıtlarını toplu olarak ve zamanlanmış bir zamanda güncelleştirmelerine ve bunlarla etkileşimde kalmalarını sağlar. İşler, aygıt ikiz güncelleştirmelerini ve zamanlanmış bir anda bir dizi aygıta karşı doğrudan yöntemleri çalıştırın. Örneğin, bir operatör, binanın işleyişini kesintiye uğratmayacak şekilde bina 43 ve kat 3'teki bir dizi aygıtı yeniden başlatmak için bir işi başlatan ve izleyen bir arka uç uygulaması kullanır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bir aygıt kümesinde aşağıdaki etkinliklerden herhangi birini zamanlamanız ve izlemeniz gerektiğinde işleri kullanmayı düşünün:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştir
* Doğrudan yöntemleri çağırma

## <a name="job-lifecycle"></a>İş yaşam döngüsü

İşler çözüm arka uç tarafından başlatılır ve IoT Hub tarafından korunur. Bir hizmeti bakan URI (`PUT https://<iot hub>/jobs/v2/<jobID>?api-version=2018-06-30`) ve hizmete bakan URI ( ) aracılığıyla yürütme işi`GET https://<iot hub>/jobs/v2/<jobID?api-version=2018-06-30`ilerleme için sorgu yoluyla bir iş başlatabilirsiniz. Bir iş başlatıldıktan sonra işleri çalıştırma durumunu yenilemek için bir iş sorgusu çalıştırın.

> [!NOTE]
> Bir işi başlattığınızda, özellik adları ve değerleri, aşağıdaki kümedekiler dışında yalnızca US-ASCII yazdırılabilir alfasayısal içerebilir:`$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`

## <a name="jobs-to-execute-direct-methods"></a>Doğrudan yöntemleri çalıştıracak işler

Aşağıdaki parçacık, bir işi kullanan bir aygıt kümesinde [doğrudan](iot-hub-devguide-direct-methods.md) bir yöntem yürütmek için HTTPS 1.1 istek ayrıntılarını gösterir:

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8

{
    "jobId": "<jobId>",
    "type": "scheduleDeviceMethod",
    "cloudToDeviceMethod": {
        "methodName": "<methodName>",
        "payload": <payload>,
        "responseTimeoutInSeconds": methodTimeoutInSeconds
    },
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

Sorgu koşulu, aşağıdaki örneklerde gösterildiği gibi tek bir aygıt kimliğinde veya aygıt kimlikleri listesinde de bulunabilir:

```
"queryCondition" = "deviceId = 'MyDevice1'"
"queryCondition" = "deviceId IN ['MyDevice1','MyDevice2']"
"queryCondition" = "deviceId IN ['MyDevice1']"
```

[IoT Hub Sorgu Dili,](iot-hub-devguide-query-language.md) IoT Hub sorgu dilini ek ayrıntılarla kapsar.

Aşağıdaki snippet contoso-hub-1 tüm cihazlarda testMethod adlı doğrudan bir yöntem çağırmak için zamanlanan bir iş için istek ve yanıt gösterir:

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job01?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=68iv------------------------------------v8Hxalg%3D&se=1556849884&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 317

{
    "jobId": "job01",
    "type": "scheduleDeviceMethod",
    "cloudToDeviceMethod": {
        "methodName": "testMethod",
        "payload": {},
        "responseTimeoutInSeconds": 30
    },
    "queryCondition": "*", 
    "startTime": "2019-05-04T15:53:00.077Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 65
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 01:46:18 GMT

{"jobId":"job01","type":"scheduleDeviceMethod","status":"queued"}
```

## <a name="jobs-to-update-device-twin-properties"></a>Aygıt ikiz özelliklerini güncelleştirme işleri

Aşağıdaki parçacık, bir işi kullanarak aygıt ikiz özelliklerini güncelleştirmek için HTTPS 1.1 istek ayrıntılarını gösterir:

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8

{
    "jobId": "<jobId>",
    "type": "scheduleUpdateTwin",
    "updateTwin": <patch>                 // Valid JSON object
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

> [!NOTE]
> *UpdateTwin* özelliği geçerli bir etag eşleşmesi gerektirir; örneğin, `etag="*"`.

Aşağıdaki snippet, contoso-hub-1'deki test aygıtı için aygıt ikiz özelliklerini güncelleştirmek üzere zamanlanan bir iş için istek ve yanıtı gösterir:

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job02?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=BN0U-------------------------------------RuA%3D&se=1556925787&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 339

{
    "jobId": "job02",
    "type": "scheduleUpdateTwin",
    "updateTwin": {
      "properties": {
        "desired": {
          "test1": "value1"
        }
      },
     "etag": "*"
     },
    "queryCondition": "deviceId = 'test-device'",
    "startTime": "2019-05-08T12:19:56.868Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 63
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 22:45:13 GMT

{"jobId":"job02","type":"scheduleUpdateTwin","status":"queued"}
```

## <a name="querying-for-progress-on-jobs"></a>İşlerde ilerleme için sorgulama

Aşağıdaki parçacık, iş sorgulama için HTTPS 1.1 istek ayrıntılarını gösterir:

```
GET /jobs/v2/query?api-version=2018-06-30[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
```

DevamıToken yanıt tan sağlanır.

[Aygıt ikizleri, işleri ve ileti yönlendirmesi için IoT Hub sorgu dilini](iot-hub-devguide-query-language.md)kullanarak her aygıttaki iş yürütme durumunu sorgulayabilirsiniz.

## <a name="jobs-properties"></a>İşler Özellikleri

Aşağıdaki liste, iş veya iş sonuçları için sorgulanırken kullanılabilecek özellikleri ve ilgili açıklamaları gösterir.

| Özellik | Açıklama |
| --- | --- |
| **Jobıd** |Başvuru iş için kimlik sağladı. |
| **startTime** |Uygulama iş için başlangıç saati (ISO-8601) sağladı. |
| **endTime** |IoT Hub, işin ne zaman tamamlandığı na ilişkin tarih (ISO-8601) sağladı. Yalnızca iş 'tamamlanmış' duruma ulaştıktan sonra geçerlidir. |
| **Türü** |İş türleri: |
| | **scheduleUpdateTwin**: İstenilen özellikler veya etiketler kümesini güncelleştirmek için kullanılan bir iş. |
| | **scheduleDeviceMethod**: Aygıt ikizlerinin setinde bir cihaz yöntemini çağırmak için kullanılan iş. |
| **Durum** |İşin şu anki durumu. Durum için olası değerler: |
| | **bekleme :** Planlanmış ve iş hizmeti tarafından alınmayı bekliyor. |
| | **scheduled**: Gelecekte bir süre için zamanlanır. |
| | **running**: Şu anda etkin iş. |
| | **iptal :** İş iptal edildi. |
| | **başarısız**: İş başarısız oldu. |
| | **tamamladı**: İş tamamlandı. |
| **cihazJobStatistics** |İşin yürütülmesiyle ilgili istatistikler. |
| | **deviceJobStatistics** özellikleri: |
| | **deviceJobStatistics.deviceCount**: İşteki aygıt sayısı. |
| | **deviceJobStatistics.failedCount**: İşin başarısız olduğu aygıt sayısı. |
| | **deviceJobStatistics.succeededCount**: İşin başarılı olduğu aygıt sayısı. |
| | **deviceJobStatistics.runningCount**: Şu anda işi çalıştıran aygıt sayısı. |
| | **deviceJobStatistics.pendingCount**: İşi çalıştırmak için bekleyen aygıt sayısı. |

### <a name="additional-reference-material"></a>Ek referans materyali

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları,](iot-hub-devguide-endpoints.md) her IoT hub'ın çalışma zamanı ve yönetim işlemleri için ortaya çıkardığı çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar,](iot-hub-devguide-quotas-throttling.md) IoT Hub hizmetine uygulanan kotaları ve hizmeti kullandığınızda bekleyeceğiniz azaltma davranışını açıklar.

* [Azure IoT aygıt ve hizmet SDK'ları,](iot-hub-devguide-sdks.md) IoT Hub ile etkileşimde bulunan hem aygıt hem de hizmet uygulamaları geliştirdiğinizde kullanabileceğiniz çeşitli dil SDK'larını listeler.

* [Aygıt ikizleri, işleri ve ileti yönlendirmesi için IoT Hub sorgu dili,](iot-hub-devguide-query-language.md) IoT Hub sorgu dilini açıklar. IoT Hub'dan aygıtınız ve işleriniz hakkında bilgi almak için bu sorgu dilini kullanın.

* [IoT Hub MQTT desteği,](iot-hub-mqtt-support.md) MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede açıklanan bazı kavramları denemek için aşağıdaki IoT Hub öğreticisine bakın:

* [İşleri zamanlama ve yayınlama](iot-hub-node-node-schedule-jobs.md)