---
title: Azure IoT Hub'ı bulut-aygıt iletisini anlama | Microsoft Dokümanlar
description: Bu geliştirici kılavuzu, IoT hub'ınızla bulut-aygıt mesajlaşmasını nasıl kullanacağınızı tartışır. İleti yaşam döngüsü ve yapılandırma seçenekleri hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 3a7254cc9de89a297811792b4dd64b4b669ba8e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271245"
---
# <a name="send-cloud-to-device-messages-from-an-iot-hub"></a>Bir IoT hub'ından buluttan aygıta iletiler gönderme

Çözümünüzden bir cihaz uygulamasına tek yönlü bildirimler göndermek için, IoT hub'ınızdan cihazınıza buluttan cihaza iletiler gönderin. Azure IoT Hub tarafından desteklenen diğer buluttan aygıta seçenekleri tartışmak için [Buluttan Cihaza iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md)bakın.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Hizmete bakan bir uç nokta, */iletiler/aygıt bağlı*aracılığıyla buluttan aygıta iletiler gönderirsiniz. Aygıt daha sonra iletileri aygıta özgü bir bitiş noktası, */devices/{deviceId}/messages/devicebound*üzerinden alır.

Her buluttan cihaza iletiyi tek bir aygıtta hedeflemek **için,** IoT hub'ınız özelliği */devices/{deviceId}/messages/devicebound*olarak ayarlar.

Her aygıt kuyruğu en fazla 50 buluttan cihaza ileti tutar. Aynı cihaza daha fazla ileti göndermeyi denemek hatayla sonuçlanır.

## <a name="the-cloud-to-device-message-life-cycle"></a>Buluttan aygıta ileti yaşam döngüsü

En az bir kez ileti teslimini garanti etmek için, IoT hub'ınız aygıt başına kuyruklarda buluttan cihaza iletileri devam ettiriyor. IoT hub'ın iletileri kuyruktan kaldırabilmesi için aygıtların *tamamlanmayı*açıkça kabul etmesi gerekir. Bu yaklaşım, bağlantı ve aygıt arızalarına karşı dayanıklılığı garanti eder.

Yaşam döngüsü durumu grafiği aşağıdaki diyagramda görüntülenir:

![Buluttan cihaza ileti yaşam döngüsü](./media/iot-hub-devguide-messages-c2d/lifecycle.png)

IoT hub hizmeti bir aygıta ileti gönderdiğinde, hizmet ileti durumunu *Enqueued'e*ayarlar. Bir aygıt ileti *almak* istediğinde, IoT hub'ı durumu *Görünmez*olarak ayarlayarak iletiyi *kilitler.* Bu durum, aygıttaki diğer iş parçacıklarının diğer iletileri almaya başlamasına olanak tanır. Aygıt iş parçacığı iletinin işlenmesini tamamladığında, iletiyi *tamamlayarak* IoT hub'ını doldurur. IoT hub'ı durumu *Tamamlanan'a*ayarlar.

Bir aygıt da şunları yapabilir:

* IoT hub'ınölü *harfli* duruma ayarlamasına neden olan iletiyi *reddedin.* İleti Sıraya Telemetri Taşıma (MQTT) Protokolü üzerinden bağlanan aygıtlar, buluttan cihaza iletileri reddedemez.

* IoT hub'ındaki iletiyi sıraya geri koymasına neden olan iletiyi *terk* etme ve durum *Enqueued*olarak ayarlanmış. MQTT Protokolü üzerinden bağlanan aygıtlar, buluttan cihaza iletileri terk edemez.

İş parçacığı, IoT hub'ına bildirmeden bir iletiyi işleyebilir. Bu durumda, iletiler görünmez *durumdan* otomatik olarak *görünürlük* zaman anına (veya *kilit* süresinden) sonra *Enqueued* durumuna geri geçiş yapar. Bu zaman-out değeri bir dakikadır ve değiştirilemez.

IoT hub'ındaki **maksimum teslim sayısı** özelliği, bir iletinin *Enqueued* ve *Görünmez* durumlar arasında geçiş yapma sayısının en fazla sayısını belirler. Bu geçiş sayısından sonra, IoT *hub'ı*iletin durumunu Dead lettered'a ayarlar. Benzer şekilde, IoT hub'ı, son kullanma tarihinden sonra *Dead'e yazılmış* bir iletinin durumunu ayarlar. Daha fazla bilgi [için, yaşama zamanı 'na](#message-expiration-time-to-live)bakın.

[IoT Hub makalesi ile buluttan cihaza iletigönderme,](iot-hub-csharp-csharp-c2d.md) buluttan buluttan cihaza iletileri nasıl göndereceğinizi ve bunları bir aygıttan nasıl alacağınızı gösterir.

İletinin kaybı uygulama mantığını etkilemediğinde aygıt normalde buluttan aygıta iletiyi tamamlar. Bunun bir örneği, aygıtın ileti içeriğini yerel olarak kalıcı hale getirmiş olması veya bir işlemi başarıyla yürüttüğü nde olabilir. İleti, kaybı uygulamanın işlevselliğini etkilemeyecek geçici bilgileri de taşıyabilir. Bazen, uzun süren görevler için şunları yapabilirsiniz:

* Aygıt görev açıklamasını yerel depolama alanında kalıcı hale getirttinden sonra buluttan aygıta iletiyi tamamlayın.

* Çözümün arka ucunu, görevin ilerlemesinin çeşitli aşamalarında bir veya daha fazla aygıttan buluta iletiyle bildirin.

## <a name="message-expiration-time-to-live"></a>İletinin sona ermesi (yaşama süresi)

Her buluttan cihaza iletinin bir son kullanma süresi vardır. Bu süre aşağıdakilerden biri tarafından ayarlanır:

* Hizmetteki **ExpiryTimeUtc** özelliği
* IoT hub'ı, ioT hub özelliği olarak belirtilen yaşamak için varsayılan *süreyi* kullanarak

[Bulut-aygıt yapılandırma seçeneklerine](#cloud-to-device-configuration-options)bakın.

İletinin son kullanma süresinden yararlanmanın ve bağlantısı kesilen aygıtlara ileti göndermeyi önlemenin yaygın bir *yolu, canlı değerleriçin* kısa bir süre ayarlamaktır. Bu yaklaşım, aygıt bağlantı durumunu korumakla aynı sonucu elde eder, ancak daha verimlidir. İleti bildirimleri istediğinizde, IoT hub'ı hangi aygıtların olduğunu size verir:

* İletileri alabilme.
* Çevrimiçi değil veya başarısız oldu.

## <a name="message-feedback"></a>İleti geri bildirimi

Buluttan aygıta ileti gönderdiğinizde, hizmet iletinin son durumu hakkında ileti başına geri bildirim gönderilmesini isteyebilir. Bunu, aşağıdaki dört değerden birine gönderilen buluttan aygıta iletide **iothub-ack** uygulama özelliğini ayarlayarak yaparsınız:

| Ack özellik değeri | Davranış |
| ------------ | -------- |
| yok     | IoT hub'ı geri bildirim iletisi (varsayılan davranış) oluşturmaz. |
| pozitif | Buluttan aygıta ileti *Tamamlanan* duruma ulaşırsa, IoT hub'ı bir geri bildirim iletisi oluşturur. |
| negatif | Buluttan aygıta ileti Ölü *harfli* duruma ulaşırsa, IoT hub'ı bir geri bildirim iletisi oluşturur. |
| Tam     | IoT hub'ı her iki durumda da bir geri bildirim iletisi oluşturur. |

**Ack** değeri *doluysa*ve geri bildirim iletisi almıyorsanız, bu geri bildirim iletisinin süresinin dolduğu anlamına gelir. Hizmet, orijinal iletiye ne olduğunu bilemiyor. Uygulamada, bir hizmet, geri bildirimi süresi dolmadan önce işleyebilir emin olmalıdır. En yüksek son kullanma süresi iki gündür ve bu da bir hata oluşursa hizmetin yeniden çalıştırMası için zaman bırakır.

[Uç Noktalar'da](iot-hub-devguide-endpoints.md)açıklandığı gibi, IoT hub'ı hizmete bakan bir uç nokta, */iletiler/servis/geri bildirim*yoluyla ileti olarak geri bildirim sağlar. Geri bildirim almanın anlambilimi, buluttan aygıta iletilerle aynıdır. Mümkün olduğunda, ileti geri bildirimi aşağıdaki biçimde tek bir iletide toplu olarak toplu olarak işlenir:

| Özellik     | Açıklama |
| ------------ | ----------- |
| EnqueuedTime | Geri bildirim iletisinin hub tarafından ne zaman alındığını gösteren bir zaman damgası |
| UserId       | `{iot hub name}` |
| Contenttype  | `application/vnd.microsoft.iothub.feedback.json` |

Gövde, her biri aşağıdaki özelliklere sahip JSON serileştirilmiş bir kayıt dizisidir:

| Özellik           | Açıklama |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | İletinin sonucunun ne zaman gerçekleştiğini belirten bir zaman damgası (örneğin, hub geri bildirim iletisini aldı veya özgün iletinin süresi doldu) |
| OrijinalMessageId  | Bu geri bildirim bilgilerinin ilişkili olduğu buluttan aygıta iletinin *MessageId'i* |
| Statuscode         | IoT hub'ı tarafından oluşturulan geri bildirim iletilerinde kullanılan gerekli dize: <br/> *Başarı* <br/> *Süresi dolmuş* <br/> *DeliveryCountExceeded* <br/> *Reddedilen* <br/> *Tasfiye* |
| Açıklama        | StatusCode için *dize* değerleri |
| DeviceId           | Bu geri bildirim parçasının ilişkili olduğu buluttan aygıta iletinin hedef aygıtının *DeviceId'i* |
| DeviceGenerationId | Bu geri bildirim parçasının ilişkili olduğu buluttan aygıta iletinin hedef aygıtının *DeviceGenerationId'i* |

Buluttan aygıta iletinin geri bildirimini özgün iletiyle ilişkilendirmesi için hizmetin bir *MessageId*belirtmesi gerekir.

Geri bildirim iletisinin gövdesi aşağıdaki kodda gösterilir:

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

**Silinen aygıtlar için bekleyen geri bildirim**

Bir aygıt silindiğinde, bekleyen geri bildirimler de silinir. Cihaz geri bildirimi toplu olarak gönderilir. Aygıtın iletinin alındığını onaylaması ile bir sonraki geri bildirim toplu işlemi nin hazırlandığı zaman arasında dar pencerede (genellikle 1 saniyeden kısa) bir aygıt silinirse, geri bildirim oluşmaz.

Cihazınızı silmeden önce bekleyen geri bildirimin gelmesini bekleyen bir süre bekleyerek bu davranışı ele alabilirsiniz. İlgili ileti geri bildirimi, aygıt silindikten sonra kaybolduğu varsayılmalıdır.

## <a name="cloud-to-device-configuration-options"></a>Bulut-aygıt yapılandırma seçenekleri

Her IoT hub'ı, bulut-aygıt mesajlaşması için aşağıdaki yapılandırma seçeneklerini ortaya çıkarır:

| Özellik                  | Açıklama | Aralık ve varsayılan |
| ------------------------- | ----------- | ----------------- |
| varsayılanTtlAsIso8601       | Buluttan aygıta iletiler için varsayılan TTL | ISO_8601 2 güne kadar (en az 1 dakika) aralığı; varsayılan: 1 saat |
| maxDeliveryCount          | Aygıt başına bulut-aygıt kuyrukları için maksimum teslim sayısı | 1 ile 100 arasında; varsayılan: 10 |
| geri bildirim.ttlAsIso8601     | Hizmete bağlı geri bildirim iletileri için bekletme | ISO_8601 2 güne kadar (en az 1 dakika) aralığı; varsayılan: 1 saat |
| feedback.maxDeliveryCount | Geri bildirim sırası için maksimum teslimat sayısı | 1 ile 100 arasında; varsayılan: 10 |
| feedback.lockDurationAsIso8601 | Geri bildirim sırası için maksimum teslimat sayısı | ISO_8601 5 ile 300 saniye arasında (en az 5 saniye) aralığı; varsayılan: 60 saniye. |

Yapılandırma seçeneklerini aşağıdaki yollardan biriyle ayarlayabilirsiniz:

* **Azure portalı**: IoT hub'ınızdaki **Ayarlar** altında Yerleşik **uç noktaları** seçin ve Bulut'u **aygıt iletisine**genişletin. **(feedback.maxDeliveryCount** ve **feedback.lockDurationAsIso8601** özelliklerinin ayarlanması şu anda Azure portalında desteklenmemektedir.)

    ![Portalda bulut-aygıt mesajlaşması için yapılandırma seçeneklerini ayarlama](./media/iot-hub-devguide-messages-c2d/c2d-configuration-portal.png)

* **Azure CLI**: [az iot hub güncelleştirme](https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az-iot-hub-update) komutunu kullanın:

    ```azurecli
    az iot hub update --name {your IoT hub name} \
        --set properties.cloudToDevice.defaultTtlAsIso8601=PT1H0M0S

    az iot hub update --name {your IoT hub name} \
        --set properties.cloudToDevice.maxDeliveryCount=10

    az iot hub update --name {your IoT hub name} \
        --set properties.cloudToDevice.feedback.ttlAsIso8601=PT1H0M0S

    az iot hub update --name {your IoT hub name} \
        --set properties.cloudToDevice.feedback.maxDeliveryCount=10

    az iot hub update --name {your IoT hub name} \
        --set properties.cloudToDevice.feedback.lockDurationAsIso8601=PT0H1M0S
    ```

## <a name="next-steps"></a>Sonraki adımlar

Buluttan cihaza ileti almak için kullanabileceğiniz SDK'lar hakkında bilgi için [Azure IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.

Buluttan cihaza iletialmayı denemek için [buluttan cihaza gönder](iot-hub-csharp-csharp-c2d.md) öğreticisine bakın.
