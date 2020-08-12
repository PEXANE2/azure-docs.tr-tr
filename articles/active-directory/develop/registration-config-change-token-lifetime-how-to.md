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
ms.openlocfilehash: 8de6a7aafdd402e4ee75862e69ac60af3af0e041
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88114940"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için belirteç ömür varsayılanlarını değiştirme

Bu makalede bir belirteç ömür ilkesi ayarlamak için Azure AD PowerShell 'in nasıl kullanılacağı gösterilmektedir. Azure AD Premium, uygulama geliştiricilerinin ve kiracı yöneticilerinin, gizli olmayan istemciler için verilen belirteçlerin ömrünü yapılandırmasına izin verir. Belirteç ömür ilkeleri, kiracı genelinde veya erişilmekte olan kaynaklara ayarlanır.

1. Belirteç ömür ilkesi ayarlamak için [Azure AD PowerShell modülünü](https://www.powershellgallery.com/packages/AzureADPreview)indirmeniz gerekir.
1. **Connect-AzureAD-Onayla** komutunu çalıştırın.

    En fazla Age tek etmenli yenileme belirtecini ayarlayan bir ilke aşağıda verilmiştir. İlkeyi oluşturun:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Kuruluşunuzdaki tüm uygulamalar için, çok kiracılı bir uygulama veya kuruluşunuzdaki belirli bir hizmet sorumlusu için belirteç yaşam sürelerini ayarlama da dahil olmak üzere, Azure AD tarafından verilen belirteç yaşam sürelerinin nasıl yapılandırılacağını öğrenmek için bkz. [Azure AD 'de yapılandırılabilir belirteç yaşam süreleri](./active-directory-configurable-token-lifetimes.md) . 
* [Azure AD belirteç başvurusu](./id-tokens.md)