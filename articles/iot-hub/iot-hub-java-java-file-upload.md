---
title: Aygıtlardan Dosyaları Java ile Azure IoT Hub'ına yükleme | Microsoft Dokümanlar
description: Java için Azure IoT aygıtı SDK'yı kullanarak bir cihazdan buluta dosya yükleme. Yüklenen dosyalar bir Azure depolama blob kapsayıcısında depolanır.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.openlocfilehash: fcc2013f67c6e91182979a9bcab683894088a1d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284531"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-java"></a>IoT Hub (Java) ile cihazınızdan buluta dosya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğretici, [IoT Hub öğreticisi ile buluttan cihaza gönder iletilerinde,](iot-hub-java-java-c2d.md) bir dosyayı [Azure blob depolamasına](../storage/index.yml)yüklemek için [IoT Hub'ın dosya yükleme özelliklerini](iot-hub-devguide-file-upload.md) nasıl kullanacağınızı göstermek için kod üzerine kurulur. Öğretici şunların nasıl yapıldığını göstermektedir:

* Bir dosyayı yüklemek için güvenli bir şekilde Azure blob URI içeren bir aygıt sağlayın.

* Uygulamanızın arka ucundaki dosyanın işlenmesini tetiklemek için IoT Hub dosya yükleme bildirimlerini kullanın.

Bir [aygıttan IoT hub'ına](quickstart-send-telemetry-java.md) hızlı başlatma ve [IoT Hub öğreticisiyle buluttan aygıta ileti gönder,IoT](iot-hub-java-java-c2d.md) Hub'ın temel aygıttan buluta ve buluttan cihaza mesajlaşma işlevselliğini gösterir. [IoT Hub öğreticisiyle yapılandırılan ileti yönlendirmesi,](tutorial-routing.md) aygıttan buluta iletileri Azure blob depolama alanında güvenilir bir şekilde depolamanın bir yolunu açıklar. Ancak, bazı senaryolarda, aygıtlarınızın gönderdiği verileri IoT Hub'ın kabul ettiği nispeten küçük aygıttan buluta iletilerle kolayca eşleyemezsiniz. Örnek:

* Resim içeren büyük dosyalar
* Videolar
* Yüksek frekansta örneklenmiş titreşim verileri
* Önceden işlenmiş bir veri türü.

Bu dosyalar genellikle [Azure Veri Fabrikası](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.yml) yığını gibi araçlar kullanılarak bulutta toplu olarak işlenir. Bir aygıttan yayla dosyaları yapmanız gerektiğinde, IoT Hub'ın güvenliğini ve güvenilirliğini kullanmaya devam edebilirsiniz.

Bu eğitimin sonunda iki Java konsolu uygulaması çalıştırın:

* **simüle edilen aygıt**, uygulamanın değiştirilmiş bir sürümü [IoT Hub ile buluttan cihaza ileti gönder] öğreticisi. Bu uygulama, IoT hub'ınız tarafından sağlanan bir SAS URI kullanarak bir dosyayı depolama alanına yükler.

* ioT hub'ınızdan dosya yükleme bildirimleri alan **okuma-dosya yükleme-bildirimi.**

> [!NOTE]
> IoT Hub, Azure IoT aygıt SDK'ları aracılığıyla birçok aygıt platformlarını ve dili (C, .NET ve Javascript dahil) destekler. Cihazınızı Azure IoT Hub'ına nasıl bağlayacaklarınız hakkında adım adım talimatlar için [Azure IoT Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/iot) bakın.

## <a name="prerequisites"></a>Ön koşullar

* [Java SE Geliştirme Kiti 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable). JDK 8 için indirme almak için **Uzun vadeli destek** altında Java **8** seçtiğinizden emin olun.

* [Maven 3](https://maven.apache.org/download.cgi)

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Aygıt uygulamasından dosya yükleme

Bu bölümde, [IoT Hub ile buluttan cihaza ileti gönder'de](iot-hub-java-java-c2d.md) oluşturduğunuz aygıt uygulamasını ioT hub'ına bir dosya yüklemek için değiştirirsiniz.

1. Bir resim dosyasını `simulated-device` klasöre kopyalayın ve yeniden `myimage.png`adlandırın.

2. Metin düzenleyicisi kullanarak `simulated-device\src\main\java\com\mycompany\app\App.java` dosyayı açın.

3. Değişken **bildirimini Uygulama** sınıfına ekleyin:

    ```java
    private static String fileName = "myimage.png";
    ```

4. Dosya yükleme durumu geri arama iletilerini işlemek için **Uygulama** sınıfına aşağıdaki iç içe sınıf ekleyin:

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

5. Görüntüleri IoT Hub'a yüklemek için, Görüntüleri IoT Hub'a yüklemek için **Uygulama** sınıfına aşağıdaki yöntemi ekleyin:

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

6. UploadFile yöntemini aşağıdaki **uploadFile** parçacıkta gösterildiği gibi çağırmak için **ana** yöntemi değiştirin:

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

7. **Benzetimli cihaz** uygulamasını oluşturmak ve hataları denetlemek için aşağıdaki komutu kullanın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-java.md)oluşturduğunuz IoT hub'ından dosya yükleme bildirim iletileri almak için bir arka uç hizmeti oluşturursunuz. Dosya yükleme bildirim iletileri almak için **hizmetinizin bağlanma** iznine ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Dosya yükleme bildirimi alma

Bu bölümde, IoT Hub'dan dosya yükleme bildirim iletileri alan bir Java konsol uytun uygulaması oluşturursunuz.

1. Komut isteminizdeki aşağıdaki komutu kullanarak **okuma-dosya yükleme-bildirimi** adı verilen bir Maven projesi oluşturun. Bu komutun tek ve uzun bir komut olduğunu unutmayın:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut isteminizde yeni `read-file-upload-notification` klasöre gidin.

3. Metin düzenleyicisi kullanarak, klasördeki dosyayı `pom.xml` `read-file-upload-notification` açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkekleyin. Bağımlılık eklemek, IoT hub hizmetinizle iletişim kurmak için uygulamanızdaki **iothub-java-service-client** paketini kullanmanıza olanak tanır:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22) kullanarak en yeni **iot-service-client** sürümünü kontrol edebilirsiniz.

4. Dosyayı kaydedin `pom.xml` ve kapatın.

5. Metin düzenleyicisi kullanarak `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` dosyayı açın.

6. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

7. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. `{Your IoT Hub connection string}` Yer tutucu değerini, IoT hub bağlantı dizesini al'da daha önce kopyaladığınız IoT hub bağlantı [dizesiyle değiştirin:](#get-the-iot-hub-connection-string)

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

8. Konsola yüklenen dosya yla ilgili bilgileri yazdırmak için **Uygulama** sınıfına aşağıdaki iç içe sınıfekleyin:

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Receive file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

9. Dosya yükleme bildirimlerini dinleyen iş parçacığı başlatmak için **ana** yönteme aşağıdaki kodu ekleyin:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

10. Dosyayı kaydedin `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` ve kapatın.

11. **Okuma-dosya yükleme-bildirim** uygulamasını oluşturmak ve hataları denetlemek için aşağıdaki komutu kullanın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

Klasördeki komut isteminde `read-file-upload-notification` aşağıdaki komutu çalıştırın:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Klasördeki komut isteminde `simulated-device` aşağıdaki komutu çalıştırın:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Aşağıdaki ekran **görüntüsü, simüle edilen aygıt** uygulamasından çıktıyı gösterir:

![Simüle edilmiş cihaz uygulamasından çıktı](media/iot-hub-java-java-upload/simulated-device.png)

Aşağıdaki ekran **görüntüsü, okuma dosyası yükleme-bildirim** uygulamasından çıktıyı gösterir:

![Okuma-dosya yükleme-bildirim uygulamasından çıktı](media/iot-hub-java-java-upload/read-file-upload-notification.png)

Yüklediğiniz dosyayı yapılandırılan depolama kapsayıcısında görüntülemek için portalı kullanabilirsiniz:

![Yüklenen dosya](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, cihazlardan gelen dosya yüklemelerini basitleştirmek için IoT Hub'ın dosya yükleme özelliklerini nasıl kullanacağınızı öğrendiniz. Aşağıdaki makalelerle IoT hub özelliklerini ve senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlı bir IoT hub'ı oluşturma](iot-hub-rm-template-powershell.md)

* [C SDK'ya Giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Edge ile bir aygıtı simüle etme](../iot-edge/tutorial-simulate-device-linux.md)
