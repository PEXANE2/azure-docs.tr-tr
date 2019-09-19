---
title: ADAL.NET 'den MSAL.NET 'e Microsoft Authenticator kullanan Xamarin iOS uygulamalarını geçirin | Mavisi
description: .NET için Azure AD kimlik doğrulama kitaplığı 'ndan Microsoft Authenticator kullanan Xamarin iOS uygulamalarını .NET için Microsoft kimlik doğrulama kitaplığı 'na (MSAL.NET) geçirmeyi öğrenin.
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
ms.openlocfilehash: 4426c070d2fd2e26e0d4368a94996bc32e95129d
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71103952"
---
# <a name="migrate-ios-applications-that-use-microsoft-authenticator-from-adalnet-to-msalnet"></a>ADAL.NET 'den MSAL.NET 'ye Microsoft Authenticator kullanan iOS uygulamalarını geçirme

.NET (ADAL.NET) ve iOS Aracısı için Azure Active Directory kimlik doğrulama kitaplığını kullanıyorsunuz. Şimdi .NET için [Microsoft kimlik doğrulama kitaplığı](msal-overview.md) 'na (msal.net) geçiş yapma zamanı, bu sürüm 4,3 ' den iOS üzerindeki aracıyı destekler. 

Nereden başlamanız gerekir? Bu makale, Xamarin iOS uygulamanızı ADAL 'dan MSAL 'e geçirmenize yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, iOS broker ile tümleştirilmiş bir Xamarin iOS uygulamasına zaten sahip olduğunuz varsayılır. Bunu yapmazsanız, doğrudan MSAL.NET 'e taşıyın ve aracı uygulamasını orada başlatın. MSAL.NET içinde iOS Broker 'ı yeni bir uygulamayla çağırma hakkında daha fazla bilgi için [Bu belgelere](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS#why-use-brokers-on-xamarinios-and-xamarinandroid-applications)bakın.

## <a name="background"></a>Arka Plan

### <a name="what-are-brokers"></a>Aracılar nelerdir?

Aracılar, Android ve iOS 'ta Microsoft tarafından sunulan uygulamalardır. (İOS ve Android 'de [Microsoft Authenticator](https://www.microsoft.com/account/authenticator) uygulamasına ve android üzerinde Intune Şirket Portalı uygulamasına bakın.) 

Bunlar şunları etkinleştirir:

- Çoklu oturum açma.
- Bazı [koşullu erişim ilkeleri](../conditional-access/overview.md)için gereken cihaz kimliği. Daha fazla bilgi için bkz. [cihaz yönetimi](../conditional-access/conditions.md#device-platforms).
- Uygulama tanımlama doğrulaması, bazı kurumsal senaryolarda de gereklidir. Daha fazla bilgi için bkz. [Intune mobil uygulama yönetimi (MAM)](https://docs.microsoft.com/intune/mam-faq).

## <a name="migrate-from-adal-to-msal"></a>ADAL 'ten MSAL 'e geçiş

### <a name="step-1-enable-the-broker"></a>1\. adım: Aracıyı etkinleştirin

<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>
ADAL.NET ' de, bir kimlik doğrulama bağlamı temelinde Aracı desteği etkinleştirilmiştir. Varsayılan olarak devre dışıdır. Şunu ayarlamanız gerekiyordu 

`useBroker`Aracıyı çağırmak için `PlatformParameters` oluşturucuda true olarak bayrak ekleyin:

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
MSAL.NET ' de, aracı desteği PublicClientApplication temelinde etkinleştirilir. Varsayılan olarak devre dışıdır. Etkinleştirmek için şunu kullanın 

`WithBroker()`Aracıyı çağırmak için parametresi (varsayılan olarak true olarak ayarlanır):

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
ADAL.NET ' de, bir UIViewController içine bir parçası `PlatformParameters`olarak geçtiniz. (Adım 1 ' deki örneğe bakın.) MSAL.NET ' de, geliştiricilere daha fazla esneklik sağlamak için bir nesne penceresi kullanılır, ancak normal iOS kullanımı için gerekli değildir. Aracıyı kullanmak için, aracıdan yanıt göndermek ve almak üzere nesne penceresini ayarlayın. 
<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>
Bir UIViewController öğesine geçirildi 

`PlatformParameters`iOS 'a özgü platformda.

```CSharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```
</td><td>
MSAL.NET ' de, iOS için nesne penceresini ayarlamak için iki şey yapmanız gerekir:

1. İçinde `AppDelegate.cs`, yeni `App.RootViewController` olarakayarlayın.`UIViewController()` Bu atama, aracı çağrısına bir UIViewController olduğundan emin olmanızı sağlar. Doğru ayarlanmamışsa şu hatayı alabilirsiniz:`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
1. Acquiretokenınteractive çağrısında `.WithParentActivityOrWindow(App.RootViewController)`, kullanın ve kullanacağınız nesne penceresi başvurusunu geçirin.

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
Hem adal hem de msal aracı çağırır ve içindeki aracı, `OpenUrl` `AppDelegate` sınıfının yöntemi aracılığıyla uygulamanıza geri çağrı çağırır. Daha fazla bilgi için [Bu belgelere](msal-net-use-brokers-with-xamarin-apps.md#step-2-update-appdelegate-to-handle-the-callback)bakın.

Burada ADAL.NET ve MSAL.NET arasında bir değişiklik yoktur.

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
> Bu URL şeması, Aracıdan yanıtı aldığında uygulamayı benzersiz şekilde tanımlamak için kullanılan yeniden yönlendirme URI 'sinin bir parçası haline gelir.

</table>

### <a name="step-5-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>5\. Adım: Aracı tanımlayıcısını Lsapplicationqueriesdüzenlerinin bölümüne ekleyin

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

### <a name="step-6-register-your-redirect-uri-in-the-portal"></a>6\. Adım: Yeniden yönlendirme URI 'nizi portala kaydetme

ADAL.NET ve MSAL.NET, aracıyı hedefliyorsa yeniden yönlendirme URI 'sine ek bir gereksinim ekler. Yeniden yönlendirme URI 'sini portaldaki uygulamanıza kaydedin.
<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL karşılığı:</td></tr>
<tr><td>

`"<app-scheme>://<your.bundle.id>"`

Örnek: 

`mytestiosapp://com.mycompany.myapp`
</td><td>

`$"msauth.{BundleId}://auth"`

Örnek:

`public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; `

</table>

Yeniden yönlendirme URI 'sini portalda kaydetme hakkında daha fazla bilgi için bkz. [Xamarin. iOS uygulamalarında aracıdan yararlanma](msal-net-use-brokers-with-xamarin-apps.md#step-7-make-sure-the-redirect-uri-is-registered-with-your-app).

## <a name="next-steps"></a>Sonraki adımlar

[Msal.net Ile Xamarin iOS 'a özgü hususlar](msal-net-xamarin-ios-considerations.md)hakkında bilgi edinin. 
