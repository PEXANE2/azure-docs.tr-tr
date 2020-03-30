---
title: GPU ile çıkarım için bir model dağıtma
titleSuffix: Azure Machine Learning
description: Bu makalede, GPU özellikli Tensorflow derin öğrenme modelini bir web hizmeti hizmeti ve puan çıkarımı istekleri olarak dağıtmak için Azure Machine Learning'i nasıl kullanacağınızı öğretir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 03/05/2020
ms.openlocfilehash: b0fd537d1930e7c9d5f7a33f56ec5d00b1556562
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78398331"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>GPU ile çıkarım için derin bir öğrenme modeli dağıtın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, GPU özellikli bir modeli web hizmeti olarak dağıtmak için Azure Machine Learning'i nasıl kullanacağınızı öğretilir. Bu makaledeki bilgiler, Azure Kubernetes Hizmeti'nde (AKS) bir model dağıtmaya dayanır. AKS kümesi çıkarım için model tarafından kullanılan bir GPU kaynağı sağlar.

Çıkarım veya model puanlama, dağıtılan modelin öngörülerde bulunmak için kullanıldığı aşamadır. CPU yerine GPU'ları kullanmak, son derece paralellenebilir hesaplamada performans avantajları sunar.

> [!IMPORTANT]
> Web hizmeti dağıtımları için GPU çıkarımı yalnızca Azure Kubernetes Hizmetinde desteklenir. __Makine öğrenimi ardışık hattını__kullanarak çıkarım için GPU'lar yalnızca Azure Machine Learning Compute'da desteklenir. ML ardışık hatları kullanma hakkında daha fazla bilgi için toplu [iş tahminlerini çalıştır'a](how-to-use-parallel-run-step.md)bakın. 

> [!TIP]
> Bu makaledeki kod parçacıkları tensorFlow modeli kullansa da, bilgileri GPU'ları destekleyen herhangi bir makine öğrenimi çerçevesine uygulayabilirsiniz.

> [!NOTE]
> Bu makaledeki bilgiler, [Azure Kubernetes Hizmeti](how-to-deploy-azure-kubernetes-service.md) makalesinde nasıl dağıtılır bilgileri temel ayar. Bu makalegenellikle AKS'ye dağıtımı kapsadığı durumlarda, bu makale GPU'ya özgü dağıtımı kapsar.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı. Daha fazla bilgi için [bkz.](how-to-manage-workspace.md)

* Azure Machine Learning SDK yüklü bir Python geliştirme ortamı. Daha fazla bilgi için Azure [Machine Learning SDK'ya](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)bakın.  

* GPU kullanan kayıtlı bir model.

    * Modelleri nasıl kaydedebilirsiniz öğrenmek için [Modelleri Dağıt'a](how-to-deploy-and-where.md#registermodel)bakın.

    * Bu belgeyi oluşturmak için kullanılan Tensorflow modelini oluşturmak ve kaydetmek için [tensorflow modelini nasıl eğitilir'e](how-to-train-tensorflow.md)bakın.

* Modellerin nasıl [ve nerede dağıtılanın acağı](how-to-deploy-and-where.md)hakkında genel bir anlayış.

## <a name="connect-to-your-workspace"></a>Çalışma alanınıza bağlanma

Varolan bir çalışma alanına bağlanmak için aşağıdaki kodu kullanın:

> [!IMPORTANT]
> Bu kod snippet çalışma alanı yapılandırmasının geçerli dizine veya üst öğesine kaydolmasını bekler. Çalışma alanı oluşturma hakkında daha fazla bilgi için Azure [Makine Öğrenimi çalışma alanlarını oluştur ve yönet' e](how-to-manage-workspace.md)bakın.   Yapılandırmayı dosyaya kaydetme hakkında daha fazla bilgi için [bkz.](how-to-configure-environment.md#workspace)

```python
from azureml.core import Workspace

# Connect to the workspace
ws = Workspace.from_config()
```

## <a name="create-a-kubernetes-cluster-with-gpus"></a>GPU'lar ile bir Kubernetes kümesi oluşturma

Azure Kubernetes Hizmeti birçok farklı GPU seçeneği sunar. Bunlardan herhangi birini model çıkarımı için kullanabilirsiniz. Yeteneklerin ve maliyetlerin tam dökümü için [N serisi VM'lerin listesine](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) bakın.

Aşağıdaki kod, çalışma alanınız için yeni bir AKS kümesinin nasıl oluşturulturunu gösterir:

```python
from azureml.core.compute import ComputeTarget, AksCompute
from azureml.exceptions import ComputeTargetException

# Choose a name for your cluster
aks_name = "aks-gpu"

# Check to see if the cluster already exists
try:
    aks_target = ComputeTarget(workspace=ws, name=aks_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    # Provision AKS cluster with GPU machine
    prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

    # Create the cluster
    aks_target = ComputeTarget.create(
        workspace=ws, name=aks_name, provisioning_configuration=prov_config
    )

    aks_target.wait_for_completion(show_output=True)
```

> [!IMPORTANT]
> Azure, AKS kümesi var olduğu sürece size fatura kesecektir. Bu kümeyle işiniz bittiğinde AKS kümenizi sildiğinizden emin olun.

Azure Machine Learning ile AKS kullanma hakkında daha fazla bilgi için [Azure Kubernetes Hizmetine nasıl dağıtılanın.](how-to-deploy-azure-kubernetes-service.md)

## <a name="write-the-entry-script"></a>Giriş komut dosyasını yazma

Giriş komut dosyası web hizmetine gönderilen verileri alır, modele aktarın ve puanlama sonuçlarını döndürür. Aşağıdaki komut dosyası başlangıç tarihinde Tensorflow modelini yükler ve daha sonra veri puan için modeli kullanır.

> [!TIP]
> Giriş betiği, modelinize göre değişir. Örneğin, komut dosyası modeli, veri biçimleri, vb ile kullanmak için çerçeve bilmeli.

```python
import json
import numpy as np
import os
import tensorflow as tf

from azureml.core.model import Model


def init():
    global X, output, sess
    tf.reset_default_graph()
    model_root = os.getenv('AZUREML_MODEL_DIR')
    # the name of the folder in which to look for tensorflow model files
    tf_model_folder = 'model'
    saver = tf.train.import_meta_graph(
        os.path.join(model_root, tf_model_folder, 'mnist-tf.model.meta'))
    X = tf.get_default_graph().get_tensor_by_name("network/X:0")
    output = tf.get_default_graph().get_tensor_by_name("network/output/MatMul:0")

    sess = tf.Session()
    saver.restore(sess, os.path.join(model_root, tf_model_folder, 'mnist-tf.model'))


def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    out = output.eval(session=sess, feed_dict={X: data})
    y_hat = np.argmax(out, axis=1)
    return y_hat.tolist()
```

Bu dosyanın `score.py`adı . Giriş komut dosyaları hakkında daha fazla bilgi için [bkz.](how-to-deploy-and-where.md)

## <a name="define-the-conda-environment"></a>Conda ortamını tanımlayın

Conda ortamı dosyası, hizmete yönelik bağımlılıkları belirtir. Hem model hem de giriş komut dosyası tarafından gerekli olan bağımlılıkları içerir. Modeli web hizmeti olarak barındırmak için gereken işlevselliği içerdiğinden, verion >= 1,0,45 pip bağımlılığı olarak azureml varsayılanlarını belirtmeniz gerektiğini lütfen unutmayın. Aşağıdaki YAML, Tensorflow modelinin ortamını tanımlar. Bu dağıtımda `tensorflow-gpu`kullanılan GPU'dan yararlanacağını belirtir:

```yaml
name: project_environment
dependencies:
  # The python interpreter version.
  # Currently Azure ML only supports 3.5.2 and later.
- python=3.6.2

- pip:
  # You must list azureml-defaults as a pip dependency
  - azureml-defaults>=1.0.45
- numpy
- tensorflow-gpu=1.12
channels:
- conda-forge
```

Bu örnekte, dosya . `myenv.yml`

## <a name="define-the-deployment-configuration"></a>Dağıtım yapılandırmasını tanımlama

Dağıtım yapılandırması, web hizmetini çalıştırmak için kullanılan Azure Kubernetes Hizmet ortamını tanımlar:

```python
from azureml.core.webservice import AksWebservice

gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled=False,
                                                    num_replicas=3,
                                                    cpu_cores=2,
                                                    memory_gb=4)
```

Daha fazla bilgi için [AksService.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none--gpu-cores-none--period-seconds-none--initial-delay-seconds-none--timeout-seconds-none--success-threshold-none--failure-threshold-none--namespace-none--token-auth-enabled-none--compute-target-name-none-)için başvuru belgelerine bakın.

## <a name="define-the-inference-configuration"></a>Çıkarım yapılandırmasını tanımlama

Çıkarım yapılandırması, giriş komut dosyasına ve GPU destekli bir docker görüntüsünü kullanan bir ortam nesnesine işaret eder. Çevre tanımı için kullanılan YAML dosyasının, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerdiğinden, >= 1.0.45 sürümüne sahip azureml varsayılanlarını pip bağımlılığı olarak listelemesi gerektiğini lütfen unutmayın.

```python
from azureml.core.model import InferenceConfig
from azureml.core.environment import Environment, DEFAULT_GPU_IMAGE

myenv = Environment.from_conda_specification(name="myenv", file_path="myenv.yml")
myenv.docker.base_image = DEFAULT_GPU_IMAGE
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
```

Ortamlar hakkında daha fazla bilgi için [bkz.](how-to-use-environments.md)
Daha fazla bilgi için [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)için başvuru belgelerine bakın.

## <a name="deploy-the-model"></a>Modeli dağıtma

Modeli AKS kümenize dağıtın ve hizmetinizi oluşturmasını bekleyin.

```python
from azureml.core.model import Model

# Name of the web service that is deployed
aks_service_name = 'aks-dnn-mnist'
# Get the registerd model
model = Model(ws, "tf-dnn-mnist")
# Deploy the model
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config,
                           deployment_config=gpu_aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output=True)
print(aks_service.state)
```

> [!NOTE]
> Nesne `InferenceConfig` varsa, `enable_gpu=True` `deployment_target` parametre gpu sağlayan bir küme başvurugerekir. Aksi takdirde, dağıtım başarısız olur.

Daha fazla bilgi için [Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py)için başvuru belgelerine bakın.

## <a name="issue-a-sample-query-to-your-service"></a>Hizmetinize örnek bir sorgu verme

Dağıtılan modele bir test sorgusu gönderin. Modele bir jpeg görüntüsü gönderdiğinde, görüntüyü puanlar. Aşağıdaki kod örneği test verilerini karşıdan yükler ve ardından hizmete göndermek için rasgele bir test görüntüsü seçer.

```python
# Used to test your webservice
import os
import urllib
import gzip
import numpy as np
import struct
import requests

# load compressed MNIST gz files and return numpy arrays
def load_data(filename, label=False):
    with gzip.open(filename) as gz:
        struct.unpack('I', gz.read(4))
        n_items = struct.unpack('>I', gz.read(4))
        if not label:
            n_rows = struct.unpack('>I', gz.read(4))[0]
            n_cols = struct.unpack('>I', gz.read(4))[0]
            res = np.frombuffer(gz.read(n_items[0] * n_rows * n_cols), dtype=np.uint8)
            res = res.reshape(n_items[0], n_rows * n_cols)
        else:
            res = np.frombuffer(gz.read(n_items[0]), dtype=np.uint8)
            res = res.reshape(n_items[0], 1)
    return res

# one-hot encode a 1-D array
def one_hot_encode(array, num_of_classes):
    return np.eye(num_of_classes)[array.reshape(-1)]

# Download test data
os.makedirs('./data/mnist', exist_ok=True)
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/mnist/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/mnist/test-labels.gz')

# Load test data from model training
X_test = load_data('./data/mnist/test-images.gz', False) / 255.0
y_test = load_data('./data/mnist/test-labels.gz', True).reshape(-1)

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

api_key = aks_service.get_keys()[0]
headers = {'Content-Type': 'application/json',
           'Authorization': ('Bearer ' + api_key)}
resp = requests.post(aks_service.scoring_uri, input_data, headers=headers)

print("POST to url", aks_service.scoring_uri)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

İstemci uygulaması oluşturma hakkında daha fazla bilgi için [bkz.](how-to-consume-web-service.md)

## <a name="clean-up-the-resources"></a>Kaynakları temizleme

AKS kümesini bu örnek için özel olarak oluşturduysanız, işiniz bittikten sonra kaynaklarınızı silin.

> [!IMPORTANT]
> AKS kümesinin ne kadar süreyle dağıtılana bağlı olarak azure faturaları. Onunla bitirdikten sonra temizlemek için emin olun.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

* [FPGA'da modeli dağıt](how-to-deploy-fpga-web-service.md)
* [ONNX ile modeli dağıtma](concept-onnx.md#deploy-onnx-models-in-azure)
* [Tren Tensorflow DNN Modelleri](how-to-train-tensorflow.md)
