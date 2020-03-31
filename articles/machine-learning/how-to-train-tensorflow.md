---
title: TensorFlow modelini eğitin ve dağıtın
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'i kullanarak TensorFlow eğitim komut dosyalarını ölçekte nasıl çalıştırılacıolarak çalıştırılamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 08/20/2019
ms.custom: seodec18
ms.openlocfilehash: 2bbd81f3858aa78b9e0e2d610c0fdb0a67816c8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78228304"
---
# <a name="build-a-tensorflow-deep-learning-model-at-scale-with-azure-machine-learning"></a>Azure Machine Learning ile ölçekte bir TensorFlow derin öğrenme modeli oluşturun
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning'in [Tensor Flow tahminci](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) sini kullanarak [TensorFlow](https://www.tensorflow.org/overview) eğitim komut dosyalarınızı ölçekte nasıl çalıştırabileceğinizi gösterir. Bu örnek, derin bir sinir ağı (DNN) kullanarak el yazısı rakamları sınıflandırmak için bir TensorFlow modelini eğiter ve kaydeder.

İster sıfırdan bir TensorFlow modeli geliştiriyor olun ister [buluta varolan](how-to-deploy-existing-model.md) bir modeli getiriyor olun, üretim sınıfı modeller oluşturmak, dağıtmak, sürümyapmak ve izlemek için açık kaynak eğitim işlerini ölçeklendirmek için Azure Machine Learning'i kullanabilirsiniz.

[Derin öğrenme vs makine öğrenimi](concept-deep-learning-vs-machine-learning.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

Bu kodu aşağıdaki ortamlardan herhangi birinde çalıştırın:

 - Azure Machine Learning bilgi işlem örneği - indirme veya yükleme ye gerek yok

     - [Öğreticiyi tamamlayın:](tutorial-1st-experiment-sdk-setup.md) SDK ve örnek depoyla önceden yüklenmiş özel bir dizüstü bilgisayar sunucusu oluşturmak için kurulum ortamı ve çalışma alanı.
    - Not defteri sunucusundaki derin öğrenme klasöründe, bu diziniçin gezinmek üzere tamamlanmış ve genişletilmiş bir not defteri bulun: **nasıl kullanılır-azureml > ml çerçeveleri > tensorflow > dağıtım > tren-hiperparametre-tune-deploy-with-tensorflow** klasörü. 
 
 - Kendi Jupyter Notebook sunucunuz

    - [Azure Machine Learning SDK'yı yükleyin.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Çalışma alanı yapılandırma dosyası oluşturun.](how-to-configure-environment.md#workspace)
    - [Örnek komut dosyası dosyalarını](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/tensorflow/deployment/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` indirin ve`utils.py`
     
    Ayrıca GitHub örnekleri sayfasında bu kılavuzun tamamlanmış bir [Jupyter Notebook sürümünü](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/tensorflow/deployment/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) de bulabilirsiniz. Not defteri, akıllı hiperparametre atonlarını, model dağıtımını ve dizüstü bilgisayar widget'larını kapsayan genişletilmiş bölümler içerir.

## <a name="set-up-the-experiment"></a>Denemeyi ayarlama

Bu bölümde, gerekli Python paketlerini yükleyerek, bir çalışma alanını başlatma, deneme oluşturma ve eğitim verilerini ve eğitim komut dosyalarını yükleyerek eğitim denemesini ayarlar.

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
from azureml.train.dnn import TensorFlow
```

### <a name="initialize-a-workspace"></a>Çalışma alanını başlatma

[Azure Machine Learning çalışma alanı,](concept-workspace.md) hizmet için en üst düzey kaynaktır. Oluşturduğunuz tüm eserlerle çalışmak için merkezi bir yer sağlar. Python SDK'da, bir [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) nesne oluşturarak çalışma alanı yapılarına erişebilirsiniz.

[Önkoşullar bölümünde](#prerequisites)oluşturulan `config.json` dosyadan bir çalışma alanı nesnesi oluşturun.

```Python
ws = Workspace.from_config()
```

### <a name="create-a-deep-learning-experiment"></a>Derin öğrenme deneyi oluşturma

Eğitim komut dosyalarınızı tutmak için bir deneme ve klasör oluşturun. Bu örnekte, "tf-mnist" adlı bir deneme oluşturun.

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="create-a-file-dataset"></a>Dosya veri kümesi oluşturma

Nesne, `FileDataset` çalışma alanı veri deponuzda veya genel url'lerinizde bir veya birden çok dosyaya başvurur. Dosyalar herhangi bir biçimde olabilir ve sınıf, dosyaları bilgisayarınıza indirme veya yükleme olanağı sağlar. Bir `FileDataset`, veri kaynağı konumuna bir başvuru oluşturursunuz. Veri kümesine herhangi bir dönüşüm uyguladıysanız, bunlar veri kümesinde de depolanır. Veriler varolan konumunda kalır, bu nedenle ek depolama maliyeti tahakkuk etmez. Daha [how-to](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets) fazla bilgi için `Dataset` paketteki nasıl yapılacağını kılavuzuna bakın.

```python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
dataset = Dataset.File.from_files(path=web_paths)
```

Veri `register()` kümesini çalışma alanınıza kaydetmek için yöntemi kullanın, böylece bunlar başkalarıyla paylaşılabilir, çeşitli denemeler arasında yeniden kullanılabilir ve eğitim komut dosyanızda adıyla anılabilen ler.

```python
dataset = dataset.register(workspace=ws,
                           name='mnist dataset',
                           description='training and test dataset',
                           create_new_version=True)

# list the files referenced by dataset
dataset.to_path()
```

## <a name="create-a-compute-target"></a>İşlem hedefi oluşturma

TensorFlow işinizin çalışması için bir bilgi işlem hedefi oluşturun. Bu örnekte, GPU etkin leştirilmiş bir Azure Machine Learning bilgi işlem kümesi oluşturun.

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

## <a name="create-a-tensorflow-estimator"></a>TensorFlow tahmincisi oluşturma

[TensorFlow tahmincisi,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) bir bilgi işlem hedefinde TensorFlow eğitim işini başlatmanın basit bir yolunu sağlar.

TensorFlow tahmincisi, herhangi bir [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) çerçeveyi desteklemek için kullanılabilecek genel sınıf aracılığıyla uygulanır. Genel tahminciyi kullanarak eğitim modelleri hakkında daha fazla bilgi [için, tahminci kullanarak Azure Machine Learning'e sahip tren modellerine](how-to-train-ml-models.md) bakın

Eğitim komut dosyanızın çalışması için ek pip veya conda paketlerine ihtiyacı varsa, paketleri, adlarını `pip_packages` `conda_packages` ve bağımsız değişkenleri aracılığıyla geçirerek ortaya çıkan Docker resmine yükletebilirsiniz.

```python
script_params = {
    '--data-folder': dataset.as_named_input('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='tf_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 use_gpu=True,
                 pip_packages=['azureml-dataprep[pandas,fuse]'])
```

> [!TIP]
> **Tensorflow 2.0** desteği Tensorflow tahminci sınıfına eklendi. Daha fazla bilgi için [blog gönderisine](https://azure.microsoft.com/blog/tensorflow-2-0-on-azure-fine-tuning-bert-for-question-tagging/) bakın.

Python ortamınızı özelleştirme hakkında daha fazla bilgi için [eğitim ve dağıtım ortamları oluşturma ve yönetme'ye](how-to-use-environments.md)bakın. 

## <a name="submit-a-run"></a>Çalıştırma gönder

[Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) nesnesi, iş çalışırken ve tamamlandıktan sonra çalışma geçmişine arabirimi sağlar.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Run yürütülderken, aşağıdaki aşamalardan geçer:

- **Hazırlama**: TensorFlow tahmincisine göre docker görüntüsü oluşturulur. Görüntü çalışma alanının kapsayıcı kayıt defterine yüklenir ve daha sonraki çalıştırmalar için önbelleğe alındı. Günlükler de çalışma geçmişine akışı ve ilerleme izlemek için görüntülenebilir.

- **Ölçeklendirme**: Toplu AI kümesi, çalıştırmayı yürütmek için şu anda kullanılabilir olandan daha fazla düğüm gerektiriyorsa, küme ölçeklendirmeye çalışır.

- **Çalışma**: Komut dosyası klasöründeki tüm komut dosyaları bilgi işlem hedefine yüklenir, veri depolarına monte edilir veya kopyalanır ve entry_script yürütülür. Stdout ve ./logs klasöründen çıkan çıktılar çalışma geçmişine aktarılır ve çalıştırmayı izlemek için kullanılabilir.

- **Post-Processing**: Çalıştırın ./çıktılar klasörü çalışma geçmişine kopyalanır.

## <a name="register-or-download-a-model"></a>Bir model kaydetme veya indirme

Modeli eğittikten sonra, çalışma alanınıza kaydedebilirsiniz. Model kaydı, [model yönetimini ve dağıtımı](concept-model-management-and-deployment.md)basitleştirmek için modellerinizi çalışma alanınızda depolamanızı ve sürümünüzü yapmanızı sağlar. Parametreleri `model_framework`belirterek `model_framework_version`, `resource_configuration`ve , no-code modeli dağıtım kullanılabilir olur. Bu, modelinizi kayıtlı modelden bir web hizmeti olarak `ResourceConfiguration` doğrudan dağıtmanıza olanak tanır ve nesne web hizmetinin bilgi işlem kaynağını tanımlar.

```Python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = run.register_model(model_name='tf-dnn-mnist', 
                           model_path='outputs/model',
                           model_framework=Model.Framework.TENSORFLOW,
                           model_framework_version='1.13.0',
                           resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5))
```

Ayrıca Çalıştır nesnesini kullanarak modelin yerel bir kopyasını indirebilirsiniz. Eğitim komut `mnist-tf.py`dosyasında, TensorFlow koruyucu nesnesi modeli yerel bir klasöre (işlem hedefine yerel) devam eder. Bir kopyasını indirmek için Çalıştır nesnesini kullanabilirsiniz.

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

Tahminci, [`TensorFlow`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) CPU ve GPU kümeleri arasında dağıtılmış eğitimi de destekler. Dağıtılmış TensorFlow işlerini kolayca çalıştırabilirsiniz ve Azure Machine Learning sizin için orkestrasyonu yönetir.

Azure Machine Learning, TensorFlow'da dağıtılmış eğitimin iki yöntemini destekler:

- [Horovod](https://github.com/uber/horovod) çerçevesini kullanarak [MPI tabanlı](https://www.open-mpi.org/) dağıtılmış eğitim
- Parametre sunucu yöntemini kullanarak yerel [dağıtılmış TensorFlow](https://www.tensorflow.org/deploy/distributed)

### <a name="horovod"></a>Horovod

[Horovod,](https://github.com/uber/horovod) Uber tarafından geliştirilen dağıtılmış eğitim için bir açık kaynak çerçevesidir. Dağıtılan GPU TensorFlow işleri için kolay bir yol sunar.

Horovod'u kullanmak [`MpiConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) için TensorFlow oluşturucudaki `distributed_training` parametre için bir nesne belirtin. Bu parametre, eğitim komut dosyanızda kullanmanız için Horovod kitaplığı yüklü olmasını sağlar.

```Python
from azureml.core.runconfig import MpiConfiguration
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True,
                      pip_packages=['azureml-dataprep[pandas,fuse]'])
```

### <a name="parameter-server"></a>Parametre sunucusu

Parametre sunucu modelini kullanan [yerel dağıtılmış TensorFlow'u](https://www.tensorflow.org/deploy/distributed)da çalıştırabilirsiniz. Bu yöntemde, parametre sunucuları ve işçiler kümesi arasında eğitim. Parametre sunucuları degradeleri toplarken, çalışanlar eğitim sırasında degradeleri hesaplarlar.

Parametre sunucu yöntemini kullanmak [`TensorflowConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.tensorflowconfiguration?view=azure-ml-py) için TensorFlow oluşturucudaki `distributed_training` parametre için bir nesne belirtin.

```Python
from azureml.train.dnn import TensorFlow

distributed_training = TensorflowConfiguration()
distributed_training.worker_count = 2

# Tensorflow constructor
tf_est= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=distributed_training,
                      use_gpu=True,
                      pip_packages=['azureml-dataprep[pandas,fuse]'])

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="define-cluster-specifications-in-tf_config"></a>Küme belirtimlerini 'TF_CONFIG' olarak tanımla

Ayrıca, Azure Machine Learning'in [`tf.train.ClusterSpec`](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec) `TF_CONFIG` ortamı sizin için belirlediği için kümenin ağ adreslerine ve bağlantı noktalarına da ihtiyacınız vardır.

Ortam `TF_CONFIG` değişkeni bir JSON dizesidir. Parametre sunucusu için değişkene bir örnek aşağıda verilmiştir:

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

TensorFlow'un yüksek [`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator) düzey API'si için `TF_CONFIG` TensorFlow değişkeni ayrıştı ve sizin için küme spektrumu oluşturur.

TensorFlow'un eğitim için alt düzey çekirdek API'leri için değişkeni ayrıştırın `TF_CONFIG` ve eğitim kodunuzda oluşturun. `tf.train.ClusterSpec`

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="deploy-a-tensorflow-model"></a>TensorFlow modelini dağıtma

Yeni kaydettiğiniz model, eğitim için hangi tahminciden kullandığınıza bakılmaksızın Azure Machine Learning'deki diğer kayıtlı modelle aynı şekilde dağıtılabilir. Dağıtım nasıl yapılacağını niçin kaydeden modellerle ilgili bir bölüm içerir, ancak zaten kayıtlı bir modeliniz olduğundan dağıtım için [doğrudan bir işlem hedefi oluşturmayı](how-to-deploy-and-where.md#choose-a-compute-target) atlayabilirsiniz.

## <a name="preview-no-code-model-deployment"></a>(Önizleme) Kodsuz model dağıtımı

Geleneksel dağıtım rotası yerine, Tensorflow için kodsuz dağıtım özelliğini (önizleme) de kullanabilirsiniz. Modelinizi yukarıda gösterildiği gibi `model_framework`, `model_framework_version`, `resource_configuration` ve parametrelerle kaydederek, modelinizi dağıtmak için `deploy()` statik işlevi kullanabilirsiniz.

```python
service = Model.deploy(ws, "tensorflow-web-service", [model])
```

Tam [nasıl yapılsa,](how-to-deploy-and-where.md) Azure Machine Learning'de dağıtımı daha derinlemesine kapsar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir TensorFlow modelini eğittiniz ve kaydettiniz ve dağıtım seçenekleri hakkında bilgi edindin. Azure Machine Learning hakkında daha fazla bilgi edinmek için diğer makalelere bakın.

* [Eğitim sırasında koşu ölçümlerini izleme](how-to-track-experiments.md)
* [Hiperparametreleri ayarlama](how-to-tune-hyperparameters.md)
* [Azure'da dağıtılmış derin öğrenim eğitimi için referans mimarisi](/azure/architecture/reference-architectures/ai/training-deep-learning)
