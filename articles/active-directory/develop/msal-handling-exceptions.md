---
title: Hatalar ve özel durumlar (MSAL) | Mavisi
description: MSAL uygulamalarında hataları ve özel durumları, koşullu erişimi ve talep sınamasını nasıl ele alabileceğinizi öğrenin.
services: active-directory
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
ms.author: negoe
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 280746281fd45b3286cc76be5d3483f0cc65f90f
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872794"
---
# <a name="handling-exceptions-and-errors-using-msal"></a>MSAL kullanarak özel durumları ve hataları işleme

Microsoft kimlik doğrulama kitaplığı 'ndaki (MSAL) özel durumlar, uygulama geliştiricilerinin son kullanıcılara görüntüleme konusunda sorun gidermesi ve bu işlemleri sürdürmelerine yöneliktir. Özel durum iletileri yerelleştirilmemiş.

Özel durumları ve hataları işlerken, özel durum türünü ve hata kodunu kullanarak özel durumlar arasında ayrım yapabilirsiniz.  Hata kodlarının bir listesi için bkz. [kimlik doğrulama ve yetkilendirme hata kodları](reference-aadsts-error-codes.md).

## <a name="net-exceptions"></a>.NET özel durumları
Özel durumları işlerken, özel durumları ayırt etmek için özel durum türünün kendisini `ErrorCode` ve üyesini kullanabilirsiniz. Değerleri `ErrorCode` [msalerror](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet)türünde sabitler.

Ayrıca, [Msalclientexception](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet), [msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet), [msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet)alanlarına göz atabilirsiniz.

[Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) oluşturulursa, hata kodu [kimlik doğrulama ve yetkilendirme hata kodlarında](reference-aadsts-error-codes.md)bulabileceğiniz bir kod içerebilir.

### <a name="common-exceptions"></a>Sık karşılaşılan özel durumlar
Aşağıda, oluşturulan ve bazı olası azaltmalar gibi yaygın özel durumlar verilmiştir.

| Özel durum | Hata kodu | Risk azaltma|
| --- | --- | --- |
| [Msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: Kullanıcı veya yönetici, ' {appName} ' adlı ' {AppID} ' KIMLIĞINE sahip uygulamayı kullanmaya yönelik değil. Bu Kullanıcı ve kaynak için etkileşimli bir yetkilendirme isteği gönderin.| Önce kullanıcı onayı almanız gerekir. .NET Core kullanmıyorsanız (herhangi bir Web Kullanıcı arabirimine sahip olmayan), çağırın (yalnızca bir kez) `AcquireTokeninteractive`. .NET Core kullanıyorsanız veya bir `AcquireTokenInteractive`yapmak istemiyorsanız, Kullanıcı onay vermek için bir URL 'ye gidebilir:. https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read Şunu çağırmak `AcquireTokenInteractive`için:`app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [Msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: Kullanıcının Multi-Factor Authentication kullanması gerekir.| Risk azaltma yok-kiracınız için MFA yapılandırıldıysa ve AAD bunu zorlamaya karar verirse, `AcquireTokenInteractive` veya `AcquireTokenByDeviceCode`gibi etkileşimli bir akışa geri dönüş yapmanız gerekir.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) |AADSTS90010: İzin türü *sık karşılaşılan* veya */tüketiciler* uç noktaları üzerinden desteklenmez. */Kuruluşlar* veya kiracıya özgü uç noktayı kullanın. *Sık karşılaşılan*kullandınız.| Azure AD 'nin iletisinde açıklandığı gibi, yetkilinin bir kiracıya ya da aksi takdirde */kuruluşlara*sahip olması gerekir.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002 İstek gövdesi şu parametreyi içermelidir: ' client_secret veya client_assertion '.| Bu özel durum, uygulamanız Azure AD 'de ortak bir istemci uygulaması olarak kaydettirilmemişse meydana gelebilir. Azure Portal, uygulamanızın bildirimini düzenleyin ve öğesini `allowPublicClient` olarak `true`ayarlayın. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| unknown_user Iletisi: Oturum açmış kullanıcı tanımlanamadı| Kitaplık, geçerli Windows oturum açmış kullanıcıyı sorgulayamadı veya bu kullanıcı AD veya AAD 'ye katılmış değil (iş yeri katılmış kullanıcıları desteklenmez). Hafifletme 1: UWP 'de, uygulamanın aşağıdaki yeteneklere sahip olup olmadığını denetleyin: Kurumsal kimlik doğrulaması, özel ağlar (Istemci ve sunucu), Kullanıcı hesabı bilgileri. Risk azaltma 2: Kullanıcı adını (örneğin, john@contoso.com) getirmek için kendi mantığınızı uygulayın ve Kullanıcı adında alan `AcquireTokenByIntegratedWindowsAuth` formu kullanın.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Bu yöntem, Active Directory (AD) tarafından sunulan bir protokolü kullanır. Kullanıcı, AD ("yönetilen" Kullanıcı) olmadan Azure Active Directory oluşturulduysa, bu yöntem başarısız olur. AD 'de oluşturulan ve AAD tarafından desteklenen kullanıcılar ("federe" kullanıcılar), etkileşimli olmayan bu kimlik doğrulama yönteminden yararlanabilir. Mayı Etkileşimli kimlik doğrulaması kullanın.|

### `MsalUiRequiredException`

`AcquireTokenSilent()` Çağrılırken`MsalError.InvalidGrantError`msal.NET tarafından döndürülen yaygın durum kodlarından biri. Bu durum kodu, uygulamanın kimlik doğrulama kitaplığını yeniden çağırması, ancak etkileşimli modda (genel istemci uygulamaları için Acquiretokenınteractive veya AcquireTokenByDeviceCodeFlow) ve Web uygulamalarında bir zorluk yapması gerektiği anlamına gelir. Bunun nedeni, kimlik doğrulama belirtecinin verilebilmesi için ek kullanıcı etkileşiminin gerekli olmasından kaynaklanır.

Çoğu zaman başarısız olduğunda `AcquireTokenSilent` , bunun nedeni belirteç önbelleğinde isteğinizle eşleşen belirteçleri yoktur. Erişim belirteçleri 1 saat içinde sona erer ve `AcquireTokenSilent` yenileme belirtecine göre yeni bir tane getirmeye çalışır (OAuth2 koşullarında, bu "belirteci Yenile ' akışındadır). Bu akış çeşitli nedenlerle da başarısız olabilir. Örneğin, bir kiracı yöneticisi daha sıkı oturum açma ilkeleri yapılandırır. 

Kullanıcının bir eylem yapması için etkileşim amaçlar. Bu koşullardan bazılarının, kullanıcıların çözebilmesi (örneğin, tek tıklamayla kullanım koşullarını kabul etmesi) ve bazıları geçerli yapılandırmayla çözümlenemez (örneğin, söz konusu makinenin belirli bir şirket ağına bağlanması gerekir). Kullanıcı, Multi-Factor Authentication 'ı ayarlamaya veya Microsoft Authenticator yüklemesine yardımcı olur.

### <a name="msaluirequiredexception-classification-enumeration"></a>`MsalUiRequiredException`sınıflandırma numaralandırması

MSAL, daha `Classification` iyi bir kullanıcı deneyimi sağlamak için okuyabilecekleri bir alan gösterir. Örneğin, kullanıcının parolasının bittiğini veya bazı kaynakları kullanma onayı sağlaması gerektiğini bildirmek gibi. Desteklenen değerler `UiRequiredExceptionClassification` numaralandırmanın bir parçasıdır:

| Sınıflandırma    | Anlamı           | Önerilen işleme |
|-------------------|-------------------|----------------------|
| BasicAction | Durum, etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir. | Acquiretokenetkileşimli () çağrısı yapın. |
| Ek | Durum, sistem ile etkileşimli kimlik doğrulama akışının dışında ek düzeltici etkileşim tarafından çözülebilir. | Düzeltici eylemi açıklayan bir ileti göstermek için Acquiretokenetkileşimli () çağrısı yapın. Çağıran uygulama, Kullanıcı düzeltici eylemi tamamlamayabilir additional_action gerektiren akışları gizlemeyi seçebilir. |
| Yalnızca Message      | Koşul şu anda çözümlenemiyor. Etkileşimli kimlik doğrulama akışının başlatılması, koşulu açıklayan bir ileti gösterir. | Koşulu açıklayan bir ileti göstermek için Acquiretokenetkileşimli () çağırın. Acquiretokenetkileşimli () Kullanıcı iletiyi okuduktan sonra pencereyi kapattığında Useriptal hatası döndürür. Çağıran uygulama, message_only ile sonuçlanan akışları gizlemeyi seçebilir ve Kullanıcı iletiden daha fazla avantaj sağlamaması durumunda olabilir.|
| ConsentRequired  | Kullanıcı onayı eksik veya iptal edildi. | Kullanıcının izin vermesi için Acquiretokenetkileşimli () çağrısı yapın. |
| UserPasswordExpired | Kullanıcının parolasının süresi doldu. | Kullanıcının parolasını sıfırlayabilmesi için Acquiretokenetkileşimli () çağırın. |
| Promptnewverfailed| Etkileşimli kimlik doğrulaması, parametre istemiyle birlikte çağrıldı = hiçbir şekilde, MSAL, tarayıcı tanımlama bilgilerine dayanmaya zorlanıyor ve tarayıcının görüntülenmesini gerektirmez. Bu başarısız oldu. | Prompt. None olmadan Acquiretokenetkileşimli () çağrısı yapın |
| AcquireTokenSilentFailed | MSAL SDK, önbellekten belirteç getirmek için yeterli bilgi içermiyor. Bunun nedeni önbellekte hiçbir belirteç olmaması veya bir hesabın bulunamaması olabilir. Hata iletisinde daha fazla ayrıntı vardır.  | Acquiretokenetkileşimli () çağrısı yapın. |
| Yok.    | Daha fazla ayrıntı sağlanmaz. Durum, etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir. | Acquiretokenetkileşimli () çağrısı yapın. |

## <a name="code-example"></a>Kod örneği

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
   res = await application.AcquireTokenInteractive(scopes)
                          .ExecuteAsync();
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


## <a name="javascript-errors"></a>JavaScript hataları

MSAL. js, soyut ve farklı türlerde yaygın hataların sınıflandırmakta olan hata nesneleri sağlar. Ayrıca, uygun şekilde işlemek üzere hata iletileri gibi hataların belirli ayrıntılarına erişmek için arabirim sağlar.

**Hata nesnesi**

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
* **Kimlik doğrulama hatası. ileti:**  ErrorMessage ile aynı.
* **AuthError. Stack:** Oluşan hatalar için yığın izlemesi. Kaynak hata noktasına izlemenin yapılmasına izin verir.

**Hata türleri**

Aşağıdaki hata türleri kullanılabilir:

* *Kimlik doğrulama hatası:* MSAL. js kitaplığı için temel hata sınıfı, ayrıca beklenmeyen hatalar için de kullanılır.

* *ClientAuthError:* Istemci kimlik doğrulamasıyla ilgili bir sorunu gösteren hata sınıfı. Kitaplıktan gelen hataların çoğu ClientAuthErrors olur. Bu hatalar, oturum açma işlemi devam ederken oturum açma ve Kullanıcı oturum açmayı iptal eden bir oturum açma yöntemi çağırma gibidir. 

* *ClientConfigurationError:* Verilen Kullanıcı yapılandırma parametreleri hatalı biçimlendirilmiş veya eksik olduğunda istekler yapılmadan önce ClientAuthError genişletme hata sınıfı.

* *ServerError* Kimlik doğrulama sunucusu tarafından gönderilen hata dizelerini temsil eden hata sınıfı. Bunlar geçersiz istek biçimleri veya parametreler gibi hatalar ya da sunucunun kimlik doğrulamasını veya yetkilendirmasını engelleyen başka hatalar olabilir.

* *Interactionrequiredadutherror:* Etkileşimli çağrı gerektiren sunucu hatalarını göstermek için ServerError 'ı genişleten hata sınıfı. Bu hata, kullanıcının kimlik `acquireTokenSilent` doğrulaması/yetkilendirme için kimlik bilgileri veya onay sağlamak üzere sunucuyla etkileşmesi gerekiyorsa oluşur. Hata kodları "interaction_required", "login_required", "consent_required" içerir.

Yeniden yönlendirme Yöntemleri (`loginRedirect`, `acquireTokenRedirect`) ile kimlik doğrulaması akışlarında hata işleme için, aşağıdaki gibi, using `handleRedirectCallback()` yönteminden sonra başarılı veya başarısız olarak çağrılan geri çağırma işlemini kaydetmeniz gerekir:

```javascript
function authCallback(error, response) {
    //handle redirect response
}


var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);

myMSALObj.acquireTokenRedirect(request);
```

Açılır deneyim (`loginPopup`, `acquireTokenPopup`) için yöntemler, ' ı geri döndürmekte, bu nedenle bunları gösterildiği gibi işlemek için Promise modelini (. then ve. catch) kullanabilirsiniz:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="interaction-required-errors"></a>Etkileşim gerekli hataları

Bir belirteç almak için etkileşimli olmayan bir yöntem kullanmayı denediğinizde (örneğin, `acquireTokenSilent`) ve msal bunu sessizce yapamadık bir hata döndürülür.

Olası nedenler şunlardır:

* kullanıcının oturum açması gerekiyor
* kullanıcının onayı gerekir
* kullanıcının bir Multi-Factor Authentication deneyimi üzerinden geçmesi gerekir.

Düzeltme, `acquireTokenPopup` veya `acquireTokenRedirect`gibi etkileşimli bir yöntemi çağırmalıdır:

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

## <a name="conditional-access-and-claims-challenges"></a>Koşullu erişim ve talep sorunları
Belirteçleri sessizce alırken, erişmeye çalıştığınız bir API 'nin MFA İlkesi gibi [koşullu erişim talep zorluğu](conditional-access-dev-guide.md) gerektiğinde uygulamanız hatalar alabilir.

Bu hatayı işleyecek desenler, MSAL kullanarak etkileşimli bir şekilde belirteç elde etmek için kullanılır. Bir belirteci etkileşimli bir şekilde almak kullanıcıya sorar ve gerekli koşullu erişim ilkesini karşılamak için bu fırsata yanıt verir.

Belirli durumlarda, koşullu erişim gerektiren bir API 'yi çağırırken API 'den hata ile bir talep zorluğu alabilirsiniz. Örneğin, koşullu erişim ilkesinde yönetilen bir cihaz (Intune) varsa, hata AADSTS53000 gibi [bir şey olacaktır: Cihazınızın bu kaynağa](reference-aadsts-error-codes.md) veya benzer bir değere erişmek için yönetilmesi gerekir. Bu durumda, kullanıcıdan uygun ilkeyi karşılaması istenir diye, talepleri alma çağrısında geçirebilirsiniz.

### <a name="net"></a>.NET
MSAL.NET 'ten koşullu erişim gerektiren bir API çağrılırken, uygulamanızın talep sınama özel durumlarını işlemesi gerekir. Bu, [talepler](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet) özelliğinin boş olacağı bir [Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) olarak görünür.

Talep sınamasını işlemek için, `.WithClaim()` `PublicClientApplicationBuilder` sınıfının yöntemini kullanmanız gerekir.

### <a name="javascript"></a>JavaScript
MSAL. js kullanarak belirteçleri sessizce `acquireTokenSilent`(kullanarak) elde etmek için, erişmeye çalıştığınız bir API 'nin MFA İlkesi gibi bir [koşullu erişim talep zorluğu](conditional-access-dev-guide.md) gerektiğinde, uygulamanız hatalar alabilir.

Bu hatayı işleme deseninin, aşağıdaki örnekte `acquireTokenPopup` `acquireTokenRedirect` olduğu gibi, msal. js ' de belirteç almak için etkileşimli bir çağrı yapmak için kullanılır:

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

Belirteci etkileşimli olarak almak kullanıcıya sorar ve gerekli koşullu erişim ilkesini karşılamak için bu fırsata yanıt verir.

Koşullu erişim gerektiren bir API çağrılırken, API 'den hatada bir talep zorluğu alabilirsiniz. Bu durumda, hatada `claimsRequest` döndürülen talepleri, uygun ilkeyi karşılamak üzere `AuthenticationParameters.ts` sınıfının alanına geçirebilirsiniz. 

Daha fazla ayrıntı için bkz. [ek talepler isteme](active-directory-optional-claims.md) .

## <a name="retrying-after-errors-and-exceptions"></a>Hatalar ve özel durumlar sonrasında yeniden deneniyor

MSAL çağrılırken size kendi yeniden deneme ilkelerinizi uygulamanız bekleniyor. MSAL AAD hizmetine HTTP çağrıları yapar ve zaman zaman meydana gelebilir, örneğin ağ kapatılabilir veya sunucu aşırı yüklenmiş olabilir.  

### <a name="http-error-codes-500-600"></a>HTTP hata kodları 500-600

MSAL.NET, HTTP hata kodları 500-600 ile hatalar için basit bir yeniden deneme mekanizması uygular.

### <a name="http-429"></a>HTTP 429

Hizmet belirteci sunucusu (STS) çok fazla istek ile aşırı yüklendiğinde, zaman içinde yeniden deneyebilmeniz için bir ipucu içeren bir HTTP hatası 429 döndürür. Hata, `Retry-After` yanıt alanından okunabilir.

#### <a name="net"></a>.NET

[Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) özel durumu özellik `System.Net.Http.Headers.HttpResponseHeaders` `namedHeaders`olarak yüzeyler. Bu nedenle, uygulamalarınızın güvenilirliğini artırmak için hata kodundan ek bilgilerden yararlanabilirsiniz. Az önce açıklandığımız durumda, `RetryAfterproperty` (türünde `RetryConditionHeaderValue`) ve işlem yeniden deneneceği işlemler ' i kullanabilirsiniz.

Bir Daemon uygulaması (istemci kimlik bilgileri akışını kullanarak) için bir örnektir, ancak bunu bir belirteç alma yöntemlerinden herhangi birine uyarlayabilirsiniz.

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
