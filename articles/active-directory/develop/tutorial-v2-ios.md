---
title: İOS ile çalışmaya başlama-Microsoft Identity platform | Mavisi
description: İOS (Swift) uygulamaları, Microsoft Identity platform kullanarak erişim belirteçleri gerektiren bir API 'YI nasıl çağırabilir
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 07/15/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 007cc21ee8c5d45037ce1183258f939f7cc24762
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320809"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Kullanıcıları oturum açın ve iOS uygulamasından Microsoft Graph çağırın

Bu öğreticide, bir iOS uygulamasını Microsoft Identity platformu ile tümleştirmeyi öğreneceksiniz. Uygulama bir kullanıcıya oturum açacaktır, Microsoft Graph API 'sini çağırmak için bir erişim belirteci alır ve Microsoft Graph API 'sine bir istek yapar.  

Kılavuzu tamamladığınızda, uygulamanız kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) ve Azure Active Directory kullanan herhangi bir şirketten veya kuruluştan iş veya okul hesapları için oturum açma işlemlerini kabul eder.

## <a name="how-this-tutorial-works"></a>Bu öğreticinin nasıl çalıştığı

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Bu öğreticideki uygulama, kullanıcıların oturum açmasını ve adına veri almasını sağlayacaktır.  Bu verilere, yetkilendirme gerektiren ve Microsoft Identity platform tarafından korunan korumalı bir API (Bu durumda Microsoft Graph API) aracılığıyla erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanız kullanıcı tarafından bir tarayıcı veya Microsoft Authenticator aracılığıyla oturum açacaktır.
* Son Kullanıcı, uygulamanızın istediği izinleri kabul eder.
* Uygulamanıza Microsoft Graph API 'SI için bir erişim belirteci verilmeyecektir.
* Erişim belirteci, Web API 'sine HTTP isteğine dahil edilir.
* Microsoft Graph yanıtını işleyin.

Bu örnek, kimlik doğrulamasını uygulamak için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanır. MSAL, belirteçleri otomatik olarak yenileyecek, cihazdaki diğer uygulamalar arasında çoklu oturum açma (SSO) sunacaktır ve hesapları yönetir.

## <a name="prerequisites"></a>Önkoşullar

- Bu kılavuzda uygulamayı derlemek için XCode sürüm 10. x gereklidir. [İTunes web sitesi](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCODE Indirme URL")'sinden Xcode 'u indirebilirsiniz.
- Microsoft kimlik doğrulama kitaplığı ([msal. Framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Bir bağımlılık Yöneticisi kullanabilir veya kitaplığı el ile ekleyebilirsiniz. Aşağıdaki yönergelerde nasıl yapılacağı gösterilmektedir.

Bu öğretici, yeni bir proje oluşturur. Bunun yerine tamamlanan öğreticiyi indirmek isterseniz [kodu indirin](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip).

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Xcode ' u açın ve **Yeni bir Xcode projesi oluştur**' u seçin.
2. **İOS** > **tek görünüm uygulaması** ' nı seçin ve **İleri ' yi**seçin.
3. Bir ürün adı belirtin.
4. **Dili** **Swift** olarak ayarlayın ve ileri ' **yi**seçin.
5. Uygulamanızı oluşturmak için bir klasör seçin ve **Oluştur**' a tıklayın.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

1. [Azure portal](https://aka.ms/MobileAppReg)'a gidin
2. [Uygulama kayıtları dikey penceresini](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) açın ve **+ Yeni kayıt**' ye tıklayın.
3. Uygulamanız için bir **ad** girin ve yeniden yönlendirme URI 'Sini ayarlamadan **Kaydet**' e tıklayın.
4. Görüntülenen bölmenin **Yönet** bölümünde **kimlik doğrulaması**' nı seçin.
5. Yeni uygulama kayıt deneyimini açmak için ekranın üst kısmındaki **Yeni deneyimi deneyin** ' e tıklayın ve ardından **+ Yeni kayıt** >  **+ Platform** > **iOS**Ekle ' ye tıklayın.
    - Projenizin paket KIMLIĞINI girin. Kodu indirdiyseniz, bu `com.microsoft.identitysample.MSALiOS`. Kendi projenizi oluşturuyorsanız, Xcode 'da projenizi seçin ve **genel** sekmesini açın. Paket tanımlayıcısı **kimlik** bölümünde görünür.
6. Uygulamanızı `Configure` daha sonra yapılandırırken girmeniz için **iOS yapılandırma** sayfasında görüntülenen **msal yapılandırmasını** tıklatın ve kaydedin.  **Bitti**’ye tıklayın.

## <a name="add-msal"></a>MSAL Ekle

Uygulamanıza MSAL kitaplığını yüklemek için aşağıdaki yöntemlerle birini seçin:

### <a name="cocoapods"></a>CocoaPods

1. [Cocoapods](https://cocoapods.org/)kullanıyorsanız, önce, projenizin `MSAL` `.xcodeproj` dosyası ile aynı klasörde adlı `podfile` boş bir dosya oluşturarak uygulamasını yükleyebilirsiniz. Aşağıdakileri öğesine `podfile`ekleyin:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL', '~> 0.4.0'
   end
   ```

2. Projenizin `<your-target-here>` adıyla değiştirin.
3. Bir Terminal penceresinde, msal kitaplığını yüklemek için oluşturduğunuz ve çalıştırdığınız `podfile` `pod install` klasörü içeren klasöre gidin.
4. Projeyi Xcode 'da yeniden `<your project name>.xcworkspace` yüklemek için Xcode 'u kapatın ve açın.

### <a name="carthage"></a>Carthage

[Carthage](https://github.com/Carthage/Carthage)kullanıyorsanız, yüklemek `MSAL` için şunu `Cartfile`ekleyin:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

### <a name="manually"></a>El ile

Git alt modülünü de kullanabilir veya uygulamanızda bir çerçeve olarak kullanmak için en son sürümü kullanıma alabilirsiniz.

## <a name="add-your-app-registration"></a>Uygulama kaydınızı ekleyin

Daha sonra, uygulama kaydınızı kodunuza ekleyeceğiz. 

İlk olarak, aşağıdaki içeri aktarma ifadesini `ViewController.swift` ve `AppDelegate.swift` dosyalarının en üstüne ekleyin:

```swift
import MSAL
```

Ardından, aşağıdaki kodu `ViewController.swift` ' dan `viewDidLoad()`önce ekleyin:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

İçin `kClientID`atanan değeri uygulama kimliğiniz olacak şekilde değiştirin. Bu değer, uygulamayı Azure portal kaydetmek için Bu öğreticinin başındaki adım sırasında kaydettiğiniz MSAL yapılandırma verilerinin bir parçasıdır.

## <a name="configure-url-schemes"></a>URL düzenlerini yapılandırma

Bu adımda, oturum açtıktan sonra kullanıcının `CFBundleURLSchemes` uygulamaya geri yönlendirilebilmesi için kayıt yaptıracaktır. Böylece uygulamanızın Microsoft Authenticator kullanmasına `LSApplicationQueriesSchemes` de olanak tanır.

Xcode 'da, kaynak `Info.plist` kodu dosyası olarak açın ve `<dict>` bölümünün içine aşağıdakini ekleyin. `[BUNDLE_ID]` Kodu`com.microsoft.identitysample.MSALiOS`indirdiyseniz, Azure Portal kullandığınız değerle değiştirin. Kendi projenizi oluşturuyorsanız, Xcode 'da projenizi seçin ve **genel** sekmesini açın. Paket tanımlayıcısı **kimlik** bölümünde görünür.

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

## <a name="create-your-apps-ui"></a>Uygulamanızın Kullanıcı arabirimini oluşturma

Şimdi, aşağıdaki kodu `ViewController`sınıfına ekleyerek Microsoft Graph API 'sini çağırmak için bir düğme, başka bir oturum kapatmak için de bir metin görünümü ve bir çıktı görmek için bir kullanıcı arabirimi oluşturun:

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

Ardından, `ViewController` sınıfının içinde, `viewDidLoad()` yöntemini ile değiştirin:

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

## <a name="use-msal"></a>MSAL kullanma

### <a name="initialize-msal"></a>MSAL Başlat

`ViewController` Sınıfına aşağıdaki `InitMSAL` yöntemi ekleyin:

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

### <a name="handle-the-sign-in-callback"></a>Oturum açma geri aramasını işleme

`AppDelegate.swift` dosyasını açın. Oturum açtıktan sonra geri çağırma işlemini işlemek için aşağıdaki gibi `MSALPublicClientApplication.handleMSALResponse` `appDelegate` sınıfa ekleyin:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Belirteçleri al

Şimdi, uygulamanın kullanıcı arabirimi işleme mantığını uygulayabiliriz ve belirteçleri MSAL üzerinden etkileşimli olarak alabilirsiniz.

MSAL belirteçleri almak için iki birincil yöntem sunar: `acquireTokenSilently()` ve `acquireTokenInteractively()`: 

- `acquireTokenSilently()`bir hesap mevcut olduğu sürece Kullanıcı etkileşimi olmadan bir kullanıcıya oturum açma ve belirteçleri almaya çalışır.

- `acquireTokenInteractively()`Kullanıcı oturumu açmaya çalışırken her zaman Kullanıcı ARABIRIMINI gösterir. Etkileşimli bir SSO deneyimi sağlamak için tarayıcıda oturum tanımlama bilgilerini veya Microsoft Authenticator içindeki bir hesabı kullanabilir.

`ViewController` Sınıfına aşağıdaki kodu ekleyin:

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

#### <a name="get-a-token-interactively"></a>Bir belirteci etkileşimli olarak alın

Aşağıdaki kod, bir `MSALInteractiveTokenParameters` nesne oluşturup çağırarak `acquireToken`bir belirteci ilk kez alır. Daha sonra şu kodu ekleyeceksiniz:

1. Kapsamlarla oluşturulur `MSALInteractiveTokenParameters` .
2. Oluşturulan `acquireToken()` parametrelerle çağırır.
3. Hataları işler. Daha fazla ayrıntı için [iOS hata işleme Kılavuzu](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling)' na bakın.
4. Başarılı durumu işler.

`ViewController` Sınıfına aşağıdaki kodu ekleyin.

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



#### <a name="get-a-token-silently"></a>Belirteci sessizce al

Güncelleştirilmiş bir belirteci sessizce almak için, `ViewController` sınıfına aşağıdaki kodu ekleyin. Bir `MSALSilentTokenParameters` nesne oluşturur ve çağırır `acquireTokenSilent()`:

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

### <a name="call-the-microsoft-graph-api"></a>Microsoft Graph API 'sini çağırma 

Belirteciniz olduktan sonra uygulamanız bunu HTTP üstbilgisinde kullanarak Microsoft Graph yetkili bir istek yapabilir:

| üst bilgi anahtarı    | value                 |
| ------------- | --------------------- |
| Authorization | Taşıyıcı \<erişim-belirteç > |

`ViewController` Sınıfına aşağıdaki kodu ekleyin:

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

Microsoft Graph API 'SI hakkında daha fazla bilgi edinmek için bkz. [MICROSOFT Graph API](https://graph.microsoft.com) .

### <a name="use-msal-for-sign-out"></a>Oturumu kapatma için MSAL kullanma

Ardından, oturum kapatma desteği ekleyin.

> [!Important]
> MSAL ile oturum açmak, bir kullanıcıyla ilgili tüm bilinen bilgileri uygulamadan kaldırır, ancak kullanıcının cihazında etkin bir oturumu olmaya devam edecektir. Kullanıcı yeniden oturum açmayı denerse, oturum açma kullanıcı arabirimini görebilir, ancak cihaz oturumu hala etkin olduğundan kimlik bilgilerini yeniden girmeniz gerekebilir.

Oturum kapatma özelliği eklemek için, `ViewController` sınıfının içine aşağıdaki kodu ekleyin. Bu yöntem tüm hesaplar boyunca geçiş yapar ve bunları kaldırır:

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

Varsayılan olarak, MSAL, uygulamanızın belirteçlerini iOS anahtarlığınızdan önbelleğe alır. 

Belirteç önbelleğe almayı etkinleştirmek için:
1. Xcode proje ayarlarınıza gidin > **Yetenekler sekmesi** > **Anahtarlık paylaşımını etkinleştir**
2. Öğesine **+** tıklayın ve `com.microsoft.adalcache` **Anahtarlık grupları** girişi olarak girin.

### <a name="add-helper-methods"></a>Yardımcı yöntemler ekleme

Örneği gerçekleştirmek için aşağıdaki yardımcı yöntemleri `ViewController` sınıfına ekleyin:

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

### <a name="multi-account-applications"></a>Çoklu hesap uygulamaları

Bu uygulama, tek bir hesap senaryosu için oluşturulmuştur. MSAL ayrıca çoklu hesap senaryolarını destekler, ancak uygulamalardan bazı ek işler gerektirir. Kullanıcının belirteç gerektiren her eylem için hangi hesabı kullanmak istediğini belirlemek için kullanıcı ARABIRIMI oluşturmanız gerekir. Alternatif olarak, uygulamanız `getAccounts()` yöntemi aracılığıyla kullanılacak hesabı seçmek için bir buluşsal yöntem uygulayabilir.

## <a name="test-your-app"></a>Uygulamanızı test etme

### <a name="run-locally"></a>Yerel olarak çalıştırma

Uygulamayı derleyin ve bir test cihazında veya öykünücüsünde dağıtın. Azure AD veya kişisel Microsoft hesapları için oturum açabiliyor ve belirteçleri alabilmesi gerekir.

Kullanıcı uygulamanızda ilk kez oturum açtığında, bu kullanıcılara istenen izinleri onaylaması için Microsoft kimliği sorulur.  Birçok kullanıcı kabul etme yeteneğine sahip olsa da, bazı Azure AD kiracılar, yöneticilerin tüm kullanıcılar adına onay vermesini gerektiren Kullanıcı onayını devre dışı bırakmış olur. Bu senaryoyu desteklemek için, uygulamanızın kapsamlarını Azure portal kaydedin.

Oturum açtıktan sonra, uygulama Microsoft Graph `/me` uç noktasından döndürülen verileri görüntüler.

## <a name="get-help"></a>Yardım alın

Bu öğreticiyle veya Microsoft Identity platformu ile ilgili sorun yaşıyorsanız [Yardım ve destek](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) bölümünü ziyaret edin.
