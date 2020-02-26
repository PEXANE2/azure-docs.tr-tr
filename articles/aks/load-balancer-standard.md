---
title: Azure Kubernetes Service (AKS) içinde standart bir SKU yük dengeleyici kullanma
description: Azure Kubernetes Service (AKS) ile hizmetlerinizi kullanıma sunmak için standart SKU ile yük dengeleyiciyi nasıl kullanacağınızı öğrenin.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 09/27/2019
ms.author: zarhoads
ms.openlocfilehash: 3fe4de2b8b85e603bc200b27fa15c67f6cc05dd4
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77595170"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içinde standart bir SKU yük dengeleyici kullanma

Azure Kubernetes Service (AKS) içinde `LoadBalancer` türündeki Kubernetes Hizmetleri aracılığıyla uygulamalara erişim sağlamak için bir Azure Load Balancer kullanabilirsiniz. AKS üzerinde çalışan bir yük dengeleyici, iç veya dış yük dengeleyici olarak kullanılabilir. İç yük dengeleyici, bir Kubernetes hizmetini yalnızca AKS kümesi ile aynı sanal ağda çalışan uygulamalar için erişilebilir hale getirir. Dış yük dengeleyici, giriş için bir veya daha fazla genel IP alır ve genel IP 'Leri kullanarak bir Kubernetes hizmetini dışarıdan erişilebilir hale getirir.

Azure Load Balancer, *temel* ve *Standart*olmak üzere iki SKU 'da kullanılabilir. Varsayılan olarak, bir AKS kümesi oluşturduğunuzda *Standart* SKU kullanılır. *Standart* SKU yük dengeleyici kullanmak, daha büyük bir arka uç havuzu boyutu ve kullanılabilirlik alanları gibi ek özellikler ve işlevler sağlar. Kullanmayı seçmeden önce *Standart* ve *temel* yük dengeleyiciler arasındaki farkları anlamanız önemlidir. Bir AKS kümesi oluşturduktan sonra, bu küme için yük dengeleyici SKU 'sunu değiştiremezsiniz. *Temel* ve *Standart* SKU 'lar hakkında daha fazla bilgi için bkz. [Azure yük dengeleyici SKU karşılaştırması][azure-lb-comparison].

Bu makalede, Kubernetes ve Azure Load Balancer kavramlarının temel bir şekilde anlaşıldığı varsayılır. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts] ve [Azure Load Balancer nedir?][azure-lb].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.81 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][install-azure-cli].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, *Standart* SKU 'nun Azure Load Balancer bir aks kümeniz olduğunu varsaymaktadır. AKS kümesine ihtiyacınız varsa bkz. [Azure CLI kullanarak][aks-quickstart-cli] aks hızlı başlangıç veya [Azure Portal kullanımı][aks-quickstart-portal].

AKS küme hizmeti sorumlusu Ayrıca mevcut bir alt ağ veya kaynak grubu kullanıyorsanız ağ kaynaklarını yönetme iznine de sahip olmalıdır. Genel olarak, temsilcili kaynaklar üzerindeki hizmet sorumlusu rolüne *ağ katılımcısı* rolünü atayın. İzinler hakkında daha fazla bilgi için bkz. [diğer Azure kaynaklarına AKS erişimi verme][aks-sp].

### <a name="moving-from-a-basic-sku-load-balancer-to-standard-sku"></a>Temel SKU Load Balancer standart SKU 'ya taşıma

Temel SKU 'nun Load Balancer var olan bir kümeniz varsa, standart SKU Load Balancer sahip bir küme kullanmaya geçiş yaparken dikkat etmeniz önemli davranış farklılıkları vardır.

Örneğin, kümeleri geçirmek için mavi/yeşil dağıtımlar yapmak, bir kümenin `load-balancer-sku` türü için yalnızca küme oluşturma zamanında tanımlandıkları yaygın bir uygulamadır. Ancak, *temel SKU* yük dengeleyiciler *Standart* SKU IP adreslerini gerektirdiğinden *Standart* SKU yük dengeleyiciler Ile uyumlu olmayan *temel SKU* IP adreslerini kullanır. Kümeleri Load Balancer SKU 'Larına geçirirken, uyumlu bir IP adresi SKU 'SU olan yeni bir IP adresi gerekir.

Kümeleri geçirme hakkında daha fazla bilgi için, geçiş yaparken göz önünde bulundurmanız gereken önemli konuların bir listesini görüntülemek için [geçiş konuları hakkındaki belgelerimizi](acs-aks-migration.md) ziyaret edin. Aşağıdaki sınırlamalar Ayrıca AKS 'de standart SKU yük dengeleyicileri kullanırken dikkat edilecek önemli davranış farklarıdır.

### <a name="limitations"></a>Sınırlamalar

*Standart* SKU ile yük dengeleyiciyi destekleyen aks kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* AKS kümesinden giden trafiğe izin vermek için en az bir genel IP veya IP öneki gereklidir. Genel IP veya IP öneki Ayrıca, denetim düzlemi ve aracı düğümleri arasındaki bağlantıyı sürdürmek için ve önceki AKS sürümleriyle uyumluluğu sürdürmek için de gereklidir. *Standart* bir SKU yük dengeleyiciye sahip genel IP 'LERI veya IP öneklerini belirtmek için aşağıdaki seçenekleriniz vardır:
    * Kendi genel IP 'nizi sağlayın.
    * Kendi genel IP öneklerinizi sağlayın.
    * AKS kümesinin, genellikle başlangıçta *Mc_* olarak adlandırılan aks kümesi olarak oluşturulan aynı kaynak grubunda *Standart* SKU genel IP 'leri oluşturmasına izin vermek için en fazla 100 ' a kadar bir sayı belirtin. AKS, genel IP 'yi *Standart* SKU yük dengeleyicisine atar. Varsayılan olarak, genel IP, genel IP öneki veya IP sayısı belirtilmemişse, AKS kümesiyle aynı kaynak grubunda bir genel IP otomatik olarak oluşturulur. Ayrıca, genel adreslere izin vermeniz ve IP oluşturma ile ilgili herhangi bir Azure Ilkesinin oluşturulmasını önetmeniz gerekir.
* Yük Dengeleyici için *Standart* SKU kullanırken, Kubernetes sürüm *1,13 veya üstünü*kullanmanız gerekir.
* Yük dengeleyici SKU 'SU tanımlama yalnızca bir AKS kümesi oluşturduğunuzda yapılabilir. Bir AKS kümesi oluşturulduktan sonra yük dengeleyici SKU 'sunu değiştiremezsiniz.
* Tek bir kümede yalnızca bir tür yük dengeleyici SKU 'SU (temel veya standart) kullanabilirsiniz.
* *Standart* SKU yük dengeleyiciler yalnızca *Standart* SKU IP adreslerini destekler.

## <a name="use-the-standard-sku-load-balancer"></a>*Standart* SKU yük dengeleyiciyi kullanma

Bir AKS kümesi oluşturduğunuzda, varsayılan olarak, bu kümede Hizmetleri çalıştırdığınızda *Standart* SKU yük dengeleyici kullanılır. Örneğin, [Azure CLI kullanan hızlı başlangıç][aks-quickstart-cli] *Standart* SKU yük dengeleyiciyi kullanan bir örnek uygulama dağıtır.

> [!IMPORTANT]
> Kullanıcı tanımlı bir yol (UDR) özelleştirilerek genel IP adreslerine kaçınılabilir. Bir AKS kümesinin giden türünü UDR olarak belirtmek, AKS tarafından oluşturulan Azure Yük Dengeleyici için IP sağlama ve arka uç havuzu kurulumunu atlayabilir. Bkz. [küme `outboundType` ' userDefinedRouting ' olarak ayarlanıyor](egress-outboundtype.md).

## <a name="configure-the-load-balancer-to-be-internal"></a>Yük dengeleyiciyi iç olarak yapılandırma

Ayrıca yük dengeleyiciyi iç olarak yapılandırabilir ve genel bir IP 'yi kullanıma sunmamalısınız. Yük dengeleyiciyi iç olarak yapılandırmak için, *LoadBalancer* hizmetine bir ek açıklama olarak `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` ekleyin. [Burada][internal-lb-yaml]bir YAML bildiriminin yanı sıra bir iç yük dengeleyici hakkında daha fazla ayrıntı görebilirsiniz.

## <a name="scale-the-number-of-managed-public-ips"></a>Yönetilen genel IP sayısını ölçeklendirme

Varsayılan olarak oluşturulan, yönetilen giden genel IP 'Ler ile *Standart* SKU yük dengeleyicisi kullanırken, *yük dengeleyici-yönetilen-IP-Count* parametresini kullanarak yönetilen giden genel IP sayısını ölçeklendirebilirsiniz.

Var olan bir kümeyi güncelleştirmek için aşağıdaki komutu çalıştırın. Bu parametre, birden çok yönetilen giden genel IP 'si olması için küme oluşturma zamanında de ayarlanabilir.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

Yukarıdaki örnek, *Myresourcegroup*Içindeki *Myakscluster* kümesi Için yönetilen giden genel IP sayısını *2* ' ye ayarlar. 

Ayrıca, `--load-balancer-managed-outbound-ip-count` parametresini ekleyerek ve istediğiniz değere ayarlayarak, kümenizi oluştururken yönetilen giden genel IP 'lerin ilk sayısını ayarlamak için *Load-dengeleyiciyle yönetilen-IP-Count* parametresini de kullanabilirsiniz. Varsayılan yönetilen giden genel IP sayısı 1 ' dir.

## <a name="provide-your-own-public-ips-or-prefixes-for-egress"></a>Çıkış için kendi genel IP 'Leri veya ön eklerini sağlayın

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

```azurecli-interactive
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

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

## <a name="configure-outbound-ports-and-idle-timeout"></a>Giden bağlantı noktalarını ve boşta kalma zaman aşımını yapılandırma

> [!WARNING]
> Aşağıdaki bölüm, daha büyük ölçekli ağın gelişmiş senaryolarına veya varsayılan yapılandırmalara karşı SNAT tükenmesi sorunlarını gidermeye yöneliktir. Sağlıklı kümeleri sürdürmek için *AllocatedOutboundPorts* veya *IdleTimeout ınminutes* değerlerini varsayılan değerinden değiştirmeden önce, VM 'ler ve IP adresleri için uygun olan kotanın doğru envanterine sahip olmanız gerekir.
> 
> *AllocatedOutboundPorts* ve *IdleTimeout ınminutes* değerlerinin değiştirilmesi, yük dengeleyiciniz için giden kuralın davranışını önemli ölçüde değiştirebilir. Değişikliklerinizin etkisini tam olarak anlamak üzere bu değerleri güncelleştirmeden önce, Azure 'daki giden kuralları, [yük dengeleyici giden kurallarını][azure-lb-outbound-rules]ve [giden bağlantıları][azure-lb-outbound-connections] [Load Balancer][azure-lb-outbound-rules-overview]gözden geçirin.

Giden ayrılmış bağlantı noktaları ve bunların boşta kalma zaman aşımları [SNAT][azure-lb-outbound-connections]için kullanılır. Varsayılan olarak, *Standart* SKU 'su yük dengeleyici, arka uç havuz boyutunu ve her bağlantı noktası için 30 dakikalık boşta kalma zaman aşımını [temel alan giden bağlantı noktası sayısı için otomatik atamayı][azure-lb-outbound-preallocatedports] kullanır. Bu değerleri görmek için [az Network lb giden kuralı listesini][az-network-lb-outbound-rule-list] kullanarak yük dengeleyicinin giden kuralını görüntüleyin:

```azurecli-interactive
NODE_RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az network lb outbound-rule list --resource-group $NODE_RG --lb-name kubernetes -o table
```

Önceki komutlarda yük dengeleyiciniz için giden kuralı listelenir, örneğin:

```console
AllocatedOutboundPorts    EnableTcpReset    IdleTimeoutInMinutes    Name             Protocol    ProvisioningState    ResourceGroup
------------------------  ----------------  ----------------------  ---------------  ----------  -------------------  -------------
0                         True              30                      aksOutboundRule  All         Succeeded            MC_myResourceGroup_myAKSCluster_eastus  
```

Örnek çıktıda *AllocatedOutboundPorts* ve *IdleTimeout ınminutes*için varsayılan değer gösterilmektedir. *AllocatedOutboundPorts* için 0 değeri, arka uç havuz boyutunu temel alan giden bağlantı noktalarının sayısı için otomatik atamayı kullanan giden bağlantı noktası sayısını ayarlar. Örneğin, kümede 50 veya daha az düğüm varsa, her düğüm için 1024 bağlantı noktaları ayrılır.

Yukarıdaki varsayılan yapılandırmaya göre 1 yüz SNAT tükenmesi beklemeniz durumunda *allocatedOutboundPorts* veya *IdleTimeout ınminutes* ayarını değiştirmeyi göz önünde bulundurun. Her ek IP adresi, ayırma için 64.000 ek bağlantı noktası sunar, ancak Azure Standart Load Balancer, daha fazla IP adresi eklendiğinde düğüm başına bağlantı noktalarını otomatik olarak artırmaz. *Yük dengeleyici-giden-bağlantı noktaları* ve *yük dengeleyici-boşta kalma zaman aşımı* parametrelerini ayarlayarak bu değerleri değiştirebilirsiniz. Örnek:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ports 0 \
    --load-balancer-idle-timeout 30
```

> [!IMPORTANT]
> Bağlantı veya ölçeklendirme sorunlarından kaçınmak için *allocatedOutboundPorts* 'i özelleştirmeden önce [gerekli kotayı hesaplamanız][calculate-required-quota] gerekir. *AllocatedOutboundPorts* için belirttiğiniz değer ayrıca 8 ' in katı olmalıdır.

Ayrıca, bir küme oluştururken *yük dengeleyici-giden-bağlantı noktaları* ve *yük dengeleyici-boşta kalma zaman aşımı* parametrelerini de kullanabilirsiniz, ancak Ayrıca *yük dengeleyici-yönetilen-giden-IP-sayısı*, *yük dengeleyici-giden-* IP veya *yük dengeleyici-giden-IP-öneklerini* de belirtmeniz gerekir.  Örnek:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-managed-outbound-ip-count 2 \
    --load-balancer-outbound-ports 0 \
    --load-balancer-idle-timeout 30
```

*Yük dengeleyici-giden-bağlantı noktaları* ve *yük dengeleyici-boşta kalma zaman aşımı* parametrelerini varsayılan olarak değiştirme sırasında, tüm kümeyi etkileyen yük dengeleyici profilinin davranışını etkiler.

### <a name="required-quota-for-customizing-allocatedoutboundports"></a>AllocatedOutboundPorts özelleştirmek için gereken kota
Düğüm sanal makinelerinizin sayısına ve istenen ayrılmış giden bağlantı noktalarına bağlı olarak yeterli giden IP kapasitesine sahip olmanız gerekir. Yeterli sayıda çıkış IP kapasitesi olduğunu doğrulamak için aşağıdaki formülü kullanın: 
 
*Outboundips* \* 64.000 \> *nodevms* \* *desiredAllocatedOutboundPorts*.
 
Örneğin, 3 *Nodevms*ve 50.000 *desiredAllocatedOutboundPorts*varsa, en az 3 *outboundıps*gerekir. İhtiyaç duyduğunuz süreden daha fazla giden IP kapasitesi eklemeniz önerilir. Ayrıca, giden IP kapasitesini hesaplarken küme otomatik Scaler ve düğüm havuzu yükseltmeleri olasılığa yönelik hesaba sahip olmanız gerekir. Küme otomatik yüklemesi için geçerli düğüm sayısını ve en fazla düğüm sayısını gözden geçirin ve daha yüksek değeri kullanın. Yükseltme için, yükseltmeye izin veren her düğüm havuzu için ek bir düğüm VM 'si hesabı.
 
*Idletimeoutınminutes* değerini varsayılan değer olan 30 dakikadan farklı bir değere ayarlarken, iş yüklerinizin giden bir bağlantıya ne kadar süreyle ihtiyacı olacağını düşünün. Ayrıca, AKS dışında kullanılan *Standart* SKU yük dengeleyici için varsayılan zaman aşımı değerini 4 dakikadır. Belirli AKS iş yükünüzü daha doğru bir şekilde yansıtan bir *ıdletimeoutınminutes* değeri, artık kullanılmayan bağlantıların AYıKLANMASıNDAN kaynaklanan SNAT tükenmesi azalmasına yardımcı olabilir.

## <a name="restrict-access-to-specific-ip-ranges"></a>Belirli IP aralıklarına erişimi kısıtla

Yük Dengeleyici için sanal ağla ilişkili ağ güvenlik grubu (NSG), varsayılan olarak tüm gelen dış trafiğe izin veren bir kural içerir. Bu kuralı, gelen trafik için yalnızca belirli IP aralıklarına izin verecek şekilde güncelleştirebilirsiniz. Aşağıdaki bildirim, gelen dış trafik için yeni bir IP aralığı belirtmek üzere *Loadbalancersourceranges* kullanır:

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
  loadBalancerSourceRanges:
  - MY_EXTERNAL_IP_RANGE
```

Yukarıdaki örnek, kuralı yalnızca *MY_EXTERNAL_IP_RANGE* aralığından gelen dış trafiğe izin verecek şekilde güncelleştirir. Yük dengeleyici hizmetine erişimi kısıtlamak için bu yöntemi kullanma hakkında daha fazla bilgi, [Kubernetes belgelerinde][kubernetes-cloud-provider-firewall]bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes Services [belgelerindeki][kubernetes-services]Kubernetes hizmetleri hakkında daha fazla bilgi edinin.

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubernetes-cloud-provider-firewall]: https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/#restrict-access-for-loadbalancer-service
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
[az-network-lb-outbound-rule-list]: /cli/azure/network/lb/outbound-rule?view=azure-cli-latest#az-network-lb-outbound-rule-list
[az-network-public-ip-show]: /cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/concepts-limitations.md#skus
[azure-lb-outbound-rules]: ../load-balancer/load-balancer-outbound-rules-overview.md#snatports
[azure-lb-outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#snat
[azure-lb-outbound-preallocatedports]: ../load-balancer/load-balancer-outbound-connections.md#preallocatedports
[azure-lb-outbound-rules-overview]: ../load-balancer/load-balancer-outbound-rules-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[calculate-required-quota]: #required-quota-for-customizing-allocatedoutboundports
