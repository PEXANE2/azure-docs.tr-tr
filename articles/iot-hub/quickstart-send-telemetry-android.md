---
title: Azure IoT Hub hızlı başlangıç 'a telemetri gönderme (Android) | Microsoft Docs
description: Bu hızlı başlangıçta, bir IoT Hub 'ına sanal telemetri göndermek ve bulutta işlenmek üzere IoT Hub 'ından Telemetriyi okumak için örnek bir Android uygulaması çalıştırırsınız.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom:
- mvc
- mqtt
ms.date: 03/15/2019
ms.author: wesmc
ms.openlocfilehash: 67d56429f092343193fe5c1e3449aff033673320
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727191"
---
# <a name="quickstart-send-iot-telemetry-from-an-android-device"></a>Hızlı başlangıç: Android cihazından IoT telemetrisi gönderme

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

Bu hızlı başlangıçta, fiziksel veya sanal cihaz üzerinde çalışan bir Android uygulamasından Azure IoT Hub telemetri gönderirsiniz. IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta Telemetriyi göndermek için önceden yazılmış bir Android uygulaması kullanılmaktadır. Telemetri, Azure Cloud Shell kullanılarak IoT Hub okunacak. Uygulamayı çalıştırmadan önce bir IoT Hub oluşturun ve bir cihazı hub 'a kaydedersiniz.

## <a name="prerequisites"></a>Ön koşullar

* Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

* [Android SDK 27 ile Android Studio](https://developer.android.com/studio/). Daha fazla bilgi için bkz. [Android-yükleme](https://developer.android.com/studio/install). Android SDK 27, bu makaledeki örnek tarafından kullanılır.

* [Örnek bir Android uygulaması](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample). Bu örnek, [Azure-IoT-Samples-Java](https://github.com/Azure-Samples/azure-iot-samples-java) deposunun bir parçasıdır.

* Bağlantı noktası 8883 güvenlik duvarınızda açık. Bu hızlı başlangıçta bulunan cihaz örneği, 8883 bağlantı noktası üzerinden iletişim kuran MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağ ortamlarında engellenebilir. Bu sorunu geçici olarak çözmek için daha fazla bilgi ve IoT Hub bkz. [bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="add-azure-iot-extension"></a>Azure IoT uzantısı ekleme

Azure CLı için Microsoft Azure IoT uzantısını Cloud Shell örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLı 'ye IoT Hub, IoT Edge ve IoT cihaz sağlama hizmeti 'ne (DPS) özel komutlar ekler.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Cihaz kimliğini oluşturmak için Azure Cloud Shell aşağıdaki komutu çalıştırın.

   **Youriothubname**: aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

   **Myandroıddevice**: Bu, kaydetmekte olduğunuz cihazın adıdır. **Myandroıddevice** 'ın gösterildiği gibi kullanılması önerilir. Cihazınız için farklı bir ad seçerseniz, bu adı bu makalede da kullanmanız ve bunları çalıştırmadan önce örnek uygulamalarda cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyAndroidDevice
    ```

2. Yeni kaydettiğiniz cihazın _Cihaz bağlantı dizesini_ almak için Azure Cloud Shell ' de aşağıdaki komutu çalıştırın:

    **Youriothubname**: aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyAndroidDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri daha sonra Telemetriyi göndermek için bu hızlı başlangıçta kullanacaksınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

1. Android Studio 'de GitHub örnek Android projesini açın. Proje, kopyalanmış veya indirilen [Azure-IoT-Sample-Java](https://github.com/Azure-Samples/azure-iot-samples-java) deposunun kopyası dizininde bulunur.

        \azure-iot-samples-java\iot-hub\Samples\device\AndroidSample

2. Android Studio, örnek proje için *Gradle. Properties* ' i açın ve **Device_Connection_String** yer tutucusunu, daha önce bir değişiklik yaptığınız cihaz bağlantı dizesiyle değiştirin.

    ```
    DeviceConnectionString=HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}
    ```

3. Android Studio ' de, **File**  >  **Gradle dosyaları ile dosya eşitleme projesi**' ne tıklayın. Oluşturma işlemini doğrulayın.

   > [!NOTE]
   > Proje eşitleme başarısız olursa, aşağıdaki nedenlerden biri olabilir:
   >
   > * Android Studio sürümünüz için, projede başvurulan Android Gradle Plugin ve Gradle sürümleri güncel değil. Yüklemeniz için eklentinin ve Gradle sürümlerinin doğru sürümlerini başvurmak ve yüklemek için [Bu yönergeleri](https://developer.android.com/studio/releases/gradle-plugin) izleyin.
   > * Android SDK için lisans sözleşmesi imzalanmadı. Lisans sözleşmesini imzalamak ve SDK 'Yı indirmek için derleme çıktısındaki yönergeleri izleyin.

4. Oluşturma işlemi tamamlandıktan sonra, **Run**  >  **' uygulama '** çalıştırmasını Çalıştır ' a tıklayın. Uygulamayı fiziksel bir Android cihazda veya bir Android öykünücüsünde çalışacak şekilde yapılandırın. Bir Android uygulamasını fiziksel bir cihazda veya Öykünücüde çalıştırma hakkında daha fazla bilgi için bkz. [uygulamanızı çalıştırma](https://developer.android.com/training/basics/firstapp/running-app).

5. Uygulama yüklendikten sonra, IoT Hub telemetri göndermeye başlamak için **Başlat** düğmesine tıklayın:

    ![Uygulama](media/quickstart-send-telemetry-android/sample-screenshot.png)


## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Bu bölümde, Android cihazı tarafından gönderilen cihaz iletilerini izlemek için [IoT uzantısıyla](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) birlikte Azure Cloud Shell kullanacaksınız.

1. Azure Cloud Shell'i kullanarak, IoT hub’ınızdan gelen iletilere bağlanmak ve bu iletileri okumak için aşağıdaki komutu çalıştırın:

   **Youriothubname**: aşağıdaki yer tutucuyu IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name {YourIoTHubName} --output table
    ```

    Aşağıdaki ekran görüntüsünde, IoT Hub 'ı Android cihaz tarafından gönderilen Telemetriyi aldığı için çıkış gösterilmektedir:

      ![Azure CLI kullanarak cihaz iletilerini okuma](media/quickstart-send-telemetry-android/read-data.png)
## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT Hub 'ı ayarlarsınız, bir cihaz kaydettiniz, bir Android uygulaması kullanarak hub 'a sanal telemetri gönderdiniz ve Azure Cloud Shell kullanarak hub 'ın Telemetriyi okuyaöğreneceksiniz.

Bir arka uç uygulamasından simülasyon cihazınızı denetlemeyi öğrenmek için sonraki hızlı başlangıçla devam edin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: IoT hub’a bağlı bir cihazı denetleme](quickstart-control-device-android.md)