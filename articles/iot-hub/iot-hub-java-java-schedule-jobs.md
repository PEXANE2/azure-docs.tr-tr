---
title: Azure IoT Hub (Java) ile işleri zamanlama | Microsoft Dokümanlar
description: Doğrudan bir yöntem çağırmak ve birden çok aygıtta istenen bir özellik ayarlamak için Azure IoT Hub işi zamanlama. Simüle edilen aygıt uygulamalarını uygulamak için Java için Azure IoT aygıtı SDK'yı ve java için Azure IoT hizmeti SDK'yı kullanarak işi çalıştıracak bir hizmet uygulamasını kullanırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/16/2019
ms.custom: mqtt
ms.openlocfilehash: 5e3f4f4aedb0bc3fb1f8ea11001b08daa57aafc1
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732479"
---
# <a name="schedule-and-broadcast-jobs-java"></a>Zamanlama ve yayın işleri (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Milyonlarca aygıtı güncelleştiren işleri zamanlamak ve izlemek için Azure IoT Hub'ı kullanın. İşleri şu şekilde kullanın:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştir
* Doğrudan yöntemleri çağırma

Bir iş bu eylemlerden birini sarar ve yürütmeyi bir dizi aygıta karşı izler. Aygıt ikiz sorgusu, işin karşı yürüttüğü aygıt kümesini tanımlar. Örneğin, bir arka uç uygulaması, aygıtları yeniden başlatan 10.000 aygıtta doğrudan bir yöntem çağırmak için bir işi kullanabilir. Aygıt ikiz sorgusuna sahip aygıt kümesini belirtir ve işi ileriki bir zamanda çalışacak şekilde zamanlarsınız. Her aygıt doğrudan yeniden başlatma yöntemini alır ve çalıştırırken iş ilerlemeyi izler.

Bu yeteneklerin her biri hakkında daha fazla bilgi edinmek için bkz:

* Cihaz ikizi ve özellikleri: [Cihaz ikizleri ile başlayın](iot-hub-java-java-twin-getstarted.md)

* Doğrudan yöntemler: [IoT Hub geliştirici kılavuzu - doğrudan yöntemler](iot-hub-devguide-direct-methods.md) ve [Öğretici: Doğrudan yöntemleri kullanın](quickstart-control-device-java.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* **lockDoor**adı verilen doğrudan bir yöntem uygulayan bir aygıt uygulaması oluşturun. Cihaz uygulaması ayrıca arka uç uygulamasından istenen özellik değişikliklerini de alır.

* Birden çok cihazda **lockDoor** doğrudan yöntemini aramak için bir iş oluşturan bir arka uç uygulaması oluşturun. Başka bir iş, istenen özellik güncelleştirmelerini birden çok aygıta gönderir.

Bu eğitimin sonunda, bir java konsolu cihazı uygulaması ve bir java konsolu arka uç uygulaması var:

IoT hub'ınıza bağlanan, **lockDoor** direct yöntemini uygulayan ve istenen özellik değişikliklerini işleyen simüle edilmiş **aygıt.**

**lockDoor** doğrudan yöntemini aramak ve aygıtı birden çok cihazda istenen ikiz özellikleri güncelleştirmek için işleri kullanan **zamanlama işleri.**

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

IoT hub'ınıza bir aygıt eklemek için Azure CLI aracı [için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) da kullanabilirsiniz.

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulamasını oluşturma

Bu bölümde, işleri şu şekilde kullanan bir Java konsol uygulaması oluşturursunuz:

* **LockDoor** direct yöntemini birden çok cihazda arayın.

* İstenilen özellikleri birden çok aygıta gönderin.

Uygulamayı oluşturmak için:

1. Geliştirme **makinenizde, iot-java-schedule-jobs**adlı boş bir klasör oluşturun.

2. **iot-java-schedule-jobs** klasöründe, komut isteminizdeki aşağıdaki komutu kullanarak **zamanlama işleri** adlı bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

   ```cmd/sh
   mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

3. Komut isteminizde, zamanlama **işleri** klasörüne gidin.

4. Metin düzenleyicisi kullanarak, **zamanlama işleri** klasöründeki **pom.xml** dosyasını açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkekleyin. Bu bağımlılık, IoT hub'ınızla iletişim kurmak için uygulamanızdaki **iot-service-client** paketini kullanmanıza olanak tanır:

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

7. Metin düzenleyicisi **kullanarak, program-jobs\src\main\java\com\mycompany\app\App.java** dosyasını açın.

8. Aşağıdaki **içeri aktarma** deyimlerini dosyaya ekleyin:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. IoT hub bağlantı dizesini al'da daha önce kopyaladığınız IoT hub bağlantı dizenizi `{youriothubconnectionstring}` [değiştirin:](#get-the-iot-hub-connection-string)

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

10. Aygıt ikizinde **Bina** ve **Kat** istenilen özellikleri güncelleştirmek için bir iş zamanlamak için **Uygulama** sınıfına aşağıdaki yöntemi ekleyin:

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule the update twin job to run now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

11. **lockDoor** yöntemini aramak için bir iş zamanlamak için **Uygulama** sınıfına aşağıdaki yöntemi ekleyin:

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now to call the lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set to 5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

12. Bir işi izlemek için **Uygulama** sınıfına aşağıdaki yöntemi ekleyin:

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check the job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

13. Çalıştırdığınız işlerin ayrıntılarını sorgulamak için aşağıdaki yöntemi ekleyin:

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using the time the jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over the list of jobs and print the details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

14. **Ana** yöntem imzasını aşağıdaki `throws` yan tümceyi içerecek şekilde güncelleştirin:

    ```java
    public static void main( String[] args ) throws Exception
    ```

15. İki işi sırayla çalıştırmak ve izlemek **için, ana** yöntemdeki kodu aşağıdaki kodla değiştirin:

    ```java
    // Record the start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query to show the job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

16. Zamanlamayı kaydet ve **kapat\jobs\src\main\java\com\mycompany\app\App.java** dosyası

17. Zamanlama **işleri** uygulamasını oluşturun ve hataları düzeltin. Komut isteminizde, zamanlama **işleri** klasörüne gidin ve aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde, IoT Hub'dan gönderilen istenilen özellikleri işleyen ve doğrudan yöntem çağrısını uygulayan bir Java konsol uygulaması oluşturursunuz.

1. **iot-java-schedule-jobs** klasöründe, komut isteminizde aşağıdaki komutu kullanarak **simüle edilmiş aygıt** adlı bir Maven projesi oluşturun. Bunun tek ve uzun bir komut olduğunu unutmayın:

   ```cmd/sh
   mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

2. Komut **isteminizde, benzetilen aygıt** klasörüne gidin.

3. Metin düzenleyicisi kullanarak, **benzetilen aygıt** klasöründeki **pom.xml** dosyasını açın ve **bağımlılık** düğümüne aşağıdaki bağımlılıkları ekleyin. Bu bağımlılık, IoT hub'ınızla iletişim kurmak için uygulamanızdaki **iot-device-client** paketini kullanmanıza olanak tanır:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.17.5</version>
    </dependency>
    ```

    > [!NOTE]
    > [Maven arama](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22) kullanarak en yeni **iot-device-client** sürümünü kontrol edebilirsiniz.

4. **Bağımlılık düğümüne** aşağıdaki bağımlılık ekleyin. Bu bağımlılık, aygıt istemcisi SDK tarafından günlük işlemlerini uygulamak için kullanılan Apache [SLF4J](https://www.slf4j.org/) günlük cephesi için bir NOP yapılandırır. Bu yapılandırma isteğe bağlıdır, ancak bunu atlarsanız, uygulamayı çalıştırdığınızda konsolda bir uyarı görebilirsiniz. Aygıt istemcisi SDK'da günlüğe kaydetme hakkında daha fazla bilgi için, Java readme dosyası *için Azure IoT aygıtı SDK Örneklerinde* [Oturum Açma'ya](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/readme.md#logging)bakın.

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

9. Aşağıdaki sınıf düzeyi değişkenleri **App** sınıfına ekleyin. IoT hub bölümünde ki yeni bir aygıtı kaydedin'de daha önce kopyaladığınız aygıt bağlantı dizesiyle `{yourdeviceconnectionstring}` [değiştirin:](#register-a-new-device-in-the-iot-hub)

    ```java
    private static String connString = "{yourdeviceconnectionstring}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Bu örnek uygulama, bir **DeviceClient** nesnesi örneğini oluşturduğunda **prokotol** değişkenini kullanır.

10. Aygıt çift bildirimlerini konsola yazdırmak için **Uygulama** sınıfına aşağıdaki iç içe geçmiş sınıfı ekleyin:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

11. Konsola doğrudan yöntem bildirimleri yazdırmak **için, Uygulama** sınıfına aşağıdaki iç içe sınıf ekleyin:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

12. IoT Hub'dan gelen doğrudan yöntem çağrılarını işlemek için **Uygulama** sınıfına aşağıdaki iç içe sınıf ekleyin:

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

13. **Ana** yöntem imzasını aşağıdaki `throws` yan tümceyi içerecek şekilde güncelleştirin:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

14. **Ana** yöntemdeki kodu aşağıdaki kodla değiştirin:
    * IoT Hub ile iletişim kurmak için bir aygıt istemcisi oluşturun.
    * Aygıtın ikiz özelliklerini depolamak için bir **Aygıt** nesnesi oluşturun.

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object to manage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

15. Aygıt istemci hizmetlerini başlatmak için **ana** yönteme aşağıdaki kodu ekleyin:

    ```java
    try {
      // Open the DeviceClient
      // Start the device twin services
      // Subscribe to direct method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

16. Kapatmadan önce kullanıcının **Enter** tuşuna basmasını beklemek için **ana** yöntemin sonuna aşağıdaki kodu ekleyin:

    ```java
    // Close the app
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

17. **Benzetimli cihaz\src\main\java\com\mycompany\app\App.java** dosyasını kaydedin ve kapatın.

18. **Simüle edilmiş aygıt** uygulamasını oluşturun ve hataları düzeltin. Komut **isteminizde, benzetilen aygıt** klasörüne gidin ve aşağıdaki komutu çalıştırın:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık konsol uygulamalarını çalıştırmaya hazırsınız.

1. **Benzetimli aygıt** klasöründeki komut isteminde, istenen özellik değişiklikleri ve doğrudan yöntem çağrıları için cihaz uygulamasını dinlemeyi başlatmak için aşağıdaki komutu çalıştırın:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Aygıt istemcisi başlatır](./media/iot-hub-java-java-schedule-jobs/device-app-1.png)

2. Klasördeki bir komut isteminde, iki işi çalıştırmak için zamanlama işleri hizmeti uygulamasını çalıştırmak için aşağıdaki komutu çalıştırın. **schedule-jobs** `schedule-jobs` İlk istenilen özellik değerlerini ayarlar, ikinci doğrudan yöntemi çağırır:

   ```cmd\sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Java IoT Hub hizmet uygulaması iki iş oluşturur](./media/iot-hub-java-java-schedule-jobs/service-app-1.png)

3. Cihaz uygulaması istenen özellik değişikliğini ve doğrudan yöntem çağrısını işler:

   ![Aygıt istemcisi değişikliklere yanıt verir](./media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir aygıta doğrudan bir yöntem ve aygıt ikizözelliklerinin güncelliğini zamanlamak için bir iş kullandınız.

Nasıl yapılacağını öğrenmek için aşağıdaki kaynakları kullanın:

* [IoT Hub öğreticisiyle başlayın'a](quickstart-send-telemetry-java.md) sahip cihazlardan telemetri gönderin.

* Cihazları etkileşimli olarak kontrol edin (kullanıcı kontrollü bir uygulamadan bir fanı açmak gibi) [doğrudan yöntem](quickstart-control-device-java.md) kullanma tutorial.s
