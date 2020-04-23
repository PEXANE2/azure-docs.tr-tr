---
title: OAuth 2.0 örtülü hibe akışı - Microsoft kimlik platformu | Azure
description: Microsoft kimlik platformu örtük akışı kullanarak tek sayfalı uygulamaları güvenli hale.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 89ae088b9cbb3bb3c593cfcbbfb4ce619baccfa8
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81868421"
---
# <a name="microsoft-identity-platform-and-implicit-grant-flow"></a>Microsoft kimlik platformu ve Örtülü hibe akışı

Microsoft kimlik platformu bitiş noktası ile, kullanıcıları Microsoft'un kişisel ve iş veya okul hesaplarıyla tek sayfalı uygulamalarınızda oturum açabilirsiniz. Öncelikle bir tarayıcıda çalışan tek sayfalı ve diğer JavaScript uygulamaları, kimlik doğrulama söz konusu olduğunda birkaç ilginç zorlukla karşı karşıyadır:

* Bu uygulamaların güvenlik özellikleri geleneksel sunucu tabanlı web uygulamalarından önemli ölçüde farklıdır.
* Birçok yetkilendirme sunucusu ve kimlik sağlayıcısı CORS isteklerini desteklemez.
* Tam sayfa tarayıcı uygulamadan uzağa yönlendirir özellikle kullanıcı deneyimine invaziv hale gelir.

Bu uygulamalar için (Açısal, Ember.js, React.js, vb.), Microsoft kimlik platformu OAuth 2.0 Örtülü Hibe akışını destekler. Örtük akış [OAuth 2.0 Belirtiminde](https://tools.ietf.org/html/rfc6749#section-4.2)açıklanmıştır. Birincil yararı, uygulamanın arka uç sunucu kimlik bilgisi alışverişi yapmadan Microsoft kimlik platformundan belirteçler elde etmesine olanak sağlamasıdır. Bu, uygulamanın kullanıcıda oturum açmasına, oturumu sürdürmesine ve istemci JavaScript kodu içindeki diğer web AP'lerine belirteçler almalarına olanak tanır. Özellikle [istemci](https://tools.ietf.org/html/rfc6749#section-10.3) ve [kullanıcı kimliğine bürünme](https://tools.ietf.org/html/rfc6749#section-10.3)etrafında örtük akışı kullanırken dikkate alınması gereken birkaç önemli güvenlik hususları vardır.

Bu makalede, uygulamanızdaki protokole karşı doğrudan programlama nın nasıl yapılacağını açıklanmaktadır.  Mümkün olduğunda, [belirteçleri elde etmek ve güvenli web API'lerini aramak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)yerine desteklenen Microsoft Kimlik Doğrulama Kitaplıklarını (MSAL) kullanmanızı öneririz.  Ayrıca [MSAL kullanan örnek uygulamalara](sample-v2-code.md)da göz atın.

Ancak, tek sayfalık uygulamanızda kitaplık kullanmamayı ve protokol iletilerini kendiniz göndermeyi tercih ediyorsanız, aşağıdaki genel adımları izleyin.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>OAuth2 örtülü hibe için uygun senaryolar

OAuth2 belirtimi, örtük hibenin kullanıcı aracısı uygulamalarını etkinleştirmek için tasarlandığını beyan eder – yani, JavaScript uygulamaları bir tarayıcı içinde yürütülür. Bu tür uygulamaların belirleyici özelliği, JavaScript kodunun sunucu kaynaklarına (genellikle bir web API'si) erişmek ve uygulama kullanıcı deneyimini buna göre güncelleştirmek için kullanılmasıdır. Gmail veya Outlook Web Erişimi gibi uygulamaları düşünün: Gelen kutunuzdan bir ileti seçtiğinizde, yeni seçimi görüntülemek için yalnızca ileti görselleştirme paneli değişirken, sayfanın geri kalanı değiştirilmemiş olarak kalır. Bu özellik, her kullanıcı etkileşiminin tam sayfa postback ve yeni sunucu yanıtının tam sayfa olarak işlenmesiyle sonuçlandığı geleneksel yönlendirme tabanlı Web uygulamalarıyla zıttır.

JavaScript tabanlı yaklaşımı aşırıya katan uygulamalara tek sayfalı uygulamalar veya SPA'lar denir. Fikir bu uygulamalar sadece bir ilk HTML sayfası ve ilişkili JavaScript hizmet, tüm sonraki etkileşimleri web API aramaları JavaScript üzerinden gerçekleştirilen tarafından tahrik ediliyor olmasıdır. Ancak, uygulama çoğunlukla postback odaklı ama zaman zaman JS çağrıları gerçekleştirir hibrid yaklaşımlar, nadir değildir - örtük akış kullanımı hakkında tartışma da bu için ilgilidir.

Yönlendirme tabanlı uygulamalar genellikle tanımlama bilgileri aracılığıyla isteklerini güvence altına alır, ancak bu yaklaşım JavaScript uygulamaları için de çalışmaz. Çerezler yalnızca oluşturuldukları etki alanına göre çalışır, Ancak JavaScript çağrıları diğer etki adlarına yönlendirilebilir. Aslında, bu sık sık olacaktır: Microsoft Graph API, Office API, Azure API çağıran uygulamaları düşünün - tüm uygulama nın sunulduğu yerden etki alanı dışında ikamet eden. JavaScript uygulamaları için büyüyen bir eğilim, üçüncü taraf web API'lerine %100 güvenerek iş işlevlerini uygulamak için hiçbir geri uça sahip olmakdeğildir.

Şu anda, bir web API aramaları korumak için tercih edilen yöntem, her arama bir OAuth2 erişim belirteci eşlik nerede OAuth2 taşıyıcı belirteç yaklaşımı kullanmaktır. Web API gelen erişim belirteci inceler ve gerekli kapsamları bulursa, istenen operasyona erişim sağlar. Örtük akış, JavaScript uygulamalarının bir web API'si için erişim belirteçleri elde etmesi için kullanışlı bir mekanizma sağlayarak çerezlerle ilgili olarak çok sayıda avantaj sunar:

* Jetonlar, çapraz menşeli aramalara gerek kalmadan güvenilir bir şekilde elde edilebilir – jetonların yerlerinin yerinden edilmeyeceğinin iade garantisi olduğu yeniden yönlendirme URI'nin zorunlu kaydı
* JavaScript uygulamaları, hedefledikleri web API'leri için, etki alanlarında herhangi bir kısıtlama olmadan, gereksinim duydukları kadar erişim belirteçleri elde edebilirsiniz
* Oturum veya yerel depolama gibi HTML5 özellikleri belirteç önbelleğe alma ve ömür boyu yönetimi üzerinde tam kontrol sağlarken, tanımlama bilgileri yönetimi uygulamaya opaktır
* Erişim belirteçleri, site ler arası istek sahteciliği (CSRF) saldırılarına açık değildir

Örtük hibe akışı, çoğunlukla güvenlik nedenleriyle yenileme belirteçleri vermez. Bir yenileme belirteci erişim belirteçleri kadar dar kapsamlı değildir, dolayısıyla dışarı sızdırılmış durumda çok daha fazla hasar veren çok daha fazla güç veren. Örtük akışta, belirteçler URL'de teslim edilir, bu nedenle engelleme riski yetkilendirme kodu hibesinden daha yüksektir.

Ancak, bir JavaScript uygulaması, kullanıcıdan sürekli olarak kimlik bilgilerini sormadan erişim belirteçlerini yenilemek için emrinde başka bir mekanizmaya sahiptir. Uygulama, Azure AD'nin yetkilendirme bitiş noktasına karşı yeni belirteç istekleri gerçekleştirmek için gizli bir iframe kullanabilir: tarayıcının Azure AD etki alanına karşı etkin bir oturumu (okuma: oturum çerezi vardır) olduğu sürece, kimlik doğrulama isteği kullanıcı etkileşimine gerek kalmadan başarıyla gerçekleşebilir.

Bu model, JavaScript uygulamasına erişim belirteçlerini bağımsız olarak yenileme ve hatta yeni bir API için yeni lerini edinme olanağı verir (kullanıcının daha önce onlar için onay vermiş olması koşuluyla). Bu, yenileme belirteci gibi yüksek değerli bir yapıyı edinme, koruma ve koruma yükünü önler. Sessiz yenilemeyi mümkün kılan yapı, Azure AD oturumu çerezi, uygulama dışında yönetilir. Bu yaklaşımın bir diğer avantajı da, kullanıcının Azure AD'de oturum açtığı uygulamaları kullanarak azure AD'den oturum açabiliyor ve tarayıcı sekmelerinden herhangi birinde çalışıyor. Bu, Azure AD oturumu çerezinin silinmesine neden olur ve JavaScript uygulaması oturumu imzalayan kullanıcının belirteçlerini otomatik olarak yenileme yeteneğini kaybeder.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Örtülü hibe uygulamam için uygun mu?

Örtülü hibe diğer hibelere göre daha fazla risk sunar ve dikkat etmeniz gereken alanlar iyi belgelenmiştir (örneğin, [Dolaylı Akış'ta Kaynak Sahibinin Kimliğine Bürünmek için Erişim Belirteci'nin Kötüye Kullanımı][OAuth2-Spec-Implicit-Misuse] ve [OAuth 2.0 Tehdit Modeli ve Güvenlik Hususları).][OAuth2-Threat-Model-And-Security-Implications] Ancak, daha yüksek risk profili büyük ölçüde etkin kodu çalıştıran, uzak bir kaynak tarafından tarayıcıya sunulan uygulamaları etkinleştirmek için olması nedeniyle. Bir SPA mimarisi planlıyorsanız, arka uç bileşenleriniz yoksa veya JavaScript üzerinden bir web API'sını çağırmak istiyorsanız, belirteç edinimi için örtülü akışın kullanılması önerilir.

Uygulamanız yerel bir istemciise, örtük akış pek uygun değildir. Azure AD oturum çerezinin yerel bir istemci bağlamında olmaması, uygulamanızı uzun ömürlü bir oturumu sürdürme şeklinden mahrum eder. Bu da, uygulamanızın yeni kaynaklar için erişim belirteçleri alırken kullanıcıdan sürekli olarak istekte bulunduğu anlamına gelir.

Arka uç içeren bir Web uygulaması geliştiriyorsanız ve arka uç kodundan bir API tüketiyorsanız, örtük akış da uygun değildir. Diğer bağışlar size çok daha fazla güç verir. Örneğin, OAuth2 istemci kimlik bilgileri hibesi, kullanıcı delegasyonlarının aksine, uygulamanın kendisine atanan izinleri yansıtan belirteçleri alma olanağı sağlar. Bu, istemcinin, kullanıcı bir oturumda etkin bir şekilde meşgul olmasa bile kaynaklara programlı erişimi koruma yeteneğine sahip olduğu anlamına gelir. Sadece bu değil, ama bu tür hibeler daha yüksek güvenlik garantileri verir. Örneğin, erişim belirteçleri kullanıcı tarayıcısı üzerinden hiçbir zaman geçiş yapmaz, tarayıcı geçmişinde kaydedilme riskini göze alamazlar ve böyle devam eder. İstemci uygulaması, belirteç isteğinde de güçlü kimlik doğrulaması gerçekleştirebilir.

[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

## <a name="protocol-diagram"></a>Protokol diyagramı

Aşağıdaki diyagram, tüm örtük oturum açma akışının nasıl göründüğünü gösterir ve izleyen bölümler her adımı daha ayrıntılı olarak açıklar.

![Örtük oturum açma akışını gösteren diyagram](./media/v2-oauth2-implicit-grant-flow/convergence-scenarios-implicit.svg)

## <a name="send-the-sign-in-request"></a>Oturum açma isteğini gönderme

Başlangıçta kullanıcıyı uygulamanızda oturum açmak için bir [OpenID Connect](v2-protocols-oidc.md) kimlik `id_token` doğrulama isteği gönderebilir ve Microsoft kimlik platformu bitiş noktasından bir giriş alabilirsiniz.

> [!IMPORTANT]
> Bir kimlik jetonu ve/veya erişim jetonunu başarıyla talep etmek için, [Azure portalındaki](https://go.microsoft.com/fwlink/?linkid=2083908) uygulama kaydı - Uygulama kayıtları sayfasında, Örtülü **hibe** bölümü altında **kimlik belirteçleri** ve **erişim belirteçleri** seçerek ilgili örtülü hibe akışının etkinleştirilmiş olması gerekir. Etkin değilse, bir `unsupported_response` hata döndürülür: **'response_type' giriş parametresi için sağlanan değer bu istemci için izin verilmez. Beklenen değer 'kod'**

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Örtük akışı kullanarak oturum açmayı test etmek <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank"> https://login.microsoftonline.com/common/oauth2/v2.0/authorize..için.</a> Oturum açmadan sonra tarayıcınız adres `https://localhost/myapp/` çubuğundaki `id_token` bir adres çubuğuna yönlendirilmelidir.
>

| Parametre |  | Açıklama |
| --- | --- | --- |
| `tenant` | gerekli |İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen `common`değerler `organizations` `consumers`, , , ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için [protokol temellerine](active-directory-v2-protocols.md#endpoints)bakın. |
| `client_id` | gerekli | Uygulama (istemci) kimliği, Uygulamanıza atanan [Azure portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasıdır. |
| `response_type` | gerekli |OpenID `id_token` Connect oturum açma için şunları içermelidir. Ayrıca response_type `token`içerebilir. Burada `token` kullanmak, uygulamanızın yetkili bitiş noktasına ikinci bir istekte bulunmak zorunda kalmadan yetkilendirme bitiş noktasından hemen bir erişim jetonu almasına olanak sağlar. `token` response_type kullanıyorsanız, `scope` parametrenin belirteci için hangi kaynağı düzenleyeceklerini belirten bir kapsam içermesi gerekir (örneğin, Microsoft Graph'ta user.read).  |
| `redirect_uri` | Önerilen |Uygulamanızın, kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabileceği redirect_uri. Portala kaydettiğiniz redirect_uris tam olarak biriyle eşleşmesi gerekir, ancak url'nin kodlanmış olması gerekir. |
| `scope` | gerekli |Alan ayrılmış [kapsamlar](v2-permissions-and-consent.md)listesi. OpenID Connect (id_tokens) için, `openid`onay UI'deki "Oturum aç" iznine çevrilen kapsamı içermelidir. İsteğe bağlı olarak, ek `email` `profile` kullanıcı verilerine erişmek için kapsamları ve kapsamları da eklemek isteyebilirsiniz. Erişim belirteci isteniyorsa, çeşitli kaynaklara onay istemek için bu isteğe başka kapsamlar da dahil edebilirsiniz. |
| `response_mode` | isteğe bağlı |Ortaya çıkan belirteci uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. Yalnızca bir erişim jetonu için sorgu varsayılan, ancak istek bir id_token içeriyorsa parçası. |
| `state` | Önerilen |Belirteç yanıtında da döndürülecek isteğe dahil edilen bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Rasgele oluşturulan benzersiz değer genellikle [çapraz site isteği sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada (örneğin, üzerinde oldukları sayfa veya görünüm) kodlamak için de kullanılır. |
| `nonce` | gerekli |Uygulama tarafından oluşturulan isteğe dahil edilen ve elde edilen id_token talep olarak dahil edilecek bir değer. Uygulama daha sonra belirteç yeniden oynatma saldırılarını azaltmak için bu değeri doğrulayabilir. Değer genellikle isteğin kaynağını tanımlamak için kullanılabilecek rasgele, benzersiz bir dizedir. yalnızca bir id_token istendiğinde gereklidir. |
| `prompt` | isteğe bağlı |Gerekli kullanıcı etkileşimi türünü gösterir. Şu anda geçerli olan tek değerler 'giriş', 'yok', 'select_account', ve 'rıza'dır. `prompt=login`kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlar ve tek oturum açmayı olumsuzlar. `prompt=none`tam tersidir - kullanıcıya herhangi bir etkileşimli istemi nin sunulmamasını sağlar. İstek tek oturum açma yoluyla sessizce tamamlanamazsa, Microsoft kimlik platformu bitiş noktası bir hata döndürecektir. `prompt=select_account`kullanıcıyı, oturumda hatırlanan tüm hesapların görüneceği bir hesap seçiciye gönderir. `prompt=consent`kullanıcı oturum açtıktan sonra OAuth onay iletişim kutusunu tetikler ve kullanıcıdan uygulamaya izin vermesini ister. |
| `login_hint`  |isteğe bağlı |Kullanıcı adını önceden biliyorsanız, kullanıcı için oturum açma sayfasının kullanıcı adı/e-posta adresi alanını önceden doldurmak için kullanılabilir. Genellikle uygulamalar, `preferred_username` talebi kullanarak önceki bir oturum açma dan kullanıcı adını ayıklamış olan yeniden kimlik doğrulama sırasında bu parametreyi kullanır.|
| `domain_hint` | isteğe bağlı |Dahil edilirse, kullanıcının oturum açma sayfasında geçtiği e-posta tabanlı bulma işlemini atlar ve bu da biraz daha kolay laştırılmış bir kullanıcı deneyimine yol açtır. Bu genellikle, belirli bir kiracı içinde bir etki alanı adı sağlayacakları tek bir kiracıda çalışan İşletmeler In Line uygulamaları için kullanılır.  Bu, kullanıcıyı kiracı için federasyon sağlayıcısına iletir.  Bunun konukların bu uygulamaya girmesini engelleyeceğini unutmayın.  |

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve kimlik doğrulamasını tamamlaması istenir. Microsoft kimlik platformu bitiş noktası, kullanıcının `scope` sorgu parametresinde belirtilen izinleri kabul etmesini de sağlar. Kullanıcı bu izinlerin **hiçbirini** kabul etmemişse, kullanıcıdan gerekli izinleri onaylamasını ister. Daha fazla bilgi için [izinlere, onay adabına ve çok kiracılı uygulamalara](v2-permissions-and-consent.md)bakın.

Kullanıcı kimlik doğrulaması yaptıktan ve onay verdikten sonra, Microsoft kimlik platformu bitiş `redirect_uri` `response_mode` noktası, parametrede belirtilen yöntemi kullanarak belirtilen uygulamanıza bir yanıt döndürür.

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir `response_mode=fragment` yanıt `response_type=id_token+token` kullanarak ve aşağıdaki gibi görünüyor (okunabilirlik için satır sonları ile):

```HTTP
GET https://localhost/myapp/#
&token_type=Bearer
&expires_in=3599
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` |İçerde `response_type` `token`yene dahil. Uygulamanın istediği erişim belirteci. Erişim belirteci deşifre edilmemeli veya başka bir şekilde denetlenmemeli, opak bir dize olarak ele alınmalıdır. |
| `token_type` |İçerde `response_type` `token`yene dahil. Her zaman. `Bearer` |
| `expires_in`|İçerde `response_type` `token`yene dahil. Önbelleğe alma amacıyla belirteç geçerli olan saniye sayısını gösterir. |
| `scope` |İçerde `response_type` `token`yene dahil. access_token geçerli olacağı kapsamı gösterir. Kullanıcı için geçerli değilse, istenen kapsamların tümünü içermeyebilir (oturum açmak için kişisel bir hesap kullanıldığında yalnızca Azure AD kapsamları isteniyorsa). |
| `id_token` | Bir imzalı JSON Web Belirteci (JWT). Uygulama, oturum giren kullanıcı hakkında bilgi istemek için bu belirteç segmentlerinin şifresini çözebilir. Uygulama değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bunlara güvenmemelidir. id_tokens hakkında daha fazla [`id_token reference`](id-tokens.md)bilgi için bkz. <br> **Not:** Yalnızca kapsam `openid` isteniyorsa sağlanır. |
| `state` |İsteğe bir durum parametresi dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve yanıttaki durum değerlerinin aynı olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Hata yanıtları, uygulamanın `redirect_uri` bunları uygun şekilde işleyebilmeleri için de gönderilebilir:

```HTTP
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| `error` |Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| `error_description` |Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

## <a name="getting-access-tokens-silently-in-the-background"></a>Arka planda sessizce erişim belirteçleri alma

Kullanıcıyı tek sayfalı uygulamanızda imzaladığınızda, [Microsoft Graph](https://developer.microsoft.com/graph)gibi Microsoft kimlik platformu tarafından güvenli web API'lerini aramak için sessizce erişim belirteçleri alabilirsiniz. `token` response_type kullanarak zaten bir belirteç almış olsanız bile, kullanıcıyı yeniden oturum açmaya yönlendirmek zorunda kalmadan ek kaynaklara belirteçler elde etmek için bu yöntemi kullanabilirsiniz.

Normal OpenID Connect/OAuth akışında, Microsoft kimlik platformu `/token` bitiş noktasına bir istekte bulunarak bunu yaparsınız. Ancak, Microsoft kimlik platformu bitiş noktası CORS isteklerini desteklemez, bu nedenle jetonları almak ve yenilemek için AJAX aramaları yapmak söz konusu değildir. Bunun yerine, diğer web API'leri için yeni belirteçler almak için gizli bir iframe örtük akışı kullanabilirsiniz:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&response_mode=fragment
&state=12345
&nonce=678910
&prompt=none
&login_hint=myuser@mycompany.com
```

URL'deki sorgu parametreleri hakkındaki ayrıntılar için [oturum açma isteğini gönderin' e](#send-the-sign-in-request)bakın.

> [!TIP]
> Aşağıdaki isteği tarayıcı sekmesine yapıştırmaya & kopyalamayı deneyin! `login_hint` (Değerleri kullanıcınız için doğru değerle değiştirmeyi unutmayın)
>
>`https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&login_hint={your-username}`
>

`prompt=none` Parametre sayesinde, bu istek ya başarılı olacak ya da hemen başarısız olacak ve başvurunuza geri dönecektir. Parametrede belirtilen yöntem kullanılarak belirtilen `redirect_uri`uygulamanıza başarılı bir yanıt gönderilir. `response_mode`

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir `response_mode=fragment` yanıt kullanarak gibi görünüyor:

```HTTP
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fdirectory.read
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` |İçerde `response_type` `token`yene dahil. Bu durumda, uygulamanın istediği erişim belirteci Microsoft Graph için. Erişim belirteci deşifre edilmemeli veya başka bir şekilde denetlenmemeli, opak bir dize olarak ele alınmalıdır. |
| `token_type` | Her zaman. `Bearer` |
| `expires_in` | Önbelleğe alma amacıyla belirteç geçerli olan saniye sayısını gösterir. |
| `scope` | access_token geçerli olacağı kapsamı gösterir. Kullanıcı için geçerli değilse, istenen kapsamların tümünü içermeyebilir (oturum açmak için kişisel bir hesap kullanıldığında yalnızca Azure AD kapsamları isteniyorsa). |
| `id_token` | Bir imzalı JSON Web Belirteci (JWT). İçerde `response_type` `id_token`yene dahil. Uygulama, oturum giren kullanıcı hakkında bilgi istemek için bu belirteç segmentlerinin şifresini çözebilir. Uygulama değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bunlara güvenmemelidir. id_tokens hakkında daha fazla bilgi için [ `id_token` başvuruya](id-tokens.md)bakın. <br> **Not:** Yalnızca kapsam `openid` isteniyorsa sağlanır. |
| `state` |İsteğe bir durum parametresi dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve yanıttaki durum değerlerinin aynı olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Hata yanıtları da uygulamanın bunları uygun şekilde işleyebilecek `redirect_uri` şekilde gönderilebilir. Durumunda `prompt=none`, beklenen bir hata olacaktır:

```HTTP
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| `error` |Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| `error_description` |Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

Bu hatayı iframe isteğinde alırsanız, yeni bir belirteç almak için kullanıcının etkileşimli olarak yeniden oturum açması gerekir. Bu servis talebiyle uygulamanız için mantıklı ne olursa olsun işlemeyi seçebilirsiniz.

## <a name="refreshing-tokens"></a>Yenileyici belirteçler

Örtülü hibe, yenileme belirteçleri sağlamaz. Hem `id_token`s `access_token`hem de s kısa bir süre sonra sona erecek, bu nedenle uygulamanız bu belirteçleri periyodik olarak yenilemeye hazır olmalıdır. Her iki belirteç türünü yenilemek için, kimlik platformunun `prompt=none` davranışını denetlemek için parametreyi kullanarak yukarıdan aynı gizli iframe isteğini gerçekleştirebilirsiniz. Yeni bir `id_token`almak istiyorsanız, `id_token` `response_type` bir `scope=openid` `nonce` parametre nin yanı sıra ve içinde kullandığınızdan emin olun.

## <a name="send-a-sign-out-request"></a>Oturum açma isteği gönderme

OpenID Connect, `end_session_endpoint` uygulamanızın kullanıcının oturumunu sona erdirmek ve Microsoft kimlik platformu bitiş noktası tarafından ayarlanan çerezleri temizlemek için Microsoft kimlik platformu bitiş noktasına bir istek göndermesine olanak tanır. Bir kullanıcıyı bir web uygulamasından tam olarak imzalamak için, uygulamanız kullanıcıyla olan oturumunu sona erdirmeli (genellikle bir belirteç önbelleğini temizleyerek veya çerezleri bırakarak) ve ardından tarayıcıyı şu şekilde yönlendirmelidir:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| `tenant` |gerekli |İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen `common`değerler `organizations` `consumers`, , , ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için [protokol temellerine](active-directory-v2-protocols.md#endpoints)bakın. |
| `post_logout_redirect_uri` | Önerilen | Oturum açma tamamlandıktan sonra kullanıcının iade edilmesi gereken URL. Bu değer, uygulama için kayıtlı yeniden yönlendirme URI'lerinden biriyle eşleşmelidir. Dahil edilmezse, kullanıcıya Microsoft kimlik platformu bitiş noktası tarafından genel bir ileti gösterilir. |

## <a name="next-steps"></a>Sonraki adımlar

* Kodlamaya başlamak için [MSAL JS örneklerini](sample-v2-code.md) ele geçin.
