---
title: Web API'lerini arayan mobil uygulamaları yapılandırma | Azure
titleSuffix: Microsoft identity platform
description: Web API'lerini (uygulamanın kod yapılandırması) çağıran bir mobil uygulamayı nasıl oluşturacağa öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/23/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 06475b53acf6c9161888e29723feab9cdc4336d5
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80882735"
---
# <a name="configure-a-mobile-app-that-calls-web-apis"></a>Web API'lerini çağıran bir mobil uygulamayı yapılandırma

Uygulamanızı oluşturduktan sonra, uygulama kayıt parametrelerini kullanarak kodu nasıl yapılandıracağınızöğrenilir. Mobil uygulamalar, oluşturma çerçevelerine uyum sağlamakla ilgili bazı karmaşıklıklar sunar.

## <a name="find-msal-support-for-mobile-apps"></a>Mobil uygulamalar için MSAL desteği bulun

Aşağıdaki Microsoft Kimlik Doğrulama Kitaplığı (MSAL) türleri mobil uygulamaları destekler.

MSAL | Açıklama
------------ | ----------
![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Taşınabilir uygulamalar geliştirmek için kullanılır. MSAL.NET bir mobil uygulama oluşturmak için aşağıdaki platformları destekler: Evrensel Windows Platformu (UWP), Xamarin.iOS ve Xamarin.Android.
![MSAL.iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL.iOS | Objective-C veya Swift kullanarak yerel iOS uygulamalarını geliştirmek için kullanılır.
![MSAL. Android](media/sample-v2-code/logo_android.png) <br/> MSAL. Android | Android için Java'da yerel Android uygulamaları geliştirmek için kullanılır.

## <a name="instantiate-the-application"></a>Uygulamayı anında

### <a name="android"></a>Android

Mobil uygulamalar `PublicClientApplication` sınıfı kullanır. Bunu şu şekilde anlayabilirsiniz:

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

iOS'taki mobil uygulamaların `MSALPublicClientApplication` sınıfı anlık olarak anması gerekir. Sınıfı anında kullanmak için aşağıdaki kodu kullanın. 

```objc
NSError *msalError = nil;
     
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

[Ek MSALPublicClientApplicationConfig özellikleri](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplicationConfig.html#/Configuration%20options) varsayılan yetkigeçersiz, yeniden yönlendirme URI belirtebilir veya MSAL belirteç önbelleğe alma davranışını değiştirebilirsiniz. 

### <a name="xamarin-or-uwp"></a>Xamarin veya UWP

Bu bölümde Xamarin.iOS, Xamarin.Android ve UWP uygulamaları için uygulamanın anlık olarak nasıl açIleniz.

#### <a name="instantiate-the-application"></a>Uygulamayı anında

Xamarin veya UWP'de, uygulamayı anında kullanmanın en basit yolu aşağıdaki kodu kullanmaktır. Bu kodda, `ClientId` kayıtlı uygulamanızın GUID'i yer alan bir durumdur.

```csharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

Ek `With<Parameter>` yöntemler Kullanıcı Arabirimi üst öğesini ayarlar, varsayılan yetkisini geçersiz kılar, telemetri için bir istemci adı ve sürüm belirtir, yeniden yönlendiren URI belirtir ve kullanmak üzere HTTP fabrikasını belirtir. ÖRNEĞIN, HTTP fabrikası yakınlıkları işlemek ve telemetri ve günlüğe kaydetmeyi belirtmek için kullanılabilir. 

Aşağıdaki bölümler, uygulamanın anlık olarak uygulanması hakkında daha fazla bilgi sağlar.

##### <a name="specify-the-parent-ui-window-or-activity"></a>Üst kullanıcı kullanıcı yı, pencereyi veya etkinliği belirtin

Android'de, etkileşimli kimlik doğrulamasını yapmadan önce üst etkinliği geçirmeniz gerekir. iOS'ta, bir broker kullandığınızda, geçiş `ViewController`yapmanız gerekir. UWP'de de aynı şekilde, ana pencereden geçmek isteyebilirsiniz. Jetonu aldığınızda ona geçersiniz. Ancak uygulamayı oluştururken, geri `UIParent`dönen bir temsilci olarak geri arama da belirtebilirsiniz.

```csharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

Android'de, [`CurrentActivityPlugin`](https://github.com/jamesmontemagno/CurrentActivityPlugin).. Ortaya çıkan `PublicClientApplication` oluşturucu kodu aşağıdaki örnek gibi görünür:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="find-more-app-building-parameters"></a>Daha fazla uygulama oluşturma parametreleri bulun

Kullanılabilir tüm yöntemlerin listesi için `PublicClientApplicationBuilder`Yöntemler [listesine](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods)bakın.

Açıkta kalan `PublicClientApplicationOptions`tüm seçeneklerin açıklaması için [başvuru belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions)bakın.

## <a name="tasks-for-xamarin-ios"></a>Xamarin iOS için görevler

Xamarin iOS'ta MSAL.NET kullanıyorsanız, aşağıdaki görevleri yapın.

* [Geçersiz kılın ve `OpenUrl` işlevi uygulayın`AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
* [Anahtarlık gruplarını etkinleştirme](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
* [Belirteç önbelleği paylaşımını etkinleştirme](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
* [Anahtarlık erişimini etkinleştirme](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Daha fazla bilgi için [Xamarin iOS ile ilgili konulara](msal-net-xamarin-ios-considerations.md)bakın.

## <a name="tasks-for-msal-for-ios-and-macos"></a>iOS ve macOS için MSAL görevleri

iOS ve macOS için MSAL kullandığınızda bu görevler gereklidir:

* [Geri `openURL` aramayı uygulayın](#brokered-authentication-for-msal-for-ios-and-macos)
* [Anahtar zinciri erişim gruplarını etkinleştirme](howto-v2-keychain-objc.md)
* [Tarayıcıları ve WebView’ları özelleştirme](customize-webviews.md)

## <a name="tasks-for-xamarinandroid"></a>Xamarin.Android için görevler

Xamarin.Android kullanıyorsanız, aşağıdaki görevleri yapın:

- [Kimlik doğrulama akışının etkileşimli bölümü sona erdikten sonra kontrolün MSAL'a geri dönmesini sağlayın](msal-net-xamarin-android-considerations.md#ensure-that-control-returns-to-msal)
- [Android manifestosunun güncelleştirin](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [Katıştırılmış web görünümünü kullanma (isteğe bağlı)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [Gerektiğinde sorun giderme](msal-net-xamarin-android-considerations.md#troubleshoot)

Daha fazla bilgi için [Xamarin.Android hususları](msal-net-xamarin-android-considerations.md)için bkz.

Android tarayıcılar hakkında dikkat edilmesi gerekenler [için, MSAL.NET ile Xamarin.Android özel hususlar](msal-net-system-browser-android-considerations.md)bakın.

#### <a name="tasks-for-uwp"></a>UWP için Görevler

UWP'de şirket ağlarını kullanabilirsiniz. Aşağıdaki bölümlerde, şirket senaryosunda tamamlamanız gereken görevler açıklanmıştır.

Daha fazla bilgi [için, MSAL.NET ile UWP'ye özgü hususlara](msal-net-uwp-considerations.md)bakın.

## <a name="configure-the-application-to-use-the-broker"></a>Aracıyı kullanacak şekilde uygulamayı yapılandırın

Android ve iOS'ta, brokerlar şunları sağlar:

- **Tek oturum açma (SSO)**: Azure Active Directory (Azure AD) kayıtlı aygıtlar için SSO'yı kullanabilirsiniz. SSO'yu kullandığınızda, kullanıcılarınızın her uygulamada oturum açmaları gerekmez.
- **Aygıt tanımlama**: Bu ayar, Azure AD aygıtları ile ilgili koşullu erişim ilkelerini sağlar. Kimlik doğrulama işlemi, aygıt iş yerine birleştiğinde oluşturulan aygıt sertifikasını kullanır.
- **Uygulama tanımlama doğrulaması**: Bir uygulama aracıyı aradığında, yeniden yönlendirme URL'sini geçer. Sonra komisyoncu bunu doğruladı.

### <a name="enable-the-broker-on-xamarin"></a>Xamarin üzerinde komisyoncu etkinleştirin

Xamarin üzerinde broker etkinleştirmek `WithBroker()` için, `PublicClientApplicationBuilder.CreateApplication` yöntemi ararken parametre kullanın. Varsayılan olarak, `.WithBroker()` doğru olarak ayarlanır. 

Xamarin.iOS için aracılı kimlik doğrulamayı etkinleştirmek için bu makaledeki [Xamarin.iOS bölümündeki](#enable-brokered-authentication-for-xamarin-ios) adımları izleyin.

### <a name="enable-the-broker-for-msal-for-android"></a>Android için MSAL aracısını etkinleştirin

Android'de bir broker etkinleştirme hakkında bilgi için [Android'de Aracılı kimlik doğrulama'ya](brokered-auth.md)bakın. 

### <a name="enable-the-broker-for-msal-for-ios-and-macos"></a>iOS ve macOS için MSAL aracısını etkinleştirme

aracılı kimlik doğrulaması, iOS ve macOS için MSAL'daki Azure REKLAM senaryoları için varsayılan olarak etkinleştirilir. 

Aşağıdaki bölümler, Xamarin.iOS için MSAL veya iOS ve macOS için MSAL için aracılı kimlik doğrulama desteği için uygulamanızı yapılandırmak için yönergeler sağlar. İki yönerge kümesinde, bazı adımlar farklıdır.

### <a name="enable-brokered-authentication-for-xamarin-ios"></a>Xamarin iOS için aracılı kimlik doğrulamayı etkinleştirme

Xamarin.iOS uygulamanızın [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) uygulamasıyla konuşmasını sağlamak için bu bölümdeki adımları izleyin.

#### <a name="step-1-enable-broker-support"></a>Adım 1: Broker desteğini etkinleştirme

Broker desteği varsayılan olarak devre dışı bırakılır. Tek bir `PublicClientApplication` sınıf için etkinleştirin. Sınıfı `WithBroker()` ' dan oluştururken `PublicClientApplication` parametreyi `PublicClientApplicationBuilder`kullanın. `WithBroker()` Parametre varsayılan olarak doğru olarak ayarlanır.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>Adım 2: Geri aramayı işlemek için AppDelegate'i güncelleştirme

MSAL.NET aracıyı aradığında, komisyoncu başvurunuza geri çağrır. Bu `AppDelegate.OpenUrl` yöntemi kullanarak geri çağırır. MSAL broker den yanıt bekler çünkü, uygulamanız MSAL.NET geri aramak için işbirliği gerekiyor. Aşağıdaki kodun gösterdiği gibi, `AppDelegate.cs` yöntemi geçersiz kılmak için dosyayı güncelleştirerek bu davranışı ayarlarsınız.

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

Bu yöntem, uygulama her başlatılınher zaman çağrılır. Bu, komisyoncudan gelen yanıtı işlemek ve MSAL.NET başlattığı kimlik doğrulama işlemini tamamlamak için bir fırsat.

#### <a name="step-3-set-a-uiviewcontroller"></a>Adım 3: Bir UIViewController() ayarlayın

Xamarin iOS için normalde bir nesne penceresi ayarlamanız gerekmez. Ancak bu durumda, bir komisyoncudan yanıt gönderip alabilmeniz için ayarlamanız gerekir. Bir nesne penceresini `AppDelegate.cs`ayarlamak için, `ViewController`içinde , bir .

Nesne penceresini ayarlamak için aşağıdaki adımları izleyin:

1. In `AppDelegate.cs`, `App.RootViewController` yeni `UIViewController()`bir ayarlayın . Bu ayar, aracıcıya yapılan `UIViewController`çağrının . Doğru ayarlı değilse, şu hatayı alabilirsiniz:

    `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker."`

1. Aramada, `AcquireTokenInteractive` kullan. `.WithParentActivityOrWindow(App.RootViewController)` Kullanacağınız nesne penceresine başvuruyu geçirin. Bir örneği aşağıda verilmiştir:

    `App.cs` içinde:
    ```csharp
       public static object RootViewController { get; set; }
    ```
    `AppDelegate.cs` içinde:
    ```csharp
       LoadApplication(new App());
       App.RootViewController = new UIViewController();
    ```
    Aramada: `AcquireToken`
    ```csharp
    result = await app.AcquireTokenInteractive(scopes)
                 .WithParentActivityOrWindow(App.RootViewController)
                 .ExecuteAsync();
    ```
    
#### <a name="step-4-register-a-url-scheme"></a>Adım 4: URL düzenini kaydetme

MSAL.NET aracıyı çağırmak ve ardından broker yanıtını uygulamanıza geri döndürmek için URL'leri kullanır. Turu tamamlamak için uygulamanızın URL şemasını dosyaya kaydedin. `Info.plist` 

Uygulamanızın URL düzenini kaydetmek için aşağıdaki adımları izleyin:

1. Önek `CFBundleURLSchemes` `msauth`ile . 
1. Sonuna `CFBundleURLName` ekle. Bu deseni izleyin: 

   `$"msauth.(BundleId)"`

   Burada, `BundleId` cihazınızı benzersiz olarak tanımlar. Örneğin, URL `BundleId` `yourcompany.xforms` `msauth.com.yourcompany.xforms`şemanız.
    
   > [!NOTE]
   > Bu URL şeması, aracının yanıtını aldığında uygulamanızı benzersiz olarak tanımlayan yeniden yönlendirme URI'nin bir parçası olur.
    
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
    
#### <a name="step-5-add-to-the-lsapplicationqueriesschemes-section"></a>Adım 5: LSApplicationQueriesSchemes bölümüne ekleme

MSAL, `–canOpenURL:` aracının aygıta yüklü olup olmadığını kontrol etmek için kullanır. iOS 9'da Apple, bir uygulamanın sorgulayabileceği şemaları kilitledi.

Aşağıdaki kod `LSApplicationQueriesSchemes` örneğinde olduğu gibi, dosyanın bölümüne ekleyin: `msauthv2` `Info.plist`

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-msal-for-ios-and-macos"></a>iOS ve macOS için MSAL için aracılı kimlik doğrulaması

Aracılı kimlik doğrulaması Varsayılan olarak Azure REKLAM senaryoları için etkinleştirilir.

#### <a name="step-1-update-appdelegate-to-handle-the-callback"></a>Adım 1: Geri aramayı işlemek için AppDelegate'i güncelleştirme

iOS ve macOS için MSAL aracıyı aradığında, broker `openURL` bu yöntemi kullanarak uygulamanızı geri çağırır. MSAL broker den yanıt bekler çünkü, uygulama MSAL geri aramak için işbirliği gerekiyor. Aşağıdaki kod örneklerinin gösterdiği `AppDelegate.m` gibi, yöntemi geçersiz kılmak için dosyayı güncelleştirerek bu özelliği ayarlayın.

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
> iOS 13 veya daha sonra kabul ettiyseniz, `UISceneDelegate` bunun `scene:openURLContexts:` `UISceneDelegate` yerine MSAL geri çağırma yerleştirin. MSAL `handleMSALResponse:sourceApplication:` her URL için yalnızca bir kez çağrılmalıdır.
>
> Daha fazla bilgi için [Apple belgelerine](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)bakın.

#### <a name="step-2-register-a-url-scheme"></a>Adım 2: URL düzenini kaydetme

iOS ve macOS için MSAL, aracıyı çağırmak ve ardından aracı yanıtı uygulamanıza döndürmek için URL'leri kullanır. Turu tamamlamak için, dosyaya uygulamanız için `Info.plist` bir URL şeması kaydedin.

Uygulamanız için bir şema kaydetmek için: 

1. Özel URL şemanınızı `msauth`önek. 

1. Paket tanımlayıcınızı şemanIzin sonuna ekleyin. Bu deseni izleyin: 

   `$"msauth.(BundleId)"`

   Burada, `BundleId` cihazınızı benzersiz olarak tanımlar. Örneğin, URL `BundleId` `yourcompany.xforms` `msauth.com.yourcompany.xforms`şemanız.
  
   > [!NOTE]
   > Bu URL şeması, aracının yanıtını aldığında uygulamanızı benzersiz olarak tanımlayan yeniden yönlendirme URI'nin bir parçası olur. Biçimdeki `msauth.(BundleId)://auth` URI yönlendirmesinin Uygulamanız için [Azure portalında](https://portal.azure.com)kayıtlı olduğundan emin olun.
  
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

#### <a name="step-3-add-lsapplicationqueriesschemes"></a>Adım 3: LSApplicationQueriesŞes Ekle

Yüklüyse Microsoft Authenticator uygulamasına izin vermek için ekleme `LSApplicationQueriesSchemes` ekleyin.

> [!NOTE]
> Uygulamanız Xcode 11 ve sonrası kullanılarak derlendiğinde `msauthv3` şema gereklidir. 

Aşağıda nasıl ekleyeceğiniz `LSApplicationQueriesSchemes`e-posta verilmiştir:

```XML 
<key>LSApplicationQueriesSchemes</key>
<array>
  <string>msauthv2</string>
  <string>msauthv3</string>
</array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>Xamarin.Android için aracılı kimlik doğrulaması

MSAL.NET Android için broker desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirteç edinme](scenario-mobile-acquire-token.md)
