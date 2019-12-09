---
title: Özel talepler iste (MSAL iOS/macOS) | Mavisi
titleSuffix: Microsoft identity platform
description: Özel talepler isteme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: twhitney
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 65b85a5c551b4e26c39959a1bd4f435a5fa4fe6e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74920319"
---
# <a name="how-to-request-custom-claims-using-msal-for-ios-and-macos"></a>Nasıl yapılır: iOS ve macOS için MSAL kullanarak özel talepler ISTEME

OpenID Connect, isteğe bağlı olarak, UserInfo uç noktasından ve/veya KIMLIK belirtecinde tek tek taleplerin döndürülmesini ister. Talep isteği, istenen talepler listesini içeren bir JSON nesnesi olarak temsil edilir. Daha fazla bilgi için bkz. [OpenID Connect Core 1,0](https://openid.net/specs/openid-connect-core-1_0-final.html#ClaimsParameter) .

İOS ve macOS için Microsoft kimlik doğrulama kitaplığı (MSAL), hem etkileşimli hem de sessiz belirteç alma senaryolarında belirli talepler istemeyi sağlar. Bu, `claimsRequest` parametresi aracılığıyla yapılır.

Bunun gerekli olduğu birden çok senaryo vardır. Örnek:

- Uygulamanız için standart küme dışında talepler isteme.
- Uygulamanız için kapsamlar kullanılarak belirtilemez standart talepler için belirli birleşimler isteniyor. Örneğin, eksik talepler nedeniyle erişim belirteci reddedilirse, uygulama MSAL kullanarak eksik talepleri isteyebilir.

> [!NOTE]
> MSAL, bir talep isteği belirtildiğinde erişim belirteci önbelleğini atlar. Ek talepler gerektiğinde yalnızca `claimsRequest` parametre sağlanması önemlidir (her bir MSAL API çağrısında aynı `claimsRequest` parametresini her zaman sağlamanın aksine).

`claimsRequest`, `MSALSilentTokenParameters` ve `MSALInteractiveTokenParameters`belirtilebilir:

```objc
/*!
 MSALTokenParameters is the base abstract class for all types of token parameters (silent and interactive).
 */
@interface MSALTokenParameters : NSObject

/*!
 The claims parameter that needs to be sent to authorization or token endpoint.
 If claims parameter is passed in silent flow, access token will be skipped and refresh token will be tried.
 */
@property (nonatomic, nullable) MSALClaimsRequest *claimsRequest;

@end
```
`MSALClaimsRequest`, JSON talep isteğinin NSString gösteriminden oluşturulabilir. 

Amaç-C:

```objc
NSError *claimsError = nil;
MSALClaimsRequest *request = [[MSALClaimsRequest alloc] initWithJsonString:@"{\"id_token\":{\"auth_time\":{\"essential\":true},\"acr\":{\"values\":[\"urn:mace:incommon:iap:silver\"]}}}" error:&claimsError];
```

SWIFT

```swift
var requestError: NSError? = nil
let request = MSALClaimsRequest(jsonString: "{\"id_token\":{\"auth_time\":{\"essential\":true},\"acr\":{\"values\":[\"urn:mace:incommon:iap:silver\"]}}}",
                                        error: &requestError)
```



Ayrıca, ek özel talepler isteyerek de değiştirilebilir:

Amaç-C:

```objc
MSALIndividualClaimRequest *individualClaimRequest = [[MSALIndividualClaimRequest alloc] initWithName:@"custom_claim"];
individualClaimRequest.additionalInfo = [MSALIndividualClaimRequestAdditionalInfo new];
individualClaimRequest.additionalInfo.essential = @1;
individualClaimRequest.additionalInfo.value = @"myvalue";
[request requestClaim:individualClaimRequest forTarget:MSALClaimsRequestTargetIdToken error:&claimsError];
```

SWIFT

```swift
let individualClaimRequest = MSALIndividualClaimRequest(name: "custom-claim")
let additionalInfo = MSALIndividualClaimRequestAdditionalInfo()
additionalInfo.essential = 1
additionalInfo.value = "myvalue"
individualClaimRequest.additionalInfo = additionalInfo
do {
  try request.requestClaim(individualClaimRequest, for: .idToken)
} catch let error as NSError {
  // handle error here  
}
        
```



`MSALClaimsRequest`, belirteç parametrelerinde ayarlanmalıdır ve MSAL Token alma API 'Lerinden birine sağlanmalıdır:

Amaç-C:

```objc
MSALPublicClientApplication *application = ...;
MSALWebviewParameters *webParameters = ...;

MSALInteractiveTokenParameters *parameters = [[MSALInteractiveTokenParameters alloc] initWithScopes:@[@"user.read"]
                                                                                  webviewParameters:webParameters];
parameters.claimsRequest = request;
    
[application acquireTokenWithParameters:parameters completionBlock:completionBlock];
```

SWIFT

```swift
let application: MSALPublicClientApplication!
let webParameters: MSALWebviewParameters!
        
let parameters = MSALInteractiveTokenParameters(scopes: ["user.read"], webviewParameters: webParameters)
parameters.claimsRequest = request
        
application.acquireToken(with: parameters) { (result: MSALResult?, error: Error?) in            ...

```



## <a name="next-steps"></a>Sonraki adımlar

[Kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin
