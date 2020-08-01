---
title: Azure Kubernetes hizmetinde Azure AD kullanma
description: Azure Kubernetes hizmetinde (AKS) Azure AD 'yi nasıl kullanacağınızı öğrenin
services: container-service
manager: gwallace
ms.topic: article
ms.date: 07/20/2020
ms.author: thomasge
ms.openlocfilehash: 896986775f0132ef08b17bdfefc00e5e06cf3d9f
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87448131"
---
# <a name="aks-managed-azure-active-directory-integration"></a>AKS tarafından yönetilen Azure Active Directory tümleştirme

AKS tarafından yönetilen Azure AD tümleştirmesi, kullanıcıların daha önce bir istemci uygulaması, bir sunucu uygulaması oluşturmak ve dizin okuma izinleri vermek için Azure AD kiracısının gerekli olduğu Azure AD tümleştirme deneyimini basitleştirmek üzere tasarlanmıştır. Yeni sürümde, AKS kaynak sağlayıcısı, istemci ve sunucu uygulamalarını sizin için yönetir.

## <a name="azure-ad-authentication-overview"></a>Azure AD kimlik doğrulamasına genel bakış

Küme yöneticileri, Kubernetes rol tabanlı erişim denetimini (RBAC) bir kullanıcının kimliğine veya dizin grubu üyeliğine göre yapılandırabilir. Azure AD kimlik doğrulaması, OpenID Connect ile AKS kümelerine sağlanır. OpenID Connect, OAuth 2,0 protokolünün üstünde oluşturulmuş bir kimlik katmanıdır. OpenID Connect hakkında daha fazla bilgi için bkz. [Açık kimlik bağlantısı belgeleri][open-id-connect].

[Azure Active Directory tümleştirme kavramları BELGELERINDEKI](concepts-identity.md#azure-active-directory-integration)AAD tümleştirme akışı hakkında daha fazla bilgi edinin.

## <a name="region-availability"></a>Bölge kullanılabilirliği

AKS tarafından yönetilen Azure Active Directory tümleştirme, [aks 'in desteklendiği](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service)Genel bölgelerde kullanılabilir.

* Azure Kamu Şu anda desteklenmiyor.
* Azure Çin 21Vianet Şu anda desteklenmemektedir.

## <a name="limitations"></a>Sınırlamalar 

* AKS tarafından yönetilen Azure AD tümleştirmesi devre dışı bırakılamaz
* RBAC özellikli olmayan kümeler, AKS tarafından yönetilen AAD tümleştirmesi için desteklenmez
* AKS tarafından yönetilen AAD tümleştirmesiyle ilişkili Azure AD kiracısı 'nin değiştirilmesi desteklenmiyor

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme esasına göre sunulmaktadır. Önizlemeler "olduğu gibi" ve "kullanılabildiği gibi" verilmiştir ve hizmet düzeyi anlaşmalarından ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteği tarafından kısmen bir en iyi performans kapsamında ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Daha fazla bilgi için aşağıdaki destek makalelerine bakın: 
> - [AKS destek Ilkeleri](support-policies.md) 
> - [Azure desteği SSS](faq.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure CLı sürüm 2.9.0 veya üzeri
* Minimum [1,18](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1180) sürümü ile kubectl

> [!Important]
> Kubectl 'yi minimum 1,18 sürümü ile kullanmanız gerekir

Kubectl 'yi yüklemek için aşağıdaki komutları kullanın:

```azurecli-interactive
sudo az aks install-cli
kubectl version --client
```

Diğer işletim sistemleri için [Bu yönergeleri](https://kubernetes.io/docs/tasks/tools/install-kubectl/) kullanın.

```azurecli-interactive 
az feature register --name AAD-V2 --namespace Microsoft.ContainerService    
``` 

Durumun **kayıtlı**olarak gösterilmesi birkaç dakika sürebilir. [Az Feature List](/cli/azure/feature?view=azure-cli-latest#az-feature-list) komutunu kullanarak kayıt durumunu kontrol edebilirsiniz: 

```azurecli-interactive 
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AAD-V2')].{Name:name,State:properties.state}"    
``` 

Durum kayıtlı olarak görünüyorsa, `Microsoft.ContainerService` [az Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) komutunu kullanarak kaynak sağlayıcının kaydını yenileyin:    

```azurecli-interactive 
az provider register --namespace Microsoft.ContainerService 
``` 


## <a name="before-you-begin"></a>Başlamadan önce

Kümeniz için bir Azure AD grubuna ihtiyacınız vardır. Bu grup, kümenin yönetici izinleri vermesi için yönetici grubu olarak gereklidir. Mevcut bir Azure AD grubunu kullanabilir veya yeni bir tane oluşturabilirsiniz. Azure AD grubunuzun nesne KIMLIĞINI kaydedin.

```azurecli-interactive
# List existing groups in the directory
az ad group list --filter "displayname eq '<group-name>'" -o table
```

Küme yöneticileriniz için yeni bir Azure AD grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
# Create an Azure AD group
az ad group create --display-name myAKSAdminGroup --mail-nickname myAKSAdminGroup
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Azure AD özellikli bir AKS kümesi oluşturma

Aşağıdaki CLı komutlarını kullanarak bir AKS kümesi oluşturun.

Azure Kaynak grubu oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

AKS kümesi oluşturma ve Azure AD grubunuz için yönetim erişimini etkinleştirme

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g myResourceGroup -n myManagedCluster --enable-aad --aad-admin-group-object-ids <id> [--aad-tenant-id <id>]
```

AKS tarafından yönetilen bir Azure AD kümesinin başarılı bir şekilde oluşturulması yanıt gövdesinde aşağıdaki bölüme sahiptir
```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Küme oluşturulduktan sonra erişime başlayabilirsiniz.

## <a name="access-an-azure-ad-enabled-cluster"></a>Azure AD özellikli bir kümeye erişme

Aşağıdaki adımları uygulamak için [Azure Kubernetes hizmet kümesi Kullanıcı](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role) yerleşik rolüne sahip olmanız gerekir.

Kümeye erişmek için Kullanıcı kimlik bilgilerini alın:
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name myManagedCluster
```
Oturum açmak için yönergeleri izleyin.

Kümedeki düğümleri görüntülemek için kubectl Get Nodes komutunu kullanın:

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```
Kümeleriniz için ek güvenlik grupları yapılandırmak üzere [Azure rol tabanlı erişim denetimi 'ni (Azure RBAC)](./azure-ad-rbac.md) yapılandırın.

## <a name="troubleshooting-access-issues-with-azure-ad"></a>Azure AD ile erişim sorunlarını giderme

> [!Important]
> Aşağıda açıklanan adımlar, normal Azure AD grubu kimlik doğrulamasını atlama. Onları yalnızca bir acil durum içinde kullanın.

Kümenize erişimi olan geçerli bir Azure AD grubuna erişim olmadan kalıcı olarak engelleniyorsa, doğrudan kümeye erişmek için yönetici kimlik bilgilerini almaya devam edebilirsiniz.

Bu adımları uygulamak için [Azure Kubernetes hizmet kümesi Yöneticisi](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role) yerleşik rolüne erişiminizin olması gerekir.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myManagedCluster --admin
```

## <a name="upgrading-to-aks-managed-azure-ad-integration"></a>AKS tarafından yönetilen Azure AD tümleştirmesine yükseltme

Kümeniz eski Azure AD tümleştirmesini kullanıyorsa, AKS tarafından yönetilen Azure AD tümleştirmesine yükseltebilirsiniz.

```azurecli-interactive
az aks update -g myResourceGroup -n myManagedCluster --enable-aad --aad-admin-group-object-ids <id> [--aad-tenant-id <id>]
```

AKS tarafından yönetilen bir Azure AD kümesinin başarılı bir geçişi yanıt gövdesinde aşağıdaki bölüme sahiptir

```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Kümeye erişmek istiyorsanız [buradaki][access-cluster]adımları izleyin.

## <a name="non-interactive-sign-in-with-kubelogin"></a>Kubelogin ile etkileşimli olmayan oturum açma

Şu anda kubectl ile kullanılamayan sürekli tümleştirme işlem hatları gibi etkileşimli olmayan bazı senaryolar vardır. [`kubelogin`](https://github.com/Azure/kubelogin)Etkileşimli olmayan hizmet sorumlusu oturum açma ile kümeye erişmek için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Kubernetes yetkilendirmesi Için Azure RBAC tümleştirmesi][azure-rbac-integration] hakkında bilgi edinin
* [Kubernetes RBAC Ile Azure AD tümleştirmesi][azure-ad-rbac]hakkında bilgi edinin.
* Kubectl 'de kullanılamayan Azure kimlik doğrulaması özelliklerine erişmek için [kubelogin](https://github.com/Azure/kubelogin) kullanın.
* [Aks ve Kubernetes kimlik kavramları][aks-concepts-identity]hakkında daha fazla bilgi edinin.
* AKS tarafından yönetilen Azure AD etkinleştirilmiş kümeler oluşturmak için [Azure Resource Manager (ARM) şablonları][aks-arm-template] kullanın.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[aks-arm-template]: /azure/templates/microsoft.containerservice/managedclusters

<!-- LINKS - Internal -->
[azure-rbac-integration]: manage-azure-rbac.md
[aks-concepts-identity]: concepts-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v2-protocols-oidc.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
[access-cluster]: #access-an-azure-ad-enabled-cluster
[aad-migrate]: #upgrading-to-aks-managed-azure-ad-integration
