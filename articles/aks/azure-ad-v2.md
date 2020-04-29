---
title: Azure Kubernetes hizmetinde Azure AD kullanma
description: Azure Kubernetes hizmetinde (AKS) Azure AD 'yi nasıl kullanacağınızı öğrenin
services: container-service
manager: gwallace
ms.topic: article
ms.date: 03/24/2020
ms.openlocfilehash: b121830192a2b88185bbbbc9a92934e51b32a61c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81114657"
---
# <a name="integrate-azure-ad-in-azure-kubernetes-service-preview"></a>Azure Kubernetes hizmeti 'nde Azure AD 'yi Tümleştirme (Önizleme)

> [!Note]
> AD tümleştirmesiyle mevcut AKS v1 kümeleri, yeni AKS v2 deneyiminden etkilenmez.

AKS v2 ile Azure AD tümleştirmesi, kullanıcıların bir istemci uygulaması, bir sunucu uygulaması oluşturmak ve dizin okuma izinleri vermek için Azure AD kiracısının gerekli olduğu AKS v1 deneyimiyle Azure AD tümleştirmesini basitleştirmek üzere tasarlanmıştır. Yeni sürümde, AKS kaynak sağlayıcısı, istemci ve sunucu uygulamalarını sizin için yönetir.

## <a name="limitations"></a>Sınırlamalar

* Şu anda mevcut bir Azure AD özellikli AKS v1 kümesini v2 deneyimine yükseltemezsiniz.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme esasına göre sunulmaktadır. Önizlemeler "olduğu gibi" ve "kullanılabildiği gibi" verilmiştir ve hizmet düzeyi anlaşmalarından ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteği tarafından kısmen bir en iyi performans kapsamında ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Daha fazla bilgi için aşağıdaki destek makalelerine bakın:
>
> - [AKS destek Ilkeleri](support-policies.md)
> - [Azure desteği SSS](faq.md)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki kaynakların yüklü olması gerekir:

- Azure CLı, sürüm 2.2.0 veya üzeri
- Aks-Preview 0.4.38 uzantısı
- Minimum [1,18 Beta](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#client-binaries) sürümü ile kubectl

Aks-Preview uzantısını veya sonrasını yüklemek/güncelleştirmek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

```azurecli
az extension update --name aks-preview
az extension list
```

Kubectl 'yi yüklemek için aşağıdakileri kullanın:

```azurecli
sudo az aks install-cli
kubectl version --client
```

Diğer işletim sistemleri için [Bu yönergeleri](https://kubernetes.io/docs/tasks/tools/install-kubectl/) kullanın.

> [!CAUTION]
> Bir aboneliği bir abonelik üzerinde kaydettikten sonra, şu anda bu özelliğin kaydını silemezsiniz. Bazı Önizleme özelliklerini etkinleştirdiğinizde, varsayılanlar daha sonra abonelikte oluşturulan tüm AKS kümeleri için kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Bunun yerine, Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name AAD-V2 --namespace Microsoft.ContainerService
```

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AAD-V2')].{Name:name,State:properties.state}"
```

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Azure AD özellikli bir AKS kümesi oluşturma

Artık aşağıdaki CLı komutlarını kullanarak bir AKS kümesi oluşturabilirsiniz.

İlk olarak, bir Azure Kaynak grubu oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

Ardından, bir AKS kümesi oluşturun:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad
```
Yukarıdaki komut üç düğümlü bir AKS kümesi oluşturur, ancak kümeyi oluşturan kullanıcı, varsayılan olarak bu kümeye erişimi olan bir grubun üyesi değildir. Bu kullanıcının bir Azure AD grubu oluşturması, kendilerini grubun bir üyesi olarak eklemesi ve ardından kümeyi aşağıda gösterildiği gibi güncelleştirmesi gerekir. [Buradaki](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) yönergeleri izleyin

Bir grup oluşturup kendinizi (ve diğer kullanıcılar) üye olarak ekledikten sonra, aşağıdaki komutu kullanarak kümeyi Azure AD grubuyla güncelleştirebilirsiniz

```azurecli-interactive
az aks update -g MyResourceGroup -n MyManagedCluster [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```
Alternatif olarak, ilk olarak bir grup oluşturup üye eklerseniz, aşağıdaki komutu kullanarak oluşturma sırasında Azure AD grubunu etkinleştirebilirsiniz.

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```

Azure AD v2 kümesinin başarılı bir şekilde oluşturulması, yanıt gövdesinde aşağıdaki bölüme sahiptir
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

## <a name="access-an-azure-ad-enabled-cluster"></a>Azure AD özellikli bir kümeye erişme
Kümeye erişmek için yönetici kimlik bilgilerini almak için:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster --admin
```
Şimdi küme içindeki düğümleri görüntülemek için kubectl Get Nodes komutunu kullanın:

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```

Kümeye erişmek için Kullanıcı kimlik bilgilerini almak için:
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Oturum açmak için yönergeleri izleyin.

Şunu alıyorsunuz: **sunucuda oturum açmış olmanız gerekir (yetkisiz)**

Kullanıcı kümeye erişimi olan bir grubun parçası olmadığından, yukarıdaki Kullanıcı bir hata alır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD rol tabanlı Access Control][azure-ad-rbac]hakkında bilgi edinin.

<!-- LINKS - Internal -->
[azure-ad-rbac]: azure-ad-rbac.md
