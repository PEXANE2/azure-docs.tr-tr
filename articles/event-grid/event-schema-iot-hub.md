---
title: Olay Izgara kaynağı olarak Azure IoT Hub
description: Bu makalede, Azure IoT Hub olaylarının özellikleri ve şeması sağlanmaktadır. Kullanılabilir olay türlerini, örnek olayı ve olay özelliklerini listeler.
services: iot-hub
documentationcenter: ''
author: spelluru
editor: ''
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: spelluru
ms.openlocfilehash: f9bf807884ab5592fa320532f3ca10a223081263
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393320"
---
# <a name="azure-iot-hub-as-an-event-grid-source"></a>Olay Izgara kaynağı olarak Azure IoT Hub
Bu makalede, Azure IoT Hub olaylarının özellikleri ve şeması sağlanmaktadır. Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın. 

## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure IoT Hub aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Bir aygıt bir IoT hub'ına kaydedildiğinde yayımlanır. |
| Microsoft.Devices.DeviceSilindi | Bir aygıt Bir IoT hub'ından silindiğinde yayımlanır. | 
| Microsoft.Devices.DeviceConnected | Bir aygıt bir IoT hub'ına bağlandığında yayımlanır. |
| Microsoft.Devices.DeviceBağlantısı Kesildi | Aygıtın IoT hub'ından bağlantısı kesildiğinde yayımlanır. | 
| Microsoft.Devices.DeviceTelemetry | Bir telemetri iletisi Bir IoT hub'ına gönderildiğinde yayımlanır. |

Aygıt telemetri etkinlikleri dışındaki tüm aygıt olayları genellikle Olay Izgaratarafından desteklenen tüm bölgelerde kullanılabilir. Aygıt telemetri etkinliği genel önizlemededir ve Doğu ABD, Batı ABD, Batı Avrupa, [Azure Hükümeti,](../azure-government/documentation-government-welcome.md) [Azure China 21Vianet](/azure/china/china-welcome)ve [Azure Almanya](https://azure.microsoft.com/global-infrastructure/germany/)dışındaki tüm bölgelerde kullanılabilir.

### <a name="example-event"></a>Örnek olay

DeviceConnected ve DeviceDisconnected olayları için şema aynı yapıya sahiptir. Bu örnek olay, bir aygıt bir IoT hub'ına bağlandığında ortaya çıkan bir olayın şemasını gösterir:

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

Bir telemetri olayı Bir IoT Hub'ına gönderildiğinde DeviceTelemetry olayı yükseltilir. Bu olay için örnek bir şema aşağıda gösterilmiştir.

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

DeviceCreated ve DeviceDeleted olayları için şema aynı yapıya sahiptir. Bu örnek olay, bir aygıt bir IoT hub'ına kaydedildiğinde ortaya çıkan bir olayın şemasını gösterir:

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

Tüm olaylar aynı üst düzey verileri içerir: 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| veri | object | IoT Hub olay verileri.  |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Tüm IoT Hub olayları için veri nesnesi aşağıdaki özellikleri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| hubName | string | Aygıtın oluşturulduğu veya silindiği IoT Hub'ının adı. |
| deviceId | string | Aygıtın benzersiz tanımlayıcısı. Bu büyük/küçük harf duyarlı dize 128 karakter uzunluğunda olabilir ve ASCII 7-bit alfanümerik `- : . + % _ # * ? ! ( ) , = @ ; $ '`karakterleri artı aşağıdaki özel karakterleri destekler: . |

Veri nesnesinin içeriği her olay yayımcısı için farklıdır. 

**AygıtA Bağlı** ve **Aygıt Bağlantısı Kesilen** IoT Hub olayları için veri nesnesi aşağıdaki özellikleri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| moduleId | string | Modülün benzersiz tanımlayıcısı. Bu alan yalnızca modül aygıtları için çıktıdır. Bu büyük/küçük harf duyarlı dize 128 karakter uzunluğunda olabilir ve ASCII 7-bit alfanümerik `- : . + % _ # * ? ! ( ) , = @ ; $ '`karakterleri artı aşağıdaki özel karakterleri destekler: . |
| deviceConnectionStateEventInfo | object | Aygıt bağlantısı durumu olay bilgileri
| Sequencenumber | string | Aygıta bağlı veya aygıt bağlantısı kesilen olayların sırasını göstermeye yardımcı olan bir sayı. Son olay önceki olaydaha yüksek bir sıra numarası olacaktır. Bu sayı 1'den fazla değişebilir, ancak kesinlikle artmaktadır. [Sıra numarasının nasıl kullanılacağını](../iot-hub/iot-hub-how-to-order-connection-state-events.md)görün. |

**Aygıt Telemetrii** IoT Hub olayı için, veri nesnesi [IoT hub ileti biçiminde](../iot-hub/iot-hub-devguide-messages-construct.md) aygıttan buluta iletiiçerir ve aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| body | string | Aygıttan gelen iletinin içeriği. |
| properties | string | Uygulama özellikleri, iletiye eklenebilecek kullanıcı tanımlı dizeleridir. Bu alanlar isteğe bağlıdır. |
| sistem özellikleri | string | [Sistem özellikleri,](../iot-hub/iot-hub-devguide-routing-query-syntax.md#system-properties) iletilerin içeriğini ve kaynağını belirlemeye yardımcı olur. Aygıt telemetri iletisi, json için içerikType ayarlanmış ve ileti sistemi özellikleriUTF-8 için ayarlanmış contentEncoding ile geçerli bir JSON biçiminde olmalıdır. Bu ayarlanmazsa, IoT Hub iletileri temel 64 kodlanmış biçimde yazar.  |

**Aygıt Oluşturulduve** **Aygıt Silinmiş** IoT Hub olayları için veri nesnesi aşağıdaki özellikleri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Ikiz | object | Uygulama aygıtı meta verilerinin bulut gösterimi olan aygıt ikizi hakkında bilgi. | 
| Deviceıd | string | Aygıt ikizinin benzersiz tanımlayıcısı. | 
| Etag | string | Aygıt ikizi güncelleştirmelerinin tutarlılığını sağlamak için bir doğrulayıcı. Her etag cihaz ikiz başına benzersiz olması garanti edilir. |  
| deviceEtag| string | Aygıt kayıt defterinde güncelleştirmelerin tutarlılığını sağlamak için bir doğrulayıcı. Her deviceEtag'ın cihaz kayıt defteri başına benzersiz olacağı garanti edilir. |
| durum | string | Aygıt ikizi etkin veya devre dışı bırakılmış olsun. | 
| statusUpdateTime | string | Son cihazın iso8601 zaman damgası ikiz durum güncellemesi. |
| bağlantıDevlet | string | Aygıtın bağlı veya bağlantısı nın kesilip kesilmediği. | 
| lastActivityTime | string | Son etkinliğin ISO8601 zaman damgası. | 
| cloudToDeviceMessageCount | integer | Bu aygıta gönderilen aygıt iletilerine bulut sayısı. | 
| authenticationType | string | Bu aygıt için kullanılan kimlik `SAS` `SelfSigned`doğrulama `CertificateAuthority`türü: |
| x509Thumbprint | string | Parmak izi, genellikle bir sertifika deposunda belirli bir sertifikayı bulmak için kullanılan x509 sertifikası için benzersiz bir değerdir. Parmak izi SHA1 algoritması kullanılarak dinamik olarak oluşturulur ve sertifikada fiziksel olarak bulunmaz. | 
| birincilThumbprint | string | x509 sertifikası için birincil parmak izi. |
| ikincilThumbprint | string | x509 sertifikası için ikincil parmak izi. | 
| version | integer | Aygıt ikizi her güncelleştiride bir oranında artımlı bir arayıcı. |
| Istenen | object | Yalnızca uygulama arka uç tarafından yazılabilir ve aygıt tarafından okunabilir özelliklerinin bir kısmı. | 
| Bildirilen | object | Yalnızca aygıt tarafından yazılabilir ve uygulama arka uç tarafından okunabilir özelliklerinin bir kısmı. |
| lastUpdated | string | Son aygıt ikiz özellik güncelleştirmesinin ISO8601 zaman damgası. | 

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık  |Açıklama  |
|---------|---------|
| [Logic Apps kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderme](publish-iot-hub-events-to-logic-apps.md) | Bir mantık uygulaması, IoT Hub'ınıza her cihaz ekleninher bir bildirim e-postası gönderir. |
| [Eylemleri tetiklemek için Olay Izgarasını kullanarak IoT Hub olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md) | IoT Hub'ı Olay Izgarasıyla tümleştirmeye genel bakış. |
| [Aygıta bağlı ve aygıt bağlantısı kesilmiş olayları sipariş edin](../iot-hub/iot-hub-how-to-order-connection-state-events.md) | Aygıt bağlantısı durum olaylarının nasıl sıralanın yapılacağını gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* IoT Hub ve Olay Izgarası'nın birlikte nasıl çalıştığı hakkında bilgi edinmek [için, eylemleri tetiklemek için Olay Grid'i kullanarak IoT Hub olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md)konusuna bakın.