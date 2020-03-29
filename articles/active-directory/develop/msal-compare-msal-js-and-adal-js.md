---
title: MSAL.js ve ADAL.js arasındaki farklar | Azure
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js) ile JavaScript için Azure AD Kimlik Doğrulama Kitaplığı (ADAL.js) arasındaki farklar ve hangisini kullanacağınızı seçme arasındaki farklar hakkında bilgi edinin.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/10/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 7238a78279528b4522d09178d00bf916f14bad88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696427"
---
# <a name="differences-between-msal-js-and-adal-js"></a>MSAL JS ve ADAL JS arasındaki farklar

Hem JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js) hem de JavaScript için Azure AD Kimlik Doğrulama Kitaplığı (ADAL.js) Azure AD varlıklarının kimliğini doğrulamak ve Azure AD'den belirteçleri istemek için kullanılır. Şimdiye kadar çoğu geliştirici, ADAL kullanarak belirteçler isteyerek Azure AD kimliklerinin (iş ve okul hesapları) kimliğini doğrulamak için geliştiriciler için Azure AD ile (v1.0) çalışmıştır. Artık MSAL.js'yi kullanarak, Microsoft kimlik platformu (v2.0) aracılığıyla daha geniş bir Microsoft kimlik kümesinin (Azure AD kimlikleri ve Microsoft hesapları ve Azure AD B2C aracılığıyla sosyal ve yerel hesaplar) kimliğini doğrulayabilirsiniz.

Bu makalede, JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js) ile JavaScript için Azure AD Kimlik Doğrulama Kitaplığı (ADAL.js) arasında nasıl seçim yapılacağı anlatılıyor ve iki kitaplığı karşılaştırıyor.

## <a name="choosing-between-adaljs-and-msaljs"></a>ADAL.js ve MSAL.js arasında seçim

Çoğu durumda, Microsoft kimlik doğrulama kitaplıklarının en son nesli olan Microsoft kimlik platformu ve MSAL.js'yi kullanmak istersiniz. MSAL.js'yi kullanarak, Azure AD (iş ve okul hesapları), Microsoft (kişisel) hesapları (MSA) veya Azure AD B2C ile uygulamanızda oturum açan kullanıcılar için belirteçler edinirsiniz.

Zaten v1.0 bitiş noktası (ve ADAL.js) aşina iseniz, [v2.0 bitiş noktası hakkında farklı ne](active-directory-v2-compare.md)okumak isteyebilirsiniz? .

Ancak, uygulamanızın [Active Directory Federation Services 'in (ADFS)](/windows-server/identity/active-directory-federation-services)önceki sürümlerine sahip kullanıcılarda oturum açması gerekiyorsa, Yine de ADAL.js kullanmanız gerekir.

## <a name="key-differences-in-authentication-with-msaljs"></a>MSAL.js ile kimlik doğrulamada temel farklar

### <a name="core-api"></a>Çekirdek API

* ADAL.js, uygulamanızın yetki sunucusuna veya kimlik sağlayıcısına bir yetkili URL aracılığıyla bağlantısının bir örneğinin temsili olarak [AuthenticationContext'ı](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Config-authentication-context#authenticationcontext) kullanır. Aksine, MSAL.js API kullanıcı aracısı istemci uygulaması (istemci kodu bir web tarayıcısı gibi bir kullanıcı aracısı yürütülür kamu istemci uygulaması biçimi) etrafında tasarlanmıştır. `UserAgentApplication` Sınıfın yetkilendirme sunucusuyla birlikte uygulamanın kimlik doğrulama bağlamının bir örneğini temsil etmesini sağlar. Daha fazla bilgi için [MSAL.js kullanarak Initialize'e](msal-js-initializing-client-applications.md)bakın.

* ADAL.js'de, belirteçleri edinme yöntemleri, belirteçleri `AuthenticationContext`ile ' de belirlenen tek bir yetki ile ilişkilidir. MSAL.js'de, edinme belirteci istekleri, `UserAgentApplication`'de ayarlanandan farklı yetki değerleri alabilir. Bu, MSAL.js'nin aynı uygulamada birden çok kiracı ve kullanıcı hesabı için ayrı ayrı belirteçler edinmesine ve önbelleğe almasına olanak tanır.

* Kullanıcılara sormadan sessizce belirteçleri edinme `acquireToken` ve yenileme yöntemi ADAL.js'de adlandırılır. MSAL.js'de bu yöntem, bu işlevselliğin daha açıklayıcı olması için adlandırılır. `acquireTokenSilent`

### <a name="authority-value-common"></a>Yetki değeri`common`

v1.0'da, `https://login.microsoftonline.com/common` yetkiyi kullanmak kullanıcıların herhangi bir Azure REKLAM hesabıyla (herhangi bir kuruluş için) oturum açmalarına olanak tanır.

V2.0'da, `https://login.microsoftonline.com/common` yetkiyi kullanarak, kullanıcıların herhangi bir Azure REKLAM kuruluş hesabı veya Microsoft kişisel hesabı (MSA) ile oturum açmalarına olanak tanır. Oturum açma oturumaçmayı yalnızca Azure AD hesaplarıyla sınırlamak için (ADAL.js ile aynı davranış) . `https://login.microsoftonline.com/organizations` Ayrıntılar için, `authority` [MSAL.js kullanarak Initialize](msal-js-initializing-client-applications.md)config seçeneğine bakın.

### <a name="scopes-for-acquiring-tokens"></a>Belirteçleri edinme kapsamları
* Belirteçleri elde etmek için kimlik doğrulama isteklerinde kaynak parametresi yerine kapsam

    v2.0 protokolü isteklerde kaynak yerine kapsamları kullanır. Başka bir deyişle, uygulamanızın MS Graph gibi bir kaynak için izinleri olan belirteçleri istemesi gerektiğinde, kitaplık yöntemlerine geçirilen değerler arasındaki fark aşağıdaki gibidir:

    v1.0: kaynak\:= https //graph.microsoft.com

    v2.0: kapsam\:= https //graph.microsoft.com/User.Read

    Bu formatta API URI kullanarak herhangi bir kaynak API için kapsam isteyebilirsiniz: appidURI/scopeÖrneğin: https:\//mytenant.onmicrosoft.com/myapi/api.read

    Yalnızca MS Graph API için, `user.read` https:\//graph.microsoft.com/User.Read için bir kapsam değeri eşlemleri ve birbirinin yerine kullanılabilir.

    ```javascript
    var request = {
        scopes = ["User.Read"];
    };

    acquireTokenPopup(request);   
    ```

* Artımlı onay için dinamik kapsamlar.

    v1.0 kullanarak uygulama yaparken, kullanıcının oturum açma sırasında onay alması için uygulamanın gerektirdiği tüm izinleri (statik kapsamlar) kaydetmeniz gerekir. v2.0'da, izinleri istediğiniz anda istemek için kapsam parametresini kullanabilirsiniz. Bunlara dinamik kapsamlar denir. Bu, kullanıcının kapsamlara artımlı onay sağlamasına olanak tanır. Bu nedenle, başlangıçta yalnızca kullanıcının uygulamanızda oturum açmasını istiyorsanız ve herhangi bir erişime ihtiyacınız yoksa, bunu yapabilirsiniz. Daha sonra kullanıcının takvimini okuma yeteneğine ihtiyacınız varsa, daha sonra satın alma Token yöntemleritakvimtakviminin kapsamını isteyebilir ve kullanıcının onayını alabilirsiniz. Örnek:

    ```javascript
    var request = {
        scopes = ["https://graph.microsoft.com/User.Read", "https://graph.microsoft.com/Calendar.Read"];
    };

    acquireTokenPopup(request);   
    ```

* V1.0 API'leri için kapsamlar

    MSAL.js kullanarak V1.0 API'leri için belirteçler alırken, KAPSAM OLARAK API'nin App Id URI'sine ekleyerek `.default` API'de kayıtlı tüm statik kapsamları isteyebilirsiniz. Örnek:

    ```javascript
    var request = {
        scopes = [ appidURI + "/.default"];
    };

    acquireTokenPopup(request);
    ```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [v1.0 ve v2.0 karşılaştırmasına](active-directory-v2-compare.md)bakın.
