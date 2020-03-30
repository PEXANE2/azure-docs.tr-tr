---
title: Azure Uygulama Öngörüleri ile Toplu İşlemi İzle | Microsoft Dokümanlar
description: Azure Application Insights kitaplığını kullanarak bir Azure Toplu İş .NET uygulamasını nasıl enstrüman edebilirsiniz öğrenin.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.devlang: .NET
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: labrenne
ms.openlocfilehash: b1f4fb0207d4f659861dbd3fdfd1b2d502409935
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022469"
---
# <a name="monitor-and-debug-an-azure-batch-net-application-with-application-insights"></a>Uygulama Öngörüleri ile Azure Toplu İşlem .NET uygulamasını izleme ve hata ayıklama

[Application Insights,](../azure-monitor/app/app-insights-overview.md) geliştiricilerin Azure hizmetlerine dağıtılan uygulamaları izlemesi ve hata ayıklamaları için zarif ve güçlü bir yol sağlar. Performans sayaçlarını ve özel durumlarını izlemek ve kodunuzu özel ölçümler ve izlemeyle kullanmak için Uygulama Öngörüleri'ni kullanın. Uygulama Öngörülerini Azure Toplu Uygulamanızla bütünleştirmek, davranışlar hakkında derin bilgiler edinmenize ve sorunları neredeyse gerçek zamanlı olarak araştırmanıza olanak tanır.

Bu makalede, Uygulama Öngörüleri kitaplığını Azure Toplu İşlem .NET çözümünüze nasıl ekleyeceğiniz ve yapılandırılabilmek te ve uygulama kodunuzu nasıl ayarlayabilirsiniz. Ayrıca, Azure portalı üzerinden uygulamanızı izlemenin ve özel panolar oluşturmanın yollarını da gösterir. Diğer dillerdeki Uygulama Öngörüleri desteği için [dillere, platformlara ve tümleştirme belgelerine](../azure-monitor/app/platforms.md)bakın.

Bu makaleye eşlik edecek kodlu örnek bir C# çözümü [GitHub'da](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights)mevcuttur. Bu örnek, [TopNWords](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords) örneğine Application Insights enstrümantasyon kodunu ekler. Bu örneğe aşina değilseniz, önce TopNWords oluşturmayı ve çalıştırmayı deneyin. Bunu yapmak, birden çok işlem düğümüne paralel olarak bir giriş blobları kümesini işlemenin temel Toplu iş akışını anlamanıza yardımcı olur. 

> [!TIP]
> Alternatif olarak, Toplu İşlem çözümünüzü Toplu Explorer'da VM performans sayaçları gibi Application Insights verilerini görüntülemek için yapılandırın. [Toplu İşlem Gezgini,](https://github.com/Azure/BatchExplorer) Azure Toplu İş uygulamaları nın oluşturulmasına, hata ayıklanmasına ve izlenmesine yardımcı olan ücretsiz, zengin özellikli, bağımsız bir istemci aracıdır. Mac, Linux veya Windows için [yükleme paketi](https://azure.github.io/BatchExplorer/) indirebilirsiniz. Toplu İş Explorer'da Application Insights verilerini etkinleştirmek için hızlı adımlar için [toplu öngörüler repo'ya](https://github.com/Azure/batch-insights) bakın. 
>

## <a name="prerequisites"></a>Ön koşullar
* [Visual Studio 2017 veya sonrası](https://www.visualstudio.com/vs)

* [Toplu iş hesabı ve bağlantılı depolama hesabı](batch-account-create-portal.md)

* [Application Insights kaynağı](../azure-monitor/app/create-new-resource.md )
  
   * Uygulama Öngörüleri *kaynağı*oluşturmak için Azure portalını kullanın. Genel *General* **Uygulama türünü**seçin.

   * [Enstrümantasyon anahtarını](../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key) portaldan kopyalayın. Daha sonra bu makalede gereklidir.
  
  > [!NOTE]
  > Application Insights'ta depolanan veriler için [ücretlendirilebilirsiniz.](https://azure.microsoft.com/pricing/details/application-insights/) Bu, bu makalede tartışılan tanılama ve izleme verilerini içerir.
  > 

## <a name="add-application-insights-to-your-project"></a>Projenize Uygulama Öngörüleri Ekleyin

**Microsoft.ApplicationInsights.WindowsServer** NuGet paketi ve bağımlılıkları projeniz için gereklidir. Bunları uygulamanızın projesine ekleyin veya geri yükleyin. Paketi yüklemek için komutu veya NuGet Paket Yöneticisi'ni `Install-Package` kullanın.

```powershell
Install-Package Microsoft.ApplicationInsights.WindowsServer
```
**Microsoft.ApplicationInsights** ad alanını kullanarak .NET uygulamanızdan Başvuru Uygulama Öngörüleri.

## <a name="instrument-your-code"></a>Kodunuzu enstrüman

Kodunuzu enstrüman için, çözüm bir Uygulama Öngörüleri [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient)oluşturmak gerekir. Örnekte, TelemetryClient yapılandırmasını [ApplicationInsights.config](../azure-monitor/app/configuration-with-applicationinsights-config.md) dosyasından yükler. ApplicationInsights.config'i Application Insights enstrümantasyon anahtarınızla aşağıdaki projelerde güncelleştirin: Microsoft.Azure.Batch.Sample.TelemetryStartTask ve TopNWordsSample.

```xml
<InstrumentationKey>YOUR-IKEY-GOES-HERE</InstrumentationKey>
```
Ayrıca dosyaya enstrümantasyon anahtarını TopNWords.cs ekleyin.

TopNWords.cs'daki örnek, Application Insights API'den aşağıdaki [enstrümantasyon çağrılarını](../azure-monitor/app/api-custom-events-metrics.md) kullanır:
* `TrackMetric()`- Bir işlem düğümünün gerekli metin dosyasını indirmek için ortalama olarak ne kadar sürdüğünü izler.
* `TrackTrace()`- Kodunuza hata ayıklama çağrıları ekler.
* `TrackEvent()`- Yakalamak için ilginç olayları izler.

Bu örnek, özel durum işlemeyi kasten dışarıda bırakır. Bunun yerine, Application Insights işlenmemiş özel durumları otomatik olarak raporlar ve bu da hata ayıklama deneyimini önemli ölçüde geliştirir. 

Aşağıdaki parçacık, bu yöntemlerin nasıl kullanılacağını göstermektedir.

```csharp
public void CountWords(string blobName, int numTopN, string storageAccountName, string storageAccountKey)
{
    // simulate exception for some set of tasks
    Random rand = new Random();
    if (rand.Next(0, 10) % 10 == 0)
    {
        blobName += ".badUrl";
    }

    // log the url we are downloading the file from
    insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Download file from: {1}", this.taskId, blobName), SeverityLevel.Verbose));

    // open the cloud blob that contains the book
    var storageCred = new StorageCredentials(storageAccountName, storageAccountKey);
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName), storageCred);
    using (Stream memoryStream = new MemoryStream())
    {
        // calculate blob download time
        DateTime start = DateTime.Now;
        blob.DownloadToStream(memoryStream);
        TimeSpan downloadTime = DateTime.Now.Subtract(start);

        // track how long the blob takes to download on this node
        // this will help debug timing issues or identify poorly performing nodes
        insightsClient.TrackMetric("Blob download in seconds", downloadTime.TotalSeconds, this.CommonProperties);

        memoryStream.Position = 0; //Reset the stream
        var sr = new StreamReader(memoryStream);
        var myStr = sr.ReadToEnd();
        string[] words = myStr.Split(' ');

        // log how many words were found in the text file
        insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Found {1} words", this.taskId, words.Length), SeverityLevel.Verbose));
        var topNWords =
            words.
                Where(word => word.Length > 0).
                GroupBy(word => word, (key, group) => new KeyValuePair<String, long>(key, group.LongCount())).
                OrderByDescending(x => x.Value).
                Take(numTopN).
                ToList();
        foreach (var pair in topNWords)
        {
            Console.WriteLine("{0} {1}", pair.Key, pair.Value);
        }

        // emit an event to track the completion of the task
        insightsClient.TrackEvent("Done counting words");
    }
}
```

### <a name="azure-batch-telemetry-initializer-helper"></a>Azure Toplu telemetri başlandırıcı yardımcı
Belirli bir sunucu ve örnek için telemetri raporederken, Application Insights varsayılan değerler için Azure VM Rolünü ve VM adını kullanır. Azure Toplu İş bağlamında, örnek havuz adının nasıl kullanılacağını ve bunun yerine düğüm adının nasıl kullanılacağını gösterir. Varsayılan değerleri geçersiz kılmak için [bir telemetri başharfi](../azure-monitor/app/api-filtering-sampling.md#add-properties) kullanın. 

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;
using System;
using System.Threading;

namespace Microsoft.Azure.Batch.Samples.TelemetryInitializer
{
    public class AzureBatchNodeTelemetryInitializer : ITelemetryInitializer
    {
        // Azure Batch environment variables
        private const string PoolIdEnvironmentVariable = "AZ_BATCH_POOL_ID";
        private const string NodeIdEnvironmentVariable = "AZ_BATCH_NODE_ID";

        private string roleInstanceName;
        private string roleName;

        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                // override the role name with the Azure Batch Pool name
                string name = LazyInitializer.EnsureInitialized(ref this.roleName, this.GetPoolName);
                telemetry.Context.Cloud.RoleName = name;
            }

            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleInstance))
            {
                // override the role instance with the Azure Batch Compute Node name
                string name = LazyInitializer.EnsureInitialized(ref this.roleInstanceName, this.GetNodeName);
                telemetry.Context.Cloud.RoleInstance = name;
            }
        }

        private string GetPoolName()
        {
            return Environment.GetEnvironmentVariable(PoolIdEnvironmentVariable) ?? string.Empty;
        }

        private string GetNodeName()
        {
            return Environment.GetEnvironmentVariable(NodeIdEnvironmentVariable) ?? string.Empty;
        }
    }
}
```

Telemetri başharfini etkinleştirmek için, TopNWordsSample projesindeki ApplicationInsights.config dosyası aşağıdakileri içerir:

```xml
<TelemetryInitializers>
    <Add Type="Microsoft.Azure.Batch.Samples.TelemetryInitializer.AzureBatchNodeTelemetryInitializer, Microsoft.Azure.Batch.Samples.TelemetryInitializer"/>
</TelemetryInitializers>
``` 

## <a name="update-the-job-and-tasks-to-include-application-insights-binaries"></a>İş ve görevleri Uygulama Öngörüleri ikililerini içerecek şekilde güncelleştirme

Uygulama Öngörüleri'nin bilgi işlem düğümlerinizde doğru çalışması için ikili lerin doğru yerleştirildiğinden emin olun. Görevin yürütüldettiği anda indirilsinler diye görevin kaynak dosyaları koleksiyonuna gerekli ikilileri ekleyin. Aşağıdaki parçacıklar Job.cs'daki koda benzer.

İlk olarak, yüklenmesi gereken Uygulama Öngörüleri dosyalarının statik bir listesini oluşturun.

```csharp
private static readonly List<string> AIFilesToUpload = new List<string>()
{
    // Application Insights config and assemblies
    "ApplicationInsights.config",
    "Microsoft.ApplicationInsights.dll",
    "Microsoft.AI.Agent.Intercept.dll",
    "Microsoft.AI.DependencyCollector.dll",
    "Microsoft.AI.PerfCounterCollector.dll",
    "Microsoft.AI.ServerTelemetryChannel.dll",
    "Microsoft.AI.WindowsServer.dll",
    
    // custom telemetry initializer assemblies
    "Microsoft.Azure.Batch.Samples.TelemetryInitializer.dll",
 };
...
```

Ardından, görev tarafından kullanılan hazırlama dosyalarını oluşturun.
```csharp
...
// create file staging objects that represent the executable and its dependent assembly to run as the task.
// These files are copied to every node before the corresponding task is scheduled to run on that node.
FileToStage topNWordExe = new FileToStage(TopNWordsExeName, stagingStorageAccount);
FileToStage storageDll = new FileToStage(StorageClientDllName, stagingStorageAccount);

// Upload Application Insights assemblies
List<FileToStage> aiStagedFiles = new List<FileToStage>();
foreach (string aiFile in AIFilesToUpload)
{
    aiStagedFiles.Add(new FileToStage(aiFile, stagingStorageAccount));
}
...
```

Yöntem, `FileToStage` kod örneğindeki bir yardımcı işlevdir ve yerel diskten Azure Depolama blob'una kolayca dosya yüklemenize olanak tanır. Her dosya daha sonra bir işlem düğümüne indirilir ve bir görev tarafından başvurulur.

Son olarak, görevleri işe ekleyin ve gerekli Application Insights ikililerini ekleyin.
```csharp
...
// initialize a collection to hold the tasks that will be submitted in their entirety
List<CloudTask> tasksToRun = new List<CloudTask>(topNWordsConfiguration.NumberOfTasks);
for (int i = 1; i <= topNWordsConfiguration.NumberOfTasks; i++)
{
    CloudTask task = new CloudTask("task_no_" + i, String.Format("{0} --Task {1} {2} {3} {4}",
        TopNWordsExeName,
        string.Format("https://{0}.blob.core.windows.net/{1}",
            accountSettings.StorageAccountName,
            documents[i]),
        topNWordsConfiguration.TopWordCount,
        accountSettings.StorageAccountName,
        accountSettings.StorageAccountKey));

    //This is the list of files to stage to a container -- for each job, one container is created and 
    //files all resolve to Azure Blobs by their name (so two tasks with the same named file will create just 1 blob in
    //the container).
    task.FilesToStage = new List<IFileStagingProvider>
                        {
                            // required application binaries
                            topNWordExe,
                            storageDll,
                        };
    foreach (FileToStage stagedFile in aiStagedFiles)
   {
        task.FilesToStage.Add(stagedFile);
   }    
    task.RunElevated = false;
    tasksToRun.Add(task);
}
```

## <a name="view-data-in-the-azure-portal"></a>Azure portalında verileri görüntüleme

Artık uygulamayı ve görevleri Uygulama Öngörüleri'ni kullanacak şekilde yapılandırdığınıza göre, havuzunuzda örnek işi çalıştırın. Azure portalına gidin ve size sağlanan Application Insights kaynağını açın. Havuz sağlandıktan sonra, verilerin aktığını ve günlüğe kaydedildiğini görmeye başlamalısınız. Bu makalenin geri kalanı yalnızca birkaç Application Insights özelliğine değinir, ancak tam özellik kümesini keşfetmekten çekinmeyin.

### <a name="view-live-stream-data"></a>Canlı akış verilerini görüntüleme

Applications Insights kaynağınızdaki izleme günlüklerini görüntülemek için **Canlı Akış'ı**tıklatın. Aşağıdaki ekran görüntüsü, havuzdaki bilgi işlem düğümlerinden gelen canlı verilerin nasıl görüntülenebildiğini (örneğin, işlem düğümü başına CPU kullanımı) gösterir.

![Canlı akış bilgi işlem düğümü verileri](./media/monitor-application-insights/applicationinsightslivestream.png)

### <a name="view-trace-logs"></a>İzleme günlüklerini görüntüleme

Uygulamalar Öngörüler kaynağınızdaki izleme günlüklerini görüntülemek için **Ara'yı**tıklatın. Bu görünüm, izlemeler, olaylar ve özel durumlar da dahil olmak üzere Application Insights tarafından yakalanan tanılama verilerinin bir listesini gösterir. 

Aşağıdaki ekran görüntüsü, bir görev için tek bir izlemenin nasıl günlüğe kaydedildiğini ve daha sonra hata ayıklama amacıyla nasıl sorgulanır olduğunu gösterir.

![İzleme günlükleri görüntüsü](./media/monitor-application-insights/tracelogsfortask.png)

### <a name="view-unhandled-exceptions"></a>Işlenmemiş özel durumları görüntüleme

Aşağıdaki ekran görüntüleri, Application Insights'ın uygulamanızdan atılan özel durumları nasıl günlüğe kaydedeyılamalarını gösterir. Bu durumda, uygulamanın özel durum atmasından saniyeler içinde, belirli bir özel durum içine delinebilir ve sorunu tanılayabilirsiniz.

![İşlenmeyen özel durumlar](./media/monitor-application-insights/exception.png)

### <a name="measure-blob-download-time"></a>Blob indirme süresini ölçün

Özel ölçümler de portalda değerli bir araçtır. Örneğin, işlemesi gereken metin dosyasını indirmek için her işlem düğümünün aldığı ortalama süreyi görüntüleyebilirsiniz.

Örnek grafik oluşturmak için:
1. Uygulama Öngörüleri kaynağınızda, **Metrikler Gezgini** > **Ekle grafiğini**tıklatın.
2. Eklenen grafikte **Edit'i** tıklatın.
2. Grafik ayrıntılarını aşağıdaki gibi güncelleştirin:
   * **Grafik türünü** **Grid'e**ayarla.
   * **Toplamayı** **Ortalama**olarak ayarlayın.
   * **Grubu** **NodeId**olarak ayarlayın.
   * **Ölçümlerde,** saniyeler içinde **Özel** > **Blob indirme'yi**seçin.
   * Ekran **Renk paletini** seçtiğinize göre ayarlayın. 

![Düğüm başına blob indirme süresi](./media/monitor-application-insights/blobdownloadtime.png)


## <a name="monitor-compute-nodes-continuously"></a>İşlem düğümlerini sürekli olarak izleme

Performans sayaçları da dahil olmak üzere tüm ölçümlerin yalnızca görevler çalışırken günlüğe kaydedildiğini fark etmiş olabilirsiniz. Bu davranış, Application Insights günlüklerinin veri miktarını sınırladığı için yararlıdır. Ancak, her zaman işlem düğümlerini izlemek istediğiniz durumlar vardır. Örneğin, Toplu İşlem hizmeti üzerinden zamanlanmamış arka plan çalışmasını çalıştırıyor olabilirler. Bu durumda, işlem düğümünün ömrü için çalışacak bir izleme işlemi ayarlayın. 

Bu davranışı elde etmenin bir yolu, Application Insights kitaplığını yükler ve arka planda çalışan bir işlem yumurtlamaktır. Örnekte, başlangıç görevi ikilileri makineye yükler ve bir işlemin süresiz çalışmasını sağlar. Bu işlem için Application Insights yapılandırma dosyasını, performans sayaçları gibi ilgilendiğiniz ek verileri yandırmak için yapılandırın.

```csharp
...
 // Batch start task telemetry runner
private const string BatchStartTaskFolderName = "StartTask";
private const string BatchStartTaskTelemetryRunnerName = "Microsoft.Azure.Batch.Samples.TelemetryStartTask.exe";
private const string BatchStartTaskTelemetryRunnerAIConfig = "ApplicationInsights.config";
...
CloudPool pool = client.PoolOperations.CreatePool(
    topNWordsConfiguration.PoolId,
    targetDedicated: topNWordsConfiguration.PoolNodeCount,
    virtualMachineSize: "standard_d1_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));
...

// Create a start task which will run a dummy exe in background that simply emits performance
// counter data as defined in the relevant ApplicationInsights.config.
// Note that the waitForSuccess on the start task was not set so the Compute Node will be
// available immediately after this command is run.
pool.StartTask = new StartTask()
{
    CommandLine = string.Format("cmd /c {0}", BatchStartTaskTelemetryRunnerName),
    ResourceFiles = resourceFiles
};
...
```

> [!TIP]
> Çözümünüzün yönetilebilirliğini artırmak için, montajı bir [uygulama paketinde](./batch-application-packages.md)paketleyebilirsiniz. Ardından, uygulama paketini havuzlarınıza otomatik olarak dağıtmak için, havuz yapılandırmasına bir uygulama paketi başvurusu ekleyin.
>

## <a name="throttle-and-sample-data"></a>Gaz ve örnek veriler 

Üretimde çalışan Azure Toplu İş uygulamalarının büyük ölçekli doğası nedeniyle, maliyetleri yönetmek için Application Insights tarafından toplanan veri miktarını sınırlamak isteyebilirsiniz. Bunu başarmak için bazı mekanizmalar için [Uygulama Öngörüleri örnekleme](../azure-monitor/app/sampling.md) bakın.


## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama Öngörüleri](../azure-monitor/app/app-insights-overview.md)hakkında daha fazla bilgi edinin.

* Diğer dillerdeki Uygulama Öngörüleri desteği için [dillere, platformlara ve tümleştirme belgelerine](../azure-monitor/app/platforms.md)bakın.


