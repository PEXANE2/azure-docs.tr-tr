---
title: Anahtarlık yapılandırma | Microsoft Identity platformu
description: Anahtarlığınıza, uygulamanızın belirteçleri anahtarlıkta önbelleğe abilmesi için Anahtarlık yapılandırmayı öğrenin.
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
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46dc3a44041acd90dbab449215138eeecbda7105
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72264186"
---
# <a name="configure-keychain"></a>Anahtarlık yapılandırma

[İOS ve macOS Için Microsoft kimlik doğrulama kitaplığı](msal-overview.md) (msal) bir kullanıcıya kaydolursa veya bir belirteci yenilediğinde, anahtarlıkta belirteçleri önbelleğe almaya çalışır. Anahtarlıkta önbelleğe alma belirteçleri, MSAL 'ın aynı Apple geliştiricisi tarafından dağıtılan birden çok uygulama arasında sessiz çoklu oturum açma (SSO) sağlamasına izin verir. Anahtar Zinciri erişim grupları işlevselliği aracılığıyla SSO elde edilir. Daha fazla bilgi için bkz. Apple 'ın [Anahtarlık öğeleri belgeleri](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc).

Bu makalede, MSAL 'in önbelleğe alınmış belirteçleri iOS ve macOS anahtarlığa yazabilmesi için uygulama yetkilendirmelerini yapılandırma konuları ele alınmaktadır.

## <a name="default-keychain-access-group"></a>Varsayılan Anahtarlık erişim grubu

### <a name="ios"></a>iOS

İOS üzerinde MSAL, varsayılan olarak `com.microsoft.adalcache` erişim grubunu kullanır. Bu, hem MSAL hem de Azure AD kimlik doğrulama kitaplığı (ADAL) SDK 'Ları tarafından kullanılan paylaşılan erişim grubudur ve aynı yayımcıdan gelen birden çok uygulama arasında en iyi çoklu oturum açma (SSO) deneyimini sağlar.

İOS 'ta, `com.microsoft.adalcache` Anahtarlık grubunu, **proje @no__t ayarları**altındaki Xcode 'da,-4**anahtarlık paylaşımı** @no__t**için**, uygulamanızın yetkilendirme grubuna ekleyin

### <a name="macos"></a>macOS

MacOS üzerinde MSAL, varsayılan olarak `com.microsoft.identity.universalstorage` erişim grubunu kullanır.

MacOS anahtar zinciri sınırlamaları nedeniyle, MSAL 'in `access group` ' ı macOS 10,14 ve önceki sürümlerde Anahtarlık erişim grubu özniteliğine (bkz. [Ksecattraccessgroup](https://developer.apple.com/documentation/security/ksecattraccessgroup?language=objc)) doğrudan çevrimez. Ancak, aynı Apple geliştiricisi tarafından dağıtılan birden çok uygulamanın sessiz SSO 'ya sahip olduğundan emin olarak SSO perspektifinden aynı şekilde davranır.

MacOS 10,15 ve sonraki sürümlerinde (macOS Catalina), MSAL, iOS 'a benzer şekilde sessiz SSO elde etmek için Anahtarlık erişim grubu özniteliğini kullanır.

## <a name="custom-keychain-access-group"></a>Özel Anahtarlık erişim grubu

Farklı bir Anahtarlık erişim grubu kullanmak istiyorsanız, `MSALPublicClientApplication` ' i oluşturmadan önce `MSALPublicClientApplicationConfig` oluştururken özel grubunuzu geçirebilirsiniz, örneğin:

Amaç-C:

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



SWIFT

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



## <a name="disable-keychain-sharing"></a>Anahtarlık paylaşımını devre dışı bırak

Birden çok uygulama arasında SSO durumu paylaşmak istemiyorsanız veya herhangi bir Anahtarlık erişim grubunu kullanarak, uygulama paketi KIMLIĞINI keychainGroup olarak geçirerek anahtar zinciri paylaşımını devre dışı bırakın:

Amaç-C:

```objc
config.cacheConfig.keychainSharingGroup = [[NSBundle mainBundle] bundleIdentifier];
```

SWIFT

```swift
if let bundleIdentifier = Bundle.main.bundleIdentifier {
    config.cacheConfig.keychainSharingGroup = bundleIdentifier
}
```

## <a name="handle--34018-error-failed-to-set-item-into-keychain"></a>Tanıtıcı-34018 hatası (öğe anahtarlığa ayarlanamadı)

Hata-34018 normalde Anahtarlık doğru şekilde yapılandırılmadığı anlamına gelir. MSAL ' de yapılandırılmış Anahtarlık erişim grubunun yetkilendirmeler ' de yapılandırılmış bir ile eşleştiğinden emin olun.

## <a name="ensure-your-application-is-properly-signed"></a>Uygulamanızın düzgün şekilde imzalandığından emin olun

MacOS 'ta uygulamalar, geliştirici tarafından imzalanmadan çalıştırılabilir. MSAL işlevlerinin büyük bir kısmının çalışmaya devam etmesi, Anahtarlık erişimi aracılığıyla SSO 'nun, uygulamanın imzalanmasını gerektirir. Birden çok Anahtarlık istemler yaşıyorsanız, uygulamanızın imzasının geçerli olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Apple 'ın [, anahtar zinciri öğelerine bir uygulamalar koleksiyonu Ile paylaşım erişimi](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) üzerinde Anahtarlık erişim grupları hakkında daha fazla bilgi edinin.
