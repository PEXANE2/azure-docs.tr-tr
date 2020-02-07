---
title: Xamarin Android konuları (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin Android kullanırken belirli hususlar hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 54df91d38541fbe17a28c9ae083ae0e7d0c9d88d
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77063671"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>MSAL.NET ile Xamarin Android 'e özgü konular
Bu makalede, .NET için Microsoft kimlik doğrulama kitaplığı (MSAL.NET) ile Xamarin Android kullanılırken belirli noktalar ele alınmaktadır.

## <a name="set-the-parent-activity"></a>Üst etkinliği ayarla

Xamarin. Android üzerinde, etkileşim kurulduktan sonra belirtecin geri dönmesi için üst etkinliği ayarlamanız gerekir.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```
Bunu, PublicClientApplication düzeyinde (MSAL 4.2 + içinde) bir geri çağırma yoluyla da ayarlayabilirsiniz.

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => parentActivity)
  .Build();
```

Bunun bir önerisi [burada](https://github.com/jamesmontemagno/CurrentActivityPlugin)CurrentActivityPlugin kullanmaktır.  Ardından, PublicClientApplication Oluşturucu kodunuz şöyle görünür:

```csharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```


## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Kimlik doğrulama akışının etkileşimli kısmı sona erdikten sonra denetimin MSAL 'e geri gitmesini sağlama
Android 'de, `Activity` `OnActivityResult` yöntemini geçersiz kılmanız ve AuthenticationContinuationHelper MSAL sınıfının SetAuthenticationContinuationEventArgs yöntemini çağırmanız gerekir.

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
Bu satır, kimlik doğrulama akışının etkileşimli kısmı sona erdikten sonra denetimin MSAL 'e geri gelmesini sağlar.

## <a name="update-the-android-manifest"></a>Android bildirimini güncelleştirme
`AndroidManifest.xml` aşağıdaki değerleri içermelidir:
```xml
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msauth"
              android:host="Enter_the_Package_Name"
              android:path="/Enter_the_Signature_Hash"/>
         </intent-filter>
</activity>
```
`android:host=` değer için Azure portal kaydettiğiniz paket adını değiştirin. `android:path=` değer için Azure portal kaydettiğiniz anahtar karmasını değiştirin. Imza karması URL kodlamalı **olmamalıdır.** Imza Karmalarınızın başlangıcında önde gelen bir `/` olduğundan emin olun.

Ya da [etkinliği kodda oluşturabilir](https://docs.microsoft.com/xamarin/android/platform/android-manifest#the-basics) ve `AndroidManifest.xml`el ile düzenleyemezsiniz. Bunun için, `Activity` ve `IntentFilter` özniteliğine sahip bir sınıf oluşturmanız gerekir. Yukarıdaki XML 'in aynı değerlerini temsil eden bir sınıf şöyle olacaktır:

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

### <a name="xamarinforms-43x-manifest"></a>XamarinForms 4.3. X bildirimi

XamarinForms 4.3. x tarafından oluşturulan kod, `AndroidManifest.xml``com.companyname.{appName}` `package` özniteliğini ayarlar. `DataScheme` `msal{client_id}`olarak kullanırsanız, değeri `MainActivity.cs` ad alanıyla aynı olacak şekilde değiştirmek isteyebilirsiniz.

## <a name="use-the-embedded-web-view-optional"></a>Katıştırılmış Web görünümünü kullanın (isteğe bağlı)

Varsayılan olarak MSAL.NET, Web uygulamaları ve diğer uygulamalarla SSO almanızı sağlayan sistem Web tarayıcısını kullanır. Nadir bazı durumlarda, katıştırılmış Web görünümünü kullanmak istediğinizi belirtmek isteyebilirsiniz. Daha fazla bilgi için bkz. [msal.net bir Web tarayıcısı](msal-net-web-browsers.md) ve [Android Sistem tarayıcısı](msal-net-system-browser-android-considerations.md)kullanır.

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>Sorun giderme
Yeni bir Xamarin. Forms uygulaması oluşturur ve MSAL.Net NuGet paketine bir başvuru eklerseniz, bu işlem yalnızca çalışır.
Ancak, var olan bir Xamarin. Forms uygulamasını MSAL.NET Preview 1.1.2 veya sonraki bir sürüme yükseltmek istiyorsanız, derleme sorunlarıyla karşılaşabilirsiniz.

Bu sorunları gidermek için şunları yapmalısınız:
- Mevcut MSAL.NET NuGet paketini MSAL.NET Preview 1.1.2 veya sonraki bir sürüme güncelleştirin
- Xamarin. Forms 'un 2.5.0.122203 sürümüne otomatik olarak güncelleştirildiğini denetleyin (değilse, bu sürüme güncelleştirin)
- Xamarin. Android. support. v4 'nin 25.4.0.2 sürümüne otomatik olarak güncelleştirildiğini denetleyin (değilse, bu sürüme güncelleştirin)
- Tüm Xamarin. Android. support paketleri 25.4.0.2 sürümünü hedefliyor olmalıdır
- Temizle/yeniden oluştur
- Visual Studio 'da maksimum paralel proje derlemelerini 1 ' e ayarlamayı deneyin (Seçenekler-> Projeler ve Çözümler-> derleme ve çalıştırma-> en fazla paralel proje derlemesi sayısı)
- Alternatif olarak, komut satırından oluşturuyorsanız, kullanıyorsanız/m komutunu komutınızdan kaldırmayı deneyin.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Hata: ' AuthenticationContinuationHelper ' adı geçerli bağlamda yok

Bu, Visual Studio 'Nun Android. csproj * dosyasını doğru bir şekilde güncelleştirmediği için büyük olasılıkla oluşur. Bazen **\<HintPath >** FilePath, **monoandroid90**yerine netstandard13 içerir.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki örnek readme.md dosyasının [Android 'e özgü hususlar](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) paragrafında daha fazla ayrıntı ve örnek verilmiştir:

| Örnek | Platform | Açıklama |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Basit bir Xamarin Forms uygulaması, AADD v 2.0 uç noktası aracılığıyla MSA ve Azure AD kimlik doğrulaması için MSAL kullanma ve elde edilen belirteçle Microsoft Graph erişme. <br>![Topoloji](media/msal-net-xamarin-android-considerations/topology.png) |
