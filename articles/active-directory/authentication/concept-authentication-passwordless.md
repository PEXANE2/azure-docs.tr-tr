---
title: Azure Active Directory parolasız oturum açma (Önizleme)
description: FIDO2 güvenlik anahtarlarını veya Microsoft Authenticator uygulamasını (Önizleme) kullanarak Azure AD 'de passwordless oturum açma
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ae8f6854241240249cb3b7494872cbbd8fd41e6
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68823792"
---
# <a name="what-is-passwordless"></a>Passwordless nedir?

Multi-Factor Authentication (MFA), kuruluşunuzu güvenli hale getirmenin harika bir yoludur, ancak kullanıcılar parolalarını anımsamak için gereken ek katman ile daha kolay bir şekilde yararlanar. Parola kaldırıldığı ve sizin veya bildiğiniz bir şeyi içeren bir şekilde değiştirildiği için passwordless kimlik doğrulama yöntemleri daha uygundur.

|   | Sahip olduğunuz bir şey | Sizin veya bildiğiniz bir şey |
| --- | --- | --- |
| Parolasız | Telefon veya güvenlik anahtarı | Biyometrik veya PIN |

Her kuruluş, kimlik doğrulamasına geldiğinde farklı gereksinimlere sahiptir. Microsoft şu anda Windows bilgisayarlarımız için Windows Hello 'yu sunmaktadır. Microsoft Authenticator App ve FIDO2 güvenlik anahtarlarını passwordless ailesine ekliyoruz.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator Uygulaması

Çalışanın telefonunun parolasız kimlik doğrulama yöntemi olmasına izin verin. Microsoft Authenticator uygulamasını bir parolanın yanı sıra uygun bir Multi-Factor Authentication seçeneği olarak zaten kullanıyor olabilirsiniz. Ancak şimdi, passwordless seçeneği olarak kullanılabilir.

![Microsoft Authenticator uygulamayla Microsoft Edge 'de oturum açın](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

Tüm iOS veya Android telefonlarını, kullanıcıların telefonda bir bildirim alarak, telefonda bir sayıyla ve daha sonra biyometrik kullanımlarını kullanarak herhangi bir platformda veya tarayıcıda oturum açmasına izin vererek, güçlü, passworddaha az bir kimlik bilgisi olarak etkinleştirir. onaylamak için dokunmatik veya yüz) veya PIN.

## <a name="fido2-security-keys"></a>FIDO2 güvenlik anahtarları

FIDO2 güvenlik anahtarları, herhangi bir form faktöründe yer alan unphishable standartlara dayalı, passwordless kimlik doğrulama yöntemidir. Hızlı kimlik çevrimiçi (FIDO), passwordless kimlik doğrulaması için açık bir standarttır. Kullanıcıların ve kuruluşların, bir dış güvenlik anahtarı veya bir cihaza yerleşik bir platform anahtarı kullanarak bir Kullanıcı adı veya parola olmadan kendi kaynaklarında oturum açmasını sağlamak için standart kullanmasına olanak sağlar.

Çalışanlar, Azure Active Directory katılmış Windows 10 makinelerinde (sürüm 1809 veya üzeri) oturum açmak için dış güvenlik anahtarlarını kullanabilir ve bulut kaynaklarında çoklu oturum açma imkanı alabilir. Ayrıca, desteklenen tarayıcılarda oturum açabilir.

![Bir güvenlik anahtarıyla Microsoft Edge 'de oturum açın](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

FIDO Alliance tarafından FIDO2 sertifikalı çok sayıda anahtar olmakla kalmaz, Microsoft, en yüksek güvenlik ve en iyi deneyimi sağlamak için FIDO2 CTAP belirtiminin bazı isteğe bağlı uzantılarını satıcı tarafından uygulanması gerekir.

Bir güvenlik anahtarı , FIDO2 CTAP protokolünden Microsoft ile uyumlu olmak için aşağıdaki özellikleri ve uzantıları gerçekleştirmelidir:

| # | Özellik/uzantı güveni | Bu özellik veya uzantı neden gereklidir? |
| --- | --- | --- |
| 1\. | Yerleşik anahtar | Bu özellik, kimlik bilgilerinizin güvenlik anahtarında depolandığı güvenlik anahtarının taşınabilir olmasını sağlar. |
| 2 | İstemci PIN 'i | Bu özellik, kimlik bilgilerinizi ikinci bir faktörle korumanıza olanak sağlar ve Kullanıcı arabirimine sahip olmayan güvenlik anahtarları için geçerlidir. |
| 3 | HMAC-gizli | Bu uzantı, hatta çevrimdışı veya uçak modunda cihazınızda oturum açabilmenizi sağlar. |
| 4 | RP başına birden çok hesap | Bu özellik, Microsoft hesabı ve Azure Active Directory gibi birden çok hizmet arasında aynı güvenlik anahtarını kullanmanıza da sağlar. |

Aşağıdaki sağlayıcılar, paswordless deneyimiyle uyumlu oldukları bilinen farklı form faktörlerinin FIDO2 güvenlik anahtarlarını sunmaktadır. Microsoft, müşterilerin ve FIDO Alliance ile iletişim kurarak bu anahtarların güvenlik özelliklerini değerlendirmesini önermektedir.

| Sağlayıcı | İlgili kişi |
| --- | --- |
| Yılıco | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| CIHAZDAN | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurlik | [https://ensurity.com/contact-us.html](https://ensurity.com/contact-us.html) |
| eWBM | [https://www.ewbm.com/page/sub1_5](https://www.ewbm.com/page/sub1_5) |

Bir satıcısıysanız ve cihazınızı bu listede almak istiyorsanız, iletişim kurun [Fido2Request@Microsoft.com](mailto:Fido2Request@Microsoft.com).

FIDO2 güvenlik anahtarları, çok güvenliğe duyarlı olan veya telefon numarası ikinci bir faktör olarak kullanabilecek senaryolar veya çalışanlar olan kuruluşlar için harika bir seçenektir.

## <a name="what-scenarios-work-with-the-preview"></a>Önizlemele hangi senaryolar çalışıyor?

- Yöneticiler, kiracı için passwordless kimlik doğrulama yöntemlerini etkinleştirebilir
- Yöneticiler tüm kullanıcıları hedefleyebilir veya kiracının her bir yöntemi için kullanıcıları/grupları seçebilir
- Son kullanıcılar, bu passwordless kimlik doğrulama yöntemlerini hesap portalında kaydedebilir ve yönetebilir
- Son kullanıcılar bu passwordless kimlik doğrulama yöntemleriyle oturum açabilirler
   - Microsoft Authenticator uygulaması: Tüm tarayıcılarda, Windows 10 kutudan çıkar (OOBE) kurulumu sırasında ve herhangi bir işletim sisteminde tümleşik mobil uygulamalarla birlikte Azure AD kimlik doğrulamasının kullanıldığı senaryolarda çalışacaktır.
   - Güvenlik anahtarları: , Microsoft Edge gibi desteklenen tarayıcılarda Windows 10 sürüm 1809 veya üzeri ve Web için kilit ekranında çalışır.

## <a name="next-steps"></a>Sonraki adımlar

[Kuruluşunuzda FIDO2 güvenlik anahtarı passwordlesss seçeneklerini etkinleştirin](howto-authentication-passwordless-security-key.md)

[Kuruluşunuzda telefon tabanlı parolasız seçenekleri etkinleştirin](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Dış bağlantılar

[FıDO Alliance](https://fidoalliance.org/)

[FIDO2 CTAP belirtimi](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
