---
title: Azure Kubernetes Service (AKS) içinde standart bir SKU yük dengeleyici kullanma
description: Azure Kubernetes Service (AKS) ile hizmetlerinizi kullanıma sunmak için standart SKU ile yük dengeleyiciyi nasıl kullanacağınızı öğrenin.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 09/27/2019
ms.author: zarhoads
ms.openlocfilehash: 55ded9a733baaac7fbc78621bd625d57d1d37ad1
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72255486"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içinde standart bir SKU yük dengeleyici kullanma

Azure Kubernetes Service (AKS) içindeki uygulamalarınıza erişim sağlamak için bir Azure Load Balancer oluşturup kullanabilirsiniz. AKS üzerinde çalışan bir yük dengeleyici, iç veya dış yük dengeleyici olarak kullanılabilir. İç yük dengeleyici, bir Kubernetes hizmetini yalnızca AKS kümesi ile aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Dış yük dengeleyici, giriş için bir veya daha fazla genel IP alır ve genel IP 'Leri kullanarak bir Kubernetes hizmetini dışarıdan erişilebilir hale getirir.

Azure Load Balancer, *temel* ve *Standart*olmak üzere iki SKU 'da kullanılabilir. Varsayılan olarak, AKS üzerinde bir yük dengeleyici oluşturmak için bir hizmet bildirimi kullanıldığında *temel* SKU kullanılır. *Standart* SKU yük dengeleyici kullanmak, daha büyük arka uç havuz boyutu ve kullanılabilirlik alanları gibi ek özellikler ve işlevler sağlar. Kullanmayı seçmeden önce *Standart* ve *temel* yük dengeleyiciler arasındaki farkları anlamanız önemlidir. Bir AKS kümesi oluşturduktan sonra, bu küme için yük dengeleyici SKU 'sunu değiştiremezsiniz. *Temel* ve *Standart* SKU 'lar hakkında daha fazla bilgi için bkz. [Azure yük dengeleyici SKU karşılaştırması][azure-lb-comparison].

Bu makalede, Azure Kubernetes Service (AKS) ile *Standart* SKU ile bir Azure Load Balancer oluşturma ve kullanma işlemlerinin nasıl yapılacağı gösterilir.

Bu makalede, Kubernetes ve Azure Load Balancer kavramlarının temel bir şekilde anlaşıldığı varsayılır. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts] ve [Azure Load Balancer nedir?][azure-lb].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.74 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` ' yı çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

## <a name="before-you-begin"></a>Başlamadan önce

Mevcut bir alt ağ veya kaynak grubu kullanıyorsanız AKS kümesi hizmet sorumlusu ağ kaynaklarını yönetmek için izne ihtiyaç duyuyor. Genel olarak, temsilcili kaynaklar üzerindeki hizmet sorumlusu rolüne *ağ katılımcısı* rolünü atayın. İzinler hakkında daha fazla bilgi için bkz. [diğer Azure kaynaklarına AKS erişimi verme][aks-sp].

Varsayılan *temel*yerine yük dengeleyici için SKU 'yu *Standart* olarak ayarlayan bir aks kümesi oluşturmanız gerekir.

### <a name="limitations"></a>Sınırlamalar

*Standart* SKU ile yük dengeleyiciyi destekleyen aks kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* AKS kümesinden giden trafiğe izin vermek için en az bir genel IP veya IP öneki gereklidir. Genel IP veya IP öneki Ayrıca, denetim düzlemi ve aracı düğümleri arasındaki bağlantıyı sürdürmek için ve önceki AKS sürümleriyle uyumluluğu sürdürmek için de gereklidir. *Standart* bir SKU yük dengeleyiciye sahip genel IP 'LERI veya IP öneklerini belirtmek için aşağıdaki seçenekleriniz vardır:
    * Kendi genel IP 'nizi sağlayın.
    * Kendi genel IP öneklerinizi sağlayın.
    * AKS kümesinin, genellikle *Mc_* olarak adlandırılan aks kümesi olarak oluşturulan aynı kaynak grubunda bulunan çok sayıda *Standart* SKU genel IP 'sini oluşturmasına izin vermek için 100 kadar bir sayı belirtin. AKS, genel IP 'yi *Standart* SKU yük dengeleyicisine atar. Varsayılan olarak, genel IP, genel IP öneki veya IP sayısı belirtilmemişse, AKS kümesiyle aynı kaynak grubunda bir genel IP otomatik olarak oluşturulur. Ayrıca, genel adreslere izin vermeniz ve IP oluşturma ile ilgili herhangi bir Azure Ilkesinin oluşturulmasını önetmeniz gerekir.
* Yük Dengeleyici için *Standart* SKU kullanırken, Kubernetes sürüm 1,13 veya üstünü kullanmanız gerekir.
* Yük dengeleyici SKU 'SU tanımlama yalnızca bir AKS kümesi oluşturduğunuzda yapılabilir. Bir AKS kümesi oluşturulduktan sonra yük dengeleyici SKU 'sunu değiştiremezsiniz.
* Tek bir kümede yalnızca bir yük dengeleyici SKU 'SU kullanabilirsiniz.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluşturduğunuzda bir konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure 'da da çalıştığı yerdir. [Az Group Create][az-group-create] komutunu kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Aşağıdaki örnek çıktıda başarıyla oluşturulan kaynak grubu gösterilmektedir:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-aks-cluster"></a>AKS kümesi oluşturma
*Standart* SKU ile yük dengeleyiciyi destekleyen bir aks kümesini çalıştırmak için, kümenizin *yük dengeleyici-SKU* parametresini *Standart*olarak ayarlaması gerekir. Bu parametre, kümeniz oluşturulduğunda *Standart* SKU ile bir yük dengeleyici oluşturur. Kümenizde bir *LoadBalancer* hizmeti çalıştırdığınızda, *Standart* SKU yük dengeleyicinin yapılandırması hizmetin yapılandırmasıyla güncelleştirilir. *Myakscluster*adlı bir aks kümesi oluşturmak için [az aks Create][az-aks-create] komutunu kullanın.

> [!NOTE]
> *Load-dengeleyici-SKU* özelliği yalnızca kümeniz oluşturulduğunda kullanılabilir. Bir AKS kümesi oluşturulduktan sonra yük dengeleyici SKU 'sunu değiştiremezsiniz. Ayrıca, tek bir kümede Yük dengeleyici SKU 'SU yalnızca bir türünü kullanabilirsiniz.
> 
> Kendi genel IP 'nizi kullanmak istiyorsanız, *yük dengeleyici-giden-IP 'leri*veya *yük dengeleyici-giden-IP-önek* parametrelerini kullanın. Bu parametrelerin her ikisi de [küme güncelleştirilirken](#optional---provide-your-own-public-ips-or-prefixes-for-egress)kullanılabilir.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON biçimli bilgileri döndürür.

## <a name="connect-to-the-cluster"></a>Kümeye Bağlan

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. Azure Cloud Shell kullanırsanız, `kubectl` zaten yüklüdür. @No__t-0 ' ı yerel olarak yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

@No__t, Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenizin bağlantısını doğrulamak için [kubectl Get][kubectl-get] komutunu kullanarak küme düğümlerinin bir listesini döndürün.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnek çıktıda, önceki adımlarda oluşturulan tek düğüm gösterilmektedir. Düğüm *durumunun olduğundan emin olun:*

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.13.10
```

## <a name="verify-your-cluster-uses-the-standard-sku"></a>Kümenizin *Standart* SKU 'yu kullandığını doğrulama

Kümenizin yapılandırmasını görüntülemek için [az aks Show][az-aks-show] ' i kullanın.

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster

{
  "aadProfile": null,
  "addonProfiles": null,
   ...
   "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerSku": "standard",
    ...
```

*Loadbalancersku* özelliğinin *Standart*olarak göründüğünü doğrulayın.

## <a name="use-the-load-balancer"></a>Yük dengeleyiciyi kullanma

Kümenizde yük dengeleyiciyi kullanmak için hizmet türü *LoadBalancer*ile bir hizmet bildirimi oluşturun. Çalışan yük dengeleyiciyi göstermek için, kümenizde çalışacak örnek bir uygulamayla başka bir bildirim oluşturun. Bu örnek uygulama, yük dengeleyici aracılığıyla sunulur ve bir tarayıcı aracılığıyla görüntülenebilir.

Aşağıdaki örnekte gösterildiği gibi `sample.yaml` adlı bir bildirim oluşturun:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
```

Yukarıdaki bildirim iki dağıtımı yapılandırır: *Azure-oy-ön* ve *Azure-oy geri*. *Azure-oyön* dağıtımını yük dengeleyici kullanılarak sunulacak şekilde yapılandırmak için, aşağıdaki örnekte gösterildiği gibi `standard-lb.yaml` adlı bir bildirim oluşturun:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

*Azure-oy-Front* hizmeti, aks kümenizdeki yük dengeleyiciyi *Azure-oy önleyicisi* dağıtımına bağlanacak şekilde yapılandırmak için *LoadBalancer* türünü kullanır.

[Kubectl Apply][kubectl-apply] kullanarak örnek uygulamayı ve yük dengeleyiciyi dağıtın ve YAML bildirimlerin adını belirtin:

```console
kubectl apply -f sample.yaml
kubectl apply -f standard-lb.yaml
```

*Standart* SKU yük dengeleyici artık örnek uygulamayı kullanıma sunmak üzere yapılandırılmıştır. Yük dengeleyicinin genel IP 'sini görmek için [kubectl Get][kubectl-get] kullanarak *Azure-oy-Front* hizmeti ayrıntılarını görüntüleyin. Yük dengeleyicinin genel IP adresi, *dış IP* sütununda gösterilir. Aşağıdaki örnekte gösterildiği gibi, IP adresinin *\<pending @ no__t-2* ' den gerçek bır dış IP adresine değiştirmesi bir veya iki dakika sürebilir:

```
$ kubectl get service azure-vote-front

NAME                TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
azure-vote-front    LoadBalancer   10.0.227.198   52.179.23.131   80:31201/TCP   16s
```

Tarayıcıda genel IP 'ye gidin ve örnek uygulamayı gördiğinizi doğrulayın. Yukarıdaki örnekte, genel IP `52.179.23.131` ' dır.

![Azure oylamaya göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-voting-application.png)

> [!NOTE]
> Ayrıca yük dengeleyiciyi iç olarak yapılandırabilir ve genel bir IP 'yi kullanıma sunmamalısınız. Yük dengeleyiciyi iç olarak yapılandırmak için, *LoadBalancer* hizmetine ek açıklama olarak `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` ekleyin. [Burada][internal-lb-yaml]bir YAML bildiriminin yanı sıra bir iç yük dengeleyici hakkında daha fazla ayrıntı görebilirsiniz.

## <a name="optional---scale-the-number-of-managed-public-ips"></a>İsteğe bağlı-yönetilen genel IP sayısını ölçeklendirme

Varsayılan olarak oluşturulan, yönetilen giden genel IP 'Ler ile *Standart* SKU yük dengeleyicisi kullanırken, *yük dengeleyici-yönetilen-IP-Count* parametresini kullanarak yönetilen giden genel IP sayısını ölçeklendirebilirsiniz.

Var olan bir kümeyi güncelleştirmek için aşağıdaki komutu çalıştırın. Bu parametre, birden çok yönetilen giden genel IP 'si olması için küme oluşturma zamanında de ayarlanabilir.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

Yukarıdaki örnek, *Myresourcegroup*Içindeki *Myakscluster* kümesi Için yönetilen giden genel IP sayısını *2* ' ye ayarlar. 

Ayrıca, `--load-balancer-managed-outbound-ip-count` parametresini ekleyerek ve istediğiniz değere ayarlayarak kümenizi oluştururken yönetilen giden genel IP 'lerin ilk sayısını ayarlamak için *yük dengeleyici-yönetilen-IP-Count* parametresini de kullanabilirsiniz. Varsayılan yönetilen giden genel IP sayısı 1 ' dir.

## <a name="optional---provide-your-own-public-ips-or-prefixes-for-egress"></a>İsteğe bağlı-çıkış için kendi genel IP 'Leri veya ön eklerini sağlayın

*Standart* SKU yük dengeleyicisi kullanılırken aks kümesi, aks kümesi için oluşturulan aynı kaynak grubunda ortak IP 'yi otomatik olarak oluşturur ve genel IP 'yi *Standart* SKU yük dengeleyicisine atar. Alternatif olarak, küme oluşturma sırasında kendi genel IP 'nizi atayabilir veya var olan bir kümenin yük dengeleyici özelliklerini güncelleştirebilirsiniz.

Birden çok IP adresi veya ön ek sunarak, tek bir yük dengeleyici nesnesi arkasında IP adresini tanımlarken birden çok yedekleme hizmeti tanımlayabilirsiniz. Belirli düğümlerin çıkış uç noktası, ilişkili oldukları hizmete bağlı olarak değişir.

> [!IMPORTANT]
> *Standart* SKU 'su yük dengeleyiciniz ile çıkış için *Standart* SKU genel IP 'lerini kullanmanız gerekir. [Az Network public-ip show][az-network-public-ip-show] komutunu kullanarak genel IP 'nizin SKU 'sunda emin olabilirsiniz:
>
> ```azurecli-interactive
> az network public-ip show --resource-group myResourceGroup --name myPublicIP --query sku.name -o tsv
> ```

Genel IP 'nizin kimliklerini listelemek için [az Network public-ip show][az-network-public-ip-show] komutunu kullanın.

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

Yukarıdaki komut, *Myresourcegroup* kaynak grubundaki *Mypublicıp* genel IP 'nin kimliğini gösterir.

Kümenizi genel IP 'larınız ile güncelleştirmek için, *Load-dengeleyici-giden-IP* parametresi ile *az aks Update* komutunu kullanın.

Aşağıdaki örnek, önceki komuttan gelen kimliklerle *yük dengeleyici-giden-IP* parametresini kullanır.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

*Standart* SKU yük dengeleyicinizle çıkış IÇIN genel IP öneklerini de kullanabilirsiniz. Aşağıdaki örnek, genel IP öneklerinizin kimliklerini listelemek için [az Network public-ip önekini göster][az-network-public-ip-prefix-show] komutunu kullanır:

```azurecli-interactive
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

Yukarıdaki komut, *Myresourcegroup* kaynak grubundaki *Mypublicipprefix* genel IP ön ekinin kimliğini gösterir.

Aşağıdaki örnek, önceki komuttan gelen kimliklerle *yük dengeleyici-giden-IP-önekleri* parametresini kullanır.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

> [!IMPORTANT]
> Genel IP 'Ler ve IP önekleri, AKS kümeniz ile aynı aboneliğin aynı bölgede ve bir parçası olmalıdır. 

### <a name="define-your-own-public-ip-or-prefixes-at-cluster-create-time"></a>Küme oluşturma zamanında kendi genel IP veya öneklerinizi tanımlama

Çıkış uç noktaları beyaz listeleme gibi senaryoları desteklemek için küme oluşturma sırasında kendi IP adreslerinizi veya IP öneklerinizi almak isteyebilirsiniz. Kümenin yaşam döngüsünün başlangıcında kendi genel IP 'Leri ve IP öneklerinizi tanımlamak için yukarıda gösterilen aynı parametreleri küme oluşturma adımınıza ekleyin.

Başlangıç aşamasında genel IP 'inizle yeni bir küme oluşturmak için, *Load-dengeleyici-giden-IP* parametresi ile *az aks Create* komutunu kullanın.

```
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Başlangıçta genel IP öneklerinizi içeren yeni bir küme oluşturmak için, *Load-dengeleyici-giden-IP-önekleri* parametresiyle *az aks Create* komutunu kullanın.

```
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

## <a name="clean-up-the-standard-sku-load-balancer-configuration"></a>Standart SKU yük dengeleyici yapılandırmasını Temizleme

Örnek uygulama ve yük dengeleyici yapılandırmasını kaldırmak için [kubectl Delete][kubectl-delete]kullanın:

```console
kubectl delete -f sample.yaml
kubectl delete -f standard-lb.yaml
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes Services [belgelerindeki][kubernetes-services]Kubernetes hizmetleri hakkında daha fazla bilgi edinin.

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-network-public-ip-show]: /cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

