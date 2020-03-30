---
title: Azure Active Directory'de tek sayfalı uygulamalar
description: Tek sayfalı uygulamaların (SPA'ların) ne olduğunu ve bu uygulama türü için protokol akışı, kayıt ve belirteç sona erme ile ilgili temel bilgileri açıklar.
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
ms.openlocfilehash: adf3c5b5cd40a9ea3f07ba9c92cfc4544ca60f1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154755"
---
# <a name="single-page-applications"></a>Tek sayfauygulamaları

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Tek sayfalı uygulamalar (SPA'lar) genellikle tarayıcıda çalışan bir JavaScript sunu katmanı (ön uç) ve sunucuda çalışan ve uygulamanın iş mantığını uygulayan bir web API arka ucu olarak yapılandırılır. Örtülü yetkilendirme hibesi hakkında daha fazla bilgi edinmek ve uygulama senaryonuz için doğru olup olmadığına karar vermenize yardımcı olmak için [bkz.](v1-oauth2-implicit-grant-flow.md)

Bu senaryoda, kullanıcı kaydolduğunda, JavaScript ön uç [JavaScript için Active Directory Authentication Library kullanır (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) ve Azure AD'den kimlik belirteci (id_token) almak için örtülü yetkilendirme hibesi. Belirteç önbelleğe alınmış ve istemci, OWIN ara yazılım kullanılarak güvenli olan Web API arka ucuna arama yaparken taşıyıcı belirteci olarak isteğe bağlanır.

## <a name="diagram"></a>Diyagram

![Tek sayfalık uygulama diyagramı](./media/authentication-scenarios/single-page-app.png)

## <a name="protocol-flow"></a>Protokol akışı

1. Kullanıcı web uygulamasına yönlendirin.
1. Uygulama JavaScript ön ucunu (sunu katmanı) tarayıcıya döndürür.
1. Kullanıcı oturum açma işlemini başlatır, örneğin oturum açma bağlantısını tıklatarak. Tarayıcı, kimlik belirteci istemek için Azure AD yetkilendirme bitiş noktasına get gönderir. Bu istek, sorgu parametrelerindeki uygulama kimliğini ve yanıt URL'sini içerir.
1. Azure AD, Yanıt URL'sini Azure portalında yapılandırılan kayıtlı Yanıt URL'si ile karşılatır.
1. Kullanıcı oturum açma sayfasında oturum açsın.
1. Kimlik doğrulama başarılı olursa, Azure AD bir kimlik belirteci oluşturur ve bunu bir URL parçası (#) olarak uygulamanın YanıtURL'sine döndürür. Bir üretim uygulaması için bu Yanıt URL'si HTTPS olmalıdır. İade edilen belirteç, kullanıcı ve Azure AD ile ilgili olarak uygulamanın belirteci doğrulamak için gerekli olan taleplerini içerir.
1. Tarayıcıda çalışan JavaScript istemci kodu, uygulamanın web API arka ucuna yapılan çağrıları n güvenliğini sağlamada kullanılacak yanıttan belirteci ayıklar.
1. Tarayıcı, yetkilendirme başlığındaki kimlik belirteciyle uygulamanın web API'sını arka uca çağırır. Azure AD kimlik doğrulama hizmeti, kaynak istemci kimliğiyle aynıysa taşıyıcı belirteci olarak kullanılabilecek bir kimlik belirteci verir (bu durumda, web API'sı uygulamanın kendi arka ucu olduğu için bu doğrudur).

## <a name="code-samples"></a>Kod örnekleri

Tek [sayfalı uygulama senaryoları için kod örneklerine](sample-v1-code.md#single-page-applications)bakın. Yeni örnekler sık sık eklendikçe sık sık tekrar kontrol ettiğinizden emin olun.

## <a name="app-registration"></a>Uygulama kaydı

* Tek kiracı - Kuruluşunuz için bir uygulama oluşturuyorsanız, Azure portalını kullanarak şirketinizin dizinine kaydedilmelidir.
* Çok kiracılı - Kuruluşunuz dışındaki kullanıcılar tarafından kullanılabilecek bir uygulama oluşturuyorsanız, bu uygulamanın şirketinizin dizinine kaydedilmesi, ancak uygulamayı kullanacak her kuruluşun dizinine kaydedilmesi gerekir. Uygulamanızı kendi dizininde kullanılabilir hale getirmek için, müşterilerinizin başvurunuzu kabul etmelerini sağlayan bir kayıt işlemi ekleyebilirsiniz. Uygulamanız için kaydolduklarında, bunlara uygulamanın gerektirdiği izinleri ve ardından onay seçeneğini gösteren bir iletişim kutusu sunulur. Gerekli izinlere bağlı olarak, diğer kuruluştaki bir yöneticinin izin vermesi gerekebilir. Kullanıcı veya yönetici izin verdiğinde, uygulama kendi dizininde kaydedilir.

Uygulamayı kaydettikten sonra, OAuth 2.0 örtülü hibe protokolünü kullanacak şekilde yapılandırılmalıdır. Varsayılan olarak, bu protokol uygulamalar için devre dışı bırakılır. Uygulamanız için OAuth2 örtülü hibe protokolünü etkinleştirmek için uygulama bildirimini Azure portalından düzenleyin ve "oauth2AllowImplicitFlow" değerini doğru olarak ayarlayın. Daha fazla bilgi için [Uygulama bildirimine](../develop/reference-app-manifest.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)bakın.

## <a name="token-expiration"></a>Belirteç sona ermesi

ADAL.js kullanarak yardımcı olur:

* Süresi dolmuş bir belirteci yenileme
* Web API kaynağını aramak için erişim belirteci isteme

Azure AD, başarılı bir kimlik doğrulamasından sonra, oturum oluşturmak için kullanıcının tarayıcısına bir çerez yazar. Oturumun kullanıcı ile Azure AD arasında (kullanıcı ile web uygulaması arasında değil) olduğunu unutmayın. Bir belirteç sona erdiğinde, ADAL.js sessizce başka bir belirteç elde etmek için bu oturumu kullanır. ADAL.js, OAuth örtülü hibe protokolünü kullanarak isteği göndermek ve almak için gizli bir iFrame kullanır. ADAL.js, bu kaynakların çapraz kaynak paylaşımını (CORS) desteklediği, kullanıcının dizininde kayıtlı olduğu ve gerekli izinlerin kullanıldığı sürece, uygulamanın çağırdığı diğer web API kaynakları için sessizce erişim belirteçleri elde etmek için de aynı mekanizmayı kullanabilir. oturum açma sırasında kullanıcı tarafından verilir.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer Uygulama [türleri ve senaryoları](app-types.md) hakkında daha fazla bilgi edinin
* Azure AD [kimlik doğrulama temelleri](v1-authentication-scenarios.md) hakkında bilgi edinin
