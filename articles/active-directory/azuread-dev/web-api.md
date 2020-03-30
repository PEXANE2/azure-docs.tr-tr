---
title: Azure Etkin Dizini'ndeki Web API uygulamaları
description: Web API uygulamalarının ne olduğunu ve bu uygulama türü için protokol akışı, kayıt ve belirteç sona erme ile ilgili temel bilgileri açıklar.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 9cf5a9c81ca1d7a42a5a8e342dee55f335656c3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154432"
---
# <a name="web-api"></a>Web API

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Web API uygulamaları, bir web API'sinden kaynak elde etmesi gereken web uygulamalarıdır. Bu senaryoda, web uygulamasının web API'sini doğrulamak ve çağırmak için kullanabileceği iki kimlik türü vardır:

- **Uygulama kimliği** - Bu senaryo, uygulama olarak kimlik doğrulaması yapmak ve web API'sine erişmek için OAuth 2.0 istemci kimlik bilgilerini kullanır. Bir uygulama kimliği kullanırken, web API'si yalnızca web API'sı kullanıcı hakkında herhangi bir bilgi almadığı için web uygulamasının onu aradığını algılayabilir. Uygulama kullanıcı hakkında bilgi alırsa, uygulama protokolü aracılığıyla gönderilir ve Azure AD tarafından imzalanmaz. Web API, web uygulamasının kullanıcının kimliğini doğruladığını güvenir. Bu nedenle, bu desen güvenilir bir alt sistem olarak adlandırılır.
- **Temsilci kullanıcı kimliği** - Bu senaryo iki şekilde gerçekleştirilebilir: OpenID Connect ve gizli bir istemci ile OAuth 2.0 yetkilendirme kodu hibe. Web uygulaması, kullanıcı için bir erişim belirteci elde eder, bu da web API'ye kullanıcının web uygulamasına başarıyla doğrulandığını ve web uygulamasının web API'sını aramak için yetkiverilen bir kullanıcı kimliği elde edebildiğini kanıtlar. Bu erişim belirteci, kullanıcıyı yetkilendirmeve istenen kaynağı döndüren web API'sine istek olarak gönderilir.

Hem uygulama kimliği hem de temsilci kullanıcı kimlik türleri aşağıdaki akışta ele alınmıştır. Aralarındaki temel fark, devredilen kullanıcı kimliğinin, kullanıcıoturum açmadan ve web API'sine erişebilmek için önce bir yetkilendirme kodu edinmesi gerektiğidir.

## <a name="diagram"></a>Diyagram

![Web Uygulaması Ndan Web API diyagramına](./media/authentication-scenarios/web-app-to-web-api.png)

## <a name="protocol-flow"></a>Protokol akışı

### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>OAuth 2.0 müşteri kimlik bilgileri hibe ile başvuru kimliği

1. Bir kullanıcı web uygulamasında Azure AD'de oturum açtır (daha fazla bilgi için **Web uygulamaları** bölümüne bakın).
1. Web uygulamasının, web API'sine kimlik doğrulaması yapabilmesi ve istenen kaynağı alabilmesi için bir erişim belirteci edinmesi gerekir. Kimlik bilgisi, uygulama kimliği ve web API uygulama kimliği URI sağlayarak Azure AD'nin belirteç bitiş noktasına bir istekte bulunarak.
1. Azure AD uygulamanın kimliğini doğrular ve web API'sını aramak için kullanılan bir JWT erişim belirteci döndürür.
1. HTTPS üzerinden, web uygulaması, web API'ya isteğin Yetkilendirme başlığında "Taşıyıcı" ibaresi bulunan JWT dizesini eklemek için döndürülen JWT erişim belirteci kullanır. Web API sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

### <a name="delegated-user-identity-with-openid-connect"></a>OpenID Connect ile temsilci kullanıcı kimliği

1. Bir kullanıcı Azure AD kullanarak bir web uygulamasında oturum açtı (yukarıdaki Web Tarayıcısı-Web Uygulaması bölümüne bakın). Web uygulamasının kullanıcısı, web uygulamasının kendi adına web API'sini aramasına izin vermeyi henüz kabul etmediyse, kullanıcının onay göstermesi gerekir. Uygulama, gerektirdiği izinleri görüntüler ve bunlardan herhangi biri yönetici düzeyinde izinlerse, dizindeki normal bir kullanıcı izin veremeyecektir. Bu onay işlemi, uygulama zaten gerekli izinlere sahip olacağından, yalnızca tek kiracı uygulamaları için değil, çok kiracılı uygulamalar için geçerlidir. Kullanıcı oturum landığında, web uygulaması kullanıcı hakkında bilgi içeren bir kimlik belirteci ve bir yetkilendirme kodu aldı.
1. Azure AD tarafından verilen yetkilendirme kodunu kullanarak, web uygulaması Azure AD'nin yetkilendirme kodunu, istemci uygulamasıyla ilgili ayrıntıları (Application ID ve yeniden yönlendirme URI) ve istenen kaynağı (uygulama kimliği) içeren belirteç bitiş noktasına bir istek gönderir Web API için URI).
1. Yetkilendirme kodu ve web uygulaması ve web API'si hakkındaki bilgiler Azure AD tarafından doğrulanır. Azure AD, başarılı bir doğrulama üzerine iki belirteç döndürür: JWT erişim belirteci ve JWT yenileme belirteci.
1. HTTPS üzerinden, web uygulaması, web API'ya isteğin Yetkilendirme başlığında "Taşıyıcı" ibaresi bulunan JWT dizesini eklemek için döndürülen JWT erişim belirteci kullanır. Web API sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>OAuth 2.0 yetki kodu hibesi ile temsilci kullanıcı kimliği

1. Bir kullanıcı, kimlik doğrulama mekanizması Azure AD'den bağımsız olan bir web uygulamasında zaten oturum açmış durumda.
1. Web uygulaması bir erişim jetonu edinmek için bir yetkilendirme kodu gerektirir, bu nedenle tarayıcı üzerinden Azure AD'nin yetkilendirme bitiş noktasına bir istek yayınlar ve başarılı kimlik doğrulamadan sonra URI'yi web uygulamasına yönlendirir. Kullanıcı Azure AD'de kaydolur.
1. Web uygulamasının kullanıcısı, web uygulamasının kendi adına web API'sini aramasına izin vermeyi henüz kabul etmediyse, kullanıcının onay göstermesi gerekir. Uygulama, gerektirdiği izinleri görüntüler ve bunlardan herhangi biri yönetici düzeyinde izinlerse, dizindeki normal bir kullanıcı izin veremeyecektir. Bu onay hem tek hem de çok kiracılı uygulama için geçerlidir. Tek kiracı durumunda, yönetici, kullanıcıları adına onay vermek için yönetici onayı gerçekleştirebilir. Bu, Azure `Grant Permissions` [portalındaki](https://portal.azure.com)düğmeyi kullanarak yapılabilir. 
1. Kullanıcı onay verdikten sonra, web uygulaması bir erişim jetonu almak için gereken yetkilendirme kodunu alır.
1. Azure AD tarafından verilen yetkilendirme kodunu kullanarak, web uygulaması Azure AD'nin yetkilendirme kodunu, istemci uygulamasıyla ilgili ayrıntıları (Application ID ve yeniden yönlendirme URI) ve istenen kaynağı (uygulama kimliği) içeren belirteç bitiş noktasına bir istek gönderir Web API için URI).
1. Yetkilendirme kodu ve web uygulaması ve web API'si hakkındaki bilgiler Azure AD tarafından doğrulanır. Azure AD, başarılı bir doğrulama üzerine iki belirteç döndürür: JWT erişim belirteci ve JWT yenileme belirteci.
1. HTTPS üzerinden, web uygulaması, web API'ya isteğin Yetkilendirme başlığında "Taşıyıcı" ibaresi bulunan JWT dizesini eklemek için döndürülen JWT erişim belirteci kullanır. Web API sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

## <a name="code-samples"></a>Kod örnekleri

Web Uygulaması ndan Web API senaryolarına yönelik kod örneklerine bakın. Ve sık sık tekrar kontrol edin - yeni örnekler sık sık eklenir. [Web Uygulaması Web API](sample-v1-code.md#web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity)için .

## <a name="app-registration"></a>Uygulama kaydı

Bir uygulamayı Azure AD v1.0 bitiş noktasına kaydetmek için [bkz.](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)

* Tek kiracı - Hem uygulama kimliği hem de temsilci kullanıcı kimliği örnekleri için, web uygulaması ve web API'sı Azure AD'de aynı dizine kaydedilmelidir. Web API' si, web uygulamasının kaynaklarına erişimini sınırlamak için kullanılan bir dizi izini ortaya çıkaracak şekilde yapılandırılabilir. Devralınan bir kullanıcı kimliği türü kullanılıyorsa, web uygulamasının **İzinler'den** Azure portalındaki diğer uygulamalar açılır menüsüne istenen izinleri seçmesi gerekir. Uygulama kimlik türü kullanılıyorsa bu adım gerekli değildir.
* Çok kiracı - İlk olarak, web uygulaması işlevsel olması için gereken izinleri belirtmek için yapılandırılmıştır. Bu gerekli izin listesi, hedef dizindeki bir kullanıcı veya yönetici uygulamaya onay verdiğinde bir iletişim kutusunda gösterilir ve bu da uygulamayı kuruluşunun kullanımına sunar. Bazı uygulamalar yalnızca, kuruluştaki herhangi bir kullanıcının izin verebileceği kullanıcı düzeyinde izinler gerektirir. Diğer uygulamalar, kuruluştaki bir kullanıcının izin veremeyeceği yönetici düzeyinde izinler gerektirir. Yalnızca bir dizin yöneticisi, bu düzeyde izin gerektiren uygulamalara izin verebilir. Kullanıcı veya yönetici izin verdiğinde, web uygulaması ve web API'si dizine kaydedilir.

## <a name="token-expiration"></a>Belirteç sona ermesi

Web uygulaması jwt erişim belirteci almak için yetkilendirme kodunu kullandığında, aynı zamanda bir JWT yenileme belirteci alır. Erişim belirteci süresi dolduğunda, yenileme belirteci yeniden oturum açmalarına gerek kalmadan kullanıcının yeniden kimlik doğruluğunu yeniden doğrulamak için kullanılabilir. Bu yenileme belirteci daha sonra kullanıcının kimliğini doğrulamak için kullanılır ve bu da yeni bir erişim belirteci ve yenibelirtere neden olur.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer Uygulama [türleri ve senaryoları](app-types.md) hakkında daha fazla bilgi edinin
- Azure AD [kimlik doğrulama temelleri](v1-authentication-scenarios.md) hakkında bilgi edinin
