---
title: Azure AD Application Proxy ile uygulamalarda tek oturum açma | Microsoft Dokümanlar
description: Azure portalında Azure AD Application Proxy ile yayınlanan şirket içi uygulamalarınız için tek oturum açma'yı açın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025750"
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Application Proxy ile tek oturum açma için şifre atlama

Azure Active Directory Application Proxy, uzak çalışanların da bunlara güvenli bir şekilde erişebilmeleri için şirket içi uygulamalar yayınlayarak üretkenliği artırmanıza yardımcı olur. Azure portalında, bu uygulamalariçin tek oturum açma (SSO) de ayarlayabilirsiniz. Kullanıcılarınızın yalnızca Azure AD ile kimlik doğrulamaları gerekir ve yeniden oturum açmalarına gerek kalmadan kurumsal uygulamanıza erişebilirler.

Uygulama Proxy birkaç [tek oturum açma modlarını](what-is-single-sign-on.md#choosing-a-single-sign-on-method)destekler. Parola tabanlı oturum açma, kimlik doğrulaması için kullanıcı adı/parola birleşimi kullanan uygulamalar için tasarlanmıştır. Uygulamanız için parola tabanlı oturum açma yı yapılandırdığınızda, kullanıcılarınızın şirket içi uygulamada bir kez oturum açmaları gerekir. Bundan sonra Azure Active Directory oturum açma bilgilerini depolar ve kullanıcılarınız uzaktan eriştildiğinde uygulamaya otomatik olarak sağlar.

Uygulama Proxy ile uygulamanızı zaten yayımlamış ve test etmiş olmalısınız. Değilse, Azure AD [Application Proxy'yi kullanarak Uygulama Yayımla'daki](application-proxy-add-on-premises-application.md) adımları izleyin ve ardından buraya geri gelin.

## <a name="set-up-password-vaulting-for-your-application"></a>Başvurunuz için parola atlama ayarlama

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
1. **Azure Active Directory** > **Enterprise uygulamalarını** > seçin**Tüm uygulamalar.**
1. Listeden, SSO ile kurmak istediğiniz uygulamayı seçin.  
1. **Uygulama Proxy'yi**seçin. 
1. Ön **Kimlik Doğrulama türünü** Passthrough olarak **değiştirin** ve **Kaydet'i**seçin. Daha sonra Azure **Active Directory** türüne tekrar geçebilirsiniz! 
1. **Tek oturum açma'yı**seçin.

   ![Uygulamanın genel bakış sayfasından Tek oturum açma'yı seçin](./media/application-proxy-configure-single-sign-on-password-vaulting/select-sso.png)

1. SSO modu için **Parola tabanlı Oturum Açma'yı**seçin.
1. Oturum Açma URL'si için, kullanıcıların şirket ağı dışında uygulamanızda oturum açmaları için kullanıcı adlarını ve parolalarını girdikleri sayfanın URL'sini girin. Bu, uygulamayı Uygulama Proxy aracılığıyla yayınladığınızda oluşturduğunuz Harici URL olabilir.

   ![Parola tabanlı Oturum Açma'yı seçin ve URL'nizi girin](./media/application-proxy-configure-single-sign-on-password-vaulting/password-sso.png)

1. **Kaydet'i**seçin.
1. **Uygulama Proxy'yi**seçin. 
1. Ön **Kimlik Doğrulama türünü** **Azure Etkin Dizini** olarak değiştirin ve **Kaydet'i**seçin. 
1. **Kullanıcıları ve Grupları**seçin.
1. **Kullanıcı Ekle'yi**seçerek kullanıcıları uygulamaya atayın. 
1. Bir kullanıcı için kimlik bilgilerini önceden tanımlamak istiyorsanız, kullanıcı adının kutusunun ön ünü işaretleyin ve **kimlik bilgilerini güncelleştir'i**seçin.
1. **Azure Active Directory** > **App kayıtlarını** > seçin**Tüm uygulamalar**.
1. Listeden, Password SSO ile yapılandırdığınız uygulamayı seçin.
1. **Markaseçin.** 
1. Parola SSO sayfasından **URL'de Oturum Aç** ile Giriş sayfası nın **URL'sini** güncelleyin ve **Kaydet'i**seçin.  



<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Uygulamanızı test etme

Uygulamalarım portalına gidin. Kimlik bilgilerinizle (veya erişimle ayarladığınız bir test hesabının kimlik bilgileriyle) oturum açın. Başarılı bir şekilde oturum aştıktan sonra uygulamanın simgesine tıklayın. Bu, Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısının yüklenmesini tetikleyebilir. Kullanıcınızın önceden tanımlanmış kimlik bilgileri varsa, uygulamanın kimlik doğrulaması otomatik olarak gerçekleşmelidir, aksi takdirde ilk kez kullanıcı adını veya parolayı belirtmeniz gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Tek oturum açma](what-is-single-sign-on.md) yı uygulamanın diğer yolları hakkında bilgi edinin
- Azure [AD Application Proxy ile uygulamalara uzaktan erişmek için güvenlik le ilgili hususlar](application-proxy-security.md) hakkında bilgi edinin
