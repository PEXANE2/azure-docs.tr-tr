---
title: Hatalar ve özel durumlar (MSAL) | Mavisi
description: MSAL uygulamalarında hataları ve özel durumları, koşullu erişimi ve talep sınamasını nasıl ele alabileceğinizi öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: c294e3bd8ac04454c2d715c665e0da4f9a4f4535
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835032"
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
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002 İstek gövdesi şu parametreyi içermelidir: ' client_secret veya client_assertion '.| Bu durum, uygulamanız Azure AD 'de ortak bir istemci uygulaması olarak kaydettirilmemişse meydana gelebilir. Azure Portal, uygulamanızın bildirimini düzenleyin ve öğesini `allowPublicClient` olarak `true`ayarlayın. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| unknown_user Iletisi: Oturum açmış kullanıcı tanımlanamadı| Kitaplık, geçerli Windows oturum açmış kullanıcıyı sorgulayamadı veya bu kullanıcı AD veya AAD 'ye katılmış değil (iş yeri katılmış kullanıcıları desteklenmez). Hafifletme 1: UWP 'de, uygulamanın aşağıdaki yeteneklere sahip olup olmadığını denetleyin: Kurumsal kimlik doğrulaması, özel ağlar (Istemci ve sunucu), Kullanıcı hesabı bilgileri. Risk azaltma 2: Kullanıcı adını (örneğin, john@contoso.com) getirmek için kendi mantığınızı uygulayın ve Kullanıcı adında alan `AcquireTokenByIntegratedWindowsAuth` formu kullanın.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Bu yöntem, Active Directory (AD) tarafından sunulan bir protokolü kullanır. Kullanıcı, AD ("yönetilen" Kullanıcı) olmadan Azure Active Directory oluşturulduysa, bu yöntem başarısız olur. AD 'de oluşturulan ve AAD tarafından desteklenen kullanıcılar ("federe" kullanıcılar), etkileşimli olmayan bu kimlik doğrulama yönteminden yararlanabilir. Mayı Etkileşimli kimlik doğrulaması kullanın.|

## <a name="javascript-errors"></a>JavaScript hataları

MSAL. js, farklı türlerde yaygın hataların soyut ve sınıflandırılacağı hata nesneleri sağlar ve bunları uygun şekilde işlemek üzere hata iletileri gibi hataların belirli ayrıntılarına erişmek için bir arabirime sahiptir.

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
* **Kimlik doğrulama hatası. ileti:** Bu, errorMessage ile aynıdır.
* **AuthError. Stack:** Oluşan hatalar için yığın izlemesi. Kaynak hata noktasına izlemenin yapılmasına izin verir.

**Hata türleri**

Aşağıdaki hata türleri kullanılabilir:

* *Kimlik doğrulama hatası:* MSAL. js kitaplığı için temel hata sınıfı, ayrıca beklenmeyen hatalar için de kullanılır.

* *ClientAuthError:* Istemci kimlik doğrulamasıyla ilgili bir sorunu belirten hata sınıfı. Kitaplıktan gelen hataların çoğu ClientAuthErrors olur. Bu işlemler, oturum açma işlemi devam ederken bir oturum açma yöntemi çağırma, Kullanıcı oturum açmayı iptal etme gibi hatalar olabilir.

* *ClientConfigurationError:* Belirtilen kullanıcı yapılandırma parametreleri hatalı biçimlendirilmiş veya eksik olduğunda istekler yapılmadan önce ClientAuthError 'ı genişleten hata sınıfı oluştu.

* *ServerError* Kimlik doğrulama sunucusu tarafından gönderilen hata dizelerini temsil eden hata sınıfı. Bunlar geçersiz istek biçimleri veya parametreler gibi hatalar ya da sunucunun kimlik doğrulamasını veya yetkilendirmasını engelleyen başka hatalar olabilir.

* *Interactionrequiredadutherror:* Etkileşimli çağrı gerektiren sunucu hatalarını göstermek için ServerError 'ı genişleten hata sınıfı. Bu, kullanıcının kimlik `acquireTokenSilent` doğrulaması/yetkilendirme için kimlik bilgileri veya onay sağlamak üzere sunucuyla etkileşimde bulunmak için gerekliyse oluşur. Hata kodları "interaction_required", "login_required", "consent_required" içerir.

Yeniden yönlendirme Yöntemleri (`loginRedirect`, `acquireTokenRedirect`) ile kimlik doğrulaması akışlarında hata işleme için, aşağıdaki gibi yeniden yönlendirme yöntemi kullanılarak `handleRedirectCallback()` , başarılı veya başarısız olarak çağrılan geri çağırma işlemini kaydetmeniz gerekir:

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

Kullanıcı arabirimi etkileşimi gerektiğinde bir hata döndürülür. Bu, bir belirteci almak için etkileşimli olmayan bir Yöntem (örneğin, `acquireTokenSilent`) kullanmayı denediği, ancak msal sessizce yapamayacağı anlamına gelir. Olası nedenler şunlardır:

* oturum açmanız gerekiyor
* onay yapmanız gerekir
* Multi-Factor Authentication deneyiminden gitmeniz gerekir.

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
    // call acquireTokenPopup in case of acquireTokenSilent failure
    myMSALObj.acquireTokenPopup(accessTokenRequest).then(
        function (accessTokenResponse) {
            // call API
        }).catch(function (error) {
            console.log(error);
        });
});
```

Belirteci etkileşimli olarak almak kullanıcıya sorar ve gerekli koşullu erişim ilkesini karşılamak için bu fırsata yanıt verir.

Koşullu erişim gerektiren bir API çağrılırken, API 'den hatada bir talep zorluğu alabilirsiniz. Bu durumda, hata `extraQueryParameters` olarak döndürülen talepleri, kullanıcıdan uygun ilkeyi karşılaması istenecektir şekilde belirteçleri alma çağrısında geçirebilirsiniz:

```javascript
var request = {
    scopes: ["user.read"],
    extraQueryParameters: {claims: claims}
}

myMSALObj.acquireTokenPopup(request);
```

## <a name="retrying-after-errors-and-exceptions"></a>Hatalar ve özel durumlar sonrasında yeniden deneniyor

### <a name="http-error-codes-500-600"></a>HTTP hata kodları 500-600
MSAL.NET, HTTP hata kodları 500-600 ile hatalar için basit bir yeniden deneme mekanizması uygular.

### <a name="http-429"></a>HTTP 429
"Çok fazla istek" nedeniyle hizmet belirteci sunucusu (STS) çok meşgul olduğunda, bir veya daha fazla gecikme veya bir tarih olarak yeniden deneyebilmeniz (yeniden deneme yanıt alanı) için bir ipucu içeren bir HTTP hatası 429 döndürür.

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
