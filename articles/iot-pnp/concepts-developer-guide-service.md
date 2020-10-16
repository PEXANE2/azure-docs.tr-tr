---
title: Hizmet Geliştirici Kılavuzu-IoT Tak ve Kullan | Microsoft Docs
description: Hizmet geliştiricileri için IoT Tak ve Kullan açıklaması
author: dominicbetts
ms.author: dobett
ms.date: 10/01/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 7523dd39303a211772dd39eef811f55739336ff0
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92093710"
---
# <a name="iot-plug-and-play-service-developer-guide"></a>IoT Tak ve Kullan hizmeti Geliştirici Kılavuzu

IoT Tak ve Kullan, yeteneklerini Azure IoT uygulamalarına duyuran akıllı cihazlar oluşturmanızı sağlar. IoT Tak ve Kullan cihazları, bir müşteri bunları IoT Tak ve Kullan özellikli uygulamalara bağladığında el ile yapılandırma gerektirmez.

IoT Tak ve Kullan, IoT Hub 'ınız ile model KIMLIĞINI duyurduğu cihazları kullanmanıza olanak sağlar. Örneğin, bir cihazın özelliklerine ve komutlarına doğrudan erişebilirsiniz.

IoT Hub 'ınıza bağlı bir IoT Tak ve Kullan cihazı kullanmak için, IoT hizmeti SDK 'Lardan birini veya IoT Hub REST API kullanın:

## <a name="service-sdks"></a>Hizmet SDK'ları

Cihazlarla ve modüllerle etkileşim kurmak için çözümünüzdeki Azure IoT hizmeti SDK 'larını kullanın. Örneğin, ikizi özelliklerini okumak ve güncelleştirmek ve komutları çağırmak için hizmet SDK 'larını kullanabilirsiniz. Desteklenen diller C#, Java, Node.js ve Python içerir.

Hizmet SDK 'Ları, masaüstü veya Web uygulaması gibi bir çözümden cihaz bilgilerine erişmenizi sağlar. Hizmet SDK 'Ları, model KIMLIĞINI almak için kullanabileceğiniz iki ad alanı ve nesne modeli içerir:

- IoT Hub hizmeti istemcisi. Bu hizmet, model KIMLIĞINI bir Device ikizi özelliği olarak kullanıma sunar.

- Dijital TWINS istemcisi. Yeni dijital TWINS API 'SI, bileşenler, Özellikler ve komutlar gibi [dijital TWINS tanım dili (DTDL)](concepts-digital-twin.md) model yapıları üzerinde çalışır. Dijital Ikizi API 'Leri, çözüm oluşturucuların IoT Tak ve Kullan çözümleri oluşturmalarına daha kolay hale getirir.

| Platform | Belgeler | Örnekler |
| -------- | ------------- | ------- |
| .NET     | [Başvuru](/dotnet/api/microsoft.azure.devices) | [Hizmet istemcisi](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/service/PnpServiceSamples) </br> [Digital Twins](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/service/DigitalTwinClientSamples) |
| Java     | [Başvuru](/java/api/com.microsoft.azure.sdk.iot.service.devicetwin.devicetwindevice?preserve-view=true&view=azure-java-stable) <br/> | [Hizmet istemcisi](https://github.com/Azure/azure-iot-sdk-java/blob/master/service/iot-service-samples/pnp-service-sample) </br>[Digital Twins](https://github.com/Azure/azure-iot-sdk-java/tree/master/service/iot-service-samples/digitaltwin-service-samples) |
| Node.js  | [Başvuru](/javascript/api/azure-iothub?preserve-view=true&view=azure-node-latest) | [Hizmet istemcisi](https://github.com/Azure/azure-iot-sdk-node/blob/master/service/samples/javascript/twin.js) </br> [Digital Twins](https://github.com/Azure/azure-iot-sdk-node/blob/master/service/samples/javascript/get_digital_twin.js) |
| Python   | [Başvuru](/python/api/azure-iot-hub/azure.iot.hub?preserve-view=true&view=azure-python) <br/> | [Hizmet istemcisi](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-hub/samples/iothub_registry_manager_method_sample.py) </br> [Digital Twins](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-hub/samples/get_digital_twin_sample.py) |

## <a name="iot-hub-service-client-examples"></a>IoT Hub hizmeti istemci örnekleri

Bu bölümde, IoT Hub Service Client ve **Registrymanager** ve **ServiceClient** sınıfları kullanılarak C# örnekleri gösterilmektedir. Cihaz TWINS 'i kullanarak cihaz durumuyla etkileşim kurmak için **Registrymanager** sınıfını kullanın. Ayrıca, IoT Hub [cihaz kayıtlarını sorgulamak](..\iot-hub\iot-hub-devguide-query-language.md) Için **registrymanager** sınıfını da kullanabilirsiniz. Cihazdaki komutları çağırmak için **ServiceClient** sınıfını kullanın. Cihazın [Dtdl](concepts-digital-twin.md) modeli, cihazın uyguladığı özellikleri ve komutları tanımlar. Kod parçacıklarında, `deviceTwinId` değişken IoT Hub 'ınıza kayıtlı olan ıot Tak ve kullan cihazının CIHAZ kimliğini barındırır.

### <a name="get-the-device-twin-and-model-id"></a>Cihaz ikizi ve model KIMLIĞINI al

IoT Hub 'ınıza bağlanan IoT Tak ve Kullan cihazının cihaz ikizi ve model KIMLIĞINI almak için:

```csharp
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(parameters.HubConnectionString);

Twin twin = await registryManager.GetTwinAsync(deviceTwinId);
Console.WriteLine($"Device twin: \n{JsonConvert.SerializeObject(twin, Formatting.Indented)}");
Console.WriteLine($"Model ID: {twin.ModelId}.");
```

### <a name="update-device-twin"></a>Cihaz ikizi Güncelleştir

Aşağıdaki kod parçacığı, `targetTemperature` bir cihazdaki özelliğinin nasıl güncelleştirilmesini gösterir. Örnek, ikizi ' i güncelleştirmeden önce nasıl almanız gerektiğini gösterir `ETag` . Özelliği, cihazın varsayılan bileşeninde tanımlanmıştır:

```csharp
Twin twin = await registryManager.GetTwinAsync(deviceTwinId);

int desiredTargetTemperature = 60;

// Update the twin
var twinPatch = new Twin();
twinPatch.Properties.Desired["targetTemperature"] = desiredTargetTemperature;

Console.WriteLine($"Update the targetTemperature property to {desiredTargetTemperature}.");

await registryManager.UpdateTwinAsync(deviceTwinId, twinPatch, twin.ETag);
```

Aşağıdaki kod parçacığında, `targetTemperature` bir bileşen üzerinde özelliğin nasıl güncelleştirilecek gösterilmektedir. Örnek, ikizi ' i güncelleştirmeden önce nasıl almanız gerektiğini gösterir `ETag` . Özelliği **Thermostat1** arabiriminde tanımlanmıştır:

```csharp
Twin twin = await registryManager.GetTwinAsync(deviceTwinId);

int desiredTargetTemperature = 60;

var twinPatch = CreatePropertyPatch("targetTemperature", desiredTargetTemperature, "thermostat1");

await registryManager.UpdateTwinAsync(deviceTwinId, twinPatch, twin.ETag);

// ...

private static Twin CreatePropertyPatch(string propertyName, object propertyValue, string componentName)
{
    var twinPatch = new Twin();
    twinPatch.Properties.Desired[componentName] = new
    {
        __t = "c"
    };
    twinPatch.Properties.Desired[componentName][propertyName] = JsonConvert.SerializeObject(propertyValue);
    return twinPatch;
}
```

Bir bileşenin özelliği için, özellik düzeltme eki aşağıdaki örneğe benzer şekilde görünür:

```json
{
"sampleComponentName":
  {
    "__t": "c",
    "samplePropertyName": 20
  }
}
```

### <a name="call-command"></a>Call komutu

Aşağıdaki kod parçacığı, `getMaxMinReport` varsayılan bir bileşende tanımlanan komutun nasıl çağıralınacağını gösterir:

```csharp
ServiceClient serviceClient = ServiceClient.CreateFromConnectionString(parameters.HubConnectionString);

var commandInvocation = new CloudToDeviceMethod("getMaxMinReport") { ResponseTimeout = TimeSpan.FromSeconds(30) };

// Set command payload
DateTimeOffset since = DateTimeOffset.Now.Subtract(TimeSpan.FromMinutes(2));
string componentCommandPayload = JsonConvert.SerializeObject(since);
commandInvocation.SetPayloadJson(componentCommandPayload);

try
{
  CloudToDeviceMethodResult result = await serviceClient.InvokeDeviceMethodAsync(deviceTwinId, commandInvocation);

  Console.WriteLine($"Command getMaxMinReport was invoked." +
      $"\nDevice returned status: {result.Status}. \nReport: {result.GetPayloadAsJson()}");
}
catch (DeviceNotFoundException)
{
    Console.WriteLine($"Unable to execute command getMaxMinReport on {deviceTwinId}.";
}
```

Aşağıdaki kod parçacığında, bir bileşende komutun nasıl çağrılacağını gösterir `getMaxMinReport` . Komut **Thermostat1** arabiriminde tanımlanmıştır:

```csharp
// Create command name to invoke for component. The command is formatted as <component name>*<command name>
string commandToInvoke = "thermostat1*getMaxMinReport";
var commandInvocation = new CloudToDeviceMethod(commandToInvoke) { ResponseTimeout = TimeSpan.FromSeconds(30) };

// Set command payload
DateTimeOffset since = DateTimeOffset.Now.Subtract(TimeSpan.FromMinutes(2));
string componentCommandPayload = JsonConvert.SerializeObject(since);
commandInvocation.SetPayloadJson(componentCommandPayload);

try
{
    CloudToDeviceMethodResult result = await serviceClient.InvokeDeviceMethodAsync(deviceTwinId, commandInvocation);
    Console.WriteLine($"Command getMaxMinReport was invoked on component thermostat1." +
        $"\nDevice returned status: {result.Status}. \nReport: {result.GetPayloadAsJson()}");
}
catch (DeviceNotFoundException)
{
    Console.WriteLine("Unable to execute command getMaxMinReport on component thermostat1.");
}
```

## <a name="iot-hub-digital-twin-examples"></a>IoT Hub Digital ikizi örnekleri

Dijital TWINS kullanarak cihaz durumuyla etkileşim kurmak için **Digitaltwınclient** sınıfını kullanırsınız. Cihazın [Dtdl](concepts-digital-twin.md) modeli, cihazın uyguladığı özellikleri ve komutları tanımlar.

Bu bölümde, dijital TWINS API 'sini kullanan C# örnekleri gösterilmektedir. Aşağıdaki kod parçacıkları, termostat ve sıcaklık denetleyicisi cihazlarının dijital ikizi temsil etmek için aşağıdaki sınıfları kullanır:

```csharp
using Microsoft.Azure.Devices.Serialization;
using Newtonsoft.Json;
using System;

namespace Microsoft.Azure.Devices.Samples
{
  internal class ThermostatTwin : BasicDigitalTwin
  {
    [JsonProperty("$metadata")]
    public new ThermostatMetadata Metadata { get; set; }

    [JsonProperty("maxTempSinceLastReboot")]
    public double? MaxTempSinceLastReboot { get; set; }

    [JsonProperty("targetTemperature")]
    public double? TargetTemperature { get; set; }
  }

  internal class ThermostatMetadata : DigitalTwinMetadata
  {
    [JsonProperty("maxTempSinceLastReboot")]
    public ReportedPropertyMetadata MaxTempSinceLastReboot { get; set; }

    [JsonProperty("targetTemperature")]
    public WritableProperty TargetTemperature { get; set; }
  }

  internal class ReportedPropertyMetadata
  {
    [JsonProperty("lastUpdateTime")]
    public DateTimeOffset LastUpdateTime { get; set; }
  }

  internal class TemperatureControllerTwin : BasicDigitalTwin
  {
    [JsonProperty("$metadata")]
    public new TemperatureControllerMetadata Metadata { get; set; }

    [JsonProperty("serialNumber")]
    public string SerialNumber { get; set; }

    [JsonProperty("thermostat1")]
    public ThermostatTwin Thermostat1 { get; set; }

    [JsonProperty("thermostat2")]
    public ThermostatTwin Thermostat2 { get; set; }
  }

  internal class TemperatureControllerMetadata : DigitalTwinMetadata
  {
    [JsonProperty("serialNumber")]
    public ReportedPropertyMetadata SerialNumber { get; set; }

    [JsonProperty("thermostat1")]
    public WritableProperty Thermostat1 { get; set; }

    [JsonProperty("thermostat2")]
    public WritableProperty Thermostat2 { get; set; }
  }
}
```

`digitalTwinId`Değişkeni, IoT Hub 'ınıza kayıtlı olan ıot Tak ve kullan cihazının CIHAZ kimliğini içerir.

### <a name="get-the-digital-twin-and-model-id"></a>Dijital ikizi ve model KIMLIĞINI alın

IoT Hub 'ınıza bağlanan IoT Tak ve Kullan cihazının dijital ikizi ve model KIMLIĞINI almak için:

```csharp
DigitalTwinClient digitalTwinClient = DigitalTwinClient.CreateFromConnectionString(parameters.HubConnectionString);
HttpOperationResponse<ThermostatTwin, DigitalTwinGetHeaders> getDigitalTwinResponse = await digitalTwinClient
    .GetDigitalTwinAsync<ThermostatTwin>(digitalTwinId);
ThermostatTwin thermostatTwin = getDigitalTwinResponse.Body;
Console.WriteLine($"Model ID: {thermostatTwin.Metadata.ModelId}.");
Console.WriteLine($"Digital Twin: \n{JsonConvert.SerializeObject(thermostatTwin, Formatting.Indented)}");
```

### <a name="update-digital-twin"></a>Dijital ikizi Güncelleştir

Aşağıdaki kod parçacığı, `targetTemperature` bir cihazdaki özelliğinin nasıl güncelleştirilmesini gösterir. Özelliği, cihazın varsayılan bileşeninde tanımlanmıştır:

```csharp
var updateOperation = new UpdateOperationsUtility();

int desiredTargetTemperature = 60;

// Get the current value of the targetTemperature property
HttpOperationResponse<ThermostatTwin, DigitalTwinGetHeaders> getDigitalTwinResponse = await digitalTwinClient
    .GetDigitalTwinAsync<ThermostatTwin>(digitalTwinId);
double? currentTargetTemperature = getDigitalTwinResponse.Body.TargetTemperature;

// Has the targetTemperature property previously been set?
if (currentTargetTemperature != null)
{
  // Update the existing property
  // Prepend the property path with a '/'
  updateOperation.AppendReplacePropertyOp($"/targetTemperature", desiredTargetTemperature);
}
else
{
  // Add a new property
  // Prepend the property path with a '/'
  updateOperation.AppendAddPropertyOp($"/targetTemperature", desiredTargetTemperature);
}

// Update the targetTemperature property on the digital twin
HttpOperationHeaderResponse<DigitalTwinUpdateHeaders> updateDigitalTwinResponse = await digitalTwinClient
    .UpdateDigitalTwinAsync(digitalTwinId, updateOperation.Serialize());

Console.WriteLine($"Update {digitalTwinId} digital twin response: {updateDigitalTwinResponse.Response.StatusCode}.");
```

Aşağıdaki kod parçacığında, `targetTemperature` bir bileşen üzerinde özelliğin nasıl güncelleştirilecek gösterilmektedir. Özelliği **Thermostat1** bileşeninde tanımlanmıştır:

```csharp
int desiredTargetTemperature = 60;

var updateOperation = new UpdateOperationsUtility();

// Look at when the property was updated and what was it set to.
HttpOperationResponse<TemperatureControllerTwin, DigitalTwinGetHeaders> getDigitalTwinResponse = await digitalTwinClient
  .GetDigitalTwinAsync<TemperatureControllerTwin>(digitalTwinId);

ThermostatTwin thermostat1 = getDigitalTwinResponse.Body.Thermostat1;

if (thermostat1 != null)
{
  // Thermostat1 is present in the TemperatureController twin. You can add/replace the component-level property "targetTemperature"
  double? currentComponentTargetTemperature = getDigitalTwinResponse.Body.Thermostat1.TargetTemperature;
  if (currentComponentTargetTemperature != null)
  {
      DateTimeOffset targetTemperatureDesiredLastUpdateTime = getDigitalTwinResponse.Body.Thermostat1.Metadata.TargetTemperature.LastUpdateTime;

      // The property path to be replaced should be prepended with a '/'
      updateOperation.AppendReplacePropertyOp("/thermostat1/targetTemperature", desiredTargetTemperature);
  }
  else
  {
      // The property path to be added should be prepended with a '/'
      updateOperation.AppendAddPropertyOp("/thermostat1/targetTemperature", desiredTargetTemperature);
  }
}
else
{
    // Thermostat1 is not present in the TemperatureController twin. Add the component.
    var componentProperty = new Dictionary<string, object> { { "targetTemperature", desiredTargetTemperature }, { "$metadata", new object() } };

    // The property path to be replaced should be prepended with a '/'
    updateOperation.AppendAddComponentOp("/thermostat1", componentProperty);
}

HttpOperationHeaderResponse<DigitalTwinUpdateHeaders> updateDigitalTwinResponse = await digitalTwinClient
    .UpdateDigitalTwinAsync(digitalTwinId, updateOperation.Serialize());

Console.WriteLine($"Update {digitalTwinId} digital twin response: {updateDigitalTwinResponse.Response.StatusCode}.");
```

### <a name="call-command"></a>Call komutu

Aşağıdaki kod parçacığı, `getMaxMinReport` varsayılan bir bileşende tanımlanan komutun nasıl çağıralınacağını gösterir:

```csharp
DateTimeOffset since = DateTimeOffset.Now.Subtract(TimeSpan.FromMinutes(2));

try
{
  HttpOperationResponse<DigitalTwinCommandResponse, DigitalTwinInvokeCommandHeaders> invokeCommandResponse = await digitalTwinClient
    .InvokeCommandAsync(digitalTwinId, "getMaxMinReport", JsonConvert.SerializeObject(since));

  Console.WriteLine($"Command getMaxMinReport was invoked. \nDevice returned status: {invokeCommandResponse.Body.Status}." +
    $"\nReport: {invokeCommandResponse.Body.Payload}");
}
catch (HttpOperationException e)
{
  if (e.Response.StatusCode == HttpStatusCode.NotFound)
  {
    Console.WriteLine($"Unable to execute command getMaxMinReport on {digitalTwinId}.");
  }
}
```

Aşağıdaki kod parçacığında, bir bileşende komutun nasıl çağrılacağını gösterir `getMaxMinReport` . Komut **Thermostat1** arabiriminde tanımlanmıştır:

```csharp
DateTimeOffset since = DateTimeOffset.Now.Subtract(TimeSpan.FromMinutes(2));

try
{
    HttpOperationResponse<DigitalTwinCommandResponse, DigitalTwinInvokeCommandHeaders> invokeCommandResponse = await digitalTwinClient
        .InvokeComponentCommandAsync(digitalTwinId, "thermostat1", "getMaxMinReport", JsonConvert.SerializeObject(since));

    Console.WriteLine("Command getMaxMinReport was invoked on component thermostat1." +
        $"\nDevice returned status: {invokeCommandResponse.Body.Status}. \nReport: {invokeCommandResponse.Body.Payload}");
}
catch (HttpOperationException e)
{
    if (e.Response.StatusCode == HttpStatusCode.NotFound)
    {
        Console.WriteLine("Unable to execute command getMaxMinReport on component thermostat1.");
    }
}
```

## <a name="read-device-telemetry"></a>Cihaz telemetrisini okuma

IoT Tak ve Kullan cihazları DTDL modelinde tanımlanan telemetrileri IoT Hub gönderir. Varsayılan olarak, IoT Hub Telemetriyi kullanabileceğiniz bir Event Hubs uç noktasına yönlendirir. Daha fazla bilgi edinmek için bkz. [farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](../iot-hub/iot-hub-devguide-messages-d2c.md).

Aşağıdaki kod parçacığı, varsayılan Event Hubs uç noktasından Telemetriyi okumayı gösterir. Bu kod parçacığındaki kod, IoT Hub hızlı başlangıçta [bir cihazdan IoT Hub 'ına telemetri gönderme ve arka uç uygulamasıyla okuma](../iot-hub/quickstart-send-telemetry-dotnet.md)işlemi gerçekleştirmektir:

```csharp
await using EventHubConsumerClient consumer = new EventHubConsumerClient(EventHubConsumerClient.DefaultConsumerGroupName, connectionString, EventHubName);

Console.WriteLine("Listening for messages on all partitions");

try
{
    await foreach (PartitionEvent partitionEvent in consumer.ReadEventsAsync(cancellationToken))
    {
        Console.WriteLine("Message received on partition {0}:", partitionEvent.Partition.PartitionId);

        string data = Encoding.UTF8.GetString(partitionEvent.Data.Body.ToArray());
        Console.WriteLine("\t{0}:", data);

        Console.WriteLine("Application properties (set by device):");
        foreach (var prop in partitionEvent.Data.Properties)
        {
            Console.WriteLine("\t{0}: {1}", prop.Key, prop.Value);
        }

        Console.WriteLine("System properties (set by IoT Hub):");
        foreach (var prop in partitionEvent.Data.SystemProperties)
        {
            Console.WriteLine("\t{0}: {1}", prop.Key, prop.Value);
        }
    }
}
catch (TaskCanceledException)
{
    // This is expected when the token is signaled; it should not be considered an
    // error in this scenario.
}
```

Önceki koddan bulunan aşağıdaki çıktıda, bileşen olmayan **termostat** IoT Tak ve kullan cihazının yalnızca varsayılan bileşene sahip olduğu sıcaklık telemetrisi gösterilmektedir. `dt-dataschema`System özelliği, model kimliğini gösterir:

```cmd/sh
Message received on partition 1:
        { "temperature": 25.5 }:
Application properties (set by device):
System properties (set by IoT Hub):
        iothub-connection-device-id: my-pnp-device
        iothub-connection-auth-method: {"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}
        iothub-connection-auth-generation-id: 637375045610235418
        iothub-enqueuedtime: 05/10/2020 14:30:58
        iothub-message-source: Telemetry
        dt-dataschema: dtmi:com:example:Thermostat;1
        content-type: application/json
        content-encoding: utf-8
```

Önceki koddan bulunan aşağıdaki çıktıda, çok bileşen **TemperatureController** IoT Tak ve kullan cihazının gönderdiği sıcaklık telemetrisi gösterilmektedir. `dt-subject`System özelliği, Telemetriyi gönderen bileşenin adını gösterir. Bu örnekte, iki bileşen `thermostat1` ve `thermostat2` dtdl modelinde tanımlanmıştır. `dt-dataschema`System özelliği, model kimliğini gösterir:

```cmd/sh
Message received on partition 1:
        {"temperature":11.1}:
Application properties (set by device):
System properties (set by IoT Hub):
        dt-subject: thermostat1
        iothub-connection-device-id: my-pnp-device
        iothub-connection-auth-method: {"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}
        iothub-connection-auth-generation-id: 637375045610235418
        iothub-enqueuedtime: 05/10/2020 14:23:36
        iothub-message-source: Telemetry
        dt-dataschema: dtmi:com:example:TemperatureController;1
        content-type: application/json
        content-encoding: utf-8
Message received on partition 1:
        {"temperature":41.2}:
Application properties (set by device):
System properties (set by IoT Hub):
        dt-subject: thermostat2
        iothub-connection-device-id: my-pnp-device
        iothub-connection-auth-method: {"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}
        iothub-connection-auth-generation-id: 637375045610235418
        iothub-enqueuedtime: 05/10/2020 14:23:36
        iothub-message-source: Telemetry
        dt-dataschema: dtmi:com:example:TemperatureController;1
        content-type: application/json
        content-encoding: utf-8
```

## <a name="read-device-twin-change-notifications"></a>Cihaz ikizi değişiklik bildirimlerini oku

Desteklenen bir uç noktaya yönlendirmek üzere cihaz ikizi değişiklik bildirimleri oluşturmak için IoT Hub yapılandırabilirsiniz. Daha fazla bilgi edinmek için bkz. [IoT Hub ileti yönlendirmeyi kullanarak, telemetri olmayan olaylar > farklı uç noktalara cihazdan buluta iletiler gönderme](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Önceki C# kod parçacığında gösterilen kod, bileşen olmayan bir termostat cihazı için IoT Hub cihaz ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Message received on partition 1:
        {"version":3,"properties":{"reported":{"maxTempSinceLastReboot":9.6,"$metadata":{"$lastUpdated":"2020-10-06T10:17:41.7408552Z","maxTempSinceLastReboot":{"$lastUpdated":"2020-10-06T10:17:41.7408552Z"}},"$version":2}}}:
Application properties (set by device):
        hubName: my-pnp-hub
        deviceId: my-pnp-device
        operationTimestamp: 2020-10-06T10:17:41.7408552+00:00
        iothub-message-schema: twinChangeNotification
        opType: updateTwin
System properties (set by IoT Hub):
        iothub-connection-device-id: my-pnp-device
        iothub-enqueuedtime: 06/10/2020 10:17:41
        iothub-message-source: twinChangeEvents
        user-id: System.ArraySegment`1[System.Byte]
        correlation-id: 61394e8ba7d
        content-type: application/json
        content-encoding: utf-8
```

Önceki C# kod parçacığında gösterilen kod, bileşenler içeren bir cihaz için cihaz ikizi değişiklik bildirimleri IoT Hub oluşturduğunda aşağıdaki çıktıyı üretir. Bu örnek, bir termostat bileşeni olan bir sıcaklık algılayıcısı cihazı bildirim oluşturduğunda çıktıyı gösterir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Message received on partition 1:
        {"version":5,"properties":{"reported":{"thermostat1":{"__t":"c","maxTempSinceLastReboot":9.6},"$metadata":{"$lastUpdated":"2020-10-06T10:27:59.515972Z","thermostat1":{"$lastUpdated":"2020-10-06T10:27:59.515972Z","__t":{"$lastUpdated":"2020-10-06T10:27:59.515972Z"},"maxTempSinceLastReboot":{"$lastUpdated":"2020-10-06T10:27:59.515972Z"}}},"$version":4}}}:
Application properties (set by device):
        hubName: my-pnp-hub
        deviceId: my-pnp-device
        operationTimestamp: 2020-10-06T10:27:59.5159720+00:00
        iothub-message-schema: twinChangeNotification
        opType: updateTwin
System properties (set by IoT Hub):
        iothub-connection-device-id: my-pnp-device
        iothub-enqueuedtime: 06/10/2020 10:27:59
        iothub-message-source: twinChangeEvents
        user-id: System.ArraySegment`1[System.Byte]
        correlation-id: 615051f364e
        content-type: application/json
        content-encoding: utf-8
```

## <a name="read-digital-twin-change-notifications"></a>Dijital ikizi değişiklik bildirimlerini oku

Desteklenen bir uç noktaya yönlendirmek için dijital ikizi değişiklik bildirimleri oluşturacak şekilde IoT Hub yapılandırabilirsiniz. Daha fazla bilgi edinmek için bkz. [IoT Hub ileti yönlendirmeyi kullanarak, telemetri olmayan olaylar > farklı uç noktalara cihazdan buluta iletiler gönderme](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Önceki C# kod parçacığında gösterilen kod, IoT Hub bileşen olmayan bir termostat cihazı için dijital ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Message received on partition 1:
        [{"op":"add","path":"/$metadata/maxTempSinceLastReboot","value":{"lastUpdateTime":"2020-10-06T10:39:16.0209836Z"}},{"op":"add","path":"/maxTempSinceLastReboot","value":34.9}]:
Application properties (set by device):
        hubName: my-pnp-hub
        deviceId: my-pnp-device
        operationTimestamp: 2020-10-06T10:39:16.0209836+00:00
        iothub-message-schema: digitalTwinChangeNotification
        opType: updateTwin
System properties (set by IoT Hub):
        iothub-connection-device-id: my-pnp-device
        iothub-enqueuedtime: 06/10/2020 10:39:16
        iothub-message-source: digitalTwinChangeEvents
        user-id: System.ArraySegment`1[System.Byte]
        correlation-id: 6169857bf8c
        content-type: application/json-patch+json
        content-encoding: utf-8
```

Önceki C# kod parçacığında gösterilen kod, IoT Hub bileşenleri olan bir cihaz için dijital ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Bu örnek, bir termostat bileşeni olan bir sıcaklık algılayıcısı cihazı bildirim oluşturduğunda çıktıyı gösterir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Message received on partition 1:
        [{"op":"add","path":"/thermostat1","value":{"$metadata":{"maxTempSinceLastReboot":{"lastUpdateTime":"2020-10-06T10:41:44.8312666Z"}},"maxTempSinceLastReboot":29.1}}]:
Application properties (set by device):
        hubName: my-pnp-hub
        deviceId: my-pnp-device
        operationTimestamp: 2020-10-06T10:41:44.8312666+00:00
        iothub-message-schema: digitalTwinChangeNotification
        opType: updateTwin
System properties (set by IoT Hub):
        iothub-connection-device-id: my-pnp-device
        iothub-enqueuedtime: 06/10/2020 10:41:44
        iothub-message-source: digitalTwinChangeEvents
        user-id: System.ArraySegment`1[System.Byte]
        correlation-id: 616f108f0e3
        content-type: application/json-patch+json
        content-encoding: utf-8
```

## <a name="next-steps"></a>Sonraki adımlar

Artık cihaz modelleme hakkında bilgi edindiğinize göre, bazı ek kaynaklar aşağıda verilmiştir:

- [Dijital TWINS tanım dili (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [C cihaz SDK’sı](/azure/iot-hub/iot-c-sdk-ref/)
- [IoT REST API](/rest/api/iothub/device)
- [Model bileşenleri](./concepts-components.md)
- [DTDL yazma araçlarını yükle ve kullan](howto-use-dtdl-authoring-tools.md)