---
title: Azure Kubernetes hizmeti (AKS) kümelerinde gecikme süresini azaltmak için yakınlık yerleştirme gruplarını kullanın
description: AKS kümesi iş yüklerinizde gecikme süresini azaltmak için yakınlık yerleşimi gruplarını nasıl kullanacağınızı öğrenin.
services: container-service
manager: gwallace
ms.topic: article
ms.date: 06/22/2020
ms.openlocfilehash: 382849c30c3520bbb208886de5f302f59fdac98b
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85374466"
---
# <a name="reduce-latency-with-proximity-placement-groups-preview"></a>Yakınlık yerleştirme gruplarıyla gecikme süresini azaltma (Önizleme)

> [!Note]
> AKS ile yakınlık yerleşimi grupları kullandığınızda, birlikte bulundurma yalnızca aracı düğümleri için geçerlidir. Düğümden düğüme ve ilgili barındırılan Pod 'ın Pod gecikme süresine kadar geliştirildi. Birlikte bulundurma, kümenin denetim düzlemi yerleştirmesini etkilemez.

Uygulamanızı Azure 'da dağıttığınızda, sanal makine (VM) örneklerinin bölgeler veya kullanılabilirlik alanları arasında yayılması ağ gecikmesi oluşturur ve bu da uygulamanızın genel performansını etkileyebilir. Yakınlık yerleşimi grubu, Azure işlem kaynaklarının fiziksel olarak birbirlerine yakın bir yerde bulunduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Oyun, mühendislik benzetimleri ve yüksek frekanslı ticaret (HFT) gibi bazı uygulamalar, düşük gecikme süresi ve hızla tamamlanan görevler gerektirir. Bunlar gibi yüksek performanslı bilgi işlem (HPC) senaryolarında, kümenizin düğüm havuzları için [yakınlık yerleştirme gruplarını](https://docs.microsoft.com/azure/virtual-machines/linux/co-location#proximity-placement-groups) kullanmayı düşünün.

## <a name="limitations"></a>Sınırlamalar

* Yakınlık yerleşimi grubu tek bir kullanılabilirlik bölgesine yayılmıştır.
* Sanal makine kullanılabilirlik kümeleri kullanan AKS kümeleri için geçerli destek yoktur.
* Var olan düğüm havuzlarını bir yakınlık yerleşimi grubu kullanacak şekilde değiştiremezsiniz.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme esasına göre sunulmaktadır. Önizlemeler "olduğu gibi" ve "kullanılabildiği gibi" verilmiştir ve hizmet düzeyi anlaşmalarından ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteği tarafından kısmen bir en iyi performans kapsamında ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Daha fazla bilgi için aşağıdaki destek makalelerine bakın:
>
> - [AKS destek Ilkeleri](support-policies.md)
> - [Azure desteği SSS](faq.md)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki kaynakların yüklü olması gerekir:

- Aks-Preview 0.4.53 uzantısı

### <a name="set-up-the-preview-feature-for-proximity-placement-groups"></a>Yakınlık yerleşimi grupları için önizleme özelliğini ayarlama

> [!IMPORTANT]
> AKS ile yakınlık yerleşimi grupları kullandığınızda, birlikte bulundurma yalnızca aracı düğümleri için geçerlidir. Düğümden düğüme ve ilgili barındırılan Pod 'ın Pod gecikme süresine kadar geliştirildi. Birlikte bulundurma, kümenin denetim düzlemi yerleştirmesini etkilemez.

```azurecli-interactive
# register the preview feature
az feature register --namespace "Microsoft.ContainerService" --name "ProximityPlacementGroupPreview"
```

Kayıt için bu işlem birkaç dakika sürebilir. Özelliğin kaydedildiğini doğrulamak için aşağıdaki komutu kullanın:

```azurecli-interactive
# Verify the feature is registered:
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/ProximityPlacementGroupPreview')].{Name:name,State:properties.state}"
```

Önizleme süresince, yakınlık yerleşimi gruplarını kullanabilmeniz için *aks-Preview* CLI uzantısının olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanın ve [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeler olup olmadığını denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
## <a name="node-pools-and-proximity-placement-groups"></a>Düğüm havuzları ve yakınlık yerleştirme grupları

Bir yakınlık yerleşimi grubuyla dağıttığınız ilk kaynak belirli bir veri merkezine bağlanır. Aynı yakınlık yerleşimi grubuyla dağıtılan ek kaynaklar aynı veri merkezinde birlikte bulunur. Yakınlık yerleşimi grubunu kullanan tüm kaynaklar durdurulduktan sonra (serbest bırakıldığında) veya silindikten sonra artık eklenmez.

* Birçok düğüm havuzu tek bir yakınlık yerleşimi grubuyla ilişkilendirilebilir.
* Düğüm havuzu yalnızca tek bir yakınlık yerleşimi grubuyla ilişkilendirilebilir.

## <a name="create-a-new-aks-cluster-with-a-proximity-placement-group"></a>Yakınlık yerleştirme grubuyla yeni bir AKS kümesi oluşturma

Aşağıdaki örnek, *merkezde ABD* bölgesinde *myresourcegroup* adlı bir kaynak grubu oluşturmak için [az Group Create][az-group-create] komutunu kullanır. *Myakscluster* adlı bir aks kümesi daha sonra [az aks Create][az-aks-create] komutu kullanılarak oluşturulur. 

Hızlandırılmış ağ, sanal makinelerin ağ performansını önemli ölçüde geliştirir. İdeal olarak, hızlandırılmış ağ ile birlikte yakınlık yerleştirme gruplarını kullanın. Varsayılan olarak, AKS, en fazla iki veya daha fazla vCPU içeren Azure sanal makinesini içeren [desteklenen sanal makine örneklerinde](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=/azure/virtual-machines/linux/toc.json#limitations-and-constraints)hızlandırılmış ağ kullanır.

Yakınlık yerleştirme grubuyla yeni bir AKS kümesi oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```
Aşağıdaki komutu çalıştırın ve döndürülen KIMLIĞI depolayın:

```azurecli-interactive
# Create proximity placement group
az ppg create -n myPPG -g myResourceGroup -l centralus -t standard
```

Komut, yaklaşan CLı komutları için ihtiyaç duyduğunuz *kimlik* değerini içeren çıktıyı üretir:

```output
{
  "availabilitySets": null,
  "colocationStatus": null,
  "id": "/subscriptions/yourSubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.Compute/proximityPlacementGroups/myPPG",
  "location": "centralus",
  "name": "myPPG",
  "proximityPlacementGroupType": "Standard",
  "resourceGroup": "myResourceGroup",
  "tags": {},
  "type": "Microsoft.Compute/proximityPlacementGroups",
  "virtualMachineScaleSets": null,
  "virtualMachines": null
}
```

Aşağıdaki komutta *Myppgresourceıd* değeri için yakınlık yerleşimi grubu kaynak kimliğini kullanın:

```azurecli-interactive
# Create an AKS cluster that uses a proximity placement group for the initial node pool
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --ppg myPPGResourceID
```

## <a name="add-a-proximity-placement-group-to-an-existing-cluster"></a>Var olan bir kümeye yakınlık yerleştirme grubu ekleme

Yeni bir düğüm havuzu oluşturarak var olan bir kümeye bir yakınlık yerleşimi grubu ekleyebilirsiniz. Daha sonra isteğe bağlı olarak, mevcut iş yüklerini yeni düğüm havuzuna geçirebilir ve sonra özgün düğüm havuzunu silebilirsiniz.

Daha önce oluşturduğunuz yakınlık yerleşimi grubunu kullanın ve bu, AKS kümenizdeki her iki düğüm havuzunda bulunan aracı düğümlerinin fiziksel olarak aynı veri merkezinde yer aldığından emin olur.

Daha önce oluşturduğunuz yakınlık yerleşimi grubundan kaynak KIMLIĞI ' ni kullanın ve komutla yeni bir düğüm havuzu ekleyin [`az aks nodepool add`][az-aks-nodepool-add] :

```azurecli-interactive
# Add a new node pool that uses a proximity placement group, use a --node-count = 1 for testing
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 1 \
    --ppg myPPGResourceID
```

## <a name="clean-up"></a>Temizleme

Kümeyi silmek için, [`az group delete`][az-group-delete] AKS kaynak grubunu silmek için komutunu kullanın:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

* [Yakınlık yerleşimi grupları][proximity-placement-groups]hakkında daha fazla bilgi edinin.

<!-- LINKS - Internal -->
[azure-ad-rbac]: azure-ad-rbac.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[proximity-placement-groups]: /virtual-machines/windows/co-location.md#proximity-placement-groups/virtual-machines/linux/co-location
[az-aks-create]: /cli/azure/aks#az-aks-create
[system-pool]: ./use-system-pools.md
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete

