---
title: Özel Azure AD uygulamaları için belirteç ömrü varsayılanlarını değiştirme
description: Azure AD 'de geliştirmekte olduğunuz uygulamanız için belirteç ömür ilkelerini güncelleştirme
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ms.openlocfilehash: fc71ebe61fba8c1bdb6b7625b16a50d8995a581a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80883362"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için belirteç ömür varsayılanlarını değiştirme

Bu makalede bir belirteç ömür ilkesi ayarlamak için Azure AD PowerShell 'in nasıl kullanılacağı gösterilmektedir. Azure AD Premium, uygulama geliştiricilerinin ve kiracı yöneticilerinin, gizli olmayan istemciler için verilen belirteçlerin ömrünü yapılandırmasına izin verir. Belirteç ömür ilkeleri, kiracı genelinde veya erişilmekte olan kaynaklara ayarlanır.

1. Belirteç ömür ilkesi ayarlamak için [Azure AD PowerShell modülünü](https://www.powershellgallery.com/packages/AzureADPreview)indirmeniz gerekir.
1. **Connect-AzureAD-Onayla** komutunu çalıştırın.

    En fazla Age tek etmenli yenileme belirtecini ayarlayan bir ilke aşağıda verilmiştir. İlkeyi oluşturun:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Kuruluşunuzdaki tüm uygulamalar için, çok kiracılı bir uygulama veya kuruluşunuzdaki belirli bir hizmet sorumlusu için belirteç yaşam sürelerini ayarlama da dahil olmak üzere, Azure AD tarafından verilen belirteç yaşam sürelerinin nasıl yapılandırılacağını öğrenmek için bkz. [Azure AD 'de yapılandırılabilir belirteç yaşam süreleri](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) . 
* [Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)
