---
title: Hatalar ve özel durumlar (MSAL)
titleSuffix: Microsoft identity platform
description: Hatalar ve özel durumlar, Koşullu Erişim ve MSAL uygulamalarındaki talepler zorluklarıyla nasıl başa çıkacağınızı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/22/2019
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: f78c64fc0ba25dc3310b24e873dbae266ea2f281
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050331"
---
# <a name="handle-msal-exceptions-and-errors"></a>MSAL özel durumlarını ve hatalarını işleme

Bu makalede, ortak oturum açma hatalarını işlemek için farklı hata türleri ve öneriler egenel bir bakış sağlar.

## <a name="msal-error-handling-basics"></a>MSAL hata işleme temelleri

Microsoft Kimlik Doğrulama Kitaplığı'ndaki (MSAL) özel durumlar, uygulama geliştiricilerin son kullanıcılara görüntülenmek için değil, sorun gidermeleri için tasarlanmıştır. Özel durum iletileri yerelleştirilmez.

Özel durumlar ve hataları işlerken, özel durumlar arasında ayrım yapmak için özel durum türü kendisini ve hata kodunu kullanabilirsiniz.  Hata kodları listesi için kimlik [doğrulama ve yetkilendirme hata kodlarına](reference-aadsts-error-codes.md)bakın.

Oturum açma deneyimi sırasında, onaylar, Koşullu Erişim (MFA, Aygıt Yönetimi, Konum tabanlı kısıtlamalar), belirteç verme ve kullanım ve kullanıcı özellikleri yle ilgili hatalarla karşılaşabilirsiniz.

Uygulamanızın hata işleme hakkında daha fazla bilgi için kullandığınız dille eşleşen aşağıdaki bölüme bakın.

## <a name="net"></a>[.NET](#tab/dotnet)

.NET özel durumları işlerken, özel durumlar arasında `ErrorCode` ayrım yapmak için özel durum türü veya üyeyi kullanabilirsiniz. `ErrorCode`değerler [MsalError](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet)türünün sabitleridir.

Ayrıca [MsalClientException](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet), [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet)ve [MsalUIRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet)alanlarına bir göz atabilirsiniz.

[MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) atılırsa, kodun orada listelanıp listelenmeyip listelenmeyini görmek için [Kimlik Doğrulama ve yetkilendirme hata kodlarını](reference-aadsts-error-codes.md) deneyin.

### <a name="common-net-exceptions"></a>Ortak .NET özel durumları

Atılabilecek genel özel durumlar ve bazı olası azaltıcı etkenler şunlardır:  

| Özel durum | Hata kodu | Risk azaltma|
| --- | --- | --- |
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: Kullanıcı veya yönetici uygulamayı '{appId}' adlı '{appName}' kimliğiyle kullanmayı kabul etmemiştir. Bu kullanıcı ve kaynak için etkileşimli bir yetkilendirme isteği gönderin.| Önce kullanıcı onayı almanız gerekir. .NET Core kullanmıyorsanız (Web Ara Birimi yok), (yalnızca bir kez) `AcquireTokeninteractive`arayın. .NET çekirdeğini kullanıyorsanız veya yapmak `AcquireTokenInteractive`istemiyorsanız, kullanıcı onay vermek için bir `https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read`URL'ye gidebilir: . aramak `AcquireTokenInteractive`için :`app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: Kullanıcının çok faktörlü kimlik doğrulaması (MFA) kullanması gerekir.| Hafifletici bir neden yok. MFA kiracınız için yapılandırılırsa ve Azure Etkin Dizini (AAD) bunu uygulamaya karar `AcquireTokenInteractive` verirse, 'veya `AcquireTokenByDeviceCode`.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) |AADSTS90010: Hibe türü */ortak* veya */tüketici* uç noktaları üzerinden desteklenmez. */kuruluşları* veya kiracıya özel bitiş noktasını kullanın. Kullandınız */ortak.*| Azure AD'den gelen iletide açıklandığı gibi, yetkili makamın bir kiracısı veya başka bir *şekilde /kuruluşları*olması gerekir.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002: İstek gövdesi aşağıdaki parametreyi `client_secret or client_assertion`içermelidir: .| Uygulamanız Azure AD'de genel istemci uygulaması olarak kaydedilmediyse, bu özel durum atılabilir. Azure portalında, uygulamanızın bildirimini ve `allowPublicClient` ' `true`i ' ye ayarlayın. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| `unknown_user Message`: Kullanıcı oturum açmış tanımlanamadı| Kitaplık geçerli Windows oturum açmış kullanıcısını sorgulayamadı veya bu kullanıcı AD veya AAD'ye katılmadı (iş yeri birleştirilmiş kullanıcılar desteklenmez). Azaltma 1: UWP'de, uygulamanın aşağıdaki özelliklere sahip olup olmadığını kontrol edin: Kurumsal Kimlik Doğrulama, Özel Ağlar (İstemci ve Sunucu), Kullanıcı Hesabı Bilgileri. Azaltma 2: Kullanıcı adını almak (örneğin,) john@contoso.comve kullanıcı adını `AcquireTokenByIntegratedWindowsAuth` alan formu kullanmak için kendi mantığınızı uygulayın.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Bu yöntem, Active Directory (AD) tarafından maruz kalan bir protokole dayanır. Bir kullanıcı Azure Active Directory'de AD desteği olmadan ("yönetilen" kullanıcı) oluşturulduysa, bu yöntem başarısız olur. AD olarak oluşturulan ve AAD ("federe" kullanıcılar) tarafından desteklenen kullanıcılar bu etkileşimli olmayan kimlik doğrulama yönteminden yararlanabilir. Azaltma: Etkileşimli kimlik doğrulama kullanın.|

### `MsalUiRequiredException`

Arama `AcquireTokenSilent()` yaparken MSAL.NET döndürülen yaygın `MsalError.InvalidGrantError`durum kodlarından biri . Bu durum kodu, uygulamanın kimlik doğrulama kitaplığını yeniden araması, ancak etkileşimli modda (Kamu istemcisi uygulamaları için EdinmeTokenInteractive veya AcquireTokenByDeviceCodeFlow ve Web uygulamalarında bir sorun yapmak) anlamına gelir. Bunun nedeni, kimlik doğrulama belirteci verilmeden önce ek kullanıcı etkileşiminin gerekli olmasıdır.

Çoğu zaman başarısız `AcquireTokenSilent` olduğunda, bunun nedeni belirteç önbelleğinin isteğinizle eşleşen belirteçleri olmamasıdır. Erişim belirteçleri 1 saat `AcquireTokenSilent` içinde sona erer ve yenibir belirteci dayalı yeni bir tane getirmek için çalışacağız (OAuth2 açısından, bu "Token' akışı yenilemek). Bu akış, örneğin kiracı yönetici daha sıkı oturum açma ilkeleri yapılandırırsa, çeşitli nedenlerle de başarısız olabilir. 

Etkileşim, kullanıcının bir eylem de yapmasını amaçlamaktadır. Bu koşullardan bazılarının çözülmesi kolaydır (örneğin, Kullanım Koşullarını tek bir tıklamayla kabul edin) ve bazıları geçerli yapılandırmayla çözülemez (örneğin, söz konusu makinenin belirli bir şirket ağına bağlanması gerekir). Bazıları, kullanıcının Çok faktörlü kimlik doğrulaması ayarlamalarına veya Microsoft Authenticator'ı aygıtlarına yüklemelerine yardımcı olur.

### <a name="msaluirequiredexception-classification-enumeration"></a>`MsalUiRequiredException`sınıflandırma numaralandırma

MSAL, daha `Classification` iyi bir kullanıcı deneyimi sağlamak için okuyabileceğiniz bir alanı ortaya çıkarır, örneğin kullanıcıya parolalarının süresinin dolduğunu veya bazı kaynakları kullanmak için izin vermeleri gerektiğini söylemek için. Desteklenen değerler enumun `UiRequiredExceptionClassification` bir parçasıdır:

| Sınıflandırma    | Anlamı           | Önerilen kullanım |
|-------------------|-------------------|----------------------|
| Temel Eylem | Durum, etkileşimli kimlik doğrulama akışı sırasında kullanıcı etkileşimi ile çözülebilir. | Çağrı AcquireTokenInteractively(). |
| EkEylem | Durum, etkileşimli kimlik doğrulama akışının dışında, sistemle ek düzeltici etkileşimle çözülebilir. | İyileştirici eylemi açıklayan bir ileti göstermek için AcquireTokenInteractively() numaralı telefonu arayın. Arama uygulaması, kullanıcının düzeltici eylemi tamamlamaolasılığı düşükse, additional_action gerektiren akışları gizlemeyi seçebilir. |
| Yalnızca Mesaj      | Durum şu anda çözülemez. Etkileşimli kimlik doğrulama akışının başlatılması, durumu açıklayan bir ileti gösterir. | Durumu açıklayan bir ileti göstermek için AcquireTokenInteractively() numaralı telefonu arayın. AcquireTokenInteractively() kullanıcı iletiyi okuyup pencereyi kapattıktan sonra UserCanceled hatasını döndürür. Arama uygulaması, kullanıcının iletiden yararlanma olasılığı düşükse message_only neden olan akışları gizlemeyi seçebilir.|
| Gerekli Olan Onay  | Kullanıcı onayı eksik veya iptal edildi. | Kullanıcının onay vermesi için AcquireTokenInteractively() numaralı telefonu arayın. |
| Kullanıcı Şifresi Süresi Doldu | Kullanıcının parolası doldu. | Kullanıcının parolasını sıfırlayabilmesi için AcquireTokenInteractively() numaralı telefonu arayın. |
| PromptNeverFailed| Etkileşimli Kimlik Doğrulama parametre istemi=asla ile çağrıldı, MSAL tarayıcı çerezleri güvenmek ve tarayıcı görüntülemek için değil zorlayarak. Bu başarısız oldu. | Çağrı AcquireTokenInteractively() Prompt.None olmadan |
| Satın AlmaTokenSilentFailed | MSAL SDK önbellekten bir belirteç getirmek için yeterli bilgiye sahip değildir. Bunun nedeni önbellekte belirteç bulunmaması veya bir hesap bulunmaması olabilir. Hata iletisi daha fazla ayrıntıya sahiptir.  | Çağrı AcquireTokenInteractively(). |
| None    | Daha fazla ayrıntı verilmedi. Durum, etkileşimli kimlik doğrulama akışı sırasında kullanıcı etkileşimi ile çözülebilir. | Çağrı AcquireTokenInteractively(). |

## <a name="net-code-example"></a>.NET kodu örneği

```csharp
AuthenticationResult res;
try
{
 res = await application.AcquireTokenSilent(scopes, account)
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex) when (ex.ErrorCode == MsalError.InvalidGrantError)
{
 switch (ex.Classification)
 {
  case UiRequiredExceptionClassification.None:
   break;
  case UiRequiredExceptionClassification.MessageOnly:
  // You might want to call AcquireTokenInteractive(). Azure AD will show a message
  // that explains the condition. AcquireTokenInteractively() will return UserCanceled error
  // after the user reads the message and closes the window. The calling application may choose
  // to hide features or data that result in message_only if the user is unlikely to benefit 
  // from the message
  try
  {
      res = await application.AcquireTokenInteractive(scopes).ExecuteAsync();
  }
  catch (MsalClientException ex2) when (ex2.ErrorCode == MsalError.AuthenticationCanceledError)
  {
   // Do nothing. The user has seen the message
  }
  break;

  case UiRequiredExceptionClassification.BasicAction:
  // Call AcquireTokenInteractive() so that the user can, for instance accept terms
  // and conditions

  case UiRequiredExceptionClassification.AdditionalAction:
  // You might want to call AcquireTokenInteractive() to show a message that explains the remedial action. 
  // The calling application may choose to hide flows that require additional_action if the user 
  // is unlikely to complete the remedial action (even if this means a degraded experience)

  case UiRequiredExceptionClassification.ConsentRequired:
  // Call AcquireTokenInteractive() for user to give consent.
  
  case UiRequiredExceptionClassification.UserPasswordExpired:
  // Call AcquireTokenInteractive() so that user can reset their password
  
  case UiRequiredExceptionClassification.PromptNeverFailed:
  // You used WithPrompt(Prompt.Never) and this failed
  
  case UiRequiredExceptionClassification.AcquireTokenSilentFailed:
  default:
  // May be resolved by user interaction during the interactive authentication flow.
  res = await application.AcquireTokenInteractive(scopes)
                         .ExecuteAsync(); break;
 }
}
```

## <a name="javascript"></a>[Javascript](#tab/javascript)

MSAL.js, farklı türdeki ortak hataları özetleyen ve sınıflandıran hata nesneleri sağlar. Ayrıca, hataların uygun şekilde işlemek için hata iletileri gibi belirli ayrıntılarına erişmek için arayüz sağlar.

### <a name="error-object"></a>Hata nesnesi

```javascript
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```

Hata sınıfını genişleterek aşağıdaki özelliklere erişebilirsiniz:
- `AuthError.message`: Aynı `errorMessage`.
- `AuthError.stack`: Atılan hatalar için yığın izi.

### <a name="error-types"></a>Hata türleri

Aşağıdaki hata türleri kullanılabilir:

- `AuthError`: Beklenmeyen hatalar için de kullanılan MSAL.js kitaplığı için temel hata sınıfı.

- `ClientAuthError`: İstemci kimlik doğrulaması ile ilgili bir sorunu ifade eden hata sınıfı. Kitaplıktan gelen hataların çoğu ClientAuthErrors olacaktır. Bu hatalar, oturum açma zaten devam ederken, kullanıcı girişi iptal ettiğinde ve benzeri durumlarda oturum açma yöntemini çağırmak gibi nedenlerden kaynaklanır.

- `ClientConfigurationError`: Hata sınıfı, `ClientAuthError` verilen kullanıcı config parametreleri yanlış biçimlendirilmiş veya eksik olduğunda istekler yapılmadan önce atılan uzanır.

- `ServerError`: Hata sınıfı, kimlik doğrulama sunucusu tarafından gönderilen hata dizelerini temsil eder. Bunlar, geçersiz istek biçimleri veya parametreleri veya sunucunun kullanıcıyı doğrulamasını veya yetkilendirmesini engelleyen diğer hatalar gibi hatalar olabilir.

- `InteractionRequiredAuthError`: Hata sınıfı, `ServerError` etkileşimli bir çağrı gerektiren sunucu hatalarını temsil etmek için uzanır. Bu hata, `acquireTokenSilent` kullanıcının kimlik bilgilerini veya kimlik doğrulama/yetkilendirme izni sağlamak için sunucuyla etkileşimde olması gerekiyorsa tarafından atılır. Hata kodları `"interaction_required"` `"login_required"`, `"consent_required"`, ve .

Yönlendirme yöntemleriile kimlik doğrulama akışlarında`loginRedirect`hata `acquireTokenRedirect`işleme için ( , ), yönlendirme yöntemini kullanarak `handleRedirectCallback()` yeniden yönlendirme yönteminden sonra başarı veya başarısızlıkla çağrılan geri aramayı kaydetmeniz gerekir:

```javascript
function authCallback(error, response) {
    //handle redirect response
}

var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);
myMSALObj.acquireTokenRedirect(request);
```

Pop-up deneyimi`loginPopup`(, `acquireTokenPopup`) dönüş vaatleri için yöntemler, böylece söz deseni kullanabilirsiniz (.sonra ve .catch) gösterildiği gibi bunları işlemek için:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="errors-that-require-interaction"></a>Etkileşim gerektiren hatalar

Bir belirteç edinme etkileşimli olmayan bir yöntem kullanmaya çalıştığınızda `acquireTokenSilent`bir hata döndürülür , ancak MSAL sessizce yapamadı.

Olası nedenler şunlardır:

- oturum açmanız gerekir
- rıza göstermeniz gerekir
- çok faktörlü bir kimlik doğrulama deneyimi yaşamanız gerekir.

Düzeltme gibi interaktif bir yöntem `acquireTokenPopup` aramaktır `acquireTokenRedirect`ya da:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

## <a name="python"></a>[Python](#tab/python)

Python için MSAL'da, hataların çoğu API çağrısından bir geri dönüş değeri olarak aktarılır. Hata, Microsoft kimlik platformundan Gelen JSON yanıtını içeren bir sözlük olarak temsil edilir.

* Başarılı bir `"access_token"` yanıt anahtarı içerir. Yanıtın biçimi OAuth2 protokolü tarafından tanımlanır. Daha fazla bilgi için bkz: [5.1 Başarılı Yanıt](https://tools.ietf.org/html/rfc6749#section-5.1)
* Bir hata `"error"` yanıtı `"error_description"`içerir ve genellikle . Yanıtın biçimi OAuth2 protokolü tarafından tanımlanır. Daha fazla bilgi için [bkz: 5.2 Hata Yanıtı](https://tools.ietf.org/html/rfc6749#section-5.2)

Bir hata döndürüldüğünde, `"error_description"` anahtar insan tarafından okunabilir bir ileti içerir; sırayla genellikle bir Microsoft kimlik platformu hata kodu içerir. Çeşitli hata kodları hakkında ayrıntılı bilgi için kimlik [doğrulama ve yetkilendirme hata kodlarına](https://docs.microsoft.com/azure/active-directory/develop/reference-aadsts-error-codes)bakın.

Python için MSAL'da, çoğu hata bir hata değeri döndürülerek işlendiğinden, özel durumlar nadirdir. Özel `ValueError` durum yalnızca kitaplığı nasıl kullanmaya çalıştığınızla ilgili bir sorun olduğunda (örneğin, API parametresi(ler) bozuk olduğunda atılır.

## <a name="java"></a>[Java](#tab/java)

Java için MSAL'da üç tür özel `MsalClientException` `MsalServiceException`durum `MsalInteractionRequiredException`vardır: , , ve; tüm bu `MsalException`miras .

- `MsalClientException`kitaplık veya aygıta yerel bir hata oluştuğunda atılır.
- `MsalServiceException`güvenli belirteç hizmeti (STS) bir hata yanıtı döndürdüğünde veya başka bir ağ hatası oluştuğunda atılır.
- `MsalInteractionRequiredException`kimlik doğrulamanın başarılı olması için UI etkileşimi gerektiğinde atılır.

### <a name="msalserviceexception"></a>MsalServiceException

`MsalServiceException`STS istekleri döndürülen HTTP üstbilgisini ortaya çıkarır. Onlara üzerinden erişin`MsalServiceException.headers()`

### <a name="msalinteractionrequiredexception"></a>MsalInteractionGerekli Özel Durum

Arama `AcquireTokenSilently()` yaparken Java için MSAL'dan döndürülen yaygın durum kodlarından biri `InvalidGrantError`. Bu, kimlik doğrulama belirteci verilmeden önce ek kullanıcı etkileşimi gerektiği anlamına gelir. Uygulamanız kimlik doğrulama kitaplığını yeniden, ancak genel `AuthorizationCodeParameters` `DeviceCodeParameters` istemci uygulamaları göndererek veya için etkileşimli modda aramalıdır.

Çoğu zaman başarısız `AcquireTokenSilently` olduğunda, bunun nedeni belirteç önbelleğinin isteğinizle eşleşen bir belirteci olmamasıdır. Erişim belirteçleri bir saat `AcquireTokenSilently` içinde sona erer ve yenileme belirteci dayalı yeni bir tane almak için çalışacağız. OAuth2 açısından, bu Token'i Yenile akışıdır. Bu akış, kiracı yöneticinin daha sıkı oturum açma ilkelerini yapılandırması gibi çeşitli nedenlerle de başarısız olabilir.

Bu hataya neden olan bazı koşulların kullanıcılar için kolayca çözülmesi kolaydır. Örneğin, Kullanım Koşullarını kabul etmeleri gerekebilir. Veya makinenin belirli bir şirket ağına bağlanması gerektiğinden, istek geçerli yapılandırmayla birlikte yerine getirilemez.

MSAL, daha `reason` iyi bir kullanıcı deneyimi sağlamak için kullanabileceğiniz bir alanı ortaya çıkarır. Örneğin, `reason` alan kullanıcıya parolalarının süresinin dolduğunu veya bazı kaynakları kullanmak için izin vermeleri gerektiğini söylemenize neden olabilir. Desteklenen değerler enumun `InteractionRequiredExceptionReason` bir parçasıdır:

| Neden | Anlamı | Önerilen Kullanım |
|---------|-----------|-----------------------------|
| `BasicAction` | Koşul, etkileşimli kimlik doğrulama akışı sırasında kullanıcı etkileşimi ile çözülebilir | Etkileşimli parametrelerle arama `acquireToken` |
| `AdditionalAction` | Durum, etkileşimli kimlik doğrulama akışının dışında sistemle ek düzeltici etkileşimle çözülebilir. | Yapılacak `acquireToken` düzeltici eylemi açıklayan bir ileti göstermek için etkileşimli parametrelerle arama. Arama uygulaması, kullanıcının düzeltici eylemi tamamlamaolasılığı düşükse, ek eylem gerektiren akışları gizlemeyi seçebilir. |
| `MessageOnly` | Durum şu anda çözülemez. Durumu açıklayan bir ileti göstermek için etkileşimli kimlik doğrulama akışını başlatın. | Durumu `acquireToken` açıklayan bir ileti göstermek için etkileşimli parametrelerle arayın. `acquireToken`kullanıcı iletiyi okuyup pencereyi kapattıktan sonra `UserCanceled` hatayı döndürür. Uygulama, kullanıcının iletiden yararlanma olasılığı düşükse, iletiyle sonuçlanan akışları gizlemeyi seçebilir. |
| `ConsentRequired`| Kullanıcı onayı eksik veya iptal edildi. |Kullanıcının izin verabilmesi için etkileşimli parametrelerle arayın. `acquireToken` |
| `UserPasswordExpired` | Kullanıcının parolası doldu. | Kullanıcının parolasını sıfırlayabilmesi için etkileşimli parametreyle arama `acquireToken` |
| `None` |  Daha fazla bilgi verilmiştir. Durum, etkileşimli kimlik doğrulama akışı sırasında kullanıcı etkileşimi tarafından çözülebilir. | Etkileşimli parametrelerle arama `acquireToken` |

### <a name="code-example"></a>Kod Örneği

```java
        IAuthenticationResult result;
        try {
            PublicClientApplication application = PublicClientApplication
                    .builder("clientId")
                    .b2cAuthority("authority")
                    .build();

            SilentParameters parameters = SilentParameters
                    .builder(Collections.singleton("scope"))
                    .build();

            result = application.acquireTokenSilently(parameters).join();
        }
        catch (Exception ex){
            if(ex instanceof MsalInteractionRequiredException){
                // AcquireToken by either AuthorizationCodeParameters or DeviceCodeParameters
            } else{
                // Log and handle exception accordingly
            }
        }
```

## <a name="iosmacos"></a>[iOS/macOS](#tab/iosmacos)

iOS ve macOS hataları için MSAL'ın tam listesi [MSALError enum'da](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALError.h#L128)listelenmiştir.

MSAL üretilen tüm hatalar `MSALErrorDomain` etki alanı ile döndürülür.

Sistem hataları için, MSAL `NSError` sistem API orijinal döndürür. Örneğin, belirteç edinimi ağ bağlantısı eksikliği nedeniyle başarısız olursa, `NSURLErrorDomain` MSAL etki alanı ve `NSURLErrorNotConnectedToInternet` kodile ilgili bir hata döndürür.

İstemci tarafında en az iki MSAL hatasını işlemenizi öneririz:

- `MSALErrorInteractionRequired`: Kullanıcı etkileşimli bir istekte bulunmalıdır. Süresi dolmuş kimlik doğrulama oturumu veya ek kimlik doğrulama gereksinimleri gereksinimi gibi bu hataya neden olabilecek birçok koşul vardır. Kurtarmak için MSAL etkileşimli belirteç edinme API'sini arayın. 

- `MSALErrorServerDeclinedScopes`: Kapsamların bir kısmı veya tamamı reddedildi. Yalnızca verilen kapsamlarla devam edip etmeyeceğine veya oturum açma işlemini durdurup durdurmamaya karar verin.

> [!NOTE]
> Enum `MSALInternalError` yalnızca başvuru ve hata ayıklama için kullanılmalıdır. Çalışma zamanında bu hataları otomatik olarak işlemeye çalışmayın. Uygulamanız altında `MSALInternalError`kalan hatalardan herhangi biri ile karşılaşırsa, genel bir kullanıcının ne olduğunu açıklayan bir mesajla karşı karşıya olduğunu göstermek isteyebilirsiniz.

Örneğin, `MSALInternalErrorBrokerResponseNotReceived` kullanıcının kimlik doğrulamasını tamamlamadığı ve uygulamaya el ile döndürüldettiği anlamına gelir. Bu durumda, uygulamanız kimlik doğrulamanın tamamlanmadığını açıklayan genel bir hata iletisi göstermeli ve yeniden kimlik doğrulamaya çalıştıklarını önermelidir.

Aşağıdaki Objective-C örnek kodu, bazı yaygın hata koşullarını işlemek için en iyi uygulamaları gösterir:

```objc
    MSALInteractiveTokenParameters *interactiveParameters = ...;
    MSALSilentTokenParameters *silentParameters = ...;
    
    MSALCompletionBlock completionBlock;
    __block __weak MSALCompletionBlock weakCompletionBlock;
    
    weakCompletionBlock = completionBlock = ^(MSALResult *result, NSError *error)
    {
        if (!error)
        {
            // Use result.accessToken
            NSString *accessToken = result.accessToken;
            return;
        }
        
        if ([error.domain isEqualToString:MSALErrorDomain])
        {
            switch (error.code)
            {
                case MSALErrorInteractionRequired:
                {
                    // Interactive auth will be required
                    [application acquireTokenWithParameters:interactiveParameters
                                            completionBlock:weakCompletionBlock];
                    
                    break;
                }
                    
                case MSALErrorServerDeclinedScopes:
                {
                    // These are list of granted and declined scopes.
                    NSArray *grantedScopes = error.userInfo[MSALGrantedScopesKey];
                    NSArray *declinedScopes = error.userInfo[MSALDeclinedScopesKey];
                    
                    // To continue acquiring token for granted scopes only, do the following
                    silentParameters.scopes = grantedScopes;
                    [application acquireTokenSilentWithParameters:silentParameters
                                                  completionBlock:weakCompletionBlock];
                    
                    // Otherwise, instead, handle error fittingly to the application context
                    break;
                }
                    
                case MSALErrorServerProtectionPoliciesRequired:
                {
                    // Integrate the Intune SDK and call the
                    // remediateComplianceForIdentity:silent: API.
                    // Handle this error only if you integrated Intune SDK.
                    // See more info here: https://aka.ms/intuneMAMSDK
                    
                    break;
                }
                    
                case MSALErrorUserCanceled:
                {
                    // The user cancelled the web auth session.
                    // You may want to ask the user to try again.
                    // Handling of this error is optional.
                    
                    break;
                }
                    
                case MSALErrorInternal:
                {
                    // Log the error, then inspect the MSALInternalErrorCodeKey
                    // in the userInfo dictionary.
                    // Display generic error message to the end user
                    // More detailed information about the specific error
                    // under MSALInternalErrorCodeKey can be found in MSALInternalError enum.
                    NSLog(@"Failed with error %@", error);
                    
                    break;
                }
                    
                default:
                    NSLog(@"Failed with unknown MSAL error %@", error);
                    
                    break;
            }
            
            return;
        }
        
        // Handle no internet connection.
        if ([error.domain isEqualToString:NSURLErrorDomain] && error.code == NSURLErrorNotConnectedToInternet)
        {
            NSLog(@"No internet connection.");
            return;
        }
        
        // Other errors may require trying again later,
        // or reporting authentication problems to the user.
        NSLog(@"Failed with error %@", error);
    };
    
    // Acquire token silently
    [application acquireTokenSilentWithParameters:silentParameters
                                  completionBlock:completionBlock];

     // or acquire it interactively.
     [application acquireTokenWithParameters:interactiveParameters
                             completionBlock:completionBlock];
```

```swift
    let interactiveParameters: MSALInteractiveTokenParameters = ...
    let silentParameters: MSALSilentTokenParameters = ...
            
    var completionBlock: MSALCompletionBlock!
    completionBlock = { (result: MSALResult?, error: Error?) in
                
        if let result = result
        {
            // Use result.accessToken
            let accessToken = result.accessToken
            return
        }

        guard let error = error as NSError? else { return }

        if error.domain == MSALErrorDomain, let errorCode = MSALError(rawValue: error.code)
        {
            switch errorCode
            {
                case .interactionRequired:
                    // Interactive auth will be required
                    application.acquireToken(with: interactiveParameters, completionBlock: completionBlock)

                case .serverDeclinedScopes:
                    let grantedScopes = error.userInfo[MSALGrantedScopesKey]
                    let declinedScopes = error.userInfo[MSALDeclinedScopesKey]

                    if let scopes = grantedScopes as? [String] {
                        silentParameters.scopes = scopes
                        application.acquireTokenSilent(with: silentParameters, completionBlock: completionBlock)
                    }
                        
                    case .serverProtectionPoliciesRequired:
                        // Integrate the Intune SDK and call the
                        // remediateComplianceForIdentity:silent: API.
                        // Handle this error only if you integrated Intune SDK.
                        // See more info here: https://aka.ms/intuneMAMSDK
                        break
                        
                    case .userCanceled:
                       // The user cancelled the web auth session.
                       // You may want to ask the user to try again.
                       // Handling of this error is optional.
                       break
                        
                    case .internal:
                        // Log the error, then inspect the MSALInternalErrorCodeKey
                        // in the userInfo dictionary.
                        // Display generic error message to the end user
                        // More detailed information about the specific error
                        // under MSALInternalErrorCodeKey can be found in MSALInternalError enum.
                        print("Failed with error \(error)");
                        
                    default:
                        print("Failed with unknown MSAL error \(error)")
            }
        }
                
        // Handle no internet connection.
        if error.domain == NSURLErrorDomain && error.code == NSURLErrorNotConnectedToInternet
        {
            print("No internet connection.")
            return
        }
                
        // Other errors may require trying again later,
        // or reporting authentication problems to the user.
        print("Failed with error \(error)");    
    }
   
    // Acquire token silently
    application.acquireToken(with: interactiveParameters, completionBlock: completionBlock)
 
    // or acquire it interactively.
    application.acquireTokenSilent(with: silentParameters, completionBlock: completionBlock)
```

---

## <a name="conditional-access-and-claims-challenges"></a>Koşullu Erişim ve talep zorlukları

Jetonları sessizce alırken, MFA ilkesi gibi [Koşullu Erişim talepleri,](../azuread-dev/conditional-access-dev-guide.md) erişmeye çalıştığınız bir API tarafından gerekli olduğunda uygulamanız hata alabilir.

Bu hatayı işlemek için desen etkileşimli MSAL kullanarak bir belirteç elde etmektir. Etkileşimli olarak bir belirteç edinme kullanıcı ister ve onlara gerekli Koşullu Erişim ilkesini karşılamak için fırsat verir.

Koşullu Erişim gerektiren bir API'yi ararken bazı durumlarda, API'den hatayla ilgili bir talep itirazı alabilirsiniz. Örneğin, Koşullu Erişim ilkesi yönetilen bir aygıta (Intune) sahip olacaksa, hata [AADSTS53000](reference-aadsts-error-codes.md) gibi bir şey olacaktır: Cihazınızın bu kaynağa veya benzer bir şeye erişebilmek için yönetilmesi gerekir. Bu durumda, kullanıcının uygun ilkeyi karşılaması istenecek şekilde edinme belirteci çağrısındaki talepleri iletebilirsiniz.

### <a name="net"></a>.NET

MSAL.NET Koşullu Erişim gerektiren bir API'yi ararken, uygulamanızın talep zorluğu özel durumlarını işlemesi gerekir. Bu, [Talepler](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet) özelliğinin boş olmayacağı bir [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) olarak görünür.

Talep zorluk işlemek için sınıfın `.WithClaim()` yöntemini `PublicClientApplicationBuilder` kullanmanız gerekir.

### <a name="javascript"></a>JavaScript

MSAL.js kullanarak jetonları sessizce (kullanarak) `acquireTokenSilent`alırken, mfa ilkesi gibi [koşullu erişim iddiaları,](../azuread-dev/conditional-access-dev-guide.md) erişmeye çalıştığınız bir API tarafından gerekli olduğunda uygulamanız hata alabilir.

Bu hatayı işlemek için desen gibi veya `acquireTokenPopup` `acquireTokenRedirect` aşağıdaki örnekte olduğu gibi MSAL.js belirteci elde etmek için etkileşimli bir arama yapmaktır:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function (accessTokenResponse) {
    // call API
}).catch( function (error) {
    if (error instanceof InteractionRequiredAuthError) {
        // Extract claims from error message
        accessTokenRequest.claimsRequest = extractClaims(error.errorMessage);
        // call acquireTokenPopup in case of InteractionRequiredAuthError failure
        myMSALObj.acquireTokenPopup(accessTokenRequest).then(function (accessTokenResponse) {
            // call API
        }).catch(function (error) {
            console.log(error);
        });
    }
});
```

Belirteci etkileşimli olarak edinme, kullanıcıyı ister ve onlara gerekli Koşullu Erişim ilkesini karşılama fırsatı verir.

Koşullu Erişim gerektiren bir API'yi ararken, API'den hatayla ilgili bir talep itirazı alabilirsiniz. Bu durumda, uygun ilkekarşılamak için `claimsRequest` `AuthenticationParameters.ts` sınıf alanına hata döndürülen iddiaları geçirebilirsiniz. 

Daha fazla ayrıntı için [Ek Talep İsteme'ye](active-directory-optional-claims.md) bakın.

### <a name="msal-for-ios-and-macos"></a>iOS ve macOS için MSAL

iOS ve macOS için MSAL, hem etkileşimli hem de sessiz belirteç edinme senaryolarında belirli taleplertalep etmenizi sağlar.

Özel talepler istemek `claimsRequest` için, belirtin `MSALSilentTokenParameters` veya `MSALInteractiveTokenParameters`.

Daha fazla bilgi [için iOS ve macOS için MSAL'ı kullanarak Özel talepte](request-custom-claims.md) bulunun.

## <a name="retrying-after-errors-and-exceptions"></a>Hatalar ve özel durumlar sonra yeniden deneme

MSAL'ı ararken kendi yeniden deneme ilkelerini uygulamanız beklenir. MSAL, AAD hizmetine HTTP aramaları yapar ve örneğin ağ kapanabilir veya sunucu aşırı yüklenebilir gibi zaman zaman hatalar oluşabilir.  

### <a name="http-error-codes-500-600"></a>HTTP hata kodları 500-600

MSAL.NET, HTTP hata kodları 500-600 ile hatalar için basit bir yeniden deneme mekanizması uygular.

### <a name="http-429"></a>HTTP 429

Hizmet Belirteç Sunucusu (STS) çok fazla istekle aşırı yüklendiğinde, `Retry-After` yanıt alanında yeniden denemenizin ne kadar süreceği hakkında bir ipucuyla HTTP hata 429'u döndürür.

### <a name="net"></a>.NET

[MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) bir `System.Net.Http.Headers.HttpResponseHeaders` özellik `namedHeaders`olarak yüzeyler. Uygulamalarınızın güvenilirliğini artırmak için hata kodundan ek bilgiler kullanabilirsiniz. Açıklanan durumda, (tür) `RetryAfterproperty` `RetryConditionHeaderValue`ve yeniden denemek için hesaplama kullanabilirsiniz.

Burada istemci kimlik bilgileri akışını kullanarak bir daemon uygulaması için bir örnektir. Bunu, belirteç edinme yöntemlerinden herhangi biri için uyarlayabilirsiniz.

```csharp
do
{
    retry = false;
    TimeSpan? delay;
    try
    {
         result = await publicClientApplication.AcquireTokenForClient(scopes, account)
                                           .ExecuteAsync();
    }
    catch (MsalServiceException serviceException)
    {
         if (ex.ErrorCode == "temporarily_unavailable")
         {
             RetryConditionHeaderValue retryAfter = serviceException.Headers.RetryAfter;
             if (retryAfter.Delta.HasValue)
             {
                 delay = retryAfter.Delta;
             }
             else if (retryAfter.Date.HasValue)
             {
                 delay = retryAfter.Date.Value.Offset;
             }
         }
    }
    …
    if (delay.HasValue)
    {
        Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
        retry = true;
    }
} while (retry);
```
