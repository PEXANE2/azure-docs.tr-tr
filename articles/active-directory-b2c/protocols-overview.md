---
title: Azure Active Directory B2C'de kimlik doğrulama protokolleri | Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183914"
---
# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Kimlik doğrulama protokolleri
Azure Active Directory B2C (Azure AD B2C), iki endüstri standardı protokolü destekleyerek uygulamalarınız için bir hizmet olarak kimlik sağlar: OpenID Connect ve OAuth 2.0. Hizmet standartlara uygundur, ancak bu protokollerin herhangi iki uygulaması ince farklılıklar olabilir.

Bu kılavuzdaki bilgiler, kodunuzu açık kaynak kitaplığı kullanmak yerine doğrudan HTTP isteklerini göndererek ve işleyerek yazarsanız yararlıdır. Her bir protokolün ayrıntılarına dalmadan önce bu sayfayı okumanızı öneririz. Ancak Azure AD B2C'yi zaten biliyorsanız, doğrudan [protokol başvuru kılavuzlarına](#protocols)gidebilirsiniz.

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Temel bilgiler
Azure AD B2C kullanan her uygulamanın [Azure portalındaki](https://portal.azure.com)B2C dizininize kaydedilmesi gerekir. Uygulama kayıt işlemi birkaç değer toplayarak uygulamanıza atar:

* Uygulamanızı benzersiz olarak tanımlayan bir **Uygulama Kimliği**.
* Yanıtları uygulamanıza yönlendirmek için kullanılabilecek bir **Yeniden Yönlendirme URI** veya paket **tanımlayıcısı.**
* Birkaç diğer senaryoya özgü değerler. Daha fazla bilgi için [başvurunuzu nasıl kaydedebilirsiniz](tutorial-register-applications.md)öğrenin.

Uygulamanızı kaydettikten sonra, son noktaya istekler göndererek Azure Etkin Dizin (Azure AD) ile iletişim kurar:

```
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/authorize
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/token
```

Hemen hemen tüm OAuth ve OpenID Connect akışlarında, değişime dört taraf katılır:

![Dört OAuth 2.0 Rolü gösteren diyagram](./media/protocols-overview/protocols_roles.png)

* **Yetkilendirme sunucusu** Azure AD bitiş noktasıdır. Kullanıcı bilgileri ve erişimle ilgili her şeyi güvenli bir şekilde işler. Ayrıca, taraflar arasındaki güven ilişkilerini bir akış içinde yönetir. Kullanıcının kimliğini doğrulamaktan, kaynaklara erişim vermekten ve iptal ten ve belirteçleri vermekten sorumludur. Kimlik sağlayıcısı olarak da bilinir.

* **Kaynak sahibi** genellikle son kullanıcıdır. Verilerin sahibi taraftır ve üçüncü tarafların bu verilere veya kaynağa erişmesine izin verme yetkisine sahiptir.

* **OAuth istemcisi** sizin uygulamanızdır. Uygulama kimliğiyle tanımlanır. Genellikle son kullanıcıların etkileşimkurduğu taraftır. Ayrıca yetkilendirme sunucusundan belirteçleri de ister. Kaynak sahibi, istemciye kaynağa erişmesi için izin vermelidir.

* **Kaynak sunucusu,** kaynağın veya verilerin bulunduğu yerdir. OAuth istemcisinin güvenli bir şekilde doğrulaması ve yetkilendirilmesi için yetkilendirme sunucusuna güvenir. Ayrıca, bir kaynağa erişimin sağlanabileceğinden emin olmak için taşıyıcı erişim belirteçleri kullanır.

## <a name="policies-and-user-flows"></a>İlkeler ve kullanıcı akışları
Azure AD B2C ilkeleri, hizmetin en önemli özellikleridir. Azure AD B2C, ilkeler sunarak standart OAuth 2.0 ve OpenID Connect protokollerini genişletir. Bunlar, Azure AD B2C'nin basit kimlik doğrulama ve yetkilendirmeden çok daha fazlasını gerçekleştirmesine olanak tanır.

Azure AD B2C portalı, en yaygın kimlik görevlerini ayarlamanıza yardımcı olmak için **kullanıcı akışları**adı verilen önceden tanımlanmış, yapılandırılabilir ilkeler içerir. Kullanıcı akışı, kaydolma, oturum açma ve profil düzenleme gibi tüketici kimlik deneyimlerini tam olarak tanımlar. Kullanıcı akışları yönetimli bir kullanıcı bir kullanıcı aracıyla tanımlanabilir. Bunlar, HTTP kimlik doğrulama isteklerinde özel bir sorgu parametresi kullanılarak yürütülebilir.

İlkeler ve kullanıcı akışları OAuth 2.0 ve OpenID Connect'in standart özellikleri değildir, bu nedenle bunları anlamak için zaman ayırmalısınız. Daha fazla bilgi için [Azure AD B2C kullanıcı akışı başvuru kılavuzuna](user-flow-overview.md)bakın.

## <a name="tokens"></a>Belirteçler
OAuth 2.0 ve OpenID Connect'in Azure AD B2C uygulaması, JSON web belirteçleri (JWTs) olarak temsil edilen taşıyıcı belirteçleri de dahil olmak üzere taşıyıcı belirteçlerini kapsamlı bir şekilde kullanır. Taşıyıcı belirteci, korunan bir kaynağa "taşıyıcı" erişimi sağlayan hafif bir güvenlik belirtecidir.

Taşıyıcı belirteç sunabilir herhangi bir partidir. Azure AD'nin, taşıyıcı belirteci alabilmek için önce bir partinin kimliğini doğrulaması gerekir. Ancak, iletim ve depolamadaki belirteci güvence altına almak için gerekli adımlar atılmamışsa, istenmeyen bir taraf tarafından engellenebilir ve kullanılabilir.

Bazı güvenlik belirteçleri yetkisiz tarafların bunları kullanmasını engelleyen yerleşik mekanizmalara sahiptir, ancak taşıyıcı belirteçleri bu mekanizmaya sahip değildir. Bunlar, aktarım katmanı güvenliği (HTTPS) gibi güvenli bir kanalda taşınmalıdır.

Taşıyıcı belirteci güvenli bir kanalın dışına aktarılırsa, kötü niyetli bir taraf belirteci elde etmek ve korumalı bir kaynağa yetkisiz erişim elde etmek için kullanmak için ortadaki adam saldırısını kullanabilir. Taşıyıcı belirteçleri depolandığında veya daha sonra kullanmak üzere önbelleğe aldığında aynı güvenlik ilkeleri geçerlidir. Uygulamanızın taşıyıcı belirteçleri güvenli bir şekilde iletmesini ve depoladığını her zaman emin olun.

Ek taşıyıcı belirteç güvenlik hususları için [RFC 6750 Bölüm 5'e](https://tools.ietf.org/html/rfc6750)bakın.

Azure AD B2C'de kullanılan farklı belirteç türleri hakkında daha fazla bilgi [Azure AD belirteci referansında](tokens-overview.md)mevcuttur.

## <a name="protocols"></a>Protokoller
Bazı örnek istekleri ni incelemeye hazır olduğunuzda, aşağıdaki öğreticilerden biriyle başlayabilirsiniz. Her biri belirli bir kimlik doğrulama senaryosuna karşılık gelir. Hangi akışın sizin için doğru olduğunu belirlemek için yardıma ihtiyacınız varsa, [Azure AD B2C'yi kullanarak oluşturabileceğiniz uygulama türlerine](application-types.md)göz atın.

* [OAuth 2.0 kullanarak mobil ve yerel uygulamalar oluşturun](authorization-code-flow.md)
* [OpenID Connect'i kullanarak web uygulamaları oluşturun](openid-connect.md)
* [OAuth 2.0 örtük akışını kullanarak tek sayfalı uygulamalar oluşturun](implicit-flow-single-page-application.md)

