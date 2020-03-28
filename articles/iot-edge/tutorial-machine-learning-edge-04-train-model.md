---
title: "Öğretici: Bir modeli eğitin ve dağıtın - Azure IoT Edge'de Makine Öğrenimi"
description: Bu eğitimde, Azure Machine Learning'i kullanarak bir makine öğrenme modeli eğitecek ve ardından modeli Azure IoT Edge Modülü olarak dağıtılabilen bir kapsayıcı görüntüsü olarak paketleyeceksiniz.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/24/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 57630b789233dd23e61398f445b434e4ba08b48e
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80236007"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Öğretici: Azure Machine Learning modelini eğitin ve dağıtın

> [!NOTE]
> Bu makale, IoT Edge'de Azure Machine Learning'i kullanma yla ilgili bir öğretici için bir serinin parçasıdır. Bu makaleye doğrudan ulaştıysanız, en iyi sonuçlar için serinin [ilk makalesiile](tutorial-machine-learning-edge-01-intro.md) başlamanızı öneririz.

Bu makalede, aşağıdaki görevleri yapmak:

* Makine öğrenimi modelini eğitmek için Azure Dizüstü Bilgisayarları'nı kullanın.
* Eğitimli modeli konteyner görüntüsü olarak paketleyin.
* Kapsayıcı görüntüsünü Azure IoT Edge modülü olarak dağıtın.

Azure Dizüstü Bilgisayarlar, makine öğrenimi modellerini denemek, eğitmek ve dağıtmak için kullanılan temel bir blok olan Azure Machine Learning çalışma alanından yararlanır.

Bu makaledeki adımlar genellikle veri bilimciler tarafından gerçekleştirilebilir.

## <a name="set-up-azure-notebooks"></a>Azure Not Defterleri Ayarlama

Azure Not Defteri'ni iki Jupyter Notebook'u ve destekleyici dosyaları barındırmak için kullanırız. Burada bir Azure Notebook projesi oluşturuyor ve yapılandırıyoruz. Jupyter ve/veya Azure Not Defteri kullanmadıysanız, aşağıda birkaç giriş belgesi ve /veya Azure Not Defteri ve

* **Hızlı başlatma:** [Not defteri oluşturma ve paylaşma](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Öğretici:** [Python ile bir Jupyter dizüstü bilgisayar oluşturun ve çalıştırın](../notebooks/tutorial-create-run-jupyter-notebook.md)

Azure dizüstü bilgisayarların kullanılması, egzersiz için tutarlı bir ortam sağlar.

> [!NOTE]
> Kurulduktan sonra Azure Notebook hizmetine herhangi bir makineden erişilebilir. Kurulum sırasında, ihtiyacınız olacak tüm dosyalara sahip geliştirme VM'sini kullanmalısınız.

### <a name="create-an-azure-notebooks-account"></a>Azure Not Defterleri hesabı oluşturma

Azure Not Defterleri'ni kullanmak için bir hesap oluşturmanız gerekir. Azure Not Defteri hesapları Azure aboneliklerinden bağımsızdır.

1. Azure [not defterlerine](https://notebooks.azure.com)gidin.

1. Sayfanın sağ üst köşesinde **Oturum Aç'ı** tıklatın.

1. İş veya okul hesabınızla (Azure Etkin Rehberi) veya kişisel hesabınızla (Microsoft Hesabı) oturum açın.

1. Azure Not Defterlerini daha önce kullanmadıysanız, Azure Not Defterleri uygulamasına erişim izni vermeniz istenir.

1. Azure Dizüstü Bilgisayarlar için bir kullanıcı kimliği oluşturun.

### <a name="upload-jupyter-notebook-files"></a>Jupyter dizüstü bilgisayar dosyalarını yükleyin

Örnek not defteri dosyalarını yeni bir Azure Notebook projesine yükleyeceğiz.

1. Yeni hesabınızın kullanıcı sayfasından, üst menü çubuğundan **Projelerim'i** seçin.

1. Düğmeyi **+** seçerek yeni bir proje ekleyin.

1. Yeni **Proje Oluştur** iletişim kutusunda, **bir Proje Adı**sağlayın. 

1. Projenin herkese açık olmasına veya bir okuması olmasına gerek olmadığı için **Herkese Açık** ve **README'yi** işaretsiz bırakın.

1. **Oluştur'u**seçin.

1. **Yükle** 'yi (yukarı ok simgesi) seçin ve **Bilgisayardan'ı**seçin.

1. **Dosyaları seçin.**

1. **C:\source\IoTEdgeAndMlSample\AzureNotebooks'a**gidin. Listedeki tüm dosyaları seçin ve **Aç'ı**tıklatın.

1. Bu **dosyaların kutusuna güveniyorum I'yi** kontrol edin.

1. **Yüklemeye** başlamak için Yükle'yi seçin ve işlem tamamlandıktan sonra **Bitti'yi** seçin.

### <a name="azure-notebook-files"></a>Azure not defteri dosyaları

Azure Not Defterleri projenize yüklediğiniz dosyaları gözden geçirelim. Öğreticinin bu bölümündeki etkinlikler, birkaç destekleyici dosya kullanan iki not defteri dosyasına yayılır.

* **01-turbofan\_regresyon.ipynb:** Bu not defteri, bir makine öğrenme deneyi oluşturmak ve çalıştırmak için Machine Learning hizmet çalışma alanını kullanır. Genel olarak, not defteri aşağıdaki adımları yapar:

  1. Aygıt donanımı tarafından oluşturulan Azure Depolama hesabından veri indirir.
  1. Verileri inceler ve hazırlar, sonra sınıflandırıcı modelini eğitmek için verileri kullanır.
  1. Deneydeki modeli bir test veri kümesi\_(Test FD003.txt) kullanarak değerlendirin.
  1. Machine Learning hizmet çalışma alanına en iyi sınıflandırıcı modelini yayımlar.

* **02-turbofan\_\_dağıtım modeli.ipynb:** Bu not defteri, önceki not defterinde oluşturulan modeli alır ve bir Azure IoT Edge aygıtına dağıtılacak bir kapsayıcı görüntüsü oluşturmak için kullanır. Not defteri aşağıdaki adımları gerçekleştirir:

  1. Model için bir puanlama komut dosyası oluşturur.
  1. Machine Learning hizmet çalışma alanına kaydedilen sınıflandırıcı modelini kullanarak bir kapsayıcı görüntüsü oluşturur.
  1. Görüntüyü Azure Kapsayıcı Örneği'nde web hizmeti olarak dağıtıyor.
  1. Modeli ve görüntüyü beklendiği gibi doğrulamak için web hizmetini kullanır. Doğrulanmış görüntü, bu öğreticinin [Özel IoT Edge modülleri](tutorial-machine-learning-edge-06-custom-modules.md) bölümünde Ki'siIOT Edge aygıtımıza dağıtılacaktır.

* **Test\_FD003.txt:** Bu dosya, eğitimli sınıflandırıcımızı doğrularken test setimiz olarak kullanacağımız verileri içerir. Test verisini, orijinal yarışma için öngörüldüğü şekilde, test imizin basitliği için belirlediği miz olarak kullanmayı seçtik.

* **RUL\_FD003.txt:** Bu dosya, Test\_FD003.txt dosyasındaki her aygıtın son döngüsü için Kalan Yararlı Yaşam (RUL) içerir. Verilerin ayrıntılı bir açıklaması için readme.txt ve C:\\kaynak\\IoTEdgeAndMlSample\\\\veri Turbofan hasar yayma modelleme.pdf dosyaları bakın.

* **Utils.py:** Verilerle çalışmak için python yardımcı programı işlevleri kümesi içerir. İlk not defteri işlevlerin ayrıntılı bir açıklamasını içerir.

* **README.md:** Not defterlerinin kullanımını açıklayan okuma me.  

## <a name="run-azure-notebooks"></a>Azure Not Defterlerini Çalıştır

Proje oluşturulduğuna göre, not defterlerini çalıştırabilirsiniz. 

1. Proje sayfanızdan **01-turbofan\_regression.ipynb'yi**seçin.

    ![Çalıştırmak için ilk not defterini seçin](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

1. Not defteri **Güvenilmeyen**olarak listelenmişse, not defterinin sağ üst kısmındaki **Güvenilmeyen** widget'ı tıklatın. İletişim kutusu geldiğinde **Güven'i**seçin.

1. En iyi sonuçlar için, her hücre için belgeleri okuyun ve tek tek çalıştırın. Araç çubuğunda **Çalıştır'ı** seçin. Daha sonra, birden çok hücre çalıştırmak için uygun bulacaksınız. Yükseltme ve amortisman uyarılarını göz ardı edebilirsiniz.

    Bir hücre çalışırken, kare ayraçlar arasında bir yıldız\*işareti görüntüler ([ ]). Hücrenin çalışması tamamlandığında, yıldız işareti bir sayıyla değiştirilir ve ilgili çıktı görünebilir. Not defterindeki hücreler sırayla oluşur ve aynı anda yalnızca bir tane çalışıyor olabilir.

    Hücre  +  `Enter` çalıştırmak `Shift`  +  ve `Enter` bir **Cell** hücreyi `Ctrl` çalıştırıp bir sonraki hücreye ilerlemek için Hücre menüsünden çalıştırma seçeneklerini de kullanabilirsiniz.

    > [!TIP]
    > Tutarlı hücre işlemleri için, tarayıcınızdaki birden çok sekmeden aynı not defterini çalıştırmaktan kaçının.

1. **Genel özellikleri ayarla** yönergelerini izleyen hücrede, Azure aboneliğiniz, ayarlarınız ve kaynaklarınız için değerleri yazın. O zaman hücreyi çalıştır.

    ![Not defterinde genel özellikleri ayarlama](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. **Çalışma Alanı ayrıntılarından**önceki hücrede , çalıştırdıktan sonra, kimlik doğrulaması için oturum açmanızı söyleyen bağlantıyı arayın:

    ![Aygıt kimlik doğrulaması için oturum açma istemi](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    Bağlantıyı açın ve belirtilen kodu girin. Bu oturum açma yordamı, Microsoft Azure Çapraz Platform Komut Satırı Arabirimi'ni kullanarak Azure kaynaklarına erişmek için Jupyter not defterinin kimliğini doğrular.  

    ![Cihaz onayında uygulamayı doğrulama](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. Sonuçları önce gelen **hücrede,** çalıştırılabilen kimlikteki değeri kopyalayın ve çalıştırmayı izleyen hücredeki çalıştırılabilen kimlik için **yapıştırın.**

   ![Çalıştırılaçalışan kimliği hücreler arasında kopyalama](media/tutorial-machine-learning-edge-04-train-model/automl-id.png)

1. Not defterinde kalan hücreleri çalıştırın.

1. Not defterini kaydedin ve proje sayfanıza geri dönün.

1. **Açın\_02-turbofan\_dağıtım model.ipynb** ve her hücre çalıştırın. **İş alanını yapılandır'ı**izleyen hücrede kimlik doğrulaması yapmak için oturum açmanız gerekir.

1. Not defterini kaydedin ve proje sayfanıza geri dönün.

### <a name="verify-success"></a>Başarıyı doğrula

Not defterlerinin başarıyla tamamlandığını doğrulamak için birkaç öğe oluşturulduğunu doğrulayın.

1. Azure Not Defterleri proje sayfanızda, bir dönemle başlayan madde adlarının görünmesi için **gizli öğeleri göster'i** seçin.

1. Aşağıdaki dosyaların oluşturulduğunu doğrulayın:

    | Dosya | Açıklama |
    | --- | --- |
    | ./aml_config/.azureml/config.json | Azure Machine Learning Workspace'i oluşturmak için kullanılan yapılandırma dosyası. |
    | ./aml_config/model_config.json | Modeli Azure'daki **turbofanDemo** Machine Learning çalışma alanında dağıtmamız gereken yapılandırma dosyası. |
    | myenv.yml| Dağıtılan Machine Learning modelinin bağımlılıkları hakkında bilgi sağlar.|

1. Aşağıdaki Azure kaynaklarının oluşturulduğunu doğrulayın. Bazı kaynak adları rasgele karakterlerle eklenir.

    | Azure kaynağı | Adı |
    | --- | --- |
    | Makine Öğrenimi çalışma alanı | turborfanDemo |
    | Container Kayıt Defteri | turbofandemoxxxxxx |
    | Uygulamalar Insights | turbofaninsightxxxxxxxx |
    | Key Vault | turbofankeyvaultbxxxxxxxx |
    | Depolama | turbofanstoragexxxxxxxxx |

### <a name="debugging"></a>Hata Ayıklama

Değerleri göstermek için `print()` komut gibi hata ayıklama için not defterine Python deyimleri ekleyebilirsiniz. Tanımlanmamış değişkenler veya nesneler görürseniz, hücreleri ilk beyan edildikleri veya anlık olarak bulundukları yerde çalıştırın.

Not defterlerini yeniden yapmanız gerekiyorsa, daha önce oluşturulmuş dosyaları ve Azure kaynaklarını silmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kalan kullanışlı yaşam (RUL) sınıflandırıcısını eğitmek, sınıflandırıcıyı model olarak kaydetmek, kapsayıcı görüntüsü oluşturmak ve görüntüyü web olarak dağıtmak ve test etmek için turbofan aygıtlarından gelen verileri kullanmak için Azure Notebook'ta çalışan iki Jupyter Notebook kullandık. Hizmet.

Bir IoT Edge aygıtı oluşturmak için sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Bir IoT Edge aygıtını yapılandırma](tutorial-machine-learning-edge-05-configure-edge-device.md)
