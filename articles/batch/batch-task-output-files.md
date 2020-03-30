---
title: Toplu Hizmet API'si ile çıktı verilerini Azure Depolama'da kalıcı olarak sada - Azure Toplu İşlem
description: Toplu iş görevini sürdürmek ve çıktı verilerini Azure Depolama'da sürdürmek için Toplu Iş hizmeti API'sini nasıl kullanacağınızı öğrenin.
services: batch
author: LauraBrenner
manager: evansma
editor: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 03/05/2019
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 11bd8bc427dd3da35ec5aa0f728f6b04b7d4527d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022860"
---
# <a name="persist-task-data-to-azure-storage-with-the-batch-service-api"></a>Toplu Iş hizmeti API'si ile görev verilerini Azure Depolama'da kalıcı olarak sataş

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Toplu İşlem hizmeti API'si, sanal makine yapılandırması ile havuzlarda çalışan görevler ve iş yöneticisi görevleri için Azure Depolama'ya devam eden çıktı verilerini destekler. Görev eklediğinizde, görev çıktısının hedefi olarak Azure Depolama'daki bir kapsayıcıyı belirtebilirsiniz. Toplu İşlem hizmeti, görev tamamlandığında bu kapsayıcıya çıktı verilerini yazar.

Görev çıktısını kalıcı hale getirmek için Toplu Iş Partisi API'sini kullanmanın bir avantajı, görevin yürüttüğü uygulamayı değiştirmeniz gerekmemesidir. Bunun yerine, istemci uygulamanızda yapılan birkaç değişiklikle, görevin çıktısını görevi oluşturan aynı kod içinden devam edebilirsiniz.

## <a name="when-do-i-use-the-batch-service-api-to-persist-task-output"></a>Görev çıktısını devam etmek için Toplu İşlem hizmeti API'sini ne zaman kullanırım?

Azure Toplu İşlem, görev çıktısını sürdürmek için birden fazla yol sağlar. Toplu Iş hizmeti API'sini kullanmak, bu senaryolara en uygun olan kullanışlı bir yaklaşımdır:

- Görevinizin yürüttüğü uygulamayı değiştirmeden istemci uygulamanızın içinden görev çıktısını kalıcı hale getirmek için kod yazmak istiyorsunuz.
- Sanal makine yapılandırmasıyla oluşturulan havuzlarda Toplu İşlem görevlerinden ve iş yöneticisi görevlerinden çıktıyı sürdürmek istiyorsunuz.
- Çıktıyı rasgele bir ada sahip bir Azure Depolama kapsayıcısına devam etmek istiyorsunuz.
- Çıktıyı [Toplu Dosya Kuralları standardına](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions)göre adlandırılan bir Azure Depolama kapsayıcısına devam etmek istiyorsunuz. 

Senaryonuz yukarıda listelenenlerden farklıysa, farklı bir yaklaşım düşünmeniz gerekebilir. Örneğin, Toplu İş hizmeti API'si, görev çalışırken Azure Depolama'ya akış çıktısını şu anda desteklemez. Çıktıakışı için ,.NET için kullanılabilen Toplu Dosya Kuralları kitaplığını kullanmayı düşünün. Diğer diller için kendi çözümünüzü uygulamanız gerekir. Görev çıktısını kalıcı hale getirmek için diğer seçenekler hakkında daha fazla bilgi için Azure [Depolama'da kalıcı iş ve görev çıktısı'na](batch-task-output.md)bakın.

## <a name="create-a-container-in-azure-storage"></a>Azure Depolama'da kapsayıcı oluşturma

Görev çıktısını Azure Depolama'da kalıcı yormak için çıktı dosyalarınız için hedef olarak hizmet veren bir kapsayıcı oluşturmanız gerekir. Görevinizi çalıştırmadan önce, tercihen işinizi göndermeden önce kapsayıcıyı oluşturun. Kapsayıcıyı oluşturmak için uygun Azure Depolama istemci kitaplığını veya SDK'yı kullanın. Azure Depolama API'leri hakkında daha fazla bilgi için [Azure Depolama belgelerine](https://docs.microsoft.com/azure/storage/)bakın.

Örneğin, uygulamanızı C#'da yazıyorsanız, [.NET için Azure Depolama istemci kitaplığını](https://www.nuget.org/packages/WindowsAzure.Storage/)kullanın. Aşağıdaki örnek, kapsayıcının nasıl oluşturulabildiğini gösterir:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await container.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-the-container"></a>Kapsayıcı için paylaşılan erişim imzası alma

Kapsayıcıyı oluşturduktan sonra, kapsayıcıya yazma erişimi olan paylaşılan bir erişim imzası (SAS) alın. Bir SAS kapsayıcıya temsilci erişimi sağlar. SAS, belirli bir izin kümesiyle ve belirli bir zaman aralığı yla erişim sağlar. Toplu İşlem hizmeti, kapsayıcıya görev çıktısı yazmak için yazma izinlerine sahip bir SAS'a ihtiyaç duyar. SAS hakkında daha fazla bilgi için bkz. Azure [Depolama'da \(paylaşılan erişim imzalarını kullanma SAS.\) ](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

Azure Depolama API'lerini kullanarak bir SAS aldığınızda, API bir SAS belirteç dizesini döndürür. Bu belirteç dizesi, izinler ve SAS'ın geçerli olduğu aralık da dahil olmak üzere SAS'ın tüm parametrelerini içerir. Azure Depolama'daki bir kapsayıcıya erişmek için SAS'ı kullanmak için SAS belirteç dizesini kaynak URI'ye eklemeniz gerekir. Uri kaynağı, eklenen SAS belirteciyle birlikte Azure Depolama'ya kimlik doğrulaması erişimi sağlar.

Aşağıdaki örnek, kapsayıcı için yalnızca yazma SAS belirteci dizesini nasıl elde edilebildiğini gösterir ve sonra SAS'ı uri konteynerine ekler:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken;
```

## <a name="specify-output-files-for-task-output"></a>Görev çıktısı için çıktı dosyalarını belirtin

Bir görev için çıktı dosyalarını belirtmek [için, Bir OutputFile](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) nesnekoleksiyonu oluşturun ve görevi oluştururken [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) özelliğine atayın.

Aşağıdaki C# kodu örneği adlı `output.txt`bir dosyaya rasgele sayılar yazan bir görev oluşturur. Örnek, kapsayıcıya yazılması `output.txt` için bir çıktı dosyası oluşturur. `std*.txt` Örnek ayrıca, dosya deseniyle eşleşen tüm günlük dosyaları için çıktı dosyaları da oluşturur (_örn._ `stdout.txt` `stderr.txt` Kapsayıcı URL'si, kapsayıcı için daha önce oluşturulmuş SAS'yi gerektirir. Toplu İşlem hizmeti, kapsayıcıya erişimi doğrulamak için SAS'ı kullanır:

```csharp
new CloudTask(taskId, "cmd /v:ON /c \"echo off && set && (FOR /L %i IN (1,1,100000) DO (ECHO !RANDOM!)) > output.txt\"")
{
    OutputFiles = new List<OutputFile>
    {
        new OutputFile(
            filePattern: @"..\std*.txt",
            destination: new OutputFileDestination(
         new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId)),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
        new OutputFile(
            filePattern: @"output.txt",
            destination: 
         new OutputFileDestination(new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId + @"\output.txt")),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
}
```

### <a name="specify-a-file-pattern-for-matching"></a>Eşleştirme için bir dosya deseni belirtin

Bir çıktı dosyası belirttiğiniz zaman, eşleştirme için bir dosya deseni belirtmek için [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) özelliğini kullanabilirsiniz. Dosya deseni sıfır dosyaları, tek bir dosyayı veya görev tarafından oluşturulan bir dosya kümesini eşleyebilir.

**FilePattern** özelliği ( özyinelemeli olmayan `*` eşleşmeler için) ve `**` (özyinelemeli eşleşmeler için) gibi standart dosya sistemi joker karakterleri destekler. Örneğin, yukarıdaki kod örneği, yinelemeli olmayan `std*.txt` bir şekilde eşleşecek dosya deseni belirtir:

`filePattern: @"..\std*.txt"`

Tek bir dosya yı yüklemek için joker karaktersiz bir dosya deseni belirtin. Örneğin, yukarıdaki kod örneği, dosya desenini `output.txt`eşleşecek şekilde belirtir:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Yükleme koşulu belirtin

[OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) özelliği çıktı dosyalarının koşullu yüklenmesine izin verir. Sık karşılaşılan bir senaryo, görev başarılı olursa bir dosya kümesini ve başarısız olursa farklı bir dosya kümesini yüklemektir. Örneğin, yalnızca görev başarısız olduğunda ve sıfır olmayan bir çıkış koduyla çıktığında ayrıntılı günlük dosyalarını yüklemek isteyebilirsiniz. Benzer şekilde, sonuç dosyalarını yalnızca görev başarılı olursa yüklemek isteyebilirsiniz, çünkü bu dosyalar görev başarısız olursa eksik veya eksik olabilir.

Yukarıdaki kod **örneği, UploadCondition** özelliğini **TaskCompletion**olarak ayarlar. Bu ayar, çıkış kodunun değerine bakılmaksızın, görevler tamamlandıktan sonra dosyanın yüklenmesi gerektiğini belirtir.

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Diğer ayarlar için [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) enumuna bakın.

### <a name="disambiguate-files-with-the-same-name"></a>Aynı ada sahip Disambiguate dosyaları

Bir işteki görevler aynı ada sahip dosyalar üretebilir. Örneğin ve `stdout.txt` `stderr.txt` bir işte çalışan her görev için oluşturulur. Her görev kendi bağlamında çalıştığından, bu dosyalar düğümün dosya sisteminde çakışma yapmaz. Ancak, birden çok görevden paylaşılan bir kapsayıcıya dosya yüklediğinizde, dosyaları aynı ada sahip olarak ayrıştırmanız gerekir.

[OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) özelliği çıkış dosyaları için hedef blob veya sanal dizin belirtir. **Blob** veya sanal dizinin adını, aynı ada sahip çıktı dosyalarının Azure Depolama'da benzersiz bir şekilde adlandırılması için Yol özelliğini kullanabilirsiniz. Yoldaki görev kimliğini kullanmak, benzersiz adlar sağlamak ve dosyaları kolayca tanımlamak için iyi bir yoldur.

**FilePattern** özelliği joker karakter ifadesine ayarlanmışsa, desenle eşleşen tüm dosyalar **Yol** özelliği tarafından belirtilen sanal dizine yüklenir. Örneğin, kapsayıcı `mycontainer`, görev kimliği `mytask`ve dosya deseni `..\std*.txt`, sonra Azure Depolama çıktı dosyalarına mutlak URIs benzer olacaktır:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

**FilePattern** özelliği tek bir dosya adıyla eşleşecek şekilde ayarlanmışsa, yani joker karakter içermiyorsa, **Yol** özelliğinin değeri tam nitelikli blob adını belirtir. Birden çok görevden tek bir dosyayla çakışma yapmayı bekliyorsanız, bu dosyaları ayırmak için dosya adının bir parçası olarak sanal dizinin adını ekleyin. Örneğin, **Yol** özelliğini görev kimliğini, sınır düşürme karakterini (genellikle ileri eğik çizgi) ve dosya adını içerecek şekilde ayarlayın:

`path: taskId + @"/output.txt"`

Bir dizi görev için çıktı dosyalarındaki mutlak URI'ler aşağıdakilere benzer olacaktır:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Azure Depolama'daki sanal [dizinler](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container)hakkında daha fazla bilgi için bkz.

## <a name="diagnose-file-upload-errors"></a>Dosya yükleme hatalarını tanılama

Çıktı dosyalarını Azure Depolama'ya yüklemek başarısız olursa, görev **Tamamlanan** duruma taşınır ve [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) özelliği ayarlanır. Hangi hatanın oluştuğunu belirlemek için **FailureInformation** özelliğini inceleyin. Örneğin, kapsayıcı bulunamıyorsa dosya yüklemede oluşan bir hata aşağıda verilmiştir:

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of the specified Azure container(s) was not found while attempting to upload an output file
```

Her dosya yükleme, Batch işlem düğümüne iki günlük `fileuploadout.txt` dosyası `fileuploaderr.txt`yazar ve . Belirli bir hata hakkında daha fazla bilgi edinmek için bu günlük dosyalarını inceleyebilirsiniz. Dosya yüklemesinin hiç denenmediği durumlarda, örneğin görevin kendisi çalıştırılamadığı için, bu günlük dosyaları bulunmaz.

## <a name="diagnose-file-upload-performance"></a>Dosya yükleme performansını tanılama

Dosya `fileuploadout.txt` günlükleri yükleme ilerleme. Dosya yüklemelerinizin ne kadar süreceğü hakkında daha fazla bilgi edinmek için bu dosyayı inceleyebilirsiniz. Düğümün boyutu, yükleme sırasındadüğümdeki diğer etkinlikler, hedef kapsayıcının Toplu İşlem havuzuyla aynı bölgede olup olmadığı, kaç düğüm yüklendiği de dahil olmak üzere performans yüklemek için birçok katkıda bulunan etken olduğunu unutmayın depolama hesabı aynı anda, ve benzeri.

## <a name="use-the-batch-service-api-with-the-batch-file-conventions-standard"></a>Toplu Iş Hizmeti API'sini Toplu Dosya Kuralları standardıyla kullanma

Toplu İşlem hizmeti API'si ile görev çıktısını devam ettirirken, hedef kapsayıcınızı ve lekeleri istediğiniz gibi adlandırabilirsiniz. Ayrıca toplu [dosya kuralları standardına](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/Batch/Support/FileConventions#conventions)göre adlandırmayı da seçebilirsiniz. Dosya Kuralları standardı, iş ve görev adlarını temel alan belirli bir çıktı dosyası için Azure Depolama'daki hedef kapsayıcının ve blob'un adlarını belirler. Çıktı dosyalarını adlandırmak için Dosya Kuralları standardını kullanıyorsanız, çıktı dosyalarınız [Azure portalında](https://portal.azure.com)görüntülenmek üzere kullanılabilir.

C#'da geliştiriyorsanız, [.NET için Toplu Dosya Kuralları kitaplığında](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files)yerleşik yöntemleri kullanabilirsiniz. Bu kitaplık sizin için düzgün adlandırılmış kapsayıcılar ve blob yolları oluşturur. Örneğin, iş adına bağlı olarak kapsayıcı için doğru adı almak için API'yi arayabilirsiniz:

```csharp
string containerName = job.OutputStorageContainerName();
```

Kapsayıcıya yazmak için kullanılan paylaşılan erişim imzası (SAS) URL'sini döndürmek için [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) yöntemini kullanabilirsiniz. Daha sonra bu SAS'ı [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) oluşturucuya geçirebilirsiniz.

C#'dan başka bir dilde geliştiriyorsanız, Dosya Kuralları standardını kendiniz uygulamanız gerekir.

## <a name="code-sample"></a>Kod örneği

[PersistOutputs][github_persistoutputs] örnek projesi, GitHub'daki [Azure Toplu İş kodu örneklerinden][github_samples] biridir. Bu Visual Studio çözümü, görev çıktısını dayanıklı depolamaya devam etmek için .NET için Toplu İstemci kitaplığını nasıl kullanacağımı gösterir. Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio **2019'da**projeyi açın.
2. Microsoft.Azure.Batch.Samples.Common projesindeki **Hesap Ayarları.ayarlarına** Toplu Iş ve Depolama **hesabı kimlik bilgilerinizi** ekleyin.
3. Çözümü **oluşturun** (ancak çalıştırmayın). İstenirse tüm NuGet paketlerini geri yükleyin.
4. **PersistOutputsTask**için bir [uygulama paketi](batch-application-packages.md) yüklemek için Azure portalını kullanın. .zip `PersistOutputsTask.exe` paketine ve bağımlı derlemelerine ekleyin, uygulama kimliğini "PersistOutputsTask" ve uygulama paketi sürümünü "1.0" olarak ayarlayın.
5. **Kalıcı Çıktılar** projesini **başlatın** (çalıştırın).
6. Numuneyi çalıştırmak için kullanılacak kalıcılık teknolojisini seçmesi istendiğinde, görev çıktısını kalıcı hale getirmek için Toplu Iş hizmeti API'sini kullanarak örneği çalıştırmak için **2** girin.
7. İstenirse, Toplu Iş hizmeti API'si ile çıktıyı sürdürmek ve ayrıca Dosya Kuralları standardına göre hedef kapsayıcı ve blob yolunu adlandırmak için **3** girerek örneği yeniden çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

- .NET için Dosya Kuralları kitaplığıyla görev çıktısını kalıcı hale getirmek hakkında daha fazla bilgi [için, .NET için Toplu Dosya Kuralları kitaplığıyla Azure Depolama'ya devam iş ve görev verilerine](batch-task-output-file-conventions.md)bakın.
- Azure Toplu İş'te çıktı verilerini kalıcı olarak sürdürmek için diğer yaklaşımlar hakkında bilgi için, [Azure Depolama'da kalıcı iş ve görev çıktısı'na](batch-task-output.md)bakın.

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
