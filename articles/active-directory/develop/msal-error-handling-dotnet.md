---
title: MSAL.NET’te hataları ve özel durumları işleme
titleSuffix: Microsoft identity platform
description: Hataları ve özel durumları, koşullu erişim talep sorunlarını ve MSAL.NET içinde yeniden denemeleri nasıl ele alabileceğinizi öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2020
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 565acd745ba5d7fdec71f306d3851e599838f7d9
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99584053"
---
# <a name="handle-errors-and-exceptions-in-msalnet"></a>MSAL.NET’te hataları ve özel durumları işleme

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msalnet"></a>MSAL.NET 'de hata işleme

.NET özel durumlarını işlerken özel `ErrorCode` durumları ayırt etmek için özel durum türünü ve üyesini kullanabilirsiniz. `ErrorCode` değerler [Msalerror](/dotnet/api/microsoft.identity.client.msalerror)türünde sabitler.

Ayrıca, [Msalclientexception](/dotnet/api/microsoft.identity.client.msalexception), [msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception)ve [msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception)alanlarına bakabilirsiniz.

[Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception) oluşturulursa, kodun orada listelenip listelenmediğini görmek Için [kimlik doğrulama ve yetkilendirme hata kodlarını](reference-aadsts-error-codes.md) deneyin.

### <a name="common-net-exceptions"></a>Ortak .NET özel durumları

Aşağıda, ortaya çıkabilecek ve bazı olası azaltmalar bulunan yaygın özel durumlar verilmiştir:  

| Özel durum | Hata kodu | Risk azaltma|
| --- | --- | --- |
| [Msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception) | AADSTS65001: Kullanıcı veya yönetici ' {appName} ' adlı ' {AppID} ' KIMLIĞIYLE uygulamayı kullanmak üzere onay vermedi. Bu Kullanıcı ve kaynak için etkileşimli bir yetkilendirme isteği gönderin.| Önce kullanıcı onayı alın. .NET Core kullanmıyorsanız (herhangi bir Web Kullanıcı arabirimine sahip olmayan), çağırın (yalnızca bir kez) `AcquireTokeninteractive` . .NET Core kullanıyorsanız veya bir yapmak istemiyorsanız `AcquireTokenInteractive` , Kullanıcı onay vermek için BIR URL 'ye gidebilir: `https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read` . Şunu çağırmak için `AcquireTokenInteractive` : `app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [Msaluırequiredexception](/dotnet/api/microsoft.identity.client.msaluirequiredexception) | AADSTS50079: kullanıcının [Multi-Factor Authentication 'ı (MFA)](../authentication/concept-mfa-howitworks.md)kullanması gerekir.| Risk azaltma yoktur. Kiracınız için MFA yapılandırıldıysa ve Azure Active Directory (AAD) uygulamayı zorlamaya karar verirse, veya gibi etkileşimli bir akışa geri dönün `AcquireTokenInteractive` `AcquireTokenByDeviceCode` .|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception) |AADSTS90010: izin türü *sık karşılaşılan* veya */tüketiciler* uç noktaları üzerinden desteklenmiyor. */Kuruluşlar* veya kiracıya özgü uç noktayı kullanın. *Sık karşılaşılan* kullandınız.| Azure AD 'nin iletisinde açıklandığı gibi, yetkilinin bir kiracıya ya da aksi takdirde */kuruluşlara* sahip olması gerekir.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception) | AADSTS70002: istek gövdesi şu parametreyi içermelidir: `client_secret or client_assertion` .| Bu özel durum, uygulamanız Azure AD 'de ortak bir istemci uygulaması olarak kaydettirilmemişse oluşturulabilir. Azure portal, uygulamanızın bildirimini düzenleyin ve `allowPublicClient` olarak ayarlayın `true` . |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception)| `unknown_user Message`: Oturum açmış kullanıcı tanımlanamadı| Kitaplık, geçerli Windows oturum açmış kullanıcıyı sorgulayamadı veya bu kullanıcı AD veya Azure AD 'ye katılmış değil (iş yeri katılmış kullanıcıları desteklenmez). Hafifletme 1: UWP 'de, uygulamanın şu yeteneklere sahip olup olmadığını denetleyin: kurumsal kimlik doğrulaması, özel ağlar (Istemci ve sunucu), Kullanıcı hesabı bilgileri. Hafifletme 2: Kullanıcı adını (örneğin,) getirmek için kendi mantığınızı uygulayın john@contoso.com ve `AcquireTokenByIntegratedWindowsAuth` Kullanıcı adında alan formu kullanın.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception)|integrated_windows_auth_not_supported_managed_user| Bu yöntem, Active Directory (AD) tarafından sunulan bir protokolü kullanır. Bir Kullanıcı Azure AD 'de AD yedekleme ("yönetilen" Kullanıcı) olmadan oluşturulduysa, bu yöntem başarısız olur. AD 'de oluşturulan ve Azure AD tarafından desteklenen kullanıcılar ("federe" kullanıcılar), etkileşimli olmayan bu kimlik doğrulama yönteminden yararlanabilir. Risk azaltma: etkileşimli kimlik doğrulaması kullanın.|

### `MsalUiRequiredException`

Çağrılırken MSAL.NET tarafından döndürülen yaygın durum kodlarından biri `AcquireTokenSilent()` `MsalError.InvalidGrantError` . Bu durum kodu, uygulamanın kimlik doğrulama kitaplığını yeniden çağırması gerektiği anlamına gelir, ancak etkileşimli modda (genel istemci uygulamaları için Acquiretokenınteractive veya AcquireTokenByDeviceCodeFlow), Web uygulamalarında bir zorluk vardır. Bunun nedeni, kimlik doğrulama belirtecinin verilebilmesi için ek kullanıcı etkileşiminin gerekli olmasından kaynaklanır.

Çoğu zaman `AcquireTokenSilent` başarısız olduğunda, bunun nedeni belirteç önbelleğinde isteğinizle eşleşen belirteçleri yoktur. Erişim belirteçleri 1 saat içinde sona erer ve `AcquireTokenSilent` yenileme belirtecine göre yeni bir tane getirmeye çalışır (OAuth2 koşullarında, bu "belirteci Yenile ' akışındadır). Bu akış çeşitli nedenlerle da başarısız olabilir. Örneğin, bir kiracı yöneticisi daha sıkı oturum açma ilkeleri yapılandırır. 

Kullanıcının bir eylem yapması için etkileşim amaçlar. Bu koşullardan bazılarının, kullanıcıların çözebilmesi (örneğin, tek tıklamayla kullanım koşullarını kabul etmesi) ve bazıları geçerli yapılandırmayla çözümlenemez (örneğin, söz konusu makinenin belirli bir şirket ağına bağlanması gerekir). Kullanıcı, Multi-Factor Authentication 'ı ayarlamaya veya Microsoft Authenticator yüklemesine yardımcı olur.

### <a name="msaluirequiredexception-classification-enumeration"></a>`MsalUiRequiredException` sınıflandırma numaralandırması

MSAL `Classification` daha iyi bir kullanıcı deneyimi sağlamak üzere okuyabilmeniz için bir alan sunar. Örneğin, kullanıcının parolasının bittiğini veya bazı kaynakları kullanma onayı sağlaması gerektiğini bildirmek için. Desteklenen değerler numaralandırmanın bir parçasıdır `UiRequiredExceptionClassification` :

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
[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

MSAL.NET 'ten koşullu erişim gerektiren bir API çağrılırken, uygulamanızın talep sınama özel durumlarını işlemesi gerekir. Bu, [talepler](/dotnet/api/microsoft.identity.client.msalserviceexception.claims) özelliğinin boş olacağı bir [Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception) olarak görünür.

Talep sınamasını işlemek için, `.WithClaim()` sınıfının yöntemini kullanmanız gerekir `PublicClientApplicationBuilder` .

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

### <a name="http-error-codes-500-600"></a>HTTP hata kodları 500-600

MSAL.NET, HTTP hata kodları 500-600 ile hatalar için basit bir yeniden deneme mekanizması uygular.

Bir özellik olarak [Msalserviceexception](/dotnet/api/microsoft.identity.client.msalserviceexception) yüzeyleri `System.Net.Http.Headers.HttpResponseHeaders` `namedHeaders` . Uygulamalarınızın güvenilirliğini artırmak için hata kodundaki ek bilgileri kullanabilirsiniz. Açıklanan örnekte, `RetryAfterproperty` (türü) öğesini kullanabilir `RetryConditionHeaderValue` ve yeniden deneneceği zaman işlem yapabilirsiniz.

İstemci kimlik bilgileri akışını kullanan bir Daemon uygulaması için bir örnek aşağıda verilmiştir. Bunu, belirteç alma yöntemlerinden herhangi birine uyarlayabilirsiniz.

```csharp

bool retry = false;
do
{
    TimeSpan? delay;
    try
    {
         result = await publicClientApplication.AcquireTokenForClient(scopes, account).ExecuteAsync();
    }
    catch (MsalServiceException serviceException)
    {
         if (serviceException.ErrorCode == "temporarily_unavailable")
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
    // . . .
    if (delay.HasValue)
    {
        Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
        retry = true;
    }
} while (retry);
```

## <a name="next-steps"></a>Sonraki adımlar

Sorunları tanılamanıza ve hata ayıklamanıza yardımcı olmak için [msal.net 'de günlüğe kaydetmeyi](msal-logging-dotnet.md) etkinleştirmeyi düşünün.
