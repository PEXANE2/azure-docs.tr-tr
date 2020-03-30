---
title: ADAL & MSAL uygulamaları arasında SSO (iOS/macOS) - Microsoft kimlik platformu | Azure
description: ''
services: active-directory
documentationcenter: dev-center-name
author: mmacy
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: ''
ms.custom: aaddev
ms.openlocfilehash: 2fbb6e837ae898daf4bc78d5cccc75660463e8a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085407"
---
# <a name="how-to-sso-between-adal-and-msal-apps-on-macos-and-ios"></a>Nasıl yapilir: macOS ve iOS'ta ADAL ve MSAL uygulamaları arasında SSO

iOS için Microsoft Kimlik Doğrulama Kitaplığı (MSAL), Uygulamalar arasında SSO durumunu [ADAL Objective-C](https://github.com/AzureAD/azure-activedirectory-library-for-objc) ile paylaşabilir. Uygulamalarınızı Kendi hızınızda MSAL'a geçirerek, kullanıcılarınızın ADAL ve MSAL tabanlı uygulamaların bir karışımıyla bile çapraz uygulama SSO'sundan yararlanmaya devam etmesini sağlayabilirsiniz.

MSAL SDK'yı kullanan uygulamalar arasında SSO kurma kılavuzu arıyorsanız, [birden çok uygulama arasında Silent SSO'ya](single-sign-on-macos-ios.md#silent-sso-between-apps)bakın. Bu makale, ADAL ve MSAL arasındaki SSO'ya odaklanır.

SSO'yu uygulayan ayrıntılar, kullanmakta olduğunuz ADAL sürümüne bağlıdır.

## <a name="adal-27x"></a>ADAL 2.7.x

Bu bölüm MSAL ve ADAL 2.7.x arasındaki SSO farklarını kapsamaktadır.

### <a name="cache-format"></a>Önbellek biçimi

ADAL 2.7.x MSAL önbellek biçimini okuyabilir. ADAL 2.7.x sürümü ile çapraz uygulama SSO için özel bir şey yapmanızgerekmez. Ancak, bu iki kitaplığın desteklediği hesap tanımlayıcıları arasındaki farklılıkların farkında olmanız gerekir.

### <a name="account-identifier-differences"></a>Hesap tanımlayıcı farkları

MSAL ve ADAL farklı hesap tanımlayıcıları kullanır. ADAL, birincil hesap tanımlayıcısı olarak UPN'yi kullanır. MSAL, nesne kimliği ve AAD hesapları için kiracı kimliği ve diğer hesap türleri için bir `sub` talep temel alan görüntülenebilir olmayan bir hesap tanımlayıcısı kullanır.

MSAL sonucu `MSALAccount` bir nesne aldığınızda, özellikte bir hesap tanımlayıcısı `identifier` içerir. Uygulama sonraki sessiz istekler için bu tanımlayıcıyı kullanmalıdır.

Buna ek `identifier` `MSALAccount` olarak , nesne adlı `username`görüntülenebilir bir tanımlayıcı içerir. Bu ADAL'da `userId` tercüme edilir. `username`benzersiz bir tanımlayıcı olarak kabul edilmez ve her zaman değişebilir, bu nedenle yalnızca ADAL ile geriye dönük uyumluluk senaryoları için kullanılmalıdır. MSAL önbellek sorgularını, `username` `identifier`sorgulamanın tavsiye `identifier` edildiği veya tarafından sorgulanmasının önerildiği önbellek sorgularını destekler.

Aşağıdaki tabloda ADAL ve MSAL arasındaki hesap tanımlayıcı farkları özetlenebilir:

| Hesap tanımlayıcısı                | MSAL                                                         | ADAL 2.7.x      | Eski ADAL (ADAL 2.7.x'ten önce) |
| --------------------------------- | ------------------------------------------------------------ | --------------- | ------------------------------ |
| görüntülenebilir tanımlayıcı            | `username`                                                   | `userId`        | `userId`                       |
| benzersiz görüntülenebilir tanımlayıcı | `identifier`                                                 | `homeAccountId` | Yok                            |
| Bilinen hesap kimliği yok               | Tüm hesapları `allAccounts:` API üzerinden sorgula`MSALPublicClientApplication` | Yok             | Yok                            |

Bu tanımlayıcıları sağlayan `MSALAccount` arabirimdir:

```objc
@protocol MSALAccount <NSObject>

/*!
 Displayable user identifier. Can be used for UI and backward compatibility with ADAL.
 */
@property (readonly, nullable) NSString *username;

/*!
 Unique identifier for the account.
 Save this for account lookups from cache at a later point.
 */
@property (readonly, nullable) NSString *identifier;

/*!
 Host part of the authority string used for authentication based on the issuer identifier.
 */
@property (readonly, nonnull) NSString *environment;

/*!
 ID token claims for the account.
 Can be used to read additional information about the account, e.g. name
 Will only be returned if there has been an id token issued for the client Id for the account's source tenant.
 */
@property (readonly, nullable) NSDictionary<NSString *, NSString *> *accountClaims;

@end
```

### <a name="sso-from-msal-to-adal"></a>MSAL'dan ADAL'a SSO

Bir MSAL uygulamanız ve Bir ADAL uygulamanız varsa ve kullanıcı MSAL tabanlı uygulamaya ilk olarak giriş yaparsa, `username` `MSALAccount` Nesneyi kaydedip ADAL tabanlı uygulamanıza `userId`aktararak SSO'yu ADAL uygulamasından alabilirsiniz. ADAL daha sonra API ile `acquireTokenSilentWithResource:clientId:redirectUri:userId:completionBlock:` sessizce hesap bilgilerini bulabilirsiniz.

### <a name="sso-from-adal-to-msal"></a>ADAL'dan MSAL'a SSO

Bir MSAL uygulamanız ve Bir ADAL uygulamanız varsa ve kullanıcı ADAL tabanlı uygulamaya ilk giriş yaparsa, MSAL'daki hesap aramaları için ADAL kullanıcı tanımlayıcılarını kullanabilirsiniz. Bu, ADAL'dan MSAL'a geçerken de geçerlidir.

#### <a name="adals-homeaccountid"></a>ADAL'ın ana hesabıAccountId

ADAL 2.7.x `homeAccountId` bu `ADUserInformation` özellik aracılığıyla sonuç nesnedöner:

```objc
/*! Unique AAD account identifier across tenants based on user's home OID/home tenantId. */
@property (readonly) NSString *homeAccountId;
```

`homeAccountId`ADAL'ın ki `identifier` MSAL'a eşdeğerdir. Bu `accountForIdentifier:error:` tanımlayıcıyı, API ile hesap aramaları için MSAL'da kullanmak üzere kaydedebilirsiniz.

#### <a name="adals-userid"></a>ADAL'ın`userId`

`homeAccountId` Kullanılamıyorsa veya yalnızca görüntülenebilir tanımlayıcınız varsa, MSAL'daki hesabı aramak `userId` için ADAL's'ı kullanabilirsiniz.

MSAL'da, önce bir `username` hesaba `identifier`bakın veya . Her `identifier` zaman varsa sorgulama için kullanın ve `username` yalnızca bir geri dönüş olarak kullanın. Hesap bulunursa, `acquireTokenSilent` aramalardaki hesabı kullanın.

Amaç-C:

```objc
NSString *msalIdentifier = @"previously.saved.msal.account.id";
MSALAccount *account = nil;
    
if (msalIdentifier)
{
    // If you have MSAL account id returned either from MSAL as identifier or ADAL as homeAccountId, use it
    account = [application accountForIdentifier:@"my.account.id.here" error:nil];
}
else
{
    // Fallback to ADAL userId for migration
    account = [application accountForUsername:@"adal.user.id" error:nil];
}
    
if (!account)
{
  // Account not found.
  return;
}

MSALSilentTokenParameters *silentParameters = [[MSALSilentTokenParameters alloc] initWithScopes:@[@"user.read"] account:account];
[application acquireTokenSilentWithParameters:silentParameters completionBlock:completionBlock];
```

Swift:

```swift
        
let msalIdentifier: String?
var account: MSALAccount
        
do {
  if let msalIdentifier = msalIdentifier {
    account = try application.account(forIdentifier: msalIdentifier)
  }
  else {
    account = try application.account(forUsername: "adal.user.id") 
  }
             
  let silentParameters = MSALSilentTokenParameters(scopes: ["user.read"], account: account)          
  application.acquireTokenSilent(with: silentParameters) {
    (result: MSALResult?, error: Error?) in
    // handle result
  }  
} catch let error as NSError {
  // handle error or account not found
}
```



MSAL desteklenen hesap arama API'leri:

```objc
/*!
 Returns account for the given account identifier (received from an account object returned in a previous acquireToken call)
 
 @param  error      The error that occurred trying to get the accounts, if any, if you're
                    not interested in the specific error pass in nil.
 */
- (nullable MSALAccount *)accountForIdentifier:(nonnull NSString *)identifier
                                         error:(NSError * _Nullable __autoreleasing * _Nullable)error;
    
/*!
Returns account for for the given username (received from an account object returned in a previous acquireToken call or ADAL)
    
@param  username    The displayable value in UserPrincipleName(UPN) format
@param  error       The error that occurred trying to get the accounts, if any, if you're
                    not interested in the specific error pass in nil.
*/
- (MSALAccount *)accountForUsername:(NSString *)username
                              error:(NSError * __autoreleasing *)error;
```

## <a name="adal-2x-266"></a>ADAL 2.x-2.6.6

Bu bölüm MSAL ve ADAL 2.x-2.6.6 arasındaki SSO farklarını kapsamaktadır.

Eski ADAL sürümleri msal önbellek biçimini yerel olarak desteklemez. Ancak, ADAL'dan MSAL'a sorunsuz geçiş sağlamak için, MSAL eski ADAL önbellek biçimini kullanıcı kimlik bilgilerini yeniden sormadan okuyabilir.

`homeAccountId` Eski ADAL sürümlerinde kullanılamadığından, aşağıdakileri kullanarak hesapları aramanız `username`gerekir:

```objc
/*!
 Returns account for for the given username (received from an account object returned in a previous acquireToken call or ADAL)

 @param  username    The displayable value in UserPrincipleName(UPN) format
 @param  error       The error that occurred trying to get the accounts, if any.  If you're not interested in the specific error pass in nil.
 */
- (MSALAccount *)accountForUsername:(NSString *)username
                              error:(NSError * __autoreleasing *)error;
```

Örnek:

Amaç-C:


```objc
MSALAccount *account = [application accountForUsername:@"adal.user.id" error:nil];;
MSALSilentTokenParameters *silentParameters = [[MSALSilentTokenParameters alloc] initWithScopes:@[@"user.read"] account:account];
[application acquireTokenSilentWithParameters:silentParameters completionBlock:completionBlock];
```

Swift:

```swift
do {
  let account = try application.account(forUsername: "adal.user.id")          
  let silentParameters = MSALSilentTokenParameters(scopes: ["user.read"], account: account)
  application.acquireTokenSilent(with: silentParameters) { 
    (result: MSALResult?, error: Error?) in
    // handle result
  }   
} catch let error as NSError { 
  // handle error or account not found
}
```



Alternatif olarak, ADAL'dan gelen hesap bilgilerini de okuyacak olan tüm hesapları okuyabilirsiniz:

Amaç-C:

```objc
NSArray *accounts = [application allAccounts:nil];
    
if ([accounts count] == 0)
{
  // No account found.
  return; 
}
if ([accounts count] > 1)
{
  // You might want to display an account picker to user in actual application
  // For this sample we assume there's only ever one account in cache
  return;
}
    ``
MSALSilentTokenParameters *silentParameters = [[MSALSilentTokenParameters alloc] initWithScopes:@[@"user.read"] account:accounts[0]];
[application acquireTokenSilentWithParameters:silentParameters completionBlock:completionBlock];
```

Swift:

```swift
      
do {
  let accounts = try application.allAccounts()
  if accounts.count == 0 {
    // No account found.
    return
  }
  if accounts.count > 1 {
    // You might want to display an account picker to user in actual application
    // For this sample we assume there's only ever one account in cache
    return
  }
  
  let silentParameters = MSALSilentTokenParameters(scopes: ["user.read"], account: accounts[0])
  application.acquireTokenSilent(with: silentParameters) {
    (result: MSALResult?, error: Error?) in
    // handle result or error  
  }  
} catch let error as NSError { 
  // handle error
}
```



## <a name="next-steps"></a>Sonraki adımlar

[Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin
