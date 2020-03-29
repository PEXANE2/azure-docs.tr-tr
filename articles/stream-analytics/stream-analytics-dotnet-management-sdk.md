---
title: Azure Akışı Analizi için Yönetim .NET SDK
description: Stream Analytics Management .NET SDK ile başlayın. Analitik işlerini nasıl ayarlayıp çalıştırılacıöğreneceksiniz. Proje, girdiler, çıktılar ve dönüşümler oluşturun.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 20be2c56635faa4f77ae8e8e6afc3c1ece6d4942
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426270"
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Management .NET SDK: .NET için Azure Akışı Analitik API'sini kullanarak analitik işleri ayarlama ve çalıştırma
Management .NET SDK'yı kullanarak .NET için Stream Analytics API'sini kullanarak analitik işlerini nasıl ayarlayıp çalıştırışta oylaçalışırım öğrenin. Bir proje ayarlayın, girdi ve çıktı kaynakları, dönüşümler oluşturun ve işleri başlatıp durdurun. Analitik işleriniz için, Verileri Blob depolamadan veya bir etkinlik merkezinden aktarabilirsiniz.

[.NET için Stream Analytics API'si için yönetim başvuru belgelerine](https://msdn.microsoft.com/library/azure/dn889315.aspx)bakın.

Azure Akış Analizi, bulutta veri akışı üzerinden düşük gecikmeli, yüksek oranda kullanılabilir, ölçeklenebilir, karmaşık olay işleme sağlayan tam olarak yönetilen bir hizmettir. Akış Analizi, müşterilerin veri akışlarını analiz etmek için akış işleri ayarlamalarına ve gerçek zamanlı analizlere yaklaşmalarına olanak tanır.  

> [!NOTE]
> Bu makalede örnek kodu Azure Stream Analytics Management .NET SDK v2.x sürümüyle güncelledik. Kullanım ları lagecy (1.x) SDK sürümünü kullanan örnek kod için lütfen [Stream Analytics için Yönetimi .NET SDK v1.x'i kullanın.](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1)

## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdaki gereksinimleriniz olmalıdır:

* Visual Studio 2019 veya 2015'i yükleyin.
* [Azure .NET SDK](https://azure.microsoft.com/downloads/)’yı indirip yükleyin.
* Aboneliğinizde bir Azure Kaynak Grubu oluşturun. Aşağıdaki örnek, örnek bir Azure PowerShell komut dosyasıdır. Azure PowerShell bilgileri için Azure [PowerShell'i Yükle ve yapılandırma](/powershell/azure/overview)(  

   ```powershell
   # Log in to your Azure account
   Add-AzureAccount
   
   # Select the Azure subscription you want to use to create the resource group
   Select-AzureSubscription -SubscriptionName <subscription name>
   
   # If Stream Analytics has not been registered to the subscription, remove the remark    symbol (#) to run the Register-AzProvider cmdlet to register the provider namespace
   #Register-AzProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
   
   # Create an Azure resource group
   New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
   ```

* İş in bağlanması için bir giriş kaynağı ve çıktı hedefi ayarlayın.

## <a name="set-up-a-project"></a>Proje ayarlama
Bir analiz işi oluşturmak için ,.NET için Stream Analytics API'sini kullanın ve önce projenizi ayarlayın.

1. Visual Studio C# .NET konsol uygulaması oluşturun.
2. Paket Yöneticisi Konsolunda, NuGet paketlerini yüklemek için aşağıdaki komutları çalıştırın. Bunlardan ilki Azure Akış Analizi Yönetimi .NET SDK'dır. İkincisi Azure istemci kimlik doğrulaması içindir.

   ```powershell   
   Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
   Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
   ```

3. App.config dosyasına aşağıdaki **appAyarlar** bölümünü ekleyin:
   
   ```powershell
   <appSettings>
       <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
       <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
       <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
       <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```

    **SubscriptionId** ve **ActiveDirectoryTenantId** değerlerini Azure aboneliğiniz ve kiracı kimlikleriniz ile değiştirin. Aşağıdaki Azure PowerShell cmdlet'ini çalıştırarak bu değerleri elde edebilirsiniz:

   ```powershell
      Get-AzureAccount
   ```

4. .csproj dosyanıza aşağıdaki başvuruyu ekleyin:

   ```csharp
   <Reference Include="System.Configuration" />
   ```

5. Projedeki kaynak dosyaya (Program.cs) aşağıdaki ifadeleri **ekleyin:**
   
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Configuration;
   using System.Threading;
   using System.Threading.Tasks;
   
   using Microsoft.Azure.Management.StreamAnalytics;
   using Microsoft.Azure.Management.StreamAnalytics.Models;
   using Microsoft.Rest.Azure.Authentication;
   using Microsoft.Rest;
   ```

6. Kimlik doğrulama yardımcı yöntemi ekleyin:

   ```csharp
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a>Akış Analizi yönetim istemcisi oluşturma
**StreamAnalyticsManagementClient** nesnesi, iş ve giriş, çıktı ve dönüşüm gibi iş bileşenlerini yönetmenize olanak tanır.

**Ana** yöntemin başına aşağıdaki kodu ekleyin:

   ```csharp
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamingJobName = "<YOUR STREAMING JOB NAME>";
    string inputName = "<YOUR JOB INPUT NAME>";
    string transformationName = "<YOUR JOB TRANSFORMATION NAME>";
    string outputName = "<YOUR JOB OUTPUT NAME>";
    
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    
    // Get credentials
    ServiceClientCredentials credentials = GetCredentials().Result;
    
    // Create Stream Analytics management client
    StreamAnalyticsManagementClient streamAnalyticsManagementClient = new StreamAnalyticsManagementClient(credentials)
    {
        SubscriptionId = ConfigurationManager.AppSettings["SubscriptionId"]
    };
   ```

**ResourceGroupName** değişkeninin değeri, ön koşul adımlarında oluşturduğunuz veya seçtiğiniz kaynak grubunun adı ile aynı olmalıdır.

İş oluşturmanın kimlik bilgisi sunusu yönünü otomatikleştirmek için, [Azure Kaynak Yöneticisi ile bir hizmet yöneticisinin kimlik doğrulamasına](../active-directory/develop/howto-authenticate-service-principal-powershell.md)bakın.

Bu makalenin kalan bölümleri, bu kodun **Ana** yöntemin başında olduğunu varsayar.

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma
Aşağıdaki kod, tanımladığınız kaynak grubu altında bir Akış Analizi işi oluşturur. Daha sonra işe bir giriş, çıktı ve dönüşüm eklersiniz.

   ```csharp
   // Create a streaming job
   StreamingJob streamingJob = new StreamingJob()
   {
       Tags = new Dictionary<string, string>()
       {
           { "Origin", ".NET SDK" },
           { "ReasonCreated", "Getting started tutorial" }
       },
       Location = "West US",
       EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Drop,
       EventsOutOfOrderMaxDelayInSeconds = 5,
       EventsLateArrivalMaxDelayInSeconds = 16,
       OutputErrorPolicy = OutputErrorPolicy.Drop,
       DataLocale = "en-US",
       CompatibilityLevel = CompatibilityLevel.OneFullStopZero,
       Sku = new Sku()
       {
           Name = SkuName.Standard
       }
   };
   StreamingJob createStreamingJobResult = streamAnalyticsManagementClient.StreamingJobs.CreateOrReplace(streamingJob, resourceGroupName, streamingJobName);
   ```

## <a name="create-a-stream-analytics-input-source"></a>Akış Analizi giriş kaynağı oluşturma
Aşağıdaki kod, blob giriş kaynak türü ve CSV serileştirme ile bir Akış Analizi giriş kaynağı oluşturur. Bir olay hub giriş kaynağı oluşturmak için **BlobStreamInputDataSource**yerine **EventHubStreamInputDataSource'u** kullanın. Benzer şekilde, giriş kaynağının serileştirme türünü özelleştirebilirsiniz.

   ```csharp
   // Create an input
   StorageAccount storageAccount = new StorageAccount()
   {
       AccountName = "<YOUR STORAGE ACCOUNT NAME>",
       AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
   };
   Input input = new Input()
   {
       Properties = new StreamInputProperties()
       {
           Serialization = new CsvSerialization()
           {
               FieldDelimiter = ",",
               Encoding = Encoding.UTF8
           },
           Datasource = new BlobStreamInputDataSource()
           {
               StorageAccounts = new[] { storageAccount },
               Container = "<YOUR STORAGE BLOB CONTAINER>",
               PathPattern = "{date}/{time}",
               DateFormat = "yyyy/MM/dd",
               TimeFormat = "HH",
               SourcePartitionCount = 16
           }
       }
   };
   Input createInputResult = streamAnalyticsManagementClient.Inputs.CreateOrReplace(input, resourceGroupName, streamingJobName, inputName);
   ```

Blob depolama veya olay hub olsun, giriş kaynakları, belirli bir işe bağlıdır. Farklı işler için aynı giriş kaynağını kullanmak için yöntemi yeniden aramanız ve farklı bir iş adı belirtmeniz gerekir.

## <a name="test-a-stream-analytics-input-source"></a>Akış Analizi giriş kaynağını test edin
**TestConnection** yöntemi, Akış Analizi işinin giriş kaynağına ve giriş kaynağı türüne özgü diğer yönlerine bağlanıp bağlanamayacağını test eder. Örneğin, daha önceki bir adımda oluşturduğunuz blob giriş kaynağında, yöntem Depolama hesabı ve anahtar çiftinin Depolama hesabına bağlanmak için kullanılAbilir olup olmasını ve belirtilen kapın var olup olmasını denetler.

   ```csharp
   // Test the connection to the input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a>Akış Analizi çıkış hedefi oluşturma
Çıktı hedefi oluşturmak, Akış Analizi giriş kaynağı oluşturmaya benzer. Girdi kaynakları gibi, çıktı hedefleri de belirli bir işe bağlıdır. Farklı işler için aynı çıktı hedefini kullanmak için yöntemi yeniden aramanız ve farklı bir iş adı belirtmeniz gerekir.

Aşağıdaki kod bir çıktı hedefi (Azure SQL veritabanı) oluşturur. Çıktı hedefinin veri türünü ve/veya serileştirme türünü özelleştirebilirsiniz.

   ```csharp
   // Create an output
   Output output = new Output()
   {
       Datasource = new AzureSqlDatabaseOutputDataSource()
       {
           Server = "<YOUR DATABASE SERVER NAME>",
           Database = "<YOUR DATABASE NAME>",
           User = "<YOUR DATABASE LOGIN>",
           Password = "<YOUR DATABASE LOGIN PASSWORD>",
           Table = "<YOUR DATABASE TABLE NAME>"
       }
   };
   Output createOutputResult = streamAnalyticsManagementClient.Outputs.CreateOrReplace(output, resourceGroupName, streamingJobName, outputName);
   ```

## <a name="test-a-stream-analytics-output-target"></a>Akış Analizi çıkış hedefini test edin
Akış Analizi çıkış hedefi, bağlantıları test etmek için **TestConnection** yöntemine de sahiptir.

   ```csharp
   // Test the connection to the output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a>Akış Analizi dönüşümü oluşturun
Aşağıdaki kod, "Girişten * seç" sorgusuyla bir Akış Analizi dönüşümü oluşturur ve Stream Analytics işi için bir akış birimi ayırmayı belirtir. Akış birimlerini ayarlama hakkında daha fazla bilgi için Azure [Akış Analizi işlerini ölçeklendir'e](stream-analytics-scale-jobs.md)bakın.

   ```csharp
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with the value you put for the 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

Giriş ve çıktı gibi, bir dönüşüm de altında oluşturulduğu belirli Stream Analytics işine bağlıdır.

## <a name="start-a-stream-analytics-job"></a>Stream Analytics işini başlatma
Bir Akış Analizi işi ve girdi(ler), çıktı(lar) ve dönüşüm oluşturduktan sonra **Başlat** yöntemini çağırarak işe başlayabilirsiniz.

Aşağıdaki örnek kod, 12 Aralık 2012, 12:12:12 UTC olarak ayarlanmış özel bir çıktı başlangıç saati ile bir Stream Analytics işini başlatır:

   ```csharp
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a>Akış Analizi işini durdurun
Stop **yöntemini** arayarak çalışan bir Akış Analizi işini durdurabilirsiniz.

   ```csharp
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a>Akış Analizi işini silme
**Sil** yöntemi, işin yanı sıra giriş(ler, çıktı(lar) ve işin dönüşümü de dahil olmak üzere temel alt kaynakları da siler.

   ```csharp
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a>Destek alın
Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Analitik işler oluşturmak ve çalıştırmak için .NET SDK kullanmanın temellerini öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akışı Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: https://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: https://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: https://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
