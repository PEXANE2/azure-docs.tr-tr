---
title: Geliştirici Kılavuzu-IoT Tak ve Kullan önizlemesi | Microsoft Docs
description: Geliştiriciler için IoT Tak ve Kullan açıklaması
author: rido-min
ms.author: rmpablos
ms.date: 07/16/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: ef221ea068f2786a4a84f20a29e80dd7176f06c6
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337424"
---
# <a name="iot-plug-and-play-preview-developer-guide"></a>IoT Tak ve Kullan önizleme Geliştirici Kılavuzu

IoT Tak ve Kullan önizlemesi, yeteneklerini Azure IoT uygulamalarına duyuran akıllı cihazlar oluşturmanızı sağlar. IoT Tak ve Kullan cihazları, bir müşteri bunları IoT Tak ve Kullan özellikli uygulamalara bağladığında el ile yapılandırma gerektirmez.

Bu kılavuzda, [ıot Tak ve kullan kurallarını](concepts-convention.md)izleyen bir cihaz oluşturmak için gereken temel adımlar ve cihazla etkileşim kurmak için KULLANABILECEĞINIZ kullanılabilir REST API 'leri açıklanmaktadır.

IoT Tak ve Kullan cihazı oluşturmak için aşağıdaki adımları izleyin:

1. Cihazınızın Azure IoT Hub bağlanmak için MQTT veya MQTT Over WebSockets protokolünü kullandığından emin olun.
1. Cihazınızı anlatmak için [dijital bir TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) modeli oluşturun. Daha fazla bilgi edinmek için bkz. [ıot Tak ve kullan modellerdeki bileşenleri anlama](concepts-components.md).
1. Cihaz bağlantısının bir parçası olarak duyurmak için cihazınızı güncelleştirin `model-id` .
1. [Iot Tak ve kullan kurallarını](concepts-convention.md) kullanarak telemetri, özellik ve komutları uygulama

Cihaz uygulamanız hazırsa, cihazın IoT Tak ve Kullan kurallarını izlediğini doğrulamak için [Azure IoT Gezginini](howto-use-iot-explorer.md) kullanın.

> [!Tip]
> Bu makaledeki tüm kod parçaları C# kullanır, ancak kavramlar C, Python, Node ve Java için kullanılabilir SDK 'Lardan herhangi biri için geçerlidir.

## <a name="model-id-announcement"></a>Model KIMLIĞI duyurusu

Model KIMLIĞINI duyurmak için cihazın bağlantı bilgilerine eklemesi gerekir:

```csharp
DeviceClient.CreateFromConnectionString(
  connectionString,
  TransportType.Mqtt,
  new ClientOptions() { ModelId = modelId })
```

Yeni `ClientOptions` aşırı yükleme, `DeviceClient` bir bağlantıyı başlatmak için kullanılan tüm yöntemlerde kullanılabilir.

Model KIMLIĞI duyurusu SDK 'ların sonraki sürümlerine eklenmiştir

|SDK|Sürüm|
|---|-------|
|C-SDK|1.3.9|
|.NET|1.27.0|
|Java|1.14.0|
|Düğüm|1.17.0|
|Python|2.1.4|

## <a name="implement-telemetry-properties-and-commands"></a>Telemetri, özellik ve komutları uygulama

[Iot Tak ve kullan modellerdeki bileşenleri anlama](concepts-components.md)bölümünde açıklandığı gibi, cihaz oluşturucuların cihazlarını açıklamak için bileşenleri kullanmak istemeseler de karar vermelidir. Bileşenler kullanılırken, cihazların bu bölümde açıklanan kurallara uymalıdır.

### <a name="telemetry"></a>Telemetri

Bileşenleri olmayan modeller özel bir özellik gerektirmez.

Bileşenler kullanılırken, cihazların bileşen adı ile bir ileti özelliği ayarlaması gerekir:

```c#
public async Task SendComponentTelemetryValueAsync(string componentName, string serializedTelemetry)
{
  var message = new Message(Encoding.UTF8.GetBytes(serializedTelemetry));
  message.Properties.Add("$.sub", componentName);
  message.ContentType = "application/json";
  message.ContentEncoding = "utf-8";
  await deviceClient.SendEventAsync(message);
}
```

### <a name="read-only-properties"></a>Salt okunurdur özellikleri

Bileşenleri olmayan modeller özel bir yapı gerektirmez:

```csharp
TwinCollection reportedProperties = new TwinCollection();
reportedProperties["maxTemperature"] = 38.7;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
      "maxTemperature" : 38.7
  }
}
```

Bileşenler kullanılırken, bileşen adı içinde Özellikler oluşturulmalıdır:

```csharp
TwinCollection reportedProperties = new TwinCollection();
TwinCollection component = new TwinCollection();
component["maxTemperature"] = 38.7;
component["__t"] = "c"; // marker to identify a component
reportedProperties["thermostat1"] = component;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
    "thermostat1" : {  
      "__t" : "c",  
      "maxTemperature" : 38.7
     } 
  }
}
```

### <a name="writable-properties"></a>Yazılabilir Özellikler

Bu özellikler cihaz tarafından ayarlanabilir veya çözüm tarafından güncelleştirilir. Çözüm bir özelliği Güncelleştir, istemci içinde bir geri çağırma olarak bir bildirim alır `DeviceClient` . IoT Tak ve Kullan kurallarını izlemek için cihazın, özelliği başarıyla alınan hizmete bildirmesi gerekir.

#### <a name="report-a-writable-property"></a>Yazılabilir bir özellik bildir

Bir cihaz yazılabilir bir özellik bildirdiğinde, `ack` kurallar içinde tanımlanan değerleri içermelidir.

Bir yazılabilir özelliği bileşenler olmadan raporlamak için:

```csharp
TwinCollection reportedProperties = new TwinCollection();
TwinCollection ackProps = new TwinCollection();
ackProps["value"] = 23.2;
ackProps["ac"] = 200; // using HTTP status codes
ackProps["av"] = 0; // not readed from a desired property
ackProps["ad"] = "reported default value";
reportedProperties["targetTemperature"] = ackProps;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
  }
}
```

Bir bileşenden yazılabilir bir özellik raporlamak için, ikizi bir işaret içermelidir:

```csharp
TwinCollection reportedProperties = new TwinCollection();
TwinCollection component = new TwinCollection();
TwinCollection ackProps = new TwinCollection();
component["__t"] = "c"; // marker to identify a component
ackProps["value"] = 23.2;
ackProps["ac"] = 200; // using HTTP status codes
ackProps["av"] = 0; // not read from a desired property
ackProps["ad"] = "reported default value";
component["targetTemperature"] = ackProps;
reportedProperties["thermostat1"] = component;
await client.UpdateReportedPropertiesAsync(reportedProperties);
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
    "thermostat1": {
      "__t" : "c",
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
    }
  }
}
```

#### <a name="subscribe-to-desired-property-updates"></a>İstenen özellik güncelleştirmelerine abone ol

Hizmetler, bağlantılı cihazlarda bir bildirim tetikleyen istenen özellikleri güncelleştirebilir. Bu bildirim, güncelleştirmeyi tanımlayan sürüm numarası da dahil olmak üzere güncelleştirilmiş Desired özelliklerini içerir. Cihazların bildirilen özelliklerle aynı iletiyle yanıt vermesi gerekir `ack` .

Bileşen olmayan modeller tek özelliğe sahiptir ve alınan sürümle bildirilen öğesini oluşturur `ack` :

```csharp
await client.SetDesiredPropertyUpdateCallbackAsync(async (desired, ctx) => 
{
  JValue targetTempJson = desired["targetTemperature"];
  double targetTemperature = targetTempJson.Value<double>();

  TwinCollection reportedProperties = new TwinCollection();
  TwinCollection ackProps = new TwinCollection();
  ackProps["value"] = targetTemperature;
  ackProps["ac"] = 200;
  ackProps["av"] = desired.Version; 
  ackProps["ad"] = "desired property received";
  reportedProperties["targetTemperature"] = ackProps;

  await client.UpdateReportedPropertiesAsync(reportedProperties);
}, null);
```

Device ikizi, özelliği istenen ve bildirilen bölümlerde gösterir:

```json
{
  "desired" : {
    "targetTemperature": 23.2,
    "$version" : 3
  },
  "reported": {
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
  }
}
```

Bileşenleri olan modeller, bileşen adı ile Sarmalanan istenen özellikleri alır ve bildirilen özelliği geri bildirmelidir `ack` :

```csharp
await client.SetDesiredPropertyUpdateCallbackAsync(async (desired, ctx) =>
{
  JObject thermostatComponent = desired["thermostat1"];
  JToken targetTempProp = thermostatComponent["targetTemperature"];
  double targetTemperature = targetTempProp.Value<double>();

  TwinCollection reportedProperties = new TwinCollection();
  TwinCollection component = new TwinCollection();
  TwinCollection ackProps = new TwinCollection();
  component["__t"] = "c"; // marker to identify a component
  ackProps["value"] = targetTemperature;
  ackProps["ac"] = 200; // using HTTP status codes
  ackProps["av"] = desired.Version; // not readed from a desired property
  ackProps["ad"] = "desired property received";
  component["targetTemperature"] = ackProps;
  reportedProperties["thermostat1"] = component;

  await client.UpdateReportedPropertiesAsync(reportedProperties);
}, null);
```

Bileşenler için Device ikizi, istenen ve raporlanan bölümleri şu şekilde gösterir:

```json
{
  "desired" : {
    "thermostat1" : {
        "__t" : "c",
        "targetTemperature": 23.2,
    }
    "$version" : 3
  },
  "reported": {
    "thermostat1" : {
        "__t" : "c",
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "complete"
      }
    }
  }
}
```

### <a name="commands"></a>Komutlar

Bileşen olmayan modeller, hizmet tarafından çağrıldığı için komut adını alır.

Bileşenleri olan modeller, bileşen ve ayırıcıya ön ek olarak kullanılacak komut adını alır `*` .

```csharp
await client.SetMethodHandlerAsync("themostat*reboot", (MethodRequest req, object ctx) =>
{
  Console.WriteLine("REBOOT");
  return Task.FromResult(new MethodResponse(200));
},
null);
```

#### <a name="request-and-response-payloads"></a>İstek ve yanıt yükleri

Komutları, istek ve yanıt yüklerini tanımlamak için türleri kullanır. Bir cihaz gelen giriş parametresinin serisini çıkarmalıdır ve yanıtı serileştirmelidir. Aşağıdaki örnek, yüklerde tanımlanan karmaşık türlerle bir komutun nasıl uygulanacağını gösterir:

```json
{
  "@type": "Command",
  "name": "start",
  "request": {
    "name": "startRequest",
    "schema": {
      "@type": "Object",
      "fields": [
        {
          "name": "startPriority",
          "schema": "integer"
        },
        {
          "name": "startMessage",
          "schema" : "string"
        }
      ]
    }
  },
  "response": {
    "name": "startReponse",
    "schema": {
      "@type": "Object",
      "fields": [
        {
            "name": "startupTime",
            "schema": "integer" 
        },
        {
          "name": "startupMessage",
          "schema": "string"
        }
      ]
    }
  }
}
```

Aşağıdaki kod parçacıkları, bir cihazın serileştirme ve serisini kaldırma için kullanılan türler dahil olmak üzere bu komut tanımını nasıl uyguladığını göstermektedir:

```csharp
class startRequest
{
  public int startPriority { get; set; }
  public string startMessage { get; set; }
}

class startResponse
{
  public int startupTime { get; set; }
  public string startupMessage { get; set; }
}

// ... 

await client.SetMethodHandlerAsync("start", (MethodRequest req, object ctx) =>
{
  var startRequest = JsonConvert.DeserializeObject<startRequest>(req.DataAsJson);
  Console.WriteLine($"Received start command with priority ${startRequest.startPriority} and ${startRequest.startMessage}");

  var startResponse = new startResponse
  {
    startupTime = 123,
    startupMessage = "device started with message " + startRequest.startMessage
  };

  string responsePayload = JsonConvert.SerializeObject(startResponse);
  MethodResponse response = new MethodResponse(Encoding.UTF8.GetBytes(responsePayload), 200);
  return Task.FromResult(response);
},null);
```

> [!Tip]
> İstek ve yanıt adları, tel üzerinden iletilen serileştirilmiş yüklere mevcut değildir.

## <a name="interact-with-the-device"></a>Cihazla etkileşim kurma 

IoT Tak ve Kullan, IoT Hub 'ınız ile model KIMLIĞINI duyurduğu cihazları kullanmanıza olanak sağlar. Örneğin, bir cihazın özelliklerine ve komutlarına doğrudan erişebilirsiniz.

IoT Hub 'ınıza bağlı bir IoT Tak ve Kullan cihazı kullanmak için, IoT Hub REST API veya IoT dil SDK 'Lardan birini kullanın. Aşağıdaki örneklerde IoT Hub REST API kullanılır. API 'nin geçerli sürümü `2020-05-31-preview` . `?api-version=2020-05-31`Rest PI çağrılarınızın sonuna ekleyin.

Termostat cihazınız çağrılırsa `t-123` , REST API Get çağrısıyla cihazınız tarafından uygulanan tüm arabirimlerin tüm özelliklerini alırsınız:

```REST
GET /digitalTwins/t-123
```

Bu çağrıda, `$metadata.$model` cihaz tarafından duyurulan model kimliğiyle JSON özelliği dahil edilir.

Tüm arabirimlerde tüm özelliklere, `GET /DigitalTwin/{device-id}` cihazın tanımlayıcısı olan REST API şablonuyla erişilir `{device-id}` :

```REST
GET /digitalTwins/{device-id}
```

IoT Tak ve Kullan cihaz komutlarını doğrudan çağırabilirsiniz. `Thermostat` `t-123` Cihazdaki bileşen bir `restart` komuta sahipse, bunu bir REST API Post çağrısıyla çağırabilirsiniz:

```REST
POST /digitalTwins/t-123/components/Thermostat/commands/restart
```

Genel olarak, bu REST API şablonu aracılığıyla komutlar çağrılabilir:

- `device-id`: cihaz için tanımlayıcı.
- `component-name`: cihaz yetenek modelindeki Implements bölümünde arabirimin adı.
- `command-name`: komutun adı.

```REST
/digitalTwins/{device-id}/components/{component-name}/commands/{command-name}
```

## <a name="next-steps"></a>Sonraki adımlar

Artık cihaz modelleme hakkında bilgi edindiğinize göre, bazı ek kaynaklar aşağıda verilmiştir:

- [Dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [C cihaz SDK’sı](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT REST API](https://docs.microsoft.com/rest/api/iothub/device)
- [Model bileşenleri](./concepts-components.md)
