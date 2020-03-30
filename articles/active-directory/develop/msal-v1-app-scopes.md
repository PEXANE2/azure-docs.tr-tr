---
title: v1.0 uygulamaları (MSAL) için kapsamlar | Azure
description: Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL) kullanarak bir v1.0 uygulamasının kapsamları hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/25/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: d5b2ef57af112169fb39e0da7a60b095698ff504
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299839"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>V1.0 belirteçlerini kabul eden bir Web API'si için kapsamlar

OAuth2 izinleri, geliştiriciler için bir Azure Active Directory (Azure AD) (Azure AD) web API (kaynak) uygulamasının istemci uygulamalarına maruz kalarak yaptığı izin kapsamlarıdır. Bu izin kapsamları, onay sırasında istemci uygulamalarına verilebilir. [Azure Active Directory uygulama bildirimi başvurusunda](reference-app-manifest.md#manifest-reference)ki bölüme `oauth2Permissions` bakın.

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>V1.0 uygulamasının belirli OAuth2 izinlerine erişim talep etmek için kapsamlar

Bir v1.0 uygulamasının belirli kapsamları için belirteçler elde etmek için https://graph.microsoft.com)(örneğin, bu kaynak için istenen bir OAuth2 izni ile istenen bir kaynak tanımlayıcısını daraltın kapsamlar oluşturmak gibi Microsoft Graph API.

Örneğin, kullanıcı adına uygulama kimliği URI olduğu bir v1.0 web `ResourceId`API erişmek için:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Microsoft Graph API'yi (https:\//graph.microsoft.com/) kullanarak Azure AD MSAL.NET ile okumak ve yazmak için aşağıdaki örneklerde gösterildiği gibi kapsamların bir listesini oluşturmanız gerekir:

```csharp
string ResourceId = "https://graph.microsoft.com/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.microsoft.com/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Azure Kaynak Yöneticisi API'sine (https:\//management.core.windows.net/) karşılık gelen kapsamı yazmak için aşağıdaki kapsamı istemeniz gerekir (iki eğik çizgiye dikkat edin):

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> Azure Kaynak Yöneticisi API hedef kitle talebinde bir kesinti beklediğinden (aud) iki eğik çizgi kullanmanız gerekir ve ardından API adını kapsamdan ayırmak için bir kesik vardır.

Azure AD tarafından kullanılan mantık aşağıdaki gibidir:

- V1.0 erişim belirteci (mümkün olan tek), aud=kaynak içeren ADAL (Azure AD v1.0) bitiş noktası için
- MSAL (Microsoft kimlik platformu (v2.0)) uç noktası için v2.0 belirteçlerini kabul eden bir kaynak için erişim belirteci soran,`aud=resource.AppId`
- MSAL (v2.0 bitiş noktası) v1.0 erişim belirteci kabul eden bir kaynak için erişim belirteci soran (yukarıdaki durumda), Azure AD, son eğik çizgiden önce her şeyi alarak ve kaynak tanımlayıcısı olarak kullanarak istenen hedef kitleyi istenen kapsamdan ayrıştırır. Bu nedenle,\/https: /database.windows.net "https:\//database.windows.net/" kitlesi bekliyorsa, "https:\//database.windows.net//.default" kapsamını istemeniz gerekir. Ayrıca bkz. GitHub sorunu [#747: Kaynak url'nin sondaki eğik çizgi leri atlanır ve bu da sql auth hatasına neden olur.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747)

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>V1.0 uygulamasının tüm izinlerine erişim isteğinde bulunmak için kapsamlar

Bir v1.0 uygulamasının tüm statik kapsamları için bir belirteç elde etmek istiyorsanız, API'nin uygulama kimliği URI'sine ".default" ekle:

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-a-client-credential-flowdaemon-app"></a>İstemci kimlik bilgisi akışı/daemon uygulaması için talep edilen kapsamlar

İstemci kimlik bilgisi akışı söz konusu olduğunda, geçecek `/.default`kapsam da . Bu, Azure AD'ye şunları söyler: "Yöneticinin uygulama kaydında onay verdiği tüm uygulama düzeyindeizinler.
