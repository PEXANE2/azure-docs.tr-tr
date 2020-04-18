---
title: Azure Kubernetes Hizmetinde (AKS) API sunucusu yetkili IP aralıkları
description: Azure Kubernetes Hizmeti'ndeki (AKS) API sunucusuna erişmek için bir IP adres aralığını kullanarak kümenizi nasıl güvene alabildiğinizi öğrenin
services: container-service
ms.topic: article
ms.date: 11/05/2019
ms.openlocfilehash: 570d842409fc019d24446e091f83402f4c288d7c
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640047"
---
# <a name="secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti 'nde (AKS) yetkili IP adresi aralıklarını kullanarak API sunucusuna güvenli erişim

Kubernetes'te, API sunucusu kümede kaynak oluşturmak veya düğüm sayısını ölçeklendirmek gibi eylemleri gerçekleştirmek için istekler alır. API sunucusu, bir kümeyle etkileşim kurmanın ve yönetmenin merkezi yoludur. Küme güvenliğini artırmak ve saldırıları en aza indirmek için API sunucusuna yalnızca sınırlı sayıda IP adresi aralığından erişilebiliyor olması gerekir.

Bu makalede, hangi IP adreslerinin ve CidR'lerin denetim düzlemine erişebileceğini sınırlamak için API sunucusu yetkili IP adresi aralıklarını nasıl kullanacağınızı gösterir.

> [!IMPORTANT]
> Yeni kümelerde, API sunucusu yetkili IP adresi aralıkları yalnızca *Standart* SKU yük dengeleyicisi üzerinde desteklenir. *Temel* SKU yük dengeleyicisi ve API sunucusu namına yetkilendirilen IP adresi aralıklarına sahip varolan kümeler olduğu gibi çalışmaya devam eder, ancak *Standart* SKU yük dengeleyicisine geçirilemez. Bu varolan kümeler, Kubernetes sürümü veya kontrol düzlemi yükseltilirse de çalışmaya devam eder.

## <a name="before-you-begin"></a>Başlamadan önce

API sunucusu yetkili IP aralıkları yalnızca oluşturduğunuz yeni AKS kümeleri için çalışır. Bu makalede, Azure CLI'yi kullanarak bir AKS kümesinin nasıl oluşturulabileceğiniz gösterilmektedir.

Azure CLI sürümü 2.0.76 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>API sunucusu yetkili IP aralıklarına genel bakış

Kubernetes API sunucusu, altta yatan Kubernetes API'lerinin nasıl ortaya teşhir ilerler. Bu bileşen, Kubernetes panosu `kubectl` gibi yönetim araçları için etkileşim sağlar. AKS, özel bir API sunucusuyla tek kiracılı küme yöneticisi sağlar. Varsayılan olarak, API sunucusuna ortak bir IP adresi atanır ve rolütabanlı erişim denetimlerini (RBAC) kullanarak erişimi denetlemeniz gerekir.

Aksi takdirde kamuya açık AKS kontrol düzlemi / API sunucusuna erişimi güvenli hale getirmek için, yetkili IP aralıkları etkinleştirebilir ve kullanabilirsiniz. Bu yetkili IP aralıkları yalnızca tanımlanmış IP adresi aralıklarının API sunucusuyla iletişim kurmasına izin verir. Bu yetkili IP aralıklarının parçası olmayan bir IP adresinden API sunucusuna yapılan istek engellenir. Kullanıcıları ve isteklerini yerine getirmek için RBAC'ı kullanmaya devam edin.

API sunucusu ve diğer küme bileşenleri hakkında daha fazla bilgi [için AKS için Kubernetes temel kavramlarına][concepts-clusters-workloads]bakın.

## <a name="create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled"></a>API sunucusu yetkili IP aralıkları etkin leştirilmiş bir AKS kümesi oluşturma

API sunucusu yetkili IP aralıkları yalnızca yeni AKS kümeleri için çalışır. [Az aks'ı][az-aks-create] kullanarak bir küme oluşturun ve yetkili IP adres aralıklarının listesini sağlamak için *--api-server-authorized-ip-ranges* parametresini belirtin. Bu IP adresi aralıkları genellikle şirket içi ağlarınız veya genel IP'leriniz tarafından kullanılan adres aralıklarıdır. Bir CIDR aralığı belirttiğiniz zaman, aralıktaki ilk IP adresiyle başlayın. Örneğin, *137.117.106.90/29* geçerli bir aralıktır, ancak *137.117.106.88/29*gibi aralıktaki ilk IP adresini belirttiğinden emin olun.

> [!IMPORTANT]
> Varsayılan olarak, kümeniz giden ağ geçidini yapılandırmak için kullanabileceğiniz [Standart SKU yük dengeleyicisini][standard-sku-lb] kullanır. Küme oluşturma sırasında API sunucusu yetkili IP aralıklarını etkinleştirdiğinizde, kümenizin genel IP'sine de belirttiğiniz aralıklara ek olarak varsayılan olarak izin verilir. *--api-server-authorized-ip-ranges*için *""* veya hiçbir değer belirtmeniz durumunda, API sunucusu yetkili IP aralıkları devre dışı bırakılır. PowerShell kullanıyorsanız, herhangi bir ayrıştırma sorunu önlemek için *--api-server-authorized-ip-ranges=""* (eşit ler işareti ile) kullanın.

Aşağıdaki örnek, API sunucusu yetkili IP aralıkları etkin olan *myResourceGroup* adlı kaynak grubunda *myAKSCluster* adlı tek düğümlü bir küme oluşturur. İzin verilen IP adres aralıkları *73.140.245.0/24:*

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --api-server-authorized-ip-ranges 73.140.245.0/24 \
    --generate-ssh-keys
```

> [!NOTE]
> Bu aralıkları izin veren bir listeye eklemeniz gerekir:
> - Güvenlik duvarı genel IP adresi
> - Kümeyi yöneteceğiniz ağları temsil eden herhangi bir aralık
> - AKS kümenizde Azure Dev Spaces kullanıyorsanız, [bölgenize göre ek aralıklara][dev-spaces-ranges]izin verebilirsiniz.

> Belirtebileceğiniz IP aralıkları sayısı için üst sınır 3500'dür. 

### <a name="specify-the-outbound-ips-for-the-standard-sku-load-balancer"></a>Standart SKU yük dengeleyicisi için giden IP'leri belirtin

Bir AKS kümesi oluştururken, küme için giden IP adreslerini veya öneklerini belirtirseniz, bu adreslere veya öneklere de izin verilir. Örneğin:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --api-server-authorized-ip-ranges 73.140.245.0/24 \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2> \
    --generate-ssh-keys
```

Yukarıdaki örnekte, parametrede sağlanan tüm IP'ler *-load-balancer-outbound-ip-önekleri* *--api-server-authorized-ip-ranges* parametredeki IP'lerle birlikte izin verilir.

Alternatif olarak, giden yük bakiyesi IP öneklerine izin vermek için *--yük bakiyesi-giden-ip önekleri* parametresini belirtebilirsiniz.

### <a name="allow-only-the-outbound-public-ip-of-the-standard-sku-load-balancer"></a>Standart SKU yük dengeleyicisinin yalnızca giden halka açık IP'sini izin verin

Küme oluşturma sırasında API sunucusu yetkili IP aralıklarını etkinleştirdiğinizde, kümeniz için Standart SKU yük dengeleyicisinin giden genel IP'sine, belirttiğiniz aralıklara ek olarak varsayılan olarak izin verilir. Standart SKU yük dengeleyicisinin yalnızca giden halka açık IP'sine izin vermek için *--api-server-authorized-ip-ranges* parametresini belirtirken *0.0.0.0/32* kullanın.

Aşağıdaki örnekte, yalnızca Standart SKU yük dengeleyicisinin giden genel IP'sine izin verilir ve API sunucusuna yalnızca küme içindeki düğümlerden erişebilirsiniz.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --api-server-authorized-ip-ranges 0.0.0.0/32 \
    --generate-ssh-keys
```

## <a name="update-a-clusters-api-server-authorized-ip-ranges"></a>Kümenin API sunucusunun yetkili IP aralıklarını güncelleştirme

Varolan bir kümede YETKILI IP aralıklarını güncelleştirmek için [az aks güncelleme][az-aks-update] komutunu kullanın ve *--api-server-authorized-ip-ranges*, *--load-balancer-outbound-ip-önekleri*, *--load-balancer-outbound-ips*veya *--load-balancer-outbound-ip-önekleri* parametrelerini kullanın.

Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myAKSCluster* adlı kümedeki API sunucusu yetkili IP aralıklarını güncelleştirir. Yetki verilen IP adresi aralığı *73.140.245.0/24:*

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges  73.140.245.0/24
```

*--api-server yetkili-ip aralıkları* parametresini, Standart SKU yük bakiyeleyicisinin yalnızca genel IP'sine izin vermek için belirtirken *0.0.0.0/32* de kullanabilirsiniz.

## <a name="disable-authorized-ip-ranges"></a>Yetkili IP aralıklarını devre dışı

Yetkili IP aralıklarını devre dışı kullanabilirsiniz, [az aks güncelleştirmesini][az-aks-update] kullanın ve yetkili IP aralıklarını devre dışı kakmak için boş bir aralık belirtin. Örneğin:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, API sunucusunun yetkili IP aralıklarını etkinleştirdin. Bu yaklaşım, güvenli bir AKS kümesini çalıştırmanın bir parçasıdır.

Daha fazla bilgi [için, AKS'deki uygulamalar ve kümeler için Güvenlik kavramları][concepts-security] na ve [AKS'deki küme güvenliği ve yükseltmeleri için en iyi uygulamalara][operator-best-practices-cluster-security]bakın.

<!-- LINKS - external -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[dev-spaces-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[kubenet]: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#kubenet

<!-- LINKS - internal -->
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[route-tables]: ../virtual-network/manage-route-table.md
[standard-sku-lb]: load-balancer-standard.md
