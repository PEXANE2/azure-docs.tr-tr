---
title: OAuth 2,0 örtük verme akışı-Microsoft Identity platform | Mavisi
description: Microsoft Identity platform örtük akışını kullanarak tek sayfalı uygulamaları güvenli hale getirin.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 34ae87859b20895598611d25eb069fd05c24d262
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82900410"
---
# <a name="microsoft-identity-platform-and-implicit-grant-flow"></a>Microsoft Identity platform ve örtük verme akışı

Microsoft Identity platform uç noktası ile, Microsoft 'un hem kişisel hem de iş veya okul hesaplarıyla kullanıcıları tek sayfalı uygulamalarınıza imzalayabilirsiniz. Birincil olarak bir tarayıcıda çalışan tek sayfalı ve diğer JavaScript uygulamaları, kimlik doğrulamasından geldiği sırada birkaç ilginç zorluk sunar:

* Bu uygulamaların güvenlik özellikleri geleneksel sunucu tabanlı Web uygulamalarından önemli ölçüde farklıdır.
* Birçok yetkilendirme sunucusu ve kimlik sağlayıcısı CORS isteklerini desteklemez.
* Uygulamanın dışından tam sayfa tarayıcı yeniden yönlendirmeleri, Kullanıcı deneyimine özellikle karşı bir inme gelir.

Bu uygulamalar (angular, Ember. js, tepki. js vb.) için Microsoft Identity platform, OAuth 2,0 örtük Izin akışını destekler. Örtük akış, [OAuth 2,0 belirtiminde](https://tools.ietf.org/html/rfc6749#section-4.2)açıklanmaktadır. Birincil avantajı, uygulamanın arka uç sunucu kimlik bilgisi değişimi gerçekleştirmeden Microsoft Identity platformundan belirteçler almasına izin vermekle aynıdır. Bu, uygulamanın kullanıcıya oturum açmasını, oturum korumasını ve diğer Web API 'Lerine yönelik belirteçleri istemci JavaScript kodu içinde almasını sağlar. Özellikle [istemci](https://tools.ietf.org/html/rfc6749#section-10.3) ve [kullanıcı kimliğine bürünme](https://tools.ietf.org/html/rfc6749#section-10.3)etrafında örtük akışı kullanırken dikkate alınması gereken birkaç önemli güvenlik konusu vardır.

Bu makalede, uygulamanızdaki protokolde doğrudan programlanın nasıl yapılacağı açıklanır.  Mümkün olduğunda, [belirteçleri edinmek ve güvenli Web API 'lerini çağırmak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)Için desteklenen Microsoft kimlik doğrulama KITAPLıKLARıNı (msal) kullanmanızı öneririz.  Ayrıca [, msal kullanan örnek uygulamalara](sample-v2-code.md)göz atın.

Ancak, tek sayfalı uygulamanızdaki bir kitaplığı kullanmayı ve protokol iletilerini kendiniz göndermenizi tercih ediyorsanız aşağıdaki genel adımları izleyin.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>OAuth2 örtük verme için uygun senaryolar

OAuth2 belirtimi, örtük verme 'nin, Kullanıcı Aracısı uygulamalarının (örneğin, bir tarayıcıda yürütülen JavaScript uygulamaları) etkinleştirilmesi için bu özelliği kaldırdığınızı bildirir. Bu tür uygulamaların tanımlanması, JavaScript kodunun sunucu kaynaklarına (genellikle bir Web API) erişmek ve uygulama kullanıcı deneyiminin uygun şekilde güncelleştirilmesi için kullanılır. Gmail veya Outlook Web Erişimi gibi uygulamalar düşünün: gelen kutunuzdan bir ileti seçtiğinizde, yalnızca ileti görselleştirme paneli yeni seçimi görüntüleyecek şekilde değişir, ancak sayfanın geri kalanı değiştirilmemiş olarak kalır. Bu özellik geleneksel yeniden yönlendirme tabanlı Web Apps 'in aksine, her kullanıcı etkileşimi tam sayfa geri gönderme işlemiyle ve yeni sunucu yanıtının tam sayfa işlemesi ile sonuçlanır.

Üst sınırına yönelik JavaScript tabanlı yaklaşımdan uygulamalar tek sayfalı uygulamalar veya maça olarak adlandırılır. Fikir, bu uygulamaların yalnızca bir ilk HTML sayfası ve ilişkili JavaScript 'e sahip olduğu ve JavaScript aracılığıyla gerçekleştirilen Web API çağrıları tarafından çalıştırılmakta olan bir sonraki etkileşimi sunan bir uygulamadır. Bununla birlikte, uygulamanın genellikle geri gönderme yapılır ancak zaman zaman JS çağrıları gerçekleştirdiği karma yaklaşımlar, seyrek erişimli değildir. örtülü akış kullanımı hakkındaki tartışma, bunlar için de geçerlidir.

Yeniden yönlendirme tabanlı uygulamalar, genellikle istekleri tanımlama bilgileri aracılığıyla güvenli hale getirmeye karşın bu yaklaşım, JavaScript uygulamaları için de çalışmaz. Tanımlama bilgileri, JavaScript çağrıları diğer etki alanlarına yönlendirilirken, yalnızca için oluşturulan etki alanına karşı çalışır. Aslında bu durum genellikle şu şekilde olacaktır: Microsoft Graph API, Office API 'si, Azure API 'SI çağıran uygulamaları, uygulamanın sunulduğu etki alanının dışında bulundurduğunu düşünün. JavaScript uygulamalarına yönelik büyümekte olan bir eğilim, iş işlevlerini uygulamak için üçüncü taraf Web API 'Lerinde %100 bağlı bir arka uç gerektirmez.

Şu anda, bir Web API 'sine yapılan çağrıları korumanın tercih edilen yöntemi, her çağrının bir OAuth2 erişim belirteciyle birlikte olduğu OAuth2 taşıyıcı belirteç yaklaşımını kullanmaktır. Web API 'SI, gelen erişim belirtecini inceler ve gerekli kapsamlar içinde bulunursa, istenen işleme erişim izni verir. Örtük akış, JavaScript uygulamalarının bir Web API 'sine erişim belirteçleri elde etmek için kullanışlı bir mekanizma sağlar ve tanımlama bilgilerine göre çok sayıda avantaj sunar:

* Belirteçler, çapraz kaynak çağrılarına gerek kalmadan güvenilir bir şekilde elde edilebilir: belirteçlerin döndürüleceği yeniden yönlendirme URI 'sinin zorunlu kaydı, belirteçlerin hatalı olarak döndürülmediğini garanti eder
* JavaScript uygulamaları, istedikleri sayıda Web API 'si için, her bir etki alanında kısıtlama olmadan, gerek duydukları sayıda erişim belirteci elde edebilir
* Oturum veya yerel depolama gibi HTML5 özellikleri, belirteç önbelleğe alma ve ömür yönetimi üzerinde tam denetim verir, ancak tanımlama bilgileri yönetimi uygulama için donuk olur
* Erişim belirteçleri siteler arası istek sahteciliğini önleme (CSRF) saldırılarına karşı savunmasız değildir

Örtük verme akışı, genellikle güvenlik nedenleriyle yenileme belirteçleri vermez. Yenileme belirteci, erişim belirteçleri olarak en dar kapsamlı değildir, bu nedenle çok daha fazla güç verilmeye devam ediyor ve çok daha fazla Zararsızmış olabilir. Örtük akışta, belirteçler URL 'de dağıtılır, bu nedenle, geri alma riski yetkilendirme kodu verenden daha yüksektir.

Ancak, bir JavaScript uygulamasının, kullanıcıya kimlik bilgileri için sürekli olarak sormadan erişim belirteçlerini yenilemeye yönelik başka bir mekanizması vardır. Uygulama, Azure AD 'nin yetkilendirme uç noktasına karşı yeni belirteç istekleri gerçekleştirmek için gizli bir iframe kullanabilir: tarayıcının hala Azure AD etki alanına karşı etkin bir oturum (okuma: bir oturum tanımlama bilgisi vardır) olduğu sürece, kimlik doğrulama isteği Kullanıcı etkileşimi gerektirmeden başarılı bir şekilde gerçekleştirilebilir.

Bu model, JavaScript uygulamasına, erişim belirteçlerini bağımsız olarak yenileme ve hatta yeni bir API için yeni bir API (Kullanıcı tarafından daha önce kendisine onay verilmiş olan) için yeni bir API alma özelliği verir. Bu, yenileme belirteci gibi yüksek değerli yapıtları alma, sürdürme ve koruma yükünü ortadan kaldırır. Sessiz yenilemeyi mümkün kılan yapıt, Azure AD oturum tanımlama bilgisi uygulama dışında yönetilir. Bu yaklaşımın bir diğer avantajı, Azure AD 'de oturum açmış uygulamalardan herhangi birini kullanarak, tarayıcı sekmelerinden birinde çalışan bir kullanıcının Azure AD 'den oturumu açmasını sağlayabilir. Bu, Azure AD oturum tanımlama bilgisinin silinmesine neden olur ve JavaScript uygulaması, oturum açan kullanıcı için belirteçleri yenileme özelliğini otomatik olarak kaybeder.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Örtük izin uygulamam için uygun midir?

Örtük verme, diğer izin verenden daha fazla risk sunar ve dikkat etmeniz gereken alanların iyi belgelendiğinden (örneğin, [örtük akışta kaynak sahibine taklit etmek Için erişim belirtecinin kötüye kullanımı][OAuth2-Spec-Implicit-Misuse] ve [OAuth 2,0 tehdit modeli ve güvenlik konuları][OAuth2-Threat-Model-And-Security-Implications]). Ancak, daha yüksek riskli profil büyük ölçüde, uzak bir kaynak tarafından tarayıcıya hizmet veren etkin kodu çalıştıran uygulamaların etkinleştirilmesi amaçlıyordu. SPA mimarisi planlarken, arka uç bileşenleri yoksa veya JavaScript aracılığıyla bir Web API 'SI çağırmak istiyorsanız, belirteç alımı için örtük akışın kullanılması önerilir.

Uygulamanız yerel bir istemcili ise, örtük akış harika bir uyum değildir. Azure AD oturum tanımlama bilgisinin bir yerel istemci bağlamında yokluğu, uygulamanızı uzun süreli bir oturum koruma yoluyla kaldırır. Yani, uygulamanız yeni kaynaklar için erişim belirteçleri alırken Kullanıcı tarafından tekrar tekrar sorulacak.

Arka uç içeren ve arka uç kodundan bir API kullanan bir Web uygulaması geliştiriyorsanız, örtük akış de uygun değildir. Diğer izinler size daha fazla güç verir. Örneğin, OAuth2 istemci kimlik bilgileri verme, Kullanıcı temsilcilerinin aksine uygulamanın kendisine atanan izinleri yansıtan belirteçleri elde etme yeteneği sağlar. Bu, bir kullanıcı oturumunda etkin bir şekilde olmadığında ve bu şekilde devam ettiğinizde, istemcinin kaynaklara programlı erişim sağlama yeteneğine sahip olduğu anlamına gelir. Ancak, bu tür izinler daha yüksek güvenlik garantisi verir. Örneğin, erişim belirteçleri Kullanıcı tarayıcısından hiçbir şekilde geçiş yapmayın, tarayıcı geçmişine kaydedilmeleri ve bu şekilde devam eder. İstemci uygulaması, belirteç istenirken güçlü kimlik doğrulaması da gerçekleştirebilir.

[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

## <a name="protocol-diagram"></a>Protokol diyagramı

Aşağıdaki diyagramda, tüm örtük oturum açma akışının nasıl göründüğü ve izleyen bölümlerin her adımı daha ayrıntılı bir şekilde açıklaması gösterilmektedir.

![Örtük oturum açma akışını gösteren diyagram](./media/v2-oauth2-implicit-grant-flow/convergence-scenarios-implicit.svg)

## <a name="send-the-sign-in-request"></a>Oturum açma isteğini gönder

Kullanıcıyı uygulamanızda ilk kez imzalamak için, bir [OpenID Connect](v2-protocols-oidc.md) kimlik doğrulama isteği gönderebilir ve Microsoft Identity platform uç noktasından `id_token` alabilirsiniz.

> [!IMPORTANT]
> KIMLIK belirteci ve/veya erişim belirtecini başarıyla istemek için, [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasındaki uygulama kaydında, **Kimlik belirteçleri** ve ' ı seçerek ve **örtülü izin** bölümünün altındaki **belirteçlere erişim** için, karşılık gelen örtük izin akışının etkinleştirilmiş olması gerekir. Etkin değilse bir `unsupported_response` hata döndürülür: **' response_type ' giriş parametresi için sağlanan değere bu istemci için izin verilmiyor. Beklenen değer ' Code '**

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
> Örtük akışı kullanarak oturum açmayı test etmek için öğesine tıklayın <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank"> https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a> Oturum açtıktan sonra, tarayıcınız adres çubuğunda bir `https://localhost/myapp/` `id_token` ile yeniden yönlendirilmelidir.
>

| Parametre |  | Açıklama |
| --- | --- | --- |
| `tenant` | gerekli |İsteğin `{tenant}` yolundaki değeri, uygulamada kimlerin oturum açmasını denetlemek için kullanılabilir. İzin verilen değerler, `common`, `organizations`ve `consumers`kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için bkz. [protokol temelleri](active-directory-v2-protocols.md#endpoints). |
| `client_id` | gerekli | [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atadığı uygulama (ISTEMCI) kimliği. |
| `response_type` | gerekli |OpenID `id_token` Connect oturum açma için içermelidir. Ayrıca, response_type `token`de içerebilir. Burada `token` kullanılması, uygulamanızın yetkilendirme uç noktasına ikinci bir istek yapmadan yetkilendirme uç noktasından hemen erişim belirteci almasına izin verir. `token` Response_type kullanırsanız, `scope` parametrenin belirtecin hangi kaynakla ilgili olduğunu belirten bir kapsam içermesi gerekir (örneğin, Microsoft Graph üzerinde User. Read).  |
| `redirect_uri` | Önerilen |Uygulamanızın, kimlik doğrulama yanıtlarının sizin uygulamanız tarafından gönderilebileceği ve alınabileceği redirect_uri. Portalın, URL kodlamalı olması dışında, portalda kaydettiğiniz redirect_uris biriyle tam olarak eşleşmesi gerekir. |
| `scope` | gerekli |[Kapsamların](v2-permissions-and-consent.md)boşlukla ayrılmış listesi. OpenID Connect (id_tokens) için, izin Kullanıcı arabirimindeki "oturum `openid`aç" iznine çeviren kapsamı içermelidir. İsteğe bağlı olarak, `email` ek kullanıcı verilerine erişim kazanmak `profile` için ve kapsamlarını da dahil etmek isteyebilirsiniz. Ayrıca, bir erişim belirteci isteniyorsa, çeşitli kaynaklara izin istemek için bu istekteki diğer kapsamları da dahil edebilirsiniz. |
| `response_mode` | isteğe bağlı |Elde edilen belirteci uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. Yalnızca bir erişim belirtecinin sorgulaması yapılır, ancak istek bir id_token içeriyorsa parçalara ayırma olur. |
| `state` | Önerilen |İsteğin belirteç yanıtında de döndürülecek bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulan benzersiz bir değer genellikle [siteler arası istek sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Durum Ayrıca, kullanıcının uygulamadaki durumu hakkında bilgi kodlamak için kullanılır; Örneğin, bulunan sayfa veya görünüm gibi kimlik doğrulama isteği gerçekleştirilmeden önce. |
| `nonce` | gerekli |Uygulama tarafından oluşturulan, sonuçta elde edilen id_token talep olarak dahil edilecek bir değer. Daha sonra uygulama, belirteç yeniden yürütme saldırılarını azaltmak için bu değeri doğrulayabilirler. Değer genellikle, isteğin kaynağını belirlemek için kullanılabilecek rastgele, benzersiz bir dizedir. Yalnızca bir id_token istendiğinde gereklidir. |
| `prompt` | isteğe bağlı |Gerekli kullanıcı etkileşiminin türünü gösterir. Şu anda yalnızca geçerli değerler: ' login', ' none ', ' select_account ' ve ' onay '. `prompt=login`, kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlar ve çoklu oturum açma 'yı yok eder. `prompt=none`Bunun tersi, kullanıcının herhangi bir etkileşimli istem ile sunulmayacağını garanti eder. İstek, tek oturum açma yoluyla sessizce tamamlanamayacak, Microsoft Identity platform uç noktası bir hata döndürür. `prompt=select_account`kullanıcıyı, oturumda hatırlanan tüm hesapların görüneceği bir hesap seçicisine gönderir. `prompt=consent`Kullanıcı oturum açtıktan sonra OAuth onay iletişim kutusunu tetikler, böylece kullanıcıdan uygulamaya izin vermesini istenir. |
| `login_hint`  |isteğe bağlı |Kullanıcı adını zaman önce biliyorsanız, Kullanıcı için oturum açma sayfasının Kullanıcı adı/e-posta adresi alanını önceden doldurabilirsiniz. Genellikle uygulamalar bu parametreyi yeniden kimlik doğrulama sırasında kullanır ve Kullanıcı adını, `preferred_username` talebi kullanarak önceki bir oturum açma işleminden zaten ayıklamış olur.|
| `domain_hint` | isteğe bağlı |Dahil edilse, kullanıcının oturum açma sayfasında, daha kolay bir kullanıcı deneyimi için önde gelen e-posta tabanlı bulma işlemini atlar. Bu, genellikle belirli bir kiracıda bir etki alanı adı sundukları tek bir kiracıya çalışan Iş kolu uygulamaları için kullanılır.  Bu işlem, kullanıcıyı bu kiracının Federasyon sağlayıcısına iletir.  Bunun, konukların bu uygulamada oturum açmasını önleyebileceğini unutmayın.  |

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve kimlik doğrulamasını tamamlaması istenir. Microsoft Identity platform uç noktası ayrıca kullanıcının `scope` sorgu parametresinde belirtilen izinlere onay aldığından emin olur. Kullanıcı bu izinlerden **hiçbirine** izin vermişse, kullanıcıdan gerekli izinleri vermesini ister. Daha fazla bilgi için bkz. [izinler, onay ve çok kiracılı uygulamalar](v2-permissions-and-consent.md).

Kullanıcı kimlik doğrulamasından ve izin verdiğinde, Microsoft Identity platform uç noktası, `redirect_uri` `response_mode` parametresinde belirtilen yöntemi kullanarak, belirtilen şekilde uygulamanıza bir yanıt döndürür.

#### <a name="successful-response"></a>Başarılı yanıt

Kullanılarak `response_mode=fragment` başarılı bir yanıt, `response_type=id_token+token` aşağıdaki gibi görünür (okunabilirliği için satır sonları ile):

```HTTP
GET https://localhost/myapp/#
&token_type=Bearer
&expires_in=3599
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` |Dahil ise `response_type` dahildir `token`. Uygulamanın istediği erişim belirteci. Erişim belirtecinin kodu çözülemedi veya bunun için bir donuk dize olarak değerlendirilmemelidir. |
| `token_type` |Dahil ise `response_type` dahildir `token`. Her zaman olacak `Bearer`. |
| `expires_in`|Dahil ise `response_type` dahildir `token`. Önbelleğe alma amacıyla belirtecin geçerli olduğu saniye sayısını gösterir. |
| `scope` |Dahil ise `response_type` dahildir `token`. Access_token geçerli olacağı kapsam (ler) i gösterir. Kullanıcı için geçerli olmadıklarında istenen tüm kapsamları içermeyebilir (oturum açmak için bir kişisel hesap kullanıldığında yalnızca Azure AD kapsamları isteniyorsa). |
| `id_token` | İmzalı bir JSON Web Token (JWT). Uygulama, oturum açan kullanıcı hakkında bilgi istemek için bu belirtecin segmentlerinin kodunu çözebilir. Uygulama, değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bu değere güvenmemelidir. İd_tokens hakkında daha fazla bilgi için bkz [`id_token reference`](id-tokens.md).. <br> **Note:** Yalnızca `openid` kapsam isteniyorsa sağlandı. |
| `state` |İsteğe bir durum parametresi dahil edilir, yanıtta aynı değer görünmelidir. Uygulama, istek ve yanıtta durum değerlerinin özdeş olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Ayrıca, `redirect_uri` uygulamanın bunları uygun şekilde işleyebilmesi için hata yanıtları da gönderilebilir.

```HTTP
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| `error` |Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi. |
| `error_description` |Bir geliştiricinin kimlik doğrulama hatasının kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

## <a name="getting-access-tokens-silently-in-the-background"></a>Arka planda sessizce erişim belirteçleri alma

Kullanıcıyı tek sayfalı uygulamanıza imzaladığınıza göre, [Microsoft Graph](https://developer.microsoft.com/graph)gibi Microsoft Identity platform tarafından güvenliği sağlanmış Web API 'lerini çağırmak için sessizce erişim belirteçleri alabilirsiniz. `token` Response_type kullanarak zaten bir belirteç almış olsanız bile, kullanıcıyı yeniden oturum açmak üzere yeniden yönlendirmek zorunda kalmadan ek kaynaklara belirteç almak için bu yöntemi kullanabilirsiniz.

Normal OpenID Connect/OAuth akışında, Microsoft Identity Platform `/token` uç noktasına bir istek yaparak bunu yapabilirsiniz. Ancak, Microsoft Identity platform uç noktası CORS isteklerini desteklemez, bu nedenle belirteçleri alma ve yenileme için AJAX çağrılarını gerçekleştirmek söz konusu değildir. Bunun yerine, diğer Web API 'Leri için yeni belirteçler almak üzere bir gizli iframe 'de örtük akışı kullanabilirsiniz:

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

URL 'deki sorgu parametreleriyle ilgili ayrıntılar için bkz. [oturum açma isteğini gönderme](#send-the-sign-in-request).

> [!TIP]
> Aşağıdaki isteği bir tarayıcı sekmesine yapıştırmayı & kopyalamayı deneyin! (Değerleri, `login_hint` Kullanıcı için doğru değerle değiştirmeyi unutmayın)
>
>`https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&login_hint={your-username}`
>

Bu `prompt=none` parametre sayesinde, bu istek hemen başarılı veya başarısız olur ve uygulamanıza geri döner. Parametresinde belirtilen yöntem kullanılarak, belirtilen `redirect_uri`şekilde uygulamanıza başarılı bir yanıt gönderilir. `response_mode`

#### <a name="successful-response"></a>Başarılı yanıt

Kullanarak `response_mode=fragment` başarılı bir yanıt şöyle görünür:

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
| `access_token` |Dahil ise `response_type` dahildir `token`. Uygulamanın istediği erişim belirteci, Microsoft Graph için bu durumda. Erişim belirtecinin kodu çözülemedi veya bunun için bir donuk dize olarak değerlendirilmemelidir. |
| `token_type` | Her zaman olacak `Bearer`. |
| `expires_in` | Önbelleğe alma amacıyla belirtecin geçerli olduğu saniye sayısını gösterir. |
| `scope` | Access_token geçerli olacağı kapsam (ler) i gösterir. Kullanıcı için geçerli olmadıklarında istenen tüm kapsamları içermeyebilir (oturum açmak için bir kişisel hesap kullanıldığında yalnızca Azure AD kapsamları isteniyorsa). |
| `id_token` | İmzalı bir JSON Web Token (JWT). Dahil ise `response_type` dahildir `id_token`. Uygulama, oturum açan kullanıcı hakkında bilgi istemek için bu belirtecin segmentlerinin kodunu çözebilir. Uygulama, değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bu değere güvenmemelidir. İd_tokens hakkında daha fazla bilgi için bkz. [ `id_token` başvuru](id-tokens.md). <br> **Note:** Yalnızca `openid` kapsam isteniyorsa sağlandı. |
| `state` |İsteğe bir durum parametresi dahil edilir, yanıtta aynı değer görünmelidir. Uygulama, istek ve yanıtta durum değerlerinin özdeş olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Ayrıca, `redirect_uri` uygulamanın bunları uygun şekilde işleyebilmesi için hata yanıtları da gönderilebilir. Bu durumda `prompt=none`, beklenen bir hata şu şekilde olur:

```HTTP
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| `error` |Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi. |
| `error_description` |Bir geliştiricinin kimlik doğrulama hatasının kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

İframe isteğinde bu hatayı alırsanız, Kullanıcı yeni bir belirteci almak için etkileşimli olarak yeniden oturum açması gerekir. Bu durumu uygulamanız için anlamlı hale getiren her türlü şekilde yapmayı tercih edebilirsiniz.

## <a name="refreshing-tokens"></a>Belirteçleri yenileme

Örtük izin yenileme belirteçleri sağlamıyor. Her `id_token`iki s `access_token`ve s kısa bir süre sonra sona erer, bu nedenle uygulamanızın bu belirteçleri düzenli aralıklarla yenilemeye hazırlanması gerekir. Her iki tür belirteci de yenilemek için, kimlik platformunun davranışını denetlemek üzere `prompt=none` parametresini kullanarak yukarıdaki aynı gizli iframe isteğini de gerçekleştirebilirsiniz. Yeni `id_token` `id_token` bir almak istiyorsanız, `response_type` ve `scope=openid`' de ' `nonce` de kullandığınızdan emin olun.

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder

OpenID Connect `end_session_endpoint` , uygulamanızın bir kullanıcının oturumunu sona erdirmek ve Microsoft Identity platform uç noktası tarafından ayarlanan tanımlama bilgilerini temizlemek için Microsoft Identity platform uç noktasına bir istek göndermesini sağlar. Bir kullanıcıyı bir Web uygulamasından tam olarak imzalamak için, uygulamanız kullanıcıyla kendi oturumunu sonlandırmalıdır (genellikle bir belirteç önbelleğini temizleyerek veya tanımlama bilgilerini bırakarak) ve ardından tarayıcıyı şuraya yönlendirmelidir:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| `tenant` |gerekli |İsteğin `{tenant}` yolundaki değeri, uygulamada kimlerin oturum açmasını denetlemek için kullanılabilir. İzin verilen değerler, `common`, `organizations`ve `consumers`kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için bkz. [protokol temelleri](active-directory-v2-protocols.md#endpoints). |
| `post_logout_redirect_uri` | Önerilen | Oturum kapatma tamamlandıktan sonra kullanıcının geri döndürülmesi gereken URL. Bu değer, uygulama için kaydedilmiş yeniden yönlendirme URI 'lerinden biriyle aynı olmalıdır. Dahil edilmezse, kullanıcıya Microsoft Identity platform uç noktası tarafından genel bir ileti gösterilir. |

## <a name="next-steps"></a>Sonraki adımlar

* Kodlamaya başlamak için [msal js örneklerinin](sample-v2-code.md) üzerine gidin.

[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
