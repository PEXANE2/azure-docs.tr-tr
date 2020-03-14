---
title: Azure Active Directory uygulama bildirimini anlama | Microsoft Docs
description: Uygulamanın Azure AD kiracısındaki kimlik yapılandırmasını temsil eden ve OAuth yetkilendirme, onay deneyimini ve daha fazlasını kolaylaştırmak için kullanılan Azure Active Directory Uygulama bildiriminin ayrıntılı kapsamı.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/03/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: sureshja
ms.openlocfilehash: a12715ba9aac77461d4968bd9b8f3de30af243c4
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79262756"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory uygulama bildirimi

Uygulama bildirimi Microsoft Identity platformunda bir uygulama nesnesinin tüm özniteliklerinin tanımını içerir. Ayrıca uygulama nesnesini güncelleştirmek için bir mekanizma işlevi görür. Uygulama varlığı ve şeması hakkında daha fazla bilgi için [Graph API uygulama varlığı belgelerine](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)bakın.

Bir uygulamanın özniteliklerini Azure portal veya [REST API](https://docs.microsoft.com/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) veya [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#applications)kullanarak program aracılığıyla yapılandırabilirsiniz. Ancak, bir uygulamanın özniteliğini yapılandırmak için uygulama bildirimini düzenlemeniz gereken bazı senaryolar vardır. Bu senaryolar şunlardır:

* Uygulamayı Azure AD çok kiracılı ve kişisel Microsoft hesapları olarak kaydettiniz, Kullanıcı arabirimindeki desteklenen Microsoft hesaplarını değiştiremezsiniz. Bunun yerine, desteklenen hesap türünü değiştirmek için uygulama bildirimi düzenleyicisini kullanmanız gerekir.
* Uygulamanızın desteklediği izinleri ve rolleri tanımlamanız gerekiyorsa, uygulama bildirimini değiştirmeniz gerekir.

## <a name="configure-the-app-manifest"></a>Uygulama bildirimini yapılandırma

Uygulama bildirimini yapılandırmak için:

1. [Azure portalına](https://portal.azure.com) gidin. **Azure Active Directory** hizmetini arayıp seçin.
1. **Uygulama kayıtları**'nı seçin.
1. Yapılandırmak istediğiniz uygulamayı seçin.
1. Uygulamanın **Genel Bakış** sayfasında, **Bildirim** bölümünü seçin. Web tabanlı bir bildirim Düzenleyicisi açılır ve bu, portalı içindeki bildirimi düzenlemenize olanak tanır. İsteğe bağlı olarak, bildirimi yerel olarak düzenlemek için **İndir** ' i seçip uygulamanıza yeniden uygulamak Için **karşıya yükle** ' yi kullanabilirsiniz.

## <a name="manifest-reference"></a>Bildirim başvurusu


### <a name="key-value-type-accesstokenacceptedversion-nullable-int32"></a>Anahtar, değer türü: `accessTokenAcceptedVersion`, Nullable Int32 
Kaynak tarafından beklenen erişim belirteci sürümünü belirtir. Bu parametre, erişim belirtecini istemek için kullanılan uç noktadan veya istemciden bağımsız olarak üretilen JWT sürümünü ve biçimini değiştirir.

Kullanılan uç nokta, v 1.0 veya v 2.0, istemci tarafından seçilir ve yalnızca id_tokens sürümünü etkiler. Kaynakların, desteklenen erişim belirteci biçimini belirtecek `accesstokenAcceptedVersion` açıkça yapılandırması gerekir.

`accesstokenAcceptedVersion` için olası değerler 1, 2 veya null değerlerdir. Değer null ise, bu parametre, v 1.0 uç noktasına karşılık gelen varsayılan olarak 1 ' dir. 

`signInAudience` `AzureADandPersonalMicrosoftAccount`, değer `2` olmalıdır  

Örnek değer: `2` 

### <a name="key-value-type-addins-collection"></a>Anahtar, değer türü: `addIns`, koleksiyon 
Bir tüketen hizmetin belirli bağlamlarda uygulama çağırmak için kullanabileceği özel davranışı tanımlar. Örneğin, dosya akışlarını işleyebilen uygulamalar, "FileHandler" işlevselliği için AddIns özelliğini ayarlayabilir. Bu parametre, Office 365 gibi hizmetlerin uygulamayı kullanıcının üzerinde çalıştığı bir belge bağlamında çağırmasını sağlar. 

Örnek değer: 
<code>{<br>&nbsp;&nbsp;&nbsp;"id":"968A844F-7A47-430C-9163-07AE7C31D407"<br>&nbsp;&nbsp;&nbsp;"type": "FileHandler",<br>&nbsp;&nbsp;&nbsp;"properties": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{"key": "version", "value": "2" }<br>&nbsp;&nbsp;&nbsp;]<br>}</code>

### <a name="key-value-type-allowpublicclient-boolean"></a>Anahtar, değer türü: `allowPublicClient`, Boolean 
Geri dönüş uygulama türünü belirtir. Azure AD, varsayılan olarak, bir uygulama türünü replyUrlsWithType öğesinden algılar. Azure AD 'nin istemci uygulama türünü belirleyeleyemiyorsa bazı senaryolar vardır. Örneğin, bu tür bir senaryo HTTP isteğinin URL yeniden yönlendirmesi olmadan gerçekleştiği [Ropc](https://tools.ietf.org/html/rfc6749#section-4.3) akışsudur. Bu durumlarda, Azure AD, uygulama türünü bu özelliğin değerine göre yorumlayacak. Bu değer true olarak ayarlanırsa, geri dönüş uygulama türü, bir mobil cihazda çalışan yüklü uygulama gibi ortak istemci olarak ayarlanır. Varsayılan değer false 'dur. Bu, geri dönüş uygulama türünün Web uygulaması gibi gizli bir istemci olduğu anlamına gelir. 

Örnek değer: `false` 

### <a name="key-value-type-availabletoothertenants-boolean"></a>Anahtar, değer türü: `availableToOtherTenants`, Boolean 
uygulama diğer kiracılar ile paylaşılmışsa doğru; Aksi takdirde, false. <br><br> Note: Bu yalnızca **uygulama kayıtları (eski)** deneyimde mevcuttur. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde `signInAudience` ile değiştirilmiştir. 

### <a name="key-value-type-appid-string"></a>Anahtar, değer türü: `appId`, dize 
Azure AD tarafından bir uygulamaya atanan uygulama için benzersiz tanımlayıcıyı belirtir. 

Örnek değer: `"601790de-b632-4f57-9523-ee7cb6ceba95"` 

### <a name="key-value-type-approles-collection"></a>Anahtar, değer türü: `appRoles`, koleksiyon 
Bir uygulamanın bildirebilen rollerin koleksiyonunu belirtir. Bu roller kullanıcılara, gruplara veya hizmet sorumlularına atanabilir. Daha fazla örnek ve bilgi için bkz. [uygulamanıza uygulama rolleri ekleme ve bunları belirtece alma](howto-add-app-roles-in-azure-ad-apps.md) 

Örnek değer: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  

### <a name="key-value-type-displayname-string"></a>Anahtar, değer türü: `displayName`, dize 
Uygulamanın görünen adı. <br><br> Note: Bu yalnızca **uygulama kayıtları (eski)** deneyimde mevcuttur. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde `name` ile değiştirilmiştir. 

Örnek değer: `"MyRegisteredApp"` 

### <a name="key-value-type-errorurl-string"></a>Anahtar, değer türü: `errorUrl`, dize 
Desteklenen. 

### <a name="key-value-type-groupmembershipclaims-string"></a>Anahtar, değer türü: `groupMembershipClaims`, dize 
Uygulamanın beklediği bir kullanıcı veya OAuth 2,0 erişim belirtecinde verilen `groups` talebini yapılandırır. Bu özniteliği ayarlamak için aşağıdaki geçerli dize değerlerinden birini kullanın:
- `"None"`
- `"SecurityGroup"` (güvenlik grupları ve Azure AD rolleri için)
- `"All"` (Bu işlem, oturum açan kullanıcının üyesi olduğu tüm güvenlik gruplarını, dağıtım gruplarını ve Azure AD dizin rollerini alır. 

Örnek değer: `"SecurityGroup"` 

### <a name="key-value-type-homepage-string"></a>Anahtar, değer türü: `homepage`, dize 
Uygulamanın giriş sayfasının URL 'SI. <br><br> Note: Bu yalnızca **uygulama kayıtları (eski)** deneyimde mevcuttur. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde `signInUrl` ile değiştirilmiştir. 

Örnek değer: `"https://MyRegisteredApp"` 

### <a name="key-value-type-objectid-string"></a>Anahtar, değer türü: `objectId`, dize 
Dizindeki uygulamanın benzersiz tanımlayıcısı. <br><br> Bu yalnızca **uygulama kayıtları (eski)** deneyimde mevcuttur. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde `id` ile değiştirilmiştir. 

Örnek değer: `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` 

### <a name="key-value-type-optionalclaims-string"></a>Anahtar, değer türü: `optionalClaims`, dize 
Bu belirli uygulama için güvenlik belirteci hizmeti tarafından belirteçte döndürülen isteğe bağlı talepler.<br>Şu anda, hem kişisel hesapları hem de Azure AD 'yi (uygulama kayıt portalı üzerinden kaydedilir) destekleyen uygulamalar isteğe bağlı talepler kullanamaz. Ancak, yalnızca Azure AD için kaydedilmiş uygulamalar, v 2.0 uç noktasını kullanarak, bildirimde istedikleri isteğe bağlı talepleri alabilir. Daha fazla bilgi için bkz. [isteğe bağlı talepler](active-directory-optional-claims.md). 

Örnek değer:  
`null` 

### <a name="key-value-type-id-string"></a>Anahtar, değer türü: `id`, dize 
Dizindeki uygulamanın benzersiz tanımlayıcısı. Bu KIMLIK, herhangi bir protokol işleminde uygulamayı tanımlamak için kullanılan tanımlayıcı değildir. Dizin sorgularında nesnesine başvurmak için kullanılır. 

Örnek değer: `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` 

### <a name="key-value-type-identifieruris-string-array"></a>Anahtar, değer türü: `identifierUris`, dize dizisi 
Bir Web uygulamasını Azure AD kiracısı içinde benzersiz bir şekilde tanımlayan Kullanıcı tanımlı URI 'ler veya uygulama çok kiracılı ise doğrulanmış bir özel etki alanı içinde. 

Örnek değer: 
<code>[<br>&nbsp;&nbsp;"https://MyRegisteredApp"<br>]</code> 

### <a name="key-value-type-informationalurls-string"></a>Anahtar, değer türü: `informationalUrls`, dize 
Uygulamanın hizmet koşulları ve gizlilik bildirimi bağlantılarını belirtir. Hizmet koşulları ve gizlilik bildirimi, kullanıcılar tarafından Kullanıcı onay deneyimi aracılığıyla ortaya çıkmış. Daha fazla bilgi için bkz. [nasıl yapılır: kayıtlı Azure AD uygulamaları için hizmet koşulları ve gizlilik bildirimi ekleme](howto-add-terms-of-service-privacy-statement.md). 

Örnek değer: 
<code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> 

### <a name="key-value-type-keycredentials-collection"></a>Anahtar, değer türü: `keyCredentials`, koleksiyon 
Uygulama tarafından atanan kimlik bilgileri, dize tabanlı paylaşılan gizlilikler ve X. 509.440 sertifikalarına yönelik başvuruları tutar. ). 

Örnek değer: 
<code>[<br>&nbsp;{<br>&nbsp;&These credentials are used when requesting access tokens (when the app is acting as a client rather that as resourcenbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> 

### <a name="key-value-type-knownclientapplications-string-array"></a>Anahtar, değer türü: `knownClientApplications`, dize dizisi 
İki bölümden oluşan bir çözümünüz varsa, bir istemci uygulaması ve özel bir Web API uygulaması varsa, bu izni paketleme için kullanılır. İstemci uygulamasının AppID 'sini bu değere girerseniz, kullanıcının istemci uygulamasına yalnızca bir kez onay girmesi gerekir. Azure AD, istemci ile ilgili bir bildirimin Web API 'sine örtülü olarak nasıl katılmadığını bilecektir. Hem istemci hem de Web API 'SI için aynı anda hizmet sorumlularını otomatik olarak sağlayacaktır. Hem istemci hem de Web API uygulaması aynı kiracıda kayıtlı olmalıdır. 

Örnek değer: `["f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"]` 

### <a name="key-value-type-logourl-string"></a>Anahtar, değer türü: `logoUrl`, dize 
Portalda karşıya yüklenen logoya yönelik CDN URL 'sine işaret eden salt okuma değeri. 

Örnek değer: `"https://MyRegisteredAppLogo"` 

### <a name="key-value-type-logouturl-string"></a>Anahtar, değer türü: `logoutUrl`, dize 
Uygulamanın oturumunu kapatmak için kullanılacak URL. 

Örnek değer:  
`"https://MyRegisteredAppLogout"` 

### <a name="key-value-type-name-string"></a>Anahtar, değer türü: `name`, dize 
Uygulamanın görünen adı. 

Örnek değer: `"MyRegisteredApp"` 

### <a name="key-value-type-oauth2allowimplicitflow-boolean"></a>Anahtar, değer türü: `oauth2AllowImplicitFlow`, Boolean 
Bu Web uygulamasının OAuth 2.0 örtük akış erişim belirteçleri isteyip isteyemeyeceğini belirtir. Varsayılan değer false. Bu bayrak, JavaScript tek sayfalı uygulamalar gibi tarayıcı tabanlı uygulamalar için kullanılır. Daha fazla bilgi edinmek için içindekiler tablosuna `OAuth 2.0 implicit grant flow` girin ve örtük akış hakkındaki konuları görüntüleyin. 

Örnek değer: `false` 

### <a name="key-value-type-oauth2allowidtokenimplicitflow-boolean"></a>Anahtar, değer türü: `oauth2AllowIdTokenImplicitFlow`, Boolean 
Bu Web uygulamasının OAuth 2.0 örtük akış KIMLIĞI belirteçleri isteyip isteyemeyeceğini belirtir. Varsayılan değer false. Bu bayrak, JavaScript tek sayfalı uygulamalar gibi tarayıcı tabanlı uygulamalar için kullanılır. 

Örnek değer: `false` 

### <a name="key-value-type-oauth2permissions-collection"></a>Anahtar, değer türü: `oauth2Permissions`, koleksiyon 
Web API (kaynak) uygulamasının istemci uygulamalarına sunduğu OAuth 2,0 izin kapsamlarının koleksiyonunu belirtir. Bu izin kapsamları, izin sırasında istemci uygulamalarına verilebilir. 

Örnek değer: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>] </code>

### <a name="key-value-type-oauth2requiredpostresponse-boolean"></a>Anahtar, değer türü: `oauth2RequiredPostResponse`, Boolean 
OAuth 2,0 belirteç isteklerinin bir parçası olarak, Azure AD 'nin istekleri almak yerine POST isteklerine izin verip vermeyeceğini belirtir. Varsayılan değer, yalnızca GET isteklerinin izin verileceğini belirten false şeklindedir. 

Örnek değer: `false` 

### <a name="key-value-type-parentalcontrolsettings-string"></a>Anahtar, değer türü: `parentalControlSettings`, dize 

`countriesBlockedForMinors`, uygulamanın minors için engellediği ülkeleri belirtir.<br>`legalAgeGroupRule`, uygulamanın kullanıcıları için geçerli olan geçerli yaş grubu kuralını belirtir. `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`veya `BlockMinors`olarak ayarlanabilir.  

Örnek değer: 
<code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> 

### <a name="key-value-type-passwordcredentials-collection"></a>Anahtar, değer türü: `passwordCredentials`, koleksiyon 
`keyCredentials` özelliğinin açıklamasına bakın. 

Örnek değer: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> 

### <a name="key-value-type-preauthorizedapplications-collection"></a>Anahtar, değer türü: `preAuthorizedApplications`, koleksiyon 
Kapalı onay için uygulamaları ve istenen izinleri listeler. Uygulamaya onay sağlanması için bir yönetici gerekir. Ön kimlik onayı Kullanıcı tarafından istenen izinlere izin vermesini gerektirmez. Ön kimlik onayı ' nda listelenen izinler için Kullanıcı onayı gerekmez. Bununla birlikte, ön ek Uthorizedapplications içinde listelenmeyen ek istenen izinler için Kullanıcı onayı gerekir. 

Örnek değer: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> 

### <a name="key-value-type-publicclient-boolean"></a>Anahtar, değer türü: `publicClient`, Boolean 
Bu uygulamanın ortak bir istemci olup olmadığını belirtir (örneğin, bir mobil cihazda çalışan yüklü bir uygulama gibi). 

Bu özellik yalnızca **uygulama kayıtları (eski)** deneyimde kullanılabilir. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde `allowPublicClient` ile değiştirilmiştir. 

### <a name="key-value-type-publisherdomain-string"></a>Anahtar, değer türü: `publisherDomain`, dize 
Uygulama için doğrulanmış yayımcı etki alanı. Salt okunurdur. 

Örnek değer: `https://www.contoso.com`

### <a name="key-value-type-replyurls-string-array"></a>Anahtar, değer türü: `replyUrls`, dize dizisi 
Bu çoklu değer özelliği, Azure AD 'nin belirteçleri döndürürken hedef olarak kabul edeceği kayıtlı redirect_uri değerlerinin listesini tutar. <br><br> Bu özellik yalnızca **uygulama kayıtları (eski)** deneyimde kullanılabilir. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminde `replyUrlsWithType` ile değiştirilmiştir. 

### <a name="key-value-type-replyurlswithtype-collection"></a>Anahtar, değer türü: `replyUrlsWithType`, koleksiyon 
Bu çoklu değer özelliği, Azure AD 'nin belirteçleri döndürürken hedef olarak kabul edeceği kayıtlı redirect_uri değerlerinin listesini tutar. Her URI değeri, ilişkili bir uygulama türü değeri içermelidir. Desteklenen tür değerleri şunlardır: <ul><li>`Web`</li><li>`InstalledClient`</li></ul><br> [Replyurl kısıtlamaları ve sınırlamaları](https://docs.microsoft.com/azure/active-directory/develop/reply-url)hakkında daha fazla bilgi edinin. 

Örnek değer: 
<code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> 

### <a name="key-value-type-requiredresourceaccess-collection"></a>Anahtar, değer türü: `requiredResourceAccess`, koleksiyon 
Dinamik onay ile, yönetici onay deneyimini ve statik onay kullanan kullanıcılar için Kullanıcı onay deneyimini `requiredResourceAccess`. Ancak, bu parametre genel durum için Kullanıcı onay deneyimini değil.<br>`resourceAppId`, uygulamanın erişmesi gereken kaynak için benzersiz tanıtıcıdır. Bu değer, hedef kaynak uygulamasında belirtilen uygulama kimliğine eşit olmalıdır.<br>`resourceAccess`, belirtilen kaynaktan uygulamanın gerektirdiği OAuth 2.0 izin kapsamlarını ve uygulama rollerini listeleyen bir dizidir. Belirtilen kaynakların `id` ve `type` değerlerini içerir. 

Örnek değer: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>] </code> 

### <a name="key-value-type-samlmetadataurl-string"></a>Anahtar, değer türü: `samlMetadataUrl`, dize 
Uygulamanın SAML meta verilerinin URL 'SI. 

Örnek değer: `https://MyRegisteredAppSAMLMetadata` 

### <a name="key-value-type-signinurl-string"></a>Anahtar, değer türü: `signInUrl`, dize 

Uygulamanın giriş sayfasına yönelik URL 'YI belirtir. 

Örnek değer: `https://MyRegisteredApp` 

### <a name="key-value-type-signinaudience-string"></a>Anahtar, değer türü: `signInAudience`, dize 
Geçerli uygulama için hangi Microsoft hesaplarının desteklendiğini belirtir. Desteklenen değerler şunlardır:
- **Azureadmyorg** -Kuruluşumun Azure AD kiracısında bir Microsoft iş veya okul hesabı olan kullanıcılar (örneğin, tek kiracılı)
- **Azureadmultipleorgs** -herhangi bir kuruluşun Azure AD kiracısında bir Microsoft iş veya okul hesabı olan kullanıcılar (örneğin, çok kiracılı)
- **Azureadandpersonmicrosoftaccount** -kişisel Microsoft hesabı olan kullanıcılar veya herhangi bir kuruluşun Azure AD kiracısında bir iş veya okul hesabı 

Örnek değer:  
`AzureADandPersonalMicrosoftAccount` 

### <a name="key-value-type-tags-string-array"></a>Anahtar, değer türü: `tags`, dize dizisi 
Uygulamayı kategorilere ayırmak ve tanımlamak için kullanılabilen özel dizeler. 

Örnek değer: 
<code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code>

## <a name="common-issues"></a>Genel sorunlar

### <a name="manifest-limits"></a>Bildirim limitleri

Uygulama bildiriminde koleksiyonlar olarak adlandırılan birden çok öznitelik vardır; Örneğin, approles, keycredentials, knownClientApplications, ıdentifieruris, rediretUris, requiredResourceAccess ve oauth2Permissions. Herhangi bir uygulama için tam uygulama bildiriminde, Birleşik tüm koleksiyonlardaki toplam giriş sayısı 1200 ' de kadık. Daha önce uygulama bildiriminde 100 yeniden yönlendirme URI 'si belirtirseniz, daha sonra yalnızca, bildirimi oluşturan diğer tüm koleksiyonlar genelinde kullanılacak 1100 kalan girişlerle birlikte kaldınız.

> [!NOTE]
> Uygulama bildiriminde 1200 'den fazla girdi eklemeye çalışırsanız, **"uygulama xxxxxx güncelleştirilemedi" hatasını görebilirsiniz. Hata ayrıntıları: bildirimin boyutu sınırı aştı. Lütfen değer sayısını azaltın ve isteğinizi yeniden deneyin. "**

### <a name="unsupported-attributes"></a>Desteklenmeyen öznitelikler

Uygulama bildirimi, Azure AD 'de temel alınan uygulama modelinin şemasını temsil eder. Temel alınan şema geliştikçe, bildirim Düzenleyicisi yeni şemayı zaman zaman yansıtacak şekilde güncelleştirilecektir. Sonuç olarak, uygulama bildiriminde yeni öznitelikler olduğunu fark edebilirsiniz. Nadir durumlarda, var olan özniteliklerde bir sözdizimsel veya anlamsal değişiklik fark edebilir veya daha önce artık desteklenmeyen bir öznitelik bulabilirsiniz. Örneğin, Uygulama kayıtları (eski) deneyiminde farklı bir adla bilinen [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908)yeni öznitelikleri görürsünüz.

| Uygulama kayıtları (eski)| Uygulama kayıtları           |
|---------------------------|-----------------------------|
| `availableToOtherTenants` | `signInAudience`            |
| `displayName`             | `name`                      |
| `errorUrl`                | -                           |
| `homepage`                | `signInUrl`                 |
| `objectId`                | `Id`                        |
| `publicClient`            | `allowPublicClient`         |
| `replyUrls`               | `replyUrlsWithType`         |

Bu özniteliklerin açıklamaları için [bildirim başvurusu](#manifest-reference) bölümüne bakın.

Daha önce indirilen bir bildirimi karşıya yüklemeye çalıştığınızda, aşağıdaki hatalardan birini görebilirsiniz. Bu hata, bildirim Düzenleyicisi 'nin artık bir şemanın daha yeni bir sürümünü desteklediğinden, bu nedenle karşıya yüklemeye çalıştığınız bir ile eşleşmediği için olasıdır.

* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: geçersiz nesne tanımlayıcısı ' undefined '. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: belirtilen bir veya daha fazla özellik değeri geçersiz. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: güncelleştirme için bu API sürümünde Availabletootherkiracılar ayarlanamaz. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: Bu uygulama için ' replyUrls ' özelliğine yönelik güncelleştirmelere izin verilmiyor. Bunun yerine ' replyUrlsWithType ' özelliğini kullanın. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: tür adı olmayan bir değer bulundu ve beklenen tür yok. Model belirtildiğinde, yükteki her bir değerin, açıkça çağıran tarafından ya da üst değerden dolaylı olarak belirtilen bir türde olması gerekir. []"

Bu hatalardan birini gördüğünüzde, aşağıdaki işlemleri yapmanızı öneririz:

1. Daha önce indirilen bir bildirimi karşıya yüklemek yerine, öznitelikleri bildirim düzenleyicisinde tek tek düzenleyin. İlgilendiğiniz öznitelikleri başarıyla düzenleyebilmeniz için, eski ve yeni özniteliklerin sözdizimini ve semantiğini anlamak için [bildirim başvuru](#manifest-reference) tablosunu kullanın. 
1. İş akışınız, daha sonra kullanmak üzere bildirimleri kaynak deponuza kaydetmenizi gerektiriyorsa, kayıtlı bildirimleri deponuzda **uygulama kayıtları** deneyiminde gördüğünüz bir şekilde yeniden temellendirmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir uygulamanın uygulaması ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz. [Azure AD 'de uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).
* Bazı temel Microsoft kimlik platformu geliştirici kavramlarının tanımları için [Microsoft Identity Platform geliştirici sözlüğü](developer-glossary.md) ' ne bakın.

İçeriğimizi iyileştirmenize ve şekillendirmeye yardımcı olacak geri bildirimler sağlamak için aşağıdaki açıklamalar bölümünü kullanın.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
