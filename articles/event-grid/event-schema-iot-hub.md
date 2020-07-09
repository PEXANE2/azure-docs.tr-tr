---
title: Azure IoT Hub Event Grid kaynak olarak
description: Bu makalede, Azure IoT Hub olayları için özellikler ve şema sağlanmaktadır. Kullanılabilir olay türlerini, örnek bir olayı ve olay özelliklerini listeler.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 02ecf8d4df55aa6b4319e40892778f85f94e29a7
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86113658"
---
# <a name="azure-iot-hub-as-an-event-grid-source"></a>Azure IoT Hub Event Grid kaynak olarak
Bu makalede, Azure IoT Hub olayları için özellikler ve şema sağlanmaktadır. Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md). 

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure IoT Hub aşağıdaki olay türlerini yayar:

| Olay türü | Description |
| ---------- | ----------- |
| Microsoft. Devices. DeviceCreated | Bir cihaz IoT Hub 'ına kaydedildiğinde yayımlandı. |
| Microsoft. Devices. DeviceDeleted | IoT Hub 'ından bir cihaz silindiğinde yayımlandı. | 
| Microsoft. Devices. DeviceConnected | Bir cihaz IoT Hub 'ına bağlandığında yayımlandı. |
| Microsoft. Devices. DeviceConnected bağlantısı kesildi | Bir cihazın IoT Hub 'ından bağlantısı kesildiğinde yayımlandı. | 
| Microsoft. Devices. Devicetelemetri | Bir IoT Hub 'ına telemetri iletisi gönderildiğinde yayımlandı. |

Cihaz telemetri olayları dışındaki tüm cihaz olayları, Event Grid tarafından desteklenen tüm bölgelerde genel kullanıma sunulmuştur. Cihaz telemetri olayı genel önizlemede bulunur ve Doğu ABD, Batı ABD, Batı Avrupa, [Azure Kamu](../azure-government/documentation-government-welcome.md), [Azure Çin 21Vianet](/azure/china/china-welcome)ve [Azure Almanya](https://azure.microsoft.com/global-infrastructure/germany/)dışındaki tüm bölgelerde kullanılabilir.

### <a name="example-event"></a>Örnek olay

DeviceConnected ve DeviceConnected olayları için şema aynı yapıya sahiptir. Bu örnek olay bir cihaz IoT Hub 'ına bağlandığında harekete geçirilen bir olay şemasını gösterir:

```json
[{
  "id": "f6bbf8f4-d365-520d-a878-17bf7238abd8", 
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>", 
  "subject": "devices/LogicAppTestDevice", 
  "eventType": "Microsoft.Devices.DeviceConnected", 
  "eventTime": "2018-06-02T19:17:44.4383997Z", 
  "data": {
    "deviceConnectionStateEventInfo": {
      "sequenceNumber":
        "000000000000000001D4132452F67CE200000002000000000000000000000001"
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "moduleId" : "DeviceModuleID"
  }, 
  "dataVersion": "1", 
  "metadataVersion": "1" 
}]
```

Bir telemetri olayı bir IoT Hub gönderildiğinde Devicetelemetri olayı tetiklenir. Bu olay için örnek bir şema aşağıda gösterilmiştir.

```json
[{
  "id": "9af86784-8d40-fe2g-8b2a-bab65e106785",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>", 
  "subject": "devices/LogicAppTestDevice", 
  "eventType": "Microsoft.Devices.DeviceTelemetry",
  "eventTime": "2019-01-07T20:58:30.48Z",
  "data": {        
      "body": {            
          "Weather": {                
              "Temperature": 900            
          },
          "Location": "USA"        
      },
        "properties": {            
          "Status": "Active"        
        },
        "systemProperties": {            
            "iothub-content-type": "application/json",
            "iothub-content-encoding": "utf-8",
            "iothub-connection-device-id": "d1",
            "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
            "iothub-connection-auth-generation-id": "123455432199234570",
            "iothub-enqueuedtime": "2019-01-07T20:58:30.48Z",
            "iothub-message-source": "Telemetry"        
        }    
    },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

DeviceCreated ve DeviceDeleted olaylarının şeması aynı yapıya sahiptir. Bu örnek olay, bir cihaz IoT Hub 'ına kaydedildiğinde oluşan bir olayın şemasını gösterir:

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "deviceEtag": "null",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Olay özellikleri

Tüm olaylar, en üst düzey verileri içerir: 

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| kimlik | string | Etkinliğin benzersiz tanımlayıcısı. |
| konu başlığı | string | Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Türü | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| veriler | nesne | Olay verilerini IoT Hub.  |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Tüm IoT Hub olayları için veri nesnesi aşağıdaki özellikleri içerir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| hubName | string | Cihazın oluşturulduğu veya silindiği IoT Hub adı. |
| deviceId | string | Cihazın benzersiz tanımlayıcısı. Bu büyük/küçük harf duyarlı dize en fazla 128 karakter uzunluğunda olabilir ve ASCII 7 bit alfasayısal karakterlerini ve şu özel karakterleri destekler: `- : . + % _ # * ? ! ( ) , = @ ; $ '` . |

Veri nesnesinin içeriği her bir olay yayımcısı için farklıdır. 

**Cihaz bağlı** ve **cihaz bağlantısı kesik** IoT Hub olayları için veri nesnesi aşağıdaki özellikleri içerir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| Modül kimliği | string | Modülün benzersiz tanımlayıcısı. Bu alan yalnızca modül cihazları için çıktıdır. Bu büyük/küçük harf duyarlı dize en fazla 128 karakter uzunluğunda olabilir ve ASCII 7 bit alfasayısal karakterlerini ve şu özel karakterleri destekler: `- : . + % _ # * ? ! ( ) , = @ ; $ '` . |
| Deviceconnectionstateeventınfo | nesne | Cihaz bağlantısı durumu olay bilgileri
| sequenceNumber | string | Cihaz bağlı veya cihaz bağlantısı kesilen olayların sırasını gösteren bir sayı. En son olay, önceki olaydan daha yüksek bir sıra numarasına sahip olacaktır. Bu sayı 1 ' den fazla değişebilir, ancak kesinlikle artmaktadır. Bkz. [sıra numarası kullanımı](../iot-hub/iot-hub-how-to-order-connection-state-events.md). |

**Cihaz telemetri** IoT Hub olayında veri nesnesi, [IoT Hub ileti biçiminde](../iot-hub/iot-hub-devguide-messages-construct.md) cihazdan buluta ileti içerir ve aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| body | string | Cihazdaki iletinin içeriği. |
| properties | string | Uygulama özellikleri, iletiye eklenebilen Kullanıcı tanımlı dizelerdir. Bu alanlar isteğe bağlıdır. |
| Sistem Özellikleri | string | [Sistem Özellikleri](../iot-hub/iot-hub-devguide-routing-query-syntax.md#system-properties) , iletilerin içeriğini ve kaynağını belirlemesine yardımcı olur. Cihaz telemetri iletisi, contentType ' ın JSON ve Contentenkodlamaya ayarlanmış olarak, ileti sistemi özelliklerinde UTF-8 olarak ayarlanmış geçerli bir JSON biçiminde olmalıdır. Bu ayarlanmamışsa, IoT Hub temel 64 kodlu biçimdeki iletileri yazar.  |

**Oluşturulan cihaz** ve **cihaz IoT Hub silinen** olaylar için, veri nesnesi aşağıdaki özellikleri içerir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| ikizi | nesne | Uygulama aygıtı meta verilerinin bulut temsili olan cihaz ikizi hakkında bilgiler. | 
| DeviceID | string | Cihazın benzersiz tanımlayıcısı ikizi. | 
| özelliği | string | Bir cihaz ikizi güncelleştirmelerinin tutarlılığını sağlamaya yönelik bir Doğrulayıcı. Her ETag 'in cihaz ikizi başına benzersiz olması garanti edilir. |  
| deviceEtag| string | Bir cihaz kayıt defterinde güncelleştirmelerin tutarlılığını sağlamaya yönelik bir Doğrulayıcı. Her deviceEtag 'in cihaz kayıt defteri başına benzersiz olması garanti edilir. |
| durum | string | Cihaz ikizi etkin veya devre dışı bırakılmış olsun. | 
| statusUpdateTime | string | Son cihaz ikizi durumu güncelleştirmesinin ıSO8601 zaman damgası. |
| connectionState | string | Cihazın bağlı veya bağlantısı kesilmiş olup olmadığı. | 
| lastActivityTime | string | Son etkinliğin ıSO8601 zaman damgası. | 
| cloudToDeviceMessageCount | integer | Bu cihaza gönderilen cihaz iletileri için bulut sayısı. | 
| authenticationType | string | Bu cihaz için kullanılan kimlik doğrulaması türü: `SAS` , `SelfSigned` , veya `CertificateAuthority` . |
| x509Thumbprint | string | Parmak izi, bir sertifika deposundaki belirli bir sertifikayı bulmak için yaygın olarak kullanılan x509 sertifikası için benzersiz bir değerdir. Parmak izi, SHA1 algoritması kullanılarak dinamik olarak oluşturulur ve sertifikada fiziksel olarak bulunmaz. | 
| Primaryparmak Izi | string | X509 sertifikası için birincil parmak izi. |
| Secondaryparmak Izi | string | X509 sertifikası için ikincil parmak izi. | 
| sürüm | integer | Cihaz ikizi her güncelleştirildiği zaman arttırılan bir tamsayı. |
| istediðiniz | nesne | Özelliklerin bir kısmı yalnızca uygulama arka ucu tarafından yazılabilir ve cihaz tarafından okunabilir. | 
| bildirilen | nesne | Özelliklerin bir kısmı yalnızca cihaz tarafından yazılabilir ve uygulama arka ucu tarafından okunabilir. |
| lastUpdated | string | Son Device ikizi özelliği güncelleştirmesinin ıSO8601 zaman damgası. | 

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık  |Açıklama  |
|---------|---------|
| [Logic Apps kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderme](publish-iot-hub-events-to-logic-apps.md) | Mantıksal uygulama, IoT Hub bir cihaz her eklendiğinde bir bildirim e-postası gönderir. |
| [Eylemleri tetiklemek için Event Grid kullanarak IoT Hub olaylara tepki verme](../iot-hub/iot-hub-event-grid.md) | IoT Hub Event Grid tümleştirilmesine genel bakış. |
| [Cihaza bağlı ve cihaz bağlantısı kesilen olayları Sırala](../iot-hub/iot-hub-how-to-order-connection-state-events.md) | Cihaz bağlantısı durum olaylarının nasıl sipariş alınacağını gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* IoT Hub ve Event Grid birlikte nasıl çalıştığı hakkında bilgi edinmek için bkz. [eylemleri tetiklemek için Event Grid kullanarak IoT Hub olaylara tepki](../iot-hub/iot-hub-event-grid.md)verme.