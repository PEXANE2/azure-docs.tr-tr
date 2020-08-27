---
title: Batch hizmeti API 'SI ile Azure depolama 'ya çıkış verilerini kalıcı hale getirme
description: Batch görevi ve iş çıktısı verilerini Azure depolama 'ya kalıcı hale getirmek için Batch hizmeti API 'sini nasıl kullanacağınızı öğrenin.
ms.topic: how-to
ms.date: 07/30/2020
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 720c064c6b382bc62565c0828422181c761df8e8
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88936937"
---
# <a name="persist-task-data-to-azure-storage-with-the-batch-service-api"></a>Batch hizmeti API 'SI ile Azure depolama 'ya görev verilerini kalıcı hale getirme

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Batch hizmeti API 'SI, sanal makine yapılandırmasıyla havuzlar üzerinde çalışan görevler ve iş Yöneticisi görevleri için Azure depolama 'da kalıcı çıkış verilerini destekler. Bir görev eklediğinizde, Azure depolama 'da görev çıkışının hedefi olarak bir kapsayıcı belirtebilirsiniz. Ardından Batch hizmeti, görev tamamlandığında bu kapsayıcıya herhangi bir çıktı verisi yazar.

Görev çıktısını sürdürmek için Batch hizmeti API 'sini kullanmanın avantajı, görevin çalıştığı uygulamayı değiştirmenize gerek kalmaz. Bunun yerine, istemci uygulamanızda birkaç değişiklikle, görevi oluşturan aynı kodun içinden görevin çıkışını kalıcı hale getirebilirsiniz.

> [!IMPORTANT]
> Batch hizmeti API 'SI ile Azure depolama 'ya yönelik kalıcı görev verileri [, 1 şubat 2018 '](https://github.com/Azure/Batch/blob/master/changelogs/nodeagent/CHANGELOG.md#1204)den önce oluşturulan havuzlarla birlikte çalışmaz.

## <a name="when-do-i-use-the-batch-service-api-to-persist-task-output"></a>Görev çıktısını sürdürmek için Batch hizmeti API 'sini ne zaman kullanabilirim?

Azure Batch, görev çıkışını kalıcı hale getirmek için birden çok yol sağlar. Batch hizmeti API 'sini kullanmak, şu senaryolara en uygun olan kullanışlı bir yaklaşımdır:

- Görevin çalıştığı uygulamayı değiştirmeden istemci uygulamanızın içinden görev çıkışını kalıcı hale getirmek için kod yazmak istiyorsunuz.
- Sanal makine yapılandırmasıyla oluşturulan havuzlardaki toplu Iş görevlerinden ve iş Yöneticisi görevlerinin çıkışını kalıcı hale getirmek istiyorsunuz.
- Çıktıyı bir Azure depolama kapsayıcısına rastgele bir adla kalıcı hale getirmek istiyorsunuz.
- [Toplu Iş dosyası kuralları standardına](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files)göre adlı bir Azure depolama kapsayıcısına çıktıyı kalıcı hale getirmek istiyorsunuz.

Senaryolarınız yukarıda listelenenlerden farklıysa, farklı bir yaklaşım düşünmeniz gerekebilir. Örneğin, Batch hizmeti API 'SI Şu anda, görev çalışırken Azure Storage 'a akış çıktısı desteklememektedir. Çıktıyı akışa almak için, .NET için kullanılabilen batch dosya kuralları kitaplığını kullanmayı düşünün. Diğer diller için kendi çözümünüzü uygulamanız gerekir. Kalıcı görev çıktısına yönelik diğer seçenekler hakkında bilgi için bkz. [Azure Storage 'da iş ve görev çıkışını kalıcı hale](batch-task-output.md)getirme.

## <a name="create-a-container-in-azure-storage"></a>Azure depolama 'da kapsayıcı oluşturma

Görev çıktısını Azure depolama 'ya kalıcı hale getirmek için, çıkış dosyalarınız için hedef olarak hizmet veren bir kapsayıcı oluşturmanız gerekir. İşinizi göndermeden önce, tercihen hesabınızı çalıştırmadan önce kapsayıcıyı oluşturun. Kapsayıcıyı oluşturmak için uygun Azure depolama istemci kitaplığını veya SDK 'sını kullanın. Azure depolama API 'Leri hakkında daha fazla bilgi için bkz. [Azure depolama belgeleri](../storage/index.yml).

Örneğin, uygulamanızı C# dilinde yazıyorsanız [.net Için Azure Storage istemci kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/)' nı kullanın. Aşağıdaki örnek, bir kapsayıcının nasıl oluşturulacağını gösterir:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await container.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-the-container"></a>Kapsayıcı için paylaşılan erişim imzası alın

Kapsayıcıyı oluşturduktan sonra kapsayıcıya yazma erişimi olan bir paylaşılan erişim imzası (SAS) alın. Bir SAS, kapsayıcıya temsilci erişimi sağlar. SAS, belirtilen bir zaman aralığı boyunca belirli bir izin kümesiyle ve erişim izni verir. Batch hizmeti, kapsayıcıya görev çıktısı yazmak için yazma izinlerine sahip bir SAS gerektirir. SAS hakkında daha fazla bilgi için bkz. [ \( \) Azure depolama 'da paylaşılan erişim imzaları SAS kullanma](../storage/common/storage-sas-overview.md).

Azure depolama API 'Lerini kullanarak bir SAS aldığınızda, API bir SAS belirteci dizesi döndürür. Bu belirteç dizesi, izinler ve SAS 'ın geçerli olduğu Aralık dahil olmak üzere SAS 'nin tüm parametrelerini içerir. Azure depolama 'daki bir kapsayıcıya erişmek üzere SAS 'yi kullanmak için, SAS belirteç dizesini Kaynak URI 'sine eklemeniz gerekir. Kaynak URI 'SI, eklenen SAS belirteciyle birlikte Azure Storage 'a kimliği doğrulanmış erişim sağlar.

Aşağıdaki örnek, kapsayıcı için salt yazılır bir SAS belirteci dizesinin nasıl alınacağını gösterir ve ardından sa 'ları kapsayıcı URI 'sine ekler:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken;
```

## <a name="specify-output-files-for-task-output"></a>Görev çıktısı için çıkış dosyalarını belirtin

Bir görevin çıkış dosyalarını belirtmek için, bir [ÇıktıDosyası](/dotnet/api/microsoft.azure.batch.outputfile) nesnelerinin koleksiyonunu oluşturun ve görevi oluştururken [Cloudtask. OutputFiles](/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) özelliğine atayın.

Aşağıdaki C# kod örneği, adlı bir dosyaya rastgele sayılar yazan bir görev oluşturur `output.txt` . Örnek, kapsayıcısına yazılacak bir çıkış dosyası oluşturur `output.txt` . Örnek ayrıca dosya düzeniyle eşleşen tüm günlük dosyaları için çıktı dosyaları oluşturur `std*.txt` (_örn._ `stdout.txt` ve `stderr.txt` ). Kapsayıcı URL 'SI, kapsayıcısı için daha önce oluşturulmuş SAS gerektirir. Batch hizmeti, kapsayıcıya erişimin kimliğini doğrulamak için SAS kullanır:

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

> [!NOTE]
> Linux ile bu örneği kullanıyorsanız, ters eğik çizgileri eğik çizgi ile değiştirmek için değiştirdiğinizden emin olun.

### <a name="specify-a-file-pattern-for-matching"></a>Eşleştirme için bir dosya kalıbı belirtin

Bir çıkış dosyası belirttiğinizde, eşleşen bir dosya kalıbı belirtmek için [ÇıktıDosyası. filemodel](/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) özelliğini kullanabilirsiniz. Dosya deseninin sıfır dosya, tek bir dosya veya görev tarafından oluşturulan bir dosya kümesiyle eşleşmesi gerekebilir.

**Filemodel** özelliği, `*` (özyinelemeli olmayan eşleşmeler için) ve `**` (özyinelemeli eşleşmeler için) gibi standart dosya sistemi joker karakterlerini destekler. Örneğin, yukarıdaki kod örneği, yinelemeli olmayan eşleştirilecek dosya modelini belirtir `std*.txt` :

`filePattern: @"..\std*.txt"`

Tek bir dosyayı karşıya yüklemek için joker karakter içermeyen bir dosya kalıbı belirtin. Örneğin, yukarıdaki kod örneği eşleşecek dosya modelini belirtir `output.txt` :

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Karşıya yükleme koşulu belirtin

[Outputfileupler. UploadCondition](/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) özelliği, çıkış dosyalarının koşullu karşıya yüklenmesine izin verir. Yaygın bir senaryo, görev başarılı olursa bir dosya kümesini karşıya yüklemek ve başarısız olursa farklı bir dosya kümesi yüklemedir. Örneğin, ayrıntılı günlük dosyalarını yalnızca görev başarısız olduğunda ve sıfır dışında bir çıkış koduyla çıkarken karşıya yüklemek isteyebilirsiniz. Benzer şekilde, görev başarısız olursa, sonuç dosyalarını yalnızca görev başarılı olursa karşıya yüklemek isteyebilirsiniz.

Yukarıdaki kod örneği, **Uploadcondition** özelliğini **taskcompletion**olarak ayarlar. Bu ayar, çıkış kodunun değerinden bağımsız olarak, görevler tamamlandıktan sonra dosyanın karşıya yükleneceğini belirtir.

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Diğer ayarlar için [Outputfileuploadcondition](/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) enum bölümüne bakın.

### <a name="disambiguate-files-with-the-same-name"></a>Aynı ada sahip dosyaları Kesinleştirme

Bir işteki görevler aynı ada sahip dosyalar üretebilir. Örneğin, `stdout.txt` ve `stderr.txt` bir işte çalışan her görev için oluşturulur. Her görev kendi bağlamında çalıştığından, bu dosyalar düğümün dosya sisteminde çakışmaz. Ancak, birden çok görevden paylaşılan bir kapsayıcıya dosya yüklediğinizde, aynı ada sahip dosyaları kesinleştirmeniz gerekir.

[Outputfileblobcontainerdestination. Path](/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) özelliği, çıkış dosyaları için hedef Blobu veya sanal dizini belirtir. Blob veya sanal dizini aynı ada sahip çıkış dosyalarının Azure depolama 'da benzersiz olarak adlandırılması için, bu **yolu** kullanarak blob veya sanal dizine ad verebilirsiniz. Yoldaki görev KIMLIĞINI kullanmak, benzersiz adlar sağlamak ve dosyaları kolayca belirlemek için iyi bir yoldur.

**Filemodel** özelliği bir joker ifadesi olarak ayarlandıysa, düzeniyle eşleşen tüm dosyalar **Path** özelliği tarafından belirtilen sanal dizine yüklenir. Örneğin, kapsayıcı, `mycontainer` görev kimliği `mytask` ve dosya örüntü Ise, `..\std*.txt` Azure Storage 'daki çıkış dosyalarına yönelik mutlak URI 'ler şuna benzer olacaktır:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

**Filemodel** özelliği tek bir dosya adıyla eşleşecek şekilde ayarlandıysa, bir joker karakter içermediği anlamına gelir, **Path** özelliğinin değeri tam olarak nitelenmiş blob adını belirtir. Birden çok görevden tek bir dosya ile adlandırma çakışmalarını tahmin ediyorsanız, bu dosyaları ortadan kaldırmak için dosya adının bir parçası olarak sanal dizinin adını dahil edin. Örneğin, **Path** ÖZELLIĞINI görev kimliği, sınırlayıcı karakter (genellikle eğik çizgi) ve dosya adı içerecek şekilde ayarlayın:

`path: taskId + @"/output.txt"`

Bir görev kümesi için çıkış dosyalarının mutlak URI 'Leri şuna benzer olacaktır:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Azure depolama 'daki sanal dizinler hakkında daha fazla bilgi için bkz. [bir kapsayıcıdaki Blobları listeleme](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container).

## <a name="diagnose-file-upload-errors"></a>Karşıya dosya yükleme hatalarını tanılama

Çıktı dosyalarını Azure depolama 'ya yüklemek başarısız olursa, görev **tamamlandı** durumuna gider ve [taskexecutionınformation. FailureInformation](/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) özelliği ayarlanır. Hangi hatanın oluştuğunu belirlemek için **FailureInformation** özelliğini inceleyin. Örneğin, kapsayıcı bulunamazsa dosya yükleme sırasında oluşan bir hata aşağıda verilmiştir:

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of the specified Azure container(s) was not found while attempting to upload an output file
```

Her dosya yükleme işleminde toplu Işlem, işlem düğümüne iki günlük dosyası yazar `fileuploadout.txt` ve `fileuploaderr.txt` . Belirli bir hata hakkında daha fazla bilgi edinmek için bu günlük dosyalarını inceleyebilirsiniz. Dosya yüklemesinin hiçbir zaman denenmediği durumlarda (örneğin, görevin kendisi çalıştırılmadığından), bu günlük dosyaları mevcut olmayacaktır.

## <a name="diagnose-file-upload-performance"></a>Karşıya dosya yükleme performansını Tanıla

`fileuploadout.txt`Dosya günlükleri karşıya yükleme ilerleme durumu. Dosya karşıya yüklemelerinizin ne kadar süreceğine ilişkin daha fazla bilgi edinmek için bu dosyayı inceleyebilirsiniz. Düğüm boyutu, yükleme sırasında düğümdeki diğer etkinlik, hedef kapsayıcının toplu Iş havuzuyla aynı bölgede olup olmadığı, aynı anda depolama hesabına kaç düğüm karşıya yükleme vb. dahil olmak üzere performansı karşıya yüklemeye yönelik birçok farklı faktör olduğunu aklınızda bulundurun.

## <a name="use-the-batch-service-api-with-the-batch-file-conventions-standard"></a>Batch hizmeti API 'sini Batch dosyası kuralları standardı ile kullanma

Görev çıktısını Batch hizmeti API 'SI ile kalıcı hale uyguladığınızda, hedef kapsayıcınızı ve bloblarınızı dilediğiniz şekilde adı verebilirsiniz. [Toplu Iş dosya kuralları standardına](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files)göre bunları da seçebilirsiniz. Dosya kuralları standardı, belirli bir çıkış dosyası için Azure Storage 'daki Hedef kapsayıcının ve Blobun adlarını iş ve görevin adlarına göre belirler. Çıktı dosyalarını adlandırmak için dosya kuralları standardını kullanırsanız, çıkış dosyalarınız [Azure Portal](https://portal.azure.com)görüntülenmek üzere kullanılabilir.

C# dilinde geliştiriyorsanız, [.net Için toplu Işlem dosya kuralları kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files)'nda yerleşik olarak bulunan yöntemleri kullanabilirsiniz. Bu kitaplık sizin için uygun şekilde adlandırılmış kapsayıcılar ve BLOB yolları oluşturur. Örneğin, iş adına göre kapsayıcının doğru adını almak için API 'YI çağırabilirsiniz:

```csharp
string containerName = job.OutputStorageContainerName();
```

Kapsayıcıya yazmak için kullanılan bir paylaşılan erişim imzası (SAS) URL 'SI döndürmek için [Cloudjobextensions. GetOutputStorageContainerUrl](/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) yöntemini kullanabilirsiniz. Daha sonra bu SAS 'yi [Outputfileblobcontainerdestination](/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) oluşturucusuna geçirebilirsiniz.

C# dışında bir dilde geliştiriyorsanız, dosya kuralları standardını kendiniz uygulamanız gerekir.

## <a name="code-sample"></a>Kod örneği

[Persistoutkoyar](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs) örnek proje, GitHub 'daki [Azure Batch kod örneklerinden](https://github.com/Azure/azure-batch-samples) biridir. Bu Visual Studio çözümü, görev çıkışını dayanıklı depolamaya devam ettirmek için .NET için Batch istemci kitaplığı 'nın nasıl kullanılacağını gösterir. Örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Projeyi **Visual Studio 2019**' de açın.
2. Batch ve Storage **hesabı kimlik bilgilerinizi** Microsoft.Azure.Batch. Samples. Common projesindeki **accountsettings. Settings** ' e ekleyin.
3. Çözümü **oluşturun** (ancak çalıştırmayın). İstenirse tüm NuGet paketlerini geri yükleyin.
4. **PersistOutputsTask**için bir [uygulama paketini](batch-application-packages.md) karşıya yüklemek üzere Azure Portal kullanın. `PersistOutputsTask.exe`. Zip paketine ve bağımlı derlemelerini ekleyin, uygulama kimliğini "PersistOutputsTask" olarak ve uygulama paketi sürümünü "1,0" olarak ayarlayın.
5. **Persistoutkoyar** projesini **başlatın** (çalıştırın).
6. Örneği çalıştırmak için kullanılacak Kalıcılık teknolojisini seçmeniz istendiğinde, örnek için Batch hizmeti API 'sini kullanarak görev çıkışını kalıcı hale getirmek için **2** girin.
7. İsterseniz, örneği Batch hizmeti API 'SI ile kalıcı hale getirmek için **3** girerek ve ayrıca dosya kuralları standardına göre hedef kapsayıcısını ve BLOB yolunu adlandırmak için örneği yeniden çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

- .NET için dosya kuralları kitaplığı ile kalıcı görev çıktısı hakkında daha fazla bilgi edinmek için bkz. [.net Için toplu Işlem dosya kuralları kitaplığı ile iş ve görev verilerini Azure depolama 'Ya kalıcı hale](batch-task-output-file-conventions.md)getirme.
- Azure Batch çıktı verilerine yönelik diğer yaklaşımlar hakkında bilgi edinmek için bkz. [Azure Storage 'da iş ve görev çıkışını kalıcı hale](batch-task-output.md)getirme.
