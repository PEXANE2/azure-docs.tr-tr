---
title: Microsoft Identity platform örtük akışını kullanarak tek sayfalı uygulamaları güvenli hale getirme | Mavisi
description: Tek sayfalı uygulamalar için örtük akışın Microsoft Identity platform uygulamasını kullanarak Web uygulamaları oluşturma.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ab5ba4fde7469854954ed19d2e643f2b8a23f34f
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70193263"
---
# <a name="microsoft-identity-platform-and-implicit-grant-flow"></a>Microsoft Identity platform ve örtük verme akışı

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Microsoft Identity platform uç noktası ile, Microsoft 'un hem kişisel hem de iş veya okul hesaplarıyla kullanıcıları tek sayfalı uygulamalarınıza imzalayabilirsiniz. Birincil olarak bir tarayıcıda çalışan tek sayfalı ve diğer JavaScript uygulamaları, kimlik doğrulamasından geldiği sırada birkaç ilginç zorluk sunar:

* Bu uygulamaların güvenlik özellikleri geleneksel sunucu tabanlı Web uygulamalarından önemli ölçüde farklıdır.
* Birçok yetkilendirme sunucusu ve kimlik sağlayıcısı CORS isteklerini desteklemez.
* Uygulamanın dışından tam sayfa tarayıcı yeniden yönlendirmeleri, Kullanıcı deneyimine özellikle karşı bir inme gelir.

Bu uygulamalar (AngularJS, Ember. js, tepki. js vb.) için Microsoft Identity platform, OAuth 2,0 örtük Izin akışını destekler. Örtük akış, [OAuth 2,0 belirtiminde](https://tools.ietf.org/html/rfc6749#section-4.2)açıklanmaktadır. Birincil avantajı, uygulamanın arka uç sunucu kimlik bilgisi değişimi gerçekleştirmeden Microsoft Identity platformundan belirteçler almasına izin vermekle aynıdır. Bu, uygulamanın kullanıcıya oturum açmasını, oturum korumasını ve diğer Web API 'Lerine yönelik belirteçleri istemci JavaScript kodu içinde almasını sağlar. Özellikle [istemci](https://tools.ietf.org/html/rfc6749#section-10.3) ve [kullanıcı kimliğine bürünme](https://tools.ietf.org/html/rfc6749#section-10.3)etrafında örtük akışı kullanırken dikkate alınması gereken birkaç önemli güvenlik konusu vardır.

JavaScript uygulamanıza kimlik doğrulaması eklemek için örtük akış ve Microsoft Identity platformunu kullanmak istiyorsanız, [msal. js](https://github.com/AzureAD/microsoft-authentication-library-for-js)açık kaynaklı JavaScript kitaplığını kullanmanızı öneririz.

Ancak, tek sayfalı uygulamanızdaki bir kitaplığı kullanmayı ve protokol iletilerini kendiniz göndermenizi tercih ediyorsanız aşağıdaki genel adımları izleyin.

> [!NOTE]
> Tüm Azure Active Directory (Azure AD) senaryoları ve özellikleri Microsoft Identity platform uç noktası tarafından desteklenmez. Microsoft Identity platform uç noktasını kullanmanız gerekip gerekmediğini öğrenmek için [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.

## <a name="protocol-diagram"></a>Protokol diyagramı

Aşağıdaki diyagramda, tüm örtük oturum açma akışının nasıl göründüğü ve izleyen bölümlerin her adımı daha ayrıntılı bir şekilde açıklaması gösterilmektedir.

![Örtük oturum açma akışını gösteren diyagram](./media/v2-oauth2-implicit-grant-flow/convergence-scenarios-implicit.svg)

## <a name="send-the-sign-in-request"></a>Oturum açma isteğini gönder

Kullanıcıyı uygulamanızda ilk kez imzalamak için, bir [OpenID Connect](v2-protocols-oidc.md) kimlik doğrulama isteği gönderebilir ve Microsoft Identity platform uç noktasından `id_token` alabilirsiniz.

> [!IMPORTANT]
> Bir KIMLIK belirtecini başarıyla istemek için, [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasındaki uygulama kaydında örtülü verme akışının, **örtük izin** altındaki **erişim belirteçleri** ve **Kimlik belirteçleri** seçilerek doğru şekilde etkinleştirilmesi gerekir kısmı. Etkin değilse bir `unsupported_response` hata döndürülür: **' Response_type ' giriş parametresi için belirtilen değere bu istemci için izin verilmiyor. Beklenen değer ' Code '**

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
> Örtük akışı kullanarak oturum açmayı test etmek için öğesine tıklayın <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank"> https://login.microsoftonline.com/common/oauth2/v2.0/authorize..</a> Oturum açtıktan sonra, tarayıcınız adres çubuğunda bir `https://localhost/myapp/` `id_token` ile yeniden yönlendirilmelidir.
>

| Parametre |  | Açıklama |
| --- | --- | --- |
| `tenant` | gerekli |İsteğin `{tenant}` yolundaki değeri, uygulamada kimlerin oturum açmasını denetlemek için kullanılabilir. İzin verilen değerler, `common`, `organizations`ve `consumers`kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için bkz. [protokol temelleri](active-directory-v2-protocols.md#endpoints). |
| `client_id` | gerekli | [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atadığı uygulama (ISTEMCI) kimliği. |
| `response_type` | gerekli |OpenID Connect oturum açma içiniçermelidir.`id_token` Ayrıca response_type `token`de içerebilir. Burada `token` kullanılması, uygulamanızın yetkilendirme uç noktasına ikinci bir istek yapmadan yetkilendirme uç noktasından hemen erişim belirteci almasına izin verir. `token` Response_type kullanıyorsanız`scope` , parametresi için belirteci hangi kaynağın yayınlamadığını gösteren bir kapsam içermelidir. |
| `redirect_uri` | Önerilen |Uygulamanızın kimlik doğrulama yanıtlarının gönderilebileceği ve alınabileceği, uygulamanızın redirect_uri 'ı. Portalın, URL kodlamalı olması dışında, portala kaydettiğiniz redirect_uris biriyle tam olarak eşleşmesi gerekir. |
| `scope` | gerekli |[Kapsamların](v2-permissions-and-consent.md)boşlukla ayrılmış listesi. OpenID Connect için, izin Kullanıcı arabirimindeki "oturum `openid`aç" iznine çeviren kapsamı içermelidir. İsteğe bağlı olarak, `email` ek kullanıcı verilerine erişim kazanmak için veya `profile` kapsamlarını da dahil etmek isteyebilirsiniz. Ayrıca, çeşitli kaynaklara izin istemek için bu istekteki diğer kapsamları da dahil edebilirsiniz. |
| `response_mode` | isteğe bağlı |Elde edilen belirteci uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. Bir erişim belirtecinin sorgu varsayılan olarak olur, ancak istek bir id_token içeriyorsa parçaya parçalar. |
| `state` | Önerilen |İsteğin belirteç yanıtında de döndürülecek bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulan benzersiz bir değer genellikle [siteler arası istek sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Durum Ayrıca, kullanıcının uygulamadaki durumu hakkında bilgi kodlamak için kullanılır; Örneğin, bulunan sayfa veya görünüm gibi kimlik doğrulama isteği gerçekleştirilmeden önce. |
| `nonce` | gerekli |Uygulama tarafından oluşturulan, sonuçta elde edilen id_token talep olarak dahil edilecek bir değer. Daha sonra uygulama, belirteç yeniden yürütme saldırılarını azaltmak için bu değeri doğrulayabilirler. Değer genellikle, isteğin kaynağını belirlemek için kullanılabilecek rastgele, benzersiz bir dizedir. Yalnızca bir id_token istendiğinde gereklidir. |
| `prompt` | isteğe bağlı |Gerekli kullanıcı etkileşiminin türünü gösterir. Şu anda yalnızca geçerli değerler: ' login', ' none ', ' select_account ' ve ' onay '. `prompt=login`, kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlar ve çoklu oturum açma 'yı yok eder. `prompt=none`Bunun tersi, kullanıcının herhangi bir etkileşimli istem ile sunulmayacağını garanti eder. İstek, tek oturum açma yoluyla sessizce tamamlanamayacak, Microsoft Identity platform uç noktası bir hata döndürür. `prompt=select_account`kullanıcıyı, oturumda hatırlanan tüm hesapların görüneceği bir hesap seçicisine gönderir. `prompt=consent`Kullanıcı oturum açtıktan sonra OAuth onay iletişim kutusunu tetikler, böylece kullanıcıdan uygulamaya izin vermesini istenir. |
| `login_hint`  |isteğe bağlı |Kullanıcı adını zaman önce biliyorsanız, Kullanıcı için oturum açma sayfasının Kullanıcı adı/e-posta adresi alanını önceden doldurabilirsiniz. Genellikle uygulamalar bu parametreyi yeniden kimlik doğrulama sırasında kullanır ve Kullanıcı adını, `preferred_username` talebi kullanarak önceki bir oturum açma işleminden zaten ayıklamış olur.|
| `domain_hint` | isteğe bağlı |`consumers` Veya`organizations`bunlardan biri olabilir. Dahil edilse, kullanıcının oturum açma sayfasında, daha kolay bir kullanıcı deneyimi için önde gelen e-posta tabanlı bulma işlemini atlar. Genellikle uygulamalar bu parametreyi yeniden kimlik doğrulama sırasında kullanır ve bu, id_token kaynağından `tid` talebi ayıklayarak kullanır. Talep değeri (Microsoft hesabı `9188040d-6c67-4c5b-b112-36a304b66dad` tüketici kiracısı) ise, kullanmanız `domain_hint=consumers`gerekir. `tid` Aksi takdirde, yeniden kimlik `domain_hint=organizations` doğrulama sırasında kullanabilirsiniz. |

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve kimlik doğrulamasını tamamlaması istenir. Microsoft Identity platform uç noktası ayrıca kullanıcının `scope` sorgu parametresinde belirtilen izinlere onay aldığından emin olur. Kullanıcı bu izinlerden **hiçbirine** izin vermişse, kullanıcıdan gerekli izinleri vermesini ister. Daha fazla bilgi için bkz. [izinler, onay ve çok kiracılı uygulamalar](v2-permissions-and-consent.md).

Kullanıcı kimlik doğrulamasından ve izin verdiğinde, Microsoft Identity platform uç noktası, `redirect_uri` `response_mode` parametresinde belirtilen yöntemi kullanarak, belirtilen şekilde uygulamanıza bir yanıt döndürür.

#### <a name="successful-response"></a>Başarılı yanıt

`response_mode=fragment` Kullanılarak`response_type=id_token+token` başarılı bir yanıt, aşağıdaki gibi görünür (okunabilirliği için satır sonları ile):

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fuser.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` |Dahil ise `response_type` `token`dahildir. Uygulamanın istediği erişim belirteci, Microsoft Graph için bu durumda. Erişim belirtecinin kodu çözülemedi veya bunun için bir donuk dize olarak değerlendirilmemelidir. |
| `token_type` |Dahil ise `response_type` `token`dahildir. Her zaman `Bearer`olacak. |
| `expires_in`|Dahil ise `response_type` `token`dahildir. Önbelleğe alma amacıyla belirtecin geçerli olduğu saniye sayısını gösterir. |
| `scope` |Dahil ise `response_type` `token`dahildir. Access_token geçerli olacak kapsam (ler) i gösterir. Kullanıcı için geçerli olmadıklarında istenen tüm kapsamları içermeyebilir (oturum açmak için bir kişisel hesap kullanıldığında yalnızca Azure AD kapsamları isteniyorsa). |
| `id_token` | İmzalı bir JSON Web Token (JWT). Uygulama, oturum açan kullanıcı hakkında bilgi istemek için bu belirtecin segmentlerinin kodunu çözebilir. Uygulama, değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bu değere güvenmemelidir. İd_tokens hakkında daha fazla bilgi için bkz [`id_token reference`](id-tokens.md). <br> **Not:** Yalnızca `openid` kapsam isteniyorsa sağlandı. |
| `state` |İsteğe bir durum parametresi dahil edilir, yanıtta aynı değer görünmelidir. Uygulama, istek ve yanıtta durum değerlerinin özdeş olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Ayrıca, `redirect_uri` uygulamanın bunları uygun şekilde işleyebilmesi için hata yanıtları da gönderilebilir.

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| `error` |Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi. |
| `error_description` |Bir geliştiricinin kimlik doğrulama hatasının kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

## <a name="validate-the-id_token"></a>İd_token doğrulama

Yalnızca bir id_token almak, kullanıcının kimliğini doğrulamak için yeterli değildir; Ayrıca, id_token's imzasını doğrulamanız ve uygulamanızın gereksinimlerine bağlı olarak belirteçteki talepleri doğrulamanız gerekir. Microsoft Identity platform uç noktası belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için [JSON Web belirteçleri (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve ortak anahtar şifrelemesi kullanır.

İstemci kodunda doğrulamayı `id_token` seçebilirsiniz, ancak bunu bir arka uç sunucusuna `id_token` göndermek ve doğrulama gerçekleştirmek için sık kullanılan bir uygulamadır. İd_token imzasını doğruladıktan sonra, doğrulamanız gereken birkaç talep vardır. [Belirteçleri doğrulama](id-tokens.md#validating-an-id_token) ve [anahtar geçişi imzalama hakkındaki önemli bilgileri](active-directory-signing-key-rollover.md)içeren daha fazla bilgi için [ başvuruyabakın.`id_token` ](id-tokens.md) Belirteçleri ayrıştırma ve doğrulama için bir kitaplık kullanmanızı öneririz. çoğu dil ve platform için en az bir kullanılabilir.

Senaryonuza bağlı olarak ek talepler de doğrulamak isteyebilirsiniz. Bazı ortak doğrulamalar şunları içerir:

* Kullanıcı/kuruluşun uygulama için kaydolmasını sağlama.
* Kullanıcının uygun yetkilendirme/ayrıcalıklara sahip olduğundan emin olma.
* Çok faktörlü kimlik doğrulaması gibi belirli bir kimlik doğrulaması kuvvetinin gerçekleştiği doğrulanıyor.

İd_token öğesini doğrulandıktan sonra, Kullanıcı ile oturum başlatabilir ve uygulamanızdaki Kullanıcı hakkında bilgi edinmek için id_token 'teki talepleri kullanabilirsiniz. Bu bilgiler, görüntüleme, kayıtlar, kişiselleştirme ve daha fazlası için kullanılabilir.

## <a name="get-access-tokens"></a>Erişim belirteçleri al

Kullanıcıyı tek sayfalı uygulamanıza imzaladığınıza göre, Microsoft Identity platform tarafından güvenliği sağlanmış Web API 'Leri için [Microsoft Graph](https://developer.microsoft.com/graph)gibi erişim belirteçleri alabilirsiniz. `token` Response_type kullanarak zaten bir belirteç almış olsanız bile, kullanıcıyı yeniden oturum açmak üzere yeniden yönlendirmek zorunda kalmadan ek kaynaklara belirteç almak için bu yöntemi kullanabilirsiniz.

Normal OpenID Connect/OAuth akışında, Microsoft Identity Platform `/token` uç noktasına bir istek yaparak bunu yapabilirsiniz. Ancak, Microsoft Identity platform uç noktası CORS isteklerini desteklemez, bu nedenle belirteçleri alma ve yenileme için AJAX çağrılarını gerçekleştirmek söz konusu değildir. Bunun yerine, diğer Web API 'Leri için yeni belirteçler almak üzere bir gizli iframe 'de örtük akışı kullanabilirsiniz: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read 
&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

URL 'deki sorgu parametreleriyle ilgili ayrıntılar için bkz. [oturum açma isteğini gönderme](#send-the-sign-in-request).

> [!TIP]
> Aşağıdaki isteği bir tarayıcı sekmesine yapıştırmayı & kopyalamayı deneyin! (Değerleri, `login_hint` Kullanıcı için doğru değerle değiştirmeyi unutmayın)
>
>`https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2user.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&login_hint=your-username`
>

`prompt=none` Bu parametre sayesinde, bu istek hemen başarılı veya başarısız olur ve uygulamanıza geri döner. Parametresinde`response_mode` belirtilen yöntem kullanılarak, belirtilen `redirect_uri`şekilde uygulamanıza başarılı bir yanıt gönderilir.

#### <a name="successful-response"></a>Başarılı yanıt

Kullanarak `response_mode=fragment` başarılı bir yanıt şöyle görünür:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` |Dahil ise `response_type` `token`dahildir. Uygulamanın istediği erişim belirteci, Microsoft Graph için bu durumda. Erişim belirtecinin kodu çözülemedi veya bunun için bir donuk dize olarak değerlendirilmemelidir. |
| `token_type` | Her zaman `Bearer`olacak. |
| `expires_in` | Önbelleğe alma amacıyla belirtecin geçerli olduğu saniye sayısını gösterir. |
| `scope` | Access_token geçerli olacak kapsam (ler) i gösterir. Kullanıcı için geçerli olmadıklarında istenen tüm kapsamları içermeyebilir (oturum açmak için bir kişisel hesap kullanıldığında yalnızca Azure AD kapsamları isteniyorsa). |
| `id_token` | İmzalı bir JSON Web Token (JWT). Dahil ise `response_type` `id_token`dahildir. Uygulama, oturum açan kullanıcı hakkında bilgi istemek için bu belirtecin segmentlerinin kodunu çözebilir. Uygulama, değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bu değere güvenmemelidir. İd_tokens hakkında daha fazla bilgi için bkz [ `id_token` . başvuru](id-tokens.md). <br> **Not:** Yalnızca `openid` kapsam isteniyorsa sağlandı. |
| `state` |İsteğe bir durum parametresi dahil edilir, yanıtta aynı değer görünmelidir. Uygulama, istek ve yanıtta durum değerlerinin özdeş olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Ayrıca, `redirect_uri` uygulamanın bunları uygun şekilde işleyebilmesi için hata yanıtları da gönderilebilir. Bu durumda `prompt=none`, beklenen bir hata şu şekilde olur:

```
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

Örtük izin yenileme belirteçleri sağlamıyor. Her `id_token`iki s `access_token`ve s kısa bir süre sonra sona erer, bu nedenle uygulamanızın bu belirteçleri düzenli aralıklarla yenilemeye hazırlanması gerekir. Her iki tür belirteci de yenilemek için, kimlik platformunun davranışını denetlemek üzere `prompt=none` parametresini kullanarak yukarıdaki aynı gizli iframe isteğini de gerçekleştirebilirsiniz. Yeni `id_token`bir almak istiyorsanız, `response_type=id_token` ve `nonce` parametrelerini de ve `scope=openid`parametresini kullandığınızdan emin olun.

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
