---
title: Azure Active Directory B2C 'da kimlik doğrulama protokolleri | Microsoft Docs
description: Azure Active Directory B2C tarafından desteklenen protokolleri kullanarak doğrudan uygulama oluşturma.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ed393f721d4461ebadea41f8dad707d4881865cd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78183914"
---
# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: kimlik doğrulama protokolleri
Azure Active Directory B2C (Azure AD B2C) iki sektör standardı protokolünü destekleyerek uygulamalarınız için bir hizmet olarak kimlik sağlar: OpenID Connect ve OAuth 2,0. Hizmet standartlara uyumludur, ancak bu protokollerin iki uygulamasının herhangi bir uygulaması hafif farklılıklar içerebilir.

Bu kılavuzdaki bilgiler, bir açık kaynak kitaplığı kullanmak yerine, doğrudan HTTP isteklerini göndererek ve işleyerek kodunuzu yazarsanız yararlı olur. Her bir protokolün ayrıntılarına geçmeden önce bu sayfayı okumanızı öneririz. Ancak Azure AD B2C zaten biliyorsanız, doğrudan [protokol başvuru kılavuzlarıyla](#protocols)gidebilirsiniz.

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Temel bilgiler
Azure AD B2C kullanan her uygulamanın, [Azure Portal](https://portal.azure.com)B2C dizininizde kayıtlı olması gerekir. Uygulama kayıt işlemi birkaç değer toplayarak uygulamanıza atar:

* Uygulamanızı benzersiz olarak tanımlayan bir **Uygulama Kimliği**.
* Yanıtları uygulamanıza geri yönlendirmek için kullanılabilen bir **yeniden yönlendirme URI 'si** veya **paket tanımlayıcısı** .
* Diğer senaryoya özgü birkaç değer. Daha fazla bilgi için, [uygulamanızı nasıl kaydedeceğinizi](tutorial-register-applications.md)öğrenin.

Uygulamanızı kaydettikten sonra, uç noktaya istek göndererek Azure Active Directory (Azure AD) ile iletişim kurar:

```
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/authorize
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/token
```

Neredeyse tüm OAuth ve OpenID Connect akışlarında dört taraf Exchange 'e dahil edilir:

![Dört OAuth 2,0 rolünü gösteren diyagram](./media/protocols-overview/protocols_roles.png)

* **Yetkilendirme sunucusu** Azure AD uç noktasıdır. Kullanıcı bilgileri ve erişimiyle ilgili her şeyi güvenli bir şekilde işler. Ayrıca, bir akıştaki taraflar arasındaki güven ilişkilerini de işler. Kullanıcının kimliğini doğrulama, kaynaklara erişimi verme ve iptal etme ve belirteçleri verme sorumludur. Kimlik sağlayıcısı olarak da bilinir.

* **Kaynak sahibi** genellikle Son Kullanıcı olur. Verilerin sahibi olan taraf, üçüncü tarafların bu verilere veya kaynağa erişmesine izin veren bir güce sahiptir.

* **OAuth istemcisi** , uygulamanız. Uygulama KIMLIĞI tarafından tanımlanır. Bu genellikle son kullanıcıların etkileşimde bulunduğu taraftır. Ayrıca yetkilendirme sunucusundan belirteçleri ister. Kaynak sahibi, kaynağa erişmek için istemci iznini vermelidir.

* Kaynak **sunucu** , kaynağın veya verilerin bulunduğu yerdir. OAuth istemcisini güvenli bir şekilde doğrulamak ve yetkilendirmek için yetkilendirme sunucusuna güvenir. Ayrıca, bir kaynağa erişimin verilmesini sağlamak için taşıyıcı erişim belirteçlerini kullanır.

## <a name="policies-and-user-flows"></a>İlkeler ve Kullanıcı akışları
Arguya, Azure AD B2C ilkeleri hizmetin en önemli özellikleridir. Azure AD B2C, ilkeleri sunarak standart OAuth 2,0 ve OpenID Connect protokollerini genişletir. Bunlar, Azure AD B2C basit kimlik doğrulaması ve yetkilendirmeden çok daha fazlasını gerçekleştirmesine olanak tanır.

En yaygın kimlik görevlerini ayarlamanıza yardımcı olması için Azure AD B2C portalı, **Kullanıcı akışları**adlı önceden tanımlanmış ve yapılandırılabilir ilkeler içerir. Kullanıcı akışları, kaydolma, oturum açma ve profil düzenlemesi dahil olmak üzere tüketici kimliği deneyimlerini tam olarak anlatmaktadır. Kullanıcı akışları, yönetim Kullanıcı arabiriminde tanımlanabilir. HTTP kimlik doğrulama isteklerinde özel bir sorgu parametresi kullanılarak yürütülenebilir.

İlkeler ve Kullanıcı akışları, OAuth 2,0 ve OpenID Connect 'in standart özellikleri değildir, bu nedenle bunları anlamak için zaman ayırın. Daha fazla bilgi için [Azure AD B2C Kullanıcı akışı başvuru kılavuzu](user-flow-overview.md)' na bakın.

## <a name="tokens"></a>Belirteçler
OAuth 2,0 ve OpenID Connect Azure AD B2C uygulanması, JSON Web belirteçleri (JWTs) olarak temsil edilen taşıyıcı belirteçleri dahil olmak üzere taşıyıcı belirteçlerinin yoğun bir şekilde kullanılmasını sağlar. Taşıyıcı belirteç, korunan bir kaynağa "taşıyıcı" erişimini sağlayan hafif bir güvenlik belirtecidir.

Taşıyıcı, belirteci sunmanızı sağlayan herhangi bir tarafdır. Azure AD, bir taşıyıcı belirteç alabilmesi için önce bir tarafın kimliğini doğrulamalıdır. Ancak, iletim ve depolamada belirtecin güvenli hale getirmek için gerekli adımlar alınmadığında, istenmeyen bir taraf tarafından yakalanabilir ve kullanılabilir.

Bazı güvenlik belirteçlerinde, yetkisiz tarafların bunları kullanmalarını önleyen yerleşik mekanizmalar vardır ancak taşıyıcı belirteçlerinin bu mekanizması yoktur. Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanalda aktarılmaları gerekir.

Bir taşıyıcı belirteci güvenli bir kanal dışında iletiiyorsa kötü amaçlı bir taraf, belirteci almak için ortadaki adam saldırısı ve korunan bir kaynağa yetkisiz erişim elde etmek için bu uygulamayı kullanabilir. Aynı güvenlik ilkeleri, taşıyıcı belirteçleri depolandığında veya daha sonra kullanılmak üzere önbelleğe alındığında geçerlidir. Her zaman uygulamanızın taşıyıcı belirteçlerini güvenli bir şekilde ilettiğinden ve depoladığından emin olun.

Ek taşıyıcı belirteç güvenliği konuları için bkz. [RFC 6750 Bölüm 5](https://tools.ietf.org/html/rfc6750).

Azure AD B2C 'de kullanılan farklı belirteç türleri hakkında daha fazla bilgi [Için Azure AD belirteç başvurusunda](tokens-overview.md)bulabilirsiniz.

## <a name="protocols"></a>Protokoller
Bazı örnek istekleri incelemeye hazırsanız, aşağıdaki öğreticilerden biriyle başlayabilirsiniz. Her biri belirli bir kimlik doğrulama senaryosuna karşılık gelir. Sizin için uygun olan akışın belirlenmesi için yardıma ihtiyacınız varsa [Azure AD B2C kullanarak oluşturabileceğiniz uygulama türlerine](application-types.md)göz atın.

* [OAuth 2,0 kullanarak mobil ve yerel uygulamalar oluşturun](authorization-code-flow.md)
* [OpenID Connect kullanarak Web uygulamaları oluşturun](openid-connect.md)
* [OAuth 2,0 örtük akışı kullanarak tek sayfalı uygulamalar oluşturun](implicit-flow-single-page-application.md)

