---
title: Microsoft Authenticator App (Önizleme) ile parolasız oturum açma-Azure Active Directory
description: Parolanızı (Genel Önizleme) kullanmadan Microsoft Authenticator uygulamasını kullanarak Azure AD 'de oturum açın
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.custom: seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c464874708c7b93ec5620cc9ae253912ce1a4790
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68357146"
---
# <a name="passwordless-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>Microsoft Authenticator uygulamayla passwordless telefon oturum açma (Genel Önizleme)

Microsoft Authenticator uygulaması, herhangi bir Azure AD hesabında parola kullanılmadan oturum açmak için kullanılabilir. [İş Için Windows Hello](/windows/security/identity-protection/hello-for-business/hello-identity-verification)teknolojisine benzer şekilde, Microsoft Authenticator bir cihaza bağlı olan ve biyometrik ya da PIN kullanan kullanıcı kimlik bilgilerini etkinleştirmek için anahtar tabanlı kimlik doğrulaması kullanır.

![Kullanıcının oturum açma onayını onaylamasını isteyen tarayıcı oturum açma örneği](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

Kullanıcı adı girdikten sonra parola istemi görmek yerine, Microsoft Authenticator uygulamada telefonla oturum açmayı etkinleştiren bir kişi, uygulamasının uygulamasındaki bir sayıya dokunmasını söyleyen bir ileti görür. Uygulamada, kullanıcının sayıyla eşleşmesi, Onayla ' yı seçmesi, sonra PIN veya biyometri sağlaması gerekir, bu durumda kimlik doğrulama tamamlanır.

## <a name="enable-my-users"></a>Kullanıcılarımı etkinleştir

### <a name="tenant-prerequisites"></a>Kiracı önkoşulları

* Azure Active Directory
* Azure Multi-Factor Authentication için etkinleştirilen son kullanıcılar
* Kullanıcılar cihazlarını kaydedebilir

### <a name="steps-to-enable"></a>Etkinleştirme adımları

Dizininizdeki parolasız kimlik doğrulama yöntemlerini etkinleştirmek için [Azure AD 'de parolasız oturum açma 'Yı etkinleştirme](howto-authentication-passwordless-enable.md#enable-new-passwordless-authentication-methods)makalesindeki adımları izleyin.

> [!NOTE]
> Daha önce kiracınız için bir PowerShell betiği kullanarak bu özelliği etkinleştirdiyseniz, kullanıcılar ve gruplar için yeni bir ilke ayarlandığında, mevcut kiracı genelindeki ilkenin üzerine yazılır. 
>

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>Son kullanıcılarım telefonla oturum açmayı nasıl etkinleştirsin?

Genel önizleme için, kullanıcıların bu yeni kimlik bilgisini oluşturmasına veya kullanmasına zorlayacağı bir yol yoktur. Son Kullanıcı, bir yönetici kiracısını etkinleştirdikten ve Kullanıcı Microsoft Authenticator uygulamasını telefonla oturum açmayı etkinleştirecek şekilde güncelleştirdikten sonra parolasız daha az oturum açma ile karşılaşacaktır.

> [!NOTE]
> Bu özellik, Mart 2017 ' den beri uygulamada vardı, bu nedenle ilke bir kiracı için etkinleştirildiğinde, kullanıcıların bu Flow anında karşılaşabileceği bir olasılık vardır. Kullanıcılarınıza bu değişikliği göz önünde bulundurun ve hazırlayın.
>

1. Azure Multi-Factor Authentication 'da kaydolma
1. İOS 8,0 veya üzerini çalıştıran cihazlarda yüklü Microsoft Authenticator en son sürümü veya Android 6,0 veya üzeri.
1. Uygulamaya anında iletme bildirimleri eklenmiş iş veya okul hesabı. Son Kullanıcı belgelerini adresinde [https://aka.ms/authappstart](https://aka.ms/authappstart)bulabilirsiniz.

Kullanıcı Microsoft Authenticator uygulamasında anında iletme bildirimleri ayarlanmış MFA hesabına sahip olduktan sonra, telefonla oturum açma kaydını tamamlamaya yönelik [parolandaki telefondan oturum açma](../user-help/microsoft-authenticator-app-phone-signin-faq.md) makalesindeki adımları izleyebilir.

## <a name="known-issues"></a>Bilinen Sorunlar

### <a name="ad-fs-integration"></a>AD FS tümleştirme

Kullanıcı Microsoft Authenticator parolasız kimlik bilgisini etkinleştirmişse, bu kullanıcı için kimlik doğrulaması her zaman onay için bir bildirim gönderir. Bu mantık, karma Kiracıdaki kullanıcıların, Kullanıcı yerine "parolanızı kullan" düğmesine tıklamadan oturum açma doğrulaması için ADFS 'ye yönlendirilmesini engeller. Bu işlem, şirket içi koşullu erişim ilkelerinin yanı sıra geçişli kimlik doğrulama akışlarını da atlar. Bu işlemin özel durumu bir login_hint belirtilmişse, bir kullanıcı AD FS olarak yeniden iletilir ve passwordless kimlik bilgisini kullanma seçeneğini atlar.

### <a name="azure-mfa-server"></a>Azure MFA sunucusu

Bir kuruluşun şirket içi Azure MFA sunucusu aracılığıyla MFA için etkinleştirilen son kullanıcılar, yine de tek bir parolasız telefon oturum açma kimlik bilgisi oluşturabilir ve kullanabilir. Kullanıcı, kimlik bilgileriyle Microsoft Authenticator birden çok yüklemeyi (5 +) yükseltmeyi denerse, bu değişiklik hataya neden olabilir.  

### <a name="device-registration"></a>Cihaz Kaydı

Bu yeni, güçlü kimlik bilgisini oluşturmak için önkoşullardan biri, bulunduğu cihazın Azure AD kiracısında tek bir kullanıcıya kayıtlı olması olabilir. Cihaz Kayıt kısıtlamaları nedeniyle, bir cihaz yalnızca tek bir kiracıda kaydedilebilir. Bu sınır, telefonla oturum açma için Microsoft Authenticator uygulamasındaki yalnızca bir iş veya okul hesabının etkinleştiribileceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Passwordless nedir?](concept-authentication-passwordless.md)

[Cihaz kaydı hakkında bilgi edinin](../devices/overview.md#getting-devices-in-azure-ad)

[Azure Multi-Factor Authentication hakkında bilgi edinin](../authentication/howto-mfa-getstarted.md)
