---
title: MSAL.js istemci uygulamalarını başlatma | Azure
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft Kimlik Doğrulama Kitaplığını (MSAL.js) kullanarak istemci uygulamalarını başlatma hakkında bilgi edinin.
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
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010163"
---
# <a name="initialize-client-applications-using-msaljs"></a>MSAL.js kullanarak istemci uygulamalarını başlatma
Bu makalede, JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js) bir kullanıcı aracısı uygulaması örneği yle baş harfe aktarma açıklanmaktadır. Kullanıcı aracısı uygulaması, istemci kodunun web tarayıcısı gibi bir kullanıcı aracısında yürütüldürün ortak istemci uygulaması biçimidir. Tarayıcı bağlamına açıkça erişilebildiğinden, bu istemciler sırları saklamaz. İstemci uygulama türleri ve uygulama yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için [genel bakışı](msal-client-applications.md)okuyun.

## <a name="prerequisites"></a>Ön koşullar
Bir uygulamayı başlatmadan önce, uygulamanızın Microsoft kimlik platformuyla tümleştirilebilmeleri için [uygulamanızı azure portalına kaydetmeniz](scenario-spa-app-registration.md) gerekir. Kayıt olduktan sonra aşağıdaki bilgilere ihtiyacınız olabilir (Azure portalında bulunabilir):

- İstemci kimliği (başvurunuz için GUID'i temsil eden bir dize)
- Kimlik sağlayıcı URL (örneğin adı) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre topluca otorite olarak bilinir.
- Yalnızca kuruluşunuz için bir iş yeri uygulaması yazıyorsanız kiracı kimliği (tek kiracılı uygulama olarak da adlandırılır).
- Web uygulamaları için, kimlik sağlayıcısının güvenlik belirteçleriyle uygulamanıza döneceği redirectUri'yi de ayarlamanız gerekir.

## <a name="initializing-applications"></a>Uygulamaları başlatma

MSAL.js'yi düz bir JavaScript/Typescript uygulamasında aşağıdaki gibi kullanabilirsiniz. Bir yapılandırma nesnesi ile anında `UserAgentApplication` anlayarak MSAL kimlik doğrulama bağlamını başlatma. MSAL.js'yi başlatmanız için gereken minimum config, başvuru kayıt portalından almalısınız.

MSAL.js 1.2.x veya daha önceki yönlendirme akışları`loginRedirect` (ve), `acquireTokenRedirect`kimlik doğrulama yöntemleri için, yöntem aracılığıyla `handleRedirectCallback()` başarı veya hata için bir geri arama yı açıkça kaydetmeniz gerekir. Yönlendirme akışları, pop-up deneyimi olan yöntemlerin verdiği gibi vaatleri döndürmediğinden bu gereklidir. Bu, MSAL.js sürüm 1.3.0'da isteğe bağlı hale geldi.

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

MSAL.js, tek bir kimlik doğrulama bağlamını temsil `UserAgentApplication` edecek şekilde tek bir örnek ve yapılandırmaya sahip olacak şekilde tasarlanmıştır. Tarayıcıda çakışan önbellek girişlerine ve davranışlara neden olduklarından, birden çok örnek önerilmez.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

MSAL.js'nin bir örneğini oluşturmak için kullanılabilir yapılandırılabilir seçeneklerin `UserAgentApplication`gruplandırılmasını sağlayan aşağıda gösterilen bir yapılandırma nesnesi vardır.

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

Aşağıda, config nesnesinde şu anda desteklenen toplam yapılandırılabilir seçenekler kümesi verilmiştir:

- **clientID**: Gerekli. Başvurunuzun müşteri kimliği, başvuru kayıt portalından bu almalısınız.

- **otorite**: İsteğe bağlı. MSAL'ın jeton isteyebileceği bir dizini gösteren bir URL. Varsayılan değer: `https://login.microsoftonline.com/common`.
    * Azure AD'de,&lt;örneğin kimlik&gt;/&lt;sağlayıcı&gt;etki &lt;&gt; alanı (örneğin, `https://login.microsoftonline.com`) ve &lt;hedef&gt; kitle oturum açma hedef kitlesini temsil eden bir tanımlayıcı dır https:// örnek hedef kitle biçimidir. Bu aşağıdaki değerler olabilir:
        * `https://login.microsoftonline.com/<tenant>`- kiracı, contoso.onmicrosoft.com veya yalnızca belirli bir kuruluşun kullanıcılarını `TenantID` oturum açmada kullanılan dizinin özelliğini temsil eden GUID gibi kiracıyla ilişkili bir etki alanıdır.
        * `https://login.microsoftonline.com/common`- İş ve okul hesapları veya Microsoft kişisel hesabı olan kullanıcıları oturum alamak için kullanılır.
        * `https://login.microsoftonline.com/organizations/`- İş ve okul hesapları olan kullanıcılarda oturum açmada kullanılır.
        * `https://login.microsoftonline.com/consumers/`- Yalnızca kişisel Microsoft hesabı (canlı) olan kullanıcılarda oturum açmada kullanılır.
    * Azure AD B2C'de, azure `https://<instance>/tfp/<tenant>/<policyName>/`AD B2C etki alanı yani {kiracı-adı}.b2clogin.com olduğu, kiracının Azure AD B2C kiracısının adı yani {kiracı-adı}.onmicrosoft.com olduğu formdur, policyName uygulanacak B2C ilkesinin adıdır.


- **validateAuthority**: İsteğe bağlı.  Belirteçlerin verenini doğrulayın. `true` varsayılan değerdir. B2C uygulamaları için, yetki değeri bilindiği nden ve ilke başına farklı olabileceğinden, yetki doğrulaması çalışmaz ve .' olarak `false`ayarlanması gerekir.

- **redirectUri**: İsteğe bağlı.  Kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabildiği uygulamanızın URI'yi yönlendirmesi. Portalda kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmelidir. Varsayılan `window.location.href`değer.

- **postLogoutRedirectUri**: İsteğe bağlı.  Oturum dışı çıktıktan `postLogoutRedirectUri` sonra kullanıcıyı yönlendirir. Varsayılan değer. `redirectUri`

- **navigateToLoginRequestUrl**: İsteğe bağlı. Girişten sonra sayfayı başlatmak için varsayılan gezinmeyi kapatma yeteneği. Varsayılan değer doğru değeridir. Bu yalnızca yeniden yönlendirme akışları için kullanılır.

- **cacheLocation**: İsteğe bağlı.  Tarayıcı depolama alanını `localStorage` `sessionStorage`ya da Varsayılan değer: `sessionStorage`.

- **storeAuthStateInCookie**: İsteğe bağlı.  Bu bayrak, MSAL.js v0.2.2'de Microsoft Internet Explorer ve Microsoft Edge'deki [kimlik doğrulama döngüsü sorunlarının](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#1-issues-due-to-security-zones) düzeltilmesi olarak tanıtıldı. Bu düzeltmeden yararlanmak için bayrağın `storeAuthStateInCookie` doğru olmasını etkinleştirin. Bu etkinleştirildiğinde, MSAL.js tarayıcı tanımlama bilgilerinde auth akışlarının doğrulanması için gereken auth istek durumunu depolar. Varsayılan olarak bu bayrak `false`.

- **logger**: İsteğe bağlı.  Günlükleri özel bir şekilde tüketmek ve yayımlamak için geliştirici tarafından sağlanabilecek bir geri arama örneği olan bir Logger nesnesi. Logger nesnesi geçen hakkında ayrıntılı bilgi için, [msal.js ile günlük](msal-logging.md)bakın.

- **loadFrameTimeout**: İsteğe bağlı.  Azure AD'den gelen belirteç yenileme yanıtından önceki milisaniyelik hareketsizlik sayısı zaman doldu olarak kabul edilmelidir. Varsayılan değer 6 saniyedir.

- **tokenRenewalOffsetSeconds**: İsteğe bağlı. Son kullanma tarihinden önce belirteci yenilemek için gereken ofset penceresini ayarlayan milisaniye sayısı. Varsayılan değer 300 milisaniyedir.

- **navigateFrameWait**: İsteğe bağlı. Gizli iframe'lerden önce bekleme süresini ayarlayan milisaniye sayısı hedeflerine doğru gidin. Varsayılan değer 500 milisaniyedir.

Bunlar yalnızca MSAL Açısal sarıcı kitaplığından geçirilebilir:
- **korumasız Kaynaklar**: İsteğe bağlı.  Korumasız kaynaklar olan UR'ler dizisi. MSAL, bu URI'ye sahip giden isteklere bir belirteç eklemez. Varsayılan `null`değer.

- **protectedResourceMap**: İsteğe bağlı.  Bu, kaynakların Web API aramalarında erişim belirteçlerini otomatik olarak eklemek için MSAL tarafından kullanılan kapsamlara eşlemedir. Kaynak için tek bir erişim belirteci elde edilir. Bu nedenle belirli bir kaynak yolunu aşağıdakihttps://graph.microsoft.com/v1.0/megibi eşleyebilirsiniz: {" ", ["user.read"]},https://graph.microsoft.com/veya kaynağın uygulama URL'si: {" ", ["user.read", "mail.send"]}. Bu, CORS aramaları için gereklidir. Varsayılan `null`değer.
