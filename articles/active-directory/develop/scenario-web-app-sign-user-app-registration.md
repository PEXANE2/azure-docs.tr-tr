---
title: Kullanıcılarda oturum açan Web uygulaması (uygulama kaydı)-Microsoft Identity platform
description: Kullanıcılara oturum açan bir Web uygulaması oluşturma hakkında bilgi edinin (uygulama kaydı)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0bdf04014d7b0382913c0a4094f7474686658441
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086695"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>Kullanıcılar-uygulama kaydı 'nda oturum açan Web uygulaması

Bu sayfada, kullanıcıları oturum açan bir Web uygulaması için uygulama kaydı özellikleri açıklanmaktadır.

Uygulamanızı kaydetmek için şunu kullanabilirsiniz:

- [Web uygulaması hızlı](#register-an-app-using-the-quickstarts) başlangıçlar-uygulama oluşturmaya yönelik harika bir ilk deneyim olmasının yanı sıra Azure Portal Hızlı başlangıçlar, **Bu değişikliği bana yap**adlı bir düğme içerir. Bu düğmeyi, mevcut bir uygulama için bile ihtiyacınız olan özellikleri ayarlamak için kullanabilirsiniz. Bu özelliklerin değerlerini kendi servis talebine uyarlamanız gerekir. Özellikle, uygulamanız için Web API URL 'SI, büyük olasılıkla önerilen varsayılan verilerden farklı olacak ve bu da oturum kapatma URI 'sini etkileyecek.
- [Uygulamanızı el ile kaydetmek](#register-an-app-using-azure-portal) için Azure Portal
- PowerShell ve komut satırı araçları

## <a name="register-an-app-using-the-quickstarts"></a>Hızlı başlangıçlarını kullanarak bir uygulamayı kaydetme

Bu bağlantıya gittiğinizde, Web uygulamanızın oluşturulmasını önyükleyebilirsiniz:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

### <a name="register-an-app-using-azure-portal"></a>Azure portal kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Uygulamanızın Microsoft Azure genel bulutta veya ulusal ya da bağımsız bulutunda çalışmasına bağlı olarak, kullanılacak Portal farklıdır. Daha fazla bilgi için bkz. [Ulusal bulutlar](./authentication-national-cloud.md#app-registration-endpoints)

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın. Alternatif olarak, tercih ettiğiniz Ulusal bulut Azure portal oturum açın.
1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst köşede hesabınızı seçin ve Portal oturumunuzu istenen Azure AD kiracısı olarak ayarlayın.
1. Sol taraftaki Gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve sonra **uygulama kayıtları** > **Yeni kayıt**' ı seçin.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. uygulamanız için desteklenen hesap türlerini seçin (bkz. [Desteklenen hesap türleri](./v2-supported-account-types.md))
   1. **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `AspNetCore-WebApp`.
   1. **Yeniden yönlendirme URI 'si**içinde, başarıyla kimlik doğrulamasından sonra döndürülen belirteç yanıtlarını kabul edecek olan uygulama türünü ve URI hedefini ekleyin. Örneğin: `https://localhost:44321/`.  **Kaydol**’u seçin.
1. **Kimlik doğrulama** menüsünü seçin ve ardından aşağıdaki bilgileri ekleyin:
   1. **Yanıt URL 'si**' nde `https://localhost:44321/signin-oidc`, ekleyin.
   1. **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL 'sini** olarak `https://localhost:44321/signout-oidc`ayarlayın.
   1. **Örtük izin**' ın altında, **kimlik belirteçlerini**denetleyin.
   1. **Kaydet**’i seçin.

### <a name="register-an-app-using-powershell"></a>PowerShell kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Şu anda Azure AD PowerShell yalnızca aşağıdaki desteklenen hesap türlerine sahip uygulamalar oluşturuyor:
>
> - MyOrg (yalnızca bu kuruluş dizinindeki hesaplar)
> - AnyOrg (herhangi bir kuruluş dizinindeki hesaplar).
>
> Kullanıcıları kendi kişisel Microsoft hesaplarıyla (örn. Skype, XBox, Outlook.com) oturum açan bir uygulama oluşturmak istiyorsanız, önce çok kiracılı bir uygulama (desteklenen hesap türleri = hesaplar herhangi bir kuruluş dizininde) oluşturabilir ve sonra değiştirebilirsiniz Azure Portal uygulama bildiriminde bulunan özellik.`signInAudience` Bu, ASP.NET Core öğreticinin [1,3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) adımındaki Ayrıntılar açıklanmaktadır (ve herhangi bir dilde web uygulamaları için genelleştirilerek bulunabilir).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanın kod yapılandırması](scenario-web-app-sign-user-app-configuration.md)
