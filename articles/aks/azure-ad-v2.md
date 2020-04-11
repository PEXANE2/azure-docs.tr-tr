---
title: Azure Kubernetes Hizmetinde Azure AD'yi kullanma
description: Azure Kubernetes Hizmetinde (AKS) Azure AD'yi nasıl kullanacağınızı öğrenin
services: container-service
manager: gwallace
ms.topic: article
ms.date: 03/24/2020
ms.openlocfilehash: b121830192a2b88185bbbbc9a92934e51b32a61c
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81114657"
---
# <a name="integrate-azure-ad-in-azure-kubernetes-service-preview"></a>Azure AD'yi Azure Kubernetes Hizmetine Entegre Edin (Önizleme)

> [!Note]
> AD tümleştirmesi ile mevcut AKS v1 kümeleri yeni AKS v2 deneyiminden etkilenmez.

AKS v2 ile Azure AD tümleştirmesi, kullanıcıların bir istemci uygulaması, bir sunucu uygulaması oluşturması gereken ve Azure AD kiracısının Dizin Okuma izinleri vermesini gerektiren AKS v1 deneyimiyle Azure AD tümleştirmesini basitleştirmek üzere tasarlanmıştır. Yeni sürümde, AKS kaynak sağlayıcısı sizin için istemci ve sunucu uygulamalarını yönetir.

## <a name="limitations"></a>Sınırlamalar

* Şu anda varolan bir Azure AD etkinleştirilmiş AKS v1 kümesini v2 deneyimine yükseltemezsiniz.

> [!IMPORTANT]
> AKS önizleme özellikleri self servis, tercih bazında mevcuttur. Önizlemeler "olduğu gibi" ve "olduğu gibi" sağlanır ve Hizmet Düzeyi Anlaşmaları ve sınırlı garanti kapsamı dışındadır. AKS önizlemeleri kısmen en iyi çaba temelinde müşteri desteği tarafından karşılanır. Bu nedenle, bu özellikler üretim kullanımı için değildir. Daha fazla bilgi için aşağıdaki destek makalelerini görün:
>
> - [AKS Destek Politikaları](support-policies.md)
> - [Azure Destek SSS](faq.md)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki kaynakların yüklü olması gerekir:

- Azure CLI, sürüm 2.2.0 veya sonrası
- Aks-önizleme 0.4.38 uzantısı
- [1.18 beta](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#client-binaries) minimum sürümü ile Kubectl

Aks önizleme uzantısını yüklemek/güncellemek veya daha sonra aşağıdaki Azure CLI komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

```azurecli
az extension update --name aks-preview
az extension list
```

Kubectl yüklemek için aşağıdakileri kullanın:

```azurecli
sudo az aks install-cli
kubectl version --client
```

Diğer işletim sistemleri için [bu yönergeleri](https://kubernetes.io/docs/tasks/tools/install-kubectl/) kullanın.

> [!CAUTION]
> Bir özelliği bir abonelikte kaydettirdikten sonra, bu özelliğin kaydını şu anda kullanabilirsiniz. Bazı önizleme özelliklerini etkinleştirdiğinizde, varsayılanlar abonelikten sonra oluşturulan tüm AKS kümeleri için kullanılabilir. Üretim aboneliklerinde önizleme özelliklerini etkinleştirme. Bunun yerine, önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name AAD-V2 --namespace Microsoft.ContainerService
```

Durumun **Kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az özellik listesi](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AAD-V2')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak gösterildiğinde, az `Microsoft.ContainerService` sağlayıcı kayıt komutunu kullanarak kaynak [sağlayıcısının](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Azure AD etkinleştirilmiş bir AKS kümesi oluşturma

Artık aşağıdaki CLI komutlarını kullanarak bir AKS kümesi oluşturabilirsiniz.

İlk olarak, bir Azure kaynak grubu oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

Ardından, bir AKS kümesi oluşturun:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad
```
Yukarıdaki komut üç düğüm AKS kümesi oluşturur, ancak kümeyi oluşturan kullanıcı varsayılan olarak, bu kümeye erişimi olan bir grubun üyesi değildir. Bu kullanıcının bir Azure REKLAM grubu oluşturması, kendilerini grubun bir üyesi olarak eklemesi ve ardından kümeyi aşağıda gösterildiği gibi güncelleştirmesi gerekir. Talimatları [buradan](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) takip edin

Bir grup oluşturduktan ve üye olarak kendinizi (ve başkalarını) ekledikten sonra, aşağıdaki komutu kullanarak kümeyi Azure AD grubuyla güncelleştirebilirsiniz

```azurecli-interactive
az aks update -g MyResourceGroup -n MyManagedCluster [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```
Alternatif olarak, önce bir grup oluşturup üye eklerseniz, aşağıdaki komutu kullanarak Azure REKLAM grubunu oluşturma zamanında etkinleştirebilirsiniz,

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```

Bir Azure AD v2 kümesinin başarılı bir şekilde oluşturulması, yanıt gövdesinde aşağıdaki bölüme sahiptir
```
"Azure ADProfile": {
    "adminGroupObjectIds": null,
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Küme birkaç dakika içinde oluşturulur.

## <a name="access-an-azure-ad-enabled-cluster"></a>Azure AD etkin kümeye erişin
Kümeye erişmek için yönetici kimlik bilgilerini almak için:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster --admin
```
Şimdi kümedeki düğümleri görüntülemek için kubectl get düğümleri komutunu kullanın:

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```

Kümeye erişmek için kullanıcı kimlik bilgilerini almak için:
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Oturum açma yönergelerini izleyin.

Alırsınız: **Sunucuda oturum açmış olmalısınız (Yetkisiz)**

Yukarıdaki kullanıcı kümeye erişimi olan bir grubun parçası olmadığından bir hata alır.

## <a name="next-steps"></a>Sonraki adımlar

Azure [AD Role Based Access Denetimi][azure-ad-rbac]hakkında bilgi edinin.

<!-- LINKS - Internal -->
[azure-ad-rbac]: azure-ad-rbac.md
