---
title: Yenilikler Sürüm notları-Azure Active Directory | Microsoft Docs
description: Azure Active Directory yenilikleri öğrenin; en son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevsellik ve yaklaşan değişiklikler gibi.
services: active-directory
author: ajburnle
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 1/29/2021
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c835ddcee332499ef7853d0d2fa4fcdbcf8a1cf
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99988873"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory yenilikler nelerdir?

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` ![ RSS akışı okuyucu simgesi ](./media/whats-new/feed-icon-16x16.png) akış okuyucusuna.

Azure AD, iyileştirmeleri sürekli olarak alır. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik
- Değişiklik planları

Bu sayfa aylık olarak güncelleştirildiğinden düzenli olarak yeniden ziyaret edin. Altı aydan eski olan öğeleri arıyorsanız, [Azure Active Directory yenilikleri Için arşiv](whats-new-archive.md)halinde bulabilirsiniz.

---
## <a name="january-2021"></a>Ocak 2021

### <a name="secret-token-will-be-a-mandatory-field-when-configuring-provisioning"></a>Sağlama yapılandırılırken gizli belirteç zorunlu bir alan olacaktır

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Geçmişte, özel/KCG uygulamasında sağlama ayarlanırken gizli dizi belirteci alanı boş tutulabilir. Bu işlev yalnızca test için kullanılmak üzere tasarlanmıştır. Alanı gerekli hale getirmek için Kullanıcı arabirimini güncelleştireceğiz. 

Müşteriler, tarayıcı URL 'sindeki bir özellik bayrağını kullanarak test amacıyla bu gereksinimi geçici olarak çözebilirler. [Daha fazla bilgi edinin](../app-provisioning/use-scim-to-provision-users-and-groups.md#authorization-to-provisioning-connectors-in-the-application-gallery).
 
---

### <a name="public-preview---customize-and-configure-android-shared-devices-for-firstline-workers-at-scale"></a>Genel Önizleme-Ilk satır çalışanları için Android paylaşılan cihazlarını ölçeklendirerek özelleştirin ve yapılandırın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Cihaz kaydı ve yönetimi  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Azure AD ve Microsoft Uç Nokta Yöneticisi ekipleri, Firstline çalışan cihazlarınızı özelleştirme, ölçeklendirme ve güvenli hale getirmeye yönelik özelliği birleştirmelidir.

Aşağıdaki Önizleme özellikleri şunları yapmanıza olanak sağlayacak:
- Microsoft Endpoint Manager ile Android paylaşılan cihazlarını ölçekli olarak sağlama
- Cihaz tabanlı koşullu erişim kullanarak vardiya çalışanları için erişiminizi güvenli hale getirme
- Yönetilen giriş ekranı ile vardiya çalışanları için oturum açma deneyimlerini özelleştirin

Daha fazla bilgi edinmek için bkz. [Firstline çalışanları için paylaşılan cihazları ölçeklendirerek özelleştirme ve yapılandırma](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-and-configure-shared-devices-for-firstline-workers-at/ba-p/1751708).

---

### <a name="public-preview---provisioning-logs-can-now-be-downloaded-as-a-csv-or-json"></a>Genel Önizleme-sağlama günlükleri artık CSV veya JSON olarak indirilebilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Müşteriler, sağlama günlüklerini Kullanıcı arabirimi ve Graph API aracılığıyla bir CSV veya JSON dosyası olarak indirebilir. Daha fazla bilgi edinmek için [Azure Active Directory portalındaki sağlama raporları](../reports-monitoring/concept-provisioning-logs.md)bölümüne bakın.

---

### <a name="public-preview---assign-cloud-groups-to-azure-ad-custom-roles-and-admin-unit-scoped-roles"></a>Genel Önizleme-bulut gruplarını Azure AD özel rollerine ve yönetici birimi kapsamlı rollere atayın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control
 
Müşteriler, bir bulut grubunu Azure AD özel rollerine veya bir yönetici birimi kapsamlı rolüne atayabilir. Bu özelliği nasıl kullanacağınızı öğrenmek için [Azure Active Directory ' de rol atamalarını yönetmek üzere bulut gruplarını kullanma](../roles/groups-concept.md)konusuna bakın.

---

### <a name="general-availability---azure-ad-connect-cloud-sync-previously-known-as-cloud-provisioning"></a>Genel kullanılabilirlik-Azure AD Connect bulut eşitlemesi (daha önce bulut sağlama olarak bilinirdi)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Connect bulut eşitleme  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure AD Connect bulut eşitlemesi artık tüm müşteriler tarafından genel kullanıma sunulmuştur.

Azure AD Connect bulutu, dönüştürme mantığının büyük bir şekilde buluta taşınması ve şirket içi parmak izinizi azaltır. Ayrıca, daha fazla eşitleme kullanılabilirliği için birden çok hafif aracı dağıtımı vardır. [Daha fazla bilgi edinin](https://aka.ms/cloudsyncGA).
 
---
### <a name="general-availability---attack-simulation-administrator-and-attack-payload-author-built-in-roles"></a>Genel kullanılabilirlik-saldırı simülasyonu Yöneticisi ve saldırı yükü yazarı yerleşik roller

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control
 
Role-Based Access Control içindeki iki yeni rol, kullanıcılara, saldırı simülasyonu yöneticisine ve saldırı yükü yazarına atamak için kullanılabilir. 

[Saldırı simülasyonu yönetici](../roles/permissions-reference.md#attack-simulation-administrator) rolündeki kullanıcıların Kiracıdaki tüm benzetimler için erişimi vardır ve şunları yapabilir:
- saldırı simülasyonu oluşturma 'nın tüm yönlerini oluşturun ve yönetin
- simülasyonu başlatma/zamanlama
-  simülasyon sonuçlarını gözden geçirin. 

[Saldırı yükü yazar](../roles/permissions-reference.md#attack-payload-author) rolündeki kullanıcılar, saldırı yükleri oluşturabilir, ancak bunu gerçekten başlatamaz veya zamanlayamaz. Böylece saldırı yükleri, kiracının benzetim oluşturmak için kullanabilecekleri tüm yöneticiler tarafından kullanılabilir.

---

### <a name="general-availability---usage-summary-reports-reader-built-in-role"></a>Genel kullanılabilirlik-Kullanım Özeti raporları okuyucu yerleşik rolü

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control
 
Kullanım Özeti raporları okuyucu rolüne sahip kullanıcılar, kullanım ve üretkenlik puanı için Microsoft 365 Yönetim Merkezi 'ndeki kiracı düzeyindeki toplu verilere ve ilişkili öngörülere erişebilir. Ancak, herhangi bir Kullanıcı düzeyi ayrıntılarına veya öngörülere erişemez. 

İki rapor için Microsoft 365 Yönetim merkezinde, kiracı düzeyi toplanmış veriler ve Kullanıcı düzeyi ayrıntıları arasında ayrım yaptık. Bu rol, bireysel kullanıcı tarafından tanımlanabilen verilere ek bir koruma katmanı ekler. [Daha fazla bilgi edinin](../roles/permissions-reference.md#usage-summary-reports-reader).

---

### <a name="general-availability---require-app-protection-policy-grant-in-azure-ad-conditional-access"></a>Genel kullanılabilirlik-Azure AD koşullu erişim 'de uygulama koruma ilkesi izni ıste

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
"Uygulama koruma ilkesi gerektir" için Azure AD koşullu erişim izni artık GA. 

İlke aşağıdaki özellikleri sağlar:
- Yalnızca Intune uygulama korumasını destekleyen bir mobil uygulama kullanılırken erişime izin verir
- Yalnızca bir Kullanıcı mobil uygulamaya teslim edilen bir Intune uygulama koruma ilkesine sahip olduğunda erişime izin verir

[Burada](../conditional-access/app-protection-based-conditional-access.md)uygulama koruması için bir koşullu erişim ilkesi ayarlama hakkında daha fazla bilgi edinin.
 
---

### <a name="general-availability---email-one-time-passcode"></a>Genel kullanılabilirlik-e-posta One-Time geçiş kodu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
E-posta OTP, dünyanın dört bir yanındaki kuruluşların e-posta yoluyla bağlantı veya davetiye göndererek kişilerle işbirliği yapmasına olanak sağlar. Davet edilen kullanıcılar kendi iş ortaklarının kaynaklarına erişmek için e-postalarına gönderilen tek seferlik geçiş kodu ile kimliklerini doğrulayabilirler. [Daha fazla bilgi edinin](../external-identities/one-time-passcode.md). 
 
---

 ### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2021"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Ocak 2021

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:
- [Fortes Change Cloud](../saas-apps/fortes-change-cloud-provisioning-tutorial.md)
- [Gtmhub](../saas-apps/gtmhub-provisioning-tutorial.md)
- [monday.com](../saas-apps/mondaycom-provisioning-tutorial.md)
- [Splashtop](../saas-apps/splashtop-provisioning-tutorial.md)
- [Templamy OpenID Connect](../saas-apps/templafy-openid-connect-provisioning-tutorial.md)
- [WEDO](../saas-apps/wedo-provisioning-tutorial.md)

Daha fazla bilgi için bkz. [Azure AD 'de otomatik SaaS uygulaması Kullanıcı hazırlama nedir?](../app-provisioning/user-provisioning.md)

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---january-2021"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ocak 2021

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Ocak 2021 ' de, federasyon desteğiyle uygulama galerimize aşağıdaki 29 yeni uygulamayı ekledik:

[Myscview](https://dev.myscview.com/), [talentech](https://talentech.com/contact/), [bipsync](https://www.bipsync.com/), [OroTimesheet](https://app.orotimesheet.com/login.php), [IO](https://app.m.io/auth/install/microsoft?scopetype=hub), [sovelto Easy](https://login.soveltoeasy.fi/), [SUPPORTBENCH](https://account.supportbench.net/agent/login/),[biensin](https://formation.bienvenue.pro/login)işleme, [Aida sağlık SSO](https://aidaforparents.com/login/organizations), [Uluslararası SOS yardım ürünleri](../saas-apps/international-sos-assistance-products-tutorial.md), [NAVEX One](../saas-apps/navex-one-tutorial.md), [lablog](../saas-apps/lablog-tutorial.md), [Oktopost SAML](../saas-apps/oktopost-saml-tutorial.md), [ePhoto Dam](../saas-apps/ephoto-dam-tutorial.md), [kavram](../saas-apps/notion-tutorial.md), [syndio](../saas-apps/syndio-tutorial.md), [sarılo Enterprise](../saas-apps/yello-enterprise-tutorial.md), [TimeClock 365 SAML](../saas-apps/timeclock-365-saml-tutorial.md), [NALCO E-Data](https://www.ecolab.com/), [boş konum filler](https://app.vacancy-filler.co.uk/VFMVC/Account/Login), [Synerıse AI büyüme ekosistemi](../saas-apps/synerise-ai-growth-ecosystem-tutorial.md), [zva Data Security](../saas-apps/imperva-data-security-tutorial.md), [Illusive Networks](../saas-apps/illusive-networks-tutorial.md), [proware](../saas-apps/proware-tutorial.md), [splan ziyaretçisi](../saas-apps/splan-visitor-tutorial.md), [Aruba Kullanıcı deneyimi öngörüleri](../saas-apps/aruba-user-experience-insight-tutorial.md), [contentsquare SSO](../saas-apps/contentsquare-sso-tutorial.md), [çevre 81](../saas-apps/perimeter-81-tutorial.md), [Burp Suite Enterprise Edition](https://docs.microsoft.com/azure/active-directory/saas-apps/burp-suite-enterprise-edition-tutorial)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz. https://aka.ms/AppsTutorial

Uygulamanızı Azure AD uygulama galerisinde listelemek için lütfen buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest 

---

### <a name="public-preview---second-level-manager-can-be-set-as-alternate-approver"></a>Genel Önizleme-Ikinci düzey yönetici, alternatif onaylayan olarak ayarlanabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Onaylayanlar ' i seçtiğinizde, artık yetkilendirme yönetiminde ek bir seçenek mevcuttur. Ilk onaylayanın "onaylayan olarak yönetici" seçeneğini belirlerseniz, alternatif onaylayan alanında seçebileceğiniz başka bir "Ikinci düzey yönetici, alternatif onaylayan olarak" seçeneğine sahip olursunuz. Bu seçeneği belirlerseniz, sistemin ikinci düzey yöneticiyi bulamadığı durumlarda isteği iletmek için bir geri dönüş onaylayan eklemeniz gerekir. [Daha fazla bilgi edinin](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers)
 
---

### <a name="general-availability---navigate-to-teams-directly-from-my-access-portal"></a>Genel kullanılabilirlik-doğrudan erişim portalından takımlara git

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Artık takımları doğrudan erişim portalından başlatabilirsiniz. 

Bunu yapmak için erişimimde oturum açın ( https://myaccess.microsoft.com/) "erişim paketlerine" gidin, daha sonra erişim sahibi olduğunuz tüm erişim paketlerini görmek için "etkin" sekmesine gidin. Seçili erişim paketini genişlettikten ve takımlar üzerine geldiğinizde, "Aç" düğmesine tıklayarak bunu başlatabilirsiniz. [Daha fazla bilgi edinin](../governance/entitlement-management-request-access.md).
 
---

### <a name="improved-logging--end-user-prompts-for-risky-guest-users"></a>Riskli Konuk kullanıcılar için iyileştirilmiş günlük & End-User Istemleri

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 

Riskli Konuk kullanıcılar için günlüğe kaydetme ve End-User Istemleri güncelleştirildi. [Kimlik koruması ve B2B kullanıcıları](../identity-protection/concept-identity-protection-b2b.md)hakkında daha fazla bilgi edinin.
 
---
 
## <a name="december-2020"></a>Aralık 2020

### <a name="public-preview---azure-ad-b2c-phone-sign-up-and-sign-in-using-built-in-policy"></a>Genel Önizleme-yerleşik Ilke kullanarak telefon kayıt ve oturum açma Azure AD B2C

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
B2C telefon kayıt ve oturum açma, yerleşik Ilke kullanarak BT yöneticileri ve kuruluş geliştiricilerinin, son kullanıcılarınızın Kullanıcı akışlarında telefon numarası kullanarak oturum açmasını ve kaydolmasına izin vermesini sağlar. Daha fazla bilgi edinmek için [Kullanıcı akışları (Önizleme) için telefon kaydı ve oturum açma ayarlama](../../active-directory-b2c/phone-authentication-user-flows.md) makalesini okuyun.

---

### <a name="general-availability---security-defaults-now-enabled-for-all-new-tenants-by-default"></a>Genel kullanılabilirlik-güvenlik Varsayılanları artık varsayılan olarak tüm yeni kiracılar için etkinleştirilmiştir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Kullanıcı hesaplarını korumak için, 12 Kasım 2020 tarihinde veya sonrasında oluşturulan tüm yeni kiracılar güvenlik Varsayılanları etkin olacak şekilde gelir. Güvenlik Varsayılanları aşağıdakiler dahil birden çok ilkeyi zorunlu tutar:
- Tüm kullanıcıların ve yöneticilerin Microsoft Authenticator uygulamasını kullanarak MFA 'ya kaydolmanızı gerektirir
- Her oturum açtıklarında MFA 'yı kullanmak için kritik yönetici rolleri gerektirir. Tüm diğer kullanıcılara her gerektiğinde MFA sorulur. 
- Eski kimlik doğrulama, kiracı genelinde engellenecek. 

Daha fazla bilgi için [güvenlik varsayılanlarını nelerdir?](../fundamentals/concept-fundamentals-security-defaults.md) bölümünü okuyun.

---

### <a name="general-availability---support-for-groups-with-up-to-250k-members-in-aadconnect"></a>Genel kullanılabilirlik-AADConnect 'te en fazla 250 k üye içeren gruplar için destek

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** AD Connect  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Microsoft, Azure Active Directory için eşitleme hizmeti işlemlerinin performansını geliştiren Azure AD Connect için yeni bir uç nokta (API) dağıttı. Yeni [v2 uç noktasını](../hybrid/how-to-connect-sync-endpoint-api-v2.md)kullandığınızda, Azure AD 'ye dışarı aktarma ve içeri aktarma sırasında dikkat çekici performans kazançları ile karşılaşırsınız. Bu yeni uç nokta aşağıdaki senaryoları destekler:

- En fazla 250 k üye içeren grupları eşitleme
- Azure AD 'ye verme ve içeri aktarma ile ilgili performans artışı

---

### <a name="general-availability---entitlement-management-available-for-tenants-in-azure-china-cloud"></a>Genel kullanılabilirlik-Azure Çin bulutundaki kiracılar için kullanılabilir Yetkilendirme Yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 

Yetkilendirme yönetiminin özellikleri artık Azure Çin bulutundaki tüm kiracılar için kullanılabilir. Bilgi için [Identity idare belgeleri](https://docs.azure.cn/zh-cn/active-directory/governance/) sitemizi ziyaret edin.

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---december-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Aralık 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Dijital Süreç Otomasyonu için Bizagi Studio](../saas-apps/bizagi-studio-for-digital-process-automation-provisioning-tutorial.md)
- [CybSafe](../saas-apps/cybsafe-provisioning-tutorial.md)
- [GroupTalk](../saas-apps/grouptalk-provisioning-tutorial.md)
- [PaperCut Cloud Print Management](../saas-apps/papercut-cloud-print-management-provisioning-tutorial.md)
- [Parsable](../saas-apps/parsable-provisioning-tutorial.md)
- [Shopify Plus](../saas-apps/shopify-plus-provisioning-tutorial.md)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](../app-provisioning/user-provisioning.md).
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---december-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Aralık 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2020 Aralık 'ta, federasyon desteğiyle App Gallery 'de aşağıdaki 18 yeni uygulamayı ekledik:

[Awarego](../saas-apps/awarego-tutorial.md), [hownow SSO](https://gethownow.com/), [Zylab tek bir yasal saklama](https://www.zylab.com/en/product/legal-hold), [gulayis](http://www.guider-ai.com/), Softpais, [Pims 365](http://www.omega365.com/pims), bilgi [dönüştürme](../saas-apps/informacast-tutorial.md), elde edilen [her ne kadar](https://www.softcrisis.se/sv/) [,](../saas-apps/retrievermediadatabase-tutorial.md)bir [işlem içi Pano](../saas-apps/count-me-in-operations-dashboard-tutorial.md), [ProProfs Bilgi Bankası](../saas-apps/proprofs-knowledge-base-tutorial.md), [Rightcrowd iş gücü yönetimi](../saas-apps/rightcrowd-workforce-management-tutorial.md), [JLL Trıriga](../saas-apps/jll-tririga-tutorial.md), [Shutterstock](../saas-apps/shutterstock-tutorial.md), [FortiWeb Web uygulaması güvenlik duvarı](../saas-apps/linkedin-talent-solutions-tutorial.md), [LinkedIn tatatzı](../saas-apps/linkedin-talent-solutions-tutorial.md), [Equinix Federasyon uygulaması](../saas-apps/equinix-federation-app-tutorial.md), [kfadvance](../saas-apps/kfadvance-tutorial.md) [](../saas-apps/vonage-tutorial.md)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz. https://aka.ms/AppsTutorial

Uygulamanızı Azure AD uygulama galerisinde listelemek için buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest

---

### <a name="navigate-to-teams-directly-from-my-access-portal"></a>Takımlara doğrudan erişim portalından git

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanıcı erişimi Yönetimi **ürün yeteneği:** Yetkilendirme Yönetimi

Artık takımları doğrudan erişim portalından başlatabilirsiniz. Bunu yapmak için [erişimimde](https://myaccess.microsoft.com/)oturum açın, erişim **paketleri**' ne gidin ve daha önce erişiminiz olan tüm erişim paketlerini görmek için **etkin** sekmesine gidin. Erişim paketini genişlettikten ve takımlar üzerine geldiğinizde, **Aç** düğmesine tıklayarak uygulamayı başlatabilirsiniz. 

Erişim Portalı 'nı kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure AD Yetkilendirme Yönetimi 'nde erişim paketine erişim isteği](../governance/entitlement-management-request-access.md#sign-in-to-the-my-access-portal).

---

### <a name="public-preview---second-level-manager-can-be-set-as-alternate-approver"></a>Genel Önizleme-Ikinci düzey yönetici, alternatif onaylayan olarak ayarlanabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi

Artık yetkilendirme yönetiminde onay sürecinde fazladan bir seçenek mevcuttur. Ilk onaylayan için yöneticiyi onaylayan olarak seçerseniz, alternatif onaylayan alanında tercih ettiğiniz Ikinci düzey yönetici olan alternatif onaylayan olarak diğer bir seçeneğe sahip olursunuz. Bu seçeneği belirlediğinizde, sistemin ikinci düzey yöneticiyi bulamadığı durumlarda isteği iletmek için bir geri dönüş onaylayan eklemeniz gerekir.

Daha fazla bilgi için [Azure AD yetkilendirme yönetiminde bir erişim paketinin onay ayarlarını değiştirin](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers)bölümüne gidin.

--- 

## <a name="november-2020"></a>Kasım 2020

### <a name="azure-active-directory-tls-10-tls-11-and-3des-deprecation"></a>Azure Active Directory TLS 1,0, TLS 1,1 ve 3DES kullanımdan kaldırıldı

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Tüm Azure AD uygulamaları  
**Ürün yeteneği:** Standartlarına

Azure Active Directory 30 Haziran 2021 ' ye kadar dünya çapında Azure Active Directory bölgelerinde aşağıdaki protokolleri kullanımdan kaldırır:

- TLS 1.0
- TLS 1.1
- 3DES şifre paketi (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Etkilenen ortamlar:
- Azure ticari bulut
- Office 365 GCC ve WW

İlgili duyuru tüm istemci-sunucu ve tarayıcı-sunucu birleşimleri, Azure, Office 365 ve Microsoft 365 hizmetleri için Azure Active Directory güvenli bir bağlantı sağlamak üzere TLS 1,2 ve modern şifre paketleri kullanmalıdır. Bu değişiklik [Azure ACTIVE DIRECTORY TLS 1,0 & 1,1 ve 3Des şifre paketi US gov bulutuna kullanımdan kalkmasıyla](whats-new.md#azure-active-directory-tls-10-tls-11-and-3des-deprecation-in-us-gov-cloud)ilgilidir.

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---november-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Kasım 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2020 Kasım ayında, federasyon desteğiyle App Gallery 'de aşağıdaki 52 yeni uygulamaları ekledik:

[Seyahat & gider yönetimi](https://app.expenseonce.com/Account/Login), [tribeloo](../saas-apps/tribeloo-tutorial.md), [itslearning dosya Seçicisi](https://pmteam.itslearning.com/), [crit denetimi](../saas-apps/crises-control-tutorial.md), [courtalert](https://www.courtalert.com/), [stealthmail](https://stealthmail.com/), [edmentum-araştırma Adası](https://app.studyisland.com/cfw/login/), [sanal risk yöneticisi](../saas-apps/virtual-risk-manager-tutorial.md), [timu](../saas-apps/timu-tutorial.md), [Looker analiz platformu](../saas-apps/looker-analytics-platform-tutorial.md) [talview-stait](https://recruit.talview.com/login), gerçek zamanlı çevirici, [Klaxoon](https://access.klaxoon.com/login), [Podbean](../saas-apps/podbean-tutorial.md), [zcal](https://zcal.co/signup), [expensemanager](https://api.expense-manager.com/), [netmini yönetim kurumsal](../saas-apps/netsparker-enterprise-tutorial.md), [en-Trak kiracı deneyimi platformu](https://portal.en-trak.app/), [Appian](../saas-apps/appian-tutorial.md), [Panorays](../saas-apps/panorays-tutorial.md), [builterra](https://portal.builterra.com/), [filiz iade](https://my.evacheckin.com/organization)etme, [hownow WebApp SSO](../saas-apps/hownow-webapp-sso-tutorial.md), VISE Identity, [lucıd (tüm ürünler)](../saas-apps/lucid-tutorial.md), [Gobright](https://portal.brightbooking.eu/) [,](../saas-apps/coupa-risk-assess-tutorial.md) [sailpoint kimnow](../saas-apps/sailpoint-identitynow-tutorial.md),[Kaynak Merkezi](../saas-apps/resource-central-tutorial.md), [UiPathStudioO365App](https://www.uipath.com/product/platform), [jetodoks](../saas-apps/jedox-tutorial.md), [cequence uygulama güvenliği](../saas-apps/cequence-application-security-tutorial.md), [PerimeterX](../saas-apps/perimeterx-tutorial.md), [Trendminer](../saas-apps/trendminer-tutorial.md), [lexiyon](../saas-apps/lexion-tutorial.md), [workware](../saas-apps/workware-tutorial.md), [prodpad](../saas-apps/prodpad-tutorial.md), [AWS clientvpn](../saas-apps/aws-clientvpn-tutorial.md), [AppSec Flow SSO](../saas-apps/appsec-flow-sso-tutorial.md), [luum](../saas-apps/luum-tutorial.md), [navlun ölçümü](https://www.gpcsl.com/freight.html), [terrayform bulut](../saas-apps/terraform-cloud-tutorial.md) [,](https://gradle.com/) [doğa Research](../saas-apps/nature-research-tutorial.md), [Play Digital tabege](https://login.playsignage.com/login), [remotepc](../saas-apps/remotepc-tutorial.md), [prolorus](../saas-apps/prolorus-tutorial.md), [Hirebridge ATS](../saas-apps/hirebridge-ats-tutorial.md), [teamgage](https://www.teamgage.com/Account/ExternalLoginAzure), [Roadmunk](../saas-apps/roadmunk-tutorial.md), [Sunrise Software Relations CRM](https://cloud.relations-crm.com/), [Procaire](../saas-apps/procaire-tutorial.md), [®](https://www.edriving.com/)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz. https://aka.ms/AppsTutorial

Uygulamanızı Azure AD uygulama galerisinde listelemek için buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest

---

### <a name="public-preview---custom-roles-for-enterprise-apps"></a>Genel Önizleme-kurumsal uygulamalar için özel roller

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control
 
 [Temsilci kurumsal uygulama yönetimi Için özel RBAC rolleri](../roles/custom-available-permissions.md) artık genel önizlemede. Bu yeni izinler, uygulama kayıt yönetimi için özel rollerde derleme yapar ve bu, yöneticilerinize erişim için ayrıntılı denetim sağlar. Zaman içinde, Azure AD 'ye temsilci yönetiminin ek izinleri yayımlanacak.

Bazı yaygın temsili senaryolar:
- SAML tabanlı çoklu oturum açma uygulamalarına erişebilen kullanıcı ve grupların atanması
- Azure AD Galeri uygulamalarının oluşturulması
- SAML tabanlı çoklu oturum açma uygulamaları için temel SAML yapılandırmalarının güncelleştirilmesi ve okunması
- SAML tabanlı çoklu oturum açma uygulamaları için imza sertifikalarının yönetimi
- SAML tabanlı çoklu oturum açma uygulamaları için sertifikaların süresi dolan oturum açma bildirimi e-posta adresleri güncelleştirmesi
- SAML tabanlı çoklu oturum açma uygulamaları için SAML belirteci imzasını ve oturum açma algoritmasını güncelleştirme
- SAML tabanlı çoklu oturum açma uygulamaları için Kullanıcı özniteliklerini ve taleplerini oluşturma, silme ve güncelleştirme
- sağlama işlerini açma, kapatma ve yeniden başlatma özelliği
- öznitelik eşleme güncelleştirmeleri
- nesneyle ilişkili sağlama ayarlarını okuma özelliği
- hizmet sorumlusuyla ilişkili sağlama ayarlarını okuma özelliği
- sağlama için uygulama erişimini yetkilendirebilme

---

### <a name="public-preview---azure-ad-application-proxy-natively-supports-single-sign-on-access-to-applications-that-use-headers-for-authentication"></a>Genel Önizleme-Azure AD Uygulama Ara Sunucusu, kimlik doğrulaması için üst bilgiler kullanan uygulamalara çoklu oturum açma erişimini yerel olarak destekler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control
 
Azure Active Directory (Azure AD) uygulama proxy 'Si, kimlik doğrulaması için üst bilgileri kullanan uygulamalara çoklu oturum açma erişimini yerel olarak destekler. Azure AD 'de uygulamanız için gereken üst bilgi değerlerini yapılandırabilirsiniz. Üst bilgi değerleri uygulama proxy 'Si aracılığıyla uygulamaya gönderilir. Daha fazla bilgi edinmek için bkz. [Azure AD uygulaması proxy ile şirket içi uygulamalar Için üst bilgi tabanlı çoklu oturum açma](../manage-apps/application-proxy-configure-single-sign-on-with-headers.md)
 
---

### <a name="general-availability---azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy"></a>Genel kullanılabilirlik-Azure AD B2C özel Ilke kullanarak telefon kayıt ve oturum açma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Telefon numarası kaydolma ve oturum açma ile, geliştiriciler ve kuruluşlar müşterilerinin SMS aracılığıyla kullanıcının telefon numarasına gönderilen tek seferlik bir parola kullanarak kaydolmasına ve oturum açmalarına izin verebilir. Bu özellik ayrıca müşterinin telefonlarına erişimi Kayıplarsa telefon numaralarını değiştirmesine de olanak tanır. Özel ilkelerin gücüyle, geliştiricilerin ve kuruluşların markalarını sayfa özelleştirmesine iletişim kurmasına izin verin. [Azure AD B2C 'de özel ilkelerle telefon kaydı ve oturum açma ayarlamayı](../../active-directory-b2c/phone-authentication.md)öğrenin.
 
---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---november-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Kasım 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Adobe Kimlik Yönetimi](../saas-apps/adobe-identity-management-provisioning-tutorial.md)
- [Blogin](../saas-apps/blogin-provisioning-tutorial.md)
- [Clarizen One](../saas-apps/clarizen-one-provisioning-tutorial.md)
- [Contentful](../saas-apps/contentful-provisioning-tutorial.md)
- [GitHub AE](../saas-apps/github-ae-provisioning-tutorial.md)
- [Playvox](../saas-apps/playvox-provisioning-tutorial.md)
- [PrinterLogic SaaS](../saas-apps/printer-logic-saas-provisioning-tutorial.md)
- [Tic-Tac mobil](../saas-apps/tic-tac-mobile-provisioning-tutorial.md)
- [Visibly](../saas-apps/visibly-provisioning-tutorial.md)

Daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](../app-provisioning/user-provisioning.md).
 
---

### <a name="public-preview---email-sign-in-with-proxyaddresses-now-deployable-via-staged-rollout"></a>Genel Önizleme-ProxyAddresses ile e-posta Sign-In artık hazırlanan dağıtım aracılığıyla dağıtılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kiracı yöneticileri artık, ProxyAddresses ile belirli Azure AD gruplarına e-posta Sign-In dağıtmak için hazırlanan dağıtımı kullanabilir. Bu, özelliği, giriş bölgesi bulma ilkesi aracılığıyla kiracı tamamına dağıtmaya başlamadan önce yapmaya çalışırken yardımcı olabilir. Hazırlanmış dağıtım aracılığıyla ProxyAddresses ile e-posta Sign-In dağıtmaya yönelik yönergeler [belgelerde](../authentication/howto-authentication-use-email-signin.md)bulunur.
 
---

### <a name="limited-preview---sign-in-diagnostic"></a>Sınırlı Önizleme-oturum açma tanılaması

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & raporlamayı izleme
 
Oturum açma tanılamasının ilk önizleme sürümü ile yöneticiler artık Kullanıcı oturum açma işlemlerini gözden geçirebilir. Yöneticiler bağlamsal, özel ve ilgili ayrıntıları ve oturum açma sırasında neler olduğunu ve sorunların nasıl düzeltileceğini gösteren kılavuzları alabilir. Tanılama, hem Azure AD düzeyinde hem de koşullu erişim 'i Tanıla ve çöz Blade ' de kullanılabilir. Bu sürümde ele alınan tanılama senaryoları, koşullu erişim, Multi-Factor Authentication ve başarılı oturum açma Erişimlerdir.

Daha fazla bilgi için bkz. [Azure AD 'de oturum açma tanılaması nedir?](../reports-monitoring/overview-sign-in-diagnostics.md).
 
---

### <a name="improved-unfamiliar-sign-in-properties"></a>Geliştirilmiş tanınmayan oturum açma özellikleri

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

  Bilinmeyen oturum açma özellikleri algılamaları güncelleştirildi. Müşteriler daha yüksek riskli, bilinmeyen oturum açma özellikleri algılamalarını fark edebilir. Daha fazla bilgi için bkz. [risk nedir?](../identity-protection/concept-identity-protection-risks.md)
 
---

### <a name="public-preview-refresh-of-cloud-provisioning-agent-now-available-version-112810"></a>Bulut sağlama aracısının genel önizleme yenilemesi kullanıma sunuldu (sürüm: 1.1.281.0)

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD bulutu sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Bulut sağlama Aracısı genel önizlemede yayımlanmıştır ve artık Portal üzerinden kullanılabilir. Bu sürümde, etki alanlarınız için GMSA desteği, daha iyi güvenlik, iyileştirilmiş ilk eşitleme döngüleri ve büyük gruplar için destek sağlayan çeşitli geliştirmeler bulunur. Daha fazla ayrıntı için yayın sürümü [geçmişine](../app-provisioning/provisioning-agent-release-version-history.md) göz atın. 
 
---

### <a name="bitlocker-recovery-key-api-endpoint-now-under-informationprotection"></a>BitLocker kurtarma anahtarı API uç noktası şu anda/ınformationprotection altında

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Cihaz erişim yönetimi  
**Ürün yeteneği:** Cihaz yaşam döngüsü yönetimi
 
Daha önce,/BitLocker uç noktası aracılığıyla BitLocker anahtarlarını kurtarabilirsiniz. Bu uç noktayı kullanımdan kaldıracağız ve müşteriler artık/ınformationprotectionaltında olan API 'YI kullanmaya başlamalıdır. 

Bu değişiklikleri yansıtmak için belgelerdeki güncelleştirmeler için bkz. [BitLocker kurtarma API 'si](/graph/api/resources/bitlockerrecoverykey?view=graph-rest-beta) .

---

### <a name="general-availability-of-application-proxy-support-for-remote-desktop-services-html5-web-client"></a>Uzak Masaüstü Hizmetleri HTML5 Web Istemcisi için uygulama ara sunucusu desteğinin genel kullanılabilirliği

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control
 
Uzak Masaüstü Hizmetleri (RDS) Web Istemcisi için Azure AD Uygulama Ara Sunucusu desteği genel kullanıma sunuldu. RDS Web istemcisi, kullanıcıların Microsoft Edge, Internet Explorer 11, Google Chrome gibi HTLM5 özellikli bir tarayıcıdan uzak masaüstü altyapısına erişmesini sağlar. Kullanıcılar, uzak uygulamalarla veya masaüstü bilgisayarlardan her yerden yerel bir cihazla etkileşim kurabilir. 

Azure AD Uygulama Ara Sunucusu kullanarak, tüm zengin istemci uygulamaları için ön kimlik doğrulama ve koşullu erişim ilkelerini zorunlu tutarak RDS dağıtımınızın güvenliğini artırabilirsiniz. Daha fazla bilgi için bkz. [Uzak Masaüstü 'Nü Azure Ile yayımlama ad uygulama ara sunucusu](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
 
---

### <a name="new-enhanced-dynamic-group-service-is-in-public-preview"></a>Yeni gelişmiş dinamik grup hizmeti genel önizlemede

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş
 
Gelişmiş dinamik grup hizmeti artık genel önizlemede. Kiracılarında dinamik gruplar oluşturan yeni müşteriler yeni hizmeti kullanacaktır. Dinamik bir grup oluşturmak için gereken süre, kiracının boyutu yerine oluşturulmakta olan grubun boyutuyla orantılıdır. Bu güncelleştirme, müşteriler daha küçük gruplar oluştururken büyük kiracıların performansını önemli ölçüde iyileştirir. 

Yeni hizmet aynı zamanda birkaç dakika içinde öznitelik değişiklikleri nedeniyle üye ekleme ve kaldırma işlemlerinin tamamlanmasını sağlar. Ayrıca, tek işleme arızaları kiracı işlemeyi engellemez. Dinamik grupları oluşturma hakkında daha fazla bilgi edinmek için [belgelerimize](../enterprise-users/groups-create-rule.md)bakın.
 
---
## <a name="october-2020"></a>Ekim 2020

### <a name="azure-ad-on-premises-hybrid-agents-impacted-by-azure-tls-certificate-changes"></a>Azure TLS sertifika değişikliklerinden etkilenen Azure AD şirket Içi karma aracıları

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Platformunun

Microsoft, Azure hizmetlerini farklı bir kök sertifika yetkilisi (CA) kümesinden TLS sertifikaları kullanacak şekilde güncelleştiriyor. Bu güncelleştirme, geçerli CA sertifikalarının CA/tarayıcı Forum temel gereksinimlerinden biriyle uyumlu olmaması nedeniyle oluşur. Bu değişiklik, şirket içinde yüklü olan ve güvenilen bir kök sertifika listesi olan sıkı ortamları olan ve yeni sertifika verenler için güncelleştirilmeleri gereken Azure AD karma aracılarını etkiler.

Bu değişiklik, hemen işlem yapmazsanız hizmeti kesintiye neden olur. Bu aracılar, kullanıcılarınızın aynı parolaları kullanarak uygulamalara oturum açmasını sağlayan [geçiş kimlik doğrulama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) aracılarına ve Azure AD EŞITLEME 'ye ad gerçekleştiren [bulut sağlama önizleme](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) aracılarına izin veren [uygulama proxy bağlayıcıları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AppProxy) içerir. 

Yalnızca belirli bir sertifika Iptal listesi (CRL) indirmesi için giden çağrılara izin veren güvenlik duvarı kuralları ayarlanmış bir ortamınız varsa, aşağıdaki CRL ve OCSP URL 'Lerine izin vermeniz gerekir. Erişimi etkinleştirmek için değişiklik ve CRL ve OCSP URL 'Leri hakkında tam Ayrıntılar için bkz.  [Azure TLS sertifikası değişiklikleri](../../security/fundamentals/tls-certificate-changes.md).

---

### <a name="provisioning-events-will-be-removed-from-audit-logs-and-published-solely-to-provisioning-logs"></a>Sağlama olayları, denetim günlüklerinden kaldırılacak ve yalnızca sağlama günlüklerine yayımlanacak

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & raporlamayı izleme
 
SCıM [sağlama hizmeti](../app-provisioning/user-provisioning.md) tarafından gerçekleştirilen etkinlik hem denetim günlükleri hem de sağlama günlüklerinde günlüğe kaydedilir. Bu, ServiceNow içindeki bir kullanıcının oluşturulması, GSuite 'te grup veya AWS 'den bir rolün içeri aktarılması gibi etkinlikleri içerir. Gelecekte bu olaylar yalnızca sağlama günlüklerinde yayımlanacaktır. Günlüklerde yinelenen olayları önlemek için bu değişiklik uygulanıyor ve günlük Analizi 'nde Günlükler kullanan müşteriler tarafından tahakkuk eden ek maliyetler. 

Bir tarih tamamlandığında güncelleştirme sağlayacağız. Bu kullanımdan kaldırma, 2020 takvim yılı için planlanmıyor. 

> [!NOTE]
> Bu, sağlama hizmeti tarafından yayılan eşitleme olaylarının dışında denetim günlüklerinde olayları etkilemez. Bir uygulama oluşturma, koşullu erişim ilkesi, dizindeki bir Kullanıcı vb. gibi olaylar, denetim günlüklerinde yayınlanmeye devam edecektir. [Daha fazla bilgi edinin](../reports-monitoring/concept-provisioning-logs.md?context=azure%2factive-directory%2fapp-provisioning%2fcontext%2fapp-provisioning-context).
 

---

### <a name="azure-ad-on-premises-hybrid-agents-impacted-by-azure-transport-layer-security-tls-certificate-changes"></a>Azure Aktarım Katmanı Güvenliği (TLS) sertifika değişikliklerinden etkilenen Azure AD şirket Içi karma aracıları

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Platformunun
 
Microsoft, Azure hizmetlerini farklı bir kök sertifika yetkilisi (CA) kümesinden TLS sertifikaları kullanacak şekilde güncelleştiriyor. CA/tarayıcı Forum temel gereksinimlerinden birini takip eden geçerli CA sertifikaları nedeniyle bir güncelleştirme olacaktır. Bu değişiklik, şirket içinde yüklü olan ve sabit bir kök sertifika listesi olan sıkı ortamları olan Azure AD karma aracılarını etkiler. Bu aracıların yeni sertifika verenler ile güveneceği şekilde güncellenmesi gerekir.

Bu değişiklik, hemen işlem yapmazsanız hizmeti kesintiye neden olur. Bu aracılar şunları içerir: 
- Şirket içi uzaktan erişim için [uygulama proxy bağlayıcıları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AppProxy) 
- Kullanıcılarınızın aynı parolaları kullanarak uygulamalarda oturum açmalarına olanak tanıyan [geçiş kimlik doğrulama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) aracıları
- Azure AD eşitleme 'ye AD veren [bulut sağlama önizleme](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect) aracıları. 

Yalnızca belirli bir sertifika Iptal listesi (CRL) indirmesi için giden çağrılara izin veren güvenlik duvarı kuralları ayarlanmış bir ortamınız varsa, CRL ve OCSP URL 'Lerine izin vermeniz gerekir. Erişimi etkinleştirmek için değişiklik ve CRL ve OCSP URL 'Leri hakkında tam Ayrıntılar için bkz.  [Azure TLS sertifikası değişiklikleri](../../security/fundamentals/tls-certificate-changes.md).
 
---

### <a name="azure-active-directory-tls-10-tls-11-and-3des-deprecation-in-us-gov-cloud"></a>Azure Active Directory TLS 1,0, TLS 1,1 ve 3DES US Gov bulutta kullanımdan kaldırıldı

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Tüm Azure AD uygulamaları  
**Ürün yeteneği:** Standartlarına
 
Azure Active Directory, aşağıdaki protokolleri 31 Mart 2021 tarihine kadar kullanımdan kaldırır:
- TLS 1.0
- TLS 1.1
- 3DES şifre paketi (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Tüm istemci-sunucu ve tarayıcı-sunucu birleşimleri, Azure, Office 365 ve Microsoft 365 hizmetleri için Azure Active Directory güvenli bir bağlantı sağlamak üzere TLS 1,2 ve modern şifre paketleri kullanmalıdır.

Etkilenen ortamlar:
- Azure US Gov
- [Office 365 GCC High & DoD](/microsoft-365/compliance/tls-1-2-in-office-365-gcc)
 
---

### <a name="assign-applications-to-roles-on-au-and-object-scope"></a>AU ve nesne kapsamındaki rollere uygulama atama

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control
 
Bu özellik, yönetim birimi kapsamındaki bir yönetici rolüne uygulama (SPN) atama yeteneği sağlar. Daha fazla bilgi edinmek için [yönetim birimine kapsamlı roller atama](../roles/admin-units-assign-roles.md)bölümüne bakın.

---

### <a name="now-you-can-disable-and-delete-guest-users-when-theyre-denied-access-to-a-resource"></a>Artık, bir kaynağa erişim izni reddedildiğinde Konuk kullanıcıları devre dışı bırakabilir ve silebilirsiniz

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** Kimlik yönetimi
 
Kuruluşların gruplar ve uygulamalarda dış konukları daha iyi yönetmesine yardımcı olmak için Azure AD erişim Incelemelerinde devre dışı bırak ve Sil gelişmiş bir denetimdir. Bir erişim incelemesinin konukları engellenirse, **devre dışı bırak ve Sil** , bu otomatik olarak 30 gün boyunca oturum açmalarını engeller. 30 gün sonra kiracıdan tamamen kaldırılır.

Bu özellik hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleri (Önizleme) ile dış kimlikleri devre dışı bırakma ve silme](../governance/access-reviews-external-users.md#disable-and-delete-external-identities-with-azure-ad-access-reviews-preview).
 
---

### <a name="access-review-creators-can-add-custom-messages-in-emails-to-reviewers"></a>Erişim gözden geçirmesi oluşturucuları, e-postalara gözden geçirenlere özel iletiler ekleyebilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** Kimlik yönetimi
 
Azure AD erişim gözden geçirmeleriyle, İncelemeleri oluşturan Yöneticiler artık gözden geçirenlere özel bir ileti yazabilir. Gözden geçirenler, aldıkları e-postadaki iletiyi inceleyerek incelemeyi tamamlamasını ister. Bu özelliği kullanma hakkında daha fazla bilgi edinmek için, [bir veya daha fazla erişim Incelemesi oluşturma](../governance/create-access-review.md#create-one-or-more-access-reviews) bölümünün 14. adımına bakın.

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---october-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Ekim 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Apple İşletme Yönetimi](../saas-apps/apple-business-manager-provision-tutorial.md)
- [Apple School Manager](../saas-apps/apple-school-manager-provision-tutorial.md)
- [Code42](../saas-apps/code42-provisioning-tutorial.md)
- [AlertMedia](../saas-apps/alertmedia-provisioning-tutorial.md)
- [OpenText Directory Services](../saas-apps/open-text-directory-services-provisioning-tutorial.md)
- [Cinode](../saas-apps/cinode-provisioning-tutorial.md)
- [Global Relay Identity Sync](../saas-apps/global-relay-identity-sync-provisioning-tutorial.md)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](../app-provisioning/user-provisioning.md).
 
---

### <a name="integration-assistant-for-azure-ad-b2c"></a>Azure AD B2C için tümleştirme Yardımcısı

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Tümleştirme Yardımcısı (Önizleme) deneyimi artık Azure AD B2C Uygulama kayıtları için kullanılabilir. Bu deneyim, uygulamanızı yaygın senaryolar için yapılandırmaya kılavuzluk eder. [Microsoft Identity platform en iyi uygulamaları ve önerileri](../develop/identity-platform-integration-checklist.md)hakkında daha fazla bilgi edinin.
 
---

### <a name="view-role-template-id-in-azure-portal-ui"></a>Azure portal Kullanıcı arabirimindeki rol şablonu KIMLIĞINI görüntüleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure rolleri  
**Ürün yeteneği:** Access Control
 

Artık Azure portal her bir Azure AD rolünün şablon KIMLIĞINI görüntüleyebilirsiniz. Azure AD 'de seçili rolün  **açıklamasını** seçin. 

Müşterilerin, görünen ad yerine, PowerShell betiklerinde ve kodunda rol şablonu kimliklerini kullanması önerilir. Rol şablonu KIMLIĞI [Directoryroles](/graph/api/resources/directoryrole) ve [roledefinition](/graph/api/resources/unifiedroledefinition?view=graph-rest-beta) nesnelerinde kullanılmak üzere desteklenir. Rol şablonu kimlikleri hakkında daha fazla bilgi için bkz. [rol şablonu kimlikleri](../roles/permissions-reference.md#role-template-ids).

---

### <a name="api-connectors-for-azure-ad-b2c-sign-up-user-flows-is-now-in-public-preview"></a>Azure AD B2C kaydolma Kullanıcı akışları için API bağlayıcıları artık genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 

API bağlayıcıları artık Azure Active Directory B2C kullanılabilir. API bağlayıcıları, oturum açma Kullanıcı akışlarınızı özelleştirmek ve dış bulut sistemleriyle tümleştirilebilen Web API 'Lerini kullanmanıza imkan tanır. API bağlayıcılarını kullanarak şunları yapabilirsiniz:

- Özel onay iş akışlarıyla tümleştirin
- Kullanıcı giriş verilerini doğrulama
- Kullanıcı özniteliklerinin üzerine yaz 
- Özel iş mantığını Çalıştır 

 Daha fazla bilgi edinmek için bkz. [Kayıt belgelerini özelleştirmek ve genişletmek IÇIN API bağlayıcılarını kullanın](../../active-directory-b2c/api-connectors-overview.md) .

---

### <a name="state-property-for-connected-organizations-in-entitlement-management"></a>Yetkilendirme yönetiminde bağlı kuruluşlar için durum özelliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Dizin Yönetimi **ürün yeteneği:** Yetkilendirme Yönetimi
 

 Tüm bağlı kuruluşların artık "State" adlı ek bir özelliği olacaktır. Durum, bağlı kuruluşun "tüm yapılandırılmış bağlı kuruluşlara" başvuran ilkelerde nasıl kullanılacağını kontrol eder. Değer "yapılandırıldı" (kuruluşun "All" yan tümcesini kullanan ilkelerin kapsamında olduğu anlamına gelir) veya "Proposed" (kuruluşun kapsamda olmadığı anlamına gelir) olacaktır.  

El ile oluşturulan bağlı kuruluşların varsayılan ayarı "yapılandırılmış" olacaktır. Bu arada otomatik olarak oluşturulan (internet 'ten herhangi bir kullanıcının erişim istemesine izin veren ilkeler aracılığıyla oluşturulur) varsayılan olarak "önerilir" olarak yapılır.  9 2020 Eylül 'tan önce oluşturulan bağlı kuruluşlar "yapılandırıldı" olarak ayarlanır. Yöneticiler, gerektiğinde bu özelliği güncelleştirebilir. [Daha fazla bilgi edinin](../governance/entitlement-management-organization.md#managing-a-connected-organization-programmatically).
 

---

### <a name="azure-active-directory-external-identities-now-has-premium-advanced-security-settings-for-b2c"></a>Azure Active Directory dış kimlikler artık B2C için Premium gelişmiş güvenlik ayarlarına sahiptir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Kimlik korumasının risk tabanlı koşullu erişim ve risk algılama özellikleri artık [Azure AD B2C](../..//active-directory-b2c/conditional-access-identity-protection-overview.md)sunulmaktadır. Bu gelişmiş güvenlik özellikleriyle müşteriler artık şunları yapabilir:
- B2C uygulamaları ve son kullanıcı hesaplarıyla riskleri değerlendirmek için akıllı öngörülerden yararlanın. Algılamalar, genel seyahat, anonim IP adresleri, kötü amaçlı yazılım bağlantılı IP adresleri ve Azure AD tehdit bilgileri içerir. Portal ve API tabanlı raporlar da kullanılabilir.
- B2C kullanıcıları için uyarlamalı kimlik doğrulama ilkelerini yapılandırarak riskleri otomatik olarak çözün. Uygulama geliştiricileri ve Yöneticiler, çok faktörlü kimlik doğrulaması (MFA) gerektirerek veya Kullanıcı risk düzeyine bağlı olarak erişimi engelleyerek, konum, Grup ve uygulamaya dayalı ek denetimlerle, gerçek zamanlı riskleri azaltabilirler.
- Azure AD B2C Kullanıcı akışları ve özel ilkelerle tümleştirin. Koşullar Azure AD B2C yerleşik Kullanıcı akışlarından tetiklenebilir veya B2C özel ilkelerine eklenebilir. B2C Kullanıcı akışının diğer yönlerini de olduğu gibi, son kullanıcı deneyimi iletileri de özelleştirilebilir. Özelleştirme, kuruluşun ses, marka ve risk azaltma alternatiflerine göre belirlenir.
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---october-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ekim 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2020 Ekim 'de, federasyon desteğiyle App Gallery 'de aşağıdaki 27 yeni uygulamayı ekledik:

[Sentry](../saas-apps/sentry-tutorial.md), [bıfrelebee-üretkenlik superapp](https://app.yellowmessenger.com/user/login), [ABBYY FlexiCapture bulutu](../saas-apps/abbyy-flexicapture-cloud-tutorial.md), [Eabesteci](../saas-apps/eacomposer-tutorial.md), [Genesys, Azure Için bir Azure için bulut tümleştirmesi](https://apps.mypurecloud.com/msteams-integration/), [bölge teknolojileri portalı](https://portail.zonetechnologie.com/signin) [Beautiful.ai](../saas-apps/beautiful.ai-tutorial.md), [datawiza erişim Aracısı](https://console.datawiza.com/), [Zokri](https://app.zokri.com/), [checkkanıt](../saas-apps/checkproof-tutorial.md), [Ecochallenge.org](https://events.ecochallenge.org/users/login), [atışınsal](http://atspoke.com/login), [randevu anımsatıcısı](https://app.appointmentreminder.co.nz/account/login), [Cloud. Market](https://cloud.market/), [seyahatperk](../saas-apps/travelperk-tutorial.md) [, içiyilik,](https://app.greetly.com/) [orgvititesi SSO](../saas-apps/orgvitality-sso-tutorial.md), [Web Cargo hava](../saas-apps/web-cargo-air-tutorial.md), [döngü Flow CRM](../saas-apps/loop-flow-crm-tutorial.md), [starakılda](../saas-apps/starmind-tutorial.md), [workstem](https://hrm.workstem.com/login), [Perakende Zipline](../saas-apps/retail-zipline-tutorial.md), [hoxhunt](../saas-apps/hoxhunt-tutorial.md), [mevisio](../saas-apps/mevisio-tutorial.md), [Samsara](../saas-apps/samsara-tutorial.md), [Nimbus](../saas-apps/nimbus-tutorial.md), [Pulse Secure sanal Traffic Manager](../saas-apps/pulse-secure-virtual-traffic-manager-tutorial.md)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz. https://aka.ms/AppsTutorial

Uygulamanızı Azure AD uygulama galerisinde listelemek için buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest

---

### <a name="provisioning-logs-can-now-be-streamed-to-log-analytics"></a>Sağlama günlükleri artık Log Analytics 'e akışla eklenebilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & raporlamayı izleme
 

Şunları yapmak için sağlama günlüklerinizi Log Analytics 'e yayımlayın:
- 30 günden uzun süre için sağlama günlüklerini depolayın
- Özel uyarıları ve bildirimleri tanımlama
- Günlükleri görselleştirmek için panolar oluşturun
- Günlükleri çözümlemek için karmaşık sorguları yürütün 

Özelliği nasıl kullanacağınızı öğrenmek için bkz. [sağlamanın Azure izleyici günlükleriyle nasıl tümleştirildiğini anlama](../app-provisioning/application-provisioning-log-analytics.md).
 
---

### <a name="provisioning-logs-can-now-be-viewed-by-application-owners"></a>Sağlama günlükleri artık uygulama sahipleri tarafından görüntülenebilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & raporlamayı izleme
 
Artık uygulama sahiplerinin etkinlikleri sağlama hizmeti tarafından izlemesini sağlayabilir ve bunları ayrıcalıklı bir rol sağlamadan veya sorunu giderebilmeden sorunları giderebilirsiniz. [Daha fazla bilgi edinin](../reports-monitoring/concept-provisioning-logs.md).
 
---

### <a name="renaming-10-azure-active-directory-roles"></a>10 Azure Active Directory rollerini yeniden adlandırma

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure rolleri  
**Ürün yeteneği:** Access Control
 
Bazı Azure Active Directory (AD) yerleşik rollerinin, Microsoft 365 Yönetim Merkezi, Azure AD portalı ve Microsoft Graph gibi görünenlerden farklı adları vardır. Bu tutarsızlık otomatik işlemlerde sorunlara neden olabilir. Bu güncelleştirmeyle, 10 rol adını tutarlı hale getirmek için yeniden adlandırıyoruz. Aşağıdaki tabloda yeni rol adları verilmiştir:

![Yeni rol adları tablosu](media/whats-new/azure-role.png)

---

### <a name="azure-ad-b2c-support-for-auth-code-flow-for-spas-using-msal-js-2x"></a>MSAL JS 2. x kullanarak SPAs için auth Code Flow Azure AD B2C desteği

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
MSAL.js sürüm 2. x artık tek sayfalı Web Apps (maça 'Lar) için yetkilendirme kodu akışı desteğini içerir. Azure AD B2C artık Azure portal üzerinde SPA uygulama türünün kullanımını ve tek sayfalı uygulamalar için PKCE ile MSAL.js yetkilendirme kodu akışının kullanımını destekleyecektir. Bu, Azure AD B2C kullanan maça 'Ları, daha yeni tarayıcılarla ve ABIDE ile daha yeni kimlik doğrulama protokolü önerilerini kullanarak SSO korumasını sağlar. Azure Active Directory B2C öğreticide [tek sayfalı uygulama (Spa) kaydetme](../../active-directory-b2c/tutorial-register-spa.md) ile çalışmaya başlayın.

---

### <a name="updates-to-remember-multi-factor-authentication-mfa-on-a-trusted-device-setting"></a>Güvenilir bir cihaz ayarında Multi-Factor Authentication (MFA) anımsanacak güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 

Son zamanlarda, kimlik doğrulamasını 365 güne kadar genişletmek için güvenilir bir cihaz özelliğindeki [hatırla Multi-Factor Authentication (MFA)](../authentication/howto-mfa-mfasettings.md#remember-multi-factor-authentication) öğesini güncelleştirdik. Azure Active Directory (Azure AD) Premium lisansları, yeniden kimlik doğrulama ayarları için daha fazla esneklik sağlayan [koşullu erişim – oturum açma sıklığı ilkesini](../conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency) de kullanabilir.

En iyi kullanıcı deneyimi için, güvenilir cihazlarda, konumlarda veya düşük riskli oturumlarda oturum ömrünü uzatmak için güvenilen bir cihaz ayarında MFA hatırlama konusuna alternatif olarak, koşullu erişim oturum açma sıklığını kullanmanızı öneririz. Başlamak için, yeniden [kimlik doğrulama deneyimini iyileştirmek üzere en son kılavuzumuzu](../authentication/concepts-azure-multi-factor-authentication-prompts-session-lifetime.md)inceleyin.

---

## <a name="september-2020"></a>Eylül 2020

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---september-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Eylül 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [CODA](../saas-apps/coda-provisioning-tutorial.md)
- [Cofense Alıcı Eşitleme](../saas-apps/cofense-provision-tutorial.md)
- [InVision](../saas-apps/invision-provisioning-tutorial.md)
- [myday](../saas-apps/myday-provision-tutorial.md)
- [SAP Analytics Cloud](../saas-apps/sap-analytics-cloud-provisioning-tutorial.md)
- [Webroot güvenlik tanıma](../saas-apps/webroot-security-awareness-training-provisioning-tutorial.md)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](../app-provisioning/user-provisioning.md).
 
---
### <a name="cloud-provisioning-public-preview-refresh"></a>Bulut sağlama genel önizlemeyi yenileme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD bulut sağlama **ürün yeteneği:** kimlik yaşam döngüsü yönetimi
 
Azure AD Connect bulut sağlama genel önizleme yenileme özellikleri müşteri geri bildirimlerinden geliştirilen iki önemli geliştirmelere sahiptir: 

- Azure portal aracılığıyla öznitelik eşleme deneyimi

    Bu özellikle, BT yöneticileri Kullanıcı, Grup veya iletişim özniteliklerini AD 'den Azure AD 'ye eşleyerek bugün mevcut çeşitli eşleme türlerini kullanarak eşleyebilirsiniz. Öznitelik eşleme, Active Directory ' den Azure Active Directory akan özniteliklerin değerlerini standartlaştırarak kullanılan bir özelliktir. Bunlardan biri, AD 'den Azure AD 'ye ait olan öznitelik değerinin doğrudan eşlenip eşlenmeyeceğini belirleyebilir veya kullanıcıları sağlarken öznitelik değerlerini dönüştürmek için ifadeler kullanın. [Daha fazla bilgi edinin](../cloud-provisioning/how-to-attribute-mapping.md)

- İsteğe bağlı sağlama veya test Kullanıcı deneyimi

    Yapılandırmanızı ayarladıktan sonra, Kullanıcı dönüşümünün kapsamdaki tüm kullanıcılarınıza uygulamadan önce beklendiği gibi çalışıp çalışmadığını görmek için test etmek isteyebilirsiniz. İsteğe bağlı sağlama sayesinde, BT yöneticileri bir AD kullanıcısının ayırt edici adını (DN) girebilir ve beklenen şekilde eşitlenip eşitlenmediğini görebilirler. İsteğe bağlı sağlama, daha önce beklenen şekilde çalışan öznitelik eşleştirmelerinin sağlanması için harika bir yol sağlar. [Daha Fazla Bilgi](../cloud-provisioning/how-to-on-demand-provision.md)
 
---

### <a name="audited-bitlocker-recovery-in-azure-ad---public-preview"></a>Azure AD 'de denetlenen BitLocker kurtarma-genel önizleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Cihaz erişim yönetimi  
**Ürün yeteneği:** Cihaz yaşam döngüsü yönetimi
 
BT yöneticileri veya son kullanıcılar erişimi olan BitLocker kurtarma anahtarlarını okuduklarında, Azure Active Directory artık kurtarma anahtarına kimin eriştiğini yakalayan bir denetim günlüğü oluşturur. Aynı denetim, BitLocker anahtarının ilişkilendirildiği cihazın ayrıntılarını sağlar.

Son kullanıcılar [, kendi Kurtarma anahtarlarına Hesabım aracılığıyla erişebilirler](../user-help/my-account-portal-devices-page.md#view-a-bitlocker-key). BT yöneticileri, [Beta 'Daki BitLocker kurtarma anahtarı API 'si](/graph/api/resources/bitlockerrecoverykey?view=graph-rest-beta) veya Azure AD portalı aracılığıyla Kurtarma anahtarlarına erişebilir. Daha fazla bilgi için bkz. [Azure AD portalındaki BitLocker anahtarlarını görüntüleme veya kopyalama](../devices/device-management-azure-portal.md#view-or-copy-bitlocker-keys).

---

### <a name="teams-devices-administrator-built-in-role"></a>Ekip cihazları yönetici yerleşik rolü

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control
 
[Takımlar cihazları yönetici](../roles/permissions-reference.md#teams-devices-administrator) rolüne sahip kullanıcılar takımlar yönetim merkezinden [ekipten sertifikalı cihazları](https://www.microsoft.com/microsoft-365/microsoft-teams/across-devices/devices) yönetebilir. 

Bu rol, kullanıcının tüm cihazları tek bakışta görüntülemesine olanak tanır ve bu sayede cihazları arayabilir ve filtreleyebilirsiniz. Kullanıcı oturum açma hesabı ve cihazın marka ve modeli dahil olmak üzere her bir cihazın ayrıntılarını da denetleyebilir. Kullanıcı cihazdaki ayarları değiştirebilir ve yazılım sürümlerini güncelleştirebilir. Bu rol, ekip etkinliklerini denetlemek ve cihazın kalitesini çağırmak için izinler vermez.
 
---

### <a name="advanced-query-capabilities-for-directory-objects"></a>Dizin nesneleri için Gelişmiş sorgu özellikleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS grafiği  
**Ürün yeteneği:** Geliştirici deneyimi
 
Azure AD API 'Lerinde dizin nesneleri için sunulan tüm yeni sorgu özellikleri artık v 1.0 uç noktasında ve üretime hazır olarak sunulmaktadır. Geliştiriciler, Standart OData işleçlerini kullanarak Dizin nesnelerini ve ilgili bağlantıları sayabilir, arayabilir, filtreleyip sıralayabilir.

Daha fazla bilgi edinmek için [buradaki](https://aka.ms/BlogPostMezzoGA)belgelere bakın ve bu [kısa anketle](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_yN8EPoGo5OpR1hgmCp1XxUMENJRkNQTk5RQkpWTE44NEk2U0RIV0VZRy4u)de geri bildirim gönderebilirsiniz.
 
---

### <a name="public-preview-continuous-access-evaluation-for-tenants-who-configured-conditional-access-policies"></a>Genel Önizleme: koşullu erişim ilkelerini yapılandıran kiracılar için sürekli erişim değerlendirmesi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Artık sürekli erişim değerlendirmesi (CAE), koşullu erişim ilkelerine sahip Azure AD kiracılar için genel önizlemede kullanıma sunulmuştur. CAE ile kritik güvenlik olayları ve ilkeleri gerçek zamanlı olarak değerlendirilir. Buna hesap devre dışı bırakma, parola sıfırlama ve konum değişikliği dahildir. Daha fazla bilgi için bkz. [sürekli erişim değerlendirmesi](../conditional-access/concept-continuous-access-evaluation.md).

---

### <a name="public-preview-ask-users-requesting-an-access-package-additional-questions-to-improve-approval-decisions"></a>Genel Önizleme: kullanıcılardan bir erişim paketi talep etmesini isteyin onay kararlarını artırmak için ek sorular

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Yöneticiler artık bir erişim paketi isteyen kullanıcıların, Azure AD Yetkilendirme Yönetimi 'nin erişim portalından yalnızca iş gerekçesinin ötesinde ek sorulara yanıt yanıtlamasını gerektirebilir. Daha sonra bu kullanıcıların yanıtları daha doğru bir erişim onayı kararı vermek için onaylayanlara gösterilir. Daha fazla bilgi edinmek için bkz. [onay için ek istek sahibi bilgileri toplama (Önizleme)](../governance/entitlement-management-access-package-approval-policy.md#collect-additional-requestor-information-for-approval-preview).
 
---

### <a name="public-preview-enhanced-user-management"></a>Genel Önizleme: Gelişmiş Kullanıcı Yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanıcı Yönetimi  
**Ürün yeteneği:** Kullanıcı Yönetimi
 

Azure AD portalı, tüm kullanıcılar ve silinen kullanıcılar sayfalarındaki kullanıcıları bulmayı kolaylaştıracak şekilde güncelleştirilmiştir. Önizlemedeki değişiklikler şunları içerir: 
- Nesne KIMLIĞI, Dizin eşitleme durumu, oluşturma türü ve kimlik veren gibi daha görünür Kullanıcı özellikleri.
- Arama şimdi ad, e-posta ve nesne kimliklerinin birleştirilmiş aramasına izin veriyor.
- Kullanıcı türüne göre Gelişmiş filtreleme (üye, konuk ve yok), Dizin eşitleme durumu, oluşturma türü, şirket adı ve etki alanı adı.
- Ad, Kullanıcı asıl adı ve silinme tarihi gibi özelliklerde yeni sıralama özellikleri.
- Yeni bir Toplam Kullanıcı sayısı, tüm aramalarla veya filtrelerle güncelleştirilir.

Daha fazla bilgi için lütfen [Azure Active Directory Içindeki Kullanıcı yönetimi geliştirmeleri (Önizleme)](../enterprise-users/users-search-enhanced.md)bölümüne bakın.

---

### <a name="new-notes-field-for-enterprise-applications"></a>Kurumsal uygulamalar için yeni notlar alanı

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar **ürün yeteneği:** SSO

Kurumsal uygulamalara ücretsiz metin notları ekleyebilirsiniz. Kurumsal uygulamalar altındaki uygulamaları yönetici yapmanıza yardımcı olacak ilgili bilgileri ekleyebilirsiniz. Daha fazla bilgi için bkz. [hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdaki bir uygulamanın özelliklerini yapılandırma](../manage-apps/add-application-portal-configure.md). 

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---september-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Eylül 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Eylül 2020 ' de, federasyon desteğiyle App Gallery 'de aşağıdaki 34 yeni uygulamaları ekledik:

[VMware ufuk-birleştirilmiş erişim ağ geçidi](), [Pulse Secure PC](../saas-apps/vmware-horizon-unified-access-gateway-tutorial.md), [Inventory360](../saas-apps/pulse-secure-pcs-tutorial.md), [innetleştirilmiş](https://services.enteksystems.de/sso/microsoft/signup), [bookpencere](https://www.bookwidgets.com/sso/office365), [ZVD_Server](https://zaas.zenmutech.com/user/signin), [Iş için HashData](https://hashdata.app/login.xhtml), [securelogin](https://securelogin.securelogin.nu/sso/azure/login), [cybersolutions MAILBASEΣ/CMSS](../saas-apps/cybersolutions-mailbase-tutorial.md), [sibsolutions CYBERMAILΣ](../saas-apps/cybersolutions-cybermail-tutorial.md), [limbpacmms](https://auth.limblecmms.com/), [Glint Inc](../saas-apps/glint-inc-tutorial.md), MB [Yükseklik](../saas-apps/zeroheight-tutorial.md), [cinsiyet uygunluk](https://app.genderfitness.com/), [COEO portalı](https://my.coeo.com/), [Grammarly](../saas-apps/grammarly-tutorial.md), [fivetran](../saas-apps/fivetran-tutorial.md), [Kumolus](../saas-apps/kumolus-tutorial.md), [RSA dever Suite](../saas-apps/rsa-archer-suite-tutorial.md), [teamzbeceri](../saas-apps/teamzskill-tutorial.md), [raumfürraum](../saas-apps/raumfurraum-tutorial.md), [Saviynt](../saas-apps/saviynt-tutorial.md), [BIZMERLINHR](https://marketplace.bizmerlin.net/bmone/signup), [Mobil dolap](../saas-apps/mobile-locker-tutorial.md), [zengine](../saas-apps/zengine-tutorial.md), [CLOUDCADı](https://app.cloudcadi.com/login), [sıtem](https://simfonianalytics.com/accounts/microsoft/login/), [bir kimlik & erişim yönetimi](https://my.priva.com/), [Nitro Pro](https://www.gonitro.com/nps/product-details/downloads), [eventfinity](../saas-apps/eventfinity-tutorial.md), [fexa](../saas-apps/fexa-tutorial.md), [güvenli imzalama Enterprise Portal](https://www.securedsigning.com/aad/Auth/ExternalLogin/AdminPortal), [güvenli imzalama Enterprise Portal AAD kurulumu](https://www.securedsigning.com/aad/Auth/ExternalLogin/AdminPortal), [wınfc çevrimiçi](https://wisteconline.com/auth/oidc), [Oracle PeopleSoft tarafından korunan, F5 BIG-IP APM](../saas-apps/oracle-peoplesoft-protected-by-f5-big-ip-apm-tutorial.md)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz: https://aka.ms/AppsTutorial .

Uygulamanızı Azure AD uygulama galerisinde listelemek için buradaki ayrıntıları okuyun: https://aka.ms/AzureADAppRequest .

---

### <a name="new-delegation-role-in-azure-ad-entitlement-management-access-package-assignment-manager"></a>Azure AD Yetkilendirme Yönetimi 'nde yeni temsili rolü: paket atama Yöneticisi 'ne erişim

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Atamaları yönetmek için ayrıntılı izinler sağlamak üzere Azure AD Yetkilendirme Yönetimi 'ne yeni bir erişim paketi atama Yöneticisi rolü eklenmiştir. Artık bu roldeki bir kullanıcıya görevler temsilcisinden, bir erişim paketinin atama yönetimini iş sahibine devredebilir. Ancak, Access Package atama Yöneticisi, erişim paketi ilkelerini veya yöneticiler tarafından ayarlanan diğer özellikleri değiştiremez. 

Bu yeni rolle, atamaların yönetimini devretmek ve diğer tüm erişim paketi yapılandırmalarında yönetim denetimini sürdürmek için gereken en az ayrıcalıklardan yararlanabilirsiniz. Daha fazla bilgi için bkz. [Yetkilendirme Yönetimi rolleri](../governance/entitlement-management-delegate.md#entitlement-management-roles).
 
---

### <a name="changes-to-privileged-identity-managements-onboarding-flow"></a>Privileged Identity Management ekleme akışında yapılan değişiklikler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Daha önce, Privileged Identity Management (PıM) gerekli Kullanıcı onayını ve Azure AD MFA 'ya kayıt içeren PıM 'nin dikey penceresindeki bir ekleme akışını ekleme. Azure AD rolleri ve yöneticiler dikey penceresinde PıM deneyiminin son tümleştirmesi ile bu deneyimi kaldırdık. Geçerli P2 lisansına sahip herhangi bir kiracı, PıM 'ye otomatik olarak eklendi.

PıM 'ye ekleme, kiracınızda herhangi bir dolaysız olumsuz etkiye sahip değildir. Aşağıdaki değişiklikleri bekleyebilir:
- Etkin vs. gibi ek atama seçenekleri, PıM veya Azure AD rolleri ve yöneticiler dikey penceresinde atama yaptığınızda başlangıç ve bitiş zamanına uygundur. 
- Doğrudan atama deneyimine getirilen yönetim birimleri ve özel roller gibi ek kapsam mekanizmaları. 
- Genel yönetici veya ayrıcalıklı rol yöneticisiyseniz, PıM haftalık Özeti gibi birkaç ek e-posta almaya başlayabilirsiniz. 
- Ayrıca, rol atamasıyla ilgili denetim günlüğünde MS-PIM hizmet sorumlusu ' nı da görebilirsiniz. Bu beklenen değişikliğin normal iş akışınızı etkilememesi gerekir.

 Daha fazla bilgi için bkz. [Privileged Identity Management kullanmaya başlama](../privileged-identity-management/pim-getting-started.md).

---

### <a name="azure-ad-entitlement-management-the-select-pane-of-access-package-resources-now-shows-by-default-the-resources-currently-in-the-selected-catalog"></a>Azure AD Yetkilendirme Yönetimi: erişim paketi kaynaklarının seçme bölmesi artık varsayılan olarak seçili katalogdaki kaynakları gösterir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanıcı erişim yönetimi  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 

Erişim paketi oluşturma akışında, kaynak rolleri sekmesinde, seçim bölmesi davranışı değişiyor. Şu anda varsayılan davranış, seçilen kataloğa eklenen kullanıcının ve kaynakların sahip olduğu tüm kaynakları göstermek olur. 

Bu deneyim, kullanıcıların katalogdan kolayca kaynak seçebilmek için varsayılan olarak katalogda zaten eklenen kaynakları görüntüleyecek şekilde değiştirilecektir. Güncelleştirme, erişim paketlerine eklenecek kaynakların keşfediliğine yardımcı olur ve kataloğun parçası olmayan kullanıcıya ait kaynakları yanlışlıkla ekleme riskini azaltır. Daha fazla bilgi edinmek için bkz. [Azure AD yetkilendirme yönetiminde yeni bir erişim paketi oluşturma](../governance/entitlement-management-access-package-create.md#resource-roles).
 
---

## <a name="august-2020"></a>Ağustos 2020 
 
### <a name="updates-to-azure-multi-factor-authentication-server-firewall-requirements"></a>Azure Multi-Factor Authentication Sunucusu Güvenlik Duvarı gereksinimlerine yönelik güncelleştirmeler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
1 Ekim 2020 ' den itibaren Azure MFA sunucusu güvenlik duvarı gereksinimleri ek IP aralıkları gerektirecektir.

Kuruluşunuzda giden güvenlik duvarı kurallarınız varsa, MFA sunucularınızın tüm gerekli IP aralıklarıyla iletişim kurabilmesi için kuralları güncelleştirin. IP aralıkları [Azure Multi-Factor Authentication sunucusu güvenlik duvarı gereksinimleriyle](../authentication/howto-mfaserver-deploy.md#azure-multi-factor-authentication-server-firewall-requirements)belgelenmiştir.

---

### <a name="upcoming-changes-to-user-experience-in-identity-secure-score"></a>Kimlik güvenli puanı 'nda Kullanıcı deneyimine yaklaşan değişiklikler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik koruması **ürün yeteneği:** kimlik güvenliği & koruması

Microsoft güvenli puanınızın [yeni sürümünde](/microsoft-365/security/mtp/microsoft-secure-score-whats-new)tanıtılan değişikliklerle uyum sağlamak Için kimlik güvenli puan portalı 'nı güncelleştiriyoruz. 

Değişikliklerle birlikte önizleme sürümü, Eylül ayının başlangıcında kullanılabilir olacaktır. Önizleme sürümündeki değişiklikler şunları içerir:
- Microsoft güvenli puanı ile marka hizalaması için "kimlik güvenli puanı", "kimlik için güvenli skor" olarak yeniden adlandırıldı
- Standart ölçeğe normalleştirilmiş ve işaret yerine yüzde cinsinden bildirilen noktaları

Bu önizlemede, müşteriler mevcut deneyim ve yeni deneyim arasında geçiş yapabilir. Bu önizleme, Kasım 2020 sonuna kadar son olacak. Önizleme sonrasında, müşteriler otomatik olarak yeni UX deneyimine yönlendirilir.

---

### <a name="new-restricted-guest-access-permissions-in-azure-ad---public-preview"></a>Azure AD 'de yeni kısıtlı Konuk erişim Izinleri-genel önizleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Access Control   
**Ürün yeteneği:** Kullanıcı Yönetimi

Konuk kullanıcılar için dizin düzeyindeki izinleri güncelleştirdik. Bu izinler, yöneticilerin dış Konuk Kullanıcı erişimi üzerinde ek kısıtlamalar ve denetimler gerektirmesini sağlar. Yöneticiler artık dış konukların Kullanıcı ve grupların profiline ve üyelik bilgilerine erişimi için ek kısıtlamalar ekleyebilir. Bu genel önizleme özelliğiyle, müşteriler, konuk kullanıcıların içinde bulundukları grup üyeliklerini görmesini kısıtlama dahil olmak üzere grup üyeliklerini kararlayarak, dış Kullanıcı erişimini ölçekte yönetebilir.

Daha fazla bilgi için bkz. [kısıtlı Konuk erişim izinleri](../enterprise-users/users-restrict-guest-permissions.md) ve [Kullanıcıların varsayılan izinleri](./users-default-permissions.md).
 
---

### <a name="general-availability-of-delta-queries-for-service-principals"></a>Hizmet sorumluları için Delta sorgularının genel kullanılabilirliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS grafiği  
**Ürün yeteneği:** Geliştirici deneyimi
 
Microsoft Graph Delta sorgusu artık v 1.0 'da kaynak türünü destekliyor:
- Hizmet Sorumlusu

Artık istemciler bu kaynaklardaki değişiklikleri etkili bir şekilde izleyebilir ve değişiklikleri yerel bir veri deposuyla eşitlemeye yönelik en iyi çözümü sağlar. Bu kaynakları bir sorguda nasıl yapılandıracağınızı öğrenmek için bkz. [Microsoft Graph verilerdeki değişiklikleri izlemek için Delta sorgu kullanma](/graph/delta-query-overview).
 
---

### <a name="general-availability-of-delta-queries-for-oauth2permissiongrant"></a>OAuth2PermissionGrant için Delta sorgularının genel kullanılabilirliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS grafiği  
**Ürün yeteneği:** Geliştirici deneyimi

Microsoft Graph Delta sorgusu artık v 1.0 'da kaynak türünü destekliyor:
- OAuth2PermissionGrant

İstemciler artık bu kaynaklardaki değişiklikleri etkili bir şekilde izleyebilir ve değişiklikleri yerel bir veri deposuyla eşitlemeye yönelik en iyi çözümü sağlar. Bu kaynakları bir sorguda nasıl yapılandıracağınızı öğrenmek için bkz. [Microsoft Graph verilerdeki değişiklikleri izlemek için Delta sorgu kullanma](/graph/delta-query-overview).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---august-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ağustos 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Ağustos 2020 ' de, federasyon desteğiyle App Gallery 'de aşağıdaki 25 yeni uygulamayı ekledik:

[Backup365](https://portal.backup365.io/login), [Soapbox](https://app.soapboxhq.com/create?step=auth&provider=azure-ad2-oauth2), [alma SIS](https://almau.getalma.com/), [enlyft Dynamics 365 Bağlayıcısı](http://enlyft.com/), [serseview Space kullanımı yazılım çözümleri](../saas-apps/serraview-space-utilization-software-solutions-tutorial.md), [Uniq](https://web.uniq.app/), [Visi,](../saas-apps/visibly-tutorial.md) [Zylo](../saas-apps/zylo-tutorial.md), [edmentum-Courseware değerlendirmesi tam yolu](https://auth.edmentum.com/elf/login), [Sivlab](https://cyberlab.evolvesecurity.com/#/welcome), [Altamıra HRM](../saas-apps/altamira-hrm-tutorial.md), [kablotekler](../saas-apps/wirewheel-tutorial.md), [ZX uyumluluk ve yakalama](https://sminstall.zixcorp.com/teams/teams.php?install_request=true&tenant_id=common), Doğa, cıklü [kurumsal iş denetimleri platformu](../saas-apps/greenlight-enterprise-business-controls-platform-tutorial.md), [Genetec payını](https://www.clearance.network/), [ISAMs](../saas-apps/isams-tutorial.md), [verasmart](../saas-apps/verasmart-tutorial.md), [AMIKO](https://amiko.web.rivero.app/), [twingate](https://auth.twingate.com/signup), [huni Kiralama](https://nestiolistings.com/sso/oidc/azure/authorize/), [scalefyayılması](https://scalefusion.com/users/sign_in/), [bpanda](https://goto.bpanda.com/login), [vivun takvim bağlantısı](https://app.vivun.com/dashboard/calendar/connect), [Fortigate SSL VPN](../saas-apps/fortigate-ssl-vpn-tutorial.md), [wandera Son Kullanıcı](https://www.wandera.com/)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz. https://aka.ms/AppsTutorial

Uygulamanızı Azure AD uygulama galerisinde listelemek için buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest

---

### <a name="resource-forests-now-available-for-azure-ad-ds"></a>Kaynak ormanları artık Azure AD DS için kullanılabilir 

**Şunu yazın:** Yeni özellik **hizmeti kategorisi:** Azure AD Domain Services   
**Ürün yeteneği:** Azure AD Domain Services
 
Azure AD Domain Services içindeki kaynak ormanların özelliği artık genel kullanıma sunulmuştur. Artık, akıllı kart yetkilendirmesi de dahil olmak üzere Azure AD Domain Services kullanmak için Parola karması eşitlemesi olmadan yetkilendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi edinmek için bkz. [Azure Active Directory Domain Services (Önizleme) Için çoğaltma kümesi kavramları ve özellikleri](../../active-directory-domain-services/concepts-replica-sets.md).
 
---

### <a name="regional-replica-support-for-azure-ad-ds-managed-domains-now-available"></a>Azure AD DS yönetilen etki alanları için bölgesel çoğaltma desteği artık kullanılabilir

**Şunu yazın:** Yeni Özellik   
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services
 
Yönetilen bir etki alanını, Azure AD kiracısı başına birden fazla çoğaltma kümesine sahip olacak şekilde genişletebilirsiniz. Çoğaltma kümeleri, Azure AD Domain Services destekleyen herhangi bir Azure bölgesindeki eşlenen herhangi bir sanal ağa eklenebilir. Farklı Azure bölgelerindeki ek çoğaltma kümeleri, bir Azure bölgesi çevrimdışı kalırsa eski uygulamalar için coğrafi olağanüstü durum kurtarma sağlar. Daha fazla bilgi edinmek için bkz. [Azure Active Directory Domain Services (Önizleme) Için çoğaltma kümesi kavramları ve özellikleri](../../active-directory-domain-services/concepts-replica-sets.md).

---

### <a name="general-availability-of-azure-ad-my-sign-ins"></a>Azure AD My Sign-Ins genel kullanılabilirliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Azure AD My Sign-Ins, kurumsal kullanıcıların olağan dışı etkinlikleri denetlemek için oturum açma geçmişini gözden geçirmesine olanak sağlayan yeni bir özelliktir. Ayrıca, bu özellik, son kullanıcıların şüpheli etkinlikler üzerinde "Bu ben değilme" veya "Bu was Me" olarak raporistemelerine izin verir. Bu özelliği kullanma hakkında daha fazla bilgi edinmek için bkz. [My Sign-Ins sayfasından en son oturum açma etkinliğinizi görüntüleme ve arama](../user-help/my-account-portal-sign-ins-page.md#confirm-unusual-activity).
 
---

### <a name="sap-successfactors-hr-driven-user-provisioning-to-azure-ad-is-now-generally-available"></a>Azure AD 'de SAP başarılı bir şekilde yönetilen Kullanıcı hazırlama, genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Artık Azure AD ile SAP başarılı çarpanlarını yetkili kimlik kaynağı olarak tümleştirebilir ve Azure AD 'de hesapların sağlanması ve ön sağlamasını yapmak için yeni işe alımlar ve sonlandırmalar gibi HR olaylarını kullanarak uçtan uca kimlik yaşam döngüsünü otomatik hale getirebilirsiniz. 

SAP başarılı bir şekilde Azure AD 'nin sağlamasını yapılandırma hakkında daha fazla bilgi edinmek için bkz. [SAP başarılı faktörleri Active Directory için Kullanıcı sağlamayı yapılandırma](../saas-apps/sap-successfactors-inbound-provisioning-tutorial.md).
 
---

### <a name="custom-open-id-connect-ms-graph-api-support-for-azure-ad-b2c"></a>Özel açık KIMLIK Azure AD B2C için MS Graph API desteğini bağlama

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Daha önce özel açık KIMLIK bağlama sağlayıcıları yalnızca Azure portal aracılığıyla eklenebilir veya yönetilebilir. Azure AD B2C müşteriler artık Microsoft Graph API beta sürümü aracılığıyla bunları ekleyebilir ve yönetebilir. Bu kaynağı API 'lerle yapılandırma hakkında bilgi edinmek için bkz. [IdentityProvider kaynak türü](/graph/api/resources/identityprovider?view=graph-rest-beta).
 
---

### <a name="assign-azure-ad-built-in-roles-to-cloud-groups"></a>Azure AD yerleşik rollerini bulut gruplarına atama

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control

Artık bu yeni özelliği kullanarak, Azure AD yerleşik rollerini bulut gruplarına atayabilirsiniz. Örneğin, SharePoint Yöneticisi rolünü Contoso_SharePoint_Admins grubuna atayabilirsiniz. Ayrıca, bir gruba bir rolün uygun bir üyesini sağlamak için PıM 'yi kullanarak, bekleyen erişim izni verebilirsiniz. Bu özelliği nasıl yapılandıracağınızı öğrenmek için bkz. [Azure Active Directory (Önizleme) içinde rol atamalarını yönetmek için bulut gruplarını kullanma](../roles/groups-concept.md).
 
---

### <a name="insights-business-leader-built-in-role-now-available"></a>Öngörüler Iş lideri yerleşik rolü artık kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control
 
Öngörüler Iş lideri rolündeki kullanıcılar, [M365 Insights uygulaması](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics)aracılığıyla bir panolar ve Öngörüler kümesine erişebilir. Bu, tüm panolara ve sunulan Öngörüler ve veri araştırma işlevlerine tam erişim içerir. Ancak, bu roldeki kullanıcılar, Öngörüler Yöneticisi rolünün sorumluluğu olan ürün yapılandırma ayarlarına erişemez. Bu rol hakkında daha fazla bilgi için, bkz. [Azure Active Directory yönetici rolü izinleri](../roles/permissions-reference.md#insights-business-leader)
 
---

### <a name="insights-administrator-built-in-role-now-available"></a>Öngörüler Yöneticisi yerleşik rolü artık kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control
 
Öngörüler yönetici rolündeki kullanıcılar, [M365 Insights uygulamasındaki](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics)tüm yönetim özelliklerine erişebilir. Bu roldeki bir Kullanıcı dizin bilgilerini okuyabilir, hizmet durumunu izleyebilir, dosya destek biletlerini izleyebilir ve Öngörüler yönetici ayarları yönlerini erişebilir. Bu rol hakkında daha fazla bilgi için, bkz. [Azure Active Directory yönetici rolü izinleri](../roles/permissions-reference.md#insights-administrator)
 
--- 

### <a name="application-admin-and-cloud-application-admin-can-manage-extension-properties-of-applications"></a>Uygulama Yöneticisi ve bulut uygulaması Yöneticisi, uygulamaların uzantı özelliklerini yönetebilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD rolleri  
**Ürün yeteneği:** Access Control
 
Daha önce, [uzantı özelliğini](/graph/api/application-post-extensionproperty?view=graph-rest-beta&tabs=http)yalnızca genel yönetici yönetebilir. Artık uygulama Yöneticisi ve bulut uygulaması Yöneticisi için de bu özelliği etkinleştiriyoruz.
 
---

### <a name="mim-2016-sp2-hotfix-462630-and-connectors-1113010"></a>MıM 2016 SP2 düzeltme 4.6.263.0 ve bağlayıcılar 1.1.1301.0

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Microsoft Identity Manager (MıM) 2016 Service Pack 2 (SP2) için bir [düzeltme paketi paketi (Build 4.6.263.0)](https://support.microsoft.com/help/4576473/hotfix-rollup-package-build-4-6-263-0-is-available-for-microsoft-ident) kullanılabilir. Bu toplu paket MIM CM, MıM Eşitleme Yöneticisi ve PAM bileşenleri için güncelleştirmeleri içerir. Ayrıca, MıM genel bağlayıcıları derleme 1.1.1301.0, Graph Bağlayıcısı için güncelleştirmeleri içerir.

---
 


