---
title: OAuth 2,0 ve OpenID Connect protokolleri-Microsoft Identity platform | Mavisi
description: Microsoft Identity platform uç noktası tarafından desteklenen OAuth 2,0 ve OpenID Connect protokollerine yönelik bir kılavuz.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 80b93efb58d225c53a64fa044f51145b392460d7
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690257"
---
# <a name="oauth-20-and-openid-connect-protocols-on-the-microsoft-identity-platform"></a>Microsoft Identity platformunda OAuth 2,0 ve OpenID Connect protokolleri

Endüstri standardı protokoller, OpenID Connect ve OAuth 2,0 ile hizmet olarak kimlik için Microsoft Identity platform uç noktası. Hizmet standartlarla uyumlu olsa da, bu protokollerin iki uygulaması arasında hafif farklar olabilir. Buradaki bilgiler, HTTP isteklerini doğrudan gönderip işleyerek veya [Açık kaynaklı kitaplıklarımızın](reference-v2-libraries.md)birini kullanmak yerine bir üçüncü taraf açık kaynak kitaplığı kullanarak kodunuzu yazmayı tercih ediyorsanız yararlı olacaktır.

## <a name="the-basics"></a>Temel bilgiler

Neredeyse tüm OAuth 2,0 ve OpenID Connect akışlarında, Exchange 'e dahil edilen dört taraf vardır:

![OAuth 2,0 rollerinin gösterildiği diyagram](./media/active-directory-v2-flows/protocols-roles.svg)

* **Yetkilendirme sunucusu** , Microsoft Identity platform uç noktasıdır ve kullanıcının kimliğini sağlamaktan, kaynaklara erişim verme ve iptal etme ve belirteçleri verme konusunda sorumludur. Kimlik sağlayıcısı olarak da bilinen yetkilendirme sunucusu, kullanıcının bilgileri, erişimleri ve bir akıştaki taraflar arasındaki güven ilişkilerini güvenli bir şekilde işler.
* **Kaynak sahibi** genellikle Son Kullanıcı olur. Bu, verilerin sahibi olan ve üçüncü tarafların bu verilere veya kaynağa erişmesine izin veren bir güce sahip olan taraftır.
* **OAuth istemcisi** , uygulama kimliğiyle tanımlanan uygulamanız. OAuth istemcisi genellikle son kullanıcının etkileşime girdiği tarafdır ve yetkilendirme sunucusundan belirteçler ister. İstemciye kaynak sahibi tarafından kaynağa erişim izni verilmesi gerekir.
* Kaynak **sunucu** , kaynağın veya verilerin bulunduğu yerdir. OAuth Istemcisini güvenli şekilde doğrulamak ve yetkilendirmek için yetkilendirme sunucusuna güvenir ve bir kaynağa erişimin verilmesini sağlamak için taşıyıcı erişim belirteçlerini kullanır.

## <a name="app-registration"></a>Uygulama kaydı

Hem kişisel hem de iş veya okul hesaplarını kabul etmek isteyen her uygulama, OAuth 2,0 veya OpenID Connect kullanarak bu kullanıcıları imzalayabilmesi için [Azure portal](https://aka.ms/appregistrations) **uygulama kayıtları** deneyiminden kaydedilmelidir. Uygulama kayıt işlemi, uygulamanıza birkaç değer toplayacak ve atayacaktır:

* Uygulamanızı benzersiz bir şekilde tanımlayan **uygulama kimliği**
* Yanıtları uygulamanıza geri yönlendirmek için kullanılabilen bir **yeniden yönlendirme URI 'si** (isteğe bağlı)
* Diğer senaryoya özgü birkaç değer.

Daha ayrıntılı bilgi için [uygulama kaydetmeyi](quickstart-register-app.md) öğrenin.

## <a name="endpoints"></a>Uç Noktalar

Kaydolduktan sonra, uygulama uç noktaya istek göndererek Microsoft Identity platformu ile iletişim kurar:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

`{tenant}` Burada dört farklı değerden birini alabilir:

| Değer | Açıklama |
| --- | --- |
| `common` | Azure AD 'den hem kişisel Microsoft hesapları hem de iş/okul hesabı olan kullanıcıların uygulamada oturum açmasını sağlar. |
| `organizations` | Yalnızca Azure AD 'den iş/okul hesapları olan kullanıcıların uygulamada oturum açmasını sağlar. |
| `consumers` | Yalnızca kişisel Microsoft hesabı (MSA) olan kullanıcıların uygulamada oturum açmasına izin verir. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` | Yalnızca belirli bir Azure AD kiracısından iş/okul hesapları olan kullanıcıların uygulamada oturum açmasını sağlar. Azure AD kiracının kolay etki alanı adı ya da kiracının GUID tanımlayıcısı kullanılabilir. |

Bu uç noktalarla nasıl etkileşim kuracağınızı öğrenmek için, [protokoller](#protocols) bölümünde belirli bir uygulama türü seçin ve daha fazla bilgi için bağlantıları izleyin.

> [!TIP]
> Azure AD 'de kayıtlı tüm uygulamalar, kişisel hesaplara kaydolmasa bile Microsoft Identity platform uç noktasını kullanabilir.  Bu şekilde, uygulamanızı yeniden oluşturmadan mevcut uygulamaları Microsoft Identity platform ve [msal](reference-v2-libraries.md) 'a geçirebilirsiniz.

## <a name="tokens"></a>Belirteçler

OAuth 2,0 ve OpenID Connect 'in Microsoft Identity platform uygulamaları, JWTs olarak temsil edilen taşıyıcı belirteçleri dahil olmak üzere taşıyıcı belirteçlerinin yoğun bir şekilde kullanılmasını kolaylaştırır. Taşıyıcı belirteç, korunan bir kaynağa "taşıyıcı" erişimini sağlayan hafif bir güvenlik belirtecidir. Bu anlamda, "taşıyıcı" belirteci sunacak herhangi bir tarafdır. Bir tarafın, taşıyıcı belirtecini almak için öncelikle Microsoft Identity platformunda kimlik doğrulaması yapması gerekir, ancak gerekli adımlar iletim ve depolama alanındaki belirtecin güvenliğini sağlamak için alınmadığından, istenmeyen bir taraf tarafından yakalanabilir ve kullanılabilir. Bazı güvenlik belirteçlerinin, yetkisiz tarafların onları kullanmasını önlemek için yerleşik bir mekanizması vardır ancak, taşıyıcı belirteçleri bu mekanizmaya sahip değildir ve Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanalda aktarılmalıdır. Bir taşıyıcı belirteci açık bir şekilde iletilebilise, kötü amaçlı bir taraf, belirteci almak ve korumalı bir kaynağa yetkisiz erişim için kullanmak üzere bir ortadaki adam saldırısı kullanabilir. Aynı güvenlik ilkeleri, daha sonra kullanılmak üzere taşıyıcı belirteçleri depolarken veya önbelleğe alırken geçerlidir. Her zaman uygulamanızın taşıyıcı belirteçlerini güvenli bir şekilde ilettiğinden ve depoladığından emin olun. Taşıyıcı belirteçleriyle ilgili daha fazla bilgi için bkz. [RFC 6750 Bölüm 5](https://tools.ietf.org/html/rfc6750).

Microsoft Identity platform uç noktasında kullanılan farklı belirteç türleri hakkında daha fazla ayrıntı [Microsoft Identity platform Endpoint Token başvurusunda](v2-id-and-access-tokens.md)bulunabilir.

## <a name="protocols"></a>Protokoller

Bazı örnek istekleri görmeyi hazırsanız aşağıdaki öğreticilerden birini kullanmaya başlayın. Her biri belirli bir kimlik doğrulama senaryosuna karşılık gelir. Sizin için doğru Flow 'un belirlenmesi için yardıma ihtiyacınız varsa, [Microsoft Identity platform ile oluşturabileceğiniz uygulama türlerine](v2-app-types.md)göz atın.

* [OAuth 2,0 ile mobil ve yerel uygulama oluşturma](v2-oauth2-auth-code-flow.md)
* [OpenID Connect ile Web uygulamaları oluşturun](v2-protocols-oidc.md)
* [OAuth 2,0 örtük Flow ile tek sayfalı uygulamalar oluşturun](v2-oauth2-implicit-grant-flow.md)
* [OAuth 2,0 istemci kimlik bilgileri akışıyla Daemon 'ları veya sunucu tarafı süreçler oluşturun](v2-oauth2-client-creds-grant-flow.md)
* [Bir Web API 'sinde, OAuth 2,0 adına sahip akışa sahip belirteçleri edinme](v2-oauth2-on-behalf-of-flow.md)
