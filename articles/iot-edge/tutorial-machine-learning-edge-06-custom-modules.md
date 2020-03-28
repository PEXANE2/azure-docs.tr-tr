---
title: "Öğretici: Özel modüller oluşturma ve dağıtma - Azure IoT Edge'de Makine Öğrenimi"
description: Bu öğretici, bir makine öğrenme modeli aracılığıyla yaprak aygıtlardan gelen verileri işleyen ve ardından bilgileri IoT Hub'a gönderen IoT Edge modüllerinin nasıl oluşturulup dağıtılanedileceğini gösterir.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3cba7781ac80ae567b2bfd54c4131429ed94b90f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75772372"
---
# <a name="tutorial-create-and-deploy-custom-iot-edge-modules"></a>Öğretici: Özel IoT Edge modülleri oluşturun ve dağıtın

> [!NOTE]
> Bu makale, IoT Edge'de Azure Machine Learning'i kullanma yla ilgili bir öğretici için bir serinin parçasıdır. Bu makaleye doğrudan ulaştıysanız, en iyi sonuçlar için serinin [ilk makalesiile](tutorial-machine-learning-edge-01-intro.md) başlamanızı öneririz.

Bu makalede, yaprak aygıtlardan ileti alan, verileri makine öğrenimi modelinizde çalıştıran ve ardından öngörüleri IoT Hub'a ileten üç IoT Edge modülü oluşturuyoruz.

IoT Edge hub modülü modül iletişimini kolaylaştırır. IoT Edge hub'ını ileti aracısı olarak kullanmak modülleri birbirinden bağımsız tutar. Modüllerin yalnızca iletileri kabul ettikleri girdileri ve ileti yazdıkları çıktıları belirtmeleri gerekir.

IoT Edge cihazının bizim için dört şeyi başarmasını istiyoruz:

* Yaprak aygıtlardan veri alma
* Verileri gönderen aygıt için kalan yararlı ömrü (RUL) tahmin etme
* Aygıt için yalnızca RUL ile Birlikte IoT Hub'a ileti gönderin (bu işlev yalnızca RUL bir seviyenin altına düşerse veri göndermek için değiştirilebilir)
* Yaprak aygıt verilerini IoT Edge aygıtındaki yerel bir dosyaya kaydedin. Bu veri dosyası, makine öğrenme modelinin eğitimini iyileştirmek için dosya yükleme yoluyla düzenli olarak IoT Hub'a yüklenir. Sürekli ileti akışı yerine dosya yüklemeyi kullanmak daha uygun maliyetlidir.

Bu görevleri gerçekleştirmek için üç özel modül kullanırız:

* **RUL Sınıflandırıcı:** Train'de oluşturduğumuz ve Azure [Machine Learning modelini dağıtan](tutorial-machine-learning-edge-04-train-model.md) turboFanRulClassifier modülü, "amlInput" adı verilen bir girişi ve "amlOutput" adlı çıktıyı ortaya çıkaran standart bir makine öğrenimi modülüdür. "amlInput" girişinin ACI tabanlı web hizmetine gönderdiğimiz girdiye tam olarak benzemesini bekler. Aynı şekilde, "amlOutput" web hizmeti yle aynı verileri döndürür.

* **Avro yazar:** Bu modül "avroModuleInput" girişinden iletialır ve daha sonra IoT Hub'a yüklemek üzere Avro formatında iletiyi diske olarak devam eder.

* **Yönlendirici Modülü:** Yönlendirici modülü, akış aşağı yaprak aygıtlardan iletileri alır, ardından biçimleri ve sınıflandırıcıya iletileri gönderir. Modül daha sonra sınıflandırıcıdan iletileri alır ve iletiyi Avro yazar modülüne ileter. Son olarak, modül IoT Hub'a sadece RUL tahmingönderir.

  * Giriş:
    * **deviceInput**: yaprak cihazlardan mesaj alır
    * **rulInput:** "amlOutput" iletileri alır

  * Çıkış:
    * **sınıflandırmak:** "amlInput" iletileri gönderir
    * **writeAvro:** "avroModuleInput" iletileri gönderir
    * **toIotHub:** iletileri bağlı IoT Hub'ına ileten $upstream iletileri gönderir

Aşağıdaki diyagram, tam çözüm için modülleri, girdileri, çıktıları ve IoT Edge Hub rotalarını gösterir:

![IoT Edge üç modül mimari diyagramı](media/tutorial-machine-learning-edge-06-custom-modules/modules-diagram.png)

Bu makaledeki adımlar genellikle bir bulut geliştiricisi tarafından gerçekleştirilir.

## <a name="create-a-new-iot-edge-solution"></a>Yeni bir IoT Edge çözümü oluşturun

İki Azure Not Defterimizin ikincisinin yürütülmesi sırasında, RUL modelimizi içeren bir kapsayıcı görüntüsü oluşturduk ve yayınladık. Azure Machine Learning, görüntü oluşturma işleminin bir parçası olarak, görüntünün Azure IoT Edge modülü olarak dağıtılabildiği bu modeli paketledi. Bu adımda, "Azure Machine Learning" modüllerini kullanarak bir Azure IoT Edge çözümü oluşturacağız ve modülü Azure Dizüstü Bilgisayarlar kullanarak yayınladığımız resme yönlendireceğiz.

1. Geliştirme makinenize uzak bir masaüstü oturumu açın.

2. Açık klasör **C:\\Visual Studio Code kaynak\\IoTEdgeAndMlSample.**

3. Explorer paneline sağ tıklayın (boş alanda) ve **Yeni IoT Edge Solution'ı**seçin.

    ![Yeni IoT Edge çözümü oluşturun](media/tutorial-machine-learning-edge-06-custom-modules/new-edge-solution-command.png)

4. Varsayılan çözüm adını **EdgeSolution**kabul edin.

5. Modül şablonu olarak **Azure Machine Learning'i** seçin.

6. Modül **turbofanRulClassifier**adı .

7. Makine öğrenimi çalışma alanınızı seçin.

8. Azure Not Defteri'ni çalıştırırken oluşturduğunuz resmi seçin.

9. Çözüme bakın ve oluşturulan dosyalara dikkat edin:

   * **deployment.template.json:** Bu dosya, çözümdeki modüllerin her birinin tanımını içerir. Bu dosyada dikkat etmek için üç bölüm vardır:

     * **Kayıt defteri kimlik bilgileri:** Çözümünde kullandığınız özel kapsayıcı kayıt defteri kümesini tanımlar. Şu anda, Azure Machine Learning görüntünüzün depolandığı makine öğrenimi çalışma alanınızdaki kayıt defterini içermelidir. Herhangi bir sayıda konteyner kayıt defterine sahip olabilirsiniz, ancak basitlik için tüm modüller için bu tek kayıt defterini kullanacağız

       ```json
       "registryCredentials": {
         "<your registry>": {
           "username": "$CONTAINER_REGISTRY_USERNAME_<your registry>",
           "password": "$CONTAINER_REGISTRY_PASSWORD_<your registry>",
           "address": "<your registry>.azurecr.io"
         }
       }
       ```

     * **Modüller:** Bu bölüm, bu çözümle birlikte kullanıcı tanımlı modüller kümesini içerir. Bu bölümde şu anda iki modül içerdiğini fark edeceksiniz: SimüleTemperatureSensor ve turbofanRulClassifier. Simüle TemperatureSensor Visual Studio Code şablonu tarafından yüklendi, ancak bu çözüm için ihtiyacımız yok. Simüle EdilenTemperatureSensor modül tanımını modüller bölümünden silebilirsiniz. TurbofanRulClassifier modülü tanımının konteyner kayıt defterinizdeki resme işaret ettiğini unutmayın. Çözüme daha fazla modül ekledikçe, bu bölümde gösterecektir.

       ```json
       "modules": {
         "SimulatedTemperatureSensor": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
             "createOptions": {}
           }
         },
         "turbofanRulClassifier": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "<your registry>.azurecr.io/edgemlsample:1",
             "createOptions": {}
           }
         }
       }
       ```

     * **Rotalar:** Bu eğitimde rotalar ile biraz çalışacağız. Rotalar, modüllerin birbirleriyle nasıl iletişim kurduğunu tanımlar. Şablon tarafından tanımlanan iki yol, ihtiyacımız olan yönlendirmeyle eşleşmiyor. İlk rota, sınıflandırıcının herhangi bir çıktısından elde edilen tüm verileri IoT Hub'ına ($upstream) gönderir. Diğer rota simüle etmek için, biz sadece silindi. İki varsayılan yolu silin.

       ```json
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
             "turbofanRulClassifierToIoTHub": "FROM /messages/modules/turbofanRulClassifier/outputs/\* INTO $upstream",
             "sensorToturbofanRulClassifier": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\\"/modules/turbofanRulClassifier/inputs/input1\\")"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       }
       ```

   * **deployment.debug.template.json:** Bu dosya deployment.template.json hata ayıklama sürümüdür. Deployment.template.json'daki tüm değişiklikleri bu dosyaya yansıtmalıyız.

   * **.env:** Bu dosya, kayıt defterinize erişmek için kullanıcı adı ve parolasağlamanız gereken dosyadır.

      ```env
      CONTAINER_REGISTRY_USERNAME_<your registry name>=<ACR username>
      CONTAINER_REGISTRY_PASSWORD_<your registry name>=<ACR password>
      ```

10. Visual Studio Code explorer'da deployment.template.json dosyasına sağ tıklayın ve **Yapı IoT Edge Solution'ı**seçin.

11. Bu komutun deployment.amd64.json dosyası içeren bir config klasörü oluşturduğuna dikkat edin. Bu dosya, çözüm için somut dağıtım şablonudur.

## <a name="add-router-module"></a>Yönlendirici modülü ekle

Daha sonra, çözüme Yönlendirici modüllerini ekliyoruz. Yönlendirici modülü çözümümüz için çeşitli sorumlulukları yerine getirir:

* **Yaprak aygıtlardan ileti alın:** İletiler alt aygıtlardan IoT Edge aygıtına geldiğinde, Yönlendirici modülü iletiyi alır ve iletinin yönlendirmesini düzenlemeye başlar.
* **RUL Sınıflandırıcı modülüne ileti gönder:** Bir akış aşağı aygıtından yeni bir ileti alındığı zaman, Yönlendirici modülü iletiyi RUL Sınıflandırıcının beklediği biçime dönüştürür. Yönlendirici, RUL öngörüsü için iletiyi RUL Sınıflandırıcısına gönderir. Sınıflandırıcı bir tahminde bulunduktan sonra, iletiyi Yönlendirici modülüne geri gönderir.
* **IoT Hub'ına RUL iletileri gönderin:** Yönlendirici sınıflandırıcıdan ileti aldığında, iletiyi yalnızca temel bilgileri, aygıt kimliğini ve RUL'u içerecek şekilde dönüştürür ve kısaltılmış iletiyi IoT hub'ına gönderir. Burada yapmadığımız bir diğer ayrıntılandırma, IoT Hub'ına yalnızca RUL tahmini bir eşiğin altına düştüğünde (örneğin, RUL 100 döngüden az olduğunda) ileti gönderir. Bu şekilde filtreleme iletilerin hacmini azaltır ve IoT hub maliyetini azaltır.
* **Avro Writer modülüne mesaj gönderin:** Aşağı akış aygıtı tarafından gönderilen tüm verileri korumak için, Yönlendirici modülü sınıflandırıcıdan alınan tüm mesajı Avro Writer modülüne gönderir ve bu da ioT Hub dosya yüklemesini kullanarak verileri yükler.

> [!NOTE]
> Modül sorumluluklarının açıklaması işlemenin sıralı görünmesini sağlayabilir, ancak akış ileti/olay tabanlıdır. Bu yüzden Router modülümüz gibi bir orkestrasyon modülüne ihtiyacımız var.

### <a name="create-module-and-copy-files"></a>Modül oluşturma ve dosyaları kopyalama

1. Visual Studio Code'daki modüller klasörüne sağ tıklayın ve **IoT Edge Modül Ekle'yi**seçin.

2. **C# modüllerini**seçin.

3. Modül **turbofanRouter**adı .

4. Docker Image Deponuz için istendiğinde, makine öğrenimi çalışma alanından kayıt defterini kullanın *(deployment.template.json* dosyanızın kayıt defteri kimlik bilgileri düğümünde bulabilirsiniz). Bu değer, ** \<kayıt defteriniz\>** gibi kayıt defterinin tam nitelikli adresidir .azurecr.io/turbofanrouter.

    > [!NOTE]
    > Bu makalede, sınıflandırıcımızı eğitmek ve dağıtmak için kullandığımız Azure Machine Learning çalışma alanı tarafından oluşturulan Azure Konteyner Kayıt Defteri'ni kullanıyoruz. Bu tamamen kolaylık sağlamak için. Yeni bir konteyner kayıt defteri oluşturup modüllerimizi orada yayınlayabilirdik.

5. Visual Studio Code **(View** > **Terminal)**'de yeni bir terminal penceresi açın ve modül dizindeki dosyaları kopyalayın.

    ```cmd
    copy c:\source\IoTEdgeAndMlSample\EdgeModules\modules\turbofanRouter\*.cs c:\source\IoTEdgeAndMlSample\EdgeSolution\modules\turbofanRouter\
    ```

6. program.cs üzerine yazmaları istendiğinde, `y` basın `Enter`ve sonra vurun.

### <a name="build-router-module"></a>Yönlendirici modülü oluşturma

1. Visual Studio Code'da **Terminal** > **Yapılandırma Varsayılan Yapı Görevi'ni**seçin.

2. **Şablondan tasks.json dosyası oluştur'a**tıklayın.

3. **.NET Core'a**tıklayın.

4. tasks.json açıldığında içindekileri aşağıdakilerle değiştirin:

    ```json
    {
      // See https://go.microsoft.com/fwlink/?LinkId=733558
      // for the documentation about the tasks.json format
      "version": "2.0.0",
      "tasks": [
        {
          "label": "build",
          "command": "dotnet",
          "type": "shell",
          "group": {
            "kind": "build",
            "isDefault": true
          },
          "args": [
            "build",
            "${workspaceFolder}/modules/turbofanRouter"
          ],
          "presentation": {
            "reveal": "always"
          },
          "problemMatcher": "$msCompile"
        }
      ]
    }
    ```

5. Görevleri kaydet ve kapat.json.

6. Yapıyı `Ctrl + Shift + B` ile çalıştır ın veya **Terminal** > **Run Build Task'ı çalıştırın.**

### <a name="set-up-module-routes"></a>Modül rotalarını ayarlama

Yukarıda belirtildiği gibi, IoT Edge çalışma zamanı, gevşek birleştirilmiş modüller arasındaki iletişimi yönetmek için *deployment.template.json* dosyasında yapılandırılan yolları kullanır. Bu bölümde, turbofanRouter modülü için yolların nasıl ayarlanır diye çalışıyoruz. Önce giriş yollarını kaplayacak, sonra çıkışlara geçeceğiz.

#### <a name="inputs"></a>Girişler

1. init() Program.cs yönteminde modül için iki geri arama kaydederiz:

   ```csharp
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromLeafDevice, LeafDeviceInputMessageHandler, ioTHubModuleClient);
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromClassifier, ClassifierCallbackMessageHandler, ioTHubModuleClient);
   ```

2. İlk geri arama, **deviceInput** lavaboya gönderilen iletileri dinler. Yukarıdaki diyagramdan, iletileri herhangi bir yaprak cihazdan bu girişe yönlendirmek istediğimizi görüyoruz. *deployment.template.json* dosyasında, IoT Edge aygıtı tarafından alınan ve IoT Edge modülü tarafından alınmayan iletileri turbofanRouter modülündeki "deviceInput" adlı girişe yönlendirmesini söyleyen bir rota ekleyin:

   ```json
   "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")"
   ```

3. Sonraki turbofanRouter modülüiçine rulClassifier modülünden iletiler için bir rota ekleyin:

   ```json
   "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amloutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")"
   ```

#### <a name="outputs"></a>Çıkışlar

Yönlendirici modülünden çıktıları işlemek için $edgeHub rota parametresine dört ek rota ekleyin.

1. Program.cs, rotayı kullanarak RUL sınıflandırıcısına ileti göndermek için modül istemcisini kullanan SendMessageToClassifier() yöntemini tanımlar:

   ```json
   "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")"
   ```

2. SendRulMessageToIotHub() rota üzerinden IoT Hub'a aygıt için sadece RUL verilerini göndermek için modül istemcisini kullanır:

   ```json
   "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream"
   ```

3. SendMessageToAvroWriter() avroFileWriter modülüne eklenen RUL verileri ile iletiyi göndermek için modül istemcisini kullanır.

   ```json
   "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")"
   ```

4. HandleBadMessage() başarısız iletileri IoT Hub'ın yukarısına gönderir ve bunlar daha sonra yönlendirilebilir.

   ```json
   "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
   ```

Birlikte alınan tüm yolları ile "$edgeHub" düğüm aşağıdaki JSON gibi görünmelidir:

```json
"$edgeHub": {
  "properties.desired": {
    "schemaVersion": "1.0",
    "routes": {
      "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")",
      "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amlOutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")",
      "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")",
      "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream",
      "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")",
      "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
    },
    "storeAndForwardConfiguration": {
      "timeToLiveSecs": 7200
    }
  }
}
```

> [!NOTE]
> TurbofanRouter modülü nün eklenmesi aşağıdaki `turbofanRouterToIoTHub": "FROM /messages/modules/turbofanRouter/outputs/* INTO $upstream`ek rotayı oluşturdu: . Yalnızca deployment.template.json dosyanızda yalnızca yukarıda listelenen yolları bırakarak bu rotayı kaldırın.

#### <a name="copy-routes-to-deploymentdebugtemplatejson"></a>Deployment.debug.template.json yollarını kopyalama

Son adım olarak, dosyalarımızı eşit tutmak için deployment.template.json in deployment.debug.template.json'da yaptığınız değişiklikleri yansıtın.

## <a name="add-avro-writer-module"></a>Avro Yazar modülü ekle

Avro Yazar modülü, mesajları depolamak ve dosya yüklemek olmak üzere çözümümüzde iki sorumluluk taslar.

* **İletileri depola**: Avro Writer modülü bir ileti aldığında, iletiyi Avro formatında yerel dosya sistemine yazar. Bir dizin (bu durumda /veri/avrofiles) modülün kapsayıcısındaki bir yola monte eden bir bağlama yuvası kullanırız. Bu montaj, modülün yerel bir yola (/avrofiles) yazmasını ve bu dosyalara doğrudan IoT Edge aygıtından erişebiliyor olmasını sağlar.

* **Dosya yükleme**: Avro Writer modülü, dosyaları bir Azure depolama hesabına yüklemek için Azure IoT Hub dosya yükleme özelliğini kullanır. Bir dosya başarıyla yüklendikten sonra, modül dosyayı diskten siler

### <a name="create-module-and-copy-files"></a>Modül oluşturma ve dosyaları kopyalama

1. Komut paletinde **Python'u seçin: Yorumlayıcı'yı seçin.**

1. C:\\Python37'de bulunan yorumlayıcıyı seçin.

1. Komut paletini yeniden açın ve **terminali seçin: Varsayılan Kabuk'u seçin.**

1. İstendiğinde Komut **İstemi'ni**seçin.

1. Yeni bir terminal kabuğu açın, **Terminal** > **Yeni Terminal.**

1. Visual Studio Code'daki modüller klasörüne sağ tıklayın ve **IoT Edge Modül Ekle'yi**seçin.

1. **Python Modülü**'nü seçin.

1. Modülü "avroFileWriter" olarak adlandırın.

1. Docker Image Deponuz için istendiğinde, Yönlendirici modüllerini eklerken kullandığınız kayıt defterini kullanın.

1. Örnek modüldeki dosyaları çözüme kopyalayın.

   ```cmd
   copy C:\source\IoTEdgeAndMlSample\EdgeModules\modules\avroFileWriter\*.py C:\source\IoTEdgeAndMlSample\EdgeSolution\modules\avroFileWriter\
   ```

1. main.py üzerine yazmak istenirse, `y` yazın `Enter`ve sonra tuşuna basın.

1. Çözüme filemanager.py ve schema.py eklendiğini ve main.py güncelleştirildiğini unutmayın.

> [!NOTE]
> Bir Python dosyasını açtığınızda, pilil yüklemeniz istenebilir. Bu öğreticiyi tamamlamak için linter'i yüklemeniz gerekmez.

### <a name="bind-mount-for-data-files"></a>Veri dosyaları için bağlama montajı

Girişte belirtildiği gibi, yazar modülü aygıtın dosya sistemine Avro dosyalarını yazmak için bind montaj varlığına dayanır.

#### <a name="add-directory-to-device"></a>Aygıta dizin ekleme

1. SSH kullanarak IoT Edge cihazınızvm'a bağlanın.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Kaydedilen yaprak aygıt iletilerini tutacak dizin oluşturun.

   ```bash
   sudo mkdir -p /data/avrofiles
   ```

3. Kapsayıcı tarafından yazılabilir hale getirmek için dizin izinlerini güncelleştirin.

   ```bash
   sudo chmod ugo+rw /data/avrofiles
   ```

4. Dizin şimdi kullanıcı, grup ve sahibi için yazma (w) izni vardır doğrulayın.

   ```bash
   ls -la /data
   ```

   ![Avrofiles için dizin izinleri](media/tutorial-machine-learning-edge-06-custom-modules/avrofiles-directory-permissions.png)

#### <a name="add-directory-to-the-module"></a>Modüle dizin ekleme

Modülün kapsayıcısına dizin eklemek için avroFileWriter modülüyle ilişkili Dockerdosyalarını değiştireceğiz. Modülle ilişkili üç Dockerfiles vardır: Dockerfile.amd64, Dockerfile.amd64.debug ve Dockerfile.arm32v7. Bir arm32 aygıtına hata ayıklamak veya dağıtmak istersek bu dosyalar eşit tutulmalıdır. Bu makale için, sadece Dockerfile.amd64 odaklanın.

1. Geliştirme makinenizde **Dockerfile.amd64** dosyasını açın.

2. Aşağıdaki örnek gibi görünen dosyayı değiştirin:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

   Ve `mkdir` `chown` komutlar, Docker yapı işlemini görüntüdeki /avrofiles adlı üst düzey bir dizin oluşturmak ve daha sonra modül kullanıcısını bu dizinin sahibi yapmak için yönlendirir. Bu komutların, modül kullanıcısı `useradd` komutla görüntüye eklendikten sonra ve bağlam modül kullanıcısına (USER moduleuser) geçmeden önce eklenmesi önemlidir.

3. Dockerfile.amd64.debug ve Dockerfile.arm32v7 ilgili değişiklikleri yapın.

#### <a name="update-the-module-configuration"></a>Modül yapılandırmasını güncelleştirme

Bind oluşturmanın son adımı, deployment.template.json (ve deployment.debug.template.json) dosyalarını bağlama bilgileriyle güncelleştirmektir.

1. Deployment.template.json'u açın.

2. Kenar aygıtındaki yerel dizine kapsayıcı `Binds` dizini /avrodosyalarını işaret eden parametreyi ekleyerek avroFileWriter için modül tanımını değiştirin. Modül tanımınız şu örnekle eşleşmelidir:

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles"
           ]
         }
       }
     }
   }
   ```

3. Deployment.debug.template.json'da karşılık gelen değişiklikleri yapın.

### <a name="bind-mount-for-access-to-configyaml"></a>Config.yaml erişim için bind montaj

Yazar modülü için bir bağlama daha eklemeliyiz. Bu bağlama, modüle IoT Edge aygıtındaki /etc/iotedge/config.yaml dosyasından bağlantı dizesini okuma olanağı sağlar. IoTHubClient oluşturmak için bağlantı dizesine ihtiyacımız var,\_böylece\_dosyaları IoT hub'ına yüklemek için yükleme blob async yöntemini arayabiliyoruz. Bu bağlama ekleme adımları önceki bölümdekilere benzer.

#### <a name="update-directory-permission"></a>Dizin izinin güncelleştirin

1. SSH kullanarak IoT Edge cihazınıza bağlanın.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. config.yaml dosyasına okuma izni ekleyin.

   ```bash
   sudo chmod +r /etc/iotedge/config.yaml
   ```

3. İzinleri doğru ayarlayın.

   ```bash
   ls -la /etc/iotedge/
   ```

4. config.yaml izinlerinin **-r-r-r--**.

#### <a name="add-directory-to-module"></a>Modüle dizin ekleme

1. Geliştirme makinenizde **Dockerfile.amd64** dosyasını açın.

2. Aşağıdaki gibi görünmesi `mkdir` `chown` için dosyaya ek bir küme ve komut ekleyin:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig

   USER moduleuser

   CMD "python3", "-u", "./main.py"]
   ```

3. Dockerfile.amd64.debug ve Dockerfile.arm32v7 ilgili değişiklikleri yapın.

#### <a name="update-the-module-configuration"></a>Modül yapılandırmasını güncelleştirme

1. **deployment.template.json** dosyasını açın.

2. AvroFileWriter için modül tanımını, konteyner dizini (/app/iotconfig) aygıttaki yerel dizine (/etc/iotedge) işaret eden `Binds` parametreye ikinci bir satır ekleyerek değiştirin.

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles",
             "/etc/iotedge:/app/iotconfig"
           ]
         }
       }
     }
   }
   ```

3. Deployment.debug.template.json'da karşılık gelen değişiklikleri yapın.

## <a name="install-dependencies"></a>Bağımlılıkları yükleme

Yazar modülü iki Python kitaplıkları, fastavro ve PyYAML bir bağımlılık alır. Geliştirme makinemize bağımlılıkları yüklememiz ve Docker yapı işlemini modülümüzün görüntüsüne yüklemeleri için talimat vermemiz gerekir.

### <a name="pyyaml"></a>PyYAML

1. Geliştirme **makinenizde, requirements.txt** dosyasını açın ve pyyaml ekleyin.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   ```

2. **Dockerfile.amd64** dosyasını açın `pip install` ve kurulum araçlarını yükseltmek için bir komut ekleyin.

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && \
       apt-get install -y --no-install-recommends libcurl4-openssl-dev python3-pip libboost-python1.58-dev libpython3-dev && \
       rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   RUN pip install -U pip setuptools
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

3. Dockerfile.amd64.debug için karşılık gelen değişiklikleri yapın. <!--may not be necessary. Add 'if needed'?-->

4. Visual Studio Code'da bir terminal açarak ve yazarak pyyaml'ı yerel olarak yükleyin

   ```cmd
   pip install pyyaml
   ```

### <a name="fastavro"></a>Fastavro

1. requirements.txt olarak, pyyaml sonra fastavro ekleyin.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   fastavro
   ```

2. Visual Studio Code terminalini kullanarak geliştirme makinenize fastavro yükleyin.

   ```cmd
   pip install fastavro
   ```

## <a name="reconfigure-iot-hub"></a>IoT Hub'ı yeniden yapılandırma

IoT Edge cihazını ve modüllerini sisteme tanıtarak, hub'a hangi verilerin ne amaçla gönderileeceğine ilişkin beklentilerimizi değiştirdik. Yeni realitemizle başa çıkmak için merkezdeki yönlendirmeyi yeniden yapılandırmalıyız.

> [!NOTE]
> Modülleri dağıtmadan önce hub'ı yeniden yapılandırıyoruz, çünkü bazı hub ayarlarının, özellikle dosya yüklemesinin avroFileWriter modülünün doğru çalışması için doğru şekilde ayarlanması gerekiyor

### <a name="set-up-route-for-rul-messages-in-iot-hub"></a>IoT Hub'da RUL iletileri için rota ayarlama

Yönlendirici ve sınıflandırıcı yerinde olduğu için, yalnızca aygıt kimliğini ve aygıt için RUL tahminini içeren düzenli iletiler almayı bekliyoruz. RUL verilerini, cihazların durumunu izleyebileceğimiz, raporlar oluşturabileceğimiz ve gerektiğinde yangın uyarıları yapabileceğimiz kendi depolama konumuna yönlendirmek istiyoruz. Aynı zamanda, mevcut depolama konumuna yönlendirmeye devam etmek için ioT Edge aygıtımıza henüz iliştirilmiş bir yaprak aygıt tarafından hala doğrudan gönderilen tüm aygıt verilerinin de olmasını istiyoruz.

#### <a name="create-a-rul-message-route"></a>RUL ileti rotası oluşturma

1. Azure portalında IoT Hub'ınıza gidin.

2. Sol gezintiden **İleti yönlendirmesini**seçin.

3. **Ekle'yi**seçin.

4. Rotayı **RulMessageRoute**olarak adlandırın.

5. **Bitiş Noktası** seçicinin yanına **Ekle'yi** seçin ve **Blob depolama alanını**seçin.

6. Depolama **bitiş noktası** formunda, bitiş noktası **ruldata'yı**adlandırın.

7. **Kapsayıcı seç'i**seçin.

8. **Iotedgeandml\<benzersiz soneki\>** gibi adlandırılır bu öğretici boyunca kullanılan depolama hesabı seçin.

9. **Ruldata** kapsayıcısını seçin ve **Seç'i**tıklatın.

10. Depolama bitiş noktasını oluşturmak için **Oluştur'u** tıklatın.

11. Yönlendirme **sorgusu**için aşağıdaki sorguyu girin:

    ```sql
    IS_DEFINED($body.PredictedRul) AND NOT IS_DEFINED($body.OperationalSetting1)
    ```

12. **Test** bölümünü ve ardından **İleti gövdesi** bölümünü genişletin. İletiyi beklenen iletilerimizin bu örneğiyle değiştirin:

    ```json
    {
      "ConnectionDeviceId": "aaLeafDevice_1",
      "CorrelationId": "b27e97bb-06c5-4553-a064-e9ad59c0fdd3",
      "PredictedRul": 132.62721409309165,
      "CycleTime": 64.0
    }
    ```

13. **Test rotasını**seçin. Test başarılı olursa, "İleti sorguyla eşleşti."

14. **Kaydet**'e tıklayın.

#### <a name="update-turbofandevicetostorage-route"></a>TurbofanDeviceToDepolama rotasını güncelleştirin

Yeni tahmin verilerini eski depolama konumumuza yönlendirmek istemiyoruz, bu nedenle bunu önlemek için rotayı güncelleştirin.

1. IoT Hub **İleti yönlendirme** sayfasında **Rotalar** sekmesini seçin.

2. **TurbofanDeviceDataToStorage'ı**veya ilk cihaz veri rotanıza hangi adı verdiyseniz seçin.

3. Yönlendirme sorgusunu güncelleştirme

   ```sql
   IS_DEFINED($body.OperationalSetting1)
   ```

4. **Test** bölümünü ve ardından **İleti gövdesi** bölümünü genişletin. İletiyi beklenen iletilerimizin bu örneğiyle değiştirin:

   ```json
   {
     "Sensor13": 2387.96,
     "OperationalSetting1": -0.0008,
     "Sensor6": 21.61,
     "Sensor11": 47.2,
     "Sensor9": 9061.45,
     "Sensor4": 1397.86,
     "Sensor14": 8140.39,
     "Sensor18": 2388.0,
     "Sensor12": 522.87,
     "Sensor2": 642.42,
     "Sensor17": 391.0,
     "OperationalSetting3": 100.0,
     "Sensor1": 518.67,
     "OperationalSetting2": 0.0002,
     "Sensor20": 39.03,
     "DeviceId": 19.0,
     "Sensor5": 14.62,
     "PredictedRul": 212.00132402791962,
     "Sensor8": 2388.01,
     "Sensor16": 0.03,
     "CycleTime": 42.0,
     "Sensor21": 23.3188,
     "Sensor15": 8.3773,
     "Sensor3": 1580.09,
     "Sensor10": 1.3,
     "Sensor7": 554.57,
     "Sensor19": 100.0
   }
   ```

5. **Test rotasını**seçin. Test başarılı olursa, "İleti sorguyla eşleşti."

6. **Kaydet'i**seçin.

### <a name="configure-file-upload"></a>Karşıya dosya yüklemeyi yapılandırma

Dosya yazar modülünün dosyaları depolama alanına yüklemesini sağlamak için IoT Hub dosya yükleme özelliğini yapılandırın.

1. IoT Hub'ınızdaki sol gezginden **Dosya yükleme'yi**seçin.

2. **Azure Depolama Kapsayıcısı'nı**seçin.

3. Listeden depolama hesabınızı seçin.

4. **Uploadturbofanfiles** kapsayıcısını seçin ve **Seç'i**tıklatın.

5. **Kaydet'i**seçin. Portal, kaydetme tamamlandığında sizi size iletir.

> [!Note]
> Bu öğretici için yükleme bildirimini açmıyoruz, ancak dosya yükleme [bildiriminin](../iot-hub/iot-hub-java-java-file-upload.md#receive-a-file-upload-notification) nasıl işleyeceğiniz hakkında ayrıntılar için dosya yükleme bildirimi alın'a bakın.

## <a name="build-publish-and-deploy-modules"></a>Modüller oluşturma, yayımlama ve dağıtma

Yapılandırma değişikliklerini yaptığımıza göre, görüntüleri oluşturmaya ve bunları Azure konteyner kayıt defterimize yayınlamaya hazırız. Yapı işlemi, hangi modüllerin oluşturulması gerektiğini belirlemek için deployment.template.json dosyasını kullanır. Sürüm de dahil olmak üzere her modülün ayarları modül klasöründeki module.json dosyasında bulunur. Yapı işlemi ilk olarak bir görüntü oluşturmak için module.json dosyasında bulunan geçerli yapılandırmayla eşleşen Dockerfiles'da bir Docker yapısı çalıştırın. Daha sonra modülü.json dosyasından, module.json dosyasındakiyle eşleşen bir sürüm etiketiyle görüntüyü kayıt defterine yayınlar. Son olarak, IoT Edge aygıtına dağıtılayabileceğimiz yapılandırmaya özgü bir dağıtım bildirimi (örneğin, deployment.amd64.json) üretir. IoT Edge aygıtı dağıtım bildirimindeki bilgileri okur ve yönergelere göre modülleri indirir, rotaları yapılandırır ve istenen özellikleri ayarlar. Bu dağıtım yönteminin farkında olmanızı gereken iki yan etkisi vardır:

* **Dağıtım gecikmesi:** IoT Edge çalışma süresi yeniden yapılandırmaya başlamadan önce istenen özelliklerdeki değişikliği tanıması gerektiğinden, modüllerinizi dağıtmadan sonra, çalışma zamanı onları alıp IoT Edge aygıtını güncelleştirmeye başlayana kadar bir miktar zaman alabilir.

* **Modül sürümleri önemlidir:** Bir modülün kapsayıcısının yeni bir sürümünü önceki modülle aynı sürüm etiketlerini kullanarak konteyner kayıt defterinize yayınlarsanız, çalışma zamanı modülün yeni sürümünü indirmez. Yerel görüntünün sürüm etiketinin ve dağıtım bildiriminden istenen görüntünün karşılaştırılmasını yapar. Bu sürümler eşleşirse, çalışma zamanı hiçbir eylemde bulunmaz. Bu nedenle, yeni değişiklikler dağıtmak istediğiniz her seferinde modülünüzün sürümünü aşamalı olarak yapmak önemlidir. Değiştirdiğiniz modül için module.json dosyasındaki **etiket** özelliğinin altındaki **sürüm** özelliğini değiştirerek sürümü artım. Sonra modülü oluşturun ve yayınlayın.

    ```json
    {
      "$schema-version": "0.0.1",
      "description": "",
      "image": {
        "repository": "<your registry>.azurecr.io/avrofilewriter",
        "tag": {
          "version": "0.0.1",
          "platforms": {
            "amd64": "./Dockerfile.amd64",
            "amd64.debug": "./Dockerfile.amd64.debug",
            "arm32v7": "./Dockerfile.arm32v7"
          }
        },
        "buildOptions": []
      },
      "language": "python"
    }
    ```

### <a name="build-and-publish"></a>Oluşturma ve yayımlama

1. Geliştirme VM'nizdeki Visual Studio Code'da bir Visual Studio Code terminal penceresi açın ve konteyner kayıt defterinize giriş yapın.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

1. Visual Studio Code'da deployment.template.json'a sağ tıklayın ve **IoT Edge Solution'ı Oluştur ve İtimi'ni**seçin.

### <a name="view-modules-in-the-registry"></a>Kayıt defterindeki modülleri görüntüleme

Yapı başarıyla tamamlandığında, yayınlanan modüllerimizi incelemek için Azure portalını kullanabileceğiz.

1. Azure portalında Azure Machine Learning çalışma alanınıza gidin ve **Kayıt Defteri**için köprüye tıklayın.

    ![Makine öğrenimi hizmeti çalışma alanından kayıt defterine gidin](media/tutorial-machine-learning-edge-06-custom-modules/follow-registry-link.png)

2. Kayıt defteri tarafındaki **gezginden, Depolar'ı**seçin.

3. Oluşturduğunuz her iki modülün, **avrofilewriter** ve **turbofanrouter'ın**de depo olarak göründüğünü unutmayın.

4. **Turbofanrouter'ı** seçin ve 0.0.1-amd64 etiketli bir resim yayınladığınızı unutmayın.

   ![Turbofanrouter'ın ilk etiketli sürümünü görüntüle](media/tutorial-machine-learning-edge-06-custom-modules/tagged-image-turbofanrouter-repo.png)

### <a name="deploy-modules-to-iot-edge-device"></a>Modülleri IoT Edge cihazına dağıtma

Çözümümüzdeki modülleri inşa ettik ve yapılandırdık, şimdi modülleri IoT Edge cihazına dağıtacağız.

1. Visual Studio Code'da config klasöründeki **deployment.amd64.json** dosyasına sağ tıklayın.

2. **Tek Aygıt için Dağıtım Oluştur'u**seçin.

3. IoT Edge cihazınızı seçin, **aaTurboFanEdgeDevice.**

4. Visual Studio Code explorer'da Azure IoT Hub aygıtları panelini yenileyin. Üç yeni modülün dağıtılmış olduğunu ancak henüz çalışmadığını görmeniz gerekir.

5. Birkaç dakika sonra tekrar yenileyin ve modüllerin çalıştığını göreceksiniz.

   ![Çalışma modüllerini Visual Studio Code'da görüntüleme](media/tutorial-machine-learning-edge-06-custom-modules/view-running-modules-list.png)

> [!NOTE]
> Modüllerin sürekli çalışma durumuna başlaması ve yerleşmesi birkaç dakika sürebilir. Bu süre zarfında, modüllerin IoT Edge hub modülüyle bağlantı kurmaya çalışırken başlayıp durduğunu görebilirsiniz.

## <a name="diagnosing-failures"></a>Hataları tanılama

Bu bölümde, bir modül veya modülile neyin yanlış gittiğini anlamak için birkaç teknik paylaşıyoruz. Genellikle bir hata ilk Visual Studio Code durumundan tespit edilebilir.

### <a name="identify-failed-modules"></a>Başarısız modülleri tanımlama

* **Görsel Stüdyo Kodu:** Azure IoT Hub aygıtları paneline bakın. Çoğu modül çalışan bir durumda ysa, ancak biri durdurulursa, durdurulan modülü daha da araştırmanız gerekir. Tüm modüller uzun bir süre için durdurulmuş durumda ysa, bu da başarısızlığa işaret edebilir.

* **Azure portalı:** Portaldaki IoT hub'ınıza gidip cihaz ayrıntıları sayfasını (IoT Edge altında, cihazınızı delerek) bularak, bir modülün bir hata bildirdiğini veya IoT hub'ına hiçbir şey bildirmediğini görebilirsiniz.

### <a name="diagnosing-from-the-device"></a>Cihazdan tanılama

IoT Edge cihazına giriş yaparak, modüllerinizin durumu hakkında çok sayıda bilgiye erişebilirsiniz. Kullandığımız ana mekanizma, cihazdaki kapları ve görüntüleri incelememize izin veren Docker komutlarıdır.

1. Çalışan tüm kapsayıcıları listele. Her modül için modüle karşılık gelen bir ad içeren bir kapsayıcı görmeyi bekliyoruz. Ayrıca, bu komut, beklentinizle eşleştirebilmeniz için sürüm de dahil olmak üzere kapsayıcının tam görüntüsünü listeler. Ayrıca komut "kapsayıcı" yerine "görüntü" yerine görüntüleri listeleyebilirsiniz.

   ```bash
   sudo docker container ls
   ```

2. Bir konteynerin günlüklerini alın. Bu komut, konteynırdaki StdErr ve StdOut'a yazılan her şeyi çıkarır. Bu komut, nedense başlatılan ve sonra ölen kapsayıcılar için çalışır. EdgeAgent veya edgeHub kapsayıcıları ile neler olduğunu anlamak için de yararlıdır.

   ```bash
   sudo docker container logs <container name>
   ```

3. Bir konteynırı inceleyin. Bu komut görüntü hakkında bir ton bilgi verir. Veriler, aradığınıza bağlı olarak filtrelenebilir. Örnek olarak, avroFileWriter üzerindeki bağlamaların doğru olup olmadığını görmek istiyorsanız aşağıdaki komutu kullanabilirsiniz:

   ```bash
   sudo docker container inspect -f "{{ json .Mounts }}" avroFileWriter | python -m json.tool
   ```

4. Çalışan bir kapsayıcıya bağlanın. Bu komut, çalışırken kapsayıcı incelemek istiyorsanız yararlı olabilir:

   ```bash
   sudo docker exec -it avroFileWriter bash
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Visual Studio Code'da üç modül, bir sınıflandırıcı, yönlendirici ve bir dosya yazarı/yükleyici içeren bir IoT Edge Çözümü oluşturduk. Modüllerin kenar aygıtında birbirleriyle iletişim kurmasını sağlayacak rotaları ayarladık, kenar aygıtının yapılandırmasını değiştirdik ve Dockerfiles'ı bağımlılıkları yüklemek ve modüllerin kapsayıcılarına bağlama yuvaları eklemek için güncelledik. Ardından, iletilerimizi türüne göre yönlendirmek ve dosya yüklemelerini işlemek için IoT Hub'ının yapılandırmasını güncelledik. Her şey yerli yerindeyken, modülleri IoT Edge cihazına yerleştirdik ve modüllerin doğru çalışmasını sağladık.

Daha fazla bilgiyi aşağıdaki sayfalarda bulabilirsiniz:

* [IoT Edge'de modülleri dağıtmayı ve yolları oluşturmayı öğrenin](module-composition.md)
* [IoT Hub ileti yönlendirme sorgusu söz dizimi](../iot-hub/iot-hub-devguide-routing-query-syntax.md)
* [IoT Hub ileti yönlendirmesi: şimdi ileti gövdesinde yönlendirme ile](https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/)
* [IoT Hub ile dosyaları karşıya yükleme](../iot-hub/iot-hub-devguide-file-upload.md)
* [IoT Hub ile cihazınızdan buluta dosya yükleme](../iot-hub/iot-hub-python-python-file-upload.md)

Veri göndermeye başlamak ve çözümünüzü iş başında görmek için bir sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Saydam ağ geçidi üzerinden veri gönderme](tutorial-machine-learning-edge-07-send-data-to-hub.md)
