---
title: Güvenlik belirteçleri | Mavisi
titleSuffix: Microsoft identity platform
description: Microsoft Identity platformunda güvenlik belirteçlerinin temelleri hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/11/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 6d9f5538d377be1414089e591559344bde4f381a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98795637"
---
# <a name="security-tokens"></a>Güvenlik belirteçleri

Merkezi bir kimlik sağlayıcısı, kullanıcıların, kuruluşun ağından oturum açması gerekmeyen dünya genelinde yer alan uygulamalar için yararlıdır. Microsoft Identity platform kullanıcıların kimliğini doğrular ve [erişim belirteçleri](developer-glossary.md#access-token), [yenileme belirteçleri](developer-glossary.md#refresh-token)ve [Kimlik belirteçleri](developer-glossary.md#id-token)gibi güvenlik belirteçleri sağlar. Güvenlik belirteçleri, bir [istemci uygulamanın](developer-glossary.md#client-application) bir [kaynak sunucusundaki](developer-glossary.md#resource-server)korumalı kaynaklara erişmesine izin verir.

**Erişim belirteci**: erişim belirteci, bir [OAuth 2,0](active-directory-v2-protocols.md) akışının parçası olarak bir [yetkilendirme sunucusu](developer-glossary.md#authorization-server) tarafından verilen bir güvenlik belirtecidir. Belirteç için tasarlanan Kullanıcı ve kaynakla ilgili bilgiler içerir. Bilgiler, Web API 'Lerine ve diğer korumalı kaynaklara erişmek için kullanılabilir. Erişim belirteçleri, bir istemci uygulamasına erişim izni vermek için kaynaklar tarafından onaylanır. Microsoft kimlik platformunun erişim belirteçleri hakkında daha fazla bilgi edinmek için bkz. [erişim belirteçleri](access-tokens.md).

**Belirteci Yenile**: erişim belirteçleri yalnızca kısa bir süre için geçerli olduğundan, yetkilendirme sunucuları bazen erişim belirteci verildiği sırada bir yenileme belirteci verir. İstemci uygulaması daha sonra gerektiğinde yeni bir erişim belirteci için bu yenileme belirtecini değiştirebilir. Microsoft Identity platformunun izinleri iptal etmek için yenileme belirteçleri kullanma hakkında daha fazla bilgi edinmek için bkz. [belirteç iptali](access-tokens.md#token-revocation).

**Kimlik belirteci**: Kimlik belirteçleri, [OpenID Connect](v2-protocols-oidc.md) akışının bir parçası olarak istemci uygulamasına gönderilir. Bunlar, bir erişim belirteci yerine veya bunlara gönderilebilir. KIMLIK belirteçleri, istemci tarafından kullanıcının kimliğini doğrulamak için kullanılır. Microsoft kimlik platformunun KIMLIK belirteçleri hakkında daha fazla bilgi edinmek için bkz. [Kimlik belirteçleri](id-tokens.md).

> [!NOTE]
> Bu makalede, OAuth2 ve OpenID Connect protokolleri tarafından kullanılan güvenlik belirteçleri ele alınmaktadır. Birçok kurumsal uygulama, kullanıcıların kimliğini doğrulamak için SAML kullanır. SAML onayları hakkında daha fazla bilgi için bkz. [Azure ACTIVE DIRECTORY SAML belirteci başvurusu](reference-saml-tokens.md).

## <a name="validate-security-tokens"></a>Güvenlik belirteçlerini doğrula

Bu, belirtecin oluşturulduğu uygulamaya, kullanıcının oturum açmış olduğu Web uygulamasına veya belirteci doğrulamak için çağrılan Web API 'sine göre yapılır. Belirteç, yetkilendirme sunucusu tarafından özel bir anahtarla imzalanır. Yetkilendirme sunucusu karşılık gelen ortak anahtarı yayımlar. Bir belirteci doğrulamak için, uygulama imzayı özel anahtar kullanılarak oluşturulduğunu doğrulamak için yetkilendirme sunucusu ortak anahtarını kullanarak imzayı doğrular.

Belirteçler yalnızca sınırlı bir süre için geçerlidir. Genellikle, yetkilendirme sunucusu, şu gibi bir çift belirteç sağlar:

* Uygulamaya veya korunan kaynağa erişen bir erişim belirteci.
* Erişim belirtecinin süresi dolmak üzere kapatıldığında erişim belirtecini yenilemek için kullanılan yenileme belirteci.

Erişim belirteçleri, üst bilgide taşıyıcı belirteci olarak bir Web API 'sine geçirilir `Authorization` . Bir uygulama, yetkilendirme sunucusuna yenileme belirteci sağlayabilir. Uygulamaya yönelik kullanıcı erişimi iptal edilmediği takdirde, yeni bir erişim belirteci ve yeni bir yenileme belirteci geri alır. Bu, kuruluşa ayrılmaya yönelik senaryonun işlenme yönteminden oluşur. Yetkilendirme sunucusu yenileme belirtecini aldığında, Kullanıcı artık yetkilendirilmezse, başka bir geçerli erişim belirteci vermez.

## <a name="json-web-tokens-and-claims"></a>JSON Web belirteçleri ve talepleri

Microsoft Identity platformu, *talepleri* Içeren JSON Web belirteçleri (JWTs) olarak güvenlik belirteçleri uygular. JWTs güvenlik belirteçleri olarak kullanıldığından, bu kimlik doğrulama biçimi bazen *JWT kimlik doğrulaması* olarak adlandırılır.

Bir [talep](developer-glossary.md#claim) , bir istemci uygulaması veya [kaynak sahibi](developer-glossary.md#resource-owner)gibi bir varlık hakkında, kaynak sunucusu gibi başka bir varlığa onay verir. Ayrıca bir talep, JWT talebi veya JSON Web Token talebi olarak da adlandırılabilir.

Talepler, belirteç konusuyla ilgili olgu geçişi yapan ad veya değer çiftleridir. Örneğin, bir talep yetkilendirme sunucusu tarafından kimliği doğrulanan güvenlik sorumlusu hakkında olgu içerebilir. Belirli bir belirteçte mevcut talepler, belirteç türü, konunun kimliğini doğrulamak için kullanılan kimlik bilgisi türü ve uygulama yapılandırması gibi birçok şeyi temel alır.

Uygulamalar, şunları yapmak gibi çeşitli görevler için talepler kullanabilir:

* Belirteci doğrulayın.
* Belirteç konusunun [kiracı](developer-glossary.md#tenant)kimliğini belirler.
* Kullanıcı bilgilerini görüntüleyin.
* Konunun yetkilendirmesini belirleme.

Bir talep, şu gibi bilgiler sağlayan anahtar-değer çiftlerinden oluşur:

* Belirteci oluşturan güvenlik belirteci sunucusu.
* Belirtecin oluşturulduğu tarih.
* Konu (Kullanıcı--Daemon 'ları dışında).
* Belirtecin oluşturulduğu uygulama olan hedef kitle.
* Belirteç için istenen uygulama (istemci). Web uygulamaları söz konusu olduğunda, bu uygulama hedef kitle ile aynı olabilir.

Microsoft Identity platformunun belirteçleri ve talep bilgilerini nasıl uyguladığı hakkında daha fazla bilgi edinmek için bkz. [erişim belirteçleri](access-tokens.md) ve [Kimlik belirteçleri](id-tokens.md).

## <a name="how-each-flow-emits-tokens-and-codes"></a>Her akışın belirteçleri ve kodları nasıl yayar

İstemcinizin nasıl oluşturulduğuna bağlı olarak, Microsoft Identity platform tarafından desteklenen kimlik doğrulama akışlarının birini (veya birkaçını) kullanabilir. Bu akışlar çeşitli belirteçler (KIMLIK belirteçleri, yenileme belirteçleri, erişim belirteçleri) ve yetkilendirme kodları oluşturabilir. Bunların çalışmasını sağlamak için farklı belirteçler olmaları gerekir. Bu tabloda genel bakış sunulmaktadır.

|Akış | Gerektirmeyen | KIMLIK belirteci | Erişim belirteci | Belirteci Yenile | Yetkilendirme kodu |
|-----|----------|----------|--------------|---------------|--------------------|
|[Yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md) | | x | x | x | x|
|[Örtük akış](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Karma OıDC akışı](v2-protocols-oidc.md#protocol-diagram-access-token-acquisition)| | x  | |          |            x   |
|[Belirteç satın alma yenileme](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | Belirteci Yenile | x | x | x| |
|[On-behalf-of akışı](v2-oauth2-on-behalf-of-flow.md) | Erişim belirteci| x| x| x| |
|[İstemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md) | | | x (yalnızca uygulama)| | |

Örtülü mod aracılığıyla verilen belirteçlerin, URL aracılığıyla tarayıcıya geri geçirildiğinden bir uzunluk sınırlaması vardır; burada `response_mode` `query` veya olur `fragment` . Bazı tarayıcılarda, tarayıcı çubuğuna koyabileceğiniz ve çok uzun olduğunda başarısız olan URL 'nin boyut sınırı vardır. Sonuç olarak, bu belirteçlerin `groups` veya talepleri yoktur `wids` .

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Identity platformunda kimlik doğrulama ve yetkilendirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* Kimlik doğrulama ve yetkilendirme ile ilgili temel kavramlar hakkında bilgi edinmek için bkz. [kimlik doğrulaması ve yetkilendirme](authentication-vs-authorization.md).
* Uygulamanızı tümleştirme için kaydetme hakkında bilgi edinmek için bkz. [uygulama modeli](application-model.md).
* Web, masaüstü ve mobil uygulamaların oturum açma akışı hakkında bilgi edinmek için bkz. [uygulama oturum açma akışı](app-sign-in-flow.md).
