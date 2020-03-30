---
title: Neden Microsoft kimlik platformu (v2.0) güncelleyin | Azure
description: Microsoft kimlik platformu (v2.0) bitiş noktası ile Azure Etkin Dizin (Azure AD) v1.0 bitiş noktası arasındaki farkları öğrenin ve v2.0'a güncelleştirmenin avantajlarını öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: ryanwi
ms.reviewer: saeeda, hirsin, jmprieur, sureshja, jesakowi, lenalepa, kkrishna, negoe
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 67a54a2cd4fa071fd47bcebb9aa53fd11fefd61e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154925"
---
# <a name="why-update-to-microsoft-identity-platform-v20"></a>Neden Microsoft kimlik platformuna (v2.0) güncelleştirmelisiniz?

Yeni bir uygulama geliştirirken, Microsoft kimlik platformu (v2.0) ve Azure Active Directory (v1.0) bitiş noktaları arasındaki farkları bilmek önemlidir. Bu makalede, bitiş noktaları ve Microsoft kimlik platformu için bazı varolan sınırlamalar arasındaki temel farkları kapsar.

> [!NOTE]
> Microsoft kimlik platformu bitiş noktası tüm Azure REKLAM senaryolarını ve özelliklerini desteklemez. Microsoft kimlik platformu bitiş noktasını kullanmanız gerekip gerekip gerekmeden karar vermek için [Microsoft kimlik platformu sınırlamaları](#limitations)hakkında bilgi edinin.

## <a name="who-can-sign-in"></a>Kimler oturum açabilir

![V1.0 ve v2.0 uç noktaları ile kimler oturum açabilir?](media/azure-ad-endpoint-comparison/who-can-signin.svg)

* v1.0 bitiş noktası, yalnızca iş ve okul hesaplarının uygulamanızda oturum açmasına olanak tanır (Azure AD)
* Microsoft kimlik platformu bitiş noktası, Azure AD'deki iş ve okul hesaplarının ve hotmail.com, outlook.com ve msn.com gibi kişisel Microsoft hesaplarındaki (MSA) oturum açma olanağı sağlar.
* Her iki uç nokta da *[tek kiracı](../develop/single-and-multi-tenant-apps.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)* olarak yapılandırılan uygulamalar için veya kiracıya özgü bitiş noktasına işaret etmek üzere yapılandırılan *çok kiracılı* uygulamalar için Bir Azure REKLAM dizininin *[konuk kullanıcılarının](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* oturum açmalarını kabul eder (`https://login.microsoftonline.com/{TenantId_or_Name}`

Microsoft kimlik platformu bitiş noktası, kişisel Microsoft hesaplarından oturum açma kabul eden uygulamalar, iş ve okul hesapları yazmanıza olanak tanır. Bu, uygulamanızı tamamen hesap-agnostik yazma olanağı sağlar. Örneğin, uygulamanız Microsoft [Graph'ı](https://graph.microsoft.io)adlandırıyorsa, SharePoint siteleri veya dizin verileri gibi çalışma hesapları için bazı ek işlevler ve veriler kullanılabilir. Ancak, [bir kullanıcının postasını okumak](https://docs.microsoft.com/graph/api/user-list-messages?view=graph-rest-1.0)gibi birçok eylem için, aynı kod hem kişisel hem de iş ve okul hesapları için e-postaya erişebilir.

Microsoft kimlik platformu bitiş noktası için, tüketici, eğitim ve kurumsal dünyalara erişmek için Microsoft Kimlik Doğrulama Kitaplığını (MSAL) kullanabilirsiniz. Azure AD v1.0 bitiş noktası yalnızca iş ve okul hesaplarından oturum açma kabul eder.

## <a name="incremental-and-dynamic-consent"></a>Artımlı ve dinamik onay

Azure AD v1.0 bitiş noktasını kullanan uygulamaların gerekli OAuth 2.0 izinlerini önceden belirtmeleri gerekir, örneğin:

![İzinler Kayıt UI'sini gösteren örnek](./media/azure-ad-endpoint-comparison/app-reg-permissions.png)

Doğrudan uygulama kaydına ayarlanan izinler **statiktir.** Azure portalında tanımlanan uygulamanın statik izinleri kodu güzel ve basit tutarken, geliştiriciler için bazı olası sorunlar sunar:

* Uygulamanın, kullanıcının ilk oturum açma sında ihtiyaç distemi gereken tüm izinleri istemesi gerekir. Bu, son kullanıcıların uygulamanın ilk oturum açma da erişimini onaylamasını engelleyen uzun bir izin listesine yol açabilir.

* Uygulamanın erişeceği tüm kaynakları önceden bilmesi gerekir. Rasgele sayıda kaynağa erişebilen uygulamalar oluşturmak zordu.

Microsoft kimlik platformu bitiş noktası ile, Azure portalındaki uygulama kayıt bilgilerinde tanımlanan statik izinleri yoksayabilir ve bunun yerine izinleri aşamalı olarak isteyebilirsiniz, bu da önceden açık minimum izin ler kümesi istemek anlamına gelir ve müşteri ek uygulama özellikleri kullandığından zaman içinde daha da büyür. Bunu yapmak için, uygulama kayıt bilgilerinde önceden tanımlamaya gerek kalmadan, `scope` erişim jetonu talep ederken yeni kapsamları parametreye ekleyerek uygulamanızın ihtiyaç duyduğu kapsamları istediğiniz zaman belirtebilirsiniz. Kullanıcı isteğe eklenen yeni kapsamları henüz kabul etmediyse, yalnızca yeni izinleri kabul etmeleri istenir. Daha fazla bilgi edinmek için [izinlere, onaya ve kapsamlara](../develop/v2-permissions-and-consent.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)bakın.

Bir uygulamanın `scope` parametre aracılığıyla dinamik olarak izin istemesine izin vermek, geliştiricilere kullanıcınızın deneyimi üzerinde tam kontrol sağlar. Ayrıca, onay deneyiminizi önceden yükleyebilir ve tüm izinleri tek bir ilk yetkilendirme isteğinde isteyebilirsiniz. Uygulamanız çok sayıda izin gerektiriyorsa, bu izinleri zaman içinde uygulamanın belirli özelliklerini kullanmaya çalışırken kullanıcıdan kademeli olarak toplayabilirsiniz.

Bir kuruluş adına yapılan yönetici onayı hala uygulama için kayıtlı statik izinleri gerektirir, bu nedenle tüm kuruluş adına onay vermek için bir yöneticiye ihtiyacınız varsa, uygulama kayıt portalındaki uygulamalar için bu izinleri ayarlamanız gerekir. Bu, uygulamayı ayarlamak için kuruluş yöneticisi tarafından gereken döngüleri azaltır.

## <a name="scopes-not-resources"></a>Kapsamlar, kaynaklar değil

v1.0 bitiş noktasını kullanan uygulamalariçin, bir uygulama **kaynak**veya belirteç alıcısı olarak görülebilir. Kaynak, istemci uygulamalarının belirli bir kapsam kümesi için bu kaynaktan belirteçler istemesine izin vererek, anladığı bir dizi **kapsam** veya **oAuth2Permissions** tanımlayabilir. Microsoft Graph API'yi bir kaynağa örnek olarak düşünün:

* Kaynak tanımlayıcısı veya: `AppID URI``https://graph.microsoft.com/`
* Kapsamlar, `oAuth2Permissions`ya `Directory.Read` `Directory.Write`da : , , ve benzeri.

Bu, Microsoft kimlik platformu bitiş noktası için geçerlidir. Bir uygulama yine de kaynak olarak görülebilir, kapsamları tanımlayabilir ve bir URI tarafından tanımlanabilir. İstemci uygulamaları yine de bu kapsamlara erişim isteyebilir. Ancak, istemcinin bu izinleri talep etme şekli değişti.

v1.0 bitiş noktası için, Azure AD'ye bir OAuth 2.0 yetkilendirme isteği aşağıdaki gibi görünebilir:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.microsoft.com/
...
```

Burada, **kaynak** parametresi istemci uygulamasının yetkilendirme istediği kaynağı belirtir. Azure AD, Uygulamanın gerektirdiği izinleri Azure portalındaki statik yapılandırmayı temel alarak hesapladı ve buna göre belirteçler yayınladı.

Microsoft kimlik platformu bitiş noktasını kullanan uygulamalarda, aynı OAuth 2.0 yetkilendirme isteği aşağıdaki gibi görünür:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.microsoft.com/directory.read%20https://graph.microsoft.com/directory.write
...
```

Burada, **kapsam** parametresi uygulamanın yetkilendirme istediği kaynağı ve izinleri gösterir. İstenilen kaynak istekte hala mevcuttur - kapsam parametresinin her bir değeri ne kapsaedilir. Kapsam parametresini bu şekilde kullanmak, Microsoft kimlik platformu bitiş noktasının OAuth 2.0 belirtimiyle daha uyumlu olmasını sağlar ve yaygın endüstri uygulamalarıyla daha yakından uyumludur. Ayrıca, uygulamaların [artımlı izin](#incremental-and-dynamic-consent) ler yapmasına da olanak sağlar - yalnızca uygulama ön izinlerin aksine izinleri istediğinde.

## <a name="well-known-scopes"></a>Tanınmış kapsamlar

### <a name="offline-access"></a>Çevrimdışı erişim

Microsoft kimlik platformu bitiş noktasını kullanan uygulamalar, `offline_access` uygulamalar için yeni bir tanınmış izin (kapsam) kullanımını gerektirebilir. Tüm uygulamaların, kullanıcı uygulamayı etkin olarak kullanmasa bile, bir kullanıcı adına kaynaklara uzun süre erişmesi gerekiyorsa bu izni istemesi gerekir. Kapsam, `offline_access` onay iletişim kutularında kullanıcıya, kullanıcının kabul etmesi gereken **verilerinize her zaman erişirken**görünür. `offline_access` İzin istemek, web uygulamanızın Microsoft kimlik platformu bitiş noktasından OAuth 2.0 refresh_tokens almasını sağlar. Yenileme belirteçleri uzun ömürlüdür ve uzun süreli erişim süreleri için yeni OAuth 2.0 erişim belirteçleri ile değiştirilebilir.

Uygulamanız `offline_access` kapsamı talep etmiyorsa, yenilenme belirteçleri almaz. Bu, OAuth 2.0 yetkilendirme kodu akışında bir yetkilendirme kodu kullandığınızda, yalnızca `/token` bitiş noktasından bir erişim belirteci geri alacağınız anlamına gelir. Bu erişim belirteci kısa bir süre (genellikle bir saat) için geçerli kalır, ancak sonunda sona erer. Bu noktada, uygulamanızın yeni bir yetkilendirme kodu almak `/authorize` için kullanıcıyı bitiş noktasına yönlendirmesi gerekir. Bu yönlendirme sırasında, uygulamanın türüne bağlı olarak kullanıcının kimlik bilgilerini yeniden girmesi veya izinlere yeniden onay lanması gerekebilir veya olmayabilir.

OAuth 2.0 `refresh_tokens`hakkında daha fazla `access_tokens`bilgi edinmek için, ve , [Microsoft kimlik platformu protokol başvurusu](../develop/active-directory-v2-protocols.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)göz atın.

### <a name="openid-profile-and-email"></a>OpenID, profil ve e-posta

Tarihsel olarak, Microsoft kimlik platformu ile en temel OpenID Connect oturum açma akışı, ortaya çıkan *id_token*kullanıcı hakkında birçok bilgi sağlayacaktır. Bir id_token talepler, kullanıcının adını, tercih edilen kullanıcı adını, e-posta adresini, nesne kimliğini ve daha fazlasını içerebilir.

`openid` Kapsamın uygulamanıza erişimini sağladığı bilgiler artık kısıtlanmıştır. Kapsam `openid` yalnızca uygulamanızın kullanıcıda oturum açmasına ve kullanıcı için uygulamaya özgü bir tanımlayıcı almasına izin verir. Uygulamanızda kullanıcı hakkında kişisel veri almak istiyorsanız, uygulamanızın kullanıcıdan ek izinler istemesi gerekir. İki yeni kapsam `email` `profile`ve , ek izinler istemenize olanak sağlar.

* Kapsam, `email` uygulamanızın adreslenebilir bir e-posta adresi `email` olduğunu varsayarak, id_token'daki talep yoluyla uygulamanızın birincil e-posta adresine erişmesine olanak tanır.
* Kapsam, `profile` uygulamanızın kullanıcı yla ilgili adı, tercih edilen kullanıcı adı, nesne kimliği ve benzeri diğer tüm temel bilgilere id_token erişimini sağlar.

Bu kapsamlar, uygulamanızı en az açıklamalı bir şekilde kodlamanızı sağlar, böylece kullanıcıdan yalnızca uygulamanızın işini yapması için gereken bilgi kümesini isteyebilirsiniz. Bu kapsamlar hakkında daha fazla bilgi için [Microsoft kimlik platformu kapsam başvurusuna](../develop/v2-permissions-and-consent.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)bakın.

## <a name="token-claims"></a>Belirteç talepleri

Microsoft kimlik platformu bitiş noktası, yükleri küçük tutmak için varsayılan olarak belirteçlerinde daha küçük bir talep kümesi yayınlar. Bir Microsoft kimlik platformu belirtecinde varsayılan olarak artık sağlanmayan bir v1.0 belirtecinde belirli bir iddiaya bağımlı olan uygulamalarınız ve hizmetleriniz varsa, bu talebi eklemek için [isteğe bağlı talepler](../develop/active-directory-optional-claims.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) özelliğini kullanmayı düşünün.

> [!IMPORTANT]
> v1.0 ve v2.0 belirteçleri hem v1.0 hem de v2.0 uç noktaları ile verilebilir! id_tokens *her zaman* istedikleri bitiş noktasıyla eşleşir ve erişim belirteçleri her *zaman* istemcinizin bu belirteci kullanarak çağıracağı Web API'sı tarafından beklenen biçimle eşleşir.  Bu nedenle, uygulamanız v1.0 formatında erişim belirteçleri bekleyen Microsoft Graph'ı aramak için bir belirteç almak için v2.0 bitiş noktasını kullanıyorsa, uygulamanız v1.0 biçiminde bir jeton alır.  

## <a name="limitations"></a>Sınırlamalar

Microsoft kimlik platformlarını kullanırken dikkat edilmesi gereken birkaç kısıtlama vardır.

Microsoft kimlik platformuyla tümlebilen uygulamalar oluştururken, Microsoft kimlik platformu bitiş noktası ve kimlik doğrulama protokollerinin gereksinimlerinizi karşılayıp karşılamadığına karar vermeniz gerekir. V1.0 bitiş noktası ve platformu hala tam olarak desteklenir ve bazı açılardan Microsoft kimlik platformundan daha zengin bir özelliktir. Ancak, Microsoft kimlik platformu geliştiriciler için [önemli avantajlar sunar.](azure-ad-endpoint-comparison.md)

Şimdi geliştiriciler için basitleştirilmiş bir öneri aşağıda vereyim:

* Uygulamanızda kişisel Microsoft hesaplarını desteklemek veya desteklemek istiyorsanız veya yeni bir uygulama yazıyorsanız, Microsoft kimlik platformunu kullanın. Ancak bunu yapmadan önce, bu makalede tartışılan sınırlamaları anladığınızdan emin olun.
* SAML'ye dayanan bir uygulamayı geçirip güncelliyorsanız, Microsoft kimlik platformlarını kullanamazsınız. Bunun yerine, [Azure AD v1.0 kılavuzuna](v1-overview.md)bakın.

Microsoft kimlik platformu bitiş noktası burada listelenen kısıtlamaları ortadan kaldırmak için gelişecektir, böylece yalnızca Microsoft kimlik platformu bitiş noktasını kullanmanız gerekir. Bu arada, Microsoft kimlik platformu bitiş noktasının sizin için doğru olup olmadığını belirlemek için bu makaleyi kullanın. Bu makaleyi Microsoft kimlik platformu bitiş noktasının geçerli durumunu yansıtacak şekilde güncelleştirmeye devam edeceğiz. Gereksinimlerinizi Microsoft kimlik platformu özelliklerine göre yeniden değerlendirmek için geri kontrol edin.

### <a name="restrictions-on-app-registrations"></a>Uygulama kayıtlarıyla ilgili kısıtlamalar

Microsoft kimlik platformu bitiş noktasıyla tümleştirmek istediğiniz her uygulama için, Azure portalındaki yeni [ **Uygulama kayıtları** deneyiminde](https://aka.ms/appregistrations) bir uygulama kaydı oluşturabilirsiniz. Varolan Microsoft hesap uygulamaları portalla uyumlu değildir, ancak nerede veya ne zaman kaydedildiklerine bakılmaksızın tüm Azure AD uygulamaları dır.

İş ve okul hesaplarını ve kişisel hesapları destekleyen uygulama kayıtları aşağıdaki uyarılara sahiptir:

* Uygulama kimliği başına yalnızca iki uygulama sırrına izin verilir.
* Kiracıya kaydedilmemiş bir uygulama yalnızca onu kaydeden hesap tarafından yönetilebilir. Diğer geliştiricilerle paylaşılamaz. Bu durum, Uygulama Kayıt Portalı'ndaki kişisel bir Microsoft hesabı kullanılarak kaydolan çoğu uygulama için geçerlidir. Uygulama kaydınızı birden çok geliştiriciyle paylaşmak istiyorsanız, Azure portalının yeni **Uygulama kayıtları** bölümünü kullanarak uygulamayı kiracıya kaydedin.
* Yeniden yönlendirme URL'sinin biçimiüzerinde izin verilen çeşitli kısıtlamalar vardır. URL'yi yeniden yönlendirme hakkında daha fazla bilgi için bir sonraki bölüme bakın.

### <a name="restrictions-on-redirect-urls"></a>Yönlendirme URL'lerine ilişkin kısıtlamalar

Microsoft kimlik platformu için kayıtlı olan uygulamalar sınırlı bir yeniden yönlendirme URL değeri kümesiyle sınırlıdır. Web uygulamaları ve hizmetleri için yönlendirme URL'si şema `https`ile başlamalı ve tüm yönlendirme URL değerleri tek bir DNS etki alanını paylaşmalıdır.  Kayıt sistemi, varolan yönlendirme URL'sinin tüm DNS adını eklediğiniz yeniden yönlendirme URL'sinin DNS adı ile karşılaştırır. `http://localhost`yönlendirme URL'si olarak da desteklenir.  

Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:  

* Yeni yönlendirme URL'sinin tüm DNS adı, varolan yeniden yönlendirme URL'sinin DNS adıile eşleşmez.
* Yeni yönlendirme URL'sinin tüm DNS adı, varolan yeniden yönlendirme URL'sinin bir alt etki alanı değildir.

#### <a name="example-1"></a>Örnek 1

Uygulamanın yönlendirme URL'si `https://login.contoso.com`varsa, Aşağıdaki örnekte gösterildiği gibi DNS adının tam olarak eşleştiği bir yönlendirme URL'si ekleyebilirsiniz:

`https://login.contoso.com/new`

Veya, aşağıdaki örnekte gösterildiği gibi, login.contoso.com bir DNS alt etki alanına başvurabilirsiniz:

`https://new.login.contoso.com`

#### <a name="example-2"></a>Örnek 2

URL'leri yeniden yönlendiren `login-east.contoso.com` bir `login-west.contoso.com` uygulamaya sahip olmak istiyorsanız, url'leri yeniden yönlendirenleri aşağıdaki sırayla eklemeniz gerekir:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

İlk yeniden yönlendirme URL'sinin alt etki alanları olduğu için ikinci ikisini ekleyebilirsiniz, contoso.com.

Belirli bir uygulama için yalnızca 20 yanıt URL'niz olabilir - bu sınır, kaydın desteklediği tüm uygulama türleri (tek sayfalı uygulama (SPA), yerel istemci, web uygulaması ve hizmet) için geçerlidir.  

Bir uygulamayı Microsoft kimlik platformunda kullanmak üzere nasıl kaydedebilirsiniz öğrenmek için, [yeni Uygulama kayıtları deneyimini kullanarak bir uygulamayı kaydedin'e](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)bakın.

### <a name="restrictions-on-libraries-and-sdks"></a>Kütüphaneler ve SDK'larla ilgili kısıtlamalar

Şu anda, Microsoft kimlik platformu bitiş noktası için kitaplık desteği sınırlıdır. Bir üretim uygulamasında Microsoft kimlik platformu bitiş noktasını kullanmak istiyorsanız, şu seçeneklere sahipsiniz:

* Bir web uygulaması oluşturuyorsanız, oturum açma ve belirteç doğrulaması yapmak için genel olarak kullanılabilen sunucu tarafındaki ara yazılımı güvenle kullanabilirsiniz. Bunlar arasında ASP.NET için OWIN OpenID Connect aracı ve Node.js Passport eklentisi yer almaktadır. Microsoft ara yazılımını kullanan kod örnekleri için Microsoft [kimlik platformunun başlangıç](../develop/v2-overview.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json#getting-started) bölümüne bakın.
* Bir masaüstü veya mobil uygulama oluşturuyorsanız, Microsoft Kimlik Doğrulama Kitaplıklarından birini (MSAL) kullanabilirsiniz. Bu kitaplıklar genellikle kullanılabilir veya üretim destekli önizlemede olduğundan, bunları üretim uygulamalarında kullanmak güvenlidir. Önizleme koşulları ve kullanılabilir kitaplıklar hakkında daha fazla bilgi için [kimlik doğrulama kitaplıkları referans.](../develop/reference-v2-libraries.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)
* Microsoft kitaplıkları tarafından kapsanmayan platformlar için, uygulama kodunuza doğrudan protokol iletileri gönderip alarak Microsoft kimlik platformu bitiş noktasıyla tümleştirebilirsiniz. OpenID Connect ve OAuth protokolleri, böyle bir tümleştirme yapmanıza yardımcı olmak için [açıkça belgelenmiştir.](../develop/active-directory-v2-protocols.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)
* Son olarak, Microsoft kimlik platformu bitiş noktasıyla tümleştirmek için açık kaynak openid bağlantısı ve OAuth kitaplıklarını kullanabilirsiniz. Microsoft kimlik platformu bitiş noktası, değişiklik yapmadan birçok açık kaynak iletişim kuralı kitaplıklarıyla uyumlu olmalıdır. Bu tür kütüphanelerin kullanılabilirliği dile ve platforma göre değişir. [OpenID Connect](https://openid.net/connect/) ve [OAuth 2.0](https://oauth.net/2/) web siteleri popüler uygulamaların bir listesini tutar. Daha fazla bilgi için [Microsoft kimlik platformu ve kimlik doğrulama kitaplıkları](../develop/reference-v2-libraries.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)ve açık kaynak istemci kitaplıklarının ve Microsoft kimlik platformu bitiş noktasıyla sınanmış örneklerin listesine bakın.
* Başvuru için, `.well-known` Microsoft kimlik platformu ortak bitiş `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`noktası için bitiş noktasıdır. Kiracınıza özel veriler almak için kiracı kimliğinizle değiştirin. `common`  

### <a name="protocol-changes"></a>Protokol değişiklikleri

Microsoft kimlik platformu bitiş noktası SAML veya WS-Federation'ı desteklemez; yalnızca OpenID Connect ve OAuth 2.0'ı destekler.  V1.0 bitiş noktasından OAuth 2.0 protokollerinde yapılan önemli değişiklikler şunlardır: 

* İsteğe bağlı bir talep yapılandırılırsa `email` **veya** istekte kapsam=e-posta belirtilmişse talep iade edilir. 
* Parametre `scope` artık `resource` parametre yerine desteklenir.  
* Birçok yanıt, örneğin OAuth 2.0 belirtimiile daha uyumlu hale getirmek `expires_in` için değiştirildi, örneğin, bir dize yerine doğru bir int olarak döndürülme.  

Microsoft kimlik platformu bitiş noktasında desteklenen protokol işlevselliğinin kapsamını daha iyi anlamak için [OpenID Connect ve OAuth 2.0 protokol başvurusuna](../develop/active-directory-v2-protocols.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)bakın.

#### <a name="saml-restrictions"></a>SAML kısıtlamaları

Windows uygulamalarında Active Directory Authentication Library'yi (ADAL) kullandıysanız, Güvenlik Öneçİşaretdili Dili (SAML) öne çıkarma hibesini kullanan Windows Tümleşik kimlik doğrulamasını kullanmış olabilirsiniz. Bu hibe ile, federe Azure AD kiracılarının kullanıcıları, kimlik bilgilerini girmeden şirket içi Active Directory örnekleriyle sessizce kimlik doğrulaması yapabilir. SAML bağımsız kabul hibesi Microsoft kimlik platformu bitiş noktasında desteklenmez.
