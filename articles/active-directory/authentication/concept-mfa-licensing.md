---
title: Azure Multi-Factor Authentication sürümleri ve tüketim planları
description: Azure Multi-Factor Authentication istemcisi ve kullanılabilecek farklı yöntemler ve sürümler hakkında bilgi edinin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 165b84ee6b124d3f6a04c8db177ef17e32784ff9
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83757424"
---
# <a name="features-and-licenses-for-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication yönelik özellikler ve lisanslar

Kuruluşunuzdaki Kullanıcı hesaplarını korumak için Multi-Factor Authentication kullanılmalıdır. Bu özellik özellikle kaynaklara ayrıcalıklı erişimi olan hesaplar için önemlidir. Temel Multi-Factor Authentication özellikleri, Office 365 ve Azure Active Directory (Azure AD) yöneticileri tarafından ek maliyet olmadan kullanılabilir. Yöneticileriniz için özellikleri yükseltmek veya Multi-Factor Authentication 'ı kullanıcılarınızın geri kalanına genişletmek istiyorsanız, Azure Multi-Factor Authentication çeşitli yollarla satın alabilirsiniz.

> [!IMPORTANT]
> Bu makalede, Azure Multi-Factor Authentication lisanslanması ve kullanılması için kullanabileceğiniz farklı yollar ayrıntılı olarak açıklanır. Fiyatlandırma ve faturalandırma hakkında belirli Ayrıntılar için bkz. [Azure Multi-Factor Authentication fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication 'nin kullanılabilir sürümleri

Azure Multi-Factor Authentication, kuruluşunuzun ihtiyaçlarına bağlı olarak birkaç farklı şekilde kullanılabilir ve lisanslanabilir. Şu anda sahip olduğunuz Azure AD, Office 365, EMS veya Microsoft 365 lisansına bağlı olarak Azure Multi-Factor Authentication kullanmaya hak kazanmış olabilirsiniz. Aşağıdaki tabloda Azure Multi-Factor Authentication almanın farklı yolları ve her biri için bazı özellikler ve kullanım örnekleri ayrıntılı olarak verilmiştir.

| Bir kullanıcısı | Yetenekler ve kullanım örnekleri |
| --- | --- |
| EMS veya Microsoft 365 E3 ve E5 | EMS E3 veya Microsoft 365 E3 (EMS ve Office 365 dahil), Azure AD Premium P1 içerir. EMS E5 veya Microsoft 365 E5, Azure AD Premium P2 içerir. Kullanıcılara Multi-Factor Authentication sağlamak için aşağıdaki bölümlerde belirtilen koşullu erişim özelliklerinin aynısını kullanabilirsiniz. |
| Azure AD Premium P1 | Belirli senaryolar veya olaylar sırasında iş gereksinimlerinize uyacak şekilde kullanıcılardan çok faktörlü kimlik doğrulaması için kullanıcı istemek üzere [Azure AD koşullu erişim](../conditional-access/howto-conditional-access-policy-all-users-mfa.md) kullanabilirsiniz. |
| Azure AD Premium P2 | En güçlü güvenlik konumunu ve geliştirilmiş Kullanıcı deneyimini sağlar. Kullanıcının desenlerine uyum sağlayan Azure AD Premium P1 özelliklerine [risk tabanlı koşullu erişim](../conditional-access/howto-conditional-access-policy-risk.md) ekler ve Multi-Factor Authentication istemlerini en aza indirir. |
| Office 365 Business Premium, E3 veya E5 | Azure Multi-Factor Authentication, [Kullanıcı başına temelinde etkinleştirilebilir](howto-mfa-userstates.md)veya tüm kullanıcılar için güvenlik varsayılanları kullanılarak tüm oturum açma olayları için etkinleştirilebilir veya devre dışı bırakılabilir. Azure Multi-Factor Authentication yönetimi, Office 365 portalından oluşur. Gelişmiş bir kullanıcı deneyimi için Azure AD Premium P1 veya P2 sürümüne yükseltin ve koşullu erişimi kullanın. Daha fazla bilgi için bkz. [Multi-Factor Authentication ile güvenli Office 365 kaynakları](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). |
| Azure AD ücretsiz | Her bir kimlik doğrulama isteği yapıldığında, tüm kullanıcılar için Multi-Factor Authentication 'ı etkinleştirmek üzere [güvenlik varsayılanlarını](../fundamentals/concept-fundamentals-security-defaults.md) kullanabilirsiniz. Etkin kullanıcılar veya senaryolar üzerinde ayrıntılı bir denetiminiz yoktur, ancak bu ek güvenlik adımını sağlar.<br /> Herkes için çok faktörlü kimlik doğrulamasını etkinleştirmek üzere güvenlik Varsayılanları kullanılmasa bile, *Azure AD Genel yönetici* rolü atanan kullanıcılar Multi-Factor Authentication kullanacak şekilde yapılandırılabilir. Ücretsiz katmanın bu özelliği, kritik yönetici hesaplarının Multi-Factor Authentication tarafından korunduğundan emin olmanızı sağlar. |

## <a name="feature-comparison-of-versions"></a>Sürümlerin Özellik Karşılaştırması

Aşağıdaki tabloda, çeşitli Azure Multi-Factor Authentication sürümlerinde kullanılabilen özelliklerin bir listesi verilmiştir. Kullanıcı kimlik doğrulamasının güvenliğini sağlamak için gereksinimlerinizi planlayın, ardından hangi yaklaşımın bu gereksinimleri karşıladığını saptayın. Örneğin, Azure AD Ücretsiz Azure Multi-Factor Authentication sağlayan güvenlik varsayılanlarını sağladığından, telefon araması veya SMS değil yalnızca mobil kimlik doğrulayıcı uygulaması kimlik doğrulama istemi için kullanılabilir. Mobil kimlik doğrulama uygulamasının bir kullanıcının kişisel cihazında yüklü olduğundan emin değilseniz bu yaklaşım bir sınırlama olabilir.

| Özellik | Azure AD Ücretsiz-güvenlik Varsayılanları | Azure AD Ücretsiz-Azure AD Genel yöneticileri | Office 365 Business Premium, E3 veya E5 | Azure AD Premium P1 veya P2 |
| --- |:---:|:---:|:---:|:---:|
| MFA ile Azure AD kiracı yönetici hesaplarını koruma | ● | ● (Yalnızca*Azure AD Genel yönetici* hesapları) | ● | ● |
| İkinci bir faktör olarak mobil uygulama | ● | ● | ● | ● |
| İkinci bir faktör olarak telefon araması | | ● | ● | ● |
| İkinci bir faktör olarak SMS | | ● | ● | ● |
| Doğrulama yöntemleri üzerinde yönetici denetimi | | ● | ● | ● |
| Sahtekarlık uyarısı | | | | ● |
| MFA Raporları | | | | ● |
| Telefon aramaları için özel karşılama | | | | ● |
| Telefon aramaları için özel arayan KIMLIĞI | | | | ● |
| Güvenilen IP'ler | | | | ● |
| Güvenilen cihazlar için MFA hatırlama | | ● | ● | ● |
| Şirket içi uygulamalar için MFA | | | | ● |

> [!IMPORTANT]
> Mart 2019 itibariyle telefon araması seçenekleri artık Azure Multi-Factor Authentication ve Azure self servis parola sıfırlama kullanıcıları Azure AD Ücretsiz/deneme kiracılarında kullanılamıyor. SMS iletileri bu değişiklikten etkilenmez. Telefon görüşmeleri Azure AD Premium P1 veya P2 kiracılarında veya Office 365 Business Premium, E3 veya E5 kullanan kullanıcılar tarafından kullanılabilir olmaya devam eder.

## <a name="purchase-and-enable-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication satın alıp etkinleştirin

Azure Multi-Factor Authentication kullanmak için uygun bir Azure AD katmanını kaydettirin veya satın alın. Azure AD, ücretsiz, Office 365 Apps Edition (Office 365 Business Premium E3 veya E5 müşterileri için), Premium P1 ve Premium P2 gibi dört sürümde sunulur.

Ücretsiz sürüm, bir Azure aboneliğine dahildir. Güvenlik varsayılanlarını kullanma veya hesapları *Azure AD Genel yönetici* rolüyle koruma hakkında bilgi için [aşağıdaki bölüme](#azure-ad-free-tier) bakın.

Azure AD Premium sürümleri, Microsoft temsilciniz, [Açık toplu lisanslama programı](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)ve [bulut çözüm sağlayıcıları programı](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409)aracılığıyla kullanılabilir. Azure ve Office 365 aboneleri Ayrıca Azure Active Directory Premium P1 ve P2 çevrimiçi olarak satın alabilir. Satın almak için [oturum açın](https://portal.office.com/Commerce/Catalog.aspx) .

> [!IMPORTANT]
> Tüketim tabanlı lisanslama, 1 Eylül 2018 ' de geçerli olan yeni müşteriler için artık kullanılamaz. Tüketim tabanlı modeli kullanan mevcut müşteriler, etkin kullanıcı başına veya kimlik doğrulaması başına faturalandırma kullanmaya devam edebilir.

Gerekli Azure AD katmanını satın aldıktan sonra [azure Multi-Factor Authentication planlayın ve dağıtın](howto-mfa-getstarted.md).

### <a name="azure-ad-free-tier"></a>Azure AD Ücretsiz katmanı

Bir Azure AD Ücretsiz kiracısındaki tüm kullanıcılar, güvenlik varsayılanlarını kullanarak Azure Multi-Factor Authentication kullanabilir. Bu güvenlik Varsayılanları, her oturum açtıklarında tüm kullanıcılar için Azure Multi-Factor Authentication 'ı etkinleştirir. Mobil kimlik doğrulama uygulaması, Azure AD Ücretsiz güvenlik Varsayılanları kullanılırken Azure Multi-Factor Authentication kullanılabilecek tek yöntemdir.

* [Azure AD güvenlik Varsayılanları hakkında daha fazla bilgi edinin](../fundamentals/concept-fundamentals-security-defaults.md)
* [Azure AD Ücretsiz kullanıcılar için güvenlik varsayılanlarını etkinleştir](../fundamentals/concept-fundamentals-security-defaults.md#enabling-security-defaults)

Azure Multi-Factor Authentication tüm kullanıcılar ve tüm oturum açma olayları için etkinleştirmek istemiyorsanız, bunun yerine yalnızca *Azure AD Genel yönetici* rolüyle Kullanıcı hesaplarını korumayı seçebilirsiniz. Bu yaklaşım, kritik yönetici hesapları için ek kimlik doğrulama istemleri sağlar. Azure Multi-Factor Authentication 'yi, kullandığınız hesap türüne bağlı olarak aşağıdaki yollarla etkinleştirin:

* Bir Microsoft hesabı kullanıyorsanız, [Multi-Factor Authentication için kaydolun](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).
* Microsoft hesabı kullanmıyorsanız, [Azure AD 'de bir kullanıcı veya grup için Multi-Factor Authentication 'ı açın](howto-mfa-userstates.md).

## <a name="next-steps"></a>Sonraki adımlar

* Maliyetler hakkında daha fazla bilgi için bkz. [Azure Multi-Factor Authentication fiyatlandırması](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).
* [Koşullu erişim nedir?](../conditional-access/overview.md)

