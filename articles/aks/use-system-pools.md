---
title: Azure Kubernetes Service (AKS) içindeki sistem düğüm havuzlarını kullanma
description: Azure Kubernetes hizmeti 'nde (AKS) sistem düğüm havuzları oluşturmayı ve yönetmeyi öğrenin
services: container-service
ms.topic: article
ms.date: 06/18/2020
ms.author: mlearned
ms.openlocfilehash: 9b6270f81e7af8bd508d29510698e6cf9a5a2010
ms.sourcegitcommit: ff19f4ecaff33a414c0fa2d4c92542d6e91332f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85052661"
---
# <a name="manage-system-node-pools-in-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmetindeki (AKS) sistem düğüm havuzlarını yönetme

Azure Kubernetes hizmeti 'nde (AKS), aynı yapılandırmanın düğümleri *düğüm havuzlarında*birlikte gruplandırılır. Düğüm havuzları, uygulamalarınızı çalıştıran temel VM 'Leri içerir. Sistem düğüm havuzları ve Kullanıcı düğümü havuzları, AKS kümeleriniz için iki farklı düğüm havuzu modudur. Sistem düğüm havuzları, CoreDNS ve tunnelfront gibi kritik sistem yığınlarını barındırmanın birincil amacını sunar. Kullanıcı düğümü havuzları, uygulama yığınlarınızı barındırmanın birincil amacını sunar. Ancak, aks kümenizde yalnızca bir havuza sahip olmak istiyorsanız, uygulama Pod 'leri sistem düğüm havuzlarında zamanlanabilir. Her bir AKS kümesi en az bir düğümü olan en az bir sistem düğüm havuzu içermelidir.

> [!Important]
> Bir üretim ortamında AKS kümeniz için tek bir sistem düğüm havuzu çalıştırırsanız, düğüm havuzu için en az üç düğüm kullanmanızı öneririz.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure CLı sürüm 2.3.1 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="limitations"></a>Sınırlamalar

Sistem düğüm havuzlarını destekleyen AKS kümelerini oluştururken ve yönetirken aşağıdaki sınırlamalar geçerlidir.

* [Azure Kubernetes Service (AKS) Içindeki kotalar, sanal makine boyutu kısıtlamaları ve bölge kullanılabilirliği][quotas-skus-regions]konusuna bakın.
* AKS kümesi VM türü olarak sanal makine ölçek kümeleri ile oluşturulmalıdır.
* Düğüm havuzunun adı yalnızca küçük harfli alfasayısal karakterler içerebilir ve küçük harfle başlamalıdır. Linux düğüm havuzları için uzunluk 1 ile 12 karakter arasında olmalıdır. Windows düğüm havuzları için uzunluk 1 ile 6 karakter arasında olmalıdır.
* Düğüm havuzu modunu ayarlamak için 2020-03-01 veya üzeri bir API sürümü kullanılmalıdır. 2020-03-01 ' den eski API sürümlerinde oluşturulan kümeler yalnızca Kullanıcı düğümü havuzlarını içerir, ancak [güncelleştirme havuzu modu adımlarını](#update-existing-cluster-system-and-user-node-pools)izleyerek Sistem düğüm havuzlarını içerecek şekilde geçirilebilirler.
* Düğüm havuzunun modu gerekli bir özelliktir ve ARM şablonları veya doğrudan API çağrıları kullanılırken açıkça ayarlanması gerekir.

## <a name="system-and-user-node-pools"></a>Sistem ve Kullanıcı düğümü havuzları

Sistem düğüm havuzu düğümlerinin her biri **Kubernetes.Azure.com/Mode: System**etiketine sahiptir. Her AKS kümesi en az bir sistem düğüm havuzu içerir. Sistem düğüm havuzları aşağıdaki kısıtlamalara sahiptir:

* Sistem havuzları osType, Linux olmalıdır.
* Kullanıcı düğümü havuzları osType, Linux veya Windows olabilir.
* Sistem havuzlarının en az bir düğüm içermesi ve Kullanıcı düğüm havuzlarının sıfır veya daha fazla düğüm içermesi gerekir.
* Sistem düğüm havuzları, en az 2 vCPU ve 4 GB bellek için bir VM SKU 'SU gerektirir.
* Sistem düğüm havuzlarının [En düşük ve en yüksek değer formülünde Pod][maximum-pods]tarafından açıklandığı gibi en az 30 tane olması gerekir.
* Spot düğüm havuzları, Kullanıcı düğümü havuzları gerektirir.

Düğüm havuzlarıyla aşağıdaki işlemleri yapabilirsiniz:

* Bir sistem düğüm havuzunu bir Kullanıcı düğümü havuzu olacak şekilde değiştirin. Bu, AKS kümesinde konumunu almak için başka bir sistem düğüm havuzunuz olması gerekir.
* Bir Kullanıcı düğümü havuzunu sistem düğüm havuzu olacak şekilde değiştirin.
* Kullanıcı düğümü havuzlarını silin.
* Sistem düğüm havuzlarını, kendi AKS kümesindeki yerini almak için başka bir sistem düğüm havuzunuz olması şartıyla silebilirsiniz.
* AKS kümesinde birden fazla sistem düğüm havuzu olabilir ve en az bir sistem düğüm havuzu gerekir.
* Mevcut düğüm havuzlarındaki çeşitli sabit ayarları değiştirmek istiyorsanız, bunları değiştirmek için yeni düğüm havuzları oluşturabilirsiniz. Bir örnek, yeni bir maxPods ayarı ile yeni bir düğüm havuzu eklemek ve eski düğüm havuzunu silmektir.

## <a name="create-a-new-aks-cluster-with-a-system-node-pool"></a>Sistem düğüm havuzu ile yeni bir AKS kümesi oluşturma

Yeni bir AKS kümesi oluşturduğunuzda, otomatik olarak tek bir düğüm içeren bir sistem düğüm havuzu oluşturursunuz. İlk düğüm havuzu, sistem türü bir mod olarak varsayılan olarak belirlenmiştir. Az aks nodepool Add ile yeni düğüm havuzları oluşturduğunuzda, mod parametresini açıkça belirtmediğiniz müddetçe bu düğüm havuzları Kullanıcı düğümü havuzlarıdır.

Aşağıdaki örnek *eastus* bölgesinde *myresourcegroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

AKS kümesi oluşturmak için [az aks create][az-aks-create] komutunu kullanın. Aşağıdaki örnek, bir düğüm içeren bir sistem havuzu ile *Myakscluster* adlı bir küme oluşturur. Üretim iş yükleriniz için en az üç düğüm ile sistem düğüm havuzlarını kullandığınızdan emin olun. Bu işlemin tamamlanması birkaç dakika sürebilir.

```azurecli-interactive
az aks create -g myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

## <a name="add-a-system-node-pool-to-an-existing-aks-cluster"></a>Mevcut bir AKS kümesine sistem düğüm havuzu ekleme

Varolan AKS kümelerine bir veya daha fazla sistem düğüm havuzu ekleyebilirsiniz. Aşağıdaki komut, varsayılan sayıda üç düğüme sahip bir mod türü sisteminin düğüm havuzunu ekler.

```azurecli-interactive
az aks nodepool add -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```
## <a name="show-details-for-your-node-pool"></a>Düğüm havuzunuzun ayrıntılarını gösterme

Aşağıdaki komutla, düğüm havuzunuzun ayrıntılarını denetleyebilirsiniz.  

```azurecli-interactive
az aks nodepool show -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

Sistem düğüm havuzları için **sistem** türü bir mod tanımlanır ve Kullanıcı düğüm havuzları için **Kullanıcı** türünde bir mod tanımlanır.

```output
{
  "agentPoolType": "VirtualMachineScaleSets",
  "availabilityZones": null,
  "count": 3,
  "enableAutoScaling": null,
  "enableNodePublicIp": false,
  "id": "/subscriptions/666d66d8-1e43-4136-be25-f25bb5de5883/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster/agentPools/mynodepool",
  "maxCount": null,
  "maxPods": 110,
  "minCount": null,
  "mode": "System",
  "name": "mynodepool",
  "nodeLabels": {},
  "nodeTaints": null,
  "orchestratorVersion": "1.15.10",
  "osDiskSizeGb": 100,
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "scaleSetEvictionPolicy": null,
  "scaleSetPriority": null,
  "spotMaxPrice": null,
  "tags": null,
  "type": "Microsoft.ContainerService/managedClusters/agentPools",
  "vmSize": "Standard_DS2_v2",
  "vnetSubnetId": null
}
```

## <a name="update-existing-cluster-system-and-user-node-pools"></a>Mevcut küme sistemini ve Kullanıcı düğümü havuzlarını Güncelleştir

> [!NOTE]
> Bir sistem düğüm havuzu modu ayarlamak için 2020-03-01 veya üzeri bir API sürümü kullanılmalıdır. 2020-03-01 'den eski API sürümlerinde oluşturulan kümeler, sonuç olarak yalnızca Kullanıcı düğümü havuzlarını içerir. Daha eski kümelerde sistem düğüm havuzu işlevselliği ve avantajları almak için, mevcut düğüm havuzlarının modunu en son Azure CLı sürümünde aşağıdaki komutlarla güncelleştirin.

Hem sistem hem de Kullanıcı düğüm havuzlarının modlarını değiştirebilirsiniz. Bir sistem düğüm havuzunu bir Kullanıcı havuzu için, yalnızca başka bir sistem düğüm havuzu zaten varsa, AKS kümesinde değiştirebilirsiniz.

Bu komut, bir sistem düğüm havuzunu bir Kullanıcı düğümü havuzuna değiştirir.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode user
```

Bu komut, bir Kullanıcı düğümü havuzunu bir sistem düğümü havuzuna değiştirir.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```

## <a name="delete-a-system-node-pool"></a>Sistem düğüm havuzunu silme

> [!Note]
> API sürüm 2020-03-02 ' den önceki AKS kümelerinde sistem düğüm havuzlarını kullanmak için yeni bir sistem düğümü havuzu ekleyin, sonra özgün varsayılan düğüm havuzunu silin.

Daha önce bir AKS kümesindeki ilk varsayılan düğüm havuzu olan sistem düğüm havuzunu silemezsiniz. Artık kümelerinizdeki düğüm havuzlarını silme esnekliği vardır. AKS kümeleri en az bir sistem düğüm havuzu gerektirdiğinden, bunlardan birini silebilmeniz için AKS kümenizde en az iki sistem düğüm havuzu olması gerekir.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir AKS kümesinde sistem düğüm havuzlarını oluşturmayı ve yönetmeyi öğrendiniz. Birden çok düğüm havuzu kullanma hakkında daha fazla bilgi için bkz. [birden çok düğüm havuzu kullanma][use-multiple-node-pools].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubernetes-labels]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
[kubernetes-label-syntax]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set

<!-- INTERNAL LINKS -->
[aks-windows]: windows-container-cli.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-list
[az-aks-nodepool-update]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-update
[az-aks-nodepool-upgrade]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-delete
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[gpu-cluster]: gpu-cluster.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[quotas-skus-regions]: quotas-skus-regions.md
[supported-versions]: supported-kubernetes-versions.md
[tag-limitation]: ../azure-resource-manager/resource-group-using-tags.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[vm-sizes]: ../virtual-machines/linux/sizes.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[maximum-pods]: configure-azure-cni.md#maximum-pods-per-node