---
title: Azure AD kimlik doğrulama & yetkilendirme hata kodları
description: Azure AD güvenlik belirteç hizmetinden (STS) döndürülen AADSTS hata kodları hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: reference
ms.date: 04/07/2020
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 87a962709638391887eaa275f059bf4ceae9218b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406984"
---
# <a name="azure-ad-authentication-and-authorization-error-codes"></a>Azure AD Kimlik Doğrulama ve yetkilendirme hata kodları

Azure Etkin Dizin (Azure AD) güvenlik belirteç hizmetinden (STS) döndürülen AADSTS hata kodları hakkında bilgi mi arıyorsunuz? AADSTS hata açıklamalarını, düzeltmeleri ve önerilen bazı geçici geçici işleri bulmak için bu belgeyi okuyun.

> [!NOTE]
> Bu bilgiler geçicidir ve değiştirilebilir. Bir sorunuz mu var ya da aradığınızı bulamıyor musunuz? Bir GitHub sorunu oluşturun veya [geliştiricilerin](active-directory-develop-help-support.md) yardım ve destek almanın diğer yolları hakkında bilgi edinmeleri için Destek ve Yardım seçeneklerini görün.
>
> Bu belge geliştirici ve yönetici kılavuzu için sağlanır, ancak istemcinin kendisi tarafından asla kullanılmamalıdır. Hata kodları, uygulamalarını yaparken geliştiriciye yardımcı olmak amacıyla daha ayrıntılı hata iletileri sağlamak için herhangi bir zamanda değiştirilebilir. Metin veya hata kodu numaralarına bağımlı olan uygulamalar zaman içinde bozulur.

## <a name="handling-error-codes-in-your-application"></a>Uygulamanızda hata kodlarını işleme

[OAuth2.0 spec](https://tools.ietf.org/html/rfc6749#section-5.2) hata yanıtı `error` bölümünü kullanarak kimlik doğrulama sırasında hataları işlemek için nasıl rehberlik sağlar. 

Örnek bir hata yanıtı aşağıda veda edebilirsiniz:

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://example.contoso.com/activity.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7", 
  "error_uri":"https://login.microsoftonline.com/error?code=70011"
}
```

| Parametre         | Açıklama    |
|-------------------|----------------|
| `error`       | Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılması gereken bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. Kodunuzdaki bir hataya tepki vermek için bu alanı asla kullanmayın. |
| `error_codes` | Tanılamada yardımcı olabilecek STS'ye özgü hata kodlarının listesi.  |
| `timestamp`   | Hatanın oluştuğu saat. |
| `trace_id`    | Tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| `correlation_id` | Bileşenler arasında tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| `error_uri` |  Hata hakkında ek bilgi içeren hata arama sayfasına bağlantı.  Bu yalnızca geliştirici kullanımı içindir, kullanıcılara sunmayın.  Yalnızca hata arama sistemi hata hakkında ek bilgilere sahipolduğunda mevcut - tüm hata ek bilgiler sağlanmış değil.|

Alan `error` birkaç olası değere sahiptir - belirli hatalar (örneğin, `authorization_pending` aygıt kodu [akışında)](v2-oauth2-device-code.md)ve bunlara nasıl tepki vereceğiniz hakkında daha fazla bilgi edinmek için protokol dokümantasyon bağlantılarını ve OAuth 2.0 özelliklerini gözden geçirin.  Bazı yaygın olanlar burada listelenir:

| Hata Kodu         | Açıklama        | İstemci Eylemi    |
|--------------------|--------------------|------------------|
| `invalid_request`  | Eksik gerekli parametre gibi protokol hatası. | İsteği düzeltin ve yeniden gönderin.|
| `invalid_grant`    | Kimlik doğrulama materyalinin bazıları (auth kodu, yenileme belirteci, erişim belirteci, PKCE zorluğu) geçersiz, ayrıayrı, eksik veya başka bir şekilde kullanılamaz | Yeni bir yetkilendirme `/authorize` kodu almak için bitiş noktasına yeni bir istek deneyin.  Uygulamanın protokolleri kullanımını gözden geçirmeyi ve doğrulamayı düşünün. |
| `unauthorized_client` | Kimliği doğrulanan istemci, bu yetkilendirme hibe türünü kullanma yetkisine izin vermez. | Bu genellikle istemci uygulaması Azure AD'de kayıtlı değilse veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| `invalid_client` | İstem kimlik doğrulaması başarısız oldu.  | İstemci kimlik bilgileri geçerli değil. Düzeltmek için, uygulama yöneticisi kimlik bilgilerini güncelleştirir.   |
| `unsupported_grant_type` | Yetkilendirme sunucusu yetkilendirme hibe türünü desteklemez. | İstekteki hibe türünü değiştirin. Bu tür hatalar yalnızca geliştirme sırasında ortaya çıkmalıdır ve ilk sınama sırasında algılanmalıdır. |
| `invalid_resource` | Hedef kaynak yok, Azure AD bulamadığı veya doğru şekilde yapılandırılamadığı için geçersizdir. | Bu, varsa kaynağın kiracıda yapılandırılmadığını gösterir. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir.  Geliştirme sırasında, bu genellikle istenen kapsam adına yanlış kurulum testi kiracı veya bir yazım hatası gösterir. |
| `interaction_required` | İstek kullanıcı etkileşimi gerektirir. Örneğin, ek bir kimlik doğrulama adımı gereklidir. | Kullanıcının gerekli zorlukları tamamlayabilmesi için isteği aynı kaynakla etkileşimli olarak yeniden deneyin.  |
| `temporarily_unavailable` | Sunucu geçici olarak isteği işlemek için çok meşgul. | İsteği yeniden deneyin. İstemci uygulaması, geçici bir durum nedeniyle yanıtının geciktiğini kullanıcıya açıklayabilir. |

## <a name="lookup-current-error-code-information"></a>Güncel hata kodu bilgilerini arama
Hata kodları ve iletiler değiştirilebilir.  En güncel bilgiler için, AADSTS hata açıklamalarını, düzeltmeleri ve önerilen bazı geçici geçici işleri bulmak için `https://login.microsoftonline.com/error` sayfaya bir göz atın.  

Döndürülen hata kodunun sayısal kısmında arama yapın.  Örneğin, "AADSTS16000" hata kodunu aldıysanız, "16000" `https://login.microsoftonline.com/error` için bir arama yapın.  Ayrıca URL'ye hata kodu numarası ekleyerek doğrudan belirli bir `https://login.microsoftonline.com/error?code=16000`hataya bağlantı olabilirsiniz: .

## <a name="aadsts-error-codes"></a>AADSTS hata kodları

| Hata | Açıklama |
|---|---|
| AADSTS16000 | SelectUserAccount - Bu, Azure AD tarafından atılan ve kullanıcının birden çok geçerli SSO oturumu arasından seçim yapmasına olanak tanıyan Kullanıcı Arabirimi ile sonuçlanan bir kesmedir. Bu hata oldukça yaygındır ve belirtilirse `prompt=none` uygulamaya döndürülebilir. |
| AADSTS16001 | UserAccountSelectionGeçersiz - Kullanıcı oturum seçmantığının reddettiği bir döşemeyi tıklattığında bu hatayı görürsünüz. Bu hata tetiklendiğinde, kullanıcının güncelleştirilmiş bir kutucuklar/oturumlar listesinden seçim yaparak veya başka bir hesap seçerek kurtarmasına olanak tanır. Bu hata, bir kod hatası veya yarış durumu nedeniyle oluşabilir. |
| AADSTS16002 | AppSessionSelectionGeçersiz - Uygulama tarafından belirtilen SID gereksinimi karşılanmadı.  |
| AADSTS16003 | SsoUserAccountNotFoundInResourceTenant - Kullanıcının kiracıya açıkça eklenmediğini gösterir. |
| AADSTS17003 | Kimlik BilgisiKeyProvisioningFailed - Azure AD kullanıcı anahtarını sağlayamaz. |
| AADSTS20001 | WsFedSignInResponseError - Federe Kimlik Sağlayıcı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS20012 | WsFedMessageGeçersiz - Federe Kimlik Sağlayıcınızla ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS20033 | FedMetadataInvalidTenantName - Federe Kimlik Sağlayıcınızla ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40008 | OAuth2IdPUnretryableServerError - Federe Kimlik Sağlayıcı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40009 | OAuth2IdPRefreshTokenRedemptionUserError - Federe Kimlik Sağlayıcı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40010 | OAuth2IdPRetryableServerError - Federe Kimlik Sağlayıcı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40015 | OAuth2IdPAuthCodeRedemptionUserError - Federe Kimlik Sağlayıcı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS50000 | TokenIssuanceError - Oturum açma hizmetiyle ilgili bir sorun var. Bu sorunu çözmek için bir [destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md). |
| AADSTS50001 | Geçersiz Kaynak - Kaynak devre dışı bırakılmış veya yok. Erişmeye çalıştığınız kaynağın tam kaynak URL'sini belirttiğinizden emin olmak için uygulamanızın kodunu kontrol edin.  |
| AADSTS50002 | NotAllowedTenant - Oturum açma kiracıüzerinde sınırlı bir proxy erişimi nedeniyle başarısız oldu. Kendi kiracı politikanızsa, bu sorunu gidermek için kısıtlı kiracı ayarlarınızı değiştirebilirsiniz. |
| AADSTS50003 | MissingSigningKey - Oturum açma, eksik imza anahtarı veya sertifikası nedeniyle başarısız oldu. Bunun nedeni, uygulamada yapılandırılan imza anahtarı nın olmaması olabilir. 'de özetlenen kararlara [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured)göz atın. Sorunlar la ilgili hala sorun görüyorsanız, uygulama sahibine veya uygulama yöneticisine başvurun. |
| AADSTS50005 | DevicePolicyError - Kullanıcı, şu anda Koşullu Erişim ilkesi yle desteklenmeyen bir platformdan bir aygıtta oturum açmaya çalıştı. |
| AADSTS50006 | Geçersiz İmza - İmza doğrulaması geçersiz imza nedeniyle başarısız oldu. |
| AADSTS50007 | PartnerEncryptionCertificateMissing - İş ortağı şifreleme sertifikası bu uygulama için bulunamadı. Bunu düzeltmek için Microsoft ile [bir destek bileti açın.](../fundamentals/active-directory-troubleshooting-support-howto.md) |
| AADSTS50008 | GeçersizSamlToken - SAML iddiası belirteçeksik veya yanlış yapılandırılmıştır. Federasyon sağlayıcınıza başvurun. |
| AADSTS50010 | AudienceUriValidationFailed - Hedef kitle URI doğrulama uygulama için hiçbir belirteç hedef kitleleri yapılandırıldı beri başarısız oldu. |
| AADSTS50011 | GeçersizYanıtT - Yanıt adresi eksik, yanlış yapılandırılmış veya uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor.  Çözüm olarak, bu eksik yanıt adresini Azure Etkin Dizin uygulamasına eklemeyi veya Etkin Dizin'de uygulamanızı yönetme iznine sahip birinin bunu sizin için yapmasını sağlayın.|
| AADSTS50012 | Kimlik DoğrulamaFailed - Kimlik doğrulama aşağıdaki nedenlerden biri için başarısız oldu:<ul><li>İmza sertifikasının özadı yetkili değil</li><li>Yetkili konu adı için eşleşen güvenilir bir yetki ilkesi bulunamadı</li><li>Sertifika zinciri geçerli değil</li><li>İmza sertifikası geçerli değil</li><li>İlke kiracı üzerinde yapılandırılmamıştır</li><li>İmza sertifikasının parmak izi onaylanmadı</li><li>İstemci iddiası geçersiz imza içerir</li></ul> |
| AADSTS50013 | Geçersiz İddia - İddia çeşitli nedenlerle geçersizdir - Belirteç veren kuruluş api sürümünü geçerli zaman aralığı içinde (süresi dolmuş -biçimsiz ) eşleştirme yapmaz - Yenileme belirteci birincil yenileme belirteci değildir. |
| AADSTS50014 | GuestUserInPendingState - Kullanıcının kullanımı beklemede bir durumdadır. Konuk kullanıcı hesabı henüz tam olarak oluşturulmadı. |
| AADSTS50015 | ViralUserLegalAgeConsentRequiredState - Kullanıcı yasal yaş grubu onayı gerektirir. |
| AADSTS50017 | CertificateValidationFailed - Sertifika doğrulama başarısız, aşağıdaki nedenlerden dolayı nedenleri:<ul><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Beklenen CrlSegment bulunamıyor</li><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Delta CRL dağıtım noktası, karşılık gelen bir CRL dağıtım noktası olmadan yapılandırılmış</li><li>Zaman açıklığı sorunu nedeniyle geçerli CRL segmentlerini alına</li><li>CRL indirilemiyor</li></ul>Kiracı yöneticisine başvurun. |
| AADSTS50020 | UserYetkisiz - Kullanıcılar bu bitiş noktasını arama yetkisizdir. |
| AADSTS50027 | GeçersizJwtToken - Aşağıdaki nedenlerden dolayı geçersiz JWT belirteci:<ul><li>Bir kerelik anahtar talebi içermiyor, alt talep</li><li>konu tanımlayıcısı uyuşmazlığı</li><li>idToken taleplerinde yinelenen talep</li><li>beklenmeyen veren</li><li>beklenmeyen hedef kitle</li><li>geçerli zaman aralığı içinde değil </li><li>belirteç biçimi doğru değil</li><li>Verenin dış kimlik belirteci imza doğrulaması başarısız oldu.</li></ul> |
| AADSTS50029 | Geçersiz URI - etki alanı adı geçersiz karakterler içeriyor. Kiracı yöneticisine başvurun. |
| AADSTS50032 | WeakRsaKey - Zayıf bir RSA anahtarını kullanmak için hatalı kullanıcı girişimini gösterir. |
| AADSTS50033 | RetryableError - Veritabanı işlemleriyle ilgili olmayan geçici bir hatayı gösterir. |
| AADSTS50034 | UserAccountNotFound - Bu uygulamada oturum açabilmek için hesabın dizine eklenmesi gerekir. |
| AADSTS50042 | UnableToGeneratePairwiseIdentifierWithMissingSalt - Çift yönlü tanımlayıcı oluşturmak için gereken tuz prensipte eksiktir. Kiracı yöneticisine başvurun. |
| AADSTS50043 | UnableToGeneratePairwiseIdentifierWithMultipleSalts |
| AADSTS50048 | SubjectMismatchesIssuer - Konu müşteri iddiasında İhraççı iddia uyuşmazlıkları. Kiracı yöneticisine başvurun. |
| AADSTS50049 | NoSuchInstanceForDiscovery - Bilinmeyen veya geçersiz örnek. |
| AADSTS50050 | MalformedDiscoveryRequest - Istek yanlış biçimlendirilmiştir. |
| AADSTS50053 | IdsLocked - Kullanıcı yanlış bir kullanıcı kimliği veya parolayla çok fazla oturum açmayı denediği için hesap kilitlendi. |
| AADSTS50055 | GeçersizPasswordExpiredPassword - Parolanın süresi doldu. |
| AADSTS50056 | Geçersiz veya null şifre -Parola bu kullanıcı için mağaza yok. |
| AADSTS50057 | UserDisabled - Kullanıcı hesabı devre dışı bırakılır. Hesap bir yönetici tarafından devre dışı bırakıldı. |
| AADSTS50058 | UserInformationNotProvided - Bu, kullanıcının oturum açmış olmadığı anlamına gelir. Bu, bir kullanıcının kimliği doğrulanmadığında ve henüz oturum açmış değilken beklenen yaygın bir hatadır.</br>Bu hata, kullanıcının daha önce oturum açmış olduğu bir SSO bağlamında teşvik edilirse, bu SSO oturumunun bulunamadığı veya geçersiz olduğu anlamına gelir.</br>İstem=hiçbiri belirtilmemişse, bu hata uygulamaya döndürülebilir. |
| AADSTS50059 | MissingTenantRealmAndNoUserInformationProvided - Kiracı tanımlayıcı bilgiler istekte bulunamadı veya sağlanan herhangi bir kimlik bilgisi tarafından ima edilmedi. Kullanıcı, sorunu çözmeye yardımcı olmak için kiracı yöneticiyle iletişime geçebilir. |
| AADSTS50061 | SignoutInvalidRequest - Oturum açma isteği geçersizdir. |
| AADSTS50064 | Kimlik Doğrulama Hatası - Kullanıcı adı veya parola üzerinde kimlik doğrulama başarısız oldu. |
| AADSTS50068 | SignoutInitiatorNotParticipant - Signout başarısız oldu. Oturum açma işlemini başlatan uygulama geçerli oturumda katılımcı değildir. |
| AADSTS50070 | SignoutUnknownSessionIdentifier - Signout başarısız oldu. Oturum açma isteği, varolan oturum(lar) ile eşleşmeyan bir ad tanımlayıcısı belirtmiş. |
| AADSTS50071 | SignoutMessageExpired - oturum açma isteği nin süresi doldu. |
| AADSTS50072 | UserStrongAuthEnrollmentRequiredInterrupt - Kullanıcı ikinci faktör kimlik doğrulama (etkileşimli) için kayıt gerekir. |
| AADSTS50074 | UserStrongAuthClientAuthNRequiredInterrupt - Güçlü kimlik doğrulaması gereklidir ve kullanıcı MFA meydan geçmedi. |
| AADSTS50076 | UserStrongAuthClientAuthNRequired - Yönetici tarafından yapılan bir yapılandırma değişikliği nedeniyle veya yeni bir konuma taşındığınız için, kullanıcıkaynağa erişmek için çok faktörlü kimlik doğrulamasını kullanmalıdır. Kaynak için yeni bir yetkilendirme isteğiyle yeniden deneyin. |
| AADSTS50079 | UserStrongAuthEnrollmentRequired - Yönetici tarafından yapılan bir yapılandırma değişikliği nedeniyle veya kullanıcı yeni bir konuma taşındığı için, kullanıcının çok faktörlü kimlik doğrulamasını kullanması gerekir. |
| AADSTS50085 | Yenileme belirteci için sosyal IDP oturum açma bilgileri gerekiyor. Kullanıcıdan kullanıcı adı-parola ile yeniden oturum açmasını isteyin |
| AADSTS50086 | SasNonRetryableHatası |
| AADSTS50087 | SasRetryableError - Hizmet geçici olarak kullanılamıyor. Yeniden Deneyin. |
| AADSTS50089 | Akış belirtecinin süresi doldu - Kimlik Doğrulaması Başarısız Oldu. Kullanıcının kullanıcı adı -parola ile yeniden oturum açmayı denemesini sorun. |
| AADSTS50097 | DeviceAuthenticationGerekli - Aygıt kimlik doğrulaması gereklidir. |
| AADSTS50099 | PKeyAuthInvalidJwtYetkisiz - JWT imzası geçersizdir. |
| AADSTS50105 | EntitlementGrantsNotFound - İmzalanan kullanıcı, oturum açmış uygulama için bir role atanmadı. Kullanıcıyı uygulamaya atayın. Daha fazla[https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)bilgi için: . |
| AADSTS50107 | InvalidRealmUri - İstenen federasyon alemnesnesi yok. Kiracı yöneticisine başvurun. |
| AADSTS50120 | ThresholdJwtInvalidJwtFormat - JWT üstbilgiile sorun. Kiracı yöneticisine başvurun. |
| AADSTS50124 | ClaimsTransformationInvalidInputParameter - Claims Transformation geçersiz giriş parametresi içerir. İlke güncelleştirmek için kiracı yöneticisine başvurun. |
| AADSTS50125 | PasswordResetRegistrationRequiredInterrupt - Oturum açma, parola sıfırlama veya parola kayıt girişi nedeniyle kesildi. |
| AADSTS50126 | GeçersizUserNameOrPassword - Geçersiz kullanıcı adı veya parola nedeniyle kimlik bilgilerini doğrulama hatası. |
| AADSTS50127 | BrokerAppNotInstalled - Kullanıcı bu içeriğe erişmek için bir broker uygulaması yüklemeniz gerekir. |
| AADSTS50128 | Geçersiz alan adı - İstekte kiracı tanımlayıcı bilgi bulunmaz veya sağlanan kimlik bilgileri tarafından ima edilmez. |
| AADSTS50129 | DeviceIsNotWorkplaceJoined - Cihazı kaydetmek için İşyeri birleştirme gereklidir. |
| AADSTS50131 | ConditionalAccessFailed - Hatalı Windows aygıt durumu, şüpheli etkinlik nedeniyle engellenen istek, erişim ilkesi veya güvenlik ilkesi kararları gibi çeşitli Koşullu Erişim hatalarını gösterir. |
| AADSTS50132 | SsoArtifactInvalidOrExpired - Oturum, parola süresi nin sona ermesi veya son parola değişikliği nedeniyle geçerli değildir. |
| AADSTS50133 | SsoArtifactRevoked - Oturum, parola süresinin dolması veya en son parola değişikliği nedeniyle geçerli değildir. |
| AADSTS50134 | DeviceFlowAuthorizeWrongDatacenter - Yanlış veri merkezi. OAuth 2.0 cihaz akışındaki bir uygulama tarafından başlatılan bir isteği yetkilendirmek için, yetkili tarafın orijinal isteğin bulunduğu veri merkezinde olması gerekir. |
| AADSTS50135 | PasswordChangeCompromisedPassword - Hesap riski nedeniyle parola değişikliği gereklidir. |
| AADSTS50136 | RedirectMsaSessionToApp - Tek MSA oturumu algılandı. |
| AADSTS50139 | SessionMissingMsaOAuth2RefreshToken - Oturum eksik bir harici yenileme belirteci nedeniyle geçersizdir. |
| AADSTS50140 | KmsiInterrupt - Bu hata, kullanıcı oturum açarken "Beni oturum açma" kesintisi nedeniyle oluştu. Daha fazla bilgi almak için Bağıntı Kimliği, İstek Kimliği ve Hata kodu ile [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md). |
| AADSTS50143 | Oturum uyuşmazlığı - Kullanıcı kiracı farklı kaynak nedeniyle etki alanı ipucu eşleşmediğinden oturum geçersizdir. Daha fazla ayrıntı almak için Korelasyon Kimliği, İstek Kimliği ve Hata koduyla  [bir destek bileti açın.](../fundamentals/active-directory-troubleshooting-support-howto.md) |
| AADSTS50144 | GeçersizPasswordExpiredOnPremPassword - Kullanıcının Active Directory şifresi nin süresi doldu. Kullanıcı için yeni bir parola oluşturun veya kullanıcının parolasını sıfırlamak için self servis sıfırlama aracını kullanmasını sağlar. |
| AADSTS50146 | EksikCustomSigningKey - Bu uygulama nın uygulamaya özgü bir imzalama anahtarıyla yapılandırılması gerekir. Bu şekilde yapılandırılmamış veya anahtarın süresi dolmuş ya da anahtar henüz geçerli değil. |
| AADSTS50147 | MissingCodeChallenge - Kod zorparametresinin boyutu geçerli değildir. |
| AADSTS50155 | DeviceAuthenticationFailed - Aygıt kimlik doğrulaması bu kullanıcı için başarısız oldu. |
| AADSTS50158 | ExternalSecurityChallenge - Dış güvenlik sorunu tatmin olmadı. |
| AADSTS50161 | GeçersizExternalSecurityChallengeConfiguration - Harici sağlayıcı tarafından gönderilen talepler yeterli değildir veya dış sağlayıcıya istenen eksik talep. |
| AADSTS50166 | ExternalClaimsProviderThrottled - Talebi talep sağlayıcıya gönderemedi. |
| AADSTS50168 | ChromeBrowserSsoInterruptRequired - İstemci, Windows 10 Hesapları uzantısı üzerinden bir SSO belirteci elde etme yeteneğine sahiptir, ancak istekte belirteç bulunamadı veya verilen belirteç süresi doldu. |
| AADSTS50169 | InvalidRequestBadRealm - Bölge, geçerli hizmet ad alanının yapılandırılmış bir alanı değildir. |
| AADSTS50170 | MissingExternalClaimsProviderMapping - Dış denetimler eşleme eksik. |
| AADSTS50177 | ExternalChallengeNotSupportedForPassthroughUsers - Harici sorun passthrough kullanıcılar için desteklenmez. |
| AADSTS50178 | SessionControlNotSupportedForPassthroughUsers - Oturum denetimi geçiş kullanıcıları için desteklenmez. |
| AADSTS50180 | WindowsIntegratedAuthMissing - Tümleşik Windows kimlik doğrulaması gereklidir. Sorunsuz SSO için kiracıyı etkinleştirin. |
| AADSTS50187 | DeviceInformationNotProvided - Hizmet aygıt kimlik doğrulaması gerçekleştirmek için başarısız oldu. |
| AADSTS50196 | LoopDetected - Bir istemci döngüsü algılandı. Belirteç önbelleğe almanın uygulandığından ve hata koşullarının doğru şekilde işlendiğinden emin olmak için uygulamanın mantığını denetleyin.  Uygulama, hatalı bir durumda olduğunu veya kötü amaçlı belirteçler talep ettiğini belirten, çok kısa bir süre içinde aynı istek çok fazla yaptı. |
| AADSTS50197 | Çakışan Kimlikler - Kullanıcı bulunamadı. Tekrar oturum açmayı dene. |
| AADSTS50199 | CmsiInterrupt - Güvenlik nedenleriyle, bu istek için kullanıcı onayı gereklidir.  Bu bir "interaction_required" hatası olduğundan, istemci etkileşimli auth yapmalıdır.  Bunun nedeni, bir sistem web görünümünün yerel bir uygulama için belirteç istemek için kullanılmasından kaynaklansa, kullanıcıdan bunun gerçekten oturum açmaları gereken uygulama olup olmadığını sorması istenir.|
| AADSTS51000 | GerekliÖzellikNotEnabled - Özellik devre dışı bırakılır. |
| AADSTS51001 | DomainHintMustbePresent - Etki alanı ipucu şirket içi güvenlik tanımlayıcısı veya şirket içi UPN ile birlikte bulunmalıdır. |
| AADSTS51004 | UserAccountNotInDirectory - Kullanıcı hesabı dizinde yok. |
| AADSTS51005 | TemporaryRedirect - İstenen bilgilerin konum üstbilgisinde belirtilen URI'de bulunduğunu gösteren HTTP durum 307'ye eşdeğerdir. Bu durumu aldığınızda, yanıtla ilişkili konum üstbilgisini izleyin. Özgün istek yöntemi POST olduğunda, yeniden yönlendirilen istek POST yöntemini de kullanır. |
| AADSTS51006 | ForceReauthDueToInsufficientAuth - Tümleşik Windows kimlik doğrulaması gereklidir. Kullanıcı, Tümleşik Windows kimlik doğrulama iddiasını eksik olan bir oturum belirteci kullanarak oturum açtı. Kullanıcıdan yeniden oturum açmasını isteyin. |
| AADSTS52004 | DelegationDoesNotExistForLinkedIn - Kullanıcı LinkedIn kaynaklarına erişim için izin vermemiştir. |
| AADSTS53000 | DeviceNotCompliant - Koşullu Erişim ilkesi uyumlu bir aygıt gerektirir ve aygıt uyumlu değildir. Kullanıcı, cihazını Intune gibi onaylanmış bir MDM sağlayıcısına kaydetmelidir. |
| AADSTS53001 | DeviceNotDomainJoined - Koşullu Erişim ilkesi bir etki alanı birleştirilmiş aygıt gerektirir ve aygıt etki alanı birleştirilmiş değildir. Kullanıcının bir etki alanı birleştirilmiş aygıt kullanmasını sağlar. |
| AADSTS53002 | ApplicationUsedIsNotAnApprovedApp - Kullanılan uygulama Koşullu Erişim için onaylanmış bir uygulama değildir. Kullanıcının erişim için kullanmak üzere onaylanmış uygulamalar listesindeki uygulamalardan birini kullanması gerekir. |
| AADSTS53003 | BlockedByConditionalAccess - Access Koşullu Erişim ilkeleri tarafından engellendi. Erişim ilkesi belirteç verilmesine izin vermez. |
| AADSTS53004 | ProofUpBlockedDueToRisk - Kullanıcının bu içeriğe erişmeden önce çok faktörlü kimlik doğrulama kayıt işlemini tamamlaması gerekir. Kullanıcının çok faktörlü kimlik doğrulamasına kaydolması gerekir. |
| AADSTS54000 | MinorUserBlockedLegalAgeGroupRule |
| AADSTS65001 | DelegationDoesNotExist - Kullanıcı veya yönetici kimliği X ile uygulamayı kullanmaya izin vermedi. |
| AADSTS65004 | UserDeclinedConsent - Kullanıcı uygulamaya erişmeye izin vermeyi reddetti. Kullanıcıdan oturum açmayı yeniden denemesini ve uygulamaya izin vermesini isteyin|
| AADSTS65005 | Yanlış Yapılandırılmış Uygulama - Kaynak erişim listesi gerekli kaynak tarafından keşfedilebilir uygulamalar içermez veya istemci uygulaması gerekli kaynak erişim listesinde belirtilmeyen kaynağa erişim istedi veya Graph hizmeti kötü isteği iade etti veya kaynak bulunamadı. Uygulama SAML'yi destekliyorsa, uygulamayı yanlış Tanımlayıcı (Entity) ile yapılandırmış olabilirsiniz. Aşağıdaki bağlantıyı kullanarak SAML için listelenen çözünürlüğü deneyin:[https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav) |
| AADSTS67003 | AktörNotValidServiceIdentity |
| AADSTS70000 | GeçersizHibe - Kimlik doğrulama başarısız oldu. Yenileme belirteci geçerli değildir. Hata aşağıdaki nedenlerden dolayı olabilir:<ul><li>Belirteç bağlama üstbilgiboş</li><li>Belirteç bağlama karma eşleşmiyor</li></ul> |
| AADSTS70001 | Yetkisiz İstemci - Uygulama devre dışı bırakılır. |
| AADSTS70002 | Geçersiz İstemci - Kimlik bilgilerini doğrulayan hata. Belirtilen client_secret bu istemci için beklenen değerle eşleşmiyor. client_secret düzeltin ve yeniden deneyin. Daha fazla bilgi için [bkz.](v2-oauth2-auth-code-flow.md#request-an-access-token) |
| AADSTS70003 | DesteklenmeyenGrantType - Uygulama desteklenmeyen bir hibe türünü döndürür. |
| AADSTS70004 | InvalidRedirectUri - Uygulama geçersiz bir yeniden yönlendirme URI döndü. İstemci tarafından belirtilen yeniden yönlendirme adresi, yapılandırılmış bir adresle veya OIDC onay listesindeki herhangi bir adresle eşleşmiyor. |
| AADSTS70005 | DesteklenmeyenYanıtTürü - Uygulama aşağıdaki nedenlerden dolayı desteklenmeyen bir yanıt türünü döndürdü:<ul><li>uygulama için yanıt türü 'belirteç' etkin değil</li><li>'id_token' yanıt türü 'OpenID' kapsamı gerektiriyor -kodlanmış wctx içinde desteklenmeyen bir OAuth parametre değeri içeriyor</li></ul> |
| AADSTS70007 | DesteklenmeyenYanıt Modu - Uygulama, bir belirteç `response_mode` isteğinde bulunulmamış bir değeri döndürür.  |
| AADSTS70008 | ExpiredOrRevokedGrant - Yenileme belirteci hareketsizlik nedeniyle sona erdi. Belirteç XXX'de yayınlandı ve belirli bir süre etkin değildi. |
| AADSTS70011 | GeçersizKapsam - Uygulama tarafından istenen kapsam geçersizdir. |
| AADSTS70012 | MsaServerError - Bir MSA (tüketici) kullanıcısının kimliğini doğrularken bir sunucu hatası oluştu. Yeniden Deneyin. Başarısız olmaya devam ederse, [destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) |
| AADSTS70016 | YetkilendirmeBekleyen - OAuth 2.0 cihaz akışı hatası. Yetkilendirme beklemede. Aygıt, isteği yeniden yoklamaya çalışacaktır. |
| AADSTS70018 | BadVerificationCode - Kullanıcının cihaz kodu akışı için yanlış kullanıcı kodu yazması nedeniyle geçersiz doğrulama kodu. Yetkilendirme onaylanmadı. |
| AADSTS70019 | CodeExpired - Doğrulama kodunun süresi doldu. Kullanıcının oturum açmayı yeniden denemesini önle. |
| AADSTS75001 | BindingSerializationError - SAML ileti bağlama sırasında bir hata oluştu. |
| AADSTS75003 | Desteklenmeyen BindingError - Uygulama desteklenmeyen bağlama ile ilgili bir hata döndürür (SAML protokol yanıtı HTTP POST dışındaki ciltler aracılığıyla gönderilemez). |
| AADSTS75005 | Saml2MessageGeçersiz - Azure AD, uygulama tarafından SSO için gönderilen SAML isteğini desteklemez. |
| AADSTS75008 | RequestDeniedError - SAML isteği beklenmeyen bir hedef olduğu için uygulamadan gelen istek reddedildi. |
| AADSTS75011 | NoMatchedAuthnContextInOutputClaims - Kullanıcının hizmetle kimlik doğrulaması için istenen kimlik doğrulama yöntemiyle eşleşmediği kimlik doğrulama yöntemi. |
| AADSTS75016 | Saml2AuthenticationRequestInvalidNameIDPolicy - SAML2 Kimlik Doğrulama İsteği geçersiz NameIdPolicy vardır. |
| AADSTS80001 | OnPremiseStoreNotAvailable - Kimlik Doğrulama Aracısı Active Directory'ye bağlanamıyor. Aracı sunucularının, parolalarının doğrulanması gereken ve Active Directory'ye bağlanabilen kullanıcılarla aynı AD ormanına üye olduğundan emin olun. |
| AADSTS80002 | OnPremisePasswordValidatorRequestTimedout - Parola doğrulama isteği zaman doldu. Etkin Dizin'in kullanılabilir olduğundan ve aracılardan gelen isteklere yanıt verdiğinden emin olun. |
| AADSTS80005 | OnPremisePasswordValidatorUnpredictableWebException - Kimlik Doğrulama Aracısı yanıtı işlerken bilinmeyen bir hata oluştu. İsteği yeniden deneyin. Başarısız olmaya devam ederse, hata hakkında daha fazla bilgi almak için [bir destek bileti açın.](../fundamentals/active-directory-troubleshooting-support-howto.md) |
| AADSTS80007 | OnPremisePasswordValidatorErrorOccurredOnPrem - Kimlik Doğrulama Aracısı kullanıcının parolasını doğrulayamıyor. Daha fazla bilgi için aracı günlüklerini denetleyin ve Active Directory'nin beklendiği gibi çalıştığını doğrulayın. |
| AADSTS80010 | OnPremisePasswordValidationEncryptionException - Kimlik Doğrulama Aracısı parolanın şifresini çözemiyor. |
| AADSTS80012 | OnPremisePasswordValidationAccountLogonInvalidHours - Kullanıcılar izin verilen saatler dışında oturum açmaya çalıştı (bu AD'de belirtilir). |
| AADSTS80013 | OnPremisePasswordValidationTimeSkew - Kimlik doğrulama girişimi, kimlik doğrulama aracısını çalıştıran makine ile AD arasındaki zaman çarpıklığını nedeniyle tamamlanamadı. Zaman eşitleme sorunlarını düzeltin. |
| AADSTS81004 | DesktopSsoIdentityInTicketIsNotAuthenticated - Kerberos kimlik doğrulama girişimi başarısız oldu. |
| AADSTS81005 | DesktopSsoAuthenticationPackageNotSupported - Kimlik doğrulama paketi desteklenmez. |
| AADSTS81006 | DesktopSsoNoAuthorizationHeader - Yetkilendirme üstbilgisi bulunamadı. |
| AADSTS81007 | DesktopSsoTenantIsNotOptIn - Kiracı Sorunsuz SSO için etkin değildir. |
| AADSTS81009 | DesktopSsoAuthorizationHeaderValueWithBadFormat - Kullanıcının Kerberos biletini doğrulayamıyor. |
| AADSTS81010 | DesktopSsoAuthTokenInvalid - Kullanıcının Kerberos biletinin süresi dolduğu veya geçersiz olduğu için Sorunsuz SSO başarısız oldu. |
| AADSTS81011 | DesktopSsoLookupUserBySidFailed - Kullanıcının Kerberos biletindeki bilgilere dayanarak kullanıcı nesnesi bulunamıyor. |
| AADSTS81012 | DesktopSsoMismatchBetweenTokenUpnAndChosenUpn - Azure AD'de oturum açmaya çalışan kullanıcı, aygıtta oturum açmış olan kullanıcıdan farklıdır. |
| AADSTS90002 | GeçersizTenantName - Kiracı adı veri deposunda bulunamadı. Doğru kiracı kimliğine sahip olup olmadığını kontrol edin. |
| AADSTS90004 | GeçersizRequestFormat - İstek düzgün biçimlendirilmemiştir. |
| AADSTS90005 | GeçersizİstekMultipleRequirements - İstek tamamlayamıyor. Tanımlayıcı ve giriş ipucu birlikte kullanılamadığıiçin istek geçerli değildir.  |
| AADSTS90006 | ExternalServerRetryableError - Hizmet geçici olarak kullanılamıyor.|
| AADSTS90007 | GeçersizSessionId - Kötü istek. Geçirilen oturum kimliği ayrıştısı olamaz. |
| AADSTS90008 | TokenForItselfRequiresGraphPermission - Kullanıcı veya yönetici uygulamayı kullanmaya izin vermedi. En azından uygulama, oturum açma ve okuma kullanıcı profili iznini belirterek Azure AD'ye erişim gerektirir. |
| AADSTS90009 | TokenForItselfMissingIdenticalAppIdentifier - Uygulama kendisi için bir belirteç talep ediyor. Bu senaryo yalnızca belirtilen kaynak GUID tabanlı uygulama kimliğini kullanıyorsa desteklenir. |
| AADSTS90010 | NotSupported - Algoritma oluşturulamıyor. |
| AADSTS90012 | RequestTimeout - İstenilen zaman doldu. |
| AADSTS90013 | InvalidUserInput - Kullanıcıdan giriş geçerli değildir. |
| AADSTS90014 | MissingRequiredField - Bu hata kodu, kimlik bilgisi nde beklenen bir alan olmadığında çeşitli durumlarda görünebilir. |
| AADSTS90015 | QueryStringTooLong - Sorgu dizesi çok uzun. |
| AADSTS90016 | MissingRequiredClaim - Erişim belirteci geçerli değildir. Gerekli talep eksik. |
| AADSTS90019 | MissingTenantRealm - Azure AD, istekteki kiracı tanımlayıcısını belirleyemedi. |
| AADSTS90022 | AuthenticatedInvalidPrincipalNameFormat - Asıl ad biçimi geçerli değildir veya `name[/host][@realm]` beklenen biçimi karşılamaz. Ana ad gereklidir, ana bilgisayar ve bölge isteğe bağlıdır ve null olarak ayarlanabilir. |
| AADSTS90023 | Geçersiz İstek - Kimlik doğrulama hizmeti isteği geçerli değildir. |
| AADSTS9002313 | Geçersiz İstek - İstek yanlış bilgilendirilmiş veya geçersizdir. - Burada sorun belirli bir bitiş noktasına istek ile yanlış bir şey vardı çünkü. Bu soruna öneri, hata nın bir kemancı izleme sini almak ve isteğin gerçekten düzgün biçimlendirilmiş olup olmadığını görmek için bakmaktır. |
| AADSTS90024 | RequestBudgetExceededError - Geçici bir hata oluştu. Yeniden Deneyin. |
| AADSTS90033 | MsodsServiceUnavailable - Microsoft Online Directory Service (MSODS) kullanılamıyor. |
| AADSTS90036 | MsodsServiceUnretryableFailure - MSODS tarafından barındırılan WCF hizmetinden beklenmeyen, yeniden denilemez bir hata oluştu. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md). |
| AADSTS90038 | NationalCloudTenantRedirection - Belirtilen kiracı 'Y' Ulusal Bulut 'X'e aittir. Geçerli bulut örneği 'Z' X ile fetorat değildir. Bulut yönlendirme hatası döndürülür. |
| AADSTS90043 | NationalCloudAuthCodeRedirection - Özellik devre dışı bırakılır. |
| AADSTS90051 | GeçersizNationalCloudId - Ulusal bulut tanımlayıcısı geçersiz bir bulut tanımlayıcısı içerir. |
| AADSTS90055 | TenantThrottlingError - Çok fazla gelen istek var. Bu özel durum, engellenen kiracılar için atılır. |
| AADSTS90056 | BadResourceRequest - Kodu erişim belirteci için kullanmak için uygulamanın `/token` bitiş noktasına bir POST isteği göndermesi gerekir. Ayrıca, bundan önce, bir yetkilendirme kodu sağlamalı ve `/token` posta isteğinde bitiş noktasına göndermelisiniz. OAuth 2.0 yetki kodu akışına genel bir [https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code)bakış için bu makaleye bakın: . Kullanıcıyı, authorization_code `/authorize` döndürecek bitiş noktasına yönlendirin. Bir isteği `/token` bitiş noktasına göndererek, kullanıcı erişim belirteci alır. Azure portalında oturum açın ve iki uç noktanın doğru şekilde yapılandırıldığını doğrulamak için **Uygulama kayıtlarını > Bitiş Noktaları'nı** kontrol edin. |
| AADSTS90072 | PassThroughUserMfaError - Kullanıcının oturum imzaladığı harici hesap, oturum açtıkça oturum açabildikleri kiracıda bulunmaz; böylece kullanıcı kiracı için MFA gereksinimlerini karşılamıyor. Hesap, önce kiracıya harici bir kullanıcı olarak eklenmelidir. Farklı bir Azure AD kullanıcı hesabıyla oturum açın ve oturum açın. |
| AADSTS90081 | OrgIdWsFederationMessageGeçersiz - Hizmet bir WS-Federation iletisini işlemeye çalıştığında bir hata oluştu. İleti geçerli değil. |
| AADSTS90082 | OrgIdWsFederationNotSupported - İstek için seçilen kimlik doğrulama ilkesi şu anda desteklenmiyor. |
| AADSTS90084 | OrgIdWsFederationGuestNotAllowed - Konuk hesapları bu site için izin verilmez. |
| AADSTS90085 | OrgIdWsFederationSltRedemptionFailed - Şirket nesnesi henüz sağlanmadı çünkü hizmet bir belirteç vermek mümkün değildir. |
| AADSTS90086 | OrgIdWsTrustDaTokenExpired - Kullanıcı DA belirteci süresi doldu. |
| AADSTS90087 | OrgIdWsFederationMessageCreationFromUriFailed - URI'den WS-Federasyon iletisi oluşturulurken bir hata oluştu. |
| AADSTS90090 | GraphRetryableError - Hizmet geçici olarak kullanılamıyor. |
| AADSTS90091 | GraphServiceErişilemez |
| AADSTS90092 | GraphNonRetryableError |
| AADSTS90093 | GraphUserYetkisiz - Grafik istek için yasak bir hata kodu ile döndürülür. |
| AADSTS90094 | AdminConsentRequired - Yönetici onayı gereklidir. |
| AADSTS900382 | Gizli İstemci, Cross Cloud isteğinde desteklenmez. |
| AADSTS90100 | GeçersizİstekParametresi - Parametre boş veya geçerli değil. |
| AADSTS901002 | AADSTS901002: 'kaynak' istek parametresi desteklenmez. |
| AADSTS90101 | GeçersizEmailAddress - Sağlanan veriler geçerli bir e-posta adresi değildir. E-posta adresi biçiminde `someone@example.com`olmalıdır. |
| AADSTS90102 | GeçersizUriParametre - Değer geçerli bir mutlak URI olmalıdır. |
| AADSTS90107 | InvalidXml - İstek geçerli değildir. Verilerinizin geçersiz karakterlere sahip olmadığından emin olun.|
| AADSTS90114 | Geçersiz Vade Tarihi - Toplu belirteç sona erme zaman damgası süresi dolmuş bir belirteç verilmesine neden olur. |
| AADSTS90117 | Geçersizİstek Girişi |
| AADSTS90119 | GeçersizUserCode - Kullanıcı kodu boş veya boştur.|
| AADSTS90120 | GeçersizDeviceFlowRequest - İstek zaten yetkilendirildi veya reddedildi. |
| AADSTS90121 | GeçersizBoş İstek - Geçersiz boş istek.|
| AADSTS90123 | IdentityProviderAccessDenied - Kimlik veya talep verme sağlayıcısı isteği reddettiği için belirteç düzenlenemez. |
| AADSTS90124 | V1ResourceV2GlobalEndpointNotSupported - Kaynak `/common` veya `/consumers` uç noktaları üzerinde desteklenmez. Bunun `/organizations` yerine kiracıya özgü bitiş noktasını kullanın. |
| AADSTS90125 | DebugModeEnrollTenantNotFound - Kullanıcı sistemde değil. Kullanıcı adını doğru girdiğinden emin olun. |
| AADSTS90126 | DebugModeEnrollTenantNotInferred - Kullanıcı türü bu uç noktada desteklenmez. Sistem, kullanıcının kiracısını kullanıcı adından çıkaramaz. |
| AADSTS90130 | NonConvergedAppV2GlobalEndpointNotSupported - Uygulama `/common` veya `/consumers` uç noktalar üzerinden desteklenmez. Bunun `/organizations` yerine kiracıya özgü bitiş noktasını kullanın. |
| AADSTS120000 | PasswordChangeIncorrectCurrentPassword |
| AADSTS120002 | PasswordChangeInvalidNewPasswordWeak |
| AADSTS120003 | PasswordChangeInvalidNewPasswordContainsMemberName |
| AADSTS120004 | PasswordChangeOnPremComplexity |
| AADSTS120005 | PasswordChangeOnPremSuccessCloudFail |
| AADSTS120008 | PasswordChangeAsyncJobStateTerminated - Yeniden denilemez bir hata oluştu.|
| AADSTS120011 | PasswordChangeAsyncUpnInferenceFailed |
| AADSTS120012 | PasswordChangeNeedsToHappenOnPrem |
| AADSTS120013 | PasswordChangeOnPremisesConnectivityFailure |
| AADSTS120014 | PasswordChangeOnPremUserAccountLockedOutOrDisabled |
| AADSTS120015 | PasswordChangeADAdminActionGerekli |
| AADSTS120016 | PasswordChangeUserNotFoundBySspr |
| AADSTS120018 | PasswordChangePasswordDoesnotComplyFuzzyPolicy |
| AADSTS120020 | PasswordChangeFailure |
| AADSTS120021 | PartnerServiceSsprInternalServiceError |
| AADSTS130004 | NgcKeyNotFound - Kullanıcı ilkesi, NGC ID anahtarını yapılandırmadı. |
| AADSTS130005 | NgcInvalidSignature - NGC anahtar imzası doğrulandı başarısız oldu.|
| AADSTS130006 | NgcTransportKeyNotFound - NGC taşıma anahtarı cihazda yapılandırılmamıştır. |
| AADSTS130007 | NgcDeviceIsDisabled - Cihaz devre dışı bırakıldı. |
| AADSTS130008 | NgcDeviceIsNotFound - NGC anahtarıtarafından başvurulan cihaz bulunamadı. |
| AADSTS135010 | Keynotfound |
| AADSTS140000 | GeçersizRequestNonce - İstek nonce sağlanmaz. |
| AADSTS140001 | GeçersizSessionKey - Oturum anahtarı geçerli değildir.|
| AADSTS165900 | GeçersizApiRequest - Geçersiz istek. |
| AADSTS220450 | DesteklenmeyenAndroidWebViewVersion - Chrome WebView sürümü desteklenmez. |
| AADSTS220501 | GeçersizCrlDownload |
| AADSTS221000 | DeviceOnlyTokensNotSupportedByResource - Kaynak yalnızca aygıt belirteçlerini kabul etmek üzere yapılandırılmamıştır. |
| AADSTS240001 | BulkAADJTokenYetkisiz - Kullanıcının Azure AD'ye cihaz kaydetme yetkisi yok. |
| AADSTS240002 | RequiredClaimIsMissing - id_token hibe olarak `urn:ietf:params:oauth:grant-type:jwt-bearer` kullanılamaz.|
| AADSTS530032 | BlockedByConditionalAccessOnSecurityPolicy - Kiracı yönetici bu isteği engelleyen bir güvenlik ilkesi yapılandırıldı. İsteğinizin ilke gereksinimlerini karşılayıp karşılamadığını belirlemek için kiracı düzeyinde tanımlanan güvenlik ilkelerini denetleyin. |
| AADSTS700016 | UnauthorizedClient_DoesNotMatchRequest - Uygulama dizinde/kiracıda bulunamadı. Bu, uygulama kiracının yöneticisi tarafından yüklenmediyse veya kiracıdaki herhangi bir kullanıcı tarafından onaylanmışsa gerçekleşebilir. Uygulama için tanımlayıcı değerini yanlış yapılandırılmış veya kimlik doğrulama isteğinizi yanlış kiracıya göndermiş olabilirsiniz. |
| AADSTS700020 | Etkileşim Gerekli - Erişim izni etkileşim gerektirir. |
| AADSTS700022 | GeçersizMultipleResourcesScope - Giriş parametresi kapsamı için sağlanan değer, birden fazla kaynak içerdiğinden geçerli değildir. |
| AADSTS700023 | GeçersizKaynakSızKapsam - Giriş parametresi kapsamı için sağlanan değer, erişim jetonu istediğinde geçerli değildir. |
| AADSTS7000222| GeçersizClientSecretExpiredKeysProvided - Sağlanan istemci gizli anahtarları süresi doldu. Uygulamanız için yeni anahtarlar oluşturmak veya daha fazla güvenlik için sertifika kimlik bilgilerini kullanmayı düşünmek için Azure Portalı'nı ziyaret edin:https://aka.ms/certCreds |
| AADSTS700005 | InvalidGrantRedeemAgainstWrongTenant - Sağlanan Yetki Kodu diğer kiracı karşı kullanmak için tasarlanmıştır, böylece reddedildi. OAuth2 Yetki Kodu, uygun olduğu şekilde (/ortak veya /{kiracı-ID} için satın alındığı aynı kiracıya karşı kullanılmalıdır. |
| AADSTS1000000 | UserNotBoundError - Bind API, Azure AD kullanıcısının henüz gerçekleşmemiş harici bir IDP ile de kimlik doğrulamasını gerektirir. |
| AADSTS1000002 | BindCompleteInterruptError - Bağlama başarıyla tamamlandı, ancak kullanıcı bilgilendirilmelidir. |
| AADSTS7000112 | YetkisizClientApplicationDisabled - Uygulama devre dışı bırakılır. |
| AADSTS7500529 | 'SAMLId-Guid' değeri geçerli bir SAML Kimliği değildir - Azure AD bu özniteliği, döndürülen yanıtın Yanıt Yanıtı özniteliğini doldurmak için kullanır. Kimlik bir sayıyla başlamamalıdır, bu nedenle ortak bir strateji guid dize temsiline "id" gibi bir dize hazırlamaktır. Örneğin, id6c1c178c166d486687be4aaf5e482730 geçerli bir kimliktir. |

## <a name="next-steps"></a>Sonraki adımlar

* Bir sorunuz mu var ya da aradığınızı bulamıyor musunuz? Bir GitHub sorunu oluşturun veya [geliştiricilerin](active-directory-develop-help-support.md) yardım ve destek almanın diğer yolları hakkında bilgi edinmeleri için Destek ve Yardım seçeneklerini görün.
