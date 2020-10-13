---
title: Microsoft Authenticator App-Azure Active Directory ile passwordless oturum açma
description: Microsoft Authenticator uygulamasını kullanarak Azure AD 'de passwordless oturum açmayı etkinleştirme (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 09/29/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 053a489993c31344b96e83253c88eed93b27b145
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91964834"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Microsoft Authenticator uygulamasıyla passwordless oturum açmayı etkinleştirme (Önizleme)

Microsoft Authenticator uygulaması, herhangi bir Azure AD hesabında parola kullanılmadan oturum açmak için kullanılabilir. [İş Için Windows Hello](/windows/security/identity-protection/hello-for-business/hello-identity-verification)teknolojisine benzer şekilde, Microsoft Authenticator bir cihaza bağlı olan ve biyometrik ya da PIN kullanan kullanıcı kimlik bilgilerini etkinleştirmek için anahtar tabanlı kimlik doğrulaması kullanır. Bu kimlik doğrulama yöntemi, mobil ve Microsoft kimlik doğrulama kitaplıklarıyla tümleştirilen herhangi bir uygulama veya Web sitesi ile birlikte herhangi bir cihaz platformunda kullanılabilir.

![Kullanıcının oturum açma onayını onaylamasını isteyen tarayıcı oturum açma örneği](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Kullanıcı adı girdikten sonra parola istemi görmek yerine, Microsoft Authenticator uygulamadan telefonla oturum açmayı etkinleştiren bir kişi, uygulamalarından bir sayıya dokunmasını isteyen bir ileti görür. Uygulamada oturum açma işlemini tamamlaması için, kullanıcının sayıyla eşleşmesi, **Onayla**' yı seçmesi, sonra PIN veya biyometri sağlaması gerekir.

## <a name="prerequisites"></a>Ön koşullar

Microsoft Authenticator uygulamasıyla passwordless telefon oturum açma 'yı kullanmak için aşağıdaki önkoşulların karşılanması gerekir:

- Doğrulama yöntemi olarak izin verilen anında iletme bildirimleri ile Azure Multi-Factor Authentication.
- İOS 8,0 veya üzerini çalıştıran cihazlarda yüklü Microsoft Authenticator en son sürümü veya Android 6,0 veya üzeri.

> [!NOTE]
> Azure AD PowerShell kullanarak daha az oturum açma Microsoft Authenticator uygulamayı etkinleştirdiyseniz, bu, tüm dizininiz için etkinleştirilmiştir. Bu yeni yöntemi kullanmayı etkinleştirirseniz, PowerShell ilkesinin yerini almaz. Yeni *kimlik doğrulama yöntemleri* menüsü aracılığıyla kiracınızdaki tüm kullanıcılar için etkinleştirmenizi öneririz, aksi halde yeni ilkede olmayan kullanıcılar artık parolasız oturum açamaz.

## <a name="enable-passwordless-authentication-methods"></a>Passwordless kimlik doğrulama yöntemlerini etkinleştir

Azure AD 'de passwordless kimlik doğrulamasını kullanmak için, önce Birleşik kayıt deneyimini etkinleştirin, ardından kullanıcıları parola daha az olan yönteme etkinleştirin.

### <a name="enable-the-combined-registration-experience"></a>Birleşik kayıt deneyimini etkinleştir

Passwordless kimlik doğrulama yöntemlerinin kayıt özellikleri, Birleşik kayıt özelliğini kullanır. Kullanıcıların birleştirilmiş kaydı tamamlamasını sağlamak için, [Birleşik güvenlik bilgileri kaydını etkinleştirmek](howto-registration-mfa-sspr-combined.md)için adımları izleyin.

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Passwordless telefon oturum açma kimlik doğrulama yöntemlerini etkinleştir

Azure AD, oturum açma işlemi sırasında hangi kimlik doğrulama yöntemlerinin kullanılabileceğini seçmenizi sağlar. Kullanıcılar, kullanmak istedikleri yöntemler için kayıt kaydeder.

Parolasız telefon oturumu açma kimlik doğrulama yöntemini etkinleştirmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) bir *genel yönetici* hesabıyla oturum açın.
1. *Azure Active Directory*arayıp seçin, sonra **güvenlik**  >  **kimlik doğrulama yöntemleri**  >  **kimlik doğrulama yöntemi ilkesi 'ne (Önizleme)** gidin
1. **Passwordless telefon oturumu açma**altında aşağıdaki seçenekleri belirleyin:
   1. **Etkinleştir** -Evet veya Hayır
   1. **Hedef** -tüm kullanıcılar veya kullanıcıları seçin
1. Yeni ilkeyi uygulamak için **Kaydet**' i seçin.

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasının Kullanıcı kaydı ve yönetimi

Azure AD 'de kullanılabilir olan passwordless kimlik doğrulama yöntemi sayesinde, kullanıcılar şimdi aşağıdaki adımları kullanarak kendi parolasız kimlik doğrulama yöntemine kaydolmalıdır:

1. [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) adresine göz atın.
1. Oturum açın, ardından **Yöntem ekle > Authenticator uygulaması**' nı ve ardından **Ekle**' yi seçerek Authenticator uygulamasını ekleyin.
1. Microsoft Authenticator uygulamasını cihazınıza yüklemek ve yapılandırmak için yönergeleri izleyin.
1. Doğrulayıcı yapılandırmasını tamamladıktan sonra **bitti** ' yi seçin.
1. **Microsoft Authenticator** uygulama ' da, kayıtlı hesabın açılan menüsünde **telefonla oturum açmayı etkinleştir** ' i seçin.
1. Parolasız telefon oturumu açma hesabı kaydını silmek için uygulamadaki yönergeleri izleyin.

Kuruluşlar, Microsoft Authenticator uygulamasını yapılandırma ve telefonla oturum açmayı etkinleştirme hakkında daha fazla yardım almak için [, kullanıcılarınızın parolasını değil telefonunuzla oturum açmasını](../user-help/user-help-auth-app-sign-in.md) sağlayabilir.

> [!NOTE]
> İlke tarafından telefonla oturum açma kullanımına izin verilmeyen kullanıcılar artık Microsoft Authenticator uygulamasında etkinleştirememektedir.  

## <a name="sign-in-with-passwordless-credential"></a>Passwordless kimlik bilgileriyle oturum açın

Bir yönetici kiracıyı etkinleştirdikten sonra Kullanıcı Microsoft Authenticator uygulamasını telefonla oturum açmayı etkinleştirmek üzere güncelleştirdikten sonra, Kullanıcı parolasız oturum açma özelliğini kullanmaya başlayabilir.

Telefonla oturum açma 'yı kullanmaya başlamak için, oturum açma sayfasında bir Kullanıcı adı yazdıktan ve **İleri**' yi seçerek, kullanıcıların **oturum açmak için başka yollar**seçmesi ve sonra **Microsoft Authenticator uygulamamda bir isteği onaylaması**gerekebilir. Daha sonra kullanıcılara bir sayı sunulur ve Microsoft Authenticator uygulamasında, parolasını kullanmak yerine kimlik doğrulaması yapmak için uygun numarayı seçmesi istenir. Kullanıcılar passwordless telefon oturum açma 'yı kullandıktan sonra, başka bir yöntem Seç ' i kullanana kadar bu uygulamayı yeniden kullanmaları istenir.

![Microsoft Authenticator uygulamasını kullanarak tarayıcı oturum açma örneği](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Bilinen Sorunlar

Geçerli önizleme deneyiminde aşağıdaki bilinen sorunlar mevcuttur.

### <a name="not-seeing-option-for-passwordless-phone-sign-in"></a>Parolasız telefon oturumu açma seçeneği görünmüyor

Bir Kullanıcı yanıtlanmayan parolasız telefon oturum açma doğrulaması bekliyor ve yeniden oturum açmayı denediğinde, Kullanıcı yalnızca bunun yerine parola girme seçeneği görebilir. Microsoft Authenticator açın ve tüm bildirim istemlerini kullanarak passwordless telefon oturum açma işlemine devam edin.

### <a name="federated-accounts"></a>Federasyon hesapları

Bir Kullanıcı herhangi bir passwordless kimlik bilgisini etkinleştirdiyse, Azure AD oturum açma işlemi, kullanıcıyı bir Federasyon oturum açma konumuna hızlandırmak için login_hint kullanmayı durdurur. Bu mantık, karma Kiracıdaki kullanıcıların, Kullanıcı yerine "parolanızı kullan" düğmesine tıklamadan oturum açma doğrulaması için AD FS yönlendirilmesine engel olur.

### <a name="azure-mfa-server"></a>Azure MFA sunucusu

Bir kuruluşun şirket içi Azure MFA sunucusu aracılığıyla MFA için etkinleştirilen son kullanıcılar, yine de tek bir parolasız telefon oturum açma kimlik bilgisi oluşturabilir ve kullanabilir. Kullanıcı, kimlik bilgileriyle Microsoft Authenticator birden çok yüklemeyi (5 +) yükseltmeyi denerse, bu değişiklik hataya neden olabilir.  

### <a name="device-registration"></a>Cihaz kaydı

Bu yeni güçlü kimlik bilgisini oluşturmak için önkoşullardan biri olan, Microsoft Authenticator uygulamasının yüklendiği cihazın tek bir kullanıcıya Azure AD kiracısı içinde de kaydedilmesi gerekir. Geçerli cihaz kayıt kısıtlamaları nedeniyle, bir cihaz yalnızca tek bir kiracıda kaydedilebilir. Bu sınır, telefonla oturum açma için Microsoft Authenticator uygulamasındaki yalnızca bir iş veya okul hesabının etkinleştiribileceği anlamına gelir.

> [!NOTE]
> Cihaz kaydı, cihaz yönetimi veya "MDM" ile aynı değildir. Yalnızca bir cihaz KIMLIĞINI ve kullanıcı KIMLIĞINI Azure AD dizininde bir araya ilişkilendirir.  

## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik doğrulaması ve parolasız yöntemler hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Passwordless kimlik doğrulamasının nasıl çalıştığını öğrenin](concept-authentication-passwordless.md)
* [Cihaz kaydı hakkında bilgi edinin](../devices/overview.md#getting-devices-in-azure-ad)
* [Azure Multi-Factor Authentication hakkında bilgi edinin](../authentication/howto-mfa-getstarted.md)
