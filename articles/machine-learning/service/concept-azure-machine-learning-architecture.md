---
title: Mimari & temel kavramlar
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti oluşturan mimari, hüküm, kavramlar ve iş akışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 07/12/2019
ms.custom: seodec18
ms.openlocfilehash: e6f6c41e5de4f4a053748dfb08dc57e8acac32e5
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848232"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Azure Machine Learning hizmeti nasıl kullanılır: Mimari ve kavramlar

Azure Machine Learning hizmeti için mimari, kavramlar ve iş akışı hakkında bilgi edinin. Hizmetin ana bileşenleri ve hizmetin kullanımı için genel iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure Machine Learning hizmeti mimarisi ve iş akışı](./media/concept-azure-machine-learning-architecture/workflow.png)

## <a name="workflow"></a>İş akışı

Machine Learning modeli iş akışı genellikle bu diziyi izler:

1. **Eğit**
    + **Python** 'da veya görsel arabirimle makine öğrenimi eğitim betikleri geliştirin.
    + Oluşturma ve yapılandırma bir **hedef işlem**.
    + **Komut Gönderme** ortamında çalıştırmak için yapılandırılmış işlem hedefine. Eğitim sırasında betikler, **veri deposundan**okuma veya yazma yapılabilir. Ve yürütme kayıtları **çalışma alanında** **çalışır** olarak kaydedilir ve **denemeleri**altında gruplandırılır.

1. **Paket** -tatmin edici bir çalıştırma oluşturulduktan sonra, kalıcı modeli **model kayıt defterine**kaydedin.

1.  - Geçerli ve geçmişteki çalıştırmaların günlüğe kaydedilen ölçümler için denemeyi**sorgulamayı** doğrula. Ölçümler istenen sonucu belirtmezse, 1. adıma dönün ve betiklerinizde yineleme yapın.

1. **Dağıtım** -modeli kullanan bir Puanlama betiği geliştirin ve modeli Azure 'da **Web hizmeti** olarak veya **IoT Edge bir cihaza** **dağıtın** .

1. Dağıtılmış bir modelin eğitim veri kümesi ve çıkarım verileri arasında **veri kayması** için **izleyici** izleyici. Gerektiğinde, modeli yeni eğitim verileriyle yeniden eğitmek için 1. adıma geri dönün.

## <a name="tools-for-azure-machine-learning"></a>Azure Machine Learning için Araçlar

Bu araçları Azure Machine Learning için kullanın:

+  [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile herhangi bir Python ortamında hizmetle etkileşime geçin.
+ [Azure MACHINE LEARNING CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli)ile makine öğrenimi etkinliklerinizi otomatikleştirin.
+ [Azure Machine Learning vs Code uzantılı](how-to-vscode-tools.md) Visual Studio Code kod yazma
+ Kod yazmadan iş akışı adımlarını gerçekleştirmek için [Azure Machine Learning hizmeti için görsel arabirimi (Önizleme)](ui-concept-visual-interface.md) kullanın.

## <a name="glossary-of-concepts"></a>Kavramların sözlüğü

+ <a href="#workspaces">Alanında</a>
+ <a href="#experiments">Denemeler</a>
+ <a href="#models">Modelde</a>
+ <a href="#run-configurations">Yapılandırmayı Çalıştır</a>
+ <a href="#datasets-and-datastores">Veri kümesi & veri depoları</a>
+ <a href="#compute-targets">İşlem hedefleri</a>
+ <a href="#training-scripts">Eğitim betiği</a>
+ <a href="#runs">Çalıştırma</a>
+ <a href="#github-tracking-and-integration">Git izleme</a>
+ <a href="#snapshots">Görüntüye</a>
+ <a href="#activities">Etkinlik</a>
+ <a href="#images">Görüntü</a>
+ <a href="#deployment">Dağıtım</a>
+ <a href="#web-service-deployments">Web Hizmetleri</a>
+ <a href="#iot-module-deployments">IoT modülleri</a>
+ <a href="#ml-pipelines">ML işlem hatları</a>
+ <a href="#logging">Logging</a>

> [!NOTE]
> Bu makalede Azure Machine Learning hizmeti tarafından kullanılan hüküm ve kavramlar tanımlanmakla birlikte, Azure platformu için hüküm ve kavramlar tanımlamaz. Azure platform terminolojisi hakkında daha fazla bilgi için [Microsoft Azure sözlüğü](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology)bölümüne bakın.


### <a name="workspaces"></a>Çalışma Alanı

[Çalışma alanı](concept-workspace.md) Azure Machine Learning hizmeti için en üst düzey kaynaktır. Azure Machine Learning hizmeti kullandığınızda oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar.

Çalışma alanının bir sınıflandırması aşağıdaki diyagramda gösterilmiştir:

[![Çalışma alanı sınıflandırma](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

Çalışma alanları hakkında daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı nedir?](concept-workspace.md).

### <a name="experiments"></a>Denemeler

Deneme, belirtilen bir betikten birçok çalıştırma gruplandırmasıdır. Her zaman, bir çalışma alanına aittir. Bir farklı çalıştır gönderdiğinizde bir deney adı sağlayın. Çalıştırma için bilgileri, bu deneme altında depolanır. Bir çalıştırma gönderirseniz ve var olmayan bir deneme adı belirtirseniz, yeni belirtilen ada sahip yeni bir deneme otomatik olarak oluşturulur.

Deneme kullanmanın bir örneği için bkz [. Öğretici: İlk modelinizi](tutorial-1st-experiment-sdk-train.md)eğitme.

### <a name="models"></a>Modeller

En basit haliyle bir girdi alır ve çıktıyı üretir kod parçasını modelidir. Makine öğrenme modeli oluşturma, bir algoritma seçme, verilerle sağlama ve ayarlama hiperparametreleri içerir. Eğitim eğitim işlemi sırasında model öğrendikleriniz yalıtan eğitilen bir modelin üreten yinelemeli bir işlemdir.

Bir model, Azure Machine learning'de bir çalıştırma tarafından oluşturulur. Ayrıca, Azure Machine Learning dışında eğitilen bir model de kullanabilirsiniz. Bir modeli Azure Machine Learning hizmet çalışma alanına kaydedebilirsiniz.

Azure Machine Learning hizmeti Framework 'ün agstik. Bir model oluşturduğunuzda, Scikit-öğren, XGBoost, PyTorch, TensorFlow ve Chainer gibi popüler Machine Learning çerçevesini kullanabilirsiniz.

Bir modele eğitim verme örneği için bkz [. Öğretici: Azure Machine Learning hizmeti](tutorial-train-models-with-aml.md)ile görüntü sınıflandırma modeli eğitme.

**Model kayıt defteri** , Azure Machine Learning hizmeti çalışma alanınızdaki tüm modelleri izler.

Modelleri, ada ve sürüme göre tanımlanır. Varolan bir adla aynı ada sahip bir modeli her kaydettiğinizde kayıt defteri yeni bir sürüm olduğunu varsayar. Sürüm artırılır ve yeni model aynı adla kaydedilir.

Modeli kaydettiğinizde, ek meta veri etiketleri sağlayabilir ve ardından modeller ararken etiketleri kullanabilirsiniz.

> [!TIP]
> Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Etkin bir dağıtım tarafından kullanılan kayıtlı bir modeli silemezsiniz.

Bir modeli kaydetme örneği için bkz. [Azure Machine Learning görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md).

### <a name="run-configurations"></a>Çalıştırma yapılandırması

Çalıştırma yapılandırması, bir betiğin belirli bir işlem hedefinde nasıl çalıştırılacağını tanımlayan bir yönergeler kümesidir. Yapılandırma, mevcut bir Python ortamının kullanılması veya bir belirtimden oluşturulan bir Conda ortamı kullanılması gibi geniş bir davranış tanımları kümesi içerir.

Çalışma yapılandırması, eğitim betiğinizi içeren dizin içindeki bir dosyaya kalıcı olabilir veya bir bellek içi nesne olarak oluşturulabilir ve bir çalıştırma göndermek için kullanılabilir.

Örneğin, çalışma yapılandırması için bkz. [modelinizi eğitme için bir işlem hedefi seçme ve kullanma](how-to-set-up-training-targets.md).

### <a name="datasets-and-datastores"></a>Veri kümeleri ve veri depoları

**Azure Machine Learning veri kümeleri** (Önizleme), erişiminizi ve verilerinize çalışmayı kolaylaştırır. Veri kümeleri, model eğitimi ve işlem hattı oluşturma gibi çeşitli senaryolarda verileri yönetir. Azure Machine Learning SDK 'yı kullanarak, temel depolamaya erişebilir, verileri inceleyebilir ve hazırlayabilir, farklı veri kümesi tanımlarının yaşam döngüsünü yönetebilir ve eğitiminde ve üretimde kullanılan veri kümeleri arasında karşılaştırma yapabilirsiniz.

Veri kümeleri, veya `from_delimited_files()` `to_pandas_dataframe()`kullanma gibi popüler biçimlerdeki verilerle çalışmak için yöntemler sağlar.

Daha fazla bilgi için bkz. [Azure Machine Learning veri kümeleri oluşturma ve kaydetme](how-to-create-register-datasets.md).  Veri kümelerini kullanarak daha fazla örnek için bkz. [örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/work-with-data/datasets).

Bir **veri deposu** , bir Azure depolama hesabı üzerinden bir depolama soyutlamasıdır. Veri deposu, arka uç depolama alanı olarak bir Azure Blob kapsayıcısını veya bir Azure dosya paylaşımından kullanabilir. Her çalışma alanının varsayılan bir veri deposu vardır ve ek veri depolarını kaydedebilirsiniz. Veri deposundan dosyaları depolamak ve almak için Python SDK API 'sini veya Azure Machine Learning CLı 'yi kullanın.

### <a name="compute-targets"></a>Hedef işlem

[İşlem hedefi](concept-compute-target.md) , eğitim betiğinizi çalıştırdığınız veya hizmet dağıtımınızı barındıran işlem kaynağını belirtmenize olanak tanır. Bu konum, yerel makineniz veya bulut tabanlı bir işlem kaynağıdır. İşlem hedefleri, kodunuzu değiştirmeden işlem ortamınızı değiştirmeyi kolaylaştırır.

[Eğitim ve dağıtım için kullanılabilir işlem hedefleri](concept-compute-target.md)hakkında daha fazla bilgi edinin.

### <a name="training-scripts"></a>Eğitim betikleriniz

Bir modeli eğitmek için eğitim betiğini ve ilişkili dosyaları içeren dizini belirtin. Eğitim sırasında toplanan bilgileri depolamak için kullanılan bir deneme adı da belirtirsiniz. Eğitim sırasında, tüm dizin eğitim ortamına (işlem hedefi) kopyalanır ve çalıştırma yapılandırması tarafından belirtilen komut dosyası başlatılır. Dizinin bir anlık görüntü, ayrıca çalışma alanında denemeyi altında depolanır.

Bir örnek için bkz [. Öğretici: Azure Machine Learning hizmeti](tutorial-train-models-with-aml.md)ile görüntü sınıflandırma modeli eğitme.

### <a name="runs"></a>Çalıştırmalar

Aşağıdaki bilgileri içeren bir kaydı bir çalıştırmadır:

* Çalıştırma (zaman damgası, süre vb.) için meta veriler
* Betiğinizin günlüğe kaydettiği ölçümler
* Deneme tarafından oto tarafından toplanan ve sizin tarafınızdan açıkça yüklenen çıkış dosyaları
* Bir anlık görüntüsünü çalıştırma önce komut dosyalarınızı içeren dizine

Bir modeli eğitme için bir komut dosyası gönderdiğinizde bir çalıştırma oluşturursunuz. Bir çalıştırma, sıfır veya daha fazla alt çalıştırma olabilir. Örneğin, en üst düzey çalıştırmanın iki alt çalıştırması olabilir ve bunların her biri kendi alt öğesi olabilir.


### <a name="github-tracking-and-integration"></a>GitHub izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Örneğin, deponun geçerli kayıt KIMLIĞI, geçmişin bir parçası olarak günlüğe kaydedilir. Bu, bir Estimator, ML işlem hattı veya betik çalıştırması kullanılarak gönderilen çalışmalarla birlikte çalışır. Ayrıca SDK veya Machine Learning CLı 'dan gönderilen çalıştırmalar için de çalışır.

### <a name="snapshots"></a>Anlık Görüntüler

Bir çalıştırma gönderdiğinizde, Azure Machine Learning betiği içeren dizini zip dosyası olarak sıkıştırır ve işlem hedefine gönderir. ZIP dosyası daha sonra ayıklanır ve betik burada çalıştırılır. Azure Machine Learning, zip dosyası da çalıştırma kaydı bir parçası olarak bir anlık görüntü olarak depolar. Çalışma alanına erişimi olan herkes bir çalıştırma kaydı göz atabilir ve anlık görüntü indirin.

> [!NOTE]
> Gereksiz dosyaların anlık görüntüye eklenmesini engellemek için, bir yoksayma dosyası (. gitignore veya. amlignore) yapın. Bu dosyayı Snapshot dizinine yerleştirin ve içinde yok sayılacak dosya adlarını ekleyin. . Amlignore dosyası [. gitignore dosyasıyla aynı söz dizimini ve desenleri](https://git-scm.com/docs/gitignore)kullanır. Her iki dosya de varsa,. amlignore dosyası önceliklidir.

### <a name="activities"></a>Etkinlikler

Bir etkinlik, uzun süre çalışan bir işlemi temsil eder. Aşağıdaki işlemleri etkinlikleri örnekleri şunlardır:

* Oluşturma ya da işlem hedefi silme
* Bir işlem hedefte betik çalıştırma

Etkinlikler, bu işlemlerin ilerlemesini kolayca izleyebilmeniz için SDK veya Web Kullanıcı arabirimi aracılığıyla bildirim sağlayabilir.

### <a name="images"></a>Görüntüler

Görüntüler, modeli kullanmanız gereken tüm bileşenlerle birlikte, bir modeli güvenilir bir şekilde dağıtmak için bir yol sağlar. Görüntü, aşağıdaki öğeleri içerir:

* Bir model.
* Bir Puanlama komut dosyası veya uygulama. Komut dosyasını modele iletmek ve modelin çıkışını döndürmek için kullanabilirsiniz.
* Model veya Puanlama betiği veya uygulaması için gereken bağımlılıklar. Örneğin, Python Paket bağımlılıklarını listeleyen bir Conda ortam dosyası içerebilir.

Azure Machine Learning, iki tür görüntü oluşturabilir:

* **FPGA görüntüsü**: Azure 'da alan programlanabilir bir kapı dizisine dağıtırken kullanılır.
* **Docker görüntüsü**: FPGA dışında işlem hedeflerine dağıtırken kullanılır. Örnekler Azure Container Instances ve Azure Kubernetes hizmetidir.

Azure Machine Learning hizmeti, varsayılan olarak kullanılan bir temel görüntü sağlar. Kendi özel görüntülerinizi de sağlayabilirsiniz.

### <a name="image-registry"></a>Görüntü kayıt defteri

Resimler, çalışma alanınızdaki **görüntü kayıt defterinde** kataloglandı. Görüntüyü oluştururken ek meta veri etiketleri sağlayabilirsiniz, böylece daha sonra görüntünüzü bulmak için bunları sorgulayabilirsiniz.

Görüntü oluşturma örneği için, bkz. [Azure Container Instances bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

Özel bir görüntü kullanarak model dağıtmaya ilişkin bir örnek için bkz. [özel bir Docker görüntüsü kullanarak model](how-to-deploy-custom-docker-image.md)dağıtma.

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

### <a name="ml-pipelines"></a>ML işlem hatları

Machine Learning işlem hatlarını kullanarak makine öğrenimi aşamalarını birlikte akışlı iş akışları oluşturabilir ve yönetebilirsiniz. Örneğin, bir işlem hattı veri hazırlığı, model eğitimi, model dağıtımı ve çıkarım/Puanlama aşamaları içerebilir. Her aşamada, her biri çeşitli işlem hedeflerini katılımsız çalışabilir, birden çok adım kapsayabilir. 

İşlem hattı adımları yeniden kullanılabilir ve bu adımın çıkışı değişmemişse sonraki adımları yeniden çalıştırmadan çalıştırılabilir. Örneğin, veriler değişmemişse maliyetli veri hazırlama adımlarını yeniden çalıştırmadan bir modeli yeniden eğitebilirsiniz. İşlem hatları, makine öğrenimi iş akışının ayrı alanlarında çalışırken veri bilimcilerinin işbirliği yapmasına de olanak tanır.

Bu hizmetle makine öğrenimi ardışık düzenleri hakkında daha fazla bilgi için bkz. işlem [hatları ve Azure Machine Learning](concept-ml-pipelines.md).

### <a name="logging"></a>Günlüğe Kaydetme

Çözümünüzü geliştirirken, isteğe bağlı ölçümleri günlüğe kaydetmek için Python betikinizdeki Azure Machine Learning Python SDK 'sını kullanın. Çalıştırıldıktan sonra, çalıştırmanın dağıtmak istediğiniz modeli üretmediğini öğrenmek için ölçümleri sorgulayın.

### <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmetine başlamak için bkz.:

* [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)
* [Azure Machine Learning hizmet çalışma alanı oluşturma](how-to-manage-workspace.md)
* [Öğretici (Bölüm 1): Model eğitme](tutorial-train-models-with-aml.md)
