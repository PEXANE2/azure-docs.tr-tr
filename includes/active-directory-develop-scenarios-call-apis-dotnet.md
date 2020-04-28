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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76308899"
---
### <a name="authenticationresult-properties-in-msalnet"></a>MSAL.NET içinde AuthenticationResult özellikleri

Belirteçleri alma yöntemleri döndürülür `AuthenticationResult`. Zaman uyumsuz metotlar için `Task<AuthenticationResult>` , döndürür.

MSAL.NET ' `AuthenticationResult` de şunları gösterir:

- `AccessToken`Web API 'sinin kaynaklara erişmesi için. Bu parametre genellikle bir Base-64 kodlu JWT dizesidir. İstemci hiçbir şekilde erişim belirtecinin içinde görünmemelidir. Biçim kararlı olmaya garanti edilmez ve kaynak için şifrelenebilir. İstemci üzerindeki erişim belirteci içeriğine bağlı olan kod yazmak, hata ve istemci mantıksal sonlarının en büyük kaynaklarından biridir. Daha fazla bilgi için bkz. [erişim belirteçleri](../articles/active-directory/develop/access-tokens.md).
- `IdToken`Kullanıcı için. Bu parametre kodlanmış bir JWT. Daha fazla bilgi için bkz. [Kimlik belirteçleri](../articles/active-directory/develop/id-tokens.md).
- `ExpiresOn`belirtecin süresinin dolacağı tarih ve saate bildirir.
- `TenantId`kullanıcının bulunduğu kiracıyı içerir. Azure Active Directory (Azure AD) B2B senaryolarında Konuk kullanıcılar için kiracı KIMLIĞI, benzersiz kiracı değil Konuk kiracıya ait değildir.
Belirteç bir kullanıcı için teslim edildiğinde, `AuthenticationResult` bu kullanıcı hakkındaki bilgileri de içerir. Uygulama için hiçbir Kullanıcı olmadan belirteçlerin istendiği gizli istemci akışları için bu kullanıcı bilgileri null olur.
- `Scopes` Belirtecinin verildiği.
- Kullanıcının benzersiz KIMLIĞI.

### <a name="iaccount"></a>IAccount

MSAL.NET, `IAccount` arabirim aracılığıyla bir hesap kavramını tanımlar. Bu son değişiklik, doğru semantiğini sağlar. Aynı kullanıcının farklı Azure AD dizinlerinde birçok hesabı olabilir. Ayrıca, MSAL.NET, ana hesap bilgileri sağlandığı için konuk senaryolarında daha fazla bilgi sağlar.
Aşağıdaki diyagramda `IAccount` arabirimin yapısı gösterilmektedir.

![IAccount arabirim yapısı](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

`AccountId` Sınıfı, aşağıdaki tabloda gösterilen özelliklerle belirli bir Kiracıdaki bir hesabı tanımlar.

| Özellik | Açıklama |
|----------|-------------|
| `TenantId` | Bir GUID için, hesabın bulunduğu kiracının KIMLIĞI olan bir dize temsili. |
| `ObjectId` | Kiracıdaki hesaba sahip olan kullanıcının KIMLIĞI olan GUID için bir dize temsili. |
| `Identifier` | Hesap için benzersiz tanımlayıcı. `Identifier`, öğesinin `ObjectId` bitiştirilmesi ve `TenantId` virgülle ayrılması. Bunlar temel 64 kodlandı değildir. |

Arabirim `IAccount` , tek bir hesapla ilgili bilgileri temsil eder. Aynı kullanıcı farklı kiracılarda bulunabilir. Bu, bir kullanıcının birden fazla hesabı olabileceği anlamına gelir. Üyeleri aşağıdaki tabloda gösterilmiştir.

| Özellik | Açıklama |
|----------|-------------|
| `Username` | Örneğin, john.doe@contoso.comUSERPRINCIPALNAME (UPN) biçimindeki görüntülenebilen değeri içeren bir dize. Bu dize, Homeaccountıd ve Homeaccountıd. Identifier 'ın aksine null değer olmayacak şekilde null olabilir. Bu özellik, `DisplayableId` önceki msal.net sürümlerindeki `IUser` öğesinin özelliğini değiştirir. |
| `Environment` | Bu hesabın kimlik sağlayıcısını içeren bir dize (örneğin, `login.microsoftonline.com`). Bu özellik, bulut `IdentityProvider` ortamının yanı `IUser`sıra kiracı hakkında `IdentityProvider` da bilgi de olan öğesinin özelliğini değiştirir. Burada, değer yalnızca ana bilgisayar olur. |
| `HomeAccountId` | Kullanıcının giriş hesabının hesap KIMLIĞI. Bu özellik, kullanıcıyı Azure AD kiracılar genelinde benzersiz şekilde tanımlar. |

### <a name="use-the-token-to-call-a-protected-api"></a>Korunan API 'yi çağırmak için belirteci kullanma

`AuthenticationResult` İçinde `result`msal tarafından DÖNDÜRÜLDÜĞÜNDE, korumalı Web API 'sine erişmek IÇIN çağrıyı yapmadan önce http yetkilendirme üstbilgisine ekleyin.

```csharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```