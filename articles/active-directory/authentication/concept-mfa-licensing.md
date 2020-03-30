---
title: Azure Çok Faktörlü Kimlik Doğrulama sürümleri ve tüketim planları
description: Azure Çok Faktörlü Kimlik Doğrulama istemcisi ve kullanılabilen farklı yöntem ve sürümler hakkında bilgi edinin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: e74a7ab0c003aaf9d90211484b39f8322cd9c329
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77648011"
---
# <a name="features-and-licenses-for-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama özellikleri ve lisansları

Kuruluşunuzdaki kullanıcı hesaplarını korumak için çok faktörlü kimlik doğrulama kullanılmalıdır. Bu özellik, özellikle kaynaklara ayrıcalıklı erişimi olan hesaplar için önemlidir. Temel çok faktörlü kimlik doğrulama özellikleri, Office 365 ve Azure Etkin Dizin (Azure AD) yöneticilerinin ek ücret ödemeden kullanılabilir. Yöneticilerinizin özelliklerini yükseltmek veya çok faktörlü kimlik doğrulamayı diğer kullanıcılarınıza genişletmek istiyorsanız, Azure Çok Faktörlü Kimlik Doğrulaması'nı çeşitli yollarla satın alabilirsiniz.

> [!IMPORTANT]
> Bu makalede, Azure Çok Faktörlü Kimlik Doğrulama'nın lisanslanıp kullanılabilme şekilleri ayrıntılı olarak anlatılabilir. Fiyatlandırma ve faturalandırma hakkında belirli ayrıntılar için [Azure Çok Faktörlü Kimlik Doğrulama fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)bakın.

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama'nın kullanılabilir sürümleri

Azure Çok Faktörlü Kimlik Doğrulama, kuruluşunuzun gereksinimlerine bağlı olarak birkaç farklı şekilde kullanılabilir ve lisanslanabilir. Şu anda sahip olduğunuz Azure AD, Office 365, EMS veya Microsoft 365 lisansına bağlı olarak Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanma hakkına sahip olabilirsiniz. Aşağıdaki tablo, Azure Çok Faktörlü Kimlik Doğrulaması'nı almanın farklı yollarını ve her biri için bazı özellikleri ve kullanım servis taleplerini ayrıntılarıyla açıklanmaktadır.

| Kullanıcıysanız | Yetenekler ve kullanım örnekleri |
| --- | --- |
| EMS veya Microsoft 365 E3 ve E5 | EMS E3 veya Microsoft 365 E3 (EMS ve Office 365'i içerir), Azure AD Premium P1 içerir. EMS E5 veya Microsoft 365 E5, Azure AD Premium P2 içerir. Kullanıcılara çok faktörlü kimlik doğrulaması sağlamak için aşağıdaki bölümlerde belirtilen koşullu Erişim özelliklerini kullanabilirsiniz. |
| Azure AD Premium P1 | [Azure AD Koşullu Erişimi,](../conditional-access/overview.md) belirli senaryolar veya etkinlikler sırasında iş gereksinimlerinize uyacak şekilde kullanıcılardan çok faktörlü kimlik doğrulaması için istekte bulunabilir. |
| Azure AD Premium P2 | En güçlü güvenlik konumunu ve geliştirilmiş kullanıcı deneyimini sağlar. Azure AD Premium P1 özelliklerine, kullanıcının desenlerine uyum sağlayan ve çok faktörlü kimlik doğrulama istemlerini en aza indiren [risk tabanlı Koşullu Erişim](../conditional-access/howto-conditional-access-policy-risk.md) ekler. |
| Office 365 Business Premium, E3 veya E5 | Azure Çok Faktörlü Kimlik Doğrulama, tüm oturum açma etkinlikleri için tüm kullanıcılar için etkin veya devre dışı bırakılır. Yalnızca bir kullanıcı alt kümesi için veya yalnızca belirli senaryolar altında çok faktörlü kimlik doğrulamayı etkinleştirme olanağı yoktur. Yönetim, Office 365 portalı üzerinden yapılır. Geliştirilmiş bir kullanıcı deneyimi için Azure AD Premium P1 veya P2'ye yükseltin ve Koşullu Erişim'i kullanın. Daha fazla bilgi [için, çok faktörlü kimlik doğrulamaiçeren güvenli Office 365 kaynaklarına](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6)bakın. |
| Azure AD ücretsiz | Kimlik doğrulama isteği her yapıldığında, tüm kullanıcılar için çok faktörlü kimlik doğrulaması etkinleştirmek için [güvenlik varsayılanlarını](../fundamentals/concept-fundamentals-security-defaults.md) kullanabilirsiniz. Etkin kullanıcılar veya senaryolar üzerinde parçalı denetiminiz yoktur, ancak bu ek güvenlik adımı sağlar.<br /> Güvenlik varsayılanları herkes için çok faktörlü kimlik doğrulaması sağlamak için kullanılmasa bile, *Azure AD Genel Yöneticisi* rolüne atanan kullanıcılar çok faktörlü kimlik doğrulaması kullanacak şekilde yapılandırılabilir. Serbest katmanın bu özelliği, kritik yönetici hesaplarının çok faktörlü kimlik doğrulamayla korunmasını sağlar. |

## <a name="feature-comparison-of-versions"></a>Sürümlerin özellik karşılaştırması

Aşağıdaki tablo, Azure Çok Faktörlü Kimlik Doğrulama'nın çeşitli sürümlerinde kullanılabilen özelliklerin bir listesini sağlar. Kullanıcı kimlik doğrulamasını güvence altına almak için ihtiyaçlarınızı planlayın ve ardından bu gereksinimleri hangi yaklaşımın karşıladığını belirleyin. Örneğin, Azure AD Free Azure Çok Faktörlü Kimlik Doğrulaması sağlayan güvenlik varsayılanları sağlasa da, telefon görüşmesi veya SMS için değil, kimlik doğrulama istemi için yalnızca mobil kimlik doğrulayıcı uygulaması kullanılabilir. Mobil kimlik doğrulama uygulamasının kullanıcının kişisel cihazına yüklendiğinden emin olamıyorsanız, bu yaklaşım bir sınırlama olabilir.

| Özellik | Azure AD Free - Güvenlik varsayılanları | Azure AD Ücretsiz - Azure AD Global Yöneticileri | Office 365 Business Premium, E3 veya E5 | Azure AD Premium P1 veya P2 |
| --- |:---:|:---:|:---:|:---:|
| MFA ile Azure AD kiracı yönetici hesaplarını koruma | ● | ● (*Yalnızca Azure AD Global Administrator* hesapları) | ● | ● |
| İkinci bir faktör olarak mobil uygulama | ● | ● | ● | ● |
| İkinci bir faktör olarak telefon görüşmesi | | ● | ● | ● |
| İkinci bir faktör olarak SMS | | ● | ● | ● |
| Doğrulama yöntemleri üzerinde yönetici kontrolü | | ● | ● | ● |
| Sahtekarlık uyarısı | | | | ● |
| MFA Raporları | | | | ● |
| Telefon aramaları için özel karşılama | | | | ● |
| Telefon görüşmeleri için özel arayan kimliği | | | | ● |
| Güvenilen IP'ler | | | | ● |
| Güvenilen cihazlar için MFA hatırlama | | ● | ● | ● |
| Şirket içi uygulamalar için MFA | | | | ● |

> [!IMPORTANT]
> Mart 2019 itibarıyla, Telefon görüşmesi seçenekleri artık Azure Çok Faktörlü Kimlik Doğrulama ve Azure Self Servis Parola Sıfırlama kullanıcıları için Azure AD Ücretsiz / deneme kiracılarında kullanılamaz. SMS mesajları bu değişiklikle etkilenmez. Telefon görüşmeleri, Azure AD Premium P1 veya P2 kiracıları veya kullanımları veya Office 365 Business Premium, E3 veya E5'teki kullanıcılar tarafından kullanılabilir olmaya devam ediyor.

## <a name="purchase-and-enable-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama'yı satın alın ve etkinleştirin

Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmak için uygun bir Azure AD katmanına kaydolun veya satın alın. Azure AD, Ücretsiz, Office 365 uygulamaları sürümü (Office 365 Business Premium E3 veya E5 müşterileri için), Premium P1 ve Premium P2 olmak üzere dört sürümle gelir.

Ücretsiz sürüm, Azure aboneliğine dahildir. *Azure AD Genel Yöneticisi* rolüyle güvenlik varsayılanlarını nasıl kullanacağınız veya hesapları nasıl koruyacağınız hakkında bilgi almak için aşağıdaki [bölüme](#azure-ad-free-tier) bakın.

Azure AD Premium sürümleri Microsoft temsilciniz, [Açık Toplu Lisans Programı](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)ve [Bulut Çözüm Sağlayıcıları programı](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409)aracılığıyla kullanılabilir. Azure ve Office 365 aboneleri Azure Active Directory Premium P1 ve P2'yi çevrimiçi olarak da satın alabilir. Satın almak için [oturum açın.](https://portal.office.com/Commerce/Catalog.aspx)

> [!IMPORTANT]
> Tüketime dayalı lisanslama, 1 Eylül 2018 tarihinden itibaren yeni müşteriler tarafından kullanılamıyor. Tüketim tabanlı modeli kullanan varolan müşteriler, etkin kullanıcı başına veya kimlik doğrulama faturalandırması başına kullanmaya devam edebilir.

Gerekli Azure AD katmanını satın aldıktan sonra [Azure Çok Faktörlü Kimlik Doğrulaması'nı planlayın ve dağıtın.](howto-mfa-getstarted.md)

### <a name="azure-ad-free-tier"></a>Azure AD Ücretsiz katman

Azure AD Free kiracısındaki tüm kullanıcılar, güvenlik varsayılanlarını kullanarak Azure Çok Faktörlü kimlik doğrulamasını kullanabilir. Bu güvenlik varsayılanları, her oturum açtıklarında tüm kullanıcılar için Azure Çok Faktörlü kimlik doğrulaması sağlar. Mobil kimlik doğrulama uygulaması, Azure AD Free güvenlik varsayılanlarını kullanırken Azure Çok Faktörlü Kimlik Doğrulaması için kullanılabilecek tek yöntemdir.

* [Azure AD güvenlik varsayılanları hakkında daha fazla bilgi edinin](../fundamentals/concept-fundamentals-security-defaults.md)
* [Azure AD Free'deki kullanıcılar için güvenlik varsayılanlarını etkinleştirme](../fundamentals/concept-fundamentals-security-defaults.md#enabling-security-defaults)

Tüm kullanıcılar ve her oturum açma olayı için Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek istemiyorsanız, bunun yerine yalnızca *Azure AD Global Administrator* rolüyle kullanıcı hesaplarını korumayı seçebilirsiniz. Bu yaklaşım, kritik yönetici hesapları için ek kimlik doğrulama istemleri sağlar. Kullandığınız hesap türüne bağlı olarak Azure Çok Faktörlü Kimlik Doğrulaması'nı aşağıdaki yollardan biriyle etkinleştirin:

* Bir Microsoft Hesabı kullanıyorsanız, [çok faktörlü kimlik doğrulama için kaydolun.](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)
* Microsoft Hesabı kullanmıyorsanız, Azure [AD'deki bir kullanıcı veya grup için çok faktörlü kimlik doğrulamasını açın.](howto-mfa-userstates.md)

## <a name="next-steps"></a>Sonraki adımlar

Maliyetler hakkında daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama fiyatlandırması'na](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)bakın.
