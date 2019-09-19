---
title: Xamarin iOS ve Android uygulamalarında Microsoft Authenticator veya Microsoft Intune Şirket Portalı kullanma | Mavisi
description: .NET için Azure AD kimlik doğrulama kitaplığı 'nı (ADAL.NET) .NET için Microsoft kimlik doğrulama kitaplığı 'na (MSAL.NET) kadar Microsoft Authenticator kullanılabilecek Xamarin iOS uygulamalarını nasıl geçirebileceğinizi öğrenin
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
ms.openlocfilehash: abac7b95ceed0a199531b7ba30cea5fc1c9cc1e0
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71103980"
---
# <a name="use-microsoft-authenticator-or-microsoft-intune-company-portal-on-xamarin-applications"></a>Xamarin uygulamalarında Microsoft Authenticator veya Microsoft Intune Şirket Portalı kullanma

Android ve iOS 'ta Microsoft Authenticator veya Microsoft Intune gibi aracılar Şirket Portalı etkinleştir (yalnızca Android):

- Çoklu oturum açma (SSO). Kullanıcılarınızın her uygulamada oturum açması gerekmez.
- Cihaz kimliği. Aracı, çalışma alanına katılmış olduğunda cihazda oluşturulan cihaz sertifikasına erişir.
- Uygulama tanımlama doğrulaması. Bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir ve aracı tarafından doğrulanır.

Bu özelliklerden birini etkinleştirmek için, uygulama geliştiricilerinin `WithBroker()` `PublicClientApplicationBuilder.CreateApplication` yöntemi çağırdıkları zaman parametresini kullanması gerekir. `.WithBroker()`Varsayılan olarak true olarak ayarlanır. Geliştiricilerin Ayrıca [iOS](#brokered-authentication-for-ios) veya [Android](#brokered-authentication-for-android) uygulamaları için buradaki adımları izlemesi gerekir.

## <a name="brokered-authentication-for-ios"></a>İOS için aracılı kimlik doğrulaması

Xamarin. iOS uygulamanızın [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) uygulamayla iletişim kurmasını sağlamak için aşağıdaki adımları izleyin.

### <a name="step-1-enable-broker-support"></a>1\. adım: Aracı desteğini etkinleştir
Aracı desteği, PublicClientApplication temelinde etkinleştirilir. Varsayılan olarak devre dışıdır. Publicclientapplicationbuilder aracılığıyla publicclientapplication oluştururken parametresini kullanın (varsayılan olarak true olarak ayarlanır). `WithBroker()`

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>2\. adım: Geri aramayı işlemek için AppDelegate 'i Güncelleştir
.NET için Microsoft kimlik doğrulama kitaplığı (msal.net) Aracısı 'nı çağırdığında, içindeki aracı, `OpenUrl` `AppDelegate` sınıfının yöntemi aracılığıyla uygulamanıza geri çağırır. MSAL, aracıdan gelen yanıtı beklediği için uygulamanızın MSAL.NET geri çağırmak için birlikte çalışması gerekir. Bu ortak işlemi etkinleştirmek için, aşağıdaki yöntemi `AppDelegate.cs` geçersiz kılmak üzere dosyayı güncelleştirin.

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

Bu yöntem, uygulama her başlatıldığında çağrılır. Aracıdan gelen yanıtı işlemek ve MSAL.NET tarafından başlatılan kimlik doğrulama işlemini tamamlamaya yönelik bir fırsat olarak kullanılır.

### <a name="step-3-set-a-uiviewcontroller"></a>3\. adım: UIViewController () ayarlama
Hala içinde `AppDelegate.cs`, bir nesne penceresi ayarlamanız gerekir. Normalde, Xamarin iOS ile nesne penceresini ayarlamanız gerekmez. Aracıdan yanıt göndermek ve almak için bir nesne penceresi gerekir. 

Bunu yapmak için iki şey yapmanız gerekir. 
1. İçinde `AppDelegate.cs`, `App.RootViewController` öğesini Yeni`UIViewController()`olarak ayarlayın. Bu atama, aracıya yönelik çağrıya sahip bir UIViewController olduğundan emin olmanızı sağlar. Doğru ayarlanmamışsa şu hatayı alabilirsiniz:`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
1. Acquiretokenınteractive çağrısında, `.WithParentActivityOrWindow(App.RootViewController)` öğesini kullanın ve kullanacağınız nesne penceresine yönelik başvuruyu geçirin.

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

### <a name="step-4-register-a-url-scheme"></a>4\. Adım: URL şeması kaydetme
MSAL.NET, aracıyı çağırmak için URL 'Ler kullanır ve ardından aracı yanıtını uygulamanıza geri döndürür. Gidiş dönüş işleminin tamamlanabilmesi için, `Info.plist` dosyaya uygulamanız için bir URL düzeni kaydedin.

Ad, ön eki `msauth.` olarak içermeli ve arkasından. `CFBundleURLName` `CFBundleURLSchemes`

`$"msauth.(BundleId)"`

**Örneğin:**

`msauth.com.yourcompany.xforms`

> [!NOTE]
> Bu URL şeması, Aracıdan yanıtı aldığında uygulamanızı benzersiz şekilde tanımlamak için kullanılan yeniden yönlendirme URI 'sinin bir parçası haline gelir.

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

### <a name="step-5-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>5\. Adım: Aracı tanımlayıcısını Lsapplicationqueriesdüzenlerinin bölümüne ekleyin
MSAL, `–canOpenURL:` aracının cihaza yüklenip yüklenmediğini denetlemek için kullanır. İOS 9 ' da, Apple bir uygulamanın sorgulayabilecekleri düzenleri kilitlemiş. 

Dosyanın bölümüne ekleyin `msauthv2`. `LSApplicationQueriesSchemes` `Info.plist`

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="step-6-register-your-redirect-uri-in-the-application-portal"></a>6\. Adım: Yeniden yönlendirme URI 'nizi uygulama portalına kaydetme
Aracının kullanılması, yeniden yönlendirme URI 'nize ek bir gereksinim ekler. Yeniden yönlendirme URI 'SI aşağıdaki biçime sahip _olmalıdır_ :
```CSharp
$"msauth.{BundleId}://auth"
```
**Örneğin:**
```CSharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
Yeniden yönlendirme URI 'sinin, `CFBundleURLSchemes` `Info.plist` dosyaya dahil ettiğiniz adla eşleştiğinden emin olun.

### <a name="step-7-make-sure-the-redirect-uri-is-registered-with-your-app"></a>7\. Adım: Yeniden yönlendirme URI 'sinin uygulamanıza kayıtlı olduğundan emin olun

Bu yeniden yönlendirme URI 'sinin, uygulamanız için geçerli bir yeniden yönlendirme URI https://portal.azure.com) 'si olarak, uygulama kayıt portalı 'nda kayıtlı olması gerekir. 

Portalda aracılı yanıt URI 'sini paket KIMLIĞINDEN hesaplamanıza yardımcı olacak yeni bir deneyim uygulama kayıt portalı vardır.

1. Uygulama kaydında **kimlik doğrulaması** ' nı seçin ve **Yeni deneyimi deneyin**' i seçin.

   ![Yeni uygulama kayıt deneyimini deneyin](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

1. **Platform Ekle**' yi seçin.

   ![Platform ekleme](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

1. Platformların listesi desteklenmiş olduğunda **iOS**' ı seçin.

   ![İOS 'ı yapılandırma](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

1. Paket KIMLIĞINIZI istenen şekilde girin ve ardından **Yapılandır**' ı seçin.

   ![Paket KIMLIĞINI girin](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

1. Yeniden yönlendirme URI 'SI sizin için hesaplanır.

   ![Yeniden yönlendirme URI 'sini Kopyala](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Android için aracılı kimlik doğrulaması

Aracı desteği Android için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

[Msal.NET ile ilgili Evrensel Windows platformu özgü konular](msal-net-uwp-considerations.md)hakkında bilgi edinin.