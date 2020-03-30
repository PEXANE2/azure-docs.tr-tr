---
title: Anahtarlık yapılandırma
titleSuffix: Microsoft identity platform
description: Uygulamanızın anahtarlıktaki belirteçleri önbelleğe alması için anahtarzinciri nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: d94bf7ffe955c9ec9ee2a2e7f7c4dbaaa28df270
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085856"
---
# <a name="configure-keychain"></a>Anahtarlık yapılandırma

[iOS ve macOS (MSAL) için Microsoft Kimlik Doğrulama Kitaplığı](msal-overview.md) bir kullanıcıda işaretlediğinde veya bir belirteci yenilediğinde, anahtarlıktaki belirteçleri önbelleğe almaya çalışır. Anahtarlıktaki belirteçleri önbelleğe alma, MSAL'ın aynı Apple geliştiricisi tarafından dağıtılan birden çok uygulama arasında sessiz tek oturum açma (SSO) sağlamasına olanak tanır. SSO, anahtarlık erişim grupları işlevi ile elde edilir. Daha fazla bilgi için Apple'ın [Anahtarlık Öğeleri belgelerine](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc)bakın.

Bu makalede, MSAL'ın iOS ve macOS anahtar zincirine önbelleğe alınmış belirteçler yazabilmesi için uygulama yetkilerinin nasıl yapılandırılabildiği ele allanmıştır.

## <a name="default-keychain-access-group"></a>Varsayılan anahtar zinciri erişim grubu

### <a name="ios"></a>iOS

iOS'taki MSAL `com.microsoft.adalcache` varsayılan olarak erişim grubunu kullanır. Bu, hem MSAL hem de Azure AD Kimlik Doğrulama Kitaplığı (ADAL) SDK'ları tarafından kullanılan paylaşılan erişim grubudur ve aynı yayımcıdan gelen birden fazla uygulama arasında en iyi tek oturum açma (SSO) deneyimini sağlar.

iOS'ta, `com.microsoft.adalcache` **Proje ayarları** > **Özellikleri** > **Anahtarzinciri paylaşımı** altında uygulamanızın XCode'daki yakınına anahtarlık grubunu ekleyin

### <a name="macos"></a>macOS

macOS'taki MSAL varsayılan olarak erişim grubunu kullanır. `com.microsoft.identity.universalstorage`

macOS anahtar zinciri sınırlamaları nedeniyle, `access group` MSAL's doğrudan macOS 10.14 ve önceki anahtarlık erişim grubu özniteliği [(bkz. kSecAttrAccessGroup)](https://developer.apple.com/documentation/security/ksecattraccessgroup?language=objc)anlamına gelmez. Ancak, aynı Apple geliştiricisi tarafından dağıtılan birden fazla uygulamanın sessiz SSO'ya sahip olmasını sağlayarak SSO perspektifinden benzer şekilde hareket eder.

MacOS 10.15'te (macOS Catalina) MSAL, iOS'a benzer şekilde sessiz SSO elde etmek için anahtarlık erişim grubu özelliğini kullanır.

## <a name="custom-keychain-access-group"></a>Özel anahtarzinciri erişim grubu

Farklı bir anahtar zinciri erişim grubu kullanmak istiyorsanız, oluşturmadan `MSALPublicClientApplicationConfig` önce özel `MSALPublicClientApplication`grubunuzu geçirebilirsiniz, bu gibi:

# <a name="objective-c"></a>[Objective-C](#tab/objc)

```objc
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"your-client-id"
                                                                                            redirectUri:@"your-redirect-uri"
                                                                                              authority:nil];
    
config.cacheConfig.keychainSharingGroup = @"custom-group";
    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:nil];
    
// Now call acquiretoken. 
// Tokens will be saved into the "custom-group" access group
// and only shared with other applications declaring the same access group
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
let config = MSALPublicClientApplicationConfig(clientId: "your-client-id",
                                            redirectUri: "your-redirect-uri",
                                              authority: nil)
config.cacheConfig.keychainSharingGroup = "custom-group"
        
do {
  let application = try MSALPublicClientApplication(configuration: config)
  // continue on with application          
} catch let error as NSError {
  // handle error here
}       
```

---

## <a name="disable-keychain-sharing"></a>Anahtarlık paylaşımını devre dışı

SSO durumunu birden çok uygulama arasında paylaşmak istemiyorsanız veya herhangi bir anahtar zinciri erişim grubu kullanmak istemiyorsanız, anahtarlık kimliğinizi anahtarlık Grubunuz olarak geçirerek anahtarlık paylaşımını devre dışı bıraktınız:

# <a name="objective-c"></a>[Objective-C](#tab/objc)

```objc
config.cacheConfig.keychainSharingGroup = [[NSBundle mainBundle] bundleIdentifier];
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
if let bundleIdentifier = Bundle.main.bundleIdentifier {
    config.cacheConfig.keychainSharingGroup = bundleIdentifier
}
```

---

## <a name="handle--34018-error-failed-to-set-item-into-keychain"></a>Tanıtıcı -34018 hatası (öğeyi anahtarlık olarak ayarlayamadı)

Hata -34018 normalde anahtarzincirinin doğru yapılandırılmadığı anlamına gelir. MSAL'da yapılandırılan anahtarlık erişim grubunun yetkilendirmelerde yapılandırılanla eşleştiğinden emin olun.

## <a name="ensure-your-application-is-properly-signed"></a>Uygulamanızın düzgün imzalandığından emin olun

macOS'ta, uygulamalar geliştirici tarafından imzalanmadan yürütülebilir. MSAL'ın işlevselliğinin çoğu çalışmaya devam ederken, anahtarlık erişimi aracılığıyla SSO'nun uygulamanın imzalanması gerekir. Birden çok anahtarlık istemi yle karşınızdaysanız, uygulamanızın imzasının geçerli olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Apple'ın [AnahtarZinciri Öğelerine Erişimi Paylaşma](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) uygulamasındaki anahtarlık erişim grupları hakkında daha fazla bilgi edinin.
