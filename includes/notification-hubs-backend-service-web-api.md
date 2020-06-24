---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: f0e83548d3ba3b353b471e2e3429a23421aec7b2
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081545"
---
### <a name="create-a-web-project"></a>Web projesi oluşturma

1. **Visual Studio**'da **Dosya**  >  **yeni çözüm**' ü seçin.

1. Bir **sonraki .NET Core**  >  **App**  >  **ASP.NET Core**  >  **API 'sini**seçin  >  **Next**.
  
1. **Yeni ASP.NET Core Web API 'Nizi yapılandırın** iletişim kutusunda, **.NET Core 3,1** **hedef çatısı** ' nı seçin.

1. **Proje adı** Için *pushdemoapı* girip **Oluştur**' u seçin.

1. Şablonlu uygulamayı test etmek için hata ayıklamayı başlatın (**komut**  +  **girin**).

    > [!NOTE]
    > Şablonlu uygulama, *Launchurl 'si*olarak **dalgalı bir denetleyiciyi** kullanacak şekilde yapılandırılmıştır. Bu, **Properties**  >  ** üzerindelaunchSettings.js**Özellikler ' de ayarlanır.  
    >
    > **Geçersiz bir geliştirme sertifikası bulundu** iletisi sorulursa:
    >
    > 1. Bu hatayı giderecek ' DotNet dev-CERT https ' aracını çalıştırmayı kabul etmek için **Evet** ' e tıklayın. ' DotNet dev-CERT https ' aracı daha sonra sertifika için bir parola ve anahtarlarınızın parolasını girmenizi ister.
    >
    > 1. **Yeni sertifikaya yüklenip güvenmesini**isteyip Istemediğiniz sorulduğunda **Evet** ' e tıklayın, ardından anahtarınıza ait parolayı girin.

1. **Controllers** klasörünü genişletin ve **WeatherForecastController.cs**silin.

1. **WeatherForecast.cs**silin.

1. **Denetim**  +  **Pushdemoapi** projesine tıklayın ve sonra **Ekle** menüsünde **yeni dosya..** . **öğesini** seçin.

1. [Gizli yönetici aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-3.1&tabs=linux#secret-manager)kullanarak yerel yapılandırma değerlerini ayarlayın. Gizli dizileri çözümden ayırarak kaynak denetiminde bitmemesini sağlar. **Terminal** ' i açın ve ardından proje dosyasının dizinine gidin ve aşağıdaki komutları çalıştırın:

    ```bash
    dotnet user-secrets init
    dotnet user-secrets set "NotificationHub:Name" <value>
    dotnet user-secrets set "NotificationHub:ConnectionString" <value>
    ```

    Yer tutucu değerlerini kendi Bildirim Hub 'ı adı ve bağlantı dizesi değerlerinizle değiştirin. [Bildirim Hub 'ı oluşturma](#create-a-notification-hub) bölümünde bunları bir yere göz önünde yaptınız. Aksi takdirde, bunları [Azure](https://portal.azure.com)'da bulabilirsiniz.

    **Notificationshub: ad**:  
    **Genel bakış**' ın en üstündeki **Essentials** özetinde *adı* inceleyin.  

    **Notificationhub: ConnectionString**:  
    **Erişim Ilkelerinde** *Defaultfullsharedaccesssignature* öğesine bakın

    > [!NOTE]
    > Üretim senaryolarında, bağlantı dizesini güvenli bir şekilde depolamak için [Azure Keykasası](https://azure.microsoft.com/services/key-vault) gibi seçeneklere bakabilirsiniz. Basitlik sağlamak için gizli dizileri [Azure App Service](https://azure.microsoft.com/services/app-service/) uygulama ayarlarına eklenecektir.

### <a name="authenticate-clients-using-an-api-key-optional"></a>API anahtarı kullanarak istemcilerin kimliğini doğrulama (Isteğe bağlı)

API anahtarları, belirteçler olarak güvenli değildir, ancak Bu öğreticinin amaçları doğrultusunda yeterli olacaktır. Bir API anahtarı, [ASP.net ara yazılımı](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/?view=aspnetcore-3.1)aracılığıyla kolayca yapılandırılabilir.

1. **API anahtarını** yerel yapılandırma değerlerine ekleyin.

    ```bash
    dotnet user-secrets set "Authentication:ApiKey" <value>
    ```

    > [!NOTE]
    > Yer tutucu değerini kendi ile değiştirmeniz ve bunu bir yere göz önünde geçirmeniz gerekir.

1. **Denetim**  +  **Pushdemoapi** projesi **' ne tıklayın** , **Ekle** menüsünde **Yeni klasör** ' i seçin ve ardından **klasör adı**olarak *kimlik doğrulaması* kullanarak **Ekle** ' ye tıklayın.

1. **Denetim**  +  **Kimlik doğrulama** klasörü ' ne ve ardından **Ekle** menüsünde **yeni dosya..** . **' ı** seçin.

1. **Genel**  >  **boş sınıf**' ı seçin, **ad**için *ApiKeyAuthOptions.cs* girin ve ardından aşağıdaki uygulamayı eklemek için **Yeni** ' ye tıklayın.

    ```csharp
    using Microsoft.AspNetCore.Authentication;

    namespace PushDemoApi.Authentication
    {
        public class ApiKeyAuthOptions : AuthenticationSchemeOptions
        {
            public const string DefaultScheme = "ApiKey";
            public string Scheme => DefaultScheme;
            public string ApiKey { get; set; }
        }
    }
    ```

1. *ApiKeyAuthHandler.cs*adlı **kimlik doğrulama** klasörüne başka bir **boş sınıf** ekleyin ve ardından aşağıdaki uygulamayı ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Security.Claims;
    using System.Text.Encodings.Web;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Authentication;
    using Microsoft.Extensions.Logging;
    using Microsoft.Extensions.Options;

    namespace PushDemoApi.Authentication
    {
        public class ApiKeyAuthHandler : AuthenticationHandler<ApiKeyAuthOptions>
        {
            const string ApiKeyIdentifier = "apikey";

            public ApiKeyAuthHandler(
                IOptionsMonitor<ApiKeyAuthOptions> options,
                ILoggerFactory logger,
                UrlEncoder encoder,
                ISystemClock clock)
                : base(options, logger, encoder, clock) {}

            protected override Task<AuthenticateResult> HandleAuthenticateAsync()
            {
                string key = string.Empty;

                if (Request.Headers[ApiKeyIdentifier].Any())
                {
                    key = Request.Headers[ApiKeyIdentifier].FirstOrDefault();
                }
                else if (Request.Query.ContainsKey(ApiKeyIdentifier))
                {
                    if (Request.Query.TryGetValue(ApiKeyIdentifier, out var queryKey))
                        key = queryKey;
                }

                if (string.IsNullOrWhiteSpace(key))
                    return Task.FromResult(AuthenticateResult.Fail("No api key provided"));

                if (!string.Equals(key, Options.ApiKey, StringComparison.Ordinal))
                    return Task.FromResult(AuthenticateResult.Fail("Invalid api key."));

                var identities = new List<ClaimsIdentity> {
                    new ClaimsIdentity("ApiKeyIdentity")
                };

                var ticket = new AuthenticationTicket(
                    new ClaimsPrincipal(identities), Options.Scheme);

                return Task.FromResult(AuthenticateResult.Success(ticket));
            }
        }
    }
    ```

    > [!NOTE]
    > Bir [kimlik doğrulama işleyicisi](https://docs.microsoft.com/aspnet/core/security/authentication/?view=aspnetcore-3.1#authentication-handler) , bu durumda özel bir API anahtar şeması olan bir düzenin davranışını uygulayan bir türdür.

1. *ApiKeyAuthenticationBuilderExtensions.cs*adlı **kimlik doğrulama** klasörüne başka bir **boş sınıf** ekleyin ve ardından aşağıdaki uygulamayı ekleyin.

    ```csharp
    using System;
    using Microsoft.AspNetCore.Authentication;

    namespace PushDemoApi.Authentication
    {
        public static class AuthenticationBuilderExtensions
        {
            public static AuthenticationBuilder AddApiKeyAuth(
                this AuthenticationBuilder builder,
                Action<ApiKeyAuthOptions> configureOptions)
            {
                return builder
                    .AddScheme<ApiKeyAuthOptions, ApiKeyAuthHandler>(
                        ApiKeyAuthOptions.DefaultScheme,
                        configureOptions);
            }
        }
    }
    ```

    > [!NOTE]
    > Bu genişletme yöntemi, **Startup.cs** ' deki ara yazılım yapılandırma kodunu daha okunaklı ve genellikle daha kolay hale getirmek için basitleştirir.

1. **Startup.cs**' de, hizmetler ÇAĞRıSıNıN altındaki API anahtarı kimlik doğrulamasını yapılandırmak Için **ConfigureServices** yöntemini güncelleştirin **. AddControllers** yöntemi.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers();

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = ApiKeyAuthOptions.DefaultScheme;
            options.DefaultChallengeScheme = ApiKeyAuthOptions.DefaultScheme;
        }).AddApiKeyAuth(Configuration.GetSection("Authentication").Bind);
    }
    ```

1. Hala **Startup.cs**içinde, uygulamanın **IApplicationBuilder**üzerinde **UseAuthentication** ve **useauthorization** uzantı yöntemlerini çağırmak için **Configure** metodunu güncelleştirin. Bu **yöntemlerin, uygulamadan önce ve uygulamadan** önce çağrıldığından emin olun **. UseEndpoints**.

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseHttpsRedirection();

        app.UseRouting();

        app.UseAuthentication();

        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllers();
        });
    }
    ```

    > [!NOTE]
    > **UseAuthentication** çağrısı, önceden kayıtlı kimlik doğrulama düzenlerini ( **ConfigureServices**) kullanan ara yazılımı kaydeder. Bu, kimlik doğrulamasından geçen kullanıcılara bağlı olan herhangi bir ara yazılım önce çağrılmalıdır.

### <a name="add-dependencies-and-configure-services"></a>Bağımlılıkları ekleme ve hizmetleri yapılandırma

ASP.NET Core, sınıflar ve bunların bağımlılıkları arasında [denetimin INVERSION (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir teknik olan [bağımlılık ekleme (dı)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-3.1) yazılım tasarım modelini destekler.  

Bildirim Hub 'ının kullanımı ve [arka uç işlemleri için NOTIFICATION HUBS SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) , bir hizmet içinde kapsüllenir. Hizmet kaydedilir ve uygun bir soyutlama aracılığıyla kullanılabilir hale getirilir.

1. **Denetim**  +  **Bağımlılıklar** klasörüne **tıklayın** ve ardından **NuGet Paketlerini Yönet...** seçeneğini belirleyin.

1. **Microsoft. Azure. Notificationhub 'ları** arayın ve bunun işaretli olduğundan emin olun.

1. **Paket Ekle**' ye tıklayın ve lisans koşullarını kabul etmek isteyip Istemediğiniz sorulduğunda **kabul et** ' e tıklayın.

1. **Denetim**  +  **Pushdemoapi** projesi **' ne tıklayın** , **Ekle** menüsünde **Yeni klasör** ' i seçin ve ardından **klasör adı**olarak *modeller* kullanarak **Ekle** ' ye tıklayın.

1. **Denetim**  +  **Modeller** klasörüne **tıklayın** ve ardından **Ekle** menüsünde **yeni dosya..** . öğesini seçin.

1. **Genel**  >  **boş sınıf**' ı seçin, **ad**için *PushTemplates.cs* girin ve ardından aşağıdaki uygulamayı eklemek için **Yeni** ' ye tıklayın.

    ```csharp
    namespace PushDemoApi.Models
    {
        public class PushTemplates
        {
            public class Generic
            {
                public const string Android = "{ \"notification\": { \"title\" : \"PushDemo\", \"body\" : \"$(alertMessage)\"}, \"data\" : { \"action\" : \"$(alertAction)\" } }";
                public const string iOS = "{ \"aps\" : {\"alert\" : \"$(alertMessage)\"}, \"action\" : \"$(alertAction)\" }";
            }

            public class Silent
            {
                public const string Android = "{ \"data\" : {\"message\" : \"$(alertMessage)\", \"action\" : \"$(alertAction)\"} }";
                public const string iOS = "{ \"aps\" : {\"content-available\" : 1, \"apns-priority\": 5, \"sound\" : \"\", \"badge\" : 0}, \"message\" : \"$(alertMessage)\", \"action\" : \"$(alertAction)\" }";
            }
        }
    }
    ```

    > [!NOTE]
    > Bu sınıf, bu senaryo için gerekli olan genel ve sessiz bildirimler için simgeleştirilmiş bildirim yüklerini içerir. Yükleri, hizmet aracılığıyla mevcut yüklemeleri güncelleştirmek zorunda kalmadan, deneme izin vermek için [yükleme](https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.installation?view=azure-dotnet) dışında tanımlanır. Bu şekilde yüklemelerin değişikliklerinin işlenmesi Bu öğreticide kapsam dışındadır. Üretim için [özel şablonları](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages)göz önünde bulundurun.

1. **Genel**  >  **boş sınıf**' ı seçin, **ad**için *DeviceInstallation.cs* girin ve ardından aşağıdaki uygulamayı eklemek için **Yeni** ' ye tıklayın.

    ```csharp
    using System.Collections.Generic;
    using System.ComponentModel.DataAnnotations;

    namespace PushDemoApi.Models
    {
        public class DeviceInstallation
        {
            [Required]
            public string InstallationId { get; set; }

            [Required]
            public string Platform { get; set; }

            [Required]
            public string PushChannel { get; set; }

            public IList<string> Tags { get; set; } = Array.Empty<string>();
        }
    }
    ```

1. *NotificationRequest.cs*adlı **modeller** klasörüne başka bir **boş sınıf** ekleyin ve ardından aşağıdaki uygulamayı ekleyin.

    ```csharp
    using System;

    namespace PushDemoApi.Models
    {
        public class NotificationRequest
        {
            public string Text { get; set; }
            public string Action { get; set; }
            public string[] Tags { get; set; } = Array.Empty<string>();
            public bool Silent { get; set; }
        }
    }
    ```

1. *NotificationHubOptions.cs*adlı **modeller** klasörüne başka bir **boş sınıf** ekleyin ve ardından aşağıdaki uygulamayı ekleyin.

    ```csharp
    using System.ComponentModel.DataAnnotations;

    namespace PushDemoApi.Models
    {
        public class NotificationHubOptions
        {
            [Required]
            public string Name { get; set; }

            [Required]
            public string ConnectionString { get; set; }
        }
    }
    ```

1. *Hizmet*adlı **pushdemoapı** projesine yeni bir klasör ekleyin.

1. *INotificationService.cs*adlı **Hizmetler** klasörüne **boş bir arabirim** ekleyin ve ardından aşağıdaki uygulamayı ekleyin.

    ```csharp
    using System.Threading.Tasks;
    using PushDemoApi.Models;

    namespace PushDemoApi.Services
    {
        public interface INotificationService
        {
            Task<bool> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation, CancellationToken token);
            Task<bool> DeleteInstallationByIdAsync(string installationId, CancellationToken token);
            Task<bool> RequestNotificationAsync(NotificationRequest notificationRequest, CancellationToken token);
        }
    }
    ```

1. *NotificationHubsService.cs*adlı **Hizmetler** klasörüne **boş bir sınıf** ekleyin ve ardından **ınocertificate ationservice** arabirimini uygulamak için aşağıdaki kodu ekleyin:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Extensions.Logging;
    using Microsoft.Extensions.Options;
    using PushDemoApi.Models;

    namespace PushDemoApi.Services
    {
        public class NotificationHubService : INotificationService
        {
            readonly NotificationHubClient _hub;
            readonly Dictionary<string, NotificationPlatform> _installationPlatform;
            readonly ILogger<NotificationHubService> _logger;

            public NotificationHubService(IOptions<NotificationHubOptions> options, ILogger<NotificationHubService> logger)
            {
                _logger = logger;
                _hub = NotificationHubClient.CreateClientFromConnectionString(
                    options.Value.ConnectionString,
                    options.Value.Name);

                _installationPlatform = new Dictionary<string, NotificationPlatform>
                {
                    { nameof(NotificationPlatform.Apns).ToLower(), NotificationPlatform.Apns },
                    { nameof(NotificationPlatform.Fcm).ToLower(), NotificationPlatform.Fcm }
                };
            }

            public async Task<bool> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation, CancellationToken token)
            {
                if (string.IsNullOrWhiteSpace(deviceInstallation?.InstallationId) ||
                    string.IsNullOrWhiteSpace(deviceInstallation?.Platform) ||
                    string.IsNullOrWhiteSpace(deviceInstallation?.PushChannel))
                    return false;

                var installation = new Installation()
                {
                    InstallationId = deviceInstallation.InstallationId,
                    PushChannel = deviceInstallation.PushChannel,
                    Tags = deviceInstallation.Tags
                };

                if (_installationPlatform.TryGetValue(deviceInstallation.Platform, out var platform))
                    installation.Platform = platform;
                else
                    return false;

                try
                {
                    await _hub.CreateOrUpdateInstallationAsync(installation, token);
                }
                catch
                {
                    return false;
                }

                return true;
            }

            public async Task<bool> DeleteInstallationByIdAsync(string installationId, CancellationToken token)
            {
                if (string.IsNullOrWhiteSpace(installationId))
                    return false;

                try
                {
                    await _hub.DeleteInstallationAsync(installationId, token);
                }
                catch
                {
                    return false;
                }

                return true;
            }

            public async Task<bool> RequestNotificationAsync(NotificationRequest notificationRequest, CancellationToken token)
            {
                if ((notificationRequest.Silent &&
                    string.IsNullOrWhiteSpace(notificationRequest?.Action)) ||
                    (!notificationRequest.Silent &&
                    (string.IsNullOrWhiteSpace(notificationRequest?.Text)) ||
                    string.IsNullOrWhiteSpace(notificationRequest?.Action)))
                    return false;

                var androidPushTemplate = notificationRequest.Silent ?
                    PushTemplates.Silent.Android :
                    PushTemplates.Generic.Android;

                var iOSPushTemplate = notificationRequest.Silent ?
                    PushTemplates.Silent.iOS :
                    PushTemplates.Generic.iOS;

                var androidPayload = PrepareNotificationPayload(
                    androidPushTemplate,
                    notificationRequest.Text,
                    notificationRequest.Action);

                var iOSPayload = PrepareNotificationPayload(
                    iOSPushTemplate,
                    notificationRequest.Text,
                    notificationRequest.Action);

                try
                {
                    if (notificationRequest.Tags.Length == 0)
                    {
                        // This will broadcast to all users registered in the notification hub
                        await SendPlatformNotificationsAsync(androidPayload, iOSPayload, token);
                    }
                    else if (notificationRequest.Tags.Length <= 20)
                    {
                        await SendPlatformNotificationsAsync(androidPayload, iOSPayload, notificationRequest.Tags, token);
                    }
                    else
                    {
                        var notificationTasks = notificationRequest.Tags
                            .Select((value, index) => (value, index))
                            .GroupBy(g => g.index / 20, i => i.value)
                            .Select(tags => SendPlatformNotificationsAsync(androidPayload, iOSPayload, tags, token));

                        await Task.WhenAll(notificationTasks);
                    }

                    return true;
                }
                catch (Exception e)
                {
                    _logger.LogError(e, "Unexpected error sending notification");
                    return false;
                }
            }

            string PrepareNotificationPayload(string template, string text, string action) => template
                .Replace("$(alertMessage)", text, StringComparison.InvariantCulture)
                .Replace("$(alertAction)", action, StringComparison.InvariantCulture);

            Task SendPlatformNotificationsAsync(string androidPayload, string iOSPayload, CancellationToken token)
            {
                var sendTasks = new Task[]
                {
                    _hub.SendFcmNativeNotificationAsync(androidPayload, token),
                    _hub.SendAppleNativeNotificationAsync(iOSPayload, token)
                };

                return Task.WhenAll(sendTasks);
            }

            Task SendPlatformNotificationsAsync(string androidPayload, string iOSPayload, IEnumerable<string> tags, CancellationToken token)
            {
                var sendTasks = new Task[]
                {
                    _hub.SendFcmNativeNotificationAsync(androidPayload, tags, token),
                    _hub.SendAppleNativeNotificationAsync(iOSPayload, tags, token)
                };

                return Task.WhenAll(sendTasks);
            }
        }
    }
    ```

    > [!NOTE]
    > **Sendtemplatenocertificate Ationasync** için belirtilen etiket ifadesi 20 etiketle sınırlıdır. Çoğu operatör için 6 ile sınırlıdır ancak ifade bu durumda yalnızca ORs (| |) içerir. İstekte 20 ' den fazla etiket varsa, bunların birden çok isteğe bölünmesi gerekir. Daha fazla ayrıntı için [Yönlendirme ve etiket ifadeleri](https://msdn.microsoft.com/library/azure/Dn530749.aspx?f=255&MSPPError=-2147217396) belgelerine bakın.

1. **Startup.cs**' de, **ConfigureServices** yöntemini, **Notificationhubsservice** 'i **ınocertificate**için tek bir uygulama olarak eklemek üzere güncelleştirin.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ...

        services.AddSingleton<INotificationService, NotificationHubService>();

        services.AddOptions<NotificationHubOptions>()
            .Configure(Configuration.GetSection("NotificationHub").Bind)
            .ValidateDataAnnotations();
    }
    ```

### <a name="create-the-notifications-api"></a>Bildirimler API 'sini oluşturma

1. **Denetim**  +  **Denetleyiciler** klasörüne **tıklayın** ve ardından **Ekle** menüsünde **yeni dosya..** . öğesini seçin.

1. **ASP.NET Core**  >  **Web API denetleyici sınıfı**' nı seçin, **ad**için *notificationscontroller* yazın ve ardından **Yeni**' ye tıklayın.

1. Aşağıdaki ad alanlarını dosyanın en üstüne ekleyin.

    ```csharp
    using System.ComponentModel.DataAnnotations;
    using System.Net;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Mvc;
    using PushDemoApi.Models;
    using PushDemoApi.Services;
    ```

1. Şablonlu denetleyiciyi **ControllerBase** 'den türeten ve **Apicontroller** özniteliğiyle donatılmış şekilde güncelleştirin.

    ```cs
    [ApiController]
    [Route("api/[controller]")]
    public class NotificationsController : ControllerBase
    {
        // Templated methods here
    }
    ```

    > [!NOTE]
    > **Denetleyici** temel sınıfı, görünümler için destek desteği sağlar, ancak bu durumda bu gerekli değildir, bunun yerine **ControllerBase** kullanılabilir.

1. [API anahtarı kullanarak Istemci kimlik doğrulaması](#authenticate-clients-using-an-api-key-optional) ' nı tamamlamayı seçtiyseniz, **notificationscontroller** 'ı **Yetkilendirme** özniteliğiyle birlikte tasarlamanız gerekir.

    ```cs
    [Authorize]
    ```

1. Oluşturucuyu, bir bağımsız değişken olarak kayıtlı **ınocertificate/Service** örneğini kabul edecek şekilde güncelleştirin ve salt okunur bir üyeye atayın.

    ```cs
    readonly INotificationService _notificationService;

    public NotificationsController(INotificationService notificationService)
    {
        _notificationService = notificationService;
    }
    ```

1. **launchSettings.json** ( **Özellikler** klasörü) içinde, **Launchurl** 'yi, `weatherforecast` **registrationscontroller** **route** özniteliğinde belirtilen URL ile eşleşecek şekilde *API/bildirimleri* olarak değiştirin.

1. **Command**  +  Uygulamanın yeni **notificationscontroller** ile çalıştığını doğrulamak ve **401 Yetkisiz** bir durum döndürdüğünden hata ayıklamayı başlatın (komut**girin**).

    > [!NOTE]
    > Visual Studio, uygulamayı tarayıcıda otomatik olarak başlatamamayabilir. Bu noktadan itibaren API 'yi test etmek için [Postman](https://www.postman.com/downloads) kullanacaksınız.

1. Yeni bir **[Postman](https://www.postman.com/downloads)** sekmesinde aşağıdaki adresi **Al** ve gir ' i ayarlayın.

    ```bash
    https://localhost:5001/api/notifications
    ```

    > [!NOTE]
    > Localhost adresilaunchSettings.js**özelliklerinde** bulunan **ApplicationUrl** değeriyle eşleşmelidir  >  ** **. Varsayılan değer, `https://localhost:5001;http://localhost:5000` bir 404 yanıtı alırsanız doğrulamak için bir şeydir.

1. [API anahtarı kullanarak Istemci kimlik doğrulaması](#authenticate-clients-using-an-api-key-optional) ' nı tamamlamayı seçtiyseniz, istek üst bilgilerini **apikey** değerini içerecek şekilde yapılandırmayı unutmayın.

   | Anahtar                            | Değer                          |
   | ------------------------------ | ------------------------------ |
   | apikey                         | <your_api_key>                 |

1. **Gönder** düğmesine tıklayın.

    > [!NOTE]
    > Bazı **JSON** içeriğiyle bir **200 Tamam** durumu almalısınız.
    >
    > **SSL sertifikası doğrulama** uyarısı alırsanız, **Ayarlar**' da SSL sertifikası doğrulama **[Postman](https://www.postman.com/downloads)** ayarını devre dışı bırakabilirsiniz.

1. Şablonlu sınıf yöntemlerini aşağıdaki kodla değiştirin.

    ```csharp
    [HttpPut]
    [Route("installations")]
    [ProducesResponseType((int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    [ProducesResponseType((int)HttpStatusCode.UnprocessableEntity)]
    public async Task<IActionResult> UpdateInstallation(
        [Required]DeviceInstallation deviceInstallation)
    {
        var success = await _notificationService
            .CreateOrUpdateInstallationAsync(deviceInstallation, HttpContext.RequestAborted);

        if (!success)
            return new UnprocessableEntityResult();

        return new OkResult();
    }

    [HttpDelete()]
    [Route("installations/{installationId}")]
    [ProducesResponseType((int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    [ProducesResponseType((int)HttpStatusCode.UnprocessableEntity)]
    public async Task<ActionResult> DeleteInstallation(
        [Required][FromRoute]string installationId)
    {
        var success = await _notificationService
            .DeleteInstallationByIdAsync(installationId, CancellationToken.None);

        if (!success)
            return new UnprocessableEntityResult();

        return new OkResult();
    }

    [HttpPost]
    [Route("requests")]
    [ProducesResponseType((int)HttpStatusCode.OK)]
    [ProducesResponseType((int)HttpStatusCode.BadRequest)]
    [ProducesResponseType((int)HttpStatusCode.UnprocessableEntity)]
    public async Task<IActionResult> RequestPush(
        [Required]NotificationRequest notificationRequest)
    {
        if ((notificationRequest.Silent &&
            string.IsNullOrWhiteSpace(notificationRequest?.Action)) ||
            (!notificationRequest.Silent &&
            string.IsNullOrWhiteSpace(notificationRequest?.Text)))
            return new BadRequestResult();

        var success = await _notificationService
            .RequestNotificationAsync(notificationRequest, HttpContext.RequestAborted);

        if (!success)
            return new UnprocessableEntityResult();

        return new OkResult();
    }
    ```

### <a name="create-the-api-app"></a>API uygulaması oluşturma

Artık arka uç hizmetini barındırmak için [Azure App Service](https://docs.microsoft.com/azure/app-service/) bir [API uygulaması](https://azure.microsoft.com/services/app-service/api/) oluşturursunuz.  

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Kaynak oluştur ' a**tıklayın, sonra **API uygulamasını**arayıp seçin ve ardından **Oluştur**' a tıklayın.

1. Aşağıdaki alanları güncelleştirin ve ardından **Oluştur**' a tıklayın.

    **Uygulama adı:**  
    **API uygulaması** için genel olarak benzersiz bir ad girin

    **Aboneliğiniz**  
    İçinde Bildirim Hub 'ını oluşturduğunuz hedef **aboneliğin** aynısını seçin.

    **Kaynak grubu:**  
    ' Da Bildirim Hub 'ını oluşturduğunuz **kaynak grubunu** seçin.

    **Plan/konum App Service:**  
    Yeni bir **App Service planı** oluşturma  

    > [!NOTE]
    > Varsayılan seçenekten **SSL** desteği içeren bir plana geçin. Aksi takdirde, **http** isteklerinin engellenmesini engellemek için Mobil uygulamayla çalışırken ilgili adımları uygulamanız gerekir.

    **Application Insights:**  
    Önerilen seçeneği (Bu adı kullanarak yeni bir kaynak oluşturulacaktır) veya var olan bir kaynağı seçmeniz gerekir.

1. **API uygulaması** sağlandığında, bu kaynağa gidin.

1. **Genel bakışın**en üstündeki **temel bilgiler** özetinde **URL** özelliğini unutmayın. Bu URL, Bu öğreticinin ilerleyen kısımlarında kullanılacak olan *arka uç uç* noktanıza ait olacaktır.

    > [!NOTE]
    > URL, daha önce belirttiğiniz API uygulaması adını biçimiyle kullanır `https://<app_name>.azurewebsites.net` .

1. Listeden **yapılandırma** ( **Ayarlar**altında) seçeneğini belirleyin.  

1. Aşağıdaki ayarların her biri için, **Yeni uygulama ayarı** ' na tıklayarak **adı** ve **değeri**girin ve ardından **Tamam**' a tıklayın.

   | Name                               | Değer                          |
   | ---------------------------------- | ------------------------------ |
   | `Authentication:ApiKey`            | <api_key_value>                |
   | `NotificationHub:Name`             | <hub_name_value>               |
   | `NotificationHub:ConnectionString` | <hub_connection_string_value>  |

   > [!NOTE]
   > Bunlar, daha önce Kullanıcı ayarlarında tanımladığınız ayarlardır. Bunları kopyalayabilmelisiniz. **Kimlik doğrulama: ApiKey** AYARı yalnızca [API anahtarı kullanarak kimlik doğrulama istemcilerinin](#authenticate-clients-using-an-api-key-optional) tamamlanmasını tercih ediyorsanız gereklidir. Üretim senaryolarında [Azure Keykasası](https://azure.microsoft.com/services/key-vault)gibi seçeneklere bakabilirsiniz. Bunlar, bu durumda basitlik için uygulama ayarları olarak eklenmiştir.

1. Tüm uygulama ayarları eklendikten sonra **Kaydet**' e tıklayın, sonra **devam edin**.

### <a name="publish-the-backend-service"></a>Arka uç hizmetini yayımlama

Ardından, uygulamayı tüm cihazlardan erişilebilir hale getirmek için API uygulamasına dağıtırsınız.  

1. Daha önce yapmadıysanız, yapılandırmanızı **hata ayıklamadan** **Yayınla** değiştirin.

1. **Denetim**  +  **Pushdemoapi** projesi **' ne tıklayın** ve ardından **Yayımla** menüsünde **Azure 'a Yayımla...** öğesini seçin.

1. İstenirse, kimlik doğrulama akışını izleyin. Önceki [API uygulaması oluşturma](#create-the-api-app) bölümünde kullandığınız hesabı kullanın.

1. Daha önce oluşturduğunuz **Azure App SERVICE API uygulamasını** listeden Yayımla hedefi olarak seçin ve ardından **Yayımla**' ya tıklayın.

Sihirbazı tamamladıktan sonra, uygulamayı Azure 'da yayımlar ve ardından uygulamayı açar. Daha önce yapmadıysanız **URL 'yi** bir yere unutmayın. Bu URL, Bu öğreticinin ilerleyen kısımlarında kullanılan *arka uç uç noktasıdır* .

### <a name="validating-the-published-api"></a>Yayınlanan API doğrulanıyor

1. **[Postman](https://www.postman.com/downloads)** 'da yeni bir sekme açın, isteği **gönderi** olarak ayarlayın ve aşağıdaki adresi girin. Yer tutucusunu, önceki [arka uç hizmetini yayımlama](#publish-the-backend-service) bölümünde yaptığınız temel adresle değiştirin.

    ```csharp
    https://<app_name>.azurewebsites.net/api/notifications/installations
    ```

    > [!NOTE]
    > Taban adresi şu biçimde olmalıdır``https://<app_name>.azurewebsites.net/``

1. [API anahtarı kullanarak Istemci kimlik doğrulaması](#authenticate-clients-using-an-api-key-optional) ' nı tamamlamayı seçtiyseniz, istek üst bilgilerini **apikey** değerini içerecek şekilde yapılandırmayı unutmayın.

   | Anahtar                            | Değer                          |
   | ------------------------------ | ------------------------------ |
   | apikey                         | <your_api_key>                 |

1. **Gövde**için **Ham** seçeneğini belirleyin, sonra Biçim seçenekleri listesinden **JSON** ' ı seçin ve sonra bazı yer tutucu **JSON** içeriğini ekleyin:

    ```json
    {}
    ```

1. **Gönder**’e tıklayın.

    > [!NOTE]
    > Hizmetten **400 hatalı bir istek** durumu almalısınız.

1. Adım 1-4 ' i yeniden yapın, ancak bu kez istekleri doğrulamak için istekler uç noktasını belirten aynı **400 hatalı istek** yanıtını alırsınız.

    ```bash
    https://<app_name>.azurewebsites.net/api/notifications/requests
    ```

> [!NOTE]
> İstemci mobil uygulamasından platforma özgü bilgiler gerektirdiğinden API 'nin geçerli istek verileri kullanılarak test olması henüz mümkün değildir.
