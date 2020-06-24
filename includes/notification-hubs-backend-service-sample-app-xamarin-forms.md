---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 40bc1c8c3d578e35b6689124f60f82d8ea85f0f2
ms.sourcegitcommit: e04a66514b21019f117a4ddb23f22c7c016da126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85112029"
---
### <a name="create-the-xamarinforms-solution"></a>Xamarin. Forms çözümünü oluşturma

1. **Visual Studio**'da, şablon olarak **boş formlar uygulamasını** kullanarak yeni bir **Xamarin. Forms** çözümü oluşturun ve **Proje adı**için *pushdemo* girerek.

    > [!NOTE]
    > **Boş form uygulamanızı yapılandırma** iletişim kutusunda, **kuruluş tanımlayıcısının** daha önce kullandığınız değerle eşleştiğinden ve hem **Android** hem de **iOS** hedeflerinin işaretli olduğundan emin olun.

1. **Denetim**  +  *Pushdemo* çözümüne ve ardından **NuGet paketlerini Güncelleştir** **' i** seçin.
1. **Denetim**  +  *Pushdemo* çözümüne ve ardından **NuGet Paketlerini Yönet..**. **öğesini** seçin.
1. **ÜzerindeNewtonsoft.Js** araması yapın ve bunun işaretli olduğundan emin olun.
1. **Paket Ekle**' ye tıklayın ve lisans koşullarını kabul etmek isteyip Istemediğiniz sorulduğunda **kabul et** ' e tıklayın.
1. **Command**  +  Kuruluşunuzda oluşturulan şablonlu uygulamayı test etmek için her hedef platformda (komut**girin**) uygulamayı derleyin ve çalıştırın.

### <a name="implement-the-cross-platform-components"></a>Platformlar arası bileşenleri uygulama

1. **Denetim**  +  **Pushdemo** projesine **tıklayın** , **Ekle** menüsünde **Yeni klasör** ' i seçin ve ardından **klasör adı**olarak *modeller* kullanarak **Ekle** ' ye tıklayın.

1. **Denetim**  +  **Modeller** klasörüne **tıklayın** ve ardından **Ekle** menüsünde **yeni dosya..** . öğesini seçin.

1. **Genel**  >  **boş sınıf**' i seçin, *DeviceInstallation.cs*girin ve ardından aşağıdaki uygulamayı ekleyin.

    ```csharp
    using System.Collections.Generic;
    using Newtonsoft.Json;

    namespace PushDemo.Models
    {
        public class DeviceInstallation
        {
            [JsonProperty("installationId")]
            public string InstallationId { get; set; }

            [JsonProperty("platform")]
            public string Platform { get; set; }

            [JsonProperty("pushChannel")]
            public string PushChannel { get; set; }

            [JsonProperty("tags")]
            public List<string> Tags { get; set; } = new List<string>();
        }
    }
    ```

1. Aşağıdaki uygulamayla *PushDemoAction.cs* adlı **modeller** klasörüne **boş bir sabit listesi** ekleyin.

    ```csharp
    namespace PushDemo.Models
    {
        public enum PushDemoAction
        {
            ActionA,
            ActionB
        }
    }
    ```

1. **Pushdemo** projesine *Hizmetler* adlı yeni bir klasör ekleyin ve ardından *ServiceContainer.cs* adlı klasöre aşağıdaki uygulamayla **boş bir sınıf** ekleyin.

     ```csharp
    using System;
    using System.Collections.Generic;

    namespace PushDemo.Services
    {
        public static class ServiceContainer
        {
            static readonly Dictionary<Type, Lazy<object>> services
                = new Dictionary<Type, Lazy<object>>();

            public static void Register<T>(Func<T> function)
                => services[typeof(T)] = new Lazy<object>(() => function());

            public static T Resolve<T>()
                => (T)Resolve(typeof(T));

            public static object Resolve(Type type)
            {
                {
                    if (services.TryGetValue(type, out var service))
                        return service.Value;

                    throw new KeyNotFoundException($"Service not found for type '{type}'");
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Bu, [XamCAT](https://github.com/xamcat/mobcat-library) deposundan [ServiceContainer](https://github.com/xamcat/mobcat-library/blob/master/MobCAT/ServiceContainer.cs) sınıfının kırpılmış bir sürümüdür. Bu, hafif bir IOC (denetim sürümü) kapsayıcısı olarak kullanılacaktır.

1. *IDeviceInstallationService.cs*adlı **Hizmetler** klasörüne **boş bir arabirim** ekleyin, ardından aşağıdaki kodu ekleyin.

    ```csharp
    using PushDemo.Models;

    namespace PushDemo.Services
    {
        public interface IDeviceInstallationService
        {
            string Token { get; set; }
            bool NotificationsSupported { get; }
            string GetDeviceId();
            DeviceInstallation GetDeviceInstallation(params string[] tags);
        }
    }
    ```

    > [!NOTE]
    > Bu arabirim, arka uç hizmeti için gereken platforma özgü işlevselliği ve **Deviceınstallation** bilgilerini sağlamak üzere her bir hedef tarafından daha sonra uygulanır ve önyüklendi.

1. *INotificationRegistrationService.cs*adlı **Hizmetler** klasörüne başka bir **boş arabirim** ekleyin ve ardından aşağıdaki kodu ekleyin.  

    ```csharp
    using System.Threading.Tasks;

    namespace PushDemo.Services
    {
        public interface INotificationRegistrationService
        {
            Task DeregisterDeviceAsync();
            Task RegisterDeviceAsync(params string[] tags);
            Task RefreshRegistrationAsync();
        }
    }
    ```

    > [!NOTE]
    > Bu, istemci ve arka uç hizmeti arasındaki etkileşimi işleymeyecektir.

1. *INotificationActionService.cs*adlı **Hizmetler** klasörüne başka bir **boş arabirim** ekleyin ve ardından aşağıdaki kodu ekleyin.  

    ```csharp
    namespace PushDemo.Services
    {
        public interface INotificationActionService
        {
            void TriggerAction(string action);
        }
    }
    ```

    > [!NOTE]
    > Bu, bildirim eylemlerinin işlenmesini merkezileştirmek için basit bir mekanizma olarak kullanılır.

1. Aşağıdaki uygulamayla, *ınocertificate Ationactionservice*öğesinden türetilen *IPushDemoNotificationActionService.cs* adlı **Hizmetler** klasörüne **boş bir arabirim** ekleyin.  

    ```csharp
    using System;
    using PushDemo.Models;

    namespace PushDemo.Services
    {
        public interface IPushDemoNotificationActionService : INotificationActionService
        {
            event EventHandler<PushDemoAction> ActionTriggered;
        }
    }
    ```

    > [!NOTE]
    > Bu tür, **Pushdemo** uygulamasına özeldir ve kesin olarak belirlenmiş bir şekilde tetiklenen eylemi belirlemek Için **pushdemoaction** sabit listesini kullanır.

1. Aşağıdaki kodla *ınocertificate Ationregistrationservice* uygulama *NotificationRegistrationService.cs* adlı **Hizmetler** klasörüne **boş bir sınıf** ekleyin.

    ```csharp
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    using PushDemo.Models;
    using Xamarin.Essentials;

    namespace PushDemo.Services
    {
        public class NotificationRegistrationService : INotificationRegistrationService
        {
            const string CachedTagsKey = "cached_tags";
            const string RequestUrl = "api/notifications/installations";

            string _baseApiUrl;
            HttpClient _client;
            IDeviceInstallationService _deviceInstallationService;

            public NotificationRegistrationService(string baseApiUri, string apiKey)
            {
                _client = new HttpClient();
                _client.DefaultRequestHeaders.Add("Accept", "application/json");
                _client.DefaultRequestHeaders.Add("apikey", apiKey);

                _baseApiUrl = baseApiUri;
            }

            IDeviceInstallationService DeviceInstallationService
                => _deviceInstallationService ??
                    (_deviceInstallationService = ServiceContainer.Resolve<IDeviceInstallationService>());

            public Task DeregisterDeviceAsync()
            {
                var deviceId = DeviceInstallationService?.GetDeviceId();

                if (string.IsNullOrWhiteSpace(deviceId))
                    throw new Exception("Unable to resolve an ID for the device.");

                SecureStorage.Remove(CachedTagsKey);

                return SendAsync(HttpMethod.Delete, $"{RequestUrl}/{deviceId}");
            }

            public async Task RegisterDeviceAsync(params string[] tags)
            {
                var deviceInstallation = DeviceInstallationService?.GetDeviceInstallation(tags);

                if (deviceInstallation == null)
                    throw new Exception($"Unable to get device installation information.");

                if (string.IsNullOrWhiteSpace(deviceInstallation.InstallationId))
                    throw new Exception($"No {nameof(deviceInstallation.InstallationId)} value for {nameof(DeviceInstallation)}");

                if (string.IsNullOrWhiteSpace(deviceInstallation.Platform))
                    throw new Exception($"No {nameof(deviceInstallation.Platform)} value for {nameof(DeviceInstallation)}");

                if (string.IsNullOrWhiteSpace(deviceInstallation.PushChannel))
                    throw new Exception($"No {nameof(deviceInstallation.PushChannel)} value for {nameof(DeviceInstallation)}");

                await SendAsync<DeviceInstallation>(HttpMethod.Put, RequestUrl, deviceInstallation)
                    .ConfigureAwait(false);

                var serializedTags = JsonConvert.SerializeObject(tags);
                await SecureStorage.SetAsync(CachedTagsKey, serializedTags);
            }

            public async Task RefreshRegistrationAsync()
            {
                var serializedTags = await SecureStorage.GetAsync(CachedTagsKey)
                    .ConfigureAwait(false);

                if (string.IsNullOrWhiteSpace(serializedTags))
                    return;

                var tags = JsonConvert.DeserializeObject<string[]>(serializedTags);

                await RegisterDeviceAsync(tags);
            }

            async Task SendAsync<T>(HttpMethod requestType, string requestUri, T obj)
            {
                string serializedContent = null;

                await Task.Run(() => serializedContent = JsonConvert.SerializeObject(obj))
                    .ConfigureAwait(false);

                await SendAsync(requestType, requestUri, serializedContent);
            }

            async Task SendAsync(
                HttpMethod requestType,
                string requestUri,
                string jsonRequest = null)
            {
                var request = new HttpRequestMessage(requestType, new Uri($"{_baseApiUrl}{requestUri}"));

                if (jsonRequest != null)
                    request.Content = new StringContent(jsonRequest, Encoding.UTF8, "application/json");

                var response = await _client.SendAsync(request).ConfigureAwait(false);

                response.EnsureSuccessStatusCode();
            }
        }
    }
    ```

    > [!NOTE]
    > **Apikey** bağımsız DEĞIŞKENI yalnızca [API anahtarı kullanarak kimlik doğrulama istemcilerinin](#authenticate-clients-using-an-api-key-optional) tamamlanmasını tercih ediyorsanız gereklidir.

1. *PushDemoNotificationActionService.cs* adlı **Hizmetler** klasörüne, *ıpushdemonocertificate* için aşağıdaki kodla bir **boş sınıf** ekleyin.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using PushDemo.Models;

    namespace PushDemo.Services
    {
        public class PushDemoNotificationActionService : IPushDemoNotificationActionService
        {
            readonly Dictionary<string, PushDemoAction> _actionMappings = new Dictionary<string, PushDemoAction>
            {
                { "action_a", PushDemoAction.ActionA },
                { "action_b", PushDemoAction.ActionB }
            };

            public event EventHandler<PushDemoAction> ActionTriggered = delegate { };

            public void TriggerAction(string action)
            {
                if (!_actionMappings.TryGetValue(action, out var pushDemoAction))
                    return;

                List<Exception> exceptions = new List<Exception>();

                foreach (var handler in ActionTriggered?.GetInvocationList())
                {
                    try
                    {
                        handler.DynamicInvoke(this, pushDemoAction);
                    }
                    catch (Exception ex)
                    {
                        exceptions.Add(ex);
                    }
                }

                if (exceptions.Any())
                    throw new AggregateException(exceptions);
            }
        }
    }
    ```

1. Aşağıdaki uygulamayla *config.cs* adlı **Pushdemo** projesine **boş bir sınıf** ekleyin.  

    ```csharp
    namespace PushDemo
    {
        public static partial class Config
        {
            public static string ApiKey = "API_KEY";
            public static string BackendServiceEndpoint = "BACKEND_SERVICE_ENDPOINT";
        }
    }
    ```

    > [!NOTE]
    > Bu, gizli dizileri kaynak denetiminden tutmanın basit bir yolu olarak kullanılır. Bu değerleri bir otomatik derleme parçası olarak değiştirebilir veya yerel bir kısmi sınıf kullanarak bunları geçersiz kılabilirsiniz. Bunu bir sonraki adımda yapacaksınız.
    >
    > **Apikey** alanı yalnızca [API anahtarı kullanarak kimlik doğrulama istemcilerini](#authenticate-clients-using-an-api-key-optional) tamamlamayı seçtiyseniz gereklidir.

1. Bu kez, aşağıdaki uygulamayla birlikte *config. local_secrets. cs* adlı **pushdemo** projesine başka bir **boş sınıf** ekleyin.  

    ```csharp
    namespace PushDemo
    {
        public static partial class Config
        {
            static Config()
            {
                ApiKey = "<your_api_key>";
                BackendServiceEndpoint = "<your_api_app_url>";
            }
        }
    }
    ```

    > [!NOTE]
    > Yer tutucu değerlerini kendi değerlerinizle değiştirin. Arka uç hizmetini oluştururken bunları bir yere döndürmelisiniz. **API uygulaması** URL 'si olmalıdır ``https://<api_app_name>.azurewebsites.net/`` . ``*.local_secrets.*``Bu dosyayı kaydetmeden kaçınmak için gitignore dosyanıza eklemeyi unutmayın.
    >
    > **Apikey** alanı yalnızca [API anahtarı kullanarak kimlik doğrulama istemcilerini](#authenticate-clients-using-an-api-key-optional) tamamlamayı seçtiyseniz gereklidir.

1. Aşağıdaki uygulamayla *Bootstrap.cs* adlı **Pushdemo** projesine **boş bir sınıf** ekleyin.  

    ```csharp
    using System;
    using PushDemo.Services;

    namespace PushDemo
    {
        public static class Bootstrap
        {
            public static void Begin(Func<IDeviceInstallationService> deviceInstallationService)
            {
                ServiceContainer.Register(deviceInstallationService);

                ServiceContainer.Register<IPushDemoNotificationActionService>(()
                    => new PushDemoNotificationActionService());

                ServiceContainer.Register<INotificationRegistrationService>(()
                    => new NotificationRegistrationService(
                        Config.BackendServiceEndpoint,
                        Config.ApiKey));
            }
        }
    }
    ```

    > [!NOTE]
    > **Begin** yöntemi, uygulama bir **ıdeviceınstalınstall hizmetinin**platforma özgü uygulamasında geçiş başlattığında her platform tarafından çağrılır.
    >
    > **Notificationregistrationservice** **apikey** Oluşturucu bağımsız değişkeni yalnızca [API anahtarı kullanarak kimlik doğrulama istemcilerinin](#authenticate-clients-using-an-api-key-optional) tamamlanmasını tercih ediyorsanız gereklidir.

### <a name="implement-the-cross-platform-ui"></a>Platformlar arası kullanıcı arabirimini uygulama

1. **Pushdemo** projesinde **MainPage. xaml** ' yi açın ve **StackLayout** denetimini aşağıdaki kodla değiştirin.

    ```xml
    <StackLayout VerticalOptions="EndAndExpand"  
                 HorizontalOptions="FillAndExpand"
                 Padding="20,40">
        <Button x:Name="RegisterButton"
                Text="Register"
                Clicked="RegisterButtonClicked" />
        <Button x:Name="DeregisterButton"
                Text="Deregister"
                Clicked="DeregisterButtonClicked" />
    </StackLayout>
    ```

1. Artık **MainPage.xaml.cs**' de, **ınocertificate ationregistrationservice** uygulamasına bir başvuruyu depolamak için **salt okunur** bir yedekleme alanı ekleyin.

    ```csharp
    readonly INotificationRegistrationService _notificationRegistrationService;
    ```

1. **MainPage** oluşturucusunda, **ServiceContainer** öğesini kullanarak **ınocertificate ationregistrationservice** uygulamasını çözümleyin ve *_notificationregistrationservice_* yedekleme alanına atayın.

    ```csharp
    public MainPage()
    {
        InitializeComponent();

        _notificationRegistrationService =
            ServiceContainer.Resolve<INotificationRegistrationService>();
    }
    ```

1. **RegisterButton** ve **DeregisterButton** Button öğesine **tıklanan** **kayıt**kaldırma yöntemlerini çağıran olaylar için olay işleyicilerini uygulayın / **Deregister** .

    ```csharp
    void RegisterButtonClicked(object sender, EventArgs e)
        => _notificationRegistrationService.RegisterDeviceAsync().ContinueWith((task)
            => { ShowAlert(task.IsFaulted ?
                    task.Exception.Message :
                    $"Device registered"); });

    void DeregisterButtonClicked(object sender, EventArgs e)
        => _notificationRegistrationService.DeregisterDeviceAsync().ContinueWith((task)
            => { ShowAlert(task.IsFaulted ?
                    task.Exception.Message :
                    $"Device deregistered"); });

    void ShowAlert(string message)
        => MainThread.BeginInvokeOnMainThread(()
            => DisplayAlert("PushDemo", message, "OK").ContinueWith((task)
                => { if (task.IsFaulted) throw task.Exception; }));
    ```

1. Artık **app.xaml.cs**' de, aşağıdaki ad alanlarına başvurulduğundan emin olun.

    ```csharp
    using PushDemo.Models;
    using PushDemo.Services;
    using Xamarin.Essentials;
    using Xamarin.Forms;
    ```

1. **Ipushdemonocertificate, Actionservice** **actiontetiklenmiş** olayı için olay işleyicisini uygulayın.

    ```csharp
    void NotificationActionTriggered(object sender, PushDemoAction e)
        => ShowActionAlert(e);

    void ShowActionAlert(PushDemoAction action)
        => MainThread.BeginInvokeOnMainThread(()
            => MainPage?.DisplayAlert("PushDemo", $"{action} action received", "OK")
                .ContinueWith((task) => { if (task.IsFaulted) throw task.Exception; }));
    ```

1. **Uygulama** oluşturucuda, **ServiceContainer** öğesini kullanarak **ıpushnotificationactionservice** uygulamasını çözümleyin ve **ıpushdemonocertificate ' actionservice** **actiontetiklenmiş** olayına abone olun.

    ```csharp
    public App()
    {
        InitializeComponent();

        ServiceContainer.Resolve<IPushDemoNotificationActionService>()
            .ActionTriggered += NotificationActionTriggered;

        MainPage = new MainPage();
    }
    ```

    > [!NOTE]
    > Bu, anında iletme bildirimi eylemlerinin alındığını ve yayılmasını göstermek için yeterlidir. Genellikle, bu durum, belirli bir görünüme gidilirken veya bazı verileri yenileyerek, bu durumda kök **sayfa**, **ana** sayfa aracılığıyla bir uyarı görüntülenmesi yerine sessizce işlenir.
