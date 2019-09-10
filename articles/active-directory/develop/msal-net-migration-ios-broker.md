---
title: ADAL.NET 'den MSAL.NET 'e Microsoft Authenticator kullanarak Xamarin iOS uygulamalarını geçirme | Mavisi
description: .NET için Azure AD kimlik doğrulama kitaplığı (ADAL.NET) aracılığıyla Microsoft Authenticator kullanarak Xamarin iOS uygulamalarını .NET için Microsoft kimlik doğrulama kitaplığı 'na geçirmeyi öğrenin (MSAL.NET)
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 609aeeada78985466cb7c254cab32864e5d84b0e
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70875659"
---
# <a name="migrating-ios-applications-using-microsoft-authenticator-from-adalnet-to-msalnet"></a>ADAL.NET 'den MSAL.NET 'e Microsoft Authenticator kullanarak iOS uygulamalarını geçirme

ADAL.NET ve iOS Broker 'ı kullanıyorsunuz ve MSAL.NET [Microsoft kimlik doğrulama kitaplığı](msal-overview.md)'na geçiş zamanı, bu, sürüm 4,3 ' den iOS üzerinde aracı destekler. 

Nereden başlamalı? Bu makale, Xamarin iOS uygulamanızı ADAL 'ten MSAL 'e geçirmenize yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar
Bu belge, iOS broker ile tümleştirilmiş bir Xamarin iOS uygulamasına zaten sahip olduğunuzu varsayar. Bunu yapmazsanız, doğrudan MSAL.NET 'e geçmek ve aracı uygulamasını burada başlatmak en iyi seçenektir. MSAL.NET içinde iOS Broker 'ı yeni bir uygulamayla çağırma hakkındaki ayrıntılar için [Bu belgelere](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS#why-use-brokers-on-xamarinios-and-xamarinandroid-applications) bakın.

## <a name="background"></a>Arka Plan

### <a name="what-are-brokers"></a>Aracılar nelerdir?

Aracılar, Android ve iOS 'ta (iOS ve Android 'de[Microsoft Authenticator](https://www.microsoft.com/account/authenticator) , android 'de Intune şirket portalı) Microsoft tarafından sunulan uygulamalardır. 

Bunlar şunları etkinleştirir:

- Çoklu oturum açma,
- Bazı [koşullu erişim ilkeleri](../conditional-access/overview.md) için gereken cihaz kimliği (bkz. [cihaz yönetimi](../conditional-access/conditions.md#device-platforms))
- Uygulama tanımlama doğrulaması, bazı kurumsal senaryolarda de gereklidir (bkz. [Intune mobil uygulama yönetimi veya mam](https://docs.microsoft.com/intune/mam-faq)için bkz.)

## <a name="migrate-from-adal-to-msal"></a>ADAL 'ten MSAL 'e geçiş

### <a name="step-1-enable-the-broker"></a>1\. adım: Aracıyı etkinleştirin

<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>
ADAL.NET ' de, aracı desteği kimlik doğrulama bağlamı temelinde etkinleştirildi, varsayılan olarak devre dışıdır. Aracıyı çağırmak için `PlatformParameters` oluşturucuda true `useBroker` olarak bir bayrak ayarlamanız gerekiyordu:

```CSharp
public PlatformParameters(
        UIViewController callerViewController, 
        bool useBroker)
```
Ayrıca, platforma özgü kodda, bu örnekte, iOS için sayfa İşleyicide,`useBroker` 
true olarak işaretle:
```CSharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```

Ardından, belirteci al çağrısına parametreleri ekleyin:
```CSharp
 AuthenticationResult result =
                    await
                        AuthContext.AcquireTokenAsync(
                              Resource, 
                              ClientId, 
                              new Uri(RedirectURI), 
                              platformParameters)
                              .ConfigureAwait(false);
```

</td><td>
MSAL.NET ' de, aracı desteği ortak bir Istemci uygulama temelinde etkinleştirilir. Varsayılan olarak devre dışıdır. Etkinleştirmek için şunu kullanın: 

`WithBroker()`Aracı çağırmak için parametresi (varsayılan olarak true olarak ayarlanır):

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos)
                .Build();
```
Belirteç al çağrısında:
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```
</table>

### <a name="step-2-set-a-uiviewcontroller"></a>2\. adım: UIViewController () ayarlama
ADAL.NET ' de, Uıformparameters 'ın bir parçası olarak UIViewController 'a geçtiniz (adım 1 ' deki örneğe bakın). Ancak, MSAL.NET ' de, geliştiriciye daha fazla esneklik sağlamak için, normal iOS kullanımında bir nesne penceresi kullanılır, ancak gerekli değildir. Ancak, aracıyı kullanmak için, aracıdan yanıt göndermek ve almak üzere nesne penceresini ayarlamanız gerekir. 
<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>
UIViewController, iOS 'a özgü platformda bulunan PlatformParamters 'ya geçirilir.

```CSharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```
</td><td>
MSAL.NET ' de, iOS için nesne penceresini ayarlamak üzere iki şey yapmanız gerekir:

1) İçinde `AppDelegate.cs`, `App.RootViewController` öğesini Yeni`UIViewController()`olarak ayarlayın. Bu atama, aracı çağrısına bir UIViewController olduğundan emin olur. Doğru ayarlanmamışsa şu hatayı alabilirsiniz:`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) Acquiretokenınteractıve çağrısında şunu kullanın`.WithParentActivityOrWindow(App.RootViewController)`
ve, kullanacağınız nesne penceresi başvurusunu geçirin.

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

</table>

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>3\. adım: Geri aramayı işlemek için AppDelegate 'i Güncelleştir
Hem adal hem de msal aracı çağırır ve Aracı, `OpenUrl` `AppDelegate` sınıf yöntemi aracılığıyla uygulamanıza geri çağrı yapılır. Daha fazla bilgiyi [burada](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS/_edit#step-two-update-appdelegate-to-handle-the-callback) bulabilirsiniz

: heavy_check_mark:**burada adal.net ve msal.net arasında bir değişiklik yok**

### <a name="step-4-register-a-url-scheme"></a>4\. Adım: URL şeması kaydetme
ADAL.NET ve MSAL.NET, aracıyı çağırmak ve aracı yanıtını uygulamaya geri döndürmek için URL 'Leri kullanın. Aşağıdaki şekilde, URL şemasını `Info.plist` uygulamanız için dosyaya kaydedin:

<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>
URL şeması, uygulamanız için benzersizdir.
</td><td>
Sanal Makineye (VM) bağlı bir veya birden çok işletim sistemi diski içerdiği için 

`CFBundleURLSchemes`ad içermeli 

`msauth.`

ön ek olarak,`CFBundleURLName`

Örneğin, `$"msauth.(BundleId")`

```CSharp
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

> [!NOTE]
>  Bu URL şeması, Aracıdan yanıtı alırken uygulamayı benzersiz şekilde tanımlamak için kullanılan Redirecturı 'nin bir parçası olacak

</table>

### <a name="step-5-lsapplicationqueriesschemes"></a>5\. Adım: Lsapplicationqueriesdüzenleri

ADAL.net ve msal.net her ikisi `-canOpenURL:` de aracının cihaza yüklenip yüklenmediğini denetlemek için kullanılır. İOS Aracısı için doğru tanımlayıcıyı Info. plist dosyasının Lsapplicationqueriesdüzenlerinin bölümüne aşağıdaki şekilde ekleyin: 
<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>
Kullanımlar 

`msauth`


```CSharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>
```
</td><td>
Kullanımlar 

`msauthv2`


```CSharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauthv2</string>
</array>
```
</table>

### <a name="step-6-register-you-redirecturi-in-the-portal"></a>6\. Adım: Portala RedirectUri 'yi kaydetme

ADAL.NET ve MSAL.NET her ikisi de aracı hedefleme sırasında redirectUri üzerinde ek bir gereksinim ekler. Yeniden yönlendirme URI 'sini portaldaki uygulamanıza kaydedin.
<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>

`"<app-scheme>://<your.bundle.id>"`örneğinde`mytestiosapp://com.mycompany.myapp`
</td><td>

`$"msauth.{BundleId}://auth"`

örnek:

`public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; `

</table>

Portala redirectUri kaydetme hakkında daha fazla bilgi için bkz. daha fazla ayrıntı için [Xamarin. iOS uygulamalarında aracıdan](msal-net-use-brokers-with-xamarin-apps.md#step-7-make-sure-the-redirect-uri-is-registered-with-your-app) yararlanma.

## <a name="next-steps"></a>Sonraki adımlar

[Msal.net Ile Xamarin iOS 'a özgü hususlar](msal-net-xamarin-ios-considerations.md)hakkında bilgi edinin. 
