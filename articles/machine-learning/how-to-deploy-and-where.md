---
title: Modellerin nasıl ve nereye dağıtılacağı
titleSuffix: Azure Machine Learning
description: Azure Container Instances, Azure Kubernetes hizmeti, Azure IoT Edge ve alan programlanabilir kapı dizileri dahil olmak üzere Azure Machine Learning modellerinizi nasıl ve nereye dağıtacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.reviewer: larryfr
ms.date: 07/08/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: fd58adf9e845e296725131da96c3319a72895cb8
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89645690"
---
# <a name="deploy-models-with-azure-machine-learning"></a>Azure Machine Learning ile modelleri dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Machine Learning modelinizi bir Web hizmeti olarak Azure bulutu 'nda veya Azure IoT Edge cihazlarda dağıtmayı öğrenin.

Modeli nerede dağıtırsanız dağıtın benzer bir iş akışı kullanılır:

1. Modeli kaydedin.
1. Çıkarım yapılandırmasını hazırlama
1. Giriş betiği hazırlama ( [kod içermeyen dağıtım](how-to-deploy-no-code-deployment.md)kullanmadıkça)
1. Modeli işlem hedefine dağıtın.
1. Web hizmeti olarak da bilinen dağıtılan modeli test edin.

Dağıtım iş akışında yer alan kavramlar hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri yönetme, dağıtma ve izleme](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Önkoşullar

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).
- Bir model. Eğitilen bir modeliniz yoksa, [Bu öğreticide](https://aka.ms/azml-deploy-cloud)verilen model ve bağımlılık dosyalarını kullanabilirsiniz.
- [Machine Learning hizmeti Için Azure komut satırı arabirimi (CLI) uzantısı](reference-azure-machine-learning-cli.md).

# <a name="python"></a>[Python](#tab/python)

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).
- Bir model. Eğitilen bir modeliniz yoksa, [Bu öğreticide](https://aka.ms/azml-deploy-cloud)verilen model ve bağımlılık dosyalarını kullanabilirsiniz.
- [Python için Azure Machine Learning yazılım geliştirme seti (SDK)](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true).

---

## <a name="connect-to-your-workspace"></a>Çalışma alanınıza bağlanma

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

[Abonelik bağlamını ayarlamak](/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription)IÇIN Azure CLI belgelerindeki yönergeleri izleyin.

Ardından şunları yapın:

```azurecli-interactive
az ml workspace list --resource-group=<my resource group>
```

erişiminiz olan çalışma alanlarını görmek için.

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core import Workspace
ws = Workspace.from_config(path=".file-path/ws_config.json")
```

Bir çalışma alanına bağlanmak için SDK 'Yı kullanma hakkında daha fazla bilgi için bkz. [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#&preserve-view=trueworkspace) belgeleri.


---


## <a name="register-your-model"></a><a id="registermodel"></a> Modelinizi kaydetme

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları çalışma alanına tek bir model olarak kaydedebilirsiniz. Dosyaları kaydettikten sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kaydettiğiniz tüm dosyaları alabilirsiniz.

> [!TIP]
> Bir modeli kaydettiğinizde, bir bulut konumunun (bir eğitim çalıştırmasında) ya da yerel bir dizinin yolunu sağlarsınız. Bu yol, kayıt işleminin bir parçası olarak karşıya yüklenecek dosyaları bulmak için yeterlidir. Giriş betiğinde kullanılan yol ile eşleşmesi gerekmez. Daha fazla bilgi için bkz. [giriş betiğinizdeki model dosyalarını bulma](how-to-deploy-advanced-entry-script.md#load-registered-models).

Machine Learning modelleri Azure Machine Learning çalışma alanınıza kaydedilir. Model Azure Machine Learning veya herhangi bir yerden gelebilir. Bir modeli kaydederken, isteğe bağlı olarak model hakkında meta veriler sağlayabilirsiniz. `tags` `properties` Model kaydına uyguladığınız ve sözlükleri, modelleri filtrelemek için kullanılabilir.

Aşağıdaki örneklerde bir modelin nasıl kaydedileceği gösterilmektedir.

### <a name="register-a-model-from-an-azure-ml-training-run"></a>Azure ML eğitim çalıştırmasında bir modeli kaydetme

```azurecli-interactive
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid --tag area=mnist
```

[!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

`--asset-path`Parametresi, modelin bulut konumunu ifade eder. Bu örnekte, tek bir dosyanın yolu kullanılır. Model kaydına birden çok dosya eklemek için, `--asset-path` dosyaları içeren bir klasörün yoluna ayarlayın.

### <a name="register-a-model-from-a-local-file"></a>Yerel dosyadan model kaydetme

```azurecli-interactive
az ml model register -n onnx_mnist -p mnist/model.onnx
```

Model kaydına birden çok dosya eklemek için, `-p` dosyaları içeren bir klasörün yoluna ayarlayın.

Hakkında daha fazla bilgi için `az ml model register` [başvuru belgelerine](/cli/azure/ext/azure-cli-ml/ml/model)bakın.

# <a name="python"></a>[Python](#tab/python)


Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları çalışma alanına tek bir model olarak kaydedebilirsiniz. Dosyaları kaydettikten sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kaydettiğiniz tüm dosyaları alabilirsiniz.

> [!TIP]
> Bir modeli kaydettiğinizde, bir bulut konumunun (bir eğitim çalıştırmasında) ya da yerel bir dizinin yolunu sağlarsınız. Bu yol, kayıt işleminin bir parçası olarak karşıya yüklenecek dosyaları bulmak için yeterlidir. Giriş betiğinde kullanılan yol ile eşleşmesi gerekmez. Daha fazla bilgi için bkz. [giriş betiğinizdeki model dosyalarını bulma](how-to-deploy-advanced-entry-script.md#load-registered-models).

Machine Learning modelleri Azure Machine Learning çalışma alanınıza kaydedilir. Model Azure Machine Learning veya herhangi bir yerden gelebilir. Bir modeli kaydederken, isteğe bağlı olarak model hakkında meta veriler sağlayabilirsiniz. `tags` `properties` Model kaydına uyguladığınız ve sözlükleri, modelleri filtrelemek için kullanılabilir.

Aşağıdaki örneklerde bir modelin nasıl kaydedileceği gösterilmektedir.

### <a name="register-a-model-from-an-azure-ml-training-run"></a>Azure ML eğitim çalıştırmasında bir modeli kaydetme

  Bir modeli eğitebilmeniz için SDK 'yı kullandığınızda, modeli eğitidiğinize bağlı olarak bir [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py&preserve-view=true) nesnesi ya da bir [oto mlrun](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun) nesnesi alabilirsiniz. Her nesne, bir deneme çalıştırması tarafından oluşturulan bir modeli kaydetmek için kullanılabilir.

  + Bir nesneden model kaydetme `azureml.core.Run` :
 
    ```python
    model = run.register_model(model_name='sklearn_mnist',
                               tags={'area': 'mnist'},
                               model_path='outputs/sklearn_mnist_model.pkl')
    print(model.name, model.id, model.version, sep='\t')
    ```

    `model_path`Parametresi, modelin bulut konumunu ifade eder. Bu örnekte, tek bir dosyanın yolu kullanılır. Model kaydına birden çok dosya eklemek için, `model_path` dosyaları içeren bir klasörün yoluna ayarlayın. Daha fazla bilgi için bkz [. Run. register_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#&preserve-view=trueregister-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-) belgeleri.

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

Daha fazla bilgi için [model sınıfına](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py&preserve-view=true)yönelik belgelere bakın.

Azure Machine Learning dışında eğitilen modellerle çalışma hakkında daha fazla bilgi için bkz. [var olan bir modeli dağıtma](how-to-deploy-existing-model.md).

---

## <a name="define-an-entry-script"></a>Giriş betiği tanımlama

[!INCLUDE [write entry script](../../includes/machine-learning-entry-script.md)]


## <a name="define-an-inference-configuration"></a>Bir çıkarım yapılandırması tanımlama

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

[!INCLUDE [inference config](../../includes/machine-learning-service-inference-config.md)]

Aşağıdaki komut, CLı kullanarak bir modelin nasıl dağıtılacağını göstermektedir:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

Bu örnekte, yapılandırma aşağıdaki ayarları belirtir:

* Modelin Python gerektirdiğini
* Dağıtılan hizmete gönderilen Web isteklerini işlemek için kullanılan [giriş betiği](#define-an-entry-script)
* Çıkarım için gereken Python paketlerini açıklayan Conda dosyası

Bir çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md).

# <a name="python"></a>[Python](#tab/python)

Bir çıkarım yapılandırması, modelinizi içeren Web hizmeti ayarlamayı açıklar. Modeli dağıtırken daha sonra kullanılır.

Çıkarım yapılandırması, dağıtımınız için gereken yazılım bağımlılıklarını tanımlamak için Azure Machine Learning ortamlar kullanır. Ortamlar, eğitim ve dağıtım için gereken yazılım bağımlılıklarını oluşturmanıza, yönetmenize ve yeniden kullanmanıza olanak tanır. Özel bağımlılık dosyalarından bir ortam oluşturabilir veya seçkin Azure Machine Learning ortamlarından birini kullanabilirsiniz. Aşağıdaki YAML, çıkarım için Conda Dependencies bir dosya örneğidir. Modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerdiğinden, bir PIP >= 1.0.45 ile azureml-varsayılan değeri, bir PIP bağımlılığı olarak belirtmeniz gerektiğini unutmayın. Otomatik şema oluşturmayı kullanmak istiyorsanız, giriş betiğinizin de paketleri içeri aktarması gerekir `inference-schema` .

```YAML

name: project_environment
dependencies:
- python=3.6.2
- scikit-learn=0.22.1
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

Azure Machine Learning ile Python ortamlarını kullanma ve özelleştirme hakkında kapsamlı bir tartışma için, bkz. [Azure Machine Learning yazılım ortamları oluşturma & kullanma](how-to-use-environments.md).

Bir çıkarım yapılandırmasıyla özel bir Docker görüntüsü kullanma hakkında daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md).


Aşağıdaki örnek, çalışma alanınızdan bir ortamı yüklemeyi ve ardından çıkarım yapılandırmasıyla kullanmayı gösterir:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig


myenv = Environment.get(workspace=ws, name='myenv', version='1')
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                    environment=myenv)
```

Ortamlar hakkında daha fazla bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md).

Çıkarım yapılandırması hakkında daha fazla bilgi için bkz. [ınenceconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py&preserve-view=true) sınıfı belgeleri.

---

## <a name="choose-a-compute-target"></a>İşlem hedefi seçin

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

## <a name="define-a-deployment-configuration"></a>Dağıtım yapılandırması tanımlama

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

Bir dağıtım yapılandırması için kullanılabilen seçenekler, seçtiğiniz işlem hedefine bağlı olarak farklılık gösterir.

[!INCLUDE [aml-local-deploy-config](../../includes/machine-learning-service-local-deploy-config.md)]

Daha fazla bilgi için bkz. [az ml model Deploy](/cli/azure/ext/azure-cli-ml/ml/model#ext-azure-cli-ml-az-ml-model-deploy) belgeleri.

# <a name="python"></a>[Python](#tab/python)

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

---



## <a name="deploy-your-model"></a>Modelinizi dağıtın

Artık modelinizi dağıtmaya hazırsınız. 

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

### <a name="using-a-registered-model"></a>Kayıtlı model kullanma

Modelinize Azure Machine Learning çalışma alanınızda kaydolduysanız, "MyModel: 1" değerini modelinizin adı ve sürüm numarası ile değiştirin.

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
```

### <a name="using-a-local-model"></a>Yerel model kullanma

Modelinize kaydolmayı tercih ediyorsanız, modelinize sunulacak yerel bir dizin belirtmek için, üzerinde inferenceconfig.js"sourceDirectory" parametresini geçirebilirsiniz.

```azurecli-interactive
az ml model deploy --ic inferenceconfig.json --dc deploymentconfig.json
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnekte yerel bir dağıtım gösterilmektedir. Sözdizimi, önceki adımda seçtiğiniz işlem hedefine bağlı olarak değişir.

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Daha fazla bilgi için bkz. [Localwebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py&preserve-view=true), [model. deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)ve [WebService](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py&preserve-view=true)belgeleri.

---

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


### <a name="batch-inference"></a><a id="azuremlcompute"></a> Toplu çıkarım
Azure Machine Learning Işlem hedefleri Azure Machine Learning tarafından oluşturulur ve yönetilir. Bunlar, Azure Machine Learning işlem hatlarından toplu tahmin için kullanılabilirler.

Azure Machine Learning Işlem ile Batch çıkarımı hakkında yönergeler için bkz. [Batch öngörülerini çalıştırma](tutorial-pipeline-batch-scoring-classification.md).

### <a name="iot-edge-inference"></a><a id="iotedge"></a> IoT Edge çıkarımı
Kenara dağıtım desteği önizleme aşamasındadır. Daha fazla bilgi için bkz. [Azure Machine Learning IoT Edge modül olarak dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).

## <a name="delete-resources"></a>Kaynakları silme

# <a name="azure-cli"></a>[Azure CLI](#tab/azcli)

Dağıtılan bir Web hizmetini silmek için kullanın `az ml service <name of webservice>` .

Çalışma alanınızdan kayıtlı bir modeli silmek için şunu kullanın `az ml model delete <model id>`

[Bir Web hizmetini silme](/cli/azure/ext/azure-cli-ml/ml/service#ext-azure-cli-ml-az-ml-service-delete) ve [bir modeli silme](/cli/azure/ext/azure-cli-ml/ml/model#ext-azure-cli-ml-az-ml-model-delete)hakkında daha fazla bilgi edinin.

# <a name="python"></a>[Python](#tab/python)

Dağıtılmış bir Web hizmetini silmek için kullanın `service.delete()` .
Kayıtlı bir modeli silmek için kullanın `model.delete()` .

Daha fazla bilgi için bkz. [WebService. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#&preserve-view=truedelete--) ve [model. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#&preserve-view=truedelete--)belgeleri.

---


## <a name="next-steps"></a>Sonraki adımlar

* [Başarısız bir dağıtımda sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md)
* [Web hizmetini güncelleştirme](how-to-deploy-update-web-service.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [TLS kullanarak Azure Machine Learning aracılığıyla web hizmetinin güvenliğini sağlama](how-to-secure-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)

