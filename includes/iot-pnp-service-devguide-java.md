---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 10/20/2020
ms.openlocfilehash: 33752c1ebb83c5d63e8e1cb396c52f01f07046cd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244701"
---
Aşağıdaki kaynaklar da kullanılabilir:

- [Java SDK'sı başvuru belgeleri](/java/api/com.microsoft.azure.sdk.iot.service.devicetwin.devicetwindevice)
- [Hizmet istemcisi örnekleri](https://github.com/Azure/azure-iot-sdk-java/blob/master/service/iot-service-samples/pnp-service-sample)
- [Dijital TWINS örnekleri](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples/digitaltwin-service-samples)

## <a name="iot-hub-service-client-examples"></a>IoT Hub hizmeti istemci örnekleri

Bu bölümde, **com. Microsoft. Azure. SDK. IoT. Service. devicetwin** ad alanından IoT Hub Service Client ve **devicetwin** ve **devicemethod** sınıfları kullanılarak Java örnekleri gösterilmektedir. Cihaz TWINS 'i kullanarak cihaz durumuyla etkileşim kurmak için **Devicetwin** sınıfını kullanırsınız. **Devicetwin** sınıfını, IoT Hub [cihaz kayıtlarını sorgulamak](../articles/iot-hub/iot-hub-devguide-query-language.md) için de kullanabilirsiniz. Cihazdaki komutları çağırmak için **Devicemethod** sınıfını kullanın. Cihazın [Dtdl](../articles/iot-pnp/concepts-digital-twin.md) modeli, cihazın uyguladığı özellikleri ve komutları tanımlar. Kod parçacıklarında, `deviceId` değişken IoT Hub 'ınıza kayıtlı olan ıot Tak ve kullan cihazının CIHAZ kimliğini barındırır.

### <a name="get-the-device-twin-and-model-id"></a>Cihaz ikizi ve model KIMLIĞINI al

IoT Hub 'ınıza bağlanan IoT Tak ve Kullan cihazının cihaz ikizi ve model KIMLIĞINI almak için:

```java
DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);

// ...

DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
twinClient.getTwin(twin);
System.out.println("Model Id of this Twin is: " + twin.getModelId());
```

### <a name="update-device-twin"></a>Cihaz ikizi Güncelleştir

Aşağıdaki kod parçacığı, `targetTemperature` bir cihazdaki özelliğinin nasıl güncelleştirilmesini gösterir. Güncelleştirmeden önce ikizi almalısınız. Özelliği, cihazın varsayılan bileşeninde tanımlanmıştır:

```java
DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
twinClient.getTwin(twin);

double propertyValue = 60.2;
twin.setDesiredProperties(Collections.singleton(new Pair("targetTemperature", propertyValue)));
twinClient.updateTwin(twin);
```

Aşağıdaki kod parçacığında, `targetTemperature` bir bileşen üzerinde özelliğin nasıl güncelleştirilecek gösterilmektedir. Güncelleştirmeden önce ikizi almalısınız. Özelliği **thermostat1** bileşeninde tanımlanmıştır:

```java
public static Set<Pair> CreateComponentPropertyPatch(@NonNull String propertyName, @NonNull double propertyValue, @NonNull String componentName)
{
    JsonObject patchJson = new JsonObject();
    patchJson.addProperty("__t", "c");
    patchJson.addProperty(propertyName, propertyValue);
    return singleton(new Pair(componentName, patchJson));
}

// ...

DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
twinClient.getTwin(twin);

double propertyValue = 60.2;
twin.setDesiredProperties(CreateComponentPropertyPatch("targetTemperature", propertyValue, "thermostat1"));
twinClient.updateTwin(twin);
```

Bir bileşenin özelliği için, özellik düzeltme eki aşağıdaki örneğe benzer şekilde görünür:

```json
{
  "thermostat1":
  {
    "__t": "c",
    "targetTemperature": 60.2
  }
}
```

### <a name="call-command"></a>Call komutu

Aşağıdaki kod parçacığı, `getMaxMinReport` varsayılan bir bileşende tanımlanan komutun nasıl çağıralınacağını gösterir:

```java
DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

Long responseTimeout = TimeUnit.SECONDS.toSeconds(200);
Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);

String commandInput = ZonedDateTime.now(ZoneOffset.UTC).minusMinutes(5).format(DateTimeFormatter.ISO_DATE_TIME);
MethodResult result = methodClient.invoke(deviceId, "getMaxMinReport", responseTimeout, connectTimeout, commandInput);

System.out.println("Method result status is: " + result.getStatus());
```

Aşağıdaki kod parçacığında, bir bileşende komutun nasıl çağrılacağını gösterir `getMaxMinReport` . Komut **thermostat1** bileşeninde tanımlanmıştır:

```java
DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

Long responseTimeout = TimeUnit.SECONDS.toSeconds(200);
Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);

String commandInput = ZonedDateTime.now(ZoneOffset.UTC).minusMinutes(5).format(DateTimeFormatter.ISO_DATE_TIME);
MethodResult result = methodClient.invoke(deviceId, "thermostat1*getMaxMinReport", responseTimeout, connectTimeout, commandInput);

System.out.println("Method result status is: " + result.getStatus());
```

## <a name="iot-hub-digital-twin-examples"></a>IoT Hub Digital ikizi örnekleri

Dijital TWINS kullanarak cihaz durumuyla etkileşimde bulunmak için **com. Microsoft. Azure. SDK. IoT. Service. digitaltwin** ad alanındaki **Digitaltwınasyncclient** sınıfını kullanın. Aşağıdaki örnekler aynı ad alanındaki **Updateoperationutility** ve **BasicDigitalTwin** sınıflarını da kullanır. Cihazın [Dtdl](../articles/iot-pnp/concepts-digital-twin.md) modeli, cihazın uyguladığı özellikleri ve komutları tanımlar.

`digitalTwinid`Değişkeni, IoT Hub 'ınıza kayıtlı olan ıot Tak ve kullan cihazının CIHAZ kimliğini içerir.

### <a name="get-the-digital-twin-and-model-id"></a>Dijital ikizi ve model KIMLIĞINI alın

IoT Hub 'ınıza bağlanan IoT Tak ve Kullan cihazının dijital ikizi ve model KIMLIĞINI almak için:

```java
DigitalTwinAsyncClient asyncClient = DigitalTwinAsyncClient.createFromConnectionString(iotHubConnectionString);

CountDownLatch latch = new CountDownLatch(1);
asyncClient.getDigitalTwin(digitalTwinid, BasicDigitalTwin.class)
    .subscribe(
        getResponse ->
        {
            System.out.println("Digital Twin Model Id: " + getResponse.getMetadata().getModelId());
            System.out.println("Digital Twin: " + prettyBasicDigitalTwin(getResponse));
            latch.countDown();
        },
        error ->
        {
            System.out.println("Get Digital Twin failed: " + error);
            latch.countDown();
        });

latch.await(10, TimeUnit.SECONDS);

// ...

private static String prettyBasicDigitalTwin(BasicDigitalTwin basicDigitalTwin)
{
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(basicDigitalTwin);
}
```

### <a name="update-digital-twin"></a>Dijital ikizi Güncelleştir

Aşağıdaki kod parçacığı, `targetTemperature` bir cihazdaki özelliğinin nasıl güncelleştirilmesini gösterir. Özelliği, cihazın varsayılan bileşeninde tanımlanmıştır:

```java
DigitalTwinAsyncClient asyncClient = DigitalTwinAsyncClient.createFromConnectionString(iotHubConnectionString);

CountDownLatch latch1 = new CountDownLatch(1);

UpdateOperationUtility updateOperationUtility = new UpdateOperationUtility();

// Add a new property.
updateOperationUtility.appendAddPropertyOperation("/" + "targetTemperature", 35);
asyncClient.updateDigitalTwin(digitalTwinid, updateOperationUtility.getUpdateOperations())
    .subscribe(
        getResponse ->
        {
            System.out.println("Updated Digital Twin");
            latch1.countDown();
        },
        error ->
        {
            System.out.println("Update Digital Twin failed: " + error);
            latch1.countDown();
        });
latch1.await(10, TimeUnit.SECONDS);
GetDigitalTwin();

// Replace an existing property.
CountDownLatch latch2 = new CountDownLatch(1);
updateOperationUtility.appendReplacePropertyOperation("/targetTemperature", 50);
asyncClient.updateDigitalTwin(digitalTwinid, updateOperationUtility.getUpdateOperations())
    .subscribe(
        getResponse ->
        {
            System.out.println("Updated Digital Twin");
            latch2.countDown();
        },
        error ->
        {
            System.out.println("Update Digital Twin failed: " + error);
            latch2.countDown();
        });

latch2.await(10, TimeUnit.SECONDS);
GetDigitalTwin();
```

Aşağıdaki kod parçacığında, `targetTemperature` bir bileşen üzerinde özelliğin nasıl güncelleştirilecek gösterilmektedir. Özelliği **thermostat1** bileşeninde tanımlanmıştır:

```java
DigitalTwinClient client = DigitalTwinClient.createFromConnectionString(iotHubConnectionString);

// Get digital twin.
ServiceResponseWithHeaders<String, DigitalTwinGetHeaders> getResponse = client.getDigitalTwinWithResponse(digitalTwinid, String.class);

// Construct the options for conditional update.
DigitalTwinUpdateRequestOptions options = new DigitalTwinUpdateRequestOptions();
options.setIfMatch(getResponse.headers().eTag());

UpdateOperationUtility updateOperationUtility = new UpdateOperationUtility();

Map<String, Object> t1properties = new HashMap<>();
t1properties.put("targetTemperature", 50);
updateOperationUtility.appendReplaceComponentOperation("/thermostat1", t1properties);

digitalTwinUpdateOperations = updateOperationUtility.getUpdateOperations();
updateResponse = client.updateDigitalTwinWithResponse(digitalTwinid, digitalTwinUpdateOperations, options);
System.out.println("Update Digital Twin response status: " + updateResponse.response().message());

getResponse = client.getDigitalTwinWithResponse(digitalTwinid, String.class);
```

### <a name="call-command"></a>Call komutu

Aşağıdaki kod parçacığı, `getMaxMinReport` varsayılan bir bileşende tanımlanan komutun nasıl çağıralınacağını gösterir:

```java
CountDownLatch latch = new CountDownLatch(1);

String commandInput = ZonedDateTime.now(ZoneOffset.UTC).minusMinutes(5).format(DateTimeFormatter.ISO_DATE_TIME);

// Invoke a method on root level.
asyncClient.invokeCommand(digitalTwinid, "getMaxMinReport", commandInput)
    .subscribe(
        response ->
        {
            System.out.println("Invoked Command getMaxMinReport response: " + prettyString(response.getPayload()));
            latch.countDown();
        },
        error ->
        {
            RestException ex = (RestException)error;
            if(ex.response().code() == 404) {
                System.out.println("Invoked Command getMaxMinReport failed: " + error);
            }
            else {
                System.out.println("Ensure the device sample is running for this sample to succeed");
            }
            latch.countDown();
        });

latch.await(10, TimeUnit.SECONDS);

// ...

private static String prettyString(String str)
{
    Gson gson = new Gson();
    Gson gsonBuilder = new GsonBuilder().setPrettyPrinting().create();
    return gsonBuilder.toJson(gson.fromJson(str, Object.class));
}
```

Aşağıdaki kod parçacığında, bir bileşende komutun nasıl çağrılacağını gösterir `getMaxMinReport` . Komut **thermostat1** bileşeninde tanımlanmıştır:

```java
DigitalTwinClient client = DigitalTwinClient.createFromConnectionString(iotHubConnectionString);

String commandInput = ZonedDateTime.now(ZoneOffset.UTC).minusMinutes(5).format(DateTimeFormatter.ISO_DATE_TIME);

DigitalTwinInvokeCommandRequestOptions options = new DigitalTwinInvokeCommandRequestOptions();
try {
    ServiceResponseWithHeaders<DigitalTwinCommandResponse, DigitalTwinInvokeCommandHeaders> commandResponse = client.invokeComponentCommandWithResponse(digitalTwinid, "thermostat1", "getMaxMinReport", commandInput, options);
    System.out.println("Command getMaxMinReport, payload: " + prettyString(commandResponse.body().getPayload()));
    System.out.println("Command getMaxMinReport, status: " + commandResponse.body().getStatus());
} catch (RestException ex)
{
    if(ex.response().code() == 404)
    {
        System.out.println("Ensure the device sample is running for this sample to succeed.");
    }
    else
    {
        throw ex;
    }
}

// ...

private static String prettyString(String str)
{
    Gson gson = new Gson();
    Gson gsonBuilder = new GsonBuilder().setPrettyPrinting().create();
    return gsonBuilder.toJson(gson.fromJson(str, Object.class));
}
```

## <a name="read-device-telemetry"></a>Cihaz telemetrisini okuma

IoT Tak ve Kullan cihazları DTDL modelinde tanımlanan telemetrileri IoT Hub gönderir. Varsayılan olarak, IoT Hub Telemetriyi kullanabileceğiniz bir Event Hubs uç noktasına yönlendirir. Daha fazla bilgi edinmek için bkz. [farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](../articles/iot-hub/iot-hub-devguide-messages-d2c.md).

Aşağıdaki kod parçacığı, varsayılan Event Hubs uç noktasından Telemetriyi okumayı gösterir. Bu kod parçacığındaki kod, IoT Hub hızlı başlangıçta [bir cihazdan IoT Hub 'ına telemetri gönderme ve arka uç uygulamasıyla okuma](../articles/iot-hub/quickstart-send-telemetry-java.md)işlemi gerçekleştirmektir:

```java
import com.azure.messaging.eventhubs.EventHubClientBuilder;
import com.azure.messaging.eventhubs.EventHubConsumerAsyncClient;

// ...

EventHubClientBuilder eventHubClientBuilder = new EventHubClientBuilder()
    .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
    .connectionString(eventHubCompatibleConnectionString);

try (EventHubConsumerAsyncClient eventHubConsumerAsyncClient = eventHubClientBuilder.buildAsyncConsumerClient()) {

    receiveFromAllPartitions(eventHubConsumerAsyncClient);

}

// ...

private static void receiveFromAllPartitions(EventHubConsumerAsyncClient eventHubConsumerAsyncClient) {

eventHubConsumerAsyncClient
    .receive(false) // set this to false to read only the newly available events
    .subscribe(partitionEvent -> {
        System.out.println();
        System.out.printf("%nTelemetry received from partition %s:%n%s",
            partitionEvent.getPartitionContext().getPartitionId(), partitionEvent.getData().getBodyAsString());
        System.out.printf("%nApplication properties (set by device):%n%s", partitionEvent.getData().getProperties());
        System.out.printf("%nSystem properties (set by IoT Hub):%n%s",
            partitionEvent.getData().getSystemProperties());
    }, ex -> {
        System.out.println("Error receiving events " + ex);
    }, () -> {
        System.out.println("Completed receiving events");
    });
}
```

Önceki koddan bulunan aşağıdaki çıktıda, bileşen olmayan **termostat** IoT Tak ve kullan cihazının yalnızca varsayılan bileşene sahip olduğu sıcaklık telemetrisi gösterilmektedir. `dt-dataschema`System özelliği, model kimliğini gösterir:

```cmd/sh
Telemetry received from partition 1:
{"temperature": 10.700000}
Application properties (set by device):
{$.cdid=my-pnp-device}
System properties (set by IoT Hub):
{correlation-id=dd960185-6ddb-4b5f-89bb-e26b0b3c201e, content-encoding=UTF-8, iothub-connection-auth-method={"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}, iothub-enqueuedtime=Tue Oct 20 12:28:10 BST 2020, dt-dataschema=dtmi:com:example:Thermostat;1, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, iothub-connection-auth-generation-id=637375776990653481, group-sequence=0, iothub-message-source=Telemetry, creation-time=0, message-id=1c05cece-070b-4e2e-b2e8-e263858594a3, content-type=application/json}

Telemetry received from partition 1:
{"temperature": 10.700000}
Application properties (set by device):
{$.cdid=my-pnp-device}
System properties (set by IoT Hub):
{correlation-id=d10a7350-43ef-4cf6-9db5-a4b08684cd9d, content-encoding=UTF-8, iothub-connection-auth-method={"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}, iothub-enqueuedtime=Tue Oct 20 12:28:15 BST 2020, dt-dataschema=dtmi:com:example:Thermostat;1, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, iothub-connection-auth-generation-id=637375776990653481, group-sequence=0, iothub-message-source=Telemetry, creation-time=0, message-id=d3a80af4-1246-41a0-a09a-582a12c17a00, content-type=application/json}
```

Önceki koddan bulunan aşağıdaki çıktıda, çok bileşen **TemperatureController** IoT Tak ve kullan cihazının gönderdiği sıcaklık telemetrisi gösterilmektedir. `dt-subject`System özelliği, Telemetriyi gönderen bileşenin adını gösterir. Bu örnekte, iki bileşen `thermostat1` ve `thermostat2` dtdl modelinde tanımlanmıştır. `dt-dataschema`System özelliği, model kimliğini gösterir:

```cmd/sh
Telemetry received from partition 1:
null
Application properties (set by device):
{$.cdid=my-pnp-device}
System properties (set by IoT Hub):
{correlation-id=413002d0-2107-4c08-8f4a-995ae1f4047b, content-encoding=UTF-8, iothub-connection-auth-method={"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}, iothub-enqueuedtime=Tue Oct 20 12:31:14 BST 2020, dt-dataschema=dtmi:com:example:TemperatureController;1, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, iothub-connection-auth-generation-id=637387902591517456, group-sequence=0, iothub-message-source=Telemetry, creation-time=0, message-id=da8bd068-850e-43fb-862f-66080d5969e4, content-type=application/json, dt-subject=thermostat1}

Telemetry received from partition 1:
null
Application properties (set by device):
{$.cdid=my-pnp-device}
System properties (set by IoT Hub):
{correlation-id=2d9407e5-413f-4f8d-bd30-cd153e03c72f, content-encoding=UTF-8, iothub-connection-auth-method={"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}, iothub-enqueuedtime=Tue Oct 20 12:31:14 BST 2020, dt-dataschema=dtmi:com:example:TemperatureController;1, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, iothub-connection-auth-generation-id=637387902591517456, group-sequence=0, iothub-message-source=Telemetry, creation-time=0, message-id=ed419c4e-ef2c-4acf-8991-6245059c5fdc, content-type=application/json, dt-subject=thermostat2}
```

## <a name="read-device-twin-change-notifications"></a>Cihaz ikizi değişiklik bildirimlerini oku

Desteklenen bir uç noktaya yönlendirmek üzere cihaz ikizi değişiklik bildirimleri oluşturmak için IoT Hub yapılandırabilirsiniz. Daha fazla bilgi edinmek için bkz. [IoT Hub ileti yönlendirmeyi kullanarak, telemetri olmayan olaylar > farklı uç noktalara cihazdan buluta iletiler gönderme](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Önceki Java kod parçacığında gösterilen kod, bileşen olmayan bir termostat cihazı için IoT Hub cihaz ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Telemetry received from partition 1:
{"version":11,"properties":{"reported":{"maxTempSinceLastReboot":43.4,"$metadata":{"$lastUpdated":"2020-10-20T11:50:41.123127Z","maxTempSinceLastReboot":{"$lastUpdated":"2020-10-20T11:50:41.123127Z"}},"$version":10}}}
Application properties (set by device):
{operationTimestamp=2020-10-20T11:50:41.1231270+00:00, opType=updateTwin, hubName=my-pnp-hub, deviceId=my-pnp-device, iothub-message-schema=twinChangeNotification}
System properties (set by IoT Hub):
{user-id=[B@12fd5bb4, correlation-id=11339418426a, content-encoding=utf-8, iothub-enqueuedtime=Tue Oct 20 12:50:41 BST 2020, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, group-sequence=0, iothub-message-source=twinChangeEvents, creation-time=0, content-type=application/json}
```

Önceki Java kod parçacığında gösterilen kod, bileşenler içeren bir cihaz için cihaz ikizi değişiklik bildirimleri IoT Hub oluşturduğunda aşağıdaki çıktıyı üretir. Bu örnek, bir termostat bileşeni olan bir sıcaklık algılayıcısı cihazı bildirim oluşturduğunda çıktıyı gösterir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Telemetry received from partition 1:
{"version":9,"properties":{"reported":{"thermostat1":{"__t":"c","maxTempSinceLastReboot":32.5},"$metadata":{"$lastUpdated":"2020-10-20T11:48:01.2960851Z","thermostat1":{"$lastUpdated":"2020-10-20T11:48:01.2960851Z","__t":{"$lastUpdated":"2020-10-20T11:48:01.2960851Z"},"maxTempSinceLastReboot":{"$lastUpdated":"2020-10-20T11:48:01.2960851Z"}}},"$version":8}}}
Application properties (set by device):
{operationTimestamp=2020-10-20T11:48:01.2960851+00:00, opType=updateTwin, hubName=my-pnp-hub, deviceId=my-pnp-device, iothub-message-schema=twinChangeNotification}
System properties (set by IoT Hub):
{user-id=[B@23949bae, correlation-id=113334d542e1, content-encoding=utf-8, iothub-enqueuedtime=Tue Oct 20 12:48:01 BST 2020, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, group-sequence=0, iothub-message-source=twinChangeEvents, creation-time=0, content-type=application/json}
```

## <a name="read-digital-twin-change-notifications"></a>Dijital ikizi değişiklik bildirimlerini oku

Desteklenen bir uç noktaya yönlendirmek için dijital ikizi değişiklik bildirimleri oluşturacak şekilde IoT Hub yapılandırabilirsiniz. Daha fazla bilgi edinmek için bkz. [IoT Hub ileti yönlendirmeyi kullanarak, telemetri olmayan olaylar > farklı uç noktalara cihazdan buluta iletiler gönderme](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Önceki Java kod parçacığında gösterilen kod, IoT Hub bileşen olmayan bir termostat cihazı için dijital ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Telemetry received from partition 1:
[{"op":"replace","path":"/$metadata/maxTempSinceLastReboot/lastUpdateTime","value":"2020-10-20T11:52:40.627628Z"},{"op":"replace","path":"/maxTempSinceLastReboot","value":16.9}]
Application properties (set by device):
{operationTimestamp=2020-10-20T11:52:40.6276280+00:00, opType=updateTwin, hubName=my-pnp-hub, deviceId=my-pnp-device, iothub-message-schema=digitalTwinChangeNotification}
System properties (set by IoT Hub):
{user-id=[B@4475ce2a, correlation-id=1133db52c0e0, content-encoding=utf-8, iothub-enqueuedtime=Tue Oct 20 12:52:40 BST 2020, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, group-sequence=0, iothub-message-source=digitalTwinChangeEvents, creation-time=0, content-type=application/json-patch+json}
```

Önceki Java kod parçacığında gösterilen kod, IoT Hub bileşenleri olan bir cihaz için dijital ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Bu örnek, bir termostat bileşeni olan bir sıcaklık algılayıcısı cihazı bildirim oluşturduğunda çıktıyı gösterir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Telemetry received from partition 1:
[{"op":"add","path":"/thermostat1","value":{"$metadata":{"maxTempSinceLastReboot":{"lastUpdateTime":"2020-10-20T11:31:04.7811405Z"}},"maxTempSinceLastReboot":27.2}}]
Application properties (set by device):
{operationTimestamp=2020-10-20T11:31:04.7811405+00:00, opType=updateTwin, hubName=my-pnp-hub, deviceId=my-pnp-device, iothub-message-schema=digitalTwinChangeNotification}
System properties (set by IoT Hub):
{user-id=[B@75981aa, correlation-id=1130d6f4d212, content-encoding=utf-8, iothub-enqueuedtime=Tue Oct 20 12:31:04 BST 2020, absolute-expiry-time=0, iothub-connection-device-id=my-pnp-device, group-sequence=0, iothub-message-source=digitalTwinChangeEvents, creation-time=0, content-type=application/json-patch+json}
```
