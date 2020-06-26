---
title: Microsoft Authenticator App-Azure Active Directory ile passwordless oturum açma
description: Microsoft Authenticator uygulamasını kullanarak Azure AD 'de passwordless oturum açmayı etkinleştirme (Önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 100e4b88589f3731d127ccb1060e556c1f3a2f39
ms.sourcegitcommit: fdaad48994bdb9e35cdd445c31b4bac0dd006294
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85413255"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Microsoft Authenticator uygulamasıyla passwordless oturum açmayı etkinleştirme (Önizleme)

Microsoft Authenticator uygulaması, herhangi bir Azure AD hesabında parola kullanılmadan oturum açmak için kullanılabilir. [İş Için Windows Hello](/windows/security/identity-protection/hello-for-business/hello-identity-verification)teknolojisine benzer şekilde, Microsoft Authenticator bir cihaza bağlı olan ve biyometrik ya da PIN kullanan kullanıcı kimlik bilgilerini etkinleştirmek için anahtar tabanlı kimlik doğrulaması kullanır. Bu kimlik doğrulama yöntemi, mobil ve Microsoft kimlik doğrulama kitaplıklarıyla tümleştirilen herhangi bir uygulama veya Web sitesi ile birlikte herhangi bir cihaz platformunda kullanılabilir. 

![Kullanıcının oturum açma onayını onaylamasını isteyen tarayıcı oturum açma örneği](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Bir Kullanıcı adı girdikten sonra parola istemi görmek yerine, Microsoft Authenticator uygulamadan telefonla oturum açmayı etkinleştiren bir kişi, uygulamasının uygulamasındaki bir sayıya dokunmasını söyleyen bir ileti görür. Uygulamada, kullanıcının sayıyla eşleşmesi, Onayla ' yı seçmesi, sonra PIN veya biyometri sağlaması gerekir, bu durumda kimlik doğrulama tamamlanır.

> [!NOTE]
> Bu özellik Mart 2017 ' den beri Microsoft Authenticator uygulamada vardı, bu nedenle ilke bir dizin için etkinleştirildiğinde, kullanıcılar bu akışla hemen karşılaşabilir ve ilke tarafından etkinleştirilmeyen bir hata mesajı görebilir. Kullanıcılarınıza bu değişikliği göz önünde bulundurun ve hazırlayın.

## <a name="prerequisites"></a>Önkoşullar

- Doğrulama yöntemi olarak izin verilen anında iletme bildirimleri ile Azure Multi-Factor Authentication 
- İOS 8,0 veya üzerini çalıştıran cihazlarda yüklü Microsoft Authenticator en son sürümü veya Android 6,0 veya üzeri.

> [!NOTE]
> Azure AD PowerShell kullanarak önceki Microsoft Authenticator Apps 'ten daha az oturum açma önizlemesini etkinleştirdiyseniz, bu, tüm dizininiz için etkinleştirilmiştir. Bu yeni yöntemi kullanmayı etkinleştirirseniz, PowerShell ilkesinin yerini alacak. Yeni kimlik doğrulama yöntemleri aracılığıyla kiracınızdaki tüm kullanıcılar için etkinleştirmenizi öneririz, aksi takdirde yeni ilkede olmayan kullanıcılar artık parolasız olarak oturum açamaz. 

## <a name="enable-passwordless-authentication-methods"></a>Passwordless kimlik doğrulama yöntemlerini etkinleştir

### <a name="enable-the-combined-registration-experience"></a>Birleşik kayıt deneyimini etkinleştir

Passwordless kimlik doğrulama yöntemlerinin kayıt özellikleri, Birleşik kayıt özelliğini kullanır. Birleşik kayıt özelliğini etkinleştirmek için [Birleşik güvenlik bilgileri kaydını etkinleştirme](howto-registration-mfa-sspr-combined.md)makalesindeki adımları izleyin.

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Passwordless telefon oturum açma kimlik doğrulama yöntemlerini etkinleştir

1. [Azure Portal](https://portal.azure.com) oturum açın
1. *Azure Active Directory*'yi bulun ve seçin. **Güvenlik**  >  **kimlik doğrulama yöntemleri**  >  **kimlik doğrulama yöntemi ilkesi (Önizleme)** seçeneğini belirleyin
1. **Passwordless telefon oturumu açma**altında aşağıdaki seçenekleri belirleyin
   1. **Etkinleştir** -Evet veya Hayır
   1. **Hedef** -tüm kullanıcılar veya kullanıcıları seçin
1. Yeni ilkeyi ayarlamak için **Kaydet**

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasının Kullanıcı kaydı ve yönetimi

1. Buraya gidin[https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo)
1. Henüz yoksa oturum aç
1. **Yöntem Ekle**' ye tıklayıp **Authenticator uygulaması**' nı seçip **Ekle** ' ye tıklayarak bir kimlik doğrulayıcı uygulaması ekleyin
1. Microsoft Authenticator uygulamasını cihazınıza yüklemek ve yapılandırmak için yönergeleri izleyin
1. Authenticator MFA uygulama kurulum akışını tamamladıktan sonra **bitti** ' ye tıklayın. 
1. **Microsoft Authenticator**, hesap açılan menüsünden **Telefon oturum açmayı etkinleştir** ' i seçin.
1. Parolasız telefon oturumu açma için kayıt işleminin bitmesini yapmak üzere uygulamadaki yönergeleri izleyin. 

Kuruluşlar, Microsoft Authenticator uygulamasında kurulum ve telefonla oturum açmayı etkinleştirme hakkında daha fazla yardım almak için [parolanğuna değil,](../user-help/microsoft-authenticator-app-phone-signin-faq.md) Kullanıcı kullanıcılarınızı telefonunuza işaret edebilir. Bu ayarları uygulamak için oturumunuzu kapatıp kiracıya yeniden oturum açmanız gerekebilir. 

## <a name="sign-in-with-passwordless-credential"></a>Passwordless kimlik bilgileriyle oturum açın

Genel önizleme için, kullanıcıların bu yeni kimlik bilgisini oluşturmasına veya kullanmasına zorlayacağı bir yol yoktur. Bir yönetici **kiracıyı etkinleştirdikten** sonra Kullanıcı Microsoft Authenticator uygulamasını telefonla oturum açmayı etkinleştirmek üzere güncelleştirdikten sonra, Kullanıcı parolasız oturum açma ile karşılaşacaktır.

Web üzerinde Kullanıcı adınızı yazdıktan ve **İleri**' yi seçtikten sonra, kullanıcılara, parolasını kullanmak yerine kimlik doğrulaması yapmak için uygun numarayı seçmesi için bir sayı sunulur ve Microsoft Authenticator uygulamasında istenir. 

![Microsoft Authenticator uygulamasını kullanarak tarayıcı oturum açma örneği](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Bilinen Sorunlar

### <a name="user-is-not-enabled-by-policy-but-still-has-passwordless-phone-sign-in-method-in-microsoft-authenticator"></a>Kullanıcı, ilke tarafından etkinleştirilmemiş, ancak hala Microsoft Authenticator içinde passwordless telefon oturum açma yöntemi zaten içeriyor

Bir kullanıcının bir noktada geçerli Microsoft Authenticator uygulamasında veya önceki bir cihazda parolasız telefon oturum açma kimlik bilgisi oluşturmuş olması mümkündür. Yönetici, passwordless telefon oturumu için kimlik doğrulama yöntemi ilkesini etkinleştirdikten sonra, kayıtlı kimlik bilgileri olan herhangi bir Kullanıcı, ilkeyi kullanma özelliğinin etkinleştirilip etkinleştirilmediğini dikkate almaksızın, yeni oturum açma istemiyle karşılaşmaya başlar. Kullanıcının kimlik bilgilerini ilkeye göre kullanmasına izin verilmiyorsa, kimlik doğrulama akışını tamamladıktan sonra bir hata görür. 

Yönetici, kullanıcının passwordless telefon oturum açma özelliğini kullanmasını etkinleştirmeyi seçebilir veya kullanıcının yöntemi kaldırması gerekir. Kullanıcı artık kayıtlı cihaza sahip değilse, [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) Bu cihaz ona gidebilir ve kaldırabilir. Hala MFA için kimlik doğrulayıcı kullanılıyorsa, Microsoft Authenticator içinden **telefonla oturum açmayı devre dışı bırak** seçeneğini seçebilirler.  

### <a name="ad-fs-integration"></a>AD FS tümleştirme

Kullanıcı Microsoft Authenticator parolasız kimlik bilgisini etkinleştirmişse, bu kullanıcı için kimlik doğrulaması her zaman onay için bir bildirim gönderir. Bu mantık, karma Kiracıdaki kullanıcıların, Kullanıcı yerine "parolanızı kullan" düğmesine tıklamadan oturum açma doğrulaması için AD FS yönlendirilmesine engel olur. Bu işlem, şirket içi koşullu erişim ilkelerinin yanı sıra geçişli kimlik doğrulama akışlarını da atlar. 

Bir Kullanıcı yanıtlanmayan parolasız telefon oturum açma doğrulaması bekliyor ve yeniden oturum açmayı denediğinde, Kullanıcı AD FS bir parola girmesi gerekebilir.  

### <a name="azure-mfa-server"></a>Azure MFA sunucusu

Bir kuruluşun şirket içi Azure MFA sunucusu aracılığıyla MFA için etkinleştirilen son kullanıcılar, yine de tek bir parolasız telefon oturum açma kimlik bilgisi oluşturabilir ve kullanabilir. Kullanıcı, kimlik bilgileriyle Microsoft Authenticator birden çok yüklemeyi (5 +) yükseltmeyi denerse, bu değişiklik hataya neden olabilir.  

### <a name="device-registration"></a>Cihaz kaydı

Bu yeni güçlü kimlik bilgisini oluşturmak için önkoşullardan biri olan, Microsoft Authenticator uygulamasının yüklendiği cihazın tek bir kullanıcıya Azure AD kiracısı içinde de kaydedilmesi gerekir. Geçerli cihaz kayıt kısıtlamaları nedeniyle, bir cihaz yalnızca tek bir kiracıda kaydedilebilir. Bu sınır, telefonla oturum açma için Microsoft Authenticator uygulamasındaki yalnızca bir iş veya okul hesabının etkinleştiribileceği anlamına gelir.

### <a name="intune-mobile-application-management"></a>Intune mobil uygulama yönetimi  

Mobil uygulama yönetimi (MAM) gerektiren bir ilkeye bağlı olan son kullanıcılar, Microsoft Authenticator uygulamasına passwordless kimlik bilgisini kaydedemez. 

> [!NOTE]
> Cihaz kaydı, cihaz yönetimi veya "MDM" ile aynı değildir. Yalnızca bir cihaz KIMLIĞINI ve kullanıcı KIMLIĞINI Azure AD dizininde bir araya ilişkilendirir.  

## <a name="next-steps"></a>Sonraki adımlar

[Parolasız işlevi nedir?](concept-authentication-passwordless.md)

[Cihaz kaydı hakkında bilgi edinin](../devices/overview.md#getting-devices-in-azure-ad)

[Azure Multi-Factor Authentication hakkında bilgi edinin](../authentication/howto-mfa-getstarted.md)
