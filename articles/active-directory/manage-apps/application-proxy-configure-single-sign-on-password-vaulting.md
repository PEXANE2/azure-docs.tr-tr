---
title: Azure AD uygulama ara sunucusu ile uygulamalara çoklu oturum açma | Microsoft Docs
description: Oturum açma için Azure portalında Azure AD uygulama ara sunucusu ile yayımlanan şirket içi uygulamalarınızı tek dönüştürün.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0259a8d9fcb4c9c513ab2c31103c9a8488e90ae
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025750"
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Uygulama proxy'si ile çoklu oturum açma için vaulting parola

Azure Active Directory Uygulama proxy'si uzak çalışanlar güvenli bir şekilde, çok erişebilmesi için şirket içi uygulamalar yayımlayarak üretkenliğini artırmaya yardımcı olur. Azure portalında, aynı zamanda çoklu oturum açma (SSO) bu uygulamalara ayarlayabilirsiniz. Kullanıcılarınız yalnızca Azure AD ile kimlik doğrulaması yapmanız ve yeniden oturum açmak zorunda kalmadan Kurumsal uygulamanızı erişebilir.

Uygulama proxy'si birkaç destekler [çoklu oturum açma modları](what-is-single-sign-on.md#choosing-a-single-sign-on-method). Parola tabanlı oturum açma kimlik doğrulaması için bir kullanıcı adı/parola birleşimini kullanan uygulamalar için tasarlanmıştır. Uygulamanız için parola tabanlı oturum açmayı yapılandırırken şirket içi uygulamaya bir kez oturum açmak, kullanıcılarınızın sahip. Bundan sonra Azure Active Directory oturum açma bilgilerini depolar ve kullanıcılarınızın, uzaktan erişim otomatik olarak uygulamaya sağlar.

Zaten varsa yayımlanan ve uygulamanızı uygulama ara sunucusu ile test. Aksi takdirde, adımları [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md) tekrar buraya gelin.

## <a name="set-up-password-vaulting-for-your-application"></a>Uygulamanız için vaulting parola ayarlama

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
1. Seçin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**.
1. SSO ile ayarlamak istediğiniz uygulamayı listeden seçin.  
1. Seçin **uygulama proxy'si**. 
1. **Ön kimlik doğrulaması türünü** **PASSTHROUGH** olarak değiştirin ve **Kaydet**' i seçin. Daha sonra yeniden **Azure Active Directory** türüne dönebilirsiniz! 
1. Seçin **çoklu oturum açma**.

   ![Uygulamanın genel bakış sayfasından çoklu oturum açma seçeneğini belirleyin](./media/application-proxy-configure-single-sign-on-password-vaulting/select-sso.png)

1. SSO modu için **parola tabanlı oturum açma**.
1. Oturum açma URL'si için burada kullanıcıların kullanıcı adı ve uygulamanızı şirket ağı dışından oturum açmak için parola girin, sayfası URL'sini girin. Bu, uygulama proxy'si aracılığıyla uygulama yayımladığınızda, oluşturduğunuz dış URL olabilir.

   ![Parola tabanlı oturum açma'yı seçin ve URL'nizi girin](./media/application-proxy-configure-single-sign-on-password-vaulting/password-sso.png)

1. **Kaydet**’i seçin.
1. Seçin **uygulama proxy'si**. 
1. **Ön kimlik doğrulama türünü** **Azure Active Directory** olarak değiştirin ve **Kaydet**' i seçin. 
1. **Kullanıcılar ve gruplar ' ı**seçin.
1. Kullanıcıları **Kullanıcı Ekle**seçeneğini belirleyerek uygulamaya atayın. 
1. Bir kullanıcı için kimlik bilgilerini önceden tanımlamak istiyorsanız, Kullanıcı adının önünü işaretleyin ve **kimlik bilgilerini güncelleştir**' i seçin.
1. **Tüm uygulamaları** > **uygulama kayıtları** **Azure Active Directory** > seçin.
1. Listeden parola SSO ile yapılandırdığınız uygulamayı seçin.
1. **Marka**seçin. 
1. **Giriş sayfası URL 'Sini** parola SSO sayfasında **oturum açma URL 'si** ile güncelleştirin ve **Kaydet**' i seçin.  



<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Uygulamanızı test etme

Uygulamalarım portalına gidin. Kimlik bilgilerinizle (veya erişim ile ayarladığınız bir test hesabının kimlik bilgileriyle) oturum açın. Başarıyla oturum açtıktan sonra uygulamanın simgesine tıklayın. Bu, uygulamalarımın güvenli oturum açma tarayıcı uzantısının yüklenmesini tetikleyebilir. Kullanıcılarınızın önceden tanımlanmış kimlik bilgileri varsa, uygulamanın kimlik doğrulaması otomatik olarak gerçekleşmelidir, aksi takdirde Kullanıcı adını veya parolayı ilk kez belirtmeniz gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

- Diğer yollarını uygulamak için okuma [çoklu oturum açma](what-is-single-sign-on.md)
- Hakkında bilgi edinin [uygulamaları Azure AD uygulama proxy'si ile uzaktan erişim için güvenlik konuları](application-proxy-security.md)
