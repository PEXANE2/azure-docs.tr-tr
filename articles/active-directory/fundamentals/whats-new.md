---
title: Yenilikler neler? Sürüm Notları - Azure Active Directory | Microsoft Docs
description: Azure Active Directory'ye yeni gibi bilinen sorunları, hata düzeltmeleri, en son sürüm notları, işlevler de kullanım dışı ve gelecek öğrenin değişiklikler.
services: active-directory
author: msmimart
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69f0c0ed1efb727c4f4f258dbb3ce0179b52de95
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76931431"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory'deki yenilikler nelerdir?

>Bu URL 'YI kopyalayıp yapıştırarak güncelleştirmeler için ne zaman geri ziyaret ettikinizle ilgili bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` ![RSS akışı okuyucu simgenizi](./media/whats-new/feed-icon-16x16.png) akış okuyucusu.

Azure AD iyileştirmeleri düzenli olarak alır. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklikleri planları

Bu sayfaya ay güncelleştirilir, böylece bunu düzenli olarak tekrar ziyaret. Altı aydan eski olan öğelere bakıyorsanız, [Azure Active Directory yenilikler Için arşivde](whats-new-archive.md)bulabilirsiniz.

---

## <a name="january-2020"></a>Ocak 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>Yeni uygulamalarım portalı genel kullanıma sunuldu

**Türü:** değişiklik planı  
**Hizmet kategorisi:** uygulamalarım  
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Kuruluşunuzu, genel kullanıma sunulan yeni uygulamalarım portalına yükseltin! Yeni Portal ve Koleksiyonlar hakkında daha fazla bilgi için [bkz. uygulamalarım portalındaki koleksiyonlar oluştur](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Azure AD 'deki çalışma alanları koleksiyonlara yeniden adlandırıldı

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** uygulamalarım   
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Yöneticiler kullanıcıların uygulamalarını düzenlemek için yapılandırabilen çalışma alanları, artık koleksiyonlar olarak anılacaktır. [Uygulamalarım portalındaki koleksiyonları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections)sırasında bunları yapılandırma hakkında daha fazla bilgi edinin.

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Azure AD B2C telefon kayıt ve özel ilke kullanarak oturum açma (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C
 
Telefon numarası kaydolma ve oturum açma ile, geliştiriciler ve kuruluşlar müşterilerinin SMS aracılığıyla kullanıcının telefon numarasına gönderilen tek seferlik bir parola kullanarak kaydolmasına ve oturum açmalarına izin verebilir. Bu özellik ayrıca müşterinin telefonlarına erişimi Kayıplarsa telefon numaralarını değiştirmesine de olanak tanır. Özel ilkelerin gücünden biri olan telefon kaydı ve oturum açma, geliştiricilerin ve kuruluşların markalarını sayfa özelleştirmesiyle iletişim kurmasına olanak tanır. [Azure AD B2C 'de özel ilkelerle telefon kaydı ve oturum açma ayarlamayı](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication)öğrenin.
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Ocak 2020

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler özel erişimi](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ocak 2020

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme
 
2020 Ocak 'ta, uygulama galerisine federasyon desteğiyle bu 33 yeni uygulamalar ekledik: 

[Josa](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [fastly Edge bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Office 365 için](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access) [paibot Netlogistik](https://1030-review-develop-3zknud.netlogistik.com/), kemli [,](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial) [leavebot](https://leavebot.io/#home), [Datacamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [Tripactions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [smartwork](https://www.intumit.com/english/SmartWork.html), [Dotcom-Monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [SSOGEN-Azure AD SSO Gateway, Oracle E-Business Suite-EBS, PeopleSoft ve JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial) [barındırılan mycirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [yuhu özelliği yönetim platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [lumapps](https://sites.lumapps.com/login), [Upwork Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [smartdb for Microsoft ekipleri](http://teams.smartdb.jp/login/), [PRESSPAGE](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [Contractsafe SAML2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [maxdentitydeploy Manager yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [Helpshıft](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/), [Coreview](https://portal.coreview.com/), [Squelch Cloud Office365 bağlayıcı](https://laxmi.squelch.io/login), [pingflow kimlik doğrulaması](https://app-staging.pingview.io/), [printerlogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [sandwai](https://app.sandwai.com/), [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [assetsonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [akarı Sanal yardımcı](https://akari.io/ava/)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="two-new-identity-protection-detections"></a>İki yeni kimlik koruması algılamaları

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması
 
Kimlik korumasına iki yeni oturum açma bağlantılı algılama türü ekledik: şüpheli gelen kutusu düzenleme kuralları ve olanaksız seyahat. Bu çevrimdışı algılamalar Microsoft Cloud App Security (MCAS) tarafından keşfedilir ve kimlik koruması sırasında kullanıcıyı ve oturum açma riskini etkiler. Bu algılamalar hakkında daha fazla bilgi için bkz. [oturum açma risk türleri](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk).
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Son değişiklik: URI parçaları, oturum açma yeniden yönlendirme aracılığıyla taşınmaz

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması
 
8 Şubat 2020 ' den itibaren, bir kullanıcının oturum açması için login.microsoftonline.com 'e bir istek gönderildiğinde, hizmet isteğe boş bir parça ekler.  Bu, tarayıcının istekteki mevcut bir parçayı temizler olarak yeniden yönlendirme saldırılarına karşı bir sınıf önler. Hiçbir uygulamanın bu davranışa bağımlılığı olmamalıdır. Daha fazla bilgi için bkz. Microsoft Identity platform belgelerindeki [son değişiklikler](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020) .

---

## <a name="december-2019"></a>Aralık 2019

### <a name="integrate-sap-successfactors-provisioning-into-azure-ad-and-on-premises-ad-public-preview"></a>SAP başarılı faktörleri sağlamasını Azure AD 'ye ve şirket içi AD 'ye tümleştirin (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi

Artık SAP başarılı faktörleri, Azure AD 'de yetkili kimlik kaynağı olarak tümleştirebilirsiniz. Bu tümleştirme, Azure AD hesaplarının sağlanmasını denetlemek için yeni işe alımlar veya sonlandırmalar gibi İK tabanlı olayları kullanma dahil olmak üzere uçtan uca kimlik yaşam döngüsünü otomatikleştirmenize yardımcı olur.

SAP 'nin Azure AD 'ye gelen sağlama için başarılı bir şekilde nasıl ayarlanacağı hakkında daha fazla bilgi için bkz. [SAP başarılı etmenleri yapılandırma otomatik sağlama](https://aka.ms/SAPSuccessFactorsInboundTutorial) öğreticisi.

---

### <a name="support-for-customized-emails-in-azure-ad-b2c-public-preview"></a>Azure AD B2C 'de özelleştirilmiş e-postalar için destek (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Artık kullanıcılarınız uygulamalarınızı kullanmak üzere kaydolduğunuzda özelleştirilmiş e-postalar oluşturmak için Azure AD B2C kullanabilirsiniz. DisplayControls (Şu anda önizleme aşamasında) ve bir üçüncü taraf e-posta sağlayıcısı (örneğin, [SendGrid](https://sendgrid.com/), [mini](https://sparkpost.com/)posta veya özel bir REST API) kullanarak kendi e-posta şablonunuzu **, adres ve** konu metinlerinizi kullanabilir ve yerelleştirme ve özel bir kerelik parola (OTP) ayarlarını destekleyebilirsiniz.

Daha fazla bilgi için bkz. [Azure Active Directory B2C özel e-posta doğrulaması](https://docs.microsoft.com/azure/active-directory-b2c/custom-email).

---

### <a name="replacement-of-baseline-policies-with-security-defaults"></a>Ana hat ilkelerinin güvenlik varsayılanlarını değiştirme

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** diğer  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Kimlik doğrulaması için güvenli bir varsayılan model kapsamında, tüm kiracılardan var olan temel koruma ilkelerini kaldırdık. Bu kaldırma işlemi Şubat 'ın sonunda tamamlanmasına yöneliktir. Bu temel koruma ilkelerinin yerini değiştirme, güvenlik varsayılanlardır. Temel koruma ilkeleri kullanıyorsanız, yeni güvenlik Varsayılanları ilkesine veya koşullu erişime taşımayı planlamanız gerekir. Bu ilkeleri kullanmadıysanız, yapmanız gereken bir işlem yoktur.

Yeni güvenlik Varsayılanları hakkında daha fazla bilgi için bkz. [güvenlik Varsayılanları nelerdir?](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) Koşullu erişim ilkeleri hakkında daha fazla bilgi için bkz. [Genel koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common).

---

## <a name="november-2019"></a>Kasım 2019

### <a name="support-for-the-samesite-attribute-and-chrome-80"></a>SameSite özniteliği ve Chrome 80 için destek

**Türü:** değişiklik planı  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Tanımlama bilgileri için güvenli bir varsayılan model modelinin parçası olarak, Chrome 80 tarayıcısı `SameSite` özniteliği olmadan tanımlama bilgilerine nasıl davrandığını değiştiriyor. `SameSite` özniteliğini belirtmeyen tüm tanımlama bilgileri, `SameSite=Lax`olarak ayarlanmış gibi değerlendirilir, bu da uygulamanızın bağlı olabileceği bazı etki alanları arası tanımlama bilgisi paylaşma senaryolarını engeller. Daha eski Chrome davranışını sürdürmek için `SameSite=None` özniteliğini kullanabilir ve ek bir `Secure` özniteliği ekleyebilirsiniz. bu nedenle, siteler arası tanımlama bilgilerine yalnızca HTTPS bağlantıları üzerinden erişilebilir. Chrome, 4 Şubat 2020 ' de bu değişikliği tamamlamaya zamanlandı.

Tüm geliştiricilerimizin bu kılavuzu kullanarak uygulamalarını test etmenizi öneririz:

- **Güvenli tanımlama bilgisi kullan** ayarının varsayılan değerini **Evet**olarak ayarlayın.

- **SameSite** özniteliği için varsayılan değeri **none**olarak ayarlayın.

- Daha fazla **güvenli**`SameSite` özniteliği ekleyin.

Daha fazla bilgi için bkz. [ASP.net ve ASP.NET Core ' de yakında bulunan SameSite tanımlama bilgisi değişiklikleri](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) ve [Chrome sürüm 79 ve sonraki sürümlerinde müşteri web siteleri ve Microsoft ürün ve hizmetlerine yönelik olası](https://support.microsoft.com/help/4522904/potential-disruption-to-microsoft-services-in-chrome-beta-version-79)kesinti.

---

### <a name="new-hotfix-for-microsoft-identity-manager-mim-2016-service-pack-2-sp2"></a>Microsoft Identity Manager (MıM) 2016 Service Pack 2 (SP2) için yeni düzeltme

**Türü:** düzeltildi  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi

Microsoft Identity Manager (MıM) 2016 Service Pack 2 (SP2) için bir düzeltme paketi paketi (Build 4.6.34.0) kullanılabilir. Bu toplu paket sorunları çözer ve "Bu güncelleştirmede eklenen sorunlar ve iyileştirmeler" bölümünde açıklanan iyileştirmeler ekler.

Daha fazla bilgi edinmek ve düzeltme paketini indirmek için, bkz. [Microsoft Identity Manager 2016 Service Pack 2 (derleme 4.6.34.0) güncelleştirme paketi kullanılabilir](https://support.microsoft.com/help/4512924/microsoft-identity-manager-2016-service-pack-2-build-4-6-34-0-update-r).

---

### <a name="new-ad-fs-app-activity-report-to-help-migrate-apps-to-azure-ad-public-preview"></a>Azure AD 'ye uygulama geçirmeye yardımcı olmak için yeni AD FS App etkinlik raporu (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

Uygulamalarınızın Azure AD 'ye geçirilme yeteneğine sahip olduğunu belirlemek için, Azure portal yeni Active Directory Federasyon Hizmetleri (AD FS) (AD FS) uygulama etkinliği raporunu kullanın. Rapor tüm AD FS uygulamaları Azure AD ile uyumluluk için değerlendirir, herhangi bir sorun olup olmadığını denetler ve tek tek uygulamaları geçişe hazırlamaya yönelik rehberlik sağlar.

Daha fazla bilgi için bkz. [uygulamaları Azure AD 'ye geçirmek için AD FS uygulama etkinliği raporunu kullanma](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity).

---

### <a name="new-workflow-for-users-to-request-administrator-consent-public-preview"></a>Kullanıcıların yönetici onayı istemesi için yeni iş akışı (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** erişim denetimi

Yeni yönetici onayı iş akışı yöneticilere yönetici onayı gerektiren uygulamalara erişim izni vermek için bir yol sağlar. Bir Kullanıcı bir uygulamaya erişmeyi dener, ancak izin sağlayamadığında, artık yönetici onayı için bir istek gönderebilirler. İstek, e-posta ile gönderilir ve Azure portal erişilebilen bir sıraya, gözden geçirenler olarak belirlenmiş tüm yöneticilere yerleştirilir. Gözden geçiren, bekleyen bir istek üzerinde işlem yaptıktan sonra, istenen kullanıcılara bu eylem bildirilir.

Daha fazla bilgi için bkz. [yönetici onayı iş akışını yapılandırma (Önizleme)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow).

---

### <a name="new-azure-ad-app-registrations-token-configuration-experience-for-managing-optional-claims-public-preview"></a>İsteğe bağlı talepleri yönetmek için yeni Azure AD Uygulaması kayıt belirteci yapılandırma deneyimi (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** geliştirici deneyimi

Azure portal yeni **Azure AD uygulaması kayıt belirteci yapılandırma** dikey penceresi artık uygulama geliştiricilerine uygulamalarına yönelik isteğe bağlı taleplerin dinamik bir listesini gösterir. Bu yeni deneyim, Azure AD uygulama geçişlerini kolaylaştırmaya ve isteğe bağlı talepler yapılandırmalarını en aza indirmeye yardımcı olur.

Daha fazla bilgi için bkz. [Azure AD uygulamanıza isteğe bağlı talepler sağlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).

---

### <a name="new-two-stage-approval-workflow-in-azure-ad-entitlement-management-public-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde yeni iki aşamalı onay iş akışı (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün yeteneği:** Yetkilendirme Yönetimi

Bir kullanıcının erişim paketine yönelik isteğini onaylaması için iki onaylayan zorunlu olmanızı sağlayan yeni bir iki aşamalı onay iş akışı sunuyoruz. Örneğin, bunu, istenen Kullanıcı yöneticisinin önce onaylaması gerekir, sonra da onaylanacak kaynak sahibi olmasını sağlayabilirsiniz. Onaylayanlardan biri onaylanmazsa erişim izni verilmez.

Daha fazla bilgi için bkz. [Azure AD yetkilendirme yönetiminde erişim paketi Için değişiklik isteği ve onay ayarları](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-request-policy).

---

### <a name="updates-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Yeni çalışma alanlarıyla birlikte uygulamalarım sayfası güncelleştirmeleri (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** 3. taraf tümleştirme

Artık kuruluşunuzun kullanıcılarının yenilenen uygulamalarımı görüntüleme ve bu deneyimle erişme yönteminizi özelleştirebilirsiniz. Bu yeni deneyim Ayrıca kullanıcılarınızın uygulamaları bulmasını ve düzenlemesini kolaylaştıran yeni çalışma alanları özelliğini de içerir.

Yeni uygulama deneyimi hakkında daha fazla bilgi ve çalışma alanı oluşturma hakkında daha fazla bilgi için bkz. [My Apps Portal 'da çalışma alanları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="google-social-id-support-for-azure-ad-b2b-collaboration-general-availability"></a>Azure AD B2B işbirliği için Google sosyal KIMLIK desteği (genel kullanılabilirlik)

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Azure AD 'de Google sosyal kimliklerini (Gmail hesapları) kullanmaya yönelik yeni destek, kullanıcılarınız ve iş ortaklarınız için işbirliğinin daha basit olmasına yardımcı olur. Artık iş ortaklarınızın Microsoft 'a özgü yeni bir hesap oluşturması ve yönetmesi gerekmez. Microsoft ekipleri artık tüm istemcilerde ve Kiracıdaki ortak ve kiracı ile ilgili kimlik doğrulama uç noktalarında Google kullanıcılarını tam olarak desteklemektedir.

Daha fazla bilgi için bkz. [B2B Konuk kullanıcıları için kimlik sağlayıcısı olarak Google ekleme](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

### <a name="microsoft-edge-mobile-support-for-conditional-access-and-single-sign-on-general-availability"></a>Koşullu erişim ve çoklu oturum açma için Microsoft Edge mobil desteği (genel kullanılabilirlik)

**Türü:** yeni özellik  
**Hizmet kategorisi:** koşullu erişim  
**Ürün özelliği:** kimlik güvenliği ve koruması

İOS ve Android 'de Microsoft Edge için Azure AD artık Azure AD çoklu oturum açma ve koşullu erişim 'i desteklemektedir:

- **Microsoft Edge çoklu oturum açma (SSO):** Çoklu oturum açma artık Azure AD 'ye bağlı tüm uygulamalar için yerel istemcilerde (Microsoft Outlook ve Microsoft Edge gibi) kullanılabilir.

- **Microsoft Edge koşullu erişimi:** Uygulama tabanlı koşullu erişim ilkeleri sayesinde, kullanıcılarınız Microsoft Edge gibi Microsoft Intune korumalı tarayıcıları kullanmalıdır.

Microsoft Edge ile koşullu erişim ve SSO hakkında daha fazla bilgi için bkz. [koşullu erişim Için Microsoft Edge mobil desteği ve çoklu oturum açma, genel kullanıma açık](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Microsoft-Edge-Mobile-Support-for-Conditional-Access-and-Single/ba-p/988179) blog gönderisi. [Uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) veya [cihaz tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)kullanarak istemci uygulamalarınızı ayarlama hakkında daha fazla bilgi için, bkz. [Microsoft Intune İlkeyle korunan tarayıcı kullanarak Web erişimini yönetme](https://docs.microsoft.com/intune/apps/app-configuration-managed-browser).

---

### <a name="azure-ad-entitlement-management-general-availability"></a>Azure AD Yetkilendirme Yönetimi (genel kullanılabilirlik)

**Türü:** yeni özellik  
**Hizmet kategorisi:** diğer  
**Ürün yeteneği:** Yetkilendirme Yönetimi

Azure AD Yetkilendirme Yönetimi, kuruluşların kimlik ve erişim yaşam döngüsünü ölçekli olarak yönetmesine yardımcı olan yeni bir kimlik idare özelliğidir. Bu yeni özellik, erişim isteği iş akışlarını otomatikleştirerek, gruplar, uygulamalar ve SharePoint Online siteleri arasında atamaları, İncelemeleri ve kullanım süresinin tamamlanmasına yardımcı olur.

Azure AD yetkilendirme yönetimi sayesinde, hem çalışanlar hem de kuruluşunuzun dışındaki kullanıcılar için erişimi daha verimli bir şekilde yönetebilirsiniz.

Daha fazla bilgi için bkz. [Azure AD yetkilendirme yönetimi nedir?](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview#license-requirements)

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme  

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

[SAP Cloud Platform kimliği kimlik doğrulama hizmeti](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-provisioning-tutorial), [spaceiq](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-provisioning-tutorial), [mıro](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-provisioning-tutorial), [cloudgate](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-provisioning-tutorial), [Infor cloudsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloudsuite-provisioning-tutorial), [officesi yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-software-provisioning-tutorial), [Öncelik matrisi](https://docs.microsoft.com/azure/active-directory/saas-apps/priority-matrix-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Kasım 2019

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

2019 Kasım 'da, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[Airtable](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [HootSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [Üyeler için mavi erişim (BAI)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [Bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [reslife Portal](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal çoklu oturum açma (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), [teamschamp](https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279), [mosyon](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial), [myaryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial), [Mave-posta](https://loginself1.bluemail.me/), [beyedle](https://teams-web.beedle.co/#/), [Visma](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial), [onedesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [Foko perakende](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [qpazarlar fikir & yenilik yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [netüslü Kullanıcı kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial), [ifkılow](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial), [claromentıı](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial) , [JISC öğrenci Voter kaydı](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial), [e4enable](https://portal.e4enable.com/)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-and-improved-azure-ad-application-gallery"></a>Yeni ve geliştirilmiş Azure AD Uygulama Galerisi

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

Azure Active Directory kiracınızda sağlamayı, OpenID Connect ve SAML 'yi destekleyen önceden tümleştirilmiş uygulamaları bulmanızı kolaylaştırmak için Azure AD uygulama galerisini güncelleştirdik.

Daha fazla bilgi için bkz. [Azure Active Directory kiracınıza uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal).

---

### <a name="increased-app-role-definition-length-limit-from-120-to-240-characters"></a>120 karakterden 240 karaktere daha fazla uygulama rolü tanımı uzunluğu sınırı

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

Müşterilerin bazı uygulama ve hizmetlerdeki uygulama rolü tanımı değeri için uzunluk sınırının 120 karakterden fazla kısa olduğunu duyduk. Yanıt olarak, rol değeri tanımının en büyük uzunluğunu 240 karakter olarak artırdık.

Uygulamaya özel rol tanımlarını kullanma hakkında daha fazla bilgi için bkz. [uygulamanıza uygulama rolleri ekleme ve bunları belirtece alma](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps).

---

## <a name="october-2019"></a>Ekim 2019

### <a name="deprecation-of-the-identityriskevent-api-for-azure-ad-identity-protection-risk-detections"></a>Azure AD Kimlik Koruması risk algılamaları için ıdentityriskevent API 'sinin kullanımdan kaldırılması  

**Türü:** değişiklik planı  
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması

Geliştirici geri bildirimlerine yanıt olarak Azure AD Premium P2 aboneleri artık Microsoft Graph için yeni riskDetection API 'sini kullanarak Azure AD Kimlik Koruması riskli algılama verilerinde karmaşık sorgular gerçekleştirebilir. Mevcut [ıdentityriskevent](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta) API beta sürümü **10 Ocak 2020 '** den itibaren veri döndürmeyi durdurur. Kuruluşunuz ıdentityriskevent API 'sini kullanıyorsa, yeni riskDetection API 'sine geçiş yapmanız gerekir.

Yeni riskDetection API 'SI hakkında daha fazla bilgi için [risk algılama API 'si başvuru belgelerine](https://aka.ms/RiskDetectionsAPI)bakın.

---

### <a name="application-proxy-support-for-the-samesite-attribute-and-chrome-80"></a>SameSite özniteliği ve Chrome 80 için uygulama ara sunucusu desteği

**Türü:** değişiklik planı  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** erişim denetimi

Chrome 80 tarayıcı sürümünden önceki birkaç hafta önce, uygulama proxy tanımlama bilgilerinin **SameSite** özniteliğini nasıl değerlendirdiğinin güncelleştirilmesini planlıyoruz. Chrome 80 ' nin yayınlanmasıyla, **SameSite** özniteliğini belirtmeyen tüm tanımlama bilgileri `SameSite=Lax`olarak ayarlanmış gibi kabul edilir.

Bu değişiklik nedeniyle olumsuz olumsuz etkileri önlemeye yardımcı olmak için uygulama proxy 'Si erişimi ve oturum tanımlama bilgilerini şu şekilde güncelleştiriyoruz:

- **Güvenli tanımlama bilgisi kullan** ayarı için varsayılan değer **Evet**olarak ayarlanıyor.

- **SameSite** özniteliği için varsayılan değer **none**olarak ayarlanıyor.

    >[!NOTE]
    > Uygulama proxy 'Si erişimi tanımlama bilgileri her zaman güvenli kanallar üzerinden iletilir. Bu değişiklikler yalnızca oturum tanımlama bilgileri için geçerlidir.

Uygulama proxy 'Si tanımlama bilgisi ayarları hakkında daha fazla bilgi için bkz. [Azure Active Directory ' de şirket içi uygulamalara erişmek Için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="app-registrations-legacy-and-converged-app-management-from-the-application-registration-portal-appsdevmicrosoftcom-will-no-longer-be-available"></a>Uygulama kayıt portalından (apps.dev.microsoft.com) Uygulama kayıtları (eski) ve yakınsama uygulama yönetimi artık kullanılamayacak

**Türü:** değişiklik planı  
**Hizmet kategorisi:** yok  
**Ürün özelliği:** geliştirici deneyimi

Yakın gelecekte, Azure AD hesapları olan kullanıcılar artık uygulama kayıt portalı 'nı (apps.dev.microsoft.com) kullanarak Yakınsanan uygulamaları kaydedemez veya Uygulama kayıtları (eski) uygulamaları kaydedebilir ve yönetebilir Azure portal deneyim.

Yeni Uygulama kayıtları deneyimi hakkında daha fazla bilgi edinmek için [Azure Portal eğitim kılavuzundaki uygulama kayıtları](../develop/app-registrations-training-guide-for-app-registrations-legacy-users.md)bakın.

---

### <a name="users-are-no-longer-required-to-re-register-during-migration-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcı başına MFA 'dan koşullu erişim tabanlı MFA 'ya geçiş sırasında kullanıcıların artık yeniden kaydedilmesi gerekmez

**Türü:** düzeltildi  
**Hizmet kategorisi:** MFA  
**Ürün özelliği:** kimlik güvenliği ve koruması

Kullanıcıların Kullanıcı başına Multi-Factor Authentication (MFA) için devre dışı bırakıldıklarında ve sonra koşullu erişim ilkesi aracılığıyla MFA için etkinleştirildiklerinde yeniden kaydolmaları gereken bilinen bir sorunu düzelttik.

Kullanıcıların yeniden kaydolmesini gerektirmek için, Azure AD portalındaki kullanıcının kimlik doğrulama yöntemlerinden **gereken MFA 'yı yeniden kaydet** seçeneğini belirleyebilirsiniz. Kullanıcı başına MFA 'dan koşullu erişim tabanlı MFA 'ya Kullanıcı geçirme hakkında daha fazla bilgi için bkz. [Kullanıcı BAŞıNA MFA 'Dan koşullu erişim tabanlı MFA 'ya Kullanıcı dönüştürme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted#convert-users-from-per-user-mfa-to-conditional-access-based-mfa).

---

### <a name="new-capabilities-to-transform-and-send-claims-in-your-saml-token"></a>SAML belirtecinizdeki talepleri dönüştürmek ve göndermek için yeni yetenekler

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** SSO

SAML belirteçinizdeki talepleri özelleştirmenize ve göndermenize yardımcı olacak ek yetenekler ekledik. Bu yeni yetenekler şunlardır:

- Ek talep dönüştürme işlevleri, talepte göndereceğiniz değeri değiştirmenize yardımcı olur.

- Tek bir talebe birden çok dönüşüm uygulama özelliği.

- Kullanıcı türüne ve kullanıcının ait olduğu gruba göre talep kaynağını belirtme özelliği.

Bu yeni yetenekler hakkında daha fazla bilgi için, bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="new-my-sign-ins-page-for-end-users-in-azure-ad"></a>Azure AD 'de son kullanıcılar için yeni oturum açma görevlerim sayfası

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** izleme ve Raporlama

Kuruluşunuzun kullanıcılarının olağandışı etkinlikleri denetlemek için en son oturum açma geçmişini görüntülemesine olanak sağlamak üzere yeni bir **oturum açma** sayfası (https://mysignins.microsoft.com) ) ekledik. Bu yeni sayfa, kullanıcılarınızın şunları görmesini sağlar:

- Herkes parolasını tahmin etmeye çalışıyorsa.

- Bir saldırgan hesabında ve bu konumdan başarıyla oturum açtı.

- Saldırganın erişmeye çalıştığı uygulamalar.

Daha fazla bilgi için, bkz. [Kullanıcılar artık olağan dışı etkinlik bloguna ait oturum açma geçmişini denetleyebilir](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Users-can-now-check-their-sign-in-history-for-unusual-activity/ba-p/916066) .

---

### <a name="migration-of-azure-ad-domain-services-azure-ad-ds-from-classic-to-azure-resource-manager-virtual-networks"></a>Azure AD Domain Services (Azure AD DS) klasik 'ten Azure Resource Manager sanal ağlara geçirme

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün özelliği:** Azure AD Domain Services

Klasik sanal ağlarda takılı olan müşterilerimiz için çok güzel bir haberimiz var! Artık, klasik bir sanal ağdan mevcut bir Kaynak Yöneticisi sanal ağa tek seferlik bir geçiş gerçekleştirebilirsiniz. Kaynak Yöneticisi sanal ağa taşıdıktan sonra, hassas parola ilkeleri, e-posta bildirimleri ve denetim günlükleri gibi ek ve yükseltilmiş özelliklerden yararlanabilirsiniz.

Daha fazla bilgi için bkz. [Klasik sanal ağ modelinden Kaynak Yöneticisi Için önizleme-geçirme Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/migrate-from-classic-vnet).

---

### <a name="updates-to-the-azure-ad-b2c-page-contract-layout"></a>Azure AD B2C sayfa sözleşmesi düzenine yönelik güncelleştirmeler

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Azure AD B2C için sayfa sözleşmesinin sürüm 1.2.0 için bazı yeni değişiklikler ekledik. Bu güncelleştirilmiş sürümde, artık öğeleriniz için yükleme sırasını denetleyebilir ve bu da, stil sayfası (CSS) yüklenirken gerçekleşen titreşimi durdurmaya da yardımcı olabilir.

Sayfa sözleşmesinde yapılan değişikliklerin tam listesi için bkz. [sürüm değişiklik günlüğü](https://docs.microsoft.com/azure/active-directory-b2c/page-layout#120).

---

### <a name="update-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Yeni çalışma alanlarıyla birlikte uygulamalarım sayfasına güncelleştir (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulamalarım  
**Ürün özelliği:** erişim denetimi

Artık, kuruluşunuzun kullanıcılarının, uygulamaları bulmasını kolaylaştırmak için yeni çalışma alanları özelliğini kullanma dahil olmak üzere yepyeni uygulamalara görüntüleme ve bu deneyimle erişme biçimini özelleştirebilirsiniz. Yeni çalışma alanları işlevselliği, kuruluşunuzun kullanıcılarının zaten erişimi olan uygulamalar için bir filtre işlevi görür.

Yeni uygulamalarım deneyimini kullanıma alma ve çalışma alanları oluşturma hakkında daha fazla bilgi için bkz. [My Apps (Önizleme) portalında çalışma alanları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="support-for-the-monthly-active-user-based-billing-model-general-availability"></a>Aylık etkin kullanıcı tabanlı faturalandırma modeli için destek (genel kullanılabilirlik)

**Türü:** yeni özellik  
**Hizmet kategorisi:** B2C - tüketici Kimlik Yönetimi  
**Ürün özelliği:** B2B/B2C

Azure AD B2C artık aylık etkin kullanıcıların (MAU) faturalandırmasını desteklemektedir. MAU faturalandırma, bir takvim ayı sırasında kimlik doğrulama etkinliği olan benzersiz kullanıcı sayısını temel alır. Mevcut müşteriler, bu yeni faturalandırma yöntemine dilediğiniz zaman geçebilir.

1 Kasım 2019 ' den itibaren, tüm yeni müşteriler otomatik olarak bu yöntem kullanılarak faturalandırılacaktır. Bu Faturalandırma yöntemi, müşterilerin maliyet avantajları ve ilerleyebilme aracılığıyla faydalanır.

Daha fazla bilgi için bkz. [aylık etkin kullanıcıları faturalama modeline yükseltme](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing#upgrade-to-monthly-active-users-billing-model).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ekim 2019

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

2019 Ekim 'de, uygulama galerisine federasyon desteğiyle bu 35 yeni uygulamalar ekledik:

Ksıs [: mobil](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-mobile-tutorial), [Juno. yolculuğa](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial), [ExponentHR](https://docs.microsoft.com/azure/active-directory/saas-apps/exponenthr-tutorial), [kişi](https://tact.ai/assistant/), [opuscapita nakit yönetimi](http://cm1.opuscapita.com/tenantname), [salestim](https://prd.salestim.io/forms), [learnster](https://docs.microsoft.com/azure/active-directory/saas-apps/learnster-tutorial), [dynaTrace](https://docs.microsoft.com/azure/active-directory/saas-apps/dynatrace-tutorial), [hunchbuzz](https://login.hunchbuzz.com/integrations/azure/process), en uygun [çalışma](https://docs.microsoft.com/azure/active-directory/saas-apps/freshworks-tutorial), [ECORNELL](https://docs.microsoft.com/azure/active-directory/saas-apps/ecornell-tutorial), [Shiphazı](https://docs.microsoft.com/azure/active-directory/saas-apps/shiphazmat-tutorial), [NETÜSE Bulut güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial), [çekişme](https://docs.microsoft.com/azure/active-directory/saas-apps/contentful-tutorial), [bindtuning](https://bindtuning.com/login), [HireVue koordinatı – AB](https://www.hirevue.com/), [HireVue koordinatı-usonly](https://www.hirevue.com/), [HireVue koordinatı-US](https://www.hirevue.com/), [WittyParrot Bilgi kutusu](https://wittyapi.wittyparrot.com/wittyparrot/api/provision/trail/signup), [cloudmore](https://docs.microsoft.com/azure/active-directory/saas-apps/cloudmore-tutorial), [Visit.org](https://docs.microsoft.com/azure/active-directory/saas-apps/visitorg-tutorial), [cambium Xirrus EasyPass Portal](https://login.xirrus.com/azure-signup), [Paylocity](https://docs.microsoft.com/azure/active-directory/saas-apps/paylocity-tutorial), [posta şanslar!](https://docs.microsoft.com/azure/active-directory/saas-apps/mail-luck-tutorial), [Teamie](https://theteamie.com/), [takımlar için hız](https://velocity.peakup.org/teams/login), [sıgnl4](https://account.signl4.com/manage), [EAB gidilecek Impl](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-impl-tutorial), [ekran karşılamaları](https://console.screenmeet.com/), [Omega noktası](https://pi.ompnt.com/), konuşma için konuşma e [-postası (iPhone)](https://speaking.email/FAQ/98/email-access-via-microsoft-intune), [Office 365 Direct (iPhone/Android)](https://speaking.email/FAQ/126/email-access-via-microsoft-office-365-direct), [ExactCare SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/exactcare-sso-tutorial), [ıhealthhome bakım gezinti sistemi](https://ihealthnav.com/account/signin), [ Qubie](https://qubie.azurewebsites.net/static/adminTab/authorize.html)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="consolidated-security-menu-item-in-the-azure-ad-portal"></a>Azure AD portalındaki birleştirilmiş güvenlik menü öğesi

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması

Artık, mevcut Azure AD güvenlik özelliklerinin tümüne yeni **güvenlik** menü öğesinden ve **arama** çubuğundan Azure Portal erişebilirsiniz. Ayrıca, **güvenlik-Başlarken**adlı yeni **güvenlik** giriş sayfası, ortak belgelerimize, güvenlik kılavuzumuza ve dağıtım kılavuzlarımıza bağlantılar sağlar.

Yeni **güvenlik** menüsü şunları içerir:

- Koşullu Erişim
- Kimlik Koruması
- Güvenlik Merkezi
- Kimlik güvenli puanı
- Kimlik doğrulama yöntemleri
- MFA
- Risk raporları-riskli kullanıcılar, riskli oturum açmalar, risk algılamaları
- Ve daha fazlası...

Daha fazla bilgi için bkz. [güvenlik-Başlarken](https://portal.azure.com/#blade/Microsoft_AAD_IAM/SecurityMenuBlade/GettingStarted).

---

### <a name="office-365-groups-expiration-policy-enhanced-with-autorenewal"></a>Otomatik yenileme ile geliştirilmiş Office 365 grupları süre sonu ilkesi

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi

Office 365 grupları süre sonu ilkesi, üyeleri tarafından etkin olarak kullanılan grupları otomatik olarak yenilemek üzere geliştirilmiştir. Gruplar, Outlook, SharePoint ve takımlar dahil olmak üzere tüm Office 365 uygulamaları genelinde Kullanıcı etkinliğine göre autorenewed olacaktır.

Bu geliştirme, Grup süre sonu bildirimlerinizi azaltmaya yardımcı olur ve etkin grupların kullanılabilir olmaya devam etmesine yardımcı olur. Office 365 gruplarınız için zaten etkin bir süre sonu ilkeniz varsa, bu yeni işlevselliği açmak için herhangi bir şey yapmanız gerekmez.

Daha fazla bilgi için bkz. [Office 365 grupları için süre sonu Ilkesini yapılandırma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-lifecycle).

---

### <a name="updated-azure-ad-domain-services-azure-ad-ds-creation-experience"></a>Azure AD Domain Services (Azure AD DS) oluşturma deneyimi güncelleştirildi

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün özelliği:** Azure AD Domain Services

Azure AD Domain Services (Azure AD DS) yeni ve geliştirilmiş bir oluşturma deneyimi içerecek şekilde güncelleştirdik ve yalnızca üç tıklamayla yönetilen bir etki alanı oluşturmanıza yardımcı olur! Ayrıca, artık Azure AD DS bir şablondan karşıya yükleyebilir ve dağıtabilirsiniz.

Daha fazla bilgi için bkz. [öğretici: Azure Active Directory Domain Services örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance).

---

## <a name="september-2019"></a>Eylül 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Değişiklik planı: Power BI içerik paketlerinin kullanımdan kaldırılması

**Türü:** değişiklik planı  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama

1 Ekim 2019 ' den başlayarak Power BI, Azure AD Power BI içerik paketi de dahil olmak üzere tüm içerik paketlerini kullanımdan kaldırmaya başlayacaktır. Bu içerik paketine alternatif olarak Azure AD çalışma kitaplarını kullanarak Azure AD ile ilgili hizmetleriniz hakkında öngörüler elde edebilirsiniz. Yalnızca rapor modundaki koşullu erişim ilkeleriyle ilgili çalışma kitapları, uygulama onayı tabanlı Öngörüler ve daha fazlası dahil olmak üzere ek çalışma kitapları geliyor.

Çalışma kitapları hakkında daha fazla bilgi için bkz. [Azure izleyici çalışma kitaplarını Azure Active Directory raporları için kullanma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks). İçerik paketlerinin kullanımdan kaldırılması hakkında daha fazla bilgi için bkz. [Power BI Template Apps genel kullanılabilirliği](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/) blog gönderisi.

---

### <a name="my-profile-is-renaming-and-integrating-with-the-microsoft-office-account-page"></a>Profilimi Microsoft Office hesabı sayfasıyla yeniden adlandırma ve Tümleştirme

**Türü:** değişiklik planı  
**Hizmet kategorisi:** Profilim/Hesabım  
**Ürün özelliği:** işbirliği

Ekim 'den itibaren profilim deneyimi Hesabım olur. Bu değişikliğin bir parçası olarak, şu anda şöyle ki, **Profilim** **Hesabım**olarak değişecektir. Adlandırma değişikliğinin en üstünde ve bazı tasarım geliştirmelerinden, güncelleştirilmiş deneyim Microsoft Office hesabı sayfasıyla ek tümleştirme sunacak. Özellikle, **genel bakış hesabı** sayfasından Office yüklemelerine ve aboneliklerine, **Gizlilik** sayfasından Office ile ilgili iletişim tercihlerini de erişebileceksiniz.

Profilim (Önizleme) deneyimi hakkında daha fazla bilgi için bkz. [Profilim (Önizleme) portalına genel bakış](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview).

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>Azure AD portalındaki CSV dosyalarını kullanarak grupları ve üyeleri toplu olarak yönetme (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** işbirliği

Azure AD portalındaki toplu Grup Yönetimi deneyimlerinin genel önizleme kullanılabilirliğine duyurmaktan mutluluk duyuyoruz. Artık aşağıdakiler dahil olmak üzere grupları ve üye listelerini yönetmek için bir CSV dosyası ve Azure AD portalını kullanabilirsiniz:

- Gruptan üye ekleme veya kaldırma.

- Dizindeki grupların listesi indiriliyor.

- Belirli bir grup için Grup üyeleri listesi indiriliyor.

Daha fazla bilgi için bkz. [toplu üye ekleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), [üyeleri toplu kaldırma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members), [üyeleri toplu indirme listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members)ve [Toplu indirme grupları listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download).

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>Dinamik izin artık yeni bir yönetici onay uç noktası aracılığıyla destekleniyor

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Microsoft Identity platformunda dinamik izin modelini kullanmak isteyen uygulamalar için yararlı olan dinamik izni desteklemek için yeni bir yönetici onay uç noktası oluşturduk.

Bu yeni uç noktanın nasıl kullanılacağı hakkında daha fazla bilgi için, bkz. [Yönetici onay uç noktası kullanma](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları mevcuttur-Eylül 2019

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Eylül 2019 ' de, uygulama galerisine federasyon desteğiyle bu 29 yeni uygulamaları ekledik:

[Schedulelook](https://schedulelook.bbsonlineservices.net/), [MS Azure SSO erişimi, Ahidex uyumluluk ofisi™-çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/ms-azure-sso-access-for-ethidex-compliance-office-tutorial), [Iver portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/iserver-portal-tutorial), [ufuk sitesi](https://docs.microsoft.com/azure/active-directory/saas-apps/skysite-tutorial), [Concur seyahat ve harcama](https://docs.microsoft.com/azure/active-directory/saas-apps/concur-travel-and-expense-tutorial), [çalışma panosu](https://docs.microsoft.com/azure/active-directory/saas-apps/workboard-tutorial), [yeeflow](https://apps.yeeflow.com/), [yay olanakları](https://docs.microsoft.com/azure/active-directory/saas-apps/arc-facilities-tutorial), [Luware stra_s ekibi](https://stratus.emea.luware.cloud/login), [geniş fikirler](https://wideideas.online/wideideas/), [Prma bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [jdlt Client hub](https://clients.jdlt.co.uk/login), [renraku](https://docs.microsoft.com/azure/active-directory/saas-apps/renraku-tutorial), [sealpath Secure Browser](https://protection.sealpath.com/SealPathInterceptorWopiSaas/Open/InstallSealPathEditorOneDrive), [prma bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [penneo](https://app.penneo.com/), [hireven](https://app.testhtm.com/settings/email-integration), [iş](https://aec.cintoo.com/login)akışı [Beyaz kaynak](https://docs.microsoft.com/azure/active-directory/saas-apps/whitesource-tutorial), [barındırılan Herısat çevrimiçi SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-heritage-online-sso-tutorial), [IDC](https://docs.microsoft.com/azure/active-directory/saas-apps/idc-tutorial), [cakehr](https://docs.microsoft.com/azure/active-directory/saas-apps/cakehr-tutorial), [\,](https://docs.microsoft.com/azure/active-directory/saas-apps/bis-tutorial) [COO Kai ekip derlemesi](https://ms-contacts.coo-kai.jp/), [sonarqude](https://docs.microsoft.com/azure/active-directory/saas-apps/sonarqube-tutorial), [Adobe Identity Management](https://docs.microsoft.com/azure/active-directory/saas-apps/adobe-identity-management-tutorial), [Discovery avantajları SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/discovery-benefits-sso-tutorial), [Amelio](https://app.amelio.co/), [itask](https://itask.yipinapp.com/)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-global-reader-role"></a>Yeni Azure AD Genel okuyucu rolü

**Türü:** yeni özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün özelliği:** erişim denetimi

24 Eylül 2019 ' den itibaren, genel okuyucu adlı yeni bir Azure Active Directory (AD) rolü kullanıma sunacağız. Bu piyasaya çıkma üretim ve küresel bulut müşterileri (GCC) ile başlar ve Ekim 'de dünya çapında bir işlem görür.

Genel okuyucu rolü, genel yöneticiye salt okunurdur. Bu roldeki kullanıcılar Microsoft 365 hizmetleri genelinde ayarları ve yönetim bilgilerini okuyabilir, ancak yönetim eylemleri alamaz. Kuruluşunuzdaki genel yönetici sayısını azaltmaya yardımcı olmak için genel okuyucu rolünü oluşturduk. Genel yönetici hesapları güçlü ve saldırılara karşı savunmasız olduğundan, beşten az genel yönetici olmasını öneririz. Planlama, denetim veya araştırmalar için genel okuyucu rolünü kullanmanızı öneririz. Ayrıca, küresel bir yönetici rolü gerekmeden çalışmanın yapılmasına yardımcı olması için Exchange Yöneticisi gibi diğer sınırlı yönetici rolleriyle birlikte genel okuyucu rolünü kullanmanızı öneririz.

Genel okuyucu rolü, yeni Microsoft 365 Yönetim Merkezi, Exchange Yönetim Merkezi, takımlar Yönetim Merkezi, Güvenlik Merkezi, Uyumluluk Merkezi, Azure AD Yönetim Merkezi ve cihaz yönetimi Yönetim Merkezi ile birlikte kullanılabilir.

>[!NOTE]
> Genel Önizleme başlangıcında, genel okuyucu rolü şu şekilde çalışmaz: SharePoint, Privileged Access Management, Müşteri Kasası, duyarlılık etiketleri, takımlar yaşam döngüsü, takımlar raporlama & Çağrı Analizi, takımlar IP telefon cihazı yönetimi ve takımlar uygulaması Kataloglarını. Bu hizmetlerin tümü, gelecekte rolle çalışmak üzere tasarlanmıştır.

Daha fazla bilgi için [Azure Active Directory Içindeki yönetici rolü izinleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)bölümüne bakın.

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama Ara Sunucusu kullanarak Power BI Mobil uygulamanızdan şirket içi rapor sunucusuna erişin

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama ara sunucusu  
**Ürün özelliği:** erişim denetimi

Power BI Mobile App ile Azure AD Uygulama Ara Sunucusu arasında yeni tümleştirme, Power BI mobil uygulamada güvenli bir şekilde oturum açmanızı ve şirket içi Power BI Rapor Sunucusu barındırılan kuruluşunuzun tüm raporlarını görüntülemenize olanak sağlar.

Uygulamanın indirileceği dahil Power BI Mobil uygulama hakkında daha fazla bilgi için [Power BI sitesine](https://powerbi.microsoft.com/mobile/)bakın. Power BI mobil uygulamayı Azure AD Uygulama Ara Sunucusu ile ayarlama hakkında daha fazla bilgi için bkz. [azure ad uygulama ara sunucusu ile Power BI mobil uzaktan erişimi etkinleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi).

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>AzureADPreview PowerShell modülünün yeni sürümü kullanılabilir

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** dizini

Yeni cmdlet 'ler, Azure AD 'de aşağıdakiler de dahil olmak üzere özel rolleri tanımlamaya ve atamaya yardımcı olmak için AzureADPreview modülüne eklenmiştir:

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>Azure AD Connect yeni sürümü

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** dizini

Otomatik yükseltme müşterileri için Azure AD Connect güncelleştirilmiş bir sürümünü yayımladık. Bu yeni sürüm bazı yeni özellikler, geliştirmeler ve hata düzeltmeleri içerir. Bu yeni sürüm hakkında daha fazla bilgi için bkz. [Azure AD Connect: sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250).

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Azure Multi-Factor Authentication (MFA) sunucusu, sürüm 8.0.2 artık kullanılabilir

**Türü:** düzeltildi  
**Hizmet kategorisi:** MFA  
**Ürün özelliği:** kimlik güvenliği ve koruması

MFA sunucusunu 1 Temmuz 2019 ' den önce etkinleştiren mevcut bir müşteriyseniz, artık MFA sunucusu 'nun en son sürümünü (sürüm 8.0.2) indirebilirsiniz. Bu yeni sürümde şunları yaptık:

- Bir sorun düzeltildi böylece Azure AD eşitleme bir kullanıcıyı devre dışı durumundan etkin olarak değiştirdiğinde kullanıcıya bir e-posta gönderilir.

- Etiketler işlevini kullanmaya devam ederken müşterilerin başarılı bir şekilde yükseltebilmesi için bir sorun düzeltildi.

- Kosovo (+ 383) ülke kodu eklendi.

- MultiFactorAuthSvc. log dosyasına bir kerelik atlama denetim günlüğü eklendi.

- Web hizmeti SDK 'sının performansı geliştirildi.

- Diğer küçük hatalar düzeltildi.

Microsoft, 1 Temmuz 2019 ' den itibaren Yeni dağıtımlar için MFA sunucusunu sunmayı durdurdu. Multi-Factor Authentication gerektiren yeni müşterilerin bulut tabanlı Azure Multi-Factor Authentication kullanması gerekir. Daha fazla bilgi için bkz. [bulut tabanlı Azure Multi-Factor Authentication dağıtımı planlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="august-2019"></a>Ağustos 2019

### <a name="enhanced-search-filtering-and-sorting-for-groups-is-available-in-the-azure-ad-portal-public-preview"></a>Azure AD portalında (Genel Önizleme), gruplar için gelişmiş arama, filtreleme ve sıralama kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** işbirliği

Azure AD portalındaki gelişmiş gruplar ile ilgili deneyimlerin genel önizleme kullanılabilirliğine duyurmaktan mutluluk duyuyoruz. Bu geliştirmeler, aşağıdakileri sağlayarak grupları ve üye listelerini daha iyi yönetmenize yardımcı olur:

- Gruplar listesinde alt dize arama gibi gelişmiş arama özellikleri.
- Üye ve sahip listelerindeki Gelişmiş filtreleme ve sıralama seçenekleri.
- Üye ve sahip listeleri için yeni arama özellikleri.
- Büyük gruplar için daha doğru grup sayısı.

Daha fazla bilgi için [Azure Portal grupları yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)bölümüne bakın.

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>Yeni özel roller uygulama kayıt yönetimi için kullanılabilir (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün özelliği:** erişim denetimi

Özel Roller (bir Azure AD P1 veya P2 aboneliği ile kullanılabilir) artık, belirli izinlerle rol tanımları oluşturmanıza ve ardından bu rolleri belirli kaynaklara atamaya izin vererek size ayrıntılı erişim sağlamanıza yardımcı olabilir. Şu anda, uygulama kayıtlarını yönetmek ve ardından rolü belirli bir uygulamaya atamak için izinleri kullanarak özel roller oluşturursunuz. Özel roller hakkında daha fazla bilgi için, bkz. [Azure Active Directory (Önizleme) Içinde özel yönetici rolleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview).

Şu anda görmezseniz ek izinlere veya kaynaklara ihtiyacınız varsa, [Azure geri bildirim sitenize](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) geri bildirimde bulunabilir ve isteğinizi güncelleştirme yol haritemiz olarak ekleyeceğiz.

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>Yeni sağlama günlükleri, uygulama sağlama dağıtımınızı izlemenize ve sorunlarını gidermenize yardımcı olabilir (Genel Önizleme)

**Türü:** yeni özellik  
**Hizmet kategorisi:** uygulama sağlama  
**Ürün özelliği:** kimlik yaşam döngüsü yönetimi

Kullanıcı ve grup sağlama dağıtımını izlemenize ve sorunlarını gidermenize yardımcı olacak yeni sağlama günlükleri mevcuttur. Bu yeni günlük dosyaları aşağıdakiler hakkında bilgiler içerir:

- [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial) 'da başarıyla oluşturulan gruplar
- [Amazon Web Services (AWS)](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws) öğesinden alınan roller
- [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) 'den hangi çalışanların içeri aktarılmadığı

Daha fazla bilgi için, [Azure Active Directory portalında (Önizleme) raporları sağlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)konusuna bakın.

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>Tüm Azure AD yöneticileri için yeni güvenlik raporları (genel kullanılabilirlik)

**Türü:** yeni özellik  
**Hizmet kategorisi:** kimlik koruması  
**Ürün özelliği:** kimlik güvenliği ve koruması

Varsayılan olarak, Azure AD yöneticileri, Azure AD içindeki modern güvenlik raporlarına yakında erişebilecektir. Eylül sonuna kadar, eski raporlara geri dönmek için modern güvenlik raporlarının en üstündeki başlığı kullanabileceksiniz.

Modern güvenlik raporları, aşağıdakiler de dahil olmak üzere eski sürümlerden ek yetenekler sağlayacaktır:

- Gelişmiş filtreleme ve sıralama
- Eksik Kullanıcı riski gibi toplu eylemler
- Güvenliği aşılmış veya güvenli varlıkların onayı
- Risk durumu, kapsayan: risk, kapatılan, düzeltilen ve onaylanan güvenliği aşılmış
- Risk ile ilgili yeni algılamalar (Azure AD Premium aboneler için kullanılabilir)

Daha fazla bilgi için bkz. [riskli kullanıcılar](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users), [riskli oturum açma](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins)işlemleri ve [risk algılamaları](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risk-detections).

---

### <a name="user-assigned-managed-identity-is-available-for-virtual-machines-and-virtual-machine-scale-sets-general-availability"></a>Kullanıcı tarafından atanan yönetilen kimlik, sanal makineler ve sanal makine ölçek kümeleri (genel kullanılabilirlik) için kullanılabilir

**Türü:** yeni özellik  
**Hizmet kategorisi:** Azure kaynakları için Yönetilen kimlikler  
**Ürün özelliği:** geliştirici deneyimi

Kullanıcı tarafından atanan Yönetilen kimlikler artık sanal makineler ve sanal makine ölçek kümeleri için genel kullanıma sunulmuştur. Bunun bir parçası olarak Azure, Azure AD kiracısında kullanımda olan aboneliğin güvendiği ve bir veya daha fazla Azure hizmet örneğine atanabilecek bir kimlik oluşturabilir. Kullanıcı tarafından atanan Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](https://aka.ms/azuremanagedidentity).

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>Kullanıcılar bir mobil uygulama veya donanım belirteci (genel kullanım) kullanarak parolalarını sıfırlayabilir

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Self Servis parola sıfırlama  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Kuruluşunuz ile bir mobil uygulama kaydeden kullanıcılar artık Microsoft Authenticator uygulamasından bir bildirim seçerek veya mobil uygulamasından veya donanım belirtecinden bir kod girerek kendi parolalarını sıfırlayabilir.

Daha fazla bilgi için bkz. [nasıl çalıştığını öğrenin: Azure AD self servis parola sıfırlama](https://aka.ms/authappsspr). Kullanıcı deneyimi hakkında daha fazla bilgi için bkz. [kendi iş veya okul parolanızı sıfırlama genel bakış](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview).

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET, yerinde senaryolar için MSAL.NET paylaşılan önbelleğini yoksayar

**Türü:** düzeltildi  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün özelliği:** kullanıcı kimlik doğrulaması

Azure AD kimlik doğrulama kitaplığı (ADAL.NET) sürüm 5.0.0-Preview sürümünden itibaren, uygulama geliştiricileri [Web uygulamaları ve Web API 'leri için hesap başına bir önbellek serileştirmelidir](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api). Aksi takdirde, bazı belirli kullanım durumları ile birlikte, `UserAssertion`[Şirket adına](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow)kullanan bazı senaryolar ayrıcalık yükselmesine neden olabilir. Bu güvenlik açığından kaçınmak için, ADAL.NET artık, için Microsoft kimlik doğrulama kitaplığı için DotNet (MSAL.NET) paylaşılan önbellek için Microsoft kimlik doğrulama kitaplığını yok sayar.

Bu sorun hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik doğrulaması kitaplığı ayrıcalık yükselmesi güvenlik açığı](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ağustos 2019

**Türü:** yeni özellik  
**Hizmet kategorisi:** kurumsal uygulamalar  
**Ürün özelliği:** 3. taraf tümleştirme

Ağustos 2019 ' de, uygulama galerisine federasyon desteğiyle bu 26 yeni uygulamalar ekledik:

[Cıvic platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [bilitodoks](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [viareport 'ın Inativ Portal (Avrupa)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [deneme](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), akademik katılım, [Öncelik matrisi](https://sync.appfluence.com/pmwebng/), [cousto MySpace](https://cousto.platformers.be/account/login) [](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial), [uploadınspecm](https://uploadcare.com/accounts/signup/), [](https://www.f5.com/products/security/access-policy-manager) [](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial) [Carbonite uç noktası](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial) [™](https://portal.deliver.media)yedeklemesi [](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial) [](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial) [ Connect](https://www.stashcat.com), [BLINK](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [renklere göre izleme](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial) [mana, EAB, stratejik bakım](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>AzureAD PowerShell ve AzureADPreview PowerShell modüllerinin yeni sürümleri mevcuttur

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** diğer  
**Ürün özelliği:** dizini

AzureAD ve AzureAD Preview PowerShell modüllerine yönelik yeni güncelleştirmeler mevcuttur:

- AzureAD modülündeki `Get-AzureADDirectoryRole` parametresine yeni bir `-Filter` parametresi eklendi. Bu parametre, cmdlet tarafından döndürülen dizin rollerini filtrelemenize yardımcı olur.
- Yeni cmdlet 'ler, Azure AD 'de aşağıdakiler de dahil olmak üzere özel rolleri tanımlamaya ve atamaya yardımcı olmak için AzureADPreview modülüne eklenmiştir:

    - `Get-AzureADMSRoleAssignment`
    - `Get-AzureADMSRoleDefinition`
    - `New-AzureADMSRoleAssignment`
    - `New-AzureADMSRoleDefinition`
    - `Remove-AzureADMSRoleAssignment`
    - `Remove-AzureADMSRoleDefinition`
    - `Set-AzureADMSRoleDefinition`

---

### <a name="improvements-to-the-ui-of-the-dynamic-group-rule-builder-in-the-azure-portal"></a>Azure portal dinamik grup kuralı oluşturucusunun Kullanıcı arabirimine yönelik iyileştirmeler

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün özelliği:** işbirliği

Yeni bir kuralı daha kolay ayarlamanıza veya mevcut kuralları değiştirmenize yardımcı olması için Azure portal kullanılabilir olan dinamik grup kuralı Oluşturucusu 'nda bazı Kullanıcı arabirimi geliştirmeleri yaptık. Bu tasarım geliştirmesi, yalnızca bir tane yerine en fazla beş ifadeye sahip kurallar oluşturmanıza olanak sağlar. Ayrıca, kullanım dışı bırakılan cihaz özelliklerini kaldırmak için cihaz özelliği listesini güncelleştirdik.

Daha fazla bilgi için bkz. [dinamik üyelik kurallarını yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership).

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>Yeni Microsoft Graph uygulama izni, erişim gözden geçirmeleri ile kullanıma sunuldu

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** erişim gözden geçirmeleri  
**Ürün yeteneği:** Kimlik yönetimi

Uygulamaların grup üyelikleri ve uygulama atamaları için otomatik olarak erişim gözden geçirmeleri oluşturmasına ve almasına izin veren yeni bir Microsoft Graph uygulama izni (`AccessReview.ReadWrite.Membership`) sunuyoruz. Bu izin, zamanlanmış işleriniz veya Otomasyon kapsamında oturum açmış bir kullanıcı bağlamı gerekmeden kullanılabilir.

Daha fazla bilgi için bkz. [PowerShell blogu ile Microsoft Graph uygulama izinleri kullanarak Azure AD erişim incelemeleri oluşturma örneği](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241).

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Azure AD etkinlik günlükleri artık Azure Izleyici 'de kamu bulutu örnekleri için kullanılabilir

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** raporlama  
**Ürün özelliği:** izleme ve Raporlama

Azure AD etkinlik günlüklerinin artık Azure Izleyici 'deki kamu bulutu örnekleri için kullanılabilir olduğunu duyurmaktan mutluluk duyuyoruz. Artık [SumoLogic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk)ve [arctıma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight)gibi SIEM araçlarınızla tümleştirebilmek için depolama hesabınıza veya bir olay hub 'ına Azure AD günlükleri gönderebilirsiniz. 

Azure Izleyici 'yi ayarlama hakkında daha fazla bilgi için bkz. Azure [izleyici 'de Azure ad etkinlik günlükleri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations).

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Kullanıcılarınızı yeni, geliştirilmiş güvenlik bilgileri deneyimiyle güncelleştirin

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:**  Kimlik doğrulamaları (oturum açma)   
**Ürün özelliği:** kullanıcı kimlik doğrulaması

25 Eylül 2019 ' de, Kullanıcı güvenlik bilgilerini kaydetmek ve yönetmek ve yalnızca yeni ve [geliştirilmiş sürümü](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271)açmak için eski, gelişmiş olmayan güvenlik bilgileri deneyimini kapatıyoruz. Bu, kullanıcılarınızın artık eski deneyimi kullanamayacağı anlamına gelir.

Gelişmiş güvenlik bilgileri deneyimi hakkında daha fazla bilgi için [yönetici belgelerimize](https://aka.ms/securityinfodocs) ve [Kullanıcı belgelerimize](https://aka.ms/securityinfoguide)bakın.

#### <a name="to-turn-on-this-new-experience-you-must"></a>Bu yeni deneyimi etkinleştirmek için şunları yapmanız gerekir:

1. Azure portal genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.

2. **Azure Active Directory > Kullanıcı ayarları ' na gidin > erişim paneli Önizleme özellikleri ayarlarını yönetin**.

3. Kullanıcılar, **güvenlik bilgilerini kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilir-gelişmiş** alanı, **Seçili**' i seçin ve ardından bir Kullanıcı grubu seçebilir ya da bu özelliği Kiracıdaki tüm kullanıcılar için açmak üzere **Tümünü** seçebilirsiniz.

4. \* * Kullanıcılar, güvenliği kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilir * * bilgi * * * * alanı, **hiçbiri**' ni seçin.

5. Ayarlarınızı kaydedin.

    Ayarlarınızı kaydettikten sonra eski güvenlik bilgileri deneyimine artık erişebileceksiniz.

>[!Important]
>25 Eylül 2019 tarihinden önce bu adımları tamamlamazsanız, Azure Active Directory kiracınız gelişmiş deneyim için otomatik olarak etkinleştirilir. Sorularınız varsa lütfen registrationpreview@microsoft.comadresinden bizimle iletişime geçin.

---

### <a name="authentication-requests-using-post-logins-will-be-more-strictly-validated"></a>Oturum açma işlemleri kullanan kimlik doğrulama istekleri daha kesin olarak doğrulanacak

**Türü:** değiştirilen özellik  
**Hizmet kategorisi:** kimlik doğrulamaları (oturum açma bilgileri)  
**Ürün yeteneği:** Standartlarına

2 Eylül 2019 ' den itibaren POST yöntemini kullanan kimlik doğrulama istekleri HTTP standartlarına karşı daha kesin olarak onaylanır. Özellikle, boşluklar ve çift tırnak işaretleri (") artık istek formu değerlerinden kaldırılmayacak. Bu değişikliklerin mevcut istemcileri bozmak beklenmez ve Azure AD 'ye gönderilen isteklerin her seferinde güvenilir bir şekilde işlendiğinden emin olmaya yardımcı olur.

Daha fazla bilgi için bkz. [Azure AD bölünmesi değişiklikleri bildirimleri](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored).

---
