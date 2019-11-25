---
title: Custom Java module tutorial - Azure IoT Edge | Microsoft Docs
description: Bu öğreticide Java koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilir.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 8f5b34d3195ec6807f6eea6388e24d283f403c0c
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457492"
---
# <a name="tutorial-develop-a-java-iot-edge-module-for-linux-devices"></a>Tutorial: Develop a Java IoT Edge module for Linux devices

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. You'll use the simulated IoT Edge device that you created in the Deploy Azure IoT Edge on a simulated device in [Linux](quickstart-linux.md) quickstart. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:    

> [!div class="checklist"]
> * Visual Studio Code ile Azure IoT Edge maven şablon paketini ve Azure IoT Java cihaz SDK'sını temel alan bir IoT Edge Java modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.


Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Solution scope

This tutorial demonstrates how to develop a module in **Java** using **Visual Studio Code**, and how to deploy it to a **Linux device**. IoT Edge does not support Java modules for Windows devices. 

Use the following table to understand your options for developing and deploying Java modules: 

| Java | Visual Studio Code | Visual Studio 2017/2019 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Use VS Code for Java modules on Linux AMD64](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![Use VS Code for Java modules on Linux ARM32](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Önkoşullar

Before beginning this tutorial, you should have gone through the previous tutorial to set up your development environment for Linux container development: [Develop IoT Edge modules for Linux devices](tutorial-develop-for-linux.md). By completing either of those tutorials, you should have the following prerequisites in place: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Linux device running Azure IoT Edge](quickstart-linux.md)
* A container registry, like [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) configured with the [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) configured to run Linux containers.

To develop an IoT Edge module in Java, install the following additional prerequisites on your development machine: 

* Visual Studio Code için [Java Uzantı Paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).
* [Java SE Development Kit 10](https://aka.ms/azure-jdks) ve [`JAVA_HOME` ortam değişkenini](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) JDK yüklemenize göre ayarlama.
* [Maven](https://maven.apache.org/)

## <a name="create-a-module-project"></a>Create a module project
The following steps create an IoT Edge module project that's based on the Azure IoT Edge maven template package and Azure IoT Java device SDK. You create the project by using Visual Studio Code and the Azure IoT Tools.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir Java çözüm şablonu oluşturun. 

1. Visual Studio Code'da VS Code komut paletini açmak için **View (Görünüm)**  > **Command Palette (Komut Paleti)** öğesini seçin. 

2. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Enter a descriptive name for your solution or accept the default **EdgeSolution**. |
   | Modül şablonunu seçin | Choose **Java Module**. |
   | Provide value for groupId | Enter a group ID value or accept the default **com.edgemodule**. |
   | Modül adı sağlayın | Name your module **JavaModule**. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Your container image is prepopulated from the name you provided in the last step. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>The final image repository looks like \<registry name\>.azurecr.io/javamodule. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-java-module/repository.png)
   
İlk kez bir Java modülü oluşturuyorsanız maven paketlerinin indirilmesi birkaç dakika sürebilir. Ardından VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler. Çözüm çalışma alanında beş üst düzey bileşen bulunur. The **modules** folder contains the Java code for your module as well as Docker files for building your module as a container image. **\.env** dosyasında kapsayıcı kayıt defterinizin kimlik bilgileri yer alır. **deployment.template.json** dosyasında IoT Edge çalışma zamanının modülleri cihazlara dağıtmak için kullandığı bilgiler bulunur. And **deployment.debug.template.json** file containers the debug version of modules. Bu öğreticide **\.vscode** klasörünü veya **\.gitignore** dosyasını düzenlemeyeceksiniz. 

Çözümünüzü oluştururken kapsayıcı kayıt defteri belirtmediyseniz ve varsayılan localhost:5000 değerini kabul ettiyseniz \.env dosyanız olmaz. 

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar. 

1. VS Code gezgininde .env dosyasını açın. 
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin. 
3. Bu dosyayı kaydedin. 

### <a name="select-your-target-architecture"></a>Select your target architecture

Currently, Visual Studio Code can develop Java modules for Linux AMD64 and Linux ARM32v7 devices. You need to select which architecture you're targeting with each solution, because the container is built and run differently for each architecture type. The default is Linux AMD64. 

1. Open the command palette and search for **Azure IoT Edge: Set Default Target Platform for Edge Solution**, or select the shortcut icon in the side bar at the bottom of the window. 

2. In the command palette, select the target architecture from the list of options. For this tutorial, we're using an Ubuntu virtual machine as the IoT Edge device, so will keep the default **amd64**. 

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

1. VS Code gezgininde **modules** > **JavaModule** > **src** > **main** > **java** > **com** > **edgemodule** > **App.java** yolunu izleyin.

2. Yeni başvurulan sınıfları içeri aktarmak için aşağıdaki kodu dosyanın en üstüne ekleyin.

    ```java
    import java.io.StringReader;
    import java.util.concurrent.atomic.AtomicLong;
    import java.util.HashMap;
    import java.util.Map;
    
    import javax.json.Json;
    import javax.json.JsonObject;
    import javax.json.JsonReader;
    
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Pair;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Property;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.TwinPropertyCallBack;
    ```

3. Aşağıdaki tanımı **App** sınıfına ekleyin. This variable sets a temperature threshold. The measured machine temperature won't be reported to IoT Hub until it goes over this value. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

4. **MessageCallbackMqtt** öğesinin yürütme metodunu aşağıdaki kodla değiştirin. Modül IoT Edge hub'ından bir MQTT iletisi aldığında bu yöntem çağrılır. Modül ikizi aracılığıyla ayarlanan sıcaklık eşiğinin altındaki sıcaklıkları rapor eden iletileri filtreler.

    ```java
    protected static class MessageCallbackMqtt implements MessageCallback {
        private int counter = 0;
        @Override
        public IotHubMessageResult execute(Message msg, Object context) {
            this.counter += 1;
 
            String msgString = new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET);
            System.out.println(
                   String.format("Received message %d: %s",
                            this.counter, msgString));
            if (context instanceof ModuleClient) {
                try (JsonReader jsonReader = Json.createReader(new StringReader(msgString))) {
                    final JsonObject msgObject = jsonReader.readObject();
                    double temperature = msgObject.getJsonObject("machine").getJsonNumber("temperature").doubleValue();
                    long threshold = App.tempThreshold.get();
                    if (temperature >= threshold) {
                        ModuleClient client = (ModuleClient) context;
                        System.out.println(
                            String.format("Temperature above threshold %d. Sending message: %s",
                            threshold, msgString));
                        client.sendEventAsync(msg, eventCallback, msg, App.OUTPUT_NAME);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return IotHubMessageResult.COMPLETE;
        }
    }
    ```

5. Aşağıdaki iki statik iç sınıfı **App** sınıfına ekleyin. These classes update the tempThreshold variable when the module twin's desired property changes. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
 
    protected static class OnProperty implements TwinPropertyCallBack {
        @Override
        public void TwinPropertyCallBack(Property property, Object context) {
            if (!property.getIsReported()) {
                if (property.getKey().equals(App.TEMP_THRESHOLD)) {
                    try {
                        long threshold = Math.round((double) property.getValue());
                        App.tempThreshold.set(threshold);
                    } catch (Exception e) {
                        System.out.println("Faile to set TemperatureThread with exception");
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```

6. Modül ikizi güncelleştirmelerine abone olmak için aşağıdaki satırları **main** metodunun **client.open()** bölümünün sonrasına ekleyin.

    ```java
    client.startTwin(new DeviceTwinStatusCallBack(), null, new OnProperty(), null);
    Map<Property, Pair<TwinPropertyCallBack, Object>> onDesiredPropertyChange = new HashMap<Property, Pair<TwinPropertyCallBack, Object>>() {
        {
            put(new Property(App.TEMP_THRESHOLD, null), new Pair<TwinPropertyCallBack, Object>(new OnProperty(), null));
        }
    };
    client.subscribeToTwinDesiredProperties(onDesiredPropertyChange);
    client.getTwin();
    ```

7. Save the App.java file.

8. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın.

9. Dağıtım bildirimine **JavaModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin: 

   ```json
     "JavaModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Add module twin to deployment template](./media/tutorial-java-module/module-twin.png)

10. Save the deployment.template.json file.

## <a name="build-and-push-your-module"></a>Build and push your module

In the previous section, you created an IoT Edge solution and added code to the **JavaModule** to filter out messages where the reported machine temperature is below the acceptable limit. Now, build the solution as a container image and push it to your container registry. 

1. **Görünüm** > **Terminal**'i seçerek VS Code tümleşik terminalini açın.

1. Sign in to Docker by entering the following command in the terminal. Sign in with the username, password, and login server from your Azure container registry. You can retrieve these values from the **Access keys** section of your registry in the Azure portal.
     
   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   You may receive a security warning recommending the use of `--password-stdin`. While that best practice is recommended for production scenarios, it's outside the scope of this tutorial. For more information, see the [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) reference.

2. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and Push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin.

   The build and push command starts three operations. First, it creates a new folder in the solution called **config** that holds the full deployment manifest, built out of information in the deployment template and other solution files. Second, it runs `docker build` to build the container image based on the appropriate dockerfile for your target architecture. Then, it runs `docker push` to push the image repository to your container registry.

## <a name="deploy-modules-to-device"></a>Deploy modules to device

Use the Visual Studio Code explorer and the Azure IoT Tools extension to deploy the module project to your IoT Edge device. You already have a deployment manifest prepared for your scenario, the **deployment.json** file in the config folder. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

Make sure that your IoT Edge device is up and running.

1. In the Visual Studio Code explorer, expand the **Azure IoT Hub Devices** section to see your list of IoT devices.

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin.

3. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın.

4. Yenile düğmesine tıklayın. You should see the new **JavaModule** running along with the **SimulatedTemperatureSensor** module and the **$edgeAgent** and **$edgeHub**.  

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. In the Visual Studio Code explorer, right-click the name of your IoT Edge device and select **Start Monitoring Built-in Event Endpoint**.

2. View the messages arriving at your IoT Hub. It may take a while for the messages to arrive, because the IoT Edge device has to receive its new deployment and start all the modules. Then, the changes we made to the JavaModule code wait until the machine temperature reaches 25 degrees before sending messages. It also adds the message type **Alert** to any messages that reach that temperature threshold. 

## <a name="edit-the-module-twin"></a>Edit the module twin

We used the JavaModule module twin in the deployment manifest to set the temperature threshold at 25 degrees. You can use the module twin to change the functionality without having to update the module code.

1. In Visual Studio Code, expand the details under your IoT Edge device to see the running modules. 

2. Right-click **JavaModule** and select **Edit module twin**. 

3. Find **TemperatureThreshold** in the desired properties. Change its value to a new temperature 5 degrees to 10 degrees higher than the latest reported temperature. 

4. Save the module twin file.

5. Right-click anywhere in the module twin editing pane and select **Update module twin**. 

6. Monitor the incoming device-to-cloud messages. You should see the messages stop until the new temperature threshold is reached. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. When you're ready to build your own modules, you can learn more about [developing your own IoT Edge modules](module-development.md) or how to [develop modules with Visual Studio Code](how-to-vs-code-develop-module.md). You can continue on to the next tutorials to learn how Azure IoT Edge can help you deploy Azure cloud services to process and analyze data at the edge.

> [!div class="nextstepaction"]
> [Functions](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Machine Learning](tutorial-deploy-machine-learning.md)
> [Custom Vision Service](tutorial-deploy-custom-vision.md)
