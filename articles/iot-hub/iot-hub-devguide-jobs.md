---
title: Azure IoT Hub işlerini anlayın | Microsoft Docs
description: Geliştirici Kılavuzu-iş planlama işleri, IoT Hub 'ınıza bağlı birden çok cihazda çalışacak şekilde çalışır. İşler, etiketleri ve istenen özellikleri güncelleştirebilir ve birden çok cihazda doğrudan yöntemleri çağırabilir.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: mqtt
ms.openlocfilehash: 5c14e8cfcbf8df86b0f71d6b12025594d2e648c4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81730098"
---
# <a name="schedule-jobs-on-multiple-devices"></a>Birden fazla cihazda işleri zamanlama

Azure IoT Hub, [Device ikizi özellikleri ve etiketleri](iot-hub-devguide-device-twins.md) ve [doğrudan yöntemleri](iot-hub-devguide-direct-methods.md)gibi çeşitli yapı taşlarına izin vermez. Tipik olarak, arka uç uygulamaları cihaz yöneticilerinin ve işleçlerin, IoT cihazlarını toplu ve zamanlanan bir zamanda güncelleştirme ve bunlarla etkileşime geçmesini sağlar. İşler cihaz ikizi güncelleştirmelerini ve doğrudan yöntemleri zamanlanan bir süre içinde bir cihaz kümesine karşı yürütür. Örneğin, bir operatör, derleme işlemlerine kesintiye uğramayan bir zamanda 43 ve 2. binadaki bir cihaz kümesini yeniden başlatmak için bir işi başlatan ve izleyen bir arka uç uygulaması kullanır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bir cihaz kümesi üzerinde aşağıdaki etkinliklerden herhangi birini zamanlamanıza ve izlemenize gerek duyduğunuzda işleri kullanmayı düşünün:

* İstenen özellikleri güncelleştirme
* Etiketleri Güncelleştir
* Doğrudan metotları çağır

## <a name="job-lifecycle"></a>İş yaşam döngüsü

İşler, çözüm arka ucu tarafından başlatılır ve IoT Hub tarafından sürdürülür. Hizmete yönelik bir URI (`PUT https://<iot hub>/jobs/v2/<jobID>?api-version=2018-06-30`) aracılığıyla bir işi başlatabilir ve bir HIZMET temelli URI (`GET https://<iot hub>/jobs/v2/<jobID?api-version=2018-06-30`) aracılığıyla yürütülen iş üzerinde ilerleme durumunu sorgulayın. İş başlatıldığında çalışan işlerin durumunu yenilemek için bir iş sorgusu çalıştırın.

> [!NOTE]
> Bir iş başlattığınızda, özellik adları ve değerleri yalnızca ABD-ASCII yazdırılabilir alfasayısal içerebilir (aşağıdaki küme dışında):`$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`

## <a name="jobs-to-execute-direct-methods"></a>Doğrudan Yöntemler yürütülecek işler

Aşağıdaki kod parçacığında, bir işi kullanarak bir dizi cihazda [doğrudan yöntem](iot-hub-devguide-direct-methods.md) YÜRÜTMEYE yönelik https 1,1 istek ayrıntıları gösterilmektedir:

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

Sorgu koşulu, aşağıdaki örneklerde gösterildiği gibi tek bir cihaz KIMLIĞINDE ya da cihaz kimliklerinin bir listesinde bulunabilir:

```
"queryCondition" = "deviceId = 'MyDevice1'"
"queryCondition" = "deviceId IN ['MyDevice1','MyDevice2']"
"queryCondition" = "deviceId IN ['MyDevice1']"
```

[IoT Hub sorgu dili](iot-hub-devguide-query-language.md) IoT Hub sorgu dilini ek ayrıntılarla kapsamaktadır.

Aşağıdaki kod parçacığında, contoso-hub-1 ' deki tüm cihazlarda testMethod adlı doğrudan bir yöntemi çağırmak üzere zamanlanan bir işin isteği ve yanıtı gösterilmektedir:

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

## <a name="jobs-to-update-device-twin-properties"></a>Cihaz ikizi özelliklerini güncelleştirmek için işler

Aşağıdaki kod parçacığında, bir işi kullanarak Device ikizi özelliklerini güncelleştirmek için HTTPS 1,1 istek ayrıntıları gösterilmektedir:

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
> *UpdateTwin* özelliği geçerli bir ETag eşleşmesi gerektirir; Örneğin, `etag="*"`.

Aşağıdaki kod parçacığında, contoso-hub-1 ' deki test cihazının cihaz ikizi özelliklerini güncelleştirmek üzere zamanlanan bir iş için istek ve yanıt gösterilmektedir:

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

## <a name="querying-for-progress-on-jobs"></a>İşlerin ilerleme durumunu sorgulama

Aşağıdaki kod parçacığında, işleri sorgulamak için HTTPS 1,1 istek ayrıntıları gösterilmektedir:

```
GET /jobs/v2/query?api-version=2018-06-30[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
```

ContinuationToken, yanıttan sağlanır.

Her cihazda iş yürütme durumunu sorgulama işlemini cihaz İKESİ [, işler ve ileti yönlendirme için IoT Hub sorgu dilini](iot-hub-devguide-query-language.md)kullanarak yapabilirsiniz.

## <a name="jobs-properties"></a>İş özellikleri

Aşağıdaki listede, işler veya iş sonuçları sorgulanırken kullanılabilecek özellikler ve ilgili açıklamalar gösterilmektedir.

| Özellik | Açıklama |
| --- | --- |
| **No** |İş için uygulama KIMLIĞI belirtildi. |
| **startTime** |Uygulama için başlangıç saati (ISO-8601). |
| **endTime** |İşin tamamlandığı tarih (ISO-8601) IoT Hub. Yalnızca iş ' tamamlandı ' durumuna ulaştıktan sonra geçerlidir. |
| **türüyle** |İş türleri: |
| | **scheduleUpdateTwin**: istenen özelliklerden veya etiketlerin bir kümesini güncelleştirmek için kullanılan bir iş. |
| | **Scheduledevicemethod**: bir cihaz-WINS kümesi üzerinde bir cihaz yöntemi çağırmak için kullanılan bir iş. |
| **durumlarına** |İşin geçerli durumu. Durum için olası değerler: |
| | **bekliyor**: zamanlanan ve iş hizmeti tarafından çekilmek üzere bekleniyor. |
| | **zamanlandı**: gelecekte bir zaman için zamanlandı. |
| | **çalışıyor**: Şu anda etkin iş. |
| | **iptal edildi**: iş iptal edildi. |
| | **başarısız**: iş başarısız oldu. |
| | **tamamlandı**: iş tamamlandı. |
| **deviceJobStatistics** |İşin yürütülmesiyle ilgili istatistikler. |
| | **Devicejobstatistics** özellikleri: |
| | **Devicejobstatistics. deviceCount**: işteki cihaz sayısı. |
| | **Devicejobstatistics. failedCount**: işin başarısız olduğu cihaz sayısı. |
| | **Devicejobstatistics. succeededCount**: işin başarılı olduğu cihaz sayısı. |
| | **Devicejobstatistics. runningCount**: Şu anda işi çalıştıran cihazların sayısı. |
| | **Devicejobstatistics. pendingCount**: işi çalıştırmak için bekleyen cihazların sayısı. |

### <a name="additional-reference-material"></a>Ek başvuru malzemeleri

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunları içerir:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md) , her bir IoT Hub 'ının çalışma zamanı ve yönetim işlemleri için sunduğu çeşitli uç noktaları açıklar.

* [Daraltma ve Kotalar](iot-hub-devguide-quotas-throttling.md) , IoT Hub hizmetine uygulanan kotaları ve hizmeti kullandığınızda bekleneceğiniz azaltma davranışını tanımlar.

* [Azure IoT cihaz ve hizmet SDK 'ları](iot-hub-devguide-sdks.md) , IoT Hub etkileşimde bulunan cihaz ve hizmet uygulamaları geliştirirken kullanabileceğiniz çeşitli dil SDK 'larını listeler.

* [Cihaz TWINS, işler ve ileti yönlendirme için IoT Hub sorgu dili,](iot-hub-devguide-query-language.md) IoT Hub sorgu dilini açıklar. Cihaz ikikinizin ve işleriniz hakkında IoT Hub bilgi almak için bu sorgu dilini kullanın.

* [MQTT desteği IoT Hub](iot-hub-mqtt-support.md) MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede açıklanan kavramların bazılarını denemek için aşağıdaki IoT Hub öğreticisine bakın:

* [İşleri zamanlama ve yayınlama](iot-hub-node-node-schedule-jobs.md)