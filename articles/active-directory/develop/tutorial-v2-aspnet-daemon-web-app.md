---
title: Microsoft kimlik platformu bitiş noktasını kullanan çok kiracılı bir daemon oluşturun
description: Bu eğitimde, Azure Active Directory tarafından korunan bir ASP.NET web API'sini bir Windows masaüstü (WPF) uygulamasından nasıl çağırabilirsiniz öğrenin. WPF istemcisi bir kullanıcının kimliğini doğrular, erişim jetonunu ister ve web API'sını çağırır.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 12/10/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.openlocfilehash: 0fb80b8a3fe9dd642b1574b35ff48b30272ce848
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81533726"
---
# <a name="tutorial-build-a-multitenant-daemon-that-uses-the-microsoft-identity-platform-endpoint"></a>Öğretici: Microsoft kimlik platformu bitiş noktasını kullanan çok kiracılı bir daemon oluşturun

Bu eğitimde, uzun süren, etkileşimli olmayan bir işlemle Microsoft iş müşterilerinin verilerine erişmek için Microsoft kimlik platformunun nasıl kullanılacağını öğrenirsiniz. Örnek daemon bir erişim belirteci elde etmek için [OAuth2 istemci kimlik bilgileri hibe](v2-oauth2-client-creds-grant-flow.md) kullanır. Daemon daha sonra [Microsoft Graph'ı](https://graph.microsoft.io) aramak ve kuruluş verilerine erişmek için belirteci kullanır.

> [!div class="checklist"]
> * Daemon uygulamasını Microsoft kimlik platformuyla tümleştirme
> * Uygulama izinlerini doğrudan bir yönetici tarafından uygulamaya verme
> * Microsoft Graph API'yi aramak için bir erişim belirteci alın
> * Microsoft Graph API'yi arayın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

Uygulama ASP.NET mvc uygulaması olarak oluşturulmuş. Kullanıcıları oturum açmak için OWIN OpenID Connect aracını kullanır.

Bu örnekteki "daemon" bileşeni bir API denetleyicisi, `SyncController.cs`. Denetleyici çağrıldığında, müşterinin Azure Etkin Dizini (Azure AD) kiracısında microsoft graph'tan kullanıcıların listesini çeker. `SyncController.cs`web uygulamasında bir AJAX çağrısı tarafından tetiklenir. Microsoft Graph için bir erişim belirteci elde etmek [için .NET için Microsoft Kimlik Doğrulama Kitaplığı 'nı (MSAL)](msal-overview.md) kullanır.

>[!NOTE]
> Microsoft kimlik platformunda yeniyseniz, [.NET Core daemon quickstart](quickstart-v2-netcore-daemon.md)ile başlamanızı öneririz.

## <a name="scenario"></a>Senaryo

Uygulama Microsoft kurumsal müşterileri için çok kiracılı bir uygulama olduğundan, müşterilerin uygulamayı şirket verilerine "kaydolmaları" veya "bağlamaları" için bir yol sağlaması gerekir. Bağlantı akışı sırasında, bir şirket yöneticisi uygulama *izinlerini* ilk olarak doğrudan uygulamaya verir, böylece şirket verilerine etkileşimli olmayan bir şekilde, oturum açmış bir kullanıcı olmadan erişebilir. Bu örnekteki mantığın çoğu, kimlik platformunun [yönetici onay](v2-permissions-and-consent.md#using-the-admin-consent-endpoint) bitiş noktasını kullanarak bu bağlantı akışınınasıl elde edeceklerini gösterir.

![Topoloji](./media/tutorial-v2-aspnet-daemon-webapp/topology.png)

Bu örnekte kullanılan kavramlar hakkında daha fazla bilgi için, [kimlik platformu bitiş noktası için istemci kimlik bilgileri iletişim kuralı belgelerini](v2-oauth2-client-creds-grant-flow.md)okuyun.

## <a name="prerequisites"></a>Ön koşullar

Örneği bu hızlı başlatmada çalıştırmak için şunları yapmanız gerekir:

- [Visual Studio 2017 veya 2019](https://visualstudio.microsoft.com/downloads/).
- Azure AD kiracısı. Daha fazla bilgi için Azure [AD kiracısını nasıl edinirdiye](quickstart-create-new-tenant.md)bakın.
- Azure AD kiracınızda bir veya daha fazla kullanıcı hesabı. Bu örnek bir Microsoft hesabıyla (eski adıyla Windows Live hesabı) çalışmaz. [Azure portalında](https://portal.azure.com) bir Microsoft hesabıyla oturum kurduysanız ve dizininizde hiç kullanıcı hesabı oluşturmadıysanız, bunu şimdi yapmanız gerekir.

## <a name="clone-or-download-this-repository"></a>Bu depoya kopya veya indirin

Kabuk veya komut satırınızdan şu komutu girin:

```Shell
git clone https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2.git
```

Veya [bir zip dosyasında örnek indirin.](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/archive/master.zip)

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Bu örnekte bir proje var. Uygulamayı Azure AD kiracınıza kaydetmek için şunları yapabilirsiniz:

- Örneği Azure [Etkin Dizin kiracınıza kaydettirme](#register-your-application) adımlarını izleyin ve [Azure AD kiracınızı kullanmak için örneği yapılandırın.](#choose-the-azure-ad-tenant)
- PowerShell komut dosyalarını kullanın:
  - Azure AD uygulamalarını ve ilgili nesneleri (parolalar, izinler, bağımlılıklar) sizin için *otomatik olarak* oluşturun.
  - Visual Studio projelerinin yapılandırma dosyalarını değiştirin.

Otomasyonu kullanmak istiyorsanız:

1. Windows'da PowerShell'i çalıştırın ve klonlanan dizinin köküne gidin.
1. Şu komutu çalıştırın:

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
   ```

1. Azure AD uygulamanızı oluşturmak ve örnek uygulamanın kodunu buna göre yapılandırmak için komut dosyasını çalıştırın:

   ```PowerShell
   .\AppCreationScripts\Configure.ps1
   ```

   Komut dosyalarını çalıştırmanın diğer yolları [Uygulama oluşturma komut dosyalarında](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/blob/master/AppCreationScripts/AppCreationScripts.md)açıklanmıştır.

1. Visual Studio çözümünü açın ve kodu çalıştırmak için **Başlat'ı** seçin.

Otomasyonu kullanmak istemiyorsanız, aşağıdaki bölümlerdeki adımları kullanın.

### <a name="choose-the-azure-ad-tenant"></a>Azure AD kiracısını seçin

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısındaysa, sayfanın üst kısmındaki menüdeki profilinizi seçin ve ardından **Dizin Değiştir'i**seçin.
1. Portal oturumunuzu istediğiniz Azure AD kiracısına değiştirin.

### <a name="register-the-client-app-dotnet-web-daemon-v2"></a>İstemci uygulamasını kaydedin (dotnet-web-daemon-v2)

1. Geliştiriciler için Microsoft kimlik platformundaki [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Yeni kayıt**seçin.
1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
   - **Ad** bölümüne, uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin. Örneğin, **dotnet-web-daemon-v2**girin.
   - Desteklenen **hesap türleri** bölümünde, **herhangi bir kuruluş dizininde Hesaplar'ı**seçin.
   - **URI'yi Yeniden Yönlendirme (isteğe bağlı)** bölümünde, açılan kutudaki **Web'i** seçin ve aşağıdaki yönlendirme URI'lerini girin:
       - **https://localhost:44316/**
       - **https://localhost:44316/Account/GrantPermissions**

     İkiden fazla yeniden yönlendirme ÜR'leri varsa, uygulama başarıyla oluşturulduktan sonra bunları daha sonra **Kimlik Doğrulama** sekmesinden eklemeniz gerekir.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamanın Genel **Bakış** **sayfasında, Uygulama (istemci) kimlik** değerini bulun ve daha sonraya kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmak için ihtiyacınız olacak.
1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin. Ardından:
   - Gelişmiş **ayarlar** **bölümünde, Giriş URL'sini** ' ye **https://localhost:44316/Account/EndSession**ayarla.
   - Gelişmiş **ayarlar** > **Örtülü hibe** **bölümünde, Erişim belirteçleri** ve **kimlik belirteçleri'ni**seçin. Bu örnek, [örtük hibe akışının](v2-oauth2-implicit-grant-flow.md) kullanıcıda oturum açmave API'yi çağırmasının etkinleştirilmesini gerektirir.
1. **Kaydet**’i seçin.
1. **Sertifikalar & sırları** sayfasından, **İstemci sırları** bölümünde Yeni istemci **sırrını**seçin. Ardından:

   1. Önemli bir açıklama girin (örneğin, **uygulama sırrı**),
   1. Ya 1 yıl **içinde**anahtar bir süre seçin , **2 yıl içinde**, ya da **Asla Sona Erer**.
   1. **Ekle** düğmesini seçin.
   1. Anahtar değeri göründüğünde, kopyalayın ve güvenli bir konuma kaydedin. Projeyi Visual Studio'da yapılandırmak için daha sonra bu anahtara ihtiyacınız olacak. Bir daha görüntülenmeyecek veya başka yollarla alınamaz.
1. Uygulamanın sayfaları listesinde **API izinlerini**seçin. Ardından:
   1. **İzin ekleyin** düğmesini seçin.
   1. **Microsoft API'leri** sekmesinin seçildiğinden emin olun.
   1. Yaygın **olarak kullanılan Microsoft API'leri** bölümünde **Microsoft Graph'ı**seçin.
   1. Uygulama **izinleri** bölümünde, doğru izinlerin seçildiğinden emin olun: **User.Read.All**.
   1. İzin **Ekle** düğmesini seçin.

## <a name="configure-the-sample-to-use-your-azure-ad-tenant"></a>Azure AD kiracınızı kullanacak şekilde örneği yapılandırma

Aşağıdaki **adımlarda, ClientID** "uygulama kimliği" veya **AppId**ile aynıdır.

Projeleri yapılandırmak için çözümü Visual Studio'da açın.

### <a name="configure-the-client-project"></a>İstemci projesini yapılandırma

Kurulum komut dosyalarını kullandıysanız, aşağıdaki değişiklikler sizin için uygulanmış olur.

1. **UserSync\Web.Config** dosyasını açın.
1. Uygulama anahtarıi bulun **ida:ClientId**. Varolan değeri Azure portalından kopyalanan **dotnet-web-daemon-v2** uygulamasının uygulama kimliğiyle değiştirin.
1. Uygulama anahtarıi bulun **ida:ClientSecret**. Varolan değeri, Azure portalında **dotnet-web-daemon-v2** uygulamasının oluşturulması sırasında kaydettiğiniz anahtarla değiştirin.

## <a name="run-the-sample"></a>Örneği çalıştırma

Çözümü temizleyin, çözümü yeniden oluşturun, UserSync uygulamasını çalıştırın ve ardından Azure AD kiracınızda yönetici olarak oturum açın. Test etmek için bir Azure AD kiracınız yoksa, bir tane almak için [bu yönergeleri uygulayabilirsiniz.](quickstart-create-new-tenant.md)

Oturum açtığınızda, uygulama ilk olarak oturum açmanız ve kullanıcı profilinizi okumanız için sizden izin ister. Bu onay, uygulamanın bir işletme kullanıcısı olduğunuzdan emin olmasını sağlar.

![Kullanıcı izni](./media/tutorial-v2-aspnet-daemon-webapp/firstconsent.png)

Uygulama daha sonra Microsoft Graph aracılığıyla Azure AD kiracınızdaki kullanıcıların listesini eşitlemeye çalışır. Bunu yapamazsa, kiracınızı (kiracı yönetici) uygulamaya bağlamanızı ister.

Uygulama daha sonra kiracınızdaki kullanıcıların listesini okumak için izin ister.

![Yönetici onayı](./media/tutorial-v2-aspnet-daemon-webapp/adminconsent.PNG)

İzin verdikten sonra, uygulamadan çıkış yapıyorsunuz. Bu oturum sonverme, Microsoft Graph için varolan tüm erişim belirteçlerinin belirteç önbelleğinden kaldırılmasını sağlar. Yeniden oturum açtığınızda, elde edilen yeni belirteç Microsoft Graph'a arama yapmak için gerekli izinlere sahip olur.


İzin verdiğınızda, uygulama herhangi bir noktada kullanıcılar için sorgu yapabilir. **Kullanıcıları Eşitle** düğmesini seçerek ve kullanıcı listesini yenileyerek bunu doğrulayabilirsiniz. Bir kullanıcı eklemeyi veya kaldırmayı ve listeyi yeniden eşitlemeyi deneyin. (Ancak uygulamanın yalnızca kullanıcıların ilk sayfasını senkronize ettiğini unutmayın.)

## <a name="about-the-code"></a>Kod hakkında

Bu örnek için ilgili kod aşağıdaki dosyalarda dır:

- **App_Start\Startup.Auth.cs**, **Denetleyiciler\AccountController.cs**: İlk oturum açma. Özellikle, denetleyiciüzerindeki eylemler, kullanıcıyı oturum açmaya zorlayan bir **Yetkilendirme** özelliğine sahiptir. Uygulama, kullanıcıda oturum açma [yetkilendirme kodu akışını](v2-oauth2-auth-code-flow.md) kullanır.
- **Denetleyiciler\SyncController.cs**: Kullanıcı listesini yerel bellek içi depoda eşitleme.
- **Denetleyiciler\UserController.cs**: Yerel bellek deposundan kullanıcı listesini görüntüler.
- **Denetleyiciler\AccountController.cs**: Yönetici onayı bitiş noktasını kullanarak kiracı yöneticiden izin alma.

## <a name="re-create-the-sample-app"></a>Örnek uygulamayı yeniden oluşturma

1. Visual Studio'da yeni bir **Visual C#** **ASP.NET Web Application (.NET Framework)** projesi oluşturun.
1. Sonraki ekranda **MVC** proje şablonu seçin. Ayrıca, daha sonra bir web API denetleyicisi ekleyeceğiniz için **Web API'sı**için klasör ve temel başvurular ekleyin. Varsayılan olarak projenin seçilen kimlik doğrulama modunu bırakın: **Kimlik Doğrulama yok.**
1. **Çözüm Gezgini** penceresindeprojeyi seçin ve **F4** tuşunu seçin.
1. Proje özelliklerinde, **SSL Etkin'den** **True'ya**ayarlayın. **SSL URL'deki**bilgilere dikkat edin. Azure portalında bu uygulamanın kaydını yapılandırırken buna ihtiyacınız vardır.
1. Aşağıdaki ASP.NET OWIN ara yazılım NuGet paketlerini ekleyin:
   - Microsoft.Owin.Security.ActiveDirectory
   - Microsoft.Owin.Security.Cookies
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.IdentityModel.Protocol.Extensions
   - Microsoft.Owin.Security.OpenIdConnect
   - Microsoft.Identity.Client
1. **App_Start** klasöründe:
   1. **Startup.Auth.cs**adlı bir sınıf oluşturun.
   1. **Kaldır. ad** alanı adından App_Start.
   1. **Başlangıç** sınıfının kodunu örnek uygulamanın aynı dosyasındaki kodla değiştirin.
   Tüm sınıf tanımını aldığından emin olun. Tanım, **ortak sınıf Başlangıç'tan** ortak kısmi sınıf Başlatma'ya **dönüşür.**
1. Startup.Auth.cs **Startup.Auth.cs**olarak, Visual Studio IntelliSense tarafından önerilen ifadeleri **kullanarak** ekleyerek eksik başvuruları giderin.
1. Projeyi sağ tıklatın, **Ekle'yi**seçin ve ardından **Sınıf'ı**seçin.
1. Arama kutusuna **OWIN**girin. **OWIN Başlangıç sınıfı** bir seçim olarak görünür. Seçin ve sınıf **Startup.cs.**
1. **Startup.cs,** **Başlangıç** sınıfının kodunu örnek uygulamanın aynı dosyasındaki kodla değiştirin. Yine, tanımın ortak **sınıf Başlangıç'tan ortak** **kısmi sınıf Başlangıç'a**değiştiğini unutmayın.
1. **Modeller** **klasörüne, MsGraphUser.cs**adlı yeni bir sınıf ekleyin. Uygulamayı örnekten aynı adı taşıyan dosyanın içeriğiyle değiştirin.
1. Yeni bir **MVC 5 Denetleyicisi** ekle - **AccountController**adı verilen boş örnek. Uygulamayı örnekten aynı adı taşıyan dosyanın içeriğiyle değiştirin.
1. Yeni bir **MVC 5 Denetleyicisi** ekle - **UserController**adı verilen boş örnek. Uygulamayı örnekten aynı adı taşıyan dosyanın içeriğiyle değiştirin.
1. Yeni bir **Web API 2 Denetleyicisi** ekle - **SyncController**adı verilen boş örnek. Uygulamayı örnekten aynı adı taşıyan dosyanın içeriğiyle değiştirin.
1. Kullanıcı arabirimi **için, Görünümler\Hesap** klasörüne, **GrantPermissions**, **Dizin**ve **UserMismatch**adlı üç **Boş (model olmadan) Görünümler** örnekleri ekleyin. **Görünümler\Kullanıcı** klasöründe Dizin ekleyin ve bir adlandırılmış **Dizin.** Uygulamayı örnekten aynı adı taşıyan dosyanın içeriğiyle değiştirin.
1. Çeşitli görünümleri doğru bir şekilde birbirine bağlamak için **Paylaşılan\_Düzen.cshtml** ve **Ana Sayfa\Index.cshtml'i** güncelleştirin.

## <a name="deploy-the-sample-to-azure"></a>Örneği Azure'a dağıtma

Bu proje web uygulaması ve web API projeleri vardır. Bunları Azure web sitelerine dağıtmak için her biri için aşağıdaki adımları izleyin:

1. Bir Azure web sitesi oluşturun.
1. Web uygulamasını ve web API'lerini web sitesinde yayınlayın.
1. Müşterileri IIS Express yerine web sitesini aramak için güncelleyin.

### <a name="create-and-publish-dotnet-web-daemon-v2-to-an-azure-website"></a>Bir Azure web sitesinde dotnet-web-daemon-v2 oluşturma ve yayımlama

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol üst köşeden **Kaynak oluştur**'u seçin.
1. **Web** > **Web App'i**seçin ve ardından web sitenize bir ad verin. Örneğin, **dotnet-web-daemon-v2-contoso.azurewebsites.net.**
1. **Abonelik,** **Kaynak grubu**ve Uygulama hizmet planı **ve konumu**için bilgileri seçin. **İşletim Sistemi** **Windows'dur**ve **Yayımla** **Koddur.**
1. **Oluştur'u** seçin ve uygulama hizmetinin oluşturulmasını bekleyin.
1. **Dağıtım'ın başarılı** bildirimini aldığınızda, yeni oluşturulan uygulama hizmetine gitmek **için kaynağa git'i** seçin.
1. Web sitesi oluşturulduktan sonra, **Pano'da** bulun ve uygulama hizmetinin **Genel Bakış** ekranını açmak için seçin.
1. Uygulama hizmetinin **Genel Bakış** sekmesinden, yayın profilini al bağlantısını seçerek **yayımlama** profilini indirin ve kaydedin. Kaynak denetiminden dağıtım gibi diğer dağıtım mekanizmalarını kullanabilirsiniz.
1. Visual Studio'ya geçin ve sonra:
   1. **dotnet-web-daemon-v2** projesine gidin.
   1. Çözüm Gezgini'nde projeyi sağ tıklatın ve sonra **Yayımla'yı**seçin.
   1. Alt çubukta **Profili İçe Aktar'ı** seçin ve daha önce indirdiğiniz yayımlama profilini aktarın.
1. **Yapılandır**'ı seçin.
1. **Bağlantı** sekmesinde, "https" kullanabilmesi için hedef URL'yi güncelleştirin. Örneğin, kullanın. [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net) **İleri**’yi seçin.
1. **Ayarlar** sekmesinde, Kuruluş **Kimlik Doğrulamasını Etkinleştir'in** temizolduğundan emin olun.
1. **Kaydet**’i seçin. Ana ekranda **Yayımla'yı** seçin.

Visual Studio projeyi yayımlar ve otomatik olarak projenin URL'sine bir tarayıcı açar. Projenin varsayılan web sayfasını görürseniz, yayın başarılı oldu.

### <a name="update-the-azure-ad-tenant-application-registration-for-dotnet-web-daemon-v2"></a>dotnet-web-daemon-v2 için Azure AD kiracı başvuru kaydını güncelleştirme

1. [Azure portalına](https://portal.azure.com) geri dönün.
1. Sol bölmede, Azure **Etkin Dizin** hizmetini seçin ve ardından **Uygulama kayıtlarını**seçin.
1. **dotnet-web-daemon-v2** uygulamasını seçin.
1. Uygulamanızın **Kimlik Doğrulama** sayfasında, Hizmet adresinizle birlikte Giriş **URL** alanlarını güncelleyin. Örneğin, kullanın. [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net)
1. **Markalama** **menüsünden, Giriş sayfası URL'sini** hizmet adresinize güncelleyin. Örneğin, kullanın. [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net)
1. Yapılandırmayı kaydedin.
1. **Kimlik Doğrulama** > **Yönlendirme URI'leri** menüsünün değerler listesine aynı URL'yi ekleyin. Birden çok yönlendirme URL'niz varsa, her yönlendirme URL'si için uygulama hizmetinin URI'sini kullanan yeni bir giriş olduğundan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekmediğinde, [uygulama adımınızı kaydedin'de](#register-your-application) oluşturduğunuz uygulama nesnesini silin.  Uygulamayı kaldırmak için, sizin [veya kuruluşunuz tarafından yazılmış bir uygulamayı kaldır'daki](quickstart-remove-app.md#remove-an-application-authored-by-you-or-your-organization)yönergeleri izleyin.

## <a name="get-help"></a>Yardım alın

Topluluktan destek almak için [Stack Overflow'u](http://stackoverflow.com/questions/tagged/msal) kullanın.
Önce Stack Overflow ile ilgili sorularınızı sorun ve birisinin sorunuza daha önce sorup sormadınızı görmek için varolan sorunlara göz atın.
Sorularınızın veya yorumlarınızın "adal", "msal" ve "dotnet" ile etiketlendirilip etiketlendirilediğinden emin olun.

Örnekte bir hata bulursanız, lütfen [GitHub Sorunları'nda](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/issues)sorunu gündeme getirin.

MSAL.NET bir hata bulursanız, lütfen [MSAL.NET GitHub Sorunları'nda](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues)sorunu gündeme getirin.

Bir öneri sağlamak için [Kullanıcı Sesi sayfasına](https://feedback.azure.com/forums/169401-azure-active-directory)gidin.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft kimlik platformunun desteklediği farklı [Kimlik Doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin.

Daha fazla bilgi için aşağıdaki kavramsal belgelere bakın:

- [Azure Etkin Dizinde Kiralama](single-and-multi-tenant-apps.md)
- [Azure AD uygulama onayı deneyimlerini anlama](application-consent-experience.md)
- [Çok kiracılı uygulama deseni kullanarak herhangi bir Azure Active Directory kullanıcısını oturum açın](howto-convert-app-to-be-multi-tenant.md)
- [Kullanıcı ve yönetici onayLarını anlama](howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)
- [Azure Active Directory'deki uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md)
- [Quickstart: Microsoft kimlik platformuile bir uygulama kaydetme](quickstart-register-app.md)
- [Hızlı başlatma: Web API'lerine erişmek için istemci uygulamasını yapılandırma](quickstart-configure-app-access-web-apis.md)
- [İstemci kimlik bilgileri akışları olan bir uygulama için belirteç alma](msal-client-applications.md)

Daha basit bir çok kiracılı konsol daemon uygulaması için [.NET Core daemon quickstart'a](quickstart-v2-netcore-daemon.md)bakın.
