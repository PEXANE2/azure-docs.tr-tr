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
ms.date: 10/23/2020
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ROBOTS: NOINDEX
ms.openlocfilehash: 5dec4958c3a1d955d8f8c1dce90e27696fbebfe5
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98063357"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Özel olarak geliştirilmiş bir uygulama için belirteç ömür varsayılanlarını değiştirme

Bu makalede, bir erişim belirteci ömür ilkesi ayarlamak için Azure AD PowerShell 'in nasıl kullanılacağı gösterilmektedir. Azure AD Premium, uygulama geliştiricilerinin ve kiracı yöneticilerinin, gizli olmayan istemciler için verilen belirteçlerin ömrünü yapılandırmasına izin verir. Belirteç ömür ilkeleri, kiracı genelinde veya erişilmekte olan kaynaklara ayarlanır.

> [!IMPORTANT]
> 2020 Mayıs 'tan sonra, kiracılar artık yenileme ve oturum belirteci yaşam sürelerini yapılandıramayacak.  Azure Active Directory, 30 Ocak 2021 ' den sonra ilkelerde bulunan mevcut yenileme ve oturum belirteci yapılandırmasını durdurur. Kullanımdan kaldırıldıktan sonra erişim belirteci yaşam sürelerini yapılandırabilirsiniz. Daha fazla bilgi için [Azure AD 'de yapılandırılabilir belirteç yaşam sürelerini](./active-directory-configurable-token-lifetimes.md)okuyun.
> Azure AD koşullu erişim 'de [kimlik doğrulama oturumu yönetimi özellikleri](../conditional-access/howto-conditional-access-session-lifetime.md)uyguladık   . Bu yeni özelliği, oturum açma sıklığını ayarlayarak yenileme belirteci yaşam sürelerini yapılandırmak için kullanabilirsiniz.  

Erişim belirteci ömür ilkesi ayarlamak için [Azure AD PowerShell modülünü](https://www.powershellgallery.com/packages/AzureADPreview)indirmeniz gerekir.
**Connect-AzureAD-Onayla** komutunu çalıştırın.

Aşağıda, kullanıcıların Web uygulamanızda daha sık kimlik doğrulaması yapmasını gerektiren örnek bir ilke verilmiştir. Bu ilke, Web uygulamanızın hizmet sorumlusuna erişimin ömrünü belirler. İlkeyi oluşturun ve hizmet sorumlusuna atayın. Ayrıca hizmet sorumlunun ObjectID 'sini almanız gerekir.

```powershell
$policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"

$sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"

Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
```

## <a name="next-steps"></a>Sonraki adımlar

* Kuruluşunuzdaki tüm uygulamalar için, çok kiracılı bir uygulama veya kuruluşunuzdaki belirli bir hizmet sorumlusu için belirteç yaşam sürelerini ayarlama da dahil olmak üzere, Azure AD tarafından verilen belirteç yaşam sürelerinin nasıl yapılandırılacağını öğrenmek için bkz. [Azure AD 'de yapılandırılabilir belirteç yaşam süreleri](./active-directory-configurable-token-lifetimes.md) . 
* [Azure AD belirteç başvurusu](./id-tokens.md)
