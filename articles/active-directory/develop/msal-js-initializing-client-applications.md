---
title: İstemci uygulamalarını başlatma (JavaScript için Microsoft kimlik doğrulama kitaplığı) | Mavisi
description: JavaScript için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL. js) kullanarak istemci uygulamalarını başlatma hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7df2f9aa56e22bd4060c823b02900fa914a0fd7f
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532805"
---
# <a name="initialize-client-applications-using-msaljs"></a>MSAL. js kullanarak istemci uygulamalarını başlatma
Bu makalede, Kullanıcı Aracısı uygulamasının bir örneğiyle JavaScript (MSAL. js) için Microsoft kimlik doğrulama Kitaplığı başlatma açıklanmaktadır. Kullanıcı Aracısı uygulaması, istemci kodunun web tarayıcısı gibi bir Kullanıcı aracısında yürütüldüğü bir ortak istemci uygulaması biçimidir. Tarayıcı bağlamı başarıyla erişilebilir olduğundan bu istemciler gizli dizileri depolamaz. İstemci uygulama türleri ve uygulama yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için [genel bakış](msal-client-applications.md)makalesini okuyun.

## <a name="prerequisites"></a>Önkoşullar
Bir uygulamayı başlatmadan önce, uygulamanızın Microsoft Identity platformu ile tümleştirilebilmesi için öncelikle [bu Azure Portal kaydetmeniz](scenario-spa-app-registration.md) gerekir. Kayıttan sonra, aşağıdaki bilgiler (Azure portal bulunabilir) gerekebilir:

- İstemci KIMLIĞI (uygulamanız için GUID 'ı temsil eden bir dize)
- Kimlik sağlayıcısı URL 'SI (örnek olarak adlandırılır) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre, her topluca yetkili olarak bilinir.
- Yalnızca kuruluşunuz için bir iş kolu uygulaması yazıyorsanız (tek kiracılı uygulama olarak da adlandırılır) kiracı KIMLIĞI.
- Web Apps için, kimlik sağlayıcısının güvenlik belirteçleriyle uygulamanıza geri dönebileceği redirectUri 'yi de ayarlamanız gerekir.

## <a name="initializing-applications"></a>Uygulamalar başlatılıyor

MSAL. js ' i düz bir JavaScript/TypeScript uygulamasında aşağıdaki gibi kullanabilirsiniz. Bir yapılandırma nesnesiyle örnekleyerek `UserAgentApplication` msal kimlik doğrulaması bağlamını başlatın. MSAL. js ' nin başlatılması için gereken en düşük yapılandırma, uygulamanızın kayıt portalından almanız gereken ClientID 'dir.

Yeniden yönlendirme akışları (`loginRedirect` ve `acquireTokenRedirect`) olan kimlik doğrulama yöntemleri için, yöntemi aracılığıyla `handleRedirectCallback()` başarılı veya hata için bir geri çağırma işlemini açık bir şekilde kaydetmeniz gerekecektir. Bu, yeniden yönlendirme akışları, bir açılır deneyimle birlikte olan yöntemler olarak geri dönmediğinden gereklidir.

```javascript
// Configuration object constructed
const config = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”
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

MSAL. js, `UserAgentApplication` tek bir kimlik doğrulama bağlamını temsil etmek için tek bir örneğe ve yapılandırmasına sahip olacak şekilde tasarlanmıştır. Birden çok örnek, tarayıcıda çakışan önbellek girişlerine ve davranışına neden olduğundan önerilmez.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

MSAL. js, bir örneği `UserAgentApplication`oluşturmak için kullanılabilen yapılandırılabilir seçenekler gruplandırması sağlayan aşağıda gösterilen bir yapılandırma nesnesine sahiptir.

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

- **ClientID**: Gerekli. Uygulamanızın ClientID 'si, bunu uygulama kayıt portalından almalısınız.

- **yetkili**: İsteğe bağlı. MSAL 'in belirteçleri isteyebileceğini bir dizin gösteren URL. Varsayılan değer: `https://login.microsoftonline.com/common`.
    * Azure AD 'de, https://&lt;örnek&gt;/&lt;hedef kitlesi&gt;, burada &lt; `https://login.microsoftonline.com`örnekkimliksağlayıcısıetkialanı(örneğin,)&gt; ve hedefkitle&gt; , oturum açma kitini temsil eden bir tanıtıcıdır. &lt; Bu, aşağıdaki değerler olabilir:
        * `https://login.microsoftonline.com/<tenant>`-kiracı, contoso.onmicrosoft.com gibi kiracıyla ilişkili bir etki alanı veya yalnızca belirli bir kuruluşun kullanıcılarına oturum açmak `TenantID` için kullanılan dizinin özelliğini temsil eden GUID 'dir.
        * `https://login.microsoftonline.com/common`-Kullanıcıları iş ve okul hesaplarıyla veya bir Microsoft kişisel hesabıyla oturum açmak için kullanılır.
        * `https://login.microsoftonline.com/organizations/`-Kullanıcıları iş ve okul hesaplarıyla oturum açmak için kullanılır.
        * `https://login.microsoftonline.com/consumers/`-Kullanıcılara yalnızca kişisel Microsoft hesabı (canlı) oturum açmak için kullanılır.
    * Azure AD B2C `https://<instance>/tfp/<tenant>/<policyName>/`, burada Örneğin Azure AD B2C etki alanı, kiracı Azure AD B2C kiracının adıdır, PolicyName, uygulanacak B2C ilkesinin adıdır.


- **Validateauthority**: İsteğe bağlı.  Belirteçlerin vereni doğrulayın. `true` varsayılan değerdir. B2C uygulamaları için, yetkili değeri bilindiğinden ve her ilke için farklı olduğundan, yetkili doğrulaması çalışmaz ve olarak `false`ayarlanması gerekir.

- **Redirecturi**: İsteğe bağlı.  Uygulamanızın kimlik doğrulama yanıtlarının gönderilebileceği ve alınabileceği, uygulamanızın yeniden yönlendirme URI 'SI. Portalın, URL kodlamalı olması dışında, portala kaydettiğiniz yeniden yönlendirme URI 'lerinden biriyle tam olarak eşleşmesi gerekir. Varsayılan olarak `window.location.href`.

- **Postlogoutredirecturi**: İsteğe bağlı.  Oturumu `postLogoutRedirectUri` kapattıktan sonra kullanıcıyı yeniden yönlendirir. Varsayılan, `redirectUri` değeridir.

- **Navigatetologinrequesturl**: İsteğe bağlı. Oturum açtıktan sonra başlangıç sayfasına varsayılan gezintiyi kapatma özelliği. Varsayılan değer true 'dur. Bu yalnızca yeniden yönlendirme akışları için kullanılır.

- **Önbellekelocation**: İsteğe bağlı.  Tarayıcı depolamayı ya `localStorage` `sessionStorage`da olarak ayarlar. Varsayılan, `sessionStorage` değeridir.

- **Storeauthstateıncookie**: İsteğe bağlı.  Bu bayrak, Microsoft Internet Explorer ve Microsoft Edge 'de [kimlik doğrulama döngüsü sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#1-issues-due-to-security-zones) için bir çözüm olarak msal. js v 0.2.2 ' de kullanıma sunulmuştur. Bu düzeltmeden `storeAuthStateInCookie` yararlanmak için doğru bayrağını etkinleştirin. Bu etkinleştirildiğinde, MSAL. js, tarayıcı tanımlama bilgilerinde kimlik doğrulama akışlarının doğrulanması için gereken kimlik doğrulama isteği durumunu depolar. Varsayılan olarak, bu bayrak olarak `false`ayarlanır.

- **günlükçü**: İsteğe bağlı.  Günlükleri özel bir biçimde tüketmek ve yayımlamak için geliştirici tarafından sağlanarak geri çağırma örneği olan bir günlükçü nesnesi. Günlükçü nesnesini geçirme hakkında ayrıntılı bilgi için bkz. [msal. js ile günlüğe kaydetme](msal-logging.md).

- **Loadframetimeout**: İsteğe bağlı.  Azure AD 'den bir belirteç yenileme yanıtı zaman aşımına uğramadan önce geçen işlem yapılmayan milisaniye sayısı. Varsayılan değer 6 saniyedir.

- **tokenRenewalOffsetSeconds**: İsteğe bağlı. Süre sonu öncesinde belirteci yenilemek için gereken konum penceresini ayarlayan milisaniye sayısı. Varsayılan değer 300 milisaniyedir.

Bunlar yalnızca MSAL angular sarmalayıcı kitaplığından geçirilme için geçerlidir:
- **Unprotectedresources**: İsteğe bağlı.  Korumasız kaynakları olan URI 'Ler dizisi. MSAL, bu URI 'ye sahip giden isteklere belirteç iliştirmez. Varsayılan olarak `null`.

- **Protectedresourcemap**: İsteğe bağlı.  Bu, Web API çağrılarında erişim belirteçlerini otomatik olarak eklemek için MSAL tarafından kullanılan kapsamlara yönelik kaynakları eşleştirmekte. Kaynak için tek bir erişim belirteci alındı. Bu nedenle, belirli bir kaynak yolunu şu şekilde eşleyebilirsiniz: { https://graph.microsoft.com/v1.0/me"", ["User. Read"]} veya kaynağın uygulama URL 'si: {"https://graph.microsoft.com/", ["User. Read", "MAIL. Send"]}. CORS çağrıları için bu gereklidir. Varsayılan olarak `null`.
