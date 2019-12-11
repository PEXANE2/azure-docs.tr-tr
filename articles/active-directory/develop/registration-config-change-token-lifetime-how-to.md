---
title: Özel Azure AD uygulamaları için belirteç ömrü varsayılanlarını değiştirin | Microsoft Docs
description: Azure AD 'de geliştirmekte olduğunuz uygulamanız için belirteç ömür ilkelerini güncelleştirme
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: a603be6d57581541c0868b9f48a0bf9997cadd71
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74962840"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için belirteç ömür varsayılanlarını değiştirme

Bu makalede bir belirteç ömür ilkesi ayarlamak için Azure AD PowerShell 'in nasıl kullanılacağı gösterilmektedir. Azure AD Premium, uygulama geliştiricilerinin ve kiracı yöneticilerinin, gizli olmayan istemciler için verilen belirteçlerin ömrünü yapılandırmasına izin verir. Belirteç ömür ilkeleri, kiracı genelinde veya erişilmekte olan kaynaklara ayarlanır.

1. Belirteç ömür ilkesi ayarlamak için [Azure AD PowerShell modülünü](https://www.powershellgallery.com/packages/AzureADPreview)indirmeniz gerekir.
1. **Connect-AzureAD-Onayla** komutunu çalıştırın.

    En fazla Age tek etmenli yenileme belirtecini ayarlayan bir ilke aşağıda verilmiştir. İlkeyi oluşturun: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Kuruluşunuzdaki tüm uygulamalar için, çok kiracılı bir uygulama veya kuruluşunuzdaki belirli bir hizmet sorumlusu için belirteç yaşam sürelerini ayarlama da dahil olmak üzere, Azure AD tarafından verilen belirteç yaşam sürelerinin nasıl yapılandırılacağını öğrenmek için bkz. [Azure AD 'de yapılandırılabilir belirteç yaşam süreleri](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) . 
* [Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)
