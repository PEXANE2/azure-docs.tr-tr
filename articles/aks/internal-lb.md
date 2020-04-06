---
title: İç yük dengeleyici oluşturma
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti (AKS) ile hizmetlerinizi ortaya çıkarmak için bir dahili yük dengeleyicisini nasıl oluşturup kullanacağınızı öğrenin.
services: container-service
ms.topic: article
ms.date: 03/04/2019
ms.openlocfilehash: 4decd66a558b031f1aaaf9c64556dae545ed05d3
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668421"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) ile dahili yük dengeleyicisi kullanın

Azure Kubernetes Hizmeti'ndeki (AKS) uygulamalarınız için erişimi kısıtlamak için bir dahili yük dengeleyicisi oluşturabilir ve kullanabilirsiniz. Dahili yük dengeleyicisi, Kubernetes hizmetini yalnızca Kubernetes kümesiyle aynı sanal ağda çalışan uygulamalar için erişilebilir kılar. Bu makalede, Azure Kubernetes Hizmeti (AKS) ile bir dahili yük dengeleyicisi nasıl oluşturulup kullanacağınızı gösterilmektedir.

> [!NOTE]
> Azure Yük Dengeleyicisi, *Temel* ve *Standart*olmak için iki SCARI'de kullanılabilir. Varsayılan olarak, bir AKS kümesi oluşturduğunuzda Standart SKU kullanılır.  LoadBalancer türünde bir Hizmet oluştururken, kümeyi sağlarken aynı LB türünü alırsınız. Daha fazla bilgi için [Azure yük dengeleyiciSKU karşılaştırması][azure-lb-comparison]bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

Varolan bir alt ağ veya kaynak grubu kullanıyorsanız, AKS küme hizmeti ilkesiağ kaynaklarını yönetmek için izin alır. Genel olarak, ağ *katılımcısı* rolünü devredilen kaynaklardaki hizmet yöneticinize atayın. İzinler hakkında daha fazla bilgi için, [aks temsilcisinin diğer Azure kaynaklarına erişimine][aks-sp]bakın.

## <a name="create-an-internal-load-balancer"></a>İç yük dengeleyici oluşturma

Dahili yük dengeleyicisi oluşturmak için, aşağıdaki örnekte gösterildiği gibi `internal-lb.yaml` *LoadBalancer* hizmet türü ve *azure-load-balancer-internal* ek açıklama ile adında bir hizmet bildirimi oluşturun:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

[Kubectl uygulayArak][kubectl-apply] dahili yük dengeleyicisini dağıtın ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f internal-lb.yaml
```

Düğüm kaynak grubunda bir Azure yük dengeleyicisi oluşturulur ve AKS kümesiyle aynı sanal ağa bağlanır.

Hizmet ayrıntılarını görüntülediğinizde, dahili yük dengeleyicisinin IP adresi *EXTERNAL-IP* sütununda gösterilir. Bu bağlamda, *Dış* yük dengeleyicisi dış arabirimi ile ilgili değil, bir ortak, dış IP adresi alır. Ip adresinin aşağıdaki örnekte gösterildiği * \<gibi,\> beklemedurumundan* gerçek bir dahili IP adresine değişmesi bir veya iki dakika sürebilir:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   2m
```

## <a name="specify-an-ip-address"></a>IP adresi belirtin

İç yük dengeleyicisi ile belirli bir IP adresi kullanmak istiyorsanız, yük dengeleyici YAML manifestosuna *loadBalancerIP* özelliğini ekleyin. Belirtilen IP adresi AKS kümesiyle aynı alt ağda yer almalıdır ve zaten bir kaynağa atanmamış olmalıdır.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: internal-app
```

Dağıtıldığında ve hizmet ayrıntılarını görüntülediğinizde, *EXTERNAL-IP* sütunundaki IP adresi belirtilen IP adresinizi yansıtır:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Özel ağları kullanma

AKS kümenizi oluşturduğunuzda, gelişmiş ağ ayarları belirtebilirsiniz. Bu yaklaşım, kümeyi varolan bir Azure sanal ağına ve alt ağlara dağıtmanızı sağlar. Bir senaryo, AKS kümenizi şirket içi ortamınıza bağlı özel bir ağa dağıtmak ve yalnızca dahili olarak erişilebilen hizmetleri çalıştırmaktır. Daha fazla bilgi için, kendi sanal ağ alt ağlarınızı [Kubenet][use-kubenet] veya [Azure CNI][advanced-networking]ile yapılandırın.

Özel ağ kullanan bir AKS kümesinde bir iç yük dengeleyicisi dağıtmak için önceki adımlarda değişiklik gerekmez. Yük dengeleyicisi AKS kümenizle aynı kaynak grubunda oluşturulur, ancak aşağıdaki örnekte gösterildiği gibi özel sanal ağınıza ve alt ağınıza bağlanır:

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> AKS kümeniz için hizmet ilkesini Azure sanal ağ kaynaklarınızın dağıtıldığı kaynak grubuna *Ağ Katılımcısı* rolünü vermeniz gerekebilir. Az [aks gösterisi][az-aks-show]ile servis müdürünü `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`görüntüleyin, gibi . Rol ataması oluşturmak için [az rol ataması oluşturma][az-role-assignment-create] komutunu kullanın.

## <a name="specify-a-different-subnet"></a>Farklı bir alt ağ belirtin

Yük bakiyeniz için bir alt ağ belirtmek için, *azure-load-balancer-internal-subnet* ek açıklamasını hizmetinize ekleyin. Belirtilen alt ağ AKS kümenizle aynı sanal ağda olmalıdır. Dağıtıldığında, yük bakiyesi *external-IP* adresi belirtilen alt ağın bir parçasıdır.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

## <a name="delete-the-load-balancer"></a>Yük bakiyesini silme

Dahili yük dengeleyicisini kullanan tüm hizmetler silindiğinde, yük dengeleyicisinin kendisi de silinir.

Ayrıca, altta yatan Azure yük bakiyesini de silen , herhangi bir Kubernetes kaynağında olduğu gibi `kubectl delete service internal-app`bir hizmeti doğrudan silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes hizmetleri hakkında daha fazla bilgi için [Kubernetes hizmetleri dokümantasyonu.][kubernetes-services]

<!-- LINKS - External -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb-comparison]: ../load-balancer/concepts-limitations.md#skus
[use-kubenet]: configure-kubenet.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
