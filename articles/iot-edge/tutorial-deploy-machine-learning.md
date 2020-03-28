---
title: Öğretici - Azure IoT Edge kullanarak bir cihaza Azure Machine Learning dağıtma
description: Bu öğreticide, bir Azure Machine Learning modeli oluşturun ve modül olarak bir kenar aygıtına dağıtın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5bfbf4a432f720b683ded4c85530135d86b24eba
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76772996"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Öğretici: Azure Machine Learning'i bir IoT Edge modülü olarak dağıtma (önizleme)

Bir makine öğrenimi modülü geliştirmek ve Azure IoT Edge çalıştıran bir Linux aygıtına dağıtmak için Azure Not Defterleri'ni kullanın.
İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide simülasyon makinesi sıcaklık verilerini temel alarak bir cihazın arızalanacağı zamanı tahmin eden bir Azure Machine Learning modülünü dağıtma adımları açıklanmaktadır. IoT Edge'de Azure Machine Learning hakkında daha fazla bilgi için [Azure Machine Learning belgelerine](../machine-learning/how-to-deploy-and-where.md)bakın.

>[!NOTE]
>Azure IoT Edge üzerindeki Azure Machine Learning modülleri genel önizleme sürümündedir.

Bu öğreticide oluşturduğunuz Azure Machine Learning modülü, cihazınızın ürettiği ortam verilerini okur ve iletileri normal veya anormal olarak etiketler.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Bir Azure Machine Learning modülü oluşturma
> * Azure kapsayıcı kayıt defterine bir modülü kapsayıcısı gönderme
> * IoT Edge cihazınıza bir Azure Machine Learning modülü dağıtma
> * Oluşturulan verileri görüntüleme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md)için hızlı başlatma adımlarını izleyerek Bir Azure sanal makinesini IoT Edge aygıtı olarak kullanabilirsiniz.
* Azure Machine Learning modülü Windows kapsayıcılarını desteklemez.
* Azure Machine Learning modülü ARM işlemcilerini desteklemez.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* Azure Machine Learning çalışma alanı. Azure Machine [Learning'e başlamak için Azure portalını kullanma](../machine-learning/tutorial-1st-experiment-sdk-setup.md) ve nasıl kullanılacağını öğrenmek için azure portalını kullanma yönergelerini izleyin.
  * Çalışma alanı adını, kaynak grubunu ve abonelik kimliğini not edin. Bu değerlerin tümü, Azure portalındaki çalışma alanına genel bakışta mevcuttur. Bir Azure not defterini çalışma alanı kaynaklarınıza bağlamak için bu değerleri daha sonra öğreticide kullanırsınız.

## <a name="create-and-deploy-azure-machine-learning-module"></a>Azure Machine Learning modülü oluşturma ve dağıtma

Bu bölümde, eğitimli makine öğrenimi model dosyalarını azure machine learning kapsayıcısına dönüştürürsunuz. Docker görüntüsü için gerekli tüm bileşenler [Azure IoT Edge için AI Toolkit deposunda](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial) mevcuttur. Kapsayıcıyı oluşturmak ve Azure Kapsayıcı Kayıt Defteri'ne itmek için bu depoyu Microsoft Azure Not Defterleri'ne yüklemek için aşağıdaki adımları izleyin.

1. Azure Not Defterleri projelerinize gidin. Azure [portalındaki](https://portal.azure.com) Azure Machine Learning çalışma alanınızdan veya Azure hesabınızla [Microsoft Azure Not Defterleri'nde](https://notebooks.azure.com/home/projects) oturum açarak bu alana ulaşabilirsiniz.

2. **Yükle GitHub Repo'yu**seçin.

3. Aşağıdaki GitHub depo adını sağlayın: `Azure/ai-toolkit-iot-edge`. Projenizi gizli tutmak istiyorsanız **Ortak** kutusunun işaretlerini kaldırın. **İçeri aktar**'ı seçin.

4. Alma işlemi tamamlandıktan sonra, yeni **ai-toolkit-iot-edge** projesine gidin ve **IoT Edge anomali algılama öğretici** klasörünü açın.

5. Projenizin çalıştığını doğrulayın. Değilse, **Serbest İşlem'de Çalıştır'ı**seçin.

   ![Ücretsiz işlemde çalıştırın](./media/tutorial-deploy-machine-learning/run-on-free-compute.png)

6. **aml_config/config.json** dosyasını açın.

7. Config dosyasını Azure abonelik kimliğinizin değerlerini, aboneliğinizdeki kaynak grubunu ve Azure Machine Learning çalışma alanı adınızı içerecek şekilde edin. Tüm bu değerleri Azure'daki çalışma alanınızın **Genel Bakış** bölümünden alabilirsiniz.

8. Config dosyasını kaydedin.

9. **00-anomali-detection-tutorial.ipynb** dosyasını açın.

10. İstendiğinde Python **3.6** çekirdeğini seçin ve **ardından Çekirdek'i Ayarla'yı**seçin.

11. Not defterindeki ilk hücreyi yorumlardaki talimatlara göre edin. Config dosyasına eklediğiniz aynı kaynak grubunu, abonelik kimliğini ve çalışma alanı adını kullanın.

12. Hücreleri not defterine seçerek ve **Çalıştır'ı** `Shift + Enter`seçerek veya basarak çalıştırın.

    >[!TIP]
    >Anomali algılama öğretici not defterindeki bazı hücreler isteğe bağlıdır, çünkü bazı kullanıcıların ioT Hub'ı gibi henüz sahip olabileceği veya sahip olmadığı kaynakları oluştururlar. Varolan kaynak bilgilerinizi ilk hücreye koyarsanız, Azure yinelenen kaynaklar oluşturmayamayacağı için yeni kaynaklar oluşturan hücreleri çalıştırırsanız hatalar alırsınız. Bu sorun değil ve hataları yoksayabilir veya bu isteğe bağlı bölümleri tamamen atlayabilirsiniz.

Not defterindeki tüm adımları tamamlayarak, bir anomali algılama modeli eğittiniz, Docker kapsayıcı görüntüsü olarak oluşturup bu görüntüyü Azure Konteyner Kayıt Defteri'ne ittiniz. Daha sonra modeli test ettin ve sonunda IoT Edge aygıtınıza dağıttınız.

## <a name="view-container-repository"></a>Kapsayıcı deposunu görüntüle

Kapsayıcı görüntünüzün makine öğrenim ortamınızla ilişkili Azure konteyner kayıt defterinde başarıyla oluşturulduğunu ve depolandığını kontrol edin. Önceki bölümde kullandığınız not defteri, kapsayıcı görüntüsünü ve kayıt defteri kimlik bilgilerini otomatik olarak IoT Edge aygıtınıza sağlar, ancak bilgileri daha sonra kendiniz bulabilmeniz için nerede depolandığını bilmeniz gerekir.

1. Azure [portalında,](https://portal.azure.com)Machine Learning hizmeti çalışma alanınıza gidin.

2. **Genel Bakış** bölümü, çalışma alanı ayrıntılarının yanı sıra ilişkili kaynaklarını da listeler. Rasgele sayılar tarafından izlenen çalışma alanı adınız olması gereken **Kayıt Defteri** değerini seçin.

3. Konteyner kayıt **defterinde, Depolar'ı**seçin. Önceki bölümde çalıştırdığınız not defteri tarafından oluşturulan **tempanomalydetection** adlı bir depo görmeniz gerekir.

4. **Tempanomalydetection**seçin. Deponun bir etiketi olduğunu görmelisiniz: **1**.

   Artık kayıt defteri adını, depo adını ve etiketini bildiğinize göre, kapsayıcının tam görüntü yolunu biliyorsunuz. Görüntü yolları ** \<registry_name\>.azurecr.io/tempanomalydetection:1**gibi görünüyor. Bu görüntü yolunu kullanarak bu kapsayıcıyı IoT Edge cihazlarına dağıtabilirsiniz.

5. Kapsayıcı kayıt defterinde **Erişim tuşlarını**seçin. Bir yönetici kullanıcısı için **Giriş sunucusu** ve Kullanıcı **Adı**ve **Parola** gibi bir dizi erişim kimlik bilgilerini görmeniz gerekir.

   Bu kimlik bilgilerini dağıtım bildirimine dahil ederek IoT Edge cihazınızın kayıt defterindeki kapsayıcı görüntülerini çekmesini sağlayabilirsiniz.

Artık Machine Learning konteyner görüntüsünün nerede depolandığınızı biliyorsunuz. Bir sonraki bölüm, IoT Edge aygıtınızda modül olarak çalışan kapsayıcıyı görüntülemek için adımlardan geçer.

## <a name="view-the-generated-data"></a>Oluşturulan verileri görüntüleme

Her bir IoT Edge modülü tarafından oluşturulan ve IoT hub'ınıza gönderilen iletileri görüntüleyebilirsiniz.

### <a name="view-data-on-your-iot-edge-device"></a>IoT Edge cihazınızdaki verileri görüntüleme

IoT Edge cihazınızda her bir modülden gönderilen iletileri görüntüleyebilirsiniz.

Komutları çalıştırmak `iotedge` `sudo` için yüksek izinler için kullanmanız gerekebilir. Oturumunuzu oturuma alma ve cihazınıza yeniden oturum açma izninizi otomatik olarak günceller.

1. IoT Edge cihazınızda tüm modülleri görüntüleyin.

   ```cmd/sh
   iotedge list
   ```

2. Belirli bir cihazdan gönderilen iletileri görüntüleyin. Önceki komutun çıktısında yer alan modül adını kullanın.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>IoT hub'ınıza ulaşan verileri görüntüleme

[Görsel Stüdyo Kodu için Azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak IoT hub'ınızın aldığı aygıttan buluta iletileri görüntüleyebilirsiniz.

Aşağıdaki adımlar, IoT hub'ınıza ulaşan cihazdan buluta iletileri izlemek için yapmanız gereken Visual Studio Code ayarlarını göstermektedir.

1. Visual Studio Code'da **IoT Hub Cihazları**'nı seçin.

2. Menüden **...** öğesini, sonra **IoT Hub Bağlantı Dizesini Ayarla**'yı seçin.

   ![IoT Hub Bağlantı Dizelerini Ayarla](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Sayfanın üstünde açılan metin kutusuna IoT Hub'ınız için iothubowner bağlantı dizesini girin. IoT Edge cihazınız IoT Hub Cihazları listesinde görünmelidir.

4. Seçin **...** tekrar sonra **Başlat İzleme Yerleşik Olay Bitiş Noktası**seçin.

5. tempSensor her beş saniyede bir gelen iletileri gözlemleyin. İleti gövdesi, machinelearningmodule'in doğru veya yanlış bir değer sağladığı **anomali**adı verilen bir özellik içerir. Model başarıyla çalıştıysa, **AzureMLResponse** özelliği "OK" değerini içerir.

   ![İleti gövdesinde Azure Machine Learning yanıtı](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning ile çalışan bir IoT Edge modülü dağıttınız. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için diğer öğreticilere geçebilirsiniz.

> [!div class="nextstepaction"]
> [Özel Görüntü İşleme hizmetiyle görüntüleri sınıflandırma](tutorial-deploy-custom-vision.md)
