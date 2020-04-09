---
title: Kullanıcılarda işaretleyen bir web uygulaması kaydolun - Microsoft kimlik platformu | Azure
description: Kullanıcılarda işaretleyen bir web uygulamasını nasıl kaydedebilirsiniz öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 7d2eb5356b1abc54508fd6bf8d35fd9fc39d02ec
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881588"
---
# <a name="web-app-that-signs-in-users-app-registration"></a>Kullanıcılarda işaretleyen web uygulaması: Uygulama kaydı

Bu makalede, kullanıcılarda imzalayan bir web uygulamasının uygulama kaydı özellikleri açıklanmaktadır.

Başvurunuzu kaydetmek için şunları kullanabilirsiniz:

- [Web uygulaması hızlı başlar.](#register-an-app-by-using-the-quickstarts) Bir uygulama oluşturma konusunda harika bir ilk deneyim olmanın yanı sıra, Azure portalındaki hızlı başlangıçlar **benim için bu değişikliği yapın**adlı bir düğme içerir. Varolan bir uygulama için bile ihtiyacınız olan özellikleri ayarlamak için bu düğmeyi kullanabilirsiniz. Bu özelliklerin değerlerini kendi durumunuza uyarlamanız gerekir. Özellikle, uygulamanızın web API URL'si büyük olasılıkla önerilen varsayılandan farklı olacaktır ve bu da çıkış URI'yi de etkileyecektir.
- [Uygulamanızı el ile kaydetmek](#register-an-app-by-using-the-azure-portal)için Azure portalı.
- PowerShell ve komut satırı araçları.

## <a name="register-an-app-by-using-the-quickstarts"></a>Hızlı başlatmaları kullanarak bir uygulamayı kaydetme

Web uygulamanızın oluşturulmasını önyükleme için bu bağlantıları kullanabilirsiniz:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

## <a name="register-an-app-by-using-the-azure-portal"></a>Azure portalını kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Kullanılacak portal, uygulamanızın Microsoft Azure genel bulutu içinde mi yoksa ulusal veya bağımsız bir bulutta mı çalıştığına bağlı olarak farklıdır. Daha fazla bilgi için Ulusal [bulutlara](./authentication-national-cloud.md#app-registration-endpoints)bakın.


1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın. Alternatif olarak, ulusal bulut için tercih edilen Azure portalında oturum açın.
1. Hesabınız birden fazla kiracıya erişim sağlıyorsa, hesabınızı sağ üst köşeden seçin. Ardından, portal oturumunuzu istediğiniz Azure Etkin Dizin (Azure AD) kiracısına ayarlayın.
1. Sol bölmede, Azure **Etkin Dizin** hizmetini seçin ve ardından **Uygulama kayıtları** > **Yeni kaydı**nı seçin.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
   1. Uygulamanız için desteklenen hesap türlerini seçin. (Bkz. [Desteklenen hesap türleri](./v2-supported-account-types.md).)
   1. **Ad** bölümüne, uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin. Örneğin, **AspNetCore-WebApp**girin.
   1. **URI'yi Yeniden Yönlendirme**için, başarılı kimlik doğrulamadan sonra döndürülen belirteç yanıtlarını kabul edecek uygulama türünü ve URI hedefini ekleyin. Örneğin, girin. **https://localhost:44321** Ardından, **Kaydol'u**seçin.
1. Kimlik **Doğrulama** menüsünü seçin ve ardından aşağıdaki bilgileri ekleyin:
   1. **YanıtURL için,** türü **https://localhost:44321/signin-oidc** **Web**ekleyin.
   1. Gelişmiş **ayarlar** **bölümünde, Giriş URL'sini** ' ye **https://localhost:44321/signout-oidc**ayarla.
   1. **Örtük onay**'ın altında **Kimlik belirteçleri**'ni seçin.
   1. **Kaydet'i**seçin.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. Uygulamanız için desteklenen hesap türlerini seçin. (Bkz. [Desteklenen hesap türleri](./v2-supported-account-types.md).)
   1. **Ad** bölümüne, uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin. Örneğin, **MailApp-openidconnect-v2**girin.
   1. **URI'yi Yeniden Yönlendirme (isteğe bağlı)** bölümünde, açılan kutudaki **Web'i** seçin ve aşağıdaki yeniden yönlendirme URI'yi girin: **https://localhost:44326/**.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Kimlik **Doğrulama** menüsünü seçin.
1. Gelişmiş **ayarlar** | **Örtülü hibe** **bölümünde, kimlik belirteçleri**seçin. Bu örnek, [örtülü hibe akışının](v2-oauth2-implicit-grant-flow.md) kullanıcıda oturum açabilmesi için etkinleştirilmelerini gerektirir.
1. **Kaydet'i**seçin.

# <a name="java"></a>[Java](#tab/java)

1. Bir **uygulama sayfası kaydedin,** uygulama için bir görüntü adı girin. Örneğin, **java-webapp**girin.
1. **Herhangi bir kuruluş dizininde ve kişisel Microsoft Hesaplarında (örneğin Skype, Xbox, Outlook.com) Hesapları**seçin ve ardından Uygulama Türü için Web uygulamasını / **API'sini** seçin. **Application Type**
1. Uygulamayı kaydetmek için **Kaydol'u** seçin.
1. Sol menüde **Kimlik Doğrulama'yı**seçin. **Yönlendirme URL'leri**altında **Web'i**seçin.

1. İki yeniden yönlendirme URI'si girin: biri oturum açma sayfası için, diğeri de grafik sayfası için. Her ikisi için de aynı ana bilgisayar ve bağlantı noktası numarasını kullanın ve ardından oturum açma sayfası için **/msal4jsample/secure/aad** ve kullanıcı bilgileri sayfası için **msal4jsample/graph/me'yi** kullanın.

   Varsayılan olarak, örnek kullanır:

   - **http://localhost:8080/msal4jsample/secure/aad**
   - **http://localhost:8080/msal4jsample/graph/me**

  Ardından **Kaydet**’i seçin.

1. Menüden **Sertifikalar & sırları** seçin.
1. **İstemci sırları** bölümünde, **Yeni istemci sırrını**seçin ve sonra:

   1. Önemli bir açıklama girin.
   1. 1 yıl **içinde**anahtar süresini seçin.
   1. **Ekle'yi**seçin.
   1. Anahtar değeri göründüğünde, daha sonra için kopyalayın. Bu değer yeniden görüntülenmez veya başka yollarla alınamaz.

# <a name="python"></a>[Python](#tab/python)

1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. **Ad** bölümüne, uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin. Örneğin, **python-webapp**girin.
   1. **Desteklenen hesap türlerini** **herhangi bir kuruluş dizininde ve kişisel Microsoft hesaplarında (örneğin Skype, Xbox, Outlook.com) Hesaplarolarak**değiştirin.
   1. **URI'yi Yeniden Yönlendirme (isteğe bağlı)** bölümünde, açılan kutudaki **Web'i** seçin ve aşağıdaki yeniden yönlendirme URI'yi girin: **http://localhost:5000/getAToken**.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamanın Genel **Bakış** **sayfasında, Uygulama (istemci) kimlik** değerini bulun ve daha sonraya kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmak için ihtiyacınız olacak.
1. Sol menüde **Sertifikalar & sırları**seçin.
1. **İstemci Sırları** bölümünde, **Yeni istemci sırrını**seçin ve ardından:

   1. Önemli bir açıklama girin.
   1. **1 yıl içinde**önemli bir süre seçin.
   1. **Ekle'yi**seçin.
   1. Anahtar değeri göründüğünde, kopyalayın. Buna daha sonra ihtiyacınız olacak.
---

## <a name="register-an-app-by-using-powershell"></a>PowerShell'i kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Şu anda Azure AD PowerShell yalnızca aşağıdaki desteklenen hesap türlerine sahip uygulamalar oluşturur:
>
> - MyOrg (yalnızca bu kuruluş dizinindeki hesaplar)
> - AnyOrg (herhangi bir kuruluş dizinindeki hesaplar)
>
> Kullanıcılarda kişisel Microsoft hesaplarıyla (örneğin, Skype, Xbox veya Outlook.com) imzalayan bir uygulama oluşturabilirsiniz. İlk olarak, çok kiracılı bir uygulama oluşturun. Desteklenen hesap türleri, herhangi bir kuruluş dizinindeki hesaplardır. Ardından, uygulama `signInAudience` bildirimindeki özelliği Azure portalından değiştirin. Daha fazla bilgi için, ASP.NET Core öğreticisinde [adım 1.3'e](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) bakın. Bu adımı herhangi bir dilde web uygulamalarına genelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanın kod yapılandırması](scenario-web-app-sign-user-app-configuration.md)
