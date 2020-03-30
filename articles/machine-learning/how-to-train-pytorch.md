---
title: Derin öğrenme PyTorch modelleri tren
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'in PyTorch tahminci sınıfını kullanarak PyTorch eğitim komut dosyalarınızı kurumsal ölçekte nasıl çalıştırılacınız öğrenin.  Örnek komut dosyaları PyTorch transfer öğrenme öğretici dayalı derin bir öğrenme sinir ağı oluşturmak için tavuk ve hindi görüntüleri sınıflandırmak.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: peterlu
author: peterclu
ms.reviewer: peterlu
ms.date: 08/01/2019
ms.custom: seodec18
ms.openlocfilehash: 136ee197271fc659497c169e27a6399c3940c19e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834854"
---
# <a name="train-pytorch-deep-learning-models-at-scale-with-azure-machine-learning"></a>Azure Machine Learning ile Pytorch derin öğrenme modellerini ölçekte eğitin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning'in [PyTorch tahminci](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) sınıfını kullanarak [PyTorch](https://pytorch.org/) eğitim komut dosyalarınızı kurumsal ölçekte nasıl çalıştırılacanız gerektiğini öğrenin.  

Bu makalede örnek komut pyTorch transfer öğrenme [öğretici](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)dayalı derin bir öğrenme sinir ağı oluşturmak için tavuk ve hindi görüntüleri sınıflandırmak için kullanılır. 

İster yerden derin bir öğrenme PyTorch modelini eğitin, ister buluta mevcut bir model getiriyor olun, elastik bulut bilgi işlem kaynaklarını kullanarak açık kaynak eğitim işlerini ölçeklendirmek için Azure Machine Learning'i kullanabilirsiniz. Azure Machine Learning ile üretim sınıfı modeller oluşturabilir, dağıtabilir, sürümleyebilir ve izleyebilirsiniz. 

[Derin öğrenme vs makine öğrenimi](concept-deep-learning-vs-machine-learning.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu aşağıdaki ortamlardan herhangi birinde çalıştırın:

- Azure Machine Learning bilgi işlem örneği - indirme veya yükleme ye gerek yok

    - [Öğreticiyi tamamlayın:](tutorial-1st-experiment-sdk-setup.md) SDK ve örnek depoyla önceden yüklenmiş özel bir dizüstü bilgisayar sunucusu oluşturmak için kurulum ortamı ve çalışma alanı.
    - Not defteri sunucusundaki derin öğrenme klasöründe, bu dizine giderek tamamlanmış ve genişletilmiş bir not defteri bulun: **nasıl kullanılır-azureml > eğitim-ile-derin öğrenme >-hiperparametre-tune-deploy-with-pytorch** klasörü. 
 
 - Kendi Jupyter Notebook sunucunuz

    - [Azure Machine Learning SDK'yı yükleyin.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Çalışma alanı yapılandırma dosyası oluşturun.](how-to-configure-environment.md#workspace)
    - [Örnek komut dosyası dosyalarını indirin](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch/deployment/train-hyperparameter-tune-deploy-with-pytorch)`pytorch_train.py`
     
    Ayrıca GitHub örnekleri sayfasında bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/pytorch/deployment/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb) de bulabilirsiniz. Not defteri, akıllı hiperparametre atonlarını, model dağıtımını ve dizüstü bilgisayar widget'larını kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölümde, gerekli python paketlerini yükleyerek, bir çalışma alanını başlatma, deneme oluşturma ve eğitim verilerini ve eğitim komut dosyalarını yükleyerek eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıklarını içe aktarın.

```Python
import os
import shutil

from azureml.core.workspace import Workspace
from azureml.core import Experiment

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
from azureml.train.dnn import PyTorch
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning çalışma alanı,](concept-workspace.md) hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm eserlerle çalışmak için merkezi bir yer sağlar. Python SDK'da, bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapılarına erişebilirsiniz.

[Önkoşullar bölümünde](#prerequisites)oluşturulan `config.json` dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="create-a-deep-learning-experiment"></a>Derin öğrenme deneyi oluşturma

Eğitim komut dosyalarınızı tutmak için bir deneme ve klasör oluşturun. Bu örnekte, "pytorch-kuşlar" adlı bir deney oluşturun.

```Python
project_folder = './pytorch-birds'
os.makedirs(project_folder, exist_ok=True)

experiment_name = 'pytorch-birds'
experiment = Experiment(ws, name=experiment_name)
```

### <a name="get-the-data"></a>Verileri alma

Veri seti, her bir sınıf için 100 doğrulama görüntüsü içeren hindi ve tavuklar için yaklaşık 120 eğitim görüntüsünden oluşur. Eğitim komut `pytorch_train.py`dosyamızın bir parçası olarak veri kümesini indirip ayıklayabiliyoruz. Görüntüler [Open Images v5 Dataset'in](https://storage.googleapis.com/openimages/web/index.html)bir alt kümesidir.

### <a name="prepare-training-scripts"></a>Eğitim komut dosyaları hazırlama

Bu öğreticide, eğitim `pytorch_train.py`komut dosyası, zaten sağlanır. Uygulamada, azure machine learning ile istediğiniz gibi herhangi bir özel eğitim komut dosyası alabilir ve çalıştırabilirsiniz.

Pytorch eğitim komut `pytorch_train.py`dosyasını yükleyin.

```Python
shutil.copy('pytorch_train.py', project_folder)
```

Ancak, Azure Machine Learning izleme ve ölçüm özelliklerini kullanmak istiyorsanız, eğitim komut dosyanızın içine küçük bir miktar kodu eklemeniz gerekir. Ölçüm izleme örnekleri ne `pytorch_train.py`bilebilir.

## <a name="create-a-compute-target"></a>İşlem hedefi oluşturma

PyTorch işinizin çalışması için bir işlem hedefi oluşturun. Bu örnekte, GPU etkin leştirilmiş bir Azure Machine Learning bilgi işlem kümesi oluşturun.

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Bilgi işlem hedefleri hakkında daha fazla bilgi için, [bilgi işlem hedef](concept-compute-target.md) makalesinin ne olduğunu görün.

## <a name="create-a-pytorch-estimator"></a>PyTorch tahmincisi oluşturma

[PyTorch tahmincisi](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) bir hesaplama hedef bir PyTorch eğitim iş başlatma basit bir yol sağlar.

PyTorch tahmincisi, herhangi bir [`estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) çerçeveyi desteklemek için kullanılabilecek genel sınıf aracılığıyla uygulanır. Genel tahminciyi kullanarak eğitim modelleri hakkında daha fazla bilgi [için, tahminci kullanarak Azure Machine Learning'e sahip tren modellerine](how-to-train-ml-models.md) bakın

Eğitim komut dosyanızın çalışması için ek pip veya conda paketlerine ihtiyacı varsa, paketleri, adlarını `pip_packages` `conda_packages` ve bağımsız değişkenleri aracılığıyla geçirerek ortaya çıkan docker görüntüsüne yükletebilirsiniz.

```Python
script_params = {
    '--num_epochs': 30,
    '--output_dir': './outputs'
}

estimator = PyTorch(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='pytorch_train.py',
                    use_gpu=True,
                    pip_packages=['pillow==5.4.1'])
```

Python ortamınızı özelleştirme hakkında daha fazla bilgi için [eğitim ve dağıtım ortamları oluşturma ve yönetme'ye](how-to-use-environments.md)bakın.

## <a name="submit-a-run"></a>Çalıştırma gönder

[Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) nesnesi, iş çalışırken ve tamamlandıktan sonra çalışma geçmişine arabirimi sağlar.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Run yürütülderken, aşağıdaki aşamalardan geçer:

- **Hazırlama**: PyTorch tahmincisine göre docker görüntüsü oluşturulur. Görüntü çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonraki çalıştırmalar için önbelleğe alındı. Günlükler de çalışma geçmişine akışı ve ilerleme izlemek için görüntülenebilir.

- **Ölçeklendirme**: Toplu AI kümesi, çalıştırmayı yürütmek için şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeye çalışır.

- **Çalışma**: Komut dosyası klasöründeki tüm komut dosyaları bilgi işlem hedefine yüklenir, veri depolarına monte edilir veya kopyalanır ve entry_script yürütülür. Stdout ve ./logs klasöründen çıkan çıktılar çalışma geçmişine aktarılır ve çalıştırmayı izlemek için kullanılabilir.

- **Post-Processing**: Çalıştırın ./çıktılar klasörü çalışma geçmişine kopyalanır.

## <a name="register-or-download-a-model"></a>Bir model kaydetme veya indirme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimini ve dağıtımı](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümünüzü yapmanızı sağlar.

```Python
model = run.register_model(model_name='pt-dnn', model_path='outputs/')
```

> [!TIP]
> Az önce kaydettiğiniz model, eğitim için hangi tahminciden kullandığınıza bakılmaksızın Azure Machine Learning'deki diğer kayıtlı modelle aynı şekilde dağıtılır. Dağıtım nasıl yapılacağını niçin kaydeden modellerle ilgili bir bölüm içerir, ancak zaten kayıtlı bir modeliniz olduğundan dağıtım için [doğrudan bir işlem hedefi oluşturmayı](how-to-deploy-and-where.md#choose-a-compute-target) atlayabilirsiniz.

Ayrıca Çalıştır nesnesini kullanarak modelin yerel bir kopyasını indirebilirsiniz. Eğitim komut `pytorch_train.py`dosyasında, PyTorch kaydet nesnesi modeli yerel bir klasöre (işlem hedefine yerel) devam eder. Bir kopyasını indirmek için Çalıştır nesnesini kullanabilirsiniz.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Dağıtılmış eğitim

Tahminci, [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) CPU ve GPU kümeleri arasında dağıtılmış eğitimi de destekler. Dağıtılmış PyTorch işlerini kolayca çalıştırabilirsiniz ve Azure Machine Learning sizin için orkestrasyonu yönetir.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) açık kaynak kodludur ve uber tarafından geliştirilen dağıtılmış eğitim için çerçeveyi azaltır. Bu dağıtılan GPU PyTorch işleri için kolay bir yol sunuyor.

Horovod kullanmak için, [`MpiConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) PyTorch `distributed_training` oluşturucuparametre için bir nesne belirtin. Bu parametre, eğitim komut dosyanızda kullanmanız için Horovod kitaplığı yüklü olmasını sağlar.


```Python
from azureml.train.dnn import PyTorch

estimator= PyTorch(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```
Horovod ve bağımlılıkları sizin için yüklenir, böylece aşağıdaki gibi `train.py` eğitim komut dosyasında içe aktarabilirsiniz:

```Python
import torch
import horovod
```
## <a name="export-to-onnx"></a>ONNX'e dışa aktarma

[ONNX Runtime](concept-onnx.md)ile çıkarımları optimize etmek için, eğitimli PyTorch modelinizi ONNX biçimine dönüştürün. Çıkarım veya model puanlama, dağıtılan modelin tahmin için kullanıldığı aşamadır, en yaygın olarak üretim verilerinde. Örnek için [öğreticiye](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Machine Learning'de PyTorch'u kullanarak derin bir öğrenme, sinir ağı eğitimi almış ve kaydedebilirsiniz. Bir modeli nasıl dağıtılacayacağını öğrenmek için, model deployment makalemize devam edin.

> [!div class="nextstepaction"]
> [Modelleri nasıl ve nerede dağıtılır](how-to-deploy-and-where.md)
* [Eğitim sırasında koşu ölçümlerini izleme](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitimli bir model dağıtma](how-to-deploy-and-where.md)
* [Azure'da dağıtılmış derin öğrenim eğitimi için referans mimarisi](/azure/architecture/reference-architectures/ai/training-deep-learning)

