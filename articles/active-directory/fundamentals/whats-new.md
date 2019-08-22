---
title: Yenilikler Sürüm Notları - Azure Active Directory | Microsoft Docs
description: Azure Active Directory'ye yeni gibi bilinen sorunları, hata düzeltmeleri, en son sürüm notları, işlevler de kullanım dışı ve gelecek öğrenin değişiklikler.
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 50f2f1f7f5d8509a28557e9704bc178be465b1ee
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648651"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory'deki yenilikler nelerdir?

>Bu URL 'yi kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` alın: ![RSS akışı okuyucu simgesi](./media/whats-new/feed-icon-16x16.png) akış okuyucusuna.

Azure AD iyileştirmeleri düzenli olarak alır. İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklikleri planları

Bu sayfaya ay güncelleştirilir, böylece bunu düzenli olarak tekrar ziyaret. Altı aydan eski olan öğelere bakıyorsanız, [Azure Active Directory yenilikler Için arşivde](whats-new-archive.md)bulabilirsiniz.

---

## <a name="august-2019"></a>Ağustos 2019

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET, yerinde senaryolar için MSAL.NET paylaşılan önbelleğini yoksayar

**Türüyle** Sabit  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı Kimlik Doğrulaması

Azure AD kimlik doğrulama kitaplığı (ADAL.NET) sürüm 5.0.0-Preview sürümünden itibaren, uygulama geliştiricileri [Web uygulamaları ve Web API 'leri için hesap başına bir önbellek serileştirmelidir](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api). Aksi takdirde, bazı belirli kullanım durumları `UserAssertion`ile birlikte, [Şirket adına akışı](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow)kullanan bazı senaryolar ayrıcalık yükselmesine neden olabilir. Bu güvenlik açığından kaçınmak için, ADAL.NET artık, için Microsoft kimlik doğrulama kitaplığı için DotNet (MSAL.NET) paylaşılan önbellek için Microsoft kimlik doğrulama kitaplığını yok sayar.

Bu sorun hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik doğrulaması kitaplığı ayrıcalık yükselmesi güvenlik açığı](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258).

---

## <a name="july-2019"></a>2019 Temmuz

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>Değişiklik planı: Yalnızca TLS 1,2 ' i destekleyecek uygulama proxy hizmeti güncelleştirmesi

**Türüyle** Değişiklik planı  
**Hizmet kategorisi:** Uygulama Proxy'si  
**Ürün yeteneği:** Erişim Denetimi

En güçlü şifreimizi size sağlamaya yardımcı olmak için, uygulama proxy hizmeti erişimini yalnızca TLS 1,2 protokollerine sınırlandırmaya başlayacağız. Bu sınırlama, başlangıçta zaten TLS 1,2 protokollerini kullanan müşterilere dağıtılır, bu nedenle etkiyi görmezsiniz. TLS 1,0 ve TLS 1,1 protokollerinin kullanım dışı bırakılması, 31 Ağustos 2019 tarihinde tamamlanacaktır. Hala TLS 1,0 ve TLS 1,1 kullanan müşteriler, bu değişikliğe hazırlanmak için gelişmiş bir bildirim alır.

Bu değişiklik boyunca uygulama ara sunucusu hizmetiyle bağlantıyı sürdürmek için, istemci-sunucu ve tarayıcı-sunucu birleşimlerinizin TLS 1,2 kullanacak şekilde güncelleştirildiğinden emin olmanızı öneririz. Ayrıca, uygulama proxy 'Si hizmeti aracılığıyla yayınlanan uygulamalara erişmek için çalışanlarınızın kullandığı tüm istemci sistemlerini dahil ettiğinizden emin olmanızı öneririz.

Daha fazla bilgi için bkz. [Azure Active Directory Içindeki uygulama proxy 'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>Değişiklik planı: Tasarım güncelleştirmeleri uygulama galerisine geliyor

**Türüyle** Değişiklik planı  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** SSO

Yeni Kullanıcı arabirimi değişiklikleri, **Uygulama Ekle** dikey penceresinin Galeri alanından **Ekle** ' nin tasarımına geliyor. Bu değişiklikler, otomatik sağlamayı, OpenID Connect, Security Assertion Markup Language (SAML) ve parola çoklu oturum açmayı (SSO) destekleyen uygulamalarınızı daha kolay bir şekilde bulmanıza yardımcı olur.

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>Değişiklik planı: MFA sunucusu IP adresinin Office 365 IP adresinden kaldırılması

**Türüyle** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması

MFA sunucusu IP adresini [Office 365 IP adresi ve URL Web hizmetinden](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service)kaldırıyoruz. Şu anda güvenlik duvarı ayarlarınızı güncelleştirmek için bu sayfalara güveniyorsanız, Başlarken 'in **Azure Multi-Factor Authentication sunucusu güvenlik duvarı gereksinimleri** bölümünde belgelenen IP adreslerinin listesini de dahil ettiğinizden emin olmanız gerekir. [ Azure Multi-Factor Authentication sunucusu makalesiyle birlikte](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements) .

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>Yalnızca uygulama belirteçleri, artık istemci uygulamasının kaynak kiracısında var olmasını gerektiriyor

**Türüyle** Sabit  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı Kimlik Doğrulaması

26 Temmuz 2019 ' de, [istemci kimlik bilgileri verme](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)yoluyla yalnızca uygulama belirteçleri sağlıyoruz. Daha önce, uygulamalar, istemci uygulamanın kiracıda olup olmamasına bakılmaksızın diğer uygulamaları çağırmak için belirteçleri alabilir. Bu davranışı, bazen Web API 'Leri olarak adlandırılan tek kiracılı kaynakların yalnızca kaynak kiracısında bulunan istemci uygulamaları tarafından çağrılabilir şekilde güncelleştirdik.

Uygulamanız kaynak kiracısında değilse, bu sorunu gidermek `The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.` için, [Yönetici onay uç noktasını](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint) veya [PowerShell aracılığıyla kiracıda istemci uygulama hizmeti sorumlusunu oluşturmanız gerektiğini belirten bir hata iletisi alırsınız. ](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell), kiracınızın kiracı içinde çalışmaya yönelik uygulama iznini vermiş olmasını sağlar.

Daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant).

> [!NOTE]
> İstemci ve API arasındaki mevcut onay gerekli olmaya devam eder. Uygulamalar yine de kendi yetkilendirme denetimlerini yapmakta olmalıdır.

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>FIDO2 güvenlik anahtarları kullanılarak Azure AD 'de yeni parolasız oturum açma

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı Kimlik Doğrulaması

Azure AD müşterileri artık, kuruluşlarının kullanıcıları ve grupları için FIDO2 güvenlik anahtarlarını yönetmek üzere ilkeler ayarlayabilir. Son kullanıcılar aynı zamanda güvenlik anahtarlarını kendi kendine kaydedebilir, FIDO özellikli cihazlarda web sitelerindeki Microsoft hesaplarında oturum açmak için anahtarları kullanabilir ve Azure AD 'ye katılmış Windows 10 cihazlarında oturum açabilirler.

Daha fazla bilgi için bkz. yönetici ile ilgili bilgiler için [Azure AD 'de passwordless oturum açma 'Yı etkinleştirme (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-enable) ve son kullanıcı ile ilgili bilgiler için Güvenlik [anahtarı (Önizleme) kullanmak üzere güvenlik bilgilerini ayarlama](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key) .

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Temmuz 2019

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** 3. Taraf Tümleştirme

Haziran 2019 ' de, uygulama galerisine federasyon desteği olan bu 18 yeni uygulamayı ekledik:

[Ungerboeck Software](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial), [parlak model omnichannel Contact Center](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial), [Clever Nelly](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial), [acquireıo](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial), [Gevop](https://www.looop.co/schedule-a-demo/), [Productboard](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial), [MS Azure SSO erişimi, nııdex uyumluluk ofisi™](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso), [hype](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial), [ Abstract](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial), [ascentis](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial), [flipsnack](https://www.flipsnack.com/accounts/sign-in-sso.html), [wandera](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial), [TwineSocial](https://twinesocial.com/), [Kallifer](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial), [hyperanna](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial), [ilaç, wastetanık](https://www.pharmid.com/), [i2B Connect](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/), [JFrog Artifactory](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Tuş takımı](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federasyon dizini](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="new-azure-ad-domain-services-service-tag-for-network-security-group"></a>Ağ güvenlik grubu için yeni Azure AD Domain Services hizmet etiketi

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

IP adresi ve aralıklarının uzun listesini yönetmeyi yorsanız, Azure ağ güvenlik grubunuzdaki yeni **AzureActiveDirectoryDomainServices** Network Service etiketini kullanarak Azure AD Domain Services sanal ağınıza gelen trafiğin güvenliğini sağlayabilirsiniz ağ alt ağı.

Bu yeni hizmet etiketi hakkında daha fazla bilgi için bkz. [Azure AD Domain Services Için ağ güvenlik grupları](https://docs.microsoft.com/azure/active-directory-domain-services/network-considerations#default-network-service-group).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Domain Services için yeni güvenlik denetimleri (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Azure AD etki alanı hizmeti güvenlik denetimi 'nin genel önizlemeye sunulduğunu duyurmaktan mutluluk duyuyoruz. Güvenlik denetimi, Azure AD etki alanı hizmeti 'ni kullanarak Azure depolama, Azure Log Analytics çalışma alanları ve Azure Olay Hub 'ı dahil olmak üzere hedeflenen kaynaklara güvenlik denetim olayları akışı yaparak size kimlik doğrulama hizmetlerinize yönelik kritik Öngörüler sağlamanıza yardımcı olur. Portal.

Daha fazla bilgi için bkz. [Azure AD Domain Services Için güvenlik denetimlerini etkinleştirme (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>Yeni kimlik doğrulama yöntemleri kullanım & öngörüleri (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Self Servis Parola Sıfırlama  
**Ürün yeteneği:** & Raporlamayı izleme

Yeni kimlik doğrulama yöntemleri kullanım & öngörüleri raporları, Azure Multi-Factor Authentication ve self servis parola sıfırlama gibi özelliklerin kuruluşunuzda nasıl kaydedildiği ve kullanıldığı hakkında bilgi almanıza yardımcı olabilir; Örneğin, kayıtlı sayısı dahil her özelliğe yönelik kullanıcılar, parolaları sıfırlamak için self servis parola sıfırlamasının ne sıklıkta kullanıldığı ve hangi yöntemin sıfırlanacağı.

Daha fazla bilgi için bkz. [kimlik doğrulama yöntemleri kullanımı & Öngörüler (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights).

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>Tüm Azure AD yöneticileri için yeni güvenlik raporları kullanılabilir (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Identity Protection  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Tüm Azure AD yöneticileri artık riskli **Kullanıcılar** ve **riskli oturum açma** raporlarında gösterildiği gibi yeni güvenlik deneyimini kullanmaya başlamak için, **risk için işaretlenmiş kullanıcılar** raporu gibi mevcut güvenlik raporlarının en üstündeki başlığı seçebilirler. . Zaman içinde, tüm güvenlik raporları eski sürümlerden yeni sürümlere taşınır ve yeni raporlar aşağıdaki ek olanakları sağlar:

- Gelişmiş filtreleme ve sıralama

- Eksik Kullanıcı riski gibi toplu eylemler

- Güvenliği aşılmış veya güvenli varlıkların onayı

- Risk durumu, kapsayan: Tehlikede, kapatıldı, düzeltildi ve onaylandı

Daha fazla bilgi için bkz. [riskli kullanıcılar raporu](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risky-users-signins#risky-users-report) ve [riskli oturum açma bileşenleri raporu](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risky-users-signins#risky-sign-ins-report).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Domain Services için yeni güvenlik denetimleri (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Azure AD etki alanı hizmeti güvenlik denetimi 'nin genel önizlemeye sunulduğunu duyurmaktan mutluluk duyuyoruz. Güvenlik denetimi, Azure AD etki alanı hizmeti 'ni kullanarak Azure depolama, Azure Log Analytics çalışma alanları ve Azure Olay Hub 'ı dahil olmak üzere hedeflenen kaynaklara güvenlik denetim olayları akışı yaparak size kimlik doğrulama hizmetlerinize yönelik kritik Öngörüler sağlamanıza yardımcı olur. Portal.

Daha fazla bilgi için bkz. [Azure AD Domain Services Için güvenlik denetimlerini etkinleştirme (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>SAML/WS-beslemesiyle yeni B2B Direct Federation (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** B2B  
**Ürün yeteneği:** B2B/B2C

Doğrudan Federasyon, SAML veya WS-Beslikli standartları destekleyen kimlik sistemleriyle çalışarak, BT tarafından yönetilen kimlik çözümü Azure AD olmayan iş ortaklarıyla çalışmanızı kolaylaştırır. Bir iş ortağıyla doğrudan Federasyon ilişkisi ayarladıktan sonra, bu etki alanından davet ettiğiniz tüm yeni Konuk kullanıcıları mevcut kurumsal hesaplarını kullanarak sizinle işbirliği yapabilir ve konuklarınız için Kullanıcı deneyimini daha sorunsuz hale getirir.

Daha fazla bilgi için bkz. [Konuk kullanıcılar için AD FS ve üçüncü taraf sağlayıcılarla doğrudan Federasyon (Önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Tuş takımı](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federasyon dizini](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-check-for-duplicate-group-names-in-the-azure-ad-portal"></a>Azure AD portalındaki yinelenen grup adları için yeni denetim

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş Birliği

Artık Azure AD portalından bir grup adı oluşturduğunuzda veya güncelleştirdiğinizde, kaynakta mevcut bir grup adını çoğaltdığınıza ilişkin bir denetim yapacağız. Adın zaten başka bir grup tarafından kullanımda olduğunu tespit ettik, adınızı değiştirmeniz istenir.

Daha fazla bilgi için bkz. [Azure AD portalındaki grupları yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD artık yanıt (yeniden yönlendirme) URI 'Lerinde statik sorgu parametrelerini desteklemektedir

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı Kimlik Doğrulaması

Azure AD Apps artık, OAuth 2,0 istekleri için statik sorgu parametreleriyle (örneğin, `https://contoso.com/oauth2?idp=microsoft`) Yanıtla (yeniden yönlendirme) URI 'leri kaydedebilir ve kullanabilir. Statik sorgu parametresi, yanıt URI 'sinin diğer herhangi bir bölümünde olduğu gibi, yanıt URI 'Leri için dize eşlemeye tabidir. URL kodu çözülmüş yeniden yönlendirme URI 'si ile eşleşen kayıtlı bir dize yoksa, istek reddedilir. Yanıt URI 'SI bulunursa, statik sorgu parametresi dahil olmak üzere, kullanıcının yeniden yönlendirileceği tüm dize kullanılır.

Bir güvenlik riskini temsil ettiğinden dinamik yanıt URI 'Leri hala yasaktır ve bir kimlik doğrulama isteği genelinde durum bilgilerini sürdürmek için kullanılamaz. Bu amaçla `state` parametresini kullanın.

Şu anda Azure portal uygulama kayıt ekranları hala sorgu parametrelerini engelliyor. Ancak, uygulamanıza sorgu parametrelerini eklemek ve test etmek için uygulama bildirimini el ile düzenleyebilirsiniz. Daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters).

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Azure AD için etkinlik günlükleri (MS Graph API 'Leri) artık PowerShell cmdlet 'Leri aracılığıyla kullanılabilir

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Raporlanıyor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlüklerinin (denetim ve oturum açma raporları) artık Azure AD PowerShell modülü aracılığıyla kullanılabildiğini duyurmak için heyecanlıyız. Daha önce, MS Graph API uç noktaları kullanarak kendi betiklerinizi oluşturabilir ve şimdi bu özelliği PowerShell cmdlet 'lerine genişlettik.

Bu cmdlet 'leri kullanma hakkında daha fazla bilgi için bkz. [Raporlama Için Azure AD PowerShell cmdlet 'leri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting).

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Azure AD 'de denetim ve oturum açma günlükleri için filtre denetimleri güncelleştirildi

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Raporlanıyor  
**Ürün yeteneği:** & Raporlamayı izleme

Denetim ve oturum açma günlüğü raporlarını, artık rapor ekranlarında sütun olarak eklemek zorunda kalmadan çeşitli filtreler uygulayabilmeniz için güncelleştirdik. Ayrıca, ekranda kaç filtre göstermek istediğinizi de seçebilirsiniz. Bu güncelleştirmeler, raporların daha kolay okunmasını ve gereksinimlerinize göre daha fazla kapsamlanmasını kolaylaştırmak için birlikte çalışır.

Bu güncelleştirmeler hakkında daha fazla bilgi için bkz. [Denetim günlüklerini filtreleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs) ve [oturum açma etkinliklerini filtreleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities).

---

## <a name="june-2019"></a>Haziran 2019

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>Microsoft Graph için yeni riskDetections API (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Identity Protection  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Microsoft Graph için yeni riskDetections API 'sini duyurmaktan mutluluk duyuyoruz, artık genel önizlemede. Bu yeni API 'YI, kuruluşunuzun kimlik korumasıyla ilgili Kullanıcı ve oturum açma riski algılamalarının bir listesini görüntülemek için kullanabilirsiniz. Bu API 'yi, algılama türü, durumu, düzeyi ve daha fazlası hakkındaki ayrıntılar dahil, risk algılamalarınızı daha verimli bir şekilde sorgulamak için de kullanabilirsiniz.

Daha fazla bilgi için [risk algılama API 'si başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Haziran 2019

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** 3. Taraf Tümleştirme

Haziran 2019 ' de, uygulama galerisine federasyon desteğiyle bu 22 yeni uygulamaları ekledik:

[Azure AD SAML araç seti](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial), [Otsuka Shokai (大塚商会)](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial), [ANAQUA](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial), [Azure VPN istemcisi](https://portal.azure.com/), [expenseın](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial), [yardımcı Yardımcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial), [Costpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial), [GlobalOne](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial), [Mercedes-Benz-otomobil ofisi](https://me.secure.mercedes-benz.com/), [Skore](https://app.justskore.it/), Oracle [bulut altyapısı konsolu](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial), [Siark SAML kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial), bir [satın alınabilen edu](https://www.scrible.com/sign-in/#/create-account), [PandaDoc](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial), [PERCEPTYX](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial), [proptibir OS](https://proptimise.co.uk/software/), [vtiger CRM (SAML)](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial), Oracle için Oracle Erişim Yöneticisi Oracle E-Iş paketi için perakende merchandising, Oracle Erişim Yöneticisi, E-Iş paketi için Oracle ıDCS, PeopleSoft için Oracle IDCs, JD Edler için Oracle ıDCS

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Yakınlaştırma](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Haberci](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4.](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>Azure AD sağlama hizmeti 'nin gerçek zamanlı ilerlemesini görüntüleme

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün yeteneği:** Kimlik Yaşam Döngüsü Yönetimi

Azure AD sağlama deneyimini, Kullanıcı sağlama sürecinde ne kadar uzakta olduğunu gösteren yeni bir ilerleme çubuğu içerecek şekilde güncelleştirdik. Bu güncelleştirilmiş deneyim Ayrıca, geçerli döngüyle sağlanan kullanıcı sayısı ve ne kadar Kullanıcı sağlandığını gösteren bilgileri de sağlar.

Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user).

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>Şirket markası artık oturum kapatma ve hata ekranları üzerinde görünüyor

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı Kimlik Doğrulaması

Azure AD 'yi, şirket markasının artık oturum açma ve hata ekranlarında, oturum açma sayfasından da görünmesini sağlayacak şekilde güncelleştirdik. Bu özelliği açmak için herhangi bir şey yapmanız gerekmez, Azure AD yalnızca Azure portal **Şirket markası** alanında ayarlamış olduğunuz varlıkları kullanır.

Şirket markanızı ayarlama hakkında daha fazla bilgi için bkz. [kuruluşunuzun Azure Active Directory sayfalarına marka ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>Azure Multi-Factor Authentication (MFA) sunucusu artık Yeni dağıtımlar için kullanılamaz

**Türüyle** Kullanım Dışı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması

1 Temmuz 2019 itibariyle, Microsoft artık Yeni dağıtımlar için MFA sunucusu sunmaz. Kuruluşunuzda Multi-Factor Authentication istemek isteyen yeni müşterilerin artık bulut tabanlı Azure Multi-Factor Authentication kullanması gerekir. MFA sunucusunu 1 Temmuz 'dan önce etkinleştiren müşteriler bir değişiklik görmez. Hala en son sürümü indirebilir, gelecekteki güncelleştirmeleri alabilir ve etkinleştirme kimlik bilgileri oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication sunucusu ile çalışmaya](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)başlama. Bulut tabanlı Azure Multi-Factor Authentication hakkında daha fazla bilgi için bkz. [bulut tabanlı Azure Multi-Factor Authentication dağıtımı planlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="may-2019"></a>Mayıs 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Hizmet değişikliği: Uygulama proxy 'Si hizmetindeki yalnızca TLS 1,2 protokolleri için gelecekteki destek

**Türüyle** Değişiklik planı  
**Hizmet kategorisi:** Uygulama Proxy'si  
**Ürün yeteneği:** Erişim Denetimi

Müşterilerimiz için en iyi sınıf şifrelemeyi sağlamaya yardımcı olmak için, yalnızca uygulama proxy 'Si hizmetindeki TLS 1,2 protokollerine erişimi sınırlıyoruz. Bu değişiklik, yalnızca TLS 1,2 protokollerini kullanan müşterilere yavaş bir şekilde dağıtılır, bu nedenle herhangi bir değişiklik göremezsiniz.

TLS 1,0 ve TLS 1,1 ' nin kullanımdan kaldırılması 31 Ağustos 2019 tarihinde yapılır, ancak bu değişikliğe hazırlanmak için zaman duyarsınız. Bu değişikliğe hazırlanmak için, kullanıcılarınızın uygulama proxy 'Si aracılığıyla yayınlanan uygulamalara erişmek için kullanacağı istemciler dahil olmak üzere, istemci-sunucu ve tarayıcı-sunucu birleşimlerinizin, uygulamayla bağlantıyı sürdürmek için TLS 1,2 protokolünü kullanacak şekilde güncelleştirildiğinden emin olun. Ara sunucu hizmeti. Daha fazla bilgi için bkz. [Azure Active Directory Içindeki uygulama proxy 'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin).

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Uygulamayla ilgili oturum açma verilerinizi görüntülemek için kullanım ve Öngörüler raporunu kullanın

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Artık aşağıdakiler hakkında bilgiler de dahil olmak üzere oturum açma verilerinizin uygulama merkezli bir görünümünü almak için Azure portal **Kurumsal uygulamalar** alanında bulunan kullanım ve Öngörüler raporunu kullanabilirsiniz:

- Kuruluşunuz için kullanılan en popüler uygulamalar

- En fazla başarısız oturum açma işlemleri içeren uygulamalar

- Her uygulama için ilk oturum açma hataları

Bu özellik hakkında daha fazla bilgi için [Azure Active Directory portalındaki Kullanım ve Öngörüler raporuna](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report) bakın

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Azure AD kullanarak bulut uygulamalarına Kullanıcı hazırlığını otomatikleştirin

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Aşağıdaki bulut tabanlı uygulamalar için Kullanıcı hesaplarının oluşturulmasını, silinmesini ve güncelleştirilmesini otomatik hale getirmek için Azure AD sağlama hizmetini kullanmak üzere bu yeni öğreticilerini izleyin:

- [Birlikte karşılayın](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [DynamicSignal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [Permi güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

Ayrıca, Grup nesneleri sağlama hakkında bilgi sağlayan bu yeni [Dropbox öğreticisini](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial)de izleyebilirsiniz.

Otomatik Kullanıcı hesabı sağlama aracılığıyla kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>Kimlik güvenli puanı artık Azure AD 'de kullanılabilir (genel kullanılabilirlik)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Yok  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Artık Azure AD 'de kimlik güvenli puanı özelliğini kullanarak kimlik güvenliğini izleyip geliştirebilirsiniz. Kimlik güvenli puanı özelliği, size yardımcı olması için tek bir pano kullanır:

- NesneKimliği, 1 ile 223 arasında bir puan temelinde kimlik güvenliği önlemidir.

- Kimlik güvenlik geliştirmelerinizi planlayın

- Güvenlik geliştirmelerinizin başarısını inceleyin

Kimlik güvenliği puanı özelliği hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik güvenli puanı nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score).

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Yeni Uygulama kayıtları deneyimi artık kullanıma sunuldu (genel kullanılabilirlik)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Geliştirici Deneyimi

Yeni [uygulama kayıtları](https://aka.ms/appregistrations) deneyimi artık genel kullanıma sunulmuştur. Bu yeni deneyim, Azure portal ve uygulama kayıt Portalı ' ndan bildiğiniz tüm önemli özellikleri içerir ve bunları aracılığıyla geliştirir:

- **Daha iyi uygulama yönetimi.** Uygulamalarınızı farklı portallarda görmek yerine, artık tüm uygulamalarınızı tek bir konumda görebilirsiniz.

- **Basitleştirilmiş uygulama kaydı.** Geliştirilmiş gezinti deneyiminden, yeniden kullanılabilir izin seçimi deneyimine yönelik olarak, uygulamalarınızı kaydetmek ve yönetmek artık daha kolay.

- **Daha ayrıntılı bilgi.** Hızlı başlangıç kılavuzlarınız ve daha fazlası dahil olmak üzere uygulamanız hakkında daha fazla ayrıntı bulabilirsiniz.

Daha fazla bilgi için bkz. [Microsoft Identity platform](https://docs.microsoft.com/azure/active-directory/develop/) ve [uygulama kayıtları deneyimi artık genel kullanıma sunuldu!](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) Blog duyurusu.

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Kimlik koruması için riskli kullanıcılar API 'sinde sunulan yeni yetenekler

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Identity Protection  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Kullanıcıların risk geçmişini almak, riskli kullanıcıları kapatmak ve kullanıcıları tehlikeye atılmak üzere doğrulamak için riskli kullanıcılar API 'sini artık kullanacağınızı duyurmaktan memnuniyet duyuyoruz. Bu değişiklik, kullanıcılarınızın risk durumunu daha verimli bir şekilde güncelleştirmenize ve risk geçmişini anlamanıza yardımcı olur.

Daha fazla bilgi için [riskli KULLANıCıLAR API başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mayıs 2019

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** 3. Taraf Tümleştirme

Mayıs 2019 ' de, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [gerçek bağlantılar](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kida](https://app.kianda.com/sso/OpenID/AzureAD/), [basit imza](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [BRAZE](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [displayr](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [templamy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo Sales](https://toutapp.com/login)katılım, [aclp](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [META4 Global HR](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), [hisse Çalışma alanı](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [Cobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [WebMethods API bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [redflag](https://pocketstop.com/redflag/), [whatdüzeltmesini](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [denetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [jobhub](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [neogov](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), pdee, [](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial) [myvr](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Azure AD portalında geliştirilmiş Grup oluşturma ve yönetim deneyimleri

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş Birliği

Azure AD portalındaki gruplarla ilgili deneyimler için geliştirmeler yaptık. Bu geliştirmeler, yöneticilerin grup listelerini, üye listelerini daha iyi yönetmesine ve ek oluşturma seçenekleri sağlamasına imkan tanır.

Geliştirmeler şunlardır:

- Üyelik türüne ve grup türüne göre temel filtreleme.

- Kaynak ve e-posta adresi gibi yeni sütunların eklenmesi.

- Kolay bir şekilde silinmek üzere grupları, üyeleri ve sahip listelerini çoklu seçme özelliği.

- Grup oluşturma sırasında e-posta adresi seçme ve sahipler ekleme özelliği.

Daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Azure AD portalındaki Office 365 grupları için adlandırma ilkesi yapılandırma (genel kullanılabilirlik)

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş Birliği

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kuralları uygulanmasını sağlar.

Office 365 grupları için adlandırma ilkesini iki farklı yolla yapılandırabilirsiniz:

- Otomatik olarak bir grup adına eklenen ön ekleri veya sonekleri tanımlayın.

- Kuruluşunuz için Grup adlarında izin verilmeyen özelleştirilmiş bir engellenen sözcük kümesini karşıya yükleyin (örneğin, "CEO, bordro, HR").

Daha fazla bilgi için bkz. [Office 365 grupları Için adlandırma Ilkesi zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Microsoft Graph API uç noktaları artık Azure AD etkinlik günlükleri (genel kullanılabilirlik) için kullanılabilir

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Raporlanıyor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlükleri için Microsoft Graph API uç noktası desteğinin genel kullanıma sunulduğunu duyurmaktan mutluluk duyuyoruz. Bu sürümle birlikte artık hem Azure AD denetim günlüklerinin hem de oturum açma günlüğü API 'Lerinin 1,0 sürümünü kullanabilirsiniz.

Daha fazla bilgi için bkz. [Azure AD denetim günlüğü API 'sine genel bakış](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0).

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Yöneticiler artık Birleşik kayıt işlemi (Genel Önizleme) için koşullu erişimi kullanabilir

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması  

Yöneticiler artık Birleşik kayıt sayfası tarafından kullanılmak üzere koşullu erişim ilkeleri oluşturabilir. Bu, şu durumlarda kayda izin vermek için ilkeleri uygulamayı içerir:

- Kullanıcılar güvenilir bir ağ üzerinde.

- Kullanıcılar, düşük bir oturum açma riskidir.

- Kullanıcılar yönetilen bir cihazlardır.

- Kullanıcılar kuruluşun kullanım koşulları 'nı (TOU) kabul etmiş olursunuz.

Koşullu erişim ve parola sıfırlama hakkında daha fazla bilgi için [Azure AD BIRLEŞIK MFA ve parola sıfırlama kayıt deneyimi blog gönderisine yönelik koşullu erişimi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348)görebilirsiniz. Birleşik kayıt işlemi için koşullu erişim ilkeleri hakkında daha fazla bilgi için bkz. [Birleşik kayıt Için koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration). Azure AD kullanım koşulları özelliği hakkında daha fazla bilgi için bkz. [kullanım koşulları özelliği Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

## <a name="april-2019"></a>2019 Nisan

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>Yeni Azure AD tehdit zekası algılaması artık Azure AD Kimlik Koruması bir parçası olarak sağlanıyor

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Azure AD Kimlik Koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Azure AD Threat Intelligence algılama artık güncelleştirilmiş Azure AD Kimlik Koruması özelliğinin bir parçası olarak sunulmaktadır. Bu yeni işlevsellik, Microsoft 'un dahili ve dış tehdit bilgileri kaynaklarına dayanan bilinen saldırı desenleriyle tutarlı olan belirli bir kullanıcı veya etkinliğin olağan dışı Kullanıcı etkinliklerini belirtmenize yardımcı olur.

Azure AD Kimlik Koruması yenilenen sürümü hakkında daha fazla bilgi için, bkz. [dört ana Azure AD kimlik koruması geliştirmesi artık genel önizleme](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) bloguna ve [ne Azure Active Directory kimlik koruması (YENİLENDİ)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) makalenin. Azure AD tehdit zekası algılaması hakkında daha fazla bilgi için [Azure Active Directory kimlik koruması risk olayları](https://docs.microsoft.com/azure/active-directory/identity-protection/risk-events-reference#azure-ad-threat-intelligence) makalesine bakın.

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Azure AD Yetkilendirme Yönetimi artık kullanıma sunuldu (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Identity Governance  
**Ürün yeteneği:** Identity Governance

Artık genel önizlemede olan Azure AD Yetkilendirme Yönetimi, müşterilerin, çalışanların ve iş ortaklarının erişim isteme, ne kadar süreceğine ve ne kadar erişebileceğini tanımlayan erişim paketleri yönetimini temsilcmalarına yardımcı olur. Erişim paketleri Azure AD ve Office 365 gruplarındaki üyeliği, kurumsal uygulamalardaki rol atamalarını ve SharePoint Online siteleri için rol atamalarını yönetebilir. Yetkilendirme Yönetimi hakkında daha fazla bilgi için [bkz. Azure AD Yetkilendirme Yönetimi](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview). Privileged Identity Management, erişim gözden geçirmeleri ve kullanım koşulları dahil Azure AD Identity Governance özellikleri hakkında daha fazla bilgi edinmek için bkz. [Azure AD Identity Governance nedir?](../governance/identity-governance-overview.md).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>Azure AD portalındaki Office 365 grupları için adlandırma ilkesi yapılandırma (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş Birliği

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kuralları uygulanmasını sağlar.

Office 365 grupları için adlandırma ilkesini iki farklı yolla yapılandırabilirsiniz:

- Otomatik olarak bir grup adına eklenen ön ekleri veya sonekleri tanımlayın.

- Kuruluşunuz için Grup adlarında izin verilmeyen özelleştirilmiş bir engellenen sözcük kümesini karşıya yükleyin (örneğin, "CEO, bordro, HR").

Daha fazla bilgi için bkz. [Office 365 grupları Için adlandırma Ilkesi zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure AD etkinlik günlükleri artık Azure Izleyici 'de kullanılabilir (genel kullanılabilirlik)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Raporlanıyor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlükleri ile görselleştirmelerle ilgili geri bildirimlerinizi ele almak için, Log Analytics yeni bir Öngörüler özelliği sunuyoruz. Bu özellik, çalışma kitapları adlı etkileşimli şablonlarımızı kullanarak Azure AD kaynaklarınız hakkında öngörüler elde etmenize yardımcı olur. Önceden oluşturulmuş bu çalışma kitapları, uygulamalar veya kullanıcılar için Ayrıntılar sağlayabilir ve şunları içerir:

- **Oturum açma işlemleri.** Uygulama ve kullanıcıların oturum açma konumu, kullanımda olan işletim sistemi veya tarayıcı istemcisi ve sürümü ve başarılı ya da başarısız oturum açma sayısı dahil olmak üzere ayrıntıları sağlar.

- **Eski kimlik doğrulama ve koşullu erişim.** Koşullu erişim ilkeleri tarafından tetiklenen çok faktörlü kimlik doğrulaması kullanımı, koşullu erişim ilkelerini kullanan uygulamalar vb. gibi eski kimlik doğrulaması kullanan uygulamalar ve kullanıcılar için ayrıntılar sağlar.

- **Oturum açma hatası analizi.** Kullanıcı eylemi, ilke sorunları veya altyapınız nedeniyle oturum açma hatalarınızın oluşup oluşmadığını belirlemenize yardımcı olur.

- **Özel raporlar.** Kuruluşunuz için Öngörüler özelliğini özelleştirmeye yardımcı olmak üzere yeni veya mevcut çalışma kitaplarını düzenleyebilirsiniz.

Daha fazla bilgi için bkz. [Azure izleyici çalışma kitaplarını Azure Active Directory raporları için kullanma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Nisan 2019

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** 3. Taraf Tümleştirme

2019 Nisan 'da, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [Hrçalışmalar çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [percogeç](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [Mobıoncontrol](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [benchling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [eğitibrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [rstudio Connect ](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba Cloud (rol tabanlı SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [cerkatlanmış hisse senedi yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [sectigo sertifika yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [tarzorbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [Monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [Gözetimi kurumsal](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [ındiggo](https://indiggolead.com/)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>Yeni erişim İncelemeleri sıklık seçeneği ve birden çok rol seçimi

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Erişim Gözden Geçirmeleri  
**Ürün yeteneği:** Identity Governance

Azure AD erişim gözden geçirmeleriyle yeni güncelleştirmeler şunları yapmanıza olanak sağlar:

- Daha önce haftalık, aylık, üç aylık ve yıllık olan mevcut seçeneklere ek olarak, erişim incelemelerinizin sıklığını **yarı yıllık**olarak değiştirin.

- Tek bir erişim incelemesi oluştururken birden çok Azure AD ve Azure Kaynak rolü seçin. Bu durumda, tüm roller aynı ayarlarla ayarlanır ve tüm gözden geçirenler aynı anda bilgilendirilir.

Erişim incelemesi oluşturma hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD Connect e-posta uyarı sistemleri geçiyor, bazı müşteriler için yeni e-posta gönderici bilgileri gönderiliyor

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** AD Eşitleme  
**Ürün yeteneği:** Platform

Azure AD Connect, e-posta uyarı sistemlerimizi geçiş sürecinde olduğundan, bazı müşterilere yeni bir e-posta gönderici gösteriliyor olabilir. Bunu çözmek için kuruluşunuzun izin verilenler listesine `azure-noreply@microsoft.com` eklemeniz gerekir veya Office 365, Azure veya eşitleme hizmetinizden önemli uyarıları almaya devam edemeyeceksiniz.

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>UPN son ek değişiklikleri artık Azure AD Connect Federasyon etki alanları arasında başarılı oluyor

**Türüyle** Sabit  
**Hizmet kategorisi:** AD Eşitleme  
**Ürün yeteneği:** Platform

Artık bir kullanıcının UPN sonekini, Azure AD Connect bir Federasyon etki alanından başka bir Federasyon etki alanına başarıyla değiştirebilirsiniz. Bu düzeltilme, eşitleme çevrimi sırasında FederatedDomainChangeError hata iletisine artık karşılaşmayabilmeniz veya bir bildirim e-postası almanız, "özniteliği nedeniyle bu nesne Azure Active Directory güncelleştirilemiyor. FederatedUser. UserPrincipalName] geçerli değil. Yerel dizin hizmetinizdeki değeri güncelleştirin.

Daha fazla bilgi için bkz. [eşitleme sırasında sorun giderme hataları](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror).

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Azure AD 'de uygulama koruma tabanlı koşullu erişim ilkesini kullanarak artırılmış güvenlik (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Uygulama koruma tabanlı koşullu erişim artık **Uygulama koruma Ilkesi gerektir** ilkesi kullanılarak kullanılabilir. Bu yeni ilke, aşağıdakileri önlemeye yardımcı olarak kuruluşunuzun güvenliğini artırmaya yardımcı olur:

- Kullanıcılar Microsoft Intune lisansı olmadan uygulamalara erişim hakkı elde edebilirler.

- Kullanıcılar Microsoft Intune uygulama koruma ilkesi alamıyor.

- Kullanıcılar, yapılandırılmış bir Microsoft Intune uygulama koruma ilkesi olmadan uygulamalara erişim hakkı elde edebilirler.

Daha fazla bilgi için bkz. [koşullu erişim ile Cloud App erişimi için uygulama koruma Ilkesi gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Microsoft Edge 'de Azure AD çoklu oturum açma ve koşullu erişim için yeni destek (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Azure AD çoklu oturum açma ve koşullu erişim için yeni destek sağlama dahil olmak üzere Microsoft Edge için Azure AD desteğimizi geliştirdik. Daha önce Microsoft Intune Managed Browser kullandıysanız, artık bunun yerine Microsoft Edge 'i kullanabilirsiniz.

Koşullu erişim kullanarak cihazlarınızı ve uygulamalarınızı ayarlama ve yönetme hakkında daha fazla bilgi için bkz. koşullu erişim [ile Cloud App erişimi için yönetilen cihazlar gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) ve [koşullu erişim ile Cloud App erişimi için onaylanmış istemci uygulamaları gerektirme ](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Microsoft Edge 'i Microsoft Intune ilkeleriyle kullanarak erişimin nasıl yönetileceği hakkında daha fazla bilgi için, bkz. [Microsoft Intune İlkeyle korunan tarayıcı kullanarak Internet erişimini yönetme](https://docs.microsoft.com/intune/app-configuration-managed-browser).

---

## <a name="march-2019"></a>Mart 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Azure Active Directory B2C 'da kimlik deneyimi çerçevesi ve özel ilke desteği artık kullanılabilir (GA)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

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

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** 3. Taraf Tümleştirme

2019 Mart 'da, uygulama galerisine federasyon desteği olan bu 14 yeni uygulamaları ekledik:

[ISEC7 mobil Exchange temsilcisi](https://www.isec7.com/english/), [mediusflow](https://office365.cloudapp.mediusflow.com/), [eplatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [fulcrud](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [excelityglobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Açıklama tabanlı denetim sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [yalın](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [powerokul performansı önemli](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [cincode ](https://cinode.com/), [Iris intraneti](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [tas](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Azure AD galerisinde yeni Zscaler ve Atlasser sağlama bağlayıcıları-Mart 2019

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün yeteneği:** 3. Taraf Tümleştirme

Aşağıdaki uygulamalar için Kullanıcı hesaplarını oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirin:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](https://aka.ms/ZscalerBetaProvisioning), [Zscaler One](https://aka.ms/ZscalerOneProvisioning), [Zscaler iki](https://aka.ms/ZscalerTwoProvisioning), [Zscaler üç](https://aka.ms/ZscalerThreeProvisioning), [Zscaler zscsesli](https://aka.ms/ZscalerZSCloudProvisioning), [atlasser bulutu](https://aka.ms/atlassianCloudProvisioning)

Otomatik Kullanıcı hesabı sağlama aracılığıyla kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://aka.ms/ProvisioningDocumentation).

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Azure AD portalındaki silinen Office 365 gruplarınızı geri yükleme ve yönetme

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş Birliği

Artık silinen Office 365 gruplarınızı Azure AD portalından görüntüleyebilir ve yönetebilirsiniz. Bu değişiklik, geri yükleme için hangi grupların kullanılabilir olduğunu görmenizi sağlar ve kuruluşunuzun ihtiyaç duymayan grupları kalıcı olarak silmenizi sağlar.

Daha fazla bilgi için bkz. [geri yükleme süre aşımına uğradı veya silinmiş gruplar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore).

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Çoklu oturum açma artık Azure AD SAML ile güvenli şirket içi uygulamalar için uygulama ara sunucusu aracılığıyla kullanılabilir (Genel Önizleme)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy'si  
**Ürün yeteneği:** Erişim Denetimi

Artık şirket içi, SAML kimliği doğrulanmış uygulamalar için çoklu oturum açma (SSO) deneyimi ve uygulama proxy 'Si aracılığıyla bu uygulamalara uzaktan erişim sağlayabilirsiniz. Şirket içi uygulamalarınızla SAML SSO 'yu ayarlama hakkında daha fazla bilgi için bkz. [uygulama proxy 'si (Önizleme) ile şirket içi uygulamalar Için SAML çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>İstek döngülerine yönelik istemci uygulamaları, güvenilirliği ve Kullanıcı deneyimini geliştirmek için kesintiye uğrar

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı Kimlik Doğrulaması

İstemci uygulamaları kısa bir süre boyunca aynı oturum açma isteklerini yanlışlıkla verebilir. Bu istekler, başarılı olsun veya etmeksizin, tüm kullanıcılar için zayıf bir kullanıcı deneyimine ve ıDP 'nin bu iş yüklerini artırarak, tüm kullanıcılar için gecikme süresini artırarak ve ıDP kullanılabilirliğini azalttığından.

Bu güncelleştirme bir `invalid_grant` hata gönderir: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request` normal işlem kapsamının ötesinde, kısa bir süre içinde birden çok kez yinelenen istekler veren istemci uygulamalarına. Bu sorunla karşılaşan istemci uygulamalarında, kullanıcının yeniden oturum açmasını gerektiren bir etkileşimli istem gösterilmelidir. Bu değişiklik ve bu hatayla karşılaştığında uygulamanızı nasıl gidereceğiniz hakkında daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted).

---

### <a name="new-audit-logs-user-experience-now-available"></a>Yeni denetim günlükleri Kullanıcı deneyimi artık kullanılabilir

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Raporlanıyor  
**Ürün yeteneği:** & Raporlamayı izleme

Okunabilirliği ve bilgilerinizi aramanızı artırmaya yardımcı olmak için yeni bir Azure AD **Denetim günlükleri** sayfası oluşturduk. Yeni **Denetim günlükleri** sayfasını görmek IÇIN Azure AD 'nin **etkinlik** bölümündeki **Denetim günlükleri** ' ni seçin.

![Yeni denetim günlükleri sayfası, örnek bilgi](media/whats-new/audit-logs-page.png)

Yeni **Denetim günlükleri** sayfası hakkında daha fazla bilgi için, bkz. [Azure Active Directory portalındaki denetim etkinliği raporları](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs).

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Yanlış yapılandırılmış koşullu erişim ilkeleriyle yanlışlıkla yöneticinin kilitlenmesini önlemeye yardımcı olacak yeni uyarılar ve kılavuzluk

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Yöneticilerin, yanlış yapılandırılmış koşullu erişim ilkeleri aracılığıyla kendi kiracılarından kendilerini yanlışlıkla kilitlemesini önlemeye yardımcı olmak için Azure portal yeni uyarılar ve güncelleştirilmiş rehberlik oluşturduk. Yeni rehberlik hakkında daha fazla bilgi için bkz. [koşullu erişim Azure Active Directory hizmet bağımlılıkları nelerdir](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Mobil cihazlarda geliştirilmiş son kullanıcı kullanım koşulları deneyimi

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** İdare

Mevcut kullanım koşullarımızı, bir mobil cihazda kullanım koşullarını nasıl gözden geçitireceğiniz ve kabul ettiğiniz hakkında daha fazla yardımcı olacak şekilde güncelleştirdik. Artık yakınlaştırıp uzaklaştırabilir, geri dönüp bilgileri indirebilir ve köprüler ' i seçebilirsiniz. Güncelleştirilmiş kullanım koşulları hakkında daha fazla bilgi için bkz. [kullanım koşulları özelliği Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users).

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Yeni Azure AD etkinlik günlükleri indirme deneyimi kullanılabilir

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Raporlanıyor  
**Ürün yeteneği:** & Raporlamayı izleme

Artık Azure portal çok miktarda etkinlik günlüğünü doğrudan indirebilirsiniz. Bu güncelleştirme şunları yapmanızı sağlar:

- En fazla 250.000 satır indirin.

- İndirme tamamlandıktan sonra bildirim alın.

- Dosya adınızı özelleştirin.

- JSON veya CSV çıkış biçiminizi belirleme.

Bu özellik hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Azure portal kullanarak bir denetim raporu indirin](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report)

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Hataya neden olan değişiklik: Exchange ActiveSync (EAS) tarafından koşul değerlendirmesi güncelleştirmeleri

**Türüyle** Değişiklik planı  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün yeteneği:** Erişim Denetimi

Exchange ActiveSync 'in (EAS) aşağıdaki koşulları nasıl değerlendirdiği hakkında güncelleştirme sürecimiz vardır:

- Ülke, bölge veya IP adresine göre Kullanıcı konumu

- Oturum açma riski

- Cihaz platformu

Bu koşulları daha önce koşullu erişim ilkeleriniz üzerinde kullandıysanız, koşul davranışının değişebilir olduğunu unutmayın. Örneğin, daha önce bir ilkede Kullanıcı konumu koşulunu kullandıysanız, ilkeyi artık kullanıcının konumuna göre atlandığını görebilirsiniz.

---

## <a name="february-2019"></a>Şubat 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Yapılandırılabilir Azure AD SAML belirteci şifrelemesi (Genel Önizleme) 

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** SSO

Artık, desteklenen herhangi bir SAML uygulamasını şifrelenmiş SAML belirteçleri alacak şekilde yapılandırabilirsiniz. Azure AD, bir uygulamayla yapılandırılıp kullanıldığında, Azure AD 'de depolanan bir sertifikadan alınan bir ortak anahtar kullanarak, verilmiş SAML onaylamalarını şifreler.

SAML belirteci şifrelemeyi yapılandırma hakkında daha fazla bilgi için bkz. [Azure AD SAML belirteci şifrelemesini yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleri kullanarak gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Erişim Gözden Geçirmeleri  
**Ürün yeteneği:** İdare

Artık grup üyeliği veya uygulama ataması için tek bir Azure AD erişim gözden geçirmesine birden çok grup veya uygulama ekleyebilirsiniz. Birden çok grup veya uygulama içeren erişim gözden geçirmeleri aynı ayarlarla ayarlanır ve tüm dahil olan gözden geçirenler aynı anda bildirilir.

Azure AD erişim gözden geçirmeleri kullanarak erişim incelemesi oluşturma hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle grup veya uygulama erişimi Incelemesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Şubat 2019

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün yeteneği:** 3. Taraf Tümleştirme
 
Şubat 2019 ' de, uygulama galerisine federasyon desteği olan bu 27 yeni uygulamayı ekledik:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [Mindticyon](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT FINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [airstack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [ufuk Ward qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [parlak tidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [alerar](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight-cloudgate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), İzin tıklama, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [storegatesmartfile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [pexıp](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [seismik](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [bir rüt](https://www.shareadream.org/how-it-works), [bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [WebMethods Integration Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [bilgi Her yerde LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope verileri](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [NetOp Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [Smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [Purecyüksek, Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [tıklama üretkenlik platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Azure AD uygulama galerisinde uygulamanızı listeleme hakkında daha fazla bilgi için bkz. [uygulamanızı Azure Active Directory Uygulama galerisinde listeleyin](https://aka.ms/azureadapprequest).

---

### <a name="enhanced-combined-mfasspr-registration"></a>Geliştirilmiş Birleşik MFA/SSPR kaydı

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** Self Servis Parola Sıfırlama  
**Ürün yeteneği:** Kullanıcı Kimlik Doğrulaması

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

**Türüyle** Değişen özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Kullanıcı akışları (daha önce, yerleşik ilkeler) için ilke oluşturma ve yönetim işlemini daha kolay bir şekilde güncelleştirdik. Bu yeni deneyim artık tüm Azure AD kiracılarınız için varsayılandır.

Portal ekranının üst kısmındaki **bize geri bildirim gönder** alanındaki gülümseme veya kaş simgeleri kullanarak ek geri bildirim ve öneriler sağlayabilirsiniz.

Yeni ilke yönetimi deneyimi hakkında daha fazla bilgi için, [şimdi Azure AD B2C JavaScript özelleştirmesine ve birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahiptir.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Azure AD B2C tarafından sunulan belirli sayfa öğesi sürümlerini seçin

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Artık, Azure AD B2C tarafından sunulan sayfa öğelerinin belirli bir sürümünü seçebilirsiniz. Belirli bir sürümü seçerek, güncelleştirmelerinizi bir sayfada görüntülenmeden önce test edebilir ve öngörülebilir bir davranış sağlayabilirsiniz. Ayrıca, artık JavaScript özelleştirmeleriyle ilgili belirli sayfa sürümlerini zorunlu kılabilirsiniz. Bu özelliği açmak için Kullanıcı akışlarınızda **Özellikler** sayfasına gidin.

Sayfa öğelerinin belirli sürümlerini seçme hakkında daha fazla bilgi için, [artık Azure AD B2C JavaScript özelleştirmesine ve birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahiptir.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>B2C için yapılandırılabilir Son Kullanıcı parolası gereksinimleri (GA)

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Artık kuruluşunuzun parola karmaşıklığını, yerel Azure AD parola ilkenizi kullanmak yerine son kullanıcılarınız için ayarlayabilirsiniz. Kullanıcı akışlarınızın **Özellikler** dikey penceresinde (önceki adıyla yerleşik ilkeleriniz), **basit** veya **güçlü**bir parola karmaşıklığı seçebilir veya **özel** bir gereksinim kümesi oluşturabilirsiniz.

Parola karmaşıklığı gereksinim yapılandırması hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C parolalar için karmaşıklık gereksinimlerini yapılandırma](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Özel markalı kimlik doğrulama deneyimleri için yeni varsayılan şablonlar

**Türüyle** Yeni özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Kullanıcılarınız için özel bir markalı kimlik doğrulama deneyimi oluşturmak üzere Kullanıcı akışlarınızın **sayfa düzenleri** dikey penceresinde (daha önce yerleşik ilkeler olarak bilinir) bulunan yeni varsayılan şablonlarımızı kullanabilirsiniz.

Şablonları kullanma hakkında daha fazla bilgi için bkz. [Azure AD B2C artık JavaScript özelleştirmesi ve birçok yeni özellik içeriyor](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595).

---
