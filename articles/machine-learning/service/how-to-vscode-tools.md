---
title: Machine Learning için Visual Studio Code kullanma
titleSuffix: Azure Machine Learning service
description: Visual Studio Code için Azure Machine Learning yüklemeyi ve Azure Machine Learning basit bir deneme oluşturmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 7/12/2019
ms.custom: seodec18
ms.openlocfilehash: 3b49def1af7f13e38c3a9daea32d56bf3c633261
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67871759"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning kullanmaya başlama

Bu makalede, makine öğrenimi ve derin öğrenme modellerini eğitmek ve dağıtmak üzere Visual Studio Code için Azure Machine Learning uzantısını nasıl kullanacağınızı öğreneceksiniz.

[Azure Machine Learning hizmeti](overview-what-is-azure-ml.md) , yerel olarak ve Uzaktan işlem hedeflerinde çalıştırdığınız denemeleri için destek sağlar. Her bir deneme için birden çok çalıştırma çalıştırmalarınızı farklı teknikleri, hiperparametreleri ve daha fazlasını deneyin genellikle gerektiği şekilde takip edebilirsiniz. Azure Machine Learning, özel ölçümler izlemek ve çalıştırma, veri bilimi yeniden üretilebilirliğini ve denetlenebilirlik etkinleştirme denemek için kullanabilirsiniz.

Bu modelleri, test ve üretim gereksinimleriniz için de dağıtabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning hizmetinin ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.

+ Visual Studio Code yüklü olmalıdır. Visual Studio Code, masaüstünüzde çalışan basit ancak güçlü bir kaynak kod düzenleyicidir. Python ve diğer programlama dilleri için yerleşik destek sunar. Visual Studio Code henüz yüklemediyseniz, [hakkında bilgi](https://code.visualstudio.com/docs/setup/setup-overview)edinin.

+ [Python 3,5 veya üstünü yükler](https://www.anaconda.com/download/).


## <a name="install-the-extension-for-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning uzantısını yükler

Azure Machine Learning uzantısını yüklediğinizde, iki uzantı otomatik olarak yüklenir (Internet erişiminiz varsa). Bunlar [Azure Hesap uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) ve [Microsoft Python uzantısıdır](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Azure Machine Learning çalışmak için, Visual Studio Code bir Python tümleşik geliştirme ortamına (IDE) açmanız gerekir. [Visual Studio Code ' de Python](https://code.visualstudio.com/docs/languages/python)kullanabilmeniz Için Microsoft Python uzantısının olması gerekir. Bu uzantı Azure Machine Learning uzantısıyla otomatik olarak yüklenir. Uzantı harika bir IDE Visual Studio Code ve çeşitli Python yorumlayıcıları olan herhangi bir işletim sisteminde kullanılabilir. Microsoft Python uzantısı, otomatik tamamlama, IntelliSense, bağlanma, hata ayıklama ve birim testi sağlamak için tüm Visual Studio Code gücünden yararlanır. Uzantı Ayrıca sanal ve Conda ortamları dahil olmak üzere Python ortamları arasında kolayca geçiş yapmanıza olanak sağlar. Python kodunu düzenlemeyle, çalıştırmaya ve hata ayıklamayla ilgili daha fazla bilgi için bkz. [Python Hello-World öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial).

Azure Machine Learning uzantısını yüklemek için:

1. Visual Studio Code'u açın.

1. Bir Web tarayıcısında, [Visual Studio Code uzantısı için Azure Machine Learning gidin (Önizleme)](https://aka.ms/vscodetoolsforai).

1. Bu Web sayfasında, **yüklensin**' i seçin. 

1. Uzantı sekmesinde, **yükler**' i seçin.

1. Uzantı için bir hoş geldiniz sekmesi Visual Studio Code açılır ve etkinlik çubuğuna Azure symbol (aşağıdaki ekran görüntüsünde kırmızı renkle özetlenmiştir) eklenir.

   ![Visual Studio Code etkinlik çubuğundaki Azure simgesi](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. İletişim kutusunda **oturum aç** ' ı seçin ve Azure ile kimlik doğrulaması yapmak için istemleri izleyin. 
   
   Visual Studio Code uzantısı için Azure Machine Learning birlikte yüklenen Azure Hesap uzantısı, Azure hesabınızda kimlik doğrulaması yapmanıza yardımcı olur. Komutların listesi için bkz. [Azure hesap uzantısının](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)sayfası.

> [!Tip] 
> [Visual Studio Code (Önizleme) Için ıntellicode uzantısına](https://go.microsoft.com/fwlink/?linkid=2006060)göz atın. Intellicode, Python 'da IntelliSense için, geçerli kod bağlamına göre en ilgili diğer işlemleri göstermek gibi bir AI yardımlı özellik kümesi sağlar.

## <a name="install-the-azure-machine-learning-sdk"></a>Azure Machine Learning SDK 'sını yükler

1. Python 3,5 veya sonraki bir sürümünün yüklü olduğundan ve Visual Studio Code tarafından tanındığından emin olun. Şimdi yüklerseniz Visual Studio Code yeniden başlatın ve [bir Python yorumlayıcı seçin](https://code.visualstudio.com/docs/python/python-tutorial).

1. Tümleşik Terminal penceresinde, kullanılacak Python yorumlayıcı 'yı belirtin. Ya da varsayılan Python yorumlayıcısını kullanmak için ENTER ' u seçin.

   ![Yorumlayıcı seçin](./media/vscode-tools-for-ai/python.png)

1. Pencerenin sağ alt köşesinde, [Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) 'nın otomatik olarak yüklendiğini belirten bir bildirim görüntülenir. Yeni oluşturulan Python ortamı yerel ve özeldir ve Azure Machine Learning hizmetiyle çalışmak için Visual Studio Code önkoşulları vardır.

   ![Python için Azure Machine Learning SDK 'sını yükler](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning kullanmaya başlayın

Visual Studio Code ' de eğitim ve makine öğrenimi modellerini dağıtmaya başlamadan önce, bulutta bir [Azure Machine Learning hizmet çalışma alanı](concept-workspace.md) oluşturmanız gerekir. Bu çalışma alanı, modellerinizi ve kaynaklarınızı içerecektir. 

Bir çalışma alanı oluşturmak ve ilk denemenizi eklemek için:

1. Visual Studio Code etkinlik çubuğunda Azure simgesini seçin. Azure Machine Learning kenar çubuğu görüntülenir.

   [![Çalışma alanı oluşturma](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Azure aboneliğinize sağ tıklayın ve **çalışma alanı oluştur**' u seçin. Bir liste görüntülenir. Örnek animasyonlu görüntüde, abonelik adı **ücretsiz denemenize**ve çalışma alanı **teamworkspace**' dür. 

1. Listeden bir kaynak grubu seçin veya komut paletindeki Sihirbazı kullanarak yeni bir tane oluşturun.

1. Alanına yeni çalışma alanınız için benzersiz ve açık bir ad yazın. Örnek görüntüde, çalışma alanı **Teamworkspace**olarak adlandırılır.

1. Yeni çalışma alanını oluşturmak için ENTER ' u seçin. Ağaçta, abonelik adının altında görünür.

1. **Deneme** düğümüne sağ tıklayın ve bağlam menüsünden **deneme oluştur** ' u seçin.  Denemeleri Azure Machine Learning kullanarak çalışmalarınızın izini devam edin.

1. Alanına, denemeniz için bir ad girin. Örnek ekran görüntülerinde, **deneme adı adlandırılmış**olur.
 
1. Yeni deneme oluşturmak için ENTER ' u seçin. Deneme, çalışma alanı adının altındaki ağaçta görünür.

1. Bir çalışma alanında, **etkin** deneme olarak ayarlamak için bir denemeye sağ tıklayabilirsiniz. **Etkin** deneme, geçerli denemenize ait. Visual Studio Code 'daki açık klasörünüz, bulutta bu deneye bağlanacak. Bu klasör yerel Python betiklerinizi içermelidir.

Artık önemli ölçümleriniz deneme geçmişi içinde depolanacak. Benzer şekilde, eğitediğiniz modeller Azure Machine Learning 'ye otomatik olarak yüklenir ve deneme ölçümleriniz ve günlüklerinizle birlikte depolanır. 

[![Visual Studio Code bir klasör iliştirme](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


## <a name="create-and-manage-compute-targets"></a>Oluşturma ve yönetme işlem hedefleri

Visual Studio Code için Azure Machine Learning sayesinde verilerinizi hazırlayabilir, modellerle eğitebilir ve bunları yerel olarak ve Uzaktan işlem hedeflerine dağıtabilirsiniz.

Uzantı Azure Machine Learning için çeşitli uzak işlem hedeflerini destekler. Daha fazla bilgi için, [Azure Machine Learning için desteklenen işlem hedeflerinin](how-to-set-up-training-targets.md)tam listesine bakın.

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>Visual Studio Code Azure Machine Learning için işlem hedefleri oluşturma

İşlem hedefi oluşturmak için:

1. Visual Studio Code etkinlik çubuğunda Azure simgesini seçin. Azure Machine Learning kenar çubuğu görüntülenir.

2. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. Aşağıdaki örnek görüntüde, abonelik adı **ücretsiz denemenize**ve çalışma alanı **teamworkspace**' dür. 

3. Çalışma alanı düğümünde sağ **işlem** düğüm ve **oluşturma işlem**.

4. İşlem hedef listeden seçin. 

5. Komut paletinde bir sanal makine boyutu seçin.

6. Komut paletinde, alanına, işlem hedefi için bir ad girin. 

7. Yeni bir sekmede açılan JSON yapılandırma dosyasında, gelişmiş özellikleri belirtin. Maksimum düğüm sayısı gibi özellikleri belirtebilirsiniz.

8. İşlem hedefini yapılandırmayı bitirdiğinizde pencerenin sağ alt köşesinde **Gönder**' i seçin.

İşte Azure Machine Learning işlem oluşturma (AMLCompute) örneği:

[![Visual Studio Code AML işlem oluşturma](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Çalışma yapılandırma dosyası

Visual Studio Code uzantısı otomatik olarak yerel bir işlem hedefi oluşturur ve yerel bilgisayarınızdaki yerel ve Docker ortamlarınız için yapılandırma çalıştırır. Çalışma yapılandırma dosyalarını ilişkili işlem hedefi düğümü altında bulabilirsiniz. 

## <a name="train-and-tune-models"></a>Modelleri eğitme ve ayarlama

Kodunuzda hızlıca yinelemek, adım adım ve hata ayıklaması yapmak ve kaynak kodu denetimi için çözümünüzü kullanmak üzere Visual Studio Code (Önizleme) için Azure Machine Learning kullanın. 

Azure Machine Learning kullanarak denemenizi yerel olarak çalıştırmak için:

1. Visual Studio Code etkinlik çubuğunda Azure simgesini seçin. Azure Machine Learning kenar çubuğu görüntülenir.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. 

1. Çalışma alanı düğümü altında, **işlem** düğümünü genişletin ve kullanmak Istediğiniz işlem **yapılandırmasını** sağ tıklatın. 

1. Seçin **denemeyi çalıştırma**.

1. Dosya Gezgini 'nde, çalıştırmak istediğiniz betiği seçin. 

1. Çalışmanızı izlemek ve eğitilen modellerinizi görmek için tümleşik Azure Machine Learning portalını görmek üzere **deneme çalıştırmasını görüntüle** ' yi seçin.

Bir denemesinin yerel olarak nasıl çalıştırılacağını gösteren bir örnek aşağıda verilmiştir:

[![Bir denemeyi yerel olarak çalıştırma](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-visual-studio-code"></a>Visual Studio Code 'de denemeleri için uzak hesaplar kullanın

Eğitim için bir uzaktan işlem hedefi kullanmak üzere bir çalıştırma yapılandırma dosyası oluşturmanız gerekir. Azure Machine Learning denemenizi çalıştırmak nerede değil yalnızca bu dosya bildirir ancak ortamı hazırlama nasıl de.

#### <a name="the-conda-dependencies-file"></a>Conda bağımlılıkları dosyasının

Varsayılan olarak, sizin için yeni bir Conda ortamı oluşturulur ve yükleme bağımlılıklarınız yönetilir. Ancak, *aml_config/conda_dependencies. yıml* dosyasında bağımlılıklarınızı ve bunların sürümlerini belirtmeniz gerekir. 

Varsayılan *aml_config/conda_dependencies. yıml* tarafından belirtilen kod parçacığı şunları belirtir `tensorflow=1.12.0`. Bağımlılığın sürümünü belirtmezseniz, en son sürüm kullanılacaktır. Yapılandırma dosyasında ek bağımlılıklar ekleyebilirsiniz.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2
- tensorflow=1.12.0

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

Deneme hesabınızı uzak bir işlem hedefinde Azure Machine Learning çalıştırmak için:

1. Visual Studio Code etkinlik çubuğunda Azure simgesini seçin. Azure Machine Learning kenar çubuğu görüntülenir.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin. 

1. Düzenleyici penceresinde, Python betiğe sağ tıklayın ve AML ' yi seçin **: Azure**'da farklı çalıştır denemesi. 

1. Komut paletinde, işlem hedefini seçin. 

1. Komut paletinde, alanına, çalışma yapılandırması adını girin. 

1. Deneme çalışma zamanı bağımlılıklarını belirtmek için *conda_dependencies. yıml* dosyasını düzenleyin. Ardından pencerenin sağ alt köşesinde **Gönder**' i seçin. 

1. Çalışmanızı izlemek ve eğitilen modellerinizi görmek için tümleşik Azure Machine Learning portalını görmek üzere **deneme çalıştırmasını görüntüle** ' yi seçin.

Bir denemeyi uzaktan işlem hedefinde çalıştırmaya ilişkin bir örnek aşağıda verilmiştir:

[![Uzak hedefte bir deneme çalıştırın](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Model dağıtıp yönetmek
Azure Machine Learning, makine öğrenimi modellerinizi bulutta ve kenarda dağıtabilir ve yönetebilirsiniz. 

### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>Modelinizi Visual Studio Code Azure Machine Learning için kaydedin

Modelinize eğitim edindiniz. bu aşamada, çalışma alanınıza kaydedebilirsiniz. Kayıtlı modelleri izleyebilir ve dağıtabilirsiniz.

Modelinizi kaydetmek için:

1. Visual Studio Code etkinlik çubuğunda Azure simgesini seçin. Azure Machine Learning kenar çubuğu görüntülenir.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanında genişletin.

1. Çalışma alanı düğümü altında sağ **modelleri** ve **modelini kaydettirmek**.

1. Komut paletinde, alanına bir model adı girin. 

1. Listeden bir **model dosyası** (tek modeller için) veya **model klasörünü** (örneğin, TensorFlow gibi birden çok dosya içeren modeller için) karşıya yüklemeyi seçin. 

1. Klasör veya dosyayı seçin.

1. Model özelliklerinizi yapılandırmayı bitirdiğinizde pencerenin sağ alt köşesinde **Gönder**' i seçin. 

Modelinizi Azure Machine Learning için nasıl kaydedeceğinizi gösteren bir örnek aşağıda verilmiştir:

[![Bir modeli AML 'ye kaydetme](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-visual-studio-code"></a>Hizmetinizi Visual Studio Code dağıtma

Visual Studio Code, Web hizmetinizi şu şekilde dağıtabilirsiniz:
+ Test için Azure Container Instances (acı).
+ Üretim için Azure Kubernetes hizmeti (AKS).

Hızlı bir şekilde test etmek için bir ACI kapsayıcısı oluşturmanız gerekmez, çünkü hızlı bir şekilde anında çalışma kapsayıcıları oluşturulur. Ancak, AKS kümelerini önceden yapılandırmanız gerekir. Daha fazla bilgi için bkz. [Azure Machine Learning hizmeti ile modelleri dağıtma](how-to-deploy-and-where.md).

Bir Web hizmeti dağıtmak için:

1. Visual Studio Code etkinlik çubuğunda Azure simgesini seçin. Azure Machine Learning kenar çubuğu görüntülenir.

1. Ağaç görünümünde, Azure aboneliğinizi ve Azure Machine Learning hizmeti çalışma alanınızı genişletin.

1. Çalışma alanı düğümünde genişletin **modelleri** düğümü.

1. Dağıtmak istediğiniz modele sağ tıklayın ve bağlam menüsünden **hizmeti kayıtlı modelden dağıt** ' ı seçin.

1. Komut paletinde, dağıtmak istediğiniz işlem hedefini seçin. 

1. Komut paletinde, alanına bu hizmet için bir ad girin.  

1. Komut paletinde, komut dosyasını bulmak ve seçmek için klavyenizde Enter tuşunu seçin.

1. Komut paletinde klavyenizdeki Enter tuşunu seçerek Conda bağımlılık dosyasına gidip seçin.

1. Hizmet özelliklerinizi yapılandırmayı bitirdiğinizde pencerenin sağ alt köşesinde dağıtılacak **Gönder** ' i seçin. Hizmet özellikleri dosyasında, yerel bir Docker dosyası veya bir Schema. JSON dosyası belirtebilirsiniz.

Web hizmeti artık dağıtılır.

Bir Web hizmetini dağıtmaya ilişkin bir örnek aşağıda verilmiştir:

[![Web hizmeti dağıtma](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Klavye kısayollarını kullanma

Visual Studio Code Azure Machine Learning özelliklere erişmek için klavyeyi kullanabilirsiniz. Bildiğiniz en önemli klavye kısayolu, komut paletini görüntüleyen CTRL + SHIFT + P ' dir. Komut paletinden, en yaygın işlemlere yönelik klavye kısayolları da dahil olmak üzere tüm Visual Studio Code işlevlerine erişebilirsiniz.

[![Visual Studio Code için Azure Machine Learning klavye kısayolları](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* Visual Studio Code dışında Azure Machine Learning eğitme hakkında bir anlatım için bkz [. Öğretici: Modelleri Azure Machine Learning](tutorial-train-models-with-aml.md)ile eğitme.
* Kodu yerel olarak düzenleme, çalıştırma ve hata ayıklama hakkında yönergeler için bkz. [Python Hello-World öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial).
