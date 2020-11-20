---
title: Öğretici-Azure 'a genel bir Python istemci uygulaması bağlama IoT Central | Microsoft Docs
description: Bu öğreticide, bir cihaz geliştiricisi olarak, Python istemci uygulaması çalıştıran bir cihaza Azure IoT Central uygulamanıza nasıl bağlanacağı gösterilmektedir. Bir işlecin bağlı bir cihazla etkileşime girmesine izin veren görünümler ekleyerek otomatik olarak oluşturulan cihaz şablonunu değiştirirsiniz.
author: dominicbetts
ms.author: dobett
ms.date: 11/03/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom:
- devx-track-python
- device-developer
ms.openlocfilehash: 47a178447533493911ae1f8ada5c617119bc3479
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94987697"
---
# <a name="tutorial-create-and-connect-a-client-application-to-your-azure-iot-central-application-python"></a>Öğretici: İstemci uygulamasını oluşturma ve Azure IoT Central uygulamanıza bağlama (Python)

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

*Bu makale, çözüm oluşturucular ve cihaz geliştiricileri için geçerlidir.*

Bu öğreticide, bir cihaz geliştiricisi olarak, bir Python istemci uygulamasını Azure IoT Central uygulamanıza nasıl bağlayabilmeniz gösterilmektedir. Python uygulaması, bir termostat cihazının davranışının benzetimini yapar. Uygulama IoT Central bağlandığı zaman, termostat cihaz modelinin model KIMLIĞINI gönderir. IoT Central, cihaz modelini almak ve sizin için bir cihaz şablonu oluşturmak üzere model KIMLIĞINI kullanır. Bir işlecin bir cihazla etkileşime geçmesini sağlamak için cihaz şablonuna özelleştirmeler ve görünümler eklersiniz.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Python Cihaz kodunu oluşturup çalıştırın ve IoT Central uygulamanıza bağlanın.
> * Cihazdan gönderilen sanal Telemetriyi görüntüleyin.
> * Özel görünümleri bir cihaz şablonuna ekleyin.
> * Cihaz şablonunu yayımlayın.
> * Cihaz özelliklerini yönetmek için bir görünüm kullanın.
> * Cihazı denetlemek için bir komut çağırın.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama** şablonu kullanılarak oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md). Uygulamanın 14 Temmuz 2020 tarihinde veya sonrasında oluşturulmuş olması gerekir.
* [Python](https://www.python.org/) sürüm 3,7 veya sonraki bir sürümü yüklü bir geliştirme makinesi. `python --version`Sürümünüzü denetlemek için komut satırından çalıştırabilirsiniz. Python, çok çeşitli işletim sistemleri için kullanılabilir. Bu öğreticideki yönergeler, Windows komut isteminde **Python** komutunu çalıştırmakta olduğunuzu varsayar.
* Örnek kodu içeren Python GitHub deposu [için Microsoft Azure ıOT SDK](https://github.com/Azure/azure-iot-sdk-python) 'nın yerel bir kopyası. Deponun bir kopyasını indirmek için bu bağlantıyı kullanın: ZIP 'i [indirin](https://github.com/Azure/azure-iot-sdk-python/archive/master.zip). Sonra dosyayı yerel makinenizde uygun bir konuma ayıklayın.

## <a name="review-the-code"></a>Kodu gözden geçirin

Daha önce indirdiğiniz Python için Microsoft Azure IoT SDK kopyasında, *Azure-IoT-SDK-Python/Azure-IoT-Device/Samples/PNP/simple_thermostat. Kopyala* dosyasını bir metin düzenleyicisinde açın.

IoT Central bağlanmak için örneği çalıştırdığınızda, cihazı kaydetmek ve bir bağlantı dizesi oluşturmak için cihaz sağlama hizmeti 'ni (DPS) kullanır. Örnek, gereken DPS bağlantı bilgilerini komut satırı ortamından alır.

`main`İşlev:

* , Cihazı sağlamak için DPS kullanır. Sağlama bilgileri, model KIMLIĞINI içerir.
* Bir `Device_client` nesne oluşturur ve `dtmi:com:example:Thermostat;1` bağlantıyı açmadan önce model kimliğini ayarlar.
* `maxTempSinceLastReboot`Özelliği IoT Central 'e gönderir.
* Komut için bir dinleyici oluşturur `getMaxMinReport` .
* Yazılabilir Özellik güncelleştirmelerini dinlemek için özellik dinleyicisi oluşturur.
* Her 10 saniyede bir sıcaklık telemetrisi göndermek için bir döngü başlatır.

```python
async def main():
    switch = os.getenv("IOTHUB_DEVICE_SECURITY_TYPE")
    if switch == "DPS":
        provisioning_host = (
            os.getenv("IOTHUB_DEVICE_DPS_ENDPOINT")
            if os.getenv("IOTHUB_DEVICE_DPS_ENDPOINT")
            else "global.azure-devices-provisioning.net"
        )
        id_scope = os.getenv("IOTHUB_DEVICE_DPS_ID_SCOPE")
        registration_id = os.getenv("IOTHUB_DEVICE_DPS_DEVICE_ID")
        symmetric_key = os.getenv("IOTHUB_DEVICE_DPS_DEVICE_KEY")

        registration_result = await provision_device(
            provisioning_host, id_scope, registration_id, symmetric_key, model_id
        )

        if registration_result.status == "assigned":

            device_client = IoTHubDeviceClient.create_from_symmetric_key(
                symmetric_key=symmetric_key,
                hostname=registration_result.registration_state.assigned_hub,
                device_id=registration_result.registration_state.device_id,
                product_info=model_id,
            )
        else:
            raise RuntimeError(
                "Could not provision device. Aborting Plug and Play device connection."
            )

    elif switch == "connectionString":

        # ...

    # Connect the client.
    await device_client.connect()

    max_temp = 10.96  # Initial Max Temp otherwise will not pass certification
    await device_client.patch_twin_reported_properties({"maxTempSinceLastReboot": max_temp})

    listeners = asyncio.gather(
        execute_command_listener(
            device_client,
            method_name="getMaxMinReport",
            user_command_handler=max_min_handler,
            create_user_response_handler=create_max_min_report_response,
        ),
        execute_property_listener(device_client),
    )

    async def send_telemetry():
        global max_temp
        global min_temp
        current_avg_idx = 0

        while True:
            current_temp = random.randrange(10, 50)
            if not max_temp:
                max_temp = current_temp
            elif current_temp > max_temp:
                max_temp = current_temp

            if not min_temp:
                min_temp = current_temp
            elif current_temp < min_temp:
                min_temp = current_temp

            avg_temp_list[current_avg_idx] = current_temp
            current_avg_idx = (current_avg_idx + 1) % moving_window_size

            temperature_msg1 = {"temperature": current_temp}
            await send_telemetry_from_thermostat(device_client, temperature_msg1)
            await asyncio.sleep(8)

    send_telemetry_task = asyncio.create_task(send_telemetry())

    # ...
```

`provision_device`İşlevi, cihazı sağlamak ve IoT Central kaydetmek IÇIN DPS kullanır. İşlevi, sağlama yükünde cihaz modeli KIMLIĞINI içerir:

```python
async def provision_device(provisioning_host, id_scope, registration_id, symmetric_key, model_id):
    provisioning_device_client = ProvisioningDeviceClient.create_from_symmetric_key(
        provisioning_host=provisioning_host,
        registration_id=registration_id,
        id_scope=id_scope,
        symmetric_key=symmetric_key,
    )
    provisioning_device_client.provisioning_payload = {"modelId": model_id}
    return await provisioning_device_client.register()
```

`execute_command_listener`İşlevi komut isteklerini işler, `max_min_handler` cihaz komutu aldığında işlevi çalıştırır `getMaxMinReport` ve `create_max_min_report_response` yanıtı oluşturmak için işlevi çalıştırır:

```python
async def execute_command_listener(
    device_client, method_name, user_command_handler, create_user_response_handler
):
    while True:
        if method_name:
            command_name = method_name
        else:
            command_name = None

        command_request = await device_client.receive_method_request(command_name)
        print("Command request received with payload")
        print(command_request.payload)

        values = {}
        if not command_request.payload:
            print("Payload was empty.")
        else:
            values = command_request.payload

        await user_command_handler(values)

        response_status = 200
        response_payload = create_user_response_handler(values)

        command_response = MethodResponse.create_from_method_request(
            command_request, response_status, response_payload
        )

        try:
            await device_client.send_method_response(command_response)
        except Exception:
            print("responding to the {command} command failed".format(command=method_name))
```

, Gibi `async def execute_property_listener` yazılabilir Özellik güncelleştirmelerini işler `targetTemperature` ve JSON yanıtını üretir:

```python
async def execute_property_listener(device_client):
    ignore_keys = ["__t", "$version"]
    while True:
        patch = await device_client.receive_twin_desired_properties_patch()  # blocking call

        print("the data in the desired properties patch was: {}".format(patch))

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

`send_telemetry_from_thermostat`İşlevi telemetri iletilerini IoT Central gönderir:

```python
async def send_telemetry_from_thermostat(device_client, telemetry_msg):
    msg = Message(json.dumps(telemetry_msg))
    msg.content_encoding = "utf-8"
    msg.content_type = "application/json"
    print("Sent message")
    await device_client.send_message(msg)
```

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

[!INCLUDE [iot-central-connection-configuration](../../../includes/iot-central-connection-configuration.md)]

## <a name="run-the-code"></a>Kodu çalıştırma

Örnek uygulamayı çalıştırmak için bir komut satırı ortamı açın ve *simple_thermostat. Kopyala* örnek dosyasını içeren *Azure-IoT-SDK-Python/Azure-IoT-Device/Samples/PNP* klasörüne gidin.

[!INCLUDE [iot-central-connection-environment](../../../includes/iot-central-connection-environment.md)]

Gerekli paketleri yükler:

```cmd/sh
pip install azure-iot-device
```

Örneği çalıştırın:

```cmd/sh
python simple_thermostat.py
```

Aşağıdaki çıktı, IoT Central kaydetme ve bağlanma için cihazı gösterir. Örnek, `maxTempSinceLastReboot` özelliği telemetri göndermeye başlamadan önce gönderir:

```cmd/sh
Device was assigned
iotc-.......azure-devices.net
sample-device-01
Listening for command requests and property updates
Press Q to quit
Sending telemetry for temperature
Sent message
Sent message
Sent message
```

[!INCLUDE [iot-central-monitor-thermostat](../../../includes/iot-central-monitor-thermostat.md)]

Aygıtın komutlara ve özellik güncelleştirmelerine nasıl yanıt verdiğini görebilirsiniz:

```cmd/sh
Sent message
the data in the desired properties patch was: {'targetTemperature': {'value': 86.3}, '$version': 2}
Sent message

...

Sent message
Command request received with payload
2020-10-14T08:00:00.000Z
Will return the max, min and average temperature from the specified time 2020-10-14T08:00:00.000Z to the current time
Done generating
{"avgTemp": 31.5, "endTime": "2020-10-16T10:07:41.580722", "maxTemp": 49, "minTemp": 12, "startTime": "2020-10-16T10:06:21.580632"}
```

## <a name="view-raw-data"></a>Ham verileri görüntüleme

[!INCLUDE [iot-central-monitor-thermostat-raw-data](../../../includes/iot-central-monitor-thermostat-raw-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

IoT Central öğreticiler kümesine devam etmeyi tercih ediyorsanız ve bir IoT Central çözümü oluşturma hakkında daha fazla bilgi edinmek istiyorsanız, bkz.:

> [!div class="nextstepaction"]
> [Ağ geçidi cihaz şablonu oluşturma](./tutorial-define-gateway-device-type.md)

Bir cihaz geliştiricisi olarak, Python kullanarak bir cihazın nasıl oluşturulacağı hakkında temel bilgileri öğrendiğinize göre, önerilen bazı sonraki adımlar şunlardır:

* Cihaz kodunuzu uygularken cihaz şablonlarının rolü hakkında daha fazla bilgi edinmek için cihaz [şablonu nedir?](./concepts-device-templates.md) makalesini okuyun.
* Cihazların IoT Central nasıl kaydedileceği ve cihaz bağlantılarının güvenliğini IoT Central sağlama hakkında daha fazla bilgi edinmek için [Azure IoT Central 'ye](./concepts-get-connected.md) bağlanın.
* IoT Central ile cihaz alışverişi yapılan veriler hakkında daha fazla bilgi edinmek için [telemetri, özellik ve komut yüklerini](concepts-telemetry-properties-commands.md) okuyun.
