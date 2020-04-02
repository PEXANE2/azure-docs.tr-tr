---
title: iOS aygıtları için paylaşılan aygıt modu
titleSuffix: Microsoft identity platform | Azure
description: Firstline Çalışanlarının bir iOS aygıtını paylaşmasına izin vermek için paylaşılan aygıt modunu nasıl etkinleştireceğimiz öğrenin
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/31/2020
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 7cecbc48eb362c2c0f1741352e6f7f5f6ad40c9e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550259"
---
# <a name="shared-device-mode-for-ios-devices"></a>iOS aygıtları için paylaşılan aygıt modu

> [!NOTE]
> Bu özellik genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Perakende çalışanları, uçuş ekibi üyeleri ve saha hizmeti çalışanları gibi Firstline Çalışanları genellikle işlerini gerçekleştirmek için paylaşılan bir mobil cihaz kullanırlar. Bu paylaşılan aygıtlar, kullanıcılarınızın paylaşılan cihazdaki müşteri ve iş verilerine erişmek için parolalarını veya PIN'lerini kasıtlı olarak veya kasıtlı olarak paylaşmaması durumunda güvenlik riskleri doğurabilir.

Paylaşılan aygıt modu, iOS 13 veya daha yüksek bir aygıtı çalışanlar tarafından daha kolay ve güvenli bir şekilde paylaşılacak şekilde yapılandırmanıza olanak tanır. Çalışanlar oturum açabilir ve müşteri bilgilerine hızlı bir şekilde erişebilir. Vardiyaları veya görevleri bittiğinde, aygıtın dışına çıkış yapabilir ve bir sonraki çalışan tarafından kullanıma hemen hazır hale getirilebilir.

Paylaşılan aygıt modu, aygıtın Microsoft kimlik destekli yönetimini de sağlar.

Bu özellik, aygıttaki kullanıcıları yönetmek ve [Apple aygıtları için Microsoft Enterprise SSO eklentisini](apple-sso-plugin.md)dağıtmak için [Microsoft Authenticator uygulamasını](../user-help/user-help-auth-app-overview.md) kullanır.

## <a name="create-a-shared-device-mode-app"></a>Paylaşılan bir aygıt modu uygulaması oluşturma

Paylaşılan bir aygıt modu uygulaması oluşturmak için geliştiriciler ve bulut aygıt yöneticileri birlikte çalışır:

1. **Uygulama geliştiricileri** tek hesaplı bir uygulama (paylaşılan aygıt modunda birden fazla hesaplı uygulamalar desteklenmez) ve paylaşılan aygıt oturum açma gibi şeyleri işlemek için kod yazar.

1. **Aygıt yöneticileri,** kuruluşlarındaki aygıtları yönetmek için Microsoft Intune gibi bir mobil aygıt yönetimi (MDM) sağlayıcısı kullanarak aygıtı paylaşılacak şekilde hazırlar. MDM, Microsoft Authenticator uygulamasını aygıtlara iter ve cihaza yönelik bir profil güncelleştirmesi aracılığıyla her aygıt için "Paylaşılan Mod"u açar. Bu Paylaşılan Mod ayarı, aygıttaki desteklenen uygulamaların davranışını değiştiren şeydir. MDM sağlayıcısının bu yapılandırması, aygıt için paylaşılan aygıt modunu ayarlar ve paylaşılan aygıt modu için gerekli olan [Apple aygıtları için Microsoft Enterprise SSO eklentisini](apple-sso-plugin.md) etkinleştirir.

1. [**Yalnızca Genel Önizleme sırasında gereklidir**] [Bulut Aygıt Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#cloud-device-administrator) rolüne sahip bir kullanıcının Microsoft [Authenticator uygulamasını](../user-help/user-help-auth-app-overview.md) başlatması ve aygıtını kuruluşa katılması gerekir.

    Azure portalındaki kuruluş rollerinizin üyeliğini yapılandırmak için: **Azure Etkin Dizin** > **Rolleri ve Yöneticiler** > **Bulut Aygıt Yöneticisi**

Aşağıdaki bölümler, paylaşılan aygıt modunu desteklemek için uygulamanızı güncelleştirmenize yardımcı olur.

## <a name="use-intune-to-enable-shared-device-mode--sso-extension"></a>Paylaşılan aygıt modunu & SSO uzantısını etkinleştirmek için Intune'u kullanma

> [!NOTE]
> Aşağıdaki adım yalnızca genel önizleme sırasında gereklidir.

Cihazınızın paylaşılan aygıt modunu destekleyecek şekilde yapılandırılması gerekir. iOS 13+ yüklü olmalı ve MDM'ye kayıtlı olmalıdır. MDM yapılandırması da [Apple cihazlar için Microsoft Enterprise SSO eklentisi](apple-sso-plugin.md)etkinleştirmek gerekir. SSO uzantıları hakkında daha fazla bilgi edinmek için [Apple videosuna](https://developer.apple.com/videos/play/tech-talks/301/)bakın.

1. Intune Configuration Portal'da, aygıta Aşağıdaki yapılandırmayla [Apple aygıtları için Microsoft Enterprise SSO eklentisini](apple-sso-plugin.md) etkinleştirmesini söyleyin:

    - **Tür**: Yönlendirme
    - **Ekstansiyon Kimliği**: com.microsoft.azureauthenticator.ssoextension
    - **Takım Kimliği**: SGGM6D27TK
    - **URL'ler**:https://login.microsoftonline.com
    - Yapılandırmak için ek veriler:
      - Anahtar: sharedDeviceMode
      - Türü: Boolean
      - Değer: True

    Intune ile yapılandırma hakkında daha fazla bilgi için [Intune yapılandırma belgelerine](https://docs.microsoft.com/intune/configuration/ios-device-features-settings)bakın.

1. Ardından, MDM'nizi Microsoft Authenticator uygulamasını bir MDM profili üzerinden cihazınıza itecek şekilde yapılandırın.

    Paylaşılan Aygıt modunu açmak için aşağıdaki yapılandırma seçeneklerini ayarlayın:

    - Yapılandırma 1:
      - Anahtar: sharedDeviceMode
      - Türü: Boolean
      - Değer: True

## <a name="modify-your-ios-application-to-support-shared-device-mode"></a>paylaşılan aygıt modunu desteklemek için iOS uygulamanızı değiştirme

Kullanıcılarınız, verilerinin başka bir kullanıcıya sızdırılmadığından emin olmak için size güvenir. Aşağıdaki bölümler, uygulamanıza bir değişikliğin oluştuğunu ve işlenmeli olduğunu belirtmek için yararlı sinyaller sağlar.

Uygulamanız her kullanıldığında cihazın kullanıcıdurumunu kontrol etmek ve ardından önceki kullanıcının verilerini temizlemekten siz sorumlusunuz. Bu, çoklu görev de arka plandan yeniden yüklenip yeniden yüklenmemesini içerir.

Kullanıcı değişikliğinde, hem önceki kullanıcının verilerinin temizlendiğinden hem de uygulamanızda görüntülenen önbelleğe alınmış verilerin kaldırıldığından emin olmalısınız. Paylaşılan cihaz modunu desteklemek için uygulamanızı güncelledikten sonra sizin ve şirketinizin bir güvenlik inceleme süreci yürütmenizi şiddetle öneririz.

### <a name="detect-shared-device-mode"></a>Paylaşılan aygıt modunu algılama

Paylaşılan aygıt modunu algılamak uygulamanız için önemlidir. Birçok uygulama, uygulama paylaşılan bir aygıtta kullanıldığında kullanıcı deneyiminde (UX) bir değişiklik gerektirir. Örneğin, uygulamanızda firstline çalışanı için uygun olmayan bir "Kaydol" özelliği olabilir, çünkü büyük olasılıkla zaten bir hesabı vardır. Ayrıca, uygulamanızın paylaşılan aygıt modundaysa veri işlemesine ek güvenlik eklemek isteyebilirsiniz.

Bir `getDeviceInformationWithParameters:completionBlock:` uygulamanın paylaşılan `MSALPublicClientApplication` aygıt modunda bir aygıtta mı çalıştığını belirlemek için API'yi kullanın.

Aşağıdaki kod parçacıkları `getDeviceInformationWithParameters:completionBlock:` API kullanma örneklerini gösterir.

#### <a name="swift"></a>Swift

```swift
application.getDeviceInformation(with: nil, completionBlock: { (deviceInformation, error) in

    guard let deviceInfo = deviceInformation else {
        return
    }

    let isSharedDevice = deviceInfo.deviceMode == .shared
    // Change your app UX if needed
})
```

#### <a name="objective-c"></a>Objective-C

```objective-c
[application getDeviceInformationWithParameters:nil
                                completionBlock:^(MSALDeviceInformation * _Nullable deviceInformation, NSError * _Nullable error)
{
    if (!deviceInformation)
    {
        return;
    }

    BOOL isSharedDevice = deviceInformation.deviceMode == MSALDeviceModeShared;
    // Change your app UX if needed
}];
```

### <a name="get-the-signed-in-user-and-determine-if-a-user-has-changed-on-the-device"></a>Oturum açmış kullanıcıyı alın ve kullanıcının cihazda değişip değişmediğini belirleyin

Paylaşılan aygıt modunu desteklemenin bir diğer önemli parçası da, kullanıcının değişmesi veya cihazda hiç kullanıcı yoksa uygulama verilerini temizlemek ve aygıtın kullanıcı durumunu belirlemektir. Verilerin başka bir kullanıcıya sızdırılmadığından emin olmak sizin sorumluluğunuzdadır.

Aygıtta `getCurrentAccountWithParameters:completionBlock:` şu anda oturum açmış hesabı sorgulamak için API'yi kullanabilirsiniz.

#### <a name="swift"></a>Swift

```swift
let msalParameters = MSALParameters()
msalParameters.completionBlockQueue = DispatchQueue.main

application.getCurrentAccount(with: msalParameters, completionBlock: { (currentAccount, previousAccount, error) in

    // currentAccount is the currently signed in account
    // previousAccount is the previously signed in account if any
})
```

#### <a name="objective-c"></a>Objective-C

```objective-c
MSALParameters *parameters = [MSALParameters new];
parameters.completionBlockQueue = dispatch_get_main_queue();

[application getCurrentAccountWithParameters:parameters
                             completionBlock:^(MSALAccount * _Nullable account, MSALAccount * _Nullable previousAccount, NSError * _Nullable error)
{
    // currentAccount is the currently signed in account
    // previousAccount is the previously signed in account if any
}];
```

### <a name="globally-sign-in-a-user"></a>Genel olarak oturum aç

Bir aygıt paylaşılan bir aygıt olarak yapılandırıldığınızda, `acquireTokenWithParameters:completionBlock:` uygulamanız api'yi hesapta oturum açmaları için arayabilir. Hesap, hesaptaki ilk uygulama işaretlerini imzaladıktan sonra cihazdaki tüm uygun uygulamalar için tüm dünyada kullanılabilir olacaktır.

#### <a name="objective-c"></a>Objective-C

```objective-c
MSALInteractiveTokenParameters *parameters = [[MSALInteractiveTokenParameters alloc] initWithScopes:@[@"api://myapi/scope"] webviewParameters:[self msalTestWebViewParameters]];

parameters.loginHint = self.loginHintTextField.text;

[application acquireTokenWithParameters:parameters completionBlock:completionBlock];
```

### <a name="globally-sign-out-a-user"></a>Genel olarak bir kullanıcıyı oturum aç

Aşağıdaki kod, oturum açmış hesabı kaldırır ve önbelleğe alınmış belirteçleri yalnızca uygulamadan değil, paylaşılan aygıt modundaki aygıttan da temizler. Ancak, uygulamanızdaki *verileri* temizlemez. Uygulamanızdaki verileri temizlemeniz ve uygulamanızın kullanıcıya görüntülenebilecek önbelleğe alınmış verileri temizlemeniz gerekir.

#### <a name="clear-browser-state"></a>Tarayıcı durumunu temizle

> [!NOTE]
> Aşağıdaki adım yalnızca genel önizleme sırasında gereklidir.

Bu genel önizleme sürümünde, [Apple aygıtları için Microsoft Enterprise SSO eklentisi](apple-sso-plugin.md) yalnızca uygulamalar için durumu temizler. Safari tarayıcısında durumu açıklamıyor. Kullanıcı durumunun iz bırakmadığından emin olmak için tarayıcı oturumunu el ile temizlemenizi öneririz. Çerezleri temizlemek `signoutFromBrowser` için aşağıda gösterilen isteğe bağlı özelliği kullanabilirsiniz. Bu, tarayıcının cihazda kısa bir süre başlatılmasına neden olur.

#### <a name="swift"></a>Swift

```swift
let account = .... /* account retrieved above */

let signoutParameters = MSALSignoutParameters(webviewParameters: self.webViewParamaters!)
signoutParameters.signoutFromBrowser = true // Only needed for Public Preview.

application.signout(with: account, signoutParameters: signoutParameters, completionBlock: {(success, error) in

    if let error = error {
        // Signout failed
        return
    }

    // Sign out completed successfully
})
```

#### <a name="objective-c"></a>Objective-C

```objective-c
MSALAccount *account = ... /* account retrieved above */;

MSALSignoutParameters *signoutParameters = [[MSALSignoutParameters alloc] initWithWebviewParameters:webViewParameters];
signoutParameters.signoutFromBrowser = YES; // Only needed for Public Preview.

[application signoutWithAccount:account signoutParameters:signoutParameters completionBlock:^(BOOL success, NSError * _Nullable error)
{
    if (!success)
    {
        // Signout failed
        return;
    }

    // Sign out completed successfully
}];
```

## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan aygıt modunu iş başında görmek için, GitHub'daki aşağıdaki kod örneği, paylaşılan aygıt modunda bir iOS aygıtında Firstline Worker uygulamasını çalıştırmanın bir örneğini içerir:

[MSAL iOS Swift Microsoft Graph API Örneği](https://github.com/Azure-Samples/ms-identity-mobile-apple-swift-objc)
