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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 50e4778693e448b9a989d70d3ca8cf91c76ce380
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74482053"
---
# <a name="web-app-that-signs-in-users-app-registration"></a>Kullanıcılarda oturum açan Web uygulaması: uygulama kaydı

Bu makalede, kullanıcılarda oturum açan bir Web uygulaması için uygulama kaydı özellikleri açıklanmaktadır.

Uygulamanızı kaydetmek için şunu kullanabilirsiniz:

- [Web uygulaması hızlı başlangıç](#register-an-app-by-using-the-quickstarts). Uygulama oluşturmaya yönelik harika bir ilk deneyim olmasının yanı sıra Azure portal hızlı başlangıçlar, **Bu değişikliği bana yap**adlı bir düğme içerir. Bu düğmeyi, mevcut bir uygulama için bile ihtiyacınız olan özellikleri ayarlamak için kullanabilirsiniz. Bu özelliklerin değerlerini kendi servis talebine uyarlamanız gerekir. Özellikle, uygulamanız için Web API URL 'SI muhtemelen önerilen varsayılan sunucudan farklı olacaktır, bu da oturum kapatma URI 'sini de etkileyecektir.
- [Uygulamanızı el ile kaydetmek](#register-an-app-by-using-the-azure-portal)için Azure Portal.
- PowerShell ve komut satırı araçları.

## <a name="register-an-app-by-using-the-quickstarts"></a>Hızlı başlangıçlarını kullanarak bir uygulamayı kaydetme

Web uygulamanızın oluşturulmasını önyüklemek için bu bağlantıları kullanabilirsiniz:

- [ASP.NET Çekirdeği](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

## <a name="register-an-app-by-using-the-azure-portal"></a>Azure portal kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Kullanılacak Portal, uygulamanızın Microsoft Azure genel bulutta mi yoksa ulusal veya bağımsız bulutta mı çalışmasına bağlı olarak farklılık belirtir. Daha fazla bilgi için bkz. [Ulusal bulutlar](./authentication-national-cloud.md#app-registration-endpoints).


1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın. Alternatif olarak, Ulusal bulut için tercih edilen Azure portal oturum açın.
1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst köşedeki hesabınızı seçin. Ardından, Portal oturumunuzu istenen Azure Active Directory (Azure AD) kiracısına ayarlayın.
1. Sol bölmede **Azure Active Directory** hizmetini seçin ve sonra **yeni kayıt** > **uygulama kayıtları** seçin.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Çekirdeği](#tab/aspnetcore)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. Uygulamanız için desteklenen hesap türlerini seçin. (Bkz. [Desteklenen hesap türleri](./v2-supported-account-types.md).)
   1. **Ad** bölümünde, uygulamanın kullanıcılarına gösterilecek anlamlı bir uygulama adı girin. Örneğin, **Aspnetcore-WebApp**yazın.
   1. **Yeniden yönlendirme URI 'si**için, başarılı kimlik doğrulamasından sonra döndürülen belirteç yanıtlarını kabul edecek olan uygulama türünü ve URI hedefini ekleyin. Örneğin **https://localhost:44321** girin. Sonra **Kaydet**' i seçin.
1. **Kimlik doğrulama** menüsünü seçin ve ardından aşağıdaki bilgileri ekleyin:
   1. **Yanıt URL 'si**için **Web**türünde **https://localhost:44321/signin-oidc** ekleyin.
   1. **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL 'sini** **https://localhost:44321/signout-oidc** olarak ayarlayın.
   1. **Örtük izin**' ın altında **Kimlik belirteçleri**' ni seçin.
   1. **Kaydet**’i seçin.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. Uygulamanız için desteklenen hesap türlerini seçin. (Bkz. [Desteklenen hesap türleri](./v2-supported-account-types.md).)
   1. **Ad** bölümünde, uygulamanın kullanıcılarına gösterilecek anlamlı bir uygulama adı girin. Örneğin, **Mailapp-openıdconnect-v2**yazın.
   1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, açılan kutuda **Web** ' i seçin ve aşağıdaki yeniden yönlendirme urı 'sini girin: **https://localhost:44326/** .
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. **Kimlik doğrulama** menüsünü seçin. 
1. **Gelişmiş ayarlar** | **örtük verme** bölümünde **Kimlik belirteçleri**' ni seçin. Bu örnek, kullanıcının oturum açması için [örtük verme akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir.
1. **Kaydet**’i seçin.

# <a name="javatabjava"></a>[Java](#tab/java)

1. **Bir uygulamayı Kaydet sayfası** göründüğünde, uygulama için bir görünen ad girin. Örneğin, **Java-WebApp**yazın. 
1. **Herhangi bir kurumsal dizin ve kişisel Microsoft hesabında (örn. Skype, Xbox, Outlook.com) hesaplar**' ı seçin ve ardından **uygulama türü**IÇIN **Web uygulaması/API** ' yi seçin.
1. Uygulamayı kaydetmek için **Kaydet** ' i seçin.
1. Sol taraftaki menüden **kimlik doğrulaması**' nı seçin. **Yeniden yönlendirme URI 'leri**altında **Web**' i seçin. 

1. İki yeniden yönlendirme URI 'si girin: bir oturum açma sayfası ve bir grafik sayfası için bir tane. Her iki için de, oturum açma sayfası ve Kullanıcı bilgileri sayfası için **msal4jsample/Graph/Me** tarafından izlenen aynı ana bilgisayar ve bağlantı noktası numarasını ve ardından **/msal4jsample/Secure/AAD** ' yi kullanın.
 
   Varsayılan olarak, örnek şunları kullanır:

   - **http://localhost:8080/msal4jsample/secure/aad**
   - **http://localhost:8080/msal4jsample/graph/me**

1. **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL 'sini** **http://localhost:8080/msal4jsample/sign_out** olarak ayarlayın. Ardından **Kaydet**’i seçin.

1. Menüden **sertifikalar & parolaları** ' nı seçin. 
1. **İstemci gizli** dizileri bölümünde **yeni istemci parolası**' nı seçin ve ardından:

   1. Bir anahtar açıklaması girin.
   1. **1 yılda**anahtar süresini seçin.
   1. **Add (Ekle)** seçeneğini belirleyin.
   1. Anahtar değeri göründüğünde, daha sonra için kopyalayın. Bu değer, bir daha görüntülenmez veya başka yollarla alınabilir.

# <a name="pythontabpython"></a>[Python](#tab/python)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. **Ad** bölümünde, uygulamanın kullanıcılarına gösterilecek anlamlı bir uygulama adı girin. Örneğin, **Python-WebApp**yazın.
   1. **Desteklenen hesap türlerini** **Tüm Kurumsal dizin ve kişisel Microsoft hesaplarında (ör. Skype, Xbox, Outlook.com) hesaplar**olarak değiştirin.
   1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, açılan kutuda **Web** ' i seçin ve aşağıdaki yeniden yönlendirme urı 'sini girin: **http://localhost:5000/getAToken** .
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamanın **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmak için gerekli olacaktır.
1. **Kimlik doğrulama** bölümünü seçin. **Gelişmiş ayarlar**' da, **oturum kapatma URL 'sini** **http://localhost:5000/logout** olarak ayarlayın. Ardından **Kaydet**’i seçin.
1. Sol taraftaki menüden **sertifikalar & gizlilikler**' ı seçin.
1. **Istemci gizli** dizileri bölümünde **yeni istemci parolası**' nı seçin ve ardından:

   1. Bir anahtar açıklaması girin.
   1. **1 yılda**bir anahtar süresi seçin.
   1. **Add (Ekle)** seçeneğini belirleyin.
   1. Anahtar değeri göründüğünde kopyalayın. Buna daha sonra ihtiyacınız olacak.
---

## <a name="register-an-app-by-using-powershell"></a>PowerShell kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Şu anda Azure AD PowerShell yalnızca aşağıdaki desteklenen hesap türleriyle uygulamalar oluşturuyor:
>
> - MyOrg (yalnızca bu kuruluş dizinindeki hesaplar)
> - AnyOrg (herhangi bir kuruluş dizinindeki hesaplar)
>
> Kullanıcılar kendi kişisel Microsoft hesaplarıyla oturum açan bir uygulama (örneğin, Skype, Xbox veya Outlook.com) oluşturabilirsiniz. İlk olarak, çok kiracılı bir uygulama oluşturun. Desteklenen hesap türleri, herhangi bir kuruluş dizinindeki hesaplardır. Sonra, Azure portal uygulama bildiriminde `signInAudience` özelliğini değiştirin. Daha fazla bilgi için ASP.NET Core öğreticisindeki [1,3. adıma](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) bakın. Bu adımı dilediğiniz dilde web uygulamalarına genelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanın kod yapılandırması](scenario-web-app-sign-user-app-configuration.md)
