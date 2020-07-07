---
title: Xamarin Android konuları (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin Android kullanma hakkında konular hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: bb5950360734bc46923ef18424e3ad1ce275ad7a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82652682"
---
# <a name="considerations-for-using-xamarin-android-with-msalnet"></a>MSAL.NET ile Xamarin Android kullanma konuları
Bu makalede, .NET için Microsoft kimlik doğrulama kitaplığı ile Xamarin Android kullandığınızda göz önünde bulundurmanız gerekenler açıklanmaktadır (MSAL.NET).

## <a name="set-the-parent-activity"></a>Üst etkinliği ayarla

Xamarin Android 'de, belirtecin etkileşimden sonra döndürdüğü şekilde üst etkinliği ayarlayın. Aşağıda bir kod örneği verilmiştir:

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```

MSAL 4,2 ve üzeri sürümlerde, bu işlevselliği düzeyinde de ayarlayabilirsiniz `PublicClientApplication` . Bunu yapmak için bir geri çağırma kullanın:

```csharp
// Requires MSAL.NET 4.2 or later
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

[CurrentActivityPlugin](https://github.com/jamesmontemagno/CurrentActivityPlugin)kullanıyorsanız, `PublicClientApplication` Oluşturucu kodunuz aşağıdaki örneğe benzer şekilde görünür.

```csharp
// Requires MSAL.NET 4.2 or later
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

## <a name="ensure-that-control-returns-to-msal"></a>Denetimin MSAL 'e döndürdüğünden emin olun 
Kimlik doğrulama akışının etkileşimli kısmı sona erdiğinde, denetimin MSAL 'e geri gitmesini sağlayın. Android 'de, yöntemini geçersiz kılın `OnActivityResult` `Activity` . Ardından `SetAuthenticationContinuationEventArgs` `AuthenticationContinuationHelper` msal sınıfının yöntemini çağırın. 

İşte bir örnek:

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```

Bu satır, denetim, kimlik doğrulama akışının etkileşimli bölümünün sonundaki denetimin MSAL 'e dönüşmesini sağlar.

## <a name="update-the-android-manifest"></a>Android bildirimini güncelleştirme
*AndroidManifest.xml* dosyası aşağıdaki değerleri içermelidir:

<!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
```
  <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
     <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Enter_the_Package_Name"
                android:path="/Enter_the_Signature_Hash" />
     </intent-filter>
 </activity>
```

Değer için Azure portal kaydettiğiniz paket adını değiştirin `android:host=` . Değer için Azure portal kaydettiğiniz anahtar karmasını değiştirin `android:path=` . İmza karması URL kodlamalı *olmamalıdır.* İmza karmalarınızın başlangıcında önde gelen eğik çizgi ( `/` ) göründüğünden emin olun.

Alternatif olarak, *AndroidManifest.xml*el ile düzenlemeniz yerine [etkinlik kodda oluşturun](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics) . Etkinliği kodda oluşturmak için önce özniteliğini ve özniteliğini içeren bir sınıf oluşturun `Activity` `IntentFilter` . 

XML dosyasının değerlerini temsil eden bir sınıf örneği aşağıda verilmiştir:

```csharp
  [Activity]
  [IntentFilter(new[] { Intent.ActionView },
        Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
        DataHost = "auth",
        DataScheme = "msal{client_id}")]
  public class MsalActivity : BrowserTabActivity
  {
  }
```

### <a name="xamarinforms-43x-manifest"></a>Xamarin. Forms 4.3. X bildirimi

Xamarin. Forms 4.3. x,AndroidManifest.xmlözniteliği olarak ayarlayan kodu oluşturur `package` `com.companyname.{appName}` . * * `DataScheme`Olarak kullanıyorsanız `msal{client_id}` , değeri ad alanı değeriyle eşleşecek şekilde değiştirmek isteyebilirsiniz `MainActivity.cs` .

## <a name="use-the-embedded-web-view-optional"></a>Katıştırılmış Web görünümünü kullanın (isteğe bağlı)

Varsayılan olarak, MSAL.NET, sistem Web tarayıcısını kullanır. Bu tarayıcı, Web uygulamalarını ve diğer uygulamaları kullanarak çoklu oturum açmayı (SSO) almanızı sağlar. Nadir bazı durumlarda sisteminizin gömülü bir Web görünümü kullanmasını isteyebilirsiniz. 

Bu kod örneği, gömülü bir Web görünümünün nasıl ayarlanacağını gösterir:

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

Daha fazla bilgi için bkz. MSAL.NET ve [Xamarin Android sistem tarayıcı konuları](msal-net-system-browser-android-considerations.md) [Için Web tarayıcıları kullanma](msal-net-web-browsers.md) .


## <a name="troubleshoot"></a>Sorun giderme
Yeni bir Xamarin. Forms uygulaması oluşturabilir ve MSAL.NET NuGet paketine bir başvuru ekleyebilirsiniz.
Ancak mevcut bir Xamarin. Forms uygulamasını MSAL.NET Preview 1.1.2 veya sonraki bir sürüme yükseltirseniz, derleme sorunlarıyla karşılaşabilirsiniz.

Derleme sorunlarını gidermek için:

- Mevcut MSAL.NET NuGet paketini MSAL.NET Preview 1.1.2 veya sonraki bir sürüme güncelleştirin.
- Xamarin. Forms 'un 2.5.0.122203 sürümüne otomatik olarak güncelleştirildiğinden emin olun. Gerekirse, Xamarin. Forms 'u bu sürüme güncelleştirin.
- Xamarin. Android. support. v4 'nin 25.4.0.2 sürümüne otomatik olarak güncelleştirilmesini denetleyin. Gerekirse, 25.4.0.2 sürümüne güncelleştirin.
- Tüm Xamarin. Android. support paketlerinin 25.4.0.2 sürümünü hedef aldığından emin olun.
- Uygulamayı temizleyin veya yeniden derleyin.
- Visual Studio 'da, en fazla paralel proje derlemesi sayısını 1 olarak ayarlamayı deneyin. Bunu yapmak için, **Seçenekler**  >  **Projeler ve çözümler**  >  **derleme ve**  >  **en fazla paralel proje derlemesi sayısını**Çalıştır ' ı seçin.
- Komut satırından derleme yapıyorsanız ve komutunuz kullanıyorsa `/m` , bu öğeyi komuttan kaldırmayı deneyin.

### <a name="error-the-name-authenticationcontinuationhelper-doesnt-exist-in-the-current-context"></a>Hata: AuthenticationContinuationHelper adı geçerli bağlamda yok

Bir hata `AuthenticationContinuationHelper` geçerli bağlamda mevcut olmadığını gösteriyorsa, Visual Studio Android. csproj * dosyasını yanlış bir şekilde güncelleştirmiş olabilir. Bazen *\<HintPath>* dosya yolu *monoandroid90*yerine *netstandard13* içerir.

Bu örnek, doğru bir dosya yolu içerir:

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Microsoft Identity platform kullanan bir Xamarin mobil uygulaması](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations)örneğine bakın. Aşağıdaki tabloda README dosyasındaki ilgili bilgiler özetlenmektedir.

| Örnek | Platform | Description |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin. iOS, Android, UWP | Azure AD 2,0 uç noktası aracılığıyla Microsoft kişisel hesaplarının ve Azure AD kimlik doğrulaması için MSAL kullanmayı gösteren basit bir Xamarin. Forms uygulaması. Uygulama ayrıca Microsoft Graph nasıl erişebileceğini ve elde edilen belirtecin nasıl gösterileceğini gösterir. <br>![Topoloji](media/msal-net-xamarin-android-considerations/topology.png) |
