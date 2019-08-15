---
title: Önizleme-Azure Kubernetes Service (AKS) ' de standart SKU yük dengeleyici kullanma
description: Azure Kubernetes Service (AKS) ile hizmetlerinizi kullanıma sunmak için standart SKU ile yük dengeleyiciyi nasıl kullanacağınızı öğrenin.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 06/25/2019
ms.author: zarhoads
ms.openlocfilehash: 4e234d3849e09bd8c57a8c33bb378ab801ce0f6d
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019460"
---
# <a name="preview---use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Önizleme-Azure Kubernetes Service (AKS) ' de standart SKU yük dengeleyici kullanma

Azure Kubernetes Service (AKS) içindeki uygulamalarınıza erişim sağlamak için bir Azure Load Balancer oluşturup kullanabilirsiniz. AKS üzerinde çalışan bir yük dengeleyici, iç veya dış yük dengeleyici olarak kullanılabilir. İç yük dengeleyici, bir Kubernetes hizmetini yalnızca AKS kümesi ile aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Dış yük dengeleyici, giriş için bir veya daha fazla genel IP alır ve genel IP 'Leri kullanarak bir Kubernetes hizmetini dışarıdan erişilebilir hale getirir.

Azure Load Balancer, *temel* ve *Standart*olmak üzere iki SKU 'da kullanılabilir. Varsayılan olarak, AKS üzerinde bir yük dengeleyici oluşturmak için bir hizmet bildirimi kullanıldığında *temel* SKU kullanılır. *Standart* SKU yük dengeleyici kullanmak, daha büyük arka uç havuz boyutu ve kullanılabilirlik alanları gibi ek özellikler ve işlevler sağlar. Kullanmayı seçmeden önce *Standart* ve *temel* yük dengeleyiciler arasındaki farkları anlamanız önemlidir. Bir AKS kümesi oluşturduktan sonra, bu küme için yük dengeleyici SKU 'sunu değiştiremezsiniz. *Temel* ve *Standart* SKU 'lar hakkında daha fazla bilgi için bkz. [Azure yük dengeleyici SKU karşılaştırması][azure-lb-comparison].

Bu makalede, Azure Kubernetes Service (AKS) ile *Standart* SKU ile bir Azure Load Balancer oluşturma ve kullanma işlemlerinin nasıl yapılacağı gösterilir.

Bu makalede, Kubernetes ve Azure Load Balancer kavramlarının temel bir şekilde anlaşıldığı varsayılır. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts] ve [Azure Load Balancer nedir?][azure-lb].

Bu özellik şu anda önizleme sürümündedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.59 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][install-azure-cli].

## <a name="before-you-begin"></a>Başlamadan önce

Mevcut bir alt ağ veya kaynak grubu kullanıyorsanız AKS kümesi hizmet sorumlusu ağ kaynaklarını yönetmek için izne ihtiyaç duyuyor. Genel olarak, temsilcili kaynaklar üzerindeki hizmet sorumlusu rolüne *ağ katılımcısı* rolünü atayın. İzinler hakkında daha fazla bilgi için bkz. [diğer Azure kaynaklarına AKS erişimi verme][aks-sp].

Varsayılan *temel*yerine yük dengeleyici için SKU 'yu *Standart* olarak ayarlayan bir aks kümesi oluşturmanız gerekir. Bir AKS kümesi oluşturmak sonraki bir adımda ele alınmıştır, ancak önce birkaç önizleme özelliğini etkinleştirmeniz gerekir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme özellikleridir. Topluluğumuza geri bildirim ve hata toplamak için sağlanırlar. Önizlemede, bu özellikler üretim kullanımı için tasarlanmamıştır. Genel önizlemede bulunan Özellikler ' en iyi çaba ' desteği altına düşmektedir. AKS teknik destek ekiplerinden yardım yalnızca çalışma saatleri Pasifik saat dilimi (PST) sırasında kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

Azure yük dengeleyici standart SKU 'sunu kullanmak için, *aks-Preview* CLI uzantısının sürüm 0.4.1 veya üzeri olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-aksazurestandardloadbalancer-preview-feature"></a>AKSAzureStandardLoadBalancer Preview özelliğini Kaydet

*Standart* SKU ile yük dengeleyici kullanan bir aks kümesi oluşturmak için aboneliğinizdeki *AKSAzureStandardLoadBalancer* Özellik bayrağını etkinleştirmeniz gerekir. *AKSAzureStandardLoadBalancer* özelliği, sanal makine ölçek kümeleri kullanarak bir küme oluştururken de *VMSSPreview* kullanır. Bu özellik, bir kümeyi yapılandırırken en son hizmet iyileştirmeleri sağlar. Gerekli olmasa da, *VMSSPreview* Özellik bayrağını da etkinleştirmeniz önerilir.

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

Aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanarak *VMSSPreview* ve *AKSAzureStandardLoadBalancer* Özellik bayraklarını kaydedin:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "VMSSPreview"
az feature register --namespace "Microsoft.ContainerService" --name "AKSAzureStandardLoadBalancer"
```

> [!NOTE]
> *VMSSPreview* veya *AKSAzureStandardLoadBalancer* Özellik bayraklarını başarıyla kaydettikten sonra oluşturduğunuz aks kümesi, bu önizleme kümesi deneyimini kullanır. Düzenli, tam olarak desteklenen kümeler oluşturmaya devam etmek için üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Sınırlamalar

*Standart* SKU ile yük dengeleyiciyi destekleyen aks kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* Yük Dengeleyici için *Standart* SKU kullanırken, genel adreslere izin vermenız ve IP oluşturma ile ilgili herhangi bir Azure ilkesinin oluşturulmaması gerekir. AKS kümesi, genellikle *Mc_* ile adlandırılan aks kümesi için aynı kaynak grubunda oluşturulan *Standart* SKU genel IP 'yi otomatik olarak oluşturur. AKS, genel IP 'yi *Standart* SKU yük dengeleyicisine atar. AKS kümesinden giden trafiğe izin vermek için genel IP gereklidir. Bu genel IP Ayrıca, denetim düzlemi ve aracı düğümleri arasındaki bağlantının yanı sıra önceki AKS sürümleriyle uyumluluğu sürdürmek için de gereklidir.
* Yük Dengeleyici için *Standart* SKU kullanırken, Kubernetes sürüm 1.13.5 veya üstünü kullanmanız gerekir.
* [Düğüm genel IP özelliğini](use-multiple-node-pools.md#assign-a-public-ip-per-node-in-a-node-pool) standart yük dengeleyiciler ile birlikte kullanıyorsanız, düğüm IÇIN bir SLB giden kuralı ya da genel IP ayarlayabilirsiniz. Bir sanal makine hem SLB giden kuralına hem de genel IP 'ye aynı anda iliştirilemediğinden bir veya başka birini seçmeniz gerekir.

Bu özellik önizlemedeyken aşağıdaki ek sınırlamalar geçerlidir:

* AKS 'deki bir yük dengeleyici için *Standart* SKU kullanırken, yük dengeleyici için çıkış için kendı genel IP adresinizi ayarlayamazsınız. AKS 'in yük dengeleyicisine atadığı IP adresini kullanmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure 'da da çalıştığı yerdir. [Az Group Create][az-group-create] komutunu kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

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
*Standart* SKU ile yük dengeleyiciyi destekleyen bir aks kümesini çalıştırmak için, kümenizin *yük dengeleyici-SKU* parametresini *Standart*olarak ayarlaması gerekir. Bu parametre, kümeniz oluşturulduğunda *Standart* SKU ile bir yük dengeleyici oluşturur. Kümenizde bir *LoadBalancer* hizmeti çalıştırdığınızda, *Standart* SK yük dengeleyici yapılandırması hizmetin yapılandırmasıyla güncelleştirilir. *Myakscluster*adlı bir aks kümesi oluşturmak için [az aks Create][az-aks-create] komutunu kullanın.

> [!NOTE]
> *Load-dengeleyici-SKU* özelliği yalnızca kümeniz oluşturulduğunda kullanılabilir. Bir AKS kümesi oluşturulduktan sonra yük dengeleyici SKU 'sunu değiştiremezsiniz. Ayrıca, tek bir kümede Yük dengeleyici SKU 'SU yalnızca bir türünü kullanabilirsiniz.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON biçimli bilgileri döndürür.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. Azure Cloud Shell kullanıyorsanız, `kubectl` zaten yüklüdür. Yerel olarak `kubectl` yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

Kubernetes kümenize bağlanacak şekilde yapılandırmak `kubectl` için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğüm durumunun olduğundan emin olun:

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.13.9
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

Yukarıdaki bildirim iki dağıtımı yapılandırır: *Azure-oy-ön* ve *Azure-oy geri*. *Azure-oy ön* dağıtımını yük dengeleyici kullanılarak sunulacak şekilde yapılandırmak için, aşağıdaki örnekte gösterildiği gibi adlı `standard-lb.yaml` bir bildirim oluşturun:

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

*Standart* SKU yük dengeleyici artık örnek uygulamayı kullanıma sunmak üzere yapılandırılmıştır. Yük dengeleyicinin genel IP 'sini görmek için [kubectl Get][kubectl-get] kullanarak *Azure-oy-Front* hizmeti ayrıntılarını görüntüleyin. Yük dengeleyicinin genel IP adresi, *dış IP* sütununda gösterilir. Aşağıdaki örnekte gösterildiği gibi, IP adresinin *\<bekliyor\>* durumundan gerçek bir dış IP adresine değiştirilmesi bir veya iki dakika sürebilir:

```
$ kubectl get service azure-vote-front

NAME                TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
azure-vote-front    LoadBalancer   10.0.227.198   52.179.23.131   80:31201/TCP   16s
```

Tarayıcıda genel IP 'ye gidin ve örnek uygulamayı gördiğinizi doğrulayın. Yukarıdaki örnekte, genel IP olur `52.179.23.131`.

![Azure Vote’a göz atma görüntüsü](media/container-service-kubernetes-walkthrough/azure-vote.png)

> [!NOTE]
> Ayrıca yük dengeleyiciyi iç olarak yapılandırabilir ve genel bir IP 'yi kullanıma sunmamalısınız. Yük dengeleyiciyi iç olarak yapılandırmak için, `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` *LoadBalancer* hizmetine bir ek açıklama olarak ekleyin. [Burada][internal-lb-yaml]bir YAML bildiriminin yanı sıra bir iç yük dengeleyici hakkında daha fazla ayrıntı görebilirsiniz.

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
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
