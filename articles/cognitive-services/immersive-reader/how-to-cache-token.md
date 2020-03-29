---
title: Kimlik doğrulaması belirtecini önbelleğe alma
titleSuffix: Azure Cognitive Services
description: Bu makalede, kimlik doğrulama belirteci önbelleğe nasıl gösterecektir.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: e652aa29b1c1935fcc4887dbe13ef9b683a8bd05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946171"
---
# <a name="how-to-cache-the-authentication-token"></a>Kimlik doğrulama belirteci önbelleğe nasıl

Bu makalede, uygulamanızın performansını artırmak için kimlik doğrulama belirteci önbelleğe nasıl gösterin.

## <a name="using-aspnet"></a>ASP.NET kullanma

Bir belirteç elde etmek için kullanılan **Microsoft.IdentityModel.Clients.ActiveDirectory** NuGet paketini içe aktarın. Ardından, Sürükleyici Okuyucu kaynağını `AuthenticationResult` [oluşturduğunuzda](./how-to-create-immersive-reader.md)var olan kimlik doğrulama değerlerini kullanarak aşağıdaki kodu kullanın.

```csharp
private async Task<AuthenticationResult> GetTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext($"https://login.windows.net/{TENANT_ID}");
    ClientCredential clientCredential = new ClientCredential(CLIENT_ID, CLIENT_SECRET);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync("https://cognitiveservices.azure.com/", clientCredential);
    return authResult;
}
```

Nesne, `AuthenticationResult` SDK'yı kullanarak Sürükleyici Okuyucuyu başlatırken kullanacağınız gerçek belirteç olan bir `AccessToken` özelliğe sahiptir. Ayrıca belirteç süresi nin ne zaman dolacağını gösteren bir `ExpiresOn` özelliğe sahiptir. Sürükleyici Okuyucu'yu başlatmadan önce, belirteci süresinin dolup dolmadığını denetleyebilir ve yalnızca süresi dolmuşsa yeni bir belirteç edinebilirsiniz.

## <a name="using-nodejs"></a>Düğüm.JS kullanma

[**İstek**](https://www.npmjs.com/package/request) npm paketini projenize ekleyin. Sürükleyici Okuyucu kaynağını oluşturduğunuzda sahip olduğunuz kimlik doğrulama değerlerini kullanarak bir belirteç elde etmek için aşağıdaki kodu [kullanın.](./how-to-create-immersive-reader.md)

```javascript
router.get('/token', function(req, res) {
    request.post(
        {
            headers: { 'content-type': 'application/x-www-form-urlencoded' },
            url: `https://login.windows.net/${TENANT_ID}/oauth2/token`,
            form: {
                grant_type: 'client_credentials',
                client_id: CLIENT_ID,
                client_secret: CLIENT_SECRET,
                resource: 'https://cognitiveservices.azure.com/'
            }
        },
        function(err, resp, json) {
            const result = JSON.parse(json);
            return res.send({
                access_token: result.access_token,
                expires_on: result.expires_on
            });
        }
    );
});
```

Özellik, `expires_on` belirteç süresinin dolduğu tarih ve saattir ve 1 Ocak 1970 UTC'den bu yana saniye sayısı olarak ifade edilir. Belirteçlerinizin yeni bir tane edinmeye çalışmadan önce süresinin dolup dolmadığını belirlemek için bu değeri kullanın.

```javascript
async function getToken() {
    if (Date.now() / 1000 > CREDENTIALS.expires_on) {
        CREDENTIALS = await refreshCredentials();
    }
    return CREDENTIALS.access_token;
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Sürükleyici [Okuyucu SDK Referans](./reference.md) keşfedin