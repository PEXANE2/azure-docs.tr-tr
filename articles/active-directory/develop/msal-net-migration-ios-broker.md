---
title: Aracıları kullanarak Xamarin uygulamalarını MSAL.NET’e geçirme
titleSuffix: Microsoft identity platform
description: Microsoft Authenticator'ı ADAL.NET MSAL.NET'dan kullanan Xamarin iOS uygulamalarını nasıl geçirteceklerini öğrenin.
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: de259daa7fd27cc4f138c294a7f347502ca482a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77185822"
---
# <a name="migrate-ios-applications-that-use-microsoft-authenticator-from-adalnet-to-msalnet"></a>Microsoft Authenticator'ı ADAL.NET'dan MSAL.NET'a geçiren iOS uygulamalarını ADAL.NET geçirin

.NET (ADAL.NET) için Azure Active Directory Authentication Library'yi ve iOS aracısını kullanıyorsunuz. Şimdi, iOS'taki brokeri 4.3 sürümünden itibaren destekleyen .NET (MSAL.NET) için [Microsoft Kimlik Doğrulama Kitaplığı'na](msal-overview.md) geçiş yapma zamanı. 

Nereden başlamalısın? Bu makale, Xamarin iOS uygulamanızı ADAL'dan MSAL'a geçirmenize yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, zaten iOS aracıile entegre bir Xamarin iOS uygulaması varsayıyor. Bunu yapmazsanız, doğrudan MSAL.NET taşıyın ve orada komisyoncu uygulamasına başlayın. Yeni bir uygulama yla MSAL.NET iOS aracısını nasıl çağırılabilirsiniz hakkında bilgi için [bu belgelere](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS#why-use-brokers-on-xamarinios-and-xamarinandroid-applications)bakın.

## <a name="background"></a>Arka plan

### <a name="what-are-brokers"></a>Komisyoncular nedir?

Aracılar, Microsoft tarafından Android ve iOS'ta sağlanan uygulamalardır. (iOS ve Android'deki [Microsoft Authenticator](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6) uygulamasına ve Android'deki Intune Company Portal uygulamasına bakın.) 

Şunları sağlarlar:

- Çoklu oturum açma.
- Bazı [Koşullu Erişim ilkeleri](../conditional-access/overview.md)tarafından gerekli olan aygıt tanımlama. Daha fazla bilgi için Aygıt [yönetimine](../conditional-access/concept-conditional-access-conditions.md#device-platforms)bakın.
- Bazı kurumsal senaryolarda da gerekli olan uygulama tanımlama doğrulaması. Daha fazla bilgi için [Intune mobil uygulama yönetimi (MAM)](https://docs.microsoft.com/intune/mam-faq)bakın.

## <a name="migrate-from-adal-to-msal"></a>ADAL'dan MSAL'a geçiş

### <a name="step-1-enable-the-broker"></a>Adım 1: Aracıyı etkinleştirin

<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL meslektaşı:</td></tr>
<tr><td>
ADAL.NET, broker desteği kimlik doğrulama bağlamı bazında etkinleştirildi. Varsayılan olarak devre dışı bırakılır. Bir 

`useBroker`aracıyı aramak `PlatformParameters` için yapıcıda doğru bayrak:

```csharp
public PlatformParameters(
        UIViewController callerViewController, 
        bool useBroker)
```
Ayrıca, platforma özgü kodda, bu örnekte, iOS için sayfa işleyicisinde,`useBroker` 
bayrak doğru:
```csharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```

Ardından, edinme belirteci çağrısına parametreleri ekleyin:
```csharp
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
MSAL.NET olarak, broker desteği PublicClientApplication bazında etkinleştirilir. Varsayılan olarak devre dışı bırakılır. Etkinleştirmek için, 

`WithBroker()`broker aramak için parametre (varsayılan olarak doğru ayarlanmıştır):

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos)
                .Build();
```
Edinme belirteci çağrısında:
```csharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```
</table>

### <a name="step-2-set-a-uiviewcontroller"></a>Adım 2: Bir UIViewController() ayarlayın
ADAL.NET yılında, bir uiViewController bir `PlatformParameters`parçası olarak geçti. (Adım 1'deki örneğe bakın.) MSAL.NET, geliştiricilere daha fazla esneklik sağlamak için bir nesne penceresi kullanılır, ancak normal iOS kullanımında gerekli değildir. Aracıyı kullanmak için, aracıdan yanıt göndermek ve almak için nesne penceresini ayarlayın. 
<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL meslektaşı:</td></tr>
<tr><td>
Bir UIViewController içine geçirilir 

`PlatformParameters`iOS'a özgü platformda.

```csharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```
</td><td>
MSAL.NET, iOS için nesne penceresini ayarlamak için iki şey yaparsınız:

1. In `AppDelegate.cs`, `App.RootViewController` yeni `UIViewController()`bir ayarlayın . Bu atama, komisyoncuya yapılan çağrıyla birlikte bir UIViewController olmasını sağlar. Doğru ayarlı değilse, şu hatayı alabilirsiniz:`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
1. AcquireTokenInteractive aramasında, `.WithParentActivityOrWindow(App.RootViewController)`kullanacağınız nesne penceresine başvuruda bulunun ve geçirin.

**Örneğin:**

`App.cs` içinde:
```csharp
   public static object RootViewController { get; set; }
```
`AppDelegate.cs` içinde:
```csharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
Edinme belirteci çağrısında:
```csharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

</table>

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>Adım 3: Geri aramayı işlemek için AppDelegate'i güncelleştirme
Hem ADAL hem de MSAL komisyoncuyu arar ve komisyoncu da `OpenUrl` `AppDelegate` sınıfın yöntemi yle başvurunuza geri çağrır. Daha fazla bilgi için [bu belgelere](msal-net-use-brokers-with-xamarin-apps.md#step-3-update-appdelegate-to-handle-the-callback)bakın.

Burada ADAL.NET ve MSAL.NET arasında bir değişiklik yok.

### <a name="step-4-register-a-url-scheme"></a>Adım 4: URL düzenini kaydetme
ADAL.NET ve MSAL.NET aracıyı çağırmak ve broker yanıtını uygulamaya geri döndürmek için URL'leri kullanır. URL şemasını uygulamanız için dosyaya `Info.plist` aşağıdaki şekilde kaydedin:

<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL meslektaşı:</td></tr>
<tr><td>
URL düzeni uygulamanız için benzersizdir.
</td><td>
Bağlı olmayan bir veya birden çok klasik disk içerdiği için 

`CFBundleURLSchemes`adı içermelidir 

`msauth.`

bir önek olarak, takip`CFBundleURLName`

Örneğin, `$"msauth.(BundleId")`

```csharp
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
> Bu URL düzeni, aracıdan yanıt aldığında uygulamayı benzersiz olarak tanımlamak için kullanılan yeniden yönlendirme URI'nin bir parçası haline gelir.

</table>

### <a name="step-5-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>Adım 5: LSApplicationQueriesSchemes bölümüne broker tanımlayıcısı ekleyin

ADAL.NET ve MSAL.NET `-canOpenURL:` hem aracının cihaza yüklü olup olmadığını kontrol etmek için kullanılır. info.plist dosyasının LSApplicationQueriesSchemes bölümüne iOS aracısı için doğru tanımlayıcıyı aşağıdaki şekilde ekleyin:

<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL meslektaşı:</td></tr>
<tr><td>
Kullanımlar 

`msauth`


```csharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>
```
</td><td>
Kullanımlar 

`msauthv2`


```csharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauthv2</string>
     <string>msauthv3</string>
</array>
```
</table>

### <a name="step-6-register-your-redirect-uri-in-the-portal"></a>Adım 6: Yeniden yönlendirme URI'nizi portala kaydedin

ADAL.NET ve MSAL.NET, komisyoncuyu hedef aldığında uri'yi yeniden yönlendirmeye ek bir gereksinim ekler. Uygulamanız ile uygulama portalda yeniden yönlendirme URI kaydedin.
<table>
<tr><td>Geçerli ADAL kodu:</td><td>MSAL meslektaşı:</td></tr>
<tr><td>

`"<app-scheme>://<your.bundle.id>"`

Örnek: 

`mytestiosapp://com.mycompany.myapp`
</td><td>

`$"msauth.{BundleId}://auth"`

Örnek:

`public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; `

</table>

Yeniden yönlendirme URI'nin portala nasıl kaydedilenhakkında daha fazla bilgi için [Xamarin.iOS uygulamalarındaki brokerdan yararlanın.](msal-net-use-brokers-with-xamarin-apps.md#step-8-make-sure-the-redirect-uri-is-registered-with-your-app)

## <a name="next-steps"></a>Sonraki adımlar

MSAL.NET [ile Xamarin iOS'a özel hususlar](msal-net-xamarin-ios-considerations.md)hakkında bilgi edinin. 
