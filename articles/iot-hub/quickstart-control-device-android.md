---
title: Azure IoT Hub quickstart (Android) | Microsoft Dokümanlar
description: Bu hızlı başlangıçta iki örnek Java uygulaması çalıştıracaksınız. Uygulamalardan biri, hub'ınıza bağlı aygıtları uzaktan denetlenebilen bir hizmet uygulamasıdır. Diğer uygulama, hub'ınıza uzaktan kontrol edilebilen fiziksel veya simüle edilmiş bir aygıtta çalışır.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/21/2019
ms.author: wesmc
ms.openlocfilehash: 5a912199869cb2cce690a3502345ec6f9ca43d22
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675683"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-android"></a>Hızlı başlatma: Bir IoT hub'ına (Android) bağlı bir aygıtı denetleme

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

Bu hızlı başlatmada, Azure IoT Hub'ına bağlı simüle edilmiş bir aygıtı denetlemek için doğrudan bir yöntem kullanırsınız. IoT Hub, IoT aygıtlarınızı buluttan yönetmenize ve depolama veya işleme için buluta yüksek hacimli aygıt telemetrisi almanızı sağlayan bir Azure hizmetidir. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz. Bu hızlı başlatma iki uygulama kullanır: arka uç servis uygulamasından çağrılan doğrudan yöntemlere yanıt veren simüle edilmiş bir aygıt uygulaması ve Android cihazındaki doğrudan yöntemi çağıran bir servis uygulaması.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Android SDK 27 ile Android Studio](https://developer.android.com/studio/). Daha fazla bilgi için [Android Studio Yükle'ye](https://developer.android.com/studio/install)bakın.

* [Git.](https://git-scm.com/download/)

* [Azure IoT Örnekleri (Java)](https://github.com/Azure-Samples/azure-iot-samples-java)dahil [cihaz SDK örnek Android uygulaması.](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/device/AndroidSample)

* [Hizmet SDK örnek Android uygulaması](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Samples/service/AndroidSample), Azure IoT Örnekleri (Java) dahil.

* Port 8883 güvenlik duvarınızda açılır. Bu hızlı başlatmadaki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="add-azure-iot-extension"></a>Azure IoT Uzantısı Ekle

Bulut Kabuğu örneğinize Azure CLI için Microsoft Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IoT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) özel komutlarını ekler.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [Quickstart'ı tamamladıysanız: Bir aygıttan IoT hub'ına telemetri gönderin,](quickstart-send-telemetry-android.md)bu adımı atlayabilir ve zaten oluşturduğunuz IoT hub'ını kullanabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki [Quickstart'ı tamamladıysanız: Bir aygıttan IoT hub'ına telemetri gönderin,](quickstart-send-telemetry-android.md)bu adımı atlayabilir ve önceki quickstart'ta kayıtlı aynı aygıtı kullanabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aygıt kimliğini oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyAndroidDevice**: Bu, kaydettiğiniz cihazın adıdır. Gösterildiği gibi **MyAndroidDevice** kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanmanız ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name {YourIoTHubName} --device-id MyAndroidDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name {YourIoTHubName} \
      --device-id MyAndroidDevice \
      --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyAndroidDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="retrieve-the-service-connection-string"></a>Hizmet bağlantı dizesini alma

Ayrıca, yöntemleri yürütmek ve iletileri almak için arka uç servis uygulamalarının IoT hub'ınıza bağlanmasını sağlamak için bir _hizmet bağlantısı dizesi_ gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

**YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

```azurecli-interactive
az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
```

Şu ifadeye benzer şekilde görünen hizmet bağlantı dizesini not edin:

`HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Bu hizmet bağlantı dizesi, önceki adımda belirttiğiniz aygıt bağlantı dizesinden farklıdır.

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Bu hızlı başlatma için alınan örneklerin her ikisi de GitHub'daki azure-iot-samples-java deposunun bir parçasıdır. [Azure-iot-örnekleri-java](https://github.com/Azure-Samples/azure-iot-samples-java) deposunu indirin veya klonla.

Cihaz SDK örnek uygulaması fiziksel bir Android cihaz veya Android emülatörü üzerinde çalıştırılabilir. Örnek, IoT hub'ınızdaki cihaza özgü bir uç noktaya bağlanır, simüle edilmiş telemetri gönderir ve hub'ınızdan doğrudan yöntem çağrıları için dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Benzetimli aygıt, doğrudan yöntemi çalıştırdıktan sonra hub'ınıza bir bildirim gönderir.

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

    ![İstemci cihaz android uygulaması örnek ekran görüntüsü](media/quickstart-control-device-android/sample-screenshot.png)

Çalışma süresi sırasında telemetri aralığını güncelleştirmek için hizmet SDK örneğini çalıştırırken bu uygulamanın fiziksel bir aygıtveya emülatörüzerinde çalışmaya bırakılması gerekir.

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Bu bölümde, Android cihazı tarafından gönderilen iletileri izlemek için [IoT uzantılı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Azure Bulut Kabuğu'nu kullanacaksınız.

1. Azure Cloud Shell'i kullanarak, IoT hub’ınızdan gelen iletilere bağlanmak ve bu iletileri okumak için aşağıdaki komutu çalıştırın:

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name {YourIoTHubName} --output table
    ```

    Aşağıdaki ekran görüntüsü, IoT hub'ı Android cihazı tarafından gönderilen telemetriyi aldığında çıktıyı gösterir:

      ![Azure CLI kullanarak cihaz iletilerini okuma](media/quickstart-control-device-android/read-data.png)

Varsayılan olarak, telemetri uygulaması Android cihazdan her beş saniyede bir telemetri gönderir. Bir sonraki bölümde, Android IoT cihazı için telemetri aralığını güncellemek için doğrudan bir yöntem çağrısı kullanacaksınız.

## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Hizmet uygulaması, IoT Hub'ınızdaki servis tarafındaki bitiş noktasına bağlanır. Uygulama, IoT hub'ınız aracılığıyla bir cihaza doğrudan yöntem aramaları yapar ve bildirimleri dinler.

Bu uygulamayı ayrı bir fiziksel Android cihazda veya Android emülatörde çalıştırın.

Bir IoT Hub arka uç hizmet uygulaması genellikle bulutta çalışır ve burada bir IoT Hub'daki tüm aygıtları kontrol eden hassas bağlantı dizesiyle ilişkili riskleri azaltmanın daha kolay olduğu bir uygulamadır. Bu örnekte, biz sadece gösteri amaçlı bir Android uygulaması olarak çalıştırıyoruz. Bu hızlı başlatmanın diğer dil sürümleri, tipik bir arka uç hizmet uygulamasıyla daha yakından hizalayan örnekler sağlar.

1. Android Studio'da GitHub servis örnek Android projesini açın. Proje, [azure-iot-sample-java](https://github.com/Azure-Samples/azure-iot-samples-java) deposunun klonlanmış veya indirilen kopyasının aşağıdaki dizininde yer alır.

        \azure-iot-samples-java\iot-hub\Samples\service\AndroidSample

2. Android Studio'da, örnek proje için *gradle.properties'i* açın. Daha önce belirttiğiniz servis bağlantı dizesiyle ve kaydettiğiniz Android cihaz kimliğiyle **ConnectionString** ve **DeviceId** özelliklerinin değerlerini güncelleştirin.

    ```
    ConnectionString=HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}
    DeviceId=MyAndroidDevice
    ```

3. Android Studio'da, Gradle Dosyalarıyla **Dosya** > **Eşitleme Projesi'ni**tıklatın. Yapının tamamlanın.

   > [!NOTE]
   > Proje eşitlemesi başarısız olursa, aşağıdaki nedenlerden biri olabilir:
   >
   > * Projede başvurulan Android Gradle eklentisi ve Gradle sürümleri Android Studio sürümünüz için güncel değildir. Yüklemeniz için eklenti ve Gradle'nin doğru sürümlerine başvurmak ve yüklemek için [bu yönergeleri](https://developer.android.com/studio/releases/gradle-plugin) izleyin.
   > * Android SDK için lisans sözleşmesi imzalanmadı. Lisans sözleşmesini imzalamak ve SDK'yı indirmek için Yapı çıktısındaki yönergeleri izleyin.

4. Yapı tamamlandıktan sonra **Çalıştır** > **'uygulamasını'** tıklatın. Uygulamayı ayrı bir fiziksel Android cihazda veya Android emülatörde çalışacak şekilde yapılandırın. Bir Android uygulamasını fiziksel bir cihazda veya emülatörde çalıştırma hakkında daha fazla bilgi [için](https://developer.android.com/training/basics/firstapp/running-app)bkz.

5. Uygulama yüklendikten **sonra, İleti Aralığını Ayarla** değerini **1000'e** güncelleyin ve **Çağır'ı**tıklatın.

    Th telemetri mesajlaşma aralığı milisaniye cinsindendir. Aygıt örneğinin varsayılan telemetri aralığı 5 saniye olarak ayarlanır. Bu değişiklik, Android IoT cihazını günceller, böylece her saniye telemetri gönderilir.

    ![Telemetri aralığını girin](media/quickstart-control-device-android/enter-telemetry-interval.png)

6. Uygulama, yöntemin başarılı bir şekilde yürütülüp yürütülmediğini belirten bir bildirim alır.

    ![Doğrudan Yöntem bildirimi](media/quickstart-control-device-android/direct-method-ack.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir arka uç uygulamasından aygıtta doğrudan bir yöntem çağırdınız ve benzetimli bir aygıt uygulamasında doğrudan yöntem çağrısına yanıt verdiniz.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Telemetriyi işlenmek üzere farklı uç noktalara yönlendirme](tutorial-routing.md)
