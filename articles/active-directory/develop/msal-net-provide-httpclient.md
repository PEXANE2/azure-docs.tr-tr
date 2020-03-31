---
title: Bir HttpClient & proxy (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak Azure AD'ye bağlanmak için kendi HttpClient ve proxy'nizi sağlama hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76695066"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>MSAL.NET kullanarak kendi HttpClient ve proxy sağlama
[Bir ortak istemci uygulaması nın başlatılmasını](msal-net-initializing-client-applications.md)sağlarken, kendi HttpClient'ınızı sağlamak `.WithHttpClientFactory method` için bu uygulamayı kullanabilirsiniz.  Kendi HttpClient sağlamak gibi gelişmiş senaryolar bir HTTP proxy, kullanıcı aracısı üstbilgileri özelleştirme veya MSAL belirli bir HttpClient (örneğin ASP.NET Core web apps / API'ler kullanmaya zorlamak gibi gelişmiş senaryolar sağlar.

## <a name="initialize-with-httpclientfactory"></a>httpClientFactory ile başlatma
Aşağıdaki örnek, bir `HttpClientFactory` ortak istemci uygulaması oluşturmak ve sonra onunla birlikte bir istemci uygulama başlatmayı gösterir:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>Httpİsteve Xamarin iOS
Xamarin iOS kullanırken, açıkça iOS 7 ve `NSURLSession`daha yeni için -tabanlı işleyici kullanan bir `HttpClient` oluşturmanıza önerilir. MSAL.NET otomatik olarak `HttpClient` iOS `NSURLSessionHandler` 7 ve daha yeni için kullanan bir oluşturur. Daha fazla bilgi için [HttpClient için Xamarin iOS belgelerini](/xamarin/cross-platform/macios/http-stack)okuyun.