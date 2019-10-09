---
title: Azure IoT Hub hızlı başlangıç 'a telemetri gönderme (Python) | Microsoft Docs
description: Bu hızlı başlangıçta, bir IoT Hub 'ına sanal telemetri göndermek ve IoT Hub 'ından telemetri okumak için bir yardımcı program kullanmak üzere örnek bir Python uygulaması çalıştırırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/28/2019
ms.openlocfilehash: 0e4cafee26d9d3345d9099c3c9fc048fb982ada5
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166406"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-python"></a>Hızlı başlangıç: bir cihazdan IoT Hub 'ına telemetri gönderme ve arka uç uygulamasıyla (Python) okuma

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan depolama ya da işleme için buluta kadar yüksek miktarda telemetri almanıza olanak sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT Hub aracılığıyla sanal bir cihaz uygulamasından, işleme için bir arka uç uygulamasına telemetri gönderirsiniz.

Hızlı başlangıç, Telemetriyi göndermek için önceden yazılmış bir Python uygulaması kullanır. Bu iki uygulamayı çalıştırmadan önce bir IoT Hub oluşturun ve bir cihazı hub 'a kaydedersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure CLı için Microsoft Azure IoT uzantısını Cloud Shell örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLı 'ye IoT Hub, IoT Edge ve IoT cihaz sağlama hizmeti 'ne (DPS) özel komutlar ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Örnek Python projesini https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip ' dan indirin ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT Hub 'ı oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Bir cihazı kaydetme

Bir cihazın bağlanabilmesi için IoT Hub 'ınız ile kayıtlı olması gerekir. Bu hızlı başlangıçta, sanal cihazı kaydetmek için Azure Cloud Shell kullanırsınız.

1. Cihaz kimliğini oluşturmak için Azure Cloud Shell aşağıdaki komutu çalıştırın.

    **Youriothubname**: aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    **Mypythondevice**: Bu, kaydetmekte olduğunuz cihazın adıdır. Gösterilen şekilde **Mypyıthondevice** kullanılması önerilir. Cihazınız için farklı bir ad seçerseniz, bu adı bu makalede da kullanmanız ve bunları çalıştırmadan önce örnek uygulamalarda cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyPythonDevice
    ```

1. Kaydettiğiniz cihazın _Cihaz bağlantı dizesini_ almak için Azure Cloud Shell ' de aşağıdaki komutu çalıştırın:

    **Youriothubname**: aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyPythonDevice --output table
    ```

    Cihaz bağlantı dizesini bir yere göz önünde bir şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyPythonDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri daha sonra hızlı başlangıçta kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönder

Sanal cihaz uygulaması, IoT Hub 'ınızdaki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisini gönderir.

1. Yerel bir Terminal penceresinde, örnek Python projesinin kök klasörüne gidin. Ardından **iot-hub\Quickstarts\simulated-Device** klasörüne gidin.

1. **SimulatedDevice.py** dosyasını istediğiniz bir metin düzenleyicisinde açın.

    @No__t-0 değişkeninin değerini, daha önce bir değişiklik yaptığınız cihaz bağlantı dizesiyle değiştirin. Sonra **SimulatedDevice.py**' ye yaptığınız değişiklikleri kaydedin.

1. Yerel Terminal penceresinde, sanal cihaz uygulaması için gerekli kitaplıkları yüklemek üzere aşağıdaki komutları çalıştırın:

    ```cmd/sh
    pip install azure-iot-device
    ```

1. Yerel Terminal penceresinde, sanal cihaz uygulamasını çalıştırmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    Aşağıdaki ekran görüntüsünde, sanal cihaz uygulamasının IoT Hub 'ınıza telemetri gönderdiği çıkış gösterilmektedir:

    ![Sanal cihazı Çalıştır](media/quickstart-send-telemetry-python/SimulatedDevice.png)


## <a name="read-the-telemetry-from-your-hub"></a>Hub 'ınızdaki Telemetriyi okuyun

IoT Hub CLı uzantısı, IoT Hub hizmet tarafı **olayları** uç noktasına bağlanabilir. Uzantı, sanal cihazınızdan gönderilen cihazdan buluta iletileri alır. Bir IoT Hub arka uç uygulaması, cihazdan buluta iletileri almak ve işlemek için genellikle bulutta çalışır.

Azure Cloud Shell, `YourIoTHubName` ' ı IoT Hub 'ınızın adıyla değiştirerek aşağıdaki komutları çalıştırın:

```azurecli-interactive
az iot hub monitor-events --hub-name {YourIoTHubName} --device-id MyPythonDevice 
```

Aşağıdaki ekran görüntüsünde, uzantı sanal cihaz tarafından hub 'a gönderilen telemetri aldığı için çıkış gösterilmektedir:

![Arka uç uygulamasını çalıştırma](media/quickstart-send-telemetry-python/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT Hub 'ı ayarlarsınız, bir cihaz kaydettiniz, bir Python uygulaması kullanarak hub 'a sanal telemetri gönderdiniz ve basit bir arka uç uygulaması kullanarak hub 'ın Telemetriyi okuyaöğreneceksiniz.

Bir arka uç uygulamasından sanal cihazınızı nasıl denetleyeceğinizi öğrenmek için sonraki hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: IoT Hub 'ına bağlı bir cihazı denetleme](quickstart-control-device-python.md)
