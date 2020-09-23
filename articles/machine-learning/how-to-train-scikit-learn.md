---
title: Scikit 'i eğitme-makine öğrenimi modellerini öğrenin
titleSuffix: Azure Machine Learning
description: Azure Machine Learning üzerinde scikit-eğitim komut dosyalarını nasıl çalıştıracağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: jordane
author: jpe316
ms.date: 07/24/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 7b63ef36d7df43168ed132a740bab026e6e00f3f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90897224"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning"></a>Derleme scikit-Azure Machine Learning uygun ölçekte modeller öğrenin


Bu makalede, Azure Machine Learning ile scikit-eğitim komut dosyalarını nasıl çalıştıracağınızı öğrenin.

Bu makaledeki örnek betikler, ırikit-öğrenme için [Iris veri kümesini](https://archive.ics.uci.edu/ml/datasets/iris)temel alan bir makine öğrenimi modeli oluşturmak üzere Iris çiçek görüntülerini sınıflandırmak için kullanılır.

Bir makine öğrenimi scikit-bir modeli baştan sona öğreniyor olun veya var olan bir modeli buluta getiriyor, elastik bulut bilgi işlem kaynaklarını kullanarak açık kaynaklı eğitim işlerini ölçeklendirmek için Azure Machine Learning kullanabilirsiniz. Azure Machine Learning ile üretim sınıfı modellerini oluşturabilir, dağıtabilir, sürüm ve izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu şu ortamlardan birinde çalıştırın:
 - Azure Machine Learning işlem örneği-indirme veya yükleme gerekli değil

    - Öğreticiyi doldurun: SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu oluşturmak için [ortamı ve çalışma alanını kurma](tutorial-1st-experiment-sdk-setup.md)  .
    - Not defteri sunucusundaki örnekler eğitim klasöründe, bu dizine giderek tamamlanmış ve genişletilmiş bir not defteri bulun: **nasıl kullanılır-azureml > ml-çerçeveleri > scikit-> eğitim > tren-hyperparameter-ayarla-dağıt-with-sköğrenme** klasörü.

 - Kendi Jupyter Notebook sunucunuz

    - [Azure Machine Learning SDK 'Sını yükler](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true).
    - [Bir çalışma alanı yapılandırma dosyası oluşturun](how-to-configure-environment.md#workspace).

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölüm, gerekli Python paketlerini yükleyerek, bir çalışma alanı başlatarak, bir deneme oluşturarak ve eğitim verilerini ve eğitim betikleri karşıya yükleyerek Eğitim denemesini ayarlar.

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning çalışma alanı](concept-workspace.md) , hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Python SDK 'sında bir nesne oluşturarak çalışma alanı yapıtlarına erişebilirsiniz [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true) .

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
from azureml.core import Workspace

ws = Workspace.from_config()
```


### <a name="prepare-scripts"></a>Betikleri hazırlama

Bu öğreticide, [burada](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train_iris.py)sizin için eğitim betiği **train_iris. Kopyala** zaten sağlanmış. Uygulamada, herhangi bir özel eğitim betiğini olduğu gibi götürebilmeniz ve kodunuzu değiştirmek zorunda kalmadan Azure ML ile çalıştırmanız gerekir.

Notlar:
- Belirtilen eğitim betiği, bazı ölçümlerin Azure ML çalıştırmak için betik içindeki nesnesini kullanarak nasıl günlüğe alınacağını gösterir `Run` .
- Belirtilen eğitim betiği, işlevden örnek verileri kullanır  `iris = datasets.load_iris()` .  Kendi verileriniz için verileri eğitim sırasında kullanılabilir hale getirmek üzere veri [kümesini ve betikleri karşıya yükleme](how-to-train-keras.md#data-upload) gibi adımları kullanmanız gerekebilir.

### <a name="define-your-environment"></a>Ortamınızı tanımlayın.

#### <a name="create-a-custom-environment"></a>Özel bir ortam oluşturun.

Conda ortamınızı (sklearn-env. yıml) yazın.
Conda ortamını bir not defterinden yazmak için, ```%%writefile sklearn-env.yml``` hücrenin en üstüne çizgiyi ekleyebilirsiniz.

```yaml
name: sklearn-training-env
dependencies:
  - python=3.6.2
  - scikit-learn
  - numpy
  - pip:
    - azureml-defaults
```

Bu Conda ortam belirtiminden bir Azure ML ortamı oluşturun. Ortam, çalışma zamanında bir Docker kapsayıcısı olarak paketlenir.
```python
from azureml.core import Environment

myenv = Environment.from_conda_specification(name = "myenv", file_path = "sklearn-env.yml")
myenv.docker.enabled = True
```

#### <a name="use-a-curated-environment"></a>Seçkin bir ortam kullanma
Azure ML, kendi görüntünüzü derlemek istemiyorsanız önceden oluşturulmuş, seçkin kapsayıcı ortamları sağlar. Daha fazla bilgi için [buraya](resource-curated-environments.md)bakın.
Seçkin bir ortam kullanmak istiyorsanız, bunun yerine aşağıdaki komutu çalıştırabilirsiniz:

```python
env = Environment.get(workspace=ws, name="AzureML-Tutorial")
```

### <a name="create-a-scriptrunconfig"></a>ScriptRunConfig oluşturma

Bu ScriptRunConfig, işinizi yerel işlem hedefinde yürütme için gönderecek.

```python
from azureml.core import ScriptRunConfig

sklearnconfig = ScriptRunConfig(source_directory='.', script='train_iris.py')
sklearnconfig.run_config.environment = myenv
```

Uzak bir kümeye karşı göndermek istiyorsanız, run_config. Target öğesini istenen işlem hedefi olarak değiştirebilirsiniz.

### <a name="submit-your-run"></a>Çalıştırmanızı gönder
```python
from azureml.core import Experiment

run = Experiment(ws,'train-sklearn').submit(config=sklearnconfig)
run.wait_for_completion(show_output=True)

```

> [!WARNING]
> Azure Machine Learning, tüm kaynak dizinini kopyalayarak eğitim betikleri çalıştırır. Karşıya yüklemek istemediğiniz gizli verileriniz varsa, bir [. Ignore dosyası](how-to-save-write-experiment-files.md#storage-limits-of-experiment-snapshots) kullanın veya kaynak dizine eklemeyin. Bunun yerine, veri [deposu](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py&preserve-view=true)kullanarak verilerinize erişin.

Python ortamınızı özelleştirme hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md). 

## <a name="what-happens-during-run-execution"></a>Çalıştırma yürütme sırasında ne olur?
Çalıştırma yürütüldüğü için aşağıdaki aşamalardan geçer:

- **Hazırlama**: bir Docker görüntüsü, TensorFlow Estimator öğesine göre oluşturulur. Görüntü, çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonra çalışacak şekilde önbelleğe alınır. Günlükler, çalıştırma geçmişine de kaydedilir ve ilerlemeyi izlemek için görüntülenebilir.

- **Ölçeklendirme**: Batch AI kümesi, çalışmayı yürütmek için daha fazla düğüm gerektiriyorsa, küme ölçeği büyütmeyi dener ve şu anda kullanılabilir.

- **Çalışıyor**: betik klasöründeki tüm betikler işlem hedefine yüklenir, veri depoları bağlanır veya kopyalanır ve entry_script yürütülür. Stdout ve./logs klasörü çıkışları, çalıştırma geçmişine akışla kaydedilir ve çalıştırmayı izlemek için kullanılabilir.

- **Işlem sonrası**: çalıştırmanın./çıktılar klasörü, çalışma geçmişine kopyalanır.

## <a name="save-and-register-the-model"></a>Modeli kaydetme ve kaydetme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilir ve kaydedebilirsiniz. Model kaydı, [model yönetimi ve dağıtımını](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümlerini oluşturmanıza imkan tanır.

Modeli kaydetmek için eğitim betiğe, train_iris. Kopyala ' ya aşağıdaki kodu ekleyin. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Aşağıdaki kodla modeli çalışma alanınıza kaydedin. , Ve parametrelerini belirterek,, `model_framework` `model_framework_version` ve `resource_configuration` , No-Code Model dağıtımı kullanılabilir hale gelir. Kod modeli dağıtımı, modelinizi kayıtlı modelden doğrudan bir Web hizmeti olarak dağıtmanızı sağlar ve [`ResourceConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.resource_configuration.resourceconfiguration?view=azure-ml-py&preserve-view=true) nesne, Web hizmeti için işlem kaynağını tanımlar.

```Python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = run.register_model(model_name='sklearn-iris', 
                           model_path='outputs/model.joblib',
                           model_framework=Model.Framework.SCIKITLEARN,
                           model_framework_version='0.19.1',
                           resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5))
```

## <a name="deployment"></a>Dağıtım

Yeni Kaydolmakta olduğunuz model, eğitim için kullandığınız tahmin etmeksizin, Azure Machine Learning ' deki diğer tüm kayıtlı modellerle tamamen aynı şekilde dağıtılabilir. Dağıtım nasıl yapılır, model kaydettirme hakkında bir bölüm içerir, ancak zaten kayıtlı bir modeliniz olduğundan, dağıtım için doğrudan [bir işlem hedefi oluşturmaya](how-to-deploy-and-where.md#choose-a-compute-target) geçebilirsiniz.

### <a name="preview-no-code-model-deployment"></a>Önizle Kod olmayan model dağıtımı

Geleneksel dağıtım yolu yerine, scikit-öğren için kod dışı dağıtım özelliğini (Önizleme) de kullanabilirsiniz. Hiçbir kod modeli dağıtımı, yerleşik tüm scikit-öğrenme model türleri için desteklenmez. Modelinizi yukarıda gösterildiği gibi `model_framework` ,, ve parametreleriyle kaydederek, `model_framework_version` `resource_configuration` [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) modelinizi dağıtmak için yalnızca statik işlevi kullanabilirsiniz.

```python
web_service = Model.deploy(ws, "scikit-learn-service", [model])
```

NOTE: Bu bağımlılıklar önceden oluşturulmuş scikit-öğrenme çıkarımı kapsayıcısına dahil edilmiştir.

```yaml
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
```

Daha ayrıntılı Azure Machine Learning dağıtımı [nasıl](how-to-deploy-and-where.md) ele alır.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir scikit-öğren modeli eğitildiniz ve kaydettiniz ve dağıtım seçenekleri hakkında bilgi edindiniz. Azure Machine Learning hakkında daha fazla bilgi edinmek için bu makaleye bakın.

* [Eğitim sırasında çalıştırma ölçümlerini izleyin](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Azure 'da dağıtılmış derin öğrenme eğitimi için başvuru mimarisi](/azure/architecture/reference-architectures/ai/training-deep-learning)
