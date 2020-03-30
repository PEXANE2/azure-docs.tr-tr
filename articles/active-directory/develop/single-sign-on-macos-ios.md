---
title: macOS ve iOS'ta SSO'ya yapı
titleSuffix: Microsoft identity platform
description: macOS ve iOS'ta tek bir tabelayı (SSO) nasıl yapılandırıştırmayı öğrenin.
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
ms.date: 02/03/2020
ms.author: marsma
ms.reviewer: ''
ms.custom: aaddev
ms.openlocfilehash: 91a55520b37c549c8f1d94ba6cf08ecd24db85b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262457"
---
# <a name="how-to-configure-sso-on-macos-and-ios"></a>Nasıl yapılandırılır: sso'nun macOS ve iOS'ta yapılandırılMası

macOS ve iOS için Microsoft Kimlik Doğrulama Kitaplığı (MSAL), macOS/iOS uygulamaları ve tarayıcılar arasında Tek Oturum Açma (SSO) destekler. Bu makale, aşağıdaki SSO senaryolarını kapsar:

- [Birden fazla uygulama arasında sessiz SSO](#silent-sso-between-apps)

Bu tür SSO, aynı Apple Geliştiricisi tarafından dağıtılan birden çok uygulama arasında çalışır. Anahtarlıktan diğer uygulamalar tarafından yazılmış yenileme belirteçlerini okuyarak ve bunları sessizce erişim belirteçleri için değiş tokuş ederek sessiz SSO (diğer bir deyişle, kullanıcı kimlik bilgileri için istenmez) sağlar.  

- [Kimlik Doğrulama aracısı aracılığıyla SSO](#sso-through-authentication-broker-on-ios)

> [!IMPORTANT]
> Bu akış macOS'ta kullanılamaz.

Microsoft, mobil aygıt Azure Active Directory'ye (AAD) kayıtlı olduğu sürece farklı satıcılardan gelen uygulamalar arasında SSO'ya olanak tanıyan broker adı verilen uygulamalar sağlar. Bu tür SSO, kullanıcının cihazına bir broker uygulamasının yüklenmesini gerektirir.

- **MSAL ve Safari arasında SSO**

[SSO, ASWebAuthenticationSession](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession?language=objc) sınıfı aracılığıyla elde edilir. Diğer uygulamalardan ve Safari tarayıcısından varolan oturum açma durumunu kullanır. Aynı Apple Developer tarafından dağıtılan uygulamalarla sınırlı değildir, ancak bazı kullanıcı etkileşimi gerektirir.

Kullanıcıları oturum etmek için uygulamanızdaki varsayılan web görünümünü kullanırsanız, MSAL tabanlı uygulamalar ve Safari arasında otomatik SSO alırsınız. MSAL'ın desteklediği web görünümleri hakkında daha fazla bilgi edinmek için [tarayıcıları ve Web Görünümlerini Özelleştir'i ziyaret edin.](customize-webviews.md)

> [!IMPORTANT]
> Bu tür SSO şu anda macOS'ta kullanılamıyor. macOS'taki MSAL yalnızca Safari ile SSO desteği olmayan WKWebView'ı destekler. 

- **ADAL ve MSAL macOS/iOS uygulamaları arasında sessiz SSO**

MSAL Objective-C, ADAL Objective-C tabanlı uygulamalarla göçü ve SSO'yı destekler. Uygulamaların aynı Apple Geliştiricisi tarafından dağıtılması gerekir.

ADAL ve MSAL tabanlı uygulamalar arasında çapraz uygulama SSO talimatları için [macOS ve iOS'taki ADAL ve MSAL uygulamaları arasındaki SSO'ya](sso-between-adal-msal-apps-macos-ios.md) bakın.

## <a name="silent-sso-between-apps"></a>Uygulamalar arasında sessiz SSO

MSAL, iOS anahtarlık erişim grupları aracılığıyla SSO paylaşımını destekler.

Uygulamalarınızda SSO'yu etkinleştirmek için, aşağıda daha ayrıntılı olarak açıklanan aşağıdaki adımları yapmanız gerekir:

1. Tüm uygulamalarınızın aynı İstemci Kimliği veya Uygulama Kimliği'ni kullandığından emin olun.
1. Anahtarlıkları paylaşabilmeniz için tüm uygulamalarınızın Apple'dan aynı imza sertifikasını paylaştığından emin olun.
1. Her uygulamanız için aynı anahtarlık hakkını isteyin.
1. MSAL SDK'lara varsayılan anahtardan farklıysa kullanmamızı istediğiniz paylaşılan anahtarzinciri nden bahsedin.

### <a name="use-the-same-client-id-and-application-id"></a>Aynı İstemci Kimliği ve Uygulama Kimliğini Kullanma

Microsoft kimlik platformunun hangi uygulamaların jetonları paylaşabileceğini bilmesi için, bu uygulamaların aynı İstemci Kimliğini veya Uygulama Kimliğini paylaşması gerekir. Bu, portala ilk uygulamanızı kaydettiğinizde size sağlanan benzersiz tanımlayıcıdır.

Microsoft kimlik platformunun, aynı Uygulama Kimliğini ayrı kullanan uygulamalara yönlendirme **uri'leri**tarafından anlatılıyor. Her uygulama, biniş portalında kayıtlı birden çok Yönlendirme URIs'ine sahip olabilir. Süitinizdeki her uygulama farklı bir yeniden yönlendirme URI olacaktır. Örnek:

App1 Uri Yönlendirme:`msauth.com.contoso.mytestapp1://auth`  
App2 Redirect URI:`msauth.com.contoso.mytestapp2://auth`  
App3 Redirect URI:`msauth.com.contoso.mytestapp3://auth`  

> [!IMPORTANT]
> Yeniden yönlendirme uris biçimi MSAL destekler biçimi ile uyumlu olmalıdır, [Hangi MSAL Yönlendirme URI biçimi gereksinimleri](redirect-uris-ios.md#msal-redirect-uri-format-requirements)belgelenmiştir.

### <a name="setup-keychain-sharing-between-applications"></a>Uygulamalar arasında anahtarlık paylaşımı

Anahtarlık paylaşımını etkinleştirmek için Apple'ın [Ekleme Özellikleri](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html) makalesine bakın. Önemli olan, anahtarzincirinizin ne olarak adlandırılmasını istediğinize karar vermeniz ve bu yeteneği SSO'da yer alacak tüm uygulamalarınız için eklemenizdir.

Yetkilendirmeleri doğru ayarladığınızda, proje dizininizde aşağıdaki `entitlements.plist` gibi bir örnek içeren bir dosya görürsünüz:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

#### <a name="add-a-new-keychain-group"></a>Yeni bir anahtarlık grubu ekleme

Proje **Yeteneklerinize**yeni bir anahtar zinciri grubu ekleyin. Anahtarlık grubu:
* `com.microsoft.adalcache`iOS üzerinde 
* `com.microsoft.identity.universalstorage`macOS'ta.

![anahtarlık örneği](media/single-sign-on-macos-ios/keychain-example.png)

Daha fazla bilgi için [anahtarlık gruplarına](howto-v2-keychain-objc.md)bakın.

## <a name="configure-the-application-object"></a>Uygulama nesnesini yapılandırma

Her uygulamanızda anahtarlık yetkilendirmesini etkinleştirdikten ve SSO'yu kullanmaya hazır olduğunuzda, `MSALPublicClientApplication` aşağıdaki örnekte olduğu gibi anahtarzinciri erişim grubunuzla yapılandırın:

Amaç-C:

```objc
NSError *error = nil;
MSALPublicClientApplicationConfig *configuration = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<my-client-id>"];
configuration.cacheConfig.keychainSharingGroup = @"my.keychain.group";
    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:configuration error:&error];
```

Swift:

```swift
let config = MSALPublicClientApplicationConfig(clientId: "<my-client-id>")
config.cacheConfig.keychainSharingGroup = "my.keychain.group"

do {
   let application = try MSALPublicClientApplication(configuration: config)
  // continue on with application
} catch let error as NSError {
  // handle error here
}
```

> [!WARNING]
> Uygulamalarınız arasında bir anahtar zinciri paylaştığınızda, herhangi bir uygulama kullanıcıları ve hatta uygulamanızdaki belirteçlerin tümünü silebilir.
> Arka plan çalışması yapmak için belirteçlere dayanan uygulamalarınız varsa, bu özellikle etkili dir.
> Anahtar zinciri paylaşmak, uygulamanız Microsoft kimlik SDK kaldırma işlemlerini kullanırken çok dikkatli olmalısınız anlamına gelir.

İşte bu kadar! Microsoft kimliği SDK artık tüm uygulamalarınızda kimlik bilgilerini paylaşır. Hesap listesi uygulama örnekleri arasında da paylaşılacaktır.

## <a name="sso-through-authentication-broker-on-ios"></a>Ios'ta Kimlik Doğrulama aracısı aracılığıyla SSO

MSAL, Microsoft Authenticator ile aracılı kimlik doğrulama desteği sağlar. Microsoft Authenticator, AAD'ye kayıtlı aygıtlar için SSO sağlar ve uygulamanızın Koşullu Erişim ilkelerini izlemesine yardımcı olur.

Uygulamanız için bir kimlik doğrulama aracısı kullanarak SSO'yu nasıl etkinleştirdiğiniz aşağıda veda edilebilen adımlar şunlardır:

1. Uygulamaiçin uygulama için aracı uyumlu bir Yönlendirme URI biçimini uygulamanızın Info.plist'ine kaydedin. Broker uyumlu Redirect URI `msauth.<app.bundle.id>://auth`biçimidir. '<app.bundle.id>'' ile başvurunuzun paket kimliğini değiştirin. Örnek:

    ```xml
    <key>CFBundleURLSchemes</key>
    <array>
        <string>msauth.<app.bundle.id></string>
    </array>
    ```

1. Uygulamanızın Info.plist aşağıdaki `LSApplicationQueriesSchemes`şemaları ekleyin:

    ```xml
    <key>LSApplicationQueriesSchemes</key>
    <array>
         <string>msauthv2</string>
         <string>msauthv3</string>
    </array>
    ```

1. Geri aramaları işlemek `AppDelegate.m` için dosyanıza aşağıdakileri ekleyin:

    Amaç-C:
    
    ```objc
    - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options
    {
        return [MSALPublicClientApplication handleMSALResponse:url sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
    }
    ```
    
    Swift:
    
    ```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
    }
    ```
    
**Xcode 11 kullanıyorsanız,** bunun yerine dosyaya `SceneDelegate` MSAL geri arama yerleştirmelisiniz.
Eski iOS ile uyumluluk için hem UISceneDelegate'ı hem de UIApplicationDelegate'i destekliyorsanız, MSAL geri aramanın her iki dosyaya da yerleştirilmesi gerekir.

Amaç-C:

```objc
 - (void)scene:(UIScene *)scene openURLContexts:(NSSet<UIOpenURLContext *> *)URLContexts
 {
     UIOpenURLContext *context = URLContexts.anyObject;
     NSURL *url = context.URL;
     NSString *sourceApplication = context.options.sourceApplication;
     
     [MSALPublicClientApplication handleMSALResponse:url sourceApplication:sourceApplication];
 }
```

Swift:

```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        
        guard let urlContext = URLContexts.first else {
            return
        }
        
        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication
        
        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
```

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin