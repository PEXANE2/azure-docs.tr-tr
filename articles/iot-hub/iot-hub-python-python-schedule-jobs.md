---
title: İşleri Azure IoT Hub zamanlayın (Python) | Microsoft Docs
description: Birden çok cihazda doğrudan yöntem çağırmak için bir Azure IoT Hub işi zamanlama. İşi çalıştırmak için sanal cihaz uygulamalarını ve bir hizmet uygulamasını uygulamak üzere Python için Azure IoT SDK 'larını kullanın.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: kgremban
ms.openlocfilehash: f4a7cbb5c4f8f4a019cbf5d63a6f2ffe8092546e
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405880"
---
# <a name="schedule-and-broadcast-jobs-python"></a>İşleri zamanlama ve yayınlama (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub, bir arka uç uygulamasının milyonlarca cihazı zamanlamayı ve güncelleştirmeyi sağlayan işleri oluşturmasına ve izlemesine olanak tanıyan, tam olarak yönetilen bir hizmettir.  İşler aşağıdaki eylemler için kullanılabilir:

* İstenen özellikleri güncelleştirme
* Etiketleri Güncelleştir
* Doğrudan metotları çağır

Kavramsal olarak, bir iş bu eylemlerden birini sarmalar ve yürütmenin ilerlemesini bir cihaz ikizi sorgusu tarafından tanımlanan bir cihaz kümesiyle izler.  Örneğin, bir arka uç uygulaması, bir cihaz ikizi sorgusu tarafından belirtilen ve gelecekteki bir zamanda zamanlanan 10.000 cihazlarda yeniden başlatma yöntemini çağırmak için bir iş kullanabilir.  Bu uygulama daha sonra bu cihazların her biri tarafından yeniden başlatma yöntemini alıp yürütebileceği ilerlemeyi izleyebilir.

Bu makalelerdeki her bir özellik hakkında daha fazla bilgi edinin:

* Cihaz ikizi ve özellikleri: [Cihaz](iot-hub-python-twin-getstarted.md) ikizlerini ve [öğreticisini kullanmaya başlayın: Cihaz ikizi özelliklerini kullanma](tutorial-device-twins.md)

* Doğrudan Yöntemler: [IoT Hub Geliştirici Kılavuzu-doğrudan Yöntemler](iot-hub-devguide-direct-methods.md) ve [öğretici: doğrudan Yöntemler](quickstart-control-device-python.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çözüm arka ucu tarafından çağrılabilen **Lockkapısı**sağlayan doğrudan yöntemine sahip bir Python sanal cihaz uygulaması oluşturun.

* Bir iş kullanarak sanal cihaz uygulamasındaki **Lockkapısı** doğrudan yöntemini çağıran ve bir cihaz işi kullanarak istenen özellikleri güncelleştiren bir Python konsol uygulaması oluşturun.

Bu öğreticinin sonunda iki Python uygulamanız vardır:

**simDevice.py**, IoT Hub 'ınıza cihaz kimliğiyle bağlanır ve bir **lockkapısı** doğrudan yöntemi alır.

**scheduleJobService.py**, sanal cihaz uygulamasında doğrudan bir yöntemi çağırır ve bir işi kullanarak Device ikizi 'ın istenen özelliklerini güncelleştirir.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Python 2. x veya 3. x](https://www.python.org/downloads/). Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*'yi yüklemeniz veya yükseltmeniz](https://pip.pypa.io/en/stable/installing/) gerekebilir.

* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi](https://www.microsoft.com/download/confirmation.aspx?id=48145).

* Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

> [!NOTE]
> **Python Için Azure ıOT SDK** , **iş** işlevlerini doğrudan desteklemez. Bu öğreticide, zaman uyumsuz iş parçacıkları ve zamanlayıcılar kullanan alternatif bir çözüm sunulmaktadır. Daha fazla güncelleştirme için, [Python Için Azure ıOT SDK](https://github.com/Azure/azure-iot-sdk-python) sayfasında **hizmet istemci SDK 'sı** özellik listesi ' ne bakın. 
>

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Python konsol uygulaması oluşturacaksınız ve bu da sanal bir **Lockkapısı** metodunu tetikler.

1. Komut istemindeki **Azure-IoT-Device-Client** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

2. Bir metin düzenleyicisi kullanarak çalışma dizininizde yeni bir **simDevice.py** dosyası oluşturun.

3. **SimDevice.py** dosyasının başlangıcında `import` aşağıdaki deyimleri ve değişkenleri ekleyin. Yukarıda `deviceConnectionString` oluşturduğunuz cihazın bağlantı dizesiyle değiştirin:

    ```python
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    METHOD_CONTEXT = 0
    TWIN_CONTEXT = 0
    WAIT_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

4. **Kilit kapısı** metodunu işlemek için aşağıdaki işlev geri aramasını ekleyin:

    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

5. Cihaz TWINS güncelleştirmelerini işlemek için başka bir işlev geri çağırması ekleyin:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "")
        print ( "Twin callback called with:")
        print ( "payload: %s" % payload )
    ```

6. **Kilit kapısı** yöntemi için işleyiciyi kaydetmek üzere aşağıdaki kodu ekleyin. Ayrıca, `main` yordamını da dahil edin:

    ```python
    def iothub_jobs_sample_run():
        try:
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
            client.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
            client.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

            print ( "Direct method initialized." )
            print ( "Device twin callback initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

            while True:
                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python jobs sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_jobs_sample_run()
    ```

7. **SimDevice.py** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.
>

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Bir doğrudan yöntem çağırmak ve bir cihaz ikizi özelliklerini güncelleştirmek için işleri zamanlama

Bu bölümde, doğrudan yöntem kullanan bir cihazda uzak **Lockkapısı** Başlatan bir Python konsol uygulaması oluşturursunuz ve Device ikizi 'in özelliklerini güncelleştirebilirsiniz.

1. Komut istemindeki **Azure-IoT-Service-Client** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iothub-service-client
    ```

2. Bir metin düzenleyicisi kullanarak çalışma dizininizde yeni bir **scheduleJobService.py** dosyası oluşturun.

3. **ScheduleJobService.py** dosyasının başlangıcında `import` aşağıdaki deyimleri ve değişkenleri ekleyin:

    ```python
    import sys
    import time
    import threading
    import uuid

    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "lockDoor"
    METHOD_PAYLOAD = "{\"lockTime\":\"10m\"}"
    UPDATE_JSON = "{\"properties\":{\"desired\":{\"building\":43,\"floor\":3}}}"
    TIMEOUT = 60
    WAIT_COUNT = 5
    ```

4. Cihazları sorgulamak için kullanılan aşağıdaki işlevi ekleyin:

    ```python
    def query_condition(device_id):
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

        number_of_devices = 10
        dev_list = iothub_registry_manager.get_device_list(number_of_devices)

        for device in range(0, number_of_devices):
            if dev_list[device].deviceId == device_id:
                return 1

        print ( "Device not found" )
        return 0
    ```

5. Doğrudan yöntem ve cihaz ikizi çağıran işleri çalıştırmak için aşağıdaki yöntemleri ekleyin:

    ```python
    def device_method_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job: " + str(job_id) )
        time.sleep(wait_time)

        if query_condition(device_id):
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            response = iothub_device_method.invoke(device_id, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Direct method " + METHOD_NAME + " called." )

    def device_twin_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job " + str(job_id) )
        time.sleep(wait_time)

        if query_condition(device_id):
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)

            print ( "" )
            print ( "Device twin updated." )
    ```

6. İşleri zamanlamak ve iş durumunu güncelleştirmek için aşağıdaki kodu ekleyin. Ayrıca, `main` yordamını da dahil edin:

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

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}" % iothub_error )
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