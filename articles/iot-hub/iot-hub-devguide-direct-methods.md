---
title: Azure IoT Hub'ı doğrudan yöntemleri ni anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - bir hizmet uygulamasından cihazlarınızda kod çağırmak için doğrudan yöntemler kullanın.
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: rezas
ms.openlocfilehash: d4040a4d0cf3fadf7a6e07c0e03e105975d17040
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499257"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Doğrudan yöntemleri anlama ve IoT Hub'dan çağırma

IoT Hub, buluttan cihazlarda doğrudan yöntemleri çağırma olanağı sağlar. Doğrudan yöntemler, başarılı veya hemen başarısız olan (kullanıcı tarafından belirtilen bir zaman ayarı sonrasında) http çağrısına benzer bir aygıtla istek-yanıt etkileşimini temsil eder. Bu yaklaşım, aygıtın yanıt verip veremediğine bağlı olarak hemen eylem rotasının farklı olduğu senaryolar için yararlıdır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Her aygıt yöntemi tek bir aygıtı hedefler. [İşleri birden çok aygıtta zamanlamak,](iot-hub-devguide-jobs.md) birden çok aygıtta doğrudan yöntemleri çağırmanın bir yolunu gösterir ve bağlantısı kesilen aygıtlar için yöntem çağrısı zamanlayın.

IoT Hub'da **hizmet bağlama** izinleri olan herkes bir aygıtta bir yöntem çağırabilir.

Doğrudan yöntemler bir istek-yanıt deseni izler ve sonuçlarının hemen onaylanmasını gerektiren iletişimler içindir. Örneğin, aygıtın bir fanı açmak gibi etkileşimli denetimi.

İstenilen özellikleri, doğrudan yöntemleri veya buluttan aygıta iletileri kullanmak arasında şüpheniz varsa [Buluttan cihaza iletişim kılavuzuna](iot-hub-devguide-c2d-guidance.md) bakın.

## <a name="method-lifecycle"></a>Yöntem yaşam döngüsü

Cihazda doğrudan yöntemler uygulanır ve doğru bir şekilde anında kullanılabilir hale getirmek için yöntem yükünde sıfır veya daha fazla giriş gerekebilir. Hizmete bakan BIR URI ()`{iot hub}/twins/{device id}/methods/`aracılığıyla doğrudan bir yöntem çağırırsınız. Aygıt, cihaza özgü MQTT konusu (`$iothub/methods/POST/{method name}/`) veya AMQP `IoThub-methodname` bağlantıları `IoThub-status` (ve uygulama özellikleri) aracılığıyla doğrudan yöntemler alır. 

> [!NOTE]
> Bir aygıtta doğrudan bir yöntem çağırdığınızda, özellik adları ve değerleri, aşağıdaki kümedekiler dışında yalnızca US-ASCII yazdırılabilir alfasayısal içerebilir:``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``
> 

Doğrudan yöntemler eşzamanlıdır ve zaman anından sonra başarılı veya başarısız olur (varsayılan: 30 saniye, 5 ile 300 saniye arasında ayarlanabilir). Doğrudan yöntemler, aygıtın çevrimiçi yse ve yalnızca komut alıyorsa harekete geçmesini istediğiniz etkileşimli senaryolarda yararlıdır. Örneğin, bir telefondan gelen ışığı açmak. Bu senaryolarda, bulut hizmetinin sonuca mümkün olan en kısa sürede görebilmesini sağlamak için anında bir başarı veya başarısızlık görmek istersiniz. Aygıt yöntemin bir sonucu olarak bazı ileti gövdesi döndürebilir, ancak yöntemin bunu yapması gerekmez. Yöntem aramalarında sipariş verme veya eşzamanlılık semantikleri konusunda hiçbir garanti yoktur.

Doğrudan yöntemler yalnızca bulut tarafından HTTPS ve cihaz tarafından MQTT veya AMQP'dir.

Yöntem istekleri ve yanıtları için yük 128 KB'a kadar bir JSON belgesidir.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Arka uç uygulamasından doğrudan bir yöntem çağırma

Şimdi, bir arka uç uygulamasından doğrudan bir yöntem çağırın.

### <a name="method-invocation"></a>Yöntem çağırma

Bir aygıttaki doğrudan yöntem çağrıları, aşağıdaki öğelerden oluşan HTTPS çağrılarıdır:

* [ApI sürümü](/rest/api/iothub/service/devicemethod/invokedevicemethod)ile birlikte cihaza özgü *istek URI:*

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* POST *yöntemi*

* Yetkilendirme, istek kimliği, içerik türü ve içerik kodlaması içeren *üstbilgi.*

* Aşağıdaki biçimde saydam bir JSON *gövdesi:*

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

İstekte olduğu `responseTimeoutInSeconds` gibi sağlanan değer, IoT Hub hizmetinin bir aygıtta doğrudan bir yöntem yürütmesinin tamamlanması için beklemesi gereken süredir. Bu zaman anına, doğrudan bir yöntemin bir aygıt tarafından beklenen yürütme süresi kadar uzun olacak şekilde ayarlayın. Zaman aşımı sağlanmadıysa, varsayılan değeri 30 saniye kullanılır. Minimum ve maksimum `responseTimeoutInSeconds` değerler sırasıyla 5 ve 300 saniyedir.

İstekte olduğu `connectTimeoutInSeconds` gibi sağlanan değer, IoT Hub hizmetinin bağlantısı kesilen bir aygıtın çevrimiçi olması için beklemesi gereken doğrudan bir yöntemin çağrılması üzerine verilen süredir. Varsayılan değer 0'dır, bu da aygıtların doğrudan bir yöntem inden sonra zaten çevrimiçi olması gerektiği anlamına gelir. En büyük `connectTimeoutInSeconds` değer 300 saniyedir.


#### <a name="example"></a>Örnek

Kullanarak `curl`bir barebone örneği için aşağıya bakın. 

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

Arka uç uygulaması aşağıdaki öğelerden oluşan bir yanıt alır:

* *HTTP durum kodu*:
  * 200 doğrudan yöntemin başarılı bir şekilde yürütülmesini gösterir;
  * 404, aygıt kimliğinin geçersiz olduğunu veya doğrudan bir yöntemin çağrılması üzerine `connectTimeoutInSeconds` ve bundan sonra (temel nedenini anlamak için eşlik eden hata iletisini kullanın) çevrimiçi olmadığını gösterir;
  * 504, aygıtın içinde `responseTimeoutInSeconds`doğrudan bir yöntem çağrısına yanıt vermemeneden ağ geçidi zaman azamanı olduğunu gösterir.

* ETag içeren *üstbilgi,* istek kimliği, içerik türü ve içerik kodlama.

* Aşağıdaki biçimde bir JSON *gövdesi:*

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Her `status` `body` ikisi de cihaz tarafından sağlanır ve cihazın kendi durum kodu ve/veya açıklamasıyla yanıt vermek için kullanılır.

### <a name="method-invocation-for-iot-edge-modules"></a>IoT Edge modülleri için yöntem çağırma

[IoT Service Client C# SDK'da](https://www.nuget.org/packages/Microsoft.Azure.Devices/)modül kimliği kullanarak doğrudan yöntemlerin çağırılmasi desteklenir.

Bu amaçla, `ServiceClient.InvokeDeviceMethodAsync()` yöntemi kullanın ve `deviceId` parametre `moduleId` olarak geçmek.

## <a name="handle-a-direct-method-on-a-device"></a>Aygıtta doğrudan bir yöntem işleme

Bir IoT aygıtında doğrudan bir yöntemin nasıl işleyeceğime bakalım.

### <a name="mqtt"></a>MQTT

Aşağıdaki bölüm MQTT protokolü içindir.

#### <a name="method-invocation"></a>Yöntem çağırma

Cihazlar MQTT konusunda doğrudan yöntem `$iothub/methods/POST/{method name}/?$rid={request id}`istekleri alır: . Cihaz başına abonelik sayısı 5 ile sınırlıdır. Bu nedenle, her doğrudan yönteme ayrı ayrı abone olmamak önerilir. Bunun yerine abone olmayı `$iothub/methods/POST/#` düşünün ve ardından teslim edilen iletileri istediğiniz yöntem adlarına göre filtreleyin.

Aygıtın aldığı gövde aşağıdaki biçimdedir:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Yöntem istekleri QoS 0'dır.

#### <a name="response"></a>Yanıt

Cihaz yanıtları `$iothub/methods/res/{status}/?$rid={request id}`, nerede gönderir:

* Özellik, `status` yöntem yürütmenin aygıt tarafından sağlanan durumudur.

* Özellik, `$rid` IoT Hub'dan alınan yöntem inme sinden gelen istek kimliğidir.

Gövde cihaz tarafından ayarlanır ve herhangi bir durum olabilir.

### <a name="amqp"></a>AMQP

Aşağıdaki bölüm AMQP protokolü içindir.

#### <a name="method-invocation"></a>Yöntem çağırma

Cihaz, adreste `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`bir alma bağlantısı oluşturarak doğrudan yöntem isteklerini alır.

AMQP iletisi yöntem isteğini temsil eden alma bağlantısına gelir. Aşağıdaki bölümleri içerir:

* İlgili yöntem yanıtıile birlikte geri geçirilmesi gereken bir istek kimliği içeren korelasyon kimliği özelliği.

* Çağrılan yöntemin `IoThub-methodname`adını içeren , adlı bir uygulama özelliği.

* Yöntem yükünü JSON olarak içeren AMQP ileti gövdesi.

#### <a name="response"></a>Yanıt

Aygıt, yöntem yanıtını adrese `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`döndürmek için bir gönderme bağlantısı oluşturur.

Yöntemin yanıtı gönderme bağlantısında döndürülür ve aşağıdaki gibi yapılandırılır:

* Yöntemin istek iletisinde geçirilen istek kimliğini içeren korelasyon kimliği özelliği.

* Kullanıcı tarafından `IoThub-status`sağlanan yöntem durumunu içeren , adlı bir uygulama özelliği.

* Yöntem yanıtını JSON olarak içeren AMQP ileti gövdesi.

## <a name="additional-reference-material"></a>Ek referans materyali

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları,](iot-hub-devguide-endpoints.md) her IoT hub'ın çalışma zamanı ve yönetim işlemleri için ortaya çıkardığı çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar,](iot-hub-devguide-quotas-throttling.md) IoT Hub'ı kullandığınızda uygulanacak kotaları ve azaltma davranışını açıklar.

* [Azure IoT aygıt ve hizmet SDK'ları,](iot-hub-devguide-sdks.md) IoT Hub ile etkileşimde bulunan hem aygıt hem de hizmet uygulamaları geliştirdiğinizde kullanabileceğiniz çeşitli dil SDK'larını listeler.

* [Aygıt ikizleri, işleri ve ileti yönlendirmesi için IoT Hub sorgu dili,](iot-hub-devguide-query-language.md) IoT Hub'dan aygıtınız ve işlerinizle ilgili bilgileri almak için kullanabileceğiniz IoT Hub sorgu dilini açıklar.

* [IoT Hub MQTT desteği,](iot-hub-mqtt-support.md) MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi doğrudan yöntemleri kullanmayı öğrendim, aşağıdaki IoT Hub geliştirici kılavuzu makale ilginizi çekebilir:

* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan bazı kavramları denemek isterseniz, aşağıdaki IoT Hub öğreticisi ilginizi çekebilir:

* [Doğrudan yöntemler kullanma](quickstart-control-device-node.md)
* [VS Code için Azure IoT Araçları ile cihaz yönetimi](iot-hub-device-management-iot-toolkit.md)
