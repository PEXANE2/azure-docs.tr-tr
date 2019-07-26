---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıcı (Python) | Microsoft Docs
description: Bu hızlı başlangıçta, bir IoT hub’a sanal telemetri göndermek için örnek bir Python uygulaması çalıştırır ve IoT hub’dan telemetri okumak için bir yardımcı program kullanırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/28/2019
ms.openlocfilehash: c92b019e15c6a9ee5b2d38e240ae4f9891621f72
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360202"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-python"></a>Hızlı Başlangıç: Bir cihazdan IoT Hub 'ına telemetri gönderme ve arka uç uygulamasıyla (Python) okuma

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, bir simülasyon cihazı uygulamasından bir arka uç uygulamasına işlenmek üzere IoT Hub aracılığıyla telemetri gönderirsiniz.

Bu hızlı başlangıçta, telemetri göndermek için önceden yazılmış bir Python uygulaması ve hub’dan telemetri okumak için bir CLI yardımcı programı kullanılır. Bu iki uygulamayı çalıştırmadan önce bir IoT hub oluşturur ve hub’a bir cihaz kaydedersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta çalıştırdığınız örnek uygulama Python kullanılarak yazılmıştır. Şu anda Python için Microsoft Azure IoT SDK 'Ları, her platform için yalnızca belirli Python sürümlerini destekler. Daha fazla bilgi için bkz. [Python SDK Benioku dosyası](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).

Bu hızlı başlangıç, bir Windows geliştirme makinesi kullandığınızı varsayar. Windows sistemleri için yalnızca [Python 3.6. x](https://www.python.org/downloads/release/python-368/) desteklenir. Çalıştığınız sistemin mimarisine uygun Python yükleyicisini seçmeniz gerekir. Sistem CPU mimariniz 32 bit ise, x86 yükleyicisi 'ni indirin; 64 bitlik mimari için x86-64 yükleyicisini indirin. Ayrıca, [Visual Studio 2019 Için Microsoft C++ Visual Redistributable](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) 'ın mimariniz (x86 veya x64) için yüklü olduğundan emin olun.

[Python.org](https://www.python.org/downloads/)adresinden diğer platformlar için Python indirebilirsiniz.

Aşağıdaki komutlardan birini kullanarak geliştirme makinenizde geçerli Python sürümünü doğrulayabilirsiniz:

```python
python - -version
```

```python
python3 - -version
```

Azure CLı için Microsoft Azure IoT uzantısını Cloud Shell örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLı 'ye IoT Hub, IoT Edge ve IoT cihaz sağlama hizmeti 'ne (DPS) özel komutlar ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

[https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip](https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip ) adresinden örnek Python projesini indirin ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliğini oluşturmak için Azure Cloud Shell aşağıdaki komutu çalıştırın.

    **Youriothubname**: Aşağıdaki yer tutucusunu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    **Mypythondevice**: Bu, kayıtlı cihaz için verilen addır. Gösterilen MyPythonDevice değerini kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyPythonDevice
    ```

1. Kaydettiğiniz cihazın _Cihaz bağlantı dizesini_ almak için Azure Cloud Shell aşağıdaki komutları çalıştırın:

    **Youriothubname**: Aşağıdaki yer tutucusunu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyPythonDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisi gönderir.

1. Yerel terminal penceresinde, örnek Python projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device** klasörüne gidin.

1. **SimulatedDevice.py** dosyasını, istediğiniz bir metin düzenleyicide açın.

    `CONNECTION_STRING` değişkeninin değerini, önceden not ettiğiniz cihaz bağlantı dizesiyle değiştirin. Daha sonra **SimulatedDevice.py** dosyasına değişikliklerinizi kaydedin.

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulaması için gerekli kitaplıkları yükleyin:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-python/SimulatedDevice.png)
    
### <a name="to-avoid-the-import-iothubclient-error"></a>İçeri aktarma iothub_client hatasını önlemek için
Python için Azure IoT SDK 'sının geçerli sürümü, [C SDK 'umuz](https://github.com/azure/azure-iot-sdk-c)üzerinde bir sarmalayıcıdır. Bu, [Boost](https://www.boost.org/) kitaplığı kullanılarak oluşturulur. Bu nedenle, bazı önemli sınırlamalara sahiptir. Daha fazla ayrıntı için [buraya](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues) bakın

1. [Python](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues)'un doğru sürümüne sahip olup olmadığınızı denetleyin. Yalnızca belirli sürümlerin bu örnek için sorunsuz bir şekilde çalıştığına dikkat edin. 
2. [Visual Studio 2019 Için Microsoft Visual C++ yeniden dağıtılabilir](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) C++ çalışma zamanının doğru sürümüne sahip olup olmadığınızı kontrol edin. (En son önerilir).
3. Iothub istemcisini yüklediğinizden emin olun: `pip install azure-iothub-device-client`.

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

IoT Hub uzantısı IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanabilir. Uzantı, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

Aşağıdaki komutları Azure Cloud Shell'de çalıştırın, `YourIoTHubName` yerine IoT hub'ınızın adını yazın:

```azurecli-interactive
az iot hub monitor-events --hub-name YourIoTHubName --device-id MyPythonDevice 
```

Aşağıdaki ekran görüntüsünde uzantı, simülasyon cihazı tarafından hub’a gönderilen telemetriyi aldığında oluşan çıktı gösterilmektedir:

![Arka uç uygulamasını çalıştırma](media/quickstart-send-telemetry-python/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub’ını ayarladınız, bir cihazı kaydettiniz, Python uygulamasını kullanarak hub’a sanal telemetri gönderdiniz ve basit bir arka uç uygulamasını kullanarak hub’dan telemetriyi okudunuz.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: IoT Hub 'ına bağlı bir cihazı denetleme](quickstart-control-device-python.md)
