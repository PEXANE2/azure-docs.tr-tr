---
title: Parolasız güvenlik anahtarı oturum açma (önizleme) - Azure Active Directory
description: FIDO2 güvenlik anahtarlarını kullanarak Azure AD'de parolasız güvenlik anahtarı oturum açmayı etkinleştirin (önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 02/12/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 066ab7892bed6e7505e7ee114ff37a7850ef5c9b
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81450930"
---
# <a name="enable-passwordless-security-key-sign-in-preview"></a>Parolasız güvenlik anahtarı oturum açma (önizleme) etkinleştirme

Bugün parola kullanan ve paylaşılan bir bilgisayar ortamına sahip kuruluşlar için güvenlik anahtarları, çalışanların kullanıcı adı veya parola girmeden kimlik doğrulamaları için sorunsuz bir yol sağlar. Güvenlik anahtarları çalışanlar için daha iyi üretkenlik sağlar ve daha iyi güvenliğe sahiptir.

Bu belge, güvenlik anahtarı tabanlı parolasız kimlik doğrulamasını etkinleştirme üzerinde odaklanır. Bu makalenin sonunda, FIDO2 güvenlik anahtarını kullanarak Azure REKLAM hesabınızla web tabanlı uygulamalarda oturum açabilirsiniz.

|     |
| --- |
| FIDO2 güvenlik anahtarları, Azure Etkin Dizini'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın|
|     |

## <a name="requirements"></a>Gereksinimler

- [Azure Multi-Factor Authentication](howto-mfa-getstarted.md)
- [Birleşik güvenlik bilgileri kaydı önizlemesi](concept-registration-mfa-sspr-combined.md)
- Uyumlu [FIDO2 güvenlik tuşları](concept-authentication-passwordless.md#fido2-security-keys)
- WebAuthN Windows 10 sürüm 1809 veya daha yüksek** gerektirir

Web uygulamalarında ve hizmetlerinde oturum açmak için güvenlik anahtarlarını kullanmak için WebAuthN protokolünü destekleyen bir tarayıcınız olması gerekir. Bunlar arasında Microsoft Edge, Chrome, Firefox ve Safari sayılabilir.

## <a name="prepare-devices-for-preview"></a>Cihazları önizlemeiçin hazırlama

Azure AD, windows 10 sürüm 1809 veya daha yüksek bir sürümünü çalıştırmanız gerekir ile pilot cihazlara katıldı. En iyi deneyim Windows 10 sürüm 1903 veya daha yüksektir.

Karma Azure AD birleştirilmiş aygıtlar Windows 10 Insider Build 18945 veya daha yeni çalıştırmalıdır.

## <a name="enable-passwordless-authentication-method"></a>Parolasız kimlik doğrulama yöntemini etkinleştirme

### <a name="enable-the-combined-registration-experience"></a>Birleştirilmiş kayıt deneyimini etkinleştirme

Parolasız kimlik doğrulama yöntemlerinin kayıt özellikleri, birleştirilmiş kayıt özelliğine dayanır. Birleştirilmiş kaydı etkinleştirmek için [birleştirme güvenlik bilgi kaydı (önizleme)](howto-registration-mfa-sspr-combined.md)etkinleştir) makaledeki adımları izleyin.

### <a name="enable-fido2-security-key-method"></a>FIDO2 güvenlik anahtarı yöntemini etkinleştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > Kimlik Doğrulama**yöntemlerine** > göz atın Kimlik doğrulama yöntemi ilkesi **(Önizleme)**.
1. **FIDO2 Güvenlik Anahtarı**yöntemi altında aşağıdaki seçenekleri seçin:
   1. **Etkinleştir** - Evet veya Hayır
   1. **Hedef** - Tüm kullanıcılar veya Select kullanıcıları
1. Yapılandırmayı **kaydedin.**

## <a name="user-registration-and-management-of-fido2-security-keys"></a>FIDO2 güvenlik anahtarlarının kullanıcı kaydı ve yönetimi

1. 'ye [https://myprofile.microsoft.com](https://myprofile.microsoft.com)göz atın.
1. Zaten değilseniz oturum açın.
1. **Güvenlik Bilgileri'ni**tıklatın.
   1. Kullanıcının zaten kayıtlı en az bir Azure Çok Faktörlü Kimlik Doğrulama yöntemi varsa, hemen bir FIDO2 güvenlik anahtarı kaydedebilir.
   1. Kayıtlı en az bir Azure Çok Faktörlü Kimlik Doğrulama yöntemi yoksa, bir tane eklemeleri gerekir.
1. **Ekle yöntemini** tıklatarak ve **Güvenlik anahtarını**seçerek FIDO2 Güvenlik tuşu ekleyin.
1. USB aygıtı veya **NFC** **aygıtını** seçin.
1. Anahtarınızı hazırlayın ve **İleri'yi**seçin.
1. Bir kutu görüntülenir ve kullanıcıdan güvenlik anahtarınız için bir PIN oluşturmasını/girmesini ister, ardından anahtar için gerekli hareketi biyometrik veya dokunma olarak gerçekleştirir.
1. Kullanıcı, birleşik kayıt deneyimine döndürülür ve anahtar için anlamlı bir ad sağlaması istenir, böylece kullanıcı birden fazla kayıt sahibi olup olmadığını belirleyebilir. **İleri**’ye tıklayın.
1. İşlemi tamamlamak için **Bitti'yi** tıklatın.

## <a name="sign-in-with-passwordless-credential"></a>Parolasız kimlik bilgisi ile oturum açma

Aşağıdaki örnekte bir kullanıcı FIDO2 güvenlik anahtarını zaten sağlamıştır. Kullanıcı, Windows 10 sürüm 1809 veya daha yüksek bir üzerinde desteklenen bir tarayıcı içinde KENDI FIDO2 güvenlik anahtarı ile web'de oturum seçebilirsiniz.

![Güvenlik anahtarı oturum açma Microsoft Edge](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## <a name="troubleshooting-and-feedback"></a>Sorun giderme ve geri bildirim

Bu özelliği niönizlerken geri bildirim paylaşmak veya sorunlarla karşılaşmak istiyorsanız, aşağıdaki adımları kullanarak Windows Geri Bildirim Hub uygulaması üzerinden paylaşın:

1. **Geri Bildirim Hub'ı** başlatın ve oturum açmış olduğunuzdan emin olun.
1. Aşağıdaki kategoriler altında geri bildirim gönderin:
   - Kategori: Güvenlik ve Gizlilik
   - Alt kategori: FIDO
1. Günlükleri yakalamak **için, Sorunumu Yeniden Oluşturma** seçeneğini kullanın

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="security-key-provisioning"></a>Güvenlik anahtarı sağlama

Yönetici sağlama ve güvenlik anahtarlarının sağlanmasının geri verilmesi genel önizlemede kullanılamaz.

### <a name="upn-changes"></a>UPN değişiklikleri

Karma Azure AD'de UPN değişikliğine ve Azure AD'nin birleştiği cihazlarda değişiklik yapılmasına olanak tanıyan bir özelliği desteklemeye çalışıyoruz. Bir kullanıcının UPN'si değişirse, fido2 güvenlik anahtarlarını artık değişikliği hesaba katmak için değiştiremezsiniz. Çözünürlük aygıtı sıfırlamak ve kullanıcıyeniden kayıt yapmak tır.

## <a name="next-steps"></a>Sonraki adımlar

[FIDO2 güvenlik anahtarı Windows 10 oturum açma](howto-authentication-passwordless-security-key-windows.md)

[FIDO2 kimlik doğrulamasını şirket içi kaynaklara etkinleştirme](howto-authentication-passwordless-security-key-on-premises.md)

[Cihaz kaydı hakkında daha fazla bilgi edinin](../devices/overview.md)

[Azure Çok Faktörlü Kimlik Doğrulama hakkında daha fazla bilgi edinin](../authentication/howto-mfa-getstarted.md)
