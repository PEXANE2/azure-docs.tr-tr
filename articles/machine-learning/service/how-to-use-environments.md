---
title: Model eğitimi ve dağıtımı için ortamları oluşturma, kullanma ve yönetme
titleSuffix: Azure Machine Learning
description: Model eğitimi ve dağıtımı için ortamları oluşturun ve yönetin. Bu ortam için Python paketlerini ve diğer ayarları yönetin.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 08/16/2019
ms.custom: seodec18
ms.openlocfilehash: 242f3cec0ac766e537bd0483725ba51685bb7ced
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70996631"
---
# <a name="create-and-manage-environments-for-training-and-deployment"></a>Eğitim ve dağıtım için ortamları oluşturma ve yönetme

Bu makalede, Azure Machine Learning [ortamları](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) oluşturma ve yönetme hakkında bilgi edinmek için projenizin yazılım bağımlılıklarını geliştikçe izleyip yeniden oluşturabilirsiniz.

Yazılım bağımlılığı yönetimi, geliştiriciler için ortak bir görevdir. Derlemelerin birçok el ile yazılım yapılandırması olmadan tekrarlanabilir olmasını sağlamak istiyorsunuz. PIP ve Conda gibi yerel geliştirme çözümleriyle, Azure Machine Learning ortamları sınıfı hem yerel hem de Dağıtılmış bulut geliştirmesi için bir çözüm sağlar.

Bu makaledeki örneklerde nasıl yapılacağı gösterilmektedir:

* Ortam oluşturma ve paket bağımlılıklarını belirtme
* Ortamları alma ve güncelleştirme
* Eğitim için ortamı kullanma
* Web hizmeti dağıtımı için ortamı kullanma

## <a name="what-are-environments"></a>Ortamlar nelerdir?

Ortamlar, eğitim ve Puanlama betikleriniz ve çalışma zamanları (Python, Spark veya Docker) içinde Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirtir. Bunlar, farklı işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışlarını etkinleştiren Azure Machine Learning çalışma alanınızda yönetilen ve sürümlü varlıklardır.

Eğitim betiğinizi geliştirmek için yerel işlem ortamınızda bir ortam nesnesi kullanabilir, aynı ortamı ölçeklendirerek model eğitimi için Azure Machine Learning Işlem sırasında yeniden kullanabilir ve hatta modelinizi aynı ortamla dağıtabilirsiniz.

Aşağıda, eğitim için çalışma yapılandırmanızda ve Web hizmeti dağıtımları için ınlekrime ve dağıtım yapılandırmanızda aynı ortam nesnesinin kullanılabileceği gösterilmektedir.

![Makine öğrenimi iş akışında ortam diyagramı](./media/how-to-use-environments/ml-environment.png)

### <a name="types-of-environments"></a>Çeşitli ortamlarda

Ortamlar, büyük ölçüde iki kategoriye ayrılabilir: **Kullanıcı tarafından yönetilen** ve **sistem tarafından yönetilen**.

Kullanıcı tarafından yönetilen bir ortam için ortamınızı ayarlamaktan ve eğitim betiğinizin ihtiyaç duyacağı her paketi işlem hedefinde yüklemeye sorumlusunuz. Conda, ortamınızı denetetmez veya sizin için herhangi bir şey yüklemez. 

Sistem tarafından yönetilen ortamlar, Python ortamını ve komut dosyası bağımlılıklarını yönetmek için [Conda](https://conda.io/docs/) 'yi kullanmak istediğinizde kullanılır. Hizmet, el ile yapılandırılamayan uzak işlem hedefleri üzerinde yararlarından dolayı bu tür ortamları varsayılan olarak varsayar.

## <a name="prerequisites"></a>Önkoşullar

* Python için Azure Machine Learning SDK 'Sı [yüklendi](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md).

## <a name="create-an-environment"></a>Ortam oluşturma

Denemeleri için bir ortam oluşturmanın birden çok yolu vardır.

### <a name="instantiate-an-environment-object"></a>Ortam nesnesi örneği oluşturma

Bir ortamı el ile oluşturmak için SDK 'dan ortam sınıfını içeri aktarın ve aşağıdaki kodla bir ortam nesnesi örneği oluşturun.

```python
from azureml.core import Environment
Environment(name="myenv")
```

### <a name="conda-and-pip-specification-files"></a>Conda ve PIP belirtim dosyaları

Ayrıca, Conda belirtimi veya bir PIP gereksinimleri dosyasından bir ortam oluşturabilirsiniz.
[From_conda_specification ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) veya [from_pip_requirements ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) yöntemini kullanın ve ortam adınızı ve istenen dosyanın dosya yolunu Yöntem bağımsız değişkenine ekleyin.

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

#From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="existing-conda-environment"></a>Mevcut Conda ortamı

Yerel bilgisayarınızda var olan bir Conda ortamınız varsa, hizmet bundan bir ortam nesnesi oluşturmaya yönelik bir çözüm sunar. Bu şekilde, yerel etkileşimli ortamınızı uzak çalıştırmalar üzerinde yeniden kullanabilirsiniz.

Aşağıdaki, `mycondaenv` [from_existing_conda_environment ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-) yöntemiyle mevcut Conda ortamından bir ortam nesnesi oluşturur.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="automatically-create-environments"></a>Ortamları otomatik olarak oluştur

Gönderme () yöntemiyle bir eğitim çalıştırarak otomatik olarak bir ortam oluşturun. Bir eğitim çalıştırması gönderdiğinizde, yeni ortam oluşturma, gerekli bağımlılıkların boyutuna bağlı olarak birkaç dakika sürebilir. 

Çalıştırmayı göndermeden önce çalıştırma yapılandırmanızda bir ortam belirtmezseniz, sizin için varsayılan bir ortam oluşturulur.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attaches training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Shows each step of run 
run.wait_for_completion(show_output=True)
```

Benzer şekilde, eğitim için bir [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) nesne kullanıyorsanız, tahmin aracı örneğini bir ortam belirtmek zorunda kalmadan doğrudan bir çalıştırma olarak gönderebilirsiniz, bunun nedeni `Estimator` nesnenin ortamı ve işlem hedefini zaten sarmalayıcısı olur.


## <a name="add-packages-to-an-environment"></a>Bir ortama paket ekleme

Bir ortama, Conda, PIP veya özel tekerlek dosyalarıyla paket ekleyin. [Condaddependency sınıfını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py)kullanarak her bir paket bağımlılığı belirtin ve ortamın PythonSection ekleyin.

### <a name="conda-and-pip-packages"></a>Conda ve PIP paketleri

Bir Conda Package deposunda bir paket varsa, PDA 'nın PIP yüklemesi üzerinden kullanılması önerilir. Bunun nedeni, Conda paketlerinin genellikle yüklemeyi daha güvenilir hale getirmek için önceden oluşturulmuş ikili dosyalarla gelir.

Aşağıdaki örnek, sırasıyla `scikit-learn` [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) ve `pillow` `myenv` [yöntemleriyleortama`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) , özellikle 0.21.3 ve paketini ekler.

```python
from azureml.core import Environment
from azureml.core.environment import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs scikit-learn version 0.21.3 conda package
conda_dep.add_conda_package("scikit-learn==0.21.3")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

### <a name="private-wheel-files"></a>Özel tekerlek dosyaları

Özel PIT tekerleği dosyalarını, önce statik [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-) yöntemi kullanarak çalışma alanı depolama alanınıza yükleyerek, ardından depolama URL 'sini yakalayıp ve URL 'yi `add_pip_package()` yöntemine geçirerek kullanabilirsiniz

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>Ortamları yönetin

Ortamları, işlem hedefleri genelinde ve çalışma alanının diğer kullanıcılarıyla güncelleştirmek, izlemek ve yeniden kullanmak için yönetin.

### <a name="register-environments"></a>Ortamları kaydetme

Bir Web hizmeti çalıştırdığınızda veya dağıtırken ortam, çalışma alanınıza otomatik olarak kaydedilir. Ayrıca [Register ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-) yöntemini kullanarak ortamı el ile kaydedebilirsiniz. Bu işlem, ortamı bulutta izlenen ve sürümlü bir varlığa yapar ve çalışma alanı kullanıcıları arasında paylaşılabilir.

Aşağıdaki, ortamını `myenv` `ws`çalışma alanına kaydeder.

```python
myenv.register(workspace=ws)
```

Eğitim veya dağıtımda ilk kez kullanıldığında, ortam, oluşturma ve işlem hedefinde dağıtılan çalışma alanına kaydedilir. Ortamlar hizmet tarafından önbelleğe alınır. Önbelleğe alınmış bir ortamı yeniden kullanmak yeni bir hizmet kullanmaktan veya güncelleştirilmiş bir hizmetin kullanılmasıyla çok daha az zaman alır.

### <a name="get-existing-environments"></a>Mevcut ortamları al

Ortam sınıfı, çalışma alanınızdaki mevcut ortamları bir liste olarak veya belirli bir eğitim çalıştırması ile almanızı sağlayan yöntemler sunar. Sorun giderme veya denetim amaçları için reproducibility

#### <a name="view-list-of-environments"></a>Ortamların listesini görüntüle

Çalışma alanınızdaki ortamları [List ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-)ile görüntüleyin ve sonra yeniden kullanmak için bir tane seçin.

```python
from azureml.core import Environment
list("workspace_name")
```

#### <a name="get-environment-by-name"></a>Ortamı ada göre al

Ayrıca, belirli bir ortamı ada ve sürüme göre de alabilirsiniz.
Aşağıdaki kod `myenv` , çalışma`ws` alanındaki ortamın sürümünü `1` almak için [Get ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) yöntemini kullanır.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="training-run-specific-environment"></a>Eğitim belirli bir ortamı Çalıştır

Eğitim tamamlandıktan sonra belirli bir çalıştırma için kullanılan ortamı almak için Run sınıfında [get_environment ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--)yöntemini kullanın.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Mevcut bir ortamı güncelleştirme

Bir Python paketi ekleme gibi var olan bir ortamda değişiklik yaparsanız, çalıştırma, dağıtım modeli gönderdiğinizde veya ortamı el ile kaydettiğinizde bir ortamın yeni bir sürümü oluşturulur. Sürüm oluşturma, zaman içinde ortamdaki değişiklikleri görüntülemenize olanak sağlar.

Mevcut bir ortamın Python paketi sürümünü güncelleştirmek için, bu paket için tam sürüm numarasını belirtin. Aksi takdirde, Azure Machine Learning ortamın oluşturulduğu sırada mevcut ortamı Paket sürümleriyle yeniden kullanacaktır.

### <a name="debug-the-image-build"></a>Görüntü derlemesinde hata ayıklama

Bu örnek, bir ortamı Docker görüntüsü olarak el ile oluşturmak için [Build ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace-) yöntemini kullanır ve [wait_for_completion ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-)kullanarak görüntü derlemeden çıkış günlüklerini izler. Oluşturulan görüntü daha sonra, hata ayıklama için yararlı olan Azure Container Registry çalışma alanı altında görüntülenir.

```python
from azureml.core import Image
build = env.build()
build.wait_for_completion(show_output=True)
```

## <a name="docker-and-environments"></a>Docker ve ortamları

 AzureMachineLearning`Environments` sınıfının [dockersection](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) , eğitim çalıştırmasının çalıştırıldığı Konuk işletim sisteminin ayrıntılarını özelleştirmenize ve denetlemenize olanak tanır.

`enable` Docker kullandığınızda, hizmet bir Docker görüntüsü oluşturur ve bu Docker kapsayıcısı içindeki belirtimlerinizi içeren bir Python ortamı oluşturur. Bu, eğitim çalışmalarınız için ek yalıtım ve reproducibility sağlar.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Oluşturulduktan sonra Docker görüntüsü, varsayılan olarak çalışma alanıyla ilişkili Azure Container Registry görüntülenir.  Depo adının *azureml/azureml_\<\>UUID*biçimi vardır. Benzersiz tanımlayıcı (*uuuıd*) bölümü, ortam yapılandırmasından hesaplanan bir karma öğesine karşılık gelir. Bu, hizmetin verilen ortama karşılık gelen bir görüntünün yeniden kullanım için zaten mevcut olup olmadığını belirlemesine izin verir.

Ayrıca, hizmet Ubuntu Linux tabanlı [temel görüntülerden](https://github.com/Azure/AzureML-Containers)birini otomatik olarak kullanır ve belirtilen Python paketlerini de kurar. Temel görüntüde CPU ve GPU sürümleri bulunur ve ayarı `gpu_support=True`yaparak GPU görüntüsünü belirtebilirsiniz.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"

# Specify GPU image
myenv.docker.gpu_support=True
```

> [!NOTE]
> Özel bir Docker görüntüsü kullanırken belirtirseniz `environment.python.user_managed_dependencies=False` , hizmet görüntüde bir Conda ortamı oluşturur ve temel görüntüde yüklü olabilecek Python kitaplıklarını kullanmak yerine bu ortamda çalışmayı yürütür. Parametresini `True` , yüklü paketlerinizi kullanacak şekilde ayarlayın.

## <a name="using-environments-for-training"></a>Eğitim için ortamları kullanma

Bir eğitim çalıştırması göndermek için ortamınızı, [işlem hedefini](concept-compute-target.md) ve eğitim komut dosyasını bir çalıştırma yapılandırması olarak birleştirmeniz gerekir; çalışma göndermek için kullanılan sarmalayıcı nesne.

Bir eğitim çalıştırması gönderdiğinizde, yeni bir ortamın oluşturulması, gerekli bağımlılıkların boyutuna bağlı olarak birkaç dakika sürebilir. Ortamlar hizmet tarafından önbelleğe alınır, bu nedenle ortam tanımı değişmeden kaldığı sürece tam kurulum süresi yalnızca bir kez uygulanır.

Aşağıda sarmalayıcı nesneniz olarak [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) kullanacağınız bir yerel betik çalıştırma örneği verilmiştir.

```python
from azureml.core import Environment, ScriptRunConfig, Experiment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.compute_target = "local"

# Attach environment to run config
runconfig.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> Çalıştırma geçmişini devre dışı bırakmak veya anlık görüntüleri çalıştırmak için altındaki `ScriptRunConfig.run_config.history`ayarı kullanın.

Çalışma yapılandırmanızda ortamı belirtmezseniz, çalıştırdığınız hizmet sizin için varsayılan bir ortam oluşturacaktır.

### <a name="train-with-an-estimator"></a>Bir tahmin ile eğitme

Eğitim için bir [tahmin aracı](how-to-train-ml-models.md) kullanıyorsanız, yalnızca ortamı ve işlem hedefini sarmalayan için tahmin aracı örneğini doğrudan gönderebilirsiniz.

Aşağıda, bir scikit-öğren modeli için uzak bir işlem üzerinde çalışan tek düğümlü bir eğitim için bir tahmin aracı kullanılmaktadır ve daha önce oluşturulmuş bir işlem hedefi nesnesi `compute_target` ve veri deposu `ds`nesnesi olduğunu varsaymaktadır.

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

## <a name="using-environments-for-web-service-deployment"></a>Web hizmeti dağıtımı için ortamları kullanma

Modelinizi bir Web hizmeti olarak dağıttığınızda ortamları kullanabilirsiniz. Bu, hem öğreticinizdeki hem de ınpam hesaplamada aynı kitaplıkları kullanarak modelinizi eğtabileceğiniz, test ettiğiniz ve dağıtabileceğiniz, tekrarlanabilir, bağlantılı bir iş akışına izin vermez.

Bir Web hizmeti dağıtmak için ortam, çıkarım işlem, Puanlama betiği ve dağıtım nesneniz içindeki kayıtlı modeli birleştirin, [dağıtın ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-). [Web hizmetlerini dağıtma](how-to-deploy-and-where.md)hakkında daha fazla bilgi edinin.

Bu örnekte, bu modeli bir Azure Container Instance 'a (ACI) dağıtmak istediğiniz bir eğitim çalıştırmasını tamamladığınızı varsayalım. Web hizmetini oluştururken, model ve Puanlama dosyaları görüntüye bağlanır ve Azure Machine Learning ınırya yalıtma yığını görüntüye eklenir.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="example-notebooks"></a>Örnek Not Defterleri

Bu [örnek Not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) , bu makalede gösterilen kavramların ve yöntemlerin üzerine genişletilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Bir modeli](tutorial-train-models-with-aml.md) eğitme bir modeli eğmek için yönetilen bir işlem hedefi kullanır.
* Eğitilen bir modelden sonra [modellerin nasıl ve nereye dağıtılacağını](how-to-deploy-and-where.md)öğrenin.
* [Ortam sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) SDK başvurusunu görüntüleyin.
