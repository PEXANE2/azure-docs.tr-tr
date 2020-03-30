---
title: Azure IoT Hub ileti biçimini anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - IoT Hub iletilerinin biçimini ve beklenen içeriğini açıklar.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: asrastog
ms.openlocfilehash: 28537ac2389fbb1ca43ca4014515564bddeba4ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69872478"
---
# <a name="create-and-read-iot-hub-messages"></a>IoT Hub iletilerini oluşturma ve okuma

IoT Hub, protokoller arasında sorunsuz birlikte çalışabilirliği desteklemek için aygıta bakan tüm protokoller için ortak bir ileti biçimi tanımlar. Bu ileti biçimi hem [aygıttan buluta yönlendirme](iot-hub-devguide-messages-d2c.md) hem de [buluttan aygıta](iot-hub-devguide-messages-c2d.md) iletiler için kullanılır. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub, akışlı ileti deseni kullanarak aygıttan buluta ileti uygular. IoT Hub'ın aygıttan buluta iletileri, hizmetten geçen ve birden çok okuyucu tarafından okunabilen yüksek sayıda olay olduğundan, [Hizmet Veri Gönderi](/azure/service-bus-messaging/) *iletilerinden* çok [Olay Hub'ları](/azure/event-hubs/) *etkinliklerine* benzer.

Bir IoT Hub iletisi aşağıdakilerden oluşur:

* Aşağıda listelenen önceden belirlenmiş *bir sistem özellikleri* kümesi.

* *Uygulama özellikleri*kümesi. İleti gövdesini deserialize etmeye gerek kalmadan, uygulamanın tanımlayıp erişebileceği dize özellikleri sözlüğü. IoT Hub bu özellikleri asla değiştirir.

* Opak ikili bir vücut.

Özellik adları ve değerleri yalnızca ASCII alfasayısal karakteriçerebilir, ayrıca ``{'!', '#', '$', '%, '&', ''', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` HTTPS protokolünü kullanarak aygıttan buluta iletiler gönderdiğinde veya buluttan cihaza iletiler gönderdiğinde.

IoT Hub ile aygıt-bulut mesajlaşma aşağıdaki özelliklere sahiptir:

* Aygıttan buluta iletiler dayanıklıdır ve bir IoT hub'ın varsayılan **iletileri/olayları** bitiş noktasında yedi güne kadar saklanır.

* Aygıttan buluta iletiler en fazla 256 KB olabilir ve gönderilen leri optimize etmek için toplu olarak gruplandırılabilir. Toplu iş en fazla 256 KB olabilir.

* IoT Hub rasgele bölümleme izin vermez. Aygıttan buluta iletiler, bunların kaynağı **olan deviceId'e**göre bölümlenir.

* [IoT Hub'a Denetim erişiminde](iot-hub-devguide-security.md)açıklandığı gibi, IoT Hub cihaz başına kimlik doğrulama ve erişim denetimi sağlar.

* İletileri uygulama özelliklerine giren bilgilerle damgalayabilirsiniz. Daha fazla bilgi için lütfen [ileti zenginleştirmelerine](iot-hub-message-enrichments-overview.md)bakın.

Farklı protokoller kullanılarak gönderilen iletilerin nasıl kodlanması ve çözülmesi hakkında daha fazla bilgi için [Azure IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.

## <a name="system-properties-of-d2c-iot-hub-messages"></a>**D2C** IoT Hub iletilerinin sistem özellikleri

| Özellik | Açıklama  |Kullanıcı Ayartablosu?|Için Anahtar Kelime </br>yönlendirme sorgusu|
| --- | --- | --- | --- |
| mesaj kimliği |İstek-yanıt desenleri için kullanılan ileti için kullanıcı ayarlı tanımlayıcı. Format: ASCII 7-bit alfasayısal karakter + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`bir büyük/küçük harf duyarlı dize (128 karakter uzunluğunda en fazla) .  | Evet | Messageıd |
| iothub-enqueuedtime |[Aygıttan Buluta](iot-hub-devguide-d2c-guidance.md) iletinin IoT Hub tarafından alındığı tarih ve saat. | Hayır | enqueuedTime |
| user-id |İletilerin kaynağını belirtmek için kullanılan bir kimlik. İletiler IoT Hub tarafından oluşturulduğunda, `{iot hub name}`'. | Evet | userId |
| iothub-bağlantı-cihaz-id |IoT Hub tarafından aygıttan buluta iletilerde ayarlanmış bir kimlik. İletiyi gönderen aygıtın **deviceId'ini** içerir. | Hayır | bağlantıDeviceId |
| iothub-bağlantı-modül-id |IoT Hub tarafından aygıttan buluta iletilerde ayarlanmış bir kimlik. İletiyi gönderen aygıtın **moduleId'ini** içerir. | Hayır | bağlantıModuleId |
| iothub-connection-auth-generation-id |IoT Hub tarafından aygıttan buluta iletilerde ayarlanmış bir kimlik. İletiyi gönderen aygıtın **deviceGenerationGenerationId bağlantısını** [(Aygıt kimlik özelliklerine](iot-hub-devguide-identity-registry.md#device-identity-properties)göre) içerir. | Hayır |bağlantıDeviceGenerationId |
| iothub-bağlantı-auth-yöntemi |IoT Hub tarafından aygıttan buluta iletilerde ayarlanan bir kimlik doğrulama yöntemi. Bu özellik, iletiyi gönderen aygıtın kimliğini doğrulamak için kullanılan kimlik doğrulama yöntemi hakkında bilgi içerir.| Hayır | bağlantıAuthMethod |

## <a name="system-properties-of-c2d-iot-hub-messages"></a>**C2D** IoT Hub iletilerinin sistem özellikleri

| Özellik | Açıklama  |Kullanıcı Ayartablosu?|
| --- | --- | --- |
| mesaj kimliği |İstek-yanıt desenleri için kullanılan ileti için kullanıcı ayarlı tanımlayıcı. Format: ASCII 7-bit alfasayısal karakter + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`bir büyük/küçük harf duyarlı dize (128 karakter uzunluğunda en fazla) .  |Evet|
| sıra-sayı |IoT Hub tarafından her buluttan cihaza iletiye atanan bir sayı (aygıt sırası başına benzersiz). |Hayır|
| - |[Buluttan Aygıta](iot-hub-devguide-c2d-guidance.md) iletilerde belirtilen bir hedef. |Hayır|
| mutlak vade sonu-zaman |İletinin son kullanma tarihi ve saati. |Hayır|   |
| correlation-id |İstek-yanıt desenlerinde genellikle isteğin MessageId'ini içeren bir yanıt iletisindeki dize özelliği. |Evet|
| user-id |İletilerin kaynağını belirtmek için kullanılan bir kimlik. İletiler IoT Hub tarafından oluşturulduğunda, `{iot hub name}`'. |Evet|
| iothub-ack |Bir geri bildirim mesajı üreteci. Bu özellik, iletinin aygıt tarafından tüketimi sonucunda Geri bildirim iletileri oluşturmak için IoT Hub'dan talep etmek için aygıttan aygıta bulutiletilerde kullanılır. Olası değerler: **yok** (varsayılan): geri bildirim iletisi oluşturulma, **pozitif**: ileti tamamlanmışsa geri bildirim iletisi alın, **negatif**: iletinin süresi dolmuşsa (veya maksimum teslimat sayısına ulaşıldıysa) bir geri bildirim iletisi alın (veya maksimum teslimat sayısına ulaşıldı) aygıt tarafından tamamlanmadan veya **tam**: hem olumlu hem de negatif. |Evet|

## <a name="message-size"></a>İleti boyutu

IoT Hub, ileti boyutunu yalnızca gerçek yükü göz önünde bulundurarak protokol agnostik bir şekilde ölçer. Baytboyutu aşağıdaki değerlerin toplamı olarak hesaplanır:

* Baytlarda vücut büyüklüğü.
* İleti sistemi özelliklerinin tüm değerlerinin baytboyutu.
* Tüm kullanıcı özellik adlarının ve değerlerinin baytboyutu.

Özellik adları ve değerleri ASCII karakterleri ile sınırlıdır, bu nedenle dizelerin uzunluğu bayt boyutuna eşittir.

## <a name="anti-spoofing-properties"></a>Sızdırma önleme özellikleri

Aygıttan buluta iletilerde aygıt sızdırmasını önlemek için, IoT Hub tüm iletileri aşağıdaki özelliklere sahip damgalar:

* **iothub-bağlantı-cihaz-id**
* **iothub-connection-auth-generation-id**
* **iothub-bağlantı-auth-yöntemi**

İlk iki [aygıt kimlik özelliklerine](iot-hub-devguide-identity-registry.md#device-identity-properties)göre, kaynaklanan cihazın **deviceId** ve **generationId** içerir.

**iothub-connection-auth-method** özelliği, aşağıdaki özelliklere sahip bir JSON seri nesnesi içerir:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub'daki ileti boyutu sınırları hakkında bilgi için [IoT Hub kotaları ve azaltma](iot-hub-devguide-quotas-throttling.md)bölümüne bakın.

* Çeşitli programlama dillerinde IoT Hub iletilerinin nasıl oluşturulup okunduğunu öğrenmek için [Quickstarts'a](quickstart-send-telemetry-node.md)bakın.
