---
title: Azure AD sağlamanın nasıl çalıştığını anlama | Microsoft Dokümanlar
description: Azure AD sağlamanın nasıl çalıştığını anlama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/10/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 241d90981ed9ba54d253e6c22c00f9e5a9197863
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80884894"
---
# <a name="how-provisioning-works"></a>Sağlama nasıl çalışır?

Otomatik sağlama, kullanıcıların erişilmesi gereken bulut uygulamalarında kullanıcı kimlikleri ve rolleri oluşturma anlamına gelir. Kullanıcı kimlikleri oluşturmaya ek olarak, otomatik sağlama, durum veya roller değiştikçe kullanıcı kimliklerinin bakımını ve kaldırılmasını içerir. Dağıtıma başlamadan önce, Azure AD hükmünün nasıl çalıştığını öğrenmek ve yapılandırma önerileri almak için bu makaleyi gözden geçirebilirsiniz. 

**Azure AD Sağlama Hizmeti,** uygulama satıcısı tarafından sağlanan Bir Etki Alanı Arası Kimlik Yönetimi Sistemi (SCIM) 2.0 kullanıcı yönetimi API bitiş noktasına bağlanarak kullanıcıları SaaS uygulamalarına ve diğer sistemlere karşı sağlar. Bu SCIM bitiş noktası, Azure AD'nin kullanıcıları programlı bir şekilde oluşturmasına, güncelleştirmesine ve kaldırmasına olanak tanır. Seçili uygulamalar için, sağlama hizmeti, gruplar ve roller gibi kimlikle ilgili ek nesneler oluşturabilir, güncelleyebilir ve kaldırabilir. Azure AD ile uygulama arasında sağlama için kullanılan kanal, HTTPS TLS şifrelemesi kullanılarak şifrelenir.


![Azure AD Sağlama](./media/how-provisioning-works/provisioning0.PNG)
Hizmeti*Şekil 1: Azure REKLAM Sağlama Hizmeti*

![Giden kullanıcı sağlama iş](./media/how-provisioning-works/provisioning1.PNG)
akışı*Şekil 2: Azure AD'den popüler SaaS uygulamalarına iş akışı sağlayan "Giden" kullanıcı*

![Gelen kullanıcı sağlama iş](./media/how-provisioning-works/provisioning2.PNG)
akışı*Şekil 3: Popüler İnsan Sermayesi Yönetimi (HCM) uygulamalarından Azure Active Directory ve Windows Server Active Directory'ye "Gelen" kullanıcı sağlama iş akışı*

## <a name="provisioning-using-scim-20"></a>SCIM 2.0 kullanarak sağlama

Azure AD sağlama hizmeti, otomatik sağlama için [SCIM 2.0 protokolünü](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/bg-p/IdentityStandards) kullanır. Hizmet, uygulama için SCIM bitiş noktasına bağlanır ve kullanıcıların ve grupların sağlanmasını ve bunların sağlanmasını otomatikleştirmek için SCIM kullanıcı nesnesi şeması ve REST API'lerini kullanır. Azure AD galerisindeki çoğu uygulama için SCIM tabanlı bir sağlama bağlayıcısı sağlanır. Geliştiriciler, Azure AD için uygulamalar oluştururken, sağlaması için Azure AD'yi tümleştiren bir SCIM bitiş noktası oluşturmak için SCIM 2.0 kullanıcı yönetimi API'sini kullanabilir. Ayrıntılar için, [bkz.](../app-provisioning/use-scim-to-provision-users-and-groups.md)

Şu anda sahip olmayan bir uygulama için otomatik Azure AD sağlama bağlayıcısı istemek için bir [Azure Etkin Dizin Uygulama İsteği](https://aka.ms/aadapprequest)doldurun.

## <a name="authorization"></a>Yetkilendirme

Azure AD'nin uygulamanın kullanıcı yönetimi API'sine bağlanması için kimlik bilgileri gereklidir. Bir uygulama için otomatik kullanıcı sağlama yapılandırmayaparken, geçerli kimlik bilgileri girmeniz gerekir. Uygulama öğreticisine atıfta bulunarak uygulama için kimlik bilgisi türlerini ve gereksinimlerini bulabilirsiniz. Azure portalında, Azure AD'nin sağlanan kimlik bilgilerini kullanarak uygulamanın sağlama uygulamasına bağlanmaya çalışmasıiçin kimlik bilgilerini sınayabilirsiniz.

SamL tabanlı tek oturum açma da uygulama için yapılandırılırsa, Azure AD'nin dahili, uygulama başına depolama sınırı 1024 bayttır. Bu sınır, bir uygulamanın tek bir örneğiyle ilişkili tüm sertifikaları, gizli belirteçleri, kimlik bilgilerini ve ilgili yapılandırma verilerini (Azure AD'de hizmet temel kaydı olarak da bilinir) içerir. SAML tabanlı tek oturum açma yapılandırıldığınızda, SAML belirteçlerini imzalamak için kullanılan sertifika genellikle alanın %50'sinden fazlasını tüketir. Kullanıcı sağlama kurulumu sırasında girdiğiniz ek öğeler (gizli belirteçler, URL'ler, bildirim e-posta adresleri, kullanıcı adları ve parolalar) depolama sınırını aşabilir. Daha fazla bilgi için, [kullanıcı sağlama yı yapılandırırken sorun kaydetme yönetici kimlik bilgilerine](../manage-apps/application-provisioning-config-problem-storage-limit.md)bakın.

## <a name="mapping-attributes"></a>Eşleme öznitelikleri

Üçüncü taraf bir SaaS uygulaması için kullanıcı sağlamayı etkinleştirdiğinizde, Azure portalı öznitelik eşlemeleri aracılığıyla öznitelik değerlerini denetler. Eşlemeler, kullanıcı hesapları sağlandığında veya güncelleştirildiğinde Azure AD ile hedef uygulama arasında akan kullanıcı özniteliklerini belirler.

Azure AD kullanıcı nesneleri ile her SaaS uygulamasının kullanıcı nesneleri arasında önceden yapılandırılmış öznitelikler ve öznitelik eşlemeleri kümesi vardır. Bazı uygulamalar, Kullanıcılar la birlikte Gruplar gibi diğer nesne türlerini yönetir.

Sağlama yı ayarlarken, Azure AD'den uygulamaya hangi kullanıcı (veya grup) özelliklerinin aktığını tanımlayan öznitelik eşlemelerini ve iş akışlarını gözden geçirmek ve yapılandırmak önemlidir. İki sistem arasındaki kullanıcıları/grupları benzersiz olarak tanımlamak ve eşleştirmek için kullanılan eşleşen özelliği **(Bu özniteliği kullanarak**eşleştirin) gözden geçirin ve yapılandırın.

Varsayılan öznitelik eşlemelerini işletme gereksinimlerinize göre özelleştirebilirsiniz. Bu nedenle, varolan öznitelik eşlemelerini değiştirebilir veya silebilir veya yeni öznitelik eşlemeleri oluşturabilirsiniz. Ayrıntılar için, [SaaS uygulamaları için kullanıcı sağlama öznitelik eşlemelerini özelleştirme konusuna](../manage-apps/customize-application-attributes.md)bakın.

Bir SaaS uygulamasına sağlama yı yapılandırdığınızda, belirtebileceğiniz öznitelik eşlemetürtürtürtür. Bu eşlemeler için, kullanıcılarınızın verilerini SaaS uygulaması için daha kabul edilebilir biçimlere dönüştürmenize olanak tanıyan komut dosyası na benzer bir ifade yazmanız gerekir. Ayrıntılar [için, öznitelik eşlemeleri için ifadeleri yazma'ya](functions-for-customizing-application-data.md)bakın.

## <a name="scoping"></a>Kapsam 
### <a name="assignment-based-scoping"></a>Atama tabanlı kapsam

Azure AD'den Bir SaaS uygulamasına giden sağlama için, [kullanıcı veya grup atamalarına](../manage-apps/assign-user-or-group-access-portal.md) güvenmek, hangi kullanıcıların sağlama kapsamında olduğunu belirlemenin en yaygın yoludur. Kullanıcı atamaları tek oturum açma sağlamak için de kullanıldığından, aynı yöntem hem erişimi hem de sağlamayı yönetmek için kullanılabilir. Atama tabanlı kapsam, İş Günü ve Başarı Faktörleri gibi gelen sağlama senaryoları için geçerli değildir.

* **Grup.** Azure AD Premium lisans planıyla, grupları bir SaaS uygulamasına erişim atamak için kullanabilirsiniz. Daha sonra, sağlama kapsamı **yalnızca atanan kullanıcıları ve grupları Eşitle**olarak ayarlandığında, Azure AD sağlama hizmeti, kullanıcılara uygulamaya atanan bir grubun üyesi olup olmadıklarına bağlı olarak sağlama veya sağlamadan kaldırma hizmeti sağlar. Uygulama grup nesnelerini desteklemedikçe grup nesnesinin kendisi sağlanmış değildir. Uygulamanıza atanan grupların "SecurityEnabled" özelliğine sahip olduğundan emin olun.

* **Dinamik gruplar.** Azure AD kullanıcı sağlama hizmeti, kullanıcıları [dinamik gruplar](../users-groups-roles/groups-create-rule.md)halinde okuyabilir ve sağlayabilir. Bu uyarılar ve öneriler akılda tutun:

  * Dinamik gruplar, Azure AD'den SaaS uygulamalarına uçtan uca sağlama performansını etkileyebilir.

  * Dinamik bir gruptaki bir kullanıcının Bir SaaS uygulamasında ne kadar hızlı sağlanmış veya de-provisioned dinamik grup üyelik değişiklikleri değerlendirebilirsiniz bağlıdır. Dinamik bir grubun işlem durumunu nasıl denetleyecekleri hakkında bilgi [için, üyelik kuralı için işleme durumunu](../users-groups-roles/groups-create-rule.md)denetle'ye bakın.

  * Bir kullanıcı dinamik grupta üyeliğini kaybettiğinde, bu bir de-provisioning olayı olarak kabul edilir. Dinamik gruplar için kurallar oluştururken bu senaryoyu göz önünde bulundurun.

* **İç içe gruplar.** Azure AD kullanıcı sağlama hizmeti, iç içe olan gruplardaki kullanıcıları okuyamaz veya sağalamaz. Hizmet yalnızca açıkça atanmış bir grubun hemen üyeleri olan kullanıcıları okuyabilir ve sağlayabilir. "Uygulamalara grup tabanlı atamalar" sınırlaması, tek oturum açmayı da etkiler (bkz. [SaaS uygulamalarına erişimi yönetmek için bir grup kullanma).](../users-groups-roles/groups-saasapps.md) Bunun yerine, sağlanması gereken kullanıcıları içeren gruplara doğrudan atama veya başka bir [kapsam.](define-conditional-rules-for-provisioning-user-accounts.md)

### <a name="attribute-based-scoping"></a>Öznitelik tabanlı kapsam 

Bir uygulamaya hangi kullanıcıların sağlanmış olduğunu belirleyen öznitelik tabanlı kuralları tanımlamak için kapsam filtreleri kullanabilirsiniz. Bu yöntem genellikle HCM uygulamalarından Azure AD ve Active Directory'ye gelen sağlama için kullanılır. Kapsam filtreleri, her Azure AD kullanıcı sağlama bağlayıcısı için öznitelik eşlemelerinin bir parçası olarak yapılandırılır. Öznitelik tabanlı kapsam filtreleri yapılandırma hakkında ayrıntılar için, [kapsam filtreleri ile Öznitelik tabanlı uygulama sağlama](define-conditional-rules-for-provisioning-user-accounts.md)bakın.

### <a name="b2b-guest-users"></a>B2B (konuk) kullanıcıları

Azure AD kullanıcı sağlama hizmetini, Azure AD'deki B2B (veya konuk) kullanıcılarını SaaS uygulamalarına sağlamak için kullanmak mümkündür. Ancak, B2B kullanıcılarının Azure AD kullanarak SaaS uygulamasında oturum açabilmesi için, SaaS uygulamasının SAML tabanlı tek oturum açma özelliğinin belirli bir şekilde yapılandırılmış olması gerekir. B2B kullanıcılarının oturum açma işlemlerini destekleyecek Şekilde SaaS uygulamalarının nasıl yapılandırılabildiğini hakkında daha fazla bilgi [için B2B işbirliği için SaaS uygulamalarını yapılandırın.](../b2b/configure-saas-apps.md)

Konuk kullanıcı için userPrincipalName'nin genellikle "diğer ad#EXT#@domain.com" olarak depoladığını unutmayın. userPrincipalName kaynak özniteliği olarak öznitelik eşlemelerinize eklendiğinde, #EXT# userPrincipalName'den çıkarılır. #EXT#'nın bulunmasını istiyorsanız, kaynak özniteliği olarak userPrincipalName'i originalUserPrincipalName ile değiştirin. 

## <a name="provisioning-cycles-initial-and-incremental"></a>Sağlama döngüleri: Başlangıç ve artımlı

Azure AD kaynak sistem olduğunda, sağlama hizmeti kullanıcıları ve grupları izlemek [için Microsoft Graph verilerindeki değişiklikleri izlemek için Kullanım deltası sorgusunu](https://docs.microsoft.com/graph/delta-query-overview) kullanır. Sağlama hizmeti, kaynak sistem ve hedef sisteme karşı bir başlangıç döngüsü ve ardından periyodik artımlı döngüler çalışır.

### <a name="initial-cycle"></a>İlk döngü

Sağlama hizmeti başlatıldığında, ilk döngü aşağıdakileri yapacaktır:

1. [Öznitelik eşlemelerinde](customize-application-attributes.md)tanımlanan tüm öznitelikleri alınarak kaynak sistemden tüm kullanıcıları ve grupları sorgulayın.

2. Yapılandırılan [atamaları](../manage-apps/assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)kullanarak döndürülen kullanıcıları ve grupları filtreleyin.

3. Bir kullanıcı atandığında veya sağlama kapsamında olduğunda, hizmet belirtilen [eşleşen öznitelikleri](customize-application-attributes.md#understanding-attribute-mapping-properties)kullanarak eşleşen bir kullanıcı için hedef sistemi sorgular. Örnek: Kaynak sistemdeki kullanıcıPrincipal adı, hedef sistemde kullanıcı Adı ile eşleşen öznitelik ve eşlemler ise, sağlama hizmeti kaynak sistemdeki kullanıcıAna ad değerleriyle eşleşen kullanıcı Adları için hedef sistemi sorgular.

4. Eşleşen bir kullanıcı hedef sistemde bulunmazsa, kaynak sistemden döndürülen öznitelikler kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra, sağlama hizmeti yeni kullanıcı için hedef sistemin kimliğini algılar ve önbelleğe dardır. Bu kimlik, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

5. Eşleşen bir kullanıcı bulunursa, kaynak sistem tarafından sağlanan öznitelikler kullanılarak güncelleştirilir. Kullanıcı hesabı eşleştikten sonra, sağlama hizmeti yeni kullanıcı için hedef sistemin kimliğini algılar ve önbelleğe dardır. Bu kimlik, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

6. Öznitelik eşlemeleri "başvuru" öznitelikleri içeriyorsa, hizmet başvurulan nesneleri oluşturmak ve bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir kullanıcıhedef sistemde oluşturulan başka bir kullanıcıya bağlı bir "Yönetici" özniteliğine sahip olabilir.

7. Daha sonraki artımlı döngüler için başlangıç noktası sağlayan ilk çevrimin sonunda bir filigran devam edin.

ServiceNow, G Suite ve Box gibi bazı uygulamalar yalnızca kullanıcıları sağlamanın yanı sıra grupları ve üyelerini de destekler. Bu gibi durumlarda, [eşlemelerde](customize-application-attributes.md)grup sağlama etkinse, sağlama hizmeti kullanıcıları ve grupları eşitler ve daha sonra grup üyeliklerini eşitler.

### <a name="incremental-cycles"></a>Artımlı döngüler

İlk döngüden sonra, diğer tüm döngüler:

1. Son filigran depolandığından beri güncelleştirilen kullanıcılar ve gruplar için kaynak sistemi sorgulayın.

2. Yapılandırılan [atamaları](../manage-apps/assign-user-or-group-access-portal.md) veya [öznitelik tabanlı kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)kullanarak döndürülen kullanıcıları ve grupları filtreleyin.

3. Bir kullanıcı atandığında veya sağlama kapsamında olduğunda, hizmet belirtilen [eşleşen öznitelikleri](customize-application-attributes.md#understanding-attribute-mapping-properties)kullanarak eşleşen bir kullanıcı için hedef sistemi sorgular.

4. Eşleşen bir kullanıcı hedef sistemde bulunmazsa, kaynak sistemden döndürülen öznitelikler kullanılarak oluşturulur. Kullanıcı hesabı oluşturulduktan sonra, sağlama hizmeti yeni kullanıcı için hedef sistemin kimliğini algılar ve önbelleğe dardır. Bu kimlik, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

5. Eşleşen bir kullanıcı bulunursa, kaynak sistem tarafından sağlanan öznitelikler kullanılarak güncelleştirilir. Eşleşen yeni atanan bir hesapsa, sağlama hizmeti yeni kullanıcı için hedef sistemin kimliğini algılar ve önbelleğe darda kalır. Bu kimlik, bu kullanıcı üzerinde gelecekteki tüm işlemleri çalıştırmak için kullanılır.

6. Öznitelik eşlemeleri "başvuru" öznitelikleri içeriyorsa, hizmet başvurulan nesneleri oluşturmak ve bağlamak için hedef sistemde ek güncelleştirmeler yapar. Örneğin, bir kullanıcıhedef sistemde oluşturulan başka bir kullanıcıya bağlı bir "Yönetici" özniteliğine sahip olabilir.

7. Daha önce sağlama kapsamında olan bir kullanıcı atanmamış olmak da dahil olmak üzere kapsamdan kaldırılırsa, hizmet bir güncelleştirme aracılığıyla hedef sistemdeki kullanıcıyı devre dışı kılabilir.

8. Daha önce sağlama kapsamında olan bir kullanıcı kaynak sistemde devre dışı bırakılmış veya silinmişse, hizmet bir güncelleştirme aracılığıyla hedef sistemdeki kullanıcıyı devre dışı eder.

9. Daha önce sağlama kapsamında olan bir kullanıcı kaynak sistemde zor silinirse, hizmet hedef sistemdeki kullanıcıyı siler. Azure AD'de, kullanıcılar yumuşak bir şekilde silindikten 30 gün sonra sabit silinir.

10. Artımlı döngünün sonunda yeni bir filigran devam edin, hangi sonraki artımlı döngüleri için başlangıç noktası sağlar.

> [!NOTE]
> [Eşlemeler](customize-application-attributes.md) bölümündehedef **nesne eylemleri** onay kutularını kullanarak **Oluşturma,** **Güncelleştir**veya **Sil** işlemlerini isteğe bağlı olarak devre dışı kullanabilirsiniz. Güncelleştirme sırasında bir kullanıcıyı devre dışı bıyıltırmanın mantığı da "accountEnabled" gibi bir alandan bir öznitelik eşleme yoluyla denetlenir.

Sağlama hizmeti, [her uygulamaya özgü öğreticide](../saas-apps/tutorial-list.md)tanımlanan aralıklarla, arka arkaya artımlı döngüleri süresiz olarak çalıştırmaya devam eder. Artımlı döngüler aşağıdaki olaylardan biri meydana gelene kadar devam eder:

- Hizmet, Azure portalı kullanılarak veya uygun Microsoft Graph API komutunu kullanarak el ile durdurulur.
- Azure portalındaki **Açıkla durumu ve yeniden başlatma** seçeneği veya uygun Microsoft Graph API komutu kullanılarak yeni bir başlangıç döngüsü tetiklenir. Bu eylem, depolanan filigranı temizler ve tüm kaynak nesnelerin yeniden değerlendirilmesine neden olur.
- Öznitelik eşlemeleri veya kapsam filtreleri bir değişiklik nedeniyle yeni bir ilk döngü tetiklenir. Bu eylem ayrıca depolanan filigranı temizler ve tüm kaynak nesnelerin yeniden değerlendirilmesine neden olur.
- Sağlama işlemi yüksek hata oranı nedeniyle karantinaya girer (aşağıya bakın) ve dört haftadan fazla karantinada kalır. Bu durumda, hizmet otomatik olarak devre dışı bırakılır.

### <a name="errors-and-retries"></a>Hatalar ve yeniden denemeler

Hedef sistemdeki bir hata, tek bir kullanıcının hedef sistemde eklenmesini, güncellemesini veya silinmesini engellerse, işlem bir sonraki eşitleme döngüsünde yeniden denenir. Kullanıcı başarısız olmaya devam ederse, yeniden denemeler daha düşük bir frekansta oluşmaya başlar ve yavaş yavaş günde yalnızca bir denemeyle yeniden ölçeklenebilir. Hatayı gidermek için, yöneticilerin temel nedeni belirlemek ve uygun eylemi yapmak için [sağlama günlüklerini denetlemeleri](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context) gerekir. Sık karşılaşılan hatalar şunları içerebilir:

- Hedef sistemde gerekli olan kaynak sistemde doldurulan bir özniteliğe sahip olmayan kullanıcılar
- Kaynak sistemde öznitelik değeri olan ve hedef sistemde benzersiz bir kısıtlama bulunan ve aynı değer başka bir kullanıcı kaydında bulunan kullanıcılar

Bu hataları, kaynak sistemde etkilenen kullanıcının öznitelik değerlerini ayarlayarak veya öznitelik eşlemelerini çakışmalara neden olmayacak şekilde ayarlayarak giderin.

### <a name="quarantine"></a>Karantina

Hedef sisteme karşı yapılan çağrıların çoğu veya tümü bir hata nedeniyle sürekli olarak başarısız olursa (örneğin geçersiz yönetici kimlik bilgileri) sağlama işi bir "karantina" durumuna girer. Bu durum, e-posta bildirimleri Azure portalında yapılandırıldıysa, [sağlayan özet raporunda](../manage-apps/check-status-user-account-provisioning.md) ve e-posta yoluyla belirtilir.

Karantinadayken, artımlı döngülerin sıklığı yavaş yavaş günde bir kez azalır.

Tüm kusurlu hatalar düzeltildikten ve bir sonraki eşitleme döngüsü başladıktan sonra, sağlama işi karantinadan çıkar. Sağlama işi dört haftadan fazla karantinada kalırsa, sağlama işi devre dışı bırakılır. Burada karantina durumu hakkında daha fazla bilgi [edinin.](../manage-apps/application-provisioning-quarantine-status.md)

### <a name="how-long-provisioning-takes"></a>Sağlamanın tamamlanma süresi

Performans, sağlama işinizin başlangıç döngüsü mü yoksa artımlı bir döngü mü çalıştırdığına bağlıdır. Sağlamanın ne kadar süreceği ve sağlama hizmetinin durumunun nasıl izlendiği hakkında ayrıntılı bilgi [için](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md)bkz.

### <a name="how-to-tell-if-users-are-being-provisioned-properly"></a>Kullanıcıların doğru şekilde sağlandığını nasıl anlaylayama

Kullanıcı sağlama hizmeti tarafından yürütülen tüm işlemler Azure AD [Sağlama günlüklerine (önizleme)](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)kaydedilir. Günlükler, kaynak ve hedef sistemlere yapılan tüm okuma ve yazma işlemlerini ve her işlem sırasında okunan veya yazılan kullanıcı verilerini içerir. Azure portalındaki sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi [için, sağlama raporlama kılavuzuna](../manage-apps/check-status-user-account-provisioning.md)bakın.

## <a name="de-provisioning"></a>De-provisioning

Azure AD sağlama hizmeti, kullanıcıların artık erişimi olmaması gereken hesapları sağlamadan kaynak ve hedef sistemleri eşit tutar. 

Azure AD sağlama hizmeti, uygulama yumuşak silme (etkin = yanlış ile güncelleştirme isteği) ve aşağıdaki olaylardan herhangi biri oluştuğunda, bir uygulamadaki bir kullanıcıyı yumuşak bir şekilde siler:

* Kullanıcı hesabı Azure AD'de silinir
*   Kullanıcı uygulamadan atanmamış
*   Kullanıcı artık bir kapsam filtresiyle karşılaşmadı ve kapsam dışına çıkıyor
    * Varsayılan olarak, Azure AD sağlama hizmeti kapsam dışına çıkan kullanıcıları siler veya devre dışı kılabilir. Bu varsayılan davranışı geçersiz kılmak istiyorsanız, [kapsam dışı silmeleri atlayacak](../app-provisioning/skip-out-of-scope-deletions.md)bir bayrak ayarlayabilirsiniz.
*   AccountEnabled özelliği False olarak ayarlanır

Yukarıdaki dört olaydan biri oluşursa ve hedef uygulama yumuşak silmeleri desteklemiyorsa, sağlama hizmeti kullanıcıyı uygulamadan kalıcı olarak silmek için bir DELETE isteği gönderir. 

Bir kullanıcı Azure AD'de silindikten 30 gün sonra, kullanıcı kiracıdan kalıcı olarak silinir. Bu noktada, sağlama hizmeti, uygulamadaki kullanıcıyı kalıcı olarak silmek için bir DELETE isteği gönderir. 30 günlük süre boyunca istediğiniz zaman, bir [kullanıcıyı kalıcı olarak el ile silebilirsiniz](../fundamentals/active-directory-users-restore.md)ve bu da uygulamaya silme isteği gönderir.

Öznitelik eşlemelerinizde Bir öznitelik IsSoftDeleted görürseniz, kullanıcının durumunu belirlemek ve kullanıcıyı yumuşak silmek için active = false içeren bir güncelleştirme isteği gönderip göndermemeniz için kullanılır. 

## <a name="next-steps"></a>Sonraki Adımlar

[Otomatik kullanıcı sağlama dağıtımı planlama](../app-provisioning/plan-auto-user-provisioning.md)

[Galeri uygulaması için sağlamayı yapılandırma](../manage-apps/configure-automatic-user-provisioning-portal.md)

[Bir SCIM bitiş noktası oluşturun ve kendi uygulamanızı oluştururken sağlamayı yapılandırın](../app-provisioning/use-scim-to-provision-users-and-groups.md)

[Kullanıcıları bir uygulamaya yapılandırma ve sağlama yla ilgili sorunları giderin.](../manage-apps/application-provisioning-config-problem.md)
