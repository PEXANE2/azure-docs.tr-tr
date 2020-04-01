---
title: Öğretici Linux için Node.js modülü geliştirmek - Azure IoT Edge | Microsoft Dokümanlar
description: Bu öğreticide Node.js koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilmektedir
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 71b22bf9bf040abcdf513a4f8baa916930c8972e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76772215"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-for-linux-devices"></a>Öğretici: Linux cihazları için bir Node.js IoT Edge modülü geliştirin ve dağıtın

Düğüm.js kodunu geliştirmek ve Azure IoT Edge çalıştıran bir Linux aygıtına dağıtmak için Visual Studio Code'u kullanın.

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide, algılayıcı verilerini filtreleyen bir IoT Edge modülü oluşturma ve dağıtma işlemlerinin adımları açıklanmaktadır. Hızlı başlangıçlarda oluşturduğunuz sanal Azure IoT Edge cihazını kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Visual Studio Code kullanarak IoT Edge Node.js modülü oluşturma
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama
> * Modüle IoT Edge cihazınıza dağıtma
> * Oluşturulan verileri görüntüleme

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğretici, **Visual Studio Code**kullanarak **Düğüm.js** bir modül geliştirmek için nasıl gösterir , ve nasıl bir **Linux cihazına**dağıtmak için . IoT Edge, Windows aygıtları için Düğüm.js modüllerini desteklemez.

Node.js modülleri geliştirme ve dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın:

| Node.js | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Linux AMD64'te Düğüm.js modülleri için VS Kodu'ni kullanın](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![Linux ARM32'de Düğüm.js modülleri için VS Kodu kullanın](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici başlamadan önce, Linux konteyner geliştirme için geliştirme ortamını ayarlamak için önceki öğretici ile gitmiş olmalıdır: [Linux cihazları için IoT Edge modülleri geliştirin.](tutorial-develop-for-linux.md) Bu eğitimlerden birini tamamlayarak, aşağıdaki ön koşullara sahip olmalısınız:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran](quickstart-linux.md) bir Linux aygıtı
* Azure Kapsayıcı Kayıt [Defteri](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile yapılandırılan [Görsel Stüdyo Kodu.](https://code.visualstudio.com/)
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak için yapılandırılmıştır.

Düğüm.js'de bir IoT Edge modülü geliştirmek için geliştirme makinenize aşağıdaki ek ön koşulları yükleyin:

* [Node.js ve npm](https://nodejs.org). npm paketi, Node.js ile birlikte dağıtılır. Başka bir deyişle Node.js'yi indirdiğinizde npm de bilgisayarınıza otomatik olarak yüklenir.

## <a name="create-a-module-project"></a>Modül projesi oluşturma

Aşağıdaki adımlar, Visual Studio Code ve Azure IoT Araçlarını kullanarak bir IoT Edge Node.js modülünün nasıl oluşturulabileceğinizi gösterir.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Temel olarak kullanabileceğiniz bir Node.js çözüm şablonu oluşturmak için **npm** kullanın.

1. Visual Studio Code'da, VS Code entegre terminalini açmak için**Entegre Terminali** **Görüntüle'yi** > seçin.

2. Tümleşik terminalde **yeoman** ve Node.js Azure IoT Edge modülü oluşturucuyu yüklemek için aşağıdaki komutu girin:

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. VS Kodu komut paletini açmak için**Komut Paletini** **Görüntüle'yi** > seçin.

4. Komut paletinde **Azure: Sign in** komutunu yazıp çalıştırdıktan sonra yönergeleri izleyerek Azure hesabınızda oturum açın. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

5. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu yazıp çalıştırın. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan **EdgeSolution'ı**kabul edin. |
   | Modül şablonunu seçin | **Düğüm.js Modül**seçin. |
   | Modül adı sağlayın | Modülünüze **NodeModule** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı resminiz, son adımda sağladığınız addan önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son resim deposu kayıt defteri \<adı\>.azurecr.io/nodemodule gibi görünüyor. |

   ![Docker görüntü deposunu sağlama](./media/tutorial-node-module/repository.png)

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı deponuzun kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde **.env** dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="select-your-target-architecture"></a>Hedef mimarinizi seçin

Şu anda Visual Studio Code, Linux AMD64 ve Linux ARM32v7 cihazları için Düğüm.js modülleri geliştirebilir. Kapsayıcı inşa edilmiş ve her mimari türü için farklı çalıştırDığınızdan, her çözümde hangi mimariyi hedeflediğinizi seçmeniz gerekir. Varsayılan Linux AMD64 olduğunu.

1. Komut paletini açın ve **Azure IoT Edge: Edge Solution için Varsayılan Hedef Platformu'nu ayarlayın**veya pencerenin altındaki yan çubuktaki kısayol simgesini seçin.

2. Komut paletinde, seçenekler listesinden hedef mimarisini seçin. Bu öğretici için, biz IoT Edge cihaz olarak bir Ubuntu sanal makine kullanıyoruz, bu yüzden varsayılan **amd64**devam edecektir.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Her şablon, Simüle Edilen Sensör verilerini **Simüle Edilen Algılayıcı Sensörü** modülünden alan ve IoT Hub'a ileten örnek kodlarla birlikte gelir. Bu bölümde NodeModule modülünün verileri göndermeden önce analiz etmesi için gerekli kodu ekleyin.

1. VS Code explorer'da,**nodemodule** > **app.js** **modüllerini** > açın.

2. Gerekli düğüm modülleri altına bir sıcaklık eşik değeri girin. Sıcaklık eşiği, verilerin IoT Hub’a gönderilmesi için, ölçülen sıcaklığın aşması gereken değeri ayarlar.

    ```javascript
    var temperatureThreshold = 25;
    ```

3. `PipeMessage` işlevinin tamamını `FilterMessage` işleviyle değiştirin.

    ```javascript
    // This function filters out messages that report temperatures below the temperature threshold.
    // It also adds the MessageType property to the message with the value set to Alert.
    function filterMessage(client, inputName, msg) {
        client.complete(msg, printResultFor('Receiving message'));
        if (inputName === 'input1') {
            var message = msg.getBytes().toString('utf8');
            var messageBody = JSON.parse(message);
            if (messageBody && messageBody.machine && messageBody.machine.temperature && messageBody.machine.temperature > temperatureThreshold) {
                console.log(`Machine temperature ${messageBody.machine.temperature} exceeds threshold ${temperatureThreshold}`);
                var outputMsg = new Message(message);
                outputMsg.properties.add('MessageType', 'Alert');
                client.sendOutputEvent('output1', outputMsg, printResultFor('Sending received message'));
            }
        }
    }

    ```

4. `pipeMessage` işlev adını `client.on()` işlevindeki `filterMessage` ile değiştirin.

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

5. Aşağıdaki kod parçacığını `client.open()` geri çağırma işlevinin içine, `else` deyiminin içindeki `client.on()` sonrasına kopyalayın. İstenen özellikler güncelleştirildiğinde bu işlev çağrılır.

    ```javascript
    client.getTwin(function (err, twin) {
        if (err) {
            console.error('Error getting twin: ' + err.message);
        } else {
            twin.on('properties.desired', function(delta) {
                if (delta.TemperatureThreshold) {
                    temperatureThreshold = delta.TemperatureThreshold;
                }
            });
        }
    });
    ```

6. app.js dosyasını kaydedin.

7. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın.

8. Dağıtım bildirimine NodeModule modül ikizini ekleyin. Aşağıdaki JSON içeriğini `moduleContent` bölümünün en altına, `$edgeHub` modül ikizinin arkasına ekleyin:

   ```json
     "NodeModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Dağıtım şablonuna modül ikizi ekleme](./media/tutorial-node-module/module-twin.png)

9. deployment.template.json dosyasını kaydedin.

## <a name="build-and-push-your-module"></a>Modülünüzü oluşturun ve itin

Önceki bölümde, bir IoT Edge çözümü oluşturdunuz ve Bildirilen makine sıcaklığının kabul edilebilir sınırlar içinde olduğu iletileri filtreleyecek bir Kod Düğüm Modülüne eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. **View** > **Terminalini**seçerek VS Code entegre terminalini açın.

1. Terminaldeki aşağıdaki komutu girerek Docker'da oturum açın. Azure konteyner kayıt defterinizden kullanıcı adı, parola ve oturum açma sunucusuyla oturum açın. Bu değerleri Azure portalındaki kayıt defterinizin **Erişim tuşları** bölümünden alabilirsiniz.

   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   'nin kullanılmasını öneren bir güvenlik `--password-stdin`uyarısı alabilirsiniz. Bu en iyi uygulama üretim senaryoları için önerilir, ancak bu öğretici kapsamı dışındadır. Daha fazla bilgi için [docker giriş](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) başvurusuna bakın.

1. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayın ve **IoT Edge çözümünü Oluştur ve İti**

   Yapı ve itme komutu üç işlem başlatır. İlk olarak, dağıtım şablonundaki ve diğer çözüm dosyalarındaki bilgiler dışında oluşturulmuş, tam dağıtım bildirimini tutan **config** adlı çözümde yeni bir klasör oluşturur. İkinci olarak, `docker build` hedef mimariniz için uygun dockerfiledayalı kapsayıcı görüntü oluşturmak için çalışır. Daha sonra, `docker push` görüntü deposunu konteyner kayıt defterinize itmek için çalışır.

## <a name="deploy-modules-to-device"></a>Modülleri aygıta dağıtma

Modül projesini IoT Edge aygıtınıza dağıtmak için Visual Studio Code gezginini ve Azure IoT Araçları uzantısını kullanın. Zaten senaryonuz için hazırlanmış bir dağıtım bildiriminiz var, config klasöründe **deployment.json** dosyası. Tek yapmanız gereken dağıtımı almak üzere bir cihaz seçmek.

IoT Edge cihazınızın çalışır durumda olduğundan emin olun.

1. Visual Studio Code gezgininde, IoT aygıtları listenizi görmek için **Azure IoT Hub Aygıtları** bölümünü genişletin.

2. IoT Edge cihazınızın adına sağ tıklayıp **Create Deployment for Single Device** (Tek bir cihaz için dağıtım oluştur) öğesini seçin.

3. **config** klasöründeki **deployment.json** dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesine tıklayın. deployment.template.json dosyasını kullanmayın.

4. Yenile düğmesine tıklayın. **Simüle Edilen TemperatureSensor** modülü ve **$edgeAgent** ve **$edgeHub**ile birlikte çalışan yeni **NodeModule** görmelisiniz.

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code explorer'da, IoT Edge cihazınızın adını sağ tıklatın ve **Yerleşik Etkinlik Bitiş Noktasını İzle'yi**seçin.

2. IoT Hub'ınıza gelen iletileri görüntüleyin. İletilerin gelmesi biraz zaman alabilir. IoT Edge cihazı yeni dağıtımını almalı ve tüm modülleri başlatacak. Daha sonra, NodeModule kodunda yaptığımız değişiklikler, mesaj göndermeden önce makine sıcaklığının 25 dereceye ulaşmasını bekler. Ayrıca, bu sıcaklık eşiğine ulaşan iletilere ileti türü **Uyarısı** da ekler.

## <a name="edit-the-module-twin"></a>Modül ikizini edin

Sıcaklık eşiğini 25 derece olarak ayarlamak için dağıtım manifestosundaki NodeModule modül ikizini kullandık. Modül kodunu güncelleştirmek zorunda kalmadan işlevselliği değiştirmek için modül ikizi kullanabilirsiniz.

1. Visual Studio Code'da, çalışan modülleri görmek için IoT Edge cihazınızın altındaki ayrıntıları genişletin.

2. Sağ tıklayın **DüğümModülü** ve **seçmodülü ikiz .**

3. **SıcaklıkEşik** istenilen özellikleri bulun. Değerini yeni bir sıcaklığa değiştirin 5 derece ile en son bildirilen sıcaklıktan 10 derece daha yüksek.

4. Modül ikiz dosyasını kaydedin.

5. Modül ikiz düzenleme bölmesinde herhangi bir yere sağ tıklayın ve **modül ikiz güncelleştir'i**seçin.

6. Gelen aygıttan buluta iletileri izleyin. İletilerin yeni sıcaklık eşiğine ulaşılına kadar durduğunu görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda, [kendi IoT Edge modüllerinizi geliştirme](module-development.md) veya Visual [Studio Code ile modülgeliştirme](how-to-vs-code-develop-module.md)hakkında daha fazla bilgi edinebilirsiniz. Simüle edilen sıcaklık modülü de dahil olmak üzere IoT Edge modülleri örnekleri için [IoT Edge modül örneklerine](https://github.com/Azure/iotedge/tree/master/edge-modules)bakın.

Azure IoT Edge'in, verileri işlemek ve en uçta analiz etmek için Azure bulut hizmetlerini dağıtmanıza nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Fonksiyonlar](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Makine Öğrenme](tutorial-deploy-machine-learning.md)
> [Özel Vizyon Hizmeti](tutorial-deploy-custom-vision.md)
