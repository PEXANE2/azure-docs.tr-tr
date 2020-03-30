---
title: Yeniden kullanılabilir ML ortamları oluşturun
titleSuffix: Azure Machine Learning
description: Model eğitimi ve dağıtımı için ortamlar oluşturun ve yönetin. Python paketlerini ve ortam la ilgili diğer ayarları yönetin.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 3b1fc5dc427a8a9a1987b0ef916b99edb25e292a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063975"
---
# <a name="reuse-environments-for-training-and-deployment-by-using-azure-machine-learning"></a>Azure Machine Learning'i kullanarak eğitim ve dağıtım ortamlarını yeniden kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning [ortamlarını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)nasıl oluşturup yöneteceklerini öğrenin. Projelerinizin yazılım bağımlılıklarını izlemek ve çoğaltmak için ortamları kullanın.

Yazılım bağımlılık yönetimi geliştiriciler için ortak bir görevdir. Kapsamlı el ile yazılım yapılandırması olmadan yapıların yeniden çoğaltılabilir olduğundan emin olmak istiyorsunuz. Azure Machine `Environment` Learning sınıfı pip ve Conda gibi yerel geliştirme çözümlerini hesaplar ve hem yerel hem de dağıtılmış bulut geliştirme için bir çözüm sağlar.

Bu makaledeki örnekler nasıl gösterin:

* Bir ortam oluşturun ve paket bağımlılıklarını belirtin.
* Ortamları alın ve güncelleyin.
* Eğitim için bir ortam kullanın.
* Web hizmeti dağıtımı için bir ortam kullanın.

Azure Machine Learning'de ortamların nasıl çalıştığına üst düzey bir genel bakış için ML [ortamları nelerdir?](concept-environments.md)

## <a name="prerequisites"></a>Ön koşullar

* [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Makine Öğrenimi çalışma alanı](how-to-manage-workspace.md)

## <a name="create-an-environment"></a>Ortam oluşturma

Aşağıdaki bölümler, denemeleriniz için bir ortam oluşturmanın birden çok yolunu inceler.

### <a name="use-a-curated-environment"></a>Küratörlü bir ortam kullanma

Başlamak için küratörlük ortamlarından birini seçebilirsiniz: 

* _AzureML-Minimal_ ortamı, çalıştırma izleme ve varlık yüklemeyi etkinleştirmek için en az paket kümesi içerir. Kendi ortamınız için bir başlangıç noktası olarak kullanabilirsiniz.

* _AzureML-Tutorial_ ortamı ortak veri bilimi paketleri içerir. Bu paketler Scikit-Learn, Pandalar, Matplotlib ve daha büyük bir azureml-sdk paketi kümesini içerir.

Küratörlüğünü yaptığı ortamlar önbelleğe alınmış Docker görüntüleriyle desteklenir. Bu destek, çalıştırma hazırlama maliyetini azaltır.

Küratörlüğünü `Environment.get` yaptığı ortamlardan birini seçmek için yöntemi kullanın:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Küratörlüğünü yaptığı ortamları ve paketlerini aşağıdaki kodu kullanarak listeleyebilirsiniz:

```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  _AzureML_ önekiyle kendi ortam adınızı başlatmayın. Bu önek, küratörlü ortamlar için ayrılmıştır.

### <a name="instantiate-an-environment-object"></a>Ortam nesnesi anında

Bir ortamı el ile oluşturmak `Environment` için sınıfı SDK'dan içeri aktarın. Ardından, bir ortam nesnesini anında kullanmak için aşağıdaki kodu kullanın.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="use-conda-and-pip-specification-files"></a>Conda ve pip belirtim dosyalarını kullanma

Ayrıca, Conda belirtimi veya pip gereksinimleri dosyasından bir ortam oluşturabilirsiniz. [`from_conda_specification()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) Yöntemi veya [`from_pip_requirements()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) yöntemi kullanın. Yöntem bağımsız değişkeninde, ortam adınızı ve istediğiniz dosyanın dosya yolunu ekleyin.

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

# From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="use-existing-conda-environments"></a>Mevcut Conda ortamlarını kullanma

Yerel bilgisayarınızda varolan bir Conda ortamı varsa, bir ortam nesnesi oluşturmak için hizmeti kullanabilirsiniz. Bu stratejiyi kullanarak, yerel etkileşimli ortamınızı uzaktan çalıştırmalarda yeniden kullanabilirsiniz.

Aşağıdaki kod, varolan Conda ortamından `mycondaenv`bir ortam nesnesi oluşturur. Bu [`from_existing_conda_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-) yöntemi kullanır.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="create-environments-automatically"></a>Ortamları otomatik olarak oluşturma

Bir eğitim çalışması göndererek otomatik olarak bir ortam oluşturun. `submit()` Yöntemi kullanarak çalıştırın. Bir eğitim çalışması gönderdiğinizde, yeni ortamın oluşturulması birkaç dakika sürebilir. Yapı süresi, gerekli bağımlılıkların boyutuna bağlıdır. 

Çalıştırmayı göndermeden önce çalıştırılayapılandırmanızda bir ortam belirtmezseniz, sizin için varsayılan bir ortam oluşturulur.

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

Benzer şekilde, bir [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) nesneyi eğitim için kullanırsanız, tahminci örneğini bir ortam belirtmeden doğrudan çalıştırılabilirsiniz. Nesne `Estimator` zaten ortamı ve bilgi işlem hedefini kapsüller.

## <a name="add-packages-to-an-environment"></a>Ortama paket ekleme

Conda, pip veya özel tekerlek dosyalarını kullanarak ortama paket ekleyin. Sınıfı kullanarak her paket [`CondaDependency`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) bağımlılığını belirtin. Çevrenin `PythonSection`.

### <a name="conda-and-pip-packages"></a>Conda ve pip paketleri

Bir paket Conda paket deposunda mevcutsa, pip yüklemesi yerine Conda yüklemesini kullanmanızı öneririz. Conda paketleri genellikle yüklemeyi daha güvenilir hale getiren önceden oluşturulmuş ikililerle birlikte gelir.

Aşağıdaki örnek ortama ekler. Bu sürüm 0.21.3 `scikit-learn`ekler. Ayrıca `pillow` paketi `myenv`ekler. Örnek, sırasıyla [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) yöntemi [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) ve yöntemi kullanır.

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

>[!IMPORTANT]
> Başka bir çalışma için aynı ortam tanımını kullanıyorsanız, Azure Machine Learning hizmeti ortamınızın önbelleğe alınmış görüntüsünü yeniden kullanır. Sabitlenmemiş bir paket bağımlılığı olan bir ortam oluşturursanız, örneğin, ```numpy```bu _ortam, ortam oluşturma sırasında_yüklü paket sürümünü kullanmaya devam edecektir. Ayrıca, eşleşen tanımı ile herhangi bir gelecekteki ortam eski sürümü kullanmaya devam edecektir. Daha fazla bilgi için [Bkz. Çevre oluşturma, önbelleğe alma ve yeniden kullanma.](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse)

### <a name="private-wheel-files"></a>Özel tekerlek dosyaları

Özel pip tekerlek dosyalarını önce çalışma alanı depolama alanınıza yükleyerek kullanabilirsiniz. Statik [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-) bir yöntem kullanarak yüklersiniz. Ardından depolama URL'sini yakalar ve `add_pip_package()` URL'yi yönteme geçirirsiniz.

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>Ortam yönetme

Ortamları, bilgi işlem hedefleri arasında ve çalışma alanının diğer kullanıcılarıyla birlikte güncelleştirebilmeniz, izleyebilmeniz ve yeniden kullanabilmeniz için yönetin.

### <a name="register-environments"></a>Kayıt ortamları

Bir çalışma gönderdiğinizde veya bir web hizmeti dağıttığınızda ortam otomatik olarak çalışma alanınıza kaydedilir. Ayrıca [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-) yöntemi kullanarak ortamı el ile kaydedebilirsiniz. Bu işlem, ortamı bulutta izlenen ve sürümlenmiş bir varlığa dönüştürür. Varlık çalışma alanı kullanıcıları arasında paylaşılabilir.

Aşağıdaki kod `myenv` ortamı `ws` çalışma alanına kaydeder.

```python
myenv.register(workspace=ws)
```

Ortamı eğitim veya dağıtımda ilk kez kullandığınızda, çalışma alanına kaydedilir. Sonra bilgisayar hedefine kurulur ve dağıtılır. Hizmet ortamları önbelleğe alır. Önbelleğe alınmış bir ortamı yeniden kullanmak, yeni bir hizmeti veya güncelleştirilmiş bir hizmeti kullanmaktan çok daha az zaman alır.

### <a name="get-existing-environments"></a>Mevcut ortamları edin

Sınıf, `Environment` çalışma alanınızdaki varolan ortamları almanızı sağlayan yöntemler sunar. Ortamları ada göre, liste olarak veya belirli bir eğitim çalışmasıyla ada göre alabilirsiniz. Bu bilgiler sorun giderme, denetleme ve tekrarlanabilirlik için yararlıdır.

#### <a name="view-a-list-of-environments"></a>Ortamlar listesini görüntüleme

Sınıfı kullanarak çalışma alanınızdaki ortamları görüntüleyin. [`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) Ardından yeniden kullanmak için bir ortam seçin.

#### <a name="get-an-environment-by-name"></a>Ada göre bir ortam elde edin

Ayrıca, ada ve sürüme göre belirli bir ortam da alabilirsiniz. Aşağıdaki [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) `ws` kod, çalışma alanında `1` ortamın `myenv` sürümünü almak için yöntemi kullanır.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="train-a-run-specific-environment"></a>Çalıştırmaya özel bir ortam eğitin

Eğitim bittikten sonra belirli bir çalışma için kullanılan ortamı [`get_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--) elde etmek `Run` için sınıftaki yöntemi kullanın.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Varolan bir ortamı güncelleştirme

Örneğin, bir Python paketi ekleyerek varolan bir ortamı değiştirdiğinizi varsan. Daha sonra bir çalışma gönderdiğinde, bir model dağıttığınızda veya ortamı el ile kaydettirdiğinizde ortamın yeni bir sürümü oluşturulur. Sürüm, zaman içinde ortamın değişikliklerini görüntülemenize olanak tanır.

Varolan bir ortamda Python paket sürümünü güncelleştirmek için, bu paketin sürüm numarasını belirtin. Tam sürüm numarasını kullanmazsanız, Azure Machine Learning varolan ortamı özgün paket sürümleriyle yeniden kullanır.

### <a name="debug-the-image-build"></a>Görüntü oluşturma hata ayıklama

Aşağıdaki örnek, [`build()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace--image-build-compute-none-) docker görüntüsü olarak bir ortamı el ile oluşturmak için yöntemi kullanır. Bu kullanarak [`wait_for_completion()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-)görüntü oluşturmak çıkış günlükleri izler. Yerleşik görüntü daha sonra çalışma alanının Azure Kapsayıcı Kayıt Defteri örneğinde görünür. Bu bilgiler hata ayıklama için yararlıdır.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="enable-docker"></a>Docker'ı etkinleştir

 Azure [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) Machine Learning `Environment` sınıfı, eğitiminizi çalıştırdığınız konuk işletim sistemini hassas bir şekilde özelleştirmenize ve denetlemenize olanak tanır.

Docker'ı etkinleştirdiğinizde, hizmet bir Docker görüntüsü oluşturur. Ayrıca, söz lerinizi docker kapsayıcısı içinde kullanan bir Python ortamı oluşturur. Bu işlevsellik, eğitim çalıştırmalarınız için ek yalıtım ve tekrarlanabilirlik sağlar.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Varsayılan olarak, yeni oluşturulan Docker görüntüsü çalışma alanıyla ilişkili kapsayıcı kayıt defterinde görünür.  Depo adı *azureml/azureml_\<\>uuid*formuna sahiptir. Adın benzersiz tanımlayıcısı *(uuid)* bölümü, ortam yapılandırmasından hesaplanan bir karmaya karşılık gelir. Bu yazışma, hizmetin verilen ortamiçin bir görüntünün yeniden kullanım için zaten var olup olmadığını belirlemesine olanak tanır.

Ayrıca, hizmet otomatik olarak Ubuntu Linux tabanlı [temel görüntülerden](https://github.com/Azure/AzureML-Containers)birini kullanır. Belirtilen Python paketlerini yükler. Temel görüntücpu sürümleri ve GPU sürümleri vardır. Azure Machine Learning hangi sürümün kullanılacağını otomatik olarak algılar.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
```

Ayrıca özel bir Dockerfile belirtebilirsiniz. Docker ```FROM``` komutunu kullanarak Azure Machine Learning temel görüntülerinden birinden başlayıp kendi özel adımlarınızı eklemek çok kolay. Python olmayan paketleri bağımlılık olarak yüklemeniz gerekiyorsa bu yaklaşımı kullanın.

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

### <a name="use-user-managed-dependencies"></a>Kullanıcı tarafından yönetilen bağımlılıkları kullanma

Bazı durumlarda, özel temel görüntünüz zaten kullanmak istediğiniz paketleriçeren bir Python ortamı içerebilir.

Varsayılan olarak, Azure Machine Learning hizmeti belirttiğiniz bağımlılıklarla bir Conda ortamı oluşturur ve temel görüntüye yüklediğiniz Python kitaplıklarını kullanmak yerine bu ortamda çalıştırmayı yürütecektir. 

Kendi yüklü paketlerinizi kullanmak için parametreyi `Environment.python.user_managed_dependencies = True`ayarlayın. Temel görüntünün bir Python yorumlayıcısı içerdiğinden ve eğitim komut dosyanızın ihtiyaç duyduğu paketlere sahip olduğundan emin olun.

Örneğin, NumPy paketinin yüklü olduğu bir temel Miniconda ortamında çalışmak için, önce paketi yüklemek için bir adım içeren bir Dockerfile belirtin. Ardından kullanıcı tarafından yönetilen bağımlılıkları `True`. 

Değişkeni ayarlayarak görüntü içinde belirli bir Python yorumlayıcısına `Environment.python.interpreter_path` giden bir yol da belirtebilirsiniz.

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

Bir eğitim çalışması göndermek için ortamınızı, [bilgi işlem hedefinizi](concept-compute-target.md)ve eğitim Python komut dosyanızı bir çalıştırma yapılandırması nda birleştirmeniz gerekir. Bu yapılandırma, çalıştırmaları göndermek için kullanılan bir sarmalayıcı nesnesidir.

Bir eğitim çalışması gönderdiğinizde, yeni bir ortam oluşturulması birkaç dakika sürebilir. Süre, gerekli bağımlılıkların boyutuna bağlıdır. Ortamlar hizmet tarafından önbelleğe alınır. Ortam tanımı değişmeden kaldığı sürece, tam kurulum süresini yalnızca bir kez alabilirsiniz.

Aşağıdaki yerel komut dosyası çalıştırma örneği, sarıcı nesneniz olarak nerede kullanacağınızı [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) gösterir.

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
> Çalışma geçmişini devre dışı kılabilir veya anlık görüntüleri `ScriptRunConfig.run_config.history`çalıştırmak için aşağıdaki ayarı kullanın.

Çalıştırıladığınız yapılandırmanızda ortamı belirtmezseniz, çalışmanızı gönderdiğinizde hizmet varsayılan bir ortam oluşturur.

### <a name="use-an-estimator-for-training"></a>Eğitim için bir tahminci kullanın

Eğitim için bir [tahminci](how-to-train-ml-models.md) kullanıyorsanız, tahminci örneğini doğrudan gönderebilirsiniz. Zaten çevreyi ve hesaplama hedefini kapsüller.

Aşağıdaki kod, tek düğümlü bir eğitim çalışması için bir tahminci kullanır. Bir `scikit-learn` model için uzaktan işlemle çalışır. Daha önce bir bilgi işlem hedef nesnesi `compute_target`ve bir datastore `ds`nesnesi oluşturduğunuzu varsayar.

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

Modelinizi bir web hizmeti olarak dağıtırken ortamları kullanabilirsiniz. Bu özellik, yeniden çoğaltılabilir, bağlı bir iş akışı sağlar. Bu iş akışında, hem eğitim bilgiişleminizde hem de çıkarım bilginizde aynı kitaplıkları kullanarak modelinizi eğitebilir, sınayabilir ve dağıtabilirsiniz.

Bir web hizmetini dağıtmak için, dağıtım nesnenizde ortamı, çıkarım bilgisini, komut [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)dosyasını ve kayıtlı modeli birleştirin. Daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağıkonusunda](how-to-deploy-and-where.md)bakın.

Bu örnekte, bir eğitim çalışmasını tamamladığınızı varsayalım. Şimdi bu modeli Azure Kapsayıcı Örnekleri'ne dağıtmak istiyorsunuz. Web hizmetini oluşturduğunuzda, model ve puanlama dosyaları görüntüye monte edilir ve görüntüye Azure Machine Learning çıkarım yığını eklenir.

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

## <a name="example-notebooks"></a>Örnek defterler

Bu [örnek not defteri,](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) bu makalede gösterilen kavramlar ve yöntemler üzerine genişletir.

[Özel docker taban görüntüsünü kullanarak bir model dağıtma,](how-to-deploy-custom-docker-image.md) özel docker taban görüntüsünü kullanarak bir modelin nasıl dağıtılanın gerektiğini gösterir.

Bu [örnek not defteri,](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/spark) bir Spark modelinin web hizmeti olarak nasıl dağıtılangerektiğini gösterir.

## <a name="create-and-manage-environments-with-the-cli"></a>CLI ile ortamlar oluşturma ve yönetme

[Azure Machine Learning CLI,](reference-azure-machine-learning-cli.md) Python SDK'nın işlevselliğinin çoğunu yansıtıyor. Ortamlar oluşturmak ve yönetmek için kullanabilirsiniz. Bu bölümde tartıştığımız komutlar temel işlevselliği gösterir.

Aşağıdaki komut, belirtilen dizinde varsayılan ortam tanımı için dosyaları iskeleler. Bu dosyalar JSON dosyaları. SDK'daki sınıf gibi çalışıyorlar. Dosyaları özel ayarları olan yeni ortamlar oluşturmak için kullanabilirsiniz. 

```azurecli-interactive
az ml environment scaffold -n myenv -d myenvdir
```

Belirli bir dizinden bir ortamı kaydetmek için aşağıdaki komutu çalıştırın.

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

* Bir modeli eğitmek için yönetilen bir işlem hedefini kullanmak için [Bkz. Öğretici: Bir model eğitin.](tutorial-train-models-with-aml.md)
* Eğitimli bir modele sahip olduktan sonra, [modelleri nasıl ve nerede dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.
* Sınıf [ `Environment` SDK başvurugörüntüleyin.](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)
* Bu makalede açıklanan kavramlar ve yöntemler hakkında daha fazla bilgi için [örnek not defterine](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments)bakın.
