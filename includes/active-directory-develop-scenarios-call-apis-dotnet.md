---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 4e01dbb0036761215a9a05c464b20ead340a2e3d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423749"
---
### <a name="authenticationresult-properties-in-msalnet"></a>MSAL.NET içinde AuthenticationResult özellikleri

Belirteçleri elde etmek için yöntemler bir `AuthenticationResult` döndürür (veya zaman uyumsuz yöntemler için bir `Task<AuthenticationResult>`.

MSAL.NET ' de `AuthenticationResult` kullanıma sunar:

- kaynaklara erişmek için Web API 'sinin `AccessToken`. Bu parametre genellikle Base64 kodlamalı JWT, ancak istemci erişim belirtecinin içinde hiçbir şekilde görünmemelidir. Biçim kararlı kalacak şekilde garanti edilmez ve kaynak için şifrelenebilir. İstemci üzerindeki erişim belirteci içeriğine bağlı olarak kod yazan kişiler, hata ve istemci mantıksal sonlarının en büyük kaynaklarından biridir. Ayrıca bkz. [erişim belirteçleri](../articles/active-directory/develop/access-tokens.md)
- Kullanıcı için `IdToken` (Bu parametre kodlanmış bir JWT). Bkz. [Kimlik belirteçleri](../articles/active-directory/develop/id-tokens.md)
- `ExpiresOn`, belirtecin süresinin dolacağı tarih/saate bildirir
- `TenantId`, kullanıcının bulunduğu kiracıyı içerir. Konuk kullanıcılar (Azure AD B2B senaryoları) için kiracı KIMLIĞI, benzersiz kiracı değil Konuk kiracıya ait değildir.
Belirteç bir kullanıcı için teslim edildiğinde `AuthenticationResult` Ayrıca bu kullanıcı hakkında bilgiler içerir. Belirteçleri Kullanıcı (uygulama için) olmadan istenen gizli istemci akışları için, bu kullanıcı bilgileri null olur.
- Belirtecin verildiği `Scopes`.
- Kullanıcının benzersiz kimliği.

### <a name="iaccount"></a>IAccount

MSAL.NET, hesap kavramını tanımlar (`IAccount` arabirimi aracılığıyla). Bu son değişiklik, doğru semantiğini sağlar: aynı kullanıcının farklı Azure AD dizinlerinde birçok hesabı olabilir. Ayrıca MSAL.NET, Konuk senaryolarında ev hesabı bilgileri sağlandığı için daha iyi bilgiler sağlar.
Aşağıdaki diyagramda `IAccount` arabiriminin yapısı gösterilmektedir:

![image](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

`AccountId` sınıfı, belirli bir Kiracıdaki bir hesabı tanımlar. Aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama |
|----------|-------------|
| `TenantId` | Bir GUID için, hesabın bulunduğu kiracının KIMLIĞI olan bir dize temsili. |
| `ObjectId` | Kiracıdaki hesaba sahip olan kullanıcının KIMLIĞI olan GUID için bir dize temsili. |
| `Identifier` | Hesap için benzersiz tanımlayıcı. `Identifier`, virgülle ayrılmış ve Base64 kodlamalı `ObjectId` ve `TenantId` bitiştirilmesi olur. |

`IAccount` arabirimi tek bir hesapla ilgili bilgileri temsil eder. Aynı kullanıcı farklı kiracılarda bulunabilir, diğer bir deyişle, bir Kullanıcı birden çok hesaba sahip olabilir. Üyeleri:

| Özellik | Açıklama |
|----------|-------------|
| `Username` | UserPrincipalName (UPN) biçimindeki görüntülenebilen değeri (örneğin, john.doe@contoso.com) içeren bir dize. Bu dize null olabilir, ancak Homeaccountıd ve Homeaccountıd. Identifier null olmaz. Bu özellik, MSAL.NET 'in önceki sürümlerindeki `IUser` `DisplayableId` özelliğini değiştirir. |
| `Environment` | Bu hesabın kimlik sağlayıcısını içeren bir dize; Örneğin, `login.microsoftonline.com`. Bu özellik, `IdentityProvider` de kiracı hakkında bilgi de (bulut ortamına ek olarak), burada değer yalnızca ana bilgisayar olduğu sürece, `IUser``IdentityProvider` özelliğini değiştirir. |
| `HomeAccountId` | Kullanıcı için ana hesabın AccountID. Bu özellik, kullanıcıyı Azure AD kiracılar genelinde benzersiz şekilde tanımlar. |

### <a name="using-the-token-to-call-a-protected-api"></a>Korunan API 'yi çağırmak için belirteci kullanma

`AuthenticationResult`, MSAL tarafından döndürüldüğünde (`result`), korumalı Web API 'sine erişmek için çağrıyı yapmadan önce HTTP yetkilendirme üst bilgisine eklemeniz gerekir.

```csharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```