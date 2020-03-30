---
title: Visual Studio ile Azure İşlevleri geliştirme
description: Visual Studio 2019 için Azure İşlevaraçları Araçlarını kullanarak Azure İşlevlerini nasıl geliştirip test edebilirsiniz öğrenin.
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: b3d683153a1e70f4c65dcc5e401f4ee702af8b49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277108"
---
# <a name="develop-azure-functions-using-visual-studio"></a>Visual Studio ile Azure İşlevleri geliştirme  

Visual Studio, C# sınıfı kitaplık işlevlerini Azure'a geliştirmenize, sınamanıza ve dağıtmanıza olanak tanır. Bu deneyim Azure İşlevleri ile ilk deneyiminizse, [Azure İşlevlerini tanıtma](functions-overview.md)bölümünden daha fazla bilgi edinebilirsiniz.

Visual Studio, işlevlerinizi geliştirirken aşağıdaki avantajları sağlar: 

* Yerel geliştirme bilgisayarınızdaki işlevleri edin, oluşturun ve çalıştırın. 
* Azure İşlevler projenizi doğrudan Azure'da yayınlayın ve gerektiğinde Azure kaynakları oluşturun. 
* Işlev bağlamalarını doğrudan C# kodunda bildirmek için C# özniteliklerini kullanın.
* Önceden derlenmiş C# işlevleri geliştirin ve dağıtın. Önceden uyulması yapılan işlevler, C# komut dosyası tabanlı işlevlere göre daha iyi bir soğuk başlangıç performansı sağlar. 
* Visual Studio geliştirmenin tüm avantajlarından yararlanırken işlevlerinizi C# ile kodlayın. 

Bu makalede, C# sınıfı kitaplık işlevlerini geliştirmek ve bunları Azure'da yayımlamak için Visual Studio'nun nasıl kullanılacağı hakkında ayrıntılı bilgi verilmektedir. Bu makaleyi okumadan önce Visual [Studio için Fonksiyonlar quickstart](functions-create-your-first-function-visual-studio.md)tamamlamanız gerekir. 

Aksi belirtilmedikçe, gösterilen prosedürler ve örnekler Visual Studio 2019 içindir. 

## <a name="prerequisites"></a>Ön koşullar

Azure İşlevaraçları, Visual Studio 2017'den itibaren Visual Studio'nun Azure geliştirme iş yüküne dahildir. Visual Studio yüklemenize **Azure geliştirme** iş yükünü eklediğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Yayımlama işlemi sırasında aboneliğinizde Azure Depolama hesabı gibi gereksinim duyduğunuz diğer kaynaklar oluşturulur.

> [!NOTE]
> Visual Studio 2017'de Azure geliştirme iş yükü Azure İşlev araçları'nı ayrı bir uzantı olarak yükler. Visual Studio 2017'nizi güncellediğinizde, Azure İşlevleri araçlarının [en son sürümünü](#check-your-tools-version) kullandığınızdan da emin olun. Aşağıdaki bölümler, Visual Studio 2017'de Azure İşlevaraçları uzantınızı nasıl denetleyip (gerekirse) güncelleştireceğimize işaret eder. 
>
> Visual Studio 2019'u kullanırken lütfen bu bölümü atlayın.

### <a name="check-your-tools-version-in-visual-studio-2017"></a><a name="check-your-tools-version"></a>Visual Studio 2017'de araç sürümünüzü kontrol edin

1. **Araçlar** menüsünden **Uzantılar ve Güncelleştirmeler'i**seçin. **Yüklü** > **Araçları** Genişletin ve Azure İş ve Web **İş Araçları'nı**seçin.

    ![Fonksiyonlar araçları sürümünü doğrulama](./media/functions-develop-vs/functions-vstools-check-functions-tools.png)

1. Yüklü **Sürüm'e**dikkat edin. Bu sürümü [sürüm notlarında](https://github.com/Azure/Azure-Functions/blob/master/VS-AzureTools-ReleaseNotes.md)listelenen en son sürümle karşılaştırabilirsiniz. 

1. Sürümünüz eskiyse, aşağıdaki bölümde gösterildiği gibi Visual Studio'daki araçlarınızı güncelleyin.

### <a name="update-your-tools-in-visual-studio-2017"></a>Visual Studio 2017'de araçlarınızı güncelleyin

1. **Uzantılar ve Güncelleştirmeler** iletişim kutusunda, **Güncelleştirmeler** > **Görsel Stüdyo Marketi'ni**genişletin, Azure **İşve İş Araçları'nı** seçin ve **Güncelleştir'i**seçin.

    ![Fonksiyonlar araçları sürümünü güncelleştirme](./media/functions-develop-vs/functions-vstools-update-functions-tools.png)   

1. Araçlar güncelleştirmesi indirildikten sonra, VSIX yükleyicisini kullanarak araç güncellemesini tetiklemek için Visual Studio'yu kapatın.

1. Yükleyicide, başlatmak için **Tamam'ı** seçin ve araçları güncelleştirmek için **değiştirin.** 

1. Güncelleştirme tamamlandıktan sonra **Kapat'ı** seçin ve Visual Studio'yu yeniden başlatın.

> [!NOTE]  
> Visual Studio 2019 ve sonraki bölümünde, Azure İşlevler araçları uzantısı Visual Studio'nun bir parçası olarak güncelleştirilir.  

## <a name="create-an-azure-functions-project"></a>Azure İşlevleri projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]

Proje şablonu bir C# projesi oluşturur, NuGet paketini `Microsoft.NET.Sdk.Functions` yükler ve hedef çerçeveyi ayarlar. Yeni projede aşağıdaki dosyalar vardır:

* **host.json**: İşlevler ana bilgisayarını yapılandırmanızı sağlar. Bu ayarlar hem yerel olarak çalışırken hem de Azure'da geçerlidir. Daha fazla bilgi için [host.json referansına](functions-host-json.md)bakın.

* **local.settings.json**: Yerel olarak işlevleri çalıştırırken kullanılan ayarları korur. Bu ayarlar Azure'da çalışırken kullanılmaz. Daha fazla bilgi için [Yerel ayarlar dosyasına](#local-settings-file)bakın.

    >[!IMPORTANT]
    >Local.settings.json dosyası sırlar içerebildiği için, dosyayı proje kaynak denetiminizden hariç tutmalısınız. Bu **dosyaiçin Çıktı Dizini** Kopyalama ayarı her zaman **kopya la olmalıdır.** 

Daha fazla bilgi için [bkz.](functions-dotnet-class-library.md#functions-class-library-project)

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Local.settings.json'daki ayarlar, projeyi yayımladığınızda otomatik olarak yüklenmez. Bu ayarların Azure'daki işlev uygulamanızda da bulunduğundan emin olmak için, projenizi yayımladıktan sonra yüklemeniz gerekir. Daha fazla bilgi için [Function uygulama ayarlarına](#function-app-settings)bakın.

**ConnectionStrings'deki** değerler hiçbir zaman yayımlanmaz.

İşlev uygulaması ayarları değerleri kodunuzda ortam değişkenleri olarak da okunabilir. Daha fazla bilgi için [Bkz. Çevre değişkenleri.](functions-dotnet-class-library.md#environment-variables)

## <a name="configure-the-project-for-local-development"></a>Projeyi yerel kalkınma için yapılandırın

İşlevler çalışma süresi dahili olarak bir Azure Depolama hesabı kullanır. HTTP ve webhooks dışındaki tüm tetikleyici türleri için **Values.AzureWebJobsStorage** anahtarını geçerli bir Azure Depolama hesabı bağlantı dizesi olarak ayarlamanız gerekir. İşlev uygulamanız, projenin gerektirdiği **AzureWebİşlerDepolama** bağlantı ayarı için [Azure depolama emülatörü](../storage/common/storage-use-emulator.md) de kullanabilir. Emülatörkullanmak için **AzureWebJobsStorage** değerini `UseDevelopmentStorage=true`. Dağıtımdan önce bu ayarı gerçek bir depolama hesabı bağlantı dizesine değiştirin.

Depolama hesabı bağlantı dizesini ayarlamak için:

1. Visual Studio'da, **Bulut Gezgini'ni**açın, **Depolama Hesabınızı** > **Your Storage Account**genişletin, ardından **Özellikler** sekmesinde Birincil **Bağlantı String** değerini kopyalayın.

2. Projenizde local.settings.json dosyasını açın ve kopyaladığınız bağlantı dizesinin **AzureWebJobsStorage** anahtarının değerini ayarlayın.

3. İşlevlerinizin gerektirdiği diğer bağlantılar için **Değerler** dizisine benzersiz anahtarlar eklemek için önceki adımı yineleyin. 

## <a name="add-a-function-to-your-project"></a>Projenize bir işlev ekleme

C# sınıfı kitaplık işlevlerinde, işlev tarafından kullanılan bağlamalar koddaki öznitelikler uygulanarak tanımlanır. Sağlanan şablonlardan işlev tetikleyicilerinizi oluşturduğunuzda, tetikleyici öznitelikleri sizin için uygulanır. 

1. **Çözüm Gezgini**’nde, proje düğümünüze sağ tıklayın ve **Yeni** > **Öğe Ekle**’yi seçin. **Azure İşlevi'ni**seçin, sınıf için bir **Ad** yazın ve **Ekle'yi**tıklatın.

2. Tetikleyicinizi seçin, bağlama özelliklerini ayarlayın ve **Oluştur'u**tıklatın. Aşağıdaki örnekte, Sıra depolama tetikleyici işlevi oluşturulurken ayarları gösterir. 

    ![Kuyruk tetiklenen işlev oluşturma](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)

    Bu tetikleyici örnek, **QueueStorage**adlı bir anahtara sahip bir bağlantı dizesi kullanır. Bu bağlantı dize ayarı [local.settings.json dosyasında](functions-run-local.md#local-settings-file)tanımlanmalıdır.

3. Yeni eklenen sınıfı inceleyin. **FunctionName** özniteliğiile atfedilen statik bir **Çalıştır** yöntemi görürsünüz. Bu öznitelik yöntem işlev için giriş noktası olduğunu gösterir.

    Örneğin, aşağıdaki C# sınıfı temel bir Sıra depolama tetikleyici işlevi temsil eder:

    ```csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.Extensions.Logging;

    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]
            public static void Run([QueueTrigger("myqueue-items", 
                Connection = "QueueStorage")]string myQueueItem, ILogger log)
            {
                log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    }
    ```

    Giriş noktası yöntemine sağlanan her bağlama parametresi için bağlamaya özgü bir öznitelik uygulanır. Öznitelik, bağlayıcı bilgileri parametre olarak alır. Önceki örnekte, ilk parametre, sıra tetiklenen işlevi gösteren bir **QueueTrigger** özniteliği uygulanmıştır. Sıra adı ve bağlantı dize ayarı **adı, QueueTrigger** özniteliğine parametre olarak geçirilir. Daha fazla bilgi için Azure [İşlevler için Azure Sırası depolama bağlamalarına](functions-bindings-storage-queue-trigger.md)bakın.

İşlev uygulaması projenize daha fazla işlev eklemek için yukarıdaki yordamı kullanabilirsiniz. Projedeki her işlevin farklı bir tetikleyicisi olabilir, ancak bir işlevin tam olarak bir tetikleyicisi olmalıdır. Daha fazla bilgi için Azure [İşlevleri tetikleyicileri ve bağlama kavramlarını](functions-triggers-bindings.md)görün.

## <a name="add-bindings"></a>Bağlama ekleme

Tetikleyicilerde olduğu gibi, giriş ve çıkış bağlamaları bağlayıcı öznitelikler olarak işlevinize eklenir. Bir işleve aşağıdaki gibi bağlamalar ekleyin:

1. [Projeyi yerel geliştirme için yapılandırdığınızdan](#configure-the-project-for-local-development)emin olun.

2. Belirli bağlama için uygun NuGet uzatma paketini ekleyin. Daha fazla bilgi için Tetikleyiciler ve Ciltler makalesinde [Visual Studio'yu kullanarak Yerel C# geliştirme](./functions-bindings-register.md#local-csharp) makalesine bakın. Bağlayıcıya özel NuGet paketi gereksinimleri, bağlayıcı için başvuru makalesinde bulunur. Örneğin, [Olay Hub'ları bağlama başvuru makalesinde](functions-bindings-event-hubs.md)Olay Hub'ları tetikleyicisi için paket gereksinimlerini bulun.

3. Bağlamanın ihtiyacı olan uygulama ayarları varsa, bunları yerel [ayar dosyasındaki](functions-run-local.md#local-settings-file) **Değerler** koleksiyonuna ekleyin. Bu değerler, işlev yerel olarak çalıştığında kullanılır. İşlev Azure'daki işlev uygulamasında çalıştığında, [işlev uygulaması ayarları](#function-app-settings) kullanılır.

4. Yöntem imzasına uygun bağlama özniteliğini ekleyin. Aşağıdaki örnekte, bir sıra iletisi işlevi tetikler ve çıktı bağlama farklı bir sırada aynı metin ile yeni bir sıra iletisi oluşturur.

    ```csharp
    public static class SimpleExampleWithOutput
    {
        [FunctionName("CopyQueueMessage")]
        public static void Run(
            [QueueTrigger("myqueue-items-source", Connection = "AzureWebJobsStorage")] string myQueueItem, 
            [Queue("myqueue-items-destination", Connection = "AzureWebJobsStorage")] out string myQueueItemCopy,
            ILogger log)
        {
            log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
            myQueueItemCopy = myQueueItem;
        }
    }
    ```
   Sıra depolama bağlantısı `AzureWebJobsStorage` ayarı elde edilir. Daha fazla bilgi için, belirli bağlama için başvuru makalesine bakın. 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="testing-functions"></a>İşlevleri test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

İşlevinizi test etmek için F5’e basın. İstenirse Visual Studio'dan gelen Azure İşlevleri Temel (CLI) araçlarını indirme ve yükleme isteğini kabul edin. Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

Proje çalışırken, dağıtılan işlevi sınarken kodunuzu sınayabilirsiniz. Daha fazla bilgi [için, kodunuzu Azure İşlevlerinde sınayın stratejileri'ne](functions-test-a-function.md)bakın. Hata ayıklama modunda çalışırken, beklendiği gibi Visual Studio'da kesme noktaları vurulur. 

<!---
For an example of how to test a queue triggered function, see the [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).  
-->

Azure İşlevler Temel Araçlarını kullanma hakkında daha fazla bilgi edinmek için [Kod'a bakın ve Azure işlevlerini yerel olarak test edin.](functions-run-local.md)

## <a name="publish-to-azure"></a>Azure’da Yayımlama

Visual Studio'dan yayımlanırken, iki dağıtım yönteminden biri kullanılır:

* [Web Deploy](functions-deployment-technologies.md#web-deploy-msdeploy): Windows uygulamalarını herhangi bir IIS sunucusuna paketler ve dağıtır.
* [Paketten Çalıştırma etkinken Zip Dağıtımı](functions-deployment-technologies.md#zip-deploy): Azure İşlevleri dağıtımları için önerilir.

Projenizi Azure'daki bir işlev uygulamasında yayınlamak için aşağıdaki adımları kullanın.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="function-app-settings"></a>İşlev uygulaması ayarları

local.settings.json'a eklediğiniz tüm ayarlar Azure'daki işlev uygulamasına da eklenmelidir. Projeyi yayımladığınızda bu ayarlar otomatik olarak yüklenmiyor.

Gerekli ayarları Azure'daki işlev uygulamanıza yüklemenin en kolay yolu, projenizi başarıyla yayımladıktan sonra görüntülenen **Uygulama Ayarlarını Yönet...** bağlantısını kullanmaktır.

![](./media/functions-develop-vs/functions-vstools-app-settings.png)

Bu, yeni uygulama ayarları ekleyebileceğiniz veya varolanları değiştirebileceğiniz işlev uygulaması için **Uygulama Ayarları** iletişim kutusunu görüntüler.

![](./media/functions-develop-vs/functions-vstools-app-settings2.png)

**Yerel yerel** ayarlar.json dosyasında bir ayar değerini temsil eder ve **Remote,** Azure'daki işlev uygulamasındageçerli ayardır.  Yeni bir uygulama ayarı oluşturmak için **Ayar Ekle'yi** seçin. Uzak **alana** bir ayar değeri kopyalamak için **Yerel bağlantıdan Ekle değerini** kullanın. **Bekleyen değişiklikler, Tamam'ı**seçtiğinizde yerel ayarlar dosyasına ve işlev uygulamasına yazılır.

> [!NOTE]
> Varsayılan olarak, local.settings.json dosyası kaynak denetiminde denetlenmez. Bu, kaynak denetiminden yerel bir İşlevler projesini klonladığınızda, projenin yerel ayarlar.json dosyası olmadığı anlamına gelir. Bu durumda, **Uygulama Ayarları** iletişim kutusunun beklendiği gibi çalışması için proje kökünde local.settings.json dosyasını el ile oluşturmanız gerekir. 

Uygulama ayarlarını şu yollardan biriyle de yönetebilirsiniz:

* [Azure portalını kullanma](functions-how-to-use-azure-function-app-settings.md#settings).
* [Azure `--publish-local-settings` İşlevler Temel Araçları'nda yayımlama seçeneğini kullanma.](functions-run-local.md#publish)
* [Azure CLI'yi kullanma](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set).

## <a name="monitoring-functions"></a>İzleme fonksiyonları

İşlevlerinizin yürütülmesini izlemenin önerilen yolu, işlev uygulamanızı Azure Application Insights ile tümleştirmektir. Azure portalında bir işlev uygulaması oluşturduğunuzda, bu tümleştirme varsayılan olarak sizin için yapılır. Ancak, Visual Studio yayımlama sırasında işlev uygulamanızı oluşturduğunuzda, Azure'daki işlev uygulamanızda tümleştirme yapılmaz.

İşlev uygulamanız için Uygulama Öngörüleri'ni etkinleştirmek için:

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Daha fazla bilgi için Azure [İşlerini İzle'ye](functions-monitoring.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevler Temel Araçları hakkında daha fazla bilgi edinmek için [Kod'a bakın ve Azure işlevlerini yerel olarak test edin.](functions-run-local.md)

.NET sınıf kitaplıkları gibi işlevler geliştirme hakkında daha fazla bilgi edinmek için [Azure İşlevler C# geliştirici başvurusuna](functions-dotnet-class-library.md)bakın. Bu makalede, Azure İşlevler tarafından desteklenen çeşitli bağlama türlerini bildirmek için özniteliklerin nasıl kullanılacağına iliş eden örneklerde de bağlantılar yer alır.    
