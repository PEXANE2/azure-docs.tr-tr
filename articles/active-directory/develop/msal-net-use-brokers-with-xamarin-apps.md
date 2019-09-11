---
title: Xamarin iOS ve Android uygulamalarında Microsoft Authenticator veya Microsoft Intune şirket portalı 'nı kullanma | Mavisi
description: Xamarin iOS uygulamalarının nasıl geçirileceğiyle ilgili bilgi edinin .NET için Azure AD kimlik doğrulama kitaplığı (ADAL.NET), .NET için Microsoft kimlik doğrulama kitaplığı 'na (MSAL.NET) nasıl Microsoft Authenticator kullanabileceğinizi öğrenin
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
ms.openlocfilehash: e16ad801b3b691d942c2ba0dc723ba72e24cf4c0
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70875646"
---
# <a name="use-microsoft-authenticator-or-microsoft-intune-company-portal-on-xamarin-applications"></a>Xamarin uygulamalarında Microsoft Authenticator veya Microsoft Intune şirket portalı 'nı kullanma

Android ve iOS 'ta Microsoft Authenticator veya Microsoft Intune şirket portalı etkinleştir gibi aracılar (yalnızca Android):

- Çoklu oturum açma (SSO). Kullanıcılarınızın her uygulamada oturum açması gerekmez.
- Cihaz kimliği. Cihazda, çalışma alanına katılmış olduğunda oluşturulan cihaz sertifikasına erişerek.
- Uygulama tanımlama doğrulaması. Bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir ve aracı tarafından doğrulanır.

Bu özelliklerden birini etkinleştirmek için, uygulama geliştiricilerinin `WithBroker()` `PublicClientApplicationBuilder.CreateApplication` yöntemi çağırırken parametresini kullanması gerekir. `.WithBroker()`Varsayılan olarak true olarak ayarlanır. Geliştiricilerin Ayrıca [iOS](#brokered-authentication-for-ios) veya [Android](#brokered-authentication-for-android) uygulamaları için aşağıdaki adımları izlemesi gerekecektir.

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
MSAL.NET aracı çağırdığında, aracı, `OpenUrl` `AppDelegate` sınıfının yöntemi aracılığıyla uygulamanıza geri çağrı yapılır. MSAL, aracıdan gelen yanıtı bekleyecek için, uygulamanızın MSAL.NET geri çağırmak için birlikte çalışması gerekir. Bu ortak işlemi etkinleştirmek için, `AppDelegate.cs` dosyayı aşağıdaki yöntemi geçersiz kılmak üzere güncelleştirin.

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

### <a name="step-3-set-a-uiviewcontroller"></a>3\. adım: UIViewController () ayarlama
Hala içinde `AppDelegate.cs`, bir nesne penceresi ayarlamanız gerekir. Normalde, Xamarin iOS ile nesne penceresini ayarlamanız gerekmez, ancak aracıdan yanıt göndermek ve almak için bir nesne penceresi gerekir. 

Bunu yapmak için iki şey yapmanız gerekir. 
1) İçinde `AppDelegate.cs`, `App.RootViewController` öğesini Yeni`UIViewController()`olarak ayarlayın. Bu atama, aracıya yönelik çağrıya sahip bir UIViewController olduğundan emin olmanızı sağlar. Doğru ayarlanmamışsa şu hatayı alabilirsiniz:`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
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

### <a name="step-4-register-a-url-scheme"></a>4\. Adım: URL şeması kaydetme
MSAL.NET, aracıyı çağırmak için URL 'Ler kullanır ve ardından aracı yanıtını uygulamanıza geri döndürür. Gidiş dönüş işleminin tamamlanabilmesi için, `Info.plist` dosyaya uygulamanız için bir URL şeması kaydetmeniz gerekir.

Ad, ön eki `msauth.` olarak içermeli ve arkasından. `CFBundleURLName` `CFBundleURLSchemes`

`$"msauth.(BundleId)"`

**Örneğin:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Bu, Aracıdan yanıtı alırken uygulamanızı benzersiz bir şekilde tanımlamak için kullanılan Redirecturı 'nin bir parçası olur.

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

### <a name="step-5-lsapplicationqueriesschemes"></a>5\. Adım: Lsapplicationqueriesdüzenleri
MSAL, `–canOpenURL:` aracının cihaza yüklenip yüklenmediğini denetlemek için kullanır. İOS 9 ' da, Apple bir uygulamanın sorgulayabilecekleri düzenleri kilitlemiş. 

**Ekle** **`msauthv2`dosyasınınbölümünegidin.** `LSApplicationQueriesSchemes` `Info.plist`

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="step-6-register-your-redirecturi-in-the-application-portal"></a>6\. Adım: Uygulama portalına Redirecturı 'yi kaydetme
Aracının kullanılması, redirectUri 'nize ek bir gereksinim ekler. RedirectUri aşağıdaki biçimde _**olmalıdır**_ :
```CSharp
$"msauth.{BundleId}://auth"
```
**Örneğin:**
```CSharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
**Redirecturı 'nin `CFBundleURLSchemes` `Info.plist` dosyaya eklediğiniz adla eşleştiğini fark edeceksiniz.**

### <a name="step-7-make-sure-the-redirect-uri-is-registered-with-your-app"></a>7\. Adım: yeniden yönlendirme URI 'sinin uygulamanıza kayıtlı olduğundan emin olun

Bu yeniden yönlendirme URI 'sinin, uygulamanız için geçerli bir yeniden yönlendirme URI https://portal.azure.com) 'si olarak, uygulama kayıt portalı 'nda kayıtlı olması gerekir. 

Portalın, paket KIMLIĞINDEN aracılı yanıt URI 'sini hesaplamanıza yardımcı olmak için yeni bir deneyim uygulama kayıt portalı vardır:

1. Uygulama kaydında **kimlik doğrulaması** ' nı seçin ve **Yeni deneyimi dene**' yi seçin.
   ![Yeni uygulama kayıt deneyimini deneyin](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

2. **Platform Ekle**' yi seçin.
   ![Platform ekleme](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

3. Platformların listesi desteklenmiş olduğunda **iOS**' ı seçin.
   ![İOS 'ı yapılandırma](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

4. Paket KIMLIĞINIZI istenen şekilde girin ve **Kaydet**' e basın.
   ![Paket KIMLIĞINI girin](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

5. Yeniden yönlendirme URI 'SI sizin için hesaplanır.
   ![Yeniden yönlendirme URI 'sini Kopyala](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Android için aracılı kimlik doğrulaması

Aracı desteği Android için kullanılamaz

## <a name="next-steps"></a>Sonraki adımlar

[MSAL.NET ile ilgili Evrensel Windows Platformu özel noktalar](msal-net-uwp-considerations.md)