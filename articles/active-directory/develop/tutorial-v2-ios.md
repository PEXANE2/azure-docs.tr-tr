---
title: iOS için MSAL & macOS öğretici - Microsoft kimlik platformu | Azure
description: iOS ve macOS (Swift) uygulamalarının Microsoft kimlik platformlarını kullanarak erişim belirteçleri gerektiren bir API'yi nasıl çağırabileceğini öğrenin
services: active-directory
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
ms.openlocfilehash: 3569d0404f6c0c7f78344ba53733ba2c0c43d979
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80880814"
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

- Uygulamayı bu kılavuzda oluşturmak için XCode sürüm 11.x veya daha büyük olması gerekir. XCode'u [iTunes web sitesinden](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode İndirme URL'si")indirebilirsiniz.
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
2. Uygulama kayıtları bıçak açın ve **+Yeni kayıt**tıklayın.
3. Uygulamanız için bir **Ad** girin ve ardından bir Yönlendirme URI'si ayarlamadan.
4. **Desteklenen hesap türleri** altında herhangi bir kuruluş **dizininde (Herhangi bir Azure REKLAM dizini - Çoklu Kiracı) ve kişisel Microsoft hesaplarında (örneğin Skype, Xbox) Hesapları** seçin
5. **Kayıt'yı** tıklatın
6. Bölmenin görünen **yönet** bölümünde **Kimlik Doğrulama'yı**seçin.

7. Yeni uygulama kaydı deneyimini açmak için ekranın üst **kısmındaki yeni deneyimi deneyin'e** tıklayın ve ardından **+Yeni kayıt** > + platform > **iOS/macOS****ekle'ye**tıklayın.
    - Projenizin Paket Kimliğini girin. Kodu indirdiyseniz, bu `com.microsoft.identitysample.MSALiOS`. Kendi projenizi oluşturuyorsanız, Projenizi Xcode'da seçin ve **Genel** sekmesini açın. Paket tanımlayıcısı **Kimlik** bölümünde görünür.
8. Uygulamanızı daha sonra yapılandırdığınızda girebilmeniz için MSAL yapılandırma sayfasında görünen `Configure` **MSAL Yapılandırmasını** tıklatın ve kaydedin. **MSAL configuration**  **Bitti**’ye tıklayın.

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

İlk olarak, aşağıdaki alma deyimini `ViewController.swift`, yanı `AppDelegate.swift` sıra `SceneDelegate.swift` veya dosyalarının üstüne ekleyin:

```swift
import MSAL
```

Sonra `ViewController.swift` önce aşağıdaki kodu `viewDidLoad()`ekleyin:

```swift
// Update the below to your client ID you received in the portal. The below is for running the demo only
let kClientID = "Your_Application_Id_Here"
let kGraphEndpoint = "https://graph.microsoft.com/" // the Microsoft Graph endpoint
let kAuthority = "https://login.microsoftonline.com/common" // this authority allows a personal Microsoft account and a work or school account in any organization’s Azure AD tenant to sign in
    
let kScopes: [String] = ["user.read"] // request permission to read the profile of the signed-in user
    
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
var webViewParameters : MSALWebviewParameters?
var currentAccount: MSALAccount?
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
var usernameLabel: UILabel!

func initUI() {
        
    usernameLabel = UILabel()
    usernameLabel.translatesAutoresizingMaskIntoConstraints = false
    usernameLabel.text = ""
    usernameLabel.textColor = .darkGray
    usernameLabel.textAlignment = .right
        
    self.view.addSubview(usernameLabel)
        
    usernameLabel.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
    usernameLabel.rightAnchor.constraint(equalTo: view.rightAnchor, constant: -10.0).isActive = true
    usernameLabel.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
    usernameLabel.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add call Graph button
    callGraphButton  = UIButton()
    callGraphButton.translatesAutoresizingMaskIntoConstraints = false
    callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
    callGraphButton.setTitleColor(.blue, for: .normal)
    callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
    self.view.addSubview(callGraphButton)
        
    callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 120.0).isActive = true
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
        
    let deviceModeButton = UIButton()
    deviceModeButton.translatesAutoresizingMaskIntoConstraints = false
    deviceModeButton.setTitle("Get device info", for: .normal);
    deviceModeButton.setTitleColor(.blue, for: .normal);
    deviceModeButton.addTarget(self, action: #selector(getDeviceMode(_:)), for: .touchUpInside)
    self.view.addSubview(deviceModeButton)
        
    deviceModeButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    deviceModeButton.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
    deviceModeButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
    deviceModeButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
    // Add logging textfield
    loggingText = UITextView()
    loggingText.isUserInteractionEnabled = false
    loggingText.translatesAutoresizingMaskIntoConstraints = false
        
    self.view.addSubview(loggingText)
        
    loggingText.topAnchor.constraint(equalTo: deviceModeButton.bottomAnchor, constant: 10.0).isActive = true
    loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
    loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: -10.0).isActive = true
    loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
}

func platformViewDidLoadSetup() {
                
    NotificationCenter.default.addObserver(self,
                        selector: #selector(appCameToForeGround(notification:)),
                        name: UIApplication.willEnterForegroundNotification,
                        object: nil)
        
}
    
@objc func appCameToForeGround(notification: Notification) {
    self.loadCurrentAccount()
}

```

### <a name="macos-ui"></a>macOS UI

```swift

var callGraphButton: NSButton!
var loggingText: NSTextView!
var signOutButton: NSButton!
    
var usernameLabel: NSTextField!

func initUI() {
        
    usernameLabel = NSTextField()
    usernameLabel.translatesAutoresizingMaskIntoConstraints = false
    usernameLabel.stringValue = ""
    usernameLabel.isEditable = false
    usernameLabel.isBezeled = false
    self.view.addSubview(usernameLabel)
        
    usernameLabel.topAnchor.constraint(equalTo: view.topAnchor, constant: 30.0).isActive = true
    usernameLabel.rightAnchor.constraint(equalTo: view.rightAnchor, constant: -10.0).isActive = true
        
    // Add call Graph button
    callGraphButton  = NSButton()
    callGraphButton.translatesAutoresizingMaskIntoConstraints = false
    callGraphButton.title = "Call Microsoft Graph API"
    callGraphButton.target = self
    callGraphButton.action = #selector(callGraphAPI(_:))
    callGraphButton.bezelStyle = .rounded
    self.view.addSubview(callGraphButton)
        
    callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
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

func platformViewDidLoadSetup() {}

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
        
        self.loadCurrentAccount()
        self.platformViewDidLoadSetup()
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

- `acquireTokenSilently()`bir hesap olduğu sürece kullanıcı etkileşimi olmadan bir kullanıcı oturum açma ve belirteçleri almaya çalışır. `acquireTokenSilently()`MSAL hesap `MSALAccount` numaralandırma API'lerinden biri kullanılarak alınabilecek bir geçerli sağlamayı gerektirir. Bu örnek, cari hesabı almak için kullanır. `applicationContext.getCurrentAccount(with: msalParameters, completionBlock: {})` 

- `acquireTokenInteractively()`kullanıcıda oturum açmaya çalışırken her zaman Kullanıcı GI'sini gösterir. Etkileşimli bir SSO deneyimi sağlamak için tarayıcıda veya Microsoft kimlik doğrulayıcısındaki bir hesapta oturum tanımlama bilgilerini kullanabilir.

`ViewController` Sınıfa aşağıdaki kodu ekleyin:

```swift
    func getGraphEndpoint() -> String {
        return kGraphEndpoint.hasSuffix("/") ? (kGraphEndpoint + "v1.0/me/") : (kGraphEndpoint + "/v1.0/me/");
    }

    @objc func callGraphAPI(_ sender: AnyObject) {
        
        self.loadCurrentAccount { (account) in
            
            guard let currentAccount = account else {
                
                // We check to see if we have a current logged in account.
                // If we don't, then we need to sign someone in.
                self.acquireTokenInteractively()
                return
            }
            
            self.acquireTokenSilently(currentAccount)
        }
    }

    typealias AccountCompletion = (MSALAccount?) -> Void

    func loadCurrentAccount(completion: AccountCompletion? = nil) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        let msalParameters = MSALParameters()
        msalParameters.completionBlockQueue = DispatchQueue.main
                
        applicationContext.getCurrentAccount(with: msalParameters, completionBlock: { (currentAccount, previousAccount, error) in
            
            if let error = error {
                self.updateLogging(text: "Couldn't query current account with error: \(error)")
                return
            }
            
            if let currentAccount = currentAccount {
                
                self.updateLogging(text: "Found a signed in account \(String(describing: currentAccount.username)). Updating data for that account...")
                
                self.updateCurrentAccount(account: currentAccount)
                
                if let completion = completion {
                    completion(self.currentAccount)
                }
                
                return
            }
            
            self.updateLogging(text: "Account signed out. Updating UX")
            self.accessToken = ""
            self.updateCurrentAccount(account: nil)
            
            if let completion = completion {
                completion(nil)
            }
        })
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
    parameters.promptType = .selectAccount
        
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
        self.updateCurrentAccount(account: result.account)
        self.getContentWithToken()
    }
}    
```


#### <a name="get-a-token-silently"></a>Sessizce bir belirteç alın

Güncelleştirmeli bir belirteci sessizce elde etmek `ViewController` için sınıfa aşağıdaki kodu ekleyin. Bir `MSALSilentTokenParameters` nesne oluşturur ve `acquireTokenSilent()`çağırır:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        /**
         
         Acquire a token for an existing account silently
         
         - forScopes:           Permissions you want included in the access token received
         in the result in the completionBlock. Not all scopes are
         guaranteed to be included in the access token returned.
         - account:             An account object that we retrieved from the application object before that the
         authentication flow will be locked down to.
         - completionBlock:     The completion block that will be called when the authentication
         flow completes, or encounters an error.
         */
        
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in
            
            if let error = error {
                
                let nsError = error as NSError
                
                // interactionRequired means we need to ask the user to sign-in. This usually happens
                // when the user's Refresh Token is expired or if the user has changed their password
                // among other possible reasons.
                
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
        let graphURI = getGraphEndpoint()
        let url = URL(string: graphURI)
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
> MSAL ile oturum açma, kullanıcı hakkında bilinen tüm bilgileri uygulamadan kaldırır ve aygıt yapılandırması tarafından izin verildiğinde aygıtlarındaki etkin oturumu kaldırır. Ayrıca isteğe bağlı olarak tarayıcıdan kullanıcıyı dışarı çıkabilirsiniz.

Oturum açma özelliği eklemek için `ViewController` sınıfın içine aşağıdaki kodu ekleyin. 

```swift 
@objc func signOut(_ sender: AnyObject) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            let signoutParameters = MSALSignoutParameters(webviewParameters: self.webViewParameters!)
            signoutParameters.signoutFromBrowser = false // set this to true if you also want to signout from browser or webview
            
            applicationContext.signout(with: account, signoutParameters: signoutParameters, completionBlock: {(success, error) in
                
                if let error = error {
                    self.updateLogging(text: "Couldn't sign out account with error: \(error)")
                    return
                }
                
                self.updateLogging(text: "Sign out completed successfully")
                self.accessToken = ""
                self.updateCurrentAccount(account: nil)
            })
            
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
    
    func updateAccountLabel() {
        
        guard let currentAccount = self.currentAccount else {
            self.usernameLabel.text = "Signed out"
            return
        }
        
        self.usernameLabel.text = currentAccount.username
    }
    
    func updateCurrentAccount(account: MSALAccount?) {
        self.currentAccount = account
        self.updateAccountLabel()
        self.updateSignOutButton(enabled: account != nil)
    }
```

### <a name="macos-ui"></a>macOS UI:

```swift
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.string = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.string = text
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
    
     func updateAccountLabel() {

         guard let currentAccount = self.currentAccount else {
            self.usernameLabel.stringValue = "Signed out"
            return
        }

        self.usernameLabel.stringValue = currentAccount.username ?? ""
        self.usernameLabel.sizeToFit()
     }

     func updateCurrentAccount(account: MSALAccount?) {
        self.currentAccount = account
        self.updateAccountLabel()
        self.updateSignOutButton(enabled: account != nil)
    }
```

### <a name="for-ios-only-get-additional-device-information"></a>Yalnızca iOS için ek aygıt bilgileri alın

Aygıtın paylaşılan olarak yapılandırılıp yapılandırılmadığı da dahil olmak üzere geçerli aygıt yapılandırmasını okumak için aşağıdaki kodu kullanın:

```swift
    @objc func getDeviceMode(_ sender: AnyObject) {
        
        if #available(iOS 13.0, *) {
            self.applicationContext?.getDeviceInformation(with: nil, completionBlock: { (deviceInformation, error) in
                
                guard let deviceInfo = deviceInformation else {
                    self.updateLogging(text: "Device info not returned. Error: \(String(describing: error))")
                    return
                }
                
                let isSharedDevice = deviceInfo.deviceMode == .shared
                let modeString = isSharedDevice ? "shared" : "private"
                self.updateLogging(text: "Received device info. Device is in the \(modeString) mode.")
            })
        } else {
            self.updateLogging(text: "Running on older iOS. GetDeviceInformation API is unavailable.")
        }
    }
```

### <a name="multi-account-applications"></a>Çoklu hesap uygulamaları

Bu uygulama tek bir hesap senaryosu için oluşturulmuştur. MSAL ayrıca çok hesaplı senaryoları destekler, ancak uygulamalardan bazı ek çalışma gerektirir. Kullanıcıların belirteçleri gerektiren her eylem için hangi hesabı kullanmak istediklerini seçmelerine yardımcı olmak için Kullanıcı Arabirimi oluşturmanız gerekir. Alternatif olarak, uygulamanız MSAL'daki tüm hesapları sorgulayarak hangi hesabı kullanacağınızı seçmek için sezgisel bir uygulama uygulayabilir. Örneğin, `accountsFromDeviceForParameters:completionBlock:` [bkz.](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplication.html#/c:objc(cs)MSALPublicClientApplication(im)accountsFromDeviceForParameters:completionBlock:)

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
