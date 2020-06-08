---
title: Microsoft Identity platform iOS ve macOS hızlı başlangıç | Mavisi
description: Bir iOS veya macOS uygulamasında kullanıcıların oturum açma ve sorgu Microsoft Graph sorgulama hakkında bilgi edinin.
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
ms.openlocfilehash: 840c0157713e9758092ca5cc51ee2745428ae568
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84483535"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-or-macos-app"></a>Hızlı başlangıç: Kullanıcı oturum açma ve iOS veya macOS uygulamasından Microsoft Graph API 'sini çağırma

Bu hızlı başlangıç, yerel bir iOS veya macOS uygulamasının kişisel, iş ve okul hesaplarında oturum açmak, bir erişim belirteci almak ve Microsoft Graph API 'sini çağırmak için Microsoft Identity platformunu nasıl kullanabileceğinizi gösteren bir kod örneği içerir.

Bu hızlı başlangıç, hem iOS hem de macOS uygulamaları için geçerlidir. Bazı adımlar yalnızca iOS uygulamaları için gereklidir. Bu adımlar yalnızca iOS için olduğunu çağırır.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Önkoşullar**
> * XCode 10 +
> * iOS 10 +
> * macOS 10.12 +

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample"></a>1. seçenek: uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğini indirin
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek için
> 1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://aka.ms/MobileAppReg) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
>      - **Ad** bölümünde, uygulamanızı oturum açtıklarında veya onaylamada uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>      - Bu sayfadaki diğer konfigürasyonları atlayın.
>      - `Register` öğesini seçin.
> 1. **Yönet** bölümünde, öğesini seçin `Authentication`  >  `Add Platform`  >  `iOS` .
>      - Uygulamanız için ***paket kimliğini*** girin. Paket tanımlayıcısı, örneğin, uygulamanızı benzersiz bir şekilde tanımlayan benzersiz bir dizedir `com.<yourname>.identitysample.MSALMacOS` . Kullandığınız değeri bir yere getirin.
>      - İOS yapılandırmasının macOS uygulamaları için de geçerli olduğunu unutmayın.
> 1. `Configure`Bu hızlı başlangıçta daha sonra Için ***msal yapılandırma*** ayrıntılarını seçin ve kaydedin.
> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, auth broker ile uyumlu bir yeniden yönlendirme URI 'SI eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-ios/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış
> 
> #### <a name="step-2-download-the-sample-project"></a>2. Adım: örnek projeyi Indirme
> > [!div id="autoupdate_ios" class="nextstepaction"]
> > [İOS için kod örneğini indirin]()
> 
> > [!div id="autoupdate_macos" class="nextstepaction"]
> > [MacOS için kod örneğini indirin]()
> [!div renderon="docs"]
> #### <a name="step-2-download-the-sample-project"></a>2. Adım: örnek projeyi Indirme
> 
> - [İOS için kod örneğini indirin](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
> - [MacOS için kod örneğini indirin](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

#### <a name="step-3-install-dependencies"></a>3. Adım: bağımlılıkları yüklemeyi

Bir Terminal penceresinde indirilen kod örneğini içeren klasöre gidin ve `pod install` en son msal kitaplığını yüklemek için çalıştırın.

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-4-your-app-is-configured-and-ready-to-run"></a>4. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık ve çalıştırılmaya hazırlanıyor.
> >  [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
>#### <a name="step-4-configure-your-project"></a>4. Adım: projenizi yapılandırma
> Yukarıdaki 1. seçeneği belirlediyseniz, bu adımları atlayabilirsiniz.
> 1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
> 1. **ViewController. Swift** 'u düzenleyin ve ' Let Kclitıd ' ile başlayan satırı aşağıdaki kod parçacığı ile değiştirin. `kClientID`Uygulamanızı bu hızlı başlangıçta daha önce portala kaydettiğinizde kaydettiğiniz ClientID değeriyle güncelleştirmeyi unutmayın:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_Here"
>    ```
> 1. [Azure AD Ulusal bulutları](https://docs.microsoft.com/graph/deployments#app-registration-and-token-service-root-endpoints)için bir uygulama oluşturuyorsanız, ' Let kGraphEndpoint ' ve ' Let kAuthority ' ile başlayan satırı doğru uç noktalarla değiştirin. Genel erişim için varsayılan değerleri kullanın:
>     ```swift
>     let kGraphEndpoint = "https://graph.microsoft.com/"
>     let kAuthority = "https://login.microsoftonline.com/common"
>     ```
> 1. Diğer uç noktalar [burada](https://docs.microsoft.com/graph/deployments#app-registration-and-token-service-root-endpoints)belgelenmiştir. Örneğin, Azure AD Almanya ile hızlı başlangıcı çalıştırmak için aşağıdakileri kullanın:
>     ```swift
>     let kGraphEndpoint = "https://graph.microsoft.de/"
>     let kAuthority = "https://login.microsoftonline.de/common"
>     ```
> 1. Proje ayarlarını açın. **Kimlik** bölümünde, portala girdiğiniz **paket kimliğini** girin.
> 1. **Info. plist** ' e sağ tıklayıp kaynak **kodu olarak aç**' ı seçin  >  **Source Code**.
> 1. Dict kök düğümünün altında, öğesini `Enter_the_bundle_Id_Here` portalda kullandığınız ***paket kimliğiyle*** değiştirin.
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
> 1. Uygulamayı oluşturun & çalıştırın!

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi almak için şu bölümleri okuyun.

### <a name="get-msal"></a>MSAL al

MSAL ([msal. Framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)), kullanıcıların oturum açmasını ve Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri istemesini sağlamak için kullanılan kitaplıktır. Aşağıdaki işlemi uygulayarak uygulamanıza MSAL ekleyebilirsiniz:

```
$ vi Podfile

```
Aşağıdakini Bu Pod dosyasına ekleyin (projenizin hedefi ile):

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL'
end

```

CocoaPods yükleme komutunu çalıştır:

```pod install```

### <a name="initialize-msal"></a>MSAL Başlat

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
> | `authority` | Microsoft Identity platform uç noktası. Çoğu durumda bu *https<span/>://login.microsoftonline.com/common* olur |
> | `redirectUri` | Uygulamanın yeniden yönlendirme URI 'SI. Varsayılan değeri veya özel yeniden yönlendirme URI 'nizi kullanmak için ' Nil ' geçirebilirsiniz. |

### <a name="for-ios-only-additional-app-requirements"></a>Yalnızca iOS için, ek uygulama gereksinimleri

Uygulamanızda de şunlar olmalıdır `AppDelegate` . Bu, kimlik doğrulaması yaparken MSAL SDK 'sının auth Broker uygulamasından belirteç yanıtı işlemesini sağlar.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {

        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
    }

 ```

> [!NOTE]
> İOS 13 ' de yerine benimsediğinizde, `UISceneDelegate` `UIApplicationDelegate` Bu kodu `scene:openURLContexts:` geri aramaya yerleştirin ( [Apple belgelerine](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)bakın).
> Daha eski iOS ile uyumluluk için hem UISceneDelegate hem de Uıapplicationdelegate 'i destekleriyseniz, MSAL geri çağrısının her iki yere de yerleştirilmesi gerekir.

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

Son olarak, uygulamanızın `LSApplicationQueriesSchemes` ***Info. plist*** dosyasında ile birlikte bir girişi olması gerekir `CFBundleURLTypes` . Örnek, bu dahil edilmiştir.

   ```xml
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Kullanıcı & istek belirteçlerinde oturum açma

Belirteç almak için MSAL’in iki yöntemi vardır: `acquireToken` ve `acquireTokenSilent`.

#### <a name="acquiretoken-get-a-token-interactively"></a>acquireToken: bir belirteci etkileşimli olarak alın

Bazı durumlar, kullanıcıların Microsoft Identity platformu ile etkileşime girmesini gerektirir. Bu durumlarda, son kullanıcının hesabını seçmesini, kimlik bilgilerini girmesi veya uygulamanızın izinlerine izin vermesini gerekli olabilir. Örneğin,

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir
* Uygulamanız bir kaynağa ilk kez erişim isteğinde bulunduğunda
* MFA veya diğer koşullu erişim ilkeleri gerektiğinde

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: self.webViewParamaters!)
self.applicationContext!.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Konumlar:||
> |---------|---------|
> | `scopes` | İstenen kapsamları içerir (yani `[ "user.read" ]` Microsoft Graph veya `[ "<Application ID URL>/scope" ]` özel Web API 'leri için ( `api://<Application ID>/access_as_user` ) |

#### <a name="acquiretokensilent-get-an-access-token-silently"></a>acquireTokenSilent: erişim belirtecini sessizce al

Uygulamalar, kullanıcıların her belirteç istediklerinde oturum açmasını gerektirmemelidir. Kullanıcı zaten oturum açmışsa, bu yöntem uygulamaların belirteçleri sessizce istemesine izin verir.

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
> | `scopes` | İstenen kapsamları içerir (yani `[ "user.read" ]` Microsoft Graph veya `[ "<Application ID URL>/scope" ]` özel Web API 'leri için ( `api://<Application ID>/access_as_user` ) |
> | `account` | Belirtecin istendiği hesap. Bu hızlı başlangıç, tek bir hesap uygulaması hakkında. Çok sunuculu bir uygulama oluşturmak istiyorsanız, kullanarak belirteç istekleri için hangi hesabın kullanılacağını belirlemek `accountsFromDeviceForParameters:completionBlock:` ve doğru geçirme yapmak için Logic tanımlamanız gerekir`accountIdentifier` |

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç hakkında tam bir açıklama da dahil olmak üzere uygulamalar oluşturmaya yönelik kapsamlı bir adım adım kılavuz için iOS ve macOS öğreticisini deneyin.

### <a name="learn-how-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturmayı öğrenin

> [!div class="nextstepaction"]
> [İOS ve macOS için Graph API öğreticisi çağırma](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
