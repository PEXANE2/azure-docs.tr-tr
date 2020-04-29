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
ms.date: 03/18/2020
ms.openlocfilehash: dc30318027962247f7504b734385d7642550ec87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81536361"
---
# <a name="reuse-environments-for-training-and-deployment-by-using-azure-machine-learning"></a>Azure Machine Learning kullanarak eğitim ve dağıtım için ortamları yeniden kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede Azure Machine Learning [ortamları](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)oluşturma ve yönetme hakkında bilgi edinin. Ortamların yazılım bağımlılıklarını geliştikçe izlemek ve yeniden oluşturmak için ortamları kullanın.

Yazılım bağımlılığı yönetimi, geliştiriciler için ortak bir görevdir. Derlemelerin kapsamlı el ile yazılım yapılandırması olmadan tekrarlanabilir olmasını sağlamak istiyorsunuz. , PIP ve Conda gibi yerel geliştirme çözümleri için Azure Machine Learning `Environment` sınıfı hesaplar ve hem yerel hem de Dağıtılmış bulut geliştirmesi için bir çözüm sağlar.

Bu makaledeki örneklerde nasıl yapılacağı gösterilmektedir:

* Bir ortam oluşturun ve paket bağımlılıklarını belirtin.
* Ortamları alın ve güncelleştirin.
* Eğitim için bir ortam kullanın.
* Web hizmeti dağıtımı için bir ortam kullanın.

Ortamların Azure Machine Learning nasıl çalıştığı hakkında üst düzey bir genel bakış için bkz. [ml ortamları nedir?](concept-environments.md).

## <a name="prerequisites"></a>Ön koşullar

* [Python için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md)

## <a name="create-an-environment"></a>Ortam oluşturma

Aşağıdaki bölümlerde, denemeleri için bir ortam oluşturabileceğiniz birçok yol araştırılamaz.

### <a name="use-a-curated-environment"></a>Seçkin bir ortam kullanma

Başlangıç ortamlarından birini seçerek şunları yapabilirsiniz: 

* _AzureML en düşük_ ortam, çalışma izlemeyi ve varlık yüklemeyi etkinleştirmek için en az bir paket kümesi içerir. Kendi ortamınız için bir başlangıç noktası olarak kullanabilirsiniz.

* _AzureML öğretici_ ortamı, ortak veri bilimi paketleri içerir. Bu paketler Scikit-öğren, Pandas, Matplotlib ve daha büyük bir azureml-SDK paketleri kümesi içerir.

Seçkin ortamlar, önbelleğe alınmış Docker görüntüleri tarafından desteklenir. Bu yedekleme, çalıştırma hazırlık maliyetini azaltır.

Bu yöntemi `Environment.get` kullanarak, seçkin ortamların birini seçin:

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

Bir ortamı el ile oluşturmak için SDK 'dan `Environment` sınıfını içeri aktarın. Ardından, bir ortam nesnesini başlatmak için aşağıdaki kodu kullanın.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="use-conda-and-pip-specification-files"></a>Conda ve PIP belirtim dosyalarını kullanma

Ayrıca, Conda belirtimi veya bir PIP gereksinimleri dosyasından bir ortam oluşturabilirsiniz. [`from_conda_specification()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) Yöntemini veya [`from_pip_requirements()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) yöntemini kullanın. Yöntem bağımsız değişkeninde, ortam adınızı ve istediğiniz dosyanın dosya yolunu ekleyin.

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

Aşağıdaki kod, mevcut Conda ortamından `mycondaenv`bir ortam nesnesi oluşturur. [`from_existing_conda_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-) Yöntemini kullanır.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="create-environments-automatically"></a>Ortamları otomatik olarak oluşturma

Eğitim çalıştırması göndererek otomatik olarak bir ortam oluşturun. `submit()` Yöntemini kullanarak çalıştırmayı gönderebilirsiniz. Bir eğitim çalıştırması gönderdiğinizde, yeni ortamın oluşturulması birkaç dakika sürebilir. Yapı süresi gerekli bağımlılıkların boyutuna bağlıdır. 

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

Benzer şekilde, eğitim için bir [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) nesnesi kullanıyorsanız, tahmin aracı örneğini bir ortam belirtmeden bir çalıştırma olarak doğrudan gönderebilirsiniz. `Estimator` Nesne, ortamı ve işlem hedefini zaten kapsüller.

## <a name="add-packages-to-an-environment"></a>Bir ortama paket ekleme

Conda, PIP veya özel tekerlek dosyalarını kullanarak bir ortama paket ekleyin. [`CondaDependency`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) Sınıfını kullanarak her bir paket bağımlılığını belirtin. Ortama ekleyin `PythonSection`.

### <a name="conda-and-pip-packages"></a>Conda ve PIP paketleri

Bir Conda Package deposunda bir paket varsa, PIP yüklemesi yerine Conda yüklemesini kullanmanızı öneririz. Conda paketleri genellikle yüklemeyi daha güvenilir hale getirmek için önceden oluşturulmuş ikililer ile gelir.

Aşağıdaki örnek ortama ekler. Sürüm 1.17.0 ekler `numpy`. Ayrıca, `pillow` `myenv`paketini de ekler. Örnek, sırasıyla [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) yöntemini ve [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) yöntemini kullanır.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs numpy version 1.17.0 conda package
conda_dep.add_conda_package("numpy==1.17.0")

# Installs pillow package
conda_dep.add_pip_package("pillow")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

>[!IMPORTANT]
> Başka bir çalıştırma için aynı ortam tanımını kullanıyorsanız, Azure Machine Learning hizmeti ortamınızın önbelleğe alınmış görüntüsünü yeniden kullanır. Örneğin ```numpy```, ayrılmış paket bağımlılığı olan bir ortam oluşturursanız, bu ortam, _ortam oluşturma sırasında_yüklenen paket sürümünü kullanmaya devam edecektir. Ayrıca, eşleşen tanımı olan gelecekteki tüm ortamlar eski sürümü kullanmaya devam eder. Daha fazla bilgi için bkz. [ortam oluşturma, önbelleğe alma ve yeniden kullanma](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse).

### <a name="private-wheel-files"></a>Özel tekerlek dosyaları

Özel PI dosyalarını, önce çalışma alanı depolama alanınıza yükleyerek kullanabilirsiniz. Onları statik [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-) bir yöntem kullanarak karşıya yüklersiniz. Ardından, depolama URL 'sini yakalar ve URL 'YI `add_pip_package()` yöntemine geçitirsiniz.

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>Ortam yönetme

Ortamları, işlem hedefleri genelinde ve çalışma alanının diğer kullanıcılarıyla güncelleştirmek, izlemek ve yeniden kullanmak için yönetin.

### <a name="register-environments"></a>Ortamları kaydetme

Bir Web hizmeti çalıştırdığınızda veya dağıtırken ortam, çalışma alanınıza otomatik olarak kaydedilir. Ayrıca, [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-) yöntemini kullanarak ortamı el ile de kaydedebilirsiniz. Bu işlem, ortamı bulutta izlenen ve sürümlü bir varlığa yapar. Varlık, çalışma alanı kullanıcıları arasında paylaşılabilir.

Aşağıdaki kod `myenv` ortamı `ws` çalışma alanına kaydeder.

```python
myenv.register(workspace=ws)
```

Ortamı eğitim veya dağıtımda ilk kez kullandığınızda, çalışma alanına kaydedilir. Daha sonra işlem hedefine oluşturulup dağıtılır. Hizmet ortamları önbelleğe alır. Önbelleğe alınmış bir ortamı yeniden kullanmak yeni bir hizmet kullanmaktan veya güncelleştirilmiş bir hizmetin kullanılmasıyla çok daha az zaman alır.

### <a name="get-existing-environments"></a>Mevcut ortamları al

Sınıfı `Environment` , çalışma alanınızdaki mevcut ortamları almanıza izin veren yöntemler sunar. Ortamları ada, liste olarak veya belirli bir eğitim çalıştırmasına göre alabilirsiniz. Bu bilgiler, sorun giderme, denetim ve reproducibility için faydalıdır.

#### <a name="view-a-list-of-environments"></a>Ortamların listesini görüntüleyin

[`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) Sınıfını kullanarak çalışma alanınızdaki ortamları görüntüleyin. Sonra yeniden kullanmak için bir ortam seçin.

#### <a name="get-an-environment-by-name"></a>Ada göre ortam al

Ayrıca, belirli bir ortamı ada ve sürüme göre de alabilirsiniz. Aşağıdaki [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) kod, `1` `myenv` `ws` çalışma alanındaki ortamın sürümünü almak için yöntemini kullanır.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="train-a-run-specific-environment"></a>Çalıştırmaya özgü bir ortamı eğitme

Eğitim bittikten sonra belirli bir çalıştırma için kullanılan ortamı almak için, [`get_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--) `Run` sınıfındaki yöntemini kullanın.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Mevcut bir ortamı güncelleştirme

Var olan bir ortamı (örneğin, bir Python paketi ekleyerek) değiştirmenizi söyleyin. Daha sonra, bir çalıştırma gönderdiğinizde, bir model dağıttığınızda veya ortamı el ile kaydettiğinizde ortamın yeni bir sürümü oluşturulur. Sürüm oluşturma, ortamın değişikliklerini zamana göre görüntülemenize olanak sağlar.

Mevcut bir ortamda bir Python paketi sürümünü güncelleştirmek için, bu pakete ait sürüm numarasını belirtin. Tam sürüm numarasını kullanmazsanız Azure Machine Learning, özgün paket sürümleri ile mevcut ortamı yeniden kullanacaktır.

### <a name="debug-the-image-build"></a>Görüntü derlemesinde hata ayıklama

Aşağıdaki örnek, bir ortamı [`build()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace--image-build-compute-none-) Docker görüntüsü olarak el ile oluşturmak için yöntemini kullanır. Kullanarak [`wait_for_completion()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-)görüntü derlemeden çıkış günlüklerini izler. Sonra oluşturulan görüntü, çalışma alanının Azure Container Registry örneğinde görüntülenir. Bu bilgiler hata ayıklama için yararlıdır.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="enable-docker"></a>Docker 'ı etkinleştir

 Azure Machine Learning [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) `Environment` sınıfı, öğreticinizi çalıştırdığınız Konuk işletim sistemini en iyi şekilde özelleştirmenize ve denetlemenize olanak tanır.

Docker 'ı etkinleştirdiğinizde hizmet bir Docker görüntüsü oluşturur. Ayrıca, bu Docker kapsayıcısı içindeki belirtimlerinizi kullanan bir Python ortamı oluşturur. Bu işlevsellik, eğitim çalışmalarınız için ek yalıtım ve reproducibility sağlar.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Varsayılan olarak, yeni oluşturulan Docker görüntüsü, çalışma alanıyla ilişkili kapsayıcı kayıt defterinde görüntülenir.  Depo adının, *azureml/azureml_\<\>UUID*biçimi vardır. Adın benzersiz tanımlayıcı (*UUID*) bölümü, ortam yapılandırmasından hesaplanan bir karmaya karşılık gelir. Bu yazışma, hizmetin, belirtilen ortam için bir görüntünün yeniden kullanım için zaten mevcut olup olmadığını belirlemesine izin verir.

Ayrıca, hizmet Ubuntu Linux tabanlı [temel görüntülerden](https://github.com/Azure/AzureML-Containers)birini otomatik olarak kullanır. Belirtilen Python paketlerini yüklüyor. Temel görüntüde CPU sürümleri ve GPU sürümleri bulunur. Azure Machine Learning, hangi sürümün kullanılacağını otomatik olarak algılar.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
```

Ayrıca, özel bir Dockerfile belirtebilirsiniz. Docker ```FROM``` komutunu kullanarak Azure Machine Learning temel görüntülerden birinden başlamak ve kendi özel adımlarınızı eklemek en iyisidir. Python olmayan paketleri bağımlılık olarak yüklemeniz gerekiyorsa bu yaklaşımı kullanın.

```python
# Specify docker steps as a string. Alternatively, load the string from a file.
dockerfile = r"""
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN echo "Hello from custom container!"
"""

# Set base image to None, because the image is defined by dockerfile.
myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile
```

### <a name="use-user-managed-dependencies"></a>Kullanıcı tarafından yönetilen bağımlılıkları kullan

Bazı durumlarda, özel taban görüntünüz kullanmak istediğiniz paketlere sahip bir Python ortamı zaten içerebilir.

Varsayılan olarak, Azure Machine Learning hizmet belirttiğiniz bağımlılıklarla bir Conda ortamı oluşturur ve temel görüntüde yüklü olan herhangi bir Python kitaplıklarını kullanmak yerine bu ortamda çalışmayı yürütür. 

Yüklü paketlerinizi kullanmak için parametresini `Environment.python.user_managed_dependencies = True`ayarlayın. Temel görüntünün bir Python yorumlayıcı içerdiğinden ve eğitim betiğinizin gereksinimlerine yönelik paketlere sahip olduğundan emin olun.

Örneğin, 2. tuş paketinin yüklü olduğu bir temel Miniconda ortamında çalıştırmak için, önce paketi yüklemek için bir adımla birlikte bir Dockerfile belirtin. Ardından Kullanıcı tarafından yönetilen bağımlılıkları olarak `True`ayarlayın. 

Ayrıca, `Environment.python.interpreter_path` değişkenini ayarlayarak görüntünün içindeki belirli bir Python Yorumlayıcısına bir yol belirtebilirsiniz.

```python
dockerfile = """
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN conda install numpy
"""

myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile
myenv.python.user_managed_dependencies=True
myenv.python.interpreter_path = "/opt/miniconda/bin/python"
```

## <a name="use-environments-for-training"></a>Eğitim için ortamları kullanma

Bir eğitim çalıştırması göndermek için ortamınızı, [işlem hedefini](concept-compute-target.md)ve eğitim Python betiğinizi çalıştırma yapılandırması olarak birleştirmeniz gerekir. Bu yapılandırma, çalıştırmaları göndermek için kullanılan bir sarmalayıcı nesnesidir.

Bir eğitim çalıştırması gönderdiğinizde, yeni bir ortam oluşturmak birkaç dakika sürebilir. Süre, gerekli bağımlılıkların boyutuna bağlıdır. Ortamlar hizmet tarafından önbelleğe alınır. Bu nedenle, ortam tanımı değişmeden kaldığı sürece, tam kurulum zamanına yalnızca bir kez tabi olursunuz.

Aşağıdaki yerel betik çalıştırma örneği, sarmalayıcı nesneniz olarak kullanacağınız [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) yeri gösterir.

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
> Çalıştırma geçmişini devre dışı bırakmak veya anlık görüntüleri çalıştırmak için altındaki `ScriptRunConfig.run_config.history`ayarı kullanın.

Çalışma yapılandırmanızda ortamı belirtmezseniz, hizmet, çalıştırmayı gönderdiğinizde varsayılan bir ortam oluşturur.

### <a name="use-an-estimator-for-training"></a>Eğitim için bir tahmin aracı kullanın

Eğitim için bir [tahmin aracı](how-to-train-ml-models.md) kullanıyorsanız, tahmin aracı örneğini doğrudan gönderebilirsiniz. Ortamı ve işlem hedefini zaten kapsüller.

Aşağıdaki kod, tek düğümlü eğitim çalışması için bir tahmin aracı kullanır. Bir `scikit-learn` model için uzak bir işlem üzerinde çalışır. Daha önce bir işlem hedefi nesnesi, `compute_target`ve bir veri deposu nesnesi oluşturduğunuzu varsayar. `ds`

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

Bir Web hizmeti dağıtmak için ortam, çıkarım işlem, Puanlama betiği ve dağıtım nesnenizin kayıtlı modelini birleştirin [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-). Daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

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

## <a name="example-notebooks"></a>Örnek Not defterleri

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
* Sınıf SDK başvurusunu görüntüleyin. [ `Environment` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)
* Bu makalede açıklanan kavramlar ve yöntemler hakkında daha fazla bilgi için bkz. [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments).
