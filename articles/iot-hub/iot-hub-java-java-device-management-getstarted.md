---
title: Azure IoT Hub aygıt yönetimi (Java) | Microsoft Dokümanlar
description: Uzak bir aygıtı yeniden başlatmak için Azure IoT Hub aygıt yönetimi nasıl kullanılır? Doğrudan bir yöntem içeren bir simüle aygıt uygulaması uygulamak için Java için Azure IoT aygıtı SDK'yı ve doğrudan yöntemi çağıran bir hizmet uygulamasını uygulamak için Java için Azure IoT hizmeti SDK'yı kullanırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: f68e25a618f5c6499ccc9d76c510eab8f1650330
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110892"
---
# <a name="get-started-with-device-management-java"></a>Cihaz yönetimine başlayın (Java)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IoT hub'ınızda bir IoT Hub'ı oluşturmak ve aygıt kimliği oluşturmak için Azure portalını kullanın.

* Aygıtı yeniden başlatmak için doğrudan bir yöntem uygulayan benzetimli bir aygıt uygulaması oluşturun. Buluttan doğrudan yöntemler çağrılır.

* IoT hub'ınız aracılığıyla simüle edilen aygıt uygulamasındadoğrudan yeniden başlatma yöntemini çağıran bir uygulama oluşturun. Bu uygulama daha sonra yeniden başlatma işleminin ne zaman tamamolduğunu görmek için aygıttan bildirilen özellikleri izler.

Bu eğitimin sonunda, iki Java konsolu uygulamanız var:

**simüle cihaz**. Bu uygulama:

* IoT hub'ınıza daha önce oluşturulan aygıt kimliğiyle bağlanır.

* Yeniden başlatma doğrudan yöntem çağrısı alır.

* Fiziksel bir yeniden başlatmayı simüle eder.

* Bildirilen bir özellik aracılığıyla son yeniden başlatma saatini bildirir.

**tetikleme yeniden başlatma**. Bu uygulama:

* Benzetimli cihaz uygulamasında doğrudan bir yöntem çağırır.

* Benzetimli aygıt tarafından gönderilen doğrudan yöntem çağrısına yanıtı görüntüler.

* Güncelleştirilmiş bildirilen özellikleri görüntüler.

> [!NOTE]
> Aygıtlarda ve çözümünüzde çalıştırmak için uygulamalar oluşturmak için kullanabileceğiniz SDK'lar hakkında bilgi için [Azure IoT SDK'larına](iot-hub-devguide-sdks.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* [Java SE Geliştirme Kiti 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable). JDK 8 için indirme almak için **Uzun vadeli destek** altında Java **8** seçtiğinizden emin olun.

* [Maven 3](https://maven.apache.org/download.cgi)

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak aygıtta uzaktan yeniden başlatmayı tetikleme

Bu bölümde, bir Java konsol uygulaması oluşturmak:

1. Benzetimli aygıt uygulamasında yeniden başlatma yöntemini çağırır.

2. Yanıtı görüntüler.

3. Yeniden başlatmanın ne zaman tamamlalda tamamlanıncaya kadar aygıttan gönderilen bildirilen özellikleri yoklar.

Bu konsol uygulaması, doğrudan yöntemi çağırmak ve bildirilen özellikleri okumak için IoT Hub'ınıza bağlanır.

1. **dm-get-started**adlı boş bir klasör oluşturun.

2. **dm-get-started** klasöründe, komut isteminizdeki aşağıdaki komutu kullanarak **tetikleyici-yeniden başlatma** adı verilen bir Maven projesi oluşturun:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. Komut isteminizde, tetikleyici **yeniden başlatma** klasörüne gidin.

4. Metin düzenleyicisi kullanarak, **tetikleyici yeniden başlatma** klasöründeki **pom.xml** dosyasını açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkekleyin. Bu bağımlılık, IoT hub'ınızla iletişim kurmak için uygulamanızdaki iot-service-client paketini kullanmanıza olanak tanır:

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

7. Metin düzenleyicisi **kullanarak, trigger-reboot\src\main\java\com\mycompany\app\App.java** kaynak dosyasını açın.

8. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. `{youriothubconnectionstring}` [IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)daha önce kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

10. Aygıt ikizinden bildirilen özellikleri her 10 saniyede bir okuyan bir iş parçacığı uygulamak **için, Uygulama** sınıfına aşağıdaki iç içe geçmiş sınıfı ekleyin:

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

11. Aşağıdaki özel durumu atmak için **ana** yöntemin imzasını değiştirin:

    ```java
    public static void main(String[] args) throws IOException
    ```

12. Benzetimli aygıtta yeniden başlatma doğrudan yöntemini çağırmak **için, ana** yöntemdeki kodu aşağıdaki kodla değiştirin:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

13. Benzetimli aygıttan bildirilen özellikleri yoklamak için iş parçacığı başlatmak için **ana** yönteme aşağıdaki kodu ekleyin:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

14. Uygulamayı durdurmanızı sağlamak için **ana** yönteme aşağıdaki kodu ekleyin:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

15. **Trigger-reboot\src\main\java\com\mycompany\app\App.java** dosyasını kaydedin ve kapatın.

16. **Tetikleyiciyi yeniden başlat** arka uç uygulamasını oluşturun ve hataları düzeltin. Komut isteminizde, tetikleyici **yeniden başlatma** klasörüne gidin ve aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bir aygıtı taklit eden bir Java konsol uygulaması oluşturursunuz. Uygulama, IoT hub'ınızdan doğrudan yeniden başlatma yöntemi çağrısını dinler ve bu çağrıya hemen yanıt verir. Uygulama daha sonra, tetikleyici yeniden başlatma arka uç uygulamasına yeniden **başlatmanın** tamamolduğunu bildirmek için bildirilen bir özelliği kullanmadan önce yeniden başlatma işlemini simüle etmek için bir süre uyur.

1. **dm-get-started** klasöründe, komut isteminizde aşağıdaki komutu kullanarak **simüle aygıt** adı verilen bir Maven projesi oluşturun:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komut **isteminizde, benzetilen aygıt** klasörüne gidin.

3. Metin düzenleyicisi kullanarak, **benzetilen aygıt** klasöründeki **pom.xml** dosyasını açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkları ekleyin. Bu bağımlılık, IoT hub'ınızla iletişim kurmak için uygulamanızdaki iot-service-client paketini kullanmanıza olanak tanır:

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

7. Metin düzenleyicisi **kullanarak, benzetilen cihaz\src\main\java\com\mycompany\app\App.java** kaynak dosyasını açın.

8. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. IoT hub bölümünde yeni bir aygıtı kaydedin'de belirttiğiniz aygıt bağlantı dizesiyle `{yourdeviceconnectionstring}` [değiştirin:](#register-a-new-device-in-the-iot-hub)

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

10. Doğrudan yöntem durumu olayları için bir geri arama işleyicisi uygulamak **için, Uygulama** sınıfına aşağıdaki iç içe sınıf ekleyin:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

11. Aygıt ikiz durum olayları için bir geri arama işleyicisi uygulamak **için, Uygulama** sınıfına aşağıdaki iç içe geçmiş sınıfı ekleyin:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

12. Özellik olayları için bir geri arama işleyicisi uygulamak **için, Uygulama** sınıfına aşağıdaki iç içe sınıf ekleyin:

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

13. Aygıtıyeniden başlatmayı simüle etmek için bir iş parçacığı uygulamak **için, Uygulama** sınıfına aşağıdaki iç içe geçen sınıfı ekleyin. İş parçacığı beş saniye uyur ve sonra **sonReboot** bildirilen özelliği ayarlar:

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

14. Cihazda doğrudan yöntemi uygulamak **için, Uygulama** sınıfına aşağıdaki iç içe sınıf ekleyin. Benzetimli uygulama doğrudan yeniden **başlatma** yöntemine bir çağrı aldığında, arayanın bildirimini döndürür ve yeniden başlatmaişlemini işlemek için bir iş parçacığı başlatır:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

15. Aşağıdaki özel durumları atmak için **ana** yöntemin imzasını değiştirin:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

16. DeviceClient'ı anında **DeviceClient**değiştirmek için **ana** yöntemdeki kodu aşağıdaki kodla değiştirin:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

17. Doğrudan yöntem çağrıları için dinlemeye başlamak için **ana** yönteme aşağıdaki kodu ekleyin:

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed to direct methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

18. Aygıt simülatörü kapatmak için **ana** yönteme aşağıdaki kodu ekleyin:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

19. Benzetimli cihaz\src\main\java\com\mycompany\app\App.java dosyasını kaydedin ve kapatın.

20. **Simüle edilmiş aygıt** uygulamasını oluşturun ve hataları düzeltin. Komut **isteminizde, benzetilen aygıt** klasörüne gidin ve aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık uygulamaları çalıştırmaya hazırsınız.

1. **Benzetimli aygıt** klasöründeki komut isteminde, IoT hub'ınızdan yeniden başlatma yöntemi çağrılarını dinlemeye başlamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub simüle cihaz uygulaması doğrudan yöntem aramaları yeniden başlatmak için dinlemek için](./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png)

2. **Tetikleyici yeniden başlatma** klasöründeki bir komut isteminde, IoT hub'ınızdan simüle edilen aygıtınızdaki yeniden başlatma yöntemini çağırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub hizmet uygulaması yeniden başlatma doğrudan yöntemi aramak için](./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png)

3. Benzetimli aygıt yeniden başlatma yöntemi çağrısına yanıt verir:

    ![Java IoT Hub simüle cihaz uygulaması doğrudan yöntem çağrısına yanıt verir](./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
