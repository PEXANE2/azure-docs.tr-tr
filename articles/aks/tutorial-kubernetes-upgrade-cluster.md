---
title: Azure’da Kubernetes öğreticisi - Kümeyi yükseltme
description: Bu Azure Kubernetes Service (AKS) öğreticisinde var olan bir AKS kümesini en son Kubernetes sürümüne yükseltmeyi öğreneceksiniz.
services: container-service
ms.topic: tutorial
ms.date: 02/25/2020
ms.custom: mvc
ms.openlocfilehash: 22aad0e601c600e582cbea0cea82dd67a20a2c06
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392685"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Öğretici: Azure Kubernetes Hizmeti’nde (AKS) Kubernetes’i yükseltme

Uygulama ve küme yaşam döngüsünün bir parçası olarak Kubernetes'in son sürümüne yükselterek yeni özelliklerden faydalanmak isteyebilirsiniz. Azure Kubernetes Hizmeti (AKS) kümesi, Azure CLI kullanılarak yükseltilebilir.

Yedi parçalık bu öğreticinin yedinci kısmında, bir Kubernetes kümesi yükseltilir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Geçerli ve kullanılabilir Kubernetes sürümlerini tanımlama
> * Kubernetes düğümlerini yükseltme
> * Başarılı bir yükseltmeyi doğrulama

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlenmişti. Bu resim Azure Kapsayıcı Kayıt Defteri'ne yüklendi ve bir AKS kümesi oluşturdunuz. Uygulama daha sonra AKS kümesine dağıtıldı. Bu adımları yapmadıysanız ve takip etmek istiyorsanız, Tutorial 1 ile başlayın [– Kapsayıcı görüntüleri oluşturun.][aks-tutorial-prepare-app]

Bu öğretici, Azure CLI sürümünü 2.0.53 veya daha sonra çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="get-available-cluster-versions"></a>Kullanılabilir küme sürümlerini alma

Bir kümeyi yükseltmeden önce, [az aks get-upgrades][] komutunu kullanarak hangi Kubernetes sürümlerinin yükseltme için kullanılabilir olduğunu öğrenin:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

Aşağıdaki örnekte, geçerli sürüm *1.14.8'dir*ve kullanılabilir *sürümler Yükseltmeler* sütunu altında gösterilir.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  --------------
default  myResourceGroup  1.14.8           1.14.8             1.15.5, 1.15.7
```

## <a name="upgrade-a-cluster"></a>Kümeyi yükseltme

Çalışan uygulamalardaki aksamaları en aza indirmek için AKS düğümleri dikkatlice kordon altına alınır ve boşaltılır. Bu işlemde, aşağıdaki adımlar gerçekleştirilir:

1. Kubernetes zamanlayıcısı yükseltilecek bir düğüm üzerinde zamanlanan ek bölmeleri önler.
1. Düğümüzerinde çalışan bölmeler kümedeki diğer düğümlerde zamanlanır.
1. En son Kubernetes bileşenlerini çalıştıran bir düğüm oluşturulur.
1. Yeni düğüm hazır olduğunda ve kümeye birleştiğinde, Kubernetes zamanlayıcısı bölmeleri çalıştırmaya başlar.
1. Eski düğüm silinir ve kümedeki bir sonraki düğüm kordon ve boşaltma işlemini başlatır.

AKS kümesini yükseltmek için [az aks upgrade][] komutunu kullanın. Aşağıdaki örnek kümeyi Kubernetes sürüm *1.14.6'ya*yükseltir.

> [!NOTE]
> Aynı anda yalnızca bir ikincil sürüm yükseltmesi yapabilirsiniz. Örneğin, *1.14.x'ten* *1.15.x'e*yükseltebilirsiniz, ancak *doğrudan 1.14.x'ten* *1.16.x'e* yükseltemezsiniz. *1.14.x'ten* *1.16.x'e*yükseltmek için, önce *1.14.x'ten* *1.15.x'e*yükseltme, sonra *1.15.x'ten* *1.16.x'e*başka bir yükseltme gerçekleştirin.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.15.5
```

Aşağıdaki yoğunlaştırılmış örnek çıktı *kubernetesVersion* şimdi *1.15.5*raporları gösterir:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.15.5",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-an-upgrade"></a>Yükseltmeyi doğrulama

[az aks show][] komutunu aşağıdaki şekilde kullanarak yükseltmenin başarılı olup olmadığını doğrulayabilirsiniz:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Aşağıdaki örnek çıktı AKS küme *KubernetesSürüm 1.15.5*çalışır gösterir:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.15.5               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Küme silme

Bu öğretici serinin son bölümü olduğundan, AKS kümesini silmek isteyebilirsiniz. Kubernetes düğümleri Azure sanal makinelerinde (VM) çalıştığından kümeyi kullanmasanız dahi ücret tahsil edilmeye devam eder. [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve herhangi bir sırrı sağlamanızı veya döndürmenizi gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir AKS kümesinde Kubernetes’i yükselttiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Geçerli ve kullanılabilir Kubernetes sürümlerini tanımlama
> * Kubernetes düğümlerini yükseltme
> * Başarılı bir yükseltmeyi doğrulama

AKS hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [AKS'ye genel bakış][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az-group-delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations
