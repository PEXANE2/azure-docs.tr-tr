---
title: Azure Kubernetes Hizmetinde (AKS) sistem düğümü havuzlarını kullanma
description: Azure Kubernetes Hizmeti'nde (AKS) sistem düğümü havuzları oluşturma ve yönetme yi öğrenin
services: container-service
ms.topic: article
ms.date: 04/06/2020
ms.openlocfilehash: b567d9e618877463e1e659f368d35fbb787a4ef2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259077"
---
# <a name="manage-system-node-pools-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) sistem düğümü havuzlarını yönetme

Azure Kubernetes Hizmeti'nde (AKS), aynı yapılandırmadaki düğümler düğüm *havuzlarında*gruplandırılır. Düğüm havuzları, uygulamalarınızı çalıştıran temel VM'leri içerir. Sistem düğümü havuzları ve kullanıcı düğümü havuzları AKS kümeleriniz için iki farklı düğüm havuzu modudur. Sistem düğüm havuzları CoreDNS ve tünel başı gibi kritik sistem bölmelerini barındırmanın birincil amacına hizmet eder. Kullanıcı düğümü havuzları, uygulama bölmelerinizi barındırmanın birincil amacına hizmet eder. Ancak, AKS kümenizde yalnızca bir havuz olmasını istiyorsanız, uygulama bölmeleri sistem düğümü havuzlarında zamanlanabilir. Her AKS kümesi en az bir düğüm ile en az bir sistem düğümü havuzu içermelidir. 

> [!Important]
> Bir üretim ortamında AKS kümeniz için tek bir sistem düğüm havuzu çalıştırıyorsanız, düğüm havuzu için en az üç düğüm kullanmanızı öneririz.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure CLI sürüm 2.3.1 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="limitations"></a>Sınırlamalar

Sistem düğüm havuzlarını destekleyen AKS kümeleri oluştururken ve yönetirken aşağıdaki sınırlamalar geçerlidir.

* [Azure Kubernetes Hizmetinde (AKS) Kotalara, sanal makine boyutu kısıtlamalarına ve bölge kullanılabilirliğine][quotas-skus-regions]bakın.
* AKS kümesi VM türü olarak sanal makine ölçek kümeleri ile oluşturulmalıdır.
* Düğüm havuzunun adı yalnızca küçük alfasayısal karakterler içerebilir ve küçük harfle başlamalıdır. Linux düğümü havuzları için uzunluk 1 ile 12 karakter arasında olmalıdır. Windows düğümü havuzları için uzunluk 1 ile 6 karakter arasında olmalıdır.

## <a name="system-and-user-node-pools"></a>Sistem ve kullanıcı düğümü havuzları

Sistem düğümü havuz düğümlerinin her birinde kubernetes.azure.com/mode etiketi **vardır: sistem.** Her AKS kümesi en az bir sistem düğüm havuzu içerir. Sistem düğümü havuzları aşağıdaki kısıtlamalara sahiptir:

* Sistem havuzları osType Linux olmalıdır.
* Kullanıcı düğümü havuzları osType Linux veya Windows olabilir.
* Sistem havuzları en az bir düğüm içermelidir ve kullanıcı düğümü havuzları sıfır veya daha fazla düğüm içerebilir.
* Sistem düğümü havuzları en az 2 vCPUs vm SKU ve 4GB bellek gerektirir.
* Sistem düğümü [havuzları, bölmeler için minimum ve maksimum değer formülüyle][maximum-pods]açıklandığı şekilde en az 30 bölmeyi desteklemelidir.
* Spot düğüm havuzları kullanıcı düğümü havuzları gerektirir.

Düğüm havuzları ile aşağıdaki işlemleri yapabilirsiniz:

* AKS kümesinde yerini alacak başka bir sistem düğüm havuzuna sahip olmak koşuluyla, bir sistem düğümü havuzunu kullanıcı düğümü havuzu olarak değiştirin.
* Kullanıcı düğümü havuzunu sistem düğümü havuzu olarak değiştirin.
* Kullanıcı düğümü havuzlarını silin.
* AKS kümesinde yerini alacak başka bir sistem düğüm havuzuna sahip seniz, sistem düğüm havuzlarını silebilirsiniz.
* Bir AKS kümesinde birden çok sistem düğümü havuzu olabilir ve en az bir sistem düğümü havuzu gerekir.

## <a name="create-a-new-aks-cluster-with-a-system-node-pool"></a>Sistem düğüm havuzu olan yeni bir AKS kümesi oluşturma

Yeni bir AKS kümesi oluşturduğunuzda, otomatik olarak tek bir düğüm ile bir sistem düğüm havuzu oluşturursunuz. İlk düğüm havuzu varsayılan olarak bir tür sistemi moduna geçer. Az aks nodepool eklenerek yeni düğüm havuzları oluşturduğunuzda, mod parametresini açıkça belirtmediğiniz sürece bu düğüm havuzları kullanıcı düğümü havuzlarıdır.

Aşağıdaki örnek, *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

AKS kümesi oluşturmak için [az aks create][az-aks-create] komutunu kullanın. Aşağıdaki örnek, bir düğüm içeren bir sistem havuzu ile *myAKSCluster* adlı bir küme oluşturur. Üretim iş yüklerin için, en az üç düğümiçeren sistem düğüm havuzları kullandığınızdan emin olun. Bu işlemin tamamlanması birkaç dakika sürebilir.

```azurecli-interactive
az aks create -g myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

## <a name="add-a-system-node-pool-to-an-existing-aks-cluster"></a>Varolan bir AKS kümesine sistem düğümü havuzu ekleme

Varolan AKS kümelerine bir veya daha fazla sistem düğümü havuzu ekleyebilirsiniz. Aşağıdaki komut, varsayılan üç düğüm sayısına sahip mod türü sistemi bir düğüm havuzu ekler.

```azurecli-interactive
az aks nodepool add -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```
## <a name="show-details-for-your-node-pool"></a>Düğüm havuzunuzun ayrıntılarını göster

Düğüm havuzunuzun ayrıntılarını aşağıdaki komutla kontrol edebilirsiniz.  

```azurecli-interactive
az aks nodepool show -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

Sistem düğüm havuzları için bir tür **Sistemi** modu tanımlanır ve kullanıcı düğümü havuzları için kullanıcı **tipi** bir mod tanımlanır.

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

## <a name="update-system-and-user-node-pools"></a>Sistem ve kullanıcı düğümü havuzlarını güncelleştirme

Hem sistem hem de kullanıcı düğümü havuzları için modları değiştirebilirsiniz. Aks kümesinde zaten başka bir sistem düğüm havuzu varsa, sistem düğümü havuzunu kullanıcı havuzuolarak değiştirebilirsiniz.

Bu komut, bir sistem düğümü havuzunu kullanıcı düğümü havuzuna değiştirir.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode user
```

Bu komut, bir kullanıcı düğümü havuzunu sistem düğümü havuzuna değiştirir.

```azurecli-interactive
az aks nodepool update -g myResourceGroup --cluster-name myAKSCluster -n mynodepool --mode system
```

## <a name="delete-a-system-node-pool"></a>Sistem düğümü havuzuni silme

> [!Note]
> API sürüm 2020-03-02'den önce AKS kümelerinde sistem düğümü havuzlarını kullanmak için yeni bir sistem düğüm havuzu ekleyin ve özgün varsayılan düğüm havuzunu silin.

Daha önce, aks kümesindeki ilk varsayılan düğüm havuzu olan sistem düğümü havuzunu silemiyordunuz. Artık kümelerinizdeki düğüm havuzunu silme esnekliğine sahipsiniz. AKS kümeleri en az bir sistem düğüm havuzu gerektirdiğinden, bunlardan birini silemeden önce AKS kümenizde en az iki sistem düğüm havuzu olmalıdır.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster -n mynodepool
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, aks kümesinde sistem düğümü havuzlarının nasıl oluşturulup yönetileceği öğrenildi. Birden çok düğüm havuzunun nasıl kullanılacağı hakkında daha fazla bilgi için [bkz.][use-multiple-node-pools]

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
[maximum-pods]: faq.md#why-cant-i-set-maxpods-below-30