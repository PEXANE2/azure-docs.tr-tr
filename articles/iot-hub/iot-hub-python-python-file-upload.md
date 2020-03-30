---
title: Python ile aygıtlardan Azure IoT Hub'ına dosya yükleme | Microsoft Dokümanlar
description: Python için Azure IoT aygıtı SDK'yı kullanarak bir cihazdan buluta dosya yükleme. Yüklenen dosyalar bir Azure depolama blob kapsayıcısında depolanır.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: robinsh
ms.openlocfilehash: f1c0c046c40ff8edbc33c5e93e4207d9fe2fc67a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110754"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-python"></a>IoT Hub (Python) ile cihazınızdan buluta dosya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu makalede, Bir dosyayı [Azure blob depolamasına](../storage/index.yml)yüklemek için [IoT Hub'ın dosya yükleme özelliklerinin](iot-hub-devguide-file-upload.md) nasıl kullanılacağı gösterilmektedir. Öğretici şunların nasıl yapıldığını göstermektedir:

* Güvenli bir şekilde bir dosya yüklemek için bir depolama kapsayıcısı sağlayın.

* IoT hub'ınızüzerinden bir dosya yüklemek için Python istemcisini kullanın.

[Bir aygıttan IoT hub'ına](quickstart-send-telemetry-python.md) hızlı başlatma gönder telemetrisi, IoT Hub'ın temel aygıttan buluta mesajlaşma işlevselliğini gösterir. Ancak, bazı senaryolarda, aygıtlarınızın gönderdiği verileri IoT Hub'ın kabul ettiği nispeten küçük aygıttan buluta iletilerle kolayca eşleyemezsiniz. Bir aygıttan yayla dosyaları yapmanız gerektiğinde, IoT Hub'ın güvenliğini ve güvenilirliğini kullanmaya devam edebilirsiniz.

> [!NOTE]
> IoT Hub Python SDK şu anda yalnızca **.txt** dosyaları gibi karakter tabanlı dosyaların yüklenmesini destekler.

Bu eğitimin sonunda Python konsol uyrama uygulamasını çalıştırın:

* python aygıt SDK kullanarak depolama bir dosya yükler **FileUpload.py.**

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

> [!NOTE]
> Dosya Yükleme özelliği henüz yeni V2 SDK'da uygulanmadığı için, bu kılavuzda amortismana alınmış V1 Python SDK'da kullanMaktadır.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Aygıt uygulamasından dosya yükleme

Bu bölümde, bir dosyayı IoT hub'ına yüklemek için aygıt uygulamasını oluşturursunuz.

1. Komut isteminizde **azure-iothub-device-client** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

2. Metin düzenleyicisi kullanarak, blob depolamaalanına yükleyeceğiniz bir test dosyası oluşturun.

    > [!NOTE]
    > IoT Hub Python SDK şu anda yalnızca **.txt** dosyaları gibi karakter tabanlı dosyaların yüklenmesini destekler.

3. Metin düzenleyicisi kullanarak, çalışma klasörünüzde **FileUpload.py** bir dosya oluşturun.

4. `import` **FileUpload.py** dosyanın başında aşağıdaki ifadeleri ve değişkenleri ekleyin. 

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name for storage]"
    ```

5. Dosyanızda, IoT hub aygıtınızın bağlantı dizesiyle değiştirin. `[Device Connection String]` Oluşturduğunuz test dosyasına giden yolu veya cihazınızdaki yüklemek istediğiniz herhangi bir dosyayla değiştirin. `[Full path to file]` Blob depolama alanına yüklendikten sonra dosyanıza vermek istediğiniz adla değiştirin. `[File name for storage]` 

6. **upload_blob** işlevi için geri arama oluşturma:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

7. İstemciyi bağlamak ve dosyayı yüklemek için aşağıdaki kodu ekleyin. Ayrıca `main` rutin içerir:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            f = open(PATHTOFILE, "r")
            content = f.read()

            client.upload_blob_async(FILENAME, content, len(content), blob_upload_conf_callback, 0)

            print ( "" )
            print ( "File upload initiated..." )

            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )

    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

8. **UploadFile.py** dosyasını kaydedin ve kapatın.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Artık uygulamayı çalıştırmaya hazırsınız.

1. Çalışma klasörünüzdeki komut isteminde aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python FileUpload.py
    ```

2. Aşağıdaki ekran görüntüsü **FileUpload** uygulamasından çıktıgösterir:

    ![Simüle edilmiş cihaz uygulamasından çıktı](./media/iot-hub-python-python-file-upload/1.png)

3. Yüklediğiniz dosyayı yapılandırılan depolama kapsayıcısında görüntülemek için portalı kullanabilirsiniz:

    ![Yüklenen dosya](./media/iot-hub-python-python-file-upload/2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, cihazlardan gelen dosya yüklemelerini basitleştirmek için IoT Hub'ın dosya yükleme özelliklerini nasıl kullanacağınızı öğrendiniz. Aşağıdaki makalelerle IoT hub özelliklerini ve senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlı bir IoT hub'ı oluşturma](iot-hub-rm-template-powershell.md)

* [C SDK'ya Giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)
