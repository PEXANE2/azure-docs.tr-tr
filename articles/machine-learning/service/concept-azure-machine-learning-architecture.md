---
title: Mimari & temel kavramlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning oluşturan mimari, hüküm, kavramlar ve iş akışları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 07/12/2019
ms.custom: seodec18
ms.openlocfilehash: c886289f098eb41f4b215b4abc2e206db93a27f9
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71710136"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>Azure Machine Learning nasıl kullanılır: mimari ve kavramlar

Azure Machine Learning için mimari, kavramlar ve iş akışı hakkında bilgi edinin. Hizmetin ana bileşenleri ve hizmetin kullanımı için genel iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure Machine Learning mimarisi ve iş akışı](./media/concept-azure-machine-learning-architecture/workflow.png)

## <a name="workflow"></a>İş Akışı

Machine Learning modeli iş akışı genellikle bu diziyi izler:

1. **Eğit**
    + **Python** 'da veya görsel arabirimle makine öğrenimi eğitim betikleri geliştirin.
    + **İşlem hedefi**oluşturun ve yapılandırın.
    + Bu ortamda çalıştırmak için betikleri yapılandırılan işlem hedefine **Gönder** . Eğitim sırasında betikler, **veri deposundan**okuma veya yazma yapılabilir. Ve yürütme kayıtları **çalışma alanında** **çalışır** olarak kaydedilir ve **denemeleri**altında gruplandırılır.

1. **Paket** -tatmin edici bir çalıştırma oluşturulduktan sonra, kalıcı modeli **model kayıt defterine**kaydedin.

1. @No__t **doğrulayın**-1 geçerli ve son çalıştırmaların günlüğe kaydedilen ölçümler Için denemeyi**sorgulayın** . Ölçümler istenen sonucu belirtmezse, 1. adıma dönün ve betiklerinizde yineleme yapın.

1. **Dağıtım** -modeli kullanan bir Puanlama betiği geliştirin ve modeli Azure 'da **Web hizmeti** olarak veya **IoT Edge bir cihaza** **dağıtın** .

1. Dağıtılmış bir modelin eğitim veri kümesi ve çıkarım verileri arasında **veri kayması** için **izleyici** izleyici. Gerektiğinde, modeli yeni eğitim verileriyle yeniden eğitmek için 1. adıma geri dönün.

## <a name="tools-for-azure-machine-learning"></a>Azure Machine Learning için Araçlar

Bu araçları Azure Machine Learning için kullanın:

+  [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile herhangi bir Python ortamında hizmetle etkileşime geçin.
+ [Azure MACHINE LEARNING CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli)ile makine öğrenimi etkinliklerinizi otomatikleştirin.
+ [Azure Machine Learning vs Code uzantılı](how-to-vscode-tools.md) Visual Studio Code kod yazma
+ Kod yazmadan iş akışı adımlarını gerçekleştirmek için [Azure Machine Learning için görsel arabirimi (Önizleme)](ui-concept-visual-interface.md) kullanın.

> [!NOTE]
> Bu makalede Azure Machine Learning tarafından kullanılan hüküm ve kavramlar tanımlanmakla birlikte, Azure platformu için hüküm ve kavramlar tanımlamaz. Azure platform terminolojisi hakkında daha fazla bilgi için [Microsoft Azure sözlüğü](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology)bölümüne bakın.

## <a name="glossary"></a>Sözlük
+ <a href="#activities">Etkinlik</a>
+ <a href="#compute-targets">İşlem hedefleri</a>
+ <a href="#datasets-and-datastores">Veri kümesi & veri depoları</a>
+ <a href="#deployment">Dağıtım</a>
+ <a href="#environments">Lý</a>
+ [Tahmini](#estimators)
+ <a href="#experiments">Denemeleri</a>
+ <a href="#github-tracking-and-integration">Git izleme</a>
+ <a href="#iot-module-deployments">IoT modülleri</a>
+ <a href="#logging">Günlüğe Kaydetme</a>
+ <a href="#ml-pipelines">ML işlem hatları</a>
+ <a href="#models">Modelde</a>
+ <a href="#runs">Çalıştır</a>
+ <a href="#run-configurations">Yapılandırmayı Çalıştır</a>
+ <a href="#snapshots">Anlık Görüntü</a>
+ <a href="#training-scripts">Eğitim betiği</a>
+ <a href="#web-service-deployments">Web Hizmetleri</a>
+ <a href="#workspaces">Alanında</a>

### <a name="activities"></a>Etkinlikler

Etkinlik uzun süren bir işlemi temsil eder. Aşağıdaki işlemler etkinlik örnekleridir:

* İşlem hedefi oluşturma veya silme
* İşlem hedefinde betik çalıştırma

Etkinlikler, bu işlemlerin ilerlemesini kolayca izleyebilmeniz için SDK veya Web Kullanıcı arabirimi aracılığıyla bildirim sağlayabilir.

### <a name="compute-targets"></a>İşlem hedefleri

[İşlem hedefi](concept-compute-target.md) , eğitim betiğinizi çalıştırdığınız veya hizmet dağıtımınızı barındıran işlem kaynağını belirtmenize olanak tanır. Bu konum, yerel makineniz veya bulut tabanlı bir işlem kaynağıdır. İşlem hedefleri, kodunuzu değiştirmeden işlem ortamınızı değiştirmeyi kolaylaştırır.

[Eğitim ve dağıtım için kullanılabilir işlem hedefleri](concept-compute-target.md)hakkında daha fazla bilgi edinin.

### <a name="datasets-and-datastores"></a>Veri kümeleri ve veri depoları

**Azure Machine Learning veri kümeleri** (Önizleme), erişimi daha kolay hale getirir ve verilerinize çalışır. Veri kümeleri, model eğitimi ve işlem hattı oluşturma gibi çeşitli senaryolarda verileri yönetir. Azure Machine Learning SDK 'yı kullanarak, temel depolamaya erişebilir, verileri keşfedebilir ve farklı veri kümesi tanımlarının yaşam döngüsünü yönetebilirsiniz.

Veri kümeleri, `from_delimited_files()` veya `to_pandas_dataframe()` kullanımı gibi popüler biçimlerdeki verilerle çalışma yöntemleri sağlar.

Daha fazla bilgi için bkz. [Azure Machine Learning veri kümeleri oluşturma ve kaydetme](how-to-create-register-datasets.md).  Veri kümelerini kullanarak daha fazla örnek için bkz. [örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets).

Bir **veri deposu** , bir Azure depolama hesabı üzerinden bir depolama soyutlamasıdır. Veri deposu, arka uç depolama alanı olarak bir Azure Blob kapsayıcısını veya bir Azure dosya paylaşımından kullanabilir. Her çalışma alanının varsayılan bir veri deposu vardır ve ek veri depolarını kaydedebilirsiniz. Veri deposundan dosyaları depolamak ve almak için Python SDK API 'sini veya Azure Machine Learning CLı 'yi kullanın.

### <a name="deployment"></a>Dağıtım

Dağıtım, modelinize bir Web hizmetine veya tümleşik cihaz dağıtımları için bir IoT modülüne yönelik bir örneklemedir.

#### <a name="web-service-deployments"></a>Web hizmeti dağıtımları

Dağıtılan bir Web hizmeti Azure Container Instances, Azure Kubernetes Service veya FPGAs kullanabilir. Hizmeti modelinizde, betiğinizden ve ilişkili dosyalardan oluşturursunuz. Bunlar, Web hizmeti için çalışma zamanı ortamı sağlayan bir görüntüde kapsüllenir. Görüntüde, Web hizmetine gönderilen Puanlama isteklerini alan, yük dengeli bir HTTP uç noktası bulunur.

Azure, bu özelliği etkinleştirmeyi seçtiyseniz Application Insights telemetri veya model telemetri toplayarak Web hizmeti dağıtımınızı izlemenize yardımcı olur. Telemetri verilerine yalnızca sizin için erişilebilir ve Application Insights ve depolama hesabı örneklerinizin içinde depolanır.

Otomatik ölçeklendirmeyi etkinleştirdiyseniz, Azure dağıtımınızı otomatik olarak ölçeklendirir.

Bir modeli Web hizmeti olarak dağıtmaya ilişkin bir örnek için, bkz. [Azure Container Instances bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

#### <a name="iot-module-deployments"></a>IoT modülü dağıtımları

Dağıtılan bir IoT modülü, modelinizi ve ilişkili betiği ya da uygulamanızı ve ek bağımlılıkları içeren bir Docker kapsayıcısıdır. Bu modülleri, uç cihazlarda Azure IoT Edge kullanarak dağıtırsınız.

İzlemeyi etkinleştirdiyseniz, Azure Azure IoT Edge modülünün içindeki modelden telemetri verileri toplar. Telemetri verilerine yalnızca sizin için erişilebilir ve depolama hesabı Örneğinizde depolanır.

Azure IoT Edge modülünüzün çalıştığından emin olur ve bunu barındıran cihazı izler.

### <a name="environments"></a>lý

Azure ML ortamları, veri hazırlama, model eğitimi ve modeli sunan bir yeniden oluşturulabilir ortam oluşturmak için kullanılan yapılandırmayı (Docker/Python/Spark/vb.) belirtmek için kullanılır. Bunlar, farklı işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışlarını etkinleştiren Azure Machine Learning çalışma alanınızda yönetilen ve sürümlü varlıklardır.

Eğitim betiğinizi geliştirmek için yerel işlem ortamınızda bir ortam nesnesi kullanabilir, aynı ortamı ölçeklendirerek model eğitimi için Azure Machine Learning Işlem sırasında yeniden kullanabilir ve hatta modelinizi aynı ortamla dağıtabilirsiniz. 

Eğitim ve çıkarım için yeniden [KULLANILABILIR ml ortamı oluşturmayı ve yönetmeyi](how-to-use-environments.md) öğrenin.

### <a name="estimators"></a>Tahmini

Popüler çerçeveler ile model eğitimi kolaylaştırmak için, tahmin aracı sınıfı kolayca çalışma yapılandırması oluşturmanıza olanak sağlar. Seçtiğiniz herhangi bir öğrenme çerçevesini kullanan eğitim betikleri göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz (örneğin, scikit-öğren).

Pytorch, TensorFlow ve Chainer görevleri için Azure Machine Learning Ayrıca bu çerçeveleri kullanmayı basitleştirmek için, ilgili [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmini 'ı da sağlar.

Daha fazla bilgi için aşağıdaki makalelere bakın:

* Tüm [ml modellerini tahmini ile eğitme](how-to-train-ml-models.md).
* [Pytorch derin öğrenme modellerini Azure Machine Learning ölçeklendirirken eğitme](how-to-train-pytorch.md).
* [TensorFlow modellerini Azure Machine Learning ölçeklendirerek eğitin ve kaydedin](how-to-train-tensorflow.md).
* [Chainer modellerini Azure Machine Learning ölçeklendirerek eğitin ve kaydedin](how-to-train-chainer.md).

### <a name="experiments"></a>Denemeleri

Deneme, belirtilen bir betikten birçok çalıştırma gruplandırmasıdır. Her zaman bir çalışma alanına aittir. Bir çalıştırma gönderdiğinizde, bir deneme adı sağlarsınız. Çalıştırma bilgileri bu deneme kapsamında saklanır. Bir çalıştırma gönderirseniz ve var olmayan bir deneme adı belirtirseniz, yeni belirtilen ada sahip yeni bir deneme otomatik olarak oluşturulur.

Deneme kullanmanın bir örneği için bkz. [öğretici: İlk modelinize eğitme](tutorial-1st-experiment-sdk-train.md).


### <a name="github-tracking-and-integration"></a>GitHub izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Örneğin, deponun geçerli kayıt KIMLIĞI, geçmişin bir parçası olarak günlüğe kaydedilir. Bu, bir Estimator, ML işlem hattı veya betik çalıştırması kullanılarak gönderilen çalışmalarla birlikte çalışır. Ayrıca SDK veya Machine Learning CLı 'dan gönderilen çalıştırmalar için de çalışır.


### <a name="logging"></a>Günlüğe Kaydetme

Çözümünüzü geliştirirken, isteğe bağlı ölçümleri günlüğe kaydetmek için Python betikinizdeki Azure Machine Learning Python SDK 'sını kullanın. Çalıştırıldıktan sonra, çalıştırmanın dağıtmak istediğiniz modeli üretmediğini öğrenmek için ölçümleri sorgulayın.

### <a name="ml-pipelines"></a>ML işlem hatları

Machine Learning işlem hatlarını kullanarak makine öğrenimi aşamalarını birlikte akışlı iş akışları oluşturabilir ve yönetebilirsiniz. Örneğin, bir işlem hattı veri hazırlığı, model eğitimi, model dağıtımı ve çıkarım/Puanlama aşamaları içerebilir. Her aşama, her biri çeşitli işlem hedeflerinde katılımsız olarak çalışabilen birden çok adımı kapsayabilir. 

İşlem hattı adımları yeniden kullanılabilir ve bu adımın çıkışı değişmemişse sonraki adımları yeniden çalıştırmadan çalıştırılabilir. Örneğin, veriler değişmemişse maliyetli veri hazırlama adımlarını yeniden çalıştırmadan bir modeli yeniden eğitebilirsiniz. İşlem hatları, makine öğrenimi iş akışının ayrı alanlarında çalışırken veri bilimcilerinin işbirliği yapmasına de olanak tanır.

Bu hizmetle makine öğrenimi ardışık düzenleri hakkında daha fazla bilgi için bkz. işlem [hatları ve Azure Machine Learning](concept-ml-pipelines.md).

### <a name="models"></a>Modeller

En basit olan model, bir girişi alan ve çıkış üreten bir kod parçasıdır. Machine Learning modeli oluşturmak, bir algoritma seçmeyi, verileri verilerle sağlamayı ve hiper parametreleri ayarlamayı içerir. Eğitim, eğitilen bir model üreten ve eğitim sürecinde modelin öğrendiği bir işlem olan yinelemeli bir işlemdir.

Bir model Azure Machine Learning bir çalıştırma tarafından üretilir. Ayrıca, Azure Machine Learning dışında eğitilen bir model de kullanabilirsiniz. Bir modeli Azure Machine Learning çalışma alanına kaydedebilirsiniz.

Azure Machine Learning çerçeve agstik. Bir model oluşturduğunuzda, Scikit-öğren, XGBoost, PyTorch, TensorFlow ve Chainer gibi popüler Machine Learning çerçevesini kullanabilirsiniz.

Scikit-öğren ve bir Estimator kullanarak bir modele eğitim verme örneği için bkz. [öğretici: görüntü sınıflandırma modelini Azure Machine Learning eğitme](tutorial-train-models-with-aml.md).

**Model kayıt defteri** , Azure Machine Learning çalışma alanınızdaki tüm modelleri izler.

Modeller ad ve sürüm ile tanımlanır. Varolan bir adla aynı ada sahip bir modeli her kaydettiğinizde kayıt defteri yeni bir sürüm olduğunu varsayar. Sürüm artırılır ve yeni model aynı adla kaydedilir.

Modeli kaydettiğinizde, ek meta veri etiketleri sağlayabilir ve ardından modeller ararken etiketleri kullanabilirsiniz.

> [!TIP]
> Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Etkin bir dağıtım tarafından kullanılan kayıtlı bir modeli silemezsiniz.

Bir modeli kaydetme örneği için bkz. [Azure Machine Learning görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md).


### <a name="runs"></a>Çalışabilir

Bir çalıştırma, bir eğitim betiğinin tek yürütülmesinden oluşur. Azure Machine Learning tüm çalıştırmaları kaydeder ve aşağıdaki bilgileri depolar:

* Çalıştırma (zaman damgası, süre vb.) için meta veriler
* Betiğinizin günlüğe kaydettiği ölçümler
* Deneme tarafından oto tarafından toplanan ve sizin tarafınızdan açıkça yüklenen çıkış dosyaları
* Çalıştırmadan önce betiklerinizi içeren dizinin bir anlık görüntüsü

Bir modeli eğitme için bir komut dosyası gönderdiğinizde bir çalıştırma oluşturursunuz. Bir çalıştırmanın sıfır veya daha fazla alt çalıştırması olabilir. Örneğin, en üst düzey çalıştırmanın iki alt çalıştırması olabilir ve bunların her biri kendi alt öğesi olabilir.

### <a name="run-configurations"></a>Çalıştırma yapılandırması

Çalıştırma yapılandırması, bir betiğin belirli bir işlem hedefinde nasıl çalıştırılacağını tanımlayan bir yönergeler kümesidir. Yapılandırma, mevcut bir Python ortamının kullanılması veya bir belirtimden oluşturulan bir Conda ortamı kullanılması gibi geniş bir davranış tanımları kümesi içerir.

Çalışma yapılandırması, eğitim betiğinizi içeren dizin içindeki bir dosyaya kalıcı olabilir veya bir bellek içi nesne olarak oluşturulabilir ve bir çalıştırma göndermek için kullanılabilir.

Örneğin, çalışma yapılandırması için bkz. [modelinizi eğitme için bir işlem hedefi seçme ve kullanma](how-to-set-up-training-targets.md).
### <a name="snapshots"></a>görüntüsünü

Bir çalıştırma gönderdiğinizde, Azure Machine Learning betiği içeren dizini zip dosyası olarak sıkıştırır ve işlem hedefine gönderir. ZIP dosyası daha sonra ayıklanır ve betik burada çalıştırılır. Azure Machine Learning Ayrıca ZIP dosyasını çalışma kaydının bir parçası olarak bir anlık görüntü olarak depolar. Çalışma alanına erişimi olan herkes bir çalıştırma kaydına gözatabilir, anlık görüntüyü indirebilir.

> [!NOTE]
> Gereksiz dosyaların anlık görüntüye eklenmesini engellemek için, bir yoksayma dosyası (. gitignore veya. amlignore) yapın. Bu dosyayı Snapshot dizinine yerleştirin ve içinde yok sayılacak dosya adlarını ekleyin. . Amlignore dosyası [. gitignore dosyasıyla aynı söz dizimini ve desenleri](https://git-scm.com/docs/gitignore)kullanır. Her iki dosya de varsa,. amlignore dosyası önceliklidir.

### <a name="training-scripts"></a>Eğitim betikleri

Bir modeli eğitebilmeniz için eğitim betiğini ve ilişkili dosyaları içeren dizini belirlersiniz. Eğitim sırasında toplanan bilgileri depolamak için kullanılan bir deneme adı da belirtirsiniz. Eğitim sırasında, tüm dizin eğitim ortamına (işlem hedefi) kopyalanır ve çalıştırma yapılandırması tarafından belirtilen komut dosyası başlatılır. Dizinin bir anlık görüntüsü de çalışma alanındaki denemeler altında depolanır.

Bir örnek için bkz. [öğretici: görüntü sınıflandırma modelini Azure Machine Learning eğitme](tutorial-train-models-with-aml.md).

### <a name="workspaces"></a>Çalışma Alanları

[Çalışma alanı](concept-workspace.md) Azure Machine Learning için en üst düzey kaynaktır. Azure Machine Learning kullandığınızda oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Çalışma alanını başkalarıyla paylaşabilirsiniz. Çalışma alanlarının ayrıntılı bir açıklaması için bkz. [Azure Machine Learning çalışma alanı nedir?](concept-workspace.md).


### <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanmaya başlamak için bkz.:

* [Azure Machine Learning nedir?](overview-what-is-azure-ml.md)
* [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md)
* [Öğretici (Bölüm 1): bir modeli eğitme](tutorial-train-models-with-aml.md)
