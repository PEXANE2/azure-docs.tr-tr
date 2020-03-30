---
title: Microsoft Authenticator uygulaması nedir? - Azure Etkin Dizin | Microsoft Dokümanlar
description: Ne olduğu, nasıl çalıştığı ve içeriğin bu bölümünde hangi bilgilerin yer aldığı dahil olmak üzere Microsoft Authenticator uygulaması hakkında bilgi edinin.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/15/2020
ms.author: curtand
ms.openlocfilehash: c95ae2e48a7d5a3b9e9e28b1d4e14b670205c043
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77062294"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması nedir?

Microsoft Authenticator uygulaması, iki faktörlü doğrulama kullanırsanız hesaplarınızda oturum açmanıza yardımcı olur. İki faktörlü doğrulama, özellikle hassas bilgileri görüntülerken hesaplarınıza daha güvenli bir şekilde erişmenize yardımcı olur. Parolalar unutulabileceğinden, çalınabilir veya tehlikeye girebildiği için, iki faktörlü doğrulama, diğer kişilerin zorla girebetmesini zorlaştırarak hesabınızı korumaya yardımcı olan ek bir güvenlik adımıdır.

Microsoft Authenticator uygulamasını aşağıdakiler dahil olmak üzere birden çok şekilde kullanabilirsiniz:

- Kullanıcı adınız ve parolanızla oturum açtıktan sonra kimlik doğrulama istemine yanıt verin.

- Kullanıcı adınızı, kimlik doğrulama uygulamanızı ve parmak izinizi, yüzünüzveya PIN'inizle mobil cihazınızı kullanarak parola girmeden oturum açın.

- Kimlik doğrulayıcı uygulamaları destekleyen diğer hesaplar için kod üreteci olarak.

> [!Important]
> Microsoft Authenticator uygulaması, iki faktörlü doğrulama kullanan ve zaman tabanlı tek seferlik parola (TOTP) standartlarını destekleyen tüm hesaplarla çalışır.
>
>Bu makale, Microsoft Authenticator uygulamasını güvenlik doğrulama yöntemi olarak indirmeye ve kullanmaya çalışan kullanıcılar için hazırlanmıştır. Çalışanlarınız ve diğer kullanımlar için Kimlik Doğrulayıcı uygulamasını kullanarak parolasız oturum açma hakkında bilgi arayan bir yöneticiyseniz, [Microsoft Authenticator uygulamasıyla (önizleme) parolasız oturum açmayı etkinleştir'e](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-phone)bakın.

## <a name="terminology"></a>Terminoloji

| Sözleşme Dönemi|Açıklama|
| ----|-----------|
| İki faktörlü doğrulama | Parola ve PIN gibi yalnızca iki doğrulama bilgisi kullanmanızı gerektiren bir doğrulama işlemi. Microsoft Authenticator uygulaması hem standart iki faktörlü doğrulamayı hem de parolasız oturum açmayı destekler. |
| Çok faktörlü kimlik doğrulaması (MFA) | Tüm iki faktörlü doğrulama, kuruluşunuzun gereksinimlerine bağlı olarak *en az* iki denetim bilgisi kullanmanızı gerektiren çok faktörlü kimlik doğrulamadır. |
| Microsoft hesabı (msa olarak da adlandırılır) | Outlook, OneDrive, Xbox LIVE veya Office 365 gibi tüketici odaklı Microsoft ürünlerinize ve bulut hizmetlerine erişmek için kendi kişisel hesaplarınızı oluşturursunuz. Microsoft hesabınız, Microsoft tarafından işletilen Microsoft tüketici kimliği hesap sisteminde oluşturulur ve depolanır. |
| İş veya okul hesabı | Kuruluşunuz, Microsoft Azure, Windows Intune ve Office 365 gibi dahili ve kısıtlanmış kaynaklara erişmenize izin vermek için çalışmanızı veya okul hesabınızı (örneğin) alain@contoso.comoluşturur. |
| Doğrulama kodu | Her eklenen hesabın altında kimlik doğrulayıcı uygulamasında görünen altı basamaklı kod. Doğrulama kodu her 30 saniyede bir değişir ve birinin kodu birden çok kez kullanmasını engeller. Bu, tek seferlik parola (OTP) olarak da bilinir. |

## <a name="how-two-factor-verification-works-with-the-app"></a>Uygulamayla iki faktörlü doğrulama nasıl çalışır?

İki faktörlü doğrulama, Microsoft Authenticator uygulamasıyla aşağıdaki yollarla çalışır:

- **Bildirim.** İş veya okul hesabınız veya kişisel Microsoft hesabınız için oturum açtığınızda oturum açtığınızda kullanıcı adınızı ve parolanızı yazın ve microsoft Authenticator uygulaması **oturum açma**onaylamanızı isteyen bir bildirim gönderir. Oturum açma girişimini tanıyorsanız **Onayla'yı** seçin. Aksi takdirde, **Reddet'i**seçin. **Reddet'i**seçerseniz, isteği sahte olarak da işaretleyebilirsiniz.

- **Doğrulama kodu.** İş veya okul hesabınız veya kişisel Microsoft hesabınız için oturum açtığınızda oturum açtığınızda kullanıcı adınızı ve parolanızı yazın ve microsoft Authenticator uygulamasının **Hesaplar** ekranından ilişkili doğrulama kodunu kopyalayın. Doğrulama kodu, tek seferlik parola (OTP) kimlik doğrulaması olarak da bilinir.

- **Şifresiz oturum açma.** İş veya okul hesabınız veya kişisel Microsoft hesabınız için oturum açtığınız cihaza kullanıcı adınızı yazın ve ardından parmak izinizi, yüzünüzü veya PIN'inizi kullanarak sizin olduğunuzu doğrulamak için mobil cihazınızı kullanın. Bu yöntem için parolanızı girmeniz gerekmez.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Cihazınızın biyometrik özelliklerini kullanıp kullanmayacağınız

Kimlik doğrulama işlemini tamamlamak için pin kullanıyorsanız, Microsoft Authenticator uygulamasını bunun yerine cihazınızın parmak izini veya yüz tanıma (biyometrik) özelliklerini kullanacak şekilde ayarlayabilirsiniz. Bunu, PIN yerine kimlik olarak cihazınızın biyometrik özelliklerini kullanma seçeneğini seçerek hesabınızı doğrulamak için kimlik doğrulayıcı uygulamasını ilk kez kullandığınızda ayarlayabilirsiniz.

## <a name="who-decides-if-you-use-this-feature"></a>Bu özelliği kullanıp kullanmadığınıza kim karar verir?

Hesap türünüze bağlı olarak, kuruluşunuz iki faktörlü doğrulama kullanmanız gerektiğine karar verebilir veya kendiniz karar verebilirsiniz.

- **İş veya okul hesabı.** Bir iş veya okul hesabı kullanıyorsanız (örneğin,), alain@contoso.combelirli doğrulama yöntemleriyle birlikte iki faktörlü doğrulama kullanıp kullanmanız kuruluşunuzun dır. İş veya okul hesabınızı Microsoft Authenticator uygulamasına ekleme hakkında daha fazla bilgi için [bkz.](user-help-auth-app-add-work-school-account.md)

- **Kişisel Microsoft hesabı.** Kişisel Microsoft hesaplarınız için iki faktörlü doğrulama ayarlamayı alain@outlook.comseçebilirsiniz (örneğin, ). Kişisel Microsoft hesabınızı ekleme hakkında daha fazla bilgi için [bkz.](user-help-auth-app-add-personal-ms-account.md)

- **Microsoft hesabı olmayan.** Microsoft dışı hesaplarınız için iki faktörlü doğrulama ayarlamayı seçebilirsiniz alain@gmail.com(örneğin, ). Microsoft dışı hesaplarınız bu terimi, iki faktörlü doğrulamayı kullanmayabilir, ancak **özelliği Güvenlik** veya **Oturum Açma** ayarlarında bulabilmelisiniz. Microsoft Authenticator uygulaması, TOTP standartlarını destekleyen tüm hesaplarla çalışır. Microsoft'a ait olmayan hesaplarınızı ekleme hakkında daha fazla bilgi [için](user-help-auth-app-add-non-ms-account.md)bkz.

## <a name="in-this-section"></a>Bu bölümde

| Makale | Açıklama |
| ------ | ------------ |
| [Uygulamayı indirme ve yükleme](user-help-auth-app-download-install.md) | Android ve iOS çalıştıran cihazlar için Microsoft Authenticator uygulamasını nereden ve nasıl yükleyebileceğinizi açıklar. |
| [İş veya okul hesaplarınızı ekleme](user-help-auth-app-add-work-school-account.md) | Microsoft Authenticator uygulamasına çeşitli çalışmanızı veya okul unuzu ve kişisel hesaplarınızı nasıl ekleyeceğiniz açıklanır. |
| [Kişisel hesaplarınızı ekleyin](user-help-auth-app-add-personal-ms-account.md) | Kişisel Microsoft hesaplarınızı Microsoft Authenticator uygulamasına nasıl ekleyeceğinizaçık. |
| [Microsoft dışı hesaplarınızı ekleme](user-help-auth-app-add-non-ms-account.md) | Microsoft Kimlik Doğrulayıcı uygulamasına Microsoft olmayan hesaplarınızı nasıl ekleyeceğiniz açıklanır. |
| [Hesaplarınızı el ile ekleyin](user-help-auth-app-add-account-manual.md) | Sağlanan QR kodunu tarayamıyorsanız, hesaplarınızı Microsoft Authenticator uygulamasına el ile nasıl ekleyeceğiniz açıklanır. |
| [Uygulamayı kullanarak oturum açma](user-help-auth-app-sign-in.md) | Microsoft Authenticator uygulamasını kullanarak çeşitli hesaplarınızda nasıl oturum açış layacağınızı açıklar.|
| [Hesap kimlik bilgilerini yedekleme ve kurtarma](user-help-auth-app-backup-recovery.md) | Microsoft Authenticator uygulamasını kullanarak hesap kimlik bilgilerinizi yedekleme ve kurtarma hakkında bilgi sağlar. |
| [Microsoft Authenticator uygulaması hakkında SSS](user-help-auth-app-faq.md) | Uygulamayla ilgili sık sorulan soruların yanıtlarını sunar. |
