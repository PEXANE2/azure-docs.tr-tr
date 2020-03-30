---
title: Belirteç önbelleğini temizle (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak belirteç önbelleğini nasıl temizlerken öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: a10efb5ff0a2c6a3ced3631dfe82c86e3e8a72fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084771"
---
# <a name="clear-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç önbelleğini temizleme

.NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak [bir erişim belirteci aldığınızda,](msal-acquire-cache-tokens.md) belirteç önbelleğe alınmış olur. Uygulamanın bir belirteci gerektiğinde, önce `AcquireTokenSilent` yöntemin önbellekte kabul edilebilir bir belirteç olup olmadığını doğrulamak için araması gerekir. 

Önbelleği temizlemek, hesapları önbellekten kaldırarak elde edilir. Bu olsa da, tarayıcıda oturum çerez kaldırmaz.  Aşağıdaki örnek, ortak istemci uygulamasını anında alır, uygulamanın hesaplarını alır ve hesapları kaldırır.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Belirteçleri edinme ve önbelleğe alma hakkında daha fazla bilgi edinmek için, [erişim jetonu edinin'i](msal-acquire-cache-tokens.md)okuyun.
