---
title: Azure Active Directory'deki yerel uygulamalar
description: Yerel uygulamaların ne olduğunu ve bu uygulama türü için protokol akışı, kayıt ve belirteç sona erme ile ilgili temel bilgileri açıklar.
services: active-directory
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
ms.openlocfilehash: 9ecf711f5442b6f21de53d2735ad1c94d7cb6223
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154806"
---
# <a name="native-apps"></a>Yerel uygulamalar

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Yerel uygulamalar, kullanıcı adına web API'si arayan uygulamalardır. Bu senaryo, [OAuth 2.0 belirtimibölüm](https://tools.ietf.org/html/rfc6749)4.1'de açıklandığı gibi, bir kamu istemcisi ile OAuth 2.0 yetkilendirme kodu hibe türü üzerine inşa edilmiştir. Yerel uygulama, OAuth 2.0 protokolünü kullanarak kullanıcı için bir erişim jetonu alır. Bu erişim belirteci daha sonra isteği kullanıcıya yetki veren ve istenen kaynağı döndüren web API'sine gönderilir.

## <a name="diagram"></a>Diyagram

![Web API Diyagramına Yerel Uygulama](./media/authentication-scenarios/native-app-to-web-api.png)

## <a name="protocol-flow"></a>Protokol akışı

AD Kimlik Doğrulama Kitaplıkları kullanıyorsanız, tarayıcı açılır pencere, belirteç önbelleğe alma ve yenileme belirteçlerinin işlenmesi gibi aşağıda açıklanan protokol ayrıntılarının çoğu sizin için işlenir.

1. Yerel uygulama, bir tarayıcı açılır penceresini kullanarak Azure AD'deki yetkilendirme bitiş noktasına istekte bulundu. Bu istek, Azure portalında gösterildiği gibi Uygulama Kimliği ve yerel uygulamanın yeniden yönlendirme URI'sini ve web API'si için uygulama kimliği URI'yi içerir. Kullanıcı oturum açmadıysa, yeniden oturum açmaları istenir
1. Azure AD kullanıcının kimliğini doğrular. Çok kiracılı bir uygulamaysa ve uygulamayı kullanmak için onay gerekiyorsa, kullanıcının bunu zaten yapmamışsa onay alması gerekir. Onay verdikten ve başarılı bir kimlik doğrulaması yaptıktan sonra Azure AD, istemci uygulamasının yeniden yönlendirme URI'sine bir yetkilendirme kodu yanıtı verir.
1. Azure AD yeniden yönlendirme URI'ye bir yetkilendirme kodu yanıtı verdiğinde, istemci uygulaması tarayıcı etkileşimini durdurur ve yetkilendirme kodunu yanıttan çıkarır. Bu yetkilendirme kodunu kullanarak, istemci uygulaması Azure AD'nin yetkilendirme kodunu, istemci uygulamasıyla ilgili ayrıntıları (Application ID ve yeniden yönlendirme URI) ve istenen kaynağı (uygulama kimliği URI için web API).
1. Yetkilendirme kodu ve istemci uygulaması ve web API'si hakkındaki bilgiler Azure AD tarafından doğrulanır. Azure AD, başarılı bir doğrulama üzerine iki belirteç döndürür: JWT erişim belirteci ve JWT yenileme belirteci. Ayrıca Azure AD, kullanıcı yla ilgili görüntü adı ve kiracı kimliği gibi temel bilgileri de sağlar.
1. HTTPS üzerinden, istemci uygulaması web API isteği Yetkilendirme üstbilgisinde bir "Taşıyıcı" atama ile JWT dize eklemek için döndürülen JWT erişim belirteci kullanır. Web API sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.
1. Erişim belirteci süresi dolduğunda, istemci uygulaması kullanıcının yeniden kimlik doğrulaması gerektiğini belirten bir hata alır. Uygulamanın geçerli bir yenileme belirteci varsa, kullanıcının yeniden oturum açmasını sormadan yeni bir erişim jetonu edinmek için kullanılabilir. Yenileme belirteci süresi dolursa, uygulamanın kullanıcıyı bir kez daha etkileşimli olarak doğrulaması gerekir.

> [!NOTE]
> Azure AD tarafından verilen yenileme belirteci birden çok kaynağa erişmek için kullanılabilir. Örneğin, iki web API'sini arama izni olan bir istemci uygulamanız varsa, yenileme belirteci diğer web API'sine erişim belirteci almak için de kullanılabilir.

## <a name="code-samples"></a>Kod örnekleri

Web API senaryolarına Yerel Uygulama için kod örneklerine bakın. Ve sık sık tekrar kontrol edin -- sık sık yeni örnekler ekliyoruz. [Web API'ya Yerel Uygulama](sample-v1-code.md#desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api).

## <a name="app-registration"></a>Uygulama kaydı

Bir uygulamayı Azure AD v1.0 bitiş noktasına kaydetmek için [bkz.](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)

* Tek kiracı - Hem yerel uygulama hem de web API'si Azure AD'de aynı dizine kaydedilmelidir. Web API, yerel uygulamanın kaynaklarına erişimini sınırlamak için kullanılan bir dizi izini ortaya çıkaracak şekilde yapılandırılabilir. İstemci uygulaması daha sonra Azure portalındaki "Diğer Uygulamalara İzinler" açılır menüsünden istenen izinleri seçer.
* Çok kiracı - İlk olarak, yerel uygulama yalnızca geliştirici veya yayıncının dizininde kayıtlıdır. İkinci olarak, yerel uygulama işlevsel olması için gereken izinleri belirtmek için yapılandırılır. Bu gerekli izin listesi, hedef dizindeki bir kullanıcı veya yönetici uygulamaya onay verdiğinde bir iletişim kutusunda gösterilir ve bu da uygulamayı kuruluşunun kullanımına sunar. Bazı uygulamalar yalnızca, kuruluştaki herhangi bir kullanıcının izin verebileceği kullanıcı düzeyinde izinler gerektirir. Diğer uygulamalar, kuruluştaki bir kullanıcının izin veremeyeceği yönetici düzeyinde izinler gerektirir. Yalnızca bir dizin yöneticisi, bu düzeyde izin gerektiren uygulamalara izin verebilir. Kullanıcı veya yönetici izin verdiğinde, yalnızca web API'si dizinde kaydedilir. 

## <a name="token-expiration"></a>Belirteç sona ermesi

Yerel uygulama, JWT erişim jetonu almak için yetkilendirme kodunu kullandığında, jwt yenileme belirteci de alır. Erişim belirteci süresi dolduğunda, yenileme belirteci yeniden oturum açmalarına gerek kalmadan kullanıcının yeniden kimliğini doğrulamak için kullanılabilir. Bu yenileme belirteci daha sonra kullanıcının kimliğini doğrulamak için kullanılır ve bu da yeni bir erişim belirteci ve yenibelirtere neden olur.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer Uygulama [türleri ve senaryoları](app-types.md) hakkında daha fazla bilgi edinin
- Azure AD [kimlik doğrulama temelleri](v1-authentication-scenarios.md) hakkında bilgi edinin
