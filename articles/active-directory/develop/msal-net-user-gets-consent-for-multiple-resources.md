---
title: Birkaç kaynak için onay al (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: Bir kullanıcının .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak birkaç kaynak için ön onay alma hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: e36237e67b4498ca6aad4b7ffa8c645abeff6143
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477303"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Kullanıcı MSAL.NET kullanarak birkaç kaynak için onay alır
Microsoft Identity platform uç noktası aynı anda birkaç kaynak için bir belirteç almanıza izin vermez. .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) kullanılırken, belirteç alma yöntemindeki kapsamlar parametresi yalnızca tek bir kaynak için kapsam içermelidir. Ancak, Oluşturucu yöntemini kullanarak ek kapsamlar belirterek, birden fazla kaynağa ön onay verebilirsiniz `.WithExtraScopeToConsent` .

> [!NOTE]
> Birkaç kaynağa onay alınması Microsoft Identity platform için geçerlidir, ancak Azure AD B2C için değildir. Azure AD B2C, Kullanıcı onayını değil yalnızca yönetici onayını destekler.

Örneğin, her biri 2 kapsam içeren iki kaynağınız varsa:

- https: \/ /mytenant.onmicrosoft.com/customerapi (2 kapsamlarla `customer.read` ve `customer.write` )
- https: \/ /mytenant.onmicrosoft.com/vendorapi (2 kapsamlarla `vendor.read` ve `vendor.write` )

`.WithExtraScopeToConsent`Aşağıdaki örnekte gösterildiği gibi, *Extrascopestoonay* parametresine sahip olan değiştiriciyi kullanmanız gerekir:

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
