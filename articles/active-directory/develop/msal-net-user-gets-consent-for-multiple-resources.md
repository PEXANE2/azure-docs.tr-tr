---
title: Çeşitli kaynaklar için onay alın (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET için Microsoft Kimlik Doğrulama Kitaplığını kullanarak bir kullanıcının çeşitli kaynaklar için nasıl ön onay alabileceğini öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 94c9a2b6a46262ad293da9ca3ba493d6f898c870
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085846"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Kullanıcı MSAL.NET kullanarak çeşitli kaynaklar için onay alır
Microsoft kimlik platformu bitiş noktası, aynı anda birden fazla kaynak için bir belirteç elde etmenize izin vermez. .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı kullanırken, edinme belirteci yöntemindeki kapsamlar parametresi yalnızca tek bir kaynak için kapsamlar içermelidir. Ancak, `.WithExtraScopeToConsent` oluşturucu yöntemini kullanarak ek kapsamlar belirterek birkaç kaynağı peşin olarak onaylayabilirsiniz.

> [!NOTE]
> Birden çok kaynak için onay almak Microsoft kimlik platformu için çalışır, ancak Azure AD B2C için çalışmaz. Azure AD B2C, kullanıcı onayı değil, yalnızca yönetici onayInı destekler.

Örneğin, her biri 2 kapsamı olan iki kaynağınız varsa:

- https:\//mytenant.onmicrosoft.com/customerapi (2 `customer.read` kapsam `customer.write`ve )
- https:\//mytenant.onmicrosoft.com/vendorapi (2 `vendor.read` kapsam `vendor.write`ve )

Aşağıdaki örnekte `.WithExtraScopeToConsent` gösterildiği gibi *extraScopesToConsent* parametresi olan değiştirici kullanmalısınız:

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

Bu, ilk web API'si için bir erişim belirteci sağlayacaktır. Ardından, ikinci web API'sine erişmeniz gerektiğinde belirteci önbelleğinden sessizce edinebilirsiniz:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
