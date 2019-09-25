---
title: Web API 'Lerini çağıran mobil uygulama (kod yapılandırması)-Microsoft Identity platform | Mavisi
description: Web API 'Lerini (uygulamanın kod yapılandırması) çağıran bir mobil uygulama oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7b731c9936ab85b19428687330044a46c563c49
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268362"
---
# <a name="mobile-app-that-calls-web-apis---code-configuration"></a>Web API 'Lerini çağıran mobil uygulama-kod yapılandırması

Uygulamanızı oluşturduktan sonra, uygulamanın kayıt parametrelerini kullanarak kodu nasıl yapılandıracağınızı öğreneceksiniz. Mobil uygulamalarda, bu uygulamaları oluşturmak için kullanılan çerçeveye ekleme yapmak zorunda olan bazı karmaşık bilgiler de vardır

## <a name="msal-libraries-supporting-mobile-apps"></a>Mobil uygulamaları destekleyen MSAL kitaplıkları

Mobil uygulamaları destekleyen Microsoft kitaplıkları şunlardır:

  MSAL kitaplığı | Açıklama
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Taşınabilir uygulamalar geliştirmek için. Mobil uygulama derlemek için MSAL.NET desteklenen platformlar UWP, Xamarin. iOS ve Xamarin. Android ' dir.
  ![MSAL. iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL. iOS | Amaç-C veya Swift ile Yerel iOS uygulamaları geliştirmek için
  ![MSAL. Android](media/sample-v2-code/logo_android.png) <br/> MSAL. Android | Android için Java 'da yerel Android uygulamaları geliştirmek için

## <a name="instantiating-the-application"></a>Uygulamanın örneğini oluşturma

### <a name="android"></a>Android

Mobil uygulamalar `PublicClientApplication` sınıfını kullanır. Bunun örneği şöyledir:

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

İOS üzerinde mobil uygulamaların `MSALPublicClientApplication` sınıfının örneğini oluşturması gerekir.

Amaç-C:

```objc
NSError *msalError = nil;
     
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];    
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

SWIFT
```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

Varsayılan yetkiyi geçersiz kılabilen [ek MSALPublicClientApplicationConfig özellikleri](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALPublicClientApplicationConfig.html#/Configuration%20options) vardır, yeniden yönlendirme URI 'si BELIRTEBILIR veya msal Token Caching davranışını değiştirebilirsiniz. 

### <a name="xamarin-or-uwp"></a>Xamarin veya UWP

Aşağıdaki paragraf, Xamarin. iOS, Xamarin. Android ve UWP uygulamaları için uygulamanın örneğini oluşturmayı açıklamaktadır.

#### <a name="instantiating-the-application"></a>Uygulamanın örneğini oluşturma

Xamarin veya UWP 'de, uygulamayı örneketmenin en kolay yolu aşağıdaki gibidir; burada `ClientId` , kayıtlı uygulamanızın GUID 'sidir.

```CSharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

UI üst öğesini, varsayılan yetkiyi geçersiz kılmayı, bir istemci adı ve sürümünü (telemetri için), yeniden yönlendirme URI 'sini, kullanılacak http fabrikasını (örneğin, proxy 'leri işlemek için) belirten*parametre* yöntemleriyle birlikte ek olarak telemetri ve günlüğe kaydetme). Aşağıdaki paragrafların konusu budur.

##### <a name="specifying-the-parent-uiwindowactivity"></a>Üst Kullanıcı arabirimini/pencereyi/etkinliği belirtme

Android 'de, etkileşimli kimlik doğrulaması yapmadan önce üst etkinliği geçirmeniz gerekir. İOS 'ta, bir aracı kullanırken, ViewController 'a geçiş yapmanız gerekir. UWP 'de aynı şekilde, üst pencereyi geçirmek isteyebilirsiniz. Bu, belirteci aldığınızda mümkündür, ancak uygulama oluşturma zamanında Uıparent döndüren bir temsilci de bir geri çağırma belirtmek mümkündür.

```CSharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

Android 'de, `CurrentActivityPlugin` [burada](https://github.com/jamesmontemagno/CurrentActivityPlugin)kullanmanız önerilir.  Ardından, `PublicClientApplication` Oluşturucu kodunuz şöyle görünür:

```CSharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="more-app-building-parameters"></a>Daha fazla uygulama derleme parametresi

- Üzerinde `PublicClientApplicationBuilder`kullanılabilen tüm değiştiricilerin listesi için bkz. Reference documentation [publicclientapplicationbuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods)
- Bkz. `PublicClientApplicationOptions` [publicclientapplicationoptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions)içinde gösterilen tüm seçeneklerin açıklaması için başvuru belgeleri

## <a name="xamarin-ios-specific-considerations"></a>Xamarin iOS 'e özgü konular

Xamarin iOS üzerinde MSAL.NET kullanırken dikkate almanız gereken birkaç önemli noktalar vardır:

1. [`OpenUrl` İşlevinde işlevi geçersiz kılın ve uygulayın`AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
1. [Anahtarlık gruplarını etkinleştir](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
1. [Belirteç önbelleği paylaşımını etkinleştir](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
1. [Anahtarlık erişimini etkinleştir](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Ayrıntılar [Xamarin iOS konuları](msal-net-xamarin-ios-considerations.md) 'nda verilmiştir

## <a name="msal-for-ios-and-macos-specific-considerations"></a>İOS için MSAL ve macOS 'a özgü konular

İOS ve macOS için MSAL kullanılırken de benzer hususlar geçerlidir:

1. [`openURL` Geri aramayı Uygula](#brokered-authentication-for-msal-for-ios-and-macos)
2. [Anahtarlık erişim gruplarını etkinleştir](howto-v2-keychain-objc.md)
3. [Tarayıcıları ve Web görünümlerini özelleştirme](customize-webviews.md)

## <a name="xamarin-android-specific-considerations"></a>Xamarin Android 'e özgü konular

Xamarin Android özellikleri şunlardır:

- [Kimlik doğrulama akışının etkileşimli kısmı sona erdikten sonra denetimin MSAL 'e geri gitmesini sağlama](msal-net-xamarin-android-considerations.md#ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends)
- [Android bildirimini güncelleştirme](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [Katıştırılmış Web görünümünü kullanın (isteğe bağlı)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [Sorun giderme](msal-net-xamarin-android-considerations.md#troubleshooting)

Ayrıntılar [Xamarin Android](msal-net-xamarin-android-considerations.md) 'de verilmiştir

Son olarak, Android 'de tarayıcılar hakkında daha fazla bilgi edinmek için bazı bazı karmaşıklık vardır. [Msal.net Ile Xamarin Android 'e özgü hususlar](msal-net-system-browser-android-considerations.md) bölümünde açıklanırlar

#### <a name="uwp-specific-considerations"></a>UWP 'e özgü konular

UWP 'de kurumsal ağları kullanabilirsiniz. UWP ile MSAL kitaplığını kullanma hakkında daha fazla bilgi için, bkz. [msal.net 'e özgü hususlar Evrensel Windows platformu](msal-net-uwp-considerations.md).

## <a name="configuring-the-application-to-use-the-broker"></a>Aracıyı kullanmak için uygulamayı yapılandırma

### <a name="why-use-brokers-in-ios-and-android-applications"></a>İOS ve Android uygulamalarında neden aracılar kullanılmalıdır?

Android ve iOS 'ta aracılar şunları etkinleştirir:

- Cihaz AAD 'ye kaydedildiğinde çoklu oturum açma (SSO). Kullanıcılarınızın her uygulamada oturum açması gerekmez.
- Cihaz kimliği. , Çalışma alanına katılmış olduğunda cihazda oluşturulan cihaz sertifikasına erişerek Azure AD cihazla ilgili koşullu erişim ilkelerini etkinleştirilir.
- Uygulama tanımlama doğrulaması. Bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir ve aracı tarafından doğrulanır.

### <a name="enable-the-brokers-on-xamarin"></a>Xamarin 'teki aracıları etkinleştirme

Bu özelliklerden birini etkinleştirmek için, `WithBroker()` `PublicClientApplicationBuilder.CreateApplication` yöntemini çağırırken parametresini kullanın. `.WithBroker()`Varsayılan olarak true olarak ayarlanır. [Xamarin. iOS](#brokered-authentication-for-xamarinios)için aşağıdaki adımları izleyin.

### <a name="enable-the-broker-for-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL aracısını etkinleştirme

Aracılı kimlik doğrulaması, iOS ve macOS için MSAL içindeki AAD senaryolarında varsayılan olarak etkindir. Uygulamanızı [iOS ve macOS Için msal](#brokered-authentication-for-msal-for-ios-and-macos)için aracılı kimlik doğrulama desteği için yapılandırmak üzere aşağıdaki adımları izleyin. [İOS ve macOS Için](#brokered-authentication-for-msal-for-ios-and-macos) [msal Için Xamarin. iOS](#brokered-authentication-for-xamarinios) ve msal arasında bazı adımların farklı olduğunu unutmayın.

### <a name="brokered-authentication-for-xamarinios"></a>Xamarin. iOS için aracılı kimlik doğrulaması

Xamarin. iOS uygulamanızın [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) uygulamayla iletişim kurmasını sağlamak için aşağıdaki adımları izleyin.

#### <a name="step-1-enable-broker-support"></a>1\. adım: Aracı desteğini etkinleştir

Aracı desteği,`PublicClientApplication` temelinde etkinleştirilir. Varsayılan olarak devre dışıdır. Aracılığıyla oluştururkenparametresini`WithBroker()` kullanmanız gerekir (varsayılan olarak true olarak ayarlanır). `PublicClientApplicationBuilder` `PublicClientApplication`

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>2\. adım: Geri aramayı işlemek için AppDelegate 'i Güncelleştir

MSAL.NET aracı çağırdığında, aracı `AppDelegate.OpenUrl` yöntemi aracılığıyla uygulamanıza geri çağrı yapılır. MSAL, aracıdan gelen yanıtı bekleyecek için, uygulamanızın MSAL.NET geri çağırmak için birlikte çalışması gerekir. Bunu, `AppDelegate.cs` aşağıdaki yöntemi geçersiz kılmak için dosyasını güncelleştirerek yapabilirsiniz.

```CSharp
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

Bu yöntem, uygulama her başlatıldığında çağrılır ve aracıdan gelen yanıtı işleme ve MSAL.NET tarafından başlatılan kimlik doğrulama işlemini tamamlamaya yönelik bir fırsat olarak kullanılır.

#### <a name="step-3-set-a-uiviewcontroller"></a>3\. adım: UIViewController () ayarlama

Xamarin iOS sayesinde normalde bir nesne penceresi ayarlamanız gerekmez, ancak bu durumda bir aracıdan yanıt göndermek ve almak için yapmanız gerekir. Hala içinde `AppDelegate.cs`, bir ViewController ayarlayın.

Nesne penceresini ayarlamak için aşağıdakileri yapın:

1) İçinde `AppDelegate.cs`, `App.RootViewController` öğesini Yeni`UIViewController()`olarak ayarlayın. Bu, aracıya yönelik çağrıya `UIViewController` sahip olduğundan emin olmanızı sağlar. Doğru ayarlanmamışsa şu hatayı alabilirsiniz:`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) Acquiretokenınteractive çağrısında, `.WithParentActivityOrWindow(App.RootViewController)` öğesini kullanın ve kullanacağınız nesne penceresine yönelik başvuruyu geçirin.

**Örneğin:**

`App.cs` içinde:
```CSharp
   public static object RootViewController { get; set; }
```
`AppDelegate.cs` içinde:
```CSharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
Belirteç al çağrısında:
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

#### <a name="step-4-register-a-url-scheme"></a>4\. Adım: URL şeması kaydetme

MSAL.NET, aracıyı çağırmak için URL 'Ler kullanır ve ardından aracı yanıtını uygulamanıza geri döndürür. Gidiş dönüş işleminin tamamlanabilmesi için, `Info.plist` dosyaya uygulamanız için bir URL şeması kaydetmeniz gerekir.

`CFBundleURLSchemes` İle`msauth`önekini öneki. Ardından sonuna `CFBundleURLName` ekleyin.

`$"msauth.(BundleId)"`

**Örneğin:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Bu URL şeması, Aracıdan yanıtı alırken uygulamanızı benzersiz bir şekilde tanımlamak için kullanılan Redirecturı 'nin bir parçası olacak.

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

#### <a name="step-5-lsapplicationqueriesschemes"></a>5\. Adım: Lsapplicationqueriesdüzenleri

MSAL, `–canOpenURL:` aracının cihaza yüklenip yüklenmediğini denetlemek için kullanır. İOS 9 ' da, Apple bir uygulamanın sorgulayabilecekleri düzenleri kilitlemiş.

**Ekle** **`msauthv2`dosyasınınbölümünegidin.** `LSApplicationQueriesSchemes` `Info.plist`

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL için aracılı kimlik doğrulaması

Aracılı kimlik doğrulaması AAD senaryolarında varsayılan olarak etkindir.

#### <a name="step-1-update-appdelegate-to-handle-the-callback"></a>1\. adım: Geri aramayı işlemek için AppDelegate 'i Güncelleştir

İOS ve MacOS için msal, aracıyı çağırdığında, aracı `openURL` yöntemi aracılığıyla uygulamanıza geri çağrı yapılır. MSAL, aracıdan gelen yanıtı bekleyecek için, uygulamanızın MSAL geri çağırmak için birlikte çalışması gerekir. Bunu, `AppDelegate.m` aşağıdaki yöntemi geçersiz kılmak için dosyasını güncelleştirerek yapabilirsiniz.

Amaç-C:

```objc
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
    return [MSALPublicClientApplication handleMSALResponse:url 
                                         sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

SWIFT

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

İOS 13 + üzerinde UISceneDelegate benimsediyseniz msal geri çağrısının bunun yerine UISceneDelegate içine `scene:openURLContexts:` yerleştirilmesi gerektiğini unutmayın (bkz. [Apple belgeleri](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)). MSAL `handleMSALResponse:sourceApplication:` her URL için yalnızca bir kez çağrılmalıdır.

#### <a name="step-2-register-a-url-scheme"></a>2\. adım: URL şeması kaydetme

İOS ve macOS için MSAL, aracıyı çağırmak için URL 'Ler kullanır ve ardından aracı yanıtını uygulamanıza geri döndürür. Gidiş dönüş işleminin tamamlanabilmesi için, `Info.plist` dosyaya uygulamanız için bir URL şeması kaydetmeniz gerekir.

Özel URL düzeninizi ile `msauth`önek yapın. Ardından **paket tanımlarınızı** sonuna ekleyin.

`msauth.(BundleId)`

**Örneğin:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Bu URL şeması, Aracıdan yanıtı alırken uygulamanızı benzersiz bir şekilde tanımlamak için kullanılan Redirecturı 'nin bir parçası olacak. Biçimindeki redirecturı 'nin `msauth.(BundleId)://auth` , [Azure portalında](https://portal.azure.com)uygulamanız için kayıtlı olduğundan emin olun.

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

#### <a name="step-3-lsapplicationqueriesschemes"></a>3\. adım: Lsapplicationqueriesdüzenleri

Yüklenmişse Microsoft Authenticator çağrısı yapılmasına izin vermek için **ekleyin `LSApplicationQueriesSchemes`**  .
Uygulamanızı Xcode 11 ve üzeri ile derlerken "msauthv3" şemasının gerekli olduğunu unutmayın. 

```XML 
<key>LSApplicationQueriesSchemes</key>
<array>
  <string>msauthv2</string>
  <string>msauthv3</string>
</array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>Xamarin. Android için aracılı kimlik doğrulaması

MSAL.NET henüz Android için aracıları desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirteç alma](scenario-mobile-acquire-token.md)
