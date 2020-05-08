---
title: Uygulama modeli | Mavisi
titleSuffix: Microsoft identity platform
description: Uygulamanızı Microsoft Identity platform (v 2.0) ile tümleştirilebilen şekilde kaydetme süreci hakkında bilgi edinin.
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
ms.openlocfilehash: 774822eae5ab327f57da3eca4499ca14d3e0c7a1
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584317"
---
# <a name="application-model"></a>Uygulama modeli

Uygulamalar, kullanıcıların kendilerine oturum açmasını veya bir kimlik sağlayıcısı için oturum açma yetkisini devredebilir. Bu konuda, bir uygulamayı Microsoft Identity platformu ile kaydetmek için gereken adımlar ele alınmaktadır.

## <a name="registering-an-application"></a>Uygulamayı kaydetme

Bir kimlik sağlayıcısının bir kullanıcının belirli bir uygulamaya erişimi olduğunu bilmesini sağlamak için, hem Kullanıcı hem de uygulamanın kimlik sağlayıcısına kayıtlı olması gerekir. Uygulamanızı Azure AD 'ye kaydettiğinizde, uygulamanız için Microsoft Identity platform ile tümleşmesini sağlayan bir kimlik yapılandırması sağladığınızda. Uygulamayı kaydetmek şunları da sağlar:

* Oturum açma iletişim kutusunda uygulamanızın markasını özelleştirin. Bu, bir kullanıcının uygulamanız için sahip olacağı ilk deneyim olduğu için önemlidir.
* Kullanıcıların yalnızca kuruluşunuza ait olmaları durumunda oturum açmalarına izin vermek istediğinize karar verin. Bu tek kiracılı bir uygulamadır. Ya da kullanıcıların herhangi bir iş veya okul hesabını kullanarak oturum açmalarına izin verin. Bu, çok kiracılı bir uygulamadır. Ayrıca, kişisel Microsoft hesaplarına veya bir sosyal hesaba LinkedIn, Google, vb. izin verebilirsiniz.
* Kapsam izinleri iste. Örneğin, oturum açmış kullanıcının profilini okuma izni veren "User. Read" kapsamını isteyebilirsiniz.
* Web API 'nize erişimi tanımlayan kapsamları tanımlayın. Genellikle, bir uygulama API 'nize erişmek istediğinde tanımladığınız kapsamlar için izin istemesi gerekecektir.
* Uygulamanın kimliğini kanıtlayan Microsoft Identity platformu ile bir gizli dizi paylaşma.  Bu, uygulamanın gizli bir istemci uygulaması olduğu durum ile ilgilidir. Gizli bir istemci uygulaması, kimlik bilgilerini güvenli bir şekilde tutan bir uygulamadır. Kimlik bilgilerini depolamak için güvenilir bir arka uç sunucusu gerekir.

Kaydolduktan sonra uygulamaya, belirteç istediğinde uygulamanın Microsoft Identity platform ile paylaştığı benzersiz bir tanımlayıcı verilir. Uygulama [gizli bir istemci uygulaması](developer-glossary.md#client-application)ise, sertifikaların veya gizli anahtarların kullanılıp kullanıldığına bağlı olarak gizli anahtar veya ortak anahtarı da paylaşır.

Microsoft Identity platform iki ana işlevi yerine getiren bir modeli kullanarak uygulamaları temsil eder:

* Uygulamayı desteklediği kimlik doğrulama protokollerine göre tanımla
* Kimlik doğrulaması için gereken tüm tanımlayıcıları, URL 'Leri, gizli dizileri ve ilgili bilgileri sağlayın

Microsoft Identity Platform:

* Çalışma zamanında kimlik doğrulamasını desteklemek için gereken tüm verileri tutar
* Bir uygulamanın erişmesi gerekebilecek kaynakları ve belirli bir isteğin yerine getirilmesi gereken koşulları belirlemek için tüm verileri tutar
* Uygulama geliştiricisinin kiracısında ve diğer Azure AD kiracılarında uygulama sağlamayı uygulamak için altyapı sağlar
* Belirteç istek süresi boyunca Kullanıcı onayını işler ve kiracılar genelinde uygulamaların dinamik sağlamasını kolaylaştırır

**Onay** , bir istemci uygulamanın, kaynak sahibi adına belirli izinler altında korumalı kaynaklara erişmesi için bir kaynak sahibi verme yetkilendirmesi işlemidir. Microsoft Identity Platform:

* Kullanıcıların ve yöneticilerin uygulamanın kendileri adına kaynaklara erişmesine dinamik olarak onay vermesini veya reddetmesini sağlar.
* Yöneticilerin uygulamaların gerçekleştirebilecekleri işlemler, belirli uygulamalara erişebilecek kullanıcılar ve erişilen dizin kaynakları hakkında son kararı vermesini sağlar.

## <a name="multi-tenant-apps"></a>Çok kiracılı uygulamalar

Microsoft Identity platformunda bir [uygulama nesnesi](developer-glossary.md#application-object) bir uygulamayı açıklar. Dağıtım zamanında, Microsoft Identity platform bir dizin veya kiracı içindeki bir uygulamanın somut örneğini temsil eden bir [hizmet sorumlusu](developer-glossary.md#service-principal-object)oluşturmak için uygulama nesnesini bir şema olarak kullanır. Hizmet sorumlusu, uygulamanın belirli bir hedef dizinde gerçekten ne yapabileceğini, bunu kullanabilecek kaynakları, hangi kaynakların erişebileceğini ve bu şekilde olduğunu tanımlar. Microsoft Identity platform bir uygulama nesnesinden [onay](developer-glossary.md#consent)aracılığıyla bir hizmet sorumlusu oluşturur.

Aşağıdaki diyagramda, izin tarafından yönetilen basitleştirilmiş bir Microsoft Identity platformu sağlama akışı gösterilmektedir. İki kiracı gösterir: *A* ve *B*.

* *Kiracı* , uygulamanın sahibi.
* *B kiracısı* , bir hizmet sorumlusu aracılığıyla uygulamayı örnekleniyor.

![Onay temelli basitleştirilmiş sağlama akışı](./media/authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

Bu sağlama akışında:

1. B kiracısından bir kullanıcı uygulamayla oturum açmaya çalışır, yetkilendirme uç noktası uygulama için bir belirteç ister.
1. Kimlik doğrulaması için Kullanıcı kimlik bilgileri alındı ve doğrulanır.
1. Kullanıcıdan, B kiracısına erişim kazanmak için uygulamanın onayını sağlaması istenir.
1. Microsoft Identity platformu, B kiracısında bir hizmet sorumlusu oluşturmak için bir şema olarak kiracı 'daki uygulama nesnesini kullanır.
1. Kullanıcı istenen belirteci alır.

Bu işlemi, ek kiracılar için yineleyebilirsiniz. Kiracı A, uygulama için şemayı saklar (uygulama nesnesi). Uygulamanın izin verdiği tüm diğer kiracıların kullanıcıları ve yöneticileri, uygulamanın her Kiracıdaki ilgili hizmet sorumlusu nesnesi aracılığıyla ne yapmasına izin verileceğini kontrol edin. Daha fazla bilgi için bkz. [Microsoft Identity platformunda uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

## <a name="next-steps"></a>Sonraki adımlar

Kimlik doğrulama ve yetkilendirme temellerini kapsayan diğer konular için:

* Microsoft Identity platform 'da kimlik doğrulaması ve yetkilendirmenin temel kavramları hakkında bilgi edinmek için bkz [. kimlik doğrulaması ve yetkilendirme karşılaştırması](authentication-vs-authorization.md) .
* Kimlik doğrulama ve yetkilendirme için erişim belirteçlerinin, yenileme belirteçlerinin ve KIMLIK belirteçlerinin nasıl kullanıldığını öğrenmek için [güvenlik belirteçlerine](security-tokens.md) bakın.
* Microsoft Identity platformunda Web, masaüstü ve mobil uygulamaların oturum açma akışı hakkında bilgi edinmek için bkz. [uygulama oturum açma akışı](app-sign-in-flow.md) .

Uygulama modeli hakkında daha fazla bilgi edinmek için:

* Microsoft Identity platform 'daki uygulama nesneleri ve hizmet sorumluları hakkında daha fazla bilgi için bkz. [Azure AD 'ye uygulamaların nasıl ve neden eklendiğini](active-directory-how-applications-are-added.md) öğrenin.
* Tek kiracılı uygulamalar ve çok kiracılı uygulamalar hakkında daha fazla bilgi için bkz. [Azure Active Directory](single-and-multi-tenant-apps.md) .
* Kuruluşların kullanıcıların, genellikle Google hesabı gibi sosyal kimlikler kullanarak kullanıcılara oturum açmasını sağlamak için Azure AD 'nin Azure Active Directory B2C nasıl [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c) hakkında daha fazla bilgi için bkz..
