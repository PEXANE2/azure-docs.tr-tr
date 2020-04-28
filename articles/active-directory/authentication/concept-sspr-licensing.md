---
title: Lisans self servis parola sıfırlama-Azure Active Directory
description: Self servis parola sıfırlama lisanslama gereksinimleri Azure Active Directory fark hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45ca11af061e37cf4f804ce2d7ceed72a9448294
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81393074"
---
# <a name="licensing-requirements-for-azure-active-directory-self-service-password-reset"></a>Azure Active Directory self servis parola sıfırlama için lisans gereksinimleri

Bir Kullanıcı cihazlarındaki veya bir uygulamada oturum açarken yardım masası çağrılarını ve üretkenlik kaybını azaltmak için, Azure Active Directory (Azure AD) içindeki kullanıcı hesapları self servis parola sıfırlama (SSPR) için etkinleştirilebilir. SSPR 'yi oluşturan Özellikler parola değiştirme, sıfırlama, kilidini açma ve bir şirket içi dizini geri yazma 'yı içerir. Temel SSPR özellikleri Office 365 ve tüm Azure AD kullanıcıları tarafından ücretsiz olarak kullanılabilir.

Bu makalede, self servis parola sıfırlamanın lisanslanması ve kullanılması için farklı yollar ayrıntılarıyla açıklanır. Fiyatlandırma ve faturalandırma hakkında belirli Ayrıntılar için bkz. [Azure AD fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Sürümleri ve özellikleri karşılaştırma

SSPR Kullanıcı başına lisanslanır. Uyumluluğun korunması için, kuruluşların kullanıcılarına uygun lisansı ataması gerekir.

Aşağıdaki tabloda parola değiştirme, sıfırlama veya şirket içi geri yazma için farklı SSPR senaryoları ve hangi SKU 'Ların özelliği sağladığı özetlenmektedir.

| Özellik | Azure AD Ücretsiz | Office 365 İş Ekstra | Microsoft 365 İş | Azure AD Premium P1 veya P2 |
| --- |:---:|:---:|:---:|:---:|
| **Yalnızca bulutta bulunan Kullanıcı parolası değiştirme**<br />Azure AD 'deki bir Kullanıcı parolasını bilir ve yeni bir şekilde değiştirmek ister. | ● | ● | ● | ● |
| **Yalnızca bulut Kullanıcı parolası sıfırlama**<br />Azure AD 'deki bir Kullanıcı parolasını unutursa ve sıfırlanması gerekir. | | ● | ● | ● |
| **Karma Kullanıcı parolası, şirket içi geri yazma ile değiştirme veya sıfırlama**<br />Azure AD 'deki bir Kullanıcı Azure AD Connect kullanan bir şirket içi dizinden eşitlendiğinde, parolasını değiştirmek veya sıfırlamak ve ayrıca yeni parolayı yeniden şirket içi olarak yazmak ister. | | | ● | ● |

> [!WARNING]
> Tek başına Office 365 lisans planları, şirket içi geri yazma ile SSPR 'yi desteklemez. Bu Office 365 lisans planları, bu işlevin çalışması için Azure AD Premium P1, Premium P2 veya Microsoft 365 İş gerektirir.

Maliyetler de dahil olmak üzere ek lisans bilgileri için aşağıdaki sayfalara bakın:

* [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory özellikleri ve özellikleri](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 İş](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Grup veya Kullanıcı tabanlı lisanslamayı etkinleştirme

Azure AD, grup tabanlı lisanslamayı destekler. Yöneticiler, lisansları tek seferde atamak yerine bir kullanıcı grubuna toplu olarak atayabilirler. Daha fazla bilgi için bkz. [lisanslarla ilgili sorunları atama, doğrulama ve çözme](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Bazı Microsoft Hizmetleri tüm konumlarda kullanılamaz. Bir lisansın bir kullanıcıya atanabilmesi için, yöneticinin kullanıcı üzerinde **Kullanım konumu** özelliğini belirtmesi gerekir. Lisansların atanması Azure Portal **Kullanıcı** > **profili** > **ayarları** bölümünde yapılabilir. *Grup lisansı atamasını kullandığınızda, kullanım konumu belirtilmemiş tüm kullanıcılar dizinin konumunu alır.*

## <a name="next-steps"></a>Sonraki adımlar

SSPR 'yi kullanmaya başlamak için aşağıdaki öğreticiyi izleyin:

> [!div class="nextstepaction"]
> [Öğretici: Self servis parola sıfırlamayı etkinleştirme (SSPR)](tutorial-enable-sspr.md)
