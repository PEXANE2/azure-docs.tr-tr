---
title: Web API 'Lerini çağıran masaüstü uygulaması (üretime geçiş)-Microsoft Identity platform
description: Web API 'Lerini çağıran bir masaüstü uygulaması oluşturmayı öğrenin (üretime geçin)
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
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a353b4577f8cfa9ba279ad2793e1a7ab8b27e55
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268330"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>Web API 'Lerini çağıran masaüstü uygulaması-üretime taşı

Bu makalede, uygulamanızı daha da geliştirmek ve üretime taşımak için ayrıntılar verilmektedir.

## <a name="handling-errors-in-desktop-applications"></a>Masaüstü uygulamalarında hataları işleme

Farklı akışlarda, sessiz akışlar için hataları nasıl işleyeceğinizi öğrendiniz (kod parçacıkları bölümünde gösterildiği gibi). Ayrıca, etkileşimin gerekli olduğu durumlar olduğunu da gördünüz (artımlı izin ve koşullu erişim).

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>Kullanıcı onayını birkaç kaynağın önüne alma

> [!NOTE]
> Birkaç kaynağa onay alınması Microsoft Identity platform için geçerlidir, ancak Azure Active Directory (Azure AD) B2C için değildir. Azure AD B2C, Kullanıcı onayını değil yalnızca yönetici onayını destekler.

Microsoft Identity platform (v 2.0) uç noktası aynı anda birkaç kaynak için bir belirteç almanıza izin vermez. Bu nedenle, `scopes` parametresi yalnızca tek bir kaynak için kapsam içerebilir. Kullanıcının `extraScopesToConsent` parametresini kullanarak birkaç kaynağa ön onay vererek emin olabilirsiniz.

Örneğin, iki kaynağınız varsa, her biri iki kapsamı vardır:

- `https://mytenant.onmicrosoft.com/customerapi`-2 kapsam `customer.read` ve`customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi`-2 kapsam `vendor.read` ve`vendor.write`

Parametresine sahip olan `.WithAdditionalPromptToConsent` değiştiriciyi kullanmanız gerekir. `extraScopesToConsent`

Örneğin:

### <a name="in-msalnet"></a>MSAL.NET içinde

```CSharp
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

Bu çağrı size ilk Web API 'SI için bir erişim belirteci alacak.

İkinci Web API 'sini çağırmanız gerektiğinde API 'yi çağırabilirsiniz `AcquireTokenSilent` :

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsenting-each-time-the-app-is-run"></a>Microsoft Kişisel hesabı, uygulamanın her çalıştırılışında reconsenting gerektirir

Microsoft kişisel hesap kullanıcıları için, yetkilendirmede her bir yerel istemcide (masaüstü/mobil uygulama) onay için yeniden sorma, amaçlanan davranıştır. Yerel istemci kimliği, doğal olarak güvenli olmayan bir şekilde (kimlik kanıtlamaları için Microsoft Identity platformu ile gizli dizi olan gizli bir istemci uygulamasının aksine). Microsoft Identity platformu, uygulamanın her yetkilendirildiği her seferinde kullanıcıdan izin vermesini isteyerek tüketici hizmetleri için bu eksik güvenliği azaltmayı tercih seçti.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
