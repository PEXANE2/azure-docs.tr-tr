---
title: HttpClient & proxy sağlama (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak Azure AD 'ye bağlanmak üzere kendi HttpClient ve proxy 'nizi sağlama hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: e13c4ed472535d2c02734949bf72a5ed42108e7d
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85477337"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>MSAL.NET kullanarak kendi HttpClient ve proxy 'nizi sağlama
[Ortak bir istemci uygulamasını başlatırken](msal-net-initializing-client-applications.md), `.WithHttpClientFactory method` kendi httpistemcinizi sağlamak için kullanabilirsiniz.  Kendi HttpClient 'ın sağlanması, bir HTTP proxy 'sinin ayrıntılı denetimi, Kullanıcı Aracısı üstbilgilerini özelleştirme veya MSAL belirli bir HttpClient (örneğin, ASP.NET Core Web Apps/API 'Leri) kullanmasına zorlama gibi gelişmiş senaryolar sağlar.

## <a name="initialize-with-httpclientfactory"></a>HttpClientFactory ile Başlat
Aşağıdaki örnek, bir oluşturup `HttpClientFactory` daha sonra bir ortak istemci uygulaması başlatmak için ' i gösterir:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient ve Xamarin iOS
Xamarin iOS kullanırken, `HttpClient` `NSURLSession` iOS 7 ve daha yeni için tabanlı işleyiciyi açıkça kullanan bir oluşturmanız önerilir. MSAL.NET, `HttpClient` `NSURLSessionHandler` iOS 7 ve daha yeni bir sürümü tarafından otomatik olarak oluşturulur. Daha fazla bilgi için, [HttpClient Için Xamarin iOS belgelerini](/xamarin/cross-platform/macios/http-stack)okuyun.