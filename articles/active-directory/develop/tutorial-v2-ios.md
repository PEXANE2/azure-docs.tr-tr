---
title: iOS için MSAL & macOS öğretici - Microsoft kimlik platformu | Azure
description: iOS ve macOS (Swift) uygulamalarının Microsoft kimlik platformlarını kullanarak erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceğini öğrenin
services: active-directory
documentationcenter: dev-center-name
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 08/30/2019
ms.author: jmprieur
ms.reviewer: oldalton
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: abf083aacbdc643d780a8061b405752f36e27e45
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79129945"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-or-macos-app"></a>Oturum açın ve bir iOS veya macOS uygulamasından Microsoft Graph'ı arayın

Bu eğitimde, bir iOS veya macOS uygulamasını Microsoft kimlik platformuyla nasıl entegre acağınızı öğreneceksiniz. Uygulama bir kullanıcıda oturum açacak, Microsoft Graph API'yi aramak için bir erişim belirteci alacak ve Microsoft Graph API'ye istekte bulunacaktır.  

Kılavuzu tamamladığınızda, uygulamanız Azure Active Directory kullanan herhangi bir şirket veya kuruluşun kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) oturum açmalarını ve iş veya okul hesaplarını kabul eder.

>[!NOTE]
> Microsoft kimlik platformunda yeniyseniz, Oturum Açma kullanıcılarıyla başlamanızı [ve bir iOS veya macOS uygulamasından Microsoft Graph API'yi aramanızı](quickstart-v2-ios.md)öneririz.

## <a name="how-this-tutorial-works"></a>Bu öğretici nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Bu eğitimdeki uygulama, kullanıcılar oturum açacak ve onların adına veri alacak.  Bu verilere, yetkilendirme gerektiren ve Microsoft kimlik platformu tarafından korunan korumalı api (bu durumda Microsoft Graph API) aracılığıyla erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanız kullanıcıda bir tarayıcı veya Microsoft Authenticator aracılığıyla oturum açacaktır.
* Son kullanıcı, uygulamanızın istediği izinleri kabul eder.
* Uygulamanız Microsoft Graph API için bir erişim belirteci verilir.
* Erişim belirteci, web API'ye http isteğine eklenecektir.
* Microsoft Graph yanıtını işleme.

Bu örnek, Kimlik Doğrulama'yı uygulamak için Microsoft Kimlik Doğrulama kitaplığını (MSAL) kullanır. MSAL belirteçleri otomatik olarak yeniler, cihazdaki diğer uygulamalar arasında tek oturum açma (SSO) sağlar ve Hesabı(lar) yönetir.

Bu öğretici hem iOS hem de macOS uygulamaları için geçerlidir. Bazı adımların bu iki platform arasında farklı olduğunu unutmayın. 

## <a name="prerequisites"></a>Ön koşullar

- Uygulamayı bu kılavuzda oluşturmak için XCode sürüm 10.x veya daha büyük olması gerekir. XCode'u [iTunes web sitesinden](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode İndirme URL'si")indirebilirsiniz.
- Microsoft Kimlik Doğrulama Kitaplığı ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Bağımlılık yöneticisi kullanabilir veya kitaplığı el ile ekleyebilirsiniz. Aşağıdaki talimatlar size nasıl yapılacağını gösterir.

Bu öğretici yeni bir proje oluşturacaktır. Bunun yerine tamamlanmış öğreticiyi indirmek istiyorsanız, kodu indirin:
- [iOS örnek kodu](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
- [macOS örnek kodu](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Xcode'u açın ve **yeni bir Xcode projesi oluştur'u**seçin.
2. iOS uygulamaları için **iOS** > **Tek görünüm Uygulamasını** seçin ve **İleri'yi**seçin.
3. macOS uygulamaları için **macOS** > **Kakao Uygulamasını** seçin ve **İleri'yi**seçin.
4. Bir ürün adı sağlayın.
5. **Dili** **Swift** olarak ayarlayın ve **İleri'yi**seçin.
6. Uygulamanızı oluşturmak için bir klasör seçin ve **Oluştur'u**tıklatın.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

1. [Azure portalına](https://aka.ms/MobileAppReg) gidin
2. Uygulama [kayıtları bıçak](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) açın ve **+Yeni kayıt**tıklayın.
3. Uygulamanız için bir **Ad** girin ve ardından, Bir Yeniden Yönlendirme URI ayarlamadan **Kaydol'u**tıklatın.
4. Bölmenin görünen **yönet** bölümünde **Kimlik Doğrulama'yı**seçin.

5. Yeni uygulama kaydı deneyimini açmak için ekranın üst **kısmındaki yeni deneyimi deneyin'e** tıklayın ve ardından **+Yeni kayıt** > + Platform > **iOS****ekle'ye**tıklayın.
    - Projenizin Paket Kimliğini girin. Kodu indirdiyseniz, bu `com.microsoft.identitysample.MSALiOS`. Kendi projenizi oluşturuyorsanız, Projenizi Xcode'da seçin ve **Genel** sekmesini açın. Paket tanımlayıcısı **Kimlik** bölümünde görünür.
    - macOS için iOS deneyimini de kullanmanız gerektiğini unutmayın. 
6. uygulamanızı daha sonra yapılandırdığınızda girebilmeniz için **iOS yapılandırma** sayfasında görünen **MSAL Yapılandırmasını** tıklatın `Configure` ve kaydedin.  **Bitti**’ye tıklayın.

## <a name="add-msal"></a>MSAL Ekle

Uygulamanızda MSAL kitaplığını yüklemenin aşağıdaki yollarından birini seçin:

### <a name="cocoapods"></a>KakaoPodlar

1. [CocoaPods](https://cocoapods.org/)kullanıyorsanız, önce `MSAL` projenizin `.xcodeproj` dosyasıyla `podfile` aynı klasörde adlandırılan boş bir dosya oluşturarak yükleyin. Aşağıdakileri `podfile`ekleyin:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL'
   end
   ```

2. Projenizin adıyla değiştirin. `<your-target-here>`
3. Terminal penceresinde, `podfile` oluşturduğunuz klasöre gidin ve MSAL kitaplığını yüklemek için çalıştırın. `pod install`
4. Xcode'u `<your project name>.xcworkspace` kapatın ve projeyi Xcode'da yeniden yüklemek için açın.

### <a name="carthage"></a>Kartaca

[Kartaca](https://github.com/Carthage/Carthage)kullanıyorsanız, `MSAL` yükleyin: `Cartfile`

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

Bir terminal penceresinden, güncelleştirilmiş `Cartfile`olarak aynı dizinde, Kartaca projenizdeki bağımlılıkları güncelleştirmek için aşağıdaki komutu çalıştırın.

iOS:

```bash
carthage update --platform iOS
```

Macos:

```bash
carthage update --platform macOS
```

### <a name="manually"></a>El ile

Git Alt Modül'üni de kullanabilir veya uygulamanızda çerçeve olarak kullanmak üzere en son sürümü niçin kullanıma alabilirsiniz.

## <a name="add-your-app-registration"></a>Uygulama kaydınızı ekleyin

Ardından, uygulama kaydınızı kodunuza ekleriz. 

İlk olarak, dosyaların ve `ViewController.swift` `AppDelegate.swift` dosyaların üstüne aşağıdaki alma deyimini ekleyin:

```swift
import MSAL
```

Sonra `ViewController.swift` önce aşağıdaki kodu `viewDidLoad()`ekleyin:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/" // the Microsoft Graph endpoint
let kScopes: [String] = ["https://graph.microsoft.com/user.read"] // request permission to read the profile of the signed-in user
let kAuthority = "https://login.microsoftonline.com/common" // this authority allows a personal Microsoft account and a work or school account in any organization’s Azure AD tenant to sign in
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
var webViewParameters : MSALWebviewParameters?
```

Yukarıda değiştirmeniz gereken tek `kClientID` [değer, Uygulama Kimliğiniz](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#application-id-client-id)olarak atanan değerdir. Bu değer, uygulamayı Azure portalına kaydetmek için bu öğreticinin başındaki adımda kaydettiğiniz MSAL Yapılandırma verilerinin bir parçasıdır.

## <a name="for-ios-only-configure-url-schemes"></a>Yalnızca iOS için URL şemalarını yapılandırın

Bu adımda, kullanıcının oturum açmadan sonra uygulamaya geri yönlendirilebilmeleri için kaydolursunuz. `CFBundleURLSchemes` Bu arada, `LSApplicationQueriesSchemes` uygulamanızın Microsoft Authenticator'dan yararlanmasına da olanak tanır.

Xcode'da `Info.plist` kaynak kodu dosyası olarak açın ve `<dict>` bölümün içine aşağıdakileri ekleyin. Azure `[BUNDLE_ID]` portalında kullandığınız değeri değiştirin, kodu indirdiyseniz `com.microsoft.identitysample.MSALiOS`. Kendi projenizi oluşturuyorsanız, Projenizi Xcode'da seçin ve **Genel** sekmesini açın. Paket tanımlayıcısı **Kimlik** bölümünde görünür.

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
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauthv2</string>
    <string>msauthv3</string>
</array>
```

## <a name="for-macos-only-configure-app-sandbox"></a>Yalnızca macOS için App Sandbox'ı yapılandırın

1. Xcode Proje Ayarlarınıza > **Özellikler sekmesi** > **App Sandbox'ınıza** gidin
2. **Giden Bağlantılar (İstemci)** onay kutusunu seçin. 

## <a name="create-your-apps-ui"></a>Uygulamanızın UI'ini oluşturma

Şimdi, Microsoft Graph API'yi aramak için bir düğme, oturum dışı kalmak için başka bir düğme ve `ViewController`sınıfa aşağıdaki kodu ekleyerek bazı çıktıları görmek için metin görünümü içeren bir kullanıcı arabirimi oluşturun:

### <a name="ios-ui"></a>iOS UI

```swift
var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

### <a name="macos-ui"></a>macOS UI

```swift

var callGraphButton: NSButton!
var loggingText: NSTextView!
var signOutButton: NSButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = NSButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.title = "Call Microsoft Graph API"
        callGraphButton.target = self
        callGraphButton.action = #selector(callGraphAPI(_:))
        callGraphButton.bezelStyle = .rounded
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 30.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
        
        // Add sign out button
        signOutButton = NSButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.title = "Sign Out"
        signOutButton.target = self
        signOutButton.action = #selector(signOut(_:))
        signOutButton.bezelStyle = .texturedRounded
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 34.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = NSTextView()
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: -10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: -10.0).isActive = true
        loggingText.widthAnchor.constraint(equalToConstant: 500.0).isActive = true
        loggingText.heightAnchor.constraint(equalToConstant: 300.0).isActive = true
    }
```

Ardından, `ViewController` yine sınıfın içinde, `viewDidLoad()` yöntemi aşağıdakilerle değiştirin:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.updateLogging(text: "Unable to create Application Context \(error)")
        }
    }
```

## <a name="use-msal"></a>MSAL kullanın

### <a name="initialize-msal"></a>MSAL'ı Başlatma

`ViewController` Sınıfa aşağıdaki `initMSAL` yöntemi ekleyin:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.updateLogging(text: "Unable to create authority URL")
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
        self.initWebViewParams()
    }
```

Aşağıdaki `initMSAL` yöntemi sınıfa `ViewController` ekleyin.

### <a name="ios-code"></a>iOS kodu:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters(parentViewController: self)
    }
```

### <a name="macos-code"></a>macOS kodu:

```swift
func initWebViewParams() {
        self.webViewParameters = MSALWebviewParameters()
        self.webViewParameters?.webviewType = .wkWebView
    }
```

### <a name="for-ios-only-handle-the-sign-in-callback"></a>Yalnızca iOS için oturum açma geri aramasını işleme

`AppDelegate.swift` dosyasını açın. Oturum açmadan sonra geri aramayı `MSALPublicClientApplication.handleMSALResponse` işlemek `appDelegate` için sınıfa şu şekilde ekleyin:

```swift
// Inside AppDelegate...
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}

```

**Xcode 11 kullanıyorsanız,** `SceneDelegate.swift` bunun yerine MSAL geri arama yerleştirmelisiniz.
Eski iOS ile uyumluluk için hem UISceneDelegate'ı hem de UIApplicationDelegate'i destekliyorsanız, MSAL geri aramanın her iki dosyaya da yerleştirilmesi gerekir.

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

#### <a name="acquire-tokens"></a>Jeton Edinin

Şimdi, uygulamanın Kullanıcı Arabirimi işleme mantığını uygulayabilir ve msal aracılığıyla etkileşimli olarak jetonları alabilirsiniz.

MSAL belirteçleri almak için iki `acquireTokenSilently()` `acquireTokenInteractively()`birincil yöntem ortaya çıkarır: ve: 

- `acquireTokenSilently()`bir hesap olduğu sürece kullanıcı etkileşimi olmadan bir kullanıcı oturum açma ve belirteçleri almaya çalışır.

- `acquireTokenInteractively()`kullanıcıda oturum açmaya çalışırken her zaman Kullanıcı GI'sini gösterir. Etkileşimli bir SSO deneyimi sağlamak için tarayıcıda veya Microsoft kimlik doğrulayıcısındaki bir hesapta oturum tanımlama bilgilerini kullanabilir.

`ViewController` Sınıfa aşağıdaki kodu ekleyin:

```swift
    @objc func callGraphAPI(_ sender: AnyObject) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>Etkileşimli bir belirteç alın

Aşağıdaki kod, bir `MSALInteractiveTokenParameters` nesne oluşturup çağırarak ilk `acquireToken`kez bir belirteç alır. Sonra kod ekleyeceğiz:

1. Kapsamlarla `MSALInteractiveTokenParameters` oluşturur.
2. Oluşturulan `acquireToken()` parametreleri içeren aramalar.
3. Hataları işler. Daha fazla ayrıntı [için iOS için MSAL ve macOS hata işleme kılavuzuna](msal-handling-exceptions.md)bakın.
4. Başarılı durumda işler.

`ViewController` Sınıfa aşağıdaki kodu ekleyin.

```swift
func acquireTokenInteractively() {
        
    guard let applicationContext = self.applicationContext else { return }
    guard let webViewParameters = self.webViewParameters else { return }
        
    // #1
    let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: webViewParameters)
        
    // #2
    applicationContext.acquireToken(with: parameters) { (result, error) in
            
        // #3
        if let error = error {
                
            self.updateLogging(text: "Could not acquire token: \(error)")
            return
        }
            
        guard let result = result else {
                
            self.updateLogging(text: "Could not acquire token: No result returned")
            return
        }
            
        // #4
        self.accessToken = result.accessToken
        self.updateLogging(text: "Access token is \(self.accessToken)")
        self.updateSignOutButton(enabled: true)
        self.getContentWithToken()
    }
}    
```


#### <a name="get-a-token-silently"></a>Sessizce bir belirteç alın

Güncelleştirmeli bir belirteci sessizce elde etmek `ViewController` için sınıfa aşağıdaki kodu ekleyin. Bir `MSALSilentTokenParameters` nesne oluşturur ve `acquireTokenSilent()`çağırır:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>Microsoft Graph API'yi arayın 

Bir belirteç aldıktan sonra, uygulamanız Microsoft Graph'a yetkili bir istekte bulunmak için bu tonu HTTP üstbilgisinde kullanabilir:

| üstbilgi anahtarı    | value                 |
| ------------- | --------------------- |
| Yetkilendirme | Taşıyıcı \<erişim belirteci> |

`ViewController` Sınıfa aşağıdaki kodu ekleyin:

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
               
        URLSession.shared.dataTask(with: request) { data, response, error in
               
        if let error = error {
            self.updateLogging(text: "Couldn't get graph result: \(error)")
            return
        }
               
        guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
               
        self.updateLogging(text: "Couldn't deserialize result JSON")
            return
        }
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

Microsoft Graph API hakkında daha fazla bilgi edinmek için [Microsoft Graph API'ye](https://graph.microsoft.com) bakın.

### <a name="use-msal-for-sign-out"></a>Oturum Açma için MSAL'ı kullanın

Ardından, oturum açma desteği ekleyin.

> [!Important]
> MSAL ile oturum alabilme, kullanıcı hakkında bilinen tüm bilgileri uygulamadan kaldırır, ancak kullanıcının aygıtında etkin bir oturumu vardır. Kullanıcı yeniden oturum açmaya çalışırsa oturum açma ui'sini görebilir, ancak aygıt oturumu hala etkin olduğundan kimlik bilgilerini yeniden girmeleri gerekmeyebilir.

Oturum açma özelliği eklemek için `ViewController` sınıfın içine aşağıdaki kodu ekleyin. Bu yöntem tüm hesaplar arasında döngüler ve bunları kaldırır:

```swift 
@objc func signOut(_ sender: AnyObject) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            try applicationContext.remove(account)
            self.updateLogging(text: "")
            self.updateSignOutButton(enabled: false)
            self.accessToken = ""
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Belirteç önbelleğe alma etkinleştirme

Varsayılan olarak, MSAL uygulamanızın belirteçlerini iOS veya macOS anahtar zincirinde önbelleğe alır. 

Belirteç önbelleğe alma etkinleştirmek için:
1. Uygulamanızın düzgün imzalandığından emin olun
2. Xcode Proje Ayarlarınıza gidin > **Özellikler sekmesi** > **Anahtar Zinciri Paylaşımını Etkinleştir**
3. Aşağıdaki **+** **Anahtarlık Grupları** girişini tıklatın ve girin: 3.a iOS için, macOS için 3.b girin `com.microsoft.adalcache``com.microsoft.identity.universalstorage`

### <a name="add-helper-methods"></a>Yardımcı yöntemleri ekleme
Örneği tamamlamak için `ViewController` sınıfa aşağıdaki yardımcı yöntemleri ekleyin.

### <a name="ios-ui"></a>iOS UI:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="macos-ui"></a>macOS UI:

```swift
func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.string = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.string = text
            }
        }
    }
```



### <a name="multi-account-applications"></a>Çoklu hesap uygulamaları

Bu uygulama tek bir hesap senaryosu için oluşturulmuştur. MSAL ayrıca çok hesaplı senaryoları destekler, ancak uygulamalardan bazı ek çalışma gerektirir. Kullanıcıların belirteçleri gerektiren her eylem için hangi hesabı kullanmak istediklerini seçmelerine yardımcı olmak için Kullanıcı Arabirimi oluşturmanız gerekir. Alternatif olarak, uygulamanız `getAccounts()` yöntem aracılığıyla hangi hesabı kullanacağınızı seçmek için sezgisel bir uygulama uygulayabilir.

## <a name="test-your-app"></a>Uygulamanızı test etme

### <a name="run-locally"></a>Yerel olarak çalıştırma

Uygulamayı bir test aygıtına veya simülatöre oluşturun ve dağıtın. Azure AD veya kişisel Microsoft hesapları için oturum açabilmeli ve jeton alabilmelisiniz.

Bir kullanıcı uygulamanızı ilk kez imzaladığında, istenen izinleri kabul etmeleri için Microsoft kimliği tarafından istenir.  Çoğu kullanıcı izin verebilme yeteneğine sahip olsa da, bazı Azure AD kiracıları, yöneticilerin tüm kullanıcılar adına onay ını gerektiren kullanıcı onayını devre dışı bıraktı. Bu senaryoyu desteklemek için uygulamanızın kapsamlarını Azure portalına kaydedin.

Oturum açmanızdan sonra, uygulama Microsoft Graph `/me` bitiş noktasından döndürülen verileri görüntüler.

## <a name="get-help"></a>Yardım alın

Bu [öğreticiyle](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) veya Microsoft kimlik platformunda sorun yaşıyorsanız Yardım ve desteği ziyaret edin.

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
