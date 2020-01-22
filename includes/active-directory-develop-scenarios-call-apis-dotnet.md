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
ms.openlocfilehash: 3d4e45d1bf53bab4d1f9c45367f9d051f1668e2b
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76308899"
---
### <a name="authenticationresult-properties-in-msalnet"></a>MSAL.NET içinde AuthenticationResult özellikleri

Belirteçleri alma yöntemleri `AuthenticationResult`döndürür. Zaman uyumsuz yöntemler için `Task<AuthenticationResult>` döndürür.

MSAL.NET ' de `AuthenticationResult` kullanıma sunar:

- kaynaklara erişmek için Web API 'sinin `AccessToken`. Bu parametre genellikle bir Base-64 kodlu JWT dizesidir. İstemci hiçbir şekilde erişim belirtecinin içinde görünmemelidir. Biçim kararlı olmaya garanti edilmez ve kaynak için şifrelenebilir. İstemci üzerindeki erişim belirteci içeriğine bağlı olan kod yazmak, hata ve istemci mantıksal sonlarının en büyük kaynaklarından biridir. Daha fazla bilgi için bkz. [erişim belirteçleri](../articles/active-directory/develop/access-tokens.md).
- Kullanıcı için `IdToken`. Bu parametre kodlanmış bir JWT. Daha fazla bilgi için bkz. [Kimlik belirteçleri](../articles/active-directory/develop/id-tokens.md).
- `ExpiresOn`, belirtecin süresinin dolacağı tarihi ve saati söyler.
- `TenantId`, kullanıcının bulunduğu kiracıyı içerir. Azure Active Directory (Azure AD) B2B senaryolarında Konuk kullanıcılar için kiracı KIMLIĞI, benzersiz kiracı değil Konuk kiracıya ait değildir.
Belirteç bir kullanıcı için teslim edildiğinde `AuthenticationResult` Ayrıca bu kullanıcı hakkında bilgiler içerir. Uygulama için hiçbir Kullanıcı olmadan belirteçlerin istendiği gizli istemci akışları için bu kullanıcı bilgileri null olur.
- Belirtecin verildiği `Scopes`.
- Kullanıcının benzersiz KIMLIĞI.

### <a name="iaccount"></a>IAccount

MSAL.NET, `IAccount` arabirimi aracılığıyla bir hesabın kavramını tanımlar. Bu son değişiklik, doğru semantiğini sağlar. Aynı kullanıcının farklı Azure AD dizinlerinde birçok hesabı olabilir. Ayrıca, MSAL.NET, ana hesap bilgileri sağlandığı için konuk senaryolarında daha fazla bilgi sağlar.
Aşağıdaki diyagramda `IAccount` arabiriminin yapısı gösterilmektedir.

![IAccount arabirim yapısı](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

`AccountId` sınıfı, aşağıdaki tabloda gösterilen özelliklerle belirli bir Kiracıdaki bir hesabı tanımlar.

| Özellik | Açıklama |
|----------|-------------|
| `TenantId` | Bir GUID için, hesabın bulunduğu kiracının KIMLIĞI olan bir dize temsili. |
| `ObjectId` | Kiracıdaki hesaba sahip olan kullanıcının KIMLIĞI olan GUID için bir dize temsili. |
| `Identifier` | Hesap için benzersiz tanımlayıcı. `Identifier`, virgülle ayrılmış `ObjectId` ve `TenantId` bitiştirilmesi olur. Bunlar temel 64 kodlandı değildir. |

`IAccount` arabirimi tek bir hesapla ilgili bilgileri temsil eder. Aynı kullanıcı farklı kiracılarda bulunabilir. Bu, bir kullanıcının birden fazla hesabı olabileceği anlamına gelir. Üyeleri aşağıdaki tabloda gösterilmiştir.

| Özellik | Açıklama |
|----------|-------------|
| `Username` | UserPrincipalName (UPN) biçimindeki görüntülenebilen değeri (örneğin, john.doe@contoso.com) içeren bir dize. Bu dize, Homeaccountıd ve Homeaccountıd. Identifier 'ın aksine null değer olmayacak şekilde null olabilir. Bu özellik, MSAL.NET 'in önceki sürümlerindeki `IUser` `DisplayableId` özelliğini değiştirir. |
| `Environment` | Bu hesabın kimlik sağlayıcısını içeren bir dize; Örneğin, `login.microsoftonline.com`. Bu özellik, bulut ortamının yanı sıra `IdentityProvider` de kiracı hakkında bilgi da olması dışında, `IUser``IdentityProvider` özelliğini değiştirir. Burada, değer yalnızca ana bilgisayar olur. |
| `HomeAccountId` | Kullanıcının giriş hesabının hesap KIMLIĞI. Bu özellik, kullanıcıyı Azure AD kiracılar genelinde benzersiz şekilde tanımlar. |

### <a name="use-the-token-to-call-a-protected-api"></a>Korunan API 'yi çağırmak için belirteci kullanma

`result`tarafından `AuthenticationResult` döndürülmeden sonra, korumalı Web API 'sine erişmek için çağrıyı yapmadan önce HTTP yetkilendirme üst bilgisine ekleyin.

```csharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```