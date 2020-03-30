---
title: Masaüstü uygulaması arama web API'lerini üretime taşıyın - Microsoft kimlik platformu | Azure
description: Web API'lerini üretime çağıran bir masaüstü uygulamasını nasıl taşıyabilirsiniz öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c8a9cf0c05d8af14d52bb1efb536dc8bbe7db84d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262574"
---
# <a name="desktop-app-that-calls-web-apis-move-to-production"></a>Web API'lerini çağıran masaüstü uygulaması: Üretime geç

Bu makalede, web API'lerini çağıran masaüstü uygulamanızı üretime nasıl taşıyabileceğinizi öğreneceksiniz.

## <a name="handle-errors-in-desktop-applications"></a>Masaüstü uygulamalarındaki hataları işleme

Farklı akışlarda, kod parçacıklarında gösterildiği gibi sessiz akışlar için hataları nasıl işleyeceğiniz öğrenilir. Artımlı onam ve koşullu erişim gibi etkileşimin gerekli olduğu durumlar olduğunu da gördünüz.

## <a name="have-the-user-consent-upfront-for-several-resources"></a>Kullanıcının çeşitli kaynaklar için önceden onayını alın

> [!NOTE]
> Birden çok kaynak için onay almak Microsoft kimlik platformu için çalışır, ancak Azure Active Directory (Azure AD) B2C için çalışmaz. Azure AD B2C, kullanıcı onayı değil, yalnızca yönetici onayInı destekler.

Microsoft kimlik platformu (v2.0) bitiş noktası ile aynı anda birden fazla kaynak için bir belirteç alamazsınız. Parametre yalnızca `scopes` tek bir kaynak için kapsamlar içerebilir. `extraScopesToConsent` Parametreyi kullanarak kullanıcının çeşitli kaynaklara önceden onay verdiğinden emin olabilirsiniz.

Örneğin, her biri iki kapsamı olan iki kaynağınız olabilir:

- `https://mytenant.onmicrosoft.com/customerapi`kapsamları `customer.read` ve`customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi`kapsamları `vendor.read` ve`vendor.write`

Bu örnekte, `.WithAdditionalPromptToConsent` `extraScopesToConsent` parametreye sahip değiştiriciyi kullanın.

Örneğin:

### <a name="in-msalnet"></a>MSAL.NET yılında

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

### <a name="in-msal-for-ios-and-macos"></a>iOS ve macOS için MSAL'da

Amaç-C:

```objc
NSArray *scopesForCustomerApi = @[@"https://mytenant.onmicrosoft.com/customerapi/customer.read",
                                @"https://mytenant.onmicrosoft.com/customerapi/customer.write"];

NSArray *scopesForVendorApi = @[@"https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                              @"https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopesForCustomerApi webviewParameters:[MSALWebviewParameters new]];
interactiveParams.extraScopesToConsent = scopesForVendorApi;
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) { /* handle result */ }];
```

Swift:

```swift
let scopesForCustomerApi = ["https://mytenant.onmicrosoft.com/customerapi/customer.read",
                            "https://mytenant.onmicrosoft.com/customerapi/customer.write"]

let scopesForVendorApi = ["https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                          "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopesForCustomerApi, webviewParameters: MSALWebviewParameters())
interactiveParameters.extraScopesToConsent = scopesForVendorApi
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in /* handle result */ })
```

Bu arama, ilk web API'si için bir erişim belirteci alır.

İkinci web API'sini aramanız gerektiğinde `AcquireTokenSilent` API'yi arayın.

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsent-each-time-the-app-runs"></a>Microsoft kişisel hesabı, uygulama her çalıştığında yeniden onay gerektirir

Microsoft kişisel hesap kullanıcıları için, her yerel istemcinin (masaüstü veya mobil uygulama) yetkilendirme çağrısının onayını yeniden isteme, amaçlanan davranıştır. Yerel istemci kimliği, gizli istemci uygulama kimliğine aykırı olan doğal olarak güvensizdir. Gizli istemci uygulamaları, kimliklerini kanıtlamak için Microsoft Identity platformuyla bir sır alışverişinde bulunarak. Microsoft kimlik platformu, uygulama nın izin li olduğu her seferde kullanıcıdan onay isteyen tüketici hizmetleri için bu güvensizliği azaltmayı seçti.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
