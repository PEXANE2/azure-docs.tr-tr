---
title: Kimliği yönetmek için en iyi uygulamalar
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti 'ndeki (AKS) kümeler için kimlik doğrulama ve yetkilendirmeyi yönetme ye ilişkin küme operatörüne en iyi uygulamaları öğrenin
services: container-service
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: 0e3569be769fcf70a65cbfee62a3b80a5abdc3b5
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668307"
---
# <a name="best-practices-for-authentication-and-authorization-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) kimlik doğrulama ve yetkilendirme için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) kümeleri dağıtıp korurken, kaynaklara ve hizmetlere erişimi yönetmenin yollarını uygulamanız gerekir. Bu denetimler olmadan, hesaplar gereksinim duymadıkları kaynaklara ve hizmetlere erişebilir. Değişiklik yapmak için hangi kimlik bilgilerinin kullanıldığını izlemek de zor olabilir.

Bu en iyi uygulamalar makalesi, bir küme işlecinin AKS kümeleri için erişimi ve kimliği nasıl yönetebileceğine odaklanır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure Active Directory ile AKS küme kullanıcılarını doğrula
> * Rol tabanlı erişim denetimleri (RBAC) ile kaynaklara erişimi denetleme
> * Diğer hizmetlerle kendilerini doğrulamak için yönetilen bir kimlik kullanma

## <a name="use-azure-active-directory"></a>Azure Active Directory kullanma

**En iyi uygulama kılavuzu** - Azure AD tümleştirmesi ile AKS kümelerini dağıtın. Azure AD'yi kullanmak kimlik yönetimi bileşenini merkezileştirir. Kullanıcı hesabındaki veya grup durumundaki tüm değişiklikler AKS kümesine erişimde otomatik olarak güncelleştirilir. Bir sonraki bölümde tartışıldığı gibi, kullanıcıları veya grupları gereken en az izin miktarına çıkarmak için Roller veya ClusterRoles ve Bindings'i kullanın.

Kubernetes kümenizin geliştiricileri ve uygulama sahiplerinin farklı kaynaklara erişmeleri gerekir. Kubernetes, hangi kullanıcıların hangi kaynaklarla etkileşimde bulunabileceğini denetlemek için bir kimlik yönetimi çözümü sağlamaz. Bunun yerine, kümenizi genellikle varolan bir kimlik çözümüyle tümleştirirsiniz. Azure Active Directory (AD), kuruluşa hazır bir kimlik yönetimi çözümü sağlar ve AKS kümeleriyle tümleştirebilirsiniz.

AKS'deki Azure AD tümleşik kümeleriyle, kaynaklara erişim izinlerini tanımlayan *Roller* veya *ClusterRoles* oluşturursunuz. Daha sonra rolleri Azure AD'deki kullanıcılara veya gruplara *bağlarsınız.* Bu Kubernetes rol tabanlı erişim denetimleri (RBAC) sonraki bölümde ele alınmıştır. Azure AD'nin entegrasyonu ve kaynaklara erişimi nasıl denetlediğiniz aşağıdaki diyagramda görülebilir:

![AKS ile Azure Active Directory tümleştirmesi için küme düzeyinde kimlik doğrulaması](media/operator-best-practices-identity/cluster-level-authentication-flow.png)

1. Geliştirici Azure AD ile kimlik doğrulaması.
1. Azure AD belirteci verme bitiş noktası erişim belirteci sorunları.
1. Geliştirici, Azure AD belirteci kullanarak bir eylem gerçekleştirir, örneğin`kubectl create pod`
1. Kubernetes belirteci Azure Etkin Dizini ile doğrular ve geliştiricinin grup üyeliklerini getirir.
1. Kubernetes rol tabanlı erişim denetimi (RBAC) ve küme ilkeleri uygulanır.
1. Geliştiricinin isteği, Azure AD grup üyeliği ve Kubernetes RBAC ve ilkelerinin önceki doğrulamalarına bağlı olarak başarılı veya değil.

Azure AD kullanan bir [AKS][aks-aad]kümesi oluşturmak için bkz.

## <a name="use-role-based-access-controls-rbac"></a>Rol tabanlı erişim denetimlerini kullanma (RBAC)

**En iyi uygulama kılavuzu** - Kullanıcıların veya grupların kümedeki kaynaklara sahip olduğu izinleri tanımlamak için Kubernetes RBAC'ı kullanın. Gereken en az izin miktarını atayabilen roller ve bağlamalar oluşturun. Kullanıcı durumundaki veya grup üyeliğindeki herhangi bir değişikliğin otomatik olarak güncellenmesi ve küme kaynaklarına erişimin güncel olması için Azure AD ile tümleştirin.

Kubernetes'te, kümedeki kaynaklara erişimin parçalı denetimini sağlayabilirsiniz. İzinler küme düzeyinde veya belirli ad alanlarında tanımlanabilir. Hangi kaynakların yönetilebileceğini ve hangi izinlerle yönetilebileceğini tanımlayabilirsiniz. Bu roller daha sonra bağlayıcısı olan kullanıcılara veya gruplara uygulanır. *Roller,* *ClusterRoles*ve *Ciltler*hakkında daha fazla bilgi için [Azure Kubernetes Hizmeti (AKS) için Erişim ve kimlik seçeneklerine][aks-concepts-identity]bakın.

Örnek olarak, aşağıdaki örnek YAML bildiriminde gösterildiği gibi, *finans-app*adlı ad alanındaki kaynaklara tam erişim sağlayan bir Rol oluşturabilirsiniz:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: finance-app-full-access-role
  namespace: finance-app
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["*"]
```

Daha sonra, aşağıdaki YAML bildiriminde gösterildiği gibi, Azure AD kullanıcı *geliştiricisini 1\@contoso.com* RoleBinding'e bağlayan bir RoleBinding oluşturulur:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: finance-app-full-access-role-binding
  namespace: finance-app
subjects:
- kind: User
  name: developer1@contoso.com
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: finance-app-full-access-role
  apiGroup: rbac.authorization.k8s.io
```

*Geliştirici1\@contoso.com* AKS kümesine karşı kimlik doğrulaması yapıldığında, finans *uygulaması* ad alanındaki kaynaklara tam izinleri vardır. Bu şekilde, mantıksal olarak kaynaklara erişimi ayırır ve denetlersiniz. Kubernetes RBAC, önceki bölümde belirtildiği gibi Azure AD tümleştirmesiyle birlikte kullanılmalıdır.

RBAC'ı kullanarak Kubernetes kaynaklarına erişimi denetlemek için Azure REKLAM gruplarını nasıl kullanacağımı görmek için, [AKS'de rol tabanlı erişim denetimlerini ve Azure Etkin Dizin kimliklerini kullanarak küme kaynaklarına erişimi denetleme'ye][azure-ad-rbac]bakın.

## <a name="use-pod-identities"></a>Bölme kimliklerini kullanma

**En iyi uygulama kılavuzu** - Bunlar maruz kalma veya kötüye kullanım riski altında olduğundan, bölme veya konteyner görüntüleri içinde sabit kimlik bilgileri kullanmayın. Bunun yerine, merkezi bir Azure AD kimlik çözümlerini kullanarak otomatik olarak erişim istemek için bölme kimliklerini kullanın. Pod kimlikleri yalnızca Linux bölmeleri ve konteyner görüntüleriyle kullanılmak üzere tasarlanmıştır.

Kapsüllerin Cosmos DB, Key Vault veya Blob Depolama gibi diğer Azure hizmetlerine erişmesi gerektiğinde, bölmenin erişim kimlik bilgilerine ihtiyacı vardır. Bu erişim kimlik bilgileri kapsayıcı görüntüsüyle tanımlanabilir veya Kubernetes sırrı olarak enjekte edilebilir, ancak el ile oluşturulması ve atanması gerekir. Kimlik bilgileri genellikle bölmeler arasında yeniden kullanılır ve düzenli olarak döndürülemez.

Azure kaynakları için yönetilen kimlikler (şu anda ilişkili bir AKS açık kaynak projesi olarak uygulanmaktadır) Azure AD aracılığıyla hizmetlere otomatik olarak erişim isteğinde bulunmanıza olanak tanır. Bölmeler için kimlik bilgilerini el ile tanımlamazsınız, bunun yerine gerçek zamanlı olarak bir erişim belirteci talep ederler ve yalnızca atanan hizmetlerine erişmek için kullanabilirler. AKS'de, bölmelerin yönetilen kimlikleri kullanmasına izin vermek için küme işleci tarafından iki bileşen dağıtılır:

* **Düğüm Yönetimi Kimliği (NMI) sunucusu,** AKS kümesindeki her düğümde DaemonSet olarak çalışan bir bölmedir. NMI sunucusu, Azure hizmetlerine yönelik bölme isteklerini dinler.
* **Yönetilen Kimlik Denetleyicisi (MIC),** Kubernetes API sunucusunu sorgulama izinleri olan merkezi bir bölmedir ve bir bölmeye karşılık gelen bir Azure kimlik eşlemi denetimleri sağlar.

Bölmeler bir Azure hizmetine erişim istediğinde, ağ kuralları trafiği Düğüm Yönetimi Kimliği (NMI) sunucusuna yönlendirir. NMI sunucusu, uzak adreslerine göre Azure hizmetlerine erişim isteyen bölmeleri tanımlar ve Yönetilen Kimlik Denetleyicisini (MIC) sorgular. MIC, AKS kümesindeki Azure kimlik eşlemelerini denetler ve NMI sunucusu pod'un kimlik eşlemesini temel alarak Azure Active Directory'den (AD) bir erişim belirteci ister. Azure AD, bölmeye döndürülen NMI sunucusuna erişim sağlar. Bu erişim belirteci, daha sonra Azure'daki hizmetlere erişim istemek için bölme tarafından kullanılabilir.

Aşağıdaki örnekte, bir geliştirici Azure SQL Server örneğine erişim istemek için yönetilen bir kimlik kullanan bir bölme oluşturur:

![Pod kimlikleri, bir bölmenin diğer hizmetlere otomatik olarak erişim istemesine izin verir](media/operator-best-practices-identity/pod-identities.png)

1. Küme işleci, önce bölmeler hizmetlere erişim istediğinde kimlikleri eşlemek için kullanılabilecek bir hizmet hesabı oluşturur.
1. NMI sunucusu ve MIC, erişim belirteçleri için herhangi bir bölme isteklerini Azure AD'ye aktarmak için dağıtılır.
1. Geliştirici, NMI sunucusu üzerinden erişim belirteci isteyen yönetilen bir kimliğe sahip bir bölme dağıtır.
1. Belirteç bölmeye döndürülür ve bir Azure SQL Server örneğine erişmek için kullanılır.

> [!NOTE]
> Yönetilen bölme kimlikleri açık kaynak kodlu bir projedir ve Azure teknik desteği tarafından desteklenmez.

Bölme kimliklerini kullanmak [için, Kubernetes uygulamaları için Azure Etkin Dizin kimlikleri'ne][aad-pod-identity]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu en iyi uygulamalar makalesi, kümeniz ve kaynaklarınız için kimlik doğrulama ve yetkilendirme ye odaklanmıştır. Bu en iyi uygulamalardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [Azure Etkin Dizini AKS ile tümleştir][aks-aad]
* [AKS ile Azure kaynakları için yönetilen kimlikleri kullanma][aad-pod-identity]

AKS'deki küme işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi uygulamalara bakın:

* [Çok kiracılılık ve küme yalıtımı][aks-best-practices-cluster-isolation]
* [Temel Kubernetes zamanlayıcı özellikleri][aks-best-practices-scheduler]
* [Gelişmiş Kubernetes zamanlayıcı özellikleri][aks-best-practices-advanced-scheduler]

<!-- EXTERNAL LINKS -->
[aad-pod-identity]: https://github.com/Azure/aad-pod-identity

<!-- INTERNAL LINKS -->
[aks-concepts-identity]: concepts-identity.md
[aks-aad]: azure-ad-integration-cli.md
[managed-identities:]: ../active-directory/managed-identities-azure-resources/overview.md
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[azure-ad-rbac]: azure-ad-rbac.md
