---
title: Azure AD'deki OAuth 2.0 yetkilendirme kodu akışını anlama
description: Bu makalede, Azure Active Directory ve OAuth 2.0'ı kullanarak kiracınızdaki web uygulamalarına ve web API'lerine erişimi yetkilendirmek için HTTP iletilerinin nasıl kullanılacağı açıklanmaktadır.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 3ec7cf5a45ce31cde923dce521636589cfcda786
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154466"
---
# <a name="authorize-access-to-azure-active-directory-web-applications-using-the-oauth-20-code-grant-flow"></a>OAuth 2.0 kod verme akışını kullanarak Azure Active Directory web uygulamalarına erişimi yetkilendirme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

> [!NOTE]
>  Sunucuya hangi kaynağı aramayı planladığınızı söylemezseniz, sunucu bu kaynak için Koşullu Erişim ilkelerini tetiklemez. Bu nedenle, MFA tetikleyicisi olması için URL'nize bir kaynak eklemeniz gerekir. 
>

Azure Active Directory (Azure AD), OAuth 2.0’ı kullanarak Azure AD kiracınızdaki web uygulamalarına ve web API’lerine erişim yetkisi vermenize olanak tanır. Bu kılavuz dilden bağımsızdır ve [açık kaynak kitaplıklarımızdan](active-directory-authentication-libraries.md)herhangi birini kullanmadan HTTP iletilerinin nasıl gönderilip alınılacağını açıklar.

OAuth 2.0 yetkilendirme kodu akışı, [OAuth 2.0 belirtiminin 4.1 bölümünde](https://tools.ietf.org/html/rfc6749#section-4.1)açıklanmıştır. Web uygulamaları ve yerel olarak yüklenmiş uygulamalar da dahil olmak üzere çoğu uygulama türünde kimlik doğrulama ve yetkilendirme gerçekleştirmek için kullanılır.

## <a name="register-your-application-with-your-ad-tenant"></a>Uygulamanızı AD kiracınıza kaydetme
Öncelikle, uygulamanızı Azure Etkin Dizin (Azure AD) kiracınıza kaydedin. Bu, uygulamanıza bir Uygulama Kimliği verir ve uygulamanızın belirteçleri alabilmesini sağlar.

1. [Azure portalında](https://portal.azure.com)oturum açın.
   
1. Sayfanın sağ üst köşesinde hesabınızı seçerek Azure AD kiracınızı seçin, ardından **Dizin Değiştir** gezintisini seçin ve ardından uygun kiracıyı seçin. 
   - Hesabınızın altında yalnızca bir Azure AD kiracınız varsa veya uygun Azure AD kiracısını zaten seçtiyseniz bu adımı atlayın.
   
1. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.
   
1. Azure **Etkin Dizin** ilerki menüsünde **Uygulama Kayıtları'nı**seçin ve ardından **Yeni Kayıt'ı**seçin.
   
1. Komut istemlerini izleyin ve yeni bir uygulama oluşturun. Bu öğretici için bir web uygulaması veya bir kamu istemcisi (mobil & masaüstü) uygulaması olup olmadığını fark etmez, ancak web uygulamaları veya kamu istemci uygulamaları için özel örnekler istiyorsanız, bizim [quickstarts](v1-overview.md)göz atın.
   
   - **Ad**, uygulamanın adıdır ve uygulamanızı son kullanıcılara açıklar.
   - **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin.
   - Yeniden **Yönlendirme URI**sağlayın. Web uygulamaları için bu, uygulamanızın kullanıcıların oturum açabileceği temel URL'sidir.  Örneğin, `http://localhost:12345`. Azure AD, ortak istemci (mobil & masaüstü) için, bu yanıtı belirteç yanıtlarını döndürmek için kullanır. Uygulamanıza özgü bir değer girin.  Örneğin, `http://MyFirstAADApp`.
   <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->  
   
1. Kaydı tamamladıktan sonra Azure AD uygulamanıza benzersiz bir istemci tanımlayıcısı **(Uygulama Kimliği)** atar. Sonraki bölümlerde bu değere ihtiyacınız var, bu nedenle uygulama sayfasından kopyalayın.
   
1. Uygulamanızı Azure portalında bulmak için **Uygulama kayıtlarını**seçin ve ardından tüm **uygulamaları Görüntüle'yi**seçin.

## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 yetkilendirme akışı

Yüksek düzeyde, bir uygulama için tüm yetkilendirme akışı biraz şuna benzer:

![OAuth Auth Kod Akışı](./media/v1-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## <a name="request-an-authorization-code"></a>Yetkilendirme kodu isteme

Yetkilendirme kodu akışı, istemcinin kullanıcıyı `/authorize` bitiş noktasına yönlendirmesiyle başlar. Bu istekte, istemci kullanıcıdan alması gereken izinleri gösterir. Azure portalında **Uygulama kayıtlarını > Bitiş Noktaları'nı** seçerek kiracınız için OAuth 2.0 yetkilendirme bitiş noktasını alabilirsiniz.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%3A12345
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |gerekli |İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen değerler kiracı tanımlayıcılarıdır, örneğin `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` `common` kiracıdan bağımsız belirteçler için |
| client_id |gerekli |Azure AD'ye kaydettiğinizde uygulamanız için atanan Uygulama Kimliği. Bunu Azure Portalı'nda bulabilirsiniz. Hizmetlerin kenar çubuğunda **Azure Etkin Dizini'ni** tıklatın, Uygulama **kayıtlarını**tıklatın ve uygulamayı seçin. |
| response_type |gerekli |Yetkilendirme `code` kodu akışı için içermelidir. |
| redirect_uri |Önerilen |Uygulamanızın, kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabileceği redirect_uri. Portala kaydettiğiniz redirect_uris tam olarak biriyle eşleşmesi gerekir, ancak url'nin kodlanmış olması gerekir. Yerel & mobil uygulamalar için varsayılan değeri `https://login.microsoftonline.com/common/oauth2/nativeclient`kullanmalısınız. |
| response_mode |isteğe bağlı |Ortaya çıkan belirteci uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. Olabilir `query`, `fragment`veya `form_post`. `query`kodu, yeniden yönlendirme URI'nizde sorgu dize parametresi olarak sağlar. Örtük akışı kullanarak bir kimlik belirteci talep ediyorsanız, `query` [OpenID spec'te](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations)belirtildiği gibi kullanamazsınız. Sadece kodu talep ediyorsanız, `query`, `fragment`veya `form_post`. `form_post`kodu içeren bir POST'u yeniden yönlendirme URI'nize yürütür. Varsayılan değer `query` bir kod akışı içindir.  |
| durum |Önerilen |Belirteç yanıtında döndürülen isteğe dahil edilen bir değer. Rasgele oluşturulan benzersiz değer genellikle [çapraz site isteği sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada (örneğin, üzerinde oldukları sayfa veya görünüm) kodlamak için de kullanılır. |
| kaynak | Önerilen |Hedef web API'nin App ID URI 'si (güvenli kaynak). Azure Portalı'nda App ID URI'yi bulmak için **Azure Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, uygulamanın **Ayarlar** sayfasını açın ve **ardından Özellikler'i**tıklatın. Ayrıca gibi `https://graph.microsoft.com`bir dış kaynak olabilir. Bu, yetkilendirme veya belirteç isteklerinden birinde gereklidir. Daha az kimlik doğrulama istemi sağlamak için, kullanıcıdan onay alınmasını sağlamak için yetkilendirme isteğine yerleştirin. |
| scope | **Göz ardı** | v1 Azure AD uygulamaları için kapsamların Azure Portalı'nda Uygulama **Ayarları**, **Gerekli İzinler**altında statik olarak yapılandırılması gerekir. |
| Istemi |isteğe bağlı |Gerekli kullanıcı etkileşimi türünü belirtin.<p> Geçerli değerler: <p> *giriş*: Kullanıcıdan yeniden kimlik doğrulaması istenmelidir. <p> *select_account*: Kullanıcıdan tek bir oturum açarak bir hesap seçmesi istenir. Kullanıcı varolan imzalı bir hesabı seçebilir, hatırlanan bir hesap için kimlik bilgilerini girebilir veya tamamen farklı bir hesap kullanmayı seçebilir. <p> *onam*: Kullanıcı onayı verilmiş, ancak güncellenmesi gerekmektedir. Kullanıcıdan onay istenmesi istenir. <p> *admin_consent*: Bir yöneticinin, kuruluşlarındaki tüm kullanıcılar adına onay vermesi istanmalıdır |
| login_hint |isteğe bağlı |Kullanıcı adını önceden biliyorsanız, oturum açma sayfasının kullanıcı adı/e-posta adresi alanını önceden doldurmak için kullanılabilir. Uygulamalar genellikle, talebi kullanarak önceki bir oturum açma dan kullanıcı adını ayıklamış `preferred_username` olan, yeniden kimlik doğrulaması sırasında bu parametreyi kullanır. |
| domain_hint |isteğe bağlı |Kullanıcının oturum açmada kullanması gereken kiracı veya etki alanı hakkında ipucu sağlar. domain_hint değeri kiracı için kayıtlı bir etki alanıdır. Kiracı şirket içi bir dizine federe ise, AAD belirtilen kiracı federasyon sunucusuna yönlendirir. |
| code_challenge_method | Önerilen    | `code_challenge` Parametreyi `code_verifier` kodlamak için kullanılan yöntem. Biri olabilir `plain` ya `S256`da . Dışlanırsa, `code_challenge` dahil edilirse düz metin `code_challenge` olarak kabul edilir. Azure AAD v1.0 `plain` `S256`hem destekler hem de. Daha fazla bilgi için [PKCE RFC'ye](https://tools.ietf.org/html/rfc7636)bakın. |
| code_challenge        | Önerilen    | Yerel veya kamu istemcisinden Proof Key for Code Exchange (PKCE) aracılığıyla yetkilendirme kodu hibelerini güvence altına almak için kullanılır. Dahil `code_challenge_method` edilirse gereklidir. Daha fazla bilgi için [PKCE RFC'ye](https://tools.ietf.org/html/rfc7636)bakın. |

> [!NOTE]
> Kullanıcı bir kuruluşun parçasıysa, kuruluşun bir yöneticisi kullanıcı adına izin verebilir veya reddedebilir veya kullanıcının onayına izin verebilir. Kullanıcıya yalnızca yönetici izin verdiğinde onay verme seçeneği verilir.
>
>

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve Azure Portalı'nda uygulama tarafından istenen izinleri kabul etmesi istenir. Kullanıcı kimlik doğrulaması yaptıktan ve onay verdikten sonra, Azure `redirect_uri` AD uygulamanıza isteğinizdeki adrese kodla birlikte bir yanıt gönderir.

### <a name="successful-response"></a>Başarılı yanıt
Başarılı bir yanıt şu şekilde görünebilir:

```
GET  HTTP/1.1 302 Found
Location: http://localhost:12345/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametre | Açıklama |
| --- | --- |
| admin_consent |Bir yönetici bir onay isteği istemini kabul ettiyse, değer Doğrudur. |
| kod |Uygulamanın istediği yetkilendirme kodu. Uygulama, hedef kaynak için bir erişim belirteci istemek için yetkilendirme kodunu kullanabilir. |
| session_state |Geçerli kullanıcı oturumunu tanımlayan benzersiz bir değer. Bu değer bir GUID'dir, ancak muayene edilmeden geçirilen opak bir değer olarak ele alınmalıdır. |
| durum |İsteğe bir durum parametresi dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Yanıtı kullanmadan önce istek ve yanıttaki durum değerlerinin aynı olduğunu doğrulamak için uygulama iyi bir uygulamadır. Bu, istemciye karşı [Site ler arası Istek Sahteciliği (CSRF) saldırılarını](https://tools.ietf.org/html/rfc6749#section-10.12) algılamaya yardımcı olur. |

### <a name="error-response"></a>Hata yanıtı
Hata yanıtları, uygulamanın `redirect_uri` bunları uygun şekilde işleyebileceği şekilde de gönderilebilir.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |[OAuth 2.0 Yetkilendirme Çerçevesi'nin](https://tools.ietf.org/html/rfc6749)Bölüm 5.2'sinde tanımlanan bir hata kodu değeri. Sonraki tabloda Azure AD'nin döndürdettiği hata kodları açıklanmaktadır. |
| error_description |Hatanın daha ayrıntılı bir açıklaması. Bu ileti son kullanıcı dostu olması amaçlanmamıştır. |
| durum |Durum değeri, istekte gönderilen ve siteler arası istek sahteciliği (CSRF) saldırılarını önlemek için yanıtolarak döndürülen rasgele oluşturulan yeniden kullanılmayan bir değerdir. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme bitiş noktası hataları için hata kodları
Aşağıdaki tabloda hata yanıtı `error` parametresi döndürülebilir çeşitli hata kodları açıklanır.

| Hata Kodu | Açıklama | İstemci Eylemi |
| --- | --- | --- |
| invalid_request |Eksik gerekli parametre gibi protokol hatası. |İsteği düzeltin ve yeniden gönderin. Bu bir geliştirme hatasıdır ve genellikle ilk sınama sırasında yakalanır. |
| unauthorized_client |İstemci uygulamasının yetkilendirme kodu istemesine izin verilmez. |Bu genellikle istemci uygulaması Azure AD'de kayıtlı olmadığında veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| access_denied |Kaynak sahibi nin rızası reddedildi |İstemci uygulaması, kullanıcı izin vermediği sürece devam edemeyeceğini kullanıcıya bildirebilir. |
| unsupported_response_type |Yetkilendirme sunucusu istekteki yanıt türünü desteklemez. |İsteği düzeltin ve yeniden gönderin. Bu bir geliştirme hatasıdır ve genellikle ilk sınama sırasında yakalanır. |
| server_error |Sunucu beklenmeyen bir hatayla karşılaştı. |İsteği yeniden deneyin. Bu hatalar geçici koşullardan kaynaklanabilir. İstemci uygulaması kullanıcıya yanıtının geçici bir hata nedeniyle geciktiğini açıklayabilir. |
| temporarily_unavailable |Sunucu geçici olarak isteği işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması kullanıcıya yanıtının geçici bir durum nedeniyle geciktiğini açıklayabilir. |
| invalid_resource |Hedef kaynak yok, Azure AD bulamadığı veya doğru şekilde yapılandırılamadığı için geçersizdir. |Bu, varsa kaynağın kiracıda yapılandırılmadığını gösterir. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Erişim jetonu istemek için yetkilendirme kodunu kullanma
Artık bir yetkilendirme kodu edindiğinize ve kullanıcı tarafından izin aldığınıza göre, kodu bitiş noktasına bir POST isteği `/token` göndererek istenen kaynağa erişim belirteci için kullanabilirsiniz:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%3A12345
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |gerekli |İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen değerler kiracı tanımlayıcılarıdır, örneğin `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` `common` kiracıdan bağımsız belirteçler için |
| client_id |gerekli |Azure AD'ye kaydettiğinizde uygulamanız için atanan Uygulama Kimliği. Bunu Azure portalında bulabilirsiniz. Uygulama Kimliği, uygulama kaydı ayarlarında görüntülenir. |
| grant_type |gerekli |Yetkilendirme `authorization_code` kodu akışı için olmalıdır. |
| kod |gerekli |`authorization_code` Önceki bölümde edindiğiniz |
| redirect_uri |gerekli | Müşteri `redirect_uri`başvurusunda kayıtlı bir kişi. |
| client_secret |web uygulamaları için gerekli, kamu istemcileri için izin verilmez |**Keys**altında uygulamanız için Azure Portalı'nda oluşturduğunuz uygulama sırrı. client_secrets cihazlarda güvenilir bir şekilde depolanamayacağından, yerel bir uygulamada (ortak istemci) kullanılamaz. Bu web uygulamaları ve web API'lar (tüm gizli istemciler), sunucu tarafında `client_secret` güvenli bir şekilde saklamak için yeteneğine sahip için gereklidir. client_secret gönderilmeden önce URL kodlanmış olmalıdır. |
| kaynak | Önerilen |Hedef web API'nin App ID URI 'si (güvenli kaynak). Azure Portalı'nda App ID URI'yi bulmak için **Azure Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, uygulamanın **Ayarlar** sayfasını açın ve **ardından Özellikler'i**tıklatın. Ayrıca gibi `https://graph.microsoft.com`bir dış kaynak olabilir. Bu, yetkilendirme veya belirteç isteklerinden birinde gereklidir. Daha az kimlik doğrulama istemi sağlamak için, kullanıcıdan onay alınmasını sağlamak için yetkilendirme isteğine yerleştirin. Hem yetkilendirme isteğihem de belirteç isteğinde kaynak parametreleri eşleşmelidir. | 
| code_verifier | isteğe bağlı | Authorization_code elde etmek için kullanılan aynı code_verifier. Yetki kodu hibe isteğinde PKCE kullanılıyorsa gereklidir. Daha fazla bilgi için [PKCE RFC'ye](https://tools.ietf.org/html/rfc7636) bakın   |

Azure Portalı'nda App ID URI'yi bulmak için **Azure Active Directory'yi**tıklatın, **Uygulama kayıtlarını**tıklatın, uygulamanın **Ayarlar** sayfasını açın ve **ardından Özellikler'i**tıklatın.

### <a name="successful-response"></a>Başarılı yanıt
Azure AD, başarılı bir yanıt üzerine bir [erişim jetonu](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) döndürür. İstemci uygulamasından gelen ağ çağrılarını ve ilişkili gecikme sürelerini en aza indirmek için, istemci uygulaması oauth 2.0 yanıtında belirtilen belirteç ömrü için erişim belirteçlerini önbelleğe almalıdır. Belirteç ömrünü belirlemek için, `expires_in` `expires_on` ya veya parametre değerlerini kullanın.

Bir web API kaynağı `invalid_token` bir hata kodu döndürürse, bu, kaynağın belirteç süresinin dolduğunu belirlediğini gösterebilir. İstemci ve kaynak saat süreleri farklıysa ("zaman eğriliği" olarak bilinir), kaynak belirteç istemci önbelleğinden temizlenmeden önce belirteci süresinin dolmasını düşünebilir. Bu durumda, hesaplanmış ömrü içinde olsa bile belirteci önbellekten temizleyin.

Başarılı bir yanıt şu şekilde görünebilir:

```
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
  "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ."
}

```

| Parametre | Açıklama |
| --- | --- |
| access_token |İstenen erişim jetonu.  Bu opak bir dizedir - kaynağın ne almayı beklediğine bağlıdır ve istemcinin bakması için tasarlanmamıştır. Uygulama, web API gibi güvenli kaynağa kimlik doğrulamak için bu belirteci kullanabilir. |
| token_type |Belirteç türü değerini gösterir. Azure AD'nin desteklediği tek tür Taşıyıcı'dır. Taşıyıcı belirteçleri hakkında daha fazla bilgi için Bkz. [OAuth2.0 Yetkilendirme Çerçevesi: Taşıyıcı Belirteç Kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Erişim belirteci nin ne kadar süre geçerli olduğu (saniye cinsinden). |
| expires_on |Erişim belirteci süresinin dolduğu saat. Tarih, 1970-01-01T0:0:0Z UTC'den son kullanma tarihine kadar olan saniye sayısı olarak temsil edilir. Bu değer, önbelleğe alınmış belirteçlerin kullanım ömrünü belirlemek için kullanılır. |
| kaynak |Web API'nin App Id URI 'si (güvenli kaynak). |
| scope |İstemci uygulamasına verilen kimlik kullanma izinleri. Varsayılan izin. `user_impersonation` Güvenli kaynağın sahibi Azure AD'de ek değerler kaydedebilir. |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama, geçerli erişim belirteci süresi dolduktan sonra ek erişim belirteçleri elde etmek için bu belirteci kullanabilirsiniz. Yenileme belirteçleri uzun ömürlüdür ve kaynaklara erişimi uzun süre tutmak için kullanılabilir. |
| id_token |[Kimlik belirteci](../develop/id-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)temsil eden imzasız bir JSON Web Belirteci (JWT). Uygulama, oturum imzalayan kullanıcı hakkında bilgi istemek için bu belirtecinin segmentlerini 64Url'yi kodlayabilir. Uygulama değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bunlara güvenmemelidir. |

JSON web belirteçleri hakkında daha fazla bilgi [için, JWT IETF taslak belirtimi](https://go.microsoft.com/fwlink/?LinkId=392344)bakın.   Hakkında `id_tokens`daha fazla bilgi edinmek için [v1.0 OpenID Connect akışına](v1-protocols-openid-connect-code.md)bakın.

### <a name="error-response"></a>Hata yanıtı
Belirteç verme bitiş noktası hataları HTTP hata kodlarıdır, çünkü istemci belirteç verme bitiş noktasını doğrudan çağırır. HTTP durum koduna ek olarak, Azure AD belirteci verme bitiş noktası hatayı açıklayan nesneleriçeren bir JSON belgesini de döndürür.

Örnek bir hata yanıtı şu şekilde görünebilir:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| error_description |Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılamada yardımcı olabilecek STS'ye özgü hata kodlarının listesi. |
| timestamp |Hatanın oluştuğu saat. |
| trace_id |Tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| correlation_id |Bileşenler arasında tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |

#### <a name="http-status-codes"></a>HTTP durum kodu
Aşağıdaki tabloda belirteç verme bitiş noktasının döndürdettiği HTTP durum kodları listelenir. Bazı durumlarda, hata kodu yanıtı açıklamak için yeterlidir, ancak hatalar varsa, eşlik eden JSON belgeyi ayrıştırmak ve hata kodunu incelemek gerekir.

| HTTP Kodu | Açıklama |
| --- | --- |
| 400 |Varsayılan HTTP kodu. Çoğu durumda kullanılır ve genellikle yanlış biçimlendirilmiş bir istek nedeniyle. İsteği düzeltin ve yeniden gönderin. |
| 401 |Kimlik doğrulaması gerçekleştirilemedi. Örneğin, istek client_secret parametresi eksik. |
| 403 |Yetkilendirme başarısız oldu. Örneğin, kullanıcının kaynağa erişim izni yoktur. |
| 500 |Hizmette bir iç hata oluştu. İsteği yeniden deneyin. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Belirteç bitiş noktası hataları için hata kodları
| Hata Kodu | Açıklama | İstemci Eylemi |
| --- | --- | --- |
| invalid_request |Eksik gerekli parametre gibi protokol hatası. |İsteği düzeltme ve yeniden gönderme |
| invalid_grant |Yetkilendirme kodu geçersiz veya süresi dolmuş. |`/authorize` Bitiş noktasına yeni bir istek deneyin |
| unauthorized_client |Kimlik doğrulaması yapılan istemci, bu yetkilendirme hibe türünü kullanma yetkisine izin vermez. |Bu genellikle istemci uygulaması Azure AD'de kayıtlı olmadığında veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| invalid_client |İstem kimlik doğrulaması başarısız oldu. |İstemci kimlik bilgileri geçerli değildir. Düzeltmek için, uygulama yöneticisi kimlik bilgilerini güncelleştirir. |
| unsupported_grant_type |Yetkilendirme sunucusu yetkilendirme hibe türünü desteklemez. |İstekteki hibe türünü değiştirin. Bu tür hatalar yalnızca geliştirme sırasında ortaya çıkmalıdır ve ilk sınama sırasında algılanmalıdır. |
| invalid_resource |Hedef kaynak yok, Azure AD bulamadığı veya doğru şekilde yapılandırılamadığı için geçersizdir. |Bu, varsa kaynağın kiracıda yapılandırılmadığını gösterir. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| interaction_required |İstek kullanıcı etkileşimi gerektirir. Örneğin, ek bir kimlik doğrulama adımı gereklidir. | Etkileşimli olmayan bir istek yerine, aynı kaynak için etkileşimli bir yetkilendirme isteğiyle yeniden deneyin. |
| temporarily_unavailable |Sunucu geçici olarak isteği işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması kullanıcıya yanıtının geçici bir durum nedeniyle geciktiğini açıklayabilir. |

## <a name="use-the-access-token-to-access-the-resource"></a>Kaynağa erişmek için erişim belirteci kullanma
Artık başarılı bir şekilde bir `access_token`, web API'lerine gelen isteklerde belirteci üstbilgiye `Authorization` ekleyerek kullanabilirsiniz. [RFC 6750](https://www.rfc-editor.org/rfc/rfc6750.txt) belirtimi, korumalı kaynaklara erişmek için HTTP isteklerinde taşıyıcı belirteçlerinin nasıl kullanılacağını açıklar.

### <a name="sample-request"></a>Örnek istek
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Hata Yanıtı
RFC 6750 sorunu HTTP durum kodlarını uygulayan güvenli kaynaklar. İstek kimlik doğrulama kimlik bilgilerini içermiyorsa veya belirteç `WWW-Authenticate` eksikse, yanıt bir üstbilgi içerir. Bir istek başarısız olduğunda, kaynak sunucusu HTTP durum kodu ve bir hata kodu yla yanıt verir.

İstemci isteği taşıyıcı belirteci içermiyorsa başarısız bir yanıt örneği aşağıda verilmiştir:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Hata parametreleri
| Parametre | Açıklama |
| --- | --- |
| authorization_uri |Yetkilendirme sunucusunun URI (fiziksel bitiş noktası). Bu değer, bir bulma bitiş noktasından sunucu hakkında daha fazla bilgi almak için bir arama anahtarı olarak da kullanılır. <p><p> İstemci, yetkilendirme sunucusunun güvenilir olduğunu doğrulamalıdır. Kaynak Azure AD tarafından korunduğunda, URL'nin Azure `https://login.microsoftonline.com` AD'nin desteklediği başka bir ana bilgisayar adı yla veya başlangıçlı olduğunu doğrulamak yeterlidir. Kiracıya özgü bir kaynak her zaman kiracıya özgü bir yetkilendirme URI'yi döndürmelidir. |
| error |[OAuth 2.0 Yetkilendirme Çerçevesi'nin](https://tools.ietf.org/html/rfc6749)Bölüm 5.2'sinde tanımlanan bir hata kodu değeri. |
| error_description |Hatanın daha ayrıntılı bir açıklaması. Bu ileti son kullanıcı dostu olması amaçlanmamıştır. |
| resource_id |Kaynağın benzersiz tanımlayıcısını döndürür. İstemci uygulaması, kaynak için bir belirteç `resource` istediğinde bu tanımlayıcıyı parametrenin değeri olarak kullanabilir. <p><p> İstemci uygulamasının bu değeri doğrulaması önemlidir, aksi takdirde kötü amaçlı bir hizmet **ayrıcalıkların yükselmesine** neden olabilir <p><p> Bir saldırıyı önlemek için önerilen strateji, `resource_id` erişim eken web API URL'sinin tabanıyla eşleştiğini doğrulamaktır. Örneğin, `https://service.contoso.com/data` erişiliyorsa, `resource_id` . `https://service.contoso.com/` İstemci uygulaması, `resource_id` kimliği doğrulamak için güvenilir bir alternatif yol olmadığı sürece temel URL ile başlamayan bir uygulamayı reddetmelidir. |

#### <a name="bearer-scheme-error-codes"></a>Taşıyıcı düzeni hata kodları
RFC 6750 belirtimi, yanıtta WWW-Authenticate üstbilgisini ve Taşıyıcı düzenini kullanan kaynaklar için aşağıdaki hataları tanımlar.

| HTTP Durum Kodu | Hata Kodu | Açıklama | İstemci Eylemi |
| --- | --- | --- | --- |
| 400 |invalid_request |İstek iyi biçimlendirilmiş değil. Örneğin, bir parametre eksik veya aynı parametreyi iki kez kullanıyor olabilir. |Hatayı düzeltin ve isteği yeniden deneyin. Bu tür hatalar yalnızca geliştirme sırasında ortaya çıkmalıdır ve ilk sınamada algılanmalıdır. |
| 401 |invalid_token |Erişim belirteci eksik, geçersiz veya iptal edildi. error_description parametrenin değeri ek ayrıntı sağlar. |Yetkilendirme sunucusundan yeni bir belirteç isteyin. Yeni belirteç başarısız olursa, beklenmeyen bir hata oluştu. Kullanıcıya bir hata iletisi gönderin ve rasgele gecikmelerden sonra yeniden deneyin. |
| 403 |insufficient_scope |Erişim belirteci, kaynağa erişmek için gereken kimliğe bürünme izinlerini içermez. |Yetkilendirme bitiş noktasına yeni bir yetkilendirme isteği gönderin. Yanıt kapsam parametresi içeriyorsa, kaynak isteğindeki kapsam değerini kullanın. |
| 403 |insufficient_access |Belirteci konusu kaynağa erişmek için gerekli izinlere sahip değildir. |Kullanıcıdan farklı bir hesap kullanmasını veya belirtilen kaynağa izin istemesini isteyin. |

## <a name="refreshing-the-access-tokens"></a>Erişim belirteçlerini yenileme

Erişim Belirteçleri kısa ömürlüdür ve kaynaklara erişmeye devam etmek için süresi dolduktan sonra yenilenmelidir. `/token` Bitiş noktasına `access_token` başka bir `POST` istek göndererek yenileyebilirsiniz, ancak bu `refresh_token` kez `code`yerine sağlayan .  Yenileme belirteçleri, istemcinize erişim izni verilmiş olan tüm kaynaklar için geçerlidir - bu `resource=https://graph.microsoft.com` nedenle, bir istek üzerine verilen `resource=https://contoso.com/api`bir yenileme belirteci için yeni bir erişim belirteci istemek için kullanılabilir. 

Belirteçleri yenilemenin belirli kullanım ömürleri yoktur. Genellikle, yenileme belirteçleri yaşam sürelerini nispeten uzun. Ancak, bazı durumlarda, yenileme belirteçleri sona erer, iptal edilir veya istenen eylem için yeterli ayrıcalıkları yoksun. Uygulamanızın belirteç verme bitiş noktası tarafından döndürülen hataları doğru şekilde beklemesi ve işlemesi gerekir.

Yenileme belirteci hatası olan bir yanıt aldığınızda, geçerli yenileme belirtecisini atın ve yeni bir yetkilendirme kodu veya erişim belirteci isteyin. Özellikle, Yetkilendirme Kodu Hibe akışında bir yenileme belirteci kullanırken, `interaction_required` `invalid_grant` hata kodlarıyla ilgili bir yanıt alırsanız, yenileme belirtecinden atın ve yeni bir yetkilendirme kodu isteyin.

Yenileme belirteci kullanarak yeni bir erişim belirteci almak için **kiracıya özgü** bitiş noktasına örnek bir istek **(ortak** bitiş noktasını da kullanabilirsiniz) aşağıdaki gibi görünür:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

### <a name="successful-response"></a>Başarılı yanıt
Başarılı bir belirteç yanıtı gibi görünecektir:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```
| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü. Desteklenen tek değer **taşıyıcıdır.** |
| expires_in |Saniye cinsinden belirteç kalan ömrü. Tipik bir değer 3600 (bir saat) olduğunu. |
| expires_on |Belirteci sona erdirdiği tarih ve saat. Tarih, 1970-01-01T0:0:0Z UTC'den son kullanma tarihine kadar olan saniye sayısı olarak temsil edilir. |
| kaynak |Erişim belirteci erişim için kullanılabilecek güvenli kaynak tanımlar. |
| scope |Yerel istemci uygulamasına verilen kimliğe bürünme izinleri. Varsayılan izin **user_impersonation.** Hedef kaynağın sahibi Azure AD'de alternatif değerler kaydedebilir. |
| access_token |İstenilen yeni erişim belirteci. |
| refresh_token |Bu yanıttaki nin süresi dolduğunda yeni erişim belirteçleri istemek için kullanılabilecek yeni bir OAuth 2.0 refresh_token. |

### <a name="error-response"></a>Hata yanıtı
Örnek bir hata yanıtı şu şekilde görünebilir:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant. You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| error_description |Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |
| error_codes |Tanılamada yardımcı olabilecek STS'ye özgü hata kodlarının listesi. |
| timestamp |Hatanın oluştuğu saat. |
| trace_id |Tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| correlation_id |Bileşenler arasında tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |

Hata kodlarının ve önerilen istemci eyleminin açıklaması [için belirteç bitiş noktası hataları için hata kodlarına](#error-codes-for-token-endpoint-errors)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD v1.0 bitiş noktası ve web uygulamalarınız ve web API'larınıza kimlik doğrulama ve yetkilendirme ekleme hakkında daha fazla bilgi edinmek için [örnek uygulamalara](sample-v1-code.md)bakın.
