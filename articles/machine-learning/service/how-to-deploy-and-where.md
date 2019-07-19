---
title: Modellerin nasıl ve nereye dağıtılacağı
titleSuffix: Azure Machine Learning service
description: 'Aşağıdakiler de dahil olmak üzere Azure Machine Learning hizmeti modellerinizi nasıl ve nereye dağıtacağınızı öğrenin: Azure Container Instances, Azure Kubernetes hizmeti, Azure IoT Edge ve alan-programlanabilir kapı dizileri.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/08/2019
ms.custom: seoapril2019
ms.openlocfilehash: 796118999041b2bef2d51657901e9e399578e97c
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68327044"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile modelleri dağıtma

Machine Learning modelinizi Azure bulutu 'nda bir Web hizmeti olarak veya IoT Edge cihaz olarak dağıtmayı öğrenin. 

İş akışı, modelinizi [dağıttığınız yere](#target) bakılmaksızın benzerdir:

1. Modeli kaydedin.
1. Dağıtıma hazırlanma (varlıkları, kullanımı, işlem hedefini belirtin)
1. Modeli işlem hedefine dağıtın.
1. Web hizmeti olarak da adlandırılan dağıtılmış modeli test edin.

Dağıtım iş akışında yer alan kavramlar hakkında daha fazla bilgi için bkz. [Azure Machine Learning hizmeti ile modelleri yönetme, dağıtma ve izleme](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Önkoşullar

- Bir model. Eğitilen bir modeliniz yoksa, [Bu öğreticide](https://aka.ms/azml-deploy-cloud)verilen model & bağımlılık dosyalarını kullanabilirsiniz.

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://aka.ms/aml-sdk)veya [Azure Machine Learning Visual Studio Code uzantısı](how-to-vscode-tools.md).

## <a id="registermodel"></a>Modelinizi kaydetme

Modelinizi oluşturan bir veya daha fazla dosya için kayıtlı model mantıksal kapsayıcısı. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları çalışma alanına tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Machine Learning modelleri Azure Machine Learning çalışma alanınıza kaydedilir. Model Azure Machine Learning gelebilir veya herhangi bir yerden gelebilir. Aşağıdaki örneklerde, bir modelin dosyadan nasıl kaydedileceği gösterilmektedir:

### <a name="register-a-model-from-an-experiment-run"></a>Deneme çalıştırmasında bir modeli kaydetme

+ **Scikit-SDK kullanarak örnek öğrenin**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  > [!TIP]
  > Model kaydına birden çok dosya eklemek için, dosyaları içeren `model_path` dizine ayarlayın.

+ **CLı 'yi kullanma**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```

  > [!TIP]
  > Model kaydına birden çok dosya eklemek için, dosyaları içeren `--asset-path` dizine ayarlayın.

+ **VS Code kullanma**

  Modelleri [vs Code](how-to-vscode-tools.md#deploy-and-manage-models) uzantılı herhangi bir model dosyası veya klasörü kullanarak kaydedin.

### <a name="register-an-externally-created-model"></a>Dışarıdan oluşturulan modeli kaydetme

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

Modele **yerel bir yol** sağlayarak dışarıdan oluşturulmuş bir modeli kaydedebilirsiniz. Bir klasör ya da tek bir dosya sağlayabilirsiniz.

+ **Python SDK ile ONNX örneği:**
  ```python
  onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
  urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
  !tar xvzf mnist.tar.gz
  
  model = Model.register(workspace = ws,
                         model_path ="mnist/model.onnx",
                         model_name = "onnx_mnist",
                         tags = {"onnx": "demo"},
                         description = "MNIST image classification CNN from ONNX Model Zoo",)
  ```

  > [!TIP]
  > Model kaydına birden çok dosya eklemek için, dosyaları içeren `model_path` dizine ayarlayın.

+ **CLı 'yi kullanma**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  > [!TIP]
  > Model kaydına birden çok dosya eklemek için, dosyaları içeren `-p` dizine ayarlayın.

**Tahmini süre**: Yaklaşık 10 saniye.

Daha fazla bilgi için başvuru belgeleri için bkz. [Model sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Azure Machine Learning hizmeti dışında eğitilen modellerle çalışma hakkında daha fazla bilgi için bkz. [var olan bir modeli dağıtma](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>İşlem hedefi seçin

Aşağıdaki işlem hedefleri veya işlem kaynakları, Web hizmeti dağıtımınızı barındırmak için kullanılabilir. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Dağıtmaya hazırlanma

Web hizmeti olarak dağıtmak için bir çıkarım yapılandırması (`InferenceConfig`) ve bir dağıtım yapılandırması oluşturmanız gerekir. Çıkarım veya model Puanlama, dağıtılan modelin tahmin için en yaygın olarak üretim verilerinde kullanıldığı aşamadır. Çıkarım yapılandırmasında, modelinize hizmeti sağlamak için gereken betikleri ve bağımlılıkları belirtirsiniz. Dağıtım yapılandırmasında, işlem hedefinde modelin nasıl kullanılacağına ilişkin ayrıntıları belirtirsiniz.

> [!IMPORTANT]
> Azure Machine Learning SDK, Web hizmeti veya IoT Edge dağıtımlarının veri deposuna veya veri kümelerine erişmesi için bir yol sağlamaz. Dağıtım dışında depolanan verilere (örneğin, bir Azure depolama hesabında) erişmek için dağıtılan modele ihtiyacınız varsa, ilgili SDK 'yı kullanarak özel bir kod çözümü geliştirmeniz gerekir. Örneğin, [Python Için Azure depolama SDK 'sı](https://github.com/Azure/azure-storage-python).
>
> Senaryonuza yönelik olabilecek başka bir alternatif de [toplu tahmindir](how-to-run-batch-predictions.md). Bu, Puanlama sırasında veri depolarına erişim sağlar.

### <a id="script"></a> 1. Giriş betiğinizi & bağımlılıklarınızı tanımlayın

Giriş betiği dağıtılan bir Web hizmetine gönderilen verileri alır ve modele geçirir. Ardından model tarafından döndürülen yanıtı alır ve istemciye döndürür. **Betik, modelinize özeldir**; modelin beklediği ve döndürdüğü verileri anlaması gerekir.

Betik, modeli yükleyen ve çalıştıran iki işlev içerir:

* `init()`: Genellikle bu işlev, modeli genel bir nesneye yükler. Bu işlev, Web hizmetiniz için Docker kapsayıcısı başlatıldığında yalnızca bir kez çalıştırılır.

* `run(input_data)`: Bu işlev, giriş verilerine göre bir değeri tahmin etmek için modeli kullanır. Çalıştırma girişleri ve çıkışları genellikle serileştirme ve seri hale getirme için JSON kullanır. Ham ikili verilerle de çalışabilirsiniz. Modele göndermeden önce veya istemciye döndürmeden önce verileri dönüştürebilirsiniz.

#### <a name="what-is-getmodelpath"></a>Get_model_path nedir?

Bir modeli kaydettiğinizde, kayıt defterinde modeli yönetmek için kullanılan bir model adı sağlarsınız. Bu adı modeliyle birlikte kullanırsınız. yerel dosya sistemindeki model dosyalarının yolunu almak için [_model_path () alın](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) . Bir klasörü veya bir dosya koleksiyonunu kaydettiğinizde, bu API bu dosyaları içeren dizinin yolunu döndürür.

Bir modeli kaydettiğinizde, modelin yerel olarak veya hizmet dağıtımı sırasında yerleştirildiği yere karşılık gelen bir ad verirsiniz.

Aşağıdaki örnek, adlı `sklearn_mnist_model.pkl` tek bir dosyaya (ada `sklearn_mnist`kayıtlı olan) bir yol döndürür:

```python
model_path = Model.get_model_path('sklearn_mnist')
``` 

#### <a name="optional-automatic-swagger-schema-generation"></a>Seçim Otomatik Swagger şeması oluşturma

Web hizmetiniz için otomatik olarak bir şema oluşturmak üzere, tanımlı tür nesnelerinden biri için kurucudaki giriş ve/veya çıkışın bir örneğini sağlayın ve tür ve örnek şemayı otomatik olarak oluşturmak için kullanılır. Azure Machine Learning hizmet daha sonra dağıtım sırasında Web hizmeti için bir [Openapı](https://swagger.io/docs/specification/about/) (Swagger) belirtimi oluşturur.

Şu türler Şu anda destekleniyor:

* `pandas`
* `numpy`
* `pyspark`
* Standart Python nesnesi

Şema oluşturmayı kullanmak için, `inference-schema` paketi Conda ortam dosyanıza ekleyin. Aşağıdaki örnek, giriş `[numpy-support]` betiği bir sayısal tuş-parametre türü kullandığından kullanılır: 

#### <a name="example-dependencies-file"></a>Örnek bağımlılıklar dosyası
Aşağıdaki YAML, çıkarım için Conda Dependencies bir dosya örneğidir.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Otomatik şema oluşturmayı kullanmak istiyorsanız, giriş betiğinizin `inference-schema` paketleri içeri aktarması **gerekir** . 

Web hizmetinin istek ve yanıt biçimlerini temsil eden `input_sample` ve `output_sample` değişkenlerinde giriş ve çıkış örnek biçimlerini tanımlayın. Bu örnekleri `run()` işlevindeki giriş ve çıkış işlevi Dekoratörleri içinde kullanın. Scikit-aşağıdaki örnek, şema oluşturmayı kullanır.

> [!TIP]
> Hizmeti dağıttıktan sonra şema JSON belgesini almak `swagger_uri` için özelliğini kullanın.

#### <a name="example-entry-script"></a>Örnek giriş betiği

Aşağıdaki örnek, JSON verilerinin nasıl kabul edileceği ve geri dönebileceğinizi göstermektedir:

```python
#example: scikit-learn and Swagger
import json
import numpy as np
from sklearn.externals import joblib
from sklearn.linear_model import Ridge
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType

def init():
    global model
    # note here "sklearn_regression_model.pkl" is the name of the model registered under
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('sklearn_regression_model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = np.array([[10,9,8,7,6,5,4,3,2,1]])
output_sample = np.array([3726.995])

@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Sözlük girişi ile örnek betik (Power BI destek tüketimi)

Aşağıdaki örnek, veri çerçevesini kullanarak giriş verilerinin < anahtar: değer > sözlüğü olarak nasıl tanımlanacağını gösterir. Bu yöntem, dağıtılan Web hizmetinin Power BI tüketmesi için desteklenir ([Power BI Web hizmetini kullanma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType

def init():
    global model
    model_path = Model.get_model_path('model_name')   # replace model_name with your actual model name, if needed
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = pd.DataFrame(data=[{
              "input_name_1": 5.1,         # This is a decimal type sample. Use the data type that reflects this column in your data
              "input_name_2": "value2",    # This is a string type sample. Use the data type that reflects this column in your data
              "input_name_3": 3            # This is a integer type sample. Use the data type that reflects this column in your data
            }])

output_sample = np.array([0])              # This is a integer type sample. Use the data type that reflects the expected result

@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```
Daha fazla örnek komut dosyası için aşağıdaki örneklere bakın:

* Pytorch:[https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow[https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras[https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX[https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* İkili verilere göre Puanlama: [Web hizmetini kullanma](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Inısenceconfig 'nizi tanımlama

Çıkarım yapılandırması, tahmine dayalı hale getirmek üzere modelin nasıl yapılandırılacağını açıklar. Aşağıdaki örnek, bir çıkarım yapılandırmasının nasıl oluşturulacağını gösterir. Bu yapılandırma, çalışma zamanını, giriş betiğini ve (isteğe bağlı olarak) Conda ortam dosyasını belirtir:

```python
inference_config = InferenceConfig(runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Daha fazla bilgi için bkz. [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfı başvurusu.

Çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>Inenceconfig CLı örneği

Aşağıdaki JSON belgesi, Machine Learning CLı ile kullanılmak üzere örnek bir çıkarım yapılandırması örneğidir:

```JSON
{
   "entryScript": "x/y/score.py",
   "runtime": "python",
   "condaFile": "env/myenv.yml",
   "sourceDirectory":"C:/abc",
}
```

Aşağıdaki varlıklar bu dosyada geçerlidir:

* __Entryscript__: Görüntüde çalıştırılacak kodu içeren yerel dosyanın yolu.
* __çalışma zamanı__: Görüntü için kullanılacak çalışma zamanı. Desteklenen geçerli çalışma zamanları ' Spark-Kopyala ' ve ' Python '.
* __Condadfile__ (isteğe bağlı): Görüntü için kullanılacak Conda ortam tanımını içeren yerel dosyanın yolu.
* __Extradockerfilesteps__ (isteğe bağlı): Görüntü ayarlanırken çalıştırılacak ek Docker adımlarını içeren yerel dosyanın yolu.
* __Sourcedirectory__ (isteğe bağlı): Görüntüyü oluşturmak için tüm dosyaları içeren klasörlerin yolu.
* __Enablegpu__ (isteğe bağlı): Görüntüde GPU desteğinin etkinleştirilip etkinleştirilmeyeceğini belirtir. GPU görüntüsünün Azure Container Instances, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti gibi Microsoft Azure hizmetlerinde kullanılması gerekir. Varsayılan değer false şeklindedir.
* __Baseımage__ (isteğe bağlı): Temel görüntü olarak kullanılacak özel bir görüntü. Hiçbir temel görüntü verilmezse, temel görüntü belirtilen çalışma zamanı parametresi temel alınarak kullanılacaktır.
* __Baseimageregistry__ (isteğe bağlı): Temel görüntüyü içeren görüntü kayıt defteri.
* __Cudadversion__ (isteğe bağlı): GPU desteği gerektiren görüntüler için yüklenecek CUDA sürümü. GPU görüntüsünün Azure Container Instances, Azure Machine Learning Işlem, Azure sanal makineleri ve Azure Kubernetes hizmeti gibi Microsoft Azure hizmetlerinde kullanılması gerekir. Desteklenen sürümler 9,0, 9,1 ve 10,0. ' Enable_gpu ' ayarlanırsa, varsayılan olarak ' 9,1 ' kullanılır.

Bu varlıklar, [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfının parametreleriyle eşlenir.

Aşağıdaki komutta, CLı kullanarak bir modelin nasıl dağıtılacağı gösterilmektedir:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

Bu örnekte, yapılandırma aşağıdaki öğeleri içerir:

* Çıkarımı için gereken varlıkları içeren bir dizin
* Bu modelin Python gerektirdiğini
* Dağıtılan hizmete gönderilen Web isteklerini işlemek için kullanılan [giriş betiği](#script)
* Çıkarımı için gereken Python paketlerini açıklayan Conda dosyası

Çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md).

### <a name="3-define-your-deployment-configuration"></a>3. Dağıtım yapılandırmanızı tanımlama

Dağıtılmadan önce dağıtım yapılandırmasını tanımlamanız gerekir. Dağıtım yapılandırması, Web hizmetini barındıracak işlem hedefine özgüdür. Örneğin, yerel olarak dağıttığınızda, hizmetin istekleri kabul ettiği bağlantı noktasını belirtmeniz gerekir.

İşlem kaynağını da oluşturmanız gerekebilir. Örneğin, çalışma alanınız ile ilişkili bir Azure Kubernetes hizmetiniz yoksa.

Aşağıdaki tabloda her işlem hedefi için bir dağıtım yapılandırması oluşturma örneği verilmiştir:

| Hedef işlem | Dağıtım yapılandırma örneği |
| ----- | ----- |
| Yerel | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Örneği | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Aşağıdaki bölümlerde, dağıtım yapılandırmasının nasıl oluşturulacağı ve Web hizmetini dağıtmak için nasıl kullanılacağı gösterilmektedir.

### <a name="optional-profile-your-model"></a>İsteğe bağlı: Modelinizin profilini yapın

Modelinizi bir hizmet olarak dağıtmadan önce SDK veya CLı kullanarak en iyi CPU ve bellek gereksinimlerini öğrenmek için profili oluşturabilirsiniz.  Model profil oluşturma sonuçları bir `Run` nesne olarak yayınlanır. [Model profili şemasının tüm ayrıntıları API belgelerinde bulunabilir](https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py)

[SDK 'yı kullanarak modelinizin profilini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-)oluşturma hakkında daha fazla bilgi edinin.

CLı kullanarak modelinizin profilini eklemek için [az ml model profili](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile)kullanın.

## <a name="deploy-to-target"></a>Hedefe dağıt

### <a id="local"></a>Yerel dağıtım

Yerel olarak dağıtmak için, yerel makinenizde **Docker 'ın yüklü** olması gerekir.

+ **SDK 'Yı kullanma**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **CLı 'yi kullanma**

    CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. Kayıtlı `mymodel:1` modelin adı ve sürümüyle değiştirin:

  ```azurecli-interactive
  az ml model deploy -m mymodel:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    `deploymentconfig.json` Belgedeki girişler [localwebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py)parametrelerine eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

    | JSON varlığı | Yöntem parametresi | Açıklama |
    | ----- | ----- | ----- |
    | `computeType` | NA | Bilgi işlem hedefi. Yerel için değer olmalıdır `local`. |
    | `port` | `port` | Hizmetin HTTP uç noktasının sergilebileceği yerel bağlantı noktası. |

    Aşağıdaki JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

    ```json
    {
        "computeType": "local",
        "port": 32267
    }
    ```

### <a id="aci"></a>Azure Container Instances (DEVTEST)

Bir web hizmeti bir veya daha aşağıdaki koşullardan biri Modellerinizi dağıtmak için Azure Container Instances kullanmak doğrudur:
- Hızlı bir şekilde dağıtın ve modelinizi doğrulama gerekir.
- Geliştirilmekte olan bir modeli test edersiniz. 

ACı 'nin kota ve bölge kullanılabilirliğini görmek için [Azure Container Instances Için kotalar ve bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) bölümüne bakın.

+ **SDK 'Yı kullanma**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **CLı 'yi kullanma**

    CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. Kayıt `mymodel:1` , kayıtlı modelin adı ve sürümü ile değiştirin. Bu `myservice` hizmete verilecek adla değiştirin:

    ```azurecli-interactive
    az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
    ```

    `deploymentconfig.json` Belgedeki girişler [aciwebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py)parametrelerine eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

    | JSON varlığı | Yöntem parametresi | Açıklama |
    | ----- | ----- | ----- |
    | `computeType` | NA | Bilgi işlem hedefi. ACI için değer olmalıdır `ACI`. |
    | `containerResourceRequirements` | NA | Kapsayıcı için ayrılan CPU ve bellek için yapılandırma öğelerini içerir. |
    | &emsp;&emsp;`cpu` | `cpu_cores` | Bu Web hizmeti için ayrılacak CPU çekirdeklerinin sayısı. Olarak`0.1` |
    | &emsp;&emsp;`memoryInGB` | `memory_gb` | Bu Web hizmeti için ayrılacak bellek miktarı (GB cinsinden). Varsayılanını`0.5` |
    | `location` | `location` | Bu Web hizmeti 'nin dağıtılacağı Azure bölgesi. Belirtilmemişse, çalışma alanı konumu kullanılacaktır. Kullanılabilir bölgeler hakkında daha fazla ayrıntı için şurada bulunabilir: [ACI bölgeleri](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
    | `authEnabled` | `auth_enabled` | Bu Web hizmeti için kimlik doğrulamanın etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan değer false şeklindedir |
    | `sslEnabled` | `ssl_enabled` | Bu Web hizmeti için SSL etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan değer false şeklindedir. |
    | `appInsightsEnabled` | `enable_app_insights` | Bu Web hizmeti için Appınsights 'ın etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan değer false şeklindedir |
    | `sslCertificate` | `ssl_cert_pem_file` | SSL etkinse gereken sertifika dosyası |
    | `sslKey` | `ssl_key_pem_file` | SSL etkinse gereken anahtar dosya |
    | `cname` | `ssl_cname` | SSL etkinse için CNAME |
    | `dnsNameLabel` | `dns_name_label` | Puanlama uç noktası için DNS ad etiketi. Belirtilmemişse, Puanlama uç noktası için benzersiz bir DNS ad etiketi oluşturulacaktır. |

    Aşağıdaki JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

    ```json
    {
        "computeType": "aci",
        "containerResourceRequirements":
        {
            "cpu": 0.5,
            "memoryInGB": 1.0
        },
        "authEnabled": true,
        "sslEnabled": false,
        "appInsightsEnabled": false
    }
    ```

+ **VS Code kullanma**

  [Modellerinizi vs Code ile dağıtmak](how-to-vscode-tools.md#deploy-and-manage-models) için, hızlı bir şekilde test etmek üzere bir aci kapsayıcısı oluşturmanız gerekmez, çünkü hızlı bir şekilde hareket halindeyken birlikte oluşturulur.

Daha fazla bilgi için başvuru belgeleri için bkz. [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) sınıfları.

### <a id="aks"></a>Azure Kubernetes hizmeti (DEVTEST & ÜRETIMI)

Mevcut bir AKS kümesi kullanmak veya Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturun.

<a id="deploy-aks"></a>

Zaten bağlı bir AKS kümeniz varsa, bu kümeye dağıtım yapabilirsiniz. Bir AKS kümesi oluşturmadıysanız veya eklemediyseniz, <a href="#create-attach-aks">Yeni BIR AKS kümesi oluşturmak</a>için işlemi izleyin.

+ **SDK 'Yı kullanma**

  ```python
  aks_target = AksCompute(ws,"myaks")
  # If deploying to a cluster configured for dev/test, ensure that it was created with enough
  # cores and memory to handle this deployment configuration. Note that memory is also used by
  # things such as dependencies and AML components.
  deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  print(service.get_logs())
  ```

+ **CLı 'yi kullanma**

    CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. Aks işlem hedefinin adıyla değiştirin `myaks` . Kayıt `mymodel:1` , kayıtlı modelin adı ve sürümü ile değiştirin. Bu `myservice` hizmete verilecek adla değiştirin:

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    `deploymentconfig.json` Belgedeki girişler, [akswebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)parametrelerine eşlenir. Aşağıdaki tabloda, JSON belgesindeki varlıklar ve yöntemin parametreleri arasındaki eşleme açıklanmaktadır:

    | JSON varlığı | Yöntem parametresi | Açıklama |
    | ----- | ----- | ----- |
    | `computeType` | NA | Bilgi işlem hedefi. AKS için değer olmalıdır `aks`. |
    | `autoScaler` | NA | Otomatik ölçeklendirme için yapılandırma öğelerini içerir. Bkz. otomatik Scaler tablosu. |
    | &emsp;&emsp;`autoscaleEnabled` | `autoscale_enabled` | Web hizmeti için otomatik ölçeklendirmenin etkinleştirilip etkinleştirilmeyeceğini belirtir. Eğer `numReplicas` , = ,yoksa, .`True` `0` `False` |
    | &emsp;&emsp;`minReplicas` | `autoscale_min_replicas` | Bu Web hizmetini otomatik ölçeklendirirken kullanılacak kapsayıcı sayısı alt sınırı. Varsayılan, `1`. |
    | &emsp;&emsp;`maxReplicas` | `autoscale_max_replicas` | Bu Web hizmetini otomatik ölçeklendirirken kullanılacak kapsayıcı sayısı üst sınırı. Varsayılan, `10`. |
    | &emsp;&emsp;`refreshPeriodInSeconds` | `autoscale_refresh_seconds` | Otomatik Scaler, bu Web hizmetini ölçeklendirmeye ne sıklıkta çalışır. Varsayılan, `1`. |
    | &emsp;&emsp;`targetUtilization` | `autoscale_target_utilization` | Otomatik Scaler 'nın bu Web hizmeti için bakımını denemesi gereken hedef kullanım (100 ' dan fazla). Varsayılan, `70`. |
    | `dataCollection` | NA | Veri toplama için yapılandırma öğelerini içerir. |
    | &emsp;&emsp;`storageEnabled` | `collect_model_data` | Web hizmeti için model veri toplamayı etkinleştirip etkinleştirmeyeceğinizi belirtir. Varsayılan, `False`. |
    | `authEnabled` | `auth_enabled` | Web hizmeti için kimlik doğrulamasının etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan, `True`. |
    | `containerResourceRequirements` | NA | Kapsayıcı için ayrılan CPU ve bellek için yapılandırma öğelerini içerir. |
    | &emsp;&emsp;`cpu` | `cpu_cores` | Bu Web hizmeti için ayrılacak CPU çekirdeklerinin sayısı. Olarak`0.1` |
    | &emsp;&emsp;`memoryInGB` | `memory_gb` | Bu Web hizmeti için ayrılacak bellek miktarı (GB cinsinden). Varsayılanını`0.5` |
    | `appInsightsEnabled` | `enable_app_insights` | Web hizmeti için Application Insights günlüğe kaydetmenin etkinleştirilip etkinleştirilmeyeceğini belirtir. Varsayılan, `False`. |
    | `scoringTimeoutMs` | `scoring_timeout_ms` | Web hizmetine yönelik Puanlama çağrılarına zorlamak için zaman aşımı. Varsayılan, `60000`. |
    | `maxConcurrentRequestsPerContainer` | `replica_max_concurrent_requests` | Bu Web hizmeti için düğüm başına en fazla eşzamanlı istek. Varsayılan, `1`. |
    | `maxQueueWaitMs` | `max_request_wait_time` | Bir 503 hatası döndürülmeden önce bir isteğin en uzun süre (milisaniye cinsinden) kalacağız. Varsayılan, `500`. |
    | `numReplicas` | `num_replicas` | Bu Web hizmeti için ayrılacak kapsayıcı sayısı. Varsayılan değer yoktur. Bu parametre ayarlanmamışsa otomatik olarak varsayılan olarak etkindir. |
    | `keys` | NA | Anahtarlar için yapılandırma öğelerini içerir. |
    | &emsp;&emsp;`primaryKey` | `primary_key` | Bu Web hizmeti için kullanılacak birincil bir kimlik doğrulama anahtarı |
    | &emsp;&emsp;`secondaryKey` | `secondary_key` | Bu Web hizmeti için kullanılacak ikincil bir kimlik doğrulama anahtarı |
    | `gpuCores` | `gpu_cores` | Bu Web hizmeti için ayrılacak GPU çekirdeklerinin sayısı. Varsayılan değer 1 ' dir. |
    | `livenessProbeRequirements` | NA | Lizleştirme araştırma gereksinimleri için yapılandırma öğelerini içerir. |
    | &emsp;&emsp;`periodSeconds` | `period_seconds` | Lizleştirme araştırması gerçekleştirmek için ne sıklıkla (saniye cinsinden). Varsayılan değer 10 saniyedir. En küçük değer 1 ' dir. |
    | &emsp;&emsp;`initialDelaySeconds` | `initial_delay_seconds` | Ebilzleştirme araştırmadan önce kapsayıcının başladıktan sonra geçmesi gereken saniye sayısı. Varsayılan değer 310 ' dir |
    | &emsp;&emsp;`timeoutSeconds` | `timeout_seconds` | Lizleştirme araştırması zaman aşımına uğramadan geçen saniye sayısı. Varsayılan değer 2 saniyedir. En düşük değer 1 ' dir |
    | &emsp;&emsp;`successThreshold` | `success_threshold` | Hatalı olduktan sonra başarılı olarak değerlendirilme araştırması için en düşük ardışık başarı. Varsayılan değer 1 ' dir. En küçük değer 1 ' dir. |
    | &emsp;&emsp;`failureThreshold` | `failure_threshold` | Bir pod başlatıldığında ve elek araştırması başarısız olduğunda Kubernetes, başlamadan önce failureThreshold sürelerini dener. Varsayılan olarak 3 ' e döner. En küçük değer 1 ' dir. |
    | `namespace` | `namespace` | Web hizmeti 'nin dağıtıldığı Kubernetes ad alanı. 63 ' e kadar küçük alfasayısal (' A'-'z ', ' 0 '-' 9 ') ve tire ('-') karakterleri. İlk ve son karakterler kısa çizgi olamaz. |

    Aşağıdaki JSON, CLı ile kullanılacak örnek bir dağıtım yapılandırması örneğidir:

    ```json
    {
        "computeType": "aks",
        "autoScaler":
        {
            "autoscaleEnabled": true,
            "minReplicas": 1,
            "maxReplicas": 3,
            "refreshPeriodInSeconds": 1,
            "targetUtilization": 70
        },
        "dataCollection":
        {
            "storageEnabled": true
        },
        "authEnabled": true,
        "containerResourceRequirements":
        {
            "cpu": 0.5,
            "memoryInGB": 1.0
        }
    }
    ```

+ **VS Code kullanma**

  Ayrıca, [vs Code uzantısı aracılığıyla aks 'e de dağıtım](how-to-vscode-tools.md#deploy-and-manage-models)yapabilirsiniz, ancak aks kümelerini önceden yapılandırmanız gerekecektir.

[Akswebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) başvurusunda aks dağıtımı ve otomatik ölçeklendirme hakkında daha fazla bilgi edinin.

#### Yeni bir AKS kümesi oluşturma<a id="create-attach-aks"></a>
**Tahmini süre**: Yaklaşık 20 dakika.

AKS kümesi oluşturma veya iliştirme, çalışma alanınız için tek seferlik bir işlemdir. Bu kümeye birden çok dağıtımlar için yeniden kullanabilirsiniz. Kümeyi veya onu içeren kaynak grubunu silerseniz, bir dahaki sefer dağıtmanız gerektiğinde yeni bir küme oluşturmanız gerekir. Çalışma alanınıza eklenmiş birden çok AKS kümeniz olabilir.

Geliştirme, doğrulama ve test için bir aks kümesi oluşturmak istiyorsanız, kullanırken `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py)ayarlanır. Bu ayarla oluşturulmuş bir küme yalnızca bir düğüme sahip olur.

> [!IMPORTANT]
> Ayar `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` , üretim trafiğini işlemek için uygun olmayan bir aks kümesi oluşturur. Çıkarım süreleri, üretim için oluşturulmuş bir kümeden daha uzun olabilir. Hata toleransı geliştirme ve test kümeleri için de garanti edilmez.
>
> Geliştirme ve test için oluşturulan kümelerin en az iki sanal CPU kullanması önerilir.

Aşağıdaki örnekte, yeni bir Azure Kubernetes hizmet kümesinin nasıl oluşturulacağı gösterilmektedir:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksComputee.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Azure Machine Learning SDK dışında bir AKS kümesi oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
* [AKS kümesi oluşturma](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [AKS kümesi oluşturma (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

`cluster_purpose` Parametresi hakkında daha fazla bilgi için, bkz. [akscompute. clusteramaç](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) başvurusu.

> [!IMPORTANT]
> İçin [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), agent_count ve vm_size için özel değerler seçerseniz, vm_size ile çarpılan agent_count 'in 12 sanal CPU 'ya eşit veya daha büyük olduğundan emin olmanız gerekir. Örneğin, 4 sanal CPU içeren bir vm_size "Standard_D3_v2" kullanırsanız, bir agent_count 3 veya daha büyük seçmeniz gerekir.
>
> Azure Machine Learning SDK, bir AKS kümesini ölçeklendirmeye yönelik destek sağlamaz. Kümedeki düğümleri ölçeklendirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. Kümenin VM boyutunu değil, yalnızca düğüm sayısını değiştirebilirsiniz.

#### <a name="attach-an-existing-aks-cluster"></a>Mevcut bir AKS kümesini iliştirme
**Tahmini süre:** Yaklaşık 5 dakika.

Azure aboneliğinizde zaten AKS kümeniz varsa ve sürüm 1.12. # # ise, görüntünüzü dağıtmak için kullanabilirsiniz.

> [!WARNING]
> Bir çalışma alanına aks kümesi eklerken, `cluster_purpose` parametresini ayarlayarak kümeyi nasıl kullanacağınızı tanımlayabilirsiniz.
>
> `cluster_purpose` Parametresini ayarlamayın veya ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, kümede en az 12 sanal CPU kullanılabilir olmalıdır.
>
> Ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, kümenin 12 sanal CPU 'ya sahip olması gerekmez. Ancak, geliştirme/test için yapılandırılan bir küme, üretim düzeyi trafiğe uygun olmayacaktır ve çıkarım sürelerini artırabilir.

Aşağıdaki kod, var olan bir AKS 1.12. # # kümesini çalışma alanınıza nasıl ekleyebilirim gösterilmektedir:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

Hakkında `attack_configuration()`daha fazla bilgi için, bkz. [akscompute. attach_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-) başvurusu.

`cluster_purpose` Parametresi hakkında daha fazla bilgi için, bkz. [akscompute. clusteramaç](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) başvurusu.

## <a name="consume-web-services"></a>Web hizmetlerini kullanma

Dağıtılan her Web hizmeti, çeşitli programlama dillerinde istemci uygulamaları oluşturabilmeniz için bir REST API sağlar. Hizmetiniz için kimlik doğrulamasını etkinleştirdiyseniz, istek üst bilgisinde belirteç olarak bir hizmet anahtarı sağlamanız gerekir.

### <a name="request-response-consumption"></a>İstek-yanıt tüketimi

Aşağıda, Python 'da hizmetinizin nasıl çağrılcağına dair bir örnek verilmiştir:
```python
import requests
import json

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Daha fazla bilgi için bkz. [WebServices kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md).


### <a id="azuremlcompute"></a>Toplu çıkarım
Azure Machine Learning Işlem hedefleri Azure Machine Learning hizmeti tarafından oluşturulur ve yönetilir. Bunlar, Azure Machine Learning işlem hatlarından toplu tahmin için kullanılabilirler.

Azure Machine Learning Işlem ile Batch çıkarımı hakkında bir anlatım için [Batch öngörülerini çalıştırma](how-to-run-batch-predictions.md) makalesini okuyun.

### <a id="iotedge"></a>IoT Edge çıkarımı
Kenara dağıtım desteği önizleme aşamasındadır. Daha fazla bilgi için [IoT Edge modül olarak Azure Machine Learning dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) makalesine bakın.


## <a id="update"></a>Web hizmetlerini güncelleştirme

Yeni bir model oluşturduğunuzda, yeni modeli kullanmak istediğiniz her hizmeti el ile güncelleştirmeniz gerekir. Web hizmetini güncelleştirmek için `update` yöntemi. Aşağıdaki kod, Web hizmetinin yeni bir model kullanmak üzere nasıl güncelleşdiğini göstermektedir:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# Update to new model(s)
service.update(models = [new_model])
print(service.state)
print(service.get_logs())
```

## <a name="continuous-model-deployment"></a>Sürekli model dağıtımı 

[Azure DevOps](https://azure.microsoft.com/services/devops/)için Machine Learning uzantısını kullanarak, modelleri sürekli olarak dağıtabilirsiniz. Azure DevOps için Machine Learning uzantısını kullanarak, yeni bir Machine Learning modeli Azure Machine Learning hizmeti çalışma alanında kaydedildiğinde bir dağıtım işlem hattı tetikleyebilirsiniz. 

1. [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops)için kaydolun, bu, uygulamanızın her türlü platforma/buluta sürekli olarak tümleştirilmesini ve teslimini sağlar. Azure Pipelines, [ml işlem hatlarından farklıdır](concept-ml-pipelines.md#compare). 

1. [Azure DevOps projesi oluşturun.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. [Azure Pipelines için Machine Learning uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) yükler 

1. Tüm yapıtlarınıza erişmek için Azure Machine Learning hizmeti çalışma alanınıza bir hizmet sorumlusu bağlantısı kurmak için __hizmet bağlantıları__ 'nı kullanın. Proje Ayarları ' na gidin, hizmet bağlantıları ' na tıklayın ve Azure Resource Manager ' ı seçin.

    ![hizmet-bağlantıyı görüntüle](media/how-to-deploy-and-where/view-service-connection.png) 

1. __Kapsam düzeyi__ olarak AzureMLWorkspace tanımlayın ve sonraki parametreleri girin.

    ![Görünüm-Azure-Resource-Manager](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Sonra, Azure Pipelines kullanarak makine öğrenimi modelinizi sürekli olarak dağıtmak için, işlem hatları altında __yayın__' ı seçin. Yeni bir yapıt ekleyin, AzureML model yapıtı ve önceki adımda oluşturulan hizmet bağlantısını seçin. Bir dağıtımı tetiklemek için modeli ve sürümü seçin. 

    ![Select-AzureMLmodel-yapay](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)

1. Model yapıtında model tetikleyiciyi etkinleştirin. Tetikleyiciyi etkinleştirerek, belirtilen sürüm her seferinde (ör. Bu modelin en yeni sürümü) çalışma alanınıza kayıt yaptırdığında bir Azure DevOps sürümü işlem hattı tetiklenir. 

    ![Enable-model-tetikleyici](media/how-to-deploy-and-where/set-modeltrigger.png)

Örnek projeler ve örnekler için [MLOps deposuna](https://github.com/Microsoft/MLOps) göz atın

## <a name="clean-up-resources"></a>Kaynakları temizleme
Dağıtılmış bir web hizmetini silmek için kullanın `service.delete()`.
Kayıtlı bir model silmek için kullanın `model.delete()`.

Daha fazla bilgi için bkz. [WebService. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--)ve [model. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--)için başvuru belgeleri.

## <a name="next-steps"></a>Sonraki adımlar
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)

