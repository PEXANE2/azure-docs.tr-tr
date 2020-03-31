---
title: OpenID Connect & Azure AD ile web uygulaması erişimini yetkilendirme | Microsoft Dokümanlar
description: Bu makalede, Azure Active Directory ve OpenID Connect'i kullanarak kiracınızdaki web uygulamalarına ve web API'lerine erişimi yetkilendirmek için HTTP iletilerinin nasıl kullanılacağı açıklanmaktadır.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: eef8174056be7e6be35cea56788c0a519d02944e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154449"
---
# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>OpenID Connect ve Azure Active Directory kullanarak web uygulamalarına erişim yetkisi verme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

[OpenID Connect,](https://openid.net/specs/openid-connect-core-1_0.html) OAuth 2.0 protokolünün üzerine inşa edilmiş basit bir kimlik katmanıdır. OAuth 2.0, korumalı kaynaklara erişmek için [**erişim belirteçlerini**](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) elde etmek ve kullanmak için mekanizmalar tanımlar, ancak kimlik bilgilerini sağlamak için standart yöntemler tanımlamaz. OpenID Connect, OAuth 2.0 yetkilendirme işleminin uzantısı olarak kimlik doğrulaması uygular. Kullanıcının kimliğini doğrulayan ve kullanıcı [`id_token`](../develop/id-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) hakkında temel profil bilgilerini sağlayan bir biçimde son kullanıcı hakkında bilgi sağlar.

OpenID Connect, sunucuda barındırılan ve tarayıcı üzerinden erişilen bir web uygulaması oluşturuyorsanız bizim önerimizdir.

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

## <a name="authentication-flow-using-openid-connect"></a>OpenID Connect kullanarak kimlik doğrulama akışı

En temel oturum açma akışı aşağıdaki adımları içerir - her biri aşağıda ayrıntılı olarak açıklanmıştır.

![OpenId Connect Kimlik Doğrulama Akışı](./media/v1-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## <a name="openid-connect-metadata-document"></a>OpenID Connect meta veri belgesi

OpenID Connect, bir uygulamanın oturum açma gerçekleştirmesi için gereken bilgilerin çoğunu içeren bir meta veri belgesini açıklar. Bu, kullanılacak URL'ler ve hizmetin genel imzalama anahtarlarının konumu gibi bilgileri içerir. OpenID Connect meta veri belgesini şu anda bulabilirsiniz:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
Meta veriler basit bir JavaScript Nesne Gösterimi (JSON) belgesidir. Bir örnek için aşağıdaki snippet bakın. Parçacık içeriği [OpenID Connect belirtiminde](https://openid.net)tam olarak açıklanmıştır. Yukarıdaki {kiracı} yerine kiracı `common` kimliği sağlamak, JSON nesnesinde kiracıya özgü ÜR'ler ile sonuçlanacaktır.

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/{tenant}/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/{tenant}/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt",
        "client_secret_basic"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    "userinfo_endpoint":"https://login.microsoftonline.com/{tenant}/openid/userinfo",
    ...
}
```

[Uygulamanızın talep eşleme](../develop/active-directory-claims-mapping.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) özelliğini kullanmanın sonucu olarak özel imzalama anahtarları `appid` varsa, uygulamanızın imzalama anahtarı bilgilerini `jwks_uri` işaretlemek için uygulama kimliğini içeren bir sorgu parametresi eklemeniz gerekir. Örneğin: `https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration?appid=6731de76-14a6-49ae-97bc-6eba6914391e` bir `jwks_uri` `https://login.microsoftonline.com/{tenant}/discovery/keys?appid=6731de76-14a6-49ae-97bc-6eba6914391e`içerir.

## <a name="send-the-sign-in-request"></a>Oturum açma isteğini gönderme

Web uygulamanızın kullanıcının kimliğini doğrulaması gerektiğinde, kullanıcıyı `/authorize` bitiş noktasına yönlendirmesi gerekir. Bu istek, birkaç önemli ayrım ile [OAuth 2.0 Yetki Kodu Akışı'nın](v1-protocols-oauth-code.md)ilk ayağına benzer:

* `scope` İstek, kapsamı `openid` parametreye dahil etmelidir.
* `response_type` Parametre. `id_token`
* İstek parametreiçermelidir. `nonce`

Örnek bir istek şuna benzer:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%3a12345
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parametre |  | Açıklama |
| --- | --- | --- |
| Kiracı |gerekli |İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen değerler kiracı tanımlayıcılarıdır, örneğin `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` `common` kiracıdan bağımsız belirteçler için |
| client_id |gerekli |Azure AD'ye kaydettiğinizde uygulamanız için atanan Uygulama Kimliği. Bunu Azure portalında bulabilirsiniz. **Azure Active Directory'yi**tıklatın, **Uygulama Kayıtları'nı**tıklatın, uygulamayı seçin ve uygulama sayfasında Uygulama Kimliğini bulun. |
| response_type |gerekli |OpenID `id_token` Connect oturum açma için şunları içermelidir. Ayrıca diğer response_types, gibi `code` olabilir. `token` |
| scope | Önerilen | OpenID Connect belirtimi, `openid`onay Arabirimi'ndeki "Oturum aç" iznine çevrilen kapsamı gerektirir. Bu ve diğer OIDC kapsamları v1.0 bitiş noktasında yoksayılır, ancak yine de standartlara uyumlu istemciler için en iyi uygulamadır. |
| Nonce |gerekli |Uygulama tarafından oluşturulan isteğe dahil edilen ve bir talep olarak `id_token` ortaya çıkan değere dahil edilir. Uygulama daha sonra belirteç yeniden oynatma saldırılarını azaltmak için bu değeri doğrulayabilir. Değer genellikle isteğin kaynağını tanımlamak için kullanılabilecek randomize, benzersiz bir dize veya GUID'dir. |
| redirect_uri | Önerilen |Uygulamanızın, kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabileceği redirect_uri. Portala kaydettiğiniz redirect_uris tam olarak biriyle eşleşmesi gerekir, ancak url'nin kodlanmış olması gerekir. Eksikse, kullanıcı aracısı uygulama için kayıtlı yönlendirme UUİ'lerinden birine rasgele gönderilir. Maksimum uzunluk 255 bayt |
| response_mode |isteğe bağlı |Elde edilen authorization_code uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. Desteklenen değerler `form_post` *HTTP form gönderisi* ve `fragment` URL *parçası*içindir. Web uygulamaları için, jetonların en güvenli şekilde uygulamanıza aktarılmasını sağlamak için kullanmanızı `response_mode=form_post` öneririz. id_token dahil olmak üzere herhangi `fragment`bir akış için varsayılan.|
| durum |Önerilen |Belirteç yanıtında döndürülen isteğe dahil edilen bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Rasgele oluşturulan benzersiz değer genellikle [çapraz site isteği sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada (örneğin, üzerinde oldukları sayfa veya görünüm) kodlamak için de kullanılır. |
| Istemi |isteğe bağlı |Gerekli kullanıcı etkileşimi türünü gösterir. Şu anda, tek geçerli değerler 'giriş', 'yok' ve 'rıza'dır. `prompt=login`kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlayarak tek oturum açmayı olumsuzlar. `prompt=none`tam tersidir - kullanıcıya herhangi bir etkileşimli istemi nin sunulmamasını sağlar. İstek tek oturum açma yoluyla sessizce tamamlanamıyorsa, bitiş noktası bir hata döndürür. `prompt=consent`kullanıcı oturum açtıktan sonra OAuth onay iletişim kutusunu tetikler ve kullanıcıdan uygulamaya izin vermesini ister. |
| login_hint |isteğe bağlı |Kullanıcı adını önceden biliyorsanız, oturum açma sayfasının kullanıcı adı/e-posta adresi alanını önceden doldurmak için kullanılabilir. Uygulamalar genellikle, talebi kullanarak önceki bir oturum açma dan kullanıcı adını ayıklamış `preferred_username` olan, yeniden kimlik doğrulaması sırasında bu parametreyi kullanır. |

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve kimlik doğrulamasını tamamlaması istenir.

### <a name="sample-response"></a>Örnek yanıt

Kullanıcı kimlik doğrulaması `redirect_uri` yaptıktan sonra oturum açma isteğinde belirtilene gönderilen örnek yanıt aşağıdaki gibi görünebilir:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |Uygulamanın `id_token` istediği. Kullanıcının kimliğini `id_token` doğrulamak ve kullanıcıyla bir oturum başlatmak için kullanabilirsiniz. |
| durum |Belirteç yanıtında döndürülen isteğe dahil edilen bir değer. Rasgele oluşturulan benzersiz değer genellikle [çapraz site isteği sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada (örneğin, üzerinde oldukları sayfa veya görünüm) kodlamak için de kullanılır. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları, uygulamanın `redirect_uri` bunları uygun şekilde işleyebilmeleri için de gönderilebilir:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| error_description |Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

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

## <a name="validate-the-id_token"></a>id_token doğrulayın

Sadece bir `id_token` alma kullanıcının kimliğini doğrulamak için yeterli değildir; imzayı doğrulamanız ve uygulamanızın `id_token` gereksinimlerine göre talepleri doğrulamanız gerekir. Azure AD bitiş noktası, belirteçleri imzalamak ve geçerli olduklarını doğrulamak için JSON Web Belirteçleri (JWT'ler) ve ortak anahtar şifrelemesini kullanır.

İstemci `id_token` kodunu doğrulamayı seçebilirsiniz, ancak yaygın bir `id_token` uygulama arka uç sunucusuna göndermek ve doğrulamayı orada gerçekleştirmektir. 

Ayrıca, senaryonuza bağlı olarak ek talepleri doğrulamak isteyebilirsiniz. Bazı yaygın doğrulamalar şunlardır:

* Kullanıcının/kuruluşun uygulamaya kaydolmasını sağlamak.
* Kullanıcının talepleri `wids` veya `roles` talepleri kullanarak uygun yetkilendirmeye/ayrıcalıklara sahip olmasını sağlamak. 
* Çok faktörlü kimlik doğrulama gibi belirli bir kimlik doğrulama gücünün oluşmasını sağlamak.

Kullanıcıyla `id_token`bir oturum başlatabilir ve uygulamanızdaki kullanıcı hakkında bilgi `id_token` almak için iddiaları kullanabilirsiniz. Bu bilgiler görüntü, kayıt, kişiselleştirme vb. için kullanılabilir. Hakkında `id_tokens` daha fazla bilgi ve iddialar için [AAD id_tokens](../develop/id-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)okuyun.

## <a name="send-a-sign-out-request"></a>Oturum açma isteği gönderme

Kullanıcıyı uygulamadan çıkarmak istediğinizde, uygulamanızın çerezlerini temizlemek veya kullanıcıyla oturumu sona erdirmek yeterli değildir. Ayrıca, kullanıcıyı oturum açma `end_session_endpoint` için yönlendirmeniz gerekir. Bunu yapmazsanız, kullanıcı azure AD bitiş noktasıyla geçerli bir oturum açma oturumu na sahip olacağından, kullanıcı kimlik bilgilerini tekrar girmeden uygulamanızın kimliğini yeniden doğrulayabiliyor.

Kullanıcıyı OpenID Connect meta `end_session_endpoint` veri belgesinde listelenenlere yönlendirebilirsiniz:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametre |  | Açıklama |
| --- | --- | --- |
| post_logout_redirect_uri |Önerilen |Başarılı bir şekilde oturum dışı attıktan sonra kullanıcının yönlendirilmesi gereken URL.  Bu URL, uygulama kayıt portalında uygulamanız için kayıtlı yönlendirme URL'lerinden biriyle eşleşmelidir.  *post_logout_redirect_uri* dahil edilmezse, kullanıcıya genel bir ileti gösterilir. |

## <a name="single-sign-out"></a>Çoklu oturum kapatma

Kullanıcıyı `end_session_endpoint`, Azure AD'ye yönlendirdiğinizde, kullanıcının oturumunu tarayıcıdan temizler. Ancak, kullanıcı kimlik doğrulaması için Azure AD kullanan diğer uygulamalarda oturum açmış olabilir. Azure AD, bu uygulamaların kullanıcıyı aynı anda oturum alabilmesini `LogoutUrl` sağlamak için, kullanıcının şu anda oturum açmış olduğu tüm uygulamaların kayıtlısına bir HTTP GET isteği gönderir. Uygulamalar, kullanıcıyı tanımlayan herhangi bir oturumu temizleyerek ve `200` yanıt döndürerek bu isteğe yanıt vermelidir. Uygulamanızda tek oturum açma yı desteklemek istiyorsanız, uygulamanızın kodunda böyle bir `LogoutUrl` uygulama uygulamanız gerekir. Azure `LogoutUrl` portalından ayarlayabilirsiniz:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sayfanın sağ üst köşesindeki hesabınıza tıklayarak Aktif Dizininizi seçin.
3. Sol daki gezinme panelinden **Azure Active Directory'yi**seçin, ardından **Uygulama kayıtlarını** seçin ve uygulamanızı seçin.
4. **Ayarlar,sonra** **Özellikler'e** tıklayın ve **Giriş URL** metin kutusunu bulun. 

## <a name="token-acquisition"></a>Jeton Edinimi
Birçok web uygulaması nın yalnızca kullanıcıyı oturum alabilmekle birlikte, OAuth'u kullanan kullanıcı adına bir web hizmetine de erişmesi gerekir. Bu senaryo, kullanıcı kimlik doğrulaması için OpenID `authorization_code` Connect'i birleştirirken aynı anda [OAuth Yetkilendirme Kodu Akışını](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) `access_tokens` kullanmak için kullanılabilecek bir tane satın alır.

## <a name="get-access-tokens"></a>Erişim Jetonları Alın
Erişim belirteçleri elde etmek için, oturum açma isteğini yukarıdan değiştirmeniz gerekir:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%3a12345          // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // `form_post' or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F        // The identifier of the protected resource (web API) that your application needs access to
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

İsteke izin kapsamları ekleyerek ve `response_type=code+id_token`kullanarak, `authorize` bitiş noktası kullanıcının `scope` sorgu parametresinde belirtilen izinleri kabul etmesini sağlar ve uygulamanıza bir erişim jetonu değişimi için bir yetkilendirme kodu döndürür.

### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir yanıt, `redirect_uri` kullanarak `response_mode=form_post`gönderilen , gibi görünüyor:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| id_token |Uygulamanın `id_token` istediği. Kullanıcının kimliğini `id_token` doğrulamak ve kullanıcıyla bir oturum başlatmak için kullanabilirsiniz. |
| kod |Uygulamanın istediği authorization_code. Uygulama, hedef kaynak için bir erişim jetonu istemek için yetkilendirme kodunu kullanabilir. Authorization_codes kısa ömürlüdür ve genellikle yaklaşık 10 dakika sonra sona erer. |
| durum |İsteğe bir durum parametresi dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve yanıttaki durum değerlerinin aynı olduğunu doğrulamalıdır. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları, uygulamanın `redirect_uri` bunları uygun şekilde işleyebilmeleri için de gönderilebilir:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| error_description |Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

Olası hata kodlarının ve önerilen istemci eyleminin açıklaması [için, yetkilendirme bitiş noktası hataları için hata kodlarına](#error-codes-for-authorization-endpoint-errors)bakın.

Bir yetkilendirme `code` ve bir `id_token`, kullanıcı oturum açabilir ve onlar adına erişim [jetonları](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) almak aldıktan sonra. Kullanıcıyı oturum alabilmek için, yukarıda açıklandığı `id_token` gibi tam olarak doğrulamanız gerekir. Erişim belirteçleri almak için, [OAuth kod akış belgelerimizin](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token)"Erişim jetonu istemek için yetkilendirme kodunu kullanın" bölümünde açıklanan adımları izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Erişim belirteçleri](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)hakkında daha fazla bilgi edinin.
* [ `id_token` İddialar ve iddialar](../develop/id-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)hakkında daha fazla bilgi edinin.
