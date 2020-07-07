---
title: Neden Microsoft Identity platform (v 2.0) güncelleştirmesi | Mavisi
description: Microsoft Identity platform (v 2.0) uç noktası ile Azure Active Directory (Azure AD) v 1.0 uç noktası arasındaki farkları öğrenin ve v 2.0 sürümüne güncelleştirme avantajları hakkında bilgi edinin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80154925"
---
# <a name="why-update-to-microsoft-identity-platform-v20"></a>Neden Microsoft kimlik platformuna (v2.0) güncelleştirmelisiniz?

Yeni bir uygulama geliştirirken, Microsoft Identity platform (v 2.0) ve Azure Active Directory (v 1.0) uç noktaları arasındaki farkları bilmemiz önemlidir. Bu makalede, uç noktalar ile Microsoft Identity platformu için bazı mevcut sınırlamalar arasındaki temel farklılıklar ele alınmaktadır.

> [!NOTE]
> Microsoft Identity platform uç noktası tüm Azure AD senaryolarını ve özelliklerini desteklemez. Microsoft Identity platform uç noktasını kullanmanız gerekip gerekmediğini öğrenmek için [Microsoft Identity platform sınırlamaları](#limitations)hakkında bilgi edinin.

## <a name="who-can-sign-in"></a>Kimler oturum açabilir

![V 1.0 ve v 2.0 uç noktaları ile oturum açabilir](media/azure-ad-endpoint-comparison/who-can-signin.svg)

* V 1.0 uç noktası yalnızca iş ve okul hesaplarının uygulamanızda oturum açmasını sağlar (Azure AD)
* Microsoft Identity platform uç noktası, Azure AD 'den ve hotmail.com, outlook.com ve msn.com gibi kişisel Microsoft hesaplarından (MSA) iş ve okul hesaplarının oturum açmasını sağlar.
* Her iki uç nokta de *[tek kiracılı](../develop/single-and-multi-tenant-apps.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)* olarak yapılandırılmış uygulamalar veya kiracıya özgü uç noktayı () işaret edecek şekilde yapılandırılmış *çok kiracılı* uygulamalar için Azure AD dizininin *[Konuk kullanıcılarının](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* oturum açma işlemlerini kabul eder `https://login.microsoftonline.com/{TenantId_or_Name}` .

Microsoft Identity platform uç noktası, kişisel Microsoft hesaplarından ve iş ve okul hesaplarından oturum açma işlemlerini kabul eden uygulamalar yazmanıza izin verir. Bu, uygulamanızı tamamen hesap belirsiz şekilde yazmanıza olanak sağlar. Örneğin, uygulamanız [Microsoft Graph](https://graph.microsoft.io)çağırırsa, bazı ek işlevler ve veriler, SharePoint siteleri veya dizin verileri gibi iş hesapları için kullanılabilir. Ancak, [bir kullanıcının postasını okumak](https://docs.microsoft.com/graph/api/user-list-messages?view=graph-rest-1.0)gibi birçok eylem için aynı kod e-postaya hem kişisel hem de iş ve okul hesapları için erişebilir.

Microsoft Identity platform uç noktası için, Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanarak tüketici, eğitim ve kurumsal işletim LDS 'ye erişim elde edebilirsiniz. Azure AD v 1.0 uç noktası yalnızca iş ve okul hesaplarından oturum açma işlemlerini kabul eder.

## <a name="incremental-and-dynamic-consent"></a>Artımlı ve dinamik onay

Azure AD v 1.0 uç noktasını kullanan uygulamalar, gerekli OAuth 2,0 izinlerinin önceden belirtilmesi için gereklidir, örneğin:

![Izin kayıt Kullanıcı arabirimini gösteren örnek](./media/azure-ad-endpoint-comparison/app-reg-permissions.png)

Uygulama kaydında doğrudan ayarlanan izinler **statiktir**. Azure portal tanımlanmış uygulamanın statik izinleri, kodu iyi ve basit durumda tutarak, geliştiriciler için bazı olası sorunları gösterir:

* Uygulamanın, kullanıcının ilk oturum açması durumunda ihtiyacı olan tüm izinleri istemesi gerekir. Bu, son kullanıcıların ilk oturum açma sırasında uygulamanın erişimini onaylayamasından etkilenmeden uzun bir izin listesine yol açabilir.

* Uygulamanın, bir süre önce erişeceğimizi tüm kaynakları bilmeleri gerekir. Rastgele sayıda kaynağa erişebilen uygulamalar oluşturmak zordur.

Microsoft Identity platform uç noktası ile Azure portal uygulama kayıt bilgilerinde tanımlanan statik izinleri yoksayabilir ve izinleri artımlı olarak isteyebilirsiniz; bu da, müşterinin ek uygulama özellikleri kullandığı için en az bir izin kümesi ve zaman içinde daha fazla büyümenin daha fazla olması anlamına gelir. Bunu yapmak için, `scope` uygulama kayıt bilgilerinde önceden tanımlamaya gerek kalmadan, bir erişim belirteci istenirken, uygulamanıza gereken yeni kapsamları dahil ederek, uygulamanızın ihtiyaç duyduğu kapsamları belirtebilirsiniz. Kullanıcı henüz isteğe eklenen yeni kapsamları kabul etmediyse, yalnızca yeni izinleri onaylaması istenir. Daha fazla bilgi için bkz. [izinler, onay ve kapsamlar](../develop/v2-permissions-and-consent.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

Bir uygulamanın parametre üzerinden dinamik olarak izin istemesine izin vermek, `scope` geliştiricilerin kullanıcı deneyimi üzerinde tam denetim sahibi olmasını sağlar. Ayrıca, izin deneyiminizi ön yükleyebilir ve tek bir ilk yetkilendirme isteğindeki tüm izinleri isteyebilirsiniz. Uygulamanız çok sayıda izin gerektiriyorsa, zaman içinde uygulamanın belirli özelliklerini kullanmaya çalıştıkları için bu izinleri Kullanıcı tarafından artımlı olarak toplayabilirsiniz.

Bir kuruluş adına yapılan yönetici onayı, yine de uygulama için kayıtlı statik izinler gerektirir, bu nedenle uygulamanın tamamı adına bir yöneticinin izin vermesini istiyorsanız uygulama kayıt portalındaki uygulamalar için bu izinleri ayarlamanız gerekir. Bu, kuruluş yöneticisinin uygulamayı ayarlaması için gereken döngüleri azaltır.

## <a name="scopes-not-resources"></a>Kapsam, kaynak değil

V 1.0 uç noktasını kullanan uygulamalar için, bir uygulama **kaynak**veya belirteç alıcısı olarak davranabilir. Bir kaynak, anladığı sayıda **kapsam** veya **oAuth2Permissions** tanımlayabilir ve bu da istemci uygulamaların belirli bir kapsam kümesi için bu kaynaktan belirteç istemesine izin verir. Microsoft Graph API 'sini bir kaynak örneği olarak düşünün:

* Kaynak tanımlayıcısı veya `AppID URI` :`https://graph.microsoft.com/`
* Kapsamlar veya `oAuth2Permissions` : `Directory.Read` , vb `Directory.Write` .

Bu, Microsoft Identity platform uç noktası için doğru bir durum içerir. Bir uygulama yine de kaynak olarak davranabilir, kapsamları tanımlayabilir ve bir URI ile tanımlanabilir. İstemci uygulamaları yine de bu kapsamlara erişim isteğinde bulunabilir. Ancak, bir istemcinin bu izinleri istemesi yöntemi değişmiştir.

V 1.0 uç noktası için, Azure AD 'ye yönelik bir OAuth 2,0 yetkilendirme isteği şöyle olabilir:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.microsoft.com/
...
```

Burada, **kaynak** parametresi, istemci uygulamanın yetkilendirme istediğini kaynağı belirtti. Azure AD, Azure portal statik yapılandırmaya dayalı olarak uygulama için gereken izinleri ve verilen belirteçleri uygun şekilde hesapladık.

Microsoft Identity platform uç noktasını kullanan uygulamalar için, aynı OAuth 2,0 yetkilendirme isteği şöyle görünür:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.microsoft.com/directory.read%20https://graph.microsoft.com/directory.write
...
```

Burada **kapsam** parametresi, uygulamanın hangi kaynak ve izinlerin yetkilendirme istediğini gösterir. İstenen kaynak, istek içinde hala mevcut. kapsam parametresinin her bir değerinin her birinde çevrelenmiş olur. Kapsam parametresinin bu şekilde kullanılması, Microsoft Identity platform uç noktasının OAuth 2,0 belirtimiyle daha uyumlu olmasını sağlar ve ortak sektör uygulamalarıyla daha yakından hizalanır. Ayrıca, uygulamaların [artımlı olarak izin](#incremental-and-dynamic-consent) vermesini sağlar-yalnızca uygulama, ön uca izin gerektirdiğinde izinleri ister.

## <a name="well-known-scopes"></a>İyi bilinen kapsamlar

### <a name="offline-access"></a>Çevrimdışı erişim

Microsoft Identity platform uç noktasını kullanan uygulamalar, uygulamalar için yeni bir iyi bilinen izin kullanımını gerektirebilir `offline_access` . Kullanıcı uygulamayı etkin bir şekilde kullanmıyor olsa bile, bir kullanıcı adına, kaynaklara erişmesi gerekiyorsa tüm uygulamaların bu izni istemesi gerekir. Kapsam, kullanıcının `offline_access` kabul etmesi gereken **her zaman verilerinize erişmek**için izin iletişim kutularında kullanıcıya görünür. İzin istemek `offline_access` Web uygulamanızın Microsoft Identity platform uç noktasından OAuth 2,0 refresh_tokens almasını olanaklı hale kullanacaktır. Belirteçleri yenileme, uzun süreli ve genişletilmiş erişim dönemlerinde yeni OAuth 2,0 erişim belirteçleri için değiş tokuş edilebilir.

Uygulamanız kapsam isteğinde yoksa `offline_access` , yenileme belirteçleri almaz. Bu, OAuth 2,0 yetkilendirme kodu akışında bir yetkilendirme kodu kullandığınızda yalnızca uç noktadan bir erişim belirteci geri alacağınız anlamına gelir `/token` . Bu erişim belirteci kısa bir süre (genellikle bir saat) için geçerli kalır, ancak sonunda süresi dolacak. Bu noktada, uygulamanızın `/authorize` Yeni bir yetkilendirme kodu almak için kullanıcıyı uç noktaya yeniden yönlendirmeniz gerekir. Bu yeniden yönlendirme sırasında, Kullanıcı, uygulama türüne bağlı olarak kimlik bilgilerini yeniden veya reconsent izin vermek zorunda kalabilir.

OAuth 2,0, ve hakkında daha fazla bilgi edinmek için `refresh_tokens` `access_tokens` [Microsoft Identity platform Protokolü başvurusunu](../develop/active-directory-v2-protocols.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)inceleyin.

### <a name="openid-profile-and-email"></a>OpenID, profile ve email

Geçmişte, Microsoft Identity platform ile en temel OpenID Connect oturum açma akışı, sonuçta elde edilen *id_token*Kullanıcı hakkında çok fazla bilgi sağlayacaktır. Bir id_token talepler kullanıcının adını, tercih edilen Kullanıcı adını, e-posta adresini, nesne KIMLIĞINI ve daha fazlasını içerebilir.

`openid`Kapsamın uygulamanızın erişimi için kullandığı bilgiler artık kısıtlıdır. `openid`Kapsam yalnızca uygulamanızın kullanıcıya oturum açmasını ve Kullanıcı için uygulamaya özel bir tanımlayıcı almasını sağlar. Uygulamanızdaki Kullanıcı hakkında kişisel veriler almak istiyorsanız, uygulamanızın kullanıcıdan ek izinler istemesi gerekir. İki yeni kapsam `email` ve `profile` , ek izin isteme izni sağlayacaktır.

* `email`Kapsam, `email` kullanıcının adreslenebilir bir e-posta adresi olduğu varsayıldığında, id_token talep aracılığıyla uygulamanızın kullanıcının birincil e-posta adresine erişmesini sağlar.
* `profile`Kapsam, uygulamanızın adı, tercih edilen Kullanıcı adı, nesne kimliği, vb. gibi diğer tüm temel bilgilere erişim için, id_token.

Bu kapsamlar, uygulamanızı en düşük düzeyde bir şekilde kodlarabilmeniz için, kullanıcıdan yalnızca uygulamanızın işini yapması için ihtiyaç duyacağı bilgi kümesini sormasını sağlayabilirsiniz. Bu kapsamlar hakkında daha fazla bilgi için bkz. [Microsoft Identity platform kapsam başvurusu](../develop/v2-permissions-and-consent.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

## <a name="token-claims"></a>Belirteç talepleri

Microsoft Identity platform uç noktası, yüklerini küçük tutmak için varsayılan olarak belirteçlerinde daha küçük bir talepler kümesi yayınlar. Bir Microsoft Identity platform belirtecinde artık varsayılan olarak sağlanmayan bir v 1.0 belirtecindeki belirli bir talebe bağımlılığı olan uygulama ve hizmetlerinize sahipseniz, bu talebi dahil etmek için [isteğe bağlı talepler](../develop/active-directory-optional-claims.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) özelliğini kullanmayı göz önünde bulundurun.

> [!IMPORTANT]
> v 1.0 ve v 2.0 belirteçleri hem v 1.0 hem de v 2.0 uç noktaları tarafından verilebilir! id_tokens, istedikleri bitiş noktasıyla *her zaman* *eşleşir ve erişim belirteçleri,* istemcinizin bu belirteci kullanarak çağırabilecekleri Web API 'si tarafından beklenen biçimle eşleşir.  Uygulama, v 1.0 biçim erişim belirteçlerini bekleyen Microsoft Graph çağrısı yapmak için v 2.0 uç noktasını kullanıyorsa, uygulamanız v 1.0 biçiminde bir belirteç alır.  

## <a name="limitations"></a>Sınırlamalar

Microsoft Identity platform kullanırken dikkat etmeniz için bazı kısıtlamalar vardır.

Microsoft Identity platformu ile tümleştirilen uygulamalar yapılandırdığınızda, Microsoft kimlik platformu uç noktası ve kimlik doğrulama protokollerinin gereksinimlerinizi karşılayıp karşılamadığını belirlemeniz gerekir. V 1.0 uç noktası ve platformu hala tam olarak desteklenmektedir ve bazı özellikler Microsoft Identity platformundan daha zengin bir özelliktir. Ancak Microsoft Identity platform, geliştiriciler için [önemli avantajlar sunar](azure-ad-endpoint-comparison.md) .

Geliştiriciler için şimdi basitleştirilmiş bir öneri aşağıda verilmiştir:

* Uygulamanızda kişisel Microsoft hesaplarını desteklemek istiyorsanız veya yeni bir uygulama yazıyorsanız Microsoft Identity platform ' u kullanın. Ancak bunu yapmadan önce, bu makalede ele alınan sınırlamaları anladığınızdan emin olun.
* SAML kullanan bir uygulamayı geçiriyorsanız veya güncelleştiriyorsanız Microsoft Identity platform 'u kullanamazsınız. Bunun yerine, [Azure AD v 1.0 kılavuzuna](v1-overview.md)bakın.

Microsoft Identity platform uç noktası burada listelenen kısıtlamaları ortadan kaldırmak için geliştireceksiniz. böylece yalnızca Microsoft Identity platform uç noktasını kullanmanız gerekir. Bu sırada, Microsoft Identity platform uç noktasının sizin için uygun olup olmadığını öğrenmek için bu makaleyi kullanın. Microsoft Identity platform uç noktasının geçerli durumunu yansıtacak şekilde bu makaleyi güncelleştirmeye devam edeceğiz. Gereksinimlerinizi Microsoft Identity platform özelliklerine göre yeniden değerlendirmeniz için tekrar denetleyin.

### <a name="restrictions-on-app-registrations"></a>Uygulama kayıtlarında kısıtlamalar

Microsoft Identity platform uç noktasıyla tümleştirebilmek istediğiniz her uygulama için, Azure portal yeni [ **uygulama kayıtları** deneyiminde](https://aka.ms/appregistrations) bir uygulama kaydı oluşturabilirsiniz. Mevcut Microsoft hesabı uygulamalar portalla uyumlu değildir, ancak nerede veya ne zaman kaydettirildiğine bakılmaksızın tüm Azure AD uygulamaları vardır.

İş ve okul hesaplarını ve kişisel hesapları destekleyen Uygulama kayıtları aşağıdaki uyarıları vardır:

* Uygulama KIMLIĞI başına yalnızca iki uygulama gizli öğesine izin verilir.
* Bir kiracıda kayıtlı olmayan bir uygulama, yalnızca onu kaydolan hesap tarafından yönetilebilir. Diğer geliştiricilerle paylaştırılamaz. Bu, uygulama kayıt portalındaki kişisel Microsoft hesabı kullanılarak kaydedilmiş uygulamaların çoğu için bu durumdur. Uygulama kaydınızı birden çok geliştiriciyle paylaşmak isterseniz, Azure portal yeni **uygulama kayıtları** bölümünü kullanarak uygulamayı bir kiracıya kaydedin.
* Yeniden yönlendirme URL 'sinin biçiminde izin verilen çeşitli kısıtlamalar vardır. Yeniden yönlendirme URL 'SI hakkında daha fazla bilgi için sonraki bölüme bakın.

### <a name="restrictions-on-redirect-urls"></a>Yeniden yönlendirme URL 'Lerinde kısıtlamalar

Microsoft Identity platformu için kaydedilen uygulamalar, sınırlı bir yeniden yönlendirme URL değerleri kümesiyle kısıtlıdır. Web uygulamaları ve hizmetleri için yeniden yönlendirme URL 'SI şemayla başlamalıdır `https` ve tüm yeniden YÖNLENDIRME URL değerleri tek BIR DNS etki alanını paylaşmalıdır.  Kayıt sistemi, mevcut yeniden yönlendirme URL 'sinin tam DNS adını, eklediğiniz yeniden yönlendirme URL 'sinin DNS adı ile karşılaştırır. `http://localhost`yeniden yönlendirme URL 'SI olarak da desteklenir.  

Aşağıdaki koşullardan biri geçerli olduğunda DNS adı ekleme isteği başarısız olur:  

* Yeni yeniden yönlendirme URL 'sinin tam DNS adı, mevcut yeniden yönlendirme URL 'sinin DNS adı ile eşleşmiyor.
* Yeni yeniden yönlendirme URL 'sinin tam DNS adı, mevcut yeniden yönlendirme URL 'sinin bir alt etki alanı değil.

#### <a name="example-1"></a>Örnek 1

Uygulamanın yeniden yönlendirme URL 'SI varsa `https://login.contoso.com` , aşağıdaki örnekte gösterildiği gibi, DNS adının tam olarak eşleştiği bir yeniden yönlendirme URL 'si ekleyebilirsiniz:

`https://login.contoso.com/new`

Ya da, aşağıdaki örnekte gösterildiği gibi login.contoso.com DNS alt etki alanına başvurabilirsiniz:

`https://new.login.contoso.com`

#### <a name="example-2"></a>Örnek 2

Yeniden yönlendirme URL 'Leri olan ve içeren bir uygulamaya sahip olmak istiyorsanız `login-east.contoso.com` `login-west.contoso.com` , bu yeniden yönlendirme URL 'lerini aşağıdaki sırayla eklemeniz gerekir:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

İkinci iki tane ekleyebilirsiniz, çünkü ilk yeniden yönlendirme URL 'SI olan contoso.com alt etki alanları.

Belirli bir uygulama için yalnızca 20 yanıt URL 'Si olabilir. bu sınır, kaydın desteklediği tüm uygulama türleri boyunca geçerlidir (tek sayfalı uygulama (SPA), yerel istemci, Web uygulaması ve hizmet).  

Bir uygulamayı Microsoft Identity platform ile kullanmak üzere nasıl kaydedeceğinizi öğrenmek için, bkz. [yeni uygulama kayıtları deneyimini kullanarak bir uygulamayı kaydetme](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

### <a name="restrictions-on-libraries-and-sdks"></a>Kitaplıklar ve SDK 'larda kısıtlamalar

Şu anda Microsoft Identity platform uç noktası için kitaplık desteği sınırlıdır. Microsoft Identity platform uç noktasını bir üretim uygulamasında kullanmak istiyorsanız, şu seçeneklere sahip olursunuz:

* Bir Web uygulaması oluşturuyorsanız, oturum açma ve belirteç doğrulama yapmak için genel kullanıma açık sunucu tarafı ara yazılımını güvenle kullanabilirsiniz. Bunlar, ASP.NET ve Node.js Passport eklentisi için OWıN OpenID Connect ara yazılımını içerir. Microsoft ara yazılımı kullanan kod örnekleri için [Microsoft Identity platform Başlarken](../develop/v2-overview.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json#getting-started) bölümüne bakın.
* Masaüstü veya mobil uygulama oluşturuyorsanız, Microsoft kimlik doğrulama kitaplıklarından birini (MSAL) kullanabilirsiniz. Bu kitaplıklar genel olarak kullanılabilir veya üretim tarafından desteklenen bir önizlemede, bunları üretim uygulamalarında kullanmak güvenlidir. Önizleme koşulları ve [kimlik doğrulama kitaplıkları](../develop/reference-v2-libraries.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)için kullanılabilir kitaplıklar hakkında daha fazla bilgi edinebilirsiniz.
* Microsoft kitaplıkları tarafından kapsanmayan platformlar için, uygulama kodunuzda protokol iletileri doğrudan göndererek ve alarak Microsoft Identity platform uç noktasıyla tümleştirilebilir. OpenID Connect ve OAuth protokolleri, bu tür bir tümleştirme yapmanıza yardımcı olmak için [açıkça belgelenmiştir](../develop/active-directory-v2-protocols.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) .
* Son olarak, açık kaynaklı OpenID Connect ve OAuth kitaplıklarını Microsoft Identity platform uç noktasıyla tümleştirme için kullanabilirsiniz. Microsoft Identity platform uç noktası, değişiklik yapılmadan birçok açık kaynaklı protokol kitaplığı ile uyumlu olmalıdır. Bu tür kitaplıkların kullanılabilirliği dile ve platforma göre değişiklik gösterir. [OpenID Connect](https://openid.net/connect/) ve [OAuth 2,0](https://oauth.net/2/) Web siteleri popüler uygulamaların bir listesini tutar. Daha fazla bilgi için bkz. [Microsoft Identity platform ve Authentication kitaplıkları](../develop/reference-v2-libraries.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)ve Microsoft Identity platform uç noktasıyla sınanmış açık kaynaklı istemci kitaplıkları ve örnekleri listesi.
* Başvuru için, `.well-known` Microsoft Identity platform ortak uç noktası için uç nokta `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration` . `common`Kiracınıza özgü verileri almak için KIRACı Kimliğinizle değiştirin.  

### <a name="protocol-changes"></a>Protokol değişiklikleri

Microsoft Identity platform uç noktası SAML veya WS-Federation; desteklemez yalnızca OpenID Connect ve OAuth 2,0 ' i destekler.  V 1.0 uç noktasındaki OAuth 2,0 protokollerine yapılan önemli değişiklikleri şunlardır: 

* `email`İsteğe bağlı bir talep yapılandırılmışsa **veya** istekte Scope = email belirtilmişse talep döndürülür. 
* Parametresi, `scope` parametresi yerine artık desteklenmektedir `resource` .  
* Birçok yanıt, OAuth 2,0 belirtimine daha uyumlu hale getirmek için değiştirilmiştir, örneğin, `expires_in` bir dize yerine doğru bir int olarak döndürülüyor.  

Microsoft Identity platform uç noktasında desteklenen protokol işlevselliğinin kapsamını daha iyi anlamak için bkz. [OpenID Connect ve OAuth 2,0 protokol başvurusu](../develop/active-directory-v2-protocols.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

#### <a name="saml-restrictions"></a>SAML kısıtlamaları

Windows uygulamalarında Active Directory Authentication Library (ADAL) kullandıysanız, Security Assertion Markup Language (SAML) onaylama izni kullanan Windows tümleşik kimlik doğrulamasının avantajlarından faydalanabilirsiniz. Bu izin ile, federasyon Azure AD kiracılarının kullanıcıları kimlik bilgilerini girmeden şirket içi Active Directory örneğiyle sessizce kimlik doğrulaması yapabilir. SAML onaylama izni, Microsoft Identity platform uç noktasında desteklenmez.
