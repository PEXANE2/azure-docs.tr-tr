---
title: Yerelleştirme dize kimlikleri-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C bir özel ilkede API. signuporsignıd kimliğine sahip bir içerik tanımının kimliklerini belirtin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/03/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: cd5e543a6d5dc2b59847a78bd925be68f8cdde25
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76982866"
---
# <a name="localization-string-ids"></a>Yerelleştirme dizesi kimlikleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yerelleştirme** öğesi, Kullanıcı yolculukları için ilkedeki birden çok yerel ayarı veya dili desteklemenize olanak sağlar. Bu makale, ilkenizde kullanabileceğiniz yerelleştirme kimliklerinin listesini sağlar. UI yerelleştirmesini öğrenmek için bkz. [Yerelleştirme](localization.md).

## <a name="sign-up-or-sign-in-page-elements"></a>Kaydolma veya oturum açma sayfası öğeleri

KIMLIĞI `api.signuporsignin`olan bir içerik tanımı için aşağıdaki kimlikler kullanılır.

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **local_intro_email** | Mevcut hesabınızla oturum açın |
| **logonIdentifier_email** | E-posta Adresi |
| **requiredField_email** | Lütfen e-postanızı girin |
| **invalid_email** | Lütfen geçerli bir e-posta adresi girin |
| **email_pattern** | ^ [a-zA-Z0-9.! # $% & ' ' *+/=? ^ _\`{\|} ~-] + @ [a-Za-z0-9-] + (?:\\. [ a-zA-Z0-9-] +)* $ |
| **local_intro_username** | Kullanıcı adınızla oturum açın |
| **logonIdentifier_username** | Kullanıcı adı |
| **requiredField_username** | Lütfen Kullanıcı adınızı girin |
| **Parola** | Parola |
| **requiredField_password** | Lütfen parolanızı girin |
| **invalid_password** | Girdiğiniz parola beklenen biçimde değil. |
| **forgotpassword_link** | Parolanızı mı unuttunuz? |
| **createaccount_intro** | Hesabınız yok mu? |
| **createaccount_link** | Hemen kaydolun |
| **divider_title** | VEYA |
| **cancel_message** | Kullanıcı parolasını unutuldu |
| **button_signin** | Oturum aç |
| **social_intro** | Sosyal hesabınızla oturum açın |
  **remember_me** |Oturumumu Açık tut|
| **unknown_error** | Oturumunuzu açmada sorun yaşıyoruz. Lütfen daha sonra tekrar deneyin. |

Aşağıdaki örnek, kayıt veya oturum açma sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma veya oturum açma sayfası UX öğeleri](./media/localization-string-ids/localization-susi.png)

Kimlik sağlayıcılarının KIMLIĞI, Kullanıcı yolculuğu **Claimsexchange** öğesinde yapılandırılır. Kimlik sağlayıcısının başlığını yerelleştirmek için, **ElementType** `ClaimsProvider`olarak ayarlanır, ancak **strıngıd** `ClaimsExchange`kimliği olarak ayarlanır.

```XML
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

```XML
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">فيس بوك</LocalizedString>
```

### <a name="sign-up-or-sign-in-error-messages"></a>Kaydolma veya oturum açma hata iletileri

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **Usermessageifınvalidpassword** | Parolanız yanlış. |
| **UserMessageIfClaimsPrincipalDoesNotExist** | Hesabınızı bulamıyoruz. |
| **UserMessageIfOldPasswordUsed** | Eski bir parola kullandınız gibi görünüyor. |
| **DefaultMessage** | Geçersiz Kullanıcı adı veya parola. |
| **UserMessageIfUserAccountDisabled** | Hesabınız kilitlendi. Dosyanın kilidini açmak için destek sorumlunuza başvurun, sonra yeniden deneyin. |
| **Usermessageifuseraccountkilitlendi** | Hesabınız yetkisiz kullanımı engellemek için geçici olarak kilitlidir. Daha sonra tekrar deneyin. |
| **Aadrequestskısıtlanıyor** | Şu anda çok fazla istek var. Lütfen bir süre bekleyip yeniden deneyin. |

## <a name="sign-up-and-self-asserted-pages-user-interface-elements"></a>Kaydolma ve kendi kendine onaylanan sayfalar Kullanıcı arabirimi öğeleri

Aşağıda, `api.localaccountsignup` KIMLIĞI olan bir içerik tanımının kimlikleri veya `api.selfasserted.profileupdate` ve `api.localaccountpasswordreset`gibi `api.selfasserted`ile başlayan herhangi bir içerik tanımı verilmiştir.

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **ver_sent** | Doğrulama kodu gönderildi: |
| **ver_but_default** | Varsayılan |
| **cancel_message** | Kullanıcı kendi kendine onaylanan bilgileri girmeyi iptal etti |
| **preloader_alt** | Lütfen bekleyin |
| **ver_but_send** | Doğrulama kodu gönder |
| **alert_yes** | Evet |
| **error_fieldIncorrect** | Bir veya daha fazla alan yanlış doldurulmuş. Lütfen girişlerinizi denetleyin ve yeniden deneyin. |
| **yıl** | Yıl |
| **verifying_blurb** | Bilgilerinizi işlerken lütfen bekleyin. |
| **button_cancel** | İptal |
| **ver_fail_no_retry** | Çok fazla hatalı deneme yaptınız. Lütfen daha sonra tekrar deneyin. |
| **başından** | Ay |
| **ver_success_msg** | E-posta adresi doğrulandı. Artık devam edebilirsiniz. |
| **months** | Ocak, Şubat, Mart, Nisan, Mayıs, Haziran, Temmuz, Ağustos, Eylül, Ekim, Kasım, Aralık |
| **ver_fail_server** | E-posta adresinizi doğrularken sorun yaşıyoruz. Lütfen geçerli bir e-posta adresi girin ve yeniden deneyin. |
| **error_requiredFieldMissing** | Gerekli bir alan eksik. Lütfen tüm gerekli alanları doldurun ve yeniden deneyin. |
| **initial_intro** | Lütfen aşağıdaki ayrıntıları sağlayın. |
| **ver_but_resend** | Yeni kod gönder |
| **button_continue** | Create |
| **error_passwordEntryMismatch** | Parola girişi alanları eşleşmiyor. Lütfen her iki alana da aynı parolayı girip yeniden deneyin. |
| **ver_incorrect_format** | Hatalı biçim. |
| **ver_but_edit** | E-postayı değiştir |
| **ver_but_verify** | Kodu doğrula |
| **alert_no** | Hayır |
| **ver_info_msg** | Doğrulama kodu gelen kutunuza gönderildi. Lütfen aşağıdaki giriş kutusuna kopyalayın. |
| **günündeki** | Gün |
| **ver_fail_throttled** | Bu e-posta adresini doğrulamak için çok fazla istek vardı. Lütfen bekleyin, sonra yeniden deneyin. |
| **helplink_text** | Bu nedir? |
| **ver_fail_retry** | Bu kod yanlış. Lütfen yeniden deneyin. |
| **alert_title** | Ayrıntılarınızı girmeyi iptal edin |
| **required_field** | Bu bilgiler gereklidir. |
| **alert_message** | Ayrıntılarınızı girmeyi iptal etmek istediğinizden emin misiniz? |
| **ver_intro_msg** | Doğrulama gereklidir. Lütfen gönder düğmesine tıklayın. |
| **ver_input** | Doğrulama kodu |

### <a name="sign-up-and-self-asserted-pages-error-messages"></a>Kaydolma ve kendi kendine onaylanan sayfalar hata iletileri

| Kimlik | Varsayılan değer |
| -- | ------------- |
| **Usermessageifclaimsprincıpalalreadyexists** | Belirtilen KIMLIĞE sahip bir kullanıcı zaten var. Lütfen farklı bir tane seçin. |
| **Usermessageifclaimnotdoğrulandı** | Talep doğrulanmadı: {0} |
| **UserMessageIfIncorrectPattern** | İçin yanlış model: {0} |
| **UserMessageIfMissingRequiredElement** | Gerekli öğe eksik: {0} |
| **UserMessageIfValidationError** | Doğrulamada hata: {0} |
| **Usermessageifınvalidınput** | {0} geçersiz giriş içeriyor. |
| **Servicekısıtlanıyor** | Şu anda çok fazla istek var. Lütfen bir süre bekleyip yeniden deneyin. |

Aşağıdaki örnek, kaydolma sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kullanıcı arabirimi öğesi adları etiketli kaydolma sayfası](./media/localization-string-ids/localization-sign-up.png)

Aşağıdaki örnek, Kullanıcı doğrulama kodu gönder düğmesine tıkladıktan sonra kaydolma sayfasındaki bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma sayfası eposta doğrulaması UX öğeleri](./media/localization-string-ids/localization-email-verification.png)


## <a name="phone-factor-authentication-page-user-interface-elements"></a>Telefon faktörü kimlik doğrulama sayfası kullanıcı arabirimi öğeleri

Aşağıda KIMLIĞI `api.phonefactor`olan bir içerik tanımının kimlikleri verilmiştir.

| Kimlik | Varsayılan değer |
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
| **number_pattern** | ^\\+ (?: [0-9] [\\x20-]?){6,14}[0-9] $ |
| **intro_sms_p** |Sizin için kayıt için aşağıdaki numaralara ihtiyacımız var. Kimliğinizi doğrulamak için SMS aracılığıyla kod gönderebileceğimiz bir sayı seçin. |
| **requiredField_countryCode** | Lütfen ülke kodunuzu seçin |
| **requiredField_number** | Lütfen telefon numaranızı girin |
| **country_code_input_placeholder_text** |Ülke veya bölge |
| **number_label** | Telefon Numarası |
| **error_tryagain** | Verdiğiniz telefon numarası meşgul veya kullanılamıyor. Lütfen numarayı denetleyip yeniden deneyin. |
| **error_incorrect_code** | Girdiğiniz doğrulama kodu kayıtlarımızla eşleşmiyor. Lütfen yeniden deneyin veya yeni bir kod isteyin. |
| **countryList** | {\"DEFAULT\":\"Country/Region\",\"AF\":\"Afghanistan\",\"AX\":\"Åland Islands\",\"AL\":\"Albania\",\"DZ\":\"Algeria\",\"AS\":\"American Samoa\",\"AD\":\"Andorra\",\"AO\":\"Angola\",\"AI\":\"Anguilla\",\"AQ\":\"Antarctica\",\"AG\":\"Antigua and Barbuda\",\"AR\":\"Argentina\",\"AM\":\"Armenia\",\"AW\":\"Aruba\",\"AU\":\"Australia\",\"AT\":\"Austria\",\"AZ\":\"Azerbaijan\",\"BS\":\"Bahamas\",\"BH\":\"Bahrain\",\"BD\":\"Bangladesh\",\"BB\":\"Barbados\",\"BY\":\"Belarus\",\"BE\":\"Belgium\",\"BZ\":\"Belize\",\"BJ\":\"Benin\",\"BM\":\"Bermuda\",\"BT\":\"Bhutan\",\"BO\":\"Bolivia\",\"BQ\":\"Bonaire\",\"BA\":\"Bosnia and Herzegovina\",\"BW\":\"Botswana\",\"BV\":\"Bouvet Island\",\"BR\":\"Brazil\",\"IO\":\"British Indian Ocean Territory\",\"VG\":\"British Virgin Islands\",\"BN\":\"Brunei\",\"BG\":\"Bulgaria\",\"BF\":\"Burkina Faso\",\"BI\":\"Burundi\",\"CV\":\"Cabo Verde\",\"KH\":\"Cambodia\",\"CM\":\"Cameroon\",\"CA\":\"Canada\",\"KY\":\"Cayman Islands\",\"CF\":\"Central African Republic\",\"TD\":\"Chad\",\"CL\":\"Chile\",\"CN\":\"China\",\"CX\":\"Christmas Island\",\"CC\":\"Cocos (Keeling) Islands\",\"CO\":\"Colombia\",\"KM\":\"Comoros\",\"CG\":\"Congo\",\"CD\":\"Congo (DRC)\",\"CK\":\"Cook Islands\",\"CR\":\"Costa Rica\",\"CI\":\"Côte d’Ivoire\",\"HR\":\"Croatia\",\"CU\":\"Cuba\",\"CW\":\"Curaçao\",\"CY\":\"Cyprus\",\"CZ\":\"Czech Republic\",\"DK\":\"Denmark\",\"DJ\":\"Djibouti\",\"DM\":\"Dominica\",\"DO\":\"Dominican Republic\",\"EC\":\"Ecuador\",\"EG\":\"Egypt\",\"SV\":\"El Salvador\",\"GQ\":\"Equatorial Guinea\",\"ER\":\"Eritrea\",\"EE\":\"Estonia\",\"ET\":\"Ethiopia\",\"FK\":\"Falkland Islands\",\"FO\":\"Faroe Islands\",\"FJ\":\"Fiji\",\"FI\":\"Finland\",\"FR\":\"France\",\"GF\":\"French Guiana\",\"PF\":\"French Polynesia\",\"TF\":\"French Southern Territories\",\"GA\":\"Gabon\",\"GM\":\"Gambia\",\"GE\":\"Georgia\",\"DE\":\"Germany\",\"GH\":\"Ghana\",\"GI\":\"Gibraltar\",\"GR\":\"Greece\",\"GL\":\"Greenland\",\"GD\":\"Grenada\",\"GP\":\"Guadeloupe\",\"GU\":\"Guam\",\"GT\":\"Guatemala\",\"GG\":\"Guernsey\",\"GN\":\"Guinea\",\"GW\":\"Guinea-Bissau\",\"GY\":\"Guyana\",\"HT\":\"Haiti\",\"HM\":\"Heard Island and McDonald Islands\",\"HN\":\"Honduras\",\"HK\":\"Hong Kong SAR\",\"HU\":\"Hungary\",\"IS\":\"Iceland\",\"IN\":\"India\",\"ID\":\"Indonesia\",\"IR\":\"Iran\",\"IQ\":\"Iraq\",\"IE\":\"Ireland\",\"IM\":\"Isle of Man\",\"IL\":\"Israel\",\"IT\":\"Italy\",\"JM\":\"Jamaica\",\"JP\":\"Japan\",\"JE\":\"Jersey\",\"JO\":\"Jordan\",\"KZ\":\"Kazakhstan\",\"KE\":\"Kenya\",\"KI\":\"Kiribati\",\"KR\":\"Korea\",\"KW\":\"Kuwait\",\"KG\":\"Kyrgyzstan\",\"LA\":\"Laos\",\"LV\":\"Latvia\",\"LB\":\"Lebanon\",\"LS\":\"Lesotho\",\"LR\":\"Liberia\",\"LY\":\"Libya\",\"LI\":\"Liechtenstein\",\"LT\":\"Lithuania\",\"LU\":\"Luxembourg\",\"MO\":\"Macao SAR\",\"MK\":\"North Macedonia\",\"MG\":\"Madagascar\",\"MW\":\"Malawi\",\"MY\":\"Malaysia\",\"MV\":\"Maldives\",\"ML\":\"Mali\",\"MT\":\"Malta\",\"MH\":\"Marshall Islands\",\"MQ\":\"Martinique\",\"MR\":\"Mauritania\",\"MU\":\"Mauritius\",\"YT\":\"Mayotte\",\"MX\":\"Mexico\",\"FM\":\"Micronesia\",\"MD\":\"Moldova\",\"MC\":\"Monaco\",\"MN\":\"Mongolia\",\"ME\":\"Montenegro\",\"MS\":\"Montserrat\",\"MA\":\"Morocco\",\"MZ\":\"Mozambique\",\"MM\":\"Myanmar\",\"NA\":\"Namibia\",\"NR\":\"Nauru\",\"NP\":\"Nepal\",\"NL\":\"Netherlands\",\"NC\":\"New Caledonia\",\"NZ\":\"New Zealand\",\"NI\":\"Nicaragua\",\"NE\":\"Niger\",\"NG\":\"Nigeria\",\"NU\":\"Niue\",\"NF\":\"Norfolk Island\",\"KP\":\"North Korea\",\"MP\":\"Northern Mariana Islands\",\"NO\":\"Norway\",\"OM\":\"Oman\",\"PK\":\"Pakistan\",\"PW\":\"Palau\",\"PS\":\"Palestinian Authority\",\"PA\":\"Panama\",\"PG\":\"Papua New Guinea\",\"PY\":\"Paraguay\",\"PE\":\"Peru\",\"PH\":\"Philippines\",\"PN\":\"Pitcairn Islands\",\"PL\":\"Poland\",\"PT\":\"Portugal\",\"PR\":\"Puerto Rico\",\"QA\":\"Qatar\",\"RE\":\"Réunion\",\"RO\":\"Romania\",\"RU\":\"Russia\",\"RW\":\"Rwanda\",\"BL\":\"Saint Barthélemy\",\"KN\":\"Saint Kitts and Nevis\",\"LC\":\"Saint Lucia\",\"MF\":\"Saint Martin\",\"PM\":\"Saint Pierre and Miquelon\",\"VC\":\"Saint Vincent and the Grenadines\",\"WS\":\"Samoa\",\"SM\":\"San Marino\",\"ST\":\"São Tomé and Príncipe\",\"SA\":\"Saudi Arabia\",\"SN\":\"Senegal\",\"RS\":\"Serbia\",\"SC\":\"Seychelles\",\"SL\":\"Sierra Leone\",\"SG\":\"Singapore\",\"SX\":\"Sint Maarten\",\"SK\":\"Slovakia\",\"SI\":\"Slovenia\",\"SB\":\"Solomon Islands\",\"SO\":\"Somalia\",\"ZA\":\"South Africa\",\"GS\":\"South Georgia and South Sandwich Islands\",\"SS\":\"South Sudan\",\"ES\":\"Spain\",\"LK\":\"Sri Lanka\",\"SH\":\"St Helena, Ascension, Tristan da Cunha\",\"SD\":\"Sudan\",\"SR\":\"Suriname\",\"SJ\":\"Svalbard\",\"SZ\":\"Swaziland\",\"SE\":\"Sweden\",\"CH\":\"Switzerland\",\"SY\":\"Syria\",\"TW\":\"Taiwan\",\"TJ\":\"Tajikistan\",\"TZ\":\"Tanzania\",\"TH\":\"Thailand\",\"TL\":\"Timor-Leste\",\"TG\":\"Togo\",\"TK\":\"Tokelau\",\"TO\":\"Tonga\",\"TT\":\"Trinidad and Tobago\",\"TN\":\"Tunisia\",\"TR\":\"Turkey\",\"TM\":\"Turkmenistan\",\"TC\":\"Turks and Caicos Islands\",\"TV\":\"Tuvalu\",\"UM\":\"U.S. Outlying Islands\",\"VI\":\"U.S. Virgin Islands\",\"UG\":\"Uganda\",\"UA\":\"Ukraine\",\"AE\":\"United Arab Emirates\",\"GB\":\"United Kingdom\",\"US\":\"United States\",\"UY\":\"Uruguay\",\"UZ\":\"Uzbekistan\",\"VU\":\"Vanuatu\",\"VA\":\"Vatican City\",\"VE\":\"Venezuela\",\"VN\":\"Vietnam\",\"WF\":\"Wallis and Futuna\",\"YE\":\"Yemen\",\"ZM\":\"Zambia\",\"ZW\":\"Zimbabwe\"} |
| **error_448** | Verdiğiniz telefon numarasına ulaşılamıyor. |
| **error_449** | Kullanıcı yeniden deneme girişimlerinin sayısını aştı. |
| **verification_code_input_placeholder_text** | Doğrulama kodu |

Aşağıdaki örnek, MFA kayıt sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma sayfası eposta doğrulaması UX öğeleri](./media/localization-string-ids/localization-mfa1.png)

Aşağıdaki örnek, MFA doğrulama sayfasında bazı Kullanıcı arabirimi öğelerinin kullanımını gösterir:

![Kaydolma sayfası eposta doğrulaması UX öğeleri](./media/localization-string-ids/localization-mfa2.png)

## <a name="verification-display-control-user-interface-elements"></a>Doğrulama görüntüleme denetimi kullanıcı arabirimi öğeleri

[Doğrulama görüntüleme denetimi](display-control-verification.md) için kimlikler şunlardır

| Kimlik | Varsayılan değer |
| -- | ------------- |
|verification_control_but_change_claims |Değiştir |
|verification_control_fail_send_code |Kod gönderilemedi, lütfen daha sonra yeniden deneyin. |
|verification_control_fail_verify_code |Kod doğrulanamadı, lütfen daha sonra yeniden deneyin. |
|verification_control_but_send_code |Kodu gönder |
|verification_control_but_send_new_code |Yeni kod gönder |
|verification_control_but_verify_code |Kodu doğrula |

## <a name="one-time-password-error-messages"></a>Bir kerelik parola hata iletileri
Bir [kerelik parola teknik profili](one-time-password-technical-profile.md) hata Iletileri için kimlikler şunlardır

| Kimlik | Varsayılan değer |
| -- | ------------- |
|UserMessageIfMaxRetryAttempted |Bir kerelik parola belirtilen doğrulama, en fazla deneme sayısını aştı |
|Usermessageifsessionffnotexist |Bir kerelik parola doğrulama oturumunun süresi doldu |
|UserMessageIfSessionConflict |Bir kerelik parola doğrulama oturumunun çakışması |
|Usermessageifınvalidcode |Doğrulama için girilen bir kerelik parola yanlış |








