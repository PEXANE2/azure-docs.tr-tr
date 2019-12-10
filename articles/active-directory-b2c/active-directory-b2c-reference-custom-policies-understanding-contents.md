---
title: Özel ilke başlangıç paketinin bileşenleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C özel ilke başlangıç paketindeki ilkelere genel bakış.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7d49bd9af80b1bb9bd86466269b14ba0a47181e0
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74948190"
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a>Azure AD B2C özel Ilke başlangıç paketinin özel ilkelerini anlama

Bu bölümde, **Başlangıç paketiyle** birlikte gelen B2C_1A_base ilkesinin tüm temel öğeleri listelenir ve bu, *B2C_1A_base_extensions ilkesinin*devralımı aracılığıyla kendi ilkelerinizi yazmak için yararlanılabilir.

Bu nedenle, daha önce önceden tanımlanmış talep türlerine, talep dönüştürmelerine, içerik tanımlarına, teknik profillerinin bulunduğu talep sağlayıcılarına ve temel Kullanıcı günlerine odaklanmaktadır.

> [!IMPORTANT]
> Microsoft, belirtilen bilgilerle ilgili olarak, açık veya zımni hiçbir garanti vermez. Değişiklikler herhangi bir zamanda, GA saati, GA saati veya sonrasında gerçekleştirilebilir.

Hem kendi ilkeleriniz hem de B2C_1A_base_extensions ilkesi bu tanımları geçersiz kılabilir ve gerektiğinde ek bir ilke sunarak bu üst ilkeyi genişletebilirler.

*B2C_1A_base ilkesinin* temel öğeleri talep türleri, talep dönüştürmeleri ve içerik tanımlardır. Bu öğelerin, kendi ilkelerinizde ve *B2C_1A_base_extensions ilkesinde*başvurulması açıktır.

## <a name="claims-schemas"></a>Talep şemaları

Bu talep şemaları üç bölüme ayrılmıştır:

1.  Kullanıcının düzgün çalışması için gerekli olan en düşük talepleri listeleyen ilk bölüm.
2.  Sorgu dizesi parametreleri için gereken talepleri ve diğer talep sağlayıcılarına geçirilecek diğer özel parametreleri listeleyen, özellikle de kimlik doğrulaması için login.microsoftonline.com olan ikinci bir bölüm. **Lütfen bu talepleri değiştirmeyin**.
3.  Sonuç olarak, Kullanıcı tarafından toplanabilecek, dizinde depolanan ve oturum açma sırasında belirteçlerde gönderilen ek, isteğe bağlı talepler listeleyen üçüncü bir bölüm. Kullanıcıdan toplanacak ve/veya belirteçte gönderilen yeni talep türü, bu bölüme eklenebilir.

> [!IMPORTANT]
> Talepler şeması, parolalar ve Kullanıcı adları gibi belirli taleplerde kısıtlamalar içerir. Trust Framework (TF) ilkesi, Azure AD 'yi başka bir talep sağlayıcı olarak değerlendirir ve tüm kısıtlamaları özel ilkede modellenmiş olarak davranır. Daha fazla kısıtlama eklemek için bir ilke değiştirilebilir veya kimlik bilgisi depolaması için kendi kısıtlamalarına sahip olan başka bir talep sağlayıcısı kullanılır.

Kullanılabilir talep türleri aşağıda listelenmiştir.

### <a name="claims-that-are-required-for-the-user-journeys"></a>Kullanıcının yolculukları için gereken talepler

Kullanıcı ilerurları düzgün çalışması için aşağıdaki talepler gereklidir:

| Talep türü | Açıklama |
|-------------|-------------|
| *UserID* | Kullanıcı adı |
| *Signınname* | Oturum açma adı |
| *tenantId* | Azure AD B2C içindeki Kullanıcı nesnesinin kiracı tanımlayıcısı (KIMLIĞI) |
| *objectId* | Azure AD B2C içindeki Kullanıcı nesnesinin nesne tanımlayıcısı (KIMLIĞI) |
| *Parola* | Parola |
| *newPassword* | |
| *reenterPassword* | |
| *passwordPolicies* | Parola gücünü, süre sonunu vb. anlamak için Azure AD B2C tarafından kullanılan parola ilkeleri. |
| *alt* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *displayName* | |
| *strongAuthenticationPhoneNumber* | Kullanıcının telefon numarası |
| *Doğrulanan. strongAuthenticationPhoneNumber* | |
| *E-posta* | Kullanıcıyla iletişim kurmak için kullanılabilen e-posta adresi |
| *signInNamesInfo.emailAddress* | Kullanıcının oturum açmak için kullanabileceği e-posta adresi |
| *Diğer postalar* | Kullanıcıyla iletişim kurmak için kullanılabilen e-posta adresleri |
| *userPrincipalName* | Azure AD B2C depolanan Kullanıcı adı |
| *upnUserName* | Kullanıcı asıl adı oluşturmak için Kullanıcı adı |
| *mailNickName* | Azure AD B2C depolanan kullanıcının posta Nick adı |
| *newUser* | |
| *executed-SelfAsserted-Input* | Kullanıcıdan özniteliklerin toplanıp toplanmadığını belirten talep |
| *executed-PhoneFactor-Input* | Kullanıcıdan yeni bir telefon numarasının toplandığını belirten talep |
| *authenticationSource* | Kullanıcının kimlik doğrulamasının sosyal kimlik sağlayıcısı, login.microsoftonline.com veya yerel hesapta yapılıp yapılmayacağını belirtir |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Sorgu dizesi parametreleri ve diğer özel parametreler için gereken talepler

Diğer talep sağlayıcılarına özel parametreleri (bazı sorgu dizesi parametreleri de dahil olmak üzere) geçirmek için aşağıdaki talepler gereklidir:

| Talep türü | Açıklama |
|-------------|-------------|
| *nux* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *nca* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *isteme* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *mkt* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *LC* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *grant_type* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *kapsam* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *client_id* | Yerel hesap kimlik doğrulaması için login.microsoftonline.com 'e geçirilen özel parametre |
| *objectIdFromSession* | Bir SSO oturumundan nesne KIMLIĞININ alındığını göstermek için varsayılan oturum yönetim sağlayıcısı tarafından belirtilen parametre |
| *isActiveMFASession* | Kullanıcının etkin bir MFA oturumu olduğunu göstermek için MFA oturum yönetimi tarafından sunulan parametre |

### <a name="additional-optional-claims-that-can-be-collected"></a>Toplanabilecek ek (isteğe bağlı) talepler

Aşağıdaki talepler, kullanıcılardan toplanabilecek, dizinde depolanan ve belirtece gönderilebilecek ek taleplerdir. Daha önce özetlenen şekilde, bu listeye ek talepler eklenebilir.

| Talep türü | Açıklama |
|-------------|-------------|
| *givenName* | Kullanıcının verilen adı (ad olarak da bilinir) |
| *Soyadı* | Kullanıcının Soyadı (aile adı veya soyadı olarak da bilinir) |
| *Extension_picture* | Kullanıcının sosyal resmini |

## <a name="claim-transformations"></a>Talep dönüştürmeleri

Kullanılabilir talep dönüştürmeleri aşağıda listelenmiştir.

| Talep dönüştürme | Açıklama |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>İçerik tanımları

Bu bölümde *B2C_1A_base* ilkesinde zaten belirtilen içerik tanımları açıklanmaktadır. Bu içerik tanımlarının, kendi ilkeleriniz ve *B2C_1A_base_extensions* ilkesi için gerektiği şekilde başvurulması, geçersiz kılınması ve/veya genişletilmesi açıktır.

| Talep sağlayıcı | Açıklama |
|-----------------|-------------|
| *Facebook* | |
| *Yerel hesap oturum açma* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Kendi kendini onaylanan* | |
| *Yerel hesap* | |
| *Oturum yönetimi* | |
| *Trustframework Ilke altyapısı* | |
| *Teknisyen* | |
| *Belirteç veren* | |

## <a name="technical-profiles"></a>Teknik profiller

Bu bölümde, *B2C_1A_base* ilkesinde talep sağlayıcı başına önceden tanımlanmış teknik profiller gösterilmektedir. Bu teknik profillerin, kendi ilkeleriniz ve *B2C_1A_base_extensions* ilkesinde olması gerektiği şekilde daha fazla başvurulması, geçersiz kılınabilmesi ve/veya genişletilmesi açıktır.

### <a name="technical-profiles-for-facebook"></a>Facebook için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *Facebook-OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>Yerel hesap oturum açma için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *Oturum açma-etkileşimsiz* | |

### <a name="technical-profiles-for-phone-factor"></a>Telefon faktörü için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *PhoneFactor-giriş* | |
| *PhoneFactor-ınputorverify* | |
| *PhoneFactor-doğrula* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Azure Active Directory için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *AAD-ortak* | Diğer AAD-xxx Teknik profillerinin içerdiği teknik profil |
| *AAD-UserWriteUsingAlternativeSecurityId* | Sosyal oturumlar için teknik profil |
| *AAD-Userreadusingalternativesecurityıd* | Sosyal oturumlar için teknik profil |
| *AAD-Userreadusingalternativesecurityıd-NoError* | Sosyal oturumlar için teknik profil |
| *AAD-UserWritePasswordUsingLogonEmail* | Yerel hesaplar için teknik profil |
| *AAD-Userreadusingemadresi* | Yerel hesaplar için teknik profil |
| *AAD-UserWriteProfileUsingObjectId* | ObjectID kullanarak Kullanıcı kaydını güncelleştirmek için teknik profil |
| *AAD-Userwritephonenumberusingobjectıd* | ObjectID kullanarak Kullanıcı kaydını güncelleştirmek için teknik profil |
| *AAD-Userwritepasswordusingobjectıd* | ObjectID kullanarak Kullanıcı kaydını güncelleştirmek için teknik profil |
| *AAD-UserReadUsingObjectId* | Teknik profil, Kullanıcı kimlik doğrulamasından sonra verileri okumak için kullanılır |

### <a name="technical-profiles-for-self-asserted"></a>Kendi kendine onaylanan için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *Selfasted-sosyal* | |
| *SelfAsserted-ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Yerel hesap için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Oturum yönetimi için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *SM-noop* | |
| *SM-AAD* | |
| *SM-SocialSignup* | Kayıt ve oturum açma arasındaki AAD oturumunu belirsizliğini ortadan kaldırmak için profil adı kullanılıyor |
| *SM-SocialLogin* | |
| *SM-MFA* | |

### <a name="technical-profiles-for-the-trust-framework-policy-engine"></a>Güven çerçevesi ilke altyapısı için teknik profiller

Şu anda, **Trustframework Policy Engine teknisyen** talep sağlayıcısı için hiçbir teknik profil tanımlanmamıştır.

### <a name="technical-profiles-for-token-issuer"></a>Belirteç veren için teknik profiller

| Teknik profil | Açıklama |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Kullanıcı yolculukları

Bu bölümde, *B2C_1A_base* ilkesinde zaten belirtilen kullanıcı ile bağlantılı olan kullanıcılar gösterilmektedir. Bu kullanıcının başvurmasının yanı sıra, *B2C_1A_base_extensions* ilkesinde ve kendi ilkeleriniz için gereken şekilde daha fazla başvurulması, geçersiz kılınmak ve/veya genişletilme açıktır.

| Kullanıcı yolculuğu | Açıklama |
|--------------|-------------|
| *Ol* | |
| *Giriş* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |
