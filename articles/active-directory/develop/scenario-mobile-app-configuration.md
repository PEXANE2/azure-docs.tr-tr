---
title: Web API 'Lerini çağıran mobil uygulamaları yapılandırma | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini (uygulamanın kod yapılandırması) çağıran bir mobil uygulama oluşturmayı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 06/16/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 717b6ed7e711b4db4cb15e55282cfcdea4659178
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89051347"
---
# <a name="configure-a-mobile-app-that-calls-web-apis"></a>Web API 'Lerini çağıran bir mobil uygulama yapılandırma

Uygulamanızı oluşturduktan sonra, uygulama kayıt parametrelerini kullanarak kodu nasıl yapılandıracağınızı öğreneceksiniz. Mobil uygulamalar, oluşturma çerçevesiyle ilgili bazı karmaşıklıkları sunar.

## <a name="find-msal-support-for-mobile-apps"></a>Mobil uygulamalar için MSAL desteğini bulun

Aşağıdaki Microsoft kimlik doğrulama kitaplığı (MSAL) türleri mobil uygulamaları destekler.

MSAL | Açıklama
------------ | ----------
![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Taşınabilir uygulamalar geliştirmek için kullanılır. MSAL.NET, mobil uygulama oluşturmak için aşağıdaki platformları destekler: Evrensel Windows Platformu (UWP), Xamarin. iOS ve Xamarin. Android.
![MSAL. iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL. iOS | Hedef-C veya Swift kullanarak Yerel iOS uygulamaları geliştirmek için kullanılır.
![MSAL. Android](media/sample-v2-code/logo_android.png) <br/> MSAL. Android | Android için Java 'da yerel Android uygulamaları geliştirmek için kullanılır.

## <a name="instantiate-the-application"></a>Uygulamanın örneğini oluşturma

### <a name="android"></a>Android

Mobil uygulamalar sınıfını kullanır `PublicClientApplication` . Burada örneği oluşturma:

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

İOS üzerinde mobil uygulamaların sınıfının örneğini oluşturması gerekir `MSALPublicClientApplication` . Sınıfı oluşturmak için aşağıdaki kodu kullanın.

```objc
NSError *msalError = nil;

MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

[Ek MSALPublicClientApplicationConfig özellikleri](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplicationConfig.html#/Configuration%20options) varsayılan yetkiyi geçersiz kılabilir, bir yeniden yönlendirme URI 'si BELIRTEBILIR veya msal Token Caching davranışını değiştirebilir.

### <a name="xamarin-or-uwp"></a>Xamarin veya UWP

Bu bölüm, Xamarin. iOS, Xamarin. Android ve UWP uygulamaları için uygulamanın örneğini oluşturmayı açıklamaktadır.

#### <a name="instantiate-the-application"></a>Uygulamanın örneğini oluşturma

Xamarin veya UWP 'de, uygulamayı örneketmenin en kolay yolu aşağıdaki kodu kullanmaktır. Bu kodda, `ClientId` kayıtlı uygulamanızın GUID 'sidir.

```csharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

`With<Parameter>`UI üst öğesini ayarlama, varsayılan yetkiyi geçersiz kılma, telemetri için bir istemci adı ve sürümü belirtme, yeniden yönlendirme URI 'si belirtme ve kullanılacak http fabrikasını belirleme ek yöntemler. HTTP fabrikası, örneğin, proxy 'leri işlemek ve telemetri ve günlüğe kaydetme belirtmek için kullanılabilir.

Aşağıdaki bölümlerde, uygulamanın örneğini oluşturma hakkında daha fazla bilgi sağlanmaktadır.

##### <a name="specify-the-parent-ui-window-or-activity"></a>Üst Kullanıcı arabirimini, pencereyi veya etkinliği belirtin

Android 'de, etkileşimli kimlik doğrulaması yapmadan önce üst etkinliği geçirmeniz gerekir. İOS 'ta, bir aracı kullandığınızda geçiş yapmanız gerekir `ViewController` . UWP 'de aynı şekilde, üst pencereyi geçirmek isteyebilirsiniz. Belirteci aldığınızda bu dosyayı geçirirsiniz. Ancak uygulamayı oluştururken, döndüren bir temsilci olarak geri çağırma de belirtebilirsiniz `UIParent` .

```csharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

Android 'de kullanmanızı öneririz [`CurrentActivityPlugin`](https://github.com/jamesmontemagno/CurrentActivityPlugin) . Sonuç `PublicClientApplication` Oluşturucu kodu şu örneğe benzer şekilde görünür:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="find-more-app-building-parameters"></a>Daha fazla uygulama oluşturma parametresi bulun

Üzerinde kullanılabilen tüm yöntemlerin bir listesi için `PublicClientApplicationBuilder` , bkz. [Yöntemler listesi](/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods).

İçinde gösterilen tüm seçeneklerin açıklaması için `PublicClientApplicationOptions` [başvuru belgelerine](/dotnet/api/microsoft.identity.client.publicclientapplicationoptions)bakın.

## <a name="tasks-for-xamarin-ios"></a>Xamarin iOS görevleri

Xamarin iOS üzerinde MSAL.NET kullanıyorsanız, aşağıdaki görevleri yapın.

* [İçindeki işlevi geçersiz kılın ve uygulayın `OpenUrl``AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
* [Anahtarlık gruplarını etkinleştir](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
* [Belirteç önbelleği paylaşımını etkinleştir](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
* [Anahtarlık erişimini etkinleştir](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Daha fazla bilgi için bkz. [Xamarin iOS konuları](msal-net-xamarin-ios-considerations.md).

## <a name="tasks-for-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL görevleri

İOS ve macOS için MSAL kullandığınızda bu görevler gereklidir:

* [`openURL`Geri aramayı Uygula](#brokered-authentication-for-msal-for-ios-and-macos)
* [Anahtarlık erişim gruplarını etkinleştir](howto-v2-keychain-objc.md)
* [Tarayıcıları ve WebView’ları özelleştirme](customize-webviews.md)

## <a name="tasks-for-xamarinandroid"></a>Xamarin. Android görevleri

Xamarin. Android kullanıyorsanız aşağıdaki görevleri yapın:

- [Kimlik doğrulama akışının etkileşimli kısmı bittikten sonra denetimin MSAL 'e geri gitmesini sağlama](msal-net-xamarin-android-considerations.md#ensure-that-control-returns-to-msal)
- [Android bildirimini güncelleştirme](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [Katıştırılmış Web görünümünü kullanın (isteğe bağlı)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [Gerektiğinde sorun giderin](msal-net-xamarin-android-considerations.md#troubleshooting)

Daha fazla bilgi için bkz. [Xamarin. Android konuları](msal-net-xamarin-android-considerations.md).

Android 'deki tarayıcılarla ilgili dikkat edilmesi gereken noktalar için bkz. [msal.net Ile Xamarin. Android 'e özgü hususlar](msal-net-system-browser-android-considerations.md).

#### <a name="tasks-for-uwp"></a>UWP görevleri

UWP 'de kurumsal ağları kullanabilirsiniz. Aşağıdaki bölümlerde, şirket senaryosunda gerçekleştirmeniz gereken görevler açıklanmaktadır.

Daha fazla bilgi için bkz. [msal.net Ile UWP 'e özgü hususlar](msal-net-uwp-considerations.md).

## <a name="configure-the-application-to-use-the-broker"></a>Aracıyı kullanmak için uygulamayı yapılandırma

Android ve iOS 'ta aracılar şunları etkinleştirir:

- **Çoklu oturum açma (SSO)**: Azure Active Directory (Azure AD) ile KAYıTLı cihazlarda SSO 'yu kullanabilirsiniz. SSO kullandığınızda kullanıcılarınızın her uygulamada oturum açması gerekmez.
- **Cihaz kimliği**: Bu ayar, Azure AD cihazlarıyla ilgili koşullu erişim ilkelerini sağlar. Kimlik doğrulama işlemi, cihaz çalışma alanına katıldığında oluşturulan cihaz sertifikasını kullanır.
- **Uygulama tanımlama doğrulaması**: bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir. Sonra aracı tarafından doğrulanır.

### <a name="enable-the-broker-on-xamarin"></a>Xamarin üzerinde aracıyı etkinleştirme

Xamarin üzerinde aracıyı etkinleştirmek için, `WithBroker()` yöntemini çağırdığınızda parametresini kullanın `PublicClientApplicationBuilder.CreateApplication` . Varsayılan olarak, `.WithBroker()` true olarak ayarlanır.

Xamarin. iOS için aracılı kimlik doğrulamasını etkinleştirmek üzere, bu makaledeki [Xamarin. iOS bölümündeki](#enable-brokered-authentication-for-xamarin-ios) adımları izleyin.

### <a name="enable-the-broker-for-msal-for-android"></a>Android için MSAL aracısını etkinleştirme

Android 'de bir aracısının etkinleştirilmesi hakkında daha fazla bilgi için bkz. [Android 'de aracılı kimlik doğrulaması](brokered-auth.md).

### <a name="enable-the-broker-for-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL aracısını etkinleştirme

Aracılı kimlik doğrulaması, iOS ve macOS için MSAL içindeki Azure AD senaryolarında varsayılan olarak etkinleştirilmiştir.

Aşağıdaki bölümlerde, iOS ve macOS için MSAL for Xamarin. iOS veya MSAL için aracılı kimlik doğrulama desteği için uygulamanızı yapılandırma yönergeleri sağlanmaktadır. İki yönerge kümesinde bazı adımlar farklılık gösterir.

### <a name="enable-brokered-authentication-for-xamarin-ios"></a>Xamarin iOS için aracılı kimlik doğrulamasını etkinleştir

Xamarin. iOS uygulamanızın [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) uygulamayla iletişim kurmasını sağlamak için bu bölümdeki adımları izleyin.

#### <a name="step-1-enable-broker-support"></a>1. Adım: aracı desteğini etkinleştirme

Aracı desteği varsayılan olarak devre dışıdır. Tek bir sınıf için etkinleştirebilirsiniz `PublicClientApplication` . `WithBroker()`Sınıfını aracılığıyla oluşturduğunuzda parametresini kullanın `PublicClientApplication` `PublicClientApplicationBuilder` . `WithBroker()`Parametresi varsayılan olarak true olarak ayarlanır.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>2. Adım: geri aramayı işlemek için AppDelegate 'i güncelleştirme

MSAL.NET aracı çağırdığında, aracı uygulamanıza geri çağrı yapılır. Yöntemini kullanarak geri çağırır `AppDelegate.OpenUrl` . MSAL, aracıdan gelen yanıtı beklediği için uygulamanızın MSAL.NET geri çağırmak için birlikte çalışması gerekir. `AppDelegate.cs`Aşağıdaki kodda gösterildiği gibi, yöntemini geçersiz kılmak için dosyayı güncelleştirerek bu davranışı ayarlarsınız.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url,
                             string sourceApplication,
                             NSObject annotation)
{
 if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
 {
  AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
  return true;
 }
 else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
 {
  return false;
 }
 return true;
}
```

Bu yöntem, uygulama her başlatıldığında çağrılır. Bu, aracıdan gelen yanıtı işlemek ve MSAL.NET tarafından başlatılan kimlik doğrulama işlemini tamamlamaya yönelik bir fırsattır.

#### <a name="step-3-set-a-uiviewcontroller"></a>3. Adım: UIViewController () ayarlama

Xamarin iOS için normalde bir nesne penceresi ayarlamanız gerekmez. Ancak bu durumda, bir aracıdan yanıt gönderebilmeniz ve alabilmesi için ayarlamanız gerekir. Bir nesne penceresi ayarlamak için, ' de `AppDelegate.cs` bir ayarlayın `ViewController` .

Nesne penceresini ayarlamak için şu adımları izleyin:

1. İçinde `AppDelegate.cs` , öğesini `App.RootViewController` yeni olarak ayarlayın `UIViewController()` . Bu ayar, aracı çağrısının dahil edilmesini sağlar `UIViewController` . Doğru ayarlanmamışsa şu hatayı alabilirsiniz:

    `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker."`

1. `AcquireTokenInteractive`Çağrısında öğesini kullanın `.WithParentActivityOrWindow(App.RootViewController)` . Kullanacağınız nesne penceresinin başvurusunu geçirin. Aşağıda bir örnek verilmiştir:

    `App.cs` içinde:
    ```csharp
       public static object RootViewController { get; set; }
    ```
    `AppDelegate.cs` içinde:
    ```csharp
       LoadApplication(new App());
       App.RootViewController = new UIViewController();
    ```
    `AcquireToken`Çağrıda:
    ```csharp
    result = await app.AcquireTokenInteractive(scopes)
                 .WithParentActivityOrWindow(App.RootViewController)
                 .ExecuteAsync();
    ```

#### <a name="step-4-register-a-url-scheme"></a>4. Adım: URL düzenini kaydetme

MSAL.NET, aracıyı çağırmak için URL 'Ler kullanır ve ardından aracı yanıtını uygulamanıza geri döndürür. Gidiş dönüş işleminin tamamlanabilmesi için, uygulamanızın URL şemasını `Info.plist` dosyaya kaydedin.

Uygulamanızın URL şemasını kaydetmek için şu adımları izleyin:

1. İle ön ek `CFBundleURLSchemes` `msauth` .
1. `CFBundleURLName`Sonuna ekleyin. Bu düzene uyun:

   `$"msauth.(BundleId)"`

   Burada, `BundleId` cihazınızı benzersiz olarak tanımlar. Örneğin, ise, `BundleId` `yourcompany.xforms` URL şemanızın olması gerekir `msauth.com.yourcompany.xforms` .

   > [!NOTE]
   > Bu URL şeması, aracının yanıtını aldığında uygulamanızı benzersiz bir şekilde tanımlayan yeniden yönlendirme URI 'sinin bir parçası olur.

   ```XML
    <key>CFBundleURLTypes</key>
       <array>
         <dict>
           <key>CFBundleTypeRole</key>
           <string>Editor</string>
           <key>CFBundleURLName</key>
           <string>com.yourcompany.xforms</string>
           <key>CFBundleURLSchemes</key>
           <array>
             <string>msauth.com.yourcompany.xforms</string>
           </array>
         </dict>
       </array>
   ```

#### <a name="step-5-add-to-the-lsapplicationqueriesschemes-section"></a>5. Adım: Lsapplicationqueriesdüzenlerinin bölümüne ekleme

MSAL, `–canOpenURL:` aracının cihaza yüklenip yüklenmediğini denetlemek için kullanır. İOS 9 ' da, Apple bir uygulamanın sorgulayabilme düzenlerini kilitlemiş.

`msauthv2` `LSApplicationQueriesSchemes` `Info.plist` Aşağıdaki kod örneğinde olduğu gibi, dosyanın bölümüne ekleyin:

```XML
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL için aracılı kimlik doğrulaması

Aracılı kimlik doğrulaması, varsayılan olarak Azure AD senaryolarında etkindir.

#### <a name="step-1-update-appdelegate-to-handle-the-callback"></a>1. Adım: geri aramayı işlemek için AppDelegate 'i güncelleştirme

İOS ve macOS için MSAL, aracıyı çağırdığında, aracı yöntemini kullanarak uygulamanıza geri çağrı yapılır `openURL` . MSAL, aracıdan gelen yanıtı beklediği için, uygulamanızın MSAL geri çağırmak için birlikte çalışması gerekir. `AppDelegate.m`Aşağıdaki kod örnekleri gösterdiği gibi, yöntemini geçersiz kılmak için dosyayı güncelleştirerek bu özelliği ayarlayın.

```objc
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
    return [MSALPublicClientApplication handleMSALResponse:url
                                         sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {

        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }

        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

> [!NOTE]
> `UISceneDelegate`İOS 13 veya sonraki bir sürümünü benimsediğiniz takdirde, msal geri aramasını `scene:openURLContexts:` yerine öğesinin içine yerleştirin `UISceneDelegate` . MSAL `handleMSALResponse:sourceApplication:` her URL için yalnızca bir kez çağrılmalıdır.
>
> Daha fazla bilgi için [Apple belgelerine](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)bakın.

#### <a name="step-2-register-a-url-scheme"></a>2. Adım: bir URL düzenini kaydetme

İOS ve macOS için MSAL, aracıyı çağırmak ve ardından aracı yanıtını uygulamanıza döndürmek için URL 'Ler kullanır. Gidiş dönüş işleminin tamamlanabilmesi için, dosyaya uygulamanız için bir URL düzeni kaydedin `Info.plist` .

Uygulamanıza bir düzen kaydetmek için:

1. Özel URL düzeninizi ile önek yapın `msauth` .

1. Şemanızın sonuna paket tanımlayıcıyı ekleyin. Bu düzene uyun:

   `$"msauth.(BundleId)"`

   Burada, `BundleId` cihazınızı benzersiz olarak tanımlar. Örneğin, ise, `BundleId` `yourcompany.xforms` URL şemanızın olması gerekir `msauth.com.yourcompany.xforms` .

   > [!NOTE]
   > Bu URL şeması, aracının yanıtını aldığında uygulamanızı benzersiz bir şekilde tanımlayan yeniden yönlendirme URI 'sinin bir parçası olur. Biçimdeki yeniden yönlendirme URI 'sinin `msauth.(BundleId)://auth` [Azure Portal](https://portal.azure.com)uygulamanız için kayıtlı olduğundan emin olun.

   ```XML
   <key>CFBundleURLTypes</key>
   <array>
       <dict>
           <key>CFBundleURLSchemes</key>
           <array>
               <string>msauth.[BUNDLE_ID]</string>
           </array>
       </dict>
   </array>
   ```

#### <a name="step-3-add-lsapplicationqueriesschemes"></a>3. Adım: Lsapplicationqueriesşemalarını ekleme

`LSApplicationQueriesSchemes`Yüklenmişse Microsoft Authenticator uygulamasına çağrılara izin vermek için ekleyin.

> [!NOTE]
> `msauthv3`Uygulamanızın Xcode 11 ve üzeri kullanılarak derlenmesi durumunda düzen gereklidir.

Ekleme hakkında bir örnek aşağıda verilmiştir `LSApplicationQueriesSchemes` :

```XML
<key>LSApplicationQueriesSchemes</key>
<array>
  <string>msauthv2</string>
  <string>msauthv3</string>
</array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>Xamarin. Android için aracılı kimlik doğrulaması

Android 'de bir aracısının etkinleştirilmesi hakkında daha fazla bilgi için bkz. [Xamarin. Android üzerinde aracılı kimlik doğrulaması](msal-net-use-brokers-with-xamarin-apps.md#brokered-authentication-for-android).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirteç alma](scenario-mobile-acquire-token.md)