---
title: Azure IoT Hub quickstart (Android) için telemetri gönder | Microsoft Dokümanlar
description: Bu hızlı başlangıçta, simüle edilmiş telemetriyi bir IoT hub'ına göndermek ve bulutta işlemek için IoT hub'ından telemetri okumak için örnek bir Android uygulaması çalıştırın.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/15/2019
ms.author: wesmc
ms.openlocfilehash: e239f8c6a7738500176b84589f2032636eeb71e5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675432"
---
# <a name="quickstart-send-iot-telemetry-from-an-android-device"></a>Quickstart: Android cihazdan IoT telemetrisi gönderme

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

Bu hızlı başlangıçta, fiziksel veya simüle edilmiş bir aygıtta çalışan bir Android uygulamasından Azure IoT Hub'ına telemetri gönderirsiniz. IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu quickstart telemetri göndermek için önceden yazılmış bir Android uygulaması kullanır. Telemetri, Azure Bulut Kabuğu kullanılarak IoT Hub'dan okunur. Uygulamayı çalıştırmadan önce bir IoT hub'ı oluşturun ve bir aygıtı hub'a kaydettirin.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Android SDK 27 ile Android Studio](https://developer.android.com/studio/). Daha fazla bilgi için [android yüklemesi'ne](https://developer.android.com/studio/install)bakın. Android SDK 27 bu makaledeki örnek tarafından kullanılır.

* [Örnek bir Android uygulaması](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample). Bu örnek [azure-iot-örnekleri-java](https://github.com/Azure-Samples/azure-iot-samples-java) deposunun bir parçasıdır.

* Port 8883 güvenlik duvarınızda açılır. Bu hızlı başlatmadaki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="add-azure-iot-extension"></a>Azure IoT Uzantısı Ekle

Bulut Kabuğu örneğinize Azure CLI için Microsoft Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IOT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) özel komutlarını ekler.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aygıt kimliğini oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyAndroidDevice**: Bu, kaydettiğiniz cihazın adıdır. Gösterildiği gibi **MyAndroidDevice** kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanmanız ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyAndroidDevice
    ```

2. Kaydolduğunuz aygıtın _aygıt bağlantı dizesini_ almak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın:

    **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyAndroidDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri daha sonra bu hızlı başlangıçta telemetri göndermek için kullanırsınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

1. Android Studio'da GitHub örnek Android projesini açın. Proje, [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java) deposunun klonlanmış veya indirilen kopyasının aşağıdaki dizininde yer alır.

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. Android Studio'da, örnek proje için *gradle özelliklerini* açın ve **Device_Connection_String** yer tutucuyu daha önce not aldığınız aygıt bağlantı dizesiyle değiştirin.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}
    ```

3. Android Studio'da, Gradle Dosyalarıyla **Dosya** > **Eşitleme Projesi'ni**tıklatın. Yapının tamamlanın.

   > [!NOTE]
   > Proje eşitlemesi başarısız olursa, aşağıdaki nedenlerden biri olabilir:
   >
   > * Projede başvurulan Android Gradle eklentisi ve Gradle sürümleri Android Studio sürümünüz için güncel değildir. Yüklemeniz için eklenti ve Gradle'nin doğru sürümlerine başvurmak ve yüklemek için [bu yönergeleri](https://developer.android.com/studio/releases/gradle-plugin) izleyin.
   > * Android SDK için lisans sözleşmesi imzalanmadı. Lisans sözleşmesini imzalamak ve SDK'yı indirmek için Yapı çıktısındaki yönergeleri izleyin.

4. Yapı tamamlandıktan sonra **Çalıştır** > **'uygulamasını'** tıklatın. Uygulamayı fiziksel bir Android cihazda veya Android emülatörde çalışacak şekilde yapılandırın. Bir Android uygulamasını fiziksel bir cihazda veya emülatörde çalıştırma hakkında daha fazla bilgi [için](https://developer.android.com/training/basics/firstapp/running-app)bkz.

5. Uygulama yüklendikten sonra, IoT Hub'ınıza telemetri göndermeye başlamak için **Başlat** düğmesini tıklatın:

    ![Uygulama](media/quickstart-send-telemetry-android/sample-screenshot.png)


## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Bu bölümde, Android cihazı tarafından gönderilen aygıt iletilerini izlemek için [IoT uzantılı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Azure Bulut Kabuğu'nu kullanacaksınız.

1. Azure Cloud Shell'i kullanarak, IoT hub’ınızdan gelen iletilere bağlanmak ve bu iletileri okumak için aşağıdaki komutu çalıştırın:

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name {YourIoTHubName} --output table
    ```

    Aşağıdaki ekran görüntüsü, IoT hub'ı Android cihazı tarafından gönderilen telemetriyi aldığında çıktıyı gösterir:

      ![Azure CLI kullanarak cihaz iletilerini okuma](media/quickstart-send-telemetry-android/read-data.png)
## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub'ı kurup bir cihazı kaydettiniz, Android uygulamasını kullanarak hub'a benzetilen telemetriyi gönderdiniz ve Azure Bulut Kabuğu'nu kullanarak hub'daki telemetriyi okudunuz.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: IoT hub’a bağlı bir cihazı denetleme](quickstart-control-device-android.md)