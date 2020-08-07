---
title: İşleri Azure IoT Hub zamanlayın (Python) | Microsoft Docs
description: Birden çok cihazda doğrudan yöntem çağırmak için bir Azure IoT Hub işi zamanlama. İşi çalıştırmak için sanal cihaz uygulamalarını ve bir hizmet uygulamasını uygulamak üzere Python için Azure IoT SDK 'larını kullanın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: robinsh
ms.custom: devx-track-python
ms.openlocfilehash: 733e3be21a1a1305b5c7947de1ae54ddce5e0d2f
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87876691"
---
# <a name="schedule-and-broadcast-jobs-python"></a>İşleri zamanlama ve yayınlama (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub, bir arka uç uygulamasının milyonlarca cihazı zamanlamayı ve güncelleştirmeyi sağlayan işleri oluşturmasına ve izlemesine olanak tanıyan, tam olarak yönetilen bir hizmettir.  İşler aşağıdaki eylemler için kullanılabilir:

* İstenen özellikleri güncelleştirme
* Etiketleri Güncelleştir
* Doğrudan metotları çağır

Kavramsal olarak, bir iş bu eylemlerden birini sarmalar ve yürütmenin ilerlemesini bir cihaz ikizi sorgusu tarafından tanımlanan bir cihaz kümesiyle izler.  Örneğin, bir arka uç uygulaması, bir cihaz ikizi sorgusu tarafından belirtilen ve gelecekteki bir zamanda zamanlanan 10.000 cihazlarda yeniden başlatma yöntemini çağırmak için bir iş kullanabilir.  Bu uygulama daha sonra bu cihazların her biri tarafından yeniden başlatma yöntemini alıp yürütebileceği ilerlemeyi izleyebilir.

Bu makalelerdeki her bir özellik hakkında daha fazla bilgi edinin:

* Cihaz ikizi ve özellikleri: [cihaz](iot-hub-python-twin-getstarted.md) Ikizlerini ve [öğreticisini kullanmaya başlama: cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)

* Doğrudan Yöntemler: [IoT Hub Geliştirici Kılavuzu-doğrudan](iot-hub-devguide-direct-methods.md) Yöntemler ve [öğretici: doğrudan Yöntemler](quickstart-control-device-python.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çözüm arka ucu tarafından çağrılabilen **Lockkapısı**sağlayan doğrudan yöntemine sahip bir Python sanal cihaz uygulaması oluşturun.

* Bir iş kullanarak sanal cihaz uygulamasındaki **Lockkapısı** doğrudan yöntemini çağıran ve bir cihaz işi kullanarak istenen özellikleri güncelleştiren bir Python konsol uygulaması oluşturun.

Bu öğreticinin sonunda iki Python uygulamanız vardır:

**simDevice.py**, IoT Hub 'ınıza cihaz kimliğiyle bağlanır ve bir **lockkapısı** doğrudan yöntemi alır.

**scheduleJobService.py**, sanal cihaz uygulamasında doğrudan bir yöntemi çağırır ve bir işi kullanarak Device ikizi 'ın istenen özelliklerini güncelleştirir.

> [!NOTE]
> **Python Için Azure ıOT SDK** , **iş** işlevlerini doğrudan desteklemez. Bu öğreticide, zaman uyumsuz iş parçacıkları ve zamanlayıcılar kullanan alternatif bir çözüm sunulmaktadır. Daha fazla güncelleştirme için, [Python Için Azure ıOT SDK](https://github.com/Azure/azure-iot-sdk-python) sayfasında **hizmet istemci SDK 'sı** özellik listesi ' ne bakın.
>

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [iot-hub-include-python-v2-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Python konsol uygulaması oluşturacaksınız ve bu da sanal bir **Lockkapısı** metodunu tetikler.

1. Komut istemindeki **Azure-IoT-Device** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-device
    ```

2. Bir metin düzenleyicisi kullanarak çalışma dizininizde yeni bir **simDevice.py** dosyası oluşturun.

3. `import` **SimDevice.py** dosyasının başlangıcında aşağıdaki deyimleri ve değişkenleri ekleyin. `deviceConnectionString`Yukarıda oluşturduğunuz cihazın bağlantı dizesiyle değiştirin:

    ```python
    import threading
    import time
    from azure.iot.device import IoTHubDeviceClient, MethodResponse

    CONNECTION_STRING = "{deviceConnectionString}"
    ```

4. **Kilit kapısı** metodunu işlemek için aşağıdaki işlev geri aramasını ekleyin:

    ```python
    def lockdoor_listener(client):
        while True:
            # Receive the direct method request
            method_request = client.receive_method_request("lockDoor")  # blocking call
            print( "Locking Door!" )

            resp_status = 200
            resp_payload = {"Response": "lockDoor called successfully"}
            method_response = MethodResponse(method_request.request_id, resp_status, resp_payload)
            client.send_method_response(method_response)
    ```

5. Cihaz TWINS güncelleştirmelerini işlemek için başka bir işlev geri çağırması ekleyin:

    ```python
    def twin_update_listener(client):
        while True:
            patch = client.receive_twin_desired_properties_patch()  # blocking call
            print ("")
            print ("Twin desired properties patch received:")
            print (patch)
    ```

6. **Kilit kapısı** yöntemi için işleyiciyi kaydetmek üzere aşağıdaki kodu ekleyin. Ayrıca, yordamını da dahil edin `main` :

    ```python
    def iothub_jobs_sample_run():
        try:
            client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)

            print( "Beginning to listen for 'lockDoor' direct method invocations...")
            lockdoor_listener_thread = threading.Thread(target=lockdoor_listener, args=(client,))
            lockdoor_listener_thread.daemon = True
            lockdoor_listener_thread.start()

            # Begin listening for updates to the Twin desired properties
            print ( "Beginning to listen for updates to Twin desired properties...")
            twin_update_listener_thread = threading.Thread(target=twin_update_listener, args=(client,))
            twin_update_listener_thread.daemon = True
            twin_update_listener_thread.start()
            
            while True:
                time.sleep(1000)

        except KeyboardInterrupt:
            print ( "IoTHubDeviceClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python jobs sample..." )
        print ( "IoTHubDeviceClient waiting for commands, press Ctrl-C to exit" )

        iothub_jobs_sample_run()
    ```

7. **SimDevice.py** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.
>

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

Bu makalede, bir cihazda doğrudan yöntem çağıran ve cihazı ikizi güncelleştiren bir arka uç hizmeti oluşturursunuz. Hizmet, bir cihazda doğrudan yöntem çağırmak için **hizmet Connect** iznine sahip olmalıdır. Ayrıca hizmet, kimlik kayıt defterini okuyup yazmak için **kayıt defteri okuma** ve **kayıt defteri yazma** izinlerine sahip olmalıdır. Yalnızca bu izinleri içeren bir varsayılan paylaşılan erişim ilkesi yoktur, bu nedenle bir tane oluşturmanız gerekir.

**Hizmet bağlantısı**, **kayıt defteri okuma**ve **kayıt defteri yazma** izinleri veren bir paylaşılan erişim ilkesi oluşturmak ve bu ilkeyle ilgili bir bağlantı dizesi almak için aşağıdaki adımları izleyin:

1. IoT Hub 'ınızı [Azure Portal](https://portal.azure.com)açın. IoT Hub 'ınıza almanın en kolay yolu **kaynak grupları**seçmek, IoT Hub 'ınızın bulunduğu kaynak grubunu seçmek ve ardından kaynak listesinden IoT Hub 'ınızı seçmeniz gerekir.

2. IoT Hub 'ınızın sol tarafındaki bölmede **paylaşılan erişim ilkeleri**' ni seçin.

3. İlke listesinin üstündeki en üstteki menüden **Ekle**' yi seçin.

4. **Paylaşılan erişim Ilkesi Ekle** bölmesinde, ilkeniz için açıklayıcı bir ad girin; Örneğin: *Serviceandregistryreadwrite*. **İzinler**altında **hizmet Connect** ve **kayıt defteri yazma** (kayıt defteri **yazma**seçeneğini belirlediğinizde**kayıt defteri okuma** otomatik olarak seçilir) seçeneğini belirleyin. Ardından **Oluştur**’u seçin.

    ![Yeni bir paylaşılan erişim ilkesinin nasıl ekleneceğini göster](./media/iot-hub-python-python-schedule-jobs/add-policy.png)

5. **Paylaşılan erişim ilkeleri** bölmesine geri döndüğünüzde, ilkeler listesinden yeni ilkenizi seçin.

6. **Paylaşılan erişim anahtarları**altında, **bağlantı dizesi--birincil anahtar** için Kopyala simgesini seçin ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınacağını göster](./media/iot-hub-python-python-schedule-jobs/get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için bkz. [erişim denetimi ve izinleri](./iot-hub-devguide-security.md#access-control-and-permissions).

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Bir doğrudan yöntem çağırmak ve bir cihaz ikizi özelliklerini güncelleştirmek için işleri zamanlama

Bu bölümde, doğrudan yöntem kullanan bir cihazda uzak **Lockkapısı** Başlatan bir Python konsol uygulaması oluşturursunuz ve ayrıca Device ikizi 'ın istenen özelliklerini güncelleştirir.

1. Komut istemindeki **Azure-IoT-Hub** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-hub
    ```

2. Bir metin düzenleyicisi kullanarak çalışma dizininizde yeni bir **scheduleJobService.py** dosyası oluşturun.

3. `import` **ScheduleJobService.py** dosyasının başlangıcında aşağıdaki deyimleri ve değişkenleri ekleyin. `{IoTHubConnectionString}`Yer tutucusunu, [IoT Hub bağlantı dizesini al](#get-the-iot-hub-connection-string)bölümünde daha önce kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin. `{deviceId}`Yer tutucusunu, [IoT Hub 'ına yeni bir cihaz kaydet](#register-a-new-device-in-the-iot-hub)' de kaydettiğiniz cihaz kimliğiyle değiştirin:

    ```python
    import sys
    import time
    import threading
    import uuid

    from azure.iot.hub import IoTHubRegistryManager
    from azure.iot.hub.models import Twin, TwinProperties, CloudToDeviceMethod, CloudToDeviceMethodResult, QuerySpecification, QueryResult

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "lockDoor"
    METHOD_PAYLOAD = "{\"lockTime\":\"10m\"}"
    UPDATE_PATCH = {"building":43,"floor":3}
    TIMEOUT = 60
    WAIT_COUNT = 5
    ```

4. Cihazları sorgulamak için kullanılan aşağıdaki işlevi ekleyin:

    ```python
    def query_condition(iothub_registry_manager, device_id):

        query_spec = QuerySpecification(query="SELECT * FROM devices WHERE deviceId = '{}'".format(device_id))
        query_result = iothub_registry_manager.query_iot_hub(query_spec, None, 1)

        return len(query_result.items)
    ```

5. Doğrudan yöntem ve cihaz ikizi çağıran işleri çalıştırmak için aşağıdaki yöntemleri ekleyin:

    ```python
    def device_method_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job: " + str(job_id) )
        time.sleep(wait_time)

        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)


        if query_condition(iothub_registry_manager, device_id):
            deviceMethod = CloudToDeviceMethod(method_name=METHOD_NAME, payload=METHOD_PAYLOAD)

            response = iothub_registry_manager.invoke_device_method(DEVICE_ID, deviceMethod)

            print ( "" )
            print ( "Direct method " + METHOD_NAME + " called." )

    def device_twin_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job " + str(job_id) )
        time.sleep(wait_time)

        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

        if query_condition(iothub_registry_manager, device_id):

            twin = iothub_registry_manager.get_twin(DEVICE_ID)
            twin_patch = Twin(properties= TwinProperties(desired=UPDATE_PATCH))
            twin = iothub_registry_manager.update_twin(DEVICE_ID, twin_patch, twin.etag)

            print ( "" )
            print ( "Device twin updated." )
    ```

6. İşleri zamanlamak ve iş durumunu güncelleştirmek için aşağıdaki kodu ekleyin. Ayrıca, yordamını da dahil edin `main` :

    ```python
    def iothub_jobs_sample_run():
        try:
            method_thr_id = uuid.uuid4()
            method_thr = threading.Thread(target=device_method_job, args=(method_thr_id, DEVICE_ID, 20, TIMEOUT), kwargs={})
            method_thr.start()

            print ( "" )
            print ( "Direct method called with Job Id: " + str(method_thr_id) )

            twin_thr_id = uuid.uuid4()
            twin_thr = threading.Thread(target=device_twin_job, args=(twin_thr_id, DEVICE_ID, 10, TIMEOUT), kwargs={})
            twin_thr.start()

            print ( "" )
            print ( "Device twin called with Job Id: " + str(twin_thr_id) )

            while True:
                print ( "" )

                if method_thr.is_alive():
                    print ( "...job " + str(method_thr_id) + " still running." )
                else:
                    print ( "...job " + str(method_thr_id) + " complete." )

                if twin_thr.is_alive():
                    print ( "...job " + str(twin_thr_id) + " still running." )
                else:
                    print ( "...job " + str(twin_thr_id) + " complete." )

                print ( "Job status posted, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(1)
                    status_counter += 1

        except Exception as ex:
            print ( "" )
            print ( "Unexpected error {0}" % ex )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubService sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub jobs Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_jobs_sample_run()
    ```

7. **ScheduleJobService.py** dosyasını kaydedin ve kapatın.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Çalışma dizininizde komut isteminde, önyükleme doğrudan metodunu dinlemeye başlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python simDevice.py
    ```

2. Çalışma dizininizde bulunan başka bir komut isteminde, kapıyı kilitlemek ve ikizi güncelleştirmek için işleri tetiklemek üzere aşağıdaki komutu çalıştırın:
  
    ```cmd/sh
    python scheduleJobService.py
    ```

3. Konsolundaki doğrudan yöntem ve cihaz TWINS güncelleştirmesine yönelik cihaz yanıtlarını görürsünüz.

    ![IoT Hub Iş örneği 1--cihaz çıkışı](./media/iot-hub-python-python-schedule-jobs/sample1-deviceoutput.png)

    ![IoT Hub Iş örneği 2--cihaz çıkışı](./media/iot-hub-python-python-schedule-jobs/sample2-deviceoutput.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir cihaza doğrudan yöntem zamanlamak ve Device ikizi 'in özelliklerinin güncelleştirilmesi için bir iş kullandınız.

AIR üretici yazılımı güncelleştirmesi üzerinden uzak gibi IoT Hub ve cihaz yönetim desenleriyle çalışmaya devam etmek için bkz. [nasıl yapılır bir bellenim güncelleştirmesi](tutorial-firmware-update.md).
