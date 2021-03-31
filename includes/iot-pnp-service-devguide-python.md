---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 10/20/2020
ms.openlocfilehash: a925c3a17988ef6f4b95a1e3cf4dd5fb8baa4829
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "102510721"
---
Aşağıdaki kaynaklar da kullanılabilir:

- [Python SDK'sı başvuru belgeleri](/python/api/azure-iot-hub/azure.iot.hub)
- [Hizmet istemcisi örnekleri](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-hub/samples/iothub_registry_manager_method_sample.py)
- [Dijital TWINS örnekleri](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-hub/samples/get_digital_twin_sample.py)

## <a name="iot-hub-service-client-examples"></a>IoT Hub hizmeti istemci örnekleri

Bu bölümde, IoT Hub Service Client ve **Iothubregistrymanager** ve **Cloudtodevicemethod** sınıfları kullanılarak Python örnekleri gösterilmektedir. Device TWINS kullanarak cihaz durumuyla etkileşim kurmak için **Iothubregistrymanager** sınıfını kullanırsınız. Ayrıca, IoT Hub [cihaz kayıtlarını sorgulamak](../articles/iot-hub/iot-hub-devguide-query-language.md) Için **Iothubregistrymanager** sınıfını da kullanabilirsiniz. Cihazdaki komutları çağırmak için **Cloudtodevicemethod** sınıfını kullanın. Cihazın [Dtdl](../articles/iot-pnp/concepts-digital-twin.md) modeli, cihazın uyguladığı özellikleri ve komutları tanımlar. Kod parçacıklarında, `device_id` değişken IoT Hub 'ınıza kayıtlı olan ıot Tak ve kullan cihazının CIHAZ kimliğini barındırır.

### <a name="get-the-device-twin-and-model-id"></a>Cihaz ikizi ve model KIMLIĞINI al

IoT Hub 'ınıza bağlanan IoT Tak ve Kullan cihazının cihaz ikizi ve model KIMLIĞINI almak için:

```python
from azure.iot.hub import IoTHubRegistryManager
from azure.iot.hub.models import Twin, TwinProperties

iothub_registry_manager = IoTHubRegistryManager(iothub_connection_str)

# ...

twin = iothub_registry_manager.get_twin(device_id)
print("The device twin is: ")
print("")
print(twin)
print("")

additional_props = twin.additional_properties
if "modelId" in additional_props:
    print("The Model ID for this device is:")
    print(additional_props["modelId"])
    print("")
```

### <a name="update-device-twin"></a>Cihaz ikizi Güncelleştir

Aşağıdaki kod parçacığı, `targetTemperature` bir cihazdaki özelliğinin nasıl güncelleştirilmesini gösterir. Örnek, ikizi ' i güncelleştirmeden önce nasıl almanız gerektiğini gösterir `etag` . Özelliği, cihazın varsayılan bileşeninde tanımlanmıştır:

```python
iothub_registry_manager = IoTHubRegistryManager(iothub_connection_str)

twin = iothub_registry_manager.get_twin(device_id)

twin_patch = Twin()

twin_patch.properties = TwinProperties(
    desired={"targetTemperature": 42}
)
updated_twin = iothub_registry_manager.update_twin(device_id, twin_patch, twin.etag)
```

Aşağıdaki kod parçacığında, `targetTemperature` bir bileşen üzerinde özelliğin nasıl güncelleştirilecek gösterilmektedir. Örnek, ikizi ' i güncelleştirmeden önce nasıl almanız gerektiğini gösterir `ETag` . Özelliği **thermostat1** bileşeninde tanımlanmıştır:

```python
iothub_registry_manager = IoTHubRegistryManager(iothub_connection_str)

twin = iothub_registry_manager.get_twin(device_id)

twin_patch = Twin()

twin_patch.properties = TwinProperties(
    desired={ "thermostat1": {
        "__t": "c",
        "targetTemperature": 42}
    }
)
updated_twin = iothub_registry_manager.update_twin(device_id, twin_patch, twin.etag)
```

Bir bileşenin özelliği için, özellik düzeltme eki aşağıdaki örneğe benzer şekilde görünür:

```json
{
"thermostat1":
  {
    "__t": "c",
    "targetTemperature": 20
  }
}
```

### <a name="call-command"></a>Call komutu

Aşağıdaki kod parçacığı, `getMaxMinReport` varsayılan bir bileşende tanımlanan komutun nasıl çağıralınacağını gösterir:

```python
from azure.iot.hub import IoTHubRegistryManager
from azure.iot.hub.models import CloudToDeviceMethod

# ...

iothub_registry_manager = IoTHubRegistryManager(iothub_connection_str)

method_payload = datetime.datetime.now() - datetime.timedelta(minutes=2)
device_method = CloudToDeviceMethod(method_name="getMaxMinReport", payload=method_payload)
result = iothub_registry_manager.invoke_device_method(device_id, device_method)
print(result.payload)
```

Aşağıdaki kod parçacığında, bir bileşende komutun nasıl çağrılacağını gösterir `getMaxMinReport` . Komut **thermostat1** bileşeninde tanımlanmıştır:

```python
from azure.iot.hub import IoTHubRegistryManager
from azure.iot.hub.models import CloudToDeviceMethod

# ...

iothub_registry_manager = IoTHubRegistryManager(iothub_connection_str)

method_payload = datetime.datetime.now() - datetime.timedelta(minutes=2)
device_method = CloudToDeviceMethod(method_name="thermostat1*getMaxMinReport", payload=method_payload)
result = iothub_registry_manager.invoke_device_method(device_id, device_method)
print(result.payload)
```

## <a name="iot-hub-digital-twin-examples"></a>IoT Hub Digital ikizi örnekleri

Dijital TWINS kullanarak cihaz durumuyla etkileşim kurmak için **Digitaltwınclient** sınıfını kullanırsınız. Cihazın [Dtdl](../articles/iot-pnp/concepts-digital-twin.md) modeli, cihazın uyguladığı özellikleri ve komutları tanımlar.

`device_id`Değişkeni, IoT Hub 'ınıza kayıtlı olan ıot Tak ve kullan cihazının CIHAZ kimliğini içerir.

### <a name="get-the-digital-twin-and-model-id"></a>Dijital ikizi ve model KIMLIĞINI alın

IoT Hub 'ınıza bağlanan IoT Tak ve Kullan cihazının dijital ikizi ve model KIMLIĞINI almak için:

```python
from azure.iot.hub import DigitalTwinClient

digital_twin_client = DigitalTwinClient(iothub_connection_str)

digital_twin = digital_twin_client.get_digital_twin(device_id)
if digital_twin:
    print(digital_twin)
    print("Model Id: " + digital_twin["$metadata"]["$model"])
else:
    print("No digital_twin found")
```

### <a name="update-digital-twin"></a>Dijital ikizi Güncelleştir

Aşağıdaki kod parçacığı, `targetTemperature` bir cihazdaki özelliğinin nasıl güncelleştirilmesini gösterir. Özelliği, cihazın varsayılan bileşeninde tanımlanmıştır:

```python
from azure.iot.hub import DigitalTwinClient

digital_twin_client = DigitalTwinClient(iothub_connection_str)

patch = [{"op": "add", "path": "/targetTemperature", "value": 42}]
digital_twin_client.update_digital_twin(device_id, patch)
```

Aşağıdaki kod parçacığında, `targetTemperature` bir bileşen üzerinde özelliğin nasıl güncelleştirilecek gösterilmektedir. Özelliği **thermostat1** bileşeninde tanımlanmıştır:

```python
from azure.iot.hub import DigitalTwinClient

digital_twin_client = DigitalTwinClient(iothub_connection_str)

patch = [{"op": "add", "path": "/targetTemperature", "value": 42}]
digital_twin_client.update_digital_twin(device_id, patch)
```

### <a name="call-command"></a>Call komutu

Aşağıdaki kod parçacığı, `getMaxMinReport` varsayılan bir bileşende tanımlanan komutun nasıl çağıralınacağını gösterir:

```python
from azure.iot.hub import DigitalTwinClient

payload = datetime.datetime.now() - datetime.timedelta(minutes=2)

connect_timeout_in_seconds = 3
response_timeout_in_seconds = 7


digital_twin_client = DigitalTwinClient(iothub_connection_str)

invoke_command_result = digital_twin_client.invoke_command(
    device_id, "getMaxMinReport", payload, connect_timeout_in_seconds, response_timeout_in_seconds
)
if invoke_command_result:
    print(invoke_command_result)
else:
    print("No invoke_command_result found")
```

Aşağıdaki kod parçacığında, bir bileşende komutun nasıl çağrılacağını gösterir `getMaxMinReport` . Komut **thermostat1** bileşeninde tanımlanmıştır:

```python
from azure.iot.hub import DigitalTwinClient

payload = datetime.datetime.now() - datetime.timedelta(minutes=2)

connect_timeout_in_seconds = 3
response_timeout_in_seconds = 7


digital_twin_client = DigitalTwinClient(iothub_connection_str)

invoke_command_result = digital_twin_client.invoke_component_command(
    device_id, "thermostat1", "getMaxMinReport", payload, connect_timeout_in_seconds, response_timeout_in_seconds
)
if invoke_command_result:
    print(invoke_command_result)
else:
    print("No invoke_command_result found")
```

## <a name="read-device-telemetry"></a>Cihaz telemetrisini okuma

IoT Tak ve Kullan cihazları DTDL modelinde tanımlanan telemetrileri IoT Hub gönderir. Varsayılan olarak, IoT Hub Telemetriyi kullanabileceğiniz bir Event Hubs uç noktasına yönlendirir. Daha fazla bilgi edinmek için bkz. [farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](../articles/iot-hub/iot-hub-devguide-messages-d2c.md).

Aşağıdaki kod parçacığı, varsayılan Event Hubs uç noktasından Telemetriyi okumayı gösterir. Bu kod parçacığındaki kod, IoT Hub hızlı başlangıçta [bir cihazdan IoT Hub 'ına telemetri gönderme ve arka uç uygulamasıyla okuma](../articles/iot-hub/quickstart-send-telemetry-python.md)işlemi gerçekleştirmektir:

```python
import asyncio
from azure.eventhub import TransportType
from azure.eventhub.aio import EventHubConsumerClient

# Define callbacks to process events
async def on_event_batch(partition_context, events):
    for event in events:
        print("Received event from partition: {}.".format(partition_context.partition_id))
        print("Telemetry received: ", event.body_as_str())
        print("Properties (set by device): ", event.properties)
        print("System properties (set by IoT Hub): ", event.system_properties)
        print()
    await partition_context.update_checkpoint()

async def on_error(partition_context, error):
    # ...

loop = asyncio.get_event_loop()
client = EventHubConsumerClient.from_connection_string(
    conn_str=CONNECTION_STR,
    consumer_group="$default",
)

try:
    loop.run_until_complete(client.receive_batch(on_event_batch=on_event_batch, on_error=on_error))
except KeyboardInterrupt:
    print("Receiving has stopped.")
finally:
    loop.run_until_complete(client.close())
    loop.stop()
```

Önceki koddan bulunan aşağıdaki çıktıda, bileşen olmayan **termostat** IoT Tak ve kullan cihazının yalnızca varsayılan bileşene sahip olduğu sıcaklık telemetrisi gösterilmektedir. `dt-dataschema`System özelliği, model kimliğini gösterir:

```cmd/sh
Received event from partition: 1.
Telemetry received:  {"temperature": 12}
Properties (set by device):  None
System properties (set by IoT Hub):  {b'content-type': b'application/json', b'content-encoding': b'utf-8', b'iothub-connection-device-id': b'my-pnp-device', b'iothub-connection-auth-method': b'{"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}', b'iothub-connection-auth-generation-id': b'637388855582764406', b'iothub-enqueuedtime': 1603288810715, b'iothub-message-source': b'Telemetry', b'dt-dataschema': b'dtmi:com:example:Thermostat;1', b'x-opt-sequence-number': 13280, b'x-opt-offset': b'12890070640', b'x-opt-enqueued-time': 1603288810824}
```

Önceki koddan bulunan aşağıdaki çıktıda, çok bileşen **TemperatureController** IoT Tak ve kullan cihazının gönderdiği sıcaklık telemetrisi gösterilmektedir. `dt-subject`System özelliği, Telemetriyi gönderen bileşenin adını gösterir. Bu örnekte, iki bileşen `thermostat1` ve `thermostat2` dtdl modelinde tanımlanmıştır. `dt-dataschema`System özelliği, model kimliğini gösterir:

```cmd/sh
Received event from partition: 1.
Telemetry received:  {"temperature": 45}
Properties (set by device):  None
System properties (set by IoT Hub):  {b'content-type': b'application/json', b'content-encoding': b'utf-8', b'iothub-connection-device-id': b'my-pnp-device', b'iothub-connection-auth-method': b'{"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}', b'iothub-connection-auth-generation-id': b'637388858939631652', b'iothub-enqueuedtime': 1603289127844, b'iothub-message-source': b'Telemetry', b'dt-subject': b'thermostat1', b'dt-dataschema': b'dtmi:com:example:TemperatureController;1', b'x-opt-sequence-number': 13328, b'x-opt-offset': b'12890095440', b'x-opt-enqueued-time': 1603289128001}

Received event from partition: 1.
Telemetry received:  {"temperature": 49}
Properties (set by device):  None
System properties (set by IoT Hub):  {b'content-type': b'application/json', b'content-encoding': b'utf-8', b'iothub-connection-device-id': b'my-pnp-device', b'iothub-connection-auth-method': b'{"scope":"device","type":"sas","issuer":"iothub","acceptingIpFilterRule":null}', b'iothub-connection-auth-generation-id': b'637388858939631652', b'iothub-enqueuedtime': 1603289133017, b'iothub-message-source': b'Telemetry', b'dt-subject': b'thermostat2', b'dt-dataschema': b'dtmi:com:example:TemperatureController;1', b'x-opt-sequence-number': 13329, b'x-opt-offset': b'12890095928', b'x-opt-enqueued-time': 1603289133173}
```

## <a name="read-device-twin-change-notifications"></a>Cihaz ikizi değişiklik bildirimlerini oku

Desteklenen bir uç noktaya yönlendirmek üzere cihaz ikizi değişiklik bildirimleri oluşturmak için IoT Hub yapılandırabilirsiniz. Daha fazla bilgi edinmek için bkz. [IoT Hub ileti yönlendirmeyi kullanarak, telemetri olmayan olaylar > farklı uç noktalara cihazdan buluta iletiler gönderme](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Önceki Python kod parçacığında gösterilen kod, bileşen olmayan bir termostat cihazı için IoT Hub cihaz ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Received event from partition: 1.
Telemetry received:  {"version":3,"properties":{"reported":{"maxTempSinceLastReboot":10.96,"$metadata":{"$lastUpdated":"2020-10-21T14:10:42.4171263Z","maxTempSinceLastReboot":{"$lastUpdated":"2020-10-21T14:10:42.4171263Z"}},"$version":2}}}
Properties (set by device):  {b'hubName': b'my-pnp-hub', b'deviceId': b'my-pnp-device', b'operationTimestamp': b'2020-10-21T14:10:42.4171263+00:00', b'iothub-message-schema': b'twinChangeNotification', b'opType': b'updateTwin'}
System properties (set by IoT Hub):  {b'user-id': b'my-pnp-hub\x81\x0e\xa4\x7f', b'correlation-id': b'12104ced5402', b'content-type': b'application/json', b'content-encoding': b'utf-8', b'iothub-connection-device-id': b'my-pnp-device', b'iothub-enqueuedtime': 1603289442519, b'iothub-message-source': b'twinChangeEvents', b'x-opt-sequence-number': 13332, b'x-opt-offset': b'12890097392', b'x-opt-enqueued-time': 1603289442738}
```

Önceki Python kod parçacığında gösterilen kod, bileşenler içeren bir cihaz için cihaz ikizi değişiklik bildirimleri IoT Hub oluşturduğunda aşağıdaki çıktıyı üretir. Bu örnek, bir termostat bileşeni olan bir sıcaklık algılayıcısı cihazı bildirim oluşturduğunda çıktıyı gösterir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Received event from partition: 1.
Telemetry received:  {"version":4,"properties":{"reported":{"thermostat1":{"maxTempSinceLastReboot":98.34,"__t":"c"},"$metadata":{"$lastUpdated":"2020-10-21T14:13:39.36491Z","thermostat1":{"$lastUpdated":"2020-10-21T14:13:39.36491Z","maxTempSinceLastReboot":{"$lastUpdated":"2020-10-21T14:13:39.36491Z"},"__t":{"$lastUpdated":"2020-10-21T14:13:39.36491Z"}}},"$version":3}}}
Properties (set by device):  {b'hubName': b'my-pnp-hub', b'deviceId': b'my-pnp-device', b'operationTimestamp': b'2020-10-21T14:13:39.3649100+00:00', b'iothub-message-schema': b'twinChangeNotification', b'opType': b'updateTwin'}
System properties (set by IoT Hub):  {b'user-id': b'my-pnp-hub', b'correlation-id': b'1210b664ab83', b'content-type': b'application/json', b'content-encoding': b'utf-8', b'iothub-connection-device-id': b'my-pnp-device', b'iothub-enqueuedtime': 1603289619481, b'iothub-message-source': b'twinChangeEvents', b'x-opt-sequence-number': 13341, b'x-opt-offset': b'12890102216', b'x-opt-enqueued-time': 1603289619668}
```

## <a name="read-digital-twin-change-notifications"></a>Dijital ikizi değişiklik bildirimlerini oku

Desteklenen bir uç noktaya yönlendirmek için dijital ikizi değişiklik bildirimleri oluşturacak şekilde IoT Hub yapılandırabilirsiniz. Daha fazla bilgi edinmek için bkz. [IoT Hub ileti yönlendirmeyi kullanarak, telemetri olmayan olaylar > farklı uç noktalara cihazdan buluta iletiler gönderme](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events).

Önceki Python kod parçacığında gösterilen kod, IoT Hub bileşen olmayan bir termostat cihazı için dijital ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Received event from partition: 1.
Telemetry received:  [{"op":"add","path":"/$metadata/maxTempSinceLastReboot","value":{"lastUpdateTime":"2020-10-21T14:10:42.4171263Z"}},{"op":"add","path":"/maxTempSinceLastReboot","value":10.96}]
Properties (set by device):  {b'hubName': b'my-pnp-hub', b'deviceId': b'my-pnp-device', b'operationTimestamp': b'2020-10-21T14:10:42.4171263+00:00', b'iothub-message-schema': b'digitalTwinChangeNotification', b'opType': b'updateTwin'}
System properties (set by IoT Hub):  {b'user-id': b'my-pnp-hub\x81\x0e\xa4\x7f', b'correlation-id': b'12104ced5402', b'content-type': b'application/json-patch+json', b'content-encoding': b'utf-8', b'iothub-connection-device-id': b'my-pnp-device', b'iothub-enqueuedtime': 1603289442519, b'iothub-message-source': b'digitalTwinChangeEvents', b'x-opt-sequence-number': 13333, b'x-opt-offset': b'12890098024', b'x-opt-enqueued-time': 1603289442738}
```

Önceki Python kod parçacığında gösterilen kod, IoT Hub bileşenleri olan bir cihaz için dijital ikizi değişiklik bildirimleri oluşturduğunda aşağıdaki çıktıyı üretir. Bu örnek, bir termostat bileşeni olan bir sıcaklık algılayıcısı cihazı bildirim oluşturduğunda çıktıyı gösterir. Uygulama özellikleri `iothub-message-schema` ve `opType` değişiklik bildirimi türü hakkında bilgi verir:

```cmd/sh
Received event from partition: 1.
Telemetry received:  [{"op":"add","path":"/thermostat1","value":{"$metadata":{"maxTempSinceLastReboot":{"lastUpdateTime":"2020-10-21T14:13:39.36491Z"}},"maxTempSinceLastReboot":98.34}}]
Properties (set by device):  {b'hubName': b'my-pnp-hub', b'deviceId': b'my-pnp-device', b'operationTimestamp': b'2020-10-21T14:13:39.3649100+00:00', b'iothub-message-schema': b'digitalTwinChangeNotification', b'opType': b'updateTwin'}
System properties (set by IoT Hub):  {b'user-id': b'my-pnp-hub', b'correlation-id': b'1210b664ab83', b'content-type': b'application/json-patch+json', b'content-encoding': b'utf-8', b'iothub-connection-device-id': b'my-pnp-device', b'iothub-enqueuedtime': 1603289619481, b'iothub-message-source': b'digitalTwinChangeEvents', b'x-opt-sequence-number': 13342, b'x-opt-offset': b'12890102984', b'x-opt-enqueued-time': 1603289619668}
```
