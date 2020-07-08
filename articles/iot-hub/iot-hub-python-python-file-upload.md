---
title: Python ile Azure IoT Hub dosyaları cihazlardan yükleme | Microsoft Docs
description: Python için Azure IoT cihaz SDK 'sını kullanarak bir cihazdan buluta dosya yükleme. Karşıya yüklenen dosyalar bir Azure Storage blob kapsayıcısında depolanır.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: robinsh
ms.custom: mqtt, tracking-python
ms.openlocfilehash: 9a3782c0d5791f20f14aabb53d486fc012518c1f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84608511"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-python"></a>IoT Hub (Python) ile cihazınızdan buluta dosya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu makalede, [Azure Blob depolamaya](../storage/index.yml)bir dosyayı karşıya yüklemek için [IoT Hub dosya yükleme yeteneklerinin](iot-hub-devguide-file-upload.md) nasıl kullanılacağı gösterilmektedir. Öğretici şunların nasıl yapıldığını göstermektedir:

* Bir dosyayı karşıya yüklemek için güvenli bir şekilde depolama kapsayıcısı sağlayın.

* IoT Hub 'ınız aracılığıyla bir dosyayı karşıya yüklemek için Python istemcisini kullanın.

[Bir cihazdan IoT Hub 'ına hızlı başlangıç Telemetriyi](quickstart-send-telemetry-python.md) , IoT Hub temel cihazdan buluta mesajlaşma işlevlerini gösterir. Ancak, bazı senaryolarda, cihazlarınızın IoT Hub kabul ettiği görece küçük cihazdan buluta iletileri içine gönderdikleri verileri kolayca eşleyemezsiniz. Dosyaları bir cihazdan kapladığınızda, IoT Hub güvenlik ve güvenilirliğini kullanmaya devam edebilirsiniz.

Bu öğreticinin sonunda, Python konsol uygulamasını çalıştırırsınız:

* Python cihaz SDK 'sını kullanarak bir dosyayı depolamaya yükleyen **FileUpload.py**.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-async-installation-notes](../../includes/iot-hub-include-python-v2-async-installation-notes.md)]

* Güvenlik duvarınızdaki 8883 numaralı bağlantı noktasını açık olduğundan emin olun. Bu makaledeki cihaz örneği, 8883 numaralı bağlantı noktası üzerinden iletişim kuran MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağ ortamlarında engellenebilir. Bu sorunu geçici olarak çözmek için daha fazla bilgi ve IoT Hub bkz. [bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Bir cihaz uygulamasından bir dosya yükleme

Bu bölümde, IoT Hub 'a bir dosya yüklemek üzere cihaz uygulaması oluşturacaksınız.

1. Komut istemindeki **Azure-IoT-Device** paketini yüklemek için aşağıdaki komutu çalıştırın. Bu paketi, dosyayı IoT Hub 'ınızla birlikte karşıya yüklemeyi koordine etmek için kullanırsınız.

    ```cmd/sh
    pip install azure-iot-device
    ```

1. Komut istemindeki [**Azure. Storage. blob**](https://pypi.org/project/azure-storage-blob/) paketini yüklemek için aşağıdaki komutu çalıştırın. Dosyayı karşıya yüklemeyi gerçekleştirmek için bu paketi kullanın.

    ```cmd/sh
    pip install azure.storage.blob
    ```

1. Blob depolamaya yüklenecek bir test dosyası oluşturun.

1. Bir metin düzenleyicisi kullanarak çalışma klasörünüzde bir **FileUpload.py** dosyası oluşturun.

1. `import` **FileUpload.py** dosyasının başlangıcında aşağıdaki deyimleri ve değişkenleri ekleyin.

    ```python
    import os
    import asyncio
    from azure.iot.device.aio import IoTHubDeviceClient
    from azure.core.exceptions import AzureError
    from azure.storage.blob import BlobClient

    CONNECTION_STRING = "[Device Connection String]"
    PATH_TO_FILE = r"[Full path to local file]"
    ```

1. Dosyanızda, ' yi `[Device Connection String]` IoT Hub cihazınızın bağlantı dizesiyle değiştirin. `[Full path to local file]`Oluşturduğunuz test dosyasının yoluyla veya cihazınızda karşıya yüklemek istediğiniz herhangi bir dosya ile değiştirin.

1. Dosyayı blob depolamaya yüklemek için bir işlev oluşturun:

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

    Bu işlev, bir [Azure. Storage. blob. BlobClient](https://docs.microsoft.com/python/api/azure-storage-blob/azure.storage.blob.blobclient?view=azure-python)başlatmak için KULLANDıĞı bir URL oluşturmak üzere kendisine geçirilen *blob_info* yapısını ayrıştırır. Daha sonra bu istemciyi kullanarak dosyanızı Azure Blob depolama alanına yükler.

1. İstemcisini bağlamak ve dosyayı karşıya yüklemek için aşağıdaki kodu ekleyin:

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

    Bu kod, zaman uyumsuz bir **ıothubdeviceclient** oluşturur ve, IoT Hub 'ınız ile karşıya dosya yüklemeyi yönetmek Için aşağıdaki API 'leri kullanır:

    * **get_storage_info_for_blob** , IoT Hub 'ınızdan daha önce oluşturduğunuz bağlı depolama hesabı hakkında bilgi alır. Bu bilgiler, ana bilgisayar adı, kapsayıcı adı, blob adı ve SAS belirtecini içerir. Depolama bilgileri **store_blob** işlevine geçirilir (önceki adımda oluşturulur), bu nedenle bu Işlevdeki **Blobclient** , Azure depolama ile kimlik doğrulaması yapabilir. **Get_storage_info_for_blob** yöntemi, **notify_blob_upload_status** yönteminde kullanılan bir correlation_id de döndürür. Correlation_id, hangi Blobun üzerinde çalıştığınızı işaretleyen IoT Hub yoludur.

    * **notify_blob_upload_status** blob Storage işleminin durumunun IoT Hub bildirir. **Get_storage_info_for_blob** yöntemi tarafından elde edilen correlation_id geçirirsiniz. Bu, dosya yükleme görevinin durumu hakkında bir bildirim dinlebilecek herhangi bir hizmeti bilgilendirmek için IoT Hub tarafından kullanılır.

1. **UploadFile.py** dosyasını kaydedin ve kapatın.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Şimdi uygulamayı çalıştırmaya hazırsınız.

1. Çalışma klasörünüzdeki bir komut isteminde aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python FileUpload.py
    ```

2. Aşağıdaki ekran görüntüsünde, **FileUpload** uygulamasının çıktısı gösterilmektedir:

    ![Benzetimli cihaz uygulamasından çıkış](./media/iot-hub-python-python-file-upload/run-device-app.png)

3. Yapılandırdığınız depolama kapsayıcısında karşıya yüklenen dosyayı görüntülemek için portalını kullanabilirsiniz:

    ![Karşıya yüklenen dosya](./media/iot-hub-python-python-file-upload/view-blob.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, cihazların dosya karşıya yüklemelerini basitleştirmek için IoT Hub dosya yükleme yeteneklerini nasıl kullanacağınızı öğrendiniz. Aşağıdaki makalelerle IoT Hub özelliklerini ve senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlama yoluyla IoT Hub 'ı oluşturma](iot-hub-rm-template-powershell.md)

* [C SDK 'ya giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

Aşağıdaki bağlantılarla Azure Blob depolama hakkında daha fazla bilgi edinin:

* [Azure Blob depolama belgeleri](https://docs.microsoft.com/azure/storage/blobs/)

* [Python API belgeleri için Azure Blob depolama](https://docs.microsoft.com/python/api/overview/azure/storage-blob-readme?view=azure-python)
