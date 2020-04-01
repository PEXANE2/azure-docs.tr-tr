---
title: Modelleri nasıl ve nerede dağıtılır
titleSuffix: Azure Machine Learning
description: Azure Kapsayıcı Örnekleri, Azure Kubernetes Hizmeti, Azure IoT Edge ve alan programlanabilir kapı dizileri dahil olmak üzere Azure Machine Learning modellerinizi nasıl ve nerede dağıtacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 02/27/2020
ms.custom: seoapril2019
ms.openlocfilehash: 0deace98c5be0b2ce2f29abce4c8a804145afdb1
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475614"
---
# <a name="deploy-models-with-azure-machine-learning"></a>Azure Machine Learning ile modelleri dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Makine öğrenimi modelinizi Azure bulutunda veya Azure IoT Edge cihazlarında bir web hizmeti olarak nasıl dağıtılayabilirsiniz öğrenin.

Modelinizi [nereye dağıtsanız dağıtın,](#target) iş akışı benzerdir:

1. Modeli kaydedin.
1. Dağıtım için hazırlayın. (Varlıkları, kullanımı, işlem hedefini belirtin.)
1. Modeli işlem hedefine dağıtın.
1. Web hizmeti olarak da adlandırılan dağıtılan modeli test edin.

Dağıtım iş akışında yer alan kavramlar hakkında daha fazla bilgi için Azure [Machine Learning ile modelleri Yönet, dağıt ve izleyin.](concept-model-management-and-deployment.md)

## <a name="prerequisites"></a>Ön koşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için [bkz.](how-to-manage-workspace.md)

- Bir model. Eğitimli bir modeliniz yoksa, [bu öğreticide](https://aka.ms/azml-deploy-cloud)sağlanan model ve bağımlılık dosyalarını kullanabilirsiniz.

- [Machine Learning hizmeti için Azure CLI uzantısı,](reference-azure-machine-learning-cli.md)Python için Azure Machine Learning [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Visual Studio Code uzantısı.](tutorial-setup-vscode-extension.md)

## <a name="connect-to-your-workspace"></a>Çalışma alanınıza bağlanma

Aşağıdaki kod, yerel geliştirme ortamına önbelleğe alınan bilgileri kullanarak Azure Machine Learning çalışma alanına nasıl bağlanışyapılacağını gösterir:

+ **SDK’yı kullanarak**

   ```python
   from azureml.core import Workspace
   ws = Workspace.from_config(path=".file-path/ws_config.json")
   ```

  Bir çalışma alanına bağlanmak için SDK'yı kullanma hakkında daha fazla bilgi için Python belgeleri [için Azure Machine Learning SDK'ya](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace) bakın.

+ **CLI'yi kullanma**

   CLI kullanırken, komutiçin çalışma alanını belirtmek için `-w` veya `--workspace-name` parametreyi kullanın.

+ **Visual Studio Code’u kullanma**

   Visual Studio Code'u kullandığınızda, grafik arabirimi kullanarak çalışma alanını seçersiniz. Daha fazla bilgi için Visual Studio Code uzantılı belgelerdeki [modelleri dağıt ve yönetin.](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model)

## <a name="register-your-model"></a><a id="registermodel"></a>Modelinizi kaydedin

Kayıtlı model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları çalışma alanında tek bir model olarak kaydedebilirsiniz. Dosyaları kaydettikten sonra, kayıtlı modeli karşıdan yükleyebilir veya dağıtabilir ve kaydettiğiniz tüm dosyaları alabilirsiniz.

> [!TIP]
> Bir modeli kaydettirdiğinizde, bulut konumunun (eğitim çalışmasından) veya yerel bir dizinin yolunu sağlarsınız. Bu yol sadece kayıt sürecinin bir parçası olarak yüklemek için dosyaları bulmak tır. Giriş komut dosyasında kullanılan yol eşleşmesi gerekmez. Daha fazla bilgi için [giriş komut dosyanızda model dosyalarını bul'a](#load-model-files-in-your-entry-script)bakın.

Makine öğrenimi modelleri Azure Machine Learning çalışma alanınızda kayıtlıdır. Model Azure Machine Learning'den veya başka bir yerden gelebilir. Bir modeli kaydederken, isteğe bağlı olarak model hakkında meta veriler sağlayabilirsiniz. Model `tags` `properties` kaydına uyguladığınız sözlükler ve sözlükler modelleri filtrelemek için kullanılabilir.

Aşağıdaki örnekler, bir modelin nasıl kaydedilenini gösterir.

### <a name="register-a-model-from-an-experiment-run"></a>Deneme çalışmasından bir model kaydetme

Bu bölümdeki kod parçacıkları, bir modelin bir eğitim çalıştırAn modelin nasıl kaydedilebildiğini gösterir:

> [!IMPORTANT]
> Bu parçacıkları kullanmak için daha önce bir eğitim çalışması gerçekleştirmiş olmanız ve `Run` nesneye (SDK örneği) veya çalışan kimlik değerine (CLI örneği) erişmeniz gerekir. Eğitim modelleri hakkında daha fazla bilgi için [bkz.](how-to-set-up-training-targets.md)

+ **SDK’yı kullanarak**

  Bir modeli eğitmek için SDK'yı kullandığınızda, modeli nasıl eğitdiğinize bağlı olarak bir [Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py) nesnesi veya [AutoMLRun](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun) nesnesi alabilirsiniz. Her nesne, deneme çalışması tarafından oluşturulan bir modeli kaydetmek için kullanılabilir.

  + Bir `azureml.core.Run` nesneden bir model kaydedin:
 
    ```python
    model = run.register_model(model_name='sklearn_mnist',
                               tags={'area': 'mnist'},
                               model_path='outputs/sklearn_mnist_model.pkl')
    print(model.name, model.id, model.version, sep='\t')
    ```

    Parametre, `model_path` modelin bulut konumunu ifade eder. Bu örnekte, tek bir dosyanın yolu kullanılır. Model kaydına birden çok dosya `model_path` eklemek için, dosyaları içeren bir klasörün yoluna ayarlayın. Daha fazla bilgi için [Run.register_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-) belgelerine bakın.

  + Bir `azureml.train.automl.run.AutoMLRun` nesneden bir model kaydedin:

    ```python
        description = 'My AutoML Model'
        model = run.register_model(description = description,
                                   tags={'area': 'mnist'})

        print(run.model_id)
    ```

    Bu örnekte, `metric` `iteration` parametreler belirtilmesin, bu nedenle en iyi birincil ölçütlü yineleme kaydedilir. Çalışmadan döndürülen `model_id` değer, model adı yerine kullanılır.

    Daha fazla bilgi için [AutoMLRun.register_model](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun#register-model-model-name-none--description-none--tags-none--iteration-none--metric-none-) belgelerine bakın.

+ **CLI'yi kullanma**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid --tag area=mnist
  ```

  [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

  Parametre, `--asset-path` modelin bulut konumunu ifade eder. Bu örnekte, tek bir dosyanın yolu kullanılır. Model kaydına birden çok dosya `--asset-path` eklemek için, dosyaları içeren bir klasörün yoluna ayarlayın.

+ **Visual Studio Code’u kullanma**

  [Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model) uzantısını kullanarak modelleri herhangi bir model dosya sını veya klasörünü kullanarak kaydedin.

### <a name="register-a-model-from-a-local-file"></a>Yerel bir dosyadan bir model kaydetme

Modelin yerel yolunu sağlayarak bir model kaydedebilirsiniz. Bir klasörün veya tek bir dosyanın yolunu sağlayabilirsiniz. Azure Machine Learning ile eğitilmiş ve daha sonra indirilen modelleri kaydetmek için bu yöntemi kullanabilirsiniz. Bu yöntemi, Azure Machine Learning dışında eğitilmiş modelleri kaydetmek için de kullanabilirsiniz.

[!INCLUDE [trusted models](../../includes/machine-learning-service-trusted-model.md)]

+ **SDK ve ONNX'i kullanma**

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

  Model kaydına birden çok dosya `model_path` eklemek için, dosyaları içeren bir klasörün yoluna ayarlayın.

+ **CLI'yi kullanma**

  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  Model kaydına birden çok dosya `-p` eklemek için, dosyaları içeren bir klasörün yoluna ayarlayın.

**Zaman tahmini**: Yaklaşık 10 saniye.

Daha fazla bilgi için [Model sınıfının](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py)belgelerine bakın.

Azure Machine Learning dışında eğitilmiş modellerle çalışma hakkında daha fazla bilgi için [varolan bir modeli nasıl dağıtılayabilirsiniz.](how-to-deploy-existing-model.md)

<a name="target"></a>

## <a name="single-versus-multi-model-endpoints"></a>Tek ve çok modelli uç noktaları
Azure ML, tek bir bitiş noktasının arkasına tek veya birden çok model dağıtmayı destekler.

Çok modelli uç noktalar, birden çok modeli barındırmak için paylaşılan bir kapsayıcı kullanır. Bu, genel gider maliyetlerini azaltmaya yardımcı olur, kullanımı artırır ve modülleri topluluklar halinde birbirine zincirlemenize olanak tanır. Dağıtım komut dosyanızda belirttiğiniz modeller monte edilir ve servis kapsayıcısının diskinde kullanılabilir hale getirilir - bunları, puanlama sırasında istenen belirli modele bağlı olarak isteğe bağlı olarak belleğe yükleyebilirsiniz.

Tek bir kapsayıcı uç noktasının arkasında birden çok modelin nasıl kullanılacağını gösteren bir E2E örneği için [aşağıdaki örneğe](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/deploy-multi-model) bakın

## <a name="prepare-to-deploy"></a>Dağıtmaya hazırlanma

Modeli hizmet olarak dağıtmak için aşağıdaki bileşenlere ihtiyacınız vardır:

* **Çıkarım ortamını tanımlayın.** Bu ortam, modelinizi çıkarım için çalıştırmak için gereken bağımlılıkları kapsüller.
* **Puanlama kodunu tanımlayın.** Bu komut dosyası istekleri kabul eder, modeli kullanarak istekleri puanlar ve sonuçları döndürür.
* **Çıkarım yapılandırması tanımlayın.** Çıkarım yapılandırması, modeli hizmet olarak çalıştırmak için gereken ortam yapılandırmasını, giriş komut dosyasını ve diğer bileşenleri belirtir.

Gerekli bileşenlere sahip olduktan sonra, modelinizin CPU ve bellek gereksinimlerini anlamak için modelinizi dağıtması sonucunda oluşturulacak hizmetin profilini oluşturabilirsiniz.

### <a name="1-define-inference-environment"></a>1. Çıkarım ortamını tanımlayın

Çıkarım yapılandırması, modelinizi içeren web hizmetinin nasıl ayarlanır olduğunu açıklar. Modeli dağıttığınızda daha sonra kullanılır.

Çıkarım yapılandırması, dağıtımınız için gereken yazılım bağımlılıklarını tanımlamak için Azure Machine Learning ortamlarını kullanır. Ortamlar, eğitim ve dağıtım için gereken yazılım bağımlılıklarını oluşturmanıza, yönetmenize ve yeniden kullanmanıza olanak tanır. Özel bağımlılık dosyalarından bir ortam oluşturabilir veya küratörlüğünü yaptığı Azure Machine Learning ortamlarından birini kullanabilirsiniz. Aşağıdaki YAML çıkarım için bir Conda bağımlılıkları dosyasının bir örneğidir. Modeli web hizmeti olarak barındırmak için gereken işlevselliği içerdiğinden, verion >= 1,0,45 pip bağımlılığı olarak azureml varsayılanlarını belirtmeniz gerektiğini unutmayın. Otomatik şema oluşturma kullanmak istiyorsanız, giriş komut dosyanızın `inference-schema` da paketleri içe aktarması gerekir.

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
> Bağımlılığınız hem Conda hem de pip (PyPi'den) aracılığıyla kullanılabilen Microsoft, Conda paketleri genellikle yüklemeyi daha güvenilir hale getiren önceden oluşturulmuş ikili lerle birlikte geldiğinden, Conda sürümünü kullanmanızı önerir.
>
> Daha fazla bilgi için Bkz. [Conda ve Pip'i Anlamak](https://www.anaconda.com/understanding-conda-and-pip/).
>
> Bağımlılığınızın Conda üzerinden kullanılabilir olup olmadığını `conda search <package-name>` kontrol etmek için komutu [https://anaconda.org/anaconda/repo](https://anaconda.org/anaconda/repo) kullanın [https://anaconda.org/conda-forge/repo](https://anaconda.org/conda-forge/repo)veya paket dizinlerini kullanın.

Bağımlılıklar dosyasını bir ortam nesnesi oluşturmak ve ileride kullanmak üzere çalışma alanınıza kaydetmek için kullanabilirsiniz:

```python
from azureml.core.environment import Environment
myenv = Environment.from_conda_specification(name = 'myenv',
                                             file_path = 'path-to-conda-specification-file'
myenv.register(workspace=ws)
```

### <a name="2-define-scoring-code"></a><a id="script"></a>2. Puanlama kodunu tanımlama

Giriş betiği, dağıtılan bir web hizmetine gönderilen verileri alır ve modele geçirir. Ardından model tarafından döndürülen yanıtı alır ve bunu istemciye döndürür. *Komut dosyası modelinize özgüdür.* Modelin beklediği ve döndürdettiği verileri anlamalıdır.

Betik, modeli yükleyen ve çalıştıran iki işlev içerir:

* `init()`: Tipik olarak, bu işlev modeli genel bir nesneye yükler. Bu işlev, web hizmetinizin Docker kapsayıcısı başlatıldığında yalnızca bir kez çalıştırılır.

* `run(input_data)`: Bu işlev, giriş verilerine dayalı bir değeri tahmin etmek için modeli kullanır. Çalıştırmanın girişleri ve çıkışlarında serileştirme ve seriden çıkarma için normal olarak JSON kullanılır. Ham ikili verilerle de çalışabilirsiniz. Verileri modele göndermeden veya istemciye döndürmeden önce dönüştürebilirsiniz.

#### <a name="load-model-files-in-your-entry-script"></a>Giriş komut dosyanızda model dosyalarını yükleme

Giriş komut dosyanızda modelleri bulmanın iki yolu vardır:
* `AZUREML_MODEL_DIR`: Model konumuna giden yolu içeren bir ortam değişkeni.
* `Model.get_model_path`: Kayıtlı model adını kullanarak model dosyasına giden yolu döndüren api.

##### <a name="azureml_model_dir"></a>AZUREML_MODEL_DIR

AZUREML_MODEL_DIR, hizmet dağıtımı sırasında oluşturulan bir ortam değişkenidir. Dağıtılan modelin(ler) konumunu bulmak için bu ortam değişkenini kullanabilirsiniz.

Aşağıdaki tabloda dağıtılan model sayısına bağlı olarak AZUREML_MODEL_DIR değeri açıklanmaktadır:

| Dağıtım | Ortam değişken değeri |
| ----- | ----- |
| Tek model | Modeli içeren klasöre giden yol. |
| Birden fazla model | Tüm modelleri içeren klasöre giden yol. Modeller bu klasörde ada ve`$MODEL_NAME/$VERSION`sürüme göre bulunur ( ) |

Model kaydı ve dağıtımı sırasında Modeller AZUREML_MODEL_DIR yoluna yerleştirilir ve özgün dosya adları korunur.

Giriş komut dosyanızda bir model dosyasına giden yolu almak için, ortam değişkenini aradığınız dosya yolu ile birleştirin.

**Tek model örneği**
```python
# Example when the model is a file
model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_regression_model.pkl')

# Example when the model is a folder containing a file
file_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'my_model_folder', 'sklearn_regression_model.pkl')
```

**Birden çok model örneği**
```python
# Example when the model is a file, and the deployment contains multiple models
model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_model', '1', 'sklearn_regression_model.pkl')
```

##### <a name="get_model_path"></a>get_model_path

Bir modeli kaydettirdiğinizde, kayıt defterinde modeli yönetmek için kullanılan bir model adı sağlarsınız. Model dosyasının veya yerel dosya sistemindeki dosyaların yolunu almak için [model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) yöntemiyle bu adı kullanırsınız. Bir klasör veya dosya koleksiyonu kaydederseniz, bu API bu dosyaları içeren dizinin yolunu döndürür.

Bir modeli kaydettirdiğinizde, ona bir ad verirsiniz. Ad, modelin yerel olarak veya hizmet dağıtımı sırasında yerleştirildiği yere karşılık gelir.

#### <a name="optional-define-model-web-service-schema"></a>(İsteğe bağlı) Model web hizmeti şema tanımlayın

Web hizmetiniz için otomatik olarak bir şema oluşturmak için, tanımlanan tür nesnelerden biri için oluşturucudaki giriş ve/veya çıktının bir örneğini sağlayın. Tür ve örnek otomatik olarak şema oluşturmak için kullanılır. Azure Machine Learning daha sonra dağıtım sırasında web hizmeti için bir [OpenAPI](https://swagger.io/docs/specification/about/) (Swagger) belirtimi oluşturur.

Bu türler şu anda desteklenir:

* `pandas`
* `numpy`
* `pyspark`
* Standart Python nesnesi

Şema oluşturma yı kullanmak için, `inference-schema` bağımlılıklar dosyanıza açık kaynak paketini ekleyin. Bu paket hakkında daha [https://github.com/Azure/InferenceSchema](https://github.com/Azure/InferenceSchema)fazla bilgi için bkz. Web hizmeti için istek ve yanıt `input_sample` `output_sample` biçimlerini temsil eden giriş ve çıktı örnek biçimlerini ve değişkenleri tanımlayın. Bu `run()` örnekleri, işlev üzerindeki giriş ve çıkış işlevi dekoratörlerinde kullanın. Aşağıdaki scikit-learn örneği şema üretimini kullanır.

##### <a name="example-entry-script"></a>Örnek giriş komut dosyası

Aşağıdaki örnek, JSON verilerinin nasıl kabul edilip iade edilebildiğini göstermektedir:

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

Aşağıdaki örnek, veri çerçevesi kullanarak giriş verilerinin `<key: value>` sözlük olarak nasıl tanımlanabildiğini göstermektedir. Bu yöntem, Power BI'den dağıtılan web hizmetini tüketmek için desteklenir. ([Power BI'den web hizmetini nasıl tüketiriz hakkında daha fazla bilgi edinin](https://docs.microsoft.com/power-bi/service-machine-learning-integration).)

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


@input_schema('data', PandasParameterType(input_sample))
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

Daha fazla örnek için aşağıdaki komut dosyalarına bakın:

* [PyTorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch)
* [TensorFlow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/tensorflow)
* [Keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* [AutoML](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features)
* [ONNX](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* [İkili Veri](#binary)
* [CORS](#cors)

### <a name="3-define-inference-configuration"></a><a id="script"></a>3. Çıkarım yapılandırması tanımlayın
    
Aşağıdaki örnek, çalışma alanınızdan bir ortam yüklemeyi ve çıkarım yapılandırmasıyla birlikte kullanmayı gösterir:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig


myenv = Environment.get(workspace=ws, name='myenv', version='1')
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                   environment=myenv)
```

Ortamlar hakkında daha fazla bilgi için [bkz.](how-to-use-environments.md)

Çıkarım yapılandırması hakkında daha fazla bilgi için [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfı belgelerine bakın.

Çıkarım yapılandırması olan özel bir Docker görüntüsünü kullanma hakkında bilgi için, [özel Docker görüntüsünü kullanarak bir modeli nasıl dağıtılayabilirsiniz'](how-to-deploy-custom-docker-image.md)e bakın.

#### <a name="cli-example-of-inferenceconfig"></a>InferenceConfig CLI örneği

[!INCLUDE [inference config](../../includes/machine-learning-service-inference-config.md)]

Aşağıdaki komut, CLI'yi kullanarak bir modelin nasıl dağıtılangerektiğini gösterir:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

Bu örnekte, yapılandırma aşağıdaki ayarları belirtir:

* Modelin Python'u gerektirdiğini.
* Dağıtılan hizmete gönderilen web isteklerini işlemek için kullanılan [giriş komut dosyası.](#script)
* Çıkarım için gerekli Python paketlerini açıklayan Conda dosyası.

Çıkarım yapılandırması olan özel bir Docker görüntüsünü kullanma hakkında bilgi için, [özel Docker görüntüsünü kullanarak bir modeli nasıl dağıtılayabilirsiniz'](how-to-deploy-custom-docker-image.md)e bakın.

### <a name="4-optional-profile-your-model-to-determine-resource-utilization"></a><a id="profilemodel"></a>4. (İsteğe bağlı) Kaynak kullanımını belirlemek için modelinizi profille

Modelinizi kaydettikten ve dağıtım için gerekli diğer bileşenleri hazırladıktan sonra, dağıtılan hizmetin gereksinim edeceği CPU'yu ve belleği belirleyebilirsiniz. Profil oluşturma, modelinizi çalıştıran hizmeti sınar ve CPU kullanımı, bellek kullanımı ve yanıt gecikmesi gibi bilgileri döndürür. Ayrıca, kaynak kullanımına dayalı olarak CPU ve bellek için bir öneri sağlar.

Modelinizin profilini çıkarmak için şunları yapmanız gerekir:
* Kayıtlı bir model.
* Giriş komut dosyanıza ve çıkarım ortamı tanımınıza dayalı bir çıkarım yapılandırması.
* Her satırın örnek istek verilerini temsil eden bir dize içerdiği tek bir sütun tabular veri kümesi.

> [!IMPORTANT]
> Bu noktada yalnızca istek verilerinin bir dize olmasını bekleyen hizmetlerin profilini çıkarmayı destekliyoruz, örneğin: string serialized json, text, string serialized image, vb. Veri kümesinin (dize) her satırının içeriği HTTP isteğinin gövdesine konur ve puanlama için modeli kapsülleyen hizmete gönderilir.

Aşağıda, gelen istek verilerinin serileştirilmiş json içermesini bekleyen bir hizmetin profilini çıkarmak için bir giriş veri kümesini nasıl oluşturabileceğinize bir örnek verilmiştir. Bu durumda, aynı istek veri içeriğinin yüz örneğine dayalı bir veri kümesi oluşturduk. Gerçek dünya senaryolarında, özellikle model kaynak kullanımınız/davranışınız girdiye bağımlıysa, çeşitli girdiler içeren daha büyük veri kümeleri kullanmanızı öneririz.

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

Örnek istek verilerini içeren veri kümesini hazırladıktan sonra bir çıkarım yapılandırması oluşturun. Çıkarım yapılandırması score.py ve ortam tanımına dayanır. Aşağıdaki örnek, çıkarım yapılandırmasının nasıl oluşturulup profil oluşturmanın nasıl yapılacağını gösterir:

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

Aşağıdaki komut, CLI'yi kullanarak bir modelin profilini nasıl gösterin:

```azurecli-interactive
az ml model profile -g <resource-group-name> -w <workspace-name> --inference-config-file <path-to-inf-config.json> -m <model-id> --idi <input-dataset-id> -n <unique-name>
```

> [!TIP]
> Profil oluşturarak döndürülen bilgileri kalıcı olarak sürdürmek için, model için etiketleri veya özellikleri kullanın. Etiketleri veya özellikleri kullanmak, verileri model kayıt defterinde modelle birlikte depolar. Aşağıdaki örnekler, ve `requestedCpu` `requestedMemoryInGb` bilgileri içeren yeni bir etiket eklemeyi gösterir:
>
> ```python
> model.add_tags({'requestedCpu': details['requestedCpu'],
>                 'requestedMemoryInGb': details['requestedMemoryInGb']})
> ```
>
> ```azurecli-interactive
> az ml model profile -g <resource-group-name> -w <workspace-name> --i <model-id> --add-tag requestedCpu=1 --add-tag requestedMemoryInGb=0.5
> ```

## <a name="deploy-to-target"></a>Hedefe dağıl

Dağıtım modelleri dağıtmak için çıkarım yapılandırma dağıtım yapılandırmasını kullanır. Dağıtım işlemi, bilgi işlem hedefine bakılmaksızın benzerdir. AKS kümesine bir başvuru sağlamanız gerektiğinden, AKS'ye dağıtım biraz farklıdır.

### <a name="choose-a-compute-target"></a>Bir işlem hedefi seçin

Web hizmeti dağıtımınızı barındırmak için aşağıdaki işlem hedeflerini veya hesaplama kaynaklarını kullanabilirsiniz:

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

### <a name="define-your-deployment-configuration"></a>Dağıtım yapılandırmanızı tanımlama

Modelinizi dağıtmadan önce dağıtım yapılandırmasını tanımlamanız gerekir. *Dağıtım yapılandırması, web hizmetini barındıracak bilgi işlem hedefine özgüdür.* Örneğin, bir modeli yerel olarak dağıttığınızda, hizmetin istekleri kabul ettiği bağlantı noktasını belirtmeniz gerekir. Dağıtım yapılandırması giriş komut dosyanızın bir parçası değildir. Model ve giriş komut dosyasını barındıracak bilgi işlem hedefinin özelliklerini tanımlamak için kullanılır.

Örneğin, çalışma alanınızla ilişkili bir Azure Kubernetes Hizmeti (AKS) örneğiniz yoksa, bilgi işlem kaynağını da oluşturmanız gerekebilir.

Aşağıdaki tablo, her bir işlem hedefi için dağıtım yapılandırması oluşturma örneğini sağlar:

| İşlem hedefi | Dağıtım yapılandırma örneği |
| ----- | ----- |
| Yerel | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Yerel, Azure Kapsayıcı Örnekleri ve AKS web hizmetleri için `azureml.core.webservice`sınıflar aşağıdakilerden içe aktarılabilir:

```python
from azureml.core.webservice import AciWebservice, AksWebservice, LocalWebservice
```

### <a name="securing-deployments-with-tls"></a>TLS ile dağıtımların güvenliğini sağlama

Bir web hizmeti dağıtımının nasıl güvenli hale sağlanabildiğini öğrenmek için [TLS'yi etkinleştir mecnuna bakın ve dağıtın.](how-to-secure-web-service.md#enable)

### <a name="local-deployment"></a><a id="local"></a>Yerel dağıtım

Bir modeli yerel olarak dağıtmak için Docker'ın yerel makinenize yüklenmesi gerekir.

#### <a name="using-the-sdk"></a>SDK’yı kullanarak

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Daha fazla bilgi için [LocalWebservice,](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py) [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)ve [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py)için belgelere bakın.

#### <a name="using-the-cli"></a>CLI'yi kullanma

CLI kullanarak bir model dağıtmak için aşağıdaki komutu kullanın. Kayıtlı `mymodel:1` modelin adı ve sürümüyle değiştirin:

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
```

[!INCLUDE [aml-local-deploy-config](../../includes/machine-learning-service-local-deploy-config.md)]

Daha fazla bilgi için [az ml modelinin](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) belgeleri dağıtması için bkz.

### <a name="understanding-service-state"></a>Hizmet durumunu anlama

Model dağıtımı sırasında, tam olarak dağıtılırken hizmet durumu değişikliğini görebilirsiniz.

Aşağıdaki tabloda farklı hizmet durumları açıklanır:

| Webservice durumu | Açıklama | Son durum mu?
| ----- | ----- | ----- |
| Geçiş | Hizmet dağıtım sürecindedir. | Hayır |
| Uygun Değil | Hizmet dağıtıldı, ancak şu anda erişilenememiştir.  | Hayır |
| Programsız | Hizmet, kaynak yetersizliği nedeniyle şu anda dağıtılamaz. | Hayır |
| Başarısız | Hizmet bir hata veya kilitlenme nedeniyle dağıtılamamış. | Evet |
| Sağlam | Hizmet sağlıklıdır ve bitiş noktası kullanılabilir. | Evet |

### <a name="compute-instance-web-service-devtest"></a><a id="notebookvm"></a>Bilgi işlem örneği web hizmeti (geliştirme/test)

Bkz. [Azure Machine Learning bilgi işlem örneğine bir model dağıtın.](how-to-deploy-local-container-notebook-vm.md)

### <a name="azure-container-instances-devtest"></a><a id="aci"></a>Azure Kapsayıcı Örnekleri (geliştirme/test)

Bkz. [Azure Kapsayıcı Örneklerine Dağıt.](how-to-deploy-azure-container-instance.md)

### <a name="azure-kubernetes-service-devtest-and-production"></a><a id="aks"></a>Azure Kubernetes Hizmeti (geliştirme/test ve üretim)

Bkz. [Azure Kubernetes Hizmetine Dağıt.](how-to-deploy-azure-kubernetes-service.md)

### <a name="ab-testing-controlled-rollout"></a>A/B Testi (kontrollü kullanıma alma)
Daha fazla bilgi için [ML modellerinin Kontrollü kullanıma sunulması](how-to-deploy-azure-kubernetes-service.md#deploy-models-to-aks-using-controlled-rollout-preview) bölümüne bakın.

## <a name="consume-web-services"></a>Web hizmetlerini kullanma

Dağıtılan her web hizmeti, herhangi bir programlama dilinde istemci uygulamaları oluşturabilmeniz için bir REST bitiş noktası sağlar.
Hizmetiniz için anahtar tabanlı kimlik doğrulamasını etkinleştirdim, istek üstbilginizde belirteç olarak bir hizmet anahtarı sağlamanız gerekir.
Hizmetiniz için belirteç tabanlı kimlik doğrulamasını etkinleştirdimyseniz, istek üstbilginizde taşıyıcı belirteci olarak bir Azure Machine Learning JSON Web Belirteci (JWT) sağlamanız gerekir. 

Birincil fark **anahtarları statik ve el ile rejenere edilebilir**ve **belirteçleri sona erme de yenilenmesi gerekir.** Anahtar tabanlı auth, Azure Kapsayıcı Örneği ve Azure Kubernetes Service tarafından dağıtılan web hizmetleri için desteklenir ve belirteç tabanlı auth **yalnızca** Azure Kubernetes Hizmeti dağıtımları için kullanılabilir. Daha fazla bilgi ve belirli kod örnekleri için kimlik doğrulamanın [nasıl yapılacağını](how-to-setup-authentication.md#web-service-authentication) görün.

> [!TIP]
> Hizmeti dağıttıktan sonra şema JSON belgesini alabilirsiniz. URI'yi yerel web hizmetinin Swagger `service.swagger_uri`dosyasına getirmek için dağıtılan web hizmetindeki [swagger_uri özelliğini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri) (örneğin,) kullanın.

### <a name="request-response-consumption"></a>İstek-yanıt tüketimi

Python'da hizmetinizi nasıl çağırabilirsiniz açısından bir çoğunluk çeşitli:
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

Daha fazla bilgi için [bkz.](how-to-consume-web-service.md)

### <a name="web-service-schema-openapi-specification"></a>Web hizmeti şeması (OpenAPI belirtimi)

Dağıtımınızda otomatik şema oluşturma kullandıysanız, [swagger_uri özelliğini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri)kullanarak hizmet için OpenAPI belirtiminin adresini alabilirsiniz. (Örneğin, `print(service.swagger_uri)`.) Belirtimi almak için GET isteğini kullanın veya uri'yi tarayıcıda açın.

Aşağıdaki JSON belgesi, dağıtım için oluşturulan şema (OpenAPI belirtimi) örneğidir:

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

Daha fazla bilgi için [OpenAPI belirtimine](https://swagger.io/specification/)bakın.

Belirtimden istemci kitaplıkları oluşturabilen bir yardımcı program [için](https://github.com/swagger-api/swagger-codegen)bkz.

### <a name="batch-inference"></a><a id="azuremlcompute"></a>Toplu çıkarım
Azure Machine Learning Compute hedefleri, Azure Machine Learning tarafından oluşturulur ve yönetilir. Azure Machine Learning ardışık işlem işlerinden toplu tahmin için kullanılabilirler.

Azure Machine Learning Compute ile toplu çıkarım için [toplu tahminleri nasıl çalıştırılabilirsiniz.](tutorial-pipeline-batch-scoring-classification.md)

### <a name="iot-edge-inference"></a><a id="iotedge"></a>IoT Edge çıkarımı
Kenara dağıtım desteği önizlemededir. Daha fazla bilgi için azure [makine öğrenimini ioT edge modülü olarak dağıt'a](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning)bakın.


## <a name="update-web-services"></a><a id="update"></a>Web hizmetlerini güncelleştirin

[!INCLUDE [aml-update-web-service](../../includes/machine-learning-update-web-service.md)]

## <a name="continuously-deploy-models"></a>Modelleri sürekli olarak dağıtın

[Azure DevOps](https://azure.microsoft.com/services/devops/)için Machine Learning uzantısını kullanarak modelleri sürekli olarak dağıtabilirsiniz. Azure Makine Öğrenimi çalışma alanında yeni bir makine öğrenme modeli kaydedildiğinde, bir dağıtım ardışık hattını tetiklemek için Azure DevOps'lerin Makine Öğrenimi uzantısını kullanabilirsiniz.

1. Uygulamanızın sürekli entegrasyonunu ve uygulamanızı herhangi bir platforma veya buluta teslim etmesini mümkün kılan [Azure Boru Hatları](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops)için kaydolun. (Azure Ardışık Hatları'nın Machine [Learning ardışık hatlarıyla](concept-ml-pipelines.md#compare)aynı olmadığını unutmayın.)

1. [Bir Azure DevOps projesi oluşturun.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Azure [Ardışık Hatları için Makine Öğrenimi uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList)yükleyin.

1. Azure Machine Learning çalışma alanınıza bir hizmet temel bağlantısı kurmak için hizmet bağlantılarını kullanın, böylece yapılarınıza erişebilirsiniz. Proje ayarlarına gidin, **Hizmet bağlantılarını**seçin ve ardından **Azure Kaynak Yöneticisi'ni**seçin:

    [![Azure Kaynak Yöneticisi'ni seçin](media/how-to-deploy-and-where/view-service-connection.png)](media/how-to-deploy-and-where/view-service-connection-expanded.png)

1. Kapsam **düzeyi** listesinde **AzureMLWorkspace'i**seçin ve ardından değerlerin geri kalanını girin:

    ![AzureMLWorkspace'i seçin](./media/how-to-deploy-and-where/resource-manager-connection.png)

1. Azure Ardışık Hatları'nı kullanarak makine öğrenimi modelinizi sürekli olarak dağıtmak için **sürüm'u**seçin. Yeni bir yapı ekleyin ve ardından **AzureML Modeli** yapısını ve daha önce oluşturduğunuz hizmet bağlantısını seçin. Dağıtımı tetiklemek için modeli ve sürümü seçin:

    [![AzureML Modelini Seçin](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)](media/how-to-deploy-and-where/enable-modeltrigger-artifact-expanded.png)

1. Model artifakı üzerinde model tetikleyici etkinleştirin. Tetikleyiciyi açtığınızda, bu modelin belirtilen sürümü (yani en yeni sürümü) çalışma alanınızda her kaydedildiğinde, bir Azure DevOps sürüm ardışık hattı tetiklenir.

    [![Model tetikleyicisini etkinleştirme](media/how-to-deploy-and-where/set-modeltrigger.png)](media/how-to-deploy-and-where/set-modeltrigger-expanded.png)

Daha fazla örnek proje ve örnek için GitHub'daki örnek repo'lara bakın:

* [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
* [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)

## <a name="download-a-model"></a>Bir model indirin
Kendi yürütme ortamınızda kullanmak için modelinizi indirmek istiyorsanız, bunu aşağıdaki SDK / CLI komutları ile yapabilirsiniz:

Sdk:
```python
model_path = Model(ws,'mymodel').download()
```

Clı:
```azurecli-interactive
az ml model download --model-id mymodel:1 --target-dir model_folder
```

## <a name="preview-no-code-model-deployment"></a>(Önizleme) Kodsuz model dağıtımı

Kodsuz model dağıtımı şu anda önizlemededir ve aşağıdaki makine öğrenimi çerçevelerini destekler:

### <a name="tensorflow-savedmodel-format"></a>Tensorflow SavedModel biçimi
Tensör akış modelleri no-code model dağıtım ile çalışmak için **SavedModel biçiminde** kayıtlı olması gerekir.

SavedModel nasıl oluşturulabildiğini öğrenmek için lütfen [bu bağlantıya](https://www.tensorflow.org/guide/saved_model) bakın.

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

ONNX model kaydı ve dağıtımı herhangi bir ONNX çıkarım grafiği için desteklenir. Ön işlem ve işlem sonrası adımlar şu anda desteklenmez.

Aşağıda, Bir MNIST ONNX modelinin nasıl kaydedilip dağıtılanabildiğini anlatan bir örnek verilmiştir:

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

Pytorch kullanıyorsanız, [PyTorch'tan ONNX'e modelleri dışa](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) aktarmada dönüşüm ve sınırlamalar hakkında ayrıntılar vardır. 

### <a name="scikit-learn-models"></a>Scikit-learn modelleri

Tüm yerleşik scikit-learn model türleri için kod modeli dağıtımı desteklenmez.

Burada, ek kodu olmayan bir sklearn modelinin nasıl kaydedilip dağıtılanacaklarına dair bir örnek verilmiştir:

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

NOT: predict_proba destekleyen modeller varsayılan olarak bu yöntemi kullanır. Tahmin kullanmak için bunu geçersiz kılmak için POST gövdesini aşağıdaki gibi değiştirebilirsiniz:
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

NOT: Bu bağımlılıklar önceden oluşturulmuş sklearn çıkarım kabına dahildir:

```yaml
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
```

## <a name="package-models"></a>Paket modelleri

Bazı durumlarda, modeli dağıtmadan bir Docker görüntüsü oluşturmak isteyebilirsiniz (örneğin, [Azure Uygulama Hizmeti'ne dağıtmayı](how-to-deploy-app-service.md)planlıyorsanız). Veya resmi indirip yerel bir Docker yüklemesi üzerinde çalıştırmak isteyebilirsiniz. Görüntüyü oluşturmak, incelemek, değiştirmek ve görüntüyü el ile oluşturmak için kullanılan dosyaları indirmek bile isteyebilirsiniz.

Model paketleme bunları yapmanızı sağlar. Bir modeli bir web hizmeti olarak barındırmak için gereken tüm varlıkları paketler ve tam olarak oluşturulmuş bir Docker görüntüsünü veya bir model oluşturmak için gereken dosyaları indirmenize olanak tanır. Model ambalajı kullanmanın iki yolu vardır:

**Paketlenmiş bir model indirin:** Modeli ve web hizmeti olarak barındırmak için gereken diğer dosyaları içeren bir Docker görüntüsünü indirin.

**Dockerdosyası oluşturun:** Dockerfile, model, giriş komut dosyası ve docker görüntü oluşturmak için gerekli diğer varlıkları indirin. Daha sonra, görüntüyü yerel olarak oluşturmadan önce dosyaları inceleyebilir veya değişiklik yapabilirsiniz.

Her iki paket de yerel bir Docker görüntüsü elde etmek için kullanılabilir.

> [!TIP]
> Paket oluşturma, bir modeli dağıtmaya benzer. Kayıtlı bir model ve çıkarım yapılandırması kullanırsınız.

> [!IMPORTANT]
> Tam olarak oluşturulmuş bir görüntüyü indirmek veya yerel olarak bir resim oluşturmak için [Docker'ın](https://www.docker.com) geliştirme ortamınıza yüklenmesi gerekir.

### <a name="download-a-packaged-model"></a>Paketlenmiş bir model indirin

Aşağıdaki örnek, çalışma alanınız için Azure kapsayıcı kayıt defterine kayıtlı bir resim oluşturur:

```python
package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
```

Bir paket oluşturduktan sonra, `package.pull()` resmi yerel Docker ortamınıza çekmek için kullanabilirsiniz. Bu komutun çıktısı görüntünün adını görüntüler. Örnek: 

`Status: Downloaded newer image for myworkspacef78fd10.azurecr.io/package:20190822181338`. 

Modeli indirdikten sonra yerel `docker images` görüntüleri listelemek için komutu kullanın:

```text
REPOSITORY                               TAG                 IMAGE ID            CREATED             SIZE
myworkspacef78fd10.azurecr.io/package    20190822181338      7ff48015d5bd        4 minutes ago       1.43 GB
```

Bu resme dayalı yerel bir kapsayıcı başlatmak için, kabuk veya komut satırından adlandırılmış bir kapsayıcı başlatmak için aşağıdaki komutu kullanın. `<imageid>` Değeri komuttarafından döndürülen görüntü `docker images` kimliğiyle değiştirin.

```bash
docker run -p 6789:5001 --name mycontainer <imageid>
```

Bu komut, adlı `myimage`görüntünün en son sürümünü başlatır. Yerel bağlantı noktası 6789'u web hizmetinin dinlediği konteynırdaki bağlantı noktasına eşler (5001). Ayrıca, kapsayıcının `mycontainer` durmasını kolaylaştıran adı kapsayıcıya atar. Kapsayıcı başlatıldıktan sonra, ''ye `http://localhost:6789/score`istekgönderebilirsiniz.

### <a name="generate-a-dockerfile-and-dependencies"></a>Dockerfile ve bağımlılıklar oluşturma

Aşağıdaki örnek, yerel olarak bir görüntü oluşturmak için gereken Dockerfile, model ve diğer varlıkları nasıl karşıdan yükleyeceklerini gösterir. Parametre, `generate_dockerfile=True` dosyaları tam olarak oluşturulmuş bir görüntü değil istediğinizi gösterir.

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

Bu kod, görüntüyü `imagefiles` dizine oluşturmak için gereken dosyaları karşıdan yükler. Kaydedilen dosyalarda yer alan Dockerfile, Azure kapsayıcı kayıt defterinde depolanan temel bir görüntüye başvurur. Resmi yerel Docker yüklemenizde oluştururken, kayıt defterine kimlik doğrulamak için adresi, kullanıcı adını ve parolayı kullanmanız gerekir. Yerel bir Docker yüklemesi kullanarak görüntüyü oluşturmak için aşağıdaki adımları kullanın:

1. Bir kabuk veya komut satırı oturumundan, Azure kapsayıcı kayıt defteriyle Docker'ın kimliğini doğrulamak için aşağıdaki komutu kullanın. Değiştir `<address>` `<username>`, `<password>` ve alınan `package.get_container_registry()`değerlerle .

    ```bash
    docker login <address> -u <username> -p <password>
    ```

2. Görüntüyü oluşturmak için aşağıdaki komutu kullanın. Dosyaları `<imagefiles>` `package.save()` kaydettiği dizinin yolu ile değiştirin.

    ```bash
    docker build --tag myimage <imagefiles>
    ```

    Bu komut, görüntü `myimage`adını .

Görüntünün üretilir olduğunu doğrulamak `docker images` için komutu kullanın. Listedeki `myimage` resmi görmelisiniz:

```text
REPOSITORY      TAG                 IMAGE ID            CREATED             SIZE
<none>          <none>              2d5ee0bf3b3b        49 seconds ago      1.43 GB
myimage         latest              739f22498d64        3 minutes ago       1.43 GB
```

Bu resme dayalı yeni bir kapsayıcı başlatmak için aşağıdaki komutu kullanın:

```bash
docker run -p 6789:5001 --name mycontainer myimage:latest
```

Bu komut, adlı `myimage`görüntünün en son sürümünü başlatır. Yerel bağlantı noktası 6789'u web hizmetinin dinlediği konteynırdaki bağlantı noktasına eşler (5001). Ayrıca, kapsayıcının `mycontainer` durmasını kolaylaştıran adı kapsayıcıya atar. Kapsayıcı başlatıldıktan sonra, ''ye `http://localhost:6789/score`istekgönderebilirsiniz.

### <a name="example-client-to-test-the-local-container"></a>Yerel kapsayıcıyı sınamak için örnek istemci

Aşağıdaki kod, kapsayıcıyla birlikte kullanılabilecek bir Python istemcisi örneğidir:

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

Örneğin diğer programlama dillerinde istemciler, [bkz.](how-to-consume-web-service.md)

### <a name="stop-the-docker-container"></a>Docker konteynerini durdurun

Kapsayıcıyı durdurmak için, farklı bir kabuk veya komut satırından aşağıdaki komutu kullanın:

```bash
docker kill mycontainer
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtılan bir web hizmetini silmek için `service.delete()`.
Kayıtlı bir modeli silmek için `model.delete()`.

Daha fazla bilgi için [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--) ve [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--)belgelerine bakın.

<a id="advanced-entry-script"></a>
## <a name="advanced-entry-script-authoring"></a>Gelişmiş giriş komut dosyası yazma

<a id="binary"></a>

### <a name="binary-data"></a>İkili veriler

Modeliniz bir resim gibi ikili verileri kabul ederse, dağıtımınız için kullanılan dosyayı `score.py` ham HTTP isteklerini kabul etmek üzere değiştirmeniz gerekir. Ham verileri kabul etmek `AMLRequest` için, giriş komut `@rawhttp` dosyasındaki sınıfı `run()` kullanın ve işyerine dekoratör ekleyin.

İkili verileri kabul eden `score.py` bir örnek aşağıda verilmiştir:

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
> Sınıf `AMLRequest` `azureml.contrib` ad alanındadır. Hizmeti geliştirmek için çalışırken, bu ad alanındaki varlıklar sık sık değişir. Bu ad alanındaki her şey, Microsoft tarafından tam olarak desteklenmeyen bir önizleme olarak kabul edilmelidir.
>
> Bunu yerel geliştirme ortamınızda test etmeniz gerekiyorsa, bileşenleri aşağıdaki komutu kullanarak yükleyebilirsiniz:
>
> ```shell
> pip install azureml-contrib-services
> ```

<a id="cors"></a>

### <a name="cross-origin-resource-sharing-cors"></a>Orijinler arası kaynak paylaşımı (CORS)

Orijinler arası kaynak paylaşımı, bir web sayfasındaki kaynakların başka bir etki alanından istenmesine izin vermenin bir yoludur. CORS, istemci isteği yle gönderilen ve hizmet yanıtı ile döndürülen HTTP üstbilgileriyle çalışır. CORS ve geçerli üstbilgiler hakkında daha fazla bilgi için Vikipedi'de [Kökenler Arası kaynak paylaşımına](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) bakın.

Model dağıtımınızı CORS'i destekleyecek şekilde `AMLResponse` yapılandırmak için giriş komut dosyanızdaki sınıfı kullanın. Bu sınıf, üstbilgi yanıt nesnesi üzerinde ayarlamanızı sağlar.

Aşağıdaki örnek, `Access-Control-Allow-Origin` giriş komut dosyasından yanıt için üstbilgi ayarlar:

```python
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

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
> Sınıf `AMLResponse` `azureml.contrib` ad alanındadır. Hizmeti geliştirmek için çalışırken, bu ad alanındaki varlıklar sık sık değişir. Bu ad alanındaki her şey, Microsoft tarafından tam olarak desteklenmeyen bir önizleme olarak kabul edilmelidir.
>
> Bunu yerel geliştirme ortamınızda test etmeniz gerekiyorsa, bileşenleri aşağıdaki komutu kullanarak yükleyebilirsiniz:
>
> ```shell
> pip install azureml-contrib-services
> ```

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Docker görüntüsünü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning aracılığıyla bir web hizmetini güvence altına almak için TLS'yi kullanın](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir Azure Machine Learning modelini kullanma](how-to-consume-web-service.md)
* [Azure Machine Learning modellerinizi Uygulama Öngörüleri ile izleyin](how-to-enable-app-insights.md)
* [Üretimdeki modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve tetikleyiciler oluşturma](how-to-use-event-grid.md)

