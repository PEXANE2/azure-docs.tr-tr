---
title: Azure Digital Twins çözümleri için güvenlik
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS ile en iyi güvenlik uygulamalarını anlayın.
author: baanders
ms.author: baanders
ms.date: 3/18/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: f438f4393538478f2960e709d8e23beb6106510d
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92485758"
---
# <a name="secure-azure-digital-twins"></a>Güvenli Azure dijital TWINS

Azure dijital TWINS güvenliği için, dağıtımınızdaki belirli veriler, kaynaklar ve eylemler üzerinde kesin erişim denetimi imkanı sunar. Bunu, **Azure rol tabanlı erişim denetimi (Azure RBAC)** adlı ayrıntılı bir rol ve izin yönetimi stratejisi aracılığıyla yapar. Azure RBAC 'nin genel ilkeleri hakkında [buradan](../role-based-access-control/overview.md)bilgi edinebilirsiniz.

Azure dijital TWINS, bekleyen verilerin şifrelenmesini de destekler.

## <a name="granting-permissions-with-azure-rbac"></a>Azure RBAC ile izin verme

Azure RBAC, [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) ile tümleştirme yoluyla Azure dijital TWINS 'e sağlanır.

Bir Kullanıcı, Grup veya uygulama hizmeti sorumlusu olabilecek bir *güvenlik sorumlusu*için izin vermek üzere Azure RBAC kullanabilirsiniz. Güvenlik sorumlusu Azure AD tarafından doğrulanır ve döndürülen bir OAuth 2,0 belirteci alır. Bu belirteç, bir Azure dijital TWINS örneğine erişim isteği yetkilendirmek için kullanılabilir.

### <a name="authentication-and-authorization"></a>Kimlik doğrulaması ve yetkilendirme

Azure AD ile, erişim iki adımlı bir işlemdir. Bir güvenlik sorumlusu (bir Kullanıcı, Grup veya uygulama) Azure dijital TWINS 'e erişmeyi denediğinde, isteğin *kimliğinin doğrulanması* ve *yetkilendirilmiş*olması gerekir. 

1. İlk olarak, güvenlik sorumlusunun kimliği *doğrulanır*ve bir OAuth 2,0 belirteci döndürülür.
2. Ardından, belirteç, belirtilen kaynağa erişimi *yetkilendirmek* Için Azure dijital TWINS hizmetine yapılan bir isteğin parçası olarak geçirilir.

Kimlik doğrulama adımı, herhangi bir uygulama isteğinin çalışma zamanında bir OAuth 2,0 erişim belirteci içermesini gerektirir. Bir uygulama, [Azure işlevleri](../azure-functions/functions-overview.md) uygulaması gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için **yönetilen bir kimlik** kullanabilir. Sonraki bölümde Yönetilen kimlikler hakkında daha fazla bilgi edinin.

Yetkilendirme adımı, güvenlik sorumlusuna bir Azure rolünün atanmasını gerektirir. Bir güvenlik sorumlusu 'na atanan roller, sorumlunun sahip olacağı izinleri belirleyebilir. Azure dijital TWINS, Azure dijital TWINS kaynakları için izin kümelerini çevreleyen Azure rolleri sağlar. Bu roller, bu makalenin ilerleyen kısımlarında açıklanmıştır.

Azure 'da desteklenen roller ve rol atamaları hakkında daha fazla bilgi edinmek için bkz. Azure RBAC belgelerindeki [*farklı rolleri anlama*](../role-based-access-control/rbac-and-directory-admin-roles.md) .

#### <a name="authentication-with-managed-identities"></a>Yönetilen kimliklerle kimlik doğrulaması

[Azure kaynakları Için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) , uygulama kodunuzun çalıştırıldığı dağıtımla ilişkili güvenli bir kimlik oluşturmanıza olanak sağlayan bir çapraz Azure özelliğidir. Böylece, uygulamanızın ihtiyaç duyacağı belirli Azure kaynaklarına erişim için özel izinler vermek üzere bu kimliği erişim denetimi rolleriyle ilişkilendirebilirsiniz.

Yönetilen kimlikler ile Azure platformu bu çalışma zamanı kimliğini yönetir. Uygulama kodunuzda veya yapılandırmanızda, kimliğin kendisi için ya da erişmeniz gereken kaynaklar için erişim anahtarlarını depolamanız ve korumanız gerekmez. Azure App Service uygulamasının içinde çalışan bir Azure dijital TWINS istemci uygulamasının SAS kurallarını ve anahtarlarını veya diğer erişim belirteçlerini işlemesi gerekmez. İstemci uygulaması yalnızca Azure Digital TWINS ad alanının uç nokta adresine ihtiyaç duyuyor. Uygulama bağlandığında, Azure Digital TWINS yönetilen varlığın bağlamını istemciye bağlar. Yönetilen bir kimlikle ilişkilendirildikten sonra Azure dijital TWINS istemciniz tüm yetkili işlemleri gerçekleştirebilir. Daha sonra, yönetilen bir varlık bir Azure dijital TWINS Azure rolüyle (aşağıda açıklanmıştır) ilişkilendirilerek yetkilendirme verilecektir.

#### <a name="authorization-azure-roles-for-azure-digital-twins"></a>Yetkilendirme: Azure dijital TWINS için Azure rolleri

Azure, bir Azure dijital TWINS kaynağına erişimi yetkilendirmek için aşağıdaki Azure yerleşik rollerini sağlar:
* *Azure dijital TWINS veri sahibi* – Azure dijital TWINS kaynakları üzerinden tam erişim sağlamak için bu rolü kullanın.
* *Azure dijital TWINS veri okuyucusu* – Azure dijital TWINS kaynaklarına salt okuma erişimi sağlamak için bu rolü kullanın.

>[!NOTE]
> Bu roller yakın zamanda Önizlemedeki önceki adlarından yeniden adlandırıldı:
> * *Azure dijital TWINS veri sahibi* , daha önce *Azure Digital TWINS sahibiydi (Önizleme)*.
> * *Azure dijital TWINS veri okuyucu* daha önce *Azure Digital TWINS okuyucu (Önizleme)*.

Yerleşik rollerin nasıl tanımlandığı hakkında daha fazla bilgi için bkz. Azure RBAC belgelerindeki [*rol tanımlarını anlama*](../role-based-access-control/role-definitions.md) . Azure özel rolleri oluşturma hakkında daha fazla bilgi için bkz. [*Azure özel roller*](../role-based-access-control/custom-roles.md).

Rolleri iki şekilde atayabilirsiniz:
* Azure portal Azure dijital TWINS için erişim denetimi (ıAM) bölmesi aracılığıyla (bkz [*. Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma*](../role-based-access-control/role-assignments-portal.md))
* bir rol eklemek veya kaldırmak için CLı komutları aracılığıyla

Bunun nasıl yapılacağı hakkında daha ayrıntılı adımlar için Azure dijital TWINS [*öğreticisinde deneyin: uçtan uca bir çözümü bağlama*](tutorial-end-to-end.md).

### <a name="permission-scopes"></a>İzin kapsamları

Güvenlik sorumlusuna bir Azure rolü atamadan önce, güvenlik sorumlusunun sahip olması gereken erişimin kapsamını saptayın. En iyi uygulamalar, yalnızca en dar olası kapsamı vermek için idealdir.

Aşağıdaki listede, Azure dijital TWINS kaynaklarına erişimi kapsama ekleyebileceğiniz düzeyler açıklanmaktadır.
* Modeller: Bu kaynak için Eylemler, Azure dijital TWINS 'te karşıya yüklenen [modeller](concepts-models.md) üzerinde denetim dikte ediyor.
* Sorgu dijital TWINS grafiği: Bu kaynak için Eylemler, Azure dijital TWINS grafiğinde dijital TWINS üzerinde [sorgu işlemleri](concepts-query-language.md) çalıştırma yeteneğini belirleme.
* Digital Ikizi: Bu kaynak için Eylemler, ikizi grafiğindeki [dijital TWINS](concepts-twins-graph.md) üzerinde CRUD işlemlerine denetim sağlar.
* Digital Ikizi Relationship: Bu kaynak için Eylemler, ikizi grafiğindeki dijital TWINS arasındaki [ilişkilerdeki](concepts-twins-graph.md) CRUD işlemleri üzerinde denetimi tanımlar.
* Olay rotası: Bu kaynak için Eylemler, Azure dijital TWINS 'deki olayları [Olay Hub](../event-hubs/event-hubs-about.md)'ı, [Event Grid](../event-grid/overview.md)veya [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)gibi bir uç nokta hizmetine [yönlendiren](concepts-route-events.md) izinleri belirleyebilir.

### <a name="troubleshooting-permissions"></a>İzinlerin sorunlarını giderme

Bir kullanıcı rolü tarafından izin verilmeyen bir eylem gerçekleştirmeye çalışırsa, hizmet isteği okumada bir hata alabilir `403 (Forbidden)` . Daha fazla bilgi ve sorun giderme adımları için bkz. [*sorun giderme: Azure dijital TWINS isteği şu durumla başarısız oldu: 403 (yasak)*](troubleshoot-error-403.md).

## <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi

Azure dijital TWINS, veri merkezlerimizde yazıldığı sırada verilerin geri kalanı ve aktarım sırasında şifrelenmesini sağlar ve ona erişirken şifresini çözer. Bu şifreleme, Microsoft tarafından yönetilen bir şifreleme anahtarı kullanılarak oluşur.

## <a name="cross-origin-resource-sharing-cors"></a>Çıkış Noktaları Arası Kaynak Paylaşımı (CORS)

Azure dijital TWINS Şu anda **çıkış noktaları arası kaynak paylaşımını (CORS)** desteklememektedir. Sonuç olarak, bir tarayıcı uygulamasından, bir [API Management (APıM)](../api-management/api-management-key-concepts.md) arabiriminden veya bir [Power Apps](https://docs.microsoft.com/powerapps/powerapps-overview) bağlayıcısından REST API arıyorsanız bir ilke hatası görebilirsiniz.

Bu hatayı çözmek için aşağıdakilerden birini yapabilirsiniz:
* İletibir bilgisayardan CORS üst bilgisini şerit `Access-Control-Allow-Origin` . Bu üstbilgi yanıtın paylaşılıp paylaşılamayacağını gösterir. 
* Alternatif olarak, bir CORS proxy 'si oluşturun ve Azure dijital TWINS REST API isteği üzerinden isteyin. 

## <a name="next-steps"></a>Sonraki adımlar

* [*Nasıl yapılır: bir örneği ve kimlik doğrulamasını ayarlama*](how-to-set-up-instance-portal.md)bölümünde bu kavramlara bakın.

* [*Nasıl yapılır: yazma uygulaması kimlik doğrulaması kodunda*](how-to-authenticate-client.md)istemci uygulama kodundan bu kavramlarla nasıl etkileşim kuracağınızı öğrenin.

* [Azure RBAC](../role-based-access-control/overview.md)hakkında daha fazla bilgi edinin.
