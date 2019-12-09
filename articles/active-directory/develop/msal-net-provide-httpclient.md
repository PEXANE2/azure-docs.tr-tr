---
title: HttpClient ve proxy sağlama (MSAL.NET)
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak Azure AD 'ye bağlanmak üzere kendi HttpClient ve proxy 'nizi sağlama hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c7b9a8d90c505cb93b6cf237ed67e9c47d9be0bb
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74915586"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>MSAL.NET kullanarak kendi HttpClient ve proxy 'nizi sağlama
[Ortak bir istemci uygulamasını başlatırken](msal-net-initializing-client-applications.md), kendi httpistemcinizi sağlamak için `.WithHttpClientFactory method` kullanabilirsiniz.  Kendi HttpClient 'ın sağlanması, bir HTTP proxy 'sinin ayrıntılı denetimi, Kullanıcı Aracısı üstbilgilerini özelleştirme veya MSAL belirli bir HttpClient (örneğin, ASP.NET Core Web Apps/API 'Leri) kullanmasına zorlama gibi gelişmiş senaryolar sağlar.

## <a name="initialize-with-httpclientfactory"></a>HttpClientFactory ile Başlat
Aşağıdaki örnek, bir `HttpClientFactory` oluşturup bir ortak istemci uygulamasını bu uygulamayla başlatmayı gösterir:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient ve Xamarin iOS
Xamarin iOS kullanırken, iOS 7 ve daha yeni bir sürümü için `NSURLSession`tabanlı işleyiciyi açıkça kullanan bir `HttpClient` oluşturmanız önerilir. MSAL.NET, iOS 7 ve üzeri için `NSURLSessionHandler` kullanan bir `HttpClient` otomatik olarak oluşturur. Daha fazla bilgi için, [HttpClient Için Xamarin iOS belgelerini](/xamarin/cross-platform/macios/http-stack)okuyun.