---
title: Microsoft Identity platformunda kimlik doğrulaması | Mavisi
description: Microsoft Identity platformunda (v 2.0) kimlik doğrulamasının temelleri hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/24/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: d979745d9b5bb65bd08f69db86801156de2a489d
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "82161750"
---
# <a name="authentication-basics"></a>Kimlik doğrulaması temel bilgileri

Bu makalede, korunan Web uygulamaları, Web API 'Leri veya korumalı Web API 'Lerini çağıran uygulamalar oluşturmak için anlamanız gereken birçok kimlik doğrulama kavramı yer almaktadır. Bildiğiniz bir terimi görürseniz, temel kavramları kapsayan [Sözlüğümüzü](developer-glossary.md) veya [Microsoft Identity platform videolarımızı](identity-videos.md) deneyin.

## <a name="authentication-vs-authorization"></a>Kimlik doğrulama ve yetkilendirme karşılaştırması

**Kimlik doğrulama** , sizin olduğunu söylediğinizde sizin olduğunu kanıtlama işlemidir. Kimlik doğrulaması bazen AuthN şeklinde kısaltılabilir. Microsoft Identity platform kimlik doğrulamasını işlemek için [OpenID Connect](https://openid.net/connect/) protokolünü uygular.

**Yetkilendirme** , kimliği doğrulanmış bir tarafın bir şeyi yapmak için izin verme işlemidir. Hangi verilerin erişebileceğini ve bu verilerle yapabileceklerinizi belirtir. Yetkilendirme bazen AuthZ şeklinde kısaltılabilir. Microsoft Identity platform yetkilendirmeyi işlemek için [OAuth 2,0](https://oauth.net/2/) protokolünü uygular.

Her birinin kendi Kullanıcı adı ve parola bilgilerini korumalarına veya birden çok uygulama arasında Kullanıcı eklemeniz veya kaldırmanız gerektiğinde yüksek bir yönetim yükü sunan uygulamalar oluşturmak yerine, uygulamalar bu sorumluluğu merkezi bir kimlik sağlayıcısına devredebilir.

Azure Active Directory (Azure AD), bulutta merkezi bir kimlik sağlayıcıdır. Kimlik doğrulama ve yetkilendirmeyi yetkilendirme, bir kullanıcının belirli bir konumda olmasını gerektiren koşullu erişim ilkeleri, Multi-Factor Authentication 'ın kullanılması ve bir kullanıcının bir kez oturum açmasını ve ardından aynı merkezi dizini paylaşan tüm Web uygulamalarında otomatik olarak oturum açmasını sağlar. Bu yetenek, **Çoklu oturum açma (SSO)** olarak adlandırılır.

Microsoft Identity platform; OAuth 2,0 ve OpenID Connect gibi sektör standardı protokoller desteğiyle ve kodlamaya hızlı bir şekilde başlamanıza yardımcı olması için farklı platformlar için açık kaynak kitaplıkların yanı sıra, uygulama geliştiricileri için kimlik doğrulama ve yetkilendirmeyi basitleştirir. Geliştiricilerin tüm Microsoft kimliklerinden oturum açmasını, [Microsoft Graph](https://developer.microsoft.com/graph/), diğer Microsoft API 'lerini veya geliştiricilerin oluşturduğu API 'leri çağıracağınız belirteçler almasını sağlar. Daha fazla bilgi için bkz. [Microsoft Identity platform 'un gelişi](about-microsoft-identity-platform.md).

## <a name="security-tokens"></a>Güvenlik belirteçleri

Merkezi bir kimlik sağlayıcısı özellikle, dünyanın her yerindeki kullanıcıların, kuruluşun ağından oturum açması gerekmeyen uygulamalar için önemlidir. Microsoft Identity platform kullanıcıların kimliğini doğrular ve [erişim belirteci](developer-glossary.md#access-token), [yenileme belirteci](developer-glossary.md#refresh-token)ve [Kimlik belirteçleri](developer-glossary.md#id-token)gibi güvenlik belirteçleri sağlar. bu sayede, bir [istemci uygulamasının](developer-glossary.md#client-application) bir [kaynak sunucusundaki](developer-glossary.md#resource-server)korumalı kaynaklara erişmesine izin verilir.

**Erişim belirteçleri** , bir yetkilendirme sunucusu tarafından verilen bir güvenlik belirtecidir. Kullanıcının ve belirtecin hedeflenen uygulamayla ilgili bilgiler içerir; Web API 'Lerine ve diğer korumalı kaynaklara erişmek için kullanılabilir. Microsoft Identity platform 'un erişim belirteçleri hakkında daha fazla bilgi edinmek için bkz. [erişim belirteçleri](access-tokens.md).

Erişim belirteçleri yalnızca kısa bir süre için geçerlidir. bu nedenle, yetkilendirme sunucuları bazen erişim belirteci verildiği sırada **yenileme belirteçleri** verir. İstemci uygulaması daha sonra gerektiğinde yeni bir erişim belirteci için bu yenileme belirtecini değiştirebilir. Microsoft Identity platformunun izinleri iptal etmek için yenileme belirteçleri kullanma hakkında daha fazla bilgi edinmek için bkz. [belirteç iptali](access-tokens.md#token-revocation).

**Kimlik belirteçleri** , bir [OpenID Connect](v2-protocols-oidc.md) akışının parçası olarak istemci uygulamasına gönderilir. Bunlar, bir erişim belirteci yerine ya da üzerinden gönderilebilir ve istemci tarafından kullanıcının kimliğini doğrulamak için kullanılır. Microsoft kimlik platformunun KIMLIK belirteçleri hakkında daha fazla bilgi edinmek için bkz. [Kimlik belirteçleri](id-tokens.md).

### <a name="validating-security-tokens"></a>Güvenlik belirteçleri doğrulanıyor

Belirteç doğrulamak için, belirtecin oluşturulduğu uygulamaya, kullanıcının oturum açmış olduğu Web uygulamasına veya Web API 'sinin çağrılmakta olduğu uygulamaya kadar olur. Belirteç, güvenlik belirteci sunucusu (STS) tarafından özel anahtarla imzalanır. STS ilgili ortak anahtarı yayımlar. Bir belirteci doğrulamak için, uygulama, imzayı özel anahtar kullanılarak oluşturulduğunu doğrulamak üzere STS ortak anahtarını kullanarak imzayı doğrular.

Belirteçler yalnızca sınırlı bir süre için geçerlidir. STS genellikle bir çift belirteç sağlar:

* Uygulamaya veya korunan kaynağa erişmek için bir erişim belirteci ve
* Erişim belirtecinin süresi dolmak üzere kapatıldığında erişim belirtecini yenilemek için kullanılan yenileme belirteci.

Erişim belirteçleri, `Authorization` üst bilgide taşıyıcı belirteci olarak BIR Web API 'sine geçirilir. Bir uygulama STS 'ye yenileme belirteci sağlayabilir ve uygulamaya yönelik kullanıcı erişimi iptal edilmediği takdirde, yeni bir erişim belirteci ve yeni bir yenileme belirteci geri alır. Bu, kuruluşa ayrılmaya yönelik senaryonun işlenme yönteminden oluşur. STS yenileme belirtecini aldığında, Kullanıcı artık yetkilendirilmezse başka bir geçerli erişim belirteci vermez.

### <a name="json-web-tokens-jwts-and-claims"></a>JSON Web belirteçleri (JWTs) ve talepleri

Microsoft Identity platformu, talepler içeren JSON Web belirteçleri (JWTs) olarak güvenlik belirteçleri uygular.

Bir [talep](developer-glossary.md#claim) , bir istemci uygulaması veya [kaynak sahibi](developer-glossary.md#resource-owner)gibi bir varlık hakkında, kaynak sunucusu gibi başka bir varlığa onay verir.

Talepler, belirteç konusuyla ilgili olgu geçişi yapan ad/değer çiftleridir. Örneğin, bir talep [yetkilendirme sunucusu](developer-glossary.md#authorization-server)tarafından kimliği doğrulanan güvenlik sorumlusu hakkında olgu içerebilir. Belirli bir belirteçte mevcut talepler, belirteç türü, konunun kimliğini doğrulamak için kullanılan kimlik bilgisi türü, uygulama yapılandırması vb. dahil olmak üzere birçok konuya bağlıdır.

Uygulamalar, şu gibi çeşitli görevler için talepler kullanabilir:

* Belirteç doğrulanıyor
* Belirteç konusunun kiracısı tanımlanıyor
* Kullanıcı bilgilerini görüntüleme
* Konunun yetkilendirmesini belirleme

Bir talep, şu gibi bilgiler sağlayan anahtar-değer çiftlerinden oluşur:

* Belirteci oluşturan güvenlik belirteci sunucusu
* Belirtecin oluşturulduğu tarih
* Konu (Kullanıcı--Daemon 'ları dışında)
* Belirtecin oluşturulduğu uygulama olan hedef kitle
* Belirteç için istenen uygulama (istemci). Web uygulamaları söz konusu olduğunda, bu, hedef kitle ile aynı olabilir

Microsoft kimlik platformunun belirteçleri ve talep bilgilerini nasıl uyguladığı hakkında daha fazla bilgi edinmek için bkz. [erişim belirteçleri](access-tokens.md) ve [Kimlik belirteçleri](id-tokens.md).

### <a name="how-each-flow-emits-tokens-and-codes"></a>Her akışın belirteçleri ve kodları nasıl yayar

İstemcinizin nasıl oluşturulduğuna bağlı olarak, Microsoft Identity platform tarafından desteklenen kimlik doğrulama akışlarının birini (veya birkaçını) kullanabilir. Bu akışlar çeşitli belirteçler (id_tokens, yenileme belirteçleri, erişim belirteçleri) ve yetkilendirme kodlarını oluşturabilir ve bunların çalışmasını sağlamak için farklı belirteçler gerektirebilir. Bu grafik genel bakış sağlar:

|Akış | Gerektirmeyen | id_token | erişim belirteci | belirteci Yenile | yetkilendirme kodu |
|-----|----------|----------|--------------|---------------|--------------------|
|[Yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md) | | x | x | x | x|
|[Örtük akış](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Karma OıDC akışı](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Belirteç satın alma yenileme](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | belirteci Yenile | x | x | x| |
|[On-behalf-of akışı](v2-oauth2-on-behalf-of-flow.md) | erişim belirteci| x| x| x| |
|[İstemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md) | | | x (yalnızca uygulama)| | |

Örtülü mod aracılığıyla yayınlanan belirteçlerin, tarayıcıya URL aracılığıyla geri geçirilme nedeniyle bir uzunluk sınırlaması vardır (burada `response_mode` `query` veya `fragment`).  Bazı tarayıcıların, tarayıcı çubuğuna koyabileceğiniz ve çok uzun olduğunda başarısız olan URL 'nin boyutunda bir sınırı vardır.  Bu nedenle, bu belirteçlerin `groups` veya `wids` talepleri yoktur.

## <a name="tenants"></a>Kiracılar

Bulut kimlik sağlayıcısı birçok kuruluşa hizmet eder. Kullanıcıları farklı kuruluşlardan ayrı tutmak için, Azure AD kiracılar halinde bölümlenmiştir ve kuruluşa göre tek bir kiracı olur.

Kiracılar kullanıcıları ve bunlarla ilişkili uygulamaları izler. Microsoft Identity platform, kişisel Microsoft hesaplarıyla oturum açarken kullanılan kullanıcıları da destekler.

Azure AD Ayrıca, kuruluşların kullanıcılara, genellikle Google hesabı gibi sosyal kimlikler kullanarak oturum açmalarına olanak tanıyan Azure Active Directory B2C de sağlar. Daha fazla bilgi için bkz. [Azure Active Directory B2C belgeleri](https://docs.microsoft.com/azure/active-directory-b2c) .

Temel bilgilere genel bir bakış edindiğinize göre, kimlik uygulama modeli ve API 'sini anlamak için ' i okuyun, sağlama işlemini Microsoft Identity platformunda nasıl çalıştığını öğrenin ve Microsoft Identity platform 'un desteklediği yaygın senaryolar hakkında ayrıntılı bilgi edinin.

## <a name="application-model"></a>Uygulama modeli

Uygulamalar, kullanıcıların kendilerine oturum açmasını veya bir kimlik sağlayıcısı için oturum açma yetkisini devredebilir. Microsoft Identity platform tarafından desteklenen oturum açma senaryoları hakkında bilgi edinmek için bkz. [kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) .

Bir kimlik sağlayıcısının bir kullanıcının belirli bir uygulamaya erişimi olduğunu bilmesini sağlamak için, hem Kullanıcı hem de uygulamanın kimlik sağlayıcısına kayıtlı olması gerekir. Uygulamanızı Azure AD 'ye kaydettiğinizde, uygulamanız için Microsoft Identity platform ile tümleşmesini sağlayan bir kimlik yapılandırması sağladığınızda. Uygulamayı kaydetmek şunları da sağlar:

* Oturum açma iletişim kutusunda uygulamanızın markasını özelleştirin. Bu, bir kullanıcının uygulamanız için sahip olacağı ilk deneyim olduğu için önemlidir.
* Kullanıcıların yalnızca kuruluşunuza ait olmaları durumunda oturum açmalarına izin vermek istediğinize karar verin. Bu tek kiracılı bir uygulamadır. Ya da kullanıcıların herhangi bir iş veya okul hesabını kullanarak oturum açmalarına izin verin. Bu, çok kiracılı bir uygulamadır. Ayrıca, kişisel Microsoft hesaplarına veya bir sosyal hesaba LinkedIn, Google, vb. izin verebilirsiniz.
* Kapsam izinleri iste. Örneğin, oturum açmış kullanıcının profilini okuma izni veren "User. Read" kapsamını isteyebilirsiniz.
* Web API 'nize erişimi tanımlayan kapsamları tanımlayın. Genellikle, bir uygulama API 'nize erişmek istediğinde tanımladığınız kapsamlar için izin istemesi gerekecektir.
* Uygulamanın kimliğini kanıtlayan Microsoft Identity platformu ile bir gizli dizi paylaşma.  Bu, uygulamanın gizli bir istemci uygulaması olduğu durum ile ilgilidir. Gizli bir istemci uygulaması, kimlik bilgilerini güvenli bir şekilde tutan bir uygulamadır. Kimlik bilgilerini depolamak için güvenilir bir arka uç sunucusu gerekir.

Kaydolduktan sonra uygulamaya, belirteç istediğinde uygulamanın Microsoft Identity platform ile paylaştığı benzersiz bir tanımlayıcı verilir. Uygulama [gizli bir istemci uygulaması](developer-glossary.md#client-application)ise, sertifikaların veya gizli anahtarların kullanıldığına bağlı olarak gizli anahtar veya ortak anahtarı da paylaşır.

Microsoft Identity platform iki ana işlevi yerine getiren bir modeli kullanarak uygulamaları temsil eder:

* Uygulamayı desteklediği kimlik doğrulama protokollerine göre tanımla
* Kimlik doğrulaması için gereken tüm tanımlayıcıları, URL 'Leri, gizli dizileri ve ilgili bilgileri sağlayın

Microsoft Identity Platform:

* Çalışma zamanında kimlik doğrulamasını desteklemek için gereken tüm verileri tutar
* Bir uygulamanın erişmesi gerekebilecek kaynakları ve belirli bir isteğin yerine getirilmesi gereken koşulları belirlemek için tüm verileri tutar
* Uygulama geliştiricisinin kiracısında ve diğer Azure AD kiracılarında uygulama sağlamayı uygulamak için altyapı sağlar
* Belirteç istek süresi boyunca Kullanıcı onayını işler ve kiracılar genelinde uygulamaların dinamik sağlamasını kolaylaştırır

Onay, bir istemci uygulamanın, kaynak sahibi adına belirli izinler altında korumalı kaynaklara erişmesi için bir kaynak sahibi verme yetkilendirmesi işlemidir. Microsoft Identity Platform:

* Kullanıcıların ve yöneticilerin uygulamanın kendileri adına kaynaklara erişmesine dinamik olarak onay vermesini veya reddetmesini sağlar.
* Yöneticilerin uygulamaların gerçekleştirebilecekleri işlemler, belirli uygulamalara erişebilecek kullanıcılar ve erişilen dizin kaynakları hakkında son kararı vermesini sağlar.

Microsoft Identity platformunda bir [uygulama nesnesi](developer-glossary.md#application-object) bir uygulamayı açıklar. Dağıtım zamanında, Microsoft Identity platform bir dizin veya kiracı içindeki bir uygulamanın somut örneğini temsil eden bir [hizmet sorumlusu](developer-glossary.md#service-principal-object)oluşturmak için uygulama nesnesini bir şema olarak kullanır. Hizmet sorumlusu, uygulamanın belirli bir hedef dizinde gerçekten ne yapabileceğini, bunu kullanabilecek kaynakları, hangi kaynakların erişebileceğini ve bu şekilde olduğunu tanımlar. Microsoft Identity platform bir uygulama nesnesinden **onay**aracılığıyla bir hizmet sorumlusu oluşturur.

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

## <a name="web-app-sign-in-flow-with-microsoft-identity-platform"></a>Microsoft Identity platform ile Web uygulaması oturum açma akışı

Bir kullanıcı tarayıcıda bir Web uygulamasına gittiğinde aşağıdakiler olur:

* Web uygulaması, kullanıcının kimlik doğrulamasının yapılıp yapılmayacağını belirler.
* Kullanıcının kimliği doğrulanmadıysa, Web uygulaması Kullanıcı oturumu açmak için Azure AD 'ye temsilciler atar. Bu oturum açma, kuruluşun ilkesiyle uyumlu olacaktır. Bu, kullanıcıdan çok faktörlü kimlik doğrulaması kullanarak veya bir parola (örneğin, Windows Hello kullanarak) kullanarak kimlik bilgilerini girmesini isteyebilir.
* Kullanıcıdan, istemci uygulamasına gereken erişimi onaylaması istenir. Bu nedenle, Microsoft Identity platform kullanıcının tarafından onaylanan erişimi temsil eden belirteçleri sunabilmesi için istemci uygulamalarının Azure AD 'ye kaydedilmesi gerekir.

Kullanıcının kimliği başarıyla doğrulandı:

* Microsoft Identity platform, Web uygulamasına bir belirteç gönderir.
* Kullanıcının tarayıcı tanımlama bilgisi jar içindeki kimliğini içeren Azure AD 'nin etki alanı ile ilişkili bir tanımlama bilgisi kaydedilir. Bir uygulama Microsoft Identity platform yetkilendirme uç noktasına gitmek için tarayıcıyı bir sonraki sefer kullandığında, tarayıcı tanımlama bilgisini Kullanıcı yeniden oturum açmak zorunda kalmayacak şekilde gösterir. Bu, SSO 'nun elde edilmesi için de bir yoldur. Tanımlama bilgisi Azure AD tarafından üretilir ve yalnızca Azure AD tarafından anlaşılabilirler.
* Web uygulaması, belirteci doğrular. Doğrulama başarılı olursa, Web uygulaması korumalı sayfayı görüntüler ve tarayıcının tanımlama bilgisi jar 'e bir oturum tanımlama bilgisi kaydeder. Kullanıcı başka bir sayfaya gittiğinde, Web uygulaması, oturum tanımlama bilgisine göre kullanıcının kimliğinin doğrulandığını bilir.

Aşağıdaki sıra diyagramı bu etkileşimi özetler:

![Web uygulaması kimlik doğrulama işlemi](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Web uygulaması, kullanıcının kimliğinin doğrulanmadığını nasıl belirler

Web uygulaması geliştiricileri, kimlik doğrulaması gerektirdiğini veya yalnızca belirli sayfaların olduğunu belirtebilir. Örneğin, ASP.NET/ASP.NET Core 'da, denetleyici eylemlerine `[Authorize]` özniteliği eklenerek yapılır.

Bu öznitelik, ASP.NET Kullanıcı kimliğini içeren bir oturum tanımlama bilgisinin varlığını denetlamasına neden olur. Bir tanımlama bilgisi yoksa, ASP.NET kimlik doğrulamasını belirtilen kimlik sağlayıcısına yönlendirir. Kimlik sağlayıcısı Azure AD ise, Web uygulaması, oturum açma iletişim kutusunu gösteren `https://login.microsoftonline.com`kimlik doğrulamasını yeniden yönlendirir.

### <a name="how-a-web-app-delegates-sign-in-to-microsoft-identity-platform-and-obtains-a-token"></a>Web uygulamasının Microsoft Identity platformunda oturum açması ve bir belirteç alacağı

Tarayıcı aracılığıyla Kullanıcı kimlik doğrulaması gerçekleşir. OpenID Protokolü standart HTTP protokol iletilerini kullanır.

* Web uygulaması, Microsoft Identity platform 'ı kullanmak için tarayıcıya bir HTTP 302 (Redirect) gönderir.
* Kullanıcının kimliği doğrulandığında, Microsoft Identity platform tarayıcı aracılığıyla yeniden yönlendirme kullanarak belirteci Web uygulamasına gönderir.
* Yeniden yönlendirme, Web uygulaması tarafından yeniden yönlendirme URI 'SI biçiminde sağlanır. Bu yeniden yönlendirme URI 'SI, Azure AD uygulama nesnesi ile kaydedilir. Uygulama çeşitli URL 'Lerde dağıtılabilmesi için birkaç yeniden yönlendirme URI 'si olabilir. Bu nedenle, Web uygulamasının kullanılacak yeniden yönlendirme URI 'sini belirtmesi de gerekecektir.
* Azure AD, Web uygulaması tarafından gönderilen yeniden yönlendirme URI 'sinin, uygulamanın kayıtlı yeniden yönlendirme URI 'lerinden biri olduğunu doğrular.

## <a name="desktop-and-mobile-app-sign-in-flow-with-microsoft-identity-platform"></a>Microsoft Identity platform ile masaüstü ve mobil uygulama oturum açma akışı

Yukarıda açıklanan akış, küçük farklılıklar ile masaüstü ve mobil uygulamalar için geçerlidir.

Masaüstü ve mobil uygulamalar, kimlik doğrulaması için katıştırılmış bir Web denetimi veya bir sistem tarayıcısı kullanabilir. Aşağıdaki diyagramda, bir masaüstü veya mobil uygulamanın, erişim belirteçleri almak ve Web API 'Lerini çağırmak için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) nasıl kullandığı gösterilmektedir.

![Masaüstü uygulaması nasıl görünür](media/authentication-scenarios/desktop-app-how-it-appears-to-be.png)

MSAL belirteçleri almak için bir tarayıcı kullanır. Web uygulamalarında olduğu gibi, kimlik doğrulaması da Microsoft Identity platformu 'na atanmış olur.

Azure AD, Web Apps için olduğu gibi tarayıcıda aynı kimlik tanımlama bilgisini kaydettiği için, yerel veya mobil uygulama sistem tarayıcısını kullanıyorsa, karşılık gelen Web uygulamasıyla SSO 'yu hemen alır.

Varsayılan olarak, MSAL sistem tarayıcısını kullanır. Bu özel durum, daha tümleşik bir kullanıcı deneyimi sağlamak için gömülü bir denetimin kullanıldığı masaüstü uygulamalarının .NET Framework.

## <a name="next-steps"></a>Sonraki adımlar

* Ortak koşulları öğrenmek için [Microsoft Identity Platform geliştirici sözlüğü](developer-glossary.md) ' ne bakın.
* Microsoft Identity platform tarafından desteklenen kullanıcıların kimliğini doğrulamak için diğer senaryolar hakkında daha fazla bilgi edinmek için bkz. [kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) .
* Microsoft hesapları, Azure AD hesapları ve Azure AD B2C kullanıcıları tek ve kolaylaştırılmış bir programlama modelinde çalışan uygulamalar geliştirmenize yardımcı olan Microsoft kitaplıkları hakkında bilgi edinmek için bkz. [msal kitaplıkları](msal-overview.md) .
* App Service uygulamanızın kimlik doğrulamasını nasıl yapılandıracağınızı öğrenmek için bkz. [Microsoft Identity platformu ile App Service tümleştirme](/azure/app-service/configure-authentication-provider-aad) .
