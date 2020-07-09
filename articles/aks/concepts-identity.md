---
title: Kavramlar-Azure Kubernetes Hizmetleri 'nde (AKS) erişim ve kimlik
description: Azure Active Directory tümleştirme, Kubernetes rol tabanlı erişim denetimi (RBAC) ve roller ve bağlamalar dahil olmak üzere Azure Kubernetes Service (AKS) içindeki erişim ve kimlik hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 07/07/2020
author: palma21
ms.author: jpalma
ms.openlocfilehash: 55fd27d473bd47fd3321bdb2e730e4ef2d35352f
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86106195"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) için erişim ve kimlik seçenekleri

Kimlik doğrulamak, erişimi denetlemek/yetkilendirmek ve Kubernetes kümelerinin güvenliğini sağlamak için farklı yollar vardır. Kubernetes rol tabanlı erişim denetimlerini (RBAC) kullanarak kullanıcılara, gruplara ve hizmet hesaplarına yalnızca ihtiyaç duydukları kaynaklara erişim izni verebilirsiniz. Azure Kubernetes hizmeti (AKS) ile Azure Active Directory ve Azure RBAC kullanarak güvenlik ve izin yapısını daha da geliştirebilirsiniz. Bu yaklaşımlar, küme erişiminizi güvenli hale getirmenize ve yalnızca geliştiricilere ve işleçlere gereken en düşük izinleri sağlamanıza yardımcı olur.

Bu makalede, AKS 'de izinleri kimlik doğrulamasından ve atamaya yardımcı olan temel kavramlar tanıtılmaktadır:

- [Kubernetes rol tabanlı erişim denetimleri (RBAC)](#kubernetes-role-based-access-controls-rbac)
  - [Roller ve Kümerolleri](#roles-and-clusterroles)
  - [RoleBindings ve ClusterRoleBindings](#rolebindings-and-clusterrolebindings) 
  - [Kubernetes hizmet hesapları](#kubernetes-service-accounts)
- [Azure Active Directory tümleştirmesi](#azure-active-directory-integration)
- [Azure RBAC](#azure-role-based-access-controls-rbac)
  - [AKS kaynağına erişimi yetkilendirmek için Azure RBAC](#azure-rbac-to-authorize-access-to-the-aks-resource)
  - [Kubernetes yetkilendirmesi için Azure RBAC (Önizleme)](#azure-rbac-for-kubernetes-authorization-preview)


## <a name="kubernetes-role-based-access-controls-rbac"></a>Kubernetes rol tabanlı erişim denetimleri (RBAC)

Kullanıcıların gerçekleştirebileceği eylemlerin parçalı filtrelemesini sağlamak için Kubernetes rol tabanlı erişim denetimleri (RBAC) kullanır. Bu denetim mekanizması, kullanıcıları veya Kullanıcı gruplarını atamanıza izin verir, kaynak oluşturma veya değiştirme gibi işlemleri yapma veya çalışan uygulama iş yüklerinden günlükleri görüntüleme izni verir. Bu izinler tek bir ad alanı kapsamında olabilir veya tüm AKS kümesi genelinde verilebilir. Kubernetes RBAC ile, izinleri tanımlamak için *Roller* oluşturun ve ardından bu rolleri *rol bağlamalarıyla*kullanıcılara atayın.

Daha fazla bilgi için bkz. [RBAC yetkilendirmesi kullanma][kubernetes-rbac].


### <a name="roles-and-clusterroles"></a>Roller ve Kümerolleri

Kubernetes RBAC ile kullanıcılara izin atamadan önce bu izinleri bir *rol*olarak tanımlarsınız. Kubernetes rolleri izinleri *verir* . *Reddetme* izni kavramı yoktur.

Roller bir ad alanı içinde izinler vermek için kullanılır. Tüm küme genelinde izin vermeniz veya belirli bir ad alanı dışında küme kaynakları sağlamanız gerekiyorsa, bunun yerine *Clusterroles*kullanabilirsiniz.

Kümerolü, kaynaklara izin vermek için aynı şekilde çalışarak, ancak belirli bir ad alanı değil tüm kümedeki kaynaklara uygulanabilir.

### <a name="rolebindings-and-clusterrolebindings"></a>RoleBindings ve ClusterRoleBindings

Roller, kaynaklara izinler vermek üzere tanımlandıktan sonra, bu Kubernetes RBAC izinlerini bir *Rolebinding*ile atarsınız. AKS kümeniz [Azure Active Directory ile tümleşiyorsa](#azure-active-directory-integration)bağlamalar, bu Azure AD kullanıcılarına küme içinde eylem gerçekleştirme izinleri verme iznleridir, bkz. [rol tabanlı erişim denetimi ve Azure Active Directory kimliklerini kullanarak küme kaynaklarına erişimi denetleme](azure-ad-rbac.md).

Rol bağlamaları, belirli bir ad alanı için roller atamak üzere kullanılır. Bu yaklaşım, tek bir AKS kümesini mantıksal olarak ayırt etmenizi sağlar, böylece kullanıcılar yalnızca atanan ad alanındaki uygulama kaynaklarına erişebilir. Rolleri tüm küme genelinde veya belirli bir ad alanı dışındaki küme kaynaklarına bağlamanız gerekiyorsa, *Clusterrolebindings*kullanabilirsiniz.

ClusterRoleBinding, rolleri kullanıcılara bağlamanın aynı şekilde çalışacaktır, ancak belirli bir ad alanı değil tüm küme genelinde kaynaklara uygulanabilir. Bu yaklaşım, yöneticilerin veya Destek mühendislerinin AKS kümesindeki tüm kaynaklara erişmesine izin vermenizi sağlar.


> [!NOTE]
> Microsoft/AKS tarafından gerçekleştirilen herhangi bir küme eylemi, yerleşik bir Kubernetes rolü `aks-service` ve yerleşik rol bağlama altında Kullanıcı onayı ile yapılır `aks-service-rolebinding` . Bu rol, AKS 'in küme sorunlarını giderme ve tanılama, ancak izinleri değiştiremeyeceği ya da roller ya da rol bağlamaları ya da diğer yüksek ayrıcalıklı eylemler oluşturmalarına olanak sağlar. Rol erişimi yalnızca, tam zamanında (JıT) erişime sahip etkin destek biletleri altında etkinleştirilir. [Aks destek ilkeleri](support-policies.md)hakkında daha fazla bilgi edinin.


### <a name="kubernetes-service-accounts"></a>Kubernetes hizmet hesapları

Kubernetes 'deki birincil kullanıcı türlerinden biri bir *hizmet hesabıdır*. ' De bir hizmet hesabı bulunur ve, Kubernetes API 'SI tarafından yönetilir. Hizmet hesapları için kimlik bilgileri Kubernetes gizli dizileri olarak depolanır, bu da API sunucusuyla iletişim kurmak için yetkili Pod tarafından kullanılmasına olanak tanır. Çoğu API isteği, bir hizmet hesabı veya normal bir kullanıcı hesabı için bir kimlik doğrulama belirteci sağlar.

Normal Kullanıcı hesapları, yalnızca hizmetler ve süreçler için değil insan yöneticileri veya geliştiricileri için daha geleneksel erişime izin verir. Kubernetes, normal kullanıcı hesaplarının ve parolaların depolandığı bir kimlik yönetimi çözümü sağlamıyor. Bunun yerine, dış kimlik çözümleri Kubernetes ile tümleştirilebilir. AKS kümelerinde, bu tümleşik kimlik çözümü Azure Active Directory.

Kubernetes 'in kimlik seçenekleri hakkında daha fazla bilgi için bkz. [Kubernetes kimlik doğrulaması][kubernetes-authentication].

## <a name="azure-active-directory-integration"></a>Azure Active Directory tümleştirmesi

AKS kümelerinin güvenliği Azure Active Directory (AD) Tümleştirmesi ile geliştirilebilir. Kurumsal kimlik yönetimi 'nde geliştirilen Azure AD, temel dizin hizmetlerini, uygulama erişim yönetimini ve kimlik korumasını birleştiren çok kiracılı, bulut tabanlı bir dizin ve kimlik yönetimi hizmetidir. Azure AD ile, hesap yönetimi ve güvenlik için tek bir kaynak sağlamak üzere şirket içi kimlikleri AKS kümeleriyle tümleştirebilirsiniz.

![AKS kümeleriyle Azure Active Directory tümleştirme](media/concepts-identity/aad-integration.png)

Azure AD ile tümleşik AKS kümeleri sayesinde, kullanıcılara veya gruplara bir ad alanı veya küme genelinde Kubernetes kaynaklarına erişim izni verebilirsiniz. Bir yapılandırma bağlamı elde etmek için `kubectl` , Kullanıcı [az aks Get-Credentials][az-aks-get-credentials] komutunu çalıştırabilir. Bir Kullanıcı daha sonra AKS kümesiyle etkileşime geçtiğinde `kubectl` Azure AD kimlik bilgileriyle oturum açması istenir. Bu yaklaşım Kullanıcı hesabı yönetimi ve parola kimlik bilgileri için tek bir kaynak sağlar. Kullanıcı yalnızca küme yöneticisi tarafından tanımlanan kaynaklara erişebilir.

Azure AD kimlik doğrulaması, OpenID Connect ile AKS kümelerine sağlanır. OpenID Connect, OAuth 2,0 protokolünün üstünde oluşturulmuş bir kimlik katmanıdır. OpenID Connect hakkında daha fazla bilgi için [açık KIMLIK bağlantısı belgeleri] [Açık kimlik-Connect] bölümüne bakın. Kubernetes kümesinin içinden, kimlik doğrulama belirteçlerini doğrulamak için [Web kancası belirteci kimlik doğrulaması][webhook-token-docs] kullanılır. Web kancası belirteci kimlik doğrulaması, AKS kümesinin bir parçası olarak yapılandırılır ve yönetilir.

Kubernetes kümesinin içinden, kimlik doğrulama belirteçlerini doğrulamak için Web kancası belirteci kimlik doğrulaması kullanılır. Web kancası belirteci kimlik doğrulaması, AKS kümesinin bir parçası olarak yapılandırılır ve yönetilir.

### <a name="webhook-and-api-server"></a>Web kancası ve API sunucusu

![Web kancası ve API sunucusu kimlik doğrulama akışı](media/concepts-identity/auth-flow.png)

Yukarıdaki grafikte gösterildiği gibi, API sunucusu AKS Web kancası sunucusunu çağırır ve aşağıdaki adımları gerçekleştirir:

1. Azure AD istemci uygulaması, [OAuth 2,0 cihaz yetkilendirme verme akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)ile kullanıcılar oturum açmak için kubectl tarafından kullanılır.
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
 
**AKS ['ı AAD ile](managed-aad.md)nasıl tümleştirileceğini öğrenin.**

## <a name="azure-role-based-access-controls-rbac"></a>Azure rol tabanlı erişim denetimleri (RBAC)

Azure RBAC, Azure kaynakları üzerinde ayrıntılı erişim yönetimi sağlayan [Azure Resource Manager](../azure-resource-manager/management/overview.md) yerleşik bir yetkilendirme sistemidir.

 Azure RBAC, Kubernetes RBAC, AKS kümenizdeki Kubernetes kaynakları üzerinde çalışmak üzere tasarlanırken Azure aboneliğinizdeki kaynaklar üzerinde çalışmak üzere tasarlanmıştır. 

Azure RBAC ile uygulanacak izinleri özetleyen bir *rol tanımı* oluşturursunuz. Daha sonra bir kullanıcı veya Grup, belirli bir *kapsam*için bir *rol ataması* aracılığıyla bu rol tanımına atanır. Bu, tek bir kaynak, bir kaynak grubu veya abonelik genelinde olabilir.

Daha fazla bilgi için bkz. [Azure RBAC nedir?][azure-rbac]

Bir AKS kümesini tam olarak çalıştırmak için iki erişim düzeyi gereklidir: 
1. [Azure aboneliğinizdeki AKS kaynağına erişin](#azure-rbac-to-authorize-access-to-the-aks-resource). Bu işlem, AKS API 'Lerini kullanarak kümenizi ölçeklendirmeyi veya yükseltmeyi denetlemenizi sağlar ve kubeconfig 'nizi çekin.
2. Kubernetes API 'sine erişim. Bu erişim, [KUBERNETES RBAC](#kubernetes-role-based-access-controls-rbac) (Geleneksel) tarafından denetlenir veya [Azure RBAC ile Kubernetes YETKILENDIRMESI için aks ile tümleştirilir](#azure-rbac-for-kubernetes-authorization-preview)

### <a name="azure-rbac-to-authorize-access-to-the-aks-resource"></a>AKS kaynağına erişimi yetkilendirmek için Azure RBAC

Azure RBAC ile kullanıcılarınıza (veya kimliklere) bir veya daha fazla abonelik genelinde AKS kaynaklarına ayrıntılı erişim sağlayabilirsiniz. Örneğin, [Azure Kubernetes hizmet katılımcısı rolüne](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-contributor-role) sahip olabilirsiniz ve kümenizi ölçeklendirme ve yükseltme gibi eylemler yapmanıza izin verir. Başka bir Kullanıcı [Azure Kubernetes hizmet kümesi Yöneticisi rolüne](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role) sahip olsa da, yalnızca admin kubeconfig 'i çekmek için izin verir.

Alternatif olarak, bir yandan yukarıdaki izinlerle ilgili genel [katkıda](../role-based-access-control/built-in-roles.md#contributor) bulunan rolünü ve izinlerin kendisini yönetme dışında aks kaynağında olabilecek her eylemi kapsayacak şekilde kullanıcıya izin verebilirsiniz.

[Burada](control-kubeconfig-access.md)Kubernetes API 'sine erişim sağlayan kubeconfig dosyasına erişimin güvenliğini sağlamak IÇIN Azure RBAC kullanma hakkında daha fazla bilgi edinebilirsiniz.

### <a name="azure-rbac-for-kubernetes-authorization-preview"></a>Kubernetes yetkilendirmesi için Azure RBAC (Önizleme)

Azure RBAC tümleştirmesiyle, AKS, Azure RBAC rol tanımı ve rol atamalarını kullanarak Azure AD ile tümleşik K8s küme kaynaklarının izinlerini ve atamalarını yönetmenizi sağlamak için Kubernetes yetkilendirme Web kancası sunucusunu kullanacaktır.

![Kubernetes yetkilendirme akışı için Azure RBAC](media/concepts-identity/azure-rbac-k8s-authz-flow.png)

Yukarıdaki diyagramda gösterildiği gibi, Azure RBAC tümleştirmesi kullanılırken, Kubernetes API 'sine yönelik tüm istekler [Azure Active Integration bölümünde](#azure-active-directory-integration)açıklanacak şekilde aynı kimlik doğrulama akışını izler. 

Bununla birlikte, kimlik doğrulama için yalnızca Kubernetes RBAC 'e güvenmek yerine, isteği AAD 'de olduğu sürece istek aslında Azure tarafından yetkilendirilir. Kimlik AAD 'de yoksa, örneğin bir Kubernetes hizmet hesabı için Azure RBAC, bu, normal Kubernetes RBAC olacaktır.

Bu senaryoda, kullanıcılara dört yerleşik rolden birini verebilir veya Kubernetes rolleriyle yaptığınız gibi özel roller oluşturabilirsiniz, ancak bu örnekte Azure RBAC mekanizmaları ve API 'Leri kullanılıyor. 

Bu özellik, örneğin, kullanıcılara yalnızca abonelikler arasında AKS kaynağı izinleri verme ancak bunları ayarlama ve onları, Kubernetes API 'sine erişimi denetleyen bu kümelerin her birinde sahip olabilecekleri rol ve izinleri vermenizi sağlayacak şekilde izin verecektir. Örneğin, `Azure Kubernetes Service RBAC Viewer` abonelik kapsamında role izin verebilir ve alıcı tüm Kubernetes nesnelerini listeleyebilir, ancak değiştiremez.


#### <a name="built-in-roles"></a>Yerleşik roller

AKS, aşağıdaki dört yerleşik rolü sağlar. Bunlar, [Kubernetes yerleşik rollerine](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles) benzerdir, ancak crds 'yi destekleme gibi birkaç farklılık vardır. Her bir yerleşik rol tarafından izin verilen eylemlerin tam listesi için lütfen [buraya](../role-based-access-control/built-in-roles.md)bakın.

| Rol                                | Description  |
|-------------------------------------|--------------|
| Azure Kubernetes hizmeti RBAC Görüntüleyicisi  | Bir ad alanındaki birçok nesneyi görmek için salt okuma erişimine izin verir. Rollerin veya rol bağlamalarının görüntülenmesine izin vermez. `Secrets`Gizli dizi içeriğini okumak, ad alanındaki `ServiceAccount` kimlik bilgilerine erişim sağladığından, `ServiceAccount` ad alanında (ayrıcalık yükseltme) bir biçimde API erişimine izin veren bu rol görüntülemeye izin vermez  |
| Azure Kubernetes hizmeti RBAC yazıcı | Bir ad alanındaki nesnelerin çoğuna okuma/yazma erişimi sağlar. Bu rol, rolleri veya rol bağlamalarını görüntülemeye veya değiştirmeye izin vermez. Bununla birlikte, bu rol `Secrets` ad alanındaki herhangi bir ServiceAccount olarak, bu rol, ad alanındaki herhangi bir ServiceAccount 'ın API erişim düzeylerini kazanmak için kullanılabilir. |
| Azure Kubernetes hizmeti RBAC Yöneticisi  | Yönetici erişimine izin verir, bir ad alanı içinde verilmek üzere tasarlanmıştır. Ad alanı içinde roller ve rol bağlamaları oluşturma özelliği de dahil olmak üzere bir ad alanındaki (veya küme kapsamındaki) kaynakların çoğuna okuma/yazma erişimi sağlar. Bu rol, kaynak kotasına veya ad alanının kendine yazma erişimine izin vermez. |
| Azure Kubernetes hizmeti RBAC kümesi Yöneticisi  | Süper Kullanıcı erişiminin herhangi bir kaynak üzerinde herhangi bir işlem gerçekleştirmesine izin verir. Kümedeki her kaynak ve tüm ad alanlarında tam denetim sağlar. |

**Bkz. Kubernetes yetkilendirmesi için Azure RBAC 'ten yararlanma [here](manage-azure-rbac.md).**

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD ve Kubernetes RBAC ile çalışmaya başlamak için bkz. [Azure Active Directory AKS Ile tümleştirme][aks-aad].
- İlişkili en iyi uygulamalar için bkz. [AKS 'de kimlik doğrulama ve yetkilendirme Için en iyi uygulamalar][operator-best-practices-identity].
- Kubernetes yetkilendirmesi için Azure RBAC ile çalışmaya başlamak için bkz. Azure [Kubernetes Service (AKS) kümesi içindeki erişimi yetkilendirmek Için Azure RBAC kullanma](manage-azure-rbac.md).
- Kubeconfig dosyanızı güvenli bir şekilde kullanmaya başlamak için bkz. [küme yapılandırma dosyasına erişimi sınırlandırma](control-kubeconfig-access.md)

Temel Kubernetes ve AKS kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes/AKS kümeleri ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes/AKS güvenliği][aks-concepts-security]
- [Kubernetes/AKS sanal ağları][aks-concepts-network]
- [Kubernetes/AKS depolaması][aks-concepts-storage]
- [Kubernetes/AKS ölçeği][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v2-protocols-oidc.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: managed-aad.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md
