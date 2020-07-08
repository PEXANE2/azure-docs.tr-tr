---
title: Azure Digital Twins çözümleri için güvenlik
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS ile en iyi güvenlik uygulamalarını anlayın.
author: baanders
ms.author: baanders
ms.date: 3/18/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 0f1f9e17646c4432d9c1103b3c579fc6bb0d2f13
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85833173"
---
# <a name="secure-azure-digital-twins-with-role-based-access-control"></a>Rol tabanlı erişim denetimi ile Azure dijital TWINS güvenliğini sağlama

Azure dijital TWINS güvenliği için, dağıtımınızdaki belirli veriler, kaynaklar ve eylemler üzerinde kesin erişim denetimi imkanı sunar. Bu, **rol tabanlı erişim denetimi (RBAC)** adlı ayrıntılı bir rol ve izin yönetimi stratejisi aracılığıyla yapılır. Azure için RBAC 'in genel ilkelerine [buradan](../role-based-access-control/overview.md)ulaşabilirsiniz.

## <a name="rbac-through-azure-ad"></a>Azure AD aracılığıyla RBAC

RBAC, Azure dijital TWINS 'e [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) ile tümleştirme yoluyla sağlanır.

Bir *güvenlik sorumlusu*için, bir Kullanıcı, Grup veya uygulama hizmeti sorumlusu olabilecek izinler vermek için RBAC kullanabilirsiniz. Güvenlik sorumlusu Azure AD tarafından doğrulanır ve döndürülen bir OAuth 2,0 belirteci alır. Bu belirteç, bir Azure dijital TWINS örneğine erişim isteği yetkilendirmek için kullanılabilir.

## <a name="authentication-and-authorization"></a>Kimlik doğrulaması ve yetkilendirme

Azure AD ile, erişim iki adımlı bir işlemdir. Bir güvenlik sorumlusu (bir Kullanıcı, Grup veya uygulama) Azure dijital TWINS 'e erişmeyi denediğinde, isteğin *kimliğinin doğrulanması* ve *yetkilendirilmiş*olması gerekir. 

1. İlk olarak, güvenlik sorumlusunun kimliği *doğrulanır*ve bir OAuth 2,0 belirteci döndürülür.
2. Ardından, belirteç, belirtilen kaynağa erişimi *yetkilendirmek* Için Azure dijital TWINS hizmetine yapılan bir isteğin parçası olarak geçirilir.

Kimlik doğrulama adımı, herhangi bir uygulama isteğinin çalışma zamanında bir OAuth 2,0 erişim belirteci içermesini gerektirir. Bir uygulama, [Azure işlevleri](../azure-functions/functions-overview.md) uygulaması gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için **yönetilen bir kimlik** kullanabilir. Sonraki bölümde Yönetilen kimlikler hakkında daha fazla bilgi edinin.

Yetkilendirme adımı, güvenlik sorumlusuna bir RBAC rolünün atanmasını gerektirir. Bir güvenlik sorumlusu 'na atanan roller, sorumlunun sahip olacağı izinleri belirleyebilir. Azure dijital TWINS, Azure dijital TWINS kaynakları için izin kümelerini çevreleyen RBAC rolleri sağlar. Bu roller, bu makalenin ilerleyen kısımlarında açıklanmıştır.

Azure 'da desteklenen roller ve rol atamaları hakkında daha fazla bilgi edinmek için bkz. Azure RBAC belgelerindeki [farklı rolleri anlama](../role-based-access-control/rbac-and-directory-admin-roles.md) .

### <a name="authentication-with-managed-identities"></a>Yönetilen kimliklerle kimlik doğrulaması

[Azure kaynakları Için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) , uygulama kodunuzun çalıştırıldığı dağıtımla ilişkili güvenli bir kimlik oluşturmanıza olanak sağlayan bir çapraz Azure özelliğidir. Böylece, uygulamanızın ihtiyaç duyacağı belirli Azure kaynaklarına erişim için özel izinler vermek üzere bu kimliği erişim denetimi rolleriyle ilişkilendirebilirsiniz.

Yönetilen kimlikler ile Azure platformu bu çalışma zamanı kimliğini yönetir. Uygulama kodunuzda veya yapılandırmanızda, kimliğin kendisi için ya da erişmeniz gereken kaynaklar için erişim anahtarlarını depolamanız ve korumanız gerekmez. Azure App Service uygulamasının içinde çalışan bir Azure dijital TWINS istemci uygulamasının SAS kurallarını ve anahtarlarını veya diğer erişim belirteçlerini işlemesi gerekmez. İstemci uygulaması yalnızca Azure Digital TWINS ad alanının uç nokta adresine ihtiyaç duyuyor. Uygulama bağlandığında, Azure Digital TWINS yönetilen varlığın bağlamını istemciye bağlar. Yönetilen bir kimlikle ilişkilendirildikten sonra Azure dijital TWINS istemciniz tüm yetkili işlemleri gerçekleştirebilir. Daha sonra, yönetilen bir varlık bir Azure dijital TWINS RBAC rolüyle ilişkilendirerek yetkilendirme verilecektir (aşağıda açıklanmıştır).

### <a name="authorization-rbac-roles-for-azure-digital-twins"></a>Yetkilendirme: Azure dijital TWINS için RBAC rolleri

Azure, bir Azure dijital TWINS kaynağına erişimi yetkilendirmek için aşağıdaki yerleşik RBAC rollerini sağlar:
* Azure dijital TWINS sahibi (Önizleme) – Azure dijital TWINS kaynakları üzerinden tam erişim sağlamak için bu rolü kullanın.
* Azure dijital TWINS okuyucu (Önizleme) – Azure dijital TWINS kaynaklarına salt okuma erişimi sağlamak için bu rolü kullanın.

> [!TIP]
> Azure dijital TWINS okuyucu (Önizleme) rolü artık gözatma ilişkilerini de destekliyor.

Yerleşik rollerin nasıl tanımlandığı hakkında daha fazla bilgi için bkz. Azure RBAC belgelerindeki [rol tanımlarını anlama](../role-based-access-control/role-definitions.md) . Özel RBAC rolleri oluşturma hakkında daha fazla bilgi için bkz. [Azure kaynakları Için özel roller](../role-based-access-control/custom-roles.md).

Rolleri iki şekilde atayabilirsiniz:
* Azure portal Azure dijital TWINS için erişim denetimi (ıAM) bölmesi aracılığıyla (bkz. [Azure RBAC kullanarak rol atamaları ekleme veya kaldırma ve Azure Portal](../role-based-access-control/role-assignments-portal.md))
* bir rol eklemek veya kaldırmak için CLı komutları aracılığıyla

Bunun nasıl yapılacağı hakkında daha ayrıntılı adımlar için [Azure dijital TWINS öğreticisinde deneyin: uçtan uca bir çözümü bağlama](tutorial-end-to-end.md).

## <a name="permission-scopes"></a>İzin kapsamları

Bir güvenlik sorumlusuna RBAC rolü atamadan önce, güvenlik sorumlusunun sahip olması gereken erişimin kapsamını saptayın. En iyi uygulamalar, yalnızca en dar olası kapsamı vermek için idealdir.

Aşağıdaki listede, Azure dijital TWINS kaynaklarına erişimi kapsama ekleyebileceğiniz düzeyler açıklanmaktadır.
* Modeller: Bu kaynak için Eylemler, Azure dijital TWINS 'te karşıya yüklenen [modeller](concepts-models.md) üzerinde denetim dikte ediyor.
* Sorgu dijital TWINS grafiği: Bu kaynak için Eylemler, Azure dijital TWINS grafiğinde dijital TWINS üzerinde [sorgu işlemleri](concepts-query-language.md) çalıştırma yeteneğini belirleme.
* Digital Ikizi: Bu kaynak için Eylemler, ikizi grafiğindeki [dijital TWINS](concepts-twins-graph.md) üzerinde CRUD işlemlerine denetim sağlar.
* Digital Ikizi Relationship: Bu kaynak için Eylemler, ikizi grafiğindeki dijital TWINS arasındaki [ilişkilerdeki](concepts-twins-graph.md) CRUD işlemleri üzerinde denetimi tanımlar.
* Olay rotası: Bu kaynak için Eylemler, Azure dijital TWINS 'deki olayları [Olay Hub](../event-hubs/event-hubs-about.md)'ı, [Event Grid](../event-grid/overview.md)veya [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)gibi bir uç nokta hizmetine [yönlendiren](concepts-route-events.md) izinleri belirleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Bkz. [nasıl yapılır: istemci uygulamasında kimlik doğrulama](how-to-authenticate-client.md)ile ilgili bir örnek istemci uygulamasıyla bu adımlarda izlenecek yol.

* [Azure Için RBAC](../role-based-access-control/overview.md)hakkında daha fazla bilgi edinin.
