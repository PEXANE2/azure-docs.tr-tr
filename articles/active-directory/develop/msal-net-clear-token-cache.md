---
title: .NET-Azure için Microsoft kimlik doğrulama kitaplığı 'nı kullanarak belirteç önbelleğini temizleme
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak belirteç önbelleğini temizleme hakkında bilgi edinin.
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
ms.date: 05/07/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1cee6443db0b019f79a80cf5b7c0e2a7a50240f2
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532670"
---
# <a name="clear-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç önbelleğini temizleme

.NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak [bir erişim belirteci](msal-acquire-cache-tokens.md) aldığınızda, belirteç önbelleğe alınır. Uygulamanın bir belirtece ihtiyacı olduğunda, öncelikle kabul edilebilir bir belirtecin önbellekte `AcquireTokenSilent` olup olmadığını doğrulamak için yöntemini çağırmalıdır. 

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
