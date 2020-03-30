---
title: Microsoft Authenticator uygulamasıyla parolasız oturum açma - Azure Active Directory
description: Microsoft Authenticator uygulamasını kullanarak Azure AD'de parolasız oturum açmayı etkinleştirin (önizleme)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c684d6f1fbd8128ae020b6fd29da928b286aa18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126688"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Microsoft Authenticator uygulamasıyla parolasız oturum açmayı etkinleştirme (önizleme)

Microsoft Authenticator uygulaması, parola kullanmadan herhangi bir Azure REKLAM hesabında oturum açabilmek için kullanılabilir. [Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-identity-verification)teknolojisine benzer şekilde, Microsoft Authenticator bir aygıta bağlı olan ve biyometrik veya PIN kullanan bir kullanıcı kimlik bilgisini etkinleştirmek için anahtar tabanlı kimlik doğrulamasını kullanır. Bu kimlik doğrulama yöntemi, mobil cihazlar da dahil olmak üzere herhangi bir aygıt platformunda ve Microsoft kimlik doğrulama kitaplıklarıyla tümleşen herhangi bir uygulama veya web sitesinde kullanılabilir. 

![Kullanıcının oturum açma yı onaylamasını isteyen bir tarayıcı oturum açma örneği](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Bir kullanıcı adı girdikten sonra parola istemi görmek yerine, Microsoft Authenticator uygulamasından telefon oturum açma özelliğini etkinleştiren bir kişi, uygulamada bir numaraya dokunmalarını söyleyen bir ileti görür. Uygulamada, kullanıcı numarası eşleştirmek gerekir, Onayseçin, sonra pin veya biyometrik sağlamak, sonra kimlik doğrulama tamamlanır.

> [!NOTE]
> Bu özellik Mart 2017'den beri Microsoft Authenticator uygulamasında olduğundan, ilke bir dizin için etkinleştirildiğinde kullanıcıların bu akışla hemen karşılaşabilme ve ilke tarafından etkinleştirilmedikleri takdirde bir hata iletisi görme olasılığı vardır. Kullanıcılarınızı bu değişikliğe hazırlayın ve unutmayın.

## <a name="prerequisites"></a>Ön koşullar

- Doğrulama yöntemi olarak anında iletme bildirimlerine izin verilen Azure Çok Faktörlü Kimlik Doğrulama 
- iOS 8.0 veya daha büyük veya daha büyük olan cihazlarda veya Android 6.0 veya daha büyük cihazlarda yüklenmiş Microsoft Authenticator'Un en son sürümü.

> [!NOTE]
> Azure AD PowerShell'i kullanarak önceki Microsoft Authenticator uygulamasında parolasız oturum açma önizlemesini etkinleştirdiyseniz, tüm diziniçin etkinleştirildi. Bu yeni yöntemi kullanarak etkinleştirin, PowerShell ilkesinin yerini alacaktır. Yeni Kimlik Doğrulama Yöntemleri aracılığıyla kiracınızdaki tüm kullanıcılara etkinleştirmenizi öneririz, aksi takdirde yeni ilkedeki kullanıcılar artık parolasız oturum açamaz. 

## <a name="enable-passwordless-authentication-methods"></a>Parolasız kimlik doğrulama yöntemlerini etkinleştirme

### <a name="enable-the-combined-registration-experience"></a>Birleştirilmiş kayıt deneyimini etkinleştirme

Parolasız kimlik doğrulama yöntemleriiçin kayıt özellikleri, birleştirilmiş kayıt önizlemesini kullanır. Birleştirilmiş kayıt önizlemesini etkinleştirmek için [birleştirilmiş güvenlik bilgileri kaydını (önizleme) etkinleştir'](howto-registration-mfa-sspr-combined.md)makaledeki adımları izleyin.

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Parolasız telefon oturum açma kimlik doğrulama yöntemlerini etkinleştirme

1. [Azure portalında](https://portal.azure.com) oturum açın
1. *Azure Active Directory*'yi bulun ve seçin. **Güvenlik** > **Kimlik Doğrulama yöntemlerini** > seçin Kimlik doğrulama yöntemi ilkesi **(Önizleme)**
1. **Şifresiz telefon oturum açma**altında aşağıdaki seçenekleri seçin
   1. **Etkinleştir** - Evet veya Hayır
   1. **Hedef** - Tüm kullanıcılar veya Select kullanıcıları
1. Yeni ilkeyi ayarlamak için **kaydet**

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasının kullanıcı kaydı ve yönetimi

1. Göz atın[https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo)
1. Zaten değilse oturum açın
1. Kimlik doğrulayıcı uygulaması ekle **yöntemini**tıklatarak, **Authenticator uygulamasını**seçerek ve **Ekle'yi** tıklatarak bir kimlik doğrulayıcı uygulaması ekleme
1. Microsoft Authenticator uygulamasını cihazınıza yüklemek ve yapılandırmak için yönergeleri izleyin
1. Authenticator MFA uygulama kurulum akışını tamamlamak için **Bitti'yi** tıklatın. 
1. **Microsoft Authenticator'da,** hesap açılır menüsünden **telefon oturum açma yı etkinleştir'i** seçin
1. Şifresiz telefon oturum açma için kaydolmayı bitirmek için uygulamadaki yönergeleri izleyin. 

Kuruluşlar, Microsoft Authenticator uygulamasında daha fazla yardım sağlamak ve telefon oturum açmanızı etkinleştirmek için [kullanıcılarını telefonunuzla oturum aç'ın](../user-help/microsoft-authenticator-app-phone-signin-faq.md) ayarına yönlendirebilir. Bu ayarları uygulamak için oturumu niçin oturumu açmanız ve kiracıya yeniden giriş yapmanız gerekebilir. 

## <a name="sign-in-with-passwordless-credential"></a>Parolasız kimlik bilgisi ile oturum açma

Genel önizleme için, kullanıcıları bu yeni kimlik belgesini oluşturmaya veya kullanmaya zorlamanın bir yolu yoktur. Bir kullanıcı yalnızca bir yönetici kiracısını etkinleştirdikten **ve** kullanıcı telefon oturum açmayı etkinleştirmek için Microsoft Authenticator uygulamasını güncelleştirince parolasız oturum açma ile karşılaşır.

Web'de kullanıcı adınızı yazdıktan ve **İleri'yi**seçtikten sonra, kullanıcılara bir numara sunulur ve Microsoft Authenticator uygulamasında parolalarını kullanmak yerine kimlik doğrulaması yapmak için uygun numarayı seçmeleri istenir. 

![Microsoft Authenticator uygulamasını kullanarak tarayıcı oturum açma örneği](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Bilinen Sorunlar

### <a name="user-is-not-enabled-by-policy-but-still-has-passwordless-phone-sign-in-method-in-microsoft-authenticator"></a>Kullanıcı ilke tarafından etkinleştirilemez, ancak Microsoft Authenticator'da yine de şifresiz telefon oturum açma yöntemi vardır

Bir kullanıcının bir noktada geçerli Microsoft Authenticator uygulamasında veya daha önceki bir aygıtta parolasız bir telefon oturum açma kimlik bilgisi oluşturmuş olması mümkündür. Bir yönetici, parolasız telefon oturum açma için kimlik doğrulama yöntemi ilkesini etkinleştirdikten sonra, kimlik bilgisi kayıtlı herhangi bir kullanıcı, ilkeyi kullanmalarına izin verilip etkinleştirilemediğine bakılmaksızın yeni oturum açma istemini deneyimlemeye başlar. Kullanıcının kimlik bilgilerini ilke yle kullanmasına izin verilmediyse, kimlik doğrulama akışını tamamladıktan sonra bir hata görür. 

Yönetici, kullanıcının şifresiz telefon oturum açma yı kullanmasını etkinleştirmeyi seçebilir veya kullanıcının yöntemi kaldırması gerekir. Kullanıcı artık kayıtlı aygıtı kullanmazsa, aygıtın [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) içine gidip kaldırabilir. MFA için Kimlik Doğrulayıcı'yı kullanmaya devam ediyorlarsa, Microsoft Authenticator'un içinden **telefon oturum açmayı devre dışı bırakmayı** seçebilirler.  

### <a name="ad-fs-integration"></a>AD FS entegrasyonu

Bir kullanıcı Microsoft Authenticator parolasız kimlik bilgilerini etkinleştirdiğinde, bu kullanıcı için kimlik doğrulaması her zaman onay için bildirim göndermekten varsayılan olarak kabul edilir. Bu mantık, hibrit kiracıya giren kullanıcıların oturum açma doğrulaması için ADFS'ye yönlendirilen kullanıcıları, kullanıcının "Parolanızı bunun yerine kullanın" düğmesini tıklatmak için ek bir adım atmadan engellemez. Bu işlem ayrıca şirket içinde koşullu erişim ilkelerini ve geçiş kimlik doğrulama akışlarını da atlar. 

Bir kullanıcının yanıtsız bir şifresiz telefon oturum açma doğrulaması varsa ve yeniden oturum açmaya çalışırsa, kullanıcı parola girmek için ADFS'ye götürülebilir.  

### <a name="azure-mfa-server"></a>Azure MFA sunucusu

Bir kuruluşun şirket içi Azure MFA sunucusu aracılığıyla MFA için etkinleştirilen son kullanıcılar, kimlik bilgisi içinde tek bir parolasız telefon işareti oluşturabilir ve kullanabilir. Kullanıcı Microsoft Authenticator'ın birden çok yüklemesini (5+) kimlik bilgisiyle yükseltmeye çalışırsa, bu değişiklik bir hataya neden olabilir.  

### <a name="device-registration"></a>Cihaz kaydı

Bu yeni güçlü kimlik bilgilerini oluşturmanın ön koşullarından biri, Microsoft Authenticator uygulamasının yüklü olduğu aygıtın azure AD kiracısına tek bir kullanıcıya da kaydedilmesi gerektiğidir. Geçerli aygıt kaydı kısıtlamaları nedeniyle, bir aygıt yalnızca tek bir kiracıya kaydedilebilir. Bu sınır, telefon oturum açma için Microsoft Authenticator uygulamasında yalnızca bir iş veya okul hesabının etkinleştirilebileceği anlamına gelir.

### <a name="intune-mobile-application-management"></a>Intune mobil uygulama yönetimi  

Mobil uygulama yönetimi (MAM) gerektiren bir ilkeye tabi olan son kullanıcılar, Microsoft Kimlik Doğrulayıcı uygulamasına parolasız kimlik belgesini kaydedemez. 

> [!NOTE]
> Cihaz kaydı, aygıt yönetimi veya "MDM" ile aynı değildir. Yalnızca Azure AD dizininde bir aygıt kimliği ve kullanıcı kimliğini ilişkilendirer.  

## <a name="next-steps"></a>Sonraki adımlar

[Parolasız işlevi nedir?](concept-authentication-passwordless.md)

[Cihaz kaydı hakkında bilgi edinin](../devices/overview.md#getting-devices-in-azure-ad)

[Azure Çok Faktörlü Kimlik Doğrulama hakkında bilgi edinin](../authentication/howto-mfa-getstarted.md)
