---
title: Güvenlik belirteçleri | Mavisi
titleSuffix: Microsoft identity platform
description: Microsoft Identity platform (v 2.0) içindeki güvenlik belirteçlerinin temelleri hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/28/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: faaf4a9c4fe37bc184b9860390f1eb99eede035c
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584291"
---
# <a name="security-tokens"></a>Güvenlik belirteçleri

Merkezi bir kimlik sağlayıcısı özellikle, dünyanın dört bir yanındaki, kuruluşun ağından oturum açması gerekmeyen kullanıcılar için yararlıdır. Microsoft Identity platform kullanıcıların kimliğini doğrular ve bir [istemci uygulamasının](developer-glossary.md#client-application) bir [kaynak sunucusundaki](developer-glossary.md#resource-server)korumalı kaynaklara erişmesine izin veren [erişim belirteci](developer-glossary.md#access-token), [yenileme belirteci](developer-glossary.md#refresh-token)ve [kimlik belirteci](developer-glossary.md#id-token)gibi güvenlik belirteçleri sağlar.

**Erişim belirteçleri** , bir [OAuth 2,0](active-directory-v2-protocols.md) akışının parçası olarak bir [yetkilendirme sunucusu](developer-glossary.md#authorization-server) tarafından verilen bir güvenlik belirtecidir. Kullanıcının ve belirtecin hedeflenen uygulamayla ilgili bilgiler içerir; Web API 'Lerine ve diğer korumalı kaynaklara erişmek için kullanılabilir. Microsoft Identity platform 'un erişim belirteçleri hakkında daha fazla bilgi edinmek için bkz. [erişim belirteçleri](access-tokens.md).

Erişim belirteçleri yalnızca kısa bir süre için geçerlidir. bu nedenle, yetkilendirme sunucuları bazen erişim belirteci verildiği sırada **yenileme belirteçleri** verir. İstemci uygulaması daha sonra gerektiğinde yeni bir erişim belirteci için bu yenileme belirtecini değiştirebilir. Microsoft Identity platformunun izinleri iptal etmek için yenileme belirteçleri kullanma hakkında daha fazla bilgi edinmek için bkz. [belirteç iptali](access-tokens.md#token-revocation).

**Kimlik belirteçleri** , bir [OpenID Connect](v2-protocols-oidc.md) akışının parçası olarak istemci uygulamasına gönderilir. Bunlar, bir erişim belirteci yerine ya da üzerinden gönderilebilir ve istemci tarafından kullanıcının kimliğini doğrulamak için kullanılır. Microsoft kimlik platformunun KIMLIK belirteçleri hakkında daha fazla bilgi edinmek için bkz. [Kimlik belirteçleri](id-tokens.md).

## <a name="validating-security-tokens"></a>Güvenlik belirteçleri doğrulanıyor

Belirteç doğrulamak için, belirtecin oluşturulduğu uygulamaya, kullanıcının oturum açmış olduğu Web uygulamasına veya Web API 'sinin çağrılmakta olduğu uygulamaya kadar olur. Belirteç, **güvenlik belirteci sunucusu (STS)** tarafından özel anahtarla imzalanır. STS ilgili ortak anahtarı yayımlar. Bir belirteci doğrulamak için, uygulama, imzayı özel anahtar kullanılarak oluşturulduğunu doğrulamak üzere STS ortak anahtarını kullanarak imzayı doğrular.

Belirteçler yalnızca sınırlı bir süre için geçerlidir. STS genellikle bir çift belirteç sağlar:

* Uygulamaya veya korunan kaynağa erişmek için bir erişim belirteci ve
* Erişim belirtecinin süresi dolmak üzere kapatıldığında erişim belirtecini yenilemek için kullanılan yenileme belirteci.

Erişim belirteçleri, `Authorization` üst bilgide taşıyıcı belirteci olarak BIR Web API 'sine geçirilir. Bir uygulama STS 'ye yenileme belirteci sağlayabilir ve uygulamaya yönelik kullanıcı erişimi iptal edilmediği takdirde, yeni bir erişim belirteci ve yeni bir yenileme belirteci geri alır. Bu, kuruluşa ayrılmaya yönelik senaryonun işlenme yönteminden oluşur. STS yenileme belirtecini aldığında, Kullanıcı artık yetkilendirilmezse başka bir geçerli erişim belirteci vermez.

## <a name="json-web-tokens-jwts-and-claims"></a>JSON Web belirteçleri (JWTs) ve talepleri

Microsoft Identity platformu, **talepler**Içeren **JSON Web belirteçleri (jwts)** olarak güvenlik belirteçleri uygular.

Bir [talep](developer-glossary.md#claim) , bir istemci uygulaması veya [kaynak sahibi](developer-glossary.md#resource-owner)gibi bir varlık hakkında, kaynak sunucusu gibi başka bir varlığa onay verir.

Talepler, belirteç konusuyla ilgili olgu geçişi yapan ad/değer çiftleridir. Örneğin, bir talep yetkilendirme sunucusu tarafından kimliği doğrulanan güvenlik sorumlusu hakkında olgu içerebilir. Belirli bir belirteçte mevcut talepler, belirteç türü, konunun kimliğini doğrulamak için kullanılan kimlik bilgisi türü, uygulama yapılandırması vb. dahil olmak üzere birçok konuya bağlıdır.

Uygulamalar, şu gibi çeşitli görevler için talepler kullanabilir:

* Belirteç doğrulanıyor
* Belirteç konusunun [kiracısı](developer-glossary.md#tenant) tanımlanıyor
* Kullanıcı bilgilerini görüntüleme
* Konunun yetkilendirmesini belirleme

Bir talep, şu gibi bilgiler sağlayan anahtar-değer çiftlerinden oluşur:

* Belirteci oluşturan güvenlik belirteci sunucusu
* Belirtecin oluşturulduğu tarih
* Konu (Kullanıcı--Daemon 'ları dışında)
* Belirtecin oluşturulduğu uygulama olan hedef kitle
* Belirteç için istenen uygulama (istemci). Web uygulamaları söz konusu olduğunda, bu, hedef kitle ile aynı olabilir

Microsoft kimlik platformunun belirteçleri ve talep bilgilerini nasıl uyguladığı hakkında daha fazla bilgi edinmek için bkz. [erişim belirteçleri](access-tokens.md) ve [Kimlik belirteçleri](id-tokens.md).

## <a name="how-each-flow-emits-tokens-and-codes"></a>Her akışın belirteçleri ve kodları nasıl yayar

İstemcinizin nasıl oluşturulduğuna bağlı olarak, Microsoft Identity platform tarafından desteklenen kimlik doğrulama akışlarının birini (veya birkaçını) kullanabilir. Bu akışlar birçok belirteç (KIMLIK belirteçleri, yenileme belirteçleri, erişim belirteçleri) ve yetkilendirme kodları oluşturabilir ve bunların çalışmasını sağlamak için farklı belirteçler gerektirebilir. Bu grafik genel bakış sağlar:

|Akış | Gerektirmeyen | KIMLIK belirteci | erişim belirteci | belirteci Yenile | yetkilendirme kodu |
|-----|----------|----------|--------------|---------------|--------------------|
|[Yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md) | | x | x | x | x|
|[Örtük akış](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Karma OıDC akışı](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Belirteç satın alma yenileme](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | belirteci Yenile | x | x | x| |
|[On-behalf-of akışı](v2-oauth2-on-behalf-of-flow.md) | erişim belirteci| x| x| x| |
|[İstemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md) | | | x (yalnızca uygulama)| | |

Örtülü mod aracılığıyla yayınlanan belirteçlerin, tarayıcıya URL aracılığıyla geri geçirilme nedeniyle bir uzunluk sınırlaması vardır (burada `response_mode` `query` veya `fragment`).  Bazı tarayıcıların, tarayıcı çubuğuna koyabileceğiniz ve çok uzun olduğunda başarısız olan URL 'nin boyutunda bir sınırı vardır.  Bu nedenle, bu belirteçlerin `groups` veya `wids` talepleri yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Kimlik doğrulama ve yetkilendirme temellerini kapsayan diğer konular için:

* Microsoft Identity platform 'da kimlik doğrulaması ve yetkilendirmenin temel kavramları hakkında bilgi edinmek için bkz [. kimlik doğrulaması ve yetkilendirme karşılaştırması](authentication-vs-authorization.md) .
* Uygulamanızı Microsoft Identity platform ile tümleştirilebilen şekilde kaydetme süreci hakkında bilgi edinmek için bkz. [uygulama modeli](application-model.md) .
* Microsoft Identity platformunda Web, masaüstü ve mobil uygulamaların oturum açma akışı hakkında bilgi edinmek için bkz. [uygulama oturum açma akışı](app-sign-in-flow.md) .
