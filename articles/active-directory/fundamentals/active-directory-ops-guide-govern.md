---
title: Azure Active Directory yönetim işlemleri başvuru kılavuzu
description: Bu işlem başvuru kılavuzu, yönetim yönetimini güvence altına almak için çekmeniz gereken denetimleri ve eylemleri açıklar
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: 4826bcdc85e0c6189c51aa262014fe154bb479b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74535463"
---
# <a name="azure-active-directory-governance-operations-reference-guide"></a>Azure Active Directory yönetim işlemleri başvuru kılavuzu

Azure AD [işlemleri başvuru kılavuzunun](active-directory-ops-guide-intro.md) bu bölümünde, çevreye verilen ayrıcalıklı olmayan ve ayrıcalıklı kimliklere, denetime ve denetim değişikliklerine erişim sağlamak ve bunları kanıtlamak için gereken denetimler ve eylemler açıklanmaktadır.

> [!NOTE]
> Bu öneriler yayımlama tarihi itibariyle geçerlidir, ancak zaman içinde değişebilir. Kuruluşlar, Microsoft ürünleri ve hizmetleri zaman içinde geliştikçe yönetim uygulamalarını sürekli olarak değerlendirmelidir.

## <a name="key-operational-processes"></a>Temel operasyonel süreçler

### <a name="assign-owners-to-key-tasks"></a>Anahtar görevlere sahipleri atama

Azure Active Directory'nin yönetimi, kullanıma sunulması projesinin parçası olmayabilecek önemli operasyonel görevlerin ve süreçlerin sürekli olarak yürütülmesini gerektirir. Ortamınızı en iyi duruma getirmek için bu görevleri ayarlamanız yine de önemlidir. Önemli görevler ve önerilen sahipleri şunlardır:

| Görev | Sahip |
| :- | :- |
| SIEM sisteminde Azure AD denetim günlüklerini arşivleyin | InfoSec Operasyon Ekibi |
| Uyumluluk dışında yönetilen uygulamaları keşfedin | IAM Operasyon Ekibi |
| Uygulamalara erişimi düzenli olarak gözden geçirin | InfoSec Mimarlık Ekibi |
| Dış kimliklere erişimi düzenli olarak gözden geçirin | InfoSec Mimarlık Ekibi |
| Ayrıcalıklı rollere sahip olanları düzenli olarak gözden geçirin | InfoSec Mimarlık Ekibi |
| Ayrıcalıklı rolleri etkinleştirmek için güvenlik kapılarını tanımlama | InfoSec Mimarlık Ekibi |
| İzin bağışlarını düzenli olarak gözden geçirin | InfoSec Mimarlık Ekibi |
| Kuruluştaki çalışanlar için temel uygulamalar ve kaynaklar için Kataloglar ve Erişim Paketleri Tasarla | Uygulama Sahipleri |
| Kullanıcıları erişim paketlerine atamak için Güvenlik İlkeleri'ni tanımlama | InfoSec ekibi + Uygulama Sahipleri |
| İlkeler onay iş akışlarını içeriyorsa, iş akışı onaylarını düzenli olarak gözden geçirin | Uygulama Sahipleri |
| Erişim incelemelerini kullanarak koşullu erişim ilkeleri gibi güvenlik ilkelerindeki özel durumları gözden geçirme | InfoSec Operasyon Ekibi |

Listenizi gözden geçirerken, sahibi eksik olan görevler için bir sahip atamanız veya yukarıdaki önerilerle uyumlu olmayan sahiplerle olan görevlerin sahipliğini ayarlamanız gerekebilir.

#### <a name="owner-recommended-reading"></a>Sahibi okuma tavsiye

- [Azure Active Directory’de yönetici rolü atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)
- [Azure’da idare](https://docs.microsoft.com/azure/security/governance-in-azure)

### <a name="configuration-changes-testing"></a>Yapılandırma değişiklikleri testi

Kullanıcıların hedef alt kümesini kullanıma alma gibi basit tekniklerden paralel test kiracısında değişiklik dağıtmaya kadar, sınama sırasında özel hususlar gerektiren değişiklikler vardır. Bir test stratejisi uygulamadıysanız, aşağıdaki tablodaki yönergelere dayalı bir test yaklaşımı tanımlamanız gerekir:

| Senaryo| Öneri |
|-|-|
|Kimlik doğrulama türünü federeden PHS/PTA'ya değiştirme veya bunun tersi| Kimlik doğrulama türünü değiştirmenin etkisini test etmek için [aşamalı kullanıma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-staged-rollout) sonlandırmayı kullanın.|
|Yeni bir koşullu erişim (CA) ilkesi veya Kimlik Koruma İlkesi kullanıma|Yeni bir CA İlkesi oluşturun ve kullanıcıları sınamak için atayın.|
|Bir uygulamanın test ortamına binme|Uygulamayı bir üretim ortamına ekleyin, MyApps panelinden gizleyin ve kalite güvencesi (QA) aşamasında kullanıcıları test etmek için atayın.|
|Eşitleme kurallarının değiştirilmesi|Bir testAzure AD Connect'teki değişiklikleri, şu anda üretimde olan ve hazırlama modu olarak da bilinen yapılandırmayla gerçekleştirin ve CSExport Sonuçlarını çözümleyin. Memnun edilirse, hazır olduğunda üretime değiştirin.|
|Marka değiştirme|Ayrı bir test kiracısına test edin.|
|Yeni bir özelliği kullanıma alma|Özellik bir hedef kullanıcı kümesine kullanıma sunulmasını destekliyorsa, pilot kullanıcıları belirleyin ve geliştirin. Örneğin, self servis parola sıfırlama ve çok faktörlü kimlik doğrulama belirli kullanıcıları veya grupları hedefleyebilir.|
|Şirket içi kimlik sağlayıcısından (IdP) Active Directory'den Azure AD'ye bir uygulama kesme|Uygulama, örneğin Salesforce gibi birden çok IdP yapılandırmasını destekliyorsa, bir değişiklik penceresi sırasında her ikisini de yapılandırın ve Azure AD'yi test edin (uygulamanın HRD sayfasını tanıtması durumunda). Uygulama birden çok IDP'yi desteklemiyorsa, denetim değişikliği penceresi ve program kapalı kalma süresi sırasında testi zamanlayın.|
|Dinamik grup kurallarını güncelleştirme|Yeni kuralla paralel dinamik bir grup oluşturun. Hesaplanan sonuçla karşılaştırın, örneğin PowerShell'i aynı koşulla çalıştırın.<br>Test geçerse, eski grubun kullanıldığı yerleri (mümkünse) değiştirin.|
|Ürün lisanslarını geçirin|Azure [Etkin Dizin'deki lisanslı bir gruptaki tek bir kullanıcının lisansını değiştir'e](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-change-licenses)bakın.|
|Yetkilendirme, Verme, MFA gibi AD FS kurallarını değiştirme|Kullanıcıların alt kümesini hedeflemek için grup iddiasını kullanın.|
|AD FS kimlik doğrulama deneyimini veya çiftlik genelindeki benzer değişiklikleri değiştirme|Aynı ana bilgisayar adına paralel bir çiftlik oluşturun, config değişiklikleri uygulayın, HOSTS dosyalarını kullanarak istemcilerden test edin, NLB yönlendirme kuralları veya benzer yönlendirme.<br>Hedef platform HOSTS dosyalarını (örneğin mobil aygıtlar) desteklemiyorsa, denetimi değiştirin.|

## <a name="access-reviews"></a>Erişim gözden geçirmeleri

### <a name="access-reviews-to-applications"></a>Uygulamalara erişim değerlendirmeleri

Zaman içinde, kullanıcılar farklı takımlar ve konumlar arasında hareket ederken kaynaklara erişim birikir. Kaynak sahiplerinin uygulamalara erişimi düzenli olarak gözden geçirmesi ve kullanıcıların yaşam döngüsü boyunca artık ihtiyaç duyulmadığı ayrıcalıkları kaldırması önemlidir. Azure AD [erişim incelemeleri,](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) kuruluşların grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmesini sağlar. Kaynak sahipleri, yalnızca doğru kişilerin sürekli erişime sahip olduğundan emin olmak için kullanıcıların erişimini düzenli olarak gözden geçirmelidir. İdeal olarak, bu görev için Azure AD erişim yorumlarını kullanmayı düşünmelisiniz.

![Erişim incelemeleri başlangıç sayfası](./media/active-directory-ops-guide/active-directory-ops-img15.png)

> [!NOTE]
> Erişim incelemeleri ile etkileşimde bulunan her kullanıcının ücretli bir Azure AD Premium P2 lisansına sahip olması gerekir.

### <a name="access-reviews-to-external-identities"></a>Dış kimliklere erişim değerlendirmeleri

Yalnızca ihtiyaç duyulan kaynaklarla sınırlandırılmış dış kimliklere erişimi sağlamak çok önemlidir. Azure AD [erişim incelemelerini](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)kullanarak tüm dış kimlikler ve uygulama erişimi için düzenli bir otomatik erişim gözden geçirme işlemi kurun. Şirket içinde zaten bir işlem varsa, Azure AD erişim incelemelerini kullanmayı düşünün. Bir uygulama kullanımdan kaldırıldıktan veya artık kullanılmadıktan sonra, uygulamaya erişimi olan tüm dış kimlikleri kaldırın.

> [!NOTE]
> Erişim incelemeleri ile etkileşimde bulunan her kullanıcının ücretli bir Azure AD Premium P2 lisansına sahip olması gerekir.

## <a name="privileged-account-management"></a>Ayrıcalıklı hesap yönetimi

### <a name="privileged-account-usage"></a>Ayrıcalıklı hesap kullanımı

Bilgisayar korsanları genellikle hassas verilere ve sistemlere hızla erişmek için yönetici hesaplarını ve ayrıcalıklı erişimin diğer öğelerini hedef ler.Ayrıcalıklı rolleri olan kullanıcılar zaman içinde birikme eğiliminde olduğundan, yönetici erişimini düzenli olarak gözden geçirmek ve yönetmek ve Azure AD ve Azure kaynaklarına tam zamanında ayrıcalıklı erişim sağlamak önemlidir.

Kuruluşunuzda ayrıcalıklı hesapları yönetmek için bir işlem yoksa veya şu anda hizmetleri ve kaynakları yönetmek için normal kullanıcı hesaplarını kullanan yöneticileriniz varsa, örneğin günlük normal hesaplar için hemen ayrı hesaplar kullanmaya başlamalısınız. faaliyetleri; ayrıcalıklı erişim için diğer ve MFA ile yapılandırılmıştır. Daha da iyisi, kuruluşunuzazure AD Premium P2 aboneliğine sahipse, hemen [Azure AD Ayrıcalıklı Kimlik Yönetimi'ni](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure#license-requirements) (PIM) dağıtmanız gerekir. Aynı şekilde, bu ayrıcalıklı hesapları da gözden geçirmeli ve varsa [daha az ayrıcalıklı roller atamalısınız.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure)

Uygulanması gereken ayrıcalıklı hesap yönetiminin bir diğer yönü de, pim aracılığıyla el ile veya [otomatik olarak](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-perform-security-review)bu hesaplar için erişim [gözden geçirmelerini](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) tanımlamaktır.

#### <a name="privileged-account-management-recommended-reading"></a>Ayrıcalıklı hesap yönetimi okuma önerilir

- [Azure AD Ayrıcalıklı Kimlik YönetimindeKi Roller](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-roles)

### <a name="emergency-access-accounts"></a>Acil erişim hesapları

Kuruluşlar, şu gibi kimlik doğrulama kesintileri gibi durumlar için Azure AD'yi yönetmeye hazır olmak için [acil durum hesapları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access) oluşturmalıdır:

- Kimlik doğrulama altyapılarının kesinti bileşenleri (AD FS, Şirket Içi AD, MFA hizmeti)
- İdari personel cirosu

Yönetici olarak varolan bir kullanıcının hesabını oturum veremedemediğinizden veya etkinleştiremediğinizden kiracınızın yanlışlıkla kilitlenmemesini önlemek için, iki veya daha fazla acil durum hesabı oluşturmalı ve bunların [Microsoft'un en iyi uygulamalarıyla](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure) uyumlu hale getirildiğinden ve [cam yordamlarını kırdığından](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure#break-glass-what-to-do-in-an-emergency)emin olmalısınız.

### <a name="privileged-access-to-azure-ea-portal"></a>Azure EA portalına ayrıcalıklı erişim

[Azure Kurumsal Sözleşmesi (Azure EA) portalı,](https://azure.microsoft.com/blog/create-enterprise-subscription-experience-in-azure-portal-public-preview/) kuruluş içinde güçlü bir rol olan ana Kurumsal Sözleşme'ye karşı Azure abonelikleri oluşturmanıza olanak tanır. Azure AD'yi yerleştirmeden önce bu portalın oluşturulmasını ön yükleme yaygındır, bu nedenle azure AD kimliklerini kullanarak kilitlemek, kişisel hesapları portaldan kaldırmak, uygun temsilcinin yerinde olduğundan emin olmak ve lokavt riskini azaltmak için gereklidir .

Açık olmak gerekirse, EA portalı yetkilendirme düzeyi şu anda "karışık mod" olarak ayarlanmışsa, EA portalındaki tüm ayrıcalıklı erişimden microsoft [hesaplarını](https://support.skype.com/en/faq/FA12059/what-is-a-microsoft-account) kaldırmanız ve EA portalını yalnızca Azure AD hesaplarını kullanacak şekilde yapılandırmanız gerekir. EA portalı tarafından devredilen roller yapılandırılmamışsa, bölümler ve hesaplar için temsilci verilen rolleri bulmalı ve uygulamalısınız.

#### <a name="privileged-access-recommended-reading"></a>Ayrıcalıklı erişim önerilen okuma

- [Azure Active Directory'deki yönetici rolü izinleri](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

## <a name="entitlement-management"></a>Yetkilendirme yönetimi

[Yetkilendirme yönetimi (EM),](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) uygulama sahiplerinin kaynakları paketleyip kuruluştaki belirli kişiliklere (hem dahili hem de harici) atamalarına olanak tanır. EM, erişim sağlamak, erişim sürelerini ayarlamak ve onay iş akışlarına izin vermek için yönetim ilkelerini tutarken self servis kaydolma ve işletme sahiplerine delegasyon sağlar. 

> [!NOTE]
> Azure AD Yetkilendirme Yönetimi için Azure AD Premium P2 lisansları gerekmektedir.

## <a name="summary"></a>Özet

Güvenli bir kimlik yönetiminin sekiz yönü vardır. Bu liste, ayrıcalıklı olmayan ve ayrıcalıklı olmayan kimliklere, denetime ve denetim değişikliklerine verilen erişimi değerlendirmek ve kanıtlamak için gereken eylemleri belirlemenize yardımcı olur.

- Önemli görevlere sahipleri atayın.
- Bir test stratejisi uygulayın.
- Grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarıverimli bir şekilde yönetmek için Azure AD Erişim İncelemeleri'ni kullanın.
- Her tür dış kimlik ve uygulama erişimi için düzenli, otomatik erişim gözden geçirme işlemi kurun.
- Yönetici erişimini düzenli olarak gözden geçirmek ve yönetmek ve Azure AD ve Azure kaynaklarına tam zamanında ayrıcalıklı erişim sağlamak için bir erişim gözden geçirme işlemi kurun.
- Beklenmeyen kesintiler için Azure AD'yi yönetmek için hazırlanacak acil durum hesaplarını sağlama.
- Azure EA portalına erişimi kilitleyin.
- Kaynak koleksiyonuna yönetilmiş erişim sağlamak için Yetkilendirme Yönetimi'ni uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD [operasyonel denetimleri ve eylemleri](active-directory-ops-guide-ops.md)ile başlayın.
