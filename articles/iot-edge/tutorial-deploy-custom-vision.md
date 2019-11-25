---
title: Tutorial deploy Custom Vision classifier to a device - Azure IoT Edge | Microsoft Docs
description: In this tutorial, learn how to make a computer vision model run as a container using Custom Vision and IoT Edge.
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/15/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3418c57493e19580f0d3dbd9ea979b0322d930b8
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457296"
---
# <a name="tutorial-perform-image-classification-at-the-edge-with-custom-vision-service"></a>Öğretici: Özel Görüntü İşleme Hizmeti ile uçta görüntü sınıflandırması gerçekleştirme

Azure IoT Edge, iş yüklerini buluttan uca taşıyarak IoT çözümünüzü daha verimli hale getirmenizi sağlayabilir. Bu özellik, özel görüntü işleme modelleri gibi çok miktarda veri işleyen hizmetler için uygundur. [Özel Görüntü İşleme Hizmeti](../cognitive-services/custom-vision-service/home.md), özel görüntü sınıflandırıcıları derlemenizi ve bunları cihazlara kapsayıcı olarak dağıtmanızı sağlar. Bu iki hizmet birlikte tüm verileri aktarmak zorunda kalmadan görüntülerden veya video akışlarından içgörü elde etmenizi sağlar. Özel Görüntü İşleme Hizmeti, içgörü oluşturmak için bir görüntüyü eğitilmiş modelle karşılaştıran bir sınıflandırıcı sunar.

Örneğin bir IoT Edge cihazı üzerinde çalışan Özel Görüntü İşleme Hizmeti bir otoyoldaki trafik yoğunluğunun beklenenden daha yüksek veya düşük olduğunu veya bir otoparkta yeterli yer bulunup bulunmadığını belirleyebilir. Bu içgörüler eylem gerçekleştirmek üzere başka bir hizmetle paylaşılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Özel Görüntü İşleme Hizmeti ile bir görüntü sınıflandırıcı derleme.
> * Cihazınızdaki Özel Görüntü İşleme Hizmeti web sunucusunu sorgulayan bir IoT Edge modülü dağıtma.
> * Görüntü sınıflandırıcı sonuçlarını IoT Hub'a gönderme.

<center>

![Diagram - Tutorial architecture, stage and deploy classifier](./media/tutorial-deploy-custom-vision/custom-vision-architecture.png)
</center>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

>[!TIP]
>This tutorial is a simplified version of the [Custom Vision and Azure IoT Edge on a Raspberry Pi 3](https://github.com/Azure-Samples/Custom-vision-service-iot-edge-raspberry-pi) sample project. This tutorial was designed to run on a cloud VM and uses static images to train and test the image classifier, which is useful for someone just starting to evaluate Custom Vision on IoT Edge. The sample project uses physical hardware and sets up a live camera feed to train and test the image classifier, which is useful for someone who wants to try a more detailed, real-life scenario.

Before beginning this tutorial, you should have gone through the previous tutorial to set up your environment for Linux container development: [Develop IoT Edge modules for Linux devices](tutorial-develop-for-linux.md). By completing that tutorial, you should have the following prerequisites in place: 

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* A [Linux device running Azure IoT Edge](quickstart-linux.md)
* A container registry, like [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) configured with the [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) configured to run Linux containers.

To develop an IoT Edge module with the Custom Vision service, install the following additional prerequisites on your development machine: 

* [Python](https://www.python.org/downloads/)
* [Git](https://git-scm.com/downloads)
* [Python extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 

## <a name="build-an-image-classifier-with-custom-vision"></a>Özel Görüntü İşleme Hizmeti ile bir görüntü sınıflandırıcı derleme

Görüntü sınıflandırıcı derlemek için bir Özel Görüntü İşleme Hizmeti projesi oluşturmanız ve eğitim amaçlı görüntü sağlamanız gerekir. Bu bölümde gerçekleştireceğiniz adımlar hakkında daha fazla bilgi için bkz. [Özel Görüntü İşleme Hizmeti ile sınıflandırıcı derleme](../cognitive-services/custom-vision-service/getting-started-build-a-classifier.md).

Görüntü sınıflandırıcıyı derleyip eğittikten sonra Docker kapsayıcısı olarak dışarı aktarabilir ve bir IoT Edge cihazına dağıtabilirsiniz. 

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Web tarayıcınızda [Özel Görüntü İşleme Hizmeti web sayfasına](https://customvision.ai/) gidin.

2. **Sign in** (Oturum aç) öğesini seçin ve Azure kaynaklarına erişmek için kullandığınız hesapla oturum açın. 

3. **New project** (Yeni proje) öğesini seçin.

4. Aşağıdaki değerlerle bir proje oluşturun:

   | Alan | Değer |
   | ----- | ----- |
   | Adı | Projeniz için bir ad belirleyin, örneğin: **EdgeTreeClassifier**. |
   | Açıklama | İsteğe bağlı proje açıklaması. |
   | Kaynak | Select one of your Azure resource groups that includes a Custom Vision Service resource or **create new** if you haven't yet added one. |
   | Proje Türleri | **Classification** (Sınıflandırma) |
   | Sınıflandırma Türleri | **Multiclass (single tag per image)** (Çok sınıflı (görüntü başına tek bir etiket)) |
   | Etki Alanları | **General (compact)** (Genel (kompakt)) |
   | Export Capabilities | **Basic platforms (Tensorflow, CoreML, ONNX, ...)** |

5. **Create project** (Proje oluştur) öğesini seçin.

### <a name="upload-images-and-train-your-classifier"></a>Görüntüleri karşıya yükleme ve sınıflandırıcınızı eğitme

Görüntü sınıflandırıcı oluşturmak için eğitim görüntülerine ve test görüntülerine ihtiyacınız vardır. 

1. [Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows) deposundaki örnek görüntüleri yerel geliştirme makinenize kopyalayın veya indirin. 

   ```cmd/sh
   git clone https://github.com/Microsoft/Cognitive-CustomVision-Windows.git
   ```

2. Özel Görüntü İşleme Hizmeti projenize dönün ve **Add images** (Görüntü ekle) öğesini seçin. 

3. Kopyaladığınız yerel git deposuna gidin ve ilk görüntü klasörünü açın: **Cognitive-CustomVision-Windows / Samples / Images / Hemlock**. Klasördeki 10 görüntüyü ve ardından **Open** (Aç) öğesini seçin. 

4. Bu görüntü grubuna **hemlock** etiketini ekleyin ve **Enter** tuşuna basarak etiketi uygulayın. 

5. **Upload 10 files** (10 dosyayı karşıya yükle) öğesini seçin. 

   ![Upload hemlock tagged files to Custom Vision](./media/tutorial-deploy-custom-vision/upload-hemlock.png)

6. Görüntüler başarıyla karşıya yüklendikten sonra **Done** (Bitti) öğesini seçin.

7. Yeniden **Add images** (Görüntü ekle) öğesini seçin.

8. İkinci görüntü klasörüne göz atın: **Cognitive-CustomVision-Windows / Samples / Images / Japanese Cherry**. Klasördeki 10 görüntüyü ve ardından **Open** (Aç) öğesini seçin. 

9. Bu görüntü grubuna **japanese cherry** etiketini ekleyin ve **Enter** tuşuna basarak etiketi uygulayın. 

10. **Upload 10 files** (10 dosyayı karşıya yükle) öğesini seçin. Görüntüler başarıyla karşıya yüklendikten sonra **Done** (Bitti) öğesini seçin. 

11. İki görüntü kümesini de etiketleyip yükledikten sonra sınıflandırıcıyı eğitmek için **Train** (Eğit) öğesini seçin. 

### <a name="export-your-classifier"></a>Sınıflandırıcınızı dışarı aktarma

1. Sınıflandırıcınızı eğittikten sonra sınıflandırıcının Performance (Performans) sayfasında **Export** (Dışarı aktar) öğesini seçin. 

   ![Export your trained image classifier](./media/tutorial-deploy-custom-vision/export.png)

2. Platform için **DockerFile** girişini seçin. 

3. Sürüm olarak **Linux** seçin.  

4. **Export** (Dışarı aktar) öğesini seçin. 

   ![Linux kapsayıcılarla DockerFile olarak dışarı aktarma](./media/tutorial-deploy-custom-vision/export-2.png)

5. Dışarı aktarma işlemi tamamlandıktan sonra **Download** (İndir) öğesini seçin ve .zip paketini bilgisayarınıza kaydedin. Paketteki tüm dosyaları ayıklayın. Bu dosyaları görüntü sınıflandırma sunucusunu içeren bir IoT Edge modülü oluşturmak için kullanacaksınız. 

Özel Görüntü İşleme Hizmeti projenizi oluşturma ve eğitme adımlarını tamamladınız. Dışarı aktarılan dosyaları bir sonraki bölümde kullanacaksınız ancak Özel Görüntü İşleme Hizmeti web sayfasıyla işiniz bitti. 

## <a name="create-an-iot-edge-solution"></a>IoT Edge çözümü oluşturma

Artık geliştirme makinenizde görüntü sınıflandırıcınızın kapsayıcı sürümü için gerekli dosyalara sahipsiniz. Bu bölümde görüntü sınıflandırıcı kapsayıcısını IoT Edge modülü olarak çalıştıracak şekilde yapılandıracaksınız. Ayrıca görüntü sınıflandırıcı ile birlikte dağıtılacak ikinci bir modül de oluşturacaksınız. İkinci modül istekleri sınıflandırıcıya, sonuçları da ileti olarak IoT Hub'a gönderir. 

### <a name="create-a-new-solution"></a>Yeni çözüm oluşturma

Çözüm, tek bir IoT Edge dağıtımı için birden fazla modül geliştirmek ve düzenlemek için kullanabileceğiniz mantıksal bir yoldur. Bir çözümde bir veya daha fazla modülün kodunun yanı sıra bunların bir IoT Edge cihazında nasıl yapılandırılacağını bildiren dağıtım bildirimi bulunur. 

1. VS Code komut paletini açmak için **View (Görünüm)**  > **Command Palette (Komut Paleti)** öğesini seçin. 

1. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için **CustomVisionSolution** gibi açıklayıcı bir ad girin veya varsayılan değeri kabul edin. |
   | Modül şablonunu seçin | **Python Modülü**'nü seçin. |
   | Modül adı sağlayın | Modülünüze **classifier** adını verin.<br><br>Modül adının küçük harfli olması önemlidir. IoT Edge, modüllere başvururken büyük/küçük harfe duyarlıdır ve bu çözümde tüm istekleri küçük harf ile biçimlendiren bir kitaplık kullanılmaktadır. |
   | Modül için Docker görüntü deposunu sağlama | Görüntü deposu, kapsayıcı kayıt defterinizin adını ve kapsayıcı görüntünüzün adını içerir. Kapsayıcı görüntünüz bir önceki adımdaki değerle önceden doldurulur. **localhost:5000** yerine Azure kapsayıcı kayıt defterinizden alacağınız oturum açma sunucusu değerini yazın. Oturum açma sunucusunu Azure portalda kapsayıcı kayıt defterinizin Genel bakış sayfasından alabilirsiniz.<br><br>The final string looks like **\<registry name\>.azurecr.io/classifier**. |
 
   ![Docker görüntü deposunu sağlama](./media/tutorial-deploy-custom-vision/repository.png)

Ardından Visual Studio Code penceresi IoT Edge çözümü çalışma alanınızı yükler.

### <a name="add-your-registry-credentials"></a>Kayıt defteri kimlik bilgilerinizi ekleme

Ortam dosyası, kapsayıcı kayıt defterinizin kimlik bilgilerini depolar ve bu bilgileri IoT Edge çalışma zamanı ile paylaşır. Çalışma zamanı, özel görüntülerinizi IoT Edge cihazına çekmek için bu kimlik bilgilerine ihtiyaç duyar.

1. VS Code gezgininde .env dosyasını açın.
2. Alanları Azure kapsayıcı kayıt defterinizden kopyaladığınız **kullanıcı adı** ve **parola** değerleriyle güncelleştirin.
3. Bu dosyayı kaydedin.

### <a name="select-your-target-architecture"></a>Select your target architecture

Currently, Visual Studio Code can develop modules for Linux AMD64 and Linux ARM32v7 devices. You need to select which architecture you're targeting with each solution, because the container is built and run differently for each architecture type. The default is Linux AMD64, which is what we'll use for this tutorial. 

1. Open the command palette and search for **Azure IoT Edge: Set Default Target Platform for Edge Solution**, or select the shortcut icon in the side bar at the bottom of the window. 

2. In the command palette, select the target architecture from the list of options. For this tutorial, we're using an Ubuntu virtual machine as the IoT Edge device, so will keep the default **amd64**. 

### <a name="add-your-image-classifier"></a>Görüntü sınıflandırıcınızı ekleme

Visual Studio Code uygulamasındaki Python modülü şablonu, IoT Edge örneğinizi test etmek için çalıştırabileceğiniz örnek kod içerir. Bu senaryoda bu kodu kullanmayacaksınız. Bunun yerine bu bölümdeki adımları kullanarak örnek kodu daha önceden dışarı aktardığınız görüntü sınıflandırıcı kapsayıcısıyla değiştirin. 

1. Dosya gezgininizde indirip ayıkladığınız Özel Görüntü İşleme Hizmeti paketine göz atın. Ayıklanan paketin içeriğin tamamını kopyalayın. **app** ve **azureml** adlı iki klasörün yanı sıra **Dockerfile** ve **README** adlı iki dosyanın bulunması gerekir. 

2. Dosya gezgininde Visual Studio Code'un IoT Edge çözümünüzü oluşturmasını istediğiniz dizine gidin. 

3. Sınıflandırıcı modülü klasörünü açın. Önceki bölümde önerilen adları kullandıysanız klasör yapısı şu şekilde olacaktır: **CustomVisionSolution / modules / classifier**. 

4. Dosyaları **classifier** klasörüne yapıştırın. 

5. Visual Studio Code penceresine dönün. Çözüm çalışma alanınızda görüntü sınıflandırıcı dosyaları modül klasöründe görünmelidir. 

   ![Görüntü sınıflandırıcı dosyalarını gösteren çözüm çalışma alanı](./media/tutorial-deploy-custom-vision/workspace.png)

6. Sınıflandırıcı klasöründeki **module.json** dosyasını açın. 

7. Update the **platforms** parameter to point to the new Dockerfile that you added, and remove all the options besides AMD64, which is the only architecture we're using for this tutorial. 

   ```json
   "platforms": {
       "amd64": "./Dockerfile"
   }
   ```

8. Yaptığınız değişiklikleri kaydedin. 

### <a name="create-a-simulated-camera-module"></a>Sanal kamera modülü oluşturma

Gerçek bir Özel Görüntü İşleme Hizmeti dağıtımında canlı görüntü veya video akışı sağlayan bir kameranız olacaktır. Bu senaryo için görüntü sınıflandırıcıya test görüntüsü gönderen bir modül oluşturarak kamera simülasyonu yapabilirsiniz. 

#### <a name="add-and-configure-a-new-module"></a>Yeni modül ekleme ve yapılandırma

Bu bölümde aynı CustomVisionSolution öğesine yeni bir modül ekleyecek ve sanal kamerayı oluşturmak için gerekli kodu sağlayacaksınız. 

1. Aynı Visual Studio Code penceresinde komut paletini kullanarak **Azure IoT Edge: Add IoT Edge Module** komutunu çalıştırın. Komut paletinde yeni modülünüz için aşağıdaki bilgileri girin: 

   | İstem | Değer | 
   | ------ | ----- |
   | Dağıtım şablonu dosyasını seçin | CustomVisionSolution klasöründeki deployment.template.json dosyasını seçin. |
   | Modül şablonunu seçin | **Python Modülü**'nü seçin |
   | Modül adı sağlayın | Modülünüze **cameraCapture** adını verin |
   | Modül için Docker görüntü deposunu sağlama | **localhost:5000** yerine Azure kapsayıcı kayıt defterinizin oturum açma sunucusu değerini yazın.<br><br>Dizenin son hali **\<registryname\>.azurecr.io/cameracapture** ifadesine benzer olmalıdır. |

   VS Code penceresi yeni modülünüzü çözüm çalışma alanında yükler ve deployment.template.json dosyasını güncelleştirir. Bu noktada iki modül klasörü görmeniz gerekir: classifier ve cameraCapture. 

2. **modules** / **cameraCapture** klasöründeki **main.py** dosyasını açın. 

3. Dosyanın tamamını aşağıdaki kod ile değiştirin. Bu kod örneği, sınıflandırıcı modülünde çalışan görüntü işleme hizmetine POST istekleri gönderir. Bu modül kapsayıcısı, isteklerde kullanabileceğiniz örnek bir görüntüyle sunulmaktadır. Modül ardından yanıtı bir IoT Hub iletisi olarak paketler ve bir çıkış kuyruğuna gönderir.  

    ```python
    # Copyright (c) Microsoft. All rights reserved.
    # Licensed under the MIT license. See LICENSE file in the project root for
    # full license information.

    import time
    import sys
    import os
    import requests
    import json
    from azure.iot.device import IoTHubModuleClient, Message

    # global counters
    SENT_IMAGES = 0

    # global client
    CLIENT = None

    # Send a message to IoT Hub
    # Route output1 to $upstream in deployment.template.json
    def send_to_hub(strMessage):
        message = Message(bytearray(strMessage, 'utf8'))
        CLIENT.send_message_to_output(message, "output1")
        global SENT_IMAGES
        SENT_IMAGES += 1
        print( "Total images sent: {}".format(SENT_IMAGES) )

    # Send an image to the image classifying server
    # Return the JSON response from the server with the prediction result
    def sendFrameForProcessing(imagePath, imageProcessingEndpoint):
        headers = {'Content-Type': 'application/octet-stream'}

        with open(imagePath, mode="rb") as test_image:
            try:
                response = requests.post(imageProcessingEndpoint, headers = headers, data = test_image)
                print("Response from classification service: (" + str(response.status_code) + ") " + json.dumps(response.json()) + "\n")
            except Exception as e:
                print(e)
                print("Response from classification service: (" + str(response.status_code))

        return json.dumps(response.json())

    def main(imagePath, imageProcessingEndpoint):
        try:
            print ( "Simulated camera module for Azure IoT Edge. Press Ctrl-C to exit." )

            try:
                global CLIENT
                CLIENT = IoTHubModuleClient.create_from_edge_environment()
            except Exception as iothub_error:
                print ( "Unexpected error {} from IoTHub".format(iothub_error) )
                return

            print ( "The sample is now sending images for processing and will indefinitely.")

            while True:
                classification = sendFrameForProcessing(imagePath, imageProcessingEndpoint)
                send_to_hub(classification)
                time.sleep(10)

        except KeyboardInterrupt:
            print ( "IoT Edge module sample stopped" )

    if __name__ == '__main__':
        try:
            # Retrieve the image location and image classifying server endpoint from container environment
            IMAGE_PATH = os.getenv('IMAGE_PATH', "")
            IMAGE_PROCESSING_ENDPOINT = os.getenv('IMAGE_PROCESSING_ENDPOINT', "")
        except ValueError as error:
            print ( error )
            sys.exit(1)

        if ((IMAGE_PATH and IMAGE_PROCESSING_ENDPOINT) != ""):
            main(IMAGE_PATH, IMAGE_PROCESSING_ENDPOINT)
        else: 
            print ( "Error: Image path or image-processing endpoint missing" )
    ```

4. **main.py** dosyasını kaydedin. 

5. **requrements.txt** dosyasını açın. 

6. Kapsayıcıya dahil etmek için yeni bir kitaplık satırı ekleyin.

   ```Text
   requests
   ```

7. **requirements.txt** dosyasını kaydedin.


#### <a name="add-a-test-image-to-the-container"></a>Kapsayıcıya test görüntüsü ekleme

Bu senaryoda görüntü beslemesi sağlamak için gerçek bir kamera kullanmak yerine tek bir test amaçlı görüntü kullanacağız. Bu öğreticinin önceki bölümlerinde eğitim görüntüleri için indirdiğiniz GitHub deposunda bir test görüntüsü bulunur. 

1. **Cognitive-CustomVision-Windows** / **Samples** / **Images** / **Test** konumundaki test görüntüsüne gidin. 

2. **test_image.jpg** dosyasını kopyalayın 

3. IoT Edge çözümünüzün dizinine gidin ve test görüntüsünü **modules** / **cameraCapture** klasörüne yapıştırın. Görüntü önceki bölümde düzenlediğiniz main.py dosyasıyla aynı klasörde olmalıdır. 

3. Visual Studio Code'da cameraCapture modülünün **Dockerfile.amd64** dosyasını açın. 

4. Çalışma dizinini belirleyen `WORKDIR /app` satırından sonra şu kod satırını ekleyin: 

   ```Dockerfile
   ADD ./test_image.jpg .
   ```

5. Dockerfile dosyasını kaydedin. 

### <a name="prepare-a-deployment-manifest"></a>Dağıtım bildirimi hazırlama

Bu öğreticide bu noktaya kadar ağaç görüntülerini sınıflandırmak için bir Özel Görüntü İşleme Hizmeti modeli eğittiniz ve bu modeli bir IoT Edge modülü olarak paketlediniz. Ardından görüntü sınıflandırma sunucusunu sorgulayıp sonuçlarını IoT Hub'a bildirebilecek ikinci bir modül oluşturdunuz. Artık bir IoT Edge cihazına bu iki modülü birlikte başlatma ve çalıştırma sürecini anlatacak dağıtım bildirimini oluşturmaya hazırsınız. 

Visual Studio Code için IoT Edge uzantısı dağıtım bildirimi oluşturmanıza yardımcı olmak için her IoT Edge çözümünde bir şablon oluşturur. 

1. Çözüm klasöründeki **deployment.template.json** dosyasını açın. 

2. Find the **modules** section, which should contain three modules: the two that you created, classifier and cameraCapture, and a third that's included by default, SimulatedTemperatureSensor. 

3. Delete the **SimulatedTemperatureSensor** module with all of its parameters. Bu modül, test senaryoları için örnek veri sağlamak için eklenmiştir ancak bu dağıtımda ihtiyacınız yoktur. 

4. Görüntü sınıflandırma modülüne **classifier** dışında bir ad verdiyseniz adı bu adımda kontrol edin ve tamamının küçük harf olduğundan emin olun. cameraCapture modülü, classifier modülünü tüm istekleri küçük harf olarak biçimlendiren istek kitaplığını kullanarak çağırır ve IoT Edge büyük/küçük harfe duyarlıdır. 

5. cameraCapture modülünün **createOptions** parametresini aşağıdaki JSON koduyla güncelleştirin. Bu bilgiler main.py işleminde alınan modül kapsayıcısında ortam değişkenlerini oluşturur. Bu bilgileri dağıtım bildirimine ekleyerek modül görüntüsünü yeniden derlemek zorunda kalmadan görüntüyü veya uç noktayı değiştirebilirsiniz. 

    ```json
    "createOptions": "{\"Env\":[\"IMAGE_PATH=test_image.jpg\",\"IMAGE_PROCESSING_ENDPOINT=http://classifier/image\"]}"
    ```

    Özel Görüntü İşleme Hizmeti modülünüzün adını *classifier* dışında bir değer olarak belirlediyseniz görüntü işleme uç nokta değerini uygun şekilde güncelleştirin. 

5. Dosyanın en altındaki $edgeHub modülünün **routes** parametresini güncelleştirin. Tahmin sonuçlarının cameraCapture kaynağından IoT Hub'a yönlendirilmesini istersiniz. 

    ```json
        "routes": {
          "CameraCaptureToIoTHub": "FROM /messages/modules/cameraCapture/outputs/* INTO $upstream"
        },
    ```

    İkinci modülün adını *cameraCapture* dışında bir değer olarak belirlediyseniz yönlendirme değerini uygun şekilde güncelleştirin. 

7. **deployment.template.json** dosyasını kaydedin.

## <a name="build-and-deploy-your-iot-edge-solution"></a>IoT Edge çözümünüzü oluşturma ve dağıtma

İki modülü de oluşturduğunuza ve dağıtım bildirimi şablonunu yapılandırdığınıza göre kapsayıcı görüntülerini oluşturmaya ve kapsayıcı kayıt defterinize göndermeye hazırsınız. 

Görüntüleri kayıt defterinize gönderdikten sonra çözümü bir IoT Edge cihazına dağıtabilirsiniz. IoT Hub üzerinden bir cihazda modül ayarlayabilirsiniz ancak IoT Hub ve cihazlara Visual Studio Code aracılığıyla da erişebilirsiniz. Bu bölümde IoT Hub'ınıza erişim ayarlarını yapacak ve ardından çözümünüzü IoT Edge cihazınıza dağıtmak için VS Code uygulamasını kullanacaksınız.

İlk olarak çözümünüzü oluşturun ve kapsayıcı kayıt defterinize gönderin. 

1. VS Code gezgininde **deployment.template.json** dosyasına sağ tıklayıp **Build and push IoT Edge solution** (IoT Edge Çözümü Oluştur ve Gönder) öğesini seçin. Bu işlemin ilerleme durumunu VS Code tümleşik terminalinde izleyebilirsiniz. 
2. Notice that a new folder was added to your solution, **config**. Expand this folder and open the **deployment.json** file inside.
3. deployment.json dosyasındaki bilgileri gözden geçirin. deployment.json dosyası; yapılandırdığınız dağıtım şablonu dosyasına ek olarak .env dosyası ve module.json dosyaları dahil olmak üzere çözümden alınan bilgilere göre otomatik olarak oluşturulur (veya güncelleştirilir). 

Next, select your device and deploy your solution.

1. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 
2. Dağıtımınızla hedeflemek istediğiniz cihaza sağ tıklayıp **Create deployment for single device** (Tek cihaz için dağıtım oluştur) öğesini seçin. 
3. Dosya gezgininde çözümünüzün içindeki **config** klasörüne gidip **deployment.json** dosyasını seçin. **Select Edge deployment manifest** (Edge dağıtım bildirimini seç) öğesine tıklayın. 

Dağıtım başarılı olursa VS Code çıkışında onay iletisi yazdırılır. VS Code gezgininde bu dağıtım için kullandığınız IoT Edge cihazıyla ilgili ayrıntıları genişletin. Modüller hemen görüntülenmezse imleci **Azure IoT Hub Devices** (Azure IoT Hub Cihazlar) başlığının üzerine getirerek yenileme düğmesini etkinleştirin. Modüllerin başlayıp IoT Hub'a bildirimde bulunması birkaç dakika sürebilir. 

Tüm modüllerin cihazınızda çalışıp çalışmadığını da kontrol edebilirsiniz. IoT Edge cihazında modüllerin durumunu görmek için aşağıdaki komutu çalıştırın. Modüllerin başlaması birkaç dakika sürebilir.

   ```bash
   iotedge list
   ```

## <a name="view-classification-results"></a>Sınıflandırma sonuçlarını görüntüleme

Modüllerinizin sonuçlarını görüntülemek için kullanabileceğiniz iki yöntem vardır. Bunu iletiler gönderilip alındıkça cihazın üzerinden veya iletiler IoT Hub'a ulaştıkça Visual Studio Code'dan gerçekleştirebilirsiniz. 

Cihazdan cameraCapture modülü günlüklerini görüntüleyerek iletilerin gönderildiğini ve IoT Hub tarafından alındığına dair onay verildiğini görebilirsiniz. 

   ```bash
   iotedge logs cameraCapture
   ```

From Visual Studio Code, right-click on the name of your IoT Edge device and select **Start Monitoring Built-in Event Endpoint**. 

cameraCapture modülünden iletiler olarak gönderilen Özel Görüntü İşleme Hizmeti modülü sonuçları, görüntünün köknar veya kiraz ağacı olma ihtimalini içerir. Görüntü köknar ağacına ait olduğundan olasılık değeri 1.0 olmalıdır. 


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz. 

Otherwise, you can delete the local configurations and the Azure resources that you used in this article to avoid charges. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Özel Görüntü İşleme Hizmeti modeli eğittiniz ve bunu modül olarak bir IoT Edge cihazına dağıttınız. Ardından görüntü sınıflandırma hizmetini sorgulayıp sonuçlarını IoT Hub'a bildirebilecek bir modül oluşturdunuz. 

Bu senaryonun canlı kamera akışı kullanan daha ayrıntılı bir sürümünü denemek isterseniz [Custom Vision and Azure IoT Edge on a Raspberry Pi 3](https://github.com/Azure-Samples/Custom-vision-service-iot-edge-raspberry-pi) (Raspberry Pi 3 üzerinde Özel Görüntü İşleme Hizmeti ve Azure IoT Edge) GitHub projesine bakın. 

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [SQL Server veritabanları ile uç cihazlarda veri depolama](tutorial-store-data-sql-server.md)
