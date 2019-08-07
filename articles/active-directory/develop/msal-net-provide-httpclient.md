---
title: HttpClient ve proxy sağlama (MSAL.NET) | Mavisi
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak Azure AD 'ye bağlanmak üzere kendi HttpClient ve proxy 'nizi sağlama hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: daae88cd8e76d0ae1af04c45a7191027e9adece9
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68834944"
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
Xamarin iOS kullanırken, iOS 7 ve daha yeni için tabanlı `HttpClient` işleyiciyi açıkça `NSURLSession`kullanan bir oluşturmanız önerilir. MSAL.net, iOS 7 `HttpClient` ve daha `NSURLSessionHandler` yeni bir sürümü tarafından otomatik olarak oluşturulur. Daha fazla bilgi için, [HttpClient Için Xamarin iOS belgelerini](/xamarin/cross-platform/macios/http-stack)okuyun.