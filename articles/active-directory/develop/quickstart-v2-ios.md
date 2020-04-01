---
title: Microsoft kimlik platformu iOS ve macOS quickstart | Azure
description: Bir iOS veya macOS uygulamasında kullanıcıları nasıl oturum alabildiğini ve Microsoft Graph'ı nasıl sorgulayacağınızı öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/24/2019
ms.author: marsma
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:iOS
ms.openlocfilehash: 3b0d8c8aa73903572a6a310e07aae57ec71f9cb7
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80473655"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-or-macos-app"></a>Hızlı başlangıç: Kullanıcıları oturum açın ve bir iOS veya macOS uygulamasından Microsoft Graph API'yi arayın

Bu hızlı başlatma, yerel bir iOS veya macOS uygulamasının kişisel, iş ve okul hesaplarında oturum kurmak, erişim jetonunu almak ve Microsoft Graph API'yi aramak için Microsoft kimlik platformlarını nasıl kullanabileceğini gösteren bir kod örneği içerir.

Bu hızlı başlangıç hem iOS hem de macOS uygulamaları için geçerlidir. Bazı adımlar yalnızca iOS uygulamaları için gereklidir. Bu adımlar sadece iOS için olduğunu çağırır.

![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Ön koşullar**
> * XCode 10+
> * iOS 10+ 
> * macOS 10.12+

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample"></a>Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğini indirin
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek için,
> 1. Yeni Azure [portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://aka.ms/MobileAppReg) sayfası.
> 1. **Yeni kayıt**seçin.
> 1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
>      - **Ad** bölümünde, oturum açabildiklerinde veya uygulamanızı kabul ettiklerinde uygulama nın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>      - Bu sayfadaki diğer yapılandırmaları atlayın.
>      - `Register` öğesini seçin.
> 1. `Authentication`  >  `Add Platform`  >  **Yönet** `iOS`bölümünde.
>      - Başvurunuz için ***Paket Tanımlayıcısı'nı*** girin. Paket tanımlayıcısı, örneğin `com.<yourname>.identitysample.MSALMacOS`uygulamanızı benzersiz olarak tanımlayan benzersiz bir dizedir. Kullandığınız değere dikkat edin.
>      - iOS yapılandırmasının macOS uygulamaları için de geçerli olduğunu unutmayın.
> 1. `Configure` ***MSAL Yapılandırma*** ayrıntılarını bu hızlı başlangıç için seçin ve kaydedin.
> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Bu hızlı başlatmanın işe yaraması için Auth aracısıyla uyumlu bir yeniden yönlendirme URI eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-ios/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış

#### <a name="step-2-download-the-sample-project"></a>Adım 2: Örnek projeyi indirin

- [iOS için Kod Örneğini İndirin](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
- [macOS için Kod Örneğini İndirin](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

#### <a name="step-3-install-dependencies"></a>Adım 3: Bağımlılıkları yükleme

Terminal penceresinde, indirilen kod örneğinin yer verdiği klasöre gidin ve en son MSAL kitaplığını yüklemek için çalıştırın. `pod install`

#### <a name="step-4-configure-your-project"></a>Adım 4: Projenizi yapılandırın

> [!div renderon="docs"]
> Yukarıda Seçenek 1'i seçtiyseniz, şu adımları atlayabilirsiniz. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
> 1. **ViewController.swift'i** düzenle ve 'let kClientID' ile başlayan satırı aşağıdaki kod parçacığıyla değiştirin. Uygulamanızı portala `kClientID` daha önce hızlı bir başlangıç la kaydettiğinizde kaydettiğiniz istemci kimliğiyle değeri güncellemeyi unutmayın:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_Here"
>    ```
> 1. **ViewController.swift'i** düzenle ve 'let kAuthority' ile başlayan satırı aşağıdaki kod parçacığıyla değiştirin:
>    ```swift
>    let kAuthority = "Enter_the_Authority_Endpoint_Host_HereEnter_the_Tenant_Info_Here"
>    ```
> 1. **ViewController.swift'i** düzenle ve 'let kGraphEndpoint' ile başlayan satırı aşağıdaki kod parçacığıyla değiştirin:
>    ```swift
>    let kGraphEndpoint = "Enter_the_MS_Graph_Endpoint_Host_Here"
>    ```
> 1. Proje ayarlarını açın. **Kimlik** bölümünde, portala girdiğiniz **Paket Tanımlayıcısını** girin.
> 1. Yalnızca iOS için **Info.plist'e** sağ tıklayın ve > **Kaynak Kodu** **Olarak Aç'ı**seçin.
> 1. Yalnızca iOS için, dikte kök düğümü `CFBundleURLSchemes` altında, portala girdiğiniz ***Paket Kimliği*** ile değiştirin.
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
>    ```
> 1. Uygulamayı çalıştırmak & oluşturun!
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
>
> 1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
> 1. **ViewController.swift'i** düzenle ve 'let kClientID' ile başlayan satırı aşağıdaki kod parçacığıyla değiştirin. Uygulamanızı bu hızlı `kClientID` başlangıçta daha önce portala kaydettiğinizde kaydettiğiniz clientID ile değeri güncelleştirmeyi unutmayın:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_Here"
>    ```
> 1. [Azure AD ulusal bulutları](https://docs.microsoft.com/graph/deployments#app-registration-and-token-service-root-endpoints)için bir uygulama oluşturuyorsanız, 'kGraphEndpoint'e izin verin' ve 'kAuthority'yi doğru uç noktalarıyla değiştirin. Genel erişim için varsayılan değerleri kullanın:
>     ```objective-c
>     let kGraphEndpoint = "https://graph.microsoft.com/"
>     let kAuthority = "https://login.microsoftonline.com/common"
>     ```
> 1. Diğer uç noktalar [burada](https://docs.microsoft.com/graph/deployments#app-registration-and-token-service-root-endpoints)belgelenmiştir. Örneğin, Azure AD Almanya ile hızlı başlatmayı çalıştırmak için aşağıdakileri kullanın:
>     ```objective-c
>     let kGraphEndpoint = "https://graph.microsoft.de/"
>     let kAuthority = "https://login.microsoftonline.de/common"
>     ```
> 1. Proje ayarlarını açın. **Kimlik** bölümünde, portala girdiğiniz **Paket Tanımlayıcısını** girin.
> 1. Yalnızca iOS için **Info.plist'e** sağ tıklayın ve > **Kaynak Kodu** **Olarak Aç'ı**seçin.
> 1. Yalnızca iOS için, dikte kök düğümü `Enter_the_bundle_Id_Here` altında, portalda kullandığınız ***Paket Kimliği*** ile değiştirin.
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
> 
>    ```
> 1. Uygulamayı çalıştırmak & oluşturun! 

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi almak için şu bölümleri okuyun.

### <a name="get-msal"></a>MSAL alın

MSAL[(MSAL.framework),](https://github.com/AzureAD/microsoft-authentication-library-for-objc)Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan kullanıcı ve istek belirteçleri oturum açmada kullanılan kitaplıktır. Aşağıdaki işlemi uygulayarak uygulamanıza MSAL ekleyebilirsiniz:

```
$ vi Podfile

```
Bu podfile (projenizin hedefi ile) aşağıdakileri ekleyin:

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL'
end

```

CocoaPods kurulum komutunu çalıştırın:

```pod install```

### <a name="initialize-msal"></a>MSAL'ı Başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```swift
import MSAL
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```swift
let authority = try MSALAADAuthority(url: URL(string: kAuthority)!)
            
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
```

> |Konumlar: ||
> |---------|---------|
> | `clientId` | *portal.azure.com*’da kaydedilen uygulamanın Uygulama Kimliği |
> | `authority` | Microsoft kimlik platformu bitiş noktası. Çoğu durumda bu *https<span/>://login.microsoftonline.com/common* olur |
> | `redirectUri` | Uygulamanın URI yönlendirme. Varsayılan değeri kullanmak için 'nil' veya özel yeniden yönlendirme URI'yi geçebilirsiniz. |

### <a name="for-ios-only-additional-app-requirements"></a>Yalnızca iOS için ek uygulama gereksinimleri

Uygulamanız da aşağıdaki leri `AppDelegate`olmalıdır. Bu, kimlik doğrulaması yaptığınızda MSAL SDK'nın Auth broker uygulamasından gelen belirteç yanıtını işlemesini sağlar.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
    }

 ```

> [!NOTE]
> `UISceneDelegate` iOS `UIApplicationDelegate`13+'ta, bunun yerine bu kodu `scene:openURLContexts:` geri arama yerine yerleştirin [(Bkz. Apple'ın belgelerine](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)bakın).
> Eski iOS ile uyumluluk için hem UISceneDelegate'ı hem de UIApplicationDelegate'i destekliyorsanız, MSAL geri çağırmanın her iki yere de yerleştirilmesi gerekir.

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

Son olarak, uygulamanızın `LSApplicationQueriesSchemes` ***Info.plist'inde*** bir `CFBundleURLTypes`girişi olmalıdır. Örnek bu dahil ile birlikte gelir. 

   ```xml 
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>İstek belirteçleri & oturum açma

Belirteç almak için MSAL’in iki yöntemi vardır: `acquireToken` ve `acquireTokenSilent`.

#### <a name="acquiretoken-get-a-token-interactively"></a>edinmeToken: Etkileşimli bir belirteç alın

Bazı durumlarda, kullanıcıların Microsoft kimlik platformuyla etkileşimde yetişmesi gerekir. Bu gibi durumlarda, son kullanıcının hesabını seçmesi, kimlik bilgilerini girmesi veya uygulamanızın izinlerini kabul etmesi gerekebilir. Örneğin, 

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Bir kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir 
* Uygulamanız ilk kez bir kaynağa erişim istediğinde
* MFA veya diğer Koşullu Erişim ilkeleri gerektiğinde

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: self.webViewParamaters!)
self.applicationContext!.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Konumlar:||
> |---------|---------|
> | `scopes` | İstenilen kapsamları içerir (diğer bir `[ "<Application ID URL>/scope" ]` şekilde Microsoft Graph`api://<Application ID>/access_as_user` `[ "user.read" ]` için veya özel Web API'leri için ( ) |

#### <a name="acquiretokensilent-get-an-access-token-silently"></a>edinmeTokenSilent: Sessizce bir erişim belirteç alın

Uygulamalar, kullanıcılarının her jeton istediklerinde oturum açmalarını gerektirmemelidir. Kullanıcı zaten oturum açmışsa, bu yöntem uygulamaların jeton larını sessizce istemesine olanak tanır. 

```swift
self.applicationContext!.getCurrentAccount(with: nil) { (currentAccount, previousAccount, error) in
            
   guard let account = currentAccount else {
      return
   }
            
   let silentParams = MSALSilentTokenParameters(scopes: self.kScopes, account: account)
   self.applicationContext!.acquireTokenSilent(with: silentParams) { (result, error) in /* Add your handling logic */}
}
```

> |Konumlar: ||
> |---------|---------|
> | `scopes` | İstenilen kapsamları içerir (diğer bir `[ "<Application ID URL>/scope" ]` şekilde Microsoft Graph`api://<Application ID>/access_as_user` `[ "user.read" ]` için veya özel Web API'leri için ( ) |
> | `account` | Bir belirteç için istenen hesap. Bu hızlı başlatma tek bir hesap uygulaması hakkındadır. Çok hesaplı bir uygulama oluşturmak istiyorsanız, belirteç isteklerini kullanmak `accountsFromDeviceForParameters:completionBlock:` ve doğru geçmek için hangi hesabı kullanacağınızı belirlemek için mantık tanımlamanız gerekir`accountIdentifier` |

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmanın tam bir açıklaması da dahil olmak üzere, uygulama oluşturma konusunda adım adım kılavuz için iOS ve macOS için öğreticiyi deneyin.

### <a name="learn-how-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlatmada kullanılan uygulamayı nasıl oluşturabilirsiniz öğrenin

> [!div class="nextstepaction"]
> [iOS ve macOS için Çağrı Grafiği API eğitimi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
