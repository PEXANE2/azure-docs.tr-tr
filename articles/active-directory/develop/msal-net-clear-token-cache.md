---
title: Belirteç önbelleğini temizle (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak belirteç önbelleğini temizleme hakkında bilgi edinin.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11ff07e551002b8a0242afe51f3868a2f4a0ee85
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75424254"
---
# <a name="clear-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç önbelleğini temizleme

.NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak [bir erişim belirteci](msal-acquire-cache-tokens.md) aldığınızda, belirteç önbelleğe alınır. Uygulamanın bir belirtece ihtiyacı olduğunda, kabul edilebilir bir belirtecin önbellekte olup olmadığını doğrulamak için öncelikle `AcquireTokenSilent` yöntemini çağırmalıdır. 

Hesapları önbellekten kaldırarak önbelleğin temizlenmesi sağlanır. Bu, tarayıcıda bulunan oturum tanımlama bilgisini kaldırmaz, ancak.  Aşağıdaki örnek bir ortak istemci uygulaması örnekleyen, uygulamanın hesaplarını alır ve hesapları kaldırır.

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

Belirteçleri edinme ve önbelleğe alma hakkında daha fazla bilgi edinmek için, [erişim belirteci alma](msal-acquire-cache-tokens.md)makalesini okuyun.
