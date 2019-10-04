---
title: 'Hızlı başlangıç: Java ile Azure IoT Hub telemetri gönderme'
description: Bu hızlı başlangıçta, bir IoT Hub 'ına sanal telemetri göndermek ve bulutta işlenmek üzere IoT Hub 'ından Telemetriyi okumak için iki örnek Java uygulaması çalıştırırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc, seo-java-august2019, seo-java-september2019
ms.date: 06/21/2019
ms.openlocfilehash: a808216b62459869e9adfd88afc60ee53259221d
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838630"
---
# <a name="quickstart-send-telemetry-to-an-azure-iot-hub-and-read-it-with-a-java-application"></a>Hızlı başlangıç: Azure IoT Hub 'ına telemetri gönderin ve Java uygulamasıyla okuyun

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

Hızlı başlangıç, bir Azure IoT Hub 'ına telemetri gönderme ve Java uygulamasıyla okuma işlemlerinin nasıl yapılacağını gösterir. IoT Hub, IoT cihazlarınızdan depolama ya da işleme için buluta kadar yüksek miktarda telemetri almanıza olanak sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, IoT Hub aracılığıyla sanal bir cihaz uygulamasından, işleme için bir arka uç uygulamasına telemetri gönderirsiniz.

Hızlı başlangıç, bir tane önceden yazılmış iki Java uygulaması kullanır, bunlardan biri Telemetriyi ve bir hub 'dan Telemetriyi okumak üzere bir tane. Bu iki uygulamayı çalıştırmadan önce bir IoT Hub oluşturun ve bir cihazı hub 'a kaydedersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Prerequisites

Bu hızlı başlangıçta çalıştırdığınız iki örnek uygulama Java kullanılarak yazılmıştır. Geliştirme makinenizde Java, 8 gerekir.

Birden çok platform için Java SE Development Kit 8 [' i, Azure ve Azure Stack için uzun süreli destekten](https://docs.microsoft.com/en-us/java/azure/jdk/?view=azure-java-stable)indirebilirsiniz. JDK 8 için karşıdan yüklemeye ulaşmak üzere **uzun süreli destek** altında **Java 8** ' i seçtiğinizden emin olun.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Java sürümünü doğrulayabilirsiniz:

```cmd/sh
java -version
```

Örnekleri derlemek için Maven 3 ' ü yüklemeniz gerekir. Maven 'yi [Apache Maven](https://maven.apache.org/download.cgi)'ten birden çok platform için indirebilirsiniz.

Aşağıdaki komutu kullanarak, geliştirme makinenizde güncel Maven sürümünü doğrulayabilirsiniz:

```cmd/sh
mvn --version
```

Azure CLı için Microsoft Azure IoT uzantısını Cloud Shell örneğinize eklemek için aşağıdaki komutu çalıştırın. IOT uzantısı, Azure CLı 'ye IoT Hub, IoT Edge ve IoT cihaz sağlama hizmeti 'ne (DPS) özel komutlar ekler.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Örnek Java projesini https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip ' dan indirin ve ZIP arşivini ayıklayın.

## <a name="create-an-iot-hub"></a>IoT Hub 'ı oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Bir cihazı kaydetme

Bir cihazın bağlanabilmesi için IoT Hub 'ınız ile kayıtlı olması gerekir. Bu hızlı başlangıçta, sanal cihazı kaydetmek için Azure Cloud Shell kullanırsınız.

1. Cihaz kimliğini oluşturmak için Azure Cloud Shell aşağıdaki komutu çalıştırın.

   **Youriothubname**: Bu yer tutucuyu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

   **Myjavadevice**: kayıt yaptığınız cihazın adı. Gösterilen **Myjavadevice** ' i kullanın. Cihazınız için farklı bir ad seçerseniz bu adı bu makale boyunca kullanmanız ve örnek uygulamalarda cihaz adını çalıştırmadan önce güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyJavaDevice
    ```

2. Yeni kaydettiğiniz cihaza yönelik _Cihaz bağlantı dizesini_ almak için Azure Cloud Shell ' de aşağıdaki komutları çalıştırın: * * YourIoTHubName: Bu yer tutucuyu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyJavaDevice --output table
    ```

    Cihaz bağlantı dizesini bir yere göz önünde bir şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri daha sonra hızlı başlangıçta kullanırsınız.

3. Ayrıca, arka uç uygulamasının IoT Hub 'ınıza bağlanmasını ve iletileri almanızı sağlamak için IoT Hub 'ınızdaki _Event Hubs uyumlu uç nokta_, _Event Hubs uyumlu yol_ve _hizmet birincil anahtarı_ gerekir. Aşağıdaki komutlar IoT Hub 'ınız için bu değerleri alır:

     \* * YourIoTHubName: Bu yer tutucuyu, IoT Hub 'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name YourIoTHubName

    az iot hub show --query properties.eventHubEndpoints.events.path --name YourIoTHubName

    az iot hub policy show --name service --query primaryKey --hub-name YourIoTHubName
    ```

    Bu üç değeri daha sonra hızlı başlangıçta kullanacağınız şekilde bir yere unutmayın.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönder

Sanal cihaz uygulaması, IoT Hub 'ınızdaki cihaza özgü bir uç noktaya bağlanır ve sanal sıcaklık ve nem telemetrisini gönderir.

1. Yerel bir Terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Ardından **iot-hub\Quickstarts\simulated-Device** klasörüne gidin.

2. **Src/Main/Java/com/Microsoft/docs/ıothub/Samples/SimulatedDevice. Java** dosyasını istediğiniz bir metin düzenleyicisinde açın.

    @No__t-0 değişkeninin değerini, daha önce bir örneği yaptığınız cihaz bağlantı dizesiyle değiştirin. Ardından **SimulatedDevice. Java** dosyasına yaptığınız değişiklikleri kaydedin.

3. Yerel Terminal penceresinde, gerekli kitaplıkları yüklemek ve sanal cihaz uygulamasını derlemek için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    mvn clean package
    ```

4. Yerel Terminal penceresinde, sanal cihaz uygulamasını çalıştırmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    java -jar target/simulated-device-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsünde, sanal cihaz uygulamasının IoT Hub 'ınıza telemetri gönderdiği çıkış gösterilmektedir:

    ![Cihazın IoT Hub 'ınıza gönderdiği telemetriden çıkış](media/quickstart-send-telemetry-java/iot-hub-simulated-device.png)

## <a name="read-the-telemetry-from-your-hub"></a>Hub 'ınızdaki Telemetriyi okuyun

Arka uç uygulaması, IoT Hub hizmet tarafı **olayları** uç noktasına bağlanır. Uygulama, sanal cihazınızdan gönderilen cihazdan buluta iletileri alır. Bir IoT Hub arka uç uygulaması, cihazdan buluta iletileri almak ve işlemek için genellikle bulutta çalışır.

1. Başka bir yerel Terminal penceresinde, örnek Java projesinin kök klasörüne gidin. Ardından **iot-hub\Quickstarts\read-D2C-messages** klasörüne gidin.

2. **Src/Main/Java/com/Microsoft/docs/ıothub/Samples/ReadDeviceToCloudMessages. Java** dosyasını istediğiniz bir metin düzenleyicisinde açın. Aşağıdaki değişkenleri güncelleştirin ve değişiklikleri dosyaya kaydedin.

    | Değişken | Değer |
    | -------- | ----------- |
    | `eventHubsCompatibleEndpoint` | Değişkenin değerini, daha önce bir notunuz yaptığınız Event Hubs uyumlu uç noktayla değiştirin. |
    | `eventHubsCompatiblePath`     | Değişkenin değerini, daha önce bir yer yaptığınız Event Hubs uyumlu yol ile değiştirin. |
    | `iotHubSasKey`                | Değişkenin değerini, daha önce bir örneği yaptığınız hizmet birincil anahtarıyla değiştirin. |

3. Yerel Terminal penceresinde, gerekli kitaplıkları yüklemek ve arka uç uygulamasını derlemek için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    mvn clean package
    ```

4. Yerel Terminal penceresinde, arka uç uygulamasını çalıştırmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    java -jar target/read-d2c-messages-1.0.0-with-deps.jar
    ```

    Aşağıdaki ekran görüntüsünde, arka uç uygulamasının hub 'a sanal cihaz tarafından gönderilen Telemetriyi aldığından, çıkış gösterilmektedir:

    ![Arka uç uygulaması olarak çıktı, IoT Hub 'ınıza gönderilen telemetri alır](media/quickstart-send-telemetry-java/iot-hub-read-device-to-cloud.png)

## <a name="clean-up-resources"></a>Kaynakları Temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT Hub 'ı oluşturdunuz, bir cihaz kaydettiniz, bir Java uygulaması kullanarak hub 'a sanal telemetri gönderdiniz ve basit bir arka uç uygulaması kullanarak hub 'ın Telemetriyi okuyatamamladınız.

Bir arka uç uygulamasından sanal cihazınızı nasıl denetleyeceğinizi öğrenmek için sonraki hızlı başlangıca geçin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: IoT Hub 'ına bağlı bir cihazı denetleme](quickstart-control-device-java.md)
