---
title: Yerelleştirme dize kimlikleri-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkede API. signuporsignıd KIMLIĞINE sahip bir içerik tanımının kimliklerini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 611d676f5f588ff32f981692456160e269642a43
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87428129"
---
# <a name="localization-string-ids"></a>Yerelleştirme dizesi kimlikleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yerelleştirme** öğesi, Kullanıcı yolculukları için ilkedeki birden çok yerel ayarı veya dili desteklemenize olanak sağlar. Bu makale, ilkenizde kullanabileceğiniz yerelleştirme kimliklerinin listesini sağlar. UI yerelleştirmesini öğrenmek için bkz. [Yerelleştirme](localization.md).

## <a name="sign-up-or-sign-in-page-elements"></a>Kaydolma veya oturum açma sayfası öğeleri

KIMLIĞI olan bir içerik tanımı için aşağıdaki kimlikler kullanılır `api.signuporsignin` .

| ID | Varsayılan değer |
| -- | ------------- |
| **local_intro_email** | Mevcut hesabınızla oturum açın |
| **logonIdentifier_email** | E-posta Adresi |
| **requiredField_email** | Lütfen e-postanızı girin |
| **invalid_email** | Lütfen geçerli bir e-posta adresi girin |
| **email_pattern** | ^ [a-zA-Z0-9.! # $% & ' '*+/=? ^ _ \` { \| } ~-] + @ [a-Za-z0-9-] + (?: \\ . [ a-zA-Z0-9-] +)*$ |
| **local_intro_username** | Kullanıcı adınızla oturum açın |
| **logonIdentifier_username** | Kullanıcı adı |
| **requiredField_username** | Lütfen Kullanıcı adınızı girin |
| **parola** | Parola |
| **requiredField_password** | Lütfen parolanızı girin |
| **invalid_password** | Girdiğiniz parola beklenen biçimde değil. |
| **forgotpassword_link** | Parolanızı mı unuttunuz? |
| **createaccount_intro** | Hesabınız yok mu? |
| **createaccount_link** | Hemen kaydolun |
| **divider_title** | VEYA |
| **cancel_message** | Kullanıcı parolasını unutuldu |
| **button_signin** | Oturum açın |
| **social_intro** | Sosyal hesabınızla oturum açın |
  **remember_me** |Oturumumu açık bırak|
| **unknown_error** | Oturumunuzu açmada sorun yaşıyoruz. Lütfen daha sonra yeniden deneyin. |

Aşağıdaki örnek, kayıt veya oturum açma sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma veya oturum açma sayfası UX öğeleri](./media/localization-string-ids/localization-susi.png)

Kimlik sağlayıcılarının KIMLIĞI, Kullanıcı yolculuğu **Claimsexchange** öğesinde yapılandırılır. Kimlik sağlayıcısının başlığını yerelleştirmek için, **ElementType** olarak ayarlanır `ClaimsProvider` , **strıngıd** , kimliğine ayarlanır `ClaimsExchange` .

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="MicrosoftExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccount" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Aşağıdaki örnek, Facebook kimlik sağlayıcısını Arapça olarak yerelleştirir:

```xml
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">فيس بوك</LocalizedString>
```

### <a name="sign-up-or-sign-in-error-messages"></a>Kaydolma veya oturum açma hata iletileri

| ID | Varsayılan değer |
| -- | ------------- |
| **Usermessageifınvalidpassword** | Parolanız yanlış. |
| **UserMessageIfClaimsPrincipalDoesNotExist** | Hesabınızı bulamıyoruz. |
| **UserMessageIfOldPasswordUsed** | Eski bir parola kullandınız gibi görünüyor. |
| **DefaultMessage** | Geçersiz Kullanıcı adı veya parola. |
| **UserMessageIfUserAccountDisabled** | Hesabınız kilitlendi. Dosyanın kilidini açmak için destek sorumlunuza başvurun, sonra yeniden deneyin. |
| **Usermessageifuseraccountkilitlendi** | Hesabınız yetkisiz kullanımı engellemek için geçici olarak kilitlidir. Daha sonra tekrar deneyin. |
| **Aadrequestskısıtlanıyor** | Şu anda çok fazla istek var. Lütfen bir süre bekleyip yeniden deneyin. |

## <a name="sign-up-and-self-asserted-pages-user-interface-elements"></a>Kaydolma ve kendiliğinden onaylanan sayfalar Kullanıcı arabirimi öğeleri

Aşağıda, ve gibi bir içerik tanımının KIMLIĞI olan bir içerik tanımının kimlikleri verilmiştir `api.localaccountsignup` `api.selfasserted` `api.selfasserted.profileupdate` `api.localaccountpasswordreset` .

| ID | Varsayılan değer |
| -- | ------------- |
| **ver_sent** | Doğrulama kodu gönderildi: |
| **ver_but_default** | Varsayılan |
| **cancel_message** | Kullanıcı kendi kendine onaylanan bilgileri girmeyi iptal etti |
| **preloader_alt** | Lütfen bekleyin |
| **ver_but_send** | Doğrulama kodu gönder |
| **alert_yes** | Yes |
| **error_fieldIncorrect** | Bir veya daha fazla alan yanlış doldurulmuş. Lütfen girişlerinizi denetleyin ve yeniden deneyin. |
| **yıl** | Yıl |
| **verifying_blurb** | Bilgilerinizi işlerken lütfen bekleyin. |
| **button_cancel** | İptal |
| **ver_fail_no_retry** | Çok fazla hatalı deneme yaptınız. Lütfen daha sonra yeniden deneyin. |
| **başından** | Ay |
| **ver_success_msg** | E-posta adresi doğrulandı. Artık devam edebilirsiniz. |
| **months** | Ocak, Şubat, Mart, Nisan, Mayıs, Haziran, Temmuz, Ağustos, Eylül, Ekim, Kasım, Aralık |
| **ver_fail_server** | E-posta adresinizi doğrularken sorun yaşıyoruz. Lütfen geçerli bir e-posta adresi girin ve yeniden deneyin. |
| **error_requiredFieldMissing** | Gerekli bir alan eksik. Lütfen tüm gerekli alanları doldurun ve yeniden deneyin. |
| **initial_intro** | Lütfen aşağıdaki ayrıntıları sağlayın. |
| **ver_but_resend** | Yeni kod gönder |
| **button_continue** | Oluştur |
| **error_passwordEntryMismatch** | Parola girişi alanları eşleşmiyor. Lütfen her iki alana da aynı parolayı girip yeniden deneyin. |
| **ver_incorrect_format** | Hatalı biçim. |
| **ver_but_edit** | E-postayı değiştir |
| **ver_but_verify** | Kodu doğrula |
| **alert_no** | No |
| **ver_info_msg** | Doğrulama kodu gelen kutunuza gönderildi. Lütfen aşağıdaki giriş kutusuna kopyalayın. |
| **günündeki** | Gün |
| **ver_fail_throttled** | Bu e-posta adresini doğrulamak için çok fazla istek vardı. Lütfen bekleyin, sonra yeniden deneyin. |
| **helplink_text** | Bu nedir? |
| **ver_fail_retry** | Bu kod yanlış. Lütfen tekrar deneyin. |
| **alert_title** | Ayrıntılarınızı girmeyi iptal edin |
| **required_field** | Bu bilgiler gereklidir. |
| **alert_message** | Ayrıntılarınızı girmeyi iptal etmek istediğinizden emin misiniz? |
| **ver_intro_msg** | Doğrulama gereklidir. Lütfen gönder düğmesine tıklayın. |
| **ver_input** | Doğrulama kodu |

### <a name="sign-up-and-self-asserted-pages-error-messages"></a>Kaydolma ve kendiliğinden onaylanan sayfalar hata iletileri

| ID | Varsayılan değer |
| -- | ------------- |
| **Usermessageifclaimsprincıpalalreadyexists** | Belirtilen KIMLIĞE sahip bir kullanıcı zaten var. Lütfen farklı bir tane seçin. |
| **Usermessageifclaimnotdoğrulandı** | Talep doğrulanmadı:{0} |
| **UserMessageIfIncorrectPattern** | İçin yanlış desenler:{0} |
| **UserMessageIfMissingRequiredElement** | Gerekli öğe eksik:{0} |
| **UserMessageIfValidationError** | Doğrulamada hata:{0} |
| **Usermessageifınvalidınput** | {0}geçersiz giriş içeriyor. |
| **Servicekısıtlanıyor** | Şu anda çok fazla istek var. Lütfen bir süre bekleyip yeniden deneyin. |

Aşağıdaki örnek, kaydolma sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kullanıcı arabirimi öğesi adları etiketli kaydolma sayfası](./media/localization-string-ids/localization-sign-up.png)

Aşağıdaki örnek, Kullanıcı doğrulama kodu gönder düğmesine tıkladıktan sonra kaydolma sayfasındaki bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma sayfası eposta doğrulaması UX öğeleri](./media/localization-string-ids/localization-email-verification.png)


## <a name="phone-factor-authentication-page-user-interface-elements"></a>Telefon faktörü kimlik doğrulama sayfası kullanıcı arabirimi öğeleri

Aşağıda KIMLIĞI olan bir içerik tanımının kimlikleri verilmiştir `api.phonefactor` .

| ID | Varsayılan değer |
| -- | ------------- |
| **button_verify** | Beni ara |
| **country_code_label** | Ülke Kodu |
| **cancel_message** | Kullanıcı Multi-Factor Authentication 'ı iptal etti |
| **text_button_send_second_code** | Yeni kod gönder |
| **code_pattern** | \\d{6} |
| **intro_mixed** | Sizin için kayıt üzerinde aşağıdaki sayıyı sunuyoruz. Kimliğinizi doğrulamak için SMS veya Phone aracılığıyla kod gönderebiliriz. |
| **intro_mixed_p** | Sizin için kayıt için aşağıdaki numaralara ihtiyacımız var. Kimliğinizi doğrulamak için SMS aracılığıyla telefon ve kod gönderebileceğimiz bir sayı seçin. |
| **button_verify_code** | Kodu doğrula |
| **requiredField_code** | Lütfen aldığınız doğrulama kodunu girin |
| **invalid_code** | Lütfen aldığınız 6 basamaklı kodu girin |
| **button_cancel** | İptal |
| **local_number_input_placeholder_text** | Telefon numarası |
| **button_retry** | Yeniden Dene |
| **alternative_text** | Telefonum yok |
| **intro_phone_p** | Sizin için kayıt için aşağıdaki numaralara ihtiyacımız var. Kimliğinizi doğrulamak için telefon yapabilmemiz gereken bir sayı seçin. |
| **intro_phone** | Sizin için kayıt üzerinde aşağıdaki sayıyı sunuyoruz. Kimliğinizi doğrulamak için telefon göndereceğiz. |
| **enter_code_text_intro** | Doğrulama kodunuzu aşağıya girin veya  |
| **intro_entry_phone** | Kimliğinizi doğrulamak için telefon yapabilmemiz gereken bir sayı girin. |
| **intro_entry_sms** | Kimliğinizi doğrulamak için SMS aracılığıyla kod gönderebileceğimiz bir sayı girin. |
| **button_send_code** | Kodu gönder |
| **invalid_number** | Lütfen geçerli bir telefon numarası girin |
| **intro_sms** | Sizin için kayıt üzerinde aşağıdaki sayıyı sunuyoruz. Kimliğinizi doğrulamak için SMS aracılığıyla bir kod göndereceğiz. |
| **intro_entry_mixed** | Kimliğinizi doğrulamak için SMS veya Phone aracılığıyla kod gönderebileceğimiz bir sayı girin. |
| **number_pattern** | ^\\+ (?: [0-9] [ \\ x20-]?) {6,14} [0-9] $ |
| **intro_sms_p** |Sizin için kayıt için aşağıdaki numaralara ihtiyacımız var. Kimliğinizi doğrulamak için SMS aracılığıyla kod gönderebileceğimiz bir sayı seçin. |
| **requiredField_countryCode** | Lütfen ülke kodunuzu seçin |
| **requiredField_number** | Lütfen telefon numaranızı girin |
| **country_code_input_placeholder_text** |Ülke veya bölge |
| **number_label** | Telefon Numarası |
| **error_tryagain** | Verdiğiniz telefon numarası meşgul veya kullanılamıyor. Lütfen numarayı denetleyip yeniden deneyin. |
| **error_incorrect_code** | Girdiğiniz doğrulama kodu kayıtlarımızla eşleşmiyor. Lütfen yeniden deneyin veya yeni bir kod isteyin. |
| **countryList** | [Ülkeler listesini](#countries-list)görüntüleyin. |
| **error_448** | Verdiğiniz telefon numarasına ulaşılamıyor. |
| **error_449** | Kullanıcı yeniden deneme girişimlerinin sayısını aştı. |
| **verification_code_input_placeholder_text** | Doğrulama kodu |

Aşağıdaki örnek, MFA kayıt sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma sayfası eposta doğrulaması UX öğeleri](./media/localization-string-ids/localization-mfa1.png)

Aşağıdaki örnek, MFA doğrulama sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma sayfası eposta doğrulaması UX öğeleri](./media/localization-string-ids/localization-mfa2.png)

## <a name="verification-display-control-user-interface-elements"></a>Doğrulama görüntüleme denetimi kullanıcı arabirimi öğeleri

[Doğrulama görüntüleme denetimi](display-control-verification.md) için kimlikler şunlardır

| ID | Varsayılan değer |
| -- | ------------- |
|verification_control_but_change_claims |Değiştir |
|verification_control_fail_send_code |Kod gönderilemedi, lütfen daha sonra yeniden deneyin. |
|verification_control_fail_verify_code |Kod doğrulanamadı, lütfen daha sonra yeniden deneyin. |
|verification_control_but_send_code |Kodu gönder |
|verification_control_but_send_new_code |Yeni kod gönder |
|verification_control_but_verify_code |Kodu doğrula |
|verification_control_code_sent| Doğrulama kodu gönderildi. Lütfen aşağıdaki giriş kutusuna kopyalayın. |

### <a name="example"></a>Örnek

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_change_claims">Change</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_fail_send_code">Failed to send the code, please try again later.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_fail_verify_code">Failed to verify the code, please try again later.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_send_code">Send Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_send_new_code">Send New Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_but_verify_code">Verify Code</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="verification_control_code_sent">Verification code has been sent. Please copy it to the input box below.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="restful-service-error-messages"></a>Yeniden hizmet hata iletileri

Aşağıda, [Restvıservice teknik profili](restful-technical-profile.md) hata iletilerinin kimlikleri verilmiştir:

| ID | Varsayılan değer |
| -- | ------------- |
|DefaultUserMessageIfRequestFailed | Yeniden hizmet uç noktası bağlantısı kurulamadı. Yeniden deneme hizmeti URL 'SI:{0} |
|Usermessageifbir ıtopen | {0}Yeniden deneme hizmeti URL 'SI:{1} |
|UserMessageIfDnsResolutionFailed | Yeniden deneme hizmeti uç noktasının ana bilgisayar adı çözümlenemedi. Yeniden deneme hizmeti URL 'SI:{0} |
|UserMessageIfRequestTimeout | Zaman aşımı sınırı saniyesi içindeki hizmet uç noktası için bağlantı kurulamadı {0} . Yeniden deneme hizmeti URL 'SI:{1} |


### <a name="example"></a>Örnek

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="DefaultUserMessageIfRequestFailed">Failed to establish connection to restful service end point.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfCircuitOpen">Unable to connect to the restful service end point.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfDnsResolutionFailed">Failed to resolve the hostname of the restful service endpoint.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfRequestTimeout">Failed to establish connection to restful service end point within timeout limit.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="azure-mfa-error-messages"></a>Azure MFA hata iletileri

Aşağıda, [Azure MFA teknik profili](multi-factor-auth-technical-profile.md) hata iletilerinin kimlikleri verilmiştir:

| ID | Varsayılan değer |
| -- | ------------- |
|Usermessageif, Dntsendsms | SMS telefona gönderilemiyor, lütfen başka bir telefon numarası deneyin. |
|Usermessageifınvalidformat | Telefon numaranız geçerli bir biçimde değil, lütfen düzeltip yeniden deneyin.|
|Usermessageifmaxallowedcoderetr'e ulaşıldı | Yanlış kod çok fazla girildi, lütfen daha sonra yeniden deneyin.|
|UserMessageIfServerError | MFA hizmeti kullanılamıyor, lütfen daha sonra yeniden deneyin.|
|Usermessageifkısıtlanıyor | İsteğiniz kısıtlandı, lütfen daha sonra yeniden deneyin.|
|Usermessageifyanlışlıkla Gcodegirildi|Yanlış kod girildi, lütfen yeniden deneyin.|

### <a name="example"></a>Örnek

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfCouldntSendSms">Cannot Send SMS to the phone, please try another phone number.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidFormat">Your phone number is not in a valid format, please correct it and try again.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfMaxAllowedCodeRetryReached">Wrong code entered too many times, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfServerError">Cannot use MFA service, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfThrottled">Your request has been throttled, please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfWrongCodeEntered">Wrong code entered, please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="azure-ad-sspr"></a>Azure AD SSPR

[Azure AD SSPR teknik profili](aad-sspr-technical-profile.md) hata Iletileri için kimlikler şunlardır:

| ID | Varsayılan değer |
| -- | ------------- |
|Usermessageifrequesgeerer | Kodun süresi doldu.|
|Usermessageifınternalerror | E-posta hizmeti bir iç hatayla karşılaştı, lütfen daha sonra yeniden deneyin.|
|Usermessageifkısıtlanıyor | Çok fazla istek gönderdiniz, lütfen daha sonra yeniden deneyin.|
|Usermessageifdoğrulamaları Icationfailednoretry | En fazla doğrulama denemesi sayısını aştınız.|
|UserMessageIfVerificationFailedRetryAllowed | Doğrulama başarısız oldu, lütfen yeniden deneyin.|


### <a name="example"></a>Örnek

```XML
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInternalError">We are having trouble verifying your email address. Please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfThrottled">There have been too many requests to verify this email address. Please wait a while, then try again.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfChallengeExpired">That code is expired. Please request a new code.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedNoRetry">You've made too many incorrect attempts. Please try again later.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedRetryAllowed">That code is incorrect. Please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="one-time-password-error-messages"></a>Bir kerelik parola hata iletileri

[Bir kerelik parola teknik profili](one-time-password-technical-profile.md) hata Iletileri için kimlikler aşağıda verilmiştir

| ID | Varsayılan değer |
| -- | ------------- |
|UserMessageIfMaxRetryAttempted |Bir kerelik parola belirtilen doğrulama, en fazla deneme sayısını aştı |
|Usermessageifsessionffnotexist |Bir kerelik parola doğrulama oturumunun süresi doldu |
|UserMessageIfSessionConflict |Bir kerelik parola doğrulama oturumunun çakışması |
|Usermessageifınvalidcode |Doğrulama için girilen bir kerelik parola yanlış |
|UserMessageIfVerificationFailedRetryAllowed |Bu kod yanlış. Lütfen tekrar deneyin. | 

### <a name="example"></a>Örnek

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfSessionDoesNotExist">You have exceed the maximum time allowed.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfMaxRetryAttempted">You have exceed the number of retries allowed.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfInvalidCode">You have entered the wrong code.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfSessionConflict">Cannot verify the code, please try again later.</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfVerificationFailedRetryAllowed">That code is incorrect. Please try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="claims-transformations-error-messages"></a>Talep dönüştürmeleri hata iletileri

Talep dönüştürmeleri hata iletileri için kimlikler şunlardır:

| ID | Talepleri dönüştürme | Varsayılan değer |
| -- | ------------- |------------- |
|UserMessageIfClaimsTransformationBooleanValueIsNotEqual |[AssertBooleanClaimIsEqualToValue](boolean-transformations.md#assertbooleanclaimisequaltovalue) | "Inputclaim" talep türü için Boole talep değeri karşılaştırması başarısız oldu.| 
|DateTimeGreaterThan |[AssertDateTimeIsGreaterThan](date-transformations.md#assertdatetimeisgreaterthan) | Talep değeri karşılaştırması başarısız oldu: belirtilen sol işlenen, sağ işlenenden büyük.|
|UserMessageIfClaimsTransformationStringsAreNotEqual |[Assertstringclaimsareeşittir](string-transformations.md#assertstringclaimsareequal) | Talep değeri karşılaştırması, StringComparison "OrdinalIgnoreCase" ile başarısız oldu.|

### <a name="example"></a>Örnek

```xml
<LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Your email address hasn't been verified.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="DateTimeGreaterThan">Expiration date must be greater that the current date.</LocalizedString>
    <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsTransformationStringsAreNotEqual">The email entry fields do not match. Please enter the same email address in both fields and try again.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

## <a name="countries-list"></a>Ülkeler listesi

Multi-Factor Authentication tarafından kullanılan countryList değerleri aşağıda verilmiştir.

```JSON
{"DEFAULT":"Country/Region","AF":"Afghanistan","AX":"Åland Islands","AL":"Albania","DZ":"Algeria","AS":"American Samoa","AD":"Andorra","AO":"Angola","AI":"Anguilla","AQ":"Antarctica","AG":"Antigua and Barbuda","AR":"Argentina","AM":"Armenia","AW":"Aruba","AU":"Australia","AT":"Austria","AZ":"Azerbaijan","BS":"Bahamas","BH":"Bahrain","BD":"Bangladesh","BB":"Barbados","BY":"Belarus","BE":"Belgium","BZ":"Belize","BJ":"Benin","BM":"Bermuda","BT":"Bhutan","BO":"Bolivia","BQ":"Bonaire","BA":"Bosnia and Herzegovina","BW":"Botswana","BV":"Bouvet Island","BR":"Brazil","IO":"British Indian Ocean Territory","VG":"British Virgin Islands","BN":"Brunei","BG":"Bulgaria","BF":"Burkina Faso","BI":"Burundi","CV":"Cabo Verde","KH":"Cambodia","CM":"Cameroon","CA":"Canada","KY":"Cayman Islands","CF":"Central African Republic","TD":"Chad","CL":"Chile","CN":"China","CX":"Christmas Island","CC":"Cocos (Keeling) Islands","CO":"Colombia","KM":"Comoros","CG":"Congo","CD":"Congo (DRC)","CK":"Cook Islands","CR":"Costa Rica","CI":"Côte d'Ivoire","HR":"Croatia","CU":"Cuba","CW":"Curaçao","CY":"Cyprus","CZ":"Czech Republic","DK":"Denmark","DJ":"Djibouti","DM":"Dominica","DO":"Dominican Republic","EC":"Ecuador","EG":"Egypt","SV":"El Salvador","GQ":"Equatorial Guinea","ER":"Eritrea","EE":"Estonia","ET":"Ethiopia","FK":"Falkland Islands","FO":"Faroe Islands","FJ":"Fiji","FI":"Finland","FR":"France","GF":"French Guiana","PF":"French Polynesia","TF":"French Southern Territories","GA":"Gabon","GM":"Gambia","GE":"Georgia","DE":"Germany","GH":"Ghana","GI":"Gibraltar","GR":"Greece","GL":"Greenland","GD":"Grenada","GP":"Guadeloupe","GU":"Guam","GT":"Guatemala","GG":"Guernsey","GN":"Guinea","GW":"Guinea-Bissau","GY":"Guyana","HT":"Haiti","HM":"Heard Island and McDonald Islands","HN":"Honduras","HK":"Hong Kong SAR","HU":"Hungary","IS":"Iceland","IN":"India","ID":"Indonesia","IR":"Iran","IQ":"Iraq","IE":"Ireland","IM":"Isle of Man","IL":"Israel","IT":"Italy","JM":"Jamaica","JP":"Japan","JE":"Jersey","JO":"Jordan","KZ":"Kazakhstan","KE":"Kenya","KI":"Kiribati","KR":"Korea","KW":"Kuwait","KG":"Kyrgyzstan","LA":"Laos","LV":"Latvia","LB":"Lebanon","LS":"Lesotho","LR":"Liberia","LY":"Libya","LI":"Liechtenstein","LT":"Lithuania","LU":"Luxembourg","MO":"Macao SAR","MK":"North Macedonia","MG":"Madagascar","MW":"Malawi","MY":"Malaysia","MV":"Maldives","ML":"Mali","MT":"Malta","MH":"Marshall Islands","MQ":"Martinique","MR":"Mauritania","MU":"Mauritius","YT":"Mayotte","MX":"Mexico","FM":"Micronesia","MD":"Moldova","MC":"Monaco","MN":"Mongolia","ME":"Montenegro","MS":"Montserrat","MA":"Morocco","MZ":"Mozambique","MM":"Myanmar","NA":"Namibia","NR":"Nauru","NP":"Nepal","NL":"Netherlands","NC":"New Caledonia","NZ":"New Zealand","NI":"Nicaragua","NE":"Niger","NG":"Nigeria","NU":"Niue","NF":"Norfolk Island","KP":"North Korea","MP":"Northern Mariana Islands","NO":"Norway","OM":"Oman","PK":"Pakistan","PW":"Palau","PS":"Palestinian Authority","PA":"Panama","PG":"Papua New Guinea","PY":"Paraguay","PE":"Peru","PH":"Philippines","PN":"Pitcairn Islands","PL":"Poland","PT":"Portugal","PR":"Puerto Rico","QA":"Qatar","RE":"Réunion","RO":"Romania","RU":"Russia","RW":"Rwanda","BL":"Saint Barthélemy","KN":"Saint Kitts and Nevis","LC":"Saint Lucia","MF":"Saint Martin","PM":"Saint Pierre and Miquelon","VC":"Saint Vincent and the Grenadines","WS":"Samoa","SM":"San Marino","ST":"São Tomé and Príncipe","SA":"Saudi Arabia","SN":"Senegal","RS":"Serbia","SC":"Seychelles","SL":"Sierra Leone","SG":"Singapore","SX":"Sint Maarten","SK":"Slovakia","SI":"Slovenia","SB":"Solomon Islands","SO":"Somalia","ZA":"South Africa","GS":"South Georgia and South Sandwich Islands","SS":"South Sudan","ES":"Spain","LK":"Sri Lanka","SH":"St Helena, Ascension, Tristan da Cunha","SD":"Sudan","SR":"Suriname","SJ":"Svalbard","SZ":"Swaziland","SE":"Sweden","CH":"Switzerland","SY":"Syria","TW":"Taiwan","TJ":"Tajikistan","TZ":"Tanzania","TH":"Thailand","TL":"Timor-Leste","TG":"Togo","TK":"Tokelau","TO":"Tonga","TT":"Trinidad and Tobago","TN":"Tunisia","TR":"Turkey","TM":"Turkmenistan","TC":"Turks and Caicos Islands","TV":"Tuvalu","UM":"U.S. Outlying Islands","VI":"U.S. Virgin Islands","UG":"Uganda","UA":"Ukraine","AE":"United Arab Emirates","GB":"United Kingdom","US":"United States","UY":"Uruguay","UZ":"Uzbekistan","VU":"Vanuatu","VA":"Vatican City","VE":"Venezuela","VN":"Vietnam","WF":"Wallis and Futuna","YE":"Yemen","ZM":"Zambia","ZW":"Zimbabwe"}
```

## <a name="next-steps"></a>Sonraki adımlar

Yerelleştirme örnekleri için aşağıdaki makalelere bakın:

- [Azure Active Directory B2C özel ilkeyle dil özelleştirmesi](custom-policy-localization.md)
- [Azure Active Directory B2C Kullanıcı akışları ile dil özelleştirmesi](user-flow-language-customization.md)