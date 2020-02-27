---
title: Yeniden kullanılabilir ML ortamları oluşturma
titleSuffix: Azure Machine Learning
description: Model eğitimi ve dağıtımı için ortamları oluşturun ve yönetin. Bu ortam için Python paketlerini ve diğer ayarları yönetin.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: cb76c7d7804a7d39e8a18c7a4cf41e9b4e0a7593
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623647"
---
# <a name="reuse-environments-for-training-and-deployment-by-using-azure-machine-learning"></a>Azure Machine Learning kullanarak eğitim ve dağıtım için ortamları yeniden kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede Azure Machine Learning [ortamları](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)oluşturma ve yönetme hakkında bilgi edinin. Ortamların yazılım bağımlılıklarını geliştikçe izlemek ve yeniden oluşturmak için ortamları kullanın.

Yazılım bağımlılığı yönetimi, geliştiriciler için ortak bir görevdir. Derlemelerin kapsamlı el ile yazılım yapılandırması olmadan tekrarlanabilir olmasını sağlamak istiyorsunuz. Azure Machine Learning, PIP ve Conda gibi yerel geliştirme çözümleri için `Environment` sınıfı hesaplar ve hem yerel hem de Dağıtılmış bulut geliştirmesi için bir çözüm sağlar.

Bu makaledeki örneklerde nasıl yapılacağı gösterilmektedir:

* Bir ortam oluşturun ve paket bağımlılıklarını belirtin.
* Ortamları alın ve güncelleştirin.
* Eğitim için bir ortam kullanın.
* Web hizmeti dağıtımı için bir ortam kullanın.

Ortamların Azure Machine Learning nasıl çalıştığı hakkında üst düzey bir genel bakış için bkz. [ml ortamları nedir?](concept-environments.md).

## <a name="prerequisites"></a>Önkoşullar

* [Python için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md)

## <a name="create-an-environment"></a>Ortam oluşturma

Aşağıdaki bölümlerde, denemeleri için bir ortam oluşturabileceğiniz birçok yol araştırılamaz.

### <a name="use-a-curated-environment"></a>Seçkin bir ortam kullanma

Başlangıç ortamlarından birini seçerek şunları yapabilirsiniz: 

* _AzureML en düşük_ ortam, çalışma izlemeyi ve varlık yüklemeyi etkinleştirmek için en az bir paket kümesi içerir. Kendi ortamınız için bir başlangıç noktası olarak kullanabilirsiniz.

* _AzureML öğretici_ ortamı, ortak veri bilimi paketleri içerir. Bu paketler Scikit-öğren, Pandas, Matplotlib ve daha büyük bir azureml-SDK paketleri kümesi içerir.

Seçkin ortamlar, önbelleğe alınmış Docker görüntüleri tarafından desteklenir. Bu yedekleme, çalıştırma hazırlık maliyetini azaltır.

`Environment.get` yöntemi kullanarak, seçkin ortamların birini seçin:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Aşağıdaki kodu kullanarak, seçkin ortamları ve paketleri listeleyebilirsiniz:

```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  _AzureML_ ön eki ile kendi ortam adınızı başlatmayın. Bu ön ek, seçkin ortamlar için ayrılmıştır.

### <a name="instantiate-an-environment-object"></a>Ortam nesnesi örneği oluşturma

El ile bir ortam oluşturmak için SDK 'dan `Environment` sınıfını içeri aktarın. Ardından, bir ortam nesnesini başlatmak için aşağıdaki kodu kullanın.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="use-conda-and-pip-specification-files"></a>Conda ve PIP belirtim dosyalarını kullanma

Ayrıca, Conda belirtimi veya bir PIP gereksinimleri dosyasından bir ortam oluşturabilirsiniz. [`from_conda_specification()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) yöntemini veya [`from_pip_requirements()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) yöntemini kullanın. Yöntem bağımsız değişkeninde, ortam adınızı ve istediğiniz dosyanın dosya yolunu ekleyin.

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

# From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="use-existing-conda-environments"></a>Mevcut Conda ortamlarını kullanın

Yerel bilgisayarınızda var olan bir Conda ortamınız varsa, bu hizmeti kullanarak bir ortam nesnesi oluşturabilirsiniz. Bu stratejiyi kullanarak, uzak çalışmalardaki yerel etkileşimli ortamınızı yeniden kullanabilirsiniz.

Aşağıdaki kod, `mycondaenv`var olan Conda ortamından bir ortam nesnesi oluşturur. [`from_existing_conda_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-) yöntemini kullanır.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="create-environments-automatically"></a>Ortamları otomatik olarak oluşturma

Eğitim çalıştırması göndererek otomatik olarak bir ortam oluşturun. `submit()` yöntemini kullanarak çalıştırmayı gönderebilirsiniz. Bir eğitim çalıştırması gönderdiğinizde, yeni ortamın oluşturulması birkaç dakika sürebilir. Yapı süresi gerekli bağımlılıkların boyutuna bağlıdır. 

Çalıştırmayı göndermeden önce çalışma yapılandırmanızda bir ortam belirtmezseniz, sizin için varsayılan bir ortam oluşturulur.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attach training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Show each step of run 
run.wait_for_completion(show_output=True)
```

Benzer şekilde, eğitim için bir [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) nesnesi kullanıyorsanız, tahmin aracı örneğini bir ortam belirtmeden bir çalıştırma olarak doğrudan gönderebilirsiniz. `Estimator` nesnesi ortamı ve işlem hedefini zaten kapsüller.

## <a name="add-packages-to-an-environment"></a>Bir ortama paket ekleme

Conda, PIP veya özel tekerlek dosyalarını kullanarak bir ortama paket ekleyin. [`CondaDependency`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) sınıfını kullanarak her bir paket bağımlılığını belirtin. Ortamın `PythonSection`ekleyin.

### <a name="conda-and-pip-packages"></a>Conda ve PIP paketleri

Bir Conda Package deposunda bir paket varsa, PIP yüklemesi yerine Conda yüklemesini kullanmanızı öneririz. Conda paketleri genellikle yüklemeyi daha güvenilir hale getirmek için önceden oluşturulmuş ikililer ile gelir.

Aşağıdaki örnek ortama ekler. `scikit-learn`sürüm 0.21.3 ekler. Ayrıca, `myenv``pillow` paketini de ekler. Örnek, sırasıyla [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) yöntemini ve [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) yöntemini kullanır.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs scikit-learn version 0.21.3 conda package
conda_dep.add_conda_package("scikit-learn==0.21.3")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

### <a name="private-wheel-files"></a>Özel tekerlek dosyaları

Özel PI dosyalarını, önce çalışma alanı depolama alanınıza yükleyerek kullanabilirsiniz. Onları statik bir [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-) yöntemi kullanarak karşıya yüklersiniz. Ardından, depolama URL 'sini yakalayıp `add_pip_package()` metoduna URL 'YI geçirin.

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>Ortamları yönetme

Ortamları, işlem hedefleri genelinde ve çalışma alanının diğer kullanıcılarıyla güncelleştirmek, izlemek ve yeniden kullanmak için yönetin.

### <a name="register-environments"></a>Ortamları kaydetme

Bir Web hizmeti çalıştırdığınızda veya dağıtırken ortam, çalışma alanınıza otomatik olarak kaydedilir. Ayrıca, [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-) yöntemini kullanarak ortamı el ile kaydedebilirsiniz. Bu işlem, ortamı bulutta izlenen ve sürümlü bir varlığa yapar. Varlık, çalışma alanı kullanıcıları arasında paylaşılabilir.

Aşağıdaki kod `myenv` ortamını `ws` çalışma alanına kaydeder.

```python
myenv.register(workspace=ws)
```

Ortamı eğitim veya dağıtımda ilk kez kullandığınızda, çalışma alanına kaydedilir. Daha sonra işlem hedefine oluşturulup dağıtılır. Hizmet ortamları önbelleğe alır. Önbelleğe alınmış bir ortamı yeniden kullanmak yeni bir hizmet kullanmaktan veya güncelleştirilmiş bir hizmetin kullanılmasıyla çok daha az zaman alır.

### <a name="get-existing-environments"></a>Mevcut ortamları al

`Environment` sınıfı, çalışma alanınızdaki mevcut ortamları almanıza izin veren yöntemler sunar. Ortamları ada, liste olarak veya belirli bir eğitim çalıştırmasına göre alabilirsiniz. Bu bilgiler, sorun giderme, denetim ve reproducibility için faydalıdır.

#### <a name="view-a-list-of-environments"></a>Ortamların listesini görüntüleyin

[`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) sınıfını kullanarak çalışma alanınızdaki ortamları görüntüleyin. Sonra yeniden kullanmak için bir ortam seçin.

#### <a name="get-an-environment-by-name"></a>Ada göre ortam al

Ayrıca, belirli bir ortamı ada ve sürüme göre de alabilirsiniz. Aşağıdaki kod, `ws` çalışma alanındaki `myenv` ortamının sürüm `1` almak için [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) yöntemini kullanır.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="train-a-run-specific-environment"></a>Çalıştırmaya özgü bir ortamı eğitme

Eğitim bittikten sonra belirli bir çalıştırma için kullanılan ortamı almak için `Run` sınıfındaki [`get_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--) yöntemi kullanın.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Mevcut bir ortamı güncelleştirme

Var olan bir ortamı (örneğin, bir Python paketi ekleyerek) değiştirmenizi söyleyin. Daha sonra, bir çalıştırma gönderdiğinizde, bir model dağıttığınızda veya ortamı el ile kaydettiğinizde ortamın yeni bir sürümü oluşturulur. Sürüm oluşturma, ortamın değişikliklerini zamana göre görüntülemenize olanak sağlar.

Mevcut bir ortamda bir Python paketi sürümünü güncelleştirmek için, bu pakete ait sürüm numarasını belirtin. Tam sürüm numarasını kullanmazsanız Azure Machine Learning, özgün paket sürümleri ile mevcut ortamı yeniden kullanacaktır.

### <a name="debug-the-image-build"></a>Görüntü derlemesinde hata ayıklama

Aşağıdaki örnekte, bir ortamı Docker görüntüsü olarak el ile oluşturmak için [`build()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace-) yöntemi kullanılmaktadır. [`wait_for_completion()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-)kullanarak görüntü derlemeden çıkış günlüklerini izler. Sonra oluşturulan görüntü, çalışma alanının Azure Container Registry örneğinde görüntülenir. Bu bilgiler hata ayıklama için yararlıdır.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="enable-docker"></a>Docker 'ı etkinleştir

 Azure Machine Learning `Environment` sınıfının [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) , öğreticinizi çalıştırdığınız Konuk işletim sistemini en iyi şekilde özelleştirmenize ve denetlemenize olanak tanır.

Docker 'ı etkinleştirdiğinizde hizmet bir Docker görüntüsü oluşturur. Ayrıca, bu Docker kapsayıcısı içindeki belirtimlerinizi kullanan bir Python ortamı oluşturur. Bu işlevsellik, eğitim çalışmalarınız için ek yalıtım ve reproducibility sağlar.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Varsayılan olarak, yeni oluşturulan Docker görüntüsü, çalışma alanıyla ilişkili kapsayıcı kayıt defterinde görüntülenir.  Depo adı, *azureml veya azureml_\<uuıd\>* . Adın benzersiz tanımlayıcı (*UUID*) bölümü, ortam yapılandırmasından hesaplanan bir karmaya karşılık gelir. Bu yazışma, hizmetin, belirtilen ortam için bir görüntünün yeniden kullanım için zaten mevcut olup olmadığını belirlemesine izin verir.

Ayrıca, hizmet Ubuntu Linux tabanlı [temel görüntülerden](https://github.com/Azure/AzureML-Containers)birini otomatik olarak kullanır. Belirtilen Python paketlerini yüklüyor. Temel görüntüde CPU sürümleri ve GPU sürümleri bulunur. Azure Machine Learning, hangi sürümün kullanılacağını otomatik olarak algılar.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
# Alternatively, you can specify the contents of dockerfile of your base image
with open("docker_file_of_your_base_image", "r") as f:
    dockerfile_contents_of_your_base_image=f.read()
myenv.docker.base_dockerfile=dockerfile_contents_of_your_base_image 
```

> [!NOTE]
> Özel bir Docker görüntüsü kullanırken `environment.python.user_managed_dependencies=False` belirtirseniz, hizmet görüntü içinde bir Conda ortamı oluşturur. Bu işlem, temel görüntüde yüklü olan herhangi bir Python kitaplıklarını kullanmak yerine bu ortamdaki çalışmayı yürütür. Kendi yüklü paketlerinizi kullanmak için parametresini `True` olarak ayarlayın.

## <a name="use-environments-for-training"></a>Eğitim için ortamları kullanma

Bir eğitim çalıştırması göndermek için ortamınızı, [işlem hedefini](concept-compute-target.md)ve eğitim Python betiğinizi çalıştırma yapılandırması olarak birleştirmeniz gerekir. Bu yapılandırma, çalıştırmaları göndermek için kullanılan bir sarmalayıcı nesnesidir.

Bir eğitim çalıştırması gönderdiğinizde, yeni bir ortam oluşturmak birkaç dakika sürebilir. Süre, gerekli bağımlılıkların boyutuna bağlıdır. Ortamlar hizmet tarafından önbelleğe alınır. Bu nedenle, ortam tanımı değişmeden kaldığı sürece, tam kurulum zamanına yalnızca bir kez tabi olursunuz.

Aşağıdaki yerel betik çalıştırma örneği, sarmalayıcı nesneniz olarak [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) nerede kullanacağınızı gösterir.

```python
from azureml.core import ScriptRunConfig, Experiment
from azureml.core.environment import Environment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.run_config.target = "local"

# Attach environment to run config
runconfig.run_config.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> Çalıştırma geçmişini devre dışı bırakmak veya anlık görüntüleri çalıştırmak için `ScriptRunConfig.run_config.history`altındaki ayarı kullanın.

Çalışma yapılandırmanızda ortamı belirtmezseniz, hizmet, çalıştırmayı gönderdiğinizde varsayılan bir ortam oluşturur.

### <a name="use-an-estimator-for-training"></a>Eğitim için bir tahmin aracı kullanın

Eğitim için bir [tahmin aracı](how-to-train-ml-models.md) kullanıyorsanız, tahmin aracı örneğini doğrudan gönderebilirsiniz. Ortamı ve işlem hedefini zaten kapsüller.

Aşağıdaki kod, tek düğümlü eğitim çalışması için bir tahmin aracı kullanır. Bir `scikit-learn` modeli için uzak bir işlem üzerinde çalışır. Daha önce bir işlem hedefi nesnesi, `compute_target`ve `ds`bir veri deposu nesnesi oluşturduğunuzu varsayar.

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])

# Submit the run 
run = experiment.submit(sk_est)
```

## <a name="use-environments-for-web-service-deployment"></a>Web hizmeti dağıtımı için ortamları kullanma

Modelinizi bir Web hizmeti olarak dağıtırken ortamları kullanabilirsiniz. Bu özellik, tekrarlanabilir, bağlantılı bir iş akışı sunar. Bu iş akışında, hem eğitim hesaplamanıza hem de çıkarım işlem ortamınızda aynı kitaplıkları kullanarak modelinizi eğitebilirsiniz, test edebilir ve dağıtabilirsiniz.

Bir Web hizmeti dağıtmak için ortam, çıkarım işlem, Puanlama betiği ve dağıtım nesneniz [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)kayıtlı modeli birleştirin. Daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

Bu örnekte, bir eğitim çalıştırmasını tamamladığınızı varsayalım. Artık bu modeli Azure Container Instances dağıtmak istiyorsunuz. Web hizmeti oluşturduğunuzda, model ve Puanlama dosyaları görüntüye bağlanır ve Azure Machine Learning çıkarım yığını görüntüye eklenir.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference, & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="example-notebooks"></a>Örnek Not Defterleri

Bu [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) , bu makalede gösterilen kavramların ve yöntemlerin üzerine genişletilir.

[Özel bir Docker temel görüntüsünü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md) özel bir Docker temel görüntüsü kullanarak bir modelin nasıl dağıtılacağını gösterir.

Bu [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/spark) , bir Spark modelinin Web hizmeti olarak nasıl dağıtılacağını göstermektedir.

## <a name="create-and-manage-environments-with-the-cli"></a>CLı ile ortam oluşturma ve yönetme

[Azure MACHINE LEARNING CLI](reference-azure-machine-learning-cli.md) , Python SDK işlevlerinin çoğunu yansıtır. Ortamları oluşturmak ve yönetmek için bu uygulamayı kullanabilirsiniz. Bu bölümde tartıştığımız komutlar temel işlevselliği göstermektedir.

Aşağıdaki komut, belirtilen dizinde varsayılan bir ortam tanımı için dosyaları tanılar. Bu dosyalar JSON dosyalarıdır. SDK 'da karşılık gelen sınıf gibi çalışır. Dosyaları, özel ayarları olan yeni ortamlar oluşturmak için kullanabilirsiniz. 

```azurecli-interactive
az ml environment scaffold -n myenv -d myenvdir
```

Belirtilen dizinden bir ortamı kaydetmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az ml environment register -d myenvdir
```

Tüm kayıtlı ortamları listelemek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az ml environment list
```

Aşağıdaki komutu kullanarak kayıtlı bir ortamı indirin.

```azurecli-interactive
az ml environment download -n myenv -d downloaddir
```

## <a name="next-steps"></a>Sonraki adımlar

* Bir modeli eğitemek üzere yönetilen bir işlem hedefi kullanmak için bkz. [öğretici: model eğitme](tutorial-train-models-with-aml.md).
* Eğitilen bir modelden sonra [modellerin nasıl ve nereye dağıtılacağını](how-to-deploy-and-where.md)öğrenin.
* [`Environment` sınıfı SDK başvurusunu](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)görüntüleyin.
* Bu makalede açıklanan kavramlar ve yöntemler hakkında daha fazla bilgi için bkz. [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments).
