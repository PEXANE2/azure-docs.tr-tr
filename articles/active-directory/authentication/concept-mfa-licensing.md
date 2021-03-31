---
title: Azure AD Multi-Factor Authentication sürümleri ve tüketim planları
description: Azure AD Multi-Factor Authentication istemcisi ve kullanılabilir farklı yöntemler ve sürümler hakkında bilgi edinin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/15/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63d880146a0b068a5d097c452c14b28db4907098
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96743964"
---
# <a name="features-and-licenses-for-azure-ad-multi-factor-authentication"></a>Azure AD Multi-Factor Authentication yönelik özellikler ve lisanslar

Kuruluşunuzdaki Kullanıcı hesaplarını korumak için Multi-Factor Authentication kullanılmalıdır. Bu özellik özellikle kaynaklara ayrıcalıklı erişimi olan hesaplar için önemlidir. Temel Multi-Factor Authentication özellikleri, Microsoft 365 ve Azure Active Directory (Azure AD) yöneticileri için ek maliyet olmadan kullanılabilir. Yöneticileriniz için özellikleri yükseltmek veya Multi-Factor Authentication 'ı kullanıcılarınızın geri kalanına genişletmek istiyorsanız, Azure AD Multi-Factor Authentication çeşitli yollarla satın alabilirsiniz.

> [!IMPORTANT]
> Bu makalede, Azure AD Multi-Factor Authentication 'nin lisanslanması ve kullanılması için kullanabileceğiniz farklı yollar ayrıntılı olarak açıklanır. Fiyatlandırma ve faturalandırma hakkında belirli Ayrıntılar için bkz. [Azure AD Multi-Factor Authentication fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-ad-multi-factor-authentication"></a>Azure AD Multi-Factor Authentication 'nin kullanılabilir sürümleri

Azure AD Multi-Factor Authentication, kuruluşunuzun ihtiyaçlarına bağlı olarak birkaç farklı şekilde kullanılabilir ve lisanslanabilir. Zaten sahip olduğunuz Azure AD, EMS veya Microsoft 365 lisansına bağlı olarak Azure AD Multi-Factor Authentication kullanma hakkına sahip olabilirsiniz. Aşağıdaki tabloda Azure AD Multi-Factor Authentication almanın farklı yolları ve her biri için bazı özellikler ve kullanım örnekleri ayrıntılı olarak verilmiştir.

| Bir kullanıcısı | Yetenekler ve kullanım örnekleri |
| --- | --- |
| Microsoft 365 İş Premium ve EMS veya Microsoft 365 E3 ve E5 | EMS E3, Microsoft 365 E3 ve Microsoft 365 İş Premium Azure AD Premium P1 içerir. EMS E5 veya Microsoft 365 E5, Azure AD Premium P2 içerir. Kullanıcılara Multi-Factor Authentication sağlamak için aşağıdaki bölümlerde belirtilen koşullu erişim özelliklerinin aynısını kullanabilirsiniz. |
| Azure AD Premium P1 | Belirli senaryolar veya olaylar sırasında iş gereksinimlerinize uyacak şekilde kullanıcılardan çok faktörlü kimlik doğrulaması için kullanıcı istemek üzere [Azure AD koşullu erişim](../conditional-access/howto-conditional-access-policy-all-users-mfa.md) kullanabilirsiniz. |
| Azure AD Premium P2 | En güçlü güvenlik konumunu ve geliştirilmiş Kullanıcı deneyimini sağlar. Kullanıcının desenlerine uyum sağlayan Azure AD Premium P1 özelliklerine [risk tabanlı koşullu erişim](../conditional-access/howto-conditional-access-policy-risk.md) ekler ve Multi-Factor Authentication istemlerini en aza indirir. |
| Tüm Microsoft 365 planları | Azure AD Multi-Factor Authentication, [Kullanıcı başına temelinde etkinleştirilebilir](howto-mfa-userstates.md)veya [güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md)kullanılarak tüm kullanıcılar için etkinleştirilebilir veya devre dışı bırakılabilir. Azure AD Multi-Factor Authentication Yönetimi Microsoft 365 Portalı üzerinden yapılır. Gelişmiş bir kullanıcı deneyimi için Azure AD Premium P1 veya P2 sürümüne yükseltin ve koşullu erişimi kullanın. Daha fazla bilgi için bkz. [Multi-Factor Authentication ile güvenli Microsoft 365 kaynakları](/microsoft-365/admin/security-and-compliance/set-up-multi-factor-authentication). |
| Azure AD ücretsiz | Tüm kullanıcılar için Multi-Factor Authentication 'ı etkinleştirmek için [güvenlik varsayılanlarını](../fundamentals/concept-fundamentals-security-defaults.md) kullanabilirsiniz. Etkin kullanıcılar veya senaryolar üzerinde ayrıntılı bir denetiminiz yoktur, ancak bu ek güvenlik adımını sağlar.<br /> Herkes için çok faktörlü kimlik doğrulamasını etkinleştirmek üzere güvenlik Varsayılanları kullanılmasa bile, *Azure AD Genel yönetici* rolü atanan kullanıcılar Multi-Factor Authentication kullanacak şekilde yapılandırılabilir. Ücretsiz katmanın bu özelliği, kritik yönetici hesaplarının Multi-Factor Authentication tarafından korunduğundan emin olmanızı sağlar. |

## <a name="feature-comparison-of-versions"></a>Sürümlerin Özellik Karşılaştırması

Aşağıdaki tabloda, çeşitli Azure AD Multi-Factor Authentication sürümlerinde kullanılabilen özelliklerin bir listesi verilmiştir. Kullanıcı kimlik doğrulamasının güvenliğini sağlamak için gereksinimlerinizi planlayın, ardından hangi yaklaşımın bu gereksinimleri karşıladığını saptayın. Örneğin, Azure AD Multi-Factor Authentication sağlayan güvenlik varsayılanlarını Azure AD Ücretsiz, ancak telefon araması veya SMS değil, kimlik doğrulama istemi için yalnızca mobil kimlik doğrulayıcı uygulaması kullanılabilir. Mobil kimlik doğrulama uygulamasının bir kullanıcının kişisel cihazında yüklü olduğundan emin değilseniz bu yaklaşım bir sınırlama olabilir.

| Özellik | Azure AD Ücretsiz-güvenlik Varsayılanları | Azure AD Ücretsiz-Azure AD Genel yöneticileri | Microsoft 365 uygulamaları | Azure AD Premium P1 veya P2 |
| --- |:---:|:---:|:---:|:---:|
| MFA ile Azure AD kiracı yönetici hesaplarını koruma | ● | ● (Yalnızca *Azure AD Genel yönetici* hesapları) | ● | ● |
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

## <a name="purchase-and-enable-azure-ad-multi-factor-authentication"></a>Azure AD Multi-Factor Authentication satın alıp etkinleştirin

Azure AD Multi-Factor Authentication kullanmak için uygun bir Azure AD katmanını kaydettirin veya satın alın. Azure AD, ücretsiz, Microsoft 365 uygulamalar, Premium P1 ve Premium P2 dört sürümde sunulur.

Ücretsiz sürüm, bir Azure aboneliğine dahildir. Güvenlik varsayılanlarını kullanma veya hesapları *Azure AD Genel yönetici* rolüyle koruma hakkında bilgi için [aşağıdaki bölüme](#azure-ad-free-tier) bakın.

Azure AD Premium sürümleri, Microsoft temsilciniz, [Açık toplu lisanslama programı](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)ve [bulut çözüm sağlayıcıları programı](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409)aracılığıyla kullanılabilir. Azure ve Microsoft 365 aboneleri Azure Active Directory Premium P1 ve P2 çevrimiçi olarak da satın alabilir. Satın almak için [oturum açın](https://portal.office.com/Commerce/Catalog.aspx) .

Gerekli Azure AD katmanını satın aldıktan sonra [Azure ad Multi-Factor Authentication planlayın ve dağıtın](howto-mfa-getstarted.md).

### <a name="azure-ad-free-tier"></a>Azure AD Ücretsiz katmanı

Bir Azure AD Ücretsiz kiracısındaki tüm kullanıcılar, güvenlik varsayılanlarını kullanarak Azure AD Multi-Factor Authentication kullanabilir. Mobil kimlik doğrulama uygulaması, Azure AD Ücretsiz güvenlik Varsayılanları kullanılırken Azure AD Multi-Factor Authentication kullanılabilecek tek yöntemdir.

* [Azure AD güvenlik Varsayılanları hakkında daha fazla bilgi edinin](../fundamentals/concept-fundamentals-security-defaults.md)
* [Azure AD Ücretsiz kullanıcılar için güvenlik varsayılanlarını etkinleştir](../fundamentals/concept-fundamentals-security-defaults.md#enabling-security-defaults)

Azure AD Multi-Factor Authentication tüm kullanıcılar için etkinleştirmek istemiyorsanız, bunun yerine yalnızca *Azure AD Genel yönetici* rolüyle Kullanıcı hesaplarını korumayı seçebilirsiniz. Bu yaklaşım, kritik yönetici hesapları için ek kimlik doğrulama istemleri sağlar. Azure AD Multi-Factor Authentication, kullandığınız hesap türüne bağlı olarak aşağıdaki yollarla etkinleştirilir:

* Bir Microsoft hesabı kullanıyorsanız, [Multi-Factor Authentication için kaydolun](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).
* Microsoft hesabı kullanmıyorsanız, [Azure AD 'de bir kullanıcı veya grup için Multi-Factor Authentication 'ı açın](howto-mfa-userstates.md).

## <a name="next-steps"></a>Sonraki adımlar

* Maliyetler hakkında daha fazla bilgi için bkz. [Azure AD Multi-Factor Authentication fiyatlandırması](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).
* [Koşullu erişim nedir?](../conditional-access/overview.md)

