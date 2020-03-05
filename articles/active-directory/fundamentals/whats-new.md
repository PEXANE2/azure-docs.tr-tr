---
title: Yenilikler Sürüm Notları - Azure Active Directory | Microsoft Docs
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
ms.date: 02/27/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5cf12c28dc04d0dbb8a680d45c8d8f5f5faa2d82
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274187"
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

## <a name="february-2020"></a>Şubat 2020
 
### <a name="identity-secure-score---mfa-improvement-action-updates"></a>Kimlik güvenli puanı-MFA geliştirme eylem güncelleştirmeleri

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
İşletmelerin, işletmelerle çalışan ilkeleri uygularken en fazla güvenliği güvence altına aldığından emin olmak için, çok faktörlü kimlik doğrulaması (MFA) etrafında ortalanmış üç geliştirme eylemini kaldırır ve iki ekleme işlemi yapılır.

Aşağıdaki geliştirme eylemleri kaldırılacak:

- MFA için tüm kullanıcıları kaydet
- Tüm kullanıcılar için MFA gerektirme
- Azure AD ayrıcalıklı rolleri için MFA gerektir

Aşağıdaki geliştirme eylemleri eklenecek:

- Tüm kullanıcıların, güvenli erişim için MFA 'yı tamambağlanabildiğinden emin olun
- Yönetim rolleri için MFA gerektir

Bu yeni geliştirme eylemleri, kullanıcılarınızın veya yöneticilerinin dizininiz genelinde MFA için kaydedilmesini ve kuruluşunuzun gereksinimlerine uygun olan doğru ilke kümesini oluşturmayı gerektirir. Ana amaç, tüm kullanıcılarınızın ve yöneticilerinin birden çok faktörle veya risk tabanlı kimlik doğrulama istemlerinde kimlik doğrulaması yapabilmesini sağlarken esneklik sağlamaktır. Bu, Microsoft 'un MFA için kullanıcıları ne zaman sınayacağına karar vermesine veya kapsamlı kararlar uygulayan birden çok ilkeye sahip olmasına izin veren güvenlik varsayılanlarını ayarlama biçimini alabilir. Bu geliştirme eylemi güncelleştirmelerinin bir parçası olarak, temel koruma ilkeleri artık Puanlama hesaplamalarına dahil olmayacaktır. [Microsoft güvenli puanına nelerin geldiği hakkında daha fazla bilgi edinin](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score-whats-coming?view=o365-worldwide).
 
---

### <a name="azure-ad-domain-services-sku-selection"></a>Azure AD Domain Services SKU seçimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services
 
Azure AD Domain Services müşterilerin, örnekleri için performans düzeylerini seçerken daha fazla esneklik istediğini belirten geri bildirimde bulunduk. 1 Şubat 2020 ' den itibaren, dinamik bir modelden (Azure AD, nesne sayısına göre performans ve fiyatlandırma katmanını belirler) bir kendi kendine seçim modeline geçtik. Artık müşteriler, ortamıyla eşleşen bir performans katmanını seçebilirler. Bu değişiklik Ayrıca, kaynak ormanları gibi yeni senaryoları ve günlük yedeklemeler gibi Premium özellikleri etkinleştirebileceğimizi de sağlar. Nesne sayısı tüm SKU 'Lar için artık sınırsız, ancak her katman için nesne sayısı önerileri sunmaya devam edeceğiz.

**Acil bir müşteri eylemi gerekli değildir.** Mevcut müşteriler için 1 Şubat 2020 ' de kullanılan dinamik katman, yeni varsayılan katmanı belirler. Bu değişikliğin sonucu olarak bir fiyatlandırma veya performans etkisi yoktur. Azure AD DS müşterilerinin, dizin boyutu ve iş yükü özellikleri değiştikçe performans gereksinimlerini değerlendirmesi gerekecektir. Hizmet katmanları arasında geçiş kesinti olmaması durumunda olmaya devam eder, ancak artık kendi dizin artışına göre müşterileri otomatik olarak yeni katmanlara taşıyamayacaktır. Ayrıca, hiçbir ücret artmayacak ve yeni fiyatlandırma geçerli faturalandırma modelimize göre hizalanacaktır. Daha fazla bilgi için bkz. [Azure AD DS SKU belgeleri](https://docs.microsoft.com/azure/active-directory-domain-services/administration-concepts#azure-ad-ds-skus) ve [Azure AD Domain Services fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory-ds/).


---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2020"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Şubat 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Şubat 2020 ' de, uygulama galerisine federasyon desteğiyle bu 31 yeni uygulamayı ekledik: 

[Iamıp patent platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/iamip-patent-platform-tutorial), [deneyim bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/experience-cloud-tutorial), [Azure için ns1 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/ns1-sso-azure-tutorial), [Barkcuda e-posta güvenlik HIZMETI](https://ess.barracudanetworks.com/sso/azure), [PAA raporlaması](https://myaba.co.uk/client-access/signin/auth/msad), [Azure ad veri Bağlayıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/beekeeper-azure-ad-data-connector-tutorial) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-online-portal-tutorial) [Korn Fraz değerlendirmesi](https://www.kornferry.com/solutions/kf-digital/kf-assess), [Verkada komutu](https://docs.microsoft.com/azure/active-directory/saas-apps/verkada-command-tutorial), [Splashtop](https://docs.microsoft.com/azure/active-directory/saas-apps/splashtop-tutorial), [syxsense](https://docs.microsoft.com/azure/active-directory/saas-apps/syxsense-tutorial), [EAB gezinmek](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-tutorial), [Yeni relik (sınırlı sürüm)](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial), [thulium](https://admin.thulium.com/login/instance), [bilet Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/ticketmanager-tutorial), [](https://docs.microsoft.com/azure/active-directory/saas-apps/bic-cloud-design-tutorial) [şablon Takımlar için Chooser](https://links.officeatwork.com/templatechooser-download-teams), [beesy](https://www.beesy.me/index.php/site/login), [sağlık destek sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/health-support-system-tutorial), [mural](https://app.mural.co/signup), [Hive](https://docs.microsoft.com/azure/active-directory/saas-apps/hive-tutorial), [LavaDo](https://appsource.microsoft.com/product/web-apps/lavaloon.lavado_standard?tab=Overview), [Wakelet](https://wakelet.com/login), [firmex VDR](https://docs.microsoft.com/azure/active-directory/saas-apps/firmex-vdr-tutorial), [öğretmenler ve okullara yönelik ThingLink](https://www.thinglink.com/), [Coda](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial), yeniden temellendirme [podapp](https://nearpod.com/signup/?oc=Microsoft&utm_campaign=Microsoft&utm_medium=site&utm_source=product), [Wedo](https://docs.microsoft.com/azure/active-directory/saas-apps/wedo-tutorial), [davetli](https://invitepeople.com/login), [çalışma masası-article Galaxy](https://docs.microsoft.com/azure/active-directory/saas-apps/reprints-desk-article-galaxy-tutorial), [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-tutorial)

 
Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---february-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Şubat 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Mixpanel](https://docs.microsoft.com/azure/active-directory/saas-apps/mixpanel-provisioning-tutorial)
- [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-provisioning-tutorial)
- [Azure Databricks](https://docs.microsoft.com/azure/active-directory/saas-apps/azure-databricks-scim-connector-provisioning-tutorial)
- [Genesys 'e göre Pureck](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-provisioning-tutorial)
- [Zapier](https://docs.microsoft.com/azure/active-directory/saas-apps/zapier-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="azure-ad-support-for-fido2-security-keys-in-hybrid-environments"></a>Karma ortamlarda FIDO2 güvenlik anahtarları için Azure AD desteği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Karma ortamlarda FIDO2 güvenlik anahtarlarına yönelik Azure AD desteğinin genel önizlemesini duyuruyoruz. Kullanıcılar, karma Azure AD 'ye katılmış Windows 10 cihazlarında oturum açmak ve şirket içi ve bulut kaynaklarında sorunsuz oturum açmak için FIDO2 güvenlik anahtarlarını kullanabilir. Azure AD 'ye katılmış cihazlarda FIDO2 desteği için genel önizlemeyi başlatdığımız için, karma ortamlar için destek, parolalarımızın en çok istenen özelliğidir. Biyometri ve genel/özel anahtar şifrelemesi gibi gelişmiş teknolojiler kullanılarak passwordless kimlik doğrulaması, güvenli hale getirmeye yönelik kolaylık ve kullanım kolaylığını sağlar. Bu genel önizleme ile, artık geleneksel Active Directory kaynaklarına erişmek için FIDO2 güvenlik anahtarları gibi modern kimlik doğrulamasını kullanabilirsiniz. Daha fazla bilgi için bkz. Şirket [içi kaynaklara SSO](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises)'ya gidin. 

Başlamak için, adım adım yönergeler için [kiracınız IÇIN FIDO2 güvenlik anahtarlarını etkinleştir](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key) ' i ziyaret edin. 

---
 
### <a name="the-new-my-account-experience-is-now-generally-available"></a>Yeni Hesabım deneyimi artık genel kullanıma sunuldu 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Profilim/Hesabım  
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Tüm Son Kullanıcı hesabı yönetimi ihtiyaçları için tek bir durdurma için Hesabım, artık genel kullanıma sunuldu! Son kullanıcılar bu yeni siteye URL aracılığıyla veya yeni uygulamalarım deneyiminin üst bilgisinde erişebilir. Tüm self servis özellikleri hakkında daha fazla bilgi edinmek için yeni deneyim [Hesap portalından genel bakış ' a](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview)

---
 
### <a name="my-account-site-url-updating-to-myaccountmicrosoftcom"></a>Hesabım sitesi URL 'SI myaccount.microsoft.com olarak güncelleştiriliyor

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Profilim/Hesabım  
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Yeni Hesabım Son Kullanıcı deneyimi, URL 'sini sonraki ay içinde https://myaccount.microsoft.com olarak güncelleştirilecektir. [Hesabım portalı yardımım](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview)'de son kullanıcılara sunduğu deneyim ve tüm hesap self servis özellikleri hakkında daha fazla bilgi edinin.

---
 
## <a name="january-2020"></a>Ocak 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>Yeni uygulamalarım portalı genel kullanıma sunuldu

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Kuruluşunuzu, genel kullanıma sunulan yeni uygulamalarım portalına yükseltin! Yeni Portal ve Koleksiyonlar hakkında daha fazla bilgi için [bkz. uygulamalarım portalındaki koleksiyonlar oluştur](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Azure AD 'deki çalışma alanları koleksiyonlara yeniden adlandırıldı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulamalarım   
**Ürün yeteneği:** Son Kullanıcı deneyimleri
 
Yöneticiler kullanıcıların uygulamalarını düzenlemek için yapılandırabilen çalışma alanları, artık koleksiyonlar olarak anılacaktır. [Uygulamalarım portalındaki koleksiyonları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections)sırasında bunları yapılandırma hakkında daha fazla bilgi edinin.

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Azure AD B2C telefon kayıt ve özel ilke kullanarak oturum açma (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Telefon numarası kaydolma ve oturum açma ile, geliştiriciler ve kuruluşlar müşterilerinin SMS aracılığıyla kullanıcının telefon numarasına gönderilen tek seferlik bir parola kullanarak kaydolmasına ve oturum açmalarına izin verebilir. Bu özellik ayrıca müşterinin telefonlarına erişimi Kayıplarsa telefon numaralarını değiştirmesine de olanak tanır. Özel ilkelerin gücünden biri olan telefon kaydı ve oturum açma, geliştiricilerin ve kuruluşların markalarını sayfa özelleştirmesiyle iletişim kurmasına olanak tanır. [Azure AD B2C 'de özel ilkelerle telefon kaydı ve oturum açma ayarlamayı](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication)öğrenin.
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Ocak 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler özel erişimi](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ocak 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2020 Ocak 'ta, uygulama galerisine federasyon desteğiyle bu 33 yeni uygulamalar ekledik: 

[Josa](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [fastly Edge bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Office 365 için](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access) [paibot Netlogistik](https://1030-review-develop-3zknud.netlogistik.com/), kemli [,](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial) [leavebot](https://leavebot.io/#home), [Datacamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [Tripactions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [smartwork](https://www.intumit.com/english/SmartWork.html), [Dotcom-Monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [SSOGEN-Azure AD SSO Gateway, Oracle E-Business Suite-EBS, PeopleSoft ve JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial) [barındırılan mycirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [yuhu özelliği yönetim platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [lumapps](https://sites.lumapps.com/login), [Upwork Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [smartdb for Microsoft ekipleri](http://teams.smartdb.jp/login/), [PRESSPAGE](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [Contractsafe SAML2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [maxdentitydeploy Manager yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [Helpshıft](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/), [Coreview](https://portal.coreview.com/), [Squelch Cloud Office365 bağlayıcı](https://laxmi.squelch.io/login), [pingflow kimlik doğrulaması](https://app-staging.pingview.io/), [printerlogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [sandwai](https://app.sandwai.com/), [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [assetsonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [akarı Sanal yardımcı](https://akari.io/akari-virtual-assistant/)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="two-new-identity-protection-detections"></a>İki yeni kimlik koruması algılamaları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Kimlik korumasına iki yeni oturum açma bağlantılı algılama türü ekledik: şüpheli gelen kutusu düzenleme kuralları ve olanaksız seyahat. Bu çevrimdışı algılamalar Microsoft Cloud App Security (MCAS) tarafından keşfedilir ve kimlik koruması sırasında kullanıcıyı ve oturum açma riskini etkiler. Bu algılamalar hakkında daha fazla bilgi için bkz. [oturum açma risk türleri](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk).
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Son değişiklik: URI parçaları, oturum açma yeniden yönlendirme aracılığıyla taşınmaz

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
8 Şubat 2020 ' den itibaren, bir kullanıcının oturum açması için login.microsoftonline.com 'e bir istek gönderildiğinde, hizmet isteğe boş bir parça ekler.  Bu, tarayıcının istekteki mevcut bir parçayı temizler olarak yeniden yönlendirme saldırılarına karşı bir sınıf önler. Hiçbir uygulamanın bu davranışa bağımlılığı olmamalıdır. Daha fazla bilgi için bkz. Microsoft Identity platform belgelerindeki [son değişiklikler](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020) .

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

Tanımlama bilgileri için güvenli bir varsayılan model modelinin parçası olarak, Chrome 80 tarayıcısı `SameSite` özniteliği olmadan tanımlama bilgilerine nasıl davrandığını değiştiriyor. `SameSite` özniteliğini belirtmeyen tüm tanımlama bilgileri, `SameSite=Lax`olarak ayarlanmış gibi değerlendirilir, bu da uygulamanızın bağlı olabileceği bazı etki alanları arası tanımlama bilgisi paylaşma senaryolarını engeller. Daha eski Chrome davranışını sürdürmek için `SameSite=None` özniteliğini kullanabilir ve ek bir `Secure` özniteliği ekleyebilirsiniz. bu nedenle, siteler arası tanımlama bilgilerine yalnızca HTTPS bağlantıları üzerinden erişilebilir. Chrome, 4 Şubat 2020 ' de bu değişikliği tamamlamaya zamanlandı.

Tüm geliştiricilerimizin bu kılavuzu kullanarak uygulamalarını test etmenizi öneririz:

- **Güvenli tanımlama bilgisi kullan** ayarının varsayılan değerini **Evet**olarak ayarlayın.

- **SameSite** özniteliği için varsayılan değeri **none**olarak ayarlayın.

- Daha fazla **güvenli**`SameSite` özniteliği ekleyin.

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

[Airtable](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [HootSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [Üyeler için mavi erişim (BAI)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [Bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [reslife Portal](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal çoklu oturum açma (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), [teamschamp](https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279), [mosyon](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial), [myaryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial), [Mave-posta](https://loginself1.bluemail.me/), [beyedle](https://teams-web.beedle.co/#/), [Visma](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial), [onedesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [Foko perakende](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [qpazarlar fikir & yenilik yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [netüslü Kullanıcı kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial), [ifkılow](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial), [claromentıı](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial) , [JISC öğrenci Voter kaydı](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial), [e4enable](https://portal.e4enable.com/)

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

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Geliştirici geri bildirimlerine yanıt olarak Azure AD Premium P2 aboneleri artık Microsoft Graph için yeni riskDetection API 'sini kullanarak Azure AD Kimlik Koruması riskli algılama verilerinde karmaşık sorgular gerçekleştirebilir. Mevcut [ıdentityriskevent](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta) API beta sürümü **10 Ocak 2020 '** den itibaren veri döndürmeyi durdurur. Kuruluşunuz ıdentityriskevent API 'sini kullanıyorsa, yeni riskDetection API 'sine geçiş yapmanız gerekir.

Yeni riskDetection API 'SI hakkında daha fazla bilgi için [risk algılama API 'si başvuru belgelerine](https://aka.ms/RiskDetectionsAPI)bakın.

---

### <a name="application-proxy-support-for-the-samesite-attribute-and-chrome-80"></a>SameSite özniteliği ve Chrome 80 için uygulama ara sunucusu desteği

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Chrome 80 tarayıcı sürümünden önceki birkaç hafta önce, uygulama proxy tanımlama bilgilerinin **SameSite** özniteliğini nasıl değerlendirdiğinin güncelleştirilmesini planlıyoruz. Chrome 80 ' nin yayınlanmasıyla, **SameSite** özniteliğini belirtmeyen tüm tanımlama bilgileri `SameSite=Lax`olarak ayarlanmış gibi kabul edilir.

Bu değişiklik nedeniyle olumsuz olumsuz etkileri önlemeye yardımcı olmak için uygulama proxy 'Si erişimi ve oturum tanımlama bilgilerini şu şekilde güncelleştiriyoruz:

- **Güvenli tanımlama bilgisi kullan** ayarı için varsayılan değer **Evet**olarak ayarlanıyor.

- **SameSite** özniteliği için varsayılan değer **none**olarak ayarlanıyor.

    >[!NOTE]
    > Uygulama proxy 'Si erişimi tanımlama bilgileri her zaman güvenli kanallar üzerinden iletilir. Bu değişiklikler yalnızca oturum tanımlama bilgileri için geçerlidir.

Uygulama proxy 'Si tanımlama bilgisi ayarları hakkında daha fazla bilgi için bkz. [Azure Active Directory ' de şirket içi uygulamalara erişmek Için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="app-registrations-legacy-and-converged-app-management-from-the-application-registration-portal-appsdevmicrosoftcom-will-no-longer-be-available"></a>Uygulama kayıt portalından (apps.dev.microsoft.com) Uygulama kayıtları (eski) ve yakınsama uygulama yönetimi artık kullanılamayacak

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Geliştirici deneyimi

Yakın gelecekte, Azure AD hesapları olan kullanıcılar artık uygulama kayıt portalı 'nı (apps.dev.microsoft.com) kullanarak Yakınsanan uygulamaları kaydedemez veya Uygulama kayıtları (eski) uygulamaları kaydedebilir ve yönetebilir Azure portal deneyim.

Yeni Uygulama kayıtları deneyimi hakkında daha fazla bilgi edinmek için [Azure Portal eğitim kılavuzundaki uygulama kayıtları](../develop/app-registrations-training-guide-for-app-registrations-legacy-users.md)bakın.

---

### <a name="users-are-no-longer-required-to-re-register-during-migration-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcı başına MFA 'dan koşullu erişim tabanlı MFA 'ya geçiş sırasında kullanıcıların artık yeniden kaydedilmesi gerekmez

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Kullanıcıların Kullanıcı başına Multi-Factor Authentication (MFA) için devre dışı bırakıldıklarında ve sonra koşullu erişim ilkesi aracılığıyla MFA için etkinleştirildiklerinde yeniden kaydolmaları gereken bilinen bir sorunu düzelttik.

Kullanıcıların yeniden kaydolmesini gerektirmek için, Azure AD portalındaki kullanıcının kimlik doğrulama yöntemlerinden **gereken MFA 'yı yeniden kaydet** seçeneğini belirleyebilirsiniz. Kullanıcı başına MFA 'dan koşullu erişim tabanlı MFA 'ya Kullanıcı geçirme hakkında daha fazla bilgi için bkz. [Kullanıcı BAŞıNA MFA 'Dan koşullu erişim tabanlı MFA 'ya Kullanıcı dönüştürme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted#convert-users-from-per-user-mfa-to-conditional-access-based-mfa).

---

### <a name="new-capabilities-to-transform-and-send-claims-in-your-saml-token"></a>SAML belirtecinizdeki talepleri dönüştürmek ve göndermek için yeni yetenekler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

SAML belirteçinizdeki talepleri özelleştirmenize ve göndermenize yardımcı olacak ek yetenekler ekledik. Bu yeni yetenekler şunlardır:

- Ek talep dönüştürme işlevleri, talepte göndereceğiniz değeri değiştirmenize yardımcı olur.

- Tek bir talebe birden çok dönüşüm uygulama özelliği.

- Kullanıcı türüne ve kullanıcının ait olduğu gruba göre talep kaynağını belirtme özelliği.

Bu yeni yetenekler hakkında daha fazla bilgi için, bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="new-my-sign-ins-page-for-end-users-in-azure-ad"></a>Azure AD 'de son kullanıcılar için yeni oturum açma görevlerim sayfası

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** & Raporlamayı izleme

Kuruluşunuzun kullanıcılarının olağandışı etkinlikleri denetlemek için en son oturum açma geçmişini görüntülemesine olanak sağlamak üzere yeni bir **oturum açma** sayfası (https://mysignins.microsoft.com)) ekledik. Bu yeni sayfa, kullanıcılarınızın şunları görmesini sağlar:

- Herkes parolasını tahmin etmeye çalışıyorsa.

- Bir saldırgan hesabında ve bu konumdan başarıyla oturum açtı.

- Saldırganın erişmeye çalıştığı uygulamalar.

Daha fazla bilgi için, bkz. [Kullanıcılar artık olağan dışı etkinlik bloguna ait oturum açma geçmişini denetleyebilir](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Users-can-now-check-their-sign-in-history-for-unusual-activity/ba-p/916066) .

---

### <a name="migration-of-azure-ad-domain-services-azure-ad-ds-from-classic-to-azure-resource-manager-virtual-networks"></a>Azure AD Domain Services (Azure AD DS) klasik 'ten Azure Resource Manager sanal ağlara geçirme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Klasik sanal ağlarda takılı olan müşterilerimiz için çok güzel bir haberimiz var! Artık, klasik bir sanal ağdan mevcut bir Kaynak Yöneticisi sanal ağa tek seferlik bir geçiş gerçekleştirebilirsiniz. Kaynak Yöneticisi sanal ağa taşıdıktan sonra, hassas parola ilkeleri, e-posta bildirimleri ve denetim günlükleri gibi ek ve yükseltilmiş özelliklerden yararlanabilirsiniz.

Daha fazla bilgi için bkz. [Klasik sanal ağ modelinden Kaynak Yöneticisi Için önizleme-geçirme Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/migrate-from-classic-vnet).

---

### <a name="updates-to-the-azure-ad-b2c-page-contract-layout"></a>Azure AD B2C sayfa sözleşmesi düzenine yönelik güncelleştirmeler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Azure AD B2C için sayfa sözleşmesinin sürüm 1.2.0 için bazı yeni değişiklikler ekledik. Bu güncelleştirilmiş sürümde, artık öğeleriniz için yükleme sırasını denetleyebilir ve bu da, stil sayfası (CSS) yüklenirken gerçekleşen titreşimi durdurmaya da yardımcı olabilir.

Sayfa sözleşmesinde yapılan değişikliklerin tam listesi için bkz. [sürüm değişiklik günlüğü](https://docs.microsoft.com/azure/active-directory-b2c/page-layout#120).

---

### <a name="update-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Yeni çalışma alanlarıyla birlikte uygulamalarım sayfasına güncelleştir (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** Access Control

Artık, kuruluşunuzun kullanıcılarının, uygulamaları bulmasını kolaylaştırmak için yeni çalışma alanları özelliğini kullanma dahil olmak üzere yepyeni uygulamalara görüntüleme ve bu deneyimle erişme biçimini özelleştirebilirsiniz. Yeni çalışma alanları işlevselliği, kuruluşunuzun kullanıcılarının zaten erişimi olan uygulamalar için bir filtre işlevi görür.

Yeni uygulamalarım deneyimini kullanıma alma ve çalışma alanları oluşturma hakkında daha fazla bilgi için bkz. [My Apps (Önizleme) portalında çalışma alanları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="support-for-the-monthly-active-user-based-billing-model-general-availability"></a>Aylık etkin kullanıcı tabanlı faturalandırma modeli için destek (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Azure AD B2C artık aylık etkin kullanıcıların (MAU) faturalandırmasını desteklemektedir. MAU faturalandırma, bir takvim ayı sırasında kimlik doğrulama etkinliği olan benzersiz kullanıcı sayısını temel alır. Mevcut müşteriler, bu yeni faturalandırma yöntemine dilediğiniz zaman geçebilir.

1 Kasım 2019 ' den itibaren, tüm yeni müşteriler otomatik olarak bu yöntem kullanılarak faturalandırılacaktır. Bu Faturalandırma yöntemi, müşterilerin maliyet avantajları ve ilerleyebilme aracılığıyla faydalanır.

Daha fazla bilgi için bkz. [aylık etkin kullanıcıları faturalama modeline yükseltme](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing#upgrade-to-monthly-active-users-billing-model).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ekim 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Ekim 'de, uygulama galerisine federasyon desteğiyle bu 35 yeni uygulamalar ekledik:

Ksıs [: mobil](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-mobile-tutorial), [Juno. yolculuğa](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial), [ExponentHR](https://docs.microsoft.com/azure/active-directory/saas-apps/exponenthr-tutorial), [kişi](https://tact.ai/assistant/), [opuscapita nakit yönetimi](http://cm1.opuscapita.com/tenantname), [salestim](https://prd.salestim.io/forms), [learnster](https://docs.microsoft.com/azure/active-directory/saas-apps/learnster-tutorial), [dynaTrace](https://docs.microsoft.com/azure/active-directory/saas-apps/dynatrace-tutorial), [hunchbuzz](https://login.hunchbuzz.com/integrations/azure/process), en uygun [çalışma](https://docs.microsoft.com/azure/active-directory/saas-apps/freshworks-tutorial), [ECORNELL](https://docs.microsoft.com/azure/active-directory/saas-apps/ecornell-tutorial), [Shiphazı](https://docs.microsoft.com/azure/active-directory/saas-apps/shiphazmat-tutorial), [NETÜSE Bulut güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial), [çekişme](https://docs.microsoft.com/azure/active-directory/saas-apps/contentful-tutorial), [bindtuning](https://bindtuning.com/login), [HireVue koordinatı – AB](https://www.hirevue.com/), [HireVue koordinatı-usonly](https://www.hirevue.com/), [HireVue koordinatı-US](https://www.hirevue.com/), [WittyParrot Bilgi kutusu](https://wittyapi.wittyparrot.com/wittyparrot/api/provision/trail/signup), [cloudmore](https://docs.microsoft.com/azure/active-directory/saas-apps/cloudmore-tutorial), [Visit.org](https://docs.microsoft.com/azure/active-directory/saas-apps/visitorg-tutorial), [cambium Xirrus EasyPass Portal](https://login.xirrus.com/azure-signup), [Paylocity](https://docs.microsoft.com/azure/active-directory/saas-apps/paylocity-tutorial), [posta şanslar!](https://docs.microsoft.com/azure/active-directory/saas-apps/mail-luck-tutorial), [Teamie](https://theteamie.com/), [takımlar için hız](https://velocity.peakup.org/teams/login), [sıgnl4](https://account.signl4.com/manage), [EAB gidilecek Impl](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-impl-tutorial), [ekran karşılamaları](https://console.screenmeet.com/), [Omega noktası](https://pi.ompnt.com/), konuşma için konuşma e [-postası (iPhone)](https://speaking.email/FAQ/98/email-access-via-microsoft-intune), [Office 365 Direct (iPhone/Android)](https://speaking.email/FAQ/126/email-access-via-microsoft-office-365-direct), [ExactCare SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/exactcare-sso-tutorial), [ıhealthhome bakım gezinti sistemi](https://ihealthnav.com/account/signin), [ Qubie](https://qubie.azurewebsites.net/static/adminTab/authorize.html)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="consolidated-security-menu-item-in-the-azure-ad-portal"></a>Azure AD portalındaki birleştirilmiş güvenlik menü öğesi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Artık, mevcut Azure AD güvenlik özelliklerinin tümüne yeni **güvenlik** menü öğesinden ve **arama** çubuğundan Azure Portal erişebilirsiniz. Ayrıca, **güvenlik-Başlarken**adlı yeni **güvenlik** giriş sayfası, ortak belgelerimize, güvenlik kılavuzumuza ve dağıtım kılavuzlarımıza bağlantılar sağlar.

Yeni **güvenlik** menüsü şunları içerir:

- Koşullu Erişim
- Kimlik Koruması
- Güvenlik Merkezi
- Kimlik güvenli puanı
- Kimlik doğrulama yöntemleri
- MFA
- Risk raporları-riskli kullanıcılar, riskli oturum açmalar, risk algılamaları
- ve daha fazlası...

Daha fazla bilgi için bkz. [güvenlik-Başlarken](https://portal.azure.com/#blade/Microsoft_AAD_IAM/SecurityMenuBlade/GettingStarted).

---

### <a name="office-365-groups-expiration-policy-enhanced-with-autorenewal"></a>Otomatik yenileme ile geliştirilmiş Office 365 grupları süre sonu ilkesi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Office 365 grupları süre sonu ilkesi, üyeleri tarafından etkin olarak kullanılan grupları otomatik olarak yenilemek üzere geliştirilmiştir. Gruplar, Outlook, SharePoint ve takımlar dahil olmak üzere tüm Office 365 uygulamaları genelinde Kullanıcı etkinliğine göre autorenewed olacaktır.

Bu geliştirme, Grup süre sonu bildirimlerinizi azaltmaya yardımcı olur ve etkin grupların kullanılabilir olmaya devam etmesine yardımcı olur. Office 365 gruplarınız için zaten etkin bir süre sonu ilkeniz varsa, bu yeni işlevselliği açmak için herhangi bir şey yapmanız gerekmez.

Daha fazla bilgi için bkz. [Office 365 grupları için süre sonu Ilkesini yapılandırma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-lifecycle).

---

### <a name="updated-azure-ad-domain-services-azure-ad-ds-creation-experience"></a>Azure AD Domain Services (Azure AD DS) oluşturma deneyimi güncelleştirildi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Azure AD Domain Services (Azure AD DS) yeni ve geliştirilmiş bir oluşturma deneyimi içerecek şekilde güncelleştirdik ve yalnızca üç tıklamayla yönetilen bir etki alanı oluşturmanıza yardımcı olur! Ayrıca, artık Azure AD DS bir şablondan karşıya yükleyebilir ve dağıtabilirsiniz.

Daha fazla bilgi için bkz. [öğretici: Azure Active Directory Domain Services örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance).

---

## <a name="september-2019"></a>Eylül 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Değişiklik planı: Power BI içerik paketlerinin kullanımdan kaldırılması

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

1 Ekim 2019 ' den başlayarak Power BI, Azure AD Power BI içerik paketi de dahil olmak üzere tüm içerik paketlerini kullanımdan kaldırmaya başlayacaktır. Bu içerik paketine alternatif olarak Azure AD çalışma kitaplarını kullanarak Azure AD ile ilgili hizmetleriniz hakkında öngörüler elde edebilirsiniz. Yalnızca rapor modundaki koşullu erişim ilkeleriyle ilgili çalışma kitapları, uygulama onayı tabanlı Öngörüler ve daha fazlası dahil olmak üzere ek çalışma kitapları geliyor.

Çalışma kitapları hakkında daha fazla bilgi için bkz. [Azure izleyici çalışma kitaplarını Azure Active Directory raporları için kullanma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks). İçerik paketlerinin kullanımdan kaldırılması hakkında daha fazla bilgi için bkz. [Power BI Template Apps genel kullanılabilirliği](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/) blog gönderisi.

---

### <a name="my-profile-is-renaming-and-integrating-with-the-microsoft-office-account-page"></a>Profilimi Microsoft Office hesabı sayfasıyla yeniden adlandırma ve Tümleştirme

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Profilim/Hesabım  
**Ürün yeteneği:** İş

Ekim 'den itibaren profilim deneyimi Hesabım olur. Bu değişikliğin bir parçası olarak, şu anda şöyle ki, **Profilim** **Hesabım**olarak değişecektir. Adlandırma değişikliğinin en üstünde ve bazı tasarım geliştirmelerinden, güncelleştirilmiş deneyim Microsoft Office hesabı sayfasıyla ek tümleştirme sunacak. Özellikle, **genel bakış hesabı** sayfasından Office yüklemelerine ve aboneliklerine, **Gizlilik** sayfasından Office ile ilgili iletişim tercihlerini de erişebileceksiniz.

Profilim (Önizleme) deneyimi hakkında daha fazla bilgi için bkz. [Profilim (Önizleme) portalına genel bakış](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview).

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>Azure AD portalındaki CSV dosyalarını kullanarak grupları ve üyeleri toplu olarak yönetme (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Azure AD portalındaki toplu Grup Yönetimi deneyimlerinin genel önizleme kullanılabilirliğine duyurmaktan mutluluk duyuyoruz. Artık aşağıdakiler dahil olmak üzere grupları ve üye listelerini yönetmek için bir CSV dosyası ve Azure AD portalını kullanabilirsiniz:

- Gruptan üye ekleme veya kaldırma.

- Dizindeki grupların listesi indiriliyor.

- Belirli bir grup için Grup üyeleri listesi indiriliyor.

Daha fazla bilgi için bkz. [toplu üye ekleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), [üyeleri toplu kaldırma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members), [üyeleri toplu indirme listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members)ve [Toplu indirme grupları listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download).

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>Dinamik izin artık yeni bir yönetici onay uç noktası aracılığıyla destekleniyor

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Microsoft Identity platformunda dinamik izin modelini kullanmak isteyen uygulamalar için yararlı olan dinamik izni desteklemek için yeni bir yönetici onay uç noktası oluşturduk.

Bu yeni uç noktanın nasıl kullanılacağı hakkında daha fazla bilgi için, bkz. [Yönetici onay uç noktası kullanma](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları mevcuttur-Eylül 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Eylül 2019 ' de, uygulama galerisine federasyon desteğiyle bu 29 yeni uygulamaları ekledik:

[Schedulelook](https://schedulelook.bbsonlineservices.net/), [MS Azure SSO erişimi, Ahidex uyumluluk ofisi™-çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/ms-azure-sso-access-for-ethidex-compliance-office-tutorial), [Iver portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/iserver-portal-tutorial), [ufuk sitesi](https://docs.microsoft.com/azure/active-directory/saas-apps/skysite-tutorial), [Concur seyahat ve harcama](https://docs.microsoft.com/azure/active-directory/saas-apps/concur-travel-and-expense-tutorial), [çalışma panosu](https://docs.microsoft.com/azure/active-directory/saas-apps/workboard-tutorial), [yeeflow](https://apps.yeeflow.com/), [yay olanakları](https://docs.microsoft.com/azure/active-directory/saas-apps/arc-facilities-tutorial), [Luware stra_s ekibi](https://stratus.emea.luware.cloud/login), [geniş fikirler](https://wideideas.online/wideideas/), [Prma bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [jdlt Client hub](https://clients.jdlt.co.uk/login), [renraku](https://docs.microsoft.com/azure/active-directory/saas-apps/renraku-tutorial), [sealpath Secure Browser](https://protection.sealpath.com/SealPathInterceptorWopiSaas/Open/InstallSealPathEditorOneDrive), [prma bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [penneo](https://app.penneo.com/), [hireven](https://app.testhtm.com/settings/email-integration), [iş](https://aec.cintoo.com/login)akışı [Beyaz kaynak](https://docs.microsoft.com/azure/active-directory/saas-apps/whitesource-tutorial), [barındırılan Herısat çevrimiçi SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-heritage-online-sso-tutorial), [IDC](https://docs.microsoft.com/azure/active-directory/saas-apps/idc-tutorial), [cakehr](https://docs.microsoft.com/azure/active-directory/saas-apps/cakehr-tutorial), [\,](https://docs.microsoft.com/azure/active-directory/saas-apps/bis-tutorial) [COO Kai ekip derlemesi](https://ms-contacts.coo-kai.jp/), [sonarqude](https://docs.microsoft.com/azure/active-directory/saas-apps/sonarqube-tutorial), [Adobe Identity Management](https://docs.microsoft.com/azure/active-directory/saas-apps/adobe-identity-management-tutorial), [Discovery avantajları SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/discovery-benefits-sso-tutorial), [Amelio](https://app.amelio.co/), [itask](https://itask.yipinapp.com/)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-azure-ad-global-reader-role"></a>Yeni Azure AD Genel okuyucu rolü

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control

24 Eylül 2019 ' den itibaren, genel okuyucu adlı yeni bir Azure Active Directory (AD) rolü kullanıma sunacağız. Bu piyasaya çıkma üretim ve küresel bulut müşterileri (GCC) ile başlar ve Ekim 'de dünya çapında bir işlem görür.

Genel okuyucu rolü, genel yöneticiye salt okunurdur. Bu roldeki kullanıcılar Microsoft 365 hizmetleri genelinde ayarları ve yönetim bilgilerini okuyabilir, ancak yönetim eylemleri alamaz. Kuruluşunuzdaki genel yönetici sayısını azaltmaya yardımcı olmak için genel okuyucu rolünü oluşturduk. Genel yönetici hesapları güçlü ve saldırılara karşı savunmasız olduğundan, beşten az genel yönetici olmasını öneririz. Planlama, denetim veya araştırmalar için genel okuyucu rolünü kullanmanızı öneririz. Ayrıca, küresel bir yönetici rolü gerekmeden çalışmanın yapılmasına yardımcı olması için Exchange Yöneticisi gibi diğer sınırlı yönetici rolleriyle birlikte genel okuyucu rolünü kullanmanızı öneririz.

Genel okuyucu rolü, yeni Microsoft 365 Yönetim Merkezi, Exchange Yönetim Merkezi, takımlar Yönetim Merkezi, Güvenlik Merkezi, Uyumluluk Merkezi, Azure AD Yönetim Merkezi ve cihaz yönetimi Yönetim Merkezi ile birlikte kullanılabilir.

>[!NOTE]
> Genel Önizleme başlangıcında, genel okuyucu rolü şu şekilde çalışmaz: SharePoint, Privileged Access Management, Müşteri Kasası, duyarlılık etiketleri, takımlar yaşam döngüsü, takımlar raporlama & Çağrı Analizi, takımlar IP telefon cihazı yönetimi ve takımlar uygulaması Kataloglarını. Bu hizmetlerin tümü, gelecekte rolle çalışmak üzere tasarlanmıştır.

Daha fazla bilgi için [Azure Active Directory Içindeki yönetici rolü izinleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)bölümüne bakın.

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama Ara Sunucusu kullanarak Power BI Mobil uygulamanızdan şirket içi rapor sunucusuna erişin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Power BI Mobile App ile Azure AD Uygulama Ara Sunucusu arasında yeni tümleştirme, Power BI mobil uygulamada güvenli bir şekilde oturum açmanızı ve şirket içi Power BI Rapor Sunucusu barındırılan kuruluşunuzun tüm raporlarını görüntülemenize olanak sağlar.

Uygulamanın indirileceği dahil Power BI Mobil uygulama hakkında daha fazla bilgi için [Power BI sitesine](https://powerbi.microsoft.com/mobile/)bakın. Power BI mobil uygulamayı Azure AD Uygulama Ara Sunucusu ile ayarlama hakkında daha fazla bilgi için bkz. [azure ad uygulama ara sunucusu ile Power BI mobil uzaktan erişimi etkinleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi).

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>AzureADPreview PowerShell modülünün yeni sürümü kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Dizinden

Yeni cmdlet 'ler, Azure AD 'de aşağıdakiler de dahil olmak üzere özel rolleri tanımlamaya ve atamaya yardımcı olmak için AzureADPreview modülüne eklenmiştir:

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>Azure AD Connect yeni sürümü

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Dizinden

Otomatik yükseltme müşterileri için Azure AD Connect güncelleştirilmiş bir sürümünü yayımladık. Bu yeni sürüm bazı yeni özellikler, geliştirmeler ve hata düzeltmeleri içerir. Bu yeni sürüm hakkında daha fazla bilgi için bkz. [Azure AD Connect: sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250).

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Azure Multi-Factor Authentication (MFA) sunucusu, sürüm 8.0.2 artık kullanılabilir

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması

MFA sunucusunu 1 Temmuz 2019 ' den önce etkinleştiren mevcut bir müşteriyseniz, artık MFA sunucusu 'nun en son sürümünü (sürüm 8.0.2) indirebilirsiniz. Bu yeni sürümde şunları yaptık:

- Bir sorun düzeltildi böylece Azure AD eşitleme bir kullanıcıyı devre dışı durumundan etkin olarak değiştirdiğinde kullanıcıya bir e-posta gönderilir.

- Etiketler işlevini kullanmaya devam ederken müşterilerin başarılı bir şekilde yükseltebilmesi için bir sorun düzeltildi.

- Kosovo (+ 383) ülke kodu eklendi.

- MultiFactorAuthSvc. log dosyasına bir kerelik atlama denetim günlüğü eklendi.

- Web hizmeti SDK 'sının performansı geliştirildi.

- Diğer küçük hatalar düzeltildi.

Microsoft, 1 Temmuz 2019 ' den itibaren Yeni dağıtımlar için MFA sunucusunu sunmayı durdurdu. Multi-Factor Authentication gerektiren yeni müşterilerin bulut tabanlı Azure Multi-Factor Authentication kullanması gerekir. Daha fazla bilgi için bkz. [bulut tabanlı Azure Multi-Factor Authentication dağıtımı planlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---
