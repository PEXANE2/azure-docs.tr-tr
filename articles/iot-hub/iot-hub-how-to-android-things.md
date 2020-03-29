---
title: Azure IoT SDK'ları kullanarak Android Things platformu için geliştirin | Microsoft Dokümanlar
description: Geliştirici kılavuzu - Azure IoT Hub SDK'larını kullanarak Android Things'te nasıl geliştirileceğimiz hakkında bilgi edinin.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: robinsh
ms.openlocfilehash: a06583e9aab4b082517d47c1022f7bec5184b9bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673390"
---
# <a name="develop-for-android-things-platform-using-azure-iot-sdks"></a>Azure IoT SDK'ları kullanarak Android Things platformu için geliştirin

[Azure IoT Hub SDK'ları,](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) Windows, Linux, OSX, MBED gibi popüler platformlar ve Android ve iOS gibi mobil platformlar için birinci katman desteği sağlar.  Java SDK, IoT dağıtımlarında daha fazla seçenek ve esneklik sağlama taahhüdümüzün bir parçası olarak [Android Things](https://developer.android.com/things/) platformlarını da destekler.  Geliştiriciler, Android Things işletim sisteminin cihaz tarafındaki avantajlarından yararlanırken, aynı anda bağlı olan milyonlarca cihaza ölçek sağlayan merkezi ileti merkezi hub olarak [Azure IoT Hub'ı](about-iot-hub.md) kullanabilir.

Bu öğretici, Azure IoT Java SDK'yı kullanarak Android Things'te aygıt tarafı uygulaması oluşturma adımlarını özetleyin.

## <a name="prerequisites"></a>Ön koşullar

* Android Things, Android Things işletim sistemi çalıştırılan donanımı destekledi.  Android Things işletim sistemi yanıp sönme hakkında [Android Things belgelerini](https://developer.android.com/things/get-started/kits#flash-at) takip edebilirsiniz.  Android Things cihazınızın klavye, ekran ve fare gibi temel çevre birimleriyle internete bağlı olduğundan emin olun.  Bu öğretici Raspberry Pi 3 kullanır.

* [Android Studio'nun](https://developer.android.com/studio/) en son sürümü

* [Git'in](https://git-scm.com/) en son sürümü

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aşağıdaki komutları Azure Cloud Shell'de çalıştırarak IoT Hub CLI uzantısını ekleyin ve cihaz kimliğini oluşturun.

   **YourIoTHubName**: Bu yer tutucusunu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyAndroidThingsDevice** : Kayıtlı cihaz için verilen addır. MyAndroidThingsDevice'ı gösterildiği gibi kullanın. Cihazınız için farklı bir ad seçerseniz bu makalenin geri kalan bölümünde aynı adı kullanmanız ve örnek uygulamaları çalıştırmadan önce bunlarda da cihaz adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az extension add --name azure-iot
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyAndroidThingsDevice
    ```

2. Yeni kaydettiğiniz aygıtın aygıt bağlantı *dizesini* almak için Azure Cloud Shell'de aşağıdaki komutları çalıştırın. Aşağıda `YourIoTHubName` IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyAndroidThingsDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidThingsDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="building-an-android-things-application"></a>Android Things uygulaması oluşturma

1. Bir Android Things uygulaması oluşturmanın ilk adımı Android Things cihazlarınıza bağlanmaktır. Android Things cihazınızı bir ekrana bağlayın ve internete bağlayın. Android Things, WiFi'a nasıl bağlanılailgili [belgeler](https://developer.android.com/things/get-started/kits) sağlar. Internet'e bağlandıktan sonra Ağlar altında listelenen IP adresini not alın.

2. Android Things cihazınıza yukarıda belirtilen IP adresiyle bağlanmak için [adb](https://developer.android.com/studio/command-line/adb) aracını kullanın. Terminalinizdeki bu komutu kullanarak bağlantıyı iki kez kontrol edin. Aygıtlarınızın "bağlı" olarak listelenmiş olduğunu görmeniz gerekir.

   ```
   adb devices
   ```

3. Android/Android Things için örneğimizi bu [depodan indirin](https://github.com/Azure-Samples/azure-iot-samples-java) veya Git'i kullanın.

   ```
   git clone https://github.com/Azure-Samples/azure-iot-samples-java.git
   ```

4. Android Studio'da Android Project'i "\azure-iot-samples-java\iot-hub\Samples\device\AndroidSample" adresinde açın.

5. gradle.properties dosyanızı açın ve "Device_connection_string" ile daha önce belirtildiği aygıt bağlantı dizenizi değiştirin.
 
6. Çalıştır - Hata Ayıklama'ya tıklayın ve bu kodu Android Things cihazlarınıza dağıtmak için cihazınızı seçin.

7. Uygulama başarıyla başlatıldığında, Android Things cihazınızda çalışan bir uygulama görebilirsiniz. Bu örnek uygulama rasgele oluşturulan sıcaklık okumaları gönderir.

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Verileri IoT hub'ınızdan alındıkça görüntüleyebilirsiniz. IoT Hub uzantısı IoT Hub’ınızdaki bir hizmet tarafı **Olaylar** uç noktasına bağlanabilir. Uzantı, simülasyon cihazınızdan gönderilen cihazdan buluta iletileri alır. IoT Hub arka uç uygulaması genellikle cihazdan buluta iletileri alıp işlemek için bulutta çalışır.

Aşağıdaki komutları Azure Cloud Shell'de çalıştırın, `YourIoTHubName` yerine IoT hub'ınızın adını yazın:

```azurecli-interactive
az iot hub monitor-events --device-id MyAndroidThingsDevice --hub-name YourIoTHubName
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub SDK'larını kullanarak [bağlantı ve güvenilir mesajlaşmayı nasıl yöneteceğimiz](iot-hub-reliability-features-in-sdks.md) hakkında bilgi edinin.
* iOS ve Android gibi [mobil platformlar için](iot-hub-how-to-develop-for-mobile-devices.md) nasıl geliştirileceğimiz hakkında bilgi edinin.
* [Azure IoT SDK platform desteği](iot-hub-device-sdk-platform-support.md)
