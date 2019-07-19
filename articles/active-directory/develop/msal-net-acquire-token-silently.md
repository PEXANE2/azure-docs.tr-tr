---
title: Sessizce belirteç alma (.NET için Microsoft kimlik doğrulama kitaplığı) | Mavisi
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak bir erişim belirtecini sessizce (belirteç önbelleğinden) nasıl edinebileceğinizi öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 309e912f2adf5249770b40a631ed62f7cb3113e5
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277954"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>MSAL.NET kullanarak belirteç önbelleğinden belirteç alma

.NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak bir erişim belirteci aldığınızda, belirteç önbelleğe alınır. Uygulamanın bir belirtece ihtiyacı olduğunda, öncelikle kabul edilebilir bir belirtecin önbellekte `AcquireTokenSilent` olup olmadığını doğrulamak için yöntemini çağırmalıdır. Çoğu durumda, önbellekteki bir belirtece göre daha fazla kapsam içeren başka bir belirteç elde etmek mümkündür. Ayrıca, kullanım süresi dolmak üzere (belirteç önbelleği de bir yenileme belirteci içerdiğinden) bir belirteci yenilemek mümkündür.

Önerilen model öncelikle `AcquireTokenSilent` yöntemi çağırmalıdır.  `AcquireTokenSilent` Başarısız olursa, diğer yöntemleri kullanarak bir belirteç alın.

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
