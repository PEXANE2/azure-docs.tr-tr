---
title: Birkaç kaynağa onay al (.NET için Microsoft kimlik doğrulama kitaplığı) | Mavisi
description: Bir kullanıcının .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak birkaç kaynak için ön onay alma hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ded7a6fc465b4cfc98d26f65195f89de8381ac6
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532355"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Kullanıcı MSAL.NET kullanarak birkaç kaynak için onay alır
Microsoft Identity platform uç noktası aynı anda birkaç kaynak için bir belirteç almanıza izin vermez. .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) kullanılırken, belirteç alma yöntemindeki kapsamlar parametresi yalnızca tek bir kaynak için kapsam içermelidir. Ancak, `.WithExtraScopeToConsent` Oluşturucu yöntemini kullanarak ek kapsamlar belirterek, birden fazla kaynağa ön onay verebilirsiniz.

> [!NOTE]
> Birkaç kaynağa onay alınması Microsoft Identity platform için geçerlidir, ancak Azure AD B2C için değildir. Azure AD B2C, Kullanıcı onayını değil yalnızca yönetici onayını destekler.

Örneğin, her biri 2 kapsam içeren iki kaynağınız varsa:

- https:\//mytenant.onmicrosoft.com/customerapi (2 kapsamlarla `customer.read` ve `customer.write`)
- https:\//mytenant.onmicrosoft.com/vendorapi (2 kapsamlarla `vendor.read` ve `vendor.write`)

Aşağıdaki örnekte gösterildiği gibi `.WithExtraScopeToConsent` , *extrascopestoonay* parametresine sahip olan değiştiriciyi kullanmanız gerekir:

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Bu, ilk Web API 'SI için bir erişim belirteci alır. Daha sonra, ikinci Web API 'sine erişmeniz gerektiğinde belirteci belirteç önbelleğinden sessizce edinebilirsiniz:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
