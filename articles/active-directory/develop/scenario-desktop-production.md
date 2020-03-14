---
title: Web API 'Lerini üretime çağıran masaüstü uygulamasını taşıma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini üretime çağıran bir masaüstü uygulamasını nasıl taşıyacağınızı öğrenin
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79262574"
---
# <a name="desktop-app-that-calls-web-apis-move-to-production"></a>Web API 'Lerini çağıran masaüstü uygulaması: üretime taşı

Bu makalede, Web API 'Lerini üretime çağıran masaüstü uygulamanızı nasıl taşıyacağınızı öğreneceksiniz.

## <a name="handle-errors-in-desktop-applications"></a>Masaüstü uygulamalarında hataları işleme

Farklı akışlarda, kod parçacıkları bölümünde gösterildiği gibi sessiz akışlar için hataları nasıl işleyeceğinizi öğrendiniz. Ayrıca, artan onay ve koşullu erişim bölümünde olduğu gibi, etkileşimin gerekli olduğu durumlar olduğunu da gördünüz.

## <a name="have-the-user-consent-upfront-for-several-resources"></a>Kullanıcı onayını birkaç kaynağın önüne alma

> [!NOTE]
> Birkaç kaynağa onay alınması Microsoft Identity platform için geçerlidir ancak Azure Active Directory (Azure AD) B2C için değildir. Azure AD B2C, Kullanıcı onayını değil yalnızca yönetici onayını destekler.

Microsoft Identity platform (v 2.0) uç noktası ile aynı anda birkaç kaynak için bir belirteç alamazsınız. `scopes` parametresi yalnızca tek bir kaynak için kapsam içerebilir. Kullanıcının `extraScopesToConsent` parametresini kullanarak birkaç kaynağa ön onay vererek emin olabilirsiniz.

Örneğin, her biri iki kapsamın bulunduğu iki kaynak olabilir:

- `https://mytenant.onmicrosoft.com/customerapi` kapsamlar `customer.read` ve `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` kapsamlar `vendor.read` ve `vendor.write`

Bu örnekte, `extraScopesToConsent` parametresine sahip `.WithAdditionalPromptToConsent` değiştiricisini kullanın.

Örneğin:

### <a name="in-msalnet"></a>MSAL.NET içinde

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

### <a name="in-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL içinde

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

SWIFT

```swift
let scopesForCustomerApi = ["https://mytenant.onmicrosoft.com/customerapi/customer.read",
                            "https://mytenant.onmicrosoft.com/customerapi/customer.write"]

let scopesForVendorApi = ["https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                          "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopesForCustomerApi, webviewParameters: MSALWebviewParameters())
interactiveParameters.extraScopesToConsent = scopesForVendorApi
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in /* handle result */ })
```

Bu çağrı, ilk Web API 'SI için bir erişim belirteci alır.

İkinci Web API 'sini çağırmanız gerektiğinde `AcquireTokenSilent` API 'sini çağırın.

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsent-each-time-the-app-runs"></a>Uygulama her çalıştığında Microsoft Kişisel hesabı için reconsent gerekir

Microsoft kişisel hesap kullanıcıları için, yetkilendirmede her bir yerel istemcide (masaüstü veya mobil uygulama) onay için yeniden sorma, amaçlanan davranıştır. Yerel istemci kimliği, gizli istemci uygulama kimliğinin aksine, doğal olarak güvenli değildir. Gizli istemci uygulamaları, kimlik kanıtlamaları için Microsoft Identity platformu ile gizli dizi değişimi. Microsoft Identity platformu, uygulamanın her yetkilendirildiği her seferinde kullanıcıdan izin vermesini isteyerek tüketici hizmetleri için bu ingüvenliğin azaltılmasına karar verebilir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
