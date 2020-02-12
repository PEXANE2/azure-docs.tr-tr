---
title: Xamarin iOS & Android ile aracıları kullanma | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft Authenticator ve Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanan Xamarin iOS uygulamalarını ayarlamayı öğrenin. Ayrıca .NET için Azure AD kimlik doğrulama kitaplığı 'ndan (ADAL.NET) .NET için Microsoft kimlik doğrulama Kitaplığı ' na (MSAL.NET) geçiş yapmayı öğrenin.
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
ms.openlocfilehash: 25b8aa9b5e80720e9543dafce7970404a62b7d1f
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132649"
---
# <a name="use-microsoft-authenticator-or-intune-company-portal-on-xamarin-applications"></a>Xamarin uygulamalarında Microsoft Authenticator veya Intune Şirket Portalı kullanma

Android ve iOS 'ta Microsoft Authenticator ve Android 'e özgü Microsoft Intune gibi aracılar Şirket Portalı etkinleştirir:

- **Çoklu oturum açma (SSO)** : kullanıcıların her uygulamada oturum açması gerekmez.
- **Cihaz kimliği**: aracı cihaz sertifikasına erişir. Bu sertifika, çalışma alanına katıldığında cihazda oluşturulur.
- **Uygulama tanımlama doğrulaması**: bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir. Aracı URL 'YI doğrular.

Bu özelliklerden birini etkinleştirmek için, `PublicClientApplicationBuilder.CreateApplication` yöntemini çağırdığınızda `WithBroker()` parametresini kullanın. `.WithBroker()` parametresi varsayılan olarak true olarak ayarlanır. 

Ayrıca [iOS](#brokered-authentication-for-ios) uygulamaları veya [Android](#brokered-authentication-for-android) uygulamaları için aracılı kimlik doğrulaması ayarlamak üzere aşağıdaki bölümlerdeki yönergeleri kullanın.

## <a name="brokered-authentication-for-ios"></a>İOS için aracılı kimlik doğrulaması

Xamarin. iOS uygulamanızın [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) uygulamayla iletişim kurmasını sağlamak için aşağıdaki adımları kullanın.

### <a name="step-1-enable-broker-support"></a>1\. Adım: aracı desteğini etkinleştirme
`PublicClientApplication`ayrı örnekleri için aracı desteğini etkinleştirmeniz gerekir. Destek varsayılan olarak devre dışıdır. `PublicClientApplicationBuilder`aracılığıyla `PublicClientApplication` oluşturduğunuzda, aşağıdaki örnekte gösterildiği gibi `WithBroker()` parametresini kullanın. `WithBroker()` parametresi varsayılan olarak true olarak ayarlanır.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-enable-keychain-access"></a>2\. Adım: Anahtarlık erişimini etkinleştirme

Anahtarlık erişimini etkinleştirmek için, uygulamanız için bir Anahtarlık erişim grubunuz olmalıdır. Uygulamanızı oluştururken Anahtarlık erişim grubunuzu ayarlamak için `WithIosKeychainSecurityGroup()` API 'sini kullanabilirsiniz:

```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
      
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Daha fazla bilgi için bkz. [Anahtarlık erişimini etkinleştirme](msal-net-xamarin-ios-considerations.md#enable-keychain-access).

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>3\. Adım: geri aramayı işlemek için AppDelegate 'i güncelleştirme
.NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) aracıyı çağırdığında, aracı `AppDelegate` sınıfının `OpenUrl` yöntemi aracılığıyla uygulamanıza geri çağrı yapılır. MSAL, aracının yanıtını beklediği için uygulamanızın MSAL.NET geri çağırmak için birlikte çalışması gerekir. Bu ortak işlemi etkinleştirmek için `AppDelegate.cs` dosyasını aşağıdaki yöntemi geçersiz kılmak üzere güncelleştirin.

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

Bu yöntem, uygulama her başlatıldığında çağrılır. Aracıdan gelen yanıtı işlemek ve MSAL.NET 'in başladığı kimlik doğrulama işlemini tamamlamaya yönelik bir fırsat olarak kullanılır.

### <a name="step-4-set-uiviewcontroller"></a>4\. Adım: UIViewController () ayarlama
Hala `AppDelegate.cs` dosyasında bir nesne penceresi ayarlamanız gerekir. Normalde, Xamarin iOS için nesne penceresini ayarlamanız gerekmez. Ancak aracıdan yanıt göndermek ve almak için bir nesne penceresi gerekir. 

Nesne penceresini ayarlamak için: 
1. `AppDelegate.cs` dosyasında, `App.RootViewController` yeni bir `UIViewController()`olarak ayarlayın. Bu atama, aracı çağrısının `UIViewController`içerir olmasını sağlar. Bu ayar yanlış atanmışsa, şu hatayı alabilirsiniz:

      `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`

1. `AcquireTokenInteractive` çağrısında `.WithParentActivityOrWindow(App.RootViewController)` ' i kullanın ve sonra kullanacağınız nesne penceresine geçirin.

    `App.cs` içinde:
    
    ```csharp
       public static object RootViewController { get; set; }
    ```
    
    `AppDelegate.cs` içinde:
    
    ```csharp
       LoadApplication(new App());
       App.RootViewController = new UIViewController();
    ```
    
    `AcquireToken` çağrısında:
    
    ```csharp
    result = await app.AcquireTokenInteractive(scopes)
                 .WithParentActivityOrWindow(App.RootViewController)
                 .ExecuteAsync();
    ```

### <a name="step-5-register-a-url-scheme"></a>5\. Adım: bir URL düzenini kaydetme
MSAL.NET, aracıyı çağırmak için URL 'Ler kullanır ve sonra, aracı yanıtını uygulamanıza döndürür. Gidiş dönüş işleminin tamamlanabilmesi için, `Info.plist` dosyasına uygulamanız için bir URL düzeni kaydedin.

`CFBundleURLSchemes` adı, ön ek olarak `msauth.` içermelidir. `CFBundleURLName`ile öneki izleyin. 

`BundleId`, URL şemasında, uygulamayı benzersiz olarak tanımlar: `$"msauth.(BundleId)"`. `BundleId` `com.yourcompany.xforms`, URL şeması `msauth.com.yourcompany.xforms`olur.

> [!NOTE]
> Bu URL şeması, Aracıdan yanıtı aldığında uygulamanızı benzersiz bir şekilde tanımlayan yeniden yönlendirme URI 'sinin bir parçası haline gelir.

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

### <a name="step-6-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>6\. Adım: aracı tanımlayıcısını Lsapplicationqueriesdüzenlerinin bölümüne ekleme
MSAL, aracının cihazda yüklü olup olmadığını denetlemek için `–canOpenURL:` kullanır. İOS 9 ' da, Apple bir uygulamanın sorgulayabilme düzenlerini kilitlemiş. 

Aşağıdaki örnekte olduğu gibi `Info.plist` dosyasının `LSApplicationQueriesSchemes` bölümüne `msauthv2` ekleyin:

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
    </array>
```

### <a name="step-7-register-your-redirect-uri-in-the-application-portal"></a>7\. Adım: yeniden yönlendirme URI 'nizi uygulama portalına kaydetme
Aracıyı kullandığınızda, yeniden yönlendirme URI 'nizin ek bir gereksinimi vardır. Yeniden yönlendirme URI 'SI aşağıdaki biçime sahip _olmalıdır_ :
```csharp
$"msauth.{BundleId}://auth"
```

Bir örneği aşağıda verilmiştir: 

```csharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
Yeniden yönlendirme URI 'sinin, `Info.plist` dosyasına dahil ettiğiniz `CFBundleURLSchemes` adıyla eşleştiğinden emin olun.

### <a name="step-8-make-sure-the-redirect-uri-is-registered-with-your-app"></a>8\. Adım: yeniden yönlendirme URI 'sinin uygulamanıza kayıtlı olduğundan emin olun

Yeniden yönlendirme URI 'sinin, uygulamanız için geçerli bir yeniden yönlendirme URI 'SI olarak [uygulama kayıt portalı](https://portal.azure.com) 'nda kayıtlı olması gerekir. 

Uygulama kayıt portalı, paket KIMLIĞINDEN aracılı yanıt URI 'sini hesaplamanıza yardımcı olacak yeni bir deneyim sunar. 

Yeniden yönlendirme URI 'sini hesaplamak için:

1. Uygulama kayıt portalında **kimlik doğrulaması** ' nı seçin > **Yeni deneyimi deneyin**.

   ![Yeni uygulama kayıt deneyimini deneyin](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

1. **Platform Ekle**' yi seçin.

   ![Platform ekleme](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

1. Platformların listesi desteklenmiş olduğunda **iOS**' ı seçin.

   ![İOS 'ı yapılandırma](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

1. Paket KIMLIĞINIZI istenen şekilde girin ve ardından **Yapılandır**' ı seçin.

   ![Paket KIMLIĞINI girin](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

Adımları tamamladığınızda, yeniden yönlendirme URI 'SI sizin için hesaplanır.

![Yeniden yönlendirme URI 'sini Kopyala](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Android için aracılı kimlik doğrulaması

MSAL.NET yalnızca Xamarin. iOS platformunu destekler. Bu, Xamarin. Android platformu için aracıları henüz desteklememektedir.

MSAL Android yerel kitaplığı aracılı kimlik doğrulamasını zaten destekliyor. Daha fazla bilgi için bkz. [Android 'de aracılı kimlik doğrulaması](brokered-auth.md).

## <a name="next-steps"></a>Sonraki adımlar

[Msal.NET ile Evrensel Windows platformu kullanma hakkında konular](msal-net-uwp-considerations.md)hakkında bilgi edinin.
