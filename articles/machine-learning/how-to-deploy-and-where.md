---
title: Modellerin nasıl ve nereye dağıtılacağı
titleSuffix: Azure Machine Learning
description: Azure Container Instances, Azure Kubernetes hizmeti, Azure IoT Edge ve alan programlanabilir kapı dizileri dahil olmak üzere Azure Machine Learning modellerinizi nasıl ve nereye dağıtacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/12/2020
ms.custom: seoapril2019, tracking-python
ms.openlocfilehash: bc9ab6ddf3a9032fd1919b70d830f0d65cdc06ed
ms.sourcegitcommit: 1383842d1ea4044e1e90bd3ca8a7dc9f1b439a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84817992"
---
# <a name="deploy-models-with-azure-machine-learning"></a>Azure Machine Learning ile modelleri dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Machine Learning modelinizi bir Web hizmeti olarak Azure bulutu 'nda veya Azure IoT Edge cihazlarda dağıtmayı öğrenin.

İş akışı modelinize [nereden dağıttığınız](#target) her ne kadar benzerdir:

1. Modeli kaydedin.
1. Dağıtım için hazırlayın. (Varlıkları, kullanımı, işlem hedefini belirtin.)
1. Modeli işlem hedefine dağıtın.
1. Web hizmeti olarak da bilinen dağıtılan modeli test edin.

Dağıtım iş akışında yer alan kavramlar hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri yönetme, dağıtma ve izleme](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Ön koşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

- Bir model. Eğitilen bir modeliniz yoksa, [Bu öğreticide](https://aka.ms/azml-deploy-cloud)verilen model ve bağımlılık dosyalarını kullanabilirsiniz.

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

## <a name="connect-to-your-workspace"></a>Çalışma alanınıza bağlanma

Aşağıdaki kod, yerel geliştirme ortamında önbelleğe alınan bilgileri kullanarak bir Azure Machine Learning çalışma alanına bağlanmayı gösterir:

+ **SDK’yı kullanarak**

   ```python
   from azureml.core import Workspace
   ws = Workspace.from_config(path=".file-path/ws_config.json")
   ```

  Bir çalışma alanına bağlanmak için SDK 'Yı kullanma hakkında daha fazla bilgi için bkz. [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace) belgeleri.

+ **CLı 'yi kullanma**

   CLı kullanırken, `-w` `--workspace-name` komut için çalışma alanını belirtmek üzere veya parametresini kullanın.

+ **Visual Studio Code’u kullanma**

   Visual Studio Code kullandığınızda, bir grafik arabirim kullanarak çalışma alanını seçersiniz. Daha fazla bilgi için bkz. Visual Studio Code uzantısı belgelerindeki [modelleri dağıtma ve yönetme](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model) .

## <a name="register-your-model"></a><a id="registermodel"></a>Modelinizi kaydetme

Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları çalışma alanına tek bir model olarak kaydedebilirsiniz. Dosyaları kaydettikten sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kaydettiğiniz tüm dosyaları alabilirsiniz.

> [!TIP]
> Bir modeli kaydettiğinizde, bir bulut konumunun (bir eğitim çalıştırmasında) ya da yerel bir dizinin yolunu sağlarsınız. Bu yol, kayıt işleminin bir parçası olarak karşıya yüklenecek dosyaları bulmak için yeterlidir. Giriş betiğinde kullanılan yol ile eşleşmesi gerekmez. Daha fazla bilgi için bkz. [giriş betiğinizdeki model dosyalarını bulma](#load-model-files-in-your-entry-script).

Machine Learning modelleri Azure Machine Learning çalışma alanınıza kaydedilir. Model Azure Machine Learning veya herhangi bir yerden gelebilir. Bir modeli kaydederken, isteğe bağlı olarak model hakkında meta veriler sağlayabilirsiniz. `tags` `properties` Model kaydına uyguladığınız ve sözlükleri, modelleri filtrelemek için kullanılabilir.

Aşağıdaki örneklerde bir modelin nasıl kaydedileceği gösterilmektedir.

### <a name="register-a-model-from-an-experiment-run"></a>Deneme çalıştırmasında bir modeli kaydetme

Bu bölümdeki kod parçacıkları, bir eğitim çalıştırmasının nasıl bir model kaydedeceğinizi göstermektedir:

> [!IMPORTANT]
> Bu kod parçacıklarını kullanmak için, daha önce bir eğitim çalıştırması gerçekleştirdiyseniz ve `Run` nesneye (SDK örneği) veya çalıştırma kimliği değerine (CLI örneği) erişiminizin olması gerekir. Eğitim modelleri hakkında daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md).

+ **SDK’yı kullanarak**

  Bir modeli eğitebilmeniz için SDK 'yı kullandığınızda, modeli eğitidiğinize bağlı olarak bir [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py) nesnesi ya da bir [oto mlrun](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun) nesnesi alabilirsiniz. Her nesne, bir deneme çalıştırması tarafından oluşturulan bir modeli kaydetmek için kullanılabilir.

  + Bir nesneden model kaydetme `azureml.core.Run` :
 
    ```python
    model = run.register_model(model_name='sklearn_mnist',
                               tags={'area': 'mnist'},
                               model_path='outputs/sklearn_mnist_model.pkl')
    print(model.name, model.id, model.version, sep='\t')
    ```

    `model_path`Parametresi, modelin bulut konumunu ifade eder. Bu örnekte, tek bir dosyanın yolu kullanılır. Model kaydına birden çok dosya eklemek için, `model_path` dosyaları içeren bir klasörün yoluna ayarlayın. Daha fazla bilgi için bkz [. Run. register_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-) belgeleri.

  + Bir nesneden model kaydetme `azureml.train.automl.run.AutoMLRun` :

    ```python
        description = 'My AutoML Model'
        model = run.register_model(description = description,
                                   tags={'area': 'mnist'})

        print(run.model_id)
    ```

    Bu örnekte, `metric` ve `iteration` parametreleri belirtilmedi, bu nedenle en iyi birincil ölçüye sahip yineleme kaydedilir. `model_id`Bir model adı yerine çalıştırağından döndürülen değer kullanılır.

    Daha fazla bilgi için bkz. [oto Mlrun. register_model](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun#register-model-model-name-none--description-none--tags-none--iteration-none--metric-none-) belgeleri.

+ **CLı 'yi kullanma**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid --tag area=mnist
  ```

  [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

  `--asset-path`Parametresi, modelin bulut konumunu ifade eder. Bu örnekte, tek bir dosyanın yolu kullanılır. Model kaydına birden çok dosya eklemek için, `--asset-path` dosyaları içeren bir klasörün yoluna ayarlayın.

+ **Visual Studio Code’u kullanma**

  [Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model) uzantısını kullanarak model dosyalarını veya klasörleri kullanarak modelleri kaydedin.

### <a name="register-a-model-from-a-local-file"></a>Yerel dosyadan model kaydetme

Modelin yerel yolunu sağlayarak bir modeli kaydedebilirsiniz. Bir klasörün ya da tek bir dosyanın yolunu sağlayabilirsiniz. Bu yöntemi, Azure Machine Learning eğitilen ve daha sonra indirilen modelleri kaydetmek için kullanabilirsiniz. Bu yöntemi, Azure Machine Learning dışında eğitilen modelleri kaydetmek için de kullanabilirsiniz.

[!INCLUDE [trusted models](../../includes/machine-learning-service-trusted-model.md)]

+ **SDK ve ONNX kullanma**

    ```python
    import os
    import urllib.request
    from azureml.core.model import Model
    # Download model
    onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
    urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
    os.system('tar xvzf mnist.tar.gz')
    # Register model
    model = Model.register(workspace = ws,
                            model_path ="mnist/model.onnx",
                            model_name = "onnx_mnist",
                            tags = {"onnx": "demo"},
                            description = "MNIST image classification CNN from ONNX Model Zoo",)
    ```

  Model kaydına birden çok dosya eklemek için, `model_path` dosyaları içeren bir klasörün yoluna ayarlayın.

+ **CLı 'yi kullanma**

  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  Model kaydına birden çok dosya eklemek için, `-p` dosyaları içeren bir klasörün yoluna ayarlayın.

**Tahmini süre**: yaklaşık 10 saniye.

Daha fazla bilgi için [model sınıfına](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py)yönelik belgelere bakın.

Azure Machine Learning dışında eğitilen modellerle çalışma hakkında daha fazla bilgi için bkz. [var olan bir modeli dağıtma](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="single-versus-multi-model-endpoints"></a>Tek ve çok modelli uç noktalar
Azure ML, tek bir uç noktanın arkasında tek veya birden çok modelin dağıtılmasını destekler.

Birden çok modeli barındırmak için çok modelli uç noktalar paylaşılan bir kapsayıcı kullanır. Bu, ek yük maliyetlerini azaltmaya, kullanımı iyileştirmenize ve modülleri Kümelemeler 'e zincirlemenize olanak sağlar. Dağıtım betiğinizdeki belirttiğiniz modeller bağlanır ve hizmet veren kapsayıcısının diskinde kullanılabilir hale getirilir. bunları, Puanlama süresi içinde istenen modele göre isteğe bağlı olarak ve puan ile yükleyebilirsiniz.

Tek kapsayıcılı bir uç noktanın arkasında birden çok modelin nasıl kullanılacağını gösteren bir E2E örneği için, [Bu örneğe](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/deploy-multi-model) bakın

## <a name="prepare-to-deploy"></a>Dağıtmaya hazırlanma

Modeli bir hizmet olarak dağıtmak için aşağıdaki bileşenlere ihtiyacınız vardır:

* **Çıkarım ortamını tanımlayın**. Bu ortam, modelinizi çıkarım için çalıştırmak için gereken bağımlılıkları kapsüller.
* **Puanlama kodu tanımlayın**. Bu betik istekleri kabul eder, modeli kullanarak istekleri puan eder ve sonuçları döndürür.
* **Çıkarım yapılandırmasını tanımlayın**. Çıkarım yapılandırması, modeli bir hizmet olarak çalıştırmak için gereken ortam yapılandırması, giriş betiği ve diğer bileşenleri belirtir.

Gerekli bileşenlere sahip olduktan sonra, kendi CPU ve bellek gereksinimlerini anlamak üzere modelinizi dağıtmanın bir sonucu olarak oluşturulacak hizmetin profilini oluşturabilirsiniz.

### <a name="1-define-inference-environment"></a>1. çıkarım ortamı tanımla

Bir çıkarım yapılandırması, modelinizi içeren Web hizmeti ayarlamayı açıklar. Modeli dağıtırken daha sonra kullanılır.

Çıkarım yapılandırması, dağıtımınız için gereken yazılım bağımlılıklarını tanımlamak için Azure Machine Learning ortamlar kullanır. Ortamlar, eğitim ve dağıtım için gereken yazılım bağımlılıklarını oluşturmanıza, yönetmenize ve yeniden kullanmanıza olanak tanır. Özel bağımlılık dosyalarından bir ortam oluşturabilir veya seçkin Azure Machine Learning ortamlarından birini kullanabilirsiniz. Aşağıdaki YAML, çıkarım için Conda Dependencies bir dosya örneğidir. Modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerdiğinden, bir PIP >= 1.0.45 ile azureml-varsayılan değeri, bir PIP bağımlılığı olarak belirtmeniz gerektiğini unutmayın. Otomatik şema oluşturmayı kullanmak istiyorsanız, giriş betiğinizin de paketleri içeri aktarması gerekir `inference-schema` .

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - scikit-learn=0.20.0
  - pip:
      # You must list azureml-defaults as a pip dependency
    - azureml-defaults>=1.0.45
    - inference-schema[numpy-support]
```

> [!IMPORTANT]
> Eğer koda ve PIP (PyPi) ile bağımlılığı varsa Microsoft, Conda 'in, daha güvenilir bir şekilde yükleme yapan önceden oluşturulmuş ikili dosyalarla birlikte geldiğinden, Conda sürümünün kullanılmasını önerir.
>
> Daha fazla bilgi için bkz. [Conda ve PIP 'Yi anlama](https://www.anaconda.com/understanding-conda-and-pip/).
>
> Bağımlılarınızın Conda ile kullanılabilir olup olmadığını denetlemek için `conda search <package-name>` komutunu kullanın ya da ve ' de paket dizinlerini kullanın [https://anaconda.org/anaconda/repo](https://anaconda.org/anaconda/repo) [https://anaconda.org/conda-forge/repo](https://anaconda.org/conda-forge/repo) .

Bir ortam nesnesi oluşturmak ve daha sonra kullanmak üzere çalışma alanınıza kaydetmek için bağımlılıklar dosyasını kullanabilirsiniz:

```python
from azureml.core.environment import Environment
myenv = Environment.from_conda_specification(name = 'myenv',
                                             file_path = 'path-to-conda-specification-file'
myenv.register(workspace=ws)
```

### <a name="2-define-scoring-code"></a><a id="script"></a>2. Puanlama kodu tanımlayın

Giriş betiği, dağıtılan bir web hizmetine gönderilen verileri alır ve modele geçirir. Ardından model tarafından döndürülen yanıtı alır ve bunu istemciye döndürür. *Betik, modelinize özeldir*. Modelin beklediği ve döndürdüğü verileri anlaması gerekir.

Betik, modeli yükleyen ve çalıştıran iki işlev içerir:

* `init()`: Genellikle, bu işlev modeli genel bir nesneye yükler. Bu işlev, Web hizmetiniz için Docker kapsayıcısı başlatıldığında yalnızca bir kez çalıştırılır.

* `run(input_data)`: Bu işlev, giriş verilerine göre bir değeri tahmin etmek için modeli kullanır. Çalıştırmanın girişleri ve çıkışlarında serileştirme ve seriden çıkarma için normal olarak JSON kullanılır. Ham ikili verilerle de çalışabilirsiniz. Verileri modele göndermeden veya istemciye döndürmeden önce dönüştürebilirsiniz.

#### <a name="load-model-files-in-your-entry-script"></a>Giriş betiğinizdeki model dosyalarını yükleme

Giriş betiğinizdeki modelleri bulmanın iki yolu vardır:
* `AZUREML_MODEL_DIR`: Model konumunun yolunu içeren bir ortam değişkeni.
* `Model.get_model_path`: Kayıtlı model adı kullanılarak model dosyasının yolunu döndüren bir API.

##### <a name="azureml_model_dir"></a>AZUREML_MODEL_DIR

AZUREML_MODEL_DIR, hizmet dağıtımı sırasında oluşturulan bir ortam değişkenidir. Dağıtılan model konumunu bulmak için bu ortam değişkenini kullanabilirsiniz.

Aşağıdaki tabloda, dağıtılan modellerin sayısına bağlı olarak AZUREML_MODEL_DIR değeri açıklanmaktadır:

| Dağıtım | Ortam değişkeni değeri |
| ----- | ----- |
| Tek model | Modeli içeren klasörün yolu. |
| Birden çok model | Tüm modelleri içeren klasörün yolu. Modeller bu klasörde ad ve sürüm tarafından bulunur ( `$MODEL_NAME/$VERSION` ) |

Model kaydı ve dağıtımı sırasında modeller AZUREML_MODEL_DIR yoluna yerleştirilir ve özgün dosya adları korunur.

Giriş betiğinizdeki bir model dosyasının yolunu almak için, ortam değişkenini aradığınız dosya yoluyla birleştirin.

**Tek model örneği**
```python
# Example when the model is a file
model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_regression_model.pkl')

# Example when the model is a folder containing a file
file_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'my_model_folder', 'sklearn_regression_model.pkl')
```

**Birden çok model örneği**

Bu senaryoda, çalışma alanına iki model kaydedilir:

* `my_first_model`: Bir dosya ( `my_first_model.pkl` ) içerir ve yalnızca bir sürüm () vardır `1` .
* `my_second_model`: Bir dosya ( `my_second_model.pkl` ) içerir ve iki sürüm vardır: `1` ve `2` .

Hizmet dağıtıldığında her iki model de dağıtım işleminde sağlanır:

```python
first_model = Model(ws, name="my_first_model", version=1)
second_model = Model(ws, name="my_second_model", version=2)
service = Model.deploy(ws, "myservice", [first_model, second_model], inference_config, deployment_config)
```

Hizmeti barındıran Docker görüntüsünde, `AZUREML_MODEL_DIR` ortam değişkeni modellerin bulunduğu dizini içerir.
Bu dizinde, modellerin her biri dizin yolunda bulunur `MODEL_NAME/VERSION` . `MODEL_NAME`, Kayıtlı modelin adıdır ve `VERSION` modelin sürümüdür. Kayıtlı modeli oluşturan dosyalar bu dizinlerde depolanır.

Bu örnekte, yollar `$AZUREML_MODEL_DIR/my_first_model/1/my_first_model.pkl` ve olur `$AZUREML_MODEL_DIR/my_second_model/2/my_second_model.pkl` .


```python
# Example when the model is a file, and the deployment contains multiple models
first_model_name = 'my_first_model'
first_model_version = '1'
first_model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), first_model_name, first_model_version, 'my_first_model.pkl')
second_model_name = 'my_second_model'
second_model_version = '2'
second_model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), second_model_name, second_model_version, 'my_second_model.pkl')
```

##### <a name="get_model_path"></a>get_model_path

Bir modeli kaydettiğinizde, kayıt defterinde modeli yönetmek için kullanılan bir model adı sağlarsınız. Bu adı model dosyasının veya yerel dosya sistemindeki dosyaların yolunu almak için [model. get_model_path ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) yöntemiyle birlikte kullanırsınız. Bir klasörü veya dosya koleksiyonunu kaydettiğinizde, bu API bu dosyaları içeren dizinin yolunu döndürür.

Bir modeli kaydettiğinizde, buna bir ad verirsiniz. Ad, modelin yerel olarak ya da hizmet dağıtımı sırasında yerleştirildiği yere karşılık gelir.

#### <a name="optional-define-model-web-service-schema"></a>Seçim Model Web hizmeti şemasını tanımla

Web hizmetiniz için otomatik olarak bir şema oluşturmak üzere, tanımlı tür nesnelerinden biri için kurucudaki girişin ve/veya çıktının bir örneğini sağlayın. Tür ve örnek, otomatik olarak şemayı oluşturmak için kullanılır. Azure Machine Learning, dağıtım sırasında Web hizmeti için bir [Openapı](https://swagger.io/docs/specification/about/) (Swagger) belirtimi oluşturur.

Bu türler Şu anda destekleniyor:

* `pandas`
* `numpy`
* `pyspark`
* Standart Python nesnesi

Şema oluşturmayı kullanmak için, `inference-schema` Bağımlılıklar dosyanıza açık kaynaklı paketi ekleyin. Bu paket hakkında daha fazla bilgi için bkz [https://github.com/Azure/InferenceSchema](https://github.com/Azure/InferenceSchema) .. `input_sample` `output_sample` Web hizmetinin istek ve yanıt biçimlerini temsil eden ve değişkenlerinde giriş ve çıkış örnek biçimlerini tanımlayın. Bu örnekleri işlevindeki giriş ve çıkış işlevi Dekoratörleri içinde kullanın `run()` . Aşağıdaki scikit-örnek, şema oluşturmayı kullanır.

##### <a name="example-entry-script"></a>Örnek giriş betiği

Aşağıdaki örnek, JSON verilerinin nasıl kabul edileceği ve geri dönebileceğinizi göstermektedir:

```python
#Example: scikit-learn and Swagger
import json
import numpy as np
import os
from sklearn.externals import joblib
from sklearn.linear_model import Ridge

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    # AZUREML_MODEL_DIR is an environment variable created during deployment. Join this path with the filename of the model file.
    # It holds the path to the directory that contains the deployed model (./azureml-models/$MODEL_NAME/$VERSION).
    # If there are multiple models, this value is the path to the directory containing all deployed models (./azureml-models).
    # Alternatively: model_path = Model.get_model_path('sklearn_mnist')
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_mnist_model.pkl')
    # Deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = np.array([[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]])
output_sample = np.array([3726.995])


@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

##### <a name="power-bi-compatible-endpoint"></a>Power BI uyumlu uç nokta 

Aşağıdaki örnek, veri çerçevesini kullanarak giriş verilerinin bir sözlük olarak nasıl tanımlanacağını göstermektedir `<key: value>` . Bu yöntem, dağıtılan Web hizmetinin Power BI tüketmesi için desteklenir. ([Power BI Web hizmetini kullanma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/power-bi/service-machine-learning-integration).)

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
    # Replace filename if needed.
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'model_file.pkl')
    # Deserialize the model file back into a sklearn model.
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    # This is a decimal type sample. Use the data type that reflects this column in your data.
    "input_name_1": 5.1,
    # This is a string type sample. Use the data type that reflects this column in your data.
    "input_name_2": "value2",
    # This is an integer type sample. Use the data type that reflects this column in your data.
    "input_name_3": 3
}])

# This is an integer type sample. Use the data type that reflects the expected result.
output_sample = np.array([0])

# To indicate that we support a variable length of data input,
# set enforce_shape=False
@input_schema('data', PandasParameterType(input_sample, enforce_shape=False))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

Daha fazla örnek için aşağıdaki betiklerine bakın:

* [PyTorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch)
* [TensorFlow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/tensorflow)
* [Keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* [AutoML](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features)
* [ONNX](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* [İkili veriler](#binary)
* [CORS](#cors)

### <a name="3-define-inference-configuration"></a><a id="script"></a>3. çıkarım yapılandırmasını tanımlayın
    
Aşağıdaki örnek, çalışma alanınızdan bir ortamı yüklemeyi ve ardından çıkarım yapılandırmasıyla kullanmayı gösterir:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig


myenv = Environment.get(workspace=ws, name='myenv', version='1')
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                   environment=myenv)
```

Ortamlar hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md).

Çıkarım yapılandırması hakkında daha fazla bilgi için bkz. [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfı belgeleri.

Bir çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md).

#### <a name="cli-example-of-inferenceconfig"></a>Inenceconfig CLı örneği

[!INCLUDE [inference config](../../includes/machine-learning-service-inference-config.md)]

Aşağıdaki komut, CLı kullanarak bir modelin nasıl dağıtılacağını göstermektedir:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

Bu örnekte, yapılandırma aşağıdaki ayarları belirtir:

* Model Python gerektirir.
* Dağıtılan hizmete gönderilen Web isteklerini işlemek için kullanılan [giriş betiği](#script).
* Çıkarımı için gereken Python paketlerini açıklayan Conda dosyası.

Bir çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md).

### <a name="4-optional-profile-your-model-to-determine-resource-utilization"></a><a id="profilemodel"></a>4. (isteğe bağlı) kaynak kullanımını belirlemede modelinize profil

Modelinize kaydolduktan ve dağıtımı için gereken diğer bileşenleri hazırladıktan sonra, dağıtılan hizmete gereken CPU ve belleği belirleyebilirsiniz. Profil oluşturma, modelinizi çalıştıran hizmeti sınar ve CPU kullanımı, bellek kullanımı ve yanıt gecikme süresi gibi bilgileri döndürür. Ayrıca, CPU ve bellek için kaynak kullanımına göre bir öneri sağlar.

Modelinizin profilini oluşturmanız için şunlar gerekir:
* Kayıtlı bir model.
* Giriş betiğinizi ve çıkarım ortam tanımınızı temel alan bir çıkarım yapılandırması.
* Her satırın örnek istek verilerini temsil eden bir dize içerdiği tek sütunlu tablosal veri kümesi.

> [!IMPORTANT]
> Bu noktada, yalnızca istek verilerinin dize olmasını bekleyen hizmetlerin profilini oluşturmayı destekliyoruz, örneğin: dize serileştirilmiş JSON, metin, dize serileştirilmiş görüntü, vb. Veri kümesinin (dize) her satırının içeriği HTTP isteğinin gövdesine konur ve Puanlama için modeli kapsüllemek için hizmete gönderilir.

Aşağıda, gelen istek verilerinin serileştirilmiş JSON içermesini bekleyen bir hizmetin profilini oluşturmak için bir giriş veri kümesi nasıl oluşturabileceğiniz hakkında bir örnek verilmiştir. Bu durumda, aynı istek verisi içeriğine sahip bir veri kümesi tabanlı 100 örnekleri oluşturduk. Gerçek dünyada senaryolarda, özellikle model kaynak kullanımınız/davranışı girişe bağımlıysa, çeşitli girişler içeren daha büyük veri kümeleri kullanmanızı öneririz.

```python
import json
from azureml.core import Datastore
from azureml.core.dataset import Dataset
from azureml.data import dataset_type_definitions

input_json = {'data': [[1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
                       [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]]}
# create a string that can be utf-8 encoded and
# put in the body of the request
serialized_input_json = json.dumps(input_json)
dataset_content = []
for i in range(100):
    dataset_content.append(serialized_input_json)
dataset_content = '\n'.join(dataset_content)
file_name = 'sample_request_data.txt'
f = open(file_name, 'w')
f.write(dataset_content)
f.close()

# upload the txt file created above to the Datastore and create a dataset from it
data_store = Datastore.get_default(ws)
data_store.upload_files(['./' + file_name], target_path='sample_request_data')
datastore_path = [(data_store, 'sample_request_data' +'/' + file_name)]
sample_request_data = Dataset.Tabular.from_delimited_files(
    datastore_path, separator='\n',
    infer_column_types=True,
    header=dataset_type_definitions.PromoteHeadersBehavior.NO_HEADERS)
sample_request_data = sample_request_data.register(workspace=ws,
                                                   name='sample_request_data',
                                                   create_new_version=True)
```

Örnek istek verilerini içeren veri kümesine sahip olduktan sonra bir çıkarım yapılandırması oluşturun. Çıkarım yapılandırması, score.py ve ortam tanımına dayalıdır. Aşağıdaki örnek, çıkarım yapılandırmasını oluşturmayı ve profil oluşturmayı çalıştırmayı gösterir:

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.dataset import Dataset


model = Model(ws, id=model_id)
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                   environment=myenv)
input_dataset = Dataset.get_by_name(workspace=ws, name='sample_request_data')
profile = Model.profile(ws,
            'unique_name',
            [model],
            inference_config,
            input_dataset=input_dataset)

profile.wait_for_completion(True)

# see the result
details = profile.get_details()
```

Aşağıdaki komut, CLı kullanarak bir modelin profilini oluşturmayı gösterir:

```azurecli-interactive
az ml model profile -g <resource-group-name> -w <workspace-name> --inference-config-file <path-to-inf-config.json> -m <model-id> --idi <input-dataset-id> -n <unique-name>
```

> [!TIP]
> Profil oluşturma tarafından döndürülen bilgileri kalıcı hale getirmek için model için Etiketler veya özellikler kullanın. Etiketlerin veya özelliklerin kullanılması, verileri model kayıt defteri 'nde modeliyle depolar. Aşağıdaki örneklerde ve bilgileri içeren yeni bir etiket ekleme gösterilmektedir `requestedCpu` `requestedMemoryInGb` :
>
> ```python
> model.add_tags({'requestedCpu': details['requestedCpu'],
>                 'requestedMemoryInGb': details['requestedMemoryInGb']})
> ```
>
> ```azurecli-interactive
> az ml model profile -g <resource-group-name> -w <workspace-name> --i <model-id> --add-tag requestedCpu=1 --add-tag requestedMemoryInGb=0.5
> ```

## <a name="deploy-to-target"></a>Hedefe dağıt

Dağıtım, modelleri dağıtmak için çıkarım yapılandırma dağıtımı yapılandırmasını kullanır. Dağıtım işlemi, işlem hedefi ne olursa olsun benzerdir. Aks kümesine bir başvuru sağlamanız gerektiğinden AKS 'e dağıtım biraz farklıdır.

### <a name="choose-a-compute-target"></a>İşlem hedefi seçin

Web hizmeti dağıtımınızı barındırmak için aşağıdaki işlem hedeflerini veya işlem kaynaklarını kullanabilirsiniz:

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

### <a name="define-your-deployment-configuration"></a>Dağıtım yapılandırmanızı tanımlama

Modelinizi dağıtılmadan önce dağıtım yapılandırmasını tanımlamanız gerekir. *Dağıtım yapılandırması, Web hizmetini barındıracak işlem hedefine özgüdür.* Örneğin, bir modeli yerel olarak dağıttığınızda, hizmetin istekleri kabul ettiği bağlantı noktasını belirtmeniz gerekir. Dağıtım yapılandırması, giriş betiğinizin bir parçası değil. Model ve giriş betiğini barındıracak işlem hedefinin özelliklerini tanımlamak için kullanılır.

Ayrıca, çalışma alanınız ile ilişkili bir Azure Kubernetes Service (AKS) örneğiniz yoksa, işlem kaynağını oluşturmanız da gerekebilir.

Aşağıdaki tabloda her işlem hedefi için bir dağıtım yapılandırması oluşturma örneği verilmiştir:

| İşlem hedefi | Dağıtım yapılandırma örneği |
| ----- | ----- |
| Yerel | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Yerel, Azure Container Instances ve AKS Web Hizmetleri için sınıflar şu kaynaktan içeri aktarılabilir `azureml.core.webservice` :

```python
from azureml.core.webservice import AciWebservice, AksWebservice, LocalWebservice
```

### <a name="securing-deployments-with-tls"></a>TLS ile dağıtımları güvenli hale getirme

Bir Web hizmeti dağıtımının güvenliğini sağlama hakkında daha fazla bilgi için bkz. [TLS 'Yi etkinleştirme ve dağıtma](how-to-secure-web-service.md#enable).

### <a name="local-deployment"></a><a id="local"></a>Yerel dağıtım

Bir modeli yerel olarak dağıtmak için, yerel makinenizde Docker yüklü olmalıdır.

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Daha fazla bilgi için bkz. [Localwebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py), [model. deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)ve [WebService](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py)belgeleri.

#### <a name="using-the-cli"></a>CLı 'yi kullanma

CLı kullanarak bir modeli dağıtmak için aşağıdaki komutu kullanın. `mymodel:1`Kayıtlı modelin adı ve sürümüyle değiştirin:

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
```

[!INCLUDE [aml-local-deploy-config](../../includes/machine-learning-service-local-deploy-config.md)]

Daha fazla bilgi için bkz. [az ml model Deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) belgeleri.

### <a name="understanding-service-state"></a>Hizmet durumunu anlama

Model dağıtımı sırasında hizmet durumu değişikliğini tam olarak dağıttığında görebilirsiniz.

Aşağıdaki tabloda farklı hizmet durumları açıklanmaktadır:

| Web hizmeti durumu | Description | Son durum?
| ----- | ----- | ----- |
| Kta | Hizmet, dağıtım sürecinde. | No |
| Uygun Değil | Hizmet dağıtıldı, ancak şu anda ulaşılamaz durumda.  | No |
| Unschedulable | Kaynak eksikliği nedeniyle hizmet şu anda dağıtılamıyor. | No |
| Başarısız | Hizmet bir hata veya kilitlenme nedeniyle dağıtılamadı. | Yes |
| Sağlam | Hizmet sağlıklı ve uç nokta kullanılabilir. | Yes |

### <a name="compute-instance-web-service-devtest"></a><a id="notebookvm"></a>İşlem örneği Web hizmeti (geliştirme/test)

Bkz. [Azure Machine Learning işlem örneği için model dağıtma](how-to-deploy-local-container-notebook-vm.md).

### <a name="azure-container-instances-devtest"></a><a id="aci"></a>Azure Container Instances (geliştirme/test)

Bkz. [Azure Container Instances dağıtma](how-to-deploy-azure-container-instance.md).

### <a name="azure-kubernetes-service-devtest-and-production"></a><a id="aks"></a>Azure Kubernetes hizmeti (geliştirme/test ve üretim)

Bkz. [Azure Kubernetes hizmetine dağıtma](how-to-deploy-azure-kubernetes-service.md).

### <a name="ab-testing-controlled-rollout"></a>A/B testi (kontrollü dağıtım)
Daha fazla bilgi için bkz. [ml modellerinin kontrollü dağıtımı](how-to-deploy-azure-kubernetes-service.md#deploy-models-to-aks-using-controlled-rollout-preview) .

## <a name="consume-web-services"></a>Web hizmetlerini kullanma

Her dağıtılan Web hizmeti bir REST uç noktası sağlar, bu sayede herhangi bir programlama dilinde istemci uygulamaları oluşturabilirsiniz.
Hizmetiniz için anahtar tabanlı kimlik doğrulamasını etkinleştirdiyseniz, istek üst bilgisinde belirteç olarak bir hizmet anahtarı sağlamanız gerekir.
Hizmetiniz için belirteç tabanlı kimlik doğrulamasını etkinleştirdiyseniz, istek üst bilgisinde taşıyıcı belirteci olarak bir Azure Machine Learning JSON Web Token (JWT) sağlamanız gerekir. 

Birincil fark **anahtarlar statiktir ve el ile yeniden üretilebilir**ve **süre sonu sonrasında belirteçlerin yenilenmesi gerekir**. Anahtar tabanlı kimlik doğrulaması, Azure Container Instance ve Azure Kubernetes hizmeti tarafından dağıtılan Web Hizmetleri için desteklenir ve belirteç tabanlı kimlik doğrulaması **yalnızca** Azure Kubernetes hizmet dağıtımları için kullanılabilir. Daha fazla bilgi ve belirli kod örnekleri için bkz. [nasıl yapılır](how-to-setup-authentication.md#web-service-authentication) kimlik doğrulaması.

> [!TIP]
> Hizmeti dağıttıktan sonra şema JSON belgesini alabilirsiniz. Yerel Web hizmetinin Swagger dosyasına URI 'yi almak için dağıtılan Web hizmetindeki [swagger_uri özelliğini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri) kullanın (örneğin, `service.swagger_uri` ).

### <a name="request-response-consumption"></a>İstek-yanıt tüketimi

Aşağıda, Python 'da hizmetinizin nasıl çağrılcağına dair bir örnek verilmiştir:
```python
import requests
import json

headers = {'Content-Type': 'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]
elif service.token_auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_token()[0]

print(headers)

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

response = requests.post(
    service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Daha fazla bilgi için bkz. [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md).

### <a name="web-service-schema-openapi-specification"></a>Web hizmeti şeması (Openapı belirtimi)

Dağıtımınızla otomatik şema oluşturma kullandıysanız, [swagger_uri özelliğini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri)kullanarak hizmetin openapı belirtiminin adresini alabilirsiniz. (Örneğin, `print(service.swagger_uri)` .) Belirtimi almak için bir GET isteği kullanın veya bir tarayıcıda URI 'yi açın.

Aşağıdaki JSON belgesi, bir dağıtım için oluşturulan bir şemanın (Openapı belirtimi) bir örneğidir:

```json
{
    "swagger": "2.0",
    "info": {
        "title": "myservice",
        "description": "API specification for Azure Machine Learning myservice",
        "version": "1.0"
    },
    "schemes": [
        "https"
    ],
    "consumes": [
        "application/json"
    ],
    "produces": [
        "application/json"
    ],
    "securityDefinitions": {
        "Bearer": {
            "type": "apiKey",
            "name": "Authorization",
            "in": "header",
            "description": "For example: Bearer abc123"
        }
    },
    "paths": {
        "/": {
            "get": {
                "operationId": "ServiceHealthCheck",
                "description": "Simple health check endpoint to ensure the service is up at any given point.",
                "responses": {
                    "200": {
                        "description": "If service is up and running, this response will be returned with the content 'Healthy'",
                        "schema": {
                            "type": "string"
                        },
                        "examples": {
                            "application/json": "Healthy"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        },
        "/score": {
            "post": {
                "operationId": "RunMLService",
                "description": "Run web service's model and get the prediction output",
                "security": [
                    {
                        "Bearer": []
                    }
                ],
                "parameters": [
                    {
                        "name": "serviceInputPayload",
                        "in": "body",
                        "description": "The input payload for executing the real-time machine learning service.",
                        "schema": {
                            "$ref": "#/definitions/ServiceInput"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "The service processed the input correctly and provided a result prediction, if applicable.",
                        "schema": {
                            "$ref": "#/definitions/ServiceOutput"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        }
    },
    "definitions": {
        "ServiceInput": {
            "type": "object",
            "properties": {
                "data": {
                    "type": "array",
                    "items": {
                        "type": "array",
                        "items": {
                            "type": "integer",
                            "format": "int64"
                        }
                    }
                }
            },
            "example": {
                "data": [
                    [ 10, 9, 8, 7, 6, 5, 4, 3, 2, 1 ]
                ]
            }
        },
        "ServiceOutput": {
            "type": "array",
            "items": {
                "type": "number",
                "format": "double"
            },
            "example": [
                3726.995
            ]
        },
        "ErrorResponse": {
            "type": "object",
            "properties": {
                "status_code": {
                    "type": "integer",
                    "format": "int32"
                },
                "message": {
                    "type": "string"
                }
            }
        }
    }
}
```

Daha fazla bilgi için bkz. [Openapı belirtimi](https://swagger.io/specification/).

Belirtiden istemci kitaplıkları oluşturabileceğiniz bir yardımcı program için bkz. [Swagger-CodeGen](https://github.com/swagger-api/swagger-codegen).

### <a name="batch-inference"></a><a id="azuremlcompute"></a>Toplu çıkarım
Azure Machine Learning Işlem hedefleri Azure Machine Learning tarafından oluşturulur ve yönetilir. Bunlar, Azure Machine Learning işlem hatlarından toplu tahmin için kullanılabilirler.

Azure Machine Learning Işlem ile Batch çıkarımı hakkında yönergeler için bkz. [Batch öngörülerini çalıştırma](tutorial-pipeline-batch-scoring-classification.md).

### <a name="iot-edge-inference"></a><a id="iotedge"></a>IoT Edge çıkarımı
Kenara dağıtım desteği önizleme aşamasındadır. Daha fazla bilgi için bkz. [Azure Machine Learning IoT Edge modül olarak dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).


## <a name="update-web-services"></a><a id="update"></a>Web hizmetlerini güncelleştirme

[!INCLUDE [aml-update-web-service](../../includes/machine-learning-update-web-service.md)]

## <a name="continuously-deploy-models"></a>Modelleri sürekli dağıt

[Azure DevOps](https://azure.microsoft.com/services/devops/)için Machine Learning uzantısını kullanarak, modelleri sürekli olarak dağıtabilirsiniz. Yeni bir makine öğrenimi modeli bir Azure Machine Learning çalışma alanına kaydedildiğinde bir dağıtım işlem hattı tetiklemek için Azure DevOps Machine Learning uzantısını kullanabilirsiniz.

1. [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops)için kaydolun, bu, uygulamanızı sürekli tümleştirme ve mümkün olan herhangi bir platforma veya buluta teslim eden bir uygulamadır. (Azure Pipelines [Machine Learning işlem hatları](concept-ml-pipelines.md#compare)ile aynı olmadığını unutmayın.)

1. [Azure DevOps projesi oluşturun.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. [Azure Pipelines için Machine Learning uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList)yükler.

1. Yapılarınıza erişebilmek için Azure Machine Learning çalışma alanınıza bir hizmet sorumlusu bağlantısı kurmak üzere hizmet bağlantıları 'nı kullanın. Proje Ayarları ' na gidin, **hizmet bağlantıları**' nı seçin ve ardından **Azure Resource Manager**' yi seçin:

    [![Azure Resource Manager seçin](media/how-to-deploy-and-where/view-service-connection.png)](media/how-to-deploy-and-where/view-service-connection-expanded.png)

1. **Kapsam düzeyi** listesinde **AzureMLWorkspace**' ı seçin ve ardından kalan değerleri girin:

    ![AzureMLWorkspace seçin](./media/how-to-deploy-and-where/resource-manager-connection.png)

1. Makine öğrenimi modelinizi Azure Pipelines kullanarak sürekli olarak dağıtmak için, işlem hatları altında **yayın**' ı seçin. Yeni bir yapıt ekleyin ve ardından daha önce oluşturduğunuz **AzureML model** yapıtını ve hizmet bağlantısını seçin. Bir dağıtımı tetiklemek için modeli ve sürümü seçin:

    [![AzureML modelini seçin](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)](media/how-to-deploy-and-where/enable-modeltrigger-artifact-expanded.png)

1. Model yapıtında model tetikleyiciyi etkinleştirin. Tetikleyiciyi açtığınızda, söz konusu modelin belirtilen sürümü (yani, en yeni sürümü) çalışma alanınıza kaydedildiğinde, bir Azure DevOps sürümü işlem hattı tetiklenir.

    [![Model tetikleyiciyi etkinleştir](media/how-to-deploy-and-where/set-modeltrigger.png)](media/how-to-deploy-and-where/set-modeltrigger-expanded.png)

Daha fazla örnek proje ve örnek için GitHub 'daki Şu örnek depolara bakın:

* [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
* [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)

## <a name="download-a-model"></a>Model indirme
Modelinizi kendi yürütme ortamınızda kullanmak üzere indirmek isterseniz, bunu aşağıdaki SDK/CLı komutlarıyla yapabilirsiniz:

'SıNıN
```python
model_path = Model(ws,'mymodel').download()
```

CLı
```azurecli-interactive
az ml model download --model-id mymodel:1 --target-dir model_folder
```

## <a name="preview-no-code-model-deployment"></a>Önizle Kod olmayan model dağıtımı

Şu anda önizleme aşamasında olan kod modeli dağıtımı yok ve şu makine öğrenimi çerçevelerini destekliyor:

### <a name="tensorflow-savedmodel-format"></a>TensorFlow SavedModel biçimi
TensorFlow modellerinin, kod olmayan model dağıtımıyla çalışmak için **savedmodel biçiminde** kaydedilmesi gerekir.

SavedModel oluşturma hakkında bilgi için lütfen [Bu bağlantıya](https://www.tensorflow.org/guide/saved_model) bakın.

```python
from azureml.core import Model

model = Model.register(workspace=ws,
                       model_name='flowers',                        # Name of the registered model in your workspace.
                       model_path='./flowers_model',                # Local Tensorflow SavedModel folder to upload and register as a model.
                       model_framework=Model.Framework.TENSORFLOW,  # Framework used to create the model.
                       model_framework_version='1.14.0',            # Version of Tensorflow used to create the model.
                       description='Flowers model')

service_name = 'tensorflow-flower-service'
service = Model.deploy(ws, service_name, [model])
```

### <a name="onnx-models"></a>ONNX modelleri

ONNX model kaydı ve dağıtımı herhangi bir ONNX çıkarım grafiği için desteklenir. Ön işlem ve postprocess adımları şu anda desteklenmiyor.

Bir MNIST ONNX modelinin nasıl kaydedileceği ve dağıtılacağı hakkında bir örnek aşağıda verilmiştir:

```python
from azureml.core import Model

model = Model.register(workspace=ws,
                       model_name='mnist-sample',                  # Name of the registered model in your workspace.
                       model_path='mnist-model.onnx',              # Local ONNX model to upload and register as a model.
                       model_framework=Model.Framework.ONNX ,      # Framework used to create the model.
                       model_framework_version='1.3',              # Version of ONNX used to create the model.
                       description='Onnx MNIST model')

service_name = 'onnx-mnist-service'
service = Model.deploy(ws, service_name, [model])
```

Pytorch kullanıyorsanız, bu [modelleri pytorch 'den ONNX 'e aktarmak](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) , dönüştürme ve sınırlamalara ilişkin ayrıntıları içerir. 

### <a name="scikit-learn-models"></a>Scikit-modelleri öğrenme

Tüm yerleşik scikit-öğrenme model türleri için hiçbir kod modeli dağıtımı desteklenmez.

Ek kod olmadan bir sköğren modelinin nasıl kaydedileceği ve dağıtıldığı hakkında bir örnek aşağıda verilmiştir:

```python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = Model.register(workspace=ws,
                       model_name='my-sklearn-model',                # Name of the registered model in your workspace.
                       model_path='./sklearn_regression_model.pkl',  # Local file to upload and register as a model.
                       model_framework=Model.Framework.SCIKITLEARN,  # Framework used to create the model.
                       model_framework_version='0.19.1',             # Version of scikit-learn used to create the model.
                       resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5),
                       description='Ridge regression model to predict diabetes progression.',
                       tags={'area': 'diabetes', 'type': 'regression'})
                       
service_name = 'my-sklearn-service'
service = Model.deploy(ws, service_name, [model])
```

NOTE: predict_proba destekleyen modeller, varsayılan olarak bu yöntemi kullanacaktır. Bunu tahmin etmek için bunu geçersiz kılmak üzere, posta gövdesini aşağıda gösterildiği gibi değiştirebilirsiniz:
```python
import json


input_payload = json.dumps({
    'data': [
        [ 0.03807591,  0.05068012,  0.06169621, 0.02187235, -0.0442235,
         -0.03482076, -0.04340085, -0.00259226, 0.01990842, -0.01764613]
    ],
    'method': 'predict'  # If you have a classification model, the default behavior is to run 'predict_proba'.
})

output = service.run(input_payload)

print(output)
```

NOTE: Bu bağımlılıklar önceden oluşturulmuş scikit-öğrenme çıkarımı kapsayıcısına dahil edilmiştir:

```yaml
    - dill
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
    - joblib
    - pandas
    - scipy
    - sklearn_pandas
```

## <a name="package-models"></a>Paket modelleri

Bazı durumlarda, modeli dağıtmadan bir Docker görüntüsü oluşturmak isteyebilirsiniz (örneğin, [Azure App Service dağıtmayı](how-to-deploy-app-service.md)planlıyorsanız). Ya da görüntüyü indirmek ve yerel bir Docker yüklemesinde çalıştırmak isteyebilirsiniz. Görüntüyü oluşturmak, incelemek, değiştirmek ve görüntüyü el ile oluşturmak için kullanılan dosyaları da indirmek isteyebilirsiniz.

Model paketleme, bu işlemleri yapmanızı sağlar. Bir modeli Web hizmeti olarak barındırmak için gereken tüm varlıkları paketler ve tamamen oluşturulmuş bir Docker görüntüsünü ya da bir tane oluşturmak için gereken dosyaları indirmelerini sağlar. Model paketlemeyi kullanmanın iki yolu vardır:

**Paketlenmiş bir model indirin:** Modeli ve bir Web hizmeti olarak barındırmak için gereken diğer dosyaları içeren bir Docker görüntüsü indirin.

**Dockerfile oluştur:** Docker görüntüsü oluşturmak için gereken Dockerfile, model, giriş betiği ve diğer varlıkları indirin. Görüntüyü yerel olarak oluşturmadan önce dosyaları inceleyebilir veya değişiklik yapabilirsiniz.

Her iki paket de yerel bir Docker görüntüsü almak için kullanılabilir.

> [!TIP]
> Bir paket oluşturmak, model dağıtmaya benzer. Kayıtlı bir model ve çıkarım yapılandırmasını kullanırsınız.

> [!IMPORTANT]
> Tamamen oluşturulmuş bir görüntüyü indirmek veya yerel olarak bir görüntü oluşturmak için, geliştirme ortamınızda [Docker](https://www.docker.com) 'ın yüklü olması gerekir.

### <a name="download-a-packaged-model"></a>Paketlenmiş model indirme

Aşağıdaki örnek, çalışma alanınız için Azure Container Registry 'de kayıtlı olan bir görüntü oluşturur:

```python
package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
```

Bir paket oluşturduktan sonra, `package.pull()` görüntüyü yerel Docker ortamınıza çekmek için ' i kullanabilirsiniz. Bu komutun çıktısı görüntünün adını görüntüler. Örneğin: 

`Status: Downloaded newer image for myworkspacef78fd10.azurecr.io/package:20190822181338`. 

Modeli indirdikten sonra, `docker images` yerel görüntüleri listelemek için komutunu kullanın:

```text
REPOSITORY                               TAG                 IMAGE ID            CREATED             SIZE
myworkspacef78fd10.azurecr.io/package    20190822181338      7ff48015d5bd        4 minutes ago       1.43 GB
```

Bu görüntüye göre yerel bir kapsayıcı başlatmak için, Shell veya komut satırından adlandırılmış bir kapsayıcı başlatmak üzere aşağıdaki komutu kullanın. Değeri, `<imageid>` komutu tarafından döndürülen görüntü kimliğiyle değiştirin `docker images` .

```bash
docker run -p 6789:5001 --name mycontainer <imageid>
```

Bu komut, adlı görüntünün en son sürümünü başlatır `myimage` . Yerel bağlantı noktası 6789 ' i Web hizmetinin dinlediği kapsayıcıda bağlantı noktasına eşler (5001). Ayrıca `mycontainer` , kapsayıcıyı daha kolay durdurmayı sağlayan kapsayıcıya de atar. Kapsayıcı başlatıldıktan sonra, istek gönderebilirsiniz `http://localhost:6789/score` .

### <a name="generate-a-dockerfile-and-dependencies"></a>Dockerfile ve bağımlılıklar oluşturma

Aşağıdaki örnek, bir görüntüyü yerel olarak oluşturmak için gereken Dockerfile, model ve diğer varlıkların nasıl indirileceği gösterilmektedir. `generate_dockerfile=True`Parametresi, tam olarak oluşturulmuş bir görüntü değil, dosyaları istediğinizi belirtir.

```python
package = Model.package(ws, [model], inference_config, generate_dockerfile=True)
package.wait_for_creation(show_output=True)
# Download the package.
package.save("./imagefiles")
# Get the Azure container registry that the model/Dockerfile uses.
acr=package.get_container_registry()
print("Address:", acr.address)
print("Username:", acr.username)
print("Password:", acr.password)
```

Bu kod, görüntüyü dizine oluşturmak için gereken dosyaları indirir `imagefiles` . Kayıtlı dosyalara eklenen Dockerfile, bir Azure Container Registry 'de depolanan bir temel görüntüye başvurur. Yerel Docker yüklemenizde görüntüyü oluştururken, kayıt defterinde kimlik doğrulamak için adresi, Kullanıcı adını ve parolayı kullanmanız gerekir. Yerel bir Docker yüklemesi kullanarak görüntüyü derlemek için aşağıdaki adımları kullanın:

1. Bir kabuk veya komut satırı oturumundan, Azure Container Registry ile Docker kimlik doğrulaması yapmak için aşağıdaki komutu kullanın. `<address>`, `<username>` Ve `<password>` değerlerini tarafından alınan değerlerle değiştirin `package.get_container_registry()` .

    ```bash
    docker login <address> -u <username> -p <password>
    ```

2. Görüntüyü oluşturmak için aşağıdaki komutu kullanın. `<imagefiles>`Dosyalarını kaydettiğiniz dizinin yoluyla değiştirin `package.save()` .

    ```bash
    docker build --tag myimage <imagefiles>
    ```

    Bu komut, görüntü adını olarak ayarlar `myimage` .

Görüntünün derlendiğini doğrulamak için `docker images` komutunu kullanın. Listede görüntü görmeniz gerekir `myimage` :

```text
REPOSITORY      TAG                 IMAGE ID            CREATED             SIZE
<none>          <none>              2d5ee0bf3b3b        49 seconds ago      1.43 GB
myimage         latest              739f22498d64        3 minutes ago       1.43 GB
```

Bu görüntüye göre yeni bir kapsayıcı başlatmak için aşağıdaki komutu kullanın:

```bash
docker run -p 6789:5001 --name mycontainer myimage:latest
```

Bu komut, adlı görüntünün en son sürümünü başlatır `myimage` . Yerel bağlantı noktası 6789 ' i Web hizmetinin dinlediği kapsayıcıda bağlantı noktasına eşler (5001). Ayrıca `mycontainer` , kapsayıcıyı daha kolay durdurmayı sağlayan kapsayıcıya de atar. Kapsayıcı başlatıldıktan sonra, istek gönderebilirsiniz `http://localhost:6789/score` .

### <a name="example-client-to-test-the-local-container"></a>Yerel kapsayıcıyı test eden örnek istemci

Aşağıdaki kod, kapsayıcında kullanılabilecek bir Python istemcisi örneğidir:

```python
import requests
import json

# URL for the web service.
scoring_uri = 'http://localhost:6789/score'

# Two sets of data to score, so we get two results back.
data = {"data":
        [
            [ 1,2,3,4,5,6,7,8,9,10 ],
            [ 10,9,8,7,6,5,4,3,2,1 ]
        ]
        }
# Convert to JSON string.
input_data = json.dumps(data)

# Set the content type.
headers = {'Content-Type': 'application/json'}

# Make the request and display the response.
resp = requests.post(scoring_uri, input_data, headers=headers)
print(resp.text)
```

Örneğin, diğer programlama dillerinde istemciler için bkz. [Web Hizmetleri olarak dağıtılan modelleri](how-to-consume-web-service.md)kullanma.

### <a name="stop-the-docker-container"></a>Docker kapsayıcısını durdur

Kapsayıcıyı durdurmak için, farklı bir kabuktan veya komut satırından aşağıdaki komutu kullanın:

```bash
docker kill mycontainer
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtılmış bir Web hizmetini silmek için kullanın `service.delete()` .
Kayıtlı bir modeli silmek için kullanın `model.delete()` .

Daha fazla bilgi için bkz. [WebService. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--) ve [model. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--)belgeleri.

<a id="advanced-entry-script"></a>
## <a name="advanced-entry-script-authoring"></a>Gelişmiş giriş betiği yazma

<a id="binary"></a>

### <a name="binary-data"></a>İkili veriler

Modeliniz bir görüntü gibi ikili verileri kabul ediyorsa, `score.py` dağıtımınız için kullanılan dosyayı ham http isteklerini kabul edecek şekilde değiştirmeniz gerekir. Ham verileri kabul etmek için, `AMLRequest` giriş betiğinizdeki sınıfını kullanın ve `@rawhttp` işleve dekoratör ekleyin `run()` .

`score.py`İkili verileri kabul eden bir örneği aşağıda verilmiştir:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse


def init():
    print("This is init()")


@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs.
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real-world solution, you would load the data from reqBody
        # and send it to the model. Then return the response.

        # For demonstration purposes, this example just returns the posted data as the response.
        return AMLResponse(reqBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> `AMLRequest`Sınıfı `azureml.contrib` ad alanıdır. Bu ad alanındaki varlıklar, hizmeti geliştirmek için çalışmamız sırasında sıklıkla değişir. Bu ad alanındaki her türlü şey, Microsoft tarafından tam olarak desteklenmeyen bir önizleme olarak düşünülmelidir.
>
> Bunu yerel geliştirme ortamınızda test etmeniz gerekirse, aşağıdaki komutu kullanarak bileşenleri yükleyebilirsiniz:
>
> ```shell
> pip install azureml-contrib-services
> ```

`AMLRequest`Sınıfı yalnızca Score.py içindeki ham gönderilen verilere erişmenize izin verir, hiçbir istemci tarafı bileşeni yoktur. Bir istemciden verileri normal olarak nakledersiniz. Örneğin, aşağıdaki Python kodu bir resim dosyasını okur ve verileri gönderir:

```python
import requests
# Load image data
data = open('example.jpg', 'rb').read()
# Post raw data to scoring URI
res = requests.post(url='<scoring-uri>', data=data, headers={'Content-Type': 'application/octet-stream'})
```

<a id="cors"></a>

### <a name="cross-origin-resource-sharing-cors"></a>Çıkış noktaları arası kaynak paylaşımı (CORS)

Çıkış noktaları arası kaynak paylaşımı, bir Web sayfasındaki kaynakların başka bir etki alanından istenmesinin izin vermesinin bir yoludur. CORS, istemci isteğiyle gönderilen ve hizmet yanıtıyla döndürülen HTTP üstbilgileri aracılığıyla çalışmaktadır. CORS ve geçerli üstbilgiler hakkında daha fazla bilgi için bkz. Vikipde [çıkış noktaları arası kaynak paylaşma](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) .

Model dağıtımınızı CORS 'yi destekleyecek şekilde yapılandırmak için, `AMLResponse` giriş betiğinizdeki sınıfını kullanın. Bu sınıf, yanıt nesnesindeki üst bilgileri ayarlamanıza olanak sağlar.

Aşağıdaki örnek, `Access-Control-Allow-Origin` giriş betiğindeki yanıtın üst bilgisini ayarlar:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs.
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real-world solution, you would load the data from reqBody
        # and send it to the model. Then return the response.

        # For demonstration purposes, this example
        # adds a header and returns the request body.
        resp = AMLResponse(reqBody, 200)
        resp.headers['Access-Control-Allow-Origin'] = "http://www.example.com"
        return resp
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> `AMLResponse`Sınıfı `azureml.contrib` ad alanıdır. Bu ad alanındaki varlıklar, hizmeti geliştirmek için çalışmamız sırasında sıklıkla değişir. Bu ad alanındaki her türlü şey, Microsoft tarafından tam olarak desteklenmeyen bir önizleme olarak düşünülmelidir.
>
> Bunu yerel geliştirme ortamınızda test etmeniz gerekirse, aşağıdaki komutu kullanarak bileşenleri yükleyebilirsiniz:
>
> ```shell
> pip install azureml-contrib-services
> ```


> [!WARNING]
> Azure Machine Learning, Puanlama hizmetini çalıştıran kapsayıcılara yalnızca POST ve GET isteklerini yönlendirir. Bu, seçenek istekleri ön uçuş CORS isteklerini kullanan tarayıcılar nedeniyle hatalara neden olabilir.
> 

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir Azure Machine Learning modeli kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)

