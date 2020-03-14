---
title: Azure IoT Hub doğrudan yöntemlerini anlama | Microsoft Docs
description: Geliştirici Kılavuzu-cihazlarınızdaki kodu bir hizmet uygulamasından çağırmak için doğrudan yöntemler kullanın.
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: rezas
ms.openlocfilehash: 4732304384b8c221ae7c8d99da7f714613ad9050
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79271284"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>IoT Hub doğrudan yöntemleri anlayın ve çağırın

IoT Hub, buluttan cihazlarda doğrudan Yöntemler çağırma olanağı sağlar. Doğrudan Yöntemler, başarılı veya başarısız olmaları durumunda (Kullanıcı tarafından belirtilen zaman aşımından sonra) bir HTTP çağrısına benzer bir cihazla bir istek-yanıt etkileşimini temsil eder. Bu yaklaşım, cihazın yanıt verip vermediği bağımsız olarak, anlık eylem kursunun farklı olduğu senaryolar için yararlıdır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Her bir cihaz yöntemi tek bir cihazı hedefler. [Birden çok cihazda Işlerin zamanlaması](iot-hub-devguide-jobs.md) , birden çok cihazda doğrudan yöntemleri çağırma ve bağlantısı kesilen cihazlar için yöntem çağırma için bir yol sağlamayı gösterir.

IoT Hub **hizmet bağlantısı** izinleri olan herkes, bir cihazdaki bir yöntemi çağırabilir.

Doğrudan Yöntemler bir istek-yanıt modelini izler ve sonuçlarının hemen onayını gerektiren iletişimler için tasarlanmıştır. Örneğin, cihaz için bir fanı açma gibi etkileşimli denetim.

İstenen özellikler, doğrudan Yöntemler veya buluttan cihaza iletileri kullanma arasında şüpheliyse, [buluttan cihaza iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md) bakın.

## <a name="method-lifecycle"></a>Yöntem yaşam döngüsü

Doğrudan yöntemler cihaza uygulanır ve doğru şekilde örneklendirilecek Yöntem yükünde sıfır veya daha fazla giriş gerektirebilir. Bir hizmete yönelik URI (`{iot hub}/twins/{device id}/methods/`) aracılığıyla doğrudan bir yöntem çağırılır. Bir cihaz, cihaza özgü bir MQTT konusu (`$iothub/methods/POST/{method name}/`) veya AMQP bağlantıları aracılığıyla doğrudan Yöntemler alır (`IoThub-methodname` ve `IoThub-status` uygulama özellikleri). 

> [!NOTE]
> Bir cihazda doğrudan yöntem çağırdığınızda, özellik adları ve değerleri yalnızca ABD-ASCII yazdırılabilir alfasayısal, aşağıdaki küme dışında bir değer içerebilir: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``
> 

Doğrudan Yöntemler zaman uyumludur ve zaman aşımı süresinden sonra başarılı ya da başarısız olur (varsayılan: 30 saniye, 5 ile 300 saniye arasında ayarlanabilir). Doğrudan Yöntemler, cihazın ve yalnızca cihaz çevrimiçi olduğunda ve komutları alacağından, bir cihazın çalışır durumda olmasını istediğiniz Etkileşimli senaryolarda faydalıdır. Örneğin, bir telefonda bir ışığı açmak. Bu senaryolarda, bulut hizmetinin sonuca en kısa sürede işlem yapabilmesi için anında başarı veya başarısızlık olduğunu görmek istersiniz. Cihaz, metodun bir sonucu olarak bazı ileti gövdesini döndürebilir, ancak bunu yapmak için gerekli değildir. Yöntem çağrılarında sıralamada veya herhangi bir eşzamanlılık semantiğinin garantisi yoktur.

Doğrudan Yöntemler, yalnızca HTTPS 'den bulut tarafında, MQTT veya AMQP 'den cihaz tarafında bulunur.

Yöntem istekleri ve yanıtları için yük, 128 KB 'ye kadar bir JSON belgesidir.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Bir arka uç uygulamasından doğrudan yöntem çağırma

Şimdi, bir arka uç uygulamasından doğrudan bir yöntemi çağırın.

### <a name="method-invocation"></a>Yöntem çağırma

Bir cihazdaki doğrudan yöntem etkinleştirmeleri, aşağıdaki öğelerden oluşan HTTPS çağrılardır:

* [API sürümüyle](/rest/api/iothub/service/invokedevicemethod)birlikte cihaza özgü *istek URI 'si* :

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* POST *yöntemi*

* Yetkilendirme, istek KIMLIĞI, içerik türü ve içerik kodlamasını içeren *üst bilgiler* .

* Aşağıdaki biçimde saydam bir JSON *gövdesi* :

    ```json
    {
        "methodName": "reboot",
        "responseTimeoutInSeconds": 200,
        "payload": {
            "input1": "someInput",
            "input2": "anotherInput"
        }
    }
    ```

İstek içinde `responseTimeoutInSeconds` olarak belirtilen değer, bir cihazdaki doğrudan yöntem yürütmeyi tamamlamak için IoT Hub hizmetinin tamamlaması gereken süre miktarıdır. Bu zaman aşımını, bir cihaz tarafından doğrudan bir metodun beklenen yürütme süresi kadar en az olacak şekilde ayarlayın. Zaman aşımı sağlanmazsa, varsayılan değer olan 30 saniye kullanılır. `responseTimeoutInSeconds` için en düşük ve en yüksek değerler, sırasıyla 5 ve 300 saniyedir.

İstekte `connectTimeoutInSeconds` olarak belirtilen değer, IoT Hub hizmetin, bağlantısı kesilen bir cihazın çevrimiçi olması için beklemek zorunda olduğu doğrudan bir yöntemi çağırmada geçen süredir. Varsayılan değer 0 ' dır, yani doğrudan bir yöntemi çağırılmak için cihazların zaten çevrimiçi olması gerekir. `connectTimeoutInSeconds` için en büyük değer 300 saniyedir.


#### <a name="example"></a>Örnek

`curl`kullanarak bir barekemiörneği için aşağıya bakın. 

```bash
curl -X POST \
  https://iothubname.azure-devices.net/twins/myfirstdevice/methods?api-version=2018-06-30 \
  -H 'Authorization: SharedAccessSignature sr=iothubname.azure-devices.net&sig=x&se=x&skn=iothubowner' \
  -H 'Content-Type: application/json' \
  -d '{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}'
```

### <a name="response"></a>Yanıt

Arka uç uygulaması, aşağıdaki öğelerden oluşan bir yanıt alır:

* *Http durum kodu*:
  * 200, doğrudan metodun başarıyla yürütülmesini gösterir;
  * 404, cihaz KIMLIĞININ geçersiz olduğunu ya da cihazın doğrudan bir yöntemi çağırıldıktan sonra çevrimiçi olmadığını veya `connectTimeoutInSeconds` bundan sonra (kök nedenini anlamak için birlikte hata iletisini kullanın) olduğunu belirtir;
  * 504, cihazın `responseTimeoutInSeconds`içindeki bir doğrudan yöntem çağrısına yanıt vermediğini neden olan ağ geçidi zaman aşımını belirtir.

* ETag, istek KIMLIĞI, içerik türü ve içerik kodlamasını içeren *üst bilgiler* .

* Aşağıdaki biçimde bir JSON *gövdesi* :

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Hem `status` hem de `body` cihaz tarafından sağlanır ve cihazın kendi durum kodu ve/veya açıklamasıyla yanıt vermek için kullanılır.

### <a name="method-invocation-for-iot-edge-modules"></a>IoT Edge modülleri için yöntem çağırma

Bir modül KIMLIĞI kullanarak doğrudan yöntemleri çağırmak, [IoT hizmeti istemci C# SDK 'sında](https://www.nuget.org/packages/Microsoft.Azure.Devices/)desteklenir.

Bu amaçla, `ServiceClient.InvokeDeviceMethodAsync()` yöntemini kullanın ve `deviceId` ve `moduleId` parametre olarak geçirin.

## <a name="handle-a-direct-method-on-a-device"></a>Bir cihazda doğrudan yöntem işleme

IoT cihazında doğrudan bir yöntemi nasıl işleyeceğinizi inceleyelim.

### <a name="mqtt"></a>MQTT

Aşağıdaki bölüm MQTT protokolüne yöneliktir.

#### <a name="method-invocation"></a>Yöntem çağırma

Cihazlar MQTT konusunda doğrudan yöntem istekleri alıyor: `$iothub/methods/POST/{method name}/?$rid={request id}`. Cihaz başına abonelik sayısı 5 ile sınırlıdır. Bu nedenle, her bir doğrudan yönteme ayrı ayrı abone olmaması önerilir. Bunun yerine `$iothub/methods/POST/#` abone olmayı düşünün ve ardından teslim edilen iletileri istediğiniz yöntem adlarına göre filtreleyerek filtreleyin.

Cihazın aldığı gövde aşağıdaki biçimdedir:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Yöntem istekleri QoS 0 ' dır.

#### <a name="response"></a>Yanıt

Cihaz yanıtları `$iothub/methods/res/{status}/?$rid={request id}`gönderir; burada:

* `status` özelliği, yöntem yürütmenin cihaz tarafından sağlanan durumudur.

* `$rid` özelliği, IoT Hub alınan yöntem çağrısından gelen istek KIMLIĞIDIR.

Gövde, cihaz tarafından ayarlanır ve herhangi bir durum olabilir.

### <a name="amqp"></a>AMQP

Aşağıdaki bölüm AMQP protokolüne yöneliktir.

#### <a name="method-invocation"></a>Yöntem çağırma

Cihaz, `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`adreste bir alma bağlantısı oluşturarak doğrudan yöntem istekleri alır.

AMQP iletisi, yöntem isteğini temsil eden alma bağlantısına ulaşır. Aşağıdaki bölümleri içerir:

* Karşılık gelen Yöntem yanıtıyla geri geçirilmesi gereken bir istek KIMLIĞI içeren bağıntı KIMLIĞI özelliği.

* Çağrılan yöntemin adını içeren `IoThub-methodname`adlı bir uygulama özelliği.

* JSON olarak Yöntem yükünü içeren AMQP ileti gövdesi.

#### <a name="response"></a>Yanıt

Cihaz, `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`adresine yönelik Yöntem yanıtını döndürmek için bir gönderme bağlantısı oluşturur.

Yöntemin yanıtı gönderme bağlantısında döndürülür ve aşağıdaki şekilde yapılandırılır:

* Metodun istek iletisinde geçirilen istek KIMLIĞINI içeren bağıntı KIMLIĞI özelliği.

* Kullanıcı tarafından sağlanan yöntem durumunu içeren `IoThub-status`adlı bir uygulama özelliği.

* JSON olarak Yöntem yanıtını içeren AMQP ileti gövdesi.

## <a name="additional-reference-material"></a>Ek başvuru malzemeleri

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunları içerir:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md) , her bir IoT Hub 'ının çalışma zamanı ve yönetim işlemleri için sunduğu çeşitli uç noktaları açıklar.

* [Daraltma ve Kotalar](iot-hub-devguide-quotas-throttling.md) , IoT Hub kullandığınızda beklenen kotaları ve azaltma davranışını tanımlar.

* [Azure IoT cihaz ve hizmet SDK 'ları](iot-hub-devguide-sdks.md) , IoT Hub etkileşimde bulunan cihaz ve hizmet uygulamaları geliştirirken kullanabileceğiniz çeşitli dil SDK 'larını listeler.

* [Cihaz TWINS, işler ve ileti yönlendirme için IoT Hub sorgu dili,](iot-hub-devguide-query-language.md) cihaz ikişlerinizle ve işleriniz hakkında IoT Hub bilgi almak için kullanabileceğiniz IoT Hub sorgu dilini açıklar.

* [MQTT desteği IoT Hub](iot-hub-mqtt-support.md) MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Artık doğrudan yöntemlerin nasıl kullanılacağını öğrendiniz, aşağıdaki IoT Hub Geliştirici Kılavuzu makalesi ile ilgileniyor olabilirsiniz:

* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan kavramların bazılarını denemek istiyorsanız aşağıdaki IoT Hub öğreticisiyle karşılaşabilirsiniz:

* [Doğrudan Yöntemler kullanma](quickstart-control-device-node.md)
* [VS Code için Azure IoT araçları ile cihaz yönetimi](iot-hub-device-management-iot-toolkit.md)
