---
title: Azure Active Directory yenilikler için arşiv var mı? | Microsoft Docs
description: Etkinliğin 6 ay bu içerik kümesinin bölümünü içeren yeni sürüm notları genel bakışta nedir? 6 ay sonra öğeleri ana makalesinden kaldırılır ve Arşiv makalede yerleştirin.
services: active-directory
author: msmimart
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64229e4433ee029787ebd88eba264833827e706c
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79136474"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Azure Active Directory yenilikler için arşiv var mı?

[Azure Active Directory 'deki yenilikler nelerdir? sürüm notları](whats-new.md) makalesi, son altı aya yönelik güncelleştirmeleri içerir, ancak bu makalede daha eski bilgiler yer alır.

Azure Active Directory yenilikler nelerdir? sürüm notları hakkında bilgi sağlar:

- En son sürümleri
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev
- Değişiklikleri planları

---

## <a name="august-2019"></a>Ağustos 2019

### <a name="enhanced-search-filtering-and-sorting-for-groups-is-available-in-the-azure-ad-portal-public-preview"></a>Azure AD portalında (Genel Önizleme), gruplar için gelişmiş arama, filtreleme ve sıralama kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Azure AD portalındaki gelişmiş gruplar ile ilgili deneyimlerin genel önizleme kullanılabilirliğine duyurmaktan mutluluk duyuyoruz. Bu geliştirmeler, aşağıdakileri sağlayarak grupları ve üye listelerini daha iyi yönetmenize yardımcı olur:

- Gruplar listesinde alt dize arama gibi gelişmiş arama özellikleri.
- Üye ve sahip listelerindeki Gelişmiş filtreleme ve sıralama seçenekleri.
- Üye ve sahip listeleri için yeni arama özellikleri.
- Büyük gruplar için daha doğru grup sayısı.

Daha fazla bilgi için [Azure Portal grupları yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)bölümüne bakın.

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>Yeni özel roller uygulama kayıt yönetimi için kullanılabilir (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** RAPORDAKI  
**Ürün yeteneği:** Access Control

Özel Roller (bir Azure AD P1 veya P2 aboneliği ile kullanılabilir) artık, belirli izinlerle rol tanımları oluşturmanıza ve ardından bu rolleri belirli kaynaklara atamaya izin vererek size ayrıntılı erişim sağlamanıza yardımcı olabilir. Şu anda, uygulama kayıtlarını yönetmek ve ardından rolü belirli bir uygulamaya atamak için izinleri kullanarak özel roller oluşturursunuz. Özel roller hakkında daha fazla bilgi için, bkz. [Azure Active Directory (Önizleme) Içinde özel yönetici rolleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview).

Şu anda görmezseniz ek izinlere veya kaynaklara ihtiyacınız varsa, [Azure geri bildirim sitenize](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) geri bildirimde bulunabilir ve isteğinizi güncelleştirme yol haritemiz olarak ekleyeceğiz.

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>Yeni sağlama günlükleri, uygulama sağlama dağıtımınızı izlemenize ve sorunlarını gidermenize yardımcı olabilir (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Kullanıcı ve grup sağlama dağıtımını izlemenize ve sorunlarını gidermenize yardımcı olacak yeni sağlama günlükleri mevcuttur. Bu yeni günlük dosyaları aşağıdakiler hakkında bilgiler içerir:

- [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial) 'da başarıyla oluşturulan gruplar
- [Amazon Web Services (AWS)](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws) öğesinden alınan roller
- [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) 'den hangi çalışanların içeri aktarılmadığı

Daha fazla bilgi için, [Azure Active Directory portalında (Önizleme) raporları sağlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)konusuna bakın.

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>Tüm Azure AD yöneticileri için yeni güvenlik raporları (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure kaynakları için Yönetilen kimlikler  
**Ürün yeteneği:** Geliştirici deneyimi

Kullanıcı tarafından atanan Yönetilen kimlikler artık sanal makineler ve sanal makine ölçek kümeleri için genel kullanıma sunulmuştur. Bunun bir parçası olarak Azure, Azure AD kiracısında kullanımda olan aboneliğin güvendiği ve bir veya daha fazla Azure hizmet örneğine atanabilecek bir kimlik oluşturabilir. Kullanıcı tarafından atanan Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](https://aka.ms/azuremanagedidentity).

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>Kullanıcılar bir mobil uygulama veya donanım belirteci (genel kullanım) kullanarak parolalarını sıfırlayabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Kuruluşunuz ile bir mobil uygulama kaydeden kullanıcılar artık Microsoft Authenticator uygulamasından bir bildirim seçerek veya mobil uygulamasından veya donanım belirtecinden bir kod girerek kendi parolalarını sıfırlayabilir.

Daha fazla bilgi için bkz. [nasıl çalıştığını öğrenin: Azure AD self servis parola sıfırlama](https://aka.ms/authappsspr). Kullanıcı deneyimi hakkında daha fazla bilgi için bkz. [kendi iş veya okul parolanızı sıfırlama genel bakış](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview).

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET, yerinde senaryolar için MSAL.NET paylaşılan önbelleğini yoksayar

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD kimlik doğrulama kitaplığı (ADAL.NET) sürüm 5.0.0-Preview sürümünden itibaren, uygulama geliştiricileri [Web uygulamaları ve Web API 'leri için hesap başına bir önbellek serileştirmelidir](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api). Aksi takdirde, bazı belirli kullanım durumları ile birlikte, `UserAssertion`[Şirket adına](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow)kullanan bazı senaryolar ayrıcalık yükselmesine neden olabilir. Bu güvenlik açığından kaçınmak için, ADAL.NET artık, için Microsoft kimlik doğrulama kitaplığı için DotNet (MSAL.NET) paylaşılan önbellek için Microsoft kimlik doğrulama kitaplığını yok sayar.

Bu sorun hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik doğrulaması kitaplığı ayrıcalık yükselmesi güvenlik açığı](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Ağustos 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Ağustos 2019 ' de, uygulama galerisine federasyon desteğiyle bu 26 yeni uygulamalar ekledik:

[Cıvic platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [bilitodoks](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [viareport 'ın Inativ Portal (Avrupa)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [deneme](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), [akademik katılım](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial), [öncelik matrisi](https://sync.appfluence.com/pmwebng/), [cousto MySpace](https://cousto.platformers.be/account/login), [danışmanlar](https://uploadcare.com/accounts/signup/), [Carbonite uç noktası](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial) [&trade;](https://portal.deliver.media)yedeklemesi [CPQSync by Cincom](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial) [Chargebee](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial) [Frontline Education](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial) [F5](https://www.f5.com/products/security/access-policy-manager) [ stashcat AD Connect](https://www.stashcat.com), [BLINK](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [renklere göre izleme](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial), [Ara,](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial) [EAB stratejik bakım](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>AzureAD PowerShell ve AzureADPreview PowerShell modüllerinin yeni sürümleri mevcuttur

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Dizinden

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

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Yeni bir kuralı daha kolay ayarlamanıza veya mevcut kuralları değiştirmenize yardımcı olması için Azure portal kullanılabilir olan dinamik grup kuralı Oluşturucusu 'nda bazı Kullanıcı arabirimi geliştirmeleri yaptık. Bu tasarım geliştirmesi, yalnızca bir tane yerine en fazla beş ifadeye sahip kurallar oluşturmanıza olanak sağlar. Ayrıca, kullanım dışı bırakılan cihaz özelliklerini kaldırmak için cihaz özelliği listesini güncelleştirdik.

Daha fazla bilgi için bkz. [dinamik üyelik kurallarını yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership).

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>Yeni Microsoft Graph uygulama izni, erişim gözden geçirmeleri ile kullanıma sunuldu

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** Kimlik yönetimi

Uygulamaların grup üyelikleri ve uygulama atamaları için otomatik olarak erişim gözden geçirmeleri oluşturmasına ve almasına izin veren yeni bir Microsoft Graph uygulama izni (`AccessReview.ReadWrite.Membership`) sunuyoruz. Bu izin, zamanlanmış işleriniz veya Otomasyon kapsamında oturum açmış bir kullanıcı bağlamı gerekmeden kullanılabilir.

Daha fazla bilgi için bkz. [PowerShell blogu ile Microsoft Graph uygulama izinleri kullanarak Azure AD erişim incelemeleri oluşturma örneği](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241).

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Azure AD etkinlik günlükleri artık Azure Izleyici 'de kamu bulutu örnekleri için kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlüklerinin artık Azure Izleyici 'deki kamu bulutu örnekleri için kullanılabilir olduğunu duyurmaktan mutluluk duyuyoruz. Artık [SumoLogic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk)ve [arctıma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight)gibi SIEM araçlarınızla tümleştirebilmek için depolama hesabınıza veya bir olay hub 'ına Azure AD günlükleri gönderebilirsiniz. 

Azure Izleyici 'yi ayarlama hakkında daha fazla bilgi için bkz. Azure [izleyici 'de Azure ad etkinlik günlükleri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations).

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Kullanıcılarınızı yeni, geliştirilmiş güvenlik bilgileri deneyimiyle güncelleştirin

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:**  Kimlik doğrulamaları (oturum açma)   
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

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

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Standartlarına

2 Eylül 2019 ' den itibaren POST yöntemini kullanan kimlik doğrulama istekleri HTTP standartlarına karşı daha kesin olarak onaylanır. Özellikle, boşluklar ve çift tırnak işaretleri (") artık istek formu değerlerinden kaldırılmayacak. Bu değişikliklerin mevcut istemcileri bozmak beklenmez ve Azure AD 'ye gönderilen isteklerin her seferinde güvenilir bir şekilde işlendiğinden emin olmaya yardımcı olur.

Daha fazla bilgi için bkz. [Azure AD bölünmesi değişiklikleri bildirimleri](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored).

---

## <a name="july-2019"></a>Temmuz 2019

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>Değişiklik planı: yalnızca TLS 1,2 ' i destekleyecek uygulama proxy hizmeti güncelleştirmesi

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

En güçlü şifreimizi size sağlamaya yardımcı olmak için, uygulama proxy hizmeti erişimini yalnızca TLS 1,2 protokollerine sınırlandırmaya başlayacağız. Bu sınırlama, başlangıçta zaten TLS 1,2 protokollerini kullanan müşterilere dağıtılır, bu nedenle etkiyi görmezsiniz. TLS 1,0 ve TLS 1,1 protokollerinin kullanım dışı bırakılması, 31 Ağustos 2019 tarihinde tamamlanacaktır. Hala TLS 1,0 ve TLS 1,1 kullanan müşteriler, bu değişikliğe hazırlanmak için gelişmiş bir bildirim alır.

Bu değişiklik boyunca uygulama ara sunucusu hizmetiyle bağlantıyı sürdürmek için, istemci-sunucu ve tarayıcı-sunucu birleşimlerinizin TLS 1,2 kullanacak şekilde güncelleştirildiğinden emin olmanızı öneririz. Ayrıca, uygulama proxy 'Si hizmeti aracılığıyla yayınlanan uygulamalara erişmek için çalışanlarınızın kullandığı tüm istemci sistemlerini dahil ettiğinizden emin olmanızı öneririz.

Daha fazla bilgi için bkz. [Azure Active Directory Içindeki uygulama proxy 'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>Değişiklik planı: uygulama galerisine yönelik tasarım güncelleştirmeleri geliyor

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Yeni Kullanıcı arabirimi değişiklikleri, **Uygulama Ekle** dikey penceresinin Galeri alanından **Ekle** ' nin tasarımına geliyor. Bu değişiklikler, otomatik sağlamayı, OpenID Connect, Security Assertion Markup Language (SAML) ve parola çoklu oturum açmayı (SSO) destekleyen uygulamalarınızı daha kolay bir şekilde bulmanıza yardımcı olur.

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>Değişiklik planı: MFA sunucusu IP adresinin Office 365 IP adresinden kaldırılması

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması

MFA sunucusu IP adresini [Office 365 IP adresi ve URL Web hizmetinden](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service)kaldırıyoruz. Şu anda güvenlik duvarı ayarlarınızı güncelleştirmek için bu sayfalara sahipseniz [azure Multi-Factor Authentication sunucusu](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements) kullanmaya başlama konusunun **Azure Multi-Factor Authentication sunucusu güvenlik duvarı GEREKSINIMLERI** bölümünde belgelenen IP adreslerinin listesini de dahil ettiğinizden emin olmanız gerekir.

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>Yalnızca uygulama belirteçleri, artık istemci uygulamasının kaynak kiracısında var olmasını gerektiriyor

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

26 Temmuz 2019 ' de, [istemci kimlik bilgileri verme](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)yoluyla yalnızca uygulama belirteçleri sağlıyoruz. Daha önce, uygulamalar, istemci uygulamanın kiracıda olup olmamasına bakılmaksızın diğer uygulamaları çağırmak için belirteçleri alabilir. Bu davranışı, bazen Web API 'Leri olarak adlandırılan tek kiracılı kaynakların yalnızca kaynak kiracısında bulunan istemci uygulamaları tarafından çağrılabilir şekilde güncelleştirdik.

Uygulamanız kaynak kiracısında değilse, bu sorunu çözmek Için `The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.` bildiren bir hata iletisi alırsınız., kiracınızın uygulama iznini kiracı içinde çalışmaya vermiş olmasını sağlayan [Yönetici onay uç noktası](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint) ya da [PowerShell aracılığıyla](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell)istemci uygulama hizmeti sorumlusunu oluşturmanız gerekir.

Daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant).

> [!NOTE]
> İstemci ve API arasındaki mevcut onay gerekli olmaya devam eder. Uygulamalar yine de kendi yetkilendirme denetimlerini yapmakta olmalıdır.

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>FIDO2 güvenlik anahtarları kullanılarak Azure AD 'de yeni parolasız oturum açma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD müşterileri artık, kuruluşlarının kullanıcıları ve grupları için FIDO2 güvenlik anahtarlarını yönetmek üzere ilkeler ayarlayabilir. Son kullanıcılar ayrıca güvenlik anahtarlarını kendi kendine kaydedebilir, FIDO özellikli cihazlarda Microsoft hesaplarında oturum açmak için anahtarları kullanabilir ve Azure AD 'ye katılmış Windows 10 cihazlarında oturum açabilirler.

Daha fazla bilgi için bkz. yönetici ile ilgili bilgiler için [Azure AD 'de passwordless oturum açma 'Yı etkinleştirme (Önizleme)](/azure/active-directory/authentication/concept-authentication-passwordless) ve son kullanıcı ile ilgili bilgiler için Güvenlik [anahtarı (Önizleme) kullanmak üzere güvenlik bilgilerini ayarlama](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key) .

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Azure AD Uygulaması galerisinde yeni Federasyon uygulamaları kullanılabilir-Temmuz 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Haziran 2019 ' de, uygulama galerisine federasyon desteği olan bu 18 yeni uygulamayı ekledik:

[Ungerboeck yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial), [parlak model omnichannel iletişim merkezi](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial), [Clever Nelly](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial), [acquireio](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial), [gevop](https://www.looop.co/schedule-a-demo/), [Productboard](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial), [MS Azure SSO, etsx uyumluluk ofisi&trade;](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso), [hype](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial), [soyut](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial), [ascentis](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial), [Flipsnack](https://www.flipsnack.com/accounts/sign-in-sso.html), [wandera](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial), [TwineSocial](https://twinesocial.com/), [kallilük](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial) [,](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/) [hyperanna](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial), ilaç, [IVG](https://www.pharmid.com/) [Artifactory](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

IP adresi ve aralıklarının uzun listesini yönetiyorsanız, Azure AD Domain Services sanal ağ alt ağına gelen trafiğin güvenliğini sağlamaya yardımcı olması için Azure ağ güvenlik grubunuzdaki yeni **AzureActiveDirectoryDomainServices** Network Service etiketini kullanabilirsiniz.

Bu yeni hizmet etiketi hakkında daha fazla bilgi için bkz. [Azure AD Domain Services Için ağ güvenlik grupları](../../active-directory-domain-services/network-considerations.md#network-security-groups-and-required-ports).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Domain Services için yeni güvenlik denetimleri (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Azure AD etki alanı hizmeti güvenlik denetimi 'nin genel önizlemeye sunulduğunu duyurmaktan mutluluk duyuyoruz. Güvenlik denetimi, Azure AD etki alanı hizmeti 'ni kullanarak Azure depolama, Azure Log Analytics çalışma alanları ve Azure Olay Hub 'ı dahil olmak üzere hedeflenen kaynaklara güvenlik denetim olayları akışı yaparak size kimlik doğrulama hizmetlerinize yönelik kritik Öngörüler sağlamanıza yardımcı olur. Portal.

Daha fazla bilgi için bkz. [Azure AD Domain Services Için güvenlik denetimlerini etkinleştirme (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>Yeni kimlik doğrulama yöntemleri kullanım & öngörüleri (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** & Raporlamayı izleme

Yeni kimlik doğrulama yöntemleri kullanım & öngörüleri raporları, Azure Multi-Factor Authentication ve self servis parola sıfırlamasının, kayıtlı sayısı dahil olmak üzere kuruluşunuzda nasıl kaydedildiğini ve kullanıldığını anlamanıza yardımcı olabilir her özelliğe yönelik kullanıcılar, parolaları sıfırlamak için self servis parola sıfırlamasının ne sıklıkta kullanıldığı ve hangi yöntemin sıfırlanacağı.

Daha fazla bilgi için bkz. [kimlik doğrulama yöntemleri kullanımı & Öngörüler (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights).

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>Tüm Azure AD yöneticileri için yeni güvenlik raporları kullanılabilir (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Tüm Azure AD yöneticileri artık riskli **Kullanıcılar** ve **riskli oturum açma** raporlarında gösterildiği gibi yeni güvenlik deneyimini kullanmaya başlamak için, **risk için işaretlenmiş kullanıcılar** raporu gibi mevcut güvenlik raporlarının en üstündeki başlığı seçebilirler. Zaman içinde, tüm güvenlik raporları eski sürümlerden yeni sürümlere taşınır ve yeni raporlar aşağıdaki ek olanakları sağlar:

- Gelişmiş filtreleme ve sıralama

- Eksik Kullanıcı riski gibi toplu eylemler

- Güvenliği aşılmış veya güvenli varlıkların onayı

- Risk durumu, kapsayan: risk, kapatılan, düzeltilen ve onaylanan güvenliği aşılmış

Daha fazla bilgi için bkz. [riskli kullanıcılar raporu](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users) ve [riskli oturum açma bileşenleri raporu](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD Domain Services için yeni güvenlik denetimleri (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Azure AD etki alanı hizmeti güvenlik denetimi 'nin genel önizlemeye sunulduğunu duyurmaktan mutluluk duyuyoruz. Güvenlik denetimi, Azure AD etki alanı hizmeti 'ni kullanarak Azure depolama, Azure Log Analytics çalışma alanları ve Azure Olay Hub 'ı dahil olmak üzere hedeflenen kaynaklara güvenlik denetim olayları akışı yaparak size kimlik doğrulama hizmetlerinize yönelik kritik Öngörüler sağlamanıza yardımcı olur. Portal.

Daha fazla bilgi için bkz. [Azure AD Domain Services Için güvenlik denetimlerini etkinleştirme (Önizleme)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>SAML/WS-beslemesiyle yeni B2B Direct Federation (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

Doğrudan Federasyon, SAML veya WS-Beslikli standartları destekleyen kimlik sistemleriyle çalışarak, BT tarafından yönetilen kimlik çözümü Azure AD olmayan iş ortaklarıyla çalışmanızı kolaylaştırır. Bir iş ortağıyla doğrudan Federasyon ilişkisi ayarladıktan sonra, bu etki alanından davet ettiğiniz tüm yeni Konuk kullanıcıları mevcut kurumsal hesaplarını kullanarak sizinle işbirliği yapabilir ve konuklarınız için Kullanıcı deneyimini daha sorunsuz hale getirir.

Daha fazla bilgi için bkz. [Konuk kullanıcılar için AD FS ve üçüncü taraf sağlayıcılarla doğrudan Federasyon (Önizleme)](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
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

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Artık Azure AD portalından bir grup adı oluşturduğunuzda veya güncelleştirdiğinizde, kaynakta mevcut bir grup adını çoğaltdığınıza ilişkin bir denetim yapacağız. Adın zaten başka bir grup tarafından kullanımda olduğunu tespit ettik, adınızı değiştirmeniz istenir.

Daha fazla bilgi için bkz. [Azure AD portalındaki grupları yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD artık yanıt (yeniden yönlendirme) URI 'Lerinde statik sorgu parametrelerini desteklemektedir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD uygulamaları artık OAuth 2,0 istekleri için statik sorgu parametreleriyle (örneğin, `https://contoso.com/oauth2?idp=microsoft`) Yanıtla (yeniden yönlendirme) URI 'Leri kaydedebilir ve kullanabilir. Statik sorgu parametresi, yanıt URI 'sinin diğer herhangi bir bölümünde olduğu gibi, yanıt URI 'Leri için dize eşlemeye tabidir. URL kodu çözülmüş yeniden yönlendirme URI 'si ile eşleşen kayıtlı bir dize yoksa, istek reddedilir. Yanıt URI 'SI bulunursa, statik sorgu parametresi dahil olmak üzere, kullanıcının yeniden yönlendirileceği tüm dize kullanılır.

Bir güvenlik riskini temsil ettiğinden dinamik yanıt URI 'Leri hala yasaktır ve bir kimlik doğrulama isteği genelinde durum bilgilerini sürdürmek için kullanılamaz. Bu amaçla `state` parametresini kullanın.

Şu anda Azure portal uygulama kayıt ekranları hala sorgu parametrelerini engelliyor. Ancak, uygulamanıza sorgu parametrelerini eklemek ve test etmek için uygulama bildirimini el ile düzenleyebilirsiniz. Daha fazla bilgi için bkz. [kimlik doğrulaması yenilikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters).

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Azure AD için etkinlik günlükleri (MS Graph API 'Leri) artık PowerShell cmdlet 'Leri aracılığıyla kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlüklerinin (denetim ve oturum açma raporları) artık Azure AD PowerShell modülü aracılığıyla kullanılabildiğini duyurmak için heyecanlıyız. Daha önce, MS Graph API uç noktaları kullanarak kendi betiklerinizi oluşturabilir ve şimdi bu özelliği PowerShell cmdlet 'lerine genişlettik.

Bu cmdlet 'leri kullanma hakkında daha fazla bilgi için bkz. [Raporlama Için Azure AD PowerShell cmdlet 'leri](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting).

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Azure AD 'de denetim ve oturum açma günlükleri için filtre denetimleri güncelleştirildi

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Denetim ve oturum açma günlüğü raporlarını, artık rapor ekranlarında sütun olarak eklemek zorunda kalmadan çeşitli filtreler uygulayabilmeniz için güncelleştirdik. Ayrıca, ekranda kaç filtre göstermek istediğinizi de seçebilirsiniz. Bu güncelleştirmeler, raporların daha kolay okunmasını ve gereksinimlerinize göre daha fazla kapsamlanmasını kolaylaştırmak için birlikte çalışır.

Bu güncelleştirmeler hakkında daha fazla bilgi için bkz. [Denetim günlüklerini filtreleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs) ve [oturum açma etkinliklerini filtreleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities).

---

## <a name="june-2019"></a>Haziran 2019

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>Microsoft Graph için yeni riskDetections API (Genel Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Microsoft Graph için yeni riskDetections API 'sini duyurmaktan mutluluk duyuyoruz, artık genel önizlemede. Bu yeni API 'YI, kuruluşunuzun kimlik korumasıyla ilgili Kullanıcı ve oturum açma riski algılamalarının bir listesini görüntülemek için kullanabilirsiniz. Bu API 'yi, algılama türü, durumu, düzeyi ve daha fazlası hakkındaki ayrıntılar dahil, risk algılamalarınızı daha verimli bir şekilde sorgulamak için de kullanabilirsiniz.

Daha fazla bilgi için [risk algılama API 'si başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Haziran 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Haziran 2019 ' de, uygulama galerisine federasyon desteğiyle bu 22 yeni uygulamaları ekledik:

[Azure AD SAML araç seti](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial), [Otsuka Shokai (大塚商会)](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial), [ANAQUA](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial), [Azure VPN istemcisi](https://portal.azure.com/), [expenseın](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial), [Helper Yardımcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial) [](https://proptimise.co.uk/software/), [costpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial), [GlobalOne](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial), [Mercedes-Benz ın-otomobil ofisi](https://me.secure.mercedes-benz.com/), [Skore](https://app.justskore.it/), [Oracle bulut altyapı konsolu](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial), [siark SAML kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial) [, Azure](https://www.scrible.com/sign-in/#/create-account)için [](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial)Oracle Erişim Yöneticisi [](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial) [](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial) Oracle E-Iş paketi için perakende merchandising, Oracle Erişim Yöneticisi, E-Iş paketi için Oracle ıDCS, PeopleSoft için Oracle IDCs, JD Edler için Oracle ıDCS

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Bu yeni desteklenen SaaS uygulamaları için Kullanıcı hesabı sağlamayı otomatikleştirin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Artık yeni tümleştirilmiş uygulamalar için Kullanıcı hesapları oluşturmayı, güncellemeyi ve silmeyi otomatik hale getirebilirsiniz:

- [Yakınlaştırma](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Haberci](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4.](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

Otomatik Kullanıcı hesabı sağlamayı kullanarak kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>Azure AD sağlama hizmeti 'nin gerçek zamanlı ilerlemesini görüntüleme

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Azure AD sağlama deneyimini, Kullanıcı sağlama sürecinde ne kadar uzakta olduğunu gösteren yeni bir ilerleme çubuğu içerecek şekilde güncelleştirdik. Bu güncelleştirilmiş deneyim Ayrıca, geçerli döngüyle sağlanan kullanıcı sayısı ve ne kadar Kullanıcı sağlandığını gösteren bilgileri de sağlar.

Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user).

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>Şirket markası artık oturum kapatma ve hata ekranları üzerinde görünüyor

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure AD 'yi, şirket markasının artık oturum açma ve hata ekranlarında, oturum açma sayfasından da görünmesini sağlayacak şekilde güncelleştirdik. Bu özelliği açmak için herhangi bir şey yapmanız gerekmez, Azure AD yalnızca Azure portal **Şirket markası** alanında ayarlamış olduğunuz varlıkları kullanır.

Şirket markanızı ayarlama hakkında daha fazla bilgi için bkz. [kuruluşunuzun Azure Active Directory sayfalarına marka ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>Azure Multi-Factor Authentication (MFA) sunucusu artık Yeni dağıtımlar için kullanılamaz

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** MFA  
**Ürün yeteneği:** Kimlik güvenliği & koruması

1 Temmuz 2019 itibariyle, Microsoft artık Yeni dağıtımlar için MFA sunucusu sunmaz. Kuruluşunuzda Multi-Factor Authentication istemek isteyen yeni müşterilerin artık bulut tabanlı Azure Multi-Factor Authentication kullanması gerekir. MFA sunucusunu 1 Temmuz 'dan önce etkinleştiren müşteriler bir değişiklik görmez. Hala en son sürümü indirebilir, gelecekteki güncelleştirmeleri alabilir ve etkinleştirme kimlik bilgileri oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication sunucusu](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)kullanmaya başlama. Bulut tabanlı Azure Multi-Factor Authentication hakkında daha fazla bilgi için bkz. [bulut tabanlı azure Multi-Factor Authentication dağıtımı planlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="may-2019"></a>Mayıs 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Hizmet değişikliği: uygulama proxy 'Si hizmetinde yalnızca TLS 1,2 protokolleri için gelecekteki destek

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Müşterilerimiz için en iyi sınıf şifrelemeyi sağlamaya yardımcı olmak için, yalnızca uygulama proxy 'Si hizmetindeki TLS 1,2 protokollerine erişimi sınırlıyoruz. Bu değişiklik, yalnızca TLS 1,2 protokollerini kullanan müşterilere yavaş bir şekilde dağıtılır, bu nedenle herhangi bir değişiklik göremezsiniz.

TLS 1,0 ve TLS 1,1 ' nin kullanımdan kaldırılması 31 Ağustos 2019 tarihinde yapılır, ancak bu değişikliğe hazırlanmak için zaman duyarsınız. Bu değişikliğe hazırlanmak için, kullanıcılarınızın uygulama proxy 'Si aracılığıyla yayınlanan uygulamalara erişmek için kullanacağı istemciler dahil olmak üzere, istemci-sunucu ve tarayıcı-sunucu birleşimlerinizin, uygulamayla bağlantıyı sürdürmek için TLS 1,2 protokolünü kullanacak şekilde güncelleştirildiğinden emin olun. Ara sunucu hizmeti. Daha fazla bilgi için bkz. [Azure Active Directory Içindeki uygulama proxy 'si aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin).

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Uygulamayla ilgili oturum açma verilerinizi görüntülemek için kullanım ve Öngörüler raporunu kullanın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Artık aşağıdakiler hakkında bilgiler de dahil olmak üzere oturum açma verilerinizin uygulama merkezli bir görünümünü almak için Azure portal **Kurumsal uygulamalar** alanında bulunan kullanım ve Öngörüler raporunu kullanabilirsiniz:

- Kuruluşunuz için kullanılan en popüler uygulamalar

- En fazla başarısız oturum açma işlemleri içeren uygulamalar

- Her uygulama için ilk oturum açma hataları

Bu özellik hakkında daha fazla bilgi için [Azure Active Directory portalındaki Kullanım ve Öngörüler raporuna](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report) bakın

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Azure AD kullanarak bulut uygulamalarına Kullanıcı hazırlığını otomatikleştirin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** & Raporlamayı izleme

Aşağıdaki bulut tabanlı uygulamalar için Kullanıcı hesaplarının oluşturulmasını, silinmesini ve güncelleştirilmesini otomatik hale getirmek için Azure AD sağlama hizmetini kullanmak üzere bu yeni öğreticilerini izleyin:

- [Birlikte karşılayın](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [DynamicSignal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [Permi güvenliği](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

Ayrıca, Grup nesneleri sağlama hakkında bilgi sağlayan bu yeni [Dropbox öğreticisini](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial)de izleyebilirsiniz.

Otomatik Kullanıcı hesabı sağlama aracılığıyla kuruluşunuzun daha iyi güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure AD Ile SaaS uygulamalarına Kullanıcı sağlamayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>Kimlik güvenli puanı artık Azure AD 'de kullanılabilir (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Artık Azure AD 'de kimlik güvenli puanı özelliğini kullanarak kimlik güvenliğini izleyip geliştirebilirsiniz. Kimlik güvenli puanı özelliği, size yardımcı olması için tek bir pano kullanır:

- NesneKimliği, 1 ile 223 arasında bir puan temelinde kimlik güvenliği önlemidir.

- Kimlik güvenlik geliştirmelerinizi planlayın

- Güvenlik geliştirmelerinizin başarısını inceleyin

Kimlik güvenliği puanı özelliği hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik güvenli puanı nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score).

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Yeni Uygulama kayıtları deneyimi artık kullanıma sunuldu (genel kullanılabilirlik)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Geliştirici deneyimi

Yeni [uygulama kayıtları](https://aka.ms/appregistrations) deneyimi artık genel kullanıma sunulmuştur. Bu yeni deneyim, Azure portal ve uygulama kayıt Portalı ' ndan bildiğiniz tüm önemli özellikleri içerir ve bunları aracılığıyla geliştirir:

- **Daha iyi uygulama yönetimi.** Uygulamalarınızı farklı portallarda görmek yerine, artık tüm uygulamalarınızı tek bir konumda görebilirsiniz.

- **Basitleştirilmiş uygulama kaydı.** Geliştirilmiş gezinti deneyiminden, yeniden kullanılabilir izin seçimi deneyimine yönelik olarak, uygulamalarınızı kaydetmek ve yönetmek artık daha kolay.

- **Daha ayrıntılı bilgi.** Hızlı başlangıç kılavuzlarınız ve daha fazlası dahil olmak üzere uygulamanız hakkında daha fazla ayrıntı bulabilirsiniz.

Daha fazla bilgi için bkz. [Microsoft Identity platform](https://docs.microsoft.com/azure/active-directory/develop/) ve [uygulama kayıtları deneyimi artık genel kullanıma sunuldu!](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) Blog duyurusu.

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Kimlik koruması için riskli kullanıcılar API 'sinde sunulan yeni yetenekler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Kullanıcıların risk geçmişini almak, riskli kullanıcıları kapatmak ve kullanıcıları tehlikeye atılmak üzere doğrulamak için riskli kullanıcılar API 'sini artık kullanacağınızı duyurmaktan memnuniyet duyuyoruz. Bu değişiklik, kullanıcılarınızın risk durumunu daha verimli bir şekilde güncelleştirmenize ve risk geçmişini anlamanıza yardımcı olur.

Daha fazla bilgi için [riskli KULLANıCıLAR API başvuru belgelerine](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Mayıs 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Mayıs 2019 ' de, uygulama galerisine federasyon desteğiyle bu 21 yeni uygulama ekledik:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [gerçek bağlantılar](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kida](https://app.kianda.com/sso/OpenID/AzureAD/), [basit imza](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [BRAZE](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [displayr](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [templamy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo Sales](https://toutapp.com/login)katılım, [aclp](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [META4 Global HR](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), [hisse çalışma alanı](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [Cobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [WebMethods API bulutu](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [redflag](https://pocketstop.com/redflag/), [whatdüzeltmesini](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [denetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [jobhub](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [neogov](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), [pdee](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial), [myvr](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Azure AD portalında geliştirilmiş Grup oluşturma ve yönetim deneyimleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Azure AD portalındaki gruplarla ilgili deneyimler için geliştirmeler yaptık. Bu geliştirmeler, yöneticilerin grup listelerini, üye listelerini daha iyi yönetmesine ve ek oluşturma seçenekleri sağlamasına imkan tanır.

Geliştirmeler şunlardır:

- Üyelik türüne ve grup türüne göre temel filtreleme.

- Kaynak ve e-posta adresi gibi yeni sütunların eklenmesi.

- Kolay bir şekilde silinmek üzere grupları, üyeleri ve sahip listelerini çoklu seçme özelliği.

- Grup oluşturma sırasında e-posta adresi seçme ve sahipler ekleme özelliği.

Daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Azure AD portalındaki Office 365 grupları için adlandırma ilkesi yapılandırma (genel kullanılabilirlik)

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Yöneticiler artık Azure AD portalını kullanarak Office 365 grupları için bir adlandırma ilkesi yapılandırabilir. Bu değişiklik, kuruluşunuzdaki kullanıcılar tarafından oluşturulan veya düzenlenen Office 365 grupları için tutarlı adlandırma kuralları uygulanmasını sağlar.

Office 365 grupları için adlandırma ilkesini iki farklı yolla yapılandırabilirsiniz:

- Otomatik olarak bir grup adına eklenen ön ekleri veya sonekleri tanımlayın.

- Kuruluşunuz için Grup adlarında izin verilmeyen özelleştirilmiş bir engellenen sözcük kümesini karşıya yükleyin (örneğin, "CEO, bordro, HR").

Daha fazla bilgi için bkz. [Office 365 grupları Için adlandırma Ilkesi zorlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Microsoft Graph API uç noktaları artık Azure AD etkinlik günlükleri (genel kullanılabilirlik) için kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlükleri için Microsoft Graph API uç noktası desteğinin genel kullanıma sunulduğunu duyurmaktan mutluluk duyuyoruz. Bu sürümle birlikte artık hem Azure AD denetim günlüklerinin hem de oturum açma günlüğü API 'Lerinin 1,0 sürümünü kullanabilirsiniz.

Daha fazla bilgi için bkz. [Azure AD denetim günlüğü API 'sine genel bakış](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0).

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Yöneticiler artık Birleşik kayıt işlemi (Genel Önizleme) için koşullu erişimi kullanabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması  

Yöneticiler artık Birleşik kayıt sayfası tarafından kullanılmak üzere koşullu erişim ilkeleri oluşturabilir. Bu, şu durumlarda kayda izin vermek için ilkeleri uygulamayı içerir:

- Kullanıcılar güvenilir bir ağ üzerinde.

- Kullanıcılar, düşük bir oturum açma riskidir.

- Kullanıcılar yönetilen bir cihazlardır.

- Kullanıcılar kuruluşun kullanım koşulları 'nı (TOU) kabul etmiş olursunuz.

Koşullu erişim ve parola sıfırlama hakkında daha fazla bilgi için [Azure AD BIRLEŞIK MFA ve parola sıfırlama kayıt deneyimi blog gönderisine yönelik koşullu erişimi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348)görebilirsiniz. Birleşik kayıt işlemi için koşullu erişim ilkeleri hakkında daha fazla bilgi için bkz. [Birleşik kayıt Için koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration). Azure AD kullanım koşulları özelliği hakkında daha fazla bilgi için bkz. [kullanım koşulları özelliği Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

## <a name="april-2019"></a>Nisan 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>Yeni Azure AD tehdit zekası algılaması artık Azure AD Kimlik Koruması bir parçası olarak sağlanıyor

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Kimlik Koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Azure AD Threat Intelligence algılama artık güncelleştirilmiş Azure AD Kimlik Koruması özelliğinin bir parçası olarak sunulmaktadır. Bu yeni işlevsellik, Microsoft 'un dahili ve dış tehdit bilgileri kaynaklarına dayanan bilinen saldırı desenleriyle tutarlı olan belirli bir kullanıcı veya etkinliğin olağan dışı Kullanıcı etkinliklerini belirtmenize yardımcı olur.

Azure AD Kimlik Koruması yenilenen sürümü hakkında daha fazla bilgi için, bkz. [dört ana Azure AD kimlik koruması geliştirmesi artık genel önizleme](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) bloguna ve [ne Azure Active Directory kimlik koruması (YENİLENDİ)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) makalenin. Azure AD Threat Intelligence algılaması hakkında daha fazla bilgi için [risk algılamalarını Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks) makalesine bakın.

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

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [Hrçalışmalar çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [percogeç](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [Mobıoncontrol](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [Riling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [eğitibrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [rstudio Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba Cloud (rol tabanlı SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [cerkatlanmış varlık yönetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [sectigo Sertifika Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [tarzorbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [Monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [ Gözetimi kurumsal](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [ındiggo](https://indiggolead.com/)

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

Koşullu erişim kullanarak cihazlarınızı ve uygulamalarınızı ayarlama ve yönetme hakkında daha fazla bilgi için bkz. [koşullu erişim ile Cloud App erişimi için yönetilen cihazlar gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) ve [koşullu erişim ile Cloud App erişimi için onaylanan istemci uygulamaları gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Microsoft Edge 'i Microsoft Intune ilkeleriyle kullanarak erişimin nasıl yönetileceği hakkında daha fazla bilgi için, bkz. [Microsoft Intune İlkeyle korunan tarayıcı kullanarak Internet erişimini yönetme](https://docs.microsoft.com/intune/app-configuration-managed-browser).

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

[ISEC7 mobil Exchange temsilcisi](https://www.isec7.com/english/), [mediusflow](https://office365.cloudapp.mediusflow.com/), [eplatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [fulcrud](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [excelityglobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Açıklama tabanlı denetim sistemi](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [yalın](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [powerokul performansı önemli](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [cincode](https://cinode.com/), [Iris intraneti](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [tas](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

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
**Ürün yeteneği:** #

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
**Ürün yeteneği:** #

Artık grup üyeliği veya uygulama ataması için tek bir Azure AD erişim gözden geçirmesine birden çok grup veya uygulama ekleyebilirsiniz. Birden çok grup veya uygulama içeren erişim gözden geçirmeleri aynı ayarlarla ayarlanır ve tüm dahil olan gözden geçirenler aynı anda bildirilir.

Azure AD erişim gözden geçirmeleri kullanarak erişim incelemesi oluşturma hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle grup veya uygulama erişimi Incelemesi oluşturma](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD uygulama galerisinde yeni Federasyon uygulamaları kullanılabilir-Şubat 2019

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Şubat 2019 ' de, uygulama galerisine federasyon desteği olan bu 27 yeni uygulamayı ekledik:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [mindtickalıbı](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT FINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [airstack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [iDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [ufuk ve qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Parlatıdea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [alerar](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight-cloudgate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), izin tıklama, [brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [storegatesmartfile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [pexıp](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [seismik](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial) [Share A Dream](https://www.shareadream.org/how-it-works) [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial) [webMethods Integration Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial) [ Her yerde LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope verileri](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [NetOp Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [Smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [Purecyüksek, Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [tıklama üretkenlik platformu](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

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

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Fize](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [tatatci paleti](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [ın, cloudsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco şemsiye](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet Erişim Yöneticisi](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [süre sonu anımsatıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [EVR Görüntüleyicisi](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [Corptax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [fiil](https://app.verb.net/login), [OPENLATCE](https://openlattice.com/agora), [theorgwiki](https://www.theorgwiki.com/signup), [pavaso Digital Close](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [goodalıştırma araç seti](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [Cloud Service Picco](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [auditboard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [Call](https://webapp.callplease.com/create-account/create-account.html) [GTNEXUS SSO System](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE serviceınsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [Ares for Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [for Office 365](https://www.k2.com/O365), [XLEDGER](https://www.xledger.net/), [idıd Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [highdişli](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Fraz alp](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle csaas platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Yeni Azure AD Kimlik Koruması geliştirmeleri (Genel Önizleme)

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Aşağıda aşağıdakiler dahil olmak üzere Azure AD Kimlik Koruması genel önizleme sunumuna aşağıdaki geliştirmeleri eklediğimiz duyurumuz ediyoruz.

- Güncelleştirilmiş ve daha tümleşik bir kullanıcı arabirimi

- Ek API 'Ler

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

Active Directory Domain Services (AD DS) nesnesi eşitleme kapsamından dışlandıysa ve sonra aşağıdaki eşitleme döngüsündeki Azure AD 'ye geri dönüşüm kutusu 'na taşındıktan sonra bir kullanıcının DirSyncEnabled bayrağının **hatalı olarak yanlış** olarak geçtiğimiz bir hata düzeltildi. Bu düzeltmeyle ilgili bir sonuç olarak, Kullanıcı eşitleme kapsamından dışlandığından ve daha sonra Azure AD geri dönüşüm kutusu 'ndan geri yüklenirse, Kullanıcı hesabı şirket içi AD 'den eşitlenmiş olarak kalır ve bu nedenle, yetki kaynağı (SoA) olarak kaldığı için bulutta yönetilemez. Şirket içi AD.

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
|appliedConditionalAccessPolicies|Bu **Conditionalaccesspolicies** alanıdır. Artık **appliedConditionalAccessPolicies** alanıdır.|düzeltme sınıfı,|düzeltme sınıfı,|
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

- Email

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
**Ürün yeteneği:** #
 
Yöneticiler artık kullanıcılarınızın kiracınızda kullandıkları her cihazda kullanım koşullarınızı kabul etmesini gerektirmek için **kullanıcıların her cihazda izin vermesini iste** seçeneğini açabilir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin cihaz başına kullanım koşulları bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Yöneticiler, yinelenen bir zamanlamaya göre kullanım koşulları 'nı zaman dolacak şekilde yapılandırabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #
 

Yöneticiler artık, belirtilen yinelenen zamanlamanıza bağlı olarak tüm kullanıcılarınız için kullanım süresini dolacak şekilde **süre sonu onayları** seçeneğini açabilir. Zamanlama yıllık, iki yıllık, üç aylık veya aylık olabilir. Kullanım koşulları sona erdiğinde, kullanıcıların yeniden kabul etmesi gerekir.

Daha fazla bilgi için [Azure Active Directory kullanım koşulları özelliğinin kullanım koşulları ekleme bölümüne](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)bakın.

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Yöneticiler, her bir kullanıcının zamanlamasını temel alarak kullanım koşullarını yapılandırabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #

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

[Corestack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetConsent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [GRA-PE](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [ehour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [appınux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [drivedolar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview), [useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [sonsuz kampüs](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [alaya](https://alayagood.com/en/demo/), [heyarkadaş](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [zenegy, Iş Merkezi 365](https://accounting.zenegy.com/), tek [köprü üye portalı](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [ıvanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [allbağlanacak SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [parçalı uygulamalar-klasik test ](https://test.plexonline.com/signon), [Parçalı uygulamalar – klasik](https://www.plexonline.com/signon), [parçalı uygulamalar-UX testi](https://test.cloud.plex.com/sso), [parçalı uygulamalar – UX](https://cloud.plex.com/sso), [parçalı uygulamalar – IAM](https://accounts.plex.com/), [öğelerini-childbakım kayıtları, katılım, & finansal izleme sistemi](https://getcrafts.ca/craftsregistration) 

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

## <a name="october-2018"></a>Ekim 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD günlükleri artık Azure Log Analytics (genel Önizleme) ile çalışma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure Log Analytics için artık Azure AD günlüklerinizi iletebilir duyurmaktan Mutluluk duyuyoruz! Bu en çok istenen özellik analytics'e işletmenizi, işlemleri ve güvenlik yanı sıra altyapınızı izlemeye yardımcı olmak için bir yol için daha iyi erişmesini yardımcı olur. Daha fazla bilgi için bkz. [Azure 'Daki etkinlik günlüklerine Azure Active Directory artık blog Log Analytics](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) .

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Ekim 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Ekim 2018'de Federasyon ile 14 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Ikramiye noktalarım](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), Ambyint, [myworkdrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [ödünç kutusu](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), dialpad, [ON24 sanal ortamı](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [halka orta](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler üç](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [workspot denetimi](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="azure-ad-domain-services-email-notifications"></a>E-posta bildirimleri Azure AD etki alanı Hizmetleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD Domain Services  
**Ürün yeteneği:** Azure AD Domain Services

Azure AD Domain Services yönetilen etki alanınız ile ilgili sorunları veya yanlış yapılandırmalar hakkında Azure portalındaki uyarılar sağlar. Bu uyarılar, desteğe başvurun gerek kalmadan sorunları düzeltmek deneyebilmek adım adım kılavuzlar içerir.

Ekim ayında başlayarak, yönetilen etki alanınız için bildirim ayarlarını özelleştirmek mümkün olacaktır için yeni uyarılar ortaya çıktığında, insanların sürekli güncelleştirmeler için portalı denetleyin gereğini ortadan kaldırır, atanmış bir gruba e-posta gönderilir.

Daha fazla bilgi için [Azure AD Domain Services bildirim ayarları](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications)bölümüne bakın.

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Özel etki alanları silinecek ForceDelete etki alanı API kullanarak azure AD portalı destekler 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Dizin Yönetimi  
**Ürün yeteneği:** Dizinden

Artık ForceDelete etki alanı API başvuruları, kullanıcılar, gruplar ve uygulamalar gibi özel bir etki alanı adınızla (contoso.com) öğesinden geri ilk varsayılan etki alanı adı (zaman uyumsuz olarak yeniden adlandırarak özel etki alanı silmek için kullanabileceğiniz duyurmaktan Mutluluk duyuyoruz contoso.onmicrosoft.com).

Bu değişiklik, özel etki alanı adı kuruluşunuz artık kullanıyorsa veya başka bir Azure AD ile etki alanı adı kullanmanız gerekiyorsa daha hızlı bir şekilde silmek için yardımcı olur.

Daha fazla bilgi için bkz. [özel etki alanı adını silme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>Eylül 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Dinamik gruplar için güncelleştirilmiş yönetici rolü izinleri

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş

Özel yönetici rolleri oluşturabilir ve grubun sahibi olmanıza gerek kalmadan dinamik Üyelik kuralları güncelleştirmek için bir sorunu düzelttik.

Rolü şunlardır:

- Genel yönetici

- Intune Yöneticisi

- Kullanıcı Yöneticisi

Daha fazla bilgi için bkz. [dinamik grup oluşturma ve durumu denetleme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Bazı üçüncü taraf uygulamalar için Basitleştirilmiş Çoklu Oturum Açma (SSO) yapılandırma ayarları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Biz yukarı çoklu oturum açma (SSO) için yazılım (SaaS) hizmet olarak ayarlama uygulamalar her apps yapılandırması benzersiz niteliği nedeniyle zor olabileceğini unutmayın. Aşağıdaki üçüncü taraf SaaS uygulamaları için SSO yapılandırma ayarlarını otomatik olarak doldurmak için Basitleştirilmiş yapılandırma deneyimi oluşturduk:

- Zendesk

- Arcgıs Online

- Jamf Pro

Bu tek tıklamayla deneyim kullanmaya başlamak için, uygulamanın **Azure portal** > **SSO yapılandırma** sayfasına gidin. Daha fazla bilgi için bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Verilerinizin bulunduğu azure Active Directory -? Sayfa

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** GoLocal

Tüm Azure AD Hizmetleri için Azure AD verilerinizi bekleyen Azure veri merkezini görüntülemek için **Azure Active Directory** şirketinizin bölgesini seçin. Tarafından özel bilgileri filtreleyebilirsiniz şirketinizin bölge için Azure AD Hizmetleri.

Bu özelliğe erişmek ve daha fazla bilgi için, bkz. [Azure Active Directory-verileriniz nerede bulunur](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Uygulamalarım Erişim paneli için yeni dağıtım planı kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO

Uygulamalarım erişim paneli (https://aka.ms/deploymentplans)için kullanılabilen yeni dağıtım planına göz atın.
My Apps erişim panelinde bulmak ve uygulamalarını erişmek için tek bir yerde kullanıcılara sağlar. Bu portal, uygulamaları ve gruplara erişim isteği veya diğerleri adına bu kaynaklara erişimi yönetme gibi fırsatlar Self Servis kullanıcılarla da sağlar.

Daha fazla bilgi için bkz. [uygulamalarım portalı nedir?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Azure portal Oturum Açma Günlükleri sayfasında yeni Sorun Giderme ve Destek sekmesi

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

Bu güncelleştirmeyle, artık dinamik Kullanıcı grubu kuralı oluşturucusunun **özel uzantı özellikleri al** bağlantısına tıklayabilir, BENZERSIZ uygulama kimliğinizi girebilir ve kullanıcılar için dinamik üyelik kuralı oluştururken kullanmak üzere özel uzantı özelliklerinin tam listesini alabilirsiniz. Bu liste, bu uygulama için tüm yeni özel uzantı özellikleri almak için aynı zamanda yenilenebilir.

Dinamik üyelik kuralları için özel uzantı özellikleri kullanma hakkında daha fazla bilgi için bkz. [uzantı özellikleri ve özel uzantı özellikleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Aşağıdaki uygulamalar [onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesidir:

- Microsoft To-Do

- Microsoft Stream

Daha fazla bilgi için bkz.

- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Windows 7/8/8.1 Kilit ekranında Self Servis Parola Sıfırlama için yeni destek

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu yeni özelliği ayarladıktan sonra kullanıcılarınız, Windows 7, Windows 8 veya Windows 8.1 çalıştıran bir cihazın **kilit** ekranından parolalarını sıfırlama bağlantısını görür. Bu bağlantıya tıklayarak, kullanıcı, web tarayıcısı gibi aynı parola sıfırlama işlem akışında yönlendirilir.

Daha fazla bilgi için bkz. [Windows 7, 8 ve 8,1 ' de parola sıfırlamayı etkinleştirme](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: Yetkilendirme kodları artık yeniden kullanılamayacak 

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

15 Kasım 2018'de başlayarak, Azure AD uygulamaları için daha önce kullanılan kimlik doğrulama kodlarını kabul durdurur. Bu güvenlik değişiklik v1 ve v2 Uç noktalara zorlanmasını sağlar ve Azure AD OAuth belirtimi ayarlarına uygun olarak çıkarmak yardımcı olur.

Uygulamanız için birden fazla kaynak belirteçlerini almak için yetkilendirme kodları yeniden kullanır, bir yenileme belirteci almak için kodu kullanın ve ardından diğer kaynaklar için ek belirteçlerini almak için yenileme belirtecini kullanmak öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden fazla kaynak arasında birden çok kez kullanılabilir. Kimlik doğrulaması kodu OAuth kod akışı sırasında yeniden başlatmayı deneyen uygulama invalid_grant hata alırsınız.

Bu ve diğer protokollerle ilgili değişiklikler için, [kimlik doğrulaması yenilikleri hakkında tam listeye](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Eylül 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Eylül 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Cokarşılayan işe alma yazılımı](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), ArcGIS [Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [jda Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [kar ke](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), Join.me, [Zephyrsso](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [SILVERBACK](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), riveryatak Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Azure için enlyft SSO, [Palatino,](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial) [dmarcıa](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="support-for-additional-claims-transformations-methods"></a>Ek talep dönüşümü yöntemleri için destek

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

SAML tabanlı **Çoklu oturum açma yapılandırması** sayfasından SAML belirteçlerine uygulanabilen yeni talep dönüştürme yöntemleri (ToLower () ve ToUpper () sunuyoruz.

Daha fazla bilgi için bkz [. Azure AD 'de kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Güncelleştirilmiş SAML tabanlı uygulama yapılandırması kullanıcı arabirimi (önizleme)

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Bizim güncelleştirilmiş SAML tabanlı uygulama yapılandırması kullanıcı Arabirimi bir parçası olarak, elde edecekleriniz:

- SAML tabanlı uygulamalarınızı yapılandırmak için bir güncelleştirilmiş izlenecek deneyimi.

- Eksik veya yanlış yapılandırmanızda sunulanlar hakkında daha fazla görünürlük sağlar.

- Birden çok e-posta adresleri için sona erme sertifika bildirim ekleme yeteneği.

- Yeni Talep dönüştürme yöntemleri ToLower() ve ToUpper() ve daha fazlası.

- Kendi belirteç imzalama sertifikası, Kurumsal uygulamalarınız için karşıya yüklemek için bir yol.

- SAML uygulamaları için Nameıd biçimi yolunu ve dizin uzantılarını olarak Nameıd değeri ayarlamak için bir yol.

Bu güncelleştirilmiş görünümü açmak için **Çoklu oturum açma** sayfasının en üstündeki **Yeni deneyimimizi deneyin** bağlantısına tıklayın. Daha fazla bilgi için bkz. [öğretici: Azure Active Directory olan bir uygulama IÇIN SAML tabanlı çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications).

---

## <a name="august-2018"></a>Ağustos 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP adresi aralıklarındaki değişiklikler

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Platformunun

Daha büyük IP aralıklarını, güvenlik duvarları, yönlendiriciler veya ağ güvenlik grupları, Azure AD IP adresi aralıklarını yapılandırdığınız anlamına güncelleştirmeniz gerekecektir Azure AD'ye kullanıma sunduğumuz. Azure AD yeni uç nokta eklediğinde, güvenlik duvarı, yönlendirici veya ağ güvenlik grupları IP aralığı yapılandırmaları yeniden değiştirmek zorunda kalmamanız için bu güncelleştirmeyi yapıyoruz. 

Ağ trafiği için bu yeni aralıklar sonraki iki ay içinde taşınıyor. Kesintisiz hizmet ile devam etmek için bu güncelleştirilmiş değerleri 10 Eylül 2018'den önce IP adreslerinizi eklemeniz gerekir:

- 20.190.128.0/18 

- 40.126.0.0/18 

Tüm ağ trafiğinizin taşındı kadar yeni aralıklar için eski IP adres aralıklarını kaldırılmıyor kesinlikle öneririz. Taşıma hakkındaki güncelleştirmeler ve eski aralıkları ne zaman kaldırabileceğinizi öğrenmek için bkz. [Office 365 URL 'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Değişiklik bildirimi: Yetkilendirme kodları artık yeniden kullanılamayacak 

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

15 Kasım 2018'de başlayarak, Azure AD uygulamaları için daha önce kullanılan kimlik doğrulama kodlarını kabul durdurur. Bu güvenlik değişiklik v1 ve v2 Uç noktalara zorlanmasını sağlar ve Azure AD OAuth belirtimi ayarlarına uygun olarak çıkarmak yardımcı olur.

Uygulamanız için birden fazla kaynak belirteçlerini almak için yetkilendirme kodları yeniden kullanır, bir yenileme belirteci almak için kodu kullanın ve ardından diğer kaynaklar için ek belirteçlerini almak için yenileme belirtecini kullanmak öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden fazla kaynak arasında birden çok kez kullanılabilir. Kimlik doğrulaması kodu OAuth kod akışı sırasında yeniden başlatmayı deneyen uygulama invalid_grant hata alırsınız.

Bu ve diğer protokollerle ilgili değişiklikler için, [kimlik doğrulaması yenilikleri hakkında tam listeye](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)bakın.
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Self servis parola (SSPR) ve Multi-Factor Authentication (MFA) için yakınsanmış güvenlik bilgileri yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Bu yeni özellik SSPR ve mfa'yı tek bir konum ve deneyimi için güvenlik bilgilerini (örneğin, telefon numarası, mobil uygulama ve benzeri) yönetme kişiler yardımcı olur; kıyasla daha önce burada iki farklı konumlarda bitti dedik.

Bu yakınsanmış deneyimi de SSPR'ı veya mfa'yı kullanan kişiler için çalışır. Ayrıca, kuruluşunuz MFA veya SSPR kayıt zorunlu değil, kişiler uygulamalarım portalında kuruluşunuz tarafından izin verilen tüm MFA veya SSPR güvenlik bilgisi yöntemleri yine de kaydedebilirsiniz.

Bu bir katılım genel önizlemesidir. Yöneticilerin yeni deneyimi (istenirse) bir kiracıdaki tüm kullanıcılar veya seçili bir grup için kapatabilirsiniz. Yakınsama deneyimi hakkında daha fazla bilgi için, bkz. [Yakınsanan deneyim blogu](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD Uygulama Ara Sunucusu uygulamalarında yeni Yalnızca HTTP tanımlama bilgileri ayarı

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Uygulama ara sunucusu uygulamalarınızda **yalnızca http tanımlama bilgileri** adlı yeni bir ayar vardır. Bu ayar, her iki uygulama ara sunucusu erişim ve oturum tanımlama bilgileri HTTP yanıt üst bilgisinde HTTPOnly bayrağı dahil olmak üzere, erişim tanımlama bilgisine istemci tarafı komut dosyasından durduruluyor ve daha da kopyalama gibi eylemleri engelleyen ek güvenlik sağlamaya yardımcı olur veya tanımlama bilgisi değiştiriliyor. Bu bayrak daha önce kullanılmamış olsa da, tanımlama bilgilerinizi her zaman silinmiş şifrelenir ve aktarılan yanlış değişikliklere karşı korumaya yardımcı olmak için bir SSL bağlantısı kullanma.

Bu ayarı, Uzak Masaüstü gibi ActiveX denetimlerini kullanan uygulamalar ile uyumlu değildir. Bu durumda kullanıyorsanız, bu ayarı devre dışı bırakmak öneririz.

Yalnızca HTTP tanımlama bilgileri ayarı hakkında daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Azure kaynakları için Privileged Identity Management (PIM), Yönetim Grubu kaynak türlerini destekler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Just-In-Time etkinleştirme ve atama ayarları şimdi zaten abonelik, kaynak grupları ve kaynakları (örneğin, VM'ler, uygulama hizmetleri ve diğer) için yaptığınız gibi yönetim grubu kaynak türleri için de uygulanabilir. Ayrıca, bir yönetim grubu için yönetici erişimi sağlayan bir rolü olan herkes bulabilir ve bu kaynağın PIM yönetme.

PıM ve Azure kaynakları hakkında daha fazla bilgi için bkz. [Privileged Identity Management kullanarak Azure kaynaklarını bulma ve yönetme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Uygulama erişimi (önizleme) Azure AD portalına daha hızlı erişim sağlar

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Bugün, PIM kullanarak rol etkinleştirme yapılırken, üzerinde etkili olması izinler için 10 dakika sürebilir. Şu anda genel Önizleme aşamasında olan uygulama erişimi kullanmayı tercih ederseniz Yöneticiler etkinleştirme isteği tamamlandıktan hemen sonra Azure AD portalına erişebilir.

Şu anda uygulama erişimi yalnızca Azure kaynakları ve Azure AD portalı deneyiminin destekler. PıM ve uygulama erişimi hakkında daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Ağustos 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Ağustos 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [bridgeline bağlanmamış](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sauce Labs-mobil ve Web testi](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [meta ağlar Bağlayıcısı](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [yaptığımız](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [Promaster (ınlogik tarafından)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [dossıer](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F-harcama raporları](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 canlı sohbet](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [safeconnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [zenqms](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eluminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Yerel Tableau desteği artık Azure AD Uygulama Ara Sunucusu içinde kullanılabilir

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Access Control

Bizim güncelleştirmesi ile Openıd Connect bizim ön kimlik doğrulama protokolü için OAuth 2.0 kodu verme protokolü, artık Tableau uygulaması Ara sunucusu ile kullanılacak tüm ek yapılandırma adımları gerekir. Bu protokol değişiklik uygulama proxy'si, JavaScript ve HTML etiketleri yaygın olarak desteklenen HTTP yönlendirmeleri kullanarak daha modern uygulamalar daha iyi destek de yardımcı olur.

Tableau için yerel destek hakkında daha fazla bilgi için bkz. [Yerel Tableau desteğiyle Azure AD uygulama ara sunucusu](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Azure Active Directory'de B2B konuk kullanıcıları için kimlik sağlayıcısı olarak Google eklemek için yeni destek (Önizleme)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

Google ile bir Federasyon kuruluşunuzdaki ayarlayarak, davet edilen Gmail kullanıcılar oturum paylaşılan uygulamaları ve kişisel bir Microsoft Account (MSA) veya bir Azure AD hesabı oluşturmak zorunda kalmadan, var olan Google hesaplarını kullanarak kaynakları yapmanız gerekiyor.

Bu bir katılım genel önizlemesidir. Google Federasyonu hakkında daha fazla bilgi için bkz. [B2B Konuk kullanıcıları için kimlik sağlayıcısı olarak Google ekleme](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Temmuz 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Azure Active Directory e-posta bildirimleri iyileştirmeleri

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Azure Active Directory (Azure AD) e-postaları artık gönderen e-posta adresi ve gönderen görünen adı, değişiklikleri yanı sıra, güncelleştirilmiş bir tasarım aşağıdaki hizmetlerinden gönderilen zaman özellik:
 
- Azure AD erişim gözden geçirmeleri
- Azure AD Connect Health 
- Azure AD Kimlik Koruması 
- Azure AD Privileged Identity Management
- Kurumsal uygulama süresi dolan sertifika bildirimleri
- Kurumsal uygulama sağlama hizmet bildirimleri
 
E-posta bildirimleri şu e-posta adresinden gönderilir ve görünen ad:

- E-posta adresi: azure-noreply@microsoft.com
- Görünen ad: Microsoft Azure
 
Yeni e-posta tasarımlarının bir örneği ve daha fazla bilgi için bkz. [Azure AD PIM 'de e-posta bildirimleri](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD Etkinlik Günlükleri artık Azure İzleyici'de kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlüklerini Azure İzleyicisi'ni (Azure'nın platform genelinde izleme hizmeti) için genel önizlemede kullanıma sunulmuştur. Azure İzleyici, uzun süreli saklama ve bu geliştirmeler yanı sıra sorunsuz tümleştirme sunar:

- Günlük dosyalarınızın kendi Azure depolama hesabına yönlendirme ile uzun süreli saklama için.

- Yazma veya özel komut dosyaları korumak gerektirmeden sorunsuz SIEM tümleştirmesi.

- Kendi özel çözümler, analiz araçları ve Olay yönetimi çözümleri ile sorunsuz tümleştirme sağlar.

Bu yeni yetenekler hakkında daha fazla bilgi için bkz. Azure [izleyici tanılama 'daki blog Azure ad etkinlik günlükleri artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) ve belgelerimizde, [Azure izleyici 'de etkinlik günlükleri Azure Active Directory (Önizleme)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Azure AD oturum açma raporuna koşullu erişim bilgileri eklendi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Bu güncelleştirme, hangi ilkelerin ilke sonucu ile birlikte bir kullanıcı oturum açtığı zaman değerlendirilen görmenize olanak tanır. Ayrıca, rapor artık eski protokol trafiğini belirleyebilmeniz kullanıcı tarafından kullanılan istemci uygulaması türünü içerir. Rapor girişleri kullanıcıya yönelik hata iletisinde bulunabilir ve belirleme ve giderme eşleşen oturum açma isteği için kullanılan bir bağıntı kimliği için artık aranabilir.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Oturum açma etkinlik günlükleri ile eski kimlik doğrulamalarını görüntüleyin

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme
 
Oturum açma etkinliği günlüklerindeki **Istemci uygulaması** alanının tanıtılmasıyla, müşteriler artık eski kimlik doğrulamaları kullanan kullanıcıları görebilirler. Müşteriler, oturum açma Microsoft Graph API 'sini kullanarak veya Azure AD portalındaki oturum açma etkinlik günlükleri aracılığıyla, eski kimlik doğrulamaları için filtre uygulamak üzere **Istemci uygulaması** denetimini kullanabileceğiniz oturum açma etkinliği günlükleri aracılığıyla bu bilgilere erişebilecektir. Daha fazla ayrıntı için belgeleri gözden geçirin.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Azure AD uygulama galerisinde yeni Federasyon Uygulamaları kullanılabilir - Temmuz 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Temmuz 2018'de Federasyon ile 16 Bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Yenilik hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [belirli yönetici SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), Psuc hazırlama, [IPASS SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [ekran kaydı-O-arada](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), powerokul Birleşik sınıf, [eli ekleme](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [bomgar remote support](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 joinnow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [kanbanize](../saas-apps/kanbanize-tutorial.md), [smartlpa](../saas-apps/smartlpa-tutorial.md), [yetenekler tabanı](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://aka.ms/azureadapprequest)listeleme bölümüne bakın.

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Yeni kullanıcı sağlama SaaS uygulama tümleştirmeleri - Temmuz 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Azure AD, oluşturma, Bakım ve Dropbox, Salesforce, ServiceNow ve diğer SaaS uygulamalarına kullanıcı kimliklerini kaldırılmasını otomatik hale getirmenizi sağlar. Kullanıcı Azure AD uygulama galerisinde şu uygulamalar için destek sağlama Temmuz 2018 tarihinden itibaren ekledik:

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Primi](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Azure AD galerisinde Kullanıcı sağlamayı destekleyen tüm uygulamaların bir listesi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Eşitleme için Connect Health - Yalnız bırakılmış ve çoğaltışmış öznitelik eşitleme hatalarını kolaylıkla düzeltme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** AD Connect  
**Ürün yeteneği:** & Raporlamayı izleme
 
Azure AD Connect Health kendi kendine düzeltme vurgulayın ve eşitleme hatalarını düzeltmeyi yardımcı olması için ortaya çıkarır. Bu özellik, yinelenen öznitelik Eşitleme hataları giderir ve yalnız bırakılmış nesneler, Azure AD'den giderir. Bu tanılama aşağıdaki faydaları sağlar:

- Belirli düzeltmeler sağlama yinelenen öznitelik Eşitleme hataları daraltır

- Azure AD senaryoları, tek bir adımda hatalarını çözümlemek için bir düzeltme uygular

- Yükseltme ya da yapılandırma'yı açmak ve bu özelliği kullanmak için gereklidir

Daha fazla bilgi için bkz. [yinelenen öznitelik eşitleme hatalarını tanılama ve](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors) Düzeltme

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Azure AD ve MSA oturum açma deneyimleri için görsel güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Microsoft online services oturum açma deneyimi için kullanıcı Arabirimi gibi Office 365 ve Azure için güncelleştirdik. Bu değişiklik ekranları, daha az anlaşılamayacak ve daha basit hale getirir. Bu değişiklik hakkında daha fazla bilgi için bkz. [Azure AD oturum açma deneyimi bloguna yönelik yakında iyileştirmeler](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) .

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Yeni Azure AD Connect sürümü - Temmuz 2018

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi

Azure AD Connect'in en son sürümünü içerir: 

- Hata düzeltmeleri ve desteklenebilirliği güncelleştirmeleri 

- Genel kullanılabilirlik Ping federasyona tümleştirme

- En son SQL 2012 istemci güncelleştirmeleri 

Bu güncelleştirme hakkında daha fazla bilgi için bkz [. Azure AD Connect: sürüm yayınlama geçmişi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-end-user-ui"></a>Son Kullanıcı Kullanıcı arabirimi kullanım koşullarına göre güncelleştirmeler

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #

Kabul dize TOU son kullanıcı arabiriminde güncelleştiriyoruz.

**Geçerli metin.** [Kiracıadı] kaynaklara erişmek için kullanım koşullarını kabul etmeniz gerekir.<br>**Yeni metin.** [Kiracıadı] kaynağa erişebilmeniz için kullanım koşullarını okuyun gerekir.

**Geçerli metin:** Kabul etmek için seçilmesi, yukarıdaki kullanım koşullarının tümünü kabul ettiğiniz anlamına gelir.<br>**Yeni metin:** Kullanım koşullarını okuduğunuzu ve anladığınızı onaylamak için lütfen kabul et ' e tıklayın.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Doğrudan kimlik doğrulama eski protokolleri ve uygulamaları destekliyor

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Geçişli kimlik doğrulaması artık eski protokolleri ve uygulamaları destekler. Aşağıdaki sınırlamalar artık tam olarak desteklenir:

- Modern kimlik doğrulaması gerektirmeden eski Office istemci uygulamaları, Office 2010 ve Office 2013, kullanıcı oturum açma işlemleri.

- Takvim Paylaşımı ve ücretsiz/meşgul bilgileri yalnızca karma ortamlarda Office 2010 Exchange erişim.

- Kullanıcı oturum açma işlemleri Skype için modern kimlik doğrulaması gerektirmeden iş istemci uygulamaları için.

- PowerShell sürüm 1.0 için kullanıcı oturum açma işlemleri.

- Apple cihaz kaydı iOS Kurulum Yardımcısı'nı kullanarak programı (Apple DEP). 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Self servis parola sıfırlama ve Multi-Factor Authentication için yakınsanmış güvenlik bilgileri yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu yeni özellik için güvenlik bilgilerini (örneğin, telefon numaranız, e-posta adresi, mobil uygulama ve benzeri) Self Servis parola sıfırlama (SSPR) ve multi-Factor Authentication (MFA) tek bir deneyimle yönetme olanağı sunar. Kullanıcıların SSPR ve MFA için iki farklı deneyimlerinde aynı güvenlik bilgilerini kaydetmek artık gerekir. Bu yeni deneyim, SSPR'ı veya MFA sahip kullanıcılar için de geçerlidir.

Bir kuruluş MFA veya SSPR kaydını zormıyorsa, kullanıcılar kendi güvenlik bilgilerini **uygulamalarım** portalı aracılığıyla kaydedebilir. Burada, kullanıcılar için mfa'yı veya SSPR etkin herhangi bir yöntem kaydedebilirsiniz. 

Bu bir katılım genel önizlemesidir. Yöneticilerin yeni deneyimi (istenirse) seçili bir grup kullanıcı ya da bir kiracıdaki tüm kullanıcılar için etkinleştirebilirsiniz.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Parolanızı sıfırlarken kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanın

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** SSPR  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Bu özellik, bir bildirim veya koddan Microsoft Authenticator'ı (ya da herhangi bir kimlik doğrulayıcı uygulama) kullanarak bir parolayı sıfırlarken kimliklerini doğrulama yönetici olmayanlar olanak sağlar. Yöneticileri açtıktan sonra yöntemi bu Self Servis parola sıfırlama, bir mobil uygulama aka.ms/mfasetup veya aka.ms/setupsecurityinfo aracılığıyla kaydolan kullanıcılar kendi mobil uygulama doğrulama yöntemi olarak, parola sıfırlama sırasında kullanabilirsiniz.

Mobil uygulama bildirimi, parolanızı sıfırlamak için iki yöntem gerektiren bir ilke bir parçası olarak yalnızca açılabilir.

---

## <a name="june-2018"></a>Haziran 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Değişiklik bildirimi: Azure AD etkinlik günlüklerini API'sini kullanarak uygulamalar için Temsilcili yetkilendirme akışı güvenlik düzeltme

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
    >Uygulama izinlerini vermek için genel yönetici olması gerekir.

Daha fazla bilgi için Azure AD Raporlama API 'SI makalesine erişim için önkoşulların [Izin verme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) bölümüne bakın.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>PCI DSS uyumluluğu için Azure AD Hizmetleri bağlanmak için TLS ayarlarını yapılandırma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** yok  
**Ürün yeteneği:** Platformunun

Aktarım Katmanı Güvenliği (TLS) arasında iki iletişim kuran uygulamaları gizlilik ve veri bütünlüğü sağlar ve Günümüzde kullanılan en yaygın olarak dağıtılan güvenlik protokolü bir protokoldür.

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) , uyumluluk **30 Haziran 2018**' den itibaren, yeni ve daha güvenli uygulama PROTOKOLLERININ etkinleştirilmesi için eski TLS ve Güvenli Yuva Katmanı (SSL) sürümlerinin devre dışı bırakılması gerektiğini belirledi. Bu değişiklik, Azure AD hizmetlerine bağlanmak ve PCI DSS uyumluluğu gerektir, TLS 1.0 mamtelemetrydisabled anlamına gelir. TLS birden çok sürümü kullanılabilir ancak TLS 1.2 en son sürümü Azure Active Directory Hizmetleri için kullanılabilir. Hem istemci/sunucu hem de tarayıcı/sunucu birleşimleri için doğrudan TLS 1.2 taşınması önerilir.

Güncel olmayan tarayıcılar, TLS 1.2 gibi yeni TLS sürümlerini desteklemiyor olabilir. Tarayıcı tarafından hangi TLS sürümlerinin desteklendiğini görmek için [Qualys SSL Labs](https://www.ssllabs.com/) sitesine gidin ve **tarayıcınızı test et**' e tıklayın. Öneririz web tarayıcınızın en son sürümüne yükseltin ve tercihen yalnızca TLS 1.2 etkinleştirin.

**TLS 1,2 'yi tarayıcıda etkinleştirmek için**

- **Microsoft Edge ve Internet Explorer (her ikisi de Internet Explorer kullanılarak ayarlanır)**

    1. Internet Explorer 'ı açın, **gelişmiş** > **Internet seçenekleri** > **Araçlar** ' ı seçin.
    2. **Güvenlik** alanında, **TLS 1,2 kullan**' ı seçin ve ardından **Tamam**' ı seçin.
    3. Tüm tarayıcı pencerelerini kapatın ve Internet Explorer'ı yeniden başlatın. 

- **Google Chrome**

    1. Google Chrome ' ı açın, adres çubuğuna *Chrome://Settings/* yazın ve **ENTER**tuşuna basın.
    2. **Gelişmiş** seçenekleri genişletin, **sistem** alanına gidin ve **proxy ayarlarını aç**' ı seçin.
    3. **Internet özellikleri** kutusunda **Gelişmiş** sekmesini seçin, **güvenlik** alanı ' na gidin, **TLS 1,2 kullan**' ı seçin ve ardından **Tamam**' ı seçin.
    4. Tüm tarayıcı pencerelerini kapatın ve Google Chrome'u yeniden başlatın.

- **Mozilla Firefox**

    1. Firefox ' u açın, adres çubuğuna *about: config* yazın ve **ENTER**tuşuna basın.
    2. *TLS*terimini arayın ve ardından **Security. TLS. Version. Max** girişini seçin.
    3. Tarayıcıyı TLS 1,2 sürümüne kadar kullanmaya zorlamak için değeri **3** olarak ayarlayın ve ardından **Tamam**' ı seçin.

        >[!NOTE]
        >Firefox sürüm 60,0, TLS 1,3 ' yi desteklediğinden, Security. TLS. Version. Max değerini **4**olarak da ayarlayabilirsiniz.

    4. Tüm tarayıcı pencerelerini kapatın ve Mozilla Firefox yeniden başlatın.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Haziran 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Haziran 2018'de Federasyon ile 15 bu yeni uygulamalar için uygulama Galerisi desteği ekledik:

[Ufuk dokunma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [müzik kapatma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1,1 belirteci etkin lob uygulaması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [süper ruh](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [otomatik görev](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [uç nokta yedekleme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ufuk yüksek ağlar](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [Tonicdm](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [SharePoint şirket içi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), bir adet [CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [yükselkx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial) [](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial). Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD parola koruması genel önizlemeye sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Ortamınızdaki parolalar kolayca tahmin gidermeye yardımcı olması için Azure AD parola koruması kullanın. Bu parolalar ortadan güvenliğinin aşılması riskine karşı bir saldırı parola ilaç türü riskini azaltmak için yardımcı olur.

Özellikle, Azure AD parola koruması, yardımcı olur:

- Hem Azure AD'de kuruluşunuzun hesapları korumak ve Windows Server Active Directory (AD). 
- Kullanıcılarınızın parolaları listesi en sık kullanılan parolaların 500'den fazla ve bu parolaları 1 milyon karakter değiştirme çözümlenmeyebileceği kullanarak durdurur.
- Her iki Azure AD için Azure AD parola koruması, Azure AD portalında tek bir konumdan yönetmek ve şirket içi Windows Server AD.

Azure AD parola koruması hakkında daha fazla bilgi için bkz. [kuruluşunuzdaki kötü parolaları kaldırma](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında yeni "tüm konuklar" koşullu erişim ilkesi şablonu oluşturuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #

Kullanım koşullarınızın oluşturulması sırasında, "tüm konuklar" ve "tüm uygulamalar" için de yeni bir koşullu erişim ilkesi şablonu oluşturulur. Bu yeni bir ilke şablonu oluşturma ve zorlama işlemi konuklar için hızlandırma, yeni oluşturulan ToU geçerlidir.

Daha fazla bilgi için bkz. [Azure Active Directory kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Kullanım koşulları oluşturma sırasında yeni "özel" koşullu erişim ilkesi şablonu oluşturuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** #

Kullanım koşullarınızın oluşturulması sırasında yeni bir "özel" koşullu erişim ilkesi şablonu da oluşturulur. Bu yeni ilke şablonu, ToU 'yi oluşturmanızı ve ardından doğrudan portalda gezinmenize gerek kalmadan koşullu erişim ilkesi oluşturma dikey penceresine gitmenizi sağlar.

Daha fazla bilgi için bkz. [Azure Active Directory kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication'ı dağıtma hakkında daha fazla yeni ve kapsamlı Kılavuzu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Yeni bir Azure multi-Factor Authentication (MFA) kuruluşunuzda dağıtma hakkında adım adım kılavuz yayımladık.

MFA dağıtım kılavuzunu görüntülemek için GitHub 'daki [kimlik dağıtım kılavuzlarını](https://aka.ms/DeploymentPlans) depoya gidin. Dağıtım kılavuzlarıyla ilgili geri bildirim sağlamak için [dağıtım planı geri bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzlarıyla ilgili sorularınız varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bizimle iletişime geçin.

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD rolleri genel Önizleme aşamasındadır uygulama yönetimi temsilcisi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** Access Control

Yöneticiler, artık genel yönetici rolü atama olmadan uygulama yönetim görevlerini devredebilirsiniz. Yeni rol ve özellikleri şunlardır:

- **Yeni standart Azure AD yönetici rolleri:**

    - **Uygulama Yöneticisi.** Kayıt, SSO ayarlarını, uygulama atamalarını ve lisanslama, uygulama proxy'si ayarları ve onay dahil tüm uygulamalar'ın tüm yönlerini yönetme olanağı veren (Azure AD kaynaklarını hariç).

    - **Bulut uygulaması Yöneticisi.** Şirket içi erişim sağlamaz çünkü uygulama Yöneticisi becerileri, uygulama ara sunucusu hariç tüm verir.

    - **Uygulama geliştiricisi.** **Kullanıcıların uygulama kaydetmesine izin ver** seçeneği devre dışı olsa bile, uygulama kayıtları oluşturma özelliğini verir.

- **Sahiplik (Grup sahipliği işlemine benzer şekilde, uygulama başına kayıt ve kurumsal başına uygulama ayarlayın:**
 
    - **Uygulama kayıt sahibi.** Sahip olunan bir uygulama kaydı, ek sahipleri ekleme ve uygulama bildirimi dahil olmak üzere tüm yönlerini yönetmek için yeteneği verir.

    - **Kurumsal uygulama sahibi.** Sahip olunan Kurumsal uygulamaları, SSO ayarlarını, uygulama atamalarını ve onay dahil olmak üzere birçok yönden yönetme olanağı veren (Azure AD kaynaklarını hariç).

Genel önizleme hakkında daha fazla bilgi için bkz. [Azure AD temsilci uygulama yönetimi rolleri genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) . blog. Roller ve izinler hakkında daha fazla bilgi için bkz. [Azure Active Directory yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Mayıs 2018

### <a name="expressroute-support-changes"></a>ExpressRoute destek değişiklikleri

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Platformunun  

Doğrudan Internet gidip ExpressRoute veya diğer özel VPN tünelleri gerek kalmadan iyi çalışacak şekilde tasarlanan Azure Active Directory (Azure AD) gibi bir hizmet olarak yazılım. Bu nedenle, **1 ağustos 2018**' de Azure genel eşleme ve Microsoft eşleme 'de Azure toplulukları KULLANıLARAK Azure AD Hizmetleri Için ExpressRoute desteğini durduracağız. Bu değişiklikten etkilenen tüm hizmetler Azure fark edebilirsiniz AD trafik ExpressRoute Internet'e giderek kaydırma.

Ayrıca var. biliyoruz ki, destek değiştirmiyorsanız olan gerek duyduğunuz senaryolara hala durumlarda kullanırken devreler adanmış bir dizi, kimlik doğrulama trafiği için. Bu nedenle, Azure AD Kiracı başına ExpressRoute ve Hizmetleri zaten "Diğer Office 365 Çevrimiçi Hizmetleri" toplulukla Microsoft eşlemesi üzerinde kullanarak IP aralığı kısıtlamalarını desteklemeye devam edecektir. Hizmetlerinizi etkilenir, ancak ExpressRoute gerektirir, aşağıdakileri yapmanız gerekir:

- **Azure genel eşleme kullanıyorsanız.** Microsoft eşlemesi ' ne gidin ve **diğer Office 365 Online Services (12076:5100)** topluluğuna kaydolun. Azure genel eşlemeden Microsoft eşlemesine nasıl geçeceğimiz hakkında daha fazla bilgi için bkz. [genel eşlemeyi Microsoft eşlemesi 'Ne taşıma](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) makalesi.

- **Microsoft eşleme kullanıyorsanız.** **Diğer Office 365 çevrimiçi hizmet (12076:5100)** topluluğu için kaydolun. Yönlendirme gereksinimleri hakkında daha fazla bilgi için, ExpressRoute yönlendirme gereksinimleri makalesinin [BGP toplulukları Için destek bölümüne](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) bakın.

Adanmış devreleri kullanmaya devam etmeniz gerekiyorsa, **diğer Office 365 çevrimiçi hizmet (12076:5100)** Community 'yi kullanma yetkisi alma hakkında Microsoft hesabı ekibinizle konuşmanız gerekir. MS Office yönetilen gözden geçirme panonun bu devreler gerekir ve başvurularınızı teknik sonuçlarını anladığınızdan emin olup olmadığını doğrular. Yetkisiz abonelikleri Office 365 için rota filtreleri oluşturulmaya çalışılırken bir hata iletisi alırsınız. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph API'leri için TOU yönetim senaryoları için

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Geliştirici deneyimi
 
Azure AD kullanım koşulları 'nın yönetim işlemi için Microsoft Graph API 'Leri ekledik. Kullanım koşulları nesnesi oluşturabilir, güncelleştirebilir, silebilirsiniz.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Bir Azure AD B2C kimlik sağlayıcısı olarak Azure AD çok kiracılı uç noktası ekleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Özel ilkeleri kullanarak, Azure AD genel uç noktası kimlik sağlayıcısı olarak Azure AD B2C artık ekleyebilirsiniz. Bu, tek bir uygulamalarınızda oturum açtığınız tüm Azure AD kullanıcıları için giriş noktası olmasını sağlar. Daha fazla bilgi için bkz. [Azure Active Directory B2C: kullanıcıların özel ilkeleri kullanarak çok kiracılı bir Azure AD kimlik sağlayıcısında oturum açmalarına Izin verin](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Her yerden uygulamalara erişmek için iç URL'lerini kullanan My Apps oturum açma Uzantımızı ve Azure AD uygulama proxy'si ile

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO
 
Kullanıcılar artık uygulamalar üzerinden şirket ağınıza olduğunda bile dışında İç URL My Apps güvenli oturum açma uzantısı için Azure AD'yi kullanarak erişebilirsiniz. Bu, Azure AD uygulama ara sunucusu, ayrıca erişim paneli tarayıcı uzantısının yüklü olan herhangi bir tarayıcı üzerinde kullanarak yayımladığınız herhangi bir uygulama ile çalışır. URL yeniden yönlendirme işlevi, uzantısı'na bir kullanıcı oturumu sonra otomatik olarak etkinleştirilir. Uzantı [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)ve [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)'ta indirilebilir.

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory - Avrupa müşterilerin Avrupa'daki veri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** GoLocal

Avrupa'da bulunan müşteriler verilerine Avrupa'da kalmasını gerektirir ve toplantı gizliliği ve Avrupa yasaları Avrupa veri merkezleri dışına çoğaltılmamış. Bu [makalede](https://go.microsoft.com/fwlink/?linkid=872328) , Avrupa 'da hangi kimlik bilgilerinin depolanacağı ve ayrıca Avrupa veri merkezleri dışında depolanacak bilgiler hakkındaki ayrıntıları sağlayan belirli ayrıntılar sağlanmaktadır. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>SaaS uygulama tümleştirmeleri - Mayıs 2018 sağlama yeni kullanıcı

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Azure AD, oluşturma, Bakım ve Dropbox, Salesforce, ServiceNow ve diğer SaaS uygulamalarına kullanıcı kimliklerini kaldırılmasını otomatik hale getirmenizi sağlar. Mayıs 2018 için kullanıcı sağlama desteği aşağıdaki uygulamalar Azure AD uygulama galerisinde ekledik:

- [Şeması](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Temel taş OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Azure AD galerisinde Kullanıcı sağlamayı destekleyen tüm uygulamaların listesi için bkz. [https://aka.ms/appstutorial](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Uygulama erişim artık yinelenen incelemeleri sağlayan ve grupları Azure AD erişim gözden geçirmeleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** #
 
Erişim gözden geçirmesi grupları ve uygulamaları şu anda genel kullanımdadır Azure AD Premium P2 bir parçası olarak.  Yöneticiler, erişim gözden geçirmeleri grup üyeliklerini ve uygulama atamaları aylık veya üç aylık gibi düzenli aralıklarla otomatik olarak yinelenmesini yapılandırmanız mümkün olacaktır.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>MS Graph ile Azure AD etkinlik günlüklerini (oturum açma işlemleri ve Denetim) artık kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme
 
Oturum açma işlemlerini ve denetim günlüklerini içeren Azure AD etkinlik günlükleri artık Microsoft Graph API aracılığıyla kullanılabilir. Bu günlüklere erişmek için Microsoft Graph API 'SI aracılığıyla iki uç noktası kullanıma sunduk. Başlamak için Azure AD Raporlama API 'Lerine programlı erişim için [belgelerimizi](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) inceleyin. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>B2B kullanım geliştirmeleri deneyimi ve bir kuruluştan ayrılmak

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

**Tam zamanında kullanım:** B2B API 'sini kullanarak bir kaynağı Konuk kullanıcıyla paylaştığınızda, özel bir davet e-postası göndermeniz gerekmez. Çoğu durumda, Konuk kullanıcı kaynaklara erişebilir ve zamanında kullanım deneyimi üzerinden gerçekleştirilmez. Daha fazla etkisi nedeniyle eksik e-postaları. Konuk kullanıcılarınızı "Bu kullanım bağlantısına tıkladı", sistem sizi gönderdi. Bunun anlamı sonra SPO davet manager'ın kullandığı – cloudy ekleri – iç ve dış – faydalarınız, herhangi bir durumdaki tüm kullanıcılar için kurallı aynı URL'ye sahip olabilir.

**Modern kullanım deneyimi:** Daha fazla bölünmüş ekran ödeme giriş sayfası yok. Kullanıcıların modern görürsünüz üçüncü taraf uygulamaları için yaptıkları gibi davet eden kuruluşunuzun gizlilik bildirimini deneyimle onayı.

**Konuk kullanıcılar kuruluş 'dan bırakabilir:** Bir kullanıcının bir org ile ilişkisi bittikten sonra, kuruluştan ayrılmaları için kendi kendine servis sağlayabilirsiniz. Kuruluşun yöneticisinin "kaldırılması" için davet eden başka hiçbir çağrı yok, daha fazla destek bileti yok.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde - Mayıs 2018

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Mayıs 2018'de uygulama galerimizden Federasyon ile 18 Bu yeni uygulamaları desteği ekledik:

[Awardspring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), ınfogix Data3Sixty yönetme [, yodestesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [JAMF Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [knowledgeowl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi Mmis](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [launchkaranlık](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Capze ana](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [monsat çevrimiçi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial),[まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [yay yayımlama-SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [plangrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [ıwellneskar](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Gözden](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Azure Active Directory için yeni bir adım adım dağıtım kılavuzları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** Dizinden
 
Self servis parola sıfırlama (SSPR), çoklu oturum açma (SSO), koşullu erişim (CA), uygulama proxy, Kullanıcı sağlama, Active Directory Federasyon Hizmetleri (AD FS) (ADFS) dahil olmak üzere Azure Active Directory (Azure AD) dağıtma hakkında yeni, adım adım yönergeler Geçişli kimlik doğrulaması (PTA) ve ADFS 'den Parola karması eşitleme (PHS).

Dağıtım kılavuzlarını görüntülemek için GitHub 'daki [kimlik dağıtım kılavuzlarını](https://aka.ms/DeploymentPlans) depoya gidin. Dağıtım kılavuzlarıyla ilgili geri bildirim sağlamak için [dağıtım planı geri bildirim formunu](https://aka.ms/deploymentplanfeedback)kullanın. Dağıtım kılavuzlarıyla ilgili sorularınız varsa, [IDGitDeploy](mailto:idgitdeploy@microsoft.com)adresinden bizimle iletişime geçin.

---

### <a name="enterprise-applications-search---load-more-apps"></a>Kurumsal uygulamaları arayın: daha fazla uygulama yükleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Hizmet sorumluları / uygulamalarınızı bulma konusunda sorun mu yaşıyorsunuz? Tüm uygulamalar listesini, Kurumsal uygulamalarınızda daha fazla uygulama yükleme olanağı ekledik. Varsayılan olarak 20 uygulamaları göstereceğiz. Artık, ek uygulamaları görüntülemek için **daha fazla yükle** ' ye tıklayabilirsiniz. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>Genel Önizleme aşamasında, PingFederate ile tümleştirme AADConnect sürümünü içeren Mayıs önemli güvenlik güncelleştirmeleri, birçok hata düzeltmeleri ve yeni harika yeni sorun giderme araçları. 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** AD Connect  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi
 
Genel Önizleme aşamasında, PingFederate ile tümleştirme AADConnect sürümünü içeren Mayıs önemli güvenlik güncelleştirmeleri, birçok hata düzeltmeleri ve yeni harika yeni sorun giderme araçları. Sürüm notlarını [buradan](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190)bulabilirsiniz.

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD erişim gözden geçirmeleri: otomatik olarak Uygula

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Erişim Incelemeleri  
**Ürün yeteneği:** #

Erişim gözden geçirmeleri, grupları ve uygulamaları sunulmuştur Azure AD Premium P2 bir parçası olarak. Bir yönetici, erişim gözden geçirmesi tamamlandığında otomatik olarak gözden geçirenin değişiklikler, Grup veya uygulama uygulamak üzere yapılandırabilirsiniz. Yönetici kullanıcının sürekli erişim gözden geçirenler olmadı yanıt, erişimini kaldırmak, erişim tutun veya sistem önerileri ele ne de belirtebilirsiniz. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-response_mode-for-new-apps"></a>Kimlik belirteçlerini, artık yeni uygulamalar için sorgu response_mode kullanarak döndürülebilir. 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
25 Nisan 2018 ' de veya sonrasında oluşturulan uygulamalar artık **sorgu** response_mode kullanarak **id_token** isteyemeyecektir.  Yardımcı uygulama saldırı yüzeyinizi azaltmak ve bu Azure AD satır içi OIDC belirtimleri ile getirir.  25 Nisan 2018 tarihinden önce oluşturulan uygulamalar, **id_token**response_type **sorgu** response_mode kullanılmasını engellenmemiş.  AAD 'den bir id_token istenirken döndürülen hata, **AADSTS70007: ' Query ', belirteç istenirken ' response_mode ' için desteklenen bir değer değildir**.

**Parça** ve **form_post** response_modes çalışmaya devam eder-yeni uygulama nesneleri oluştururken (örneğin, uygulama proxy 'si kullanımı için), yeni bir uygulama oluşturmadan önce bu response_modes birini kullandığınızdan emin olun.  

---
 
## <a name="april-2018"></a>Nisan 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C erişim belirteci olan genel kullanım

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C 

Azure AD B2C ile güvenliği sağlanan Web API'leri artık erişebilirsiniz erişim belirteçleri kullanarak. Özellik genel Önizleme için büyüyecek taşınıyor Azure AD B2C uygulamaları ve web API'leri yapılandırmak için kullanıcı Arabirimi deneyimi İyileştirildi ve başka küçük geliştirmeler yapıldı.
 
Daha fazla bilgi için bkz. [Azure AD B2C: erişim belirteçleri isteme](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>SAML tabanlı uygulamalar için çoklu oturum açma yapılandırması test

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

SAML tabanlı SSO uygulamaları yapılandırırken, yapılandırma sayfasında tümleştirme test edebilirsiniz. Oturum açma sırasında bir hatayla karşılaşırsanız, hata test deneyiminde sağlayabilir ve Azure AD ile çözüm adımları belirli sorunu çözmek için sağlar.

Daha fazla bilgi için bkz.

- [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Azure Active Directory içindeki uygulamalarda SAML tabanlı çoklu oturum açma hatalarını ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD kullanım koşulları artık Kullanıcı başına raporlamaya sahiptir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 
Yöneticiler artık belirli bir ToU'ı seçin ve ToU ve hangi, tarih yer aldığı onay tüm kullanıcılar görür.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health'i: AD FS extranet kilitleme koruması riskli IP 

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** & Raporlamayı izleme

Bir saatlik veya günlük olarak hatalı U/P oturum açma eşiğini aşan IP algılama özelliğine destekler adresleri artık connect Health. Bu özellik tarafından sağlanan özellikler şunlardır:

- IP adresi ve bir saatlik/günlük olarak özelleştirilebilir eşik ile oluşturulan başarısız oturum açma sayısını gösteren kapsamlı bir rapor.
- E-posta tabanlı uyarılar belirli bir IP adresi, gösteren saatlik/günlük olarak hatalı U/P oturum açma eşiğini aştı.
- Verilerin ayrıntılı bir analiz yapmak için bir yükleme seçeneği

Daha fazla bilgi için bkz. [RISKLI IP raporu](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Meta veri dosyası veya URL ile kolay uygulama yapılandırması

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO

Kurumsal uygulamalar sayfasında Yöneticiler SAML tabanlı oturum açma AAD galeri ve galeri dışı bir uygulama için yapılandırmak için bir SAML meta veri dosyasını karşıya yükleyebilirsiniz.

Ayrıca, Azure AD uygulama Federasyon meta veri URL'si ile hedeflenen uygulamaya SSO yapılandırmak için kullanabilirsiniz.

Daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde olmayan uygulamalarda çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)konusuna bakın.

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD kullanım koşulları artık kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 

Azure AD kullanım koşulları, genel önizlemeden genel kullanıma açık olarak taşınmıştır.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>İzin verme veya davetleri B2B kullanıcıları belirli kuruluşlardan engelleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C
 

Paylaşın ve Azure AD B2B işbirliği birlikte çalışmak istediğiniz hangi iş ortağı kuruluşlar artık belirtebilirsiniz. Bunu yapmak için özel bir listesini oluşturmak seçebileceğiniz izin ver veya Reddet etki alanları. Bu özellikleri kullanılarak bir etki alanı engellendiğinde, çalışanlar artık bu etki alanında davetleri kişilere gönderebilirsiniz.

Bu, onaylanan kullanıcılar için sorunsuz bir deneyim etkinleştirirken, kaynaklarınıza erişimi denetlemenize yardımcı olur.

Bu B2B Işbirliği özelliği tüm Azure Active Directory müşterileri tarafından kullanılabilir ve dış iş kullanıcılarının ne zaman ve nasıl imzalandığını daha ayrıntılı bir şekilde denetlemek için koşullu erişim ve kimlik koruması gibi Azure AD Premium özelliklerle birlikte kullanılabilir. ve ' de erişim elde edin.

Daha fazla bilgi için, bkz. [belirli kuruluşlardan B2B kullanıcılarına Izin verme veya bu kullanıcıların davetlerini engelleme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama galerisinde

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Nisan 2018'de uygulama galerimizden Federasyon ile 13 bu yeni uygulamaları desteği ekledik:

Ölçüt HCM, [Fcalnote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (on-premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dinamik sinyal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindwireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [kuruluş şeması artık](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [appneta performans izleyicisi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [floxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), rafı, [SafetyNET](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın.

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Şirket içi uygulamalarınıza (genel Önizleme) Azure AD erişim verme B2B kullanıcıları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** EAı  
**Ürün yeteneği:** B2B/B2C

Azure ad iş ortağı kuruluşlardan Konuk kullanıcıları davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş, artık bu B2B kullanıcıları şirket içi uygulamalara erişim sağlayabilir. Bu şirket içi uygulamaları, Kerberos Kısıtlı temsilci (KCD) ile SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (IWA) kullanabilirsiniz.

Daha fazla bilgi için bkz. Şirket [içi uygulamalarınıza Azure AD erişimi IÇIN B2B kullanıcıları verme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Azure Market'ten SSO tümleştirme öğreticilerine erişin

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Farklı  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

[Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) 'nde listelenen BIR uygulama SAML tabanlı çoklu oturum açmayı destekliyorsa, **Şimdi al** ' a tıklamak bu uygulamayla ilişkili tümleştirme öğreticisini sağlar. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Azure AD SaaS uygulamaları için otomatik kullanıcı hazırlama, daha hızlı performans

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Daha önce Azure AD kiracıları 100. 000'den birleşik kullanıcılar içeriyorsa sağlama bağlayıcılar (örneğin Salesforce, ServiceNow ve kutusu) SaaS uygulamaları için Azure Active Directory kullanıcı kullanan müşteriler yavaş performans karşılaşması ve Gruplar ve hangi kullanıcıların sağlanan belirlemek için kullanıcı ve Grup atamaları kullanıyordu.

2 Nisan 2018'de, Azure Active Directory ve hedef SaaS uygulamaları arasında ilk eşitlemeler gerçekleştirmek için gereken süreyi büyük ölçüde azaltmak Azure AD sağlama hizmeti için önemli performans geliştirmeleri dağıtıldı.

Sonuç olarak, birçok müşteri, ilk eşitleme geçen gün sayısı veya hiçbir zaman tamamlandı, artık yalnızca birkaç dakika veya saat içinde tamamlıyorsanız uygulamalara vardı.

Daha fazla bilgi için bkz. [sağlama sırasında ne olur?](/azure//active-directory/app-provisioning/how-provisioning-works)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self Servis parola sıfırlama için hibrit Azure AD'ye Windows 10 kilit ekranından makineler katılmış

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Hibrit Azure AD'ye katılmış olan makineler için destek eklemek için Windows 10 SSPR özelliğini güncelleştirdik. Bu özellik kullanılabilir Windows 10 RS4 bir Windows 10 makinenin kilit ekranında parolalarını sıfırlamalarına olanak tanır. Etkin ve Self Servis parola sıfırlama için kaydolan kullanıcılar, bu özelliği kullanabilir.

Daha fazla bilgi için bkz. [oturum açma ekranından Azure AD parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Mart 2018
 
### <a name="certificate-expire-notification"></a>Sertifika sona bildirimi

**Şunu yazın:** Düzenle  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
Galeri dışı uygulama dolmak üzere olduğu ya da Azure AD galeri için bir sertifika olduğunda bir bildirim gönderir. 

Bazı kullanıcılar, SAML tabanlı çoklu oturum açma için yapılandırılmış kurumsal uygulamalar için bildirim almadı. Bu sorun çözüldü. Azure AD 7, 30 ve 60 gün içinde sona erecek sertifikaları için bildirim gönderir. Bu olay denetim günlüklerinde görebilirsiniz. 

Daha fazla bilgi için bkz.

- [Azure Active Directory 'da federe çoklu oturum açma için sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory portalındaki denetim etkinliği raporları](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C, twitter ve GitHub kimlik sağlayıcıları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** B2C-tüketici kimlik yönetimi  
**Ürün yeteneği:** B2B/B2C
 
Artık, bir Azure AD B2C kimlik sağlayıcısı olarak Twitter veya GitHub ekleyebilirsiniz. Twitter büyüyecek için genel preview'dan taşınıyor GitHub genel önizlemede yayınlanır.

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
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 için yerel istemcileri tarafından etkileşimli olmayan bir protokol kullanarak sorunsuz SSO desteklenir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Office 365 yerel istemci kullanarak kullanıcı (sürüm 16.0.8730.xxxx ve üstü) sorunsuz çoklu oturum açma kullanarak sessiz oturum açma deneyimi alın. Bu destek, Azure AD'ye etkileşimli olmayan bir protokol (WS-Trust) ekleyerek sağlanır.

Daha fazla bilgi için bkz. [sorunsuz SSO ile yerel bir istemcide oturum açma nasıl çalışır?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Bir uygulamayı Azure AD'nin Kiracı uç noktalar için oturum açma isteği gönderirse bir sessiz oturum açma deneyimini sorunsuz SSO ile kullanıcılar Al

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kullanıcılar, sorunsuz SSO ile sessiz bir oturum açma deneyimi alırlar. bir uygulama (örneğin, `https://contoso.sharepoint.com`) Azure AD 'nin kiracı uç noktalarına oturum açma istekleri gönderiyor; bu, Azure AD 'nin ortak uç noktası (`https://login.microsoftonline.com/common/<...>`) yerine `https://login.microsoftonline.com/contoso.com/<..>` veya `https://login.microsoftonline.com/<tenant_ID>/<..>`.

Daha fazla bilgi için bkz. [kesintisiz çoklu oturum açma Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Sorunsuz çoklu oturum açma almak için kullanıcıların Intranet bölgesi ayarlarını daha önce iki URL yerine yalnızca bir Azure AD URL'sini eklemeniz gerekir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kullanıcılarınıza sorunsuz SSO eklemek için, Active Directory: `https://autologon.microsoftazuread-sso.com`Grup İlkesi kullanarak kullanıcıların Intranet bölgesi ayarlarına yalnızca bir Azure AD URL 'SI eklemeniz gerekir. Daha önce müşterilerin iki URL'si eklemek için zorunluluğundaydı.

Daha fazla bilgi için bkz. [kesintisiz çoklu oturum açma Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama Galerisi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Mart 2018'de bu 15 yeni Federasyon uygulamalarla uygulama galerimizden desteği ekledik:

[BoxCryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [cylanceprotect](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [Signalfx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Asstagenda, [yardione](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), vtiger CRM, ınkırpması, [genlik](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [Contractçalışma,](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial) [bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [trısotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Azure kaynakları için PIM kullanıma sunuldu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Dizin rolleri için Azure AD Privileged Identity Management kullanıyorsanız, artık PIM'ın süresi sınırlı erişim ve atama özellikleri abonelik, kaynak grupları, sanal makineler ve desteklenen herhangi bir kaynak gibi Azure kaynak rolleri için kullanabilirsiniz Azure Resource Manager tarafından. Just-ın-Time rolleri etkinleştirirken multi-Factor Authentication yürürlüğe ve onaylanan değişiklik windows ile koordinasyon halinde etkinleştirmeleri zamanlayabilirsiniz. Ayrıca, bu sürüm geliştirmeleri güncelleştirilmiş bir kullanıcı Arabirimi, onay iş akışları ve özelliği yakında sona erecek rolleri genişletmek ve süresi dolan rolleri yenileme dahil olmak üzere genel Önizleme sırasında kullanılabilir değil ekler.

Daha fazla bilgi için bkz. [Azure kaynakları Için PIM (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Uygulamaları belirteçlerinizi (genel Önizleme) için isteğe bağlı bir talep ekleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Özel veya isteğe bağlı isteği Jwt'ler veya SAML belirteçlerini talep artık Azure AD uygulamanızı geçirebilirsiniz.  Bu, varsayılan boyutu veya Uygulanabilirlik kısıtlamaları nedeniyle bir belirteç olarak bulunmayan bir kullanıcı ya da Kiracı hakkında taleplerdir.  Şu anda genel önizlemede Azure AD uygulamaları v1.0 ve v2.0 uç noktalarda budur.  Bilgi için bkz. hangi talepleri temel eklenebilir ve bunları istemek için uygulama bildirimini düzenleme.  

Daha fazla bilgi için bkz. [Azure AD 'de Isteğe bağlı talepler](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD PKCE daha güvenli OAuth akışlar için destekler.

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD belgeleri, OAuth 2.0 yetkilendirme kodu verme akışı sırasında daha güvenli iletişime izin veren PKCE desteği Not güncelleştirildi.  S256 hem düz metin code_challenges v1.0 ve v2.0 Uç noktalara desteklenir. 

Daha fazla bilgi için bkz. [yetkilendirme kodu isteme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-get_workers-api"></a>Tüm kullanıcı öznitelik değerleri Workday Get_Workers API'SİNDE kullanılabilir sağlama desteği

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama sağlama  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi
 
Gelen Workday'den Active Directory artık Azure AD için sağlama ve genel Önizleme çıkarma ve sağlama Workday Get_Workers API'SİNDE kullanılabilir tüm öznitelik değerleri destekler. Bu ek standart yüzlerce destekler ekler ve ilk sürümü Workday ile birlikte gelen olanlar dışında özel öznitelikler gelen bağlayıcı sağlama.

Daha fazla bilgi için bkz. [Workday kullanıcı özniteliklerinin listesini özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Grup üyeliğini dinamik olan statik ve tersi değiştirme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Grup Yönetimi  
**Ürün yeteneği:** İş
 
Üyelik bir gruba nasıl yönetilir değiştirmek mümkündür. Bu gruba varolan herhangi bir başvuruyu hala geçerli olduğundan sistemde aynı grubu adını ve Kimliğini tutmak istediğinizde kullanışlıdır. Yeni grup oluşturma, bu başvuruları güncelleştirilmesi gerekir.
Bu işlevselliği desteklemek için Azure AD yönetim merkezini güncelleştirdik. Şimdi, müşterilerin var olan grupları dinamik üyelik atanan üyelik ve tersi dönüştürebilirsiniz. Mevcut PowerShell cmdlet'leri de yine kullanılabilir durumdadır.

Daha fazla bilgi için bkz. [Azure Active Directory gruplar Için dinamik üyelik kuralları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Sorunsuz çoklu oturum açma ile geliştirilmiş oturum kapatma davranışı

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Kullanıcılar, Azure AD tarafından güvenli hale getirilmiş bir uygulamaya dışında açıkça imzalanmış olsa bile, daha önce bunlar otomatik olarak etki alanına katılmış cihazlarından bir Azure AD uygulaması, corpnet içinde yeniden erişmeye çalışıyorsanız geri sorunsuz SSO kullanarak oturum açmış olmanız. Bu değişiklik, oturumunuzu desteklenir.  Bu aynı veya farklı Azure seçmelerini sağlar geri, otomatik olarak sorunsuz SSO kullanarak oturum açmış yerine oturum açmak için bir AD hesabı.

Daha fazla bilgi için bkz. [Azure Active Directory kesintisiz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Uygulama Proxy Bağlayıcısı sürümü yayımlanan 1.5.402.0

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Kimlik güvenliği & koruması
 
Bu bağlayıcı sürüm kademeli olarak Kasım kullanıma sunulacaktır. Bu yeni bağlayıcı sürüm, aşağıdaki değişiklikleri içerir:

- Bağlayıcı artık etki alanı düzeyi tanımlama bilgilerini bunun yerine alt etki alanı düzeyini ayarlar. Bu daha sorunsuz SSO bir deneyim sağlar ve yedek kimlik doğrulama istemleri önler.
- Öbekli kodlama istekleri için destek
- Geliştirilmiş bağlayıcı sistem durumu izleme 
- Birçok hata düzeltmesi ve kararlılık geliştirmeleri

Daha fazla bilgi için bkz. [Azure AD uygulama ara sunucusu bağlayıcıları anlama](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Şubat 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Kullanıcılar ve grupları yönetmek için Gezinti geliştirildi

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Dizin Yönetimi  
**Ürün yeteneği:** Dizinden

Kullanıcılar ve grupları yönetmek için gezinti deneyimini basitleştirilmiştir. Artık dizine genel bakış'tan doğrudan silinen kullanıcıların listesine daha kolay erişimi olan tüm kullanıcıların listesini gidebilirsiniz. Grup Yönetimi ayarlarına daha kolay erişim ile tüm grupların listesine doğrudan dizin genel bakış'tan da gidebilirsiniz. Ve ayrıca dizine genel bakış sayfasından bir kullanıcı, Grup, Kurumsal uygulama veya uygulama kaydını arayabilirsiniz. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Oturum açma işlemleri ve denetim kullanılabilirliğini raporlar 21Vianet tarafından işletilen Microsoft azure'da (Azure China 21Vianet)

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure Stack  
**Ürün yeteneği:** & Raporlamayı izleme

Azure AD etkinlik günlük raporlar tarafından (Azure China 21Vianet) örnekleri 21Vianet tarafından işletilen Microsoft azure'da kullanıma sunulmuştur. Aşağıdaki günlüklere dahil edilir:

- **Oturum açma etkinliği günlükleri** -kiracınızla ilişkili tüm oturum açma günlüklerini içerir.

- **Self servis parola denetim günlükleri** -tüm SSPR denetim günlüklerini içerir.

- **Dizin Yönetimi denetim günlükleri** -Kullanıcı yönetimi, uygulama yönetimi ve diğerleri gibi dizin yönetimiyle ilgili tüm denetim günlüklerini içerir.

Bu günlükleri ile ortamınızın nasıl çalıştığını içine öngörü sahibi olabilir. Sağlanan verilerle:

- Uygulama ve hizmetlerinizin kullanıcılarınız tarafından nasıl kullanıldığını saptayabilirsiniz.

- Kullanıcılarınızın işlerini yapmalarını engelleyen sorunları giderin.

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Azure AD Etkinlik raporlarını görüntülemek için "Rapor okuyucu" rolünü (yönetici olmayan rol) kullanın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** & Raporlamayı izleme

Yönetici olmayan rollerin Azure AD etkinlik günlüklerine erişimine sahip olmasını sağlamak için müşterilerin geri bildirimlerinden bir parçası olarak, "rapor okuyucu" rolünde olan kullanıcıların Azure portal içinde oturum açma işlemlerini ve denetim etkinliklerini ve Microsoft Graph kullanma yeteneğini etkinleştirdik. 'Sindeki. 

Bu raporların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Kullanıcı özniteliği ve kullanıcı tanımlayıcısı olarak kullanılabilir EmployeeID talebi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** SSO
 
**ÇalışanNo** 'ı, kurumsal uygulama kullanıcı arabiriminden SAML tabanlı oturum açma uygulamalarında üye KULLANıCıLAR ve B2B konukları için Kullanıcı tanımlayıcısı ve kullanıcı özniteliği olarak yapılandırabilirsiniz.

Daha fazla bilgi için bkz. [Azure Active Directory 'de kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Azure AD uygulama ara Sunucusu'nda joker kullanarak Basitleştirilmiş uygulama yönetimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Uygulama dağıtımını kolaylaştırın ve yönetim yükünüzü azaltmak için artık joker karakter kullanan uygulamalar yayımlama olanağı desteklenmektedir. Bir joker uygulama yayımlamak için standart bir uygulama yayımlama akışını takip edin, ancak iç ve dış URL'lerinde bir joker karakter kullanın.

Daha fazla bilgi için [Azure Active Directory uygulama proxy 'Sinde joker uygulamalar](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard) bölümüne bakın

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Uygulama Ara Sunucusu'nun yapılandırılmasını desteklemeye yönelik yeni cmdlet'ler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Platformunun

AzureAD PowerShell Önizleme modülünün en son sürümü, müşterilerin PowerShell kullanarak uygulama Proxy uygulamaları yapılandırmanıza imkan tanıyan yeni cmdlet'ler içerir.

Yeni cmdlet'ler şunlardır: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Grupların yapılandırılmasını desteklemeye yönelik yeni cmdlet'ler

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulama proxy 'Si  
**Ürün yeteneği:** Platformunun

AzureAD PowerShell modülünün en son sürümü Azure AD'de grupları yönetmek için cmdlet'leri içerir. Bu cmdlet'ler, daha önce AzureADPreview modülünde kullanılabilen ve şimdi AzureAD modülüne eklendi

Şimdi genel kullanılabilirlik sürümü olan Grup cmdlet'ler şunlardır: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- Yeni-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Ekle-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Azure AD Connect'in yeni bir sürüm kullanılabilir

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** AD Eşitleme  
**Ürün yeteneği:** Platformunun
 
Azure AD Connect, Azure AD arasında ve Windows Server Active Directory ve LDAP dahil olmak üzere şirket içi veri kaynakları verileri eşitlemek için tercih edilen bir araçtır.

>[!Important]
>Bu derleme şema ve eşitleme tanıtır kural değişiklikler. Azure AD Connect eşitleme hizmeti bir yükseltmeden sonra bir tam içeri aktarma ve tam eşitleme adımları tetikler. Bu davranışı değiştirme hakkında daha fazla bilgi için, [yükseltmeden sonra tam eşitlemenin ertelenmesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade)bölümüne bakın.

Bu sürüm aşağıdaki güncelleştirmeleri ve değişiklikleri sahiptir:

**Sorun düzeltildi**

- Sonraki sayfaya geçiş yaparken bölüm filtreleme sayfası için arka plan görevleri zamanlama penceresi düzeltmesi.

- Yapılandırmaveritabanı özel eylemi sırasında erişim ihlaline neden bir hata düzeltildi.

- SQL bağlantı zaman aşımı kurtarmak için bir hata düzeltildi.

- Burada SAN joker karakterler sertifikalarla Önkoşul denetimi başarısız bir hata düzeltildi.

- AAD bağlayıcı dışarı aktarma sırasında miiserver.exe kilitlenmeye neden olan bir hata düzeltildi.

- DC üzerinde çalışan AAD neden olduğunda günlüğe bir hatalı parola denemesi yapılandırmasını değiştirmek için Sihirbazı eriştikleri bir hata düzeltildi

**Yeni özellikler ve geliştirmeler**
 
- Uygulama telemetrisini - Yöneticiler, bu sınıf Aç/Kapat verilerinin geçiş yapabilirsiniz.

- Azure AD sistem durumu verilerini - yöneticiler kendi sistem durumu ayarlarını denetlemek için sistem durumu portalı ziyaret etmeniz gerekir. Hizmet İlkesi değiştirildi sonra aracılar okuyun ve onu zorla.

- Cihaz geri yazmayı yapılandırma eylemleri ve sayfa başlatma için bir ilerleme çubuğu ekledik.

- Geliştirilmiş genel tanılama HTML rapor ve tam veri toplama ZIP metin / HTML raporu.

- Geliştirilmiş otomatik güvenilirliğini yükseltin ve sunucusunun sistem durumunu belirlenebilir emin olmak için ek telemetri eklendi.

- Kullanılabilir AD Bağlayıcısı hesabındaki ayrıcalıklı hesaplara kısıtlayın. Yeni yüklemeler için sihirbazın ayrıcalıklı hesapları izinleri kısıtlar MSOL hesabı oluşturduktan sonra MSOL hesabına sahip. Değişiklikler express yüklemeleri ve otomatik olarak oluşturma hesabıyla özel yüklemeleri etkiler.

- Yükleyici SA ayrıcalık AADConnect temiz yüklemesini zorunlu tutulmayacak şekilde değiştirildi.

- Belirli bir nesnesi için eşitleme sorunlarını gidermek için yeni yardımcı program'ı seçin. Şu anda aşağıdakiler için yardımcı program denetler:

    - Azure AD kiracısında kullanıcı hesabı ile eşitlenmiş kullanıcı nesnesi arasındaki UserPrincipalName uyuşmazlığı.
  
    - Nesne filtreleme etki alanı nedeniyle eşitleme filtrelediyseniz
  
    - Nesne eşitleme filtreleme kuruluş birimi (OU) nedeniyle filtrelediyseniz

- Belirli bir kullanıcı hesabı için şirket içi Active Directory içinde depolanan geçerli parola karması eşitleme için yeni yardımcı program'ı seçin. Yardımcı programı, parola değişikliği gerektirmez. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim ile kullanılmak üzere eklenen Intune Uygulama Koruması ilkelerini destekleyen uygulamalar

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Uygulama tabanlı koşullu erişimi destekleyen daha fazla uygulama ekledik. Artık, Office 365 ve bu onaylı istemci uygulamalarını kullanarak diğer Azure AD bağlantılı bulut uygulamalarınıza erişimi alabilirsiniz.

Aşağıdaki uygulamalar Şubat sonuna eklenecek:

- Microsoft Power BI

- Microsoft Başlatıcısı

- Microsoft faturalama

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Mobil deneyimle Kullanım koşulları güncelleştirme 

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk

Kullanım koşulları görüntülendiğinde artık sorun yaşamadan ' a tıklayabilirsiniz.  **Buraya tıklayın**. Bu bağlantıya tıkladığınızda kullanım Cihazınızda yerel olarak açılır. Belgenin yazı tipi boyutu veya cihazın ekran boyutu ne olursa olsun yakınlaştırma ve gerektiğinde belgeyi okuyalım. 

---
 
## <a name="january-2018"></a>Ocak 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Yeni Federasyon uygulamaları kullanılabilir Azure AD uygulama Galerisi 

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Ocak 2018'de app Galerisi'nde aşağıdaki yeni uygulamaları ile Federasyonu Desteği eklendi:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [Onetrust gizlilik yönetimi yazılımı](https://go.microsoft.com/fwlink/?linkid=861660), [Satıcıyolu](https://go.microsoft.com/fwlink/?linkid=863526), [ıriusrisfedermi federe dizini ve [uygunlukta netavantajlar](https://go.microsoft.com/fwlink/?linkid=864701).

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Ek risk algılandı bilgilerinizle oturum açın

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği & koruması

Algılanan bir risk algılaması için aldığınız Öngörüler, Azure AD aboneliğinize bağlıdır. Azure AD Premium P2 sürümü ile temel alınan tüm algılamalar hakkında en ayrıntılı bilgileri alın.

Azure AD Premium P1 sürümü sayesinde, lisansınız kapsamında olmayan algılamalar, risk algılama tarafından algılanan ek risklerle birlikte oturum açma olarak görünür.

Daha fazla bilgi için bkz. [risk algılamalarını Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Son kullanıcı erişim panellerinde Office 365 uygulamalarından gizleme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO

Artık, nasıl Office 365 uygulamaları üzerinde yeni bir kullanıcı ayarı ile kullanıcı erişim panellerinde görünmesi daha iyi yönetebilirsiniz. Bu seçenek Office uygulamalarını yalnızca Office Portalı'nda gösterilecek tercih ederseniz bir kullanıcının erişim panellerinde uygulamaları sayısını azaltmak için yararlıdır. Bu ayar **Kullanıcı ayarlarında** bulunur ve etiketlidir, **kullanıcılar yalnızca Office 365 portalında Office 365 uygulamalarını görebilirler**.

Daha fazla bilgi için bkz. [Azure Active Directory kullanıcının deneyiminden bir uygulamayı gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Uygulamanın URL'sini doğrudan parola SSO için etkinleştirildiği uygulamalar sorunsuz oturum 

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** SSO

Uygulamalarım tarayıcı uzantısı aracılığıyla uygulamalarım çoklu oturum tarayıcınızda bir kısayol olarak özellik hakkında size uygun bir aracı kullanıma sunuldu. Kullanıcının yükledikten sonra tarayıcılarında bunları uygulamalarına hızlı erişim sağlayan bir waffle menüsü simgesi görürsünüz. Kullanıcılar artık yararlanabilirsiniz:

- Uygulamanın oturum açma sayfasından parola SSO tabanlı uygulamalarda doğrudan oturum açma özelliği
- Hızlı arama özelliğini kullanarak herhangi bir uygulama başlatın
- Uzantı son kullanılan uygulamalar için kısayollar
- Uzantı, Microsoft Edge, Chrome ve Firefox için kullanılabilir.
 
Daha fazla bilgi için bkz. [uygulamalarımın güvenli oturum açma uzantısı](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure AD yönetim Klasik Azure portalı deneyiminde Çekildi

**Şunu yazın:** Kullanım dışı   
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Dizinden

8 Ocak 2018'den itibaren Azure AD Yönetim'den itibaren Klasik Azure portalı deneyiminde Çekildi. Bu, Azure Klasik Portalı'nın kendisini kullanımdan kaldırma ile birlikte yapıldı. Gelecekte Azure AD 'nin tüm portal tabanlı yönetimi için [Azure AD Yönetim merkezini](https://aad.portal.azure.com) kullanmanız gerekir.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor web portalı kullanımdan kaldırıldı

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Dizinden
 
8 Ocak 2018'den itibaren PhoneFactor web portalı kullanımdan kaldırıldı. Bu portalı MFA sunucusunun yönetimi için kullanılan, ancak bu işlevleri portal.azure.com adresindeki Azure portalında taşınmıştır. 

MFA yapılandırması şurada bulunur: **Azure Active Directory \> MFA sunucusu**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlar kullanımdan kaldırma

**Şunu yazın:** Kullanım dışı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi  


Genel kullanılabilirlik yeni API'ler hem etkinlik ve güvenlik raporları için rapor API'leri kullanıma sunuldu ve yeni Azure Active Directory Yönetim Konsolu ile altında "/ reports" uç nokta son 31 Aralık 2017 itibariyle devre dışı bırakılmış.

**Neler mevcuttur?**

Yeni Yönetici Konsolu geçiş işleminin bir parçası olarak 2 yeni API'ler Azure AD etkinlik günlüklerini almak için kullanılabilir gerçekleştirdik. Yeni API'leri daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak işlevselliği sunmaktadır. Daha önce güvenlik raporları üzerinden kullanılabilen verilere artık Microsoft Graph kimlik koruması risk algılamaları API 'SI aracılığıyla erişilebilir.

Daha fazla bilgi için bkz.

- [Azure Active Directory Reporting API 'SI ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Azure Active Directory Kimlik Koruması ve Microsoft Graph kullanmaya başlama](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Aralık 2017

### <a name="terms-of-use-in-the-access-panel"></a>Erişim panelinde kullanım koşulları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 
Artık erişim Masası'na gidin ve daha önce kabul ettiğiniz kullanım koşullarını görüntüleyin.

Şu adımları uygulayın:

1. [Uygulamaps portalına](https://myapps.microsoft.com)gidin ve oturum açın.

2. Sağ üst köşede adınızı seçin ve ardından listeden **profil** ' i seçin. 

3. **Profilinizde** **kullanım koşullarını gözden geçir**' i seçin. 

4. Kullanım koşullarını gözden geçirebilir, kabul edildi. 

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği (Önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Yeni Azure AD oturum açma deneyimi

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD ve Microsoft hesabı kimlik sistemi kullanıcı arabirimleri, tutarlı bir görünüm olduğuna şekilde tasarlanmıştır. Ayrıca, Azure AD oturum açma sayfasının kullanıcı adının ilk olarak, ikinci bir ekranda kimlik bilgisi tarafından izlenen toplar.

Daha fazla bilgi için bkz. [yeni Azure AD oturum açma deneyimi artık genel önizlemede](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Daha az oturum açma istemi: yeni "Oturumumu açık bırak" bir deneyimi için Azure AD oturum açma

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması
 
Azure AD oturum açma sayfasındaki Oturumumu Açık **bırak** onay kutusu, başarıyla kimlik doğrulamasından geçtikten sonra görüntülenen yeni bir istem ile değiştirilmiştir. 

Bu istem için **Evet** 'e yanıt verirseniz, hizmet sizi kalıcı yenileme belirteci sağlar. Bu davranış, eski deneyimdeki Oturumumu **açık tut** onay kutusunu seçtiğiniz durumla aynıdır. Federasyon Hizmeti ile başarıyla kimlik doğrulama işleminden sonra Federasyon kiracıları için bu istemi gösterir.

Daha fazla bilgi için bkz. daha [az oturum açma istemi: Azure AD için yeni "Oturumumu Açık tut" deneyimi önizlemededir](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Kabul etmeden önce genişletilecek kullanım koşullarını gerektirecek şekilde yapılandırması Ekle

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk
 
Yöneticiler için bir seçenek, kullanıcıların koşulları kabul etmeden önce kullanım koşullarını genişletmesini gerektirir.

Kullanıcıların kullanım koşullarını genişletmesini gerektirmek için **Açık** veya **kapalı** seçeneklerinden birini belirleyin. **Açık** ayarı, kullanıcıların kabul etmeden önce kullanım koşullarını görüntülemesini gerektirir.

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları özelliği (Önizleme)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Uygun rol atamaları için kapsamlı etkinleştirme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Kapsamlı etkinleştirme, özgün atama varsayılan değerinden daha az bağımsız çalışma sınırı ile uygun bir Azure kaynak rol atamalarını etkinleştirmek için kullanabilirsiniz. Kiracınızda atadığınız bir abonelik sahibi olarak bir örnektir. Kapsamlı etkinleştirme'yle (örneğin, kaynak grupları ve sanal makineler) abonelik kapsamında yer alan en fazla beş kaynakları için sahip rolü etkinleştirebilirsiniz. Aktivasyon kapsamı, kritik Azure kaynakları için istenmeyen değişiklikleri yürütülmeden olasılığını düşürebilir.

Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD uygulama Galerisi'nde yeni Federasyon uygulamaları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kurumsal uygulamalar  
**Ürün yeteneği:** üçüncü taraf tümleştirmesi

Aralık 2017'de, bu yeni Federasyon uygulamalarla uygulama galerimizden desteği ekledik:

[Accanble](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital storefront](https://go.microsoft.com/fwlink/?linkid=861685), [communifire](https://go.microsoft.com/fwlink/?linkid=861676) cybsafe, [factu](https://go.microsoft.com/fwlink/?linkid=863525), [Image çalışma,](https://go.microsoft.com/fwlink/?linkid=863517) [mobi](https://go.microsoft.com/fwlink/?linkid=863521), [Mobileiron Azure AD tümleştirmesi](https://go.microsoft.com/fwlink/?linkid=858027), yansıma [ktive](https://go.microsoft.com/fwlink/?linkid=863518), [tarafından Bamboo için](https://go.microsoft.com/fwlink/?linkid=863520) [SAML SSO](https://go.microsoft.com/fwlink/?linkid=863519) [](https://go.microsoft.com/fwlink/?linkid=863522)

Uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory SaaS uygulama tümleştirmesi](https://aka.ms/appstutorial).

Uygulamanızı Azure AD uygulama galerisinde listeleme hakkında daha fazla bilgi için, [Azure Active Directory Uygulama galerisinde uygulamanızı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)listeleme bölümüne bakın. 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD Dizin rolleri için onay iş akışları

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Azure AD Dizin rolleri için onay iş akışı genel kullanıma sunulmuştur.

Ayrıcalıklı rol kullanabilmeniz için önce onay iş akışı ile ayrıcalıklı rol yöneticileri rol etkinleştirme isteği uygun rolü üyeleri gerektirebilir. Birden çok kullanıcı ve grupları temsilcisi onay sorumlulukları olabilir. Uygun Rol üyeleri onayı tamamlandı ve rollerine etkin olduğunda bildirim alırsınız.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Geçişli kimlik doğrulaması: Skype for Business desteği

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik doğrulamaları (oturum açma)  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Geçişli kimlik doğrulaması artık kullanıcı oturum açma işlemleri Skype Kurumsal çevrimiçi içeren modern kimlik doğrulamasını destekleyen iş istemci uygulamalarını ve karma topolojiler için destekler. 

Daha fazla bilgi için bkz. [modern kimlik doğrulamasıyla desteklenen Skype Kurumsal topolojileri](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Azure RBAC (Önizleme) için Azure AD Privileged Identity Management güncelleştirmeleri

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management
 
Azure AD Privileged Identity Management (PIM) Azure rol tabanlı Access Control (RBAC) için genel Önizleme yenileme ile şunları yapabilirsiniz:

* Yeterli yönetim kullanın.
* Kaynak rolleri etkinleştirmek için onay gerektirir.
* Azure RBAC rolleri ile hem Azure AD için onay gerektiren bir rolü ve gelecekteki bir etkinleştirme zamanlayın.
 
Daha fazla bilgi için bkz. [Azure kaynakları için Privileged Identity Management (Önizleme)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>Kasım 2017
 
### <a name="access-control-service-retirement"></a>Erişim denetimi hizmeti devre dışı bırakma

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Access Control hizmeti  
**Ürün yeteneği:** Access Control hizmeti 

Azure Active Directory erişim denetimi (erişim denetimi hizmeti olarak da bilinir) geç 2018'de kullanımdan kaldırılacaktır. Önümüzdeki birkaç hafta içinde ayrıntılı zamanlama ve üst düzey geçiş kılavuzunu içeren daha fazla bilgi sağlanacaktır. Erişim denetimi hizmeti hakkında sorular ile bu sayfada açıklamaları bırakabilirsiniz ve bir takım üyesi yanıt.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Intune Managed Browser için tarayıcı erişimi kısıtlama 

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Onaylanmış bir uygulama Intune Managed Browser'ı kullanarak, Office 365 ve diğer Azure AD bağlantılı bulut uygulamaları için tarayıcı erişimini kısıtlayabilirsiniz. 

Artık uygulama tabanlı koşullu erişim için aşağıdaki koşulu yapılandırabilirsiniz:

**İstemci uygulamaları:** Tarayıcı

**Değişikliğin etkisi nedir?**

Günümüzde bu durum kullandığınızda erişim engellenir. Önizleme mevcut değil, tüm erişim yönetilen tarayıcı uygulaması kullanımını gerektirir. 

Bu özellik ve yaklaşan blogları ve sürüm notları hakkında daha fazla bilgi arayın. 

Daha fazla bilgi için bkz. [Azure AD 'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Aşağıdaki uygulamalar [onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesidir:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- Microsoft StaffHub

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Kullanım koşulları birden çok dil desteği

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk

Yöneticiler artık birden çok PDF belgeleri içeren yeni kullanım koşulları oluşturabilirsiniz. Bu PDF belgeleri karşılık gelen bir dil ile etiketleyebilirsiniz. Kullanıcılara, PDF, eşleşen dil tercihlerine göre ile gösterilir. Eşleşme yoksa varsayılan dilde gösterilir.

---
 
### <a name="real-time-password-writeback-client-status"></a>Gerçek zamanlı parola geri yazma istemcinizin durumunu

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Self servis parola sıfırlama  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Artık, şirket içi parola geri yazma istemcinizin durumunu gözden geçirebilirsiniz. Bu seçenek, [parola sıfırlama](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) sayfasının **Şirket içi tümleştirme** bölümünde bulunur. 

Şirket içi geri yazma istemciniz için bağlantınızda bir sorun varsa, size sağlayan bir hata iletisi görürsünüz:

- Şirket içi geri yazma istemcinize neden bağlanamıyorsunuz bilgi.
- Sorunun çözümlenmesinde yardımcı belgeleri bağlantısı. 

Daha fazla bilgi için bkz. [Şirket içi tümleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim 
 
**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Azure AD  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Artık Office 365 ve diğer Azure AD bağlı bulut uygulamalarına erişimi, [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)kullanarak Intune uygulama koruma ilkelerini destekleyen [onaylanan istemci uygulamalarına](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview) kısıtlayabilirsiniz. Intune uygulama koruma ilkelerini yapılandırma ve bu istemci uygulamalarını üzerindeki şirket verilerini korumak için kullanılır.

[Uygulama tabanlı](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) [cihaz tabanlı](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) koşullu erişim ilkeleriyle, kişisel ve şirket cihazlarındaki verileri koruma esnekliğine sahip olursunuz.

Aşağıdaki koşullar ve denetimler artık uygulama tabanlı koşullu erişim ile birlikte kullanılabilir:

**Desteklenen platform koşulu**

- iOS
- Android

**İstemci uygulamaları koşulu**

- Mobil uygulamalar ve masaüstü istemcileri

**Erişim denetimi**

- Onaylı istemci uygulaması gerektir

Daha fazla bilgi için bkz. [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure portalında Azure AD cihazları yönetme

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Cihaz kaydı ve yönetimi  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Artık tüm cihazlarınızı Azure AD'ye bağlı bulabilirsiniz ve cihaz ile ilgili etkinlikleri tek bir yerde. Tüm cihaz kimliklerini ve Azure Portalı'ndaki ayarları yönetmek için yeni bir yönetim deneyimi vardır. Bu sürümde, şunları yapabilirsiniz:

- Azure AD 'de koşullu erişim için kullanılabilen tüm cihazlarınızı görüntüleyin.
- Azure hibrit içeren özelliklerini görüntüleme AD alanına katılmış cihazlar.
- Azure AD'ye katılmış cihazlar için BitLocker anahtarlarını bulmak, Cihazınızı ve Intune ile yönetme.
- Azure AD cihaz ile ilgili ayarları yönetin.

Daha fazla bilgi için [Azure Portal kullanarak cihazları yönetme](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal)konusuna bakın.

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Azure AD koşullu erişim için bir cihaz platformu olarak macOS desteği 

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması 

Artık macOS 'ı Azure AD koşullu erişim ilkenize cihaz platformu olarak dahil edebilir (veya dışlayabilirsiniz). Desteklenen cihaz platformları için macOS'ın eklenmesiyle, şunları yapabilirsiniz:

- **Intune kullanarak macOS cihazlarını kaydedin ve yönetin.** Benzer şekilde, iOS ve Android gibi diğer platformlarda, Şirket portalı uygulamasını birleşik kayıtları yapmak için macOS için kullanılabilir. Yeni Şirket portalı uygulaması macOS için Intune ile cihaz kaydetme ve Azure AD ile kaydetmek için kullanabilirsiniz.
- **MacOS cihazlarının, kuruluşunuzun Intune 'da tanımlanan uyumluluk ilkelerine bağlı olduğundan emin olun.** Azure portalında Intune'da artık macOS cihazları için Uyumluluk ilkelerini ayarlayabilirsiniz. 
- **Azure AD 'deki uygulamalara erişimi yalnızca uyumlu macOS cihazlarına kısıtlayın.** Koşullu erişim ilkesi yazma, macOS öğesini ayrı bir cihaz platformu seçeneği olarak içerir. Artık Azure 'da ayarlanan hedeflenen uygulama için macOS 'a özgü koşullu erişim ilkelerini yazabilirsiniz.

Daha fazla bilgi için bkz.

- [Intune ile macOS cihazları için cihaz uyumluluğu ilkesi oluşturma](https://aka.ms/macoscompliancepolicy)
- [Azure AD 'de koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication için ağ ilkesi sunucusu uzantısı 

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:**  Multi-Factor Authentication  
**Ürün yeteneği:** Kullanıcı kimlik doğrulaması

Azure multi-Factor Authentication için ağ ilkesi sunucusu uzantısı, mevcut sunucularınızda kullanarak bulut tabanlı çok faktörlü kimlik doğrulama özellikleri kimlik doğrulama altyapınızı ekler. Ağ İlkesi Sunucusu uzantısı ile mevcut kimlik doğrulama akışınıza telefon araması, SMS mesajı ve telefon uygulaması doğrulama ekleyebilirsiniz. Yükleme, yapılandırma ve yeni sunuculara bakım gerekmez. 

Bu uzantı, sanal özel ağ bağlantılarıyla Azure multi-Factor Authentication Sunucusu'nu dağıtmadan korumak istediğiniz kuruluşlar için oluşturuldu. Ağ ilkesi sunucusu arasında RADIUS ve Azure multi-Factor Authentication'ı bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için bir bağdaştırıcı uzantısı görür federasyona eklenenler veya eşitlenmiş kullanıcıları.

Daha fazla bilgi için bkz. [mevcut ağ Ilkesi sunucusu altyapınızı Azure Multi-Factor Authentication tümleştirme](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Geri yükleme ya da kalıcı olarak silinen Kullanıcıları Kaldır

**Şunu yazın:** Yeni Özellik    
**Hizmet kategorisi:** Kullanıcı Yönetimi  
**Ürün yeteneği:** Dizinden 

Azure AD Yönetim merkezinde artık şunları yapabilirsiniz:

- Silinen bir kullanıcıyı geri yükleyin. 
- Kullanıcı kalıcı olarak siler.

**Denemek için:**

1. Azure AD Yönetim merkezinde **Yönet** bölümündeki [tüm kullanıcılar](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) ' ı seçin. 

2. **Göster** listesinden, **son silinen kullanıcılar**' ı seçin. 

3. Yakın zamanda silinen kullanıcılar, bir veya daha fazla seçin ve ardından ya da geri yükleyebilir veya kalıcı olarak silebilirsiniz.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Azure AD uygulama tabanlı koşullu erişim için yeni onaylanan istemci uygulamaları
 
**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

[Onaylanan istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)listesine aşağıdaki uygulamalar eklenmiştir:

- Microsoft Planner
- Azure Information Protection 

Daha fazla bilgi için bkz.

- [Onaylanan istemci uygulaması gereksinimi](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [Azure AD uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Koşullu erişim ilkesindeki denetimler arasında "OR" kullanın 

**Şunu yazın:** Değiştirilen özellik    
**Hizmet kategorisi:** Koşullu erişim  
**Ürün yeteneği:** Kimlik güvenliği ve koruması
 
Koşullu erişim denetimleri için artık "veya" (seçili denetimlerden birini gerektir) kullanabilirsiniz. "Veya" arasında erişim denetim ilkeleri oluşturmak için bu özelliği kullanabilirsiniz. Örneğin, bir kullanıcı çok faktörlü kimlik doğrulaması kullanarak oturum açın "veya" uyumlu bir cihaz üzerinde olmasını gerektiren bir ilke oluşturmak için bu özelliği kullanabilirsiniz.

Daha fazla bilgi için bkz. [Azure AD koşullu erişim 'Deki denetimler](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-detections"></a>Gerçek zamanlı risk algılamalarını toplama

**Şunu yazın:** Değiştirilen özellik    
**Hizmet kategorisi:** Kimlik koruması  
**Ürün yeteneği:** Kimlik güvenliği ve koruması

Azure AD Kimlik Koruması, belirli bir gündeki aynı IP adresinden kaynaklanan tüm gerçek zamanlı risk algılamaları artık her risk algılama türü için toplanır. Bu değişiklik, Kullanıcı güvenliğine hiçbir değişiklik yapılmadan gösterilen risk algılamaları hacminin sınırlarını sınırlar.

Temel alınan gerçek zamanlı algılama, kullanıcı oturum açtığı her zaman çalışır. Çok faktörlü kimlik doğrulaması veya erişimi engelleme için ayarlanmış bir oturum açma riski ilkesi varsa, yine de her riskli oturum açma sırasında tetiklenir.
 
---
 
## <a name="october-2017"></a>Ekim 2017

### <a name="deprecate-azure-ad-reports"></a>Azure AD raporlar kullanımdan kaldırma

**Şunu yazın:** Değişiklik planı  
**Hizmet kategorisi:** Rapor  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi  

Azure portalı ile sağlar:

- Yeni bir Azure AD Yönetim Konsolu.
- Yeni API'ler için etkinlik ve güvenlik raporları.
 
Bu yeni özellikler nedeniyle/Reports uç nokta altında API'ler rapor kullanımdan kaldırılmıştır 10 Aralık 2017'de. 

---

### <a name="automatic-sign-in-field-detection"></a>Otomatik oturum açma alanı algılaması

**Şunu yazın:** Düzenle   
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** Çoklu oturum açma  

Azure AD, bir HTML kullanıcı adı ve parola alanı işlemek uygulamalar için otomatik oturum açma alanı algılaması destekler. Bu adımlar, [bir uygulama için oturum açma alanlarını otomatik olarak yakalama](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications-problems#manually-capture-sign-in-fields-for-an-app)bölümünde belgelenmiştir. Bu özelliği, [Azure Portal](https://aad.portal.azure.com) **Enterprise Applications** sayfasında *Galeri dışı* bir uygulama ekleyerek bulabilirsiniz. Ayrıca, bu yeni uygulamadaki **Çoklu oturum** açma modunu **parola tabanlı çoklu oturum açma**için yapılandırabilir, bir Web URL 'si girebilir ve sonra sayfayı kaydedebilirsiniz.
 
Bir hizmet sorunu nedeniyle bu işlevselliği geçici olarak devre dışı bırakıldı. Sorunun çözülüp çözülmediğini ve otomatik oturum açma alanı algılaması yeniden kullanılabilir.

---

### <a name="new-multi-factor-authentication-features"></a>Yeni çok faktörlü kimlik doğrulaması özellikleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Multi-Factor Authentication  
**Ürün yeteneği:** Kimlik güvenliği ve koruması  

Çok faktörlü kimlik doğrulaması (MFA), kuruluşunuzun koruma önemli bir parçasıdır. Kimlik bilgileri daha Uyarlamalı ve deneyimi daha sorunsuz hale getirmek için aşağıdaki özellikleri eklendi: 

- Çok faktörlü sınama sonuçları, MFA sonuçları programlı erişim içeren Azure AD oturum açma raporu doğrudan bütünleştirilmiştir.
- MFA yapılandırmanın daha derin bir şekilde Azure AD'ye yapılandırmaya tümleşik deneyimi Azure Portalı'nda.

Bu genel Önizleme ile MFA yönetim ve raporlama çekirdek Azure AD'ye yapılandırma deneyimi tümleşik bir parçası olan. Artık Azure AD deneyimi içinde MFA Yönetim Portalı işlevlerini yönetebilirsiniz.

Daha fazla bilgi için bkz. [Azure Portal MFA raporlaması Için başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Kullanım koşulları

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Kullanım koşulları  
**Ürün yeteneği:** Uyumluluk  

Azure AD kullanım koşulları yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri gibi bilgiler sunmak için kullanabilirsiniz.

Azure AD kullanım koşulları aşağıdaki senaryolarda kullanabilirsiniz:

- Genel, kuruluşunuzdaki tüm kullanıcılar için kullanım koşulları
- Belirli kullanıcı özniteliklerine (doktorlarla nurses karşılaştırması gibi) ya da yurtiçi ve uluslararası çalışanlara, dinamik gruplar tarafından yapılan temel kullanım koşulları
- Salesforce gibi yüksek etkili iş uygulamaları erişmek için belirli kullanım koşulları

Daha fazla bilgi için bkz. [Azure AD kullanım koşulları](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="enhancements-to-privileged-identity-management"></a>Privileged Identity Management geliştirmeleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Privileged Identity Management  
**Ürün yeteneği:** Privileged Identity Management  

Azure AD Privileged Identity Management ile yönetebilir, denetleyebilir ve kuruluşunuz içinde (Önizleme) Azure kaynaklarına erişimi izleyin:

- Abonelikler
- Kaynak grupları
- Sanal makineler 

Azure RBAC işlevselliği kullandığı tüm kaynakları Azure portalındaki güvenlik ve Azure AD Privileged Identity Management sunduğu yaşam döngüsü yönetim özellikleri yararlanabilirsiniz.

Daha fazla bilgi için bkz. [Azure kaynakları için Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Erişim gözden geçirmeleri

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Erişim İncelemeleri  
**Ürün yeteneği:** Uyumluluk  

Kuruluşların grup üyeliklerini ve kurumsal uygulamalara erişimi etkili bir şekilde yönetmek için erişim gözden geçirmeleri (Önizleme) kullanabilirsiniz: 

- Konuk kullanıcıların uygulamalara ve grup üyeliklerine erişimlerine ait erişim gözden geçirmelerini kullanarak bu kullanıcıların erişimini yeniden onaylayabilirsiniz. Gözden geçirenler, verimli bir şekilde erişim gözden geçirmeleri tarafından sağlanan bilgiler dayalı olarak erişimi Konukları izin vermek isteyip devam karar verebilirsiniz.
- Erişim gözden geçirmeleri ile çalışanların uygulamalara erişimini ve grup üyeliklerini yeniden onaylayabilirsiniz.

Erişim gözden geçirmesi denetimlerini kuruluşunuza uygun programlarda toplayarak, uyumluluk veya riske duyarlı uygulamalar için gözden geçirmeleri takip edebilirsiniz.

Daha fazla bilgi için bkz. [Azure AD erişim İncelemeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Üçüncü taraf uygulamalardan uygulamalarım ve Office 365 uygulama başlatıcısında Gizle

**Şunu yazın:** Yeni Özellik  
**Hizmet kategorisi:** Uygulamalarım  
**Ürün yeteneği:** Çoklu oturum açma  

Şimdi, yeni bir **uygulama Gizle** özelliği aracılığıyla kullanıcılarınızın portallarında görüntülenen uygulamaları daha iyi yönetebilirsiniz. Arka uç hizmetlerine veya yinelenen kutucukları ve dağınıklık kullanıcıların uygulama launchers uygulama kutucuklarına burada görünmesi durumlarda yardımcı olması için uygulamanızı gizleyebilirsiniz. Geçiş, üçüncü taraf uygulamanın **Özellikler** bölümüdür ve **Kullanıcı tarafından görülebilir** olarak etiketlenir. Ayrıca PowerShell üzerinden program aracılığıyla uygulama gizleyebilirsiniz. 

Daha fazla bilgi için bkz. [Azure AD 'de kullanıcının deneyiminden üçüncü taraf bir uygulamayı gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Neler mevcuttur?**

 Yeni Yönetici Konsolu, iki yeni API geçişi kapsamında günlüklerine Azure AD etkinlik almak için kullanılabilir. Yeni API'leri daha zengin filtreleme ve sıralama daha zengin denetim ve oturum açma etkinlikleri sağlamaya ek olarak işlevselliği sunmaktadır. Daha önce güvenlik raporları üzerinden kullanılabilen verilere artık Microsoft Graph kimlik koruması risk algılamaları API 'SI aracılığıyla erişilebilir.


## <a name="september-2017"></a>Eylül 2017

### <a name="hotfix-for-identity-manager"></a>Identity Manager için düzeltme

**Şunu yazın:** Değiştirilen özellik  
**Hizmet kategorisi:** Kimlik Yöneticisi  
**Ürün yeteneği:** Kimlik yaşam döngüsü yönetimi  

Bir düzeltme dökümü paket (derleme 4.4.1642.0), 25 Eylül 2017 Identity Manager 2016 Service Pack 1'den itibaren kullanılabilir. Bu döküm paketi:

- Sorunları giderir ve geliştirmeleri içerir.
- Identity Manager 2016 için derleme 4.4.1459.0 kadar tüm Identity Manager 2016 Service Pack 1 güncelleştirmeleri değiştirir toplu bir güncelleştirmesidir. 
- Identity Manager 2016 4.4.1302.0 derleme olmasını gerektirir. 

Daha fazla bilgi için bkz. [Identity Manager 2016 Service Pack 1 Için düzeltme paketi paketi (Build 4.4.1642.0) kullanılabilir](https://support.microsoft.com/help/4021562). 

---
