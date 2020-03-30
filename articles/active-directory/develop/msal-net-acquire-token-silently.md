---
title: Önbellekten bir belirteç edinme (MSAL.NET)
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak sessizce (belirteç önbelleğinden) bir erişim belirteci nasıl elde edeceğinizi öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 90189a1d7fd6421b7a24940e8c6ed615fa0df6d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084842"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç önbelleğinden bir belirteç alma

.NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak bir erişim belirteci aldığınızda, belirteç önbelleğe alınmış olur. Uygulamanın bir belirteci gerektiğinde, önce `AcquireTokenSilent` yöntemin önbellekte kabul edilebilir bir belirteç olup olmadığını doğrulamak için araması gerekir. Çoğu durumda, önbellekteki bir belirteç temel alınarak daha fazla kapsama alanı olan başka bir belirteç elde etmek mümkündür. Bir belirteci son kullanma tarihine yaklaşırken yenilemek de mümkündür (belirteç önbelleği de yenileme belirteci içerdiğinden).

Önerilen desen önce `AcquireTokenSilent` yöntemi aramaktır.  Başarısız `AcquireTokenSilent` olursa, diğer yöntemleri kullanarak bir belirteç edinin.

Aşağıdaki örnekte, uygulama ilk olarak belirteç önbelleğinden bir belirteç elde etmeye çalışır.  Bir `MsalUiRequiredException` özel durum atılırsa, uygulama etkileşimli bir şekilde bir belirteç kazanır. 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilent.
 // This indicates you need to call AcquireTokenInteractive to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```
