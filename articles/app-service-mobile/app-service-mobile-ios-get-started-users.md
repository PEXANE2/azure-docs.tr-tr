---
title: iOS'ta Kimlik Doğrulama Ekleme
description: AAD, Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcıları aracılığıyla iOS uygulamanızın kullanıcılarının kimliğini doğrulamak için Azure Mobil Uygulamalarını nasıl kullanacağınızı öğrenin.
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: fd7860053e8c04ca9d5e355a721afd834835a441
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459032"
---
# <a name="add-authentication-to-your-ios-app"></a>iOS uygulamanıza kimlik doğrulama ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Bu öğreticide, desteklenen bir kimlik sağlayıcısını kullanarak [iOS hızlı başlatma] projesine kimlik doğrulama eklersiniz. Bu öğretici, önce tamamlamanız gereken [iOS hızlı başlangıç] öğreticisini temel alınr.

## <a name="register-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Uygulamanızı kimlik doğrulama için kaydedin ve Uygulama Hizmetini yapılandırın
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><a name="redirecturl"></a>Uygulamanızı İzin Verilen Harici Yönlendirme URL'lerine ekleyin

Güvenli kimlik doğrulaması, uygulamanız için yeni bir URL düzeni tanımlamanızı gerektirir.  Bu, kimlik doğrulama işlemi tamamlandıktan sonra kimlik doğrulama sisteminin uygulamanıza geri yönlendirmesine olanak tanır.  Bu öğreticide, URL şeması _ekadını_ sonuna kadar kullanırız.  Ancak, seçtiğiniz herhangi bir URL düzenini kullanabilirsiniz.  Mobil uygulamanıza özgü olmalıdır.  Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. Azure [portalında]Uygulama Hizmetinizi seçin.

2. Kimlik **Doğrulama / Yetkilendirme** menüsü seçeneğini tıklatın.

3. **Kimlik Doğrulama Sağlayıcıları** bölümünde Azure Etkin **Dizini'ni** tıklatın.

4. Yönetim **modunu** **Gelişmiş**olarak ayarlayın.

5. İzin **Verilen Harici Yönlendirme URL'lerine**girin. `appname://easyauth.callback`  Bu dizedeki _ad,_ mobil uygulamanızın URL Düzeni'dir.  Bir protokol için normal URL belirtimini izlemeli (yalnızca harfleri ve sayıları kullanın ve harfle başlamalıdır).  Mobil uygulama kodunuzu URL Düzeni ile birkaç yerde ayarlamanız gerektiğinden, seçtiğiniz dizeye not alabilirsiniz.

6. **Tamam**'a tıklayın.

7. **Kaydet**'e tıklayın.

## <a name="restrict-permissions-to-authenticated-users"></a><a name="permissions"></a>İzinleri kimlik doğrulaması verilen kullanıcılarla sınırlama
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Xcode'da uygulamayı başlatmak için **Çalıştır'a** basın. Uygulama, kimliği doğrulanmamış bir kullanıcı olarak arka uça erişmeye çalıştığından, ancak *TodoItem* tablosu artık kimlik doğrulaması gerektirdiğinden bir özel durum yükseltilir.

## <a name="add-authentication-to-app"></a><a name="add-authentication"></a>Uygulamaya kimlik doğrulama ekleme
**Amaç-C**:

1. Mac'inizde *Xcode'da QSTodoListViewController.m'yi* açın ve aşağıdaki yöntemi ekleyin:

    ```Objective-C
    - (void)loginAndGetData
    {
        QSAppDelegate *appDelegate = (QSAppDelegate *)[UIApplication sharedApplication].delegate;
        appDelegate.qsTodoService = self.todoService;

        [self.todoService.client loginWithProvider:@"google" urlScheme:@"appname" controller:self animated:YES completion:^(MSUser * _Nullable user, NSError * _Nullable error) {
            if (error) {
                NSLog(@"Login failed with error: %@, %@", error, [error userInfo]);
            }
            else {
                self.todoService.client.currentUser = user;
                NSLog(@"User logged in: %@", user.userId);

                [self refresh];
            }
        }];
    }
    ```

    *Google'ı microsoftaccount,* *twitter,* *facebook*veya *windowsazureactivedirectory* olarak *değiştirin.* Facebook kullanıyorsanız, uygulamanızda [Facebook alan adlarını beyaz listeye][1] almalısınız.

    **urlScheme'i** uygulamanız için benzersiz bir adla değiştirin.  UrlScheme, Azure portalındaki İzin Verilen Harici Yönlendirme **URL'leri** alanında belirttiğiniz URL Düzeni protokolüyle aynı olmalıdır. UrlScheme, kimlik doğrulama isteği tamamlandıktan sonra uygulamanıza geri dönmek için kimlik doğrulama geri araması tarafından kullanılır.

2. *QSTodoListViewController.m'de* aşağıdaki kodla değiştirin: `[self refresh]` `viewDidLoad`

    ```Objective-C
    [self loginAndGetData];
    ```

3. `QSAppDelegate.h` dosyasını açın ve aşağıdaki kodu ekleyin:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. `QSAppDelegate.m` dosyasını açın ve aşağıdaki kodu ekleyin:

    ```Objective-C
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
    {
        if ([[url.scheme lowercaseString] isEqualToString:@"appname"]) {
            // Resume login flow
            return [self.qsTodoService.client resumeWithURL:url];
        }
        else {
            return NO;
        }
    }
    ```

   Bu kodu satır okumadan `#pragma mark - Core Data stack`önce doğrudan ekleyin.  _Appname'yi_ adım 1'de kullandığınız urlScheme değeriyle değiştirin.

5. Dosyayı `AppName-Info.plist` açın (AppName'yi uygulamanızın adıyla değiştirin) ve aşağıdaki kodu ekleyin:

    ```XML
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    Bu kod öğenin `<dict>` içine yerleştirilmelidir.  _Uygulama_ dizesini **(CFBundleURLSchemes**için dizi içinde) adım 1'de seçtiğiniz uygulama adıile değiştirin.  Ayrıca bu değişiklikleri plist düzenleyicisinde de yapabilirsiniz `AppName-Info.plist` - plist düzenleyicisini açmak için XCode'daki dosyaya tıklayın.

    `com.microsoft.azure.zumo` **CFBundleURLName** dizesini Apple paket tanımlayıcınızla değiştirin.

6. Uygulamayı başlatmak için *Çalıştır'a* basın ve ardından oturum açın. Oturum açtığınızda, Todo listesini görüntüleyebilmeli ve güncelleştirmeler yapabilmelisiniz.

**Hızlı :**

1. Mac'inizde *Xcode'da ToDoTableViewController.swift'i* açın ve aşağıdaki yöntemi ekleyin:

    ```swift
    func loginAndGetData() {

        guard let client = self.table?.client, client.currentUser == nil else {
            return
        }

        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        appDelegate.todoTableViewController = self

        let loginBlock: MSClientLoginBlock = {(user, error) -> Void in
            if (error != nil) {
                print("Error: \(error?.localizedDescription)")
            }
            else {
                client.currentUser = user
                print("User logged in: \(user?.userId)")
            }
        }

        client.login(withProvider:"google", urlScheme: "appname", controller: self, animated: true, completion: loginBlock)

    }
    ```

    *Google'ı microsoftaccount,* *twitter,* *facebook*veya *windowsazureactivedirectory* olarak *değiştirin.* Facebook kullanıyorsanız, uygulamanızda [Facebook alan adlarını beyaz listeye][1] almalısınız.

    **urlScheme'i** uygulamanız için benzersiz bir adla değiştirin.  UrlScheme, Azure portalındaki İzin Verilen Harici Yönlendirme **URL'leri** alanında belirttiğiniz URL Düzeni protokolüyle aynı olmalıdır. UrlScheme, kimlik doğrulama isteği tamamlandıktan sonra uygulamanıza geri dönmek için kimlik doğrulama geri araması tarafından kullanılır.

2. Satırları `self.refreshControl?.beginRefreshing()` kaldırın `self.onRefresh(self.refreshControl)` ve `viewDidLoad()` *ToDoTableViewController.swift*sonunda . Onların yerine `loginAndGetData()` bir çağrı ekleyin:

    ```swift
    loginAndGetData()
    ```

3. Dosyayı `AppDelegate.swift` açın ve `AppDelegate` sınıfa aşağıdaki satırı ekleyin:

    ```swift
    var todoTableViewController: ToDoTableViewController?

    func application(_ application: UIApplication, openURL url: NSURL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.scheme?.lowercased() == "appname" {
            return (todoTableViewController!.table?.client.resume(with: url as URL))!
        }
        else {
            return false
        }
    }
    ```

    _Appname'yi_ adım 1'de kullandığınız urlScheme değeriyle değiştirin.

4. Dosyayı `AppName-Info.plist` açın (AppName'yi uygulamanızın adıyla değiştirin) ve aşağıdaki kodu ekleyin:

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    Bu kod öğenin `<dict>` içine yerleştirilmelidir.  _Uygulama_ dizesini **(CFBundleURLSchemes**için dizi içinde) adım 1'de seçtiğiniz uygulama adıile değiştirin.  Ayrıca bu değişiklikleri plist düzenleyicisinde de yapabilirsiniz `AppName-Info.plist` - plist düzenleyicisini açmak için XCode'daki dosyaya tıklayın.

    `com.microsoft.azure.zumo` **CFBundleURLName** dizesini Apple paket tanımlayıcınızla değiştirin.

5. Uygulamayı başlatmak için *Çalıştır'a* basın ve ardından oturum açın. Oturum açtığınızda, Todo listesini görüntüleyebilmeli ve güncelleştirmeler yapabilmelisiniz.

Uygulama Hizmeti Kimlik Doğrulaması, Apple'ların Uygulamalar Arası İletişimini kullanır.  Bu konuda daha fazla bilgi için [Apple Dokümantasyon][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure portalında]: https://portal.azure.com

[iOS hızlı başlangıç]: app-service-mobile-ios-get-started.md

