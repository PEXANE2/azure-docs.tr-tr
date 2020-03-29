---
title: Etkin Dizini Azure Etkin Dizini ile karşılaştırın
description: Bu belge, Etkin Dizin Etki Alanı Hizmetlerini (ADDS) Azure Etkin Dizini (AD) ile karşılaştırır. Her iki kimlik çözümünde de anahtar kavramları özetler ve nasıl farklı veya benzer olduğunu açıklar.
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
ms.openlocfilehash: 5075ae57df6a7306f0c860690931c846e52c2a89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78926894"
---
# <a name="compare-active-directory-to-azure-active-directory"></a>Etkin Dizini Azure Etkin Dizini ile karşılaştırın

Azure Active Directory, bulut için kimlik ve erişim yönetimi çözümlerinin bir sonraki evrimidir. Microsoft, kuruluşlara kullanıcı başına tek bir kimlik kullanarak birden çok şirket içi altyapı bileşenini ve sistemi yönetme olanağı sağlamak için Windows 2000'de Active Directory Domain Services'ı tanıttı.

Azure AD, kuruluşlara bulut ve şirket içi tüm uygulamaları için Hizmet Olarak Kimlik (IDaaS) çözümü sunarak bu yaklaşımı bir sonraki seviyeye taşır.

BT yöneticilerinin çoğu Active Directory Domain Services kavramlarını bilmektedir. Aşağıdaki tabloda Active Directory kavramları ile Azure Etkin Dizini arasındaki farklar ve benzerlikler sıralanmaktadır.

|Kavram|Aktif Dizin (AD)|Azure Active Directory |
|:-|:-|:-|
|**Kullanıcılar**|||
|Sağlama: kullanıcılar | Kuruluşlar, bir İk sistemiyle tümleştirmek için dahili kullanıcıları el ile oluşturur veya Microsoft Identity Manager gibi şirket içi veya otomatik bir sağlama sistemi kullanır.|Varolan AD kuruluşları, kimlikleri bulutla eşitlemek için [Azure AD Connect'i](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) kullanır.</br> Azure [AD, bulut İk sistemlerinden](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial)kullanıcıları otomatik olarak oluşturmak için destek ekler. </br>Azure AD, [SCIM özellikli](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups) SaaS uygulamalarında kimlik sağlayarak uygulamalara kullanıcılara erişim sağlamak için gerekli ayrıntıları otomatik olarak sağlayabilir. |
|Sağlama: dış kimlikler| Kuruluşlar, özel bir dış AD ormanında düzenli kullanıcılar olarak dış kullanıcıları el ile oluşturur ve bu da harici kimliklerin (konuk kullanıcıların) yaşam döngüsünü yönetmek için yönetim yüküyle sonuçlanır| Azure AD, dış kimlikleri desteklemek için özel bir kimlik sınıfı sağlar. [Azure AD B2B,](https://docs.microsoft.com/azure/active-directory/b2b/) geçerli olduğundan emin olmak için harici kullanıcı kimliğibağlantısını yönetir. |
| Yetki yönetimi ve gruplar| Yöneticiler, kullanıcıları grupların üyeleri yapar. Uygulama ve kaynak sahipleri daha sonra gruplara uygulamalara veya kaynaklara erişim sağlar.| [Gruplar](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) Azure AD'de de kullanılabilir ve yöneticiler kaynaklara izin vermek için grupları kullanabilir. Azure AD'de yöneticiler gruplara el ile üyelik atayabilir veya kullanıcıları bir gruba dinamik olarak dahil etmek için sorgu kullanabilir. </br> Yöneticiler, kullanıcılara iş akışlarını ve gerekirse zamana dayalı ölçütleri kullanarak bir uygulama ve kaynak koleksiyonuna erişim sağlamak için Azure AD'deki [Yetkilendirme yönetimini](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) kullanabilir. |
| Yönetici yönetimi|Kuruluşlar, denetlenen dizin ve kaynakları yönetmek için yönetim haklarını devretmek için AD'deki etki alanları, kuruluş birimleri ve grupların bir birleşimini kullanır.| Azure AD, rol tabanlı erişim denetimi (RBAC) sistemiyle [yerleşik roller](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) sağlar ve kimlik sistemine, uygulamalara ve denetlediğiniz kaynaklara ayrıcalıklı erişimi devretmek için [özel roller oluşturmak](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview) için sınırlı destek sağlar.</br>Rolleri yönetmek, ayrıcalıklı rollere tam zamanında, zaman kısıtlamalı veya iş akışı tabanlı erişim sağlamak için [Ayrıcalıklı Kimlik Yönetimi (PIM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) ile geliştirilebilir. |
| Kimlik bilgisi yönetimi| Etkin Dizini'ndeki kimlik bilgileri parolaları, sertifika kimlik doğrulamasını ve akıllı kart kimlik doğrulamasını temel alar. Parolalar, parola uzunluğu, son kullanma tarihi ve karmaşıklığı temel alan parola ilkeleri kullanılarak yönetilir.|Azure AD, bulut ve şirket içi için akıllı [parola koruması](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad) kullanır. Koruma akıllı kilitleme yanı sıra ortak ve özel parola ifadeleri ve değiştirmeengelleme içerir. </br>Azure AD, FIDO2 gibi [çok faktörlü kimlik doğrulama](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) ve [parolasız](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless) teknolojiler aracılığıyla güvenliği önemli ölçüde artırır. </br>Azure AD, kullanıcılara self [servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks) sistemi sağlayarak destek maliyetlerini azaltır. |
| **Uygulamalar**|||
| Altyapı uygulamaları|Etkin Dizin, DNS, DHCP, IPSec, WiFi, NPS ve VPN erişimi gibi birçok altyapı şirket içi bileşeninin temelini oluşturur|Yeni bir bulut dünyasında, Azure AD, uygulamalara erişmek ve ağ denetimleri için yeni bir denetim düzlemidir. Kullanıcılar kimlik doğrulaması[yaptığında, Koşullu erişim (CA)](https://docs.microsoft.com/azure/active-directory/conditional-access/overview), hangi kullanıcıların hangi uygulamalara erişeceğini, gerekli koşullar altında hangi uygulamalara erişeceğini denetler.|
| Geleneksel ve eski uygulamalar| Şirket içi uygulamaların çoğu, kullanıcılara erişimi denetlemek için LDAP, Windows-Integrated Authentication (NTLM ve Kerberos) veya Üstbilgi tabanlı kimlik doğrulamasını kullanır.| Azure AD, şirket içinde çalışan [Azure AD uygulama proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) aracılarını kullanarak bu tür şirket içi uygulamalara erişim sağlayabilir. Bu yöntemi kullanarak Azure AD, siz geçiş yaparken veya eski uygulamalarla bir arada bulunmanız gerekirken Kerberos'u kullanarak Etkin Dizin kullanıcılarının kimliğini doğrulayabilir. |
| SaaS uygulamaları|Active Directory, SaaS uygulamalarını yerel olarak desteklemez ve AD FS gibi federasyon sistemi gerektirir.|OAuth2, SAML ve WS kimlik\* doğrulamasını destekleyen SaaS uygulamaları, kimlik doğrulama için Azure AD'yi kullanmak üzere tümleştirilebilir. |
| Modern kimlik doğrulaması ile iş hattı (LOB) uygulamaları|Kuruluşlar, modern kimlik doğrulaması gerektiren LOB uygulamalarını desteklemek için Active Directory ile AD FS'yi kullanabilir.| Modern kimlik doğrulaması gerektiren LOB uygulamaları, kimlik doğrulaması için Azure AD'yi kullanacak şekilde yapılandırılabilir. |
| Orta kademe/Daemon hizmetleri|Şirket içi ortamlarda çalışan hizmetler, normalde çalıştırmak için AD hizmet hesaplarını veya Yönetilen Hizmet Hesapları grubunu (gMSA) kullanır. Bu uygulamalar daha sonra hizmet hesabının izinlerini devralır.| Azure AD, buluttaki diğer iş yüklerini çalıştırmak için [yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/index) sağlar. Bu kimliklerin yaşam döngüsü Azure AD tarafından yönetilir ve kaynak sağlayıcısına bağlıdır, arka kapı erişimi elde etmek için başka amaçlarla kullanılamaz.|
| **Cihazlar**|||
| Mobil|Active Directory, üçüncü taraf çözümleri olmadan mobil cihazları yerel olarak desteklemez.| Microsoft'un mobil cihaz yönetimi çözümü Microsoft Intune, Azure AD ile entegre edilmiştir. Microsoft Intune kimlik doğrulama sırasında değerlendirmek için kimlik sistemine aygıt durumu bilgileri sağlar. |
| Windows masaüstü bilgisayarlar|Active Directory, Grup İlkesi, Sistem Merkezi Yapılandırma Yöneticisi veya diğer üçüncü taraf çözümleri kullanarak windows aygıtlarına katılma olanağı sağlar.|Windows aygıtları [Azure AD'ye katılabilir.](https://docs.microsoft.com/azure/active-directory/devices/) Koşullu erişim, kimlik doğrulama işleminin bir parçası olarak biraygıta Azure AD katılıp katılmaz olmadığını denetleyebilir. Windows aygıtları Microsoft [Intune](https://docs.microsoft.com/intune/what-is-intune)ile de yönetilebilir. Bu durumda, koşullu erişim, uygulamalara erişime izin vermeden önce bir aygıtın şikayet olup olmadığını (örneğin, güncel güvenlik düzeltme emültleri ve virüs imzaları) dikkate alır.|
| Windows sunucuları| Etkin Dizin, Grup İlkesi veya diğer yönetim çözümlerini kullanarak şirket içi Windows sunucuları için güçlü yönetim özellikleri sağlar.| Azure'daki Windows sunucuları sanal makineleri [Azure AD Etki Alanı Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/)ile yönetilebilir. [Yönetilen kimlikler,](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/index) VM'lerin kimlik sistemi dizinine veya kaynaklarına erişilmesi gerektiğinde kullanılabilir.|
| Linux/Unix iş yükleri|Linux makineleri Bir Kerberos alemi olarak Active Directory ile kimlik doğrulaması için yapılandırılabilir rağmen Active Directory, üçüncü taraf çözümleri olmadan windows olmayan desteklemez.|Linux/Unix [VM'ler,](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/index) kimlik sistemine veya kaynaklara erişmek için yönetilen kimlikleri kullanabilir. Bazı kuruluşlar, bu iş yüklerini yönetilen kimlikleri de kullanabilen bulut kapsayıcı teknolojilerine geçirin.|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizin nedir?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
- [Kendi kendini yöneten Active Directory Etki Alanı Hizmetlerini, Azure Etkin Dizini ve yönetilen Azure Etkin Dizin Etki Alanı Hizmetlerini karşılaştırın](https://docs.microsoft.com/azure/active-directory-domain-services/compare-identity-solutions)
- [Azure Etkin Dizini hakkında sık sorulan sorular](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-faq)
- [Azure Etkin Dizin'deki yenilikler nelerdir?](https://docs.microsoft.com/azure/active-directory/fundamentals/whats-new)
