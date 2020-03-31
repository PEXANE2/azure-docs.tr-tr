---
title: Xamarin Android hususlar (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile Xamarin Android'i kullanmak için dikkat edilmesi gereken hususlar hakkında bilgi edinin.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81b55253d757f641979c6f72001803d7d38d9af3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132508"
---
# <a name="considerations-for-using-xamarin-android-with-msalnet"></a>xamarin Android'i MSAL.NET ile kullanmak için dikkat edilmesi gerekenler
Bu makalede, .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı ile Xamarin Android kullandığınızda neleri göz önünde bulundurmanız gerektiğini anlatılmaktadır.

## <a name="set-the-parent-activity"></a>Üst etkinliği ayarlama

Xamarin Android'de, ana etkinliği etkileşimden sonra belirteç lerin geri dönmesi için ayarlayın. Aşağıda bir kod örneği verilmiştir:

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```

MSAL 4.2 ve sonraki durumlarda, bu işlevselliği `PublicClientApplication`. Bunu yapmak için geri arama kullanın:

```csharp
// Requires MSAL.NET 4.2 or later
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

[CurrentActivityPlugin](https://github.com/jamesmontemagno/CurrentActivityPlugin)kullanıyorsanız, oluşturucu kodunuz `PublicClientApplication` aşağıdaki örneğe benzer.

```csharp
// Requires MSAL.NET 4.2 or later
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

## <a name="ensure-that-control-returns-to-msal"></a>Kontrolün MSAL'a geri dönmesini sağlayın 
Kimlik doğrulama akışının etkileşimli bölümü sona erdiğinde, denetimin MSAL'a geri gittiğinden emin olun. Android'de, `OnActivityResult` `Activity`. Sonra MSAL sınıfının `SetAuthenticationContinuationEventArgs` yöntemini `AuthenticationContinuationHelper` arayın. 

Bir örneği aşağıda verilmiştir:

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

Bu satır, kimlik doğrulama akışının etkileşimli bölümünün sonunda denetimin MSAL'a dönmesini sağlar.

## <a name="update-the-android-manifest"></a>Android manifestosunun güncelleştirin
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

Azure portalına kaydettiğiniz paket adını değer `android:host=` için değiştirin. Azure portalında kaydettiğiniz anahtar karmasını değer `android:path=` için değiştirin. İmza karma URL kodlanmış *olmamalıdır.* Önde gelen bir kesme`/`( ) imza karmanızın başında göründüğünden emin olun.

Alternatif olarak, *AndroidManifest.xml'i*el ile düzenlemek yerine [kodda etkinlik oluşturun.](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics) Etkinliği kodda oluşturmak için, önce `Activity` öznitelik ve `IntentFilter` özniteliği içeren bir sınıf oluşturun. 

Aşağıda, XML dosyasının değerlerini temsil eden bir sınıf örneği verilmiştir:

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

### <a name="xamarinforms-43x-manifest"></a>Xamarin.Forms 4.3.X manifestosu

Xamarin.Forms 4.3.x `package` *AndroidManifest.xml*özniteliği `com.companyname.{appName}` ayarlar kod oluşturur. `DataScheme` ', `msal{client_id}` `MainActivity.cs` ad alanının değeriyle eşleşecek şekilde değeri değiştirmek isteyebilirsiniz.

## <a name="use-the-embedded-web-view-optional"></a>Katıştırılmış web görünümünü kullanma (isteğe bağlı)

Varsayılan olarak, MSAL.NET sistem web tarayıcısını kullanır. Bu tarayıcı, web uygulamalarını ve diğer uygulamaları kullanarak tek oturum açma (SSO) almanızı sağlar. Bazı nadir durumlarda, sisteminizin katışılmış bir web görünümü kullanmasını isteyebilirsiniz. 

Bu kod örneği, gömülü bir web görünümünün nasıl ayarlanır olduğunu gösterir:

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

Daha fazla bilgi için MSAL.NET ve [Xamarin Android sistem tarayıcı ları için](msal-net-system-browser-android-considerations.md)web [tarayıcılarını kullanın'](msal-net-web-browsers.md) a bakın.


## <a name="troubleshoot"></a>Sorun giderme
Yeni bir Xamarin.Forms uygulaması oluşturabilir ve MSAL.NET NuGet paketine bir referans ekleyebilirsiniz.
Ancak, 1.1.2 veya daha sonra önizlemeMSAL.NET için varolan bir Xamarin.Forms uygulamasını yükseltiyorsanız, yapı sorunlarınız olabilir.

Yapı sorunlarını gidermek için:

- Mevcut MSAL.NET NuGet paketini önizleme 1.1.2 veya daha sonra MSAL.NET güncelleştirmek için güncelleştirin.
- Xamarin.Forms'un 2.5.0.122203 sürümüne otomatik olarak güncelleştirin. Gerekirse Xamarin.Forms'u bu sürüme güncelleyin.
- Xamarin.Android.Support.v4'ün 25.4.0.2 sürümüne otomatik olarak güncelleştirin. Gerekirse, sürüm 25.4.0.2'ye güncelleştirin.
- Tüm Xamarin.Android.Support paketleri hedef sürümü 25.4.0.2 emin olun.
- Uygulamayı temizleyin veya yeniden oluşturun.
- Visual Studio'da, en fazla paralel proje oluşturma sayısını 1 olarak ayarlamayı deneyin. Bunu yapmak için **Seçenekler** > **Projeleri ve Çözümleri'ni** > seçin Maksimum sayıda paralel proje**oluşturur.** > **Maximum number of parallel projects builds**
- Komut satırından bina oluşturuyorsanız ve komutunuz kullanıyorsa, `/m`bu öğeyi komuttan kaldırmayı deneyin.

### <a name="error-the-name-authenticationcontinuationhelper-doesnt-exist-in-the-current-context"></a>Hata: Kimlik DoğrulamaContinuationHelper adı geçerli bağlamda yok

Geçerli bağlamda bir `AuthenticationContinuationHelper` hata olmadığını gösteriyorsa, Visual Studio Android.csproj* dosyasını yanlış güncelleştirmiş olabilir. Bazen * \<İpucuPath>* dosya yolu yanlış *netstandard13* yerine *monoandroid90*içerir.

Bu örnek, doğru bir dosya yolu içerir:

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için, [Microsoft kimlik platformlarını kullanan bir Xamarin mobil uygulama](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations)örneğine bakın. Aşağıdaki tabloREADME dosyasındaki ilgili bilgileri özetleyin.

| Örnek | Platform | Açıklama |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin.iOS, Android, UWP | Azure AD 2.0 bitiş noktası üzerinden Microsoft kişisel hesaplarını ve Azure AD'yi doğrulamak için MSAL'ın nasıl kullanılacağını gösteren basit bir Xamarin.Forms uygulaması. Uygulama ayrıca Microsoft Graph'a nasıl erişilir ve ortaya çıkan belirteci gösterir. <br>![Topoloji](media/msal-net-xamarin-android-considerations/topology.png) |
