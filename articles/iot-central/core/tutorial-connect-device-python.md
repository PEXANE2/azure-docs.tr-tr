---
title: Öğretici-Azure 'a genel bir Python istemci uygulaması bağlama IoT Central | Microsoft Docs
description: Bu öğreticide, bir cihaz geliştiricisi olarak, Python istemci uygulaması çalıştıran bir cihaza Azure IoT Central uygulamanıza nasıl bağlanacağı gösterilmektedir. Cihaz yetenek modeli içeri aktararak ve bağlı bir cihazla etkileşime girebilen görünümler ekleyerek bir cihaz şablonu oluşturursunuz
author: dominicbetts
ms.author: dobett
ms.date: 03/24/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: tracking-python
ms.openlocfilehash: 5555c176adfb5be78ea73f17bfa01ba87766acc1
ms.sourcegitcommit: 51718f41d36192b9722e278237617f01da1b9b4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85100404"
---
# <a name="tutorial-create-and-connect-a-client-application-to-your-azure-iot-central-application-python"></a>Öğretici: Azure IoT Central uygulamanıza istemci uygulaması oluşturma ve bağlama (Python)

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

*Bu makale, çözüm oluşturucular ve cihaz geliştiricileri için geçerlidir.*

Bu öğreticide, bir cihaz geliştiricisi olarak, bir Python istemci uygulamasını Azure IoT Central uygulamanıza nasıl bağlayabilmeniz gösterilmektedir. Python uygulaması, bir ortam algılayıcısı cihazının davranışını benzetir. IoT Central bir _cihaz şablonu_ oluşturmak için örnek bir _cihaz yetenek modeli_ kullanın. Bir işlecin bir cihazla etkileşime geçmesini sağlamak için cihaz şablonuna görünümler eklersiniz.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz şablonu oluşturmak için bir cihaz yetenek modeli içeri aktarın.
> * Varsayılan ve özel görünümleri bir cihaz şablonuna ekleyin.
> * Bir cihaz şablonu yayımlayın ve IoT Central uygulamanıza gerçek bir cihaz ekleyin.
> * Python Cihaz kodunu oluşturup çalıştırın ve IoT Central uygulamanıza bağlanın.
> * Cihazdan gönderilen sanal Telemetriyi görüntüleyin.
> * Cihaz özelliklerini yönetmek için bir görünüm kullanın.
> * Cihazı denetlemek için zaman uyumlu ve zaman uyumsuz komutları çağırın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama** şablonu kullanılarak oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* [Python](https://www.python.org/) sürüm 3,7 veya sonraki bir sürümü yüklü bir geliştirme makinesi. `python3 --version`Sürümünüzü denetlemek için komut satırından çalıştırabilirsiniz. Python, çok çeşitli işletim sistemleri için kullanılabilir. Bu öğreticideki yönergeler, **python3** komutunu Windows komut isteminde kullandığınızı varsayar.

[!INCLUDE [iot-central-add-environmental-sensor](../../../includes/iot-central-add-environmental-sensor.md)]

### <a name="create-a-python-application"></a>Python uygulaması oluşturma

Aşağıdaki adımlarda, uygulamaya eklediğiniz gerçek cihaza bağlanan bir Python istemci uygulamasının nasıl oluşturulacağı gösterilmektedir. Bu Python uygulaması, gerçek bir cihazın davranışının benzetimini yapar.

1. Komut satırı ortamınızda, `environmental-sensor` daha önce oluşturduğunuz klasöre gidin.

1. Gerekli kitaplıkları yüklemek için aşağıdaki komutları çalıştırın:

    ```cmd
    pip install azure-iot-device
    ```

1. Klasörde **environmental_sensor. Kopyala** adlı bir dosya oluşturun `environmental-sensor` .

1. `import` **Environmental_sensor. Kopyala** dosyasının başlangıcına aşağıdaki deyimlerini ekleyin:

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

1. Aşağıdaki zaman uyumsuz `main` işlev ve değişken bildirimlerini dosyaya ekleyin:

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

    Yer tutucuları `{your Scope ID}` , `{your Device ID}` , ve `{your Primary Key}` daha önce bir notcuı yaptığınız değerlerle güncelleştirin. Gerçek bir uygulamada, bu bilgileri uygulamada sabit olarak kodmayın.

    Aşağıdaki işlev tanımlarının ve kodların hepsi işlevin içinde iç içe `main` .

1. `main`Cihazı kaydetmek ve IoT Central uygulamanıza bağlamak için aşağıdaki iki işlevi işlevin içine ekleyin. Kayıt, Azure cihaz sağlama hizmeti 'ni kullanır:

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

1. `main`IoT Central uygulamanıza telemetri göndermek için işlevin içine aşağıdaki işlevi ekleyin:

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

    Telemetri öğelerinin (ve) adları, `temp` `humid` cihaz şablonunda kullanılan adlarla aynı olmalıdır.

1. `main`IoT Central uygulamanızdan çağrılan komutları işlemek için işlevin içine aşağıdaki işlevleri ekleyin:

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

    Komutların ( `blink` , `turnon` , ve) adları, `turnoff` `rundiagnostics` cihaz şablonunda kullanılan adlarla aynı olmalıdır.

    Şu anda IoT Central, cihaz yetenek modelinde tanımlanan yanıt şemasını kullanmıyor. Zaman uyumlu bir komut için, yanıt yükü geçerli bir JSON olabilir. Zaman uyumsuz bir komut için, cihazın hemen bir 202 yanıtı döndürmesi ve sonra iş tamamlandığında bildirilen özellik güncelleştirmesi gelmelidir. Bildirilen özellik güncelleştirmesinin biçimi:

    ```json
    {
      [command name] : {
        value: 'response message'
      }
    }
    ```

    Bir işleç, yanıt yükünü komut geçmişinde görüntüleyebilir.

1. `main`IoT Central uygulamanızdan gönderilen Özellik güncelleştirmelerini işlemek için işlevin içine aşağıdaki işlevleri ekleyin:

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

    İşleci IoT Central uygulamasında yazılabilir bir özellik ayarladığında, uygulama, değeri cihaza göndermek için bir Device ikizi istenen özelliğini kullanır. Cihaz daha sonra bir Device ikizi bildirilen özelliği kullanarak yanıt verir. IoT Central bildirilen özellik değerini aldığında, özellik görünümünü **eşitlenmiş**durumuyla güncelleştirir.

    Özelliklerin (ve) adları, `name` `brightness` cihaz şablonunda kullanılan adlarla eşleşmelidir.

1. `main`Uygulamayı denetlemek için işlevin içine aşağıdaki işlevleri ekleyin:

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

1. **Environmental_sensor. Kopyala** dosyasını kaydedin.

## <a name="run-your-python-application"></a>Python uygulamanızı çalıştırma

Cihaz istemci uygulamasını başlatmak için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd
python3 environmental_sensor.py
```

Cihazın Azure IoT Central uygulamanıza bağlandığını görebilir ve telemetri göndermeye başlar:

![İstemci uygulamasını çalıştırma](media/tutorial-connect-device-python/run-application.png)

[!INCLUDE [iot-central-monitor-environmental-sensor](../../../includes/iot-central-monitor-environmental-sensor.md)]

Aygıtın komutlara ve özellik güncelleştirmelerine nasıl yanıt verdiğini görebilirsiniz:

![İstemci uygulamasını gözlemleyin](media/tutorial-connect-device-python/run-application-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisi olarak, Python kullanarak bir cihazın nasıl oluşturulacağı hakkında temel bilgileri öğrendiğinize göre, önerilen bazı sonraki adımlar şunlardır:

* Bir [Mxyonga IoT DevKit cihazını Azure IoT Central uygulama](./howto-connect-devkit.md) nasıl yapılır makalesine bağlama hakkında IoT Central için gerçek bir cihazı nasıl bağlayacağınızı öğrenin.
* Cihaz kodunuzu uygularken cihaz şablonlarının rolü hakkında daha fazla bilgi edinmek için cihaz [şablonu nedir?](./concepts-device-templates.md) makalesini okuyun.
* Cihazların IoT Central nasıl kaydedileceği ve cihaz bağlantılarının güvenliğini IoT Central sağlama hakkında daha fazla bilgi edinmek için [Azure IoT Central 'ye](./concepts-get-connected.md) bağlanın.

IoT Central öğreticiler kümesine devam etmeyi tercih ediyorsanız ve bir IoT Central çözümü oluşturma hakkında daha fazla bilgi edinmek istiyorsanız, bkz.:

> [!div class="nextstepaction"]
> [Ağ geçidi cihaz şablonu oluşturma](./tutorial-define-gateway-device-type.md)
