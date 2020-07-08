---
title: Önbellekten belirteç alma (MSAL.NET)
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak bir erişim belirtecini sessizce (belirteç önbelleğinden) nasıl edinebileceğinizi öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: c4e60e7e6a16b3e526d2f1581bfa145b74e5da01
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477507"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç önbelleğinden belirteç alma

.NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak bir erişim belirteci aldığınızda, belirteç önbelleğe alınır. Uygulamanın bir belirtece ihtiyacı olduğunda, öncelikle `AcquireTokenSilent` kabul edilebilir bir belirtecin önbellekte olup olmadığını doğrulamak için yöntemini çağırmalıdır. Çoğu durumda, önbellekteki bir belirtece göre daha fazla kapsam içeren başka bir belirteç elde etmek mümkündür. Ayrıca, kullanım süresi dolmak üzere (belirteç önbelleği de bir yenileme belirteci içerdiğinden) bir belirteci yenilemek mümkündür.

Önerilen model `AcquireTokenSilent` öncelikle yöntemi çağırmalıdır.  `AcquireTokenSilent`Başarısız olursa, diğer yöntemleri kullanarak bir belirteç alın.

Aşağıdaki örnekte, uygulama ilk olarak belirteç önbelleğinden bir belirteç edinmeye çalışır.  Bir `MsalUiRequiredException` özel durum oluşturulursa, uygulama bir belirteci etkileşimli olarak alır. 

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
