---
title: Azure Active Directory nedir? -Azure Active Directory | Microsoft Docs
description: Terminoloji, kullanılabilir lisanslar ve daha fazla bilgi için bağlantılarla ilişkili özelliklerin bir listesi de dahil olmak üzere Azure Active Directory hakkında genel bakış ve kavramsal bilgiler.
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79240789"
---
# <a name="what-is-azure-active-directory"></a>Azure Active Directory nedir?

Azure Active Directory (Azure AD), Microsoft 'un bulut tabanlı kimlik ve erişim yönetimi hizmeti olduğundan, çalışanlarınızın ' de oturum açmasını ve kaynaklara erişmesini sağlar.

- Microsoft Office 365, Azure portalı ve diğer SaaS uygulamalarına binlerce gibi dış kaynaklar.

- Şirket ağına ve intranet, kendi kuruluşunuzun içinde geliştirilen bir bulut uygulamalarının yanı sıra uygulamaları gibi iç kaynaklara.

Azure, Azure AD ve Office 365 ' deki temel kimlik hizmetlerini daha iyi anlamak için [kuruluş mimarları serisi](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources#identity) posterleri için çeşitli Microsoft bulut kullanabilirsiniz.

## <a name="who-uses-azure-ad"></a>Azure AD kullanan?

Azure AD için tasarlanmıştır:

- **BT yöneticileri.** BT yöneticisi, uygulamalarınızın ve iş gereksinimlerinize göre uygulama kaynaklarınızı erişimi denetlemek için Azure AD kullanabilirsiniz. Örneğin, önemli kurumsal kaynaklara erişirken çok faktörlü kimlik doğrulaması gerektirmek için Azure AD kullanın. Ayrıca, Azure AD kullanıcı sağlama, mevcut Windows Server AD ve Office 365 dahil olmak üzere bulut uygulamalarınız arasında otomatik hale getirmek için kullanabilirsiniz. Son olarak, Azure AD'ye otomatik olarak kullanıcı kimliklerini ve kimlik bilgilerinin korunmasına yardımcı olmak ve erişim yönetimi gereksinimlerinizi karşılamak için güçlü araçlar sağlar. Başlamak için [30 günlük ücretsiz Azure Active Directory Premium deneme sürümü](https://azure.microsoft.com/trial/get-started-active-directory/)için kaydolun.

- **Uygulama geliştiricileri.** Uygulama geliştiricisi olarak, Azure AD 'yi uygulamanıza çoklu oturum açma (SSO) eklemek için standartlara dayalı bir yaklaşım olarak kullanabilirsiniz, böylece kullanıcının önceden mevcut kimlik bilgileriyle çalışmasına izin verebilirsiniz. Azure AD, mevcut kurumsal verileri kullanarak kişiselleştirilmiş uygulama deneyimleri oluşturmanıza yardımcı olabilecek API 'Ler de sağlar. Başlamak için [30 günlük ücretsiz Azure Active Directory Premium deneme sürümü](https://azure.microsoft.com/trial/get-started-active-directory/)için kaydolun. Daha fazla bilgi için [Azure Active Directory geliştiriciler için](../develop/index.yml)de bkz.

- **Microsoft 365, Office 365, Azure veya Dynamics CRM Online aboneleri.** Abone olarak, Azure AD zaten kullanıyorsunuz. Microsoft 365, Office 365, Azure ve Dynamics CRM Online her kiracıya otomatik olarak Azure AD kiracısı olur. Tümleşik bulut uygulamalarınıza erişimi yönetmek hemen başlayabilirsiniz.

## <a name="what-are-the-azure-ad-licenses"></a>Azure AD lisansı nedir?

Office 365 veya Microsoft Azure gibi Microsoft Online iş Hizmetleri için oturum açma ve kimlik koruması ile yardımcı olmak için Azure AD gerektirir. Herhangi bir Microsoft Online iş hizmetine abone değilseniz, tüm ücretsiz özelliklere erişimi olan Azure AD 'yi otomatik olarak alırsınız.

Azure AD uygulamanızı geliştirmek için, Azure Active Directory Premium P1 veya Premium P2 lisanslarına yükselterek ücretli yetenekler de ekleyebilirsiniz. Azure AD ücretli lisanslar, mobil kullanıcılarınız için self servis, Gelişmiş izleme, güvenlik raporlaması ve güvenli erişim sağlayan mevcut ücretsiz dizininizin üzerine kurulmuştur.

>[!Note]
>Bu lisansların fiyatlandırma seçenekleri için bkz. [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/).
>
>Azure Active Directory Premium P1 ve Premium P2 Şu anda Çin 'de desteklenmemektedir. Azure AD fiyatlandırması hakkında daha fazla bilgi için [Azure Active Directory forumuna](https://azure.microsoft.com/support/community/?product=active-directory)başvurun.

- **Azure Active Directory Ücretsiz.** Kullanıcı ve Grup Yönetimi, şirket içi dizin eşitleme, temel raporlar, bulut kullanıcıları için self servis parola değişikliği ve Azure, Office 365 ve birçok popüler SaaS uygulamasında çoklu oturum açma sağlar.

- **Azure Active Directory Premium P1.** P1, ücretsiz özelliklere ek olarak, karma kullanıcılarınızın hem şirket içi hem de bulut kaynaklarına erişmesine imkan tanır. Gelişmiş yönetimini de destekler, dinamik gruplar, Self Servis Grup Yönetimi, Microsoft Identity Manager (bir şirket içi kimlik ve erişim Yönetim Paketi) ve bulut geri yazma özellikleri gibi Self Servis parola sıfırlama için ver Şirket içi kullanıcılarınızın.

- **Azure Active Directory Premium P2.** Ücretsiz ve P1 özelliklerine ek olarak, P2, uygulamalarınıza ve önemli şirket verilerinize yönelik risk tabanlı koşullu erişim sağlamaya yardımcı olmak için [Azure Active Directory kimlik koruması](../identity-protection/overview-identity-protection.md) , yöneticilerin ve kaynaklara erişimlerini bulma, kısıtlama ve izleme konusunda yardımcı olmak ve gerektiğinde tam zamanında erişim sağlamak için [Privileged Identity Management](../privileged-identity-management/pim-getting-started.md) sunar.

- **"Kullandıkça öde" özelliği lisansları.** Ayrıca, Azure Active Directory iş müşteriye (B2C) gibi ek özellik lisans alabilirsiniz. B2C kimlik sağlama ve erişim yönetimi çözümleri, müşteriye dönük uygulamalarınız için yardımcı olabilir. Daha fazla bilgi için bkz. [Azure Active Directory B2C belgeleri](../../active-directory-b2c/index.yml).

Azure aboneliğini Azure AD 'ye ilişkilendirme hakkında daha fazla bilgi için bkz. [nasıl yapılır: Azure aboneliğini ilişkilendirme veya ekleme Azure Active Directory](active-directory-how-subscriptions-associated-directory.md) ve kullanıcılarınıza lisans atama hakkında daha fazla bilgi için bkz. [nasıl yapılır: Azure Active Directory lisansları atama veya kaldırma](license-users-groups.md).

## <a name="terminology"></a>Terminoloji

Azure AD ve belgelerini daha iyi anlamak için aşağıdaki koşulları gözden geçiririz.

|Kavram veya sözleşme|Açıklama|
|---------------|-----------|
|Kimlik| Kimliği doğrulanmış bir şey. Kimlik, Kullanıcı adı ve parola içeren bir kullanıcı olabilir. Kimlikler Ayrıca gizli anahtarlar veya sertifikalar aracılığıyla kimlik doğrulaması gerektirebilecek uygulamaları veya diğer sunucuları da içerir.|
|Hesap| Kendisiyle ilişkili verilerin bulunduğu bir kimlik. Kimliği olmayan bir hesabınız olamaz.|
|Azure AD hesabı| Azure AD oluşturulan bir kimlik veya Office 365 gibi başka bir Microsoft bulut hizmeti. Azure AD'de depolanan ve kuruluşunuzun bulut hizmeti abonelikleri için erişilebilir Kimlikleridir. Bu hesap bazen çalışma adlandırılır ya da Okul hesabı.|
|Azure aboneliği| Azure bulut Hizmetleri için kullandığınız ödeme yöntemi için kullanılır. Birçok abonelik olabilir ve bir kredi kartına bağlı.|
|Azure kiracısı| Kuruluşunuz Microsoft Azure, Microsoft Intune veya Office 365 gibi Microsoft bulut hizmeti aboneliği kaydolduğunda otomatik olarak oluşturulan Azure ad ayrılmış ve güvenilir bir örneği. Azure kiracısı tek kuruluşu temsil eder.|
|Tek kiracılı| Ayrılmış bir ortamda diğer hizmetlere erişmek azure kiracılarında, tek bir kiracı olarak kabul edilir.|
|Çok Kiracılı| Diğer hizmetler paylaşılan bir ortamda birden çok kuruluşta, erişimi azure kiracılarında, çok kiracılı olarak kabul edilir.|
|Azure AD dizini|Her bir Azure kiracısına sahip adanmış ve güvenilir bir Azure AD dizini. Azure AD dizini, kiracının kullanıcıları, grupları ve uygulamaları içerir ve kimliği gerçekleştirin ve yönetim işlevleri için Kiracı kaynaklarına erişmek için kullanılır.|
|Özel etki alanı|Her yeni Azure AD dizini bir ilk etki alanı adı ile gelir domainname.onmicrosoft.com. Buna ek olarak, ilk adı listesi için kuruluşunuzun kaynaklarına erişmek için iş ve kullanıcılarınızı yapmak için kullandığınız adlarında adlarını kullanın, kuruluşunuzun etki alanı da ekleyebilirsiniz. Özel etki alanı adları eklemek, kullanıcılarınıza bildiğiniz alain@contoso.comgibi kullanıcı adları oluşturmanıza yardımcı olur.|
|Hesap Yöneticisi|Bu Klasik Abonelik Yöneticisi rolü kavramsal olarak bir abonelik fatura sahibidir. Bu rolün [Azure Hesap Merkezi](https://account.azure.com/Subscriptions) erişimi vardır ve bir hesaptaki tüm abonelikleri yönetmenizi sağlar. Daha fazla bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure rol tabanlı erişim denetimi (RBAC) rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Hizmet Yöneticisi|Bu Klasik Abonelik Yöneticisi rolü erişim dahil olmak üzere tüm Azure kaynaklarını yönetmenizi sağlar. Bu rol, abonelik kapsamında sahip rolüne atanan kullanıcı eşdeğer erişebilir. Daha fazla bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Sahip|Bu rolü dahil olmak üzere tüm Azure kaynaklarını yönetmenize yardımcı olur. Bu rol, Azure kaynaklarına ayrıntılı erişim yönetimi sağlar, rol tabanlı erişim denetimi (RBAC) adı verilen yeni bir yetkilendirme sistemine yerleşik olarak bulunur. Daha fazla bilgi için bkz. [Klasik abonelik yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rolleri](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Azure AD Genel yöneticisi|Bu yönetici rolü otomatik olarak, Azure AD kiracısını oluşturan kişiye atandı. Genel Yöneticiler, Azure AD 'ye yönelik tüm yönetim işlevlerini ve Exchange Online, SharePoint Online ve Skype Kurumsal Çevrimiçi sürümü gibi Azure AD 'ye federe Hizmetleri gerçekleştirebilir. Birden çok genel Yöneticiler olabilir, ancak yalnızca genel Yöneticiler, kullanıcılara yönetici rolleri (başka genel Yöneticiler atama dahil olmak üzere) atayabilirsiniz.<br><br>**Not**<br>Bu yönetici rolü Azure portal genel yönetici olarak adlandırılır, ancak Microsoft Graph API ve Azure AD PowerShell 'de **Şirket Yöneticisi** olarak adlandırılır.<br><br>Çeşitli yönetici rolleri hakkında daha fazla bilgi için [Azure Active Directory Içindeki yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md)bölümüne bakın.|
|Microsoft hesabı (olarak da bilinen MSA)|Tüketici yönelimli Microsoft ürünlerine erişim sağlar ve bulut Hizmetleri, Outlook, OneDrive, Xbox LIVE veya Office 365 gibi kişisel hesaplar. Microsoft hesabınızı oluşturulur ve Microsoft tarafından çalıştırılan Microsoft tüketici kimlik hesap sistemi depolanır.|

## <a name="which-features-work-in-azure-ad"></a>Azure AD 'de hangi özellikler çalışıyor?

Azure AD lisansınızı seçtikten sonra, kuruluşunuz için aşağıdaki özelliklerden bazılarına veya tümüne erişebilirsiniz:

|Kategori|Açıklama|
|-------|-----------|
|Uygulama yönetimi|Uygulama Ara sunucusu kullanarak Bulut ve şirket içi uygulamalarınızı yönetin çoklu oturum açma, uygulamalarım portalında (erişim paneli olarak da bilinir) ve yazılım hizmet (SaaS) uygulamaları olarak. Daha fazla bilgi için bkz. Şirket [içi uygulamalara güvenli uzaktan erişim sağlama](../manage-apps/application-proxy.md) ve [uygulama yönetimi belgeleri](../manage-apps/index.yml).|
|Kimlik Doğrulaması|Azure Active Directory Self Servis parola sıfırlama, çok faktörlü kimlik doğrulaması, özel yasaklı parola listesi ve akıllı kilitleme yönetin. Daha fazla bilgi için bkz. [Azure AD kimlik doğrulaması belgeleri](../authentication/index.yml).|
|İşletmeden İşletmeye (B2B)|Konuk kullanıcıları ve dış iş ortakları, şirket verilerinizin kontrolünü korurken yönetin. Daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY B2B belgeleri](../b2b/index.yml).|
|İşletmeden Müşteriye (B2C)|Özelleştirme ve denetim nasıl, kullanıcıların oturum açın ve uygulamalarınızı kullanırken profillerini yönetme. Daha fazla bilgi için bkz. [Azure Active Directory B2C belgeleri](../../active-directory-b2c/index.yml).|
|Koşullu Erişim|Bulut uygulamalarınıza erişimi yönetin. Daha fazla bilgi için bkz. [Azure AD koşullu erişim belgeleri](../conditional-access/index.yml).|
|Geliştiriciler için Azure Active Directory|Tüm Microsoft kimliklerini oturum, Microsoft Graph, diğer Microsoft APIs veya özel API'leri çağırmak için belirteçleri almak uygulamalar oluşturun. Daha fazla bilgi için bkz. [Microsoft Identity platform (geliştiriciler için Azure Active Directory)](../develop/index.yml).|
|Cihaz Yönetimi|Bulutta veya şirket içi cihazlarınızı şirket verilerinize nasıl erişim yönetin. Daha fazla bilgi için bkz. [Azure AD cihaz yönetimi belgeleri](../devices/index.yml).|
|Etki alanı hizmetleri|Azure sanal makinelerini, etki alanı denetleyicileri kullanmak zorunda kalmadan bir etki alanına katılın. Daha fazla bilgi için bkz. [Azure AD Domain Services belgeleri](../../active-directory-domain-services/index.yml).|
|Kurumsal kullanıcılar|Lisans ataması, uygulamalara erişimi yönetme ve temsilciler gruplar ve yönetici rolleri kullanarak ayarlayın. Daha fazla bilgi için bkz. [Kullanıcı yönetimi belgelerini Azure Active Directory](../users-groups-roles/index.yml).|
|Karma kimlik|Kimlik doğrulaması ve yetkilendirme (Bulut veya şirket içi) konumdan bağımsız olarak tüm kaynaklar için bir tek kullanıcı kimlik sağlamak için Azure Active Directory Connect ve Connect Health kullanın. Daha fazla bilgi için bkz. [karma kimlik belgeleri](../hybrid/index.yml).|
|Kimlik idaresi|Kuruluşunuzun kimlik çalışan, iş ortağı, satıcı, hizmet ve uygulama erişim denetimleri aracılığıyla yönetin. Erişim gözden geçirmeleri de gerçekleştirebilirsiniz. Daha fazla bilgi için bkz. [Azure AD Identity idare belgeleri](../governance/identity-governance-overview.md) ve [Azure AD erişim İncelemeleri](../governance/access-reviews-overview.md).|
|Kimlik koruması|Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını kuşkulu eylemleri için yanıt vermek için ilkeler yapılandırın ve ardından bunları gidermek için uygun eylemi gerçekleştirin algılayın. Daha fazla bilgi için bkz. [Azure AD kimlik koruması](../identity-protection/index.yml).|
|Azure kaynakları için yönetilen kimlikler|Azure hizmetlerinizi otomatik olarak yönetilen bir kimlik ile anahtar kasası dahil olmak üzere, herhangi bir Azure AD tarafından desteklenen kimlik doğrulama hizmeti kimlik doğrulaması Azure AD'de sağlar. Daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../managed-identities-azure-resources/overview.md).|
|Privileged identity management (PIM)|Yönetebilir, denetleyebilir ve erişimi kuruluşunuz içinde izleyin. Bu özellik, Azure AD ve Azure 'daki kaynaklara ve Office 365 veya Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi içerir. Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management](../privileged-identity-management/index.yml).|
|Raporlar ve izleme|Ortamınızdaki güvenlik ve kullanım desenleri hakkında Öngörüler elde edin. Daha fazla bilgi için bkz. [Azure Active Directory raporları ve izleme](../reports-monitoring/index.yml).|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory Premium için kaydolun](active-directory-get-started-premium.md)

- [Azure aboneliğini Azure Active Directory ilişkilendir](active-directory-how-subscriptions-associated-directory.md)

- [Azure Active Directory erişin ve yeni bir kiracı oluşturun](active-directory-access-create-new-tenant.md)

- [Azure Active Directory Premium P2 özelliği dağıtım denetim listesi](active-directory-deployment-checklist-p2.md)
