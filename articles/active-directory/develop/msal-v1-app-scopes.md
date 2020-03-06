---
title: V 1.0 uygulamaları için kapsamlar (MSAL) | Mavisi
description: Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanarak bir v 1.0 uygulaması için kapsamlar hakkında bilgi edinin.
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
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78299839"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>V 1.0 belirteçlerini kabul eden bir Web API 'SI için kapsamlar

OAuth2 izinleri, geliştiriciler (v 1.0) için bir Azure Active Directory (Azure AD) Web API (kaynak) uygulamasının istemci uygulamalarına sunduğu izin kapsamlardır. Bu izin kapsamları, izin sırasında istemci uygulamalarına verilebilir. [Azure Active Directory uygulama bildirimi başvurusunda](reference-app-manifest.md#manifest-reference)`oauth2Permissions` hakkında bölümüne bakın.

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Bir v 1.0 uygulamasının belirli OAuth2 izinlerine erişim istemek için kapsamlar

Bir v 1.0 uygulamasının belirli kapsamlarına yönelik belirteçleri almak için (örneğin, https://graph.microsoft.com)Microsoft Graph API 'SI, istenen kaynak tanımlayıcısını ilgili kaynak için istenen bir OAuth2 izniyle birleştirerek kapsamlar oluşturun.

Örneğin, Kullanıcı adına, uygulama KIMLIĞI URI 'sinin `ResourceId`bir v 1.0 Web API 'sine erişmek için:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Microsoft Graph API (https:\//graph.microsoft.com/) kullanarak MSAL.NET Azure AD ile okumak ve yazmak için aşağıdaki örneklerde gösterildiği gibi kapsamların bir listesini oluşturmanız gerekir:

```csharp
string ResourceId = "https://graph.microsoft.com/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.microsoft.com/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Azure Resource Manager API 'sine (https:\//management.core.windows.net/) karşılık gelen kapsamı yazmak için aşağıdaki kapsamı istemeniz gerekir (iki eğik çizgi).

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> Azure Resource Manager API 'SI, hedef kitle talebinde (AUD) bir eğik çizgi beklediği için iki eğik çizgi kullanmanız gerekir ve ardından API adını kapsamdan ayırmak için bir eğik çizgi vardır.

Azure AD tarafından kullanılan mantık şunlardır:

- Bir v 1.0 erişim belirtecine sahip ADAL (Azure AD v 1.0) uç noktası (tek olası), AUD = kaynağı için
- MSAL (Microsoft Identity platform (v 2.0)) uç noktası için, v 2.0 belirteçlerini kabul eden bir kaynak için erişim belirteci soran bir uç nokta, `aud=resource.AppId`
- MSAL (v 2.0 uç noktası) için, bir v 1.0 erişim belirtecini kabul eden bir kaynak için erişim belirteci isteyen (Yukarıdaki durum), Azure AD, son eğik çizgiden önce her şeyi alıp kaynak tanımlayıcısı olarak kullanarak istenen kitleyi istenen kapsamdan ayrıştırır. Bu nedenle, https:\//database.windows.net "https:\//database.windows.net/" kitlesini beklediğinde, "https:\//database.windows.net//.default" kapsamını istemeniz gerekir. Ayrıca bkz. GitHub sorunu [#747: kaynak URL 'sinin sondaki eğik çizgi, SQL kimlik doğrulama hatasına neden oldu](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747).

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>Bir v 1.0 uygulamasının tüm izinlerine erişim istemek için kapsamlar

Bir v 1.0 uygulamasının tüm statik kapsamları için bir belirteç almak istiyorsanız, API 'nin uygulama KIMLIĞI URI 'sine ". default" ekleyin:

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-a-client-credential-flowdaemon-app"></a>İstemci kimlik bilgileri akışı/Daemon uygulaması için istek yapılacak kapsamlar

İstemci kimlik bilgileri akışı durumunda geçirilecek kapsam de `/.default`olacaktır. Bu, Azure AD 'ye ("yöneticinin uygulamanın kaydında sahip olduğu tüm uygulama düzeyindeki izinleri) söyler.
