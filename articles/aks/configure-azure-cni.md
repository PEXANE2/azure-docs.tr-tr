---
title: Azure Kubernetes Hizmetinde (AKS) Azure CNI ağlarını yapılandırma
description: Bir AKS kümesini varolan bir sanal ağa ve alt ağa dağıtmak da dahil olmak üzere Azure Kubernetes Hizmeti'nde (AKS) Azure CNI (gelişmiş) ağ larını nasıl yapılandıracaklarını öğrenin.
services: container-service
ms.topic: article
ms.date: 06/03/2019
ms.openlocfilehash: 400d5a46ad62f8ac391c573eb64a7eb22dc4062c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80048002"
---
# <a name="configure-azure-cni-networking-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Azure CNI ağlarını yapılandırma

Varsayılan olarak, AKS kümeleri [kubenet][kubenet]kullanır ve sizin için sanal bir ağ ve alt ağ oluşturulur. *Kubenet*ile düğümler sanal ağ alt ağlarından IP adresi alır. Ağ adresi çevirisi (NAT) daha sonra düğümler üzerinde yapılandırılır ve bölmeler düğüm IP'nin arkasında "gizli" bir IP adresi alır. Bu yaklaşım, bölmelerin kullanabilmesi için ağ alanınızda ayırmanız gereken IP adreslerinin sayısını azaltır.

[Azure Kapsayıcı Ağ Arabirimi (CNI)][cni-networking]ile her bölme alt ağdan bir IP adresi alır ve doğrudan erişilebilir. Bu IP adresleri ağ alanınızda benzersiz olmalıdır ve önceden planlanmalıdır. Her düğüm, desteklediği en fazla bölme sayısı için bir yapılandırma parametresi vardır. Düğüm başına eşdeğer IP adresi sayısı, bu düğüm için ön tarafta ayrılmıştır. Bu yaklaşım daha fazla planlama gerektirir ve genellikle IP adresi tükenmesine veya uygulama talepleriniz arttıkça kümeleri daha büyük bir alt ağda yeniden oluşturma gereksinimine yol açar.

Bu makalede, bir AKS kümesi için sanal ağ alt ağı oluşturmak ve kullanmak için *Azure CNI* ağlarını nasıl kullanacağınızı gösterir. Ağ seçenekleri ve değerlendirmelerhakkında daha fazla bilgi için [Kubernetes ve AKS için Ağ kavramlarına][aks-network-concepts]bakın.

## <a name="prerequisites"></a>Ön koşullar

* AKS kümesi için sanal ağ giden internet bağlantısına izin vermelidir.
* `169.254.0.0/16`AKS kümeleri Kubernetes `172.30.0.0/16` `172.31.0.0/16` `192.0.2.0/24` servis adresi aralığını kullanamaz.
* AKS kümesi tarafından kullanılan hizmet sorumlusunun, sanal ağınızdaki alt ağda en az [Ağ Katılımcısı](../role-based-access-control/built-in-roles.md#network-contributor) izinleri olmalıdır. Yerleşik Ağ Katılımcısı rolünü kullanmak yerine özel bir [rol](../role-based-access-control/custom-roles.md) tanımlamak istiyorsanız, aşağıdaki izinler gereklidir:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`
* Hizmet sorumlusu yerine, yönetilen kimlik atanan sistemi izinler için kullanabilirsiniz. Daha fazla bilgi için [bkz.](use-managed-identity.md)

## <a name="plan-ip-addressing-for-your-cluster"></a>Kümeniz için IP adresleme planı

Azure CNI ağıyla yapılandırılan kümeler ek planlama gerektirir. Sanal ağınızın ve alt ağının boyutu, çalıştırmayı planladığınız bölme sayısını ve küme için düğüm sayısını barındırmalıdır.

Bölmelerin IP adresleri ve kümenin düğümleri sanal ağ içinde belirtilen alt ağdan atanır. Her düğüm birincil IP adresi ile yapılandırılır. Varsayılan olarak, 30 ek IP adresi, düğümüzerinde zamanlanan bölmelere atanan Azure CNI tarafından önceden yapılandırılır. Kümenizi ölçeklediğinizde, her düğüm benzer şekilde alt ağdaki IP adresleriyle yapılandırılır. Düğüm [başına maksimum bölmeleri](#maximum-pods-per-node)de görüntüleyebilirsiniz.

> [!IMPORTANT]
> Gerekli IP adreslerinin sayısı yükseltme ve ölçekleme işlemleri için dikkate alınması gerekenleri içermelidir. IP adresi aralığını yalnızca sabit sayıda düğümü destekleyecek şekilde ayarlarsanız, kümenizi yükseltemez veya ölçeklendiremezsiniz.
>
> - AKS **upgrade** kümenizi yükselttiğinizde, kümeye yeni bir düğüm dağıtılır. Hizmetler ve iş yükleri yeni düğümüzerinde çalışmaya başlar ve kümeden eski bir düğüm kaldırılır. Bu yuvarlanma yükseltme işlemi, kullanılabilir olmak için en az bir ek IP adresi bloğu gerektirir. Düğüm sayınız o `n + 1`zaman.
>   - Windows Server düğüm havuzlarını kullandığınızda (şu anda AKS'de önizlemede) bu husus özellikle önemlidir. AKS'deki Windows Server düğümleri otomatik olarak Windows Güncelleştirmeleri uygulamaz, bunun yerine düğüm havuzunda yükseltme yaparsınız. Bu yükseltme, en son Window Server 2019 temel düğüm görüntüsü ve güvenlik yamaları ile yeni düğümleri dağır. Windows Server düğüm havuzunun yükseltilmesi hakkında daha fazla bilgi için [AKS'de düğüm havuzuyükseltme bölümüne][nodepool-upgrade]bakın.
>
> - Bir **scale** AKS kümesini ölçeklediğinizde, kümeye yeni bir düğüm dağıtılır. Hizmetler ve iş yükleri yeni düğümüzerinde çalışmaya başlar. IP adresi aralığınızın kümenizin destekleyebilecekleri düğüm ve bölme sayısını nasıl ölçeklendirmek isteyebileceğinizi göz önünde bulundurmanız gerekir. Yükseltme işlemleri için ek bir düğüm de eklenmelidir. Düğüm sayınız o `n + number-of-additional-scaled-nodes-you-anticipate + 1`zaman.

Düğümlerinizin en fazla sayıda bölme çalışmasını ve bölmeleri düzenli olarak yok etmesini ve dağıtmasını bekliyorsanız, düğüm başına bazı ek IP adreslerini de hesaba katmalısınız. Bu ek IP adresleri, bir hizmetin silinmesini ve yeni bir hizmetin dağıtılması ve adresi edinilmesi için yayımlanan IP adresinin birkaç saniye sini göz önünde bulundurabilir.

BIR AKS kümesinin IP adres planı sanal ağ, düğümler ve bölmeler için en az bir alt ağ ve Bir Kubernetes hizmet adresi aralığından oluşur.

| Adres aralığı / Azure kaynağı | Sınırlar ve boyutlandırma |
| --------- | ------------- |
| Sanal ağ | Azure sanal ağı /8 kadar büyük olabilir, ancak 65.536 yapılandırılmış IP adresiyle sınırlıdır. |
| Alt ağ | Kümenizde sağlanmış olabilecek düğümleri, bölmeleri ve tüm Kubernetes ve Azure kaynaklarını barındıracak kadar büyük olmalıdır. Örneğin, bir dahili Azure Yük Dengeleyicisi dağıtığınızda, ön uç IP'leri ortak IP'lerden değil, küme alt kümesinden ayrılır. Alt ağ boyutu da dikkate yükseltme işlemleri veya gelecekteki ölçekleme gereksinimlerini almalıdır.<p />Yükseltme işlemleri için ek bir düğüm de dahil olmak üzere *minimum* alt net boyutunu hesaplamak için:`(number of nodes + 1) + ((number of nodes + 1) * maximum pods per node that you configure)`<p/>50 düğüm kümesi için `(51) + (51  * 30 (default)) = 1,581` örnek: (/21 veya daha büyük)<p/>Ek 10 düğümü ölçeklendirmek için hükmü de içeren 50 düğüm `(61) + (61 * 30 (default)) = 1,891` kümesi için örnek: (/21 veya daha büyük)<p>Kümenizi oluşturduğunuzda düğüm başına en fazla bölme sayısı belirtmezseniz, düğüm başına en fazla bölme sayısı *30*olarak ayarlanır. Gereken minimum IP adresi sayısı bu değere bağlıdır. Minimum IP adresi gereksinimlerinizi farklı bir maksimum değerde hesaplarsanız, kümenizi dağıttığınızda bu değeri ayarlamak için [düğüm başına maksimum bölme sayısını nasıl yapılandırabileceğinizi](#configure-maximum---new-clusters) görün. |
| Kubernetes hizmeti adres aralığı | Bu aralık, bu sanal ağüzerinde veya bağlı herhangi bir ağ öğesi tarafından kullanılmamalıdır. Servis adresi CIDR /12'den küçük olmalıdır. Bu aralığı farklı AKS kümeleri arasında yeniden kullanabilirsiniz. |
| Kubernetes DNS hizmeti IP adresi | Küme hizmeti bulma (kube-dns) tarafından kullanılacak Kubernetes servis adresi aralığındaki IP adresi. Adres aralığınızdaki 0,1 gibi ilk IP adresini kullanmayın. Alt net aralığınızdaki ilk adres *kubernetes.default.svc.cluster.local* adresi için kullanılır. |
| Docker köprüsü adresi | Docker köprüsü ağ adresi, tüm Docker yüklemelerinde bulunan varsayılan *docker0* köprüsü ağ adresini temsil eder. *Docker0* köprüsü AKS kümeleri veya bölmeler kendileri tarafından kullanılmaz iken, AKS kümesi içinde *docker build* gibi senaryoları desteklemeye devam etmek için bu adresi ayarlamanız gerekir. Aksi takdirde Docker diğer CidR'ler ile çakışabilir otomatik olarak bir alt net seçecektir, çünkü Docker köprü ağ adresi için bir CIDR seçmek için gereklidir. Kümenin hizmeti CIDR ve pod CIDR dahil olmak üzere ağlarınızdaki Diğer CiR'lerle çakışmayan bir adres alanı seçmeniz gerekir. Varsayılan 172.17.0.1/16. Bu aralığı farklı AKS kümeleri arasında yeniden kullanabilirsiniz. |

## <a name="maximum-pods-per-node"></a>Düğüm başına maksimum bölme

Bir AKS kümesinde düğüm başına en fazla bölme sayısı 250'dir. Varsayılan *default* düğüm başına bölme sayısı *kubenet* ve *Azure CNI* ağ ağı ile küme dağıtım yöntemi arasında değişir.

| Dağıtım yöntemi | Kubenet varsayılan | Azure CNI varsayılan | Dağıtımda yapılandırılabilir |
| -- | :--: | :--: | -- |
| Azure CLI | 110 | 30 | Evet (250'ye kadar) |
| Resource Manager şablonu | 110 | 30 | Evet (250'ye kadar) |
| Portal | 110 | 30 | Hayır |

### <a name="configure-maximum---new-clusters"></a>Maksimum yapılandırma - yeni kümeler

Yalnızca *küme dağıtım zamanında*düğüm başına en fazla bölme sayısını yapılandırabilirsiniz. Azure CLI ile veya Kaynak Yöneticisi şablonuyla dağıtıyorsanız, düğüm başına en yüksek bölme değerini 250'ye kadar ayarlayabilirsiniz.

Küme durumu için kritik olan sistem bölmeleri için alanı garanti etmek için düğüm başına maksimum bölme için minimum değer uygulanır. Her düğüm havuzunun yapılandırmasının en az 30 pod için alanı varsa, her düğüm başına maksimum bölme için ayarlanabilecek minimum değer 10'dur. Örneğin, düğüm başına en büyük bölmeleri en az 10'a ayarlamak, her bir düğüm havuzunun en az 3 düğüm e sahip olmasını gerektirir. Bu gereksinim oluşturulan her yeni düğüm havuzu için de geçerlidir, bu nedenle 10 düğüm başına maksimum bölme olarak tanımlanırsa, sonraki düğüm havuzu eklenen en az 3 düğüm olmalıdır.

| Ağ Oluşturma | Minimum | Maksimum |
| -- | :--: | :--: |
| Azure CNI | 10 | 250 |
| Kubenet | 10 | 110 |

> [!NOTE]
> Yukarıdaki tablodaki minimum değer AKS hizmeti tarafından katı bir şekilde uygulanır. Bir maxPods değerini, kümenin başlamasını engelleyebildiği gibi gösterilen minimumdan daha düşük olarak ayarlayamam.

* **Azure CLI**: `--max-pods` [Az aks oluşturma][az-aks-create] komutu yla bir küme dağıttığınızda bağımsız değişkeni belirtin. Maksimum değer 250'dir.
* **Kaynak Yöneticisi şablonu**: Kaynak Yöneticisi şablonu olan bir küme dağıttığınızda `maxPods` [ManagedClusterAgentPoolProfile] nesnesinde özelliği belirtin. Maksimum değer 250'dir.
* **Azure portalı**: Azure portalı ile bir küme dağıttığınızda düğüm başına maksimum bölme sayısını değiştiremezsiniz. Azure portalını kullanarak dağıttığınızda Azure CNI ağ kümeleri düğüm başına 30 pod ile sınırlıdır.

### <a name="configure-maximum---existing-clusters"></a>Maksimum yapıla - varolan kümeleri

Varolan bir AKS kümesinde düğüm başına en büyük bölmeleri değiştiremezsiniz. Sayıyı yalnızca kümeyi ilk dağıttığınızda ayarlayabilirsiniz.

## <a name="deployment-parameters"></a>Dağıtım parametreleri

Bir AKS kümesi oluşturduğunuzda, Azure CNI ağı için aşağıdaki parametreler yapılandırılabilir:

**Sanal ağ**: Kubernetes kümesini dağıtmak istediğiniz sanal ağ. Kümeniz için yeni bir sanal ağ oluşturmak istiyorsanız, *yeni oluştur'u* seçin ve *sanal ağ oluştur* bölümündeki adımları izleyin. Azure sanal ağının sınırları ve kotaları hakkında bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits)bölümüne bakın.

**Alt Ağ**: Kümeyi dağıtmak istediğiniz sanal ağ içindeki alt ağ. Kümeniz için sanal ağda yeni bir alt ağ oluşturmak istiyorsanız, *yeni oluştur'u* seçin ve *alt ağ oluştur* bölümündeki adımları izleyin. Karma bağlantı için, adres aralığı ortamınızdaki diğer sanal ağlarla örtüşmemelidir.

**Kubernetes servis adresi aralığı**: Bu, Kubernetes'in kümenizdeki dahili [hizmetlere][services] atadığı sanal IP'ler kümesidir. Aşağıdaki gereksinimleri karşılayan herhangi bir özel adres aralığık ta:

* Kümenizin sanal ağ IP adres aralığında olmamalıdır
* Küme sanal ağının eşlendiği diğer sanal ağlarla çakışmamalıdır
* Şirket içi IP'lerle örtüşmemelidir
* Aralıkları `169.254.0.0/16`içinde olmamalıdır , `172.30.0.0/16` `172.31.0.0/16`, veya`192.0.2.0/24`

Kümenizle aynı sanal ağ içinde bir hizmet adresi aralığı belirtmek teknik olarak mümkün olsa da, bunu yapmak önerilmez. Çakışan IP aralıkları kullanılırsa öngörülemeyen davranışlar ortaya çıkabilir. Daha fazla bilgi için bu makalenin [SSS](#frequently-asked-questions) bölümüne bakın. Kubernetes hizmetleri hakkında daha fazla bilgi için Kubernetes belgelerindeki [Hizmetler'e][services] bakın.

**Kubernetes DNS hizmet IP adresi**: Kümenin DNS hizmetinin IP adresi. Bu adresin *Kubernetes hizmeti adres aralığı* içinde olması gerekir. Adres aralığınızdaki 0,1 gibi ilk IP adresini kullanmayın. Alt net aralığınızdaki ilk adres *kubernetes.default.svc.cluster.local* adresi için kullanılır.

**Docker Bridge adresi**: Docker köprüsü ağ adresi, tüm Docker yüklemelerinde bulunan varsayılan *docker0* köprü ağ adresini temsil eder. *Docker0* köprüsü AKS kümeleri veya bölmeler kendileri tarafından kullanılmaz iken, AKS kümesi içinde *docker build* gibi senaryoları desteklemeye devam etmek için bu adresi ayarlamanız gerekir. Aksi takdirde Docker diğer CidR'ler ile çakışabilir otomatik olarak bir alt net seçecektir, çünkü Docker köprü ağ adresi için bir CIDR seçmek için gereklidir. Kümenin hizmeti CIDR ve pod CIDR dahil olmak üzere ağlarınızdaki Diğer CiR'lerle çakışmayan bir adres alanı seçmeniz gerekir.

## <a name="configure-networking---cli"></a>Ağ yapılandırma - CLI

Azure CLI ile bir AKS kümesi oluşturduğunuzda, Azure CNI ağını da yapılandırabilirsiniz. Azure CNI ağ etkinyeni bir AKS kümesi oluşturmak için aşağıdaki komutları kullanın.

İlk olarak, AKS kümesinin birleceği varolan alt ağ için alt kaynak kimliğini alın:

```azurecli-interactive
$ az network vnet subnet list \
    --resource-group myVnet \
    --vnet-name myVnet \
    --query "[0].id" --output tsv

/subscriptions/<guid>/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Gelişmiş ağ ile bir küme `--network-plugin azure` oluşturmak için bağımsız değişken ile [az aks oluşturmak][az-aks-create] komutu kullanın. Önceki `--vnet-subnet-id` adımda toplanan alt net kimliği yle değeri güncelleştirin:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --generate-ssh-keys
```

## <a name="configure-networking---portal"></a>Ağ yapılandırma - portal

Azure portalından aşağıdaki ekran görüntüsü, AKS küme oluşturma sırasında bu ayarları yapılandırmaya bir örnek gösterir:

![Azure portalında gelişmiş ağ yapılandırması][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki sorular ve yanıtlar **Azure CNI** ağ yapılandırması için geçerlidir.

* *Küme alt amıma VM dağıtabilir miyim?*

  Hayır. Kubernetes kümeniz tarafından kullanılan alt ağa VM'ler dağıtmak desteklenmez. VM'ler aynı sanal ağda, ancak farklı bir alt ağda dağıtılabilir.

* *Pod başına ağ ilkelerini yapılandırabilir miyim?*

  Evet, Kubernetes ağ ilkesi AKS'de kullanılabilir. Başlamak için [AKS'deki ağ ilkelerini kullanarak bölmeler arasındaki güvenli trafiği][network-policy]görün.

* *Düğüme dağıtılabilir en fazla bölme sayısı yapılandırılabilir mi?*

  Evet, Azure CLI veya Kaynak Yöneticisi şablonuyla bir küme dağıttığınızda. Düğüm [başına maksimum bölmeye](#maximum-pods-per-node)bakın.

  Varolan bir kümede düğüm başına en fazla bölme sayısını değiştiremezsiniz.

* *AKS küme oluşturma sırasında oluşturduğum alt ağ için ek özellikleri nasıl yapılandırıyorum? Örneğin, hizmet bitiş noktaları.*

  AKS küme oluşturma sırasında oluşturduğunuz sanal ağ ve alt ağların özelliklerinin tam listesi Azure portalındaki standart sanal ağ yapılandırma sayfasında yapılandırılabilir.

* **Kubernetes servis adresi aralığı** *için küme sanal ağımda farklı bir alt ağ kullanabilir miyim?*

  Bu önerilmez, ancak bu yapılandırma mümkündür. Hizmet adresi aralığı, Kubernetes'in kümenizdeki dahili hizmetlere atadığı sanal IP'ler (VIP' ler) kümesidir. Azure Ağı'nın Kubernetes kümesinin hizmet IP aralığında görünürlüğü yoktur. Kümenin hizmet adresi aralığında görünürlük olmaması nedeniyle, daha sonra küme sanal ağında hizmet adresi aralığıyla çakışan yeni bir alt ağ oluşturmak mümkündür. Böyle bir çakışma oluşursa, Kubernetes bir hizmete alt ağdaki başka bir kaynak tarafından zaten kullanılmakta olan bir IP atayabilir ve bu da öngörülemeyen davranışlara veya hatalara neden olabilir. Kümenin sanal ağının dışında bir adres aralığı kullandığınızdan emin olarak, bu çakışma riskini önleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aks ağ hakkında daha fazla bilgi için aşağıdaki makalelerde:

- [Azure Kubernetes Hizmeti (AKS) yük dengeleyicisi ile statik bir IP adresi kullanın](static-ip.md)
- [Azure Kapsayıcı Hizmeti (AKS) ile dahili yük dengeleyicisi kullanın](internal-lb.md)

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Dinamik bir genel IP'ye sahip bir giriş denetleyicisi oluşturun ve TLS sertifikalarını otomatik olarak oluşturmak için Şifreleyelim'i yapılandırın][aks-ingress-tls]
- [Statik genel IP'ye sahip bir giriş denetleyicisi oluşturun ve TLS sertifikalarını otomatik olarak oluşturmak için Şifreleyelim'i yapılandırın][aks-ingress-static-tls]

### <a name="aks-engine"></a>AKS Motor

[Azure Kubernetes Hizmet Altyapısı (AKS Engine),][aks-engine] Azure'da Kubernetes kümelerini dağıtmak için kullanabileceğiniz Azure Kaynak Yöneticisi şablonları oluşturan açık kaynaklı bir projedir.

AKS Engine ile oluşturulan Kubernetes kümeleri hem [kubenet][kubenet] hem de [Azure CNI][cni-networking] eklentilerini destekler. Bu nedenle, her iki ağ senaryoları AKS Engine tarafından desteklenir.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md
[network-policy]: use-network-policies.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[network-comparisons]: concepts-network.md#compare-network-models
