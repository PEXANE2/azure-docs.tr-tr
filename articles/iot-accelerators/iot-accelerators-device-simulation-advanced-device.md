---
title: Gelişmiş bir simüle aygıt modeli oluşturma - Azure| Microsoft Dokümanlar
description: Bu nasıl yapılmalı kılavuzunda, Aygıt Simülasyonu çözüm hızlandırıcısıyla kullanılmak üzere gelişmiş bir aygıt modeli oluşturmayı öğrenirsiniz.
author: troyhopwood
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 03/18/2019
ms.author: troyhop
ms.openlocfilehash: 4401d4b93a27e76554368ce72d256b38de61df4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61449057"
---
# <a name="create-an-advanced-device-model"></a>Gelişmiş bir cihaz modeli oluşturma

Bu nasıl yapıltanımlanır kılavuzu, özel bir aygıt modelini tanımlayan JSON ve JavaScript dosyalarını açıklar. Makale, bazı örnek aygıt modeli tanım dosyalarını içerir ve bunları Aygıt Simülasyonu örneğinize nasıl yükleyersiniz gösterir. Testiniz için daha gerçekçi aygıt davranışlarını simüle etmek için gelişmiş aygıt modelleri oluşturabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'ı kullanma kılavuzundaki adımları izlemek için Azure aboneliğinizde dağıtılmış bir Aygıt Simülasyonu örneği gerekir.

Cihaz Simülasyonu'nu henüz dağıtmadıysanız, [Azure'da IoT cihaz simülasyonunu dağıtma ve çalıştırma](quickstart-device-simulation-deploy.md) hızlı başlangıç kılavuzunu tamamlamalısınız.

### <a name="open-device-simulation"></a>Cihaz Benzetimini açma

Tarayıcınızda Cihaz Simülasyonu'nu çalıştırmak için, önce [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com)'na gidin.

Azure aboneliği kimlik bilgilerinizi kullanarak oturum açmanız istenebilir.

Ardından Dağıt'ta dağıttığınız Aygıt Simülasyonu için **Hazırla'yı** tıklatın ve Azure [quickstart'ta bir IoT aygıt simülasyonu çalıştırın.](quickstart-device-simulation-deploy.md)

## <a name="device-models"></a>Cihaz modelleri

Simüle edilen her aygıt simülasyon davranışını tanımlayan belirli bir aygıt modeline aittir. Bu davranış, telemetri nin ne sıklıkta gönderilecek, ne tür iletiler gönderilenve desteklenen yöntemleri içerir.

Bir JSON aygıt tanım dosyası ve JavaScript dosyaları kümesi ni kullanarak bir aygıt modeli tanımlarsınız. Bu JavaScript dosyaları rasgele telemetri ve yöntem mantığı gibi simülasyon davranışını tanımlar.

Tipik bir aygıt modeli vardır:

* Her cihaz modeli için bir JSON dosyası (örneğin, elevator.json).
* Her aygıt modeli için bir JavaScript davranış komut dosyası dosyası dosyası (örneğin, asansör-state.js)
* Her cihaz yöntemi için bir JavaScript yöntem komut dosyası dosyası (örneğin, asansör-go-down.js)

> [!NOTE]
> Tüm aygıt modelleri yöntemleri tanımlamaz. Bu nedenle, bir aygıt modelinde yöntem komut dosyaları olabilir veya olmayabilir. Ancak, tüm aygıt modellerinde bir davranış komut dosyası olmalıdır.

## <a name="device-definition-file"></a>Aygıt tanım dosyası

Her aygıt tanım dosyası, aşağıdaki bilgiler de dahil olmak üzere benzetimli aygıt modelinin ayrıntılarını içerir:

* Aygıt modeli adı: string.
* Protokol: AMQP | MQTT | HTTP' ye göre.
* İlk aygıt durumu.
* Aygıt durumunu niçin yenileyene.
* Aygıt durumunu yenilemek için hangi JavaScript dosyasını kullanılacak.
* Her biri belirli bir frekansa sahip, gönderilecek telemetri iletilerinin listesi.
* Telemetri mesajları şeması, alınan telemetri ayrıştırmak için arka uç uygulaması tarafından kullanılan.
* Desteklenen yöntemlerin listesi ve her yöntemi simüle etmek için kullanılacak JavaScript dosyası.

### <a name="file-schema"></a>Dosya şeması

Şema sürümü her zaman "1.0.0" ve bu dosyanın biçimine özgüdür:

```json
"SchemaVersion": "1.0.0"
```

### <a name="device-model-description"></a>Aygıt modeli açıklaması

Aşağıdaki özellikler aygıt modelini açıklar. Her türün benzersiz bir tanımlayıcısı, anlamsal bir sürümü, adı ve açıklaması vardır:

```json
"Id": "chiller-01",
"Version": "0.0.1",
"Name": "Chiller",
"Description": "Chiller with external temperature and humidity sensors."
```

### <a name="iot-protocol"></a>IoT Protokolü

IoT aygıtları farklı protokoller kullanarak bağlanabilir. Simülasyon ya **AMQP**, **MQTT**, ya da **HTTP**kullanmanızı sağlar:

```json
"Protocol": "AMQP"
```

### <a name="simulated-device-state"></a>Benzetimli aygıt durumu

Her benzetilen aygıtın tanımlanması gereken bir iç durumu vardır. Durum, telemetride bildirilebilen özellikleri de tanımlar. Örneğin, bir soğutucu gibi bir başlangıç durumu olabilir:

```json
"InitialState": {
    "temperature": 50,
    "humidity": 40
},
```

Birkaç sensöre sahip hareketli bir aygıtın daha fazla özelliği olabilir, örneğin:

```json
"InitialState": {
    "latitude": 47.445301,
    "longitude": -122.296307,
    "speed": 30.0,
    "speed_unit": "mph",
    "cargotemperature": 38.0,
    "cargotemperature_unit": "F"
}
```

Aygıt durumu simülasyon hizmeti tarafından bellekte tutulur ve JavaScript işlevine giriş olarak sağlanır. JavaScript işlevi aşağıdakileri belirleyebilir:

* Durumu yoksaymak ve bazı rasgele veriler oluşturmak için.
* Belirli bir senaryo için gerçekçi bir şekilde aygıt durumunu güncelleştirmek için.

Durumu oluşturan işlev de girdi olarak alır:

* Cihaz kimliği.
* Cihaz modeli.
* Geçerli saat. Bu değer, aygıta ve zamana göre farklı veri oluşturmayı mümkün kılar.

### <a name="generating-telemetry-messages"></a>Telemetri iletileri oluşturma

Simülasyon hizmeti her aygıt için birkaç telemetri türü gönderebilir. Genellikle, telemetri aygıt durumundan verileri içerir. Örneğin, simüle edilmiş bir oda her 10 saniyede bir sıcaklık ve nem hakkında bilgi gönderebilir. Aygıt durumundan gelen değerlerle otomatik olarak değiştirilen aşağıdaki parçacıktaki yer tutuculara dikkat edin:

```json
"Telemetry": [
    {
        "Interval": "00:00:10",
        "MessageTemplate":
            "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
        "MessageSchema": {
            "Name": "RoomComfort;v1",
            "Format": "JSON",
            "Fields": {
                "temperature": "double",
                "temperature_unit": "text",
                "humidity": "integer"
            }
        }
    }
],
```

Yer tutucular, **AD'ın** JavaScript **main** işlevi tarafından döndürülen aygıt durumu nesnesinden bir anahtar olduğu özel bir sözdizimi **${NAME}** kullanır. Dizeleri tırnak içinde, sayılar ise tırnak içinde tırnak içinde olmalıdır.

#### <a name="message-schema"></a>İleti şeması

Her ileti türüiyi tanımlanmış bir şema olmalıdır. İleti şeması, arka uç uygulamalarının gelen telemetriyi yorumlamak için bilgileri yeniden kullanabilmesi için IoT Hub'a da yayımlanır.

Şema, çeşitli sistemler ve hizmetler arasında kolay ayrıştırma, dönüşüm ve analiz sağlayan JSON biçimini destekler.

Şemada listelenen alanlar aşağıdaki türlerden olabilir:

* Nesne - JSON kullanılarak seri hale getir
* İkili - base64 kullanılarak serihale
* Metin
* Boole
* Tamsayı
* Çift
* DateTime

### <a name="supported-methods"></a>Desteklenen yöntemler

Benzetimli aygıtlar yöntem çağrılarına da tepki verebilir, bu durumda bazı mantık lar uygular ve bazı yanıtlar sağlarlar. Simülasyona benzer şekilde, yöntem mantığı bir JavaScript dosyasında depolanır ve aygıt durumuyla etkileşimkurabilir. Örnek:

```json
"CloudToDeviceMethods": {
    "Start": {
        "Type": "JavaScript",
        "Path": "release-pressure.js"
    }
}
```

## <a name="create-a-device-definition-file"></a>Aygıt tanım dosyası oluşturma

Bu nasıl-to-kılavuzu size nasıl bir drone için bir cihaz modeli oluşturmak için bakın. İnsansız hava aracı, konum ve irtifayı değiştiren bir koordinat kümesi etrafında rastgele uçacak.

Bir metin editörü içine aşağıdaki JSON kopyalayın ve **drone.json**olarak kaydedin.

### <a name="device-definition-json-example"></a>Cihaz tanımı JSON örneği

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "drone",
  "Version": "0.0.1",
  "Name": "Drone",
  "Description": "Simple drone.",
  "Protocol": "AMQP",
  "Simulation": {
    "InitialState": {
      "velocity": 0.0,
      "velocity_unit": "mm/sec",
      "acceleration": 0.0,
      "acceleration_unit": "mm/sec^2",
      "latitude": 47.476075,
      "longitude": -122.192026,
      "altitude": 0.0
    },
    "Interval": "00:00:05",
    "Scripts": [{
      "Type": "JavaScript",
      "Path": "drone-state.js"
    }]
  },
  "Properties": {
    "Type": "Drone",
    "Firmware": "1.0",
    "Model": "P-96"
  },
  "Tags": {
    "Owner": "Contoso"
  },
  "Telemetry": [{
      "Interval": "00:00:05",
      "MessageTemplate": "{\"velocity\":\"${velocity}\",\"acceleration\":\"${acceleration}\",\"position\":\"${latitude}|${longitude}|${altitude}\"}",
      "MessageSchema": {
        "Name": "drone-event-sensor;v1",
        "Format": "JSON",
        "Fields": {
          "velocity": "double",
          "velocity_unit": "text",
          "acceleration": "double",
          "acceleration_unit": "text",
          "latitude": "double",
          "longitude": "double",
          "altitude": "double"
        }
      }
    }
  ],
    "CloudToDeviceMethods": {
        "RecallDrone": {
            "Type": "JavaScript",
            "Path": "droneRecall-method.js"
        }
    }
}
```

## <a name="behavior-script-files"></a>Davranış komut dosyası dosyaları

Davranış dosyası dosyasındaki kod drone'u hareket ettirir. Komut dosyası, cihazın bellek durumunu manipüle ederek drone'un yüksekliğini ve konumunu değiştirir.

JavaScript dosyaları, iki parametre kabul eden bir **ana** işlev olmalıdır:

* Üç özellik içeren bir **bağlam** nesnesi:
    * **formatı** **yy-MM-dd'T'HH:mm:sszzz**ile bir dize olarak currentTime .
    * **deviceId**. Örneğin, **Simulated.Elevator.123**.
    * **deviceModel**. Örneğin, **Asansör**.
* Önceki çağrıda işlev tarafından döndürülen değer olan bir **durum** nesnesi. Bu aygıt durumu simülasyon hizmeti tarafından korunur ve telemetri iletileri oluşturmak için kullanılır.

**Ana** işlev yeni aygıt durumunu döndürür. Örnek:

```JavaScript
function main(context, state) {

    // Use context if the simulation depends on
    // time or device details.
    // Execute some logic, updating 'state'

    return state;
}
```

## <a name="create-a-behavior-script-file"></a>Davranış komut dosyası dosyası dosyası oluşturma

Aşağıdaki JavaScript'i bir metin düzenleyicisine kopyalayın ve **drone-state.js**olarak kaydedin.

### <a name="device-model-javascript-simulation-example"></a>Cihaz modeli JavaScript simülasyon örneği

```JavaScript
"use strict";

// Position control
const DefaultLatitude = 47.476075;
const DefaultLongitude = -122.192026;
const DistanceVariation = 10;

// Altitude control
const DefaultAltitude = 0.0;
const AverageAltitude = 499.99;
const AltitudeVariation = 5;

// Velocity control
const AverageVelocity = 60.00;
const MinVelocity = 20.00;
const MaxVelocity = 120.00;
const VelocityVariation = 5;

// Acceleration control
const AverageAcceleration = 2.50;
const MinAcceleration = 0.01;
const MaxAcceleration = 9.99;
const AccelerationVariation = 1;

// Display control for position and other attributes
const GeoSpatialPrecision = 6;
const DecimalPrecision = 2;

// Default state
var state = {
    velocity: 0.0,
    velocity_unit: "mm/sec",
    acceleration: 0.0,
    acceleration_unit: "mm/sec^2",
    latitude: DefaultLatitude,
    longitude: DefaultLongitude,
    altitude: DefaultAltitude
};

// Default device properties
var properties = {};

/**
 * Restore the global state using data from the previous iteration.
 *
 * @param previousState device state from the previous iteration
 * @param previousProperties device properties from the previous iteration
 */
function restoreSimulation(previousState, previousProperties) {
    // If the previous state is null, force a default state
    if (previousState) {
        state = previousState;
    } else {
        log("Using default state");
    }

    if (previousProperties) {
        properties = previousProperties;
    } else {
        log("Using default properties");
    }
}

/**
 * Simple formula generating a random value around the average
 * in between min and max
 */
function vary(avg, percentage, min, max) {
    var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
    value = Math.max(value, min);
    value = Math.min(value, max);
    return value;
}

/**
 * Entry point function called by the simulation engine.
 * Returns updated simulation state.
 * Device property updates must call updateProperties() to persist.
 *
 * @param context             The context contains current time, device model and id
 * @param previousState       The device state since the last iteration
 * @param previousProperties  The device properties since the last iteration
 */
/*jslint unparam: true*/
function main(context, previousState, previousProperties) {

    // Restore the global state before generating the new telemetry, so that
    // the telemetry can apply changes using the previous function state.
    restoreSimulation(previousState, previousProperties);

    state.acceleration = vary(AverageAcceleration, AccelerationVariation, MinAcceleration, MaxAcceleration).toFixed(DecimalPrecision);
    state.velocity = vary(AverageVelocity, VelocityVariation, MinVelocity, MaxVelocity).toFixed(DecimalPrecision);

    // Use the last coordinates to calculate the next set with a given variation
    var coords = varylocation(Number(state.latitude), Number(state.longitude), DistanceVariation);
    state.latitude = Number(coords.latitude).toFixed(GeoSpatialPrecision);
    state.longitude = Number(coords.longitude).toFixed(GeoSpatialPrecision);

    // Fluctuate altitude between given variation constant by more or less
    state.altitude = vary(AverageAltitude, AltitudeVariation, AverageAltitude - AltitudeVariation, AverageAltitude + AltitudeVariation).toFixed(DecimalPrecision);

    return state;
}

/**
 * Generate a random geolocation at some distance (in miles)
 * from a given location
 */
function varylocation(latitude, longitude, distance) {
    // Convert to meters, use Earth radius, convert to radians
    var radians = (distance * 1609.344 / 6378137) * (180 / Math.PI);
    return {
        latitude: latitude + radians,
        longitude: longitude + radians / Math.cos(latitude * Math.PI / 180)
    };
}
```

## <a name="create-a-method-script-file"></a>Yöntem komut dosyası dosyası dosyası oluşturma

Yöntem komut dosyaları davranış komut dosyalarıbenzer. Belirli bir buluttan aygıt yöntemine çağrıldığında aygıt davranışını tanımlarlar.

İhA geri çağırma komut dosyası, eve dönen drone'u simüle etmek için İhA'nın koordinatlarını sabit bir noktaya ayarlar.

Aşağıdaki JavaScript'i bir metin düzenleyicisine kopyalayın ve **droneRecall-method.js**olarak kaydedin.

### <a name="device-model-javascript-simulation-example"></a>Cihaz modeli JavaScript simülasyon örneği

```JavaScript
"use strict";

// Default state
var state = {
    velocity: 0.0,
    velocity_unit: "mm/sec",
    acceleration: 0.0,
    acceleration_unit: "mm/sec^2",
    latitude: 0.0,
    longitude: 0.0,
    altitude: 0.0
};

// Default device properties
var properties = {};

/**
 * Restore the global state using data from the previous iteration.
 *
 * @param previousState device state from the previous iteration
 * @param previousProperties device properties from the previous iteration
 */
function restoreSimulation(previousState, previousProperties) {
    // If the previous state is null, force a default state
    if (previousState) {
        state = previousState;
    } else {
        log("Using default state");
    }

    if (previousProperties) {
        properties = previousProperties;
    } else {
        log("Using default properties");
    }
}

/**
 * Entry point function called by the simulation engine.
 *
 * @param context        The context contains current time, device model and id, not used
 * @param previousState  The device state since the last iteration, not used
 * @param previousProperties  The device properties since the last iteration
 */
/*jslint unparam: true*/
function main(context, previousState, previousProperties) {

    // Restore the global device properties and the global state before
    // generating the new telemetry, so that the telemetry can apply changes
    // using the previous function state.
    restoreSimulation(previousState, previousProperties);

    //simulate the behavior of a drone when recalled
  state.latitude = 47.476075;
  state.longitude = -122.192026;
  return state;
}
```

## <a name="debugging-script-files"></a>Komut dosyası dosyalarını hata ayıklama

Çalışan davranış dosyasına hata ayıklama ekleyemeseniz de, **günlük** işlevini kullanarak hizmet günlüğüne bilgi yazmak mümkündür. Sözdizimi hataları için, yorumlayıcı başarısız olur ve günlüğün özel durumu hakkında bilgi yazar.

Günlük örneği:

```JavaScript
function main(context, state) {

    log("This message will appear in the service logs.");

    log(context.deviceId);

    if (typeof(state) !== "undefined" && state !== null) {
        log("Previous value: " + state.temperature);
    }

    // ...

    return updateState;
}
```

## <a name="deploy-an-advanced-device-model"></a>Gelişmiş bir aygıt modelini dağıtma

Gelişmiş aygıt modelinizi dağıtmak için Aygıt Simülasyonu örneğiniz olan dosyaları yüklersiniz:

Menü çubuğunda **Cihaz modelleri**'ni seçin. **Aygıt modelleri** sayfası, Aygıt Simülasyonu'nun bu örneğinde bulunan aygıt modellerini listeler:

![Cihaz modelleri](media/iot-accelerators-device-simulation-advanced-device/devicemodelnav.png)

Sayfanın sağ üst köşesindeki **+ Cihaz Modeli Ekle**'ye tıklayın:

![Cihaz modeli ekleme](media/iot-accelerators-device-simulation-advanced-device/devicemodels.png)

Gelişmiş aygıt modeli sekmesini açmak için **Gelişmiş'i** tıklatın:

![Gelişmiş Sekme](media/iot-accelerators-device-simulation-advanced-device/advancedtab.png)

**Gözat'ı** tıklatın ve oluşturduğunuz JSON ve JavaScript dosyalarını seçin. Üç dosyayı da seçtiğinizden emin olun. Herhangi bir dosya eksikse, doğrulama başarısız olur:

![Dosyalara Gözat](media/iot-accelerators-device-simulation-advanced-device/browse.png)

Dosyalarınız doğrulamayı geçiyorsa, **Kaydet'i** tıklatın ve aygıt modeliniz simülasyonda kullanılmaya hazır. Aksi takdirde, hataları düzeltin ve dosyaları yeniden yükleyin:

![Kaydet](media/iot-accelerators-device-simulation-advanced-device/validated.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılmalı kılavuzunda, Aygıt Simülasyonu'nda kullanılan aygıt modeli dosyalarını ve gelişmiş bir aygıt modelinin nasıl oluşturulabileceğinizi öğrendiniz. Daha sonra, [Aygıt Simülasyonu çözüm hızlandırıcısından gönderilen telemetriyi görselleştirmek için Zaman Serisi Öngörülerini](https://docs.microsoft.com/azure/iot-accelerators/iot-accelerators-device-simulation-time-series-insights)nasıl kullanacağınızı keşfetmek isteyebilirsiniz.
