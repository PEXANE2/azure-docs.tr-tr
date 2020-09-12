---
title: Sanal ağlarla güvenli ınırel sınırlama ortamları
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Inde bir Azure sanal ağını kullanarak, sanal ağ oluşturma ortamınızı güvenli hale getirin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 07/16/2020
ms.custom: contperfq4, tracking-python
ms.openlocfilehash: 43d8211c7cbe5d785f6004157ff40c0bf9dfa788
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89665179"
---
# <a name="secure-an-azure-machine-learning-inferencing-environment-with-virtual-networks"></a>Sanal ağlarla Azure Machine Learning ınvenli bir ortamın güvenliğini sağlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning ' de sanal ağ ile ınırel korumalı ortamların güvenliğini nasıl sağlayacağınızı öğreneceksiniz.

Bu makale, bir Azure Machine Learning iş akışını güvenli hale getirmek için size kılavuzluk eden beş bölümlü bir serinin dördüncü kısmıdır. İlk olarak birinci mimariyi anlamak için [tek bir bölüm (VNet genel bakış](how-to-network-security-overview.md) ) okumanız önemle tavsiye ederiz. 

Bu serideki diğer makalelere göz atın:

[1. VNET genel bakış](how-to-network-security-overview.md)  >  [çalışma alanı](how-to-secure-workspace-vnet.md)3 ' ü güvenceye alın  >  [. Eğitim ortamının](how-to-secure-training-vnet.md)4 ' ü koruyun  >  **. Invenli ortam**5 ' i güvenli hale getirin  >  [. Studio işlevselliğini etkinleştir](how-to-enable-studio-virtual-network.md)

Bu makalede, bir sanal ağda aşağıdaki ınırm kaynaklarını güvenli hale getirme hakkında bilgi edineceksiniz:
> [!div class="checklist"]
> - Varsayılan Azure Kubernetes hizmeti (AKS) kümesi
> - Özel AKS kümesi
> - Azure Container Instances (ACI)


## <a name="prerequisites"></a>Ön koşullar

+ Genel sanal ağ senaryolarını ve genel sanal ağ mimarisini anlamak için [ağ güvenliğine genel bakış](how-to-network-security-overview.md) makalesini okuyun.

+ İşlem kaynaklarınızla kullanılacak mevcut bir sanal ağ ve alt ağ.

+ Bir sanal ağa veya alt ağa kaynak dağıtmak için, Kullanıcı hesabınızın Azure rol tabanlı erişim denetimlerinde (RBAC) aşağıdaki eylemler için izinleri olmalıdır:

    - Sanal ağ kaynağında "Microsoft. Network/virtualNetworks/JOIN/Action".
    - Alt ağ kaynağında "Microsoft. Network/virtualNetworks/subnet/JOIN/Action".

    Ağ ile RBAC hakkında daha fazla bilgi için bkz. [ağ yerleşik rolleri](/azure/role-based-access-control/built-in-roles#networking)

<a id="aksvnet"></a>

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Bir sanal ağda AKS kümesi kullanmak için aşağıdaki ağ gereksinimlerinin karşılanması gerekir:

> [!div class="checklist"]
> * [Azure Kubernetes hizmeti 'nde (AKS) Gelişmiş ağ yapılandırma](../aks/configure-azure-cni.md#prerequisites)' daki önkoşulları izleyin.
> * AKS örneği ve sanal ağ aynı bölgede olmalıdır. Bir sanal ağ içinde çalışma alanı tarafından kullanılan Azure depolama hesaplarını güvenli hale getirmek istiyorsanız, AKS örneğiyle aynı sanal ağda olmaları gerekir.


Bir sanal ağdaki AKS 'leri çalışma alanınıza eklemek için aşağıdaki adımları kullanın:

1. [Azure Machine Learning Studio](https://ml.azure.com/)' da oturum açın ve ardından aboneliğinizi ve çalışma alanınızı seçin.

1. Sol tarafta __işlem__ ' ı seçin.

1. Merkezden __çıkarım kümeleri__ ' ni seçin ve ardından öğesini seçin __+__ .

1. __Yeni çıkarım kümesi__ Iletişim kutusunda __ağ yapılandırması__altında __Gelişmiş__ ' i seçin.

1. Bu işlem kaynağını bir sanal ağ kullanacak şekilde yapılandırmak için aşağıdaki eylemleri gerçekleştirin:

    1. __Kaynak grubu__ açılır listesinde, sanal ağı içeren kaynak grubunu seçin.
    1. __Sanal ağ__ açılan listesinde, alt ağı içeren sanal ağı seçin.
    1. __Alt ağ__ açılan listesinde, alt ağı seçin.
    1. __Kubernetes hizmeti adres aralığı__ kutusuna Kubernetes hizmeti adres aralığını girin. Bu adres aralığı, küme için kullanılabilir IP adreslerini tanımlamak üzere sınıfsız etki alanları arası yönlendirme (CıDR) gösterimi IP aralığını kullanır. Herhangi bir alt ağ IP aralığı ile çakışmamalıdır (örneğin, 10.0.0.0/16).
    1. __KUBERNETES DNS HIZMETI IP adresi__ kutusuna Kubernetes DNS hizmeti IP adresini girin. Bu IP adresi, Kubernetes DNS hizmetine atanır. Bu, Kubernetes hizmeti adres aralığı içinde olmalıdır (örneğin, 10.0.0.10).
    1. __Docker köprü adresi__ kutusuna Docker köprü adresini girin. Bu IP adresi Docker köprüsüne atandı. Herhangi bir alt ağ IP aralığında olmamalı veya Kubernetes hizmeti adres aralığı (örneğin, 172.17.0.1/16) olmalıdır.

   ![Azure Machine Learning: Machine Learning İşlem sanal ağ ayarları](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Sanal ağı denetleyen NSG grubunun, sanal ağ dışından çağrılabilmesi için Puanlama uç noktası için etkin bir gelen güvenlik kuralına sahip olduğundan emin olun.
   > [!IMPORTANT]
   > NSG için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.

   [![Bir gelen güvenlik kuralı](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

Azure Kubernetes hizmetini bir sanal ağa eklemek için Azure Machine Learning SDK 'sını de kullanabilirsiniz. Bir sanal ağda zaten bir AKS kümeniz varsa, bunları [aks 'e dağıtma](how-to-deploy-and-where.md)bölümünde açıklandığı gibi çalışma alanına ekleyin. Aşağıdaki kod, `default` adlı bir sanal ağın alt ağında yeni BIR AKS örneği oluşturur `mynetwork` :

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

Oluşturma işlemi tamamlandığında, bir sanal ağın arkasındaki AKS kümesinde çıkarım veya model Puanlama çalıştırabilirsiniz. Daha fazla bilgi için bkz. [AKS 'e dağıtma](how-to-deploy-and-where.md).

## <a name="private-aks-cluster"></a>Özel AKS kümesi

Varsayılan olarak, AKS kümelerinin, genel IP adreslerine sahip bir denetim düzlemi veya API sunucusu vardır. Özel bir AKS kümesi oluşturarak, AKS 'i özel denetim düzlemi kullanacak şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. [özel Azure Kubernetes hizmet kümesi oluşturma](../aks/private-clusters.md).

Özel AKS kümesi oluşturduktan sonra, Azure Machine Learning ile kullanmak üzere [kümeyi sanal ağa bağlayın](how-to-create-attach-kubernetes.md) .

## <a name="internal-aks-load-balancer"></a>İç AKS yük dengeleyici

Varsayılan olarak, AKS dağıtımları [ortak yük dengeleyici](../aks/load-balancer-standard.md)kullanır. Bu bölümde, AKS 'in iç yük dengeleyiciyi kullanmak üzere nasıl yapılandırılacağını öğreneceksiniz. Ön uç olarak yalnızca özel IP 'Lere izin verilen bir iç (veya özel) yük dengeleyici kullanılır. İç yük dengeleyiciler, bir sanal ağ içindeki trafiğin yükünü dengelemek için kullanılır

Bir özel yük dengeleyici, AKS 'in _iç yük dengeleyiciyi_kullanacak şekilde yapılandırılması ile etkinleştirilir. 

### <a name="network-contributor-role"></a>Ağ katılımcısı rolü

> [!IMPORTANT]
> Daha önce oluşturduğunuz bir sanal ağ sağlayarak bir AKS kümesi oluşturur veya iliştirdiyseniz, AKS kümeniz için hizmet sorumlusu (SP) veya yönetilen kimliğe, sanal ağı içeren kaynak grubuna _ağ katılımcısı_ rolü vermelisiniz. İç yük dengeleyiciyi özel IP olarak değiştirmeye çalışmadan önce bunun yapılması gerekir.
>
> Kimliği ağ katılımcısı olarak eklemek için aşağıdaki adımları kullanın:

1. AKS için hizmet sorumlusu veya yönetilen kimlik KIMLIĞINI bulmak için aşağıdaki Azure CLı komutlarını kullanın. `<aks-cluster-name>`Kümenin adıyla değiştirin. `<resource-group-name>` _, Aks kümesini içeren_kaynak grubunun adıyla değiştirin:

    ```azurecli-interactive
    az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query servicePrincipalProfile.clientId
    ``` 

    Bu komut değerini döndürürse `msi` , yönetilen kimliğin asıl kimliğini tanımlamak için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query identity.principalId
    ```

1. Sanal ağınızı içeren kaynak grubunun KIMLIĞINI bulmak için aşağıdaki komutu kullanın. `<resource-group-name>` _Sanal ağı içeren_kaynak grubunun adıyla değiştirin:

    ```azurecli-interactive
    az group show -n <resource-group-name> --query id
    ```

1. Hizmet sorumlusu veya yönetilen kimliği bir ağ katılımcısı olarak eklemek için aşağıdaki komutu kullanın. `<SP-or-managed-identity>`Hizmet sorumlusu veya yönetilen kimlik için döndürülen kimlikle değiştirin. `<resource-group-id>`Sanal ağı içeren kaynak grubu için döndürülen kimlikle değiştirin:

    ```azurecli-interactive
    az role assignment create --assignee <SP-or-managed-identity> --role 'Network Contributor' --scope <resource-group-id>
    ```
AKS ile iç yük dengeleyiciyi kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmeti ile iç yük dengeleyici kullanma](/azure/aks/internal-lb).

### <a name="enable-private-load-balancer"></a>Özel yük dengeleyiciyi etkinleştir

> [!IMPORTANT]
> Azure Kubernetes hizmet kümesini oluştururken özel IP 'yi etkinleştiremezsiniz. Mevcut bir kümeye güncelleştirme olarak etkinleştirilmelidir.

Aşağıdaki kod parçacığı, __Yeni BIR AKS kümesi oluşturmayı__ve ardından özel bir IP/iç yük dengeleyici kullanmak üzere güncelleştirmeyi göstermektedir:

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute, ComputeTarget

# Verify that cluster does not exist already
try:
    aks_target = AksCompute(workspace=ws, name=aks_cluster_name)
    print("Found existing aks cluster")

except:
    print("Creating new aks cluster")

    # Subnet to use for AKS
    subnet_name = "default"
    # Create AKS configuration
    prov_config = AksCompute.provisioning_configuration(location = "eastus2")
    # Set info for existing virtual network to create the cluster in
    prov_config.vnet_resourcegroup_name = "myvnetresourcegroup"
    prov_config.vnet_name = "myvnetname"
    prov_config.service_cidr = "10.0.0.0/16"
    prov_config.dns_service_ip = "10.0.0.10"
    prov_config.subnet_name = subnet_name
    prov_config.docker_bridge_cidr = "172.17.0.1/16"

    # Create compute target
    aks_target = ComputeTarget.create(workspace = ws, name = "myaks", provisioning_configuration = prov_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
    
    # Update AKS configuration to use an internal load balancer
    update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
    aks_target.update(update_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
```

__Azure CLI__

```azurecli-interactive
az rest --method put --uri https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace>/computes/<compute>?api-version=2018-11-19 --body @body.json
```

`body.json`Komutu tarafından başvurulan dosyanın içeriği AŞAĞıDAKI JSON belgesine benzer:

```json
{ 
    "location": "<region>", 
    "properties": { 
        "resourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-name>", 
        "computeType": "AKS", 
        "provisioningState": "Succeeded", 
        "properties": { 
            "loadBalancerType": "InternalLoadBalancer", 
            "agentCount": <agent-count>, 
            "agentVmSize": "vm-size", 
            "clusterFqdn": "<cluster-fqdn>" 
        } 
    } 
} 
```

__Var olan bir kümeyi çalışma alanınıza iliştirirken__ , yük dengeleyiciyi yapılandırmak için iliştirme işleminden sonra beklemeniz gerekir.

Bir küme ekleme hakkında bilgi için bkz. [var olan AKS kümesi ekleme](how-to-create-attach-kubernetes.md).

Mevcut kümeyi iliştirdikten sonra, kümeyi özel bir IP kullanacak şekilde güncelleştirebilirsiniz.

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute

# ws = workspace object. Creation not shown in this snippet
aks_target = AksCompute(ws,"myaks")

# Change to the name of the subnet that contains AKS
subnet_name = "default"
# Update AKS configuration to use an internal load balancer
update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
aks_target.update(update_config)
# Wait for the operation to complete
aks_target.wait_for_completion(show_output = True)
```

## <a name="enable-azure-container-instances-aci"></a>Azure Container Instances etkinleştir (ACI)

Azure Container Instances, bir model dağıtıldığında dinamik olarak oluşturulur. Azure Machine Learning sanal ağ içinde ACI oluşturmak üzere etkinleştirmek için, dağıtım tarafından kullanılan alt ağ için __alt ağ temsilcisini__ etkinleştirmeniz gerekir.

> [!WARNING]
> Bir sanal ağda Azure Container Instances kullanırken, sanal ağın Azure Machine Learning çalışma alanınız ile aynı kaynak grubunda olması gerekir.
>
> Sanal ağ içinde Azure Container Instances kullanırken, çalışma alanınızın Azure Container Registry (ACR) Sanal ağda de olamaz.

Çalışma alanınıza bir sanal ağda ACI 'yi kullanmak için aşağıdaki adımları kullanın:

1. Sanal ağınızda alt ağ temsilcisini etkinleştirmek için [alt ağ temsili ekleme veya kaldırma](../virtual-network/manage-subnet-delegation.md) makalesindeki bilgileri kullanın. Bir sanal ağ oluştururken temsilciyi etkinleştirebilir veya var olan bir ağa ekleyebilirsiniz.

    > [!IMPORTANT]
    > Temsilci seçme etkinleştirildiğinde, `Microsoft.ContainerInstance/containerGroups` __hizmet verme için alt ağ__ olarak kullanın.

2. [Aciwebservice. deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none--vnet-name-none--subnet-name-none-&preserve-view=true)kullanarak modeli dağıtın, `vnet_name` ve `subnet_name` parametrelerini kullanın. Bu parametreleri, temsilciyi etkinleştirdiğiniz sanal ağ adı ve alt ağa ayarlayın.


## <a name="next-steps"></a>Sonraki adımlar

Bu makale, dört bölümlü bir sanal ağ serisinde üç bölümden oluşan bir parçasıdır. Bir sanal ağın güvenliğini nasıl sağlayacağınızı öğrenmek için makalelerin geri kalanına bakın:

* [1. kısım: sanal ağa genel bakış](how-to-network-security-overview.md)
* [2. Bölüm: çalışma alanı kaynaklarını güvenli hale getirme](how-to-secure-workspace-vnet.md)
* [3. kısım: eğitim ortamının güvenliğini sağlama](how-to-secure-training-vnet.md)
* [5. Bölüm: Studio işlevselliğini etkinleştirme](how-to-enable-studio-virtual-network.md)