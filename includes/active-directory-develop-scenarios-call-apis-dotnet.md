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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76308899"
---
### <a name="authenticationresult-properties-in-msalnet"></a>Kimlik DoğrulamaMSAL.NETdaki Sonuç özellikleri

Belirteçleri dönmek `AuthenticationResult`elde etmek için yöntemler. Async yöntemleri `Task<AuthenticationResult>` için, döndürür.

MSAL.NET olarak, `AuthenticationResult` ortaya çıkarır:

- `AccessToken`web API'sinin kaynaklara erişmesini sağlar. Bu parametre bir dize, genellikle bir Base-64 kodlanmış JWT. İstemci asla erişim jetonunun içine bakmamalıdır. Biçimin kararlı kalacağı garanti edilemez ve kaynak için şifrelenebilir. İstemci deki belirteç içeriğine erişime bağlı kod yazmak, hataların ve istemci mantığının kırılmalarının en büyük kaynaklarından biridir. Daha fazla bilgi için [Erişim belirteçleri'ne](../articles/active-directory/develop/access-tokens.md)bakın.
- `IdToken`kullanıcı için. Bu parametre kodlanmış bir JWT'dir. Daha fazla bilgi için [kimlik belirteçlerine](../articles/active-directory/develop/id-tokens.md)bakın.
- `ExpiresOn`belirteci süresinin dolduğu tarih ve saati bildirir.
- `TenantId`kullanıcının bulunduğu kiracıyı içerir. Azure Etkin Dizin (Azure AD) B2B senaryolarında konuk kullanıcılar için kiracı kimliği, benzersiz kiracı değil, konuk kiracıdır.
Belirteç bir kullanıcı için `AuthenticationResult` teslim edildiğinde, bu kullanıcı hakkında da bilgi içerir. Kullanıcı olmadan başvuru nun istendiği gizli istemci akışları için bu kullanıcı bilgileri geçersizdir.
- `Scopes` Belirteci verildiği.
- Kullanıcı için benzersiz kimlik.

### <a name="iaccount"></a>IAccount

MSAL.NET `IAccount` arabirim üzerinden hesap kavramını tanımlar. Bu kırılma değişikliği doğru semantik sağlar. Aynı kullanıcının farklı Azure REKLAM dizinlerinde birden fazla hesabı olabilir. Ayrıca, ev hesabı bilgileri sağlandığı için MSAL.NET konuk senaryolarında daha iyi bilgi sağlar.
Aşağıdaki diyagram `IAccount` arabirimin yapısını gösterir.

![IAccount arayüz yapısı](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

Sınıf, `AccountId` aşağıdaki tabloda gösterilen özellikleri ile belirli bir kiracı bir hesap tanımlar.

| Özellik | Açıklama |
|----------|-------------|
| `TenantId` | Hesabın bulunduğu kiracının kimliği olan GUID için dize gösterimi. |
| `ObjectId` | Kiracıdaki hesabın sahibi olan kullanıcının kimliği olan GUID için dize gösterimi. |
| `Identifier` | Hesap için benzersiz tanımlayıcı. `Identifier`bir virgülle `ObjectId` ayrılması `TenantId` ve ayrılmasıdır. 64. üs kodlanmış değiller. |

Arabirim, `IAccount` tek bir hesap la ilgili bilgileri temsil eder. Aynı kullanıcı farklı kiracılarda bulunabilir, bu da kullanıcının birden çok hesabı olabileceği anlamına gelir. Üyeleri aşağıdaki tabloda gösterilmiştir.

| Özellik | Açıklama |
|----------|-------------|
| `Username` | Örneğin UserPrincipalName (UPN) biçiminde görüntülenebilir değeri içeren bir john.doe@contoso.comdize. Bu dize, HomeAccountId ve HomeAccountId.Identifier'in aksine null olabilir. Bu özellik, `DisplayableId` MSAL.NET `IUser` önceki sürümlerinde özelliği değiştirir. |
| `Environment` | Örneğin, bu hesabın kimlik sağlayıcısını içeren `login.microsoftonline.com`bir dize. Bu `IdentityProvider` `IUser`özellik, bulut ortamına `IdentityProvider` ek olarak kiracı hakkında da bilgi sahibi olması dışında, mülkün yerini alır. Burada, değer yalnızca ana bilgisayardır. |
| `HomeAccountId` | Kullanıcının ev hesabının hesap kimliği. Bu özellik, Azure AD kiracılarında kullanıcıyı benzersiz olarak tanımlar. |

### <a name="use-the-token-to-call-a-protected-api"></a>Korumalı API çağırmak için belirteci kullanın

MSAL tarafından iade `result`edildikten sonra, `AuthenticationResult` korumalı web API'sine erişmek için arama yapmadan önce http yetkilendirme üstbilgisine ekleyin.

```csharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```