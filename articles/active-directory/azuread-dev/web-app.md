---
title: Azure Active Directory'deki Web uygulamaları
description: Web uygulamalarının ne olduğunu ve bu uygulama türü için protokol akışı, kayıt ve belirteç sona erme ile ilgili temel bilgileri açıklar.
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
ms.openlocfilehash: e4a7fb72d40f5db65e8e30264e9d68b2727749e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154415"
---
# <a name="web-apps"></a>Web uygulamaları

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Web uygulamaları, bir web tarayıcısındaki bir kullanıcıyı bir web uygulamasına doğrulayan uygulamalardır. Bu senaryoda, web uygulaması kullanıcının tarayıcısını Azure AD'de oturum açmaları için yönlendirir. Azure AD, kullanıcının tarayıcısı aracılığıyla, güvenlik jetonundaki kullanıcı yla ilgili iddiaları içeren bir oturum açma yanıtı döndürür. Bu senaryo, OpenID Connect, SAML 2.0 ve WS-Federation protokollerini kullanarak oturum açmayı destekler.

## <a name="diagram"></a>Diyagram

![Web uygulamasına tarayıcı için kimlik doğrulama akışı](./media/authentication-scenarios/web-browser-to-web-api.png)

## <a name="protocol-flow"></a>Protokol akışı

1. Bir kullanıcı uygulamayı ziyaret ettiğinde ve oturum açması gerektiğinde, oturum açma isteği yle Azure AD'deki kimlik doğrulama bitiş noktasına yönlendirilir.
1. Kullanıcı oturum açma sayfasında oturum açsın.
1. Kimlik doğrulama başarılı olursa, Azure AD bir kimlik doğrulama belirteci oluşturur ve uygulamanın Azure portalında yapılandırılan Yanıt URL'sine bir oturum açma yanıtı döndürür. Bir üretim uygulaması için bu Yanıt URL'si HTTPS olmalıdır. İade edilen belirteç, kullanıcı ve Azure AD ile ilgili olarak uygulamanın belirteci doğrulamak için gerekli olan taleplerini içerir.
1. Uygulama, Azure AD için federasyon meta veri belgesinde bulunan genel imza anahtarı ve veren bilgilerini kullanarak belirteci doğrular. Uygulama belirteci doğruladıktan sonra, kullanıcı ile yeni bir oturum başlar. Bu oturum, kullanıcının süresi dolana kadar uygulamaya erişmesine olanak tanır.

## <a name="code-samples"></a>Kod örnekleri

Web uygulaması senaryolarına web tarayıcısı için kod örneklerine bakın. Ve, yeni örnekler sık sık eklendikçe sık sık kontrol edin.

## <a name="app-registration"></a>Uygulama kaydı

Bir web uygulamasını kaydetmek için [bkz.](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)

* Tek kiracı - Kuruluşunuz için bir uygulama oluşturuyorsanız, Azure portalını kullanarak şirketinizin dizinine kaydedilmelidir.
* Çok kiracılı - Kuruluşunuz dışındaki kullanıcılar tarafından kullanılabilecek bir uygulama oluşturuyorsanız, bu uygulamanın şirketinizin dizinine kaydedilmesi, ancak uygulamayı kullanacak her kuruluşun dizinine kaydedilmesi gerekir. Uygulamanızı kendi dizininde kullanılabilir hale getirmek için, müşterilerinizin başvurunuzu kabul etmelerini sağlayan bir kayıt işlemi ekleyebilirsiniz. Uygulamanız için kaydolduklarında, bunlara uygulamanın gerektirdiği izinleri ve ardından onay seçeneğini gösteren bir iletişim kutusu sunulur. Gerekli izinlere bağlı olarak, diğer kuruluştaki bir yöneticinin izin vermesi gerekebilir. Kullanıcı veya yönetici izin verdiğinde, uygulama kendi dizininde kaydedilir.

## <a name="token-expiration"></a>Belirteç sona ermesi

Azure AD tarafından verilen belirteç lerin kullanım ömrü sona erdiğinde kullanıcının oturumunun süresi dolur. Uygulamanız, kullanıcılarını bir hareketsizlik dönemine göre devre dışı anlaşma kullanabilirsiniz. Oturumun süresi dolduğunda, kullanıcıdan yeniden oturum açması istenir.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer Uygulama [türleri ve senaryoları](app-types.md) hakkında daha fazla bilgi edinin
* Azure AD [kimlik doğrulama temelleri](v1-authentication-scenarios.md) hakkında bilgi edinin
