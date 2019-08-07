---
title: PyTorch ile derin öğrenme sinir ağını eğitme
titleSuffix: Azure Machine Learning service
description: Pytorch eğitim betiklerinizi, Azure Machine Learning pytorch tahmin aracı sınıfını kullanarak kurumsal ölçekte çalıştırmayı öğrenin.  Örnek betikler,, PyTorch 'ın aktarım öğrenimi öğreticisini temel alarak derin bir öğrenme sinir ağı oluşturmak için tavuk ve Türkiye görüntülerini sınıflandırır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 08/01/2019
ms.custom: seodec18
ms.openlocfilehash: 99217106c456adcc338138190be2060b0c9a195b
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68772683"
---
# <a name="train-pytorch-deep-learning-models-at-scale-with-azure-machine-learning"></a>Pytorch derin öğrenme modellerini Azure Machine Learning ölçeklendirirken eğitme

Bu makalede, Azure Machine Learning [pytorch tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) sınıfını kullanarak [pytorch](https://pytorch.org/) eğitim betiklerinizi kurumsal ölçekte çalıştırmayı öğrenin.  

Bu makaledeki örnek betikler, PyTorch 'ın aktarım öğrenimi [öğreticisini](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)temel alan derin bir öğrenme sinir ağı oluşturmak için tavuk ve Türkiye görüntülerini sınıflandırmak üzere kullanılır. 

Derin bir öğrenme PyTorch modelini baştan sona eğiyor veya mevcut bir modeli buluta çıkarksanız, elastik bulut işlem kaynakları kullanarak açık kaynaklı eğitim işlerini ölçeklendirmek için Azure Machine Learning kullanabilirsiniz. Azure Machine Learning ile üretim sınıfı modellerini oluşturabilir, dağıtabilir, sürüm ve izleyebilirsiniz. 

[Derin öğrenme ve makine öğrenimi](concept-deep-learning-vs-machine-learning.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu şu ortamlardan birinde çalıştırın:

 - Azure Machine Learning Not defteri VM-indirme veya yükleme gerekli değil

    - [Öğreticiyi doldurun: SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu oluşturmak için ortamı ve çalışma alanını](tutorial-1st-experiment-sdk-setup.md) ayarlayın.
    - Not defteri sunucusundaki örnekler derin öğrenimi klasöründe, bu dizine giderek tamamlanmış ve genişletilmiş bir not defteri bulun: **kullanımı nasıl kullanılır-azureml > eğitimi-derin öğrenme > tren-hyperparameter-ayarla-dağıt--pytorch** klasörü. 
 
 - Kendi Jupyter Notebook sunucunuz

    - [Python için Azure Machine Learning SDK 'sını yükler](setup-create-workspace.md#sdk)
    - [Çalışma alanı yapılandırma dosyası oluşturma](setup-create-workspace.md#write-a-configuration-file)
    - [Örnek betik dosyalarını indirin](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)`pytorch_train.py`
     
    Ayrıca, bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch/train-hyperparameter-tune-deploy-with-pytorch.ipynb) GitHub örnekleri sayfasından bulabilirsiniz. Not defteri, akıllı hiper parametre ayarlamayı, model dağıtımını ve Not defteri pencere öğelerini kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölüm, gerekli Python paketlerini yükleyerek, bir çalışma alanı başlatarak, bir deneme oluşturarak ve eğitim verilerini ve eğitim betikleri karşıya yükleyerek Eğitim denemesini ayarlar.

### <a name="import-packages"></a>Paketleri içeri aktarma

İlk olarak, gerekli Python kitaplıklarını içeri aktarın.

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

[Azure Machine Learning hizmet çalışma alanı](concept-workspace.md) , hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm yapıtlarla çalışmak için merkezi bir yer sağlar. Python SDK 'sında bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapıtlarına erişebilirsiniz.

`config.json` [Önkoşullar bölümünde](#prerequisites)oluşturulan dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="create-a-deep-learning-experiment"></a>Derin öğrenme denemesi oluşturun

Eğitim betiklerinizi tutmak için bir deneme ve bir klasör oluşturun. Bu örnekte, "pytorch-kuşlar" adlı bir deneme oluşturun.

```Python
project_folder = './pytorch-birds'
os.makedirs(project_folder, exist_ok=True)

experiment_name = 'pytorch-birds'
experiment = Experiment(ws, name=experiment_name)
```

### <a name="get-the-data"></a>Verileri alma

Veri kümesi, her bir sınıf için 100 doğrulama görüntüleriyle birlikte, her biri Turkey ve tavkens için 120 eğitim görüntülerinden oluşur. Veri kümesini eğitim betiğimizin `pytorch_train.py`bir parçası olarak indirecek ve ayıklayacağız. Görüntüler, [Açık görüntüler v5 veri kümesinin](https://storage.googleapis.com/openimages/web/index.html)bir alt kümesidir.

### <a name="prepare-training-scripts"></a>Eğitim betikleri hazırlama

Bu öğreticide, eğitim betiği `pytorch_train.py`zaten sağlanmış. Uygulamada, tüm özel eğitim komut dosyalarını olduğu gibi alabilir ve Azure Machine Learning hizmeti ile çalıştırabilirsiniz.

Pytorch eğitim betiğini `pytorch_train.py`yükleyin.

```Python
shutil.copy('pytorch_train.py', project_folder)
```

Ancak Azure Machine Learning hizmeti izleme ve ölçüm yeteneklerini kullanmak istiyorsanız, eğitim betiğinizin içine küçük bir tutar kodu eklemeniz gerekir. Ölçüm izleme örnekleri içinde `pytorch_train.py`bulunabilir.

## <a name="create-a-compute-target"></a>İşlem hedefi oluşturmak

PyTorch işinizin üzerinde çalışacağı bir işlem hedefi oluşturun. Bu örnekte, GPU özellikli Azure Machine Learning işlem kümesi oluşturun.

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

İşlem hedefleri hakkında daha fazla bilgi için bkz. [işlem hedefi nedir](concept-compute-target.md) makalesi.

## <a name="create-a-pytorch-estimator"></a>Pytorch tahmin aracı oluşturma

[Pytorch tahmin aracı](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) , bir bilgi işlem hedefinde bir pytorch eğitim işi başlatmanın basit bir yolunu sağlar.

Pytorch tahmin aracı, herhangi bir çerçeveyi desteklemek için [`estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) kullanılabilen genel sınıf aracılığıyla uygulanır. Genel tahmin aracı kullanan eğitim modelleri hakkında daha fazla bilgi için bkz. [tahmin aracı kullanarak Azure Machine Learning modelleri eğitme](how-to-train-ml-models.md)

Eğitim betiğinizin çalışması için ek PIP veya Conda paketleri gerekiyorsa, bu paketlerin adlarını `pip_packages` ve `conda_packages` bağımsız değişkenleri aracılığıyla geçirerek elde edilen Docker görüntüsüne yüklenmiş olmasını sağlayabilirsiniz.

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

## <a name="submit-a-run"></a>Bir çalıştırma gönder

[Run nesnesi](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) , iş çalışırken ve tamamlandıktan sonra çalışma geçmişi için arabirim sağlar.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Çalıştırma yürütüldüğü için aşağıdaki aşamalardan geçer:

- **Hazırlanıyor**: PyTorch Estimator 'a göre bir Docker görüntüsü oluşturulur. Görüntü, çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonra çalışacak şekilde önbelleğe alınır. Günlükler, çalıştırma geçmişine de kaydedilir ve ilerlemeyi izlemek için görüntülenebilir.

- **Ölçeklendirme**: Batch AI kümesi, çalışmayı yürütmek için daha fazla düğüm, şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeyi dener.

- **Çalıştırma**: Betik klasöründeki tüm betikler işlem hedefine yüklenir, veri depoları bağlanır veya kopyalanır ve entry_script yürütülür. Stdout ve./logs klasörü çıkışları, çalıştırma geçmişine akışla kaydedilir ve çalıştırmayı izlemek için kullanılabilir.

- **Işlem sonrası**: Çalıştırmanın./çıktılar klasörü, çalışma geçmişine kopyalanır.

## <a name="register-or-download-a-model"></a>Bir modeli kaydetme veya indirme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimi ve dağıtımını](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümlerini oluşturmanıza imkan tanır.

```Python
model = run.register_model(model_name='pt-dnn', model_path='outputs/')
```

Ayrıca, Çalıştır nesnesini kullanarak modelin yerel bir kopyasını indirebilirsiniz. Eğitim betiğinde `pytorch_train.py`, pytorch Save nesnesi modeli yerel bir klasöre (yerel olarak işlem hedefine) devam ettirir. Bir kopyasını indirmek için Run nesnesini kullanabilirsiniz.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Dağıtılmış eğitimi

[`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) Tahmin aracı Ayrıca CPU ve GPU kümelerinde dağıtılmış eğitimi destekler. Dağıtılmış PyTorch işlerini kolayca çalıştırabilir ve Azure Machine Learning hizmeti düzenlemeyi sizin için yönetecektir.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) , bir açık kaynak, hepsi Uber tarafından geliştirilen dağıtılmış eğitimin çerçevesini azaltır. Dağıtılmış GPU PyTorch işlerinin kolay bir yolunu sunar.

Horovod 'yi kullanmak için, pytorch [`MpiConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) oluşturucusunda `distributed_training` parametresi için bir nesne belirtin. Bu parametre, eğitim betiğinizdeki kullanabileceğiniz Horovod kitaplığının yüklenmesini sağlar.


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
Horovod ve bağımlılıkları sizin için yüklenecek, bu sayede eğitim betiğinizdeki `train.py` şu şekilde içeri aktarabilirsiniz:

```Python
import torch
import horovod
```
## <a name="export-to-onnx"></a>ONNX 'e aktar

[Onnx çalışma zamanıyla](concept-onnx.md)çıkarımı iyileştirmek için, eğitimli PyTorch MODELINIZI onnx biçimine dönüştürün. Çıkarım veya model Puanlama, dağıtılan modelin tahmin için en yaygın olarak üretim verilerinde kullanıldığı aşamadır. Örnek için [öğreticiye](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Machine Learning hizmetinde PyTorch kullanarak derin bir öğrenme, sinir ağı eğitildiniz ve kaydettiniz. Modeli dağıtmayı öğrenmek için model dağıtım makalemize devam edin.

> [!div class="nextstepaction"]
> [Modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)
* [İzleme ölçümlerini eğitim sırasında çalıştırın](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Eğitilen model dağıtma](how-to-deploy-and-where.md)
