---
title: Mimari & temel kavramlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning oluşturan mimari, hüküm ve kavramlar hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 08/20/2020
ms.custom: seoapril2019, seodec18
ms.openlocfilehash: d7bad24510f74a7fadd74328e24ea22855e6fe02
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750840"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>Azure Machine Learning nasıl kullanılır: mimari ve kavramlar

Azure Machine Learning için mimari ve kavramlar hakkında bilgi edinin.

> [!NOTE]
> Bu makalede Azure Machine Learning tarafından kullanılan hüküm ve kavramlar tanımlanmakla birlikte, Azure platformu için hüküm ve kavramlar tanımlamaz. Azure platform terminolojisi hakkında daha fazla bilgi için [Microsoft Azure sözlüğü](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology)bölümüne bakın.

## <a name="workspace"></a><a name="workspace"></a> Alanında

:::image type="content" source="media/concept-azure-machine-learning-architecture/architecture.svg" alt-text="Azure Machine Learning mimarisi":::

[Machine Learning çalışma alanı](concept-workspace.md) , Azure Machine Learning için en üst düzey kaynaktır.  Çalışma alanı, şunları yapmak için merkezi yerdir:
* Modellerle ilgili eğitim ve dağıtım için kullandığınız kaynakları yönetme (örneğin, [hesaplar](#compute-instance) )
* Azure Machine Learning kullandığınızda oluşturduğunuz varlıkları depolama, aşağıdakiler dahil:
  * [Ortamlar](#environments)
  * [Çalıştırmalar](#runs)
  * [Pipelines](#ml-pipelines)
  * [Veri kümeleri](#datasets-and-datastores)
  * [Modeller](#models)
  * [Uç Noktalar](#endpoints)

Çalışma alanı, çalışma alanı tarafından kullanılan diğer Azure kaynaklarını içerir:

+ [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): eğitim sırasında ve bir modeli dağıtırken kullandığınız Docker kapsayıcılarını kaydeder. Maliyetleri en aza indirmek için, dağıtım görüntüleri oluşturuluncaya kadar ACR **yavaş yüklenir** .
+ [Azure depolama hesabı](https://azure.microsoft.com/services/storage/): çalışma alanı için varsayılan veri deposu olarak kullanılır.  Azure Machine Learning işlem örneklerinizde kullanılan jupi Not defterleri burada da depolanır.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): modelleriniz hakkında izleme bilgilerini depolar.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): işlem hedefleri ve çalışma alanı için gereken diğer hassas bilgiler tarafından kullanılan gizli dizileri depolar.

Çalışma alanını başkalarıyla paylaşabilirsiniz.

## <a name="studio"></a>Studio

[Azure Machine Learning Studio](https://ml.azure.com) , çalışma alanınızdaki tüm yapıtların bir web görünümünü sağlar.  Ayrıca, bu portal Azure Machine Learning bir parçası olan etkileşimli araçlara de erişirsiniz:

+ Kod yazmadan iş akışı adımlarını gerçekleştirmek için [Azure Machine Learning tasarımcı (Önizleme)](concept-designer.md)
+ [Otomatik makine öğrenimi](concept-automated-ml.md) için Web deneyimi
+ Verilerin etiketlenmesi için projeleri oluşturmak, yönetmek ve izlemek için [veri etiketleme projeleri](how-to-create-labeling-projects.md)

##  <a name="computes"></a>Karmasını

<a name="compute-targets"></a>[İşlem hedefi](concept-compute-target.md) , eğitim betiğinizi çalıştırdığınız veya hizmet dağıtımınızı barındırabileceğiniz makinedir veya makineler kümesidir. Bu konum, yerel makineniz veya uzak bir işlem kaynağı olabilir.

Azure Machine Learning, makine öğrenimi görevleri için yapılandırılmış iki tam olarak yönetilen bulut tabanlı işlem kaynağı tanıtır:

* <a name="compute-instance"></a>**İşlem örneği** ([Computeinstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance?view=azure-ml-py)): bir işlem örneği, Machine Learning için yüklenen birden çok araç ve ortamı içeren bir sanal makinedir (VM). Kurulum gerekmeden örnek not defterlerini çalıştırmaya başlamak için geliştirme iş istasyonunuz olarak bir işlem örneği kullanın. , Eğitim ve ınişsiz işler için bir işlem hedefi olarak da kullanılabilir.
* **İşlem kümeleri** ([amlcompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py)): çok düğümlü ölçeklendirme özelliklerine sahip VM 'ler kümesi. Bir iş gönderildiğinde otomatik olarak ölçeği ölçeklendirir. Büyük işler ve üretim için işlem hedeflerine daha uygun. Eğitim işlem hedefi olarak veya geliştirme/test dağıtımı için kullanın.

Eğitim işlem hedefleri hakkında daha fazla bilgi için bkz. [eğitim işlem hedefleri](concept-compute-target.md#train).  Dağıtım işlem hedefleri hakkında daha fazla bilgi için bkz. [dağıtım hedefleri](concept-compute-target.md#deploy).

## <a name="datasets-and-datastores"></a>Veri kümeleri ve veri depoları

[**Azure Machine Learning veri kümeleri**](concept-data.md#datasets)  , verilere erişmeyi ve verilerinize çalışmayı kolaylaştırır. Veri kümeleri, model eğitimi ve işlem hattı oluşturma gibi çeşitli senaryolarda verileri yönetir. Azure Machine Learning SDK 'yı kullanarak, temel depolamaya erişebilir, verileri keşfedebilir ve farklı veri kümesi tanımlarının yaşam döngüsünü yönetebilirsiniz.

Veri kümeleri, veya kullanma gibi popüler biçimlerdeki verilerle çalışmaya yönelik yöntemler sağlar `from_delimited_files()` `to_pandas_dataframe()` .

Daha fazla bilgi için bkz. [Azure Machine Learning veri kümeleri oluşturma ve kaydetme](how-to-create-register-datasets.md).  Veri kümelerini kullanarak daha fazla örnek için bkz. [örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets-tutorial).

Bir [**veri deposu**](concept-data.md#datastores) , bir Azure depolama hesabı üzerinden bir depolama soyutlamasıdır. Her çalışma alanının varsayılan bir veri deposu vardır ve ek veri depolarını kaydedebilirsiniz. Veri deposundan dosyaları depolamak ve almak için Python SDK API 'sini veya Azure Machine Learning CLı 'yi kullanın. 

## <a name="models"></a>Modeller

En basit olan model, bir girişi alan ve çıkış üreten bir kod parçasıdır. Machine Learning modeli oluşturmak, bir algoritma seçmeyi, verileri verilerle sağlamayı ve [hiper parametreleri ayarlamayı](how-to-tune-hyperparameters.md)içerir. Eğitim, eğitilen bir model üreten ve eğitim sürecinde modelin öğrendiği bir işlem olan yinelemeli bir işlemdir.

Bir model, Azure Machine Learning bir deneme [çalıştırması](#runs) tarafından üretilir [experiment](#experiments) . Ayrıca, Azure Machine Learning dışında eğitilen bir model de kullanabilirsiniz. Daha sonra [modeli](#register-model) çalışma alanına kaydedersiniz.

Azure Machine Learning çerçeve agstik. Bir model oluşturduğunuzda, Scikit-öğren, XGBoost, PyTorch, TensorFlow ve Chainer gibi popüler Machine Learning çerçevesini kullanabilirsiniz.

Scikit-öğrenme kullanarak bir modele eğitim verme örneği için bkz. [öğretici: görüntü sınıflandırma modelini Azure Machine Learning Ile eğitme](tutorial-train-models-with-aml.md).

### <a name="model-registry"></a><a name="register-model"></a> Model kayıt defteri
[Çalışma alanı](#workspace)  >  **Model kayıt defteri**

**Model kayıt defteri** , Azure Machine Learning çalışma alanınızdaki tüm modelleri izlemenize olanak sağlar.

Modeller ad ve sürüm ile tanımlanır. Varolan bir adla aynı ada sahip bir modeli her kaydettiğinizde kayıt defteri yeni bir sürüm olduğunu varsayar. Sürüm artırılır ve yeni model aynı adla kaydedilir.

Modeli kaydettiğinizde, ek meta veri etiketleri sağlayabilir ve ardından modeller ararken etiketleri kullanabilirsiniz.

> [!TIP]
> Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Etkin bir dağıtım tarafından kullanılan kayıtlı bir modeli silemezsiniz.

Bir modeli kaydetme örneği için bkz. [Azure Machine Learning görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md).


### <a name="environments"></a>Ortamlar

[Çalışma alanı](#workspace)  >  **Ortamlar**

[Ortam](concept-environments.md) , makine öğrenimi modelinizin eğitiminin veya Puanlarınızın gerçekleştiği ortamın kapsüllenmesi. Ortam, eğitim ve Puanlama betikleriniz etrafında Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirtir.

Kod örnekleri için, [ortamları kullanma](how-to-use-environments.md#manage-environments)konusunun "ortamları yönetme" bölümüne bakın.

### <a name="experiments"></a>Denemeler

[Çalışma alanı](#workspace)  >  **Denemeleri**

Deneme, belirtilen bir betikten birçok çalıştırma gruplandırmasıdır. Her zaman bir çalışma alanına aittir. Bir çalıştırma gönderdiğinizde, bir deneme adı sağlarsınız. Çalıştırma bilgileri bu deneme kapsamında saklanır. Bir deneme gönderdiğinizde ad yoksa, otomatik olarak yeni bir deneme oluşturulur.

Deneme kullanmanın bir örneği için bkz. [öğretici: İlk modelinize eğitme](tutorial-1st-experiment-sdk-train.md).

### <a name="runs"></a>Çalıştırmalar

[Çalışma alanı](#workspace)  >  [Denemeleri](#experiments)  >  **Çalıştır**

Bir çalıştırma, bir eğitim betiğinin tek yürütülmesinden oluşur. Deneme genellikle birden çok çalıştırma içerir.

Azure Machine Learning tüm çalıştırmaları kaydeder ve aşağıdaki bilgileri denemenize depolar:

* Çalıştırma (zaman damgası, süre vb.) için meta veriler
* Betiğinizin günlüğe kaydettiği ölçümler
* Deneme tarafından oto tarafından toplanan ve sizin tarafınızdan açıkça yüklenen çıkış dosyaları
* Çalıştırmadan önce betiklerinizi içeren dizinin bir anlık görüntüsü

Bir modeli eğitme için bir komut dosyası gönderdiğinizde bir çalıştırma oluşturursunuz. Bir çalıştırmanın sıfır veya daha fazla alt çalıştırması olabilir. Örneğin, en üst düzey çalıştırmanın iki alt çalıştırması olabilir ve bunların her biri kendi alt öğesi olabilir.

### <a name="run-configurations"></a>Çalıştırma yapılandırması

[Çalışma alanı](#workspace)  >  [Denemeleri](#experiments)  >  [Çalıştır](#runs)  >  **Yapılandırmayı Çalıştır**

Çalıştırma yapılandırması, bir betiğin belirli bir işlem hedefinde nasıl çalıştırılacağını tanımlayan bir yönergeler kümesidir. Yapılandırma, mevcut bir Python ortamının kullanılması veya bir belirtimden oluşturulan bir Conda ortamı kullanılması gibi geniş bir davranış tanımları kümesi içerir.

Çalışma yapılandırması, eğitim betiğinizi içeren dizin içindeki bir dosyaya kalıcı yapılabilir.   Ya da bellek içi bir nesne olarak oluşturulabilir ve bir çalıştırma göndermek için kullanılabilir.

Örneğin, çalışma yapılandırması için bkz. [modelinizi eğitme için bir işlem hedefi seçme ve kullanma](how-to-set-up-training-targets.md).

### <a name="estimators"></a>Tahmini

Popüler çerçeveler ile model eğitimi kolaylaştırmak için, tahmin aracı sınıfı kolayca çalışma yapılandırması oluşturmanıza olanak sağlar. Seçtiğiniz herhangi bir öğrenme çerçevesini kullanan eğitim betikleri göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz (örneğin, scikit-öğren).

Pytorch, TensorFlow ve Chainer görevleri için Azure Machine Learning Ayrıca bu çerçeveleri kullanmayı basitleştirmek için, ilgili [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmini 'ı da sağlar.

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

* Tüm [ml modellerini tahmini ile eğitme](how-to-train-ml-models.md).
* [Pytorch derin öğrenme modellerini Azure Machine Learning ölçeklendirirken eğitme](how-to-train-pytorch.md).
* [TensorFlow modellerini Azure Machine Learning ölçeklendirerek eğitin ve kaydedin](how-to-train-tensorflow.md).
* [Chainer modellerini Azure Machine Learning ölçeklendirerek eğitin ve kaydedin](how-to-train-ml-models.md).

### <a name="snapshots"></a>Anlık Görüntüler

[Çalışma alanı](#workspace)  >  [Denemeleri](#experiments)  >  [Çalıştır](#runs)  >  **Anlık görüntü**

Bir çalıştırma gönderdiğinizde, Azure Machine Learning betiği içeren dizini zip dosyası olarak sıkıştırır ve işlem hedefine gönderir. ZIP dosyası daha sonra ayıklanır ve betik burada çalıştırılır. Azure Machine Learning Ayrıca ZIP dosyasını çalışma kaydının bir parçası olarak bir anlık görüntü olarak depolar. Çalışma alanına erişimi olan herkes bir çalıştırma kaydına gözatabilir, anlık görüntüyü indirebilir.


### <a name="logging"></a>Günlüğe Kaydetme

Çözümünüzü geliştirirken, isteğe bağlı ölçümleri günlüğe kaydetmek için Python betikinizdeki Azure Machine Learning Python SDK 'sını kullanın. Çalıştırıldıktan sonra, çalıştırmanın dağıtmak istediğiniz modeli üretmediğini öğrenmek için ölçümleri sorgulayın.


> [!NOTE]
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]

### <a name="git-tracking-and-integration"></a>Git izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Bu, bir Estimator, ML işlem hattı veya betik çalıştırması kullanılarak gönderilen çalışmalarla birlikte çalışır. Ayrıca SDK veya Machine Learning CLı 'dan gönderilen çalıştırmalar için de çalışır.

Daha fazla bilgi için bkz. [Azure Machine Learning Için git tümleştirmesi](concept-train-model-git-integration.md).

## <a name="deployment"></a>Dağıtım

[Kayıtlı bir modeli](#register-model) hizmet uç noktası olarak dağıtırsınız. Aşağıdaki bileşenlere ihtiyacınız vardır:

* **Ortam**. Bu ortam, modelinizi çıkarım için çalıştırmak için gereken bağımlılıkları kapsüller.
* **Puanlama kodu**. Bu betik istekleri kabul eder, modeli kullanarak istekleri puan eder ve sonuçları döndürür.
* **Çıkarım yapılandırması**. Çıkarım yapılandırması, modeli bir hizmet olarak çalıştırmak için gereken ortamı, giriş betiğini ve diğer bileşenleri belirtir.

Bu bileşenler hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="endpoints"></a>Uç Noktalar

[Çalışma alanı](#workspace)  >  **Uç noktalar**

Bir uç nokta, modelinize veya tümleşik cihaz dağıtımları için bir IoT modülüne bir Web hizmetine barındırılabilir.

#### <a name="web-service-endpoint"></a>Web hizmeti uç noktası

Bir modeli Web hizmeti olarak dağıtırken, uç nokta Azure Container Instances, Azure Kubernetes hizmeti veya FPGAs üzerinde dağıtılabilir. Hizmeti modelinizde, betiğinizden ve ilişkili dosyalardan oluşturursunuz. Bunlar, modelin yürütme ortamını içeren bir temel kapsayıcı görüntüsüne yerleştirilir. Görüntüde, Web hizmetine gönderilen Puanlama isteklerini alan, yük dengeli bir HTTP uç noktası bulunur.

Web hizmetinizi izlemek için telemetri veya model telemetrisini Application Insights sağlayabilirsiniz. Telemetri verilerine yalnızca sizin erişebilirsiniz.  Application Insights ve depolama hesabı örneklerinizin içinde depolanır.

Otomatik ölçeklendirmeyi etkinleştirdiyseniz, Azure dağıtımınızı otomatik olarak ölçeklendirir.

Bir modeli Web hizmeti olarak dağıtmaya ilişkin bir örnek için, bkz. [Azure Container Instances bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

#### <a name="iot-module-endpoints"></a>IoT modülü uç noktaları

Dağıtılan bir IoT modülü uç noktası, modelinizi ve ilişkili betiği ya da uygulamanızı ve ek bağımlılıkları içeren bir Docker kapsayıcısıdır. Bu modülleri, uç cihazlarda Azure IoT Edge kullanarak dağıtırsınız.

İzlemeyi etkinleştirdiyseniz, Azure Azure IoT Edge modülünün içindeki modelden telemetri verileri toplar. Telemetri verilerine yalnızca sizin için erişilebilir ve depolama hesabı Örneğinizde depolanır.

Azure IoT Edge modülünüzün çalıştığından emin olur ve bunu barındıran cihazı izler.
. 
## <a name="automation"></a>Otomasyon

### <a name="azure-machine-learning-cli"></a>Azure Machine Learning CLı 

[Azure MACHINE LEARNING CLI](reference-azure-machine-learning-cli.md) , Azure platformu için platformlar arası komut satırı arabirimi olan Azure CLI 'nin bir uzantısıdır. Bu uzantı, makine öğrenimi etkinliklerinizi otomatikleştirmek için komutlar sağlar.

### <a name="ml-pipelines"></a>ML işlem hatları

Machine Learning işlem [hatlarını](concept-ml-pipelines.md) kullanarak makine öğrenimi aşamalarını birlikte akışlı iş akışları oluşturabilir ve yönetebilirsiniz. Örneğin, bir işlem hattı veri hazırlığı, model eğitimi, model dağıtımı ve çıkarım/Puanlama aşamaları içerebilir. Her aşama, her biri çeşitli işlem hedeflerinde katılımsız olarak çalışabilen birden çok adımı kapsayabilir. 

İşlem hattı adımları yeniden kullanılabilir ve bu adımların çıkışı değişmemişse önceki adımları yeniden çalıştırmadan çalıştırılabilir. Örneğin, veriler değişmemişse maliyetli veri hazırlama adımlarını yeniden çalıştırmadan bir modeli yeniden eğitebilirsiniz. İşlem hatları, makine öğrenimi iş akışının ayrı alanlarında çalışırken veri bilimcilerinin işbirliği yapmasına de olanak tanır.

## <a name="interacting-with-machine-learning"></a>Machine Learning ile etkileşim kurma

> [!IMPORTANT]
> Aşağıda işaretlenmiş araçlar (Önizleme) Şu anda genel önizlemede.
> Önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

+  [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile herhangi bir Python ortamında hizmetle etkileşime geçin.
+ [R için Azure MACHINE LEARNING SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html) (Önizleme) ile herhangi bir r ortamındaki hizmetle etkileşime geçin.
+ Kod yazmadan iş akışı adımlarını gerçekleştirmek için [Azure Machine Learning tasarımcı (Önizleme)](concept-designer.md) kullanın. (Bir [Kurumsal çalışma alanı](concept-workspace.md#upgrade)) tasarımcı kullanmak için gereklidir.)
+ Otomasyon için [Azure MACHINE LEARNING CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) kullanın.
+ [Birçok model Çözüm Hızlandırıcısı](https://aka.ms/many-models) (önizleme) Azure Machine Learning oluşturur ve yüzlerce veya hatta binlerce makine öğrenimi modelini eğitmenize, çalıştırmanıza ve yönetmenize olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanmaya başlamak için bkz.:

* [Azure Machine Learning nedir?](overview-what-is-azure-ml.md)
* [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md)
* [Öğretici (Bölüm 1): bir modeli eğitme](tutorial-train-models-with-aml.md)
