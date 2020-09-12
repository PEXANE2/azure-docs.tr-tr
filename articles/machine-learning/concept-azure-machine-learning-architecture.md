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
ms.openlocfilehash: c24e9f58154b1523496a82761a8c48ba06dea46c
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651256"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>Azure Machine Learning nasıl kullanılır: mimari ve kavramlar

[Azure Machine Learning](overview-what-is-azure-ml.md)için mimari ve kavramlar hakkında bilgi edinin.  Bu makale, bileşenleri ve makine öğrenimi modellerini oluşturma, dağıtma ve sürdürme sürecinde yardımcı olmak üzere nasıl birlikte çalıştıkları hakkında daha fazla bilgi sahibi olmanızı sağlar.

## <a name="workspace"></a><a name="workspace"></a> Alanında

[Machine Learning çalışma alanı](concept-workspace.md) , Azure Machine Learning için en üst düzey kaynaktır.

:::image type="content" source="media/concept-azure-machine-learning-architecture/architecture.svg" alt-text="Diyagram: bir çalışma alanının ve bileşenlerinin Azure Machine Learning mimarisi":::

Çalışma alanı, şunları yapmak için merkezi yerdir:

* Modellerle ilgili eğitim ve dağıtım için kullandığınız kaynakları yönetme (örneğin, [hesaplar](#compute-instance) )
* Azure Machine Learning kullandığınızda oluşturduğunuz varlıkları depolama, aşağıdakiler dahil:
  * [Ortamlar](#environments)
  * [Denemeler](#experiments)
  * [Pipelines](#ml-pipelines)
  * [Veri kümeleri](#datasets-and-datastores)
  * [Modeller](#models)
  * [Uç Noktalar](#endpoints)

Çalışma alanı, çalışma alanı tarafından kullanılan diğer Azure kaynaklarını içerir:

+ [Azure Container Registry (ACR)](https://azure.microsoft.com/services/container-registry/): eğitim sırasında ve bir modeli dağıtırken kullandığınız Docker kapsayıcılarını kaydeder. Maliyetleri en aza indirmek için ACR yalnızca dağıtım görüntüleri oluşturulduğunda oluşturulur.
+ [Azure depolama hesabı](https://azure.microsoft.com/services/storage/): çalışma alanı için varsayılan veri deposu olarak kullanılır.  Azure Machine Learning işlem örneklerinizde kullanılan jupi Not defterleri burada da depolanır.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): modelleriniz hakkında izleme bilgilerini depolar.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): işlem hedefleri ve çalışma alanı için gereken diğer hassas bilgiler tarafından kullanılan gizli dizileri depolar.

Çalışma alanını başkalarıyla paylaşabilirsiniz.

## <a name="computes"></a>Karmasını

<a name="compute-targets"></a>[İşlem hedefi](concept-compute-target.md) , eğitim betiğinizi çalıştırmak veya hizmet dağıtımınızı barındırmak için kullandığınız herhangi bir makine veya makine kümesidir. Yerel makinenizi veya uzak bir işlem kaynağını işlem hedefi olarak kullanabilirsiniz.  İşlem hedefleri sayesinde, yerel makinenizde eğitime başlayabilir ve ardından eğitim betiğinizi değiştirmeden buluta ölçeklendirebilirsiniz.

Azure Machine Learning, makine öğrenimi görevleri için yapılandırılmış iki tam olarak yönetilen bulut tabanlı sanal makine (VM) sunar:

* <a name="compute-instance"></a>**İşlem örneği**: işlem örneği makine öğrenimi için yüklenen birden çok araç ve ortamı IÇEREN bir sanal makinedir. Bir işlem örneğinin birincil kullanımı geliştirme iş istasyonunuz içindir.  Örnek not defterlerini, kurulum gerekmeden çalıştırmaya başlayabilirsiniz. Bir işlem örneği, eğitim ve ınişsiz işler için işlem hedefi olarak da kullanılabilir.

* **İşlem kümeleri**: işlem kümeleri, çok düğümlü ölçeklendirme özelliklerine sahip VM 'lerin bir kümesidir. Bilgi işlem kümeleri, büyük işler ve üretim için işlem hedefleri için daha uygundur.  Bir iş gönderildiğinde küme otomatik olarak ölçeklendirilir.  Eğitim işlem hedefi olarak veya geliştirme/test dağıtımı için kullanın.

Eğitim işlem hedefleri hakkında daha fazla bilgi için bkz. [eğitim işlem hedefleri](concept-compute-target.md#train).  Dağıtım işlem hedefleri hakkında daha fazla bilgi için bkz. [dağıtım hedefleri](concept-compute-target.md#deploy).

## <a name="datasets-and-datastores"></a>Veri kümeleri ve veri depoları

[**Azure Machine Learning veri kümeleri**](concept-data.md#datasets)  , verilere erişmeyi ve verilerinize çalışmayı kolaylaştırır. Bir veri kümesi oluşturarak, veri kaynağı konumuna, meta verilerinin bir kopyasıyla birlikte bir başvuru oluşturursunuz. Veriler mevcut konumunda kaldığı için ek depolama ücreti ödemeniz ve veri kaynaklarınızın bütünlüğünü riske çıkarmazsınız.

Daha fazla bilgi için bkz. [Azure Machine Learning veri kümeleri oluşturma ve kaydetme](how-to-create-register-datasets.md).  Veri kümelerini kullanarak daha fazla örnek için bkz. [örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets-tutorial).

Veri kümeleri, Azure depolama hizmetlerine güvenli bir şekilde bağlanmak için [veri depolarını](concept-data.md#datastores) kullanır. Veri, kimlik doğrulama kimlik bilgilerinizi ve risk altındaki özgün veri kaynağınızın bütünlüğünü yapmadan mağaza bağlantı bilgilerini depolar. Bu kişiler, çalışma alanıyla ilişkili Key Vault abonelik KIMLIĞINIZ ve belirteç yetkilendirmesi gibi bağlantı bilgilerini depolar, böylece depolama alanınıza güvenli bir şekilde kod yazmanız gerekmeden depolamaya güvenle erişebilirsiniz.

## <a name="environments"></a>Ortamlar

[Çalışma alanı](#workspace)  >  **Ortamlar**

[Ortam](concept-environments.md) , makine öğrenimi modelinizin eğitiminin veya Puanlarınızın gerçekleştiği ortamın kapsüllenmesi. Ortam, eğitim ve Puanlama betikleriniz etrafında Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirtir.  

Kod örnekleri için, [ortamları kullanma](how-to-use-environments.md#manage-environments)konusunun "ortamları yönetme" bölümüne bakın.

## <a name="experiments"></a>Denemeler

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

Örneğin, çalışma yapılandırması için bkz. [modelinize eğitme için bir işlem hedefi kullanma](how-to-set-up-training-targets.md).

### <a name="estimators"></a>Tahmini

Popüler çerçeveler ile model eğitimi kolaylaştırmak için, tahmin aracı sınıfı kolayca çalışma yapılandırması oluşturmanıza olanak sağlar. Seçtiğiniz herhangi bir öğrenme çerçevesini kullanan eğitim betikleri göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py&preserve-view=true) oluşturabilir ve kullanabilirsiniz (örneğin, scikit-öğren).

Tahmini hakkında daha fazla bilgi için bkz. [tahmini ile ml modellerini eğitme](how-to-train-ml-models.md).

### <a name="snapshots"></a>Anlık Görüntüler

[Çalışma alanı](#workspace)  >  [Denemeleri](#experiments)  >  [Çalıştır](#runs)  >  **Anlık görüntü**

Bir çalıştırma gönderdiğinizde, Azure Machine Learning betiği içeren dizini zip dosyası olarak sıkıştırır ve işlem hedefine gönderir. ZIP dosyası daha sonra ayıklanır ve betik burada çalıştırılır. Azure Machine Learning Ayrıca ZIP dosyasını çalışma kaydının bir parçası olarak bir anlık görüntü olarak depolar. Çalışma alanına erişimi olan herkes bir çalıştırma kaydına gözatabilir, anlık görüntüyü indirebilir.


### <a name="logging"></a>Günlüğe Kaydetme

Azure Machine Learning, standart çalıştırma ölçümlerini sizin için otomatik olarak günlüğe kaydeder. Ancak, [isteğe bağlı ölçümleri günlüğe kaydetmek Için Python SDK 'sını](how-to-track-experiments.md)de kullanabilirsiniz.

Günlüklerinizi görüntülemenin birden çok yolu vardır: çalışma durumunu gerçek zamanlı olarak izleme veya tamamlandığında sonuçları görüntüleme. Daha fazla bilgi için bkz. [ml çalıştırma günlüklerini izleme ve görüntüleme](how-to-monitor-view-training-logs.md).


> [!NOTE]
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]

### <a name="git-tracking-and-integration"></a>Git izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Bu, bir Estimator, ML işlem hattı veya betik çalıştırması kullanılarak gönderilen çalışmalarla birlikte çalışır. Ayrıca SDK veya Machine Learning CLı 'dan gönderilen çalıştırmalar için de çalışır.

Daha fazla bilgi için bkz. [Azure Machine Learning Için git tümleştirmesi](concept-train-model-git-integration.md).

## <a name="models"></a>Modeller

En basit olan model, bir girişi alan ve çıkış üreten bir kod parçasıdır. Machine Learning modeli oluşturmak, bir algoritma seçmeyi, verileri verilerle sağlamayı ve [hiper parametreleri ayarlamayı](how-to-tune-hyperparameters.md)içerir. Eğitim, eğitilen bir model üreten ve eğitim sürecinde modelin öğrendiği bir işlem olan yinelemeli bir işlemdir.

Azure Machine Learning dışında eğitilen bir modeli getirebilirsiniz. Ya da Azure Machine Learning bir [işlem hedefine](#compute-targets) bir [deneme](#experiments) [çalıştırması](#runs) göndererek modeli eğitebilirsiniz. Bir modelinize sahip olduktan sonra modeli çalışma alanına [kaydedersiniz](#register-model) .

Azure Machine Learning çerçeve agstik. Bir model oluşturduğunuzda, Scikit-öğren, XGBoost, PyTorch, TensorFlow ve Chainer gibi popüler Machine Learning çerçevesini kullanabilirsiniz.

Scikit-öğrenme kullanarak bir modele eğitim verme örneği için bkz. [öğretici: görüntü sınıflandırma modelini Azure Machine Learning Ile eğitme](tutorial-train-models-with-aml.md).


### <a name="model-registry"></a><a name="register-model"></a> Model kayıt defteri

[Çalışma alanı](#workspace)  >  **Modeller**

**Model kayıt defteri** , Azure Machine Learning çalışma alanınızdaki tüm modelleri izlemenize olanak sağlar.

Modeller ad ve sürüm ile tanımlanır. Varolan bir adla aynı ada sahip bir modeli her kaydettiğinizde kayıt defteri yeni bir sürüm olduğunu varsayar. Sürüm artırılır ve yeni model aynı adla kaydedilir.

Modeli kaydettiğinizde, ek meta veri etiketleri sağlayabilir ve ardından modeller ararken etiketleri kullanabilirsiniz.

> [!TIP]
> Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Etkin bir dağıtım tarafından kullanılan kayıtlı bir modeli silemezsiniz.

Bir modeli kaydetme örneği için bkz. [Azure Machine Learning görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md).

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

#### <a name="real-time-endpoints"></a>Gerçek zamanlı uç noktalar

Tasarımcı 'da (Önizleme) eğitilen bir model dağıttığınızda, [modeli gerçek zamanlı bir uç nokta olarak dağıtabilirsiniz](tutorial-designer-automobile-price-deploy.md). Gerçek zamanlı bir uç nokta, genellikle REST uç noktası aracılığıyla tek bir istek alır ve gerçek zamanlı bir tahmin döndürür. Bu, aynı anda birden çok değeri işleyen ve bir veri deposuna tamamlandığında sonuçları kaydeden toplu işleme karşıtlığına karşılık gelir.

#### <a name="pipeline-endpoints"></a>Ardışık düzen uç noktaları

Ardışık düzen uç noktaları, bir REST uç noktası aracılığıyla [ml](#ml-pipelines) işlem hattı program aracılığıyla aramanızı sağlar. Ardışık düzen uç noktaları, işlem hattı iş akışlarınızı otomatikleştirmenizi sağlar.

Ardışık düzen uç noktası yayımlanmış işlem hatları koleksiyonudur. Bu mantıksal kuruluş aynı uç noktayı kullanarak birden çok işlem hattını yönetmenizi ve çağırmanızı sağlar. Bir ardışık düzen uç noktasındaki yayımlanan her bir ardışık düzen sürümlüdür. Uç nokta için varsayılan bir işlem hattı seçebilir veya REST çağrısında bir sürüm belirtebilirsiniz.
 

#### <a name="iot-module-endpoints"></a>IoT modülü uç noktaları

Dağıtılan bir IoT modülü uç noktası, modelinizi ve ilişkili betiği ya da uygulamanızı ve ek bağımlılıkları içeren bir Docker kapsayıcısıdır. Bu modülleri, uç cihazlarda Azure IoT Edge kullanarak dağıtırsınız.

İzlemeyi etkinleştirdiyseniz, Azure Azure IoT Edge modülünün içindeki modelden telemetri verileri toplar. Telemetri verilerine yalnızca sizin için erişilebilir ve depolama hesabı Örneğinizde depolanır.

Azure IoT Edge modülünüzün çalıştığından emin olur ve bunu barındıran cihazı izler. 
## <a name="automation"></a>Otomasyon

### <a name="azure-machine-learning-cli"></a>Azure Machine Learning CLı 

[Azure MACHINE LEARNING CLI](reference-azure-machine-learning-cli.md) , Azure platformu için platformlar arası komut satırı arabirimi olan Azure CLI 'nin bir uzantısıdır. Bu uzantı, makine öğrenimi etkinliklerinizi otomatikleştirmek için komutlar sağlar.

### <a name="ml-pipelines"></a>ML işlem hatları

Machine Learning işlem [hatlarını](concept-ml-pipelines.md) kullanarak makine öğrenimi aşamalarını birlikte akışlı iş akışları oluşturabilir ve yönetebilirsiniz. Örneğin, bir işlem hattı veri hazırlığı, model eğitimi, model dağıtımı ve çıkarım/Puanlama aşamaları içerebilir. Her aşama, her biri çeşitli işlem hedeflerinde katılımsız olarak çalışabilen birden çok adımı kapsayabilir. 

İşlem hattı adımları yeniden kullanılabilir ve bu adımların çıkışı değişmemişse önceki adımları yeniden çalıştırmadan çalıştırılabilir. Örneğin, veriler değişmemişse maliyetli veri hazırlama adımlarını yeniden çalıştırmadan bir modeli yeniden eğitebilirsiniz. İşlem hatları, makine öğrenimi iş akışının ayrı alanlarında çalışırken veri bilimcilerinin işbirliği yapmasına de olanak tanır.

## <a name="interacting-with-your-workspace"></a>Çalışma alanınız ile etkileşim kurma

### <a name="studio"></a>Studio

[Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md) , çalışma alanınızdaki tüm yapıtların bir web görünümünü sağlar.  Veri kümelerinizin, denemeleri, işlem hatlarının, modellerinizin ve uç noktaların sonuçlarını ve ayrıntılarını görüntüleyebilirsiniz.  Ayrıca, Studio 'daki işlem kaynaklarını ve veri depolarını yönetebilirsiniz.

Ayrıca, Azure Machine Learning parçası olan etkileşimli araçlara de erişebilirsiniz:

+ Kod yazmadan iş akışı adımlarını gerçekleştirmek için [Azure Machine Learning tasarımcı (Önizleme)](concept-designer.md)
+ [Otomatik makine öğrenimi](concept-automated-ml.md) için Web deneyimi
+ Tümleşik Jupyter Not defteri sunucularında kendi kodunuzu yazmak ve çalıştırmak için [not defterlerini Azure Machine Learning](how-to-run-jupyter-notebooks.md) .
+ Verilerinizi etiketleyerek projeler oluşturmak, yönetmek ve izlemek için [veri etiketleme projeleri](how-to-create-labeling-projects.md)

### <a name="programming-tools"></a>Programlama araçları

> [!IMPORTANT]
> Aşağıda işaretlenmiş araçlar (Önizleme) Şu anda genel önizlemede.
> Önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

+  [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)ile herhangi bir Python ortamında hizmetle etkileşime geçin.
+ [R için Azure MACHINE LEARNING SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html) (Önizleme) ile herhangi bir r ortamındaki hizmetle etkileşime geçin.
+ Otomasyon için [Azure MACHINE LEARNING CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) kullanın.
+ [Birçok model Çözüm Hızlandırıcısı](https://aka.ms/many-models) (önizleme) Azure Machine Learning oluşturur ve yüzlerce veya hatta binlerce makine öğrenimi modelini eğitmenize, çalıştırmanıza ve yönetmenize olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanmaya başlamak için bkz.:

* [Azure Machine Learning nedir?](overview-what-is-azure-ml.md)
* [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md)
* [Öğretici (Bölüm 1): bir modeli eğitme](tutorial-train-models-with-aml.md)
