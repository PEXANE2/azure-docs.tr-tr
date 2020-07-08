---
title: MSAL.js istemci uygulamalarını Başlat | Mavisi
titleSuffix: Microsoft identity platform
description: JavaScript (MSAL.js) için Microsoft kimlik doğrulama kitaplığı 'nı kullanarak istemci uygulamalarını başlatma hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/12/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: fbd700c787a844fa7538ed198f76ed5c06af2c28
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81010163"
---
# <a name="initialize-client-applications-using-msaljs"></a>MSAL.js kullanarak istemci uygulamalarını başlatma
Bu makalede, Kullanıcı Aracısı uygulamasının bir örneğiyle JavaScript (MSAL.js) için Microsoft kimlik doğrulama Kitaplığı başlatma açıklanmaktadır. Kullanıcı Aracısı uygulaması, istemci kodunun web tarayıcısı gibi bir Kullanıcı aracısında yürütüldüğü bir ortak istemci uygulaması biçimidir. Tarayıcı bağlamı başarıyla erişilebilir olduğundan bu istemciler gizli dizileri depolamaz. İstemci uygulama türleri ve uygulama yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için [genel bakış](msal-client-applications.md)makalesini okuyun.

## <a name="prerequisites"></a>Ön koşullar
Bir uygulamayı başlatmadan önce, uygulamanızın Microsoft Identity platformu ile tümleştirilebilmesi için öncelikle [bu Azure Portal kaydetmeniz](scenario-spa-app-registration.md) gerekir. Kayıttan sonra, aşağıdaki bilgiler (Azure portal bulunabilir) gerekebilir:

- İstemci KIMLIĞI (uygulamanız için GUID 'ı temsil eden bir dize)
- Kimlik sağlayıcısı URL 'SI (örnek olarak adlandırılır) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre, her topluca yetkili olarak bilinir.
- Yalnızca kuruluşunuz için bir iş kolu uygulaması yazıyorsanız (tek kiracılı uygulama olarak da adlandırılır) kiracı KIMLIĞI.
- Web Apps için, kimlik sağlayıcısının güvenlik belirteçleriyle uygulamanıza geri dönebileceği redirectUri 'yi de ayarlamanız gerekir.

## <a name="initializing-applications"></a>Uygulamalar başlatılıyor

MSAL.js, düz bir JavaScript/TypeScript uygulamasında aşağıdaki gibi kullanabilirsiniz. Bir yapılandırma nesnesiyle örnekleyerek MSAL kimlik doğrulaması bağlamını başlatın `UserAgentApplication` . MSAL.js başlatmak için gereken en düşük yapılandırma, uygulamanızın kayıt portalından almanız gereken ClientID 'dir.

Yeniden yönlendirme akışları (ve) olan kimlik doğrulama yöntemleri için `loginRedirect` `acquireTokenRedirect` , MSAL.js 1.2. x veya önceki sürümlerde, yöntemi aracılığıyla başarılı veya hata için bir geri çağırma işlemini açık bir şekilde kaydetmeniz gerekecektir `handleRedirectCallback()` . Bu, yeniden yönlendirme akışları, bir açılır deneyimle birlikte olan yöntemler olarak geri dönmediğinden gereklidir. Bu, MSAL.js Version 1.3.0 içinde isteğe bağlı hale gelmiştir.

```javascript
// Configuration object constructed
const config = {
    auth: {
        clientId: "abcd-ef12-gh34-ikkl-ashdjhlhsdg"
    }
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(config);

function authCallback(error, response) {
    //handle redirect response
}

// (optional when using redirect methods) register redirect call back for Success or Error
myMSALObj.handleRedirectCallback(authCallback);
```

MSAL.js, `UserAgentApplication` tek bir kimlik doğrulama bağlamını temsil etmek için tek bir örneğe ve yapılandırmasına sahip olacak şekilde tasarlanmıştır. Birden çok örnek, tarayıcıda çakışan önbellek girişlerine ve davranışına neden olduğundan önerilmez.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

MSAL.js, örneği oluşturmak için kullanılabilen yapılandırılabilir seçenekler gruplandırması sağlayan aşağıda gösterilen bir yapılandırma nesnesine sahiptir `UserAgentApplication` .

```javascript
type storage = "localStorage" | "sessionStorage";

// Protocol Support
export type AuthOptions = {
    clientId: string;
    authority?: string;
    validateAuthority?: boolean;
    redirectUri?: string | (() => string);
    postLogoutRedirectUri?: string | (() => string);
    navigateToLoginRequestUrl?: boolean;
};

// Cache Support
export type CacheOptions = {
    cacheLocation?: CacheLocation;
    storeAuthStateInCookie?: boolean;
};

// Library support
export type SystemOptions = {
    logger?: Logger;
    loadFrameTimeout?: number;
    tokenRenewalOffsetSeconds?: number;
    navigateFrameWait?: number;
};

// Developer App Environment Support
export type FrameworkOptions = {
    isAngular?: boolean;
    unprotectedResources?: Array<string>;
    protectedResourceMap?: Map<string, Array<string>>;
};

// Configuration Object
export type Configuration = {
    auth: AuthOptions,
    cache?: CacheOptions,
    system?: SystemOptions,
    framework?: FrameworkOptions
};
```

Şu anda yapılandırma nesnesinde desteklenen yapılandırılabilir seçeneklerin toplam kümesi aşağıda verilmiştir:

- **ClientID**: gerekli. Uygulamanızın ClientID 'si, bunu uygulama kayıt portalından almalısınız.

- **yetkili**: isteğe bağlı. MSAL 'in belirteçleri isteyebileceğini bir dizin gösteren URL. Varsayılan değer: `https://login.microsoftonline.com/common` .
    * Azure AD 'de, örneğin &lt; &gt; / &lt; &gt; &lt; &gt; kimlik sağlayıcısı etki alanı (örneğin, `https://login.microsoftonline.com` ) ve &lt; seyirci, &gt; oturum açma kitini temsil eden bir tanımlayıcı olan https://örnek hedef kitlesi. Bu, aşağıdaki değerler olabilir:
        * `https://login.microsoftonline.com/<tenant>`-kiracı, contoso.onmicrosoft.com gibi kiracıyla ilişkili bir etki alanı veya `TenantID` yalnızca belirli bir kuruluşun kullanıcılarına oturum açmak için kullanılan dizinin özelliğini temsil eden GUID 'dir.
        * `https://login.microsoftonline.com/common`-Kullanıcıları iş ve okul hesaplarıyla veya bir Microsoft kişisel hesabıyla oturum açmak için kullanılır.
        * `https://login.microsoftonline.com/organizations/`-Kullanıcıları iş ve okul hesaplarıyla oturum açmak için kullanılır.
        * `https://login.microsoftonline.com/consumers/`-Kullanıcılara yalnızca kişisel Microsoft hesabı (canlı) oturum açmak için kullanılır.
    * Azure AD B2C, `https://<instance>/tfp/<tenant>/<policyName>/` burada örnek, Azure AD B2C Örneğin, örneğin, "Kiracı-adı}. b2clogin. com", kiracı Azure AD B2C kiracının adıdır, örneğin, {kiracınız}. onmicrosoft. com, policyName, uygulanacak B2C ilkesinin adıdır.


- **Validateauthority**: isteğe bağlı.  Belirteçlerin vereni doğrulayın. `true` varsayılan değerdir. B2C uygulamaları için, yetkili değeri bilindiğinden ve her ilke için farklı olduğundan, yetkili doğrulaması çalışmaz ve olarak ayarlanması gerekir `false` .

- **Redirecturi**: isteğe bağlı.  Uygulamanızın kimlik doğrulama yanıtlarının gönderilebileceği ve alınabileceği, uygulamanızın yeniden yönlendirme URI 'SI. Portalda kaydettiğiniz yeniden yönlendirme URI 'lerinden biriyle tam olarak eşleşmesi gerekir. Varsayılan olarak olur `window.location.href` .

- **Postlogoutredirecturi**: isteğe bağlı.  `postLogoutRedirectUri`Oturumu kapattıktan sonra kullanıcıyı yeniden yönlendirir. Varsayılan değer `redirectUri` .

- **Navigatetologinrequesturl**: isteğe bağlı. Oturum açtıktan sonra başlangıç sayfasına varsayılan gezintiyi kapatma özelliği. Varsayılan değer doğru değeridir. Bu yalnızca yeniden yönlendirme akışları için kullanılır.

- **Önbellekelocation**: isteğe bağlı.  Tarayıcı depolamayı ya da olarak `localStorage` Ayarlar `sessionStorage` . Varsayılan değer: `sessionStorage`.

- **Storeauthstateıncookie**: isteğe bağlı.  Bu bayrak, Microsoft Internet Explorer ve Microsoft Edge 'de [kimlik doğrulama döngüsü sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#1-issues-due-to-security-zones) için bir çözüm olarak MSAL.js v 0.2.2 ' de tanıtılmıştır. `storeAuthStateInCookie`Bu düzeltmeden yararlanmak için doğru bayrağını etkinleştirin. Bu etkinleştirildiğinde MSAL.js, tarayıcı tanımlama bilgilerinde kimlik doğrulama akışlarının doğrulanması için gereken kimlik doğrulama isteği durumunu depolayacaktır. Varsayılan olarak, bu bayrak olarak ayarlanır `false` .

- **günlükçü**: isteğe bağlı.  Günlükleri özel bir biçimde tüketmek ve yayımlamak için geliştirici tarafından sağlanarak geri çağırma örneği olan bir günlükçü nesnesi. Günlükçü nesnesini geçirme hakkında ayrıntılı bilgi için bkz. [msal.jsile günlüğe kaydetme ](msal-logging.md).

- **Loadframetimeout**: isteğe bağlı.  Azure AD 'den bir belirteç yenileme yanıtı zaman aşımına uğramadan önce geçen işlem yapılmayan milisaniye sayısı. Varsayılan değer 6 saniyedir.

- **tokenRenewalOffsetSeconds**: isteğe bağlı. Süre sonu öncesinde belirteci yenilemek için gereken konum penceresini ayarlayan milisaniye sayısı. Varsayılan değer 300 milisaniyedir.

- **Navigateframewait**: isteğe bağlı. Gizli iframe 'ler hedefine geçmeden önce bekleme süresini ayarlayan milisaniye sayısı. Varsayılan değer 500 milisaniyedir.

Bunlar yalnızca MSAL angular sarmalayıcı kitaplığından geçirilme için geçerlidir:
- **Unprotectedresources**: isteğe bağlı.  Korumasız kaynakları olan URI 'Ler dizisi. MSAL, bu URI 'ye sahip giden isteklere belirteç iliştirmez. Varsayılan olarak olur `null` .

- **Protectedresourcemap**: isteğe bağlı.  Bu, Web API çağrılarında erişim belirteçlerini otomatik olarak eklemek için MSAL tarafından kullanılan kapsamlara yönelik kaynakları eşleştirmekte. Kaynak için tek bir erişim belirteci alındı. Bu nedenle, belirli bir kaynak yolunu şu şekilde eşleyebilirsiniz: {" https://graph.microsoft.com/v1.0/me ", ["User. Read"]} veya kaynağın uygulama URL 'si: {" https://graph.microsoft.com/ ", ["User. Read", "MAIL. Send"]}. CORS çağrıları için bu gereklidir. Varsayılan olarak olur `null` .
