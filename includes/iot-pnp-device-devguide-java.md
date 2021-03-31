---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 11/19/2020
ms.openlocfilehash: 51f66a674f9e730670084a43bd7bf059a0742cc3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104582835"
---
## <a name="model-id-announcement"></a>Model KIMLIĞI duyurusu

Model KIMLIĞINI duyurmak için cihazın bağlantı bilgilerine eklemesi gerekir:

```java
ClientOptions options = new ClientOptions();
options.setModelId(MODEL_ID);
deviceClient = new DeviceClient(deviceConnectionString, protocol, options);
```

`ClientOptions`Aşırı yükleme, `DeviceClient` bir bağlantıyı başlatmak için kullanılan tüm yöntemlerde kullanılabilir.

> [!TIP]
> Modüller ve IoT Edge için yerine kullanın `ModuleClient` `DeviceClient` .

> [!TIP]
> Bu, bir cihazın model KIMLIĞINI ayarlayabir zaman, cihaz bağlandıktan sonra güncelleştirilemeyebilir.

## <a name="dps-payload"></a>DPS yükü

[Cihaz sağlama hizmeti 'ni (DPS)](../articles/iot-dps/about-iot-dps.md) kullanan cihazlar, `modelId` aşağıdaki JSON yükü kullanılarak sağlama işlemi sırasında kullanılacak öğesini içerebilir.

```json
{
    "modelId" : "dtmi:com:example:Thermostat;1"
}
```

## <a name="implement-telemetry-properties-and-commands"></a>Telemetri, özellik ve komutları uygulama

[Iot Tak ve kullan modellerdeki bileşenleri anlama](../articles/iot-pnp/concepts-modeling-guide.md)bölümünde açıklandığı gibi, cihaz oluşturucuların cihazlarını açıklamak için bileşenleri kullanmak istemeseler de karar vermelidir. Bileşenler kullanılırken, cihazların bu bölümde açıklanan kurallara uymalıdır.

### <a name="telemetry"></a>Telemetri

Varsayılan bir bileşen herhangi bir özel özellik gerektirmez.

İç içe bileşenler kullanılırken, cihazların bileşen adı ile bir ileti özelliği ayarlaması gerekir:

```java
private static void sendTemperatureTelemetry(String componentName) {
  double currentTemperature = temperature.get(componentName);

  Map<String, Object> payload = singletonMap("temperature", currentTemperature);

  Message message = new Message(gson.toJson(payload));
  message.setContentEncoding("utf-8");
  message.setContentTypeFinal("application/json");

  if (componentName != null) {
      message.setProperty("$.sub", componentName);
  }
  deviceClient.sendEventAsync(message, new MessageIotHubEventCallback(), message);
}
```

### <a name="read-only-properties"></a>Salt okunur özellikler

Varsayılan bileşenden bir özelliği raporlamak için özel bir yapı gerekmez:

```java
Property reportedProperty = new Property("maxTempSinceLastReboot", 38.7);

deviceClient.sendReportedProperties(Collections.singleton(reportedProperty));
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
      "maxTempSinceLastReboot" : 38.7
  }
}
```

İç içe bileşenler kullanılırken, Özellikler bileşen adı içinde oluşturulmalıdır:

```java
Map<String, Object> componentProperty = new HashMap<String, Object>() {{
    put("__t", "c");
    put("maxTemperature", 38.7);
}};

Set<Property> reportedProperty = new Property("thermostat1", componentProperty)

deviceClient.sendReportedProperties(reportedProperty);
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

Bu özellikler cihaz tarafından ayarlanabilir veya çözüm tarafından güncelleştirilir. Çözüm bir özelliği güncelleştirdi, istemci veya içinde geri çağırma olarak bir bildirim alır `DeviceClient` `ModuleClient` . IoT Tak ve Kullan kurallarını izlemek için cihazın, özelliği başarıyla alınan hizmete bildirmesi gerekir.

#### <a name="report-a-writable-property"></a>Yazılabilir bir özellik bildir

Bir cihaz yazılabilir bir özellik bildirdiğinde, `ack` kurallar içinde tanımlanan değerleri içermelidir.

Yazılabilir bir özelliği varsayılan bileşenden raporlamak için:

```java
@AllArgsConstructor
private static class EmbeddedPropertyUpdate {
  @NonNull
  @SerializedName("value")
  public Object value;
  @NonNull
  @SerializedName("ac")
  public Integer ackCode;
  @NonNull
  @SerializedName("av")
  public Integer ackVersion;
  @SerializedName("ad")
  public String ackDescription;
}

EmbeddedPropertyUpdate completedUpdate = new EmbeddedPropertyUpdate(23.2, 200, 3, "Successfully updated target temperature");
Property reportedPropertyCompleted = new Property("targetTemperature", completedUpdate);
deviceClient.sendReportedProperties(Collections.singleton(reportedPropertyCompleted));
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
      "targetTemperature": {
          "value": 23.2,
          "ac": 200,
          "av": 3,
          "ad": "Successfully updated target temperature"
      }
  }
}
```

İç içe geçmiş bir bileşenden yazılabilir bir özellik raporlamak için, ikizi bir işaret içermelidir:

```java
Map<String, Object> embeddedProperty = new HashMap<String, Object>() {{
    put("value", 23.2);
    put("ac", 200);
    put("av", 3);
    put("ad", "complete");
}};

Map<String, Object> componentProperty = new HashMap<String, Object>() {{
    put("__t", "c");
    put("targetTemperature", embeddedProperty);
}};

Set<Property> reportedProperty = new Property("thermostat1", componentProperty));

deviceClient.sendReportedProperties(reportedProperty);
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

Varsayılan bir bileşen, tek bir özelliği görür ve `ack` alınan sürüm ile bildirilen bir oluşturur:

```java
private static class TargetTemperatureUpdateCallback implements TwinPropertyCallBack {

    String propertyName = "targetTemperature";

    @Override
    public void TwinPropertyCallBack(Property property, Object context) {
        double targetTemperature = ((Number)property.getValue()).doubleValue();

        EmbeddedPropertyUpdate completedUpdate = new EmbeddedPropertyUpdate(temperature, 200, property.getVersion(), "Successfully updated target temperature");
        Property reportedPropertyCompleted = new Property(propertyName, completedUpdate);
        deviceClient.sendReportedProperties(Collections.singleton(reportedPropertyCompleted));
    }
}

// ...

deviceClient.startDeviceTwin(new TwinIotHubEventCallback(), null, new TargetTemperatureUpdateCallback(), null);
Map<Property, Pair<TwinPropertyCallBack, Object>> desiredPropertyUpdateCallback =
  Collections.singletonMap(
    new Property("targetTemperature", null),
    new Pair<>(new TargetTemperatureUpdateCallback(), null));
deviceClient.subscribeToTwinDesiredProperties(desiredPropertyUpdateCallback);
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
          "ad": "Successfully updated target temperature"
      }
  }
}
```

İç içe yerleştirilmiş bir bileşen, bileşen adı ile Sarmalanan istenen özellikleri alır ve bildirilen özelliği geri bildirmelidir `ack` :

```java
private static final Map<String, Double> temperature = new HashMap<>();

private static class TargetTemperatureUpdateCallback implements TwinPropertyCallBack {

    String propertyName = "targetTemperature";

    @Override
    public void TwinPropertyCallBack(Property property, Object context) {
        String componentName = (String) context;

        if (property.getKey().equalsIgnoreCase(componentName)) {
            double targetTemperature = (double) ((TwinCollection) property.getValue()).get(propertyName);

            Map<String, Object> embeddedProperty = new HashMap<String, Object>() {{
                put("value", temperature.get(componentName));
                put("ac", 200);
                put("av", property.getVersion().longValue());
                put("ad", "Successfully updated target temperature.");
            }};

            Map<String, Object> componentProperty = new HashMap<String, Object>() {{
                put("__t", "c");
                put(propertyName, embeddedProperty);
            }};

            Set<Property> completedPropertyPatch = new Property(componentName, componentProperty));

            deviceClient.sendReportedProperties(completedPropertyPatch);
        } else {
            log.debug("Property: Received an unrecognized property update from service.");
        }
    }
}

// ...

deviceClient.startDeviceTwin(new TwinIotHubEventCallback(), null, new GenericPropertyUpdateCallback(), null);
Map<Property, Pair<TwinPropertyCallBack, Object>> desiredPropertyUpdateCallback = Stream.of(
  new AbstractMap.SimpleEntry<Property, Pair<TwinPropertyCallBack, Object>>(
    new Property("thermostat1", null),
    new Pair<>(new TargetTemperatureUpdateCallback(), "thermostat1")),
  new AbstractMap.SimpleEntry<Property, Pair<TwinPropertyCallBack, Object>>(
    new Property("thermostat2", null),
    new Pair<>(new TargetTemperatureUpdateCallback(), "thermostat2"))
).collect(Collectors.toMap(AbstractMap.SimpleEntry::getKey, AbstractMap.SimpleEntry::getValue));

deviceClient.subscribeToTwinDesiredProperties(desiredPropertyUpdateCallback);
```

Yuvalanmış bir bileşen için cihaz ikizi, istenen ve raporlanan bölümleri aşağıdaki gibi gösterir:

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

Varsayılan bir bileşen, hizmet tarafından çağrıldığı şekilde komut adını alır.

İç içe yerleştirilmiş bir bileşen, bileşen adı ve ayırıcıdan önekli komut adını alır `*` .

```java
deviceClient.subscribeToDeviceMethod(new MethodCallback(), null, new MethodIotHubEventCallback(), null);

// ...
private static final Map<String, Double> temperature = new HashMap<>();

private static class MethodCallback implements DeviceMethodCallback {
  final String reboot = "reboot";
  final String getMaxMinReport1 = "thermostat1*getMaxMinReport";
  final String getMaxMinReport2 = "thermostat2*getMaxMinReport";

  @Override
  public DeviceMethodData call(String methodName, Object methodData, Object context) {
    String jsonRequest = new String((byte[]) methodData, StandardCharsets.UTF_8);

    switch (methodName) {
      case reboot:
        int delay = gson.fromJson(jsonRequest, Integer.class);

        Thread.sleep(delay * 1000);

        temperature.put("thermostat1", 0.0d);
        temperature.put("thermostat2", 0.0d);

        return new DeviceMethodData(200, null);

      // ...

      default:
        log.debug("Command: command=\"{}\" is not implemented, no action taken.", methodName);
          return new DeviceMethodData(404, null);
    }
  }
}
```

#### <a name="request-and-response-payloads"></a>İstek ve yanıt yükleri

Komutları, istek ve yanıt yüklerini tanımlamak için türleri kullanır. Bir cihaz gelen giriş parametresinin serisini çıkarmalıdır ve yanıtı serileştirmelidir.

Aşağıdaki örnek, yüklerde tanımlanan karmaşık türlerle bir komutun nasıl uygulanacağını gösterir:

```json
{
  "@type": "Command",
  "name": "getMaxMinReport",
  "displayName": "Get Max-Min report.",
  "description": "This command returns the max, min and average temperature from the specified time to the current time.",
  "request": {
    "name": "since",
    "displayName": "Since",
    "description": "Period to return the max-min report.",
    "schema": "dateTime"
  },
  "response": {
    "name" : "tempReport",
    "displayName": "Temperature Report",
    "schema": {
      "@type": "Object",
      "fields": [
        {
          "name": "maxTemp",
          "displayName": "Max temperature",
          "schema": "double"
        },
        {
          "name": "minTemp",
          "displayName": "Min temperature",
          "schema": "double"
        },
        {
          "name" : "avgTemp",
          "displayName": "Average Temperature",
          "schema": "double"
        },
        {
          "name" : "startTime",
          "displayName": "Start Time",
          "schema": "dateTime"
        },
        {
          "name" : "endTime",
          "displayName": "End Time",
          "schema": "dateTime"
        }
      ]
    }
  }
}
```

Aşağıdaki kod parçacıkları, bir cihazın serileştirme ve serisini kaldırma için kullanılan türler dahil olmak üzere bu komut tanımını nasıl uyguladığını göstermektedir:

```java
deviceClient.subscribeToDeviceMethod(new GetMaxMinReportMethodCallback(), "getMaxMinReport", new MethodIotHubEventCallback(), "getMaxMinReport");

// ...

private static class GetMaxMinReportMethodCallback implements DeviceMethodCallback {
    String commandName = "getMaxMinReport";

    @Override
    public DeviceMethodData call(String methodName, Object methodData, Object context) {

        String jsonRequest = new String((byte[]) methodData, StandardCharsets.UTF_8);
        Date since = gson.fromJson(jsonRequest, Date.class);

        String responsePayload = String.format(
                "{\"maxTemp\": %.1f, \"minTemp\": %.1f, \"avgTemp\": %.1f, \"startTime\": \"%s\", \"endTime\": \"%s\"}",
                maxTemp,
                minTemp,
                avgTemp,
                since,
                endTime);

        return new DeviceMethodData(StatusCode.COMPLETED.value, responsePayload);
    }
}
```

> [!Tip]
> İstek ve yanıt adları, tel üzerinden iletilen serileştirilmiş yüklere mevcut değildir.
