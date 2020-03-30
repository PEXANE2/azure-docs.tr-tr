---
title: Azure Kubernetes Hizmetlerinde (AKS) Azure CLI'yi kullanarak sanal düğümler oluşturma
description: Bölmeleri çalıştırmak için sanal düğümler kullanan bir Azure Kubernetes Hizmetleri (AKS) kümesi oluşturmak için Azure CLI'yi nasıl kullanacağınızı öğrenin.
services: container-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 31e8b5aceb356ca1415419650a9df3070462bde0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475536"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-using-the-azure-cli"></a>Azure CLI'yi kullanarak sanal düğümler kullanmak için bir Azure Kubernetes Hizmetleri (AKS) kümesi oluşturun ve yapılandırma

Bir Azure Kubernetes Hizmeti (AKS) kümesinde uygulama iş yüklerini hızla ölçeklendirmek için sanal düğümler kullanabilirsiniz. Sanal düğümlerle, bölmelerin hızlı bir şekilde sağlanmasına ve yalnızca yürütme sürelerinin saniyede ödenmesine sahipsiniz. Ek bölmeleri çalıştırmak için VM bilgi işlem düğümleri dağıtmak için Kubernetes küme otomatik ölçekleyici beklemeniz gerekmez. Sanal düğümler yalnızca Linux bölmeleri ve düğümleri ile desteklenir.

Bu makalede, sanal ağ kaynaklarını ve AKS kümesini nasıl oluşturup yapılandırabileceğinizi, ardından sanal düğümleri nasıl etkinleştirdiğinizi gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Sanal düğümler, ACI ve AKS kümesinde çalışan bölmeler arasındaki ağ iletişimini sağlar. Bu iletişimi sağlamak için bir sanal ağ alt ağı oluşturulur ve temsilci izinleri atanır. Sanal düğümler yalnızca *gelişmiş* ağ kullanılarak oluşturulan AKS kümeleriyle çalışır. Varsayılan olarak, AKS kümeleri *temel* ağ ile oluşturulur. Bu makalede, sanal ağ ve alt ağlar nasıl oluşturulacak, ardından gelişmiş ağ kullanan bir AKS kümesi dağıtılanın.

Daha önce ACI kullanmadıysanız, hizmet sağlayıcısını aboneliğinize kaydedin. Aşağıdaki örnekte gösterildiği gibi az sağlayıcı [listesi][az-provider-list] komutunu kullanarak ACI sağlayıcı kaydının durumunu kontrol edebilirsiniz:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

*Microsoft.ContainerInstance* sağlayıcısı, aşağıdaki örnek çıktıda gösterildiği gibi *Kayıtlı*olarak rapor vermelidir:

```output
Namespace                    RegistrationState
---------------------------  -------------------
Microsoft.ContainerInstance  Registered
```

Sağlayıcı *NotRegistered*olarak gösterirse, aşağıdaki örnekte gösterildiği gibi [az sağlayıcı kaydını][az-provider-register] kullanarak sağlayıcıyı kaydedin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Aşağıdaki bölgeler sanal düğüm dağıtımları için desteklenir:

* Avustralya Doğu (avustralya doğu)
* Orta ABD (centralus)
* Doğu ABD (doğu)
* Doğu ABD 2 (eastus2)
* Japonya Doğu (Japaneast)
* Kuzey Avrupa (kuzeyavrupa)
* Güneydoğu Asya (güneydoğuasya)
* Batı Orta ABD (westcentralus)
* Batı Avrupa (westeurope)
* Batı ABD (westus)
* Batı ABD 2 (westus2)

## <a name="known-limitations"></a>Bilinen sınırlamalar
Sanal Düğümler işlevselliği büyük ölçüde ACI'ın özellik kümesine bağlıdır. Aşağıdaki senaryolar henüz Sanal Düğümlerle desteklenmez

* ACR görüntülerini çekmek için servis ilkesini kullanma. [Geçici çözüm](https://github.com/virtual-kubelet/virtual-kubelet/blob/master/providers/azure/README.md#Private-registry) [Kubernetes sırlarını](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line) kullanmaktır
* VNet eşleme, Kubernetes ağ ilkeleri ve ağ güvenlik grupları yla internete giden trafiği içeren [Sanal Ağ Sınırlamaları.](../container-instances/container-instances-vnet.md)
* Init kapları
* [Ana bilgisayar takma adları](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/)
* ACI exec için [argümanlar](../container-instances/container-instances-exec.md#restrictions)
* [DaemonSets](concepts-clusters-workloads.md#statefulsets-and-daemonsets) sanal düğüm e poddağıtmaz
* [Windows Server düğümleri (şu anda AKS önizlemede)](windows-container-cli.md) sanal düğümlerin yanında desteklenmez. Bir AKS kümesinde Windows Server düğümlerine gerek kalmadan Windows Server kapsayıcılarını zamanlamak için sanal düğümler kullanabilirsiniz.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Bulut Kabuğunu açmak için, kod bloğunun sağ üst köşesinden **deneyin'i** seçin. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/bash](https://shell.azure.com/bash)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yüklemeyi ve kullanmayı tercih ederseniz, bu makalede Azure CLI sürümü 2.0.49 veya sonrası gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. [az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *westus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[az ağ vnet oluşturma][az-network-vnet-create] komutunu kullanarak sanal ağ oluşturun. Aşağıdaki örnek, *10.0.0.0/8*adres öneki ile sanal ağ adı *myVnet* ve *myAKSSubnet*adlı bir alt ağ oluşturur. Bu alt net in adres öneki *varsayılan 10.240.0.0/16:*

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16
```

Şimdi [az ağ vnet subnet oluşturma][az-network-vnet-subnet-create] komutunu kullanarak sanal düğümler için ek bir alt ağ oluşturun. Aşağıdaki örnek, *10.241.0.0/16*adres öneki ile *myVirtualNodeSubnet* adlı bir alt ağ oluşturur.

```azurecli-interactive
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name myVirtualNodeSubnet \
    --address-prefixes 10.241.0.0/16
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Bu hizmet sorumlusu Azure CLI veya portal ile otomatik olarak oluşturulabilir veya kendiniz önceden bir tane oluşturup ek izinler atayabilirsiniz.

[az ad sp create-for-rbac][az-ad-sp-create-for-rbac] komutunu kullanarak bir hizmet sorumlusu oluşturun. `--skip-assignment` parametresi, ek izinlerin atanmasını engeller.

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzer:

```output
{
  "appId": "bef76eb3-d743-4a97-9534-03e9388811fc",
  "displayName": "azure-cli-2018-11-21-18-42-00",
  "name": "http://azure-cli-2018-11-21-18-42-00",
  "password": "1d257915-8714-4ce7-a7fb-0e5a5411df7f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

*appId* ve *password* değerlerini not edin. Bu değerler aşağıdaki adımlarda kullanılacaktır.

## <a name="assign-permissions-to-the-virtual-network"></a>Sanal ağa izin atama

Kümenizin sanal ağı kullanmasına ve yönetmesine izin vermek için, AKS hizmet sorumlusuna ağ kaynaklarını kullanmak için doğru hakları vermeniz gerekir.

İlk olarak, az ağ [vnet göstermek][az-network-vnet-show]kullanarak sanal ağ kaynak kimliği almak:

```azurecli-interactive
az network vnet show --resource-group myResourceGroup --name myVnet --query id -o tsv
```

AKS kümesinin sanal ağı kullanması için doğru erişim sağlamak için, az rol ataması oluşturma komutunu kullanarak bir [rol ataması oluşturun.][az-role-assignment-create] `<appId`> ve `<vnetId>` yerine önceki iki adımda topladığınız değerleri yazın.

```azurecli-interactive
az role assignment create --assignee <appId> --scope <vnetId> --role Contributor
```

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Bir önceki adımda oluşturulan AKS alt ağına bir AKS kümesi dağıtabilirsiniz. [Az network vnet subnet show][az-network-vnet-subnet-show]kullanarak bu alt netin kimliğini alın:

```azurecli-interactive
az network vnet subnet show --resource-group myResourceGroup --vnet-name myVnet --name myAKSSubnet --query id -o tsv
```

AKS kümesi oluşturmak için [az aks create][az-aks-create] komutunu kullanın. Aşağıdaki örnekte, bir düğüm ile *myAKSCluster* adlı bir küme oluşturulmuştur. Önceki `<subnetId>` adımda elde edilen `<password>` kimlikle `<appId>` değiştirin ve ardından 

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id <subnetId> \
    --service-principal <appId> \
    --client-secret <password>
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON tarafından biçimlendirilmiş bilgiler gösterilir.

## <a name="enable-virtual-nodes-addon"></a>Sanal düğüm eklemeyi etkinleştirme

Sanal düğümleri etkinleştirmek için şimdi [az aks enable-addons][az-aks-enable-addons] komutunu kullanın. Aşağıdaki örnek, önceki adımda oluşturulan *myVirtualNodeSubnet* adlı alt ağı kullanır:

```azurecli-interactive
az aks enable-addons \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --addons virtual-node \
    --subnet-name myVirtualNodeSubnet
```

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu adım kimlik bilgilerini indirir ve Kubernetes CLI’yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnek çıktı oluşturulan tek VM düğümü ve daha sonra Linux, *sanal-düğüm-aci-linux*için sanal düğüm gösterir:

```output
NAME                          STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux        Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0      Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Örnek bir uygulama dağıtma

Adlandırılmış `virtual-node.yaml` bir dosya oluşturun ve aşağıdaki YAML'de kopyalayın. Kapsayıcıyı düğümüzerinde zamanlamak için bir [düğümSeçici][node-selector] ve [tolerasyon][toleration] tanımlanır.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

[Kubectl uygula][kubectl-apply] komutu ile uygulamayı çalıştırın.

```console
kubectl apply -f virtual-node.yaml
```

Bölme lerin ve zamanlanan düğümün `-o wide` listesini çıktılamak için bağımsız değişkenle [kubectl get pods][kubectl-get] komutunu kullanın. Bölmenin `aci-helloworld` düğümde zamanlandığına `virtual-node-aci-linux` dikkat edin.

```console
kubectl get pods -o wide
```

```output
NAME                            READY     STATUS    RESTARTS   AGE       IP           NODE
aci-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Bölmeye, sanal düğümlerle kullanılmak üzere atanan Azure sanal ağ alt ağından bir dahili IP adresi atanır.

> [!NOTE]
> Azure Kapsayıcı Kayıt Defteri'nde depolanan görüntüleri kullanıyorsanız, [bir Kubernetes sırrını yapılandırın ve kullanın.][acr-aks-secrets] Sanal düğümlerin geçerli bir sınırlaması, tümleşik Azure AD hizmeti temel kimlik doğrulamasını kullanamamanızdır. Bir sır kullanmazsanız, sanal düğümlerde zamanlanan bölmeler başlatılamaz `HTTP response status code 400 error code "InaccessibleImage"`ve hatayı bildiremez.

## <a name="test-the-virtual-node-pod"></a>Sanal düğüm bölmesini test edin

Sanal düğümde çalışan bölmeyi test etmek için, bir web istemcisi ile demo uygulamasına göz atın. Bölmeye dahili bir IP adresi atandığı için, AKS kümesindeki başka bir bölmeden gelen bu bağlantıyı hızlı bir şekilde sınayabilirsiniz. Bir test bölmesi oluşturun ve bir terminal oturumu takın:

```console
kubectl run --generator=run-pod/v1 -it --rm testvk --image=debian
```

Pod `curl` kullanarak yükleyin: `apt-get`

```console
apt-get update && apt-get install -y curl
```

Şimdi pod'unuzun adresine erişin, `curl`örneğin *http://10.241.0.4*. Önceki `kubectl get pods` komutta gösterilen kendi iç IP adresinizi sağlayın:

```console
curl -L http://10.241.0.4
```

Demo uygulaması, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi görüntülenir:

```output
<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

Terminal oturumunu test bölmenize `exit`' le kapatın. Oturumunuz sona erdiğinde, bölme silinir.

## <a name="remove-virtual-nodes"></a>Sanal düğümleri kaldırma

Artık sanal düğümler kullanmak istemiyorsanız, [az aks devre dışı-addons][az aks disable-addons] komutunu kullanarak bunları devre dışı bırakabilirsiniz. 

Gerekirse tarayıcınızda [https://shell.azure.com](https://shell.azure.com) Azure Bulut Su Larını açmaya gidin.

İlk olarak, sanal düğüm üzerinde çalışan helloworld pod silin:

```console
kubectl delete -f virtual-node.yaml
```

Aşağıdaki örnek komut Uyruşur sanal düğümleri devre dışı kalmaktadır:

```azurecli-interactive
az aks disable-addons --resource-group myResourceGroup --name myAKSCluster --addons virtual-node
```

Şimdi, sanal ağ kaynaklarını ve kaynak grubunu kaldırın:

```azurecli-interactive
# Change the name of your resource group, cluster and network resources as needed
RES_GROUP=myResourceGroup
AKS_CLUSTER=myAKScluster
AKS_VNET=myVnet
AKS_SUBNET=myVirtualNodeSubnet

# Get AKS node resource group
NODE_RES_GROUP=$(az aks show --resource-group $RES_GROUP --name $AKS_CLUSTER --query nodeResourceGroup --output tsv)

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $NODE_RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name $AKS_VNET --name $AKS_SUBNET --remove delegations 0
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal düğümüzerinde bir bölme zamanlandı ve özel, dahili BIR IP adresi atandı. Bunun yerine bir hizmet dağıtımı oluşturabilir ve bir yük dengeleyicisi veya giriş denetleyicisi aracılığıyla pod'unuza trafik yönlendirebilirsiniz. Daha fazla bilgi için [aks'ta temel bir giriş denetleyicisi oluşturma'ya][aks-basic-ingress]bakın.

Sanal düğümler genellikle AKS'deki ölçekleme çözümünün bir bileşenidir. Ölçekleme çözümleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes yatay pod otomatik ölçekleyicikullanın][aks-hpa]
- [Kubernetes küme otomatik ölçekleyicisini kullanma][aks-cluster-autoscaler]
- [Sanal Düğümler için Otomatik Ölçeklendirme örneğine göz atın][virtual-node-autoscale]
- [Sanal Kubelet açık kaynak kitaplığı hakkında daha fazla bilgi edinin][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[aks-github]: https://github.com/azure/aks/issues
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet
[acr-aks-secrets]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks disable-addons]: /cli/azure/aks#az-aks-disable-addons
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register
