---
title: WebJobs SDK ile başlayın
description: Olay odaklı arka plan işleme için WebJobs SDK'ya giriş. Azure hizmetlerinde ve üçüncü taraf hizmetlerinde verilere nasıl erişilenleri öğrenin.
author: ggailey777
ms.devlang: dotnet
ms.topic: article
ms.date: 02/18/2019
ms.author: glenga
ms.openlocfilehash: 4ee724ec66d5fb474f8c8a9a967cc7235fef5e85
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732620"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Olay temelli arka plan işleme için Azure Web İşleri SDK'sını kullanmaya başlama

Bu makalede, Visual Studio 2019'un azure Web Jobs SDK projesi oluşturmak, yerel olarak çalıştırmak ve ardından [Azure Uygulama Hizmeti'ne](overview.md)dağıtmak için nasıl kullanılacağı gösterilmektedir. WebJobs SDK Sürümü 3.x hem .NET Core ve .NET Framework konsol uygulamaları destekler. Web İşler SDK ile çalışma hakkında daha fazla bilgi edinmek [için, etkinliğe dayalı arka plan işlemleri için Azure Web İşlerI SDK'sını nasıl kullanacağınız](webjobs-sdk-how-to.md)konusuna bakın.

Bu makalede, WebJobs'ı .NET Core konsol uygulaması olarak nasıl dağıtabileceğiniz gösterilmektedir. WebJobs'u bir .NET Framework konsolu uygulaması olarak dağıtmak için [WebJobs'u .NET Framework konsolu uygulamaları olarak](webjobs-dotnet-deploy-vs.md#webjobs-as-net-framework-console-apps)görün. Yalnızca .NET Framework'u destekleyen WebJobs SDK sürüm 2.x ile ilgileniyorsanız Visual [Studio - Azure Uygulama Hizmeti'ni kullanarak Web İşler geliştir in ve dağıtı(Web İşleri Geliştirme)](webjobs-dotnet-deploy-vs.md)sayfasını görün.

## <a name="prerequisites"></a>Ön koşullar

* **Azure geliştirme** iş yüküyle Visual [Studio 2019'u yükleyin.](/visualstudio/install/) Visual Studio'nuz varsa ancak bu iş yükünüz yoksa, **Araçları ve Özellikleri Al araçları >** seçerek iş yükünü ekleyin.

* Web Jobs SDK projenizi Azure'da yayınlamak için [bir Azure hesabınız](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) olması gerekir.

## <a name="create-a-project"></a>Proje oluşturma

1. Visual Studio'da **Yeni Proje Oluştur'u**seçin.

2. **Konsol Uygulamasını (.NET Core)** seçin.

3. Proje *WebJobsSDKSample'ı*adlandırın ve ardından **Oluştur'u**seçin.

   ![Yeni Proje iletişim kutusu](./media/webjobs-sdk-get-started/new-project.png)

## <a name="webjobs-nuget-packages"></a>WebJobs NuGet paketleri

1. NuGet paketinin en son kararlı 3.x `Microsoft.Azure.WebJobs`sürümünü yükleyin, hangi içerir . [ `Microsoft.Azure.WebJobs.Extensions` ](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/)

     Paket Yöneticisi **Konsolu** komutu aşağıda veda edin:

     ```powershell
     Install-Package Microsoft.Azure.WebJobs.Extensions -version <3_X_VERSION>
     ```

    Bu komutta, `<3_X_VERSION>` paketin desteklenen bir sürümüyle değiştirin. 

## <a name="create-the-host"></a>Ana Bilgisayar Oluşturma

Ana bilgisayar, tetikleyicileri ve çağrıları işlevleri dinleyen işlevler için çalışma zamanı kapsayıcısiyidir. Aşağıdaki [`IHost`](/dotnet/api/microsoft.extensions.hosting.ihost)adımlar, ASP.NET Core'daki Genel Ana Bilgisayar'ı uygulayan bir ana bilgisayar oluşturur.

1. *Program.cs*olarak, `using` şu ifadeleri ekleyin:

    ```cs
    using System.Threading.Tasks;
    using Microsoft.Extensions.Hosting;
    ```

1. `Main` yöntemini aşağıdaki kod ile değiştirin:

    ```cs
    static async Task Main()
    {
        var builder = new HostBuilder();
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                });
        var host = builder.Build();
        using (host)
        {
            await host.RunAsync();
        }
    }
    ```

ASP.NET Core'da, ana bilgisayar yapılandırmaları [`HostBuilder`](/dotnet/api/microsoft.extensions.hosting.hostbuilder) örnekteki çağrı yöntemleriyle ayarlanır. Daha fazla bilgi için [bkz.](/aspnet/core/fundamentals/host/generic-host) Uzantı `ConfigureWebJobs` yöntemi Webİşler ana bilgisayarını başlatılmasını önler. ' `ConfigureWebJobs`de, belirli Web İşler uzantılarını başharfe alerler ve bu uzantıların özelliklerini ayarlarsınız.  

## <a name="enable-console-logging"></a>Konsol günlüğe kaydetmeyi etkinleştirme

Bu bölümde, [ASP.NET Core günlük çerçevesini](/aspnet/core/fundamentals/logging)kullanan konsol günlüğü' diye ayarlarsınız.

1. NuGet paketinin en son kararlı sürümünü `Microsoft.Extensions.Logging`yükleyin, hangi içerir. [ `Microsoft.Extensions.Logging.Console` ](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)

   Paket Yöneticisi **Konsolu** komutu aşağıda veda edin:

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version <3_X_VERSION>
   ```

1. *Program.cs,* bir `using` deyim ekleyin:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

    Bu komutta, `<3_X_VERSION>` paketin desteklenen 3.x sürümüyle değiştirin.

1. Yöntemi [`ConfigureLogging`](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) 'de [`HostBuilder`](/dotnet/api/microsoft.extensions.hosting.hostbuilder)çağırın. Yöntem, [`AddConsole`](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions.addconsole) yapılandırmaya konsol günlüğe kaydetme ekler.

    ```cs
    builder.ConfigureLogging((context, b) =>
    {
        b.AddConsole();
    });
    ```

    Yöntem `Main` şimdi şuna benzer:

    ```cs
    static async Task Main()
    {
        var builder = new HostBuilder();
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                });
        builder.ConfigureLogging((context, b) =>
                {
                    b.AddConsole();
                });
        var host = builder.Build();
        using (host)
        {
            await host.RunAsync();
        }
    }
    ```

    Bu güncelleştirme aşağıdakileri yapar:

    * [Pano günlüğe kaydetmeyi](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs)devre dışı kılabilir. Pano eski bir izleme aracıdır ve pano günlüğü yüksek üretim senaryoları için önerilmez.
    * Varsayılan [filtreleme](webjobs-sdk-how-to.md#log-filtering)ile konsol sağlayıcısı ekler.

Artık, Azure Depolama kuyruğuna gelen iletiler tarafından tetiklenen bir işlev ekleyebilirsiniz.

## <a name="install-the-storage-binding-extension"></a>Depolama bağlama uzantısını yükleme

Sürüm 3.x ile başlayarak, WebJobs SDK tarafından gerekli Depolama bağlama uzantısını açıkça yüklemeniz gerekir. Önceki sürümlerde, Depolama ciltleri SDK'ya dahil edilmiştir.

1. [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketinin en son kararlı sürümünü, sürüm 3.x'i yükleyin. 

    Paket Yöneticisi **Konsolu** komutu aşağıda veda edin:

    ```powershell
    Install-Package Microsoft.Azure.WebJobs.Extensions.Storage -Version <3_X_VERSION>
    ```
    
    Bu komutta, `<3_X_VERSION>` paketin desteklenen bir sürümüyle değiştirin. 

2. Uzantı `ConfigureWebJobs` yönteminde, `AddAzureStorage` Depolama uzantısını başlatmaörneğinde [`HostBuilder`](/dotnet/api/microsoft.extensions.hosting.hostbuilder) yöntemi arayın. Bu noktada, `ConfigureWebJobs` yöntem aşağıdaki örnek gibi görünür:

    ```cs
    builder.ConfigureWebJobs(b =>
                    {
                        b.AddAzureStorageCoreServices();
                        b.AddAzureStorage();
                    });
    ```

## <a name="create-a-function"></a>İşlev oluşturma

1. Projeye sağ tıklayın, > **Yeni Öğe Ekle'yi seçin...**, **Sınıf'ı**seçin, yeni C# sınıfı dosyasına *Functions.cs*ve **Ekle'yi**seçin. **Add**

1. Functions.cs olarak, oluşturulan şablonu aşağıdaki kodla değiştirin:
    
    ```cs
    using Microsoft.Azure.WebJobs;
    using Microsoft.Extensions.Logging;
    
    namespace WebJobsSDKSample
    {
        public class Functions
        {
            public static void ProcessQueueMessage([QueueTrigger("queue")] string message, ILogger logger)
            {
                logger.LogInformation(message);
            }
        }
    }
    ```

   Öznitelik, `QueueTrigger` '. `queue` Sıra iletisinin içeriği parametredeki yöntem koduna `message` sağlanır. Yöntemin gövdesi, tetikleyici verileri işlediğiniz yerdir. Bu örnekte, kod sadece iletiyi kaydeder.

   `message` Parametre nin bir dize olması gerekyok. Ayrıca bir JSON nesnesine, bir bayt dizisine veya [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage) nesnesine de bağlanabilirsiniz. [Bkz. Sıra tetikleyici kullanımı.](/azure/azure-functions/functions-bindings-storage-queue-trigger?tabs=csharp#usage) Her bağlama türü (kuyruklar, blobs veya tablolar gibi) bağlayabilirsiniz parametre türleri farklı bir dizi vardır.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Yerel olarak çalışan Azure Depolama emülatörü, Web İşler SDK'nın ihtiyaç duyduğu tüm özelliklere sahip değildir. Bu nedenle bu bölümde Azure'da bir depolama hesabı oluşturur ve projeyi kullanacak şekilde yapılandırabilirsiniz. Zaten bir depolama hesabınız varsa, adım 6'ya aşağı atlayın.

1. Visual studio'da **Sunucu Gezgini'ni** açın ve Azure'da oturum açın. **Azure** düğümüne sağ tıklayın ve ardından **Microsoft Azure Aboneliği'ne Bağlan'ı**seçin.

   ![Azure'da oturum açma](./media/webjobs-sdk-get-started/sign-in.png)

1. **Sunucu Gezgini'ndeki** **Azure** düğümünün **altında, Depolama'yı**sağ tıklatın ve ardından Depolama **Hesabı Oluştur'u**seçin.

   ![Depolama hesabı menüsü oluştur](./media/webjobs-sdk-get-started/create-storage-account-menu.png)

1. Depolama **Hesabı Oluştur** iletişim kutusuna, depolama hesabı için benzersiz bir ad girin.

1. Uygulama Hizmeti uygulamanızı oluşturduğunuz **bölgeyle** aynı bölgeyi veya size yakın bir bölgeyi seçin.

1. **Oluştur**’u seçin.

   ![Depolama hesabı oluşturma](./media/webjobs-sdk-get-started/create-storage-account.png)

1. **Server Explorer'daki** **Depolama** düğümünün altında yeni Depolama hesabını seçin. **Özellikler** penceresinde, **Bağlantı Dizesi** değer alanının sağındaki elipsis (**...**) seçeneğini belirleyin.

   ![Bağlantı String elipsis](./media/webjobs-sdk-get-started/conn-string-ellipsis.png)

1. Bağlantı dizesini kopyalayın ve bu değeri yeniden kolayca kopyalayabileceğiniz bir yere kaydedin.

   ![Bağlantı dizelerini kopyalama](./media/webjobs-sdk-get-started/copy-key.png)

## <a name="configure-storage-to-run-locally"></a>Depolama alanını yerel olarak çalışacak şekilde yapılandırın

WebJobs SDK, Azure'daki Uygulama Ayarları'nda depolama bağlantı dizesini arar. Yerel olarak çalıştırdığınızda, bu değeri yerel yapılandırma dosyasında veya ortam değişkenlerinde arar.

1. Projeye sağ tıklayın, > **Yeni Öğe Ekle'yi seçin...**, **JavaScript JSON yapılandırma dosyasını**seçin, yeni dosya *appsettings.json* dosyasını adlandırın ve **Ekle'yi**seçin. **Add** 

1. Yeni dosyada, aşağıdaki `AzureWebJobsStorage` örnekte olduğu gibi bir alan ekleyin:

    ```json
    {
        "AzureWebJobsStorage": "{storage connection string}"
    }
    ```

1. *{depolama bağlantı dizesi}* ile daha önce kopyaladığınız bağlantı dizesini değiştirin.

1. Solution Explorer'da ve **Özellikler** penceresinde *appsettings.json* dosyasını seçin, **daha yeniyse Kopyala'dan Kopyaya** **Kopyala'yı** kopyala'ya kopyala'yı ayarlayın.

Daha sonra, Azure Uygulama Hizmeti'nde uygulamanızda aynı bağlantı dize uygulaması ayarını eklersiniz.

## <a name="test-locally"></a>Yerel olarak test edin

Bu bölümde, projeyi yerel olarak oluşturur ve çalıştırın ve bir sıra iletisi oluşturarak işlevi tetiklersiniz.

1. Projeyi çalıştırmak için **Ctrl+F5** tuşuna basın.

   Konsol, çalışma zamanının işlevinizi bulduğunu ve bunu tetiklemek için sıra iletilerinin beklediğini gösterir. Aşağıdaki çıktı v3.x ana bilgisayar tarafından oluşturulur:

   ```console
    info: Microsoft.Azure.WebJobs.Hosting.JobHostService[0]
          Starting JobHost
    info: Host.Startup[0]
          Found the following functions:
          WebJobsSDKSample.Functions.ProcessQueueMessage

    info: Host.Startup[0]
          Job host started
    Application started. Press Ctrl+C to shut down.
    Hosting environment: Development
    Content root path: C:\WebJobsSDKSample\WebJobsSDKSample\bin\Debug\netcoreapp2.1\
   ```

1. Konsol penceresini kapatın.

1. Visual Studio'daki **Sunucu Gezgini'nde,** yeni depolama hesabınız için düğümü genişletin ve **ardından Kuyruklar'ı**sağ tıklatın.

1. **Sıra Oluştur'u**seçin.

1. *Sırayı* sıranın adı olarak girin ve sonra **Tamam'ı**seçin.

   ![Sıra oluşturma](./media/webjobs-sdk-get-started/create-queue.png)

1. Yeni sıra için düğüme sağ tıklayın ve ardından **Sırayı Görüntüle'yi**seçin.

1. İleti **Ekle** simgesini seçin.

   ![Sıra oluşturma](./media/webjobs-sdk-get-started/create-queue-message.png)

1. İleti **Ekle** iletişim kutusuna *Merhaba Dünyası'nı girin!* İleti **metni**olarak ve sonra **Tamam'ı**seçin. Şimdi sırada bir ileti var.

   ![Sıra oluşturma](./media/webjobs-sdk-get-started/hello-world-text.png)

1. Projeyi tekrar çalıştırın.

   İşlevdeki `QueueTrigger` özniteliği kullandığınızdan, WeJobs SDK çalışma zamanı başlatıldığında sıra iletilerini dinler. `ProcessQueueMessage` *Sıra* adlı sırada yeni bir sıra iletisi bulur ve işlevi çağırır.

   Sıra [yoklama üstel geri tepme](/azure/azure-functions/functions-bindings-storage-queue-trigger?tabs=csharp#polling-algorithm)nedeniyle, iletibulmak ve işlevi çağırmak için çalışma süresi için 2 dakika kadar sürebilir. Bu bekleme süresi [geliştirme modunda](webjobs-sdk-how-to.md#host-development-settings)çalıştırılarak azaltılabilir.

   Konsol çıkışı aşağıdaki gibi görünür:

   ```console
    info: Function.ProcessQueueMessage[0]
          Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=2c319369-d381-43f3-aedf-ff538a4209b8)
    info: Function.ProcessQueueMessage[0]
          Trigger Details: MessageId: b00a86dc-298d-4cd2-811f-98ec39545539, DequeueCount: 1, InsertionTime: 1/18/2019 3:28:51 AM +00:00
    info: Function.ProcessQueueMessage.User[0]
          Hello World!
    info: Function.ProcessQueueMessage[0]
          Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=2c319369-d381-43f3-aedf-ff538a4209b8)
   ```

1. Konsol penceresini kapatın. 

1. Sıra penceresine geri dön ve yenileyin. Yerel olarak çalışan işleviniz tarafından işlendiği için ileti gitti. 

## <a name="add-application-insights-logging"></a>Uygulama Bilgileri günlüğe ekleme

Proje Azure'da çalıştığında, konsol çıktısını görüntüleyerek işlev yürütmeyi izleyebilirsiniz. Önerdiğimiz izleme çözümü [Application Insights](../azure-monitor/app/app-insights-overview.md)olduğunu. Daha fazla bilgi için [bkz.](../azure-functions/functions-monitoring.md)

Bu bölümde, Azure'a dağıtılamadan önce Application Insights günlüğe kaydetmeyi ayarlamak için aşağıdaki görevleri yaparsınız:

* Bir Uygulama Hizmeti uygulamanız ve çalışmanız gereken bir Uygulama Öngörüleri örneğine sahip olduğundan emin olun.
* Uygulama Hizmeti uygulamasını, Uygulama Öngörüleri örneğini ve daha önce oluşturduğunuz depolama hesabını kullanacak şekilde yapılandırın.
* Application Insights'a günlüğe kaydolmak için projeyi ayarlayın.

### <a name="create-app-service-app-and-application-insights-instance"></a>Uygulama Hizmeti uygulaması ve Uygulama Öngörüleri örneği oluşturun

1. Kullanabileceğiniz bir Uygulama Hizmeti uygulamanız yoksa, [bir uygulama oluşturun.](app-service-web-get-started-dotnet-framework.md) Uygulamanızı oluşturduğunuzda, bağlı bir Application Insights kaynağı da oluşturabilirsiniz. Bunu yaptığınızda, `APPINSIGHTS_INSTRUMENTATIONKEY` uygulamanızda sizin için ayarlanır.

1. Kullanabileceğiniz bir Uygulama Öngörüleri kaynağınız yoksa, [bir tane oluşturun.](../azure-monitor/app/create-new-resource.md ) **Uygulama türünü** **Genel**olarak ayarlayın ve **enstrümantasyon anahtarını kopyala'yı**izleyen bölümleri atlayın.

1. Kullanmak istediğiniz bir Application Insights kaynağınız varsa, [enstrümantasyon anahtarını kopyalayın.](../azure-monitor/app/create-new-resource.md#copy-the-instrumentation-key)

### <a name="configure-app-settings"></a>Uygulama ayarlarını yapılandırma 

1. Visual Studio'daki **Sunucu Gezgini'nde,** **Azure**altında **Uygulama Hizmeti** düğümlerini genişletin.

1. Uygulama Hizmeti uygulamanızın içinde yer alan kaynak grubunu genişletin ve ardından Uygulama Hizmeti uygulamanıza sağ tıklayın.

1. **Ayarları Görüntüle'yi**seçin.

1. Bağlantı **Dizeleri** kutusuna aşağıdaki girişi ekleyin.

   |Adı  |bağlantı String  |Veritabanı Türü|
   |---------|---------|------|
   |AzureWebJobsStorage | {Daha önce kopyaladığınız Depolama bağlantı dizesi}|Özel|

1. Uygulama **Ayarları** kutusunda Application Insights enstrümantasyon anahtarı yoksa, daha önce kopyaladığınız anahtarı ekleyin. (Uygulama Hizmeti uygulamasını nasıl oluşturduğunuza bağlı olarak enstrümantasyon tuşu zaten orada olabilir.)

   |Adı  |Değer  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {enstrümantasyon tuşu} |

1. *{instrumentation tuşu}* kullanarak kullandığınız Application Insights kaynağından enstrümantasyon anahtarıyla değiştirin.

1. **Kaydet**’i seçin.

1. Projeye Uygulama Öngörüleri bağlantısını ekleyin, böylece yerel olarak çalıştırabilirsiniz. *appsettings.json* dosyasında, aşağıdaki `APPINSIGHTS_INSTRUMENTATIONKEY` örnekte olduğu gibi bir alan ekleyin:

    ```json
    {
        "AzureWebJobsStorage": "{storage connection string}",
        "APPINSIGHTS_INSTRUMENTATIONKEY": "{instrumentation key}"
    }
    ```

    *{instrumentation tuşu}* kullanarak kullandığınız Application Insights kaynağından enstrümantasyon anahtarıyla değiştirin.

1. Yaptığınız değişiklikleri kaydedin.

### <a name="add-application-insights-logging-provider"></a>Uygulama Öngörüleri günlük sağlayıcısı ekle

[Application Insights](../azure-monitor/app/app-insights-overview.md) günlüğe kaydetme avantajından yararlanmak için, aşağıdakileri yapmak için günlük kodunuzu güncelleyin:

* Varsayılan [filtreleme](webjobs-sdk-how-to.md#log-filtering)ile Bir Uygulama Öngörüleri günlük sağlayıcısı ekleyin. Yerel olarak çalışırken, tüm Bilgiler ve üst düzey günlükler hem konsola hem de Uygulama Öngörülerine yazılır.
* Giriş noktası çıktığında günlük `using` çıkışının temizlendiğinden emin olmak için [LoggerFactory](./webjobs-sdk-how-to.md#logging-and-monitoring) nesnesini bir bloğa koyun.

1. [ `Microsoft.Azure.WebJobs.Logging.ApplicationInsights` NuGet paketinin](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/)en son kararlı 3.x sürümünü yükleyin.

   Paket Yöneticisi **Konsolu** komutu aşağıda veda edin:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -Version <3_X_VERSION>
   ```
    Bu komutta, `<3_X_VERSION>` paketin desteklenen bir sürümüyle değiştirin.

1. *Program.cs* açın ve `Main` yöntemdeki kodu aşağıdaki kodla değiştirin:

    ```cs
    static async Task Main()
    {
        var builder = new HostBuilder();
        builder.UseEnvironment(EnvironmentName.Development);
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                    b.AddAzureStorage();
                });
        builder.ConfigureLogging((context, b) =>
                {
                    b.AddConsole();

                    // If the key exists in settings, use it to enable Application Insights.
                    string instrumentationKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
                    if (!string.IsNullOrEmpty(instrumentationKey))
                    {
                        b.AddApplicationInsightsWebJobs(o => o.InstrumentationKey = instrumentationKey);
                    }
                });
        var host = builder.Build();
        using (host)
        {
            await host.RunAsync();
        }
    }
    ```

    Bu, uygulama ayarlarınıza daha önce eklediğiniz anahtarı kullanarak Uygulama Öngörüleri sağlayıcısını günlüğe ekler.

## <a name="test-application-insights-logging"></a>Uygulama Öngörülerini Test Edin

Bu bölümde, günlüğe kaydetme verilerinin artık Application Insights'A ve konsola gittiğini doğrulamak için yerel olarak yeniden çalışırsınız.

1. Hello App Insights'ı girmek [dışında, daha önce](#test-locally)yaptığınız gibi bir sıra iletisi oluşturmak için Visual Studio'daki Sunucu *Gezgini'ni kullanın!* **Server Explorer** mesaj metni olarak.

1. Projeyi çalıştırın.

   WebJobs SDK sıra iletisini işler ve konsol penceresinde günlükleri görürsünüz.

1. Konsol penceresini kapatın.

1. Uygulama Öngörüleri kaynağınızı görüntülemek için [Azure portalına](https://portal.azure.com/) gidin. **Uygulama Öngörülerini**arayın ve seçin.

1. Uygulama Öngörüleri örneğini seçin.

1. **Ara'yı**seçin.

   ![Arama'yı Seçin](./media/webjobs-sdk-get-started/select-search.png)

1. *Merhaba App Insights'ı göremiyorsanız!* dakika boyunca düzenli olarak **Yenile'yi** seçin. (Günlükler hemen görünmez, çünkü Application Insights istemcisinin işlediği günlükleri temizlemesi biraz zaman alır.)

   ![Uygulama Öngörülerinde Oturum Açma](./media/webjobs-sdk-get-started/logs-in-ai.png)

1. Konsol penceresini kapatın.

## <a name="deploy-to-azure"></a><a name="deploy-as-a-webjob"></a>Azure’a dağıtma

Dağıtım sırasında, işlevlerinizi çalıştırabileceğiniz bir uygulama hizmeti örneği oluşturursunuz. Bir .NET Core konsol uygulamasını Azure'daki App Service'e yayımladığınızda, otomatik olarak WebJob olarak çalıştırılır. Yayımlama hakkında daha fazla bilgi edinmek için [Visual Studio'yı kullanarak Web İşler geliştir ve dağıt'ı](webjobs-dotnet-deploy-vs.md)'na bakın.

[!INCLUDE [webjobs-publish-net-core](../../includes/webjobs-publish-net-core.md)]

## <a name="trigger-the-function-in-azure"></a>Azure'da işlevi tetikle

1. Yerel olarak çalışmadığınızdan emin olun (hala açıksa konsol penceresini kapatın). Aksi takdirde, yerel örnek oluşturduğunuz sıra iletilerini işleyen ilk örnek olabilir.

1. Visual Studio'daki **Sıra** sayfasında, sıraya eskisi gibi bir ileti ekleyin.

1. **Kuyruk** sayfasını yenileyin ve Azure'da çalışan işlev tarafından işlendiği için yeni ileti kaybolur.

   > [!TIP]
   > Azure'da sınama yaparken, bir kuyruk tetikleyici işlevinin hemen çağrıldığından emin olmak için [geliştirme modunu](webjobs-sdk-how-to.md#host-development-settings) kullanın ve [sıra yoklama üstel geri çekilme](/azure/azure-functions/functions-bindings-storage-queue-trigger?tabs=csharp#polling-algorithm)nedeniyle gecikmeleri önleyin.

### <a name="view-logs-in-application-insights"></a>Uygulama Öngörüleri'ndeki günlükleri görüntüleme

1. Azure [portalını](https://portal.azure.com/)açın ve Application Insights kaynağınıza gidin.

1. **Ara'yı**seçin.

1. *Merhaba Azure'u görmüyorsanız!* dakika boyunca düzenli olarak **Yenile'yi** seçin.

   Hello Azure da dahil olmak üzere bir WebJob'ta çalışan işlevin günlüklerini *görürsünüz!* önceki bölüme girdiğiniz metin.

## <a name="add-an-input-binding"></a>Giriş bağlama ekleme

Giriş bağlamaları, verileri okuyan kodu basitleştirir. Bu örnekte, sıra iletisi bir blob adı olur ve Azure Depolama'da bir blob bulmak ve okumak için blob adını kullanırsınız.

1. *Functions.cs,* yöntemi `ProcessQueueMessage` aşağıdaki kodla değiştirin:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
   }
   ```

   Bu kodda, `queueTrigger` bağlayıcı bir [ifadedir,](../azure-functions/functions-bindings-expressions-patterns.md)bu da çalışma zamanında farklı bir değere çözüldüğü anlamına gelir.  Çalışma zamanında, sıra iletisinin içeriğini vardır.

1. Bir `using`ekle :

   ```cs
   using System.IO;
   ```

1. Depolama hesabınızda bir blob kapsayıcısı oluşturun.

   a. Visual Studio'daki **Sunucu Gezgini'nde,** depolama hesabınız için düğümü genişletin, **Blobs'a**sağ tıklayın ve ardından **Blob Kapsayıcısı Oluştur'u**seçin.

   b. **Blob Kapsayıcı oluştur** iletişim kutusunda, kapsayıcı adı olarak *kapsayıcıyı* girin ve ardından **Tamam'ı**tıklatın.

1. *Program.cs* dosyasını blob kapsayıcısına yükleyin. (Bu dosya burada örnek olarak kullanılır; herhangi bir metin dosyası yükleyebilir ve dosyanın adını içeren bir sıra iletisi oluşturabilirsiniz.)

   a. **Sunucu Gezgini'nde,** oluşturduğunuz kapsayıcının düğüme çift tıklayın.

   b. **Kapsayıcı** penceresinde **Yükle** düğmesini seçin.

   ![Blob yükleme düğmesi](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. *Program.cs*bul up seçve sonra **Tamam'ı**seç.

1. Daha önce oluşturduğunuz sırada, iletinin metni olarak *Program.cs* bir sıra iletisi oluşturun.

   ![Sıra iletisi Program.cs](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

1. Projeyi yerel olarak çalıştırın.

   Sıra iletisi, daha sonra blob okur ve uzunluğunu günlükleri işlevi tetikler. Konsol çıkışı aşağıdaki gibi görünür:

   ```console
   Found the following functions:
   ConsoleApp1.Functions.ProcessQueueMessage
   Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   Blob name:Program.cs
   Size: 532 bytes
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   ```

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

Çıktı bağlamaları, veri yazan kodu basitleştirir. Bu örnek, boyutunu günlüğe kaydetmek yerine blob bir kopyasını yazarak önceki bir değiştirir. Blob depolama bağlamaları, daha önce yüklediğimiz Azure Depolama uzantısı paketine dahildir.

1. `ProcessQueueMessage` yöntemini aşağıdaki kod ile değiştirin:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       [Blob("container/copy-{queueTrigger}", FileAccess.Write)] Stream outputBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
       myBlob.CopyTo(outputBlob);
   }
   ```

1. İletimetni olarak *Program.cs* başka bir sıra iletisi oluşturun.

1. Projeyi yerel olarak çalıştırın.

   Sıra iletisi, daha sonra blob okur, uzunluğunu günlükleri ve yeni bir blob oluşturur işlevi tetikler. Konsol çıkışı aynıdır, ancak blob konteyner penceresine gidip **Yenile'yi**seçtiğinizde, *copy-Program.cs* adlı yeni bir leke görürsünüz.

## <a name="republish-the-updates-to-azure"></a>Güncelleştirmeleri Azure'da yeniden yayımlayın

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Yayımla** iletişim kutusunda, geçerli profilin seçildiğinden emin olun ve sonra **Yayımla'yı**seçin. Yayımlamanın sonuçları **Çıktı** penceresinde ayrıntılı olarak açıklanır.
 
1. Bir dosyayı blob kapsayıcısına yeniden yükleyerek ve yüklenen dosyanın adı olan kuyruğa bir ileti ekleyerek Azure'daki işlevi doğrulayın. İletinin kuyruktan kaldırıldığını ve blob kapsayıcısında oluşturulan dosyanın bir kopyasını görürsünüz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir WebJobs SDK 3.x projesi oluşturma, çalıştırma ve dağıtma gibi bir proje gösterilmektedir.

> [!div class="nextstepaction"]
> [WebJobs SDK hakkında daha fazla bilgi edinin](webjobs-sdk-how-to.md)
