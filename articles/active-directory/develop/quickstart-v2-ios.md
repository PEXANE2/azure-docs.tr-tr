---
title: Microsoft Identity platform iOS hızlı başlangıç | Mavisi
description: Bir iOS uygulamasındaki kullanıcıların oturumunu açma ve sorgu Microsoft Graph hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: twhitney
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e6a9dab8a2a37d7fa312b525453683fe789269b
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852942"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Hızlı Başlangıç: Kullanıcı oturum açma ve iOS uygulamasından Microsoft Graph API 'sini çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıç, yerel bir iOS uygulaması ile kişisel, iş ve okul hesaplarının oturumunu açmayı, erişim belirteci almayı ve Microsoft Graph API’sini çağırmayı gösteren bir kod örneği içerir.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Önkoşullar**
> * XCode 10 +
> * iOS 10 + 

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * Çaba [Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * Kitabında [Seçenek 2: Uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydedin
> Uygulamanızı kaydetmek için
> 1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2: Uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydedin
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://aka.ms/MobileAppReg) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - **Ad** bölümünde, örneğin `iOSQuickstart`, uygulamanızda oturum açtıklarında veya onaylarsa uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>      - Bu sayfadaki diğer konfigürasyonları atlayın. 
>      - `Register` Düğmeye basın.
> 1. Git `Authentication` >Yeni`Add Platform`uygulamaya tıklayın .`iOS`  >   >     
>      - Uygulamanız için ***paket kimliğini*** girin. 
> 1. Daha `Configure` sonra ***msal yapılandırma*** ayrıntılarını seçin ve kaydedin. 

> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>1\. adım: Uygulamanızı yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, auth broker ile uyumlu bir yeniden yönlendirme URI 'SI eklemeniz gerekir. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-ios/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış

#### <a name="step-2-download-your-web-server-or-project"></a>2\. adım: Web sunucunuzu veya projenizi indirin

- [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3\. adım: Projenizi yapılandırma

> [!div renderon="docs"]
> Yukarıdaki 1. seçeneği belirlediyseniz, bu adımları atlayabilirsiniz. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
> 1. **ViewController.swift**’i düzenleyin ve 'let kClientID' ile başlayan satırı aşağıdaki kod parçacığı ile değiştirin:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_here"
>    let kAuthority = "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
>    ``` 
> 1. **Info. plist** öğesine sağ tıklayın ve > **kaynak kodu**aç ' ı seçin.
> 1. Dict kök düğümünün altında, değerini ***paket Kimliğinizle***değiştirin:
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

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Bu hızlı başlangıç, Enter_the_Supported_Account_Info_Here destekler.

> [!div renderon="docs"]
>
> 1. Zip dosyasını ayıklayın ve projeyi XCode’da açın.
> 1. **ViewController. Swift** 'u düzenleyin ve ' Let Kclitıd ' ile başlayan satırı aşağıdaki kod parçacığı ile değiştirin:
>
>    ```swift
>    let kClientID = "<ENTER_YOUR_APPLICATION/CLIENT_ID>"
> 
>    ```
> 1. **Info. plist** öğesine sağ tıklayın ve > **kaynak kodu**aç ' ı seçin.
> 1. Dict kök düğümünün altında, değerini ***paket Kimliğinizle***değiştirin:
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.<ENTER_YOUR_BUNDLE_ID></string>
>          </array>
>       </dict>
>    </array>
>
>    ```
> 1. Uygulamayı oluşturun & çalıştırın! 

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi almak için şu bölümleri okuyun.

### <a name="getting-msal"></a>MSAL alma

MSAL ([msal. Framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)), kullanıcıların oturum açmasını ve Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri istemesini sağlamak için kullanılan kitaplıktır. Aşağıdaki işlemi uygulayarak uygulamanıza MSAL ekleyebilirsiniz:

```
$ vi Podfile

```
Aşağıdakini Bu Pod dosyasına ekleyin (projenizin hedefi ile):

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL', '~> 0.4.0'
end

```

### <a name="msal-initialization"></a>MSAL başlatma

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
> | `clientId` | *portal.azure.com* adresinde kayıtlı uygulamaya ait Uygulama Kimliği |
> | `authority` | Microsoft Identity platform uç noktası. Çoğu durumda bu *https<span/>://login.microsoftonline.com/common* olur |
> | `redirectUri` | Uygulamanın yeniden yönlendirme URI 'SI. Varsayılan değeri veya özel yeniden yönlendirme URI 'nizi kullanmak için ' Nil ' geçirebilirsiniz. |

### <a name="additional-app-requirements"></a>Ek uygulama gereksinimleri  

Uygulamanızda de şunlar `AppDelegate`olmalıdır. Bu, kimlik doğrulaması gerçekleştirirken MSAL SDK 'sının auth Broker uygulamasından belirteç yanıtı işlemesini sağlar.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
         guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
             return false
         }
         
         return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
     }

```

Son olarak, uygulamanızın `LSApplicationQueriesSchemes` ***Info.*** `CFBundleURLTypes`plist dosyasında ile birlikte bir girişi olması gerekir. Örnek, bu dahil edilmiştir. 

   ```xml 
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauth</string>
      <string>msauthv2</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Kullanıcı & Istek belirteçlerinde oturum açma

Belirteç almak için MSAL’in iki yöntemi vardır: `acquireToken` ve `acquireTokenSilent`.

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Belirteci etkileşimli olarak alma

Bazı durumlar, kullanıcıların Microsoft Identity platformu ile etkileşime girmesini gerektirir. Bu durumlarda, son kullanıcının hesabını seçmesini, kimlik bilgilerini girmesi veya uygulamanızın izinlerine izin vermesini gerekli olabilir. Örneğin, 

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Kullanıcı parolasını sıfırlarsa, kimlik bilgilerini girmeleri gerekir 
* Uygulamanız bir kaynağa ilk kez erişim isteğinde bulunduğunda
* MFA veya diğer koşullu erişim ilkeleri gerektiğinde

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
applicationContext.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Konumlar:||
> |---------|---------|
> | `scopes` | İstenen kapsamları içerir (yani `[ "user.read" ]` Microsoft Graph veya `[ "<Application ID URL>/scope" ]` özel Web API 'leri için (`api://<Application ID>/access_as_user`) |

#### <a name="acquiretokensilent-getting-an-access-token-silently"></a>acquireTokenSilent: Erişim belirtecini sessiz bir şekilde alma

Uygulamalar, kullanıcıların her belirteç istediklerinde oturum açmasını gerektirmemelidir. Kullanıcı zaten oturum açmışsa, bu yöntem uygulamaların belirteçleri sessizce istemesine izin verir. 

```swift
let parameters = MSALSilentTokenParameters(scopes: kScopes, account: applicationContext.allAccounts().first)
applicationContext.acquireTokenSilent(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Konumlar: ||
> |---------|---------|
> | `scopes` | İstenen kapsamları içerir (yani `[ "user.read" ]` Microsoft Graph veya `[ "<Application ID URL>/scope" ]` özel Web API 'leri için (`api://<Application ID>/access_as_user`) |
> | `account` | Belirtecin istendiği hesap. Bu hızlı başlangıç tek bir hesap uygulamasıdır ve çok sunuculu bir uygulama oluşturmak istiyorsanız, belirteç istekleri için kullanılacak hesabı belirlemek üzere mantığı tanımlamanız gerekir`applicationContext.account(forHomeAccountId: self.homeAccountId)` |

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç hakkında tam bir açıklama da dahil olmak üzere uygulamalar oluşturmaya yönelik kapsamlı adım adım yönergeler için iOS öğreticisini deneyin.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

> [!div class="nextstepaction"]
> [Graph API’si çağırma iOS öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)