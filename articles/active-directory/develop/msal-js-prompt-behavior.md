---
title: Etkileşimli istek istemi davranışı (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft Kimlik Doğrulama Kitaplığını (MSAL.js) kullanarak etkileşimli aramalarda hızlı davranışı özelleştirmeyi öğrenin.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 778e89655019a49a30904fbe8d8e6aedf1833e9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76695985"
---
# <a name="prompt-behavior-in-msaljs-interactive-requests"></a>MSAL.js etkileşimli isteklerde istem davranışı

Bir kullanıcı birden çok kullanıcı hesabıyla etkin bir Azure REKLAM oturumu oluşturduğunda, Azure REKLAM oturum sayfası varsayılan olarak oturum açmadan önce kullanıcıdan bir hesap seçmesini ister. Azure AD ile yalnızca tek bir kimlik doğrulaması oturumu varsa, kullanıcılar hesap seçimi deneyimi görmez.

MSAL.js kitaplığı (v0.2.4'ten başlayarak) etkileşimli istekler`loginRedirect`sırasında `loginPopup` `acquireTokenRedirect` bir `acquireTokenPopup`istem parametresi göndermez ( , ve , ve bu nedenle herhangi bir istem davranışı zorlamaz. `acquireTokenSilent` Yöntemi kullanarak sessiz belirteç istekleri için, MSAL.js `none`bir istem parametresi kümesi geçer.

Uygulama senaryonuza bağlı olarak, yöntemlere geçirilen istek parametrelerindeki istem parametresini ayarlayarak etkileşimli isteklerin istem davranışını denetleyebilirsiniz. Örneğin, hesap seçimi deneyimini çağırmak istiyorsanız:

```javascript
var request = {
    scopes: ["user.read"],
    prompt: 'select_account',
}

userAgentApplication.loginRedirect(request);
```


Azure AD ile kimlik doğrulaması yaparken aşağıdaki istem değerleri geçirilebilir:

**giriş:** Bu değer, kullanıcıyı kimlik doğrulama isteğine kimlik bilgilerini girmeye zorlar.

**select_account:** Bu değer, kullanıcıya oturumdaki tüm hesapları listeleyen bir hesap seçimi deneyimi sağlar.

**rıza:** Bu değer, kullanıcıların uygulamaya izin vermesini sağlayan OAuth onay diyaloğunu çağırır.

**yok:** Bu değer, kullanıcının etkileşimli bir istem görmemesini sağlar. Beklenmeyen davranışlar olabileceğiiçin bu değeri MSAL.js'deki etkileşimli yöntemlere aktarmamanız önerilir. Bunun yerine, `acquireTokenSilent` sessiz aramaları elde etmek için yöntemi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

MSAL.js kitaplığında kullandığı `prompt` [OAuth 2.0 örtük hibe](v2-oauth2-implicit-grant-flow.md) protokolündeki parametre hakkında daha fazla bilgi edinin.
