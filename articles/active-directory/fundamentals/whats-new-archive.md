---
title: Azure Active Directory yenilikler için arşiv var mı? | Microsoft Belgeleri
description: Bu içerik kümesinin Genel Bakış bölümündeki yenilikler sürüm notları, 6 aylık etkinlik içerir. 6 ay sonra, öğeler ana makaleden kaldırılır ve bu arşiv makalesine yerleştirilir.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan, has-adal-ref
ms.collection: M365-identity-device-management
ms.openlocfilehash: e27adde84f62f8c8d79389e1de4e867439d5a3be
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87034746"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Azure Active Directory yenilikler için arşiv var mı?

[Azure Active Directory 'deki yenilikler nelerdir? sürüm notları](whats-new.md) makalesi, son altı aya yönelik güncelleştirmeleri içerir, ancak bu makalede daha eski bilgiler yer alır.

Azure Active Directory yenilikler nelerdir? sürüm notları hakkında bilgi sağlar:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik
- Değişiklik planları

---

## <a name="december-2019"></a>Aralık 2019

### <a name="integrate-sap-successfactors-provisioning-into-azure-ad-and-on-premises-ad-public-preview"></a>SAP başarılı faktörleri sağlamasını Azure AD 'ye ve şirket içi AD 'ye tümleştirin (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Artık SAP başarılı faktörleri, Azure AD 'de yetkili kimlik kaynağı olarak tümleştirebilirsiniz. Bu tümleştirme, Azure AD hesaplarının sağlanmasını denetlemek için yeni işe alımlar veya sonlandırmalar gibi İK tabanlı olayları kullanma dahil olmak üzere uçtan uca kimlik yaşam döngüsünü otomatikleştirmenize yardımcı olur.

SAP 'nin Azure AD 'ye gelen sağlama için başarılı bir şekilde nasıl ayarlanacağı hakkında daha fazla bilgi için bkz. [SAP başarılı etmenleri yapılandırma otomatik sağlama](https://aka.ms/SAPSuccessFactorsInboundTutorial) öğreticisi.

---

### <a name="support-for-customized-emails-in-azure-ad-b2c-public-preview"></a>Azure AD B2C 'de özelleştirilmiş e-postalar için destek (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Artık kullanıcılarınız uygulamalarınızı kullanmak üzere kaydolduğunuzda özelleştirilmiş e-postalar oluşturmak için Azure AD B2C kullanabilirsiniz. DisplayControls (Şu anda önizleme aşamasında) ve bir üçüncü taraf e-posta sağlayıcısı (örneğin, [SendGrid](https://sendgrid.com/), [mini](https://sparkpost.com/)posta veya özel bir REST API) kullanarak kendi e-posta şablonunuzu **, adres ve** konu metinlerinizi kullanabilir ve yerelleştirme ve özel bir kerelik parola (OTP) ayarlarını destekleyebilirsiniz.

Daha fazla bilgi için bkz. [Azure Active Directory B2C özel e-posta doğrulaması](https://docs.microsoft.com/azure/active-directory-b2c/custom-email).

---

### <a name="replacement-of-baseline-policies-with-security-defaults"></a>Ana hat ilkelerinin güvenlik varsayılanlarını değiştirme

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Kimlik doğrulaması için güvenli bir varsayılan model kapsamında, tüm kiracılardan var olan temel koruma ilkelerini kaldırdık. Bu kaldırma işlemi Şubat 'ın sonunda tamamlanmasına yöneliktir. Bu temel koruma ilkelerinin yerini değiştirme, güvenlik varsayılanlardır. Temel koruma ilkeleri kullanıyorsanız, yeni güvenlik Varsayılanları ilkesine veya koşullu erişime taşımayı planlamanız gerekir. Bu ilkeleri kullanmadıysanız, yapmanız gereken bir işlem yoktur.

Yeni güvenlik Varsayılanları hakkında daha fazla bilgi için bkz. [güvenlik Varsayılanları nelerdir?](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) Koşullu erişim ilkeleri hakkında daha fazla bilgi için bkz. [Genel koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common).

---

## <a name="november-2019"></a>Kasım 2019

### <a name="support-for-the-samesite-attribute-and-chrome-80"></a>SameSite özniteliği ve Chrome 80 için destek

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Tanımlama bilgileri için güvenli bir varsayılan model modelinin parçası olarak, Chrome 80 tarayıcısı özniteliği olmadan tanımlama bilgilerine nasıl davrandığını değiştiriyor `SameSite` . Özniteliği belirtmeyen tüm tanımlama bilgileri `SameSite` , olarak ayarlanmış gibi değerlendirilir, bu da `SameSite=Lax` uygulamanızın bağlı olabileceği bazı etki alanları arası tanımlama bilgisi paylaşma senaryolarını engeller. Daha eski Chrome davranışını sürdürmek için, `SameSite=None` özniteliğini kullanabilir ve ek bir öznitelik ekleyebilirsiniz. bu `Secure` nedenle, siteler arası tanımlama BILGILERINE yalnızca HTTPS bağlantıları üzerinden erişilebilir. Chrome, 4 Şubat 2020 ' de bu değişikliği tamamlamaya zamanlandı.

Tüm geliştiricilerimizin bu kılavuzu kullanarak uygulamalarını test etmenizi öneririz:

- **Güvenli tanımlama bilgisi kullan** ayarının varsayılan değerini **Evet**olarak ayarlayın.

- **SameSite** özniteliği için varsayılan değeri **none**olarak ayarlayın.

- Ek bir `SameSite` **güvenli**öznitelik ekleyin.

Daha fazla bilgi için bkz. [ASP.net ve ASP.NET Core ' de yakında bulunan SameSite tanımlama bilgisi değişiklikleri](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) ve [Chrome sürüm 79 ve sonraki sürümlerinde müşteri web siteleri ve Microsoft ürün ve hizmetlerine yönelik olası](https://support.microsoft.com/help/4522904/potential-disruption-to-microsoft-services-in-chrome-beta-version-79)kesinti.

---

### <a name="new-hotfix-for-microsoft-identity-manager-mim-2016-service-pack-2-sp2"></a>Microsoft Identity Manager (MıM) 2016 Service Pack 2 (SP2) için yeni düzeltme

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Microsoft Identity Manager (MıM) 2016 Service Pack 2 (SP2) için bir düzeltme paketi paketi (Build 4.6.34.0) kullanılabilir. Bu toplu paket sorunları çözer ve "Bu güncelleştirmede eklenen sorunlar ve iyileştirmeler" bölümünde açıklanan iyileştirmeler ekler.

Daha fazla bilgi edinmek ve düzeltme paketini indirmek için, bkz. [Microsoft Identity Manager 2016 Service Pack 2 (derleme 4.6.34.0) güncelleştirme paketi kullanılabilir](https://support.microsoft.com/help/4512924/microsoft-identity-manager-2016-service-pack-2-build-4-6-34-0-update-r).

---

### <a name="new-ad-fs-app-activity-report-to-help-migrate-apps-to-azure-ad-public-preview"></a>Azure AD 'ye uygulama geçirmeye yardımcı olmak için yeni AD FS App etkinlik raporu (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Uygulamalarınızın Azure AD 'ye geçirilme yeteneğine sahip olduğunu belirlemek için, Azure portal yeni Active Directory Federasyon Hizmetleri (AD FS) (AD FS) uygulama etkinliği raporunu kullanın. Rapor tüm AD FS uygulamaları Azure AD ile uyumluluk için değerlendirir, herhangi bir sorun olup olmadığını denetler ve tek tek uygulamaları geçişe hazırlamaya yönelik rehberlik sağlar.

Daha fazla bilgi için bkz. [uygulamaları Azure AD 'ye geçirmek için AD FS uygulama etkinliği raporunu kullanma](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity).

---

### <a name="new-workflow-for-users-to-request-administrator-consent-public-preview"></a>Kullanıcıların yönetici onayı istemesi için yeni iş akışı (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** Access Control

Yeni yönetici onayı iş akışı yöneticilere yönetici onayı gerektiren uygulamalara erişim izni vermek için bir yol sağlar. Bir Kullanıcı bir uygulamaya erişmeyi dener, ancak izin sağlayamadığında, artık yönetici onayı için bir istek gönderebilirler. İstek, e-posta ile gönderilir ve Azure portal erişilebilen bir sıraya, gözden geçirenler olarak belirlenmiş tüm yöneticilere yerleştirilir. Gözden geçiren, bekleyen bir istek üzerinde işlem yaptıktan sonra, istenen kullanıcılara bu eylem bildirilir.

Daha fazla bilgi için bkz. [yönetici onayı iş akışını yapılandırma (Önizleme)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow).

---

### <a name="new-azure-ad-app-registrations-token-configuration-experience-for-managing-optional-claims-public-preview"></a>İsteğe bağlı talepleri yönetmek için yeni Azure AD Uygulaması kayıt belirteci yapılandırma deneyimi (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Geliştirici deneyimi

Azure portal yeni **Azure AD uygulaması kayıt belirteci yapılandırma** dikey penceresi artık uygulama geliştiricilerine uygulamalarına yönelik isteğe bağlı taleplerin dinamik bir listesini gösterir. Bu yeni deneyim, Azure AD uygulama geçişlerini kolaylaştırmaya ve isteğe bağlı talepler yapılandırmalarını en aza indirmeye yardımcı olur.

Daha fazla bilgi için bkz. [Azure AD uygulamanıza isteğe bağlı talepler sağlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).

---

### <a name="new-two-stage-approval-workflow-in-azure-ad-entitlement-management-public-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde yeni iki aşamalı onay iş akışı (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Yetkilendirme Yönetimi

Bir kullanıcının erişim paketine yönelik isteğini onaylaması için iki onaylayan zorunlu olmanızı sağlayan yeni bir iki aşamalı onay iş akışı sunuyoruz. Örneğin, bunu, istenen Kullanıcı yöneticisinin önce onaylaması gerekir, sonra da onaylanacak kaynak sahibi olmasını sağlayabilirsiniz. Onaylayanlardan biri onaylanmazsa erişim izni verilmez.

Daha fazla bilgi için bkz. [Azure AD yetkilendirme yönetiminde erişim paketi Için değişiklik isteği ve onay ayarları](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-request-policy).

---

### <a name="updates-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Yeni çalışma alanlarıyla birlikte uygulamalarım sayfası güncelleştirmeleri (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Artık kuruluşunuzun kullanıcılarının yenilenen uygulamalarımı görüntüleme ve bu deneyimle erişme yönteminizi özelleştirebilirsiniz. Bu yeni deneyim Ayrıca kullanıcılarınızın uygulamaları bulmasını ve düzenlemesini kolaylaştıran yeni çalışma alanları özelliğini de içerir.

Yeni uygulama deneyimi hakkında daha fazla bilgi ve çalışma alanı oluşturma hakkında daha fazla bilgi için bkz. [My Apps Portal 'da çalışma alanları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="google-social-id-support-for-azure-ad-b2b-collaboration-general-availability"></a>Azure AD B2B işbirliği için Google sosyal KIMLIK desteği (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD 'de Google sosyal kimliklerini (Gmail hesapları) kullanmaya yönelik yeni destek, kullanıcılarınız ve iş ortaklarınız için işbirliğinin daha basit olmasına yardımcı olur. Artık iş ortaklarınızın Microsoft 'a özgü yeni bir hesap oluşturması ve yönetmesi gerekmez. Microsoft ekipleri artık tüm istemcilerde ve Kiracıdaki ortak ve kiracı ile ilgili kimlik doğrulama uç noktalarında Google kullanıcılarını tam olarak desteklemektedir.

Daha fazla bilgi için bkz. [B2B Konuk kullanıcıları için kimlik sağlayıcısı olarak Google ekleme](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

### <a name="microsoft-edge-mobile-support-for-conditional-access-and-single-sign-on-general-availability"></a>Koşullu erişim ve çoklu oturum açma için Microsoft Edge mobil desteği (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

İOS ve Android 'de Microsoft Edge için Azure AD artık Azure AD çoklu oturum açma ve koşullu erişim 'i desteklemektedir:

- **Microsoft Edge çoklu oturum açma (SSO):** Çoklu oturum açma artık Azure AD 'ye bağlı tüm uygulamalar için yerel istemcilerde (Microsoft Outlook ve Microsoft Edge gibi) kullanılabilir.

- **Microsoft Edge koşullu erişimi:** Uygulama tabanlı koşullu erişim ilkeleri sayesinde, kullanıcılarınız Microsoft Edge gibi Microsoft Intune korumalı tarayıcıları kullanmalıdır.

Microsoft Edge ile koşullu erişim ve SSO hakkında daha fazla bilgi için bkz. [koşullu erişim Için Microsoft Edge mobil desteği ve çoklu oturum açma, genel kullanıma açık](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Microsoft-Edge-Mobile-Support-for-Conditional-Access-and-Single/ba-p/988179) blog gönderisi. [Uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) veya [cihaz tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)kullanarak istemci uygulamalarınızı ayarlama hakkında daha fazla bilgi için, bkz. [Microsoft Intune İlkeyle korunan tarayıcı kullanarak Web erişimini yönetme](https://docs.microsoft.com/intune/apps/app-configuration-managed-browser).

---

### <a name="azure-ad-entitlement-management-general-availability"></a>Azure AD Yetkilendirme Yönetimi (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Yetkilendirme Yönetimi

Azure AD Yetkilendirme Yönetimi, kuruluşların kimlik ve erişim yaşam döngüsünü ölçekli olarak yönetmesine yardımcı olan yeni bir kimlik idare özelliğidir. Bu yeni özellik, erişim isteği iş akışlarını otomatikleştirerek, gruplar, uygulamalar ve SharePoint Online siteleri arasında atamaları, İncelemeleri ve kullanım süresinin tamamlanmasına yardımcı olur.

Azure AD yetkilendirme yönetimi sayesinde, hem çalışanlar hem de kuruluşunuzun dışındaki kullanıcılar için erişimi daha verimli bir şekilde yönetebilirsiniz.

Daha fazla bilgi için bkz. [Azure AD yetkilendirme yönetimi nedir?](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview#license-requirements)

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi  

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

[SAP Cloud Platform kimliği kimlik doğrulama hizmeti](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-provisioning-tutorial), [spaceiq](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-provisioning-tutorial), [mıro](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-provisioning-tutorial), [cloudgate](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-provisioning-tutorial), [Infor cloudsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloudsuite-provisioning-tutorial), [officesi yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-software-provisioning-tutorial), [Öncelik matrisi](https://docs.microsoft.com/azure/active-directory/saas-apps/priority-matrix-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Kasım 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Kasım 'da, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[Airtable](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [HootSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [Üyeler için mavi erişim (BAI)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [Bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [reslife portalı](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal çoklu oturum açma (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), [teamschamp](https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279), [motlar](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial), [myaryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial), [Mave-posta](https://loginself1.bluemail.me/), [Beedle](https://teams-web.beedle.co/#/), [Visma](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial) [Claromentis](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial), [onedesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [Foko perakende](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [gpazarlar fikir & yenilik yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [netüse Kullanıcı kimliği](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial) [uniFLOW Online](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial) [Jisc Student Voter Registration](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial) [e4enable](https://portal.e4enable.com/)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-and-improved-azure-ad-application-gallery"></a>Yeni ve geliştirilmiş Azure AD Uygulama Galerisi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Azure Active Directory kiracınızda sağlamayı, OpenID Connect ve SAML 'yi destekleyen önceden tümleştirilmiş uygulamaları bulmanızı kolaylaştırmak için Azure AD uygulama galerisini güncelleştirdik.

Daha fazla bilgi için bkz. [Azure Active Directory kiracınıza uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal).

---

### <a name="increased-app-role-definition-length-limit-from-120-to-240-characters"></a>120 karakterden 240 karaktere daha fazla uygulama rolü tanımı uzunluğu sınırı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Müşterilerin bazı uygulama ve hizmetlerdeki uygulama rolü tanımı değeri için uzunluk sınırının 120 karakterden fazla kısa olduğunu duyduk. Yanıt olarak, rol değeri tanımının en büyük uzunluğunu 240 karakter olarak artırdık.

Uygulamaya özel rol tanımlarını kullanma hakkında daha fazla bilgi için bkz. [uygulamanıza uygulama rolleri ekleme ve bunları belirtece alma](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps).

---

## <a name="october-2019"></a>Ekim 2019

### <a name="deprecation-of-the-identityriskevent-api-for-azure-ad-identity-protection-risk-detections"></a>Azure AD Kimlik Koruması risk algılamaları için ıdentityriskevent API 'sinin kullanımdan kaldırılması

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Geliştirici geri bildirimlerine yanıt olarak Azure AD Premium P2 aboneleri artık Microsoft Graph için yeni riskDetection API 'sini kullanarak Azure AD Kimlik Koruması riskli algılama verilerinde karmaşık sorgular gerçekleştirebilir. Mevcut [ıdentityriskevent](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta) API beta sürümü **10 Ocak 2020 '** den itibaren veri döndürmeyi durdurur. Kuruluşunuz ıdentityriskevent API 'sini kullanıyorsa, yeni riskDetection API 'sine geçiş yapmanız gerekir.

Yeni riskDetection API 'SI hakkında daha fazla bilgi için [risk algılama API 'si başvuru belgelerine](https://aka.ms/RiskDetectionsAPI)bakın.

---

### <a name="application-proxy-support-for-the-samesite-attribute-and-chrome-80"></a>SameSite özniteliği ve Chrome 80 için uygulama ara sunucusu desteği

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** uygulama proxy 'si **ürün yeteneği:** Access Control

Chrome 80 tarayıcı sürümünden önceki birkaç hafta önce, uygulama proxy tanımlama bilgilerinin **SameSite** özniteliğini nasıl değerlendirdiğinin güncelleştirilmesini planlıyoruz. Chrome 80 sürümü ile, **SameSite** özniteliğini belirtmeyen tüm tanımlama bilgileri, olarak ayarlanmış olsa da olarak kabul edilir `SameSite=Lax` .

Bu değişiklik nedeniyle olumsuz olumsuz etkileri önlemeye yardımcı olmak için uygulama proxy 'Si erişimi ve oturum tanımlama bilgilerini şu şekilde güncelleştiriyoruz:

- **Güvenli tanımlama bilgisi kullan** ayarı için varsayılan değer **Evet**olarak ayarlanıyor.

- **SameSite** özniteliği için varsayılan değer **none**olarak ayarlanıyor.

    >[!NOTE]
    > Uygulama proxy 'Si erişimi tanımlama bilgileri her zaman güvenli kanallar üzerinden iletilir. Bu değişiklikler yalnızca oturum tanımlama bilgileri için geçerlidir.

Uygulama proxy 'Si tanımlama bilgisi ayarları hakkında daha fazla bilgi için bkz. [Azure Active Directory ' de şirket içi uygulamalara erişmek Için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="app-registrations-legacy-and-app-management-in-the-application-registration-portal-appsdevmicrosoftcom-is-no-longer-available"></a>Uygulama kayıt portalındaki (apps.dev.microsoft.com) Uygulama kayıtları (eski) ve uygulama yönetimi artık kullanılamıyor

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** yok **ürün yeteneği:** geliştirici deneyimi

Azure AD hesapları olan kullanıcılar artık uygulama kayıt portalı 'nı (apps.dev.microsoft.com) kullanarak uygulamaları kaydedemez veya yönetemez veya Azure portal Uygulama kayıtları (eski) deneyimdeki uygulamaları kaydedebilir ve yönetebilir.

Yeni Uygulama kayıtları deneyimi hakkında daha fazla bilgi edinmek için [Azure Portal eğitim kılavuzundaki uygulama kayıtları](../develop/app-registrations-training-guide-for-app-registrations-legacy-users.md)bakın.

---

### <a name="users-are-no-longer-required-to-re-register-during-migration-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcı başına MFA 'dan koşullu erişim tabanlı MFA 'ya geçiş sırasında kullanıcıların artık yeniden kaydedilmesi gerekmez

**Şunu yazın:** Sabit **hizmet kategorisi:** MFA **ürün yeteneği:** kimlik güvenliği & koruması

Kullanıcıların Kullanıcı başına Multi-Factor Authentication (MFA) için devre dışı bırakıldıklarında ve sonra koşullu erişim ilkesi aracılığıyla MFA için etkinleştirildiklerinde yeniden kaydolmaları gereken bilinen bir sorunu düzelttik.

Kullanıcıların yeniden kaydolmesini gerektirmek için, Azure AD portalındaki kullanıcının kimlik doğrulama yöntemlerinden **gereken MFA 'yı yeniden kaydet** seçeneğini belirleyebilirsiniz. Kullanıcı başına MFA 'dan koşullu erişim tabanlı MFA 'ya Kullanıcı geçirme hakkında daha fazla bilgi için bkz. [Kullanıcı BAŞıNA MFA 'Dan koşullu erişim tabanlı MFA 'ya Kullanıcı dönüştürme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted#convert-users-from-per-user-mfa-to-conditional-access-based-mfa).

---

### <a name="new-capabilities-to-transform-and-send-claims-in-your-saml-token"></a>SAML belirtecinizdeki talepleri dönüştürmek ve göndermek için yeni yetenekler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

SAML belirteçinizdeki talepleri özelleştirmenize ve göndermenize yardımcı olacak ek yetenekler ekledik. Bu yeni yetenekler şunlardır:

- Ek talep dönüştürme işlevleri, talepte göndereceğiniz değeri değiştirmenize yardımcı olur.

- Tek bir talebe birden çok dönüşüm uygulama özelliği.

- Kullanıcı türüne ve kullanıcının ait olduğu gruba göre talep kaynağını belirtme özelliği.

Bu yeni yetenekler hakkında daha fazla bilgi için, bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="new-my-sign-ins-page-for-end-users-in-azure-ad"></a>Azure AD 'de son kullanıcılar için yeni oturum açma görevlerim sayfası

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** izleme & raporlama

Yeni **oturum açma** işlemleri sayfası ekledik ( https://mysignins.microsoft.com) kuruluşunuzun kullanıcılarına olağan dışı etkinlikleri denetlemek için en son oturum açma geçmişini görüntülemesine olanak tanır. Bu yeni sayfa, kullanıcılarınızın şunları görmesini sağlar:

- Herkes parolasını tahmin etmeye çalışıyorsa.

- Bir saldırgan hesabında ve bu konumdan başarıyla oturum açtı.

- Saldırganın erişmeye çalıştığı uygulamalar.

Daha fazla bilgi için, bkz. [Kullanıcılar artık olağan dışı etkinlik bloguna ait oturum açma geçmişini denetleyebilir](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Users-can-now-check-their-sign-in-history-for-unusual-activity/ba-p/916066) .

---

### <a name="migration-of-azure-ad-domain-services-azure-ad-ds-from-classic-to-azure-resource-manager-virtual-networks"></a>Azure AD Domain Services (Azure AD DS) klasik 'ten Azure Resource Manager sanal ağlara geçirme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD Domain Services **ürün özelliği:** Azure AD Domain Services

Klasik sanal ağlarda takılı olan müşterilerimiz için çok güzel bir haberimiz var! Artık, klasik bir sanal ağdan mevcut bir Kaynak Yöneticisi sanal ağa tek seferlik bir geçiş gerçekleştirebilirsiniz. Kaynak Yöneticisi sanal ağa taşıdıktan sonra, hassas parola ilkeleri, e-posta bildirimleri ve denetim günlükleri gibi ek ve yükseltilmiş özelliklerden yararlanabilirsiniz.

Daha fazla bilgi için bkz. [Klasik sanal ağ modelinden Kaynak Yöneticisi Için önizleme-geçirme Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/migrate-from-classic-vnet).

---

### <a name="updates-to-the-azure-ad-b2c-page-contract-layout"></a>Azure AD B2C sayfa sözleşmesi düzenine yönelik güncelleştirmeler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Azure AD B2C için sayfa sözleşmesinin sürüm 1.2.0 için bazı yeni değişiklikler ekledik. Bu güncelleştirilmiş sürümde, artık öğeleriniz için yükleme sırasını denetleyebilir ve bu da, stil sayfası (CSS) yüklenirken gerçekleşen titreşimi durdurmaya da yardımcı olabilir.

Sayfa sözleşmesinde yapılan değişikliklerin tam listesi için bkz. [sürüm değişiklik günlüğü](https://docs.microsoft.com/azure/active-directory-b2c/page-layout#120).

---

### <a name="update-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Yeni çalışma alanlarıyla birlikte uygulamalarım sayfasına güncelleştir (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulamalarım **ürün yeteneği:** Access Control

Artık, kuruluşunuzun kullanıcılarının, uygulamaları bulmasını kolaylaştırmak için yeni çalışma alanları özelliğini kullanma dahil olmak üzere yepyeni uygulamalara görüntüleme ve bu deneyimle erişme biçimini özelleştirebilirsiniz. Yeni çalışma alanları işlevselliği, kuruluşunuzun kullanıcılarının zaten erişimi olan uygulamalar için bir filtre işlevi görür.

Yeni uygulamalarım deneyimini kullanıma alma ve çalışma alanları oluşturma hakkında daha fazla bilgi için bkz. [My Apps (Önizleme) portalında çalışma alanları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="support-for-the-monthly-active-user-based-billing-model-general-availability"></a>Aylık etkin kullanıcı tabanlı faturalandırma modeli için destek (genel kullanılabilirlik)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Azure AD B2C artık aylık etkin kullanıcıların (MAU) faturalandırmasını desteklemektedir. MAU faturalandırma, bir takvim ayı sırasında kimlik doğrulama etkinliği olan benzersiz kullanıcı sayısını temel alır. Mevcut müşteriler, bu yeni faturalandırma yöntemine dilediğiniz zaman geçebilir.

1 Kasım 2019 ' den itibaren, tüm yeni müşteriler otomatik olarak bu yöntem kullanılarak faturalandırılacaktır. Bu Faturalandırma yöntemi, müşterilerin maliyet avantajları ve ilerleyebilme aracılığıyla faydalanır.

Daha fazla bilgi için bkz. [aylık etkin kullanıcıları faturalama modeline yükseltme](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing#upgrade-to-monthly-active-users-billing-model).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ekim 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Ekim 'de, uygulama galerisine federasyon desteğiyle bu 35 yeni uygulamalar ekledik:

[Çapraz-mobil](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-mobile-tutorial), [Juno ile yolculuğa](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial), [ExponentHR](https://docs.microsoft.com/azure/active-directory/saas-apps/exponenthr-tutorial), [kişi](https://tact.ai/assistant/), [opuscapita nakit yönetimi](http://cm1.opuscapita.com/tenantname), [salestim](https://prd.salestim.io/forms), [learnster](https://docs.microsoft.com/azure/active-directory/saas-apps/learnster-tutorial), [dynaTrace](https://docs.microsoft.com/azure/active-directory/saas-apps/dynatrace-tutorial), [hunchbuzz](https://login.hunchbuzz.com/integrations/azure/process), en uygun [çalışma](https://docs.microsoft.com/azure/active-directory/saas-apps/freshworks-tutorial), [ECornell](https://docs.microsoft.com/azure/active-directory/saas-apps/ecornell-tutorial), [Shiphazı](https://docs.microsoft.com/azure/active-directory/saas-apps/shiphazmat-tutorial), [NETÜSE Bulut güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial), [çekişme](https://docs.microsoft.com/azure/active-directory/saas-apps/contentful-tutorial), [bindtuning](https://bindtuning.com/login), [HireVue koordinatı – Avrupa](https://www.hirevue.com/), [HireVue koordinatı-usonly](https://www.hirevue.com/), [HireVue koordinatı-US](https://www.hirevue.com/), [WittyParrot BILGI kutusu](https://wittyapi.wittyparrot.com/wittyparrot/api/provision/trail/signup), [cloudmore](https://docs.microsoft.com/azure/active-directory/saas-apps/cloudmore-tutorial), [Visit.org](https://docs.microsoft.com/azure/active-directory/saas-apps/visitorg-tutorial), [cambium Xirrus EasyPass Portal](https://login.xirrus.com/azure-signup), [Paylocity](https://docs.microsoft.com/azure/active-directory/saas-apps/paylocity-tutorial), [posta şanslar!](https://docs.microsoft.com/azure/active-directory/saas-apps/mail-luck-tutorial), [teamie](https://theteamie.com/), [takımlar için hız](https://velocity.peakup.org/teams/login), [sıgnl4](https://account.signl4.com/manage), [EAB gidilecek Impl](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-impl-tutorial), [ekran karşılamaları](https://console.screenmeet.com/), [Omega noktası](https://pi.ompnt.com/), [konuşma](https://speaking.email/FAQ/98/email-access-via-microsoft-intune)için konuşma e-postası, [for Office 365 Direct (iPhone/Android)](https://speaking.email/FAQ/126/email-access-via-microsoft-office-365-direct), [ExactCare SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/exactcare-sso-tutorial), [ıhealthhome bakım gezinti sistemi](https://ihealthnav.com/account/signin), [qubie](https://qubie.azurewebsites.net/static/adminTab/authorize.html)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="consolidated-security-menu-item-in-the-azure-ad-portal"></a>Azure AD portalındaki birleştirilmiş güvenlik menü öğesi

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

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

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

Office 365 grupları süre sonu ilkesi, üyeleri tarafından etkin olarak kullanılan grupları otomatik olarak yenilemek üzere geliştirilmiştir. Gruplar, Outlook, SharePoint ve takımlar dahil olmak üzere tüm Office 365 uygulamaları genelinde Kullanıcı etkinliğine göre autorenewed olacaktır.

Bu geliştirme, Grup süre sonu bildirimlerinizi azaltmaya yardımcı olur ve etkin grupların kullanılabilir olmaya devam etmesine yardımcı olur. Office 365 gruplarınız için zaten etkin bir süre sonu ilkeniz varsa, bu yeni işlevselliği açmak için herhangi bir şey yapmanız gerekmez.

Daha fazla bilgi için bkz. [Office 365 grupları için süre sonu Ilkesini yapılandırma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-lifecycle).

---

### <a name="updated-azure-ad-domain-services-azure-ad-ds-creation-experience"></a>Azure AD Domain Services (Azure AD DS) oluşturma deneyimi güncelleştirildi

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Azure AD Domain Services **ürün yeteneği:** Azure AD Domain Services

Azure AD Domain Services (Azure AD DS) yeni ve geliştirilmiş bir oluşturma deneyimi içerecek şekilde güncelleştirdik ve yalnızca üç tıklamayla yönetilen bir etki alanı oluşturmanıza yardımcı olur! Ayrıca, artık Azure AD DS bir şablondan karşıya yükleyebilir ve dağıtabilirsiniz.

Daha fazla bilgi için bkz. [öğretici: Azure Active Directory Domain Services örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance).

---

## <a name="september-2019"></a>Eylül 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Değişiklik planı: Power BI içerik paketlerinin kullanımdan kaldırılması

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** raporlama **ürün yeteneği:** izleme & raporlama

1 Ekim 2019 ' den başlayarak Power BI, Azure AD Power BI içerik paketi de dahil olmak üzere tüm içerik paketlerini kullanımdan kaldırmaya başlayacaktır. Bu içerik paketine alternatif olarak Azure AD çalışma kitaplarını kullanarak Azure AD ile ilgili hizmetleriniz hakkında öngörüler elde edebilirsiniz. Yalnızca rapor modundaki koşullu erişim ilkeleriyle ilgili çalışma kitapları, uygulama onayı tabanlı Öngörüler ve daha fazlası dahil olmak üzere ek çalışma kitapları geliyor.

Çalışma kitapları hakkında daha fazla bilgi için bkz. [Azure izleyici çalışma kitaplarını Azure Active Directory raporları için kullanma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks). İçerik paketlerinin kullanımdan kaldırılması hakkında daha fazla bilgi için bkz. [Power BI Template Apps genel kullanılabilirliği](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/) blog gönderisi.

---

### <a name="my-profile-is-renaming-and-integrating-with-the-microsoft-office-account-page"></a>Profilimi Microsoft Office hesabı sayfasıyla yeniden adlandırma ve Tümleştirme

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** profil/hesap **ürün yeteneği:** işbirliği

Ekim 'den itibaren profilim deneyimi Hesabım olur. Bu değişikliğin bir parçası olarak, şu anda şöyle ki, **Profilim** **Hesabım**olarak değişecektir. Adlandırma değişikliğinin en üstünde ve bazı tasarım geliştirmelerinden, güncelleştirilmiş deneyim Microsoft Office hesabı sayfasıyla ek tümleştirme sunacak. Özellikle, **genel bakış hesabı** sayfasından Office yüklemelerine ve aboneliklerine, **Gizlilik** sayfasından Office ile ilgili iletişim tercihlerini de erişebileceksiniz.

Profilim (Önizleme) deneyimi hakkında daha fazla bilgi için bkz. [Profilim (Önizleme) portalına genel bakış](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview).

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>Azure AD portalındaki CSV dosyalarını kullanarak grupları ve üyeleri toplu olarak yönetme (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Azure AD portalındaki toplu Grup Yönetimi deneyimlerinin genel önizleme kullanılabilirliğine duyurmaktan mutluluk duyuyoruz. Artık aşağıdakiler dahil olmak üzere grupları ve üye listelerini yönetmek için bir CSV dosyası ve Azure AD portalını kullanabilirsiniz:

- Gruptan üye ekleme veya kaldırma.

- Dizindeki grupların listesi indiriliyor.

- Belirli bir grup için Grup üyeleri listesi indiriliyor.

Daha fazla bilgi için bkz. [toplu üye ekleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), [üyeleri toplu kaldırma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members), [üyeleri toplu indirme listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members)ve [Toplu indirme grupları listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download).

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>Dinamik izin artık yeni bir yönetici onay uç noktası aracılığıyla destekleniyor

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Microsoft Identity platformunda dinamik izin modelini kullanmak isteyen uygulamalar için yararlı olan dinamik izni desteklemek için yeni bir yönetici onay uç noktası oluşturduk.

Bu yeni uç noktanın nasıl kullanılacağı hakkında daha fazla bilgi için, bkz. [Yönetici onay uç noktası kullanma](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları mevcuttur-Eylül 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Eylül 2019 ' de, uygulama galerisine federasyon desteğiyle bu 29 yeni uygulamaları ekledik:

[Schedulelook](https://schedulelook.bbsonlineservices.net/), [MS Azure SSO erişimi, etidex uyumluluk ofisi &trade; -Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/ms-azure-sso-access-for-ethidex-compliance-office-tutorial), [Iver portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/iserver-portal-tutorial), [ufuk sitesi](https://docs.microsoft.com/azure/active-directory/saas-apps/skysite-tutorial), [yarışmayı ve harcama](https://docs.microsoft.com/azure/active-directory/saas-apps/concur-travel-and-expense-tutorial), [çalışma panosu](https://docs.microsoft.com/azure/active-directory/saas-apps/workboard-tutorial), `https://apps.yeeflow.com/` [yay tesislerini](https://docs.microsoft.com/azure/active-directory/saas-apps/arc-facilities-tutorial), [luware strazı ekibi](https://stratus.emea.luware.cloud/login), [geniş fikirler](https://wideideas.online/wideideas/), [przma bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [jdlt Client hub](https://clients.jdlt.co.uk/login), [renpaku](https://docs.microsoft.com/azure/active-directory/saas-apps/renraku-tutorial), [sealpath Secure Browser](https://protection.sealpath.com/SealPathInterceptorWopiSaas/Open/InstallSealPathEditorOneDrive), [przma bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), `https://app.penneo.com/` ,, işlem `https://app.testhtm.com/settings/email-integration` [çok fazla bulut](https://aec.cintoo.com/login), [beyaz kaynak](https://docs.microsoft.com/azure/active-directory/saas-apps/whitesource-tutorial), [barındırılan herials online SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-heritage-online-sso-tutorial), [IDC](https://docs.microsoft.com/azure/active-directory/saas-apps/idc-tutorial), [cakehr](https://docs.microsoft.com/azure/active-directory/saas-apps/cakehr-tutorial) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/bis-tutorial)b, [COO Kai ekip derlemesi](https://ms-contacts.coo-kai.jp/), [sonarquin](https://docs.microsoft.com/azure/active-directory/saas-apps/sonarqube-tutorial), [Adobe Identity Management](https://docs.microsoft.com/azure/active-directory/saas-apps/adobe-identity-management-tutorial), [Discovery avantajları SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/discovery-benefits-sso-tutorial), [Amelio](https://app.amelio.co/),`https://itask.yipinapp.com/`

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-azure-ad-global-reader-role"></a>Yeni Azure AD Genel okuyucu rolü

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** RBAC **ürün yeteneği:** Access Control

24 Eylül 2019 ' den itibaren, genel okuyucu adlı yeni bir Azure Active Directory (AD) rolü kullanıma sunacağız. Bu piyasaya çıkma üretim ve küresel bulut müşterileri (GCC) ile başlar ve Ekim 'de dünya çapında bir işlem görür.

Genel okuyucu rolü, genel yöneticiye salt okunurdur. Bu roldeki kullanıcılar Microsoft 365 hizmetleri genelinde ayarları ve yönetim bilgilerini okuyabilir, ancak yönetim eylemleri alamaz. Kuruluşunuzdaki genel yönetici sayısını azaltmaya yardımcı olmak için genel okuyucu rolünü oluşturduk. Genel yönetici hesapları güçlü ve saldırılara karşı savunmasız olduğundan, beşten az genel yönetici olmasını öneririz. Planlama, denetim veya araştırmalar için genel okuyucu rolünü kullanmanızı öneririz. Ayrıca, küresel bir yönetici rolü gerekmeden çalışmanın yapılmasına yardımcı olması için Exchange Yöneticisi gibi diğer sınırlı yönetici rolleriyle birlikte genel okuyucu rolünü kullanmanızı öneririz.

Genel okuyucu rolü, yeni Microsoft 365 Yönetim Merkezi, Exchange Yönetim Merkezi, takımlar Yönetim Merkezi, Güvenlik Merkezi, Uyumluluk Merkezi, Azure AD Yönetim Merkezi ve cihaz yönetimi Yönetim Merkezi ile birlikte kullanılabilir.

>[!NOTE]
> Genel Önizleme 'nin başlangıcında, genel okuyucu rolü şu şekilde çalışmaz: SharePoint, Privileged Access Management, Müşteri Kasası, duyarlılık etiketleri, takımlar yaşam döngüsü, takımlar raporlama & çağrı analizi, takımlar IP telefon cihazı yönetimi ve takımlar uygulama kataloğu.

Daha fazla bilgi için [Azure Active Directory Içindeki yönetici rolü izinleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)bölümüne bakın.

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama Ara Sunucusu kullanarak Power BI Mobil uygulamanızdan şirket içi rapor sunucusuna erişin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** Access Control

Power BI Mobile App ile Azure AD Uygulama Ara Sunucusu arasında yeni tümleştirme, Power BI mobil uygulamada güvenli bir şekilde oturum açmanızı ve şirket içi Power BI Rapor Sunucusu barındırılan kuruluşunuzun tüm raporlarını görüntülemenize olanak sağlar.

Uygulamanın indirileceği dahil Power BI Mobil uygulama hakkında daha fazla bilgi için [Power BI sitesine](https://powerbi.microsoft.com/mobile/)bakın. Power BI mobil uygulamayı Azure AD Uygulama Ara Sunucusu ile ayarlama hakkında daha fazla bilgi için bkz. [azure ad uygulama ara sunucusu ile Power BI mobil uzaktan erişimi etkinleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi).

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>AzureADPreview PowerShell modülünün yeni sürümü kullanılabilir

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** Dizin

Yeni cmdlet 'ler, Azure AD 'de aşağıdakiler de dahil olmak üzere özel rolleri tanımlamaya ve atamaya yardımcı olmak için AzureADPreview modülüne eklenmiştir:

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>Azure AD Connect yeni sürümü

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** Dizin

Otomatik yükseltme müşterileri için Azure AD Connect güncelleştirilmiş bir sürümünü yayımladık. Bu yeni sürüm bazı yeni özellikler, geliştirmeler ve hata düzeltmeleri içerir. Bu yeni sürüm hakkında daha fazla bilgi için bkz. [Azure AD Connect: sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250).

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Azure Multi-Factor Authentication (MFA) sunucusu, sürüm 8.0.2 artık kullanılabilir

**Şunu yazın:** Sabit **hizmet kategorisi:** MFA **ürün yeteneği:** kimlik güvenliği & koruması

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

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Azure AD portalındaki gelişmiş gruplar ile ilgili deneyimlerin genel önizleme kullanılabilirliğine duyurmaktan mutluluk duyuyoruz. Bu geliştirmeler, aşağıdakileri sağlayarak grupları ve üye listelerini daha iyi yönetmenize yardımcı olur:

- Gruplar listesinde alt dize arama gibi gelişmiş arama özellikleri.
- Üye ve sahip listelerindeki Gelişmiş filtreleme ve sıralama seçenekleri.
- Üye ve sahip listeleri için yeni arama özellikleri.
- Büyük gruplar için daha doğru grup sayısı.

Daha fazla bilgi için [Azure Portal grupları yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)bölümüne bakın.

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>Yeni özel roller uygulama kayıt yönetimi için kullanılabilir (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** RBAC **ürün yeteneği:** Access Control

Özel Roller (bir Azure AD P1 veya P2 aboneliği ile kullanılabilir) artık, belirli izinlerle rol tanımları oluşturmanıza ve ardından bu rolleri belirli kaynaklara atamaya izin vererek size ayrıntılı erişim sağlamanıza yardımcı olabilir. Şu anda, uygulama kayıtlarını yönetmek ve ardından rolü belirli bir uygulamaya atamak için izinleri kullanarak özel roller oluşturursunuz. Özel roller hakkında daha fazla bilgi için, bkz. [Azure Active Directory (Önizleme) Içinde özel yönetici rolleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview).

Şu anda görmezseniz ek izinlere veya kaynaklara ihtiyacınız varsa, [Azure geri bildirim sitenize](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) geri bildirimde bulunabilir ve isteğinizi güncelleştirme yol haritemiz olarak ekleyeceğiz.

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>Yeni sağlama günlükleri, uygulama sağlama dağıtımınızı izlemenize ve sorunlarını gidermenize yardımcı olabilir (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

Kullanıcı ve grup sağlama dağıtımını izlemenize ve sorunlarını gidermenize yardımcı olacak yeni sağlama günlükleri mevcuttur. Bu yeni günlük dosyaları aşağıdakiler hakkında bilgiler içerir:

- [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial) 'da başarıyla oluşturulan gruplar
- [Amazon Web Services (AWS)](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws) öğesinden alınan roller
- [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) 'den hangi çalışanların içeri aktarılmadığı

Daha fazla bilgi için, [Azure Active Directory portalında (Önizleme) raporları sağlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)konusuna bakın.

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>Tüm Azure AD yöneticileri için yeni güvenlik raporları (genel kullanılabilirlik)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

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

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure kaynakları için Yönetilen kimlikler **ürün yeteneği:** geliştirici deneyimi

Kullanıcı tarafından atanan Yönetilen kimlikler artık sanal makineler ve sanal makine ölçek kümeleri için genel kullanıma sunulmuştur. Bunun bir parçası olarak Azure, Azure AD kiracısında kullanımda olan aboneliğin güvendiği ve bir veya daha fazla Azure hizmet örneğine atanabilecek bir kimlik oluşturabilir. Kullanıcı tarafından atanan Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](https://aka.ms/azuremanagedidentity).

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>Kullanıcılar bir mobil uygulama veya donanım belirteci (genel kullanım) kullanarak parolalarını sıfırlayabilir

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** self servis parola sıfırlama **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Kuruluşunuz ile bir mobil uygulama kaydeden kullanıcılar artık Microsoft Authenticator uygulamasından bir bildirim seçerek veya mobil uygulamasından veya donanım belirtecinden bir kod girerek kendi parolalarını sıfırlayabilir.

Daha fazla bilgi için bkz. [nasıl çalıştığını öğrenin: Azure AD self servis parola sıfırlama](https://aka.ms/authappsspr). Kullanıcı deneyimi hakkında daha fazla bilgi için bkz. [kendi iş veya okul parolanızı sıfırlama genel bakış](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview).

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET, yerinde senaryolar için MSAL.NET paylaşılan önbelleğini yoksayar

**Şunu yazın:** Sabit **hizmet kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD kimlik doğrulama kitaplığı (ADAL.NET) sürüm 5.0.0-Preview sürümünden itibaren, uygulama geliştiricileri [Web uygulamaları ve Web API 'leri için hesap başına bir önbellek serileştirmelidir](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api). Aksi takdirde, bazı belirli kullanım durumları ile birlikte, [Şirket adına akışı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow)kullanan bazı senaryolar `UserAssertion` ayrıcalık yükselmesine neden olabilir. Bu güvenlik açığından kaçınmak için, ADAL.NET artık, için Microsoft kimlik doğrulama kitaplığı için DotNet (MSAL.NET) paylaşılan önbellek için Microsoft kimlik doğrulama kitaplığını yok sayar.

Bu sorun hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik doğrulaması kitaplığı ayrıcalık yükselmesi güvenlik açığı](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ağustos 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Ağustos 2019 ' de, uygulama galerisine federasyon desteğiyle bu 26 yeni uygulamalar ekledik:

[Cıvic platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [bilitodoks](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [viareport 'ın Inativ Portal (Avrupa)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [deneme](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), akademik katılım, [Öncelik matrisi](https://sync.appfluence.com/pmwebng/), [cousto MySpace](https://cousto.platformers.be/account/login), [uploadilgilenme](https://uploadcare.com/accounts/signup/), [Carbonite uç nokta yedeklemesi](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial),, [Incom ile cpqsync](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial), [chargebee](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial), [sunma. Media &trade; Portal](https://portal.deliver.media), [Frontline eğitim](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial), [F5](https://www.f5.com/products/security/access-policy-manager), [stashcat AD Connect](https://www.stashcat.com), [yanıp sönme](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [renklerin göre izleme](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial) [, ana,](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial) [EAB stratejik bakım](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial) [Academy Attendance](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>AzureAD PowerShell ve AzureADPreview PowerShell modüllerinin yeni sürümleri mevcuttur

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** Dizin

AzureAD ve AzureAD Preview PowerShell modüllerine yönelik yeni güncelleştirmeler mevcuttur:

- `-Filter` `Get-AzureADDirectoryRole` Azuread modülündeki parametreye yeni bir parametre eklendi. Bu parametre, cmdlet tarafından döndürülen dizin rollerini filtrelemenize yardımcı olur.
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

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Yeni bir kuralı daha kolay ayarlamanıza veya mevcut kuralları değiştirmenize yardımcı olması için Azure portal kullanılabilir olan dinamik grup kuralı Oluşturucusu 'nda bazı Kullanıcı arabirimi geliştirmeleri yaptık. Bu tasarım geliştirmesi, yalnızca bir tane yerine en fazla beş ifadeye sahip kurallar oluşturmanıza olanak sağlar. Ayrıca, kullanım dışı bırakılan cihaz özelliklerini kaldırmak için cihaz özelliği listesini güncelleştirdik.

Daha fazla bilgi için bkz. [dinamik üyelik kurallarını yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership).

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>Yeni Microsoft Graph uygulama izni, erişim gözden geçirmeleri ile kullanıma sunuldu

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** erişim gözden geçirmeleri **ürün yeteneği:** Identity idare

`AccessReview.ReadWrite.Membership`Uygulamaların grup üyelikleri ve uygulama atamaları için otomatik olarak erişim İncelemeleri oluşturmasına ve almasına izin veren yeni bir Microsoft Graph App izni sunuyoruz. Bu izin, zamanlanmış işleriniz veya Otomasyon kapsamında oturum açmış bir kullanıcı bağlamı gerekmeden kullanılabilir.

Daha fazla bilgi için bkz. [PowerShell blogu ile Microsoft Graph uygulama izinleri kullanarak Azure AD erişim incelemeleri oluşturma örneği](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241).

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Azure AD etkinlik günlükleri artık Azure Izleyici 'de kamu bulutu örnekleri için kullanılabilir

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure AD etkinlik günlüklerinin artık Azure Izleyici 'deki kamu bulutu örnekleri için kullanılabilir olduğunu duyurmaktan mutluluk duyuyoruz. Artık [SumoLogic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk)ve [arctıma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight)gibi SIEM araçlarınızla tümleştirebilmek için depolama hesabınıza veya bir olay hub 'ına Azure AD günlükleri gönderebilirsiniz.

Azure Izleyici 'yi ayarlama hakkında daha fazla bilgi için bkz. Azure [izleyici 'de Azure ad etkinlik günlükleri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations).

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Kullanıcılarınızı yeni, geliştirilmiş güvenlik bilgileri deneyimiyle güncelleştirin

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

25 Eylül 2019 ' de, Kullanıcı güvenlik bilgilerini kaydetmek ve yönetmek ve yalnızca yeni ve [geliştirilmiş sürümü](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271)açmak için eski, gelişmiş olmayan güvenlik bilgileri deneyimini kapatıyoruz. Bu, kullanıcılarınızın artık eski deneyimi kullanamayacağı anlamına gelir.

Gelişmiş güvenlik bilgileri deneyimi hakkında daha fazla bilgi için [yönetici belgelerimize](https://aka.ms/securityinfodocs) ve [Kullanıcı belgelerimize](https://aka.ms/securityinfoguide)bakın.

#### <a name="to-turn-on-this-new-experience-you-must"></a>Bu yeni deneyimi etkinleştirmek için şunları yapmanız gerekir:

1. Azure portal genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.

2. **Azure Active Directory > Kullanıcı ayarları ' na gidin > erişim paneli Önizleme özellikleri ayarlarını yönetin**.

3. Kullanıcılar, **güvenlik bilgilerini kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilir-gelişmiş** alanı, **Seçili**' i seçin ve ardından bir Kullanıcı grubu seçebilir ya da bu özelliği Kiracıdaki tüm kullanıcılar için açmak üzere **Tümünü** seçebilirsiniz.

4. * * Kullanıcılar, güvenliği kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilir * * bilgi * * * * alanı, **hiçbiri**' ni seçin.

5. Ayarlarınızı kaydedin.

    Ayarlarınızı kaydettikten sonra eski güvenlik bilgileri deneyimine artık erişebileceksiniz.

>[!Important]
>25 Eylül 2019 tarihinden önce bu adımları tamamlamazsanız, Azure Active Directory kiracınız gelişmiş deneyim için otomatik olarak etkinleştirilir. Sorularınız varsa lütfen adresinden bizimle iletişime geçin registrationpreview@microsoft.com .

---

### <a name="authentication-requests-using-post-logins-will-be-more-strictly-validated"></a>Oturum açma işlemleri kullanan kimlik doğrulama istekleri daha kesin olarak doğrulanacak

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** standartlar

2 Eylül 2019 ' den itibaren POST yöntemini kullanan kimlik doğrulama istekleri HTTP standartlarına karşı daha kesin olarak onaylanır. Özellikle, boşluklar ve çift tırnak işaretleri (") artık istek formu değerlerinden kaldırılmayacak. Bu değişikliklerin mevcut istemcileri bozmak beklenmez ve Azure AD 'ye gönderilen isteklerin her seferinde güvenilir bir şekilde işlendiğinden emin olmaya yardımcı olur.

Daha fazla bilgi için bkz. [Azure AD bölünmesi değişiklikleri bildirimleri](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored).

---

## <a name="july-2019"></a>Temmuz 2019

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>Değişiklik planı: yalnızca TLS 1,2 ' i destekleyecek uygulama proxy hizmeti güncelleştirmesi

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** uygulama proxy 'si **ürün yeteneği:** Access Control

En güçlü şifreimizi size sağlamaya yardımcı olmak için, uygulama proxy hizmeti erişimini yalnızca TLS 1,2 protokollerine sınırlandırmaya başlayacağız. Bu sınırlama, başlangıçta zaten TLS 1,2 protokollerini kullanan müşterilere dağıtılır, bu nedenle etkiyi görmezsiniz. TLS 1,0 ve TLS 1,1 protokollerinin kullanım dışı bırakılması, 31 Ağustos 2019 tarihinde tamamlanacaktır. Hala TLS 1,0 ve TLS 1,1 kullanan müşteriler, bu değişikliğe hazırlanmak için gelişmiş bir bildirim alır.

Bu değişiklik boyunca uygulama ara sunucusu hizmetiyle bağlantıyı sürdürmek için, istemci-sunucu ve tarayıcı-sunucu birleşimlerinizin TLS 1,2 kullanacak şekilde güncelleştirildiğinden emin olmanızı öneririz. Ayrıca, uygulama proxy 'Si hizmeti aracılığıyla yayınlanan uygulamalara erişmek için çalışanlarınızın kullandığı tüm istemci sistemlerini dahil ettiğinizden emin olmanızı öneririz.

Daha fazla bilgi için bkz. [Azure Active Directory Içindeki uygulama proxy 'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>Değişiklik planı: uygulama galerisine yönelik tasarım güncelleştirmeleri geliyor

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** kurumsal uygulamalar **ürün yeteneği:** SSO

Yeni Kullanıcı arabirimi değişiklikleri, **Uygulama Ekle** dikey penceresinin Galeri alanından **Ekle** ' nin tasarımına geliyor. Bu değişiklikler, otomatik sağlamayı, OpenID Connect, Security Assertion Markup Language (SAML) ve parola çoklu oturum açmayı (SSO) destekleyen uygulamalarınızı daha kolay bir şekilde bulmanıza yardımcı olur.

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>Değişiklik planı: MFA sunucusu IP adresinin Office 365 IP adresinden kaldırılması

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** MFA **ürün yeteneği:** kimlik güvenliği & koruması

MFA sunucusu IP adresini [Office 365 IP adresi ve URL Web hizmetinden](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service)kaldırıyoruz. Şu anda güvenlik duvarı ayarlarınızı güncelleştirmek için bu sayfalara sahipseniz [azure Multi-Factor Authentication sunucusu](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements) kullanmaya başlama konusunun **Azure Multi-Factor Authentication sunucusu güvenlik duvarı GEREKSINIMLERI** bölümünde belgelenen IP adreslerinin listesini de dahil ettiğinizden emin olmanız gerekir.

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>Yalnızca uygulama belirteçleri, artık istemci uygulamasının kaynak kiracısında var olmasını gerektiriyor

**Şunu yazın:** Sabit **hizmet kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

26 Temmuz 2019 ' de, [istemci kimlik bilgileri verme](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)yoluyla yalnızca uygulama belirteçleri sağlıyoruz. Daha önce, uygulamalar, istemci uygulamanın kiracıda olup olmamasına bakılmaksızın diğer uygulamaları çağırmak için belirteçleri alabilir. Bu davranışı, bazen Web API 'Leri olarak adlandırılan tek kiracılı kaynakların yalnızca kaynak kiracısında bulunan istemci uygulamaları tarafından çağrılabilir şekilde güncelleştirdik.

Uygulamanız kaynak kiracısında değilse, bu sorunu çözmek için, bir hata iletisi alırsınız `The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.` . Bu sorunu gidermek için, [Yönetici onay uç noktasını](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint) veya [PowerShell](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell)'i kullanarak kiracıda istemci uygulama hizmeti sorumlusunu oluşturmanız gerektiğini, kiracınızın kiracı içinde çalışmaya yönelik uygulama iznini vermiş olmasını sağlar.

Daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant).

> [!NOTE]
> İstemci ve API arasındaki mevcut onay gerekli olmaya devam eder. Uygulamalar yine de kendi yetkilendirme denetimlerini yapmakta olmalıdır.

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>FIDO2 güvenlik anahtarları kullanılarak Azure AD 'de yeni parolasız oturum açma

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD müşterileri artık, kuruluşlarının kullanıcıları ve grupları için FIDO2 güvenlik anahtarlarını yönetmek üzere ilkeler ayarlayabilir. Son kullanıcılar ayrıca güvenlik anahtarlarını kendi kendine kaydedebilir, FIDO özellikli cihazlarda Microsoft hesaplarında oturum açmak için anahtarları kullanabilir ve Azure AD 'ye katılmış Windows 10 cihazlarında oturum açabilirler.

Daha fazla bilgi için bkz. yönetici ile ilgili bilgiler için [Azure AD 'de passwordless oturum açma 'Yı etkinleştirme (Önizleme)](/azure/active-directory/authentication/concept-authentication-passwordless) ve son kullanıcı ile ilgili bilgiler için Güvenlik [anahtarı (Önizleme) kullanmak üzere güvenlik bilgilerini ayarlama](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key) .

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Temmuz 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Haziran 2019 ' de, uygulama galerisine federasyon desteği olan bu 18 yeni uygulamayı ekledik:

[Ungerboeck yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial), [parlak model omnichannel iletişim merkezi](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial), [Clever Nelly](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial), [acquireio](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial), [gevop](https://www.looop.co/schedule-a-demo/), [productboard](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial), [MS Azure SSO, etiıdex uyumluluk ofisi &trade; ](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso), [hype](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial), [soyut](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial), [ascentis](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial), [flipsnack](https://www.flipsnack.com/accounts/sign-in-sso.html), [wandera](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial), [TwineSocial](https://twinesocial.com/), [kallifer](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial), [hyperanna](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial) [ilaç](https://pharmid.com/) [i2B Connect](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** izleme & raporlama

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federasyon Dizini](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="new-azure-ad-domain-services-service-tag-for-network-security-group"></a>Ağ güvenlik grubu için yeni Azure AD Domain Services hizmet etiketi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD Domain Services **ürün özelliği:** Azure AD Domain Services

IP adresi ve aralıklarının uzun listesini yönetiyorsanız, Azure AD Domain Services sanal ağ alt ağına gelen trafiğin güvenliğini sağlamaya yardımcı olması için Azure ağ güvenlik grubunuzdaki yeni **AzureActiveDirectoryDomainServices** Network Service etiketini kullanabilirsiniz.

Bu yeni hizmet etiketi hakkında daha fazla bilgi için bkz. [Azure AD Domain Services Için ağ güvenlik grupları](../../active-directory-domain-services/network-considerations.md#network-security-groups-and-required-ports).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Domain Services için yeni güvenlik denetimleri (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD Domain Services **ürün özelliği:** Azure AD Domain Services

Azure AD etki alanı hizmeti güvenlik denetimi 'nin genel önizlemeye sunulduğunu duyurmaktan mutluluk duyuyoruz. Güvenlik denetimi, Azure depolama, Azure Log Analytics çalışma alanları ve Azure Olay Hub 'ı dahil olmak üzere Azure AD etki alanı hizmet portalı 'nı kullanarak, hedeflenen kaynaklara güvenlik denetim olayları aktararak kimlik doğrulama hizmetlerinize yönelik kritik Öngörüler sağlamanıza yardımcı olur.

Daha fazla bilgi için bkz. [Azure AD Domain Services Için güvenlik denetimlerini etkinleştirme (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>Yeni kimlik doğrulama yöntemleri kullanım & öngörüleri (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** self servis parola sıfırlama **ürün yeteneği:** izleme & raporlama

Yeni kimlik doğrulama yöntemleri kullanım & öngörüleri raporları, Azure Multi-Factor Authentication ve self servis parola sıfırlama gibi özelliklerin kuruluşunuzda nasıl kaydedildiğini ve kullanıldığını, her bir özellik için kayıtlı Kullanıcı sayısı, parolaları sıfırlamak için hangi sıklıkla parola sıfırlamasının kullanıldığı ve sıfırlama yöntemi dahil olmak üzere nasıl çalıştığını anlamanıza yardımcı olabilir.

Daha fazla bilgi için bkz. [kimlik doğrulama yöntemleri kullanımı & Öngörüler (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights).

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>Tüm Azure AD yöneticileri için yeni güvenlik raporları kullanılabilir (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Tüm Azure AD yöneticileri artık riskli **Kullanıcılar** ve **riskli oturum açma** raporlarında gösterildiği gibi yeni güvenlik deneyimini kullanmaya başlamak için, **risk için işaretlenmiş kullanıcılar** raporu gibi mevcut güvenlik raporlarının en üstündeki başlığı seçebilirler. Zaman içinde, tüm güvenlik raporları eski sürümlerden yeni sürümlere taşınır ve yeni raporlar aşağıdaki ek olanakları sağlar:

- Gelişmiş filtreleme ve sıralama

- Eksik Kullanıcı riski gibi toplu eylemler

- Güvenliği aşılmış veya güvenli varlıkların onayı

- Risk durumu, kapsayan: risk, kapatılan, düzeltilen ve onaylanan güvenliği aşılmış

Daha fazla bilgi için bkz. [riskli kullanıcılar raporu](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users) ve [riskli oturum açma bileşenleri raporu](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Domain Services için yeni güvenlik denetimleri (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD Domain Services **ürün özelliği:** Azure AD Domain Services

Azure AD etki alanı hizmeti güvenlik denetimi 'nin genel önizlemeye sunulduğunu duyurmaktan mutluluk duyuyoruz. Güvenlik denetimi, Azure depolama, Azure Log Analytics çalışma alanları ve Azure Olay Hub 'ı dahil olmak üzere Azure AD etki alanı hizmet portalı 'nı kullanarak, hedeflenen kaynaklara güvenlik denetim olayları aktararak kimlik doğrulama hizmetlerinize yönelik kritik Öngörüler sağlamanıza yardımcı olur.

Daha fazla bilgi için bkz. [Azure AD Domain Services Için güvenlik denetimlerini etkinleştirme (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>SAML/WS-beslemesiyle yeni B2B Direct Federation (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2B **ürün yeteneği:** B2B/B2C

Doğrudan Federasyon, SAML veya WS-Beslikli standartları destekleyen kimlik sistemleriyle çalışarak, BT tarafından yönetilen kimlik çözümü Azure AD olmayan iş ortaklarıyla çalışmanızı kolaylaştırır. Bir iş ortağıyla doğrudan Federasyon ilişkisi ayarladıktan sonra, bu etki alanından davet ettiğiniz tüm yeni Konuk kullanıcıları mevcut kurumsal hesaplarını kullanarak sizinle işbirliği yapabilir ve konuklarınız için Kullanıcı deneyimini daha sorunsuz hale getirir.

Daha fazla bilgi için bkz. [Konuk kullanıcılar için AD FS ve üçüncü taraf sağlayıcılarla doğrudan Federasyon (Önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** izleme & raporlama

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federasyon Dizini](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-check-for-duplicate-group-names-in-the-azure-ad-portal"></a>Azure AD portalındaki yinelenen grup adları için yeni denetim

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Artık Azure AD portalından bir grup adı oluşturduğunuzda veya güncelleştirdiğinizde, kaynakta mevcut bir grup adını çoğaltdığınıza ilişkin bir denetim yapacağız. Adın zaten başka bir grup tarafından kullanımda olduğunu tespit ettik, adınızı değiştirmeniz istenir.

Daha fazla bilgi için bkz. [Azure AD portalındaki grupları yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD artık yanıt (yeniden yönlendirme) URI 'Lerinde statik sorgu parametrelerini desteklemektedir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD Apps artık, `https://contoso.com/oauth2?idp=microsoft` OAuth 2,0 istekleri için statik sorgu parametreleriyle (örneğin,) Yanıtla (yeniden yönlendirme) URI 'leri kaydedebilir ve kullanabilir. Statik sorgu parametresi, yanıt URI 'sinin diğer herhangi bir bölümünde olduğu gibi, yanıt URI 'Leri için dize eşlemeye tabidir. URL kodu çözülmüş yeniden yönlendirme URI 'si ile eşleşen kayıtlı bir dize yoksa, istek reddedilir. Yanıt URI 'SI bulunursa, statik sorgu parametresi dahil olmak üzere, kullanıcının yeniden yönlendirileceği tüm dize kullanılır.

Bir güvenlik riskini temsil ettiğinden dinamik yanıt URI 'Leri hala yasaktır ve bir kimlik doğrulama isteği genelinde durum bilgilerini sürdürmek için kullanılamaz. Bu amaçla `state` parametresini kullanın.

Şu anda Azure portal uygulama kayıt ekranları hala sorgu parametrelerini engelliyor. Ancak, uygulamanıza sorgu parametrelerini eklemek ve test etmek için uygulama bildirimini el ile düzenleyebilirsiniz. Daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters).

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Azure AD için etkinlik günlükleri (MS Graph API 'Leri) artık PowerShell cmdlet 'Leri aracılığıyla kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure AD etkinlik günlüklerinin (denetim ve oturum açma raporları) artık Azure AD PowerShell modülü aracılığıyla kullanılabildiğini duyurmak için heyecanlıyız. Daha önce, MS Graph API uç noktaları kullanarak kendi betiklerinizi oluşturabilir ve şimdi bu özelliği PowerShell cmdlet 'lerine genişlettik.

Bu cmdlet 'leri kullanma hakkında daha fazla bilgi için bkz. [Raporlama Için Azure AD PowerShell cmdlet 'leri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting).

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Azure AD 'de denetim ve oturum açma günlükleri için filtre denetimleri güncelleştirildi

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Denetim ve oturum açma günlüğü raporlarını, artık rapor ekranlarında sütun olarak eklemek zorunda kalmadan çeşitli filtreler uygulayabilmeniz için güncelleştirdik. Ayrıca, ekranda kaç filtre göstermek istediğinizi de seçebilirsiniz. Bu güncelleştirmeler, raporların daha kolay okunmasını ve gereksinimlerinize göre daha fazla kapsamlanmasını kolaylaştırmak için birlikte çalışır.

Bu güncelleştirmeler hakkında daha fazla bilgi için bkz. [Denetim günlüklerini filtreleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs) ve [oturum açma etkinliklerini filtreleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities).

---

## <a name="june-2019"></a>Haziran 2019

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>Microsoft Graph için yeni riskDetections API (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Microsoft Graph için yeni riskDetections API 'sini duyurmaktan mutluluk duyuyoruz, artık genel önizlemede. Bu yeni API 'YI, kuruluşunuzun kimlik korumasıyla ilgili Kullanıcı ve oturum açma riski algılamalarının bir listesini görüntülemek için kullanabilirsiniz. Bu API 'yi, algılama türü, durumu, düzeyi ve daha fazlası hakkındaki ayrıntılar dahil, risk algılamalarınızı daha verimli bir şekilde sorgulamak için de kullanabilirsiniz.

Daha fazla bilgi için [risk algılama API 'si başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Haziran 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Haziran 2019 ' de, uygulama galerisine federasyon desteğiyle bu 22 yeni uygulamaları ekledik:

[Azure AD SAML araç seti](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial), [Otsuka Shokai (大塚商会)](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial), [ANAQUA](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial), [Azure VPN istemcisi](https://portal.azure.com/), [expenseın](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial), [yardımcı Yardımcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial), [Costpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial), [GlobalOne](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial), [Mercedes-Benz-otomobil ofisi](https://me.secure.mercedes-benz.com/), [Skore](https://app.justskore.it/), [Oracle Cloud Infrastructure Console](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial), [SIVARK SAML kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial) [,](https://www.scrible.com/sign-in/#/create-account)IServiceProvider, [PandaDoc](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial), [Perceptyx](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial), [proptıbol OS](https://proptimise.co.uk/software/), [vtiger CRM (SAML)](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial), Oracle Retail Merchandising için Oracle Erişim Yöneticisi, Oracle e-iş paketi için Oracle Erişim Yöneticisi, c-Business Suite için Oracle IDCs, JD Edler için Oracle IDCs

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** izleme & raporlama

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Zoom](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Envoy](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>Azure AD sağlama hizmeti 'nin gerçek zamanlı ilerlemesini görüntüleme

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

Azure AD sağlama deneyimini, Kullanıcı sağlama sürecinde ne kadar uzakta olduğunu gösteren yeni bir ilerleme çubuğu içerecek şekilde güncelleştirdik. Bu güncelleştirilmiş deneyim Ayrıca, geçerli döngüyle sağlanan kullanıcı sayısı ve ne kadar Kullanıcı sağlandığını gösteren bilgileri de sağlar.

Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user).

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>Şirket markası artık oturum kapatma ve hata ekranları üzerinde görünüyor

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD 'yi, şirket markasının artık oturum açma ve hata ekranlarında, oturum açma sayfasından da görünmesini sağlayacak şekilde güncelleştirdik. Bu özelliği açmak için herhangi bir şey yapmanız gerekmez, Azure AD yalnızca Azure portal **Şirket markası** alanında ayarlamış olduğunuz varlıkları kullanır.

Şirket markanızı ayarlama hakkında daha fazla bilgi için bkz. [kuruluşunuzun Azure Active Directory sayfalarına marka ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>Azure Multi-Factor Authentication (MFA) sunucusu artık Yeni dağıtımlar için kullanılamaz

**Şunu yazın:** Kullanım dışı **hizmet kategorisi:** MFA **ürün yeteneği:** kimlik güvenliği & koruması

1 Temmuz 2019 itibariyle, Microsoft artık Yeni dağıtımlar için MFA sunucusu sunmaz. Kuruluşunuzda Multi-Factor Authentication istemek isteyen yeni müşterilerin artık bulut tabanlı Azure Multi-Factor Authentication kullanması gerekir. MFA sunucusunu 1 Temmuz 'dan önce etkinleştiren müşteriler bir değişiklik görmez. Hala en son sürümü indirebilir, gelecekteki güncelleştirmeleri alabilir ve etkinleştirme kimlik bilgileri oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication sunucusu](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)kullanmaya başlama. Bulut tabanlı Azure Multi-Factor Authentication hakkında daha fazla bilgi için bkz. [bulut tabanlı azure Multi-Factor Authentication dağıtımı planlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="may-2019"></a>Mayıs 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Hizmet değişikliği: uygulama proxy 'Si hizmetinde yalnızca TLS 1,2 protokolleri için gelecekteki destek

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** uygulama proxy 'si **ürün yeteneği:** Access Control

Müşterilerimiz için en iyi sınıf şifrelemeyi sağlamaya yardımcı olmak için, yalnızca uygulama proxy 'Si hizmetindeki TLS 1,2 protokollerine erişimi sınırlıyoruz. Bu değişiklik, yalnızca TLS 1,2 protokollerini kullanan müşterilere yavaş bir şekilde dağıtılır, bu nedenle herhangi bir değişiklik göremezsiniz.

TLS 1,0 ve TLS 1,1 ' nin kullanımdan kaldırılması 31 Ağustos 2019 tarihinde yapılır, ancak bu değişikliğe hazırlanmak için zaman duyarsınız. Bu değişikliğe hazırlanmak için, kullanıcılarınızın uygulama proxy 'Si aracılığıyla yayınlanan uygulamalara erişmek için kullanacağı istemciler dahil olmak üzere, istemci-sunucu ve tarayıcı-sunucu birleşimlerinizin, uygulama proxy 'Si hizmetiyle bağlantıyı sürdürmek için TLS 1,2 protokolünü kullanacak şekilde güncelleştirildiğinden emin olun. Daha fazla bilgi için bkz. [Azure Active Directory Içindeki uygulama proxy 'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin).

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Uygulamayla ilgili oturum açma verilerinizi görüntülemek için kullanım ve Öngörüler raporunu kullanın

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** izleme & raporlama

Artık aşağıdakiler hakkında bilgiler de dahil olmak üzere oturum açma verilerinizin uygulama merkezli bir görünümünü almak için Azure portal **Kurumsal uygulamalar** alanında bulunan kullanım ve Öngörüler raporunu kullanabilirsiniz:

- Kuruluşunuz için kullanılan en popüler uygulamalar

- En fazla başarısız oturum açma işlemleri içeren uygulamalar

- Her uygulama için ilk oturum açma hataları

Bu özellik hakkında daha fazla bilgi için [Azure Active Directory portalındaki Kullanım ve Öngörüler raporuna](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report) bakın

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Azure AD kullanarak bulut uygulamalarına Kullanıcı hazırlığını otomatikleştirin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** izleme & raporlama

Aşağıdaki bulut tabanlı uygulamalar için Kullanıcı hesaplarının oluşturulmasını, silinmesini ve güncelleştirilmesini otomatik hale getirmek için Azure AD sağlama hizmetini kullanmak üzere bu yeni öğreticilerini izleyin:

- [Birlikte karşılayın](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [DynamicSignal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [Permi güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

Ayrıca, Grup nesneleri sağlama hakkında bilgi sağlayan bu yeni [Dropbox öğreticisini](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial)de izleyebilirsiniz.

Otomatik Kullanıcı hesabı sağlama aracılığıyla kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>Kimlik güvenli puanı artık Azure AD 'de kullanılabilir (genel kullanılabilirlik)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** yok **ürün yeteneği:** kimlik güvenliği & koruması

Artık Azure AD 'de kimlik güvenli puanı özelliğini kullanarak kimlik güvenliğini izleyip geliştirebilirsiniz. Kimlik güvenli puanı özelliği, size yardımcı olması için tek bir pano kullanır:

- NesneKimliği, 1 ile 223 arasında bir puan temelinde kimlik güvenliği önlemidir.

- Kimlik güvenlik geliştirmelerinizi planlayın

- Güvenlik geliştirmelerinizin başarısını inceleyin

Kimlik güvenliği puanı özelliği hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik güvenli puanı nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score).

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Yeni Uygulama kayıtları deneyimi artık kullanıma sunuldu (genel kullanılabilirlik)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** geliştirici deneyimi

Yeni [uygulama kayıtları](https://aka.ms/appregistrations) deneyimi artık genel kullanıma sunulmuştur. Bu yeni deneyim, Azure portal ve uygulama kayıt Portalı ' ndan bildiğiniz tüm önemli özellikleri içerir ve bunları aracılığıyla geliştirir:

- **Daha iyi uygulama yönetimi.** Uygulamalarınızı farklı portallarda görmek yerine, artık tüm uygulamalarınızı tek bir konumda görebilirsiniz.

- **Basitleştirilmiş uygulama kaydı.** Geliştirilmiş gezinti deneyiminden, yeniden kullanılabilir izin seçimi deneyimine yönelik olarak, uygulamalarınızı kaydetmek ve yönetmek artık daha kolay.

- **Daha ayrıntılı bilgi.** Hızlı başlangıç kılavuzlarınız ve daha fazlası dahil olmak üzere uygulamanız hakkında daha fazla ayrıntı bulabilirsiniz.

Daha fazla bilgi için bkz. [Microsoft Identity platform](https://docs.microsoft.com/azure/active-directory/develop/) ve [uygulama kayıtları deneyimi artık genel kullanıma sunuldu!](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) Blog duyurusu.

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Kimlik koruması için riskli kullanıcılar API 'sinde sunulan yeni yetenekler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Kullanıcıların risk geçmişini almak, riskli kullanıcıları kapatmak ve kullanıcıları tehlikeye atılmak üzere doğrulamak için riskli kullanıcılar API 'sini artık kullanacağınızı duyurmaktan memnuniyet duyuyoruz. Bu değişiklik, kullanıcılarınızın risk durumunu daha verimli bir şekilde güncelleştirmenize ve risk geçmişini anlamanıza yardımcı olur.

Daha fazla bilgi için [riskli KULLANıCıLAR API başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mayıs 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Mayıs 2019 ' de, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [gerçek bağlantılar](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kida](https://app.kianda.com/sso/OpenID/AzureAD/), [basit imza](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [BRAZE](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [displayr](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [templamy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo Sales](https://toutapp.com/login)katılım, [aclp](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [META4 Global HR](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), [hisse çalışma alanı](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [Cobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [WebMethods API bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [redflag](https://pocketstop.com/redflag/), [whatdüzeltmesini](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [denetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [jobhub](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [neogov](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), [pdee](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial), [myvr](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Azure AD portalında geliştirilmiş Grup oluşturma ve yönetim deneyimleri

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Azure AD portalındaki gruplarla ilgili deneyimler için geliştirmeler yaptık. Bu geliştirmeler, yöneticilerin grup listelerini, üye listelerini daha iyi yönetmesine ve ek oluşturma seçenekleri sağlamasına imkan tanır.

Geliştirmeler şunları içerir:

- Üyelik türüne ve grup türüne göre temel filtreleme.

- Kaynak ve e-posta adresi gibi yeni sütunların eklenmesi.

- Kolay bir şekilde silinmek üzere grupları, üyeleri ve sahip listelerini çoklu seçme özelliği.

- Grup oluşturma sırasında e-posta adresi seçme ve sahipler ekleme özelliği.

Daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Azure AD portalındaki Office 365 grupları için adlandırma ilkesi yapılandırma (genel kullanılabilirlik)

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kuralları uygulanmasını sağlar.

Office 365 grupları için adlandırma ilkesini iki farklı yolla yapılandırabilirsiniz:

- Otomatik olarak bir grup adına eklenen ön ekleri veya sonekleri tanımlayın.

- Kuruluşunuz için Grup adlarında izin verilmeyen özelleştirilmiş bir engellenen sözcük kümesini karşıya yükleyin (örneğin, "CEO, bordro, HR").

Daha fazla bilgi için bkz. [Office 365 grupları Için adlandırma Ilkesi zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Microsoft Graph API uç noktaları artık Azure AD etkinlik günlükleri (genel kullanılabilirlik) için kullanılabilir

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure AD etkinlik günlükleri için Microsoft Graph API uç noktası desteğinin genel kullanıma sunulduğunu duyurmaktan mutluluk duyuyoruz. Bu sürümle birlikte artık hem Azure AD denetim günlüklerinin hem de oturum açma günlüğü API 'Lerinin 1,0 sürümünü kullanabilirsiniz.

Daha fazla bilgi için bkz. [Azure AD denetim günlüğü API 'sine genel bakış](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0).

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Yöneticiler artık Birleşik kayıt işlemi (Genel Önizleme) için koşullu erişimi kullanabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği & koruması

Yöneticiler artık Birleşik kayıt sayfası tarafından kullanılmak üzere koşullu erişim ilkeleri oluşturabilir. Bu, şu durumlarda kayda izin vermek için ilkeleri uygulamayı içerir:

- Kullanıcılar güvenilir bir ağ üzerinde.

- Kullanıcılar, düşük bir oturum açma riskidir.

- Kullanıcılar yönetilen bir cihazlardır.

- Kullanıcılar kuruluşun kullanım koşulları 'nı (TOU) kabul etmiş olursunuz.

Koşullu erişim ve parola sıfırlama hakkında daha fazla bilgi için [Azure AD BIRLEŞIK MFA ve parola sıfırlama kayıt deneyimi blog gönderisine yönelik koşullu erişimi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348)görebilirsiniz. Birleşik kayıt işlemi için koşullu erişim ilkeleri hakkında daha fazla bilgi için bkz. [Birleşik kayıt Için koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration). Azure AD kullanım koşulları özelliği hakkında daha fazla bilgi için bkz. [kullanım koşulları özelliği Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

## <a name="april-2019"></a>Nisan 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>Yeni Azure AD tehdit zekası algılaması artık Azure AD Kimlik Koruması bir parçası olarak sağlanıyor

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Azure AD Threat Intelligence algılama artık güncelleştirilmiş Azure AD Kimlik Koruması özelliğinin bir parçası olarak sunulmaktadır. Bu yeni işlevsellik, Microsoft 'un dahili ve dış tehdit bilgileri kaynaklarına dayanan bilinen saldırı desenleriyle tutarlı olan belirli bir kullanıcı veya etkinliğin olağan dışı Kullanıcı etkinliklerini belirtmenize yardımcı olur.

Azure AD Kimlik Koruması yenilenen sürümü hakkında daha fazla bilgi için, bkz. [dört ana Azure AD kimlik koruması geliştirmesi artık genel önizleme](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) bloguna ve [ne Azure Active Directory kimlik koruması (YENİLENDİ)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) bakın. Azure AD Threat Intelligence algılaması hakkında daha fazla bilgi için [risk algılamalarını Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks) makalesine bakın.

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Azure AD Yetkilendirme Yönetimi artık kullanıma sunuldu (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Identity idare **ürün yeteneği:** kimlik yönetimi

Artık genel önizlemede olan Azure AD Yetkilendirme Yönetimi, müşterilerin, çalışanların ve iş ortaklarının erişim isteme, ne kadar süreceğine ve ne kadar erişebileceğini tanımlayan erişim paketleri yönetimini temsilcmalarına yardımcı olur. Erişim paketleri Azure AD ve Office 365 gruplarındaki üyeliği, kurumsal uygulamalardaki rol atamalarını ve SharePoint Online siteleri için rol atamalarını yönetebilir. Yetkilendirme Yönetimi hakkında daha fazla bilgi için [bkz. Azure AD Yetkilendirme Yönetimi](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview). Privileged Identity Management, erişim gözden geçirmeleri ve kullanım koşulları dahil Azure AD Identity Governance özellikleri hakkında daha fazla bilgi edinmek için bkz. [Azure AD Identity Governance nedir?](../governance/identity-governance-overview.md).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>Azure AD portalındaki Office 365 grupları için adlandırma ilkesi yapılandırma (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kuralları uygulanmasını sağlar.

Office 365 grupları için adlandırma ilkesini iki farklı yolla yapılandırabilirsiniz:

- Otomatik olarak bir grup adına eklenen ön ekleri veya sonekleri tanımlayın.

- Kuruluşunuz için Grup adlarında izin verilmeyen özelleştirilmiş bir engellenen sözcük kümesini karşıya yükleyin (örneğin, "CEO, bordro, HR").

Daha fazla bilgi için bkz. [Office 365 grupları Için adlandırma Ilkesi zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure AD etkinlik günlükleri artık Azure Izleyici 'de kullanılabilir (genel kullanılabilirlik)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure AD etkinlik günlükleri ile görselleştirmelerle ilgili geri bildirimlerinizi ele almak için, Log Analytics yeni bir Öngörüler özelliği sunuyoruz. Bu özellik, çalışma kitapları adlı etkileşimli şablonlarımızı kullanarak Azure AD kaynaklarınız hakkında öngörüler elde etmenize yardımcı olur. Önceden oluşturulmuş bu çalışma kitapları, uygulamalar veya kullanıcılar için Ayrıntılar sağlayabilir ve şunları içerir:

- **Oturum açma işlemleri.** Uygulama ve kullanıcıların oturum açma konumu, kullanımda olan işletim sistemi veya tarayıcı istemcisi ve sürümü ve başarılı ya da başarısız oturum açma sayısı dahil olmak üzere ayrıntıları sağlar.

- **Eski kimlik doğrulama ve koşullu erişim.** Koşullu erişim ilkeleri tarafından tetiklenen Multi-Factor Authentication kullanımı, koşullu erişim ilkelerini kullanan uygulamalar vb. dahil olmak üzere eski kimlik doğrulaması kullanan uygulamalar ve kullanıcılar için ayrıntılar sağlar.

- **Oturum açma hatası analizi.** Kullanıcı eylemi, ilke sorunları veya altyapınız nedeniyle oturum açma hatalarınızın oluşup oluşmadığını belirlemenize yardımcı olur.

- **Özel raporlar.** Kuruluşunuz için Öngörüler özelliğini özelleştirmeye yardımcı olmak üzere yeni veya mevcut çalışma kitaplarını düzenleyebilirsiniz.

Daha fazla bilgi için bkz. [Azure izleyici çalışma kitaplarını Azure Active Directory raporları için kullanma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Nisan 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Nisan 'da, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [Hrçalışmalar çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [percogeç](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [Mobıoncontrol](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [benchling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [eğitibrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [rstudio Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba Cloud (rol tabanlı SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [cerkatlanmış hisse senedi yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [sectigo Sertifika Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [tarzorbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [Monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [gözcemaymun Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [ındiggo](https://indiggolead.com/)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>Yeni erişim İncelemeleri sıklık seçeneği ve birden çok rol seçimi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** erişim gözden geçirmeleri **ürün yeteneği:** Identity idare

Azure AD erişim gözden geçirmeleriyle yeni güncelleştirmeler şunları yapmanıza olanak sağlar:

- Daha önce haftalık, aylık, üç aylık ve yıllık olan mevcut seçeneklere ek olarak, erişim incelemelerinizin sıklığını **yarı yıllık**olarak değiştirin.

- Tek bir erişim incelemesi oluştururken birden çok Azure AD ve Azure Kaynak rolü seçin. Bu durumda, tüm roller aynı ayarlarla ayarlanır ve tüm gözden geçirenler aynı anda bilgilendirilir.

Erişim incelemesi oluşturma hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD Connect e-posta uyarı sistemleri geçiyor, bazı müşteriler için yeni e-posta gönderici bilgileri gönderiliyor

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** ad eşitleme **ürün yeteneği:** platform

Azure AD Connect, e-posta uyarı sistemlerimizi geçiş sürecinde olduğundan, bazı müşterilere yeni bir e-posta gönderici gösteriliyor olabilir. Bunu çözmek için `azure-noreply@microsoft.com` kuruluşunuzun izin verilenler listesine eklemeniz gerekir veya Office 365, Azure veya eşitleme hizmetinizden önemli uyarıları almaya devam edemeyeceksiniz.

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>UPN son ek değişiklikleri artık Azure AD Connect Federasyon etki alanları arasında başarılı oluyor

**Şunu yazın:** Sabit **hizmet kategorisi:** ad eşitleme **ürün yeteneği:** platform

Artık bir kullanıcının UPN sonekini, Azure AD Connect bir Federasyon etki alanından başka bir Federasyon etki alanına başarıyla değiştirebilirsiniz. Bu çözüm, eşitleme çevrimi sırasında FederatedDomainChangeError hata iletisini artık deneymemelisiniz veya "[FederatedUser. UserPrincipalName] özniteliği geçerli olmadığından," Bu nesne Azure Active Directory güncelleştirilemiyor, "bir bildirim e-postası alacaksınız. Yerel dizin hizmetinizdeki değeri güncelleştirin.

Daha fazla bilgi için bkz. [eşitleme sırasında sorun giderme hataları](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror).

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Azure AD 'de uygulama koruma tabanlı koşullu erişim ilkesini kullanarak artırılmış güvenlik (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği & koruması

Uygulama koruma tabanlı koşullu erişim artık **Uygulama koruma Ilkesi gerektir** ilkesi kullanılarak kullanılabilir. Bu yeni ilke, aşağıdakileri önlemeye yardımcı olarak kuruluşunuzun güvenliğini artırmaya yardımcı olur:

- Kullanıcılar Microsoft Intune lisansı olmadan uygulamalara erişim hakkı elde edebilirler.

- Kullanıcılar Microsoft Intune uygulama koruma ilkesi alamıyor.

- Kullanıcılar, yapılandırılmış bir Microsoft Intune uygulama koruma ilkesi olmadan uygulamalara erişim hakkı elde edebilirler.

Daha fazla bilgi için bkz. [koşullu erişim ile Cloud App erişimi için uygulama koruma Ilkesi gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Microsoft Edge 'de Azure AD çoklu oturum açma ve koşullu erişim için yeni destek (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği & koruması

Azure AD çoklu oturum açma ve koşullu erişim için yeni destek sağlama dahil olmak üzere Microsoft Edge için Azure AD desteğimizi geliştirdik. Daha önce Microsoft Intune Managed Browser kullandıysanız, artık bunun yerine Microsoft Edge 'i kullanabilirsiniz.

Koşullu erişim kullanarak cihazlarınızı ve uygulamalarınızı ayarlama ve yönetme hakkında daha fazla bilgi için bkz. [koşullu erişim ile Cloud App erişimi için yönetilen cihazlar gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) ve [koşullu erişim ile Cloud App erişimi için onaylanan istemci uygulamaları gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Microsoft Edge 'i Microsoft Intune ilkeleriyle kullanarak erişimin nasıl yönetileceği hakkında daha fazla bilgi için, bkz. [Microsoft Intune İlkeyle korunan tarayıcı kullanarak Internet erişimini yönetme](https://docs.microsoft.com/intune/app-configuration-managed-browser).

---

## <a name="march-2019"></a>Mart 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Azure Active Directory B2C 'da kimlik deneyimi çerçevesi ve özel ilke desteği artık kullanılabilir (GA)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Artık Azure AD B2C, ölçeklendirerek ve Azure SLA 'umuz kapsamında desteklenen aşağıdaki görevler de dahil olmak üzere özel ilkeler oluşturabilirsiniz:

- Özel ilkeler kullanarak özel kimlik doğrulama Kullanıcı yolculukları oluşturun ve karşıya yükleyin.

- Talep sağlayıcıları arasındaki değişimleri adım adım kabul eden kullanıcı hakkındaki adımları açıklama.

- Kullanıcı yolculukları 'nda koşullu dallanma tanımlayın.

- Gerçek zamanlı kararlar ve iletişimlerde kullanım için talepleri dönüştürün ve eşleyin.

- Özel kimlik doğrulama Kullanıcı yolculuğa REST API özellikli Hizmetleri kullanın. Örneğin, e-posta sağlayıcıları, CRMs ve özel yetkilendirme sistemleri.

- Openıdconnect protokolüyle uyumlu kimlik sağlayıcılarıyla federasyona bağlayın. Örneğin, çok kiracılı Azure AD, sosyal hesap sağlayıcıları veya iki öğeli doğrulama sağlayıcıları ile.

Özel ilkeler oluşturma hakkında daha fazla bilgi için, bkz. [Azure Active Directory B2C özel ilkeler Için geliştirici notları](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-developer-notes-custom) ve [örnek olay incelemeleri de dahil, Alex SION blog gönderisini](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-custom-policies-to-build-your-own-identity-journeys/ba-p/382791)okuyun.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mart 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Mart 'da, uygulama galerisine federasyon desteği olan bu 14 yeni uygulamaları ekledik:

[ISEC7 mobil Exchange temsilcisi](https://www.isec7.com/english/), [mediusflow](https://office365.cloudapp.mediusflow.com/), [eplatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [fulcrud](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [excelityglobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Açıklama tabanlı denetim sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [yalın](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [powerokul performansı önemli](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [cincode](https://cinode.com/), [Iris intraneti](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [tas](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Azure AD galerisinde yeni Zscaler ve Atlasser sağlama bağlayıcıları-Mart 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği:** üçüncü taraf tümleştirmesi

Aşağıdaki uygulamalar için Kullanıcı hesaplarını oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirin:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](https://aka.ms/ZscalerBetaProvisioning), [Zscaler One](https://aka.ms/ZscalerOneProvisioning), [Zscaler iki](https://aka.ms/ZscalerTwoProvisioning), [Zscaler üç](https://aka.ms/ZscalerThreeProvisioning), [Zscaler zscsesli](https://aka.ms/ZscalerZSCloudProvisioning), [atlasser bulutu](https://aka.ms/atlassianCloudProvisioning)

Otomatik Kullanıcı hesabı sağlama aracılığıyla kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://aka.ms/ProvisioningDocumentation).

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Azure AD portalındaki silinen Office 365 gruplarınızı geri yükleme ve yönetme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Artık silinen Office 365 gruplarınızı Azure AD portalından görüntüleyebilir ve yönetebilirsiniz. Bu değişiklik, geri yükleme için hangi grupların kullanılabilir olduğunu görmenizi sağlar ve kuruluşunuzun ihtiyaç duymayan grupları kalıcı olarak silmenizi sağlar.

Daha fazla bilgi için bkz. [geri yükleme süre aşımına uğradı veya silinmiş gruplar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore).

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Çoklu oturum açma artık Azure AD SAML ile güvenli şirket içi uygulamalar için uygulama ara sunucusu aracılığıyla kullanılabilir (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** Access Control

Artık şirket içi, SAML kimliği doğrulanmış uygulamalar için çoklu oturum açma (SSO) deneyimi ve uygulama proxy 'Si aracılığıyla bu uygulamalara uzaktan erişim sağlayabilirsiniz. Şirket içi uygulamalarınızla SAML SSO 'yu ayarlama hakkında daha fazla bilgi için bkz. [uygulama proxy 'si (Önizleme) ile şirket içi uygulamalar Için SAML çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>İstek döngülerine yönelik istemci uygulamaları, güvenilirliği ve Kullanıcı deneyimini geliştirmek için kesintiye uğrar

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

İstemci uygulamaları kısa bir süre boyunca aynı oturum açma isteklerini yanlışlıkla verebilir. Bu istekler, başarılı olsun veya etmeksizin, tüm kullanıcılar için zayıf bir kullanıcı deneyimine ve ıDP 'nin bu iş yüklerini artırarak, tüm kullanıcılar için gecikme süresini artırarak ve ıDP kullanılabilirliğini azalttığından.

Bu güncelleştirme bir `invalid_grant` hata gönderir: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request` normal işlem kapsamının ötesinde, kısa bir süre içinde birden çok kez yinelenen istekler veren istemci uygulamalarına. Bu sorunla karşılaşan istemci uygulamalarında, kullanıcının yeniden oturum açmasını gerektiren bir etkileşimli istem gösterilmelidir. Bu değişiklik ve bu hatayla karşılaştığında uygulamanızı nasıl gidereceğiniz hakkında daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted).

---

### <a name="new-audit-logs-user-experience-now-available"></a>Yeni denetim günlükleri Kullanıcı deneyimi artık kullanılabilir

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Okunabilirliği ve bilgilerinizi aramanızı artırmaya yardımcı olmak için yeni bir Azure AD **Denetim günlükleri** sayfası oluşturduk. Yeni **Denetim günlükleri** sayfasını görmek IÇIN Azure AD 'nin **etkinlik** bölümündeki **Denetim günlükleri** ' ni seçin.

![Yeni denetim günlükleri sayfası, örnek bilgi](media/whats-new/audit-logs-page.png)

Yeni **Denetim günlükleri** sayfası hakkında daha fazla bilgi için, bkz. [Azure Active Directory portalındaki denetim etkinliği raporları](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs).

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Yanlış yapılandırılmış koşullu erişim ilkeleriyle yanlışlıkla yöneticinin kilitlenmesini önlemeye yardımcı olacak yeni uyarılar ve kılavuzluk

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği & koruması

Yöneticilerin, yanlış yapılandırılmış koşullu erişim ilkeleri aracılığıyla kendi kiracılarından kendilerini yanlışlıkla kilitlemesini önlemeye yardımcı olmak için Azure portal yeni uyarılar ve güncelleştirilmiş rehberlik oluşturduk. Yeni rehberlik hakkında daha fazla bilgi için bkz. [koşullu erişim Azure Active Directory hizmet bağımlılıkları nelerdir](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Mobil cihazlarda geliştirilmiş son kullanıcı kullanım koşulları deneyimi

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** idare

Mevcut kullanım koşullarımızı, bir mobil cihazda kullanım koşullarını nasıl gözden geçitireceğiniz ve kabul ettiğiniz hakkında daha fazla yardımcı olacak şekilde güncelleştirdik. Artık yakınlaştırıp uzaklaştırabilir, geri dönüp bilgileri indirebilir ve köprüler ' i seçebilirsiniz. Güncelleştirilmiş kullanım koşulları hakkında daha fazla bilgi için bkz. [kullanım koşulları özelliği Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users).

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Yeni Azure AD etkinlik günlükleri indirme deneyimi kullanılabilir

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Artık Azure portal çok miktarda etkinlik günlüğünü doğrudan indirebilirsiniz. Bu güncelleştirme şunları yapmanızı sağlar:

- En fazla 250.000 satır indirin.

- İndirme tamamlandıktan sonra bildirim alın.

- Dosya adınızı özelleştirin.

- JSON veya CSV çıkış biçiminizi belirleme.

Bu özellik hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Azure Portal kullanarak bir denetim raporu yükleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report)

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Son değişiklik: Exchange ActiveSync (EAS) tarafından koşul değerlendirmesine yönelik güncelleştirmeler

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** koşullu erişim **ürün yeteneği:** Access Control

Exchange ActiveSync 'in (EAS) aşağıdaki koşulları nasıl değerlendirdiği hakkında güncelleştirme sürecimiz vardır:

- Ülke, bölge veya IP adresine göre Kullanıcı konumu

- Oturum açma riski

- Cihaz platformu

Bu koşulları daha önce koşullu erişim ilkeleriniz üzerinde kullandıysanız, koşul davranışının değişebilir olduğunu unutmayın. Örneğin, daha önce bir ilkede Kullanıcı konumu koşulunu kullandıysanız, ilkeyi artık kullanıcının konumuna göre atlandığını görebilirsiniz.

---

## <a name="february-2019"></a>Şubat 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Yapılandırılabilir Azure AD SAML belirteci şifrelemesi (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

Artık, desteklenen herhangi bir SAML uygulamasını şifrelenmiş SAML belirteçleri alacak şekilde yapılandırabilirsiniz. Azure AD, bir uygulamayla yapılandırılıp kullanıldığında, Azure AD 'de depolanan bir sertifikadan alınan bir ortak anahtar kullanarak, verilmiş SAML onaylamalarını şifreler.

SAML belirteci şifrelemeyi yapılandırma hakkında daha fazla bilgi için bkz. [Azure AD SAML belirteci şifrelemesini yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleri kullanarak gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** erişim gözden geçirmeleri **ürün yeteneği:** idare

Artık grup üyeliği veya uygulama ataması için tek bir Azure AD erişim gözden geçirmesine birden çok grup veya uygulama ekleyebilirsiniz. Birden çok grup veya uygulama içeren erişim gözden geçirmeleri aynı ayarlarla ayarlanır ve tüm dahil olan gözden geçirenler aynı anda bildirilir.

Azure AD erişim gözden geçirmeleri kullanarak erişim incelemesi oluşturma hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle grup veya uygulama erişimi Incelemesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Şubat 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Şubat 2019 ' de, uygulama galerisine federasyon desteği olan bu 27 yeni uygulamayı ekledik:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [Mindticyon](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT FINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [airstack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [iDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [ufuk ve qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Parlatidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [alerla](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight-cloudgate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), izin tıklama, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [storegatesmartfile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexıp](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [seismik](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [bir rüt](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [WebMethods Integration Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [her yerde LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope verileri](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [NetOp Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [purecg, Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), İK [üretkenlik platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="enhanced-combined-mfasspr-registration"></a>Geliştirilmiş Birleşik MFA/SSPR kaydı

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** self servis parola sıfırlama **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Müşteri geri bildirimlerine yanıt olarak, birleştirilmiş MFA/SSPR kayıt önizleme deneyimini geliştirdik. böylece kullanıcılarınız hem MFA hem de SSPR için güvenlik bilgilerini daha hızlı kaydetmeye yardımcı olur.

**Kullanıcılarınızın gelişmiş deneyimini bugün açmak için şu adımları izleyin:**

1. Genel yönetici veya Kullanıcı Yöneticisi olarak Azure portal oturum açın ve **> Kullanıcı ayarları ' na Azure Active Directory gidin > erişim paneli Önizleme özellikleri ayarlarını yönetin**.

2. **Güvenlik bilgilerini kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilecek kullanıcılar – Yenile** seçeneği için **Seçili bir Kullanıcı grubu** veya **tüm kullanıcılar**için özellikleri açmayı seçin.

Önümüzdeki birkaç hafta içinde, zaten açık olmayan kiracılar için eski Birleşik MFA/SSPR kayıt önizleme deneyimini açma imkanını kaldıracağız.

**Kiracınız için denetimin kaldırılıp kaldırılmadığını görmek için şu adımları izleyin:**

1. Genel yönetici veya Kullanıcı Yöneticisi olarak Azure portal oturum açın ve **> Kullanıcı ayarları ' na Azure Active Directory gidin > erişim paneli Önizleme özellikleri ayarlarını yönetin**.

2. **Güvenlik bilgilerini kaydetmek ve yönetmek için Önizleme özelliklerini kullanabilecek kullanıcılar** **hiçbiri**olarak ayarlanırsa, bu seçenek kiracınızdan kaldırılır.

Daha önce, kullanıcılar için eski Birleşik MFA/SSPR kayıt önizleme deneyimini önceden açtığınızdan bağımsız olarak, eski deneyim gelecekteki bir tarihte kapatılacak. Bu nedenle, yeni ve gelişmiş deneyimle mümkün olan en kısa sürede geçmeniz önemle tavsiye ederiz.

Gelişmiş kayıt deneyimi hakkında daha fazla bilgi için bkz. [Azure AD BIRLEŞIK MFA ve parola sıfırlama kayıt deneyimi için seyrek geliştirmeler](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271).

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Kullanıcı akışları için ilke yönetimi deneyimi güncelleştirildi

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Kullanıcı akışları (daha önce, yerleşik ilkeler) için ilke oluşturma ve yönetim işlemini daha kolay bir şekilde güncelleştirdik. Bu yeni deneyim artık tüm Azure AD kiracılarınız için varsayılandır.

Portal ekranının üst kısmındaki **bize geri bildirim gönder** alanındaki gülümseme veya kaş simgeleri kullanarak ek geri bildirim ve öneriler sağlayabilirsiniz.

Yeni ilke yönetimi deneyimi hakkında daha fazla bilgi için, [şimdi Azure AD B2C JavaScript özelleştirmesine ve birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahiptir.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Azure AD B2C tarafından sunulan belirli sayfa öğesi sürümlerini seçin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Artık, Azure AD B2C tarafından sunulan sayfa öğelerinin belirli bir sürümünü seçebilirsiniz. Belirli bir sürümü seçerek, güncelleştirmelerinizi bir sayfada görüntülenmeden önce test edebilir ve öngörülebilir bir davranış sağlayabilirsiniz. Ayrıca, artık JavaScript özelleştirmeleriyle ilgili belirli sayfa sürümlerini zorunlu kılabilirsiniz. Bu özelliği açmak için Kullanıcı akışlarınızda **Özellikler** sayfasına gidin.

Sayfa öğelerinin belirli sürümlerini seçme hakkında daha fazla bilgi için, [artık Azure AD B2C JavaScript özelleştirmesine ve birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahiptir.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>B2C için yapılandırılabilir Son Kullanıcı parolası gereksinimleri (GA)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Artık kuruluşunuzun parola karmaşıklığını, yerel Azure AD parola ilkenizi kullanmak yerine son kullanıcılarınız için ayarlayabilirsiniz. Kullanıcı akışlarınızın **Özellikler** dikey penceresinde (önceki adıyla yerleşik ilkeleriniz), **basit** veya **güçlü**bir parola karmaşıklığı seçebilir veya **özel** bir gereksinim kümesi oluşturabilirsiniz.

Parola karmaşıklığı gereksinim yapılandırması hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C parolalar için karmaşıklık gereksinimlerini yapılandırma](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Özel markalı kimlik doğrulama deneyimleri için yeni varsayılan şablonlar

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Kullanıcılarınız için özel bir markalı kimlik doğrulama deneyimi oluşturmak üzere Kullanıcı akışlarınızın **sayfa düzenleri** dikey penceresinde (daha önce yerleşik ilkeler olarak bilinir) bulunan yeni varsayılan şablonlarımızı kullanabilirsiniz.

Şablonları kullanma hakkında daha fazla bilgi için bkz. [Azure AD B2C artık JavaScript özelleştirmesi ve birçok yeni özellik içeriyor](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595).

---

## <a name="january-2019"></a>Ocak 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Tek seferlik geçiş kodu kimlik doğrulaması kullanarak B2B işbirliği Active Directory (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2B **ürün yeteneği:** B2B/B2C

Azure AD, Microsoft hesabı (MSA) veya Google Federasyonu gibi diğer yollarla kimlik doğrulamasından geçmediğiniz B2B Konuk kullanıcıları için bir kerelik geçiş kodu kimlik doğrulaması (OTP) sunuyoruz. Bu yeni kimlik doğrulama yöntemi, konuk kullanıcıların yeni bir Microsoft hesabı oluşturması gerekmediği anlamına gelir. Bunun yerine, bir daveti benimseme veya paylaşılan bir kaynağa erişirken, Konuk Kullanıcı bir e-posta adresine gönderilmek üzere geçici bir kod isteyebilir. Bu geçici kodu kullanarak Konuk Kullanıcı oturum açmaya devam edebilir.

Daha fazla bilgi için bkz. [e-posta bir kerelik geçiş kodu kimlik doğrulaması (Önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) ve blogda [Azure AD, herhangi bir hesabı kullanarak tüm kullanıcılar için paylaşım ve işbirliği yapma olanağı sunar](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949).

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Yeni Azure AD Uygulama Ara Sunucusu tanımlama bilgisi ayarları

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** Access Control

Uygulama proxy 'Si aracılığıyla yayınlanan uygulamalarınız için kullanılabilen üç yeni tanımlama bilgisi ayarı ekledik:

- **Yalnızca HTTP tanımlama bilgisini kullanın.** Uygulama proxy 'Si erişiminizin ve oturum tanımlama bilgilerinizdeki **HttpOnly** bayrağını ayarlar. Bu ayarı açmak, istemci tarafı komut dosyası aracılığıyla tanımlama bilgilerinin kopyalanmasını veya değiştirilmesini önlemeye yardımcı olmak gibi ek güvenlik avantajları sağlar. Bu bayrağı açmanızı öneririz ( **Evet**' i seçin).

- **Güvenli tanımlama bilgisini kullanın.** Uygulama proxy 'Si erişiminizin ve oturum tanımlama bilgilerinde **güvenli** bayrağını ayarlar. Bu ayarı açmak, tanımlama bilgilerinin yalnızca HTTPS gibi TLS güvenli kanallarından aktarılmasını sağlamak için ek güvenlik avantajları sağlar. Bu bayrağı açmanızı öneririz ( **Evet**' i seçin).

- **Kalıcı tanımlama bilgisini kullanın.** Web tarayıcısı kapalıyken erişim tanımlama bilgilerinin süresinin dolmasını önler. Bu tanımlama bilgileri, erişim belirtecinin ömrü için en son. Ancak, süre sonu zamanına ulaşıldığında veya Kullanıcı tanımlama bilgisini el ile silerse tanımlama bilgileri sıfırlanır. Varsayılan ayar olarak, yalnızca süreçler arasında tanımlama bilgilerini paylaşmayan eski uygulamalar **için ayarı açarak**yapmanız önerilir.

Yeni tanımlama bilgileri hakkında daha fazla bilgi için bkz. [Azure Active Directory ' de şirket içi uygulamalara erişmek Için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ocak 2019

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Ocak 'ta, uygulama galerisine federasyon desteğiyle bu 35 yeni uygulamalar ekledik:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Fize](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Tatatci paleti](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial),, [cloudsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco şemsiye](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet Erişim Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [süre sonu anımsatıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [EVR Görüntüleyicisi](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [Corptax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [Verb](https://app.verb.net/login), [OPENLATLET](https://openlattice.com/agora), [theorgwiki](https://www.theorgwiki.com/signup), [pavaso DIJITAL kapatma](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [Goodalıştırma araç seti](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [Cloud Service Picco](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [denetimpanosu](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [Callbir](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE serviceınsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [Ares](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [for Enterprise, for Office 365](https://www.k2.com/O365), [xledger](https://www.xledger.net/), [IDID Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [highdişli](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [K2](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Fraz alp](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Visitly csaas platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Yeni Azure AD Kimlik Koruması geliştirmeleri (Genel Önizleme)

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Aşağıda aşağıdakiler dahil olmak üzere Azure AD Kimlik Koruması genel önizleme sunumuna aşağıdaki geliştirmeleri eklediğimiz duyurumuz ediyoruz.

- Güncelleştirilmiş ve daha tümleşik bir kullanıcı arabirimi

- Ek API’ler

- Machine Learning ile iyileştirilmiş risk değerlendirmesi

- Riskli kullanıcılar ve riskli oturum açma işlemleri arasında ürün genelinde hizalama

Geliştirmeler hakkında daha fazla bilgi için bkz. [ne Azure Active Directory kimlik koruması (YENİLENDİ)?](https://aka.ms/IdentityProtectionDocs) daha fazla bilgi edinmek ve fikirlerinizi ürün içi istemler aracılığıyla paylaşmak için.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>İOS ve Android cihazlarda Microsoft Authenticator uygulaması için yeni uygulama kilidi özelliği

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Microsoft Authenticator uygulama **ürün yeteneği:** kimlik güvenliği & koruması

Tek seferlik geçiş kodlarınızın, uygulama bilgilerinizin ve uygulama ayarlarının daha güvenli olmasını sağlamak için Microsoft Authenticator uygulamasındaki uygulama kilidi özelliğini açabilirsiniz. Uygulama kilidi 'ni açmak, Microsoft Authenticator uygulamasını her açışınızda PIN 'inizi veya biyometri kullanarak kimlik doğrulaması yapmanız istenecektir.

Daha fazla bilgi için bkz. [uygulama sss Microsoft Authenticator](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Gelişmiş Azure AD Privileged Identity Management (PıM) dışa aktarma özellikleri

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Privileged Identity Management **ürün özelliği:** Privileged Identity Management

Privileged Identity Management (PıM) yöneticileri artık tüm alt kaynaklar için rol atamaları içeren belirli bir kaynak için tüm etkin ve uygun rol atamalarını dışarı aktarabilir. Daha önce, yöneticilerin bir abonelik için rol atamalarının tamamen bir listesini alması ve her belirli kaynak için rol atamalarını dışarı aktarmaları gerekiyordu.

Daha fazla bilgi için bkz. [PIM 'de Azure Kaynak rolleri için etkinliği ve denetim geçmişini görüntüleme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

## <a name="novemberdecember-2018"></a>Kasım/Aralık 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Eşitleme kapsamından kaldırılan kullanıcılar artık yalnızca bulut hesaplarına geçmesiz

**Şunu yazın:** Sabit **hizmet kategorisi:** Kullanıcı Yönetimi **ürün yeteneği:** Dizin

>[!Important]
>Bu çözüm nedeniyle rahatsız etmemizi duyduk ve anladık. Bu nedenle, bu değişikliği kuruluşunuzda uygulamanızı daha kolay hale getirmek için bu süre kadar geri döndürüyoruz.

Active Directory Domain Services (AD DS) nesnesi eşitleme kapsamından dışlandıysa ve sonra aşağıdaki eşitleme döngüsündeki Azure AD 'ye geri dönüşüm kutusu 'na taşındıktan sonra bir kullanıcının DirSyncEnabled bayrağının **hatalı olarak yanlış** olarak geçtiğimiz bir hata düzeltildi. Bu düzeltmeyle ilgili olarak, Kullanıcı eşitleme kapsamından dışlandığından ve daha sonra Azure AD geri dönüşüm kutusu 'ndan geri yüklenirse, Kullanıcı hesabı şirket içi AD 'den eşitlenmiş olarak kalır ve bu nedenle, yetki kaynağı (SoA) Şirket içi AD olarak kaldığı için bulutta yönetilemez.

Bu düzeltmeyle önce, DirSyncEnabled bayrağı false olarak geçildiğinde bir sorun oluştu. Bu hesaplar, bu hesapların yalnızca bulut nesnelerine dönüştürüldüğüne ve hesapların bulutta yönetilebileceğinden yanlış izlenimi verdi. Ancak, hesaplar hala SoA 'yi şirket içinde ve şirket içi AD 'den gelen tüm eşitlenmiş Özellikler (gölge öznitelikleri) olarak bekletilir. Bu durum, Azure AD 'de ve diğer bulut iş yüklerinde (Exchange Online gibi), bu hesapların AD 'den eşitlenmiş olduğunu ancak artık yalnızca bulut hesapları gibi davranmasının beklendiği bir şekilde daha fazla soruna neden oldu.

Şu anda, eşitlenmiş bir AD hesabını gerçekten yalnızca bulut hesabına dönüştürmenin tek yolu, bir arka uç işlemini SoA aktarmaya tetikleyen kiracı düzeyinde DirSync 'i devre dışı bırakmalıdır. Bu tür SoA değişikliği, şirket içi ilgili tüm öznitelikleri (LastDirSyncTime ve Shadow öznitelikleri gibi) temizlemek ve ilgili nesnenin kendisini yalnızca bulutta bir hesaba dönüştürmek üzere diğer bulut iş yüklerine bir sinyal göndermek için gereklidir (ancak bunlarla sınırlı değildir).

Bu, sonuçta, geçmişteki bazı senaryolarda, AD 'den eşitlenen bir kullanıcının ImmutableID özniteliğinde doğrudan güncelleştirmelerin yapılmasını önler. Tasarım yaparak, adın gösterdiği gibi Azure AD 'deki bir nesnenin ImmutableID, sabit olması anlamına gelir. Azure AD Connect Health ve Azure AD Connect eşitleme istemcisinde uygulanan yeni özellikler, bu senaryolara yönelik olarak kullanılabilir:

- **Aşamalı bir yaklaşımda çok sayıda kullanıcı için büyük ölçekli ImmutableID güncelleştirmesi**

  Örneğin, uzun AD DS ormanlar arası geçiş yapmanız gerekir. Çözüm: **kaynak bağlantısını yapılandırmak** Için Azure AD Connect kullanın ve Kullanıcı geçirildiğinde, mevcut ImmutableID değerlerini Azure AD DS ad 'den yeni ormanın ms-DS-tutarlılık-GUID özniteliğine kopyalayın. Daha fazla bilgi için bkz. [MS-DS-ımıbu GUID 'ı Sourcetutturucu olarak kullanma](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor).

- **Tek bir görüntüsündeki birçok kullanıcı için büyük ölçekli ImmutableID güncelleştirmeleri**

  Örneğin, Azure AD Connect uygulanırken bir hata yaparsınız ve şimdi Sourcetutturucu özniteliğini değiştirmeniz gerekir. Çözüm: kiracı düzeyinde DirSync 'i devre dışı bırakın ve tüm geçersiz ImmutableID değerlerini temizleyin. Daha fazla bilgi için bkz. [Office 365 için dizin eşitlemeyi devre dışı bırakma](/office365/enterprise/turn-off-directory-synchronization).

- **Azure AD 'de mevcut bir kullanıcıyla şirket içi Kullanıcı Ile yeniden eşleştir** Örneğin, AD DS ' de yeniden oluşturulmuş bir kullanıcı var olan bir Azure AD hesabıyla (yalnız bırakılmış nesne) yeniden eşleştirmek yerine Azure AD hesabında yinelenen bir öğe oluşturur. Çözüm: kaynak bağlayıcısını/ImmutableID yeniden eşlemek için Azure portal Azure AD Connect Health kullanın. Daha fazla bilgi için bkz. [yalnız bırakılmış nesne senaryosu](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario).

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Son değişiklik: Azure Izleyici aracılığıyla denetim ve oturum açma günlüğü şemasında yapılan güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure Izleyici aracılığıyla hem denetim hem de oturum açma günlüğü akışlarını yayımlıyoruz, böylece günlük dosyalarını SıEM araçlarınızla veya Log Analytics ile sorunsuzca tümleştirebilirsiniz. Geri bildirimlerinize göre ve bu özelliğin genel kullanılabilirlik duyurusunu hazırlıyoruz, şemanızda aşağıdaki değişiklikleri yapıyoruz. Bu şema değişiklikleri ve ilgili belge güncelleştirmeleri, Ocak 'un ilk haftası tarafından gerçekleşir.

#### <a name="new-fields-in-the-audit-schema"></a>Denetim şemasındaki yeni alanlar
Kaynak üzerinde gerçekleştirilen işlem türünü sağlamak için yeni bir **Işlem türü** alanı ekliyoruz. Örneğin, **ekleme**, **güncelleştirme**veya **silme**.

#### <a name="changed-fields-in-the-audit-schema"></a>Denetim şemasındaki alanları değiştirme
Aşağıdaki alanlar denetim şemasında değişiyor:

|Alan adı|Değişen|Eski değerler|Yeni değerler|
|----------|------------|----------|----------|
|Kategori|Bu **hizmet adı** alanıdır. Artık **denetim kategorileri** alanıdır. **Hizmet adı** **Loggedbyservice** alanı olarak yeniden adlandırıldı.|<ul><li>Hesap Sağlama</li><li>Çekirdek Dizin</li><li>Self servis parola sıfırlama</li></ul>|<ul><li>Kullanıcı Yönetimi</li><li>Grup Yönetimi</li><li>Uygulama yönetimi</li></ul>|
|targetResources|En üst düzeyde **Targetresourcetype** içerir.|&nbsp;|<ul><li>İlke</li><li>Uygulama</li><li>Kullanıcı</li><li>Grup</li></ul>|
|loggedByService|Denetim günlüğünü oluşturan hizmetin adını sağlar.|Null|<ul><li>Hesap Sağlama</li><li>Çekirdek Dizin</li><li>Self servis parola sıfırlama</li></ul>|
|Sonuç|Denetim günlüklerinin sonucunu sağlar. Bu, daha önce numaralandırılmıştı, ancak şimdi gerçek değeri gösteriyoruz.|<ul><li>0</li><li>1</li></ul>|<ul><li>Başarılı</li><li>Hata</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>Oturum açma şemasında değiştirilen alanlar
Aşağıdaki alanlar, oturum açma şemasında değişiyor:

|Alan adı|Değişen|Eski değerler|Yeni değerler|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|Bu **Conditionalaccesspolicies** alanıdır. Artık **appliedConditionalAccessPolicies** alanıdır.|düzeltme sınıfı,|düzeltme sınıfı,|
|conditionalAccessStatus|Oturum açma sırasında koşullu erişim Ilkesi durumunun sonucunu sağlar. Bu, daha önce numaralandırılmıştı, ancak şimdi gerçek değeri gösteriyoruz.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|
|appliedConditionalAccessPolicies: sonuç|Oturum açma sırasında bireysel koşullu erişim Ilkesi durumunun sonucunu sağlar. Bu, daha önce numaralandırılmıştı, ancak şimdi gerçek değeri gösteriyoruz.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|

Şema hakkında daha fazla bilgi için bkz. [Azure izleyici 'de Azure AD denetim günlükleri şemasını yorumlama (Önizleme)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Denetimli makine öğrenimi modeli ve risk puanı altyapısında kimlik koruması geliştirmeleri

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** risk puanları

Kimlik koruması ile ilgili Kullanıcı ve oturum açma risk değerlendirmesi altyapısı geliştirmeleri, Kullanıcı risk doğruluğu ve kapsamını geliştirmeye yardımcı olabilir. Yöneticiler, Kullanıcı risk düzeyinin artık belirli algılamaların risk düzeyine doğrudan bağlı olmadığını ve riskli oturum açma olaylarının sayısı ve düzeyinde bir artış olduğunu fark edebilir.

Risk algılamaları artık kullanıcının oturum açma işlemlerinin ek özelliklerini ve bir algılama düzenlerini kullanarak Kullanıcı riskini hesaplayan, denetimli makine öğrenimi modeli tarafından değerlendirilir. Yönetici, bu modele bağlı olarak, bu kullanıcıyla ilişkili algılamalar düşük veya orta riskli olsa bile, yüksek riskli puanları olan kullanıcılar tarafından bulunabilir.

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Yöneticiler, Microsoft Authenticator uygulamasını kullanarak kendi parolalarını sıfırlayabilir (Genel Önizleme)

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** self servis parola sıfırlama **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD yöneticileri artık Microsoft Authenticator uygulama bildirimlerini veya herhangi bir mobil kimlik doğrulayıcı uygulamasının veya donanım belirtecinden bir kodu kullanarak kendi parolasını sıfırlayabilir. Yöneticiler, kendi parolasını sıfırlamak için şu yöntemlerden ikisini de kullanabilir:

- Microsoft Authenticator uygulama bildirimi

- Diğer mobil kimlik doğrulayıcı uygulaması/donanım belirteci kodu

- E-posta

- Telefon görüşmesi

- Kısa mesaj

Parolaları sıfırlamak için Microsoft Authenticator uygulamasını kullanma hakkında daha fazla bilgi için bkz. [Azure AD self servis parola sıfırlama-mobil uygulama ve SSPR (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Yeni Azure AD bulut aygıtı yönetici rolü (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** cihaz kaydı ve Yönetim **ürün yeteneği:** erişim denetimi

Yöneticiler, bulut aygıtı yönetici görevlerini gerçekleştirmek için kullanıcıları yeni bulut cihaz yöneticisi rolüne atayabilir. Bulut cihaz yöneticileri rolüne atanan kullanıcılar, Azure AD 'de cihazları etkinleştirebilir, devre dışı bırakabilir ve silebilir ve Azure portal Windows 10 BitLocker anahtarlarını (varsa) okuyabilir.

Roller ve izinler hakkında daha fazla bilgi için bkz. [Azure Active Directory yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Azure AD 'de yeni etkinlik zaman damgasını kullanarak cihazlarınızı yönetin (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** cihaz kaydı ve Yönetim **ürün yeteneği:** cihaz yaşam döngüsü yönetimi

Ortamınızda eski cihazları kullanmaktan kaçınmak için, Azure AD 'de kuruluşların cihazlarını yenilemeniz ve devre dışı bırakmanız gerektiğini fark ettik. Azure AD, bu işleme yardımcı olmak için artık cihazlarınızı yeni bir etkinlik zaman damgasıyla güncelleştirir ve cihaz yaşam döngüsünü yönetmenize yardımcı olur.

Bu zaman damgasını alma ve kullanma hakkında daha fazla bilgi için bkz [. nasıl yapılır: Azure AD 'de eski cihazları yönetme](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Yöneticiler, kullanıcıların her cihazda kullanım koşullarını kabul etmesini gerektirebilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** idare

Yöneticiler artık kullanıcılarınızın kiracınızda kullandıkları her cihazda kullanım koşullarınızı kabul etmesini gerektirmek için **kullanıcıların her cihazda izin vermesini iste** seçeneğini açabilir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin cihaz başına kullanım koşulları bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Yöneticiler, yinelenen bir zamanlamaya göre kullanım koşulları 'nı zaman dolacak şekilde yapılandırabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** idare


Yöneticiler artık, belirtilen yinelenen zamanlamanıza bağlı olarak tüm kullanıcılarınız için kullanım süresini dolacak şekilde **süre sonu onayları** seçeneğini açabilir. Zamanlama yıllık, iki yıllık, üç aylık veya aylık olabilir. Kullanım koşulları sona erdiğinde, kullanıcıların yeniden kabul etmesi gerekir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin kullanım koşulları ekleme bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Yöneticiler, her bir kullanıcının zamanlamasını temel alarak kullanım koşullarını yapılandırabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** idare

Yöneticiler artık kullanıcının kullanım koşullarını yeniden kabul etmesi için bir süre belirtebilir. Örneğin, Yöneticiler kullanıcıların her 90 günde bir kullanım koşulları 'nı yeniden kabul etmesi gerektiğini belirtebilir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin kullanım koşulları ekleme bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)bakın.

---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Azure Active Directory rolleri için yeni Azure AD Privileged Identity Management (PıM) e-postaları

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Privileged Identity Management **ürün özelliği:** Privileged Identity Management

Azure AD Privileged Identity Management (PıM) kullanan müşteriler artık son yedi güne ait aşağıdaki bilgiler dahil olmak üzere haftalık bir Özet e-postası alabilir:

- En uygun ve kalıcı rol atamalarına genel bakış

- Rolleri etkinleştiren kullanıcı sayısı

- PıM 'de rollere atanan kullanıcı sayısı

- PıM dışında rollere atanan kullanıcı sayısı

- PıM 'de "kalıcı hale getirilir" Kullanıcı sayısı

PıM ve kullanılabilir e-posta bildirimleri hakkında daha fazla bilgi için bkz. [PIM 'de e-posta bildirimleri](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications).

---

### <a name="group-based-licensing-is-now-generally-available"></a>Grup tabanlı lisanslama genel kullanıma sunuldu

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** Dizin

Grup tabanlı lisanslama genel önizleme aşamasındadır ve genel kullanıma sunulmuştur. Bu genel yayının bir parçası olarak, bu özelliği daha ölçeklenebilir hale aldık ve tek bir kullanıcı için grup tabanlı lisanslama atamalarını yeniden işleme özelliği ve Office 365 E3/a3 lisanslarıyla grup tabanlı lisanslama kullanma özelliği ekledik.

Grup tabanlı lisanslama hakkında daha fazla bilgi için bkz. [Azure Active Directory grup tabanlı lisanslama nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Kasım 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

2018 Kasım 'da, uygulama galerisine federasyon desteğiyle bu 26 yeni uygulama ekledik:

[Corestack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [Getno](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [GRA-PE](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [ehour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [appınux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [drivedolar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview), [useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [sonsuz kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [alaya](https://alayagood.com), [heyıarkadaş](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [zenegy for Business Central 365](https://accounting.zenegy.com/), [yaprak Köprüsü üye portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [allbağlanacak SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [parçalı uygulamalar-klasik test](https://test.plexonline.com/signon), [parçalı uygulamalar](https://www.plexonline.com/signon)– bir, parçalı uygulamalar [-UX testi](https://test.cloud.plex.com/sso), [parçalı uygulamalar – UX](https://cloud.plex.com/sso), [parçalı uygulamalar – IAM](https://accounts.plex.com/), [öğelerini-childilgilenme kayıtları, katılım, & finansal izleme sistemi](https://getcrafts.ca/craftsregistration)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

## <a name="october-2018"></a>Ekim 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD günlükleri artık Azure Log Analytics ile çalışıyor (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure AD günlüklerinizi artık Azure Log Analytics ilettiğini duyurmaktan mutluluk duyuyoruz! Bu en iyi istenen özellik, iş, işlemler ve güvenlik için Analize daha iyi erişim sağlamanıza yardımcı olur ve altyapınızı izlemeye yardımcı olmak için bir yoldur. Daha fazla bilgi için bkz. [Azure 'Daki etkinlik günlüklerine Azure Active Directory artık blog Log Analytics](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) .

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ekim 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

2018 Ekim 'de, uygulama galerisine federasyon desteği olan bu 14 yeni uygulamaları ekledik:

[Ikramiye noktalarım](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), Ambyint, [myworkdrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [ödünç kutusu](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), dialpad, [ON24 sanal ortamı](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [halka orta](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler üç](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [workspot denetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="azure-ad-domain-services-email-notifications"></a>E-posta bildirimlerini Azure AD Domain Services

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD Domain Services **ürün özelliği:** Azure AD Domain Services

Azure AD Domain Services, yanlış yapılandırma veya yönetilen etki alanı ile ilgili sorunlar hakkında Azure portal uyarılar sağlar. Bu uyarılar, destek ile iletişime geçerek sorunları gidermeyi deneyebilmeniz için adım adım kılavuzlar içerir.

Ekim 'den başlayarak, yönetilen etki alanınız için bildirim ayarlarını özelleştirerek yeni uyarılar gerçekleştiğinde, belirlenen bir kişi grubuna e-posta gönderilir ve portalda güncelleştirmeler için sürekli olarak denetlenmesi gereğini ortadan kaldırır.

Daha fazla bilgi için [Azure AD Domain Services bildirim ayarları](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications)bölümüne bakın.

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD Portal, özel etki alanlarını silmek için ForceDelete etki alanı API 'sini kullanmayı destekler

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Dizin Yönetimi **ürün yeteneği:** Dizin

Özel etki alanı adınızdan (contoso.com) bulunan kullanıcılar, gruplar ve uygulamalar gibi başvuruları, ilk varsayılan etki alanı adına (contoso.onmicrosoft.com) geri yükleyerek özel etki alanı adlarınızı silmek için artık ForceDelete etki alanı API 'sini kullanabilirsiniz.

Bu değişiklik, kuruluşunuz artık adı kullanmadıysa veya başka bir Azure AD ile etki alanı adını kullanmanız gerekiyorsa, özel etki alanı adlarınızı daha hızlı bir şekilde silmenizi sağlar.

Daha fazla bilgi için bkz. [özel etki alanı adını silme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>Eylül 2018

### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Dinamik Gruplar için yönetici rolü izinleri güncelleştirildi

**Şunu yazın:** Sabit **hizmet kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Belirli Yönetici rollerinin artık grubun sahibi olması gerekmeden dinamik üyelik kuralları oluşturup güncelleştirebilmesi için bir sorunu düzelttik.

Roller şunlardır:

- Genel yönetici

- Intune yöneticisi

- Kullanıcı yöneticisi

Daha fazla bilgi için bkz. [dinamik grup oluşturma ve durumu denetleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Bazı üçüncü taraf uygulamalar için Basitleştirilmiş çoklu oturum açma (SSO) yapılandırma ayarları

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

Hizmet olarak yazılım (SaaS) uygulamaları için çoklu oturum açma (SSO) ayarlamanın, her bir uygulama yapılandırmasının benzersiz yapısı nedeniyle zorlayıcı bir şekilde ayarlanmasını fark ettik. Aşağıdaki üçüncü taraf SaaS uygulamalarına yönelik SSO yapılandırma ayarlarını otomatik olarak doldurmak için basitleştirilmiş bir yapılandırma deneyimi geliştirdik:

- Zendesk

- Çevrimiçi olan ArcGIS

- Jamf Pro

Bu tek tıklamayla deneyim 'i kullanmaya başlamak için, uygulamanın **Azure Portal**  >  **SSO yapılandırma** sayfasına gidin. Daha fazla bilgi için bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Azure Active Directory-verileriniz nerede bulunuyor? sayfasında

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** altın ocal

Tüm Azure AD Hizmetleri için Azure AD verilerinizi bekleyen Azure veri merkezini görüntülemek için **Azure Active Directory** şirketinizin bölgesini seçin. Bilgileri şirketinizin bölgesi için belirli Azure AD hizmetlerine göre filtreleyebilirsiniz.

Bu özelliğe erişmek ve daha fazla bilgi için, bkz. [Azure Active Directory-verileriniz nerede bulunur](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Uygulamalarım erişim paneli için yeni dağıtım planı kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulamalarım **ürün yeteneği:** SSO

Uygulamalarım erişim paneli için kullanılabilen yeni dağıtım planına göz atın ( https://aka.ms/deploymentplans) .
Uygulamalarım erişim paneli kullanıcılara uygulamaları bulmak ve bunlara erişmek için tek bir yer sağlar. Bu portal Ayrıca kullanıcılara, uygulamalar ve gruplar için erişim isteme ya da diğer kişilerin adına bu kaynaklara erişimi yönetme gibi self servis fırsatları sağlar.

Daha fazla bilgi için bkz. [uygulamalarım portalı nedir?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Azure portal oturum açma günlükleri sayfasında yeni sorun giderme ve destek sekmesi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure portal **oturum açma** Işlemlerinin yeni **sorun giderme ve destek** sekmesi, yöneticilerin ve Destek mühendislerinin Azure AD oturum açma konularıyla ilgili sorunları gidermelerine yardımcı olmak için tasarlanmıştır. Bu yeni sekme, sorunu çözmeye yardımcı olmak için hata kodu, hata iletisi ve düzeltme önerilerini (varsa) sağlar. Sorunu çözemezse, bir destek bileti **oluşturmak için,** destek biletinizdeki günlük dosyası IÇIN **Istek kimliği** ve **Tarih (UTC)** alanlarını dolduran bir destek bileti oluşturmaya yönelik yeni bir yol da sunuyoruz.

![Yeni sekmeyi gösteren oturum açma günlükleri](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Dinamik üyelik kuralları oluşturmak için kullanılan özel uzantı özellikleri için gelişmiş destek

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Bu güncelleştirmeyle, artık dinamik Kullanıcı grubu kuralı oluşturucusunun **özel uzantı özellikleri al** bağlantısına tıklayabilir, BENZERSIZ uygulama kimliğinizi girebilir ve kullanıcılar için dinamik üyelik kuralı oluştururken kullanmak üzere özel uzantı özelliklerinin tam listesini alabilirsiniz. Bu liste ayrıca, bu uygulama için yeni özel uzantı özellikleri almak üzere yenilenebilir.

Dinamik üyelik kuralları için özel uzantı özellikleri kullanma hakkında daha fazla bilgi için bkz. [uzantı özellikleri ve özel uzantı özellikleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** koşullu erişim **ürün yeteneği:** kimlik güvenliği ve koruma

Aşağıdaki uygulamalar [onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesidir:

- Microsoft To-Do

- Microsoft Stream

Daha fazla bilgi için bkz.

- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Windows 7/8/8.1 kilit ekranından self servis parola sıfırlama için yeni destek

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** SSPR **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu yeni özelliği ayarladıktan sonra kullanıcılarınız, Windows 7, Windows 8 veya Windows 8.1 çalıştıran bir cihazın **kilit** ekranından parolalarını sıfırlama bağlantısını görür. Bu bağlantıya tıkladığınızda, Kullanıcı Web tarayıcısı aracılığıyla aynı parola sıfırlama akışında gezinerek yapılır.

Daha fazla bilgi için bkz. [Windows 7, 8 ve 8,1 ' de parola sıfırlamayı etkinleştirme](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: yetkilendirme kodları artık yeniden kullanım için kullanılamayacak

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** kimlik doğrulamaları (oturum açma) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

15 Kasım 2018 tarihinden itibaren Azure AD, uygulamalar için önceden kullanılan kimlik doğrulama kodlarını kabul etmeyi durduracak. Bu güvenlik değişikliği, Azure AD 'yi OAuth belirtimine göre satıra getirmeye yardımcı olur ve hem v1 hem de v2 uç noktalarında zorunlu kılınır.

Uygulamanız birden fazla kaynağa belirteç almak için yetkilendirme kodlarını yeniden kullanıyorsa, bir yenileme belirteci almak için kodu kullanmanızı ve ardından bu yenileme belirtecini kullanarak diğer kaynaklar için ek belirteçler elde etmenizi öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden çok kaynak arasında birden çok kez kullanılabilir. OAuth kod akışı sırasında bir kimlik doğrulama kodunu yeniden kullanmayı deneyen bir uygulama invalid_grant bir hata alır.

Bu ve diğer protokollerle ilgili değişiklikler için, [kimlik doğrulaması yenilikleri hakkında tam listeye](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Eylül 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Eylül 2018 ' de, uygulama galerisine federasyon desteğiyle bu 16 yeni uygulamayı ekledik:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Cokarşılayan işe alma yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), ArcGIS [Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [jda Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [kar ke](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), Join.me, [Zephyrsso](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [SILVERBACK](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), riveryatak Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Azure için enlyft SSO, [Palatino,](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial) [dmarcıa](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="support-for-additional-claims-transformations-methods"></a>Ek talep dönüştürmeleri yöntemleri için destek

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

SAML tabanlı **Çoklu oturum açma yapılandırması** sayfasından SAML belirteçlerine uygulanabilen yeni talep dönüştürme yöntemleri (ToLower () ve ToUpper () sunuyoruz.

Daha fazla bilgi için bkz [. Azure AD 'de kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>SAML tabanlı uygulama yapılandırma kullanıcı arabirimi (Önizleme) güncelleştirildi

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

Güncelleştirilmiş SAML tabanlı uygulama yapılandırma Kullanıcı arabirimimizin bir parçası olarak şunları alacaksınız:

- SAML tabanlı uygulamalarınızı yapılandırmaya yönelik güncelleştirilmiş bir gözden geçirme deneyimi.

- Yapılandırmanızda nelerin eksik olduğu veya yanlış olduğu hakkında daha fazla görünürlük.

- Süre sonu sertifikası bildirimi için birden çok e-posta adresi ekleme özelliği.

- Yeni talep dönüştürme yöntemleri, ToLower () ve ToUpper () ve daha fazlası.

- Kurumsal uygulamalarınız için kendi belirteç imzalama sertifikanızı karşıya yüklemenin bir yolu.

- SAML uygulamaları için NameID biçimini ayarlamak için bir yol ve NameID değerini Dizin uzantıları olarak ayarlamak için bir yol.

Bu güncelleştirilmiş görünümü açmak için **Çoklu oturum açma** sayfasının en üstündeki **Yeni deneyimimizi deneyin** bağlantısına tıklayın. Daha fazla bilgi için bkz. [öğretici: Azure Active Directory olan bir uygulama IÇIN SAML tabanlı çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications).

---

## <a name="august-2018"></a>Ağustos 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP adresi aralıklarına yapılan değişiklikler

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** diğer **ürün yeteneği:** platform

Azure AD 'ye daha büyük IP aralıkları sunuyoruz, yani güvenlik duvarları, yönlendiricileriniz veya ağ güvenlik grupları için Azure AD IP adresi aralıklarını yapılandırdıysanız, bunları güncelleştirmeniz gerekir. Bu güncelleştirmeyi yaptığımız için, Azure AD yeni uç noktalar eklediğinde güvenlik duvarı, yönlendirici veya ağ güvenlik grupları IP aralığı yapılandırmalarının yeniden değiştirilmesini istemezsiniz.

Ağ trafiği, sonraki iki ay içinde bu yeni aralığa taşınıyor. Kesintisiz hizmet ile devam etmek için, bu güncelleştirilmiş değerleri 10 Eylül 2018 ' dan önce IP adreslerinize eklemeniz gerekir:

- 20.190.128.0/18

- 40.126.0.0/18

Tüm ağ trafiğiniz yeni aralıklara taşınana kadar eski IP adresi aralıklarını kaldırmamak kesinlikle önerilir. Taşıma hakkındaki güncelleştirmeler ve eski aralıkları ne zaman kaldırabileceğinizi öğrenmek için bkz. [Office 365 URL 'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: yetkilendirme kodları artık yeniden kullanım için kullanılamayacak

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** kimlik doğrulamaları (oturum açma) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

15 Kasım 2018 tarihinden itibaren Azure AD, uygulamalar için önceden kullanılan kimlik doğrulama kodlarını kabul etmeyi durduracak. Bu güvenlik değişikliği, Azure AD 'yi OAuth belirtimine göre satıra getirmeye yardımcı olur ve hem v1 hem de v2 uç noktalarında zorunlu kılınır.

Uygulamanız birden fazla kaynağa belirteç almak için yetkilendirme kodlarını yeniden kullanıyorsa, bir yenileme belirteci almak için kodu kullanmanızı ve ardından bu yenileme belirtecini kullanarak diğer kaynaklar için ek belirteçler elde etmenizi öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden çok kaynak arasında birden çok kez kullanılabilir. OAuth kod akışı sırasında bir kimlik doğrulama kodunu yeniden kullanmayı deneyen bir uygulama invalid_grant bir hata alır.

Bu ve diğer protokollerle ilgili değişiklikler için, [kimlik doğrulaması yenilikleri hakkında tam listeye](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.

---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Self servis parola (SSPR) ve Multi-Factor Authentication (MFA) için yakınsanmış güvenlik bilgi yönetimi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** SSPR **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu yeni özellik, kullanıcıların güvenlik bilgilerini (örneğin, telefon numarası, mobil uygulama vb.) SSPR ve MFA için tek bir konum ve deneyimle yönetmesine yardımcı olur; daha önce olduğu gibi, iki farklı konumda yapıldığı gibi.

Bu yakınsama deneyimi Ayrıca SSPR veya MFA kullanan kişiler için de geçerlidir. Ayrıca, kuruluşunuz MFA veya SSPR kaydını zorlayamazsa, kullanıcılar uygulama tarafından izin verilen tüm MFA veya SSPR güvenlik bilgileri yöntemlerini uygulamalarım portalından kaydedebilir.

Bu, isteğe bağlı bir genel önizlemedir. Yöneticiler seçili bir grup veya bir Kiracıdaki tüm kullanıcılar için yeni deneyimi (isteniyorsa) açabilir. Yakınsama deneyimi hakkında daha fazla bilgi için, bkz. [Yakınsanan deneyim blogu](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD uygulama ara sunucusu uygulamalarında yalnızca yeni HTTP tanımlama bilgileri ayarı

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** Access Control

Uygulama ara sunucusu uygulamalarınızda **yalnızca http tanımlama bilgileri** adlı yeni bir ayar vardır. Bu ayar, hem uygulama proxy 'Si erişimi hem de oturum tanımlama bilgileri için HTTP yanıt üst bilgisine HTTPOnly bayrağını ekleyerek ek güvenlik sağlanmasına yardımcı olur, istemci tarafı betikten tanımlama bilgisine erişimi durduruyor ve tanımlama bilgisini kopyalama ya da değiştirme gibi eylemleri daha da engelliyor. Bu bayrak daha önce kullanılmasa da, tanımlama bilgileriniz her zaman hatalı değişikliklere karşı korumaya yardımcı olmak için bir TLS bağlantısı kullanılarak şifrelenir ve iletilir.

Bu ayar, Uzak Masaüstü gibi ActiveX denetimlerini kullanan uygulamalarla uyumlu değildir. Bu durumda, bu ayarı kapatmanız önerilir.

Yalnızca HTTP tanımlama bilgileri ayarı hakkında daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Azure kaynakları için Privileged Identity Management (PıM), yönetim grubu kaynak türlerini destekler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Privileged Identity Management **ürün özelliği:** Privileged Identity Management

Tam zamanında etkinleştirme ve atama ayarları artık abonelikler, kaynak grupları ve kaynaklar (VM 'Ler, uygulama hizmetleri ve daha fazlası gibi) için zaten yaptığınız gibi yönetim grubu kaynak türlerine uygulanabilir. Ayrıca, bir yönetim grubu için yönetici erişimi sağlayan role sahip olan herkes bu kaynağı PıM 'de bulabilir ve yönetebilir.

PıM ve Azure kaynakları hakkında daha fazla bilgi için bkz. [Privileged Identity Management kullanarak Azure kaynaklarını bulma ve yönetme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)

---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Uygulama erişimi (Önizleme) Azure AD portalına daha hızlı erişim sağlar

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Privileged Identity Management **ürün özelliği:** Privileged Identity Management

Bugün, PıM kullanarak bir rolü etkinleştirirken, izinlerin etkili olması 10 dakika sürebilir. Şu anda genel önizleme aşamasında olan uygulama erişimini kullanmayı seçerseniz, Yöneticiler, etkinleştirme isteği tamamlandıktan hemen sonra Azure AD portalına erişebilir.

Şu anda uygulama erişimi yalnızca Azure AD Portal deneyimini ve Azure kaynaklarını destekler. PıM ve uygulama erişimi hakkında daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ağustos 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Ağustos 2018 ' de, uygulama galerisine federasyon desteğiyle bu 16 yeni uygulamayı ekledik:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [bridgeline bağlanmamış](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sauce Labs-mobil ve Web testi](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [meta ağlar Bağlayıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [yaptığımız](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [Promaster (ınlogik tarafından)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [dossıer](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F-harcama raporları](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 canlı sohbet](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [safeconnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [zenqms](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eluminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Yerel Tableau desteği artık Azure AD Uygulama Ara Sunucusu kullanılabilir

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** Access Control

OpenID 'deki güncelleştirmemizden, ön kimlik doğrulama protokoliz için OAuth 2,0 kod verme protokolüne bağlanın, artık uygulama proxy 'Si ile Tableau kullanmak için ek yapılandırma yapmanız gerekmez. Bu protokol değişikliği Ayrıca, JavaScript ve HTML etiketlerinde yaygın olarak desteklenen yalnızca HTTP yeniden yönlendirmeleri kullanarak, uygulama proxy 'Sinin daha modern uygulamaları daha iyi desteklemesini sağlar.

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) içinde B2B Konuk kullanıcıları için bir kimlik sağlayıcısı olarak Google eklemek üzere yeni destek

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2B **ürün yeteneği:** B2B/B2C

Kuruluşunuzda Google ile Federasyon ayarlayarak, bir kişisel Microsoft hesabı (MSAs) veya bir Azure AD hesabı oluşturmaya gerek kalmadan, Gmail kullanıcılarının mevcut Google hesabını kullanarak paylaşılan uygulamalarınızda ve kaynaklarda oturum açmasını sağlayabilirsiniz.

Bu, isteğe bağlı bir genel önizlemedir. Google Federasyonu hakkında daha fazla bilgi için bkz. [B2B Konuk kullanıcıları için kimlik sağlayıcısı olarak Google ekleme](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Temmuz 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Azure Active Directory e-posta bildirimlerine yönelik iyileştirmeler

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

Azure Active Directory (Azure AD) e-postaları artık güncelleştirilmiş bir tasarımın yanı sıra, aşağıdaki hizmetlerden gönderildiğinde gönderenin e-posta adresi ve gönderen görünen adı üzerinde değişiklik gösterir:

- Azure AD erişim Incelemeleri
- Azure AD Connect Health
- Azure AD Kimlik Koruması
- Azure AD Privileged Identity Management
- Kurumsal uygulamanın süresi dolan sertifika bildirimleri
- Kurumsal uygulama sağlama hizmeti bildirimleri

E-posta bildirimleri şu e-posta adresinden ve görünen adla gönderilir:

- E-posta adresi:azure-noreply@microsoft.com
- Görünen ad: Microsoft Azure

Yeni e-posta tasarımlarının bir örneği ve daha fazla bilgi için bkz. [Azure AD PIM 'de e-posta bildirimleri](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD etkinlik günlükleri artık Azure Izleyici aracılığıyla kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Azure AD etkinlik günlükleri artık Azure Izleyici (Azure 'un platform genelinde izleme hizmeti) için genel önizlemede kullanıma sunulmuştur. Azure Izleyici, bu geliştirmelere ek olarak uzun süreli saklama ve sorunsuz tümleştirme sağlar:

- Günlük dosyalarınızı kendi Azure depolama hesabınıza yönlendirerek uzun süreli saklama.

- Sorunsuz SıEM tümleştirmesi, Özel betikler yazmanızı veya bakımını gerektirmenize gerek kalmadan.

- Kendi özel çözümlerinizle, analiz araçlarınızla veya olay yönetimi çözümleriyle sorunsuz tümleştirme.

Bu yeni yetenekler hakkında daha fazla bilgi için bkz. Azure [izleyici tanılama 'daki blog Azure ad etkinlik günlükleri artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) ve belgelerimizde, [Azure izleyici 'de etkinlik günlükleri Azure Active Directory (Önizleme)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Azure AD oturum açma raporuna koşullu erişim bilgileri eklendi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** kimlik güvenliği & koruması

Bu güncelleştirme, bir kullanıcı ilke sonucuyla birlikte oturum açtığında hangi ilkelerin değerlendirildiğini görmenizi sağlar. Ayrıca, rapor artık Kullanıcı tarafından kullanılan istemci uygulaması türünü içerir, böylece eski protokol trafiğini belirleyebilirsiniz. Rapor girdileri Ayrıca, kullanıcıya yönelik hata iletisinde bulunan bir bağıntı KIMLIĞI için aranabilir ve eşleşen oturum açma isteğini tanımlamak ve sorunlarını gidermek için kullanılabilir.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Oturum açma işlemleri etkinlik günlükleri aracılığıyla eski kimlik doğrulamaları görüntüleme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Oturum açma etkinliği günlüklerindeki **Istemci uygulaması** alanının tanıtılmasıyla, müşteriler artık eski kimlik doğrulamaları kullanan kullanıcıları görebilirler. Müşteriler, oturum açma Microsoft Graph API 'sini kullanarak veya Azure AD portalındaki oturum açma etkinlik günlükleri aracılığıyla, eski kimlik doğrulamaları için filtre uygulamak üzere **Istemci uygulaması** denetimini kullanabileceğiniz oturum açma etkinliği günlükleri aracılığıyla bu bilgilere erişebilecektir. Daha fazla bilgi için belgelere göz atın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Temmuz 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Haziran 2018 ' de, uygulama galerisine federasyon desteğiyle bu 16 yeni uygulamayı ekledik:

[Yenilik hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [belirli yönetici SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), Psuc hazırlama, [IPASS SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [ekran kaydı-O-arada](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), powerokul Birleşik sınıf, [eli ekleme](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [bomgar remote support](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 joinnow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [kanbanize](../saas-apps/kanbanize-tutorial.md), [smartlpa](../saas-apps/smartlpa-tutorial.md), [yetenekler tabanı](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Yeni Kullanıcı hazırlama SaaS uygulaması tümleştirmeleri-2018 Temmuz

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği:** üçüncü taraf tümleştirmesi

Azure AD, Dropbox, Salesforce, ServiceNow gibi SaaS uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatik hale getirmenizi sağlar. 2018 Temmuz için, Azure AD uygulama galerisinde aşağıdaki uygulamalar için Kullanıcı hazırlama desteği ekledik:

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Azure AD galerisinde Kullanıcı sağlamayı destekleyen tüm uygulamaların bir listesi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Eşitleme için Connect Health-yalnız bırakılmış ve yinelenen öznitelik eşitleme hatalarını gidermenin daha kolay bir yolu

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** AD Connect **ürün yeteneği:** izleme & raporlama

Azure AD Connect Health, eşitleme hatalarını vurgulamanıza ve düzeltmemize yardımcı olmak için self servis düzeltmesini tanıtır. Bu özellik yinelenen öznitelik eşitleme hatalarını giderir ve Azure AD 'den yalnız bırakılmış nesneleri düzeltir. Bu tanılamada aşağıdaki avantajlar bulunur:

- Yinelenen öznitelik eşitleme hatalarını daraltır ve belirli düzeltmeleri sağlar

- Özel Azure AD senaryoları için bir çözüm uygular, tek bir adımda hataları çözer

- Bu özelliği açmak ve kullanmak için yükseltme veya yapılandırma gerekmez

Daha fazla bilgi için bkz. [yinelenen öznitelik eşitleme hatalarını tanılama ve](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors) Düzeltme

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Azure AD ve MSA oturum açma deneyimlerine yönelik görsel güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Azure AD **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Microsoft 'un Office 365 ve Azure gibi çevrimiçi hizmetler oturum açma deneyimi için Kullanıcı arabirimini güncelleştirdik. Bu değişiklik ekranları daha az karışık ve daha basit hale getirir. Bu değişiklik hakkında daha fazla bilgi için bkz. [Azure AD oturum açma deneyimi bloguna yönelik yakında iyileştirmeler](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) .

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Azure AD Connect yeni sürümü-Temmuz 2018

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

Azure AD Connect en son sürümü şunları içerir:

- Hata düzeltmeleri ve desteklenebilirlik Updates

- Ping-Federate tümleştirmesinin genel kullanılabilirliği

- En son SQL 2012 istemcisine yönelik güncelleştirmeler

Bu güncelleştirme hakkında daha fazla bilgi için bkz [. Azure AD Connect: sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-end-user-ui"></a>Son Kullanıcı Kullanıcı arabirimi kullanım koşullarına göre güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** idare

TOU Son Kullanıcı Kullanıcı arabirimindeki kabul dizesini güncelleştiriyoruz.

**Geçerli metin.** [TenantName] kaynaklarına erişmek için kullanım koşullarını kabul etmeniz gerekir.<br>**Yeni metin.** [TenantName] kaynağına erişmek için kullanım koşullarını okumanız gerekir.

**Geçerli metin:** Kabul etmek için seçilmesi, yukarıdaki kullanım koşullarının tümünü kabul ettiğiniz anlamına gelir.<br>**Yeni metin:** Kullanım koşullarını okuduğunuzu ve anladığınızı onaylamak için lütfen kabul et ' e tıklayın.

---

### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Doğrudan kimlik doğrulaması eski protokolleri ve uygulamaları destekler

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Doğrudan kimlik doğrulaması artık eski protokolleri ve uygulamaları desteklemektedir. Aşağıdaki sınırlamalar artık tam olarak desteklenmektedir:

- Modern kimlik doğrulaması gerekmeden eski Office istemci uygulamalarına, Office 2010 ve Office 2013 Kullanıcı oturum açma işlemleri.

- Yalnızca Office 2010 ' de Exchange karma ortamlarında Takvim paylaşımına ve ücretsiz/meşgul bilgilere erişim.

- Modern kimlik doğrulaması gerekmeden Skype Kurumsal istemci uygulamalarına Kullanıcı oturum açma işlemleri.

- PowerShell sürüm 1,0 'de Kullanıcı oturum açma işlemleri.

- Apple Aygıt Kayıt Programı (Apple DEP), iOS Kurulum Yardımcısı kullanılarak.

---

### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Self servis parola sıfırlama ve Multi-Factor Authentication için yakınsama güvenlik bilgileri yönetimi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** SSPR **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu yeni özellik, kullanıcıların güvenlik bilgilerini (örneğin, telefon numarası, e-posta adresi, mobil uygulama vb.) self servis parola sıfırlama (SSPR) ve Multi-Factor Authentication (MFA) için tek bir deneyimde yönetmesine olanak tanır. Kullanıcılar artık SSPR ve MFA için aynı güvenlik bilgilerini iki farklı deneyimle kaydetmeyecektir. Bu yeni deneyim Ayrıca SSPR ya da MFA 'ya sahip kullanıcılar için de geçerlidir.

Bir kuruluş MFA veya SSPR kaydını zormıyorsa, kullanıcılar kendi güvenlik bilgilerini **uygulamalarım** portalı aracılığıyla kaydedebilir. Buradan, kullanıcılar MFA veya SSPR için etkinleştirilen tüm yöntemleri kaydedebilir.

Bu, isteğe bağlı bir genel önizlemedir. Yöneticiler seçili bir Kullanıcı grubu veya bir Kiracıdaki tüm kullanıcılar için yeni deneyimi (isteniyorsa) açabilir.

---

### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Parolanızı sıfırladıktan sonra kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanın

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** SSPR **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu özellik, Microsoft Authenticator (veya başka bir Authenticator uygulamadan) bir bildirim veya kod kullanarak bir parolayı sıfırlarken yönetici olmayan kimlik doğrulamasını sağlar. Yöneticiler bu Self servis parola sıfırlama yöntemini etkinleştirdikten sonra, aka.ms/mfasetup veya aka.ms/setupsecurityinfo aracılığıyla bir mobil uygulama kaydeden kullanıcılar, parolalarını sıfırlarken kendi mobil uygulamalarını doğrulama yöntemi olarak kullanabilir.

Mobil uygulama bildirimi, yalnızca parolanızı sıfırlamak için iki yöntem gerektiren bir ilkenin parçası olarak açılabilir.

---

## <a name="june-2018"></a>Haziran 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Değişiklik bildirimi: Azure AD etkinlik günlükleri API 'sini kullanarak uygulamalar için yetkilendirilmiş yetkilendirme akışına güvenlik düzeltme

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** raporlama **ürün yeteneği:** izleme & raporlama

Daha güçlü güvenlik uygulamamız nedeniyle, [Azure ad etkinlik günlükleri API 'lerine](https://aka.ms/aadreportsapi)erişmek için yetkilendirilmiş yetkilendirme akışı kullanan uygulamalar için izinlerde değişiklik yapmamız gerekiyordu. Bu değişiklik **26 haziran 2018 '** de gerçekleşir.

Uygulamalarınızdan herhangi biri Azure AD etkinlik günlüğü API 'Lerini kullanıyorsa, değişikliğin gerçekleşmesinden sonra uygulamanın kesilmesini olmamasını sağlamak için aşağıdaki adımları izleyin.

**Uygulama izinlerinizi güncelleştirmek için**

1. Azure portal oturum açın, **Azure Active Directory**' i seçin ve ardından **uygulama kayıtları**' nı seçin.
2. Azure AD etkinlik günlükleri API 'sini kullanan uygulamanızı seçin, **Ayarlar**' ı seçin, **gerekli izinler**' i seçin ve ardından **Windows Azure Active Directory** API 'sini seçin.
3. **Erişimi etkinleştir** dikey penceresinin **temsilci izinleri** alanında, **Dizin verilerini oku** ' nın yanındaki kutuyu işaretleyin ve ardından **Kaydet**' i seçin.
4. **Izin ver**' i seçin ve ardından **Evet**' i seçin.

    >[!Note]
    >Uygulamaya izin vermek için bir genel yönetici olmanız gerekir.

Daha fazla bilgi için Azure AD Raporlama API 'SI makalesine erişim için önkoşulların [Izin verme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) bölümüne bakın.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>PCI DSS uyumluluğu için Azure AD hizmetlerine bağlanmak üzere TLS ayarlarını yapılandırma

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** yok **ürün yeteneği:** platform

Aktarım Katmanı Güvenliği (TLS), iki iletişim uygulaması arasında gizlilik ve veri bütünlüğü sağlayan bir protokoldür ve günümüzde kullanılan en yaygın olarak dağıtılan güvenlik protokolüdür.

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) , uyumluluk **30 Haziran 2018**' den itibaren, yeni ve daha güvenli uygulama PROTOKOLLERININ etkinleştirilmesi için eski TLS ve Güvenli Yuva Katmanı (SSL) sürümlerinin devre dışı bırakılması gerektiğini belirledi. Bu değişiklik, Azure AD hizmetlerine bağlanıp PCI DSS uyumluluğunu gerektirdiğinde TLS 1,0 ' i devre dışı bırakmanız anlamına gelir. Birden çok TLS sürümü mevcuttur, ancak TLS 1,2 Azure Active Directory Hizmetleri için kullanılabilen en son sürümdür. Hem istemci/sunucu hem de tarayıcı/sunucu birleşimleri için doğrudan TLS 1,2 ' e geçmeyi öneririz.

Güncel olmayan tarayıcılar TLS 1,2 gibi daha yeni TLS sürümlerini desteklemeyebilir. Tarayıcı tarafından hangi TLS sürümlerinin desteklendiğini görmek için [Qualys SSL Labs](https://www.ssllabs.com/) sitesine gidin ve **tarayıcınızı test et**' e tıklayın. Web tarayıcınızın en son sürümüne yükseltmenizi ve tercihen yalnızca TLS 1,2 ' i etkinleştirmenizi öneririz.

**TLS 1,2 'yi tarayıcıda etkinleştirmek için**

- **Microsoft Edge ve Internet Explorer (her ikisi de Internet Explorer kullanılarak ayarlanır)**

    1. Internet Explorer 'ı açın, **Araçlar**  >  **Internet seçenekleri**  >  **Gelişmiş**' i seçin.
    2. **Güvenlik** alanında, **TLS 1,2 kullan**' ı seçin ve ardından **Tamam**' ı seçin.
    3. Tüm tarayıcı pencerelerini kapatın ve Internet Explorer 'ı yeniden başlatın.

- **Google Chrome**

    1. Google Chrome ' ı açın, adres çubuğuna *Chrome://Settings/* yazın ve **ENTER**tuşuna basın.
    2. **Gelişmiş** seçenekleri genişletin, **sistem** alanına gidin ve **proxy ayarlarını aç**' ı seçin.
    3. **Internet özellikleri** kutusunda **Gelişmiş** sekmesini seçin, **güvenlik** alanı ' na gidin, **TLS 1,2 kullan**' ı seçin ve ardından **Tamam**' ı seçin.
    4. Tüm tarayıcı pencerelerini kapatın ve Google Chrome 'u yeniden başlatın.

- **Mozilla Firefox**

    1. Firefox ' u açın, adres çubuğuna *about: config* yazın ve **ENTER**tuşuna basın.
    2. *TLS*terimini arayın ve ardından **Security. TLS. Version. Max** girişini seçin.
    3. Tarayıcıyı TLS 1,2 sürümüne kadar kullanmaya zorlamak için değeri **3** olarak ayarlayın ve ardından **Tamam**' ı seçin.

        >[!NOTE]
        >Firefox sürüm 60,0, TLS 1,3 ' yi desteklediğinden, Security. TLS. Version. Max değerini **4**olarak da ayarlayabilirsiniz.

    4. Tüm tarayıcı pencerelerini kapatın ve Mozilla Firefox 'u yeniden başlatın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Haziran 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Haziran 2018 ' de, uygulama galerisine federasyon desteğiyle bu 15 yeni uygulamayı ekledik:

[Ufuk dokunma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [müzik kapatma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1,1 belirteci etkin lob uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [süper ruh](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [otomatik görev](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [uç nokta yedekleme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ufuk yüksek ağlar](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [Tonicdm](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [SharePoint şirket içi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), bir adet [CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [yükselkx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial) [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD parola koruması genel önizlemede kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Ortamınızdaki kolayca tahmin edilen parolaları ortadan kaldırmaya yardımcı olması için Azure AD parola korumasını kullanın. Bu parolaların ortadan kaldırılması, parola spreyi saldırılarına karşı riskli olma riskini azaltmaya yardımcı olur.

Azure AD parola koruması özellikle şunları yapmanıza yardımcı olur:

- Kuruluşunuzun hesaplarını hem Azure AD hem de Windows Server Active Directory (AD) içinde koruyun.
- Kullanıcılarınızın, en sık kullanılan parolaların 500 ' inden daha fazla bir listede parola kullanmasını ve bu parolaların 1.000.000 karakterlik değiştirme çeşitlemelerini engeller.
- Azure AD parola korumasını Azure AD portalındaki tek bir konumdan, hem Azure AD hem de şirket içi Windows Server AD için yönetin.

Azure AD parola koruması hakkında daha fazla bilgi için bkz. [kuruluşunuzdaki kötü parolaları kaldırma](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında yeni "tüm konuklar" koşullu erişim ilkesi şablonu oluşturuldu

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** idare

Kullanım koşullarınızın oluşturulması sırasında, "tüm konuklar" ve "tüm uygulamalar" için de yeni bir koşullu erişim ilkesi şablonu oluşturulur. Bu yeni ilke şablonu, yeni oluşturulan ToU 'yi uygular ve konuklar için oluşturma ve zorlama sürecini hızlandırma.

Daha fazla bilgi için bkz. [Azure Active Directory kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında yeni "özel" koşullu erişim ilkesi şablonu oluşturuldu

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** idare

Kullanım koşullarınızın oluşturulması sırasında yeni bir "özel" koşullu erişim ilkesi şablonu da oluşturulur. Bu yeni ilke şablonu, ToU 'yi oluşturmanızı ve ardından doğrudan portalda gezinmenize gerek kalmadan koşullu erişim ilkesi oluşturma dikey penceresine gitmenizi sağlar.

Daha fazla bilgi için bkz. [Azure Active Directory kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication dağıtma hakkında yeni ve kapsamlı kılavuz

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** kimlik güvenliği & koruması

Kuruluşunuzda Azure Multi-Factor Authentication (MFA) dağıtma hakkında yeni adım adım yönergeler yayımladık.

MFA dağıtım kılavuzunu görüntülemek için GitHub 'daki [kimlik dağıtım kılavuzlarını](https://aka.ms/DeploymentPlans) depoya gidin. Dağıtım kılavuzlarıyla ilgili geri bildirim sağlamak için [dağıtım planı geri bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzlarıyla ilgili sorularınız varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bizimle iletişime geçin.

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD tarafından atanan uygulama yönetimi rolleri genel önizlemede

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** Access Control

Yöneticiler artık genel yönetici rolünü atamadan uygulama yönetim görevleri için temsilci verebilir. Yeni roller ve yetenekler şunlardır:

- **Yeni standart Azure AD yönetici rolleri:**

    - **Uygulama Yöneticisi.** Kayıt, SSO ayarları, uygulama atamaları ve lisanslama, uygulama ara sunucusu ayarları ve onay (Azure AD kaynakları hariç) dahil olmak üzere tüm uygulamaların tüm yönlerini yönetme olanağı verir.

    - **Bulut uygulaması Yöneticisi.** Uygulama proxy 'si dışında, şirket içi erişim sağlamadığı için tüm uygulama yöneticisi yeteneklerini verir.

    - **Uygulama geliştiricisi.** **Kullanıcıların uygulama kaydetmesine izin ver** seçeneği devre dışı olsa bile, uygulama kayıtları oluşturma özelliğini verir.

- **Sahiplik (Grup sahipliği işlemine benzer şekilde, uygulama başına kayıt ve kurumsal başına uygulama ayarlayın:**

    - **Uygulama kayıt sahibi.** Uygulama bildirimi ve ek sahipler ekleme dahil olmak üzere sahip olan uygulama kaydı 'nın tüm yönlerini yönetme olanağı verir.

    - **Kurumsal uygulama sahibi.** SSO ayarları, uygulama atamaları ve onay (Azure AD kaynakları hariç) dahil olmak üzere sahip olunan kurumsal uygulamaların birçok yönlerini yönetme olanağı verir.

Genel önizleme hakkında daha fazla bilgi için bkz. [Azure AD temsilci uygulama yönetimi rolleri genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) . lenemeyen. Roller ve izinler hakkında daha fazla bilgi için bkz. [Azure Active Directory yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Mayıs 2018

### <a name="expressroute-support-changes"></a>ExpressRoute destek değişiklikleri

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** kimlik doğrulamaları (oturum açma) **ürün yeteneği:** platform

Azure Active Directory (Azure AD) gibi bir hizmet sunumu, ExpressRoute veya diğer özel VPN tünellerine gerek duymadan doğrudan Internet üzerinden çalışacak şekilde tasarlanmıştır. Bu nedenle, **1 ağustos 2018**' de Azure genel eşleme ve Microsoft eşleme 'de Azure toplulukları KULLANıLARAK Azure AD Hizmetleri Için ExpressRoute desteğini durduracağız. Bu değişiklikten etkilenen tüm hizmetler, Azure AD trafiğinin yavaş şekilde ExpressRoute 'a Internet 'e kaydırdığına bildirimde bulunabilir.

Desteğimizi değiştirirken, kimlik doğrulama trafiğiniz için adanmış bir devre kümesi kullanmanız gerekebilecek durumlar olduğunu da biliyoruz. Bu nedenle, Azure AD "diğer Office 365 Online Services" topluluğuyla Microsoft eşliğinde zaten ExpressRoute ve hizmetleri kullanan kiracı başına IP aralığı kısıtlamalarını desteklemeye devam edecektir. Hizmetleriniz etkilenirse, ancak ExpressRoute gerekliyse, aşağıdakileri yapmanız gerekir:

- **Azure genel eşleme kullanıyorsanız.** Microsoft eşlemesi ' ne gidin ve **diğer Office 365 Online Services (12076:5100)** topluluğuna kaydolun. Azure genel eşlemeden Microsoft eşlemesine nasıl geçeceğimiz hakkında daha fazla bilgi için bkz. [genel eşlemeyi Microsoft eşlemesi 'Ne taşıma](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) makalesi.

- **Microsoft eşleme kullanıyorsanız.** **Diğer Office 365 çevrimiçi hizmet (12076:5100)** topluluğu için kaydolun. Yönlendirme gereksinimleri hakkında daha fazla bilgi için, ExpressRoute yönlendirme gereksinimleri makalesinin [BGP toplulukları Için destek bölümüne](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) bakın.

Adanmış devreleri kullanmaya devam etmeniz gerekiyorsa, **diğer Office 365 çevrimiçi hizmet (12076:5100)** Community 'yi kullanma yetkisi alma hakkında Microsoft hesabı ekibinizle konuşmanız gerekir. MS Office tarafından yönetilen gözden geçirme panosu, bu devrelere ihtiyacınız olup olmadığını doğrular ve bunları tutmanın teknik etkilerini anladığınızdan emin olmanızı sağlar. Office 365 için yol filtreleri oluşturmaya çalışan yetkisiz abonelikler, bir hata iletisi alır.

---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>TOU için yönetim senaryoları için Microsoft Graph API 'Leri

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** geliştirici deneyimi

Azure AD kullanım koşulları 'nın yönetim işlemi için Microsoft Graph API 'Leri ekledik. Kullanım koşulları nesnesi oluşturabilir, güncelleştirebilir, silebilirsiniz.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Azure AD B2C bir kimlik sağlayıcısı olarak Azure AD çok kiracılı uç noktası ekleme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Özel ilkeleri kullanarak artık Azure AD ortak uç noktasını Azure AD B2C bir kimlik sağlayıcısı olarak ekleyebilirsiniz. Bu, uygulamalarınızda oturum açan tüm Azure AD kullanıcıları için tek bir giriş noktasına sahip etmenize olanak tanır. Daha fazla bilgi için bkz. [Azure Active Directory B2C: kullanıcıların özel ilkeleri kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısında oturum açmalarına Izin verin](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Uygulamalarım oturum açma uzantımız ve Azure AD Uygulama Ara Sunucusu her yerden uygulamalara erişmek için Iç URL 'Leri kullanın

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulamalarım **ürün yeteneği:** SSO

Kullanıcılar, Azure AD için uygulamalarım güvenli oturum açma uzantısını kullanarak kurumsal ağınızın dışında bile uygulamalara iç URL 'Ler aracılığıyla erişebilir. Bu, aynı zamanda erişim paneli tarayıcı uzantısının yüklü olduğu herhangi bir tarayıcıda Azure AD Uygulama Ara Sunucusu kullanarak yayımladığınız herhangi bir uygulamayla birlikte çalışacaktır. Kullanıcı uzantıya oturum açtıktan sonra URL yeniden yönlendirme işlevi otomatik olarak etkinleştirilir. Uzantı [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)ve [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)'ta indirilebilir.

---

### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory-Avrupa müşterileri için Avrupa 'daki veriler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** altın ocal

Avrupa 'daki müşteriler, verilerin Avrupa 'da kalmasını ve müşterilerin gizlilik ve Avrupa yasaları için Avrupa veri merkezleri dışında çoğaltılmaması gerekir. Bu [makalede](https://go.microsoft.com/fwlink/?linkid=872328) , Avrupa 'da hangi kimlik bilgilerinin depolanacağı ve ayrıca Avrupa veri merkezleri dışında depolanacak bilgiler hakkındaki ayrıntıları sağlayan belirli ayrıntılar sağlanmaktadır.

---

### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Yeni Kullanıcı hazırlama SaaS uygulaması tümleştirmeleri-Mayıs 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği:** üçüncü taraf tümleştirmesi

Azure AD, Dropbox, Salesforce, ServiceNow gibi SaaS uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatik hale getirmenizi sağlar. Mayıs 2018 için, Azure AD uygulama galerisinde aşağıdaki uygulamalar için Kullanıcı hazırlama desteği ekledik:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Cornerstone OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Azure AD galerisinde Kullanıcı sağlamayı destekleyen tüm uygulamaların listesi için bkz [https://aka.ms/appstutorial](https://aka.ms/appstutorial) ..

---

### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Grupların ve uygulama erişiminin Azure AD erişim gözden geçirmeleri artık yinelenen incelemeler sağlıyor

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** erişim gözden geçirmeleri **ürün yeteneği:** idare

Grupların ve uygulamaların erişim incelemesi artık Azure AD Premium P2 bir parçası olarak genel kullanıma sunulmuştur.  Yöneticiler, aylık veya üç aylık gibi düzenli aralıklarla otomatik olarak yinelenecek grup üyelikleri ve uygulama atamalarının erişim incelemelerini yapılandırabilecektir.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD etkinlik günlükleri (oturum açma ve denetim) artık MS Graph aracılığıyla kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Oturum açma işlemlerini ve denetim günlüklerini içeren Azure AD etkinlik günlükleri artık Microsoft Graph API aracılığıyla kullanılabilir. Bu günlüklere erişmek için Microsoft Graph API 'SI aracılığıyla iki uç noktası kullanıma sunduk. Başlamak için Azure AD Raporlama API 'Lerine programlı erişim için [belgelerimizi](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) inceleyin.

---

### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>B2B kullanım deneyimine yönelik geliştirmeler ve bir kuruluş bırakma

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2B **ürün yeteneği:** B2B/B2C

**Tam zamanında kullanım:** B2B API 'sini kullanarak bir kaynağı Konuk kullanıcıyla paylaştığınızda, özel bir davet e-postası göndermeniz gerekmez. Çoğu durumda, Konuk Kullanıcı kaynağa erişebilir ve tam zamanında kullanım deneyiminden alınacaktır. Eksik e-postalar nedeniyle daha fazla etki yok. Konuk kullanıcılarınızı "Bu kullanım bağlantısına tıkladı", sistem sizi gönderdi. Bu, SPO davet yöneticisini kullandığında-bulutlu ekleri tüm kullanıcılar için aynı kurallı URL 'yi (iç ve dış), herhangi bir kullanım durumunda sahip olabileceği anlamına gelir.

**Modern kullanım deneyimi:** Daha fazla bölünmüş ekran ödeme giriş sayfası yok. Kullanıcılar, üçüncü taraf uygulamalar için olduğu gibi, kuruluşun gizlilik bildirimiyle ilgili modern bir onay deneyimi görür.

**Konuk kullanıcılar kuruluş 'dan bırakabilir:** Bir kullanıcının bir org ile ilişkisi bittikten sonra, kuruluştan ayrılmaları için kendi kendine servis sağlayabilirsiniz. Kuruluşun yöneticisinin "kaldırılması" için davet eden başka hiçbir çağrı yok, daha fazla destek bileti yok.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mayıs 2018

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Mayıs 2018 ' de, uygulama galerimize federasyon desteği olan bu 18 yeni uygulamayı ekledik:

[Awardspring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), ınfogix Data3Sixty yönetme [, yodestesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [JAMF Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [knowledgeowl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi Mmis](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [launchkaranlık](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Capze ana](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [monsat çevrimiçi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial),[まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [yay yayımlama-SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [plangrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [ıwellneskar](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Gözden](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---

### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Azure Active Directory için yeni adım adım dağıtım kılavuzu

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** Dizin

Self servis parola sıfırlama (SSPR), çoklu oturum açma (SSO), koşullu erişim (CA), uygulama proxy 'si, Kullanıcı sağlama, Active Directory Federasyon Hizmetleri (AD FS) (ADFS) ve parola karması eşitleme 'ye (PHS) dahil olmak üzere Azure Active Directory (Azure AD) dağıtma hakkında yeni, adım adım yönergeler.

Dağıtım kılavuzlarını görüntülemek için GitHub 'daki [kimlik dağıtım kılavuzlarını](https://aka.ms/DeploymentPlans) depoya gidin. Dağıtım kılavuzlarıyla ilgili geri bildirim sağlamak için [dağıtım planı geri bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzlarıyla ilgili sorularınız varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bizimle iletişime geçin.

---

### <a name="enterprise-applications-search---load-more-apps"></a>Kurumsal uygulamalar arama-daha fazla uygulama yükleme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

Uygulamalarınızın/hizmet sorumlularını bulmada sorun mu yaşıyorsunuz? Kurumsal uygulamalarınızın tüm uygulamalar listesine daha fazla uygulama yükleme özelliği ekledik. Varsayılan olarak 20 uygulama gösterilmektedir. Artık, ek uygulamaları görüntülemek için **daha fazla yükle** ' ye tıklayabilirsiniz.

---

### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>AADConnect 'in Mayıs yayını, PingFederate ile tümleştirmenin genel önizlemesini, önemli güvenlik güncelleştirmelerini, birçok hata düzeltmesi ve yeni harika yeni sorun giderme araçlarını içerir.

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** AD Connect **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

AADConnect 'in Mayıs yayını, PingFederate ile tümleştirmenin genel önizlemesini, önemli güvenlik güncelleştirmelerini, birçok hata düzeltmesi ve yeni harika yeni sorun giderme araçlarını içerir. Sürüm notlarını [buradan](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190)bulabilirsiniz.

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD erişim gözden geçirmeleri: otomatik uygulama

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** erişim gözden geçirmeleri **ürün yeteneği:** idare

Grupların ve uygulamaların erişim gözden geçirmeleri artık Azure AD Premium P2 bir parçası olarak genel kullanıma sunulmuştur. Yönetici, gözden geçirenin değişikliklerini bu grup veya uygulamaya otomatik olarak uygulamak için erişim gözden geçirmesi tamamlanana kadar yapılandırabilir. Yönetici ayrıca, gözden geçirenler yanıt vermezse, erişimi kaldırmadıysa, erişimi devam ettirdiğinde veya sistem önerilerini almak için kullanıcının devam erişiminin ne olacağını da belirtebilir.

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-response_mode-for-new-apps"></a>KIMLIK belirteçleri artık yeni uygulamalar için sorgu response_mode kullanılarak döndürülemez.

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

25 Nisan 2018 ' de veya sonrasında oluşturulan uygulamalar artık **sorgu** response_mode kullanarak **id_token** isteyemeyecektir.  Bu, Azure AD 'yi OıDC belirtimleriyle satır içi olarak getirir ve uygulamalarınızın saldırı yüzeyini azaltmaya yardımcı olur.  25 Nisan 2018 tarihinden önce oluşturulan uygulamalar, **id_token**response_type **sorgu** response_mode kullanılmasını engellenmemiş.  AAD 'den bir id_token istenirken döndürülen hata, **AADSTS70007: ' Query ', belirteç istenirken ' response_mode ' için desteklenen bir değer değildir**.

**Parça** ve **form_post** response_modes çalışmaya devam eder-yeni uygulama nesneleri oluştururken (örneğin, uygulama proxy 'si kullanımı için), yeni bir uygulama oluşturmadan önce bu response_modes birini kullandığınızdan emin olun.

---

## <a name="april-2018"></a>Nisan 2018

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C erişim belirteci GA

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Artık erişim belirteçlerini kullanarak Azure AD B2C tarafından güvenliği sağlanmış Web API 'Lerine erişebilirsiniz. Bu özellik genel önizlemeden GA 'ye taşınıyor. Azure AD B2C uygulamaları ve Web API 'Lerini yapılandırmak için Kullanıcı arabirimi deneyimi geliştirilmiştir ve diğer küçük geliştirmeler yapılmıştır.

Daha fazla bilgi için bkz. [Azure AD B2C: erişim belirteçleri isteme](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>SAML tabanlı uygulamalar için çoklu oturum açma yapılandırmasını test etme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

SAML tabanlı SSO uygulamalarını yapılandırırken yapılandırma sayfasında tümleştirmeyi test edebilirsiniz. Oturum açma sırasında bir hatayla karşılaşırsanız, testi deneymenizde hata verebilir ve Azure AD, belirli sorunu çözmek için çözüm adımları sağlar.

Daha fazla bilgi için bkz.

- [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Azure Active Directory içindeki uygulamalarda SAML tabanlı çoklu oturum açma hatalarını ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---

### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD kullanım koşulları artık Kullanıcı başına raporlamaya sahiptir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** uyumluluk

Yöneticiler artık belirli bir ToU seçebilir ve bu ToU 'ya onaylanan tüm kullanıcıları ve ne zaman/saat sürdüğünü görebilir.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: AD FS Extranet kilitleme koruması için riskli IP

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** izleme & raporlama

Connect Health artık, saat başına veya günlük olarak başarısız U/P oturum açma eşiklerini aşan IP adreslerini algılama özelliğini desteklemektedir. Bu özellik tarafından sunulan yetenekler şunlardır:

- Özelleştirilebilir eşikle, IP adresini ve saatlik/günlük temelinde oluşturulan başarısız oturum açma sayısını gösteren kapsamlı rapor.
- Belirli bir IP adresinin saat/gün başına, başarısız U/P oturum açma eşiğini ne zaman aşmadığını gösteren e-posta tabanlı uyarılar.
- Verilerin ayrıntılı analizini yapmak için bir indirme seçeneği

Daha fazla bilgi için bkz. [RISKLI IP raporu](https://aka.ms/aadchriskyip).

---

### <a name="easy-app-config-with-metadata-file-or-url"></a>Meta veri dosyası veya URL 'SI olan kolay uygulama yapılandırması

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

Kurumsal uygulamalar sayfasında, Yöneticiler AAD Galerisi ve Galeri dışı uygulama için SAML tabanlı oturum açmayı yapılandırmak üzere bir SAML meta veri dosyasını karşıya yükleyebilir.

Ayrıca, hedef uygulamayla SSO 'yu yapılandırmak için Azure AD uygulama Federasyon meta verileri URL 'sini de kullanabilirsiniz.

Daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde olmayan uygulamalarda çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)konusuna bakın.

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD Kullanım koşulları genel kullanıma sunuldu

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** uyumluluk


Azure AD kullanım koşulları, genel önizlemeden genel kullanıma açık olarak taşınmıştır.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Belirli kuruluşlardan B2B kullanıcılarına gönderilen davetlere izin verme veya engelleme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2B **ürün yeteneği:** B2B/B2C


Artık Azure AD B2B Işbirliğinde paylaşmak ve işbirliği yapmak istediğiniz iş ortağı kuruluşlarını belirtebilirsiniz. Bunu yapmak için, belirli izin verme veya reddetme etki alanlarının listesini oluşturmayı seçebilirsiniz. Bu özellikleri kullanarak bir etki alanı engellendiğinde, çalışanlar artık bu etki alanındaki kişilere davetiye gönderemezler.

Bu, onaylanan kullanıcılar için sorunsuz bir deneyim etkinleştirirken kaynaklarınıza erişimi denetlemenize yardımcı olur.

Bu B2B Işbirliği özelliği tüm Azure Active Directory müşterileri tarafından kullanılabilir ve dış iş kullanıcılarının oturum açma ve erişim kazanmasıyla ilgili daha ayrıntılı denetim sağlamak için koşullu erişim ve kimlik koruması gibi Azure AD Premium özelliklerle birlikte kullanılabilir.

Daha fazla bilgi için, bkz. [belirli kuruluşlardan B2B kullanıcılarına Izin verme veya bu kullanıcıların davetlerini engelleme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

2018 Nisan 'da, uygulama galerimize federasyon desteği olan bu 13 yeni uygulamayı ekledik:

Ölçüt HCM, [Fcalnote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (on-premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dinamik sinyal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindwireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [kuruluş şeması artık](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [appneta performans izleyicisi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [floxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), rafı, [SafetyNET](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---

### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Azure AD 'de B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2B **ürün yeteneği:** B2B/B2C

Konuk kullanıcıları iş ortağı kuruluşlarınızdan Azure AD 'ye davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş olarak, bu B2B kullanıcılarına şirket içi uygulamalara erişimi sağlayabilirsiniz. Bu şirket içi uygulamalar, SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (ıWA) kullanarak Kerberos kısıtlanmış temsili (KCD) kullanabilir.

Daha fazla bilgi için bkz. Şirket [içi uygulamalarınıza Azure AD erişimi IÇIN B2B kullanıcıları verme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---

### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Azure Marketi 'nden SSO tümleştirme öğreticileri alın

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** diğer **ürün yeteneği:** üçüncü taraf tümleştirmesi

[Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) 'nde listelenen BIR uygulama SAML tabanlı çoklu oturum açmayı destekliyorsa, **Şimdi al** ' a tıklamak bu uygulamayla ilişkili tümleştirme öğreticisini sağlar.

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>SaaS uygulamalarına Azure AD otomatik Kullanıcı sağlamasının daha hızlı performansı

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği: 3.** taraf tümleştirmesi

Daha önce, SaaS uygulamaları için Azure Active Directory Kullanıcı sağlama bağlayıcılarını kullanan müşteriler (örneğin, Salesforce, ServiceNow ve Box), Azure AD kiracılar 100.000 Birleşik Kullanıcı ve grup üzerinde yer alıyorsa ve hangi kullanıcıların sağlanması gerektiğini belirlemekte Kullanıcı ve Grup atamaları kullanıyorsa, yavaş performansla karşılaşabilir.

2 Nisan 2018 ' de, Azure AD sağlama hizmeti 'ne Azure Active Directory ve hedef SaaS uygulamaları arasında ilk eşitlemeleri gerçekleştirmek için gereken süreyi büyük ölçüde azaltan önemli performans geliştirmeleri dağıtıldı.

Sonuç olarak, çok sayıda gün veya hiç tamamlanmamış uygulamalar için ilk eşitlemeleri olan birçok müşteri artık birkaç dakika veya saat içinde tamamlanır.

Daha fazla bilgi için bkz. [sağlama sırasında ne olur?](/azure//active-directory/app-provisioning/how-provisioning-works)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Karma Azure AD 'ye katılmış makineler için Windows 10 kilit ekranından self servis parola sıfırlama

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** self servis parola sıfırlama **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Windows 10 SSPR özelliğini, karma Azure AD 'ye katılmış makineler için destek içerecek şekilde güncelleştirdik. Bu özellik Windows 10 ' da kullanılabilir RS4, kullanıcıların parolalarını bir Windows 10 makinesinin kilit ekranından sıfırlamasına izin verir. Self servis parola sıfırlama için etkin ve kayıtlı kullanıcılar bu özelliği kullanabilir.

Daha fazla bilgi için bkz. [oturum açma ekranından Azure AD parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Mart 2018

### <a name="certificate-expire-notification"></a>Sertifika süre sonu bildirimi

**Şunu yazın:** Sabit **hizmet kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

Bir galeri veya Galeri dışı bir uygulama için bir sertifika sona ermek üzereyken Azure AD bir bildirim gönderir.

Bazı kullanıcılar SAML tabanlı çoklu oturum açma için yapılandırılmış kurumsal uygulamalar için bildirim almadı. Bu sorun çözüldü. Azure AD, 7, 30 ve 60 gün içinde süresi dolan sertifikaların bildirimini gönderir. Bu olayı denetim günlüklerinde görebilirsiniz.

Daha fazla bilgi için bkz.

- [Azure Active Directory 'da federe çoklu oturum açma için sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory portalındaki denetim etkinliği raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)

---

### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C Twitter ve GitHub kimlik sağlayıcıları

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** B2C-tüketici kimlik yönetimi **ürün yeteneği:** B2B/B2C

Artık, Azure AD B2C bir kimlik sağlayıcısı olarak Twitter veya GitHub ekleyebilirsiniz. Twitter, genel önizlemeden GA 'ye taşınıyor. GitHub, genel önizlemede kullanıma sunuldu.

Daha fazla bilgi için bkz. [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>İOS ve Android için Azure AD uygulama tabanlı koşullu erişim ile Intune Managed Browser kullanarak tarayıcı erişimini kısıtlama

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği & koruması

**Şimdi genel önizlemede!**

**INTUNE Managed Browser SSO:** Çalışanlarınız yerel istemcilerde (Microsoft Outlook gibi) çoklu oturum açma ve tüm Azure AD ile bağlantılı uygulamalar için Intune Managed Browser kullanabilir.

**Intune Managed Browser koşullu erişim desteği:** Artık çalışanların, uygulama tabanlı koşullu erişim ilkeleri kullanarak Intune Managed Browser 'ı kullanmasını zorunlu kılabilirsiniz.

[Blog gönderimizde](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/)bunun hakkında daha fazla bilgi edinin.

Daha fazla bilgi için bkz.

- [Uygulama tabanlı koşullu erişim kurulumu](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Yönetilen tarayıcı ilkelerini yapılandırma](https://aka.ms/managedbrowser)

---

### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>PowerShell GA modülünde uygulama proxy cmdlet 'Leri

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** Access Control

Uygulama proxy cmdlet 'leri için destek artık PowerShell GA modülünde! Bu, PowerShell modüllerinde güncelleşmenizi gerektirir; arkasında bir yıldan daha fazla olduğunuzda bazı cmdlet 'ler çalışmayı durdurabilir.

Daha fazla bilgi için bkz. [Azuread](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).

---

### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 yerel istemcileri etkileşimli olmayan bir protokol kullanılarak sorunsuz SSO tarafından desteklenir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Office 365 Native istemcileri kullanan kullanıcı (sürüm 16.0.8730. xxxx ve üzeri) sorunsuz SSO kullanarak sessiz bir oturum açma deneyimi alın. Bu destek, Azure AD 'ye etkileşimli olmayan bir protokol (WS-Trust) eklenerek sağlanır.

Daha fazla bilgi için bkz. [sorunsuz SSO ile yerel bir istemcide oturum açma nasıl çalışır?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)

---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Uygulamalar, Azure AD 'nin kiracı uç noktalarına oturum açma istekleri gönderirse sorunsuz SSO ile sessiz oturum açma deneyimi alırlar.

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Kullanıcılar, sorunsuz SSO ile sessiz bir oturum açma deneyimi alırlar. bir uygulama (örneğin, `https://contoso.sharepoint.com` ) Azure AD 'nin kiracı uç noktalarına oturum açma istekleri gönderirse (örneğin, `https://login.microsoftonline.com/contoso.com/<..>` `https://login.microsoftonline.com/<tenant_ID>/<..>` Azure AD 'nin ortak uç noktası () yerine) `https://login.microsoftonline.com/common/<...>` .

Daha fazla bilgi için bkz. [kesintisiz çoklu oturum açma Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso).

---

### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Sorunsuz SSO 'yu kullanıma almak için kullanıcıların Intranet bölgesi ayarlarına daha önce iki URL yerine yalnızca bir Azure AD URL 'SI eklemeniz gerekir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Kullanıcılarınıza sorunsuz SSO eklemek için, Active Directory ' de Grup İlkesi kullanarak kullanıcıların Intranet bölgesi ayarlarına yalnızca bir Azure AD URL 'SI eklemeniz gerekir: `https://autologon.microsoftazuread-sso.com` . Daha önce müşterilerin iki URL eklemesi gerekiyordu.

Daha fazla bilgi için bkz. [kesintisiz çoklu oturum açma Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Bu 15 Mart 2018 ' de, uygulama galerimize federasyon desteği olan bu 15 yeni uygulamayı ekledik:

[BoxCryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [cylanceprotect](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [Signalfx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Asstagenda, [yardione](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), vtiger CRM, ınkırpması, [genlik](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [Contractçalışma,](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial) [bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [trısotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---

### <a name="pim-for-azure-resources-is-generally-available"></a>Azure kaynakları için PıM genel kullanıma sunuldu

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Privileged Identity Management **ürün özelliği:** Privileged Identity Management

Dizin rolleri için Azure AD Privileged Identity Management kullanıyorsanız, artık, abonelik, kaynak grupları, sanal makineler ve Azure Resource Manager tarafından desteklenen diğer kaynaklar gibi Azure Kaynak rolleri için PıM 'nin saat bağlantılı erişim ve atama yeteneklerini kullanabilirsiniz. Tek seferde rolleri etkinleştirirken Multi-Factor Authentication uygulayın ve etkinleştirmeleri, onaylanan değişiklik pencereleri ile koordinasyon olarak zamanlayın. Buna ek olarak, bu sürüm, güncelleştirilmiş bir kullanıcı arabirimi, onay iş akışları ve rolleri yakında süresi dolacak şekilde genişletme ve süresi dolan rolleri yenileme özelliği dahil olmak üzere genel önizleme sırasında kullanılamayan geliştirmeler ekler.

Daha fazla bilgi için bkz. [Azure kaynakları Için PIM (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)

---

### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Uygulama belirteçlerinize Isteğe bağlı talepler ekleme (Genel Önizleme)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD uygulamanız artık JWTs veya SAML belirteçlerinde özel veya isteğe bağlı talepler talep edebilir.  Bunlar, boyut veya uygulanabilirlik kısıtlamaları nedeniyle, belirteçte varsayılan olarak bulunmayan Kullanıcı veya kiracı hakkında taleplerdir.  Bu, şu anda v 1.0 ve v 2.0 uç noktalarında Azure AD uygulamaları için genel önizlemededir.  Hangi taleplerin eklenebileceklerini ve bunları istemek için uygulama bildiriminizi nasıl düzenleyeceğini öğrenmek için belgelere bakın.

Daha fazla bilgi için bkz. [Azure AD 'de Isteğe bağlı talepler](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).

---

### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD, daha güvenli OAuth akışları için PKI CE 'yi destekler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD docs, OAuth 2,0 yetkilendirme kodu verme akışı sırasında daha güvenli iletişim sağlayan PCE için destek olarak güncelleştirilmiştir.  V 1.0 ve v 2.0 uç noktalarında hem S256 hem de düz metin code_challenges desteklenir.

Daha fazla bilgi için bkz. [yetkilendirme kodu isteme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code).

---

### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-get_workers-api"></a>Workday Get_Workers API 'sinde bulunan tüm kullanıcı özniteliği değerlerini sağlama desteği

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama sağlama **ürün yeteneği:** üçüncü taraf tümleştirmesi

Workday 'den Active Directory ve Azure AD 'ye gelen kaynak sağlamanın genel önizlemesi artık Workday Get_Workers API 'sinde bulunan tüm öznitelik değerlerini ayıklama ve sağlama özelliğini desteklemektedir. Bu, iş günü gelen sağlama bağlayıcısının ilk sürümü ile sevk edilen yüzlerce ek standart ve özel öznitelik için destek sağlar.

Daha fazla bilgi için bkz. [Workday kullanıcı özniteliklerinin listesini özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Grup üyeliğini dinamikten statik olarak değiştirme ve tam tersi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Grup Yönetimi **ürün yeteneği:** işbirliği

Bir grupta üyeliğin yönetilme şeklini değiştirmek mümkündür. Bu, sistemde aynı grup adını ve KIMLIĞINI tutmak istediğinizde yararlıdır, bu nedenle gruba yönelik varolan başvurular hala geçerlidir; Yeni bir grup oluşturmak bu başvuruların güncelleştirilmesini gerektirir.
Azure AD Yönetim merkezini bu işlevselliği destekleyecek şekilde güncelleştirdik. Artık müşteriler, mevcut grupları dinamik üyeliğinden atanan üyeliğe veya tam tersi yönde dönüştürebilir. Mevcut PowerShell cmdlet 'leri yine de kullanılabilir.

Daha fazla bilgi için bkz. [Azure Active Directory gruplar Için dinamik üyelik kuralları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Sorunsuz SSO ile iyileştirilmiş oturum kapatma davranışı

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Daha önce, kullanıcılar Azure AD tarafından güvenli hale getirilmiş bir uygulamanın oturumunu kapatsa bile, bir Azure AD uygulamasına, etki alanına katılmış cihazlarından Corpnet dahilinde yeniden erişmeye çalıştıkları durumlarda sorunsuz SSO kullanılarak otomatik olarak oturum açabilirler. Bu değişiklik ile oturumu kapatma desteklenir.  Böylece kullanıcılar, sorunsuz SSO kullanılarak otomatik olarak oturum açmak yerine, kullanıcıların ile yeniden oturum açmasını sağlamak için aynı veya farklı Azure AD hesabını seçmesine olanak sağlar.

Daha fazla bilgi için bkz. [Azure Active Directory kesintisiz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)

---

### <a name="application-proxy-connector-version-154020-released"></a>Uygulama proxy Bağlayıcısı Sürüm 1.5.402.0 yayınlandı

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** kimlik güvenliği & koruması

Bu bağlayıcı sürümü, Kasım ile kademeli olarak dışarı aktarılır. Bu yeni bağlayıcı sürümü aşağıdaki değişiklikleri içerir:

- Bağlayıcı artık alt etki alanı düzeyi yerine etki alanı düzeyi tanımlama bilgilerini ayarlıyor. Bu, daha yumuşak bir SSO deneyimi sağlar ve gereksiz kimlik doğrulama istemlerini önler.
- Yığın halinde kodlama istekleri desteği
- Geliştirilmiş bağlayıcı sistem durumu izleme
- Çeşitli hata düzeltmeleri ve kararlılık geliştirmeleri

Daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu bağlayıcıları anlama](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

---

## <a name="february-2018"></a>Şubat 2018

### <a name="improved-navigation-for-managing-users-and-groups"></a>Kullanıcıları ve grupları yönetmek için geliştirilmiş gezinti

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** Dizin Yönetimi **ürün yeteneği:** Dizin

Kullanıcıları ve grupları yönetmeye yönelik gezinti deneyimi kolaylaştırılmıştır. Artık, silinen kullanıcılar listesine daha kolay erişim sağlamak için doğrudan dizine genel bakış ' ı tüm kullanıcılar listesine gidebilirsiniz. Ayrıca, Grup yönetimi ayarlarına daha kolay erişim sayesinde dizine genel bakış ' a doğrudan tüm grupların listesine gidebilirsiniz. Ayrıca, dizine genel bakış sayfasından bir Kullanıcı, Grup, kurumsal uygulama veya uygulama kaydı için arama yapabilirsiniz.

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>21Vianet tarafından çalıştırılan Microsoft Azure oturum açma ve denetim raporlarının kullanılabilirliği (Azure Çin 21Vianet)

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure Stack **ürün yeteneği:** izleme & raporlama

Azure AD etkinlik günlüğü raporları artık 21Vianet (Azure Çin 21Vianet) örnekleri tarafından işletilen Microsoft Azure sunulmaktadır. Aşağıdaki günlükler dahil edilmiştir:

- **Oturum açma etkinliği günlükleri** -kiracınızla ilişkili tüm oturum açma günlüklerini içerir.

- **Self servis parola denetim günlükleri** -tüm SSPR denetim günlüklerini içerir.

- **Dizin Yönetimi denetim günlükleri** -Kullanıcı yönetimi, uygulama yönetimi ve diğerleri gibi dizin yönetimiyle ilgili tüm denetim günlüklerini içerir.

Bu günlüklerle ortamınızın nasıl yaptığına ilişkin Öngörüler elde edebilirsiniz. Sağlanan verilerle:

- Uygulamalarınızın ve hizmetlerinizin kullanıcılarınız tarafından nasıl kullanıldığını saptayın.

- Kullanıcılarınızın işlerini halledmesini önlemeye yönelik sorunları giderin.

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Azure AD etkinlik raporlarını görüntülemek için "rapor okuyucu" rolünü (yönetici olmayan rol) kullanın

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** raporlama **ürün yeteneği:** izleme & raporlama

Yönetici olmayan rollerin Azure AD etkinlik günlüklerine erişimine sahip olmasını sağlamak için müşterilerin geri bildirimlerinden bir parçası olarak, "rapor okuyucu" rolünde olan kullanıcıların, Azure portal içinde oturum açma ve denetim etkinliğine ve Microsoft Graph API 'sini kullanmaya erişme yeteneğini etkinleştirdik.

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Kullanıcı özniteliği ve Kullanıcı tanımlayıcısı olarak kullanılabilir EmployeeID talebi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** SSO

**ÇalışanNo** 'ı, kurumsal uygulama kullanıcı arabiriminden SAML tabanlı oturum açma uygulamalarında üye KULLANıCıLAR ve B2B konukları için Kullanıcı tanımlayıcısı ve kullanıcı özniteliği olarak yapılandırabilirsiniz.

Daha fazla bilgi için bkz. [Azure Active Directory 'de kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu joker karakter kullanarak Basitleştirilmiş uygulama yönetimi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Uygulama dağıtımını daha kolay hale getirmek ve yönetim yükünüzü azaltmak için artık joker karakterler kullanarak uygulama yayımlama özelliğini destekliyoruz. Bir joker uygulama yayımlamak için standart uygulama yayımlama akışını takip edebilir, ancak iç ve dış URL 'lerde bir joker karakter kullanabilirsiniz.

Daha fazla bilgi için [Azure Active Directory uygulama proxy 'Sinde joker uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard) bölümüne bakın

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Uygulama proxy 'sinin yapılandırmasını desteklemeye yönelik yeni cmdlet 'ler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** platform

AzureAD PowerShell önizleme modülünün en son sürümü, müşterilerin PowerShell kullanarak uygulama proxy uygulamalarını yapılandırmasına izin veren yeni cmdlet 'ler içerir.

Yeni cmdlet 'ler şunlardır:

- Get-Azureadapplicationproxun
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-Azureadapplicationproxdıte
- New-AzureADApplicationProxyConnectorGroup
- Remove-Azureadapplicationproxfail
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-Azureadapplicationproxun
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---

### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Grupların yapılandırılmasını desteklemeye yönelik yeni cmdlet 'ler

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulama proxy 'si **ürün yeteneği:** platform

AzureAD PowerShell modülünün en son sürümü, Azure AD 'de grupları yönetmek için cmdlet 'ler içerir. Bu cmdlet 'ler daha önce AzureADPreview modülünde kullanılabilir ve artık AzureAD modülüne eklendi

Genel kullanılabilirlik için şimdi serbest olan grup cmdlet 'leri şunlardır:

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup
- Get-AzureADMSLifecyclePolicyGroup

---

### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Azure AD Connect yeni bir sürümü var

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** ad eşitleme **ürün yeteneği:** platform

Azure AD Connect, Windows Server Active Directory ve LDAP dahil olmak üzere Azure AD ile şirket içi veri kaynakları arasında veri eşitlemesi için tercih edilen bir araçtır.

>[!Important]
>Bu derleme şema ve eşitleme kuralı değişikliklerini tanıtır. Azure AD Connect eşitleme hizmeti bir yükseltmeden sonra tam Içeri aktarma ve tam eşitleme adımlarını tetikler. Bu davranışı değiştirme hakkında daha fazla bilgi için, [yükseltmeden sonra tam eşitlemenin ertelenmesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade)bölümüne bakın.

Bu sürümde aşağıdaki güncelleştirmeler ve değişiklikler bulunur:

**Sorun düzeltildi**

- Sonraki sayfaya geçiş yaparken bölüm filtreleme sayfasına yönelik arka plan görevlerinde zamanlama penceresini düzeltir.

- ConfigDB özel eylemi sırasında erişim ihlaline neden olan bir hata düzeltildi.

- SQL bağlantı zaman aşımından kurtarmak için bir hata düzeltildi.

- SAN joker karakterlerinden oluşan sertifikaların ön REQ denetimi başarısız olduğu bir hata düzeltildi.

- AAD Bağlayıcısı dışarı aktarma sırasında miiserver.exe kilitlenmeye neden olan bir hata düzeltildi.

- Çalışırken DC 'de hatalı parola denemesinin oturum açtığı bir hata düzeltildi

**Yeni özellikler ve geliştirmeler**

- Uygulama telemetrisi-Yöneticiler bu veri sınıfını açabilir/kapatabilir.

- Azure AD sistem durumu verileri-Yöneticiler, sistem durumu ayarlarını denetlemek için sistem durumu portalını ziyaret etmelidir. Hizmet ilkesi değiştirildikten sonra aracılar bunu okur ve uygular.

- Sayfa başlatma için cihaz geri yazma yapılandırma eylemleri ve ilerleme çubuğu eklendi.

- Bir ZIP-Text/HTML raporundaki HTML raporu ve tam veri koleksiyonuyla gelişmiş genel Tanılamalar.

- Otomatik yükseltmenin güvenilirliği geliştirildi ve sunucunun sistem durumunun belirlenebildiğinden emin olmak için ek telemetri eklendi.

- AD bağlayıcı hesabında ayrıcalıklı hesaplar için kullanılabilir izinleri kısıtla. Yeni yüklemeler için, sihirbaz, MSOL hesabını oluşturduktan sonra ayrıcalıklı hesapların MSOL hesabında sahip olduğu izinleri kısıtlar. Değişiklikler, otomatik oluşturma hesabıyla hızlı yüklemeleri ve özel yüklemeleri etkiler.

- Yükleyiciyi, AADConnect 'in temiz yüklemesi sırasında SA ayrıcalığı gerektirecek şekilde değiştirdi.

- Belirli bir nesne için eşitleme sorunlarını gidermeye yönelik yeni yardımcı program. Şu anda yardımcı program aşağıdaki şeyleri denetler:

    - Azure AD kiracısındaki eşitlenmiş Kullanıcı nesnesi ile Kullanıcı hesabı arasında UserPrincipalName uyumsuzluğu.

    - Nesne, etki alanı filtreleme nedeniyle eşitlemeden filtrelendiğinde

    - Nesne, kuruluş birimi (OU) filtrelemesi nedeniyle eşitlemeden filtrelendiğinde

- Belirli bir kullanıcı hesabı için şirket içi Active Directory depolanan geçerli parola karmasını eşitlemeye yönelik yeni yardımcı program. Yardımcı program parola değişikliği gerektirmez.

---

### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim ile kullanılmak üzere eklenen Intune Uygulama Koruması ilkelerini destekleyen uygulamalar

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği & koruması

Uygulama tabanlı koşullu erişimi destekleyen daha fazla uygulama ekledik. Artık, bu onaylanan istemci uygulamalarını kullanarak Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişebilirsiniz.

Aşağıdaki uygulamalar Şubat sonuna kadar eklenecektir:

- Microsoft Power BI

- Microsoft başlatıcısı

- Microsoft faturalandırma

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Mobil deneyimle Kullanım koşulları güncelleştirme

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** uyumluluk

Kullanım koşulları görüntülendiğinde artık sorun yaşamadan ' a tıklayabilirsiniz. ** Buraya tıklayın**. Bu bağlantıya tıkladığınızda cihazınızdaki kullanım koşulları yerel olarak açılır. Belgedeki yazı tipi boyutundan veya cihazın ekran boyutundan bağımsız olarak, belgeyi gerektiği gibi yakınlaştırıp okuyabilirsiniz.

---

## <a name="january-2018"></a>Ocak 2018

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Ocak 2018 ' de, uygulama galerisine federasyon desteği olan aşağıdaki yeni uygulamalar eklenmiştir:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [Onetrust gizlilik yönetimi yazılımı](https://go.microsoft.com/fwlink/?linkid=861660), [Satıcıyolu](https://go.microsoft.com/fwlink/?linkid=863526), [ıriusrisfedermi federe dizini ve [uygunlukta netavantajlar](https://go.microsoft.com/fwlink/?linkid=864701).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---

### <a name="sign-in-with-additional-risk-detected"></a>Algılanan ek risk ile oturum açın

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Algılanan bir risk algılaması için aldığınız Öngörüler, Azure AD aboneliğinize bağlıdır. Azure AD Premium P2 sürümüyle, temeldeki Tüm algılamalar hakkında en ayrıntılı bilgileri alırsınız.

Azure AD Premium P1 sürümü sayesinde, lisansınız kapsamında olmayan algılamalar, risk algılama tarafından algılanan ek risklerle birlikte oturum açma olarak görünür.

Daha fazla bilgi için bkz. [risk algılamalarını Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).

---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Office 365 uygulamalarını son kullanıcının erişim panellerinden gizle

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulamalarım **ürün yeteneği:** SSO

Artık, Office 365 uygulamalarının yeni bir Kullanıcı ayarı aracılığıyla kullanıcının erişim panellerinde nasıl gösterileceğini daha iyi yönetebilirsiniz. Bu seçenek, yalnızca Office portalında Office uygulamalarını göstermek istiyorsanız kullanıcının erişim panellerindeki uygulama sayısını azaltmak için yararlıdır. Bu ayar **Kullanıcı ayarlarında** bulunur ve etiketlidir, **kullanıcılar yalnızca Office 365 portalında Office 365 uygulamalarını görebilirler**.

Daha fazla bilgi için bkz. [Azure Active Directory kullanıcının deneyiminden bir uygulamayı gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---

### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Parola SSO 'SU doğrudan uygulamanın URL 'sinden etkin olan uygulamalarda sorunsuz oturum açma

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulamalarım **ürün yeteneği:** SSO

Uygulamalarım tarayıcı uzantısı, artık uygulamalarımın tek oturum açma özelliğini tarayıcınızda bir kısayol olarak sunan kullanışlı bir araç aracılığıyla kullanıma sunulmuştur. ' İ yükledikten sonra, kullanıcıların tarayıcılarında uygulamalara hızlı erişim sağlayan bir waffle simgesi görürsünüz. Kullanıcılar artık şu özelliklerden yararlanabilir:

- Uygulamanın oturum açma sayfasından parola SSO tabanlı uygulamalarda doğrudan oturum açma özelliği
- Hızlı arama özelliğini kullanarak herhangi bir uygulamayı başlatma
- Uzantıdan son kullanılan uygulamalar için kısayollar
- Uzantı, Microsoft Edge, Chrome ve Firefox için kullanılabilir.

Daha fazla bilgi için bkz. [uygulamalarımın güvenli oturum açma uzantısı](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Klasik Azure Portalı Azure AD yönetim deneyimi kullanımdan kaldırılmıştır

**Şunu yazın:** Kullanım dışı **hizmet kategorisi:** Azure AD **ürün yeteneği:** Dizin

8 Ocak 2018 itibariyle, klasik Azure portalındaki Azure AD yönetim deneyimi kullanımdan kaldırılmıştır. Bu, klasik Azure portalının kullanımdan kaldırılması ile birlikte gerçekleşir. Gelecekte Azure AD 'nin tüm portal tabanlı yönetimi için [Azure AD Yönetim merkezini](https://aad.portal.azure.com) kullanmanız gerekir.

---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor Web portalı kullanımdan kaldırılmıştır

**Şunu yazın:** Kullanım dışı **hizmet kategorisi:** Azure AD **ürün yeteneği:** Dizin

8 Ocak 2018 itibariyle PhoneFactor Web portalı kullanımdan kaldırılmıştır. Bu portal MFA sunucusu 'nun yönetimi için kullanılmıştır, ancak bu işlevler portal.azure.com adresinde Azure portal taşınmıştır.

MFA yapılandırması şurada bulunur: **Azure Active Directory \> MFA sunucusu**

---

### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlarını kullanımdan kaldırma

**Şunu yazın:** Kullanım dışı **hizmet kategorisi:** raporlama **ürün yeteneği:** kimlik yaşam döngüsü yönetimi


Yeni Azure Active Directory Yönetim konsolunun genel kullanıma sunulduğuna ve yeni API 'lerin hem etkinlik hem de güvenlik raporlarında kullanılabilir olması sayesinde, "/Reports" uç noktasındaki rapor API 'Leri 31 Aralık 2017 ' in sonunda kullanımdan kaldırılmıştır.

**Neler mevcuttur?**

Yeni Yönetici konsoluna geçişin bir parçası olarak, Azure AD etkinlik günlüklerini almak için 2 yeni API 'si yaptık. Yeni API 'Ler kümesi, daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak daha zengin filtreleme ve sıralama işlevselliği sağlar. Daha önce güvenlik raporları üzerinden kullanılabilen verilere artık Microsoft Graph kimlik koruması risk algılamaları API 'SI aracılığıyla erişilebilir.

Daha fazla bilgi için bkz.

- [Azure Active Directory Reporting API 'SI ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Azure Active Directory Kimlik Koruması ve Microsoft Graph kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Aralık 2017

### <a name="terms-of-use-in-the-access-panel"></a>Erişim panelinde Kullanım koşulları

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** uyumluluk

Artık erişim paneline gidebilir ve önceden kabul ettiğiniz kullanım koşullarını görüntüleyebilirsiniz.

Şu adımları izleyin:

1. [Uygulamaps portalına](https://myapps.microsoft.com)gidin ve oturum açın.

2. Sağ üst köşede adınızı seçin ve ardından listeden **profil** ' i seçin.

3. **Profilinizde** **kullanım koşullarını gözden geçir**' i seçin.

4. Artık kabul ettiğiniz kullanım koşullarını gözden geçirebilirsiniz.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği (Önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-azure-ad-sign-in-experience"></a>Yeni Azure AD oturum açma deneyimi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD ve Microsoft hesabı Identity System U, tutarlı bir görünüme sahip olacak şekilde yeniden tasarlanmıştır. Ayrıca, Azure AD oturum açma sayfası önce Kullanıcı adını, ardından ikinci ekranda kimlik bilgisini toplar.

Daha fazla bilgi için bkz. [yeni Azure AD oturum açma deneyimi artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).

---

### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Daha az oturum açma istemi: Azure AD oturum açma için yeni bir "Oturumumu Açık tut" deneyimi

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD oturum açma sayfasındaki Oturumumu Açık **bırak** onay kutusu, başarıyla kimlik doğrulamasından geçtikten sonra görüntülenen yeni bir istem ile değiştirilmiştir.

Bu istem için **Evet** 'e yanıt verirseniz, hizmet sizi kalıcı yenileme belirteci sağlar. Bu davranış, eski deneyimdeki Oturumumu **açık tut** onay kutusunu seçtiğiniz durumla aynıdır. Federal kiracılar için bu istem, Federasyon Hizmeti ile başarıyla kimlik doğrulamasından geçtikten sonra görüntülenir.

Daha fazla bilgi için bkz. daha [az oturum açma istemi: Azure AD için yeni "Oturumumu Açık tut" deneyimi önizlemededir](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/).

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Kullanım koşullarının kabul edilmeden önce genişletilmesi için yapılandırma ekleyin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** uyumluluk

Yöneticiler için bir seçenek, kullanıcıların koşulları kabul etmeden önce kullanım koşullarını genişletmelerini gerektirir.

Kullanıcıların kullanım koşullarını genişletmesini gerektirmek için **Açık** veya **kapalı** seçeneklerinden birini belirleyin. **Açık** ayarı, kullanıcıların kabul etmeden önce kullanım koşullarını görüntülemesini gerektirir.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği (Önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Privileged Identity Management **ürün özelliği:** Privileged Identity Management

Varsayılan olarak orijinal atama varsayılanlarından daha az bağımsız çalışma sınırı sahip olan uygun Azure Kaynak rolü atamalarını etkinleştirmek için kapsamlı etkinleştirme kullanabilirsiniz. Kiracınızda aboneliğin sahibi olarak atandıysanız örnek bir örnektir. Kapsamlı etkinleştirme sayesinde, abonelik içinde (kaynak grupları ve sanal makineler gibi) bulunan en fazla beş kaynak için sahip rolünü etkinleştirebilirsiniz. Etkinleştirmenin kapsamını belirlemek, önemli Azure kaynaklarında istenmeyen değişiklikler yürütme olasılığını azaltabilir.

Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).

---

### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kurumsal uygulamalar **ürün yeteneği:** üçüncü taraf tümleştirmesi

Bu yeni uygulamaları, Aralık 2017 ' de, uygulama galerimize federasyon desteğiyle ekledik:

[Accanble](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital storefront](https://go.microsoft.com/fwlink/?linkid=861685), [communifire](https://go.microsoft.com/fwlink/?linkid=861676) cybsafe, [factu](https://go.microsoft.com/fwlink/?linkid=863525), [Image çalışma,](https://go.microsoft.com/fwlink/?linkid=863517) [mobi](https://go.microsoft.com/fwlink/?linkid=863521), [Mobileiron Azure AD tümleştirmesi](https://go.microsoft.com/fwlink/?linkid=858027), yansıma [ktive](https://go.microsoft.com/fwlink/?linkid=863518), [tarafından Bamboo için](https://go.microsoft.com/fwlink/?linkid=863520) [SAML SSO](https://go.microsoft.com/fwlink/?linkid=863519) [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD dizin rolleri için onay iş akışları

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Privileged Identity Management **ürün yeteneği:** Privileged Identity Management

Azure AD dizin rolleri için onay iş akışı genel kullanıma sunulmuştur.

Onay iş akışı ile ayrıcalıklı rol yöneticileri, ayrıcalıklı rolü kullanabilmeniz için, uygun rol üyelerinin rol etkinleştirme istemesi için gerekli olabilir. Birden çok kullanıcı ve gruba, onay sorumlulukları atanabilir. Uygun rol üyeleri onay tamamlandığında ve rolleri etkin olduğunda bildirim alır.

---

### <a name="pass-through-authentication-skype-for-business-support"></a>Geçişli kimlik doğrulaması: Skype Kurumsal desteği

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulamaları (oturum açmalar) **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Doğrudan kimlik doğrulaması artık, çevrimiçi ve karma topolojilerini içeren modern kimlik doğrulamayı destekleyen Skype Kurumsal istemci uygulamalarına yönelik kullanıcı oturum açma işlemlerini destekler.

Daha fazla bilgi için bkz. [modern kimlik doğrulamasıyla desteklenen Skype Kurumsal topolojileri](https://technet.microsoft.com/library/mt803262.aspx).

---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Azure RBAC için Azure AD Privileged Identity Management güncelleştirmeleri (Önizleme)

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Privileged Identity Management **ürün yeteneği:** Privileged Identity Management

Azure rol tabanlı erişim denetimi (Azure RBAC) için Azure AD Privileged Identity Management (PıM) genel önizleme yenilemesinde, artık şunları yapabilirsiniz:

* Yalnızca yeterli yönetimi kullanın.
* Kaynak rollerini etkinleştirmek için onay gerektir.
* Hem Azure AD hem de Azure RBAC rolleri için onay gerektiren bir rolün daha sonra etkinleştirilmesini zamanlayın.

Daha fazla bilgi için bkz. [Azure kaynakları için Privileged Identity Management (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

## <a name="november-2017"></a>Kasım 2017

### <a name="access-control-service-retirement"></a>Access Control hizmeti kullanımdan kaldırma

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** Access Control hizmeti **ürün özelliği:** Access Control hizmet

Azure Active Directory Access Control (Access Control hizmeti olarak da bilinir), geç 2018 ' de kullanımdan kaldırılacaktır. Sonraki birkaç hafta içinde ayrıntılı bir zamanlamayı ve üst düzey geçiş kılavuzunu içeren daha fazla bilgi sağlanacaktır. Bu sayfadaki açıklamaları Access Control hizmetiyle ilgili herhangi bir soru ile bırakabilir ve bir takım üyesi onları yanıtlayabilir.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Intune Managed Browser tarayıcı erişimini kısıtlama

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** koşullu erişim **ürün yeteneği:** kimlik güvenliği ve koruma

Onaylanan bir uygulama olarak Intune Managed Browser kullanarak Office 365 ve diğer Azure AD bağlı bulut uygulamalarına tarayıcı erişimini kısıtlayabilirsiniz.

Artık uygulama tabanlı koşullu erişim için aşağıdaki koşulu yapılandırabilirsiniz:

**İstemci uygulamaları:** Tarayıcı

**Değişikliğin etkisi nedir?**

Bugün, bu durumu kullandığınızda erişim engellenir. Önizleme kullanılabilir olduğunda, tüm erişimlerle yönetilen tarayıcı uygulamasının kullanılması gerekir.

Yaklaşan blogların ve sürüm notlarındaki bu özelliği ve daha fazla bilgiyi bulun.

Daha fazla bilgi için bkz. [Azure AD 'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** koşullu erişim **ürün yeteneği:** kimlik güvenliği ve koruma

Aşağıdaki uygulamalar [onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesidir:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- Microsoft StaffHub

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Birden çok dil için kullanım koşulları desteği

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** uyumluluk

Yöneticiler, artık birden çok PDF belgesi içeren yeni kullanım koşulları oluşturabilir. Bu PDF belgelerini karşılık gelen bir dille etiketlendirebilirsiniz. Kullanıcılar, tercihleri temelinde, eşleşen dille PDF gösterilir. Eşleşme yoksa, varsayılan dil gösterilir.

---

### <a name="real-time-password-writeback-client-status"></a>Gerçek zamanlı parola geri yazma istemci durumu

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** self servis parola sıfırlama **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Artık şirket içi parola geri yazma istemcinizdeki durumu gözden geçirebilirsiniz. Bu seçenek, [parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) sayfasının **Şirket içi tümleştirme** bölümünde bulunur.

Şirket içi geri yazma istemcinizdeki bağlantınız ile ilgili sorunlar varsa şunları sağlayan bir hata iletisi görürsünüz:

- Şirket içi geri yazma istemcinizle ilgili olarak neden bağlanamadığına ilişkin bilgiler.
- Sorunu çözmenize yardımcı olacak belge bağlantısı.

Daha fazla bilgi için bkz. [Şirket içi tümleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD **ürün yeteneği:** kimlik güvenliği ve koruması

Artık Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişimi, [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)kullanarak Intune uygulama koruma ilkelerini destekleyen [onaylanan istemci uygulamalarına](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview) kısıtlayabilirsiniz. Intune uygulama koruma ilkeleri, bu istemci uygulamalarında şirket verilerini yapılandırmak ve korumak için kullanılır.

[Uygulama tabanlı](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) [cihaz tabanlı](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) koşullu erişim ilkeleriyle, kişisel ve şirket cihazlarındaki verileri koruma esnekliğine sahip olursunuz.

Aşağıdaki koşullar ve denetimler artık uygulama tabanlı koşullu erişim ile birlikte kullanılabilir:

**Desteklenen platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve Masaüstü istemcileri

**Erişim denetimi**

- Onaylanan istemci uygulaması gerektir

Daha fazla bilgi için bkz. [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).

---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure portal Azure AD cihazlarını yönetme

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** cihaz kaydı ve Yönetim **ürün yeteneği:** kimlik güvenliği ve koruması

Artık Azure AD 'ye bağlı tüm cihazlarınızı ve cihazla ilgili etkinlikleri tek bir yerde bulabilirsiniz. Azure portal tüm cihaz kimliklerinizi ve ayarlarını yönetmek için yeni bir yönetim deneyimi vardır. Bu sürümde şunları yapabilirsiniz:

- Azure AD 'de koşullu erişim için kullanılabilen tüm cihazlarınızı görüntüleyin.
- Karma Azure AD 'ye katılmış cihazlarınızı içeren özellikleri görüntüleyin.
- Azure AD 'ye katılmış cihazlarınız için BitLocker anahtarları bulun, cihazınızı Intune ile yönetin ve daha fazlasını yapın.
- Azure AD cihazla ilgili ayarları yönetin.

Daha fazla bilgi için [Azure Portal kullanarak cihazları yönetme](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal)konusuna bakın.

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Azure AD koşullu erişim için bir cihaz platformu olarak macOS desteği

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği ve koruması

Artık macOS 'ı Azure AD koşullu erişim ilkenize cihaz platformu olarak dahil edebilir (veya dışlayabilirsiniz). MacOS 'un desteklenen cihaz platformlarına eklenmesi sayesinde şunları yapabilirsiniz:

- **Intune kullanarak macOS cihazlarını kaydedin ve yönetin.** İOS ve Android gibi diğer platformlara benzer şekilde, macOS 'un birleştirilmiş kayıtları yapması için bir şirket portalı uygulaması vardır. MacOS için yeni şirket portalı uygulamasını kullanarak bir cihazı Intune 'a kaydedebilir ve Azure AD 'ye kaydedebilirsiniz.
- **MacOS cihazlarının, kuruluşunuzun Intune 'da tanımlanan uyumluluk ilkelerine bağlı olduğundan emin olun.** Azure portal Intune 'da, artık macOS cihazları için uyumluluk ilkeleri ayarlayabilirsiniz.
- **Azure AD 'deki uygulamalara erişimi yalnızca uyumlu macOS cihazlarına kısıtlayın.** Koşullu erişim ilkesi yazma, macOS öğesini ayrı bir cihaz platformu seçeneği olarak içerir. Artık Azure 'da ayarlanan hedeflenen uygulama için macOS 'a özgü koşullu erişim ilkelerini yazabilirsiniz.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazları için cihaz uyumluluğu ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD 'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)

---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication için ağ Ilkesi sunucusu uzantısı

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Multi-Factor Authentication **ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure Multi-Factor Authentication için ağ Ilkesi sunucu uzantısı, mevcut sunucularınızı kullanarak kimlik doğrulama altyapınıza bulut tabanlı Multi-Factor Authentication özellikleri ekler. Ağ Ilkesi sunucusu uzantısıyla, mevcut kimlik doğrulama akışınıza telefon araması, SMS mesajı veya telefon uygulaması doğrulaması ekleyebilirsiniz. Yeni sunucuları yüklemek, yapılandırmak ve bakımını yapmak zorunda değilsiniz.

Bu uzantı, Azure Multi-Factor Authentication Sunucusu dağıtımı yapmadan sanal özel ağ bağlantılarını korumak isteyen kuruluşlar için oluşturulmuştur. Ağ Ilkesi sunucu uzantısı, federal veya eşitlenmiş kullanıcılar için ikinci bir kimlik doğrulama faktörü sağlamak üzere RADIUS ve bulut tabanlı Azure Multi-Factor Authentication arasında bir bağdaştırıcı görevi görür.

Daha fazla bilgi için bkz. [mevcut ağ Ilkesi sunucusu altyapınızı Azure Multi-Factor Authentication tümleştirme](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).

---

### <a name="restore-or-permanently-remove-deleted-users"></a>Silinen kullanıcıları geri yükleme veya kalıcı olarak kaldırma

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Kullanıcı Yönetimi **ürün yeteneği:** Dizin

Azure AD Yönetim merkezinde artık şunları yapabilirsiniz:

- Silinen bir kullanıcıyı geri yükleyin.
- Bir kullanıcıyı kalıcı olarak silme.

**Denemek için:**

1. Azure AD Yönetim merkezinde **Yönet** bölümündeki [tüm kullanıcılar](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) ' ı seçin.

2. **Göster** listesinden, **son silinen kullanıcılar**' ı seçin.

3. Son silinen bir veya daha fazla kullanıcı seçin ve ardından onları geri yükleyin veya kalıcı olarak silin.

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği ve koruma

[Onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesine aşağıdaki uygulamalar eklenmiştir:

- Microsoft Planner
- Azure Information Protection

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Koşullu erişim ilkesindeki denetimler arasında "OR" kullanın

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** koşullu erişim **ürün yeteneği:** kimlik güvenliği ve koruma

Koşullu erişim denetimleri için artık "veya" (seçili denetimlerden birini gerektir) kullanabilirsiniz. Bu özelliği, erişim denetimleri arasında "veya" ile ilke oluşturmak için kullanabilirsiniz. Örneğin, bir kullanıcının uyumlu bir cihazda olması için Multi-Factor Authentication "veya" kullanarak oturum açmasını gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için bkz. [Azure AD koşullu erişim 'Deki denetimler](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).

---

### <a name="aggregation-of-real-time-risk-detections"></a>Gerçek zamanlı risk algılamalarını toplama

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik koruması **ürün yeteneği:** kimlik güvenliği ve koruması

Azure AD Kimlik Koruması, belirli bir gündeki aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk algılamaları artık her risk algılama türü için toplanır. Bu değişiklik, Kullanıcı güvenliğine hiçbir değişiklik yapılmadan gösterilen risk algılamaları hacminin sınırlarını sınırlar.

Temeldeki gerçek zamanlı algılama, kullanıcının her oturum açışında işe yarar. Multi-Factor Authentication veya erişimi engellemek üzere ayarlanmış bir oturum açma riski güvenlik ilkesi varsa, bu, her riskli oturum açma işlemi sırasında tetiklenir.

---

## <a name="october-2017"></a>Ekim 2017

### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlarını kullanımdan kaldırma

**Şunu yazın:** Değişiklik **hizmeti kategorisini planlayın:** raporlama **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

Azure portal şunları sağlar:

- Yeni bir Azure AD Yönetim Konsolu.
- Etkinlik ve güvenlik raporları için yeni API 'Ler.

Bu yeni yetenekler nedeniyle,/Reports uç noktasındaki rapor API 'Leri 10 Aralık 2017 ' de kullanımdan kalkmıştı.

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alanı algılama

**Şunu yazın:** Sabit **hizmet kategorisi:** uygulamalarım **ürün yeteneği:** çoklu oturum açma

Azure AD, bir HTML Kullanıcı adı ve parola alanı işleyen uygulamalar için otomatik oturum açma alanı algılamayı destekler. Bu adımlar, [bir uygulama için oturum açma alanlarını otomatik olarak yakalama](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications-problems#manually-capture-sign-in-fields-for-an-app)bölümünde belgelenmiştir. Bu özelliği, [Azure Portal](https://aad.portal.azure.com) **Enterprise Applications** sayfasında *Galeri dışı* bir uygulama ekleyerek bulabilirsiniz. Ayrıca, bu yeni uygulamadaki **Çoklu oturum** açma modunu **parola tabanlı çoklu oturum açma**için yapılandırabilir, bir Web URL 'si girebilir ve sonra sayfayı kaydedebilirsiniz.

Bir hizmet sorunu nedeniyle, bu işlev geçici olarak devre dışı bırakıldı. Sorun çözüldü ve otomatik oturum açma alanı algılaması yeniden kullanılabilir.

---

### <a name="new-multi-factor-authentication-features"></a>Yeni Multi-Factor Authentication özellikleri

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Multi-Factor Authentication **ürün yeteneği:** kimlik güvenliği ve koruması

Multi-Factor Authentication (MFA), kuruluşunuzu korumanın önemli bir parçasıdır. Kimlik bilgilerini daha uyumlu hale getirmek ve deneyimi daha sorunsuz hale getirmek için aşağıdaki özellikler eklenmiştir:

- Multi-Factor Challenge sonuçları doğrudan Azure AD oturum açma raporuyla tümleşiktir ve bu da MFA sonuçlarına programlı erişim içerir.
- MFA yapılandırması, Azure portal Azure AD yapılandırma deneyimiyle daha ayrıntılı bir şekilde tümleştirilir.

Bu genel önizleme sayesinde MFA yönetimi ve raporlama, çekirdek Azure AD yapılandırma deneyiminin tümleşik bir parçasıdır. Artık MFA yönetim portalı işlevini Azure AD deneyiminden yönetebilirsiniz.

Daha fazla bilgi için bkz. [Azure Portal MFA raporlaması Için başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa).

---

### <a name="terms-of-use"></a>Kullanım koşulları

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** kullanım koşulları **ürün yeteneği:** uyumluluk

Kullanıcılara yasal veya uyumluluk gereksinimleri için ilgili bildirimler gibi bilgileri sunmak için Azure AD kullanım koşulları 'nı kullanabilirsiniz.

Azure AD kullanım koşulları 'nı aşağıdaki senaryolarda kullanabilirsiniz:

- Kuruluşunuzdaki tüm kullanıcılar için genel kullanım koşulları
- Kullanıcının özniteliklerine dayalı belirli kullanım koşulları (örneğin, doktorlarla veya yerel ve uluslararası çalışanlar, dinamik gruplar tarafından yapılır)
- Salesforce gibi yüksek etkili iş uygulamalarına erişmek için belirli kullanım koşulları

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="enhancements-to-privileged-identity-management"></a>Privileged Identity Management geliştirmeleri

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Privileged Identity Management **ürün özelliği:** Privileged Identity Management

Azure AD Privileged Identity Management, kuruluşunuzda Azure kaynaklarına (Önizleme) erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz:

- Abonelikler
- Kaynak grupları
- Sanal makineler

Azure RBAC işlevselliğini kullanan Azure portal tüm kaynaklar, Azure AD Privileged Identity Management sunabileceği tüm güvenlik ve yaşam döngüsü yönetimi yetilerinden yararlanabilir.

Daha fazla bilgi için bkz. [Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Erişim gözden geçirmeleri

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** erişim gözden geçirmeleri **ürün yeteneği:** uyumluluk

Kuruluşlar, grup üyeliklerini verimli bir şekilde yönetmek ve kurumsal uygulamalara erişmek için erişim gözden geçirmeleri (Önizleme) kullanabilir:

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler, erişim gözden geçirmeleri tarafından belirtilen Öngörüler temelinde konuklara erişime izin verip vermeyeceğine karar verebilir.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için bkz. [Azure AD erişim İncelemeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Uygulamalarım ve Office 365 uygulama başlatıcısı 'ndan üçüncü taraf uygulamaları gizleyin

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** uygulamalarım **ürün yeteneği:** çoklu oturum açma

Şimdi, yeni bir **uygulama Gizle** özelliği aracılığıyla kullanıcılarınızın portallarında görüntülenen uygulamaları daha iyi yönetebilirsiniz. Uygulama kutucuklarının arka uç hizmetleri veya yinelenen kutucuklar ve en yüksek kullanıcıların uygulama başlatanlar için göstereceği durumlarda yardımcı olması için uygulamaları gizleyebilirsiniz. Geçiş, üçüncü taraf uygulamanın **Özellikler** bölümüdür ve **Kullanıcı tarafından görülebilir** olarak etiketlenir. Ayrıca, bir uygulamayı PowerShell aracılığıyla programlı bir şekilde gizleyebilirsiniz.

Daha fazla bilgi için bkz. [Azure AD 'de kullanıcının deneyiminden üçüncü taraf bir uygulamayı gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).


**Neler mevcuttur?**

 Yeni Yönetici konsoluna geçişin bir parçası olarak, Azure AD etkinlik günlüklerini almak için iki yeni API kullanılabilir. Yeni API 'Ler kümesi, daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak daha zengin filtreleme ve sıralama işlevselliği sağlar. Daha önce güvenlik raporları üzerinden kullanılabilen verilere artık Microsoft Graph kimlik koruması risk algılamaları API 'SI aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-identity-manager"></a>Identity Manager düzeltmesi

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** Identity Manager **ürün yeteneği:** kimlik yaşam döngüsü yönetimi

Kimlik Yöneticisi 2016 Service Pack 1 ' den itibaren, 25 Eylül 2017 itibariyle bir düzeltme toplaması paketi (Build 4.4.1642.0) kullanılabilir. Bu toplama paketi:

- Sorunları çözer ve iyileştirmeler ekler.
- , Identity Manager 2016 için 4.4.1459.0 derlemesi için tüm Identity Manager 2016 hizmet paketi 1 güncelleştirmelerini değiştiren bir toplu güncelleştirmedir.
- Identity Manager 2016 derleme 4.4.1302.0 sahip olmanızı gerektirir.

Daha fazla bilgi için bkz. [Identity Manager 2016 Service Pack 1 Için düzeltme paketi paketi (Build 4.4.1642.0) kullanılabilir](https://support.microsoft.com/help/4021562).

---
