---
title: Tek oturum açma (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft Kimlik Doğrulama Kitaplığını (MSAL.js) kullanarak tek oturum açma deneyimleri oluşturma hakkında bilgi edinin.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 8080d4cf4c3f0091f7837b3fccead5474c42db55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262860"
---
# <a name="single-sign-on-with-msaljs"></a>MSAL.js ile çoklu oturum açma

Tek Oturum Açma (SSO), kullanıcıların oturum açmaları ve yeniden kimlik doğrulamasına gerek kalmadan birden çok uygulamada yeniden kullanılabilecek bir oturum oluşturmaları için kimlik bilgilerini girmelerini sağlar. Bu, kullanıcıya sorunsuz bir deneyim sağlar ve kimlik bilgileri için yinelenen istemleri azaltır.

Azure AD, kullanıcı ilk kez kimlik doğruladığında bir oturum çerezi ayarlayarak uygulamalara SSO özellikleri sağlar. MSAL.js kitaplığı uygulamaların bu uygulamadan birkaç şekilde yararlanmasına olanak tanır.

## <a name="sso-between-browser-tabs"></a>Tarayıcı sekmeleri arasında SSO

Uygulamanız birden çok sekmede açık olduğunda ve kullanıcıyı ilk kez bir sekmede oturum açtığınızda, kullanıcı da istenmeden diğer sekmelerde oturum açar. MSAL.js tarayıcıdaki `localStorage` kullanıcının kimlik belirteci önbelleğe gelir ve kullanıcıyı diğer açık sekmelerde uygulamaya oturum açar.

Varsayılan olarak, MSAL.js oturumun sekmeler arasında paylaşılmasına izin vermeyen kullanır. `sessionStorage` Sekmeler arasında SSO almak için, `cacheLocation` aşağıda gösterildiği `localStorage` gibi MSAL.js olarak ayarlamak için emin olun.

```javascript
const config = {
    auth: {
        clientId: "abcd-ef12-gh34-ikkl-ashdjhlhsdg"
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

## <a name="sso-between-apps"></a>Uygulamalar arasında SSO

Bir kullanıcı kimlik doğruladığında, tarayıcıdaki Azure AD etki alanında bir oturum çerezi ayarlanır. MSAL.js, kullanıcıya farklı uygulamalar arasında SSO sağlamak için bu oturum çerezine dayanır. MSAL.js ayrıca uygulama etki alanı başına tarayıcı depolama sında kullanıcının kimlik belirteçlerini ve erişim belirteçlerini de önbelleğe adar. Sonuç olarak, SSO davranışı farklı durumlar için değişir:  

### <a name="applications-on-the-same-domain"></a>Aynı etki alanında uygulamalar

Uygulamalar aynı etki alanında barındırıldığında, kullanıcı bir uygulamada bir kez oturum açabilir ve ardından bir istem olmadan diğer uygulamalara kimlik doğrulaması yaptırabilir. MSAL.js, kullanıcının etki alanında SSO sağlaması için önbelleğe alınmış belirteçlerden yararlanır.

### <a name="applications-on-different-domain"></a>Farklı etki alanında uygulamalar

Uygulamalar farklı etki alanlarında barındırıldığında, A etki alanında önbelleğe alınmış belirteçlere B etki alanında MSAL.js tarafından erişilemez.

Bu, A etki alanında oturum açtıklarında kullanıcıların B etki alanında bir uygulamaya yön verdiklerinde, Azure REKLAM sayfasıyla yeniden yönlendirilecekleri veya istenecekleri anlamına gelir. Azure AD kullanıcı oturumu çerezine sahip olduğundan, kullanıcıda oturum açacak tır ve kimlik bilgilerini yeniden girmek zorunda kalmayacaktır. Kullanıcının Azure AD ile oturumda birden çok kullanıcı hesabı varsa, kullanıcıdan oturum açması için ilgili hesabı seçmesi istenir.

### <a name="automatically-select-account-on-azure-ad"></a>Azure AD'de hesabı otomatik olarak seçin

Bazı durumlarda, uygulama kullanıcının kimlik doğrulama bağlamına erişebilir ve birden çok hesap oturum açıldığında Azure REKLAM hesap seçimi isteminden kaçınmak ister.  Bu birkaç farklı şekilde yapılabilir:

**Oturum Kimliğini (SID) kullanma**

Oturum Kimliği, kimlik belirteçlerinde yapılandırılabilen [isteğe bağlı](active-directory-optional-claims.md) bir taleptir. Bu talep, uygulamanın kullanıcının hesap adından veya kullanıcı adından bağımsız olarak kullanıcının Azure REKLAM oturumunu tanımlamasına olanak tanır. İstek parametrelerindeki SID'yi `acquireTokenSilent` çağrıya geçirebilirsiniz. Bu, Azure AD'nin hesap seçimini atlatır. SID oturum çerezine bağlıdır ve tarayıcı bağlamlarını geçmez.

```javascript
var request = {
    scopes: ["user.read"],
    sid: sid
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

> [!Note]
> SID sadece MSAL.js'deki `acquireTokenSilent` çağrı ile yapılan sessiz kimlik doğrulama istekleriile kullanılabilir.
İsteğe bağlı talepleri yapılandırmak için adımları uygulama bildiriminizde [bulabilirsiniz.](active-directory-optional-claims.md)

**Giriş İpucu kullanma**

SID talebiniz yapılandırılmamışsa veya etkileşimli kimlik doğrulama aramalarında hesap seçim istemini atlamanız gerekiyorsa, bunu istek parametrelerinde bir `login_hint` tane ve isteğe bağlı olarak MSAL.js etkileşimli yöntemlerdeki `domain_hint` gibi `extraQueryParameters` (,`loginPopup` `loginRedirect`ve `acquireTokenPopup` `acquireTokenRedirect`) sağlayarak yapabilirsiniz. Örnek:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.loginRedirect(request);
```

Kullanıcı için kimlik belirtecinde döndürülen talepleri okuyarak login_hint ve domain_hint değerlerini alabilirsiniz.

* **loginHint** kimlik belirteci nde `preferred_username` talep olarak ayarlanmalıdır.

* **domain_hint** yalnızca /ortak otoriteyi kullanırken geçirilmesi gerekir. Etki alanı ipucu kiracı kimliği (tid) tarafından belirlenir.  Kimlik `tid` belirtecindeki talep `9188040d-6c67-4c5b-b112-36a304b66dad` tüketicilerse. Aksi takdirde, bu kuruluşlardır.

Giriş ipucu ve etki alanı ipucu değerleri hakkında daha fazla bilgi için [burayı](v2-oauth2-implicit-grant-flow.md) okuyun.

> [!Note]
> SID ve login_hint aynı anda geçemezsiniz. Bu hata yanıtı neden olur.

## <a name="sso-without-msaljs-login"></a>MSAL.js girişi olmadan SSO

Tasarım olarak, MSAL.js API'ler için belirteçleri almadan önce bir kullanıcı bağlamı oluşturmak için bir oturum açma yöntemi çağrılmasını gerektirir. Oturum açma yöntemleri etkileşimli olduğundan, kullanıcı bir istem görür.

Uygulamaların, başka bir uygulamada başlatılan kimlik doğrulama yoluyla kimlik doğrulama yoluyla kimlik doğrulaması veya kimlik belirtecine erişebilen ve SSO'dan msal.js aracılığıyla ilk oturum açmadan belirteçler elde etmek için yararlanmak istediği bazı durumlar vardır.

Bunun bir örneği: Bir kullanıcı, eklenti veya eklenti olarak çalışan başka bir JavaScript uygulamasını barındıran bir üst web uygulamasında oturum açılır.

Bu senaryodaki SSO deneyimi aşağıdaki gibi elde edilebilir:

Varsa `sid` (veya `login_hint` isteğe bağlı `domain_hint`olarak) istek parametreleri olarak `acquireTokenSilent` MSAL.js çağrısını aşağıdaki şekilde iletin:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

## <a name="sso-in-adaljs-to-msaljs-update"></a>SSO ADAL.js msal.js güncelleme

MSAL.js, Azure AD kimlik doğrulama senaryoları için ADAL.js ile özellik eşitliği ni getirir. ADAL.js'den MSAL.js'ye geçişi kolaylaştırmak ve kullanıcılarınızın yeniden oturum açmalarını engellemek için, kitaplık ADAL.js önbelleğinde kullanıcının oturumunu temsil eden kimlik belirtecive MSAL.js'deki kullanıcıda sorunsuz bir şekilde oturum lar.  

ADAL.js'den güncellenirken tek oturum açma (SSO) davranışından yararlanmak için, kitaplıkların belirteçleri önbelleğe almak için kullandığından `localStorage` emin olmanız gerekir. `cacheLocation` Başlangıç olarak `localStorage` hem MSAL.js hem de ADAL.js yapılandırmasında aşağıdaki gibi ayarlayın:


```javascript
// In ADAL.js
window.config = {
   clientId: 'g075edef-0efa-453b-997b-de1337c29185',
   cacheLocation: 'localStorage'
};

var authContext = new AuthenticationContext(config);


// In latest MSAL.js version
const config = {
    auth: {
        clientId: "abcd-ef12-gh34-ikkl-ashdjhlhsdg"
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

Bu yapılandırıldıktan sonra, MSAL.js ADAL.js'deki kimlik doğrulaması kullanıcının önbelleğe alınmış durumunu okuyabilir ve bunu MSAL.js'de SSO sağlamak için kullanabilecektir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD'deki [tek oturum oturumu ve belirteç ömür değerleri](active-directory-configurable-token-lifetimes.md) hakkında daha fazla bilgi edinin.
