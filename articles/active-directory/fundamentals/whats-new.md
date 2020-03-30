---
title: Yenilikler Sürüm notları - Azure Etkin Dizini | Microsoft Dokümanlar
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, amortismana uygun işlevler ve yaklaşan değişiklikler gibi Azure Etkin Dizin'inde yenilikleri öğrenin.
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
ms.date: 03/25/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f547d67dcb2880b0b51088bc17c43eb9436a0903
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369628"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Etkin Dizin'deki yenilikler nelerdir?

>Bu URL'yi kopyalayıp rss özet akışı okuyucu simge `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` ![](./media/whats-new/feed-icon-16x16.png) okuyucunuza kopyalayıp yapıştırarak güncellemeler için bu sayfayı ne zaman tekrar ziyaret edeceğiz konusunda bilgilendirilin.

Azure AD sürekli olarak geliştirmeler alır. En son gelişmelerden haberdar olmak için bu makalede, aşağıdakiler hakkında bilgi verilmektedir:

- En son sürümler
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik
- Değişiklik planları

Bu sayfa aylık olarak güncellenir, bu nedenle düzenli olarak yeniden ziyaret edin. Altı aydan eski öğeler arıyorsanız, bunları [Azure Etkin Dizini'ndeki yenilikler için Arşiv'de](whats-new-archive.md)bulabilirsiniz.

---

## <a name="march-2020"></a>Mart 2020

### <a name="users-with-the-default-access-role-will-be-in-scope-for-provisioning"></a>Varsayılan erişim rolüne sahip kullanıcılar,

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi
 
Tarihsel olarak, varsayılan erişim rolüne sahip kullanıcılar sağlama kapsamı dışında dır. Müşterilerin bu role sahip kullanıcıların sağlama kapsamında olmasını istediklerine dair geri bildirimler duyduk. Tüm yeni sağlama yapılandırmalarının varsayılan erişim rolüne sahip kullanıcıların sağlanmasına olanak sağlaması için bir değişiklik dağıtmaya çalışıyoruz. Yavaş yavaş, bu rolü sağlayan kullanıcıları desteklemek için varolan sağlama yapılandırmaları için davranışı değiştireceğiz. Müşteri eylemi gerekmez. Bu değişiklik yapıldıktan sonra [belgelerimizde](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned) bir güncelleştirme yayınlayacağız.

---

### <a name="azure-ad-b2b-collaboration-will-be-available-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet-tenants"></a>Azure AD B2B işbirliği, 21Vianet (Azure China 21Vianet) kiracıları tarafından işletilen Microsoft Azure'da kullanıma sunulacaktır

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C
 
Azure AD B2B işbirliği özellikleri, 21Vianet (Azure China 21Vianet) kiracıları tarafından işletilen Microsoft Azure'da kullanıma sunulacak ve Azure China 21Vianet kiracısındaki kullanıcıların diğer Azure China 21Vianet kiracılarında kullanıcılarla sorunsuz bir şekilde işbirliği yapmalarını sağlayacak. [Azure AD B2B işbirliği hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/b2b/)

---
 
### <a name="azure-ad-b2b-collaboration-invitation-email-redesign"></a>Azure AD B2B İşbirliği daveti e-posta yeniden tasarımı

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C
 
Kullanıcıları dizine davet etmek için Azure AD B2B işbirliği davet hizmeti tarafından gönderilen [e-postalar,](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) davet bilgilerini ve kullanıcının sonraki adımlarını daha net hale getirmek üzere yeniden tasarlanır.

---

### <a name="homerealmdiscovery-policy-changes-will-appear-in-the-audit-logs"></a>HomeRealmDiscovery ilke değişiklikleri denetim günlüklerinde görünecektir

**Türü:** Sabit  
**Hizmet kategorisi:** Denetim  
**Ürün kapasitesi:** İzleme & Raporlama
 
[HomeRealmDiscovery ilkesindeki](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) değişikliklerin denetim günlüklerine dahil olmadığı bir hata düzeltildik. Artık ilkenin ne zaman ve nasıl değiştirildiğini ve kim tarafından değiştirileceğini görebilirsiniz. 

---

### <a name="azure-ad-b2b-collaboration-available-in-azure-government-tenants"></a>Azure AD B2B İşbirliği Azure Kamu kiracılarında kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C
 
Azure AD B2B işbirliği özellikleri artık bazı Azure Kamu kiracıları arasında kullanılabilir.  Kiracınızın bu özellikleri kullanıp kullanabildiğini öğrenmek için, [Azure ABD Devlet kiracımda B2B işbirliğinin kullanılabilir olup olmadığını nasıl anlayabilirim?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant)

---

### <a name="azure-monitor-integration-for-azure-logs-is-now-available-in-azure-government"></a>Azure Günlükleri için Azure Monitör tümlemesi artık Azure Kamu'da kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama
 
Azure Ad günlükleriyle Azure Monitör tümleştirmesi artık Azure Kamu'da kullanılabilir. Azure REKLAM Günlüklerini (Denetim ve Oturum Açma Günlükleri) bir depolama hesabına, Etkinlik Hub'ına ve Günlük Analitiğine yönlendirebilirsiniz. Lütfen Azure REKLAM senaryoları [için raporlama ve izleme için](https://docs.microsoft.com/azure/active-directory/reports-monitoring/plan-monitoring-and-reporting) ayrıntılı [belgelerin](https://aka.ms/aadlogsinamd) yanı sıra dağıtım planlarına göz atın.

---

### <a name="identity-protection-refresh-in-azure-government"></a>Azure Kamu'da Kimlik Koruması Yenileme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

[Microsoft Azure Kamu portalında](https://portal.azure.us/)yenilenen Azure AD [Kimlik Koruması](https://aka.ms/IdentityProtectionDocs) deneyimini kullanıma sunmuş olmamızı paylaşmaktan heyecan içindeyiz. Daha fazla bilgi için duyuru [blog yazımıza](https://techcommunity.microsoft.com/t5/public-sector-blog/identity-protection-refresh-in-microsoft-azure-government/ba-p/1223667)bakın.

---

### <a name="disaster-recovery-download-and-store-your-provisioning-configuration"></a>Olağanüstü durum kurtarma: Sağlama yapılandırmanızı indirin ve saklayın

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi
 
Azure AD sağlama hizmeti zengin bir yapılandırma özellikleri kümesi sağlar. Müşterilerin yapılandırmalarını daha sonra başvurabilmeleri veya bilinen iyi bir sürüme geri dönebilmeleri için yapılandırmalarını kaydedebilmeleri gerekir. Sağlama yapılandırmanızı JSON dosyası olarak indirebilme ve ihtiyacınız olduğunda yükleme olanağı nı ekledik. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/app-provisioning/export-import-provisioning-configuration).

---
 
### <a name="sspr-self-service-password-reset-now-requires-two-gates-for-admins-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>SSPR (self servis parola sıfırlama) artık 21Vianet (Azure China 21Vianet) tarafından işletilen Microsoft Azure yöneticileri için iki kapı gerektirir 

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Self Servis Şifre Sıfırlama  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
Daha önce 21Vianet (Azure China 21Vianet) tarafından işletilen Microsoft Azure'da, kendi parolalarını sıfırlamak için self servis parola sıfırlama (SSPR) kullanan yöneticiler, kimliklerini kanıtlamak için yalnızca bir "geçit" (meydan okuma) gerekiyordu. Kamu ve diğer ulusal bulutlarda, yöneticiler genellikle SSPR kullanırken kimliklerini kanıtlamak için iki kapı kullanmalıdır. Ancak Azure China 21Vianet'te SMS veya telefon görüşmelerini desteklemediğimiz için, yöneticiler tarafından tek kapılı parolanın sıfırlanmasına izin verdik.

Azure China 21Vianet ile genel bulut arasında SSPR özellik paritesi oluşturuyoruz. İleriye dönük olarak, yöneticiler SSPR kullanırken iki kapı kullanmalıdır. SMS, telefon görüşmeleri ve Authenticator uygulaması bildirimleri ve kodları desteklenecektir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#administrator-reset-policy-differences).

---

### <a name="password-length-is-limited-to-256-characters"></a>Parola uzunluğu 256 karakterle sınırlıdır

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Azure AD hizmetinin güvenilirliğini sağlamak için kullanıcı parolalarının uzunluğu artık 256 karakterle sınırlıdır. Bundan daha uzun parolaları olan kullanıcılardan, yöneticileriyle iletişim kurarak veya self servis parola sıfırlama özelliğini kullanarak sonraki girişte parolalarını değiştirmeleri istenir.

Bu değişiklik 13 Mart 2020 tarihinde 10AM PST (18:00 UTC) ve hata AADSTS 50052, InvalidPasswordExceedsMaxLength olarak etkinleştirildi. Daha fazla ayrıntı için [kesme değişiklik bildirimine](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#user-passwords-will-be-restricted-to-256-characters) bakın.

---

### <a name="azure-ad-sign-in-logs-are-now-available-for-all-free-tenants-through-the-azure-portal"></a>Azure AD oturum açma günlükleri artık Azure portalı üzerinden tüm ücretsiz kiracılar için kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama
 
Şu andan itibaren, ücretsiz kiracısı olan müşteriler [Azure REKLAM oturum açma günlüklerine Azure portalından](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) 7 güne kadar erişebilir. Daha önce oturum açma günlükleri yalnızca Azure Active Directory Premium lisansına sahip müşteriler için kullanılabilirdi. Bu değişiklikle, tüm kiracılar bu günlüklere portal üzerinden erişebilir.

> [!NOTE]
> Müşterilerin oturum açma günlerine Microsoft Graph API ve Azure Monitor üzerinden erişmek için yine de premium lisansa (Azure Active Directory Premium P1 veya P2) ihtiyacı vardır.

---

### <a name="deprecation-of-directory-wide-groups-option-from-groups-general-settings-on-azure-portal"></a>Azure portalındaki Gruplar Genel Ayarları'ndan Dizin çapında gruplar seçeneğinin amortismanı

**Türü:** Kaldırıl -mış  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Müşterilerin ihtiyaçlarını en iyi şekilde karşılayan dizin çapında gruplar oluşturmaları için daha esnek bir yol sağlamak için, Azure portalındaki**Genel** **Gruplar** > ayarlarından dinamik [grup belgelerine](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)bağlantı yla **Dizin çapında Gruplar** seçeneğini değiştirdik. Yöneticilerin konuk kullanıcıları içeren veya hariç tutan tüm kullanıcı grupları oluşturabilmesi için belgelerimizi daha fazla yönerge içerecek şekilde geliştirdik.

---

## <a name="february-2020"></a>Şubat 2020

### <a name="upcoming-changes-to-custom-controls"></a>Özel denetimlerde yapılacak değişiklikler

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Mfa  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
Mevcut özel denetim önizlemesini, iş ortağı tarafından sağlanan kimlik doğrulama özelliklerinin Azure Active Directory yöneticisi ve son kullanıcı deneyimleriyle sorunsuz çalışmasına olanak tanıyan bir yaklaşımla değiştirmeyi planlıyoruz. Bugün, ortak MFA çözümleri aşağıdaki sınırlamalarla karşı karşıyadır: yalnızca bir parola girildikten sonra çalışırlar; diğer önemli senaryolarda hızlandırma kimlik doğrulaması için MFA olarak hizmet etmezler; ve son kullanıcı veya yönetim kimlik bilgileri yönetim işlevleri ile entegre etmez. Yeni uygulama, iş ortağı tarafından sağlanan kimlik doğrulama etkenlerinin kayıt, kullanım, MFA talepleri, kimlik doğrulama, raporlama ve günlüğe kaydetme gibi önemli senaryolar için yerleşik etkenlerle birlikte çalışmasına olanak sağlar. 

Özel denetimler, genel kullanılabilirliğe ulaşana kadar yeni tasarımın yanı sıra önizlemede de desteklenmeye devam eder. Bu noktada, müşterilere yeni tasarıma geçiş için zaman vereceğiz. Geçerli yaklaşımın sınırlamaları nedeniyle, yeni tasarım kullanılabilir olana kadar yeni sağlayıcılar üzerinde olmaz. Müşteriler ve sağlayıcılarla yakın bir şekilde çalışıyoruz ve yaklaştıkça zaman çizelgesini ileteceğiz. [Daha fazla bilgi edinin](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/upcoming-changes-to-custom-controls/ba-p/1144696#).

---

### <a name="identity-secure-score---mfa-improvement-action-updates"></a>Identity Secure Score - MFA geliştirme eylem güncellemeleri

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Mfa  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
Microsoft Secure Score, işletmelerin kendi leriyle çalışan ilkeleri uygularken en yüksek güvenliği sağlama gereksinimini yansıtmak için, çok faktörlü kimlik doğrulama (MFA) merkezli üç geliştirme eylemini kaldırıyor ve iki tane ekliyor.

Aşağıdaki geliştirme eylemleri kaldırılacak:

- MFA için tüm kullanıcıları kaydedin
- Tüm kullanıcılar için MFA gerektirme
- Azure AD ayrıcalıklı rolleri için MFA gerektirir

Aşağıdaki iyileştirme eylemleri eklenecektir:

- Güvenli erişim için tüm kullanıcıların MFA'yı tamamlayabilmesini sağlayın
- İdari roller için MFA gerektirir

Bu yeni geliştirme eylemleri, kullanıcılarınızı veya yöneticilerinizi Dizininizde MFA'ya kaydetmenizi ve kuruluş gereksinimlerinize uygun doğru ilkeler kümesini oluşturmanızı gerektirir. Temel amaç, tüm kullanıcılarınızın ve yöneticilerinizin birden fazla etken veya risk tabanlı kimlik doğrulama istemiyle kimlik doğrulamalarını sağlarken esnekliğe sahip olmaktır. Bu, Microsoft'un MFA kullanıcıları için ne zaman meydan okuyacağına karar vermesine izin veren güvenlik varsayılanları ayarlama veya kapsamlı kararlar uygulayan birden çok ilke olması şeklinde olabilir. Bu geliştirme eylem güncelleştirmelerinin bir parçası olarak, Temel koruma ilkeleri artık puanlama hesaplamalarına dahil edilecektir. [Microsoft Secure Score'da nelerin geldiği hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score-whats-coming?view=o365-worldwide)

---

### <a name="azure-ad-domain-services-sku-selection"></a>Azure AD Etki Alanı Hizmetleri SKU seçimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Etki Alanı Hizmetleri  
**Ürün kapasitesi:** Azure AD Etki Alanı Hizmetleri
 
Azure AD Etki Alanı Hizmetleri müşterilerinin örnekleri için performans düzeylerini seçmekte daha fazla esneklik istediklerine dair geri bildirimler duyduk. 1 Şubat 2020'den itibaren dinamik bir modelden (Azure AD'nin nesne sayısına göre performans ve fiyatlandırma katmanını belirlediği) kendi seçimini modele geçtik. Artık müşteriler ortamlarıyla eşleşen bir performans katmanı seçebilirler. Bu değişiklik aynı zamanda Kaynak Ormanları gibi yeni senaryoları ve günlük yedeklemeler gibi Premium özellikleri etkinleştirmemizi de sağlar. Nesne sayısı artık tüm SUS'lar için sınırsızdır, ancak her katman için nesne sayısı önerileri sunmaya devam edeceğiz.

**Hemen müşteri eylemi gerekmez.** Varolan müşteriler için, 1 Şubat 2020'de kullanılmakta olan dinamik katman, yeni varsayılan katmanı belirler. Bu değişikliğin sonucu olarak herhangi bir fiyatlandırma veya performans etkisi yoktur. İleriye dönük olarak, Azure AD DS müşterilerinin dizin boyutu ve iş yükü özellikleri değiştikçe performans gereksinimlerini değerlendirmesi gerekir. Hizmet katmanları arasında geçiş bir kesinti ye gerek kalma işlemi olmaya devam eder ve artık müşterileri dizilişlerinin büyümesine bağlı olarak otomatik olarak yeni katmanlara taşımayız. Ayrıca, hiçbir fiyat artışları olacak ve yeni fiyatlandırma mevcut faturalandırma modeli ile uyumlu olacaktır. Daha fazla bilgi için [Azure AD DS SK'leri belgelerine](https://docs.microsoft.com/azure/active-directory-domain-services/administration-concepts#azure-ad-ds-skus) ve [Azure AD Etki Alanı Hizmetleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/active-directory-ds/)bakın.

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2020"></a>Azure AD App galerisinde bulunan yeni Federe Uygulamalar - Şubat 2020

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Şubat 2020'de, Federasyon desteğine sahip bu 31 yeni uygulamayı uygulama galerisine ekledik: 

[IamIP Patent Platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/iamip-patent-platform-tutorial), [Deneyim Bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/experience-cloud-tutorial), [NS1 SSO Azure için](https://docs.microsoft.com/azure/active-directory/saas-apps/ns1-sso-azure-tutorial), [Barracuda E-posta Güvenlik Servisi](https://ess.barracudanetworks.com/sso/azure), [ABa Raporlama](https://myaba.co.uk/client-access/signin/auth/msad), Kriz Durumunda - Online [Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-online-portal-tutorial), BIC [Bulut Tasarım](https://docs.microsoft.com/azure/active-directory/saas-apps/bic-cloud-design-tutorial), [Arıcı Azure AD Veri Bağlayıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/beekeeper-azure-ad-data-connector-tutorial), [Korn Feribot Değerlendirmeler ,](https://www.kornferry.com/solutions/kf-digital/kf-assess) [Verkada Komut](https://docs.microsoft.com/azure/active-directory/saas-apps/verkada-command-tutorial), [Splashtop](https://docs.microsoft.com/azure/active-directory/saas-apps/splashtop-tutorial), [Syxsense](https://docs.microsoft.com/azure/active-directory/saas-apps/syxsense-tutorial), [EAB Gezinme](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-tutorial), Yeni Emanet [(Sınırlı Sürüm)](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial), [Thulium](https://admin.thulium.com/login/instance), [Bilet Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/ticketmanager-tutorial), [Şablon Takımlar için Seçici](https://links.officeatwork.com/templatechooser-download-teams), [Beesy](https://www.beesy.me/index.php/site/login), [Sağlık Destek Sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/health-support-system-tutorial), [MURAL](https://app.mural.co/signup), [Hive](https://docs.microsoft.com/azure/active-directory/saas-apps/hive-tutorial), [LavaDo](https://appsource.microsoft.com/product/web-apps/lavaloon.lavado_standard?tab=Overview), [Wakelet](https://wakelet.com/login), [Firmex VDR](https://docs.microsoft.com/azure/active-directory/saas-apps/firmex-vdr-tutorial), [Öğretmenler ve Okullar için ThingLink](https://www.thinglink.com/), [Coda](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial), [NearpodApp](https://nearpod.com/signup/?oc=Microsoft&utm_campaign=Microsoft&utm_medium=site&utm_source=product), [WEDO](https://docs.microsoft.com/azure/active-directory/saas-apps/wedo-tutorial), [Davetliler](https://invitepeople.com/login), [Reprints Desk - Madde Galaxy](https://docs.microsoft.com/azure/active-directory/saas-apps/reprints-desk-article-galaxy-tutorial), [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-tutorial)

 
Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---february-2020"></a>Azure AD Uygulama Galerisi'nde yeni sağlama bağlayıcıları - Şubat 2020

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Artık bu yeni tümleken uygulamalar için kullanıcı hesaplarını oluşturmayı, güncelleyebilir ve silebilirsiniz:

- [Mixpanel](https://docs.microsoft.com/azure/active-directory/saas-apps/mixpanel-provisioning-tutorial)
- [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-provisioning-tutorial)
- [Azure Databricks](https://docs.microsoft.com/azure/active-directory/saas-apps/azure-databricks-scim-connector-provisioning-tutorial)
- [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-provisioning-tutorial)
- [Zapier](https://docs.microsoft.com/azure/active-directory/saas-apps/zapier-provisioning-tutorial)

Otomatik kullanıcı hesabı sağlama yı kullanarak kuruluşunuzun daha iyi nasıl güvenli hale alınabildiğini hakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlamasağlamayı otomatikleştir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)bakın.

---
 
### <a name="azure-ad-support-for-fido2-security-keys-in-hybrid-environments"></a>Karma ortamlarda FIDO2 güvenlik anahtarları için Azure AD desteği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Hibrit ortamlarda KI FIDO2 güvenlik anahtarları için Azure AD desteğinin genel önizlemesini duyuruyoruz. Kullanıcılar artık FIDO2 güvenlik anahtarlarını kullanarak Windows 10 aygıtlarına katılan Karma Azure AD aygıtlarında oturum açabilir ve şirket içi ve bulut kaynaklarında sorunsuz oturum açabilir. Karma ortamlar için destek, Azure AD'nin birleştiği cihazlarda FIDO2 desteği için genel önizlemeyi başlattığımızdan beri parolasız müşterilerimizin en çok istediği özellik olmuştur. Biyometri ve genel/özel anahtar şifrelemesi gibi gelişmiş teknolojileri kullanarak parolasız kimlik doğrulaması, güvenli olurken kolaylık ve kullanım kolaylığı sağlar. Bu genel önizleme ile artık geleneksel Active Directory kaynaklarına erişmek için FIDO2 güvenlik anahtarları gibi modern kimlik doğrulamasını kullanabilirsiniz. Daha fazla bilgi [için SSO'ya giderek şirket içi kaynaklara gidin.](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises) 

Başlamak için, adım adım talimatlar [için kiracınız için FIDO2 güvenlik anahtarlarını etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key) sayfasını ziyaret edin. 

---
 
### <a name="the-new-my-account-experience-is-now-generally-available"></a>Yeni Hesabım deneyimi artık genel olarak kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Profilim/Hesabım  
**Ürün kapasitesi:** Son Kullanıcı Deneyimleri
 
Tüm son kullanıcı hesap yönetimi ihtiyaçları için tek durak olan Hesabım artık genel olarak kullanılabilir! Son kullanıcılar bu yeni siteye URL üzerinden veya yeni Uygulamalarım deneyiminin üstbilgisinde erişebilir. Yeni deneyimin [Hesabım Portalıma Genel Bakış'ta](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview)sunduğu tüm self servis özellikleri hakkında daha fazla bilgi edinin.

---
 
### <a name="my-account-site-url-updating-to-myaccountmicrosoftcom"></a>Hesabımın site URL'si myaccount.microsoft.com güncelleniyor

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Profilim/Hesabım  
**Ürün kapasitesi:** Son Kullanıcı Deneyimleri
 
Yeni Hesabım son kullanıcı deneyimi, URL'sini `https://myaccount.microsoft.com` bir sonraki aya güncelleyecek. [Hesabım portalında](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview)son kullanıcılara sunduğu deneyim ve tüm hesap self servis özellikleri hakkında daha fazla bilgi edinin.

---
 
## <a name="january-2020"></a>Ocak 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>Yeni Uygulamalarım portalı artık genel olarak kullanılabilir

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Son Kullanıcı Deneyimleri
 
Kuruluşunuzun artık genel olarak kullanılabilen yeni Uygulamalarım portalına yükseltin! Yeni portal ve koleksiyonlar hakkında daha fazla bilgiyi [My Apps portalındaki Koleksiyonoluştur'da](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections)bulabilirsiniz.

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Azure AD'deki çalışma alanları koleksiyonlarla yeniden adlandırıldı

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulamalarım   
**Ürün kapasitesi:** Son Kullanıcı Deneyimleri
 
Çalışma alanları, filtreler yöneticileri kullanıcılarının uygulamaları düzenlemek için yapılandırabilirsiniz, şimdi koleksiyon olarak anılacaktır. Bunları nasıl yapılandırabileceğiniz hakkında daha fazla bilgiyi [Uygulamalarım portalındaki Koleksiyonoluştur'da](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections)bulabilirsiniz.

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Azure AD B2C Özel İlke (Genel Önizleme) kullanarak Kaydolma ve Kaydolma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C
 
Telefon numarası kaydolma ve kaydolma ile geliştiriciler ve işletmeler, müşterilerinin SMS yoluyla kullanıcının telefon numarasına gönderilen tek seferlik bir şifre kullanarak kaydolmasını ve oturum açmalarına izin verebilir. Bu özellik, müşterinin telefonuna erişimini kaybetmesi durumunda telefon numarasını değiştirmesine de olanak tanır. Özel politikaların gücüyle, telefona kaydolma ve kaydolma, geliştiricilerin ve işletmelerin markalarını sayfa özelleştirme yoluyla iletişim kurmasına olanak tanır. Azure AD [B2C'de özel ilkelerle telefon adedi](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication)ve oturum açma ayarlama yı öğrenin.
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Azure AD Uygulama Galerisi'nde yeni sağlama bağlayıcıları - Ocak 2020

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Artık bu yeni tümleken uygulamalar için kullanıcı hesaplarını oluşturmayı, güncelleyebilir ve silebilirsiniz:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler Private Access](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Otomatik kullanıcı hesabı sağlama yı kullanarak kuruluşunuzun daha iyi nasıl güvenli hale alınabildiğini hakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlamasağlamayı otomatikleştir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)bakın.

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Azure AD App galerisinde bulunan yeni Federe Uygulamalar - Ocak 2020

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Ocak 2020'de, Federasyon desteğine sahip bu 33 yeni uygulamayı uygulama galerisine ekledik: 

[JOSA](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [Fastly Edge Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [Terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [Spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Abibot Netlogistik](https://azuremarketplace.microsoft.com/marketplace/apps/aad.abibotnetlogistik) `https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access`, [Upshotly](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial), [LeaveBot](https://leavebot.io/#home), [DataCamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [TripActions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [SmartWork](https://www.intumit.com/english/SmartWork.html), [Dotcom-Monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [SSOGEN - Azure AD SSO Gateway Oracle E-Business Suite için - EBS, PeopleSoft ve JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial), [Barındırılan MyCirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [Yuhu Emlak Yönetim Platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), `https://sites.lumapps.com/login` [Upwork Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [ Talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [Microsoft Teams için SmartDB](http://teams.smartdb.jp/login/), [PressPage](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [ContractSafe Saml2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [Maxient Conduct Manager Software](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [Helpshift](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/), [CoreView](https://portal.coreview.com/), [Squelch Cloud Office365 Bağlayıcı](https://laxmi.squelch.io/login), [PingFlow Kimlik Doğrulama](https://app-staging.pingview.io/), [PrinterLogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [Taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), `https://app.sandwai.com/` [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [AssetSonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [Akari Sanal Yardımcısı](https://akari.io/akari-virtual-assistant/)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="two-new-identity-protection-detections"></a>İki yeni Kimlik Koruması algılaması

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
Kimlik Koruması'na iki yeni oturum açma bağlantılı algılama türü ekledik: Şüpheli gelen kutusu işleme kuralları ve Impossible seyahati. Bu çevrimdışı algılamalar Microsoft Cloud App Security (MCAS) tarafından keşfedilir ve Kimlik Koruması'nda kullanıcıyı ve oturum açma riskini etkiler. Bu algılamalar hakkında daha fazla bilgi için [oturum açma risk türlerimize](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk)bakın.
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Son Dakika Değişimi: URI Parçaları giriş yönlendirmesi ile taşınmayacak

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
8 Şubat 2020'den itibaren, bir kullanıcıyı oturum açma isteği login.microsoftonline.com gönderildiğinde, hizmet isteğe boş bir parça ekler.  Bu, tarayıcının istekteki varolan herhangi bir parçayı silmesini sağlayarak bir yeniden yönlendirme saldırısı sınıfının önüne geçimesini sağlar. Hiçbir uygulamanın bu davranışa bağımlılığı olmamalıdır. Daha fazla bilgi için bkz: Microsoft kimlik platformu belgelerindeki [son dakika değişiklikleri.](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020)

---

## <a name="december-2019"></a>Aralık 2019

### <a name="integrate-sap-successfactors-provisioning-into-azure-ad-and-on-premises-ad-public-preview"></a>Azure AD ve şirket içi AD (Genel Önizleme) içine sağlayan SAP Başarı Faktörleri'ni tümleştirin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi

Artık SAP Başarı Faktörleri'ni Azure AD'de yetkili bir kimlik kaynağı olarak tümleştirebilirsiniz. Bu tümleştirme, Azure AD hesaplarının sağlanmasını denetlemek için yeni işe almalar veya sonlandırmalar gibi İk tabanlı etkinlikler de dahil olmak üzere uçtan uca kimlik yaşam döngüsünü otomatikleştirmenize yardımcı olur.

Azure AD'ye SAP SuccessFactors gelen sağlama nın nasıl ayarlanması hakkında daha fazla bilgi için, [Yapılandırma SAP Başarı Faktörleri otomatik sağlama](https://aka.ms/SAPSuccessFactorsInboundTutorial) öğreticisine bakın.

---

### <a name="support-for-customized-emails-in-azure-ad-b2c-public-preview"></a>Azure AD B2C'de özelleştirilmiş e-postalar için destek (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Artık, kullanıcılarınız uygulamalarınızı kullanmak için kaydolduğunda özelleştirilmiş e-postalar oluşturmak için Azure AD B2C'yi kullanabilirsiniz. DisplayControls (şu anda önizlemede) ve bir üçüncü taraf e-posta sağlayıcısı [(SendGrid,](https://sendgrid.com/) [SparkPost](https://sparkpost.com/)veya özel BIR REST API gibi) kullanarak, kendi e-posta şablonunuzu, **Adresten** ve konu metninizi kullanabilir, yerelleştirme ve özel tek seferlik parola (OTP) ayarlarını destekleyebilirsiniz.

Daha fazla bilgi için [Azure Active Directory B2C'de Özel e-posta doğrulaması'na](https://docs.microsoft.com/azure/active-directory-b2c/custom-email)bakın.

---

### <a name="replacement-of-baseline-policies-with-security-defaults"></a>Temel ilkelerin güvenlik varsayılanlarıyla değiştirilmesi

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Kimlik Güvenliği ve Koruma

Kimlik doğrulama için varsayılan olarak güvenli bir modelin bir parçası olarak, varolan temel koruma ilkelerini tüm kiracılardan kaldırıyoruz. Bu kaldırma Şubat ayı sonunda tamamlanması hedeflenir. Bu temel koruma ilkelerinin yerine güvenlik varsayılanları dır. Temel koruma ilkelerini kullanıyorsanız, yeni güvenlik varsayılan ilkesine veya Koşullu Erişim'e geçmeyi planlamanız gerekir. Bu ilkeleri kullanmadıysanız, işlem yapmak için herhangi bir işlem yoktur.

Yeni güvenlik varsayılanları hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) Koşullu Erişim ilkeleri hakkında daha fazla bilgi için [Ortak Koşullu Erişim ilkelerine](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common)bakın.

---

## <a name="november-2019"></a>Kasım 2019

### <a name="support-for-the-samesite-attribute-and-chrome-80"></a>SameSite özniteliği ve Chrome 80 desteği

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Tanımlama bilgileri için varsayılan olarak güvenli bir modelin parçası olan Chrome 80 tarayıcısı, tanımlama bilgilerini öznitelik olmadan `SameSite` nasıl kullandığını değiştiriyor. `SameSite` Öznitelik belirtmeyen herhangi bir `SameSite=Lax`çerez, Chrome'un uygulamanızın bağlı olabileceği belirli etki alanı arası çerez paylaşımı senaryolarını engellemesine neden olacak şekilde ayarlanmış gibi değerlendirilir. Eski Chrome davranışını korumak için, `SameSite=None` özniteliği kullanabilir `Secure` ve ek bir öznitelik ekleyebilirsiniz, böylece siteler arası tanımlama bilgilerine yalnızca HTTPS bağlantıları üzerinden erişilebilir. Chrome'un bu değişikliği 4 Şubat 2020'ye kadar tamamlaması planlanıyor.

Tüm geliştiricilerimizin uygulamalarını bu kılavuzu kullanarak test etmesini öneririz:

- **Güvenli Çerez Kullan** ayarı için varsayılan değeri **Evet**olarak ayarlayın.

- **SameSite** özniteliği için varsayılan değeri **Yok**olarak ayarlayın.

- Secure ek `SameSite` bir **Secure**öznitelik ekleyin.

Daha fazla bilgi için, Chrome sürüm 79 ve [sonraki sürümlerde müşteri web siteleri ve Microsoft ürün ve hizmetlerinde](https://support.microsoft.com/help/4522904/potential-disruption-to-microsoft-services-in-chrome-beta-version-79)ASP.NET ve ASP.NET Çekirdek ve Potansiyel kesintiye [yaklaşan SameSite Çerez Değişiklikleri](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) bakın.

---

### <a name="new-hotfix-for-microsoft-identity-manager-mim-2016-service-pack-2-sp2"></a>Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2) için yeni düzeltme

**Türü:** Sabit  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi

Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2) için bir düzeltme toplama paketi (build 4.6.34.0) kullanılabilir. Bu toplama paketi sorunları giderir ve "Düzeltilen sorunlar ve bu güncelleştirmede eklenen iyileştirmeler" bölümünde açıklanan iyileştirmeler ekler.

Daha fazla bilgi ve hotfix paketini indirmek için [Microsoft Identity Manager 2016 Service Pack 2 (build 4.6.34.0) Update Rollup'a](https://support.microsoft.com/help/4512924/microsoft-identity-manager-2016-service-pack-2-build-4-6-34-0-update-r)bakın.

---

### <a name="new-ad-fs-app-activity-report-to-help-migrate-apps-to-azure-ad-public-preview"></a>Uygulamaları Azure AD'ye geçirmelerine yardımcı olmak için yeni AD FS uygulama etkinlik raporu (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Azure portalındaki yeni Active Directory Federation Services (AD FS) uygulama etkinlik raporunu kullanarak hangi uygulamalarınızın Azure AD'ye geçirilme kapasitesine sahip olduğunu belirleyin. Rapor, Azure AD ile uyumluluk için tüm AD FS uygulamalarını değerlendirir, herhangi bir sorunu denetler ve geçiş için tek tek uygulamalar hazırlama konusunda rehberlik sağlar.

Daha fazla bilgi için bkz: [Uygulamaları Azure AD'ye geçirmek için AD FS uygulama etkinlik raporunu kullan.](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity)

---

### <a name="new-workflow-for-users-to-request-administrator-consent-public-preview"></a>Kullanıcıların yönetici onayı istemesi için yeni iş akışı (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Erişim Kontrolü

Yeni yönetici onayı iş akışı, yöneticilere yönetici onayı gerektiren uygulamalara erişim izni verme nin bir yolunu sunar. Bir kullanıcı bir uygulamaya erişmeye çalışırsa, ancak onay sağlayamazsa, artık yönetici onayı için bir istek gönderebilir. İstek e-posta yla gönderilir ve Azure portalından erişilebilen bir kuyruğa, gözden geçiren olarak atanan tüm yöneticilere yerleştirilir. Bir gözden geçiren bekleyen bir istek üzerinde işlem yaptıktan sonra, isteyen kullanıcılar eylem hakkında bilgilendirilir.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow)

---

### <a name="new-azure-ad-app-registrations-token-configuration-experience-for-managing-optional-claims-public-preview"></a>İsteğe bağlı talepleri yönetmek için yeni Azure AD Uygulama Kayıtları Belirteç yapılandırma deneyimi (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Geliştirici Deneyimi

Azure portalındaki yeni **Azure AD Uygulama Kayıtları Belirteç yapılandırma** bıçağı, artık uygulama geliştiricilere uygulamaları için isteğe bağlı taleplerin dinamik bir listesini gösterir. Bu yeni deneyim, Azure AD uygulama geçişlerini kolaylaştırmaya ve isteğe bağlı talep yanlış yapılandırmalarını en aza indirmeye yardımcı olur.

Daha fazla bilgi için bkz. Azure [AD uygulamanızda isteğe bağlı talepler sağlayın.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims)

---

### <a name="new-two-stage-approval-workflow-in-azure-ad-entitlement-management-public-preview"></a>Azure AD yetkilendirme yönetiminde yeni iki aşamalı onay iş akışı (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Yetki Yönetimi

Bir erişim paketinde kullanıcının isteğini onaylaması için iki onaylayıcıya ihtiyaç duymanızı sağlayan yeni bir iki aşamalı onay iş akışı başlattık. Örneğin, bunu, isteyen kullanıcıyöneticisinin önce onaylaması ve ardından bir kaynak sahibinin onaylaması için de gerekli olacak şekilde ayarlayabilirsiniz. Onaylayanlardan biri onaylamıyorsa, erişim izni verilmez.

Daha fazla bilgi için Azure [AD yetkilendirme yönetiminde bir erişim paketi için değişiklik isteği ve onay ayarlarına](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-request-policy)bakın.

---

### <a name="updates-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Yeni çalışma alanlarıyla birlikte Uygulamalarım sayfasındaki güncelleştirmeler (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** 3.

Artık kuruluşunuzun kullanıcılarının yenilenen Uygulamalarım deneyimini görüntüleme ve bunlara erişme biçimini özelleştirebilirsiniz. Bu yeni deneyim, kullanıcılarınızın uygulamaları bulmasını ve düzenlemesini kolaylaştıran yeni çalışma alanları özelliğini de içerir.

Yeni Uygulamalarım deneyimi ve çalışma alanları oluşturma hakkında daha fazla bilgi için Bkz. [Uygulamalarım portalında çalışma alanları oluştur.](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces)

---

### <a name="google-social-id-support-for-azure-ad-b2b-collaboration-general-availability"></a>Azure AD B2B işbirliği için Google sosyal kimlik desteği (Genel Kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD'de Google sosyal bağlantılarını (Gmail hesapları) kullanmaya yönelik yeni destek, kullanıcılarınızın ve iş ortaklarının işbirliğini kolaylaştırmaya yardımcı olur. Artık iş ortaklarınızın Microsoft'a özel yeni bir hesap oluşturmasına ve yönetmesine gerek yoktur. Microsoft Teams artık google kullanıcılarını tüm istemciler ve ortak ve kiracıyla ilgili kimlik doğrulama bitiş noktalarında tam olarak desteklemektedir.

Daha fazla bilgi için [B2B konuk kullanıcıları için google'ı kimlik sağlayıcısı olarak ekle'ye](https://docs.microsoft.com/azure/active-directory/b2b/google-federation)bakın.

---

### <a name="microsoft-edge-mobile-support-for-conditional-access-and-single-sign-on-general-availability"></a>Koşullu Erişim ve Tek Oturum Açma için Microsoft Edge Mobil Desteği (Genel Kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

iOS ve Android'deki Microsoft Edge için Azure REKLAM artık Azure AD Tek Oturum Açma ve Koşullu Erişimi destekler:

- **Microsoft Edge tek oturum açma (SSO):** Azure AD'ye bağlı tüm uygulamalarda artık yerel istemciler (Microsoft Outlook ve Microsoft Edge gibi) tek oturum açma kullanılabilir.

- **Microsoft Edge koşullu erişim:** Uygulama tabanlı koşullu erişim ilkeleri sayesinde, kullanıcılarınızın Microsoft Edge gibi Microsoft Intune korumalı tarayıcıları kullanması gerekir.

Koşullu erişim ve Microsoft Edge ile SSO hakkında daha fazla bilgi için, [Koşullu Erişim için Microsoft Edge Mobil Desteği ve Şimdi Genel Olarak Kullanılabilen Tek Oturum](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Microsoft-Edge-Mobile-Support-for-Conditional-Access-and-Single/ba-p/988179) Açma blog gönderisi için bkz. [Uygulama tabanlı koşullu erişimi](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) veya aygıt tabanlı [koşullu erişimi](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)kullanarak istemci uygulamalarınızı nasıl ayarlayınız hakkında daha fazla bilgi için bkz. [Manage web access using a Microsoft Intune policy-protected browser](https://docs.microsoft.com/intune/apps/app-configuration-managed-browser)

---

### <a name="azure-ad-entitlement-management-general-availability"></a>Azure AD yetkilendirme yönetimi (Genel Kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Yetki Yönetimi

Azure AD yetkilendirme yönetimi, kuruluşların kimlik ve erişim yaşam döngüsünü ölçekte yönetmesine yardımcı olan yeni bir kimlik yönetimi özelliğidir. Bu yeni özellik, gruplar, uygulamalar ve SharePoint Online siteleri arasında erişim isteği iş akışlarını, erişim atamalarına, incelemelere ve sona erme tarihini otomatikleştirerek yardımcı olur.

Azure AD yetkilendirme yönetimi yle, hem çalışanlar hem de kuruluşunuz dışındaki bu kaynaklara erişmeye ihtiyaç duyan kullanıcılar için erişimi daha verimli bir şekilde yönetebilirsiniz.

Daha fazla bilgi için Azure [AD yetkilendirme yönetimi nedir?](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview#license-requirements)

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için kullanıcı hesabı sağlamayı otomatikleştirin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.  

Artık bu yeni tümleken uygulamalar için kullanıcı hesaplarını oluşturmayı, güncelleyebilir ve silebilirsiniz:

[SAP Bulut Platformu Kimlik Doğrulama Servisi](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-provisioning-tutorial), [SpaceIQ](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-provisioning-tutorial), [Miro](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-provisioning-tutorial), [Cloudgate](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-provisioning-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloudsuite-provisioning-tutorial), [OfficeSpace Yazılım](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-software-provisioning-tutorial), [Öncelik Matris](https://docs.microsoft.com/azure/active-directory/saas-apps/priority-matrix-provisioning-tutorial)

Otomatik kullanıcı hesabı sağlama yı kullanarak kuruluşunuzun daha iyi nasıl güvenli hale alınabildiğini hakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlamasağlamayı otomatikleştir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2019"></a>Azure AD App galerisinde yeni Federe Uygulamalar mevcuttur - Kasım 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Kasım 2019'da, Federasyon desteğine sahip 21 yeni uygulamayı uygulama galerisine ekledik:

[Airtable](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [Hootsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [Üyeler için Mavi Erişim (BAM)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [Bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [ResLife Portal](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal Tek İşaret Açık (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), `https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279` [Motus](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial), [MyAryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial), [BlueMail](https://loginself1.bluemail.me/), [Beedle](https://teams-web.beedle.co/#/), [Visma](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial), [OneDesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [Foko Perakende](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [Qmarkets Fikir & Yenilik Yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [Netskope Kullanıcı Özlaması](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial), [uniFLOW Online](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial), [Claromentis](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial), [Jisc Öğrenci Seçmen Kaydı](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial), [e4enable](https://portal.e4enable.com/)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="new-and-improved-azure-ad-application-gallery"></a>Yeni ve geliştirilmiş Azure AD uygulama galerisi

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Azure Active Directory kiracınızda sağlama, OpenID Connect ve SAML'yi destekleyen önceden tümleşik uygulamaları bulmanızı kolaylaştırmak için Azure AD uygulama galerisini güncelledik.

Daha fazla bilgi için bkz: [Azure Etkin Dizin kiracınıza uygulama ekle.](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal)

---

### <a name="increased-app-role-definition-length-limit-from-120-to-240-characters"></a>Uygulama rol tanımı uzunluk sınırı 120 ile 240 karakter arasında artırıldı

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Müşterilerden, bazı uygulamalarda ve hizmetlerde uygulama rol tanımı değerinin uzunluk sınırının 120 karakterde çok kısa olduğunu duyduk. Buna karşılık, rol değeri tanımının maksimum uzunluğunu 240 karaktere çıkardık.

Uygulamaya özgü rol tanımlarını kullanma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)

---

## <a name="october-2019"></a>Ekim 2019

### <a name="deprecation-of-the-identityriskevent-api-for-azure-ad-identity-protection-risk-detections"></a>Azure AD Kimlik Koruması risk algılamaları için kimlikRiskEvent API'sinin amortismanı  

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Geliştirici geri bildirimlerine yanıt olarak, Azure AD Premium P2 aboneleri artık Microsoft Graph için yeni risk Algılama API'sini kullanarak Azure AD Identity Protection'ın risk algılama verilerinde karmaşık sorgular gerçekleştirebilir. Mevcut [kimlikRiskEvent](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta) API beta sürümü **10 Ocak 2020**civarında veri dönen duracak. Kuruluşunuz identityRiskEvent API'yi kullanıyorsa, yeni risk Algılama API'sine geçiş yapmalısınız.

Yeni risk Algılama API'si hakkında daha fazla bilgi için [Risk algılama API başvuru belgelerine](https://aka.ms/RiskDetectionsAPI)bakın.

---

### <a name="application-proxy-support-for-the-samesite-attribute-and-chrome-80"></a>SameSite Öznitelik ve Chrome 80 için Uygulama Proxy desteği

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Chrome 80 tarayıcı sürümünden birkaç hafta önce, Application Proxy tanımlama bilgilerinin **SameSite** özniteliğine nasıl davrandığını güncellemeyi planlıyoruz. Chrome 80 sürümü ile, **SameSite** özniteliği belirtmeyen herhangi bir çerez olarak ayarlanmış `SameSite=Lax`gibi kabul edilecektir .

Bu değişiklik nedeniyle olası olumsuz etkileri önlemek için, Uygulama Proxy erişimini ve oturum çerezlerini şu şekilde güncelliyoruz:

- **Güvenli Çerez Kullan** ayarı için varsayılan değeri **Evet**olarak ayarlama.

- **SameSite** özniteliği için varsayılan değeri **Yok**olarak ayarlama.

    >[!NOTE]
    > Uygulama Proxy erişim çerezleri her zaman yalnızca güvenli kanallar üzerinden iletilir. Bu değişiklikler yalnızca oturum tanımlama bilgileri için geçerlidir.

Uygulama Proxy çerez ayarları hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki şirket içi uygulamalara erişmek için Çerez ayarlarına](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings)bakın.

---

### <a name="app-registrations-legacy-and-converged-app-management-from-the-application-registration-portal-appsdevmicrosoftcom-will-no-longer-be-available"></a>Uygulama Kayıt Portalı'ndan (apps.dev.microsoft.com) uygulama kayıtları (eski) ve yakınsama uygulaması yönetimi artık kullanılamaya

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Yok  
**Ürün kapasitesi:** Geliştirici Deneyimi

Yakın gelecekte, Azure AD hesapları olan kullanıcılar artık Uygulama Kayıt Portalı 'nı (apps.dev.microsoft.com) kullanarak yakınsanan uygulamaları kaydedebilecek ve yönetemeyecek veya Uygulama kayıtlarındaki (eski) uygulamaları kaydedebilecek ve yönetemeyecektir. Azure portalında deneyim.

Yeni Uygulama kayıtları deneyimi hakkında daha fazla bilgi edinmek için [Azure portalı eğitim kılavuzundaki Uygulama kayıtlarına](../develop/app-registrations-training-guide-for-app-registrations-legacy-users.md)bakın.

---

### <a name="users-are-no-longer-required-to-re-register-during-migration-from-per-user-mfa-to-conditional-access-based-mfa"></a>Kullanıcılar artık kullanıcı başına MFA'dan Koşullu Erişim tabanlı MFA'ya geçiş sırasında yeniden kaydolmak zorunda değildir

**Türü:** Sabit  
**Hizmet kategorisi:** Mfa  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Kullanıcıların kullanıcı başına Çok Faktörlü Kimlik Doğrulama (MFA) için devre dışı bırakılmış ve daha sonra Koşullu Erişim ilkesi yle MFA için etkinleştirildiğinde yeniden kaydolmaları gereken bilinen bir sorunu giderdik.

Kullanıcıların yeniden kaydolmasını sağlamak için, Azure AD portalında kullanıcının kimlik doğrulama yöntemlerinden **Gerekli Yeniden Kayıt MFA** seçeneğini seçebilirsiniz. Kullanıcı başına MFA'dan Koşullu Erişim tabanlı MFA'ya geçiş hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted#convert-users-from-per-user-mfa-to-conditional-access-based-mfa)

---

### <a name="new-capabilities-to-transform-and-send-claims-in-your-saml-token"></a>SAML belirtecinizdeki talepleri dönüştürmek ve göndermek için yeni yetenekler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

SAML belirtecinizde talepleri özelleştirmenize ve göndermenize yardımcı olacak ek özellikler ekledik. Bu yeni yetenekler şunlardır:

- Ek talepler dönüştürme işlevleri, talep gönderdiğiniz değeri değiştirmenize yardımcı olur.

- Tek bir iddiaya birden çok dönüşüm uygulama becerisi.

- Kullanıcı türüne ve kullanıcının ait olduğu gruba bağlı olarak talep kaynağını belirtebilme.

Bu yeni özellikler hakkında ayrıntılı bilgi için, bunların nasıl kullanılacağı da dahil olmak üzere, [kurumsal uygulamalar için SAML belirtecinde verilen talepleri Özelleştir'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)bakın.

---

### <a name="new-my-sign-ins-page-for-end-users-in-azure-ad"></a>Azure AD'deki son kullanıcılar için Yeni Oturum Açmasayfam

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** İzleme & Raporlama

Kuruluşunuzun kullanıcılarının olağan dışı etkinlikleri kontrolhttps://mysignins.microsoft.com) etmek için son oturum açma geçmişlerini görüntülemelerini sağlamak için yeni bir **Oturum Açma sayfam** ekledik. Bu yeni sayfa, kullanıcılarınızın şunları görmesini sağlar:

- Herkes şifresini tahmin etmeye çalışıyorsa.

- Bir saldırgan hesabına ve hangi konumdan başarılı bir şekilde oturum açmışsa.

- Saldırganın erişmeye çalıştığı uygulamalar.

Daha fazla bilgi için, Kullanıcılar artık alışılmadık etkinlik blogu [için oturum açma geçmişini kontrol edebilirsiniz](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Users-can-now-check-their-sign-in-history-for-unusual-activity/ba-p/916066) bakın.

---

### <a name="migration-of-azure-ad-domain-services-azure-ad-ds-from-classic-to-azure-resource-manager-virtual-networks"></a>Azure AD Etki Alanı Hizmetlerinin (Azure AD DS) klasikten Azure Kaynak Yöneticisi sanal ağlarına geçişi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Etki Alanı Hizmetleri  
**Ürün kapasitesi:** Azure AD Etki Alanı Hizmetleri

Klasik sanal ağlarda sıkışıp kalmış müşterilerimiziçin harika haberlerimiz var! Artık klasik bir sanal ağdan varolan bir Kaynak Yöneticisi sanal ağa tek seferlik geçiş gerçekleştirebilirsiniz. Kaynak Yöneticisi sanal ağına geçtikten sonra, ince taneli parola ilkeleri, e-posta bildirimleri ve denetim günlükleri gibi ek ve yükseltilmiş özelliklerden yararlanabilirsiniz.

Daha fazla bilgi için bkz: [Önizleme - Azure AD Etki Alanı Hizmetlerini Klasik sanal ağ modelinden Kaynak Yöneticisi'ne geçirin.](https://docs.microsoft.com/azure/active-directory-domain-services/migrate-from-classic-vnet)

---

### <a name="updates-to-the-azure-ad-b2c-page-contract-layout"></a>Azure AD B2C sayfa sözleşme düzeninde güncelleştirmeler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Azure AD B2C için sayfa sözleşmesinin 1.2.0 sürümünde bazı yeni değişiklikler sunduk. Bu güncelleştirilmiş sürümde, artık öğelerinizin yük sırasını denetleyebilirsiniz, bu da stil sayfası (CSS) yüklendiğinde oluşan titremeyi durdurmaya yardımcı olabilir.

Sayfa sözleşmesinde yapılan değişikliklerin tam listesi için [Sürüm değiştirme günlüğüne](https://docs.microsoft.com/azure/active-directory-b2c/page-layout#120)bakın.

---

### <a name="update-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Yeni çalışma alanlarıyla birlikte Uygulamalarım sayfasına güncelleştir (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Erişim Kontrolü

Artık kuruluşunuzun kullanıcılarının uygulamaları bulmalarını kolaylaştırmak için yeni çalışma alanları özelliğini kullanmak da dahil olmak üzere yepyeni Uygulamalarım deneyimini görüntüleme ve bunlara erişme biçimini özelleştirebilirsiniz. Yeni çalışma alanları işlevi, kuruluşunuzun kullanıcılarının zaten erişebildiği uygulamalar için bir filtre görevi görür.

Yeni Uygulamalarım deneyimini kullanıma alma ve çalışma alanları oluşturma hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces)bkz.

---

### <a name="support-for-the-monthly-active-user-based-billing-model-general-availability"></a>Aylık etkin kullanıcı tabanlı faturalandırma modeli desteği (Genel kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Azure AD B2C artık aylık etkin kullanıcıların (MAU) faturalandırmasını destekliyor. MAU faturalandırma, bir takvim ayı boyunca kimlik doğrulama etkinliği olan benzersiz kullanıcı sayısına dayanır. Varolan müşteriler bu yeni faturalandırma yöntemine istediği zaman geçiş yapabilir.

1 Kasım 2019 tarihinden itibaren tüm yeni müşteriler bu yöntemkullanılarak otomatik olarak faturalandırılır. Bu faturalandırma yöntemi, maliyet avantajları ve önceden plan yapma becerisi sayesinde müşterilere yarar sağlar.

Daha fazla bilgi için bkz: [Aylık etkin kullanıcı faturalandırma modeline yükseltin.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing#upgrade-to-monthly-active-users-billing-model)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2019"></a>Azure AD App galerisinde yeni Federe Uygulamalar mevcuttur - Ekim 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Ekim 2019'da, Federasyon desteğine sahip bu 35 yeni uygulamayı uygulama galerisine ekledik:

[Kriz Durumunda – Mobil](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-mobile-tutorial), [Juno Journey](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial), [ExponentHR](https://docs.microsoft.com/azure/active-directory/saas-apps/exponenthr-tutorial), [Tact](https://tact.ai/assistant/), [OpusCapita Nakit Yönetimi](http://cm1.opuscapita.com/tenantname), [Salestim](https://prd.salestim.io/forms), [Learnster](https://docs.microsoft.com/azure/active-directory/saas-apps/learnster-tutorial), [Dynatrace](https://docs.microsoft.com/azure/active-directory/saas-apps/dynatrace-tutorial), [Önsezi](https://login.hunchbuzz.com/integrations/azure/process), [Freshworks](https://docs.microsoft.com/azure/active-directory/saas-apps/freshworks-tutorial), [eCornell](https://docs.microsoft.com/azure/active-directory/saas-apps/ecornell-tutorial), [ShipHazmat](https://docs.microsoft.com/azure/active-directory/saas-apps/shiphazmat-tutorial), [Netskope Bulut Güvenlik](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial), [Contentful](https://docs.microsoft.com/azure/active-directory/saas-apps/contentful-tutorial), [Bağlama](https://bindtuning.com/login), [HireVue Koordinat – Avrupa](https://www.hirevue.com/), [KiralamaVue Koordinat - USOnly](https://www.hirevue.com/), [Kiralama Koordinat - ABD](https://www.hirevue.com/), [ WittyParrot Bilgi Kutusu](https://wittyapi.wittyparrot.com/wittyparrot/api/provision/trail/signup), [Cloudmore](https://docs.microsoft.com/azure/active-directory/saas-apps/cloudmore-tutorial), [Visit.org](https://docs.microsoft.com/azure/active-directory/saas-apps/visitorg-tutorial), [Cambium Xirrus EasyPass Portal](https://login.xirrus.com/azure-signup), [Paylocity](https://docs.microsoft.com/azure/active-directory/saas-apps/paylocity-tutorial), [Mail Luck!](https://docs.microsoft.com/azure/active-directory/saas-apps/mail-luck-tutorial), [Teamie](https://theteamie.com/), [Takımlar için Hız](https://velocity.peakup.org/teams/login), [SIGNL4](https://account.signl4.com/manage), [EAB IMPL gezinmek](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-impl-tutorial), [ScreenMeet](https://console.screenmeet.com/), [Omega Point](https://pi.ompnt.com/), [Intune için E-posta konuşan (iPhone)](https://speaking.email/FAQ/98/email-access-via-microsoft-intune), [Ofis 365 Doğrudan (iPhone / Android) için Konuşma E-posta ( iPhone / Android)](https://speaking.email/FAQ/126/email-access-via-microsoft-office-365-direct), [ExactCare SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/exactcare-sso-tutorial), [iHealthHome Bakım Navigasyon Sistemi ](https://ihealthnav.com/account/signin), [Qubie](https://qubie.azurewebsites.net/static/adminTab/authorize.html)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="consolidated-security-menu-item-in-the-azure-ad-portal"></a>Azure AD portalında birleştirilmiş Güvenlik menü öğesi

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Artık kullanılabilir tüm Azure REKLAM güvenlik özelliklerine yeni **Güvenlik** menüsü öğesinden ve Azure portalındaki **Arama** çubuğundan erişebilirsiniz. Ayrıca, **Güvenlik - Başlarken**olarak adlandırılan yeni **Güvenlik** açılış sayfası, herkese açık belgelerimize, güvenlik kılavuzumuza ve dağıtım kılavuzlarımıza bağlantılar sağlayacaktır.

Yeni **Güvenlik** menüsü şunları içerir:

- Koşullu Erişim
- Kimlik Koruması
- Güvenlik Merkezi
- Kimlik Güvenli Puanı
- Kimlik doğrulama yöntemleri
- Mfa
- Risk raporları - Riskli kullanıcılar, Riskli oturum açmalar, Risk tespitleri
- Ve daha fazlası...

Daha fazla bilgi için [Güvenlik - Başlarken'e](https://portal.azure.com/#blade/Microsoft_AAD_IAM/SecurityMenuBlade/GettingStarted)bakın.

---

### <a name="office-365-groups-expiration-policy-enhanced-with-autorenewal"></a>Office 365 grupları son kullanma ilkesi otomatik yenileme ile geliştirilmiştir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi

Office 365 grupları son kullanma ilkesi, üyeleri tarafından etkin olarak kullanılan grupları otomatik olarak yenilemek için geliştirilmiştir. Gruplar, Outlook, SharePoint ve Takımlar dahil olmak üzere tüm Office 365 uygulamalarındaki kullanıcı etkinliğine bağlı olarak otomatik olarak yenilenir.

Bu geliştirme, grup sona erme bildirimlerinizi azaltmaya ve etkin grupların kullanılabilir olmaya devam etmesini sağlamaya yardımcı olur. Office 365 gruplarınız için zaten etkin bir son kullanma denetiminiz varsa, bu yeni işlevselliği açmak için hiçbir şey yapmanız gerekmez.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-lifecycle)

---

### <a name="updated-azure-ad-domain-services-azure-ad-ds-creation-experience"></a>Güncelleştirilmiş Azure AD Etki Alanı Hizmetleri (Azure AD DS) oluşturma deneyimi

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD Etki Alanı Hizmetleri  
**Ürün kapasitesi:** Azure AD Etki Alanı Hizmetleri

Azure AD Etki Alanı Hizmetlerini (Azure AD DS) yeni ve geliştirilmiş bir oluşturma deneyimi içerecek şekilde güncelledik ve sadece üç tıklamayla yönetilen bir etki alanı oluşturmanıza yardımcı olduk! Ayrıca, artık bir şablondan Azure AD DS'yi yükleyebilir ve dağıtabilirsiniz.

Daha fazla bilgi için [Bkz. Öğretici: Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance)

---
