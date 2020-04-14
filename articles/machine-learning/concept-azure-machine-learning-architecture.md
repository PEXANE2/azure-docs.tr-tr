---
title: Mimari & temel kavramlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'i oluşturan mimari, terimler, kavramlar ve iş akışları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 03/17/2020
ms.custom: seoapril2019, seodec18
ms.openlocfilehash: 9f1d23f11cf73680a8861c9f1ac6cbd40ad497a4
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257343"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>Azure Machine Learning nasıl çalışır: Mimari ve kavramlar

Azure Machine Learning'in mimarisi, kavramları ve iş akışı hakkında bilgi edinin. Hizmetin ana bileşenleri ve hizmeti kullanmak için genel iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure Machine Learning mimarisi ve iş akışı](./media/concept-azure-machine-learning-architecture/workflow.png)

## <a name="workflow"></a>İş akışı

Makine öğrenme modeli iş akışı genellikle şu sırayı izler:

1. **Eğitim**
    + **Python,** **R**veya görsel tasarımcı ile makine öğrenimi eğitim komut geliştirin.
    + **Bir bilgi işlem hedefi**oluşturun ve yapılandırın.
    + **Komut dosyalarını,** bu ortamda çalıştırmak üzere yapılandırılmış bir bilgi işlem hedefine gönderin. Eğitim sırasında, komut dosyaları veri depolarından okuyabilir veya **yazabilir.** Eğitim sırasında üretilen günlükler ve çıktılar **çalışma alanında** **çalıştırılır** ve **denemeler**altında gruplandırılır.

1. **Paket** - Tatmin edici bir çalışma bulunduktan sonra, kalıcı modeli **model kayıt defterine kaydedin.**

1. **Validate** - Geçerli ve geçmiş çalıştırmalardan günlüğe kaydedilmiş ölçümler için**denemeyi** sorgula'yı doğrulayın. Ölçümler istenen sonucu göstermiyorsa, adım 1'e geri dön ve komut dosyalarınızı yineleyin.

1. **Dağıtma** - Modeli kullanan bir puanlama komut dosyası geliştirin ve modeli Azure'da **bir web hizmeti** olarak veya bir **IoT Edge aygıtına** **dağıtın.**

1. **Monitör** - Eğitim veri kümesi ile dağıtılan bir modelin çıkarım verileri arasındaki **veri sürüklenmesini** izleyin. Gerektiğinde, modeli yeni eğitim verileriyle yeniden eğitmek için adım 1'e geri dön.

## <a name="tools-for-azure-machine-learning"></a>Azure Makine Öğrenimi Için Araçlar

Azure Machine Learning için bu araçları kullanın:

+  Python için Azure Machine Learning [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile herhangi bir Python ortamında hizmetle etkileşimkurun.
+ R için Azure Machine Learning [SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html)ile herhangi bir R ortamında hizmetle etkileşimkurun.
+ [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli)ile makine öğrenimi etkinliklerinizi otomatikleştirin.
+ İş akışı adımlarını kod yazmadan gerçekleştirmek için [Azure Machine Learning tasarımcısını (önizleme)](concept-designer.md) kullanın.


> [!NOTE]
> Bu makalede Azure Machine Learning tarafından kullanılan terimler ve kavramlar tanımlansa da, Azure platformu için terimler ve kavramlar tanımlanmaz. Azure platformu terminolojisi hakkında daha fazla bilgi için [Microsoft Azure sözlüğüne](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology)bakın.

## <a name="glossary"></a>Sözlük

* [Etkinlik](#activities)
* [Çalışma alanı](#workspaces)
    * [Denemeler](#experiments)
        * [Çalıştırmak](#runs) 
            * [Yapılandırmayı çalıştır](#run-configurations)
            * [Anlık Görüntü](#snapshots)
            * [Git izleme](#github-tracking-and-integration)
            * [Günlüğe kaydetme](#logging)
    * [ML işlem hatları](#ml-pipelines)
    * [Modeller](#models)
        * [Ortamlar](#environments)
        * [Eğitim komut dosyası](#training-scripts)
        * [Tahminciler](#estimators)
    * [Uç Noktalar](#endpoints)
        * [Web hizmeti](#web-service-endpoint)
        * [IoT modülleri](#iot-module-endpoints)
    * [Veri kümesi & veri depoları](#datasets-and-datastores)
    * [İşlem hedefleri](#compute-targets)

### <a name="activities"></a>Etkinlikler

Bir etkinlik uzun süren bir işlemi temsil eder. Aşağıdaki işlemler etkinlik örnekleridir:

* İşlem hedefi oluşturma veya silme
* İşlem hedefinde komut dosyası çalıştırma

Etkinlikler, bu işlemlerin ilerlemesini kolayca izleyebilmeniz için SDK veya web Kullanıcı Durumu aracılığıyla bildirimler sağlayabilir.

### <a name="workspaces"></a>Çalışma Alanları

[Çalışma alanı,](concept-workspace.md) Azure Machine Learning için en üst düzey kaynaktır. Azure Machine Learning'i kullandığınızda oluşturduğunuz tüm yapılarla çalışmak için merkezi bir yer sağlar. Çalışma alanını başkalarıyla paylaşabilirsiniz. Çalışma alanlarının ayrıntılı bir açıklaması için [bkz.](concept-workspace.md)

### <a name="experiments"></a>Denemeler

Deneme, belirli bir komut dosyasından birçok çalıştırmanın gruplandırmasıdır. Her zaman bir çalışma alanına aittir. Bir çalıştırma gönderdiğinde, bir deneme adı sağlarsınız. Çalıştırma bilgileri bu denemenin altında depolanır. Bir çalıştırma gönderir ve var olmayan bir deneme adı belirtirseniz, bu yeni belirtilen adla ilgili yeni bir deneme otomatik olarak oluşturulur.

Deneme kullanma örneği için [Bkz. Öğretici: İlk modelinizi eğitin.](tutorial-1st-experiment-sdk-train.md)

### <a name="runs"></a>Çalıştırmalar

Çalışma, bir eğitim komut dosyasının tek bir yürütülmesidir. Bir deneme genellikle birden çok çalıştırma içerir.

Azure Machine Learning tüm çalıştırmaları kaydeder ve denemede aşağıdaki bilgileri saklar:

* Çalıştırma yla ilgili meta veriler (zaman damgası, süre vb.)
* Komut dosyanız tarafından günlüğe kaydedilen ölçümler
* Deneme tarafından otomatik olarak toplanan veya sizin yeriniz tarafından açıkça yüklenen çıktı dosyaları
* Çalışmadan önce komut dosyalarınızı içeren dizinin anlık görüntüsü

Bir modeli eğitmek için bir komut dosyası gönderdiğinde bir çalışma üretirsiniz. Bir çalıştırmasıfır veya daha fazla alt çalışır olabilir. Örneğin, üst düzey çalıştırmada, her biri kendi alt çalıştırılan iki alt çalıştırma olabilir.

### <a name="run-configurations"></a>Yapılandırmaları çalıştırma

Çalıştırılan yapılandırma, komut dosyasının belirli bir işlem hedefinde nasıl çalıştırılması gerektiğini tanımlayan bir yönerge kümesidir. Yapılandırma, varolan bir Python ortamını kullanmak veya belirtimden oluşturulmuş bir Conda ortamını kullanmak gibi çok çeşitli davranış tanımları içerir.

Çalışma yapılandırması, eğitim komut dosyanızı içeren dizinin içindeki bir dosyada kalıcı hale getirilebilir veya bellek içi bir nesne olarak oluşturulabilir ve bir çalıştırma göndermek için kullanılabilir.

Örneğin yapılandırmaları çalıştırın, [bkz.](how-to-set-up-training-targets.md)

### <a name="snapshots"></a>Anlık Görüntüler

Bir çalıştırma gönderdiğinde, Azure Machine Learning komut dosyasını içeren dizini sıkıştırır ve bunu bilgi işlem hedefine gönderir. Zip dosyası daha sonra ayıklanır ve komut dosyası orada çalıştırılır. Azure Machine Learning, zip dosyasını çalışma kaydının bir parçası olarak anlık görüntü olarak da saklar. Çalışma alanına erişimi olan herkes bir çalışma kaydına göz atabilir ve anlık görüntünün karşıdan yüklenebilir.

> [!NOTE]
> Gereksiz dosyaların anlık görüntüye eklenmesini önlemek için, yoksayma dosyası (.gitignore veya .amlignore) yapın. Bu dosyayı Anlık Görüntü dizini'ne yerleştirin ve dosya adlarını yoksaymak için ekleyin. .amlignore dosyası, [.gitignore dosyasıyla aynı sözdizimini ve desenleri](https://git-scm.com/docs/gitignore)kullanır. Her iki dosya varsa, .amlignore dosyası önceliklidir.

### <a name="github-tracking-and-integration"></a>GitHub izleme ve entegrasyon

Kaynak dizininin yerel bir Git deposu olduğu bir eğitim çalışması başlattığınızda, depo hakkındaki bilgiler çalışma geçmişinde depolanır. Bu, bir tahminci, ML ardışık işlem veya komut dosyası çalıştırıkullanılarak gönderilen çalıştırmalarla çalışır. Ayrıca SDK veya Machine Learning CLI'den gönderilen çalıştırmalar için de çalışır.

Daha fazla bilgi [için Azure Machine Learning için Git tümleştirmesi'ne](concept-train-model-git-integration.md)bakın.

### <a name="logging"></a>Günlüğe kaydetme

Çözümünüzü geliştirirken, rasgele ölçümleri günlüğe kaydetmek için Python komut dosyanızdaki Azure Machine Learning Python SDK'yı kullanın. Çalıştırmadan sonra, çalıştırmanın dağıtmak istediğiniz modeli üretip üretmediğini belirlemek için ölçümleri sorgula.

### <a name="ml-pipelines"></a>ML Boru Hatları

Makine öğrenimi aşamalarını bir araya getiren iş akışlarını oluşturmak ve yönetmek için makine öğrenimi ardışık hatlarını kullanırsınız. Örneğin, bir ardışık iş tonu veri hazırlama, model eğitimi, model dağıtımı ve çıkarım/puanlama aşamalarını içerebilir. Her aşama, her biri çeşitli işlem hedeflerinde gözetimsiz çalıştırılabilen birden çok adımı kapsayabilir. 

Ardışık iş adı adımları yeniden kullanılabilir ve bu adımların çıktısı değişmemişse önceki adımları yeniden çalıştırmadan çalıştırılabilir. Örneğin, veriler değişmediyse, maliyetli veri hazırlama adımlarını yeniden çalıştırmadan bir modeli yeniden eğitebilirsiniz. Boru hatları, veri bilimcilerin makine öğrenimi iş akışının ayrı alanlarında çalışırken işbirliği yapmasına da olanak sağlar.

Bu hizmete sahip makine öğrenimi boru hatları hakkında daha fazla bilgi [için, bkz.](concept-ml-pipelines.md)

### <a name="models"></a>Modeller

En basit haliyle, bir model girdi alan ve çıktı üreten bir kod parçasıdır. Bir makine öğrenme modeli oluşturma, bir algoritma seçme, veri ile sağlayan ve hiperparametreleri atoklama içerir. Eğitim, modelin eğitim sürecinde öğrendiklerini özetleyen eğitilmiş bir model üreten yinelemeli bir süreçtir.

Bir model, Azure Machine Learning'deki bir çalışma yla üretilir. Azure Machine Learning dışında eğitilmiş bir modeli de kullanabilirsiniz. Bir modeli Azure Machine Learning çalışma alanına kaydedebilirsiniz.

Azure Machine Learning çerçeve agnostiktir. Bir model oluşturduğunuzda, Scikit-learn, XGBoost, PyTorch, TensorFlow ve Chainer gibi popüler makine öğrenimi çerçevelerini kullanabilirsiniz.

Scikit-learn ve bir tahminci kullanarak bir modeli eğitmek için [bkz.](tutorial-train-models-with-aml.md)

**Model kayıt defteri,** Azure Machine Learning çalışma alanınızdaki tüm modelleri izler.

Modeller ad ve sürümile tanımlanır. Varolan bir modelle aynı ada sahip bir modeli her kaydettirseniz, kayıt defteri bunun yeni bir sürüm olduğunu varsayar. Sürüm artımlı ve yeni model aynı adla kaydedilir.

Modeli kaydettirdiğinizde, ek meta veri etiketleri sağlayabilir ve modelleri ararken etiketleri kullanabilirsiniz.

> [!TIP]
> Kayıtlı model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıt olduktan sonra, kayıtlı modeli karşıdan yükleyebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Etkin bir dağıtım tarafından kullanılan kayıtlı bir modeli silemezsiniz.

Bir modelkaydetme örneği için bkz: [Azure Machine Learning ile bir görüntü sınıflandırma modelini eğitin.](tutorial-train-models-with-aml.md)

### <a name="environments"></a>Ortamlar

Azure ML Ortamları, veri hazırlama, model eğitimi ve model sunma için tekrarlanabilir bir ortam oluşturmak için kullanılan yapılandırmayı (Docker / Python / Spark / vb.) belirtmek için kullanılır. Bunlar, Azure Machine Learning çalışma alanınızda farklı bilgi işlem hedefleri arasında çoğaltılabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışları sağlayan yönetilen ve sürümlenmiş varlıklardır.

Eğitim komut dosyanızı geliştirmek, ölçekolarak model eğitimi için Azure Machine Learning Compute'da aynı ortamı yeniden kullanmak ve hatta modelinizi aynı ortamla dağıtmak için yerel bilgi işleminizdeki bir ortam nesnesini kullanabilirsiniz. 

Eğitim ve çıkarım [için yeniden kullanılabilir bir ML ortamını nasıl oluşturup yönetebileceğimizi](how-to-use-environments.md) öğrenin.

### <a name="training-scripts"></a>Eğitim komut dosyaları

Bir modeli eğitmek için, eğitim komut dosyasını ve ilişkili dosyaları içeren dizini belirtirsiniz. Ayrıca, eğitim sırasında toplanan bilgileri depolamak için kullanılan bir deneme adı da belirtirsiniz. Eğitim sırasında, tüm dizin eğitim ortamına (bilgi işlem hedefi) kopyalanır ve çalıştırma yapılandırması tarafından belirtilen komut dosyası başlatılır. Dizinin anlık görüntüsü de çalışma alanındaki denemenin altında depolanır.

Örneğin, [Bkz. Öğretici: Azure Machine Learning ile bir görüntü sınıflandırma modeli eğitin.](tutorial-train-models-with-aml.md)

### <a name="estimators"></a>Tahminciler

Popüler çerçevelerle model eğitimini kolaylaştırmak için, tahminci sınıfı çalıştırma yapılandırmalarını kolayca oluşturmanıza olanak tanır. Seçtiğiniz herhangi bir öğrenme çerçevesini (scikit-learn gibi) kullanan eğitim komut dosyalarını göndermek için genel bir [Tahminci](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz.

PyTorch, TensorFlow ve Chainer görevleri için Azure Machine Learning, bu çerçeveleri kullanarak basitleştirmek için ilgili [PyTorch,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmincileri de sağlar.

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Tahmincilerile ML modellerini eğitin.](how-to-train-ml-models.md)
* [Azure Machine Learning ile Pytorch derin öğrenme modellerini ölçekte eğitin.](how-to-train-pytorch.md)
* [Azure Machine Learning ile TensorFlow modellerini ölçekte eğitin ve kaydedin.](how-to-train-tensorflow.md)
* [Azure Machine Learning ile Chainer modellerini ölçekte eğitin ve kaydedin.](how-to-train-ml-models.md)

### <a name="endpoints"></a>Uç Noktalar

Bitiş noktası, modelinizin bulutta barındırılabilen bir web hizmetine veya tümleşik aygıt dağıtımları için bir IoT modülüne anında dönüştürülmesidir.

#### <a name="web-service-endpoint"></a>Web hizmeti bitiş noktası

Bir modeli web hizmeti olarak dağıtırken bitiş noktası Azure Kapsayıcı Örnekleri, Azure Kubernetes Hizmeti veya FPGA'larda dağıtılabilir. Hizmeti modelinizden, komut dosyanızdan ve ilişkili dosyalarınızdan oluşturursunuz. Bunlar, modelin yürütme ortamını içeren bir temel kapsayıcı görüntüsüne yerleştirilir. Görüntü, web hizmetine gönderilen puanlama isteklerini alan, yük dengeli, HTTP bitiş noktasına sahiptir.

Azure, bu özelliği etkinleştirmeyi seçtiyseniz, Uygulama Öngörüleri telemetrisini veya model telemetrisini toplayarak web hizmetinizi izlemenize yardımcı olur. Telemetri verilerine yalnızca sizin erişebilirsiniz ve Uygulama Öngörüleri ve depolama hesabı örneklerinizde depolanır.

Otomatik ölçeklendirmeyi etkinleştirdimseniz, Azure dağıtımınızı otomatik olarak ölçeklendirin.

Bir modeli web hizmeti olarak dağıtma örneği için bkz: [Azure Kapsayıcı Örnekleri'nde bir resim sınıflandırma modeli dağıt.](tutorial-deploy-models-with-aml.md)

#### <a name="iot-module-endpoints"></a>IoT modülü uç noktaları

Dağıtılan IoT modülü bitiş noktası, modelinizi, ilişkili komut dosyanızı veya uygulamanızı ve ek bağımlılıkları içeren bir Docker kapsayıcısır. Bu modülleri kenar aygıtlarında Azure IoT Edge'i kullanarak dağıtAbilirsiniz.

İzlemeyi etkinleştirdiyseniz, Azure, Azure IoT Edge modülündeki modelden telemetri verileri toplar. Telemetri verilerine yalnızca sizin erişebilirsiniz ve depolama hesabı örneğinde depolanır.

Azure IoT Edge, modülünüzün çalışmasını sağlar ve modülü barındıran aygıtı izler.


### <a name="compute-instance-preview"></a><a name="compute-instance"></a>İşlem örneği (önizleme)

Azure Machine Learning bilgi işlem örneği (eski adıyla Notebook VM), makine **öğrenimi** için yüklü birden çok araç ve ortam içeren tam olarak yönetilen bulut tabanlı bir iş istasyonudur. İşlem örnekleri, işleri eğitmek ve çıkartma için bir işlem hedefi olarak kullanılabilir. Büyük görevlerde, çok düğümlü ölçekleme özelliklerine sahip Azure Machine Learning bilgi işlem [kümeleri](how-to-set-up-training-targets.md#amlcompute) daha iyi bir bilgi işlem hedef seçimidir.

[Bilgi işlem örnekleri](concept-compute-instance.md)hakkında daha fazla bilgi edinin.

### <a name="datasets-and-datastores"></a>Veri kümeleri ve veri depoları

**Azure Machine Learning Datasets** (önizleme), verilerinize erişmenizi ve verilerinizle çalışmayı kolaylaştırır. Veri kümeleri, model eğitimi ve ardışık hat oluşturma gibi çeşitli senaryolarda verileri yönetir. Azure Machine Learning SDK'yı kullanarak, temel depolama alanına erişebilir, verileri keşfedebilir ve farklı Dataset tanımlarının yaşam döngüsünü yönetebilirsiniz.

Veri kümeleri, veri `from_delimited_files()` kullanma veya `to_pandas_dataframe()`kullanma gibi popüler biçimlerde çalışma yöntemleri sağlar.

Daha fazla bilgi için Azure [Machine Learning Datasets oluştur ve kaydol 'a](how-to-create-register-datasets.md)bakın.  Datasets kullanarak daha fazla örnek için [örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets)bakın.

**Veri deposu,** Azure depolama hesabı üzerinden bir depolama soyutlamadır. Veri deposu, arka uç depolama alanı olarak bir Azure blob kapsayıcısı veya Azure dosya paylaşımı nı kullanabilir. Her çalışma alanının varsayılan bir veri deposu vardır ve ek veri depolarını kaydedebilirsiniz. Veri deposundaki dosyaları depolamak ve almak için Python SDK API'sını veya Azure Machine Learning CLI'yi kullanın.

### <a name="compute-targets"></a>İşlem hedefleri

Bir [bilgi işlem hedefi,](concept-compute-target.md) eğitim komut dosyanızı çalıştırdığınız veya hizmet dağıtımınızı barındırdığınız bilgi işlem kaynağını belirtmenize olanak tanır. Bu konum yerel makineniz veya bulut tabanlı bir bilgi işlem kaynağınız olabilir.

Eğitim ve [dağıtım için kullanılabilir bilgi işlem hedefleri](concept-compute-target.md)hakkında daha fazla bilgi edinin.

### <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning'e başlamak için bkz:

* [Azure Machine Learning nedir?](overview-what-is-azure-ml.md)
* [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md)
* [Öğretici (bölüm 1): Bir model eğitmek](tutorial-train-models-with-aml.md)
