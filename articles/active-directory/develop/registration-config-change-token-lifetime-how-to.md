---
title: Özel Azure AD uygulamaları için belirteç kullanım ömrü varsayılanlarını değiştirme
description: Azure AD'de geliştirmekte olduğunuz uygulamanız için Belirteç Kullanım Ömrü ilkelerini güncelleştirme
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
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883362"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için belirteç ömür varsayılanları nasıl değiştirilir?

Bu makalede, bir belirteç yaşam ilkesi ayarlamak için Azure AD PowerShell nasıl kullanılacağını gösterir. Azure AD Premium, uygulama geliştiricilerin inve kiracı yöneticilerinin gizli olmayan istemciler için verilen belirteçlerin kullanım ömrünü yapılandırmasına olanak tanır. Belirteç yaşam alanı ilkeleri kiracı veya erişilen kaynaklar bazında ayarlanır.

1. Bir belirteç ömür ilkesi ayarlamak için [Azure AD PowerShell Modül'ü](https://www.powershellgallery.com/packages/AzureADPreview)indirmeniz gerekir.
1. **Connect-AzureAD -Onayla** komutunu çalıştırın.

    Burada, maksimum yaş tek faktörlü yenileme belirteci ayarlayan bir örnek ilke verilmiştir. İlke oluşturun:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD tarafından verilen belirteç yaşam sürelerini (kuruluşunuzdaki tüm uygulamalar, çok kiracılı bir uygulama veya kuruluşunuzdaki belirli bir hizmet müdürü için belirteç yaşam sürelerini nasıl ayarlanız) nasıl yapılandırılabildiğini öğrenmek için [Azure AD'deki yapılandırılabilir belirteç kullanım ömürlerini](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) görün. 
* [Azure AD Belirteç Başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)
