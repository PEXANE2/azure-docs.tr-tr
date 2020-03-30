---
title: Azure IoT Hub (iOS) ile buluttan cihaza iletiler | Microsoft Dokümanlar
description: iOS için Azure IoT SDK'larını kullanarak bir Azure IoT hub'ından bir aygıta buluttan aygıta ileti gönderme.
author: kgremban
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: kgremban
ms.openlocfilehash: 544b1108a3c79da50969f7fdd33cfb9af373d854
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110872"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-ios"></a>IoT Hub (iOS) ile buluttan aygıta iletiler gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub, milyonlarca aygıt ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlamaya yardımcı olan tam olarak yönetilen bir hizmettir. Bir [aygıttan IoT hub'ına hızlı başlat'a gönder telemetrisi,](quickstart-send-telemetry-ios.md) bir IoT hub'ı oluşturmanın, içinde aygıt kimliğini nasıl sağlayabilirsiniz ve aygıttan buluta iletigönderen simüle edilmiş bir aygıt uygulamasını kodlamayı gösterir.

Bu öğretici şunların nasıl yapıldığını gösterir:

* Çözümünüzden arka uçtan, IoT Hub üzerinden tek bir cihaza buluttan cihaza iletigönderin.

* Bir aygıttan buluttan aygıta iletiler alın.

* Çözümünüzden arka uçtan, IoT Hub'dan bir cihaza gönderilen iletiler için teslimat bildirimi *(geri bildirim)* isteyin.

[IoT Hub geliştirici kılavuzunun ileti bölümünde](iot-hub-devguide-messaging.md)buluttan cihaza iletiler hakkında daha fazla bilgi bulabilirsiniz.

Bu makalenin sonunda, iki Swift iOS projesi çalıştırırsınız:

* **örnek cihaz**, [bir cihazdan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-ios.md)oluşturulan ve IoT hub'ınıza bağlanan ve buluttan cihaza iletiler alan aynı uygulamadır.

* ioT Hub üzerinden simüle edilen aygıt uygulamasına buluttan aygıta ileti gönderen ve ardından teslim bildirimini alan **örnek hizmet.**

> [!NOTE]
> IoT Hub, Azure IoT aygıt SDK'ları aracılığıyla birçok aygıt platformu ve dili (C, Java, Python ve Javascript dahil) için SDK desteğine sahiptir. Cihazınızı bu öğreticinin koduna ve genellikle Azure IoT Hub'ına nasıl bağlayacaklarına ilişkin adım adım talimatlar için [Azure IoT Geliştirici Merkezi'ne](https://www.azure.com/develop/iot)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* Azure'da etkin bir IoT hub'ı.

* [Azure örneklerinden](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip)kod örneği.

* En son iOS SDK sürümünü çalıştıran en yeni [XCode](https://developer.apple.com/xcode/) sürümü. Bu hızlı başlangıç XCode 9.3 ve iOS 11.3 ile test edilmiştir.

* [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)'un en son sürümü.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="simulate-an-iot-device"></a>Bir IoT aygıtını simüle etme

Bu bölümde, IoT hub'ından buluttan aygıta iletialmak için Swift uygulaması çalıştıran bir iOS aygıtını simüle emersiniz. 

Bu, makalede oluşturduğunuz örnek aygıtbir [aygıttan IoT hub'ına telemetri gönder.](quickstart-send-telemetry-ios.md) Zaten çalışıyorsanız, bu bölümü atlayabilirsiniz.

### <a name="install-cocoapods"></a>CocoaPods yükleme

CocoaPods, üçüncü taraf kitaplıklar kullanan iOS projeleri için bağımlılıkları yönetir.

Bir terminal penceresinde, önkoşulların bir parçası olarak indirdiğiniz Azure-IoT-Samples-iOS klasörüne gidin. Ardından örnek projeye gidin:

```sh
cd quickstart/sample-device
```

XCode’un kapalı olduğundan emin olun ve **podfile** dosyasında bildirilen CocoaPods’u yüklemek üzere aşağıdaki komutu çalıştırın:

```sh
pod install
```

Yükleme komutu, projeniz için gereken podları yüklemeye ek olarak bağımlılıklar için podları kullanacak şekilde önceden yapılandırılmış bir XCode çalışma alanı dosyası da oluşturmuştur.

### <a name="run-the-sample-device-application"></a>Örnek cihaz uygulamasını çalıştırma

1. Cihazınızın bağlantı dizesini alın. Bu dizeyi aygıt ayrıntıları bıçağındaki [Azure portalından](https://portal.azure.com) kopyalayabilir veya aşağıdaki CLI komutuyla geri alabilirsiniz:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id {YourDeviceID} --output table
    ```

1. Örnek çalışma alanını XCode'da açın.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. **MQTT İstemci Örnek** projesini ve ardından aynı adı taşıyan klasörü genişletin.  

3. XCode’da düzenlemek üzere **ViewController.swift** dosyasını açın. 

4. **ConnectionString** değişkenini arayın ve ilk adımda kopyaladığınız aygıt bağlantı dizesi ile değeri güncelleştirin.

5. Yaptığınız değişiklikleri kaydedin. 

6. **Derle ve çalıştır** düğmesi ya da **command + r** tuş birleşimi ile projeyi cihaz öykünücüsünde çalıştırın.

   ![Projeyi çalıştırma](media/iot-hub-ios-swift-c2d/run-sample.png)

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-ios.md)oluşturduğunuz IoT hub'ı üzerinden buluttan aygıta iletigöndermek için bir arka uç hizmeti oluşturursunuz. Buluttan cihaza ileti göndermek için hizmetinizin **hizmet bağlantısı** namına ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="simulate-a-service-device"></a>Bir servis aygıtını simüle edin

Bu bölümde, IoT hub üzerinden buluttan cihaza ileti gönderen Swift uygulamasına sahip ikinci bir iOS aygıtını simüle emletebilirsiniz. Bu yapılandırma, bir IOT hub'ına bağlı diğer iOS aygıtları için denetleyici olarak çalışan bir iPhone veya iPad'in bulunduğu IoT senaryoları için yararlıdır.

### <a name="install-cocoapods"></a>CocoaPods yükleme

CocoaPods, üçüncü taraf kitaplıklar kullanan iOS projeleri için bağımlılıkları yönetir.

Ön koşullarda indirdiğiniz Azure IoT iOS Örnekleri klasörüne gidin. Ardından, örnek servis projesine gidin:

```sh
cd quickstart/sample-service
```

XCode’un kapalı olduğundan emin olun ve **podfile** dosyasında bildirilen CocoaPods’u yüklemek üzere aşağıdaki komutu çalıştırın:

```sh
pod install
```

Yükleme komutu, projeniz için gereken podları yüklemeye ek olarak bağımlılıklar için podları kullanacak şekilde önceden yapılandırılmış bir XCode çalışma alanı dosyası da oluşturmuştur.

### <a name="run-the-sample-service-application"></a>Örnek servis uygulamasını çalıştırma

1. Örnek çalışma alanını XCode'da açın.

   ```sh
   open AzureIoTServiceSample.xcworkspace
   ```

2. **AzureIoTServiceSample** projesini genişletin ve ardından aynı adı taşıyan klasörü genişletin.  

3. XCode’da düzenlemek üzere **ViewController.swift** dosyasını açın. 

4. **BağlantıString** değişkenini arayın ve [IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)daha önce kopyaladığınız servis bağlantı dizesi ile değeri güncelleştirin.

5. Yaptığınız değişiklikleri kaydedin.

6. Xcode'da, emülatör ayarlarını IoT aygıtını çalıştırmak için kullandığınızdan farklı bir iOS aygıtıyla değiştirin. XCode aynı türden birden fazla emülatör çalıştıramaz.

   ![Emülatör aygıtını değiştirme](media/iot-hub-ios-swift-c2d/change-device.png)

7. **Yapı ve çalıştır** düğmesi veya anahtar açılan Komut + **r**ile cihaz emülatörü nde proje çalıştırın.

   ![Projeyi çalıştırma](media/iot-hub-ios-swift-c2d/run-app.png)

## <a name="send-a-cloud-to-device-message"></a>Buluttan aygıta ileti gönderme

Artık buluttan cihaza ileti göndermek ve almak için iki uygulamayı kullanmaya hazırsınız.

1. Simüle edilmiş IoT aygıtında çalışan **iOS Uygulama Örneği** uygulamasında **Başlat'ı**tıklatın. Uygulama aygıttan buluta iletiler göndermeye başlar, ancak buluttan aygıta iletileri dinlemeye de başlar.

   ![Örnek IoT cihaz uygulamasını görüntüleyin](media/iot-hub-ios-swift-c2d/view-d2c.png)

2. Benzetimli hizmet aygıtında çalışan **IoTHub Service Client Sample** uygulamasında, ileti göndermek istediğiniz IoT aygıtının kimliğini girin. 

3. Düz metin iletisi yazın, ardından **Gönder'i**tıklatın.

    Gönder'i tıklattığınızda çeşitli eylemler gerçekleşir. Hizmet örneği, sağladığınız servis bağlantısı dizesi nedeniyle uygulamanın erişebildiği IoT hub'ınıza iletiyi gönderir. IoT hub'ınız aygıt kimliğini denetler, iletiyi hedef aygıta gönderir ve kaynak aygıta bir onay makbuzu gönderir. Simüle edilmiş IoT aygıtınızda çalışan uygulama, IoT Hub'dan gelen iletileri denetler ve ekrandaki en son mesajdaki metni yazdırır.

    Çıktınız aşağıdaki örnek gibi görünmelidir:

   ![Buluttan cihaza iletileri görüntüleme](media/iot-hub-ios-swift-c2d/view-c2d.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, buluttan cihaza ileti göndermeyi ve almayı öğrendiniz.

IoT Hub'ı kullanan eksiksiz uçtan uca çözümlerin örneklerini görmek için [Azure IoT Çözüm Hızlandırıcıları](https://azure.microsoft.com/documentation/suites/iot-suite/) belgelerine bakın.

IoT Hub ile çözüm geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.
