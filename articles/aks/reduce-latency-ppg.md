---
title: Azure Kubernetes hizmeti (AKS) kümelerinde gecikme süresini azaltmak için yakınlık yerleştirme gruplarını kullanın
description: AKS kümesi iş yüklerinizde gecikme süresini azaltmak için yakınlık yerleşimi gruplarını nasıl kullanacağınızı öğrenin.
services: container-service
manager: gwallace
ms.topic: article
ms.date: 07/10/2020
author: jluk
ms.openlocfilehash: f6cb370d258a79420b03baf17ec964b091cdebb7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87056580"
---
# <a name="reduce-latency-with-proximity-placement-groups-preview"></a>Yakınlık yerleştirme gruplarıyla gecikme süresini azaltma (Önizleme)

> [!Note]
> AKS üzerinde yakınlık yerleşimi grupları kullanırken, birlikte bulundurma yalnızca aracı düğümleri için geçerlidir. Düğümden düğüme ve ilgili barındırılan Pod 'ın Pod gecikme süresine kadar geliştirildi. Birlikte bulundurma, kümenin denetim düzlemi yerleştirmesini etkilemez.

Uygulamanızı Azure 'da dağıttığınızda, sanal makine (VM) örneklerinin bölgeler veya kullanılabilirlik alanları arasında yayılması ağ gecikmesi oluşturur ve bu da uygulamanızın genel performansını etkileyebilir. Yakınlık yerleşimi grubu, Azure işlem kaynaklarının fiziksel olarak birbirlerine yakın bir yerde bulunduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Oyun, mühendislik benzetimleri ve yüksek frekanslı ticaret (HFT) gibi bazı uygulamalar, düşük gecikme süresi ve hızla tamamlanan görevler gerektirir. Bunlar gibi yüksek performanslı bilgi işlem (HPC) senaryolarında, kümenizin düğüm havuzları için [yakınlık yerleşimi grupları](../virtual-machines/linux/co-location.md#proximity-placement-groups) (PPG) kullanmayı düşünün.

## <a name="limitations"></a>Sınırlamalar

* Yakınlık yerleşimi grubu en fazla bir kullanılabilirlik bölgesine eşlenir.
* Bir yakınlık yerleşimi grubunu ilişkilendirmek için bir düğüm havuzunun sanal makine ölçek kümelerini kullanması gerekir.
* Düğüm havuzu, yalnızca düğüm havuzu oluşturma zamanında bir yakınlık yerleşimi grubunu ilişkilendirebilir.

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
> AKS düğüm havuzlarıyla yakınlık yerleştirme grupları kullanırken, birlikte bulundurma yalnızca aracı düğümleri için geçerlidir. Düğümden düğüme ve ilgili barındırılan Pod 'ın Pod gecikme süresine kadar geliştirildi. Birlikte bulundurma, kümenin denetim düzlemi yerleştirmesini etkilemez.

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

### <a name="configure-proximity-placement-groups-with-availability-zones"></a>Kullanılabilirlik alanları ile yakınlık yerleşimi gruplarını yapılandırma

> [!NOTE]
> Yakınlık yerleştirme grupları, en fazla bir kullanılabilirlik alanında kullanmak üzere bir düğüm havuzu gerektirdiğinde, tek bir bölgedeki VM 'Ler için [% 99,9 olan temel Azure VM SLA 'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_9/) hala geçerli olur.

Yakınlık yerleştirme grupları bir düğüm havuzu kavramıdır ve tek tek düğüm havuzıyla ilişkilendirilir. Bir PPG kaynağı kullanmanın AKS denetim düzlemi kullanılabilirliği üzerinde hiçbir etkisi yoktur. Bu, bir kümenin bölgelerle nasıl tasarlanmalıdır. Bir kümenin birden çok bölgeye yayılmasını sağlamak için aşağıdaki tasarımın kullanılması önerilir.

* 3 bölge kullanarak ilk sistem havuzu ile bir küme sağlayın ve hiçbir yakınlık yerleştirme grubu ilişkilendirilmez. Bu, sistem ayırımlarının birden çok bölgeye yayılan ayrılmış bir düğüm havuzunda yer almasını sağlar.
* Her havuz ile ilişkili benzersiz bir bölge ve yakınlık yerleşimi grubu olan ek Kullanıcı düğümü havuzları ekleyin. Nodepool1 bölge 1 ve PPG1, nodepool2 bölge 2 ve PPG2, nodepool3 bölge 3 ' te PPG3 bir örnektir. Bu, bir küme düzeyinde, düğümlerin birden çok bölgeye yayılmasını ve her bir düğüm havuzunun ayrılmış bir PPG kaynağı ile belirlenen bölgede birlikte yer almasını sağlar.

## <a name="create-a-new-aks-cluster-with-a-proximity-placement-group"></a>Yakınlık yerleştirme grubuyla yeni bir AKS kümesi oluşturma

Aşağıdaki örnek, *merkezde ABD* bölgesinde *myresourcegroup* adlı bir kaynak grubu oluşturmak için [az Group Create][az-group-create] komutunu kullanır. *Myakscluster* adlı bir aks kümesi daha sonra [az aks Create][az-aks-create] komutu kullanılarak oluşturulur.

Hızlandırılmış ağ, sanal makinelerin ağ performansını önemli ölçüde geliştirir. İdeal olarak, hızlandırılmış ağ ile birlikte yakınlık yerleştirme gruplarını kullanın. Varsayılan olarak, AKS, en fazla iki veya daha fazla vCPU içeren Azure sanal makinesini içeren [desteklenen sanal makine örneklerinde](../virtual-network/create-vm-accelerated-networking-cli.md?toc=/azure/virtual-machines/linux/toc.json#limitations-and-constraints)hızlandırılmış ağ kullanır.

İlk sistem düğüm havuzuyla ilişkili bir yakınlık yerleşimi grubu ile yeni bir AKS kümesi oluşturun:

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
# Create an AKS cluster that uses a proximity placement group for the initial system node pool only. The PPG has no effect on the cluster control plane.
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
[proximity-placement-groups]: ../virtual-machines/linux/co-location.md#proximity-placement-groups
[az-aks-create]: /cli/azure/aks#az-aks-create
[system-pool]: ./use-system-pools.md
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest#az-aks-nodepool-add
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
