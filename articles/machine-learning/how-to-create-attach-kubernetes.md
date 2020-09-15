---
title: Azure Kubernetes hizmeti oluşturma ve iliştirme
titleSuffix: Azure Machine Learning
description: Azure Kubernetes hizmeti (AKS), bir makine öğrenimi modelini Web hizmeti olarak dağıtmak için kullanılabilir. Azure Machine Learning aracılığıyla yeni bir AKS kümesi oluşturmayı öğrenin. Ayrıca, Azure Machine Learning çalışma alanınıza mevcut bir AKS kümesi eklemeyi öğreneceksiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 09/01/2020
ms.openlocfilehash: cac14d5995042847bc98e47e50ea2d188382fd2a
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564347"
---
# <a name="create-and-attach-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesi oluşturma ve iliştirme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning, eğitilen makine öğrenimi modellerini Azure Kubernetes hizmetine dağıtabilir. Ancak, önce Azure ML çalışma alanınızdan bir Azure Kubernetes hizmeti (AKS) kümesi __oluşturmanız__ ya da mevcut bir aks kümesini __eklemeniz__ gerekir. Bu makalede, küme oluşturma ve ekleme hakkında bilgi sağlanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure Machine Learning çalışma alanı. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

- [Machine Learning hizmeti Için Azure CLI uzantısı](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)veya [Azure Machine Learning Visual Studio Code uzantısı](tutorial-setup-vscode-extension.md).

- Azure ML çalışma alanınız ve AKS kümesi arasındaki iletişimin güvenliğini sağlamak için bir Azure sanal ağı kullanmayı planlıyorsanız, [eğitim & çıkarım makalesi sırasında Ağ yalıtımını](how-to-enable-virtual-network.md) okuyun.

## <a name="limitations"></a>Sınırlamalar

- Temel bir Load Balancer (BLB) yerine kümenizde dağıtılan bir **Standart Load Balancer (SLB)** gerekiyorsa, aks PORTALıNDA/CLı/SDK ' da bir küme oluşturun ve ardından AML çalışma alanına **ekleyin** .

- Genel IP adreslerinin oluşturulmasını kısıtlayan bir Azure Ilkeniz varsa, AKS kümesi oluşturma işlemi başarısız olur. AKS, [çıkış trafiği](/azure/aks/limit-egress-traffic)için genel bir IP gerektirir. TH çıkış trafiği makalesi, bazı tam etki alanı adları dışında, genel IP aracılığıyla kümeden çıkış trafiğini kilitlemek için rehberlik sağlar. Genel IP 'yi etkinleştirmenin 2 yolu vardır:
    - Küme, varsayılan olarak BLB veya SLB ile oluşturulan genel IP 'yi kullanabilir veya
    - Küme, genel IP olmadan oluşturulabilir ve ardından genel IP, Kullanıcı tanımlı bir yol ile bir güvenlik duvarıyla yapılandırılır. Daha fazla bilgi için bkz. [Kullanıcı tanımlı rota ile küme çıkışı özelleştirme](/azure/aks/egress-outboundtype).
    
    AML denetim düzlemi bu genel IP ile iletişim kurmadı. Dağıtımlar için AKS denetim düzlemiyle konuşuyor. 

- [API sunucusuna erişim Için yetkilendirilmiş BIR IP aralığı etkin](/azure/aks/api-server-authorized-ip-ranges) **olan bir** aks KÜMESI eklerseniz, aks kümesi için AML denetim düzlemi IP aralıklarını etkinleştirin. AML denetim düzlemi eşleştirilmiş bölgeler arasında dağıtılır ve aks kümesinde çıkarım Pod dağıtır. API sunucusuna erişim olmadan, çıkarım 'ler dağıtılamaz. Bir AKS kümesindeki IP aralıklarını etkinleştirirken, her iki [eşleştirilmiş bölge](/azure/best-practices-availability-paired-regions) Için de [IP aralıklarını](https://www.microsoft.com/download/confirmation.aspx?id=56519) kullanın.

    Yetkilendirilmiş IP aralıkları yalnızca Standart Load Balancer ile kullanılabilir.

- Özel bir AKS kümesi (Azure özel bağlantısı kullanarak) kullanmak istiyorsanız, önce kümeyi oluşturmanız ve ardından çalışma alanına **bağlamanız** gerekir. Daha fazla bilgi için bkz. [özel Azure Kubernetes hizmet kümesi oluşturma](/azure/aks/private-clusters).

- AKS kümesinin işlem adı, Azure ML çalışma alanınız içinde benzersiz OLMALıDıR.
    - Ad gereklidir ve 3 ila 24 karakter uzunluğunda olmalıdır.
    - Geçerli karakterler büyük ve küçük harf, rakam ve-karakter.
    - Ad bir harfle başlamalıdır.
    - Adın, bir Azure bölgesindeki tüm mevcut hesaplar arasında benzersiz olması gerekir. Seçtiğiniz ad benzersiz değilse bir uyarı görürsünüz.
   
 - Modelleri **GPU** düğümlerine veya **FPGA** düğümlerine (ya da belirli bir SKU) DAĞıTMAK istiyorsanız, belirli SKU 'ya sahip bir küme oluşturmanız gerekir. Mevcut bir kümede ikincil düğüm havuzu oluşturma ve ikincil düğüm havuzunda modelleri dağıtma desteği yoktur.
 
- Bir küme oluştururken veya iliştirirken, __geliştirme ve test__ ya da __Üretim__için kümenin oluşturulup oluşturulmayacağını seçebilirsiniz. Üretim yerine __geliştirme__, __doğrulama__ve __Test__ için bir aks kümesi oluşturmak istiyorsanız, __küme amacını__ __geliştirme-test__olarak ayarlayın. Küme amacını belirtmezseniz, bir __Üretim__ kümesi oluşturulur. 

    > [!IMPORTANT]
    > __Geliştirme ve test__ kümesi, üretim düzeyi trafiğe uygun değildir ve çıkarım sürelerini artırabilir. Geliştirme ve test kümeleri de hata toleransı garantisi vermez.

- Küme oluştururken veya iliştirirken, küme __Üretim__için kullanılacaksa, en az 12 __sanal CPU__içermesi gerekir. Sanal CPU sayısı, seçilen VM boyutu tarafından belirtilen __çekirdek sayısına__ göre kümedeki __düğümlerin sayısı__ çarpılarak hesaplanabilir. Örneğin, 4 sanal çekirdeği olan "Standard_D3_v2" sanal makine boyutunu kullanırsanız, düğüm sayısı olarak 3 veya daha büyük bir değer seçmeniz gerekir.

    Geliştirme ve __Test__ kümesi için en az 2 sanal CPU 'yu yeniden komutlarız.

- Azure Machine Learning SDK, bir AKS kümesini ölçeklendirmeye yönelik destek sağlamaz. Kümedeki düğümleri ölçeklendirmek için Azure Machine Learning Studio 'daki AKS kümeniz için Kullanıcı arabirimini kullanın. Kümenin VM boyutunu değil, yalnızca düğüm sayısını değiştirebilirsiniz. Bir AKS kümesindeki düğümleri ölçeklendirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

    - [AKS kümesindeki düğüm sayısını el ile ölçeklendirme](../aks/scale-cluster.md)
    - [AKS 'de küme otomatik Scaler 'ı ayarlama](../aks/cluster-autoscaler.md)

## <a name="azure-kubernetes-service-version"></a>Azure Kubernetes hizmet sürümü

Azure Kubernetes hizmeti, çeşitli Kubernetes sürümlerini kullanarak bir küme oluşturmanıza olanak sağlar. Kullanılabilir sürümler hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmetinde desteklenen Kubernetes sürümleri](/azure/aks/supported-kubernetes-versions).

Aşağıdaki yöntemlerden birini kullanarak bir Azure Kubernetes hizmet kümesi **oluştururken** , oluşturulan kümenin *sürümünde bir seçeneğiniz* yoktur:

* Azure Machine Learning Studio veya Azure portal Azure Machine Learning bölümü.
* Azure CLı için Machine Learning uzantısı.
* SDK Azure Machine Learning.

AKS kümesi oluşturma yöntemleri kümenin __varsayılan__ sürümünü kullanır. Yeni Kubernetes sürümleri kullanılabilir hale geldiğinde *, varsayılan sürüm zaman içinde değişir* .

Mevcut bir AKS kümesini **eklerken** , şu anda desteklenen tüm aks sürümlerini destekliyoruz.

> [!NOTE]
> Artık desteklenmeyen daha eski bir kümeniz olan uç durumlar olabilir. Bu durumda iliştirme işlemi bir hata döndürür ve şu anda desteklenen sürümleri listeler.
>
> **Önizleme** sürümleri ekleyebilirsiniz. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Önizleme sürümlerinin kullanımı için destek sınırlı olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="available-and-default-versions"></a>Kullanılabilir ve varsayılan sürümler

Kullanılabilir ve varsayılan AKS sürümlerini bulmak için [az aks get-versions](/cli/azure/aks?view=azure-cli-latest#az_aks_get_versions) [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) komutunu kullanın. Örneğin, aşağıdaki komut Batı ABD bölgesinde bulunan sürümleri döndürür:

```azurecli-interactive
az aks get-versions -l westus -o table
```

Bu komutun çıktısı aşağıdaki metne benzer:

```text
KubernetesVersion    Upgrades
-------------------  ----------------------------------------
1.18.6(preview)      None available
1.18.4(preview)      1.18.6(preview)
1.17.9               1.18.4(preview), 1.18.6(preview)
1.17.7               1.17.9, 1.18.4(preview), 1.18.6(preview)
1.16.13              1.17.7, 1.17.9
1.16.10              1.16.13, 1.17.7, 1.17.9
1.15.12              1.16.10, 1.16.13
1.15.11              1.15.12, 1.16.10, 1.16.13
```

Azure Machine Learning aracılığıyla bir küme **oluştururken** kullanılan varsayılan sürümü bulmak için, `--query` varsayılan sürümü seçmek için parametresini kullanabilirsiniz:

```azurecli-interactive
az aks get-versions -l westus --query "orchestrators[?default == `true`].orchestratorVersion" -o table
```

Bu komutun çıktısı aşağıdaki metne benzer:

```text
Result
--------
1.16.13
```

**Kullanılabilir sürümleri programlı olarak denetlemek**Isterseniz, [kapsayıcı hizmeti istemci listesi düzenleyiciler](https://docs.microsoft.com/rest/api/container-service/container%20service%20client/listorchestrators) REST API kullanın. Kullanılabilir sürümleri bulmak için, olduğu girişlere bakın `orchestratorType` `Kubernetes` . İlişkili `orchestrationVersion` girişler, çalışma alanınıza **iliştirilebilecek** kullanılabilir sürümleri içerir.

Azure Machine Learning aracılığıyla küme **oluştururken** kullanılan varsayılan sürümü bulmak için, ve olduğu girişi bulun `orchestratorType` `Kubernetes` `default` `true` . İlişkili `orchestratorVersion` değer varsayılan sürümdür. Aşağıdaki JSON kod parçacığında bir örnek girişi gösterilmektedir:

```json
...
 {
        "orchestratorType": "Kubernetes",
        "orchestratorVersion": "1.16.13",
        "default": true,
        "upgrades": [
          {
            "orchestratorType": "",
            "orchestratorVersion": "1.17.7",
            "isPreview": false
          }
        ]
      },
...
```

## <a name="create-a-new-aks-cluster"></a>Yeni bir AKS kümesi oluşturma

**Tahmini süre**: yaklaşık 10 dakika.

AKS kümesi oluşturma veya iliştirme, çalışma alanınız için tek seferlik bir işlemdir. Bu kümeyi birden çok dağıtım için yeniden kullanabilirsiniz. Kümeyi veya onu içeren kaynak grubunu silerseniz, bir dahaki sefer dağıtmanız gerektiğinde yeni bir küme oluşturmanız gerekir. Çalışma alanınıza eklenmiş birden çok AKS kümeniz olabilir.

Aşağıdaki örnek, SDK ve CLı kullanarak yeni bir AKS kümesi oluşturmayı göstermektedir:

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

# Example configuration to use an existing virtual network
# prov_config.vnet_name = "mynetwork"
# prov_config.vnet_resourcegroup_name = "mygroup"
# prov_config.subnet_name = "default"
# prov_config.service_cidr = "10.0.0.0/16"
# prov_config.dns_service_ip = "10.0.0.10"
# prov_config.docker_bridge_cidr = "172.17.0.1/16"

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AksCompute. Clusteramacını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py&preserve-view=true)
* [AksCompute. provisioning_configuration](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [ComputeTarget. Create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-)
* [ComputeTarget. wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#wait-for-completion-show-output-false-)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az ml computetarget create aks -n myaks
```

Daha fazla bilgi için, [az ml computetarget Create aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-aks) Reference bölümüne bakın.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Portalda AKS kümesi oluşturma hakkında daha fazla bilgi için, bkz. [Azure Machine Learning Studio 'da işlem hedefleri oluşturma](how-to-create-attach-compute-studio.md#inference-clusters).

---

## <a name="attach-an-existing-aks-cluster"></a>Mevcut bir AKS kümesini iliştirme

**Tahmini süre:** Yaklaşık 5 dakika.

Azure aboneliğinizde zaten AKS kümeniz varsa ve sürüm 1,17 veya daha düşükse, görüntünüzü dağıtmak için kullanabilirsiniz.

> [!TIP]
> Mevcut AKS kümesi, Azure Machine Learning çalışma alanınızın dışında bir Azure bölgesinde olabilir.


> [!WARNING]
> Çalışma alanınızdan aynı AKS kümesine birden çok, eşzamanlı ek oluşturmayın. Örneğin, iki farklı ad kullanarak bir AKS kümesini çalışma alanına ekleme. Her yeni ek önceki mevcut ekleri keser.
>
> Bir AKS kümesini yeniden eklemek istiyorsanız (örneğin, TLS veya başka bir küme yapılandırma ayarını değiştirmek için), önce, bu eki,, [Akscompute. detach ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#detach--)kullanarak kaldırmanız gerekir.

Azure CLı veya portalını kullanarak bir AKS kümesi oluşturma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [AKS kümesi oluşturma (CLI)](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [AKS kümesi oluşturma (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)
* [AKS kümesi oluşturma (Azure hızlı başlangıç şablonlarında ARM şablonu)](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aks-azml-targetcompute)

Aşağıdaki örnek, var olan bir AKS kümesinin çalışma alanınıza nasıl ekleneceğini göstermektedir:

# <a name="python"></a>[Python](#tab/python)

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

# Wait for the attach process to complete
aks_target.wait_for_completion(show_output = True)
```

Bu örnekte kullanılan sınıflar, Yöntemler ve parametreler hakkında daha fazla bilgi için, aşağıdaki başvuru belgelerine bakın:

* [AksCompute. attach_configuration ()](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [AksCompute. Clusteramacını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py&preserve-view=true)
* [AksCompute. Attach](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#attach-workspace--name--attach-configuration-)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

CLı kullanarak var olan bir kümeyi eklemek için, var olan kümenin kaynak KIMLIĞINI almanız gerekir. Bu değeri almak için aşağıdaki komutu kullanın. `myexistingcluster`Değerini AKS kümenizin adıyla değiştirin. `myresourcegroup`Kümeyi içeren kaynak grubuyla değiştirin:

```azurecli
az aks show -n myexistingcluster -g myresourcegroup --query id
```

Bu komut aşağıdaki metne benzer bir değer döndürür:

```text
/subscriptions/{GUID}/resourcegroups/{myresourcegroup}/providers/Microsoft.ContainerService/managedClusters/{myexistingcluster}
```

Mevcut kümeyi çalışma alanınıza eklemek için aşağıdaki komutu kullanın. `aksresourceid`Önceki komutun döndürdüğü değerle değiştirin. `myresourcegroup`Çalışma alanınızı içeren kaynak grubuyla değiştirin. `myworkspace`Çalışma alanınızın adıyla değiştirin.

```azurecli
az ml computetarget attach aks -n myaks -i aksresourceid -g myresourcegroup -w myworkspace
```

Daha fazla bilgi için, [az ml computetarget Attach aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks) başvurusuna bakın.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Portala AKS kümesi ekleme hakkında daha fazla bilgi için, bkz. [Azure Machine Learning Studio 'da işlem hedefleri oluşturma](how-to-create-attach-compute-studio.md#inference-clusters).

---

## <a name="next-steps"></a>Sonraki adımlar

* [Bir modelin nasıl ve nereye dağıtılacağı](how-to-deploy-and-where.md)
* [Azure Kubernetes hizmet kümesine model dağıtma](how-to-deploy-azure-kubernetes-service.md)