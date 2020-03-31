---
title: Modelleri Azure Kubernetes Hizmetine dağıtma
titleSuffix: Azure Machine Learning
description: Azure Kubernetes Hizmetini kullanarak Azure Machine Learning modellerinizi bir web hizmeti olarak nasıl dağıtılayarak dağıtılamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 01/16/2020
ms.openlocfilehash: db2e80ebb6cbe5f31f2d99a1403a15daf38fd877
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722416"
---
# <a name="deploy-a-model-to-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesine model dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Kubernetes Hizmeti'nde (AKS) bir modeli web hizmeti olarak dağıtmak için Azure Machine Learning'i nasıl kullanacağınızı öğrenin. Azure Kubernetes Hizmeti, yüksek ölçekli üretim dağıtımları için iyidir. Aşağıdaki özelliklerden birine veya daha fazlasına ihtiyacınız varsa Azure Kubernetes hizmetini kullanın:

- __Hızlı tepki süresi__.
- Dağıtılan hizmetin __otomatik ölçekletilmesi.__
- GPU ve alan programlanabilir geçit dizileri (FPGA) gibi __donanım hızlandırma__ seçenekleri.

> [!IMPORTANT]
> Azure Machine Learning SDK aracılığıyla küme ölçekleme sağlanmaz. Bir AKS kümesindeki düğümleri ölçeklendirme hakkında daha fazla bilgi için, [aks kümesindeki düğüm sayısını ölçeklendir'e](../aks/scale-cluster.md)bakın.

Azure Kubernetes Hizmetine dağıtırken, çalışma alanınıza bağlı bir AKS __kümesine dağıtırsınız.__ Bir AKS kümesini çalışma alanınıza bağlamanın iki yolu vardır:

* Azure Machine Learning SDK, Machine Learning CLI veya [Azure Machine Learning stüdyosunu](https://ml.azure.com)kullanarak AKS kümesini oluşturun. Bu işlem kümeyi otomatik olarak çalışma alanına bağlar.
* Azure Machine Learning çalışma alanınıza varolan bir AKS kümesini takın. Bir küme, Azure Machine Learning SDK, Machine Learning CLI veya Azure Machine Learning stüdyosu kullanılarak eklenebilir.

> [!IMPORTANT]
> Oluşturma veya eki işlemi tek seferlik bir görevdir. Bir AKS kümesi çalışma alanına bağlandıktan sonra, onu dağıtımlar için kullanabilirsiniz. Artık ihtiyacınız yoksa AKS kümesini ayırabilir veya silebilirsiniz. Bir kez ayrılmış veya silinmiş, artık kümeye dağıtmak mümkün olmayacaktır.

## <a name="prerequisites"></a>Ön koşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için [bkz.](how-to-manage-workspace.md)

- Çalışma alanınızda kayıtlı bir makine öğrenme modeli. Kayıtlı bir modeliniz yoksa, [modelleri nasıl ve nerede dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.

- [Machine Learning hizmeti için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), Azure Machine Learning Python [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

- Bu makaledeki __Python__ kod parçacıkları aşağıdaki değişkenlerin ayarlı olduğunu varsayar:

    * `ws`- Çalışma alanınıza ayarlayın.
    * `model`- Kayıtlı modelinize ayarlayın.
    * `inference_config`- Modelin çıkarım yapılandırmasına ayarlayın.

    Bu değişkenleri ayarlama hakkında daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağınız](how-to-deploy-and-where.md)hakkında bilgi alabiliyorum.

- Bu makaledeki __CLI__ parçacıkları bir `inferenceconfig.json` belge oluşturduğunuzu varsayar. Bu belgeyi oluşturma hakkında daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağınız](how-to-deploy-and-where.md)hakkında bilgi alabiliyorum.

## <a name="create-a-new-aks-cluster"></a>Yeni bir AKS kümesi oluşturma

**Zaman tahmini**: Yaklaşık 20 dakika.

AKS kümesi oluşturma veya iliştirme, çalışma alanınız için tek seferlik bir işlemdir. Bu kümeyi birden çok dağıtım için yeniden kullanabilirsiniz. Kümeyi veya onu içeren kaynak grubunu silerseniz, bir sonraki dağıtım yapmanız gereken yeni bir küme oluşturmanız gerekir. Çalışma alanınıza birden çok AKS kümesi eklenmiş olabilir.

> [!TIP]
> Aks kümenizi azure sanal ağ kullanarak güvenli hale getirmek istiyorsanız, önce sanal ağı oluşturmanız gerekir. Daha fazla bilgi için Azure [Sanal Ağı ile Güvenli deneme ve çıkarım](how-to-enable-virtual-network.md#aksvnet)alameti'ne bakın.

Üretim yerine __geliştirme,__ __doğrulama__ve __sınama__ için bir AKS kümesi oluşturmak istiyorsanız, __geliştirme testi__için küme __amacını__ belirtebilirsiniz.

> [!WARNING]
> Ayarlarsanız, `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`oluşturulan küme üretim düzeyi trafiği için uygun değildir ve çıkarım sürelerini artırabilir. Dev/test kümeleri de hata toleransı garanti etmez. Dev/test kümeleri için en az 2 sanal CPU öneririz.

Aşağıdaki örnekler, SDK ve CLI kullanarak yeni bir AKS kümesinin nasıl oluşturulacak larını göstermektedir:

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
> Bunun [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) `agent_count` için, özel değerleri `vm_size`seçerseniz `cluster_purpose` ve `DEV_TEST`, değilse , `agent_count` o zaman `vm_size` çarpıldığından emin olmanız gerekir 12 sanal CPU'dan daha büyük veya eşittir. Örneğin, 4 sanal `vm_size` CPU'su olan "Standard_D3_v2" kullanıyorsanız, 3 veya `agent_count` daha büyük bir alan seçmengerekir.
>
> Azure Machine Learning SDK, bir AKS kümesini ölçekleme desteği sağlamaz. Kümedeki düğümleri ölçeklendirmek için Azure Machine Learning stüdyosunda AKS kümeniz için UI'yi kullanın. Kümenin VM boyutunu değil, yalnızca düğüm sayısını değiştirebilirsiniz.

Bu örnekte kullanılan sınıflar, yöntemler ve parametreler hakkında daha fazla bilgi için aşağıdaki başvuru belgelerine bakın:

* [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute.provisioning_configuration](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [ComputeTarget.create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-)
* [ComputeTarget.wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#wait-for-completion-show-output-false-)

**CLI'yi kullanma**

```azurecli
az ml computetarget create aks -n myaks
```

Daha fazla bilgi için [aks referansı oluşturmak az ml computetarget](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-aks) bakın.

## <a name="attach-an-existing-aks-cluster"></a>Varolan bir AKS kümesini ekleme

**Zaman tahmini:** Yaklaşık 5 dakika.

Azure aboneliğinizde zaten AKS kümesi varsa ve sürüm 1.17 veya daha düşükse, resminizi dağıtmak için kullanabilirsiniz.

> [!TIP]
> Varolan AKS kümesi, Azure Machine Learning çalışma alanınız dışında bir Azure bölgesinde olabilir.
>
> Aks kümenizi azure sanal ağ kullanarak güvenli hale getirmek istiyorsanız, önce sanal ağı oluşturmanız gerekir. Daha fazla bilgi için Azure [Sanal Ağı ile Güvenli deneme ve çıkarım](how-to-enable-virtual-network.md#aksvnet)alameti'ne bakın.

Bir çalışma alanına AKS kümesi iliştirirken, parametreyi ayarlayarak `cluster_purpose` kümeyi nasıl kullanacağınızı tanımlayabilirsiniz.

Parametreyi `cluster_purpose` ayarlamaz veya kümelemezse, `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`kümenin en az 12 sanal CPU'su olmalıdır.

Ayarlarsanız, `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`kümenin 12 sanal CPU'ya sahip olması gerekmez. Biz dev / test için en az 2 sanal CPU öneririz. Ancak dev/test için yapılandırılan bir küme üretim düzeyi trafiği için uygun değildir ve çıkarım sürelerini artırabilir. Dev/test kümeleri de hata toleransı garanti etmez.

> [!WARNING]
> Çalışma alanınızdan aynı AKS kümesine birden çok, eşzamanlı ek oluşturmayın. Örneğin, iki farklı ad kullanarak bir AKS kümesini çalışma alanına ekleme. Her yeni ek, önceki varolan eki(ler) kırar.
>
> Bir AKS kümesini yeniden takmak istiyorsanız, örneğin SSL veya diğer küme yapılandırma ayarını değiştirmek için, önce [AksCompute.detach()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#detach--)kullanarak varolan eki kaldırmanız gerekir.

Azure CLI veya portalını kullanarak AKS kümesi oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [AKS kümesi oluşturma (CLI)](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [AKS kümesi (portal) oluşturma](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Aşağıdaki örnekler, varolan bir AKS kümesinin çalışma alanınıza nasıl eklenir olduğunu gösterir:

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

Bu örnekte kullanılan sınıflar, yöntemler ve parametreler hakkında daha fazla bilgi için aşağıdaki başvuru belgelerine bakın:

* [AksCompute.attach_configuration()](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute.attach](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#attach-workspace--name--attach-configuration-)

**CLI'yi kullanma**

CLI'yi kullanarak varolan bir küme eklemek için varolan kümenin kaynak kimliğini almanız gerekir. Bu değeri elde etmek için aşağıdaki komutu kullanın. AKS kümenizin adıyla değiştirin. `myexistingcluster` Kümeiçeren kaynak grubuyla değiştirin: `myresourcegroup`

```azurecli
az aks show -n myexistingcluster -g myresourcegroup --query id
```

Bu komut, aşağıdaki metne benzer bir değer döndürür:

```text
/subscriptions/{GUID}/resourcegroups/{myresourcegroup}/providers/Microsoft.ContainerService/managedClusters/{myexistingcluster}
```

Varolan kümeyi çalışma alanınıza eklemek için aşağıdaki komutu kullanın. Önceki `aksresourceid` komuttarafından döndürülen değerle değiştirin. Çalışma `myresourcegroup` alanınızı içeren kaynak grubuyla değiştirin. Çalışma `myworkspace` alanı adınız ile değiştirin.

```azurecli
az ml computetarget attach aks -n myaks -i aksresourceid -g myresourcegroup -w myworkspace
```

Daha fazla bilgi için [aks referansı eklemek az ml computetarget](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks) bakın.

## <a name="deploy-to-aks"></a>AKS’ye dağıtma

Bir modeli Azure Kubernetes Hizmetine dağıtmak için, gereken işlem kaynaklarını açıklayan bir __dağıtım yapılandırması__ oluşturun. Örneğin, çekirdek ve bellek sayısı. Ayrıca, modeli ve web hizmetini barındırmak için gereken ortamı açıklayan bir __çıkarım yapılandırmasına__da ihtiyacınız vardır. Çıkarım yapılandırması oluşturma hakkında daha fazla bilgi [için, modelleri nasıl ve nerede dağıtacağınız](how-to-deploy-and-where.md)konusunda bakın.

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

Bu örnekte kullanılan sınıflar, yöntemler ve parametreler hakkında daha fazla bilgi için aşağıdaki başvuru belgelerine bakın:

* [AksCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute?view=azure-ml-py)
* [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)
* [Model.deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-)
* [Webservice.wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#wait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>CLI'yi kullanma

CLI kullanarak dağıtmak için aşağıdaki komutu kullanın. AKS işlem hedefinin adı ile değiştirin. `myaks` Kayıtlı `mymodel:1` modelin adı ve sürümüyle değiştirin. Bu `myservice` hizmeti vermek için adla değiştirin:

```azurecli-interactive
az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

Daha fazla bilgi için [az ml modeli dağıtım](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) referansına bakın.

### <a name="using-vs-code"></a>VS Code'u kullanma

VS Kodu'nu kullanma hakkında daha fazla bilgi için, [VS Kodu uzantısı üzerinden AKS'ye dağıtı'ya](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model)bakın.

> [!IMPORTANT]
> VS Kodu üzerinden dağıtmak, AKS kümesinin önceden oluşturulmasını veya çalışma alanınıza eklenmesini gerektirir.

## <a name="deploy-models-to-aks-using-controlled-rollout-preview"></a>Modelleri kontrollü kullanıma alma (önizleme) kullanarak AKS'ye dağıtın
Son noktaları kullanarak model sürümlerini kontrollü bir şekilde analiz edin ve tanıtın. Tek bir bitiş noktasının arkasına en fazla 6 sürüm dağıtın ve dağıtılan her sürüme puanlama trafiğinin %'sini yapılandırın. Son noktaların ve dağıtılan sürümlerin operasyonel ölçümlerini görüntülemek için uygulama öngörülerini etkinleştirebilirsiniz.

### <a name="create-an-endpoint"></a>Uç nokta oluşturma
Modellerinizi dağıtmaya hazır olduktan sonra, bir puanlama bitiş noktası oluşturun ve ilk sürümünüzü dağıtın. Aşağıdaki adım, SDK'yı kullanarak bitiş noktasını nasıl dağıtabileceğinizi ve oluşturabileceğinizi gösterir. İlk dağıtım varsayılan sürüm olarak tanımlanır, bu da tüm sürümler arasında belirtilmeyen trafiğin yüzdesinin varsayılan sürüme gideceği anlamına gelir.  

```python
import azureml.core,
from azureml.core.webservice import AksEndpoint
from azureml.core.compute import AksCompute
from azureml.core.compute import ComputeTarget
# select a created compute
compute = ComputeTarget(ws, 'myaks')
namespace_name= endpointnamespace
# define the endpoint and version name
endpoint_name = "mynewendpoint",
version_name= "versiona",
# create the deployment config and define the scoring traffic percentile for the first deployment
endpoint_deployment_config = AksEndpoint.deploy_configuration(cpu_cores = 0.1, memory_gb = 0.2,
                                                              enable_app_insights = True,
                                                              tags = {'sckitlearn':'demo'},
                                                              description = "testing versions",
                                                              version_name = version_name,
                                                              traffic_percentile = 20)
 # deploy the model and endpoint
 endpoint = Model.deploy(ws, endpoint_name, [model], inference_config, endpoint_deployment_config, compute)
 ```

### <a name="update-and-add-versions-to-an-endpoint"></a>Sürümleri bir bitiş noktasına güncelleştirme ve ekleme

Bitiş noktanıza başka bir sürüm ekleyin ve sürüme giden puanlama trafiğinin yüzdeliğini yapılandırın. İki tür versiyon vardır, bir kontrol ve bir tedavi sürümü. Tek bir denetim sürümüyle karşılaştırmaya yardımcı olacak birden çok tedavi sürümü olabilir.

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
```

Varolan sürümleri güncelleştirin veya bitiş noktasında silin. Sürümün varsayılan türünü, denetim türünü ve trafik yüzdeliğini değiştirebilirsiniz.

 ```python
from azureml.core.webservice import AksEndpoint

# update the version's scoring traffic percentage and if it is a default or control type
endpoint.update_version(version_name=endpoint.versions["versionb"].name,
                        description="my second version update",
                        traffic_percentile=40,
                        is_default=True,
                        is_control_version_type=True)

# delete a version in an endpoint
endpoint.delete_version(version_name="versionb")

```


## <a name="web-service-authentication"></a>Web hizmeti kimlik doğrulaması

Azure Kubernetes Hizmetine dağıtılırken, __anahtar tabanlı__ kimlik doğrulaması varsayılan olarak etkinleştirilir. __Belirteç tabanlı__ kimlik doğrulamasını da etkinleştirebilirsiniz. Belirteç tabanlı kimlik doğrulama, istemcilerin dağıtılan hizmete istekte bulunmak için kullanılan bir kimlik doğrulama belirteci istemek için bir Azure Etkin Dizin hesabı kullanmasını gerektirir.

Kimlik doğrulamayı devre dışı `auth_enabled=False` __ksaymak__ için, dağıtım yapılandırmasını oluştururken parametreyi ayarlayın. Aşağıdaki örnek, SDK kullanarak kimlik doğrulamasını devre dışı bırakmaz:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, auth_enabled=False)
```

İstemci uygulamasından kimlik doğrulama hakkında daha fazla bilgi [için, web hizmeti olarak dağıtılan Azure Machine Learning modelini tüket'e](how-to-consume-web-service.md)bakın.

### <a name="authentication-with-keys"></a>Anahtarlarla kimlik doğrulama

Anahtar kimlik doğrulaması etkinleştirilmişse, birincil ve ikincil kimlik doğrulama anahtarını almak için `get_keys` yöntemi kullanabilirsiniz:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarı yenilemeniz gerekiyorsa,[`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)

### <a name="authentication-with-tokens"></a>Belirteçlerle kimlik doğrulama

Belirteç kimlik doğrulamasını `token_auth_enabled=True` etkinleştirmek için, dağıtım oluştururken veya güncellerken parametreyi ayarlayın. Aşağıdaki örnek, SDK kullanarak belirteç kimlik doğrulaması sağlar:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, token_auth_enabled=True)
```

Belirteç kimlik doğrulaması etkinse, bir JWT belirteci ve bu belirteç son kullanma süresini almak için `get_token` yöntemi kullanabilirsiniz:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirteç `refresh_by` saatinden sonra yeni bir belirteç talep etmeniz gerekir.
>
> Microsoft, Azure Makine Öğrenimi çalışma alanınızı Azure Kubernetes Hizmet kümenizle aynı bölgede oluşturmanızı şiddetle önerir. Web hizmeti, bir belirteçle kimlik doğrulaması yapmak için Azure Machine Learning çalışma alanınızın oluşturulduğu bölgeye bir arama yapar. Çalışma alanınızın bölgesi kullanılamıyorsa, kümeniz çalışma alanınızdan farklı bir bölgede yse, web hizmetiniz için bir belirteç getiremezsiniz. Bu, çalışma alanınız yeniden kullanılabilir olana kadar Token tabanlı Kimlik Doğrulama'nın kullanılamamasına neden olur. Ayrıca, kümenizin bölgesi yle çalışma alanınızın bölgesi arasındaki mesafe ne kadar büyükse, bir belirteci getirmesi o kadar uzun sürer.

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirme

[!INCLUDE [aml-update-web-service](../../includes/machine-learning-update-web-service.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağda güvenli deneme ve çıkarım](how-to-enable-virtual-network.md)
* [Özel Docker görüntüsünü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md)
* [Dağıtım sorun giderme](how-to-troubleshoot-deployment.md)
* [SSL ile Güvenli Azure Machine Learning web hizmetleri](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir ML Modelini tüketin](how-to-consume-web-service.md)
* [Azure Machine Learning modellerinizi Uygulama Öngörüleri ile izleyin](how-to-enable-app-insights.md)
* [Üretimdeki modeller için veri toplama](how-to-enable-data-collection.md)
