---
title: ADAL istemci uygulaması hata işleme en iyi uygulamaları | Azure
description: ADAL istemci uygulamaları için hata işleme kılavuzu ve en iyi uygulamaları sağlar.
services: active-directory
author: rwike77
manager: CelesteDG
ms.author: ryanwi
ms.service: active-directory
ms.subservice: azuread-dev
ms.custom: aaddev
ms.topic: conceptual
ms.workload: identity
ms.date: 02/27/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 9fc45ead65a29f2e7567133b5af4667bdb7c79ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154993"
---
# <a name="error-handling-best-practices-for-azure-active-directory-authentication-library-adal-clients"></a>Azure Etkin Dizin Kimlik Doğrulama Kitaplığı (ADAL) istemcileri için en iyi uygulamaları hata işleme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Bu makalede, kullanıcıların kimliğini doğrulamak için ADAL kullanırken geliştiricilerin karşılaşabileceği hata türü hakkında kılavuz lar sağlanmaktadır. ADAL kullanırken, bir geliştiricinin devreye girip hataları işlemesi gerekebileceği birkaç durum vardır. Uygun hata işleme harika bir son kullanıcı deneyimi sağlar ve son kullanıcının oturum açma sayısını sınırlar.

Bu makalede, ADAL tarafından desteklenen her platform için belirli durumları ve uygulamanızın her servis talebiyle nasıl düzgün şekilde işleyebileceğini inceliyoruz. Hata kılavuzu, ADAL API'leri tarafından sağlanan belirteç edinim desenleri temel alınarak iki daha geniş kategoriye ayrılır:

- **AcquireTokenSilent**: İstemci sessizce bir belirteç almaya çalışır (UI yok) ve ADAL başarısız olursa başarısız olabilir. 
- **Edinme Token**: İstemci sessiz satın alma girişiminde bulunabilir, ancak oturum açma gerektiren etkileşimli istekleri de gerçekleştirebilir.

> [!TIP]
> ADAL ve Azure AD kullanırken tüm hataları ve özel durumları günlüğe kaydetmek iyi bir fikirdir. Günlükler yalnızca uygulamanızın genel durumunu anlamak için yararlı olmakla kalmıyor, aynı zamanda daha geniş sorunları hata ayıklarken de önemlidir. Uygulamanız belirli hatalardan kurtulabilir, ancak bunlar çözmek için kod değişiklikleri gerektiren daha geniş tasarım sorunlarına işaret edebilir. 
> 
> Bu belgede kapsanan hata koşullarını uygularken, daha önce açıklanan nedenlerle hata kodunu ve açıklamasını günlüğe kaydetmeniz gerekir. Günlük kodu örnekleri için [Hata ve günlük başvurusuna](#error-and-logging-reference) bakın. 
>

## <a name="acquiretokensilent"></a>Satın AlmaTokenSilent

AcquireTokenSilent, son kullanıcının Kullanıcı Arabirimi (UI) görmediğine dair garanti veren bir belirteç almaya çalışır. Sessiz edinmenin başarısız olabileceği ve etkileşimli istekler veya varsayılan bir işleyici tarafından ele alınması gereken birkaç durum vardır. Takip eden bölümlerde her vakanın ne zaman ve nasıl işledileceklerine dair ayrıntılara dalıyoruz.

İşletim sistemi tarafından oluşturulan ve uygulamaya özgü hata işleme gerektiren bir dizi hata vardır. Daha fazla bilgi için hata [ve günlük başvurusu](#error-and-logging-reference)"İşletim Sistemi" hataları bölümüne bakın. 

### <a name="application-scenarios"></a>Uygulama senaryoları

- [Yerel istemci](../develop/developer-glossary.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json#native-client) uygulamaları (iOS, Android, .NET Masaüstü veya Xamarin)
- [Kaynak](../develop/developer-glossary.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json#resource-server) çağıran [Web istemcisi](../develop/developer-glossary.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json#web-client) uygulamaları (.NET)

### <a name="error-cases-and-actionable-steps"></a>Hata örnekleri ve işlem uygulanabilir adımlar

Temelde, AcquireTokenSilent hataları iki durum vardır:

| Case (Olay) | Açıklama |
|------|-------------|
| **Büyük/Küçük Harf 1**: Hata etkileşimli oturum açma ile çözülebilir | Geçerli belirteçlerin eksikliğinden kaynaklanan hatalar için etkileşimli bir istek gereklidir. Özellikle, önbellek araması ve geçersiz/süresi dolmuş bir yenileme belirteci çözmek için bir EdinmeToken araması gerektirir.<br><br>Bu gibi durumlarda, son kullanıcının oturum açması istenir. Uygulama, son kullanıcı etkileşiminden (oturum açma düğmesine basmak gibi) sonra veya daha sonra etkileşimli bir istek yapmayı seçebilir. Seçim, uygulamanın istenen davranışına bağlıdır.<br><br>Bu özel durum ve tanılayan hatalar için aşağıdaki bölümdeki kodu görün.|
| **Örnek 2**: Hata etkileşimli oturum açma ile çözülemez | Ağ ve geçici/geçici hatalar veya diğer hatalar için etkileşimli Bir Edinme Token isteği gerçekleştirmek sorunu çözmez. Gereksiz etkileşimli oturum açma istemleri de son kullanıcıları hayal kırıklığına uğratabilir. ADAL, AcquireTokenSilent hatalarından en çok hata için otomatik olarak tek bir yeniden deneme dener.<br><br>İstemci uygulaması daha sonraki bir noktada yeniden denemeyi de deneyebilir, ancak uygulama davranışına ve istenen son kullanıcı deneyimine ne zaman ve nasıl bağlıdır. Örneğin, uygulama birkaç dakika sonra veya bazı son kullanıcı eylemine yanıt olarak Bir EdinmeTokenSilent yeniden deneme yapabilir. Hemen yeniden deneme, uygulamanın daraltılmasıyla sonuçlanır ve denenmemelidir.<br><br>Aynı hatayla başarısız olan sonraki yeniden deneme, istemcinin hatayı çözmediği için AcquireToken'i kullanarak etkileşimli bir istekte bulunması gerektiği anlamına gelmez.<br><br>Bu özel durum ve tanılayan hatalar için aşağıdaki bölümdeki kodu görün. |

### <a name="net"></a>.NET

Aşağıdaki kılavuz, ADAL yöntemleri ile birlikte hata işleme örnekleri sağlar: 

- satın AlmaTokenSilentAsync(...)
- satın AlmaTokenSilentSync(...) 
- [deprecated] satın TokenSilent(...)
- [deprecated] satın TokenByRefreshToken(...) 

Kodunuz aşağıdaki gibi uygulanacaktır:

```csharp
try{
    AcquireTokenSilentAsync(…);
} 

catch (AdalSilentTokenAcquisitionException e) {
    // Exception: AdalSilentTokenAcquisitionException
    // Caused when there are no tokens in the cache or a required refresh failed. 

    // Action: Case 1, resolvable with an interactive request. 
} 

catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
} 
    
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET. 
    // e.ErrorCode contains the error code. 

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
    // Example Error: network_not_available, default case.
}
```

### <a name="android"></a>Android

Aşağıdaki kılavuz, ADAL yöntemleri ile birlikte hata işleme örnekleri sağlar: 

- satın AlmaTokenSilentSync(...)
- satın AlmaTokenSilentAsync(...)
- [deprecated] satın TokenSilent(...)

Kodunuz aşağıdaki gibi uygulanacaktır:

```java
// *Inside callback*
public void onError(Exception e) {

    if (e instanceof AuthenticationException) {
        // Exception: AdalException
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode().

        // Errors: ADALError.ERROR_SILENT_REQUEST,
        // ADALError.AUTH_REFRESH_FAILED_PROMPT_NOT_ALLOWED,
        // ADALError.INVALID_TOKEN_CACHE_ITEM
        // Description: Request failed due to no tokens in
        // cache or failed a required refresh. 

        // Action: Case 1, resolvable with an interactive request. 

        // Action: Case 2, not resolvable with an interactive request.
        // Attempt retry after a timed interval or user action.
        // Example Errors: default case,
        // DEVICE_CONNECTION_IS_NOT_AVAILABLE, 
        // BROKER_AUTHENTICATOR_ERROR_GETAUTHTOKEN,
    }
}
```

### <a name="ios"></a>iOS

Aşağıdaki kılavuz, ADAL yöntemleri ile birlikte hata işleme örnekleri sağlar: 

- satın almaTokenSilentWithResource(...)

Kodunuz aşağıdaki gibi uygulanacaktır:

```objc
[context acquireTokenSilentWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL Objective-C.
            // Error Code: result.error.code

            // Errors: AD_ERROR_SERVER_REFRESH_TOKEN_REJECTED, AD_ERROR_CACHE_NO_REFRESH_TOKEN
            // Description: No tokens in cache or failed a required token refresh failed. 
            // Action: Case 1, resolvable with an interactive request. 

            // Error: AD_ERROR_CACHE_MULTIPLE_USERS
            // Description: There was ambiguity in the silent request resulting in multiple cache items.
            // Action: Special Case, application should perform another silent request and specify the user using ADUserIdentifier. 
            // Can be caused in cases of a multi-user application. 

            // Action: Case 2, not resolvable with an interactive request.
            // Attempt retry after some time or user action.
            // Example Errors: default case,
            // AD_ERROR_CACHE_BAD_FORMAT
        }
    }
}]
```

## <a name="acquiretoken"></a>Satın AlmaToken

EdinmeToken belirteçleri almak için kullanılan varsayılan ADAL yöntemidir. Kullanıcı kimliğinin gerekli olduğu durumlarda, AcquireToken önce sessizce bir belirteç almaya çalışır, sonra gerekirse Kullanıcı-ı UI görüntüler (PromptBehavior.Never geçirilmediği sürece). Uygulama kimliğinin gerekli olduğu durumlarda, AcquireToken bir belirteç almaya çalışır, ancak son kullanıcı olmadığı için Kullanıcı Aracı'nı göstermez. 

EdinmeToken hatalarını işlerken, hata işleme platformuna ve uygulamanın başarmaya çalıştığı senaryoya bağlıdır. 

İşletim sistemi, belirli bir uygulamaya bağlı hata işleme gerektiren bir dizi hata da oluşturabilir. Daha fazla bilgi için Hata ve [günlüğe kaydetme başvurusunda](#error-and-logging-reference)"İşletim Sistemi hataları" bilgisine bakın. 

### <a name="application-scenarios"></a>Uygulama senaryoları

- Yerel istemci uygulamaları (iOS, Android, .NET Masaüstü veya Xamarin)
- Kaynak API (.NET) olarak adlandırdığı web uygulamaları
- Tek sayfauygulamaları (JavaScript)
- Servise Servis uygulamaları (.NET, Java)
  - Adına-of-dahil olmak üzere tüm senaryolar,
  - Belirli senaryoların Adına

### <a name="error-cases-and-actionable-steps-native-client-applications"></a>Hata örnekleri ve işlem yapılabilir adımlar: Yerel istemci uygulamaları

Yerel bir istemci uygulaması oluşturuyorsanız, ağ sorunları, geçici hatalar ve platforma özgü diğer hatalarla ilgili göz önünde bulundurulması gereken birkaç hata işleme durumu vardır. Çoğu durumda, bir uygulama hemen yeniden deneme yapmamalıdır, ancak oturum açmayı gerektiren son kullanıcı etkileşimini beklemeli. 

Tek bir yeniden denemenin sorunu çözebileceği birkaç özel durum vardır. Örneğin, bir kullanıcının bir aygıttaki verileri etkinleştirmesi gerektiğinde veya ilk hatadan sonra Azure AD aracısını karşıdan yüklemeyi tamamladığınızda. 

Hata durumlarında, bir uygulama son kullanıcının yeniden deneme gerektiren bazı etkileşimleri gerçekleştirmesine izin vermek için Kullanıcı Arabirimi sunabilir. Örneğin, aygıt çevrimdışı bir hata için başarısız olduysa, hatayı hemen yeniden denemek yerine Bir Edinme Token yeniden denemesini isteyen bir "Yeniden Oturum Açmayı Deneyin" düğmesi. 

Yerel uygulamalarda hata işleme iki durumla tanımlanabilir:

|  |  |
|------|-------------|
| **Örnek 1**:<br>Yeniden DenemeyeN Hata (çoğu durumda) | 1. Hemen yeniden denemeye çalışmayın. Yeniden deneme gerektiren belirli bir hataya dayalı olarak son kullanıcı kullanıcı arabirimi kullanıcı arasını sunun (örneğin, "Yeniden Oturum Açmayı Deneyin" veya "Azure AD aracısı uygulamasını indirin"). |
| **Örnek 2**:<br>Yeniden Ölçülebilir Hata | 1. Son kullanıcı başarı yla sonuçlanan bir duruma girmiş olabileceğinden tek bir yeniden deneme gerçekleştirin.<br><br>2. Yeniden deneme başarısız olursa, yeniden deneme gerektiren belirli bir hataya bağlı olarak son kullanıcı kullanıcı kullanıcı arabirimi'ni sunun ("Yeniden oturum açmayı deneyin", "Azure AD aracı uygulamasını indirin" vb.) |

> [!IMPORTANT]
> Bir kullanıcı hesabı sessiz bir aramada ADAL'a aktarılır ve başarısız olursa, sonraki etkileşimli istek son kullanıcının farklı bir hesap kullanarak oturum açmasına olanak tanır. Bir kullanıcı hesabı kullanarak başarılı bir Edinme Token'den sonra, uygulamanın oturum açmış kullanıcının uygulamaların yerel kullanıcı nesnesi ile eşleştiğini doğrulaması gerekir. Uyuşmazlık bir özel durum oluşturmaz (Amaç C hariç), ancak kullanıcının kimlik doğrulama isteklerinden önce yerel olarak bilindiği durumlarda (başarısız sessiz arama gibi) göz önünde bulundurulmalıdır.
>

#### <a name="net"></a>.NET

Aşağıdaki kılavuz, sessiz olmayan tüm AcquireToken(...) ile birlikte hata işleme örnekleri sağlar. ADAL yöntemleri, *hariç:* 

- EdinmeTokenAsync(..., IClientAssertionCertification, ...)
- EdinmeTokenAsync(..., ClientCredential, ...)
- EdinmeTokenAsync(..., Müşteri İddiası, ...)
- EdinmeTokenAsync(..., UserAssertion,...)   

Kodunuz aşağıdaki gibi uygulanacaktır:

```csharp
try {
    AcquireTokenAsync(…);
} 
    
catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.
    
    // Design time consideration: Certain errors may be caused at development and exposed through this exception. 
    // Looking inside the description will give more guidance on resolving the specific issue. 

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

    } 

catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // e.ErrorCode contains the error code

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

> [!NOTE]
> ADAL .NET, AcquireTokenSilent gibi davranışları olan PromptBehavior.Never'i desteklediği için ekstra bir değerlendirmeye sahiptir.
>

Aşağıdaki kılavuz, ADAL yöntemleri ile birlikte hata işleme örnekleri sağlar: 

- edinmeToken(..., PromptBehavior.Never)

Kodunuz aşağıdaki gibi uygulanacaktır:

```csharp
    try {acquireToken(…, PromptBehavior.Never);
    } 

catch(AdalServiceException e) {
    // Exception: AdalServiceException represents 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

} catch (AdalException e) {
    // Error Code: e.ErrorCode == "user_interaction_required"
    // Description: user_interaction_required indicates the silent request failed 
    // in a way that's resolvable with an interactive request.
    // Action: Resolvable with an interactive request. 

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

#### <a name="android"></a>Android

Aşağıdaki kılavuz, sessiz olmayan tüm AcquireToken(...) ile birlikte hata işleme örnekleri sağlar. ADAL yöntemleri. 

Kodunuz aşağıdaki gibi uygulanacaktır:

```java
AcquireTokenAsync(…);

// *Inside callback*
public void onError(Exception e) {
    if (e instanceof AuthenticationException) {
        // Exception: AdalException 
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode();

        // Error: ADALError.BROKER_APP_INSTALLATION_STARTED
        // Description: Broker app not installed, user will be prompted to download the app. 

        // Action: Case 2, Retriable Error 
        // Perform a single retry. If that fails, only try again after user action. 

        // Action: Case 1, Non-Retriable 
        // Do not perform an immediate retry. Only retry after user action. 
        // Example Errors: default case, DEVICE_CONNECTION_IS_NOT_AVAILABLE
    }
}
```

#### <a name="ios"></a>iOS

Aşağıdaki kılavuz, sessiz olmayan tüm AcquireToken(...) ile birlikte hata işleme örnekleri sağlar. ADAL yöntemleri. 

Kodunuz aşağıdaki gibi uygulanacaktır:

```objc
[context acquireTokenWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL ObjC.
            // Error Code: result.error.code 

            // Error: AD_ERROR_SERVER_WRONG_USER
            // Description: App passed a user into ADAL and the end user signed in with a different account. 
            // Action: Case 1, Non-retriable (as is) and up to the application on how to handle this case. 
            // It can attempt a new request without specifying the user, or use UI to clarify the user account to sign in. 

            // Action: Case 1, Non-Retriable 
            // Do not perform an immediate retry. Only retry after user action. 
            // Example Errors: default case
        }
    }
}]
```

### <a name="error-cases-and-actionable-steps-web-applications-that-call-a-resource-api-net"></a>Hata durumları ve işlem yapılabilir adımlar: Kaynak API(.NET) olarak adlandıran Web uygulamaları

Bir .NET web uygulaması oluşturuyorsanız, aramalar bir kaynak için yetkilendirme kodu kullanarak bir belirteç alır, gereken tek kod genel servis talebi için varsayılan işleyicidir. 

Aşağıdaki kılavuz, ADAL yöntemleri ile birlikte hata işleme örnekleri sağlar: 

- EdinmeTokenByAuthorizationCodeAsync(...)

Kodunuz aşağıdaki gibi uygulanacaktır:

```csharp
try {
    AcquireTokenByAuthorizationCodeAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action or wait until much later. 
    // Example Errors: default case
}
```

### <a name="error-cases-and-actionable-steps-single-page-applications-adaljs"></a>Hata durumları ve işlem yapılabilir adımlar: Tek sayfalı uygulamalar (adal.js)

AcquireToken ile adal.js kullanarak tek sayfalık bir uygulama oluşturuyorsanız, hata işleme kodu tipik bir sessiz aramanınkine benzer. Özellikle adal.js, AcquireToken bir UI gösterir asla. 

Başarısız bir AcquireToken aşağıdaki durumlarda vardır:

|  |  |
|------|-------------|
| **Örnek 1**:<br>Etkileşimli bir istekle çözülebilir | 1. Giriş() başarısız olursa, hemen yeniden deneme yapmayın. Yalnızca kullanıcı eylemi yeniden denemeyi İstedikten sonra yeniden deneyin.|
| **Örnek 2**:<br>Etkileşimli bir istekle çözülemez değil. Hata yeniden denilebilir. | 1. Son kullanıcı büyük bir başarı ile sonuçlanan bir duruma girmiş gibi tek bir yeniden deneyin.<br><br>2. Yeniden deneme başarısız olursa, son kullanıcıya yeniden deneme gerektirebilecek belirli bir hataya dayalı bir eylem sunun ("Yeniden oturum açmayı deneyin"). |
| **Örnek 3**:<br>Etkileşimli bir istekle çözülemez değil. Hata yeniden deneebilir değildir. | 1. Hemen yeniden denemeye çalışmayın. Son kullanıcıya, yeniden deneme yi çağırabilecek belirli bir hataya dayalı bir eylem sunun ("Yeniden Oturum Açmayı Deneyin"). |

Kodunuz aşağıdaki gibi uygulanacaktır:

```javascript
AuthContext.acquireToken(…, function(error, errorDesc, token) {
    if (error || errorDesc) {
        // Represents any token acquisition failure that occurred. 
        // Error Code: error.indexOf("<ERROR_STRING>")

        // Errors: if (error.indexOf("interaction_required"))
        //         if (error.indexOf("login required"))
        // Description: ADAL wasn't able to silently acquire a token because of expire or fresh session. 
        // Action: Case 1, Resolvable with an interactive login() request. 

        // Error: if (error.indexOf("Token Renewal Failed")) 
        // Description: Timeout when refreshing the token.
        // Action: Case 2, Not resolvable interactively, error is retriable.
        // Perform a single retry. Only try again after user action.

        // Action: Case 3, Not resolvable interactively, error is not retriable. 
        // Do not perform an immediate retry. Only retry after user action.
        // Example Errors: default case
    }
}
```

### <a name="error-cases-and-actionable-steps-service-to-service-applications-net-only"></a>Hata durumları ve işlem yapılabilir adımlar: hizmete yönelik uygulamalar (yalnızca.NET)

Edinme Token'i kullanan bir servise hizmet uygulaması oluşturuyorsanız, kodunuzu işlemesi gereken birkaç önemli hata vardır. Hatanın tek yolu, hatayı arama uygulamasına geri döndürmek (servis talepleri adına) veya yeniden deneme stratejisi uygulamaktır. 

#### <a name="all-scenarios"></a>Tüm senaryolar

Adına dahil olmak üzere *tüm* hizmet-servis uygulama senaryoları için:

- Hemen yeniden denemeye çalışmayın. ADAL, belirli başarısız istekler için tek bir yeniden deneme dener. 
- Yalnızca bir kullanıcı veya uygulama eylemi yeniden deneme yistemledikten sonra yeniden denemeye devam edin. Örneğin, belirli bir aralıkta çalışan bir daemon uygulaması yeniden denemek için bir sonraki arayla beklemeniz gerekir.

Aşağıdaki kılavuz, ADAL yöntemleri ile birlikte hata işleme örnekleri sağlar: 

- EdinmeTokenAsync(..., IClientAssertionCertification, ...)
- EdinmeTokenAsync(...,ClientCredential, ...)
- EdinmeTokenAsync(...,Müşteri İddiası, ...)
- EdinmeTokenAsync(...,Kullanıcı İddiası, ...)

Kodunuz aşağıdaki gibi uygulanacaktır:

```csharp
try {
    AcquireTokenAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Errors: default case
}  
```

#### <a name="on-behalf-of-scenarios"></a>Senaryolar adına

Hizmete hizmet uygulama senaryoları *adına.*

Aşağıdaki kılavuz, ADAL yöntemleri ile birlikte hata işleme örnekleri sağlar: 

- EdinmeTokenAsync(..., UserAssertion, ...)

Kodunuz aşağıdaki gibi uygulanacaktır:

```csharp
try {
AcquireTokenAsync(…);
} 

catch (AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Error: On-Behalf-Of Error Handler
    if (e.ErrorCode == "interaction_required") {
        // Description: The client needs to perform some action due to a config from admin. 
        // Action: Capture `claims` parameter inside ex.InnerException.InnerException.
        // Generate HTTP error 403 with claims, throw back HTTP error to client.
        // Wait for client to retry. 
    }
} 
        
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Error: default case
}
```

Bu senaryoyu gösteren tam bir [örnek](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) yaptık.

## <a name="error-and-logging-reference"></a>Hata ve günlük başvurusu

### <a name="logging-personal-identifiable-information--organizational-identifiable-information"></a>Kişisel Tanımlanabilir Bilgileri & Kuruluş Tanımlı Bilgileri Kaydetme 
Varsayılan olarak, ADAL günlük yakalamaz veya herhangi bir kişisel tanımlanabilir bilgi veya kurumsal tanımlanabilir bilgi günlüğe kaydetmez. Kitaplık, uygulama geliştiricilerin logger sınıfındabir ayarlayıcı aracılığıyla bu açmak için izin verir. Uygulama, kişisel tanımlanabilir bilgileri veya kuruluş tarafından tanımlanabilir bilgileri günlüğe kaydederek, son derece hassas verileri güvenli bir şekilde kullanma ve tüm yasal gerekliliklere uyma sorumluluğunu üstleniyor.

### <a name="net"></a>.NET

#### <a name="adal-library-errors"></a>ADAL kitaplık hataları

Belirli ADAL hatalarını keşfetmek [için, azure-activedirectory-library-for-dotnet deposundaki](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/8f6d560fbede2247ec0e217a21f6929d4375dcaa/src/ADAL.PCL/Utilities/Constants.cs#L58) kaynak kodu en iyi hata başvurusudur.

#### <a name="guidance-for-error-logging-code"></a>Hata günlüğe kaydetme kodu kılavuzu

ADAL .NET günlük, üzerinde çalışılan platforma bağlı olarak değişir. Günlük günlüğe kaydetmeyi etkinleştirme hakkında kod için [Günlük wiki'sine](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Logging-in-ADAL.Net) bakın.

### <a name="android"></a>Android

#### <a name="adal-library-errors"></a>ADAL kitaplık hataları

Belirli ADAL hatalarını keşfetmek [için, azure-activedirectory-library-for-android deposundaki](https://github.com/AzureAD/azure-activedirectory-library-for-android/blob/dev/adal/src/main/java/com/microsoft/aad/adal/ADALError.java#L33) kaynak kodu en iyi hata başvurusudur.

#### <a name="operating-system-errors"></a>İşletim Sistemi hataları

Android işletim sistemi hataları ADAL AuthenticationException ile maruz kalır, "SERVER_INVALID_REQUEST" olarak tanımlanabilir ve hata açıklamaları ile daha ayrıntılı olabilir. 

Yaygın hataların tam listesi ve uygulamanız veya son kullanıcılar bunlarla karşılaştığında hangi adımları niçin atması gerektiğini n [için ADAL Android Wiki'ye](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki)bakın. 

#### <a name="guidance-for-error-logging-code"></a>Hata günlüğe kaydetme kodu kılavuzu

```java
// 1. Configure Logger
Logger.getInstance().setExternalLogger(new ILogger() {    
    @Override   
    public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) { 
    // …
    // You can write this to logfile depending on level or errorcode. 
    writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);    
    }
}

// 2. Set the log level
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

// By default, the `Logger` does not capture any PII or OII

// PII or OII will be logged
Logger.getInstance().setEnablePII(true);

// To STOP logging PII or OII, use the following setter
Logger.getInstance().setEnablePII(false);


// 3. Send logs to logcat.
adb logcat > "C:\logmsg\logfile.txt";
```

### <a name="ios"></a>iOS

#### <a name="adal-library-errors"></a>ADAL kitaplık hataları

Belirli ADAL hatalarını keşfetmek için [azure-activedirectory-library-for-objc deposundaki](https://github.com/AzureAD/azure-activedirectory-library-for-objc/blob/dev/ADAL/src/ADAuthenticationError.m#L295) kaynak kodu en iyi hata başvurusudur.

#### <a name="operating-system-errors"></a>İşletim sistemi hataları

oturum açma sırasında kullanıcılar web görünümlerini ve kimlik doğrulamanın doğasını kullanan iOS hataları ortaya çıkabilir. Bunun nedeni SSL hataları, zaman ekmeleri veya ağ hataları gibi koşullar olabilir:

- Yetkilendirme Paylaşımı için oturum açmakalıcı değildir ve önbellek boş görünür. Anahtarlığa şeni şekene şeni ekleyerek çözüm`[[ADAuthenticationSettings sharedInstance] setSharedCacheKeychainGroup:nil];`
- NsUrlDomain hata kümesi için eylem uygulama mantığına bağlı olarak değişir. Ele alınabilecek belirli örnekler için [NSURLErrorDomain başvuru belgelerine](https://developer.apple.com/documentation/foundation/nsurlerrordomain#declarations) bakın.
- ADAL Objective-C ekibi tarafından tutulan yaygın hatalar listesi için [ADAL Obj-C Sık Karşılaşılan Sorunlar](https://github.com/AzureAD/azure-activedirectory-library-for-objc#adauthenticationerror) bölümüne bakın.

#### <a name="guidance-for-error-logging-code"></a>Hata günlüğe kaydetme kodu kılavuzu

```objc
// 1. Enable NSLogging
[ADLogger setNSLogging:YES];

// 2. Set the log level (if you want verbose)
[ADLogger setLevel:ADAL_LOG_LEVEL_VERBOSE];

// 3. Set up a callback block to simply print out
[ADLogger setLogCallBack:^(ADAL_LOG_LEVEL logLevel, NSString *message, NSString *additionalInformation, NSInteger errorCode, NSDictionary *userInfo) {
     NSString* log = [NSString stringWithFormat:@"%@ %@", message, additionalInformation];    
     NSLog(@"%@", log);
}];
```

### <a name="guidance-for-error-logging-code---javascript"></a>Hata günlüğü kodu için kılavuz - JavaScript 

```javascript
0: Error1: Warning2: Info3: Verbose
window.Logging = {
    level: 3,
    log: function (message) {
        console.log(message);
    }
};
```

## <a name="related-content"></a>İlgili içerik

* [Azure AD Kimlik Doğrulama Kitaplıkları][AAD-Auth-Libraries]
* [Azure AD Kimlik Doğrulama Senaryoları][AAD-Auth-Scenarios]
* [Uygulamaları Azure Etkin Dizini ile Tümleştirme][AAD-Integrating-Apps]

Geri bildirim sağlamak ve içeriğimizi geliştirmemize ve şekillendirmemize yardımcı olmak için aşağıdaki yorumlar bölümünü kullanın.

[!["Microsoft ile oturum aç" düğmesini gösterir][AAD-Sign-In]][AAD-Sign-In]
<!--Reference style links -->

[AAD-Auth-Libraries]: ./active-directory-authentication-libraries.md
[AAD-Auth-Scenarios]:v1-authentication-scenarios.md
[AAD-Integrating-Apps]:../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json
[AZURE-portal]: https://portal.azure.com

<!--Image references-->
[AAD-Sign-In]:./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png

