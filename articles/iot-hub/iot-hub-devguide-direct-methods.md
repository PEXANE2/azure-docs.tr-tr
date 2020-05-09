---
title: Azure IoT Hub doğrudan yöntemlerini anlama | Microsoft Docs
description: Geliştirici Kılavuzu-cihazlarınızdaki kodu bir hizmet uygulamasından çağırmak için doğrudan yöntemler kullanın.
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: rezas
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 9fb2242f6e3f8ce78a0e5043a53ce3055819725b
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583672"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Doğrudan yöntemleri anlama ve IoT Hub'dan çağırma

IoT Hub, buluttan cihazlarda doğrudan Yöntemler çağırma olanağı sağlar. Doğrudan Yöntemler, başarılı veya başarısız olmaları durumunda (Kullanıcı tarafından belirtilen zaman aşımından sonra) bir HTTP çağrısına benzer bir cihazla bir istek-yanıt etkileşimini temsil eder. Bu yaklaşım, cihazın yanıt verip vermediği bağımsız olarak, anlık eylem kursunun farklı olduğu senaryolar için yararlıdır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Her bir cihaz yöntemi tek bir cihazı hedefler. [Birden çok cihazda Işlerin zamanlaması](iot-hub-devguide-jobs.md) , birden çok cihazda doğrudan yöntemleri çağırma ve bağlantısı kesilen cihazlar için yöntem çağırma için bir yol sağlamayı gösterir.

IoT Hub **hizmet bağlantısı** izinleri olan herkes, bir cihazdaki bir yöntemi çağırabilir.

Doğrudan Yöntemler bir istek-yanıt modelini izler ve sonuçlarının hemen onayını gerektiren iletişimler için tasarlanmıştır. Örneğin, cihaz için bir fanı açma gibi etkileşimli denetim.

İstenen özellikler, doğrudan Yöntemler veya buluttan cihaza iletileri kullanma arasında şüpheliyse, [buluttan cihaza iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md) bakın.

## <a name="method-lifecycle"></a>Yöntem yaşam döngüsü

Doğrudan yöntemler cihaza uygulanır ve doğru şekilde örneklendirilecek Yöntem yükünde sıfır veya daha fazla giriş gerektirebilir. Bir hizmete yönelik URI (`{iot hub}/twins/{device id}/methods/`) aracılığıyla doğrudan bir yöntem çağırılır. Bir cihaz,`$iothub/methods/POST/{method name}/` `IoThub-methodname` cihaza özgü bir MQTT konusu () veya AMQP bağlantıları aracılığıyla doğrudan Yöntemler alır (ve `IoThub-status` uygulama özellikleri). 

> [!NOTE]
> Bir cihazda doğrudan yöntem çağırdığınızda, özellik adları ve değerleri, aşağıdaki küme dışında yalnızca US-ASCII yazdırılabilir alfasayısal içerebilir:``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``
> 

Doğrudan Yöntemler zaman uyumludur ve zaman aşımı süresinden sonra başarılı ya da başarısız olur (varsayılan: 30 saniye, 5 ile 300 saniye arasında ayarlanabilir). Doğrudan Yöntemler, cihazın ve yalnızca cihaz çevrimiçi olduğunda ve komutları alacağından, bir cihazın çalışır durumda olmasını istediğiniz Etkileşimli senaryolarda faydalıdır. Örneğin, bir telefonda bir ışığı açmak. Bu senaryolarda, bulut hizmetinin sonuca en kısa sürede işlem yapabilmesi için anında başarı veya başarısızlık olduğunu görmek istersiniz. Cihaz, metodun bir sonucu olarak bazı ileti gövdesini döndürebilir, ancak bunu yapmak için gerekli değildir. Yöntem çağrılarında sıralamada veya herhangi bir eşzamanlılık semantiğinin garantisi yoktur.

Doğrudan Yöntemler, yalnızca HTTPS 'den bulut tarafında, MQTT veya AMQP 'den cihaz tarafında bulunur.

Yöntem istekleri ve yanıtları için yük, 128 KB 'ye kadar bir JSON belgesidir.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Bir arka uç uygulamasından doğrudan yöntem çağırma

Şimdi, bir arka uç uygulamasından doğrudan bir yöntemi çağırın.

### <a name="method-invocation"></a>Yöntem çağırma

Bir cihazdaki doğrudan yöntem etkinleştirmeleri, aşağıdaki öğelerden oluşan HTTPS çağrılardır:

* [API sürümüyle](/rest/api/iothub/service/devicemethod/invokedevicemethod)birlikte cihaza özgü *istek URI 'si* :

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

İstekte olarak `responseTimeoutInSeconds` belirtilen değer, bir cihazda doğrudan yöntem yürütmeyi tamamlamak için IoT Hub hizmetin tamamlaması gereken süre miktarıdır. Bu zaman aşımını, bir cihaz tarafından doğrudan bir metodun beklenen yürütme süresi kadar en az olacak şekilde ayarlayın. Zaman aşımı sağlanmazsa, varsayılan değer olan 30 saniye kullanılır. İçin `responseTimeoutInSeconds` en düşük ve en yüksek değerler, sırasıyla 5 ve 300 saniyedir.

İstekte olarak `connectTimeoutInSeconds` belirtilen değer, IoT Hub hizmetin, bağlantısı kesilen bir cihazın çevrimiçi olması için beklemek zorunda olduğu doğrudan bir yöntemi çağırmada geçen süredir. Varsayılan değer 0 ' dır, yani doğrudan bir yöntemi çağırılmak için cihazların zaten çevrimiçi olması gerekir. İçin `connectTimeoutInSeconds` en büyük değer 300 saniyedir.


#### <a name="example"></a>Örnek

Bu örnek, bir Azure IoT Hub kayıtlı bir IoT cihazında doğrudan yöntem çağırma isteğini güvenli bir şekilde başlatabilmeniz için size izin verir.

Başlamak için, SharedAccessSignature oluşturmak üzere [Azure CLI için Microsoft Azure IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) kullanın. 

```bash
az iot hub generate-sas-token -n <iothubName> -du <duration>
```

Ardından, yetkilendirme üst bilgisini yeni oluşturulan sharedaccesssignature ile değiştirin, ardından `iothubName`, `deviceId` `methodName` ve `payload` parametrelerini aşağıdaki örnek `curl` komutta uygulamanız ile eşleşecek şekilde değiştirin.  

```bash
curl -X POST \
  https://<iothubName>.azure-devices.net/twins/<deviceId>/methods?api-version=2018-06-30 \
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

Belirtilen doğrudan yöntemi çağırmak için Modified komutunu yürütün. Başarılı istekler, bir HTTP 200 durum kodu döndürür.

> [!NOTE]
> Yukarıdaki örnekte, bir cihazda doğrudan bir yöntemi çağırma gösterilmektedir.  Bir IoT Edge modülünde doğrudan bir yöntemi çağırmak isterseniz, URL isteğini aşağıda gösterildiği gibi değiştirmeniz gerekir:

```bash
https://<iothubName>.azure-devices.net/twins/<deviceId>/modules/<moduleName>/methods?api-version=2018-06
```
### <a name="response"></a>Yanıt

Arka uç uygulaması, aşağıdaki öğelerden oluşan bir yanıt alır:

* *Http durum kodu*:
  * 200, doğrudan metodun başarıyla yürütülmesini gösterir;
  * 404, cihaz KIMLIĞININ geçersiz olduğunu ya da cihazın doğrudan bir yöntemi çağırdıktan sonra çevrimiçi olmadığını veya daha `connectTimeoutInSeconds` sonra (kök nedenini anlamak için birlikte hata iletisini kullanın) olduğunu belirtir;
  * 504, cihazın içindeki `responseTimeoutInSeconds`doğrudan yöntem çağrısına yanıt vermemesinin neden olduğu ağ geçidi zaman aşımını belirtir.

* ETag, istek KIMLIĞI, içerik türü ve içerik kodlamasını içeren *üst bilgiler* .

* Aşağıdaki biçimde bir JSON *gövdesi* :

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    `body` Hem hem de `status` cihaz tarafından sağlanır ve cihazın kendi durum kodu ve/veya açıklamasıyla yanıt vermek için kullanılır.

### <a name="method-invocation-for-iot-edge-modules"></a>IoT Edge modülleri için yöntem çağırma

Bir modül KIMLIĞI kullanarak doğrudan yöntemleri çağırmak, [IoT hizmeti istemci C# SDK 'sında](https://www.nuget.org/packages/Microsoft.Azure.Devices/)desteklenir.

Bu amaçla `ServiceClient.InvokeDeviceMethodAsync()` yöntemini kullanın ve `deviceId` ve `moduleId` parametrelerini olarak geçirin.

## <a name="handle-a-direct-method-on-a-device"></a>Bir cihazda doğrudan yöntem işleme

IoT cihazında doğrudan bir yöntemi nasıl işleyeceğinizi inceleyelim.

### <a name="mqtt"></a>MQTT

Aşağıdaki bölüm MQTT protokolüne yöneliktir.

#### <a name="method-invocation"></a>Yöntem çağırma

Cihazlar MQTT konusunda doğrudan yöntem istekleri alıyor: `$iothub/methods/POST/{method name}/?$rid={request id}`. Cihaz başına abonelik sayısı 5 ile sınırlıdır. Bu nedenle, her bir doğrudan yönteme ayrı ayrı abone olmaması önerilir. Bunun yerine, istenen `$iothub/methods/POST/#` Yöntem adlarınız temelinde teslim edilen iletilere abone olmayı ve bunları filtrelemeyi düşünün.

Cihazın aldığı gövde aşağıdaki biçimdedir:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Yöntem istekleri QoS 0 ' dır.

#### <a name="response"></a>Yanıt

Cihaz yanıtlarını gönderir `$iothub/methods/res/{status}/?$rid={request id}`, burada:

* `status` Özelliği, yöntem yürütmenin cihaz tarafından sağlanan durumudur.

* `$rid` Özelliği, IoT Hub 'den alınan yöntem çağrısından gelen istek kimliğidir.

Gövde, cihaz tarafından ayarlanır ve herhangi bir durum olabilir.

### <a name="amqp"></a>AMQP

Aşağıdaki bölüm AMQP protokolüne yöneliktir.

#### <a name="method-invocation"></a>Yöntem çağırma

Cihaz, adreste `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`bir alma bağlantısı oluşturarak doğrudan yöntem istekleri alır.

AMQP iletisi, yöntem isteğini temsil eden alma bağlantısına ulaşır. Aşağıdaki bölümleri içerir:

* Karşılık gelen Yöntem yanıtıyla geri geçirilmesi gereken bir istek KIMLIĞI içeren bağıntı KIMLIĞI özelliği.

* Çağrılan yöntemin adını içeren `IoThub-methodname`adlı bir uygulama özelliği.

* JSON olarak Yöntem yükünü içeren AMQP ileti gövdesi.

#### <a name="response"></a>Yanıt

Cihaz, adreste `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`Yöntem yanıtını döndürmek için bir gönderme bağlantısı oluşturur.

Yöntemin yanıtı gönderme bağlantısında döndürülür ve aşağıdaki şekilde yapılandırılır:

* Metodun istek iletisinde geçirilen istek KIMLIĞINI içeren bağıntı KIMLIĞI özelliği.

* Kullanıcı tarafından sağlanan yöntem `IoThub-status`durumunu içeren adlı bir uygulama özelliği.

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

* [Doğrudan yöntemler kullanma](quickstart-control-device-node.md)
* [VS Code için Azure IoT Araçları ile cihaz yönetimi](iot-hub-device-management-iot-toolkit.md)
