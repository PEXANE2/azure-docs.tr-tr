---
title: Tarayıcıları Web Görünümleri & özelleştirin (MSAL iOS/macOS) | Azure
titleSuffix: Microsoft identity platform
description: Kullanıcıları oturum etmek için MSAL iOS/macOS tarayıcı deneyimini nasıl özelleştireceğimiz öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 3b4362e4c5e69efddfbc99ef0f98ad3c5966165c
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81450893"
---
# <a name="how-to-customize-browsers-and-webviews-for-iosmacos"></a>Nasıl yapar: iOS/macOS için tarayıcıları ve Web Görünümlerini özelleştirin

Etkileşimli kimlik doğrulaması için bir web tarayıcısı gereklidir. iOS ve macOS 10.15+'da, Microsoft Kimlik Doğrulama Kitaplığı (MSAL), kullanıcıları oturum açmaları için etkileşimli kimlik doğrulaması yapmak için varsayılan olarak sistem web tarayıcısını (uygulamanızın üstünde görünebilir) kullanır. Sistem tarayıcısını kullanmak, Tek İşaret Açma (SSO) durumunu diğer uygulamalarla ve web uygulamalarıyla paylaşma avantajına sahiptir.

Yapılandırmayı web içeriğini görüntülemek için diğer seçeneklerle özelleştirerek deneyimi değiştirebilirsiniz:

Yalnızca iOS için:

- [SFAuthentication Oturumu](https://developer.apple.com/documentation/safariservices/sfauthenticationsession?language=objc) 
- [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?language=objc)

iOS ve macOS için:

- [ASWeb AuthenticationSession](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession?language=objc)
- [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview?language=objc).

macOS için MSAL `WKWebView` yalnızca eski işletim sistemi sürümlerinde destekler. `ASWebAuthenticationSession`yalnızca macOS 10.15 ve üzeri desteklenir. 

## <a name="system-browsers"></a>Sistem tarayıcıları

iOS `ASWebAuthenticationSession`için, `SFAuthenticationSession`ve `SFSafariViewController` sistem tarayıcıları olarak kabul edilir. macOS için `ASWebAuthenticationSession` yalnızca kullanılabilir. Genel olarak, sistem tarayıcıları çerezleri ve diğer web sitesi verilerini Safari tarayıcı uygulamasıyla paylaşır.

Varsayılan olarak, MSAL iOS sürümünü dinamik olarak algılar ve bu sürümde bulunan önerilen sistem tarayıcısını seçer. iOS 12+'da `ASWebAuthenticationSession`olacak. 

### <a name="default-configuration-for-ios"></a>iOS için varsayılan yapılandırma

| Sürüm | Web tarayıcısı |
|:-------------:|:-------------:|
| iOS 12+ | ASWeb AuthenticationSession |
| iOS 11 | SFAuthentication Oturumu |
| iOS 10 | SFSafariViewController |

### <a name="default-configuration-for-macos"></a>macOS için varsayılan yapılandırma

| Sürüm | Web tarayıcısı |
|:-------------:|:-------------:|
| macOS 10.15+ | ASWeb AuthenticationSession |
| diğer sürümler | WKWebView |

Geliştiriciler ayrıca MSAL uygulamaları için farklı bir sistem tarayıcısı seçebilir:

- `SFAuthenticationSession`iOS 11 sürümü. `ASWebAuthenticationSession`
- `SFSafariViewController`daha genel bir amaçtır ve web'de gezinmek için bir arayüz sağlar ve oturum açma amacıyla da kullanılabilir. iOS 9 ve 10'da çerezler ve diğer web sitesi verileri Safari ile paylaşılır, ancak iOS 11 ve sonraki yerlerde paylaşılmaz.

## <a name="in-app-browser"></a>Uygulama içi tarayıcı

[WKWebView,](https://developer.apple.com/documentation/webkit/wkwebview) web içeriğini görüntüleyen bir uygulama içi tarayıcıdır. Çerezleri veya web sitesi verilerini diğer **WKWebView** örnekleriyle veya Safari tarayıcısıyla paylaşmaz. WKWebView, hem iOS hem de macOS için kullanılabilen bir çapraz platform tarayıcısıdır.

## <a name="cookie-sharing-and-single-sign-on-sso-implications"></a>Çerez paylaşımı ve Tek oturum açma (SSO) etkileri

Kullandığınız tarayıcı, çerezleri paylaşma şekli nedeniyle SSO deneyimini etkiler. Aşağıdaki tablolar tarayıcı başına SSO deneyimlerini özetler.

| Teknoloji    | Tarayıcı Türü  | iOS kullanılabilirliği | macOS kullanılabilirliği | Çerezleri ve diğer verileri paylaşır  | MSAL kullanılabilirliği | SSO |
|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|-------------:|
| [ASWeb AuthenticationSession](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession) | Sistem | iOS12 ve yukarısı | macOS 10.15 ve yukarısı | Evet | iOS ve macOS 10.15+ | w/ Safari örnekleri
| [SFAuthentication Oturumu](https://developer.apple.com/documentation/safariservices/sfauthenticationsession) | Sistem | iOS11 ve yukarısı | Yok | Evet | Yalnızca iOS |  w/ Safari örnekleri
| [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) | Sistem | iOS11 ve yukarısı | Yok | Hayır | Yalnızca iOS | Hayır**
| **SFSafariViewController** | Sistem | iOS10 | Yok | Evet | Yalnızca iOS |  w/ Safari örnekleri
| **WKWebView**  | Uygulama içi | iOS8 ve yukarı | macOS 10.10 ve yukarı | Hayır | iOS ve macOS | Hayır**

** SSO'nun çalışması için belirteçlerin uygulamalar arasında paylaşılması gerekir. Bu, iOS için Microsoft Authenticator gibi bir belirteç önbelleği veya aracı uygulaması gerektirir.

## <a name="change-the-default-browser-for-the-request"></a>İstek için varsayılan tarayıcıyı değiştirme

Aşağıdaki özelliği değiştirerek, UX gereksinimlerinize bağlı olarak bir uygulama içi tarayıcı veya `MSALWebviewParameters`belirli bir sistem tarayıcısı kullanabilirsiniz:

```objc
@property (nonatomic) MSALWebviewType webviewType;
```

## <a name="change-per-interactive-request"></a>Etkileşimli istek başına değişiklik

Her `acquireTokenWithParameters:completionBlock:` istek, API'ye geçmeden önce özelliği `MSALInteractiveTokenParameters.webviewParameters.webviewType` değiştirerek varsayılan tarayıcıyı geçersiz kılacak şekilde yapılandırılabilir.

Ayrıca, MSAL `WKWebView` `MSALInteractiveTokenParameters.webviewParameters.customWebView` özelliği ayarlayarak özel bir geçen destekler.

Örneğin:

Objective-C
```objc
UIViewController *myParentController = ...;
WKWebView *myCustomWebView = ...;
MSALWebviewParameters *webViewParameters = [[MSALWebviewParameters alloc] initWithAuthPresentationViewController:myParentController];
webViewParameters.webviewType = MSALWebviewTypeWKWebView;
webViewParameters.customWebview = myCustomWebView;
MSALInteractiveTokenParameters *interactiveParameters = [[MSALInteractiveTokenParameters alloc] initWithScopes:@[@"myscope"] webviewParameters:webViewParameters];
    
[app acquireTokenWithParameters:interactiveParameters completionBlock:completionBlock];
```
Swift
```swift
let myParentController: UIViewController = ...
let myCustomWebView: WKWebView = ...
let webViewParameters = MSALWebviewParameters(authPresentationViewController: myParentController)
webViewParameters.webviewType = MSALWebviewType.wkWebView
webViewParameters.customWebview = myCustomWebView
let interactiveParameters = MSALInteractiveTokenParameters(scopes: ["myscope"], webviewParameters: webViewParameters)

app.acquireToken(with: interactiveParameters, completionBlock: completionBlock)
```

Özel bir web görünümü kullanıyorsanız, bildirimler görüntülenen web içeriğinin durumunu belirtmek için kullanılır:

```objc
/*! Fired at the start of a resource load in the webview. The URL of the load, if available, will be in the @"url" key in the userInfo dictionary */
extern NSString *MSALWebAuthDidStartLoadNotification;

/*! Fired when a resource finishes loading in the webview. */
extern NSString *MSALWebAuthDidFinishLoadNotification;

/*! Fired when web authentication fails due to reasons originating from the network. Look at the @"error" key in the userInfo dictionary for more details.*/
extern NSString *MSALWebAuthDidFailNotification;

/*! Fired when authentication finishes */
extern NSString *MSALWebAuthDidCompleteNotification;

/*! Fired before ADAL invokes the broker app */
extern NSString *MSALWebAuthWillSwitchToBrokerApp;
```

### <a name="options"></a>Seçenekler

MSAL desteklenen tüm web tarayıcısı türleri [MSALWebviewType enum'da](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALDefinitions.h#L47) bildirilir

```objc
typedef NS_ENUM(NSInteger, MSALWebviewType)
{
    /**
     For iOS 11 and up, uses AuthenticationSession (ASWebAuthenticationSession or SFAuthenticationSession).
     For older versions, with AuthenticationSession not being available, uses SafariViewController.
     For macOS 10.15 and above uses ASWebAuthenticationSession
     For older macOS versions uses WKWebView
     */
    MSALWebviewTypeDefault,
    
    /** Use ASWebAuthenticationSession where available.
     On older iOS versions uses SFAuthenticationSession
     Doesn't allow any other webview type, so if either of these are not present, fails the request*/
    MSALWebviewTypeAuthenticationSession,
    
#if TARGET_OS_IPHONE
    
    /** Use SFSafariViewController for all versions. */
    MSALWebviewTypeSafariViewController,
    
#endif
    /** Use WKWebView */
    MSALWebviewTypeWKWebView,
};
```

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin
