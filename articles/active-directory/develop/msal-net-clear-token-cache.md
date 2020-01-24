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
ms.openlocfilehash: 51407c4c157fa171bc5fd18e8db1f97f677ed973
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76695594"
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
