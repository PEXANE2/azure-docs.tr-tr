---
title: 'Öğretici: Azure IoT Edge bir model Machine Learning eğitme ve dağıtma'
description: Bu öğreticide, Azure Machine Learning kullanarak bir makine öğrenimi modeli eğitecaksınız ve sonra modeli bir Azure IoT Edge modülü olarak dağıtılabilecek bir kapsayıcı görüntüsü olarak paketleyebilirsiniz.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/24/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2cc96db88d9a2aec02de5e2fc4ed18b445972e7b
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121165"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Öğretici: Azure Machine Learning modeli eğitme ve dağıtma

Bu makalede, aşağıdaki görevleri yaptık:

* Machine Learning modelini eğitmek için Azure Machine Learning Studio kullanın.
* Eğitilen modeli kapsayıcı görüntüsü olarak paketleyin.
* Kapsayıcı görüntüsünü Azure IoT Edge modülü olarak dağıtın.

Azure Machine Learning Studio, makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullanılan temel bir bloğudur.

Bu makaledeki adımlar genellikle veri bilimcileri tarafından gerçekleştirilmiş olabilir.

Öğreticinin bu bölümünde şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Makine öğrenimi modelini eğitmek için Azure Machine Learning Çalışma Alanı bir jupi Not defteri oluşturun.
> * Eğitilen makine öğrenme modelini kapsayıcıya geçirin.
> * Kapsayıcılı makine öğrenimi modelinden bir Azure IoT Edge modülü oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu makale, IoT Edge Azure Machine Learning kullanımı hakkında öğretici için bir serinin bir parçasıdır. Serideki her makale, önceki makaledeki iş üzerinde oluşturulur. Bu makaleye doğrudan ulaşdıysanız, serideki [ilk makaleyi](tutorial-machine-learning-edge-01-intro.md) ziyaret edin.

## <a name="set-up-azure-machine-learning"></a>Azure Machine Learning ayarlama 

İki jupi Not defterini ve destekleyici dosyaları barındırmak için Azure Machine Learning Studio kullanırız. Burada bir Azure Machine Learning projesi oluşturacağız ve yapılandıracağız. Jupyter ve/veya Azure Machine Learning Studio kullanmadıysanız, bir dizi giriş belgesi aşağıda verilmiştir:

* **Jupyter Not defterleri:** [Visual Studio Code 'Da Jupyter Not defterleri ile çalışma](https://code.visualstudio.com/docs/python/jupyter-support)
* **Azure Machine Learning:** [Jupyter not defterlerinde Azure Machine Learning kullanmaya başlama](../machine-learning/tutorial-1st-experiment-sdk-setup.md)


> [!NOTE]
> Ayarladıktan sonra, Azure Machine Learning hizmetine herhangi bir makineden erişilebilir. Kurulum sırasında, ihtiyacınız olacak tüm dosyaları içeren geliştirme sanal makinesini kullanmanız gerekir.

### <a name="install-azure-machine-learning-visual-studio-code-extension"></a>Azure Machine Learning Visual Studio Code uzantısını yükler
Geliştirme sanal makinesinde VS Code Bu uzantının yüklü olması gerekir. Farklı bir örnek üzerinde çalıştırıyorsanız, lütfen uzantıyı burada açıklandığı gibi yeniden yükleyin [.](../machine-learning/tutorial-setup-vscode-extension.md)

### <a name="create-an-azure-machine-learning-account"></a>Azure Machine Learning hesabı oluşturma  
Azure 'da kaynak sağlamak ve iş yüklerini çalıştırmak için Azure hesabı kimlik bilgilerinizle oturum açmanız gerekir.

1. Visual Studio Code menüsünde, menü çubuğundan komut paleti **görüntüle**' yi seçerek komut paleti ' ni açın  >   . 

1. `Azure: Sign In`Oturum açma işlemini başlatmak için komut paletine komutu girin. Oturum açmayı tamamlamaya yönelik yönergeleri izleyin. 

1. İş yükünüzü çalıştırmak için bir Azure ML İşlem örneği oluşturun. Komut paleti kullanma komutunu girin `Azure ML: Create Compute` . 
1. Azure Aboneliğinizi seçin
1. **+ Yeni Azure ML çalışma alanı oluştur** ' u seçin ve ad girin `turbofandemo` .
1. Bu demo için kullandığınız kaynak grubunu seçin.
1. VS Code pencerenizin sağ alt köşesinde çalışma alanı oluşturma işleminin ilerlemesini görebilmeniz gerekir: **çalışma alanı oluşturuluyor: turobofandemo** (Bu, bir dakika veya iki sürebilir). 
1. Lütfen çalışma alanının başarıyla oluşturulmasını bekleyin. **Azure ML çalışma alanı turbofandemo 'nin oluşturulmasını** söylemelidir.


### <a name="upload-jupyter-notebook-files"></a>Jupyter Notebook dosyalarını karşıya yükle

Örnek Not defteri dosyalarını yeni bir Azure ML çalışma alanına yükleyeceğiz.

1. Ml.azure.com adresine gidin ve oturum açın.
1. Microsoft dizininizi, Azure aboneliğinizi ve yeni oluşturulan Azure ML çalışma alanını seçin.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-studio-workspace.png" alt-text="Azure ML çalışma alanınızı seçin." :::

1. Azure ML çalışma alanınızda oturum açtıktan sonra, sol taraftaki menüyü kullanarak **Not defterleri** bölümüne gidin.
1. **My Files** sekmesini seçin.

1. **Karşıya yükle** ' yi seçin (yukarı ok simgesi) 


1. **C:\source\ıotedgeandmlsample\azurenotebooks** dizinine gidin. Listedeki tüm dosyaları seçin ve **Aç**' a tıklayın.

1. **Bu dosyaların içeriğine güvenim** kutusunu işaretleyin.

1. Karşıya yüklemeye başlamak için **karşıya yükle** ' yi seçin ve sonra işlem tamamlandıktan sonra **bitti** ' yi seçin.

### <a name="jupyter-notebook-files"></a>Jupyter Notebook dosyaları

Azure ML çalışma alanınıza yüklediğiniz dosyaları gözden geçirelim. Öğreticinin bu bölümündeki etkinlikler, birkaç destekleyici dosya kullanan iki Not Defteri dosyasında yayılamaz.

* **01-turbofan \_ regresyon. ipynb:** bu not defteri, Machine Learning denemesi oluşturmak ve çalıştırmak için Machine Learning hizmeti çalışma alanını kullanır. Genel olarak, Not defteri aşağıdaki adımları yapar:

  1. Cihaz bandı tarafından oluşturulan Azure Storage hesabından verileri indirir.
  1. Verileri araştırır ve hazırlar, ardından sınıflandırıcı modelini eğitmek için verileri kullanır.
  1. Test veri kümesi (testFD003.txt) kullanarak deneyden modeli değerlendirin \_ .
  1. En iyi sınıflandırıcı modelini Machine Learning hizmeti çalışma alanına yayınlar.

* **02-turbofan \_ dağıtım \_ modeli. ipynb:** bu not defteri, önceki not defterinde oluşturulan modeli alır ve bir Azure IoT Edge cihazına dağıtılmaya yönelik bir kapsayıcı görüntüsü oluşturmak için kullanır. Not defteri aşağıdaki adımları gerçekleştirir:

  1. Model için bir Puanlama betiği oluşturur.
  1. Machine Learning hizmeti çalışma alanında kaydedilen sınıflandırıcı modelini kullanarak bir kapsayıcı görüntüsü üretir.
  1. Görüntüyü Azure Container Instance üzerinde bir Web hizmeti olarak dağıtır.
  1. , Modeli doğrulamak için Web hizmetini kullanır ve görüntü beklendiği gibi çalışır. Doğrulanan görüntü, Bu öğreticinin [özel IoT Edge modülleri oluştur ve dağıt](tutorial-machine-learning-edge-06-custom-modules.md) bölümünde IoT Edge cihazımız cihaza dağıtılır.

* **Test \_FD003.txt:** bu dosya, eğitilen sınıflandırıcımız doğrulanırken test kümesi olarak kullanacağız verileri içerir. Kendi basitliği için test kümesi olarak, özgün yarışma göre belirtilen test verilerini kullanmayı seçtik.

* **Rul \_FD003.txt:** bu dosya, testFD003.txt dosyasındaki her bir cihazın son döngüsü Için kalan yararlı yaşam süresini (rul) içerir \_ . \\ \\ \\ \\ Verilerin ayrıntılı bir açıklaması için C: kaynak iotedgeandmlsample verilerinde bulunan readme.txt ve hasar yayma Modeling.pdf dosyalarına bakın.

* **Utils.py:** Verilerle çalışmak için bir Python yardımcı program işlevleri kümesi içerir. İlk not defteri, işlevlerin ayrıntılı bir açıklamasını içerir.

* **README.MD:** Not defterlerinin kullanımını açıklayan Benioku dosyası.  

## <a name="run-jupyter-notebooks"></a>Jupyter Notebooks’u çalıştırma

Çalışma alanı oluşturuldığına göre, artık not defterlerini çalıştırabilirsiniz. 

1. **My Files** sayfasında, **01-turbofan \_ regresyon. ipynb**' yi seçin.

    :::image type="content" source="media/tutorial-machine-learning-edge-04-train-model/select-turbofan-notebook.png" alt-text="Çalıştırmak için ilk not defteri ' ni seçin. ":::

1. Not defteri **güvenilir değil** olarak listeleniyorsa, Not defterinin sağ üst köşesindeki **güvenilir değil** pencere öğesine tıklayın. İletişim kutusu geldiğinde **güven**' i seçin.

1. En iyi sonuçlar için, her bir hücrenin belgelerini okuyun ve tek tek çalıştırın. Araç çubuğunda **Çalıştır** ' ı seçin. Daha sonra, daha sonra birden çok hücre çalıştırmak için BT 'yi görürsünüz. Yükseltme ve kullanımdan kaldırma uyarılarını yoksayabilirsiniz.

    Bir hücre çalışırken, köşeli ayraçlar ([]) arasında bir yıldız işareti görüntüler \* . Hücrenin işlemi tamamlandığında, yıldız işareti bir sayıyla değişir ve ilgili çıktı görünebilir. Bir not defterindeki hücreler sırayla oluşturulur ve tek seferde yalnızca bir adet çalıştırılabilir.

    Ayrıca **hücre menüsündeki Çalıştır seçeneklerini,** `Ctrl`  +  `Enter` bir hücreyi çalıştırmak ve `Shift`  +  `Enter` bir hücreyi çalıştırmak ve bir sonraki hücreye ilerlemek için kullanabilirsiniz.

    > [!TIP]
    > Tutarlı hücre işlemleri için tarayıcınızda birden çok sekmeden aynı not defterini çalıştırmaktan kaçının.

1. **Set global Properties** yönergelerini izleyen hücrede, Azure aboneliğinizin, ayarlarınızın ve kaynaklarınızın değerlerini yazın. Sonra hücreyi çalıştırın.

    ![Not defterindeki genel özellikleri ayarlama](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. **Çalışma alanı ayrıntılarının** önceki hücresinde, çalıştırıldıktan sonra kimlik doğrulaması için oturum açmanızı sağlayan bağlantıyı arayın:

    ![Cihaz kimlik doğrulaması için oturum açma istemi](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    Bağlantıyı açın ve belirtilen kodu girin. Bu oturum açma yordamı, Microsoft Azure platformlar arası komut satırı arabirimini kullanarak Azure kaynaklarına erişmek için Jupyter Not defterinin kimliğini doğrular.  

    ![Cihazda uygulamanın kimliğini doğrulama onayı](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. **Sonuçları keşfetmeye** önce gelen hücrede, çalışma kimliği ' nden değeri kopyalayın ve **bir çalıştırma edilmeyen** izleyen hücrede çalıştırma kimliği için yapıştırın.

   ![Çalışma KIMLIĞINI hücreler arasında kopyalama](media/tutorial-machine-learning-edge-04-train-model/automl-id.png)

1. Not defterinde kalan hücreleri çalıştırın.

1. Not defterini kaydedin ve proje sayfanıza dönün.

1. **02-turbofan \_ Deploy \_ model. ipynb** 'yi açın ve her hücreyi çalıştırın. **Çalışma alanını Yapılandır**' ın bulunduğu hücrede kimlik doğrulaması yapmak için oturum açmanız gerekir.

1. Not defterini kaydedin ve proje sayfanıza dönün.

### <a name="verify-success"></a>Başarıyı doğrula

Not defterlerinin başarıyla tamamlandığını doğrulamak için, birkaç öğenin oluşturulduğunu doğrulayın.

1. Azure ML Not defterleri **My Files** sekmesinde **Yenile**' yi seçin.

1. Aşağıdaki dosyaların oluşturulduğunu doğrulayın:

    | Dosya | Description |
    | --- | --- |
    | ./aml_config/.azureml/config.js | Azure Machine Learning Çalışma Alanı oluşturmak için kullanılan yapılandırma dosyası. |
    | ./aml_config/model_config.jsaçık | Modeli Azure 'daki **Turbofandemo** Machine Learning çalışma alanına dağıtmak için gereken yapılandırma dosyası. |
    | MyENV. yıml| Dağıtılan Machine Learning modeli için bağımlılıklar hakkında bilgi sağlar.|

1. Aşağıdaki Azure kaynaklarının oluşturulduğunu doğrulayın. Bazı kaynak adlarına rastgele karakterler eklenir.

    | Azure kaynağı | Ad |
    | --- | --- |
    | Machine Learning çalışma alanı | turborfanDemo |
    | Container Registry | turbofandemoxxxxxxxx |
    | Uygulama öngörüleri | turbofanınsilerxxxxxxxx |
    | Key Vault | turbofankeyvaultbxxxxxxxx |
    | Depolama | turbofanstoragexxxxxxxxx |

### <a name="debugging"></a>Hata Ayıklama

Hata ayıklama için not defterine Python deyimleri ekleyebilirsiniz, örneğin, `print()` değerleri gösterme komutu. Tanımlı olmayan değişkenler veya nesneler görürseniz, ilk olarak bildirildiği veya örneklendiği hücreleri çalıştırın.

Not defterlerini yeniden yapmanız gerekiyorsa, önceden oluşturulmuş dosyaları ve Azure kaynaklarını silmeniz gerekebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici, her bir makalenin bir önceki bölümde gerçekleştirilen iş üzerinde oluşturulduğu bir küme parçasıdır. Lütfen son öğreticiyi tamamlayana kadar tüm kaynakları temizlemeyi bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, turbofa cihazlarındaki verileri kullanarak geriye kalan bir kullanım ömrü (RUL) sınıflandırıcısını eğitmek, bir kapsayıcı görüntüsü oluşturmak ve görüntüyü bir Web hizmeti olarak dağıtmak ve test etmek için Azure ML Studio 'da çalışan iki jupi Not defteri kullandık.

IoT Edge bir cihaz oluşturmak için sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [IoT Edge cihazını yapılandırma](tutorial-machine-learning-edge-05-configure-edge-device.md)