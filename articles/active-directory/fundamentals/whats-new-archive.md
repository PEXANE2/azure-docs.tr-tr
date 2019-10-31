---
title: Azure Active Directory yenilikler için arşiv var mı? | Microsoft Docs
description: Bu içerik kümesinin Genel Bakış bölümündeki yenilikler sürüm notları, 6 aylık etkinlik içerir. 6 ay sonra, öğeler ana makaleden kaldırılır ve bu arşiv makalesine yerleştirilir.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fcf1e5966be7708e7c6278839cbfbe5d65cdffa
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73149136"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Azure Active Directory yenilikler için arşiv var mı?

[Azure Active Directory 'deki yenilikler nelerdir? sürüm notları](whats-new.md) makalesi, son altı aya yönelik güncelleştirmeleri içerir, ancak bu makalede daha eski bilgiler yer alır.

Azure Active Directory yenilikler nelerdir? sürüm notları hakkında bilgi sağlar:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklik planları

---

## <a name="april-2019"></a>Nisan 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>Yeni Azure AD tehdit zekası algılaması artık Azure AD Kimlik Koruması bir parçası olarak sağlanıyor

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Kimlik Koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Azure AD Threat Intelligence algılama artık güncelleştirilmiş Azure AD Kimlik Koruması özelliğinin bir parçası olarak sunulmaktadır. Bu yeni işlevsellik, Microsoft 'un dahili ve dış tehdit bilgileri kaynaklarına dayanan bilinen saldırı desenleriyle tutarlı olan belirli bir kullanıcı veya etkinliğin olağan dışı Kullanıcı etkinliklerini belirtmenize yardımcı olur.

Azure AD Kimlik Koruması yenilenen sürümü hakkında daha fazla bilgi için, bkz. [dört ana Azure AD kimlik koruması geliştirmesi artık genel önizleme](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) bloguna ve [ne Azure Active Directory kimlik koruması (YENİLENDİ)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) makalenin. Azure AD Threat Intelligence algılaması hakkında daha fazla bilgi için [risk algılamalarını Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/identity-protection/risk-events-reference#azure-ad-threat-intelligence) makalesine bakın.

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Azure AD Yetkilendirme Yönetimi artık kullanıma sunuldu (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik yönetimi  
**Ürün yeteneği:** Kimlik yönetimi

Artık genel önizlemede olan Azure AD Yetkilendirme Yönetimi, müşterilerin, çalışanların ve iş ortaklarının erişim isteme, ne kadar süreceğine ve ne kadar erişebileceğini tanımlayan erişim paketleri yönetimini temsilcmalarına yardımcı olur. Erişim paketleri Azure AD ve Office 365 gruplarındaki üyeliği, kurumsal uygulamalardaki rol atamalarını ve SharePoint Online siteleri için rol atamalarını yönetebilir. Yetkilendirme Yönetimi hakkında daha fazla bilgi için [bkz. Azure AD Yetkilendirme Yönetimi](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview). Privileged Identity Management, erişim gözden geçirmeleri ve kullanım koşulları dahil Azure AD Identity Governance özellikleri hakkında daha fazla bilgi edinmek için bkz. [Azure AD Identity Governance nedir?](../governance/identity-governance-overview.md).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>Azure AD portalındaki Office 365 grupları için adlandırma ilkesi yapılandırma (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kuralları uygulanmasını sağlar.

Office 365 grupları için adlandırma ilkesini iki farklı yolla yapılandırabilirsiniz:

- Otomatik olarak bir grup adına eklenen ön ekleri veya sonekleri tanımlayın.

- Kuruluşunuz için Grup adlarında izin verilmeyen özelleştirilmiş bir engellenen sözcük kümesini karşıya yükleyin (örneğin, "CEO, bordro, HR").

Daha fazla bilgi için bkz. [Office 365 grupları Için adlandırma Ilkesi zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure AD etkinlik günlükleri artık Azure Izleyici 'de kullanılabilir (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlükleri ile görselleştirmelerle ilgili geri bildirimlerinizi ele almak için, Log Analytics yeni bir Öngörüler özelliği sunuyoruz. Bu özellik, çalışma kitapları adlı etkileşimli şablonlarımızı kullanarak Azure AD kaynaklarınız hakkında öngörüler elde etmenize yardımcı olur. Önceden oluşturulmuş bu çalışma kitapları, uygulamalar veya kullanıcılar için Ayrıntılar sağlayabilir ve şunları içerir:

- **Oturum açma işlemleri.** Uygulama ve kullanıcıların oturum açma konumu, kullanımda olan işletim sistemi veya tarayıcı istemcisi ve sürümü ve başarılı ya da başarısız oturum açma sayısı dahil olmak üzere ayrıntıları sağlar.

- **Eski kimlik doğrulama ve koşullu erişim.** Koşullu erişim ilkeleri tarafından tetiklenen Multi-Factor Authentication kullanımı, koşullu erişim ilkelerini kullanan uygulamalar vb. dahil olmak üzere eski kimlik doğrulaması kullanan uygulamalar ve kullanıcılar için ayrıntılar sağlar.

- **Oturum açma hatası analizi.** Kullanıcı eylemi, ilke sorunları veya altyapınız nedeniyle oturum açma hatalarınızın oluşup oluşmadığını belirlemenize yardımcı olur.

- **Özel raporlar.** Kuruluşunuz için Öngörüler özelliğini özelleştirmeye yardımcı olmak üzere yeni veya mevcut çalışma kitaplarını düzenleyebilirsiniz.

Daha fazla bilgi için bkz. [Azure izleyici çalışma kitaplarını Azure Active Directory raporları için kullanma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Nisan 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Nisan 'da, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [Hrçalışmalar çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [percogeç](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [Mobıoncontrol](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [benchling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [eğitibrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [rstudio Connect ](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba Cloud (rol tabanlı SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [cerkatlanmış hisse senedi yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [sectigo sertifika yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [tarzorbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [Monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [Gözetimi kurumsal](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [ındiggo](https://indiggolead.com/)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>Yeni erişim İncelemeleri sıklık seçeneği ve birden çok rol seçimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** Kimlik yönetimi

Azure AD erişim gözden geçirmeleriyle yeni güncelleştirmeler şunları yapmanıza olanak sağlar:

- Daha önce haftalık, aylık, üç aylık ve yıllık olan mevcut seçeneklere ek olarak, erişim incelemelerinizin sıklığını **yarı yıllık**olarak değiştirin.

- Tek bir erişim incelemesi oluştururken birden çok Azure AD ve Azure Kaynak rolü seçin. Bu durumda, tüm roller aynı ayarlarla ayarlanır ve tüm gözden geçirenler aynı anda bilgilendirilir.

Erişim incelemesi oluşturma hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD Connect e-posta uyarı sistemleri geçiyor, bazı müşteriler için yeni e-posta gönderici bilgileri gönderiliyor

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** AD Eşitleme  
**Ürün yeteneği:** Platformunun

Azure AD Connect, e-posta uyarı sistemlerimizi geçiş sürecinde olduğundan, bazı müşterilere yeni bir e-posta gönderici gösteriliyor olabilir. Bunu çözmek için kuruluşunuzun izin verilenler listesine `azure-noreply@microsoft.com` eklemeniz gerekir veya Office 365, Azure veya eşitleme hizmetinizden önemli uyarıları almaya devam edemeyeceksiniz.

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>UPN son ek değişiklikleri artık Azure AD Connect Federasyon etki alanları arasında başarılı oluyor

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** AD Eşitleme  
**Ürün yeteneği:** Platformunun

Artık bir kullanıcının UPN sonekini, Azure AD Connect bir Federasyon etki alanından başka bir Federasyon etki alanına başarıyla değiştirebilirsiniz. Bu düzeltilme, eşitleme çevrimi sırasında FederatedDomainChangeError hata iletisine artık karşılaşmayabilmeniz veya bir bildirim e-postası almanız, "özniteliği nedeniyle bu nesne Azure Active Directory güncelleştirilemiyor. FederatedUser. UserPrincipalName] geçerli değil. Yerel dizin hizmetinizdeki değeri güncelleştirin.

Daha fazla bilgi için bkz. [eşitleme sırasında sorun giderme hataları](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror).

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Azure AD 'de uygulama koruma tabanlı koşullu erişim ilkesini kullanarak artırılmış güvenlik (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Uygulama koruma tabanlı koşullu erişim artık **Uygulama koruma Ilkesi gerektir** ilkesi kullanılarak kullanılabilir. Bu yeni ilke, aşağıdakileri önlemeye yardımcı olarak kuruluşunuzun güvenliğini artırmaya yardımcı olur:

- Kullanıcılar Microsoft Intune lisansı olmadan uygulamalara erişim hakkı elde edebilirler.

- Kullanıcılar Microsoft Intune uygulama koruma ilkesi alamıyor.

- Kullanıcılar, yapılandırılmış bir Microsoft Intune uygulama koruma ilkesi olmadan uygulamalara erişim hakkı elde edebilirler.

Daha fazla bilgi için bkz. [koşullu erişim ile Cloud App erişimi için uygulama koruma Ilkesi gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Microsoft Edge 'de Azure AD çoklu oturum açma ve koşullu erişim için yeni destek (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Azure AD çoklu oturum açma ve koşullu erişim için yeni destek sağlama dahil olmak üzere Microsoft Edge için Azure AD desteğimizi geliştirdik. Daha önce Microsoft Intune Managed Browser kullandıysanız, artık bunun yerine Microsoft Edge 'i kullanabilirsiniz.

Koşullu erişim kullanarak cihazlarınızı ve uygulamalarınızı ayarlama ve yönetme hakkında daha fazla bilgi için bkz. koşullu erişim [ile Cloud App erişimi için yönetilen cihazlar gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) ve [koşullu erişim ile Cloud App erişimi için onaylanmış istemci uygulamaları gerektirme ](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Microsoft Edge 'i Microsoft Intune ilkeleriyle kullanarak erişimin nasıl yönetileceği hakkında daha fazla bilgi için, bkz. [Microsoft Intune İlkeyle korunan tarayıcı kullanarak Internet erişimini yönetme](https://docs.microsoft.com/intune/app-configuration-managed-browser).

---

## <a name="march-2019"></a>Mart 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Azure Active Directory B2C 'da kimlik deneyimi çerçevesi ve özel ilke desteği artık kullanılabilir (GA)

**Şunu yazın:** Yeni Özellik  
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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2019 Mart 'da, uygulama galerisine federasyon desteği olan bu 14 yeni uygulamaları ekledik:

[ISEC7 mobil Exchange temsilcisi](https://www.isec7.com/english/), [mediusflow](https://office365.cloudapp.mediusflow.com/), [eplatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [fulcrud](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [excelityglobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Açıklama tabanlı denetim sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [yalın](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [powerokul performansı önemli](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [cincode ](https://cinode.com/), [Iris intraneti](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [tas](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Azure AD galerisinde yeni Zscaler ve Atlasser sağlama bağlayıcıları-Mart 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Aşağıdaki uygulamalar için Kullanıcı hesaplarını oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirin:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](https://aka.ms/ZscalerBetaProvisioning), [Zscaler One](https://aka.ms/ZscalerOneProvisioning), [Zscaler iki](https://aka.ms/ZscalerTwoProvisioning), [Zscaler üç](https://aka.ms/ZscalerThreeProvisioning), [Zscaler zscsesli](https://aka.ms/ZscalerZSCloudProvisioning), [atlasser bulutu](https://aka.ms/atlassianCloudProvisioning)

Otomatik Kullanıcı hesabı sağlama aracılığıyla kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://aka.ms/ProvisioningDocumentation).

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Azure AD portalındaki silinen Office 365 gruplarınızı geri yükleme ve yönetme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Artık silinen Office 365 gruplarınızı Azure AD portalından görüntüleyebilir ve yönetebilirsiniz. Bu değişiklik, geri yükleme için hangi grupların kullanılabilir olduğunu görmenizi sağlar ve kuruluşunuzun ihtiyaç duymayan grupları kalıcı olarak silmenizi sağlar.

Daha fazla bilgi için bkz. [geri yükleme süre aşımına uğradı veya silinmiş gruplar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore).

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Çoklu oturum açma artık Azure AD SAML ile güvenli şirket içi uygulamalar için uygulama ara sunucusu aracılığıyla kullanılabilir (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Artık şirket içi, SAML kimliği doğrulanmış uygulamalar için çoklu oturum açma (SSO) deneyimi ve uygulama proxy 'Si aracılığıyla bu uygulamalara uzaktan erişim sağlayabilirsiniz. Şirket içi uygulamalarınızla SAML SSO 'yu ayarlama hakkında daha fazla bilgi için bkz. [uygulama proxy 'si (Önizleme) ile şirket içi uygulamalar Için SAML çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>İstek döngülerine yönelik istemci uygulamaları, güvenilirliği ve Kullanıcı deneyimini geliştirmek için kesintiye uğrar

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

İstemci uygulamaları kısa bir süre boyunca aynı oturum açma isteklerini yanlışlıkla verebilir. Bu istekler, başarılı olsun veya etmeksizin, tüm kullanıcılar için zayıf bir kullanıcı deneyimine ve ıDP 'nin bu iş yüklerini artırarak, tüm kullanıcılar için gecikme süresini artırarak ve ıDP kullanılabilirliğini azalttığından.

Bu güncelleştirme bir `invalid_grant` hatası gönderir: kısa bir süre içinde birden çok kez yinelenen istekler veren istemci uygulamalarına `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request`, normal işlem kapsamının ötesinde. Bu sorunla karşılaşan istemci uygulamalarında, kullanıcının yeniden oturum açmasını gerektiren bir etkileşimli istem gösterilmelidir. Bu değişiklik ve bu hatayla karşılaştığında uygulamanızı nasıl gidereceğiniz hakkında daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted).

---

### <a name="new-audit-logs-user-experience-now-available"></a>Yeni denetim günlükleri Kullanıcı deneyimi artık kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Okunabilirliği ve bilgilerinizi aramanızı artırmaya yardımcı olmak için yeni bir Azure AD **Denetim günlükleri** sayfası oluşturduk. Yeni **Denetim günlükleri** sayfasını görmek IÇIN Azure AD 'nin **etkinlik** bölümündeki **Denetim günlükleri** ' ni seçin.

![Yeni denetim günlükleri sayfası, örnek bilgi](media/whats-new/audit-logs-page.png)

Yeni **Denetim günlükleri** sayfası hakkında daha fazla bilgi için, bkz. [Azure Active Directory portalındaki denetim etkinliği raporları](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs).

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Yanlış yapılandırılmış koşullu erişim ilkeleriyle yanlışlıkla yöneticinin kilitlenmesini önlemeye yardımcı olacak yeni uyarılar ve kılavuzluk

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Yöneticilerin, yanlış yapılandırılmış koşullu erişim ilkeleri aracılığıyla kendi kiracılarından kendilerini yanlışlıkla kilitlemesini önlemeye yardımcı olmak için Azure portal yeni uyarılar ve güncelleştirilmiş rehberlik oluşturduk. Yeni rehberlik hakkında daha fazla bilgi için bkz. [koşullu erişim Azure Active Directory hizmet bağımlılıkları nelerdir](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Mobil cihazlarda geliştirilmiş son kullanıcı kullanım koşulları deneyimi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #C1

Mevcut kullanım koşullarımızı, bir mobil cihazda kullanım koşullarını nasıl gözden geçitireceğiniz ve kabul ettiğiniz hakkında daha fazla yardımcı olacak şekilde güncelleştirdik. Artık yakınlaştırıp uzaklaştırabilir, geri dönüp bilgileri indirebilir ve köprüler ' i seçebilirsiniz. Güncelleştirilmiş kullanım koşulları hakkında daha fazla bilgi için bkz. [kullanım koşulları özelliği Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users).

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Yeni Azure AD etkinlik günlükleri indirme deneyimi kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Artık Azure portal çok miktarda etkinlik günlüğünü doğrudan indirebilirsiniz. Bu güncelleştirme şunları yapmanızı sağlar:

- En fazla 250.000 satır indirin.

- İndirme tamamlandıktan sonra bildirim alın.

- Dosya adınızı özelleştirin.

- JSON veya CSV çıkış biçiminizi belirleme.

Bu özellik hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Azure Portal kullanarak bir denetim raporu yükleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report)

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Son değişiklik: Exchange ActiveSync (EAS) tarafından koşul değerlendirmesine yönelik güncelleştirmeler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Access Control

Exchange ActiveSync 'in (EAS) aşağıdaki koşulları nasıl değerlendirdiği hakkında güncelleştirme sürecimiz vardır:

- Ülke, bölge veya IP adresine göre Kullanıcı konumu

- Oturum açma riski

- Cihaz platformu

Bu koşulları daha önce koşullu erişim ilkeleriniz üzerinde kullandıysanız, koşul davranışının değişebilir olduğunu unutmayın. Örneğin, daha önce bir ilkede Kullanıcı konumu koşulunu kullandıysanız, ilkeyi artık kullanıcının konumuna göre atlandığını görebilirsiniz.

---

## <a name="february-2019"></a>Şubat 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Yapılandırılabilir Azure AD SAML belirteci şifrelemesi (Genel Önizleme) 

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Artık, desteklenen herhangi bir SAML uygulamasını şifrelenmiş SAML belirteçleri alacak şekilde yapılandırabilirsiniz. Azure AD, bir uygulamayla yapılandırılıp kullanıldığında, Azure AD 'de depolanan bir sertifikadan alınan bir ortak anahtar kullanarak, verilmiş SAML onaylamalarını şifreler.

SAML belirteci şifrelemeyi yapılandırma hakkında daha fazla bilgi için bkz. [Azure AD SAML belirteci şifrelemesini yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleri kullanarak gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** #C1

Artık grup üyeliği veya uygulama ataması için tek bir Azure AD erişim gözden geçirmesine birden çok grup veya uygulama ekleyebilirsiniz. Birden çok grup veya uygulama içeren erişim gözden geçirmeleri aynı ayarlarla ayarlanır ve tüm dahil olan gözden geçirenler aynı anda bildirilir.

Azure AD erişim gözden geçirmeleri kullanarak erişim incelemesi oluşturma hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle grup veya uygulama erişimi Incelemesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Şubat 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Şubat 2019 ' de, uygulama galerisine federasyon desteği olan bu 27 yeni uygulamayı ekledik:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [Mindticyon](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT FINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [airstack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [ufuk Ward qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [parlak tidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [alerar](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight-cloudgate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), İzin tıklama, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [storegatesmartfile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [pexıp](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [seismik](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [bir rüt](https://www.shareadream.org/how-it-works), [bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [WebMethods Integration Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [bilgi Her yerde LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope verileri](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [NetOp Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [Smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [Purecyüksek, Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [tıklama üretkenlik platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="enhanced-combined-mfasspr-registration"></a>Geliştirilmiş Birleşik MFA/SSPR kaydı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

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

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Kullanıcı akışları (daha önce, yerleşik ilkeler) için ilke oluşturma ve yönetim işlemini daha kolay bir şekilde güncelleştirdik. Bu yeni deneyim artık tüm Azure AD kiracılarınız için varsayılandır.

Portal ekranının üst kısmındaki **bize geri bildirim gönder** alanındaki gülümseme veya kaş simgeleri kullanarak ek geri bildirim ve öneriler sağlayabilirsiniz.

Yeni ilke yönetimi deneyimi hakkında daha fazla bilgi için, [şimdi Azure AD B2C JavaScript özelleştirmesine ve birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahiptir.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Azure AD B2C tarafından sunulan belirli sayfa öğesi sürümlerini seçin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Artık, Azure AD B2C tarafından sunulan sayfa öğelerinin belirli bir sürümünü seçebilirsiniz. Belirli bir sürümü seçerek, güncelleştirmelerinizi bir sayfada görüntülenmeden önce test edebilir ve öngörülebilir bir davranış sağlayabilirsiniz. Ayrıca, artık JavaScript özelleştirmeleriyle ilgili belirli sayfa sürümlerini zorunlu kılabilirsiniz. Bu özelliği açmak için Kullanıcı akışlarınızda **Özellikler** sayfasına gidin.

Sayfa öğelerinin belirli sürümlerini seçme hakkında daha fazla bilgi için, [artık Azure AD B2C JavaScript özelleştirmesine ve birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahiptir.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>B2C için yapılandırılabilir Son Kullanıcı parolası gereksinimleri (GA)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Artık kuruluşunuzun parola karmaşıklığını, yerel Azure AD parola ilkenizi kullanmak yerine son kullanıcılarınız için ayarlayabilirsiniz. Kullanıcı akışlarınızın **Özellikler** dikey penceresinde (önceki adıyla yerleşik ilkeleriniz), **basit** veya **güçlü**bir parola karmaşıklığı seçebilir veya **özel** bir gereksinim kümesi oluşturabilirsiniz.

Parola karmaşıklığı gereksinim yapılandırması hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C parolalar için karmaşıklık gereksinimlerini yapılandırma](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Özel markalı kimlik doğrulama deneyimleri için yeni varsayılan şablonlar

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C

Kullanıcılarınız için özel bir markalı kimlik doğrulama deneyimi oluşturmak üzere Kullanıcı akışlarınızın **sayfa düzenleri** dikey penceresinde (daha önce yerleşik ilkeler olarak bilinir) bulunan yeni varsayılan şablonlarımızı kullanabilirsiniz.

Şablonları kullanma hakkında daha fazla bilgi için bkz. [Azure AD B2C artık JavaScript özelleştirmesi ve birçok yeni özellik içeriyor](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595).

---

## <a name="january-2019"></a>Ocak 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Tek seferlik geçiş kodu kimlik doğrulaması kullanarak B2B işbirliği Active Directory (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

Azure AD, Microsoft hesabı (MSA) veya Google Federasyonu gibi diğer yollarla kimlik doğrulamasından geçmediğiniz B2B Konuk kullanıcıları için bir kerelik geçiş kodu kimlik doğrulaması (OTP) sunuyoruz. Bu yeni kimlik doğrulama yöntemi, konuk kullanıcıların yeni bir Microsoft hesabı oluşturması gerekmediği anlamına gelir. Bunun yerine, bir daveti benimseme veya paylaşılan bir kaynağa erişirken, Konuk Kullanıcı bir e-posta adresine gönderilmek üzere geçici bir kod isteyebilir. Bu geçici kodu kullanarak Konuk Kullanıcı oturum açmaya devam edebilir.

Daha fazla bilgi için bkz. [e-posta bir kerelik geçiş kodu kimlik doğrulaması (Önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) ve blogda [Azure AD, herhangi bir hesabı kullanarak tüm kullanıcılar için paylaşım ve işbirliği yapma olanağı sunar](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949).

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Yeni Azure AD Uygulama Ara Sunucusu tanımlama bilgisi ayarları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Uygulama proxy 'Si aracılığıyla yayınlanan uygulamalarınız için kullanılabilen üç yeni tanımlama bilgisi ayarı ekledik:

- **Yalnızca HTTP tanımlama bilgisini kullanın.** Uygulama proxy 'Si erişiminizin ve oturum tanımlama bilgilerinizdeki **HttpOnly** bayrağını ayarlar. Bu ayarı açmak, istemci tarafı komut dosyası aracılığıyla tanımlama bilgilerinin kopyalanmasını veya değiştirilmesini önlemeye yardımcı olmak gibi ek güvenlik avantajları sağlar. Bu bayrağı açmanızı öneririz ( **Evet**' i seçin).

- **Güvenli tanımlama bilgisini kullanın.** Uygulama proxy 'Si erişiminizin ve oturum tanımlama bilgilerinde **güvenli** bayrağını ayarlar. Bu ayarı açmak, tanımlama bilgilerinin yalnızca HTTPS gibi TLS güvenli kanallarından aktarılmasını sağlamak için ek güvenlik avantajları sağlar. Bu bayrağı açmanızı öneririz ( **Evet**' i seçin).

- **Kalıcı tanımlama bilgisini kullanın.** Web tarayıcısı kapalıyken erişim tanımlama bilgilerinin süresinin dolmasını önler. Bu tanımlama bilgileri, erişim belirtecinin ömrü için en son. Ancak, süre sonu zamanına ulaşıldığında veya Kullanıcı tanımlama bilgisini el ile silerse tanımlama bilgileri sıfırlanır. Varsayılan ayar olarak, yalnızca süreçler arasında tanımlama bilgilerini paylaşmayan eski uygulamalar **için ayarı açarak**yapmanız önerilir.

Yeni tanımlama bilgileri hakkında daha fazla bilgi için bkz. [Azure Active Directory ' de şirket içi uygulamalara erişmek Için tanımlama bilgisi ayarları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ocak 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2019 Ocak 'ta, uygulama galerisine federasyon desteğiyle bu 35 yeni uygulamalar ekledik:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [izlenen](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Tatatze paleti](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [cloudsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco şemsiye](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet erişimi Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [süre sonu anımsatıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [EVR Görüntüleyicisi](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [corptax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [ Fiil](https://app.verb.net/login), [openlatce](https://openlattice.com/agora), [theorgwiki](https://www.theorgwiki.com/signup), [pavaso Digital Close](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [goodalıştırma araç seti](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [bulut hizmeti Picco](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [auditboard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [worlenebilir](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [calllütfen ](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO System](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE serviceınsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [Ares for Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [Office 365 için K2](https://www.k2.com/O365), [xledger](https://www.xledger.net/), [idıd Yöneticisi ](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [Highdişli](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Fraz alp](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle csaas platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Yeni Azure AD Kimlik Koruması geliştirmeleri (Genel Önizleme)

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Aşağıda aşağıdakiler dahil olmak üzere Azure AD Kimlik Koruması genel önizleme sunumuna aşağıdaki geliştirmeleri eklediğimiz duyurumuz ediyoruz.

- Güncelleştirilmiş ve daha tümleşik bir kullanıcı arabirimi

- Diğer API’ler

- Machine Learning ile iyileştirilmiş risk değerlendirmesi

- Riskli kullanıcılar ve riskli oturum açma işlemleri arasında ürün genelinde hizalama

Geliştirmeler hakkında daha fazla bilgi için bkz. [ne Azure Active Directory kimlik koruması (YENİLENDİ)?](https://aka.ms/IdentityProtectionDocs) daha fazla bilgi edinmek ve fikirlerinizi ürün içi istemler aracılığıyla paylaşmak için.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>İOS ve Android cihazlarda Microsoft Authenticator uygulaması için yeni uygulama kilidi özelliği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Microsoft Authenticator uygulaması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Tek seferlik geçiş kodlarınızın, uygulama bilgilerinizin ve uygulama ayarlarının daha güvenli olmasını sağlamak için Microsoft Authenticator uygulamasındaki uygulama kilidi özelliğini açabilirsiniz. Uygulama kilidi 'ni açmak, Microsoft Authenticator uygulamasını her açışınızda PIN 'inizi veya biyometri kullanarak kimlik doğrulaması yapmanız istenecektir.

Daha fazla bilgi için bkz. [uygulama sss Microsoft Authenticator](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Gelişmiş Azure AD Privileged Identity Management (PıM) dışa aktarma özellikleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management

Privileged Identity Management (PıM) yöneticileri artık tüm alt kaynaklar için rol atamaları içeren belirli bir kaynak için tüm etkin ve uygun rol atamalarını dışarı aktarabilir. Daha önce, yöneticilerin bir abonelik için rol atamalarının tamamen bir listesini alması ve her belirli kaynak için rol atamalarını dışarı aktarmaları gerekiyordu.

Daha fazla bilgi için bkz. [PIM 'de Azure Kaynak rolleri için etkinliği ve denetim geçmişini görüntüleme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

## <a name="novemberdecember-2018"></a>Kasım/Aralık 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Eşitleme kapsamından kaldırılan kullanıcılar artık yalnızca bulut hesaplarına geçmesiz

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kullanıcı Yönetimi  
**Ürün yeteneği:** Dizinden

>[!Important]
>Bu çözüm nedeniyle rahatsız etmemizi duyduk ve anladık. Bu nedenle, bu değişikliği kuruluşunuzda uygulamanızı daha kolay hale getirmek için bu süre kadar geri döndürüyoruz.

Active Directory Domain Services (AD DS) nesnesi eşitleme kapsamından hariç tutulduğunda ve ardından aşağıdakiler üzerinde Azure AD 'ye taşındıktan sonra kullanıcının DirSyncEnabled bayrağının **hatalı olarak yanlış** olarak geçtiğimiz bir hata düzeltildi Eşitleme çevrimi. Bu düzeltmeyle ilgili bir sonuç olarak, Kullanıcı eşitleme kapsamından dışlandığından ve daha sonra Azure AD geri dönüşüm kutusu 'ndan geri yüklenirse, Kullanıcı hesabı şirket içi AD 'den eşitlenmiş olarak kalır ve bu nedenle, yetki kaynağı (SoA) olarak kaldığı için bulutta yönetilemez. Şirket içi AD.

Bu düzeltmeyle önce, DirSyncEnabled bayrağı false olarak geçildiğinde bir sorun oluştu. Bu hesaplar, bu hesapların yalnızca bulut nesnelerine dönüştürüldüğüne ve hesapların bulutta yönetilebileceğinden yanlış izlenimi verdi. Ancak, hesaplar hala SoA 'yi şirket içinde ve şirket içi AD 'den gelen tüm eşitlenmiş Özellikler (gölge öznitelikleri) olarak bekletilir. Bu durum, Azure AD 'de ve diğer bulut iş yüklerinde (Exchange Online gibi), bu hesapların AD 'den eşitlenmiş olduğunu ancak artık yalnızca bulut hesapları gibi davranmasının beklendiği bir şekilde daha fazla soruna neden oldu.

Şu anda, eşitlenmiş bir AD hesabını gerçekten yalnızca bulut hesabına dönüştürmenin tek yolu, bir arka uç işlemini SoA aktarmaya tetikleyen kiracı düzeyinde DirSync 'i devre dışı bırakmalıdır. Bu tür SoA değişikliği, şirket içi ilgili tüm öznitelikleri (LastDirSyncTime ve Shadow öznitelikleri gibi) temizlemek ve diğer bulut iş yüklerine bir sinyal göndermek için, ilgili nesnenin de yalnızca bulut hesabına dönüştürülmesi için bir sinyal göndermesi gerekir .

Bu, sonuçta, geçmişteki bazı senaryolarda, AD 'den eşitlenen bir kullanıcının ImmutableID özniteliğinde doğrudan güncelleştirmelerin yapılmasını önler. Tasarım yaparak, adın gösterdiği gibi Azure AD 'deki bir nesnenin ImmutableID, sabit olması anlamına gelir. Azure AD Connect Health ve Azure AD Connect eşitleme istemcisinde uygulanan yeni özellikler, bu senaryolara yönelik olarak kullanılabilir:

- **Aşamalı bir yaklaşımda çok sayıda kullanıcı için büyük ölçekli ImmutableID güncelleştirmesi**
  
  Örneğin, uzun AD DS ormanlar arası geçiş yapmanız gerekir. Çözüm: **kaynak bağlantısını yapılandırmak** Için Azure AD Connect kullanın ve Kullanıcı geçirildiğinde, mevcut ImmutableID değerlerini Azure AD DS ad 'den yeni ormanın ms-DS-tutarlılık-GUID özniteliğine kopyalayın. Daha fazla bilgi için bkz. [MS-DS-ımıbu GUID 'ı Sourcetutturucu olarak kullanma](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor).

- **Tek bir görüntüsündeki birçok kullanıcı için büyük ölçekli ImmutableID güncelleştirmeleri**

  Örneğin, Azure AD Connect uygulanırken bir hata yaparsınız ve şimdi Sourcetutturucu özniteliğini değiştirmeniz gerekir. Çözüm: kiracı düzeyinde DirSync 'i devre dışı bırakın ve tüm geçersiz ImmutableID değerlerini temizleyin. Daha fazla bilgi için bkz. [Office 365 için dizin eşitlemeyi devre dışı bırakma](/office365/enterprise/turn-off-directory-synchronization).

- **Azure AD 'de mevcut bir kullanıcıyla şirket içi Kullanıcı Ile yeniden eşleştir** Örneğin, AD DS ' de yeniden oluşturulmuş bir kullanıcı var olan bir Azure AD hesabıyla (yalnız bırakılmış nesne) yeniden eşleştirmek yerine Azure AD hesabında yinelenen bir öğe oluşturur. Çözüm: kaynak bağlayıcısını/ImmutableID yeniden eşlemek için Azure portal Azure AD Connect Health kullanın. Daha fazla bilgi için bkz. [yalnız bırakılmış nesne senaryosu](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario).

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Son değişiklik: Azure Izleyici aracılığıyla denetim ve oturum açma günlüğü şemasında yapılan güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

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
|appliedConditionalAccessPolicies|Bu **Conditionalaccesspolicies** alanıdır. Artık **appliedConditionalAccessPolicies** alanıdır.|Değişiklik yok|Değişiklik yok|
|conditionalAccessStatus|Oturum açma sırasında koşullu erişim Ilkesi durumunun sonucunu sağlar. Bu, daha önce numaralandırılmıştı, ancak şimdi gerçek değeri gösteriyoruz.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|
|appliedConditionalAccessPolicies: sonuç|Oturum açma sırasında bireysel koşullu erişim Ilkesi durumunun sonucunu sağlar. Bu, daha önce numaralandırılmıştı, ancak şimdi gerçek değeri gösteriyoruz.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|

Şema hakkında daha fazla bilgi için bkz. [Azure izleyici 'de Azure AD denetim günlükleri şemasını yorumlama (Önizleme)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Denetimli makine öğrenimi modeli ve risk puanı altyapısında kimlik koruması geliştirmeleri

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Risk puanları

Kimlik koruması ile ilgili Kullanıcı ve oturum açma risk değerlendirmesi altyapısı geliştirmeleri, Kullanıcı risk doğruluğu ve kapsamını geliştirmeye yardımcı olabilir. Yöneticiler, Kullanıcı risk düzeyinin artık belirli algılamaların risk düzeyine doğrudan bağlı olmadığını ve riskli oturum açma olaylarının sayısı ve düzeyinde bir artış olduğunu fark edebilir.

Risk algılamaları artık kullanıcının oturum açma işlemlerinin ek özelliklerini ve bir algılama düzenlerini kullanarak Kullanıcı riskini hesaplayan, denetimli makine öğrenimi modeli tarafından değerlendirilir. Yönetici, bu modele bağlı olarak, bu kullanıcıyla ilişkili algılamalar düşük veya orta riskli olsa bile, yüksek riskli puanları olan kullanıcılar tarafından bulunabilir. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Yöneticiler, Microsoft Authenticator uygulamasını kullanarak kendi parolalarını sıfırlayabilir (Genel Önizleme)

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD yöneticileri artık Microsoft Authenticator uygulama bildirimlerini veya herhangi bir mobil kimlik doğrulayıcı uygulamasının veya donanım belirtecinden bir kodu kullanarak kendi parolasını sıfırlayabilir. Yöneticiler, kendi parolasını sıfırlamak için şu yöntemlerden ikisini de kullanabilir:

- Microsoft Authenticator uygulama bildirimi

- Diğer mobil kimlik doğrulayıcı uygulaması/donanım belirteci kodu

- E-posta

- Telefon araması

- Kısa mesaj

Parolaları sıfırlamak için Microsoft Authenticator uygulamasını kullanma hakkında daha fazla bilgi için bkz. [Azure AD self servis parola sıfırlama-mobil uygulama ve SSPR (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Yeni Azure AD bulut aygıtı yönetici rolü (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Cihaz kaydı ve yönetimi  
**Ürün yeteneği:** Erişim denetimi

Yöneticiler, bulut aygıtı yönetici görevlerini gerçekleştirmek için kullanıcıları yeni bulut cihaz yöneticisi rolüne atayabilir. Bulut cihaz yöneticileri rolüne atanan kullanıcılar, Azure AD 'de cihazları etkinleştirebilir, devre dışı bırakabilir ve silebilir ve Azure portal Windows 10 BitLocker anahtarlarını (varsa) okuyabilir.

Roller ve izinler hakkında daha fazla bilgi için bkz. [Azure Active Directory yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Azure AD 'de yeni etkinlik zaman damgasını kullanarak cihazlarınızı yönetin (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Cihaz kaydı ve yönetimi  
**Ürün yeteneği:** Cihaz yaşam döngüsü yönetimi

Ortamınızda eski cihazları kullanmaktan kaçınmak için, Azure AD 'de kuruluşların cihazlarını yenilemeniz ve devre dışı bırakmanız gerektiğini fark ettik. Azure AD, bu işleme yardımcı olmak için artık cihazlarınızı yeni bir etkinlik zaman damgasıyla güncelleştirir ve cihaz yaşam döngüsünü yönetmenize yardımcı olur.

Bu zaman damgasını alma ve kullanma hakkında daha fazla bilgi için bkz [. nasıl yapılır: Azure AD 'de eski cihazları yönetme](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Yöneticiler, kullanıcıların her cihazda kullanım koşullarını kabul etmesini gerektirebilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #C1
 
Yöneticiler artık kullanıcılarınızın kiracınızda kullandıkları her cihazda kullanım koşullarınızı kabul etmesini gerektirmek için **kullanıcıların her cihazda izin vermesini iste** seçeneğini açabilir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin cihaz başına kullanım koşulları bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Yöneticiler, yinelenen bir zamanlamaya göre kullanım koşulları 'nı zaman dolacak şekilde yapılandırabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #C1
 

Yöneticiler artık, belirtilen yinelenen zamanlamanıza bağlı olarak tüm kullanıcılarınız için kullanım süresini dolacak şekilde **süre sonu onayları** seçeneğini açabilir. Zamanlama yıllık, iki yıllık, üç aylık veya aylık olabilir. Kullanım koşulları sona erdiğinde, kullanıcıların yeniden kabul etmesi gerekir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin kullanım koşulları ekleme bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Yöneticiler, her bir kullanıcının zamanlamasını temel alarak kullanım koşullarını yapılandırabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #C1

Yöneticiler artık kullanıcının kullanım koşullarını yeniden kabul etmesi için bir süre belirtebilir. Örneğin, Yöneticiler kullanıcıların her 90 günde bir kullanım koşulları 'nı yeniden kabul etmesi gerektiğini belirtebilir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin kullanım koşulları ekleme bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)bakın.
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Azure Active Directory rolleri için yeni Azure AD Privileged Identity Management (PıM) e-postaları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Azure AD Privileged Identity Management (PıM) kullanan müşteriler artık son yedi güne ait aşağıdaki bilgiler dahil olmak üzere haftalık bir Özet e-postası alabilir:

- En uygun ve kalıcı rol atamalarına genel bakış

- Rolleri etkinleştiren kullanıcı sayısı

- PıM 'de rollere atanan kullanıcı sayısı

- PıM dışında rollere atanan kullanıcı sayısı

- PıM 'de "kalıcı hale getirilir" Kullanıcı sayısı

PıM ve kullanılabilir e-posta bildirimleri hakkında daha fazla bilgi için bkz. [PIM 'de e-posta bildirimleri](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications).

---

### <a name="group-based-licensing-is-now-generally-available"></a>Grup tabanlı lisanslama genel kullanıma sunuldu

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Dizinden

Grup tabanlı lisanslama genel önizleme aşamasındadır ve genel kullanıma sunulmuştur. Bu genel yayının bir parçası olarak, bu özelliği daha ölçeklenebilir hale aldık ve tek bir kullanıcı için grup tabanlı lisanslama atamalarını yeniden işleme özelliği ve Office 365 E3/a3 lisanslarıyla grup tabanlı lisanslama kullanma özelliği ekledik.

Grup tabanlı lisanslama hakkında daha fazla bilgi için bkz. [Azure Active Directory grup tabanlı lisanslama nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Kasım 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
2018 Kasım 'da, uygulama galerisine federasyon desteğiyle bu 26 yeni uygulama ekledik:

[Corestack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [Getno](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [GRA-PE](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [ehour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [appınux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [drivedolar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview), [useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [sonsuz kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [alaya](https://alayagood.com/en/demo/), [heyıarkadaş](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial) [, Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy for Business Central 365](https://accounting.zenegy.com/), [yaprak Köprüsü üye portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [ıDEO](https://profile.ideo.com/users/sign_up), [ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [allbağlanacak SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [parçalı uygulamalar-klasik test ](https://test.plexonline.com/signon), [Parçalı uygulamalar – klasik](https://www.plexonline.com/signon), [parçalı uygulamalar-UX testi](https://test.cloud.plex.com/sso), [parçalı uygulamalar – UX](https://cloud.plex.com/sso), [parçalı uygulamalar – IAM](https://accounts.plex.com/), [öğelerini-childbakım kayıtları, katılım, & finansal izleme sistemi](https://getcrafts.ca/craftsregistration) 

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

## <a name="october-2018"></a>Ekim 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD günlükleri artık Azure Log Analytics ile çalışıyor (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD günlüklerinizi artık Azure Log Analytics ilettiğini duyurmaktan mutluluk duyuyoruz! Bu en iyi istenen özellik, iş, işlemler ve güvenlik için Analize daha iyi erişim sağlamanıza yardımcı olur ve altyapınızı izlemeye yardımcı olmak için bir yoldur. Daha fazla bilgi için bkz. [Azure 'Daki etkinlik günlüklerine Azure Active Directory artık blog Log Analytics](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) .

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ekim 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2018 Ekim 'de, uygulama galerisine federasyon desteği olan bu 14 yeni uygulamaları ekledik:

[Ikramiye noktalarım](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [VİŞ HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), Ambyint, [myworkdrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [ödünç kutusu](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), dialpad, [ON24 sanal ortamı](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [halka orta](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler üç](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [workspot Denetim](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="azure-ad-domain-services-email-notifications"></a>E-posta bildirimlerini Azure AD Domain Services

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Azure AD Domain Services, yanlış yapılandırma veya yönetilen etki alanı ile ilgili sorunlar hakkında Azure portal uyarılar sağlar. Bu uyarılar, destek ile iletişime geçerek sorunları gidermeyi deneyebilmeniz için adım adım kılavuzlar içerir.

Ekim 'den başlayarak, yönetilen etki alanınız için bildirim ayarlarını özelleştirerek yeni uyarılar gerçekleştiğinde, belirlenen bir kişi grubuna e-posta gönderilir ve portalda güncelleştirmeler için sürekli olarak denetlenmesi gereğini ortadan kaldırır.

Daha fazla bilgi için [Azure AD Domain Services bildirim ayarları](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications)bölümüne bakın.

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD Portal, özel etki alanlarını silmek için ForceDelete etki alanı API 'sini kullanmayı destekler 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Dizin Yönetimi  
**Ürün yeteneği:** Dizinden

Özel etki alanı adınızdan (contoso.com) Kullanıcı, Grup ve uygulamalar gibi başvuruları, ilk varsayılan etki alanı adına geri yükleyerek özel etki alanı adlarınızı silmek için artık ForceDelete etki alanı API 'sini kullanmak üzere duyuyoruz. contoso.onmicrosoft.com).

Bu değişiklik, kuruluşunuz artık adı kullanmadıysa veya başka bir Azure AD ile etki alanı adını kullanmanız gerekiyorsa, özel etki alanı adlarınızı daha hızlı bir şekilde silmenizi sağlar.

Daha fazla bilgi için bkz. [özel etki alanı adını silme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>Eylül 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Dinamik Gruplar için yönetici rolü izinleri güncelleştirildi

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Belirli Yönetici rollerinin artık grubun sahibi olması gerekmeden dinamik üyelik kuralları oluşturup güncelleştirebilmesi için bir sorunu düzelttik.

Roller şunlardır:

- Genel yönetici

- Intune Yöneticisi

- Kullanıcı Yöneticisi

Daha fazla bilgi için bkz. [dinamik grup oluşturma ve durumu denetleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Bazı üçüncü taraf uygulamalar için Basitleştirilmiş çoklu oturum açma (SSO) yapılandırma ayarları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Hizmet olarak yazılım (SaaS) uygulamaları için çoklu oturum açma (SSO) ayarlamanın, her bir uygulama yapılandırmasının benzersiz yapısı nedeniyle zorlayıcı bir şekilde ayarlanmasını fark ettik. Aşağıdaki üçüncü taraf SaaS uygulamalarına yönelik SSO yapılandırma ayarlarını otomatik olarak doldurmak için basitleştirilmiş bir yapılandırma deneyimi geliştirdik:

- Zendesk

- Çevrimiçi olan ArcGIS

- Jamf Pro

Bu tek tıklamayla deneyim kullanmaya başlamak için, uygulamanın **Azure portal** > **SSO yapılandırma** sayfasına gidin. Daha fazla bilgi için bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Azure Active Directory-verileriniz nerede bulunuyor? sayfasında

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** GoLocal

Tüm Azure AD Hizmetleri için Azure AD verilerinizi bekleyen Azure veri merkezini görüntülemek için **Azure Active Directory** şirketinizin bölgesini seçin. Bilgileri şirketinizin bölgesi için belirli Azure AD hizmetlerine göre filtreleyebilirsiniz.

Bu özelliğe erişmek ve daha fazla bilgi için, bkz. [Azure Active Directory-verileriniz nerede bulunur](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Uygulamalarım erişim paneli için yeni dağıtım planı kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO

Uygulamalarım erişim paneli (https://aka.ms/deploymentplans) için kullanılabilen yeni dağıtım planına göz atın.
Uygulamalarım erişim paneli kullanıcılara uygulamaları bulmak ve bunlara erişmek için tek bir yer sağlar. Bu portal Ayrıca kullanıcılara, uygulamalar ve gruplar için erişim isteme ya da diğer kişilerin adına bu kaynaklara erişimi yönetme gibi self servis fırsatları sağlar.

Daha fazla bilgi için bkz. [uygulamalarım portalı nedir?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Azure portal oturum açma günlükleri sayfasında yeni sorun giderme ve destek sekmesi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure portal **oturum açma** Işlemlerinin yeni **sorun giderme ve destek** sekmesi, yöneticilerin ve Destek mühendislerinin Azure AD oturum açma konularıyla ilgili sorunları gidermelerine yardımcı olmak için tasarlanmıştır. Bu yeni sekme, sorunu çözmeye yardımcı olmak için hata kodu, hata iletisi ve düzeltme önerilerini (varsa) sağlar. Sorunu çözemezse, bir destek bileti **oluşturmak için,** destek biletinizdeki günlük dosyası IÇIN **Istek kimliği** ve **Tarih (UTC)** alanlarını dolduran bir destek bileti oluşturmaya yönelik yeni bir yol da sunuyoruz.  

![Yeni sekmeyi gösteren oturum açma günlükleri](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Dinamik üyelik kuralları oluşturmak için kullanılan özel uzantı özellikleri için gelişmiş destek

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Bu güncelleştirmeyle, artık dinamik Kullanıcı grubu kuralı oluşturucusunun **özel uzantı özellikleri al** bağlantısına tıklayabilir, BENZERSIZ uygulama kimliğinizi girebilir ve kullanıcılar için dinamik üyelik kuralı oluştururken kullanmak üzere özel uzantı özelliklerinin tam listesini alabilirsiniz. Bu liste ayrıca, bu uygulama için yeni özel uzantı özellikleri almak üzere yenilenebilir.

Dinamik üyelik kuralları için özel uzantı özellikleri kullanma hakkında daha fazla bilgi için bkz. [uzantı özellikleri ve özel uzantı özellikleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Aşağıdaki uygulamalar [onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement)listesidir:

- Microsoft To-Do

- Microsoft Stream

Daha fazla bilgi için bkz.

- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Windows 7/8/8.1 kilit ekranından self servis parola sıfırlama için yeni destek

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu yeni özelliği ayarladıktan sonra kullanıcılarınız, Windows 7, Windows 8 veya Windows 8.1 çalıştıran bir cihazın **kilit** ekranından parolalarını sıfırlama bağlantısını görür. Bu bağlantıya tıkladığınızda, Kullanıcı Web tarayıcısı aracılığıyla aynı parola sıfırlama akışında gezinerek yapılır.

Daha fazla bilgi için bkz. [Windows 7, 8 ve 8,1 ' de parola sıfırlamayı etkinleştirme](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: yetkilendirme kodları artık yeniden kullanım için kullanılamayacak 

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

15 Kasım 2018 tarihinden itibaren Azure AD, uygulamalar için önceden kullanılan kimlik doğrulama kodlarını kabul etmeyi durduracak. Bu güvenlik değişikliği, Azure AD 'yi OAuth belirtimine göre satıra getirmeye yardımcı olur ve hem v1 hem de v2 uç noktalarında zorunlu kılınır.

Uygulamanız birden fazla kaynağa belirteç almak için yetkilendirme kodlarını yeniden kullanıyorsa, bir yenileme belirteci almak için kodu kullanmanızı ve ardından bu yenileme belirtecini kullanarak diğer kaynaklar için ek belirteçler elde etmenizi öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden çok kaynak arasında birden çok kez kullanılabilir. OAuth kod akışı sırasında bir kimlik doğrulama kodunu yeniden kullanmayı deneyen bir uygulama bir invalid_grant hatası alır.

Bu ve diğer protokollerle ilgili değişiklikler için, [kimlik doğrulaması yenilikleri hakkında tam listeye](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Eylül 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Eylül 2018 ' de, uygulama galerisine federasyon desteğiyle bu 16 yeni uygulamayı ekledik:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Cokarşılayan işe alma yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), ArcGIS [Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [jda Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [kar ke](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), Join.me, [zephyrsso](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riveryatak Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Azure için enlyft SSO, gözetimi, [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcıa](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="support-for-additional-claims-transformations-methods"></a>Ek talep dönüştürmeleri yöntemleri için destek

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

SAML tabanlı **Çoklu oturum açma yapılandırması** sayfasından SAML belirteçlerine uygulanabilen yeni talep dönüştürme yöntemleri (ToLower () ve ToUpper () sunuyoruz.

Daha fazla bilgi için bkz [. Azure AD 'de kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>SAML tabanlı uygulama yapılandırma kullanıcı arabirimi (Önizleme) güncelleştirildi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Güncelleştirilmiş SAML tabanlı uygulama yapılandırma Kullanıcı arabirimimizin bir parçası olarak şunları alacaksınız:

- SAML tabanlı uygulamalarınızı yapılandırmaya yönelik güncelleştirilmiş bir gözden geçirme deneyimi.

- Yapılandırmanızda nelerin eksik olduğu veya yanlış olduğu hakkında daha fazla görünürlük.

- Süre sonu sertifikası bildirimi için birden çok e-posta adresi ekleme özelliği.

- Yeni talep dönüştürme yöntemleri, ToLower () ve ToUpper () ve daha fazlası.

- Kurumsal uygulamalarınız için kendi belirteç imzalama sertifikanızı karşıya yüklemenin bir yolu.

- SAML uygulamaları için NameID biçimini ayarlamak için bir yol ve NameID değerini Dizin uzantıları olarak ayarlamak için bir yol.

Bu güncelleştirilmiş görünümü açmak için **Çoklu oturum açma** sayfasının en üstündeki **Yeni deneyimimizi deneyin** bağlantısına tıklayın. Daha fazla bilgi için bkz. [öğretici: Azure Active Directory olan bir uygulama IÇIN SAML tabanlı çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).

---

## <a name="august-2018"></a>Ağustos 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP adresi aralıklarına yapılan değişiklikler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Platformunun

Azure AD 'ye daha büyük IP aralıkları sunuyoruz, yani güvenlik duvarları, yönlendiricileriniz veya ağ güvenlik grupları için Azure AD IP adresi aralıklarını yapılandırdıysanız, bunları güncelleştirmeniz gerekir. Bu güncelleştirmeyi yaptığımız için, Azure AD yeni uç noktalar eklediğinde güvenlik duvarı, yönlendirici veya ağ güvenlik grupları IP aralığı yapılandırmalarının yeniden değiştirilmesini istemezsiniz. 

Ağ trafiği, sonraki iki ay içinde bu yeni aralığa taşınıyor. Kesintisiz hizmet ile devam etmek için, bu güncelleştirilmiş değerleri 10 Eylül 2018 ' dan önce IP adreslerinize eklemeniz gerekir:

- 20.190.128.0/18 

- 40.126.0.0/18 

Tüm ağ trafiğiniz yeni aralıklara taşınana kadar eski IP adresi aralıklarını kaldırmamak kesinlikle önerilir. Taşıma hakkındaki güncelleştirmeler ve eski aralıkları ne zaman kaldırabileceğinizi öğrenmek için bkz. [Office 365 URL 'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: yetkilendirme kodları artık yeniden kullanım için kullanılamayacak 

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

15 Kasım 2018 tarihinden itibaren Azure AD, uygulamalar için önceden kullanılan kimlik doğrulama kodlarını kabul etmeyi durduracak. Bu güvenlik değişikliği, Azure AD 'yi OAuth belirtimine göre satıra getirmeye yardımcı olur ve hem v1 hem de v2 uç noktalarında zorunlu kılınır.

Uygulamanız birden fazla kaynağa belirteç almak için yetkilendirme kodlarını yeniden kullanıyorsa, bir yenileme belirteci almak için kodu kullanmanızı ve ardından bu yenileme belirtecini kullanarak diğer kaynaklar için ek belirteçler elde etmenizi öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden çok kaynak arasında birden çok kez kullanılabilir. OAuth kod akışı sırasında bir kimlik doğrulama kodunu yeniden kullanmayı deneyen bir uygulama bir invalid_grant hatası alır.

Bu ve diğer protokollerle ilgili değişiklikler için, [kimlik doğrulaması yenilikleri hakkında tam listeye](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Self servis parola (SSPR) ve Multi-Factor Authentication (MFA) için yakınsanmış güvenlik bilgi yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Bu yeni özellik, kullanıcıların güvenlik bilgilerini (örneğin, telefon numarası, mobil uygulama vb.) SSPR ve MFA için tek bir konum ve deneyimle yönetmesine yardımcı olur; daha önce olduğu gibi, iki farklı konumda yapıldığı gibi.

Bu yakınsama deneyimi Ayrıca SSPR veya MFA kullanan kişiler için de geçerlidir. Ayrıca, kuruluşunuz MFA veya SSPR kaydını zorlayamazsa, kullanıcılar uygulama tarafından izin verilen tüm MFA veya SSPR güvenlik bilgileri yöntemlerini uygulamalarım portalından kaydedebilir.

Bu, isteğe bağlı bir genel önizlemedir. Yöneticiler seçili bir grup veya bir Kiracıdaki tüm kullanıcılar için yeni deneyimi (isteniyorsa) açabilir. Yakınsama deneyimi hakkında daha fazla bilgi için, bkz. [Yakınsanan deneyim blogu](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD uygulama ara sunucusu uygulamalarında yalnızca yeni HTTP tanımlama bilgileri ayarı

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Uygulama ara sunucusu uygulamalarınızda **yalnızca http tanımlama bilgileri** adlı yeni bir ayar vardır. Bu ayar, hem uygulama proxy 'Si erişimi, hem de oturum tanımlama bilgileri için HTTP yanıt üst bilgisine HTTPOnly bayrağını ekleyerek ek güvenlik sağlanmasına yardımcı olur, istemci tarafı betikten tanımlama bilgisine erişimi durduruyor ve kopyalama veya kopyalama gibi eylemleri daha da engelliyor tanımlama bilgisini değiştirme. Bu bayrak daha önce kullanılmasa da, tanımlama bilgileri, yanlış değişikliklere karşı korumaya yardımcı olmak için her zaman bir SSL bağlantısı kullanılarak şifrelenir ve iletilir.

Bu ayar, Uzak Masaüstü gibi ActiveX denetimlerini kullanan uygulamalarla uyumlu değildir. Bu durumda, bu ayarı kapatmanız önerilir.

Yalnızca HTTP tanımlama bilgileri ayarı hakkında daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Azure kaynakları için Privileged Identity Management (PıM), yönetim grubu kaynak türlerini destekler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Tam zamanında etkinleştirme ve atama ayarları artık abonelikler, kaynak grupları ve kaynaklar (VM 'Ler, uygulama hizmetleri ve daha fazlası gibi) için zaten yaptığınız gibi yönetim grubu kaynak türlerine uygulanabilir. Ayrıca, bir yönetim grubu için yönetici erişimi sağlayan role sahip olan herkes bu kaynağı PıM 'de bulabilir ve yönetebilir.

PıM ve Azure kaynakları hakkında daha fazla bilgi için bkz. [Privileged Identity Management kullanarak Azure kaynaklarını bulma ve yönetme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Uygulama erişimi (Önizleme) Azure AD portalına daha hızlı erişim sağlar

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Bugün, PıM kullanarak bir rolü etkinleştirirken, izinlerin etkili olması 10 dakika sürebilir. Şu anda genel önizleme aşamasında olan uygulama erişimini kullanmayı seçerseniz, Yöneticiler, etkinleştirme isteği tamamlandıktan hemen sonra Azure AD portalına erişebilir.

Şu anda uygulama erişimi yalnızca Azure AD Portal deneyimini ve Azure kaynaklarını destekler. PıM ve uygulama erişimi hakkında daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Ağustos 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Ağustos 2018 ' de, uygulama galerisine federasyon desteğiyle bu 16 yeni uygulamayı ekledik:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [bridgeline bağlanmamış](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sauce Labs-mobil ve Web testi](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [meta ağlar Bağlayıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [yaptığımız](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [Promaster (ınlogik tarafından)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [dossıer](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F-harcama raporları](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 canlı sohbet](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [safeconnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [zenqms](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eluminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Yerel Tableau desteği artık Azure AD Uygulama Ara Sunucusu kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

OpenID 'deki güncelleştirmemizden, ön kimlik doğrulama protokoliz için OAuth 2,0 kod verme protokolüne bağlanın, artık uygulama proxy 'Si ile Tableau kullanmak için ek yapılandırma yapmanız gerekmez. Bu protokol değişikliği Ayrıca, JavaScript ve HTML etiketlerinde yaygın olarak desteklenen yalnızca HTTP yeniden yönlendirmeleri kullanarak, uygulama proxy 'Sinin daha modern uygulamaları daha iyi desteklemesini sağlar.

Tableau için yerel destek hakkında daha fazla bilgi için bkz. [Yerel Tableau desteğiyle Azure AD uygulama ara sunucusu](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) içinde B2B Konuk kullanıcıları için bir kimlik sağlayıcısı olarak Google eklemek üzere yeni destek

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

Kuruluşunuzda Google ile Federasyon ayarlayarak, bir kişisel Microsoft hesabı (MSAs) veya bir Azure AD hesabı oluşturmaya gerek kalmadan, Gmail kullanıcılarının mevcut Google hesabını kullanarak paylaşılan uygulamalarınızda ve kaynaklarda oturum açmasını sağlayabilirsiniz.

Bu, isteğe bağlı bir genel önizlemedir. Google Federasyonu hakkında daha fazla bilgi için bkz. [B2B Konuk kullanıcıları için kimlik sağlayıcısı olarak Google ekleme](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Temmuz 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Azure Active Directory e-posta bildirimlerine yönelik iyileştirmeler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure Active Directory (Azure AD) e-postaları artık güncelleştirilmiş bir tasarımın yanı sıra, aşağıdaki hizmetlerden gönderildiğinde gönderenin e-posta adresi ve gönderen görünen adı üzerinde değişiklik gösterir:
 
- Azure AD erişim Incelemeleri
- Azure AD Connect Health 
- Azure AD Kimlik Koruması 
- Azure AD Privileged Identity Management
- Kurumsal uygulamanın süresi dolan sertifika bildirimleri
- Kurumsal uygulama sağlama hizmeti bildirimleri
 
E-posta bildirimleri şu e-posta adresinden ve görünen adla gönderilir:

- E-posta adresi: azure-noreply@microsoft.com
- Görünen ad: Microsoft Azure
 
Yeni e-posta tasarımlarının bir örneği ve daha fazla bilgi için bkz. [Azure AD PIM 'de e-posta bildirimleri](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD etkinlik günlükleri artık Azure Izleyici aracılığıyla kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlükleri artık Azure Izleyici (Azure 'un platform genelinde izleme hizmeti) için genel önizlemede kullanıma sunulmuştur. Azure Izleyici, bu geliştirmelere ek olarak uzun süreli saklama ve sorunsuz tümleştirme sağlar:

- Günlük dosyalarınızı kendi Azure depolama hesabınıza yönlendirerek uzun süreli saklama.

- Sorunsuz SıEM tümleştirmesi, Özel betikler yazmanızı veya bakımını gerektirmenize gerek kalmadan.

- Kendi özel çözümlerinizle, analiz araçlarınızla veya olay yönetimi çözümleriyle sorunsuz tümleştirme.

Bu yeni yetenekler hakkında daha fazla bilgi için bkz. Azure [izleyici tanılama 'daki blog Azure ad etkinlik günlükleri artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) ve belgelerimizde, [Azure izleyici 'de etkinlik günlükleri Azure Active Directory (Önizleme)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Azure AD oturum açma raporuna koşullu erişim bilgileri eklendi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Bu güncelleştirme, bir kullanıcı ilke sonucuyla birlikte oturum açtığında hangi ilkelerin değerlendirildiğini görmenizi sağlar. Ayrıca, rapor artık Kullanıcı tarafından kullanılan istemci uygulaması türünü içerir, böylece eski protokol trafiğini belirleyebilirsiniz. Rapor girdileri Ayrıca, kullanıcıya yönelik hata iletisinde bulunan bir bağıntı KIMLIĞI için aranabilir ve eşleşen oturum açma isteğini tanımlamak ve sorunlarını gidermek için kullanılabilir.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Oturum açma işlemleri etkinlik günlükleri aracılığıyla eski kimlik doğrulamaları görüntüleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme
 
Oturum açma etkinliği günlüklerindeki **Istemci uygulaması** alanının tanıtılmasıyla, müşteriler artık eski kimlik doğrulamaları kullanan kullanıcıları görebilirler. Müşteriler, Azure AD portalındaki oturum açma etkinlik günlükleri aracılığıyla veya bu bilgilere Graph API, eski kimlik doğrulamaları için filtre uygulamak üzere **Istemci uygulama** denetimini kullanabileceğiniz oturum açma etkinliği günlükleri aracılığıyla erişebilir. Daha fazla bilgi için belgelere göz atın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Temmuz 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Haziran 2018 ' de, uygulama galerisine federasyon desteğiyle bu 16 yeni uygulamayı ekledik:

[Yenilik hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [belirli yönetici SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), Psuc hazırlama, [IPASS SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [ekran kaydı-O-tek](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), powerokul Birleşik sınıf, [eli ekleme](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [bomgar uzak desteği](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex ](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 joinnow Bağlayıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [smartlpa](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [beceriler tabanı](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Yeni Kullanıcı hazırlama SaaS uygulaması tümleştirmeleri-2018 Temmuz

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Azure AD, Dropbox, Salesforce, ServiceNow gibi SaaS uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatik hale getirmenizi sağlar. 2018 Temmuz için, Azure AD uygulama galerisinde aşağıdaki uygulamalar için Kullanıcı hazırlama desteği ekledik:

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Primi](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Azure AD galerisinde Kullanıcı sağlamayı destekleyen tüm uygulamaların bir listesi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Eşitleme için Connect Health-yalnız bırakılmış ve yinelenen öznitelik eşitleme hatalarını gidermenin daha kolay bir yolu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** AD Connect  
**Ürün yeteneği:** & Raporlamayı izleme
 
Azure AD Connect Health, eşitleme hatalarını vurgulamanıza ve düzeltmemize yardımcı olmak için self servis düzeltmesini tanıtır. Bu özellik yinelenen öznitelik eşitleme hatalarını giderir ve Azure AD 'den yalnız bırakılmış nesneleri düzeltir. Bu tanılamada aşağıdaki avantajlar bulunur:

- Yinelenen öznitelik eşitleme hatalarını daraltır ve belirli düzeltmeleri sağlar

- Özel Azure AD senaryoları için bir çözüm uygular, tek bir adımda hataları çözer

- Bu özelliği açmak ve kullanmak için yükseltme veya yapılandırma gerekmez

Daha fazla bilgi için bkz. [yinelenen öznitelik eşitleme hatalarını tanılama ve](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors) Düzeltme

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Azure AD ve MSA oturum açma deneyimlerine yönelik görsel güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Microsoft 'un Office 365 ve Azure gibi çevrimiçi hizmetler oturum açma deneyimi için Kullanıcı arabirimini güncelleştirdik. Bu değişiklik ekranları daha az karışık ve daha basit hale getirir. Bu değişiklik hakkında daha fazla bilgi için bkz. [Azure AD oturum açma deneyimi bloguna yönelik yakında iyileştirmeler](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) .

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Azure AD Connect yeni sürümü-Temmuz 2018

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Azure AD Connect en son sürümü şunları içerir: 

- Hata düzeltmeleri ve desteklenebilirlik Updates 

- Ping-Federate tümleştirmesinin genel kullanılabilirliği

- En son SQL 2012 istemcisine yönelik güncelleştirmeler 

Bu güncelleştirme hakkında daha fazla bilgi için bkz [. Azure AD Connect: sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-end-user-ui"></a>Son Kullanıcı Kullanıcı arabirimi kullanım koşullarına göre güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #C1

TOU Son Kullanıcı Kullanıcı arabirimindeki kabul dizesini güncelleştiriyoruz.

**Geçerli metin.** [TenantName] kaynaklarına erişmek için kullanım koşullarını kabul etmeniz gerekir.<br>**Yeni metin.** [TenantName] kaynağına erişmek için kullanım koşullarını okumanız gerekir.

**Geçerli metin:** Kabul etmek için seçilmesi, yukarıdaki kullanım koşullarının tümünü kabul ettiğiniz anlamına gelir.<br>**Yeni metin:** Kullanım koşullarını okuduğunuzu ve anladığınızı onaylamak için lütfen kabul et ' e tıklayın.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Doğrudan kimlik doğrulaması eski protokolleri ve uygulamaları destekler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Doğrudan kimlik doğrulaması artık eski protokolleri ve uygulamaları desteklemektedir. Aşağıdaki sınırlamalar artık tam olarak desteklenmektedir:

- Modern kimlik doğrulaması gerekmeden eski Office istemci uygulamalarına, Office 2010 ve Office 2013 Kullanıcı oturum açma işlemleri.

- Yalnızca Office 2010 ' de Exchange karma ortamlarında Takvim paylaşımına ve ücretsiz/meşgul bilgilere erişim.

- Modern kimlik doğrulaması gerekmeden Skype Kurumsal istemci uygulamalarına Kullanıcı oturum açma işlemleri.

- PowerShell sürüm 1,0 'de Kullanıcı oturum açma işlemleri.

- Apple Aygıt Kayıt Programı (Apple DEP), iOS Kurulum Yardımcısı kullanılarak. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Self servis parola sıfırlama ve Multi-Factor Authentication için yakınsama güvenlik bilgileri yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu yeni özellik, kullanıcıların güvenlik bilgilerini (örneğin, telefon numarası, e-posta adresi, mobil uygulama vb.) self servis parola sıfırlama (SSPR) ve Multi-Factor Authentication (MFA) için tek bir deneyimde yönetmesine olanak tanır. Kullanıcılar artık SSPR ve MFA için aynı güvenlik bilgilerini iki farklı deneyimle kaydetmeyecektir. Bu yeni deneyim Ayrıca SSPR ya da MFA 'ya sahip kullanıcılar için de geçerlidir.

Bir kuruluş MFA veya SSPR kaydını zormıyorsa, kullanıcılar kendi güvenlik bilgilerini **uygulamalarım** portalı aracılığıyla kaydedebilir. Buradan, kullanıcılar MFA veya SSPR için etkinleştirilen tüm yöntemleri kaydedebilir. 

Bu, isteğe bağlı bir genel önizlemedir. Yöneticiler seçili bir Kullanıcı grubu veya bir Kiracıdaki tüm kullanıcılar için yeni deneyimi (isteniyorsa) açabilir.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Parolanızı sıfırladıktan sonra kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanın

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu özellik, Microsoft Authenticator (veya başka bir Authenticator uygulamadan) bir bildirim veya kod kullanarak bir parolayı sıfırlarken yönetici olmayan kimlik doğrulamasını sağlar. Yöneticiler bu Self servis parola sıfırlama yöntemini etkinleştirdikten sonra, aka.ms/mfasetup veya aka.ms/setupsecurityinfo aracılığıyla bir mobil uygulama kaydeden kullanıcılar, parolalarını sıfırlarken kendi mobil uygulamalarını doğrulama yöntemi olarak kullanabilir.

Mobil uygulama bildirimi, yalnızca parolanızı sıfırlamak için iki yöntem gerektiren bir ilkenin parçası olarak açılabilir.

---

## <a name="june-2018"></a>Haziran 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Değişiklik bildirimi: Azure AD etkinlik günlükleri API 'sini kullanarak uygulamalar için yetkilendirilmiş yetkilendirme akışına güvenlik düzeltme

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Platformunun

Aktarım Katmanı Güvenliği (TLS), iki iletişim uygulaması arasında gizlilik ve veri bütünlüğü sağlayan bir protokoldür ve günümüzde kullanılan en yaygın olarak dağıtılan güvenlik protokolüdür.

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) , uyumluluk **30 Haziran 2018**' den itibaren, yeni ve daha güvenli uygulama PROTOKOLLERININ etkinleştirilmesi için eski TLS ve Güvenli Yuva Katmanı (SSL) sürümlerinin devre dışı bırakılması gerektiğini belirledi. Bu değişiklik, Azure AD hizmetlerine bağlanıp PCI DSS uyumluluğunu gerektirdiğinde TLS 1,0 ' i devre dışı bırakmanız anlamına gelir. Birden çok TLS sürümü mevcuttur, ancak TLS 1,2 Azure Active Directory Hizmetleri için kullanılabilen en son sürümdür. Hem istemci/sunucu hem de tarayıcı/sunucu birleşimleri için doğrudan TLS 1,2 ' e geçmeyi öneririz.

Güncel olmayan tarayıcılar TLS 1,2 gibi daha yeni TLS sürümlerini desteklemeyebilir. Tarayıcı tarafından hangi TLS sürümlerinin desteklendiğini görmek için [Qualys SSL Labs](https://www.ssllabs.com/) sitesine gidin ve **tarayıcınızı test et**' e tıklayın. Web tarayıcınızın en son sürümüne yükseltmenizi ve tercihen yalnızca TLS 1,2 ' i etkinleştirmenizi öneririz.

**TLS 1,2 'yi tarayıcıda etkinleştirmek için**

- **Microsoft Edge ve Internet Explorer (her ikisi de Internet Explorer kullanılarak ayarlanır)**

    1. Internet Explorer 'ı açın, **gelişmiş** > **Internet seçenekleri** > **Araçlar** ' ı seçin.
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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Haziran 2018 ' de, uygulama galerisine federasyon desteğiyle bu 15 yeni uygulamayı ekledik:

[Ufuk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [müzik kapatma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1,1 belirteci etkin lob uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [süper ruh](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [otomatik görev](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [uç nokta yedekleme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ufuk yüksek ağlar](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [tonicdm](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [SharePoint şirket içi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [ForeSee CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [lıakx, ıkıx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial) [](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD parola koruması genel önizlemede kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Ortamınızdaki kolayca tahmin edilen parolaları ortadan kaldırmaya yardımcı olması için Azure AD parola korumasını kullanın. Bu parolaların ortadan kaldırılması, parola spreyi saldırılarına karşı riskli olma riskini azaltmaya yardımcı olur.

Azure AD parola koruması özellikle şunları yapmanıza yardımcı olur:

- Kuruluşunuzun hesaplarını hem Azure AD hem de Windows Server Active Directory (AD) içinde koruyun. 
- Kullanıcılarınızın, en sık kullanılan parolaların 500 ' inden daha fazla bir listede parola kullanmasını ve bu parolaların 1.000.000 karakterlik değiştirme çeşitlemelerini engeller.
- Azure AD parola korumasını Azure AD portalındaki tek bir konumdan, hem Azure AD hem de şirket içi Windows Server AD için yönetin.

Azure AD parola koruması hakkında daha fazla bilgi için bkz. [kuruluşunuzdaki kötü parolaları kaldırma](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında yeni "tüm konuklar" koşullu erişim ilkesi şablonu oluşturuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #C1

Kullanım koşullarınızın oluşturulması sırasında, "tüm konuklar" ve "tüm uygulamalar" için de yeni bir koşullu erişim ilkesi şablonu oluşturulur. Bu yeni ilke şablonu, yeni oluşturulan ToU 'yi uygular ve konuklar için oluşturma ve zorlama sürecini hızlandırma.

Daha fazla bilgi için bkz. [Azure Active Directory kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında yeni "özel" koşullu erişim ilkesi şablonu oluşturuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #C1

Kullanım koşullarınızın oluşturulması sırasında yeni bir "özel" koşullu erişim ilkesi şablonu da oluşturulur. Bu yeni ilke şablonu, ToU 'yi oluşturmanızı ve ardından doğrudan portalda gezinmenize gerek kalmadan koşullu erişim ilkesi oluşturma dikey penceresine gitmenizi sağlar.

Daha fazla bilgi için bkz. [Azure Active Directory kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication dağıtma hakkında yeni ve kapsamlı kılavuz

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Kuruluşunuzda Azure Multi-Factor Authentication (MFA) dağıtma hakkında yeni adım adım yönergeler yayımladık.

MFA dağıtım kılavuzunu görüntülemek için GitHub 'daki [kimlik dağıtım kılavuzlarını](https://aka.ms/DeploymentPlans) depoya gidin. Dağıtım kılavuzlarıyla ilgili geri bildirim sağlamak için [dağıtım planı geri bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzlarıyla ilgili sorularınız varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bizimle iletişime geçin.

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD tarafından atanan uygulama yönetimi rolleri genel önizlemede

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** Access Control

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

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Platformunun  

Azure Active Directory (Azure AD) gibi bir hizmet sunumu, ExpressRoute veya diğer özel VPN tünellerine gerek duymadan doğrudan Internet üzerinden çalışacak şekilde tasarlanmıştır. Bu nedenle, **1 ağustos 2018**' de Azure genel eşleme ve Microsoft eşleme 'de Azure toplulukları KULLANıLARAK Azure AD Hizmetleri Için ExpressRoute desteğini durduracağız. Bu değişiklikten etkilenen tüm hizmetler, Azure AD trafiğinin yavaş şekilde ExpressRoute 'a Internet 'e kaydırdığına bildirimde bulunabilir.

Desteğimizi değiştirirken, kimlik doğrulama trafiğiniz için adanmış bir devre kümesi kullanmanız gerekebilecek durumlar olduğunu da biliyoruz. Bu nedenle, Azure AD "diğer Office 365 Online Services" topluluğuyla Microsoft eşliğinde zaten ExpressRoute ve hizmetleri kullanan kiracı başına IP aralığı kısıtlamalarını desteklemeye devam edecektir. Hizmetleriniz etkilenirse, ancak ExpressRoute gerekliyse, aşağıdakileri yapmanız gerekir:

- **Azure genel eşleme kullanıyorsanız.** Microsoft eşlemesi ' ne gidin ve **diğer Office 365 Online Services (12076:5100)** topluluğuna kaydolun. Azure genel eşlemeden Microsoft eşlemesine nasıl geçeceğimiz hakkında daha fazla bilgi için bkz. [genel eşlemeyi Microsoft eşlemesi 'Ne taşıma](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) makalesi.

- **Microsoft eşleme kullanıyorsanız.** **Diğer Office 365 çevrimiçi hizmet (12076:5100)** topluluğu için kaydolun. Yönlendirme gereksinimleri hakkında daha fazla bilgi için, ExpressRoute yönlendirme gereksinimleri makalesinin [BGP toplulukları Için destek bölümüne](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) bakın.

Adanmış devreleri kullanmaya devam etmeniz gerekiyorsa, **diğer Office 365 çevrimiçi hizmet (12076:5100)** Community 'yi kullanma yetkisi alma hakkında Microsoft hesabı ekibinizle konuşmanız gerekir. MS Office tarafından yönetilen gözden geçirme panosu, bu devrelere ihtiyacınız olup olmadığını doğrular ve bunları tutmanın teknik etkilerini anladığınızdan emin olmanızı sağlar. Office 365 için yol filtreleri oluşturmaya çalışan yetkisiz abonelikler, bir hata iletisi alır. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>TOU için yönetim senaryoları için Microsoft Graph API 'Leri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Geliştirici deneyimi
 
Azure AD kullanım koşulları 'nın yönetim işlemi için Microsoft Graph API 'Leri ekledik. Kullanım koşulları nesnesi oluşturabilir, güncelleştirebilir, silebilirsiniz.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Azure AD B2C bir kimlik sağlayıcısı olarak Azure AD çok kiracılı uç noktası ekleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Özel ilkeleri kullanarak artık Azure AD ortak uç noktasını Azure AD B2C bir kimlik sağlayıcısı olarak ekleyebilirsiniz. Bu, uygulamalarınızda oturum açan tüm Azure AD kullanıcıları için tek bir giriş noktasına sahip etmenize olanak tanır. Daha fazla bilgi için bkz. [Azure Active Directory B2C: kullanıcıların özel ilkeleri kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısında oturum açmalarına Izin verin](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Uygulamalarım oturum açma uzantımız ve Azure AD Uygulama Ara Sunucusu her yerden uygulamalara erişmek için Iç URL 'Leri kullanın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO
 
Kullanıcılar, Azure AD için uygulamalarım güvenli oturum açma uzantısını kullanarak kurumsal ağınızın dışında bile uygulamalara iç URL 'Ler aracılığıyla erişebilir. Bu, aynı zamanda erişim paneli tarayıcı uzantısının yüklü olduğu herhangi bir tarayıcıda Azure AD Uygulama Ara Sunucusu kullanarak yayımladığınız herhangi bir uygulamayla birlikte çalışacaktır. Kullanıcı uzantıya oturum açtıktan sonra URL yeniden yönlendirme işlevi otomatik olarak etkinleştirilir. Uzantı [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)ve [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)'ta indirilebilir.

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory-Avrupa müşterileri için Avrupa 'daki veriler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** GoLocal

Avrupa 'daki müşteriler, verilerin Avrupa 'da kalmasını ve müşterilerin gizlilik ve Avrupa yasaları için Avrupa veri merkezleri dışında çoğaltılmaması gerekir. Bu [makalede](https://go.microsoft.com/fwlink/?linkid=872328) , Avrupa 'da hangi kimlik bilgilerinin depolanacağı ve ayrıca Avrupa veri merkezleri dışında depolanacak bilgiler hakkındaki ayrıntıları sağlayan belirli ayrıntılar sağlanmaktadır. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Yeni Kullanıcı hazırlama SaaS uygulaması tümleştirmeleri-Mayıs 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Azure AD, Dropbox, Salesforce, ServiceNow gibi SaaS uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatik hale getirmenizi sağlar. Mayıs 2018 için, Azure AD uygulama galerisinde aşağıdaki uygulamalar için Kullanıcı hazırlama desteği ekledik:

- [Şeması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Temel taş OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Azure AD galerisinde Kullanıcı sağlamayı destekleyen tüm uygulamaların listesi için bkz. [https://aka.ms/appstutorial](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Grupların ve uygulama erişiminin Azure AD erişim gözden geçirmeleri artık yinelenen incelemeler sağlıyor

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** #C1
 
Grupların ve uygulamaların erişim incelemesi artık Azure AD Premium P2 bir parçası olarak genel kullanıma sunulmuştur.  Yöneticiler, aylık veya üç aylık gibi düzenli aralıklarla otomatik olarak yinelenecek grup üyelikleri ve uygulama atamalarının erişim incelemelerini yapılandırabilecektir.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD etkinlik günlükleri (oturum açma ve denetim) artık MS Graph aracılığıyla kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme
 
Oturum açma işlemlerini ve denetim günlüklerini içeren Azure AD etkinlik günlükleri artık MS Graph üzerinden kullanılabilir. Bu günlüklere erişmek için MS Graph aracılığıyla iki uç noktası kullanıma sunduk. Başlamak için Azure AD Raporlama API 'Lerine programlı erişim için [belgelerimizi](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) inceleyin. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>B2B kullanım deneyimine yönelik geliştirmeler ve bir kuruluş bırakma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

**Tam zamanında kullanım:** B2B API 'sini kullanarak bir kaynağı Konuk kullanıcıyla paylaştığınızda, özel bir davet e-postası göndermeniz gerekmez. Çoğu durumda, Konuk Kullanıcı kaynağa erişebilir ve tam zamanında kullanım deneyiminden alınacaktır. Eksik e-postalar nedeniyle daha fazla etki yok. Konuk kullanıcılarınızı "Bu kullanım bağlantısına tıkladı", sistem sizi gönderdi. Bu, SPO davet yöneticisini kullandığında-bulutlu ekleri tüm kullanıcılar için aynı kurallı URL 'yi (iç ve dış), herhangi bir kullanım durumunda sahip olabileceği anlamına gelir.

**Modern kullanım deneyimi:** Daha fazla bölünmüş ekran ödeme giriş sayfası yok. Kullanıcılar, üçüncü taraf uygulamalar için olduğu gibi, kuruluşun gizlilik bildirimiyle ilgili modern bir onay deneyimi görür.

**Konuk kullanıcılar kuruluş 'dan bırakabilir:** Bir kullanıcının bir org ile ilişkisi bittikten sonra, kuruluştan ayrılmaları için kendi kendine servis sağlayabilirsiniz. Kuruluşun yöneticisinin "kaldırılması" için davet eden başka hiçbir çağrı yok, daha fazla destek bileti yok.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mayıs 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Mayıs 2018 ' de, uygulama galerimize federasyon desteği olan bu 18 yeni uygulamayı ekledik:

[Awardspring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), ınfogix Data3Sixty Yönet, [yodestesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [JAMF Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [knowledgeowl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi mmis](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [launchkaranlık](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe capkıt](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial) [,](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial)[まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [Arc Yayımlama-SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [plangrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [ıwellneskar](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [belgebir Snap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Azure Active Directory için yeni adım adım dağıtım kılavuzu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Dizinden
 
Self servis parola sıfırlama (SSPR), çoklu oturum açma (SSO), koşullu erişim (CA), uygulama proxy, Kullanıcı sağlama, Active Directory Federasyon Hizmetleri (AD FS) (ADFS) dahil olmak üzere Azure Active Directory (Azure AD) dağıtma hakkında yeni, adım adım yönergeler Geçişli kimlik doğrulaması (PTA) ve ADFS 'den Parola karması eşitleme (PHS).

Dağıtım kılavuzlarını görüntülemek için GitHub 'daki [kimlik dağıtım kılavuzlarını](https://aka.ms/DeploymentPlans) depoya gidin. Dağıtım kılavuzlarıyla ilgili geri bildirim sağlamak için [dağıtım planı geri bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzlarıyla ilgili sorularınız varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bizimle iletişime geçin.

---

### <a name="enterprise-applications-search---load-more-apps"></a>Kurumsal uygulamalar arama-daha fazla uygulama yükleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Uygulamalarınızın/hizmet sorumlularını bulmada sorun mu yaşıyorsunuz? Kurumsal uygulamalarınızın tüm uygulamalar listesine daha fazla uygulama yükleme özelliği ekledik. Varsayılan olarak 20 uygulama gösterilmektedir. Artık, ek uygulamaları görüntülemek için **daha fazla yükle** ' ye tıklayabilirsiniz. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>AADConnect 'in Mayıs yayını, PingFederate ile tümleştirmenin genel önizlemesini, önemli güvenlik güncelleştirmelerini, birçok hata düzeltmesi ve yeni harika yeni sorun giderme araçlarını içerir. 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** AD Connect  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
AADConnect 'in Mayıs yayını, PingFederate ile tümleştirmenin genel önizlemesini, önemli güvenlik güncelleştirmelerini, birçok hata düzeltmesi ve yeni harika yeni sorun giderme araçlarını içerir. Sürüm notlarını [buradan](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190)bulabilirsiniz.

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD erişim gözden geçirmeleri: otomatik uygulama

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** #C1

Grupların ve uygulamaların erişim gözden geçirmeleri artık Azure AD Premium P2 bir parçası olarak genel kullanıma sunulmuştur. Yönetici, gözden geçirenin değişikliklerini bu grup veya uygulamaya otomatik olarak uygulamak için erişim gözden geçirmesi tamamlanana kadar yapılandırabilir. Yönetici ayrıca, gözden geçirenler yanıt vermezse, erişimi kaldırmadıysa, erişimi devam ettirdiğinde veya sistem önerilerini almak için kullanıcının devam erişiminin ne olacağını da belirtebilir. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-response_mode-for-new-apps"></a>KIMLIK belirteçleri artık yeni uygulamalar için Query response_mode kullanılarak döndürülemez. 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
25 Nisan 2018 ' de veya sonrasında oluşturulan uygulamalar artık **Query** response_mode kullanarak bir **id_token** isteyemeyecektir.  Bu, Azure AD 'yi OıDC belirtimleriyle satır içi olarak getirir ve uygulamalarınızın saldırı yüzeyini azaltmaya yardımcı olur.  25 Nisan 2018 tarihinden önce oluşturulan uygulamalar, response_type of **id_token**ile **Query** response_mode 'in kullanılmasını engellenemez.  AAD 'den bir id_token istenirken döndürülen hata, **AADSTS70007: ' Query ', belirteç istenirken desteklenen bir ' response_mode ' değeri değildir**.

**Parça** ve **form_post** response_modes çalışmaya devam eder-yeni uygulama nesneleri oluştururken (örneğin, uygulama proxy 'si kullanımı için), yeni bir uygulama oluşturmadan önce bu response_modes birini kullandığınızdan emin olun.  

---
 
## <a name="april-2018"></a>Nisan 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C erişim belirteci GA

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C 

Artık erişim belirteçlerini kullanarak Azure AD B2C tarafından güvenliği sağlanmış Web API 'Lerine erişebilirsiniz. Bu özellik genel önizlemeden GA 'ye taşınıyor. Azure AD B2C uygulamaları ve Web API 'Lerini yapılandırmak için Kullanıcı arabirimi deneyimi geliştirilmiştir ve diğer küçük geliştirmeler yapılmıştır.
 
Daha fazla bilgi için bkz. [Azure AD B2C: erişim belirteçleri isteme](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>SAML tabanlı uygulamalar için çoklu oturum açma yapılandırmasını test etme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

SAML tabanlı SSO uygulamalarını yapılandırırken yapılandırma sayfasında tümleştirmeyi test edebilirsiniz. Oturum açma sırasında bir hatayla karşılaşırsanız, testi deneymenizde hata verebilir ve Azure AD, belirli sorunu çözmek için çözüm adımları sağlar.

Daha fazla bilgi için bkz.

- [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Azure Active Directory içindeki uygulamalarda SAML tabanlı çoklu oturum açma hatalarını ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD kullanım koşulları artık Kullanıcı başına raporlamaya sahiptir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 
Yöneticiler artık belirli bir ToU seçebilir ve bu ToU 'ya onaylanan tüm kullanıcıları ve ne zaman/saat sürdüğünü görebilir.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: AD FS Extranet kilitleme koruması için riskli IP 

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** & Raporlamayı izleme

Connect Health artık, saat başına veya günlük olarak başarısız U/P oturum açma eşiklerini aşan IP adreslerini algılama özelliğini desteklemektedir. Bu özellik tarafından sunulan yetenekler şunlardır:

- Özelleştirilebilir eşikle, IP adresini ve saatlik/günlük temelinde oluşturulan başarısız oturum açma sayısını gösteren kapsamlı rapor.
- Belirli bir IP adresinin saat/gün başına, başarısız U/P oturum açma eşiğini ne zaman aşmadığını gösteren e-posta tabanlı uyarılar.
- Verilerin ayrıntılı analizini yapmak için bir indirme seçeneği

Daha fazla bilgi için bkz. [RISKLI IP raporu](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Meta veri dosyası veya URL 'SI olan kolay uygulama yapılandırması

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Kurumsal uygulamalar sayfasında, Yöneticiler AAD Galerisi ve Galeri dışı uygulama için SAML tabanlı oturum açmayı yapılandırmak üzere bir SAML meta veri dosyasını karşıya yükleyebilir.

Ayrıca, hedef uygulamayla SSO 'yu yapılandırmak için Azure AD uygulama Federasyon meta verileri URL 'sini de kullanabilirsiniz.

Daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde olmayan uygulamalarda çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)konusuna bakın.

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD Kullanım koşulları genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 

Azure AD kullanım koşulları, genel önizlemeden genel kullanıma açık olarak taşınmıştır.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Belirli kuruluşların B2B kullanıcılarına yönelik davetlere izin verme veya bunları engelleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 

Artık Azure AD B2B Işbirliğinde paylaşmak ve işbirliği yapmak istediğiniz iş ortağı kuruluşlarını belirtebilirsiniz. Bunu yapmak için, belirli izin verme veya reddetme etki alanlarının listesini oluşturmayı seçebilirsiniz. Bu özellikleri kullanarak bir etki alanı engellendiğinde, çalışanlar artık bu etki alanındaki kişilere davetiye gönderemezler.

Bu, onaylanan kullanıcılar için sorunsuz bir deneyim etkinleştirirken kaynaklarınıza erişimi denetlemenize yardımcı olur.

Bu B2B Işbirliği özelliği tüm Azure Active Directory müşterileri tarafından kullanılabilir ve dış iş kullanıcılarının ne zaman ve nasıl imzalandığını daha ayrıntılı bir şekilde denetlemek için koşullu erişim ve kimlik koruması gibi Azure AD Premium özelliklerle birlikte kullanılabilir. ve ' de erişim elde edin.

Daha fazla bilgi için, bkz. [belirli kuruluşlardan B2B kullanıcılarına Izin verme veya bu kullanıcıların davetlerini engelleme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

2018 Nisan 'da, uygulama galerimize federasyon desteği olan bu 13 yeni uygulamayı ekledik:

Ölçüt HCM, [Fcalnote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (on-premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dinamik sinyal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindwireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [kuruluş şeması şimdi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [appneta performans izleyicisi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [floxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco bulutu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), rafı, [SafetyNET](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Azure AD 'de B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

Konuk kullanıcıları iş ortağı kuruluşlarınızdan Azure AD 'ye davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş olarak, bu B2B kullanıcılarına şirket içi uygulamalara erişimi sağlayabilirsiniz. Bu şirket içi uygulamalar, SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (ıWA) kullanarak Kerberos kısıtlanmış temsili (KCD) kullanabilir.

Daha fazla bilgi için bkz. Şirket [içi uygulamalarınıza Azure AD erişimi IÇIN B2B kullanıcıları verme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Azure Marketi 'nden SSO tümleştirme öğreticileri alın

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

[Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) 'nde listelenen BIR uygulama SAML tabanlı çoklu oturum açmayı destekliyorsa, **Şimdi al** ' a tıklamak bu uygulamayla ilişkili tümleştirme öğreticisini sağlar. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>SaaS uygulamalarına Azure AD otomatik Kullanıcı sağlamasının daha hızlı performansı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Daha önce, SaaS uygulamaları için Azure Active Directory Kullanıcı sağlama bağlayıcılarını kullanan müşteriler (örneğin Salesforce, ServiceNow ve Box), Azure AD kiracılarının 100.000 Birleşik Kullanıcı üzerinde yer aldığı ve grupları ve hangi kullanıcıların sağlanması gerektiğini öğrenmek için Kullanıcı ve grup atamalarını kullanmıştı.

2 Nisan 2018 ' de, Azure AD sağlama hizmeti 'ne Azure Active Directory ve hedef SaaS uygulamaları arasında ilk eşitlemeleri gerçekleştirmek için gereken süreyi büyük ölçüde azaltan önemli performans geliştirmeleri dağıtıldı.

Sonuç olarak, çok sayıda gün veya hiç tamamlanmamış uygulamalar için ilk eşitlemeleri olan birçok müşteri artık birkaç dakika veya saat içinde tamamlanır.

Daha fazla bilgi için bkz. [sağlama sırasında ne olur?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Karma Azure AD 'ye katılmış makineler için Windows 10 kilit ekranından self servis parola sıfırlama

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Windows 10 SSPR özelliğini, karma Azure AD 'ye katılmış makineler için destek içerecek şekilde güncelleştirdik. Bu özellik Windows 10 ' da kullanılabilir RS4, kullanıcıların parolalarını bir Windows 10 makinesinin kilit ekranından sıfırlamasına izin verir. Self servis parola sıfırlama için etkin ve kayıtlı kullanıcılar bu özelliği kullanabilir.

Daha fazla bilgi için bkz. [oturum açma ekranından Azure AD parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Mart 2018
 
### <a name="certificate-expire-notification"></a>Sertifika süre sonu bildirimi

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Bir galeri veya Galeri dışı bir uygulama için bir sertifika sona ermek üzereyken Azure AD bir bildirim gönderir. 

Bazı kullanıcılar SAML tabanlı çoklu oturum açma için yapılandırılmış kurumsal uygulamalar için bildirim almadı. Bu sorun çözüldü. Azure AD, 7, 30 ve 60 gün içinde süresi dolan sertifikaların bildirimini gönderir. Bu olayı denetim günlüklerinde görebilirsiniz. 

Daha fazla bilgi için bkz.

- [Azure Active Directory 'da federe çoklu oturum açma için sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory portalındaki denetim etkinliği raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C Twitter ve GitHub kimlik sağlayıcıları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Artık, Azure AD B2C bir kimlik sağlayıcısı olarak Twitter veya GitHub ekleyebilirsiniz. Twitter, genel önizlemeden GA 'ye taşınıyor. GitHub, genel önizlemede kullanıma sunuldu.

Daha fazla bilgi için bkz. [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>İOS ve Android için Azure AD uygulama tabanlı koşullu erişim ile Intune Managed Browser kullanarak tarayıcı erişimini kısıtlama

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
**Şimdi genel önizlemede!**

**INTUNE Managed Browser SSO:** Çalışanlarınız yerel istemcilerde (Microsoft Outlook gibi) çoklu oturum açma ve tüm Azure AD ile bağlantılı uygulamalar için Intune Managed Browser kullanabilir.

**Intune Managed Browser koşullu erişim desteği:** Artık çalışanların, uygulama tabanlı koşullu erişim ilkeleri kullanarak Intune Managed Browser 'ı kullanmasını zorunlu kılabilirsiniz.

[Blog gönderimizde](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/)bunun hakkında daha fazla bilgi edinin.

Daha fazla bilgi için bkz.

- [Uygulama tabanlı koşullu erişim kurulumu](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Yönetilen tarayıcı ilkelerini yapılandırma](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>PowerShell GA modülünde uygulama proxy cmdlet 'Leri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control
 
Uygulama proxy cmdlet 'leri için destek artık PowerShell GA modülünde! Bu, PowerShell modüllerinde güncelleşmenizi gerektirir; arkasında bir yıldan daha fazla olduğunuzda bazı cmdlet 'ler çalışmayı durdurabilir. 

Daha fazla bilgi için bkz. [Azuread](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 yerel istemcileri etkileşimli olmayan bir protokol kullanılarak sorunsuz SSO tarafından desteklenir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Office 365 Native istemcileri kullanan kullanıcı (sürüm 16.0.8730. xxxx ve üzeri) sorunsuz SSO kullanarak sessiz bir oturum açma deneyimi alın. Bu destek, Azure AD 'ye etkileşimli olmayan bir protokol (WS-Trust) eklenerek sağlanır.

Daha fazla bilgi için bkz. [sorunsuz SSO ile yerel bir istemcide oturum açma nasıl çalışır?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Uygulamalar, Azure AD 'nin kiracı uç noktalarına oturum açma istekleri gönderirse sorunsuz SSO ile sessiz oturum açma deneyimi alırlar.

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kullanıcılar, sorunsuz SSO ile sessiz bir oturum açma deneyimi alırlar. bir uygulama (örneğin, `https://contoso.sharepoint.com`) Azure AD 'nin kiracı uç noktalarına oturum açma istekleri gönderiyor; bu, Azure AD 'nin ortak uç noktası (`https://login.microsoftonline.com/common/<...>`) yerine `https://login.microsoftonline.com/contoso.com/<..>` veya `https://login.microsoftonline.com/<tenant_ID>/<..>`.

Daha fazla bilgi için bkz. [kesintisiz çoklu oturum açma Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Sorunsuz SSO 'yu kullanıma almak için kullanıcıların Intranet bölgesi ayarlarına daha önce iki URL yerine yalnızca bir Azure AD URL 'SI eklemeniz gerekir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kullanıcılarınıza sorunsuz SSO eklemek için, Active Directory: `https://autologon.microsoftazuread-sso.com`Grup İlkesi kullanarak kullanıcıların Intranet bölgesi ayarlarına yalnızca bir Azure AD URL 'SI eklemeniz gerekir. Daha önce müşterilerin iki URL eklemesi gerekiyordu.

Daha fazla bilgi için bkz. [kesintisiz çoklu oturum açma Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Bu 15 Mart 2018 ' de, uygulama galerimize federasyon desteği olan bu 15 yeni uygulamayı ekledik:

[BoxCryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [cylanceprotect](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [Signalfx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Asstagenda, [yardione](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), vtiger CRM, ınkırpması, [genlik](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [contractçalışma,](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial) [bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial) [, Trısotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu bulutu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Azure kaynakları için PıM genel kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Dizin rolleri için Azure AD Privileged Identity Management kullanıyorsanız, artık, abonelik, kaynak grupları, sanal makineler ve desteklenen diğer kaynaklar gibi Azure Kaynak rolleri için PıM 'nin saat bağlantılı erişim ve atama özelliklerini kullanabilirsiniz Azure Resource Manager. Tek seferde rolleri etkinleştirirken Multi-Factor Authentication uygulayın ve etkinleştirmeleri, onaylanan değişiklik pencereleri ile koordinasyon olarak zamanlayın. Buna ek olarak, bu sürüm, güncelleştirilmiş bir kullanıcı arabirimi, onay iş akışları ve rolleri yakında süresi dolacak şekilde genişletme ve süresi dolan rolleri yenileme özelliği dahil olmak üzere genel önizleme sırasında kullanılamayan geliştirmeler ekler.

Daha fazla bilgi için bkz. [Azure kaynakları Için PIM (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Uygulama belirteçlerinize Isteğe bağlı talepler ekleme (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD uygulamanız artık JWTs veya SAML belirteçlerinde özel veya isteğe bağlı talepler talep edebilir.  Bunlar, boyut veya uygulanabilirlik kısıtlamaları nedeniyle, belirteçte varsayılan olarak bulunmayan Kullanıcı veya kiracı hakkında taleplerdir.  Bu, şu anda v 1.0 ve v 2.0 uç noktalarında Azure AD uygulamaları için genel önizlemededir.  Hangi taleplerin eklenebileceklerini ve bunları istemek için uygulama bildiriminizi nasıl düzenleyeceğini öğrenmek için belgelere bakın.  

Daha fazla bilgi için bkz. [Azure AD 'de Isteğe bağlı talepler](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD, daha güvenli OAuth akışları için PKI CE 'yi destekler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD docs, OAuth 2,0 yetkilendirme kodu verme akışı sırasında daha güvenli iletişim sağlayan PCE için destek olarak güncelleştirilmiştir.  S256 ve düz metin code_challenges, v 1.0 ve v 2.0 uç noktalarında desteklenir. 

Daha fazla bilgi için bkz. [yetkilendirme kodu isteme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-get_workers-api"></a>Workday Get_Workers API 'sinde kullanılabilen tüm kullanıcı özniteliği değerlerini sağlama desteği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Workday Get_Workers API 'sinde bulunan tüm öznitelik değerlerini ayıklama ve sağlama özelliğini artık iş Workday 'den Active Directory ve Azure AD 'ye yönelik genel önizleme destekler. Bu, iş günü gelen sağlama bağlayıcısının ilk sürümü ile sevk edilen yüzlerce ek standart ve özel öznitelik için destek sağlar.

Daha fazla bilgi için bkz. [Workday kullanıcı özniteliklerinin listesini özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Grup üyeliğini dinamikten statik olarak değiştirme ve tam tersi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş
 
Bir grupta üyeliğin yönetilme şeklini değiştirmek mümkündür. Bu, sistemde aynı grup adını ve KIMLIĞINI tutmak istediğinizde yararlıdır, bu nedenle gruba yönelik varolan başvurular hala geçerlidir; Yeni bir grup oluşturmak bu başvuruların güncelleştirilmesini gerektirir.
Azure AD Yönetim merkezini bu işlevselliği destekleyecek şekilde güncelleştirdik. Artık müşteriler, mevcut grupları dinamik üyeliğinden atanan üyeliğe veya tam tersi yönde dönüştürebilir. Mevcut PowerShell cmdlet 'leri yine de kullanılabilir.

Daha fazla bilgi için bkz. [Azure Active Directory gruplar Için dinamik üyelik kuralları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Sorunsuz SSO ile iyileştirilmiş oturum kapatma davranışı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Daha önce, kullanıcılar Azure AD tarafından güvenli hale getirilmiş bir uygulamanın oturumunu kapatsa bile, bir Azure AD uygulamasına, etki alanına katılmış cihazlarından Corpnet dahilinde yeniden erişmeye çalıştıkları durumlarda sorunsuz SSO kullanılarak otomatik olarak oturum açabilirler. Bu değişiklik ile oturumu kapatma desteklenir.  Böylece kullanıcılar, sorunsuz SSO kullanılarak otomatik olarak oturum açmak yerine, kullanıcıların ile yeniden oturum açmasını sağlamak için aynı veya farklı Azure AD hesabını seçmesine olanak sağlar.

Daha fazla bilgi için bkz. [Azure Active Directory kesintisiz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Uygulama proxy Bağlayıcısı Sürüm 1.5.402.0 yayınlandı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Bu bağlayıcı sürümü, Kasım ile kademeli olarak dışarı aktarılır. Bu yeni bağlayıcı sürümü aşağıdaki değişiklikleri içerir:

- Bağlayıcı artık alt etki alanı düzeyi yerine etki alanı düzeyi tanımlama bilgilerini ayarlıyor. Bu, daha yumuşak bir SSO deneyimi sağlar ve gereksiz kimlik doğrulama istemlerini önler.
- Yığın halinde kodlama istekleri desteği
- Geliştirilmiş bağlayıcı sistem durumu izleme 
- Çeşitli hata düzeltmeleri ve kararlılık geliştirmeleri

Daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu bağlayıcıları anlama](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Şubat 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Kullanıcıları ve grupları yönetmek için geliştirilmiş gezinti

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Dizin Yönetimi  
**Ürün yeteneği:** Dizinden

Kullanıcıları ve grupları yönetmeye yönelik gezinti deneyimi kolaylaştırılmıştır. Artık, silinen kullanıcılar listesine daha kolay erişim sağlamak için doğrudan dizine genel bakış ' ı tüm kullanıcılar listesine gidebilirsiniz. Ayrıca, Grup yönetimi ayarlarına daha kolay erişim sayesinde dizine genel bakış ' a doğrudan tüm grupların listesine gidebilirsiniz. Ayrıca, dizine genel bakış sayfasından bir Kullanıcı, Grup, kurumsal uygulama veya uygulama kaydı için arama yapabilirsiniz. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>21Vianet tarafından çalıştırılan Microsoft Azure oturum açma ve denetim raporlarının kullanılabilirliği (Azure Çin 21Vianet)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure Stack  
**Ürün yeteneği:** & Raporlamayı izleme

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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Yönetici olmayan rollerin Azure AD etkinlik günlüklerine erişimine sahip olmasını sağlamak için müşterilerin geri bildirimlerinden bir parçası olarak, "rapor okuyucu" rolünde olan kullanıcıların, Azure portal içinde oturum açma işlemleri ve denetim etkinliğine ve Graf API 'lerimizi kullanarak erişmesine olanak sağlıyoruz. 

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Kullanıcı özniteliği ve Kullanıcı tanımlayıcısı olarak kullanılabilir EmployeeID talebi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
**ÇalışanNo** 'ı, kurumsal uygulama kullanıcı arabiriminden SAML tabanlı oturum açma uygulamalarında üye KULLANıCıLAR ve B2B konukları için Kullanıcı tanımlayıcısı ve kullanıcı özniteliği olarak yapılandırabilirsiniz.

Daha fazla bilgi için bkz. [Azure Active Directory 'de kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu joker karakter kullanarak Basitleştirilmiş uygulama yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Uygulama dağıtımını daha kolay hale getirmek ve yönetim yükünüzü azaltmak için artık joker karakterler kullanarak uygulama yayımlama özelliğini destekliyoruz. Bir joker uygulama yayımlamak için standart uygulama yayımlama akışını takip edebilir, ancak iç ve dış URL 'lerde bir joker karakter kullanabilirsiniz.

Daha fazla bilgi için [Azure Active Directory uygulama proxy 'Sinde joker uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard) bölümüne bakın

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Uygulama proxy 'sinin yapılandırmasını desteklemeye yönelik yeni cmdlet 'ler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Platformunun

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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Platformunun

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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** AD Eşitleme  
**Ürün yeteneği:** Platformunun
 
Azure AD Connect, Windows Server Active Directory ve LDAP dahil olmak üzere Azure AD ile şirket içi veri kaynakları arasında veri eşitlemesi için tercih edilen bir araçtır.

>[!Important]
>Bu derleme şema ve eşitleme kuralı değişikliklerini tanıtır. Azure AD Connect eşitleme hizmeti bir yükseltmeden sonra tam Içeri aktarma ve tam eşitleme adımlarını tetikler. Bu davranışı değiştirme hakkında daha fazla bilgi için, [yükseltmeden sonra tam eşitlemenin ertelenmesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade)bölümüne bakın.

Bu sürümde aşağıdaki güncelleştirmeler ve değişiklikler bulunur:

**Sorun düzeltildi**

- Sonraki sayfaya geçiş yaparken bölüm filtreleme sayfasına yönelik arka plan görevlerinde zamanlama penceresini düzeltir.

- ConfigDB özel eylemi sırasında erişim ihlaline neden olan bir hata düzeltildi.

- SQL bağlantı zaman aşımından kurtarmak için bir hata düzeltildi.

- SAN joker karakterlerinden oluşan sertifikaların ön REQ denetimi başarısız olduğu bir hata düzeltildi.

- AAD Bağlayıcısı dışarı aktarma sırasında mııver. exe kilitlenmesine neden olan bir hata düzeltildi.

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

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Uygulama tabanlı koşullu erişimi destekleyen daha fazla uygulama ekledik. Artık, bu onaylanan istemci uygulamalarını kullanarak Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişebilirsiniz.

Aşağıdaki uygulamalar Şubat sonuna kadar eklenecektir:

- Microsoft Power BI

- Microsoft Launcher

- Microsoft Invoicing

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Mobil deneyimle Kullanım koşulları güncelleştirme 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk

Kullanım koşulları görüntülendiğinde artık sorun yaşamadan ' a tıklayabilirsiniz.  **Buraya tıklayın**. Bu bağlantıya tıkladığınızda cihazınızdaki kullanım koşulları yerel olarak açılır. Belgedeki yazı tipi boyutundan veya cihazın ekran boyutundan bağımsız olarak, belgeyi gerektiği gibi yakınlaştırıp okuyabilirsiniz. 

---
 
## <a name="january-2018"></a>Ocak 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir 

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Ocak 2018 ' de, uygulama galerisine federasyon desteği olan aşağıdaki yeni uygulamalar eklenmiştir:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [Onetrust gizlilik yönetimi yazılımı](https://go.microsoft.com/fwlink/?linkid=861660), [Satıcıyolu](https://go.microsoft.com/fwlink/?linkid=863526), [ıriusrisfedermi federe dizini ve [uygunlukta netavantajlar](https://go.microsoft.com/fwlink/?linkid=864701).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Algılanan ek risk ile oturum açın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Algılanan bir risk algılaması için aldığınız Öngörüler, Azure AD aboneliğinize bağlıdır. Azure AD Premium P2 sürümüyle, temeldeki Tüm algılamalar hakkında en ayrıntılı bilgileri alırsınız.

Azure AD Premium P1 sürümü sayesinde, lisansınız kapsamında olmayan algılamalar, risk algılama tarafından algılanan ek risklerle birlikte oturum açma olarak görünür.

Daha fazla bilgi için bkz. [risk algılamalarını Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Office 365 uygulamalarını son kullanıcının erişim panellerinden gizle

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO

Artık, Office 365 uygulamalarının yeni bir Kullanıcı ayarı aracılığıyla kullanıcının erişim panellerinde nasıl gösterileceğini daha iyi yönetebilirsiniz. Bu seçenek, yalnızca Office portalında Office uygulamalarını göstermek istiyorsanız kullanıcının erişim panellerindeki uygulama sayısını azaltmak için yararlıdır. Bu ayar **Kullanıcı ayarlarında** bulunur ve etiketlidir, **kullanıcılar yalnızca Office 365 portalında Office 365 uygulamalarını görebilirler**.

Daha fazla bilgi için bkz. [Azure Active Directory kullanıcının deneyiminden bir uygulamayı gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Parola SSO 'SU doğrudan uygulamanın URL 'sinden etkin olan uygulamalarda sorunsuz oturum açma 

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO

Uygulamalarım tarayıcı uzantısı, artık uygulamalarımın tek oturum açma özelliğini tarayıcınızda bir kısayol olarak sunan kullanışlı bir araç aracılığıyla kullanıma sunulmuştur. ' İ yükledikten sonra, kullanıcıların tarayıcılarında uygulamalara hızlı erişim sağlayan bir waffle simgesi görürsünüz. Kullanıcılar artık şu özelliklerden yararlanabilir:

- Uygulamanın oturum açma sayfasından parola SSO tabanlı uygulamalarda doğrudan oturum açma özelliği
- Hızlı arama özelliğini kullanarak herhangi bir uygulamayı başlatma
- Uzantıdan son kullanılan uygulamalar için kısayollar
- Uzantı, Microsoft Edge, Chrome ve Firefox için kullanılabilir.
 
Daha fazla bilgi için bkz. [uygulamalarımın güvenli oturum açma uzantısı](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Klasik Azure Portalı Azure AD yönetim deneyimi kullanımdan kaldırılmıştır

**Şunu yazın:** Kullanım dışı   
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Dizinden

8 Ocak 2018 itibariyle, klasik Azure portalındaki Azure AD yönetim deneyimi kullanımdan kaldırılmıştır. Bu, klasik Azure portalının kullanımdan kaldırılması ile birlikte gerçekleşir. Gelecekte Azure AD 'nin tüm portal tabanlı yönetimi için [Azure AD Yönetim merkezini](https://aad.portal.azure.com) kullanmanız gerekir.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor Web portalı kullanımdan kaldırılmıştır

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Dizinden
 
8 Ocak 2018 itibariyle PhoneFactor Web portalı kullanımdan kaldırılmıştır. Bu portal MFA sunucusu 'nun yönetimi için kullanılmıştır, ancak bu işlevler portal.azure.com adresinde Azure portal taşınmıştır. 

MFA yapılandırması şurada bulunur: **Azure Active Directory \> MFA sunucusu**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlarını kullanımdan kaldırma

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi  


Yeni Azure Active Directory Yönetim konsolunun genel kullanıma sunulduğuna ve yeni API 'lerin hem etkinlik hem de güvenlik raporlarında kullanılabilir olması sayesinde, "/Reports" uç noktasındaki rapor API 'Leri 31 Aralık 2017 ' in sonunda kullanımdan kaldırılmıştır.

**Neler mevcuttur?**

Yeni Yönetici konsoluna geçişin bir parçası olarak, Azure AD etkinlik günlüklerini almak için 2 yeni API 'si yaptık. Yeni API 'Ler kümesi, daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak daha zengin filtreleme ve sıralama işlevselliği sağlar. Daha önce güvenlik raporları üzerinden kullanılabilen verilere artık Microsoft Graph kimlik koruması risk algılamaları API 'SI aracılığıyla erişilebilir.

Daha fazla bilgi için bkz.

- [Azure Active Directory Reporting API 'SI ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Azure Active Directory Kimlik Koruması ve Microsoft Graph kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Aralık 2017

### <a name="terms-of-use-in-the-access-panel"></a>Erişim panelinde Kullanım koşulları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 
Artık erişim paneline gidebilir ve önceden kabul ettiğiniz kullanım koşullarını görüntüleyebilirsiniz.

Şu adımları uygulayın:

1. [Uygulamaps portalına](https://myapps.microsoft.com)gidin ve oturum açın.

2. Sağ üst köşede adınızı seçin ve ardından listeden **profil** ' i seçin. 

3. **Profilinizde** **kullanım koşullarını gözden geçir**' i seçin. 

4. Artık kabul ettiğiniz kullanım koşullarını gözden geçirebilirsiniz. 

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği (Önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Yeni Azure AD oturum açma deneyimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD ve Microsoft hesabı Identity System U, tutarlı bir görünüme sahip olacak şekilde yeniden tasarlanmıştır. Ayrıca, Azure AD oturum açma sayfası önce Kullanıcı adını, ardından ikinci ekranda kimlik bilgisini toplar.

Daha fazla bilgi için bkz. [yeni Azure AD oturum açma deneyimi artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Daha az oturum açma istemi: Azure AD oturum açma için yeni bir "Oturumumu Açık tut" deneyimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD oturum açma sayfasındaki Oturumumu Açık **bırak** onay kutusu, başarıyla kimlik doğrulamasından geçtikten sonra görüntülenen yeni bir istem ile değiştirilmiştir. 

Bu istem için **Evet** 'e yanıt verirseniz, hizmet sizi kalıcı yenileme belirteci sağlar. Bu davranış, eski deneyimdeki Oturumumu **açık tut** onay kutusunu seçtiğiniz durumla aynıdır. Federal kiracılar için bu istem, Federasyon Hizmeti ile başarıyla kimlik doğrulamasından geçtikten sonra görüntülenir.

Daha fazla bilgi için bkz. daha [az oturum açma istemi: Azure AD için yeni "Oturumumu Açık tut" deneyimi önizlemededir](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Kullanım koşullarının kabul edilmeden önce genişletilmesi için yapılandırma ekleyin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 
Yöneticiler için bir seçenek, kullanıcıların koşulları kabul etmeden önce kullanım koşullarını genişletmelerini gerektirir.

Kullanıcıların kullanım koşullarını genişletmesini gerektirmek için **Açık** veya **kapalı** seçeneklerinden birini belirleyin. **Açık** ayarı, kullanıcıların kabul etmeden önce kullanım koşullarını görüntülemesini gerektirir.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği (Önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Varsayılan olarak orijinal atama varsayılanlarından daha az bağımsız çalışma sınırı sahip olan uygun Azure Kaynak rolü atamalarını etkinleştirmek için kapsamlı etkinleştirme kullanabilirsiniz. Kiracınızda aboneliğin sahibi olarak atandıysanız örnek bir örnektir. Kapsamlı etkinleştirme sayesinde, abonelik içinde (kaynak grupları ve sanal makineler gibi) bulunan en fazla beş kaynak için sahip rolünü etkinleştirebilirsiniz. Etkinleştirmenin kapsamını belirlemek, önemli Azure kaynaklarında istenmeyen değişiklikler yürütme olasılığını azaltabilir.

Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Bu yeni uygulamaları, Aralık 2017 ' de, uygulama galerimize federasyon desteğiyle ekledik:

[Accanble](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital storefront](https://go.microsoft.com/fwlink/?linkid=861685), [communifire](https://go.microsoft.com/fwlink/?linkid=861676) cybsafe, [factset](https://go.microsoft.com/fwlink/?linkid=863525), [Image çalışma,](https://go.microsoft.com/fwlink/?linkid=863517) [mobi](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD tümleştirmesi](https://go.microsoft.com/fwlink/?linkid=858027), [yansıma](https://go.microsoft.com/fwlink/?linkid=863518)dosyası, [SAML SSO 'su Bamboo by çözünürlüklü GmbH](https://go.microsoft.com/fwlink/?linkid=863520), çözünürlükte GmbH, [vodeoni](https://go.microsoft.com/fwlink/?linkid=863522), Webhr, Zenegy Azure AD tümleştirmesi [tarafından BITBUCKET için SAML SSO 'su](https://go.microsoft.com/fwlink/?linkid=863519).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD dizin rolleri için onay iş akışları

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Azure AD dizin rolleri için onay iş akışı genel kullanıma sunulmuştur.

Onay iş akışı ile ayrıcalıklı rol yöneticileri, ayrıcalıklı rolü kullanabilmeniz için, uygun rol üyelerinin rol etkinleştirme istemesi için gerekli olabilir. Birden çok kullanıcı ve gruba, onay sorumlulukları atanabilir. Uygun rol üyeleri onay tamamlandığında ve rolleri etkin olduğunda bildirim alır.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Geçişli kimlik doğrulaması: Skype Kurumsal desteği

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Doğrudan kimlik doğrulaması artık, çevrimiçi ve karma topolojilerini içeren modern kimlik doğrulamayı destekleyen Skype Kurumsal istemci uygulamalarına yönelik kullanıcı oturum açma işlemlerini destekler. 

Daha fazla bilgi için bkz. [modern kimlik doğrulamasıyla desteklenen Skype Kurumsal topolojileri](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Azure RBAC için Azure AD Privileged Identity Management güncelleştirmeleri (Önizleme)

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Azure rol tabanlı Access Control (RBAC) için Azure AD Privileged Identity Management (PıM) genel önizleme yenilemesinde, artık şunları yapabilirsiniz:

* Yalnızca yeterli yönetimi kullanın.
* Kaynak rollerini etkinleştirmek için onay gerektir.
* Hem Azure AD hem de Azure RBAC rolleri için onay gerektiren bir rolün daha sonra etkinleştirilmesini zamanlayın.
 
Daha fazla bilgi için bkz. [Azure kaynakları için Privileged Identity Management (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>2017 Kasım
 
### <a name="access-control-service-retirement"></a>Access Control hizmeti kullanımdan kaldırma

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Access Control hizmeti  
**Ürün yeteneği:** Access Control hizmeti 

Azure Active Directory Access Control (Access Control hizmeti olarak da bilinir), geç 2018 ' de kullanımdan kaldırılacaktır. Sonraki birkaç hafta içinde ayrıntılı bir zamanlamayı ve üst düzey geçiş kılavuzunu içeren daha fazla bilgi sağlanacaktır. Bu sayfadaki açıklamaları Access Control hizmetiyle ilgili herhangi bir soru ile bırakabilir ve bir takım üyesi onları yanıtlayabilir.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Intune Managed Browser tarayıcı erişimini kısıtlama 

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Onaylanan bir uygulama olarak Intune Managed Browser kullanarak Office 365 ve diğer Azure AD bağlı bulut uygulamalarına tarayıcı erişimini kısıtlayabilirsiniz. 

Artık uygulama tabanlı koşullu erişim için aşağıdaki koşulu yapılandırabilirsiniz:

**İstemci uygulamaları:** Tarayıcı

**Değişikliğin etkisi nedir?**

Bugün, bu durumu kullandığınızda erişim engellenir. Önizleme kullanılabilir olduğunda, tüm erişimlerle yönetilen tarayıcı uygulamasının kullanılması gerekir. 

Yaklaşan blogların ve sürüm notlarındaki bu özelliği ve daha fazla bilgiyi bulun. 

Daha fazla bilgi için bkz. [Azure AD 'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Aşağıdaki uygulamalar [onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)listesidir:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- Microsoft StaffHub

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Birden çok dil için kullanım koşulları desteği

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk

Yöneticiler, artık birden çok PDF belgesi içeren yeni kullanım koşulları oluşturabilir. Bu PDF belgelerini karşılık gelen bir dille etiketlendirebilirsiniz. Kullanıcılar, tercihleri temelinde, eşleşen dille PDF gösterilir. Eşleşme yoksa, varsayılan dil gösterilir.

---
 
### <a name="real-time-password-writeback-client-status"></a>Gerçek zamanlı parola geri yazma istemci durumu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Artık şirket içi parola geri yazma istemcinizdeki durumu gözden geçirebilirsiniz. Bu seçenek, [parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) sayfasının **Şirket içi tümleştirme** bölümünde bulunur. 

Şirket içi geri yazma istemcinizdeki bağlantınız ile ilgili sorunlar varsa şunları sağlayan bir hata iletisi görürsünüz:

- Şirket içi geri yazma istemcinizle ilgili olarak neden bağlanamadığına ilişkin bilgiler.
- Sorunu çözmenize yardımcı olacak belge bağlantısı. 

Daha fazla bilgi için bkz. [Şirket içi tümleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim 
 
**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Artık Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişimi, [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)kullanarak Intune uygulama koruma ilkelerini destekleyen [onaylanan istemci uygulamalarına](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) kısıtlayabilirsiniz. Intune uygulama koruma ilkeleri, bu istemci uygulamalarında şirket verilerini yapılandırmak ve korumak için kullanılır.

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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Cihaz kaydı ve yönetimi  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Artık Azure AD 'ye bağlı tüm cihazlarınızı ve cihazla ilgili etkinlikleri tek bir yerde bulabilirsiniz. Azure portal tüm cihaz kimliklerinizi ve ayarlarını yönetmek için yeni bir yönetim deneyimi vardır. Bu sürümde şunları yapabilirsiniz:

- Azure AD 'de koşullu erişim için kullanılabilen tüm cihazlarınızı görüntüleyin.
- Karma Azure AD 'ye katılmış cihazlarınızı içeren özellikleri görüntüleyin.
- Azure AD 'ye katılmış cihazlarınız için BitLocker anahtarları bulun, cihazınızı Intune ile yönetin ve daha fazlasını yapın.
- Azure AD cihazla ilgili ayarları yönetin.

Daha fazla bilgi için [Azure Portal kullanarak cihazları yönetme](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal)konusuna bakın.

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Azure AD koşullu erişim için bir cihaz platformu olarak macOS desteği 

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması 

Artık macOS 'ı Azure AD koşullu erişim ilkenize cihaz platformu olarak dahil edebilir (veya dışlayabilirsiniz). MacOS 'un desteklenen cihaz platformlarına eklenmesi sayesinde şunları yapabilirsiniz:

- **Intune kullanarak macOS cihazlarını kaydedin ve yönetin.** İOS ve Android gibi diğer platformlara benzer şekilde, macOS 'un birleştirilmiş kayıtları yapması için bir şirket portalı uygulaması vardır. MacOS için yeni şirket portalı uygulamasını kullanarak bir cihazı Intune 'a kaydedebilir ve Azure AD 'ye kaydedebilirsiniz.
- **MacOS cihazlarının, kuruluşunuzun Intune 'da tanımlanan uyumluluk ilkelerine bağlı olduğundan emin olun.** Azure portal Intune 'da, artık macOS cihazları için uyumluluk ilkeleri ayarlayabilirsiniz. 
- **Azure AD 'deki uygulamalara erişimi yalnızca uyumlu macOS cihazlarına kısıtlayın.** Koşullu erişim ilkesi yazma, macOS öğesini ayrı bir cihaz platformu seçeneği olarak içerir. Artık Azure 'da ayarlanan hedeflenen uygulama için macOS 'a özgü koşullu erişim ilkelerini yazabilirsiniz.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazları için cihaz uyumluluğu ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD 'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication için ağ Ilkesi sunucusu uzantısı 

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:**  Multi-Factor Authentication  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure Multi-Factor Authentication için ağ Ilkesi sunucu uzantısı, mevcut sunucularınızı kullanarak kimlik doğrulama altyapınıza bulut tabanlı Multi-Factor Authentication özellikleri ekler. Ağ Ilkesi sunucusu uzantısıyla, mevcut kimlik doğrulama akışınıza telefon araması, SMS mesajı veya telefon uygulaması doğrulaması ekleyebilirsiniz. Yeni sunucuları yüklemek, yapılandırmak ve bakımını yapmak zorunda değilsiniz. 

Bu uzantı, Azure Multi-Factor Authentication Sunucusu dağıtımı yapmadan sanal özel ağ bağlantılarını korumak isteyen kuruluşlar için oluşturulmuştur. Ağ Ilkesi sunucu uzantısı, federal veya eşitlenmiş kullanıcılar için ikinci bir kimlik doğrulama faktörü sağlamak üzere RADIUS ve bulut tabanlı Azure Multi-Factor Authentication arasında bir bağdaştırıcı görevi görür.

Daha fazla bilgi için bkz. [mevcut ağ Ilkesi sunucusu altyapınızı Azure Multi-Factor Authentication tümleştirme](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Silinen kullanıcıları geri yükleme veya kalıcı olarak kaldırma

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:** Kullanıcı Yönetimi  
**Ürün yeteneği:** Dizinden 

Azure AD Yönetim merkezinde artık şunları yapabilirsiniz:

- Silinen bir kullanıcıyı geri yükleyin. 
- Bir kullanıcıyı kalıcı olarak silme.

**Denemek için:**

1. Azure AD Yönetim merkezinde **Yönet** bölümündeki [tüm kullanıcılar](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) ' ı seçin. 

2. **Göster** listesinden, **son silinen kullanıcılar**' ı seçin. 

3. Son silinen bir veya daha fazla kullanıcı seçin ve ardından onları geri yükleyin veya kalıcı olarak silin.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları
 
**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

[Onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)listesine aşağıdaki uygulamalar eklenmiştir:

- Microsoft Planner
- Azure Information Protection 

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Koşullu erişim ilkesindeki denetimler arasında "OR" kullanın 

**Şunu yazın:** Değiştirilen özellik    
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması
 
Koşullu erişim denetimleri için artık "veya" (seçili denetimlerden birini gerektir) kullanabilirsiniz. Bu özelliği, erişim denetimleri arasında "veya" ile ilke oluşturmak için kullanabilirsiniz. Örneğin, bir kullanıcının uyumlu bir cihazda olması için Multi-Factor Authentication "veya" kullanarak oturum açmasını gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için bkz. [Azure AD koşullu erişim 'Deki denetimler](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-detections"></a>Gerçek zamanlı risk algılamalarını toplama

**Şunu yazın:** Değiştirilen özellik    
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Azure AD Kimlik Koruması, belirli bir gündeki aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk algılamaları artık her risk algılama türü için toplanır. Bu değişiklik, Kullanıcı güvenliğine hiçbir değişiklik yapılmadan gösterilen risk algılamaları hacminin sınırlarını sınırlar.

Temeldeki gerçek zamanlı algılama, kullanıcının her oturum açışında işe yarar. Multi-Factor Authentication veya erişimi engellemek üzere ayarlanmış bir oturum açma riski güvenlik ilkesi varsa, bu, her riskli oturum açma işlemi sırasında tetiklenir.
 
---
 
## <a name="october-2017"></a>2017 Ekim

### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlarını kullanımdan kaldırma

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi  

Azure portal şunları sağlar:

- Yeni bir Azure AD Yönetim Konsolu.
- Etkinlik ve güvenlik raporları için yeni API 'Ler.
 
Bu yeni yetenekler nedeniyle,/Reports uç noktasındaki rapor API 'Leri 10 Aralık 2017 ' de kullanımdan kalkmıştı. 

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alanı algılama

**Şunu yazın:** Düzenle   
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** Çoklu oturum açma  

Azure AD, bir HTML Kullanıcı adı ve parola alanı işleyen uygulamalar için otomatik oturum açma alanı algılamayı destekler. Bu adımlar, [bir uygulama için oturum açma alanlarını otomatik olarak yakalama](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications-problems#manually-capture-sign-in-fields-for-an-app)bölümünde belgelenmiştir. Bu özelliği, [Azure Portal](https://aad.portal.azure.com) **Enterprise Applications** sayfasında *Galeri dışı* bir uygulama ekleyerek bulabilirsiniz. Ayrıca, bu yeni uygulamadaki **Çoklu oturum** açma modunu **parola tabanlı çoklu oturum açma**için yapılandırabilir, bir Web URL 'si girebilir ve sonra sayfayı kaydedebilirsiniz.
 
Bir hizmet sorunu nedeniyle, bu işlev geçici olarak devre dışı bırakıldı. Sorun çözüldü ve otomatik oturum açma alanı algılaması yeniden kullanılabilir.

---

### <a name="new-multi-factor-authentication-features"></a>Yeni Multi-Factor Authentication özellikleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Multi-Factor Authentication  
**Ürün yeteneği:** Kimlik güvenliği ve koruması  

Multi-Factor Authentication (MFA), kuruluşunuzu korumanın önemli bir parçasıdır. Kimlik bilgilerini daha uyumlu hale getirmek ve deneyimi daha sorunsuz hale getirmek için aşağıdaki özellikler eklenmiştir: 

- Multi-Factor Challenge sonuçları doğrudan Azure AD oturum açma raporuyla tümleşiktir ve bu da MFA sonuçlarına programlı erişim içerir.
- MFA yapılandırması, Azure portal Azure AD yapılandırma deneyimiyle daha ayrıntılı bir şekilde tümleştirilir.

Bu genel önizleme sayesinde MFA yönetimi ve raporlama, çekirdek Azure AD yapılandırma deneyiminin tümleşik bir parçasıdır. Artık MFA yönetim portalı işlevini Azure AD deneyiminden yönetebilirsiniz.

Daha fazla bilgi için bkz. [Azure Portal MFA raporlaması Için başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Kullanım koşulları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk  

Kullanıcılara yasal veya uyumluluk gereksinimleri için ilgili bildirimler gibi bilgileri sunmak için Azure AD kullanım koşulları 'nı kullanabilirsiniz.

Azure AD kullanım koşulları 'nı aşağıdaki senaryolarda kullanabilirsiniz:

- Kuruluşunuzdaki tüm kullanıcılar için genel kullanım koşulları
- Kullanıcının özniteliklerine dayalı belirli kullanım koşulları (örneğin, doktorlarla veya yerel ve uluslararası çalışanlar, dinamik gruplar tarafından yapılır)
- Salesforce gibi yüksek etkili iş uygulamalarına erişmek için belirli kullanım koşulları

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="enhancements-to-privileged-identity-management"></a>Privileged Identity Management geliştirmeleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management  

Azure AD Privileged Identity Management, kuruluşunuzda Azure kaynaklarına (Önizleme) erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz:

- Abonelikler
- Kaynak grupları
- Sanal makineler 

Azure RBAC işlevselliğini kullanan Azure portal tüm kaynaklar, Azure AD Privileged Identity Management sunabileceği tüm güvenlik ve yaşam döngüsü yönetimi yetilerinden yararlanabilir.

Daha fazla bilgi için bkz. [Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Erişim incelemeleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim İncelemeleri  
**Ürün yeteneği:** Uyumluluk  

Kuruluşlar, grup üyeliklerini verimli bir şekilde yönetmek ve kurumsal uygulamalara erişmek için erişim gözden geçirmeleri (Önizleme) kullanabilir: 

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler, erişim gözden geçirmeleri tarafından belirtilen Öngörüler temelinde konuklara erişime izin verip vermeyeceğine karar verebilir.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için bkz. [Azure AD erişim İncelemeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Uygulamalarım ve Office 365 uygulama başlatıcısı 'ndan üçüncü taraf uygulamaları gizleyin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** Çoklu oturum açma  

Şimdi, yeni bir **uygulama Gizle** özelliği aracılığıyla kullanıcılarınızın portallarında görüntülenen uygulamaları daha iyi yönetebilirsiniz. Uygulama kutucuklarının arka uç hizmetleri veya yinelenen kutucuklar ve en yüksek kullanıcıların uygulama başlatanlar için göstereceği durumlarda yardımcı olması için uygulamaları gizleyebilirsiniz. Geçiş, üçüncü taraf uygulamanın **Özellikler** bölümüdür ve **Kullanıcı tarafından görülebilir** olarak etiketlenir. Ayrıca, bir uygulamayı PowerShell aracılığıyla programlı bir şekilde gizleyebilirsiniz. 

Daha fazla bilgi için bkz. [Azure AD 'de kullanıcının deneyiminden üçüncü taraf bir uygulamayı gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Neler mevcuttur?**

 Yeni Yönetici konsoluna geçişin bir parçası olarak, Azure AD etkinlik günlüklerini almak için iki yeni API kullanılabilir. Yeni API 'Ler kümesi, daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak daha zengin filtreleme ve sıralama işlevselliği sağlar. Daha önce güvenlik raporları üzerinden kullanılabilen verilere artık Microsoft Graph kimlik koruması risk algılamaları API 'SI aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-identity-manager"></a>Identity Manager düzeltmesi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik Yöneticisi  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi  

Kimlik Yöneticisi 2016 Service Pack 1 ' den itibaren, 25 Eylül 2017 itibariyle bir düzeltme toplaması paketi (Build 4.4.1642.0) kullanılabilir. Bu toplama paketi:

- Sorunları çözer ve iyileştirmeler ekler.
- , Identity Manager 2016 için 4.4.1459.0 derlemesi için tüm Identity Manager 2016 hizmet paketi 1 güncelleştirmelerini değiştiren bir toplu güncelleştirmedir. 
- Identity Manager 2016 derleme 4.4.1302.0 sahip olmanızı gerektirir. 

Daha fazla bilgi için bkz. [Identity Manager 2016 Service Pack 1 Için düzeltme paketi paketi (Build 4.4.1642.0) kullanılabilir](https://support.microsoft.com/help/4021562). 

---
