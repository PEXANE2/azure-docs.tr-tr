---
title: Azure Kubernetes hizmetini (AKS) başlatma ve durdurma
description: Bir Azure Kubernetes hizmeti (AKS) kümesini durdurmayı veya başlatmayı öğrenin.
services: container-service
ms.topic: article
ms.date: 09/24/2020
author: palma21
ms.openlocfilehash: 4a814d575e0879daec64ebfdabc1539219bea250
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91368949"
---
# <a name="stop-and-start-an-azure-kubernetes-service-aks-cluster-preview"></a>Azure Kubernetes hizmeti (AKS) kümesini durdurma ve başlatma (Önizleme)

AKS iş yüklerinizin, örneğin yalnızca iş saatlerinde kullanılan bir geliştirme kümesi gibi sürekli olarak çalıştırılması gerekmez. Bu, Azure Kubernetes hizmeti (AKS) kümenizin, sistem bileşenlerinden daha fazla olmayan şekilde boşta kalabileceği sürelere yol açar. [Tüm `User` düğüm havuzlarını 0 olarak ölçeklendirerek](scale-cluster.md#scale-user-node-pools-to-0)küme parmak izini azaltabilirsiniz, ancak küme çalışırken [ `System` havuzunuzun](use-system-pools.md) sistem bileşenlerini çalıştırmak için yine de gereklidir. Bu dönemler sırasında maliyetlerinizi daha iyi hale getirebilmeniz için kümenizi tamamen kapatabilirsiniz (durdurabilirsiniz). Bu eylem, denetim düzlemi ve aracı düğümlerinizi tamamen durdurup tüm işlem maliyetlerine kaydetmenizi sağlayarak, yeniden başlattığınızda tüm nesnelerinizi ve küme eyaletlerini sürdürmenizi sağlar. Daha sonra, bir hafta sonuna kadar kaldığınız yerden veya kümenizi yalnızca Batch işlerinizi çalıştırdığınızda çalışır durumda olacak şekilde seçebilirsiniz.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, mevcut bir AKS kümeniz olduğunu varsaymaktadır. AKS kümesine ihtiyacınız varsa bkz. [Azure CLI kullanarak][aks-quickstart-cli] aks hızlı başlangıç veya [Azure Portal kullanımı][aks-quickstart-portal].


### <a name="limitations"></a>Sınırlamalar

Küme Başlat/Durdur özelliğini kullanırken aşağıdaki kısıtlamalar geçerlidir:

- Bu özellik yalnızca sanal makine ölçek kümeleri desteklenen kümeler için desteklenir.
- Önizleme süresince, bu özellik özel kümeler için desteklenmez.
- Durdurulan bir AKS kümesinin küme durumu 12 aya kadar korunur. Kümeniz 12 aydan uzun bir süre için durdurulmuşsa, küme durumu kurtarılamaz. Daha fazla bilgi için bkz. [aks destek ilkeleri](support-policies.md).
- Yalnızca durdurulmuş bir AKS kümesini başlatabilir ya da silebilirsiniz. Ölçek veya yükseltme gibi işlemleri gerçekleştirmek için önce kümenizi başlatın.

### <a name="install-the-aks-preview-azure-cli"></a>`aks-preview`Azure CLI 'yı yükler 

Ayrıca, *aks-Preview* Azure CLI uzantısı sürüm 0.4.64 veya sonraki bir sürüme de ihtiyacınız vardır. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükler. Veya [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir tüm güncelleştirmeleri yükler.

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
``` 

### <a name="register-the-startstoppreview-preview-feature"></a>`StartStopPreview`Önizleme özelliğini kaydetme

Kümeyi Başlat/Durdur özelliğini kullanmak için `StartStopPreview` aboneliğinizde Özellik bayrağını etkinleştirmeniz gerekir.

`StartStopPreview`Aşağıdaki örnekte gösterildiği gibi, [az Feature Register][az-feature-register] komutunu kullanarak özellik bayrağını kaydedin:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "StartStopPreview"
```

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu doğrulayın:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/StartStopPreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="stop-an-aks-cluster"></a>AKS kümesini durdurma

`az aks stop`Çalıştıran BIR AKS kümesinin düğümlerini ve denetim düzlemi 'ni durdurmak için komutunu kullanabilirsiniz. Aşağıdaki örnek, *Myakscluster*adlı bir kümeyi durduruyor:

```azurecli-interactive
az aks stop --name myAKSCluster --resource-group myResourceGroup
```

[Az aks Show][az-aks-show] komutunu kullanarak kümenizin ne zaman durdurulmuş olduğunu ve `powerState` aşağıdaki çıktıda gösterildiği gibi gösterir `Stopped` :

```json
{
[...]
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_westus2",
  "powerState":{
    "code":"Stopped"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
[...]
}
```

Gösteriyorsa, `provisioningState` `Stopping` kümenizin henüz tam olarak durdurulmadığı anlamına gelir.

> [!IMPORTANT]
> [Pod kesinti bütçeleri](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) kullanıyorsanız, boşaltma işleminin tamamlanması daha fazla zaman alacak şekilde durdurma işlemi daha uzun sürebilir.


## <a name="start-an-aks-cluster"></a>AKS kümesi başlatma

`az aks start`Durdurulmuş BIR AKS kümesinin düğümleri ve denetim düzlemi başlatmak için komutunu kullanabilirsiniz. Küme, önceki denetim düzlemi durumu ve aracı düğümleri sayısıyla yeniden başlatılır.  
Aşağıdaki örnek, *Myakscluster*adlı bir küme başlatır:

```azurecli-interactive
az aks start --name myAKSCluster --resource-group myResourceGroup
```

[Az aks Show][az-aks-show] komutunu kullanarak kümenizin başlatıldığı zaman ve `powerState` `Running` aşağıdaki çıktıda gösterildiği gibi gösterir:

```json
{
[...]
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_westus2",
  "powerState":{
    "code":"Running"
  },
  "privateFqdn": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
[...]
}
```

Gösteriyorsa, `provisioningState` `Starting` kümeniz henüz tam olarak başlatılmamış demektir.


## <a name="next-steps"></a>Sonraki adımlar

- Havuzları 0 ' a ölçeklendirmeye yönelik bilgi edinmek için `User` bkz. [ölçek `User` havuzları 0 olarak](scale-cluster.md#scale-user-node-pools-to-0).
- Spot örnekleri kullanarak maliyetlerin nasıl kaydedileceğini öğrenmek için bkz. [AKS 'e spot düğüm havuzu ekleme](spot-node-pool.md).
- AKS destek ilkeleri hakkında daha fazla bilgi edinmek için bkz. [aks destek ilkeleri](support-policies.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli&preserve-view=true
[az-extension-add]: /cli/azure/extension?view=azure-cli-latest#az-extension-add&preserve-view=true
[az-extension-update]: /cli/azure/extension?view=azure-cli-latest#az-extension-update&preserve-view=true
[az-feature-register]: /cli/azure/feature?view=azure-cli-latest#az-feature-register&preserve-view=true
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list&preserve-view=true
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register&preserve-view=true
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az_aks_show
