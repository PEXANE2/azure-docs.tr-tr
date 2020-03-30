---
title: Xamarin iOS & Android ile broker kullanın | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Authenticator ve Microsoft Authentication Library'yi kullanabilen Xamarin iOS uygulamalarını nasıl kurabileceğinizi öğrenin. Ayrıca,.NET (ADAL.NET) için Azure AD Kimlik Doğrulama Kitaplığı'ndan Microsoft Authentication Library'ye .NET (MSAL.NET) için nasıl geçiş yapılacağını da öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262795"
---
# <a name="use-microsoft-authenticator-or-intune-company-portal-on-xamarin-applications"></a>Xamarin uygulamalarında Microsoft Authenticator veya Intune Company Portalını kullanma

Android ve iOS'ta, Microsoft Authenticator ve Android'e özel Microsoft Intune Company Portal gibi aracılar şunları sağlar:

- **Tek oturum açma (SSO)**: Kullanıcıların her uygulamada oturum açmalarına gerek yoktur.
- **Cihaz tanımlaması**: Aracı cihaz sertifikasına erişer. Bu sertifika, işyerine katıldığında aygıtta oluşturulur.
- **Uygulama tanımlama doğrulaması**: Bir uygulama aracıyı aradığında, yeniden yönlendirme URL'sini geçer. Aracı URL'yi doğrular.

Bu özelliklerden birini etkinleştirmek `WithBroker()` için yöntemi ararken parametreyi `PublicClientApplicationBuilder.CreateApplication` kullanın. `.WithBroker()` Parametre varsayılan olarak doğru olarak ayarlanır. 

Ayrıca, [iOS](#brokered-authentication-for-ios) uygulamaları veya [Android](#brokered-authentication-for-android) uygulamaları için aracılı kimlik doğrulaması ayarlamak için aşağıdaki bölümlerdeki yönergeleri kullanın.

## <a name="brokered-authentication-for-ios"></a>iOS için aracılı kimlik doğrulama

Xamarin.iOS uygulamanızın [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) uygulamasıyla konuşmasını sağlamak için aşağıdaki adımları kullanın.

### <a name="step-1-enable-broker-support"></a>Adım 1: Broker desteğini etkinleştirme
Tek tek örnekleri için broker `PublicClientApplication`desteğini etkinleştirmelisiniz. Destek varsayılan olarak devre dışı bırakılır. Bunu `PublicClientApplication` `PublicClientApplicationBuilder`oluştururken, aşağıdaki `WithBroker()` örnekte görüldüğü gibi parametreyi kullanın. `WithBroker()` Parametre varsayılan olarak doğru olarak ayarlanır.

```csharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-enable-keychain-access"></a>Adım 2: Anahtarlık erişimini etkinleştirme

Anahtarlık erişimini etkinleştirmek için, uygulamanız için bir anahtarlık erişim grubuna sahip olmalısınız. Uygulamanızı oluştururken anahtarzinciri erişim grubunuzu ayarlamak için `WithIosKeychainSecurityGroup()` API'yi kullanabilirsiniz:

```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
      
     .WithIosKeychainSecurityGroup("com.microsoft.adalcache")
     .Build();
```

Daha fazla bilgi için [bkz.](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>Adım 3: Geri aramayı işlemek için AppDelegate'i güncelleştirme
.NET için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.NET) aracıyı aradığında, `OpenUrl` broker `AppDelegate` sınıfın yöntemi yle uygulamanıza geri çağrır. MSAL komisyoncunun yanıtını beklediği için, başvurunuzun MSAL.NET geri çağırmak için işbirliği yapması gerekir. Bu işbirliğini etkinleştirmek `AppDelegate.cs` için, aşağıdaki yöntemi geçersiz kılmak için dosyayı güncelleştirin.

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

Bu yöntem, uygulama her başlatıldığında çağrılır. Komisyoncudan gelen yanıtı işlemek ve MSAL.NET başlattığı kimlik doğrulama işlemini tamamlamak için bir fırsat olarak kullanılır.

### <a name="step-4-set-uiviewcontroller"></a>Adım 4: UIViewController() ayarlayın
Dosyada `AppDelegate.cs` hala bir nesne penceresi ayarlamanız gerekir. Normalde Xamarin iOS için nesne penceresini ayarlamanız gerekmez. Ancak komisyoncudan yanıt göndermek ve almak için bir nesne penceresine ihtiyacınız vardır. 

Nesne penceresini ayarlamak için: 
1. Dosyada, `AppDelegate.cs` `App.RootViewController` yeni `UIViewController()`bir . Bu atama, komisyoncuya yapılan çağrının `UIViewController`. Bu ayar yanlış atanırsa, bu hatayı alabilirsiniz:

      `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`

1. Aramada, `AcquireTokenInteractive` kullanacağınız nesne penceresine başvuruyu kullanın `.WithParentActivityOrWindow(App.RootViewController)` ve aktarın.

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

### <a name="step-5-register-a-url-scheme"></a>Adım 5: URL düzenini kaydetme
MSAL.NET aracıyı çağırmak ve ardından aracı yanıtı uygulamanıza döndürmek için URL'leri kullanır. Gidiş-dönüş yolculuğunu tamamlamak için, dosyaya uygulamanız için bir URL şeması kaydedin. `Info.plist`

Ad `CFBundleURLSchemes` önek `msauth.` olarak içermelidir. öneki ile `CFBundleURLName`izleyin. 

URL düzeninde, `BundleId` uygulamayı benzersiz bir `$"msauth.(BundleId)"`şekilde tanımlar: . Yani `BundleId` ise `com.yourcompany.xforms`, o zaman `msauth.com.yourcompany.xforms`URL düzeni .

> [!NOTE]
> Bu URL düzeni, aracıdan yanıt aldığında uygulamanızı benzersiz olarak tanımlayan yeniden yönlendirme URI'nin bir parçası olur.

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

### <a name="step-6-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>Adım 6: LSApplicationQueriesSchemes bölümüne broker tanımlayıcısı ekleyin

MSAL, `–canOpenURL:` aracının aygıta yüklü olup olmadığını kontrol etmek için kullanır. iOS 9'da Apple, bir uygulamanın sorgulayabileceği şemaları kilitledi. 

Aşağıdaki örnekte olduğu gibi, dosyanın bölümüne `msauthv2` `LSApplicationQueriesSchemes` ekleyin: `Info.plist`

```XML
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
    </array>
```

### <a name="step-7-register-your-redirect-uri-in-the-application-portal"></a>Adım 7: Yeniden yönlendirme URI'nizi başvuru portalına kaydedin

Aracıyı kullandığınızda, yeniden yönlendirme URI'nizin ekstra bir gereksinimi vardır. Yeniden yönlendirme URI aşağıdaki biçime sahip _olmalıdır:_

```csharp
$"msauth.{BundleId}://auth"
```

Bir örneği aşağıda verilmiştir:

```csharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```

Yeniden yönlendirme URI'nin `CFBundleURLSchemes` dosyaya dahil ettiğiniz `Info.plist` adla eşleştiğinden dikkat edin.

### <a name="step-8-make-sure-the-redirect-uri-is-registered-with-your-app"></a>Adım 8: Yeniden yönlendirme URI'nin uygulamanızda kayıtlı olduğundan emin olun

Yeniden yönlendirme URI'nin uygulamanız için geçerli bir yönlendirme URI olarak [uygulama kayıt portalına](https://portal.azure.com) kaydedilmesi gerekir. 

Uygulama kayıt portalı, paket kimliğinden aracılı yanıt URI'yi hesaplamanıza yardımcı olacak yeni bir deneyim sağlar. 

URI'yi yeniden yönlendirmek için:

1. Uygulama kayıt portalında **Kimlik Doğrulama'yı** > seçin**yeni deneyimi deneyin.**

   ![Yeni uygulama kaydı deneyimini deneyin](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

1. **Platform ekle'yi**seçin.

   ![Platform ekleme](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

1. Platform listesi desteklendiğinde **iOS'u**seçin.

   ![iOS'u yapılandırma](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

1. Paket kimliğinizi istendiği gibi girin ve sonra **Yapılandır'ı**seçin.

   ![Paket kimliğini girin](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

Adımları bitirdiğinizde, yeniden yönlendirme URI sizin için hesaplanır.

![Uri yeniden yönlendirme kopyala](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Android için aracılı kimlik doğrulaması

### <a name="step-1-enable-broker-support"></a>Adım 1: Broker desteğini etkinleştirme

Broker desteği, PublicClientApplication bazında etkinleştirilir. Varsayılan olarak devre dışı bırakılır. Parametreyi `WithBroker()` (varsayılan olarak doğru olarak ayarlanmış) kullanarak. `PublicClientApplicationBuilder` `IPublicClientApplication`

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithRedirectUri(redirectUriOnAndroid) //(see step 4 below)
                .Build();
```

### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>Adım 2: Geri aramayı işlemek için AppDelegate'i güncelleştirme

MSAL.NET broker'ı aradığında, broker da OnActivityResult() yöntemiyle başvurunuza geri çağrıyapacaktır. MSAL broker dan yanıt bekleyecek bu yana, uygulamanızın MSAL.NET sonucu yönlendirmek gerekir.
Bu, aşağıda gösterildiği gibi OnActivityResult() yöntemini geçersiz `SetAuthenticationContinuationEventArgs(int requestCode, Result resultCode, Intent data)` kılarak sonucu yönlendirme ile elde edilebilir

```CSharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
   base.OnActivityResult(requestCode, resultCode, data);
   AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
}
```

Bu yöntem, broker uygulaması her başlatıldığında çağrılır ve broker dan yanıt işlemek ve MSAL.NET tarafından başlatılan kimlik doğrulama işlemini tamamlamak için bir fırsat olarak kullanılır.

### <a name="step-3-set-an-activity"></a>Adım 3: Etkinlik Ayarlama

Aracılı kimlik doğrulamanın çalışması için, MSAL'ın yanıtı brokerdan gönderip alabilmesi için bir etkinlik ayarlamanız gerekir.

Bunu yapmak için, etkinliği (genellikle MainActivity) ana nesne `WithParentActivityOrWindow(object parent)` olarak sağlamanız gerekir. 

**Örneğin:**

Edinme Belirteci çağrısında:

```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow((Activity)context))
             .ExecuteAsync();
```

### <a name="step-4-register-your-redirecturi-in-the-application-portal"></a>Adım 4: RedirectUri'nizi başvuru portalına kaydedin

MSAL, aracıyı çağırmak ve ardından uygulamanıza geri dönmek için URL'leri kullanır. Bu gidiş-dönüş seyahatini tamamlamak için uygulamanız için bir URL düzeni kaydetmeniz gerekir. Bu Yönlendirme URI'nin, uygulamanız için geçerli bir yönlendirme URI olarak Azure AD uygulama kayıt portalına kaydedilmesi gerekir.


Uygulamanız için gereken yeniden yönlendirme URI, APK'yı imzalamak için kullanılan sertifikaya bağlıdır.

```
Example: msauth://com.microsoft.xforms.testApp/hgbUYHVBYUTvuvT&Y6tr554365466=
```

URI'nin son bölümü, `hgbUYHVBYUTvuvT&Y6tr554365466=`APK'nın imzalandığı imzadır, base64 kodlanır.
Ancak, Visual Studio'yu kullanarak uygulamanızın geliştirme aşamasında, apk'yi belirli bir sertifikayla imzalamadan kodunuzu hata ayıklıyorsanız, Visual Studio hata ayıklama amacıyla apk'yi imzalayarak APK'ya benzersiz bir imza üzerine inşa edilmiş bir makine. Bu nedenle, uygulamanızı farklı bir makinede her oluşturduğunuzda, MSAL ile kimlik doğrulaması yapmak için uygulamakodundaki yeniden yönlendirme URI'yi ve uygulamanın Azure portalına kaydını güncelleştirmeniz gerekir. 

Hata ayıklama sırasında, uri'nin sağladığı yeniden yönlendirmenin yanlış olduğunu belirten bir MSAL özel durumla (veya günlük iletisi) karşılaşabilirsiniz. Bu özel durum, hata ayıklama nız olan geçerli makineyle **birlikte kullanmanız gereken yeniden yönlendirme URI'yi de sağlar.** Bu yönlendirme URI'yi şimdilik geliştirmeye devam etmek için kullanabilirsiniz.

Kodunuzu tamamlamaya hazır olduğunuzda, APK ile imzalayacağınız sertifikan imzalanmasını kullanmak için koddaki ve uygulamanın Azure portalındaki başvuru dağıtımını gündirdürüm

Uygulamada bu, takımın her üyesi için bir yeniden yönlendirme URI'si ve APK'nın üretim imzalı sürümü için uri'yi yeniden yönlendirmeniz gerektiği anlamına gelir.

MSAL'ın yaptığına benzer şekilde bu imzayı da kendiniz hesaplayabilirsiniz: 

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

Ayrıca aşağıdaki komutları ile anahtar aracını kullanarak paketiniz için imza alma seçeneğiniz vardır:

Windows için:`keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android`

Mac için:`keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64`

## <a name="next-steps"></a>Sonraki adımlar

MSAL.NET [ile Evrensel Windows Platform'u kullanmada dikkat](msal-net-uwp-considerations.md)edilmesi gerekenler hakkında bilgi edinin.
