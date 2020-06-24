---
title: AD FS uygulamalarını Azure Active Directory taşımak için etkinlik raporunu kullanın | Microsoft Docs '
description: Active Directory Federasyon Hizmetleri (AD FS) (AD FS) uygulama etkinliği raporu AD FS uygulamaları hızla Azure Active Directory (Azure AD) ile geçirmenize olanak sağlar. AD FS için bu geçiş aracı, Azure AD ile uyumluluğu tanımlar ve geçiş kılavuzu sağlar.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/14/2019
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: e379b81fafb156b1ea71a8992abfdc060065b8b2
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763185"
---
# <a name="use-the-ad-fs-application-activity-report-preview-to-migrate-applications-to-azure-ad"></a>Uygulamaları Azure AD 'ye geçirmek için AD FS uygulama etkinliği raporunu (Önizleme) kullanın

Birçok kuruluş, bulut uygulamalarında çoklu oturum açma sağlamak için Active Directory Federasyon Hizmetleri (AD FS) (AD FS) kullanır. Özellikle maliyet yönetimi, risk yönetimi, üretkenlik, uyumluluk ve idare bakımından AD FS uygulamalarınızı kimlik doğrulaması için Azure AD 'ye taşımaya yönelik önemli yararlar vardır. Ancak hangi uygulamaların Azure AD ile uyumlu olduğunu ve belirli geçiş adımlarının belirlenmesi zaman alabilir.

Azure portal AD FS uygulama etkinliği raporu (Önizleme), uygulamalarınızın Azure AD 'ye geçirilme yeteneğine hızlı bir şekilde yararlanmanızı sağlar. Tüm AD FS uygulamaları Azure AD ile uyumluluk için değerlendirir, herhangi bir sorun olup olmadığını denetler ve tek tek uygulamaları geçişe hazırlamaya yönelik rehberlik sağlar. AD FS uygulama etkinliği raporuyla şunları yapabilirsiniz:

* **AD FS uygulamaları ve geçişinizin kapsamını bulun.** AD FS uygulama etkinliği raporu, kuruluşunuzdaki tüm AD FS uygulamaları listeler ve Azure AD 'ye geçiş için hazırlığını gösterir.
* **Geçiş için uygulamaları önceliklendirin.** Uygulamanın geçiş riskini veya riskini belirlemede yardımcı olması için son 1, 7 veya 30 gün içinde uygulamada oturum açmış benzersiz kullanıcıların sayısını alın.
* **Geçiş testlerini çalıştırın ve sorunları giderin.** Raporlama hizmeti, bir uygulamanın geçişe hazırlanma olup olmadığını anlamak için testleri otomatik olarak çalıştırır. Sonuçlar, AD FS uygulama etkinliği raporunda geçiş durumu olarak görüntülenir. Olası geçiş sorunları tanımlanmışsa, sorunların nasıl ele alınacağını gösteren özel yönergeler alırsınız.

AD FS uygulama etkinliği verileri şu yönetici rollerinden herhangi biri atanmış kullanıcılar tarafından kullanılabilir: genel yönetici, rapor okuyucu, güvenlik okuyucusu, uygulama Yöneticisi veya bulut uygulama Yöneticisi.

## <a name="prerequisites"></a>Ön koşullar

* Kuruluşunuzun uygulamalara erişmek için şu anda AD FS kullanıyor olması gerekir.
* Azure AD Connect Health Azure AD kiracınızda etkinleştirilmelidir.
   * [Azure AD Connect Health hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)
   * [Azure AD Connect Health ayarlamaya başlama](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-agent-install)

## <a name="discover-ad-fs-applications-that-can-be-migrated"></a>Geçirilebileceğini AD FS uygulamaları bulma 

AD FS uygulama etkinliği raporu, Azure AD **kullanımı & Öngörüler** raporlaması altındaki Azure Portal kullanılabilir. AD FS uygulama etkinliği raporu, ' nin olduğu gibi geçirilip geçirilmeyeceğini veya ek inceleme gerekip gerekmediğini öğrenmek için her bir AD FS uygulamasını analiz eder. 

1. AD FS uygulama etkinlik verilerine (genel yönetici, rapor okuyucu, güvenlik okuyucusu, uygulama Yöneticisi veya bulut uygulama Yöneticisi) erişimi olan yönetici rolüyle [Azure Portal](https://portal.azure.com) oturum açın.

2. **Azure Active Directory**' yi seçin ve ardından **Kurumsal uygulamalar**' ı seçin.

3. **Etkinlik**' in altında, **kullanım & Öngörüler ' i (Önizleme)** seçin ve sonra kuruluşunuzdaki tüm AD FS uygulamaların bir listesini açmak için **AD FS uygulama etkinliği** ' ni seçin.

   ![AD FS uygulama etkinliği](media/migrate-adfs-application-activity/adfs-application-activity.png)

4. AD FS uygulaması etkinlik listesindeki her bir uygulama için, **geçiş durumunu**görüntüleyin:

   * Geçişe **hazırlanma** , Azure AD 'de AD FS uygulama yapılandırmasının tam desteklendiğinden ve olduğu gibi geçirilebileceği anlamına gelir.

   * **Gerekli gözden geçirme** , bazı uygulama AYARLARıNıN Azure AD 'ye geçirilebileceği anlamına gelir, ancak olduğu gibi geçirilemeyen ayarları gözden geçirmeniz gerekir.

   * **Gerekli ek adımlar** Azure AD 'nin bazı uygulama ayarlarını desteklemediği anlamına gelir, bu nedenle uygulama geçerli durumunda geçirilemez.

## <a name="evaluate-the-readiness-of-an-application-for-migration"></a>Geçiş için bir uygulamanın hazır olduğunu değerlendirin 

1. AD FS uygulama etkinliği listesinde geçiş ayrıntıları ' nı açmak için **geçiş durumu** sütunundaki duruma tıklayın. Olası geçiş sorunlarıyla birlikte, geçilen yapılandırma testlerinin bir özetini görürsünüz.

   ![Geçiş ayrıntıları](media/migrate-adfs-application-activity/migration-details.png)

2. Ek geçiş kuralı ayrıntılarını açmak için bir iletiye tıklayın. Sınanan özelliklerin tam listesi için aşağıdaki [AD FS uygulama yapılandırma testleri](#ad-fs-application-configuration-tests) tablosuna bakın.

   ![Geçiş kuralı ayrıntıları](media/migrate-adfs-application-activity/migration-rule-details.png)

### <a name="ad-fs-application-configuration-tests"></a>Uygulama yapılandırma testlerini AD FS

Aşağıdaki tabloda AD FS uygulamalarda gerçekleştirilen tüm yapılandırma sınamaları listelenmektedir.

|Sonuç  |Geçiş/uyarı/başarısız  |Description  |
|---------|---------|---------|
|Test-ADFSRPAdditionalAuthenticationRules <br> AdditionalAuthentication için en az bir geçirilip geçirilemeyeceğini denetleyin olmayan kural algılandı.       | Geçiş/uyarı          | Bağlı olan taraf, Multi-Factor Authentication (MFA) için sorulacak kurallara sahiptir. Azure AD 'ye geçmek için bu kuralları koşullu erişim ilkelerine çevirin. Şirket içi MFA kullanıyorsanız Azure MFA ' ya geçmeniz önerilir. [Koşullu erişim hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).        |
|Test-ADFSRPAdditionalWSFedEndpoint <br> Bağlı olan tarafın AdditionalWSFedEndpoint değeri true olarak ayarlanmıştır.       | Başarılı/Başarısız          | AD FS bağlı olan taraf, birden çok WS-Besde onaylama uç noktasına izin veriyor.Şu anda Azure AD yalnızca bir tane destekler.Bu sonucun geçişi engellediği bir senaryonuz varsa [bize bilgi verin](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695621-allow-multiple-ws-fed-assertion-endpoints).     |
|Test-Adfsrpallowedaduthenticationclassreferences <br> Bağlı olan taraf, Allowedaduthenticationclassreferences olarak ayarlanmıştır.       | Başarılı/Başarısız          | AD FS Bu ayar, uygulamanın yalnızca belirli kimlik doğrulama türlerine izin vermek üzere yapılandırılıp yapılandırılmadığını belirtmenize olanak tanır. Bu özelliği başarmak için Koşullu erişimin kullanılması önerilir. Bu sonucun geçişi engellediği bir senaryonuz varsa [bize bilgi verin](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695672-allow-in-azure-ad-to-specify-certain-authentication).  [Koşullu erişim hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).          |
|Test-Adfsrpalwaysrequiyeniden kimlik doğrulaması <br> AlwaysRequireAuthenticationCheckResult      | Başarılı/Başarısız          | AD FS Bu ayar, uygulamanın SSO tanımlama bilgilerini yoksayacak şekilde yapılandırılıp yapılandırılmadığını belirtmenizi sağlar ve **her zaman kimlik doğrulaması Için istem**yapın. Azure AD 'de, benzer davranışa ulaşmak için koşullu erişim ilkelerini kullanarak kimlik doğrulama oturumunu yönetebilirsiniz. [Kimlik doğrulama oturumu yönetimini koşullu erişimle yapılandırma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).          |
|Test-ADFSRPAutoUpdateEnabled <br> Bağlı olan taraf, true olarak ayarlanmış bir       | Geçiş/uyarı          | AD FS Bu ayar, AD FS Federasyon meta verilerindeki değişikliklere göre uygulamayı otomatik olarak güncelleştirmek üzere yapılandırılıp yapılandırılmadığını belirtmenize olanak tanır. Azure AD bu günü desteklemez, ancak uygulamanın Azure AD 'ye geçirilmesini engellemez.           |
|Test-ADFSRPClaimsProviderName <br> Bağlı olan tarafın birden çok ClaimsProviders etkin       | Başarılı/Başarısız          | AD FS Bu ayar, bağlı olan tarafın talepleri kabul ettiği kimlik sağlayıcılarını çağırır. Azure AD 'de, Azure AD B2B kullanarak dış işbirliğini etkinleştirebilirsiniz. [Azure AD B2B hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b).          |
|Test-ADFSRPDelegationAuthorizationRules      | Başarılı/Başarısız          | Uygulamanın tanımlı özel temsili yetkilendirme kuralları vardır. Bu, Azure AD 'nin, OpenID Connect ve OAuth 2,0 gibi modern kimlik doğrulama protokollerini kullanarak desteklediği WS-Trust kavramıdır. [Microsoft Identity platformu hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc).          |
|Test-Adfsrpımpersonationauthorizationrules       | Geçiş/uyarı          | Uygulamanın tanımlı özel kimliğe bürünme yetkilendirme kuralları vardır.Bu, Azure AD 'nin, OpenID Connect ve OAuth 2,0 gibi modern kimlik doğrulama protokollerini kullanarak desteklediği WS-Trust kavramıdır. [Microsoft Identity platformu hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc).          |
|Test-Adfsrpıssuanceauthorizationrules <br> Issuanceauthorization için en az bir geçirilip geçirilemeyeceğini denetleyin olmayan kural algılandı.       | Geçiş/uyarı          | Uygulamanın, AD FS tanımlı özel verme yetkilendirme kuralları vardır.Azure AD, Azure AD koşullu erişimiyle bu işlevselliği destekler. [Koşullu erişim hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/conditional-access/overview). <br> Ayrıca, uygulamaya atanan kullanıcı veya gruplarla bir uygulamaya erişimi kısıtlayabilirsiniz. [Uygulamalara erişmek için Kullanıcı ve Grup atama hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups).            |
|Test-Adfsrpıssuancetransformrules <br> Issuancetransform için en az bir geçirilip geçirilemeyeceğini denetleyin olmayan kural algılandı.       | Geçiş/uyarı          | Uygulamanın, AD FS tanımlı özel verme dönüştürme kuralları vardır. Azure AD, belirteçte verilen talepleri özelleştirmeyi destekler. Daha fazla bilgi edinmek için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).           |
|Test-ADFSRPMonitoringEnabled <br> Bağlı olan tarafın MonitoringEnabled değeri true olarak ayarlandı.       | Geçiş/uyarı          | AD FS Bu ayar, AD FS Federasyon meta verilerindeki değişikliklere göre uygulamayı otomatik olarak güncelleştirmek üzere yapılandırılıp yapılandırılmadığını belirtmenize olanak tanır. Azure AD bu günü desteklemez, ancak uygulamanın Azure AD 'ye geçirilmesini engellemez.           |
|Test-ADFSRPNotBeforeSkew <br> NotBeforeSkewCheckResult      | Geçiş/uyarı          | AD FS SAML belirtecindeki NotBefore ve NotOnOrAfter zamanına bağlı olarak bir zaman çarpıklığı sağlar. Azure AD varsayılan olarak bunu otomatik olarak işler.          |
|Test-ADFSRPRequestMFAFromClaimsProviders <br> Bağlı olan tarafın RequestMFAFromClaimsProviders değeri true olarak ayarlanmıştır.       | Geçiş/uyarı          | AD FS Bu ayar, Kullanıcı farklı bir talep sağlayıcısından geldiğinde MFA davranışını belirler. Azure AD 'de, Azure AD B2B kullanarak dış işbirliğini etkinleştirebilirsiniz. Daha sonra, konuk erişimini korumak için koşullu erişim ilkeleri uygulayabilirsiniz. [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b) ve [koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)hakkında daha fazla bilgi edinin.          |
|Test-ADFSRPSignedSamlRequestsRequired <br> Bağlı olan taraf için SignedSamlRequestsRequired değeri true olarak ayarlandı       | Başarılı/Başarısız          | Uygulama, SAML isteğindeki imzayı doğrulamak için AD FS ' de yapılandırılır. Azure AD, imzalı bir SAML isteğini kabul eder; Ancak, imzayı doğrulamaz. Azure AD kötü amaçlı çağrılara karşı korumak için farklı yöntemlere sahiptir. Örneğin, Azure AD, SAML isteğini doğrulamak için uygulamada yapılandırılan yanıt URL 'Lerini kullanır. Azure AD, yalnızca uygulama için yapılandırılmış yanıt URL 'Lerine belirteç gönderecek. Bu sonucun geçişi engellediği bir senaryonuz varsa [bize bilgi verin](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/13394589-saml-signature).          |
|Test-ADFSRPTokenLifetime <br> TokenLifetimeCheckResult        | Geçiş/uyarı         | Uygulama, özel bir belirteç ömrü için yapılandırılmıştır. AD FS varsayılan değer bir saattir.Azure AD, koşullu erişimi kullanarak bu işlevselliği destekler. Daha fazla bilgi için bkz. [koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).          |
|Bağlı olan taraf talepleri şifrelemek üzere ayarlanır. Bu, Azure AD tarafından desteklenir       | Aktar          | Azure AD ile uygulamaya gönderilen belirteci şifreleyebilirsiniz. Daha fazla bilgi için bkz. [Azure AD SAML belirteci şifrelemesini yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).          |
|Encryptednameıdrequiredcheckresult      | Başarılı/Başarısız          | Uygulama, SAML belirtecindeki NameID talebini şifrelemek üzere yapılandırılmıştır.Azure AD ile uygulamaya gönderilen belirtecin tamamını şifreleyebilirsiniz.Belirli taleplerin şifrelenmesi henüz desteklenmiyor. Daha fazla bilgi için bkz. [Azure AD SAML belirteci şifrelemesini yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).         |

## <a name="check-the-results-of-claim-rule-tests"></a>Talep kuralı testlerinin sonuçlarını denetleyin

AD FS uygulama için bir talep kuralı yapılandırdıysanız, deneyim tüm talep kuralları için ayrıntılı bir analiz sağlar. Hangi talep kurallarının Azure AD 'ye taşınabileceğini ve hangilerinin daha fazla gözden geçirilmesi gerektiğini göreceksiniz.

1. AD FS uygulama etkinliği listesinde geçiş ayrıntıları ' nı açmak için **geçiş durumu** sütunundaki duruma tıklayın. Olası geçiş sorunlarıyla birlikte, geçilen yapılandırma testlerinin bir özetini görürsünüz.

2. **Geçiş kuralı ayrıntıları** sayfasında, olası geçiş sorunları hakkındaki ayrıntıları göstermek ve ek yönergeler almak için sonuçları genişletin. Test edilen tüm talep kurallarının ayrıntılı bir listesi için, aşağıdaki [talep kuralı testlerinin sonuçları](#check-the-results-of-claim-rule-tests) tablosuna bakın.

   Aşağıdaki örnekte, ıssuancetransform kuralına ilişkin geçiş kuralı ayrıntıları gösterilmektedir. Uygulamayı Azure AD 'ye geçirebilmeniz için gözden geçirilmesi ve giderilmesi gereken talebin belirli kısımlarını listeler.

   ![Geçiş kuralı ayrıntıları ek kılavuz](media/migrate-adfs-application-activity/migration-rule-details-guidance.png)

### <a name="claim-rule-tests"></a>Talep kuralı testleri

Aşağıdaki tabloda AD FS uygulamalarda gerçekleştirilen tüm talep kuralı sınamaları listelenmektedir.

|Özellik  |Açıklama  |
|---------|---------|
|UNSUPPORTED_CONDITION_PARAMETER      | Koşul deyimi, talebin belirli bir Düzenle eşleşip eşleşmediğini değerlendirmek için normal Ifadeler kullanır.Azure AD 'de benzer bir işlevselliğe ulaşmak için, diğerleri arasında IfEmpty (), StartWith (), Contains () gibi önceden tanımlanmış dönüştürmeyi kullanabilirsiniz. Daha fazla bilgi için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|UNSUPPORTED_CONDITION_CLASS      | Condition bildiriminde, verme ifadesini çalıştırmadan önce değerlendirilmesi gereken birden çok koşul vardır.Azure AD, bu işlevselliği birden fazla talep değerini değerlendirebileceğiniz talebin dönüştürme işlevleriyle destekleyebilir.Daha fazla bilgi için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|UNSUPPORTED_RULE_TYPE      | Talep kuralı tanınamadı. Azure AD 'de talepler yapılandırma hakkında daha fazla bilgi için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|CONDITION_MATCHES_UNSUPPORTED_ISSUER      | Condition bildiriminde, Azure AD 'de desteklenmeyen bir veren kullanılmaktadır.Şu anda Azure AD, Active Directory veya Azure AD 'den farklı mağazalardan kaynaklı talepler değildir. Bu, uygulamaları Azure AD 'ye geçirmeyi engelliyorsa [bize izin verin](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire).         |
|UNSUPPORTED_CONDITION_FUNCTION      | Condition ifadesinde, eşleşme sayısından bağımsız olarak tek bir talep vermek veya eklemek için bir toplama işlevi kullanılır.Azure AD 'de, bir kullanıcının özniteliğini, IfEmpty () gibi işlevlerle hangi değerin kullanılacağına karar vermek için, diğer bir deyişle, () Ile StartWith (), Içerir.Daha fazla bilgi için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|RESTRICTED_CLAIM_ISSUED      | Condition ifadesinde, Azure AD 'de kısıtlanmış bir talep kullanılır. Kısıtlı bir talep verebilir, ancak kaynağını değiştiremez veya herhangi bir dönüştürme uygulayamazsınız. Daha fazla bilgi için bkz. [Azure AD 'de belirli bir uygulama için belirteçlerde yayılan talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).          |
|EXTERNAL_ATTRIBUTE_STORE      | Verme bildiriminde Active Directory farklı bir öznitelik deposu kullanılmaktadır. Şu anda Azure AD, Active Directory veya Azure AD 'den farklı mağazalardan kaynaklı talepler değildir. Bu sonuç, uygulamaları Azure AD 'ye geçirmeyi engelliyorsa [bize bilgi verin](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire).          |
|UNSUPPORTED_ISSUANCE_CLASS      | Verme bildiriminde, gelen talep kümesine talepler eklemek için Ekle kullanılır. Azure AD 'de, bu, birden çok talep dönüştürmesi olarak yapılandırılabilir.Daha fazla bilgi için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).         |
|UNSUPPORTED_ISSUANCE_TRANSFORMATION      | Verme deyimi, oluşturulacak talebin değerini dönüştürmek için normal Ifadeler kullanır.Azure AD 'de benzer işlevlere ulaşmak için, ayıklama (), Trim (), diğer bir deyişle, diğer kullanıcıların arasında önceden tanımlanmış bir dönüşüm kullanabilirsiniz. Daha fazla bilgi için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |


## <a name="next-steps"></a>Sonraki adımlar

- [Video: uygulamayı geçirmek için AD FS Etkinlik raporunu kullanma](https://www.youtube.com/watch?v=OThlTA239lU)
- [Azure Active Directory ile uygulamaları yönetme](what-is-application-management.md)
- [Uygulamalara erişimi yönetme](what-is-access-management.md)
- [Azure AD Connect Federasyonu](../hybrid/how-to-connect-fed-whatis.md)
