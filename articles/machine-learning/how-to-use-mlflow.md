---
title: ML denemeleri için MLflow Izleme
titleSuffix: Azure Machine Learning
description: ML modellerinden ölçümleri ve yapıtları günlüğe kaydetmek için Azure Machine Learning ile MLflow ayarlayın ve ML modellerinizi bir Web hizmeti olarak dağıtın.
services: machine-learning
author: shivp950
ms.author: shipatel
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.date: 09/08/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 116faae1bc0a93ce2007fcf809a8c96475289036
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90897195"
---
# <a name="track-model-metrics-and-deploy-ml-models-with-mlflow-and-azure-machine-learning-preview"></a>MLflow ve Azure Machine Learning (Önizleme) ile model ölçümlerini izleyin ve ML modellerini dağıtın

Bu makalede, mlflow denemeleri ve Azure Machine Learning bağlamak için mlflow izleme URI 'SI ve günlük API 'si ve [Mlflow izleme](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api)olarak bilinen günlüğe kaydetme API 'sinin nasıl etkinleştirileceği gösterilmektedir. 

MLflow için Azure Machine Learning yerel destekle birlikte şunları yapabilirsiniz

+ [Azure Machine Learning çalışma](https://docs.microsoft.com/azure/machine-learning/concept-azure-machine-learning-architecture#workspaces)alanınızdaki deneme ölçümlerini ve yapıtları izleyin ve günlüğe kaydedin. Denemeleri için MLflow Izlemeyi zaten kullanıyorsanız, çalışma alanı eğitim ölçümlerini ve modellerini depolamak için merkezi, güvenli ve ölçeklenebilir bir konum sağlar.

+ Azure Machine Learning arka uç desteğiyle (Önizleme) MLflow projeleriyle eğitim işleri gönderebilirsiniz. İşleri Azure Machine Learning izlemeye yerel olarak gönderebilir veya yürütmeleri [Azure Machine Learning bir işlem](https://docs.microsoft.com/azure/machine-learning/how-to-create-attach-compute-sdk#amlcompute)aracılığıyla buluta geçirebilirsiniz.

+ MLflow ve Azure Machine Learning modeli kayıt defterindeki modelleri izleyin ve yönetin.

+ MLflow denemeleri 'nizi Azure Machine Learning Web hizmeti olarak dağıtın. Web hizmeti olarak dağıtarak, Azure Machine Learning izleme ve veri DRI algılama işlevlerini üretim modellerinize uygulayabilirsiniz. 

[Mlflow](https://www.mlflow.org) , Machine Learning denemeleri 'in yaşam döngüsünü yönetmeye yönelik açık kaynaklı bir kitaplıktır. MLFlow Izleme, bir MLflow bileşeni olan ve denemenizin ortamınız tarafından, uzak bir işlem hedefinde, bir sanal makinede veya Azure Databricks kümesinde yerel olarak, kendi ortamınızda olduğu gibi, eğitim çalıştırma ölçümleri ve model yapıtlarını kaydeder ve izler. 

>[!NOTE]
> Açık kaynak kitaplığı olarak MLflow sık sık değişir. Bu nedenle, Azure Machine Learning ve MLflow tümleştirmesiyle sunulan işlevlerin Microsoft tarafından tam olarak desteklenmeden bir önizleme olarak değerlendirilmesi gerekir.

Aşağıdaki diyagramda, MLflow Izlemenin yanı sıra, Azure Machine Learning çalışma alanınızda bir deneyin çalışma ölçümlerini ve mağaza modeli yapılarını izlersiniz.

![Azure Machine Learning diyagramı ile mlflow](./media/how-to-use-mlflow/mlflow-diagram-track.png)

> [!TIP]
> Bu belgedeki bilgiler öncelikli olarak, model eğitimi sürecini izlemek isteyen veri bilimcileri ve geliştiricileri içindir. Kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning kaynak kullanımını ve olayları izlemek isteyen bir yöneticiyseniz, bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>MLflow ve Azure Machine Learning istemcilerini karşılaştırın

 Aşağıdaki tabloda Azure Machine Learning kullanılabilecek farklı istemciler ve ilgili işlev özellikleri özetlenmektedir.

 MLflow Izleme yalnızca [Azure Machine Learning Python SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)aracılığıyla kullanılabilen ölçüm günlüğü ve yapıt depolama işlevleri sunar.

| Özellik | MLflow Izleme & dağıtımı | Python SDK Azure Machine Learning |  Azure Machine Learning CLı | Azure Machine Learning Studio|
|---|---|---|---|---|
| Çalışma alanını yönet |   | ✓ | ✓ | ✓ |
| Veri depolarını kullanma  |   | ✓ | ✓ | |
| Günlük ölçümleri      | ✓ | ✓ |   | |
| Yapıtları karşıya yükle | ✓ | ✓ |   | |
| Ölçümleri görüntüle     | ✓ | ✓ | ✓ | ✓ |
| İşlemi yönetme   |   | ✓ | ✓ | ✓ |
| Modelleri dağıtma    | ✓ | ✓ | ✓ | ✓ |
|Model performansını izleme||✓|  |   |
| Veri değişikliklerini algılama |   | ✓ |   | ✓ |

## <a name="prerequisites"></a>Önkoşullar

* [MLflow 'ı yükler.](https://mlflow.org/docs/latest/quickstart.html)
* [Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true) yerel BILGISAYARıNıZA yüklediğinizde SDK, çalışma alanınıza erişmek Için mlflow bağlantısını sağlar.
* [Azure Machine Learning çalışma alanı oluşturun](how-to-manage-workspace.md).

## <a name="track-local-runs"></a>Yerel çalıştırmaları izle

Azure Machine Learning ile MLflow Izleme, yerel çalıştırmanıza ait günlüğe kaydedilen ölçümleri ve yapıtları Azure Machine Learning çalışma alanınıza depolamanıza olanak sağlar.

`azureml-mlflow`Denemeleri veya kod düzenleyicisinde yerel olarak Jupyter Notebook çalıştırdığınız Azure Machine Learning Ile MLflow izlemeyi kullanmak için paketini yükler.

```shell
pip install azureml-mlflow
```

`mlflow` [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py&preserve-view=true) Mlflow 'un izleme URI 'sine erişmek ve çalışma alanınızı yapılandırmak için ve sınıflarını içeri aktarın.

Aşağıdaki kodda `get_mlflow_tracking_uri()` yöntemi, çalışma alanına benzersiz bir Izleme URI adresi atar `ws` ve `set_tracking_uri()` MLFLOW izleme URI 'sini bu adrese yönlendirir.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>İzleme URI 'SI saat veya daha az bir saate kadar geçerlidir. Bir boşta kalma zamanından sonra betiğinizi yeniden başlatırsanız, yeni bir URI almak için get_mlflow_tracking_uri API 'sini kullanın.

MLflow deney adını ile ayarlayın `set_experiment()` ve öğreticinizi ile çalıştırın `start_run()` . Daha sonra `log_metric()` MLflow günlüğü API 'sini etkinleştirmek ve eğitim çalıştırma ölçümlerinizi günlüğe kaydetmeye başlamak için kullanın.

```Python
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Uzak çalıştırmaları izle

Azure Machine Learning ile MLflow Izleme, uzak çalıştırmanıza ait günlüğe kaydedilen ölçümleri ve yapıtları Azure Machine Learning çalışma alanınıza depolamanıza olanak sağlar.

Uzak çalıştırmalar, modellerinizi GPU etkin sanal makineler veya Machine Learning İşlem kümeler gibi daha güçlü bir şekilde bir şekilde eğitmenizi sağlar. Farklı işlem seçenekleri hakkında bilgi edinmek için bkz. [model eğitimi için işlem hedeflerini kullanma](how-to-set-up-training-targets.md) .

İşlem ve eğitim çalıştırma ortamınızı [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true) sınıfıyla yapılandırın. `mlflow` `azureml-mlflow` Ortamın bölümüne ekleme ve PIP paketleri [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py&preserve-view=true) . Sonra  [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py&preserve-view=true) işlem hedefi olarak uzak işlem ile oluşturun.

```Python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = 'my_workspace',
                 name='my_experiment')

mlflow_env = Environment(name='mlflow-env')

cd = CondaDependencies.create(pip_packages=['mlflow', 'azureml-mlflow'])

mlflow_env.python.conda_dependencies = cd

src = ScriptRunConfig(source_directory='./my_script_location', script='my_training_script.py')

src.run_config.target = 'my-remote-compute-compute'
src.run_config.environment = mlflow_env
```

Eğitim betiğinizdeki `mlflow` MLflow günlüğü API 'lerini kullanmak için içeri aktarın ve çalıştırma ölçümlerinizi günlüğe kaydetmeye başlayın.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric('example', 1.23)
```

Bu işlem ve eğitim çalıştırma yapılandırmasıyla, `Experiment.submit()` bir çalıştırma göndermek için yöntemini kullanın. Bu yöntem, MLflow izleme URI 'sini otomatik olarak ayarlar ve MLflow oturumunu çalışma alanınıza yönlendirir.

```Python
run = exp.submit(src)
```

## <a name="train-with-mlflow-projects"></a>MLflow projeleriyle eğitme

[Mlflow projeleri](https://mlflow.org/docs/latest/projects.html) , diğer veri bilimcilerinin (veya otomatikleştirilmiş araçların) çalışmasını sağlamak için kodunuzu düzenlemenize ve açıklamanıza olanak tanır. Azure Machine Learning olan MLflow projeleri, çalışma alanınızda eğitim çalıştırmalarını izlemenize ve yönetmenize olanak sağlar. 

Bu örnek, Azure Machine Learning izleme ile MLflow projelerinin yerel olarak nasıl gönderileceği gösterilmektedir.

`azureml-mlflow`Denemeleri yerel bilgisayarınızda Azure Machine Learning Ile MLflow izlemeyi kullanmak için paketini yükler. Denemeleri, bir Jupyter Not defteri veya kod Düzenleyicisi aracılığıyla çalıştırılabilir.

```shell
pip install azureml-mlflow
```

`mlflow` [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) Mlflow 'un izleme URI 'sine erişmek ve çalışma alanınızı yapılandırmak için ve sınıflarını içeri aktarın.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

MLflow deney adını ile ayarlayın `set_experiment()` ve öğreticinizi ile çalıştırın `start_run()` . Ardından, `log_metric()` MLflow günlüğü API 'sini etkinleştirmek ve eğitim çalıştırma ölçümlerinizi günlüğe kaydetmeye başlamak için kullanın.

```Python
experiment_name = 'experiment-with-mlflow-projects'
mlflow.set_experiment(experiment_name)
```

İşlem hedefi ve kullanılacak yönetilen ortam türü gibi tümleştirme için gerekli bilgileri depolamak üzere arka uç yapılandırma nesnesi oluşturun.

```python
backend_config = {"USE_CONDA": False}
```
`azureml-mlflow`Çalışma alanınızdaki ölçümleri ve anahtar yapıtları izlemek için, paketi ortam yapılandırma dosyanıza bir PIP bağımlılığı olarak ekleyin. 

``` shell
name: mlflow-example
channels:
  - defaults
  - anaconda
  - conda-forge
dependencies:
  - python=3.6
  - scikit-learn=0.19.1
  - pip
  - pip:
    - mlflow
    - azureml-mlflow
```
Yerel çalıştırmayı gönder ve parametresini ayarlamış olduğunuzdan emin olun `backend = "azureml" ` . Bu ayarla, çalıştırmaları yerel olarak gönderebilir ve çalışma alanınızda otomatik çıkış izleme, günlük dosyaları, anlık görüntüler ve yazdırılmış hatalar için ek destek alabilirsiniz. 

[Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md)'da çalıştırmaların ve ölçümlerinizi görüntüleyin. 


```python
local_env_run = mlflow.projects.run(uri=".", 
                                    parameters={"alpha":0.3},
                                    backend = "azureml",
                                    use_conda=False,
                                    backend_config = backend_config, 
                                    )

```

## <a name="track-azure-databricks-runs"></a>Azure Databricks çalıştırmalarını izleyin

Azure Machine Learning ile MLflow Izleme, günlüğe kaydedilen ölçümleri ve yapıtları, Azure Databricks çalışmalarından her seferinde aşağıdaki alanlardan oluşan üç bölümde depolamanızı sağlar: 

* Azure Machine Learning çalışma alanı
* Azure Databricks çalışma alanı.
* MLflow

Mlflow denemeleri Azure Databricks çalıştırmak için öncelikle bir [Azure Databricks çalışma alanı ve küme](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturmanız gerekir. Kümenizde, kümenizin gerekli işlevlere ve sınıflara erişimi olduğundan emin olmak için PyPi 'den *azureml-mlflow* kitaplığını yüklersiniz.

> [!NOTE]
> `azureml.core`Paket şunları içerir `azureml-mlflow` . Zaten yüklüyse `azureml.core` , `azureml-mlflow` yükleme adımını atlayabilirsiniz. 

Buradan, deneme not defterinizi içeri aktarın, Azure Databricks kümenize ekleyin ve denemenizi çalıştırın. 

### <a name="install-libraries"></a>Kitaplıkları yükler

Kümenizi kümenize yüklemek için **Kitaplıklar** sekmesine gidin ve **Yeni** ' ye tıklayın

 ![Azure databricks ile mlflow](./media/how-to-use-mlflow/azure-databricks-cluster-libraries.png)

**Paket** alanına, azureml-mlflow yazın ve ardından Install ' a tıklayın. Bu adımı, denemeniz için kümenize diğer ek paketleri yüklemek için gereken şekilde tekrarlayın.

 ![Azure DB install mlflow kitaplığı](./media/how-to-use-mlflow/install-libraries.png)

### <a name="set-up-your-notebook-and-workspace"></a>Not defterinizi ve çalışma alanınızı ayarlama

Kümeniz kurulduktan sonra, deneme not defterinizi içeri aktarın, açın ve kümenizi ona ekleyin.

Aşağıdaki kod, deneme Not defterinizde olmalıdır. Bu kod, çalışma alanınızı oluşturmak için Azure aboneliğinizin ayrıntılarını alır. Bu kod, var olan bir kaynak grubuna ve Azure Machine Learning çalışma alanına sahip olduğunuzu varsayar, aksi takdirde [bunları oluşturabilirsiniz](how-to-manage-workspace.md). 

```python
import mlflow
import mlflow.azureml
import azureml.mlflow
import azureml.core

from azureml.core import Workspace
from azureml.mlflow import get_portal_url

subscription_id = 'subscription_id'

# Azure Machine Learning resource group NOT the managed resource group
resource_group = 'resource_group_name' 

#Azure Machine Learning workspace name, NOT Azure Databricks workspace
workspace_name = 'workspace_name'  

# Instantiate Azure Machine Learning workspace
ws = Workspace.get(name=workspace_name,
                   subscription_id=subscription_id,
                   resource_group=resource_group)
```

### <a name="connect-your-azure-databricks-and-azure-machine-learning-workspaces"></a>Azure Databricks ve Azure Machine Learning çalışma alanlarınızı bağlama

[Azure Portal](https://ms.portal.azure.com), Azure Databricks (ADB) çalışma alanınızı yeni veya mevcut bir Azure Machine Learning çalışma alanına bağlayabilirsiniz. Bunu yapmak için ADB çalışma alanınıza gidin ve sağ alt köşedeki **Azure Machine Learning çalışma alanını bağla** düğmesini seçin. Çalışma alanlarınızı bağlamak, deneme verilerinizi Azure Machine Learning çalışma alanında izlemenize olanak sağlar. 

### <a name="mlflow-tracking-in-your-workspaces"></a>Çalışma Alanlarınızdaki MLflow izleme

Çalışma alanınızı örnekledikten sonra, MLflow izleme otomatik olarak aşağıdaki yerlerde izlenecek şekilde ayarlanır:

* Bağlı Azure Machine Learning çalışma alanı.
* Özgün ADB çalışma alanınız. 
* MLflow. 

Tüm denemeleri, yönetilen Azure Machine Learning izleme hizmetine eklenecektir.

#### <a name="set-mlflow-tracking-to-only-track-in-your-azure-machine-learning-workspace"></a>MLflow Izlemeyi yalnızca Azure Machine Learning çalışma alanınızda izlemek üzere ayarla

İzlenen denemeleri 'yi merkezi bir konumda yönetmeyi tercih ediyorsanız, MLflow izlemesini **yalnızca** Azure Machine Learning çalışma alanınızda izlemek üzere ayarlayabilirsiniz. 


```python
uri = ws.get_mlflow_tracking_uri()
mlflow.set_tracking_uri(uri)
```

Eğitim betiğinizdeki `mlflow` MLflow günlüğü API 'lerini kullanmak için içeri aktarın ve çalıştırma ölçümlerinizi günlüğe kaydetmeye başlayın. Aşağıdaki örnek, dönem kayıp ölçümünü günlüğe kaydeder. 

```python
import mlflow 
mlflow.log_metric('epoch_loss', loss.item()) 
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Çalışma alanınızdaki ölçümleri ve yapıtları görüntüleme

MLflow günlüğü 'ndeki ölçümler ve yapıtlar çalışma alanınızda tutulur. Bunları dilediğiniz zaman görüntülemek için, çalışma alanınıza gidin ve [Azure Machine Learning Studio](https://ml.azure.com)'daki çalışma alanınızda ada göre deneyin ' i bulun.  Veya aşağıdaki kodu çalıştırın. 

```python
run.get_metrics()
ws.get_details()
```

## <a name="manage-models"></a>Modelleri yönetme 

MLflow modeli kayıt defterini destekleyen [Azure Machine Learning modeli kayıt defteriyle](concept-model-management-and-deployment.md#register-package-and-deploy-models-from-anywhere) modellerinizi kaydedin ve izleyin. Azure Machine Learning modeller, bu modelleri farklı iş akışlarıyla dışarı ve içeri aktarmayı kolaylaştıran MLflow model şeması ile hizalanır. Çalıştırma kimliği gibi MLflow ile ilgili meta veriler de izlenebilirlik için kayıtlı modelle etiketlenir. Kullanıcılar, MLflow çalıştırmalarından üretilen eğitim çalıştırmaları, kayıt ve dağıtım modellerini gönderebilir. 

Üretime yönelik modeli tek bir adımda dağıtmak ve kaydettirmek istiyorsanız bkz. [MLflow modellerini dağıtma ve kaydetme](#deploy-and-register-mlflow-models).

Bir çalışma kaynağından bir modeli kaydetmek ve görüntülemek için aşağıdaki adımları kullanın:

1. Çalıştırma tamamlandıktan sonra `register_model()` yöntemi çağırın.

    ```python
    # the model folder produced from the run is registered. This includes the MLmodel file, model.pkl and the conda.yaml.
    run.register_model(model_name = 'my-model', model_path = 'model')
    ```

1. [Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md)ile çalışma alanınızda kayıtlı modeli görüntüleyin.

    Aşağıdaki örnekte kayıtlı modelde, `my-model` MLflow izleme meta verileri etiketlendi. 

    ![kayıtlı-mlflow-model](./media/how-to-use-mlflow/registered-mlflow-model.png)

1. MLflow model şeması (Conda. YAML, MLmodel, model. pkl) ile hizalı tüm model dosyalarını görmek için **yapılar** sekmesini seçin.

    ![Model-şema](./media/how-to-use-mlflow/mlflow-model-schema.png)

1. Çalıştırma tarafından oluşturulan MLmodel dosyasını görmek için MLmodel ' i seçin.

    ![MLmodel-şema](./media/how-to-use-mlflow/mlmodel-view.png)



## <a name="deploy-and-register-mlflow-models"></a>MLflow modellerini dağıtma ve kaydetme 

MLflow denemeleri 'i Azure Machine Learning Web hizmeti olarak dağıtmak, üretim modellerinize Azure Machine Learning model yönetimi ve veri Drın algılama özelliklerini kullanmanıza ve uygulamanıza olanak tanır.

Bunu yapmak için, şunu yapmanız gerekir

1. Modelinizi kaydedin.
1. Senaryolarınız için kullanmak istediğiniz dağıtım yapılandırmasını saptayın.

    1. [Azure Container Instance (acı)](#deploy-to-aci) , hızlı bir geliştirme ve test dağıtımı için uygun bir seçimdir.
    1. [Azure Kubernetes hizmeti (AKS)](#deploy-to-aks) , ölçeklenebilir üretim dağıtımları için uygundur.

Aşağıdaki diyagramda, MLflow dağıtım API 'SI ile, var olan MLflow modellerinizi bir Azure Machine Learning Web hizmeti olarak dağıtabileceğiniz, çerçeveler--PyTorch, TensorFlow, scikit-öğren, ONNX, vb. ve çalışma alanınızdaki üretim modellerinizi yönetme gibi işlemler gösterilmektedir.

![ Azure Machine Learning ile mlflow modellerini dağıtma](./media/how-to-use-mlflow/mlflow-diagram-deploy.png)


### <a name="deploy-to-aci"></a>ACI'ye dağıtma

[Deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) yöntemiyle dağıtım yapılandırmanızı ayarlayın. Web hizmetinizi izlemeye yardımcı olmak için Etiketler ve açıklamalar da ekleyebilirsiniz.

```python
from azureml.core.webservice import AciWebservice, Webservice

# Set the model path to the model folder created by your run
model_path = "model"

# Configure 
aci_config = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                memory_gb=1, 
                                                tags={'method' : 'sklearn'}, 
                                                description='Diabetes model',
                                                location='eastus2')
```

Ardından, Azure Machine Learning SDK [Deploy](/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) yöntemiyle modeli tek bir adımda kaydedin ve dağıtın. 

```python
(webservice,model) = mlflow.azureml.deploy( model_uri='runs:/{}/{}'.format(run.id, model_path),
                      workspace=ws,
                      model_name='sklearn-model', 
                      service_name='diabetes-model-1', 
                      deployment_config=aci_config, 
                      tags=None, mlflow_home=None, synchronous=True)

webservice.wait_for_deployment(show_output=True)
```

### <a name="deploy-to-aks"></a>AKS’ye dağıtma

AKS 'e dağıtmak için önce bir AKS kümesi oluşturun. [ComputeTarget. Create ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py#&preserve-view=truecreate-workspace--name--provisioning-configuration-) yöntemini kullanarak bir aks kümesi oluşturun. Yeni bir küme oluşturmak 20-25 dakika sürebilir.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aks-mlflow'

# Create the cluster
aks_target = ComputeTarget.create(workspace=ws, 
                                  name=aks_name, 
                                  provisioning_configuration=prov_config)

aks_target.wait_for_completion(show_output = True)

print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```
[Deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) yöntemiyle dağıtım yapılandırmanızı ayarlayın. Web hizmetinizi izlemeye yardımcı olmak için Etiketler ve açıklamalar da ekleyebilirsiniz.

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True, compute_target_name='aks-mlflow')

```

Ardından, modeli Azure Machine Learning SDK [Deploy ()] ile tek bir adımda kaydedip dağıtın (sonra, modeli Azure Machine Learning SDK [Deploy](/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) metodunu kullanarak kaydedin ve dağıtın. 

```python

# Webservice creation using single command
from azureml.core.webservice import AksWebservice, Webservice

# set the model path 
model_path = "model"

(webservice, model) = mlflow.azureml.deploy( model_uri='runs:/{}/{}'.format(run.id, model_path),
                      workspace=ws,
                      model_name='sklearn-model', 
                      service_name='my-aks', 
                      deployment_config=aks_config, 
                      tags=None, mlflow_home=None, synchronous=True)


webservice.wait_for_deployment()
```

Hizmet dağıtımı birkaç dakika sürebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Çalışma alanınızda günlüğe kaydedilen ölçümleri ve yapıtları kullanmayı planlamıyorsanız, bunları tek tek silme özelliği şu anda kullanılamaz. Bunun yerine, depolama hesabı ve çalışma alanını içeren kaynak grubunu silip herhangi bir ücret ödemeniz gerekmez:

1. Azure portalının en sol tarafındaki **Kaynak gruplarını** seçin.

   ![Azure portal Sil](./media/how-to-use-mlflow/delete-resources.png)

1. Listeden oluşturduğunuz kaynak grubunu seçin.

1. **Kaynak grubunu sil**'i seçin.

1. Kaynak grubu adını girin. Ardından **Sil**’i seçin.

## <a name="example-notebooks"></a>Örnek not defterleri

[Azure ML Not defterleri Ile Mlflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/track-and-monitor-experiments/using-mlflow) , bu makalede sunulan kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar

* [Modellerinizi yönetin](concept-model-management-and-deployment.md).
* [Veri kayması](how-to-monitor-data-drift.md)için üretim modellerinizi izleyin.
