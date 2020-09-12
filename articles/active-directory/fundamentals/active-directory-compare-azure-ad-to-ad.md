---
title: Active Directory Azure Active Directory karşılaştırın
description: Bu belge Active Directory Domain Services (ekler) Azure Active Directory (AD) ile karşılaştırır. Her iki kimlik çözümünden de temel kavramları özetler ve bunların nasıl farklı olduğunu veya benzer olduğunu açıklar.
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 02/26/2020
ms.author: martinco
ms.openlocfilehash: e71ed9655c7b195fea8a2eeeaa76d8a28717637f
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89318565"
---
# <a name="compare-active-directory-to-azure-active-directory"></a>Active Directory Azure Active Directory karşılaştırın

Azure Active Directory, bulut için kimlik ve erişim yönetimi çözümlerinin bir sonraki gelişmidir. Microsoft, kuruluşların Kullanıcı başına tek bir kimlik kullanarak birden çok şirket içi altyapı bileşenini ve sistemini yönetmesine olanak tanımak için Windows 2000 ' de Active Directory Domain Services sunmuştur.

Azure AD, kuruluşların bulut ve şirket genelinde tüm uygulamalarına yönelik bir hizmet olarak kimlik (IDaaS) çözümü sağlayarak bu yaklaşımı bir sonraki düzeye götürür.

Çoğu BT yöneticileri Active Directory Domain Services kavramları öğrenmelidir. Aşağıdaki tabloda, Active Directory kavramları ve Azure Active Directory arasındaki farklar ve benzerlikler özetlenmektedir.

|Konsept|Active Directory (AD)|Azure Active Directory |
|:-|:-|:-|
|**Kullanıcılar**|||
|Sağlama: kullanıcılar | Kuruluşlar, iç kullanıcıları el ile oluşturur veya bir ık sistemle tümleştirilecek Microsoft Identity Manager gibi bir şirket içi veya otomatik sağlama sistemi kullanır.|Mevcut AD kuruluşları kimlikleri buluta eşitlemek için [Azure AD Connect](../hybrid/how-to-connect-sync-whatis.md) kullanır.</br> Azure AD, otomatik olarak [bulut HR sistemlerinden](../saas-apps/workday-tutorial.md)Kullanıcı oluşturmaya yönelik destek ekler. </br>Azure AD, kullanıcıların erişimine izin vermek için gerekli ayrıntıları kullanarak uygulamaları otomatik olarak sağlamak üzere [SCIM etkin](../app-provisioning/use-scim-to-provision-users-and-groups.md) SaaS uygulamalarında kimlikler sağlayabilir. |
|Sağlama: dış kimlikler| Kuruluşlar, özel bir dış AD ormanında düzenli kullanıcılar olarak dış kullanıcılar oluşturur ve dış kimliklerin yaşam döngüsünü yönetmeye yönelik yönetim yüküne neden olur (Konuk kullanıcılar)| Azure AD, dış kimlikleri desteklemek için özel bir kimlik sınıfı sağlar. [Azure AD B2B](/azure/active-directory/b2b/) , dış kullanıcı kimliğine ait bağlantıyı, geçerli olduklarından emin olmak için yönetecektir. |
| Yetkilendirme Yönetimi ve gruplar| Yöneticiler, kullanıcıların grupların üyelerini yapar. Uygulama ve kaynak sahipleri daha sonra gruplara uygulama veya kaynaklara erişim izni verir.| [Gruplar](./active-directory-groups-create-azure-portal.md) da Azure AD 'de kullanılabilir ve Yöneticiler, kaynaklara izin vermek için grupları da kullanabilir. Azure AD 'de Yöneticiler gruplara el ile üyelik atayabilir veya bir gruba kullanıcıları dinamik olarak dahil etmek için bir sorgu kullanabilir. </br> Yöneticiler, kullanıcılara iş akışlarını kullanarak bir uygulama ve kaynak koleksiyonuna erişim sağlamak için Azure AD 'de [Yetkilendirme yönetimini](../governance/entitlement-management-overview.md) kullanabilir ve gerekirse, zaman tabanlı ölçütlerde. |
| Yönetici Yönetimi|Kuruluşlar, denetlediği dizin ve kaynakları yönetmek için yönetim hakları sağlamak üzere AD 'de etki alanları, kuruluş birimleri ve grupların birleşimini kullanacaktır.| Azure AD, Azure AD rol tabanlı erişim denetimi (Azure AD RBAC) sistemiyle birlikte [yerleşik roller](./active-directory-users-assign-role-azure-portal.md) sağlar ve bu sayede kimlik sistemine, ayrıcalıklı erişim sağlamak için [özel roller oluşturmaya](../users-groups-roles/roles-custom-overview.md) yönelik sınırlı destek sağlar.</br>Rolleri yönetmek, ayrıcalıklı rollere tam zamanında, zaman kısıtlı veya iş akışı tabanlı erişim sağlamak için [Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) ile iyileştirilen bir şekilde geliştirilebilir. |
| Kimlik bilgisi yönetimi| Active Directory kimlik bilgileri, parolaları, sertifika kimlik doğrulamasını ve akıllı kart kimlik doğrulamasını temel alır. Parolalar, parola uzunluğu, süre sonu ve karmaşıklık temelinde parola ilkeleri kullanılarak yönetilir.|Azure AD, bulut ve şirket içi için akıllı [parola koruması](../authentication/concept-password-ban-bad.md) kullanır. Koruma, akıllı kilitleme ile ortak ve özel parola tümceciklerini ve alternatifleri engellemeyi içerir. </br>Azure AD, [çok faktörlü kimlik doğrulaması](../authentication/concept-mfa-howitworks.md) ve FIDO2 gibi [parolasız](../authentication/concept-authentication-passwordless.md) teknolojiler aracılığıyla güvenliği önemli ölçüde artırır. </br>Azure AD, kullanıcılara bir [self servis parola sıfırlama](../authentication/concept-sspr-howitworks.md) sistemi sağlayarak destek maliyetlerini azaltır. |
| **Uygulamalar**|||
| Altyapı uygulamaları|Active Directory, Örneğin DNS, DHCP, IPSec, WiFi, NPS ve VPN erişimi gibi birçok altyapı şirket içi bileşeninin temelini oluşturur|Azure AD, yeni bir bulut dünyasında ağ denetimlerine bağlı olarak uygulamalara erişmek için yeni denetim düzledir. Kullanıcılar kimlik doğrulaması yaparken[, koşullu erişim (CA)](../conditional-access/overview.md), hangi kullanıcıların gerekli koşullar altında hangi uygulamalara erişebileceğini denetler.|
| Geleneksel ve eski uygulamalar| Çoğu şirket içi uygulama, kullanıcılara erişimi denetlemek için LDAP, Windows ile tümleşik kimlik doğrulaması (NTLM ve Kerberos) veya üst bilgi tabanlı kimlik doğrulaması kullanır.| Azure AD, şirket içi çalışan [Azure AD uygulama proxy](../manage-apps/application-proxy.md) aracılarını kullanarak bu tür şirket içi uygulamalara erişim sağlayabilir. Bu yöntemi kullanarak Azure AD, geçiş yaparken veya eski uygulamalarla birlikte bir arada olması gerektiğinde, şirket içi kullanıcıların Active Directory kimlik doğrulaması yapabilir. |
| SaaS uygulamaları|Active Directory, SaaS uygulamalarını yerel olarak desteklemez ve AD FS gibi Federasyon sistemi gerektirir.|OAuth2, SAML ve WS kimlik doğrulamasını destekleyen SaaS uygulamaları, \* kimlik doğrulaması Için Azure AD kullanacak şekilde tümleştirilebilir. |
| Modern kimlik doğrulaması ile iş kolu (LOB) uygulamaları|Kuruluşlar, modern kimlik doğrulaması gerektiren LOB uygulamalarını desteklemek için Active Directory AD FS kullanabilir.| Modern kimlik doğrulaması gerektiren LOB uygulamaları, kimlik doğrulaması için Azure AD kullanacak şekilde yapılandırılabilir. |
| Orta katman/Daemon Hizmetleri|Şirket içi ortamlarda çalışan hizmetler normalde, çalıştırılacak AD hizmet hesaplarını veya grup yönetilen hizmet hesaplarını (gMSA) kullanır. Bu uygulamalar daha sonra hizmet hesabının izinlerini devralacak.| Azure AD, bulutta diğer iş yüklerini çalıştırmak için [Yönetilen kimlikler](../managed-identities-azure-resources/index.yml) sağlar. Bu kimliklerin yaşam döngüsü Azure AD tarafından yönetilir ve kaynak sağlayıcısına bağlı olan, biriktirme listesi erişimi sağlamak için başka amaçlar için kullanılamaz.|
| **Cihazlar**|||
| Mobil|Active Directory, üçüncü taraf çözümler olmadan mobil cihazları yerel olarak desteklemez.| Microsoft 'un mobil cihaz yönetimi çözümü, Microsoft Intune Azure AD ile tümleşiktir. Microsoft Intune kimlik doğrulaması sırasında değerlendirilecek kimlik sistemine cihaz durum bilgilerini sağlar. |
| Windows masaüstleri|Active Directory, Windows cihazlarını grup ilkesi, System Center Configuration Manager veya diğer üçüncü taraf çözümlerini kullanarak yönetmek için etki alanına ekleme olanağı sunar.|Windows cihazları [Azure AD 'ye katılabilir](../devices/index.yml). Koşullu erişim, bir cihazın kimlik doğrulama işleminin bir parçası olarak Azure AD 'ye katılıp katılmadığını denetleyebilir. Windows cihazları, [Microsoft Intune](/intune/what-is-intune)ile de yönetilebilir. Bu durumda, koşullu erişim, uygulamalara erişim izni vermeden önce cihazın uyumlu olup olmadığını (örneğin, güncel güvenlik düzeltme ekleri ve virüs imzaları) göz önünde bulunduracaktır.|
| Windows sunucuları| Active Directory, şirket içi Windows Server 'lar için grup ilkesi veya diğer yönetim çözümlerini kullanarak güçlü yönetim özellikleri sağlar.| Azure 'daki Windows Server sanal makineleri, [Azure AD Domain Services](../../active-directory-domain-services/index.yml)ile yönetilebilir. [Yönetilen kimlikler](../managed-identities-azure-resources/index.yml) , VM 'lerin kimlik sistemi dizinine veya kaynaklarına erişmesi gerektiğinde kullanılabilir.|
| Linux/Unix iş yükleri|Active Directory, üçüncü taraf çözümleri olmayan Windows dışı bir şekilde desteklemez, ancak Linux makineleri Kerberos bölgesi olarak Active Directory kimlik doğrulaması yapacak şekilde yapılandırılabilirler.|Linux/UNIX VM 'Leri, kimlik sistemine veya kaynaklara erişmek için [Yönetilen kimlikler](../managed-identities-azure-resources/index.yml) kullanabilir. Bazı kuruluşlar, bu iş yüklerini bulut kapsayıcısı teknolojilerine geçirin ve bu da yönetilen kimlikleri de kullanabilir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory nedir?](./active-directory-whatis.md)
- [Kendi kendini yönetilen Active Directory Domain Services, Azure Active Directory ve yönetilen Azure Active Directory Domain Services karşılaştırın](../../active-directory-domain-services/compare-identity-solutions.md)
- [Azure Active Directory hakkında sık sorulan sorular](./active-directory-faq.md)
- [Azure Active Directory yenilikler nelerdir?](./whats-new.md)