---
title: Azure AD sağlamasının nasıl çalıştığını anlayın | Microsoft Docs
description: Azure AD sağlamasının nasıl çalıştığını anlama
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: conceptual
ms.workload: identity
ms.date: 05/20/2020
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 38ddfad179a8ef459b5c10f84619a836193ee092
ms.sourcegitcommit: 52d2f06ecec82977a1463d54a9000a68ff26b572
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84781659"
---
# <a name="how-provisioning-works"></a>Sağlama nasıl çalışır?

Otomatik sağlama, kullanıcıların erişmesi gereken bulut uygulamalarında kullanıcı kimliklerinin ve rollerinin oluşturulmasına başvurur. Otomatik sağlama, kullanıcı kimliklerinin oluşturulmasına ek olarak, durum veya rol değişikliği olarak Kullanıcı kimliklerinin bakımını ve kaldırılmasını içerir. Dağıtıma başlamadan önce, Azure AD sağlama 'nın nasıl çalıştığını ve yapılandırma önerilerini alma hakkında bilgi edinmek için bu makaleyi gözden geçirebilirsiniz. 

**Azure AD sağlama hizmeti** , uygulama satıcısı tarafından sağlanan etki alanları arası kimlik yönetimi (SCIM) 2,0 Kullanıcı yönetimi API uç noktası Için bir sisteme bağlanarak kullanıcılara ve diğer sistemlere SaaS uygulamaları sağlar. Bu SCıM uç noktası, Azure AD 'nin Kullanıcı aracılığıyla kullanıcıları oluşturmasına, güncelleştirmesine ve kaldırmasına izin verir. Seçilen uygulamalarda, sağlama hizmeti gruplar ve roller gibi kimlik ile ilgili diğer nesneleri de oluşturabilir, güncelleştirebilir ve kaldırabilir. Azure AD ile uygulama arasında sağlama için kullanılan kanal, HTTPS TLS 1,2 şifrelemesi kullanılarak şifrelenir.


![Azure AD sağlama hizmeti ](./media/how-provisioning-works/provisioning0.PNG)
 *Şekil 1: Azure AD sağlama hizmeti*

![Giden Kullanıcı sağlama iş akışı ](./media/how-provisioning-works/provisioning1.PNG)
 *Şekil 2: Azure AD 'Den popüler SaaS uygulamalarına "giden" Kullanıcı sağlama iş akışı*

![Gelen Kullanıcı sağlama iş akışı ](./media/how-provisioning-works/provisioning2.PNG)
 *Şekil 3: popüler ınsan büyük yönetim (HCM) uygulamalarından Azure Active Directory ve Windows Server 'A "gelen" Kullanıcı sağlama iş akışı Active Directory*

## <a name="provisioning-using-scim-20"></a>SCıM 2,0 kullanarak sağlama

Azure AD sağlama hizmeti otomatik sağlama için [SCIM 2,0 protokolünü](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/bg-p/IdentityStandards) kullanır. Hizmet, uygulama için SCıM uç noktasına bağlanır ve kullanıcıların ve grupların sağlanması ve sağlanması için otomatik hale getirme işlemini otomatikleştirmek üzere SCıM Kullanıcı nesne şemasını ve REST API 'Lerini kullanır. Azure AD galerisinde çoğu uygulama için bir SCıM tabanlı sağlama Bağlayıcısı sağlanır. Azure AD için uygulama oluştururken, geliştiriciler SCıM 2,0 kullanıcı yönetim API 'sini kullanarak Azure AD 'yi sağlama için tümleştiren bir SCıM uç noktası oluşturabilir. Ayrıntılar için bkz. [SCıM uç noktası oluşturma ve Kullanıcı sağlamayı yapılandırma](../app-provisioning/use-scim-to-provision-users-and-groups.md).

Şu anda biri olmayan bir uygulama için otomatik bir Azure AD sağlama Bağlayıcısı istemek üzere bir [Azure Active Directory uygulama isteği](https://aka.ms/aadapprequest)doldurun.

## <a name="authorization"></a>Yetkilendirme

Azure AD 'nin uygulamanın kullanıcı yönetim API 'sine bağlanması için kimlik bilgileri gereklidir. Bir uygulama için otomatik Kullanıcı sağlamayı yapılandırırken, geçerli kimlik bilgilerini girmeniz gerekir. Uygulama öğreticisine başvurarak uygulamanın kimlik bilgisi türlerini ve gereksinimlerini bulabilirsiniz. Azure portal, Azure AD 'nin sağlanan kimlik bilgilerini kullanarak uygulamanın sağlama uygulamasına bağlanmasını sağlayarak kimlik bilgilerini sınayabileceksiniz.

SAML tabanlı çoklu oturum açma, uygulama için de yapılandırılmışsa Azure AD 'nin iç, uygulama başına depolama sınırı 1024 bayttır. Bu sınır, bir uygulamanın tek bir örneğiyle ilişkili tüm sertifikaları, gizli belirteçleri, kimlik bilgilerini ve ilgili yapılandırma verilerini (Azure AD 'de hizmet sorumlusu kaydı olarak da bilinir) içerir. SAML tabanlı çoklu oturum açma yapılandırıldığında, SAML belirteçlerini imzalamak için kullanılan sertifika, genellikle alanın yüzde 50 ' ünü kullanır. Kullanıcı hazırlama kurulumu sırasında girdiğiniz ek öğeler (gizli belirteçler, URI 'Ler, bildirim e-posta adresleri, Kullanıcı adları ve parolalar) depolama sınırını aşabilir. Daha fazla bilgi için, bkz. [Kullanıcı sağlama yapılandırılırken yönetici kimlik bilgilerini kaydetme sorunu](../manage-apps/application-provisioning-config-problem-storage-limit.md).

## <a name="mapping-attributes"></a>Öznitelikleri eşleme

Bir üçüncü taraf SaaS uygulaması için Kullanıcı sağlamayı etkinleştirdiğinizde Azure portal, öznitelik değerlerini öznitelik eşlemeleriyle denetler. Eşlemeler, Kullanıcı hesapları sağlandığında veya güncelleştirilirken Azure AD ile hedef uygulama arasında akan Kullanıcı özniteliklerini belirleme.

Azure AD Kullanıcı nesneleri ve her bir SaaS uygulamasının Kullanıcı nesneleri arasında önceden yapılandırılmış bir öznitelikler ve öznitelik eşlemeleri kümesi vardır. Bazı uygulamalar, gruplar gibi kullanıcılarla birlikte diğer nesne türlerini de yönetir.

Sağlamayı ayarlarken, hangi kullanıcı (veya grup) özelliklerinin Azure AD 'den uygulamaya akmasını tanımlayan öznitelik eşlemelerini ve iş akışlarını gözden geçirmeniz ve yapılandırmanız önemlidir. İki sistem arasındaki kullanıcıları/grupları benzersiz şekilde tanımlamak ve eşleştirmek için kullanılan eşleşen özelliği (**Bu özniteliği kullanarak nesneleri Eşleştir**) gözden geçirin ve yapılandırın.

Varsayılan öznitelik eşlemelerini iş gereksinimlerinize göre özelleştirebilirsiniz. Bu nedenle, var olan öznitelik eşlemelerini değiştirebilir veya silebilir veya yeni öznitelik eşlemeleri oluşturabilirsiniz. Ayrıntılar için bkz. [SaaS uygulamaları için Kullanıcı hazırlama özniteliğini özelleştirme-eşlemeler](../manage-apps/customize-application-attributes.md).

Sağlamayı bir SaaS uygulamasına yapılandırdığınızda, belirtebileceğiniz öznitelik eşlemelerinin türlerinden biri bir ifade eşlemedir. Bu eşlemeler için, kullanıcılarınızın verilerini SaaS uygulaması için daha kabul edilebilir biçimlere dönüştürmenizi sağlayan bir betik benzeri ifade yazmalısınız. Ayrıntılar için bkz. [öznitelik eşlemeleri için Ifadeler yazma](functions-for-customizing-application-data.md).

## <a name="scoping"></a>Kapsamlar 
### <a name="assignment-based-scoping"></a>Atama tabanlı kapsam

Azure AD 'den bir SaaS uygulamasına giden sağlama için, [Kullanıcı veya grup atamalarına](../manage-apps/assign-user-or-group-access-portal.md) güvenmek, hangi kullanıcıların sağlama kapsamında olduğunu belirlemenin en yaygın yoludur. Kullanıcı atamaları çoklu oturum açmayı etkinleştirmek için de kullanıldığından, aynı yöntem hem erişim hem de sağlamayı yönetmek için kullanılabilir. Atama tabanlı kapsam, Workday ve başarılı etmenler gibi gelen sağlama senaryolarına uygulanmaz.

* **Gruplandıran.** Bir Azure AD Premium lisans planıyla, bir SaaS uygulamasına erişim atamak için grupları kullanabilirsiniz. Ardından, sağlama kapsamı **yalnızca atanmış kullanıcıları ve grupları Eşitle**olarak AYARLANDıĞıNDA Azure AD sağlama hizmeti, uygulamaya atanan bir grubun üyesi olup olmadıkları temel alınarak kullanıcıları sağlar veya serbest bırakılır. Uygulama, Grup nesnelerini desteklemediği takdirde grup nesnesinin kendisi tarafından sağlanmamıştır. Uygulamanıza atanan grupların "SecurityEnabled" özelliği "true" olarak ayarlanmış olduğundan emin olun.

* **Dinamik Gruplar.** Azure AD Kullanıcı sağlama hizmeti, [dinamik gruplardaki](../users-groups-roles/groups-create-rule.md)kullanıcıları okuyabilir ve sağlayabilir. Bu uyarıları ve önerileri göz önünde bulundurun:

  * Dinamik Gruplar, Azure AD 'den SaaS uygulamalarına kadar uçtan uca sağlamanın performansını etkileyebilir.

  * Bir SaaS uygulamasında, dinamik bir gruptaki bir kullanıcının ne kadar hızlı sağlandığını veya sağlanması gerektiğini, dinamik grubun üyelik değişikliklerini ne kadar hızlı değerlendirebileceğinizi gösterir. Dinamik bir grubun işleme durumunu denetleme hakkında daha fazla bilgi için bkz. [bir üyelik kuralı için işleme durumunu denetleme](../users-groups-roles/groups-create-rule.md).

  * Bir kullanıcı dinamik gruptaki üyeliği kaybettiğinde, bu, bir sağlama olayı olarak kabul edilir. Dinamik Gruplar için kurallar oluştururken bu senaryoyu göz önünde bulundurun.

* **İç içe gruplar.** Azure AD Kullanıcı sağlama hizmeti iç içe gruplardaki kullanıcıları okuyamıyor veya sağlayamaz. Hizmet yalnızca açıkça atanan bir grubun hemen üyesi olan kullanıcıları okuyabilir ve sağlayabilir. "Uygulamalara grup tabanlı atamalar" da bu sınırlama çoklu oturum açmayı da etkiler (bkz. [SaaS uygulamalarına erişimi yönetmek için Grup kullanma](../users-groups-roles/groups-saasapps.md)). Bunun yerine, sağlanması gereken kullanıcıları içeren gruplarda doğrudan atayın veya başka bir şekilde [kapsamı](define-conditional-rules-for-provisioning-user-accounts.md) yoktur.

### <a name="attribute-based-scoping"></a>Öznitelik tabanlı kapsam 

Bir uygulamaya hangi kullanıcıların sağlandığını belirleyen öznitelik tabanlı kurallar tanımlamak için kapsam filtrelerini kullanabilirsiniz. Bu yöntem, HCM uygulamalarından Azure AD 'ye gelen sağlama ve Active Directory için yaygın olarak kullanılır. Kapsam filtreleri, her bir Azure AD Kullanıcı sağlama bağlayıcısının öznitelik eşlemelerinin bir parçası olarak yapılandırılır. Öznitelik tabanlı kapsam filtrelerini yapılandırma hakkında ayrıntılı bilgi için bkz. [kapsam filtreleri Ile öznitelik tabanlı uygulama sağlama](define-conditional-rules-for-provisioning-user-accounts.md).

### <a name="b2b-guest-users"></a>B2B (konuk) kullanıcıları

Azure AD Kullanıcı sağlama hizmeti 'ni kullanarak Azure AD 'deki B2B (veya konuk) kullanıcılarını SaaS uygulamalarına sağlayabilirsiniz. Ancak, B2B kullanıcılarının Azure AD 'yi kullanarak SaaS uygulamasında oturum açması için, SaaS uygulamasının SAML tabanlı çoklu oturum açma yeteneğine belirli bir şekilde yapılandırılmış olması gerekir. SaaS uygulamalarının B2B kullanıcılarından oturum açma işlemlerini desteklemesi için nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. [B2B işbirliği Için SaaS uygulamalarını yapılandırma](../b2b/configure-saas-apps.md).

Konuk Kullanıcı için userPrincipalName genellikle "alias # EXT #" olarak depolandığını unutmayın @domain.com . userPrincipalName, öznitelik eşlemelerinizde kaynak özniteliği olarak dahil edildiğinde, #EXT # userPrincipalName öğesinden çıkarılır. #EXT # öğesinin mevcut olmasını istiyorsanız, userPrincipalName değerini kaynak öznitelik olarak originalUserPrincipalName ile değiştirin. 

## <a name="provisioning-cycles-initial-and-incremental"></a>Sağlama döngüleri: Ilk ve artımlı

Azure AD kaynak sistem olduğunda, sağlama hizmeti, kullanıcıları ve grupları izlemek için [Microsoft Graph verilerdeki değişiklikleri izlemek üzere Delta sorgusunu](https://docs.microsoft.com/graph/delta-query-overview) kullanır. Sağlama Hizmeti, kaynak sistem ve hedef sisteme karşı bir başlangıç döngüsü çalıştırır ve ardından düzenli artımlı Döngülerde çalışır.

### <a name="initial-cycle"></a>Başlangıç çevrimi

Sağlama hizmeti başlatıldığında, ilk işlem şunları yapar:

1. [Öznitelik eşlemelerinde](customize-application-attributes.md)tanımlanmış tüm öznitelikleri alarak kaynak sistemden tüm kullanıcıları ve grupları sorgulayın.

2. Yapılandırılan [atamaları](../manage-apps/assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtrelerini](define-conditional-rules-for-provisioning-user-accounts.md)kullanarak döndürülen kullanıcıları ve grupları filtreleyin.

3. Bir Kullanıcı atandığında veya sağlama kapsamında olduğunda, hizmet belirtilen [eşleşen öznitelikleri](customize-application-attributes.md#understanding-attribute-mapping-properties)kullanarak hedef sistemi eşleşen bir kullanıcı için sorgular. Örnek: Kaynak sistemdeki userPrincipal adı eşleşen özniteliktir ve hedef sistemdeki Kullanıcı adı ile eşleşiyorsa, sağlama hizmeti kaynak sistemdeki userPrincipal ad değerleriyle eşleşen kullanıcı adları için hedef sistemi sorgular.

4. Hedef sistemde eşleşen bir Kullanıcı bulunamazsa, kaynak sistemden döndürülen öznitelikler kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra, sağlama hizmeti yeni kullanıcı için hedef sistemin KIMLIĞINI algılar ve önbelleğe alır. Bu KIMLIK, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

5. Eşleşen bir kullanıcı bulunursa, kaynak sistem tarafından belirtilen öznitelikler kullanılarak güncelleştirilir. Kullanıcı hesabı eşleştirildikten sonra, sağlama hizmeti yeni kullanıcı için hedef sistemin KIMLIĞINI algılar ve önbelleğe alır. Bu KIMLIK, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

6. Öznitelik eşlemeleri "başvuru" öznitelikleri içeriyorsa, hizmet, başvurulan nesneleri oluşturmak ve bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir Kullanıcı hedef sistemde bir "Yönetici" özniteliğine sahip olabilir ve bu, hedef sistemde oluşturulan başka bir kullanıcıyla bağlantılıdır.

7. İlk döngüsünün sonunda, sonraki artımlı döngüler için başlangıç noktasını sağlayan bir filigranı kalıcı hale getirin.

ServiceNow, G Suite ve Box gibi bazı uygulamalar yalnızca kullanıcıları sağlamaktan, ayrıca grupları ve üyelerini sağlamaktan de destek vermez. Bu durumlarda, [eşlemelerde](customize-application-attributes.md)grup sağlama etkinse, sağlama hizmeti kullanıcıları ve grupları eşitler ve daha sonra grup üyeliklerini eşitler.

### <a name="incremental-cycles"></a>Artımlı döngüler

İlk döngüden sonra diğer tüm döngüler şunları yapar:

1. Son eşik depolanmasından bu yana güncelleştirilmiş olan tüm kullanıcılar ve gruplar için kaynak sistemi sorgulayın.

2. Yapılandırılan [atamaları](../manage-apps/assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtrelerini](define-conditional-rules-for-provisioning-user-accounts.md)kullanarak döndürülen kullanıcıları ve grupları filtreleyin.

3. Bir Kullanıcı atandığında veya sağlama kapsamında olduğunda, hizmet belirtilen [eşleşen öznitelikleri](customize-application-attributes.md#understanding-attribute-mapping-properties)kullanarak hedef sistemi eşleşen bir kullanıcı için sorgular.

4. Hedef sistemde eşleşen bir Kullanıcı bulunamazsa, kaynak sistemden döndürülen öznitelikler kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra, sağlama hizmeti yeni kullanıcı için hedef sistemin KIMLIĞINI algılar ve önbelleğe alır. Bu KIMLIK, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

5. Eşleşen bir kullanıcı bulunursa, kaynak sistem tarafından belirtilen öznitelikler kullanılarak güncelleştirilir. Bu, eşleşen yeni bir hesap ise, sağlama hizmeti yeni kullanıcı için hedef sistemin KIMLIĞINI algılar ve önbelleğe alır. Bu KIMLIK, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

6. Öznitelik eşlemeleri "başvuru" öznitelikleri içeriyorsa, hizmet, başvurulan nesneleri oluşturmak ve bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir Kullanıcı hedef sistemde bir "Yönetici" özniteliğine sahip olabilir ve bu, hedef sistemde oluşturulan başka bir kullanıcıyla bağlantılıdır.

7. Önceden sağlama kapsamında olan bir Kullanıcı, atanmamış olarak da dahil olmak üzere kapsamdan kaldırılırsa, hizmet, hedef sistemdeki kullanıcıyı bir güncelleştirme aracılığıyla devre dışı bırakır.

8. Kaynak sistemde daha önce sağlama kapsamında olan bir kullanıcı devre dışıysa veya geçici olarak silinirse, hizmet, hedef sistemdeki kullanıcıyı bir güncelleştirme aracılığıyla devre dışı bırakır.

9. Daha önce sağlama kapsamında olan bir Kullanıcı kaynak sistemde sabit bir şekilde silinirse, hizmet kullanıcıyı hedef sistemde siler. Azure AD 'de, kullanıcılar, geçici olarak silindikten sonra 30 gün sonra kalıcı olarak silinir.

10. Daha sonraki artımlı döngüler için başlangıç noktasını sağlayan artımlı döngüsünün sonunda yeni bir filigranı kalıcı hale getirin.

> [!NOTE]
> İsteğe bağlı olarak, [eşlemeler](customize-application-attributes.md) bölümündeki **hedef nesne eylemleri** onay kutularını kullanarak **oluşturma**, **güncelleştirme**veya **silme** işlemlerini devre dışı bırakabilirsiniz. Bir güncelleştirme sırasında kullanıcıyı devre dışı bırakma mantığı, "accountEnabled" gibi bir alandan bir öznitelik eşlemesi aracılığıyla da denetlenir.

Sağlama Hizmeti, [her uygulamaya özgü öğreticide](../saas-apps/tutorial-list.md)tanımlanan aralıklarda sonsuza kadar sürekli artımlı Döngülerde çalışmaya devam eder. Artımlı döngüler aşağıdaki olaylardan biri gerçekleşene kadar devam eder:

- Hizmet, Azure portal kullanılarak veya uygun Microsoft Graph API komutu kullanılarak el ile durdurulur.
- Yeni bir başlangıç çevrimi, Azure portal **durumu temizle ve yeniden Başlat** seçeneği kullanılarak veya uygun Microsoft Graph API komutu kullanılarak tetiklenir. Bu eylem tüm depolanmış filigranı temizler ve tüm kaynak nesnelerinin yeniden değerlendirilmesini sağlar.
- Öznitelik eşlemelerinde veya kapsam filtrelerinde değişiklik nedeniyle yeni bir başlangıç çevrimi tetiklenir. Bu eylem Ayrıca, depolanan tüm filigranı temizler ve tüm kaynak nesnelerinin yeniden değerlendirilmesini sağlar.
- Sağlama işlemi, yüksek bir hata oranı nedeniyle karantinaya alınır (aşağıya bakın) ve dört haftadan uzun bir şekilde karantinayla kalır. Bu olay, hizmet otomatik olarak devre dışı bırakılır.

### <a name="errors-and-retries"></a>Hatalar ve yeniden denemeler

Hedef sistemdeki bir hata, hedef sistemde tek bir kullanıcının eklenmesini, güncelleştirilmesini veya silinmesini engelliyorsa, işlem sonraki eşitleme döngüsüne yeniden denenir. Kullanıcı başarısız olmaya devam ederse, yeniden denemeler daha düşük bir sıklıkta gerçekleşecek ve aşamalı olarak günde yalnızca bir denemeye geri dönebilir. Sorunu çözmek için Yöneticiler, temel nedeni tespit etmek ve uygun eylemi gerçekleştirmek için [sağlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) denetlemelidir. Yaygın hatalarda şunlar olabilir:

- Hedef sistemde gerekli olan kaynak sistemde doldurulmuş bir özniteliği olmayan kullanıcılar
- Kaynak sistemde, hedef sistemde benzersiz bir kısıtlama olan ve aynı değer başka bir Kullanıcı kaydında bulunan bir öznitelik değeri olan kullanıcılar

Kaynak sistemdeki etkilenen kullanıcının öznitelik değerlerini ayarlayarak veya öznitelik eşlemelerini çakışmalara neden olmayan şekilde ayarlayarak bu sorunları çözün.

### <a name="quarantine"></a>Karantina

Hedef sisteme karşı gerçekleştirilen çağrıların çoğu veya hepsi bir hata nedeniyle (örneğin, geçersiz yönetici kimlik bilgileri) başarısız olursa, sağlama işi "Karantina" durumuna geçer. Bu durum, [sağlama Özeti raporunda](../manage-apps/check-status-user-account-provisioning.md) ve Azure Portal e-posta bildirimleri yapılandırılmışsa e-postayla belirtilir.

Karantinaya alma sırasında, artımlı döngülerin sıklığı günde bir kez yavaş şekilde azaltılır.

Tüm sorunlu hatalar düzeltildikten ve sonraki eşitleme döngüsünün başlaması durumunda sağlama işi karantinadan çıkar. Sağlama işi dört haftadan uzun bir süreyle karantinayla kalırsa, sağlama işi devre dışı bırakılır. [Burada karantina durumu hakkında](../manage-apps/application-provisioning-quarantine-status.md)daha fazla bilgi edinin.

### <a name="how-long-provisioning-takes"></a>Sağlamanın tamamlanma süresi

Performans, sağlama işinizin ilk sağlama döngüsünü mi yoksa artımlı bir döngüyü mi çalıştırdığına bağlıdır. Sağlama süresinin ne kadar süreceği ve sağlama hizmetinin durumunun nasıl izleneceği hakkında ayrıntılar için bkz. [Kullanıcı sağlama durumunu denetleme](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md).

### <a name="how-to-tell-if-users-are-being-provisioned-properly"></a>Kullanıcıların düzgün şekilde sağlandığını söylemek

Kullanıcı sağlama hizmeti tarafından çalıştırılan tüm işlemler, Azure AD [sağlama günlüklerine (Önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)kaydedilir. Günlükler, kaynak ve hedef sistemlere yapılan tüm okuma ve yazma işlemlerini ve her işlem sırasında okunan veya yazılan kullanıcı verilerini içerir. Azure portal sağlama günlüklerini okuma hakkında daha fazla bilgi için bkz. [sağlama Raporlama Kılavuzu](../manage-apps/check-status-user-account-provisioning.md).

## <a name="de-provisioning"></a>Serbest sağlama

Azure AD sağlama hizmeti, kullanıcıların artık erişim sahibi olmaması gerektiğinde, kaynak ve hedef sistemleri, hesapları serbest bırakma ile eşitlenmiş halde tutar. 

Azure AD sağlama hizmeti, uygulama geçici silmeleri desteklediğinde (etkin = false ile güncelleştirme isteği) ve aşağıdaki olaylardan herhangi biri gerçekleştiğinde, bir uygulamadaki kullanıcıyı geçici olarak siler:

* Kullanıcı hesabı Azure AD 'de silinir
*   Kullanıcı, uygulamadan atanmamış
*   Kullanıcı artık kapsam filtresini karşılamadığı ve kapsam dışına çıkış
    * Varsayılan olarak, Azure AD sağlama hizmeti, kapsam dışına çıkan kullanıcıları geçici olarak siler veya devre dışı bırakır. Bu varsayılan davranışı geçersiz kılmak istiyorsanız, [kapsam dışı silme işlemlerini atlamak](../app-provisioning/skip-out-of-scope-deletions.md)için bir bayrak ayarlayabilirsiniz.
*   AccountEnabled özelliği false olarak ayarlandı

Yukarıdaki dört olaydan biri meydana gelirse ve hedef uygulama geçici silme işlemini desteklemiyorsa, sağlama hizmeti kullanıcıyı uygulamadan kalıcı olarak silmek için bir SILME isteği gönderir. 

Azure AD 'de bir Kullanıcı silindikten 30 gün sonra, bu kullanıcılar kiracıdan kalıcı olarak silinir. Bu noktada, sağlama hizmeti uygulamada kullanıcıyı kalıcı olarak silmek için bir SILME isteği gönderir. 30 günlük pencerede herhangi bir zamanda, uygulamayı [kalıcı olarak el ile silebilirsiniz](../fundamentals/active-directory-users-restore.md). Bu, uygulamaya bir silme isteği gönderir.

Öznitelik eşlemelerinizde bir öznitelik ıssoftdeleted görürseniz, kullanıcının durumunu ve etkin = false olan bir güncelleştirme isteğinin Kullanıcı tarafından geçici olarak silinmesini sağlamak için kullanılır. 

## <a name="next-steps"></a>Sonraki Adımlar

[Otomatik kullanıcı sağlama dağıtımı planlama](../app-provisioning/plan-auto-user-provisioning.md)

[Galeri uygulaması için sağlamayı yapılandırma](../manage-apps/configure-automatic-user-provisioning-portal.md)

[Kendi uygulamanızı oluştururken bir SCıM uç noktası oluşturun ve sağlamayı yapılandırın](../app-provisioning/use-scim-to-provision-users-and-groups.md)

[Kullanıcılara yapılandırma ve uygulama sağlama ile ilgili sorunları giderin](../manage-apps/application-provisioning-config-problem.md).
