---
title: .NET Dosya Kuralları kitaplığı yla çıktı verilerini Azure Depolama'da kalıcı olarak sataşma - Azure Toplu İşlemi
description: Toplu iş çıktısını Azure Depolama'ya & devam etmek ve bu çıktıyı Azure portalında görüntülemek için .NET için Azure Toplu Dosya Kuralları kitaplığını nasıl kullanacağınızı öğrenin.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 11/14/2018
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 693017e529f2869c16d94c30cdf48ec228df3276
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022877"
---
# <a name="persist-job-and-task-data-to-azure-storage-with-the-batch-file-conventions-library-for-net"></a>.NET için Toplu Dosya Kuralları kitaplığıyla iş ve görev verilerini Azure Depolama'da kalıcı olarak sataş

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Görev verilerini sebat etmenin bir [yolu, .NET için Azure Toplu Dosya Kuralları kitaplığını][nuget_package]kullanmaktır. Dosya Kuralları kitaplığı, görev çıktı verilerini Azure Depolama'ya depolama ve alma işlemini basitleştirir. Dosya Kuralları kitaplığını, kalıcı dosyalar için &mdash; görev kodunda hem görev hem de istemci kodunda ve bunları listelemek ve almak için istemci kodunda kullanabilirsiniz. Görev kodunuz, [görev bağımlılıkları](batch-task-dependencies.md) senaryosu gibi akış yukarı görevlerin çıktısını almak için kitaplığı da kullanabilir.

Dosya Kuralları kitaplığı ile çıktı dosyalarını almak için, belirli bir iş veya görev için dosyaları kimlik ve amaca göre listeleyerek bulabilirsiniz. Dosyaların adlarını veya konumlarını bilmeniz gerekmez. Örneğin, belirli bir görev için tüm ara dosyaları listelemek veya belirli bir iş için önizleme dosyası almak için Dosya Kuralları kitaplığını kullanabilirsiniz.

> [!TIP]
> Toplu Işlem hizmeti API, 2017-05-01 sürümünden başlayarak, sanal makine yapılandırmasıyla oluşturulan havuzlarda çalışan görevler ve iş yöneticisi görevleri için Azure Depolama'ya kalıcı çıktı verilerini destekler. Toplu İşlem hizmeti API' si, bir görev oluşturan ve Dosya Kuralları kitaplığına alternatif olarak hizmet veren kodun içinden çıktıyı sürdürmek için basit bir yol sağlar. Toplu İstemci uygulamalarınızı, görevinizin yürüttüğü uygulamayı güncelleştirmeye gerek kalmadan çıktıyı kalıcı hale getirmek için değiştirebilirsiniz. Daha fazla bilgi için, [Toplu Iş hizmeti API'si ile Azure Depolama'daki görev verilerini kalıcı olarak](batch-task-output-files.md)görün.

## <a name="when-do-i-use-the-file-conventions-library-to-persist-task-output"></a>Görev çıktısını sürdürmek için Dosya Kuralları kitaplığını ne zaman kullanırım?

Azure Toplu İşlem, görev çıktısını sürdürmek için birden fazla yol sağlar. Dosya Kuralları bu senaryolar için en uygun olan:

- Dosya Kuralları kitaplığını kullanarak, görevinizin yürüttüğü uygulamanın kodunu dosyaları kalıcı hale getirmek için kolayca değiştirebilirsiniz.
- Görev çalışırken verileri Azure Depolama'ya aktarmak istiyorsunuz.
- Bulut hizmeti yapılandırması veya sanal makine yapılandırması ile oluşturulan havuzlardan gelen verileri kalıcı hale getirmek istiyorsunuz.
- İstemci uygulamanızın veya işteki diğer görevlerin görev çıktıdosyalarını kimlik veya amaçla bulması ve indirmesi gerekir.
- Görev çıktısını Azure portalında görüntülemek istiyorsunuz.

Senaryonuz yukarıda listelenenlerden farklıysa, farklı bir yaklaşım düşünmeniz gerekebilir. Görev çıktısını kalıcı hale getirmek için diğer seçenekler hakkında daha fazla bilgi için Azure [Depolama'da kalıcı iş ve görev çıktısı'na](batch-task-output.md)bakın.

## <a name="what-is-the-batch-file-conventions-standard"></a>Toplu Dosya Kuralları standardı nedir?

[Toplu Dosya Kuralları standardı,](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files) çıkış dosyalarınızın yazıldığı hedef kapsayıcılar ve blob yolları için bir adlandırma düzeni sağlar. Dosya Kuralları standardına uygun olan Azure Depolama'da kalıcı dosyalar, Azure portalında görüntülenmek üzere otomatik olarak kullanılabilir. Portal adlandırma kuralının farkındadır ve bu nedenle ona uyan dosyaları görüntüleyebilir.

.NET için Dosya Kuralları kitaplığı, depolama kapsayıcılarınızı ve görev çıktıdosyalarını Dosya Kuralları standardına göre otomatik olarak adlandırır. Dosya Kuralları kitaplığı, iş kimliğine, görev kimliğine veya amaca göre Azure Depolama'daki çıktı dosyalarını sorgulama yöntemleri de sağlar.

.NET dışında bir dil geliştiriyorsanız, başvurunuzda Dosya Kuralları standardını kendiniz uygulayabilirsiniz. Daha fazla bilgi için toplu [dosya kuralları standardına](batch-task-output.md#implement-the-batch-file-conventions-standard)bakın.

## <a name="link-an-azure-storage-account-to-your-batch-account"></a>Azure Depolama hesabını Toplu Hesabınıza bağlama

Çıktı verilerini Dosya Kuralları kitaplığını kullanarak Azure Depolama'da kalıcı yormak için öncelikle bir Azure Depolama hesabını Toplu Hesabınıza bağlamanız gerekir. Bunu daha önce yapmadıysanız, [Azure portalını](https://portal.azure.com)kullanarak bir Depolama hesabınızı Toplu Hesabınıza bağlayın:

1. Azure portalında Batch hesabınıza gidin.
1. **Ayarlar** **altında, Depolama Hesabı'nı**seçin.
1. Toplu Iş hesabınızla ilişkili bir Depolama hesabınız yoksa, **Depolama Hesabı'nı (Yok)** tıklatın.
1. Aboneliğiniz için listeden bir Depolama hesabı seçin. En iyi performans için, görevlerinizi çalıştıran Toplu Iş hesabıyla aynı bölgede bulunan bir Azure Depolama hesabı kullanın.

## <a name="persist-output-data"></a>Çıktı verilerini kalıcı olarak sa

Dosya Kuralları kitaplığıyla iş ve görev çıktısı verilerini sürdürmek için Azure Depolama'da bir kapsayıcı oluşturun ve çıktıyı kapsayıcıya kaydedin. Görev çıktısını kapsayıcıya yüklemek için görev kodunuzdaki [.NET için Azure Depolama istemci kitaplığını](https://www.nuget.org/packages/WindowsAzure.Storage) kullanın.

Azure Depolama'da kapsayıcılar ve lekelerle çalışma hakkında daha fazla bilgi için [.NET'i kullanarak Azure Blob depolama alanına başlayın'a](../storage/blobs/storage-dotnet-how-to-use-blobs.md)bakın.

> [!WARNING]
> Dosya Kuralları kitaplığıyla devam eden tüm iş ve görev çıktıları aynı kapsayıcıda depolanır. Çok sayıda görev dosyaları aynı anda kalıcı olarak kalmaya çalışırsa, Azure Depolama azaltma sınırları zorlanabilir. Azaltma sınırları hakkında daha fazla bilgi [için, Blob depolama için Performans ve ölçeklenebilirlik denetim listesine](../storage/blobs/storage-performance-checklist.md)bakın.

### <a name="create-storage-container"></a>Depolama kapsayıcısı oluşturma

Görev çıktısını Azure Depolama'da sürdürmek için önce [CloudJob'u][net_cloudjob]arayarak bir kapsayıcı oluşturun. [PrepareOutputStorageAsync][net_prepareoutputasync]. Bu uzantı yöntemi, bir [CloudStorageAccount][net_cloudstorageaccount] nesnesini parametre olarak alır. İçeriğinin Azure portalı ve makalenin daha sonra tartışılan alma yöntemleri tarafından bulunabilmesi için Dosya Kuralları standardına göre adlandırılmış bir kapsayıcı oluşturur.

Genellikle istemci uygulamanızda &mdash; havuzlarınızı, işlerinizi ve görevlerinizi oluşturan uygulamada bir kapsayıcı oluşturmak için kodu yerleştirirsiniz.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Görev çıktılarını depolama

Azure Depolama'da bir kapsayıcı hazırladığınıza göre, görevler Dosya Kuralları kitaplığında bulunan [TaskOutputStorage][net_taskoutputstorage] sınıfını kullanarak çıktıyı kapsayıcıya kaydedebilir.

Görev kodunuzda, önce [TaskOutputStorage][net_taskoutputstorage] nesnesi oluşturun, sonra görev çalışmasını tamamladığında [TaskOutputStorage'ı][net_taskoutputstorage]arayın. Çıkışını Azure Depolama'ya kaydetmek için [SaveAsync][net_saveasync] yöntemi.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

`kind` [TaskOutputStorage](/dotnet/api/microsoft.azure.batch.conventions.files.taskoutputstorage)parametresi. [SaveAsync](/dotnet/api/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync#overloads) yöntemi, kalıcı dosyaları kategorilere ayırıyor. Dört önceden tanımlanmış [TaskOutputKind][net_taskoutputkind] `TaskOutput`türü `TaskPreview` `TaskLog`vardır: `TaskIntermediate.` , , , ve ayrıca özel çıktı kategorileri tanımlayabilirsiniz.

Bu çıktı türleri, belirli bir görevin kalıcı çıktıları için daha sonra Toplu İşlem'i sorguladığınızı listelemek için hangi çıktı türünü belirtmenize olanak sağlar. Başka bir deyişle, bir görevin çıktılarını listelediğinizde, çıktı türlerinden birinde listeyi filtreleyebilirsiniz. Örneğin, "Görev *109*için *önizleme* çıktısını verin." Çıktıları listeleme ve alma hakkında daha fazla şey, makalenin ilerleyen saatlerinde Al çıktısında görünür.

> [!TIP]
> Çıktı türü ayrıca Azure portalında belirli bir dosyanın nerede göründüğünü de belirler: *Görev Çıktısı*kategorize edilmiş **dosyalar Görev çıktısı dosyalarının**altında görünür ve *TaskLog* dosyaları **Görev günlüklerinin**altında görünür.

### <a name="store-job-outputs"></a>İş çıktılarını depolama

Görev çıktılarını depolamaya ek olarak, tüm iş ile ilişkili çıktıları depolayabilirsiniz. Örneğin, bir film işleme işinin birleştirme görevinde, tam olarak işlenen filmi bir iş çıktısı olarak devam ettirebilirsiniz. İşiniz tamamlandığında, istemci uygulamanız iş çıktılarını listeleyebilir ve alabilir ve tek tek görevleri sorgulaması gerekmez.

[JobOutputStorage'ı][net_joboutputstorage]arayarak iş çıktısını depolayın. [SaveAsync][net_joboutputstorage_saveasync] yöntemi ve [JobOutputKind][net_joboutputkind] ve dosya adını belirtin:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Görev çıktıları için **TaskOutputKind** türünde olduğu gibi, bir işin kalıcı dosyalarını kategorilere ayırmak için [JobOutputKind][net_joboutputkind] türünü kullanırsınız. Bu parametre, belirli bir çıktı türü için daha sonra (liste) sorgulamanızı sağlar. **JobOutputKind** türü hem çıktı hem de önizleme kategorilerini içerir ve özel kategoriler oluşturmayı destekler.

### <a name="store-task-logs"></a>Görev günlüklerini depolama

Bir görev veya iş tamamlandığında bir dosyayı dayanıklı depolamaya devam etmeye ek olarak, görev &mdash; günlüğü dosyalarının `stdout.txt` yürütülmesi `stderr.txt`sırasında güncelleştirilen dosyaları veya örneğin, kalıcı olarak devam etmeniz gerekebilir. Bu amaçla, Azure Toplu Dosya Kuralları kitaplığı [TaskOutputStorage][net_taskoutputstorage]sağlar. [SaveTrackedAsync][net_savetrackedasync] yöntemi. [SaveTrackedAsync][net_savetrackedasync]ile, düğümdeki bir dosyanın güncelleştirmelerini izleyebilir (belirttiğiniz bir aralıkta) ve Azure Depolama'daki bu güncelleştirmeleri devam ettirebilirsiniz.

Aşağıdaki kod snippet'inde, görevin yürütülmesi `stdout.txt` sırasında Her 15 saniyede bir Azure Depolama'da güncelleştirmek için [SaveTrackedAsync'i][net_savetrackedasync] kullanıyoruz:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
     await Task.Delay(stdoutFlushDelay);
}
```

Yorumlanan `Code to process data and produce output file(s)` bölüm, görevinizin normalde gerçekleştireceği kod için bir yer tutucudur. Örneğin, Azure Depolama'dan veri indiren ve üzerinde dönüşüm veya hesaplama gerçekleştiren kodunuz olabilir. Bu snippet önemli bir parçası [saveTrackedAsync][net_savetrackedasync]ile `using` periyodik olarak bir dosyayı güncelleştirmek için bir blok ta bu tür kodu sarın nasıl gösteren.

Düğüm aracısı, havuzdaki her düğümde çalışan ve düğüm ile Toplu İşlem hizmeti arasındaki komut ve denetim arabirimi sağlayan bir programdır. Bu `Task.Delay` `using` bloğun sonunda, düğüm aracısının standart içeriği düğümüzerindeki stdout.txt dosyasına atmak için zamanı olduğundan emin olmak için çağrı gereklidir. Bu gecikme olmadan, çıktının son birkaç saniyesini kaçırmak mümkündür. Bu gecikme tüm dosyalar için gerekli olmayabilir.

> [!NOTE]
> **SaveTrackedAsync**ile dosya izlemeyi etkinleştirdiğinizde, yalnızca izlenen dosyanın *ekleri* Azure Depolama'ya kalıcı dır. Bu yöntemi yalnızca, dosyanın sonuna kadar ekleme işlemleriyle birlikte yazılan döndürmeyen günlük dosyalarını veya diğer dosyaları izlemek için kullanın.

## <a name="retrieve-output-data"></a>Çıktı verilerini alma

Kalıcı çıktınızı Azure Toplu Toplu Dosya Kuralları kitaplığını kullanarak aldığınızda, bunu görev ve iş odaklı bir şekilde yaparsınız. Azure Depolama'daki yolunu ve hatta dosya adını bilmenize gerek kalmadan, belirli bir görev veya iş için çıktı isteyebilirsiniz. Bunun yerine, görev veya iş kimliğine göre çıktı dosyaları isteyebilirsiniz.

Aşağıdaki kod, bir işin görevlerini snippet iterates, görev için çıktı dosyaları hakkında bazı bilgiler yazdırır ve sonra Depolama dosyalarını karşıdan yüklez.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-the-azure-portal"></a>Azure portalındaki çıktı dosyalarını görüntüleme

Azure portalı, [Toplu Dosya Kuralları standardını](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions)kullanarak bağlantılı bir Azure Depolama hesabına kalıcı görev çıktısı dosyalarını ve günlüklerini görüntüler. Bu kuralları kendi seçtiğiniz dilde uygulayabilir veya .NET uygulamalarınızda Dosya Kuralları kitaplığını kullanabilirsiniz.

Çıktı dosyalarınızın portalda görüntülenmesini etkinleştirmek için aşağıdaki gereksinimleri karşılamanız gerekir:

1. Bir Azure Depolama hesabını Toplu Hesabınıza bağlayın.
1. Çıktıları kalıcı hale alırken Depolama kapsayıcıları ve dosyaları için önceden tanımlanmış adlandırma kurallarına uyun. Bu sözleşmelerin tanımını Dosya Kuralları kitaplığında [README'de][github_file_conventions_readme]bulabilirsiniz. Çıktınızı devam ettirebilmek için [Azure Toplu Dosya Kuralları][nuget_package] kitaplığını kullanırsanız, dosyalarınız Dosya Kuralları standardına göre kalıcıdır.

Azure portalında görev çıktıdosyalarını ve günlüklerini görüntülemek için çıktıyla ilgilendiğiniz göreve gidin ve ardından **Çıktı yı kaydedin** veya **kaydedilmiş günlükleri**tıklatın. Bu resim, "007" kimliğiyle görev için **Kaydedilen çıktı dosyalarını** gösterir:

![Azure portalında görev çıktıları bıçak][2]

## <a name="code-sample"></a>Kod örneği

[PersistOutputs][github_persistoutputs] örnek projesi, GitHub'daki [Azure Toplu İş kodu örneklerinden][github_samples] biridir. Bu Visual Studio çözümü, görev çıktısını dayanıklı depolamaya devam etmek için Azure Toplu Dosya Kuralları kitaplığını nasıl kullanacağımı gösterir. Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio **2019'da**projeyi açın.
2. Microsoft.Azure.Batch.Samples.Common projesindeki **Hesap Ayarları.ayarlarına** Toplu Iş ve Depolama **hesabı kimlik bilgilerinizi** ekleyin.
3. Çözümü **oluşturun** (ancak çalıştırmayın). İstenirse tüm NuGet paketlerini geri yükleyin.
4. **PersistOutputsTask**için bir [uygulama paketi](batch-application-packages.md) yüklemek için Azure portalını kullanın. .zip `PersistOutputsTask.exe` paketine ve bağımlı derlemelerine ekleyin, uygulama kimliğini "PersistOutputsTask" ve uygulama paketi sürümünü "1.0" olarak ayarlayın.
5. **Kalıcı Çıktılar** projesini **başlatın** (çalıştırın).
6. Örneği çalıştırmak için kullanılacak kalıcılık teknolojisini seçmesi istendiğinde, görev çıktısını kalıcı hale getirmek için Dosya Kuralları kitaplığını kullanarak örneği çalıştırmak için **1** girin. 

## <a name="next-steps"></a>Sonraki adımlar

### <a name="get-the-batch-file-conventions-library-for-net"></a>.NET için Toplu Dosya Kuralları kitaplığını alın

.NET için Toplu Dosya Kuralları kitaplığı [NuGet'de][nuget_package]mevcuttur. Kitaplık, [CloudJob][net_cloudjob] ve [CloudTask][net_cloudtask] sınıflarını yeni yöntemlerle genişletir. Ayrıca Dosya Kuralları kitaplığı için [başvuru belgelerine](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) bakın.

Dosya Kuralları kitaplığı için [kaynak kodu][github_file_conventions] GitHub'da Microsoft Azure SDK'da .NET deposu için kullanılabilir. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Kalıcı çıktı verilerini araştırmak için diğer yaklaşımları keşfedin

- Kalıcı görev ve iş verilerine genel bakış için [Azure Depolama'da iş](batch-task-output.md) ve görev çıktısını devam edin' e bakın.
- Çıktı verilerini kalıcı olarak sürdürmek için Toplu Iş hizmeti API'sini nasıl kullanacağınızı öğrenmek için [Toplu Iş hizmeti API'sıyla Azure Depolama'da görev verilerini devam](batch-task-output-files.md) edin.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage._cloud_storage_account
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: https://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Kaydedilen çıktı dosyaları ve Kaydedilmiş portaldaki seçicileri kaydeder"
[2]: ./media/batch-task-output/task-output-02.png "Azure portalında görev çıktıları bıçak"
