---
title: Azure IoT Hub (Java) ile buluttan cihaza iletiler | Microsoft Dokümanlar
description: Java için Azure IoT SDK'larını kullanarak bir Azure IoT hub'ından bir aygıta buluttan aygıta ileti gönderme. Simüle edilmiş bir aygıt uygulamasını, buluttan cihaza iletiler almak için ve buluttan cihaza iletigöndermek için bir arka uç uygulamasını değiştirmek için değiştirirsiniz.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: e16d0ed264f32746c11d89e88ea1e67f9383b773
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732526"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>IoT Hub (Java) ile buluttan cihaza iletiler gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub, milyonlarca aygıt ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlamaya yardımcı olan tam olarak yönetilen bir hizmettir. Bir [aygıttan IoT hub'ına hızlı başlat'a gönder telemetrisi,](quickstart-send-telemetry-java.md) bir IoT hub'ı oluşturmanın, içinde aygıt kimliğini nasıl sağlayabilirsiniz ve aygıttan buluta iletigönderen simüle edilmiş bir aygıt uygulamasını kodlamayı gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici, [bir aygıttan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-java.md)biryapılsa. Aşağıdakileri nasıl yapacağınızı gösterir:

* Çözümünüzden arka uçtan, IoT Hub üzerinden tek bir cihaza buluttan cihaza iletigönderin.

* Bir aygıttan buluttan aygıta iletiler alın.

* Çözümünüzden arka uçtan, IoT Hub'dan bir cihaza gönderilen iletiler için teslimat bildirimi *(geri bildirim)* isteyin.

[IoT Hub geliştirici kılavuzunda buluttan cihaza iletiler](iot-hub-devguide-messaging.md)hakkında daha fazla bilgi bulabilirsiniz.

Bu eğitimin sonunda, iki Java konsolu uygulaması çalıştırın:

* **simüle edilen aygıt,** [bir cihazdan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-java.md)oluşturulan uygulamanın değiştirilmiş bir sürümü , IoT hub'ınıza bağlanır ve buluttan cihaza iletiler alır.

* ioT Hub üzerinden simüle edilen aygıt uygulamasına buluttan cihaza ileti gönderen ve ardından teslim bildirimini alan **send-c2d-messages.**

> [!NOTE]
> IoT Hub, Azure IoT aygıt SDK'ları aracılığıyla birçok aygıt platformu ve dili (C, Java, Python ve Javascript dahil) için SDK desteğine sahiptir. Cihazınızı bu öğreticinin koduna ve genellikle Azure IoT Hub'ına nasıl bağlayacaklarına ilişkin adım adım talimatlar için [Azure IoT Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/iot)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Bir [aygıttan IoT hub](quickstart-send-telemetry-java.md) quickstart veya [IoT Hub](tutorial-routing.md) öğretici ile Yapılandırma ileti yönlendirme telemetri gönder tam bir çalışma sürümü.

* [Java SE Geliştirme Kiti 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable). JDK 8 için indirme almak için **Uzun vadeli destek** altında Java **8** seçtiğinizden emin olun.

* [Maven 3](https://maven.apache.org/download.cgi)

* Etkin bir Azure hesabı. Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="receive-messages-in-the-simulated-device-app"></a>Benzetimli cihaz uygulamasından ileti alma

Bu bölümde, IoT hub'ından buluttan aygıta iletiler almak için telemetri gönder'de oluşturduğunuz simüle edilmiş aygıt uygulamasını [bir aygıttan IoT](quickstart-send-telemetry-java.md) hub'ına değiştirirsiniz.

1. Bir metin düzenleyicisi kullanarak simulated-device\src\main\java\com\mycompany\app\App.java dosyasını açın.

2. Aşağıdaki **MessageCallback** sınıfını **App** sınıfının içine iç içe bir sınıf olarak ekleyin. Yürütme **execute** yöntemi, aygıt IoT Hub'ından bir ileti aldığında çağrılır. Bu örnekte, aygıt her zaman IoT hub'ına iletiyi tamamladığını belirtir:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. **AppMessageCallback** örneği oluşturmak için **ana** yöntemi değiştirin ve istemciyi aşağıdaki gibi açmadan önce **setMessageCallback** yöntemini arayın:

    ```java
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > Aktarım olarak MQTT veya AMQP yerine HTTPS kullanıyorsanız, **DeviceClient** örneği IoT Hub'ından gelen iletileri seyrek olarak denetler (her 25 dakikadan az). MQTT, AMQP ve HTTPS desteği ve IoT Hub azaltma arasındaki farklar hakkında daha fazla bilgi için [IoT Hub geliştirici kılavuzunun ileti bölümüne](iot-hub-devguide-messaging.md)bakın.

4. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-java.md)oluşturduğunuz IoT hub'ı üzerinden buluttan aygıta iletigöndermek için bir arka uç hizmeti oluşturursunuz. Buluttan cihaza ileti göndermek için hizmetinizin **hizmet bağlantısı** namına ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="send-a-cloud-to-device-message"></a>Buluttan aygıta ileti gönderme

Bu bölümde, simüle edilen aygıt uygulamasına buluttan cihaza iletigönderen bir Java konsol uygulaması oluşturursunuz. Bir [aygıttan IoT](quickstart-send-telemetry-java.md) hub'ına hızlı başlat'a telemetri gönder'e eklediğiniz aygıtın aygıt kimliğine ihtiyacınız vardır. Ayrıca, IoT hub bağlantı dizesini al'da daha önce kopyaladığınız [IoT hub bağlantı dizesini](#get-the-iot-hub-connection-string)de almanız gerekir.

1. Komut isteminizde aşağıdaki komutu kullanarak **send-c2d-iletileri** adlı bir Maven projesi oluşturun. Bu komutun tek ve uzun bir komut olduğunu unutmayın:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde, yeni send-c2d iletileri klasörüne gidin.

3. Metin düzenleyicisi kullanarak, send-c2d iletileri klasöründeki pom.xml dosyasını açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkları ekleyin. Bağımlılık eklemek, IoT hub hizmetinizle iletişim kurmak için uygulamanızdaki **iothub-java-service-client** paketini kullanmanıza olanak tanır:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22) kullanarak en yeni **iot-service-client** sürümünü kontrol edebilirsiniz.

4. pom.xml dosyasını kaydedin ve kapatın.

5. Metin düzenleyicisi kullanarak send-c2d-messages\src\main\java\com\mycompany\app\App.java dosyasını açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. **Uygulama** sınıfına aşağıdaki sınıf düzeyinde değişkenleri ekleyin, **{yourhubconnectionstring}** ve **{yourdeviceid}** değerlerini daha önce belirttiğiniz değerlerle değiştirin:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol =    
        IotHubServiceClientProtocol.AMQPS;
    ```

8. **Ana** yöntemi aşağıdaki kodla değiştirin. Bu kod IoT hub'ınıza bağlanır, cihazınıza bir ileti gönderir ve ardından aygıtın iletiyi alıp işlediğine dair bir bildirim bekler:

    ```java
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver();
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [!NOTE]
    > Basitlik için, bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkeleri (üstel geri tepme gibi) uygulamanız gerekir.

9. Maven kullanarak **simulated-device** uygulamasını oluşturmak için, simulated-device klasöründeki komut isteminde aşağıdaki komutu yürütün:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Benzetimli aygıt klasöründeki komut isteminde, IoT hub'ınıza telemetri göndermeye başlamak ve hub'ınızdan gönderilen buluttan cihaza iletileri dinlemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Benzetimli cihaz uygulamasını çalıştırma](./media/iot-hub-java-java-c2d/receivec2d.png)

2. Send-c2d iletileri klasöründeki komut isteminde, buluttan aygıta ileti göndermek ve geri bildirim bildirimini beklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Buluttan aygıta ileti göndermek için komutu çalıştırın](media/iot-hub-java-java-c2d/sendc2d.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, buluttan cihaza ileti göndermeyi ve almayı öğrendiniz.

IoT Hub'ı kullanan eksiksiz uçtan uca çözümlerin örneklerini görmek için [Azure IoT Çözüm Hızlandırıcıları'na](https://azure.microsoft.com/documentation/suites/iot-suite/)bakın.

IoT Hub ile çözüm geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.
