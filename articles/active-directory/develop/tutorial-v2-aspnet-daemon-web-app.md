---
title: Azure AD ile korunan bir ASP.NET Web API 'SI çağırma-Microsoft Identity platform
description: Bu hızlı başlangıçta, Windows Masaüstü (WPF) uygulamasından Azure Active Directory tarafından korunan bir ASP.NET Web API 'sinin nasıl çağrılacağını öğrenin. WPF istemcisi bir kullanıcının kimliğini doğrular, bir erişim belirteci ister ve Web API 'sini çağırır.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.collection: M365-identity-device-management
ms.openlocfilehash: c558d45702498e6c1164d7ee1731e80ff195349e
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328419"
---
# <a name="build-a-multi-tenant-daemon-with-the-microsoft-identity-platform-endpoint"></a>Microsoft Identity platform uç noktası ile çok kiracılı bir Daemon oluşturma

Bu öğreticide Microsoft Identity platformunu kullanarak, uzun süreli ve etkileşimli olmayan bir işlemle Microsoft iş müşterilerinin verilerine nasıl erişebileceğinizi öğreneceksiniz. Örnek arka plan programı, daha sonra [Microsoft Graph](https://graph.microsoft.io) çağırmak ve kuruluş verilerine erişmek için kullanılan bir erişim belirteci almak için [OAuth2 istemci kimlik bilgilerini](v2-oauth2-client-creds-grant-flow.md) kullanır.

Uygulama bir ASP.NET MVC uygulaması olarak oluşturulmuştur ve Kullanıcı oturumu açmak için OWIN OpenID Connect ara yazılımını kullanır.  Bu örnekteki "Daemon" bileşeni, çağrıldığında müşterinin Azure AD kiracısındaki Kullanıcı listesini Microsoft Graph bir API denetleyicisidir.  Bu `SyncController.cs`, Web uygulamasındaki bir AJAX çağrısıyla tetiklenir ve Microsoft Graph için bir erişim belirteci almak üzere [.net Için Microsoft kimlik doğrulama kitaplığı 'nı (msal)](msal-overview.md) kullanır.

Daha basit bir konsol Daemon uygulaması için [.NET Core Daemon hızlı başlangıç](quickstart-v2-netcore-daemon.md)' a göz atın.

## <a name="scenario"></a>Senaryo

Uygulama, herhangi bir Microsoft iş müşterisi tarafından kullanılmak üzere tasarlanan çok kiracılı bir uygulama olduğundan, müşterilerin uygulamayı şirket verilerine "kaydolmalarına" veya "bağlanmasına" olanak sağlaması gerekir.  Bağlantı akışı sırasında, şirket yöneticisi öncelikle uygulamaya doğrudan uygulama **izinleri** verir, böylece oturum açmış bir Kullanıcı mevcut olmadan şirket verilerine etkileşimli olmayan bir biçimde erişebilir.  Bu örnekteki mantığın çoğunluğunda, kimlik Platform [Yöneticisi onay](v2-permissions-and-consent.md#using-the-admin-consent-endpoint) uç noktası kullanılarak bu bağlantı akışının nasıl elde edilebileceği gösterilmektedir.

![Topoloji](./media/tutorial-v2-aspnet-daemon-webapp/topology.png)

Bu örnekte kullanılan kavramlar hakkında daha fazla bilgi için [kimlik platformu uç noktası istemci kimlik bilgileri Protokolü belgelerini](v2-oauth2-client-creds-grant-flow.md)okuduğunuzdan emin olun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta örnek çalıştırmak için şunlar gerekir:

- [Visual Studio 2017 veya 2019](https://visualstudio.microsoft.com/downloads/)
- Azure Active Directory (Azure AD) kiracısı. Azure AD kiracısı alma hakkında daha fazla bilgi için bkz. [Azure AD kiracısı alma](quickstart-create-new-tenant.md)
- Azure AD kiracınızdaki bir veya daha fazla kullanıcı hesabı. Bu örnek, bir Microsoft hesabı (eski adıyla Windows Live hesabı) çalışmaz. Bu nedenle, bir Microsoft hesabı [Azure Portal](https://portal.azure.com) oturum açtıysanız ve daha önce dizininizde hiç bir kullanıcı hesabı oluşturmadıysanız, bunu şimdi yapmanız gerekir.

## <a name="clone-or-download-this-repository"></a>Bu depoyu Kopyala veya indir

Kabuk veya komut satırınızdan:

```Shell
git clone https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2.git
```

Ya da [örneği BIR ZIP dosyasına indirin](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/archive/master.zip).

## <a name="register-the-sample-application-with-your-azure-ad-tenant"></a>Örnek uygulamayı Azure AD kiracınızla kaydetme

Bu örnekte bir proje var. Kaydolmak için şunları yapabilirsiniz:

- Adımları izleyerek [örneği Azure Active Directory kiracınızla kaydedin](#register-the-sample-application-with-your-azure-ad-tenant) ve [örneği Azure AD kiracınızı kullanacak şekilde yapılandırın](#choose-the-azure-ad-tenant-for-the-applications)
- Veya PowerShell betiklerini kullanarak şunları yapın:
  - , Azure AD uygulamalarını ve ilgili nesneleri (parolalar, izinler, bağımlılıklar) sizin için **otomatik olarak** oluşturur
  - Visual Studio projelerinin yapılandırma dosyalarını değiştirin.

Bu Otomasyonu kullanmak istiyorsanız:

1. Windows üzerinde PowerShell 'i çalıştırın ve kopyalanan dizinin köküne gidin
1. PowerShell çalıştırmasında:

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
   ```

1. Azure AD uygulamanızı oluşturmak ve örnek uygulamanın kodunu uygun şekilde yapılandırmak için betiği çalıştırın.
1. PowerShell çalıştırmasında:

   ```PowerShell
   .\AppCreationScripts\Configure.ps1
   ```

   > Betikleri çalıştırmanın diğer yolları [uygulama oluşturma betiklerine](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/blob/master/AppCreationScripts/AppCreationScripts.md) göre açıklanmıştır

1. Visual Studio çözümünü açın ve kodu çalıştırmak için Başlat ' a tıklayın.

Bu Otomasyonu kullanmak istemiyorsanız, aşağıdaki adımları izleyin.

### <a name="choose-the-azure-ad-tenant-for-the-applications"></a>Uygulamalar için Azure AD kiracısını seçin

İlk adım olarak şunları yapmanız gerekir:

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Hesabınız birden fazla Azure AD kiracısında mevcutsa, sayfanın üstündeki menüdeki sağ üst köşedeki profilinizi seçin ve ardından dizin ' i **değiştirin**.
   Portal oturumunuzu istenen Azure AD kiracısına değiştirin.

### <a name="register-the-client-app-dotnet-web-daemon-v2"></a>İstemci uygulamasını kaydetme (DotNet-Web-Daemon-v2)

1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Yeni kayıt**seçeneğini belirleyin.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `dotnet-web-daemon-v2`.
   - **Desteklenen hesap türleri** bölümünde, **herhangi bir kuruluş dizininde hesaplar**' ı seçin.
   - Yeniden yönlendirme URI 'SI (isteğe bağlı) bölümünde, açılan kutudan **Web** ' i seçin ve aşağıdaki yeniden yönlendirme URI 'lerini girin:
       - `https://localhost:44316/`
       - `https://localhost:44316/Account/GrantPermissions` birden fazla yeniden yönlendirme URI 'si varsa, uygulama başarıyla oluşturulduktan sonra bunları **kimlik doğrulama** sekmesinden eklemeniz gerekir.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamaya **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin. Bu proje için Visual Studio yapılandırma dosyasını yapılandırmak için gerekli olacaktır.
1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
   - **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL 'sini** `https://localhost:44316/Account/EndSession` olarak ayarlayın
   - **Gelişmiş ayarlar** | **örtük verme** bölümünde, bu örnek, kullanıcının oturum açması için [örtük verme AKıŞıNıN](v2-oauth2-implicit-grant-flow.md) etkinleştirilmesini gerektirdiğinden ve bir API çağırdıkça, **erişim belirteçlerini** ve **kimlik belirteçlerini** denetleyin.
1. **Kaydet**’i seçin.
1. **Sertifikalar & gizlilikler** sayfasında, **istemci gizli** dizileri bölümünde **yeni istemci parolası**' nı seçin:

   - Bir anahtar açıklaması yazın (örneğin `app secret`),
   - **1 yılda**, **2 yıl içinde**bir anahtar süresi seçin veya **hiçbir zaman sona ermez**.
   - **Ekle** düğmesine bastığınızda, anahtar değeri görüntülenir, kopyalayın ve değeri güvenli bir konumda kaydeder.
   - Bu anahtar daha sonra Visual Studio 'da projeyi yapılandırmak için gereklidir. Bu anahtar değeri bir daha görüntülenmez veya başka yollarla alınabilir, bu nedenle Azure portal görünür hale geldiğinde onu kaydedin.
1. Uygulama için sayfa listesinde, **API izinleri** ' ni seçin.
   - **Izin Ekle** düğmesine tıklayın ve ardından
   - **Microsoft API 'leri** sekmesinin seçili olduğundan emin olun
   - *Yaygın olarak kullanılan Microsoft API 'leri* bölümünde, **Microsoft Graph** ' ye tıklayın.
   - **Uygulama izinleri** bölümünde, doğru izinlerin işaretli olduğundan emin olun: **User. Read. All**
   - **Izin Ekle** düğmesini seçin

## <a name="configure-the-sample-to-use-your-azure-ad-tenant"></a>Örneği Azure AD kiracınızı kullanacak şekilde yapılandırın

Aşağıdaki adımlarda, "ClientID", "uygulama KIMLIĞI" veya "AppID" ile aynıdır.

Projeleri yapılandırmak için Visual Studio 'da çözümü açın

### <a name="configure-the-client-project"></a>İstemci projesini yapılandırma

Kurulum betiklerini kullandıysanız, sizin için aşağıdaki değişiklikler uygulanmış olur.

1. `UserSync\Web.Config` dosyasını açın
1. Uygulama anahtarı `ida:ClientId` bulun ve mevcut değeri, Azure portal kopyaladığınız `dotnet-web-daemon-v2` uygulamanın uygulama KIMLIĞI (ClientID) ile değiştirin.
1. Uygulama anahtarı `ida:ClientSecret` bulun ve mevcut değeri, Azure portal `dotnet-web-daemon-v2` uygulamasının oluşturulması sırasında kaydettiğiniz anahtarla değiştirin.

## <a name="run-the-sample"></a>Örneği çalıştırma

Çözümü temizleyin, çözümü yeniden derleyin ve UserSync uygulamasını çalıştırın ve Azure AD kiracınızda yönetici olarak oturum açarak başlayın.  Test için bir Azure AD kiracınız yoksa, bir tane almak için [Bu yönergeleri izleyebilirsiniz](quickstart-create-new-tenant.md) .

Oturum açtığınızda, uygulama öncelikle Kullanıcı profilinizi okumak & oturumunuzu açmanız için sizden izin ister.  Bu izin, uygulamanın bir iş kullanıcısı olduğunuzdan emin olmanızı sağlar.

![Kullanıcı onayı](./media/tutorial-v2-aspnet-daemon-webapp/firstconsent.png)

Uygulama daha sonra Microsoft Graph aracılığıyla Azure AD kiracınızdan bir kullanıcı listesini eşitlemeye çalışır.  Bunu yapamaması durumunda, kiracınızı uygulamaya bağlamak için sizden (kiracı yöneticisi) sorun olacaktır.

Daha sonra uygulama, kiracınızdaki kullanıcıların listesini okumak için izin ister.

![Yönetici onayı](./media/tutorial-v2-aspnet-daemon-webapp/adminconsent.PNG)

**İzin verdikten sonra uygulamadan oturumunuz açılır**. Bu işlem, Graph için var olan erişim belirteçlerinin belirteç önbelleğinden kaldırıldığından emin olmak için yapılır. Yeniden oturum açtıktan sonra, elde edilen yeni belirteç MS grafiğine çağrı yapmak için gerekli izinlere sahip olur.
İzin verdiğinizde, uygulama herhangi bir noktada kullanıcıları sorgulayabilir.  Kullanıcılar sayfasında kullanıcıları **Eşitle** düğmesine tıklayıp Kullanıcı listesini yenileyerek bunu doğrulayabilirsiniz.  Bir Kullanıcı eklemeyi veya kaldırmayı deneyin (ancak yalnızca kullanıcıların ilk sayfasını eşitletdiğine unutmayın!).

## <a name="about-the-code"></a>Kod hakkında

Bu örnek için ilgili kod aşağıdaki dosyalardır:

- İlk oturum açma: `App_Start\Startup.Auth.cs`, `Controllers\AccountController.cs`.  Özellikle, denetleyicideki eylemlerin bir Yetkilendir özniteliği vardır ve bu, kullanıcıyı oturum açmaya zorlar. Uygulama, kullanıcının oturum açması için [yetkilendirme kodu akışını](v2-oauth2-auth-code-flow.md) kullanır.
- Kullanıcı listesini yerel bellek içi depoda eşitleme: `Controllers\SyncController.cs`
- Yerel bellek içi depodaki kullanıcıların listesini görüntüleme: `Controllers\UserController.cs`
- Yönetici onay uç noktasını kullanarak kiracı yöneticisinden izinler alınıyor: `Controllers\AccountController.cs`

## <a name="recreate-this-sample-app"></a>Bu örnek uygulamayı yeniden oluştur

1. Visual Studio 'da yeni bir `Visual C#` `ASP.NET Web Application (.NET Framework)` projesi oluşturun. Sonraki ekranda `MVC` proje şablonunu seçin. Ayrıca, daha sonra bir Web API denetleyicisi eklerken `Web API` için klasör ve çekirdek başvurular ekleyin.  Projenin seçili kimlik doğrulama modu ' nu varsayılan, yani `No Authentication`olarak bırakın.
2. **Çözüm Gezgini** penceresinde projeyi seçin ve proje özelliklerini getirmek için **F4** tuşuna basın. Proje özellikleri 'nde **SSL** 'yi `True`olarak ayarlayın. **SSL URL 'sini**aklınızda edin. Azure portal bu uygulamanın kaydını yapılandırırken ihtiyacınız olacak.
3. Aşağıdaki ASP.Net OWIN ara yazılımını ekleyin Nual: `Microsoft.Owin.Security.ActiveDirectory`, `Microsoft.Owin.Security.Cookies` ve `Microsoft.Owin.Host.SystemWeb`, `Microsoft.IdentityModel.Protocol.Extensions`, `Microsoft.Owin.Security.OpenIdConnect` ve `Microsoft.Identity.Client`. 
4. `App_Start` klasöründe bir sınıf `Startup.Auth.cs`oluşturun. Ad alanı adından `.App_Start` kaldırın.  `Startup` sınıfı için kodu, örnek uygulamanın aynı dosyasındaki kodla değiştirin.  Tüm sınıf tanımını aldığınızdan emin olun!  Tanım `public class Startup` `public partial class Startup` olarak değişir
5. `Startup.Auth.cs` içinde, Visual Studio IntelliSense tarafından önerildiği şekilde `using` deyimleri ekleyerek eksik başvuruları çözün.
6. Projeye sağ tıklayın, Ekle ' yi seçin, "sınıf" ' i seçin ve arama kutusuna "OWIN" yazın.  "OWıN Startup Class" seçim olarak görünür; seçin ve `Startup.cs`sınıfı adlandırın.
7. `Startup.cs`, `Startup` sınıfının kodunu örnek uygulamanın aynı dosyasındaki kodla değiştirin.  Tanımı, `public class Startup` `public partial class Startup`olarak değiştirir.
8. Klasöründe `MsGraphUser.cs`adlı yeni bir sınıf ekleyin.  Uygulamayı örnekteki aynı adlı dosyanın içeriğiyle değiştirin.
9. Yeni bir **MVC 5 denetleyicisi ekleme-** `AccountController`adlı boş. Uygulamayı örnekteki aynı adlı dosyanın içeriğiyle değiştirin.
10. Yeni bir **MVC 5 denetleyicisi ekleme-** `UserController`adlı boş. Uygulamayı örnekteki aynı adlı dosyanın içeriğiyle değiştirin.
11. Yeni bir **Web API 2 denetleyicisi ekleyin-** `SyncController`adlı bir boş. Uygulamayı örnekteki aynı adlı dosyanın içeriğiyle değiştirin.
12. Kullanıcı arabirimi için, `Views\Account` klasöründe, `GrantPermissions`, `Index` ve `UserMismatch` adlı üç **boş (model olmadan) görünümleri** ve `Index` klasöründe bir adlandırılmış `Views\User` ekleyin. Uygulamayı örnekteki aynı adlı dosyanın içeriğiyle değiştirin.
13. `Shared\_Layout.cshtml` ve `Home\Index.cshtml` çeşitli görünümleri birbirine doğru şekilde bağlamak için güncelleştirin.

## <a name="deploy-this-sample-to-azure"></a>Bu örneği Azure 'a dağıtın

Bu projede WebApp/Web API projeleri vardır. Bunları Azure Web sitelerine dağıtmak için, her biri için şunları yapmanız gerekir:

- Azure Web sitesi oluşturma
- Web uygulaması/Web API 'Lerini Web sitesinde yayımlayın ve
- istemci (ler) i IIS Express yerine Web sitesini çağırmak için güncelleştirin.

### <a name="create-and-publish-the-dotnet-web-daemon-v2-to-an-azure-web-site"></a>`dotnet-web-daemon-v2` Azure Web sitesinde oluşturma ve yayımlama

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol üst köşedeki `Create a resource` ' a tıklayın, **web** --> **Web uygulaması**' nı seçin ve Web sitenize bir ad verin, örneğin `dotnet-web-daemon-v2-contoso.azurewebsites.net`.
1. Bundan sonra `Subscription`, `Resource Group``App service plan and Location`seçin. `OS`, **Windows** ve `Publish` **kod**olacaktır.
1. `Create` ' a tıklayın ve App Service oluşturulmasını bekleyin.
1. `Deployment succeeded` bildirimi aldıktan sonra, yeni oluşturulan App Service ' e gitmek için `Go to resource` ' a tıklayın.
1. Web sitesi oluşturulduktan sonra **panoyu panoda** bulun ve **App Services** **genel bakış** ekranını açmak için tıklayın.
1. App Service **genel bakış** sekmesinden yayımlama profilini **Al** bağlantısına tıklayarak yayımlama profilini indirin ve kaydedin.  Kaynak denetimindeki gibi diğer dağıtım mekanizmaları de kullanılabilir.
1. Visual Studio 'ya geçin ve DotNet-Web-Daemon-v2 projesine gidin.  Çözüm Gezgini projeye sağ tıklayın ve **Yayımla**' yı seçin.  Alt çubukta **profili Içeri aktar** ' a tıklayın ve daha önce indirdiğiniz yayımlama profilini içeri aktarın.
1. **Yapılandır** ' a tıklayın ve `Connection tab`, hedef URL 'yi giriş sayfası URL 'sindeki bir `https` olacak şekilde güncelleştirin, örneğin [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net). **İleri**’ye tıklayın.
1. Ayarlar sekmesinde `Enable Organizational Authentication` seçili olmadığından emin olun.  **Save (Kaydet)** düğmesine tıklayın. Ana ekranda **Yayımla** ' ya tıklayın.
1. Visual Studio projeyi yayımlayacak ve projenin URL 'sine otomatik olarak bir tarayıcı açacak.  Projenin varsayılan Web sayfasını görürseniz, yayın başarılı olmuştur.

### <a name="update-the-azure-ad-tenant-application-registration-for-dotnet-web-daemon-v2"></a>`dotnet-web-daemon-v2` için Azure AD kiracı uygulaması kaydını güncelleştirme

1. [Azure Portal](https://portal.azure.com)geri gidin.
Sol taraftaki Gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve **uygulama kayıtları**' ı seçin.
1. Sonuç ekranında `dotnet-web-daemon-v2` uygulamasını seçin.
1. **Kimlik doğrulamasında** | sayfasında, oturum kapatma URL 'SI alanlarını hizmetinizin adresiyle güncelleştirin, örneğin [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net)
1. *Marka* menüsünde, **giriş sayfası URL 'sini**hizmetinizin adresine güncelleştirin, örneğin [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net). Yapılandırmayı kaydedin.
1. *Kimlik doğrulama-> yeniden yönlendirme URI 'leri* menüsündeki değerler listesinde aynı URL 'yi ekleyin. Birden çok yeniden yönlendirme URL 'Si varsa, her yönlendirme URL 'si için App Service 'in URI 'sini kullanarak yeni bir giriş olduğundan emin olun.

## <a name="community-help-and-support"></a>Topluluk yardım ve destek

Topluluktan destek almak için [Stack Overflow](http://stackoverflow.com/questions/tagged/msal) kullanın.
Önce Stack Overflow sorularınızı sorun ve daha önce sorunuzu isteyip istemediğini görmek için mevcut sorunları inceleyin.
Sorularınıza veya yorumlarınızın [`adal` `msal` `dotnet`] ile etiketlendiğinden emin olun.

Örnekte hata bulursanız ve hata varsa, lütfen [GitHub sorunları](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/issues)üzerinde sorunu yükseltin.

MSAL.NET içinde bir hata bulursanız, lütfen sorunu [msal.net GitHub sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues)üzerinde yükseltin.

Öneri sağlamak için aşağıdaki [Kullanıcı sesi sayfasını](https://feedback.azure.com/forums/169401-azure-active-directory)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Identity platformunun desteklediği farklı [kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) hakkında daha fazla bilgi edinin.

Daha fazla bilgi için aşağıdaki kavramsal belgelere bakın:

- [Azure Active Directory kiracı](single-and-multi-tenant-apps.md)
- [Azure AD uygulaması onay deneyimlerini anlama](application-consent-experience.md)
- [Nasıl yapılır: çok kiracılı uygulama modelini kullanarak tüm Azure Active Directory kullanıcıları oturum açma](howto-convert-app-to-be-multi-tenant.md)
- [Kullanıcı ve yönetici onayını anlama](howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)
- [Azure Active Directory içindeki uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md)
- [Hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme](quickstart-register-app.md)
- [Hızlı başlangıç: Web API 'Lerine erişmek için bir istemci uygulaması yapılandırma](quickstart-configure-app-access-web-apis.md)
- [İstemci kimlik bilgisi akışlarıyla bir uygulama için belirteç alma](msal-client-applications.md)

Daha basit bir çok kiracılı konsol Daemon uygulaması için [.NET Core Daemon hızlı başlangıç](quickstart-v2-netcore-daemon.md)' a göz atın.
