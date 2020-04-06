---
title: Standart SKU yük dengeleyicisi kullanın
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti (AKS) ile hizmetlerinizi ortaya çıkarmak için Standart SKU'lu bir yük dengeleyicisini nasıl kullanacağınızı öğrenin.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 09/27/2019
ms.author: zarhoads
ms.openlocfilehash: 17e474de9c221126d67cc2982ba11c6ff75e7aa3
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668503"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Standart SKU yük dengeleyicisi kullanın

Azure Kubernetes `LoadBalancer` Hizmeti'ndeki (AKS) Kubernetes hizmetleri aracılığıyla uygulamalara erişim sağlamak için bir Azure Yük Dengeleyicisi kullanabilirsiniz. AKS üzerinde çalışan bir yük dengeleyici dahili veya harici yük dengeleyici olarak kullanılabilir. Dahili yük dengeleyicisi, Kubernetes hizmetini yalnızca AKS kümesiyle aynı sanal ağda çalışan uygulamalar için erişilebilir kılar. Harici yük dengeleyicisi giriş için bir veya daha fazla genel IP alır ve bir Kubernetes hizmetini genel IP'leri kullanarak harici olarak erişilebilir hale getirir.

Azure Yük Dengeleyicisi, *Temel* ve *Standart*olmak için iki SCARI'de kullanılabilir. Varsayılan olarak, bir AKS kümesi oluşturduğunuzda *Standart* SKU kullanılır. *Standart* SKU yük dengeleyicisi kullanmak, daha büyük bir arka uç havuzu boyutu ve Kullanılabilirlik Bölgeleri gibi ek özellikler ve işlevler sağlar. Hangisini kullanacağınızı seçmeden önce *Standart* ve *Temel* yük dengeleyicileri arasındaki farkları anlamanız önemlidir. Bir AKS kümesi oluşturduktan sonra, bu kümeiçin yük dengeleyici SKU'yu değiştiremezsiniz. *Temel* ve *Standart* SKU'lar hakkında daha fazla bilgi için [Azure yük dengeleyici SKU karşılaştırması'na][azure-lb-comparison]bakın.

Bu makalede, Kubernetes ve Azure Yük Dengeleyici kavramları temel bir anlayış varsayar. Daha fazla bilgi için Azure [Kubernetes Hizmeti (AKS)][kubernetes-concepts] ve [Azure Yük Dengeleyicisi nedir?][azure-lb]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.81 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, *Standart* SKU Azure Yük Dengeleyicisi ile bir AKS kümeniz olduğunu varsayar. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Varolan bir alt ağ veya kaynak grubu kullanıyorsanız, AKS küme hizmeti ilkesinin ağ kaynaklarını yönetmek için de izin alması gerekir. Genel olarak, ağ *katılımcısı* rolünü devredilen kaynaklardaki hizmet yöneticinize atayın. İzinler hakkında daha fazla bilgi için, [aks temsilcisinin diğer Azure kaynaklarına erişimine][aks-sp]bakın.

### <a name="moving-from-a-basic-sku-load-balancer-to-standard-sku"></a>Temel SKU Yük Dengeleyicisinden Standart SKU'ya geçme

Temel SKU Yük Dengeleyicisi ile varolan bir kümeniz varsa, Standart SKU Yük Dengeleyicisi ile bir küme kullanmak için geçiş yaparken dikkat edilmesi gereken önemli davranış farklılıkları vardır.

Örneğin, kümeleri geçirmek için mavi/yeşil dağıtımlar yapmak, `load-balancer-sku` küme nin türü yalnızca küme oluşturma zamanında tanımlanabildiği göz önüne alındığında yaygın bir uygulamadır. Ancak, *Temel SKU* Yük Dengeleyicileri, *Standart* SKU IP Adresleri gerektirdiği için *Standart SKU* Yük Dengeleyicileri ile uyumlu olmayan *Temel SKU* IP Adreslerini kullanır. Yük Dengeleyici SKU'larını yükseltmek için kümeleri geçiş yaparken, uyumlu BIR IP Adresi SKU'ya sahip yeni bir IP adresi gerekir.

Kümeleri nasıl geçirteceklerine ilişkin daha fazla dikkat için, geçiş yaparken göz önünde bulundurulması gereken önemli konuların listesini görüntülemek için [geçiş le ilgili belgelerimizi](aks-migration.md) ziyaret edin. Aks'te Standart SKU Yük Dengeleyicileri kullanırken dikkat edilmesi gereken önemli davranış farklılıkları da aşağıdaki sınırlamalardır.

### <a name="limitations"></a>Sınırlamalar

*Standart* SKU ile yük dengeleyicisini destekleyen AKS kümeleri oluştururken ve yönetirken aşağıdaki sınırlamalar geçerlidir:

* AKS kümesinden çıkış trafiğine izin vermek için en az bir genel IP veya IP öneki gereklidir. Ortak IP veya IP öneki, denetim düzlemi ve aracı düğümleri arasındaki bağlantıyı korumak ve AKS'nin önceki sürümleriyle uyumluluğu korumak için de gereklidir. *Standart* SKU yük dengeleyicisi ile genel IP veya IP önekleri belirtmek için aşağıdaki seçeneklere sahipsiniz:
    * Kendi genel IP'lerinizi sağlayın.
    * Kendi genel IP öneklerinizi sağlayın.
    * AKS kümesinin aks kümesiyle oluşturulan aynı kaynak grubunda, genellikle başında *MC_* ile birlikte adlandırılmış olan aynı kaynak grubunda bu kadar çok *Standart* SKU ortak IP'si oluşturmasına izin vermek için 100'e kadar bir sayı belirtin. AKS, kamu IP'sini *Standart* SKU yük dengeleyicisine atar. Varsayılan olarak, ortak IP, genel IP öneki veya IP sayısı belirtilmemişse, aks kümesiyle aynı kaynak grubunda otomatik olarak bir genel IP oluşturulur. Ayrıca herkese açık adreslere izin vermeli ve IP oluşturmayı yasaklayan herhangi bir Azure İlkesi oluşturmaktan kaçınmalısınız.
* Bir yük dengeleyicisi için *Standart* SKU'yu kullanırken, Kubernetes sürüm *1.13 veya daha büyük*bir sürüm kullanmanız gerekir.
* Yük dengeleyicisi SKU'yu tanımlama, yalnızca bir AKS kümesi oluşturduğunuzda yapılabilir. Bir AKS kümesi oluşturulduktan sonra yük dengeleyici SKU'yu değiştiremezsiniz.
* Tek bir kümede yalnızca bir yük dengeleyici sku (Temel veya Standart) türü kullanabilirsiniz.
* *Standart* SKU Yük Dengeleyicileri yalnızca *Standart* SKU IP Adreslerini destekler.

## <a name="use-the-standard-sku-load-balancer"></a>*Standart* SKU yük dengeleyicisini kullanın

Bir AKS kümesi oluşturduğunuzda, varsayılan olarak, bu kümedeki hizmetleri çalıştırdığınızda *Standart* SKU yük dengeleyicisi kullanılır. Örneğin, [Azure CLI'yi kullanan hızlı başlangıç,][aks-quickstart-cli] *Standart* SKU yük dengeleyicisini kullanan bir örnek uygulama dağıtıyor.

> [!IMPORTANT]
> Ortak IP adresleri, kullanıcı tanımlı bir rotayı (UDR) özelleştirerek önlenebilir. UDR olarak bir AKS kümesinin giden türünü belirtme, AKS oluşturulan Azure yük dengeleyicisi için IP sağlama ve arka uç havuzu kurulumlarını atlayabilir. Bir [kümenin 'userDefinedRouting' `outboundType` olarak ayarlanmasına](egress-outboundtype.md)bakın.

## <a name="configure-the-load-balancer-to-be-internal"></a>Yük dengeleyicisini dahili olacak şekilde yapılandırın

Ayrıca, yük dengeleyicisini dahili olacak şekilde yapılandırabilir ve genel bir IP'yi açığa çıkarmazsınız. Yük bakiyesini dahili olarak yapılandırmak için `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` *LoadBalancer* hizmetine ek açıklama olarak ekleyin. Burada bir örnek yaml manifestoyanı sıra bir iç yük [dengeleyici][internal-lb-yaml]hakkında daha fazla bilgi görebilirsiniz.

## <a name="scale-the-number-of-managed-public-ips"></a>Yönetilen genel IP'lerin sayısını ölçeklendirin

Varsayılan olarak oluşturulan yönetilen giden genel IP'lere sahip *standart* bir SKU yük dengeleyicisini kullanırken, yük *bakiyesi yönetilen ip sayısı* parametresini kullanarak yönetilen giden genel IP'lerin sayısını ölçeklendirebilirsiniz.

Varolan bir kümeyi güncelleştirmek için aşağıdaki komutu çalıştırın. Bu parametre, birden çok yönetilen giden genel IP'lere sahip olmak için küme oluşturma zamanında da ayarlanabilir.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

Yukarıdaki örnekte, *myResourceGroup'taki* *myAKSCluster* kümesi için yönetilen giden genel IP'lerin sayısı *2* olarak ayarlanır. 

Ayrıca, `--load-balancer-managed-outbound-ip-count` kümenizi oluştururken, parametreyi ekleyerek ve istediğiniz değere ayarlayarak yönetilen giden genel IP'lerin ilk sayısını ayarlamak için *yük bakiyesi yönetilen ip sayısı* parametresini de kullanabilirsiniz. Yönetilen giden genel IP'lerin varsayılan sayısı 1'dir.

## <a name="provide-your-own-public-ips-or-prefixes-for-egress"></a>Çıkış için kendi genel IP'lerinizi veya öneklerinizi sağlayın

*Standart* SKU yük dengeleyicisi kullanırken, AKS kümesi aks kümesi için oluşturulan aynı kaynak grubunda otomatik olarak bir genel IP oluşturur ve ortak IP'yi *Standart* SKU yük dengeleyicisine atar. Alternatif olarak, küme oluşturma zamanında kendi ortak IP'nizi atayabilir veya varolan kümenin yük dengeleyici özelliklerini güncelleştirebilirsiniz.

Birden çok IP adresi veya önek getirerek, tek bir yük dengeleyici nesnesinin arkasındaki IP adresini tanımlarken birden çok destek hizmeti tanımlayabilirsiniz. Belirli düğümlerin çıkış bitiş noktası, hangi hizmetle ilişkili olduklarına bağlıdır.

> [!IMPORTANT]
> *Standart* SKU'nuzla çıkış için *Standart* SKU kamu IP'lerini kullanmanız gerekir. Az [network public-ip show][az-network-public-ip-show] komutunu kullanarak ortak IP'lerinizin SKU'unu doğrulayabilirsiniz:
>
> ```azurecli-interactive
> az network public-ip show --resource-group myResourceGroup --name myPublicIP --query sku.name -o tsv
> ```

Ortak IP'lerinizin dislerini listelemek için [az network public-ip show][az-network-public-ip-show] komutunu kullanın.

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

Yukarıdaki komut, *myResourceGroup* kaynak grubunda *myPublicIP* genel IP için kimlik gösterir.

Kümenizi genel IP'lerinizle güncellemek için *yük bakiyesi-giden ips* parametresi ile *az aks güncelleme* komutunu kullanın.

Aşağıdaki örnekte, önceki komuttaki dislerle *yük bakiyesi-giden ips* parametresi kullanır.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

*Standart* SKU yük bakiyecinizle çıkış için genel IP önekleri de kullanabilirsiniz. Aşağıdaki örnekte, genel IP öneklerinizin dislerini listelemek için [az network public-ip öneki göster][az-network-public-ip-prefix-show] komutu kullanır:

```azurecli-interactive
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

Yukarıdaki komut, *myResourceGroup* kaynak grubunda *myPublicIPPrefix* public IP önekinin kimliğini gösterir.

Aşağıdaki örnekte, önceki komuttaki dislerle *yük bakiyesi-giden-ip önekleri* parametresi kullanır.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

> [!IMPORTANT]
> Genel IP'ler ve IP önekleri, AKS kümenizle aynı bölgede ve aynı aboneliğin bir parçası olmalıdır. 

### <a name="define-your-own-public-ip-or-prefixes-at-cluster-create-time"></a>Küme oluşturma zamanında kendi genel IP'nizi veya öneklerinizi tanımlayın

Beyaz liste çıkış uç noktaları gibi senaryoları desteklemek için küme oluşturma zamanında çıkış için kendi IP adreslerinizi veya IP öneklerinizi getirmek isteyebilirsiniz. Bir kümenin yaşam döngüsünün başlangıcında kendi genel IP'lerinizi ve IP öneklerinizi tanımlamak için küme oluşturma adımınızın yukarıda gösterilen parametrelerin aynısını ekin.

Başlangıçta ortak IP'lerinizle yeni bir küme oluşturmak için *yük bakiyesi-giden-ips* parametresi ile *az aks oluşturma* komutunu kullanın.

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

Başlangıçta ortak IP önekleri ile yeni bir küme oluşturmak için *yük-balancer-outbound-ip-önekleri* parametresi ile *az aks oluşturma* komutunu kullanın.

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

## <a name="configure-outbound-ports-and-idle-timeout"></a>Giden bağlantı noktalarını ve boşta zaman aşımlarını yapılandırma

> [!WARNING]
> Aşağıdaki bölüm, daha büyük ölçekli ağ iletişiminin gelişmiş senaryoları veya varsayılan yapılandırmalarla Birlikte SNAT tükenme sorunlarını gidermek için tasarlanmıştır. Sağlıklı kümeleri korumak için *AllocatedOutboundPorts* veya *IdleTimeoutInMinutes'i* varsayılan değerlerinden değiştirmeden önce VM'ler ve IP adresleri için kullanılabilir kotanın doğru bir envanterine sahip olmalısınız.
> 
> *AllocatedOutboundPorts* ve *IdleTimeoutInMinutes* değerlerini değiştirmek, yük bakiyeniz için giden kuralın davranışını önemli ölçüde değiştirebilir. Değişikliklerinizin etkisini tam olarak anlamak için bu değerleri güncelleştirmeden önce [Yük Bakiyesi giden kurallarını,][azure-lb-outbound-rules-overview] [Bakiyegiden][azure-lb-outbound-rules]giden kuralları ve [Giden Bağlantıları Azure'da][azure-lb-outbound-connections] gözden geçirin.

Giden ayrılan bağlantı noktaları ve boşta kalma zaman aşımları [SNAT][azure-lb-outbound-connections]için kullanılır. Varsayılan olarak, *Standart* SKU yük dengeleyici, arka uç havuzu boyutuna ve her bağlantı noktası için 30 dakikalık boşta kalma [süresine bağlı olarak giden bağlantı noktası sayısı için otomatik atama][azure-lb-outbound-preallocatedports] kullanır. Bu değerleri görmek için, yük dengeleyicisi için giden kuralı göstermek için [az ağ lb giden kural listesini][az-network-lb-outbound-rule-list] kullanın:

```azurecli-interactive
NODE_RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az network lb outbound-rule list --resource-group $NODE_RG --lb-name kubernetes -o table
```

Önceki komutlar, örneğin yük bakiyecinizin giden kuralını listeler:

```console
AllocatedOutboundPorts    EnableTcpReset    IdleTimeoutInMinutes    Name             Protocol    ProvisioningState    ResourceGroup
------------------------  ----------------  ----------------------  ---------------  ----------  -------------------  -------------
0                         True              30                      aksOutboundRule  All         Succeeded            MC_myResourceGroup_myAKSCluster_eastus  
```

Örnek çıktı, *AllocatedOutboundPorts* ve *IdleTimeoutInMinutes*için varsayılan değeri gösterir. *AllocatedOutboundPorts* için 0 değeri, arka uç havuz boyutuna bağlı olarak giden bağlantı noktası sayısı için otomatik atama yı kullanarak giden bağlantı noktası sayısını ayarlar. Örneğin, kümede 50 veya daha az düğüm varsa, her düğüm için 1024 bağlantı noktası ayrılır.

Yukarıdaki varsayılan yapılandırmaya dayalı olarak SNAT tükenmesi ile karşılaşmayı *bekliyorsanız, ayrılan Giden Bağlantı Noktaları* veya *IdleTimeoutInMinutes* ayarını değiştirmeyi düşünün. Her ek IP adresi tahsis için 64.000 ek bağlantı noktası sağlar, ancak Azure Standart Yük Dengeleyicisi daha fazla IP adresi eklendiğinde düğüm başına bağlantı noktalarını otomatik olarak artırmaz. *Yük bakiyesi-giden bağlantı noktalarını* ve *yük bakiyesi-boşta-zaman ayarı* parametrelerini ayarlayarak bu değerleri değiştirebilirsiniz. Örnek:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ports 0 \
    --load-balancer-idle-timeout 30
```

> [!IMPORTANT]
> Bağlantı veya ölçekleme sorunlarını önlemek için *ayrılan OutboundPorts'u* özelleştirmeden önce [gerekli kotanızı hesaplamanız][calculate-required-quota] gerekir. *Tahsis Edilen Giden Bağlantı Noktaları* için belirttiğiniz değer de 8'in bir katı olmalıdır.

Küme oluştururken *yük bakiyesi-giden bağlantı noktalarını* ve *yük bakiyesi-boşta zaman ayırma* parametrelerini de kullanabilirsiniz, ancak *yük-bakiye-yönetilen-giden ip sayısı,* *yük-bakiye-giden ip sayısı*veya *yük-bakiye-giden ip-önekleri* de belirtmeniz gerekir.  Örnek:

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

*Yük bakiyesi-giden bağlantı noktalarını* ve *yük bakiyesi-boşta kalma-zaman ayarı* parametrelerini varsayılanlarından değiştirirken, tüm kümeyi etkileyen yük dengeleyici profilinin davranışını etkiler.

### <a name="required-quota-for-customizing-allocatedoutboundports"></a>Tahsis edilen OutboundPorts özelleştirmek için gerekli kota
Düğüm VM'lerinizin ve istenilen ayrılan giden bağlantı noktalarının sayısına bağlı olarak yeterli giden IP kapasitesine sahip olmalısınız. Yeterli giden IP kapasitesine sahip olduğunuzu doğrulamak için aşağıdaki formülü kullanın: 
 
*gidenIPs* \* 64.000 \> *nodeVMs* \* *istenilenAllocatedOutboundPorts*.
 
Örneğin, 3 *nodeVMs*ve 50.000 *istenilenAllocatedOutboundPorts*varsa, en az 3 *gidenIIp*olması gerekir. İhtiyacınız olanın ötesinde ek giden IP kapasitesini dahil etmeniz önerilir. Ayrıca, giden IP kapasitesi hesaplanırken küme otomatik ölçeklendirici ve düğüm havuzu yükseltmeleri olasılığını hesaba katmalısınız. Küme otomatik ölçeklendirici için geçerli düğüm sayısını ve maksimum düğüm sayısını gözden geçirin ve daha yüksek değeri kullanın. Yükseltme için, yükseltmeye izin veren her düğüm havuzu için ek bir düğüm VM'si hesapkuru.
 
*IdleTimeoutInMinutes'i* 30 dakikalık varsayılan değerden farklı bir değere ayarlarken, iş yüklerinizin giden bir bağlantıya ne kadar süre gerekeceğini düşünün. Ayrıca AKS dışında kullanılan *standart* sku yük dengeleyici için varsayılan zaman kaybı değeri 4 dakika olduğunu düşünün. Belirli AKS iş yükünüzü daha doğru bir şekilde yansıtan *Bir IdleTimeoutInMinutes* değeri, artık kullanılmayan bağlantıları bağlamanın neden olduğu SNAT yorgunluğunu azaltmaya yardımcı olabilir.

## <a name="restrict-access-to-specific-ip-ranges"></a>Belirli IP aralıklarına erişimi kısıtlama

Yük dengeleyicisi için sanal ağla ilişkili Ağ Güvenlik Grubu (NSG), varsayılan olarak, tüm gelen dış trafiğine izin veren bir kurala sahiptir. Bu kuralı yalnızca gelen trafik için belirli IP aralıklarına izin verecek şekilde güncelleştirebilirsiniz. Aşağıdaki manifesto gelen dış trafik için yeni bir IP aralığı belirtmek için *loadBalancerSourceRanges* kullanır:

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

Yukarıdaki örnek, kuralı yalnızca *MY_EXTERNAL_IP_RANGE* aralığından gelen dış trafiğe izin verecek şekilde güncelleştirir. Yük dengeleyici hizmetine erişimi kısıtlamak için bu yöntemi kullanma hakkında daha fazla bilgi [Kubernetes belgelerinde][kubernetes-cloud-provider-firewall]mevcuttur.

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes hizmetleri hakkında daha fazla bilgi için [Kubernetes hizmetleri dokümantasyonu.][kubernetes-services]

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
