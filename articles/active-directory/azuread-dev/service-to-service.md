---
title: Azure Active Directory'de hizmete hizmet uygulamaları
description: Bu uygulama türü için hangi hizmet-servis uygulamalarını ve protokol akışı, kayıt ve belirteç sona erme ile ilgili temel bilgileri açıklar.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 179034533d90dbbb6ca362fc6f72996f32873729
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154772"
---
# <a name="service-to-service-apps"></a>Servise servis uygulamaları

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Servise hizmet uygulamaları, bir web API'sinden kaynak alması gereken bir daemon veya sunucu uygulaması olabilir. Bu bölüme uygulanan iki alt senaryo vardır:

- OAuth 2.0 istemci kimlik bilgileri hibe türü üzerine inşa edilmiş bir web API aramak için gereken bir daemon

    Bu senaryoda, birkaç şeyi anlamak önemlidir. İlk olarak, uygulamanın kendi kimliğine sahip olmasını gerektiren bir daemon uygulaması ile kullanıcı etkileşimi mümkün değildir. Daemon uygulamasına örnek olarak toplu iş veya arka planda çalışan bir işletim sistemi hizmeti örnektir. Bu tür bir uygulama, uygulama kimliğini kullanarak ve Uygulama Kimliğini, kimlik bilgilerini (parola veya sertifika) ve uygulama kimliği URI'yi Azure AD'ye sunarak bir erişim belirteci ister. Başarılı kimlik doğrulamasından sonra daemon, Azure AD'den bir erişim jetonu alır ve bu jeton web API'sını aramak için kullanılır.

- OAuth 2.0 On-Behalf-Of taslak belirtimi üzerine inşa edilmiş bir web API'si çağırması gereken bir sunucu uygulaması (web API gibi)

    Bu senaryoda, bir kullanıcının yerel bir uygulamada kimlik doğrulaması yaptığını ve bu yerel uygulamanın bir web API'sini çağırması gerektiğini düşünün. Azure AD, web API'sını aramak için Bir JWT erişim belirteci yayınlar. Web API'sinin başka bir akış aşağı web API'sini çağırması gerekiyorsa, kullanıcının kimliğini devretmek ve ikinci katman web API'sine kimlik doğrulamak için akış adına kullanabilir.

## <a name="diagram"></a>Diyagram

![Web API diyagramına Daemon veya Sunucu Uygulaması](./media/authentication-scenarios/daemon-server-app-to-web-api.png)

## <a name="protocol-flow"></a>Protokol akışı

### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>OAuth 2.0 müşteri kimlik bilgileri hibe ile başvuru kimliği

1. İlk olarak, sunucu uygulamasının etkileşimli oturum açma iletişim kutusu gibi herhangi bir insan etkileşimi olmadan Azure AD ile kimliği doğrulanması gerekir. Kimlik bilgisi, Uygulama Kimliği ve uygulama kimliği URI'yi sağlayarak Azure AD'nin belirteç bitiş noktasına bir istekte bulunarak bu isteği yapar.
1. Azure AD uygulamanın kimliğini doğrular ve web API'sını aramak için kullanılan bir JWT erişim belirteci döndürür.
1. HTTPS üzerinden, web uygulaması, web API'ya isteğin Yetkilendirme başlığında "Taşıyıcı" ibaresi bulunan JWT dizesini eklemek için döndürülen JWT erişim belirteci kullanır. Web API sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>OAuth 2.0 Taslak Şartnamesi Ile Temsilci kullanıcı kimliği

Aşağıda tartışılan akış, bir kullanıcının başka bir uygulamada (yerel bir uygulama gibi) kimlik doğrulandığını ve kullanıcı kimliğinin birinci katmanweb API'sine erişim belirteci elde etmek için kullanıldığını varsayar.

1. Yerel uygulama, erişim belirtecisini birinci katmanweb API'sine gönderir.
1. Birinci katmandaki web API'si, Azure AD'nin belirteç bitiş noktasına, Uygulama Kimliği ve kimlik bilgilerinin yanı sıra kullanıcının erişim belirteci sağlayan bir istek gönderir. Ayrıca, istek, web API'sinin orijinal kullanıcı adına bir akış aşağı web API'sını çağırmak için yeni belirteçler istediğini belirten on_behalf_of bir parametreyle gönderilir.
1. Azure AD, birinci katman web API'sine erişmek için birinci katman web API izinlerine sahip olduğunu doğrular ve isteği doğrulayarak JWT erişim jetonunu ve Birinci katman web API'sine JWT yenileme belirteci döndürer.
1. HTTPS üzerinden, birinci katman web API'si, istekte Yetkilendirme üstbilgisine belirteç dizesini ekleyerek ikinci katman web API'sını çağırır. Birinci katmanweb API'si, erişim belirteci ve yenileme belirteçleri geçerli olduğu sürece ikinci katman web API'sını aramaya devam edebilir.

## <a name="code-samples"></a>Kod örnekleri

Daemon veya Sunucu Uygulaması için Kod örneklerine bakın Web API senaryoları: [Sunucu veya Web API'ye Daemon Uygulaması](sample-v1-code.md#daemon-applications-accessing-web-apis-with-the-applications-identity)

## <a name="app-registration"></a>Uygulama kaydı

* Tek kiracı - Hem uygulama kimliği hem de temsilci kullanıcı kimliği örnekleri için, daemon veya sunucu uygulamasının Azure AD'de aynı dizine kaydedilmesi gerekir. Web API' si, daemon veya sunucunun kaynaklarına erişimini sınırlamak için kullanılan bir dizi izini ortaya çıkaracak şekilde yapılandırılabilir. Devredilen bir kullanıcı kimliği türü kullanılıyorsa, sunucu uygulamasının istenen izinleri seçmesi gerekir. Uygulama kaydı için **API İzin** sayfasında, izin **ekle'yi** seçtikten ve API ailesini seçtikten sonra, **Temsilci lik izinlerini**seçin ve ardından izinlerinizi seçin. Uygulama kimlik türü kullanılıyorsa bu adım gerekli değildir.
* Çok kiracılı - İlk olarak, daemon veya sunucu uygulaması işlevsel olması için gereken izinleri belirtmek için yapılandırılır. Bu gerekli izin listesi, hedef dizindeki bir kullanıcı veya yönetici uygulamaya onay verdiğinde bir iletişim kutusunda gösterilir ve bu da uygulamayı kuruluşunun kullanımına sunar. Bazı uygulamalar yalnızca, kuruluştaki herhangi bir kullanıcının izin verebileceği kullanıcı düzeyinde izinler gerektirir. Diğer uygulamalar, kuruluştaki bir kullanıcının izin veremeyeceği yönetici düzeyinde izinler gerektirir. Yalnızca bir dizin yöneticisi, bu düzeyde izin gerektiren uygulamalara izin verebilir. Kullanıcı veya yönetici izin verdiğinde, web API'lerinin her ikisi de kendi dizinde kaydedilir.

## <a name="token-expiration"></a>Belirteç sona ermesi

İlk uygulama bir JWT erişim belirteci almak için yetkilendirme kodunu kullandığında, aynı zamanda bir JWT yenileme belirteci alır. Erişim belirteci süresi dolduğunda, yenileme belirteci kimlik bilgileri istenmeden kullanıcının yeniden kimlik doğrulaması için kullanılabilir. Bu yenileme belirteci daha sonra kullanıcının kimliğini doğrulamak için kullanılır ve bu da yeni bir erişim belirteci ve yenibelirtere neden olur.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer Uygulama [türleri ve senaryoları](app-types.md) hakkında daha fazla bilgi edinin
- Azure AD [kimlik doğrulama temelleri](v1-authentication-scenarios.md) hakkında bilgi edinin
