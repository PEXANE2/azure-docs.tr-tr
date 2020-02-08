---
title: Operatör en iyi yöntemleri-Azure Kubernetes hizmetlerindeki kimlik (AKS)
description: Azure Kubernetes Service (AKS) içindeki kümeler için kimlik doğrulama ve yetkilendirmeyi yönetmek üzere küme operatörü en iyi uygulamalarını öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: mlearned
ms.openlocfilehash: 35cded14977988c80455a9c28a0e01e7913d58f4
ms.sourcegitcommit: a460fdc19d6d7af6d2b5a4527e1b5c4e0c49942f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77069515"
---
# <a name="best-practices-for-authentication-and-authorization-in-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmetinde (AKS) kimlik doğrulama ve yetkilendirme için en iyi yöntemler

Azure Kubernetes Service (AKS) içinde kümeler dağıtıp bakımını yaparken, kaynak ve hizmetlere erişimi yönetmenin yollarını uygulamanız gerekir. Bu denetimler olmadan, hesapların ihtiyaç duyduklarında kaynak ve hizmetlere erişimi olabilir. Değişiklik yapmak için hangi kimlik bilgilerinin kullanıldığını da izlemek zor olabilir.

Bu en iyi yöntemler makalesi, bir küme işlecinin AKS kümelerinin erişimini ve kimliğini nasıl yönetebileceğini ele alır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure Active Directory ile AKS kümesi kullanıcılarının kimliğini doğrulama
> * Rol tabanlı erişim denetimleri (RBAC) ile kaynaklara erişimi denetleme
> * Diğer hizmetlerle kimlik doğrulamak için yönetilen bir kimlik kullanın

## <a name="use-azure-active-directory"></a>Azure Active Directory kullanma

**En iyi Yöntem Kılavuzu** -Azure AD TÜMLEŞTIRMESIYLE aks kümelerini dağıtma. Azure AD 'yi kullanarak kimlik yönetimi bileşenini merkezileştirin. Kullanıcı hesabı veya grup durumundaki herhangi bir değişiklik, AKS kümesine erişim için otomatik olarak güncelleştirilir. Kullanıcıları veya grupları gereken en az izin miktarına göre kapsam yapmak için, sonraki bölümde açıklandığı gibi rolleri veya Kümerolleri ve bağlamaları kullanın.

Kubernetes kümenizin geliştirici ve uygulama sahiplerinin farklı kaynaklara erişmesi gerekir. Kubernetes hangi kullanıcıların hangi kaynaklarla etkileşime girebileceklerini denetlemek için bir kimlik yönetimi çözümü sağlamaz. Bunun yerine, genellikle kümenizi mevcut bir kimlik çözümüyle tümleştirmeniz gerekir. Azure Active Directory (AD), kurumsal özellikli bir kimlik yönetimi çözümü sağlar ve AKS kümeleriyle tümleştirilebilir.

AKS 'de Azure AD ile tümleşik kümeler sayesinde, kaynaklara erişim izinleri tanımlayan *Roller* veya *kümerolleri* oluşturursunuz. Daha sonra rolleri Azure AD 'den kullanıcılara veya gruplara *bağlarsınız* . Bu Kubernetes rol tabanlı erişim denetimleri (RBAC) sonraki bölümde ele alınmıştır. Azure AD 'nin tümleştirilmesi ve kaynaklara erişimi nasıl denetlemenizden aşağıdaki diyagramda görünebilirler:

![AKS ile Azure Active Directory tümleştirme için küme düzeyinde kimlik doğrulaması](media/operator-best-practices-identity/cluster-level-authentication-flow.png)

1. Geliştirici Azure AD ile kimlik doğrulaması yapar.
1. Azure AD belirteç verme uç noktası erişim belirtecini yayınlar.
1. Geliştirici `kubectl create pod` gibi Azure AD belirtecini kullanarak bir eylem gerçekleştirir
1. Kubernetes, belirteci Azure Active Directory doğrular ve geliştiricinin grup üyeliklerini getirir.
1. Kubernetes rol tabanlı erişim denetimi (RBAC) ve küme ilkeleri uygulanır.
1. Geliştirici isteği, Azure AD grup üyeliği ve Kubernetes RBAC ve ilkelerine ait önceki doğrulamaya bağlı olarak başarılı veya başarısız olur.

Azure AD kullanan bir AKS kümesi oluşturmak için bkz. [Azure ACTIVE DIRECTORY aks Ile tümleştirme][aks-aad].

## <a name="use-role-based-access-controls-rbac"></a>Rol tabanlı erişim denetimlerini (RBAC) kullanma

**En iyi Yöntem Kılavuzu** -kullanıcıların veya grupların kümedeki kaynaklara sahip olduğu izinleri tanımlamak Için Kubernetes RBAC kullanın. Gerekli en az izin miktarını atayan roller ve bağlamalar oluşturun. Azure AD ile tümleştirin böylece Kullanıcı durumu veya grup üyeliğindeki tüm değişiklikler otomatik olarak güncelleştirilir ve küme kaynaklarına erişim geçerli olur.

Kubernetes 'de, kümedeki kaynaklara yönelik ayrıntılı denetim sağlayabilirsiniz. İzinler, küme düzeyinde veya belirli ad alanları ile tanımlanabilir. Hangi kaynakların yönetilebileceklerini ve hangi izinlere sahip olduğunu tanımlayabilirsiniz. Bu roller daha sonra bağlama ile kullanıcılara veya gruplara uygulanır. *Roller*, *Kümerolleri*ve *bağlamalar*hakkında daha fazla bilgi Için bkz. [Azure KUBERNETES hizmeti (aks) için erişim ve kimlik seçenekleri][aks-concepts-identity].

Örnek olarak, aşağıdaki örnekteki YAML bildiriminde gösterildiği gibi *finans-App*adlı ad alanındaki kaynaklara tam erişim veren bir rol oluşturabilirsiniz:

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

Daha sonra, aşağıdaki YAML bildiriminde gösterildiği gibi Azure AD user *developer1\@contoso.com* 'ı rolebinding 'e bağlayan bir rolebinding oluşturulur:

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

AKS kümesinde *developer1\@contoso.com* kimlik doğrulaması yapıldığında, *finans-uygulama* ad alanındaki kaynaklar üzerinde tam izinlere sahip olurlar. Bu şekilde, kaynaklara erişimi mantıksal olarak ayırabilirsiniz ve kontrol edersiniz. Kubernetes RBAC, önceki bölümde anlatıldığı gibi Azure AD tümleştirmesi ile birlikte kullanılmalıdır.

RBAC kullanarak Kubernetes kaynaklarına erişimi denetlemek için Azure AD gruplarını nasıl kullanacağınızı görmek için bkz. [rol tabanlı erişim denetimleri ve AKS 'de Azure Active Directory kimlikleri kullanarak küme kaynaklarına erişimi denetleme][azure-ad-rbac].

## <a name="use-pod-identities"></a>Pod kimliklerini kullanma

**En iyi Yöntem Kılavuzu** -, pozlama veya kötüye kullanım riski altında olduklarından, Pod veya kapsayıcı görüntüleri içinde sabit kimlik bilgilerini kullanmayın. Bunun yerine, merkezi bir Azure AD kimlik çözümü kullanarak otomatik olarak erişim istemek için pod kimliklerini kullanın. Pod kimlikleri yalnızca Linux Pod ve kapsayıcı görüntüleri ile kullanılmaya yöneliktir.

Cosmos DB, Key Vault veya blob depolama gibi diğer Azure hizmetlerine erişim gerektiğinde Pod 'ın erişim kimlik bilgilerine ihtiyacı vardır. Bu erişim kimlik bilgileri kapsayıcı görüntüsü ile tanımlanabilir veya bir Kubernetes parolası olarak eklenebilir, ancak el ile oluşturulup atanması gerekir. Genellikle, kimlik bilgileri eksik ve düzenli olarak döndürülmemektedir.

Azure kaynakları için Yönetilen kimlikler (Şu anda ilişkili bir AKS açık kaynak projesi olarak uygulanır), Azure AD aracılığıyla hizmetlere otomatik olarak erişim isteme izni verir. Bir erişim belirtecini gerçek zamanlı olarak istedikleri ve yalnızca atanmış hizmetlerine erişmek için kullanabilmesi için, pods 'nin kimlik bilgilerini el ile tanımlayamazsınız. AKS 'de, Dizin 'lerin yönetilen kimlikleri kullanmasına izin vermek için küme operatörü tarafından iki bileşen dağıtılır:

* **Düğüm yönetimi kimliği (NMI) sunucusu** , aks kümesindeki her bir düğümde DaemonSet olarak çalışan bir pod 'dir. NMI sunucusu, Azure hizmetlerine Pod isteklerini dinler.
* **Yönetilen kimlik denetleyicisi (MIC)** , Kubernetes API sunucusunu sorgulama ve pod 'a karşılık gelen bir Azure kimlik eşlemesini denetleme izinlerine sahip merkezi bir pod 'dir.

Pod bir Azure hizmetine erişim isteğinde bulunduğunda, ağ kuralları trafiği düğüm yönetim kimliği (NMI) sunucusuna yönlendirir. NMI sunucusu, uzak adreslerine bağlı olarak Azure hizmetlerine erişim isteyen Pod 'yi tanımlar ve yönetilen kimlik denetleyicisini (MIC) sorgular. MıC, AKS kümesinde Azure kimlik eşlemelerini denetler ve NMI sunucusu, Pod 'un kimlik eşlemesine göre Azure Active Directory (AD) öğesinden bir erişim belirteci ister. Azure AD, Pod 'a döndürülen NMI sunucusuna erişim sağlar. Bu erişim belirteci, Pod tarafından Azure 'daki hizmetlere erişim istemek için kullanılabilir.

Aşağıdaki örnekte, bir geliştirici Azure SQL Server örneğine erişim istemek için yönetilen kimlik kullanan bir pod oluşturuyor:

![Pod kimlikleri, Pod 'ın diğer hizmetlere otomatik olarak erişim istemesine izin verir](media/operator-best-practices-identity/pod-identities.png)

1. Küme operatörü ilk olarak, bir pod hizmetlere erişim isteğinde bulunduğunda kimlikleri eşlemek için kullanılabilen bir hizmet hesabı oluşturur.
1. Azure AD 'ye erişim belirteçleri için herhangi bir pod isteği geçirmek üzere NMI sunucusu ve MIK dağıtılır.
1. Bir geliştirici, NMI sunucusu aracılığıyla erişim belirteci isteyen yönetilen bir kimlikle Pod dağıtır.
1. Belirteç Pod 'a döndürülür ve Azure SQL Server örneğine erişmek için kullanılır.

> [!NOTE]
> Yönetilen Pod kimlikleri açık kaynaklı bir projem ve Azure teknik desteği tarafından desteklenmiyor.

Pod kimliklerini kullanmak için bkz. [Kubernetes uygulamaları için Azure Active Directory kimlikleri][aad-pod-identity].

## <a name="next-steps"></a>Sonraki adımlar

Bu en iyi yöntemler, kümenizin ve kaynaklarınızın kimlik doğrulama ve yetkilendirilmesine odaklanan makaledir. Bu en iyi uygulamalardan bazılarını uygulamak için aşağıdaki makalelere bakın:

* [Azure Active Directory AKS ile tümleştirme][aks-aad]
* [AKS ile Azure kaynakları için Yönetilen kimlikler kullanma][aad-pod-identity]

AKS 'deki küme işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi yöntemlere bakın:

* [Çok kiracılı ve küme yalıtımı][aks-best-practices-cluster-isolation]
* [Temel Kubernetes Zamanlayıcı özellikleri][aks-best-practices-scheduler]
* [Gelişmiş Kubernetes Zamanlayıcı özellikleri][aks-best-practices-advanced-scheduler]

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
