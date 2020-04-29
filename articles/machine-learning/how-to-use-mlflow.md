---
title: ML denemeleri için MLflow Izleme
titleSuffix: Azure Machine Learning
description: Databricks kümelerinde, yerel ortamınızda veya VM ortamınızda oluşturulan ML modellerinden ölçümleri ve yapıtları günlüğe kaydetmek için Azure Machine Learning ile MLflow ayarlayın.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76983707"
---
# <a name="track-models-metrics-with-mlflow-and-azure-machine-learning-preview"></a>MLflow ve Azure Machine Learning (Önizleme) ile model ölçümlerini izleyin

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, mlflow denemeleri ve Azure Machine Learning bağlamak için mlflow izleme URI 'SI ve günlük API 'si ve [Mlflow izleme](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api)olarak bilinen günlüğe kaydetme API 'sinin nasıl etkinleştirileceği gösterilmektedir. Bunun yapılması, [Azure Machine Learning çalışma](https://docs.microsoft.com/azure/machine-learning/concept-azure-machine-learning-architecture#workspaces)alanınızdaki deneme ölçümlerini ve yapıtları izlemenize ve günlüğe almanıza olanak sağlar. Denemeleri için MLflow Izlemeyi zaten kullanıyorsanız, çalışma alanı eğitim ölçümlerini ve modellerini depolamak için merkezi, güvenli ve ölçeklenebilir bir konum sağlar.

<!--
+ Deploy your MLflow experiments as an Azure Machine Learning web service. By deploying as a web service, you can apply the Azure Machine Learning monitoring and data drift detection functionalities to your production models. 
-->

[Mlflow](https://www.mlflow.org) , Machine Learning denemeleri 'in yaşam döngüsünü yönetmeye yönelik açık kaynaklı bir kitaplıktır. MLFlow Izleme, bir MLflow bileşeni olan ve denemenizin ortamınız tarafından, uzak bir işlem hedefinde, bir sanal makinede veya Azure Databricks kümesinde yerel olarak, kendi ortamınızda olduğu gibi, eğitim çalıştırma ölçümleri ve model yapıtlarını kaydeder ve izler. 

Aşağıdaki diyagramda, MLflow Izlemenin yanı sıra, Azure Machine Learning çalışma alanınızda bir deneyin çalışma ölçümlerini ve mağaza modeli yapılarını izlersiniz.

![Azure Machine Learning diyagramı ile mlflow](./media/how-to-use-mlflow/mlflow-diagram-track.png)

> [!TIP]
> Bu belgedeki bilgiler öncelikli olarak, model eğitimi sürecini izlemek isteyen veri bilimcileri ve geliştiricileri içindir. Kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning kaynak kullanımını ve olayları izlemek isteyen bir yöneticiyseniz, bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>MLflow ve Azure Machine Learning istemcilerini karşılaştırın

 Aşağıdaki tabloda Azure Machine Learning kullanılabilecek farklı istemciler ve ilgili işlev özellikleri özetlenmektedir.

 MLflow Izleme yalnızca [Azure Machine Learning Python SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)aracılığıyla kullanılabilen ölçüm günlüğü ve yapıt depolama işlevleri sunar.


| | MLflow&nbsp;izleme <!--& Deployment--> | Python SDK Azure Machine Learning |  Azure Machine Learning CLı | Azure Machine Learning Studio|
|---|---|---|---|---|
| Çalışma alanını yönet |   | ✓ | ✓ | ✓ |
| Veri depolarını kullanma  |   | ✓ | ✓ | |
| Günlük ölçümleri      | ✓ | ✓ |   | |
| Yapıtları karşıya yükle | ✓ | ✓ |   | |
| Ölçümleri görüntüle     | ✓ | ✓ | ✓ | ✓ |
| İşlemi yönetme   |   | ✓ | ✓ | ✓ |

<!--| Deploy models    | ✓ | ✓ | ✓ | ✓ |
|Monitor model performance||✓|  |   |
| Detect data drift |   | ✓ |   | ✓ |
-->
## <a name="prerequisites"></a>Ön koşullar

* [MLflow 'ı yükler.](https://mlflow.org/docs/latest/quickstart.html)
* [Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) yerel BILGISAYARıNıZA yüklediğinizde SDK, çalışma alanınıza erişmek Için mlflow bağlantısını sağlar.
* [Azure Machine Learning çalışma alanı oluşturun](how-to-manage-workspace.md).

## <a name="track-local-runs"></a>Yerel çalıştırmaları izle

Azure Machine Learning ile MLflow Izleme, yerel çalıştırmanıza ait günlüğe kaydedilen ölçümleri ve yapıtları Azure Machine Learning çalışma alanınıza depolamanıza olanak sağlar.

Denemeleri veya `azureml-mlflow` kod düzenleyicisinde yerel olarak Jupyter Notebook çalıştırdığınız Azure Machine Learning Ile Mlflow izlemeyi kullanmak için paketini yükler.

```shell
pip install azureml-mlflow
```

>[!NOTE]
>Hizmetin geliştirilmesi için çalışdığımız için, azureml. contrib ad alanı sıklıkla değişir. Bu nedenle, bu ad alanındaki her şey Microsoft tarafından tam olarak desteklenmez ve önizleme olarak değerlendirilmelidir.

MLflow `mlflow` 'un [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) izleme URI 'sine erişmek ve çalışma alanınızı yapılandırmak için ve sınıflarını içeri aktarın.

Aşağıdaki kodda `get_mlflow_tracking_uri()` yöntemi, çalışma alanına `ws`benzersiz bir izleme URI adresi atar ve `set_tracking_uri()` mlflow izleme URI 'sini bu adrese yönlendirir.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>İzleme URI 'SI saat veya daha az bir saate kadar geçerlidir. Bir boşta kalma zamanından sonra betiğinizi yeniden başlatırsanız, yeni bir URI almak için get_mlflow_tracking_uri API 'sini kullanın.

MLflow deney adını ile `set_experiment()` ayarlayın ve öğreticinizi ile `start_run()`çalıştırın. Daha sonra `log_metric()` mlflow günlüğü API 'sini etkinleştirmek ve eğitim çalıştırma ölçümlerinizi günlüğe kaydetmeye başlamak için kullanın.

```Python
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Uzak çalıştırmaları izle

Azure Machine Learning ile MLflow Izleme, uzak çalıştırmanıza ait günlüğe kaydedilen ölçümleri ve yapıtları Azure Machine Learning çalışma alanınıza depolamanıza olanak sağlar.

Uzak çalıştırmalar, modellerinizi GPU etkin sanal makineler veya Machine Learning İşlem kümeler gibi daha güçlü bir şekilde bir şekilde eğitmenizi sağlar. Farklı işlem seçenekleri hakkında bilgi edinmek için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md) .

İşlem ve eğitim çalıştırma ortamınızı [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) sınıfıyla yapılandırın. Ortamın `mlflow` [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) bölümüne `azureml-mlflow` ekleme ve PIP paketleri. Sonra işlem [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) hedefi olarak uzak işlem ile oluşturun.

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

Eğitim betiğinizdeki MLflow `mlflow` günlüğü API 'lerini kullanmak için içeri aktarın ve çalıştırma ölçümlerinizi günlüğe kaydetmeye başlayın.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric('example', 1.23)
```

Bu işlem ve eğitim çalıştırma yapılandırmasıyla, bir çalıştırma göndermek `Experiment.submit('train.py')` için yöntemini kullanın. Bu yöntem, MLflow izleme URI 'sini otomatik olarak ayarlar ve MLflow oturumunu çalışma alanınıza yönlendirir.

```Python
run = exp.submit(src)
```

## <a name="track-azure-databricks-runs"></a>Azure Databricks çalıştırmalarını izleyin

Azure Machine Learning ile MLflow Izleme, Azure Machine Learning çalışma alanınızda Azure Databricks çalıştırınızdan günlüğe kaydedilen ölçümleri ve yapıtları depolamanıza olanak tanır.

Mlflow denemeleri Azure Databricks çalıştırmak için öncelikle bir [Azure Databricks çalışma alanı ve küme](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturmanız gerekir. Kümenizde, kümenizin gerekli işlevlere ve sınıflara erişebildiğinden emin olmak için, PyPI 'den *azureml-mlflow* kitaplığını yüklediğinizden emin olun.

Buradan, deneme not defterinizi içeri aktarın, Azure Databricks kümenize ekleyin ve denemenizi çalıştırın. 

### <a name="install-libraries"></a>Kitaplıkları yükler

Kümenizi kümenize yüklemek için **Kitaplıklar** sekmesine gidin ve **Yeni** ' ye tıklayın

 ![Azure Machine Learning diyagramı ile mlflow](./media/how-to-use-mlflow/azure-databricks-cluster-libraries.png)

**Paket** alanına, azureml-mlflow yazın ve ardından Install ' a tıklayın. Bu adımı, denemeniz için kümenize diğer ek paketleri yüklemek için gereken şekilde tekrarlayın.

 ![Azure Machine Learning diyagramı ile mlflow](./media/how-to-use-mlflow/install-libraries.png)

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

#### <a name="connect-your-azure-databricks-and-azure-machine-learning-workspaces"></a>Azure Databricks ve Azure Machine Learning çalışma alanlarınızı bağlama

[Azure Portal](https://ms.portal.azure.com), Azure Databricks (ADB) çalışma alanınızı yeni veya mevcut bir Azure Machine Learning çalışma alanına bağlayabilirsiniz. Bunu yapmak için ADB çalışma alanınıza gidin ve sağ alt köşedeki **Azure Machine Learning çalışma alanını bağla** düğmesini seçin. Çalışma alanlarınızı bağlamak, deneme verilerinizi Azure Machine Learning çalışma alanında izlemenize olanak sağlar. 

### <a name="link-mlflow-tracking-to-your-workspace"></a>MLflow izlemeyi çalışma alanınıza bağlama

Çalışma alanınızı örnekledikten sonra MLflow izleme URI 'sini ayarlayın. Bunu yaparak, MLflow izlemesini Azure Machine Learning çalışma alanına bağlarsınız. Bağlandıktan sonra, tüm denemeleri yönetilen Azure Machine Learning izleme hizmetine eklenecektir.

#### <a name="directly-set-mlflow-tracking-in-your-notebook"></a>Not defterinizde MLflow Izlemeyi doğrudan ayarlama

```python
uri = ws.get_mlflow_tracking_uri()
mlflow.set_tracking_uri(uri)
```

Eğitim betiğinizdeki mlflow günlüğü API 'Lerini kullanmak için mlflow 'u içeri aktarın ve çalıştırma ölçümlerinizi günlüğe kaydetmeye başlayın. Aşağıdaki örnek, dönem kayıp ölçümünü günlüğe kaydeder. 

```python
import mlflow 
mlflow.log_metric('epoch_loss', loss.item()) 
```

#### <a name="automate-setting-mlflow-tracking"></a>MLflow Izleme ayarını otomatikleştirin

Kümelerinizde sonraki tüm denemeler Not defteri oturumunda izleme URI 'sini el ile ayarlamak yerine bu [Azure Machine Learning Izleme kümesi Init betiğini](https://github.com/Azure/MachineLearningNotebooks/blob/3ce779063b000e0670bdd1acc6bc3a4ee707ec13/how-to-use-azureml/azure-databricks/linking/README.md)kullanarak otomatik olarak bunu yapın.

Doğru yapılandırıldığında, MLflow izleme verilerinizi Azure Machine Learning REST API ve tüm istemcilerde ve Azure Databricks ' de MLflow Kullanıcı arabirimi aracılığıyla veya MLflow istemcisini kullanarak görebileceksiniz.

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Çalışma alanınızdaki ölçümleri ve yapıtları görüntüleme

MLflow günlüğü 'ndeki ölçümler ve yapıtlar çalışma alanınızda tutulur. Bunları dilediğiniz zaman görüntülemek için, çalışma alanınıza gidin ve [Azure Machine Learning Studio](https://ml.azure.com)'daki çalışma alanınızda ada göre deneyin ' i bulun.  Veya aşağıdaki kodu çalıştırın. 

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

 ## <a name="example-notebooks"></a>Örnek Not defterleri

[Azure ML Not defterleri Ile Mlflow](https://aka.ms/azureml-mlflow-examples) , bu makalede sunulan kavramları gösterir ve genişletir.

## <a name="next-steps"></a>Sonraki adımlar
* [Modellerinizi yönetin](concept-model-management-and-deployment.md).
* [Veri kayması](how-to-monitor-data-drift.md)için üretim modellerinizi izleyin.
