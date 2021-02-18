---
title: Cihaz telemetrisini Azure IoT Hub hızlı başlangıç 'a gönderme (Python)
description: Bu hızlı başlangıçta, bir cihazdan IoT Hub 'ına telemetri göndermek için Python için Azure IoT Hub cihaz SDK 'sını kullanacaksınız.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: python
ms.topic: quickstart
ms.date: 01/11/2021
ms.openlocfilehash: 0d791ebedd1b03bd50150170ad1b36a08371c9ae
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654985"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-azure-iot-hub-python"></a>Hızlı başlangıç: cihazdan Azure IoT Hub 'a (Python) telemetri gönderme

Uygulama **hedefi**: [cihaz uygulaması geliştirme](about-iot-develop.md#device-application-development)

Bu hızlı başlangıçta, temel bir IoT cihaz uygulama geliştirme iş akışı öğrenirsiniz. Azure CLı 'yı kullanarak bir Azure IoT Hub 'ı ve bir cihaz oluşturun, ardından sanal bir istemci cihazı oluşturmak ve hub 'a telemetri göndermek için Azure IoT Python SDK 'sını kullanın. 

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .
- Azure CLI. Bu hızlı başlangıçta, tarayıcınızda çalışan etkileşimli bir CLı kabuğu olan Azure Cloud Shell kullanarak tüm komutları çalıştırabilirsiniz. Cloud Shell kullanıyorsanız, herhangi bir şey yüklemeniz gerekmez. CLı 'yi yerel olarak kullanmayı tercih ediyorsanız bu hızlı başlangıç, Azure CLı sürüm 2.0.76 veya üstünü gerektirir. Sürümü bulmak için az --version komutunu çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).
- [Python 3.7 +](https://www.python.org/downloads/). Desteklenen Python 'un diğer sürümleri için bkz. [Azure IoT cihaz özellikleri](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device#azure-iot-device-features).
    
    Python sürümünüzün güncel olduğundan emin olmak için çalıştırın `python --version` . Hem Python 2 hem de Python 3 ' ü yüklediyseniz ve Python 3 ortamı kullanıyorsanız, kullanarak tüm kitaplıkları yükleyebilirsiniz `pip3` . Bu, kitaplıkların Python 3 çalışma zamanına yüklenmesini sağlar.
    > [!IMPORTANT]
    > Python Yükleyicisi ' nde, **yola Python ekleme** seçeneğini belirleyin. Python 3,7 veya üzeri bir sürümü zaten yüklüyse, Python yükleme klasörünü ortam değişkenine eklediğini onaylayın `PATH` .

[!INCLUDE [iot-hub-include-create-hub-cli](../../includes/iot-hub-include-create-hub-cli.md)]

## <a name="use-the-python-sdk-to-send-messages"></a>İleti göndermek için Python SDK 'sını kullanma
Bu bölümde, sanal cihazınızdan IoT Hub 'ınıza ileti göndermek için Python SDK 'sını kullanacaksınız.

1. Yeni bir terminal penceresi açın. Bu terminali, Python SDK 'sını yüklemek ve Python örnek kodu ile çalışmak için kullanacaksınız. Artık iki terminalin açık olması gerekir: Python ile çalışmak için açtığınız bir tane ve önceki bölümlerde Azure CLı komutları girmek için kullandığınız CLı kabuğu.       

1. [Azure IoT Python SDK cihaz örneklerini](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples) yerel makinenize kopyalayın:

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

    ve *Azure-IoT-SDK-Python/Azure-IoT-Device/Samples* dizinine gidiliyor:

    ```console
    cd azure-iot-sdk-python/azure-iot-device/samples
    ```
1. Azure IoT Python SDK 'sını yükler:

    ```console
    pip install azure-iot-device
    ```
1. Cihaz bağlantı dizesini adında bir ortam değişkeni olarak ayarlayın `IOTHUB_DEVICE_CONNECTION_STRING` . Bu, sanal Python cihazınızı oluşturduktan sonra önceki bölümde edindiğiniz dizedir.

    **Windows (cmd)**

    ```console
    set IOTHUB_DEVICE_CONNECTION_STRING=<your connection string here>
    ```

    > [!NOTE]
    > Windows CMD için bağlantı dizesini çevreleyen tırnak işareti yoktur.

    **Linux (Bash)**

    ```bash
    export IOTHUB_DEVICE_CONNECTION_STRING="<your connection string here>"
    ```

1. Açık CLı kabuğunuzun içinde, sanal IoT cihazınızdan olayları izlemeye başlamak için [az IoT Hub Monitor-Events](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub?view=azure-cli-latest#ext-azure-iot-az-iot-hub-monitor-events&preserve-view=true) komutunu çalıştırın.  Olay iletileri, geldikçe terminalde yazdırılacaktır.

    ```azurecli
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Python terminalinizde, yüklü örnek dosya *simple_send_message. Kopyala* için kodu çalıştırın. Bu kod, sanal IoT cihazına erişir ve IoT Hub 'ına bir ileti gönderir.

    Terminalden Python örneğini çalıştırmak için:
    ```console
    python ./simple_send_message.py
    ```

    İsteğe bağlı olarak, Python IDE 'niz içindeki örnekteki Python kodunu çalıştırabilirsiniz:
    ```python
    import os
    import asyncio
    from azure.iot.device.aio import IoTHubDeviceClient


    async def main():
        # Fetch the connection string from an environment variable
        conn_str = os.getenv("IOTHUB_DEVICE_CONNECTION_STRING")

        # Create instance of the device client using the authentication provider
        device_client = IoTHubDeviceClient.create_from_connection_string(conn_str)

        # Connect the device client.
        await device_client.connect()

        # Send a single message
        print("Sending message...")
        await device_client.send_message("This is a message that is being sent")
        print("Message successfully sent!")

        # finally, disconnect
        await device_client.disconnect()


    if __name__ == "__main__":
        asyncio.run(main())
    ```

Python kodu, cihazınızdan IoT Hub 'ına bir ileti gönderdiğinde, bu ileti CLı kabuğunuzun olayları izleyen şekilde görünür:

```output
Starting event monitor, use ctrl-c to stop...
event:
origin: <your Device name>
payload: This is a message that is being sent
```

Cihazınız artık güvenli bir şekilde bağlı ve Azure IoT Hub telemetri gönderiyor.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlangıçta oluşturulan Azure kaynaklarına artık ihtiyacınız yoksa, Azure CLı 'yi kullanarak bunları silebilirsiniz.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun.

Bir kaynak grubunu adıyla silmek için:
1. [Az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete&preserve-view=true) komutunu çalıştırın. Bu, kaynak grubunu, IoT Hub ve oluşturduğunuz cihaz kaydını kaldırır.

    ```azurecli
    az group delete --name MyResourceGroup
    ```
1. Kaynak grubunun silindiğini onaylamak için [az Group List](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-list&preserve-view=true) komutunu çalıştırın.  

    ```azurecli
    az group list
    ```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir cihazı buluta güvenli bir şekilde bağlamak ve cihazdan buluta telemetri göndermek için temel bir Azure IoT uygulama iş akışı öğrendiniz. Azure CLı 'yi bir IoT Hub 'ı ve bir cihaz oluşturmak için kullandınız, ardından sanal cihaz oluşturmak ve hub 'a telemetri göndermek için Azure IoT Python SDK 'sını kullandınız. 

Sonraki adım olarak, uygulama örnekleri aracılığıyla Azure IoT Python SDK 'sını keşfedebilirsiniz.

- [Zaman uyumsuz örnekler](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-hub-scenarios): Bu dizin, ek IoT Hub senaryolar için zaman uyumsuz Python örnekleri içerir.
- [Zaman uyumlu örnekler](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/sync-samples): Bu dizin Python 2,7 ile kullanmak için Python örnekleri veya Python 3.5 + için zaman uyumlu uyumluluk senaryoları içerir
- [IoT Edge örnekleri](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-edge-scenarios): Bu dizin, Edge modülleri ve aşağı akış cihazlarıyla çalışmak için Python örnekleri içerir.