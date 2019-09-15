---
title: Scikit-öğrenme ile makine öğrenimi modellerini eğitme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning SKIT tahmin aracı sınıfını kullanarak scikit-kurumsal ölçekte eğitim komut dosyalarınızı nasıl çalıştıracağınızı öğrenin. Örnek betikler, ırikit-öğrenimi veri kümesini temel alan bir makine öğrenimi modeli oluşturmak için Iris çiçek görüntülerini sınıflandırır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 08/02/2019
ms.custom: seodec18
ms.openlocfilehash: 2b05ba7e4d38b596bdf76655fad0736425f8ce89
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002528"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning"></a>Derleme scikit-Azure Machine Learning uygun ölçekte modeller öğrenin

Bu makalede, Azure Machine Learning [SKIT tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) sınıfını kullanarak scikit-kurumsal ölçekte eğitim komut dosyalarınızı nasıl çalıştıracağınızı öğrenin. 

Bu makaledeki örnek betikler, ırikit-öğrenme için [Iris veri kümesini](https://archive.ics.uci.edu/ml/datasets/iris)temel alan bir makine öğrenimi modeli oluşturmak üzere Iris çiçek görüntülerini sınıflandırmak için kullanılır.

Bir makine öğrenimi scikit-bir modeli baştan sona öğreniyor olun veya var olan bir modeli buluta getiriyor, elastik bulut bilgi işlem kaynaklarını kullanarak açık kaynaklı eğitim işlerini ölçeklendirmek için Azure Machine Learning kullanabilirsiniz. Azure Machine Learning ile üretim sınıfı modellerini oluşturabilir, dağıtabilir, sürüm ve izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu şu ortamlardan birinde çalıştırın:
 - Azure Machine Learning Not defteri VM-indirme veya yükleme gerekli değil

    - [Öğreticiyi doldurun: SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu oluşturmak için ortamı ve çalışma alanını](tutorial-1st-experiment-sdk-setup.md) ayarlayın.
    - Not defteri sunucusundaki örnekler eğitim klasöründe, bu dizine giderek tamamlanmış ve genişletilmiş bir not defteri bulun: **nasıl kullanılır-azureml > eğitimi > eğitme-hiper parametre-ayarla-dağıt-with-sköğren** Folder.

 - Kendi Jupyter Notebook sunucunuz

    - [Azure Machine Learning SDK 'Sını yükler](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
    - [Bir çalışma alanı yapılandırma dosyası oluşturun](how-to-configure-environment.md#workspace).
    - Veri kümesini ve örnek betik dosyasını indirin 
        - [Iris veri kümesi](https://archive.ics.uci.edu/ml/datasets/iris)
        - [`train_iris.py`](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn)
    - Ayrıca, bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb) GitHub örnekleri sayfasından bulabilirsiniz. Not defteri, akıllı hiper parametre ayarlamayı kapsayan ve birincil ölçümler için en iyi modeli alan genişletilmiş bir bölüm içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölüm, gerekli Python paketlerini yükleyerek, bir çalışma alanı başlatarak, bir deneme oluşturarak ve eğitim verilerini ve eğitim betikleri karşıya yükleyerek Eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıklarını içeri aktarın.

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

[Azure Machine Learning çalışma alanı](concept-workspace.md) , hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Python SDK 'sında bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapıtlarına erişebilirsiniz.

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="create-a-machine-learning-experiment"></a>Machine Learning denemesi oluşturun

Eğitim betiklerinizi tutmak için bir deneme ve bir klasör oluşturun. Bu örnekte "sköğren-Iris" adlı bir deneme oluşturun.

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="upload-dataset-and-scripts"></a>Veri kümesini ve betikleri karşıya yükle

Veri [deposu](how-to-access-data.md) , verileri bir işlem hedefine bağlayarak veya kopyalayarak verilerin saklanabileceği ve erişebileceği bir yerdir. Her çalışma alanı varsayılan bir veri deposu sağlar. Eğitim sırasında kolayca erişilebilmesi için veri ve eğitim betikleri veri deposuna yükleyin.

1. Verileriniz için dizin oluşturun.

    ```Python
    os.makedirs('./data/iris', exist_ok=True)
    ```

1. Iris veri kümesini varsayılan veri deposuna yükleyin.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/iris', target_path='iris', overwrite=True, show_progress=True)
    ```

1. Scikit-öğrenme eğitim betiğini `train_iris.py`yükleyin.

    ```Python
    shutil.copy('./train_iris.py', project_folder)
    ```

## <a name="create-or-get-a-compute-target"></a>İşlem hedefi oluştur veya al

Scikit-üzerinde çalıştırılacak iş için bilgi işlem hedefi oluşturun. Scikit-yalnızca tek düğümlü, CPU bilgi işlem desteği.

Aşağıdaki kod, uzaktan eğitim işlem kaynağınız için Azure Machine Learning yönetilen bir işlem (AmlCompute) oluşturur. AmlCompute oluşturma işlemi yaklaşık 5 dakika sürer. Bu ada sahip AmlCompute zaten çalışma alanınızda varsa, bu kod oluşturma işlemini atlar.

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

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

## <a name="create-a-scikit-learn-estimator"></a>Bir scikit oluşturma-tahmin aracı

[Scikit-öğrenme tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn?view=azure-ml-py) , bir işlem hedefinde bir scikit-öğrenme eğitimi işi başlatmanın basit bir yolunu sunar. Tek düğümlü CPU eğitimini desteklemek [`SKLearn`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) için kullanılabilecek sınıfı aracılığıyla uygulanır.

Eğitim betiğinizin çalışması için ek PIP veya Conda paketleri gerekiyorsa, bu paketlerin adlarını `pip_packages` ve `conda_packages` bağımsız değişkenleri aracılığıyla geçirerek elde edilen Docker görüntüsüne yüklenmiş olmasını sağlayabilirsiniz.

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

## <a name="submit-a-run"></a>Bir çalıştırma gönder

[Run nesnesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) , iş çalışırken ve tamamlandıktan sonra çalışma geçmişi için arabirim sağlar.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütüldüğü için aşağıdaki aşamalardan geçer:

- **Hazırlanıyor**: Bir Docker görüntüsü, TensorFlow Estimator öğesine göre oluşturulur. Görüntü, çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonra çalışacak şekilde önbelleğe alınır. Günlükler, çalıştırma geçmişine de kaydedilir ve ilerlemeyi izlemek için görüntülenebilir.

- **Ölçeklendirme**: Batch AI kümesi, çalışmayı yürütmek için daha fazla düğüm, şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeyi dener.

- **Çalıştırma**: Betik klasöründeki tüm betikler işlem hedefine yüklenir, veri depoları bağlanır veya kopyalanır ve entry_script yürütülür. Stdout ve./logs klasörü çıkışları, çalıştırma geçmişine akışla kaydedilir ve çalıştırmayı izlemek için kullanılabilir.

- **Işlem sonrası**: Çalıştırmanın./çıktılar klasörü, çalışma geçmişine kopyalanır.

## <a name="save-and-register-the-model"></a>Modeli kaydetme ve kaydetme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilir ve kaydedebilirsiniz. Model kaydı, [model yönetimi ve dağıtımını](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümlerini oluşturmanıza imkan tanır.

Modeli kaydetmek için, train_iris. Kopyala komut dosyasına aşağıdaki kodu ekleyin. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Aşağıdaki kodla modeli çalışma alanınıza kaydedin.

```Python
model = run.register_model(model_name='sklearn-iris', model_path='model.joblib')
```

## <a name="next-steps"></a>Sonraki adımlar


Bu makalede, Azure Machine Learning bir keras modeli eğitildiniz ve kaydettiniz. Modeli dağıtmayı öğrenmek için model dağıtım makalemize devam edin.

> [!div class="nextstepaction"]
> [Modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)
* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
* [Azure 'da dağıtılmış derin öğrenme eğitimi için başvuru mimarisi](/azure/architecture/reference-architectures/ai/training-deep-learning)
