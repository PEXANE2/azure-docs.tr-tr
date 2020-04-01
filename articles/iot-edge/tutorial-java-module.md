---
title: Öğretici - Azure IoT Edge kullanarak Özel Java modülü eğitimi
description: Bu öğreticide Java koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilir.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 228e50160e5c13b2d24a504b02c4bb7e3a420a46
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76772921"
---
# <a name="tutorial-develop-a-java-iot-edge-module-for-linux-devices"></a>Öğretici: Linux cihazları için bir Java IoT Edge modülü geliştirin

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Azure IoT Edge'de oluşturduğunuz simüle edilmiş IoT Edge aygıtını [Linux](quickstart-linux.md) quickstart'ındaki simüle edilmiş bir aygıtta kullanırsınız. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Visual Studio Code ile Azure IoT Edge maven şablon paketini ve Azure IoT Java cihaz SDK'sını temel alan bir IoT Edge Java modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğretici, **Visual Studio Code**kullanarak **Java'da** bir modülün nasıl geliştirilebildiğini ve bir **Linux cihazına**nasıl dağıtılanın caydığını göstermektedir. IoT Edge, Windows aygıtları için Java modüllerini desteklemez.

Java modülleri geliştirme ve dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın:

| Java | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Linux AMD64'te Java modülleri için VS Kodu'ni kullanın](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![Linux ARM32'de Java modülleri için VS Kodu kullanın](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici başlamadan önce, Linux konteyner geliştirme için geliştirme ortamını ayarlamak için önceki öğretici ile gitmiş olmalıdır: [Linux cihazları için IoT Edge modülleri geliştirin.](tutorial-develop-for-linux.md) Bu eğitimlerden birini tamamlayarak, aşağıdaki ön koşullara sahip olmalısınız:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran](quickstart-linux.md) bir Linux aygıtı
* Azure Kapsayıcı Kayıt [Defteri](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile yapılandırılan [Görsel Stüdyo Kodu.](https://code.visualstudio.com/)
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak için yapılandırılmıştır.

Java'da bir IoT Edge modülü geliştirmek için geliştirme makinenize aşağıdaki ek ön koşulları yükleyin: 

* Visual Studio Code için [Java Uzantı Paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).
* [Java SE Development Kit 10](https://aka.ms/azure-jdks) ve [`JAVA_HOME` ortam değişkenini](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) JDK yüklemenize göre ayarlama.
* [Maven](https://maven.apache.org/)

## <a name="create-a-module-project"></a>Modül projesi oluşturma

Aşağıdaki adımlar, Azure IoT Edge maven şablon paketini ve Azure IoT Java aygıt SDK'sını temel alan bir IoT Edge modülü projesi oluşturur. Projeyi Visual Studio Code ve Azure IoT Araçlarını kullanarak oluşturursunuz.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kendi yazacağınız kodla özelleştirebileceğiniz bir Java çözüm şablonu oluşturun.

1. Visual Studio Code'da VS Kodu komut paletini açmak için**Komut Paletini** **Görüntüle'yi** > seçin.

2. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan **EdgeSolution'ı**kabul edin. |
   | Modül şablonunu seçin | **Java Modül'üni**seçin. |
   | groupId için değer sağlama | Grup kimliği değeri girin veya varsayılan **com.edgemodule'ı**kabul edin. |
   | Modül adı sağlayın | Modülünüzü **javamodule**adlandırın. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı resminiz, son adımda sağladığınız addan önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son resim deposu kayıt defteri \<adı\>.azurecr.io/javamodule gibi görünüyor. |

   ![Docker görüntü deposunu sağlama](./media/tutorial-java-module/repository.png)

Java modüllerini ilk kez oluşturuyorsanız, maven paketlerini indirmek birkaç dakika sürebilir. Çözüm hazır olduğunda, VS Kodu penceresi IoT Edge çözüm çalışma alanınızı yükler. Çözüm çalışma alanı beş üst düzey bileşen içerir:

* **Modüller** klasörü, modülünüzü kapsayıcı görüntüsü olarak oluşturmak için modülünüzün Java kodunu ve Docker dosyalarını içerir.
* ** \.Env** dosyası konteyner kayıt kimlik bilgilerinizi depolar.
* **deployment.template.json** dosyasında IoT Edge çalışma zamanının modülleri cihazlara dağıtmak için kullandığı bilgiler bulunur.
* **Deployment.debug.template.json** dosyası modüllerin hata ayıklama sürümünü kaplar.
* Bu öğreticide ** \.vscode** klasörünü veya ** \.gitignore** dosyasını da yemezsin.

Çözümünüzü oluştururken kapsayıcı kayıt defteri belirtmediyseniz ve varsayılan localhost:5000 değerini kabul ettiyseniz \.env dosyanız olmaz.

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde .env dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="select-your-target-architecture"></a>Hedef mimarinizi seçin

Şu anda Visual Studio Code, Linux AMD64 ve Linux ARM32v7 cihazları için Java modülleri geliştirebilir. Kapsayıcı inşa edilmiş ve her mimari türü için farklı çalıştırDığınızdan, her çözümde hangi mimariyi hedeflediğinizi seçmeniz gerekir. Varsayılan Linux AMD64 olduğunu.

1. Komut paletini açın ve **Azure IoT Edge: Edge Solution için Varsayılan Hedef Platformu'nu ayarlayın**veya pencerenin altındaki yan çubuktaki kısayol simgesini seçin.

2. Komut paletinde, seçenekler listesinden hedef mimarisini seçin. Bu öğretici için, biz IoT Edge cihaz olarak bir Ubuntu sanal makine kullanıyoruz, bu yüzden varsayılan **amd64**devam edecektir.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

1. VS Code explorer'da, açık **modüller** > **JavaModule** > **src** > **ana** > **java** > **com** > **edgemodule** > **App.java**.

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

3. Aşağıdaki tanımı **App** sınıfına ekleyin. Bu değişken bir sıcaklık eşiği ayarlar. Ölçülen makine sıcaklığı, bu değeri geçene kadar IoT Hub'a bildirilmez.

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

5. Aşağıdaki iki statik iç sınıfı **App** sınıfına ekleyin. Bu sınıflar, modül ikizinin istediği özellik değiştiğinde tempThreshold değişkenini günceller. Tüm modüllerin, doğrudan buluttan bir modülün içinde çalışan kodu yapılandırmanıza izin veren kendi modül ikizi vardır.

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

7. App.java dosyasını kaydedin.

8. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın.

9. Dağıtım bildirimine **JavaModule** modül ikizini ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin:

   ```json
     "JavaModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Dağıtım şablonuna modül ikizi ekleme](./media/tutorial-java-module/module-twin.png)

10. deployment.template.json dosyasını kaydedin.

## <a name="build-and-push-your-module"></a>Modülünüzü oluşturun ve itin

Önceki bölümde, bildirilen makine sıcaklığının kabul edilebilir sınırın altında olduğu iletileri filtrelemek için bir IoT Edge çözümü oluşturdunuz ve **JavaModülüne** kod eklediniz. Şimdi, çözümü bir kapsayıcı görüntüsü olarak oluşturun ve kapsayıcı kayıt defterinize itin.

1. **View** > **Terminalini**seçerek VS Code entegre terminalini açın.

1. Terminaldeki aşağıdaki komutu girerek Docker'da oturum açın. Azure konteyner kayıt defterinizden kullanıcı adı, parola ve oturum açma sunucusuyla oturum açın. Bu değerleri Azure portalındaki kayıt defterinizin **Erişim tuşları** bölümünden alabilirsiniz.

   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   'nin kullanılmasını öneren bir güvenlik `--password-stdin`uyarısı alabilirsiniz. Bu en iyi uygulama üretim senaryoları için önerilir, ancak bu öğretici kapsamı dışındadır. Daha fazla bilgi için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvurusuna bakın.

1. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayın ve **IoT Edge çözümünü Oluştur ve İti**

   Yapı ve itme komutu üç işlem başlatır. İlk olarak, dağıtım şablonu ve diğer çözüm dosyalarındaki bilgiler dışında oluşturulan tam dağıtım bildirimini tutan **config** adlı çözümde yeni bir klasör oluşturur. İkinci olarak, `docker build` hedef mimariniz için uygun dockerfiledayalı kapsayıcı görüntü oluşturmak için çalışır. Daha sonra, `docker push` görüntü deposunu konteyner kayıt defterinize itmek için çalışır.

## <a name="deploy-modules-to-device"></a>Modülleri aygıta dağıtma

Modül projesini IoT Edge aygıtınıza dağıtmak için Visual Studio Code gezginini ve Azure IoT Araçları uzantısını kullanın. Zaten senaryonuz için hazırlanmış bir dağıtım bildiriminiz var, config klasöründe **deployment.json** dosyası. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IoT Edge cihazınızın çalışır durumda olduğundan emin olun.

1. Visual Studio Code gezgininde, IoT aygıtları listenizi görmek için **Azure IoT Hub Aygıtları** bölümünü genişletin.

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin.

3. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın.

4. Yenile düğmesine tıklayın. Yeni **JavaModule'in** **Simüle TemperatureSensor** modülü ve **$edgeAgent** ve **$edgeHub**ile birlikte çalıştığını görmelisiniz.  

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code explorer'da, IoT Edge cihazınızın adını sağ tıklatın ve **Yerleşik Etkinlik Bitiş Noktasını İzle'yi**seçin.

2. IoT Hub'ınıza gelen iletileri görüntüleyin. İletilerin gelmesi biraz zaman alabilir. IoT Edge cihazı yeni dağıtımını almalı ve tüm modülleri başlatacak. Daha sonra, JavaModule kodunda yaptığımız değişiklikler, ileti göndermeden önce makine sıcaklığının 25 dereceye ulaşmasını bekler. Ayrıca, bu sıcaklık eşiğine ulaşan iletilere ileti türü **Uyarısı** da ekler.

## <a name="edit-the-module-twin"></a>Modül ikizini edin

Sıcaklık eşiğini 25 derece olarak ayarlamak için dağıtım manifestosundaki JavaModule modül ikizini kullandık. Modül kodunu güncelleştirmek zorunda kalmadan işlevselliği değiştirmek için modül ikizi kullanabilirsiniz.

1. Visual Studio Code'da, çalışan modülleri görmek için IoT Edge cihazınızın altındaki ayrıntıları genişletin.

2. **JavaModule'yi** sağ tıklatın ve **modül ikizi edit**seçin.

3. **SıcaklıkEşik** istenilen özellikleri bulun. Değerini yeni bir sıcaklığa değiştirin 5 derece ile en son bildirilen sıcaklıktan 10 derece daha yüksek.

4. Modül ikiz dosyasını kaydedin.

5. Modül ikiz düzenleme bölmesinde herhangi bir yere sağ tıklayın ve **modül ikiz güncelleştir'i**seçin.

6. Gelen aygıttan buluta iletileri izleyin. İletilerin yeni sıcaklık eşiğine ulaşılına kadar durduğunu görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, IoT Edge aygıtınız tarafından oluşturulan ham verileri filtreleyen bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda, [IoT Edge modülleri geliştirme](module-development.md) veya Visual [Studio Code ile modüllerin](how-to-vs-code-develop-module.md)nasıl geliştirilen hakkında daha fazla bilgi edinebilirsiniz. Simüle edilmiş sıcaklık modülü de dahil olmak üzere kod örnekleri için [IoT Edge modül örneklerine](https://github.com/Azure/iotedge/tree/master/edge-modules) bakın.

Azure IoT Edge'in, verileri işlemek ve en uçta analiz etmek için Azure bulut hizmetlerini dağıtmanıza nasıl yardımcı olduğunu öğrenmek için sonraki öğreticilere devam edin.

> [!div class="nextstepaction"]
> [Fonksiyonlar](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Makine Öğrenme](tutorial-deploy-machine-learning.md)
> [Özel Vizyon Hizmeti](tutorial-deploy-custom-vision.md)
