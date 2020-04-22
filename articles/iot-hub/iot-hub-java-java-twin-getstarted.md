---
title: Azure IoT Hub aygıt ikizleri (Java) | Microsoft Dokümanlar
description: Etiketler eklemek ve ardından bir IoT Hub sorgusu kullanmak için Azure IoT Hub aygıt ikizleri nasıl kullanılır? Aygıt uygulamasını uygulamak için Java için Azure IoT aygıtı SDK'yı ve Etiketleri ekleyen ve IoT Hub sorgusunu çalıştıran bir hizmet uygulamasını uygulamak için Java için Azure IoT hizmeti SDK'yı kullanırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/26/2019
ms.custom: mqtt
ms.openlocfilehash: 3ea2f0eec12d756a898f1761f6b22fd034c1bc3e
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732447"
---
# <a name="get-started-with-device-twins-java"></a>Cihaz ikizleri (Java) ile başlayın

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticide, iki Java konsol uyrama uygulaması oluşturursunuz:

* **add-tags-query**, etiketleri ve sorguları aygıt ikizleri ekler bir Java arka uç uygulaması.
* **simüle edilen aygıt**, IoT hub'ınıza bağlanan ve bildirilen bir özelliği kullanarak bağlantı durumunu bildiren bir Java cihaz uygulaması.

> [!NOTE]
> Azure [IoT SDK'lar](iot-hub-devguide-sdks.md) makalesi, hem aygıt hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IoT SK'ları hakkında bilgi sağlar.

## <a name="prerequisites"></a>Ön koşullar

* [Java SE Geliştirme Kiti 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable). JDK 8 için indirme almak için **Uzun vadeli destek** altında Java **8** seçtiğinizden emin olun.

* [Maven 3](https://maven.apache.org/download.cgi)

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulamasını oluşturma

Bu bölümde, **myDeviceId**ile ilişkili IoT Hub'daki aygıt ikizine etiket olarak konum meta verileri ekleyen bir Java uygulaması oluşturursunuz. Uygulama önce ABD'de bulunan aygıtlar için IoT hub'ını, ardından da hücresel ağ bağlantısını bildiren aygıtlar için sorgular.

1. Geliştirme **makinenizde, iot-java-twin-getstarted**adlı boş bir klasör oluşturun.

2. **iot-java-twin-getstarted** klasöründe, komut isteminizde aşağıdaki komutu kullanarak **add-tags-query** adlı bir Maven projesi oluşturun:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. Komut isteminizde, **add-tags-query** klasörüne gidin.

4. Metin düzenleyicisi kullanarak, **add-tags-query** klasöründeki **pom.xml** dosyasını açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkekleyin. Bu bağımlılık, IoT hub'ınızla iletişim kurmak için uygulamanızdaki **iot-service-client** paketini kullanmanıza olanak tanır:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.17.1</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22) kullanarak en yeni **iot-service-client** sürümünü kontrol edebilirsiniz.

5. **Bağımlılıkdüğümünden** sonra aşağıdaki **yapı** düğümlerini ekleyin. Bu yapılandırma, Maven'e uygulamayı oluşturmak için Java 1.8'i kullanmasını bildirir.

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

6. **Pom.xml** dosyasını kaydedin ve kapatın.

7. Metin düzenleyicisi kullanarak **add-tags-query\src\main\java\com\mycompany\app\App.java** dosyasını açın.

8. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. Kopyala'da kopyaladığınız `{youriothubconnectionstring}` [IoT hub bağlantı dizesini değiştirin.](#get-the-iot-hub-connection-string)

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

10. **Ana** yöntem imzasını aşağıdaki `throws` yan tümceyi içerecek şekilde güncelleştirin:

    ```java
    public static void main( String[] args ) throws IOException
    ```

11. DeviceTwin ve **DeviceTwinDevice** nesnelerini oluşturmak **DeviceTwin** için **ana** yöntemdeki kodu aşağıdaki kodla değiştirin. **DeviceTwin** nesnesi, IoT hub'ınızla iletişimi yönetir. **DeviceTwinDevice** nesnesi, özellikleri ve etiketleri ile aygıt ikizini temsil eder:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

12. `try/catch` **Ana** yönteme aşağıdaki bloğu ekleyin:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

13. **Aygıtınızdaki bölge** ve **tesis** aygıtı ikiz etiketlerini güncelleştirmek için `try` bloka aşağıdaki kodu ekleyin:

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

14. Aygıt ikizlerini IoT hub'ında sorgulamak `try` için, önceki adımda eklediğiniz koddan sonra bloğuna aşağıdaki kodu ekleyin. Kod iki sorgu çalıştırıyor. Her sorgu en fazla 100 aygıt döndürür.

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

15. **Add-tags-query\src\main\java\com\mycompany\app\App.java** dosyasını kaydet ve kapat

16. **Add-tags-query** uygulamasını oluşturun ve hataları düzeltin. Komut **isteminizde, add-tags-query** klasörüne gidin ve aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde, IoT Hub'a gönderilen bildirilen bir özellik değerini ayarlayan bir Java konsol uygulaması oluşturursunuz.

1. **iot-java-twin-getstarted** klasöründe, komut isteminizde aşağıdaki komutu kullanarak **simüle edilmiş aygıt** adlı bir Maven projesi oluşturun:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut **isteminizde, benzetilen aygıt** klasörüne gidin.

3. Metin düzenleyicisi kullanarak, **benzetilen aygıt** klasöründeki **pom.xml** dosyasını açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkları ekleyin. Bu bağımlılık, IoT hub'ınızla iletişim kurmak için uygulamanızdaki **iot-device-client** paketini kullanmanıza olanak tanır.

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.17.5</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22) kullanarak en yeni **iot-device-client** sürümünü kontrol edebilirsiniz.

4. **Bağımlılık düğümüne** aşağıdaki bağımlılık ekleyin. Bu bağımlılık, aygıt istemcisi SDK tarafından günlük işlemlerini uygulamak için kullanılan Apache [SLF4J](https://www.slf4j.org/) günlük cephesi için bir NOP yapılandırır. Bu yapılandırma isteğe bağlıdır, ancak bunu atlarsanız, uygulamayı çalıştırdığınızda konsolda bir uyarı görebilirsiniz. Aygıt istemcisi SDK'da günlüğe kaydetme hakkında daha fazla bilgi için, Java readme dosyası *için Azure IoT aygıtı SDK Örneklerinde* [Oturum Açma'ya](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/readme.md#logging) bakın.

    ```xml
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-nop</artifactId>
      <version>1.7.28</version>
    </dependency>
    ```

5. **Bağımlılıkdüğümünden** sonra aşağıdaki **yapı** düğümlerini ekleyin. Bu yapılandırma, Maven'e uygulamayı oluşturmak için Java 1.8'i kullanmasını bildirir:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

6. **Pom.xml** dosyasını kaydedin ve kapatın.

7. Metin düzenleyicisi **kullanarak, benzetilen cihaz\src\main\java\com\mycompany\app\App.java** dosyasını açın.

8. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. IoT hub'ında yeni bir aygıtı kaydedin'de kopyaladığınız aygıt bağlantı dizesiyle `{yourdeviceconnectionstring}` [değiştirin.](#register-a-new-device-in-the-iot-hub)

    ```java
    private static String connString = "{yourdeviceconnectionstring}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır.

10. İkiz güncelleştirmeler hakkında bilgi yazdırmak için **Uygulama** sınıfına aşağıdaki yöntemi ekleyin:

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
          System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
      }
    ```

11. **Ana** yöntemdeki kodu aşağıdaki kodla değiştirin:

    * IoT Hub ile iletişim kurmak için bir aygıt istemcisi oluşturun.

    * Aygıtın ikiz özelliklerini depolamak için bir **Aygıt** nesnesi oluşturun.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object to store the device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
    };
    ```

12. **Bir bağlantıTürü** bildirilen özellik oluşturmak ve IoT Hub'a göndermek için **ana** yönteme aşağıdaki kodu ekleyin:

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

13. **Ana** yöntemin sonuna aşağıdaki kodu ekleyin. **Enter** tuşunu beklemek, IoT Hub'ın aygıt ikiz işlemlerinin durumunu bildirmesine olanak tanır.

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

14. **main** yönteminin imzasını, aşağıda gösterilen özel durumları içerecek şekilde değiştirin:

     ```java
     public static void main(String[] args) throws URISyntaxException, IOException
     ```

15. **Benzetimli cihaz\src\main\java\com\mycompany\app\App.java** dosyasını kaydedin ve kapatın.

16. **Simüle edilmiş aygıt** uygulamasını oluşturun ve hataları düzeltin. Komut **isteminizde, benzetilen aygıt** klasörüne gidin ve aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık konsol uygulamalarını çalıştırmaya hazırsınız.

1. **Add-tags-query** klasöründeki komut isteminde, **add-tags-query** hizmeti uygulamasını çalıştırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Etiket değerlerini güncellemek ve aygıt sorgularını çalıştırmak için Java IoT Hub hizmet uygulaması](./media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Aygıt ikizine eklenen **bitki** ve **bölge** etiketlerini görebilirsiniz. İlk sorgu aygıtınızı döndürür, ancak ikinci si dönmez.

2. **Benzetimli aygıt** klasöründeki komut isteminde, aygıt ikizine **connectivityType** bildirilen özelliği eklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Aygıt istemcisi **connectivityType** bildirilen özelliği ekler](./media/iot-hub-java-java-twin-getstarted/device-app-1.png)

3. **Add-tags-query** klasöründeki komut isteminde, **add-tags-query** hizmeti uygulamasını ikinci kez çalıştırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Etiket değerlerini güncellemek ve aygıt sorgularını çalıştırmak için Java IoT Hub hizmet uygulaması](./media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Aygıtınız **connectivityType** özelliğini IoT Hub'a gönderdiğine göre, ikinci sorgu aygıtınızı döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Aygıt meta verilerini arka uç uygulamasından etiketler olarak eklediniz ve aygıt bağlantısı bilgilerini aygıt ikizinde bildirmek için bir aygıt uygulaması yazdınız. Ayrıca, SQL benzeri IoT Hub sorgu dilini kullanarak aygıt ikiz bilgilerini nasıl sorgulayabileceğinizi de öğrendiniz.

Nasıl yapılacağını öğrenmek için aşağıdaki kaynakları kullanın:

* [IoT Hub öğreticisiyle başlayın'a](quickstart-send-telemetry-java.md) sahip cihazlardan telemetri gönderin.

* Cihazları etkileşimli olarak kontrol edin (kullanıcı kontrollü bir uygulamadan bir fanı açmak gibi) [doğrudan yöntem](quickstart-control-device-java.md) kullanma öğreticisiyle.
