---
title: AD FS uygulamalarını Azure Etkin Dizini'ne taşımak için etkinlik raporunu kullanın | Microsoft Dokümanlar'
description: Active Directory Federation Services (AD FS) uygulama etkinlik raporu, uygulamaları AD FS'den Azure Etkin Dizini'ne (Azure AD) hızla geçirmenize olanak tanır. AD FS için bu geçiş aracı Azure AD ile uyumluluğu tanımlar ve geçiş kılavuzu verir.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/14/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 333e440fdd5f5062dda45fb12a83543c63e66c04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978027"
---
# <a name="use-the-ad-fs-application-activity-report-preview-to-migrate-applications-to-azure-ad"></a>Uygulamaları Azure AD'ye geçirmek için AD FS uygulama etkinlik raporunu (önizleme) kullanma

Birçok kuruluş, bulut uygulamalarına tek oturum açma sağlamak için Active Directory Federation Services (AD FS) kullanır. AD FS uygulamalarınızı, özellikle maliyet yönetimi, risk yönetimi, üretkenlik, uyumluluk ve yönetim açısından kimlik doğrulama için Azure AD'ye taşımanın önemli avantajları vardır. Ancak hangi uygulamaların Azure AD ile uyumlu olduğunu anlamak ve belirli geçiş adımlarını belirlemek zaman alabilir.

Azure portalındaki AD FS uygulama etkinlik raporu (önizleme), hangi uygulamalarınızın Azure AD'ye geçirilme kapasitesine sahip olduğunu hızlı bir şekilde belirlemenize olanak tanır. Azure AD ile uyumluluk için tüm AD FS uygulamalarını değerlendirir, herhangi bir sorunu denetler ve geçiş için tek tek uygulamalar hazırlama konusunda rehberlik sağlar. AD FS uygulama faaliyet raporu ile şunları yapabilirsiniz:

* **AD FS uygulamalarını keşfedin ve geçişinizi kapsamınıza göre.** AD FS uygulama etkinliği raporu, kuruluşunuzdaki tüm AD FS uygulamalarını listeler ve Azure AD'ye geçişe hazır olduklarını gösterir.
* **Geçiş uygulamaları için öncelik verin.** Uygulamayı geçirmenin kritikliğini veya riskini belirlemeye yardımcı olmak için son 1, 7 veya 30 gün içinde uygulamaya giriş yapmış benzersiz kullanıcı sayısını alın.
* **Geçiş testlerini çalıştırın ve sorunları düzeltin.** Raporlama hizmeti, bir uygulamanın geçirilip geçirilemeye hazır olup olmadığını belirlemek için testleri otomatik olarak çalıştırAr. Sonuçlar, AD FS uygulama etkinlik raporunda geçiş durumu olarak görüntülenir. Olası geçiş sorunları tanımlanırsa, sorunları nasıl ele alacağınıza ilişkin özel yönergeler alırsınız.

AD FS uygulama etkinlik verileri, bu yönetici rollerinden herhangi biri atanan kullanıcılar tarafından kullanılabilir: genel yönetici, rapor okuyucu, güvenlik okuyucu, uygulama yöneticisi veya bulut uygulama yöneticisi.

## <a name="prerequisites"></a>Ön koşullar

* Kuruluşunuzun uygulamalara erişmek için şu anda AD FS kullanıyor olması gerekir.
* Azure AD Bağlantı Durumu, Azure AD kiracınızda etkinleştirilmiş olmalıdır.
   * [Azure AD Connect Health hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)
   * [Azure AD Bağlantı Durumu'nun ayarlanmasına başlayın](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-agent-install)

## <a name="discover-ad-fs-applications-that-can-be-migrated"></a>Geçirilebilen AD FS uygulamalarını keşfedin 

AD FS uygulama etkinlik raporu Azure portalında Azure REKLAM **Kullanımı & öngörüler** raporlaması altında kullanılabilir. AD FS uygulama faaliyet raporu, her AD FS uygulamasını olduğu gibi geçirilip geçirilmeyebileceğini veya ek inceleme gerekip gerekmeden olup olmadığını belirlemek için analiz eder. 

1. AD FS uygulama etkinlik verilerine (genel yönetici, rapor okuyucusu, güvenlik okuyucusu, uygulama yöneticisi veya bulut uygulama yöneticisi) erişimi olan bir yönetici rolüyle [Azure portalında](https://portal.azure.com) oturum açın.

2. **Azure Etkin Dizini'ni**seçin ve ardından **Kurumsal uygulamaları**seçin.

3. **Etkinlik**altında, **Kullanım & Öngörüleri (Önizleme)** seçeneğini belirleyin ve ardından kuruluşunuzdaki tüm AD FS uygulamalarının listesini açmak için **AD FS uygulama etkinliğini** seçin.

   ![AD FS uygulama etkinliği](media/migrate-adfs-application-activity/adfs-application-activity.png)

4. AD FS uygulama etkinlik listesindeki her uygulama için **Geçiş durumunu**görüntüleyin:

   * **Geçişe hazır,** AD FS uygulama yapılandırmasının Azure AD'de tam olarak desteklenebileceği ve olduğu gibi geçirilebileceği anlamına gelir.

   * **İhtiyaçların gözden geçirilmesi,** uygulamanın bazı ayarlarının Azure AD'ye geçirilebileceği, ancak olduğu gibi geçirilmeyen ayarları gözden geçirmeniz gerektiği anlamına gelir.

   * **Gereken ek adımlar,** Azure AD'nin uygulamanın bazı ayarlarını desteklemediği anlamına gelir, bu nedenle uygulama geçerli durumunda geçirilemez.

## <a name="evaluate-the-readiness-of-an-application-for-migration"></a>Geçiş için bir uygulamanın hazır olup olma durumunu değerlendirme 

1. AD FS uygulama etkinlik listesinde, geçiş ayrıntılarını açmak için **Geçiş durumu** sütunundaki durumu tıklatın. Olası geçiş sorunlarıyla birlikte geçen yapılandırma testlerinin bir özetini görürsünüz.

   ![Geçiş ayrıntıları](media/migrate-adfs-application-activity/migration-details.png)

2. Ek geçiş kuralı ayrıntılarını açmak için bir iletiyi tıklatın. Test edilen özelliklerin tam listesi için aşağıdaki [AD FS uygulama yapılandırma sınayı tablosuna](#ad-fs-application-configuration-tests) bakın.

   ![Geçiş kuralı ayrıntıları](media/migrate-adfs-application-activity/migration-rule-details.png)

### <a name="ad-fs-application-configuration-tests"></a>AD FS uygulama yapılandırma testleri

Aşağıdaki tablo, AD FS uygulamalarında gerçekleştirilen tüm yapılandırma testlerini listeler.

|Sonuç  |Geçiş/Uyarı/Başarısız  |Açıklama  |
|---------|---------|---------|
|Test-ADFSRPAdditionalAuthenticationRules <br> Ek Kimlik Doğrulama için en az bir geçirilemez kural algılandı.       | Geçiş/Uyarı          | Güvenen tarafın çok faktörlü kimlik doğrulaması (MFA) için istekte bulunur kuralları vardır. Azure AD'ye geçmek için bu kuralları Koşullu Erişim ilkelerine çevirin. Şirket içi MFA kullanıyorsanız, Azure MFA'ya geçmenizi öneririz. [Koşullu Erişim hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)        |
|Test-ADFSRPAdditionalWSFedEndpoint <br> Güvenen taraf AdditionalWSFedEndpoint doğru ayarlanmıştır.       | Başarılı/Başarısız          | AD FS'de güvenen taraf birden fazla WS-Fed iddia bitiş noktası sağlar.Şu anda Azure AD yalnızca birini destekler.Bu sonucun geçişi engellediği bir senaryonuz varsa, [bize bildirin.](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695621-allow-multiple-ws-fed-assertion-endpoints)     |
|Test-ADFSRPAllowedAuthenticationClassReferences <br> Relying Party, İzin Kimlik DoğrulamaSınıf Başvuruları'nı belirlemiştir.       | Başarılı/Başarısız          | AD FS'deki bu ayar, uygulamanın yalnızca belirli kimlik doğrulama türlerine izin verecek şekilde yapılandırılıp yapılandırılmadığını belirtmenize olanak tanır. Bu yeteneğe ulaşmak için Koşullu Erişim'i kullanmanızı öneririz. Bu sonucun geçişi engellediği bir senaryonuz varsa, [bize bildirin.](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695672-allow-in-azure-ad-to-specify-certain-authentication)  [Koşullu Erişim hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)          |
|Test-ADFSRPAlwaysRequire Authentication <br> AlwaysRequireAuthenticationCheckResult      | Başarılı/Başarısız          | AD FS'deki bu ayar, uygulamanın SSO çerezlerini yoksaymak ve **Her Zaman Kimlik Doğrulama İstemi'ni**yok saymak için yapılandırılıp yapılandırılmadığını belirtmenize olanak tanır. Azure AD'de, benzer davranışlar elde etmek için Koşullu Erişim ilkelerini kullanarak kimlik doğrulama oturumunu yönetebilirsiniz. [Koşullu Erişim ile kimlik doğrulama oturumu yönetimini yapılandırma hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime)          |
|Test-ADFSRPAutoUpdateEnabled <br> Güvenen Taraf AutoUpdateEnabled gerçek ayarlanmış       | Geçiş/Uyarı          | AD FS'deki bu ayar, AD FS'nin federasyonun meta verileri içindeki değişikliklere göre uygulamayı otomatik olarak güncelleştirmek üzere yapılandırılıp yapılandırılmadığını belirtmenize olanak tanır. Azure AD bugün bunu desteklemez, ancak uygulamanın Azure AD'ye geçişini engellememelidir.           |
|Test-ADFSRPClaimsSağlayıcı Adı <br> Güvenen Taraf birden fazla İddia Sağlayıcısı etkin       | Başarılı/Başarısız          | AD FS'deki bu ayar, güvenen tarafın talepleri kabul ettiği kimlik sağlayıcılarını çağırır. Azure AD'de, Azure AD B2B'yi kullanarak dış işbirliğini etkinleştirebilirsiniz. [Azure AD B2B hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)          |
|Test-ADFSRPDelegationAuthorizationRules Kuralları      | Başarılı/Başarısız          | Uygulama, özel delegasyon yetkilendirme kuralları tanımlanmıştır. Bu, Azure AD'nin OpenID Connect ve OAuth 2.0 gibi modern kimlik doğrulama protokollerini kullanarak desteklediği bir WS-Trust konseptidir. [Microsoft Identity Platformu hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc)          |
|Test-ADFSRPImpersonationAuthorizationRules Kuralları       | Geçiş/Uyarı          | Uygulama, tanımlanan özel kimliğe bürünme yetkilendirme kuralları na sahiptir.Bu, Azure AD'nin OpenID Connect ve OAuth 2.0 gibi modern kimlik doğrulama protokollerini kullanarak desteklediği bir WS-Trust konseptidir. [Microsoft Identity Platformu hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc)          |
|Test-ADFSRPIssuanceAuthorizationRules Kuralları <br> Verme Yetkilendirmesi için en az bir geçirilemez kural algılandı.       | Geçiş/Uyarı          | Uygulama, AD FS'de tanımlanan özel verme yetkilendirme kurallarına sahiptir.Azure AD, Azure AD Koşullu Erişim ile bu işlevselliği destekler. [Koşullu Erişim hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) <br> Ayrıca, kullanıcı veya uygulamaya atanan gruplar tarafından bir uygulamaya erişimi kısıtlayabilirsiniz. [Uygulamalara erişmek için kullanıcı ve grup atama hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups)            |
|Test-ADFSRPIssuanceTransformRules <br> IssuanceTransform için en az bir geçirilemez kural algılandı.       | Geçiş/Uyarı          | Uygulama, AD FS'de tanımlanan özel verme dönüştürme kurallarına sahiptir. Azure AD, belirteçte verilen talepleri özelleştirmeyi destekler. Daha fazla bilgi edinmek [için, kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)bakın.           |
|Test-ADFSRPMonitoring Etkin <br> Güvenen Taraf, İzleme Etkin'ine sahip.       | Geçiş/Uyarı          | AD FS'deki bu ayar, AD FS'nin federasyonun meta verileri içindeki değişikliklere göre uygulamayı otomatik olarak güncelleştirmek üzere yapılandırılıp yapılandırılmadığını belirtmenize olanak tanır. Azure AD bugün bunu desteklemez, ancak uygulamanın Azure AD'ye geçişini engellememelidir.           |
|Test-ADFSRPNotBeforeSkew <br> NotBeforeSkewCheckResult      | Geçiş/Uyarı          | AD FS, SAML belirtecindeki NotBefore ve NotOnOrAfter zamanlarını temel alan bir zaman eğrilmesine izin verir. Azure AD bunu varsayılan olarak otomatik olarak işler.          |
|Test-ADFSRPRequestMFAFromClaimsSağlayıcıları <br> Güvenen Taraf RequestMFAFromClaimsProviders doğru ayarlanmış vardır.       | Geçiş/Uyarı          | AD FS'deki bu ayar, kullanıcı farklı bir talep sağlayıcısından geldiğinde MFA'nın davranışını belirler. Azure AD'de, Azure AD B2B'yi kullanarak dış işbirliğini etkinleştirebilirsiniz. Ardından, konuk erişimini korumak için Koşullu Erişim ilkelerini uygulayabilirsiniz. [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b) ve [Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)hakkında daha fazla bilgi edinin.          |
|Test-ADFSRPSignedSamlRequestsGerekli <br> Güvenen Taraf İmzalandıSamlRequestsGerekli doğru ayarlanmış       | Başarılı/Başarısız          | Uygulama, SAML isteğindeki imzayı doğrulamak için AD FS'de yapılandırılır. Azure AD imzalı bir SAML isteğini kabul eder; ancak, imzayı doğrulamaz. Azure AD'nin kötü amaçlı çağrılara karşı korumak için farklı yöntemleri vardır. Örneğin, Azure AD, SAML isteğini doğrulamak için uygulamada yapılandırılan yanıt URL'lerini kullanır. Azure AD yalnızca uygulama için yapılandırılan URL'leri yanıtlamak için bir belirteç gönderir. Bu sonucun geçişi engellediği bir senaryonuz varsa, [bize bildirin.](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/13394589-saml-signature)          |
|Test-ADFSRPTokenÖmür boyu <br> TokenÖmür BoyuKontrol Sonucu        | Geçiş/Uyarı         | Uygulama özel bir belirteç ömrü için yapılandırılır. AD FS varsayılan bir saattir.Azure AD, Koşullu Erişim'i kullanarak bu işlevselliği destekler. Daha fazla bilgi edinmek için bkz: [Koşullu Erişim ile kimlik doğrulama oturum yönetimini yapılandır.](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime)          |
|Relying Party iddiaları şifrelemek için ayarlanır. Bu, Azure AD tarafından desteklenir       | Geçirmek          | Azure AD ile uygulamaya gönderilen belirteci şifreleyebilirsiniz. Daha fazla bilgi için Azure [AD SAML belirteç şifrelemesini Yapılandır' a](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption)bakın.          |
|ŞifreliNameIdRequiredCheckResult      | Başarılı/Başarısız          | Uygulama, SAML belirtecindeki nameID iddiasını şifrelemek için yapılandırılır.Azure AD ile uygulamaya gönderilen belirtecin tamamını şifreleyebilirsiniz.Belirli taleplerin şifrelemesi henüz desteklenmez. Daha fazla bilgi için Azure [AD SAML belirteç şifrelemesini Yapılandır' a](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption)bakın.         |

## <a name="check-the-results-of-claim-rule-tests"></a>Talep kuralı testlerinin sonuçlarını kontrol edin

AD FS'deki uygulama için bir talep kuralı yapılandırıldıysanız, deneyim tüm talep kuralları için ayrıntılı bir çözüm sağlar. Hangi talep kurallarının Azure AD'ye taşınabileceğini ve hangilerinin daha fazla gözden geçirilmesi gerektiğini görürsünüz.

1. AD FS uygulama etkinlik listesinde, geçiş ayrıntılarını açmak için **Geçiş durumu** sütunundaki durumu tıklatın. Olası geçiş sorunlarıyla birlikte geçen yapılandırma testlerinin bir özetini görürsünüz.

2. Geçiş **kuralı ayrıntıları** sayfasında, olası geçiş sorunlarıyla ilgili ayrıntıları görüntülemek ve ek kılavuz almak için sonuçları genişletin. Test edilen tüm talep kurallarının ayrıntılı bir listesi için, [aşağıdaki talep kuralı testleri tablosunun sonuçlarını kontrol](#check-the-results-of-claim-rule-tests) edin.

   Aşağıdaki örnekte, IssuanceTransform kuralıiçin geçiş kuralı ayrıntıları gösterilmektedir. Uygulamayı Azure AD'ye geçirebilmeniz için talebin gözden geçirilmesi ve ele alınması gereken belirli bölümlerini listeler.

   ![Geçiş kuralı ek kılavuz ayrıntıları](media/migrate-adfs-application-activity/migration-rule-details-guidance.png)

### <a name="claim-rule-tests"></a>Talep kuralı testleri

Aşağıdaki tablo, AD FS uygulamalarında gerçekleştirilen tüm talep kural testlerini listeler.

|Özellik  |Açıklama  |
|---------|---------|
|UNSUPPORTED_CONDITION_PARAMETER      | Koşul deyimi, talebin belirli bir desenle eşleşip eşleşmeyeceklerini değerlendirmek için Düzenli İfadeler kullanır.Azure AD'de benzer bir işlevsellik elde etmek için, diğerlerinin yanı sıra IfEmpty(), StartWith(), Contains() gibi önceden tanımlanmış dönüşümü kullanabilirsiniz. Daha fazla bilgi için, [kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)bakın.          |
|UNSUPPORTED_CONDITION_CLASS      | Koşul deyimi, verme deyimini çalıştırmadan önce değerlendirilmesi gereken birden çok koşula sahiptir.Azure AD, birden çok talep değerini değerlendirebileceğiniz talebin dönüştürme işlevleriyle bu işlevselliği destekleyebilir.Daha fazla bilgi için, [kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)bakın.          |
|UNSUPPORTED_RULE_TYPE      | Talep kuralı tanınamadı. Azure AD'deki taleplerin nasıl yapılandırılabildiğini hakkında daha fazla bilgi için, [kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)bakın.          |
|CONDITION_MATCHES_UNSUPPORTED_ISSUER      | Koşul deyimi, Azure AD'de desteklenmeyen bir Veren kullanır.Şu anda Azure AD, etkin dizin veya Azure AD gibi farklı mağazalardan talepte bulunmaktadır. Bu durum, uygulamaları Azure AD'ye geçirmenizi engelliyorsa, [bize bildirin.](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire)         |
|UNSUPPORTED_CONDITION_FUNCTION      | Koşul deyimi, eşleşme sayısına bakılmaksızın tek bir talep vermek veya eklemek için toplu bir işlev kullanır.Azure AD'de, diğer işlevlerle ifempty(), StartWith(), Contains() gibi işlevlerle talep için hangi değeri kullanacağına karar vermek için kullanıcının özniteliğini değerlendirebilirsiniz.Daha fazla bilgi için, [kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)bakın.          |
|RESTRICTED_CLAIM_ISSUED      | Koşul deyimi, Azure AD'de kısıtlanmış bir talep kullanır. Kısıtlı bir talepte bulunabilirsiniz, ancak kaynağını değiştiremezsiniz veya herhangi bir dönüşüm uygulayamazsınız. Daha fazla bilgi için, [Azure AD'deki belirli bir uygulamaiçin belirteçlerde yayılan talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping)bakın.          |
|EXTERNAL_ATTRIBUTE_STORE      | Verme deyimi, Active Directory'den farklı bir öznitelik deposu kullanır. Şu anda Azure AD, etkin dizin veya Azure AD gibi farklı mağazalardan talepte bulunmaktadır. Bu sonuç, uygulamaları Azure AD'ye geçirmenizi engelliyorsa, [bize bildirin.](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire)          |
|UNSUPPORTED_ISSUANCE_CLASS      | Verme deyimi, gelen talep kümesine talep eklemek için ADD'yi kullanır. Azure AD'de bu, birden çok talep dönüşümü olarak yapılandırılabilir.Daha fazla bilgi için, [kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping)bakın.         |
|UNSUPPORTED_ISSUANCE_TRANSFORMATION      | Verme deyimi, yayımlanacak talebin değerini dönüştürmek için Düzenli İfadeler kullanır.Azure AD'de benzer işlevleri elde etmek için, diğerleri arasında Extract(), Trim(), ToLower gibi önceden tanımlanmış dönüşümü kullanabilirsiniz. Daha fazla bilgi için, [kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)bakın.          |


## <a name="next-steps"></a>Sonraki adımlar

- [Video: Bir uygulamayı geçirmek için AD FS etkinlik raporu nasıl kullanılır?](https://www.youtube.com/watch?v=OThlTA239lU)
- [Azure Active Directory ile uygulamaları yönetme](what-is-application-management.md)
- [Uygulamalara erişimi yönetme](what-is-access-management.md)
- [Azure AD Connect federasyonu](../hybrid/how-to-connect-fed-whatis.md)
