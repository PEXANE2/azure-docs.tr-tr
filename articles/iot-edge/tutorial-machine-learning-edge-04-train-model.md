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
ms.openlocfilehash: cfb778a1a632dc17a9f50c7ea05debed0edb4fb6
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88660256"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Öğretici: Azure Machine Learning modeli eğitme ve dağıtma

> [!NOTE]
> Bu makale, IoT Edge Azure Machine Learning kullanımı hakkında öğretici için bir serinin bir parçasıdır. Bu makaleye doğrudan ulaşdıysanız, en iyi sonuçlar için serideki [ilk makaleyle](tutorial-machine-learning-edge-01-intro.md) başlamanızı öneririz.

Bu makalede, aşağıdaki görevleri yaptık:

* Machine Learning modelini eğitmek için Azure Notebooks kullanın.
* Eğitilen modeli kapsayıcı görüntüsü olarak paketleyin.
* Kapsayıcı görüntüsünü Azure IoT Edge modülü olarak dağıtın.

Azure Notebooks, makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullanılan temel bir blok olan Azure Machine Learning çalışma alanından faydalanır.

Bu makaledeki adımlar genellikle veri bilimcileri tarafından gerçekleştirilmiş olabilir.

## <a name="set-up-azure-notebooks"></a>Azure Notebooks ayarlama

İki jupi Not defterini ve destekleyici dosyaları barındırmak için Azure Notebooks kullanırız. Burada bir Azure Notebooks projesi oluşturacağız ve yapılandıracağız. Jupyter ve/veya Azure Notebooks kullanmadıysanız, bir dizi giriş belgesi aşağıda verilmiştir:

* **Hızlı başlangıç:** [Not defteri oluşturma ve paylaşma](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Öğretici:** [Python Ile Jupyter Not defteri oluşturma ve çalıştırma](../notebooks/tutorial-create-run-jupyter-notebook.md)

Azure Notebooks kullanmak, alıştırma için tutarlı bir ortam sağlar.

> [!NOTE]
> Ayarladıktan sonra, Azure Notebooks hizmetine herhangi bir makineden erişilebilir. Kurulum sırasında, ihtiyacınız olacak tüm dosyaları içeren geliştirme sanal makinesini kullanmanız gerekir.

### <a name="create-an-azure-notebooks-account"></a>Azure Notebooks hesabı oluşturma

Azure Notebooks kullanmak için bir hesap oluşturmanız gerekir. Azure Not defteri hesapları Azure aboneliklerinden bağımsızdır.

1. [Azure Notebooks](https://notebooks.azure.com)gidin.

1. Sayfanın sağ üst köşesinde **bulunan oturum aç** ' a tıklayın.

1. İş veya okul hesabınızla (Azure Active Directory) ya da kişisel hesabınızla (Microsoft hesabı) oturum açın.

1. Daha önce Azure Notebooks kullanmadıysanız, Azure Notebooks uygulamasına erişim vermeniz istenir.

1. Azure Notebooks için bir kullanıcı KIMLIĞI oluşturun.

### <a name="upload-jupyter-notebook-files"></a>Jupyter Not defteri dosyalarını karşıya yükle

Örnek Not defteri dosyalarını yeni bir Azure Notebooks projesine yükleyeceğiz.

1. Yeni hesabınızın kullanıcı sayfasında, üst menü çubuğundan **Projelerim** ' ı seçin.

1. Düğmeyi seçerek yeni bir proje ekleyin **+** .

1. **Yeni proje oluştur** iletişim kutusunda bir **Proje adı**girin. 

1. Projenin public olması veya bir Benioku dosyası olması gerektiği için **Public** ve **README** işaretini kaldırın.

1. **Oluştur**’u seçin.

1. **Karşıya yükle** ' yi (yukarı ok simgesi) seçin ve **bilgisayardan**öğesini seçin.

1. **Dosya Seç**' i seçin.

1. **C:\source\ıotedgeandmlsample\azurenotebooks**dizinine gidin. Listedeki tüm dosyaları seçin ve **Aç**' a tıklayın.

1. **Bu dosyaların içeriğine güvenim** kutusunu işaretleyin.

1. Karşıya yüklemeye başlamak için **karşıya yükle** ' yi seçin ve sonra işlem tamamlandıktan sonra **bitti** ' yi seçin.

### <a name="azure-notebook-files"></a>Azure Not defteri dosyaları

Azure Notebooks projenize yüklediğiniz dosyaları gözden geçirelim. Öğreticinin bu bölümündeki etkinlikler, birkaç destekleyici dosya kullanan iki Not Defteri dosyasında yayılamaz.

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

## <a name="run-azure-notebooks"></a>Azure Notebooks Çalıştır

Artık proje oluşturulduğuna göre, not defterlerini çalıştırabilirsiniz. 

1. Proje sayfanıza **01-turbofan \_ regresyon. ipynb**öğesini seçin.

    ![Çalıştırılacak ilk not defterini seçin](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

1. Not defteri **güvenilir değil**olarak listeleniyorsa, Not defterinin sağ üst köşesindeki **güvenilir değil** pencere öğesine tıklayın. İletişim kutusu geldiğinde **güven**' i seçin.

1. En iyi sonuçlar için, her bir hücrenin belgelerini okuyun ve tek tek çalıştırın. Araç çubuğunda **Çalıştır** ' ı seçin. Daha sonra, daha sonra birden çok hücre çalıştırmak için BT 'yi görürsünüz. Yükseltme ve kullanımdan kaldırma uyarılarını yoksayabilirsiniz.

    Bir hücre çalışırken, köşeli ayraçlar ([]) arasında bir yıldız işareti görüntüler \* . Hücrenin işlemi tamamlandığında, yıldız işareti bir sayıyla değişir ve ilgili çıktı görünebilir. Bir not defterindeki hücreler sırayla oluşturulur ve tek seferde yalnızca bir adet çalıştırılabilir.

    Ayrıca **hücre menüsündeki Çalıştır seçeneklerini,** `Ctrl`  +  `Enter` bir hücreyi çalıştırmak ve `Shift`  +  `Enter` bir hücreyi çalıştırmak ve bir sonraki hücreye ilerlemek için kullanabilirsiniz.

    > [!TIP]
    > Tutarlı hücre işlemleri için tarayıcınızda birden çok sekmeden aynı not defterini çalıştırmaktan kaçının.

1. **Set global Properties** yönergelerini izleyen hücrede, Azure aboneliğinizin, ayarlarınızın ve kaynaklarınızın değerlerini yazın. Sonra hücreyi çalıştırın.

    ![Not defterindeki genel özellikleri ayarlama](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. **Çalışma alanı ayrıntılarının**önceki hücresinde, çalıştırıldıktan sonra kimlik doğrulaması için oturum açmanızı sağlayan bağlantıyı arayın:

    ![Cihaz kimlik doğrulaması için oturum açma istemi](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    Bağlantıyı açın ve belirtilen kodu girin. Bu oturum açma yordamı, Microsoft Azure platformlar arası komut satırı arabirimini kullanarak Azure kaynaklarına erişmek için Jupyter Not defterinin kimliğini doğrular.  

    ![Cihazda uygulamanın kimliğini doğrulama onayı](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. **Sonuçları keşfetmeye**önce gelen hücrede, çalışma kimliği ' nden değeri kopyalayın ve **bir çalıştırma edilmeyen**izleyen hücrede çalıştırma kimliği için yapıştırın.

   ![Çalışma KIMLIĞINI hücreler arasında kopyalama](media/tutorial-machine-learning-edge-04-train-model/automl-id.png)

1. Not defterinde kalan hücreleri çalıştırın.

1. Not defterini kaydedin ve proje sayfanıza dönün.

1. **02-turbofan \_ Deploy \_ model. ipynb** 'yi açın ve her hücreyi çalıştırın. **Çalışma alanını Yapılandır**' ın bulunduğu hücrede kimlik doğrulaması yapmak için oturum açmanız gerekir.

1. Not defterini kaydedin ve proje sayfanıza dönün.

### <a name="verify-success"></a>Başarıyı doğrula

Not defterlerinin başarıyla tamamlandığını doğrulamak için, birkaç öğenin oluşturulduğunu doğrulayın.

1. Azure Notebooks projesi sayfanızda, bir noktayla başlayan öğe adlarının görünmesi için **gizli öğeleri göster** ' i seçin.

1. Aşağıdaki dosyaların oluşturulduğunu doğrulayın:

    | Dosya | Açıklama |
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

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, geriye kalan bir kullanım ömrü (RUL) sınıflandırıcısını eğitmek, bir kapsayıcı görüntüsü oluşturmak ve görüntüyü bir Web hizmeti olarak dağıtmak ve test etmek için turbofa cihazlarındaki verileri kullanmak üzere Azure Notebooks çalıştıran iki JUPITER Not defteri kullandık.

IoT Edge bir cihaz oluşturmak için sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [IoT Edge cihazını yapılandırma](tutorial-machine-learning-edge-05-configure-edge-device.md)
