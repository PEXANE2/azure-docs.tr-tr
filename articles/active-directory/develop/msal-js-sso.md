---
title: Çoklu oturum açma (MSAL.js) | Mavisi
titleSuffix: Microsoft identity platform
description: JavaScript (MSAL.js) için Microsoft kimlik doğrulama kitaplığı 'nı kullanarak çoklu oturum açma deneyimleri oluşturma hakkında bilgi edinin.
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
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84690787"
---
# <a name="single-sign-on-with-msaljs"></a>MSAL.js ile çoklu oturum açma

Çoklu oturum açma (SSO), kullanıcıların kimlik doğrulaması gerektirmeden birden çok uygulama arasında yeniden kullanılabilecek bir oturum oluşturmak ve oturum açması için kullanıcıların kimlik bilgilerini bir kez girmelerini sağlar. Bu, kullanıcıya sorunsuz bir deneyim sağlar ve kimlik bilgileri için yinelenen istekleri azaltır.

Azure AD, Kullanıcı ilk kez kimlik doğrulaması gerçekleştirdiğinde bir oturum tanımlama bilgisi ayarlayarak uygulamalara SSO özellikleri sağlar. MSAL.js kitaplığı, uygulamaların bunu birkaç yolla kullanmasına izin verir.

## <a name="sso-between-browser-tabs"></a>Tarayıcı sekmeleri arasındaki SSO

Uygulamanız birden çok sekmede açık olduğunda ve öncelikle bir sekmede Kullanıcı oturum açabilirseniz, Kullanıcı sorulmadan diğer sekmelerde da oturum açar. MSAL.js tarayıcıda kullanıcının KIMLIK belirtecini önbelleğe alır `localStorage` ve diğer açık sekmelerdeki uygulamayı uygulamada oturum açar.

Varsayılan olarak, MSAL.js, `sessionStorage` oturumun sekmeler arasında paylaşılmasına izin vermiyor ' ı kullanır. Sekmeler arasında SSO almak için `cacheLocation` MSAL.js ın ' u `localStorage` aşağıda gösterildiği gibi ayarladığınızdan emin olun.

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

Bir kullanıcı kimlik doğrulaması gerçekleştirdiğinde, tarayıcıda Azure AD etki alanı üzerinde bir oturum tanımlama bilgisi ayarlanır. MSAL.js, Kullanıcı için farklı uygulamalar arasında SSO sağlamak üzere bu oturum tanımlama bilgisine bağımlıdır. MSAL.js Ayrıca, uygulama etki alanı başına tarayıcı depolamada kullanıcının KIMLIK belirteçlerini ve erişim belirteçlerini önbelleğe alır. Sonuç olarak, SSO davranışı farklı durumlar için farklılık gösterir:  

### <a name="applications-on-the-same-domain"></a>Aynı etki alanındaki uygulamalar

Uygulamalar aynı etki alanında barındırıldığı zaman, Kullanıcı bir kez oturum açıp daha sonra bir istem olmadan diğer uygulamalara kimlik doğrulaması yapabilir. MSAL.js, SSO sağlamak için etki alanındaki Kullanıcı için önbelleğe alınmış belirteçlerden yararlanır.

### <a name="applications-on-different-domain"></a>Farklı etki alanındaki uygulamalar

Uygulamalar farklı etki alanlarında barındırıldığı zaman etki alanı A 'da önbelleğe alınan belirteçlere etki alanı B MSAL.js tarafından erişilemez.

Bu, etki alanı A 'da oturum açan kullanıcılar B etki alanındaki bir uygulamaya gittiklerinde, bu kullanıcıların Azure AD sayfasında yeniden yönlendirilmesi veya istenir olması anlamına gelir. Azure AD hala Kullanıcı oturumu tanımlama bilgisine sahip olduğundan, Kullanıcı oturum açacaktır ve kimlik bilgilerini yeniden girmesi gerekmez. Kullanıcının oturum açmak için Azure AD ile birden çok kullanıcı hesabı varsa, kullanıcıdan oturum açması için ilgili hesabı seçmesi istenir.

### <a name="automatically-select-account-on-azure-ad"></a>Azure AD 'de otomatik olarak hesap Seç

Bazı durumlarda, uygulamanın kullanıcının kimlik doğrulama bağlamına erişimi vardır ve birden çok hesap oturum açarken Azure AD hesabı seçim isteminden kaçınmak istiyor.  Bu, birkaç farklı şekilde yapılabilir:

**Oturum KIMLIĞINI (SID) kullanma**

Oturum KIMLIĞI, KIMLIK belirteçlerinde yapılandırılabilecek [isteğe bağlı bir talep](active-directory-optional-claims.md) . Bu talep, uygulamanın kullanıcının hesap adının veya Kullanıcı adından bağımsız olarak kullanıcının Azure AD oturumunu belirlemesine izin verir. SID 'yi istek parametrelerinde `acquireTokenSilent` çağrıya geçirebilirsiniz. Bu, Azure AD 'nin hesap seçimini atlamasına izin verir. SID, oturum tanımlama bilgisine bağlanır ve tarayıcı bağlamlarına geçmez.

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
> SID yalnızca MSAL.js içindeki çağrıya göre yapılan sessiz kimlik doğrulama istekleri ile kullanılabilir `acquireTokenSilent` .
Uygulama bildiriminizde isteğe bağlı talepler yapılandırma adımlarını [buradan](active-directory-optional-claims.md)öğrenebilirsiniz.

**Oturum açma Ipucu kullanma**

SID talebi yapılandırılmamışsa veya etkileşimli kimlik doğrulama çağrılarında hesap seçimi istemi 'ni atlamanıza gerek yoksa, `login_hint` istek parametrelerinde bir ve isteğe bağlı `domain_hint` olarak `extraQueryParameters` MSAL.js etkileşimli metotlarda ( `loginPopup` , `loginRedirect` , `acquireTokenPopup` ve `acquireTokenRedirect` ) bir olarak belirterek bunu yapabilirsiniz. Örneğin:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.loginRedirect(request);
```

Login_hint ve domain_hint değerlerini, kullanıcının KIMLIK belirtecinde döndürülen talepleri okuyarak alabilirsiniz.

* **Loginipucu** `preferred_username` , kimlik belirtecindeki talep olarak ayarlanmalıdır.

* **domain_hint** yalnızca sık karşılaşılan yetkilisi kullanılırken geçirilmesi gerekir. Etki alanı ipucu, kiracı KIMLIĞI (TID) tarafından belirlenir.  `tid`Kimlik belirtecindeki talep ise `9188040d-6c67-4c5b-b112-36a304b66dad` tüketicilerdir. Aksi takdirde, kuruluşlardır.

Oturum açma ipucu ve etki alanı ipucu değerleri hakkında daha fazla bilgi için [buraya](v2-oauth2-implicit-grant-flow.md) okuyun.

> [!Note]
> SID ve login_hint aynı anda geçirilemez. Bu, hata yanıtına neden olur.

## <a name="sso-without-msaljs-login"></a>MSAL.js oturum açma olmadan SSO

Tasarıma göre MSAL.js, API 'Ler için belirteçleri almadan önce Kullanıcı bağlamı oluşturmak üzere bir oturum açma yönteminin çağrılması gerekir. Oturum açma yöntemleri etkileşimli olduğundan Kullanıcı bir istem görür.

Uygulamaların kimliği doğrulanmış kullanıcının bağlamına veya kimlik belirtecine, başka bir uygulamada başlatılan kimlik doğrulaması aracılığıyla erişiminin olduğu bazı durumlar vardır ve öncelikle MSAL.js aracılığıyla oturum açmadan belirteçleri almak için SSO 'yu kullanmak ister.

Buna bir örnek: bir Kullanıcı, eklenti veya eklenti olarak çalışan başka bir JavaScript uygulamasını barındıran bir üst Web uygulamasında oturum açtı.

Bu senaryodaki SSO deneyimi aşağıdaki gibi sağlanabilir:

`sid`Kullanılabilir (veya `login_hint` ve isteğe bağlı olarak `domain_hint` ) istek parametrelerini `acquireTokenSilent` aşağıdaki şekilde MSAL.js çağrısına geçirin:

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

## <a name="sso-in-adaljs-to-msaljs-update"></a>MSAL.js güncelleştirmesine ADAL.js SSO

MSAL.js, özellik eşlik 'nı Azure AD kimlik doğrulama senaryoları için ADAL.js taşır. ADAL.js geçişi MSAL.js kolay hale getirmek ve kullanıcılarınızın yeniden oturum açmasını önlemek için, kitaplık, Kullanıcı oturumunu ADAL.js önbelleğinde temsil eden KIMLIK belirtecini okur ve MSAL.js içinde Kullanıcı sorunsuzca oturum açar.  

ADAL.js güncelleştirme sırasında çoklu oturum açma (SSO) davranışından yararlanmak için, kitaplıkların `localStorage` önbelleğe alma belirteçleri için kullandığından emin olmanız gerekir. `cacheLocation` `localStorage` Öğesini başlatma sırasında hem MSAL.js hem de ADAL.js yapılandırmasında aşağıdaki şekilde ayarlayın:


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

Bu yapılandırıldıktan sonra, MSAL.js kimliği doğrulanmış kullanıcının önbelleğe alınmış durumunu ADAL.js okuyabilir ve bunu MSAL.js SSO sağlamak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD 'de [Çoklu oturum açma oturumu ve belirteç yaşam süresi](active-directory-configurable-token-lifetimes.md) değerleri hakkında daha fazla bilgi edinin.
