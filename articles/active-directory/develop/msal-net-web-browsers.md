---
title: Web tarayıcılarını kullanma (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET için Microsoft Kimlik Doğrulama Kitaplığı ile Xamarin Android kullanırken belirli hususlar hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: ed1f47ae99f6346a932d0fe94be7586dc25a672f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262743"
---
# <a name="using-web-browsers-msalnet"></a>Web tarayıcılarını kullanma (MSAL.NET)

Etkileşimli kimlik doğrulaması için web tarayıcıları gereklidir. Varsayılan olarak, MSAL.NET Xamarin.iOS ve Xamarin.Android [sistem web tarayıcısını](#system-web-browser-on-xamarinios-xamarinandroid) destekler. Ancak, [Xamarin.iOS](#choosing-between-embedded-web-browser-or-system-browser-on-xamarinios) ve [Xamarin.Android](#detecting-the-presence-of-custom-tabs-on-xamarinandroid) uygulamalarında gereksinimlerinize (UX, tek oturum açma (SSO) güvenlik gereksinimi) bağlı olarak [Gömülü Web tarayıcısını da etkinleştirebilirsiniz.](#enable-embedded-webviews-on-ios-and-android) Ayrıca, Chrome'un varlığına veya [Android'de](#detecting-the-presence-of-custom-tabs-on-xamarinandroid) Chrome özel sekmelerini destekleyen bir tarayıcıya bağlı olarak dinamik olarak hangi web tarayıcısını kullanacağınızı seçebilirsiniz. MSAL.NET sadece .NET Core masaüstü uygulamalarında sistem tarayıcısını destekler.

## <a name="web-browsers-in-msalnet"></a>MSAL.NET'deki web tarayıcıları

### <a name="interaction-happens-in-a-web-browser"></a>Etkileşim bir Web tarayıcısında gerçekleşir

Etkileşimli olarak bir belirteç edinirken, iletişim kutusunun içeriğinin kitaplık tarafından değil, STS (Güvenlik Belirteci Hizmeti) tarafından sağlandığını anlamak önemlidir. Kimlik doğrulama bitiş noktası, etkileşimi kontrol eden bazı HTML ve JavaScript'i geri gönderir ve bu da bir web tarayıcısında veya web denetiminde işlenir. STS'nin HTML etkileşimini ele almasına izin vermenin birçok avantajı vardır:

- Parola (bir tane yazdıysa) uygulama veya kimlik doğrulama kitaplığı tarafından hiçbir zaman depolanmaz.
- Diğer kimlik sağlayıcılarına yeniden yönlendirmeler sağlar (örneğin, iş okulu hesabıyla veya MSAL'da kişisel bir hesapla veya Azure AD B2C'li bir sosyal hesapla giriş yapın).
- Örneğin, kullanıcının kimlik doğrulama aşamasında (Windows Hello pini girilmesi veya telefonlarında çağrılması veya telefonlarındaki bir kimlik doğrulama uygulamasında) birden çok faktörlü kimlik doğrulaması (MFA) yapmasını sağlayarak STS Koşullu Erişimi denetlemesini sağlar. Gerekli çok faktörlü kimlik doğrulamanın henüz ayarlanmadığı durumlarda, kullanıcı aynı iletişim kutusunda tam zamanında ayarlayabilir.  Kullanıcı cep telefonu numarasını girer ve bir kimlik doğrulama uygulaması yüklemek ve hesaplarını eklemek için bir QR etiketit için yönlendirilir. Bu sunucu odaklı etkileşim büyük bir deneyim!
- Parolanın süresi dolduğunda kullanıcının aynı iletişim kutusunda parolasını değiştirmesine izin verir (eski parola ve yeni parola için ek alanlar sağlar).
- Azure AD kiracı yöneticisi / uygulama sahibi tarafından kontrol edilen kiracının veya uygulamanın (görüntülerin) markalanmasını sağlar.
- Kullanıcıların, uygulamanın kimlik doğrulamadan hemen sonra kendi adlarındaki kaynaklara/ kapsamlara erişmesini izin vermelerini sağlar.

### <a name="embedded-vs-system-web-ui"></a>Gömülü vs Sistem Web UI

MSAL.NET çok çerçeveli bir kitaplıktır ve bir tarayıcıyı ui denetiminde barındıracak çerçeveye özgü bir koda sahiptir (örneğin, .Net Classic'te WinForms kullanır, Xamarin'de yerel mobil denetimler vb.) Bu denetime `embedded` web arabirimi denir. Alternatif olarak, MSAL.NET da sistem işletim sistemi tarayıcısı başlatmak mümkün.

Genellikle, platform varsayılanını kullanmanız önerilir ve bu genellikle sistem tarayıcısıdır. Sistem tarayıcısı, daha önce oturum açmış olan kullanıcıları hatırlamakta daha iyidir. Bu davranışı değiştirmeniz gerekiyorsa,`WithUseEmbeddedWebView(bool)`

### <a name="at-a-glance"></a>Bir bakışta

| Framework        | Katıştırılmış | Sistem | Varsayılan |
| ------------- |-------------| -----| ----- |
| .NET Klasik     | Evet | Evet^ | Katıştırılmış |
| .NET Core     | Hayır | Evet^ | Sistem |
| .NET Standard | Hayır | Evet^ | Sistem |
| UWP | Evet | Hayır | Katıştırılmış |
| Xamarin.Android | Evet | Evet  | Sistem |
| Xamarin.iOS | Evet | Evet  | Sistem |
| Xamarin.Mac| Evet | Hayır | Katıştırılmış |

^ Gerektirirhttp://localhost" " URI yönlendirme

## <a name="system-web-browser-on-xamarinios-xamarinandroid"></a>Xamarin.iOS, Xamarin.Android sistem web tarayıcısı

Varsayılan olarak, MSAL.NET Xamarin.iOS, Xamarin.Android ve .NET Core sistem web tarayıcısını destekler. UI sağlayan tüm platformlar için (yani .NET Core değil), kitaplık tarafından bir web tarayıcıdenetimi katıştırma iletişim ilerler. MSAL.NET ayrıca UWP platformu için .NET Desktop ve WAB için gömülü bir web görünümü de kullanır. Ancak, varsayılan olarak Xamarin iOS ve Xamarin Android uygulamaları için **sistem web tarayıcısı** yararlanır. iOS'ta, İşletim Sistemi'nin (iOS12, iOS11 ve daha önceki) sürümüne bağlı olarak kullanılacak web görünümünü bile seçer.

Sistem tarayıcısı kullanarak diğer uygulamalar ve web uygulamaları ile bir broker (Şirket portalı / Authenticator) gerek kalmadan SSO devlet paylaşımı önemli bir avantaja sahiptir. Sistem tarayıcısı, varsayılan olarak, Xamarin iOS ve Xamarin Android platformları için MSAL.NET kullanıldı çünkü, bu platformlarda, sistem web tarayıcısı tüm ekranı kaplar ve kullanıcı deneyimi daha iyidir. Sistem web görünümü bir iletişim kutusundan ayırt edilemez. Ancak iOS'ta, kullanıcının tarayıcının uygulamayı geri çağırması için izin vermesi gerekebilir, bu da rahatsız edici olabilir.

## <a name="system-browser-experience-on-net-core"></a>.NET Core'da sistem tarayıcısı deneyimi

.NET Core'da MSAL.NET ayrı bir işlem olarak sistem tarayıcısını başlatacaktır. MSAL.NET bu tarayıcı üzerinde denetimi yoktur, ancak kullanıcı kimlik doğrulaması bittikten sonra, web sayfası MSAL.NET Uri'yi durdurabilecek şekilde yönlendirilir.

.NET Classic için yazılmış uygulamaları da bu tarayıcıyı kullanmak üzere yapılandırabilirsiniz.

```csharp
await pca.AcquireTokenInteractive(s_scopes)
         .WithUseEmbeddedWebView(false)
```

MSAL.NET kullanıcının uzağa gitup gitmeyeceğini veya tarayıcıyı kapatıp kapatmadiğini algılayamaz. Bu tekniği kullanan uygulamaların bir zaman ayarı `CancellationToken`tanımlaması (üzerinden) teşvik edilir. Kullanıcıdan parolayı değiştirmesi veya çok faktörlü kimlik doğrulaması gerçekleştirmesi istendiği durumları dikkate almak için en az birkaç dakikalık bir zaman dilimi öneririz.

### <a name="how-to-use-the-default-os-browser"></a>Varsayılan İşletim Sistemi Tarayıcısı nasıl kullanılır?

MSAL.NET kullanıcı kimlik `http://localhost:port` doğrulaması yapıldığında AAD'nin gönderdiği kodu dinlemesi ve engellemesi gerekir (Ayrıntılar için [Yetkilendirme koduna](v2-oauth2-auth-code-flow.md) bakın)

Sistem tarayıcısını etkinleştirmek için:

1. Uygulama kaydı sırasında, `http://localhost` yeniden yönlendirme uriolarak yapılandırın (şu anda B2C tarafından desteklenmez)
2. PublicClientApplication'ınızı oluşturunuzu yaptığınızda, şu yönlendirmeyi belirtin:

```csharp
IPublicClientApplication pca = PublicClientApplicationBuilder
                            .Create("<CLIENT_ID>")
                             // or use a known port if you wish "http://localhost:1234"
                            .WithRedirectUri("http://localhost")  
                            .Build();
```

> [!Note]
> Yapılandırırsanız, `http://localhost`dahili olarak MSAL.NET rasgele bir açık bağlantı noktası bulur ve kullanır.

### <a name="linux-and-mac"></a>Linux ve MAC

Linux'ta MSAL.NET, xdg açık aracını kullanarak varsayılan işletim sistemi tarayıcısını açar. Sorun gidermek için, örneğin bir terminalden aracı çalıştırın,`xdg-open "https://www.bing.com"`  
Mac'te, tarayıcı çağırılarak açılır`open <url>`

### <a name="customizing-the-experience"></a>Deneyimi özelleştirme

> [!NOTE]
> Özelleştirme 4.1.0 veya daha sonra MSAL.NET kullanılabilir.

MSAL.NET bir belirteç aldığında veya hata durumunda bir HTTP iletisi ile yanıt verebiliyor. Bir HTML iletisi görüntüleyebilir veya seçtiğiniz bir url'ye yönlendirebilirsiniz:

```csharp
var options = new SystemWebViewOptions() 
{
    HtmlMessageError = "<p> An error occured: {0}. Details {1}</p>",
    BrowserRedirectSuccess = new Uri("https://www.microsoft.com");
}

await pca.AcquireTokenInteractive(s_scopes)
         .WithUseEmbeddedWebView(false)
         .WithSystemWebViewOptions(options)
         .ExecuteAsync();
```

### <a name="opening-a-specific-browser-experimental"></a>Belirli bir tarayıcıyı açma (Deneysel)

tarayıcıyı açma MSAL.NET şekilde özelleştirebilirsiniz. Örneğin varsayılan tarayıcı ne olursa olsun kullanmak yerine, belirli bir tarayıcıyı açmaya zorlayabilirsiniz:

```csharp
var options = new SystemWebViewOptions() 
{
    OpenBrowserAsync = SystemWebViewOptions.OpenWithEdgeBrowserAsync
}
```

### <a name="uwp-doesnt-use-the-system-webview"></a>UWP Sistem Web görünümü kullanmaz

Ancak masaüstü uygulamalariçin, bir System Webview başlatılması, kullanıcı tarayıcıyı gördüğünden, diğer sekmelerin zaten açılmış olabileceği bir alt kullanıcı deneyimine yol açar. Kimlik doğrulama gerçekleştiğinde, kullanıcılardan bu pencereyi kapatmalarını isteyen bir sayfa alır. Kullanıcı dikkat etmezse, tüm işlemi (kimlik doğrulamayla ilgisi olmayan diğer sekmeler dahil) kapatabilir. Masaüstünde sistem tarayıcısı yararlanarak da yerel bağlantı noktaları açılması ve uygulama için gelişmiş izinler gerektirebilir onları dinleme gerektirir. Geliştirici, kullanıcı veya yönetici olarak bu gereksinim konusunda isteksiz olabilirsiniz.

## <a name="enable-embedded-webviews-on-ios-and-android"></a>iOS ve Android'de gömülü web görüntülemelerini etkinleştirme

Xamarin.iOS ve Xamarin.Android uygulamalarında gömülü web görünümlerini de etkinleştirebilirsiniz. 2.0.0 önizlemeMSAL.NET ile başlayan MSAL.NET, **gömülü** web görünümü seçeneğini de destekler. ADAL.NET için, gömülü web görünümü desteklenen tek seçenektir.

Xamarin'i hedefleyen MSAL.NET kullanan bir geliştirici olarak, gömülü web görünümlerini veya sistem tarayıcılarını kullanmayı seçebilirsiniz. Bu, hedeflemek istediğiniz kullanıcı deneyimine ve güvenlik endişelerine bağlı olarak sizin seçiminizdir.

Şu anda, MSAL.NET henüz Android ve iOS brokerdestek lemiyor. Bu nedenle, tek oturum açma (SSO) sağlamanız gerekiyorsa, sistem tarayıcısı yine de daha iyi bir seçenek olabilir. Gömülü web tarayıcısı ile destekleyici broker biriktirme listesi MSAL.NET.

### <a name="differences-between-embedded-webview-and-system-browser"></a>Gömülü web görünümü ve sistem tarayıcısı arasındaki farklar
MSAL.NET gömülü web görünümü ve sistem tarayıcısı arasında bazı görsel farklılıklar vardır.

**Gömülü Web Görünümü'ni kullanarak MSAL.NET ile etkileşimli oturum açma:**

![katıştırılmış](media/msal-net-web-browsers/embedded-webview.png)

**Sistem Tarayıcısı'nı kullanarak MSAL.NET ile etkileşimli oturum açma:**

![Sistem tarayıcısı](media/msal-net-web-browsers/system-browser.png)

### <a name="developer-options"></a>Geliştirici Seçenekleri

MSAL.NET kullanan bir geliştirici olarak, STS'den etkileşimli iletişim kutusunu görüntülemek için çeşitli seçenekleriniz vardır:

- **Sistem tarayıcısı.** Sistem tarayıcısı kitaplıkta varsayılan olarak ayarlanır. Android kullanıyorsanız, kimlik doğrulama için hangi tarayıcıların desteklendiğine ilişkin belirli bilgiler için [sistem tarayıcılarını](msal-net-system-browser-android-considerations.md) okuyun. Android'de sistem tarayıcısını kullanırken, cihazın Chrome özel sekmelerini destekleyen bir tarayıcıya sahip olduğunu öneririz.  Aksi takdirde, kimlik doğrulama başarısız olabilir.
- **Gömülü web görünümü.** Yalnızca MSAL.NET gömülü web görünümünü `AcquireTokenInteractively` kullanmak için `WithUseEmbeddedWebView()` parametre oluşturucusu bir yöntem içerir.

    iOS

    ```csharp
    AuthenticationResult authResult;
    authResult = app.AcquireTokenInteractively(scopes)
                    .WithUseEmbeddedWebView(useEmbeddedWebview)
                    .ExecuteAsync();
    ```

    Android:

    ```csharp
    authResult = app.AcquireTokenInteractively(scopes)
                .WithParentActivityOrWindow(activity)
                .WithUseEmbeddedWebView(useEmbeddedWebview)
                .ExecuteAsync();
    ```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinios"></a>Xamarin.iOS'ta gömülü web tarayıcısı veya sistem tarayıcısı arasında seçim yapmak

iOS uygulamanızda, `AppDelegate.cs` 'nin ' `ParentWindow` ini başolarak `null` iOS'ta kullanılmaz

```csharp
App.ParentWindow = null; // no UI parent on iOS
```

#### <a name="choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid"></a>Xamarin.Android'de gömülü web tarayıcısı veya sistem tarayıcısı arasında seçim yapmak

Android uygulamanızda, `MainActivity.cs` kimlik doğrulama sonucunun geri dönebilmeleri için üst etkinliği ayarlayabilirsiniz:

```csharp
 App.ParentWindow = this;
```

Sonra: `MainPage.xaml.cs`

```csharp
authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(true)
                      .ExecuteAsync();
```

#### <a name="detecting-the-presence-of-custom-tabs-on-xamarinandroid"></a>Xamarin.Android'de özel sekmelerin varlığını algılama

Eğer tarayıcıda çalışan uygulamalar ile SSO etkinleştirmek için sistem web tarayıcısı kullanmak istiyorsanız, ancak özel sekme desteği ile bir tarayıcı olmayan Android `IsSystemWebViewAvailable()` cihazlar `IPublicClientApplication`için kullanıcı deneyimi hakkında endişe duyuyorsanız, yöntemi arayarak karar verme seçeneğiniz var. PackageManager `true` özel sekmeleri algılarsa ve `false` aygıtta algılanmamışsa bu yöntem döndürür.

Bu yöntemle döndürülen değere ve gereksinimlerinize bağlı olarak bir karar verebilirsiniz:

- Kullanıcıya özel bir hata iletisi döndürebilirsiniz. Örneğin: "Kimlik doğrulamasına devam etmek için lütfen Chrome'u yükleyin" -OR-
- Katıştılmış web görünümü seçeneğine geri dönebilir ve ui'yi gömülü bir web görünümü olarak başlatabilirsiniz.

Aşağıdaki kod, gömülü web görünümü seçeneğini gösterir:

```csharp
bool useSystemBrowser = app.IsSystemWebviewAvailable();

authResult = await App.PCA.AcquireTokenInteractive(App.Scopes)
                      .WithParentActivityOrWindow(App.ParentWindow)
                      .WithUseEmbeddedWebView(!useSystemBrowser)
                      .ExecuteAsync();
```

#### <a name="net-core-doesnt-support-interactive-authentication-with-an-embedded-browser"></a>.NET Core, gömülü bir tarayıcıyla etkileşimli kimlik doğrulamasını desteklemez

.NET Core için, jeton satın alma etkileşimli olarak sadece sistem web tarayıcısı üzerinden kullanılabilir, gömülü web görünümleri ile değil. Nitekim, .NET Core henüz UI sağlamaz.
Sistem web tarayıcısı ile tarama deneyimini özelleştirmek istiyorsanız, [IWithCustomUI](scenario-desktop-acquire-token.md#withcustomwebui) arabirimini uygulayabilir ve hatta kendi tarayıcınızı bile sağlayabilirsiniz.
