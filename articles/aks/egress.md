---
title: Çıkış trafiği için statik IP'yi kullanma
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti (AKS) kümesinde çıkış trafiği için statik bir genel IP adresi oluşturma ve kullanma
services: container-service
ms.topic: article
ms.date: 03/04/2019
ms.openlocfilehash: 08a9682434605fffde73c835e7a9e9d6971d7ff0
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80803391"
---
# <a name="use-a-static-public-ip-address-for-egress-traffic-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) çıkış trafiği için statik bir genel IP adresi kullanın

Varsayılan olarak, bir Azure Kubernetes Hizmeti (AKS) kümesinden gelen çıkış IP adresi rasgele atanır. Bu yapılandırma, örneğin harici hizmetlere erişim için bir IP adresi belirlemeniz gerektiğinde ideal değildir. Bunun yerine, hizmet erişimi için beyaz listeye alınabilecek statik bir IP adresi atamanız gerekebilir.

Bu makalede, bir AKS kümesinde çıkış trafiği ile kullanmak için statik bir genel IP adresi oluşturmak ve kullanmak nasıl gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="egress-traffic-overview"></a>Çıkış trafiğine genel bakış

Bir AKS kümesinden giden [trafik, Azure Yük Dengeleyici kurallarını][outbound-connections]izler. İlk Kubernetes türünün `LoadBalancer` hizmeti oluşturulmadan önce, AKS kümesindeki aracı düğümleri herhangi bir Azure Yük Dengeleyici havuzunun parçası değildir. Bu yapılandırmada, düğümlerin genel IP adresi yoktur. Azure, giden akışı, yapılandırılabilir veya deterministik olmayan ortak kaynaklı bir IP adresine çevirir.

Bir Kubernetes türü `LoadBalancer` hizmeti oluşturulduktan sonra, aracı düğümleri Bir Azure Yük Dengeleyici havuzuna eklenir. Giden akış için Azure, bunu yük dengeleyicisi üzerinde yapılandırılan ilk genel IP adresine çevirir. Bu genel IP adresi yalnızca bu kaynağın ömrü için geçerlidir. Kubernetes LoadBalancer hizmetini silerseniz, ilişkili yük dengeleyicisi ve IP adresi de silinir. Belirli bir IP adresi atamak veya yeniden dağıtılan Kubernetes hizmetleri için bir IP adresi tutmak istiyorsanız, statik bir genel IP adresi oluşturabilir ve kullanabilirsiniz.

## <a name="create-a-static-public-ip"></a>Statik genel IP oluşturma

[Az aks show][az-aks-show] komutu ile kaynak grup `--query nodeResourceGroup` adını alın ve sorgu parametresini ekleyin. Aşağıdaki örnek, *myResourceGroup*kaynak grubu adı aks küme adı *myAKSCluster* için düğüm kaynak grubu alır:

```azurecli-interactive
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Şimdi az ağ ortak ip oluşturma komutu ile statik bir ortak IP adresi [oluşturun.][az-network-public-ip-create] Önceki komutta elde edilen düğüm kaynak grubu adını ve ardından *myAKSPublicIP*gibi IP adresi kaynağının adını belirtin:

```azurecli-interactive
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

IP adresi, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi gösterilir:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
```

Daha sonra [az network public-ip listesi][az-network-public-ip-list] komutunu kullanarak ortak IP adresini alabilirsiniz. Düğüm kaynak grubunun adını belirtin ve ardından aşağıdaki örnekte gösterildiği gibi *ipAddress* için sorgulayın:

```azurecli-interactive
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-with-the-static-ip"></a>Statik IP ile hizmet oluşturma

Statik genel IP adresine sahip bir `loadBalancerIP` hizmet oluşturmak için, yaml bildirimine özelliği ve statik genel IP adresinin değerini ekleyin. Adlandırılmış `egress-service.yaml` bir dosya oluşturun ve aşağıdaki YAML'de kopyalayın. Önceki adımda oluşturulan kendi genel IP adresinizi sağlayın.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-egress
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
```

`kubectl apply` Komutla hizmet ve dağıtım oluşturun.

```console
kubectl apply -f egress-service.yaml
```

Bu hizmet, Azure Yük Dengeleyicisi'nde yeni bir ön uç IP'si yapılandırır. Başka bir IP yapılandırmanız yoksa, **tüm** çıkış trafiği artık bu adresi kullanmalıdır. Azure Yük Dengeleyicisi'nde birden çok adres yapılandırıldığında, çıkış bu yük dengeleyicisindeki ilk IP'yi kullanır.

## <a name="verify-egress-address"></a>Çıkış adresini doğrula

Statik genel IP adresinin kullanıldığını doğrulamak için `checkip.dyndns.org`DNS arama hizmetini kullanabilirsiniz.

Temel bir *Debian* bölmesini başlatın ve iliştirin:

```console
kubectl run -it --rm aks-ip --image=debian --generator=run-pod/v1
```

Bir web sitesine kapsayıcının içinden `apt-get` erişmek `curl` için kapsayıcıya yüklemek için kullanın.

```console
apt-get update && apt-get install curl -y
```

Şimdi *checkip.dyndns.org* sitesine erişmek için kıvırma kullanın. Çıkış IP adresi, aşağıdaki örnek çıktıda gösterildiği gibi gösterilir. Bu IP adresi, loadBalancer hizmeti için oluşturulan ve tanımlanan statik genel IP adresiyle eşleşir:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 40.121.183.52</body></html>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Yük Dengeleyicisi'nde birden çok genel IP adresi nin korunmasını önlemek için, bunun yerine bir giriş denetleyicisi kullanabilirsiniz. Giriş denetleyicileri, SSL/TLS sonlandırma, URI yeniden yazma desteği ve yukarı SSL/TLS şifreleme gibi ek avantajlar sağlar. Daha fazla bilgi için [aks'ta temel bir giriş denetleyicisi oluşturma'ya][ingress-aks-cluster]bakın.

<!-- LINKS - internal -->
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[azure-cli-install]: /cli/azure/install-azure-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
