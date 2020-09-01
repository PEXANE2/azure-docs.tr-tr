---
title: Bir işlem hedefine eğitim çalıştırması gönderme
titleSuffix: Azure Machine Learning
description: Çeşitli eğitim ortamlarında (işlem hedefleri) makine öğrenimi modelinizi eğitme. Eğitim ortamları arasında kolayca geçiş yapabilirsiniz. Eğitimi yerel olarak başlatın. Ölçeği ölçeklendirmeniz gerekiyorsa, bulut tabanlı bir işlem hedefine geçiş yapın.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 08/28/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: ca76ec5bef1d908ca3cea6ce0f58b1205c1676ca
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89144101"
---
# <a name="submit-a-training-run-to-a-compute-target"></a>Bir işlem hedefine eğitim çalıştırması gönderme

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, makine öğrenimi modelinizi eğitebilmeniz için çeşitli eğitim ortamlarını ([işlem hedefleri](concept-compute-target.md)) nasıl kullanacağınızı öğreneceksiniz.

Eğitim sırasında yerel bilgisayarınızda başlamak yaygındır ve daha sonra bu eğitim betiği farklı bir işlem hedefinde çalıştırılır. Azure Machine Learning, komut dosyanızı, eğitim betiğinizi değiştirmek zorunda kalmadan çeşitli işlem hedeflerinde çalıştırabilirsiniz.

Yapmanız gereken tek şey, bir **komut dosyası çalıştırma yapılandırması**içindeki her bir işlem hedefi için ortamı tanımlamaktır.  Daha sonra eğitim denemenizi farklı bir işlem hedefinde çalıştırmak istediğinizde, bu işlem için çalıştırma yapılandırmasını belirtin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin
* [Python için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md),`ws`
* İşlem hedefi, `my_compute_target` .  İle bir işlem hedefi oluşturun:
  * [Python SDK'sı](how-to-create-attach-compute-sdk.md) 
  * [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md)

## <a name="whats-a-script-run-configuration"></a><a name="whats-a-run-configuration"></a>Betik çalıştırma Yapılandırması nedir?

Eğitim denemenizi bir [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) nesnesi ile gönderebilirsiniz.  Bu nesne şunları içerir:

* **source_directory**: eğitim betiğinizi içeren kaynak dizin
* **betik**: eğitim betiğini tanımla
* **run_config**: [çalıştırma yapılandırması](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py), daha sonra eğitimin nerede olacağını tanımlar. İçinde, bilgi `run_config` işlem hedefini ve eğitim betiği çalıştırılırken kullanılacak ortamı belirtirsiniz.  

## <a name="whats-an-environment"></a>Ortam nedir?

Azure Machine Learning [ortamlar](concept-environments.md) , Machine Learning eğitiminin gerçekleştiği ortamın kapsüllenmesi. Bunlar, eğitim ve Puanlama betikleriniz etrafında Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirler. Bunlar ayrıca çalışma zamanlarını belirtir (Python, Spark veya Docker).  

Ortamları,  `run_config` içindeki nesnesinde belirtilir `ScriptRunConfig` .

## <a name="train-your-model"></a><a id="submit"></a>Modelinizi eğitme

Eğitim çalışması göndermek için kod deseninin her türlü bilgi işlem hedefi vardır:

1. Çalıştırmak için bir deneme oluşturun
1. Betiğin çalışacağı bir ortam oluşturun
1. İşlem hedefine ve ortamına başvuran bir betik çalıştırma yapılandırması oluşturma
1. Çalıştırmayı gönder
1. Çalıştırmanın tamamlanmasını bekleyin

İsterseniz şunları yapabilirsiniz:

* Denemesi, `Estimator` [ml modellerini tahmini ile eğitme](how-to-train-ml-models.md)bölümünde gösterildiği gibi bir nesneyle birlikte gönder.
* [Hiper parametre ayarlama](how-to-tune-hyperparameters.md)Için bir Hyperdrive çalıştırması gönderebilirsiniz.
* [Vs Code uzantısı](tutorial-train-deploy-image-classification-model-vscode.md#train-the-model)aracılığıyla bir deneme gönderir.

## <a name="create-an-experiment"></a>Deneme oluşturma

Çalışma alanınızda bir deneme oluşturun.

```python
from azureml.core import Experiment

experiment_name = 'my_experiment'

experiment = Experiment(workspace=ws, name=experiment_name)
```

## <a name="create-an-environment"></a>Ortam oluşturma

Seçkin ortamlar, Python paketlerinin koleksiyonlarını içerir ve çalışma alanınızda varsayılan olarak kullanılabilir. Bu ortamlar, çalışma hazırlığı maliyetini azaltan önbelleğe alınmış Docker görüntüleri tarafından desteklenir. Uzaktan işlem hedefi için, aşağıdaki popüler ortamlarından birini kullanarak şunları yapabilirsiniz:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
my_environment = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Ortamlar hakkında daha fazla bilgi ve Ayrıntılar için bkz. [Azure Machine Learning yazılım ortamları oluşturma & kullanma](how-to-use-environments.md).
  
### <a name="local-compute-target"></a>Yerel işlem hedefi

İşlem hedefinizin **yerel makineniz**olması durumunda, gerekli tüm paketlerin betiğin çalıştığı Python ortamında kullanılabilir olmasını sağlamaktan siz sorumlusunuz.  `python.user_managed_dependencies`Geçerli Python ortamınızı (veya belirttiğiniz yolda Python) kullanmak için kullanın.

```python
from azureml.core import Environment

# Editing a run configuration property on-fly.
my_environment = Environment("user-managed-env")

my_environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#my_environment.python.interpreter_path = '/home/johndoe/miniconda3/envs/myenv/bin/python'
```

## <a name="create-script-run-configuration"></a>Betik çalıştırma yapılandırması oluştur

Artık bir işlem hedefine ( `compute_target` ) ve ortamınıza () sahip olduğunuza göre `my_environment` , kendi dizininizde bulunan eğitim betiğinizi () çalıştıran bir betik çalıştırma yapılandırması oluşturun `train.py` `project_folder` :

```python
from azureml.core import ScriptRunConfig

script_run_config = ScriptRunConfig(source_directory=project_folder, script='train.py')

# Set compute target
script_run_config.run_config.target = my_compute_target

# Set environment.   If you don't do this, a default environment will be created.
script_run_config.run_config.environment = my_environment
```

Ayrıca, çalıştırmak için Framework 'ü ayarlamak isteyebilirsiniz.

* Bir HDI kümesi için:
    ```python
    src.run_config.framework = "pyspark"
    ```

* Uzak bir sanal makine için:
    ```python
    src.run_config.framework = "python"
    ```

## <a name="submit-the-experiment"></a>Denemeyi gönderme

```python
run = experiment.submit(config=script_run_config)
```

> [!IMPORTANT]
> Eğitim çalıştırmasını gönderdiğinizde, eğitim betiklerinizi içeren dizinin bir anlık görüntüsü oluşturulur ve işlem hedefine gönderilir. Ayrıca çalışma alanınızdaki denemenin bir parçası olarak da depolanır. Dosyaları değiştirir ve çalışmayı yeniden gönderirseniz yalnızca değiştirilen dosyalar karşıya yüklenir.
>
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]
> 
> Anlık görüntüler hakkında daha fazla bilgi için bkz. [anlık görüntüler](concept-azure-machine-learning-architecture.md#snapshots).


<a id="gitintegration"></a>

## <a name="git-tracking-and-integration"></a>Git izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Daha fazla bilgi için bkz. [Azure Machine Learning Için git tümleştirmesi](concept-train-model-git-integration.md).

## <a name="notebook-examples"></a>Not defteri örnekleri

Çeşitli işlem hedeflerine yönelik eğitim örnekleri için bu not defterlerine bakın:
* [Nasıl yapılır kullanımı-azureml/eğitim](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [Öğreticiler/img-Classification-part1-Training. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: bir modeli eğitme](tutorial-train-models-with-aml.md) bir modeli eğmek için yönetilen bir işlem hedefi kullanır.
* Daha iyi modeller oluşturmak için [hiper parametreleri verimli](how-to-tune-hyperparameters.md) bir şekilde ayarlamayı öğrenin.
* Eğitilen bir modelden sonra [modellerin nasıl ve nereye dağıtılacağını](how-to-deploy-and-where.md)öğrenin.
* [RunConfiguration sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) SDK başvurusunu görüntüleyin.
* [Azure sanal ağları ile Azure Machine Learning kullanma](how-to-enable-virtual-network.md)
