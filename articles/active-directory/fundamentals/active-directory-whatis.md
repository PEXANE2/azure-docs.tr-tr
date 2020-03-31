---
title: Azure Active Directory nedir? - Azure Etkin Dizin | Microsoft Dokümanlar
description: Terminoloji, hangi lisansların kullanılabili ve daha fazla bilgi için bağlantılara sahip ilişkili özelliklerin listesi de dahil olmak üzere Azure Etkin Dizini hakkında genel ve kavramsal bilgiler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: overview
ms.date: 07/31/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd7b412e99526935738c2494d31a16fded7101a6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240789"
---
# <a name="what-is-azure-active-directory"></a>Azure Active Directory nedir?

Azure Active Directory (Azure AD), çalışanlarınızın aşağıdaki lerde oturum açmalarına ve kaynaklara erişmesine yardımcı olan Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir:

- Microsoft Office 365, Azure portalı ve binlerce diğer SaaS uygulaması gibi dış kaynaklar.

- Kurumsal ağınızdaki ve intranetinizdeki uygulamalar gibi dahili kaynakların yanı sıra kendi kuruluşunuz tarafından geliştirilen tüm bulut uygulamaları.

Azure, Azure AD ve Office 365'teki temel kimlik hizmetlerini daha iyi anlamak [için Kurumsal Mimarlar Serisi](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources#identity) için çeşitli Microsoft Cloud posterlerini kullanabilirsiniz.

## <a name="who-uses-azure-ad"></a>Azure AD'yi kimler kullanır?

Azure AD için tasarlanmıştır:

- **BT yöneticileri.** BT yöneticisi olarak, iş gereksinimlerinize bağlı olarak uygulamalarınıza ve uygulama kaynaklarınıza erişimi denetlemek için Azure AD'yi kullanabilirsiniz. Örneğin, önemli kuruluş kaynaklarına erişirken çok faktörlü kimlik doğrulaması gerektirmek için Azure AD'yi kullanabilirsiniz. Ayrıca, mevcut Windows Server REKLAM'ınız ile Office 365 de dahil olmak üzere bulut uygulamalarınız arasında kullanıcı sağlamayı otomatikleştirmek için Azure AD'yi kullanabilirsiniz. Son olarak, Azure AD, kullanıcı kimliklerinin ve kimlik bilgilerinin otomatik olarak korunmasına ve erişim yönetimi gereksinimlerinizi karşılamanıza yardımcı olacak güçlü araçlar sağlar. Başlamak için [30 günlük ücretsiz Azure Active Directory Premium deneme sürümüne](https://azure.microsoft.com/trial/get-started-active-directory/)kaydolun.

- **Uygulama geliştiricileri.** Uygulama geliştiricisi olarak, uygulamanıza tek oturum açma (SSO) ekleyerek kullanıcının önceden varolan kimlik bilgileriyle çalışmasını sağlayan standartlara dayalı bir yaklaşım olarak Azure AD'yi kullanabilirsiniz. Azure AD, varolan kuruluş verilerini kullanarak kişiselleştirilmiş uygulama deneyimleri oluşturmanıza yardımcı olabilecek API'ler de sağlar. Başlamak için [30 günlük ücretsiz Azure Active Directory Premium deneme sürümüne](https://azure.microsoft.com/trial/get-started-active-directory/)kaydolun. Daha fazla bilgi için [geliştiriciler için Azure Etkin Dizini'ni](../develop/index.yml)de görebilirsiniz.

- **Microsoft 365, Office 365, Azure veya Dynamics CRM Online aboneleri.** Abone olarak Azure AD'yi zaten kullanıyorsunuz. Her Microsoft 365, Office 365, Azure ve Dynamics CRM Online kiracıotomatik olarak bir Azure AD kiracıdır. Entegre bulut uygulamalarınıza erişimi hemen yönetmeye başlayabilirsiniz.

## <a name="what-are-the-azure-ad-licenses"></a>Azure REKLAM lisansları nelerdir?

Office 365 veya Microsoft Azure gibi Microsoft Online iş hizmetleri, oturum açma ve kimlik koruması konusunda yardımcı olmak için Azure AD gerektirir. Herhangi bir Microsoft Online iş hizmetine abone olursanız, tüm ücretsiz özelliklere erişebilmek için otomatik olarak Azure AD alırsınız.

Azure AD uygulamanızı geliştirmek için, Azure Active Directory Premium P1 veya Premium P2 lisanslarına yükselterek ücretli özellikler de ekleyebilirsiniz. Azure AD ücretli lisansları, mobil kullanıcılarınız için self servis, gelişmiş izleme, güvenlik raporlaması ve güvenli erişim sağlayan mevcut ücretsiz dizininizin üzerine inşa edilmiştir.

>[!Note]
>Bu lisansların fiyatlandırma seçenekleri için [Azure Etkin Dizin](https://azure.microsoft.com/pricing/details/active-directory/)Fiyatlandırması'na bakın.
>
>Azure Active Directory Premium P1 ve Premium P2 şu anda Çin'de desteklenmez. Azure AD fiyatlandırması hakkında daha fazla bilgi için [Azure Etkin Dizin Forumu'na](https://azure.microsoft.com/support/community/?product=active-directory)başvurun.

- **Azure Active Directory Free.** Kullanıcı ve grup yönetimi, şirket içi dizin eşitleme, temel raporlar, bulut kullanıcıları için self servis parola değişikliği ve Azure, Office 365 ve birçok popüler SaaS uygulamasında tek oturum açma sağlar.

- **Azure Active Directory Premium P1.** Ücretsiz özelliklere ek olarak, P1 ayrıca hibrit kullanıcılarınızın hem şirket içi hem de bulut kaynaklarına erişmesine olanak tanır. Ayrıca dinamik gruplar, self servis grup yönetimi, Microsoft Identity Manager (şirket içi kimlik ve erişim yönetimi paketi) ve otomatik kullanım parola sıfırlama özelliği gibi gelişmiş yönetimi de destekler şirket içi kullanıcılarınız.

- **Azure Active Directory Premium P2.** Ücretsiz ve P1 özelliklerine ek olarak P2, yöneticilerin ve kaynakların erişimini keşfetmeye, kısıtlamaya ve izlemeye ve gerektiğinde tam zamanında erişim sağlamaya yardımcı olmak için uygulamalarınıza ve kritik şirket verilerinize risk tabanlı Koşullu Erişim ve [Ayrıcalıklı Kimlik Yönetimi](../privileged-identity-management/pim-getting-started.md) sağlamaya yardımcı olmak için Azure Active [Directory Identity Protection](../identity-protection/overview-identity-protection.md) da sunar.

- **"Gittikçe öde" özelliği lisansları.** Azure Active Directory Business-to-Customer (B2C) gibi ek özellik lisansları da alabilirsiniz. B2C, müşteriye yönelik uygulamalarınız için kimlik ve erişim yönetimi çözümleri sağlamanıza yardımcı olabilir. Daha fazla bilgi için Azure [Active Directory B2C belgelerine](../../active-directory-b2c/index.yml)bakın.

Azure AD aboneliğini ilişkilendirme hakkında daha fazla bilgi için [bkz: Azure Etkin Dizin'ine Azure aboneliğini nasıl ilişkilendirin veya ekleyin](active-directory-how-subscriptions-associated-directory.md) ve kullanıcılarınıza lisans atama hakkında daha fazla bilgi için Azure [Etkin Dizin lisanslarını nasıl atayın veya kaldırın.](license-users-groups.md)

## <a name="terminology"></a>Terminoloji

Azure REKLAM'ı ve belgelerini daha iyi anlamak için aşağıdaki koşulları gözden geçirmenizi öneririz.

|Terim veya kavram|Açıklama|
|---------------|-----------|
|Kimlik| Doğrulayabilen bir şey. Kimlik, kullanıcı adı ve parolaiçeren bir kullanıcı olabilir. Kimlikler, gizli anahtarlar veya sertifikalar aracılığıyla kimlik doğrulaması gerektirebilecek uygulamaları veya diğer sunucuları da içerir.|
|Hesap| Onunla ilişkili verilere sahip bir kimlik. Kimlik olmadan bir hesabınız olamaz.|
|Azure AD hesabı| Azure AD veya Office 365 gibi başka bir Microsoft bulut hizmeti aracılığıyla oluşturulan kimlik. Kimlikler Azure AD'de depolanır ve kuruluşunuzun bulut hizmeti abonelikleri tarafından erişilebilir. Bu hesap bazen Çalışma veya okul hesabı olarak da adlandırılır.|
|Azure aboneliği| Azure bulut hizmetleri için ödeme yapmak için kullanılır. Birçok aboneliğiniz olabilir ve bunlar kredi kartına bağlıdır.|
|Azure kiracısı| Kuruluşunuz Microsoft Azure, Microsoft Intune veya Office 365 gibi bir Microsoft bulut hizmeti aboneliğine kaydolduğunda otomatik olarak oluşturulan özel ve güvenilir bir Azure REKLAM örneği. Azure kiracısı tek kuruluşu temsil eder.|
|Tek kiracılı| Özel bir ortamda diğer hizmetlere erişen Azure kiracıları tek kiracı olarak kabul edilir.|
|Çok Kiracılı| Paylaşılan bir ortamda, birden çok kuruluşta diğer hizmetlere erişen Azure kiracıları çok kiracı olarak kabul edilir.|
|Azure AD dizini|Her Azure kiracının özel ve güvenilir bir Azure REKLAM dizini vardır. Azure REKLAM dizini kiracının kullanıcılarını, gruplarını ve uygulamalarını içerir ve kiracı kaynakları için kimlik ve erişim yönetimi işlevlerini gerçekleştirmek için kullanılır.|
|Özel etki alanı|Her yeni Azure REKLAM dizininde domainname.onmicrosoft.com bir başlangıç etki alanı adı vardır. Bu ilk ada ek olarak, kuruluşunuzun iş yapmak için kullandığınız adları ve kullanıcılarınızın kuruluşunuzun kaynaklarına erişmek için kullandığı adları da içeren alan adlarını da listeye ekleyebilirsiniz. Özel alan adları eklemek, kullanıcılarınıza tanıdık kullanıcı adları oluşturmanıza yardımcı olur. alain@contoso.com|
|Hesap Yöneticisi|Bu klasik abonelik yöneticisi rolü kavramsal olarak bir aboneliğin fatura sahibidir. Bu rol Azure [Hesap Merkezi'ne](https://account.azure.com/Subscriptions) erişebilir ve bir hesaptaki tüm abonelikleri yönetmenize olanak tanır. Daha fazla bilgi için [Bkz. Klasik abonelik yöneticisi rolleri, Azure Rolü tabanlı erişim denetimi (RBAC) rolleri ve Azure AD yöneticisi rolleri.](../../role-based-access-control/rbac-and-directory-admin-roles.md)|
|Hizmet Yöneticisi|Bu klasik abonelik yöneticisi rolü, erişim de dahil olmak üzere tüm Azure kaynaklarını yönetmenize olanak tanır. Bu rol, abonelik kapsamında Sahip rolü atanan bir kullanıcının eşdeğer erişimine sahiptir. Daha fazla bilgi için [Bkz. Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure REKLAM yöneticisi rolleri.](../../role-based-access-control/rbac-and-directory-admin-roles.md)|
|Sahip|Bu rol, erişim de dahil olmak üzere tüm Azure kaynaklarını yönetmenize yardımcı olur. Bu rol, Azure kaynaklarına ince taneli erişim yönetimi sağlayan rol temel erişim denetimi (RBAC) adı verilen daha yeni bir yetkilendirme sistemi üzerine oluşturulur. Daha fazla bilgi için [Bkz. Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure REKLAM yöneticisi rolleri.](../../role-based-access-control/rbac-and-directory-admin-roles.md)|
|Azure AD Genel yöneticisi|Bu yönetici rolü, Azure AD kiracısını oluşturan kişiye otomatik olarak atanır. Global yöneticiler, Azure AD'nin tüm yönetim işlevlerini ve Exchange Online, SharePoint Online ve Skype for Business Online gibi Azure AD'ye aktaran tüm hizmetleri yapabilir. Birden çok Global yöneticiniz olabilir, ancak kullanıcılara yönetici rolleri (diğer Global yöneticilerin atanması dahil) yalnızca Global yöneticiler atayabilir.<br><br>**Not**<br>Bu yönetici rolü, Azure portalında Global yönetici olarak adlandırılır, ancak Microsoft Graph API ve Azure AD PowerShell'deki **Şirket yöneticisi** olarak adlandırılır.<br><br>Çeşitli yönetici rolleri hakkında daha fazla bilgi için [Azure Etkin Dizini'nde Yönetici rol izinlerine](../users-groups-roles/directory-assign-admin-roles.md)bakın.|
|Microsoft hesabı (msa olarak da adlandırılır)|Outlook, OneDrive, Xbox LIVE veya Office 365 gibi tüketici odaklı Microsoft ürünlerinize ve bulut hizmetlerine erişim sağlayan kişisel hesaplar. Microsoft hesabınız, Microsoft tarafından işletilen Microsoft tüketici kimliği hesap sisteminde oluşturulur ve depolanır.|

## <a name="which-features-work-in-azure-ad"></a>Azure AD'de hangi özellikler çalışır?

Azure AD lisansınızı seçtikten sonra, kuruluşunuz için aşağıdaki özelliklerden bazılarına veya tümüne erişebilirsiniz:

|Kategori|Açıklama|
|-------|-----------|
|Uygulama yönetimi|Uygulama Proxy,tek oturum açma, Uygulamalar ım portalı (Access paneli olarak da bilinir) ve Hizmet Olarak Yazılım (SaaS) uygulamalarını kullanarak bulut ve şirket içi uygulamalarınızı yönetin. Daha fazla bilgi için, şirket içi uygulamalara ve [Uygulama Yönetimi belgelerine](../manage-apps/index.yml) [güvenli uzaktan erişim innasıl sağlayabileceğinize](../manage-apps/application-proxy.md) bakın.|
|Kimlik doğrulaması|Azure Active Directory self servis parola sıfırlama, Çok Faktörlü Kimlik Doğrulama, özel yasaklı parola listesini ve akıllı kilitlemeyi yönetin. Daha fazla bilgi için [Azure AD Kimlik Doğrulama belgelerine](../authentication/index.yml)bakın.|
|İşletmeden İşletmeye (B2B)|Kendi kurumsal verileriniz üzerinde denetim sağlarken konuk kullanıcılarınızı ve harici iş ortaklarınızı yönetin. Daha fazla bilgi için Azure [Active Directory B2B belgelerine](../b2b/index.yml)bakın.|
|İşletmeden Müşteriye (B2C)|Uygulamalarınızı kullanırken kullanıcıların nasıl kaydolduğunu, oturum açtığını ve profillerini nasıl yönetlerini özelleştirin ve denetleyin. Daha fazla bilgi için Azure [Active Directory B2C belgelerine](../../active-directory-b2c/index.yml)bakın.|
|Koşullu Erişim|Bulut uygulamalarınız için erişimi yönetin. Daha fazla bilgi için Azure [AD Koşullu Erişim belgelerine](../conditional-access/index.yml)bakın.|
|Geliştiriciler için Azure Active Directory|Tüm Microsoft kimliklerini oturum açan uygulamalar oluşturun, Microsoft Graph, diğer Microsoft API'lerini veya özel API'leri aramak için belirteçler edinin. Daha fazla bilgi için [Microsoft kimlik platformuna (geliştiriciler için Azure Etkin Dizin)](../develop/index.yml)bakın.|
|Aygıt Yönetimi|Buluttaki veya şirket içindeki cihazlarınızın kurumsal verilerinize erişimini yönetin. Daha fazla bilgi için Azure [AD Aygıt Yönetimi belgelerine](../devices/index.yml)bakın.|
|Etki alanı hizmetleri|Etki alanı denetleyicilerini kullanmadan Azure sanal makinelerine katılarak bir etki alanına katılın. Daha fazla bilgi için Azure [AD Etki Alanı Hizmetleri belgelerine](../../active-directory-domain-services/index.yml)bakın.|
|Kurumsal kullanıcılar|Gruplar ve yönetici rollerini kullanarak lisans atamasını, uygulamalara erişimi yönetin ve temsilciler ayarlayın. Daha fazla bilgi için Azure [Active Directory kullanıcı yönetimi belgelerine](../users-groups-roles/index.yml)bakın.|
|Karma kimlik|Konuma (bulut veya şirket içinde) bakılmaksızın tüm kaynaklara kimlik doğrulama ve yetkilendirme için tek bir kullanıcı kimliği sağlamak için Azure Active Directory Connect ve Connect Health'i kullanın. Daha fazla bilgi için [Karma kimlik belgelerine](../hybrid/index.yml)bakın.|
|Kimlik idaresi|Çalışan, iş ortağı, satıcı, hizmet ve uygulama erişim denetimleri aracılığıyla kuruluşunuzun kimliğini yönetin. Erişim incelemeleri de gerçekleştirebilirsiniz. Daha fazla bilgi için Azure [AD kimlik yönetimi belgelerine](../governance/identity-governance-overview.md) ve [Azure AD erişim incelemelerini](../governance/access-reviews-overview.md)görün.|
|Kimlik koruması|Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılayın, ilkeleri şüpheli eylemlere yanıt verecek şekilde yapılandırın ve bunları çözmek için uygun eylemi gerçekleştirin. Daha fazla bilgi için Azure [AD Kimlik Koruması'na](../identity-protection/index.yml)bakın.|
|Azure kaynakları için yönetilen kimlikler|Azure hizmetlerinize Azure AD'de, Key Vault da dahil olmak üzere Azure AD destekli tüm kimlik doğrulama hizmetlerinin kimliğini doğrulayabilen otomatik olarak yönetilen bir kimlik sağlar. Daha fazla bilgi için azure [kaynakları için yönetilen kimlikler nedir?](../managed-identities-azure-resources/overview.md)|
|Privileged identity management (PIM)|Kuruluşunuzdaki erişimi yönetin, denetler ve izleyin. Bu özellik, Azure AD ve Azure'daki kaynaklara ve Office 365 veya Intune gibi diğer Microsoft Çevrimiçi Hizmetlerine erişimi içerir. Daha fazla bilgi için Azure [AD Ayrıcalıklı Kimlik Yönetimi'ne](../privileged-identity-management/index.yml)bakın.|
|Raporlar ve izleme|Ortamınızdaki güvenlik ve kullanım alışkanlıkları hakkında bilgi edinin. Daha fazla bilgi için [Azure Etkin Dizin raporları ve izleme](../reports-monitoring/index.yml)ye bakın.|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory Premium'a kaydolma](active-directory-get-started-premium.md)

- [Azure aboneliğini Azure Etkin Dizininizde ilişkilendirin](active-directory-how-subscriptions-associated-directory.md)

- [Azure Etkin Dizini'ne erişin ve yeni bir kiracı oluşturun](active-directory-access-create-new-tenant.md)

- [Azure Active Directory Premium P2 özelliği dağıtım denetim listesi](active-directory-deployment-checklist-p2.md)
