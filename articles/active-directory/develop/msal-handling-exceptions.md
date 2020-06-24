---
title: Hatalar ve özel durumlar (MSAL)
titleSuffix: Microsoft identity platform
description: MSAL uygulamalarında hataları ve özel durumları, koşullu erişimi ve talep sorunlarını nasıl ele alabileceğinizi öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: c27938227a13934de11dd6e88d58138c46c3f58e
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85204635"
---
# <a name="handle-msal-exceptions-and-errors"></a>MSAL özel durumlarını ve hatalarını işleme

Bu makalede, yaygın oturum açma hatalarını işlemeye yönelik farklı hata türleri ve önerilerin bir özeti verilmektedir.

## <a name="msal-error-handling-basics"></a>MSAL hata işleme temelleri

Microsoft kimlik doğrulama kitaplığı 'ndaki (MSAL) özel durumlar, uygulama geliştiricilerinin son kullanıcılara görüntüleme için değil, sorun gidermesi için tasarlanmıştır. Özel durum iletileri yerelleştirilmemiş.

Özel durumları ve hataları işlerken, özel durum türünü ve hata kodunu kullanarak özel durumlar arasında ayrım yapabilirsiniz.  Hata kodlarının bir listesi için bkz. [kimlik doğrulama ve yetkilendirme hata kodları](reference-aadsts-error-codes.md).

Oturum açma deneyimi sırasında, yarışmalar, koşullu erişim (MFA, cihaz yönetimi, konum tabanlı kısıtlamalar), belirteç verme ve teminat ve Kullanıcı özellikleri hakkında hatalarla karşılaşabilirsiniz.

Uygulamanızda hata işleme hakkında daha fazla ayrıntı için, kullandığınız dille eşleşen aşağıdaki bölüme bakın.

## <a name="net"></a>[.NET](#tab/dotnet)

.NET özel durumlarını işlerken özel `ErrorCode` durumları ayırt etmek için özel durum türünü ve üyesini kullanabilirsiniz. `ErrorCode`değerler [Msalerror](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet)türünde sabitler.

Ayrıca, [Msalclientexception](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet), [msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet)ve [msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet)alanlarına bakabilirsiniz.

[Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) oluşturulursa, kodun orada listelenip listelenmediğini görmek Için [kimlik doğrulama ve yetkilendirme hata kodlarını](reference-aadsts-error-codes.md) deneyin.

### <a name="common-net-exceptions"></a>Ortak .NET özel durumları

Aşağıda, ortaya çıkabilecek ve bazı olası azaltmalar bulunan yaygın özel durumlar verilmiştir:  

| Özel durum | Hata kodu | Risk azaltma|
| --- | --- | --- |
| [Msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: Kullanıcı veya yönetici ' {appName} ' adlı ' {AppID} ' KIMLIĞIYLE uygulamayı kullanmak üzere onay vermedi. Bu Kullanıcı ve kaynak için etkileşimli bir yetkilendirme isteği gönderin.| Önce kullanıcı onayı almanız gerekir. .NET Core kullanmıyorsanız (herhangi bir Web Kullanıcı arabirimine sahip olmayan), çağırın (yalnızca bir kez) `AcquireTokeninteractive` . .NET Core kullanıyorsanız veya bir yapmak istemiyorsanız `AcquireTokenInteractive` , Kullanıcı onay vermek için BIR URL 'ye gidebilir: `https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read` . Şunu çağırmak için `AcquireTokenInteractive` :`app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [Msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: kullanıcının [Multi-Factor Authentication 'ı (MFA)](../authentication/concept-mfa-howitworks.md)kullanması gerekir.| Risk azaltma yoktur. Kiracınız için MFA yapılandırıldıysa ve Azure Active Directory (AAD) uygulamayı zorlamaya karar verirse, veya gibi etkileşimli bir akışa geri dönebilmeniz gerekir `AcquireTokenInteractive` `AcquireTokenByDeviceCode` .|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) |AADSTS90010: izin türü *sık karşılaşılan* veya */tüketiciler* uç noktaları üzerinden desteklenmiyor. */Kuruluşlar* veya kiracıya özgü uç noktayı kullanın. *Sık karşılaşılan*kullandınız.| Azure AD 'nin iletisinde açıklandığı gibi, yetkilinin bir kiracıya ya da aksi takdirde */kuruluşlara*sahip olması gerekir.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002: istek gövdesi şu parametreyi içermelidir: `client_secret or client_assertion` .| Bu özel durum, uygulamanız Azure AD 'de ortak bir istemci uygulaması olarak kaydettirilmemişse oluşturulabilir. Azure portal, uygulamanızın bildirimini düzenleyin ve `allowPublicClient` olarak ayarlayın `true` . |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| `unknown_user Message`: Oturum açmış kullanıcı tanımlanamadı| Kitaplık, geçerli Windows oturum açmış kullanıcıyı sorgulayamadı veya bu kullanıcı AD veya AAD 'ye katılmış değil (iş yeri katılmış kullanıcıları desteklenmez). Hafifletme 1: UWP 'de, uygulamanın şu yeteneklere sahip olup olmadığını denetleyin: kurumsal kimlik doğrulaması, özel ağlar (Istemci ve sunucu), Kullanıcı hesabı bilgileri. Hafifletme 2: Kullanıcı adını (örneğin,) getirmek için kendi mantığınızı uygulayın john@contoso.com ve `AcquireTokenByIntegratedWindowsAuth` Kullanıcı adında alan formu kullanın.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Bu yöntem, Active Directory (AD) tarafından sunulan bir protokolü kullanır. Kullanıcı, AD ("yönetilen" Kullanıcı) olmadan Azure Active Directory oluşturulduysa, bu yöntem başarısız olur. AD 'de oluşturulan ve AAD tarafından desteklenen kullanıcılar ("federe" kullanıcılar), etkileşimli olmayan bu kimlik doğrulama yönteminden yararlanabilir. Risk azaltma: etkileşimli kimlik doğrulaması kullanın.|

### `MsalUiRequiredException`

Çağrılırken MSAL.NET tarafından döndürülen yaygın durum kodlarından biri `AcquireTokenSilent()` `MsalError.InvalidGrantError` . Bu durum kodu, uygulamanın kimlik doğrulama kitaplığını yeniden çağırması, ancak etkileşimli modda (genel istemci uygulamaları için Acquiretokenınteractive veya AcquireTokenByDeviceCodeFlow) ve Web uygulamalarında bir zorluk yapması gerektiği anlamına gelir. Bunun nedeni, kimlik doğrulama belirtecinin verilebilmesi için ek kullanıcı etkileşiminin gerekli olmasından kaynaklanır.

Çoğu zaman `AcquireTokenSilent` başarısız olduğunda, bunun nedeni belirteç önbelleğinde isteğinizle eşleşen belirteçleri yoktur. Erişim belirteçleri 1 saat içinde sona erer ve `AcquireTokenSilent` yenileme belirtecine göre yeni bir tane getirmeye çalışır (OAuth2 koşullarında, bu "belirteci Yenile ' akışındadır). Bu akış çeşitli nedenlerle da başarısız olabilir. Örneğin, bir kiracı yöneticisi daha sıkı oturum açma ilkeleri yapılandırır. 

Kullanıcının bir eylem yapması için etkileşim amaçlar. Bu koşullardan bazılarının, kullanıcıların çözebilmesi (örneğin, tek tıklamayla kullanım koşullarını kabul etmesi) ve bazıları geçerli yapılandırmayla çözümlenemez (örneğin, söz konusu makinenin belirli bir şirket ağına bağlanması gerekir). Kullanıcı, Multi-Factor Authentication 'ı ayarlamaya veya Microsoft Authenticator yüklemesine yardımcı olur.

### <a name="msaluirequiredexception-classification-enumeration"></a>`MsalUiRequiredException`sınıflandırma numaralandırması

MSAL `Classification` , daha iyi bir kullanıcı deneyimi sağlamak için okuyabilecekleri bir alan gösterir. Örneğin, kullanıcının parolasının bittiğini veya bazı kaynakları kullanma onayı sağlaması gerektiğini bildirmek gibi. Desteklenen değerler numaralandırmanın bir parçasıdır `UiRequiredExceptionClassification` :

| Sınıflandırma    | Anlamı           | Önerilen işleme |
|-------------------|-------------------|----------------------|
| BasicAction | Durum, etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir. | Acquiretokenetkileşimli () çağrısı yapın. |
| Ek | Durum, sistem ile etkileşimli kimlik doğrulama akışının dışında ek düzeltici etkileşim tarafından çözülebilir. | Düzeltici eylemi açıklayan bir ileti göstermek için Acquiretokenetkileşimli () çağrısı yapın. Çağıran uygulama, Kullanıcı düzeltici eylemi tamamlamayabilir additional_action gerektiren akışları gizlemeyi seçebilir. |
| Yalnızca Message      | Koşul şu anda çözümlenemiyor. Etkileşimli kimlik doğrulama akışının başlatılması, koşulu açıklayan bir ileti gösterir. | Koşulu açıklayan bir ileti göstermek için Acquiretokenetkileşimli () çağırın. Acquiretokenetkileşimli () Kullanıcı iletiyi okuduktan sonra pencereyi kapattığında Useriptal hatası döndürür. Çağıran uygulama, kullanıcının iletiden daha fazla avantaj sağlamaması halinde message_only elde edilen akışları gizlemek için seçim gösterebilir.|
| ConsentRequired  | Kullanıcı onayı eksik veya iptal edildi. | Kullanıcının izin vermesi için Acquiretokenetkileşimli () çağrısı yapın. |
| UserPasswordExpired | Kullanıcının parolasının süresi doldu. | Kullanıcının parolasını sıfırlayabilmesi için Acquiretokenetkileşimli () çağırın. |
| Promptnewverfailed| Etkileşimli kimlik doğrulaması, parametre istemiyle birlikte çağrıldı = hiçbir şekilde, MSAL, tarayıcı tanımlama bilgilerine dayanmaya zorlanıyor ve tarayıcının görüntülenmesini gerektirmez. Bu başarısız oldu. | Prompt. None olmadan Acquiretokenetkileşimli () çağrısı yapın |
| AcquireTokenSilentFailed | MSAL SDK, önbellekten belirteç getirmek için yeterli bilgi içermiyor. Bunun nedeni önbellekte hiçbir belirteç olmaması veya bir hesabın bulunamaması olabilir. Hata iletisinde daha fazla ayrıntı vardır.  | Acquiretokenetkileşimli () çağrısı yapın. |
| Yok    | Daha fazla ayrıntı sağlanmaz. Durum, etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir. | Acquiretokenetkileşimli () çağrısı yapın. |

## <a name="net-code-example"></a>.NET kod örneği

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

## <a name="javascript"></a>[JavaScript](#tab/javascript)

MSAL.js, soyut ve farklı türlerde ortak hataları sınıflandıran hata nesneleri sağlar. Ayrıca, uygun şekilde işlemek üzere hata iletileri gibi hataların belirli ayrıntılarına erişmek için arabirim sağlar.

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
- `AuthError.message`: İle aynı `errorMessage` .
- `AuthError.stack`: Oluşturulan hatalar için yığın izlemesi.

### <a name="error-types"></a>Hata türleri

Aşağıdaki hata türleri kullanılabilir:

- `AuthError`: MSAL.js kitaplığı için temel hata sınıfı, ayrıca beklenmeyen hatalar için kullanılır.

- `ClientAuthError`: Istemci kimlik doğrulamasıyla ilgili bir sorunu gösteren hata sınıfı. Kitaplıktan gelen hataların çoğu ClientAuthErrors olur. Bu hatalar, oturum açma işlemi devam ederken bir oturum açma yöntemi çağırma gibi işlemlerden kaynaklanır, Kullanıcı oturum açma işlemini iptal eder ve bu şekilde devam eder.

- `ClientConfigurationError`: `ClientAuthError` Belirtilen kullanıcı yapılandırma parametreleri hatalı biçimlendirilmiş veya eksik olduğunda istekler yapılmadan önce, hata sınıfı, genişletiliyor.

- `ServerError`: Error sınıfı, kimlik doğrulama sunucusu tarafından gönderilen hata dizelerini temsil eder. Bunlar geçersiz istek biçimleri veya parametreler gibi hatalar ya da sunucunun kimlik doğrulamasını veya yetkilendirmasını engelleyen başka hatalar olabilir.

- `InteractionRequiredAuthError`: Hata sınıfı, `ServerError` etkileşimli çağrı gerektiren sunucu hatalarını temsil edecek şekilde genişletilir. Bu hata, `acquireTokenSilent` kullanıcının kimlik doğrulaması/yetkilendirme için kimlik bilgileri veya onay sağlamak üzere sunucuyla etkileşmesi gerekiyorsa oluşur. Hata kodları `"interaction_required"` ,, `"login_required"` ve içerir `"consent_required"` .

Yeniden yönlendirme Yöntemleri (,) ile kimlik doğrulaması akışlarında hata işleme için `loginRedirect` `acquireTokenRedirect` , aşağıdaki gibi, using yönteminden sonra başarılı veya başarısız olarak çağrılan geri çağırma işlemini kaydetmeniz gerekir `handleRedirectCallback()` :

```javascript
function authCallback(error, response) {
    //handle redirect response
}

var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);
myMSALObj.acquireTokenRedirect(request);
```

Açılır deneyim (,) için yöntemler, `loginPopup` `acquireTokenPopup` ' ı geri döndürmekte, bu nedenle bunları gösterildiği gibi işlemek için Promise modelini (. then ve. catch) kullanabilirsiniz:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="errors-that-require-interaction"></a>Etkileşim gerektiren hatalar

Bir belirteci almak için etkileşimli olmayan bir yöntem kullanmaya çalıştığınızda bir hata döndürülür `acquireTokenSilent` , ancak msal bunu sessizce yapamadık.

Olası nedenler şunlardır:

- oturum açmanız gerekiyor
- onay yapmanız gerekir
- Multi-Factor Authentication deneyiminden gitmeniz gerekir.

Düzeltme, veya gibi etkileşimli bir yöntemi çağırmalıdır `acquireTokenPopup` `acquireTokenRedirect` :

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

Python için MSAL içinde, çoğu hata API çağrısından bir dönüş değeri olarak dağıtılır. Hata, Microsoft Identity platformundan gelen JSON yanıtını içeren bir sözlük olarak temsil edilir.

* Başarılı bir yanıt anahtarını içerir `"access_token"` . Yanıtın biçimi OAuth2 protokolü tarafından tanımlanır. Daha fazla bilgi için bkz. [5,1 başarılı yanıt](https://tools.ietf.org/html/rfc6749#section-5.1)
* Bir hata yanıtı içerir `"error"` ve genellikle `"error_description"` . Yanıtın biçimi OAuth2 protokolü tarafından tanımlanır. Daha fazla bilgi için bkz. [5,2 hata yanıtı](https://tools.ietf.org/html/rfc6749#section-5.2)

Bir hata döndürüldüğünde, `"error_description"` anahtar okunabilir bir ileti içerir; bu, genellikle bir Microsoft Identity platform hata kodu içerir. Çeşitli hata kodları hakkında daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme hata kodları](https://docs.microsoft.com/azure/active-directory/develop/reference-aadsts-error-codes).

Python için MSAL içinde, çoğu hata bir hata değeri döndürerek işlendiği için özel durumlar nadir bir durumdur. `ValueError`Özel durum yalnızca, API parametrelerinin hatalı biçimlendirilmiş olduğu gibi, kitaplığı nasıl kullanmaya çalıştığınız ile ilgili bir sorun olduğunda oluşturulur.

## <a name="java"></a>[Java](#tab/java)

Java için MSAL içinde üç özel durum türü vardır: `MsalClientException` , `MsalServiceException` , ve `MsalInteractionRequiredException` ; Tümü öğesinden devralınır `MsalException` .

- `MsalClientException`, kitaplıkta veya cihazda yerel olan bir hata oluştuğunda oluşturulur.
- `MsalServiceException`, güvenli belirteç hizmeti (STS) bir hata yanıtı döndürdüğünde veya başka bir ağ hatası oluştuğunda oluşturulur.
- `MsalInteractionRequiredException`, kimlik doğrulamasının başarılı olması için Kullanıcı arabirimi etkileşimi gerektiğinde oluşturulur.

### <a name="msalserviceexception"></a>MsalServiceException

`MsalServiceException`STS 'ye isteklerde döndürülen HTTP üstbilgilerini gösterir. Kullanarak bunlara erişin`MsalServiceException.headers()`

### <a name="msalinteractionrequiredexception"></a>Msalınteractionrequiredexception

Çağrılırken, Java için MSAL tarafından döndürülen yaygın durum kodlarından biridir `AcquireTokenSilently()` `InvalidGrantError` . Bu, bir kimlik doğrulama belirtecinin verilebilmesi için ek kullanıcı etkileşiminin gerekli olduğu anlamına gelir. Uygulamanız, kimlik doğrulama kitaplığını yeniden çağırmalıdır, ancak `AuthorizationCodeParameters` `DeviceCodeParameters` genel istemci uygulamaları göndererek veya için etkileşimli modda.

Çoğu zaman `AcquireTokenSilently` başarısız olduğunda, bunun nedeni belirteç önbelleğinde isteğinizle eşleşen bir belirteç yoktur. Erişim belirteçlerinin bir saatte bir süre sonu vardır ve `AcquireTokenSilently` yenileme belirtecine göre yeni bir tane almaya çalışır. OAuth2 koşullarında, bu yenileme belirteci akışdır. Bu akış, kiracı yöneticisinin daha sıkı oturum açma ilkelerini yapılandırdığında olduğu gibi çeşitli nedenlerle de başarısız olabilir.

Bu hatayla sonuçlanan bazı koşullar, kullanıcıların çözebilmesi için kolaydır. Örneğin, kullanım koşullarını kabul etmesi gerekebilir. Ya da makinenin belirli bir kurumsal ağa bağlanması gerektiğinden istek geçerli yapılandırmayla yerine getirilemiyor.

MSAL `reason` , daha iyi bir kullanıcı deneyimi sağlamak için kullanabileceğiniz bir alan sunar. Örneğin, `reason` alanı kullanıcıya parolasının dolduğunu veya bazı kaynakları kullanmak için onay sağlamaları gerektiğini söyleyecektir. Desteklenen değerler numaralandırmanın bir parçasıdır `InteractionRequiredExceptionReason` :

| Nedeni | Anlamı | Önerilen Işleme |
|---------|-----------|-----------------------------|
| `BasicAction` | Durum etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir | `acquireToken`Etkileşimli parametrelerle çağrı |
| `AdditionalAction` | Koşul, etkileşimli kimlik doğrulama akışı dışındaki sistemle ek düzeltme etkileşimi tarafından çözülebilir. | `acquireToken`Gerçekleştirilecek düzeltici eylemi açıklayan bir ileti göstermek için etkileşimli parametrelerle çağırın. Çağıran uygulama, Kullanıcı düzeltici eylemi tamamlayamamasının olası olması beklenmez, ek eylem gerektiren akışları gizlemeyi tercih edebilir. |
| `MessageOnly` | Koşul şu anda çözümlenemiyor. Koşulu açıklayan bir ileti göstermek için etkileşimli kimlik doğrulama akışını başlatın. | `acquireToken`Koşulu açıklayan bir ileti göstermek için etkileşimli parametrelerle çağırın. `acquireToken``UserCanceled`Kullanıcı iletiyi okuduktan sonra hatayı döndürür ve pencereyi kapatır. Uygulama, kullanıcıdan iletiden daha fazla avantaj sağlamaması durumunda ileti ile sonuçlanan akışları gizlemeyi tercih edebilir. |
| `ConsentRequired`| Kullanıcı onayı eksik veya iptal edildi. |`acquireToken`Kullanıcının izin vermesi için etkileşimli parametrelerle çağırın. |
| `UserPasswordExpired` | Kullanıcının parolasının süresi doldu. | `acquireToken`Kullanıcının parolasını sıfırlayabilmesi için etkileşimli parametreyle çağırın |
| `None` |  Daha fazla ayrıntı sağlanır. Bu koşul etkileşimli kimlik doğrulama akışı sırasında Kullanıcı etkileşimi tarafından çözülebilir. | `acquireToken`Etkileşimli parametrelerle çağrı |

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

İOS ve macOS hatalarının tüm MSAL listesi [Msalerror numaralandırması](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALError.h#L128)bölümünde listelenmiştir.

MSAL tarafından oluşturulan tüm hatalar `MSALErrorDomain` etki alanı ile döndürülür.

Sistem hataları için, MSAL `NSError` SISTEM API 'sinden orijinali döndürür. Örneğin, bir ağ bağlantısı olmaması nedeniyle belirteç alımı başarısız olursa, MSAL `NSURLErrorDomain` etki alanı ve kodla bir hata döndürür `NSURLErrorNotConnectedToInternet` .

İstemci tarafında en az aşağıdaki iki MSAL hatasını işlemenizi öneririz:

- `MSALErrorInteractionRequired`: Kullanıcının etkileşimli bir istek yapması gerekir. Bu hataya, zaman aşımına uğradı bir kimlik doğrulama oturumu veya ek kimlik doğrulama gereksinimleri gereksinimi gibi pek çok koşul neden olabilir. Kurtarmak için MSAL Interactive Token alma API 'sini çağırın. 

- `MSALErrorServerDeclinedScopes`: Bazı kapsamlar veya tüm kapsamlar reddedildi. Yalnızca verilen kapsamlarla devam edilip edilmeyeceğini veya oturum açma sürecini durdurmayı belirleyin.

> [!NOTE]
> `MSALInternalError`Enum yalnızca başvuru ve hata ayıklama için kullanılmalıdır. Çalışma zamanında bu hataları otomatik olarak işlemeye çalışmayın. Uygulamanız, altında kalan hatalardan biriyle karşılaşırsa `MSALInternalError` , ne olduğunu açıklayan genel bir kullanıcıya yönelik iletiyi göstermek isteyebilirsiniz.

Örneğin, `MSALInternalErrorBrokerResponseNotReceived` kullanıcının kimlik doğrulamasını tamamlamadığını ve uygulamaya el ile geri döndürmediği anlamına gelir. Bu durumda, uygulamanız kimlik doğrulamanın tamamlanmamış olduğunu açıklayan genel bir hata iletisi göstermelidir ve yeniden kimlik doğrulaması yapmayı denemeleri önerilir.

Aşağıdaki amaç-C örnek kodu, bazı genel hata koşullarını işlemek için en iyi yöntemleri göstermektedir:

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

## <a name="conditional-access-and-claims-challenges"></a>Koşullu erişim ve talep sorunları

Belirteçleri sessizce alırken, erişmeye çalıştığınız bir API 'nin MFA İlkesi gibi [koşullu erişim talep zorluğu](../azuread-dev/conditional-access-dev-guide.md) gerektiğinde uygulamanız hatalar alabilir.

Bu hatayı işleme deseninin, MSAL kullanarak etkileşimli bir şekilde belirteç edinmesi. Bir belirteci etkileşimli bir şekilde almak kullanıcıya sorar ve gerekli koşullu erişim ilkesini karşılamak için bu fırsata yanıt verir.

Belirli durumlarda, koşullu erişim gerektiren bir API 'yi çağırırken API 'den hata ile bir talep zorluğu alabilirsiniz. Örneğin, koşullu erişim ilkesinde yönetilen bir cihaz (Intune) varsa, hata AADSTS53000 gibi bir şey olacaktır [: cihazınızın bu kaynağa erişmek için yönetilmesi gerekir](reference-aadsts-error-codes.md) veya benzer bir şeydir. Bu durumda, kullanıcıdan uygun ilkeyi karşılaması istenir diye, talepleri alma çağrısında geçirebilirsiniz.

### <a name="net"></a>.NET

MSAL.NET 'ten koşullu erişim gerektiren bir API çağrılırken, uygulamanızın talep sınama özel durumlarını işlemesi gerekir. Bu, [talepler](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet) özelliğinin boş olacağı bir [Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) olarak görünür.

Talep sınamasını işlemek için, `.WithClaim()` sınıfının yöntemini kullanmanız gerekir `PublicClientApplicationBuilder` .

### <a name="javascript"></a>JavaScript

MSAL.js kullanarak belirteçleri sessizce (kullanarak `acquireTokenSilent` ) alırken, erişmeye çalıştığınız BIR API IÇIN MFA İlkesi gibi bir [koşullu erişim talep zorluğu](../azuread-dev/conditional-access-dev-guide.md) gerektiğinde uygulamanız hatalar alabilir.

Bu hatayı işleme deseninin, `acquireTokenPopup` Aşağıdaki örnekte olduğu gibi MSAL.js belirteç almak için etkileşimli bir çağrı yapılır `acquireTokenRedirect` :

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // call API
}).catch(function(error) {
    if (error instanceof InteractionRequiredAuthError) {
    
        // extract, if exists, claims from error message
        if (error.ErrorMessage.claims) {
            accessTokenRequest.claimsRequest = JSON.stringify(error.ErrorMessage.claims);
        }
        
        // call acquireTokenPopup in case of InteractionRequiredAuthError failure
        myMSALObj.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // call API
        }).catch(function(error) {
            console.log(error);
        });
    }
});
```

Belirteci etkileşimli olarak almak kullanıcıya sorar ve gerekli koşullu erişim ilkesini karşılamak için bu fırsata yanıt verir.

Koşullu erişim gerektiren bir API çağrılırken, API 'den hatada bir talep zorluğu alabilirsiniz. Bu durumda, hatada döndürülen talepleri, `claimsRequest` `AuthenticationParameters.ts` uygun ilkeyi karşılamak üzere sınıfının alanına geçirebilirsiniz. 

Daha fazla ayrıntı için bkz. [ek talepler isteme](active-directory-optional-claims.md) .

### <a name="msal-for-ios-and-macos"></a>iOS ve macOS için MSAL

İOS ve macOS için MSAL, hem etkileşimli hem de sessiz belirteç alma senaryolarında belirli talepler talep etmenizi sağlar.

Özel talepler istemek için `claimsRequest` veya ' de `MSALSilentTokenParameters` belirtin `MSALInteractiveTokenParameters` .

Daha fazla bilgi için bkz. [iOS IÇIN msal ve macOS kullanarak özel talepler isteme](request-custom-claims.md) .

## <a name="retrying-after-errors-and-exceptions"></a>Hatalar ve özel durumlar sonrasında yeniden deneniyor

MSAL çağrılırken size kendi yeniden deneme ilkelerinizi uygulamanız bekleniyor. MSAL AAD hizmetine HTTP çağrıları yapar ve zaman zaman meydana gelebilir, örneğin ağ kapatılabilir veya sunucu aşırı yüklenmiş olabilir.  

### <a name="http-error-codes-500-600"></a>HTTP hata kodları 500-600

MSAL.NET, HTTP hata kodları 500-600 ile hatalar için basit bir yeniden deneme mekanizması uygular.

### <a name="http-429"></a>HTTP 429

Hizmet belirteci sunucusu (STS) çok fazla istek ile aşırı yüklendiğinde, yanıt alanında yeniden deneyebilmeniz için ne kadar süreyle bir ipucu ile HTTP hatası 429 döndürür `Retry-After` .

### <a name="net"></a>.NET

Bir özellik olarak [Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) yüzeyleri `System.Net.Http.Headers.HttpResponseHeaders` `namedHeaders` . Uygulamalarınızın güvenilirliğini artırmak için hata kodundaki ek bilgileri kullanabilirsiniz. Açıklanan örnekte, `RetryAfterproperty` (türü) öğesini kullanabilir `RetryConditionHeaderValue` ve yeniden deneneceği zaman işlem yapabilirsiniz.

İstemci kimlik bilgileri akışını kullanan bir Daemon uygulaması için bir örnek aşağıda verilmiştir. Bunu, belirteç alma yöntemlerinden herhangi birine uyarlayabilirsiniz.

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
