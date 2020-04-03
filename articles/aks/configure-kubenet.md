---
title: Azure Kubernetes Hizmetinde (AKS) kubenet ağ yapılandırma
description: Bir AKS kümesini varolan bir sanal ağa ve alt ağa dağıtmak için Azure Kubernetes Hizmeti'ndeki (AKS) kubenet (temel) ağı nasıl yapılandırıştırmayı öğrenin.
services: container-service
ms.topic: article
ms.date: 06/26/2019
ms.reviewer: nieberts, jomore
ms.openlocfilehash: 119265efa7b6504f3faf2e89cb68b9e9bd70bf9f
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617252"
---
# <a name="use-kubenet-networking-with-your-own-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) kendi IP adres aralıklarınız ile kubenet ağ larını kullanın

Varsayılan olarak, AKS kümeleri [kubenet][kubenet]kullanır ve sizin için bir Azure sanal ağı ve alt ağı oluşturulur. *Kubenet*ile düğümler Azure sanal ağ alt netinden bir IP adresi alır. Podlar, düğümlerin Azure sanal ağ alt ağından mantıksal olarak farklı bir adres alanından IP adresi alır. Ardından podların Azure sanal ağındaki kaynaklara erişebilmesi için ağ adresi çevirisi (NAT) yapılandırması gerçekleştirilir. Trafiğin kaynak IP adresi düğümün birincil IP adresine NAT'd'dir. Bu yaklaşım, bölmelerin kullanabilmesi için ağ alanınızda ayırmanız gereken IP adreslerinin sayısını büyük ölçüde azaltır.

[Azure Kapsayıcı Ağ Arabirimi (CNI)][cni-networking]ile her bölme alt ağdan bir IP adresi alır ve doğrudan erişilebilir. Bu IP adresleri ağ alanınızda benzersiz olmalıdır ve önceden planlanmalıdır. Her düğüm, desteklediği en fazla bölme sayısı için bir yapılandırma parametresi vardır. Düğüm başına eşdeğer IP adresi sayısı, bu düğüm için ön tarafta ayrılmıştır. Bu yaklaşım daha fazla planlama gerektirir ve genellikle IP adresi tükenmesine veya uygulama talepleriniz arttıkça kümeleri daha büyük bir alt ağda yeniden oluşturma gereksinimine yol açar.

Bu makalede, bir AKS kümesi için sanal ağ alt ağı oluşturmak ve kullanmak için *kubenet* ağ kullanımını zedelemeniz gösterilmektedir. Ağ seçenekleri ve değerlendirmelerhakkında daha fazla bilgi için [Kubernetes ve AKS için Ağ kavramlarına][aks-network-concepts]bakın.

## <a name="prerequisites"></a>Ön koşullar

* AKS kümesi için sanal ağ giden internet bağlantısına izin vermelidir.
* Aynı alt ağda birden fazla AKS kümesi oluşturmayın.
* `169.254.0.0/16`AKS kümeleri Kubernetes `172.30.0.0/16` `172.31.0.0/16` `192.0.2.0/24` servis adresi aralığını kullanamaz.
* AKS kümesi tarafından kullanılan hizmet ilkesi, sanal ağınızdaki alt ağda en az [Ağ Katılımcısı](../role-based-access-control/built-in-roles.md#network-contributor) rolüne sahip olmalıdır. Yerleşik Ağ Katılımcısı rolünü kullanmak yerine özel bir [rol](../role-based-access-control/custom-roles.md) tanımlamak istiyorsanız, aşağıdaki izinler gereklidir:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

> [!WARNING]
> Windows Server düğüm havuzlarını kullanmak için (şu anda AKS'de önizlemede), Azure CNI kullanmanız gerekir. Ağ modeli olarak kubenet kullanımı Windows Server kapsayıcıları için kullanılamaz.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI sürümü 2.0.65 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="overview-of-kubenet-networking-with-your-own-subnet"></a>Kendi alt ağınızla kubenet ağına genel bakış

Birçok ortamda, tahsis edilmiş IP adresi aralıklarına sahip sanal ağlar ve alt ağlar tanımladınız. Bu sanal ağ kaynakları, birden çok hizmeti ve uygulamayı desteklemek için kullanılır. Ağ bağlantısı sağlamak için AKS kümeleri *kubenet* (temel ağ) veya Azure CNI *(gelişmiş ağ)* kullanabilir.

*Kubenet*ile, sanal ağ alt ağında yalnızca düğümler ip adresi alır. Podlar birbirleriyle doğrudan iletişim kuramazlar. Bunun yerine, Kullanıcı Tanımlı Yönlendirme (UDR) ve IP iletme düğümleri arasında bölmeler arasında bağlantı için kullanılır. Ayrıca, atanmış bir IP adresi alan ve uygulama için trafiği yükleyen bir hizmetin arkasına bölme dağıtabilirsiniz. Aşağıdaki diyagram, AKS düğümlerinin sanal ağ alt ağında nasıl ip adresi aldığını, ancak bölmeleri almadığını gösterir:

![AKS kümesine sahip Kubenet ağ modeli](media/use-kubenet/kubenet-overview.png)

Azure, UDR'de en fazla 400 yolu destekler, bu nedenle 400 düğümden büyük bir AKS kümesine sahip olamazsınız. AKS [Sanal Düğümler][virtual-nodes] ve Azure Ağ İlkeleri *kubenet*ile desteklenmez.  Kubenet ile desteklendikleri için [Calico Ağ İlkeleri'ni][calico-network-policies]kullanabilirsiniz.

*Azure CNI*ile her bölme IP alt netinde bir IP adresi alır ve diğer bölmeler ve hizmetlerle doğrudan iletişim kurabilir. Kümeleriniz belirttiğiniz IP adresi aralığı kadar büyük olabilir. Ancak, IP adres aralığı önceden planlanmalıdır ve tüm IP adresleri destekleebilecekleri maksimum bölme sayısına bağlı olarak AKS düğümleri tarafından tüketilir. [Sanal Düğümler][virtual-nodes] veya Ağ İlkeleri (Azure veya Calico) gibi gelişmiş ağ özellikleri ve senaryoları *Azure CNI*ile desteklenir.

### <a name="ip-address-availability-and-exhaustion"></a>IP adresi kullanılabilirliği ve tükenme

*Azure CNI*ile, atanan IP adresi aralığı, bir kümeyi ölçeklendirdiğinizde veya yükselttidiğinizde ek düğüm ler eklemek için çok küçük olmasıdır. Ağ ekibi, beklenen uygulama taleplerini destekleyecek kadar büyük bir IP adresi aralığı da yayınlayamayabilir.

Uzlaşma olarak, *kubenet* kullanan ve varolan bir sanal ağ alt ağına bağlanan bir AKS kümesi oluşturabilirsiniz. Bu yaklaşım, düğümlerin kümede çalıştırılabilen tüm olası bölmeler için çok sayıda IP adresini önceden ayırmaya gerek kalmadan tanımlı IP adresleri almasını sağlar.

*Kubenet*ile çok daha küçük bir IP adresi aralığı kullanabilir ve büyük kümeleri ve uygulama taleplerini destekleyebilirsiniz. Örneğin, */27* IP adres aralığında bile, ölçeklendirmek veya yükseltmek için yeterli alana sahip 20-25 düğüm kümesi çalıştırabilirsiniz. Bu küme boyutu *2.200-2.750* bakla (varsayılan maksimum düğüm başına 110 pod) destekler. AKS'de *kubenet* ile yapılandırabileceğiniz düğüm başına maksimum bölme sayısı 110'dur.

Aşağıdaki temel hesaplamalar ağ modellerindeki farkı karşılaştırır:

- **kubenet** - basit bir */24* IP adres aralığı kümede *251* düğüme kadar destekleyebilir (her Azure sanal ağ alt ağı yönetim işlemleri için ilk üç IP adresini saklı tutar)
  - Bu düğüm sayısı *27.610* pod'a kadar destekleyebilir *(kubenet*ile düğüm başına varsayılan maksimum 110 bölme ile)
- **Azure CNI** - aynı temel */24* alt ağ aralığı kümedeki en fazla *8* düğümü destekleyebilir
  - Bu düğüm sayısı yalnızca *240* bölmeye kadar destekleyebilir *(Azure CNI*ile düğüm başına varsayılan maksimum 30 bölme)

> [!NOTE]
> Bu maksimumlar yükseltme veya ölçek işlemlerini hesaba katmaz. Uygulamada, alt net IP adresi aralığının desteklediği maksimum düğüm sayısını çalıştıramazsınız. Yükseltme işlemleri ölçeği sırasında kullanılmak üzere bazı IP adreslerini kullanılabilir bırakmanız gerekir.

### <a name="virtual-network-peering-and-expressroute-connections"></a>Sanal ağ izleme ve ExpressRoute bağlantıları

Şirket içi bağlantı sağlamak için hem *kubenet* hem de *Azure-CNI* ağ yaklaşımları [Azure sanal ağ izleme][vnet-peering] veya [ExpressRoute bağlantılarını][express-route]kullanabilir. Çakışmayı ve yanlış trafik yönlendirmesini önlemek için IP adresinizin aralıklarını dikkatle planlayın. Örneğin, birçok şirket içi ağ, ExpressRoute bağlantısı üzerinden reklamı yapılan *10.0.0.0/8* adres aralığını kullanır. AKS kümelerinizi bu adres aralığının dışında *172.16.0.0/16*gibi Azure sanal ağ alt ağlarında oluşturmanız önerilir.

### <a name="choose-a-network-model-to-use"></a>Kullanmak için bir ağ modeli seçin

AKS kümenizde hangi ağ eklentisinin kullanılacağının seçimi genellikle esneklik ve gelişmiş yapılandırma gereksinimleri arasındaki dengedir. Aşağıdaki hususlar, her ağ modelinin en uygun olduğu zaman anahat yardımcı olur.

*Kubenet'i* şu zaman kullanın:

- Sınırlı IP adresi alanınız var.
- Pod iletişiminin çoğu küme içindedir.
- Sanal düğümler veya Azure Ağ İlkesi gibi gelişmiş AKS özelliklerine ihtiyacınız yoktur.  [Calico ağ ilkelerini][calico-network-policies]kullanın.

*Azure CNI'yi* şu anda kullanın:

- Kullanılabilir IP adres alanınız var.
- Pod iletişiminin çoğu kümenin dışındaki kaynaklara yapılır.
- ÜT'leri yönetmek istemezsin.
- Sanal düğümler veya Azure Ağ İlkesi gibi AKS gelişmiş özelliklerine ihtiyacınız vardır.  [Calico ağ ilkelerini][calico-network-policies]kullanın.

Hangi ağ modelini kullanacağınıza karar vermenize yardımcı olacak daha fazla bilgi için [bkz.][network-comparisons]

## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma

*Kubenet* ve kendi sanal ağ alt ağınızı kullanmaya başlamak için önce az grubu oluşturma komutunu kullanarak bir kaynak grubu [oluşturun.][az-group-create] Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Kullanabileceğiniz mevcut bir sanal ağınız ve alt ağınız yoksa, [az network vnet create][az-network-vnet-create] komutunu kullanarak bu ağ kaynaklarını oluşturun. Aşağıdaki örnekte, sanal ağa *192.168.0.0/16*adres öneki ile *myVnet* adı verilir. *192.168.1.0/24*adresi öneki ile *myAKSSubnet* adında bir alt ağ oluşturulur.

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myAKSVnet \
    --address-prefixes 192.168.0.0/16 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-a-service-principal-and-assign-permissions"></a>Bir hizmet sorumlusu oluşturma ve izinatama

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Hizmet sorumlusunun, AKS düğümlerinin kullandığı sanal ağı ve alt ağı yönetmek için izinlere sahip olması gerekir. Bir hizmet ilkesi oluşturmak [için az reklam sp create-for-rbac][az-ad-sp-create-for-rbac] komutunu kullanın:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Aşağıdaki örnek çıktı, hizmet müdürünüz için uygulama kimliğini ve parolasını gösterir. Bu değerler, hizmet ilkesine bir rol atamak ve sonra AKS kümesini oluşturmak için ek adımlarda kullanılır:

```azurecli
az ad sp create-for-rbac --skip-assignment
```

```output
{
  "appId": "476b3636-5eda-4c0e-9751-849e70b5cfad",
  "displayName": "azure-cli-2019-01-09-22-29-24",
  "name": "http://azure-cli-2019-01-09-22-29-24",
  "password": "a1024cd7-af7b-469f-8fd7-b293ecbb174e",
  "tenant": "72f998bf-85f1-41cf-92ab-2e7cd014db46"
}
```

Kalan adımlarda doğru delegasyonları atamak için, gerekli kaynak adlarını almak için [az ağ vnet show][az-network-vnet-show] ve az network [vnet subnet show][az-network-vnet-subnet-show] komutlarını kullanın. Bu kaynak adlar değişken olarak depolanır ve kalan adımlarda başvurulmaktadır:

```azurecli-interactive
VNET_ID=$(az network vnet show --resource-group myResourceGroup --name myAKSVnet --query id -o tsv)
SUBNET_ID=$(az network vnet subnet show --resource-group myResourceGroup --vnet-name myAKSVnet --name myAKSSubnet --query id -o tsv)
```

Şimdi [az rol atama oluşturma][az-role-assignment-create] komutunu kullanarak sanal ağdaki AKS küme *Katılımcısı* izinleriniz için hizmet ata. Hizmet ilkesini oluşturmak için önceki komuttan çıktıda gösterildiği gibi kendi * \<appId>* sağlayın:

```azurecli-interactive
az role assignment create --assignee <appId> --scope $VNET_ID --role Contributor
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Sanal ağda AKS kümesi oluşturma

Artık bir sanal ağ ve alt ağ oluşturdunuz ve bir hizmet sorumlusunun bu ağ kaynaklarını kullanması için izinler oluşturdunuz ve atadınız. Şimdi [az aks komutu kullanarak][az-aks-create] sanal ağ ve alt ağ bir AKS küme oluşturun. Hizmet sorumlusu oluşturmak için önceki * \< *komuttan çıktıda gösterildiği gibi kendi hizmet temel * \<appId>* ve parola>tanımlayın.

Aşağıdaki IP adresi aralıkları küme oluşturma işleminin bir parçası olarak da tanımlanır:

* *--service-cidr,* AKS kümesinde bir IP adresi iç hizmetleri atamak için kullanılır. Bu IP adres aralığı, ağ ortamınızın başka yerlerinde kullanılmayan bir adres alanı olmalıdır. Bu aralık, Express Route veya Siteden Siteye VPN bağlantısı kullanarak Azure sanal ağlarınıza bağlanır veya bağlanmayı planlarsanız şirket içi ağ aralıklarını içerir.

* *--dns-service-ip* adresi, hizmet IP adres aralığınızın *0,10* adresi olmalıdır.

* *--pod-cidr,* ağ ortamınızın başka yerlerinde kullanılmayan büyük bir adres alanı olmalıdır. Bu aralık, Express Route veya Siteden Siteye VPN bağlantısı kullanarak Azure sanal ağlarınıza bağlanır veya bağlanmayı planlarsanız şirket içi ağ aralıklarını içerir.
    * Bu adres aralığı, ölçeklendirmeyi beklediğiniz düğüm sayısını karşılayacak kadar büyük olmalıdır. Ek düğümler için daha fazla adrese ihtiyacınız varsa küme dağıtıldıktan sonra bu adres aralığını değiştiremezsiniz.
    * Pod IP adresi aralığı kümedeki her düğüme *bir /24* adres alanı atamak için kullanılır. Aşağıdaki örnekte, *--pod-cidr* *10.244.0.0/16* ilk düğüm *10.244.0.0/24*, ikinci düğüm *10.244.1.0/24*ve üçüncü düğüm *10.244.2.0/24*atar.
    * Küme ölçeklendikçe veya yükseltilirken, Azure platformu her yeni düğüme bir pod IP adresi aralığı atamaya devam eder.
    
* *--docker-bridge adresi,* AKS düğümlerinin temel yönetim platformuyla iletişim kurmasına olanak tanır. Bu IP adresi kümenizin sanal ağ IP adres aralığında olmamalıdır ve ağınızdaki diğer adres aralıklarıyla örtüşmemelidir.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --network-plugin kubenet \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --pod-cidr 10.244.0.0/16 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal <appId> \
    --client-secret <password>
```

> [!Note]
> Bir AKS kümesinin [bir Calico ağ ilkesi][calico-network-policies] ni içermesini sağlamak istiyorsanız aşağıdaki komutu kullanabilirsiniz.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --network-plugin kubenet --network-policy calico \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --pod-cidr 10.244.0.0/16 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal <appId> \
    --client-secret <password>
```

Bir AKS kümesi oluşturduğunuzda, bir ağ güvenlik grubu ve rota tablosu oluşturulur. Bu ağ kaynakları AKS denetim düzlemi tarafından yönetilir. Ağ güvenlik grubu düğümlerinizdeki sanal NIC'lerle otomatik olarak ilişkilidir. Rota tablosu sanal ağ alt ağıyla otomatik olarak ilişkilidir. Ağ güvenlik grubu kuralları ve rota tabloları, siz hizmetleri oluştururken ve açığa çıkarırken otomatik olarak güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Varolan sanal ağ alt ağınıza dağıtılan bir AKS kümesiyle, artık kümeyi normal olarak kullanabilirsiniz. [Azure Dev Spaces'i kullanarak veya][dev-spaces] [Taslak'ı kullanarak][use-draft]uygulama oluşturmaya veya [Miğfer'i kullanarak uygulamaları dağıtmaya][use-helm]başlayın.

<!-- LINKS - External -->
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[Calico-network-policies]: https://docs.projectcalico.org/v3.9/security/calico-network-policy

<!-- LINKS - Internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-network-concepts]: concepts-network.md
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[use-helm]: kubernetes-helm.md
[use-draft]: kubernetes-draft.md
[virtual-nodes]: virtual-nodes-cli.md
[vnet-peering]: ../virtual-network/virtual-network-peering-overview.md
[express-route]: ../expressroute/expressroute-introduction.md
[network-comparisons]: concepts-network.md#compare-network-models
