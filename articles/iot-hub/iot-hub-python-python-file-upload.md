---
title: Python ile aygıtlardan Azure IoT Hub'ına dosya yükleme | Microsoft Dokümanlar
description: Python için Azure IoT aygıtı SDK'yı kullanarak bir cihazdan buluta dosya yükleme. Yüklenen dosyalar bir Azure depolama blob kapsayıcısında depolanır.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: robinsh
ms.custom: mqtt
ms.openlocfilehash: 47fb7c615389e24322450ed1785aa7da9ec50db6
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759697"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-python"></a>IoT Hub (Python) ile cihazınızdan buluta dosya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu makalede, Bir dosyayı [Azure blob depolamasına](../storage/index.yml)yüklemek için [IoT Hub'ın dosya yükleme özelliklerinin](iot-hub-devguide-file-upload.md) nasıl kullanılacağı gösterilmektedir. Öğretici şunların nasıl yapıldığını göstermektedir:

* Güvenli bir şekilde bir dosya yüklemek için bir depolama kapsayıcısı sağlayın.

* IoT hub'ınızüzerinden bir dosya yüklemek için Python istemcisini kullanın.

[Bir aygıttan IoT hub'ına](quickstart-send-telemetry-python.md) hızlı başlatma gönder telemetrisi, IoT Hub'ın temel aygıttan buluta mesajlaşma işlevselliğini gösterir. Ancak, bazı senaryolarda, aygıtlarınızın gönderdiği verileri IoT Hub'ın kabul ettiği nispeten küçük aygıttan buluta iletilerle kolayca eşleyemezsiniz. Bir aygıttan yayla dosyaları yapmanız gerektiğinde, IoT Hub'ın güvenliğini ve güvenilirliğini kullanmaya devam edebilirsiniz.

Bu eğitimin sonunda Python konsol uyrama uygulamasını çalıştırın:

* python aygıt SDK kullanarak depolama bir dosya yükler **FileUpload.py.**

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-async-installation-notes](../../includes/iot-hub-include-python-v2-async-installation-notes.md)]

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Aygıt uygulamasından dosya yükleme

Bu bölümde, bir dosyayı IoT hub'ına yüklemek için aygıt uygulamasını oluşturursunuz.

1. Komut isteminizde **azure-iot-device** paketini yüklemek için aşağıdaki komutu çalıştırın. Bu paketi, dosya yüklemesini IoT hub'ınızla koordine etmek için kullanırsınız.

    ```cmd/sh
    pip install azure-iot-device
    ```

1. Komut isteminizde [**azure.storage.blob**](https://pypi.org/project/azure-storage-blob/) paketini yüklemek için aşağıdaki komutu çalıştırın. Dosya yüklemesini gerçekleştirmek için bu paketi kullanırsınız.

    ```cmd/sh
    pip install azure.storage.blob
    ```

1. Blob depolama alanına yükleyeceğiniz bir test dosyası oluşturun.

1. Metin düzenleyicisi kullanarak, çalışma klasörünüzde **FileUpload.py** bir dosya oluşturun.

1. `import` **FileUpload.py** dosyanın başında aşağıdaki ifadeleri ve değişkenleri ekleyin.

    ```python
    import os
    import asyncio
    from azure.iot.device.aio import IoTHubDeviceClient
    from azure.core.exceptions import AzureError
    from azure.storage.blob import BlobClient

    CONNECTION_STRING = "[Device Connection String]"
    PATH_TO_FILE = r"[Full path to local file]"
    ```

1. Dosyanızda, IoT hub aygıtınızın bağlantı dizesiyle değiştirin. `[Device Connection String]` Oluşturduğunuz test dosyasına giden yolu veya cihazınızda yüklemek istediğiniz herhangi bir dosyayla değiştirin. `[Full path to local file]`

1. Dosyayı blob depolama alanına yüklemek için bir işlev oluşturun:

    ```python
    async def store_blob(blob_info, file_name):
        try:
            sas_url = "https://{}/{}/{}{}".format(
                blob_info["hostName"],
                blob_info["containerName"],
                blob_info["blobName"],
                blob_info["sasToken"]
            )

            print("\nUploading file: {} to Azure Storage as blob: {} in container {}\n".format(file_name, blob_info["blobName"], blob_info["containerName"]))

            # Upload the specified file
            with BlobClient.from_blob_url(sas_url) as blob_client:
                with open(file_name, "rb") as f:
                    result = blob_client.upload_blob(f, overwrite=True)
                    return (True, result)

        except FileNotFoundError as ex:
            # catch file not found and add an HTTP status code to return in notification to IoT Hub
            ex.status_code = 404
            return (False, ex)

        except AzureError as ex:
            # catch Azure errors that might result from the upload operation
            return (False, ex)
    ```

    Bu işlev, [azure.storage.blob.BlobClient'ı](https://docs.microsoft.com/python/api/azure-storage-blob/azure.storage.blob.blobclient?view=azure-python)başlatmayı kullandığı bir URL oluşturmak için içine geçirilen *blob_info* yapıyı ayrıştırır. Ardından bu istemciyi kullanarak dosyanızı Azure blob depolama alanına yükler.

1. İstemciyi bağlamak ve dosyayı yüklemek için aşağıdaki kodu ekleyin:

    ```python
    async def main():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            conn_str = CONNECTION_STRING
            file_name = PATH_TO_FILE
            blob_name = os.path.basename(file_name)

            device_client = IoTHubDeviceClient.create_from_connection_string(conn_str)

            # Connect the client
            await device_client.connect()

            # Get the storage info for the blob
            storage_info = await device_client.get_storage_info_for_blob(blob_name)

            # Upload to blob
            success, result = await store_blob(storage_info, file_name)

            if success == True:
                print("Upload succeeded. Result is: \n") 
                print(result)
                print()

                await device_client.notify_blob_upload_status(
                    storage_info["correlationId"], True, 200, "OK: {}".format(file_name)
                )

            else :
                # If the upload was not successful, the result is the exception object
                print("Upload failed. Exception is: \n") 
                print(result)
                print()

                await device_client.notify_blob_upload_status(
                    storage_info["correlationId"], False, result.status_code, str(result)
                )

        except Exception as ex:
            print("\nException:")
            print(ex)

        except KeyboardInterrupt:
            print ( "\nIoTHubDeviceClient sample stopped" )

        finally:
            # Finally, disconnect the client
            await device_client.disconnect()


    if __name__ == "__main__":
        asyncio.run(main())
        #loop = asyncio.get_event_loop()
        #loop.run_until_complete(main())
        #loop.close()
    ```

    Bu kod, eşzamanlı bir **IoTHubDeviceClient** oluşturur ve dosya yüklemesini IoT hub'ınızla yönetmek için aşağıdaki API'leri kullanır:

    * **get_storage_info_for_blob,** IoT hub'ınızdan daha önce oluşturduğunuz bağlantılı Depolama Hesabı hakkında bilgi alır. Bu bilgiler, ana bilgisayar adı, kapsayıcı adı, blob adı ve Bir SAS belirteci içerir. Depolama bilgileri **store_blob** işlevine (önceki adımda oluşturulmuş) aktarılır, böylece bu işlevdeki **BlobClient** Azure depolama ile kimlik doğrulaması yapabilir. **get_storage_info_for_blob** yöntemi, **notify_blob_upload_status** yönteminde kullanılan bir correlation_id de döndürür. correlation_id, IoT Hub'ın üzerinde çalıştığınız blob'u işaretleme şeklidir.

    * **notify_blob_upload_status,** ioT Hub'a blob depolama işleminizin durumunu belirtir. **get_storage_info_for_blob** yöntemiyle elde edilen correlation_id aktarın. IoT Hub tarafından, dosya yükleme görevinin durumu yla ilgili bir bildirimi dinleyen herhangi bir hizmeti bildirmek için kullanılır.

1. **UploadFile.py** dosyasını kaydedin ve kapatın.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Artık uygulamayı çalıştırmaya hazırsınız.

1. Çalışma klasörünüzdeki komut isteminde aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python FileUpload.py
    ```

2. Aşağıdaki ekran görüntüsü **FileUpload** uygulamasından çıktıgösterir:

    ![Simüle edilmiş cihaz uygulamasından çıktı](./media/iot-hub-python-python-file-upload/run-device-app.png)

3. Yüklediğiniz dosyayı yapılandırılan depolama kapsayıcısında görüntülemek için portalı kullanabilirsiniz:

    ![Yüklenen dosya](./media/iot-hub-python-python-file-upload/view-blob.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, cihazlardan gelen dosya yüklemelerini basitleştirmek için IoT Hub'ın dosya yükleme özelliklerini nasıl kullanacağınızı öğrendiniz. Aşağıdaki makalelerle IoT hub özelliklerini ve senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlı bir IoT hub'ı oluşturma](iot-hub-rm-template-powershell.md)

* [C SDK'ya Giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

Azure Blob Depolama hakkında aşağıdaki bağlantılarla daha fazla bilgi edinin:

* [Azure Blob Depolama belgeleri](https://docs.microsoft.com/azure/storage/blobs/)

* [Python API belgeleri için Azure Blob Depolama](https://docs.microsoft.com/python/api/overview/azure/storage-blob-readme?view=azure-python)
