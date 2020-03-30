---
title: Özel talepler isteyin (MSAL iOS/macOS) | Azure
titleSuffix: Microsoft identity platform
description: Özel talepler nasıl isteyeceğini öğrenin.
services: active-directory
documentationcenter: ''
author: mmacy
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: marsma
ms.reviewer: ''
ms.custom: aaddev
ms.openlocfilehash: 44158296faaf238fd72f2360149d3d93f68c5ba0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085596"
---
# <a name="how-to-request-custom-claims-using-msal-for-ios-and-macos"></a>Nasıl yapilir: iOS ve macOS için MSAL kullanarak özel talep teshis ihya

OpenID Connect, UserInfo Bitiş Noktası'ndan ve/veya Id Jetonundan bireysel taleplerin iadesini isteğe bağlı olarak talep etmenizi sağlar. Talep isteği, istenen taleplerin listesini içeren bir JSON nesnesi olarak temsil edilir. Daha fazla bilgi için [OpenID Connect Core 1.0'a](https://openid.net/specs/openid-connect-core-1_0-final.html#ClaimsParameter) bakın.

iOS ve macOS için Microsoft Kimlik Doğrulama Kitaplığı (MSAL), hem etkileşimli hem de sessiz belirteç edinme senaryolarında belirli talepler talep edilmesine olanak tanır. Bunu `claimsRequest` parametre aracılığıyla yapar.

Bunun gerekli olduğu birden çok senaryo vardır. Örnek:

- Başvurunuz için standart kümenin dışında talepte bulunmak.
- Uygulamanız için kapsamlar kullanılarak belirtilemeyen standart taleplerin belirli birleşimlerini istemek. Örneğin, bir erişim belirteci eksik talepler nedeniyle reddedilirse, uygulama MSAL kullanarak eksik talepleri isteyebilir.

> [!NOTE]
> MSAL, talep isteği belirtildiğinde erişim belirteç önbelleğini atlar. Yalnızca ek talepler gerektiğinde `claimsRequest` parametre sağlamak önemlidir (her MSAL API çağrısında her zaman aynı `claimsRequest` parametrenin sağlanmasının aksine).

`claimsRequest`belirtilebilir `MSALSilentTokenParameters` ve: `MSALInteractiveTokenParameters`

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
`MSALClaimsRequest`JSON Talepleri isteğinin NSString gösteriminden oluşturulabilir. 

Amaç-C:

```objc
NSError *claimsError = nil;
MSALClaimsRequest *request = [[MSALClaimsRequest alloc] initWithJsonString:@"{\"id_token\":{\"auth_time\":{\"essential\":true},\"acr\":{\"values\":[\"urn:mace:incommon:iap:silver\"]}}}" error:&claimsError];
```

Swift:

```swift
var requestError: NSError? = nil
let request = MSALClaimsRequest(jsonString: "{\"id_token\":{\"auth_time\":{\"essential\":true},\"acr\":{\"values\":[\"urn:mace:incommon:iap:silver\"]}}}",
                                        error: &requestError)
```



Ayrıca ek özel talepler istenerek değiştirilebilir:

Amaç-C:

```objc
MSALIndividualClaimRequest *individualClaimRequest = [[MSALIndividualClaimRequest alloc] initWithName:@"custom_claim"];
individualClaimRequest.additionalInfo = [MSALIndividualClaimRequestAdditionalInfo new];
individualClaimRequest.additionalInfo.essential = @1;
individualClaimRequest.additionalInfo.value = @"myvalue";
[request requestClaim:individualClaimRequest forTarget:MSALClaimsRequestTargetIdToken error:&claimsError];
```

Swift:

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



`MSALClaimsRequest`daha sonra belirteç parametrelerinde ayarlanmalı ve MSAL belirteç kazanımlarından birine SAĞlanmalıdır:

Amaç-C:

```objc
MSALPublicClientApplication *application = ...;
MSALWebviewParameters *webParameters = ...;

MSALInteractiveTokenParameters *parameters = [[MSALInteractiveTokenParameters alloc] initWithScopes:@[@"user.read"]
                                                                                  webviewParameters:webParameters];
parameters.claimsRequest = request;
    
[application acquireTokenWithParameters:parameters completionBlock:completionBlock];
```

Swift:

```swift
let application: MSALPublicClientApplication!
let webParameters: MSALWebviewParameters!
        
let parameters = MSALInteractiveTokenParameters(scopes: ["user.read"], webviewParameters: webParameters)
parameters.claimsRequest = request
        
application.acquireToken(with: parameters) { (result: MSALResult?, error: Error?) in            ...

```



## <a name="next-steps"></a>Sonraki adımlar

[Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin
