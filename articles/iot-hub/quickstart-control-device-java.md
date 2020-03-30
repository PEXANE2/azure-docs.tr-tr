---
title: "Hızlı başlatma: Java ile Azure IoT Hub'ından bir aygıtı denetleme"
description: Bu hızlı başlangıçta iki örnek Java uygulaması çalıştıracaksınız. Bir uygulama, hub’ınıza bağlı cihazları uzaktan denetleyebilen bir arka uç uygulamasıdır. Diğer uygulama, uzaktan denetlenebilen hub’ınıza bağlanan bir cihazın simülasyonunu yapar.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc, seo-java-august2019, seo-java-september2019
ms.date: 06/21/2019
ms.openlocfilehash: 96d30d283ec1565e512be77f55d15305fa226db4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675359"
---
# <a name="quickstart-control-a-device-connected-to-an-azure-iot-hub-with-java"></a>Hızlı başlatma: Java ile Azure IoT hub'ına bağlı bir aygıtı denetleme

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

Bu hızlı başlatmada, Azure IoT Hub'ına bağlı bir aygıtı Java uygulamasıyla denetlemek için doğrudan bir yöntem kullanırsınız. IoT Hub, IoT aygıtlarınızı buluttan yönetmenize ve depolama veya işleme için buluta yüksek hacimli aygıt telemetrisi almanızı sağlayan bir Azure hizmetidir. IoT hub’ınıza bağlı bir cihazın davranışını uzaktan değiştirmek için doğrudan yöntemler kullanabilirsiniz. Bu hızlı başlatma iki Java uygulaması kullanır: arka uç uygulamasından çağrılan doğrudan yöntemlere yanıt veren simüle edilmiş bir aygıt uygulaması ve benzetimli aygıtta doğrudan yöntemi çağıran bir hizmet uygulaması.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* Java SE Geliştirme Kiti 8. [Azure ve Azure Yığını için Java uzun vadeli desteğinde](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable), Uzun süreli **destek**altında, **Java 8'i**seçin.

* [Apaçi Maven 3](https://maven.apache.org/download.cgi).

* [Örnek bir Java projesi.](https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip)

* Port 8883 güvenlik duvarınızda açılır. Bu hızlı başlatmadaki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Java sürümünü doğrulayabilirsiniz:

```cmd/sh
java -version
```

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Maven sürümünü doğrulayabilirsiniz:

```cmd/sh
mvn --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="add-azure-iot-extension"></a>Azure IoT Uzantısı Ekle

Bulut Kabuğu örneğinize Azure CLI için Microsoft Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IoT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) özel komutlarını ekler.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-java.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Önceki [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-java.md) öğreticisini tamamladıysanız bu adımı atlayabilirsiniz.

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu hızlı başlangıçta Azure Cloud Shell kullanarak bir simülasyon cihazı kaydedeceksiniz.

1. Aygıt kimliğini oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyJavaDevice**: Bu, kaydettiğiniz aygıtın adıdır. Gösterildiği gibi **MyJavaDevice** kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanmanız ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name {YourIoTHubName} --device-id MyJavaDevice
    ```

2. Yeni kaydettiğiniz cihazın _cihaz bağlantı dizesini_ almak için aşağıdaki komutları Azure Cloud Shell'de çalıştırın:

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name {YourIoTHubName} \
      --device-id MyJavaDevice \
      --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız.

## <a name="retrieve-the-service-connection-string"></a>Hizmet bağlantı dizesini alma

Arka uç uygulamasının IoT hub’ınıza bağlanmasına ve iletileri almasına olanak sağlamak için bir _hizmet bağlantı dizesi_ de gerekir. Aşağıdaki komut, IoT hub'ınız için hizmeti bağlantı dizesini alır:

**YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

```azurecli-interactive
az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
```

Şu ifadeye benzer şekilde görünen hizmet bağlantı dizesini not edin:

`HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Bu değeri hızlı başlangıcın ilerleyen bölümlerinde kullanacaksınız. Bu hizmet bağlantı dizesi, önceki adımda belirttiğiniz aygıt bağlantı dizesinden farklıdır.

## <a name="listen-for-direct-method-calls"></a>Doğrudan yöntem çağrılarını dinleme

Simülasyon cihazı, IoT hub’ınızdaki cihaza özgü bir uç noktaya bağlanır, sanal telemetri gönderir ve hub’ınızdan gelen doğrudan yöntem çağrılarını dinler. Bu hızlı başlangıçta, hub’dan gelen doğrudan yöntem çağrısı, telemetri gönderme aralığını değiştirmesini cihaza bildirir. Benzetimli aygıt, doğrudan yöntemi çalıştırdıktan sonra hub'ınıza bir bildirim gönderir.

1. Yerel terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\simulated-device-2** klasörüne gidin.

2. **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** dosyasını istediğiniz bir metin düzenleyicide açın.

    `connString` Değişkenin değerini daha önce not aldığınız aygıt bağlantı dizesiyle değiştirin. Ardından değişikliklerinizi **SimulatedDevice.java'ya**kaydedin.

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve simülasyon cihazı uygulamasını derleyin:

    ```cmd/sh
    mvn clean package
    ```

4. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak simülasyon cihazı uygulamasını çalıştırın:

    ```cmd/sh
    java -jar target/simulated-device-2-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Cihaz tarafından IoT hub'ınıza gönderilen telemetriden çıkış](./media/quickstart-control-device-java/iot-hub-application-send-telemetry-output.png)

## <a name="call-the-direct-method"></a>Doğrudan yöntem çağırma

Arka uç uygulaması, IoT Hub’ınızdaki bir hizmet tarafı uç noktasına bağlanır. Uygulama, IoT hub'ınız aracılığıyla bir cihaza doğrudan yöntem aramaları yapar ve bildirimleri dinler. IoT Hub arka uç uygulaması genellikle bulutta çalışır.

1. Başka bir yerel terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Daha sonra **iot-hub\Quickstarts\back-end-application** klasörüne gidin.

2. **src/main/java/com/microsoft/docs/iothub/samples/BackEndApplication.java** dosyasını istediğiniz bir metin düzenleyicide açın.

    `iotHubConnectionString` Değişkenin değerini daha önce not aldığınız servis bağlantısı dizesiyle değiştirin. Ardından değişikliklerinizi **BackEndApplication.java'ya**kaydedin.

3. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak gerekli kitaplıkları yükleyin ve arka uç uygulamasını derleyin:

    ```cmd/sh
    mvn clean package
    ```

4. Yerel terminal penceresinde, aşağıdaki komutları çalıştırarak arka uç uygulamasını çalıştırın:

    ```cmd/sh
    java -jar target/back-end-application-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsü, uygulama cihaza doğrudan bir yöntem çağrısı yaptığı ve bir bildirim aldığında çıktıyı gösterir:

    ![Uygulama IoT hub'ınız aracılığıyla doğrudan bir yöntem çağrısı yaptığında çıktı](./media/quickstart-control-device-java/iot-hub-direct-method-call-output.png)

    Arka uç uygulamasını çalıştırdıktan sonra, simülasyon cihazını çalıştıran konsol penceresinde bir ileti ve ileti değişikliklerini gönderdiği hızı görürsünüz:

    ![Aygıttan gelen konsol iletisi, değiştirme hızını gösterir](./media/quickstart-control-device-java/iot-hub-sent-message-change-rate.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir arka uç uygulamasından aygıtta doğrudan bir yöntem çağırdınız ve benzetimli bir aygıt uygulamasında doğrudan yöntem çağrısına yanıt verdiniz.

Cihazdan buluta iletileri, buluttaki farklı hedeflere yönlendirmeyi öğrenmek için sonraki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Telemetriyi işlenmek üzere farklı uç noktalara yönlendirme](tutorial-routing.md)
