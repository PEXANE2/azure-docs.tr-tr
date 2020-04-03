---
title: Azure Kubernetes Hizmetleri'ndeki (AKS) portalı kullanarak sanal düğümler oluşturma
description: Podları çalıştırmak için sanal düğümler kullanan bir Azure Kubernetes Hizmetleri (AKS) kümesi oluşturmak için Azure portalını nasıl kullanacağınızı öğrenin.
services: container-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 696821e12e963292107cad5b22f00a9816a94b25
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80616419"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Azure portalında sanal düğümler kullanmak için bir Azure Kubernetes Hizmetleri (AKS) kümesi oluşturun ve yapılandırma

İş yüklerini bir Azure Kubernetes Hizmeti (AKS) kümesinde hızla dağıtmak için sanal düğümler kullanabilirsiniz. Sanal düğümler ile, bölmeleri hızlı sağlama var ve sadece onların yürütme süresi için saniyede ödeme. Ölçeklendirme senaryosunda, ek bölmeleri çalıştırmak için VM bilgi işlem düğümlerini dağıtmak için Kubernetes küme otomatik ölçekleyicisini beklemeniz gerekmez. Sanal düğümler yalnızca Linux bölmeleri ve düğümleri ile desteklenir.

Bu makalede, sanal ağ kaynakları ve sanal düğümleri etkin bir AKS küme oluşturmak ve yapılandırmak nasıl gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Sanal düğümler, Azure Kapsayıcı Örnekleri 'nde (ACI) çalışan bölmeler ile AKS kümesi arasında ağ iletişimini sağlar. Bu iletişimi sağlamak için bir sanal ağ alt ağı oluşturulur ve temsilci izinleri atanır. Sanal düğümler yalnızca *gelişmiş* ağ kullanılarak oluşturulan AKS kümeleriyle çalışır. Varsayılan olarak, AKS kümeleri *temel* ağ ile oluşturulur. Bu makalede, sanal ağ ve alt ağlar nasıl oluşturulacak, ardından gelişmiş ağ kullanan bir AKS kümesi dağıtılanın.

Daha önce ACI kullanmadıysanız, hizmet sağlayıcısını aboneliğinize kaydedin. Aşağıdaki örnekte gösterildiği gibi az sağlayıcı [listesi][az-provider-list] komutunu kullanarak ACI sağlayıcı kaydının durumunu kontrol edebilirsiniz:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

*Microsoft.ContainerInstance* sağlayıcısı, aşağıdaki örnek çıktıda gösterildiği gibi *Kayıtlı*olarak rapor vermelidir:

```output
Namespace                    RegistrationState    RegistrationPolicy
---------------------------  -------------------  --------------------
Microsoft.ContainerInstance  Registered           RegistrationRequired
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

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Azure portalının sol üst köşesinde, kaynak > **Kubernetes Hizmeti** **Oluştur'u**seçin.

Temel **Bilgiler** sayfasında, aşağıdaki seçenekleri yapılandırın:

- *PROJE AYRINTILARI*: Bir Azure aboneliği seçtikten sonra bir Azure kaynak grubu seçin veya *myResourceGroup* adıyla yeni bir tane oluşturun. **Kubernetes kümesi adı** alanına *myAKSCluster* gibi bir ad girin.
- *KÜME AYRINTILARI*: AKS kümesi için bölge, Kubernetes sürümü ve DNS adı ön eki seçin.
- *PRIMER DÜĞÜM HAVUZU*: AKS düğümleri için bir VM boyutu seçin. AKS kümesi dağıtıldıktan sonra, sanal makine boyutu **değiştirilemez**.
     - Kümeye dağıtılacak düğüm sayısını seçin. Bu makale için Düğüm **sayısını** *1*olarak ayarlayın. Küme dağıtıldıktan sonra düğüm sayısı **ayarlanabilir**.

**Sonraki'yi tıklatın: Ölçeklendirin.**

**Ölçek** sayfasında, **Sanal düğümler**altında *Etkin'i* seçin.

![AKS kümesi oluşturma ve sanal düğümleri etkinleştirme](media/virtual-nodes-portal/enable-virtual-nodes.png)

Varsayılan olarak, bir Azure Etkin Dizin hizmet ilkesi oluşturulur. Bu hizmet ilkesi küme iletişimi ve diğer Azure hizmetleriyle tümleştirme için kullanılır.

Küme, gelişmiş ağ için de yapılandırılmıştır. Sanal düğümler kendi Azure sanal ağ alt ağlarını kullanacak şekilde yapılandırılır. Bu alt ağ, AkS kümesi arasında Azure kaynaklarını bağlamak için izinler devralmıştır. Azure portalı, önceden devralmış alt ağınız yoksa, Azure sanal ağını ve alt netini sanal düğümlerle kullanmak üzere oluşturur ve yapılandırır.

**İncele ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur'u**seçin.

AKS kümesinin oluşturulması ve kullanıma hazır olması birkaç dakika sürer.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bir Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi [kubectl][kubectl]’i kullanın. `kubectl` istemcisi Azure Cloud Shell’de önceden yüklüdür.

Bulut Kabuğunu açmak için, kod bloğunun sağ üst köşesinden **deneyin'i** seçin. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/bash](https://shell.azure.com/bash)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

[az aks get-credentials][az-aks-get-credentials] komutunu kullanarak, `kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırın. Aşağıdaki örnek *myResourceGroup* adlı kaynak grubu içindeki *myAKSCluster* adlı kümenin kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnek çıktı oluşturulan tek VM düğümü ve daha sonra Linux, *sanal-düğüm-aci-linux*için sanal düğüm gösterir:

```output
NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Örnek bir uygulama dağıtma

Azure Bulut Kabuğu'nda, aşağıdaki `virtual-node.yaml` YAML'de bir dosya oluşturun ve kopyalayın. Kapsayıcıyı düğümüzerinde zamanlamak için bir [düğümSeçici][node-selector] ve [tolerasyon][toleration] tanımlanır. Bu ayarlar bölmenin sanal düğümde zamanlanmasına izin verir ve özelliğin başarıyla etkinleştirildiğinden onaylar.

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

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

Bölme lerin ve zamanlanan düğümün `-o wide` listesini çıktılamak için bağımsız değişkenle [kubectl get pods][kubectl-get] komutunu kullanın. Bölmenin `virtual-node-helloworld` düğümde zamanlandığına `virtual-node-linux` dikkat edin.

```console
kubectl get pods -o wide
```

```output
NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Bölmeye, sanal düğümlerle kullanılmak üzere atanan Azure sanal ağ alt ağından bir dahili IP adresi atanır.

> [!NOTE]
> Azure Kapsayıcı Kayıt Defteri'nde depolanan görüntüleri kullanıyorsanız, [bir Kubernetes sırrını yapılandırın ve kullanın.][acr-aks-secrets] Sanal düğümlerin geçerli bir sınırlaması, tümleşik Azure AD hizmeti temel kimlik doğrulamasını kullanamamanızdır. Bir sır kullanmazsanız, sanal düğümlerde zamanlanan bölmeler başlatılamaz `HTTP response status code 400 error code "InaccessibleImage"`ve hatayı bildiremez.

## <a name="test-the-virtual-node-pod"></a>Sanal düğüm bölmesini test edin

Sanal düğümde çalışan bölmeyi test etmek için, bir web istemcisi ile demo uygulamasına göz atın. Bölmeye dahili bir IP adresi atandığı için, AKS kümesindeki başka bir bölmeden gelen bu bağlantıyı hızlı bir şekilde sınayabilirsiniz. Bir test bölmesi oluşturun ve bir terminal oturumu takın:

```console
kubectl run -it --rm virtual-node-test --image=debian
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

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal düğümüzerinde bir bölme zamanlandı ve özel, dahili BIR IP adresi atandı. Bunun yerine bir hizmet dağıtımı oluşturabilir ve bir yük dengeleyicisi veya giriş denetleyicisi aracılığıyla pod'unuza trafik yönlendirebilirsiniz. Daha fazla bilgi için [aks'ta temel bir giriş denetleyicisi oluşturma'ya][aks-basic-ingress]bakın.

Sanal düğümler AKS'deki ölçekleme çözümünün bir bileşenidir. Ölçekleme çözümleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes yatay pod otomatik ölçekleyicikullanın][aks-hpa]
- [Kubernetes küme otomatik ölçekleyicisini kullanma][aks-cluster-autoscaler]
- [Sanal Düğümler için Otomatik Ölçeklendirme örneğine göz atın][virtual-node-autoscale]
- [Sanal Kubelet açık kaynak kitaplığı hakkında daha fazla bilgi edinin][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[aks-github]: https://github.com/azure/aks/issues]
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet
[acr-aks-secrets]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: cluster-autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register