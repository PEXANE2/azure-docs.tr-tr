---
title: Azure Kubernetes hizmetine modeller dağıtma
titleSuffix: Azure Machine Learning
description: Azure Kubernetes hizmetini kullanarak Azure Machine Learning modellerinizi bir Web hizmeti olarak dağıtmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 01/16/2020
ms.openlocfilehash: aec1b7f7bf60be34d21d52ca652a776cf3275fe8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80811757"
---
# <a name="deploy-a-model-to-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesine model dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Kubernetes Service (AKS) üzerinde bir modeli Web hizmeti olarak dağıtmak için Azure Machine Learning kullanmayı öğrenin. Azure Kubernetes hizmeti, yüksek ölçekli üretim dağıtımları için uygundur. Aşağıdaki özelliklerde bir veya daha fazlasına ihtiyacınız varsa Azure Kubernetes hizmetini kullanın:

- __Hızlı yanıt süresi__.
- Dağıtılan hizmetin __Otomatik ölçeklendirilmesi__ .
- GPU ve alan-programlanabilir kapı dizileri (FPGA) gibi __donanım hızlandırma__ seçenekleri.

> [!IMPORTANT]
> Küme ölçeklendirme, Azure Machine Learning SDK aracılığıyla sağlanmaz. Bir AKS kümesindeki düğümleri ölçeklendirme hakkında daha fazla bilgi için bkz. [AKS kümesindeki düğüm sayısını ölçeklendirme](../aks/scale-cluster.md).

Azure Kubernetes hizmetine dağıtırken, __çalışma alanınıza bağlı__bir aks kümesine dağıtırsınız. Bir AKS kümesini çalışma alanınıza bağlamak için iki yol vardır:

* Azure Machine Learning SDK, Machine Learning CLı veya [Azure Machine Learning Studio 'yu](https://ml.azure.com)kullanarak aks kümesini oluşturun. Bu işlem, kümeyi otomatik olarak çalışma alanına bağlar.
* Azure Machine Learning çalışma alanınıza mevcut bir AKS kümesi iliştirin. Bir küme Azure Machine Learning SDK, Machine Learning CLı veya Azure Machine Learning Studio kullanılarak iliştirilebilir.

> [!IMPORTANT]
> Oluşturma veya ekleme işlemi bir kerelik görevdir. Bir AKS kümesi çalışma alanına bağlandıktan sonra dağıtım için kullanabilirsiniz. Artık gerekmiyorsa AKS kümesini ayırabilirsiniz veya silebilirsiniz. Ayrıldıktan veya silindikten sonra artık kümeye dağıtım yapamayacaktır.

## <a name="prerequisites"></a>Ön koşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

- Bir Machine Learning modeli, çalışma alanınıza kaydedildi. Kayıtlı bir modeliniz yoksa, bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

- Bu makaledeki __Python__ kod parçacıkları aşağıdaki değişkenlerin ayarlandığı varsayılır:

    * `ws`-Çalışma alanınıza ayarlayın.
    * `model`-Kayıtlı modelinize ayarlanır.
    * `inference_config`-Modelin çıkarım yapılandırmasına ayarlayın.

    Bu değişkenleri ayarlama hakkında daha fazla bilgi için bkz. [modellerin nasıl ve ne şekilde dağıtılacağı](how-to-deploy-and-where.md).

- Bu makaledeki __CLI__ kod parçacıkları bir `inferenceconfig.json` belge oluşturduğunuzu varsayar. Bu belgeyi oluşturma hakkında daha fazla bilgi için bkz. [modellerin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md).

## <a name="create-a-new-aks-cluster"></a>Yeni bir AKS kümesi oluşturma

**Tahmini süre**: yaklaşık 20 dakika.

AKS kümesi oluşturma veya iliştirme, çalışma alanınız için tek seferlik bir işlemdir. Bu kümeyi birden çok dağıtım için yeniden kullanabilirsiniz. Kümeyi veya onu içeren kaynak grubunu silerseniz, bir dahaki sefer dağıtmanız gerektiğinde yeni bir küme oluşturmanız gerekir. Çalışma alanınıza eklenmiş birden çok AKS kümeniz olabilir.

> [!TIP]
> Azure sanal ağını kullanarak AKS kümenizi güvenli hale getirmek istiyorsanız, önce sanal ağı oluşturmanız gerekir. Daha fazla bilgi için bkz. [Azure sanal ağ Ile güvenli deneme ve çıkarım](how-to-enable-virtual-network.md#aksvnet).

Üretim yerine __geliştirme__, __doğrulama__ve __Test__ için bir aks kümesi oluşturmak istiyorsanız, geliştirme __testi__için __küme amacını__ belirtebilirsiniz.

> [!WARNING]
> Ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, oluşturulan küme üretim düzeyi trafik için uygun değildir ve çıkarım sürelerini artırabilir. Geliştirme ve test kümeleri de hata toleransı garantisi vermez. Geliştirme ve test kümeleri için en az 2 sanal CPU önerilir.

Aşağıdaki örneklerde SDK ve CLı kullanarak yeni bir AKS kümesinin nasıl oluşturulacağı gösterilmektedir:

**SDK’yı kullanarak**

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
> İçin [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) `agent_count` , ve `vm_size`için özel değerler seçerseniz ve `cluster_purpose` yoksa `DEV_TEST`, ile çarpılarak `agent_count` `vm_size` 12 sanal CPU 'ya eşit veya daha büyük olduğundan emin olmanız gerekir. Örneğin, 4 sanal CPU içeren bir `vm_size` "Standard_D3_v2" kullanıyorsanız, 3 veya daha büyük bir `agent_count` sürümü seçmelisiniz.
>
> Azure Machine Learning SDK, bir AKS kümesini ölçeklendirmeye yönelik destek sağlamaz. Kümedeki düğümleri ölçeklendirmek için Azure Machine Learning Studio 'daki AKS kümeniz için Kullanıcı arabirimini kullanın. Kümenin VM boyutunu değil, yalnızca düğüm sayısını değiştirebilirsiniz.

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AksCompute. Clusteramacını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute. provisioning_configuration](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [ComputeTarget. Create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-)
* [ComputeTarget. wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#wait-for-completion-show-output-false-)

**CLı 'yi kullanma**

```azurecli
az ml computetarget create aks -n myaks
```

Daha fazla bilgi için, [az ml computetarget Create aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-aks) Reference bölümüne bakın.

## <a name="attach-an-existing-aks-cluster"></a>Mevcut bir AKS kümesini iliştirme

**Tahmini süre:** Yaklaşık 5 dakika.

Azure aboneliğinizde zaten AKS kümeniz varsa ve sürüm 1,17 veya daha düşükse, görüntünüzü dağıtmak için kullanabilirsiniz.

> [!TIP]
> Mevcut AKS kümesi, Azure Machine Learning çalışma alanınızın dışında bir Azure bölgesinde olabilir.
>
> Azure sanal ağını kullanarak AKS kümenizi güvenli hale getirmek istiyorsanız, önce sanal ağı oluşturmanız gerekir. Daha fazla bilgi için bkz. [Azure sanal ağ Ile güvenli deneme ve çıkarım](how-to-enable-virtual-network.md#aksvnet).

Bir çalışma alanına AKS kümesi eklerken, `cluster_purpose` parametresini ayarlayarak kümeyi nasıl kullanacağınızı tanımlayabilirsiniz.

`cluster_purpose` Parametresini ayarlamayın veya ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, kümede en az 12 sanal CPU kullanılabilir olmalıdır.

Ayarlarsanız `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, kümenin 12 sanal CPU 'ya sahip olması gerekmez. Geliştirme ve test için en az 2 sanal CPU önerilir. Ancak, geliştirme/test için yapılandırılan bir küme, üretim düzeyi trafiğe uygun değildir ve çıkarım sürelerini artırabilir. Geliştirme ve test kümeleri de hata toleransı garantisi vermez.

> [!WARNING]
> Çalışma alanınızdan aynı AKS kümesine birden çok, eşzamanlı ek oluşturmayın. Örneğin, iki farklı ad kullanarak bir AKS kümesini çalışma alanına ekleme. Her yeni ek önceki mevcut ekleri keser.
>
> Bir AKS kümesini yeniden eklemek istiyorsanız (örneğin, TLS veya başka bir küme yapılandırma ayarını değiştirmek için), önce, bu eki,, [Akscompute. detach ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#detach--)kullanarak kaldırmanız gerekir.

Azure CLı veya portalını kullanarak bir AKS kümesi oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [AKS kümesi oluşturma (CLI)](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [AKS kümesi oluşturma (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Aşağıdaki örneklerde, var olan bir AKS kümesini çalışma alanınıza nasıl ekleyebileceğiniz gösterilmektedir:

**SDK’yı kullanarak**

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

* [AksCompute. attach_configuration ()](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
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
from azureml.core.webservice import AksWebservice, Webservice
from azureml.core.model import Model

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
* [Model. deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)
* [WebService. wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#wait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>CLı 'yi kullanma

CLı kullanarak dağıtmak için aşağıdaki komutu kullanın. AKS işlem hedefinin adıyla değiştirin `myaks` . Kayıt `mymodel:1` , kayıtlı modelin adı ve sürümü ile değiştirin. Bu `myservice` hizmete verilecek adla değiştirin:

```azurecli-interactive
az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

Daha fazla bilgi için, [az ml model dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) başvurusuna bakın.

### <a name="using-vs-code"></a>VS Code'u kullanma

VS Code kullanımı hakkında bilgi için bkz. [vs Code uzantısı aracılığıyla AKS 'e dağıtma](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model).

> [!IMPORTANT]
> VS Code aracılığıyla dağıtmak, AKS kümesinin önceden oluşturulmasını veya çalışma alanınıza eklenmesini gerektirir.

## <a name="deploy-models-to-aks-using-controlled-rollout-preview"></a>Denetimli dağıtımı kullanarak AKS 'e model dağıtma (Önizleme)

Uç noktaları kullanarak, denetimli bir biçimde model sürümlerini çözümleyin ve yükseltin. Tek bir uç noktanın arkasında altı adede kadar sürüm dağıtabilirsiniz. Uç noktalar aşağıdaki özellikleri sağlar:

* Her bir __uç noktaya gönderilen Puanlama trafiği yüzdesini__yapılandırın. Örneğin, trafiğin %20 ' sini ' test ' ve %80 ' i ' üretim ' noktasına yönlendirin.

    > [!NOTE]
    > Trafiğin %100 ' i için hesap yapmazsanız, kalan tüm yüzde __varsayılan__ uç nokta sürümüne yönlendirilir. Örneğin, ' test ' uç nokta sürümünü trafiğin %10 ' u ve %30 ' u için ' üretim ' olarak yapılandırırsanız, kalan %60, varsayılan uç nokta sürümüne gönderilir.
    >
    > Oluşturulan ilk uç nokta sürümü otomatik olarak varsayılan olarak yapılandırılır. Bu ayarı `is_default=True` , bir uç nokta sürümü oluştururken veya güncelleştirirken değiştirebilirsiniz.
     
* Bir uç nokta sürümünü __Denetim__ veya __işleme__olarak etiketleyin. Örneğin, geçerli üretim uç noktası sürümü denetim olabilir, ancak olası yeni modeller bir işleme sürümü olarak dağıtılır. İşlem sürümlerinin performansını değerlendirdikten sonra, bir çıktı geçerli denetimi gerçekleştirdiğinde, yeni üretime/denetime yükseltilebilir.

    > [!NOTE]
    > Yalnızca __bir__ denetiminiz olabilir. Birden çok anlaşma olabilir.

Uç noktaların ve dağıtılan sürümlerin işletimsel ölçümlerini görüntülemek için App Insights 'ı etkinleştirebilirsiniz.

### <a name="create-an-endpoint"></a>Uç nokta oluşturma
Modellerinizi dağıtmaya hazırsanız, bir Puanlama uç noktası oluşturun ve ilk sürümünüzü dağıtın. Aşağıdaki örnek, SDK kullanarak bitiş noktasının nasıl dağıtılacağını ve oluşturulacağını göstermektedir. İlk dağıtım varsayılan sürüm olarak tanımlanacak, yani tüm sürümlerde belirtilmeyen trafik yüzdelik değeri varsayılan sürüme gidecektir.  

> [!TIP]
> Aşağıdaki örnekte, yapılandırma ilk uç nokta sürümünü trafiğin %20 ' sini işleyecek şekilde ayarlar. Bu ilk uç nokta olduğundan, aynı zamanda varsayılan sürümdür. Trafiğin diğer %80 ' i için başka bir sürüm olmadığı için, bu, varsayılan olarak da yönlendirilir. Trafik yüzdesi alan diğer sürümler dağıtılana kadar bu, trafiğin %100 ' u etkili bir şekilde alır.

```python
import azureml.core,
from azureml.core.webservice import AksEndpoint
from azureml.core.compute import AksCompute
from azureml.core.compute import ComputeTarget
# select a created compute
compute = ComputeTarget(ws, 'myaks')
namespace_name= endpointnamespace
# define the endpoint and version name
endpoint_name = "mynewendpoint"
version_name= "versiona"
# create the deployment config and define the scoring traffic percentile for the first deployment
endpoint_deployment_config = AksEndpoint.deploy_configuration(cpu_cores = 0.1, memory_gb = 0.2,
                                                              enable_app_insights = True,
                                                              tags = {'sckitlearn':'demo'},
                                                              description = "testing versions",
                                                              version_name = version_name,
                                                              traffic_percentile = 20)
 # deploy the model and endpoint
 endpoint = Model.deploy(ws, endpoint_name, [model], inference_config, endpoint_deployment_config, compute)
 # Wait for he process to complete
 endpoint.wait_for_deployment(True)
 ```

### <a name="update-and-add-versions-to-an-endpoint"></a>Bir uç noktaya güncelleştirme ve sürüm ekleme

Uç noktanıza başka bir sürüm ekleyin ve Puanlama trafiği yüzdebirlik sürümünü sürüme göre yapılandırın. İki tür sürüm, bir denetim ve bir işleme sürümü vardır. Tek bir denetim sürümüne karşı karşılaştırmanıza yardımcı olmak için birden çok işleme sürümü olabilir.

> [!TIP]
> Aşağıdaki kod parçacığı tarafından oluşturulan ikinci sürüm, trafiğin %10 ' u kabul eder. İlk sürüm %20 için yapılandırılmıştır, bu nedenle trafiğin yalnızca %30 ' u belirli sürümler için yapılandırılmıştır. Kalan %70, aynı zamanda varsayılan sürüm olduğundan, ilk uç nokta sürümüne gönderilir.

 ```python
from azureml.core.webservice import AksEndpoint

# add another model deployment to the same endpoint as above
version_name_add = "versionb"
endpoint.create_version(version_name = version_name_add,
                        inference_config=inference_config,
                        models=[model],
                        tags = {'modelVersion':'b'},
                        description = "my second version",
                        traffic_percentile = 10)
endpoint.wait_for_deployment(True)
```

Mevcut sürümleri güncelleştirin veya bir uç noktada silin. Sürümün varsayılan türünü, denetim türünü ve trafik Yüzdeliğini değiştirebilirsiniz. Aşağıdaki örnekte, ikinci sürüm trafiği %40 olarak artırır ve artık varsayılandır.

> [!TIP]
> Aşağıdaki kod parçacığı sonrasında ikinci sürüm artık varsayılandır. Artık %40 için yapılandırılmıştır, ancak özgün sürüm %20 için yapılandırılmakta. Bu, trafiğin %40 ' un sürüm yapılandırmalarına göre hesaba katılmaz anlamına gelir. Artık varsayılan olduğu için soltover trafiği ikinci sürüme yönlendirilir. Trafiğin %80 ' ü etkili bir şekilde alır.

 ```python
from azureml.core.webservice import AksEndpoint

# update the version's scoring traffic percentage and if it is a default or control type
endpoint.update_version(version_name=endpoint.versions["versionb"].name,
                        description="my second version update",
                        traffic_percentile=40,
                        is_default=True,
                        is_control_version_type=True)
# Wait for the process to complete before deleting
endpoint.wait_for_deployment(true)
# delete a version in an endpoint
endpoint.delete_version(version_name="versionb")

```


## <a name="web-service-authentication"></a>Web hizmeti kimlik doğrulaması

Azure Kubernetes hizmetine dağıtım yaparken, __anahtar tabanlı__ kimlik doğrulaması varsayılan olarak etkindir. __Belirteç tabanlı__ kimlik doğrulamasını da etkinleştirebilirsiniz. Belirteç tabanlı kimlik doğrulaması, istemcilerin dağıtılan hizmette istek yapmak için kullanılan bir kimlik doğrulama belirteci istemek için bir Azure Active Directory hesabı kullanmasını gerektirir.

Kimlik doğrulamasını __devre dışı bırakmak__ için `auth_enabled=False` , dağıtım yapılandırmasını oluştururken parametresini ayarlayın. Aşağıdaki örnek, SDK 'Yı kullanarak kimlik doğrulamasını devre dışı bırakır:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, auth_enabled=False)
```

İstemci uygulamasından kimlik doğrulama hakkında daha fazla bilgi için, [Web hizmeti olarak dağıtılan Azure Machine Learning modelini](how-to-consume-web-service.md)kullanma konusuna bakın.

### <a name="authentication-with-keys"></a>Anahtarlar ile kimlik doğrulama

Anahtar kimlik doğrulaması etkinleştirilirse, birincil ve ikincil kimlik doğrulama `get_keys` anahtarını almak için yöntemini kullanabilirsiniz:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarı yeniden oluşturmanız gerekiyorsa, şunu kullanın[`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)

### <a name="authentication-with-tokens"></a>Belirteçlerle kimlik doğrulama

Belirteç kimlik doğrulamasını etkinleştirmek için, bir `token_auth_enabled=True` dağıtımı oluştururken veya güncelleştirirken parametreyi ayarlayın. Aşağıdaki örnek SDK kullanarak belirteç kimlik doğrulamasını mümkün bir şekilde sunar:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, token_auth_enabled=True)
```

Belirteç kimlik doğrulaması etkinleştirilirse, bir JWT belirteci almak için `get_token` yöntemini ve bu belirtecin sona erme süresini kullanabilirsiniz:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirtecin `refresh_by` zamanından sonra yeni bir belirteç istemeniz gerekir.
>
> Microsoft, Azure Machine Learning çalışma alanınızı Azure Kubernetes hizmet kümeniz ile aynı bölgede oluşturmanızı kesinlikle önerir. Bir belirteçle kimlik doğrulaması yapmak için Web hizmeti, Azure Machine Learning çalışma alanınızın oluşturulduğu bölgeye bir çağrı yapar. Çalışma alanınızın bölgesi kullanılamıyorsa, kümeniz çalışma alanınızdan farklı bir bölgedeyse, Web hizmetiniz için de bir belirteç getirimeyeceksiniz. Bu, çalışma alanınızın bölgesi yeniden kullanılabilir olana kadar belirteç tabanlı kimlik doğrulamanın kullanılamamasına neden olur. Ayrıca, kümenizin bölgesi ve çalışma alanınızın bölgesi arasındaki mesafe ne kadar fazlaysa bir belirteci getirmek için bu daha uzun sürer.

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirme

[!INCLUDE [aml-update-web-service](../../includes/machine-learning-update-web-service.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Bir sanal ağda güvenli deneme ve çıkarım](how-to-enable-virtual-network.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir ML modelini kullanma](how-to-consume-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
