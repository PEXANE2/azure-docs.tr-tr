---
title: İOS - Microsoft kimlik platformu ile çalışmaya başlama | Azure
description: İOS (Swift) uygulamalar bir API, nasıl arayabilir, erişim belirteçleri kullanarak Microsoft kimlik platformu gerektirir
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.topic: tutorial
ms.workload: identity
ms.date: 07/15/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 910069ab89cef18794e637b6bfbbc57fb732871c
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67872083"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Kullanıcılar oturum ve bir iOS uygulamasından Microsoft Graph'i çağırmaya

Bu öğreticide, bir iOS uygulamasını Microsoft kimlik platformu ile tümleştirmeyi öğreneceksiniz. Uygulama bir kullanıcının oturumunu, Microsoft Graph API'sini çağırmak için erişim belirteci almak ve Microsoft Graph API için bir istek oluşturun.  

Kılavuzu tamamladıktan sonra uygulamanızın oturum açma işlemleri kişisel Microsoft hesabı (outlook.com, live.com ve diğerleri dahil) kabul eder ve herhangi bir şirket veya Azure Active Directory kullanan kuruluş iş veya Okul hesapları.

## <a name="how-this-tutorial-works"></a>Bu öğreticide nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Bu öğreticide uygulama kullanıcılarının oturumunu ve onların adına veri alın.  Bu veriler, yetkilendirme gerektirir ve Microsoft kimlik platformu tarafından korunan korumalı bir API (Bu durumda Microsoft Graph API) aracılığıyla erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanızı Microsoft Authenticator ya da bir tarayıcı aracılığıyla kullanıcının oturum açmasını.
* Son kullanıcının, uygulamanın istediği izinleri kabul eder.
* Uygulamanızı Microsoft Graph API'si için bir erişim belirteci verilir.
* HTTP isteği Web API'sine erişim belirtecinin dahil edilir.
* Microsoft Graph yanıta işler.

Bu örnek, kimlik doğrulaması uygulamak için Microsoft Authentication library (MSAL) kullanır. MSAL otomatik olarak yenileme belirteçleri, çoklu oturum açma (SSO) arasında cihazdaki diğer uygulamalar sunun ve hesapları yönetin.

## <a name="prerequisites"></a>Önkoşullar

- XCode sürümü 10.x, bu kılavuzdaki uygulama oluşturmak için gereklidir. Xcode'dan indirebileceğiniz [iTunes Web sitesi](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode indirme URL'si").
- Microsoft kimlik doğrulama kitaplığı ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Bir bağımlılık Yöneticisi'ni kullanın ya da kitaplık el ile ekleyin. Aşağıdaki yönergeler size yol gösterecektir.

Bu öğreticide, yeni bir proje oluşturur. Tamamlanan öğretici bunun yerine, karşıdan yüklemek isterseniz [kodu indirmek](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip).

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Xcode açıp seçin **yeni bir Xcode projesi oluştur**.
2. Seçin **iOS** > **tek görünüm uygulaması** seçip **sonraki**.
3. Bir ürün adı belirtin.
4. Ayarlama **dil** için **Swift** seçip **sonraki**.
5. Uygulamanızı oluşturun ve'ı tıklatın, bir klasör seçin **Oluştur**.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

1. [Azure portal](https://aka.ms/MobileAppReg)'a gidin
2. Açık [uygulama kayıtları dikey penceresinde](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) tıklatıp **+ yeni kaydı**.
3. Girin bir **adı** uygulamanız için ve daha sonra bir yeniden yönlendirme URI'si ayarı olmadan **kaydetme**.
4. İçinde **Yönet** görünürse, bölmenin seçin **kimlik doğrulaması**.
5. Tıklayın **yeni deneyimi görmek** yeni uygulama kaydı deneyimini açın ve ardından ekranın üst **+ yeni kaydı** >  **+platformEkle**  >  **iOS**.
    - Proje paket kimliğini girin. Kodu, bu ise `com.microsoft.identitysample.MSALiOS`. Kendi proje oluşturuyorsanız, Xcode ve açık projenizi seçin **genel** sekmesi. Paket tanımlayıcısı görünür **kimlik** bölümü.
6. Tıklayın `Configure` kaydedip **MSAL yapılandırma** , görünür **iOS yapılandırma** uygulamanızı daha sonra yapılandırdığınızda girebilmeniz için sayfa.  **Bitti**’ye tıklayın.

## <a name="add-msal"></a>MSAL ekleyin

Uygulamanızda MSAL kitaplığını yüklemek için aşağıdaki yöntemlerden birini seçin:

### <a name="cocoapods"></a>CocoaPods

1. Kullanıyorsanız [CocoaPods](https://cocoapods.org/), yükleme `MSAL` ilk boş bir dosya oluşturarak adlı `podfile` projenizin aynı klasörde `.xcodeproj` dosya. Ekleyin `podfile`:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL', '~> 0.4.0'
   end
   ```

2. Değiştirin `<your-target-here>` projenizin adına sahip.
3. Bir terminal penceresinde içeren klasöre gidin `podfile` oluşturup çalıştırdınız `pod install` MSAL kitaplığını yüklemek için.
4. Xcode kapatıp açmanız `<your project name>.xcworkspace` Xcode projesinde yeniden yüklemek için.

### <a name="carthage"></a>Carthage

Kullanıyorsanız [Carthage](https://github.com/Carthage/Carthage), yükleme `MSAL` ekleyerek, `Cartfile`:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

### <a name="manually"></a>El ile

Git alt modülüdür kullanın veya uygulamanızdaki bir çerçeve olarak kullanılacak en son sürüm göz atın.

## <a name="add-your-app-registration"></a>Uygulama kaydınızı Ekle

Ardından, uygulama kaydınızı kodunuza ekleyeceğiz. 

İlk olarak, en üst kısmına aşağıdaki içeri aktarma deyimini ekleyin `ViewController.swift` ve `AppDelegate.swift` dosyaları:

```swift
import MSAL
```

Ardından aşağıdaki kodu ekleyin `ViewController.swift` öncesinde `viewDidLoad()`:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

Atanan değer değiştirme `kClientID`uygulama kimliği olması Bu değeri Azure Portal'da uygulamayı kaydetmek için bu öğreticinin başında bir adımı sırasında kaydedilir MSAL yapılandırma verilerini bir parçasıdır.

## <a name="configure-url-schemes"></a>URL şemaları yapılandırın

Bu adımda, kaydedeceksiniz `CFBundleURLSchemes` böylece kullanıcı oturum açma sonra uygulamaya geri yönlendirilebilir. Bu arada `LSApplicationQueriesSchemes` ayrıca Microsoft Authenticator'ı kullanmak uygulamanızı sağlar.

Xcode'da açın `Info.plist` kaynak kodu dosyasını bulun ve içine aşağıdaki `<dict>` bölümü. Değiştirin `[BUNDLE_ID]` kod indirdiyseniz olan Azure portalında kullandığınız değerle `com.microsoft.identitysample.MSALiOS`. Kendi proje oluşturuyorsanız, Xcode ve açık projenizi seçin **genel** sekmesi. Paket tanımlayıcısı görünür **kimlik** bölümü.

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
    <string>msauth</string>
    <string>msauthv2</string>
</array>
```

## <a name="create-your-apps-ui"></a>Uygulamanızın kullanıcı Arabirimi oluşturma

Şimdi, oturum açmak için başka bir Microsoft Graph API'sini çağırmak için bir düğme içeren bir kullanıcı Arabirimi oluşturun ve bir metin görüntülemek için aşağıdaki kodu ekleyerek bazı çıktıyı görmek için `ViewController`sınıfı:

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

Ayrıca içinde sonraki `ViewController` sınıfı, yerine `viewDidLoad()` yöntemiyle:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }
```

## <a name="use-msal"></a>MSAL kullanın

### <a name="initialize-msal"></a>MSAL Başlat

Aşağıdaki `InitMSAL` yönteme `ViewController` sınıfı:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.loggingText.text = "Unable to create authority URL"
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
    }
```

### <a name="handle-the-sign-in-callback"></a>Tanıtıcı oturum geri çağırma

`AppDelegate.swift` dosyasını açın. Oturum açma işleminden sonra geri çağırma işlemek için ekleme `MSALPublicClientApplication.handleMSALResponse` için `appDelegate` şöyle sınıfı:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Belirteç Al

Şimdi biz uygulamanın UI işleme mantığı uygulamak ve belirteçleri MSAL etkileşimli olarak alın.

MSAL, belirteç almaya yönelik başlıca iki yöntem sunar: `acquireTokenSilently()` ve `acquireTokenInteractively()`: 

- `acquireTokenSilently()` bir kullanıcının oturum açmasını ve bir hesap var olduğu sürece herhangi bir kullanıcı etkileşimi olmadan belirteçleri almak çalışır.

- `acquireTokenInteractively()` her zaman kullanıcı Arabirimi, kullanıcının oturum açmasını çalışırken gösterir. SSO etkileşimli deneyimi sağlamak için tarayıcıda oturum tanımlama bilgileri veya Microsoft Authenticator'ı bir hesabı kullanabilirsiniz.

Aşağıdaki kodu ekleyin `ViewController` sınıfı:

```swift
    @objc func callGraphAPI(_ sender: UIButton) {
        
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

#### <a name="get-a-token-interactively"></a>Etkileşimli bir belirteç Al

Aşağıdaki kod, oluşturarak ilk kez bir belirteç alır bir `MSALInteractiveTokenParameters` nesne ve arama `acquireToken`. İleri, kod ekleyecek:

1. Oluşturur `MSALInteractiveTokenParameters` kapsamları ile.
2. Çağrıları `acquireToken()` oluşturulan parametrelere sahip.
3. Hatalarını ele alır. Daha fazla ayrıntı için başvurmak [iOS hata işleme Kılavuzu](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling).
4. Başarılı durumu işler.

Aşağıdaki kodu ekleyin `ViewController` sınıfı.

```swift
    func acquireTokenInteractively() {
   
        guard let applicationContext = self.applicationContext else { return }
     // #1    
        let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
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



#### <a name="get-a-token-silently"></a>Sessiz bir belirteç Al

Sessiz bir şekilde güncelleştirilmiş bir belirteci almak için aşağıdaki kodu ekleyin. `ViewController` sınıfı. Oluşturur bir `MSALSilentTokenParameters` nesne ve çağrıları `acquireTokenSilent()`:

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

### <a name="call-the-microsoft-graph-api"></a>Microsoft Graph API çağırma 

Bir belirteci aldıktan sonra uygulamanızı, HTTP üst bilgisinde Microsoft Graph için yetkili bir isteği yapmak için kullanabilirsiniz:

| Üstbilgi anahtarı    | value                 |
| ------------- | --------------------- |
| Authorization | Taşıyıcı \<erişim belirteci > |

Aşağıdaki kodu ekleyin `ViewController` sınıfı:

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

Bkz: [Microsoft Graph API](https://graph.microsoft.com) Microsoft Graph API hakkında daha fazla bilgi edinmek için.

### <a name="use-msal-for-sign-out"></a>Kullanmak için MSAL oturum kapatma

Ardından, için destek ekleme oturum kapatma.

> [!Important]
> MSAL ile oturumunuzu bir kullanıcı ile ilgili tüm bilinen bilgileri uygulamadan kaldırır, ancak kullanıcı kendi cihazında hala etkin bir oturuma sahip olur. Kullanıcının oturum açmak çalışırsa yeniden, oturum açma kullanıcı Arabirimi görebilirsiniz, ancak cihaz oturumu hala etkin olduğu için kimlik bilgilerini yeniden girmeniz gerekmez.

Oturum kapatma özelliği eklemek için aşağıdaki kodu ekleyin. `ViewController` sınıfı. Bu yöntem, tüm hesapları arasında geçiş yapar ve bunları kaldırır:

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache */
            
            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signOutButton.isEnabled = false
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Belirteç önbelleğe almayı etkinleştir

Varsayılan olarak, uygulamanızın belirteçleri iOS Anahtarlıkta MSAL önbelleğe alır. 

Belirteç önbelleğe almayı etkinleştirmek için:
1. Xcode proje ayarlarınızın gidin > **Özellikleri sekmesi** > **Anahtarlık paylaşımını etkinleştir**
2. Tıklayın **+** girin `com.microsoft.adalcache` olarak bir **Anahtarlık grupları** girişi.

### <a name="add-helper-methods"></a>Yardımcı yöntemler ekler

Aşağıdaki yardımcı yöntemini ekleyin `ViewController` sınıfı örneği tamamlamak için:

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

### <a name="multi-account-applications"></a>Birden çok hesap uygulamalar

Bu uygulama, tek bir hesap senaryo için oluşturulmuştur. MSAL birden çok hesap senaryoları da destekler, ancak bazı ek işleri uygulamalardan gerektirir. Belirteçleri gerektiren her eylem için kullanmak istediğiniz hesabı seçin kullanıcının yardımcı olmak için kullanıcı Arabirimi oluşturmanız gerekir. Alternatif olarak, uygulamanızı aracılığıyla kullanacağınız hesabı seçmek için bir buluşsal yöntem uygulayabilirsiniz `getAccounts()` yöntemi.

## <a name="test-your-app"></a>Uygulamanızı test etme

### <a name="run-locally"></a>Yerel olarak çalıştırma

Oluşturun ve bir test cihaza veya öykünücüye uygulamayı dağıtın. Oturum açmak ve belirteçleri almak için Azure AD veya kişisel olmalıdır Microsoft hesapları.

İlk kez bir kullanıcı oturum açtığında, uygulamanıza, Microsoft kimlik tarafından istenen izinleri kabul istenir.  Çoğu kullanıcı tümleştirip özelliğine sahip olsa da, bazı Azure AD kiracılarıyla yöneticilerin tüm kullanıcılar adına onay vermesini gerektiren kullanıcı onayı devre dışı bırakıldı. Bu senaryoyu desteklemek için Azure Portal'da uygulamanızın kapsamları kaydedin.

Oturum açtıktan sonra uygulamayı Microsoft Graph'teki döndürülen veriler görüntüler `/me` uç noktası.

## <a name="get-help"></a>Yardım alın

Ziyaret [Yardım ve Destek](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) Bu öğretici veya Microsoft kimlik platformu ile bir sorun varsa.
