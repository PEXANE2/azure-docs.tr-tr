---
title: Azure Kubernetes Service (AKS) kümesini yükseltme
description: Azure Kubernetes Hizmeti (AKS) kümesini nasıl yükselteceklerini öğrenin
services: container-service
ms.topic: article
ms.date: 05/31/2019
ms.openlocfilehash: 4520297e83f96f95b10ecafd5af52a913dc5f450
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77621972"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Azure Kubernetes Service (AKS) kümesini yükseltme

Bir AKS kümesinin yaşam döngüsünün bir parçası olarak, genellikle en son Kubernetes sürümüne yükseltmeniz gerekir. En son Kubernetes güvenlik sürümlerini uygulamanız veya en son özellikleri almak için yükseltmeniz önemlidir. Bu makalede, aks kümesinde ana bileşenleri veya tek bir varsayılan düğüm havuzunu nasıl yükseltersiniz gösterilmektedir.

Birden çok düğüm havuzu veya Windows Server düğümleri (şu anda AKS önizlemede) kullanan [AKS][nodepool-upgrade]kümeleri için bkz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLI sürümünü 2.0.65 veya sonraki sürümde çalıştırmanız gerekmektedir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

> [!WARNING]
> AKS küme yükseltmesi düğümlerinizi bir kordon ve drenaj tetikler. Düşük bir işlem kotanız varsa, yükseltme başarısız olabilir. Daha fazla bilgi için [kotaları artırın.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)
> Kendi küme otomatik ölçeklendirici dağıtımı çalıştırıyorsanız, yükseltme işlemiile müdahale edecek bir şans olduğu için yükseltme sırasında lütfen devre dışı (sıfır yinelemeler için ölçeklendirebilirsiniz). Yönetilen otomatik ölçeklendirici bunu otomatik olarak işler. 

## <a name="check-for-available-aks-cluster-upgrades"></a>Kullanılabilir AKS küme yükseltmelerini denetleyin

Kümeniz için hangi Kubernetes sürümlerinin kullanılabilip kullanılabilip bulunmadığını kontrol etmek için [az aks get-upgrades][az-aks-get-upgrades] komutunu kullanın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myAKSCluster* adlı kümedeki kullanılabilir yükseltmeleri denetler:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Bir AKS kümesini yükselttiğinde, Kubernetes küçük sürümleri atlanamaz. Örneğin, *1.12.x* -> *1.13.x* veya *1.13.x* -> *1.14.x* arasındaki yükseltmelere izin verilir, ancak *1.12.x* -> *1.14.x* değildir.
>
> Yükseltmek için, *1.12.x* -> *1.14.x*, ilk yükseltme *1.12.x* -> *1.13.x*, sonra *1.13.x* -> *1.14.x*yükseltme .

Aşağıdaki örnek çıktı kümenin *1.13.9 ve 1.13.10* sürümlerine yükseltilen olabileceğini gösterir: *1.13.10*

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
Yükseltme yoksa, şunları alırsınız:
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="upgrade-an-aks-cluster"></a>AKS kümesini yükseltme

AKS kümeniz için kullanılabilir sürümlerin bir listesi yle, yükseltmek için [az aks yükseltme][az-aks-upgrade] komutunu kullanın. Yükseltme işlemi sırasında AKS, kümeye belirtilen Kubernetes sürümünü çalıştıran yeni bir düğüm ekler, ardından eski düğümlerden birini dikkatlice [kordon altına alıp,][kubernetes-drain] çalışan uygulamalardaki kesintiyi en aza indirmek için boşaltır. Yeni düğüm çalışan uygulama bölmeleri olarak onaylandığında, eski düğüm silinir. Bu işlem, kümedeki tüm düğümler yükseltilene kadar yinelenir.

Aşağıdaki örnek, bir kümeyi *sürüm 1.13.10'a*yükseltir:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.13.10
```

Kümeyi yükseltmek, kaç düğüme sahip olduğunuza bağlı olarak birkaç dakika sürer. 

> [!NOTE]
> Küme yükseltmesi tamamlamak için izin verilen toplam süre vardır. Bu süre ürünü alınarak `10 minutes * total number of nodes in the cluster`hesaplanır. Örneğin, 20 düğüm kümesinde yükseltme işlemleri 200 dakika içinde başarılı olmalıdır veya AKS kurtarılamaz küme durumunu önlemek için işlemi başarısız olur. Yükseltme hatasını kurtarmak için, zaman arızası vurulduktan sonra yükseltme işlemini yeniden deneyin.

Yükseltmenin başarılı olduğunu doğrulamak için [az aks show][az-aks-show] komutunu kullanın:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Aşağıdaki örnek çıktı kümenin şu anda *1.13.10*çalıştığını gösterir:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, varolan bir AKS kümesini nasıl yükseltersiniz gösterin. AKS kümelerini dağıtma ve yönetme hakkında daha fazla bilgi edinmek için öğreticiler kümesine bakın.

> [!div class="nextstepaction"]
> [AKS eğitimleri][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
