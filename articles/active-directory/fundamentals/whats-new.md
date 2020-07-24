---
title: Yenilikler Sürüm notları-Azure Active Directory | Microsoft Docs
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevler ve yaklaşan değişiklikler gibi Azure Active Directory yenilikleri öğrenin.
services: active-directory
author: msaburnley
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ac87bda4251a8414ef659ad486c989c0c1cd42e1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87034729"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory yenilikler nelerdir?

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` ![ RSS akışı okuyucu simgesi ](./media/whats-new/feed-icon-16x16.png) akış okuyucusuna.

Azure AD, iyileştirmeleri sürekli olarak alır. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik
- Değişiklik planları

Bu sayfa aylık olarak güncelleştirildiğinden düzenli olarak yeniden ziyaret edin. Altı aydan eski olan öğelere bakıyorsanız, [Azure Active Directory yenilikler Için arşivde](whats-new-archive.md)bulabilirsiniz.

---

## <a name="june-2020"></a>Haziran 2020 

### <a name="user-risk-condition-in-conditional-access-policy"></a>Koşullu erişim ilkesinde Kullanıcı risk koşulu

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 

Azure AD koşullu erişim ilkesinde Kullanıcı risk desteği, birden çok Kullanıcı risk tabanlı ilke oluşturmanıza olanak sağlar. Farklı kullanıcılar ve uygulamalar için farklı en düşük Kullanıcı risk düzeyleri gerekli olabilir. Kullanıcı riskine bağlı olarak, erişimi engellemek, Multi-Factor Authentication, güvenli parola değişikliği yapmak veya ek denetim gibi oturum ilkesini zorlamak için Microsoft Cloud App Security yeniden yönlendirmek üzere ilkeler oluşturabilirsiniz.

Kullanıcı risk koşulu, bir P2 teklifi olan Azure kimlik koruması kullandığından Azure AD Premium P2 gerektirir. Koşullu erişim hakkında daha fazla bilgi için [Azure AD koşullu erişim belgelerine](https://docs.microsoft.com/azure/active-directory/conditional-access/)bakın.

---

### <a name="saml-sso-now-supports-apps-that-require-spnamequalifier-to-be-set-when-requested"></a>SAML SSO artık istendiğinde, SPNameQualifier 'nin ayarlanması gereken uygulamaları destekliyor

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Bazı SAML uygulamaları, istendiğinde onay konusunun döndürülmesini gerektirir. İstek NameID ilkesinde bir SPNameQualifier istendiğinde Azure AD doğru şekilde yanıt veriyor. Bu, SP tarafından başlatılan oturum açma için de çalışır ve IDP tarafından başlatılan oturum açma işlemi uygulanır.  Azure Active Directory 'de SAML protokolü hakkında daha fazla bilgi edinmek için bkz. [Çoklu oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol).

---

### <a name="azure-ad-b2b-collaboration-supports-inviting-msa-and-google-users-in-azure-government-tenants"></a>Azure AD B2B Işbirliği, Azure Kamu kiracılarında MSA ve Google kullanıcılarını davet etme desteği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 

B2B işbirliği özelliklerini kullanan Azure Kamu kiracılar artık Microsoft veya Google hesabı olan kullanıcıları davet edebilir. Kiracınızın bu özellikleri kullanıp kullanmızdan emin olmak için, [Azure ABD kamu kiracımda B2B işbirliğinin kullanılabilir olup olmadığını nasıl anlayabilirim?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant) konusundaki yönergeleri izleyin.

 
---
 
### <a name="user-object-in-ms-graph-v1-now-includes-externaluserstate-and-externaluserstatechangeddatetime-properties"></a>MS Graph v1 'deki Kullanıcı nesnesi artık externalUserState ve externalUserStateChangedDateTime özelliklerini içerir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 

ExternalUserState ve externalUserStateChangedDateTime özellikleri, davetlerini henüz kabul etmemiş olan davet edilen B2B konuklarının yanı sıra bazı gün sayısından sonra davetlerini kabul edilmeyen kullanıcıları silme gibi derleme Otomasyonu 'nu bulmak için de kullanılabilir. Bu özellikler artık MS Graph v1 'de kullanılabilir. Bu özellikleri kullanma hakkında rehberlik için [Kullanıcı kaynak türü](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0)' ne bakın.
 
---

### <a name="manage-authentication-sessions-in-azure-ad-conditional-access-is-now-generally-available"></a>Azure AD koşullu erişim 'de kimlik doğrulama oturumlarını yönetme genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Kimlik doğrulama oturumu yönetimi özellikleri, kullanıcılarınızın ortamınızda daha fazla güvenlik ve esneklik sunmak için oturum açma kimlik bilgilerini ve tarayıcıları kapatıp yeniden açtıktan sonra kimlik bilgileri sağlamaları gerekip gerekmediğini yapılandırmanıza olanak tanır.
 
Ayrıca, kimlik doğrulama oturumu yönetimi yalnızca Azure AD 'ye katılmış, hibrit Azure AD 'ye katılmış ve Azure AD 'ye kayıtlı cihazlarda yalnızca birinci faktör kimlik doğrulamasına uygulamak için kullanılır. Artık, kimlik doğrulama oturumu yönetimi MFA için de geçerlidir. Daha fazla bilgi için bkz. [koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---june-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Haziran 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Haziran 2020 ' de, federasyon desteğiyle App Gallery 'de aşağıdaki 29 yeni uygulamayı ekledik:

[Shopify Plus](https://docs.microsoft.com/azure/active-directory/saas-apps/shopify-plus-tutorial), [Ekarda](https://docs.microsoft.com/azure/active-directory/saas-apps/ekarda-tutorial), [MailGates](https://docs.microsoft.com/azure/active-directory/saas-apps/mailgates-tutorial), [BullseyeTDP](https://docs.microsoft.com/azure/active-directory/saas-apps/bullseyetdp-tutorial), [finketa](https://docs.microsoft.com/azure/active-directory/saas-apps/raketa-tutorial), [segment](https://docs.microsoft.com/azure/active-directory/saas-apps/segment-tutorial), [AI denetleyicisi](https://www.mindbridge.ai/products/ai-auditor/), [Pobuca Connect](https://app.pobu.ca/), [proto.io](https://docs.microsoft.com/azure/active-directory/saas-apps/proto.io-tutorial), [Gatekeeper](https://www.gatekeeperhq.com/), [Merkez planlayıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/hub-planner-tutorial), [ansıra-iş ortağı go-to-market](https://ansira.com/technology/channel-engagement), [bulutta IBM dijital iş Otomasyonu](https://docs.microsoft.com/azure/active-directory/saas-apps/ibm-digital-business-automation-on-cloud-tutorial), [kişi fiziksel güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/kisi-physical-security-tutorial), [viewpointon](https://team.viewpoint.com/), [ıntelligencebank](https://docs.microsoft.com/azure/active-directory/saas-apps/intelligencebank-tutorial), [pyölçümler](https://docs.microsoft.com/azure/active-directory/saas-apps/pymetrics-tutorial), [sıfır](https://www.teamzero.com/), [ınstation](https://instation.invillia.com/), [EDX for Business SAML 2,0 tümleştirmesi](https://docs.microsoft.com/azure/active-directory/saas-apps/edx-for-business-saml-integration-tutorial), [MOOC Office 365](https://mooc.office365-training.com/en/), [SmartKargo](https://docs.microsoft.com/azure/active-directory/saas-apps/smartkargo-tutorial), [pkııgmpplatform](https://platform.pkisigning.nl/), [siteıntel](https://docs.microsoft.com/azure/active-directory/saas-apps/siteintel-tutorial), [alan kimliği](https://docs.microsoft.com/azure/active-directory/saas-apps/field-id-tutorial), [Curricula SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/curricula-saml-tutorial), [Perforce Helix Core-Helix Authentication Service](https://docs.microsoft.com/azure/active-directory/saas-apps/perforce-helix-core-tutorial), [myuyumluluk bulutu](https://cloud.metacompliance.com/), [smallstep SSH](https://smallstep.com/sso-ssh/)  

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz https://aka.ms/AppsTutorial . Uygulamanızı Azure AD uygulama galerisinde listelemek için lütfen buradaki ayrıntıları okuyun: https://aka.ms/AzureADAppRequest .

---

### <a name="api-connectors-for-external-identities-self-service-sign-up-are-now-in-public-preview"></a>Dış kimlikler self servis kaydolma için API bağlayıcıları artık genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Dış kimlikler API bağlayıcıları, self servis kaydolma 'yı dış bulut sistemleriyle tümleştirmenizi sağlayan Web API 'Lerinden yararlanmanızı sağlar. Bu, artık bulut tabanlı özel iş akışlarını tetiklemek için bir kayıt akışında özel adımlar olarak Web API 'Leri çağırabileceğiniz anlamına gelir. Örneğin, API bağlayıcılarını kullanarak şunları yapabilirsiniz:

- Özel bir onay iş akışlarıyla tümleştirin.
- Kimlik doğrulama gerçekleştir
- Kullanıcı giriş verilerini doğrulama
- Kullanıcı özniteliklerinin üzerine yaz
- Özel iş mantığını Çalıştır

API bağlayıcılarıyla ilgili tüm deneyimler hakkında daha fazla bilgi için bkz. [API bağlayıcılarını özelleştirmek ve genişletmek için API bağlayıcıları kullanma](https://docs.microsoft.com/azure/active-directory/b2b/api-connectors-overview)veya [Web API 'si tümleştirmelerine sahip dış kimlikler self servis kaydolma](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-external-identities-self-service-sign-up-with-web-api/ba-p/1257364#.XvNz2fImuQg.linkedin).
 
---

### <a name="provision-on-demand-and-get-users-into-your-apps-in-seconds"></a>İsteğe bağlı sağlayın ve uygulamaları saniyeler içinde alın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure AD sağlama hizmeti şu anda döngüsel olarak çalışır. Hizmet, her 40 dakikada bir çalışır. [İsteğe bağlı sağlama özelliği](https://aka.ms/provisionondemand) , bir Kullanıcı seçmenizi ve saniyeler içinde sağlamanızı sağlar. Bu özellik, sağlama döngüsünün yeniden başlatılmasına zorlamak için yeniden başlatma işlemi yapmanıza gerek kalmadan sağlama sorunlarını hızla giderebilmenizi sağlar. 
 
---

### <a name="new-permission-for-using-azure-ad-entitlement-management-in-graph"></a>Graph 'ta Azure AD Yetkilendirme Yönetimi 'ni kullanmaya yönelik yeni izin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Yetkilendirme Yönetimi
 
Yeni bir Temsilcili izin EntitlementManagement. Read. All artık Microsoft Graph Beta 'da yetkilendirme yönetimi API 'SI ile kullanılabilir. Kullanılabilir API 'Ler hakkında daha fazla bilgi edinmek için bkz. [Azure AD yetkilendirme yönetimi API 'si Ile çalışma](https://docs.microsoft.com/graph/api/resources/entitlementmanagement-root?view=graph-rest-beta).

---

### <a name="identity-protection-apis-available-in-v10"></a>V 1.0 'da kimlik koruması API 'Leri kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
RiskyUsers ve riskDetections Microsoft Graph API 'Leri artık genel kullanıma sunulmuştur. Artık v 1.0 uç noktasında kullanılabilir olduğuna göre, bunları üretimde kullanmak için davet ediyoruz. Daha fazla bilgi için lütfen [Microsoft Graph belgelerine](https://docs.microsoft.com/graph/api/resources/identityprotectionroot?view=graph-rest-1.0)göz atın.
 
---

### <a name="sensitivity-labels-to-apply-policies-to-microsoft-365-groups-is-now-generally-available"></a>Microsoft 365 gruplara ilke uygulamak için duyarlılık etiketleri genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş
 

Artık duyarlılık etiketleri oluşturabilir ve Gizlilik (genel veya özel) ve dış Kullanıcı erişimi ilkesi gibi Microsoft 365 gruplarına ilke uygulamak için etiket ayarlarını kullanabilirsiniz. Gizli olması için Gizlilik ilkesiyle birlikte bir etiket ve Konuk kullanıcılar eklemeye izin vermek için ise harici Kullanıcı erişim ilkesi oluşturabilirsiniz. Bir Kullanıcı bu etiketi bir gruba uygularsa, Grup özel olur ve gruba hiçbir Konuk kullanıcının eklenmesine izin verilmez. 

Duyarlılık etiketleri, iş açısından kritik verilerinizi korumak ve grupları uyumlu ve güvenli bir biçimde yönetmenize olanak tanımak için önemlidir. Duyarlılık etiketlerini kullanma hakkında yönergeler için [Azure Active Directory (Önizleme) Içinde Office 365 gruplarına duyarlılık etiketleri atama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-assign-sensitivity-labels)bölümüne bakın.
 
---

### <a name="updates-to-support-for-microsoft-identity-manager-for-azure-ad-premium-customers"></a>Azure AD Premium müşterileri için Microsoft Identity Manager Destek güncelleştirmeleri

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Microsoft Identity Manager  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure desteği, Microsoft Identity Manager 2016 için genişletilmiş destek sonuna kadar Microsoft Identity Manager 2016 ' nin Azure AD tümleştirme bileşenleri ile sunulmaktadır. [Microsoft Identity Manager kullanarak Azure AD Premium müşterileri Için destek güncelleştirmesinde](https://docs.microsoft.com/microsoft-identity-manager/support-update-for-azure-active-directory-premium-customers)daha fazla bilgi edinin.

---

### <a name="the-use-of-group-membership-conditions-in-sso-claims-configuration-is-increased"></a>SSO talep yapılandırmasındaki grup üyeliği koşullarının kullanımı artırıldı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Daha önce, tek bir uygulama yapılandırması içindeki grup üyeliğine göre talepleri koşullu olarak değiştirdiğinizde kullanabileceğiniz grupların sayısı 10 ile sınırlıdır. SSO talep yapılandırmasındaki grup üyeliği koşullarının kullanılması artık maksimum 50 gruba yükselmiştir. Talepleri yapılandırma hakkında daha fazla bilgi için bkz. [Kurumsal uygulamalar SSO talep yapılandırması](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization#emitting-claims-based-on-conditions). 

---

### <a name="enabling-basic-formatting-on-the-sign-in-page-text-component-in-company-branding"></a>Şirket markalaması 'nda oturum açma sayfası metin bileşeninde temel biçimlendirmeyi etkinleştirme.

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD/Microsoft 365 oturum açma deneyiminde Şirket markası işlevselliği, müşterinin kalın yazı tipi, altı çizili ve italik gibi köprü ve basit biçimlendirme eklemesine izin verecek şekilde güncelleştirilmiştir. Bu işlevi kullanma hakkında yönergeler için bkz. [kuruluşunuzun Azure Active Directory oturum açma sayfasına marka ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="provisioning-performance-improvements"></a>Performans iyileştirmeleri sağlama

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Sağlama Hizmeti, [artımlı bir döngüyü](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works#incremental-cycles) tamamlamaya yönelik süreyi azaltacak şekilde güncelleştirilmiştir. Bu, kullanıcıların ve grupların uygulamalarına daha hızlı sağlandığı anlamına gelir. 6/10/2020 sonra oluşturulan tüm yeni sağlama işleri, performans iyileştirmelerinden otomatik olarak faydalanır. Performans iyileştirmelerinden faydalanmak için 6/10/2020 ' den önce sağlama için yapılandırılmış tüm uygulamaların, 6/10/2020 ' den sonra yeniden başlatılması gerekir. 

---

### <a name="announcing-the-deprecation-of-adal-and-ms-graph-parity"></a>ADAL ve MS Graf eşliği kullanımdan kaldırılması duyurusu

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Cihaz yaşam döngüsü yönetimi

Artık Microsoft kimlik doğrulama kitaplıkları (MSAL) kullanılabilir olduğuna göre, artık Azure Active Directory kimlik doğrulama kitaplıklarına (ADAL) yeni özellikler ekleyeceğiz ve 30 Haziran 2022 ' de güvenlik düzeltme eklerini sona erdirmek için. MSAL ' ye geçiş hakkında daha fazla bilgi için bkz. [uygulamaları Microsoft kimlik doğrulama kitaplığı 'Na geçirme (msal)](https://docs.microsoft.com/azure/active-directory/develop/msal-migration).

Ayrıca, tüm Azure AD Graph işlevlerini MS Graph aracılığıyla kullanılabilir hale getirmek için çalışmayı tamamladık. Bu nedenle, Azure AD Graph API 'Leri 30 Haziran 2022 ' e kadar yalnızca bugdüzeltmesini ve güvenlik düzeltmelerini alacak. Daha fazla bilgi için bkz. [Microsoft kimlik doğrulama kitaplığı ve apı Microsoft Graph kullanmak için uygulamalarınızı güncelleştirme](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363)
 
---
 
## <a name="may-2020"></a>Mayıs 2020

### <a name="retirement-of-properties-in-signins-riskyusers-and-riskdetections-apis"></a>SignIns, riskyUsers ve riskDetections API 'Lerinde özelliklerin kullanımdan kaldırılması

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Şu anda numaralandırılmış türler, riskType özelliğini hem riskDetections API hem de Riskyuseriztem (önizlemede) içinde temsil etmek için kullanılır. Numaralandırılmış türler Ayrıca signIns API 'sindeki riskEventTypes özelliği için de kullanılır. İleri giderek bu özellikleri dizeler olarak temsil edeceğiz. 

Müşteriler Beta riskDetections ve Riskyuseriztem API 'sindeki riskEventType özelliğine ve 9 Eylül 2020 ' e kadar Beta signIns API 'sindeki riskEventTypes_v2 özelliğine geçiş yapılmalıdır. Bu tarihte, geçerli riskType ve riskEventTypes özelliklerini devre dışı bırakılacağız. Daha fazla bilgi için [Microsoft Graph risk olayı özelliklerine ve kimlik koruması API 'lerine yönelik değişikliklere](https://developer.microsoft.com/graph/blogs/changes-to-risk-event-properties-and-identity-protection-apis-on-microsoft-graph/)bakın.

--- 

### <a name="deprecation-of-riskeventtypes-property-in-signins-v10-api-on-microsoft-graph"></a>SignIns v 1.0 API 'sindeki riskEventTypes özelliğinin kullanımdan kaldırılması Microsoft Graph

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Numaralandırılmış türler Microsoft Graph Eylül 2020 ' deki risk olayı özelliklerini temsil edildiğinde dize türlerine geçiş yapar. Bu değişiklik, önizleme API 'Lerini etkilemenin yanı sıra üretim içi signIns API 'sini de etkileyecektir.

SignIns v 1.0 API 'sine yeni bir riskEventsTypes_v2 (String) özelliği ekledik. Geçerli riskEventTypes (enum) özelliğini 11 Haziran 2022 ' de kullanımdan kaldıracağız ve Microsoft Graph kullanım dışı bırakma ilkenize uygun şekilde devre dışı bırakacağız. Müşteriler, 11 Haziran 2022 ' e kadar v 1.0 signIns API 'sindeki riskEventTypes_v2 özelliğine geçiş yapılmalıdır. Daha fazla bilgi için [Microsoft Graph signIns v 1.0 API 'Sindeki riskEventTypes özelliğinin kullanımdan kaldırılması](https://developer.microsoft.com/graph/blogs/deprecation-of-riskeventtypes-property-in-signins-v1-0-api-on-microsoft-graph//)bölümüne bakın.

--- 

### <a name="upcoming-changes-to-mfa-email-notifications"></a>MFA e-posta bildirimlerinde yaklaşan değişiklikler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 

Cloud MFA için e-posta bildirimlerinde aşağıdaki değişiklikleri yapıyoruz:

E-posta bildirimleri şu adresten gönderilir: azure-noreply@microsoft.com ve msonlineservicesteam@microsoftonline.com . Dolandırıcıların engellemesini kaldırmak için gerekli adımları daha iyi göstermek üzere sahtekarlık uyarı e-postalarının içeriğini güncelleştiriyoruz.

---

### <a name="new-self-service-sign-up-for-users-in-federated-domains-who-cant-access-microsoft-teams-because-they-arent-synced-to-azure-active-directory"></a>Azure Active Directory eşitlenmediği için, Microsoft ekiplerine erişemeyen Federasyon etki alanlarında bulunan kullanıcılar için yeni self servis kaydolun.

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 

Şu anda, Azure AD 'de Federasyon etki alanlarında bulunan ancak kiracı ile eşitlenmemiş olan kullanıcılar takımlara erişemez. Haziran 'un sonundan itibaren, bu yeni özellik, var olan e-posta doğrulanmış kaydolma özelliğini genişleterek bunu yapabilmesini sağlayacak. Bu, Federasyon IDP 'de oturum açabilen ancak henüz Azure KIMLIĞINDE bir kullanıcı nesnesine sahip olmayan ve takımlar için otomatik olarak oluşturulmuş bir kullanıcı nesnesine sahip olmayan kullanıcılara izin verir. Kullanıcı nesneleri, "self servis kaydolma" olarak işaretlenir. Bu, yönetilen etki alanındaki kullanıcıların yapabileceği ve aynı bayrak kullanılarak denetlenebileceği, var olan bir e-posta doğrulama özelliğinin bir uzantısıdır. Bu değişiklik, aşağıdaki iki ay boyunca kullanıma alınır. Belge güncelleştirmelerini [buradan](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-self-service-signup)izleyin.
 
---

### <a name="upcoming-fix-the-oidc-discovery-document-for-the-azure-government-cloud-is-being-updated-to-reference-the-correct-graph-endpoints"></a>Yaklaşan Düzeltme: Azure Kamu Bulutu için OıDC keşif belgesi, doğru grafik uç noktalarına başvuracak şekilde güncelleştiriliyor.

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Bağımsız bulutlar  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Haziran 'dan itibaren, [Azure Kamu Bulutu](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud) uç noktasındaki (login.microsoftonline.us) OIDC keşif belgesi [Microsoft Identity platform ve OpenID Connect Protokolü](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc) , doğru [National Cloud Graph](https://docs.microsoft.com/graph/deployments) uç noktasını ( https://graph.microsoft.us veya https://dod-graph.microsoft.us) belirtilen kiracıya göre) döndürmeye başlayacaktır.  Şu anda hatalı grafik uç noktası (graph.microsoft.com) "msgraph_host" alanı sağlar.  

Bu hata düzeltmesinin süresi yaklaşık 2 aydan fazla olacak şekilde alınacaktır.  

---

### <a name="azure-government-users-will-no-longer-be-able-to-sign-in-on-loginmicrosoftonlinecom"></a>Azure Kamu kullanıcıları artık login.microsoftonline.com üzerinde oturum açamaz

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Bağımsız bulutlar  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
1 Haziran 2018 ' de, Azure Kamu için resmi Azure Active Directory (AAD) yetkilisi ' den ' a değiştirilir https://login-us.microsoftonline.com https://login.microsoftonline.us . Bir Azure Kamu kiracısı içindeki bir uygulamaya sahipseniz, uygulamanızı. us uç noktasındaki kullanıcıları imzalamak için güncelleştirmeniz gerekir.

Azure AD, Mayıs 5 ' ten itibaren, Azure Kamu kullanıcılarının genel uç nokta (microsoftonline.com) kullanılarak Azure Kamu kiracılarında barındırılan uygulamalarda oturum açmasını engelleyen bitiş noktası değişikliğini zorlamaya başlayacak. Etkilenen uygulamalar, AADSTS900439-USGClientNotSupportedOnPublicEndpoint hatasını görmeye başlayacaktır. 

Bu değişikliğin, Haziran 2020 ' de tüm uygulamalarda tamamlanamamış olması beklendiğinden aşamalı olarak bir dağıtım olması gerekir. Daha fazla ayrıntı için lütfen [Azure Kamu blog gönderisine](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/)bakın.

---

### <a name="saml-single-logout-request-now-sends-nameid-in-the-correct-format"></a>SAML çoklu oturum kapatma isteği artık NameID 'yi doğru biçimde gönderiyor

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kullanıcı, oturum kapatma (örneğin, Uygps portalındaki) oturumunu tıklattığında, Azure AD kullanıcı oturumunda etkin olan ve oturum kapatma URL 'SI yapılandırılmış her uygulamaya bir SAML Single Logout iletisi gönderir. Bu iletiler kalıcı biçimde bir NameID içerir.

Özgün SAML oturum açma belirteci, NameID (örn. e-posta/UPN) için farklı bir biçim kullanıyorsa, SAML uygulaması oturum kapatma iletisindeki NameID 'yi mevcut bir oturumla ilişkilendiremiyor (her iki iletide kullanılan Nameıds farklı olduğu için), bu da oturum kapatma iletisinin SAML uygulaması ve Kullanıcı oturum açmış durumda kalmasını sağlar. Bu onarım, oturum kapatma iletisinin uygulama için yapılandırılan NameID ile tutarlı olmasını sağlar.

---

### <a name="hybrid-identity-administrator-role-is-now-available-with-cloud-provisioning"></a>Karma kimlik yöneticisi rolü artık bulut sağlaması ile kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD bulutu sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
BT yöneticileri yeni "karma yönetici" rolünü, Azure ADConnect bulut sağlamasını ayarlamaya yönelik en düşük ayrıcalıklı rol olarak kullanmaya başlayabilir. Bu yeni rolle, artık bulut sağlamasını ayarlamak ve yapılandırmak için genel yönetici rolünü kullanmanız gerekmez. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-delegate-by-task#connect).
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---may-2020"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mayıs 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2020 Mayıs 'ta, federasyon desteğiyle uygulama galerimize aşağıdaki 36 yeni uygulamaları ekledik:

[Moula](https://moula.com.au/pay/merchants), [gözetimi ypal](https://www.surveypal.com/app), [Kbot365](https://www.konverso.ai/virtual-assistant-digital-workplace/), [tacklebox](http://www.tacklebox.app/), [Powell ekipleri](https://powell-software.com/en/powell-teams-en/), [talentsoft Yardımcısı](https://msteams.talent-soft.com/), [ASC kayıt öngörüleri](https://teams.asc-recording.app/product), [GO1](https://www.go1.com/), [B-bağlı](https://b-engaged.se/), [Competella Contact Center çalışma grubu](http://www.competella.com/), [asite](http://www.asite.com/), [ımavals kimliği](https://identity.imagesoftinc.com/), [ibısworld](https://identity.imagesoftinc.com/), [ınsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/insuite-tutorial), [değişiklik işlemi yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/change-process-management-tutorial), [Cyara CX güvence platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/cyara-cx-assurance-platform-tutorial), [Smart Global idare](https://docs.microsoft.com/azure/active-directory/saas-apps/smart-global-governance-tutorial), [PREZI](https://docs.microsoft.com/azure/active-directory/saas-apps/prezi-tutorial), [Mapbox](https://docs.microsoft.com/azure/active-directory/saas-apps/mapbox-tutorial), [datava Enterprise Service platform](https://docs.microsoft.com/azure/active-directory/saas-apps/datava-enterprise-service-platform-tutorial), [Whimsical](https://docs.microsoft.com/azure/active-directory/saas-apps/whimsical-tutorial), [trelica](https://docs.microsoft.com/azure/active-directory/saas-apps/trelica-tutorial), [Tsysso](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-confluence-tutorial)for the [Bitbucket](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-bitbucket-tutorial)için easysso, [Bamboo](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-bamboo-tutorial), [Torii](https://docs.microsoft.com/azure/active-directory/saas-apps/torii-tutorial), [axiad bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/axiad-cloud-tutorial), [humanage](https://docs.microsoft.com/azure/active-directory/saas-apps/humanage-tutorial), [colortokens ztna](https://docs.microsoft.com/azure/active-directory/saas-apps/colortokens-ztna-tutorial), [CCH Tagetik](https://docs.microsoft.com/azure/active-directory/saas-apps/cch-tagetik-tutorial), [sharekasa,](https://docs.microsoft.com/azure/active-directory/saas-apps/sharevault-tutorial) [Vyond](https://docs.microsoft.com/azure/active-directory/saas-apps/vyond-tutorial), [textexpander](https://docs.microsoft.com/azure/active-directory/saas-apps/textexpander-tutorial), bir [Ev CRM](https://docs.microsoft.com/azure/active-directory/saas-apps/anyone-home-crm-tutorial) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/ice-contact-center-tutorial) [askbağlı](https://docs.microsoft.com/azure/active-directory/saas-apps/askspoke-tutorial)

Ayrıca, tüm uygulamaların belgelerini buradan bulabilirsiniz https://aka.ms/AppsTutorial .

Uygulamanızı Azure AD uygulama galerisinde listelemek için lütfen buradaki ayrıntıları okuyun https://aka.ms/AzureADAppRequest .

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>Koşullu erişim için yalnızca rapor modu genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

[Azure AD koşullu erişim için yalnızca rapor modu](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) , erişim denetimlerini zorlamadan bir ilkenin sonucunu değerlendirmenizi sağlar. Kuruluşunuzda yalnızca rapor ilkelerini test edebilir ve etkinleştirilmeden önce etkilerini anlayabilmeniz, dağıtımın daha güvenli ve kolay olmasını sağlayabilirsiniz. Son birkaç aya göre yalnızca rapor modunun güçlü şekilde benimsenmesini gördük. 26 ' dan fazla kullanıcı zaten rapor ilkesi kapsamında. Günümüzde duyuru sayesinde, yeni Azure AD koşullu erişim ilkeleri varsayılan olarak yalnızca rapor modunda oluşturulur. Bu, ilkelerinizin oluşturuldukları andan itibaren etkilerini izleyebilmeniz anlamına gelir. Ayrıca, MS Graph API 'Lerini kullananlar için [yalnızca rapor ilkelerini](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta) de daha fazla yönetebilirsiniz. 

---

### <a name="self-service-sign-up-for-guest-users"></a>Konuk kullanıcılar için self servis kaydolma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Azure AD 'de dış kimliklerle, kuruluşunuzun dışındaki kişilerin, tercih ettikleri kimliği kullanarak oturum açmalarına izin vererek uygulamalarınıza ve kaynaklarınıza erişmesini sağlayabilirsiniz. Bir uygulamayı dış kullanıcılarla paylaşırken, uygulamaya erişmesi gereken her zaman önceden bilmiyor olabilirsiniz. [Self servis kaydolma](https://docs.microsoft.com/azure/active-directory/b2b/self-service-sign-up-overview)sayesinde, konuk kullanıcıların kaydolup iş kolu (LOB) Uygulamalarınız için bir Konuk hesabı elde etmesine olanak sağlayabilirsiniz. Kaydolma akışı, Azure AD ve sosyal kimlik kimliklerini destekleyecek şekilde oluşturulabilir ve özelleştirilebilir. Kayıt sırasında Kullanıcı hakkında ek bilgiler de toplayabilirsiniz.

---

 ### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>Koşullu erişim öngörüleri ve raporlama çalışma kitabı genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

[Öngörüler ve raporlama çalışma kitabı](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting) , yöneticilere kiracınızda Azure AD koşullu erişiminin Özet görünümünü sağlar. Tek bir ilke seçme özelliği sayesinde Yöneticiler her bir ilkenin ne yaptığını daha iyi anlayabilir ve değişiklikleri gerçek zamanlı olarak izleyebilir. Çalışma kitabı, Azure Izleyici 'de depolanan verileri akışlar ve [Bu yönergeleri izleyerek](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)birkaç dakika içinde ayarlayabilirsiniz. Panoyu daha fazla bulunabilir hale getirmek için Azure AD koşullu erişim menüsündeki yeni Öngörüler ve Raporlama sekmesine taşıdık.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>Koşullu erişimin ilke ayrıntıları dikey penceresi genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Yeni [ilke ayrıntıları dikey](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access) penceresi, koşullu erişim ilkesi değerlendirmesi sırasında bulunan atamaları, koşulları ve denetimleri görüntüler. Oturum açma ayrıntılarının koşullu erişim veya yalnızca rapor sekmelerinde bulunan bir satırı seçerek dikey pencereye erişebilirsiniz.

---

### <a name="new-query-capabilities-for-directory-objects-in-microsoft-graph-are-in-public-preview"></a>Microsoft Graph dizin nesneleri için yeni sorgu özellikleri genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** MS Graph **ürün yeteneği:** geliştirici deneyimi

Microsoft Graph dizin nesneleri API 'Leri için yeni yetenekler tanıtılmakta, sayı, arama, filtre ve sıralama işlemleri etkinleştiriliyor. Bu, geliştiricilere bellek içi filtreleme ve sıralama gibi geçici çözümler olmadan dizin nesnelerinizi hızlı bir şekilde sorgulama olanağı sunar. Bu [blog gönderisine](https://aka.ms/CountFilterMSGraphAAD)daha fazla bilgi edinin.

Şu anda genel önizleme aşamasındadır ve geri bildirim aranıyor. Lütfen bu [kısa anketle](https://aka.ms/MsGraphAADSurveyDocs)yorumlarınızı gönderin.

---

### <a name="configure-saml-based-single-sign-on-using-microsoft-graph-api-beta"></a>Microsoft Graph API (Beta) kullanarak SAML tabanlı çoklu oturum açmayı yapılandırma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Beta 'daki MS Graph API 'Leri kullanılarak Azure AD galerisinden bir uygulama oluşturma ve yapılandırma desteği artık kullanılabilir. Bir uygulamanın birden çok örneği için SAML tabanlı çoklu oturum açmayı ayarlamanız gerekiyorsa, [SAML tabanlı çoklu oturum açma yapılandırmasını otomatikleştirmek](https://docs.microsoft.com/azure/active-directory/manage-apps/application-saml-sso-configure-api)Için Microsoft Graph API 'lerini kullanarak zamandan tasarruf edin.
 
---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---may-2020"></a>Azure AD uygulama galerisinde yeni sağlama bağlayıcıları-Mayıs 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

* [8x8](https://docs.microsoft.com/azure/active-directory/saas-apps/8x8-provisioning-tutorial)
* [Juno Journey](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-provisioning-tutorial)
* [MediusFlow](https://docs.microsoft.com/azure/active-directory/saas-apps/mediusflow-provisioning-tutorial)
* [Kuruluşa göre New Relic](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-by-organization-provisioning-tutorial)
* [Oracle Cloud Infrastructure Console](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-infratstructure-console-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="saml-token-encryption-is-generally-available"></a>SAML belirteci şifrelemesi genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
[SAML belirteci şifrelemesi](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption) , UYGULAMALARıN şifreli SAML onayları alacak şekilde yapılandırılmasını sağlar. Özelliği artık tüm bulutlarda genel kullanıma sunulmuştur.
 
---

### <a name="group-name-claims-in-application-tokens-is-generally-available"></a>Uygulama belirteçlerinde grup adı talepleri genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Bir belirteçte verilen grup talepleri artık yalnızca uygulamaya atanan gruplarla sınırlı olabilir.  Bu özellikle, kullanıcılar çok sayıda grubun üyesi olduğunda ve belirteç boyutu sınırlarını aşmamak açısından önemlidir. Bu yeni özellik hazır olduğunda, [belirteçlere grup adları ekleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-group-claims) özelliği genel kullanıma sunulmuştur.
 
---

### <a name="workday-writeback-now-supports-setting-work-phone-number-attributes"></a>Workday geri yazma özelliği artık iş telefonu numarası özniteliklerinin ayarlanmasını destekliyor

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
İş günü geri yazma sağlama uygulamasının artık iş telefonu numarası ve mobil numara özniteliklerinin geri yazma işlemlerini desteklemesi için geliştirilmiştir. E-posta ve Kullanıcı adının yanı sıra, artık Workday geri yazma sağlama uygulamasını Azure AD 'den Workday 'e telefon numarası değerlerini Flow şekilde yapılandırabilirsiniz. Telefon numarası geri yazma özelliğinin nasıl yapılandırılacağı hakkında daha fazla bilgi için, [Workday geri yazma](https://aka.ms/WorkdayWriteback) uygulaması öğreticisine bakın. 

---

### <a name="publisher-verification-preview"></a>Yayımcı doğrulaması (Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Geliştirici deneyimi
 
Yayımcı doğrulaması (Önizleme), yöneticilerin ve son kullanıcıların Microsoft Identity platformu ile tümleştirerek uygulama geliştiricilerinin orijinalliğini anlamalarına yardımcı olur. Ayrıntılar için [Yayımcı doğrulama (Önizleme)](https://docs.microsoft.com/azure/active-directory/develop/publisher-verification-overview)bölümüne bakın.
 
---

### <a name="authorization-code-flow-for-single-page-apps"></a>Tek sayfalı uygulamalar için yetkilendirme kodu akışı

**Şunu yazın:** Değiştirilen özellik **hizmeti kategorisi:** kimlik doğrulama **ürün yeteneği:** geliştirici deneyimi

[Safari IOP gibi modern tarayıcı 3 taraf tanımlama bilgisi kısıtlamaları](https://docs.microsoft.com/azure/active-directory/develop/reference-third-party-cookies-spas)nedeniyle, maça 'ın SSO 'yu korumak için örtük akış yerine yetkilendirme kodu akışını kullanması gerekecektir; MSAL.js v 2. x artık yetkilendirme kodu akışını destekleyecektir. Azure portal, SPA 'nizi "Spa" türüne ve kimlik doğrulama kod akışını kullanacak şekilde güncelleştirebilmeniz için bunlara karşılık gelen güncelleştirmeler de vardır. Rehberlik için [hızlı başlangıç: Kullanıcı oturum açma ve kimlik doğrulama kod akışını kullanarak bir JAVASCRIPT Spa 'da erişim belirteci edinme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-javascript-auth-code)bölümüne bakın.

---

### <a name="improved-filtering-for-devices-is-in-public-preview"></a>Cihazlar için geliştirilmiş filtreleme genel önizlemede

**Şunu yazın:** Değiştirilen özellik   
**Hizmet kategorisi:** Cihaz Yönetimi **ürün yeteneği:** cihaz yaşam döngüsü yönetimi
 
Daha önce, kullanabileceğiniz tek filtreler "etkinleştirildi" ve "etkinlik tarihi" olarak değiştirildi. Artık, işletim sistemi türü, JOIN türü, uyumluluk ve daha fazlası dahil olmak üzere [daha fazla özellik için cihaz listenizi filtreleyebilirsiniz](https://docs.microsoft.com/azure/active-directory/devices/device-management-azure-portal#device-list-filtering-preview). Bu eklemeler belirli bir cihazı konumlandırmayı basitleştirir.

---

### <a name="the-new-app-registrations-experience-for-azure-ad-b2c-is-now-generally-available"></a>Azure AD B2C için yeni Uygulama kayıtları deneyimi artık genel kullanıma sunuldu

**Şunu yazın:** Değiştirilen özellik   
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure AD B2C için yeni Uygulama kayıtları deneyimi artık genel kullanıma sunulmuştur. 

Daha önce, B2C tüketicisine yönelik uygulamalarınızı eski ' uygulamalar ' deneyimini kullanarak uygulamalarınızın geri kalanından ayrı olarak yönetmeniz gerekiyordu. Bu, Azure 'daki farklı yerlerde farklı uygulama oluşturma deneyimleri sunmaktır.

Yeni deneyim tüm B2C uygulama kayıtlarını ve Azure AD uygulama kayıtlarını tek bir yerde gösterir ve bunları yönetmek için tutarlı bir yol sağlar. Azure AD B2C kaynaklarını programlı bir şekilde yönetmek için Microsoft Graph erişimi olan bir uygulamayı veya bir uygulamayı yönetmeniz gerekip gerekmediğini öğrenmek için yalnızca bir yol yapmanız gerekir.

Azure AD B2C hizmetinde gezinerek ve Uygulama kayıtları dikey penceresini seçerek yeni deneyimle ulaşabilirsiniz. Deneyim Azure Active Directory hizmetinden de erişilebilir.

Azure AD B2C Uygulama kayıtları deneyimi, Azure AD kiracılarına yönelik genel [uygulama kayıt deneyimini](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) temel alır, ancak Azure AD B2C için tasarlanmıştır. Eski "uygulamalar" deneyimi gelecekte kullanım dışı olacaktır.

Daha fazla bilgi için [Azure AD B2C Için yeni uygulama kaydı deneyimini](https://aka.ms/b2cappregtraining)ziyaret edin.

---

## <a name="april-2020"></a>Nisan 2020

### <a name="combined-security-info-registration-experience-is-now-generally-available"></a>Birleşik güvenlik bilgileri kayıt deneyimi artık genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)

**Ürün yeteneği:** Kimlik güvenliği & koruması

Multi-Factor Authentication (MFA) ve self servis parola sıfırlama (SSPR) için Birleşik kayıt deneyimi artık genel kullanıma sunulmuştur. Bu yeni kayıt deneyimi, kullanıcıların MFA ve SSPR 'ye tek bir adım adım işlem halinde kaydolmalarını sağlar. Kuruluşunuz için yeni deneyimi dağıttığınızda, kullanıcılar daha az zaman ve daha az kurtulur kaydedebilir. Blog gönderisine [buradan](https://bit.ly/3etiRyQ)göz atın.

---

### <a name="continuous-access-evaluation"></a>Sürekli erişim değerlendirmesi

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)

**Ürün yeteneği:** Kimlik güvenliği & koruması

Sürekli erişim değerlendirmesi, Azure AD 'de (Kullanıcı hesabı silme gibi) olaylar gerçekleştiğinde Azure AD erişim belirteçlerini kullanan bağlı olan taraflar üzerinde neredeyse gerçek zamanlı ilke uygulanmasını sağlayan yeni bir güvenlik özelliğidir. Bu özelliği ilk olarak takımlar ve Outlook istemcileri için kullanıma sunuyoruz. Daha fazla ayrıntı için lütfen [blogumuzu](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933) ve [belgelerimizi](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-continuous-access-evaluation)okuyun.

---

### <a name="sms-sign-in-firstline-workers-can-sign-in-to-azure-ad-backed-applications-with-their-phone-number-and-no-password"></a>SMS oturum açma: Firstline çalışanları, telefon numarası ve parola olmadan Azure AD ile desteklenen uygulamalarda oturum açabilir

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)

**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Office, geleneksel olmayan kuruluşlara ve birincil iletişim yöntemi olarak e-posta kullanmayan büyük kuruluşlardaki çalışanlara yönelik bir dizi mobil ilk iş uygulaması başlatıyor. Bu uygulamalar, kendi işverenlerinden bir e-posta adresi alamaz, bir bilgisayara veya bu uygulamalara erişim sahibi olmayan çalışanlar, deskuz çalışanları, alan aracılarını veya perakende çalışanları hedefleyebilir. Bu proje, bu çalışanların bir telefon numarası girerek ve bir kodu aşağı girerken iş uygulamalarında oturum açmalarına olanak tanır. Daha fazla ayrıntı için lütfen [yönetici belgelerimize](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-sms-signin) ve [Son Kullanıcı belgelerinize](https://docs.microsoft.com/azure/active-directory/user-help/sms-sign-in-explainer)bakın.

---

### <a name="invite-internal-users-to-use-b2b-collaboration"></a>B2B işbirliği kullanan dahili kullanıcıları davet etme

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** EAı

**Ürün yeteneği:**

Mevcut iç hesapların, daha sonra B2B işbirliği kimlik bilgilerini kullanacak şekilde davet edilmesine izin vermek için B2B davetiyesi özelliğini genişlettik. Bu, davet edilen e-posta adresi gibi tipik parametrelere ek olarak Kullanıcı nesnesi davet API 'sine geçirilerek yapılır. Kullanıcının nesne KIMLIĞI, UPN, Grup üyeliği, uygulama ataması, vb. değişmeden kalır, ancak iletme, bir yandan, Davetten önce kullandıkları iç kimlik bilgileri yerine kendi giriş Kiracı kimlik bilgileriyle kimlik doğrulaması yapmak için B2B kullanır. Ayrıntılar için [belgelerine](https://docs.microsoft.com/azure/active-directory/b2b/invite-internal-users)bakın.

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>Koşullu erişim için yalnızca rapor modu genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Koşullu erişim

**Ürün yeteneği:** Kimlik güvenliği & koruması

[Azure AD koşullu erişim için yalnızca rapor modu](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) , erişim denetimlerini zorlamadan bir ilkenin sonucunu değerlendirmenizi sağlar. Kuruluşunuzda yalnızca rapor ilkelerini test edebilir ve etkinleştirilmeden önce etkilerini anlayabilmeniz, dağıtımın daha güvenli ve kolay olmasını sağlayabilirsiniz. Son birkaç ay içinde, yalnızca rapor ilkesi kapsamında, 26 ' dan fazla Kullanıcı önceden rapor modunun güçlü şekilde benimsenmesini gördük. Bu duyuru ile yeni Azure AD koşullu erişim ilkeleri varsayılan olarak yalnızca rapor modunda oluşturulur. Bu, ilkelerinizin oluşturuldukları andan itibaren etkilerini izleyebilmeniz anlamına gelir. Ayrıca, MS Graph API 'Lerini kullananlar için, [yalnızca rapor ilkelerini programlı](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta)bir şekilde yönetebilirsiniz. 

---

### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>Koşullu erişim öngörüleri ve raporlama çalışma kitabı genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Koşullu erişim

**Ürün yeteneği:** Kimlik güvenliği & koruması

Koşullu erişim [öngörüleri ve raporlama çalışma kitabı](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting) , yöneticilere kiracınızda Azure AD koşullu erişiminin Özet görünümünü sağlar. Tek bir ilke seçme özelliği sayesinde, yöneticiler her bir ilkenin ne yaptığını daha iyi anlayabilir ve değişiklikleri gerçek zamanlı olarak izleyebilir. Çalışma kitabı, Azure Izleyici 'de depolanan verileri akışlar ve [Bu yönergeleri izleyerek](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)birkaç dakika içinde ayarlayabilirsiniz. Panoyu daha fazla bulunabilir hale getirmek için Azure AD koşullu erişim menüsündeki yeni Öngörüler ve Raporlama sekmesine taşıdık.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>Koşullu erişimin ilke ayrıntıları dikey penceresi genel önizlemede

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Koşullu erişim

**Ürün yeteneği:** Kimlik güvenliği & koruması

Yeni [ilke ayrıntıları dikey](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access) penceresi, koşullu erişim ilkesi değerlendirmesi sırasında hangi atamaların, koşulların ve denetimlerin karşılanmasını görüntüler. Oturum açma ayrıntılarının **koşullu erişim** veya **yalnızca rapor** sekmelerinde bulunan bir satırı seçerek dikey pencereye erişebilirsiniz.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2020"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Nisan 2020

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kurumsal uygulamalar

**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2020 Nisan 'da, uygulama galerisine federasyon desteği olan bu 31 yeni uygulamayı ekledik: 

[Saygılarınızdan oluşan Ropool uygulamaları](https://www.sincropool.com/), [smartdb](https://hibiki.dreamarts.co.jp/smartdb/trial/), [float](https://docs.microsoft.com/azure/active-directory/saas-apps/float-tutorial), [LMS365](https://lms.365.systems/), [IWT tedarik Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/iwt-procurement-suite-tutorial), [lunnı](https://lunni.fi/), [easysso, Jira](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-jira-tutorial), [sanal eğitim akademik My](https://vta.c3p.ca/app/en/openid?authenticate_with=microsoft), [Meraki panosu](https://docs.microsoft.com/azure/active-directory/saas-apps/meraki-dashboard-tutorial), [Office 365 taşıyıcısı](https://app.mover.io/login), [Konuşmacı](https://speakerengage.com/login.php)katılım, [his](https://docs.microsoft.com/azure/active-directory/saas-apps/honestly-tutorial) [, önemli,](https://docs.microsoft.com/azure/active-directory/saas-apps/ally-tutorial) [DutyFlow](https://app.dutyflow.nl/), [Alertmedia](https://docs.microsoft.com/azure/active-directory/saas-apps/alertmedia-tutorial), [gr8 kişileri](https://docs.microsoft.com/azure/active-directory/saas-apps/gr8-people-tutorial), [pendo](https://docs.microsoft.com/azure/active-directory/saas-apps/pendo-tutorial) [, üst](https://docs.microsoft.com/azure/active-directory/saas-apps/highground-tutorial)düzey, [uyum](https://docs.microsoft.com/azure/active-directory/saas-apps/harmony-tutorial), [timetabling çözümleri](https://docs.microsoft.com/azure/active-directory/saas-apps/timetabling-solutions-tutorial), [Synchronet tıklama](https://docs.microsoft.com/azure/active-directory/saas-apps/synchronet-click-tutorial), [Empower](https://www.made-in-office.com/en/), [ktes değişiklik bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/fortes-change-cloud-tutorial), [Litmus](https://docs.microsoft.com/azure/active-directory/saas-apps/litmus-tutorial), [grouptalk](https://recorder.grouptalk.com/), [mintify](https://docs.microsoft.com/azure/active-directory/saas-apps/frontify-tutorial), [MongoDB Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/mongodb-cloud-tutorial), [tickitlms öğreni](https://docs.microsoft.com/azure/active-directory/saas-apps/tickitlms-learn-tutorial), [Coco](https://hexaware.com/partnerships-and-alliances/digital-transformation-using-microsoft-azure/), [Nitro üretkenlik Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/nitro-productivity-suite-tutorial) , [Trend Micro Web Security (tmws)](https://review.docs.microsoft.com/azure/active-directory/saas-apps/trend-micro-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="microsoft-graph-delta-query-support-for-oauth2permissiongrant-available-for-public-preview"></a>OAuth2PermissionGrant için Delta sorgu desteği genel önizlemeye sunuldu Microsoft Graph

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

OAuth2PermissionGrant için Delta sorgusu genel önizleme için kullanılabilir! Artık Microsoft Graph sürekli olarak yoklama yapmadan değişiklikleri izleyebilirsiniz. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/oAuth2PermissionGrant-delta?view=graph-rest-beta&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-organizational-contact-generally-available"></a>Kurumsal iletişim için Microsoft Graph Delta sorgu desteği genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

Kurumsal kişiler için Delta sorgusu genel kullanıma sunulmuştur! Artık, Microsoft Graph sürekli olarak yoklama yapmadan üretim uygulamalarındaki değişiklikleri izleyebilirsiniz. Performansı önemli ölçüde artırmak için, orgContact verilerini Delta sorgusuna sürekli yoklayan mevcut herhangi bir kodu değiştirin. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/orgcontact-delta?view=graph-rest-1.0&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-application-generally-available"></a>Uygulama için Microsoft Graph Delta sorgu desteği genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

Uygulamalar için Delta sorgusu genel kullanıma sunulmuştur! Artık, Microsoft Graph sürekli olarak yoklama yapmadan üretim uygulamalarındaki değişiklikleri izleyebilirsiniz. Performansı önemli ölçüde artırmak için uygulama verilerini Delta sorgusuna göre sürekli yoklayan mevcut herhangi bir kodu değiştirin. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/application-delta?view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-administrative-units-available-for-public-preview"></a>Genel önizleme için kullanılabilen yönetim birimlerine yönelik Delta sorgu desteğini Microsoft Graph

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Yönetim birimleri için geliştirici deneyimi Delta sorgusu genel önizleme için kullanılabilir! Artık Microsoft Graph sürekli olarak yoklama yapmadan değişiklikleri izleyebilirsiniz. [Daha fazla bilgi edinin.](https://docs.microsoft.com/graph/api/administrativeunit-delta?view=graph-rest-beta&tabs=http)

---

### <a name="manage-authentication-phone-numbers-and-more-in-new-microsoft-graph-beta-apis"></a>Yeni Microsoft Graph Beta API 'Lerinde kimlik doğrulama telefon numaralarını ve daha fazlasını yönetme

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** MS grafiği

**Ürün yeteneği:** Geliştirici deneyimi

Bu API 'Ler, kullanıcılarınızın kimlik doğrulama yöntemlerini yönetmeye yönelik bir temel araçtır. Artık MFA ve self servis parola sıfırlama (SSPR) için kullanılan kimlik doğrulayıcılar aracılığıyla programlı olarak kayıt yapabilir ve yönetebilirsiniz. Bu, Azure MFA, SSPR ve Microsoft Graph alanlarında en çok istenen özelliklerden biridir. Bu dalgaya yayımladığımız yeni API 'Ler şunları yapabilmenizi sağlar:

- Kullanıcının kimlik doğrulama telefonlarını okuma, ekleme, güncelleştirme ve kaldırma
- Kullanıcının parolasını sıfırlama
- SMS 'yi açma ve kapatma-oturum açma

Daha fazla bilgi için bkz. [Azure AD kimlik doğrulama yöntemleri API 'sine genel bakış](https://docs.microsoft.com/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

---

### <a name="administrative-units-public-preview"></a>Yönetim birimleri genel önizleme

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** RAPORDAKI

**Ürün yeteneği:** Access Control

Yönetim birimleri bir departman, bölge veya tanımladığınız kuruluşunuzun diğer kesimiyle kısıtlanan yönetici izinleri vermenizi sağlar. Bölgesel yöneticilerle izinleri devretmek veya ayrıntılı bir düzeyde ilke ayarlamak için yönetim birimlerini kullanabilirsiniz. Örneğin, bir kullanıcı hesabı Yöneticisi profil bilgilerini güncelleştirebilir, parolaları sıfırlayabilir ve yalnızca kendi yönetim biriminde kullanıcılar için lisans atayabilir.

Yönetim birimlerini kullanarak bir merkezi yönetici şunları sağlayabilir:

- Kaynakların merkezileşmemiş yönetimi için yönetim birimi oluşturma
- Yönetici izinlerine sahip bir rolü yalnızca Azure AD kullanıcıları üzerinde bir yönetim biriminde atama
- Yönetim birimlerini Kullanıcı ve gruplarla gerektiği şekilde doldurma

Daha fazla bilgi için bkz. [Azure Active Directory (Önizleme) Içinde yönetim birimleri yönetimi](https://aka.ms/AdminUnitsDocs).

---

### <a name="printer-administrator-and-printer-technician-built-in-roles"></a>Yazıcı Yöneticisi ve yazıcı teknisyeni yerleşik rolleri

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** RAPORDAKI

**Ürün yeteneği:** Access Control

**Yazıcı Yöneticisi**: Bu role sahip kullanıcılar, Evrensel Yazdırma Bağlayıcısı ayarları dahil olmak üzere Microsoft Evrensel Yazdırma çözümünde tüm yazıcı yapılandırmalarının yazıcılarını kaydedebilir ve tüm yönlerini yönetebilir. Tüm temsilci atanmış yazdırma izni isteklerini kabul edebilirler. Yazıcı yöneticilerinin Ayrıca raporları yazdırma erişimi de vardır. 

**Yazıcı teknisyeni**: Bu role sahip kullanıcılar Microsoft Evrensel Yazdırma çözümünde yazıcı kaydedebilir ve yazıcı durumunu yönetebilir. Ayrıca, tüm bağlayıcı bilgilerini okuyabilir. Ana görevler bir yazıcı teknisyeni, yazıcılar ve paylaşım yazıcılarında Kullanıcı izinlerini ayarlar. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#printer-administrator)

---

### <a name="hybrid-identity-admin-built-in-role"></a>Karma kimlik yöneticisi yerleşik rolü

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** RAPORDAKI

**Ürün yeteneği:** Access Control

Bu roldeki kullanıcılar, Azure AD 'de karma kimliği etkinleştirme ile ilgili hizmetleri ve ayarları etkinleştirebilir, yapılandırabilir ve yönetebilir. Bu rol, Azure AD 'yi desteklenen üç kimlik doğrulama yönteminden birine&#8212;, Parola karması eşitlemesi (PHS), geçişli kimlik doğrulaması (PTA) veya Federasyon (AD FS ya da 3. taraf Federasyon sağlayıcısı) &#8212;ve bunları etkinleştirmek üzere ilgili şirket içi altyapıyı dağıtmak için yapılandırma olanağı verir. Şirket içi altyapı, sağlama ve PTA aracılarını içerir. Bu rol, Windows 10 olmayan cihazlarda veya Windows Server 2016 olmayan bilgisayarlarda sorunsuz kimlik doğrulamayı etkinleştirmek için sorunsuz çoklu oturum açmayı (S-SSO) etkinleştirme olanağı verir. Ayrıca, bu rol, oturum açma günlüklerini görüntüleme ve izleme ve sorun giderme amacıyla sistem durumuna ve analizine erişme olanağı verir. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#hybrid-identity-administrator)

---

### <a name="network-administrator-built-in-role"></a>Ağ Yöneticisi yerleşik rolü

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** RAPORDAKI

**Ürün yeteneği:** Access Control

Bu role sahip olan kullanıcılar, Microsoft 'tan gelen ve Kullanıcı konumlarından ağ telemetrisini temel alan ağ çevre mimarisi önerilerini gözden geçirebilir. Office 365 için ağ performansı, genellikle kullanıcı konumlarına özgü olan dikkatli kurumsal müşteri ağ çevre mimarisine dayanır. Bu rol, geliştirilmiş telemetri ölçümlerini ve tasarım önerilerini kolaylaştırmak amacıyla, bulunan Kullanıcı konumlarının düzenlenmesine ve bu konumların ağ parametrelerinin yapılandırılmasını sağlar. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#network-administrator)

---

### <a name="bulk-activity-and-downloads-in-the-azure-ad-admin-portal-experience"></a>Azure AD Yönetici portalı deneyiminde toplu etkinlik ve İndirmeler

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kullanıcı Yönetimi

**Ürün yeteneği:** Dizinden

Artık Azure AD Yönetim Portalı deneyimine bir CSV dosyası yükleyerek Azure AD 'de kullanıcılar ve gruplar üzerinde toplu etkinlikler gerçekleştirebilirsiniz. Kullanıcılar oluşturabilir, kullanıcıları silebilir ve konuk kullanıcıları davet edebilirsiniz. Ve bir gruba üye ekleyebilir ve kaldırabilirsiniz.

Azure AD kaynakları listesini Azure AD Yönetici portalı deneyiminden de indirebilirsiniz. Dizindeki Kullanıcı listesini, dizindeki grupların listesini ve belirli bir grubun üyelerini indirebilirsiniz.

Daha fazla bilgi için aşağıdakileri gözden geçirin:

- [Kullanıcı oluşturma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-add) veya [Konuk kullanıcıları davet etme](https://docs.microsoft.com/azure/active-directory/b2b/tutorial-bulk-invite)
- [Kullanıcıları silme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-delete) veya [silinen kullanıcıları geri yükleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-restore)
- [Kullanıcı listesini indir](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-download) veya [Grup listesini indir](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download)
- Üye [ekleme (içeri aktarma)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members) veya üye [kaldırma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members) ya da bir grup için [üye listesini indirme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members)

---

### <a name="my-staff-delegated-user-management"></a>Personelin Temsilcili Kullanıcı Yönetimi

**Şunu yazın:** Yeni Özellik

**Hizmet kategorisi:** Kullanıcı Yönetimi

**Ürün yeteneği:**

Personeliniz, personel üyelerinin Azure AD hesaplarına erişebildiğinden emin olmak için mağaza yöneticisi gibi Firstline yöneticileri sağlar. Kuruluşlar, merkezi bir yardım masasına güvenmek yerine, parolaları sıfırlama veya telefon numaralarını değiştirme gibi genel görevleri bir Firstline Manager 'a devredebilir. Personeli sayesinde, hesaplarına erişemesiz bir Kullanıcı, yardım masası veya BT personeli gerekmeden yalnızca birkaç tıklamayla erişim elde edebilir. Daha fazla bilgi için bkz. [kullanıcılarımızla kullanıcılarınızı yönetme (Önizleme)](https://aka.ms/MyStaffAdminDocs) ve [Kullanıcılarım Ile Kullanıcı yönetimi temsilcisi seçme (Önizleme)](https://aka.ms/MyStaffUserDocs).

---

### <a name="an-upgraded-end-user-experience-in-access-reviews"></a>Erişim incelemeleriyle yükseltilen Son Kullanıcı deneyimi

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Erişim Incelemeleri

**Ürün yeteneği:** Kimlik yönetimi

My Apps portalındaki Azure AD erişim gözden geçirmeleriyle ilgili gözden geçiren deneyimini güncelleştirdik. Nisan 'un sonunda, Azure AD erişim gözden geçirmeleri gözden geçirmesinde oturum açmış olan gözden geçirenler, erişimimde güncelleştirilmiş deneyimi denemeye imkan tanıyan bir başlık görülecektir. Lütfen güncelleştirilmiş erişim İncelemeleri deneyiminin geçerli deneyimle aynı işlevleri sunduğunu, ancak kullanıcılarınızın üretken olmalarını sağlamak için yeni yetenekler üzerinde geliştirilmiş bir kullanıcı arabirimi olduğunu unutmayın. [Burada güncelleştirilmiş deneyim hakkında daha fazla bilgi](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review)edinebilirsiniz. Bu genel önizleme, 2020 Temmuz 'un sonuna kadar son olacak. Temmuz 'un sonunda, önizleme deneyimine karar verilmeyen gözden geçirenler, erişim gözden geçirmeleri gerçekleştirmek için erişimime otomatik olarak yönlendirilir. Gözden geçirenlerden şimdi erişimimde önizleme deneyimine geçiş yapmasını istiyorsanız, [lütfen buradan bir istek yapın](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u).

---

### <a name="workday-inbound-user-provisioning-and-writeback-apps-now-support-the-latest-versions-of-workday-web-services-api"></a>İşgünü gelen Kullanıcı hazırlama ve geri yazma uygulamaları artık Workday Web Hizmetleri API 'sinin en son sürümlerini desteklemektedir

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Uygulama sağlama

**Ürün yeteneği:** 

Müşteri geri bildirimlerine bağlı olarak, Workday Web Hizmetleri (WWS) API 'sinin en son sürümlerini desteklemek üzere kurumsal uygulama galerisinde iş günü gelen Kullanıcı hazırlama ve geri yazma uygulamalarını güncelleştirdik. Bu değişiklik ile müşteriler, bağlantı dizesinde kullanmak istedikleri WWS API sürümünü belirtebilir. Bu, müşterilere Workday 'in sürümlerindeki daha fazla ık özniteliği alma olanağı sağlar. Workday geri yazma uygulaması artık Maintain_Contact_Info sınırlamalarını aşmak için önerilen Change_Work_Contact_Info Workday Web hizmetini kullanmaktadır.

Bağlantı dizesinde sürüm belirtilmemişse, varsayılan olarak, Workday gelen sağlama uygulamaları, gelen Kullanıcı sağlaması için en son Workday API 'Lerine geçiş yapmak üzere WWS v 21.1 kullanmaya devam eder, müşterilerin bağlantı dizesini [öğreticide](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles) belgelendiği şekilde güncelleştirmesi ve ayrıca Workday [özniteliği başvuru kılavuzu](https://docs.microsoft.com/azure/active-directory/app-provisioning/workday-attribute-reference#xpath-values-for-workday-web-services-wws-api-v30)'Nda belgelendiği gibi Workday öznitelikleri için kullanılan XPaths 'ı güncelleştirmesi gerekir. 

Yeni API 'yi geri yazma için kullanmak üzere, Workday geri yazma sağlama uygulamasında hiçbir değişiklik yapılması gerekmez. Workday tarafında, Workday Integration System User (ıSU) hesabının öğretici bölümünde belgelendiği gibi Change_Work_Contact iş sürecini çağırma izinlerine sahip olduğundan emin olun, [iş süreci güvenlik ilkesi Izinlerini yapılandırın](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#configuring-business-process-security-policy-permissions). 

[Öğretici kılavuzumuzu](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) yeni API sürümü desteğini yansıtacak şekilde güncelleştirdik.

---

### <a name="users-with-default-access-role-are-now-in-scope-for-provisioning"></a>Varsayılan erişim rolüne sahip kullanıcılar artık sağlama için kapsamdadır

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Uygulama sağlama

**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Geçmişte, varsayılan erişim rolüne sahip olan kullanıcılar sağlama için kapsam dışındaydı. Müşterilerin bu rolü olan kullanıcıların sağlama kapsamında olmasını istediğini belirten geri bildirimde bulunduk. 16 Nisan 2020 itibariyle, tüm yeni sağlama yapılandırmalarının varsayılan erişim rolüne sahip kullanıcıların sağlanması sağlanır. Kademeli olarak, mevcut sağlama yapılandırmalarının davranışını bu rolle kullanıcıları sağlamayı destekleyecek şekilde değiştirecek. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned)

---

### <a name="updated-provisioning-ui"></a>Hazırlama Kullanıcı arabirimi güncelleştirildi

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Uygulama sağlama

**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Daha odaklanmış bir yönetim görünümü oluşturmak için sağlama deneyimimizi yeniledik. Zaten yapılandırılmış bir kurumsal uygulama için sağlama dikey penceresine gittiğinizde, sağlama, durdurma ve yeniden başlatma gibi eylemleri sağlama ve yönetme sürecini kolayca izleyebilirsiniz. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

---

### <a name="dynamic-group-rule-validation-is-now-available-for-public-preview"></a>Dinamik grup kuralı doğrulaması artık genel önizleme için kullanılabilir

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** Grup Yönetimi

**Ürün yeteneği:** İş

Azure Active Directory (Azure AD), artık dinamik grup kurallarını doğrulama araçlarını sağlıyor. Kuralların beklenen şekilde çalıştığını onaylamak için **kuralları doğrula** sekmesinde, dinamik kuralınızı örnek grup üyelerine doğrulayabilirsiniz. Yöneticiler, dinamik grup kuralları oluştururken veya güncelleştirirken, bir kullanıcının veya cihazın grubun üyesi olup olmayacağını bilmesini ister. Bu, bir kullanıcının veya cihazın, üyelik beklenmediği zaman sorun gidermede kural ölçütlerini ve yardımları karşılayıp karşılamadığını değerlendirmenize yardımcı olur.

Daha fazla bilgi için bkz. [dinamik grup üyeliği kuralını doğrulama (Önizleme)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-rule-validation).

---

### <a name="identity-secure-score---security-defaults-and-mfa-improvement-action-updates"></a>Kimlik güvenli puanı-güvenlik Varsayılanları ve MFA geliştirme eylem güncelleştirmeleri

**Şunu yazın:** Değiştirilen özellik

**Hizmet kategorisi:** yok

**Ürün yeteneği:** Kimlik güvenliği & koruması

**Azure AD geliştirme eylemleri için güvenlik varsayılanlarını destekleme:** Microsoft güvenli puanı, [Azure AD 'deki güvenlik varsayılanlarını](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults)desteklemek için geliştirme eylemlerini güncelleyecektir. Bu, kuruluşunuzun yaygın saldırılar için önceden yapılandırılmış güvenlik ayarlarıyla korunmasını kolaylaştırır. Bu, aşağıdaki geliştirme eylemlerini etkiler:

- Tüm kullanıcıların güvenli erişim için Multi-Factor Authentication 'ı tamamlayabilmesi için
- Yönetim rolleri için MFA gerektir
- Eski kimlik doğrulamasını engellemek için ilkeyi etkinleştir
 
**MFA geliştirme eylemi güncelleştirmeleri:** İşletmelerin, işletmelerle çalışan ilkeleri uygularken en çok güvenliği güvence altına aldığından emin olmak için, Microsoft güvenli puanı, Multi-Factor Authentication ' ın çevresinde ortalanmış üç geliştirme eylemini kaldırmış ve iki eklenmiştir.

Geliştirme eylemleri kaldırıldı:

- Multi-Factor Authentication için tüm kullanıcıları kaydet
- Tüm kullanıcılar için MFA gerektirme
- Azure AD ayrıcalıklı rolleri için MFA gerektir

Geliştirme eylemleri eklendi:

- Tüm kullanıcıların güvenli erişim için Multi-Factor Authentication 'ı tamamlayabilmesi için
- Yönetim rolleri için MFA gerektir

Bu yeni geliştirme eylemleri, kullanıcılarınızı veya yöneticilerinizi, dizininiz genelinde Multi-Factor Authentication (MFA) için kaydetmeyi ve kuruluşunuzun gereksinimlerine uygun olan doğru ilke kümesini oluşturmayı gerektirir. Ana amaç, tüm kullanıcılarınızın ve yöneticilerinin birden çok faktörle veya risk tabanlı kimlik doğrulama istemlerinde kimlik doğrulaması yapabilmesini sağlarken esneklik sağlamaktır. Bu, kapsamlı kararlar uygulayan veya güvenlik varsayılanlarını (16 Mart itibariyle) ayarlayarak, Microsoft 'un MFA için kullanıcılara ne zaman zorluk cağına karar vermesine olanak tanıyan birden çok ilke olmasını sağlayabilir. [Microsoft güvenli puandaki yenilikler hakkında daha fazla bilgi edinin](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide#whats-new).

---

## <a name="march-2020"></a>Mart 2020

### <a name="unmanaged-azure-active-directory-accounts-in-b2b-update-for-march-2021"></a>2021 Mart için B2B güncelleştirmesinde yönetilmeyen Azure Active Directory hesapları

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Microsoft, **31 mart 2021**' den ıtıbaren, B2B işbirliği senaryolarına yönelik yönetilmeyen Azure Active Directory (Azure AD) hesapları ve kiracılar oluşturarak davetlerin kullanımını desteklememektedir. Buna hazırlık olarak, [bir kerelik geçiş kodu kimlik doğrulamasını e-posta](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode)ile yapmayı tercih etmeniz önerilir.

---

### <a name="users-with-the-default-access-role-will-be-in-scope-for-provisioning"></a>Varsayılan erişim rolüne sahip kullanıcılar sağlama kapsamında olacaktır

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Geçmişte, varsayılan erişim rolüne sahip olan kullanıcılar sağlama için kapsam dışındaydı. Müşterilerin bu rolü olan kullanıcıların sağlama kapsamında olmasını istediğini belirten geri bildirimde bulunduk. Tüm yeni sağlama yapılandırmalarının, varsayılan erişim rolüne sahip kullanıcıların sağlanmasına olanak tanımak için bir değişikliği dağıtmaya çalışıyoruz. Kademeli olarak, mevcut sağlama yapılandırmalarının davranışını bu rolle kullanıcıları sağlamayı destekleyecek şekilde değiştireceksiniz. Hiçbir müşteri eylemi gerekli değildir. Bu değişiklik gerçekleştikten sonra [belgelerimize](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned) bir güncelleştirme göndereceğiz.

---

### <a name="azure-ad-b2b-collaboration-will-be-available-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet-tenants"></a>Azure AD B2B işbirliği, 21Vianet tarafından çalıştırılan Microsoft Azure (Azure Çin 21Vianet) kiracılarında kullanıma sunulacaktır

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Azure AD B2B işbirliği özellikleri, 21Vianet tarafından çalıştırılan Microsoft Azure (Azure Çin 21Vianet) kiracılarında kullanıma sunulacaktır ve bir Azure Çin 21Vianet kiracısındaki kullanıcıların diğer Azure Çin 21Vianet kiracılarındaki kullanıcılarla sorunsuz bir şekilde işbirliği yapmasına olanak tanır. [Azure AD B2B işbirliği hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/b2b/).

---
 
### <a name="azure-ad-b2b-collaboration-invitation-email-redesign"></a>Azure AD B2B Işbirliği daveti e-postası tasarımı

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Kullanıcıları dizine davet etmek için Azure AD B2B işbirliği daveti hizmeti tarafından gönderilen [e-postalar](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) , davet bilgilerini ve kullanıcının sonraki adımları daha net hale getirmek için yeniden tasarlanmıştır.

---

### <a name="homerealmdiscovery-policy-changes-will-appear-in-the-audit-logs"></a>HomeRealmDiscovery ilkesi değişiklikleri denetim günlüklerinde görüntülenir

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Denetlenmesini  
**Ürün yeteneği:** & raporlamayı izleme
 
[Homerealmdiscovery ilkesinde](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) yapılan değişikliklerin denetim günlüklerine dahil edilmediğinden oluşan bir hata düzeltildi. Artık, ilkenin ne zaman ve nasıl değiştiğini ve kim tarafından nasıl değiştirildiğini görebileceksiniz. 

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2020"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları mevcuttur-Mart 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2020 Mart 'da, uygulama galerisine federasyon desteğiyle bu 51 yeni uygulamalar ekledik: 

[Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect), [Zoho One Çin](https://docs.microsoft.com/azure/active-directory/saas-apps/zoho-one-china-tutorial), [PlusPlus](https://test.plusplus.app/auth/login/azuread-outlook/), [Profit.co SAML App](https://docs.microsoft.com/azure/active-directory/saas-apps/profitco-saml-app-tutorial), [IPoint Service Provider](https://docs.microsoft.com/azure/active-directory/saas-apps/ipoint-service-provider-tutorial), [Contexxt.ai Sphere](https://contexxt-sphere.com/login), [ınkıs tarafından Wıkıx](https://docs.microsoft.com/azure/active-directory/saas-apps/wisdom-by-invictus-tutorial), [Mercek Parlaması, açılan dijital imza](https://spark-dev.pixelnebula.com/login), [LOGZ.IO-Cloud Observability for mühendisler](https://docs.microsoft.com/azure/active-directory/saas-apps/logzio-cloud-observability-for-engineers-tutorial), [SpectrumU](https://docs.microsoft.com/azure/active-directory/saas-apps/spectrumu-tutorial), [Bizzcontact](https://bizzcontact.app/), [elqano SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/elqano-sso-tutorial), [Pazar Signshare](http://www.signshare.com/), [crossknowledge Learning Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/crossknowledge-learning-suite-tutorial), [NetVision Compas](https://docs.microsoft.com/azure/active-directory/saas-apps/netvision-compas-tutorial), [fcm hub](https://docs.microsoft.com/azure/active-directory/saas-apps/fcm-hub-tutorial), [Rib A/S byggeweb Mobile](https://apps.apple.com/us/app/docia/id529058757), [golmürekkepler](https://docs.microsoft.com/azure/active-directory/saas-apps/golinks-tutorial), [Dataköpek](https://docs.microsoft.com/azure/active-directory/saas-apps/datadog-tutorial), [Zscaler B2B Kullanıcı Portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-b2b-user-portal-tutorial), [yükseltme,](https://docs.microsoft.com/azure/active-directory/saas-apps/lift-tutorial) [Planview Enterprise One](https://docs.microsoft.com/azure/active-directory/saas-apps/planview-enterprise-one-tutorial), [WatchTeams](https://www.devfinition.com/), [Aster](https://demo.asterapp.io/login), [yetenekler iş akışı](https://docs.microsoft.com/azure/active-directory/saas-apps/skills-workflow-tutorial), [düğüm öngörüleri](https://admin.nodeinsight.com/AADLogin.aspx) [IP platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/ip-platform-tutorial), [Invision](https://docs.microsoft.com/azure/active-directory/saas-apps/invision-tutorial), [Pipedrive](https://docs.microsoft.com/azure/active-directory/saas-apps/pipedrive-tutorial), gösterim [Atölyesi](https://app.showcaseworkshop.com/), [melight Integration platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-integration-platform-tutorial), [doğa Light uyumlu erişim yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-compliant-access-management-tutorial), [grok Learning](https://docs.microsoft.com/azure/active-directory/saas-apps/grok-learning-tutorial), [Mıradore çevrimiçi](https://login.online.miradore.com/), [Khoros Hizmetleri](https://docs.microsoft.com/azure/active-directory/saas-apps/khoros-care-tutorial), [askyourteam](https://docs.microsoft.com/azure/active-directory/saas-apps/askyourteam-tutorial), [TruNarrative](https://docs.microsoft.com/azure/active-directory/saas-apps/trunarrative-tutorial), [smartwaiver](https://www.smartwaiver.com/m/user/sw_login.php?wms_login), [BizAgi Studio for Digital Process Automation](https://docs.microsoft.com/azure/active-directory/saas-apps/bizagi-studio-for-digital-process-automation-tutorial), [yalıtılmış Tex](https://www.insuite.jp/), [Sybo](https://www.systexsoftware.com.tw/), [britive](https://docs.microsoft.com/azure/active-directory/saas-apps/britive-tutorial), [WhosOffice](https://docs.microsoft.com/azure/active-directory/saas-apps/whosoffice-tutorial), [E-Days](https://docs.microsoft.com/azure/active-directory/saas-apps/e-days-tutorial), [kollesel Sdn](https://portal.kollective.app/login), [Witivio](https://app.witivio.com/), [playtı](https://my.playvox.com/login), [Korn Fraz 360](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-360-tutorial), [kampüs Café](https://docs.microsoft.com/azure/active-directory/saas-apps/campus-cafe-tutorial), [catch noktası](https://docs.microsoft.com/azure/active-directory/saas-apps/catchpoint-tutorial), [Code42](https://docs.microsoft.com/azure/active-directory/saas-apps/code42-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="azure-ad-b2b-collaboration-available-in-azure-government-tenants"></a>Azure AD B2B Işbirliği Azure Kamu kiracılarında kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 
Azure AD B2B işbirliği özellikleri artık bazı Azure Kamu kiracılar arasında kullanılabilir.  Kiracınızın bu özellikleri kullanıp kullanabileceklerini öğrenmek için, [B2B işbirliğinin Azure ABD kamu kiracısında kullanılabilir olup olmadığını nasıl anlayabilirim?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant)konusundaki yönergeleri izleyin.

---

### <a name="azure-monitor-integration-for-azure-logs-is-now-available-in-azure-government"></a>Azure günlükleri için Azure Izleyici tümleştirmesi artık Azure Kamu 'da kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & raporlamayı izleme
 
Azure AD günlükleriyle Azure Izleyici tümleştirmesi artık Azure Kamu 'da kullanıma sunulmuştur. Azure AD günlüklerini (denetim ve oturum açma günlükleri) bir depolama hesabına, Olay Hub 'ına ve Log Analytics yönlendirebilirsiniz. Lütfen [ayrıntılı belgelere](https://aka.ms/aadlogsinamd) ve Azure AD senaryolarına yönelik [Raporlama ve izleme için dağıtım planlarına](https://docs.microsoft.com/azure/active-directory/reports-monitoring/plan-monitoring-and-reporting) göz atın.

---

### <a name="identity-protection-refresh-in-azure-government"></a>Azure Kamu 'da kimlik koruması yenileme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Artık [Azure AD Identity Protection](https://aka.ms/IdentityProtectionDocs)   , [Microsoft Azure Kamu portalında](https://portal.azure.us/)yenilenen Azure AD kimlik koruması deneyimini kullanıma sunduğumuz için paylaştık. Daha fazla bilgi için bkz. [duyuru blog gönderimiz](https://techcommunity.microsoft.com/t5/public-sector-blog/identity-protection-refresh-in-microsoft-azure-government/ba-p/1223667).

---

### <a name="disaster-recovery-download-and-store-your-provisioning-configuration"></a>Olağanüstü durum kurtarma: sağlama yapılandırmanızı Indirin ve depolayın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure AD sağlama hizmeti, zengin bir yapılandırma özellikleri kümesi sağlar. Müşterilerin, daha sonra başvurabilmeleri veya bilinen iyi bir sürüme geri dönebilmeleri için yapılandırmalarını kaydedebilmeleri gerekir. Sağlama yapılandırmanızı bir JSON dosyası olarak indirme ve ihtiyacınız olduğunda karşıya yükleme özelliği ekledik. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/app-provisioning/export-import-provisioning-configuration).

---
 
### <a name="sspr-self-service-password-reset-now-requires-two-gates-for-admins-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>SSPR (self servis parola sıfırlama) artık Microsoft Azure 21Vianet tarafından çalıştırılan Yöneticiler için iki ağ geçidi gerektirir (Azure Çin 21Vianet) 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Daha önce 21Vianet tarafından çalıştırılan Microsoft Azure (Azure Çin 21Vianet), kendi parolalarını sıfırlamak için self servis parola sıfırlama (SSPR) kullanan yöneticiler, kimliklerini kanıtlamak için yalnızca bir "Gate" (zorluk) gerekir. Genel ve diğer ulusal bulutlarda Yöneticiler, SSPR kullanırken kimliklerini kanıtlamak için genellikle iki kapı kullanmalıdır. Ancak, Azure Çin 21Vianet 'de SMS veya telefon çağrılarını desteklemediğimiz için, yöneticiler tarafından tek bir geçit parola sıfırlamasına izin verdik.

Azure Çin 21Vianet ve genel bulut arasında SSPR özelliği eşliği oluşturacağız. Bundan sonra, Yöneticiler SSPR kullanırken iki ağ geçidi kullanmalıdır. SMS, telefon aramaları ve Authenticator uygulama bildirimleri ve kodları desteklenecektir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#administrator-reset-policy-differences).

---

### <a name="password-length-is-limited-to-256-characters"></a>Parola uzunluğu 256 karakterle sınırlıdır

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD hizmeti 'nin güvenilirliğini güvence altına almak için kullanıcı parolalarının uzunluğu 256 karakterle sınırlıdır. Bundan daha uzun parolalara sahip olan kullanıcılar, yöneticilerle iletişim kurarak veya self servis parola sıfırlama özelliğini kullanarak, sonraki oturum açmada parolasını değiştirmeyecektir.

Bu değişiklik, 13 Mart, 2020, saat 10:00:00 PST (18:00 UTC) tarihinde etkinleştirildi ve hata AADSTS 50052, ınvalidpasswordexceedsmaxlength. Daha fazla ayrıntı için bkz. [değişiklik bildirimi](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#user-passwords-will-be-restricted-to-256-characters) .

---

### <a name="azure-ad-sign-in-logs-are-now-available-for-all-free-tenants-through-the-azure-portal"></a>Azure AD oturum açma günlükleri artık Azure portal aracılığıyla tüm ücretsiz kiracılar için kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & raporlamayı izleme
 
Şimdi başlayarak, ücretsiz kiracılar olan müşteriler [Azure Portal Azure AD oturum açma günlüklerine](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) en fazla 7 güne kadar erişebilir. Daha önce, oturum açma günlükleri yalnızca Azure Active Directory Premium lisansları olan müşteriler için kullanılabilir. Bu değişiklik ile tüm kiracılar bu günlüklere Portal üzerinden erişebilir.

> [!NOTE]
> Müşterilerin, Microsoft Graph API ve Azure Izleyici aracılığıyla oturum açma günlüklerine erişmesi için Premium lisansına (Azure Active Directory Premium P1 veya P2) ihtiyacı vardır.

---

### <a name="deprecation-of-directory-wide-groups-option-from-groups-general-settings-on-azure-portal"></a>Grupların genel ayarlarından Dizin genelinde gruplar seçeneğinin kullanımdan kaldırılması Azure portal

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Müşterilerin ihtiyaçlarını en iyi şekilde karşılayan Dizin genelinde gruplar oluşturması için daha esnek bir yol sağlamak üzere, **Dizin genelinde** gruplar seçeneğini, Azure Portal **gruplar**  >  **genel** ayarlarından [dinamik grup belgelerinin](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)bir bağlantısıyla değiştirdik. Yöneticiler, Konuk kullanıcıları dahil eden veya hariç tutmaları için daha fazla yönerge eklemek üzere belgelerimizi geliştirdik.

---

## <a name="february-2020"></a>Şubat 2020

### <a name="upcoming-changes-to-custom-controls"></a>Özel denetimlerde yaklaşan değişiklikler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
İş ortağı tarafından sunulan kimlik doğrulama yeteneklerinin Azure Active Directory yönetici ve son kullanıcı deneyimleri ile sorunsuz bir şekilde çalışmasını sağlayan bir yaklaşım ile geçerli özel denetim önizlemesini değiştirmeyi planlıyoruz. Günümüzde, iş ortağı MFA çözümleri şu sınırlamalara sahiptir: yalnızca bir parola girildikten sonra çalışır; diğer anahtar senaryolarında adım adım kimlik doğrulaması için MFA olarak işlev görürler; Ayrıca, son kullanıcı veya yönetim kimlik bilgileri yönetim işlevleriyle tümleştirilemiyor. Yeni uygulama, iş ortağı tarafından belirtilen kimlik doğrulama faktörlerinin, kayıt, kullanım, MFA talepleri, adım kimlik doğrulama, raporlama ve günlüğe kaydetme gibi önemli senaryolar için yerleşik faktörlerle birlikte çalışmasına izin verir. 

Özel denetimler, genel kullanıma ulaşıncaya kadar yeni tasarımın yanı sıra önizlemede desteklenmeye devam edecektir. Bu noktada, müşterilere yeni tasarıma geçiş süresi vereceğiz. Geçerli yaklaşımın sınırlamaları nedeniyle, yeni tasarım kullanılabilir olana kadar yeni sağlayıcılar sunmayacağız. Müşteriler ve sağlayıcılar ile yakından çalışıyoruz ve zaman çizelgesini daha yakından ilettiğimiz için iletişim kuracak. [Daha fazla bilgi edinin](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/upcoming-changes-to-custom-controls/ba-p/1144696#).

---

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

[Iamıp patent platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/iamip-patent-platform-tutorial), [deneyim bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/experience-cloud-tutorial), [Azure için ns1 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/ns1-sso-azure-tutorial), [Barkcuda e-posta güvenlik hizmeti](https://ess.barracudanetworks.com/sso/azure), [aba raporlaması](https://myaba.co.uk/client-access/signin/auth/msad), [Çapraz çevrimiçi portal](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-online-portal-tutorial), [Bic bulut tasarımı](https://docs.microsoft.com/azure/active-directory/saas-apps/bic-cloud-design-tutorial), [Splashtop](https://docs.microsoft.com/azure/active-directory/saas-apps/splashtop-tutorial) [Azure ad veri Bağlayıcısı için beekeeto](https://docs.microsoft.com/azure/active-directory/saas-apps/beekeeper-azure-ad-data-connector-tutorial) [Verkada Command](https://docs.microsoft.com/azure/active-directory/saas-apps/verkada-command-tutorial) [Yeni relik (sınırlı sürüm)](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial), [thulium](https://admin.thulium.com/login/instance), [bilet Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/ticketmanager-tutorial), [Syxsense](https://docs.microsoft.com/azure/active-directory/saas-apps/syxsense-tutorial) [takımlar için şablon Seçicisi](https://links.officeatwork.com/templatechooser-download-teams), [beesy](https://www.beesy.me/index.php/site/login), [sağlık desteği sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/health-support-system-tutorial), [mural](https://app.mural.co/signup), [Hive](https://docs.microsoft.com/azure/active-directory/saas-apps/hive-tutorial), [LavaDo](https://appsource.microsoft.com/product/web-apps/lavaloon.lavado_standard?tab=Overview), [Wakelet](https://wakelet.com/login), [firmex VDR](https://docs.microsoft.com/azure/active-directory/saas-apps/firmex-vdr-tutorial), [öğretmenler ve okullar için ThingLink](https://www.thinglink.com/), [Coda](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial), beklenen [podapp](https://nearpod.com/signup/?oc=Microsoft&utm_campaign=Microsoft&utm_medium=site&utm_source=product), [Wedo](https://docs.microsoft.com/azure/active-directory/saas-apps/wedo-tutorial), [Korn Ferry Assessments](https://www.kornferry.com/solutions/kf-digital/kf-assess) [EAB Navigate](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-tutorial) [davetli](https://invitepeople.com/login) [,](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-tutorial) [Reprints Desk - Article Galaxy](https://docs.microsoft.com/azure/active-directory/saas-apps/reprints-desk-article-galaxy-tutorial)

 
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
- [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-provisioning-tutorial)
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
 
Yeni Hesabım Son Kullanıcı deneyimi, URL 'sini bir sonraki ay içinde olacak şekilde güncelleştirilecektir `https://myaccount.microsoft.com` . [Hesabım portalı yardımım](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview)'de son kullanıcılara sunduğu deneyim ve tüm hesap self servis özellikleri hakkında daha fazla bilgi edinin.

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
 
Yöneticiler kullanıcıların uygulamalarını düzenlemek üzere yapılandırabilen çalışma alanları, artık koleksiyonlar olarak anılacaktır. [Uygulamalarım portalındaki koleksiyonları oluşturma](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections)sırasında bunları yapılandırma hakkında daha fazla bilgi edinin.

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
- [Zscaler Private Access](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ocak 2020

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2020 Ocak 'ta, uygulama galerisine federasyon desteğiyle bu 33 yeni uygulamalar ekledik: 

[Josa](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [fastly Edge bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [Spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [abibot Netlogistik](https://azuremarketplace.microsoft.com/marketplace/apps/aad.abibotnetlogistik), [ufuk](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access)anında, [Upshotly](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial) [upvebot](https://leavebot.io/#home), [Datacamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [tripactions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [smartwork](https://www.intumit.com/english/SmartWork.html), [Dotcom-Monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [Ssogen-Azure AD SSO Gateway for Oracle E-Business Suite-EBS, PeopleSoft ve JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial), [barındırılan Mycirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [yuhu özelliği YÖNETIM platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [Lumapps](https://sites.lumapps.com/login), [upwork Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [smartdb for Microsoft ekipleri](http://teams.smartdb.jp/login/), [presspage](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [contractsafe SAML2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [maxdentitydeploy Manager yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [helpshıft](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/), [coreview](https://portal.coreview.com/), [Squelch Cloud Office365 bağlayıcı](https://laxmi.squelch.io/login), [pingflow kimlik doğrulaması](https://app-staging.pingview.io/), [printerlogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [sandwai](https://app.sandwai.com/), [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [assetsonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [akarı Sanal Yardımcısı](https://akari.io/akari-virtual-assistant/)

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

