---
title: OAuth 2.0 ve OpenID Connect protokolleri - Microsoft kimlik platformu | Azure
description: Microsoft kimlik platformu bitiş noktası tarafından desteklenen OAuth 2.0 ve OpenID Connect protokolleri kılavuzu.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: e94bdd6e79ff13d8ba736e140538bae74091f727
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680083"
---
# <a name="oauth-20-and-openid-connect-protocols-on-the-microsoft-identity-platform"></a>Microsoft kimlik platformunda OAuth 2.0 ve OpenID Connect protokolleri

Endüstri standardı protokolleri, OpenID Connect ve OAuth 2.0 ile hizmet olarak kimlik için Microsoft kimlik platformu bitiş noktası. Hizmet standartlara uygun olsa da, bu protokollerin herhangi iki uygulaması arasında ince farklar olabilir. Burada bilgiler, doğrudan HTTP isteklerini göndererek ve işleyerek veya [açık kaynak kitaplıklarımızdan](reference-v2-libraries.md)birini kullanmak yerine üçüncü taraf açık kaynak kitaplığını kullanarak kodunuzu yazmayı seçerseniz yararlı olacaktır.

## <a name="the-basics"></a>Temel bilgiler

Hemen hemen tüm OAuth 2.0 ve OpenID Connect akışlarında, değişimde yer alan dört taraf vardır:

![OAuth 2.0 rollerini gösteren diyagram](./media/active-directory-v2-flows/protocols-roles.svg)

* **Yetkilendirme Sunucusu,** Microsoft kimlik platformubitiş noktasıdır ve kullanıcının kimliğini sağlamaktan, kaynaklara erişim sağlamaktan ve iptal etmekten ve belirteçler vermekten sorumludur. Kimlik sağlayıcısı olarak da bilinen yetkilendirme sunucusu, kullanıcının bilgileri, erişimleri ve bir akıştaki taraflar arasındaki güven ilişkileriyle ilgili her şeyi güvenli bir şekilde işler.
* **Kaynak Sahibi** genellikle son kullanıcıdır. Verilere sahip olan ve üçüncü tarafların bu verilere veya kaynağa erişmesine izin verme yetkisine sahip olan taraftır.
* **OAuth İstemcisi,** uygulama kimliğiyle tanımlanan uygulamanızdır. OAuth istemcisi genellikle son kullanıcının etkileşimde olduğu taraftır ve yetkilendirme sunucusundan belirteçler ister. İstemciye kaynak sahibi tarafından kaynağa erişim izni verilmelidir.
* **Kaynak Sunucusu,** kaynağın veya verilerin bulunduğu yerdir. OAuth İstemci'nin güvenli bir şekilde kimlik doğrulaması ve yetkilendirilmesi için Yetkilendirme Sunucusuna güvenir ve bir kaynağa erişimin sağlanabileceğinden emin olmak için Taşıyıcı erişim belirteçlerini kullanır.

## <a name="app-registration"></a>Uygulama kaydı

Kişisel ve iş veya okul hesaplarını kabul etmek isteyen her uygulamanın, bu kullanıcıları OAuth 2.0 veya OpenID Connect kullanarak oturum açabilmesi için [Azure portalındaki](https://aka.ms/appregistrations) **Uygulama kayıtları** deneyimi aracılığıyla kaydedilmesi gerekir. Uygulama kayıt işlemi, uygulamanız için birkaç değer toplar ve atar:

* Uygulamanızı benzersiz olarak tanımlayan bir **Uygulama Kimliği**
* Yanıtları uygulamanıza yönlendirmek için kullanılabilecek Bir **Yönlendirme URI** (isteğe bağlı)
* Birkaç diğer senaryoya özgü değerler.

Daha ayrıntılı bilgi için [uygulama kaydetmeyi](quickstart-register-app.md) öğrenin.

## <a name="endpoints"></a>Uç Noktalar

Uygulama, kaydolduktan sonra, istekleri bitiş noktasına göndererek Microsoft kimlik platformu yla iletişim kurar:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Dört `{tenant}` farklı değerden birini alabilecekleri yer:

| Değer | Açıklama |
| --- | --- |
| `common` | Azure AD'den hem kişisel Microsoft hesapları hem de iş/okul hesapları olan kullanıcıların uygulamada oturum açmalarına olanak tanır. |
| `organizations` | Yalnızca Azure AD'den iş/okul hesabı olan kullanıcıların uygulamaya oturum açmasına olanak tanır. |
| `consumers` | Yalnızca kişisel Microsoft hesapları (MSA) olan kullanıcıların uygulamada oturum açmasına izin verir. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` | Yalnızca belirli bir Azure AD kiracısından iş/okul hesabı olan kullanıcıların uygulamada oturum açmasına olanak tanır. Azure AD kiracısının arkadaş çaları veya kiracının GUID tanımlayıcısı kullanılabilir. |

Bu uç noktalarla nasıl etkileşimde ebileceğinizi öğrenmek için [Protokoller](#protocols) bölümünde belirli bir uygulama türünü seçin ve daha fazla bilgi için bağlantıları izleyin.

> [!TIP]
> Azure AD'de kayıtlı herhangi bir uygulama, kişisel hesaplarda oturum açmasalar bile Microsoft kimlik platformu bitiş noktasını kullanabilir.  Bu şekilde, varolan uygulamaları, uygulamanızı yeniden oluşturmadan Microsoft kimlik platformuna ve [MSAL'a](reference-v2-libraries.md) geçirebilirsiniz.

## <a name="tokens"></a>Belirteçler

OAuth 2.0 ve OpenID Connect'in Microsoft kimlik platformu uygulaması, JWT olarak temsil edilen taşıyıcı belirteçleri de dahil olmak üzere taşıyıcı belirteçlerini kapsamlı bir şekilde kullanır. Taşıyıcı belirteci, korunan bir kaynağa "taşıyıcı" erişimi sağlayan hafif bir güvenlik belirtecidir. Bu anlamda, "taşıyıcı" belirteci sunabilir herhangi bir partidir. Bir tarafın taşıyıcı belirteci almak için önce Microsoft kimlik platformunda kimlik doğrulaması yapması gerekse de, iletim ve depolamadaki belirteci güvence altına almak için gerekli adımlar atılmamışsa, istenmeyen bir taraf tarafından ele geçirilebilir ve kullanılabilir. Bazı güvenlik belirteçleri yetkisiz tarafların bunları kullanmasını önlemek için yerleşik bir mekanizmaya sahip olsa da, taşıyıcı belirteçleri bu mekanizmaya sahip değildir ve aktarım katmanı güvenliği (HTTPS) gibi güvenli bir kanalda taşınması gerekir. Taşıyıcı belirteci açık olarak iletilirse, kötü niyetli bir taraf belirteci elde etmek ve korumalı bir kaynağa yetkisiz erişim için kullanmak için ortadaki adam saldırısını kullanabilir. Ayı belirteçlerini daha sonra kullanmak üzere depolama veya önbelleğe alma da aynı güvenlik ilkeleri geçerlidir. Uygulamanızın taşıyıcı belirteçleri güvenli bir şekilde iletmesini ve depoladığını her zaman emin olun. Taşıyıcı belirteçleri hakkında daha fazla güvenlik hususları için [RFC 6750 Bölüm 5'e](https://tools.ietf.org/html/rfc6750)bakın.

Microsoft kimlik platformu bitiş noktasında kullanılan farklı belirteç türlerinin daha ayrıntılı ayrıntılarını [Microsoft kimlik platformu uç noktası belirteç başvurusu](v2-id-and-access-tokens.md)mevcuttur.

## <a name="protocols"></a>Protokoller

Bazı örnek istekleri görmeye hazırsanız, aşağıdaki öğreticilerden biriyle başlayın. Her biri belirli bir kimlik doğrulama senaryosuna karşılık gelir. Sizin için doğru akışı belirlemek için yardıma ihtiyacınız varsa, [Microsoft kimlik platformu ile oluşturabileceğiniz uygulama türlerine](v2-app-types.md)göz atın.

* [OAuth 2.0 ile mobil ve yerel uygulama oluşturun](v2-oauth2-auth-code-flow.md)
* [OpenID Connect ile web uygulamaları oluşturun](v2-protocols-oidc.md)
* [OAuth 2.0 Örtülü Akış ile tek sayfalı uygulamalar oluşturun](v2-oauth2-implicit-grant-flow.md)
* [OAuth 2.0 istemci kimlik bilgileri akışı ile daemons veya sunucu tarafı işlemleri oluşturun](v2-oauth2-client-creds-grant-flow.md)
* [OAuth 2.0 on-of Flow ile bir web API belirteçleri alın](v2-oauth2-on-behalf-of-flow.md)
