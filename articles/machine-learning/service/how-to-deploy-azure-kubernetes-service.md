---
title: Azure Kubernetes hizmetine modeller dağıtma
titleSuffix: Azure Machine Learning service
description: Azure Kubernetes hizmetini kullanarak Azure Machine Learning hizmet modellerinizi Web hizmeti olarak dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/08/2019
ms.openlocfilehash: deb6482c0419a5872ccf86f0014adbecc7be6c9d
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68694398"
---
# <a name="deploy-a-model-to-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesine model dağıtma

Azure Kubernetes Service (AKS) üzerinde bir modeli Web hizmeti olarak dağıtmak için Azure Machine Learning hizmetini nasıl kullanacağınızı öğrenin. Azure Kubernetes hizmeti, yüksek ölçekli üretim dağıtımları için uygundur. Aşağıdaki özelliklerde bir veya daha fazlasına ihtiyacınız varsa Azure Kubernetes hizmetini kullanın:

- __Hızlı yanıt süresi__.
- Dağıtılan hizmetin __Otomatik ölçeklendirilmesi__ .
- GPU ve alan-programlanabilir kapı dizileri (FPGA) gibi __donanım hızlandırma__ seçenekleri.

> [!IMPORTANT]
> Azure Machine Learning SDK aracılığıyla cluter Ölçeklendirmesi sağlanmaz. Bir AKS kümesindeki düğümleri ölçeklendirme hakkında daha fazla bilgi için bkz. [AKS kümesindeki düğüm sayısını ölçeklendirme](../../aks/scale-cluster.md).

Azure Kubernetes hizmetine dağıtırken, __çalışma alanınıza bağlı__bir aks kümesine dağıtırsınız. Bir AKS kümesini çalışma alanınıza bağlamak için iki yol vardır:

* Azure Machine Learning Service SDK, Machine Learning CLı veya Azure portal kullanarak AKS kümesini oluşturun. Bu işlem, kümeyi otomatik olarak çalışma alanına bağlar.
* Azure Machine Learning hizmeti çalışma alanınıza mevcut bir AKS kümesi iliştirin. Bir küme Azure Machine Learning Service SDK, Machine Learning CLı veya Azure portal kullanılarak eklenebilir.

> [!IMPORTANT]
> Oluşturma veya ekleme işlemi bir kerelik görevdir. Bir AKS kümesi çalışma alanına bağlandıktan sonra dağıtım için kullanabilirsiniz. Artık gerekmiyorsa AKS kümesini ayırabilirsiniz veya silebilirsiniz. Detaalı veya silindikten sonra, artık kümeye dağıtım yapamayacaktır.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Machine Learning hizmeti çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning hizmet çalışma alanı oluşturma](setup-create-workspace.md).

- Bir Machine Learning modeli, çalışma alanınıza kaydedildi. Kayıtlı bir modeliniz yoksa, bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://aka.ms/aml-sdk)veya [Azure Machine Learning Visual Studio Code uzantısı](how-to-vscode-tools.md).

- Bu makaledeki __Python__ kod parçacıkları aşağıdaki değişkenlerin ayarlandığı varsayılır:

    * `ws`-Çalışma alanınıza ayarlayın.
    * `model`-Kayıtlı modelinize ayarlanır.
    * `inference_config`-Modelin çıkarım yapılandırmasına ayarlayın.

    Bu değişkenleri ayarlama hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

- Bu makaledeki __CLI__ kod parçacıkları bir `inferenceconfig.json` belge oluşturduğunuzu varsayar. Bu belgeyi oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

## <a name="create-a-new-aks-cluster"></a>Yeni bir AKS kümesi oluşturma

**Tahmini süre**: Yaklaşık 20 dakika.

AKS kümesi oluşturma veya iliştirme, çalışma alanınız için tek seferlik bir işlemdir. Bu kümeye birden çok dağıtımlar için yeniden kullanabilirsiniz. Kümeyi veya onu içeren kaynak grubunu silerseniz, bir dahaki sefer dağıtmanız gerektiğinde yeni bir küme oluşturmanız gerekir. Çalışma alanınıza eklenmiş birden çok AKS kümeniz olabilir.

Üretim yerine __geliştirme__, __doğrulama__ve __Test__ için bir aks kümesi oluşturmak istiyorsanız, geliştirme __testi__için __küme amacını__ belirtebilirsiniz.

Aşağıdaki örneklerde SDK ve CLı kullanarak yeni bir AKS kümesinin nasıl oluşturulacağı gösterilmektedir:

**SDK 'Yı kullanma**

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

> [!IMPORTANT]
> İçin [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), agent_count ve vm_size için özel değerler seçerseniz, vm_size ile çarpılan agent_count 'in 12 sanal CPU 'ya eşit veya daha büyük olduğundan emin olmanız gerekir. Örneğin, 4 sanal CPU içeren bir vm_size "Standard_D3_v2" kullanırsanız, bir agent_count 3 veya daha büyük seçmeniz gerekir.
>
> Azure Machine Learning SDK, bir AKS kümesini ölçeklendirmeye yönelik destek sağlamaz. Kümedeki düğümleri ölçeklendirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. Kümenin VM boyutunu değil, yalnızca düğüm sayısını değiştirebilirsiniz.

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AksCompute. Clusteramacını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute. provisioning_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [ComputeTarget. Create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-)
* [ComputeTarget.wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#wait-for-completion-show-output-false-)

**CLı 'yi kullanma**

```azurecli
az ml computetarget create aks -n myaks
```

Daha fazla bilgi için bkz. [az ml computetarget Create ask](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-aks) Reference.

## <a name="attach-an-existing-aks-cluster"></a>Mevcut bir AKS kümesini iliştirme

**Tahmini süre:** Yaklaşık 5 dakika.

Azure aboneliğinizde zaten AKS kümeniz varsa ve sürüm 1.12. # # ise, görüntünüzü dağıtmak için kullanabilirsiniz.

> [!TIP]
> Mevcut AKS kümesi, Azure Machine Learning hizmeti çalışma alanınızdan bir Azure bölgesinde olabilir.

> [!WARNING]
> Bir çalışma alanına aks kümesi eklerken, `cluster_purpose` parametresini ayarlayarak kümeyi nasıl kullanacağınızı tanımlayabilirsiniz.
>
> `cluster_purpose` Parametresini ayarlamayın veya ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, kümede en az 12 sanal CPU kullanılabilir olmalıdır.
>
> Ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, kümenin 12 sanal CPU 'ya sahip olması gerekmez. Ancak, geliştirme/test için yapılandırılan bir küme, üretim düzeyi trafiğe uygun olmayacaktır ve çıkarım sürelerini artırabilir.

Azure CLı veya portalını kullanarak bir AKS kümesi oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [AKS kümesi oluşturma (CLı)](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [AKS kümesi oluşturma (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Aşağıdaki örneklerde, var olan bir AKS 1.12. # # kümesini çalışma alanınıza nasıl ekleyebileceğiniz gösterilmektedir:

**SDK 'Yı kullanma**

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'myexistingcluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'myaks', attach_config)
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AksCompute. attach_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [AksCompute. Clusteramacını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute. Attach](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#attach-workspace--name--attach-configuration-)

**CLı 'yi kullanma**

CLı kullanarak var olan bir kümeyi eklemek için, var olan kümenin kaynak KIMLIĞINI almanız gerekir. Bu değeri almak için aşağıdaki komutu kullanın. Değerini `myexistingcluster` aks kümenizin adıyla değiştirin. Kümeyi `myresourcegroup` içeren kaynak grubuyla değiştirin:

```azurecli
az aks show -n myexistingcluster -g myresourcegroup --query id
```

Bu komut aşağıdaki metne benzer bir değer döndürür:

```text
/subscriptions/{GUID}/resourcegroups/{myresourcegroup}/providers/Microsoft.ContainerService/managedClusters/{myexistingcluster}
```

Mevcut kümeyi çalışma alanınıza eklemek için aşağıdaki komutu kullanın. Önceki `aksresourceid` komutun döndürdüğü değerle değiştirin. Çalışma `myresourcegroup` alanınızı içeren kaynak grubuyla değiştirin. Çalışma `myworkspace` alanınızın adıyla değiştirin.

```azurecli
az ml computetarget attach aks -n myaks -i aksresourceid -g myresourcegroup -w myworkspace
```

Daha fazla bilgi için, [az ml computetarget Attach aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks) başvurusuna bakın.

## <a name="deploy-to-aks"></a>AKS’ye dağıtma

Azure Kubernetes hizmetine bir model dağıtmak için, gereken işlem kaynaklarını açıklayan bir __dağıtım yapılandırması__ oluşturun. Örneğin, çekirdek ve bellek sayısı. Ayrıca, modeli ve Web hizmetini barındırmak için gereken ortamı açıklayan bir __çıkarım yapılandırmasına__ihtiyacınız vardır. Çıkarım yapılandırmasını oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

### <a name="using-the-sdk"></a>SDK’yı kullanarak

```python
aks_target = AksCompute(ws,"myaks")
# If deploying to a cluster configured for dev/test, ensure that it was created with enough
# cores and memory to handle this deployment configuration. Note that memory is also used by
# things such as dependencies and AML components.
deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config, aks_target)
service.wait_for_deployment(show_output = True)
print(service.state)
print(service.get_logs())
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AksCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute?view=azure-ml-py)
* [AksWebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)
* [Model. deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-)
* [WebService. wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#wait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>CLı 'yi kullanma

CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. Aks işlem hedefinin adıyla değiştirin `myaks` . Kayıt `mymodel:1` , kayıtlı modelin adı ve sürümü ile değiştirin. Bu `myservice` hizmete verilecek adla değiştirin:

```azurecli-interactive
az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aks-deploy-config.md)]

Daha fazla bilgi için, [az ml model dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) başvurusuna bakın. 

## <a name="using-vs-code"></a>VS Code kullanma

VS Code kullanımı hakkında bilgi için bkz. [vs Code uzantısı aracılığıyla AKS 'e dağıtma](how-to-vscode-tools.md#deploy-and-manage-models).

> [!IMPORTANT] 
> VS Code aracılığıyla dağıtmak, AKS kümesinin önceden oluşturulmasını veya çalışma alanınıza eklenmesini gerektirir.

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek

[!INCLUDE [aml-update-web-service](../../../includes/machine-learning-update-web-service.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
