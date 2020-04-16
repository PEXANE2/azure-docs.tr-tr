---
title: Self servis parola sıfırlama lisansı - Azure Active Directory
description: Azure Active Directory self servis parola sıfırlama lisans lama gereksinimleri yle arasındaki fark hakkında bilgi edinin
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
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393074"
---
# <a name="licensing-requirements-for-azure-active-directory-self-service-password-reset"></a>Azure Active Directory self servis parola sıfırlama için lisans gereksinimleri

Bir kullanıcı aygıtında veya bir uygulamada oturum açamasada yardım masası çağrılarını ve üretkenlik kaybını azaltmak için, self servis parola sıfırlama (SSPR) için Azure Active Directory (Azure AD) kullanıcı hesapları etkinleştirilebilir. SSPR'yi oluşturan özellikler arasında parola değişikliği, sıfırlama, kilidini açma ve şirket içi dizine geri yazma yer alıyor. Temel SSPR özellikleri Office 365 ve tüm Azure AD kullanıcıları tarafından ücretsiz olarak kullanılabilir.

Bu makalede, self servis parola sıfırlamanın lisanslanıp kullanılabilme sinin farklı yolları ayrıntılı olarak açıklanır. Fiyatlandırma ve faturalandırma hakkında belirli ayrıntılar için [Azure AD fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

## <a name="compare-editions-and-features"></a>Sürümleri ve özellikleri karşılaştırın

SSPR kullanıcı başına lisanslıdır. Uyumluluğu korumak için kuruluşların kullanıcılarına uygun lisansı atamaları gerekir.

Aşağıdaki tabloda, parola değişikliği, sıfırlama veya şirket içi geri yazma için farklı SSPR senaryoları özetlenmektedir ve bu özelliği SK'ler sağlar.

| Özellik | Azure AD Ücretsiz | Office 365 İş Ekstra | Microsoft 365 İş | Azure AD Premium P1 veya P2 |
| --- |:---:|:---:|:---:|:---:|
| **Yalnızca bulut kullanıcı parolası değişikliği**<br />Azure AD'deki bir kullanıcı parolasını biliyorsa ve yeni bir şeyle değiştirmek istediğinde. | ● | ● | ● | ● |
| **Yalnızca bulut kullanıcı parolası sıfırlama**<br />Azure AD'deki bir kullanıcı parolasını unuttuğunda ve sıfırlaması gerektiğinde. | | ● | ● | ● |
| **Ön önce yazma yla hibrit kullanıcı parolası değiştirme veya sıfırlama**<br />Azure AD Connect'i kullanarak şirket içi bir dizinden senkronize edilen Bir Kullanıcı parolasını değiştirmek veya sıfırlamak ve yeni parolayı ön prem'e geri yazmak istediğinde. | | | ● | ● |

> [!WARNING]
> Bağımsız Office 365 lisans lama planları, şirket içi geri yazıyla SSPR'yi desteklemez. Bu Office 365 lisanslama planları, bu işlevselliğin çalışması için Azure AD Premium P1, Premium P2 veya Microsoft 365 Business gerektirir.

Maliyetler de dahil olmak üzere ek lisans bilgileri için aşağıdaki sayfalara bakın:

* [Azure Active Directory fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory özellikleri ve özellikleri](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Kurumsal Mobilite + Güvenlik](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 İş](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Grup veya kullanıcı tabanlı lisanslamayı etkinleştirme

Azure AD, grup tabanlı lisanslamayı destekler. Yöneticiler, lisansları teker teker atamak yerine bir kullanıcı grubuna toplu olarak atayabilir. Daha fazla bilgi için [bkz.](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses)

Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Lisansın bir kullanıcıya atanabilmesi için, yöneticinin kullanıcıüzerindeki **Kullanım konumu** özelliğini belirtmesi gerekir. Lisansların atanması Azure portalındaki **Kullanıcı** > **Profili** > **Ayarları** bölümü altında yapılabilir. *Grup lisans atamasını kullandığınızda, kullanım yeri belirtilmeyen kullanıcılar dizinin konumunu devralır.*

## <a name="next-steps"></a>Sonraki adımlar

SSPR ile başlamak için aşağıdaki öğreticiyi tamamlayın:

> [!div class="nextstepaction"]
> [Öğretici: Self servis parola sıfırlamayı etkinleştirme (SSPR)](tutorial-enable-sspr.md)
