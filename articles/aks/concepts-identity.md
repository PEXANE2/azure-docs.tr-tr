---
title: Kavramlar - Azure Kubernetes Hizmetlerinde (AKS) erişim ve kimlik
description: Azure Active Directory entegrasyonu, Kubernetes rol tabanlı erişim denetimi (RBAC) ve roller ve ciltler dahil olmak üzere Azure Kubernetes Hizmeti'nde (AKS) erişim ve kimlik hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 02/28/2019
ms.openlocfilehash: e4945535417f7d8d33308121267ba97e1f835e13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259610"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) için erişim ve kimlik seçenekleri

Kubernetes kümelerini doğrulamak ve güvenli hale getirmek için farklı yollar vardır. Rol tabanlı erişim denetimlerini (RBAC) kullanarak, kullanıcılara veya gruplara yalnızca gereksinim duydukları kaynaklara erişim izni verebilirsiniz. Azure Kubernetes Hizmeti (AKS) ile Azure Etkin Dizini'ni kullanarak güvenlik ve izin yapısını daha da geliştirebilirsiniz. Bu yaklaşımlar, uygulama iş yüklerinizi ve müşteri verilerinizi güvence altına almanıza yardımcı olur.

Bu makalede, AKS'de izinleri doğrulamanıza ve atamanıza yardımcı olan temel kavramlar tanıtışlar:

- [Kubernetes hizmet hesapları](#kubernetes-service-accounts)
- [Azure Active Directory tümleştirmesi](#azure-active-directory-integration)
- [Rol tabanlı erişim denetimleri (RBAC)](#role-based-access-controls-rbac)
- [Roller ve Kümerolleri](#roles-and-clusterroles)
- [RoleBindings ve ClusterRoleBindings](#rolebindings-and-clusterrolebindings)

## <a name="kubernetes-service-accounts"></a>Kubernetes hizmet hesapları

Kubernetes birincil kullanıcı türlerinden biri bir *hizmet hesabıdır.* Kubernetes API'sinde bir hizmet hesabı bulunur ve bu hesap tarafından yönetilir. Hizmet hesaplarının kimlik bilgileri, API Sunucusu ile iletişim kurmak için yetkili bölmeler tarafından kullanılmasına olanak tanıyan Kubernetes sırları olarak depolanır. Çoğu API isteği, bir hizmet hesabı veya normal bir kullanıcı hesabı için kimlik doğrulama belirteci sağlar.

Normal kullanıcı hesapları, yalnızca hizmetler ve işlemler için değil, insan yöneticiler veya geliştiriciler için daha geleneksel erişim sağlar. Kubernetes'in kendisi, normal kullanıcı hesaplarının ve parolaların depolandığı bir kimlik yönetimi çözümü sağlamaz. Bunun yerine, dış kimlik çözümleri Kubernetes entegre edilebilir. AKS kümeleri için bu tümleşik kimlik çözümü Azure Active Directory'dir.

Kubernetes'teki kimlik seçenekleri hakkında daha fazla bilgi için [Kubernetes kimlik doğrulama bilgisini][kubernetes-authentication]görün.

## <a name="azure-active-directory-integration"></a>Azure Active Directory tümleştirmesi

AKS kümelerinin güvenliği Azure Active Directory (AD) entegrasyonu ile artırılabilir. Onlarca yıllık kurumsal kimlik yönetimi üzerine inşa edilen Azure AD, temel dizin hizmetleri, uygulama erişim yönetimi ve kimlik korumayı birleştiren çok kiracılı, bulut tabanlı bir dizin ve kimlik yönetimi hizmetidir. Azure AD ile, hesap yönetimi ve güvenliği için tek bir kaynak sağlamak için şirket içi kimlikleri AKS kümelerine entegre edebilirsiniz.

![AKS kümeleriyle Azure Active Directory tümleştirmesi](media/concepts-identity/aad-integration.png)

Azure AD ile tümleşik AKS kümeleri ile kullanıcılara veya gruplara bir ad alanı içinde veya küme genelinde Kubernetes kaynaklarına erişim izni verebilirsiniz. Yapılandırma `kubectl` bağlamı elde etmek için, bir kullanıcı [az aks get-credentials][az-aks-get-credentials] komutunu çalıştırabilirsiniz. Bir kullanıcı AKS kümesiyle etkileşimde `kubectl`kaldığında, Azure AD kimlik bilgileriyle oturum açmaları istenir. Bu yaklaşım, kullanıcı hesabı yönetimi ve parola kimlik bilgileri için tek bir kaynak sağlar. Kullanıcı yalnızca küme yöneticisi tarafından tanımlanan kaynaklara erişebilir.

AKS kümelerinde Azure AD kimlik doğrulaması, OAuth 2.0 protokolünün üzerine oluşturulmuş bir kimlik katmanı olan OpenID Connect'i kullanır. OAuth 2.0, korumalı kaynaklara erişmek için erişim belirteçlerini elde etmek ve kullanmak için mekanizmalar tanımlar ve OpenID Connect kimlik doğrulamayı OAuth 2.0 yetkilendirme işleminin bir uzantısı olarak uygular. OpenID Connect hakkında daha fazla bilgi için [Open ID Connect belgelerine][openid-connect]bakın. OpenID Connect aracılığıyla Azure AD'den elde edilen kimlik doğrulama belirteçlerini doğrulamak için AKS kümeleri Kubernetes Webhook Belirteç Kimlik Doğrulaması'nı kullanır. Daha fazla bilgi için [Webhook Belirteç Kimlik Doğrulama belgelerine][webhook-token-docs]bakın.

## <a name="role-based-access-controls-rbac"></a>Rol tabanlı erişim denetimleri (RBAC)

Kubernetes, kullanıcıların gerçekleştirebileceği eylemlerin parçalı filtrelemasını sağlamak için rol tabanlı erişim denetimleri (RBAC) kullanır. Bu denetim mekanizması, kaynakları oluşturma veya değiştirme veya uygulama iş yüklerini çalıştıran günlükleri görüntüleme gibi şeyler yapmak için kullanıcı veya kullanıcı grupları atamanıza olanak tanır. Bu izinler tek bir ad alanına kapsamlandırılabilir veya tüm AKS kümesinde verilebilir. Kubernetes RBAC ile, izinleri tanımlamak için *roller* oluşturur sunuz ve bu rolleri rol bağlamalı kullanıcılara *atarsınız.*

Daha fazla bilgi için [RBAC yetkilendirmesi kullanma'ya][kubernetes-rbac]bakın.

### <a name="azure-role-based-access-controls-rbac"></a>Azure rol tabanlı erişim denetimleri (RBAC)
Kaynaklara erişimi denetlemek için ek bir mekanizma, Azure rol tabanlı erişim denetimleridir (RBAC). Kubernetes RBAC, AKS kümenizdeki kaynaklar üzerinde çalışmak üzere tasarlanmıştır ve Azure RBAC'ı Azure aboneliğinizdeki kaynaklar üzerinde çalışmak üzere tasarlanmıştır. Azure RBAC *ile,* uygulanacak izinleri özetleyen bir rol tanımı oluşturursunuz. Daha sonra bir kullanıcı veya grup, tek bir kaynak, kaynak grubu veya abonelik genelinde olabilecek belirli bir *kapsam*için bu rol tanımına atanır.

Daha fazla bilgi için Azure [RBAC nedir?][azure-rbac]

## <a name="roles-and-clusterroles"></a>Roller ve Kümerolleri

Kubernetes RBAC'lı kullanıcılara izin atamadan önce, bu izinleri önce *Rol*olarak tanımlarsınız. Kubernetes rolleri *izin* verir. *İnkar* izni kavramı yoktur.

Roller, ad alanı içinde izin vermek için kullanılır. Tüm kümede izin vermeniz veya belirli bir ad alanı dışında kaynakları kümelemeniz gerekiyorsa, bunun yerine *ClusterRoles'u*kullanabilirsiniz.

ClusterRole, kaynaklara izin vermek için aynı şekilde çalışır, ancak belirli bir ad alanı değil, tüm kümedeki kaynaklara uygulanabilir.

## <a name="rolebindings-and-clusterrolebindings"></a>RoleBindings ve ClusterRoleBindings

Roller kaynaklara izin vermek için tanımlandıktan sonra, bu Kubernetes RBAC izinlerini *rolebinding*ile atarsınız. AKS kümeniz Azure Etkin Dizini ile tümleşirse, bağlamalar bu Azure AD kullanıcılarına küme içinde eylem gerçekleştirme izni nin verildiği şekilde verilir.

Rol bağlamaları, belirli bir ad alanı için rol atamak için kullanılır. Bu yaklaşım, kullanıcıların yalnızca atanan ad alanında uygulama kaynaklarına erişebilmelerini sağlayan tek bir AKS kümesini mantıksal olarak ayırmanızı sağlar. Rolleri tüm kümeye bağlamanız veya belirli bir ad alanı nın dışındaki kaynakları kümelemeniz gerekiyorsa, bunun yerine *ClusterRoleBindings'ı*kullanabilirsiniz.

ClusterRoleBinding rolleri kullanıcılara bağlamak için aynı şekilde çalışır, ancak belirli bir ad alanı değil, tüm kümedeki kaynaklara uygulanabilir. Bu yaklaşım, yöneticilere veya destek mühendislerine AKS kümesindeki tüm kaynaklara erişim izni vermenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD ve Kubernetes RBAC ile başlamak için Azure [Etkin Dizini AKS ile Tümleştir'e][aks-aad]bakın.

İlişkili en iyi uygulamalar [için AKS'de kimlik doğrulama ve yetkilendirme için en iyi uygulamalara][operator-best-practices-identity]bakın.

Çekirdek Kubernetes ve AKS kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümeleri ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeği][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v2-protocols-oidc.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: azure-ad-integration-cli.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md
