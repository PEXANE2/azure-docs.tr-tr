---
author: gvashishtha
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: include
ms.date: 07/31/2020
ms.author: gopalv
ms.openlocfilehash: 97f0412141f15ad0a72c02b92cfcf089b61db0cf
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88120362"
---
## <a name="prerequisites"></a>Ön koşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](../articles/machine-learning/how-to-manage-workspace.md).
- Bir model. Eğitilen bir modeliniz yoksa, [Bu öğreticide](https://aka.ms/azml-deploy-cloud)verilen model ve bağımlılık dosyalarını kullanabilirsiniz.
- [Python için Azure Machine Learning yazılım geliştirme seti (SDK)](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)


## <a name="connect-to-your-workspace"></a>Çalışma alanınıza bağlanma

```python
from azureml.core import Workspace
ws = Workspace.from_config(path=".file-path/ws_config.json")
```

Bir çalışma alanına bağlanmak için SDK 'Yı kullanma hakkında daha fazla bilgi için bkz. [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace) belgeleri.

## <a name="register-your-model"></a><a id="registermodel"></a>Modelinizi kaydetme

Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları çalışma alanına tek bir model olarak kaydedebilirsiniz. Dosyaları kaydettikten sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kaydettiğiniz tüm dosyaları alabilirsiniz.

> [!TIP]
> Bir modeli kaydettiğinizde, bir bulut konumunun (bir eğitim çalıştırmasında) ya da yerel bir dizinin yolunu sağlarsınız. Bu yol, kayıt işleminin bir parçası olarak karşıya yüklenecek dosyaları bulmak için yeterlidir. Giriş betiğinde kullanılan yol ile eşleşmesi gerekmez. Daha fazla bilgi için bkz. [giriş betiğinizdeki model dosyalarını bulma](../articles/machine-learning/how-to-deploy-advanced-entry-script.md#load-registered-models).

Machine Learning modelleri Azure Machine Learning çalışma alanınıza kaydedilir. Model Azure Machine Learning veya herhangi bir yerden gelebilir. Bir modeli kaydederken, isteğe bağlı olarak model hakkında meta veriler sağlayabilirsiniz. `tags` `properties` Model kaydına uyguladığınız ve sözlükleri, modelleri filtrelemek için kullanılabilir.

Aşağıdaki örneklerde bir modelin nasıl kaydedileceği gösterilmektedir.

### <a name="register-a-model-from-an-azure-ml-training-run"></a>Azure ML eğitim çalıştırmasında bir modeli kaydetme

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


### <a name="register-a-model-from-a-local-file"></a>Yerel dosyadan model kaydetme

Modelin yerel yolunu sağlayarak bir modeli kaydedebilirsiniz. Bir klasörün ya da tek bir dosyanın yolunu sağlayabilirsiniz. Bu yöntemi, Azure Machine Learning eğitilen ve daha sonra indirilen modelleri kaydetmek için kullanabilirsiniz. Bu yöntemi, Azure Machine Learning dışında eğitilen modelleri kaydetmek için de kullanabilirsiniz.

[!INCLUDE [trusted models](machine-learning-service-trusted-model.md)]

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

Daha fazla bilgi için [model sınıfına](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py)yönelik belgelere bakın.

Azure Machine Learning dışında eğitilen modellerle çalışma hakkında daha fazla bilgi için bkz. [var olan bir modeli dağıtma](../articles/machine-learning/how-to-deploy-existing-model.md).

## <a name="define-an-entry-script"></a>Giriş betiği tanımlama

[!INCLUDE [write entry script](machine-learning-entry-script.md)]

## <a name="define-an-inferenceconfig"></a>Bir ınenceconfig tanımlayın

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

Azure Machine Learning ile Python ortamlarını kullanma ve özelleştirme hakkında kapsamlı bir tartışma için bkz. [Azure Machine Learning yazılım ortamları oluşturma & kullanma](../articles/machine-learning/how-to-use-environments.md)

Bir çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](../articles/machine-learning/how-to-deploy-custom-docker-image.md).


Aşağıdaki örnek, çalışma alanınızdan bir ortamı yüklemeyi ve ardından çıkarım yapılandırmasıyla kullanmayı gösterir:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig


myenv = Environment.get(workspace=ws, name='myenv', version='1')
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                    environment=myenv)
```

Ortamlar hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](../articles/machine-learning/how-to-use-environments.md).

Çıkarım yapılandırması hakkında daha fazla bilgi için bkz. [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) sınıfı belgeleri.

## <a name="choose-a-compute-target"></a>İşlem hedefi seçin

[!INCLUDE [aml-compute-target-deploy](aml-compute-target-deploy.md)]

## <a name="define-a-deploymentconfiguration"></a>Bir DeploymentConfiguration tanımlayın

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

## <a name="deploy-your-model"></a>Modelinizi dağıtın

Artık modelinizi dağıtmaya hazırsınız. Aşağıdaki örnekte yerel bir dağıtım gösterilmektedir. Sözdizimi, önceki adımda seçtiğiniz işlem hedefine bağlı olarak değişir.

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Daha fazla bilgi için bkz. [Localwebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py), [model. deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)ve [WebService](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py)belgeleri.

## <a name="delete-resources"></a>Kaynakları silme

Dağıtılmış bir Web hizmetini silmek için kullanın `service.delete()` .
Kayıtlı bir modeli silmek için kullanın `model.delete()` .

Daha fazla bilgi için bkz. [WebService. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--) ve [model. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--)belgeleri.