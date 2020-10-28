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
ms.openlocfilehash: a81e60e3bb7a1b0f34a29ccd9cebf3d82279027e
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92676653"
---
# <a name="track-experiment-runs-and-deploy-ml-models-with-mlflow-and-azure-machine-learning-preview"></a>Deneme çalışmalarını izleyin ve ML modellerini MLflow ve Azure Machine Learning (Önizleme) ile dağıtın

Bu makalede, mlflow [izleme](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api)olarak bilinen MLFLOW izleme URI 'sini ve günlüğe kaydetme API 'sini etkinleştirmeyi ve Azure Machine Learning mlflow denemeleri 'ın arka ucu olarak nasıl bağlanacağınızı öğrenin. 

Desteklenen yetenekler şunlardır: 

+ [Azure Machine Learning çalışma](https://docs.microsoft.com/azure/machine-learning/concept-azure-machine-learning-architecture#workspaces)alanınızdaki deneme ölçümlerini ve yapıtları izleyin ve günlüğe kaydedin. Denemeleri için MLflow Izlemeyi zaten kullanıyorsanız, çalışma alanı eğitim ölçümlerini ve modellerini depolamak için merkezi, güvenli ve ölçeklenebilir bir konum sağlar.

+ Azure Machine Learning arka uç desteğiyle (Önizleme) MLflow projeleriyle eğitim işleri gönderebilirsiniz. İşleri Azure Machine Learning izlemeye yerel olarak gönderebilir veya yürütmeleri [Azure Machine Learning bir işlem](https://docs.microsoft.com/azure/machine-learning/how-to-create-attach-compute-sdk#amlcompute)aracılığıyla buluta geçirebilirsiniz.

+ MLflow ve Azure Machine Learning modeli kayıt defterindeki modelleri izleyin ve yönetin.

+ MLflow denemeleri 'nizi Azure Machine Learning Web hizmeti olarak dağıtın. Web hizmeti olarak dağıtarak, Azure Machine Learning izleme ve veri DRI algılama işlevlerini üretim modellerinize uygulayabilirsiniz. 

[Mlflow](https://www.mlflow.org) , Machine Learning denemeleri 'in yaşam döngüsünü yönetmeye yönelik açık kaynaklı bir kitaplıktır. MLFlow Izleme, bir MLflow bileşeni olan ve denemenizin ortamınız tarafından, uzak bir işlem hedefinde, bir sanal makinede veya [Azure Databricks kümesinde](how-to-use-mlflow-azure-databricks.md)yerel olarak, kendi ortamınızda olduğu gibi, eğitim çalıştırma ölçümleri ve model yapıtlarını kaydeder ve izler. 

>[!NOTE]
> Açık kaynak kitaplığı olarak MLflow sık sık değişir. Bu nedenle, Azure Machine Learning ve MLflow tümleştirmesiyle sunulan işlevlerin Microsoft tarafından tam olarak desteklenmeden bir önizleme olarak değerlendirilmesi gerekir.

Aşağıdaki diyagramda, MLflow Izlemenin yanı sıra, Azure Machine Learning çalışma alanınızda bir deneyin çalışma ölçümlerini ve mağaza modeli yapılarını izlersiniz.

![Azure Machine Learning diyagramı ile mlflow](./media/how-to-use-mlflow/mlflow-diagram-track.png)

> [!TIP]
> Bu belgedeki bilgiler öncelikli olarak, model eğitimi sürecini izlemek isteyen veri bilimcileri ve geliştiricileri içindir. Kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning kaynak kullanımını ve olayları izlemek isteyen bir yöneticiyseniz, bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>MLflow ve Azure Machine Learning istemcilerini karşılaştırın

 Aşağıdaki tabloda Azure Machine Learning kullanılabilecek farklı istemciler ve ilgili işlev özellikleri özetlenmektedir.

 MLflow Izleme yalnızca [Azure Machine Learning Python SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)aracılığıyla kullanılabilen ölçüm günlüğü ve yapıt depolama işlevleri sunar.

| Yetenek | MLflow Izleme & dağıtımı | Python SDK Azure Machine Learning |  Azure Machine Learning CLI | Azure Machine Learning Studio|
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

## <a name="prerequisites"></a>Ön koşullar

* `azureml-mlflow` paketini yükleyin. 
    * Bu paket `azureml-core` , çalışma alanınıza erişmek Için MLflow bağlantısını sağlayan [Azure Machine Learning Python SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true)otomatik olarak getirir.
* [Azure Machine Learning çalışma alanı oluşturun](how-to-manage-workspace.md).

## <a name="track-local-runs"></a>Yerel çalıştırmaları izle

Azure Machine Learning ile MLflow Izleme, yerel çalıştırmanıza ait günlüğe kaydedilen ölçümleri ve yapıtları Azure Machine Learning çalışma alanınıza depolamanıza olanak sağlar.

`mlflow` [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py&preserve-view=true) Mlflow 'un izleme URI 'sine erişmek ve çalışma alanınızı yapılandırmak için ve sınıflarını içeri aktarın.

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

Uzak çalıştırmalar, modellerinizi GPU etkin sanal makineler veya Machine Learning İşlem kümeler gibi daha güçlü bir şekilde bir şekilde eğitmenizi sağlar. Farklı işlem seçenekleri hakkında bilgi edinmek için bkz. [model eğitimi için işlem hedeflerini kullanma](how-to-set-up-training-targets.md) .

Azure Machine Learning ile MLflow Izleme, uzak çalıştırmanıza ait günlüğe kaydedilen ölçümleri ve yapıtları Azure Machine Learning çalışma alanınıza depolamanıza olanak sağlar. İçindeki MLflow Izleme kodu ile çalıştırılan tüm çalışan, çalışma alanında otomatik olarak günlüğe kaydedilecek ölçümleri olacaktır. 

Aşağıdaki örnek Conda ortamı, `mlflow` `azureml-mlflow` PIP paketleri içerir. 


```yaml
name: sklearn-example
dependencies:
  - python=3.6.2
  - scikit-learn
  - matplotlib
  - numpy
  - pip:
    - azureml-mlflow
    - numpy
```

Betiğinizdeki işlem ve eğitim çalıştırma ortamınızı [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true) sınıfıyla yapılandırın. Sonra,  [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py&preserve-view=true) işlem hedefi olarak uzak işlem ile oluşturun.

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

`mlflow` [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29?view=azure-ml-py&preserve-view=true) Mlflow 'un izleme URI 'sine erişmek ve çalışma alanınızı yapılandırmak için ve sınıflarını içeri aktarın.

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

    ![Register-mlflow-model](./media/how-to-use-mlflow/registered-mlflow-model.png)

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

[Deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) yöntemiyle dağıtım yapılandırmanızı ayarlayın. Web hizmetinizi izlemeye yardımcı olmak için Etiketler ve açıklamalar da ekleyebilirsiniz.

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

Ardından, Azure Machine Learning SDK [Deploy](/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) yöntemiyle modeli tek bir adımda kaydedin ve dağıtın. 

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

AKS 'e dağıtmak için önce bir AKS kümesi oluşturun. [ComputeTarget. Create ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py&preserve-view=true#&preserve-view=truecreate-workspace--name--provisioning-configuration-) yöntemini kullanarak bir aks kümesi oluşturun. Yeni bir küme oluşturmak 20-25 dakika sürebilir.

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
[Deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) yöntemiyle dağıtım yapılandırmanızı ayarlayın. Web hizmetinizi izlemeye yardımcı olmak için Etiketler ve açıklamalar da ekleyebilirsiniz.

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True, compute_target_name='aks-mlflow')

```

Ardından, modeli Azure Machine Learning SDK [Deploy ()] ile tek bir adımda kaydedip dağıtın (sonra, modeli Azure Machine Learning SDK [Deploy](/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py&preserve-view=true#&preserve-view=truedeploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) metodunu kullanarak kaydedin ve dağıtın. 

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

1. **Kaynak grubunu sil** 'i seçin.

1. Kaynak grubu adını girin. Ardından **Sil** ’i seçin.

## <a name="example-notebooks"></a>Örnek not defterleri

[Azure ML Not defterleri Ile Mlflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/track-and-monitor-experiments/using-mlflow) , bu makalede sunulan kavramları gösterir ve genişletir.

> [!NOTE]
> Mlflow kullanarak bir topluluk odaklı örnek deposu şurada bulunabilir: https://github.com/Azure/azureml-examples .

## <a name="next-steps"></a>Sonraki adımlar

* [Modellerinizi yönetin](concept-model-management-and-deployment.md).
* [Veri kayması](how-to-monitor-data-drift.md)için üretim modellerinizi izleyin.
* [MLflow ile Azure Databricks çalıştırmalarını izleyin](how-to-use-mlflow-azure-databricks.md). 
