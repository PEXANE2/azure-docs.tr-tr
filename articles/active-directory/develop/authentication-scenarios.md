---
title: Microsoft kimlik platformunda kimlik doğrulama | Azure
description: Microsoft kimlik platformunda (v2.0) kimlik doğrulamanın temelleri hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/03/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: e78f822a88b093992f065a509c2250e6a5c0dec2
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885574"
---
# <a name="authentication-basics"></a>Kimlik doğrulaması temel bilgileri

## <a name="what-is-authentication"></a>Kimlik doğrulama nedir

Bu makalede, korumalı web uygulamaları, web API'leri veya korumalı Web API'lerini çağıran uygulamalar oluşturmak için anlamanız gereken kimlik doğrulama kavramlarının birçoğunu kapsar. Bilmediğiniz bir terim görürseniz, temel kavramları kapsayan [sözlüğümüzünü](developer-glossary.md) veya [Microsoft kimlik platformu videolarımızı](identity-videos.md) deneyin.

**Kimlik doğrulama,** söylediğiniz kişi olduğunuzu kanıtlama işlemidir. Kimlik doğrulaması bazen AuthN şeklinde kısaltılabilir.

**Yetkilendirme,** bir şey yapmak için doğrulanmış bir tarafa izin verme eylemidir. Hangi verilere erişebileceğinizi ve bu verilerle neler yapabileceğinizi belirtir. Yetkilendirme bazen AuthZ şeklinde kısaltılabilir.

Uygulamalar, birden çok uygulama üzerinden kullanıcı eklemeniz veya kaldırmanız gerektiğinde yüksek bir yönetim yükü oluşturan, her birinin kendi kullanıcı adı ve parola bilgilerini saklayan uygulamalar oluşturmak yerine, bu sorumluluğu merkezi bir kimlik sağlayıcısına devredebilir.

Azure Etkin Dizin (Azure AD), bulutta merkezi bir kimlik sağlayıcısıdır. Kimlik doğrulaması ve yetkilendirmesini ona devretmek, kullanıcının belirli bir konumda olmasını gerektiren Koşullu Erişim ilkeleri, çok faktörlü kimlik doğrulamanın kullanımı gibi senaryoların yanı sıra bir kullanıcının bir kez oturum açmasını ve aynı merkezi dizini paylaşan tüm web uygulamalarında otomatik olarak oturum açmasını sağlar. Bu özellik, Tek İşaret Açma (SSO) olarak adlandırılır.

Merkezi bir kimlik sağlayıcısı, dünyanın dört bir yanında bulunan ve kuruluşun ağında oturum açmamış olan uygulamalar için daha da önemlidir. Azure AD, kullanıcıların kimliğini doğrular ve erişim belirteçleri sağlar. [Erişim belirteci,](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#access-token) yetkilendirme sunucusu tarafından verilen bir güvenlik belirtecidir. Kullanıcı ve belirteç amaçlandığı uygulama hakkında bilgi içerir; Web API'lerine ve diğer korumalı kaynaklara erişmek için kullanılabilir.

Microsoft kimlik platformu, [OAuth 2.0](https://oauth.net/2/) ve [OpenID Connect](https://openid.net/connect/)gibi endüstri standardı protokollerin yanı sıra hızlı bir şekilde kodlamaya başlamanıza yardımcı olacak farklı platformlar için açık kaynak kitaplıkları desteğiyle, hizmet olarak kimlik sağlayarak uygulama geliştiricileri için kimlik doğrulamayı kolaylaştırır. Geliştiricilerin tüm Microsoft kimliklerini oturum açan uygulamalar oluşturmasına, Microsoft [Graph'ı,](https://developer.microsoft.com/graph/)diğer Microsoft API'lerini veya geliştiricilerin oluşturduğu API'leri arama belirteçlerini almalarına olanak tanır. Daha fazla bilgi için Microsoft [kimlik platformunun Evrimi'ne](about-microsoft-identity-platform.md)bakın.

### <a name="tenants"></a>Kiracılar

Bulut kimlik sağlayıcısı birçok kuruluşa hizmet vermektedir. Azure AD, farklı kuruluşlardan kullanıcıları ayrı tutmak için, kuruluş başına bir kiracıyla birlikte kiracı olarak bölümlere ayrılır.

Kiracılar kullanıcıları ve ilişkili uygulamalarını izler. Microsoft kimlik platformu, kişisel Microsoft hesaplarıyla oturum açan kullanıcıları da destekler.

Azure AD ayrıca, kuruluşların google hesabı gibi sosyal kimlikleri kullanarak kullanıcılarda, genellikle müşterilerde oturum açabilmesi için Azure Active Directory B2C de sağlar. Daha fazla bilgi için Azure [Active Directory B2C belgelerine](https://docs.microsoft.com/azure/active-directory-b2c) bakın.

### <a name="security-tokens"></a>Güvenlik belirteçleri

Güvenlik belirteçleri kullanıcılar ve uygulamalar hakkında bilgi içerir. Azure AD, talep içeren JSON tabanlı belirteçleri (JWTs) kullanır.

Talep, [bir istemci uygulaması](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#client-application) veya kaynak [sahibi](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#resource-owner)gibi bir varlık la ilgili iddiaları [kaynak sunucusu](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#resource-server)gibi başka bir varlığa sağlar.

Talepler, belirteç konusuyla ilgili gerçekleri aktaran ad/değer çiftleridir. Örneğin, bir [talep, yetkilendirme sunucusu](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#authorization-server)tarafından kimlik doğrulaması yapılan güvenlik ilkesiyle ilgili gerçekleri içerebilir. Belirli bir belirteçte bulunan talepler, belirteç türü, öznenin kimliğini doğrulamak için kullanılan kimlik bilgisi türü, uygulama yapılandırması ve benzeri birçok şeye bağlıdır.

Uygulamalar gibi çeşitli görevler için talep kullanabilirsiniz:

* Belirteci doğrulama
* Belirteç öznesinin kiracısını tanımlama
* Kullanıcı bilgilerini görüntüleme
* Öznenin yetkilendirmesinin belirlenmesi

Bir talep, aşağıdakiler gibi bilgiler sağlayan anahtar değer çiftlerinden oluşur:

* Belirteci oluşturan Güvenlik Belirteci Sunucusu
* Belirteci oluşturulduğu tarih
* Konu (kullanıcı gibi--daemons hariç)
* Belirteç oluşturulduğu uygulama olan hedef kitle
* Belirteci isteyen uygulama (istemci). Web uygulamaları söz konusu olduğunda, bu hedef kitle ile aynı olabilir

Daha ayrıntılı talep bilgileri için [erişim belirteçleri](access-tokens.md) ve [kimlik belirteçleri](id-tokens.md)bakın.

Belirteci oluşturulan uygulamaya, kullanıcıda oturum açan web uygulamasına veya belirteci doğrulamak için Web API'sinin çağrılması gereken uygulamaya kalmış. Belirteç, Güvenlik Belirteci Sunucusu (STS) tarafından özel bir anahtarla imzalanır. STS ilgili ortak anahtarı yayımlar. Bir belirteci doğrulamak için uygulama, imzanın özel anahtar kullanılarak oluşturulduğunu doğrulamak için STS ortak anahtarını kullanarak imzayı doğrular.

Belirteçler yalnızca sınırlı bir süre için geçerlidir. Genellikle STS bir çift belirteç sağlar: uygulamaya veya korumalı kaynağa erişmek için bir erişim belirteci ve erişim belirteci sona ermek üzereyken erişim belirteci yenilemek için kullanılan bir yenileme belirteci.

Erişim belirteçleri, üstbilgideki taşıyıcı belirteci olarak `Authorization` bir Web API'sine aktarılır. Bir uygulama STS'ye yeni bir belirteç sağlayabilir ve kullanıcının uygulamaya erişimi iptal edilmediyse, yeni bir erişim belirteci ve yeni bir yenileme belirteci geri alır. İşletmeden ayrılan birinin senaryosu bu şekilde işlenir. STS yenileme belirteci aldığında, kullanıcı artık yetkili değilse başka bir geçerli erişim belirteci vermez.

### <a name="how-each-flow-emits-tokens-and-codes"></a>Her akış nasıl belirteçleri ve kodları yayır

İstemcinizin nasıl oluşturulabildiğine bağlı olarak, Azure AD tarafından desteklenen kimlik doğrulama akışlarından birini (veya birkaçını) kullanabilir. Bu akışlar, yetkilendirme kodlarının yanı sıra çeşitli belirteçler (id_tokens, yenileme belirteçleri, erişim belirteçleri) üretebilir ve bunları çalışması için farklı belirteçler gerektirebilir. Bu grafik genel bir bakış sağlar:

|Akış | Gerektirir | id_token | erişim belirteci | belirteci yenileme | yetkilendirme kodu | 
|-----|----------|----------|--------------|---------------|--------------------|
|[Yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md) | | x | x | x | x|  
|[Örtük akış](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Hibrid OIDC akışı](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Belirteç itfasını yenile](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | belirteci yenileme | x | x | x| |
|[On-behalf-of akışı](v2-oauth2-on-behalf-of-flow.md) | erişim belirteci| x| x| x| |
|[İstemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md) | | | x (yalnızca uygulama)| | |

Örtük mod üzerinden verilen belirteçler, URL üzerinden tarayıcıya geri aktarılması `response_mode` `query` nedeniyle `fragment`bir uzunluk sınırlamasına sahiptir (nerede veya).  Bazı tarayıcıların, tarayıcı çubuğuna konulabilecek ve çok uzun olduğunda başarısız olabilecek URL boyutuyla ilgili bir sınırı vardır.  Bu nedenle, bu belirteçleri yok `groups` veya `wids` iddiaları yok. 

Artık temel bilgilere genel bir bakışa sahip olduğunuza göre, kimlik uygulaması modelini ve API'yi anlamak için okumaya devam edin, Azure AD'de sağlamanın nasıl çalıştığını öğrenin ve Azure AD'nin desteklediği yaygın senaryolar hakkında ayrıntılı bilgilere bağlantılar edinin.

## <a name="application-model"></a>Uygulama modeli

Uygulamalar, kullanıcıların kendilerini oturum açabilir veya oturum açabilir. Azure AD tarafından desteklenen oturum açma senaryoları hakkında bilgi edinmek için [Kimlik Doğrulama akışlarına ve uygulama senaryolarına](authentication-flows-app-scenarios.md) bakın.

Bir kimlik sağlayıcısının bir kullanıcının belirli bir uygulamaya erişimi olduğunu bilmesi için hem kullanıcının hem de uygulamanın kimlik sağlayıcısına kayıtlı olması gerekir. Uygulamanızı Azure AD'ye kaydettiğinizde, uygulamanız için Azure AD ile tümleştirmesine olanak tanıyan bir kimlik yapılandırması sağlarsınız. Uygulamayı kaydetmek aynı zamanda şunları yapmanızı sağlar:

* Oturum açma iletişim kutusunda uygulamanızın markasını özelleştirin. Bu önemlidir, çünkü bu bir kullanıcının uygulamanızla yaşayacağı ilk deneyimdir.
* Kullanıcıların yalnızca kuruluşunuza aitse oturum açmalarına izin vermek isteyip istemediğinize karar verin. Bu tek bir kiracı uygulamasıdır. Veya kullanıcıların herhangi bir iş veya okul hesabını kullanarak oturum açmalarına izin verin. Bu çok kiracılı bir uygulamadır. Ayrıca kişisel Microsoft hesaplarına veya LinkedIn, Google ve benzeri ülkelerden bir sosyal hesaba da izin verebilirsiniz.
* Kapsam izinleri isteyin. Örneğin, oturum açmış kullanıcının profilini okuma izni veren "user.read" kapsamını isteyebilirsiniz.
* Web API'nıza erişimi tanımlayan kapsamları tanımlayın. Genellikle, bir uygulama API'nize erişmek istediğinde, tanımladığınız kapsamlar için izin istemesi gerekir.
* Uygulamanın kimliğini Azure AD ile kanıtlayan bir sırrı Azure AD ile paylaşın.  Bu, uygulamanın gizli bir istemci uygulaması olduğu durumlarda geçerlidir. Gizli istemci uygulaması, kimlik bilgilerini güvenli bir şekilde tutabilen bir uygulamadır. Kimlik bilgilerini depolamak için güvenilir bir arka uç sunucusu gerektirirler.

Kaydolduktan sonra, uygulama, belirteçleri istediğinde uygulamanın Azure AD ile paylaştığı benzersiz bir tanımlayıcı verilir. Uygulama gizli bir [istemci uygulamasıysa,](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#client-application)sertifikaların veya sırların kullanılıp kullanılmadığına bağlı olarak gizli veya ortak anahtarı da paylaşır.

Microsoft kimlik platformu, iki ana işlevi yerine getiren bir modeli kullanarak uygulamaları temsil eder:

* Uygulamayı desteklediği kimlik doğrulama protokollerine göre tanımlama
* Kimlik doğrulaması için gerekli olan tüm tanımlayıcıları, URL'leri, sırları ve ilgili bilgileri sağlayın

Microsoft kimlik platformu:

* Çalışma zamanında kimlik doğrulamasını desteklemek için gereken tüm verileri tutar
* Bir uygulamanın hangi kaynaklara erişebileceğine ve belirli bir isteğin hangi koşullar altında yerine getirilmesi gerektiğine karar vermek için tüm verileri tutar
* Uygulama geliştiricisinin kiracısı içinde ve diğer Azure AD kiracılarına uygulama sağlama için altyapı sağlar
* Belirteç isteği süresi sırasında kullanıcı onayını işler ve uygulamaların kiracılar arasında dinamik olarak sağlanmasını kolaylaştırır

İzin, bir kaynak sahibinin, kaynak sahibi adına belirli izinler altında korunan kaynaklara erişmek için istemci başvurusu için yetki verme işlemidir. Microsoft kimlik platformu:

* Kullanıcıların ve yöneticilerin uygulamanın kendileri adına kaynaklara erişmesine dinamik olarak onay vermesini veya reddetmesini sağlar.
* Yöneticilerin uygulamaların gerçekleştirebilecekleri işlemler, belirli uygulamalara erişebilecek kullanıcılar ve erişilen dizin kaynakları hakkında son kararı vermesini sağlar.

Microsoft kimlik platformunda, bir [uygulama nesnesi](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#application-object) bir uygulamayı açıklar. Dağıtım sırasında, Microsoft kimlik platformu bir dizin veya kiracı içinde bir uygulamanın somut bir örneği temsil eden bir [hizmet anabilim](https://docs.microsoft.com/azure/active-directory/develop/developer-glossary#service-principal-object)oluşturmak için bir plan olarak uygulama nesnesi kullanır. Hizmet sorumlusu, uygulamanın belirli bir hedef dizinde gerçekte neler yapabileceğini, kimlerin kullanabileceğini, hangi kaynaklara erişebileceğini ve benzeri ni tanımlar. Microsoft kimlik **platformu, onay**yoluyla bir uygulama nesnesinden bir hizmet ilkesi oluşturur.

Aşağıdaki diyagram, rıza ile yönlendirilen basitleştirilmiş bir Microsoft kimlik platformu sağlama akışını gösterir. İki kiracı gösterir: A ve B. Kiracı A uygulama sahibi. Kiracı B, bir servis müdürü aracılığıyla başvuruyu anında gerçekletir.  

![Onay temelli basitleştirilmiş sağlama akışı](./media/authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

Bu sağlama akışında:

1. Kiracı B'den bir kullanıcı uygulamayla oturum açmaya çalışırsa, yetkilendirme bitiş noktası uygulama için bir belirteç ister.
1. Kullanıcı kimlik bilgileri kimlik doğrulaması için elde edilir ve doğrulanır.
1. Kullanıcıdan, uygulamanın kiracı B'ye erişebilmesi için onay vermesi istenir.
1. Microsoft kimlik platformu, kiracı B'de bir hizmet ilkesi oluşturmak için kiracı A'daki uygulama nesnesini bir plan olarak kullanır.
1. Kullanıcı istenen belirteci alır.

Ek kiracılar için bu işlemi yineleyebilirsiniz. Kiracı A, uygulamanın (uygulama nesnesi) planını korur. Uygulamanın onay landığı diğer tüm kiracıların kullanıcıları ve yöneticileri, uygulamanın her kiracıdaki ilgili hizmet ana nesnesi aracılığıyla ne yapmasına izin verileceğini kontrol eder. Daha fazla bilgi için [Microsoft kimlik platformundaki Uygulama ve hizmet ana nesnelerine](app-objects-and-service-principals.md)bakın.

## <a name="web-app-sign-in-flow-with-azure-ad"></a>Azure AD ile Web uygulaması oturum açma akışı

Bir kullanıcı tarayıcıda bir web uygulamasına gittiğinde aşağıdaki gibi olur:

* Web uygulaması, kullanıcının kimliğinin doğrulanıp doğrulanmayacağını belirler.
* Kullanıcının kimliği doğrulanmıyorsa, web uygulaması kullanıcıda oturum açmaları için Azure AD'ye devreder. Bu oturum açma, kullanıcıdan kimlik bilgilerini girmesini istemek, çok faktörlü kimlik doğrulaması kullanmak veya parola kullanmamak (örneğin Windows Hello'yu kullanmak) anlamına gelen kuruluşun ilkesiyle uyumlu olacaktır.
* Kullanıcıdan, istemci uygulamasının ihtiyaç duyduğu erişimi kabul etmesi istenir. Bu nedenle istemci uygulamaların Azure AD'ye kaydedilmesi gerekir, böylece Azure AD kullanıcının onay verdiği erişimi temsil eden belirteçler sunabilir.

Kullanıcı başarılı bir şekilde doğrulandığında:

* Azure AD, web uygulamasına bir belirteç gönderir.
* Azure AD'nin etki alanıyla ilişkili ve tarayıcının çerez kavanozundaki kullanıcının kimliğini içeren bir çerez kaydedilir. Bir uygulama Azure AD yetkilendirme bitiş noktasına gitmek için tarayıcıyı bir sonraki kez kullandığında, kullanıcının yeniden oturum açmaması için tarayıcı çerezi sunar. SSO'ya da bu şekilde ulaşılıyor. Çerez Azure AD tarafından üretilir ve yalnızca Azure AD tarafından anlaşılabilir.
* Web uygulaması daha sonra belirteci doğrular. Doğrulama başarılı olursa, web uygulaması korunan sayfayı görüntüler ve tarayıcının çerez kavanozuna bir oturum çerezi kaydeder. Kullanıcı başka bir sayfaya gittiğinde, web uygulaması kullanıcının oturum çerezine göre kimlik doğrusu olduğunu bilir.

Aşağıdaki sıralı diyagrambu etkileşimi özetler:

![web uygulaması kimlik doğrulama işlemi](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Bir web uygulaması kullanıcının kimlik doğrulaması olup olmadığını nasıl belirler?

Web uygulaması geliştiricileri, belirli sayfaların tamamının veya yalnızca kimlik doğrulaması gerekip gerekmediğini gösterebilir. Örneğin, ASP.NET/ASP.NET Core'da, bu denetleyici `[Authorize]` eylemlerine öznitelik eklenerek yapılır. 

Bu öznitelik, ASP.NET kullanıcının kimliğini içeren bir oturum çerezinin varlığını denetlemesine neden olur. Çerez yoksa, ASP.NET kimlik doğrulamasını belirtilen kimlik sağlayıcısına yönlendirir. Kimlik sağlayıcısı Azure AD ise, web uygulaması kimlik `https://login.microsoftonline.com`doğrulamayı oturum açma iletişim kutusunu görüntüleyen 'e yönlendirir.

### <a name="how-a-web-app-delegates-sign-in-to-azure-ad-and-obtains-a-token"></a>Bir web uygulaması Azure AD'de oturum açma ve belirteç edinme

Kullanıcı kimlik doğrulaması tarayıcı üzerinden gerçekleşir. OpenID protokolü standart HTTP iletişim kuralı iletilerini kullanır.
* Web uygulaması, Azure AD'yi kullanmak için tarayıcıya bir HTTP 302 (yönlendirme) gönderir.
* Kullanıcının kimliği doğrulandığında, Azure AD tarayıcı üzerinden yönlendirme kullanarak belirteci web uygulamasına gönderir.
* Yönlendirme, web uygulaması tarafından yeniden yönlendirme URI şeklinde sağlanır. Bu yeniden yönlendirme URI, Azure AD uygulama nesnesi ile kaydedilir. Uygulama birkaç URL'de dağıtılabileceğinden, birkaç yeniden yönlendirme URI'sı olabilir. Bu nedenle web uygulamasının kullanmak için URI'yi yeniden yönlendirmesini de belirtmeniz gerekir.
* Azure AD, web uygulaması tarafından gönderilen yeniden yönlendirme URI'nin uygulama için kayıtlı yönlendirme URI'lerinden biri olduğunu doğrular.

## <a name="desktop-and-mobile-app-sign-in-flow-with-azure-ad"></a>Azure AD ile masaüstü ve mobil uygulama oturum açma akışı

Yukarıda açıklanan akış, hafif farklılıklarla masaüstü ve mobil uygulamalariçin geçerlidir.

Masaüstü ve mobil uygulamalar kimlik doğrulaması için katıştırılmış bir Web denetimi veya sistem tarayıcısı kullanabilir. Aşağıdaki diyagram, bir Masaüstü veya mobil uygulamanın erişim belirteçleri edinmek ve web API'lerini aramak için Microsoft kimlik doğrulama kitaplığını (MSAL) nasıl kullandığını gösterir.

![Masaüstü uygulaması nasıl göründüğü](media/authentication-scenarios/desktop-app-how-it-appears-to-be.png)

MSAL belirteçleri almak için bir tarayıcı kullanır. Web uygulamalarında olduğu gibi, kimlik doğrulaması Azure AD'ye devredilir.

Azure AD, tarayıcıda web uygulamalarında olduğu gibi aynı kimlik çerezini kaydettiği için, yerel veya mobil uygulama sistem tarayıcısını kullanıyorsa, ilgili web uygulamasıyla hemen SSO'yu alır.

Varsayılan olarak, MSAL sistem tarayıcısını kullanır. Bunun istisnası, daha entegre bir kullanıcı deneyimi sağlamak için katıştırılmış denetimin kullanıldığı .NET Framework masaüstü uygulamalarıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Ortak terimleri tanımak için [Microsoft kimlik platformu geliştirici sözlüğüne](developer-glossary.md) bakın.
* Microsoft kimlik platformu tarafından desteklenen kullanıcıların kimlik doğrulamasına yönelik diğer senaryolar hakkında daha fazla bilgi edinmek için [Kimlik Doğrulama akışlarına ve uygulama senaryolarına](authentication-flows-app-scenarios.md) bakın.
* Microsoft Hesapları, Azure AD hesapları ve Azure AD B2C kullanıcılarıyla çalışan uygulamaları tek ve kolay bir programlama modelinde geliştirmenize yardımcı olan Microsoft kitaplıkları hakkında bilgi edinmek için [MSAL kitaplıklarına](msal-overview.md) bakın.
* Uygulama Hizmeti uygulamanız için kimlik doğrulamayı nasıl yapılandıracağınızı öğrenmek için [Uygulama Hizmetini Microsoft kimlik platformuyla tümleştir'e](/azure/app-service/configure-authentication-provider-aad) bakın.
