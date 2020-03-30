---
title: .NET arka uç sunucusu SDK ile çalışma
description: Azure App Service Mobile Apps için .NET arka uç sunucusu SDK ile nasıl çalışacağınızı öğrenin.
keywords: uygulama hizmeti, azure uygulama hizmeti, mobil uygulama, mobil hizmet, ölçek, ölçeklenebilir, uygulama dağıtımı, azure uygulama dağıtımı
author: conceptdev
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.openlocfilehash: cafb0a7e2bf0fbce82448236a2da98079144121e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250146"
---
# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Azure Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışın
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Bu konu, önemli Azure App Service Mobile Apps senaryolarında .NET arka uç sunucusu SDK'nın nasıl kullanılacağını gösterir. Azure Mobil Uygulamalar SDK, ASP.NET uygulamanızdan mobil istemcilerle çalışmanıza yardımcı olur.

> [!TIP]
> [Azure Mobile Apps için .NET sunucusu SDK,][2] GitHub'da açık kaynak koddur. Depo, tüm sunucu SDK birim test paketi ve bazı örnek projeler de dahil olmak üzere tüm kaynak kodunu içerir.
>
>

## <a name="reference-documentation"></a>Referans belgeleri
Sunucu SDK için başvuru belgeleri burada bulunur: [Azure Mobile Apps .NET Başvuru][1].

## <a name="how-to-create-a-net-mobile-app-backend"></a><a name="create-app"></a>Nasıl yapilir: .NET Mobile App arka uç oluşturma
Yeni bir projeye başlıyorsanız, [Azure portalını] veya Visual Studio'yu kullanarak bir Uygulama Hizmeti uygulaması oluşturabilirsiniz. Uygulama Hizmeti uygulamasını yerel olarak çalıştırabilir veya projeyi bulut tabanlı App Service mobil uygulamanızda yayınlayabilirsiniz.

Varolan bir projeye mobil özellikler ekliyorsanız, İndir bölümüne bakın ve SDK bölümünü [başlatın.](#install-sdk)

### <a name="create-a-net-backend-using-the-azure-portal"></a>Azure portalını kullanarak bir .NET arka uç oluşturma
Bir Uygulama Hizmeti mobil arka uç oluşturmak [için, Quickstart öğreticiizleyin][3] veya aşağıdaki adımları izleyin:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

*Başlangıç* bıçağına geri dön, **tablo API'si oluştur**altında, Arka **uç diliniz**olarak **C#'ı** seçin. **İndir'i**tıklatın, sıkıştırılmış proje dosyalarını yerel bilgisayarınıza ayıklayın ve Çözümü Visual Studio'da açın.

### <a name="create-a-net-backend-using-visual-studio-2017"></a>Visual Studio 2017'yi kullanarak .NET arka uç oluşturma

Visual Studio'dan Azure Mobil Uygulamalar projesinde yayımlamak için Visual Studio Yükleyici aracılığıyla Azure iş yükünü yükleyin. SDK'yı yükledikten sonra, aşağıdaki adımları kullanarak bir ASP.NET uygulaması oluşturun:

1. Yeni **Proje** iletişim kutusunu açın **(Dosya** > **Yeni** > **Proje...'den).**
2. **Visual C#** seçeneğini genişletin ve **Web'i**seçin.
3. **Web Uygulaması (.NET Framework) ASP.NET**seçin.
4. Proje adını doldurun. Ardından **Tamam**'a tıklayın.
5. Şablonlar listesinden **Azure Mobil Uygulamasını** seçin.
6. Çözümü oluşturmak için **Tamam'ı** tıklatın.
7. **Solution Explorer'daki** projeye sağ tıklayın ve **Yayımla'yı seçin...** **App Service**
8. Kimlik doğrulama istemlerini izleyin ve yayımlanacak yeni veya varolan bir Azure Uygulama Hizmeti seçin.

### <a name="create-a-net-backend-using-visual-studio-2015"></a>Visual Studio 2015'i kullanarak .NET arka uç oluşturma

Visual Studio'da bir Azure Mobil Uygulamalar projesi oluşturmak [için .NET için .NET][4] (sürüm 2.9.0 veya sonraki sürüm) için Azure SDK'yı yükleyin. SDK'yı yükledikten sonra, aşağıdaki adımları kullanarak bir ASP.NET uygulaması oluşturun:

1. Yeni **Proje** iletişim kutusunu açın **(Dosya** > **Yeni** > **Proje...'den).**
2. **Şablonları** > **Görsel C#** genişletin ve **Web'i**seçin.
3. **ASP.NET Web Uygulaması**'nı seçin.
4. Proje adını doldurun. Ardından **Tamam**'a tıklayın.
5. *4.5.2 Şablonları ASP.NET*altında **Azure Mobil Uygulamasını**seçin. Bu projeyi yayımlayabileceğiniz bulutta bir mobil arka uç oluşturmak için **bulutta Ana Bilgisayar'ı** denetleyin.
6. **Tamam**'a tıklayın.

## <a name="how-to-download-and-initialize-the-sdk"></a><a name="install-sdk"></a>Nasıl yapılsın: SDK'yı indirin ve başlatma
SDK [NuGet.org]mevcuttur. Bu paket, SDK'yı kullanmaya başlamak için gereken temel işlevselliği içerir. SDK'yı başlatmanız için **HttpConfiguration** nesnesi üzerinde eylem gerçekleştirmeniz gerekir.

### <a name="install-the-sdk"></a>SDK yükle
SDK'yı yüklemek için Visual Studio'daki sunucu projesine sağ tıklayın, **NuGet Paketlerini Yönet'i**seçin, [Microsoft.Azure.Mobile.Server] paketini arayın ve ardından **Yükle'yi**tıklatın.

### <a name="initialize-the-server-project"></a><a name="server-project-setup"></a>Sunucu projesini başlatma
Bir .NET arka uç sunucu projesi, bir OWIN başlangıç sınıfı ekleyerek diğer ASP.NET projelerine benzer şekilde başharfe çevrilir. NuGet paketine `Microsoft.Owin.Host.SystemWeb`başvurulduğunuzdan emin olun. Visual Studio'da bu sınıfı eklemek için sunucu projenize sağ tıklayın ve**Yeni Öğe** **Ekle'yi** >
seçin, ardından **Web** > **Genel** > **OWIN Başlangıç sınıfı.**  Bir sınıf aşağıdaki öznitelik ile oluşturulur:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

OWIN `Configuration()` başlangıç sınıfınızın yönteminde, Azure Mobil Uygulamalar ortamını yapılandırmak için bir **HttpConfiguration** nesnesi kullanın.
Aşağıdaki örnek, hiçbir ek özelliği ile sunucu proje sini başlatır:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Tek tek özellikleri etkinleştirmek **için, ApplyTo'yu**aramadan önce **MobileAppConfiguration** nesnesindeki uzantı yöntemlerini aramanız gerekir. Örneğin, aşağıdaki kod, başlatma sırasında özniteliği `[MobileAppController]` olan tüm API denetleyicilerine varsayılan yolları ekler:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Azure portalından sunucu hızlı **başlatı() UseDefaultConfiguration()** olarak adlandırıyor. Bu, aşağıdaki kuruluma eşdeğerdir:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Kullanılan uzantı yöntemleri şunlardır:

* `AddMobileAppHomeController()`varsayılan Azure Mobil Uygulamalar giriş sayfasını sağlar.
* `MapApiControllers()`öznitelik ile `[MobileAppController]` dekore WebAPI denetleyicileri için özel API yetenekleri sağlar.
* `AddTables()`uç noktaların `/tables` tablo denetleyicilerine eşlenemesini sağlar.
* `AddTablesWithEntityFramework()`Entity Framework tabanlı denetleyicileri `/tables` kullanarak uç noktaları eşleme için kısa bir eldir.
* `AddPushNotifications()`Bildirim Hub'ları için aygıtları kaydetmek için basit bir yöntem sağlar.
* `MapLegacyCrossDomainController()`yerel kalkınma için standart CORS başlıkları sağlar.

### <a name="sdk-extensions"></a>SDK uzantıları
Aşağıdaki NuGet tabanlı uzantı paketleri, uygulamanız tarafından kullanılabilecek çeşitli mobil özellikler sağlar. **MobileAppConfiguration** nesnesini kullanarak başlatma sırasında uzantıları etkinleştirin.

* [Microsoft.Azure.Mobile.Server.Quickstart] temel Mobil Uygulamalar kurulumlarını destekler. Başlatma sırasında **UseDefaultConfiguration** uzantı yöntemini çağırarak yapılandırmaya eklendi. Bu uzantı aşağıdaki uzantıları içerir: Bildirimler, Kimlik Doğrulama, Varlık, Tablolar, Çapraz etki alanı ve Ev paketleri. Bu paket, Azure portalında bulunan Mobile Apps Quickstart tarafından kullanılır.
* [Microsoft.Azure.Mobile.Server.Home](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) *varsayılan olarak bu mobil uygulamanın* web sitesi kökü için sayfayı çalışır durumda dır. **AddMobileAppHomeController** uzantı yöntemini arayarak yapılandırmaya ekleyin.
* [Microsoft.Azure.Mobile.Server.Tables](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) veri ile çalışmak ve veri ardışık alanı kurmak için sınıflar içerir. **AddTables uzantı** yöntemini çağırarak yapılandırmaya ekleyin.
* [Microsoft.Azure.Mobile.Server.Entity,](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Varlık Çerçevesi'nin SQL Veritabanındaki verilere erişmesini sağlar. **AddTablesWithEntityFramework** uzantı yöntemini arayarak yapılandırmaya ekleyin.
* [Microsoft.Azure.Mobile.Server.Authentication] Kimlik doğrulamayı etkinleştirin ve belirteçleri doğrulamak için kullanılan OWIN aracını ayarlar. **AddAppServiceAuthentication** ve **IAppBuilder'ı**arayarak yapılandırmaya ekleyin. **UseAppServiceAuthentication uzantı** yöntemleri.
* [Microsoft.Azure.Mobile.Server.Notifications] Anında iletme bildirimlerini etkinleştirir ve anında iletme kaydı bitiş noktasını tanımlar. **AddPushBildirimlers** uzantı yöntemini çağırarak yapılandırmaya ekleyin.
* [Microsoft.Azure.Mobile.Server.CrossDomain,](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Mobil Uygulamanızdan eski web tarayıcılarına veri sunan bir denetleyici oluşturur. **MapLegacyCrossDomainController** uzantı yöntemini arayarak yapılandırmaya ekleyin.
* [Microsoft.Azure.Mobile.Server.Login,] özel kimlik doğrulama senaryoları sırasında kullanılan statik bir yöntem olan AppServiceLoginHandler.CreateToken() yöntemini sağlar.

## <a name="how-to-publish-the-server-project"></a><a name="publish-server-project"></a>Nasıl yapılır: Sunucu projesini yayımlama
Bu bölümde, .NET arka uç projenizi Visual Studio'dan nasıl yayınlayacağınızı gösterir. Ayrıca, arka uç projenizi [Git'i](../app-service/deploy-local-git.md) veya orada bulunan diğer yöntemlerden herhangi birini kullanarak da dağıtabilirsiniz.

1. Visual Studio'da, NuGet paketlerini geri yüklemek için projeyi yeniden oluşturabilirsiniz.
2. Çözüm Gezgini'nde projeyi sağ tıklatın, **Yayımla'yı**tıklatın. İlk yayımlama da bir yayımlama profili tanımlamanız gerekir. Zaten tanımlanmış bir profiliniz varsa, profili seçebilir ve **Yayımla'yı**tıklatın.
3. Yayımlama hedefi seçmeniz istenirse, Azure kimlik bilgilerinizle (gerekirse) Oturum Aç'ı microsoft **Azure App Service** > **Next'i**tıklatın.
   Visual Studio, yayımlama ayarlarınızı doğrudan Azure'dan indirir ve güvenli bir şekilde saklar.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. **Aboneliğinizi**seçin, **Görünüm'den** **Kaynak Türü'nü** seçin, **Mobil Uygulama'yı**genişletin ve Mobil Uygulama arka uçunuzu tıklatın, ardından **Tamam'ı**tıklatın.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Yayımlama profil bilgilerini doğrulayın ve **Yayımla'yı**tıklatın.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Mobil Uygulama arka sayfanız başarılı bir şekilde yayımlandığında, başarıyı gösteren bir açılış sayfası görürsünüz.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="how-to-define-a-table-controller"></a><a name="define-table-controller"></a>Nasıl yapılır: Tablo denetleyicisi tanımlan
SQL tablosunu mobil istemcilere maruz bırakmak için bir Tablo Denetleyicisi tanımlayın.  Tablo Denetleyicisi'ni yapılandırmak üç adım gerektirir:

1. Veri Aktarım Nesnesi (DTO) sınıfı oluşturun.
2. Mobil DbContext sınıfında bir tablo başvurusu yapılandırın.
3. Bir tablo denetleyicisi oluşturun.

Veri Aktarım Nesnesi (DTO), 'den devralan düz bir C# nesnesidir. `EntityData`  Örnek:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

DTO, SQL veritabanıiçindeki tabloyu tanımlamak için kullanılır.  Veritabanı girişini oluşturmak için, `DbSet<>` kullanmakta olduğunuz DbContext'a bir özellik ekleyin.  Azure Mobil Uygulamaları için varsayılan proje şablonunda `Models\MobileServiceContext.cs`DbContext şu şekilde adlandırılır:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Azure SDK yüklüyse, artık aşağıdaki gibi bir şablon tablo denetleyicisi oluşturabilirsiniz:

1. Denetleyiciler klasörüne sağ tıklayın ve > **Denetleyici Ekle'yi seçin...**. **Add**
2. Azure **Mobil Uygulamalar Tablo Denetleyicisi** seçeneğini seçin ve ardından **Ekle'yi**tıklatın.
3. Denetleyici **Ekle** iletişim kutusunda:
   * Model **sınıfı** açılır düşüşünde yeni DTO'nuzu seçin.
   * **DbContext** açılır düşüşünde, Mobil Hizmet DbContext sınıfını seçin.
   * Denetleyici adı sizin için oluşturuldu.
4. **Ekle**’ye tıklayın.

Quickstart sunucu projesi basit bir **TodoItemController**için bir örnek içerir.

### <a name="how-to-adjust-the-table-paging-size"></a><a name="adjust-pagesize"></a>Nasıl yapılsın: Tablo sayfalama boyutunu ayarlama
Varsayılan olarak, Azure Mobile Apps istek başına 50 kayıt döndürür.  Sayfalama, istemcinin kullanıcı arabirimi iş parçacığıveya sunucuyu çok uzun süre bağlamamasını sağlayarak iyi bir kullanıcı deneyimi sağlar. Tablo sayfalama boyutunu değiştirmek için, sunucu tarafı "izin verilen sorgu boyutu" ve istemci tarafı sayfa boyutunu artırmak `EnableQuery` sunucu tarafı "izin verilen sorgu boyutu" öznitelik kullanılarak ayarlanır:

    [EnableQuery(PageSize = 500)]

PageSize'ın istemci tarafından istenen boyuttan aynı veya daha büyük olduğundan emin olun.  İstemci sayfa boyutunu değiştirme yle ilgili ayrıntılar için belirli istemci HOWTO belgelerine bakın.

## <a name="how-to-define-a-custom-api-controller"></a>Nasıl yapılsın: Özel bir API denetleyicisi tanımlayın
Özel API denetleyicisi, bir bitiş noktası ortaya çıkararak Mobil Uygulama arka ucunuz için en temel işlevselliği sağlar. [MobileAppController] özelliğini kullanarak mobil cihazlara özel bir API denetleyicisi kaydedebilirsiniz. Öznitelik `MobileAppController` rotayı kaydeder, Mobil Uygulamalar JSON serializer'ı ayarlar ve [istemci sürüm denetimini](app-service-mobile-client-and-server-versioning.md)açar.

1. Visual Studio'da Denetleyiciler klasörüne sağ tıklayın, ardından**Denetleyici** **Ekle'yi** > tıklatın, **Web API 2 Controller&mdash;Boş'u** seçin ve **Ekle'yi**tıklatın.
2. Denetleyici **adı**,örneğin, `CustomController`ve **Ekle'yi**tıklatın.
3. Yeni denetleyici sınıfı dosyasında aşağıdaki leri kullanarak ifade yi ekleyin:

        using Microsoft.Azure.Mobile.Server.Config;
4. Aşağıdaki örnekte olduğu gibi API denetleyici sınıf tanımına **[MobileAppController]** atniteliğiuygulayın:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. App_Start/Startup.MobileApp.cs dosyasında, aşağıdaki örnekte olduğu gibi **MapApiControllers** uzantı yöntemine bir çağrı ekleyin:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Uzantı yöntemini `UseDefaultConfiguration()` de '' yerine `MapApiControllers()`kullanabilirsiniz. **MobileAppControllerAttribute** uygulanan olmayan herhangi bir denetleyici hala istemciler tarafından erişilebilir, ancak doğru herhangi bir Mobil App istemciSDK kullanarak istemciler tarafından tüketilen olmayabilir.

## <a name="how-to-work-with-authentication"></a>Nasıl yapılır: Kimlik doğrulamayla çalışma
Azure Mobile Apps, mobil arka uçunuzu güvence altına almak için Uygulama Hizmeti Kimlik Doğrulaması / Yetkilendirme'yi kullanır.  Bu bölümde, .NET arka uç sunucu projenizde aşağıdaki kimlik doğrulamayla ilgili görevlerin nasıl gerçekleştirilişiyi gösterir:

* [Nasıl yapilir: Sunucu projesine kimlik doğrulama ekleme](#add-auth)
* [Nasıl kullanılır: Uygulamanız için özel kimlik doğrulaması kullanın](#custom-auth)
* [Nasıl yapılsın: Kimlik doğrulaması kullanıcı bilgilerini alma](#user-info)
* [Nasıl kullanılır: Yetkili kullanıcılar için veri erişimini kısıtlama](#authorize)

### <a name="how-to-add-authentication-to-a-server-project"></a><a name="add-auth"></a>Nasıl yapilir: Sunucu projesine kimlik doğrulama ekleme
**MobileAppConfiguration** nesnesini genişleterek ve OWIN ara yazılımını yapılandırarak sunucu projenize kimlik doğrulama sı ekleyebilirsiniz. [Microsoft.Azure.Mobile.Server.Quickstart] paketini yüklediğinizde ve **UseDefaultConfiguration** uzantı yöntemini aradiğinizde, adım 3'e atlayabilirsiniz.

1. Visual Studio'da [Microsoft.Azure.Mobile.Server.Authentication paketini] yükleyin.
2. proje Startup.cs, **Yapılandırma** yönteminin başında aşağıdaki kod satırını ekleyin:

        app.UseAppServiceAuthentication(config);

    Bu OWIN ara yazılım bileşeni, ilişkili App Service ağ geçidi tarafından verilen belirteçleri doğrular.
3. `[Authorize]` Özniteliği kimlik doğrulaması gerektiren herhangi bir denetleyiciye veya yönteme ekleyin.

Mobil Uygulamalar arka uçunuzda istemcilerin kimliğini niçin doğruleyeceğinizi öğrenmek için [uygulamanıza kimlik doğrulama ekleme](app-service-mobile-ios-get-started-users.md)'ye bakın.

### <a name="how-to-use-custom-authentication-for-your-application"></a><a name="custom-auth"></a>Nasıl kullanılır: Uygulamanız için özel kimlik doğrulaması kullanın
> [!IMPORTANT]
> Özel kimlik doğrulamasını etkinleştirmek için, Azure portalında Uygulama Hizmetiniz için bir sağlayıcı seçmeden önce Uygulama Hizmeti Kimlik Doğrulaması'nı etkinleştirmeniz gerekir. Bu, barındırıldığında WEBSITE_AUTH_SIGNING_KEY ortam değişkenini etkinleştirecektir.
> 
> 
> Uygulama Hizmeti Kimlik Doğrulama/Yetkilendirme sağlayıcılarından birini kullanmak istemiyorsanız, kendi giriş sisteminizi uygulayabilirsiniz. Kimlik doğrulama belirteç oluşturmakonusunda yardımcı olmak için [Microsoft.Azure.Mobile.Server.Login] paketini yükleyin.  Kullanıcı kimlik bilgilerini doğrulamak için kendi kodunuzu sağlayın. Örneğin, bir veritabanında tuzlu ve hashed parolalara karşı denetleyebilirsiniz. Aşağıdaki örnekte, `isValidAssertion()` yöntem (başka bir yerde tanımlanmış) bu denetimlerden sorumludur.

Özel kimlik doğrulaması, bir ApiController oluşturarak ve teşhir `register` ve `login` eylemlerle ortaya çıkar. İstemci, bilgileri kullanıcıdan toplamak için özel bir kullanıcı aracı kullanmalıdır.  Bilgiler daha sonra standart bir HTTP POST çağrısıyla API'ye gönderilir. Sunucu iddiayı doğruladıktan sonra, `AppServiceLoginHandler.CreateToken()` yöntem kullanılarak bir belirteç verilir.  ApiController özniteliği **kullanmamalıdır.** `[MobileAppController]`

Örnek `login` bir eylem:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Önceki örnekte, LoginResult ve LoginResultUser gerekli özellikleri açığa serileştirilebilir nesnelerdir. İstemci, oturum açma yanıtlarının formun JSON nesneleri olarak döndürülmesini bekler:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Yöntem, `AppServiceLoginHandler.CreateToken()` bir *hedef kitle* ve *veren* parametre içerir. Bu parametrelerin her ikisi de HTTPS düzenini kullanarak uygulama kökünüzün URL'sine ayarlanır. Benzer *şekilde, uygulamanızın* imzalama anahtarının değeri olarak secretKey'i ayarlamanız gerekir. Anahtarlarını darp etmek ve kullanıcıları taklit etmek için kullanılabilen imza anahtarını istemciye dağıtmayın. *Web sitesi\_AUTH\_SIGNING\_KEY* ortam değişkenine başvurarak App Service'de barındırılan imza anahtarını elde edebilirsiniz. Yerel hata ayıklama bağlamında gerekirse, anahtarı almak ve uygulama ayarı olarak depolamak için [kimlik doğrulama bölümüyle Yerel hata ayıklama](#local-debug) yönergelerini izleyin.

Verilen belirteç, diğer talepleri ve son kullanma tarihini de içerebilir.  En az, verilen belirteç bir konu **(alt)** iddia içermelidir.

Kimlik doğrulama rotasını `loginAsync()` aşırı yükleyerek standart istemci yöntemini destekleyebilirsiniz.  İstemci `client.loginAsync('custom');` oturum açmak için ararsa, rotanız `/.auth/login/custom`.  Özel kimlik doğrulama denetleyicisinin rotasını `MapHttpRoute()`aşağıdakileri kullanarak ayarlayabilirsiniz:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Yaklaşımın `loginAsync()` kullanılması, kimlik doğrulama belirtecinin hizmete sonraki her çağrıya eklenmesini sağlar.
>
>

### <a name="how-to-retrieve-authenticated-user-information"></a><a name="user-info"></a>Nasıl yapılsın: Kimlik doğrulaması kullanıcı bilgilerini alma
Bir kullanıcının App Service tarafından kimliği doğrulandığında, atanan kullanıcı kimliğine ve diğer bilgilere .NET arka uç kodunuzdan erişebilirsiniz. Kullanıcı bilgileri arka uçta yetkilendirme kararları için kullanılabilir. Aşağıdaki kod, bir istekle ilişkili kullanıcı kimliğini alır:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

SID sağlayıcıya özgü kullanıcı kimliğinden türetilmiştir ve belirli bir kullanıcı ve oturum açma sağlayıcısı için statiktir.  SID geçersiz kimlik doğrulama belirteçleri için null.

Uygulama Hizmeti, oturum açma sağlayıcınızdan belirli taleplerde bulunmanızı da sağlar. Her kimlik sağlayıcısı, kimlik sağlayıcısı SDK'yı kullanarak daha fazla bilgi sağlayabilir.  Örneğin, arkadaş bilgileri için Facebook Graph API'yi kullanabilirsiniz.  Azure portalında sağlayıcı bıçak ta istenen talepleri belirtebilirsiniz. Bazı talepler, kimlik sağlayıcısıyla ek yapılandırma gerektirir.

Aşağıdaki kod, Facebook Graph API'ye karşı istekte bulunmak için gereken erişim belirteci içeren giriş kimlik bilgilerini almak için **GetAppServiceIdentityAsync** uzantısı yöntemini çağırır:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

`System.Security.Principal` **GetAppServiceIdentityAsync** uzantı yöntemini sağlamak için bir kullanım ifadesi ekleyin.

### <a name="how-to-restrict-data-access-for-authorized-users"></a><a name="authorize"></a>Nasıl kullanılır: Yetkili kullanıcılar için veri erişimini kısıtlama
Önceki bölümde, kimlik doğrulaması yapılan bir kullanıcının kullanıcı kimliğini nasıl alabileceğimizi gösterdik. Bu değere dayalı olarak verilere ve diğer kaynaklara erişimi kısıtlayabilirsiniz. Örneğin, tablolara userId sütunu eklemek ve sorgu sonuçlarını kullanıcı kimliğine göre filtrelemek, döndürülen verileri yalnızca yetkili kullanıcılarla sınırlamanın basit bir yoludur. Aşağıdaki kod, yalnızca SID TodoItem tablosundaki UserId sütunundaki değerle eşleştiğinde veri satırlarını döndürür:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

Yöntem, `Query()` filtreleme işlemek için LINQ tarafından manipüle edilebilir bir `IQueryable` döndürür.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Nasıl yapilir: Sunucu projesine anında iletme bildirimleri ekleme
**MobileAppConfiguration** nesnesini genişleterek ve bir Bildirim Hub'ları istemcisi oluşturarak sunucu projenize anında iletme bildirimleri ekleyin.

1. Visual Studio'da sunucu projesini sağ tıklatın ve **NuGet Paketlerini Yönet'i** `Microsoft.Azure.Mobile.Server.Notifications`tıklatın, ardından **Yükle'yi**tıklatın.
2. Bildirim Hub'ları `Microsoft.Azure.NotificationHubs` istemci kitaplığını içeren paketi yüklemek için bu adımı yineleyin.
3. App_Start/Startup.MobileApp.cs ve başlatma sırasında **AddPushBildirimler()** uzantısı yöntemine bir çağrı ekleyin:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. Bildirim Hub'ları istemcisi oluşturan aşağıdaki kodu ekleyin:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Artık bildirim hub istemcisini kullanarak kayıtlı aygıtlara anında iletme bildirimleri gönderebilirsiniz. Daha fazla bilgi için bkz. [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-ios-get-started-push.md). Bildirim Hub'ları hakkında daha fazla bilgi edinmek için [Bildirim Hub'larına Genel Bakış'a](../notification-hubs/notification-hubs-push-notification-overview.md)bakın.

## <a name="how-to-enable-targeted-push-using-tags"></a><a name="tags"></a>Nasıl Kullanılır: Etiketleri kullanarak hedefli itmeyi etkinleştirme
Bildirim Hub'ları, etiketleri kullanarak belirli kayıtlara hedefli bildirimler göndermenize olanak tanır. Birkaç etiket otomatik olarak oluşturulur:

* Yükleme Kimliği belirli bir aygıtı tanımlar.
* Kimlik doğrulaması SID'yi temel alan Kullanıcı Kimliği belirli bir kullanıcıyı tanımlar.

Yükleme kimliğine **MobileServiceClient'daki** **installationId** özelliğinden erişilebilir.  Aşağıdaki örnek, Bildirim Hub'larında belirli bir yüklemeye etiket eklemek için yükleme kimliğinin nasıl kullanılacağını gösterir:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Anında bildirim kaydı sırasında istemci tarafından sağlanan tüm etiketler, yükleme oluşturulurken arka uç tarafından yoksayılır. İstemcinin yüklemeye etiket eklemesini sağlamak için, önceki deseni kullanarak etiket ekleyen özel bir API oluşturmanız gerekir.

Uygulama Hizmeti Mobil Uygulamaları'nda [İstemci tarafından eklenen anında iletme bildirim etiketlerini][5] görün örnek olarak hızlı başlatma örneğini tamamlamıştır.

## <a name="how-to-send-push-notifications-to-an-authenticated-user"></a><a name="push-user"></a>Nasıl yapılsın: Kimlik doğrulaması yapılan bir kullanıcıya anında iletme bildirimleri gönderme
Kimlik doğrulaması yapılan bir kullanıcı anında iletme bildirimleri için kaydolduğunda, kullanıcı kimliği etiketi otomatik olarak kayda eklenir. Bu etiketi kullanarak, o kişi tarafından kayıtlı tüm aygıtlara anında iletme bildirimleri gönderebilirsiniz. Aşağıdaki kod, kullanıcının isteği yapan SID'sini alır ve bu kişi için her aygıt kaydına bir şablon anında iletme bildirimi gönderir:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Kimlik doğrulaması yapılan bir istemciden gelen anında iletme bildirimleri için kaydolurken, kayıt denemeden önce kimlik doğrulamanın tamamlandığından emin olun. Daha fazla bilgi için ,.NET arka uç için hızlı başlatma örneğini tamamlayan App Service Mobile Apps'taki [kullanıcılara itin'e][6] bakın.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Nasıl yapılacağını: Hata ayıklama ve .NET Server SDK sorun giderme
Azure Uygulama Hizmeti, ASP.NET uygulamalar için çeşitli hata ayıklama ve sorun giderme teknikleri sağlar:

* [Azure Uygulama Hizmetini İzleme](../app-service/web-sites-monitor.md)
* [Azure Uygulama Hizmetinde Tanısal Günlük'e Olanak Tanılama](../app-service/troubleshoot-diagnostic-logs.md)
* [Visual Studio'da Azure Uygulama Hizmeti sorun giderme](../app-service/troubleshoot-dotnet-visual-studio.md)

### <a name="logging"></a>Günlüğe Kaydetme
Standart ASP.NET izleme yazısını kullanarak Uygulama Hizmeti tanılama günlüklerine yazabilirsiniz. Günlüklere yazabilmeniz için Mobil Uygulama arka uçunuzda tanılamayı etkinleştirmeniz gerekir.

Tanılamayı etkinleştirmek ve günlüklere yazmak için:

1. [Uygulama günlüğe kaydetme (Windows)](../app-service/troubleshoot-diagnostic-logs.md#enable-application-logging-windows)adımlarını izleyin.
2. Kod dosyanıza aşağıdaki ifadesini ekleyin:

        using System.Web.Http.Tracing;
3. .NET arka ucundan tanı günlüklerine yazmak için aşağıdaki gibi bir izleme yazarı oluşturun:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. Sunucu projenizi yeniden yayımlayın ve günlük le kod yolunu yürütmek için Mobil Uygulama arka ucuna erişin.
5. [Access günlük dosyalarında](../app-service/troubleshoot-diagnostic-logs.md#access-log-files)açıklandığı gibi günlükleri indirin ve değerlendirin.

### <a name="local-debugging-with-authentication"></a><a name="local-debug"></a>Kimlik doğrulama ile yerel hata ayıklama
Değişiklikleri bulutta yayımlamadan önce test etmek için uygulamanızı yerel olarak çalıştırabilirsiniz. Azure Mobil Uygulamaların çoğu arka uçlarında Visual Studio'da yken *F5* tuşuna basın. Ancak, kimlik doğrulaması kullanırken bazı ek hususlar vardır.

Uygulama Hizmeti Kimlik Doğrulaması/Yetkilendirme yapılandırılmış bulut tabanlı bir mobil uygulamanız olmalıdır ve istemcinizin alternatif giriş ana bilgisayarı olarak bulut bitiş noktası belirtilmelidir. Gerekli belirli adımlar için istemci platformuiçin belgelere bakın.

Mobil arka uçunuzda [Microsoft.Azure.Mobile.Server.Authentication] yüklü olduğundan emin olun. Daha sonra, uygulamanızın OWIN başlangıç sınıfında, aşağıdakileri ekleyin, `MobileAppConfiguration` `HttpConfiguration`sonra sizin için uygulandı:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Önceki örnekte, HTTPS düzenini kullanarak Web.config dosyanızdaki *authAudience* ve *authIssuer* uygulama ayarlarını uygulama kökünüzün URL'si olacak şekilde yapılandırmanız gerekir. Benzer *şekilde, authSigningKey'i* uygulamanızın imzalama anahtarının değeri olarak ayarlamanız gerekir.
İmza anahtarını almak için:

1. [Azure portalında] uygulamanıza gidin
2. **Araçlar**, **Kudu**, **Git**tıklatın .
3. Kudu Management sitesinde **Çevre'yi**tıklatın.
4. *WEB SİTESİ\_\_AUTH\_İmZA ANAHTARI*için değeri bulun.

Yerel uygulama config *authSigningKey* parametresi için imza anahtarını kullanın.  Mobil arka ucunuz artık yerel olarak çalışırken belirteçleri doğrulayan ve istemcinin bulut tabanlı bitiş noktasından belirteci elde ettiği donanıma sahip.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portalında]: https://portal.azure.com
[NuGet.org]: https://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Giriş]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Bildirimler]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
