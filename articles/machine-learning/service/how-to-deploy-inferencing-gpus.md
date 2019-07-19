---
title: GPU ile çıkarım için model dağıtma
titleSuffix: Azure Machine Learning service
description: Bu makalede, bir Web hizmeti olarak GPU özellikli bir TensorFlow derin öğrenme modelini dağıtmak için Azure Machine Learning hizmetini nasıl kullanacağınız öğretilir. hizmet ve puan çıkarım istekleri.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 06/01/2019
ms.openlocfilehash: eeb1bc35e0438a7e99ea5ed8284f0c8611108da0
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326983"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>GPU ile çıkarım için derin öğrenme modeli dağıtma

Bu makalede, bir Web hizmeti olarak GPU özellikli bir TensorFlow derin öğrenme modelini dağıtmak için Azure Machine Learning hizmetini nasıl kullanacağınız öğretilir.

Bir Azure Kubernetes hizmeti (AKS) kümesine, GPU etkin bir ınconnectionpoint yapmak için modelinizi dağıtın. Indıor veya model Puanlama, dağıtılan modelin tahmin için kullanıldığı aşamadır. CPU yerine GPU kullanımı, yüksek bir paralellik hesaplama üzerinde performans avantajları sunmaktadır.

Bu örnek bir TensorFlow modeli kullanmasına karşın, Puanlama dosyasında ve ortam dosyasında küçük değişiklikler yaparak GPU 'ları destekleyen herhangi bir makine öğrenimi çerçevesine aşağıdaki adımları uygulayabilirsiniz. 

Bu makalede, aşağıdaki adımları uygulayın:

* GPU özellikli bir AKS kümesi oluşturma
* TensorFlow GPU modeli dağıtma
* Dağıtılan modelinize örnek sorgu verme

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning Services çalışma alanı.
* Python yok.
* Kayıtlı bir TensorFlow modeli kaydedildi.
    * Modellerin nasıl kaydedileceği hakkında bilgi edinmek için bkz. [modelleri dağıtma](../service/how-to-deploy-and-where.md#registermodel).

Bu nasıl yapılır serisinden bir kısmını, [bir TensorFlow modelini nasıl Eğitekullanacağınızı](how-to-train-tensorflow.md), gerekli önkoşulları yerine getirebilirsiniz.

## <a name="provision-an-aks-cluster-with-gpus"></a>GPU 'Lar ile AKS kümesi sağlama

Azure 'da birçok farklı GPU seçeneği vardır. Bunlardan herhangi birini, ınıri için kullanabilirsiniz. Yeteneklerin ve maliyetlerin tam bir dökümü için [N serisi VM 'lerin listesine](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) bakın.

Azure Machine Learning hizmeti ile AKS kullanma hakkında daha fazla bilgi için bkz. [dağıtım ve nerede](../service/how-to-deploy-and-where.md#deploy-aks).

```Python
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
> AKS kümesi sağlandığı sürece Azure sizi faturalandıracaktır. İle işiniz bittiğinde AKS kümenizi sildiğinizden emin olun.

## <a name="write-the-entry-script"></a>Giriş betiğini yazın

Aşağıdaki kodu çalışma dizininize olarak `score.py`kaydedin. Bu dosya görüntüleri hizmetinize gönderilirken puan alır. TensorFlow kaydedilmiş modelini yükler, giriş görüntüsünü her bir POST isteğindeki TensorFlow oturumuna geçirir ve sonra elde edilen puanları döndürür. Diğer ınırm çerçeveleri farklı Puanlama dosyaları gerektirir.

```python
import json
import numpy as np
import os
import tensorflow as tf

from azureml.core.model import Model

def init():
    global X, output, sess
    tf.reset_default_graph()
    model_root = Model.get_model_path('tf-dnn-mnist')
    saver = tf.train.import_meta_graph(os.path.join(model_root, 'mnist-tf.model.meta'))
    X = tf.get_default_graph().get_tensor_by_name("network/X:0")
    output = tf.get_default_graph().get_tensor_by_name("network/output/MatMul:0")
    
    sess = tf.Session()
    saver.restore(sess, os.path.join(model_root, 'mnist-tf.model'))

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    out = output.eval(session=sess, feed_dict={X: data})
    y_hat = np.argmax(out, axis=1)
    return y_hat.tolist()

```
## <a name="define-the-conda-environment"></a>Conda ortamını tanımlama

Hizmetinizin bağımlılıklarını belirtmek için adlı `myenv.yml` bir Conda ortam dosyası oluşturun. Hızlandırılmış performans elde `tensorflow-gpu` etmek için kullandığınızdan emin olmanız önemlidir.

```yaml
name: project_environment
dependencies:
  # The python interpreter version.
  # Currently Azure ML only supports 3.5.2 and later.
- python=3.6.2

- pip:
  - azureml-defaults==1.0.43.*
- numpy
- tensorflow-gpu=1.12
channels:
- conda-forge
```

## <a name="define-the-gpu-inferenceconfig-class"></a>GPU ınenceconfig sınıfını tanımlama

GPU 'ları `InferenceConfig` sağlayan bir nesne oluşturun ve CUDA 'ın Docker yansımasıyla yüklenmesini sağlar.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='aks-dnn-mnist'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"tf-dnn-mnist")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   enable_gpu=True)
```

Daha fazla bilgi için bkz.

- [Inenceconfig sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [AksServiceDeploymentConfiguration sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Modeli dağıtma

Modeli AKS kümenize dağıtın ve hizmetinizi oluşturmak için bekleyin.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=gpu_aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Azure Machine Learning hizmet GPU 'ya sahip olmayan bir kümeye `InferenceConfig` GPU 'nun etkinleştirilmesini bekleyen bir nesne içeren bir model dağıtmayacaktır.

Daha fazla bilgi için bkz. [model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-a-sample-query-to-your-model"></a>Modelinize örnek sorgu verme

Dağıtılan modele bir test sorgusu gönderin. Modele bir JPEG görüntüsü gönderdiğinizde, görüntüde puan alır. Aşağıdaki kod örneği, görüntüleri yüklemek için bir dış yardımcı program işlevi kullanır. İlgili kodu [GitHub 'da PIR TensorFlow örneğinde](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/utils.py)bulabilirsiniz. 

```python
# Used to test your webservice
from utils import load_data 

# Load test data from model training
X_test = load_data('./data/mnist/test-images.gz', False) / 255.0
y_test = load_data('./data/mnist/test-labels.gz', True).reshape(-1)

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

api_key = aks_service.get_keys()[0]
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
resp = requests.post(aks_service.scoring_uri, input_data, headers=headers)

print("POST to url", aks_service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

> [!IMPORTANT]
> Gecikme süresini en aza indirmek ve aktarım hızını iyileştirmek için, istemcinizin uç noktayla aynı Azure bölgesinde olduğundan emin olun. Bu örnekte, API 'Ler Doğu ABD Azure bölgesinde oluşturulur.

## <a name="clean-up-the-resources"></a>Kaynakları Temizleme

Bu örnek için AKS kümesini özel olarak oluşturduysanız, işiniz bittiğinde kaynakları silin.

> [!IMPORTANT]
> Azure, AKS kümesinin ne kadar süreyle dağıtıldığını temel alır. Bu işlemi tamamladıktan sonra temizlediğinizden emin olun.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Sonraki adımlar

* [FPGA 'da model dağıtma](../service/how-to-deploy-fpga-web-service.md)
* [Modeli ONNX ile Dağıt](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [TensorFlow DNN modellerini eğitme](../service/how-to-train-tensorflow.md)
