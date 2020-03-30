---
title: ML deneyleri için MLflow Takibi
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile MLflow'u, Databricks kümelerinde, yerel ortamınızda veya VM ortamında oluşturulan ML modellerinden ölçümleri ve yapıları günlüğe kaydetmek için ayarlayın.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: dce7db9fc508c70d79be62a7e97b3bf52a316b22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76983707"
---
# <a name="track-models-metrics-with-mlflow-and-azure-machine-learning-preview"></a>MLflow ve Azure Machine Learning ile model ölçümlerini izleme (önizleme)

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, MLflow denemelerinizi ve Azure Machine Learning'inizi bağlamak için MLflow'un izleme URI'sini ve toplu olarak [MLflow İzleme](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api)olarak bilinen günlük API'sini nasıl etkinleştireceğimiz gösteriş. Bunu yapmak, [Azure Machine Learning çalışma alanınızda](https://docs.microsoft.com/azure/machine-learning/concept-azure-machine-learning-architecture#workspaces)deneme ölçümlerini ve yapılarını izlemenize ve günlüğe kaydetmenize olanak tanır. Denemeleriniz için MLflow İzleme'yi zaten kullanıyorsanız, çalışma alanı eğitim ölçümlerini ve modellerini depolamak için merkezi, güvenli ve ölçeklenebilir bir konum sağlar.

<!--
+ Deploy your MLflow experiments as an Azure Machine Learning web service. By deploying as a web service, you can apply the Azure Machine Learning monitoring and data drift detection functionalities to your production models. 
-->

[MLflow,](https://www.mlflow.org) makine öğrenimi deneylerinizin yaşam döngüsünü yönetmek için açık kaynak kodlu bir kitaplıktır. MLFlow Tracking, denemenizin ortamı ne olursa olsun, uzaktan bilgi işlem hedefi, sanal makine veya Azure Databricks kümesinde, denemenizin ortamı ne olursa olsun, eğitim çalışma ölçümlerinizi ve model yapılarınızı günlüğe kaydeden ve izleyen MLflow'un bir bileşenidir. 

Aşağıdaki diyagram, MLflow İzleme ile bir denemenin çalışma ölçümlerini izlediğinizve model yapılarını Azure Machine Learning çalışma alanınızda sakladığınızı göstermektedir.

![masmavi makine öğrenme diyagramı ile mlflow](./media/how-to-use-mlflow/mlflow-diagram-track.png)

> [!TIP]
> Bu belgedeki bilgiler öncelikle model eğitim sürecini izlemek isteyen veri bilimciler ve geliştiriciler içindir. Kotalar, tamamlanmış eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning'deki kaynak kullanımını ve etkinlikleri izlemek isteyen bir yöneticiyseniz, [bkz.](monitor-azure-machine-learning.md)

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>MLflow ve Azure Machine Learning istemcilerini karşılaştırın

 Aşağıdaki tabloda, Azure Machine Learning'i kullanabilen farklı istemciler ve bunların ilgili işlev yetenekleri özetlenmiştir.

 MLflow Tracking, [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)üzerinden yalnızca başka bir şekilde kullanılabilen metrik günlük ve yapı depolama işlevleri sunar.


| | MLflow&nbsp;Takibi <!--& Deployment--> | Azure Machine Öğrenme Python SDK |  Azure Makine Öğrenme CLI | Azure Machine Learning Studio|
|---|---|---|---|---|
| Çalışma alanını yönetme |   | ✓ | ✓ | ✓ |
| Veri depolarını kullanma  |   | ✓ | ✓ | |
| Günlük ölçümleri      | ✓ | ✓ |   | |
| Eserleri yükleme | ✓ | ✓ |   | |
| Ölçümleri görüntüle     | ✓ | ✓ | ✓ | ✓ |
| İşlemi yönetme   |   | ✓ | ✓ | ✓ |

<!--| Deploy models    | ✓ | ✓ | ✓ | ✓ |
|Monitor model performance||✓|  |   |
| Detect data drift |   | ✓ |   | ✓ |
-->
## <a name="prerequisites"></a>Ön koşullar

* [MLflow'u yükleyin.](https://mlflow.org/docs/latest/quickstart.html)
* [Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) yerel bilgisayarınıza yükleyin SDK, Çalışma alanınıza erişmek için MLflow bağlantısı sağlar.
* [Azure Makine Öğrenimi Çalışma Alanı oluşturun.](how-to-manage-workspace.md)

## <a name="track-local-runs"></a>Yerel çalıştırmaları izleme

Azure Machine Learning ile MLflow İzleme, yerel koşularınızdan günlüğe kaydedilmiş ölçümleri ve yapıları Azure Machine Learning çalışma alanınızda saklamanıza olanak tanır.

Bir `azureml-mlflow` Jupyter Notebook veya kod düzenleyicisinde yerel olarak yürütülen denemelerinizde Azure Machine Learning ile MLflow İzleme'yi kullanmak için paketi yükleyin.

```shell
pip install azureml-mlflow
```

>[!NOTE]
>Hizmeti geliştirmeye çalıştığımıziçin azureml.contrib ad alanı sık sık değişir. Bu nedenle, bu ad alanındaki her şey önizleme olarak kabul edilmeli ve Microsoft tarafından tam olarak desteklenmemelidir.

MLflow'un izleme URI'sine erişmek `mlflow` ve çalışma alanınızı yapılandırmak için sınıfları [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) içe aktarın.

Aşağıdaki kodda, `get_mlflow_tracking_uri()` yöntem çalışma alanına `ws`benzersiz bir izleme URI adresi `set_tracking_uri()` atar ve MLflow izleme URI'yi bu adrese işaret eleştirir.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>İzleme URI bir saat veya daha az geçerlidir. Komut dosyanızı boşta kaldıktan sonra yeniden başlatAcakolursanız, yeni bir URI almak için get_mlflow_tracking_uri API'yi kullanın.

MLflow deneme adını `set_experiment()` ayarlayın ve eğitim `start_run()`çalışmanızı '' ile başlatın. Ardından, `log_metric()` MLflow günlük API'sini etkinleştirmek ve eğitim çalıştırma ölçümlerinizi günlüğe kaydetmeye başlamak için kullanın.

```Python
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Uzaktan çalıştırmaları izleme

Azure Machine Learning ile MLflow İzleme, kaydedilmiş ölçümleri ve yapıları uzaktan çalıştırmalarınızdan Azure Machine Learning çalışma alanınızda saklamanıza olanak tanır.

Uzaktan çalıştırmalar, modellerinizi GPU özellikli sanal makineler veya Machine Learning Compute kümeleri gibi daha güçlü hesaplamalar üzerinde eğitmenize olanak sağlar. Farklı bilgi işlem seçenekleri hakkında bilgi edinmek [için model eğitimi için bilgi işlem hedeflerini ayarlama'ya](how-to-set-up-training-targets.md) bakın.

İşlem ve eğitim çalışma ortamınızı [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) sınıfla birlikte yapılandırın. Çevre `mlflow` `azureml-mlflow` [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) bölümüne paketleri ekleyin ve pip layın. Sonra [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) işlem hedefi olarak uzaktan işlem ile inşa edin.

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

Eğitim komut dosyanızda, MLflow günlük API'lerini kullanmak için içe aktarın `mlflow` ve çalışma ölçümlerinizi günlüğe kaydetmeye başlayın.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric('example', 1.23)
```

Bu işlem ve eğitim çalıştırma yapılandırması `Experiment.submit('train.py')` ile, bir çalıştırma göndermek için yöntemi kullanın. Bu yöntem, MLflow izleme URI'yi otomatik olarak ayarlar ve günlüğe kaydetmeyi MLflow'dan Çalışma Alanınıza yönlendirir.

```Python
run = exp.submit(src)
```

## <a name="track-azure-databricks-runs"></a>Azure Veri Tuğlaları çalışanlarını izleme

Azure Machine Learning ile MLflow İzleme, Azure Veri Tuğlaları'nızdaki günlüğe kaydedilmiş ölçümleri ve yapıları Azure Machine Learning çalışma alanınızda depolamanıza olanak tanır.

Azure Databricks ile Mlflow denemelerinizi çalıştırmak için öncelikle bir [Azure Databricks çalışma alanı ve küme](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturmanız gerekir. Kümenizde, kümenizin gerekli işlevlere ve sınıflara erişebilmesini sağlamak için PyPi'den *azureml-mlflow* kitaplığını yüklediğinizden emin olun.

Buradan deneme not defterinizi içe aktarın, Azure Databricks kümenize takın ve denemenizi çalıştırın. 

### <a name="install-libraries"></a>Kitaplıkları yükleme

Kümenize kitaplıkyüklemek için **Kitaplıklar** sekmesine gidin ve **Yeni Yükle'yi** tıklatın

 ![masmavi makine öğrenme diyagramı ile mlflow](./media/how-to-use-mlflow/azure-databricks-cluster-libraries.png)

**Paket** alanında azureml-mlflow yazın ve sonra yükle'yi tıklatın. Denemeniz için kümenize diğer ek paketleri yüklemek için gerektiğinde bu adımı yineleyin.

 ![masmavi makine öğrenme diyagramı ile mlflow](./media/how-to-use-mlflow/install-libraries.png)

### <a name="set-up-your-notebook-and-workspace"></a>Not defterinizi ve çalışma alanınızı ayarlama

Kümeniz kurulduktan sonra deneme not defterinizi içe aktarın, açın ve kümenizi ona takın.

Aşağıdaki kod deneme not defterinizde olmalıdır. Bu kod, çalışma alanınızı anında oluşturmak için Azure aboneliğinizin ayrıntılarını alır. Bu kod, varolan bir kaynak grubunuz ve Azure Machine Learning çalışma alanınız olduğunu varsayar, aksi takdirde [bunları oluşturabilirsiniz.](how-to-manage-workspace.md) 

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

#### <a name="connect-your-azure-databricks-and-azure-machine-learning-workspaces"></a>Azure Veri Tuğlalarınızı ve Azure Makine Öğrenimi çalışma alanlarınızı bağlayın

Azure [portalında,](https://ms.portal.azure.com)Azure Veri Tuğlaları (ADB) çalışma alanınızı yeni veya varolan bir Azure Machine Learning çalışma alanına bağlayabilirsiniz. Bunu yapmak için ADB çalışma alanınıza gidin ve sağ alttaki **Azure Machine Learning çalışma alanı** bağlantısını seçin. Çalışma alanlarınızı bağlamak, Deneme verilerinizi Azure Machine Learning çalışma alanında izlemenize olanak tanır. 

### <a name="link-mlflow-tracking-to-your-workspace"></a>MLflow izlemeyi çalışma alanınıza bağla

Çalışma alanınızı anında ayarladıktan sonra, URI'yi takip eden MLflow'u ayarlayın. Bunu yaparak, MLflow izlemeyi Azure Machine Learning çalışma alanına bağlarsınız. Bağlantı yaptıktan sonra, tüm denemeleriniz yönetilen Azure Machine Learning izleme hizmetine iner.

#### <a name="directly-set-mlflow-tracking-in-your-notebook"></a>Not defterinizde Doğrudan MLflow İzleme'yi ayarlayın

```python
uri = ws.get_mlflow_tracking_uri()
mlflow.set_tracking_uri(uri)
```

Eğitim komut dosyanızda, MLflow günlük API'lerini kullanmak için mlflow'u alın ve çalışma ölçümlerinizi günlüğe kaydetmeye başlayın. Aşağıdaki örnek, dönem kaybı metrik kaydeder. 

```python
import mlflow 
mlflow.log_metric('epoch_loss', loss.item()) 
```

#### <a name="automate-setting-mlflow-tracking"></a>Ayar MLflow İzleme'yi otomatikleştirin

Sonraki her deneme not defteri oturumunda izleme URI'sini kümelerinizde el ile ayarlamak yerine, bu [Azure Machine Learning Tracking Cluster Init komut dosyasını](https://github.com/Azure/MachineLearningNotebooks/blob/3ce779063b000e0670bdd1acc6bc3a4ee707ec13/how-to-use-azureml/azure-databricks/linking/README.md)otomatik olarak kullanarak bunu yapın.

Doğru şekilde yapılandırıldığında, MLflow izleme verilerinizi Azure Machine Learning REST API'sinde ve tüm istemcilerde ve Azure Databricks'te MLflow kullanıcı arabirimi üzerinden veya MLflow istemcisini kullanarak görebilirsiniz.

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Çalışma alanınızdaki ölçümleri ve yapıları görüntüleme

MLflow günlüğe kaydetmedeki ölçümler ve yapılar çalışma alanınızda tutulur. Bunları her zaman görüntülemek için çalışma alanınıza gidin ve [Azure Machine Learning stüdyosunda](https://ml.azure.com)çalışma alanınızda denemeyi ada göre bulun.  Veya aşağıdaki kodu çalıştırın. 

```python
run.get_metrics()
ws.get_details()
```

<!-- ## Deploy MLflow models as a web service

Deploying your MLflow experiments as an Azure Machine Learning web service allows you to leverage the Azure Machine Learning model management and data drift detection capabilities and apply them to your production models.

The following diagram demonstrates that with the MLflow deploy API you can deploy your existing MLflow models as an Azure Machine Learning web service, despite their frameworks--PyTorch, Tensorflow, scikit-learn, ONNX, etc., and manage your production models in your workspace.

![mlflow with azure machine learning diagram](./media/how-to-use-mlflow/mlflow-diagram-deploy.png)

### Log your model

Before you can deploy, be sure that your model is saved so you can reference it and its path location for deployment. In your training script, there should be code similar to the following [mlflow.sklearn.log_model()](https://www.mlflow.org/docs/latest/python_api/mlflow.sklearn.html) method, that saves your model to the specified outputs directory. 

```python
# change sklearn to pytorch, tensorflow, etc. based on your experiment's framework 
import mlflow.sklearn

# Save the model to the outputs directory for capture
mlflow.sklearn.log_model(regression_model, model_save_path)
```
>[!NOTE]
> Include the `conda_env` parameter to pass a dictionary representation of the dependencies and environment this model should be run in.

### Retrieve model from previous run

To retrieve the run, you need the run ID and the path in run history of where the model was saved. 

```python
# gets the list of runs for your experiment as an array
experiment_name = 'experiment-with-mlflow'
exp = ws.experiments[experiment_name]
runs = list(exp.get_runs())

# get the run ID and the path in run history
runid = runs[0].id
model_save_path = 'model'
```

### Create Docker image

The `mlflow.azureml.build_image()` function builds a Docker image from the saved model in a framework-aware manner. It automatically creates the framework-specific inferencing wrapper code and specifies package dependencies for you. Specify the model path, your workspace, run ID and other parameters.

The following code builds a docker image using *runs:/<run.id>/model* as the model_uri path for a Scikit-learn experiment.

```python
import mlflow.azureml

azure_image, azure_model = mlflow.azureml.build_image(model_uri='runs:/{}/{}'.format(runid, model_save_path),
                                                      workspace=ws,
                                                      model_name='sklearn-model',
                                                      image_name='sklearn-image',
                                                      synchronous=True)
```
The creation of the Docker image can take several minutes. 

### Deploy the Docker image 

After the image is created, use the Azure Machine Learning SDK to deploy the image as a web service.

First, specify the deployment configuration. Azure Container Instance (ACI) is a suitable choice for a quick dev-test deployment, while Azure Kubernetes Service (AKS) is suitable for scalable production deployments.

#### Deploy to ACI

Set up your deployment configuration with the [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) method. You can also add tags and descriptions to help keep track of your web service.

```python
from azureml.core.webservice import AciWebservice, Webservice

# Configure 
aci_config = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                memory_gb=1, 
                                                tags={'method' : 'sklearn'}, 
                                                description='Diabetes model',
                                                location='eastus2')
```

Then, deploy the image by using the Azure Machine Learning SDK [deploy_from_image()](/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none--overwrite-false-) method. 

```python
webservice = Webservice.deploy_from_image( image=azure_image, 
                                           workspace=ws, 
                                           name='diabetes-model-1', 
                                           deployment_config=aci_config)

webservice.wait_for_deployment(show_output=True)
```
#### Deploy to AKS

To deploy to AKS, first create an AKS cluster and bring over the Docker image you want to deploy. For this example, bring over the previously created image from the ACI deployment.

To get the image from the previous ACI deployment use the [Image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image.image?view=azure-ml-py) class. 

```python
from azureml.core.image import Image

# Get the image by name, you can change this based on the image you want to deploy
myimage = Image(workspace=ws, name='sklearn-image') 
```

Create an AKS cluster using the [ComputeTarget.create()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-) method. It may take 20-25 minutes to create a new cluster.

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
Set up your deployment configuration with the [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) method. You can also add tags and descriptions to help keep track of your web service.

```python
from azureml.core.webservice import Webservice, AksWebservice
from azureml.core.image import ContainerImage

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)

# Unique service name
service_name ='aks-service'
```

Then, deploy the image by using the Azure Machine Learning SDK [deploy_from_image()](/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none--overwrite-false-) method. 

```python
# Webservice creation using single command
aks_service = Webservice.deploy_from_image( workspace=ws, 
                                            name=service_name,
                                            deployment_config = aks_config
                                            image = myimage,
                                            deployment_target = aks_target)

aks_service.wait_for_deployment(show_output=True)
```

The service deployment can take several minutes.

## Clean up resources

If you don't plan to use the logged metrics and artifacts in your workspace, the ability to delete them individually is currently unavailable. Instead, delete the resource group that contains the storage account and workspace, so you don't incur any charges:

1. In the Azure portal, select **Resource groups** on the far left.

   ![Delete in the Azure portal](./media/how-to-use-mlflow/delete-resources.png)

1. From the list, select the resource group you created.

1. Select **Delete resource group**.

1. Enter the resource group name. Then select **Delete**.

 -->

 ## <a name="example-notebooks"></a>Örnek defterler

[Azure ML dizüstü bilgisayarlara sahip MLflow,](https://aka.ms/azureml-mlflow-examples) bu makalede sunulan kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar
* [Modellerinizi yönetin.](concept-model-management-and-deployment.md)
* [Veri kayması](how-to-monitor-data-drift.md)için üretim modellerinizi izleyin.
