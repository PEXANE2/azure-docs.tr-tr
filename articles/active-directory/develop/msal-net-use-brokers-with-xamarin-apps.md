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
ms.openlocfilehash: 1a57173311278c5e3e0304aeb12d4d6999379eb5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84692334"
---
# <a name="use-microsoft-authenticator-or-intune-company-portal-on-xamarin-applications"></a>Xamarin uygulamalarında Microsoft Authenticator veya Intune Şirket Portalı kullanma

Android ve iOS 'ta Microsoft Authenticator ve Android 'e özgü Microsoft Intune gibi aracılar Şirket Portalı etkinleştirir:

- **Çoklu oturum açma (SSO)**: kullanıcıların her uygulamada oturum açması gerekmez.
- **Cihaz kimliği**: aracı cihaz sertifikasına erişir. Bu sertifika, çalışma alanına katıldığında cihazda oluşturulur.
- **Uygulama tanımlama doğrulaması**: bir uygulama aracıyı çağırdığında, yeniden yönlendirme URL 'sini geçirir. Aracı URL 'YI doğrular.

Bu özelliklerden birini etkinleştirmek için, `WithBroker()` yöntemini çağırdığınızda parametresini kullanın `PublicClientApplicationBuilder.CreateApplication` . `.WithBroker()`Parametresi varsayılan olarak true olarak ayarlanır. 

Ayrıca [iOS](#brokered-authentication-for-ios) uygulamaları veya [Android](#brokered-authentication-for-android) uygulamaları için aracılı kimlik doğrulaması ayarlamak üzere aşağıdaki bölümlerdeki yönergeleri kullanın.

## <a name="brokered-authentication-for-ios"></a>İOS için aracılı kimlik doğrulaması

Xamarin. iOS uygulamanızın [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) uygulamayla iletişim kurmasını sağlamak için aşağıdaki adımları kullanın.

### <a name="step-1-enable-broker-support"></a>1. Adım: aracı desteğini etkinleştirme
Ayrı örnekleri için aracı desteğini etkinleştirmeniz gerekir `PublicClientApplication` . Destek varsayılan olarak devre dışıdır. `PublicClientApplication`Aracılığıyla oluşturduğunuzda `PublicClientApplicationBuilder` , `WithBroker()` Aşağıdaki örnekte gösterildiği gibi parametresini kullanın. `WithBroker()`Parametresi varsayılan olarak true olarak ayarlanır.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-enable-keychain-access"></a>2. Adım: Anahtarlık erişimini etkinleştirme

Anahtarlık erişimini etkinleştirmek için, uygulamanız için bir Anahtarlık erişim grubunuz olmalıdır. `WithIosKeychainSecurityGroup()`Uygulamanızı oluştururken Anahtarlık erişim grubunuzu ayarlamak için API 'yi kullanabilirsiniz:

```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
      
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Daha fazla bilgi için bkz. [Anahtarlık erişimini etkinleştirme](msal-net-xamarin-ios-considerations.md#enable-keychain-access).

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>3. Adım: geri aramayı işlemek için AppDelegate 'i güncelleştirme
.NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) Aracısı 'nı çağırdığında, aracı, sınıfının yöntemi aracılığıyla uygulamanıza geri çağrı yapılır `OpenUrl` `AppDelegate` . MSAL, aracının yanıtını beklediği için uygulamanızın MSAL.NET geri çağırmak için birlikte çalışması gerekir. Bu ortak işlemi etkinleştirmek için, `AppDelegate.cs` aşağıdaki yöntemi geçersiz kılmak üzere dosyayı güncelleştirin.

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

### <a name="step-4-set-uiviewcontroller"></a>4. Adım: UIViewController () ayarlama
Hala `AppDelegate.cs` dosyada, bir nesne penceresi ayarlamanız gerekir. Normalde, Xamarin iOS için nesne penceresini ayarlamanız gerekmez. Ancak aracıdan yanıt göndermek ve almak için bir nesne penceresi gerekir. 

Nesne penceresini ayarlamak için: 
1. `AppDelegate.cs`Dosyasında, `App.RootViewController` yeni olarak ayarlayın `UIViewController()` . Bu atama, aracı çağrısının dahil edilmesini sağlar `UIViewController` . Bu ayar yanlış atanmışsa, şu hatayı alabilirsiniz:

      `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`

1. `AcquireTokenInteractive`Çağrıda, `.WithParentActivityOrWindow(App.RootViewController)` öğesini kullanın ve ardından kullanacağınız nesne penceresi başvurusunu geçirin.

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

### <a name="step-5-register-a-url-scheme"></a>5. Adım: bir URL düzenini kaydetme
MSAL.NET, aracıyı çağırmak için URL 'Ler kullanır ve sonra, aracı yanıtını uygulamanıza döndürür. Gidiş dönüş işleminin tamamlanabilmesi için, dosyaya uygulamanız için bir URL düzeni kaydedin `Info.plist` .

`CFBundleURLSchemes`Ad, `msauth.` ön ek olarak içermeli. Önekini ile izleyin `CFBundleURLName` . 

URL şemasında, `BundleId` uygulamayı benzersiz olarak tanımlar: `$"msauth.(BundleId)"` . Öyleyse `BundleId` `com.yourcompany.xforms` , URL şeması olur `msauth.com.yourcompany.xforms` .

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

### <a name="step-6-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>6. Adım: aracı tanımlayıcısını Lsapplicationqueriesdüzenlerinin bölümüne ekleme

MSAL, `–canOpenURL:` aracının cihazda yüklü olup olmadığını denetlemek için kullanır. İOS 9 ' da, Apple bir uygulamanın sorgulayabilme düzenlerini kilitlemiş. 

`msauthv2` `LSApplicationQueriesSchemes` `Info.plist` Aşağıdaki örnekteki gibi, dosyanın bölümüne ekleyin:

```XML
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
    </array>
```

### <a name="step-7-register-your-redirect-uri-in-the-application-portal"></a>7. Adım: yeniden yönlendirme URI 'nizi uygulama portalına kaydetme

Aracıyı kullandığınızda, yeniden yönlendirme URI 'nizin ek bir gereksinimi vardır. Yeniden yönlendirme URI 'SI aşağıdaki biçime sahip _olmalıdır_ :

```csharp
$"msauth.{BundleId}://auth"
```

İşte bir örnek:

```csharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```

Yeniden yönlendirme URI 'sinin, `CFBundleURLSchemes` dosyaya dahil ettiğiniz adla eşleştiğinden emin olun `Info.plist` .

### <a name="step-8-make-sure-the-redirect-uri-is-registered-with-your-app"></a>8. Adım: yeniden yönlendirme URI 'sinin uygulamanıza kayıtlı olduğundan emin olun

Yeniden yönlendirme URI 'sinin, uygulamanız için geçerli bir yeniden yönlendirme URI 'SI olarak [uygulama kayıt portalı](https://portal.azure.com) 'nda kayıtlı olması gerekir. 

Uygulama kayıt portalı, paket KIMLIĞINDEN aracılı yanıt URI 'sini hesaplamanıza yardımcı olacak yeni bir deneyim sunar. 

Yeniden yönlendirme URI 'sini hesaplamak için:

1. Uygulama kayıt portalında **kimlik doğrulama**' yı seçin,  >  **Yeni deneyimi deneyin**.

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

### <a name="step-1-enable-broker-support"></a>1. Adım: aracı desteğini etkinleştirme

Aracı desteği, PublicClientApplication temelinde etkinleştirilir. Varsayılan olarak devre dışıdır. `WithBroker()`Aracılığıyla oluştururken parametresini kullanın (varsayılan olarak true olarak ayarlanır) `IPublicClientApplication` `PublicClientApplicationBuilder` .

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithRedirectUri(redirectUriOnAndroid) //(see step 4 below)
                .Build();
```

### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>2. Adım: geri aramayı işlemek için AppDelegate 'i güncelleştirme

MSAL.NET aracı çağırdığında, aracı sırasıyla OnActivityResult () yöntemiyle uygulamanıza geri çağrı yapılır. MSAL, aracıdan gelen yanıtı bekleyecek için, uygulamanızın sonucu MSAL.NET 'e yönlendirmesi gerekir.
Bu, `SetAuthenticationContinuationEventArgs(int requestCode, Result resultCode, Intent data)` aşağıda gösterildiği gibi OnActivityResult () yöntemi geçersiz kılınarak sonucu öğesine yönlendirilerek elde edilebilir

```CSharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
   base.OnActivityResult(requestCode, resultCode, data);
   AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
}
```

Bu yöntem, aracı uygulaması her başlatıldığında çağrılır ve aracıdan gelen yanıtı işleme ve MSAL.NET tarafından başlatılan kimlik doğrulama işlemini tamamlamaya yönelik bir fırsat olarak kullanılır.

### <a name="step-3-set-an-activity"></a>3. Adım: etkinlik ayarlama

Aracılı kimlik doğrulamanın çalışması için, MSAL 'in aracıdan yanıt gönderebilmesi ve alabilmesi için bir etkinlik ayarlamanız gerekir.

Bunu yapmak için, etkinliği (genellikle MainActivity) `WithParentActivityOrWindow(object parent)` üst nesne olarak belirtmeniz gerekir. 

**Örneğin:**

Belirteç al çağrısında:

```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow((Activity)context))
             .ExecuteAsync();
```

### <a name="step-4-register-your-redirecturi-in-the-application-portal"></a>4. Adım: RedirectUri 'nizi uygulama portalına kaydetme

MSAL, aracıyı çağırmak ve sonra uygulamanıza geri dönmek için URL 'Ler kullanır. Bu gidiş dönüş işleminin tamamlanabilmesi için, uygulamanız için bir URL şeması kaydetmeniz gerekir. Bu yeniden yönlendirme URI 'sinin, uygulamanız için geçerli bir yeniden yönlendirme URI 'SI olarak Azure AD uygulama kayıt portalı 'nda kayıtlı olması gerekir.


Uygulamanız için gereken yeniden yönlendirme URI 'SI, APK 'yi imzalamak için kullanılan sertifikaya bağımlıdır.

```
Example: msauth://com.microsoft.xforms.testApp/hgbUYHVBYUTvuvT&Y6tr554365466=
```

URI 'nin son bölümü, `hgbUYHVBYUTvuvT&Y6tr554365466=` APK 'nin imzalandığı, Base64 kodlamalı imza olur.
Bununla birlikte, Visual Studio 'Yu kullanarak uygulamanızın geliştirme aşamasında, belirli bir sertifikayla APK imzalamadan kodunuzda hata ayıklaması yapıyorsanız, Visual Studio, hata ayıklama amacıyla APK 'yı imzalayıp oluşturulduğu makine için benzersiz bir imza sağlar. Bu nedenle, uygulamanızı farklı bir makinede derlediğinizde, MSAL ile kimlik doğrulamak için uygulamanın kodundaki yeniden yönlendirme URI 'sini ve uygulamanın Azure portal kaydını güncelleştirmeniz gerekir. 

Hata ayıklama sırasında, belirtilen yeniden yönlendirme URI 'sinin yanlış olduğunu belirten bir MSAL özel durumu (veya günlük iletisi) ile karşılaşabilirsiniz. Bu özel durum Ayrıca, üzerinde hata ayıkladığınız geçerli makineyle birlikte kullanmanız **gereken yeniden yönlendirme URI 'si sağlar** . Bu yeniden yönlendirme URI 'sini kullanarak geliştirmeye devam edebilirsiniz.

Kodunuzu sonuçlamaya hazırladıktan sonra, koddaki yeniden yönlendirme URI 'sini güncelleştirdiğinizden emin olun ve Azure portal sertifikanın imzasını kullanmak için, ile APK 'yi imzalamış olursunuz.

Uygulamada, bu, ekibin her üyesi için bir yeniden yönlendirme URI 'SI ve APK 'nin üretim imzalı sürümü için bir yeniden yönlendirme URI 'si kaydetmeniz gerektiği anlamına gelir.

Ayrıca, MSAL şuna benzer şekilde, bu imzayı kendiniz de hesaplamanız gerekir: 

```CSharp
   private string GetRedirectUriForBroker()
   {
      string packageName = Application.Context.PackageName;
      string signatureDigest = this.GetCurrentSignatureForPackage(packageName);
      if (!string.IsNullOrEmpty(signatureDigest))
      {
            return string.Format(CultureInfo.InvariantCulture, "{0}://{1}/{2}", RedirectUriScheme,
               packageName.ToLowerInvariant(), signatureDigest);
      }

      return string.Empty;
   }

   private string GetCurrentSignatureForPackage(string packageName)
   {
            PackageInfo info = Application.Context.PackageManager.GetPackageInfo(packageName,
               PackageInfoFlags.Signatures);
            if (info != null && info.Signatures != null && info.Signatures.Count > 0)
            {
               // First available signature. Applications can be signed with multiple signatures.
               // The order of Signatures is not guaranteed.
               Signature signature = info.Signatures[0];
               MessageDigest md = MessageDigest.GetInstance("SHA");
               md.Update(signature.ToByteArray());
               return Convert.ToBase64String(md.Digest(), Base64FormattingOptions.None);
               // Server side needs to register all other tags. ADAL will
               // send one of them.
            }
   }
```

Aşağıdaki komutlarla Keytool kullanarak paketinizin imzasını alma seçeneğiniz de vardır:

Windows için:`keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android`

Mac için:`keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64`

## <a name="next-steps"></a>Sonraki adımlar

[Msal.NET ile Evrensel Windows platformu kullanma hakkında konular](msal-net-uwp-considerations.md)hakkında bilgi edinin.
