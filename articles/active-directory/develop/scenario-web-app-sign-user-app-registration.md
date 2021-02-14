---
title: Kullanıcılara oturum açan bir Web uygulamasını kaydetme | Mavisi
titleSuffix: Microsoft identity platform
description: Kullanıcılara oturum açan bir Web uygulamasını nasıl kaydedeceğinizi öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 96fbe676d4b929d2c1ee2efce0d10f3b5dc7c6ab
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100103201"
---
# <a name="web-app-that-signs-in-users-app-registration"></a>Kullanıcılarda oturum açan Web uygulaması: uygulama kaydı

Bu makalede, kullanıcılarda oturum açan bir Web uygulaması için uygulama kaydı adımları açıklanmaktadır.

Uygulamanızı kaydetmek için şunu kullanabilirsiniz:

- [Web uygulaması hızlı başlangıç](#register-an-app-by-using-the-quickstarts). Uygulama oluşturmaya yönelik harika bir ilk deneyim olmasının yanı sıra Azure portal hızlı başlangıçlar, **Bu değişikliği bana yap** adlı bir düğme içerir. Bu düğmeyi, mevcut bir uygulama için bile ihtiyacınız olan özellikleri ayarlamak için kullanabilirsiniz. Bu özelliklerin değerlerini kendi çalışmanıza uyarlayabilirsiniz. Özellikle, uygulamanız için Web API URL 'SI muhtemelen önerilen varsayılan sunucudan farklı olacaktır, bu da oturum kapatma URI 'sini de etkileyecektir.
- [Uygulamanızı el ile kaydetmek](#register-an-app-by-using-the-azure-portal)için Azure Portal.
- PowerShell ve komut satırı araçları.

## <a name="register-an-app-by-using-the-quickstarts"></a>Hızlı başlangıçlarını kullanarak bir uygulamayı kaydetme

Web uygulamanızın oluşturulmasını önyüklemek için bu bağlantıları kullanabilirsiniz:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

## <a name="register-an-app-by-using-the-azure-portal"></a>Azure portal kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Kullanılacak Portal, uygulamanızın Microsoft Azure genel bulutta mi yoksa ulusal veya bağımsız bulutta mı çalışmasına bağlı olarak farklılık belirtir. Daha fazla bilgi için bkz. [Ulusal bulutlar](./authentication-national-cloud.md#app-registration-endpoints).


1. <a href="https://portal.azure.com/" target="_blank">Azure portalında</a> oturum açın. 
1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
1. **Azure Active Directory**'yi bulun ve seçin.
1. **Yönet** altında   >  **Yeni kayıt** uygulama kayıtları ' yi seçin.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
   1. Uygulamanız için bir **ad** girin (örneğin,) `AspNetCore-WebApp` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
   1. Uygulamanız için desteklenen hesap türlerini seçin. (Bkz. [Desteklenen hesap türleri](./v2-supported-account-types.md).)
   1. **Yeniden yönlendirme URI 'si** için, başarılı kimlik doğrulamasından sonra döndürülen belirteç yanıtlarını kabul edecek olan uygulama türünü ve URI hedefini ekleyin. Örneğin, `https://localhost:44321` girin.
   1. **Kaydet**’i seçin.
1. **Yönet** altında **kimlik doğrulaması** ' nı seçin ve ardından aşağıdaki bilgileri ekleyin:
   1. **Web** bölümünde, `https://localhost:44321/signin-oidc` **yeniden yönlendirme URI 'si** olarak ekleyin.
   1. **Ön kanal oturum kapatma URL 'si**' nde, girin `https://localhost:44321/signout-oidc` .
   1. **Örtük verme ve karma akışlar** altında **Kimlik belirteçleri**' ni seçin.
   1. **Kaydet**’i seçin.
   
# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. Uygulamanız için bir **ad** girin (örneğin,) `MailApp-openidconnect-v2` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
   1. Uygulamanız için desteklenen hesap türlerini seçin. (Bkz. [Desteklenen hesap türleri](./v2-supported-account-types.md).)
   1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, açılan kutuda **Web** ' i seçin ve BIR **yeniden yönlendirme URI 'si** girin `https://localhost:44326/` .
   1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. **Yönet** altında **kimlik doğrulaması**' nı seçin.
1. **Örtük verme ve karma akışlar** bölümünde **Kimlik belirteçleri**' ni seçin. Bu örnek, kullanıcının oturum açması için [örtük verme akışının](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirir.
1. **Kaydet**’i seçin.

# <a name="java"></a>[Java](#tab/java)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin: 
    1. Uygulamanız için bir **ad** girin (örneğin,) `java-webapp` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz. 
    1. **Herhangi bir kurumsal dizin ve kişisel Microsoft hesabında (örn. Skype, Xbox, Outlook.com) hesaplar ' ı** seçin.
    1. Uygulamayı kaydetmek için **Kaydet** ' i seçin.
1. **Yönet** altında **kimlik doğrulama**  >  **Platform ekleme**' yi seçin.
1. **Web**'i seçin.
1. **Yeniden yönlendirme URI 'si** için, aynı ana bilgisayar ve bağlantı noktası numarasını girin ve ardından `/msal4jsample/secure/aad` oturum açma sayfasına yazın. 
1. **Yapılandır**'ı seçin.
1. **Web** bölümünde, ana bilgisayar ve bağlantı noktası numarasını, ardından `/msal4jsample/graph/me` Kullanıcı bilgileri sayfası Için bir **yeniden yönlendirme URI 'si** olarak kullanın.
Varsayılan olarak, örnek şunları kullanır:
   - `http://localhost:8080/msal4jsample/secure/aad`
   - `http://localhost:8080/msal4jsample/graph/me`

1. **Kaydet**’i seçin.
1. **Yönet**’in altında **Sertifikalar ve gizli diziler**’i seçin.
1. **İstemci gizli** dizileri bölümünde **yeni istemci parolası**' nı seçin ve ardından:

   1. Bir anahtar açıklaması girin.
   1. **1 yılda** anahtar süresini seçin.
   1. **Add (Ekle)** seçeneğini belirleyin.
   1. Anahtar değeri göründüğünde, daha sonra için kopyalayın. Bu değer, bir daha görüntülenmez veya başka yollarla alınabilir.

# <a name="python"></a>[Python](#tab/python)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. Uygulamanız için bir **ad** girin (örneğin,) `python-webapp` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
   1. **Desteklenen hesap türlerini** **Tüm Kurumsal dizin ve kişisel Microsoft hesaplarında (ör. Skype, Xbox, Outlook.com) hesaplar** olarak değiştirin.
   1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, açılan kutuda **Web** ' i seçin ve aşağıdaki yeniden yönlendirme URI 'sini girin: `http://localhost:5000/getAToken` .
   1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamanın **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmak için gerekli olacaktır.
1. **Yönet**’in altında **Sertifikalar ve gizli diziler**’i seçin.
1. **Istemci gizli** dizileri bölümünde **yeni istemci parolası**' nı seçin ve ardından:
   1. Bir anahtar açıklaması girin.
   1. **1 yılda** bir anahtar süresi seçin.
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
> Kullanıcılar kendi kişisel Microsoft hesaplarıyla oturum açan bir uygulama (örneğin, Skype, Xbox veya Outlook.com) oluşturabilirsiniz. İlk olarak, çok kiracılı bir uygulama oluşturun. Desteklenen hesap türleri, herhangi bir kuruluş dizinindeki hesaplardır. Ardından, [`accessTokenAcceptedVersion`](./reference-app-manifest.md#accesstokenacceptedversion-attribute) özelliği **2** olarak ve [`signInAudience`](./reference-app-manifest.md#signinaudience-attribute) özelliğini `AzureADandPersonalMicrosoftAccount` Azure Portal [uygulama bildiriminde](./reference-app-manifest.md) olarak değiştirin. Daha fazla bilgi için ASP.NET Core öğreticisindeki [1,3. adıma](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) bakın. Bu adımı dilediğiniz dilde web uygulamalarına genelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryonun, [uygulamanın kod yapılandırması](scenario-web-app-sign-user-app-configuration.md)olan bir sonraki makaleye geçin.
