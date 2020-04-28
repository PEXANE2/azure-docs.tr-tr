---
title: HttpClient & proxy sağlama (MSAL.NET) | Mavisi
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
ms.openlocfilehash: dbf08e23b2bc1f657363f69df55763437e6c8a90
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76695066"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>MSAL.NET kullanarak kendi HttpClient ve proxy 'nizi sağlama
[Ortak bir istemci uygulamasını başlatırken](msal-net-initializing-client-applications.md), kendi httpistemcinizi sağlamak `.WithHttpClientFactory method` için kullanabilirsiniz.  Kendi HttpClient 'ın sağlanması, bir HTTP proxy 'sinin ayrıntılı denetimi, Kullanıcı Aracısı üstbilgilerini özelleştirme veya MSAL belirli bir HttpClient (örneğin, ASP.NET Core Web Apps/API 'Leri) kullanmasına zorlama gibi gelişmiş senaryolar sağlar.

## <a name="initialize-with-httpclientfactory"></a>HttpClientFactory ile Başlat
Aşağıdaki örnek, bir `HttpClientFactory` oluşturup daha sonra bir ortak istemci uygulaması başlatmak için ' i gösterir:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient ve Xamarin iOS
Xamarin iOS kullanırken, iOS 7 ve daha yeni için tabanlı `HttpClient` işleyiciyi açıkça kullanan `NSURLSession`bir oluşturmanız önerilir. MSAL.NET, iOS 7 `HttpClient` ve daha `NSURLSessionHandler` yeni bir sürümü tarafından otomatik olarak oluşturulur. Daha fazla bilgi için, [HttpClient Için Xamarin iOS belgelerini](/xamarin/cross-platform/macios/http-stack)okuyun.