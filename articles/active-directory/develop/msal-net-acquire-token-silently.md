---
title: Belirteci sessizce al (MSAL.NET)
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak bir erişim belirtecini sessizce (belirteç önbelleğinden) nasıl edinebileceğinizi öğrenin.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c9c988eba278e184d0e60a4ede456a340c258e71
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74915982"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç önbelleğinden belirteç alma

.NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak bir erişim belirteci aldığınızda, belirteç önbelleğe alınır. Uygulamanın bir belirtece ihtiyacı olduğunda, kabul edilebilir bir belirtecin önbellekte olup olmadığını doğrulamak için öncelikle `AcquireTokenSilent` yöntemini çağırmalıdır. Çoğu durumda, önbellekteki bir belirtece göre daha fazla kapsam içeren başka bir belirteç elde etmek mümkündür. Ayrıca, kullanım süresi dolmak üzere (belirteç önbelleği de bir yenileme belirteci içerdiğinden) bir belirteci yenilemek mümkündür.

Önerilen model öncelikle `AcquireTokenSilent` yöntemini çağırmalıdır.  `AcquireTokenSilent` başarısız olursa, diğer yöntemleri kullanarak bir belirteç alın.

Aşağıdaki örnekte, uygulama ilk olarak belirteç önbelleğinden bir belirteç edinmeye çalışır.  `MsalUiRequiredException` bir özel durum oluşturulursa, uygulama bir belirteci etkileşimli olarak alır. 

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
