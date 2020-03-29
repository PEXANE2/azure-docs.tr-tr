---
title: Tren scikit-öğren makine öğrenme modelleri
titleSuffix: Azure Machine Learning
description: Azure Machine Learning SKlearn tahminci sınıfını kullanarak scikit-learn eğitim komut dosyalarınızı kurumsal ölçekte nasıl çalıştırılacınız öğrenin. Örnek komut dosyaları, scikit-learn'in iris veri kümesine dayalı bir makine öğrenme modeli oluşturmak için iris çiçek görüntülerini sınıfa sınıfa.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: bdd2cc400c3df75742689258caea8cb87ee8ccc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942280"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning"></a>Azure Machine Learning ile ölçekte scikit-learn modelleri oluşturun
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning [SKlearn tahminci](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) sınıfını kullanarak scikit-learn eğitim komut dosyalarınızı kurumsal ölçekte nasıl çalıştırılacanız gerektiğini öğrenin. 

Bu makaledeki örnek komut dosyaları, iris çiçek görüntülerini sınıflandırmak için scikit-learn'in [iris veri kümesine](https://archive.ics.uci.edu/ml/datasets/iris)dayalı bir makine öğrenme modeli oluşturmak için kullanılır.

İster makine öğrenimi scikit-learn modelini sıfırdan eğitin, ister buluta mevcut bir modeli getiriyor olun, elastik bulut bilgi işlem kaynaklarını kullanarak açık kaynak eğitim işlerini ölçeklendirmek için Azure Machine Learning'i kullanabilirsiniz. Azure Machine Learning ile üretim sınıfı modeller oluşturabilir, dağıtabilir, sürümleyebilir ve izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu aşağıdaki ortamlardan herhangi birinde çalıştırın:
 - Azure Machine Learning bilgi işlem örneği - indirme veya yükleme ye gerek yok

    - [Öğreticiyi tamamlayın:](tutorial-1st-experiment-sdk-setup.md) SDK ve örnek depoyla önceden yüklenmiş özel bir dizüstü bilgisayar sunucusu oluşturmak için kurulum ortamı ve çalışma alanı.
    - Not defteri sunucusundaki örnekler eğitim klasöründe, bu diziniçin gezinerek tamamlanmış ve genişletilmiş bir not defteri bulun: **nasıl kullanılır-azureml > ml-çerçeveler > scikit-learn > eğitimi > tren-hiperparametre-tune-deploy-with-sklearn** klasörü.

 - Kendi Jupyter Notebook sunucunuz

    - [Azure Machine Learning SDK'yı yükleyin.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Çalışma alanı yapılandırma dosyası oluşturun.](how-to-configure-environment.md#workspace)
    - Veri kümesini ve örnek komut dosyası dosyasını indirin 
        - [iris veri seti](https://archive.ics.uci.edu/ml/datasets/iris)
        - [train_iris.py](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn)
    - Ayrıca GitHub örnekleri sayfasında bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb) de bulabilirsiniz. Not defteri, akıllı hiperparametre atonlama ve birincil ölçümlere göre en iyi modeli alma kapsayan genişletilmiş bir bölüm içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölümde, gerekli python paketlerini yükleyerek, bir çalışma alanını başlatma, deneme oluşturma ve eğitim verilerini ve eğitim komut dosyalarını yükleyerek eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıklarını içe aktarın.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning çalışma alanı,](concept-workspace.md) hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm eserlerle çalışmak için merkezi bir yer sağlar. Python SDK'da, bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapılarına erişebilirsiniz.

[Önkoşullar bölümünde](#prerequisites)oluşturulan `config.json` dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="create-a-machine-learning-experiment"></a>Makine öğrenmesi denemesi oluşturma

Eğitim komut dosyalarınızı tutmak için bir deneme ve klasör oluşturun. Bu örnekte, "sklearn-iris" adlı bir deneme oluşturun.

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="prepare-training-script"></a>Eğitim komut dosyası hazırlama

Bu öğreticide, eğitim komut dosyası **train_iris.py** zaten sizin için sağlanmıştır. Uygulamada, herhangi bir özel eğitim komut dosyasını olduğu gibi alabilir ve kodunuzu değiştirmek zorunda kalmadan Azure ML ile çalıştırabilirsiniz.

Azure ML izleme ve ölçüm özelliklerini kullanmak için, eğitim komut dosyanıza az miktarda Azure ML kodu ekleyin.  Eğitim komut dosyası **train_iris.py,** komut dosyasıiçindeki `Run` nesneyi kullanarak Azure ML çalıştırınıza bazı ölçümlerin nasıl günlüğe kaydedilebildiğini gösterir.

Sağlanan eğitim komut dosyası `iris = datasets.load_iris()` işlevden örnek verileri kullanır.  Kendi verileriniz için, eğitim sırasında verileri kullanılabilir hale getirmek için [veri kümesini ve komut dosyalarını yükleme](how-to-train-keras.md#data-upload) gibi adımları kullanmanız gerekebilir.

Eğitim komut dosyasını **train_iris.py'yi** proje dizininize kopyalayın.

```
import shutil
shutil.copy('./train_iris.py', project_folder)
```

## <a name="create-or-get-a-compute-target"></a>Bir işlem hedefi oluşturma veya alma

Scikit-learn işinizin devam etmesi için bir bilgi işlem hedefi oluşturun. Scikit-learn yalnızca tek düğüm, CPU bilgi işlem destekler.

Aşağıdaki kod, uzaktan eğitim bilgi işlem kaynağınız için Azure Machine Learning yönetilen bir bilgi işlem (AmlCompute) oluşturur. AmlCompute oluşturulması yaklaşık 5 dakika sürer. Bu ada sahip AmlCompute zaten çalışma alanınızdaysa, bu kod oluşturma işlemini atlar.

```Python
cluster_name = "cpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Bilgi işlem hedefleri hakkında daha fazla bilgi için, [bilgi işlem hedef](concept-compute-target.md) makalesinin ne olduğunu görün.

## <a name="create-a-scikit-learn-estimator"></a>Bir scikit-learn tahmincisi oluşturma

[Scikit-learn tahmincisi,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn?view=azure-ml-py) bir bilgi işlem hedefi üzerinde scikit-learn eğitim işi başlatmanın basit bir yolunu sağlar. Tek düğümlü CPU [`SKLearn`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) eğitimini desteklemek için kullanılabilen sınıf aracılığıyla uygulanır.

Eğitim komut dosyanızın çalışması için ek pip veya conda paketlerine ihtiyacı varsa, paketleri, adlarını `pip_packages` `conda_packages` ve bağımsız değişkenleri aracılığıyla geçirerek ortaya çıkan docker görüntüsüne yükletebilirsiniz.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    '--kernel': 'linear',
    '--penalty': 1.0,
}

estimator = SKLearn(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train_iris.py'
                    pip_packages=['joblib']
                   )
```


Python ortamınızı özelleştirme hakkında daha fazla bilgi için [eğitim ve dağıtım ortamları oluşturma ve yönetme'ye](how-to-use-environments.md)bakın. 

## <a name="submit-a-run"></a>Çalıştırma gönder

[Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) nesnesi, iş çalışırken ve tamamlandıktan sonra çalışma geçmişine arabirimi sağlar.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütülderken, aşağıdaki aşamalardan geçer:

- **Hazırlama**: TensorFlow tahmincisine göre docker görüntüsü oluşturulur. Görüntü çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonraki çalıştırmalar için önbelleğe alındı. Günlükler de çalışma geçmişine akışı ve ilerleme izlemek için görüntülenebilir.

- **Ölçeklendirme**: Toplu AI kümesi, çalıştırmayı yürütmek için şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeye çalışır.

- **Çalışma**: Komut dosyası klasöründeki tüm komut dosyaları bilgi işlem hedefine yüklenir, veri depolarına monte edilir veya kopyalanır ve entry_script yürütülür. Stdout ve ./logs klasöründen çıkan çıktılar çalışma geçmişine aktarılır ve çalıştırmayı izlemek için kullanılabilir.

- **Post-Processing**: Çalıştırın ./çıktılar klasörü çalışma geçmişine kopyalanır.

## <a name="save-and-register-the-model"></a>Modeli kaydetme ve kaydetme

Modeli eğittikten sonra, modelini kaydedebilir ve çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimini ve dağıtımı](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümünüzü yapmanızı sağlar.

Modeli kaydetmek için eğitim komut dosyanıza aşağıdaki kodu train_iris.py ekleyin. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Aşağıdaki kodla modeli çalışma alanınıza kaydedin. Parametreleri `model_framework`belirterek `model_framework_version`, `resource_configuration`ve , no-code modeli dağıtım kullanılabilir olur. Bu, modelinizi kayıtlı modelden bir web hizmeti olarak [`ResourceConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.resource_configuration.resourceconfiguration?view=azure-ml-py) doğrudan dağıtmanıza olanak tanır ve nesne web hizmetinin bilgi işlem kaynağını tanımlar.

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

Yeni kaydettiğiniz model, eğitim için hangi tahminciden kullandığınıza bakılmaksızın Azure Machine Learning'deki diğer kayıtlı modelle aynı şekilde dağıtılabilir. Dağıtım nasıl yapılacağını niçin kaydeden modellerle ilgili bir bölüm içerir, ancak zaten kayıtlı bir modeliniz olduğundan dağıtım için [doğrudan bir işlem hedefi oluşturmayı](how-to-deploy-and-where.md#choose-a-compute-target) atlayabilirsiniz.

### <a name="preview-no-code-model-deployment"></a>(Önizleme) Kodsuz model dağıtımı

Geleneksel dağıtım rotası yerine, scikit-learn için kodsuz dağıtım özelliğini (önizleme) de kullanabilirsiniz. Kodsuz model dağıtımı, tüm yerleşik scikit-learn model türleri için desteklenir. Modelinizi yukarıda gösterildiği gibi `model_framework`, `model_framework_version`, `resource_configuration` ve parametrelerle kaydederek, modelinizi dağıtmak için [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) statik işlevi kullanabilirsiniz.

```python
web_service = Model.deploy(ws, "scikit-learn-service", [model])
```

NOT: Bu bağımlılıklar önceden yapılmış scikit-learn çıkarım kabına dahildir.

```yaml
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
```

Tam [nasıl yapılsa,](how-to-deploy-and-where.md) Azure Machine Learning'de dağıtımı daha derinlemesine kapsar.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir scikit-learn modelini eğittiniz ve kaydettiniz ve dağıtım seçeneklerini öğrendiniz. Azure Machine Learning hakkında daha fazla bilgi edinmek için diğer makalelere bakın.

* [Eğitim sırasında koşu ölçümlerini izleme](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Azure'da dağıtılmış derin öğrenim eğitimi için referans mimarisi](/azure/architecture/reference-architectures/ai/training-deep-learning)
