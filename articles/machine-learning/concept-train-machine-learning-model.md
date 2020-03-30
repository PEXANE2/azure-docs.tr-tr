---
title: Model eğitim yöntemleri
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile modeli eğitmek için kullanabileceğiniz farklı yöntemleri öğrenin. Tahminciler Scikit-learn, TensorFlow, Keras, PyTorch ve Chainer gibi popüler çerçevelerle çalışmak için kolay bir yol sağlar. Machine Learning ardışık hatları, gözetimsiz çalıştırmaları zamanlayabilmek, heterojen bilgi işlem ortamlarını kullanmak ve iş akışınızın bazı bölümlerini yeniden kullanmak için kolaylaştırır. Ve çalıştırın yapılandırmaları, eğitim sürecinin çalıştığı bilgi işlem hedefleri üzerinde ayrıntılı denetim sağlar.
services: machine-learning
ms.service: machine-learning
author: Blackmist
ms.author: larryfr
ms.subservice: core
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 68ad9cc47d68f7bc3ae952f7e458781cdc1c4ab2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79129776"
---
# <a name="train-models-with-azure-machine-learning"></a>Azure Machine Learning ile tren modelleri

Azure Machine Learning, SDK'yı kullanarak kod ilk çözümlerinden otomatik makine öğrenimi ve görsel tasarımcı gibi düşük kod çözümlerine kadar modellerinizi eğitmenin çeşitli yollarını sunar. Hangi eğitim yönteminin sizin için uygun olduğunu belirlemek için aşağıdaki listeyi kullanın:

+ [Python için Azure Machine Learning SDK](#python-sdk): Python SDK, her biri farklı özelliklere sahip modelleri eğitmek için çeşitli yollar sağlar.

    | Eğitim yöntemi | Açıklama |
    | ----- | ----- |
    | [Yapılandırmayı çalıştır](#run-configuration) | **Modelleri eğitmenin genel** bir yolu, bir eğitim komut dosyası kullanmak ve yapılandırmayı çalıştırmaktır. Çalıştırma yapılandırması, modelinizi eğitmek için kullanılan eğitim ortamını yapılandırmak için gereken bilgileri sağlar. Bir çalıştırma yapılandırması, eğitim komut dosyası ve bir bilgi işlem hedefi (eğitim ortamı) alabilir ve bir eğitim işi çalıştırabilirsiniz. |
    | [Otomatik makine öğrenimi](#automated-machine-learning) | Otomatik makine **öğrenimi, kapsamlı veri bilimi veya programlama bilgisi olmadan modelleri eğitmenizi**sağlar. Veri bilimi ve programlama geçmişi olan kişiler için, algoritma seçimini ve hiperparametre atomasını otomatikleştirerek zamandan ve kaynaklardan tasarruf etmenin bir yolunu sağlar. Otomatik makine öğrenimini kullanırken bir çalıştırma yapılandırmasını tanımlama konusunda endişelenmenize gerek yoktur. |
    | [Tahminciler](#estimators) | Tahminci **sınıfları, popüler makine öğrenimi çerçevelerine dayalı modelleri eğitmeyi kolaylaştırır.** **Scikit-öğrenmek,** **PyTorch,** **TensorFlow**ve **Chainer**için tahmin sınıfı vardır. Ayrıca, zaten özel bir tahminci sınıfına sahip olmayan çerçevelerle kullanılabilen genel bir tahminci de vardır. Tahmincileri kullanırken çalıştırılan yapılandırmayı tanımlama konusunda endişelenmenize gerek yoktur. |
    | [Makine öğrenimi boru hattı](#machine-learning-pipeline) | Boru hatları farklı bir eğitim yöntemi değildir, ancak iş akışının bir parçası olarak eğitim içerebilir **modüler, yeniden kullanılabilir adımlar kullanarak bir iş akışı tanımlama**yolu. Makine öğrenimi boru hatları, modelleri eğitmek için otomatik makine öğrenimini, tahmincileri ve çalıştıryapılandırmayı kullanmayı destekler. Boru hatları özellikle eğitime odaklanmadığından, boru hattı kullanma nedenleri diğer eğitim yöntemlerinden daha çeşitlidir. Genellikle, şu anda bir ardışık hatlar<br>* Uzun süren eğitim işleri veya veri hazırlama gibi **katılımsız işlemleri zamanlamak** istiyorsunuz.<br>* Heterojen işlem kaynakları ve depolama konumları arasında koordine edilen **birden çok adım** kullanın.<br>* Yeniden eğitim veya toplu puanlama gibi belirli senaryolar için **yeniden kullanılabilir** bir şablon olarak ardışık alanı kullanın.<br>* İş akışınız için **veri kaynaklarını, girdileri ve çıktıları izleyin ve sürüm kurun.**<br>* İş akışınız, **belirli adımlar üzerinde bağımsız olarak çalışan farklı ekipler tarafından uygulanır.** Adımlar daha sonra iş akışını uygulamak için bir ardışık alanda birleşebilir. |

+ [Python için Azure Machine Learning SDK](#r-sdk): SDK, Azure Machine Learning'in Python SDK'sına bağlamak için reticulate paketini kullanır. Bu, Python SDK'da uygulanan temel nesnelere ve yöntemlere herhangi bir R ortamından erişmenizi sağlar.

+ **Tasarımcı**: Azure Machine Learning tasarımcısı (önizleme), kavramların kanıtlanması için veya çok az kodlama deneyimi olan kullanıcılar için makine öğrenimine kolay bir giriş noktası sağlar. Bu bir sürükle ve web tabanlı kullanıcı bir sürülme kullanarak modelleri eğitmek için izin verir. Python kodunu tasarımın bir parçası olarak kullanabilir veya herhangi bir kod yazmadan modelleri eğitebilirsiniz.

+ **CLI**: CLI öğrenen makine, Azure Machine Learning ile sık karşılaşılan görevler için komutlar sağlar ve genellikle **komut dosyası oluşturmak ve görevleri otomatikleştirmek**için kullanılır. Örneğin, bir eğitim komut dosyası veya ardışık bir program oluşturduktan sonra, bir zamanlamada veya eğitim için kullanılan veri dosyaları güncelleştirildiğinde bir eğitim çalışması başlatmak için CLI'yi kullanabilirsiniz. Eğitim modelleri için, eğitim işleri gönderen komutlar sağlar. Çalıştırılabilen yapılandırmaları veya ardışık lıkları kullanarak iş gönderebilir.

Bu eğitim yöntemlerinin her biri, eğitim için farklı türde bilgi işlem kaynakları kullanabilir. Topluca, bu kaynaklar [__işlem hedefleri__](concept-azure-machine-learning-architecture.md#compute-targets)olarak adlandırılır. Bilgi işlem hedefi, Azure Machine Learning Compute, Azure HDInsight veya uzak bir sanal makine gibi yerel bir makine veya bulut kaynağı olabilir.

## <a name="python-sdk"></a>Python SDK'sı

Python için Azure Machine Learning SDK, Azure Machine Learning ile makine öğrenimi iş akışları oluşturmanıza ve çalıştırmanıza olanak tanır. Etkileşimli Python oturumundan, Jupyter Not Defterlerinden, Visual Studio Kodu'ndan veya diğer IDE'den hizmetle etkileşim kurabilirsiniz.

* [Python için Azure Machine Learning SDK nedir](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [SDK’yı yükleme/güncelleştirme](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Machine Learning için bir geliştirme ortamı nı yapılandırma](how-to-configure-environment.md)

### <a name="run-configuration"></a>Yapılandırmayı çalıştır

Azure Machine Learning ile genel bir eğitim işi [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py)kullanılarak tanımlanabilir. Çalıştırma yapılandırması, bir bilgi işlem hedefi üzerinde bir model eğitmek için eğitim komut dosyası(lar) ile birlikte kullanılır.

Yerel bilgisayarınız için bir çalıştırma yapılandırmasıyla başlayabilir ve gerektiğinde bulut tabanlı bir bilgi işlem hedefine geçebilirsiniz. İşlem hedefini değiştirirken, yalnızca kullandığınız çalıştırma yapılandırmasını değiştirirsiniz. Çalışma, girişler, çıktılar ve günlükler gibi eğitim işi yle ilgili bilgileri de kaydeder.

* [Çalıştırılanın yapılandırması nedir?](concept-azure-machine-learning-architecture.md#run-configurations)
* [Öğretici: Tren ilk ML modeli](tutorial-1st-experiment-sdk-train.md)
* [Örnekler: Jupyter Notebook eğitim modelleri örnekleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training)
* [Nasıl kullanılır: Model eğitimi için işlem hedeflerini ayarlama ve kullanma](how-to-set-up-training-targets.md)

### <a name="automated-machine-learning"></a>Otomatik Makine Öğrenmesi

Yinelemeleri, hiperparametre ayarlarını, featurization'ı ve diğer ayarları tanımlayın. Eğitim sırasında Azure Machine Learning farklı algoritmaları ve parametreleri paralel olarak dener. Belirlediğiniz çıkış kriterlerine ulaştığında eğitim durur. Tahmincileri kullanırken çalıştırılan yapılandırmayı tanımlama konusunda endişelenmenize gerek yoktur.

> [!TIP]
> Python SDK'ya ek olarak, [Azure Machine Learning stüdyosu](https://ml.azure.com)aracılığıyla Otomatik ML'yi de kullanabilirsiniz.

* [Otomatik makine öğrenmesi nedir?](concept-automated-ml.md)
* [Öğretici: Otomatik makine öğrenimi ile ilk sınıflandırma modelinizi oluşturun](tutorial-first-experiment-automated-ml.md)
* [Öğretici: Taksi ücretlerini tahmin etmek için otomatik makine öğrenimini kullanın](tutorial-auto-train-models.md)
* [Örnekler: Otomatik makine öğrenimi için Jupyter Notebook örnekleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)
* [Nasıl yapılandırılır: Python'da otomatik ML denemelerini yapılandırma](how-to-configure-auto-train.md)
* [Nasıl: Bir zaman serisi tahmin modeli otomatik tren](how-to-auto-train-forecast.md)
* [Nasıl olun: Azure Machine Learning stüdyosuyla otomatik makine öğrenimi denemeleri oluşturun, keşfedin ve dağıtın](how-to-use-automated-ml-for-ml-models.md)

### <a name="estimators"></a>Tahminciler

Tahminciler, popüler ML çerçevelerini kullanarak modelleri eğitmelerini kolaylaştırır. **Eğer Scikit-learn,** **PyTorch,** **TensorFlow**veya **Chainer**kullanıyorsanız, eğitim için bir tahminci kullanmayı düşünmelisiniz. Ayrıca, zaten özel bir tahminci sınıfına sahip olmayan çerçevelerle kullanılabilen genel bir tahminci de vardır. Tahmincileri kullanırken çalıştırılan yapılandırmayı tanımlama konusunda endişelenmenize gerek yoktur.

* [Tahminciler nedir?](concept-azure-machine-learning-architecture.md#estimators)
* [Öğretici: MNIST verileriyle görüntü sınıflandırma modellerini eğitin ve Azure Machine Learning'i kullanarak scikit-learn](tutorial-train-models-with-aml.md)
* [Örnekler: Jupyter Notebook tahmincileri kullanma örnekleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning)
* [Nasıl yapilir: Eğitimde tahminciler oluşturun](how-to-train-ml-models.md)

### <a name="machine-learning-pipeline"></a>Makine öğrenimi boru hattı

Makine öğrenimi boru hatları daha önce bahsedilen eğitim yöntemlerini (yapılandırmayı çalıştırın, tahminciler ve otomatik makine öğrenimi) kullanabilir. Boru hatları daha çok bir iş akışı oluşturmakla ilgilidir, bu nedenle modellerin eğitiminden daha fazlasını kapsar. Bir ardışık yapıda, otomatik makine öğrenimi, tahminciler veya yapılandırmaları çalıştırkullanarak bir modeli eğitebilirsiniz.

* [Azure Machine Learning'de ML boru hatları nedir?](concept-ml-pipelines.md)
* [Azure Machine Learning SDK ile makine öğrenimi boru hatları oluşturma ve çalıştırma](how-to-create-your-first-pipeline.md)
* [Öğretici: Toplu puanlama için Azure Machine Learning Pipelines'ı kullanın](tutorial-pipeline-batch-scoring-classification.md)
* [Örnekler: Makine öğrenimi boru hatları için Jupyter Notebook örnekleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines)
* [Örnekler: Otomatik makine öğrenimi ile boru hattı](https://aka.ms/pl-automl)
* [Örnekler: Tahmincileri olan boru hattı](https://aka.ms/pl-estimator)

## <a name="r-sdk"></a>R SDK

R SDK, Azure Machine Learning ile R dilini kullanmanızı sağlar. SDK, Azure Machine Learning'in Python SDK'sına bağlamak için reticulate paketini kullanır. Bu, Python SDK'da uygulanan temel nesnelere ve yöntemlere herhangi bir R ortamından erişmenizi sağlar.

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Öğretici: Lojistik regresyon modeli oluşturun](tutorial-1st-r-experiment.md)
* [R başvurusu için Azure Machine Learning SDK](https://azure.github.io/azureml-sdk-for-r/index.html)

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısı

Tasarımcı, web tarayıcınızda sürükle ve bırak arabirimi kullanarak modelleri eğitmenize olanak tanır.

+ [Tasarımcı nedir?](concept-designer.md)
+ [Öğretici : Otomobil fiyatını tahmin et](tutorial-designer-automobile-price-train-score.md)
+ [Regresyon: Fiyatı tahmin edin](how-to-designer-sample-regression-automobile-price-basic.md)
+ [Sınıflandırma: Geliri tahmin etme](how-to-designer-sample-classification-predict-income.md)
+ [Sınıflandırma: Tahmin çalkalama, appetency ve up-satış](how-to-designer-sample-classification-churn.md)
+ [Özel R komut dosyası ile sınıflandırma: Uçuş gecikmelerini tahmin edin](how-to-designer-sample-classification-flight-delay.md)
+ [Metin Sınıflandırması: Vikipedi SP 500 Dataset](how-to-designer-sample-text-classification.md)

## <a name="cli"></a>CLI

CLI öğrenen makine Azure CLI için bir uzantısıdır. Azure Machine Learning ile çalışmak için çapraz platform CLI komutları sağlar. Genellikle, bir makine öğrenme modelini eğitmek gibi görevleri otomatikleştirmek için CLI'yi kullanırsınız.

* [Azure Machine Learning için CLI uzantısını kullanma](reference-azure-machine-learning-cli.md)
* [Azure'da MLOps](https://github.com/microsoft/MLOps)

## <a name="next-steps"></a>Sonraki adımlar

Eğitim ortamlarını nasıl [ayarlayınız](how-to-set-up-training-targets.md)öğrenin.
