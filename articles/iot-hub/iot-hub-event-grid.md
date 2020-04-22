---
title: Azure IoT Hub ve Olay Izgara | Microsoft Dokümanlar
description: IoT Hub'da meydana gelen eylemleri temel alan işlemleri tetiklemek için Azure Olay Ağıt'ı kullanın.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: robinsh
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: a67d90a0888c39938f07c294f8e161ce98fd945a
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732510"
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions"></a>Eylemleri tetiklemek için Olay Izgarasını kullanarak IoT Hub olaylarına tepki verme

Azure IoT Hub, diğer hizmetlere olay bildirimleri gönderebilmeniz ve akış aşağı işlemlerini tetikleyebilmeniz için Azure Etkinlik Ağıt'ı ile tümleşir. Kritik olaylara güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki verebilmeniz için iş uygulamalarınızı IoT Hub olaylarını dinleyecek şekilde yapılandırın.Örneğin, veritabanını güncelleyen, iş cezası oluşturan ve IoT hub'ınıza her yeni Bir IoT aygıtı kaydedilişinde bir e-posta bildirimi sunan bir uygulama oluşturun.

[Azure Olay Ağıt,](../event-grid/overview.md) yayımlama-abone etme modelini kullanan tam olarak yönetilen bir olay yönlendirme hizmetidir. Olay Ağı, [Azure İşlevleri](../azure-functions/functions-overview.md) ve [Azure Mantıksal Uygulamaları](../logic-apps/logic-apps-what-are-logic-apps.md)gibi Azure hizmetleri için yerleşik destek sağlar ve web hook'ları kullanarak Azure'a uygun olmayan hizmetlere etkinlik uyarıları sunabilir. Olay Izgarası'nın desteklediği olay işleyicilerinin tam listesi için Azure [Olay Izgarasına giriş bölümüne](../event-grid/overview.md)bakın.

![Azure Olay Izgara mimarisi](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Olay Izgara tümleştirmesi, Olay Izgarasının desteklendiği bölgelerde bulunan IoT hub'ları için kullanılabilir. En son bölgeler listesi için [Azure Etkinlik Ağıtı'na giriş bölümüne](../event-grid/overview.md)bakın.

## <a name="event-types"></a>Olay türleri

IoT Hub aşağıdaki olay türlerini yayımlar:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Bir aygıt bir IoT hub'ına kaydedildiğinde yayımlanır. |
| Microsoft.Devices.DeviceSilindi | Bir aygıt Bir IoT hub'ından silindiğinde yayımlanır. |
| Microsoft.Devices.DeviceConnected | Bir aygıt bir IoT hub'ına bağlandığında yayımlanır. |
| Microsoft.Devices.DeviceBağlantısı Kesildi | Aygıtın IoT hub'ından bağlantısı kesildiğinde yayımlanır. |
| Microsoft.Devices.DeviceTelemetry | Aygıt telemetri iletisi Bir IoT hub'ına gönderildiğinde yayınlandı |

Her IoT hub'ından hangi olayların yayımlanmasını sağlamak için Azure portalını veya Azure CLI'yi kullanın. Örneğin, [Logic Apps'ı kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönder'i](../event-grid/publish-iot-hub-events-to-logic-apps.md)deneyin.

## <a name="event-schema"></a>Olay şeması

IoT Hub olayları, cihazınızın yaşam döngüsündeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliğinin **Microsoft.Devices**ile başedip başlamadığını denetleyerek bir IoT Hub olayını tanımlayabilirsiniz. Olay Izgara olay özelliklerinin nasıl kullanılacağı hakkında daha fazla bilgi için [Olay Izgara olay şemasına](../event-grid/event-schema.md)bakın.

### <a name="device-connected-schema"></a>Cihaza bağlı şema

Aşağıdaki örnek, aygıta bağlı bir olayın şemalarını gösterir:

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
    "moduleId" : "DeviceModuleID",
  }, 
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="device-telemetry-schema"></a>Cihaz Telemetri şeması

Cihaz telemetri iletisi, ileti [sistemi özelliklerinde](iot-hub-devguide-routing-query-syntax.md#system-properties) **UTF-8'e** ayarlanmış içerikType **uygulaması/json** ve contentEncoding ile geçerli bir JSON formatında olmalıdır. Bu özelliklerin her ikisi de büyük/küçük harf duyarsız. İçerik kodlaması ayarlanmazsa, IoT Hub iletileri temel 64 kodlanmış biçimde yazar.

Aygıt telemetri olaylarını olay ızgarası olarak bitiş noktasını seçerek Olay Izgarası'nda yayımlanmadan önce zenginleştirebilirsiniz. Daha fazla bilgi için [İleti Zenginleştirmelerine Genel Bakış'a](iot-hub-message-enrichments-overview.md)bakın.

Aşağıdaki örnek, bir aygıt telemetri olayının şemagösterir:

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

### <a name="device-created-schema"></a>Cihaz şema oluşturuldu

Aşağıdaki örnek, oluşturulan bir aygıtın şemalarını gösterir:

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
      "deviceEtag":"null",
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

Her özelliğin ayrıntılı bir açıklaması [için, IoT Hub için Azure Olay Izgara olay şemasına](../event-grid/event-schema-iot-hub.md)bakın.

## <a name="filter-events"></a>Olayları filtreleme

IoT Hub etkinlik abonelikleri, olayları olay türüne, veri içeriğine ve cihaz adı olan konuya göre filtreleyebilir.

Olay Izgara olay türleri, konular ve veri içeriği [üzerinde filtreleme](../event-grid/event-filtering.md) sağlar. Olay Izgara aboneliğini oluştururken, seçili IoT etkinliklerine abone olmayı seçebilirsiniz. Olay Izgara'daki konu filtreleri, **Ile Başlar** (önek) ve Ends **With** (soneki) eşleşmelerine göre çalışır. Filtre bir `AND` işleç kullanır, bu nedenle hem önek hem de sonekile eşleşen bir özneiçeren olaylar aboneye teslim edilir.

IoT Events konusu biçimini kullanır:

```json
devices/{deviceId}
```

Olay Izgara da veri içeriği de dahil olmak üzere her olayın öznitelikleri, filtreleme için izin verir. Bu, telemetri iletisinin içeriğine dayalı olarak hangi olayların teslim edileceğini seçmenize olanak tanır. Örnekleri görüntülemek için lütfen [gelişmiş filtreleme](../event-grid/event-filtering.md#advanced-filtering) bakın. Telemetri ileti gövdesinde filtreleme yapmak için, ileti [sistemi özelliklerinde](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#system-properties)contentType'ı **uygulama/json** ve contentEncoding'i **UTF-8** olarak ayarlamanız gerekir. Bu özelliklerin her ikisi de büyük/küçük harf duyarsız.

DeviceConnected, DeviceConnected, DeviceCreated ve DeviceDeleted gibi telemetri dışı olaylariçin, abonelik oluşturulurken Olay Izgara filtresi kullanılabilir. Telemetri olayları için, Olay Izgara'daki filtrelemeye ek olarak, kullanıcılar ileti yönlendirme sorgusu aracılığıyla aygıt ikizleri, ileti özellikleri ve gövdeye de filtre uygulayabilir. 

Olay Izgarası üzerinden telemetri olaylarına abone olduğunuzda, IoT Hub Olay Grid'e veri kaynağı türü aygıt iletileri göndermek için varsayılan bir ileti rotası oluşturur. İleti yönlendirme hakkında daha fazla bilgi için [IoT Hub ileti yönlendirme'ye](iot-hub-devguide-messages-d2c.md)bakın. Bu rota, IoT Hub > İleti Yönlendirme altındaki portalda görünür olacaktır. Telemetri etkinlikleri için oluşturulan EG aboneliklerinin sayısına bakılmaksızın Olay Izgarası'na yalnızca bir rota oluşturulur. Bu nedenle, farklı filtrelere sahip birden fazla aboneliğe ihtiyacınız varsa, bu sorgularda AYNı rotada OR işlecikk kullanabilirsiniz. Rotanın oluşturulması ve silinmesi, Olay Grid üzerinden telemetri olaylarının aboneliği yoluyla denetlenir. IoT Hub İleti Yönlendirme'yi kullanarak Olay Izgarası'na giden bir rota oluşturamaz veya silemezsiniz.

Telemetri verileri gönderilmeden önce iletileri filtrelemek için [yönlendirme sorgunuzu](iot-hub-devguide-routing-query-syntax.md)güncelleştirebilirsiniz. Yönlendirme sorgusunun ileti gövdesine yalnızca gövde JSON ise uygulanabileceğini unutmayın. Ayrıca ileti [sistemi özelliklerinde](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#system-properties)contentType'ı **uygulama/json** ve contentEncoding'i **UTF-8** olarak ayarlamanız gerekir.

## <a name="limitations-for-device-connected-and-device-disconnected-events"></a>Cihazla bağlandı ve cihaz bağlantısı kesildi olaylarına ilişkin sınırlamalar

Aygıt bağlantısı durum olaylarını almak için, bir aygıtın Iot Hub ile bir 'D2C Gönder Telemetri' veya 'C2D İleti Alma' işlemi yapması gerekir. Ancak, bir aygıt Iot Hub'a bağlanmak için AMQP protokolü kullanıyorsa, 'C2D İleti Al' işlemi yapmaları önerilir aksi takdirde bağlantı durumu bildirimleri birkaç dakika gecikebilir. Aygıtınız MQTT protokolü kullanıyorsa, IoT Hub C2D bağlantısını açık tutar. AMQP için, IoT Hub C# SDK için [Async API Al'ı](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet)veya [AMQP için aygıt istemcisini](iot-hub-amqp-support.md#device-client)arayarak C2D bağlantısını açabilirsiniz.

Telemetri gönderiyorsanız D2C bağlantısı açıktır. 

Aygıt bağlantısı titriyorsa, bu da aygıtın sık sık bağlanıp bağlantılarının kesildiği anlamına geliyorsa, her bir bağlantı durumunu göndermeyiz, ancak titreme devam edene kadar periyodik anlık görüntüde alınan geçerli bağlantı durumunu yayımlayacağız. Farklı sıra numaraları veya farklı bağlantı durumu olayları ile aynı bağlantı durumu olay alma her ikisi de aygıt bağlantı durumunda bir değişiklik olduğu anlamına gelir.

## <a name="tips-for-consuming-events"></a>Etkinlikleri tüketmek için ipuçları

IoT Hub olaylarını işleyen uygulamalar aşağıdaki uygulamaları izlemelidir:

* Olayları aynı olay işleyicisine yönlendirmek için birden çok abonelik yapılandırılabilir, bu nedenle olayların belirli bir kaynaktan geldiğini düşünmeyin. Beklediğiniz IoT hub'ından geldiğinden emin olmak için ileti konusunu her zaman denetleyin.

* Aldığınız tüm olayların beklediğiniz türler olduğunu düşünmeyin. İletiyi işlemeden önce her zaman eventType'ı denetleyin.

* İletiler sıradışı veya gecikmeden sonra gelebilir. Nesneler le ilgili bilgilerinizin oluşturulan aygıt veya aygıt silinmiş olaylar için güncel olup olmadığını anlamak için etag alanını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub etkinlikleri öğreticisini deneyin](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* [Cihazla bağlantılı ve bağlantısız olayları nasıl sıralayacağınızı öğrenin](iot-hub-how-to-order-connection-state-events.md)

* [Olay Izgarası hakkında daha fazla bilgi edinin](../event-grid/overview.md)

* [Yönlendirme IoT Hub olayları ve iletileri arasındaki farkları karşılaştırın](iot-hub-event-grid-routing-comparison.md)

* [Azure Haritalar'ı kullanarak IoT mekansal analitiği uygulamak için IoT telemetri etkinliklerini nasıl kullanacağınızı öğrenin](../azure-maps/tutorial-iot-hub-maps.md#create-an-azure-function-and-add-an-event-grid-subscription)
