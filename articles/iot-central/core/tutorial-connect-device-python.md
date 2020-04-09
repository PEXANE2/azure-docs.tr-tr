---
title: Öğretici - Genel bir Python istemci uygulamasını Azure IoT Central'a bağlayın | Microsoft Dokümanlar
description: Bu öğretici, bir aygıt geliştiricisi olarak, Python istemcisi uygulamasını çalıştıran bir aygıtı Azure IoT Merkezi uygulamanıza nasıl bağlayabildiğinizi gösterir. Aygıt yeteneği modelini içe aktararak bir aygıt şablonu oluşturur ve bağlı bir aygıtla etkileşim kurmanıza izin veren görünümler eklersiniz
author: dominicbetts
ms.author: dobett
ms.date: 03/24/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 493dcd71905970434dd2ab2997cfebd17b8e47ff
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80891603"
---
# <a name="tutorial-create-and-connect-a-python-client-application-to-your-azure-iot-central-application-python"></a>Öğretici: Python istemcisi uygulaması oluşturun ve Azure IoT Merkezi uygulamanıza (Python) bağlayın

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

Bu öğretici, bir aygıt geliştiricisi olarak Python istemci uygulamasını Azure IoT Merkezi uygulamanıza nasıl bağlayacaklarını gösterir. Python uygulaması bir çevre sensörü aygıtının davranışını simüle eder. IoT Central'da bir _aygıt şablonu_ oluşturmak için örnek bir _aygıt yetenek modeli_ kullanırsınız. Bir işleç bir aygıtla etkileşim etemesini sağlamak için aygıt şablonuna görünümler eklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Aygıt şablonu oluşturmak için aygıt yeteneği modelini aktarın.
> * Aygıt şablonuna varsayılan ve özel görünümler ekleyin.
> * Bir aygıt şablonu yayımlayın ve IoT Central uygulamanıza gerçek bir aygıt ekleyin.
> * Python aygıt kodunu oluşturun ve çalıştırın ve IoT Central uygulamanıza bağlandığına bakın.
> * Aygıttan gönderilen benzetimli telemetriyi görüntüleyin.
> * Aygıt özelliklerini yönetmek için bir görünüm kullanın.
> * Cihazı kontrol etmek için senkron ve eşzamanlı komutları arayın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama** şablonu kullanılarak oluşturulan bir Azure IoT Merkezi uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* [Python](https://www.python.org/) sürüm 3.7 veya daha sonra yüklü bir geliştirme makinesi. Sürümünüzü `python3 --version` kontrol etmek için komut satırında çalıştırabilirsiniz. Python çok çeşitli işletim sistemleri için kullanılabilir. Bu öğreticideki talimatlar, Windows komut isteminde **python3** komutunu çalıştırdığınızı varsayar.

[!INCLUDE [iot-central-add-environmental-sensor](../../../includes/iot-central-add-environmental-sensor.md)]

### <a name="create-a-python-application"></a>Python uygulaması oluşturma

Aşağıdaki adımlar, uygulamaya eklediğiniz gerçek aygıta bağlanan bir Python istemci saki uygulamasının nasıl oluşturulabileceğinizi gösterir. Bu Python uygulaması gerçek bir aygıtın davranışını simüle eder.

1. Komut satırı ortamınızda, daha `environmental-sensor` önce oluşturduğunuz klasöre gidin.

1. Gerekli kitaplıkları yüklemek için aşağıdaki komutları çalıştırın:

    ```cmd
    pip install azure-iot-device
    ```

1. `environmental-sensor` Klasörde **environmental_sensor.py** adlı bir dosya oluşturun.

1. `import` **environmental_sensor.py** dosyasının başında aşağıdaki ifadeleri ekleyin:

    ```python
    import asyncio
    import os
    import json
    import datetime
    import random

    from azure.iot.device.aio import ProvisioningDeviceClient
    from azure.iot.device.aio import IoTHubDeviceClient
    from azure.iot.device import MethodResponse
    from azure.iot.device import Message
    ```

1. Dosyaya aşağıdaki eşzamanlı `main` işlev ve değişken bildirimleri ekleyin:

    ```python
    async def main():
      # In a production environment, don't store
      # connection information in the code.
      provisioning_host = 'global.azure-devices-provisioning.net'
      id_scope = '{your Scope ID}'
      registration_id = '{your Device ID}'
      symmetric_key = '{your Primary Key}'

      delay = 2

      # All the remaining code is nested within this main function

    if __name__ == '__main__':
    asyncio.run(main())
    ```

    Yer tutucuları `{your Scope ID}` `{your Device ID}`ve `{your Primary Key}` daha önce not aldığınız değerlerle güncelleştirin. Gerçek bir uygulamada, uygulamada bu bilgileri kodlamayın.

    Aşağıdaki tüm işlev tanımları ve kodu `main` işlev içinde iç içe dir.

1. Aygıtı kaydetmek ve IoT Merkezi uygulamanıza bağlamak için işlevin `main` içine aşağıdaki iki işlevi ekleyin. Kayıt, Azure Aygıt Sağlama Hizmetini kullanır:

    ```python
      async def register_device():
        provisioning_device_client = ProvisioningDeviceClient.create_from_symmetric_key(
          provisioning_host=provisioning_host,
          registration_id=registration_id,
          id_scope=id_scope,
          symmetric_key=symmetric_key,
        )

        registration_result = await provisioning_device_client.register()

        print(f'Registration result: {registration_result.status}')

        return registration_result

      async def connect_device():
        device_client = None
        try:
          registration_result = await register_device()
          if registration_result.status == 'assigned':
            device_client = IoTHubDeviceClient.create_from_symmetric_key(
              symmetric_key=symmetric_key,
              hostname=registration_result.registration_state.assigned_hub,
              device_id=registration_result.registration_state.device_id,
            )
            # Connect the client.
            await device_client.connect()
            print('Device connected successfully')
        finally:
          return device_client
    ```

1. IoT Merkezi `main` uygulamanıza telemetri göndermek için işlevin içine aşağıdaki işlevi ekleyin:

    ```python
      async def send_telemetry():
        print(f'Sending telemetry from the provisioned device every {delay} seconds')
        while True:
          temp = random.randrange(1, 75)
          humid = random.randrange(30, 99)
          payload = json.dumps({'temp': temp, 'humid': humid})
          msg = Message(payload)
          await device_client.send_message(msg, )
          print(f'Sent message: {msg}')
          await asyncio.sleep(delay)
    ```

    Telemetri öğelerinin adları`temp` ( `humid`ve ) aygıt şablonunda kullanılan adlarıyla eşleşmelidir.

1. IoT Merkezi `main` uygulamanızdan çağrılan komutları işlemek için işlevin içine aşağıdaki işlevleri ekleyin:

    ```python
      async def blink_command(request):
        print('Received synchronous call to blink')
        response = MethodResponse.create_from_method_request(
          request, status = 200, payload = {'description': f'Blinking LED every {request.payload} seconds'}
        )
        await device_client.send_method_response(response)  # send response
        print(f'Blinking LED every {request.payload} seconds')

      async def diagnostics_command(request):
        print('Starting asynchronous diagnostics run...')
        response = MethodResponse.create_from_method_request(
          request, status = 202
        )
        await device_client.send_method_response(response)  # send response
        print('Generating diagnostics...')
        await asyncio.sleep(2)
        print('Generating diagnostics...')
        await asyncio.sleep(2)
        print('Generating diagnostics...')
        await asyncio.sleep(2)
        print('Sending property update to confirm command completion')
        await device_client.patch_twin_reported_properties({'rundiagnostics': {'value': f'Diagnostics run complete at {datetime.datetime.today()}.'}})

      async def turnon_command(request):
        print('Turning on the LED')
        response = MethodResponse.create_from_method_request(
          request, status = 200
        )
        await device_client.send_method_response(response)  # send response

      async def turnoff_command(request):
        print('Turning off the LED')
        response = MethodResponse.create_from_method_request(
          request, status = 200
        )
        await device_client.send_method_response(response)  # send response

      commands = {
        'blink': blink_command,
        'rundiagnostics': diagnostics_command,
        'turnon': turnon_command,
        'turnoff': turnoff_command,
      }

      # Define behavior for handling commands
      async def command_listener():
        while True:
          method_request = await device_client.receive_method_request()  # Wait for commands
          await commands[method_request.name](method_request)
    ```

    Komutların`blink`adları ( , `turnon` `turnoff`, `rundiagnostics`, ve ) aygıt şablonunda kullanılan adlarla eşleşmelidir.

    Şu anda, IoT Central aygıt yeteneği modelinde tanımlanan yanıt şeasını kullanmıyor. Senkron bir komut için yanıt yükü geçerli bir JSON olabilir. Bir eşzamanlı komut için, aygıt hemen bir 202 yanıtı döndürmelidir ve ardından iş bittiğinde bildirilen özellik güncelleştirmesi. Bildirilen özellik güncelleştirmesinin biçimi:

    ```json
    {
      [command name] : {
        value: 'response message'
      }
    }
    ```

    Bir işleç komut geçmişinde yanıt yükünü görüntüleyebilir.

1. IoT Central `main` uygulamanızdan gönderilen özellik güncelleştirmelerini işlemek için işlevin içine aşağıdaki işlevleri ekleyin:

    ```python
        async def name_setting(value, version):
          await asyncio.sleep(1)
          print(f'Setting name value {value} - {version}')
          await device_client.patch_twin_reported_properties({'name' : {'value': value['value'], 'status': 'completed', 'desiredVersion': version}})

        async def brightness_setting(value, version):
          await asyncio.sleep(5)
          print(f'Setting brightness value {value} - {version}')
          await device_client.patch_twin_reported_properties({'brightness' : {'value': value['value'], 'status': 'completed', 'desiredVersion': version}})

        settings = {
          'name': name_setting,
          'brightness': brightness_setting
        }

        # define behavior for receiving a twin patch
        async def twin_patch_listener():
          while True:
            patch = await device_client.receive_twin_desired_properties_patch() # blocking
            to_update = patch.keys() & settings.keys()
            await asyncio.gather(
              *[settings[setting](patch[setting], patch['$version']) for setting in to_update]
            )
    ```

    Operatör IoT Merkezi uygulamasında yazılabilir bir özellik ayarladığında, uygulama değeri aygıta göndermek için iki zıt bir aygıt kullanır. Aygıt daha sonra ikiz bildirilen bir aygıt özelliği kullanarak yanıt verir. IoT Central bildirilen özellik değerini aldığında, özellik görünümünü eşitlenmiş bir durumla **güncelleştirir.**

    Özelliklerin adları (`name` `brightness`ve ) aygıt şablonunda kullanılan adlar eşleşmelidir.

1. Uygulamayı denetlemek için `main` işlevin içine aşağıdaki işlevleri ekleyin:

    ```python
      # Define behavior for halting the application
      def stdin_listener():
        while True:
          selection = input('Press Q to quit\n')
          if selection == 'Q' or selection == 'q':
            print('Quitting...')
            break

      device_client = await connect_device()

      if device_client is not None and device_client.connected:
        print('Send reported properties on startup')
        await device_client.patch_twin_reported_properties({'state': 'true'})
        tasks = asyncio.gather(
          send_telemetry(),
          command_listener(),
          twin_patch_listener(),
        )

        # Run the stdin listener in the event loop
        loop = asyncio.get_running_loop()
        user_finished = loop.run_in_executor(None, stdin_listener)

        # Wait for user to indicate they are done listening for method calls
        await user_finished

        # Cancel tasks
        tasks.add_done_callback(lambda r: r.exception())
        tasks.cancel()
        await device_client.disconnect()

      else:
        print('Device could not connect')
    ```

1. **environmental_sensor.py** dosyasını kaydedin.

## <a name="run-your-python-application"></a>Python uygulamanızı çalıştırın

Aygıt istemcisi uygulamasını başlatmak için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd
python3 environmental_sensor.py
```

Aygıtın Azure IoT Merkezi uygulamanıza bağlandığıve telemetri göndermeye başladığı nızı görebilirsiniz:

![İstemci uygulamasını çalıştırma](media/tutorial-connect-device-python/run-application.png)

[!INCLUDE [iot-central-monitor-environmental-sensor](../../../includes/iot-central-monitor-environmental-sensor.md)]

Aygıtın komutlara ve özellik güncelleştirmelerine nasıl yanıt verebileceğini görebilirsiniz:

![İstemci uygulamasını gözlemleyin](media/tutorial-connect-device-python/run-application-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Aygıt yeteneği modelleri ve kendi cihaz şablonlarınızı nasıl oluşturup oluşturabilirsiniz hakkında daha fazla bilgi edinmek için nasıl yapılacağınız kılavuzuna devam edin:

> [!div class="nextstepaction"]
> [Yeni bir IoT aygıt türü tanımlama](./howto-set-up-template.md)
