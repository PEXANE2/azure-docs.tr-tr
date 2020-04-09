---
title: MSAL (iOS/macOS) ile yönlendirme URI'lerini kullanma | Azure
titleSuffix: Microsoft identity platform
description: ObjectiveC için Microsoft Kimlik Doğrulama Kitaplığı (iOS ve macOS için MSAL) ile ObjectiveC için Azure AD Kimlik Doğrulama Kitaplığı (ADAL) arasındaki farklar hakkında bilgi edinin. ObjC) ve nasıl aralarında göç etmek.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: overview
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: jak
ms.custom: aaddev
ms.openlocfilehash: 1291563a39e3cf3acd4b343302be8b150bf794ca
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883517"
---
# <a name="using-redirect-uris-with-the-microsoft-authentication-library-for-ios-and-macos"></a>iOS ve macOS için Microsoft kimlik doğrulama kitaplığıyla yeniden yönlendirme URL'lerini kullanma

Bir kullanıcı kimlik doğrulaması yaptığında, Azure Etkin Dizin (Azure AD) Azure AD uygulamasına kayıtlı uri yönlendirmeyi kullanarak belirteci uygulamaya gönderir.

Microsoft Kimlik Doğrulama kitaplığı (MSAL), yeniden yönlendirme URI'sinin Azure AD uygulamasına belirli bir biçimde kaydolmasını gerektirir. MSAL, belirtmezseniz, varsayılan olarak URI'yi yeniden yönlendirme kullanır. Biçim şöyledir: `msauth.[Your_Bundle_Id]://auth`.

Varsayılan yeniden yönlendirme URI biçimi, aracılı kimlik doğrulama ve sistem web görünümü de dahil olmak üzere çoğu uygulama ve senaryo için çalışır. Mümkün olduğunda varsayılan biçimi kullanın.

Ancak, aşağıda açıklandığı gibi, gelişmiş senaryolar için yeniden yönlendirme URI değiştirmeniz gerekebilir.

## <a name="scenarios-that-require-a-different-redirect-uri"></a>Farklı bir yeniden yönlendirme URI gerektiren senaryolar

### <a name="cross-app-single-sign-on-sso"></a>Çapraz uygulama tek oturum açma (SSO)

Microsoft Identity platformunun uygulamalar arasında belirteçleri paylaşabilmesi için her uygulamanın aynı istemci kimliğine veya uygulama kimliğine sahip olması gerekir. Bu, uygulamanızı portala kaydettiğinizde sağlanan benzersiz tanımlayıcıdır (Uygulama başına Apple'a kaydettiğiniz uygulama paketi kimliği değil).

Yönlendirme URI'leri her iOS uygulaması için farklı olmalıdır. Bu, Microsoft kimlik hizmetinin bir uygulama kimliğini paylaşan farklı uygulamaları benzersiz olarak tanımlamasına olanak tanır. Her uygulamanın Azure portalında kayıtlı birden çok yeniden yönlendirme URL'si olabilir. Süitinizdeki her uygulama farklı bir yeniden yönlendirme URI olacaktır. Örneğin:

Azure portalında aşağıdaki uygulama kaydı göz önüne alındığında:

    Client ID: ABCDE-12345 (this is a single client ID)
    RedirectUris: msauth.com.contoso.app1://auth, msauth.com.contoso.app2://auth, msauth.com.contoso.app3://auth

App1, App2'yi yeniden yönlendirme `msauth.com.contoso.app1://auth` kullanır `msauth.com.contoso.app2://auth` App3 kullanır`msauth.com.contoso.app1://auth`

### <a name="migrating-from-adal-to-msal"></a>ADAL'dan MSAL'a göç

Azure AD Kimlik Doğrulama Kitaplığı'nı (ADAL) kullanan kodu MSAL'a geçirdiğinizde, uygulamanız için zaten bir yeniden yönlendirme URI yapılandırılmış olabilir. ADAL uygulamanız aracılı senaryoları destekleyecek şekilde yapılandırıldığı ve yeniden yönlendirme URI'niz MSAL yeniden yönlendirme URI biçimi gereksinimlerini karşıladığı sürece aynı yönlendirme URI'yi kullanmaya devam edebilirsiniz.

## <a name="msal-redirect-uri-format-requirements"></a>MSAL, URI biçim gereksinimlerini yeniden yönlendirin

* MSAL yönlendirme URI şeklinde olmalıdır`<scheme>://host`

    Uygulamanızı tanımlayan benzersiz bir dize nerededir. `<scheme>` Öncelikle benzersizliği garanti etmek için uygulamanızın Paket Tanımlayıcısı'na dayanır. Örneğin, uygulamanızın Paket Kimliği `com.contoso.myapp`ise, yeniden yönlendirme URI'niz şu `msauth.com.contoso.myapp://auth`şekilde olacaktır: .

    ADAL'den geçiş yapıyorsunuzsa, yeniden yönlendirmeNIZ URI `<scheme>://[Your_Bundle_Id]`büyük `scheme` olasılıkla bu biçime sahip olacaktır: , benzersiz bir dize nerededir. MSAL kullandığınızda bu biçim çalışmaya devam edecektir.

* `<scheme>`uygulamanızın Info.plist'ine kaydolmalıdır. `CFBundleURLTypes > CFBundleURLSchemes`  Bu örnekte, Info.plist kaynak kodu olarak açılmıştır:

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>msauth.[BUNDLE_ID]</string>
            </array>
        </dict>
    </array>
    ```
    

MSAL, yeniden yönlendirmeNIZIN URI'nin doğru kaydolup olmadığını doğrular ve değilse bir hata döndürür.
    
* Evrensel bağlantıları yeniden yönlendirme URI olarak kullanmak `<scheme>` istiyorsanız, `https` 'de beyan edilmesi gerekir `CFBundleURLSchemes`ve gerekmez. Bunun yerine, uygulamayı ve etki alanını Geliştiriciler için Evrensel Bağlantılar'da `MSALPublicClientApplication` Apple'ın [talimatlarına](https://developer.apple.com/ios/universal-links/) göre yapılandırın ve uygulamanızın evrensel bir bağlantı aracılığıyla ne zaman `handleMSALResponse:sourceApplication:` açıldığının yöntemini arayın.

## <a name="use-a-custom-redirect-uri"></a>Özel yönlendirme URI kullanın

Özel bir yeniden yönlendirme URI `redirectUri` kullanmak için, `MSALPublicClientApplicationConfig` parametreyi `MSALPublicClientApplication` geçirin ve nesneyi başharfe aldığınızda bu nesneyi geçirin. Yeniden yönlendirme URI geçersizse, baş harfdöndürür `nil` `redirectURIError`ve ek bilgilerle birlikte ayarlar.  Örneğin:

Amaç-C:

```objc
MSALPublicClientApplicationConfig *config =
        [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"your-client-id"
                                                        redirectUri:@"your-redirect-uri"
                                                        authority:authority];
NSError *redirectURIError;
MSALPublicClientApplication *application =
        [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&redirectURIError];
```

Swift:

```swift
let config = MSALPublicClientApplicationConfig(clientId: "your-client-id",
                                            redirectUri: "your-redirect-uri",
                                              authority: authority)
do {
  let application = try MSALPublicClientApplication(configuration: config)
  // continue on with application          
} catch let error as NSError {
  // handle error here
}       
```



## <a name="handle-the-url-opened-event"></a>AÇıLAN URL olayını işleme

Uygulamanız, URL şemaları veya evrensel bağlantılar aracılığıyla herhangi bir yanıt aldığında MSAL'ı aramalıdır. Uygulamanızın `handleMSALResponse:sourceApplication:` `MSALPublicClientApplication` ne zaman açıldığını n için arayın. Özel düzenler için bir örnek aşağıda verilmiştir:

Amaç-C:

```objc
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
    return [MSALPublicClientApplication handleMSALResponse:url 
                                         sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

Swift:

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}
```



## <a name="next-steps"></a>Sonraki adımlar

[Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin
