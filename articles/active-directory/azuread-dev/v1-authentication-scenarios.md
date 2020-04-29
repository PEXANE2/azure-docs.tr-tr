---
title: Geliştiriciler için Azure AD (v 1.0) | Mavisi
description: Uygulama modeli, API, sağlama ve en yaygın kimlik doğrulama senaryoları gibi geliştiriciler için Azure AD kimlik doğrulaması temelleri hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 10/14/2019
ms.author: ryanwi
ms.reviewer: saeeda, sureshja, hirsin
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 36b39f3706db615e40ebfadebf36be4d8b29c33e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80154738"
---
# <a name="what-is-authentication"></a>Kimlik doğrulaması nedir?

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

*Kimlik doğrulaması*, taraflardan geçerli kimlik bilgileri isteyerek kimlik ve erişim denetimi için kullanılacak bir güvenlik sorumlusu oluşturmaktır. Daha basit bir deyişle söylediğiniz kişi olduğunuzu kanıtlama işlemidir. Kimlik doğrulaması bazen AuthN şeklinde kısaltılabilir.

*Yetkilendirme*, kimliği doğrulanmış bir güvenlik sorumlusuna bir şeyi yapma iznini vermektir. Erişme iznine sahip olduğunuz verileri ve bu verilerle gerçekleştirebileceğiniz işlemleri belirtir. Yetkilendirme bazen AuthZ şeklinde kısaltılabilir.

Geliştiriciler için Azure Active Directory (v 1.0) (Azure AD), OAuth 2,0 ve OpenID Connect gibi endüstri standardı protokoller desteğiyle ve kodlamaya hızlı bir şekilde başlamanıza yardımcı olması için farklı platformlar için açık kaynak kitaplıkların yanı sıra uygulama geliştiricileri için kimlik doğrulamasını basitleştirir.

Azure AD programlama modelinde iki temel kullanım örneği vardır:

* OAuth 2.0 yetki verme akışı: Burada kaynak sahibi istemci uygulamayı yetkilendirerek istemcinin kaynak sahibinin kaynaklarına erişmesini sağlar.
* İstemci tarafından kaynak erişimi sırasında: Kaynak sunucusu erişim belirtecindeki talep değerlerini kullanarak erişim denetimi kararları alır.

## <a name="authentication-basics-in-azure-ad"></a>Azure AD 'de kimlik doğrulama temelleri

Kimliğin gerekli olduğu en basit senaryoyu ele alalım: Bir kullanıcının web tarayıcısından bir web uygulaması için kimlik doğrulamasından geçmesi gerekiyor. Aşağıdaki diyagramda bu senaryo gösterilmektedir:

![Web uygulamasında oturum açmaya genel bakış](./media/v1-authentication-scenarios/auth-basics-microsoft-identity-platform.svg)

Diyagramda gösterilen çeşitli bileşenler hakkında bilmeniz gerekenler şunlardır:

* Azure AD, kimlik sağlayıcısıdır. Kimlik sağlayıcısı, bir kuruluşun dizininde bulunan kullanıcıların ve uygulamaların kimliğini doğrulamadan sorumludur ve bu kullanıcıların ve uygulamaların başarıyla doğrulanması sırasında güvenlik belirteçleri verir.
* Azure AD 'de kimlik doğrulaması yapmak isteyen bir uygulama Azure Active Directory (Azure AD) içinde kayıtlı olmalıdır. Azure AD, dizindeki her uygulamayı kaydeder ve benzersiz bir şekilde tanımlar.
* Geliştiriciler açık kaynak Azure AD kimlik doğrulaması kitaplıklarını kullanarak protokolün ayrıntılarıyla ilgilenmeden kolayca kimlik doğrulaması gerçekleştirebilir. Daha fazla bilgi için bkz. Microsoft Identity Platform [v 2.0 kimlik doğrulama kitaplıkları](../develop/reference-v2-libraries.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) ve [v 1.0 kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md).
* Bir kullanıcının kimliği doğrulandıktan sonra, uygulamanın, kimlik doğrulamasının başarılı olduğundan emin olmak için kullanıcının güvenlik belirtecini doğrulaması gerekir. Farklı diller ve çerçeveler için uygulamanın yapması gereken işlemleri anlatan birçok hızlı başlangıç, öğretici ve kod örneği mevcuttur.
  * Hızlıca uygulama derlemek ve belirteç alma, belirteçleri yenileme, kullanıcı oturumu açma ve kullanıcı bilgilerini görüntüleme gibi işlevler eklemek için belgelerin **Hızlı başlangıçlar** bölümüne bakın.
  * Erişim belirteçleri alma ve bunları Microsoft Graph API ve diğer API'lere yapılan çağrılarda kullanma, OpenID Connect kullanarak geleneksel bir web tarayıcısı tabanlı uygulamaya Microsoft ile oturum açma özelliğini ekleme ve daha fazla geliştirici görevi için ayrıntılı ve senaryo tabanlı yordamlar için belgelerin **Öğreticiler** bölümüne bakın.
  * Kod örneklerini indirmek için [GitHub](https://github.com/Azure-Samples?q=active-directory)'a gidin.
* Kimlik doğrulaması işlemi için istek ve yanıt akışı OAuth 2.0, OpenID Connect, WS-Federation veya SAML 2.0 gibi kullandığınız kimlik doğrulaması protokolü tarafından belirlenir. Protokoller hakkında daha fazla bilgi için, belgelerinin **> kimlik doğrulama protokolü** bölümüne bakın.

Yukarıdaki örnek senaryoda uygulamaları bu iki role göre sınıflandırabilirsiniz:

* Kaynaklara güvenli bir şekilde erişmesi gereken uygulamalar
* Kaynağın rolünü oynaması gereken uygulamalar

### <a name="how-each-flow-emits-tokens-and-codes"></a>Her akışın belirteçleri ve kodları nasıl yayar

İstemcinizin nasıl oluşturulduğuna bağlı olarak, Azure AD tarafından desteklenen kimlik doğrulama akışlarının birini (veya birkaçını) kullanabilir. Bu akışlar çeşitli belirteçler (id_tokens, yenileme belirteçleri, erişim belirteçleri) ve yetkilendirme kodlarını oluşturabilir ve bunların çalışmasını sağlamak için farklı belirteçler gerektirebilir. Bu grafik genel bakış sağlar:

|Akış | Gerektirmeyen | id_token | erişim belirteci | belirteci Yenile | yetkilendirme kodu | 
|-----|----------|----------|--------------|---------------|--------------------|
|[Yetkilendirme kodu akışı](v1-protocols-oauth-code.md) | | x | x | x | x|  
|[Örtük akış](v1-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Karma OıDC akışı](v1-protocols-openid-connect-code.md#get-access-tokens)| | x  | |          |            x   |
|[Belirteç satın alma yenileme](v1-protocols-oauth-code.md#refreshing-the-access-tokens) | belirteci Yenile | x | x | x| |
|[On-behalf-of akışı](v1-oauth2-on-behalf-of-flow.md) | erişim belirteci| x| x| x| |
|[İstemci kimlik bilgileri](v1-oauth2-client-creds-grant-flow.md) | | | x (yalnızca uygulama)| | |

Örtülü mod aracılığıyla yayınlanan belirteçlerin, tarayıcıya URL aracılığıyla geri geçirilme nedeniyle bir uzunluk sınırlaması vardır (burada `response_mode` `query` veya `fragment`).  Bazı tarayıcıların, tarayıcı çubuğuna koyabileceğiniz ve çok uzun olduğunda başarısız olan URL 'nin boyutunda bir sınırı vardır.  Bu nedenle, bu belirteçlerin `groups` veya `wids` talepleri yoktur. 

Temel kavramları oturttuğunuza göre okumaya devam ederek kimlik uygulaması modeli ve API, Azure AD'te sağlama ve Azure AD tarafından desteklenen yaygın senaryolar hakkında ayrıntılı bilgiler bölümlerini inceleyebilirsiniz.

## <a name="application-model"></a>Uygulama modeli

Azure AD uygulamaları iki ana işlevi gerçekleştirmek üzere tasarlanmış bir modelle temsil eder:

* **Uygulamayı desteklediği kimlik doğrulaması protokollerine göre tanımlama**: Bu model tüm tanımlayıcıların, URL'lerin, gizli dizilerin ve kimlik doğrulaması sırasında gerekli olan ilgili bilgilerin listelenmesini kapsar. Burada Azure AD:

    * Çalışma zamanında kimlik doğrulamasını gerçekleştirmek için gerekli tüm verileri barındırır.
    * Bir uygulamanın erişmesi gerekebilecek kaynakları ve belirli bir isteğin gerçekleştirilip gerçekleştirilmeyeceğini ve hangi koşullarda gerçekleştirileceğini belirlemek için gerekli tüm verileri barındırır.
    * Uygulama geliştiricisinin kiracısında ve diğer Azure AD kiracısında uygulama sağlamak için gerekli altyapıyı sağlar.

* **Belirteç istendiğinde kullanıcı onayını işleme ve kiracılar arasında uygulamaların dinamik sağlamasını kolaylaştırma**: Burada Azure AD:

    * Kullanıcıların ve yöneticilerin uygulamanın kendileri adına kaynaklara erişmesine dinamik olarak onay vermesini veya reddetmesini sağlar.
    * Yöneticilerin uygulamaların gerçekleştirebilecekleri işlemler, belirli uygulamalara erişebilecek kullanıcılar ve erişilen dizin kaynakları hakkında son kararı vermesini sağlar.

Azure AD'de **uygulama nesnesi**, bir uygulamayı soyut varlık olarak tanımlar. Geliştiriciler uygulamalarla çalışır. Dağıtım zamanında Azure AD belirli bir uygulama nesnesini şema olarak kullanarak dizin veya kiracı içindeki uygulama örneğini temsil eden bir **hizmet sorumlusu** oluşturur. Uygulamanın belirli bir hedef dizinde yapabileceklerini, onu kullanabilecek kişileri, erişim sağlayabileceği kaynakları ve diğer bilgileri tanımlayan hizmet sorumlusudur. Azure AD, uygulama nesnesinden hizmet sorumlusu oluşturmak için **onayları** kullanır.

Aşağıdaki diyagramda onay temelli basitleştirilmiş bir Azure AD sağlama akışı gösterilmektedir.  Bu durumda, kiracının uygulamanın sahibi olduğu iki kiracı (A ve B) vardır ve B kiracısı bir hizmet sorumlusu aracılığıyla uygulamayı örnekledir.  

![Onay temelli basitleştirilmiş sağlama akışı](./media/v1-authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

Bu sağlama akışında:

1. B kiracısından bir kullanıcı uygulamayla oturum açmaya çalışır, yetkilendirme uç noktası uygulama için bir belirteç ister.
1. Kimlik doğrulaması için Kullanıcı kimlik bilgileri alındı ve doğrulandı
1. Kullanıcıdan, B kiracısına erişim kazanmak için uygulamanın onayını sağlaması istenir
1. Azure AD, B kiracısında bir hizmet sorumlusu oluşturmak için bir şema olarak kiracı 'daki uygulama nesnesini kullanır
1. Kullanıcı istenen belirteci alır

Bu işlemi diğer kiracılar için (C, D vb.) istediğiniz kadar tekrarlayabilirsiniz. Kiracı A, uygulama için şemayı saklar (uygulama nesnesi). Uygulama için onay verilen diğer kiracıların kullanıcıları ve yöneticileri kiracıdaki hizmet sorumlusu nesnesini kullanarak uygulamanın gerçekleştirmesine izin verilen işlemleri belirler. Daha fazla bilgi için bkz. [Microsoft Identity platformunda uygulama ve hizmet sorumlusu nesneleri](../develop/app-objects-and-service-principals.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

## <a name="claims-in-azure-ad-security-tokens"></a>Azure AD güvenlik belirteçlerindeki talepler

Azure AD tarafından oluşturulan güvenlik belirteçlerinde (erişim ve kimlik belirteçleri) talepler, başka bir deyişle kimliği doğrulanan özne hakkındaki onaylanmış bilgiler bulunur. Uygulamalar talepleri birçok görev için kullanabilir; bazıları:

* Belirteci doğrulama
* Öznenin dizin kiracısını tanımlama
* Kullanıcı bilgilerini görüntüleme
* Öznenin yetkilendirme durumunu belirleme

Herhangi bir güvenlik belirtecindeki talepler belirteç türüne, kullanıcının kimliğini doğrulamak için kullanılan kimlik bilgisi türüne ve uygulama yapılandırmasına göre değişiklik gösterir.

Aşağıdaki tabloda Azure AD tarafından düzenlenen talep türleri hakkında kısa açıklamalara yer verilmiştir. Daha ayrıntılı bilgi için bkz. Azure AD tarafından verilen [erişim belirteçleri](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) ve [Kimlik belirteçleri](../develop/id-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) .

| İste | Açıklama |
| --- | --- |
| Uygulama Kimliği | Belirteci kullanan uygulamayı tanımlar. |
| Hedef kitle | Belirtecin gönderileceği alıcı kaynağını tanımlar. |
| Uygulama Kimlik Doğrulaması Bağlamı Sınıf Başvurusu | İstemcinin kimliğinin nasıl doğrulandığını belirtir (genel istemci veya gizli istemci). |
| Kimlik Doğrulaması Anı | Kimlik doğrulamasının gerçekleştirildiği tarihi ve saati kaydeder. |
| Kimlik Doğrulama Yöntemi | Belirtecin kimlik doğrulama yöntemini (parola, sertifika vb.) belirtir. |
| Ad | Kullanıcının Azure AD'deki adını sağlar. |
| Gruplar | Kullanıcının üyesi olduğu Azure AD gruplarının nesne kimliklerini içerir. |
| Kimlik Sağlayıcı | Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. |
| Verilme Zamanı | Belirtecin verilme zamanını kaydeder ve bu değer genellikle belirtecin ne kadar güncel olduğunu anlamak için kullanılır. |
| Veren | Belirteci oluşturan STS ve Azure AD kiracısını tanımlar. |
| Soyadı | Kullanıcının Azure AD'deki soyadını sağlar. |
| Adı | Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. |
| Nesne Kimliği | Öznenin Azure AD'deki değişmez ve benzersiz tanıtıcısını içerir. |
| Roller | Kullanıcıya verilmiş olan Azure AD Uygulama Rollerinin kolay adlarını içerir. |
| Kapsam | İstemci uygulamasına verilmiş olan izinleri belirtir. |
| Özne | Belirtecin bilgi verdiği sorumluyu belirtir. |
| Kiracı Kimliği | Belirteci düzenleyen dizin kiracısının değişmez ve benzersiz tanıtıcısını içerir. |
| Belirteç Ömrü | Belirtecin geçerli olduğu zaman aralığını tanımlar. |
| Kullanıcı Asıl Adı | Öznenin kullanıcı asıl adını içerir. |
| Sürüm | Belirtecin sürüm numarasını içerir. |

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Identity platformunda desteklenen uygulama türleri ve senaryoları](app-types.md) hakkında bilgi edinin
