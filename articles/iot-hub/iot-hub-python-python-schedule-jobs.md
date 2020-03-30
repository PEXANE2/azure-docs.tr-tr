---
title: Azure IoT Hub (Python) ile işleri zamanlama | Microsoft Dokümanlar
description: Birden çok aygıtta doğrudan bir yöntem çağırmak için Azure IoT Hub işi zamanlama. Python için Azure IoT SDK'larını kullanarak simüle edilmiş aygıt uygulamalarını ve işi yürütmek için bir hizmet uygulamasını uygularsınız.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: robinsh
ms.openlocfilehash: 1d721e89534c09a5572e5674796f28355f652165
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527410"
---
# <a name="schedule-and-broadcast-jobs-python"></a>Zamanlama ve yayın işleri (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub, milyonlarca aygıtı zamanlayan ve güncelleştiren işleri oluşturmak ve izlemek için bir arka uç uygulamasısağlayan tam olarak yönetilen bir hizmettir.  İşler aşağıdaki eylemler için kullanılabilir:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştir
* Doğrudan yöntemleri çağırma

Kavramsal olarak, bir iş bu eylemlerden birini sarar ve aygıt ikiz sorgusu tarafından tanımlanan bir aygıt kümesine karşı yürütmenin ilerlemesini izler.  Örneğin, bir arka uç uygulaması, aygıt ikiz sorgusu tarafından belirtilen ve ileride zamanlanan 10.000 aygıtta yeniden başlatma yöntemini çağırmak için bir işi kullanabilir.  Bu uygulama daha sonra bu aygıtların her biri yeniden başlatma yöntemini alır ve çalıştırır gibi ilerlemeyi izleyebilir.

Bu makalelerdeki bu özelliklerin her biri hakkında daha fazla bilgi edinin:

* Cihaz ikizi ve özellikleri: [Aygıt ikizleri](iot-hub-python-twin-getstarted.md) ve Öğretici ile [başlayın: Cihaz ikiz özellikleri nasıl kullanılır](tutorial-device-twins.md)

* Doğrudan yöntemler: [IoT Hub geliştirici kılavuzu - doğrudan yöntemler](iot-hub-devguide-direct-methods.md) ve [Öğretici: doğrudan yöntemler](quickstart-control-device-python.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Doğrudan bir yöntemi olan python simüle edilmiş bir aygıt uygulaması oluşturun, bu uygulamanın arka ucuyla çağrılabilir **lockDoor'u**etkinleştirin.

* Simüle edilen aygıt uygulamasında **ki lockDoor** direct yöntemini bir iş kullanarak çağıran ve bir aygıt işini kullanarak istenen özellikleri güncelleyen bir Python konsol uygulaması oluşturun.

Bu eğitimin sonunda, iki Python uygulamanız var:

**simDevice.py,** aygıt kimliği ile IoT hub'ınıza bağlanır ve bir **lockDoor** doğrudan yöntemi alır.

**scheduleJobService.py,** simüle cihaz uygulamasında doğrudan bir yöntem çağırır ve bir iş kullanarak aygıt ikiz istenen özellikleri günceller.

> [!NOTE]
> **Python için Azure IoT SDK,** **İşler** işlevselliğini doğrudan desteklemez. Bunun yerine bu öğretici asynchronous konuları ve zamanlayıcılar kullanarak alternatif bir çözüm sunuyor. Daha fazla güncelleştirme için Python için Azure [IoT SDK](https://github.com/Azure/azure-iot-sdk-python) sayfasındaki **Servis İstemci SDK** özellik listesine bakın.
>

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bulut tarafından çağrılan doğrudan bir yönteme yanıt veren ve benzetimli **lockDoor** yöntemini tetikleyen bir Python konsol uygulaması oluşturursunuz.

1. Komut isteminizde **azure-iot-device** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-device
    ```

2. Metin düzenleyicisi kullanarak, çalışma dizininizde yeni bir **simDevice.py** dosyası oluşturun.

3. `import` **simDevice.py** dosyanın başında aşağıdaki ifadeleri ve değişkenleri ekleyin. Yukarıda `deviceConnectionString` oluşturduğunuz aygıtın bağlantı dizesiyle değiştirin:

    ```python
    import threading
    import time
    from azure.iot.device import IoTHubDeviceClient, MethodResponse

    CONNECTION_STRING = "{deviceConnectionString}"
    ```

4. **LockDoor** yöntemini işlemek için aşağıdaki işlev geri aramasını ekleyin:

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

5. Aygıt ikiz güncelleştirmelerini işlemek için başka bir işlev geri arama sı ekleyin:

    ```python
    def twin_update_listener(client):
        while True:
            patch = client.receive_twin_desired_properties_patch()  # blocking call
            print ("")
            print ("Twin desired properties patch received:")
            print (patch)
    ```

6. **LockDoor** yöntemi için işleyiciyi kaydetmek için aşağıdaki kodu ekleyin. Ayrıca `main` rutin içerir:

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

7. **simDevice.py** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkeleri (üstel geri tepme gibi) uygulamanız gerekir.
>

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, aygıtta doğrudan bir yöntem çağıran ve aygıt ikizini güncelleyen bir arka uç hizmeti oluşturursunuz. Hizmetin, bir aygıtta doğrudan bir yöntem çağırmak için **servis bağlantısı** iznine ihtiyacı vardır. Hizmet ayrıca **kayıt defteri okumak** ve kayıt defteri kimlik defteri okumak ve yazmak için izinleri **yazmak** gerekir. Yalnızca bu izinleri içeren varsayılan paylaşılan erişim ilkesi yoktur, bu nedenle bir tane oluşturmanız gerekir.

**Hizmet bağlantısı,** **kayıt defteri okuma**ve kayıt **defteri yazma** izinleri veren paylaşılan bir erişim ilkesi oluşturmak ve bu ilke için bağlantı dizesi almak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)IoT hub'ınızı açın. IoT hub'ınıza gitmenin en kolay yolu **Kaynak gruplarını**seçmek, IoT hub'ınızın bulunduğu kaynak grubunu seçmek ve ardından kaynak listesinden IoT hub'ınızı seçmektir.

2. IoT hub'ınızın sol tarafındaki bölmede **Paylaşılan erişim ilkeleri'ni**seçin.

3. İlkeler listesinin üst menüsünden **Ekle'yi**seçin.

4. Paylaşılan **erişim ilkesi ekle** bölmesine, ilkeniz için açıklayıcı bir ad girin; örneğin: *serviceAndRegistryReadWrite*. **İzinler** **altında, Hizmet bağlantısı** ve **Kayıt Defteri yazmayı** seçin ( **Kayıt Defteri yazma'yı**seçtiğinizde**okunan kayıt defteri** otomatik olarak seçilir). Ardından **Oluştur**’u seçin.

    ![Yeni bir paylaşılan erişim ilkesinin nasıl ekleyeceğini göster](./media/iot-hub-python-python-schedule-jobs/add-policy.png)

5. **Paylaşılan erişim ilkeleri** bölmesine geri dönerek, ilkeler listesinden yeni ilkenizi seçin.

6. **Paylaşılan erişim tuşları**altında, Bağlantı **dizesi** için kopya simgesini seçin -- birincil anahtar ve değeri kaydedin.

    ![Bağlantı dizesinin nasıl alınır göster](./media/iot-hub-python-python-schedule-jobs/get-connection-string.png)

IoT Hub paylaşılan erişim ilkeleri ve izinleri hakkında daha fazla bilgi için [Erişim denetimi ve izinleri](./iot-hub-devguide-security.md#access-control-and-permissions)bkz.

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Doğrudan bir yöntem çağırmak ve aygıt ikizinin özelliklerini güncelleştirmek için işleri zamanlama

Bu bölümde, doğrudan bir yöntem kullanarak bir aygıtta uzaktan **kilitKapı** başlatan ve aynı zamanda aygıt ikizinin istenen özelliklerini güncelleyen bir Python konsol uygulaması oluşturursunuz.

1. Komut isteminizde **azure-iot-hub** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-hub
    ```

2. Metin düzenleyicisi kullanarak, çalışma dizininizde yeni bir **scheduleJobService.py** dosyası oluşturun.

3. `import` **scheduleJobService.py** dosyasının başında aşağıdaki ifadeleri ve değişkenleri ekleyin. Yer `{IoTHubConnectionString}` [tutucuyu, IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)daha önce kopyaladığınız IoT hub bağlantı dizesiyle değiştirin. Yer `{deviceId}` [tutucuyu, IoT hub'ında yeni bir aygıtı kaydedin'de](#register-a-new-device-in-the-iot-hub)kaydettiğiniz aygıt kimliğiyle değiştirin:

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

4. Aygıtlar için sorgu lamak için kullanılan aşağıdaki işlevi ekleyin:

    ```python
    def query_condition(iothub_registry_manager, device_id):

        query_spec = QuerySpecification(query="SELECT * FROM devices WHERE deviceId = '{}'".format(device_id))
        query_result = iothub_registry_manager.query_iot_hub(query_spec, None, 1)

        return len(query_result.items)
    ```

5. Doğrudan yöntem ve aygıt ikizi olarak adlandırdığı işleri çalıştırmak için aşağıdaki yöntemleri ekleyin:

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

6. İşleri zamanlamak ve iş durumunu güncelleştirmek için aşağıdaki kodu ekleyin. Ayrıca `main` rutin içerir:

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

7. **scheduleJobService.py** dosyasını kaydedin ve kapatın.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Çalışma dizininizdeki komut isteminde, doğrudan yeniden başlatma yöntemini dinlemeye başlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python simDevice.py
    ```

2. Çalışma dizininizdeki başka bir komut isteminde, kapıyı kilitlemek ve ikizi güncelleştirmek için işleri tetiklemek için aşağıdaki komutu çalıştırın:
  
    ```cmd/sh
    python scheduleJobService.py
    ```

3. Doğrudan yönteme verilen aygıt yanıtlarını ve aygıt ikizlerini konsolda günceller.

    ![IoT Hub İş örneği 1 -- cihaz çıkışı](./media/iot-hub-python-python-schedule-jobs/sample1-deviceoutput.png)

    ![IoT Hub İş örneği 2- cihaz çıkışı](./media/iot-hub-python-python-schedule-jobs/sample2-deviceoutput.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir aygıta doğrudan bir yöntem ve aygıt ikizözelliklerinin güncelliğini zamanlamak için bir iş kullandınız.

IoT Hub ve hava firmware güncellemesi üzerinden uzaktan gibi cihaz yönetimi desenleri ile çalışmaya başlamak için, [bir firmware güncelleştirmesi nasıl yapılacağını](tutorial-firmware-update.md)görün.