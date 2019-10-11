---
title: Azure Active Directory kimlik doğrulaması ve Kaynak Yöneticisi | Microsoft Docs
description: Geliştirici Azure Resource Manager API ile kimlik doğrulama ve bir uygulamayı diğer Azure abonelikleri ile tümleştirmek için Azure Active Directory.
author: dushyantgill
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: dugill
ms.openlocfilehash: 033f3ca9ca79903f884c625dc694b06a3e4fd04c
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72263016"
---
# <a name="use-resource-manager-authentication-api-to-access-subscriptions"></a>Aboneliklere erişmek için Kaynak Yöneticisi kimlik doğrulama API 'SI kullanma

Müşterinin Azure kaynaklarını yöneten bir uygulama oluşturması gereken bir yazılım geliştiricisiyseniz, bu makalede Azure Resource Manager API 'Leriyle nasıl kimlik doğrulaması yapılacağı ve diğer aboneliklerdeki kaynaklara erişim elde etme konusu gösterilmektedir.

Uygulamanız Kaynak Yöneticisi API 'Lerine birkaç yolla erişebilir:

1. **Kullanıcı + uygulama erişimi**: oturum açmış bir kullanıcının kaynaklarına erişen uygulamalar için. Bu yaklaşım, Azure kaynaklarından yalnızca "Etkileşimli Yönetim" ile ilgilenen Web uygulamaları ve komut satırı araçları gibi uygulamalar için geçerlidir.
2. **Yalnızca uygulama erişimi**: Daemon Hizmetleri ve zamanlanan işleri çalıştıran uygulamalar için. Uygulamanın kimliğine, kaynaklara doğrudan erişim izni verilir. Bu yaklaşım, Azure 'a uzun süreli gözetimsiz (katılımsız) erişim gerektiren uygulamalar için geçerlidir.

Bu makalede, bu yetkilendirme yöntemlerini kullanan bir uygulama oluşturmak için adım adım yönergeler sağlanmaktadır. REST API veya C#ile her adımın nasıl yapılacağını gösterir. Tam ASP.NET MVC uygulaması [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense)' de mevcuttur.

## <a name="what-the-web-app-does"></a>Web uygulaması ne yapar

Web uygulaması:

1. Bir Azure kullanıcısı oturum açar.
2. Kullanıcıdan Web uygulamasına Kaynak Yöneticisi erişimi vermesini ister.
3. Kaynak Yöneticisi erişmek için Kullanıcı + uygulama erişim belirtecini alır.
4. Uygulamanın hizmet sorumlusunu abonelikte bir role atamak için belirteci (adım 3 ' ten) kullanır. Bu adım, uygulamaya abonelik için uzun süreli erişim sağlar.
5. Yalnızca uygulama erişim belirtecini alır.
6. Kaynak Yöneticisi aracılığıyla abonelikteki kaynakları yönetmek için belirteci (5. adımdan) kullanır.

Web uygulamasının akışı aşağıda verilmiştir.

![Kaynak Yöneticisi kimlik doğrulama akışı](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Bir kullanıcı olarak, kullanmak istediğiniz aboneliğin abonelik KIMLIĞINI sağlarsınız:

![abonelik KIMLIĞI sağla](./media/resource-manager-api-authentication/sample-ux-1.png)

Oturum açmak için kullanılacak hesabı seçin.

![hesap seçin](./media/resource-manager-api-authentication/sample-ux-2.png)

Kimlik bilgilerinizi sağlayın.

![kimlik bilgilerini belirtin](./media/resource-manager-api-authentication/sample-ux-3.png)

Uygulamaya Azure aboneliklerinize erişim izni verin:

![Erişim izni ver](./media/resource-manager-api-authentication/sample-ux-4.png)

Bağlı aboneliklerinizi yönetin:

![Abonelik bağlama](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Uygulamayı Kaydet
Kodlamaya başlamadan önce, Web uygulamanızı Azure Active Directory (AD) ile kaydedin. Uygulama kaydı, Azure AD 'de uygulamanız için merkezi bir kimlik oluşturur. Uygulamanızın kimlik doğrulaması yapmak ve Azure Resource Manager API 'Lerine erişmek için kullandığı OAuth Istemci KIMLIĞI, yanıt URL 'Leri ve kimlik bilgileri gibi temel bilgileri barındırır. Uygulama kaydı Ayrıca, Kullanıcı için Microsoft API 'Lerine erişirken uygulamanızın ihtiyacı olan çeşitli izinleri kaydeder.

Uygulamanızı kaydetmek için bkz. [hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme](../active-directory/develop/quickstart-register-app.md). Uygulamanıza bir ad verin ve desteklenen hesap türleri için **herhangi bir kuruluş dizininde hesaplar** ' ı seçin. Yeniden yönlendirme URL 'SI için Azure Active Directory ilişkili bir etki alanı sağlayın.

AD uygulaması olarak oturum açmak için uygulama KIMLIĞI ve gizli anahtar gerekir. Uygulama KIMLIĞI, uygulamaya genel bakış bölümünde görüntülenir. Gizli anahtar oluşturmak ve API izinleri istemek için bkz. [hızlı başlangıç: Web API 'lerine erişmek için istemci uygulaması yapılandırma](../active-directory/develop/quickstart-configure-app-access-web-apis.md). Yeni bir istemci parolası sağlayın. API izinleri için **Azure hizmet yönetimi**' ni seçin. **Temsilci izinleri** ve **user_impersonation**seçin.

### <a name="optional-configuration---certificate-credential"></a>İsteğe bağlı yapılandırma-sertifika kimlik bilgileri
Azure AD, uygulamalar için sertifika kimlik bilgilerini de destekler: otomatik olarak imzalanan bir sertifika oluşturur, özel anahtarı koruyabilir ve Azure AD uygulama kaydınıza ortak anahtar ekleyebilirsiniz. Uygulamanız, kimlik doğrulaması için Azure AD 'ye özel anahtarınız kullanılarak imzalanmış küçük bir yük gönderir ve Azure AD, kaydettiğiniz ortak anahtarı kullanarak imzayı doğrular.

Sertifika ile bir AD uygulaması oluşturma hakkında daha fazla bilgi için bkz. Azure PowerShell kullanarak kaynaklara erişmek için [hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority) veya [kaynaklara erişmek üzere hizmet sorumlusu oluşturmak Için Azure CLI kullanma](resource-group-authenticate-service-principal-cli.md).

## <a name="get-tenant-id-from-subscription-id"></a>Abonelik KIMLIĞINDEN kiracı KIMLIĞINI al
Kaynak Yöneticisi çağırmak için kullanılabilecek bir belirteç istemek için uygulamanızın Azure aboneliğini barındıran Azure AD kiracısının kiracı KIMLIĞINI bilmeleri gerekir. Büyük olasılıkla, kullanıcılarınız abonelik kimliklerini bilir, ancak Azure Active Directory için kiracı kimliklerini bilmiyor olabilir. Kullanıcının kiracı KIMLIĞINI almak için kullanıcıdan abonelik KIMLIĞINI isteyin. Abonelik hakkında bir istek gönderirken bu abonelik KIMLIĞINI sağlayın:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Kullanıcı henüz oturum açmamış, ancak yanıttan kiracı KIMLIĞINI alabileceğiniz için istek başarısız olur. Bu özel durumda, **www-Authenticate**yanıt üst bilgisi DEĞERINDEN Kiracı kimliğini alın. Bu uygulamayı [Getdirectoryforsubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) yönteminde görürsünüz.

## <a name="get-user--app-access-token"></a>Kullanıcı ve uygulama erişim belirteci al
Uygulamanız, kullanıcının kimlik bilgilerini doğrulamak ve bir yetkilendirme kodu geri almak için bir OAuth 2,0 yetkilendirme Isteğiyle kullanıcıyı Azure AD 'ye yönlendirir. Uygulamanız Kaynak Yöneticisi için bir erişim belirteci almak üzere yetkilendirme kodunu kullanır. [Connectsubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) yöntemi yetkilendirme isteği oluşturur.

Bu makalede, kullanıcının kimliğini doğrulamak için REST API istekleri gösterilir. Kodunuzda kimlik doğrulaması yapmak için yardımcı kitaplıklarını da kullanabilirsiniz. Bu kitaplıklar hakkında daha fazla bilgi için bkz. [Azure Active Directory kimlik doğrulama kitaplıkları](../active-directory/active-directory-authentication-libraries.md). Bir uygulamada kimlik yönetimini tümleştirmeyle ilgili yönergeler için, [Azure Active Directory Geliştirici Kılavuzu](../active-directory/develop/v1-overview.md)' na bakın.

### <a name="auth-request-oauth-20"></a>Auth isteği (OAuth 2,0)
Azure AD yetkilendirme uç noktasına açık bir KIMLIK Connect/OAuth 2.0 yetkisi verme Isteği gönderin:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Bu istek için kullanılabilen sorgu dizesi parametreleri, [bir yetkilendirme kodu iste](../active-directory/develop/v1-protocols-oauth-code.md#request-an-authorization-code) makalesinde açıklanmaktadır.

Aşağıdaki örnek, OAuth 2.0 yetkilendirmesinin nasıl isteneceğini göstermektedir:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD kullanıcının kimliğini doğrular ve gerekirse, kullanıcıdan uygulamaya izin vermesini ister. Yetkilendirme kodunu, uygulamanızın yanıt URL 'sine döndürür. İstenen response_mode bağlı olarak, Azure AD, verileri sorgu dizesinde ya da post Data olarak geri gönderir.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth isteği (açık KIMLIK bağlantısı)
Yalnızca kullanıcıya yönelik Azure Resource Manager erişmek istemiyorsanız, ancak kullanıcının Azure AD hesabını kullanarak uygulamanızda oturum açmasına izin verdiğinizde, açık bir KIMLIK Connect yetkilendirme Isteği verin. Açık KIMLIK bağlantısı ile, uygulamanız Azure AD 'den, uygulamanızın kullanıcı oturumu açmak için kullanabileceği bir id_token alır.

Bu istek için kullanılabilen sorgu dizesi parametreleri, [oturum açma Isteği gönderme](../active-directory/develop/v1-protocols-openid-connect-code.md#send-the-sign-in-request) makalesinde açıklanmaktadır.

Örnek bir Open ID Connect isteği:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD kullanıcının kimliğini doğrular ve gerekirse, kullanıcıdan uygulamaya izin vermesini ister. Yetkilendirme kodunu, uygulamanızın yanıt URL 'sine döndürür. İstenen response_mode bağlı olarak, Azure AD, verileri sorgu dizesinde ya da post Data olarak geri gönderir.

Örnek bir açma KIMLIĞI bağlantı yanıtı şunlardır:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Belirteç isteği (OAuth 2.0 kodu verme akışı)
Artık uygulamanız Azure AD 'den yetkilendirme kodunu aldığından, Azure Resource Manager için erişim belirtecini alma zamanı.  Azure AD belirteç uç noktasına bir OAuth 2.0 kodu verme belirteci Isteği gönderin:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Bu istek için kullanılabilen sorgu dizesi parametreleri, [Yetkilendirme kodunu kullanma](../active-directory/develop/v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) makalesinde açıklanmaktadır.

Aşağıdaki örnek, parola kimlik bilgileriyle kod verme belirteci için bir istek gösterir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Sertifika kimlik bilgileriyle çalışırken, uygulamanızın sertifika kimlik bilgisinin özel anahtarını kullanarak bir JSON Web Token (JWT) ve işaret (RSA SHA256) oluşturun. Bu belirtecin oluşturulması [istemci kimlik bilgisi akışında](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#second-case-access-token-request-with-a-certificate)gösterilmektedir.

İstemci kimlik doğrulamasıyla ilgili ayrıntılar için bkz. [Açık kimlik Connect spec](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) .

Aşağıdaki örnek, sertifika kimlik bilgileri ile kod verme belirteci için bir istek gösterir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Kod verme belirteci için örnek yanıt:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Tanıtıcı kodu verme belirteci yanıtı
Başarılı bir belirteç yanıtı, Azure Resource Manager için (Kullanıcı + uygulama) erişim belirtecini içerir. Uygulamanız, Kullanıcı için Kaynak Yöneticisi erişmek üzere bu erişim belirtecini kullanır. Azure AD tarafından verilen erişim belirteçlerinin yaşam süresi bir saattir. Web uygulamanızın (Kullanıcı + uygulama) erişim belirtecini yenilemesi gerekir. Erişim belirtecini yenilemesi gerekiyorsa uygulamanızın belirteç yanıtında aldığı yenileme belirtecini kullanın. Azure AD belirteç uç noktasına bir OAuth 2.0 belirteç Isteği gönderin:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Yenileme isteğiyle birlikte kullanılacak parametreler, [erişim belirtecini yenileme](../active-directory/develop/v1-protocols-oauth-code.md#refreshing-the-access-tokens)bölümünde açıklanmaktadır.

Aşağıdaki örnek, yenileme belirtecinin nasıl kullanılacağını gösterir:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Yenileme belirteçleri Azure Resource Manager için yeni erişim belirteçleri almak üzere kullanılabilmesine rağmen, uygulamanız tarafından çevrimdışı erişim için uygun değildir. Belirteçleri yenileme ömrü sınırlıdır ve yenileme belirteçleri kullanıcıya bağlanır. Kullanıcı kuruluştan ayrılsa, yenileme belirtecini kullanan uygulama erişimi kaybeder. Bu yaklaşım, takımlar tarafından Azure kaynaklarını yönetmek için kullanılan uygulamalar için uygun değildir.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Kullanıcının aboneliğe erişim atayıp atayabileceği denetlenir
Uygulamanız artık Kullanıcı için Azure Resource Manager erişim belirtecine sahiptir. Sonraki adım, uygulamanızı aboneliğe bağlamak için kullanılır. Bağlandıktan sonra, uygulamanız Kullanıcı mevcut olmadığında bile bu abonelikleri yönetebilir (uzun süreli çevrimdışı erişim).

Bağlanılacak her abonelik için, kullanıcının abonelik için erişim yönetimi haklarına sahip olup olmadığını öğrenmek üzere [Kaynak Yöneticisi List Permissions](https://docs.microsoft.com/rest/api/authorization/permissions) API 'sini çağırın.

ASP.NET MVC örnek uygulamasının [Usercanmanageraccessforsubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) yöntemi bu çağrıyı uygular.

Aşağıdaki örnek, bir abonelik üzerinde kullanıcının izinlerinin nasıl isteneceğini gösterir. 83cfe939-2402-4581-b761-4f59b0a041e4 aboneliğin KIMLIĞIDIR.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Kullanıcının abonelikle ilgili izinlerini almak için yanıta bir örnek:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

İzinler API 'SI birden çok izin döndürür. Her izin, izin verilen eylemlerden (**Eylemler**) ve izin verilmeyen eylemlerden (**notactions**) oluşur. Bir eylem izin verilen eylemlerde varsa ve bu iznin izin verilmeyen eylemlerde yoksa, kullanıcının bu eylemi yapmasına izin verilir. **Microsoft. Authorization/roleatamalar/Write** , erişim yönetimi hakları veren eylemdir. Uygulamanız, her iznin **Eylemler** ve **notactions** bu eylem dizesinde bir Regex eşleşmesi aramak için izinler sonucunu ayrıştırmalıdır.

## <a name="get-app-only-access-token"></a>Yalnızca uygulama erişim belirtecini al
Şimdi, kullanıcının Azure aboneliğine erişim atayıp atamadığına bilirsiniz. Sonraki adımlar şunlardır:

1. Abonelik üzerinde uygulamanızın kimliğine uygun RBAC rolünü atayın.
2. Uygulamanın abonelik iznini sorgulayarak veya yalnızca uygulama belirtecini kullanarak Kaynak Yöneticisi erişerek erişim atamasını doğrulayın.
3. Bağlantıyı uygulamalarınıza "bağlı abonelikler" veri yapısı olarak kaydedin-aboneliğin KIMLIĞI kalıcı hale.

İlk adımdan daha yakından bakalım. Uygun RBAC rolünü uygulamanın kimliğine atamak için şunu belirlemelisiniz:

* Kullanıcı Azure Active Directory uygulamanızın kimliğinin nesne KIMLIĞI
* Uygulamanızın abonelikte gerektirdiği RBAC rolünün tanıtıcısı

Uygulamanız bir Azure AD 'den bir kullanıcının kimliğini doğruladığında, bu Azure AD 'de uygulamanız için bir hizmet sorumlusu nesnesi oluşturur. Azure, Azure kaynaklarında ilgili uygulamalara doğrudan erişim sağlamak için RBAC rollerinin hizmet sorumlularına atanmasına izin verir. Bu eylem, tam olarak yapmak istediğiniz şeydir. Azure AD Graph API, oturum açan kullanıcının Azure AD 'de uygulamanızın hizmet sorumlusu tanımlayıcısını öğrenmek için sorgulayın.

Azure Resource Manager için yalnızca bir erişim belirteciniz var; Azure AD Graph API çağırmak için yeni bir erişim belirtecine ihtiyacınız vardır. Azure AD 'deki her uygulamanın kendi hizmet sorumlusu nesnesini sorgulama izni vardır, bu nedenle yalnızca uygulama erişim belirteci yeterlidir.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Azure AD Graph API için yalnızca uygulama erişim belirtecini al

Uygulamanızın kimliğini doğrulamak ve Azure AD Graph API bir belirteç almak için, bir Istemci kimlik bilgisi verme Azure AD belirteç uç noktasına OAuth 2.0 akış belirteci isteği verme (**https: \//Login. microsoftonline. com/{directory_domain_name}/OAuth2/Token**).

ASP.net MVC örnek uygulamasının [Getobjectidofserviceprincipalınorganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) yöntemi, .net için Active Directory Authentication Library kullanarak Graph API için yalnızca uygulamaya yönelik bir erişim belirteci alır.

Bu istek için kullanılabilen sorgu dizesi parametreleri, [erişim belirteci isteme](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#request-an-access-token) makalesinde açıklanmaktadır.

İstemci kimlik bilgileri verme belirteci için örnek istek:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

İstemci kimlik bilgileri verme belirtecine yönelik örnek yanıt:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Kullanıcı Azure AD 'de uygulama hizmeti sorumlusu ObjectID 'yi al
Şimdi, dizinde uygulamanın hizmet sorumlusunun nesne KIMLIĞINI öğrenmek için [Azure AD Graph hizmet sorumluları](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API 'sini sorgulamak üzere yalnızca uygulama erişim belirtecini kullanın.

ASP.net MVC örnek uygulamasının [Getobjectidofservicesprincipalınorganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) yöntemi bu çağrıyı uygular.

Aşağıdaki örnek, bir uygulamanın hizmet sorumlusu nasıl isteyeceğini gösterir. a0448380-c346-4f9f-b897-c18733de9394, uygulamanın istemci KIMLIĞIDIR.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Aşağıdaki örnek, bir uygulamanın hizmet sorumlusu için bir istek yanıtı gösterir

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC rol tanımlayıcısı 'nı al
Uygun RBAC rolünü hizmet sorumlusuna atamak için Azure RBAC rolünün tanımlayıcısını belirlemelisiniz.

Uygulamanız için doğru RBAC rolü:

* Uygulamanız yalnızca aboneliği izler, hiçbir değişiklik yapmadan abonelik üzerinde yalnızca okuyucu izinleri gerekir. **Okuyucu** rolünü atayın.
* Uygulamanız Azure aboneliğini yönetirse, varlıkları oluşturuyor/değiştiriyor/siliyor, katkıda bulunan izinlerden birini gerektirir.
  * Belirli bir kaynak türünü yönetmek için kaynağa özgü katkıda bulunan rolleri atayın (sanal makine katılımcısı, sanal ağ katılımcısı, depolama hesabı katılımcısı vb.)
  * Herhangi bir kaynak türünü yönetmek için, **katkıda bulunan** rolünü atayın.

Uygulamanız için rol ataması kullanıcılar tarafından görülebilir, bu nedenle en düşük gerekli ayrıcalıkları seçin.

Tüm Azure RBAC rollerini listelemek için [Kaynak Yöneticisi rol tanımı API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) 'sini çağırın ve ardından ad ile rol tanımını bulmak için sonucu yineleyin.

ASP.net MVC örnek uygulamasının [Getroleıd](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) yöntemi bu çağrıyı uygular.

Aşağıdaki istek örneği, Azure RBAC rol tanımlayıcısının nasıl alınacağını göstermektedir. 09cbd307-aa71-4aca-b346-5f253e6e3ebb aboneliğin KIMLIĞIDIR.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Yanıt aşağıdaki biçimdedir:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Bu API 'yi sürekli olarak çağırmanız gerekmez. Rol tanımının iyi bilinen GUID 'sini belirledikten sonra, rol tanımı KIMLIĞINI şu şekilde oluşturabilirsiniz:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Yaygın olarak kullanılan yerleşik rollerin tanımlayıcıları aşağıda verilmiştir:

| Rol | GUID |
| --- | --- |
| Okuyucu |acdd72a7-3385-48ef-bd42-f606fba81ae7 |
| Mcý |b24988ac-6180-42a0-ab88-20f7382dd24c |
| Sanal makine Katılımcısı |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Sanal ağ katılımcısı |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Depolama hesabı Katılımcısı |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Web sitesi Katılımcısı |de139f84-1756-47ae-9be6-808fbbe84772 |
| Web planı Katılımcısı |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| Katkıda bulunan SQL Server |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| SQL DB Katılımcısı |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-to-application"></a>Uygulamaya RBAC rolü atama
[Kaynak Yöneticisi rol atama](https://docs.microsoft.com/rest/api/authorization/roleassignments) API 'sini kullanarak uygun RBAC rolünü hizmet sorumlusuna atamanız için ihtiyacınız olan her şeye sahipsiniz.

ASP.net MVC örnek uygulamasının [Grantrotatoserviceprincipalonsubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) yöntemi bu çağrıyı uygular.

Uygulamaya RBAC rolü atama isteği örneği:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

İstekte aşağıdaki değerler kullanılır:

| Guid | Açıklama |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |aboneliğin KIMLIĞI |
| c3097b31-7309-4c59-b4e3-770f8406bad2 |uygulamanın hizmet sorumlusunun nesne KIMLIĞI |
| b24988ac-6180-42a0-ab88-20f7382dd24c |katkıda bulunan rolünün KIMLIĞI |
| 4f87261d-2816-465D-8311-70a27558df4c |Yeni rol ataması için yeni bir GUID oluşturuldu |

Yanıt aşağıdaki biçimdedir:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Azure Resource Manager için yalnızca uygulama erişim belirteci al
Uygulamanın aboneliğe erişebileceğini doğrulamak için, yalnızca uygulama belirtecini kullanarak abonelikte bir test görevi yapın.

Yalnızca uygulama erişim belirtecini almak için, kaynak parametresi için farklı bir değerle [Azure AD Graph API için yalnızca uygulama erişim belirtecini al](#app-azure-ad-graph)bölümündeki yönergeleri izleyin:

    https://management.core.windows.net/

ASP.NET MVC örnek uygulamasının [Serviceprincipalhasreadaccesstosubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) yöntemi, .net için Active Directory Authentication Library kullanarak Azure Resource Manager için yalnızca uygulamaya yönelik bir erişim belirteci alır.

#### <a name="get-applications-permissions-on-subscription"></a>Uygulamanın abonelik Izinlerini al
Uygulamanızın bir Azure aboneliğine erişebileceğini denetlemek için [Kaynak Yöneticisi izin](https://docs.microsoft.com/rest/api/authorization/permissions) API 'sini de çağırabilirsiniz. Bu yaklaşım, kullanıcının abonelik için erişim yönetimi haklarına sahip olup olmadığını nasıl belirlediğinize benzer. Ancak, bu kez, önceki adımda aldığınız salt uygulama erişim belirteci ile izin API 'sini çağırın.

ASP.NET MVC örnek uygulamasının [Serviceprincipalhasreadaccesstosubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) yöntemi bu çağrıyı uygular.

## <a name="manage-connected-subscriptions"></a>Bağlı abonelikleri yönetme
Abonelik üzerinde uygulamanızın hizmet sorumlusuna uygun RBAC rolü atandığında, uygulamanız Azure Resource Manager için yalnızca uygulama erişim belirteçlerini kullanarak izlemeyi/yönetmeyi devam edebilir.

Bir abonelik sahibi, portal veya komut satırı araçlarını kullanarak uygulamanızın rol atamasını kaldırırsa, uygulamanız artık bu aboneliğe erişemez. Bu durumda, kullanıcıya abonelikle bağlantının uygulamanın dışından çıkartılmış olduğunu ve bu bağlantıya "Repair" bir seçenek sunmasını bildirmelisiniz. "Onar", çevrimdışı olarak silinen rol atamasını yeniden oluşturur.

Kullanıcı aboneliklerinizi uygulamanıza bağlamayı etkinleştirdiğinizde olduğu gibi, kullanıcının aboneliklerin bağlantısını kesmelerine de izin vermeniz gerekir. Bir erişim yönetimi noktasından, bağlantıyı kesme, uygulamanın hizmet sorumlusunun abonelikte sahip olduğu rol atamasının kaldırılması anlamına gelir. İsteğe bağlı olarak, aboneliğin uygulamasındaki herhangi bir durum da kaldırılabilir.
Yalnızca abonelik üzerinde erişim yönetimi iznine sahip kullanıcılar aboneliğin bağlantısını kesebilir.

ASP.net MVC örnek uygulamasının [iptal eden Erolefromserviceprincipalonsubscription yöntemi](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) bu çağrıyı uygular.

Bu BT, kullanıcılar artık Azure aboneliklerini uygulamanızla kolayca bağlayıp yönetebilir.
