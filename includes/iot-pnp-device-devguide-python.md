---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 11/19/2020
ms.openlocfilehash: f4eb312aff200389f59a3e342305b8eda98f213e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104582822"
---
## <a name="model-id-announcement"></a>Model KIMLIĞI duyurusu

Model KIMLIĞINI duyurmak için cihazın bağlantı bilgilerine eklemesi gerekir:

```python
device_client = IoTHubDeviceClient.create_from_symmetric_key(
    symmetric_key=symmetric_key,
    hostname=registration_result.registration_state.assigned_hub,
    device_id=registration_result.registration_state.device_id,
    product_info=model_id,
)
```

> [!TIP]
> Modüller ve IoT Edge için yerine kullanın `IoTHubModuleClient` `IoTHubDeviceClient` .

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

```python
async def send_telemetry_from_temp_controller(device_client, telemetry_msg, component_name=None):
    msg = Message(json.dumps(telemetry_msg))
    msg.content_encoding = "utf-8"
    msg.content_type = "application/json"
    if component_name:
        msg.custom_properties["$.sub"] = component_name
    await device_client.send_message(msg)
}
```

### <a name="read-only-properties"></a>Salt okunur özellikler

Varsayılan bileşenden bir özelliği raporlamak için özel bir yapı gerekmez:

```python
await device_client.patch_twin_reported_properties({"maxTempSinceLastReboot": 38.7})
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

```python
inner_dict = {}
inner_dict["targetTemperature"] = 38.7
inner_dict["__t"] = "c"
prop_dict = {}
prop_dict["thermostat1"] = inner_dict

await device_client.patch_twin_reported_properties(prop_dict)
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
    "thermostat1" : {  
      "__t" : "c",  
      "maxTempSinceLastReboot" : 38.7
     }
  }
}
```

### <a name="writable-properties"></a>Yazılabilir Özellikler

Bu özellikler cihaz tarafından ayarlanabilir veya çözüm tarafından güncelleştirilir. Çözüm bir özelliği güncelleştirdi, istemci veya içinde geri çağırma olarak bir bildirim alır `IoTHubDeviceClient` `IoTHubModuleClient` . IoT Tak ve Kullan kurallarını izlemek için cihazın, özelliği başarıyla alınan hizmete bildirmesi gerekir.

#### <a name="report-a-writable-property"></a>Yazılabilir bir özellik bildir

Bir cihaz yazılabilir bir özellik bildirdiğinde, `ack` kurallar içinde tanımlanan değerleri içermelidir.

Yazılabilir bir özelliği varsayılan bileşenden raporlamak için:

```python
prop_dict = {}
prop_dict["targetTemperature"] = {
    "ac": 200,
    "ad": "reported default value",
    "av": 0,
    "value": 23.2
}

await device_client.patch_twin_reported_properties(prop_dict)
```

Device ikizi, sonraki bildirilen özelliği ile güncellenir:

```json
{
  "reported": {
    "targetTemperature": {
      "value": 23.2,
      "ac": 200,
      "av": 0,
      "ad": "reported default value"
    }
  }
}
```

İç içe geçmiş bir bileşenden yazılabilir bir özellik raporlamak için, ikizi bir işaret içermelidir:

```python
inner_dict = {}
inner_dict["targetTemperature"] = {
    "ac": 200,
    "ad": "reported default value",
    "av": 0,
    "value": 23.2
}
inner_dict["__t"] = "c"
prop_dict = {}
prop_dict["thermostat1"] = inner_dict

await device_client.patch_twin_reported_properties(prop_dict)
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
          "av": 0,
          "ad": "complete"
      }
    }
  }
}
```

#### <a name="subscribe-to-desired-property-updates"></a>İstenen özellik güncelleştirmelerine abone ol

Hizmetler, bağlantılı cihazlarda bir bildirim tetikleyen istenen özellikleri güncelleştirebilir. Bu bildirim, güncelleştirmeyi tanımlayan sürüm numarası da dahil olmak üzere güncelleştirilmiş Desired özelliklerini içerir. Cihazların bildirilen özelliklerle aynı iletiyle yanıt vermesi gerekir `ack` .

Varsayılan bir bileşen, tek bir özelliği görür ve `ack` alınan sürüm ile bildirilen bir oluşturur:

```python
async def execute_property_listener(device_client):
    ignore_keys = ["__t", "$version"]
    while True:
        patch = await device_client.receive_twin_desired_properties_patch()  # blocking call

        version = patch["$version"]
        prop_dict = {}

        for prop_name, prop_value in patch.items():
            if prop_name in ignore_keys:
                continue
            else:
                prop_dict[prop_name] = {
                    "ac": 200,
                    "ad": "Successfully executed patch",
                    "av": version,
                    "value": prop_value,
                }

        await device_client.patch_twin_reported_properties(prop_dict)
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

İç içe yerleştirilmiş bir bileşen, bileşen adı ile Sarmalanan istenen özellikleri alır ve bildirilen özelliği geri bildirmelidir `ack` :

```python
def create_reported_properties_from_desired(patch):
    ignore_keys = ["__t", "$version"]
    component_prefix = list(patch.keys())[0]
    values = patch[component_prefix]

    version = patch["$version"]
    inner_dict = {}

    for prop_name, prop_value in values.items():
        if prop_name in ignore_keys:
            continue
        else:
            inner_dict["ac"] = 200
            inner_dict["ad"] = "Successfully executed patch"
            inner_dict["av"] = version
            inner_dict["value"] = prop_value
            values[prop_name] = inner_dict

    properties_dict = dict()
    if component_prefix:
        properties_dict[component_prefix] = values
    else:
        properties_dict = values

    return properties_dict

async def execute_property_listener(device_client):
    while True:
        patch = await device_client.receive_twin_desired_properties_patch()  # blocking call
        properties_dict = create_reported_properties_from_desired(patch)

        await device_client.patch_twin_reported_properties(properties_dict)
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

Varsayılan bir bileşen, hizmet tarafından çağrıldığı şekilde komut adını alır.

İç içe yerleştirilmiş bir bileşen, bileşen adı ve ayırıcıdan önekli komut adını alır `*` .

```python
command_request = await device_client.receive_method_request("thermostat1*reboot")
```

#### <a name="request-and-response-payloads"></a>İstek ve yanıt yükleri

Komutları, istek ve yanıt yüklerini tanımlamak için türleri kullanır. Bir cihaz gelen giriş parametresinin serisini çıkarmalıdır ve yanıtı serileştirmelidir. Aşağıdaki örnek, yüklerde tanımlanan karmaşık türlerle bir komutun nasıl uygulanacağını gösterir:

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

```python
def create_max_min_report_response(values):
    response_dict = {
        "maxTemp": max_temp,
        "minTemp": min_temp,
        "avgTemp": sum(avg_temp_list) / moving_window_size,
        "startTime": (datetime.now() - timedelta(0, moving_window_size * 8)).isoformat(),
        "endTime": datetime.now().isoformat(),
    }
    # serialize response dictionary into a JSON formatted str
    response_payload = json.dumps(response_dict, default=lambda o: o.__dict__, sort_keys=True)
    return response_payload
```

> [!Tip]
> İstek ve yanıt adları, tel üzerinden iletilen serileştirilmiş yüklere mevcut değildir.
