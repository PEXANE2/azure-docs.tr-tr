---
title: Azure Kubernetes hizmetinde Azure AD kullanma
description: Azure Kubernetes hizmetinde (AKS) Azure AD 'yi nasıl kullanacağınızı öğrenin
services: container-service
manager: gwallace
author: mlearned
ms.topic: article
ms.date: 06/25/2020
ms.author: mlearned
ms.openlocfilehash: 280637be417d904de6dbb7ae2e2647026da6c838
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85374551"
---
# <a name="integrate-aks-managed-azure-ad-preview"></a>AKS tarafından yönetilen Azure AD 'yi tümleştirin (Önizleme)

> [!Note]
> Azure Active Directory (Azure AD) Tümleştirmesi olan mevcut AKS (Azure Kubernetes hizmeti) kümeleri, yeni AKS tarafından yönetilen Azure AD deneyiminden etkilenmez.

AKS tarafından yönetilen Azure AD ile Azure AD tümleştirmesi, kullanıcıların daha önce bir istemci uygulaması, bir sunucu uygulaması oluşturmak ve dizin okuma izinleri vermek için Azure AD kiracısının gerekli olduğu Azure AD tümleştirme deneyimini basitleştirmek üzere tasarlanmıştır. Yeni sürümde, AKS kaynak sağlayıcısı, istemci ve sunucu uygulamalarını sizin için yönetir.

## <a name="limitations"></a>Sınırlamalar

* Şu anda mevcut bir AKS Azure AD ile tümleşik kümesini yeni AKS tarafından yönetilen Azure AD deneyimine yükseltemezsiniz.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme esasına göre sunulmaktadır. Önizlemeler "olduğu gibi" ve "kullanılabildiği gibi" verilmiştir ve hizmet düzeyi anlaşmalarından ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteği tarafından kısmen bir en iyi performans kapsamında ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Daha fazla bilgi için aşağıdaki destek makalelerine bakın:
>
> - [AKS destek Ilkeleri](support-policies.md)
> - [Azure desteği SSS](faq.md)

## <a name="before-you-begin"></a>Başlamadan önce

* Azure portal giderek Azure hesap kiracı KIMLIĞINIZI bulun ve Azure Active Directory > Özellikler > dizin KIMLIĞI ' ni seçin.

> [!Important]
> Kubectl 'yi minimum 1,18 sürümü ile kullanmanız gerekir

Aşağıdaki kaynakların yüklü olması gerekir:

- Azure CLı, sürüm 2.5.1 veya üzeri
- Aks-Preview 0.4.38 uzantısı
- Minimum [1,18](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1180) sürümü ile kubectl

Aks-Preview uzantısını veya sonrasını yüklemek/güncelleştirmek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli
az extension add --name aks-preview
az extension list
```

```azurecli
az extension update --name aks-preview
az extension list
```

Kubectl 'yi yüklemek için aşağıdaki komutları kullanın:

```azurecli
sudo az aks install-cli
kubectl version --client
```

Diğer işletim sistemleri için [Bu yönergeleri](https://kubernetes.io/docs/tasks/tools/install-kubectl/) kullanın.

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
## <a name="azure-ad-authentication-overview"></a>Azure AD kimlik doğrulamasına genel bakış

Küme yöneticileri, Kubernetes rol tabanlı erişim denetimini (RBAC) bir kullanıcının kimliğine veya dizin grubu üyeliğine göre yapılandırabilir. Azure AD kimlik doğrulaması, OpenID Connect ile AKS kümelerine sağlanır. OpenID Connect, OAuth 2,0 protokolünün üstünde oluşturulmuş bir kimlik katmanıdır. OpenID Connect hakkında daha fazla bilgi için bkz. [Açık kimlik bağlantısı belgeleri][open-id-connect].

Kubernetes kümesinin içinden, kimlik doğrulama belirteçlerini doğrulamak için Web kancası belirteci kimlik doğrulaması kullanılır. Web kancası belirteci kimlik doğrulaması, AKS kümesinin bir parçası olarak yapılandırılır ve yönetilir.

## <a name="webhook-and-api-server"></a>Web kancası ve API sunucusu

:::image type="content" source="media/aad-integration/auth-flow.png" alt-text="Web kancası ve API sunucusu kimlik doğrulama akışı":::

Yukarıdaki grafikte gösterildiği gibi, API sunucusu AKS Web kancası sunucusunu çağırır ve aşağıdaki adımları gerçekleştirir:

1. Azure AD istemci uygulaması, [OAuth 2,0 cihaz yetkilendirme verme akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)ile kullanıcıların oturum açması için kubectl tarafından kullanılır.
2. Azure AD, bir access_token, id_token ve bir refresh_token sağlar.
3. Kullanıcı, kubeconfig 'ten bir access_token kubectl 'ye bir istek yapar.
4. Kubectl, access_token Apıver 'e gönderir.
5. API sunucusu, doğrulama gerçekleştirmek için auth Web kancası sunucusuyla yapılandırılır.
6. Kimlik doğrulama Web kancası sunucusu, Azure AD ortak imzalama anahtarını denetleyerek JSON Web Token imzasının geçerli olduğunu onaylar.
7. Sunucu uygulaması, oturum açmış kullanıcının grup üyeliklerini MS Graph API sorgulamak için Kullanıcı tarafından sağlanmış kimlik bilgilerini kullanır.
8. Erişim belirtecinin Kullanıcı asıl adı (UPN) talebi ve nesne KIMLIĞINE göre kullanıcının grup üyeliği gibi Kullanıcı bilgileri ile Apıver 'e bir yanıt gönderilir.
9. API, Kubernetes role/RoleBinding ' i temel alan bir yetkilendirme kararı uygular.
10. Yetkilendirildikten sonra, API sunucusu kubectl 'ye bir yanıt döndürür.
11. Kubectl kullanıcıya geri bildirim sağlar.


## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Azure AD özellikli bir AKS kümesi oluşturma

Aşağıdaki CLı komutlarını kullanarak bir AKS kümesi oluşturun.

Azure Kaynak grubu oluşturun:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

Mevcut bir Azure AD grubunu kullanabilir veya yeni bir tane oluşturabilirsiniz. Azure AD grubunuz için nesne KIMLIĞI gereklidir.

```azurecli-interactive
# List existing groups in the directory
az ad group list
```

Küme yöneticileriniz için yeni bir Azure AD grubu eklemek için aşağıdaki komutu kullanın:

```azurecli-interactive
# Create an Azure AD group
az ad group create --display-name MyDisplay --mail-nickname MyDisplay
```

AKS kümesi oluşturma ve Azure AD grubunuz için yönetim erişimini etkinleştirme

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad [--aad-admin-group-object-ids <id>] [--aad-tenant-id <id>]
```

AKS tarafından yönetilen bir Azure AD kümesinin başarılı bir şekilde oluşturulması yanıt gövdesinde aşağıdaki bölüme sahiptir
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

Aşağıdaki adımları gerçekleştirmek için [Azure Kubernetes hizmet kümesi Kullanıcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#azure-kubernetes-service-cluster-user-role) yerleşik rolüne sahip olmanız gerekir.

Kümeye erişmek için Kullanıcı kimlik bilgilerini alın:
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
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
Kümeleriniz için ek güvenlik grupları yapılandırmak üzere [rol tabanlı Access Control (RBAC)](https://review.docs.microsoft.com/azure/aks/azure-ad-rbac?branch=pr-en-us-117564) yapılandırın.

## <a name="troubleshooting-access-issues-with-azure-ad"></a>Azure AD ile erişim sorunlarını giderme

> [!Important]
> Aşağıda açıklanan adımlar, normal Azure AD grubu kimlik doğrulamasını atlama. Onları yalnızca bir acil durum içinde kullanın.

Kümenize erişimi olan geçerli bir Azure AD grubuna erişim olmadan kalıcı olarak engelleniyorsa, kümeye doğrudan erişmek için yönetici kimlik bilgilerini almaya devam edebilirsiniz.

Bu adımları gerçekleştirmek için [Azure Kubernetes hizmet kümesi Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#azure-kubernetes-service-cluster-admin-role) yerleşik rolüne erişiminizin olması gerekir.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster --admin
```

## <a name="non-interactive-login-with-kubelogin"></a>Kubelogin ile etkileşimli olmayan oturum açma

Şu anda kubectl ile kullanılamayan sürekli tümleştirme işlem hatları gibi etkileşimli olmayan bazı senaryolar vardır. [Kubelogin](https://github.com/Azure/kubelogin) ' i etkileşimli olmayan hizmet sorumlusu oturum açma bilgileriyle kümeye erişmek için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD rol tabanlı Access Control][azure-ad-rbac]hakkında bilgi edinin.
* Kubectl 'de kullanılamayan Azure kimlik doğrulaması özelliklerine erişmek için [kubelogin](https://github.com/Azure/kubelogin) kullanın.
* AKS tarafından yönetilen Azure AD etkinleştirilmiş kümeler oluşturmak için [Azure Resource Manager (ARM) şablonları][aks-arm-template] kullanın.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[aks-arm-template]: https://docs.microsoft.com/azure/templates/microsoft.containerservice/managedclusters

<!-- LINKS - Internal -->
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

