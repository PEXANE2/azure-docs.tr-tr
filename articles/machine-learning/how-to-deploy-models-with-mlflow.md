---
title: MLflow ile ML denemeleri dağıtma
titleSuffix: Azure Machine Learning
description: ML modellerinizi bir Web hizmeti olarak dağıtmak için Azure Machine Learning ile MLflow ayarlayın.
services: machine-learning
author: shivp950
ms.author: shipatel
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.date: 12/23/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: b905b050752e2a6b7acd11e82420c0b0203dfcd1
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97882203"
---
# <a name="deploy-mlflow-models-with-azure-machine-learning-preview"></a>Azure Machine Learning ile MLflow modellerini dağıtma (Önizleme)

Bu makalede, [Mlflow](https://www.mlflow.org) modelinizi bir Azure Machine Learning Web hizmeti olarak dağıtmayı öğrenirsiniz. böylece, üretim modellerinize Azure Machine Learning model yönetimi ve veri DRI algılama yeteneklerini kullanabilir ve uygulayabilirsiniz.

Azure Machine Learning için dağıtım yapılandırması sunar:
* Hızlı geliştirme ve test dağıtımı için uygun bir seçim olan Azure Container Instance (acı).
* Ölçeklenebilir üretim dağıtımları için önerilen Azure Kubernetes hizmeti (AKS).

MLflow, Machine Learning denemeleri 'in yaşam döngüsünü yönetmeye yönelik açık kaynaklı bir kitaplıktır. Azure Machine Learning tümleştirmesi, bu yönetimi model eğitim aşamasından daha fazla üretim modelinizin dağıtım aşamasına genişletmenizi sağlar.

>[!NOTE]
> Açık kaynak kitaplığı olarak MLflow sık sık değişir. Bu nedenle, Azure Machine Learning ve MLflow tümleştirmesiyle sunulan işlevlerin Microsoft tarafından tam olarak desteklenmeden bir önizleme olarak değerlendirilmesi gerekir.

Aşağıdaki diyagramda, MLflow dağıtım API 'SI ve Azure Machine Learning ile birlikte, bilinen çerçevelerle oluşturulmuş modelleri (PyTorch, TensorFlow, scikit-öğren, vb. gibi), Azure Machine Learning Web Hizmetleri olarak ve çalışma alanınızda yönetebileceğiniz bir şekilde dağıtabilirsiniz. 

![ Azure Machine Learning ile mlflow modellerini dağıtma](./media/how-to-use-mlflow/mlflow-diagram-deploy.png)

> [!TIP]
> Bu belgedeki bilgiler, birincil olarak MLflow modellerini bir Azure Machine Learning Web hizmeti uç noktasına dağıtmak isteyen veri bilimcileri ve geliştiricileri içindir. Kotalar, tamamlanan eğitim çalıştırmaları veya tamamlanmış model dağıtımları gibi Azure Machine Learning kaynak kullanımını ve olayları izlemek isteyen bir yöneticiyseniz, bkz. [izleme Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="prerequisites"></a>Önkoşullar

* Machine Learning modeli. Eğitilen bir modeliniz yoksa, [Bu](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/using-mlflow) depodaki işlem senaryonuza en uygun olan Not defteri örneğini bulun ve talimatlarını izleyin. 
* [Azure Machine Learning bağlanmak Için MLflow Izleme URI 'Sini ayarlayın](how-to-use-mlflow.md#track-local-runs).
* `azureml-mlflow` paketini yükleyin. 
    * Bu paket `azureml-core` , çalışma alanınıza erişmek Için MLflow bağlantısını sağlayan [Azure Machine Learning Python SDK 'sını](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py)otomatik olarak getirir.
* [MLflow işlemlerinizi çalışma alanınız ile gerçekleştirmek için hangi erişim izinlerine ihtiyacınız](how-to-assign-roles.md#mlflow-operations)olduğunu görün. 

## <a name="deploy-to-aci"></a>ACI'ye dağıtma

MLflow modelinizi bir Azure Machine Learning Web hizmetine dağıtmak için, modelinizin [Azure Machine Learning bağlanmak üzere Mlflow Izleme URI](how-to-use-mlflow.md)'siyle ayarlanmalıdır. 

[Deploy_configuration ()](/python/api/azureml-core/azureml.core.webservice.aciwebservice?preserve-view=true&view=azure-ml-py#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) yöntemiyle dağıtım yapılandırmanızı ayarlayın. Web hizmetinizi izlemeye yardımcı olmak için Etiketler ve açıklamalar da ekleyebilirsiniz.

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

Daha sonra, Azure Machine Learning için MLflow 'un [Deploy](https://www.mlflow.org/docs/latest/python_api/mlflow.azureml.html#mlflow.azureml.deploy) yöntemiyle modeli tek bir adımda kaydedin ve dağıtın. 

```python
(webservice,model) = mlflow.azureml.deploy( model_uri='runs:/{}/{}'.format(run.id, model_path),
                      workspace=ws,
                      model_name='sklearn-model', 
                      service_name='diabetes-model-1', 
                      deployment_config=aci_config, 
                      tags=None, mlflow_home=None, synchronous=True)

webservice.wait_for_deployment(show_output=True)
```

## <a name="deploy-to-aks"></a>AKS’ye dağıtma

MLflow modelinizi bir Azure Machine Learning Web hizmetine dağıtmak için, modelinizin [Azure Machine Learning bağlanmak üzere Mlflow Izleme URI](how-to-use-mlflow.md)'siyle ayarlanmalıdır. 

AKS 'e dağıtmak için önce bir AKS kümesi oluşturun. [ComputeTarget. Create ()](/python/api/azureml-core/azureml.core.computetarget?preserve-view=true&view=azure-ml-py#&preserve-view=truecreate-workspace--name--provisioning-configuration-) yöntemini kullanarak bir aks kümesi oluşturun. Yeni bir küme oluşturmak 20-25 dakika sürebilir.

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
[Deploy_configuration ()](/python/api/azureml-core/azureml.core.webservice.aciwebservice?preserve-view=true&view=azure-ml-py#&preserve-view=truedeploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) yöntemiyle dağıtım yapılandırmanızı ayarlayın. Web hizmetinizi izlemeye yardımcı olmak için Etiketler ve açıklamalar da ekleyebilirsiniz.

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True, compute_target_name='aks-mlflow')

```

Daha sonra, Azure Machine Learning için MLflow 'un [Deploy](https://www.mlflow.org/docs/latest/python_api/mlflow.azureml.html#mlflow.azureml.deploy) yöntemiyle modeli tek bir adımda kaydedin ve dağıtın. 

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

Dağıtılmış Web hizmetinizi kullanmayı planlamıyorsanız, `service.delete()` bunu Not defterinizden silmek için kullanın.  Daha fazla bilgi için bkz. [WebService. Delete ()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truedelete--)belgeleri.

## <a name="example-notebooks"></a>Örnek not defterleri

[Azure Machine Learning Not defterlerindeki Mlflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/using-mlflow) , bu makalede sunulan kavramları gösterir ve genişletir.

> [!NOTE]
> Mlflow kullanarak bir topluluk odaklı örnek deposu şurada bulunabilir: https://github.com/Azure/azureml-examples .

## <a name="next-steps"></a>Sonraki adımlar

* [Modellerinizi yönetin](concept-model-management-and-deployment.md).
* [Veri kayması](./how-to-enable-data-collection.md)için üretim modellerinizi izleyin.
* [MLflow ile Azure Databricks çalıştırmalarını izleyin](how-to-use-mlflow-azure-databricks.md).

