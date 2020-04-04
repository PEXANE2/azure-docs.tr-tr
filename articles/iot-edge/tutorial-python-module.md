---
title: Özel Python modülü oluşturma - Azure IoT Edge | Microsoft Dokümanlar
description: Bu öğreticide Python koduyla IoT Edge modülü oluşturma ve bir Edge cihazına dağıtma adımları gösterilmektedir.
services: iot-edge
author: shizn
manager: philmea
ms.reviewer: kgremban
ms.author: xshi
ms.date: 10/14/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: f474ec121f444f5f0c41272f5d87a7f8abfadb8d
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657045"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-for-linux-devices"></a>Öğretici: Linux cihazları için bir Python IoT Edge modülü geliştirin ve dağıtın

Python kodunu geliştirmek ve Azure IoT Edge çalıştıran bir Linux aygıtına dağıtmak için Visual Studio Code'u kullanın.

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için Azure IoT Edge modüllerini kullanabilirsiniz. Bu öğretici, hızlı başlatmada ayarladığınız IoT Edge aygıtındaki sensör verilerini filtreleyen bir IoT Edge modülü oluşturmave dağıtma konusunda size yol alır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Visual Studio Code kullanarak IoT Edge Python modülü oluşturma.
> * Visual Studio Code ve Docker kullanarak bir Docker görüntüsü oluşturma ve bunu kayıt defterinizde yayımlama.
> * Modülü IoT Edge cihazınıza dağıtma.
> * Oluşturulan verileri görüntüleme.

Bu öğreticide oluşturacağınız IoT Edge modülü, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İletileri yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde gönderir. Bu tür bir analiz, buluta iletilen ve bulutta depolanan veri miktarını azaltmak için yararlıdır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Çözüm kapsamı

Bu öğretici, **Visual Studio Code**kullanarak **Python'da** bir modülün nasıl geliştirilebildiğini ve bir **Linux cihazına**nasıl dağıtılanın caydığını göstermektedir. IoT Edge, Windows aygıtları için Python modüllerini desteklemez.

Python modüllerini Geliştirme ve Linux'a dağıtma seçeneklerinizi anlamak için aşağıdaki tabloyu kullanın:

| Python | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![Linux AMD64'te Python modülleri için VS Kodu'ni kullanın](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![Linux ARM32'de Python modülleri için VS Kodu kullanın](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici başlamadan önce, Linux konteyner geliştirme için geliştirme ortamını ayarlamak için önceki öğretici ile gitmiş olmalıdır: [Linux cihazları için IoT Edge modülleri geliştirin.](tutorial-develop-for-linux.md) Bu öğreticiyi tamamlayarak, aşağıdaki ön koşulları yerine koymalısınız:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* [Azure IoT Edge çalıştıran](quickstart-linux.md) bir Linux aygıtı
* Azure Kapsayıcı Kayıt [Defteri](https://docs.microsoft.com/azure/container-registry/)gibi bir kapsayıcı kayıt defteri.
* [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile yapılandırılan [Görsel Stüdyo Kodu.](https://code.visualstudio.com/)
* [Docker CE](https://docs.docker.com/install/) Linux kapsayıcıları çalıştırmak için yapılandırılmıştır.

Python'da bir IoT Edge modülü geliştirmek için geliştirme makinenize aşağıdaki ek ön koşulları yükleyin:

* Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python).
* [Python](https://www.python.org/downloads/).
* Python paketlerini (genellikle Python yüklemenize dahildir) yüklemek için [PIP](https://pip.pypa.io/en/stable/installing/#installation).

>[!Note]
>`bin` klasörünüzün platformunuzun yolunda olduğundan emin olun. Bu yol genelde UNIX ve macOS için `~/.local/`, Windows için `%APPDATA%\Python` olacaktır.

## <a name="create-a-module-project"></a>Modül projesi oluşturma

Aşağıdaki adımlar, Visual Studio Code ve Azure IoT Araçlarını kullanarak bir IoT Edge Python modülü oluşturur.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Üstüne oluşturabileceğiniz bir Python çözüm şablonu oluşturmak için VS Kodunu kullanın.

1. Visual Studio Code'da VS Code entegre terminalini açmak için **View** > **Terminali'ni** seçin.

1. VS Kodu komut paletini açmak için**Komut Paletini** **Görüntüle'yi** > seçin.

1. Komut paletinde **Azure: Sign in** komutunu girip çalıştırdıktan sonra yönergeleri izleyerek Azure hesabınızda oturum açın. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.

1. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. İstemleri izleyin ve çözümünüzü oluşturmak için aşağıdaki bilgileri sağlayın:

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan **EdgeSolution'ı**kabul edin. |
   | Modül şablonunu seçin | **Python Modülü**'nü seçin. |
   | Modül adı sağlayın | Modülünüze **PythonModule** adını verin. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı resminiz, son adımda sağladığınız addan önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz. <br><br>Son resim deposu kayıt defteri \<adı\>.azurecr.io/pythonmodule gibi görünüyor. |

   ![Docker görüntü deposunu sağlama](./media/tutorial-python-module/repository.png)

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı deponuzun kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde **.env** dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. .env dosyasını kaydedin.

### <a name="select-your-target-architecture"></a>Hedef mimarinizi seçin

Şu anda Visual Studio Code, Linux AMD64 ve Linux ARM32v7 cihazları için Python modülleri geliştirebilir. Kapsayıcı inşa edilmiş ve her mimari türü için farklı çalıştırDığınızdan, her çözümde hangi mimariyi hedeflediğinizi seçmeniz gerekir. Varsayılan Linux AMD64 olduğunu.

1. Komut paletini açın ve **Azure IoT Edge: Edge Solution için Varsayılan Hedef Platformu'nu ayarlayın**veya pencerenin altındaki yan çubuktaki kısayol simgesini seçin.

2. Komut paletinde, seçenekler listesinden hedef mimarisini seçin. Bu öğretici için, biz IoT Edge cihaz olarak bir Ubuntu sanal makine kullanıyoruz, bu yüzden varsayılan **amd64**devam edecektir.

### <a name="update-the-module-with-custom-code"></a>Modülü özel kodla güncelleştirme

Her şablon, Simüle Edilen Sensör verilerini **Simüle Edilen Algısensörü** modülünden alan ve IoT hub'ına ileten örnek kodu içerir. Bu bölümde, göndermeden önce iletileri çözümlemek için **PythonModule** genişleten kodu ekleyin.

1. VS Code explorer'da**PythonModule** > **main.py** **modüllerini** > açın.

2. **main.py** dosyasının en üst kısmından **json** kitaplığını içeri aktarın:

    ```python
    import json
    ```

3. **TEMPERATURE_THRESHOLD** ve **TWIN_CALLBACKS** değişkenlerini global sayaçlarının altına ekleyin. Sıcaklık eşiği, verilerin IoT Hub’a gönderilmesi için, ölçülen makine sıcaklığının aşması gereken değeri ayarlar.

    ```python
    # global counters
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    RECEIVED_MESSAGES = 0
    ```

4. **input1_listener** işlevini aşağıdaki kodla değiştirin:

    ```python
        # Define behavior for receiving an input message on input1
        # Because this is a filter module, we forward this message to the "output1" queue.
        async def input1_listener(module_client):
            global RECEIVED_MESSAGES
            global TEMPERATURE_THRESHOLD
            while True:
                try:
                    input_message = await module_client.receive_message_on_input("input1")  # blocking call
                    message = input_message.data
                    size = len(message)
                    message_text = message.decode('utf-8')
                    print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
                    custom_properties = input_message.custom_properties
                    print ( "    Properties: %s" % custom_properties )
                    RECEIVED_MESSAGES += 1
                    print ( "    Total messages received: %d" % RECEIVED_MESSAGES )
                    data = json.loads(message_text)
                    if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
                        custom_properties["MessageType"] = "Alert"
                        print ( "Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
                        await module_client.send_message_to_output(input_message, "output1")
                except Exception as ex:
                    print ( "Unexpected error in input1_listener: %s" % ex )

        # twin_patch_listener is invoked when the module twin's desired properties are updated.
        async def twin_patch_listener(module_client):
            global TWIN_CALLBACKS
            global TEMPERATURE_THRESHOLD
            while True:
                try:
                    data = await module_client.receive_twin_desired_properties_patch()  # blocking call
                    print( "The data in the desired properties patch was: %s" % data)
                    if "TemperatureThreshold" in data:
                        TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
                    TWIN_CALLBACKS += 1
                    print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
                except Exception as ex:
                    print ( "Unexpected error in twin_patch_listener: %s" % ex )
    ```

5. İkiz güncelleştirmeleri de dinlemek için **dinleyicileri** güncelleyin.

    ```python
        # Schedule task for C2D Listener
        listeners = asyncio.gather(input1_listener(module_client), twin_patch_listener(module_client))

        print ( "The sample is now waiting for messages. ")
    ```

6. main.py dosyasını kaydedin.

7. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki **deployment.template.json** dosyasını açın.

8. **PythonModule** modül ikizini dağıtım bildirimine ekleyin. Aşağıdaki JSON içeriğini **moduleContent** bölümünün en altına, **$edgeHub** modül ikizinin arkasına ekleyin:

   ```json
       "PythonModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Dağıtım şablonuna modül ikizi ekleme](./media/tutorial-python-module/module-twin.png)

9. deployment.template.json dosyasını kaydedin.

## <a name="build-and-push-your-module"></a>Modülünüzü oluşturun ve itin

Önceki bölümde, bir IoT Edge çözümü oluşturdunuz ve PythonModule'e bildirilen makine sıcaklığının kabul edilebilir sınırlar içinde olduğu iletileri filtreleyecek kod eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

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

4. Yenile düğmesine tıklayın. Yeni **PythonModule'i** **Simüle EdilenTemperatureSensor** modülü ve **$edgeAgent** ve **$edgeHub**ile birlikte görmelisiniz.

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

Dağıtım bildirimini IoT Edge cihazınıza uyguladıktan sonra cihazdaki IoT Edge çalışma zamanı yeni dağıtım bilgilerini toplar ve yürütmeye başlar. Cihazda çalışan ve dağıtım bildiriminde bulunmayan modüller durdurulur. Cihazda eksik olan modüller başlatılır.

IoT Edge cihazınızın durumunu görüntülemek için Visual Studio Code gezgininin **Azure IoT Hub Cihazları** bölümünü kullanabilirsiniz. Dağıtılan ve çalışan modüllerin listesini görmek için cihazınızın ayrıntılarını genişletin.

1. Visual Studio Code explorer'da, IoT Edge cihazınızın adını sağ tıklatın ve **Yerleşik Etkinlik Bitiş Noktasını İzle'yi**seçin.

2. IoT Hub'ınıza gelen iletileri görüntüleyin. İletilerin gelmesi biraz zaman alabilir. IoT Edge cihazı yeni dağıtımını almalı ve tüm modülleri başlatacak. Daha sonra PythonModule kodunda yaptığımız değişiklikler, mesaj göndermeden önce makine sıcaklığının 25 dereceye ulaşmasını bekler. Ayrıca, bu sıcaklık eşiğine ulaşan iletilere ileti türü **Uyarısı** da ekler.

## <a name="edit-the-module-twin"></a>Modül ikizini edin

Sıcaklık eşiğini 25 derece olarak ayarlamak için dağıtım manifestosunda PythonModule modül ikizini kullandık. Modül kodunu güncelleştirmek zorunda kalmadan işlevselliği değiştirmek için modül ikizi kullanabilirsiniz.

1. Visual Studio Code'da, çalışan modülleri görmek için IoT Edge cihazınızın altındaki ayrıntıları genişletin.

2. **PythonModule'e** sağ tıklayın ve **modül ikizini edit'i**seçin.

3. **SıcaklıkEşik** istenilen özellikleri bulun. Değerini yeni bir sıcaklığa değiştirin 5 derece ile en son bildirilen sıcaklıktan 10 derece daha yüksek.

4. Modül ikiz dosyasını kaydedin.

5. Modül ikiz düzenleme bölmesinde herhangi bir yere sağ tıklayın ve **modül ikiz güncelleştir'i**seçin.

6. Gelen aygıttan buluta iletileri izleyin. İletilerin yeni sıcaklık eşiğine ulaşılına kadar durduğunu görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Aksi takdirde, ücretlendirmeleri önlemek için bu makalede kullandığınız yerel yapılandırmaları ve Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtrelemek için kod içeren bir IoT Edge modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda, [kendi IoT Edge modüllerinizi geliştirme](module-development.md) veya Visual [Studio Code ile modülgeliştirme](how-to-vs-code-develop-module.md)hakkında daha fazla bilgi edinebilirsiniz. Simüle sıcaklık modülü de dahil olmak üzere IoT Edge modülleri örnekleri için [IoT Edge modül örneklerine](https://github.com/Azure/iotedge/tree/master/edge-modules) ve [IoT Python SDK örneklerine](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-edge-scenarios)bakın.

Azure IoT Edge'in, verileri işlemek ve en uçta analiz etmek için Azure bulut hizmetlerini dağıtmanıza nasıl yardımcı olabileceğini öğrenmek için sonraki öğreticilere devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Fonksiyonlar](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Makine Öğrenme](tutorial-deploy-machine-learning.md)
> [Özel Vizyon Hizmeti](tutorial-deploy-custom-vision.md)
