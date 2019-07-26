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
ms.openlocfilehash: 3bdf9210eb88b2057cf861b208f19d3e6f562e9a
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414856"
---
# <a name="mobile-app-that-calls-web-apis---code-configuration"></a>Web API 'Lerini çağıran mobil uygulama-kod yapılandırması

Uygulamanızı oluşturduktan sonra uygulamanın uygulama kaydı sırasında aldığınız parametrelerden kodu nasıl yapılandıracağınızı öğreneceksiniz. Mobil uygulamalarda, bu uygulamaları oluşturmak için kullanılan çerçeveye ekleme yapmak zorunda olan bazı karmaşık bilgiler de vardır

## <a name="msal-libraries-supporting-mobile-apps"></a>Mobil uygulamaları destekleyen MSAL kitaplıkları

Mobil uygulamaları destekleyen Microsoft kitaplıkları şunlardır:

  MSAL kitaplığı | Açıklama
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Taşınabilir uygulamalar geliştirmek için. Mobil uygulama derlemek için MSAL.NET desteklenen platformlar UWP, Xamarin. iOS ve Xamarin. Android ' dir.
  ![MSAL. iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL. iOS | Hedef C veya Swift ile Yerel iOS uygulamaları geliştirmek için
  ![MSAL. Android](media/sample-v2-code/logo_android.png) <br/> MSAL. Android | Android için Java 'da yerel Android uygulamaları geliştirmek için

## <a name="configuring-the-application"></a>Uygulamayı yapılandırma

Mobil uygulamalar msal 'in `PublicClientApplication` sınıfını kullanır. Bunun örneği şöyledir:

### <a name="android"></a>Android

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

```swift
// Initialize the app.
guard let authorityURL = URL(string: kAuthority) else {
    self.loggingText.text = "Unable to create authority URL"
    return
}
let authority = try MSALAADAuthority(url: authorityURL)
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
}
```

### <a name="xamarin-or-uwp"></a>Xamarin veya UWP

Aşağıdaki paragrafta, Xamarin. iOS, Xamarin. Android ve UWP uygulamaları için uygulama kodunun nasıl yapılandırılacağı açıklanmaktadır. İlk adım, uygulamayı örnekleyemedi. Aracıyı yapılandırmak, isteğe bağlı bir adımdır.

#### <a name="instantiating-the-application"></a>Uygulamanın örneğini oluşturma

Xamarin veya UWP 'de, uygulamayı örneketmenin en kolay yolu aşağıdaki gibidir. burada `ClientId` , uygulamanın GUID 'sidir.

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

Android 'de, `CurrentActivityPlugin` [burada](https://github.com/jamesmontemagno/CurrentActivityPlugin)size önerilir.  Ardından, `PublicClientApplication` Oluşturucu kodunuz şöyle görünür:

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

#### <a name="xamarin-ios-specific-considerations"></a>Xamarin iOS 'e özgü konular

Xamarin iOS üzerinde MSAL.NET kullanırken dikkate almanız gereken birkaç önemli noktalar vardır:

1. [`OpenUrl` İşlevinde işlevi geçersiz kılın ve uygulayın`AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
1. [Anahtarlık gruplarını etkinleştir](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
1. [Belirteç önbelleği paylaşımını etkinleştir](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
1. [Anahtarlık erişimini etkinleştir](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Ayrıntılar [Xamarin iOS konuları](msal-net-xamarin-ios-considerations.md) 'nda verilmiştir

#### <a name="other-xamarin-android-specific-considerations"></a>Diğer Xamarin Android 'e özgü konular

Xamarin Android özellikleri şunlardır:

- [Kimlik doğrulama akışının etkileşimli kısmı sona erdikten sonra denetimin MSAL 'e geri gitmesini sağlama](msal-net-xamarin-android-considerations.md#ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends)
- [Android bildirimini güncelleştirme](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [Katıştırılmış Web görünümünü kullanın (isteğe bağlı)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [Sorun giderme](msal-net-xamarin-android-considerations.md#troubleshooting)

Ayrıntılar [Xamarin Android](msal-net-xamarin-android-considerations.md) 'de verilmiştir

Son olarak, Android 'de tarayıcılar hakkında daha fazla bilgi edinmek için bazı bazı karmaşıklık vardır. [Msal.net Ile Xamarin Android 'e özgü hususlar](msal-net-system-browser-android-considerations.md) bölümünde açıklanırlar

#### <a name="uwp-specific-considerations"></a>UWP 'e özgü konular

UWP 'de kurumsal ağları kullanabilirsiniz. UWP özellikleri hakkında daha fazla bilgi için bkz. [msal.NET ile ilgili Evrensel Windows platformu özel noktalar](msal-net-uwp-considerations.md).

## <a name="configuring-the-application-to-use-the-broker"></a>Aracıyı kullanmak için uygulamayı yapılandırma

### <a name="why-use-brokers-on-xamarinios-and-xamarinandroid-applications"></a>Xamarin. iOS ve Xamarin. Android uygulamalarında neden aracılar kullanılmalıdır?

Android ve iOS 'ta aracılar şunları etkinleştirir:

- Çoklu oturum açma (SSO). Kullanıcılarınızın her uygulamada oturum açması gerekmez.
- Cihaz kimliği. , Çalışma alanına katılmış olduğunda cihazda oluşturulan cihaz sertifikasına erişerek Azure AD cihazla ilgili koşullu erişim ilkelerini etkinleştirilir.
- Uygulama tanımlama doğrulaması. Bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir ve aracı tarafından doğrulanır.

### <a name="enable-the-brokers-on-xamarin"></a>Xamarin 'teki aracıları etkinleştirme

Bu özelliklerden birini etkinleştirmek için, `WithBroker()` `PublicClientApplicationBuilder.CreateApplication` yöntemini çağırırken parametresini kullanın. `.WithBroker()`Varsayılan olarak true olarak ayarlanır. [İOS](#brokered-authentication-for-xamarinios)için aşağıdaki adımları izleyin.

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

Xamarin iOS ile normalde bir nesne penceresi ayarlamanız gerekmez, ancak bu durumda bir aracıdan yanıt göndermek ve almak için yapmanız gerekir. Hala içinde `AppDelegate.cs`, bir ViewController ayarlayın.

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

### <a name="brokered-authentication-for-xamarinandroid"></a>Xamarin. Android için aracılı kimlik doğrulaması

MSAL.NET henüz Android için aracıları desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirteç alma](scenario-mobile-acquire-token.md)
