---
title: Azure Active Directory'de yenilikler için arşivleme nedir? | Microsoft Belgeleri
description: Bu içerik kümesinin Genel Bakış bölümündeki yeni sürüm notları 6 aylık etkinlik içerir. 6 ay sonra, öğeler ana makaleden kaldırılır ve bu arşiv makalesine konur.
services: active-directory
author: msmimart
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25191951472e30492606ad97c440a13359c8ef24
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253178"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Azure Active Directory'de yenilikler için arşivleme nedir?

Birincil [Azure Etkin Dizin'inde yenilikler nelerdir? sürüm notları](whats-new.md) makalesi son altı ayın güncelleştirmelerini içerirken, bu makale tüm eski bilgileri içerir.

Azure Etkin Dizini'ndeki yenilikler nelerdir? sürüm notları hakkında bilgi sağlar:

- En son sürümler
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlevsellik
- Değişiklik planları

---

## <a name="september-2019"></a>Eylül 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Değişiklik planı: Power BI içerik paketlerinin amortismanı

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Power BI, 1 Ekim 2019'dan itibaren Azure AD Power BI içerik paketi de dahil olmak üzere tüm içerik paketlerini amortismana girmeye başlayacaktır. Bu içerik paketine alternatif olarak, Azure AD ile ilgili hizmetlerinizle ilgili öngörüler elde etmek için Azure AD Çalışma Kitaplarını kullanabilirsiniz. Yalnızca rapor modundaki Koşullu Erişim ilkeleriyle ilgili çalışma kitapları, uygulama onayına dayalı öngörüler ve daha fazlası dahil olmak üzere ek çalışma kitapları geliyor.

Çalışma kitapları hakkında daha fazla bilgi için Azure [Etkin Dizin raporları için Azure Monitor çalışma kitaplarını nasıl kullanacağınız](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks)abakın. İçerik paketlerinin amortismanı hakkında daha fazla bilgi için, [Duyuru Güç BI şablonu uygulamaları genel kullanılabilirlik](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/) blog gönderisi bölümüne bakın.

---

### <a name="my-profile-is-renaming-and-integrating-with-the-microsoft-office-account-page"></a>Profilim Microsoft Office hesap sayfasını yeniden adlandırma ve tümleştirme

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Profilim/Hesabım  
**Ürün kapasitesi:** Işbirliği

Ekim ayından itibaren Profilim deneyimi Hesabım olacak. Bu değişikliğin bir parçası olarak, şu anda yazan her yerde **Profilim** **Hesabım**olarak değişecektir. Adlandırma değişikliği ve bazı tasarım geliştirmelerinin yanı sıra, güncelleştirilmiş deneyim Microsoft Office hesap sayfasıyla ek tümleştirme sunar. Özellikle, Office yüklemelerine ve aboneliklerine Genel Bakış **Hesabı** sayfasından ve **Gizlilik** sayfasından Office ile ilgili kişi tercihlerine erişebilirsiniz.

Profilim (önizleme) deneyimi hakkında daha fazla bilgi için [Profilim (önizleme) portalına genel bakış](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview)bilgisine bakın.

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>Azure AD portalındaki CSV dosyalarını kullanarak grupları ve üyeleri toplu olarak yönetin (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Azure AD portalında toplu grup yönetimi deneyimlerinin genel önizleme kullanılabilirliğini duyurmaktan mutluluk duyarız. Artık aşağıdakiler de dahil olmak üzere grupları ve üye listelerini yönetmek için bir CSV dosyasını ve Azure AD portalını kullanabilirsiniz:

- Bir gruptan üye ekleme veya kaldırma.

- Grup listesini dizinden indirme.

- Belirli bir grup için grup üyelerinin listesini indirme.

Daha fazla bilgi için, [toplu üye eklemek](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), Toplu üye [kaldırmak](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members), Toplu indirme [üyeleri listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members)ve Toplu indirme [grupları listesi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download)bakın .

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>Dinamik onay artık yeni bir yönetici onayı bitiş noktası ile desteklenir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Dinamik onayı desteklemek için yeni bir yönetici onayı bitiş noktası oluşturduk ve bu da Microsoft Identity platformundaki dinamik onay modelini kullanmak isteyen uygulamalar için yararlıdır.

Bu yeni bitiş [noktasının](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent)nasıl kullanılacağı hakkında daha fazla bilgi için bkz.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2019"></a>Azure AD App galerisinde yeni Federe Uygulamalar mevcuttur - Eylül 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Eylül 2019'da, Federasyon desteğine sahip 29 yeni uygulamayı uygulama galerisine ekledik:

[ScheduleLook](https://schedulelook.bbsonlineservices.net/), [Ethidex&trade; Uyumluluk Ofisi için MS Azure SSO Erişimi - Tek oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/ms-azure-sso-access-for-ethidex-compliance-office-tutorial), [iServer Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/iserver-portal-tutorial), [SKYSITE](https://docs.microsoft.com/azure/active-directory/saas-apps/skysite-tutorial), [Concur Travel and Expense](https://docs.microsoft.com/azure/active-directory/saas-apps/concur-travel-and-expense-tutorial), [WorkBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/workboard-tutorial), `https://apps.yeeflow.com/`ARC [Tesisleri](https://docs.microsoft.com/azure/active-directory/saas-apps/arc-facilities-tutorial), [Luware Stratus Team](https://stratus.emea.luware.cloud/login), Geniş [Fikirler](https://wideideas.online/wideideas/), [Prisma Bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [JDLT Client Hub](https://clients.jdlt.co.uk/login), [RENRAK U ,](https://docs.microsoft.com/azure/active-directory/saas-apps/renraku-tutorial) [SealPath Güvenli Tarayıcı](https://protection.sealpath.com/SealPathInterceptorWopiSaas/Open/InstallSealPathEditorOneDrive), [Prisma Bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), `https://app.penneo.com/`, `https://app.testhtm.com/settings/email-integration` [Cintoo Bulut](https://aec.cintoo.com/login), [Whitesource](https://docs.microsoft.com/azure/active-directory/saas-apps/whitesource-tutorial), [Barındırılan Miras Online SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-heritage-online-sso-tutorial), [IDC](https://docs.microsoft.com/azure/active-directory/saas-apps/idc-tutorial), [CakeHR](https://docs.microsoft.com/azure/active-directory/saas-apps/cakehr-tutorial), [BIS](https://docs.microsoft.com/azure/active-directory/saas-apps/bis-tutorial), Coo Kai Takım [Build](https://ms-contacts.coo-kai.jp/), [Sonarqube](https://docs.microsoft.com/azure/active-directory/saas-apps/sonarqube-tutorial), Adobe Identity [Management](https://docs.microsoft.com/azure/active-directory/saas-apps/adobe-identity-management-tutorial), Discovery Faydaları [SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/discovery-benefits-sso-tutorial), [Amelio](https://app.amelio.co/),`https://itask.yipinapp.com/`

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="new-azure-ad-global-reader-role"></a>Yeni Azure AD Global Reader rolü

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Rbac  
**Ürün kapasitesi:** Erişim Kontrolü

24 Eylül 2019'dan itibaren Global Reader adında yeni bir Azure Active Directory (AD) rolü kullanıma başlayacağız. Bu ürün üretimi ve Global bulut müşterileri (GCC) ile başlayacak ve dünya çapında Ekim ayında sona erecek.

Global Reader rolü, Global Administrator'un salt okunur karşılığıdır. Bu roldeki kullanıcılar Microsoft 365 hizmetlerinde ayarları ve yönetim bilgilerini okuyabilir, ancak yönetim eylemlerinde görev alamaz. Kuruluşunuzdaki Genel Yöneticilerin sayısını azaltmaya yardımcı olmak için Global Reader rolünü oluşturduk. Global Administrator hesapları güçlü ve saldırıya açık olduğundan, beşten az Genel Yöneticiniz olmasını öneririz. Planlama, denetim ler veya soruşturmalar için Global Reader rolünü kullanmanızı öneririz. Ayrıca, Global Administrator rolüne gerek kalmadan işin yapılmasına yardımcı olmak için Exchange Administrator gibi diğer sınırlı yönetici rolleriyle birlikte Global Reader rolünü kullanmanızı öneririz.

Global Reader rolü yeni Microsoft 365 Yönetici Merkezi, Exchange Yönetici Merkezi, Ekipler Yönetici Merkezi, Güvenlik Merkezi, Uyumluluk Merkezi, Azure AD Yönetici Merkezi ve Aygıt Yönetimi Yönetici Merkezi ile çalışır.

>[!NOTE]
> Genel önizlemenin başlangıcında, Global Reader rolü ile çalışmaz: SharePoint, Ayrıcalıklı Erişim Yönetimi, Müşteri Kilit Kutusu, duyarlılık etiketleri, Takımlar Yaşam Döngüsü, Arama Analizi & Raporlama ekipleri, Ekipler IP Telefon Cihaz Yönetimi ve Takımlar Uygulama Kataloğu. 

Daha fazla bilgi için [Azure Etkin Dizin'inde Yönetici rol izinlerine](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)bakın.

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy'yi kullanarak Power BI Mobile uygulamanızdan şirket içi Rapor Sunucusuna erişin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Power BI mobil uygulaması ile Azure AD Application Proxy arasındaki yeni tümleştirme, Power BI mobil uygulamasında güvenli bir şekilde oturum açmanızı ve kuruluşunuzun şirket içi Power BI Report Server'da barındırılan raporlarından herhangi birini görüntülemenize olanak tanır.

Uygulamayı nerede indirebileceğiniz i dahil olmak üzere Power BI Mobile uygulaması hakkında daha fazla bilgi için [Power BI sitesine](https://powerbi.microsoft.com/mobile/)bakın. Azure AD Application Proxy ile Power BI mobil uygulamasının nasıl ayarlanması hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi)

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>AzureADPreview PowerShell modülünün yeni sürümü kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Dizin

Azure ADPreview modülüne, Azure AD'de aşağıdakiler dahil olmak üzere özel rollerin tanımlanmasına ve atayabına yardımcı olmak için yeni cmdletler eklendi:

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>Azure AD Connect'in yeni sürümü

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Dizin

Otomatik yükseltme müşterileri için Azure AD Connect'in güncelleştirilmiş bir sürümünü yayımladık. Bu yeni sürüm birkaç yeni özellik, geliştirme ve hata düzeltmeleri içerir. Bu yeni sürüm hakkında daha fazla bilgi için Azure [AD Connect: Sürüm sürüm geçmişine](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250)bakın.

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Azure Çok Faktörlü Kimlik Doğrulama (MFA) Sunucusu, sürüm 8.0.2 artık kullanıma sunuldu

**Türü:** Sabit  
**Hizmet kategorisi:** Mfa  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

MFA Server'ı 1 Temmuz 2019'dan önce etkinleştiren mevcut bir müşteriyseniz, artık MFA Server'ın en son sürümünü indirebilirsiniz (sürüm 8.0.2). Bu yeni sürümde, biz:

- Azure AD eşitlediğinde bir kullanıcıyı Devre Dışı Bırakılan'dan Etkin'e değiştirdiğinde kullanıcıya bir e-posta gönderilmesi gibi bir sorun giderilir.

- Etiketler işlevini kullanmaya devam ederken, müşterilerin başarılı bir şekilde yükseltebilmeleri için bir sorun giderildi.

- Kosova (+383) ülke kodu eklendi.

- MultiFactorAuthSvc.log'a tek seferlik bypass denetim günlüğü eklendi.

- Web Hizmeti SDK için geliştirilmiş performans.

- Diğer küçük hatalar düzeltildi.

Microsoft, 1 Temmuz 2019'dan itibaren yeni dağıtımlar için MFA Server teklifini durdurdu. Çok faktörlü kimlik doğrulama gerektiren yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. Daha fazla bilgi için bulut [tabanlı Azure Çok Faktörlü Kimlik Doğrulama dağıtımı planlama'ya](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)bakın.

---

## <a name="august-2019"></a>Ağustos 2019

### <a name="enhanced-search-filtering-and-sorting-for-groups-is-available-in-the-azure-ad-portal-public-preview"></a>Gruplar için geliştirilmiş arama, filtreleme ve sıralama Azure AD portalında (Genel Önizleme) kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Azure AD portalında, gruplarla ilgili gelişmiş deneyimlerin genel önizleme kullanılabilirliğini duyurmaktan mutluluk duyarız. Bu geliştirmeler, şunları sağlayarak grupları ve üye listelerini daha iyi yönetmenize yardımcı olur:

- Grup listelerinde alt düzey arama gibi gelişmiş arama yetenekleri.
- Üye ve sahip listelerinde gelişmiş filtreleme ve sıralama seçenekleri.
- Üye ve sahip listeleri için yeni arama özellikleri.
- Büyük gruplar için daha doğru grup sayıları.

Daha fazla bilgi için Azure [portalındaki grupları yönet'e](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)bakın.

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>Uygulama kayıt yönetimi için yeni özel roller mevcuttur (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Rbac  
**Ürün kapasitesi:** Erişim Kontrolü

Özel roller (Azure AD P1 veya P2 aboneliğiyle kullanılabilir) artık belirli izinlerle rol tanımları oluşturmanıza ve ardından bu rolleri belirli kaynaklara atamanıza izin vererek ayrıntılı erişim sağlamanıza yardımcı olabilir. Şu anda, uygulama kayıtlarını yönetmek için izinleri kullanarak ve ardından rolü belirli bir uygulamaya atayarak özel roller oluşturursunuz. Özel roller hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki (önizleme) Özel yönetici rollerine](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview)bakın.

Şu anda görmediğiniz ek izinlere veya kaynaklara ihtiyacınız varsa, Azure [geri bildirim sitemize](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) geri bildirim gönderebilir ve isteğinizi güncelleme yol haritamıza ekleyeceğiz.

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>Yeni sağlama günlükleri, uygulama sağlama dağıtımınızı izlemenize ve sorun gidermenize yardımcı olabilir (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi

Kullanıcı ve grup sağlama dağıtımını izlemenize ve sorun gidermenize yardımcı olmak için yeni sağlama günlükleri kullanılabilir. Bu yeni günlük dosyaları hakkında bilgi içerir:

- [ServiceNow'da](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial) hangi gruplar başarıyla oluşturuldu
- [Amazon Web Services'ten (AWS)](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws) hangi roller alındı?
- [Çalışanların İş Günü'nden](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) alınmadığı

Daha fazla bilgi için Azure [Etkin Dizin portalında (önizleme) Sağlama raporlarına](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)bakın.

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>Tüm Azure REKLAM yöneticileri için yeni güvenlik raporları (Genel Kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Varsayılan olarak, tüm Azure AD yöneticileri yakında Azure AD içindeki modern güvenlik raporlarına erişebilecektir. Eylül sonuna kadar, eski raporlara dönmek için modern güvenlik raporlarının en üstündeki afişi kullanabilirsiniz.

Modern güvenlik raporları, aşağıdakiler de dahil olmak üzere eski sürümlerden ek özellikler sağlar:

- Gelişmiş filtreleme ve sıralama
- Kullanıcı riskini niçin yok sayan toplu eylemler
- İhlal edilen veya güvenli varlıkların onayı
- Risk durumu, kapsayan: Risk altında, Görevden, Düzeltilmiş ve Teyit tehlikeye
- Riskle ilgili yeni algılamalar (Azure AD Premium aboneleri için kullanılabilir)

Daha fazla bilgi için Riskli [kullanıcılar, Riskli](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users) [oturum açma lar](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins)ve Risk [algılamalarına](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risk-detections)bakın.

---

### <a name="user-assigned-managed-identity-is-available-for-virtual-machines-and-virtual-machine-scale-sets-general-availability"></a>Sanal Makineler ve Sanal Makine Ölçek Setleri (Genel Kullanılabilirlik) için kullanıcı tarafından atanan yönetilen kimlik kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure kaynakları için yönetilen kimlikler  
**Ürün kapasitesi:** Geliştirici Deneyimi

Kullanıcı tarafından atanan yönetilen kimlikler artık genellikle Sanal Makineler ve Sanal Makine Ölçek Kümeleri için kullanılabilir. Bunun bir parçası olarak Azure, Azure AD kiracısında abonelik tarafından güvenilen ve bir veya daha fazla Azure hizmeti örneğine atanabilecek bir kimlik oluşturabilir. Kullanıcı tarafından atanan yönetilen kimlikler hakkında daha fazla bilgi için Azure [kaynakları için yönetilen kimlikler nedir?](https://aka.ms/azuremanagedidentity)

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>Kullanıcılar bir mobil uygulama veya donanım belirteci kullanarak parolalarını sıfırlayabilir (Genel Kullanılabilirlik)

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Self Servis Şifre Sıfırlama  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Kuruluşunuza bir mobil uygulama kaydetmiş olan kullanıcılar artık Microsoft Authenticator uygulamasından gelen bir bildirimi onaylayarak veya mobil uygulamalarından veya donanım belirtecinden bir kod girerek kendi parolalarını sıfırlayabilirler.

Daha fazla bilgi için [bkz: Azure AD self servis parola sıfırlama.](https://aka.ms/authappsspr) Kullanıcı deneyimi hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview)

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET, senaryolar adına paylaşılan MSAL.NET önbelleği yoksa

**Türü:** Sabit  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD kimlik doğrulama kitaplığı (ADAL.NET) sürüm 5.0.0-önizleme ile başlayarak, uygulama geliştiricilerin [web uygulamaları ve web API'leri için hesap başına bir önbelleği serihale etmesi](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api)gerekir. Aksi takdirde, akış [adına](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow)bazı senaryolar , bazı özel kullanım `UserAssertion`durumları ile birlikte , ayrıcalık bir yükseklik neden olabilir. Bu güvenlik açığını önlemek için ADAL.NET, artık senaryolar adına dotnet (MSAL.NET) paylaşılan önbelleği için Microsoft kimlik doğrulama kitaplığını yok sayar.

Bu sorunla ilgili daha fazla bilgi için azure [Etkin Dizin Kimlik Doğrulama Kitaplığı Ayrıcalık Güvenlik Açığı Yüksekliği'ne](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Azure AD App galerisinde bulunan yeni Federe Uygulamalar - Ağustos 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Ağustos 2019'da, Federasyon desteğine sahip 26 yeni uygulamayı uygulama galerisine ekledik:

[Toplumsal Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [Cognidox](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [Viareport's Inativ Portal (Avrupa)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [Robin](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), [Akademi Katılım](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial), [Öncelik Matris](https://sync.appfluence.com/pmwebng/), [Cousto MySpace](https://cousto.platformers.be/account/login), [Uploadcare](https://uploadcare.com/accounts/signup/), [Karbonit Endpoint Yedekleme](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial), CP [Cincom tarafından QSync](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial), [Chargebee](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial), [&trade; deliver.media Portal](https://portal.deliver.media), [Frontline Eğitim](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial), [F5](https://www.f5.com/products/security/access-policy-manager), [stashcat AD bağlanmak](https://www.stashcat.com), [Blink](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [Vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [Sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [Darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [Renklere göre izle](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial), [Harness](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial), [EAB Stratejik Bakım gezinmek](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>AzureAD PowerShell ve AzureADPreview PowerShell modüllerinin yeni sürümleri mevcuttur

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Dizin

AzureAD ve AzureAD Preview PowerShell modüllerinde yeni güncellemeler mevcuttur:

- AzureAD `-Filter` modülündeki parametreye `Get-AzureADDirectoryRole` yeni bir parametre eklendi. Bu parametre, cmdlet tarafından döndürülen dizin rollerine filtre uygulamanıza yardımcı olur.
- Azure ADPreview modülüne, Azure AD'de aşağıdakiler dahil olmak üzere özel rollerin tanımlanmasına ve atayabına yardımcı olmak için yeni cmdletler eklendi:

    - `Get-AzureADMSRoleAssignment`
    - `Get-AzureADMSRoleDefinition`
    - `New-AzureADMSRoleAssignment`
    - `New-AzureADMSRoleDefinition`
    - `Remove-AzureADMSRoleAssignment`
    - `Remove-AzureADMSRoleDefinition`
    - `Set-AzureADMSRoleDefinition`

---

### <a name="improvements-to-the-ui-of-the-dynamic-group-rule-builder-in-the-azure-portal"></a>Azure portalındaki dinamik grup kural oluşturucusu arabirimi geliştirmeleri

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Yeni bir kural daha kolay ayarlamanıza veya varolan kuralları değiştirmenize yardımcı olmak için Azure portalında bulunan dinamik grup kuralı oluşturucusunda bazı Kişisel Ara birimi iyileştirmeleri yaptık. Bu tasarım geliştirme, yalnızca bir yerine en fazla beş ifadeiçeren kurallar oluşturmanıza olanak tanır. Ayrıca, amortismana uyran aygıt özelliklerini kaldırmak için aygıt özellik listesini de güncelledik.

Daha fazla bilgi için dinamik [üyelik kurallarını yönet'e](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)bakın.

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>Erişim incelemeleri ile kullanıma sunulan yeni Microsoft Graph uygulaması izni

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Erişim İncelemeleri  
**Ürün kapasitesi:** Kimlik Yönetimi

Uygulamaların grup üyelikleri ve uygulama atamaları `AccessReview.ReadWrite.Membership`için erişim yorumlarını otomatik olarak oluşturmasına ve almasına olanak tanıyan yeni bir Microsoft Graph uygulaması izni sunduk. Bu izin, zamanlanmış işleriniz tarafından veya oturum açmış bir kullanıcı bağlamı gerektirmeden otomasyonunuzun bir parçası olarak kullanılabilir.

Daha fazla bilgi için [PowerShell blogu yla Microsoft Graph uygulama izinlerini kullanarak Azure AD erişim gözden incelemeleri nasıl oluşturulur](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241)örneğine bakın.

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Azure REKLAM etkinlik günlükleri artık Azure Monitor'da devlet bulutu örnekleri için kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Azure AD etkinlik günlüklerinin artık Azure Monitor'da devlet bulutu örnekleri için kullanılabildiğini duyurmaktan heyecan duyuyoruz. Artık Azure AD günlüklerini depolama hesabınıza veya [Sumlogic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [Splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk)ve [ArcSight](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight)gibi SIEM araçlarınızla tümleştirmek için bir etkinlik merkezine gönderebilirsiniz. 

Azure Monitörü'ne ayarlama hakkında daha fazla bilgi için [Azure Monitor'daki Azure REKLAM etkinlik günlüklerine](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations)bakın.

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Kullanıcılarınızı yeni ve gelişmiş güvenlik bilgileri deneyimiyle güncelleyin

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:**  Kimlik doğrulamaları (Oturum Açmalar)   
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

25 Eylül 2019'da, kullanıcı güvenlik bilgilerini kaydetmek ve yönetmek için eski, geliştirilmiş olmayan güvenlik bilgileri deneyimini kapatmış ve yalnızca yeni, [geliştirilmiş sürümü](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271)açacağız. Bu, kullanıcılarınızın artık eski deneyimi kullanamayacağı anlamına gelir.

Gelişmiş güvenlik bilgileri deneyimi hakkında daha fazla bilgi için [yönetici belgelerimize](https://aka.ms/securityinfodocs) ve [kullanıcı belgelerimize](https://aka.ms/securityinfoguide)bakın.

#### <a name="to-turn-on-this-new-experience-you-must"></a>Bu yeni deneyimi açmak için şunları yapmalısınız:

1. Azure portalında Global Administrator veya Kullanıcı Yöneticisi olarak oturum açın.

2. Azure **Active Directory > Kullanıcı ayarlarına**gidin > Access panel önizleme özellikleri için ayarları yönetin.

3. **Kullanıcılar, güvenlik bilgilerini kaydetmek ve yönetmek için önizleme özelliklerini kullanabilir - gelişmiş** alan, **Seçili'yi**seçin ve ardından bir kullanıcı grubu seçebilir veya kiracıdaki tüm kullanıcılar için bu özelliği açmak için **Tümü'nü** seçebilir.

4. **Kullanıcılar güvenlik **info**** alanını kaydetmek ve yönetmek için önizleme özelliklerini kullanabilirler, **Yok'u**seçin.

5. Ayarlarınızı kaydedin.

    Ayarlarınızı kaydettikten sonra, artık eski güvenlik bilgileri deneyimine erişemeniz gerekmez.

>[!Important]
>Bu adımları 25 Eylül 2019'dan önce tamamlamazsanız, azure Active Directory kiracınız gelişmiş deneyim için otomatik olarak etkinleştirilir. Sorularınız varsa, lütfen bizimle registrationpreview@microsoft.comirtibata geçiniz.

---

### <a name="authentication-requests-using-post-logins-will-be-more-strictly-validated"></a>POST oturum açmalarını kullanarak kimlik doğrulama istekleri daha sıkı bir şekilde doğrulanacaktır

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Standart

2 Eylül 2019 tarihinden itibaren POST yöntemini kullanan kimlik doğrulama istekleri HTTP standartlarına göre daha sıkı bir şekilde doğrulanacaktır. Özellikle, boşluklar ve çift tırnak (") artık istek formu değerlerinden kaldırılmaz. Bu değişikliklerin varolan istemcileri kırması beklenmez ve Azure AD'ye gönderilen isteklerin her seferinde güvenilir bir şekilde işlenmediğinden emin olmaya yardımcı olur.

Daha fazla bilgi için [Azure REKLAM'ın değişiklik bildirimlerini bozması'na](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored)bakın.

---

## <a name="july-2019"></a>Temmuz 2019

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>Değişiklik planı: Yalnızca TLS 1.2'yi desteklemek için Uygulama Proxy hizmet güncelleştirmesi

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Size en güçlü şifrelememizi sağlamak için, Uygulama Proxy hizmeti erişimini yalnızca TLS 1.2 protokolleriyle sınırlamaya başlayacağız. Bu sınırlama başlangıçta TLS 1.2 protokollerini zaten kullanmakta olan müşterilere sunulacaktır, böylece etkisini görmezsiniz. TLS 1.0 ve TLS 1.1 protokollerinin tamamen amortismanı 31 Ağustos 2019 tarihinde tamamlanacaktır. TLS 1.0 ve TLS 1.1 kullanan müşteriler, bu değişikliğe hazırlanmak için gelişmiş bildirim alacaklardır.

Bu değişiklik boyunca Uygulama Proxy hizmetine olan bağlantıyı korumak için, istemci-sunucu ve tarayıcı-sunucu kombinasyonlarınızın TLS 1.2'yi kullanacak şekilde güncelleştirdiğinden emin olmamızı öneririz. Ayrıca, uygulama proxy hizmeti aracılığıyla yayınlanan uygulamalara erişmek için çalışanlarınız tarafından kullanılan istemci sistemlerini eklemenizi öneririz.

Daha fazla bilgi için bkz: [Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>Değişiklik planı: Uygulama Galerisi için tasarım güncellemeleri geliyor

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Add **the** gallery alanından **Add** uygulama bıçağının tasarımına yeni kullanıcı arabirimi değişiklikleri geliyor. Bu değişiklikler, otomatik sağlama, OpenID Connect, Security Assertion Markup Language (SAML) ve Password single sign-on (SSO) destekleyen uygulamalarınızı daha kolay bulmanıza yardımcı olur.

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>Değişiklik planı: MFA sunucusu IP adresinin Office 365 IP adresinden kaldırılması

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Mfa  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

MFA sunucusu IP adresini [Office 365 IP Adresi ve URL Web hizmetinden](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service)kaldırıyoruz. Güvenlik duvarı ayarlarınızı güncellemek için şu anda bu sayfalara güveniyorsanız, [Azure Çok Faktörlü Kimlik Doğrulama Sunucusu makalesiyle başlarken](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements) Azure **Çok Faktörlü Kimlik Doğrulama Sunucusu güvenlik duvarı gereksinimleri** bölümüne belgelenen IP adreslerinin listesini de dahil ettiğinizden emin olmalısınız.

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>Yalnızca uygulama belirteçleri artık istemci uygulamasının kaynak kiracısında bulunmasını gerektirir

**Türü:** Sabit  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

26 Temmuz 2019'da, [müşteri kimlik bilgileri hibesi](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)aracılığıyla yalnızca uygulama belirteçlerini nasıl sağladığımızı değiştirdik. Daha önce, istemci uygulamasının kiracıda olup olmadığına bakılmaksızın, uygulamalar diğer uygulamaları aramak için belirteçler alabilir. Bu davranışı, bazen Web API'leri olarak da adlandırılan tek kiracılı kaynakların yalnızca kaynak kiracısında bulunan istemci uygulamaları tarafından çağrılaabilmesi için güncelledik.

Uygulamanız kaynak kiracıda bulunmuyorsa, bu sorunu gidermek `The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.` için, yönetici onayı bitiş [noktasını](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint) veya kiracınızın uygulamanın kiracı içinde çalışmasına izin verilmesini sağlayan [PowerShell aracılığıyla](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell)kiracıda istemci uygulama hizmeti ilkesini oluşturmanız gerektiğini belirten bir hata iletisi alırsınız.

Daha fazla bilgi için kimlik [doğrulama için yeniliklere bakın.](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant)

> [!NOTE]
> İstemci ve API arasında varolan onay gerekli olmaya devam etmektedir. Uygulamalar hala kendi yetkilendirme denetimlerini yapıyor olmalıdır.

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>FIDO2 güvenlik anahtarlarını kullanarak Azure AD'de yeni şifresiz oturum açma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD müşterileri artık kuruluşlarının kullanıcıları ve grupları için FIDO2 güvenlik anahtarlarını yönetecek ilkeler ayarlayabilir. Son kullanıcılar ayrıca güvenlik anahtarlarını kendi kendilerine kaydedebilir, FIDO özellikli cihazlarda yken web sitelerinde Microsoft hesaplarında oturum açma anahtarlarını kullanabilir ve Azure AD'ye katılan Windows 10 cihazlarında oturum açabilir.

Daha fazla bilgi için, yöneticiyle ilgili bilgiler [için Azure AD için parolasız oturum aç (önizleme)](/azure/active-directory/authentication/concept-authentication-passwordless) ve son kullanıcıyla ilgili bilgiler için bir güvenlik anahtarı [(Önizleme) kullanmak üzere güvenlik bilgilerini ayarlama'ya](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key) bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Azure AD App galerisinde bulunan yeni Federe Uygulamalar - Temmuz 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Temmuz 2019'da, Federasyon desteğine sahip bu 18 yeni uygulamayı uygulama galerisine ekledik:

[Ungerboeck Yazılım](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial), [Parlak Desen Omnichannel İletişim Merkezi](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial), Clever [Nelly](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial), [AcquireIO](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial), [Looop](https://www.looop.co/schedule-a-demo/), [ürün borda](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial), [MS Azure SSO Erişim Ethidex Uyumluluk Ofisi&trade;için](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso), [Hype](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial), [Özet](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial), [Ascentis](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial), [Flipsnack](https://www.flipsnack.com/accounts/sign-in-sso.html), [Wandera](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial), [TwineSocial](https://twinesocial.com/), [Kallidus](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial), [HyperAnna](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial), [Pharmid WasteWitness](https://pharmid.com/), [i2B Connect](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/), [JFrog Artifactory](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için kullanıcı hesabı sağlamayı otomatikleştirin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** İzleme & Raporlama

Artık bu yeni tümleken uygulamalar için kullanıcı hesaplarını oluşturmayı, güncelleyebilir ve silebilirsiniz:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federasyon Dizini](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Otomatik kullanıcı hesabı sağlama yı kullanarak kuruluşunuzun daha iyi nasıl güvenli hale sağlayacağıhakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlama](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) sağlama sını otomatikleştir'e bakın

---

### <a name="new-azure-ad-domain-services-service-tag-for-network-security-group"></a>Ağ Güvenliği Grubu için yeni Azure AD Etki Alanı Hizmetleri hizmet etiketi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Etki Alanı Hizmetleri  
**Ürün kapasitesi:** Azure AD Etki Alanı Hizmetleri

UZUN IP adresleri ve aralıkları listelerini yönetmekten sıkıldıysanız, Azure AD Etki Alanı Hizmetleri sanal ağ alt ağınıza gelen trafiği güvenli hale getirmek için Azure ağ güvenlik grubunuzdaki yeni **AzureActiveDirectoryDomainServices** ağ hizmet etiketini kullanabilirsiniz.

Bu yeni hizmet etiketi hakkında daha fazla bilgi için [Azure AD Etki Alanı Hizmetleri için Ağ Güvenlik Grupları'na](../../active-directory-domain-services/network-considerations.md#network-security-groups-and-required-ports)bakın.

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Etki Alanı Hizmetleri için Yeni Güvenlik Denetimleri (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Etki Alanı Hizmetleri  
**Ürün kapasitesi:** Azure AD Etki Alanı Hizmetleri

Azure AD Etki Alanı Hizmeti Güvenliği Denetimi'nin genel önizlemesürümüne duyurulmasından mutluluk duyarız. Güvenlik denetimi, Azure AD Etki Alanı Hizmeti portalını kullanarak Azure Depolama, Azure Log Analytics çalışma alanları ve Azure Etkinlik Merkezi gibi hedeflenen kaynaklara güvenlik denetim etkinlikleri akışı sağlayarak kimlik doğrulama hizmetlerinizde kritik bilgiler sağlamanıza yardımcı olur.

Daha fazla bilgi için bkz: [Azure AD Etki Alanı Hizmetleri için Güvenlik Denetimlerini Etkinleştir (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>Yeni Kimlik Doğrulama yöntemleri kullanımı & öngörüleri (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Self Servis Şifre Sıfırlama  
**Ürün kapasitesi:** İzleme & Raporlama

Yeni Kimlik Doğrulama yöntemleri &, azure çok faktörlü kimlik doğrulama ve self servis parola sıfırlama gibi özelliklerin kuruluşunuzda nasıl kaydedildiğini ve kullanıldığını (her bir özellik için kayıtlı kullanıcı sayısı, parolaları sıfırlamak için ne sıklıkta self servis parola sıfırlamanın kullanıldığını ve sıfırlama yöntemiyle nasıl yapıldığını anlamanıza yardımcı olabilir.

Daha fazla bilgi için [&](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights)bkz.

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>Tüm Azure AD yöneticileri için yeni güvenlik raporları kullanılabilir (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Tüm Azure REKLAM yöneticileri artık, **Riskli kullanıcılarda** ve **Riskli oturum** açma raporlarında gösterildiği gibi yeni güvenlik deneyimini kullanmaya başlamak için risk raporu **için işaretlenen Kullanıcılar** gibi varolan güvenlik raporlarının üst kısmındaki banner'ı seçebilir. Zaman içinde, tüm güvenlik raporları eski sürümlerden yeni sürümlere taşınır ve yeni raporlar size aşağıdaki ek özellikler sağlar:

- Gelişmiş filtreleme ve sıralama

- Kullanıcı riskini niçin yok sayan toplu eylemler

- İhlal edilen veya güvenli varlıkların onayı

- Risk durumu, kapsayan: Risk altında, Görevden, Düzeltilmiş ve Teyit tehlikeye

Daha fazla bilgi için Bkz. [Riskli kullanıcılar raporu](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users) ve Riskli oturum açma [raporu.](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins)

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Etki Alanı Hizmetleri için Yeni Güvenlik Denetimleri (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Etki Alanı Hizmetleri  
**Ürün kapasitesi:** Azure AD Etki Alanı Hizmetleri

Azure AD Etki Alanı Hizmeti Güvenliği Denetimi'nin genel önizlemesürümüne duyurulmasından mutluluk duyarız. Güvenlik denetimi, Azure AD Etki Alanı Hizmeti portalını kullanarak Azure Depolama, Azure Log Analytics çalışma alanları ve Azure Etkinlik Merkezi gibi hedeflenen kaynaklara güvenlik denetim etkinlikleri akışı sağlayarak kimlik doğrulama hizmetlerinizde kritik bilgiler sağlamanıza yardımcı olur.

Daha fazla bilgi için bkz: [Azure AD Etki Alanı Hizmetleri için Güvenlik Denetimlerini Etkinleştir (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>SAML/WS-Fed kullanarak yeni B2B doğrudan federasyon (Genel Önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C

Doğrudan federasyon, SAML veya WS-Fed standartlarını destekleyen kimlik sistemleriyle çalışarak, BT tarafından yönetilen kimlik çözümü Azure AD olmayan iş ortaklarıyla çalışmanızı kolaylaştırır. Bir iş ortağıyla doğrudan federasyon ilişkisi kurduktan sonra, bu etki alanından davet ettiğiniz herhangi bir yeni konuk kullanıcı, mevcut kuruluş hesaplarını kullanarak sizinle işbirliği yapabilir ve bu da misafirlerinizin kullanıcı deneyimini daha sorunsuz hale getirebilir.

Daha fazla bilgi için, [konuk kullanıcılar için AD FS ve üçüncü taraf sağlayıcılarla doğrudan federasyon (önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation)bakın.

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için kullanıcı hesabı sağlamayı otomatikleştirin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** İzleme & Raporlama

Artık bu yeni tümleken uygulamalar için kullanıcı hesaplarını oluşturmayı, güncelleyebilir ve silebilirsiniz:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federasyon Dizini](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Otomatik kullanıcı hesabı sağlama yı kullanarak kuruluşunuzun daha iyi nasıl güvenli hale alınabildiğini hakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlamasağlamayı otomatikleştir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)bakın.

---

### <a name="new-check-for-duplicate-group-names-in-the-azure-ad-portal"></a>Azure AD portalında yinelenen grup adları için yeni denetim

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Azure AD portalından bir grup adı oluşturduğunuzda veya güncellediğinizde, kaynağınızda varolan bir grup adını çoğaltıp çoğaltmadığınızı görmek için bir denetim gerçekleştiririz. Adın başka bir grup tarafından zaten kullanılmakta olduğunu belirlersek, adınızı değiştirmeniz istenir.

Daha fazla bilgi için Azure [AD portalındaki grupları yönet'e](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)bakın.

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD artık yanıt (yönlendirme) URL'lerinde statik sorgu parametrelerini destekler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD uygulamaları artık OAuth 2.0 istekleri için statik sorgu `https://contoso.com/oauth2?idp=microsoft`parametreleri (örneğin, ) ile yanıt (yeniden yönlendirme) URL'lerini kaydedebilir ve kullanabilir. Statik sorgu parametresi, yanıt URI'sinin diğer tüm parçası gibi yanıt URI'leri için dize eşleştirmesine tabidir. URL deşifre edilen yeniden yönlendirme-uri ile eşleşen kayıtlı bir dize yoksa, istek reddedilir. Yanıt URI bulunursa, statik sorgu parametresi de dahil olmak üzere kullanıcıyı yeniden yönlendirmek için tüm dize kullanılır.

Dinamik yanıt URL'leri, bir güvenlik riski temsil ettikleri ve kimlik doğrulama isteği nde durum bilgilerini saklamak için kullanılamadığından hala yasaktır. Bu amaçla, parametreyi `state` kullanın.

Şu anda, Azure portalının uygulama kayıt ekranları sorgu parametrelerini hala engelliyor. Ancak, uygulamanıza sorgu parametreleri eklemek ve test etmek için uygulama bildirimini el ile edinebilirsiniz. Daha fazla bilgi için kimlik [doğrulama için yeniliklere bakın.](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters)

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Azure AD için etkinlik günlükleri (MS Graph API'leri) artık PowerShell Cmdlets aracılığıyla kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Azure AD etkinlik günlüklerinin (Denetim ve Oturum Açma raporları) Azure AD PowerShell modülü üzerinden kullanılabildiğini duyurmaktan heyecan duyuyoruz. Daha önce, MS Graph API uç noktalarını kullanarak kendi komut dosyalarınızı oluşturabilirsiniz ve şimdi bu özelliği PowerShell cmdlets'e genişlettik.

Bu cmdlets nasıl kullanılacağı hakkında daha fazla bilgi için, [raporlama için Azure AD PowerShell cmdlets](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting)bakın.

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Azure AD'de Denetim ve Oturum Açma günlükleri için güncelleştirilmiş filtre denetimleri

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Denetim ve Oturum Açma günlük raporlarını güncelledik, böylece artık çeşitli filtreleri rapor ekranlarına sütun olarak eklemek zorunda kalmadan uygulayabilirsiniz. Ayrıca, artık ekranda kaç filtre göstermek istediğinize karar verebilirsiniz. Bu güncelleştirmelerin tümü, raporlarınızın okunmasını ve gereksinimlerinize daha fazla kapsamlı olmasını sağlamak için birlikte çalışır.

Bu güncelleştirmeler hakkında daha fazla bilgi için filtre [denetim günlükleri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs) ve [Filtre oturum açma etkinliklerine](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities)bakın.

---

## <a name="june-2019"></a>Haziran 2019

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>Microsoft Graph için yeni risk Algılamaları API (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Microsoft Graph için yeni risk Detections API'sinin artık genel önizlemede olduğunu duyurmaktan mutluluk duyuyoruz. Kuruluşunuzun Kimlik Koruması yla ilgili kullanıcısının listesini ve oturum açma risk algılamalarını görüntülemek için bu yeni API'yi kullanabilirsiniz. Bu API'yi, algılama türü, durum, düzey ve daha fazlası hakkındaki ayrıntılar da dahil olmak üzere risk algılamalarınızı daha verimli bir şekilde sorgulamak için de kullanabilirsiniz.

Daha fazla bilgi için [Risk algılama API başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Azure AD uygulama galerisinde bulunan yeni Federe Uygulamalar - Haziran 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Haziran 2019'da, Federasyon desteğine sahip bu 22 yeni uygulamayı uygulama galerisine ekledik:

[Azure AD SAML Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial), [Otsuka Shokai (Japonca)](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial), [ANAQUA](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial), [Azure VPN İstemci](https://portal.azure.com/), [ExpenseIn](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial), [Helper Helper](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial), [Costpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial), [GlobalOne](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial), [Mercedes-Benz Araç İçi Ofis](https://me.secure.mercedes-benz.com/), [Skore](https://app.justskore.it/), [Oracle Cloud Infrastructure Console](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial), [CyberArk SAML Kimlik Doğrulama](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial), [Scrible Edu](https://www.scrible.com/sign-in/#/create-account), [PandaDoc](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial), [Perceptyx](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial), [Proptimise OS](https://proptimise.co.uk/software/), [Vtiger CRM (SAML)](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial), Oracle Access Yöneticisi Oracle Perakende Satış, Satış Için, Oracle E-Business Suite için Oracle Access Yöneticisi, E-Business Suite için Oracle IDCS, PeopleSoft için Oracle IDCS, JD Edwards için Oracle IDCS

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için kullanıcı hesabı sağlamayı otomatikleştirin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** İzleme & Raporlama

Artık bu yeni tümleken uygulamalar için kullanıcı hesaplarını oluşturmayı, güncelleyebilir ve silebilirsiniz:

- [Zoom](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Envoy](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

Otomatik kullanıcı hesabı sağlama yı kullanarak kuruluşunuzun daha iyi nasıl güvenli hale sağlayacağıhakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlama](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) sağlama sını otomatikleştir'e bakın

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>Azure AD sağlama hizmetinin gerçek zamanlı ilerlemesini görüntüleyin

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi

Azure AD sağlama deneyimini, kullanıcı sağlama sürecinde ne kadar uzakta olduğunuzu gösteren yeni bir ilerleme çubuğu içerecek şekilde güncelledik. Bu güncelleştirilmiş deneyim, geçerli döngü sırasında sağlanan kullanıcı sayısı ve bugüne kadar kaç kullanıcının sağlandığı hakkında da bilgi sağlar.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>Şirket markası artık oturum açma ve hata ekranlarında görünüyor

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD'yi, şirket markanızın artık oturum açma ve hata ekranlarının yanı sıra oturum açma sayfasında görünmesi için güncelledik. Bu özelliği açmak için hiçbir şey yapmanız alameti yoktur, Azure AD yalnızca Azure portalının **Şirket markalama** alanında zaten kurduğunuz varlıkları kullanır.

Şirket markanızı ayarlama hakkında daha fazla bilgi için [kuruluşunuzun Azure Etkin Dizin sayfalarına marka ekleme 'ye](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding)bakın.

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>Azure Çok Faktörlü Kimlik Doğrulama (MFA) Sunucusu artık yeni dağıtımlar için kullanılamıyor

**Türü:** Kaldırıl -mış  
**Hizmet kategorisi:** Mfa  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kuruluşlarında çok faktörlü kimlik doğrulaması gerektiren yeni müşterilerin artık bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanması gerekir. MFA Server'ı 1 Temmuz'dan önce etkinleştiren müşteriler de bir değişiklik görmez. Yine de en son sürümü indirebilir, gelecekteki güncelleştirmeleri alabilir ve etkinleştirme kimlik bilgilerini oluşturabilirsiniz.

Daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama Sunucusu'na başlarken](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)bakın. Bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması hakkında daha fazla bilgi için bulut [tabanlı Azure Çok Faktörlü Kimlik Doğrulama dağıtımı planlama'ya](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)bakın.

---

## <a name="may-2019"></a>Mayıs 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Hizmet değişikliği: Uygulama Proxy hizmetinde yalnızca TLS 1.2 protokolleri için gelecekteki destek

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Müşterilerimiz için sınıfının en iyisi şifrelemenin sağlanmasına yardımcı olmak için, Uygulama Proxy hizmetinde yalnızca TLS 1.2 protokollerine erişimi sınırlandırıyoruz. Bu değişiklik, yalnızca TLS 1.2 protokollerini kullanan müşterilere kademeli olarak dağıtılmaktadır, bu nedenle herhangi bir değişiklik görmemeniz gerekir.

TLS 1.0 ve TLS 1.1 amortismanı 31 Ağustos 2019 tarihinde gerçekleşir, ancak ek gelişmiş bildirim sağlayacağız, böylece bu değişikliğe hazırlanmak için zamanA sahip olacaksınız. Bu değişikliğe hazırlanmak için, kullanıcılarınızın Application Proxy aracılığıyla yayınlanan uygulamalara erişmek için kullandıkları istemciler de dahil olmak üzere istemci-sunucu ve tarayıcı-sunucu kombinasyonlarının, Uygulama Proxy hizmetine olan bağlantıyı korumak için TLS 1.2 protokolünü kullanacak şekilde güncelleştirdiğinden emin olun. Daha fazla bilgi için bkz: [Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin)

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Uygulamayla ilgili oturum açma verilerinizi görüntülemek için kullanım ve öngörüraporunu kullanma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** İzleme & Raporlama

Artık Azure portalının **Kurumsal uygulamalar** alanında bulunan kullanım ve öngörüler raporunu kullanarak oturum açma verilerinizin uygulama merkezli bir görünümünü elde edebilirsiniz:

- Kuruluşunuz için en çok kullanılan uygulamalar

- En başarısız oturum açma lara sahip uygulamalar

- Her uygulama için en iyi oturum açma hataları

Bu özellik hakkında daha fazla bilgi için [Azure Active Directory portalında Kullanım ve öngörüler raporuna](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report) bakın

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Azure AD'yi kullanarak kullanıcı sağlamanızı bulut uygulamalarına otomatikleştirme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** İzleme & Raporlama

Aşağıdaki bulut tabanlı uygulamalar için kullanıcı hesaplarının oluşturulmasını, silinmesini ve güncelleştirilmesini otomatikleştirmek için Azure AD Sağlama Hizmetini kullanmak için aşağıdaki yeni öğreticileri izleyin:

- [Comeet](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [Dinamik Sinyal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [Kaleci Güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

Ayrıca, grup nesnelerinin nasıl sağlandığı hakkında bilgi sağlayan bu yeni [Dropbox öğreticisini](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial)de takip edebilirsiniz.

Otomatik leştirilmiş kullanıcı hesabı sağlama yoluyla kuruluşunuzun daha iyi nasıl güvenli hale alınabildiğini hakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlamasağlamayı otomatikleştir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)bakın.

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>Kimlik güvenli puanı artık Azure AD'de kullanılabilir (Genel kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Yok  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Artık Azure AD'deki kimlik güvenliği puanı özelliğini kullanarak kimlik güvenliği duruşunuzu izleyebilir ve geliştirebilirsiniz. Kimlik güvenli puan özelliği, size yardımcı olmak için tek bir pano kullanır:

- 1 ile 223 arasındaki puana göre kimlik güvenliği duruşunuzu objektif olarak ölçün.

- Kimlik güvenliği iyileştirmelerinizi planlayın

- Güvenlik geliştirmelerinizin başarısını gözden geçirin

Kimlik güvenliği puanı özelliği hakkında daha fazla bilgi için Azure [Active Directory'deki kimlik güvenli puanı nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score)

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Yeni Uygulama kayıtları deneyimi artık kullanılabilir (Genel kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Geliştirici Deneyimi

Yeni [Uygulama kayıtları](https://aka.ms/appregistrations) deneyimi artık genel olarak kullanılabilir durumda. Bu yeni deneyim, Azure portalından ve Uygulama Kaydı portalından aşina olduğunuz tüm temel özellikleri içerir ve bunları aşağıdakiler aracılığıyla geliştirir:

- **Daha iyi uygulama yönetimi.** Uygulamalarınızı farklı portallarda görmek yerine, artık tüm uygulamalarınızı tek bir konumda görebilirsiniz.

- **Basitleştirilmiş uygulama kaydı.** Geliştirilmiş navigasyon deneyiminden yenilenen izin seçimi deneyimine kadar uygulamalarınızı kaydetmek ve yönetmek artık daha kolay.

- **Daha ayrıntılı bilgi.** Hızlı başlatma kılavuzları ve daha fazlası dahil olmak üzere uygulamanız hakkında daha fazla ayrıntı bulabilirsiniz.

Daha fazla bilgi için [Microsoft kimlik platformuna](https://docs.microsoft.com/azure/active-directory/develop/) bakın ve [Uygulama kayıtları deneyimi artık genel olarak kullanılabilir!](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) blog duyurusu.

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Kimlik Koruması için Riskli Kullanıcı API'sinde yeni özellikler mevcut

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Artık Riskli Kullanıcı API'sini kullanıcıların risk geçmişini almak, riskli kullanıcıları görevden almak ve kullanıcıların tehlikede olduğunu onaylamak için kullanabileceğinizi duyurmaktan mutluluk duyuyoruz. Bu değişiklik, kullanıcılarınızın risk durumunu daha verimli bir şekilde güncelleştirmenize ve risk geçmişlerini anlamanıza yardımcı olur.

Daha fazla bilgi için [Riskli Kullanıcılar API başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Azure AD uygulama galerisinde bulunan yeni Federe Uygulamalar - Mayıs 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Mayıs 2019'da, Federasyon desteğine sahip bu 21 yeni uygulamayı uygulama galerisine ekledik:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [Real Linkler](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kianda](https://app.kianda.com/sso/OpenID/AzureAD/), [Basit İşaret](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [Braze](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [Görüntüleyici](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [Templafy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo Satış Meşgul](https://toutapp.com/login), [ACLP](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [Meta4 Global HR](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), Kuantum [İşyeri](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [Kobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [webMethods API Bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [RedFlag](https://pocketstop.com/redflag/), [Whatfix](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [Kontrol](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [JOBHUB](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [NEOGOV](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), [Foodee](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial), [MyVR](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Azure AD portalında gelişmiş grup oluşturma ve yönetim deneyimleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Azure AD portalında gruplarla ilgili deneyimlerde iyileştirmeler yaptık. Bu geliştirmeler, yöneticilerin grup listelerini, üye listelerini daha iyi yönetmesine ve ek oluşturma seçenekleri sağlamasına olanak sağlar.

Geliştirmeler şunlardır:

- Üyelik türüne ve grup türüne göre temel filtreleme.

- Kaynak ve E-posta adresi gibi yeni sütunların eklenmesi.

- Kolay silme için grupları, üyeleri ve sahip listelerini birden çok seçme yeteneği.

- Grup oluşturma sırasında bir e-posta adresi seçme ve sahip ekleme becerisi.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal)

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Azure AD portalındaki Office 365 grupları için bir adlandırma ilkesi ni yapılandırma (Genel kullanılabilirlik)

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi ni yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kurallarının uygulanmasına yardımcı olur.

Office 365 grupları için adlandırma ilkesini iki farklı şekilde yapılandırabilirsiniz:

- Grup adına otomatik olarak eklenen önekleri veya sonekleri tanımlayın.

- Kuruluşunuz için grup adlarında izin verilmeyen özelleştirilmiş engellenmiş sözcükler kümesi yükleyin (örneğin, "CEO, Bordro, İk").

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy)

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Azure REKLAM etkinlik günlükleri (Genel kullanılabilirlik) için Artık Microsoft Graph API uç noktaları kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Azure REKLAM etkinlik günlükleri için Microsoft Graph API uç noktaları desteğinin genel kullanılabilirliğini duyurmaktan mutluluk duyarız. Bu sürümle, artık hem Azure AD denetim günlüklerinin hem de oturum açma günlüklerinin Sürüm 1.0'ını kullanabilirsiniz.

Daha fazla bilgi için Azure [AD denetim günlüğü API'ye genel bakış'a](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0)bakın.

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Yöneticiler artık birleştirilmiş kayıt işlemi için Koşullu Erişim'i kullanabilir (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması  

Yöneticiler artık birleştirilmiş kayıt sayfası tarafından kullanılmak üzere Koşullu Erişim ilkeleri oluşturabilir. Bu, şu nda varsa kayda izin vermek için ilkeler uygulanmasını içerir:

- Kullanıcılar güvenilir bir ağdadır.

- Kullanıcılar düşük oturum açma riski altındadır.

- Kullanıcılar yönetilen bir aygıttadır.

- Kullanıcılar kuruluşun kullanım koşullarını (TOU) kabul eder.

Koşullu Erişim ve parola sıfırlama hakkında daha fazla bilgi için, [Azure AD birleşimi mfa ve parola sıfırlama kaydı deneyimi blog gönderisini koşullu erişim](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348)olarak görebilirsiniz. Birleşik kayıt işlemi için Koşullu Erişim ilkeleri hakkında daha fazla bilgi için, [birleşik kayıt için Koşullu Erişim ilkelerine](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration)bakın. Azure REKLAM kullanım koşulları özelliği hakkında daha fazla bilgi için [Azure Etkin Dizin özelliğine](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.

---

## <a name="april-2019"></a>Nisan 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>Yeni Azure AD tehdit zekası algılama artık Azure AD Kimlik Korumasının bir parçası olarak kullanıma sunuldu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Azure AD tehdit zekası algılama, güncelleştirilmiş Azure AD Kimlik Koruması özelliğinin bir parçası olarak artık kullanılabilir. Bu yeni işlev, Microsoft'un iç ve dış tehdit istihbarat kaynaklarına dayalı olarak bilinen saldırı kalıplarıyla tutarlı olan belirli bir kullanıcı veya etkinlik için olağandışı kullanıcı etkinliğini göstermeye yardımcı olur.

Azure AD Kimlik Koruması'nın yenilenmiş sürümü hakkında daha fazla bilgi için, [dört büyük Azure AD Kimlik Koruması geliştirmesinin artık genel önizleme](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) blogunda ve Azure Etkin [Dizin Kimlik Koruması (yenilenmiş) nedir'e bakın.](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) bakın. Azure AD tehdit istihbaratı algılama hakkında daha fazla bilgi için [Azure Etkin Dizin Kimlik Koruması risk algılama makalesine](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks) bakın.

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Azure AD yetkilendirme yönetimi artık kullanılabilir (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Yönetimi  
**Ürün kapasitesi:** Kimlik Yönetimi

Azure AD yetkilendirme yönetimi, artık genel önizlemede, müşterilerin çalışanların ve iş ortaklarının nasıl erişim isteyebileceğini, kimlerin onaylaması gerektiğini ve ne kadar süreyle erişime sahip olduklarını tanımlayan erişim paketlerinin yönetimini yetkilendirmelerine yardımcı olur. Erişim paketleri, Azure AD ve Office 365 gruplarına üyeliği, kurumsal uygulamalardaki rol atamaları ve SharePoint Online siteleri için rol atamaları yönetebilir. [Azure AD yetkilendirme yönetimine genel bakışta](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview)yetkilendirme yönetimi hakkında daha fazla bilgi edinin. Ayrıcalıklı Kimlik Yönetimi, erişim incelemeleri ve kullanım koşulları gibi Azure AD Kimlik Yönetimi özelliklerinin genişliği hakkında daha fazla bilgi edinmek için azure [AD Kimlik Yönetimi nedir?](../governance/identity-governance-overview.md)

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>Azure AD portalında Office 365 grupları için bir adlandırma ilkesi yapılandırma (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi ni yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kurallarının uygulanmasına yardımcı olur.

Office 365 grupları için adlandırma ilkesini iki farklı şekilde yapılandırabilirsiniz:

- Grup adına otomatik olarak eklenen önekleri veya sonekleri tanımlayın.

- Kuruluşunuz için grup adlarında izin verilmeyen özelleştirilmiş engellenmiş sözcükler kümesi yükleyin (örneğin, "CEO, Bordro, İk").

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy)

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure REKLAM Etkinliği günlükleri artık Azure Monitor'da kullanılabilir (Genel kullanılabilirlik)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Azure REKLAM Etkinliği günlükleriyle görselleştirmelerle ilgili geri bildirimlerinizi ele almak için Log Analytics'te yeni bir Öngörüler özelliği ni kullanıma salıyoruz. Bu özellik, Çalışma Kitapları adı verilen etkileşimli şablonlarımızı kullanarak Azure REKLAM kaynaklarınız hakkında öngörüler elde eve yardımcı olur. Önceden oluşturulmuş bu Çalışma Kitapları, uygulamalar veya kullanıcılar için ayrıntılar sağlayabilir ve şunları içerir:

- **Oturum açmalar.** Oturum açma konumu, kullanımda işletim sistemi veya tarayıcı istemcisi ve sürümü ve başarılı veya başarısız oturum açma sayısı gibi uygulamalar ve kullanıcılar için ayrıntılar sağlar.

- **Eski kimlik doğrulaması ve Koşullu Erişim.** Koşullu Erişim ilkeleri tarafından tetiklenen Çok Faktörlü Kimlik Doğrulama kullanımı, Koşullu Erişim ilkelerini kullanan uygulamalar ve benzeri gibi eski kimlik doğrulaması kullanan uygulamalar ve kullanıcılar için ayrıntılar sağlar.

- **Oturum açma hatası analizi.** Oturum açma hatalarınızın bir kullanıcı eylemi, ilke sorunları veya altyapınız nedeniyle oluşmuş olup olmadığını belirlemenize yardımcı olur.

- **Özel raporlar.** Kuruluşunuz için Öngörüler özelliğini özelleştirmeye yardımcı olmak için yeni çalışma kitapları oluşturabilir veya varolan Çalışma Kitaplarını düzenlemeyapabilirsiniz.

Daha fazla bilgi için Azure [Etkin Dizin raporları için Azure Monitor çalışma kitaplarını nasıl kullanacağınız](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks)abakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Azure AD uygulama galerisinde bulunan yeni Federe Uygulamalar - Nisan 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Nisan 2019'da, Federasyon desteğine sahip bu 21 yeni uygulamayı uygulama galerisine ekledik:

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [HRworks Tek İşaret-On](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [Percolate](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [MobiControl](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [Shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [Benchling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [EduBrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [RStudio Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba Cloud (Rol tabanlı SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [Certent Equity Management](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [Sectigo Sertifika Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [GreenOrbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [Workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [SurveyMonkey Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [Indiggo](https://indiggolead.com/)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>Yeni erişim incelemeleri sıklık seçeneği ve birden çok rol seçimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Erişim İncelemeleri  
**Ürün kapasitesi:** Kimlik Yönetimi

Azure AD erişim incelemelerinde yeni güncelleştirmeler şunları sağlar:

- Haftalık, aylık, üç aylık ve yıllık daha önce varolan seçeneklere ek olarak, erişim değerlendirmelerinizin sıklığını **altı aylık**olarak değiştirin.

- Tek bir erişim incelemesi oluştururken birden çok Azure AD ve Azure kaynak rolü seçin. Bu durumda, tüm roller aynı ayarlarla ayarlanır ve tüm gözden geçirenler aynı anda bildirilir.

Erişim incelemesi oluşturma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD Connect e-posta uyarı sistemi(ler) geçiş sürecinde, bazı müşteriler için yeni e-posta gönderen bilgileri gönderiyor

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** AD Senkronizasyonu  
**Ürün kapasitesi:** Platform

Azure AD Connect, bazı müşterilere yeni bir e-posta göndereni göstererek e-posta uyarı sistemimizi(ler) geçiş sürecindedir. Bunu gidermek için kuruluşunuzun izin listesine eklemeniz `azure-noreply@microsoft.com` gerekir veya Office 365, Azure veya Eşitleme hizmetlerinizden önemli uyarılar almaya devam edemezsiniz.

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>Azure AD Connect'teki Federe etki alanları arasında UPN sonek değişiklikleri artık başarılı oldu

**Türü:** Sabit  
**Hizmet kategorisi:** AD Senkronizasyonu  
**Ürün kapasitesi:** Platform

Artık bir kullanıcının UPN sonekini bir Federe etki alanından Azure AD Connect'te başka bir Federe etki alanına başarılı bir şekilde değiştirebilirsiniz. Bu düzeltme, eşitleme döngüsü sırasında FedereDomainChangeError hata iletisini artık deneyimlemeniz veya "Azure Active Directory'de bu nesneyi güncelleştirilemediğiniz, çünkü [FedereUser.UserPrincipalName] niteliği geçerli olmadığından bir bildirim e-postası almamalısınız. Yerel dizin hizmetlerinizdeki değeri güncelleştirin".

Daha fazla bilgi için [eşitleme sırasında Sorun Giderme Hataları'na](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror)bakın.

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Azure AD'de uygulama koruması tabanlı Koşullu Erişim ilkesini kullanarak artırılmış güvenlik (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Uygulama koruma tabanlı Koşullu Erişim artık **Uygulama koruma ilkesini kullanarak** kullanılabilir. Bu yeni ilke, şunları önlemeye yardımcı olarak kuruluşunuzun güvenliğini artırmaya yardımcı olur:

- Microsoft Intune lisansı olmayan uygulamalara erişen kullanıcılar.

- Kullanıcılar Microsoft Intune uygulama koruma ilkesini alamıyor.

- Yapılandırılmış bir Microsoft Intune uygulama koruma ilkesi olmadan uygulamalara erişim sağlayan kullanıcılar.

Daha fazla bilgi için, [Koşullu Erişim ile bulut uygulamasıerişimi için uygulama koruma ilkesinasıl gerekir'](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access)e bakın.

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Microsoft Edge'de Azure AD tek oturum açma ve Koşullu Erişim için yeni destek (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Azure AD tek oturum açma ve Koşullu Erişim için yeni destek sağlamak da dahil olmak üzere Microsoft Edge için Azure AD desteğimizi artırdık. Microsoft Intune Yönetilen Tarayıcı'yı daha önce kullandıysanız, artık Microsoft Edge'i kullanabilirsiniz.

Koşullu Erişim'i kullanarak cihazlarınızı ve uygulamalarınızı ayarlama ve yönetme hakkında daha fazla bilgi için, [Koşullu Erişim ile bulut uygulaması erişimi için yönetilen cihazları İste](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) ve [Koşullu Erişim ile bulut uygulaması erişimi için onaylı istemci uygulamalarını gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)ye bakın. Microsoft Intune ilkeleriyle Microsoft Edge'i kullanarak erişimi nasıl yönetişsüreceğiniz hakkında daha fazla bilgi için Microsoft [Intune ilkesi korumalı bir tarayıcı kullanarak Internet erişimini yönet'e](https://docs.microsoft.com/intune/app-configuration-managed-browser)bakın.

---

## <a name="march-2019"></a>Mart 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Azure Active Directory B2C'de Kimlik Deneyimi Çerçevesi ve özel ilke desteği artık kullanılabilir (GA)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Artık Azure AD B2C'de, ölçekte ve Azure SLA'mız altında desteklenen aşağıdaki görevler de dahil olmak üzere özel ilkeler oluşturabilirsiniz:

- Özel ilkeler kullanarak özel kimlik doğrulama kullanıcı yolculukları oluşturun ve yükleyin.

- Kullanıcı yolculuklarını adım adım talep sağlayıcılar arasında alışverişolarak açıklayın.

- Kullanıcı yolculuklarında koşullu dallanmayı tanımlayın.

- Gerçek zamanlı kararlarda ve iletişimlerde kullanım taleplerini dönüştürün ve haritalanın.

- Özel kimlik doğrulama kullanıcı yolculuklarınızda REST API özellikli hizmetleri kullanın. Örneğin, e-posta sağlayıcıları, CRM'ler ve özel yetkilendirme sistemleri yle.

- OpenIDConnect protokolüne uygun kimlik sağlayıcıları yla federate. Örneğin, çok kiracılı Azure AD ile, sosyal hesap sağlayıcıları veya iki faktörlü doğrulama sağlayıcıları.

Özel ilkeler oluşturma hakkında daha fazla bilgi için Azure [Active Directory B2C'deki özel ilkeler için Geliştirici notlarına](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-developer-notes-custom) bakın ve [örnek incelemeler de dahil olmak üzere Alex Simon'ın blog gönderilerini](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-custom-policies-to-build-your-own-identity-journeys/ba-p/382791)okuyun.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2019"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Mart 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Mart 2019'da, Federasyon desteğine sahip bu 14 yeni uygulamayı uygulama galerisine ekledik:

[ISEC7 Mobil Değişim Delegesi](https://www.isec7.com/english/), [MediusFlow](https://office365.cloudapp.mediusflow.com/), [ePlatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [Fulcrum](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [ExcelityGlobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Açıklama Tabanlı Denetim Sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [Yalın](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [Powerschool Performans Konuları](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [Cinode](https://cinode.com/), [Iris Intranet](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [Empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [TAS](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Azure AD galerisinde yeni Zscaler ve Atlassian sağlama bağlayıcıları - Mart 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** 3.

Aşağıdaki uygulamalar için kullanıcı hesapları oluşturmayı, güncelleştirmeyi ve silmeyi otomatikleştirin:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](https://aka.ms/ZscalerBetaProvisioning), [Zscaler Bir](https://aka.ms/ZscalerOneProvisioning), [Zscaler İki](https://aka.ms/ZscalerTwoProvisioning), [Zscaler Üç](https://aka.ms/ZscalerThreeProvisioning), [Zscaler ZSCloud](https://aka.ms/ZscalerZSCloudProvisioning), [Atlassian Bulut](https://aka.ms/atlassianCloudProvisioning)

Otomatik leştirilmiş kullanıcı hesabı sağlama yoluyla kuruluşunuzun daha iyi nasıl güvenli hale alınabildiğini hakkında daha fazla bilgi için Azure [AD ile SaaS uygulamalarına kullanıcı sağlamasağlamayı otomatikleştir'e](https://aka.ms/ProvisioningDocumentation)bakın.

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Azure AD portalında silinen Office 365 gruplarınızı geri yükleme ve yönetme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Artık silinen Office 365 gruplarınızı Azure AD portalından görüntüleyebilir ve yönetebilirsiniz. Bu değişiklik, kuruluşunuz tarafından gerekli olmayan grupları kalıcı olarak silmenize izin vermenin yanı sıra, hangi grupların geri yüklenenebilen olduğunu görmenize yardımcı olur.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore)

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Azure AD SAML tarafından güvenli şirket içi uygulamalariçin Uygulama Proxy 'si (genel önizleme) aracılığıyla tek oturum açma artık kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Artık şirket içi, SAML kimlik doğrulaması yapılan uygulamalar için tek bir oturum açma (SSO) deneyimi ve Bu uygulamalara Uygulama Proxy üzerinden uzaktan erişim sağlayabilirsiniz. Saml SSO'yu şirket içi uygulamalarınızla nasıl kurabileceğiniz hakkında daha fazla bilgi için [Uygulama Proxy 'si (Önizleme) ile şirket içi uygulamalar için SAML tek oturum](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps)açma'ya bakın.

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>Güvenilirlik ve kullanıcı deneyimini geliştirmek için istek döngüleri ndeki istemci uygulamaları kesintiye uğrar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

İstemci uygulamaları, kısa bir süre içinde aynı oturum açma isteklerinden yüzlercesini yanlış bir şekilde verebilir. Bu istekler, başarılı olsun veya olmasın, tüm bunlar zayıf bir kullanıcı deneyimine ve IDP için artan iş yüklerine katkıda bulunur, tüm kullanıcılar için gecikme süresini artırır ve IDP'nin kullanılabilirliğini azaltır.

Bu güncelleştirme `invalid_grant` bir `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request` hata gönderir: yinelenen istekleri kısa bir süre içinde, normal çalışma kapsamı dışında birden çok kez veren istemci uygulamalarına. Bu sorunla karşılaşan istemci uygulamaları, kullanıcının yeniden oturum açmasını gerektiren etkileşimli bir istem göstermelidir. Bu değişiklik ve bu hatayla karşılaşırsa uygulamanızın nasıl düzeltilenhakkında daha fazla bilgi için kimlik [doğrulama için yeniliklere bakın.](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted)

---

### <a name="new-audit-logs-user-experience-now-available"></a>Yeni Denetim Günlükleri kullanıcı deneyimi artık kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Hem okunabilirliği hem de bilgilerinizi nasıl aradığınızı geliştirmeye yardımcı olmak için yeni bir Azure AD **Denetimi günlükleri** sayfası oluşturduk. Yeni Denetim **günlükleri** sayfasını görmek için Azure AD'nin **Etkinlik** bölümünde **Denetim günlüklerini** seçin.

![Yeni Denetim günlükleri sayfası, örnek bilgi ile](media/whats-new/audit-logs-page.png)

Yeni **Denetim günlükleri** sayfası hakkında daha fazla bilgi için [Azure Etkin Dizin portalındaki Denetim etkinlik raporlarına](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs)bakın.

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Yanlışlıkla yönetici kilitlemenin yanlış yapılandırılmış Koşullu Erişim ilkelerinden engellenmesine yardımcı olacak yeni uyarılar ve kılavuzlar

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Yanlış yapılandırılmış Koşullu Erişim ilkeleri yle yöneticilerin yanlışlıkla kendilerini kendi kiracılarından kilitlemelerini önlemek için Azure portalında yeni uyarılar ve güncelleştirilmiş kılavuzlar oluşturduk. Yeni kılavuz hakkında daha fazla bilgi için Azure [Etkin Dizin Koşullu Erişim'deki hizmet bağımlılıkları nelerdir'e](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies)bakın.

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Mobil cihazlarda gelişmiş son kullanıcı kullanım koşulları

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Yönetim

Mobil cihazdaki kullanım koşullarını gözden geçirme ve kabul etme şeklinizi iyileştirmenize yardımcı olmak için mevcut kullanım koşullarımızı güncelledik. Artık yakınlaştırıp uzaklaştırabilir, geri dönebilir, bilgileri karşıdan yükleyebilir ve köprüleri seçebilirsiniz. Güncelleştirilmiş kullanım koşulları hakkında daha fazla bilgi için [Azure Etkin Dizin özelliğine](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users)bakın.

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Yeni Azure REKLAM Etkinliği günlükleri indirme deneyimi kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Artık büyük miktarda etkinlik günlüklerini doğrudan Azure portalından indirebilirsiniz. Bu güncelleştirme şunları yapmanızı sağlar:

- En fazla 250.000 satır indirin.

- İndirme tamamlandıktan sonra bilgilendirilin.

- Dosya adınızı özelleştirin.

- Çıktı biçiminizi JSON veya CSV olarak belirleyin.

Bu özellik hakkında daha fazla bilgi için [Bkz. Quickstart: Azure portalını kullanarak bir denetim raporu indirin](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report)

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Son dakika değişikliği: Exchange ActiveSync (EAS) tarafından durum değerlendirmesi güncelleştirmeleri

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Erişim Kontrolü

Exchange ActiveSync'in (EAS) aşağıdaki koşulları nasıl değerlendirdığını güncelleme aşamasındayız:

- Ülkeye, bölgeye veya IP adresine bağlı olarak kullanıcı konumu

- Oturum açma riski

- Cihaz platformu

Koşullu Erişim ilkelerinizde bu koşulları daha önce kullandıysanız, koşul davranışının değişebileceğini unutmayın. Örneğin, kullanıcı konumu koşulunu daha önce bir ilkede kullandıysanız, kullanıcınızın konumuna bağlı olarak atlanan ilkeyi bulabilirsiniz.

---

## <a name="february-2019"></a>Şubat 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Yapılandırılabilir Azure AD SAML belirteç şifrelemesi (Genel önizleme) 

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Artık desteklenen herhangi bir SAML uygulamasını şifreli SAML belirteçleri alacak şekilde yapılandırabilirsiniz. Azure AD, bir uygulamayla yapılandırıldığında ve kullanıldığında, yayılan SAML iddialarını Azure AD'de depolanan bir sertifikadan elde edilen ortak bir anahtarı kullanarak şifreler.

SAML belirteç şifrelemenizi yapılandırma hakkında daha fazla bilgi için Azure [AD SAML belirteç şifrelemesini yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption)bilgisine bakın.

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Azure AD Erişim İncelemeleri'ni kullanarak gruplar veya uygulamalar için erişim incelemesi oluşturma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Erişim İncelemeleri  
**Ürün kapasitesi:** Yönetim

Artık grup üyeliği veya uygulama ataması için tek bir Azure REKLAM erişim incelemesine birden çok grup veya uygulama ekleyebilirsiniz. Birden çok grup veya uygulama içeren erişim incelemeleri aynı ayarlar kullanılarak ayarlanır ve tüm dahil edilen gözden geçirenler aynı anda bildirilir.

Azure AD Erişim İncelemeleri'ni kullanarak erişim incelemesi oluşturma hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/governance/create-access-review) bkz.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Şubat 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Şubat 2019'da, Federasyon destekli bu 27 yeni uygulamayı uygulama galerisine ekledik:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT FINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight-CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), İzin Tıklama, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [Sismik](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), Bir [Rüya Paylaş](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [webMethods Entegrasyon Bulut](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), Bilgi Anywhere [LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU Kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope Veri](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [Netop Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io,](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial) [PureCloud By Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [ClickUp](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="enhanced-combined-mfasspr-registration"></a>Geliştirilmiş kombine MFA/SSPR kaydı

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Self Servis Şifre Sıfırlama  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Müşteri geri bildirimlerine yanıt olarak, kullanıcılarınızın güvenlik bilgilerini hem MFA hem de SSPR için daha hızlı kaydetmelerine yardımcı olarak, birleşik MFA/SSPR kayıt önizleme deneyimini artırdık. 

**Kullanıcılarınızın bugün için geliştirilmiş deneyimi açmak için aşağıdaki adımları izleyin:**

1. Genel yönetici veya kullanıcı yöneticisi olarak Azure portalında oturum açın ve **Access panel önizleme özellikleri için ayarları > Kullanıcı ayarlarını > Azure Active Directory'ye**gidin. 

2. Güvenlik **bilgilerini kaydetmek ve yönetmek için önizleme özelliklerini kullanabilen** **Kullanıcılar'da** – yenileme seçeneği, Seçili bir kullanıcı grubu veya **Tüm kullanıcılar**için özellikleri açmayı seçin.

Önümüzdeki birkaç hafta içinde, zaten açık olmayan kiracılar için eski kombine MFA / SSPR kayıt önizleme deneyimi açmak için yeteneği kaldırarak olacak.

**Denetimin kiracınız için kaldırılıp kaldırılmeyeceğini görmek için aşağıdaki adımları izleyin:**

1. Genel yönetici veya kullanıcı yöneticisi olarak Azure portalında oturum açın ve **Access panel önizleme özellikleri için ayarları > Kullanıcı ayarlarını > Azure Active Directory'ye**gidin.  

2. Güvenlik bilgileri seçeneğini **kaydetmek ve yönetmek için önizleme özelliklerini kullanabilen Kullanıcılar** **Yok**olarak ayarlanırsa, seçenek kiracınızdan kaldırılır.

Daha önce kullanıcılar için eski birleşik MFA/SSPR kayıt önizleme deneyimini açıp açmadığınızdan bağımsız olarak, eski deneyim ileribir tarihte kapatılır. Bu nedenle, en kısa zamanda yeni ve gelişmiş deneyime geçmenizi şiddetle öneririz.

Gelişmiş kayıt deneyimi hakkında daha fazla bilgi için Azure [AD birleşimi olan MFA ve parola sıfırlama kaydı deneyimindeki Cool geliştirmelerine](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271)bakın.

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Kullanıcı akışları için güncelleştirilmiş ilke yönetimi deneyimi

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Kullanıcı akışları (daha önce yerleşik ilkeler olarak bilinir) için ilke oluşturma ve yönetim sürecini daha kolay güncelledik. Bu yeni deneyim artık tüm Azure AD kiracılarınız için varsayılan değerdir.

Portal ekranının üst kısmındaki **geri bildirim** gönder alanındaki gülümseme veya kaş çatma simgelerini kullanarak ek geri bildirim ve öneriler sağlayabilirsiniz.

Yeni ilke yönetimi deneyimi hakkında daha fazla bilgi için Azure [AD B2C'nin artık JavaScript özelleştirmesi ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahip olmasına bakın.

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Azure AD B2C tarafından sağlanan belirli sayfa öğesi sürümlerini seçin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Artık Azure AD B2C tarafından sağlanan sayfa öğelerinin belirli bir sürümünü seçebilirsiniz. Belirli bir sürümü seçerek, güncelleştirmelerinizi bir sayfada görünmeden önce sınayabilir ve öngörülebilir davranışlar elde edebilirsiniz. Ayrıca, javascript özelleştirmelerine izin vermek için belirli sayfa sürümlerini zorlamayı da tercih edebilirsiniz. Bu özelliği açmak için, kullanıcı akışlarınızdaki **Özellikler** sayfasına gidin.

Sayfa öğelerinin belirli sürümlerini seçme hakkında daha fazla bilgi için, [Azure AD B2C'nin artık JavaScript özelleştirmesi ve daha birçok yeni özellik](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) bloguna sahip olmasına bakın.

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>B2C (GA) için yapılandırılabilir son kullanıcı parola gereksinimleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Artık, yerel Azure AD parola ilkenizi kullanmak zorunda kalmak yerine son kullanıcılarınız için kuruluşunuzun parola karmaşıklığını ayarlayabilirsiniz. Kullanıcı akışlarınızın **Özellikleri** bıtır(önceden yerleşik ilkeleriniz olarak bilinir), **Basit** veya **Güçlü'nün**parola karmaşıklığını seçebilir veya **Özel** bir gereksinim ler kümesi oluşturabilirsiniz.

Parola karmaşıklığı gereksinimi yapılandırması hakkında daha fazla bilgi için Azure [Etkin Dizin B2C'deki parolalar için karmaşıklık gereksinimlerini yapılandırma'ya](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)bakın.

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Özel markalı kimlik doğrulama deneyimleri için yeni varsayılan şablonlar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C

Kullanıcılarınız için özel markalı bir kimlik doğrulama deneyimi oluşturmak için kullanıcı akışlarınızın (önceden yerleşik ilkeler olarak bilinir) **Sayfa düzenleri** bıçak üzerinde bulunan yeni varsayılan şablonlarımızı kullanabilirsiniz.

Şablonları kullanma hakkında daha fazla bilgi için azure [AD B2C'nin artık JavaScript özelleştirmesi ve daha birçok yeni özelliğe sahip olduğunu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595)görün.

---

## <a name="january-2019"></a>Ocak 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Tek seferlik parola kimlik doğrulaması kullanarak Etkin Dizin B2B işbirliği (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C

Azure AD, Microsoft hesabı (MSA) veya Google federasyonu gibi diğer yollarla kimlik doğrulaması verilmeyen B2B konuk kullanıcıları için tek seferlik parola kimlik doğrulaması (OTP) başlattık. Bu yeni kimlik doğrulama yöntemi, konuk kullanıcıların yeni bir Microsoft hesabı oluşturması gerekmez anlamına gelir. Bunun yerine, bir daveti kullanmak veya paylaşılan bir kaynağa erişirken, konuk kullanıcı bir e-posta adresine gönderilmesi için geçici bir kod isteyebilir. Bu geçici kodu kullanarak, konuk kullanıcı oturum açmaya devam edebilir.

Daha fazla bilgi için, [bkz.](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) [Azure AD makes sharing and collaboration seamless for any user with any account](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949)

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Yeni Azure AD Uygulama Proxy çerez ayarları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Uygulama Proxy aracılığıyla yayınlanan uygulamalarınız için kullanılabilen üç yeni çerez ayarını kullanıma sunduk:

- **Yalnızca HTTP çerezini kullanın.** Uygulama Proxy erişiminizde ve oturum tanımlama bilgilerinizde **HTTPOnly** bayrağını ayarlar. Bu ayarı açmak, istemci tarafındaki komut dosyası oluşturma yoluyla tanımlama bilgilerinin kopyalanmasını veya değiştirilmesini önlemeye yardımcı olmak gibi ek güvenlik avantajları sağlar. Ek avantajlar için bu bayrağı açmanızı **(Evet'i**seçin).

- **Güvenli çerez kullanın.** Uygulama Proxy erişiminizde ve oturum tanımlama bilgilerinizde **Güvenli** bayrağı ayarlar. Bu ayarı açmak, tanımlama bilgilerinin yalnızca HTTPS gibi TLS güvenli kanallar üzerinden iletilmesini sağlayarak ek güvenlik avantajları sağlar. Ek avantajlar için bu bayrağı açmanızı **(Evet'i**seçin).

- **Kalıcı çerez kullanın.** Web tarayıcısı kapatıldığında çerezlere erişimin sona ermesini önler. Bu çerezler erişim belirteci ömür boyu sürer. Ancak, son kullanma süresine ulaşılırsa veya kullanıcı çerezi el ile silerse çerezler sıfırlanır. Varsayılan ayar **No'yu**tutmanızı öneririz, yalnızca işlemler arasında tanımlama bilgilerini paylaşmayan eski uygulamalar için ayarı açmanızı öneririz.

Yeni tanımlama bilgileri hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki şirket içi uygulamalara erişmek için Çerez ayarlarına](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Ocak 2019

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Ocak 2019'da, Federasyon desteğine sahip bu 35 yeni uygulamayı uygulama galerisine ekledik:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Yetenek Paleti](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco Umbrella](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet Access Administrator](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), Sona [Erme Hatırlatma](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Görüntüleyici](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [Fiil](https://app.verb.net/login), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Pavaso Dijital Close](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [Bulut Hizmeti PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [Denetim Kurulu](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [İş Able](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO Sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [KURUMSAL ARES](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [Office 365 için K2](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Ferry ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas Platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Yeni Azure AD Kimlik Koruması geliştirmeleri (Genel önizleme)

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Aşağıdakiler de dahil olmak üzere Azure AD Kimlik Koruması genel önizleme teklifine aşağıdaki geliştirmeleri eklediğimizi duyurmaktan heyecan duyuyoruz:

- Güncelleştirilmiş ve daha entegre kullanıcı arabirimi

- Ek API’ler

- Makine öğrenimi yoluyla geliştirilmiş risk değerlendirmesi

- Riskli kullanıcılar ve riskli oturum açma lar arasında ürün genelinde uyum

Geliştirmeler hakkında daha fazla bilgi için Azure [Active Directory Identity Protection (yenilenmiş) nedir?](https://aka.ms/IdentityProtectionDocs) daha fazla bilgi edinmek ve ürün içi istemler aracılığıyla düşüncelerinizi paylaşmak için.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>iOS ve Android cihazlardaki Microsoft Authenticator uygulaması için yeni Uygulama Kilidi özelliği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Microsoft Authenticator Uygulaması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Tek seferlik parolalarınızı, uygulama bilgilerinizi ve uygulama ayarlarınızı daha güvenli tutmak için Microsoft Authenticator uygulamasındaApp Lock özelliğini açabilirsiniz. Uygulama Kilidi'ni açmak, Microsoft Authenticator uygulamasını her açtığınızda PIN veya biyometrik kullanarak kimliğinizin doğrulanacağınız anlamına gelir.

Daha fazla bilgi için [Microsoft Authenticator uygulaması SSS'ye](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq)bakın.

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Gelişmiş Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) dışa aktarma özellikleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi

Ayrıcalıklı Kimlik Yönetimi (PIM) yöneticileri artık tüm alt kaynaklar için rol atamaları içeren belirli bir kaynak için tüm etkin ve uygun rol atamalarını dışa aktarabilir. Daha önce, yöneticilerin bir abonelik için rol atamalarının tam listesini almak zordu ve her bir kaynak için rol atamaları dışa aktarmak zorundaydılar.

Daha fazla bilgi için, [PIM'deki Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüle'ye](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)bakın.

---

## <a name="novemberdecember-2018"></a>Kasım/Aralık 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Eşitleme kapsamından kaldırılan kullanıcılar artık yalnızca bulut hesaplarına geçmiyor

**Türü:** Sabit  
**Hizmet kategorisi:** Kullanıcı Yönetimi  
**Ürün kapasitesi:** Dizin

>[!Important]
>Biz duydum ve bu düzeltme nedeniyle hayal kırıklığı anlıyorum. Bu nedenle, düzeltmeyi kuruluşunuzda uygulamanızı kolaylaştıracak kadar bu değişikliği geri aldık.

Active Directory Domain Services (AD DS) nesnesi eşitleme kapsamı dışında kaldığında ve ardından aşağıdaki eşitleme döngüsünde Azure AD'de Geri Dönüşüm Kutusu'na taşındığında, bir kullanıcının DirSyncEnabled bayrağının hatalı bir şekilde **False'a** geçeceği bir hata düzelttik. Bu düzeltmenin bir sonucu olarak, kullanıcı eşitleme kapsamı dışında tutulur ve daha sonra Azure AD Geri Dönüşüm Kutusu'ndan geri yüklenirse, kullanıcı hesabı beklendiği gibi şirket içi AD'den senkronize olarak kalır ve yetki kaynağı (SoA) şirket içi AD olarak kaldığından bulutta yönetilemez.

Bu düzeltmeden önce, DirSyncEnabled bayrağı False'a geçtiğinde bir sorun vardı. Bu, bu hesapların yalnızca bulut nesnelerine dönüştürüldüğü ve hesapların bulutta yönetilebileceği izlenimini verdi. Ancak, hesaplar hala şirket içi olarak SoA ve şirket içi AD gelen tüm senkronize özellikleri (gölge öznitelikleri) olarak korudu. Bu durum, Azure AD'de ve diğer bulut iş yüklerinde (Exchange Online gibi) birden çok soruna neden olarak bu hesapları AD'den senkronize edilmiş olarak ele alabildi, ancak artık yalnızca bulut hesapları gibi davranıyordu.

Şu anda, ad'dan gelen senkronize bir hesabı yalnızca bulut hesabına dönüştürmenin tek yolu, Kiralayan düzeyinde DirSync'i devre dışı bırakmaktır ve bu da SoA'yı aktarmak için bir arka uç işlemini tetikler. Bu tür Bir SoA değişikliği, şirket içi ilgili tüm öznitelikleri (LastDirSyncTime ve gölge öznitelikleri gibi) temizlemeyi ve ilgili nesnenin yalnızca bulut hesabına dönüştürülmesi için diğer bulut iş yüklerine sinyal göndermeyi gerektirir (ancak bunlarla sınırlı değildir).

Bu düzeltme, geçmişte bazı senaryolarda gerekli olan AD'den eşitlenmiş bir kullanıcının Değişmez ID özniteliği yle ilgili doğrudan güncelleştirmeleri önler. Tasarım gereği, Azure AD'deki bir nesnenin Değişmez Kimliği, adından da anlaşılacağı gibi, değişmez olması gerekir. Azure AD Connect Health ve Azure AD Connect Synchronization istemcisinde uygulanan yeni özellikler bu tür senaryoları ele almak için kullanılabilir:

- **Aşamalı bir yaklaşımla birçok kullanıcı için büyük ölçekli ImmutableID güncelleştirmesi**
  
  Örneğin, uzun bir AD DS ormanlar arası geçiş yapmanız gerekir. Çözüm: **Kaynak Çapayı Yapılandırmak** için Azure AD Connect'i kullanın ve kullanıcı geçiş yaptıkça, mevcut Değişmez ID değerlerini Azure AD'den yerel AD DS kullanıcısının ms-DS Tutarlılığı-Guid özelliğine kopyalayın. Daha fazla bilgi için bkz: [ms-DS-ConsistencyGuid'i kaynak Olarak KullanmaAnchor.](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor)

- **Tek çekimde birçok kullanıcı için büyük ölçekli ImmutableID güncelleştirmeleri**

  Örneğin, Azure AD Connect'i uygularken bir hata yaparsınız ve şimdi SourceAnchor özniteliğini değiştirmeniz gerekir. Çözüm: Kiracı düzeyinde DirSync'i devre dışı bırakıp tüm geçersiz ImmutableID değerlerini temizleyin. Daha fazla bilgi için Bkz. [Office 365 için dizin eşitlemesi kapatın.](/office365/enterprise/turn-off-directory-synchronization)

- **Azure AD'de mevcut bir kullanıcıyla şirket içi kullanıcıyı yeniden eşleştirme** Örneğin, AD DS'de yeniden oluşturulan bir kullanıcı, onu varolan bir Azure AD hesabıyla (eski nesne) yeniden eşleştirmek yerine Azure AD hesabında bir yinelenen oluşturur. Çözüm: Kaynak Çapa/Değişmez Kimliği yeniden eşlemek için Azure portalında Azure AD Bağlantı Durumu'nu kullanın. Daha fazla bilgi için [Bkz. Orphaned nesne senaryosu.](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario)

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Son İşlem: Azure Monitor üzerinden denetim ve oturum açma günlüklerinde güncelleştirmeler şema

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Şu anda Azure Monitor üzerinden hem Denetim hem de Oturum Açma günlük akışlarını yayınlıyoruz, böylece günlük dosyalarını SIEM araçlarınızla veya Log Analytics ile sorunsuz bir şekilde entegre edebilirsiniz. Geri bildirimlerinize dayanarak ve bu özelliğin genel kullanılabilirlik duyurusuna hazırlık olarak şemamızda aşağıdaki değişiklikleri yapıyoruz. Bu şema değişiklikleri ve ilgili belge güncellemeleri Ocak ayının ilk haftasında gerçekleşecek.

#### <a name="new-fields-in-the-audit-schema"></a>Denetim şemasında yeni alanlar
Kaynak üzerinde gerçekleştirilen işlem türünü sağlamak için yeni bir **İşlem Türü** alanı ekliyoruz. Örneğin, **Ekle,** **Güncelleştir**veya **Sil**.

#### <a name="changed-fields-in-the-audit-schema"></a>Denetim şemasında değişen alanlar
Denetim şemasında aşağıdaki alanlar değişiyor:

|Alan adı|Ne değişti|Eski değerler|Yeni Değerler|
|----------|------------|----------|----------|
|Kategori|Burası **Hizmet Adı** alanıydı. Artık **Denetim Kategorileri** alanı. **Hizmet Adı,** **günlüğe kaydedilmişByService** alanına yeniden adlandırılmıştır.|<ul><li>Hesap Sağlama</li><li>Çekirdek Dizin</li><li>Self servis Şifre Sıfırlama</li></ul>|<ul><li>Kullanıcı Yönetimi</li><li>Grup Yönetimi</li><li>Uygulama Yönetimi</li></ul>|
|hedefKaynaklar|En üst düzeyde **TargetResourceType** içerir.|&nbsp;|<ul><li>İlke</li><li>Uygulama</li><li>Kullanıcı</li><li>Grup</li></ul>|
|loggedByService|Denetim günlüğünü oluşturan hizmetin adını sağlar.|Null|<ul><li>Hesap Sağlama</li><li>Çekirdek Dizin</li><li>Self servis parola sıfırlama</li></ul>|
|Sonuç|Denetim günlüklerinin sonucunu sağlar. Daha önce, bu numaralandırılmış, ancak şimdi gerçek değeri gösterir.|<ul><li>0</li><li>1</li></ul>|<ul><li>Başarılı</li><li>Hata</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>Oturum açma şemasında değişen alanlar
Oturum açma şemasında aşağıdaki alanlar değişiyor:

|Alan adı|Ne değişti|Eski değerler|Yeni Değerler|
|----------|------------|----------|----------|
|uygulamalıKoşulalErişim İlkeleri|Bu **koşullu erişim İlkeleri** alanı oldu. Şimdi **uygulamalı KoşulluErişim İlkeleri** alanı.|düzeltme sınıfı,|düzeltme sınıfı,|
|koşullu Erişim Durumu|Oturum açma sırasında Koşullu Erişim İlkesi Durumunun sonucunu sağlar. Daha önce, bu numaralandırılmış, ancak şimdi gerçek değeri gösterir.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|
|appliedConditionalAccessPolicies: sonuç|Oturum açma sırasında tek tek Koşullu Erişim İlkesi Durumunun sonucunu sağlar. Daha önce, bu numaralandırılmış, ancak şimdi gerçek değeri gösterir.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Başarılı</li><li>Hata</li><li>Uygulanmadı</li><li>Devre dışı</li></ul>|

Şema hakkında daha fazla bilgi için azure [denetimi günlüklerini Azure Monitor'da yorumla (önizleme)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Denetlenen makine öğrenme modeli ve risk puanı motoruiçin Kimlik Koruma iyileştirmeleri

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Risk Puanları

Kimlik Koruması ile ilgili kullanıcı ve oturum açma risk değerlendirme motorundaki iyileştirmeler, kullanıcı risk doğruluğunu ve kapsamını geliştirmeye yardımcı olabilir. Yöneticiler, kullanıcı risk düzeyinin artık belirli algılamaların risk düzeyiyle doğrudan bağlantılı olmadığını ve riskli oturum açma olaylarının sayısında ve düzeyinde bir artış olduğunu fark edebilirler.

Risk algılamaları artık kullanıcının oturum açma ek özellikleri ve algılama lar bir model kullanılarak kullanıcı riskini hesaplar denetlenen makine öğrenme modeli tarafından değerlendirilir. Bu modele göre, yönetici, bu kullanıcıyla ilişkili algılamalar düşük veya orta riskli olsa bile, yüksek risk puanlarına sahip kullanıcıları bulabilir. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Yöneticiler Microsoft Authenticator uygulamasını kullanarak kendi parolalarını sıfırlayabilirler (Genel önizleme)

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Self Servis Şifre Sıfırlama  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD yöneticileri artık Microsoft Authenticator uygulama bildirimlerini veya herhangi bir mobil kimlik doğrulayıcı uygulamasından veya donanım belirtecinden bir kodu kullanarak kendi parolalarını sıfırlayabilir. Yöneticiler kendi parolalarını sıfırlamak için artık aşağıdaki yöntemlerden ikisini kullanabilecekler:

- Microsoft Authenticator uygulaması bildirimi

- Diğer mobil kimlik doğrulama uygulaması / Donanım belirteç kodu

- Email

- Telefon görüşmesi

- Kısa mesaj

Parolaları sıfırlamak için Microsoft Authenticator uygulamasını kullanma hakkında daha fazla bilgi için [Azure AD self servis parola sıfırlama - Mobil uygulama ve SSPR (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Yeni Azure AD Bulut Aygıt Yöneticisi rolü (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Cihaz Kayıt ve Yönetimi  
**Ürün kapasitesi:** Erişim denetimi

Yöneticiler, bulut aygıt yöneticisi görevlerini gerçekleştirmek için kullanıcıları yeni Bulut Aygıt Yöneticisi rolüne atayabilir. Bulut Aygıt Yöneticileri rolüne atanan kullanıcılar, Azure AD'deki aygıtları etkinleştirebilir, devre dışı edebilir ve silebilir ve Azure portalında Windows 10 BitLocker anahtarlarını (varsa) okuyabilir.

Roller ve izinler hakkında daha fazla bilgi için Azure [Etkin Dizini'nde yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Azure AD'deki yeni etkinlik zaman damgasını kullanarak cihazlarınızı yönetin (Genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Cihaz Kayıt ve Yönetimi  
**Ürün kapasitesi:** Cihaz Yaşam Döngüsü Yönetimi

Ortamınızda eski aygıtların olmaması için zaman içinde kuruluşlarınizin aygıtlarını Azure AD'de yenilemeniz ve emekli yedirmeniz gerektiğinin farkındayız. Azure AD, bu işleme yardımcı olmak için artık cihazlarınızı yeni bir etkinlik zaman damgasıyla günceller ve cihazınızın yaşam döngüsünü yönetmenize yardımcı olur.

Bu zaman damgasını nasıl alabilirim ve kullanacağız hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Yöneticiler, kullanıcıların her cihazda kullanım koşullarını kabul etmesini isteyebilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Yönetim
 
Yöneticiler **artık, kullanıcılarınızın** kiracınızda kullandıkları her cihazda kullanım koşullarınızı kabul etmesini gerektirecek her cihaz seçeneğini kabul etmesini talep etmek için kullanıcıları nızı açabilir.

Daha fazla bilgi için Azure [Etkin Dizin özelliğinin cihaz başına kullanım koşulları bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Yöneticiler, yinelenen bir zamanlamaya bağlı olarak kullanım koşullarını süresi dolacak şekilde yapılandırabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Yönetim
 

Yöneticiler artık, belirtilen yinelenen zamanlamanıza bağlı olarak tüm kullanıcılarınız için kullanım süresinin dolmasını sağlamak için **Kullanım Süresi Dolması** seçeneğini açabilir. Zamanlama yıllık, iki yılda, üç aylık veya aylık olabilir. Kullanım koşullarının süresi dolduktan sonra, kullanıcılar yeniden kabul etmelidir.

Daha fazla bilgi için Azure [Etkin Dizin koşulları özelliğinin kullanım koşullarını ekle bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Yöneticiler, her kullanıcının zamanlamasına bağlı olarak kullanım koşullarını sona erecek şekilde yapılandırabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Yönetim

Yöneticiler artık kullanıcının kullanım koşullarını yeniden kabul etmesi gereken bir süre belirtebilir. Örneğin, yöneticiler kullanıcıların her 90 günde bir kullanım koşullarını yeniden kabul etmesi gerektiğini belirtebilir.

Daha fazla bilgi için Azure [Etkin Dizin koşulları özelliğinin kullanım koşullarını ekle bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)bakın.
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Azure Etkin Dizin rolleri için yeni Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) e-postaları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi
 
Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) kullanan müşteriler artık son yedi gün için aşağıdaki bilgileri içeren haftalık özet e-posta alabilirler:

- En uygun ve kalıcı rol atamalarına genel bakış

- Rolleri etkinleştiren kullanıcı sayısı

- PIM'deki rollere atanan kullanıcı sayısı

- PIM dışındaki rollere atanan kullanıcı sayısı

- PIM'de "kalıcı hale getirilen" kullanıcı sayısı

PIM ve mevcut e-posta bildirimleri hakkında daha fazla bilgi için [PIM'deki E-posta bildirimlerine](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications)bakın.

---

### <a name="group-based-licensing-is-now-generally-available"></a>Grup tabanlı lisanslama artık genel olarak kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Dizin

Grup tabanlı lisanslama genel önizleme dışındadır ve artık genel olarak kullanılabilir. Bu genel sürümün bir parçası olarak, bu özelliği daha ölçeklenebilir hale getirdik ve tek bir kullanıcı için grup tabanlı lisans atamalarını yeniden işleme ve Office 365 E3/A3 lisanslarıyla grup tabanlı lisanslama yı kullanabilme özelliğini ekledik.

Grup tabanlı lisanslama hakkında daha fazla bilgi için Azure [Active Directory'de grup tabanlı lisanslama nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Kasım 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Kasım 2018'de, Federasyon desteğine sahip 26 yeni uygulamayı uygulama galerisine ekledik:

[CoreStack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [Gra-Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [eHour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [DriveDollar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview), [Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [Sonsuz Kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [Alaya](https://alayagood.com/en/demo/), [HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy Business Central 365 için](https://accounting.zenegy.com/), [Everbridge Üye Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [Plex Apps - Klasik Test](https://test.plexonline.com/signon), Plex Apps [- Klasik](https://www.plexonline.com/signon), Plex Apps - [UX Testi](https://test.cloud.plex.com/sso), Plex Apps - UX , Plex Apps - [UX](https://cloud.plex.com/sso), [IAM](https://accounts.plex.com/), [CRAFTS - Çocuk Bakımı Kayıtları, Katılım, & Finansal Takip Sistemi](https://getcrafts.ca/craftsregistration) 

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

## <a name="october-2018"></a>Ekim 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure REKLAM Günlükleri artık Azure Günlük Analitiği (Genel önizleme) ile çalışır

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Azure REKLAM günlüklerinizi Azure Log Analytics'e iletebileceğinizi duyurmaktan heyecan duyuyoruz! En çok istenen özellik, işletmeniz, operasyonlarınız ve güvenliğiniz için analizlere daha iyi erişim inyanık üzere altyapınızı izlemenize yardımcı olur. Daha fazla bilgi için Azure [Günlük Analizi'ndeki Azure Etkin Dizin Etkinliği günlüklerine](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Ekim 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Ekim 2018'de, Federasyon desteğine sahip bu 14 yeni uygulamayı uygulama galerisine ekledik:

[Ödül Puanlarım](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), Dialpad, [ON24 Sanal Çevre](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler Üç](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot Control](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Etki Alanı Hizmetleri E-posta Bildirimleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD Etki Alanı Hizmetleri  
**Ürün kapasitesi:** Azure AD Etki Alanı Hizmetleri

Azure AD Etki Alanı Hizmetleri, Azure portalında yanlış yapılandırmalar veya yönetilen etki alanınızla ilgili sorunlar hakkında uyarılar sağlar. Bu uyarılar, desteğe başvurmak zorunda kalmadan sorunları gidermeye çalışabilmeniz için adım adım kılavuzlar içerir.

Ekim ayından itibaren, yönetilen etki alanınızın bildirim ayarlarını özelleştirebilirsiniz, böylece yeni uyarılar oluştuğunda, belirlenmiş bir grup kişiye bir e-posta gönderilir ve bu da portalın sürekli olarak güncelleştirmeleri denetleme ihtiyacını ortadan kaldırır.

Daha fazla bilgi için [Azure AD Etki Alanı Hizmetleri'ndeki Bildirim ayarlarına](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications)bakın.

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD portalı özel etki alanlarını silmek için ForceDelete etki alanı API'sini kullanmayı destekler 

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Dizin Yönetimi  
**Ürün kapasitesi:** Dizin

ForceDelete etki alanı API'sini kullanarak özel alan adlarınızı, kullanıcılar, gruplar ve uygulamalar gibi özel alan adınızdan (contoso.com) ilk varsayılan alan adı (contoso.onmicrosoft.com) gibi uygulamaları senkronize bir şekilde yeniden adlandırarak silebileceğinizi duyurmaktan mutluluk duyarız.

Bu değişiklik, kuruluşunuz artık adı kullanmıyorsa veya etki alanı adını başka bir Azure REKLAMıyla kullanmanız gerekiyorsa, özel alan adlarınızı daha hızlı silmenize yardımcı olur.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name)

---

## <a name="september-2018"></a>Eylül 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Dinamik gruplar için güncelleştirilmiş yönetici rol izinleri

**Türü:** Sabit  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Belirli yönetici rollerinin artık grubun sahibi olmanıza gerek kalmadan dinamik üyelik kuralları oluşturup güncelleştirebilmeleri için bir sorunu düzelttin.

Roller şunlardır:

- Genel yönetici

- Intune yöneticisi

- Kullanıcı yöneticisi

Daha fazla bilgi için bkz. [dinamik bir grup oluştur ve durumu kontrol et](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Bazı üçüncü taraf uygulamalar için basitleştirilmiş Tek Oturum Açma (SSO) yapılandırma ayarları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Hizmet Olarak Yazılım (SaaS) uygulamaları için Tek Oturum Açma (SSO) oluşturmanın, her uygulama yapılandırmasının benzersiz doğası nedeniyle zor olabileceğinin farkındayız. Aşağıdaki üçüncü taraf SaaS uygulamaları için SSO yapılandırma ayarlarını otomatik olarak doldurmak için basitleştirilmiş bir yapılandırma deneyimi kurduk:

- Zendesk

- ArcGis Online

- Jamf Pro

Bu tek tıklamayla deneyimi kullanmaya başlamak için uygulama için **Azure portalı** > **SSO yapılandırma** sayfasına gidin. Daha fazla bilgi için Azure [Active Directory ile SaaS uygulama tümleştirmesi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Azure Active Directory - Verileriniz nerede bulunur? Sayfası

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** GoLocal

Azure Etkin Dizini'nden şirketinizin bölgesini seçin - Hangi Azure veri merkezinin Azure REKLAM verilerinizi tüm Azure REKLAM hizmetleri için hazır olarak barındıracağını görüntülemek için **veri nizin bulunduğu** sayfa nerededir. Bilgileri şirketinizin bölgesiiçin belirli Azure REKLAM hizmetlerine göre filtreleyebilirsiniz.

Bu özelliğe erişmek ve daha fazla bilgi için [Azure Etkin Dizini - Verileriniz nerede bulunur.](https://aka.ms/AADDataMap)

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Uygulamalarım Erişimi paneli için yeni dağıtım planı kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Sso

Uygulamalarım Erişim paneli için kullanılabilen yeni dağıtım planına göz atın (https://aka.ms/deploymentplans).
Uygulamalarım Erişim paneli, kullanıcılara uygulamalarını bulup erişebilmek için tek bir yer sağlar. Bu portal, kullanıcılara uygulamalara ve gruplara erişim istemek veya bu kaynaklara başkaları adına erişimi yönetmek gibi self servis fırsatları da sağlar.

Daha fazla bilgi için [Uygulamalarım portalınedir?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Azure portalının Oturum Açma Günlükleri sayfasında yeni Sorun Giderme ve Destek sekmesi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Azure portalının Oturum Açma **lar** sayfasındaki yeni **Sorun Giderme ve Destek** sekmesi, yöneticilerin ve mühendislerin Azure AD oturum açmasorunlarıyla ilgili sorunları gidermelerine yardımcı olmayı amaçlamaktadır. Bu yeni sekme, sorunu çözmeye yardımcı olmak için hata kodu, hata iletisi ve düzeltme önerileri (varsa) sağlar. Sorunu çözemiyorsanız, destek biletinizdeki günlük dosyası için **İstek Kimliği** ve **Tarih (UTC)** alanlarını dolduran **Pano deneyimine** Kopyala'yı kullanarak destek bileti oluşturmanız için yeni bir yol da sayılabiliriz.  

![Yeni sekmeyi gösteren oturum açma günlükleri](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Dinamik üyelik kuralları oluşturmak için kullanılan özel uzantı özellikleri için geliştirilmiş destek

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği

Bu güncelleştirmeyle, artık dinamik kullanıcı grubu kural oluşturucusundan **Özel Ekspertiz Özellikleri Al** bağlantısını tıklatabilir, benzersiz uygulama kimliğinizi girebilir ve kullanıcılar için dinamik bir üyelik kuralı oluştururken kullanılacak özel uzantı özelliklerinin tam listesini alabilirsiniz. Bu liste, bu uygulama için yeni özel uzantı özellikleri almak için de yenilenebilir.

Dinamik üyelik kuralları için özel uzantı özelliklerini kullanma hakkında daha fazla bilgi için [Bkz. Uzantın özellikleri ve özel uzantı özellikleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı Koşullu Erişim için yeni onaylı istemci uygulamaları

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma

Aşağıdaki uygulamalar onaylı istemci [uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesindeyer almaktadır:

- Microsoft To-Do

- Microsoft Stream

Daha fazla bilgi için bkz.

- [Azure AD uygulama tabanlı Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Windows 7/8/8.1 Kilit ekranından Self Servis Parola Sıfırlama için yeni destek

**Türü:** Yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Bu yeni özelliği ayarladıktan sonra, kullanıcılarınız Windows 7, Windows 8 veya Windows 8.1 çalıştıran bir aygıtın **Kilit** ekranından parolalarını sıfırlamak için bir bağlantı görür. Bu bağlantıyı tıklatarak, kullanıcı web tarayıcısı üzerinden aynı parola sıfırlama akışı üzerinden yönlendirilir.

Daha fazla bilgi için [Windows 7, 8 ve 8.1'den parola sıfırlamayı nasıl etkinleştirin](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: Yetkilendirme kodları artık yeniden kullanılamaz 

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD, 15 Kasım 2018'den itibaren uygulamalar için daha önce kullanılmış kimlik doğrulama kodlarını kabul etmeyi durdurur. Bu güvenlik değişikliği, Azure AD'nin OAuth belirtimine uygun hale getirilmesine yardımcı olur ve hem v1 hem de v2 uç noktalarında uygulanır.

Uygulamanız birden çok kaynak için belirteçler almak için yetkilendirme kodlarını yeniden kullanıyorsa, yenileme jetonu almak için kodu kullanmanızı ve ardından diğer kaynaklar için ek belirteçler elde etmek için bu yenileme belirteci'ni kullanmanızı öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden çok kaynak ta birden çok kez kullanılabilir. OAuth kod akışı sırasında kimlik doğrulama kodunu yeniden kullanmaya çalışan bir uygulama invalid_grant bir hata alır.

Bu ve protokollerle ilgili diğer değişiklikler [için kimlik doğrulama için yeniliklerin tam listesine](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Eylül 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Eylül 2018'de, Federasyon desteğine sahip bu 16 yeni uygulamayı uygulama galerisine ekledik:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet Recruiting Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud , [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass , [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Enlyft SSO Azure, SurveyMonkey, [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="support-for-additional-claims-transformations-methods"></a>Ek talep dönüşümleri yöntemleri için destek

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

SAML tabanlı **Tek Oturum Açma Yapılandırması** sayfasından SAML belirteçlerine uygulanabilen yeni talep dönüştürme yöntemlerini, ToLower() ve ToUpper(), tanıttık.

Daha fazla bilgi için, [Azure AD'deki kurumsal uygulamalar için SAML belirtecinde verilen talepleri nasıl özelleştirin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Güncelleştirilmiş SAML tabanlı uygulama yapılandırması UI (önizleme)

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Güncelleştirilmiş SAML tabanlı uygulama yapılandırma sıyrıklarımızın bir parçası olarak şunları elde eedeceksiniz:

- SAML tabanlı uygulamalarınızı yapılandırmak için güncelleştirilmiş bir gözden geçirme deneyimi.

- Yapılandırmanızda neyin eksik veya yanlış olduğu hakkında daha fazla görünürlük.

- Son kullanma sertifikası bildirimi için birden çok e-posta adresi ekleme özelliği.

- Yeni talep dönüşüm yöntemleri, ToLower() ve ToUpper(), ve daha fazlası.

- Kurumsal uygulamalarınız için kendi belirteç imzalama sertifikanızı yüklemenin bir yolu.

- SAML uygulamaları için NameID Biçimini ayarlamanın ve NameID değerini Dizin Uzantıları olarak ayarlamanın bir yolu.

Bu güncelleştirilmiş görünümü açmak **için, Tek Oturum Aç** sayfasının üst kısmından **yeni deneyim bağlantımızı deneyin** bağlantısını tıklatın. Daha fazla bilgi için [Bkz. Öğretici: Azure Active Directory'li bir uygulama için SAML tabanlı tek oturum açma'yı yapılandırın.](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications)

---

## <a name="august-2018"></a>Ağustos 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP adres aralıklarında yapılan değişiklikler

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Platform

Azure AD'ye daha büyük IP aralıkları sıyoruz, bu da güvenlik duvarlarınız, yönlendiricileriniz veya Ağ Güvenlik Gruplarınız için Azure AD IP adres aralıklarını yapılandırmışsanız bunları güncellemeniz gerektiği anlamına gelir. Azure AD yeni uç noktaları eklediğinde güvenlik duvarınızı, yönlendiricinizi veya Ağ Güvenlik Grupları IP aralığı yapılandırmalarınızı yeniden değiştirmek zorunda kalmamanız için bu güncelleştirmeyi yapıyoruz. 

Ağ trafiği önümüzdeki iki ay içinde bu yeni aralıklara taşınıyor. Kesintisiz hizmete devam etmek için, 10 Eylül 2018'den önce ip adreslerinize bu güncelleştirilmiş değerleri eklemeniz gerekir:

- 20.190.128.0/18 

- 40.126.0.0/18 

Tüm ağ trafiğiniz yeni aralıklara taşınana kadar eski IP Adresi aralıklarını kaldırmamanızı öneririz. Taşıma yla ilgili güncelleştirmeler ve eski aralıkları ne zaman kaldırabileceğinizi öğrenmek için [Bkz. Office 365 URL'leri ve IP adresi aralıkları.](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: Yetkilendirme kodları artık yeniden kullanılamaz 

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Azure AD, 15 Kasım 2018'den itibaren uygulamalar için daha önce kullanılmış kimlik doğrulama kodlarını kabul etmeyi durdurur. Bu güvenlik değişikliği, Azure AD'nin OAuth belirtimine uygun hale getirilmesine yardımcı olur ve hem v1 hem de v2 uç noktalarında uygulanır.

Uygulamanız birden çok kaynak için belirteçler almak için yetkilendirme kodlarını yeniden kullanıyorsa, yenileme jetonu almak için kodu kullanmanızı ve ardından diğer kaynaklar için ek belirteçler elde etmek için bu yenileme belirteci'ni kullanmanızı öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden çok kaynak ta birden çok kez kullanılabilir. OAuth kod akışı sırasında kimlik doğrulama kodunu yeniden kullanmaya çalışan bir uygulama invalid_grant bir hata alır.

Bu ve protokollerle ilgili diğer değişiklikler [için kimlik doğrulama için yeniliklerin tam listesine](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Self servis parolası (SSPR) ve Çok Faktörlü Kimlik Doğrulama (MFA) için yakınsak güvenlik bilgi yönetimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Bu yeni özellik, insanların SSPR ve MFA için güvenlik bilgilerini (telefon numarası, mobil uygulama vb. gibi) tek bir konum ve deneyimde yönetmelerine yardımcı olur; daha önce, iki farklı yerlerde yapıldı göre.

Bu yakınsama deneyimi aynı zamanda SSPR veya MFA kullanan kişiler için de çalışır. Ayrıca, kuruluşunuz MFA veya SSPR kaydını uygulamazsa, kişiler yine de Kuruluşunuz tarafından izin verilen MFA veya SSPR güvenlik bilgileri yöntemlerini Uygulamalarım portalından kaydedebilir.

Bu bir opt-in genel önizlemesidir. Yöneticiler, seçili bir grup veya kiracıdaki tüm kullanıcılar için yeni deneyimi (istenirse) açabilir. Yakınsanan deneyim hakkında daha fazla bilgi [için, Yakınsama deneyimi bloguna](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/) bakın

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD Uygulaması proxy uygulamalarında yalnızca yeni HTTP tanımlama bilgileri ayarı

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Uygulama Proxy uygulamalarınızda **yalnızca HTTP-Yalnızca Çerezler** adlı yeni bir ayar vardır. Bu ayar, hem Uygulama Proxy erişimi hem de oturum tanımlama bilgileri için HTTP yanıt üstbilgisine HTTPOnly bayrağını ekleyerek, istemci tarafındaki komut dosyasından çerezerişimi durdurarak ve çerezi kopyalama veya değiştirme gibi eylemleri daha da engelleyerek ekstra güvenlik sağlamaya yardımcı olur. Bu bayrak daha önce kullanılmamış olsa da, yanlış değişikliklere karşı korunmaya yardımcı olmak için çerezleriniz her zaman bir TLS bağlantısı kullanılarak şifrelenmiş ve iletilmiştir.

Bu ayar, Uzak Masaüstü gibi ActiveX denetimlerini kullanan uygulamalarla uyumlu değildir. Bu durumdaysanız, bu ayarı kapatmanızı öneririz.

Yalnızca HTTP Çerezler ayarı hakkında daha fazla bilgi için azure [AD Application Proxy kullanarak uygulamaları yayımla'ya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal)bakın.

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Azure kaynakları için ayrıcalıklı kimlik yönetimi (PIM), Yönetim Grubu kaynak türlerini destekler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi
 
Abonelikler, Kaynak Grupları ve Kaynaklar (VM'ler, Uygulama Hizmetleri ve daha fazlası gibi) için yaptığınız gibi, Tam Zamanında etkinleştirme ve atama ayarları artık Yönetim Grubu kaynak türlerine uygulanabilir. Buna ek olarak, bir Yönetim Grubu için yönetici erişimi sağlayan bir rolü olan herkes bu kaynağı PIM'de keşfedebilir ve yönetebilir.

PIM ve Azure kaynakları hakkında daha fazla bilgi için, [Ayrıcalıklı Kimlik Yönetimi'ni kullanarak Azure kaynaklarını keşfedin ve yönetin](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Uygulama erişimi (önizleme), Azure AD portalına daha hızlı erişim sağlar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi
 
Bugün, PIM kullanarak bir rolü etkinleştirirken, izinlerin etkili olması 10 dakikadan fazla sürebilir. Şu anda genel önizlemede olan Uygulama erişimini kullanmayı seçerseniz, yöneticiler etkinleştirme isteği tamamlanır tamamlanmaz Azure AD portalına erişebilir.

Şu anda, Uygulama erişimi yalnızca Azure AD portalı deneyimini ve Azure kaynaklarını destekler. PIM ve Uygulama erişimi hakkında daha fazla bilgi için Azure [AD Ayrıcalıklı Kimlik Yönetimi nedir?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Ağustos 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Ağustos 2018'de, Federasyon desteğine sahip bu 16 yeni uygulamayı uygulama galerisine ekledik:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline Unbound](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sos Labs - Mobil ve Web Testi](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), Meta Networks [Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [Way We Do](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial)( [Inlogik tarafından)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dosya](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - Gider raporları](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 Canlı Sohbet](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Yerel Tableau desteği artık Azure AD Application Proxy'de kullanılabilir

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü

Ön kimlik doğrulama protokolümüz için OpenID Connect'ten OAuth 2.0 Code Grant protokolüne yaptığımız güncelleme yle, Application Proxy ile Tableau'yu kullanmak için ek yapılandırma yapmanız alabilirsiniz. Bu protokol değişikliği, Yalnızca JavaScript ve HTML etiketlerinde yaygın olarak desteklenen yalnızca HTTP yönlendirmelerini kullanarak Application Proxy'nin daha iyi desteklemesine yardımcı olur.

Tableau'ya yönelik yerel desteğimiz hakkında daha fazla bilgi için, [yerel Tableau desteğiyle azure AD Application Proxy'ye](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support)bakın.

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Azure Active Directory'deki B2B konuk kullanıcıları için Google'ı kimlik sağlayıcısı olarak eklemek için yeni destek (önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C

Kuruluşunuzdaki Google ile federasyon kurarak, davet edilen Gmail kullanıcılarının kişisel bir Microsoft Hesabı (MSAs) veya Bir Azure AD hesabı oluşturmak zorunda kalmadan, mevcut Google hesaplarını kullanarak paylaşılan uygulamalarınızda ve kaynaklarınızda oturum açmalarına izin verebilirsiniz.

Bu bir opt-in genel önizlemesidir. Google federasyonu hakkında daha fazla bilgi için [B2B konuk kullanıcıları için google'ı bir kimlik sağlayıcısı olarak ekle'ye](https://docs.microsoft.com/azure/active-directory/b2b/google-federation)bakın.

---

## <a name="july-2018"></a>Temmuz 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Azure Active Directory e-posta bildirimlerinde iyileştirmeler

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Kimlik yaşam döngüsü yönetimi
 
Azure Etkin Dizin (Azure AD) e-postaları artık güncelleştirilmiş bir tasarımın yanı sıra aşağıdaki hizmetlerden gönderildiğinde gönderen e-posta adresi ve gönderenin görüntü adı değişiklikleri ne kadar dır:
 
- Azure AD Erişim Yorumları
- Azure AD Connect Health 
- Azure AD Kimlik Koruması 
- Azure AD Privileged Identity Management
- Kurumsal Uygulama Süresi Dolan Sertifika Bildirimleri
- Kurumsal Uygulama Sağlama Hizmeti Bildirimleri
 
E-posta bildirimleri aşağıdaki e-posta adresinden ve görüntü adından gönderilecektir:

- E-posta adresi:azure-noreply@microsoft.com
- Görüntü adı: Microsoft Azure
 
Bazı yeni e-posta tasarımları ve daha fazla bilgi örneği için [Azure AD PIM'deki E-posta bildirimlerine](https://go.microsoft.com/fwlink/?linkid=2005832)bakın.

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure REKLAM Etkinlik Günlükleri artık Azure Monitor üzerinden kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Azure REKLAM Etkinliği Günlükleri artık Azure Monitörü (Azure'un platform genelindeki izleme hizmeti) için genel önizlemede kullanılabilir. Azure Monitor, bu geliştirmelere ek olarak size uzun süreli bekletme ve sorunsuz tümleştirme sunar:

- Günlük dosyalarınızı kendi Azure depolama hesabınıza yönlendirme yaparak uzun süreli bekletme.

- Özel komut dosyaları yazmanızı veya korumanızı gerektirmeden sorunsuz SIEM tümleştirmesi.

- Kendi özel çözümlerinizle, analiz araçlarınızla veya olay yönetimi çözümlerinizle sorunsuz entegrasyon.

Bu yeni özellikler hakkında daha fazla bilgi için, [Azure Monitor tanılamalarında blogumuza bakın Azure AD etkinlik günlükleri artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) ve belgelerimiz, [Azure Etkin Dizin etkinliği günlükleri Azure Monitor'da (önizleme)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Azure AD oturum açma raporuna eklenen Koşullu Erişim bilgileri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
Bu güncelleştirme, bir kullanıcı ilke sonucuyla birlikte kaydettiğinde hangi ilkelerin değerlendirildiğini görmenizi sağlar. Buna ek olarak, rapor artık kullanıcı tarafından kullanılan istemci uygulaması türünü içerir, böylece eski protokol trafiğini tanımlayabilirsiniz. Rapor girişleri artık kullanıcıya bakan hata iletisinde bulunan ve eşleşen oturum açma isteğini tanımlamak ve gidermek için kullanılabilecek bir korelasyon kimliği için de aranabilir.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Oturum açma etkinlik günlükleri aracılığıyla eski kimlik doğrulamalarını görüntüleme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama
 
**Müşteri Uygulaması** alanının Oturum Açma etkinlik günlüklerine girmesiyle, müşteriler artık eski kimlik doğrulamalarını kullanan kullanıcıları görebilir. Müşteriler bu bilgilere Oturum Açma Microsoft Graph API'sini kullanarak veya eski kimlik doğrulamalarına filtre sağlamak için **Müşteri Uygulaması** denetimini kullanabileceğiniz Azure AD portalındaki Oturum Açma etkinlik günlükleri aracılığıyla erişebilecektir. Daha fazla ayrıntı için belgelere göz atın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Temmuz 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Temmuz 2018'de, Federasyon desteğine sahip bu 16 yeni uygulamayı uygulama galerisine ekledik:

[Yenilik Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [Bazı Yönetici SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC Evreleme , [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Screencast-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), PowerSchool Birleşik Sınıf , [Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar Uzaktan Destek](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow Bağlayıcı](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](../saas-apps/kanbanize-tutorial.md), [SmartLPA](../saas-apps/smartlpa-tutorial.md), [Beceri Bankası](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://aka.ms/azureadapprequest)bkz.

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>SaaS uygulama entegrasyonlarını sağlayan yeni kullanıcı - Temmuz 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** 3.
 
Azure AD, Dropbox, Salesforce, ServiceNow ve daha fazlası gibi SaaS uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatikleştirmenize olanak tanır. Temmuz 2018 için Azure AD uygulama galerisine aşağıdaki uygulamalar için kullanıcı sağlama desteği ekledik:

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Azure AD galerisinde kullanıcı sağlamayı destekleyen tüm uygulamaların listesi için Bkz. [Azure Etkin Dizini ile SaaS uygulama tümleştirmesi.](https://aka.ms/appstutorial)

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Eşitleme için Sistem Durumu'na Bağlanma - Verilen ve yinelenen öznitelik eşitleme hatalarını düzeltmenin daha kolay bir yolu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** AD Bağlantı  
**Ürün kapasitesi:** İzleme & Raporlama
 
Azure AD Connect Health, eşitleme hatalarını vurgulamanıza ve düzeltmenize yardımcı olmak için self servis düzeltmesi sunar. Bu özellik, yinelenen öznitelik eşitleme hatalarını giderir ve Azure AD'den farklı olan nesneleri düzeltir. Bu tanının aşağıdaki yararları vardır:

- Yinelenen öznitelik eşitleme hatalarını daraltarak belirli düzeltmeler sağlar

- Özel Azure REKLAM senaryoları için düzeltme uygular ve hataları tek bir adımda giderir

- Bu özelliği açmak ve kullanmak için yükseltme veya yapılandırma gerekmez

Daha fazla bilgi için bkz: [Tanıla ve yinelenen öznitelik eşitleme hatalarını düzeltin](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors)

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Azure AD ve MSA oturum açma deneyimleriiçin görsel güncelleştirmeler

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Microsoft'un Çevrimiçi Hizmetler oturum açma deneyimi (Office 365 ve Azure gibi) için web'de bulunan web'yi güncelledik. Bu değişiklik, ekranları daha az karmaşık ve daha basit hale getirir. Bu değişiklik hakkında daha fazla bilgi için [Azure AD oturum açma deneyimi blogunda yapılacak iyileştirmelere](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) bakın.

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Azure AD Connect'in yeni sürümü - Temmuz 2018

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi

Azure AD Connect'in en son sürümü şunları içerir: 

- Hata düzeltmeleri ve desteklenebilirlik güncelleştirmeleri 

- Ping-Federate entegrasyonunun Genel Durumu

- En son SQL 2012 istemcisi güncelleştirmeleri 

Bu güncelleştirme hakkında daha fazla bilgi için Azure [AD Connect: Sürüm sürüm geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-end-user-ui"></a>Kullanım koşullarını son kullanıcı kullanıcı arabirimi güncelleştirmeleri

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Yönetim

TOU son kullanıcı kullanıcı kullanıcı ui'deki kabul dizesini güncelliyoruz.

**Geçerli metin.** [tenantName] kaynaklarına erişmek için kullanım koşullarını kabul etmelisiniz.<br>**Yeni metin.** [tenantName] kaynağına erişmek için kullanım koşullarını okumanız gerekir.

**Geçerli metin:** Kabul etmeyi seçmek, yukarıdaki kullanım koşullarının tümlerini kabul ettiğiniz anlamına gelir.<br>**Yeni metin:** Kullanım koşullarını okuduğunuzve anladığınızı doğrulamak için lütfen Kabul et'i tıklatın.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Geçiş Kimlik Doğrulaması eski protokolleri ve uygulamaları destekler

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Geçiş Kimlik Doğrulaması artık eski protokolleri ve uygulamaları destekler. Aşağıdaki sınırlamalar artık tam olarak desteklenir:

- Eski Office istemci uygulamalarına, Office 2010 ve Office 2013'e modern kimlik doğrulama gerektirmeden kullanıcı oturum açma.

- Yalnızca Office 2010'daki Exchange karma ortamlarında takvim paylaşımına ve ücretsiz/meşgul bilgilere erişim.

- Modern kimlik doğrulaması gerektirmeden Skype for Business istemci uygulamalarında kullanıcı oturum açma.

- PowerShell sürüm 1.0'da kullanıcı oturum açma.

- iOS Kurulum Yardımcısı'nı kullanan Apple Device Kayıt Programı (Apple DEP). 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Self servis parola sıfırlama ve Çok Faktörlü Kimlik Doğrulama için yakınsak güvenlik bilgi yönetimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** SSPR  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Bu yeni özellik, kullanıcıların kendi self servis parola sıfırlama (SSPR) ve Çok Faktörlü Kimlik Doğrulama (MFA) için güvenlik bilgilerini (örneğin, telefon numarası, e-posta adresi, mobil uygulama vb.) tek bir deneyimle yönetmelerine olanak tanır. Kullanıcılar artık iki farklı deneyimde SSPR ve MFA için aynı güvenlik bilgilerini kaydetmek zorunda kalmayacaktır. Bu yeni deneyim, SSPR veya MFA'ya sahip kullanıcılar için de geçerlidir.

Bir kuruluş MFA veya SSPR kaydını uygulamıyorsa, kullanıcılar güvenlik bilgilerini **Uygulamalarım** portalı üzerinden kaydedebilirler. Buradan, kullanıcılar MFA veya SSPR için etkinleştirilen tüm yöntemleri kaydedebilirler. 

Bu bir opt-in genel önizlemesidir. Yöneticiler, seçilen bir kullanıcı grubu veya kiracıdaki tüm kullanıcılar için yeni deneyimi (istenirse) açabilir.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Parolanızı sıyrırken kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanın

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** SSPR  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Bu özellik, Microsoft Authenticator'dan (veya başka bir kimlik doğrulayıcı uygulamadan) gelen bir bildirim veya kodu kullanarak parolayı sıfırlarken yönetici olmayanların kimliklerini doğrulamalarına olanak tanır. Yöneticiler bu self servis parola sıfırlama yöntemini açtıktan sonra, aka.ms/mfasetup veya aka.ms/setupsecurityinfo aracılığıyla mobil uygulama kaydetmiş olan kullanıcılar, parolalarını sıfırlarken mobil uygulamalarını doğrulama yöntemi olarak kullanabilirler.

Mobil uygulama bildirimi yalnızca parolanızı sıfırlamak için iki yöntem gerektiren bir ilkenin parçası olarak açılabilir.

---

## <a name="june-2018"></a>Haziran 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Değişiklik bildirimi: Azure REKLAM Etkinlik Günlükleri API'sini kullanan uygulamalar için yetki lendirme akışına güvenlik düzeltmesi

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Daha güçlü güvenlik uygulamamız sayesinde, [Azure REKLAM Etkinliği Günlükleri API'lerine](https://aka.ms/aadreportsapi)erişmek için yetkilendirme akışı kullanan uygulamaların izinlerinde değişiklik yapmak zorunda kaldık. Bu değişiklik **26 Haziran 2018'e**kadar gerçekleşecektir.

Uygulamalarınızdan herhangi biri Azure REKLAM Etkinliği Günlüğü API'leri kullanıyorsa, değişiklik olduktan sonra uygulamanın kırılmadığından emin olmak için aşağıdaki adımları izleyin.

**Uygulama izinlerinizi güncellemek için**

1. Azure portalında oturum açın, **Azure Etkin Dizini'ni**seçin ve ardından **Uygulama Kayıtları'nı**seçin.
2. Azure REKLAM Etkinliği Günlükleri API'sini kullanan uygulamanızı seçin, **Ayarlar'ı**seçin, **Gerekli izinleri**seçin ve ardından **Windows Azure Etkin Dizin API'sini** seçin.
3. **Access** blade'in **Temsilciler Tarafından İzinler** alanında, Okuma **dizini** verilerinin yanındaki kutuyu seçin ve sonra **Kaydet'i**seçin.
4. **Hibe izinlerini**seçin ve ardından **Evet'i**seçin.
    
    >[!Note]
    >Uygulamaya izin vermek için Global yöneticisi olmalısınız.

Daha fazla bilgi için, Azure AD raporlama API makalesine erişmek için Ön Koşulların [Hibe izinleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) alanına bakın.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>TLS ayarlarını PCI DSS uyumluluğu için Azure AD hizmetlerine bağlanmak üzere yapılandırın

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Yok  
**Ürün kapasitesi:** Platform

Aktarım Katmanı Güvenliği (TLS), iletişim sağlayan iki uygulama arasında gizlilik ve veri bütünlüğü sağlayan ve günümüzde kullanılan en yaygın olarak dağıtılan güvenlik protokolüolan bir protokoldür.

[PCI Güvenlik Standartları Konseyi,](https://www.pcisecuritystandards.org/) TLS ve Secure Sockets Layer'ın (SSL) ilk sürümlerinin **30 Haziran 2018**tarihinden itibaren yeni ve daha güvenli uygulama protokolleri sağlamak lehine devre dışı bırakılmasını belirlemiştir. Bu değişiklik, Azure AD hizmetlerine bağlanır ve PCI DSS uyumluluğu gerektirirseniz TLS 1.0'ı devre dışı bmelisiniz. TLS'nin birden çok sürümü mevcuttur, ancak TLS 1.2 Azure Active Directory Services için kullanılabilen en son sürümüdür. Hem istemci/sunucu hem de tarayıcı/sunucu kombinasyonları için doğrudan TLS 1.2'ye geçmenizi önemle öneririz.

Güncel olmayan tarayıcılar TLS 1.2 gibi yeni TLS sürümlerini desteklemeyebilir. TLS'nin hangi sürümlerinin tarayıcınız tarafından desteklenerek destekleniyi görmek için [Qualys SSL Labs](https://www.ssllabs.com/) sitesine gidin ve **tarayıcınızı test**edin'e tıklayın. Web tarayıcınızın en son sürümüne yükseltmenizi ve tercihen yalnızca TLS 1.2'yi etkinleştirmenizi öneririz.

**TLS 1.2'yi tarayıcı ile etkinleştirmek için**

- **Microsoft Edge ve Internet Explorer (her ikisi de Internet Explorer kullanılarak ayarlanır)**

    1. Internet Explorer'ı açın, **Araçlar** > **Internet Seçenekleri** > **İleri'yi**seçin.
    2. **Güvenlik** alanında **TLS 1.2'yi ve**ardından **Tamam'ı**seçin.
    3. Tüm tarayıcı pencerelerini kapatın ve Internet Explorer'ı yeniden başlatın. 

- **Google Chrome**

    1. Google Chrome'u açın, adres çubuğuna *chrome://settings/* yazın ve **Enter**tuşuna basın.
    2. **Gelişmiş** seçenekleri genişletin, **Sistem** alanına gidin ve **proxy ayarlarını aç'ı**seçin.
    3. Internet **Özellikleri** kutusunda **Gelişmiş** sekmesini seçin, **Güvenlik** alanına gidin, **TLS 1.2'yi kullanın**ve ardından **Tamam'ı**seçin.
    4. Tüm tarayıcı pencerelerini kapatın ve Google Chrome'u yeniden başlatın.

- **Mozilla Firefox**

    1. Firefox'u açın, adres çubuğuna *şu bilgileri* yazın ve **enter**tuşuna basın.
    2. Terim, *TLS'yi*arayın ve ardından **security.tls.version.max** girişini seçin.
    3. Tarayıcıyı TLS 1.2 sürümüne kadar kullanmaya zorlamak için değeri **3** olarak ayarlayın ve ardından **Tamam'ı**seçin.

        >[!NOTE]
        >Firefox sürüm 60.0 TLS 1.3'ü destekler, böylece security.tls.version.max değerini **4**olarak da ayarlayabilirsiniz.

    4. Tüm tarayıcı pencerelerini kapatın ve Mozilla Firefox'u yeniden başlatın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Haziran 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Haziran 2018'de, Federasyon desteğine sahip bu 15 yeni uygulamayı uygulama galerisine ekledik:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [Yerleşme müzik](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1.1 Belirteç lobi uygulaması etkin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Endpoint Yedekleme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh Networks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2 , [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [SharePoint on-premises](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [ForeSee CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın. Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)bkz. 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD Parola Koruması genel önizlemede kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması

Ortamınızdan kolayca tahmin edilen parolaları ortadan kaldırmaya yardımcı olmak için Azure AD Parola Koruması'nı kullanın. Bu parolaların ortadan kaldırılması, parola püskürtme saldırı türünden ödün verme riskini azaltmaya yardımcı olur.

Azure AD Parola Koruması özellikle şunları yardımcı olur:

- Kuruluşunuzun hesaplarını hem Azure AD hem de Windows Server Active Directory (AD) olarak koruyun. 
- Kullanıcılarınızın en sık kullanılan 500'den fazla parola ve bu parolaların 1 milyondan fazla karakter değiştirme varyasyonu listesinde parola kullanmasını durdurur.
- Azure AD Parola Koruması'nı Hem Azure AD hem de şirket içi Windows Server AD için Azure AD portalındaki tek bir konumdan uygulayın.

Azure AD Parola Koruması hakkında daha fazla bilgi [için](https://aka.ms/aadpasswordprotectiondocs)bkz.

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında oluşturulan yeni "tüm konuklar" Koşullu Erişim ilkesi şablonu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Yönetim

Kullanım koşullarınızın oluşturulması sırasında, "tüm konuklar" ve "tüm uygulamalar" için yeni bir Koşullu Erişim ilkesi şablonu da oluşturulur. Bu yeni ilke şablonu, misafirler için oluşturma ve uygulama işlemini kolaylaştıran yeni oluşturulan ToU'yu uygular.

Daha fazla bilgi için Azure [Etkin Dizin Kullanım Koşulları özelliğine](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında oluşturulan yeni "özel" Koşullu Erişim ilkesi şablonu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Yönetim

Kullanım koşullarınızın oluşturulması sırasında, yeni bir "özel" Koşullu Erişim ilkesi şablonu da oluşturulur. Bu yeni ilke şablonu, portalda el ile gezinmeye gerek kalmadan ToU'yu oluşturmanıza ve ardından hemen Koşullu Erişim ilkesi oluşturma bıçağına gitmenize olanak tanır.

Daha fazla bilgi için Azure [Etkin Dizin Kullanım Koşulları özelliğine](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama'yı dağıtma konusunda yeni ve kapsamlı kılavuz

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
Azure Çok Faktörlü Kimlik Doğrulaması'nı (MFA) kuruluşunuzda nasıl dağıtılayabildiğimizle ilgili adım adım yeni bir kılavuz yayınladık.

MFA dağıtım kılavuzunu görüntülemek için GitHub'daki [Kimlik Dağıtım Kılavuzları](https://aka.ms/DeploymentPlans) repo'ya gidin. Dağıtım kılavuzları hakkında geri bildirim sağlamak için [Dağıtım Planı Geri Bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzları hakkında herhangi bir sorunuz varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bize ulaşın.

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD yetkin uygulama yönetimi rolleri genel önizlemede

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Erişim Kontrolü

Yöneticiler artık Genel Yönetici rolünü atamadan uygulama yönetimi görevlerini devredebilir. Yeni roller ve yetenekler şunlardır:

- **Yeni standart Azure AD yönetici rolleri:**

    - **Uygulama Yöneticisi.** Kayıt, SSO ayarları, uygulama atamaları ve lisanslama, Uygulama proxy ayarları ve onay (Azure REKLAM kaynakları hariç) dahil olmak üzere tüm uygulamaların tüm yönlerini yönetme olanağı sağlar.

    - **Bulut Uygulama Yöneticisi.** Şirket içi erişim sağlamadığından, Uygulama proxy'si dışında tüm Uygulama Yöneticisi yeteneklerini verir.

    - **Uygulama Geliştiricisi.** **Kullanıcıların uygulamaları kaydetmesine izin verme** seçeneği kapalı olsa bile, uygulama kayıtları oluşturma olanağı sağlar.

- **Sahiplik (grup sahipliği sürecine benzer şekilde uygulama başına kayıt ve şirket başına uygulama ayarlama:**
 
    - **Uygulama Kayıt Sahibi.** Uygulama bildirimi ve ek sahipler ekleme dahil olmak üzere, sahip olunan uygulama kaydının tüm yönlerini yönetme olanağı sağlar.

    - **Kurumsal Uygulama Sahibi.** SSO ayarları, uygulama atamaları ve onay (Azure AD kaynakları hariç) dahil olmak üzere sahip olunan kurumsal uygulamaların birçok yönünü yönetme olanağı sağlar.

Genel önizleme hakkında daha fazla bilgi için, [Azure AD tarafından yetkiverilen uygulama yönetimi rolleri genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) yer alan bölüme bakın! Blog. Roller ve izinler hakkında daha fazla bilgi için Azure [Etkin Dizini'nde yönetici rolleri atama'ya](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)bakın.

---

## <a name="may-2018"></a>Mayıs 2018

### <a name="expressroute-support-changes"></a>ExpressRoute destek değişiklikleri

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Platform  

Azure Active Directory (Azure AD) gibi Hizmet sunan yazılımlar, ExpressRoute veya diğer özel VPN tünellerine gerek kalmadan doğrudan Internet üzerinden giderek en iyi şekilde çalışacak şekilde tasarlanmıştır. Bu nedenle, **1 Ağustos 2018'de**Microsoft'a bakan Azure genel bakışlarını ve Azure topluluklarını kullanarak Azure REKLAM hizmetleri için ExpressRoute'u desteklemeyi durduracağız. Bu değişiklikten etkilenen tüm hizmetler, Azure AD trafiğinin ExpressRoute'dan Internet'e kademeli olarak kaydığını fark edebilir.

Desteğimizi değiştirirken, kimlik doğrulama trafiğiniz için özel bir devre seti kullanmanız gerekebileceğini de biliyoruz. Bu nedenle Azure AD, ExpressRoute'u ve Microsoft'taki hizmetleri kullanarak kiracı başına IP aralığı kısıtlamalarını desteklemeye devam edecektir ve "Diğer Office 365 Çevrimiçi hizmetler" topluluğuna bakar. Hizmetleriniz etkilenir, ancak ExpressRoute'a ihtiyaç duyarsanız, aşağıdakileri yapmanız gerekir:

- **Azure genel olarak genel olarak bakıyorsanız.** Microsoft'a göz atarak geçin ve **Diğer Office 365 Çevrimiçi hizmetleri (12076:5100)** topluluğuna kaydolun. Azure genel inden Microsoft'a bakışa nasıl taşınır hakkında daha fazla bilgi için, [Microsoft'a bakan](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) bir ortak makaleye bakış taşı'na bakın.

- **Microsoft'a bakıyorsanız.** Diğer Office **365 Çevrimiçi hizmeti (12076:5100)** topluluğuna kaydolun. Yönlendirme gereksinimleri hakkında daha fazla bilgi için, ExpressRoute yönlendirme gereksinimleri makalesinin [BGP toplulukları için Destek bölümüne](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) bakın.

Özel devreleri kullanmaya devam ederseniz, **Diğer Office 365 Çevrimiçi hizmetini (12076:5100)** kullanmak için yetkilendirmeyi nasıl alacağınız konusunda Microsoft Hesap ekibinizle konuşmanız gerekir. MS Office tarafından yönetilen inceleme kurulu, bu devrelere ihtiyacınız olup olmadığını doğrular ve bunları tutmanın teknik sonuçlarını anladığınızdan emin olur. Office 365 için rota filtreleri oluşturmaya çalışan yetkisiz abonelikler bir hata iletisi alır. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>TOU için yönetim senaryoları için Microsoft Grafik API'leri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Geliştirici Deneyimi
 
Azure AD kullanım koşullarının yönetim çalışması için Microsoft Graph API'leri ekledik. Kullanım koşulları nesnesi oluşturabilir, güncelleyebilir, silebilirsiniz.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Azure AD B2C'de kimlik sağlayıcısı olarak Azure AD çok kiracılı bitiş noktası ekleme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C
 
Özel ilkeleri kullanarak, artık Azure AD B2C'de kimlik sağlayıcısı olarak Azure AD ortak bitiş noktasını ekleyebilirsiniz. Bu, uygulamalarınızda oturum açan tüm Azure AD kullanıcıları için tek bir giriş noktasına sahip olmanızı sağlar. Daha fazla bilgi için [bkz: Azure Etkin Dizin B2C: Kullanıcıların özel ilkeler kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısında oturum açmalarına izin verin.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom)

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Uygulamalarım Oturum Açma Uzantımız ve Azure AD Uygulama Proxy'miz ile her yerden uygulamalara erişmek için Dahili URL'leri kullanın

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Sso
 
Kullanıcılar artık Azure AD için Uygulamalarım Güvenli Oturum Açma Uzantısı'nı kullanarak kurumsal ağınızın dışında yken bile dahili URL'ler aracılığıyla uygulamalara erişebilir. Bu, Access Panel tarayıcı uzantısı yüklü olan herhangi bir tarayıcıda Azure AD Application Proxy kullanarak yayımladığınızdaki tüm uygulamalarla çalışır. Kullanıcı uzantıya giriş yaptıktan sonra URL yeniden yönlendirme işlevi otomatik olarak etkinleştirilir. Uzantısı [Microsoft Edge,](https://go.microsoft.com/fwlink/?linkid=845176) [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)ve [Firefox'ta](https://go.microsoft.com/fwlink/?linkid=866366)indirmek için kullanılabilir.

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory - Avrupa müşterileri için Avrupa'daki veriler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** GoLocal

Avrupa'daki müşteriler, gizlilik ve Avrupa yasalarına uygun olarak verilerinin Avrupa'da kalmasını ve Avrupa veri merkezlerinin dışında çoğaltılmamasını gerektirir. Bu [makalede,](https://go.microsoft.com/fwlink/?linkid=872328) hangi kimlik bilgilerinin Avrupa'da depolanacağı hakkında özel ayrıntılar ve Avrupa veri merkezleri dışında depolanacak bilgiler hakkında ayrıntılı bilgi verilmektedir. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>SaaS uygulama entegrasyonlarını sağlayan yeni kullanıcı - Mayıs 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** 3.
 
Azure AD, Dropbox, Salesforce, ServiceNow ve daha fazlası gibi SaaS uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatikleştirmenize olanak tanır. Mayıs 2018 için Azure AD uygulama galerisine aşağıdaki uygulamalar için kullanıcı sağlama desteği ekledik:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Cornerstone OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Azure AD galerisinde kullanıcı sağlamayı destekleyen tüm uygulamaların listesi [https://aka.ms/appstutorial](https://aka.ms/appstutorial)için bkz.

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Azure AD erişim incelemeleri ve uygulama erişimi artık yinelenen incelemeler sağlar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Erişim İncelemeleri  
**Ürün kapasitesi:** Yönetim
 
Grupların ve uygulamaların erişim incelemesi artık Azure AD Premium P2'nin bir parçası olarak genel olarak kullanılabilir.  Yöneticiler, grup üyeliklerinin ve uygulama atamalarının erişim değerlendirmelerini aylık veya üç aylık gibi düzenli aralıklarla otomatik olarak yinelenecek şekilde yapılandırabilecektir.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure REKLAM Etkinliği günlükleri (oturum açma ve denetim) artık MS Graph aracılığıyla kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama
 
Oturum Açma ve Denetim günlüklerini içeren Azure REKLAM Etkinliği günlükleri artık Microsoft Graph API'si aracılığıyla kullanılabilir. Bu günlüklere erişmek için Microsoft Graph API aracılığıyla iki uç noktayı ortaya çıkardık. Başlamak için Azure AD Raporlama API'lerine programlı erişim için [belgelerimize](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) göz atın. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>B2B itfa deneyimi iyileştirmeler ve bir org bırakın

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C

**Tam zamanında kefaret:** B2B API kullanarak bir kaynağı konuk kullanıcıyla paylaştıktan sonra özel bir davet e-postası göndermeniz gerekmez. Çoğu durumda, konuk kullanıcı kaynağa erişebilir ve tam zamanında kullanım deneyimi yoluyla alınacaktır. Cevapsız e-postalar nedeniyle daha fazla etki yok. Artık konuk kullanıcılarınıza "Sistemin size gönderdiği ödeme bağlantısını tıkladınız mı?" diye sormak yok. Bu, SPO davet yöneticisini kullandığında – bulutlu eklerin tüm kullanıcılar için aynı kanonik URL'ye sahip olabileceği anlamına gelir – dahili ve harici - herhangi bir kullanım durumunda.

**Modern kurtuluş deneyimi:** Artık bölünmüş ekran redemption açılış sayfası yok. Kullanıcılar, üçüncü taraf uygulamalarda olduğu gibi davet eden kuruluşun gizlilik bildiriminde de modern bir onay deneyimi yaşarlar.

**Konuk kullanıcılar org'dan ayrılabilir:** Bir kullanıcının bir org ile ilişkisi sona erdiğinde, kuruluştan ayrılarak kendi kendine hizmet edebilir. Artık davet eden org'un yöneticisini "kaldırılacak" diye çağırmak yok, destek biletlerini yükseltmek yok.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Azure AD uygulama galerisinde yeni Federe Uygulamalar mevcuttur - Mayıs 2018

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.
 
Mayıs 2018'de, Federasyon desteğine sahip bu 18 yeni uygulamayı uygulama galerimizle ekledik:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty Govern, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial), OpenReel, Arc Yayıncılık - [SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial) [まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın.

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)bkz.

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Azure Etkin Dizin için yeni adım adım dağıtım kılavuzları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** Dizin
 
Azure Active Directory'nin (Azure AD) nasıl dağıtılanacağı, self servis parola sıfırlama (SSPR), tek oturum açma (SSO), Koşullu Erişim (CA), Uygulama proxy'si, Kullanıcı sağlama, Etkin Dizin Federasyonu Hizmetleri (ADFS) ile Geçiş Kimlik Doğrulama (PTA) ve ADFS'nin Parola karma senkronizasyonu (PHS) dahil olmak üzere nasıl dağıtılanacağı hakkında adım adım kılavuz.

Dağıtım kılavuzlarını görüntülemek için GitHub'daki [Kimlik Dağıtım Kılavuzları](https://aka.ms/DeploymentPlans) repo'ya gidin. Dağıtım kılavuzları hakkında geri bildirim sağlamak için [Dağıtım Planı Geri Bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzları hakkında herhangi bir sorunuz varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bize ulaşın.

---

### <a name="enterprise-applications-search---load-more-apps"></a>Kurumsal Uygulamalar Arama - Daha Fazla Uygulama Yükle

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso
 
Başvurularınızı / hizmet müdürlerinizi bulmakta sorun mu yaşıyormusunuz? Kurumsal uygulamalarınıza tüm uygulama listenize daha fazla uygulama yükleme olanağı ekledik. Varsayılan olarak, 20 uygulama gösteririz. Artık ek uygulamaları görüntülemek için **daha fazla yükleyin,** tıklayabilirsiniz. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>AADConnect'in Mayıs sürümü PingFederate ile tümleştirmenin genel önizlemesini, önemli güvenlik güncelleştirmelerini, birçok hata düzeltmesini ve yeni harika yeni sorun giderme araçlarını içerir. 

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** AD Bağlantı  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi
 
AADConnect'in Mayıs sürümü PingFederate ile tümleştirmenin genel önizlemesini, önemli güvenlik güncelleştirmelerini, birçok hata düzeltmesini ve yeni harika yeni sorun giderme araçlarını içerir. Burada sürüm [notları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190)bulabilirsiniz.

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD erişim incelemeleri: otomatik uygulama

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Erişim İncelemeleri  
**Ürün kapasitesi:** Yönetim

Grupların ve uygulamaların erişim incelemeleri artık Azure AD Premium P2'nin bir parçası olarak genel olarak kullanılabilir. Bir yönetici, erişim incelemesi tamamlandığında gözden geçirenin değişikliklerini otomatik olarak bu gruba veya uygulamaya uygulayacak şekilde yapılandırabilir. Yönetici, gözden geçirenler yanıt vermemişse, erişimi kaldırmamışsa, erişimi korumazsa veya sistem önerilerini kabul etmemişse, kullanıcının sürekli erişimine ne olacağını da belirtebilir. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-response_mode-for-new-apps"></a>Kimlik belirteçleri artık yeni uygulamalar için sorgu response_mode kullanılarak döndürülemez. 

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
25 Nisan 2018 tarihinde veya sonrasında oluşturulan uygulamalar artık **sorgu** response_mode kullanarak **id_token** isteyemez.  Bu, Azure AD'yi OIDC belirtimleriyle aynı çizgiye getirir ve uygulamalarınızın saldırı yüzeyini azaltmaya yardımcı olur.  25 Nisan 2018 tarihinden önce oluşturulan uygulamaların, **id_token**response_type olan **sorgu** response_mode kullanması engellenmez.  AAD'den bir id_token talep ederken döndürülen hata **AADSTS70007'dir: 'sorgu' belirteç talep ederken 'response_mode' olarak desteklenen bir değer değildir.**

**Parça** ve **form_post** response_modes çalışmaya devam response_modes - yeni uygulama nesneleri oluştururken (örneğin, App Proxy kullanımı için), yeni bir uygulama oluşturmadan önce bu response_modes birinin kullanılmasını sağlar.  

---
 
## <a name="april-2018"></a>Nisan 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C Erişim Belirteci GA

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C 

Artık azure AD B2C tarafından güvence altına alınmıştır Web API'lerine erişim belirteçlerini kullanarak erişebilirsiniz. Özellik genel önizlemeden GA'ya taşınıyor. Azure AD B2C uygulamalarını ve web API'lerini yapılandırmak için kullanıcı arabirimi deneyimi geliştirildi ve diğer küçük iyileştirmeler yapıldı.
 
Daha fazla bilgi için Azure [AD B2C: Erişim belirteçleri isteme.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens)

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>SAML tabanlı uygulamalar için tek oturum açma yapılandırmasını test edin

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

SAML tabanlı SSO uygulamalarını yapılandırırken, tümleştirmeyi yapılandırma sayfasında sınayabilirsiniz. Oturum açma sırasında bir hatayla karşılaşırsanız, sınama deneyiminde hata sağlayabilir siniz ve Azure AD size belirli sorunu çözmek için çözüm adımları sağlar.

Daha fazla bilgi için bkz.

- [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Azure Active Directory'deki uygulamalarda SAML tabanlı tek oturum açma hataayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD kullanım koşulları artık kullanıcı başına raporlamaya sahip

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Uyumlu -luk
 
Yöneticiler artık belirli bir ToU seçebilir ve bu ToU'ya izin veren tüm kullanıcıları ve hangi tarih/saatin gerçekleştiğini görebilir.

Daha fazla bilgi için [Azure REKLAM kullanım koşulları özelliğine](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: AD FS extranet kilitleme koruması için riskli IP 

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** İzleme & Raporlama

Connect Health artık, başarısız U/P giriş eşiğini saat başı veya günlük olarak aşan IP adreslerini algılama olanağını destekler. Bu özellik tarafından sağlanan özellikler şunlardır:

- IP adresini ve özelleştirilebilir eşikle saatlik/günlük olarak oluşturulan başarısız giriş sayısını gösteren kapsamlı rapor.
- Belirli bir IP adresinin başarısız U/P giriş eşiğini saatlik/günlük olarak aştığını gösteren e-posta tabanlı uyarılar.
- Verilerin ayrıntılı analizini yapmak için bir indirme seçeneği

Daha fazla bilgi için [Riskli IP Raporu'na](https://aka.ms/aadchriskyip)bakın.

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Meta veri dosyası veya URL ile kolay uygulama config

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso

Kurumsal uygulamalar sayfasında yöneticiler, AAD Galerisi ve Galeri Dışı uygulama için SAML tabanlı oturum açma yapılandırmak için bir SAML meta veri dosyası yükleyebilir.

Ayrıca, SSO'yu hedeflenen uygulamayla yapılandırmak için Azure AD uygulama federasyonu meta veri URL'sini kullanabilirsiniz.

Daha fazla bilgi için bkz: [Azure Etkin Dizin uygulama galerisinde olmayan uygulamalarda tek oturum](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)açma yapılandırma.

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD Kullanım Koşulları artık genel olarak kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Uyumlu -luk
 

Azure AD kullanım koşulları genel önizlemeden genel kullanıma taşındı.

Daha fazla bilgi için [Azure REKLAM kullanım koşulları özelliğine](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Belirli kuruluşlara ait B2B kullanıcılardan gelen davetlere izin verme veya engelleme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C
 

Artık Azure AD B2B İşbirliği'nde hangi ortak kuruluşları paylaşmak ve işbirliği yapmak istediğinizi belirtebilirsiniz. Bunu yapmak için, belirli izin verme veya reddet etki alanlarının listesini oluşturmayı seçebilirsiniz. Bir etki alanı bu özellikleri kullanarak engellendiğinde, çalışanlar artık bu etki alanındaki kişilere davet gönderemez.

Bu, onaylanan kullanıcılar için sorunsuz bir deneyim sağlarken kaynaklarınıza erişimi denetlemenize yardımcı olur.

Bu B2B İşbirliği özelliği tüm Azure Active Directory müşterileri için kullanılabilir ve koşullu erişim ve kimlik koruması gibi Azure AD Premium özellikleriyle birlikte kullanılabilir ve harici iş kullanıcılarının ne zaman ve nasıl oturum açarak erişeceğini daha ayrıntılı olarak kontrol etmek için kullanılabilir.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list)

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde bulunan yeni federe uygulamalar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Nisan 2018'de, Federasyon desteğine sahip bu 13 yeni uygulamayı uygulama galerimizle ekledik:

Kriter HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (On-Premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [Dinamik Sinyal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [OrgChart Şimdi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performans Monitörü](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), Cisco [Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), Raf , [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın.

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)bkz.

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Azure AD'deki B2B kullanıcılarına şirket içi uygulamalarınıza erişim izni verme (genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2b  
**Ürün kapasitesi:** B2B/B2C

Ortak kuruluşlardan gelen konuk kullanıcıları Azure AD'nize davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş olarak, artık bu B2B kullanıcılarına şirket içi uygulamalara erişim sağlayabilirsiniz. Bu şirket içi uygulamalar, Saml tabanlı kimlik doğrulamayı veya Entegre Windows Kimlik Doğrulaması'nı (IWA) Kerberos kısıtlı delegasyonu (KCD) ile kullanabilir.

Daha fazla bilgi için Azure [AD'deki Grant B2B kullanıcılarına şirket içi uygulamalarınız için erişim](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises)bakın.

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Azure Marketi'nden SSO tümleştirme eğitimlerini alın

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Diğer  
**Ürün kapasitesi:** 3.

[Azure marketinde](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) listelenen bir uygulama SAML tabanlı tek oturum açmayı destekliyorsa, **Şimdi Al'ı** tıklatarak bu uygulamayla ilişkili tümleştirme öğreticisini sağlar. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>SaaS uygulamalarına Azure AD otomatik kullanıcı sağlamanın daha hızlı performansı

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** 3.
 
Daha önce, SaaS uygulamaları için Azure Active Directory kullanıcı sağlama bağlayıcılarını kullanan müşteriler (örneğin Salesforce, ServiceNow ve Box), Azure AD kiracıları 100.000'den fazla birleştirilmiş kullanıcı ve grup içeriyorsa ve hangi kullanıcıların sağlanması gerektiğini belirlemek için kullanıcı ve grup atamalarını kullanıyorsa yavaş performans yaşayabilir.

2 Nisan 2018'de, Azure Active Directory ve hedef SaaS uygulamaları arasında ilk eşitlemeleri gerçekleştirmek için gereken süreyi büyük ölçüde azaltan Azure AD sağlama hizmetine önemli performans geliştirmeleri dağıtıldı.

Sonuç olarak, birkaç gün süren veya hiç tamamlanmamış uygulamalara ilk senkronizasyonları olan birçok müşteri, artık birkaç dakika veya saat içinde tamamlanmaktadır.

Daha fazla bilgi için [bkz.](/azure//active-directory/app-provisioning/how-provisioning-works)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Karma Azure AD için Windows 10 kilit ekranından otomatik hizmet parola sıfırlama makineleri katıldı

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Self Servis Şifre Sıfırlama  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Windows 10 SSPR özelliğini, karma Azure AD'si birleştirilmiş makineleriçin desteği içerecek şekilde güncelledik. Bu özellik Windows 10 RS4'te mevcuttur ve kullanıcıların parolalarını bir Windows 10 makinesinin kilit ekranından sıfırlamalarına olanak tanır. Self servis parola sıfırlama için etkinleştirilen ve kaydolan kullanıcılar bu özelliği kullanabilir.

Daha fazla bilgi için [giriş ekranından Azure AD parola sıfırlama bilgisine](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows)bakın.

---

## <a name="march-2018"></a>Mart 2018
 
### <a name="certificate-expire-notification"></a>Sertifika süresi sona erme bildirimi

**Türü:** Sabit  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso
 
Azure AD, galeri veya galeri dışı uygulama sertifikasının süresi dolmak üzereyken bir bildirim gönderir. 

Bazı kullanıcılar SAML tabanlı tek oturum açma için yapılandırılan kurumsal uygulamalar için bildirim almadı. Bu sorun çözüldü. Azure AD, 7, 30 ve 60 gün içinde süresi dolan sertifikalar için bildirim gönderir. Bu olayı denetim günlüklerinde görebilirsiniz. 

Daha fazla bilgi için bkz.

- [Azure Active Directory'de federe tek oturum açma sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory portalındaki denetim etkinliği raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C'de Twitter ve GitHub kimlik sağlayıcıları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** B2C - Tüketici Kimlik Yönetimi  
**Ürün kapasitesi:** B2B/B2C
 
Artık Azure AD B2C'de kimlik sağlayıcısı olarak Twitter veya GitHub ekleyebilirsiniz. Twitter genel önizleme GA hareket ediyor. GitHub genel önizlemede yayınlanıyor.

Daha fazla bilgi için Azure [AD B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>iOS ve Android için Azure AD uygulama tabanlı Koşullu Erişim ile Intune Yönetilen Tarayıcı'yı kullanarak tarayıcı erişimini kısıtlama

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
**Şimdi genel önizleme!**

**Intune Yönetilen Tarayıcı SSO:** Çalışanlarınız, Azure AD'ye bağlı tüm uygulamalar için yerel istemciler (Microsoft Outlook gibi) ve Intune Yönetilen Tarayıcı arasında tek oturum açma kullanabilir.

**Intune Yönetilen Tarayıcı Koşullu Erişim Desteği:** Artık çalışanların Uygulama Tabanlı Koşullu Erişim ilkelerini kullanarak Intune Yönetilen tarayıcıyı kullanmasını gerektirebilirsiniz.

Blog [yazımızda](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/)bu konuda daha fazla bilgi edinin.

Daha fazla bilgi için bkz.

- [Kurulum uygulama tabanlı Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Yönetilen tarayıcı ilkelerini yapılandırma](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>PowerShell GA Modülünde Uygulama Proxy Cmdlets

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Erişim Kontrolü
 
Uygulama Proxy cmdlets desteği powershell GA Modülü artık! Bu powershell modülleri üzerinde güncel kalmak gerektirir - bir yıldan fazla geride kalırsanız, bazı cmdlets çalışmayı durdurabilir. 

Daha fazla bilgi için [AzureAD'a](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0)bakın.
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 yerel istemcileri etkileşimli olmayan bir protokol kullanarak Seamless SSO tarafından desteklenir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Office 365 yerel istemcilerini (sürüm 16.0.8730.xxxx ve üzeri) kullanan kullanıcı, Kesintisiz SSO kullanarak sessiz bir oturum açma deneyimi yaşar. Bu destek, Azure AD'ye etkileşimli olmayan bir protokol (WS-Trust) eklenmesiyle sağlanır.

Daha fazla bilgi için, [Sorunsuz SSO ile yerel bir istemcide oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work) nın nasıl çalıştığını görün?
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Bir uygulama Azure AD'nin kiracı uç noktalarına oturum açma istekleri gönderirse, kullanıcılar Sorunsuz SSO ile sessiz oturum açma deneyimi yaşarlar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Kullanıcılar, bir `https://contoso.sharepoint.com`uygulama (örneğin, ) Azure AD'nin kiracı uç noktalarına (yani `https://login.microsoftonline.com/contoso.com/<..>` `https://login.microsoftonline.com/<tenant_ID>/<..>` Azure AD'nin ortak bitiş noktası ( yerine)`https://login.microsoftonline.com/common/<...>`oturum açma istekleri gönderirse, Sorunsuz SSO ile sessiz bir oturum açma deneyimi yaşarlar.

Daha fazla bilgi için Azure [Active Directory Seamless Tek Oturum Açma'ya](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)bakın. 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Sorunsuz SSO'yu kullanıma çıkarmak için kullanıcıların Intranet bölge ayarlarına daha önce iki URL yerine yalnızca bir Azure REKLAM URL'si eklemeniz gerekiyor

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Sorunsuz SSO'yu kullanıcılarınıza kullanıma sunması için, Active Directory'de grup ilkesini kullanarak kullanıcıların Intranet `https://autologon.microsoftazuread-sso.com`bölge ayarlarına yalnızca bir Azure REKLAM URL'si eklemeniz gerekir: . Daha önce, müşterilerin iki URL eklemesi gerekiyordu.

Daha fazla bilgi için Azure [Active Directory Seamless Tek Oturum Açma'ya](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)bakın. 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde bulunan yeni Federe Uygulamalar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Mart 2018'de, Federasyon desteğine sahip bu 15 yeni uygulamayı uygulama galerimizle ekledik:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Asistan FirstAgenda tarafından, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Genlik](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech Dijital Kurumsal Sunucu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Bulut](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın.

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)bkz. 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Azure Kaynakları için PIM genellikle kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi
 
Dizin rolleri için Azure AD Ayrıcalıklı Kimlik Yönetimi'ni kullanıyorsanız, artık ABONELIKler, Kaynak Grupları, Sanal Makineler ve Azure Kaynak Yöneticisi tarafından desteklenen diğer kaynaklar gibi Azure Kaynak rolleri için PIM'nin zamana bağlı erişim ve atama özelliklerini kullanabilirsiniz. Rolleri Tam Zamanında etkinleştirirken Çok Faktörlü Kimlik Doğrulaması'nı uygulayın ve etkinleştirmeleri onaylı değişiklik pencereleriyle eşgüdüm halinde zamanlayın. Buna ek olarak, bu sürüm, güncelleştirilmiş bir bilgi işi, onay iş akışları ve yakında sona erecek rolleri uzatma ve süresi dolan rolleri yenileme olanağı da dahil olmak üzere genel önizleme sırasında kullanılamayan geliştirmeler ekler.

Daha fazla bilgi [için Azure kaynakları için PIM'ye bakın (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Uygulama belirteçlerinize İsteğe Bağlı Talepler Ekleme (genel önizleme)

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Azure AD uygulamanız artık JWT'lerde veya SAML belirteçlerinde özel veya isteğe bağlı talepler talep edebilir.  Bunlar, boyut veya uygulanabilirlik kısıtlamaları nedeniyle belirteci varsayılan olarak dahil olmayan kullanıcı veya kiracı hakkındaki taleplerdir.  Bu, şu anda v1.0 ve v2.0 uç noktalarındaki Azure AD uygulamaları için genel önizlemededir.  Hangi taleplerin eklenebilir ve bunları istemek için başvuru bildiriminizi nasıl dizeeceğiniz hakkında bilgi almak için belgelere bakın.  

Daha fazla bilgi için Azure [AD'deki Isteğe bağlı taleplere](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims)bakın.
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD, daha güvenli OAuth akışları için PKCE'yi destekler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Azure AD dokümanları, OAuth 2.0 Yetkilendirme Kodu hibe akışı sırasında daha güvenli iletişim sağlayan PKCE desteğine destek vermek üzere güncelleştirildi.  Hem S256 hem de düz metin code_challenges v1.0 ve v2.0 uç noktalarında desteklenir. 

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code) 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-get_workers-api"></a>İş Günü Get_Workers API'de bulunan tüm kullanıcı öznitelik değerlerini sağlama desteği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Sağlama  
**Ürün kapasitesi:** 3.
 
İş Günü'nden Etkin Dizin ve Azure AD'ye gelen sağlamanın genel önizlemesi, artık İş Günü Get_Workers API'de bulunan tüm öznitelik değerlerini ayıklama ve sağlama olanağını destekler. Bu, İş Günü gelen sağlama bağlayıcısının ilk sürümüyle gönderilenlerin ötesinde yüzlerce ek standart ve özel öznitelik için destek ekler.

Daha fazla bilgi için bkz: [İş Günü kullanıcı öznitelikleri listesini özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Grup üyeliğini dinamikten statike değiştirme ve bunun tersi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün kapasitesi:** Işbirliği
 
Bir grupta üyeliğin nasıl yönetildiğini değiştirmek mümkündür. Bu, aynı grup adını ve kimliğini sistemde tutmak istediğinizde yararlıdır, bu nedenle gruba yapılan varolan başvurular hala geçerlidir; yeni bir grup oluşturmak için bu başvuruların güncelleştirilmesi gerekir.
Bu işlevselliği desteklemek için Azure AD Yöneticisi merkezini güncelledik. Artık müşteriler varolan grupları dinamik üyelikten atanmış üyeliğe ve tam tersi bir üyeliğe dönüştürebilir. Mevcut PowerShell cmdlets de hala mevcuttur.

Daha fazla bilgi için Azure [Active Directory'deki gruplar için Dinamik üyelik kurallarına](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership) bakın

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Dikişsiz SSO ile geliştirilmiş oturum açma davranışı

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Daha önce, kullanıcılar Azure AD tarafından güvenli bir uygulamadan açıkça oturum açmış olsalar bile, etki alanlarının birleştiği aygıtlardan kendi alanlarındaki bir Azure REKLAM uygulamasına yeniden erişmeye çalışıyorlarsa, Sorunsuz SSO'yu kullanarak otomatik olarak oturum açmış olurlar. Bu değişiklikle, oturum dışı oturum açma desteklenir.  Bu, kullanıcıların Sorunsuz SSO kullanılarak otomatik olarak oturum açmaları yerine oturum açmaları için aynı veya farklı Azure REKLAM hesabını seçmelerine olanak tanır.

Daha fazla bilgi için Azure [Active Directory Seamless Tek Oturum Açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Uygulama Proxy Bağlayıcı Sürüm 1.5.402.0 Yayınlandı

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması
 
Bu bağlayıcı sürümü yavaş yavaş Kasım boyunca dışarı haddelenmiş ediliyor. Bu yeni bağlayıcı sürümü aşağıdaki değişiklikleri içerir:

- Bağlayıcı şimdi alt etki alanı düzeyi yerine etki alanı düzeyinde çerezleri ayarlar. Bu, daha sorunsuz bir SSO deneyimi sağlar ve gereksiz kimlik doğrulama istemlerini önler.
- Yığınlı kodlama istekleri için destek
- Geliştirilmiş konektör sistem durumu izleme 
- Çeşitli hata düzeltmeleri ve kararlılık iyileştirmeleri

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors)
 
---

## <a name="february-2018"></a>Şubat 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Kullanıcıları ve grupları yönetmek için geliştirilmiş gezinme

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Dizin Yönetimi  
**Ürün kapasitesi:** Dizin

Kullanıcıları ve grupları yönetmek için gezinme deneyimi kolaylaştırıldı. Artık dizin genel bakışından silinen kullanıcılar listesine daha kolay erişebilmek için doğrudan tüm kullanıcıların listesine gidebilirsiniz. Ayrıca, grup yönetimi ayarlarına daha kolay erişim le doğrudan dizin genel bakışından tüm grupların listesine gidebilirsiniz. Ayrıca dizin genel bakış sayfasından, bir kullanıcı, grup, kurumsal uygulama veya uygulama kaydı arayabilirsiniz. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>21Vianet (Azure China 21Vianet) tarafından işletilen Microsoft Azure'da oturum açma ve denetim raporlarının kullanılabilirliği

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure Yığını  
**Ürün kapasitesi:** İzleme & Raporlama

Azure REKLAM Etkinliği günlük raporları artık 21Vianet (Azure China 21Vianet) örnekleri tarafından işletilen Microsoft Azure'da kullanılabilir. Aşağıdaki günlükler dahildir:

- **Oturum açma etkinlik günlükleri** - Kiracınızla ilişkili tüm oturum açma günlüklerini içerir.

- **Self servis Şifre Denetim Günlükleri** - Tüm SSPR denetim günlüklerini içerir.

- **Dizin Yönetimi Denetimi günlükleri** - Kullanıcı yönetimi, Uygulama Yönetimi ve diğerleri gibi dizin yönetimiyle ilgili tüm denetim günlüklerini içerir.

Bu günlüklerle, ortamınızın nasıl olduğu hakkında bilgi edinebilirsiniz. Sağlanan verilerle:

- Uygulamalarınızın ve hizmetlerinizin kullanıcılarınız tarafından nasıl kullanılacağını belirleyin.

- Kullanıcılarınızın işlerini halletmesini engelleyen sorunları giderin.

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için [Azure Etkin Dizin raporlaması](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)'na bakın.

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Azure REKLAM Etkinlik Raporlarını görüntülemek için "Rapor Okuyucu" rolünü (yönetici olmayan rol) kullanma

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** İzleme & Raporlama

Yönetici olmayan rollerin Azure AD etkinlik günlüklerine erişmesini sağlamak için müşteri geri bildirimlerinin bir parçası olarak, "Rapor Okuyucu" rolünde olan kullanıcıların Azure portalındaki Oturum Açma ve Denetleme etkinliklerine erişmesinin yanı sıra Microsoft Graph API'sini kullanma olanağı sağladık. 

Daha fazla bilgi için, bu raporların nasıl kullanılacağını öğrenmek için [Azure Etkin Dizin raporlaması'na](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)bakın. 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak kullanılabilir EmployeeID talebi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** Sso
 
**EmployeeID'yi,** Kurumsal uygulama kullanıcı arabirimi'nden SAML tabanlı oturum açma uygulamalarında üye kullanıcılar ve B2B konukları için Kullanıcı tanımlayıcısı ve Kullanıcı özniteliği olarak yapılandırabilirsiniz.

Daha fazla bilgi için, [Azure Etkin Dizini'ndeki kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)ye bakın.

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Azure AD Uygulama Proxy'sinde Joker Karakterleri Kullanarak Basitleştirilmiş Uygulama Yönetimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Kullanıcı Kimlik Doğrulaması
 
Uygulama dağıtımını kolaylaştırmak ve yönetim ek yükünüzü azaltmak için artık joker karakterleri kullanarak uygulamaları yayımlama olanağını destekliyoruz. Joker karakter uygulamasını yayımlamak için standart uygulama yayımlama akışını izleyebilir, ancak dahili ve harici URL'lerde joker karakter kullanabilirsiniz.

Daha fazla bilgi için [Azure Active Directory uygulama proxy'sindeki Joker](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard) uygulamalara bakın

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Uygulama Proxy yapılandırmasını desteklemek için yeni cmdlets

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Platform

AzureAD PowerShell Preview modülünün en son sürümü, müşterilerin PowerShell kullanarak Uygulama Proxy Uygulamalarını yapılandırmasına olanak tanıyan yeni cmdletler içerir.

Yeni cmdlets şunlardır: 

- Al-AzureADApplicationProxyApplication
- Al-AzureADApplicationProxyApplicationConnectorGroup
- AzureADApplicationProxyConnector
- AzureADApplicationProxyConnectorGroup
- AzureADApplicationProxyConnectorGroupMembers
- Al-AzureADApplicationProxyConnectorMemberOf
- Yeni-AzureADApplicationProxyApplication
- Yeni-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Kaldır-AzureADApplicationProxyApplicationConnectorGroup
- Kaldır-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Ayar-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationTekSignon
- Ayar-AzureADApplicationProxyConnector
- Ayar-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Grupların yapılandırmasını destekleyecek yeni cmdletler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulama Proxy  
**Ürün kapasitesi:** Platform

AzureAD PowerShell modülünün en son sürümü, Azure AD'deki grupları yönetmek için cmdletiçerir. Bu cmdletler daha önce AzureADPreview modülünde mevcutdu ve şimdi AzureAD modülüne eklendi

Şu anda Genel Kullanılabilirlik için piyasaya sürülecek Grup cmdletler şunlardır: 

- AzureADMSGroup'u Al
- Yeni AzureADMSGroup
- AzureADMSGroup'u Kaldır
- Ayar-AzureADMSGroup
- AzureADMSGroupYaşam Döngüsü Politikası
- Yeni AzureADMSGroupYaşam Döngüsü Politikası
- Kaldır-AzureADMSGroupLifecyclePolicy
- Ekle-AzureADMSLifecyclePolicyGroup
- Kaldır-AzureADMSLifecyclePolicyGroup
- Sıfırlama-AzureADMSLifeCycleGroup   
- AzureADMSLifecyclePolicyGroup'u Al

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Azure AD Connect'in yeni sürümü kullanılabilir

**Türü:** Yeni özellik  
**Hizmet kategorisi:** AD Senkronizasyonu  
**Ürün kapasitesi:** Platform
 
Azure AD Connect, Azure AD ile Windows Server Active Directory ve LDAP dahil olmak üzere şirket içi veri kaynakları arasında veri eşitlemek için tercih edilen araçtır.

>[!Important]
>Bu yapı şema ve eşitleme kuralı değişiklikleri tanır. Azure AD Connect Eşitleme Hizmeti, yükseltmeden sonra Tam Alma ve Tam Eşitleme adımlarını tetikler. Bu davranışın nasıl değiştirilen hakkında bilgi için [yükseltmeden sonra tam eşitlemeyi nasıl ertelerize edeceğiniz](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade)konusuna bakın.

Bu sürümde aşağıdaki güncelleştirmeler ve değişiklikler vardır:

**Düzeltilen sorunlar**

- Bir sonraki sayfaya geçerken Bölüm Filtreleme sayfası için arka plan görevlerinde zamanlama penceresini düzeltin.

- ConfigDB özel eylemi sırasında Access ihlaline neden olan bir hata düzeltildi.

- SQL bağlantı zaman amıkurtarmak için bir hata düzeltildi.

- SAN joker karakterli sertifikaların önceden req denetiminde başarısız olduğu bir hata düzeltildi.

- AAD bağlayıcısı dışa aktarım sırasında miiserver.exe çökmesine neden olan bir hata düzeltildi.

- Çalışırken DC'de hatalı bir parola denemesinin günlüğe kaydedildiği bir hata düzeltildi AAD connect sihirbazı yapılandırmayı değiştirmesine neden oldu

**Yeni özellikler ve geliştirmeler**
 
- Uygulama telemetrisi - Yöneticiler bu veri sınıfını açma/kapama arasında geçiş yapabilir.

- Azure AD Sağlık verileri - Yöneticiler, sistem durumu ayarlarını kontrol etmek için sistem durumu portalını ziyaret etmelidir. Hizmet ilkesi değiştirildikten sonra, aracılar bunu okur ve uygular.

- Eklenen aygıt geri yapılandırma eylemleri ve sayfa başlatma için bir ilerleme çubuğu.

- ZIP-Text / HTML Raporu'nda HTML raporu ve tam veri toplama ile geliştirilmiş genel tanılama.

- Otomatik yükseltme nin güvenilirliği artırıldı ve sunucunun durumunun belirlenebilmesini sağlamak için ek telemetri eklendi.

- AD Bağlayıcısı hesabında ayrıcalıklı hesaplariçin kullanılabilen izinleri kısıtlayın. Sihirbaz, yeni yüklemeler için MSOL hesabını oluşturduktan sonra ayrıcalıklı hesapların MSOL hesabında sahip olduğu izinleri kısıtlar. Değişiklikler, Otomatik Oluştur hesabıyla ekspres yüklemeleri ve özel yüklemeleri etkiler.

- AADConnect'in temiz kurulumunda SA ayrıcalığı gerektirmeyecek şekilde yükleyici değiştirildi.

- Belirli bir nesne için eşitleme sorunlarını gidermek için yeni yardımcı program. Şu anda, yardımcı program aşağıdaki şeyleri denetler:

    - UserPrincipalName, senkronize edilmiş kullanıcı nesnesi ile Azure AD Kiracı'daki kullanıcı hesabı arasındaki uyuşmazlık.
  
    - Nesne etki alanı filtreleme nedeniyle eşitleme den filtrelenirse
  
    - Kuruluş birimi (OU) filtreleme nedeniyle nesne eşitlemeden filtrelenirse

- Belirli bir kullanıcı hesabı için şirket içi Active Directory'de depolanan geçerli parola karmasını eşitlemek için yeni yardımcı program. Yardımcı program parola değişikliği gerektirmez. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Azure AD uygulama tabanlı Koşullu Erişim ile kullanılmak üzere eklenen Intune Uygulama Koruması ilkelerini destekleyen uygulamalar

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Uygulama tabanlı Koşullu Erişimi destekleyen daha fazla uygulama ekledik. Artık, bu onaylı istemci uygulamalarını kullanarak Office 365 ve Azure AD'ye bağlı diğer bulut uygulamalarına erişebilirsiniz.

Aşağıdaki başvurular Şubat ayı sonuna kadar eklenecektir:

- Microsoft Power BI

- Microsoft Başlatıcısı

- Microsoft Faturalama

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Mobil deneyime kullanım koşulları güncellemesi 

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Uyumlu -luk

Kullanım koşulları görüntülendiğinde, şimdi görüntüleme **de sorun yaşıyor'u tıklatabilirsiniz? Buraya tıklayın**. Bu bağlantıyı tıklattığınızda, cihazınızda doğal olarak kullanım koşulları açılır. Belgedeki yazı tipi boyutundan veya aygıtın ekran boyutundan bağımsız olarak, belgeyi gerektiği gibi yakınlaştırabilir ve okuyabilirsiniz. 

---
 
## <a name="january-2018"></a>Ocak 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD uygulama galerisinde bulunan yeni Federe Uygulamalar 

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal Uygulamalar  
**Ürün kapasitesi:** 3.

Ocak 2018'de federasyon destekli aşağıdaki yeni uygulamalar uygulama galerisine eklendi:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Gizlilik Yönetimi Yazılımı](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk Federe Dizin ve [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın.

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)bkz. 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Ek risk algılandırarak oturum açın

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kimlik Koruması  
**Ürün kapasitesi:** Kimlik Güvenliği & Koruması

Algılanan bir risk algılama için aldığınız bilgiler Azure AD aboneliğinize bağlıdır. Azure AD Premium P2 sürümü yle, temel deki tüm algılamalar hakkında en ayrıntılı bilgilere sahip olursunuz.

Azure AD Premium P1 sürümünde, lisansınızın kapsamında olmayan algılamaları ek risk algılanan risk algılama oturum açma olarak görünür.

Daha fazla bilgi için Azure [Etkin Dizin risk algılamalarına](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events)bakın.
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Office 365 uygulamalarını son kullanıcının erişim panellerinden gizleme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Sso

Artık Office 365 uygulamalarının yeni bir kullanıcı ayarı aracılığıyla kullanıcınızın erişim panellerinde nasıl görünebileceğini daha iyi yönetebilirsiniz. Bu seçenek, yalnızca Office uygulamalarını Office portalında göstermeyi tercih ederseniz, kullanıcının erişim panellerinde uygulama sayısını azaltmak için yararlıdır. Ayar **Kullanıcı Ayarları'nda** bulunur ve etiketlenir, **Kullanıcılar yalnızca Office 365 portalında Office 365 uygulamalarını görebilir.**

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Parola SSO için etkinleştirilen uygulamalarda doğrudan uygulamanın URL'sinden sorunsuz oturum açma 

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Sso

Uygulamalarım tarayıcı uzantısı artık tarayıcınızda bir kısayol olarak Uygulamalarım'ı tek oturum açma özelliğini sağlayan kullanışlı bir araç aracılığıyla kullanılabilir. Yükledikten sonra, kullanıcı tarayıcılarında uygulamalara hızlı erişim sağlayan bir waffle simgesi görür. Kullanıcılar artık şu avantajlardan yararlanabilir:

- Uygulamanın oturum açma sayfasından parola-SSO tabanlı uygulamalarda doğrudan oturum açma özelliği
- Hızlı arama özelliğini kullanarak herhangi bir uygulamayı başlatın
- Uzantıdan son kullanılan uygulamalara kısayollar
- Uzantı Microsoft Edge, Chrome ve Firefox için kullanılabilir.
 
Daha fazla bilgi için [Uygulamalarım Güvenli Oturum Açma Uzantısı'na](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension)bakın.

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure Classic Portal'daki Azure AD yönetim deneyimi kullanımdan kaldırıldı

**Türü:** Kaldırıl -mış   
**Hizmet kategorisi:** Azure AD  
**Ürün kapasitesi:** Dizin

8 Ocak 2018 itibarıyla Azure klasik portalındaki Azure AD yönetimi deneyimi kullanımdan kaldırılmıştır. Bu, Azure klasik portalının emekliliğiyle birlikte gerçekleşti. Gelecekte, Azure AD'nin tüm portal tabanlı yönetiminiz için [Azure AD yönetici merkezini](https://aad.portal.azure.com) kullanmanız gerekir.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor web portalı kullanımdan kaldırıldı

**Türü:** Kaldırıl -mış  
**Hizmet kategorisi:** Azure AD  
**Ürün kapasitesi:** Dizin
 
8 Ocak 2018 tarihi itibariyle PhoneFactor web portalı kullanımdan kaldırılmıştır. Bu portal MFA sunucusunun yönetimi için kullanıldı, ancak bu işlevler portal.azure.com'de Azure portalına taşındı. 

MFA yapılandırması şu adreste dir: **Azure Active Directory \> MFA Server**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlarını amortismana erdir

**Türü:** Kaldırıl -mış  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi  


Yeni Azure Active Directory Administration konsolunun genel kullanılabilirliği ve hem etkinlik hem de güvenlik raporları için kullanılabilen yeni API'ler ile "/reports" bitiş noktası altındaki rapor API'leri 31 Aralık 2017 sonu itibariyle kullanımdan kaldırılmıştır.

**Neler var?**

Yeni yönetici konsoluna geçişin bir parçası olarak, Azure AD Etkinlik Günlüklerini almak için 2 yeni API kullanıma sunulduk. Yeni API seti, daha zengin denetim ve oturum açma etkinlikleri sağlamanın yanı sıra daha zengin filtreleme ve sıralama işlevleri sağlar. Güvenlik raporları aracılığıyla daha önce kullanılabilen verilere artık Microsoft Graph'taki Kimlik Koruması risk algılama ları API'si aracılığıyla erişilebilir.

Daha fazla bilgi için bkz.

- [Azure Active Directory raporlama API'si ile başlayın](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Azure Active Directory Identity Protection ve Microsoft Graph ile başlayın](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Aralık 2017

### <a name="terms-of-use-in-the-access-panel"></a>Erişim Paneli'ndeki kullanım koşulları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Uyumlu -luk
 
Artık Erişim Paneli'ne gidebilir ve daha önce kabul ettiğiniz kullanım koşullarını görüntüleyebilirsiniz.

Şu adımları uygulayın:

1. [MyApps portalına](https://myapps.microsoft.com)gidin ve oturum açın.

2. Sağ üst köşede adınızı seçin ve ardından listeden **Profil'i** seçin. 

3. **Profilinizde**Kullanım **Koşullarını Gözden Geçir'i**seçin. 

4. Artık kabul ettiğiniz kullanım koşullarını gözden geçirebilirsiniz. 

Daha fazla bilgi için [Azure REKLAM kullanım koşulları özelliğine (önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Yeni Azure AD oturum açma deneyimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün kapasitesi:** Kullanıcı kimlik doğrulaması
 
Azure AD ve Microsoft hesap kimlik sistemi UI'leri tutarlı bir görünüme ve hislere sahip olacak şekilde yeniden tasarlandı. Ayrıca, Azure AD oturum açma sayfası önce kullanıcı adını toplar, ardından ikinci ekranda kimlik bilgisi toplanır.

Daha fazla bilgi için bkz. [Yeni Azure AD oturum açma deneyimi artık genel önizlemede.](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/)
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Daha az oturum açma istemi: Azure AD oturum açma için yeni bir "oturum açma" deneyimi

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün kapasitesi:** Kullanıcı kimlik doğrulaması
 
Azure AD oturum açma sayfasındaki beni oturum açma kutusunu **tut,** siz başarılı bir şekilde kimlik doğrulaması yaptıktan sonra görünen yeni bir istemle değiştirildi. 

Bu istem'e **Evet** yanıtı verirseniz, hizmet size kalıcı bir yenileme belirteci verir. Bu davranış, eski deneyimdeki beni tut onay kutusunda **imzala'yı** seçtiğinizzamanki davranışla aynıdır. Federe kiracılar için, bu istek federe hizmet ile başarılı bir şekilde doğrulaması sonra gösterir.

Daha fazla bilgi için daha [az oturum açma istemlerine bakın: Azure AD için yeni "beni oturum açma" deneyimi önizlemede.](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Kullanım koşullarının kabul edilmeden önce genişletilmesini gerektirecek yapılandırma ekleme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Uyumlu -luk
 
Yöneticiler için bir seçenek, kullanıcılarının koşulları kabul etmeden önce kullanım koşullarını genişletmelerini gerektirir.

Kullanıcıların kullanım koşullarını genişletmesini sağlamak için **Aç** veya **Kapalı'yı** seçin. **Ayarı,** kullanıcıların bunları kabul etmeden önce kullanım koşullarını görüntülemelerini gerektirir.

Daha fazla bilgi için [Azure REKLAM kullanım koşulları özelliğine (önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi
 
Kapsamlı etkinleştirme, özgün atama varsayılanlarından daha az özerklikle uygun Azure kaynak rol atamalarını etkinleştirmek için kullanabilirsiniz. Buna bir örnek, kiracınızdaki bir aboneliğin sahibi olarak atanmışsanız. Kapsamlı etkinleştirme ile, abonelik içinde bulunan en fazla beş kaynak (kaynak grupları ve sanal makineler gibi) için sahip rolünü etkinleştirebilirsiniz. Etkinleştirmenizi kapsama getirmek, kritik Azure kaynaklarında istenmeyen değişiklikler gerçekleştirme olasılığını azaltabilir.

Daha fazla bilgi için Azure [AD Ayrıcalıklı Kimlik Yönetimi nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama galerisindeki yeni federe uygulamalar

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün kapasitesi:** 3.

Aralık 2017'de, Federasyon desteği ne sahip bu yeni uygulamaları uygulama galerimizle ekledik:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD entegrasyonu](https://go.microsoft.com/fwlink/?linkid=858027), [Reflve](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO Bambu için çözünürlük GmbH tarafından, SAML SSO](https://go.microsoft.com/fwlink/?linkid=863520) [Bitbucket çözünürlük GmbH tarafından](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD Entegrasyonu.

Uygulamalar hakkında daha fazla bilgi için [Azure Active Directory ile SaaS uygulama tümleştirmesi'ne](https://aka.ms/appstutorial)bakın.

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)bkz. 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD dizini rolleri için onay iş akışları

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi
 
Azure AD dizini rolleri için onay iş akışı genellikle kullanılabilir.

Onay iş akışı yla, ayrıcalıklı rol yöneticileri, ayrıcalıklı rolü kullanabilmeleri için uygun rol üyelerinin rol etkinleştirme talebinde bulunmalarını isteyebilir. Birden çok kullanıcı ve gruba onay sorumlulukları devredilebilir. Uygun rol üyeleri onay bittiğinde ve rolleri etkin olduğunda bildirimler alır.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Geçiş kimlik doğrulaması: Skype for Business desteği

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (Oturum Açmalar)  
**Ürün kapasitesi:** Kullanıcı kimlik doğrulaması

Geçiş kimlik doğrulaması artık çevrimiçi ve hibrit topolojileri içeren modern kimlik doğrulamasını destekleyen Skype for Business istemci uygulamalarında kullanıcı oturum açmalarını destekler. 

Daha fazla bilgi için modern [kimlik doğrulamayla desteklenen Skype for Business topolojilerine](https://technet.microsoft.com/library/mt803262.aspx)bakın.
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Azure RBAC için Azure AD Ayrıcalıklı Kimlik Yönetimi güncelleştirmeleri (önizleme)

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi
 
Azure Role Tabanlı Erişim Denetimi (RBAC) için Azure AD Ayrıcalıklı Kimlik Yönetimi'nin (PIM) genel önizleme yenilemesiyle artık şunları yapabilirsiniz:

* Yeterli yönetim kullanın.
* Kaynak rollerini etkinleştirmek için onay gerektirir.
* Hem Azure AD hem de Azure RBAC rolleri için onay gerektiren bir rolün gelecekteki etkinleştirmesini zamanlayın.
 
Daha fazla bilgi [için Azure kaynakları için Ayrıcalıklı Kimlik Yönetimi (önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)bakın.

---
 
## <a name="november-2017"></a>Kasım 2017
 
### <a name="access-control-service-retirement"></a>Access Control hizmeti emeklilik

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Erişim Denetimi hizmeti  
**Ürün kapasitesi:** Erişim Denetimi hizmeti 

Azure Active Directory Access Control (Access Control hizmeti olarak da bilinir) 2018 sonlarında kullanımdan kaldırılacaktır. Ayrıntılı bir zamanlama ve üst düzey göç kılavuzu içeren daha fazla bilgi önümüzdeki birkaç hafta içinde sağlanacaktır. Access Control hizmeti yle ilgili sorularınız için bu sayfaya yorum bırakabilirsiniz ve bir ekip üyesi bunları yanıtlayacaktır.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Tarayıcı erişimini Intune Yönetilen Tarayıcıya kısıtlama 

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma

Intune Yönetilen Tarayıcı'yı onaylanmış bir uygulama olarak kullanarak tarayıcı erişimini Office 365 ve Diğer Azure AD'ye bağlı bulut uygulamalarına kısıtlayabilirsiniz. 

Artık uygulama tabanlı Koşullu Erişim için aşağıdaki koşulu yapılandırabilirsiniz:

**İstemci uygulamaları:** Tarayıcı

**Değişikliğin etkisi nedir?**

Bugün, bu koşulu kullandığınızda erişim engellenir. Önizleme kullanılabilir olduğunda, tüm erişim yönetilen tarayıcı uygulamasının kullanımını gerektirir. 

Bu yetenek ve yaklaşan bloglar ve sürüm notları daha fazla bilgi arayın. 

Daha fazla bilgi için Azure [AD'de Koşullu Erişim'e](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)bakın.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı Koşullu Erişim için yeni onaylı istemci uygulamaları

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma

Aşağıdaki uygulamalar onaylı istemci [uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesindeyer almaktadır:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- Microsoft StaffHub

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Birden çok dil için kullanım koşulları desteği

**Türü:** Yeni özellik    
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Uyumlu -luk

Yöneticiler artık birden çok PDF belgesi içeren yeni kullanım koşulları oluşturabilir. Bu PDF belgelerini ilgili bir dille etiketleyebilirsiniz. Kullanıcılara tercihlerine göre eşleşen bir dille PDF gösterilir. Eşleşme yoksa, varsayılan dil gösterilir.

---
 
### <a name="real-time-password-writeback-client-status"></a>Gerçek zamanlı parola yazma istemci durumu

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün kapasitesi:** Kullanıcı kimlik doğrulaması

Artık şirket içi parola yazma istemcinizin durumunu gözden geçirebilirsiniz. Bu seçenek, [Parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) sayfasının şirket **içi tümleştirme** bölümünde kullanılabilir. 

Şirket içi geri yazma istemcinizle bağlantınızla ilgili sorunlar varsa, size aşağıdakileri sağlayan bir hata iletisi görürsünüz:

- Şirket içi geri yazma istemcinize neden bağlanamadığınıza ilişkin bilgiler.
- Sorunu çözmede size yardımcı olan belgelere bağlantı. 

Daha fazla bilgi için [şirket içi tümleştirmeye](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration)bakın.

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı Koşullu Erişim 
 
**Türü:** Yeni özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma

Artık, [Azure AD uygulama tabanlı Koşullu Erişim'i](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)kullanarak Office 365 ve diğer Azure AD'ye bağlı bulut uygulamalarına erişimi, Intune uygulama koruma ilkelerini destekleyen onaylanmış istemci [uygulamalarıyla](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview) sınırlandırabilirsiniz. Intune uygulama koruma ilkeleri, bu istemci uygulamalarındaki şirket verilerini yapılandırmak ve korumak için kullanılır.

[Uygulama tabanlı](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) [koşullu](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) erişim ilkeleriyle birleşerek, kişisel ve şirket aygıtları için verileri koruma esnekliğine sahip olabilirsiniz.

Aşağıdaki koşullar ve denetimler artık uygulama tabanlı Koşullu Erişim ile kullanılabilir:

**Desteklenen platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve masaüstü istemcileri

**Erişim denetimi**

- Onaylı istemci uygulaması gerektirir

Daha fazla bilgi için Azure [AD uygulama tabanlı Koşullu Erişim'e](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)bakın.
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure portalında Azure REKLAM aygıtlarını yönetme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Cihaz kaydı ve yönetimi  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma

Artık Azure AD'ye bağlı tüm aygıtlarınızı ve aygıtla ilgili etkinlikleri tek bir yerde bulabilirsiniz. Azure portalındaki tüm cihaz kimliklerinizi ve ayarlarınızı yönetmek için yeni bir yönetim deneyimi vardır. Bu sürümde şunları yapabilirsiniz:

- Azure AD'de Koşullu Erişim için kullanılabilen tüm aygıtlarınızı görüntüleyin.
- Karma Azure AD'ye bağlı aygıtlarınızı içeren özellikleri görüntüleyin.
- Azure AD'ye bağlı aygıtlarınız için BitLocker tuşlarını bulun, cihazınızı Intune ile yönetin ve daha fazlasını yap.
- Azure AD aygıtıyla ilgili ayarları yönetin.

Daha fazla bilgi için Azure [portalını kullanarak cihazları yönet'e](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal)bakın.

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Azure AD Koşullu Erişim için bir aygıt platformu olarak macOS desteği 

**Türü:** Yeni özellik    
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma 

Artık macOS'u Azure AD Koşullu Erişim ilkenize aygıt platformu koşulu olarak ekleyebilir (veya hariç tutabilirsiniz). desteklenen aygıt platformlarına macOS eklenmesiyle şunları yapabilirsiniz:

- **Intune'u kullanarak macOS aygıtlarını kaydedin ve yönetin.** iOS ve Android gibi diğer platformlara benzer şekilde, macOS'un birleşik kayıtlar yapması için bir şirket portalı uygulaması kullanılabilir. MacOS için yeni şirket portalı uygulamasını kullanarak Bir aygıtı Intune'a kaydedebilir ve Azure AD'ye kaydedebilirsiniz.
- **macOS aygıtlarının kuruluşunuzun Intune'da tanımlanan uyumluluk ilkelerine uyduğunu sağlayın.** Azure portalındaki Intune'da artık macOS aygıtları için uyumluluk ilkeleri ayarlayabilirsiniz. 
- **Azure AD'deki uygulamalara erişimi yalnızca uyumlu macOS aygıtlarıyla sınırlandırın.** Koşullu Erişim ilkesi yazma ayrı bir aygıt platformu seçeneği olarak macOS vardır. Artık Azure'da ayarlanan hedeflenen uygulama için MACOS'a özgü Koşullu Erişim ilkeleri yazabilirsiniz.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazları için cihaz uyumluluğu ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD'de Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama için Ağ İlkesi Sunucusu uzantısı 

**Türü:** Yeni özellik    
**Hizmet kategorisi:**  Çok faktörlü kimlik doğrulama  
**Ürün kapasitesi:** Kullanıcı kimlik doğrulaması

Azure Çok Faktörlü Kimlik Doğrulama için Ağ İlkesi Sunucusu uzantısı, varolan sunucularınızı kullanarak kimlik doğrulama altyapınıza bulut tabanlı Çok Faktörlü Kimlik Doğrulama özellikleri ekler. Network Policy Server uzantısı ile, mevcut kimlik doğrulama akışınıza telefon görüşmesi, kısa mesaj veya telefon uygulaması doğrulaması ekleyebilirsiniz. Yeni sunucular yüklemeniz, yapılandırmanız ve bakımını yapmak zorunda değilsiniz. 

Bu uzantı, Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nu dağıtmadan sanal özel ağ bağlantılarını korumak isteyen kuruluşlar için oluşturulmuştur. Ağ İlkesi Sunucusu uzantısı, federe veya senkronize edilmiş kullanıcılar için ikinci bir kimlik doğrulama faktörü sağlamak için RADIUS ve bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması arasında bir bağdaştırıcı görevi görür.

Daha fazla bilgi için bkz: [Mevcut Ağ İlkesi Sunucusu altyapınızı Azure Çok Faktörlü Kimlik Doğrulama ile tümleştirin.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension)
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Silinen kullanıcıları geri yükleme veya kalıcı olarak kaldırma

**Türü:** Yeni özellik    
**Hizmet kategorisi:** Kullanıcı yönetimi  
**Ürün kapasitesi:** Dizin 

Azure AD yönetici merkezinde artık şunları yapabilirsiniz:

- Silinen bir kullanıcıyı geri yükleyin. 
- Bir kullanıcıyı kalıcı olarak silin.

**Denemek için:**

1. Azure AD yönetici **merkezinde, Yönet** bölümündeki [Tüm kullanıcıları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) seçin. 

2. **Göster** listesinden, **Son silinen kullanıcıları**seçin. 

3. Yakın zamanda silinen bir veya daha fazla kullanıcı seçin ve sonra bunları geri yükleyin veya kalıcı olarak silin.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı Koşullu Erişim için yeni onaylı istemci uygulamaları
 
**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma

[Onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesine aşağıdaki uygulamalar eklendi:

- Microsoft Planlayıcısı
- Azure Information Protection 

Daha fazla bilgi için bkz.

- [Onaylı istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Koşullu Erişim ilkesindeki denetimler arasında "VEYA" kullanma 

**Türü:** Değiştirilen özellik    
**Hizmet kategorisi:** Koşullu Erişim  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma
 
Artık Koşullu Erişim denetimleri için "VEYA" (seçili denetimlerden birini gerektirir) kullanabilirsiniz. Bu özelliği, erişim denetimleri arasında "VEYA" içeren ilkeler oluşturmak için kullanabilirsiniz. Örneğin, uyumlu bir aygıtta olmak için Çok Faktörlü Kimlik Doğrulama "VEYA" kullanarak bir kullanıcının oturum açmasını gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için Azure [AD Koşullu Erişim'deki Denetimler'e](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)bakın.
 
---

### <a name="aggregation-of-real-time-risk-detections"></a>Gerçek zamanlı risk tespitlerinin toplanması

**Türü:** Değiştirilen özellik    
**Hizmet kategorisi:** Kimlik koruması  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma

Azure AD Kimlik Koruması'nda, belirli bir günde aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk algılamaları artık her risk algılama türü için toplanır. Bu değişiklik, kullanıcı güvenliğinde herhangi bir değişiklik olmadan gösterilen risk algılamalarının hacmini sınırlar.

Altta yatan gerçek zamanlı algılama, kullanıcı her gün işe yaradığı zaman çalışır. Çok Faktörlü Kimlik Doğrulama veya engelleme erişimi için ayarlanmış bir oturum açma risk güvenlik ilkesiniz varsa, her riskli oturum açma sırasında yine de tetiklenir.
 
---
 
## <a name="october-2017"></a>Ekim 2017

### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlarını amortismana erdir

**Türü:** Değişiklik planı  
**Hizmet kategorisi:** Raporlama  
**Ürün kapasitesi:** Kimlik Yaşam Döngüsü Yönetimi  

Azure portalı size şunları sağlar:

- Yeni bir Azure AD yönetim konsolu.
- Etkinlik ve güvenlik raporları için yeni API'ler.
 
Bu yeni yetenekler nedeniyle, rapor api'leri /reports endpoint altında 10 Aralık 2017 tarihinde kullanımdan kaldırıldı. 

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alanı algılama

**Türü:** Sabit   
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Tek oturum açma  

Azure AD, HTML kullanıcı adı ve parola alanı oluşturan uygulamalar için otomatik oturum açma alanı algılamayı destekler. Bu adımlar, [bir uygulama için oturum açma alanlarını otomatik olarak nasıl yakalayacağınız](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications-problems#manually-capture-sign-in-fields-for-an-app)olarak belgelenmiştir. Bu özelliği, [Azure portalındaki](https://aad.portal.azure.com) **Kurumsal Uygulamalar** sayfasına *Galeri Dışı* bir uygulama ekleyerek bulabilirsiniz. Ayrıca, bu yeni uygulamadaki **Tek Oturum Açma** modunu Parola tabanlı Tek Oturum **Açma,** web URL'si girin ve sayfayı kaydedebilirsiniz.
 
Bir hizmet sorunu nedeniyle, bu işlevsellik geçici olarak devre dışı bırakıldı. Sorun çözüldü ve otomatik oturum açma alanı algılama yeniden kullanılabilir.

---

### <a name="new-multi-factor-authentication-features"></a>Yeni Çok Faktörlü Kimlik Doğrulama özellikleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Çok faktörlü kimlik doğrulama  
**Ürün kapasitesi:** Kimlik güvenliği ve koruma  

Çok faktörlü kimlik doğrulama (MFA), kuruluşunuzun korunmasının önemli bir parçasıdır. Kimlik bilgilerini daha uyumlu ve deneyimi daha sorunsuz hale getirmek için aşağıdaki özellikler eklendi: 

- Çok faktörlü zorluk sonuçları, MFA sonuçlarına programlı erişimi içeren Azure AD oturum açma raporuna doğrudan entegre edilmiştir.
- MFA yapılandırması, Azure portalındaki Azure REKLAM yapılandırma deneyimine daha derinden entegre edilmiştir.

Bu genel önizlemeyle, MFA yönetimi ve raporlama, temel Azure REKLAM yapılandırma deneyiminin tümleşik bir parçasıdır. Artık Azure AD deneyimi kapsamında MFA yönetim portalı işlevselliğini yönetebilirsiniz.

Daha fazla bilgi için Azure [portalında MFA raporlaması için Başvuru bölümüne](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa)bakın. 

---

### <a name="terms-of-use"></a>Kullanım koşulları

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün kapasitesi:** Uyumlu -luk  

Kullanıcılara yasal veya uyumluluk gereksinimleri için ilgili feragatnameler gibi bilgileri sunmak için Azure REKLAM kullanım koşullarını kullanabilirsiniz.

Azure AD kullanım koşullarını aşağıdaki senaryolarda kullanabilirsiniz:

- Kuruluşunuzdaki tüm kullanıcılar için genel kullanım koşulları
- Bir kullanıcının özelliklerine göre belirli kullanım koşulları (örneğin, doktorlar hemşirelere karşı veya dinamik gruplar tarafından yapılan yurtiçi ve uluslararası çalışanlara karşı)
- Salesforce gibi yüksek etkili iş uygulamalarına erişmek için belirli kullanım koşulları

Daha fazla bilgi için [Azure REKLAM kullanım koşullarına](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)bakın.

---

### <a name="enhancements-to-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimine Geliştirmeler

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Ayrıcalıklı Kimlik Yönetimi  
**Ürün kapasitesi:** Ayrıcalıklı Kimlik Yönetimi  

Azure AD Ayrıcalıklı Kimlik Yönetimi ile, kuruluşunuzdaki Azure kaynaklarına (önizleme) erişimi şu şekilde yönetebilir, denetleyebilir ve izleyebilirsiniz:

- Abonelikler
- Kaynak grupları
- Sanal makineler 

Azure RBAC işlevini kullanan Azure portalıiçindeki tüm kaynaklar, Azure AD Ayrıcalıklı Kimlik Yönetimi'nin sunduğu tüm güvenlik ve yaşam döngüsü yönetimi özelliklerinden yararlanabilir.

Daha fazla bilgi [için Azure kaynakları için Ayrıcalıklı Kimlik Yönetimi'ne](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)bakın.

---

### <a name="access-reviews"></a>Erişim gözden geçirmeleri

**Türü:** Yeni özellik  
**Hizmet kategorisi:** İncelemeleri erişin  
**Ürün kapasitesi:** Uyumlu -luk  

Kuruluşlar, grup üyeliklerini ve kurumsal uygulamalara erişimi verimli bir şekilde yönetmek için erişim değerlendirmelerini (önizleme) kullanabilir: 

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler, erişim incelemeleri tarafından sağlanan öngörülere dayanarak konukların erişime devam etmesine izin verip vermemekonusunda etkin bir şekilde karar verebilirler.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için Azure [AD erişim incelemeleri'ne](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview)bakın.

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Üçüncü taraf uygulamaları Uygulamalarım'dan ve Office 365 uygulama başlatıcısından gizleme

**Türü:** Yeni özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün kapasitesi:** Tek oturum açma  

Artık yeni bir **gizleme uygulaması** özelliği aracılığıyla kullanıcıportallarında görünen uygulamaları daha iyi yönetebilirsiniz. Arka uç hizmetleri veya yinelenen kutucuklar ve kullanıcıların uygulama başlatıcılarını yığılmayı durumlarında yardımcı olmak için uygulamaları gizleyebilirsiniz. Geçiş, üçüncü taraf uygulamanın **Özellikler** bölümünde ve **kullanıcıtarafından görünür** olarak etiketlendi mi? Ayrıca PowerShell aracılığıyla programlı bir uygulama gizleyebilirsiniz. 

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app) 


**Neler var?**

 Yeni yönetici konsoluna geçişin bir parçası olarak, Azure AD etkinlik günlüklerini almak için iki yeni API kullanılabilir. Yeni API seti, daha zengin denetim ve oturum açma etkinlikleri sağlamanın yanı sıra daha zengin filtreleme ve sıralama işlevleri sağlar. Güvenlik raporları aracılığıyla daha önce kullanılabilen verilere artık Microsoft Graph'taki Kimlik Koruma Risk Algılamaları API'si aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-identity-manager"></a>Kimlik Yöneticisi için Hotfix

**Türü:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik Yöneticisi  
**Ürün kapasitesi:** Kimlik yaşam döngüsü yönetimi  

Identity Manager 2016 Hizmet Paketi 1 için 25 Eylül 2017 tarihi itibariyle bir hotfix roll-up paketi (build 4.4.1642.0) mevcuttur. Bu toplama paketi:

- Sorunları giderir ve iyileştirmeler ekler.
- Identity Manager 2016 için 4.4.1459.0 oluşturmak üzere tüm Identity Manager 2016 Service Pack 1 güncelleştirmelerinin yerini alacak kümülatif bir güncelleştirmedir. 
- Kimlik Yöneticisi 2016 oluşturmak 4.4.1302.0 gerektirir. 

Daha fazla bilgi için Bkz. [Hotfix rollup paketi (build 4.4.1642.0) Identity Manager 2016 Hizmet Paketi 1 için kullanılabilir.](https://support.microsoft.com/help/4021562) 

---
