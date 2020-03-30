---
title: Azure Kubernetes Service (AKS) kümesini ölçeklendirme
description: Azure Kubernetes Hizmeti (AKS) kümesindeki düğüm sayısını nasıl ölçeklendireceklerini öğrenin.
services: container-service
author: iainfoulds
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: 55d7a00a0a8c0b655f06810f8bcea7126bb9167f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368426"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Azure Kubernetes Service (AKS) kümesindeki düğüm sayısını ölçeklendirme

Uygulamalarınızın kaynak gereksinimleri değişirse, farklı sayıda düğüm çalıştırmak için bir AKS kümesini el ile ölçeklendirebilirsiniz. Küçülttüldüğünde, düğümler dikkatle kordon altına alınır ve çalışan uygulamaların aksamasını en aza indirmek için [boşaltılır.][kubernetes-drain] Ölçeklendirdiğinizde, AKS düğümleri bölmeler `Ready` zamanlanmadan önce Kubernetes kümesi tarafından işaretlenene kadar bekler.

## <a name="scale-the-cluster-nodes"></a>Küme düğümlerini ölçeklendirme

İlk olarak, [az aks show][az-aks-show] komutunu kullanarak düğüm havuzunuzun *adını* alın. Aşağıdaki örnek, *myResourceGroup* kaynak grubunda *myAKSCluster* adlı kümenin düğüm havuzu adını alır:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

Aşağıdaki örnek *çıktı, adın* *nodepool1*olduğunu gösterir:

```output
[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

Küme düğümlerini ölçeklendirmek için [az aks ölçeği][az-aks-scale] komutunu kullanın. Aşağıdaki örnek, *myAKSCluster* adlı bir kümeyi tek bir düğüme ölçeklendirür. Önceki komuttan *nodepool1*gibi kendi *--nodepool-adınızı* sağlayın:

```azurecli-interactive
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

Aşağıdaki örnek çıktı, kümenin *agentPoolProfiles* bölümünde gösterildiği gibi başarıyla tek bir düğüme ölçeklendirildini gösterir:

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  [...]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, düğüm sayısını artırmak veya azaltmak için bir AKS kümesini el ile ölçeklendirin. Kümenizi otomatik olarak ölçeklendirmek için [küme otomatik ölçeklendiricisini][cluster-autoscaler] de kullanabilirsiniz.

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[cluster-autoscaler]: cluster-autoscaler.md
