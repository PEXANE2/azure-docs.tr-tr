---
title: Azure Kubernetes Hizmeti (AKS) yük dengeleyicisi ile statik ip adresi ve DNS etiketi kullanın
description: Azure Kubernetes Hizmeti (AKS) yük dengeleyicisi ile statik bir IP adresi oluşturmayı ve nasıl kullanacağınızı öğrenin.
services: container-service
ms.topic: article
ms.date: 03/09/2020
ms.openlocfilehash: 6c219976db21fb05ea1ad313b4effdf95906f986
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80047963"
---
# <a name="use-a-static-public-ip-address-and-dns-label-with-the-azure-kubernetes-service-aks-load-balancer"></a>Azure Kubernetes Hizmeti (AKS) yük dengeleyicisi ile statik bir genel IP adresi ve DNS etiketi kullanın

Varsayılan olarak, bir AKS kümesi tarafından oluşturulan bir yük dengeleyici kaynağına atanan genel IP adresi yalnızca bu kaynağın ömrü için geçerlidir. Kubernetes hizmetini silerseniz, ilişkili yük dengeleyicisi ve IP adresi de silinir. Belirli bir IP adresi atamak veya yeniden dağıtılan Kubernetes hizmetleri için bir IP adresi tutmak istiyorsanız, statik bir genel IP adresi oluşturabilir ve kullanabilirsiniz.

Bu makalede, statik bir genel IP adresi oluşturmak ve Kubernetes hizmetine atamak nasıl gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

Bu makale, *Standart* SKU yük dengeleyicisi olan bir *Standart* SKU IP'si kullanılmasını kapsamaktadır. Daha fazla bilgi için [Azure'daki IP adres türlerine ve ayırma yöntemlerine][ip-sku]bakın.

## <a name="create-a-static-ip-address"></a>Statik IP adresi oluşturma

az network public ip oluşturma komutu ile statik bir genel IP adresi [oluşturun.][az-network-public-ip-create] Aşağıdaki *myResourceGroup* kaynak grubunda *myAKSPublicIP* adlı statik bir IP kaynağı oluşturur:

```azurecli-interactive
az network public-ip create \
    --resource-group myResourceGroup \
    --name myAKSPublicIP \
    --sku Standard \
    --allocation-method static
```

> [!NOTE]
> AKS kümenizde *Temel* SKU yük dengeleyicisi kullanıyorsanız, ortak ip tanımlarken *sku* parametresi için *Basic'i* kullanın. Sadece *Temel* SKU IP'leri *Temel* SKU yük dengeleyicisiyle çalışır ve sadece *Standart* SKU IP'leri *Standart* SKU yük dengeleyicileriyle çalışır. 

IP adresi, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi görüntülenir:

```json
{
  "publicIp": {
    ...
    "ipAddress": "40.121.183.52",
    ...
  }
}
```

Daha sonra [az network public-ip listesi][az-network-public-ip-list] komutunu kullanarak ortak IP adresini alabilirsiniz. Oluşturduğunuz düğüm kaynak grubunun adını ve genel IP adresini belirtin ve aşağıdaki örnekte gösterildiği gibi *ipAddress* için sorgu:

```azurecli-interactive
$ az network public-ip show --resource-group myResourceGroup --name myAKSPublicIP --query ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Statik IP adresini kullanarak hizmet oluşturma

Hizmet oluşturmadan önce, AKS kümesi tarafından kullanılan hizmet ilkesinin izinleri diğer kaynak grubuna devrettiğinden emin olun. Örnek:

```azurecli-interactive
az role assignment create \
    --assignee <SP Client ID> \
    --role "Network Contributor" \
    --scope /subscriptions/<subscription id>/resourceGroups/<resource group name>
```

Alternatif olarak, hizmet sorumlusu yerine izinler için yönetilen kimlik atanan sistemi kullanabilirsiniz. Daha fazla bilgi için [bkz.](use-managed-identity.md)

Statik genel IP adresine sahip bir *LoadBalancer* hizmeti oluşturmak için, yaml bildirimine `loadBalancerIP` statik genel IP adresinin özelliğini ve değerini ekleyin. Adlandırılmış `load-balancer-service.yaml` bir dosya oluşturun ve aşağıdaki YAML'de kopyalayın. Önceki adımda oluşturulan kendi genel IP adresinizi sağlayın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubuna ek açıklama da ayarlar. Kendi kaynak grup adınızı sağlayın.

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-resource-group: myResourceGroup
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

`kubectl apply` Komutla hizmet ve dağıtım oluşturun.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="apply-a-dns-label-to-the-service"></a>Hizmete Bir DNS etiketi uygulama

Hizmetiniz dinamik veya statik bir genel IP adresi kullanıyorsa, `service.beta.kubernetes.io/azure-dns-label-name` genel kullanıma açık bir DNS etiketi ayarlamak için hizmet ek açıklamasını kullanabilirsiniz. Bu, Azure'un genel DNS sunucularını ve üst düzey etki alanını kullanarak hizmetiniz için tam nitelikli bir etki alanı adı yayımlar. Ek açıklama değeri Azure konumu içinde benzersiz olmalıdır, bu nedenle yeterince nitelikli bir etiket kullanılması önerilir.   

Azure daha sonra, tam nitelikli DNS `<location>.cloudapp.azure.com` adını oluşturmak için sağladığınız ada (seçtiğiniz bölge konumu) gibi varsayılan bir alt ağı otomatik olarak ekler. Örnek:

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-dns-label-name: myserviceuniquelabel
  name: azure-load-balancer
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

> [!NOTE] 
> Hizmeti kendi etki alanınızda yayımlamak için [Azure DNS][azure-dns-zone] ve [harici dns][external-dns] projesine bakın.

## <a name="troubleshoot"></a>Sorun giderme

Kubernetes hizmet bildiriminin *loadBalancerIP* özelliğinde tanımlanan statik IP adresi yoksa veya düğüm kaynak grubunda oluşturulmamadıysa ve yapılandırılmatılmamışsa, yük dengeleyici hizmet oluşturma başarısız olur. Sorun gidermek [için, kubectl açıklaykomutu][kubectl-describe] ile hizmet oluşturma olaylarını gözden geçirin. Aşağıdaki örnekte gösterildiği gibi, HIZMETin adını YAML bildiriminde belirtildiği şekilde sağlayın:

```console
kubectl describe service azure-load-balancer
```

Kubernetes hizmet kaynağı hakkındaki bilgiler görüntülenir. Aşağıdaki örnek çıktının sonundaki *Olaylar,* *kullanıcının verilen IP Adresinin bulunamadı.* Bu senaryolarda, düğüm kaynak grubunda statik genel IP adresini oluşturduğunuzu ve Kubernetes hizmet bildiriminde belirtilen IP adresinin doğru olduğunu doğrulayın.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınıza ağ trafiği üzerinde ek denetim için, bunun yerine [bir giriş denetleyicisi oluşturmak][aks-ingress-basic]isteyebilirsiniz. Statik [genel IP adresine sahip bir giriş denetleyicisi][aks-static-ingress]de oluşturabilirsiniz.

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[azure-dns-zone]: https://azure.microsoft.com/services/dns/
[external-dns]: https://github.com/kubernetes-sigs/external-dns

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ip-sku]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md#sku
