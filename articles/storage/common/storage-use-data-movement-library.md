---
title: .NET için Veri Hareketi kitaplığı ile veri aktarımı
titleSuffix: Azure Storage
description: Verileri blob ve dosya içeriğine taşımak veya kopyalamak için Veri Hareketi kitaplığını kullanın. Verileri yerel dosyalardan Azure Depolama'ya kopyalayın veya depolama hesapları içinde veya arasında veri kopyalayın. Verilerinizi Azure Depolama'ya kolayca aktarın.
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5b8654500fd697685b38e4f51ba1069e0cf6ccfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942901"
---
# <a name="transfer-data-with-the-data-movement-library"></a>Veri Taşıma kitaplığıyla veri aktarma

Azure Depolama Veri Hareketi kitaplığı, blob'ların ve dosyaların yüksek performanslı yükleme, indirme ve kopyalama için tasarlanmış çapraz platform açık kaynak kitaplığıdır. Veri Hareketi kitaplığı, .NET için Azure Depolama istemci kitaplığında bulunmayan kullanışlı yöntemler sağlar. Bu yöntemler, paralel işlem sayısını ayarlama, aktarım ilerlemesini izleme, iptal edilen aktarıma kolayca devam etme ve çok daha fazlasını sağlar.

Bu kitaplık aynı zamanda .NET Core'u da kullanır, bu da Windows, Linux ve macOS için .NET uygulamaları oluştururken kullanabileceğiniz anlamına gelir. .NET Core hakkında daha fazla bilgi edinmek için [.NET Core belgelerine](https://dotnet.github.io/)bakın. Bu kitaplık aynı zamanda Windows için geleneksel .NET Framework uygulamaları için de çalışır.

Bu belge, Windows, Linux ve macOS'ta çalışan ve aşağıdaki senaryoları gerçekleştiren bir .NET Core konsol uygulamasının nasıl oluşturulacağını gösterir:

- Dosyaları ve dizinleri Blob Depolama'ya yükleyin.
- Veri aktarırken paralel işlem sayısını tanımlayın.
- Veri aktarım ilerlemeyi izleyin.
- İptal edilen veri aktarımına devam edin.
- Dosyayı URL'den Blob Depolama'ya kopyalayın.
- Blob Depolama'dan Blob Depolama'ya kopyalayın.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio Code](https://code.visualstudio.com/)
- Azure [depolama hesabı](storage-account-create.md)

## <a name="setup"></a>Kurulum

1. .NET Core'u yüklemek için [.NET Çekirdek Yükleme Kılavuzu'nu](https://www.microsoft.com/net/core) ziyaret edin. Ortamınızı seçerken komut satırı seçeneğini seçin.
2. Komut satırından, projeniz için bir dizin oluşturun. Bu dizine gidin ve `dotnet new console -o <sample-project-name>` c# konsolu projesi oluşturmak için yazın.
3. Bu dizini Visual Studio Code'da açın. Bu adım Windows yazarak `code .` komut satırı üzerinden hızlı bir şekilde yapılabilir.
4. Visual Studio Code Marketplace'ten [C# uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) yükleyin. Visual Studio Kodunu Yeniden Başlatın.
5. Bu noktada, iki istem leri görmeniz gerekir. Bunlardan biri "oluşturmak ve hata ayıklamak için gerekli varlıkları" eklemek içindir. "Evet"i tıklatın. Başka bir istek çözülmemiş bağımlılıkları geri içindir. "Geri yükle"ye tıklayın.
6. Harici `launch.json` `.vscode` terminali konsol olarak kullanmak için altında değiştirin. Bu ayar şöyle olmalıdır:`"console": "externalTerminal"`
7. Visual Studio Code .NET Core uygulamalarını hata ayıklamanızı sağlar. Uygulamanızı çalıştırmak ve kurulumunuzun çalıştığını doğrulamak için vurun. `F5` "Merhaba Dünya!" konsola yazdırılır.

## <a name="add-the-data-movement-library-to-your-project"></a>Projenize Veri Hareketi kitaplığını ekleme

1. Dosyanızın `dependencies` `<project-name>.csproj` bölümüne Veri Hareketi kitaplığı'nın en son sürümünü ekleyin. Yazma sırasında, bu sürüm`"Microsoft.Azure.Storage.DataMovement": "0.6.2"`
2. Projenizi geri yüklemek için bir istem görüntülenmelidir. "Geri yükle" düğmesini tıklatın. Ayrıca, proje dizinizin köküne komut `dotnet restore` yazarak projenizi komut satırından geri yükleyebilirsiniz.

Değiştir `<project-name>.csproj`:

```xml
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Storage.DataMovement" Version="0.6.2" />
        </ItemGroup>
    </Project>
```

## <a name="set-up-the-skeleton-of-your-application"></a>Uygulamanızın iskeletini ayarlama

Yaptığımız ilk şey uygulamamızın "iskelet" kodunu kurmaktır. Bu kod bize bir Depolama hesabı adı ve hesap anahtarı `CloudStorageAccount` ister ve bir nesne oluşturmak için bu kimlik bilgilerini kullanır. Bu nesne, tüm aktarım senaryolarında Depolama hesabımızla etkileşim kurmak için kullanılır. Kod ayrıca yürütmek istediğimiz aktarım işlemi türünü seçmemizi de ister.

Değiştir `Program.cs`:

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Diagnostics;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;
using Microsoft.Azure.Storage.DataMovement;

namespace DMLibSample
{
    public class Program
    {
        public static void Main()
        {
            Console.WriteLine("Enter Storage account name:");
            string accountName = Console.ReadLine();

            Console.WriteLine("\nEnter Storage account key:");
            string accountKey = Console.ReadLine();

            string storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + accountName + ";AccountKey=" + accountKey;
            CloudStorageAccount account = CloudStorageAccount.Parse(storageConnectionString);

            ExecuteChoice(account);
        }

        public static void ExecuteChoice(CloudStorageAccount account)
        {
            Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
            int choice = int.Parse(Console.ReadLine());

            if(choice == 1)
            {
                TransferLocalFileToAzureBlob(account).Wait();
            }
            else if(choice == 2)
            {
                TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
            }
            else if(choice == 3)
            {
                TransferUrlToAzureBlob(account).Wait();
            }
            else if(choice == 4)
            {
                TransferAzureBlobToAzureBlob(account).Wait();
            }
        }

        public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
        {

        }

        public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
        {

        }
    }
}
```

## <a name="upload-a-local-file-to-a-blob"></a>Yerel bir dosyayı blob'a yükleme

Yöntemleri `GetSourcePath` ekleyin `GetBlob` ve `Program.cs`için:

```csharp
public static string GetSourcePath()
{
    Console.WriteLine("\nProvide path for source:");
    string sourcePath = Console.ReadLine();

    return sourcePath;
}

public static CloudBlockBlob GetBlob(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    Console.WriteLine("\nProvide name of new Blob:");
    string blobName = Console.ReadLine();
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    return blob;
}
```

Yöntemi `TransferLocalFileToAzureBlob` değiştirin:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    await TransferManager.UploadAsync(localFilePath, blob);
    Console.WriteLine("\nTransfer operation complete.");
    ExecuteChoice(account);
}
```

Bu kod, yerel bir dosyaya giden yol, yeni veya varolan bir kapsayıcının adı ve yeni bir blob'un adı için bizi ister. Yöntem `TransferManager.UploadAsync` bu bilgileri kullanarak yükleme gerçekleştirir.

Başvurunuzu çalıştırmak için vurun. `F5` Yüklemenin, Depolama hesabınızı [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)ile görüntüleyerek gerçekleştiğini doğrulayabilirsiniz.

## <a name="set-the-number-of-parallel-operations"></a>Paralel işlem sayısını ayarlama

Veri Hareketi kitaplığı tarafından sunulan özelliklerden biri, veri aktarım veri sini artırmak için paralel işlem sayısını ayarlama yeteneğidir. Varsayılan olarak, Veri Hareketi kitaplığı paralel işlem sayısını 8 *makinenizdeki çekirdek sayısını ayarlar.

Düşük bant genişliğine sahip bir ortamdaki birçok paralel operasyonun ağ bağlantısını bozabileceğini ve aslında işlemlerin tam olarak tamamlanmasını engelleyebileceğini unutmayın. Kullanılabilir ağ bant genişliğinize göre en iyi neyin çalıştığını belirlemek için bu ayarı denemeniz gerekir.

Paralel işlemlerin sayısını ayarlamamızı sağlayan bazı kodlar ekleyelim. Ayrıca, aktarımın tamamlanmasının ne kadar sürdüğünü gösteren kod da ekleyelim.

Bir `SetNumberOfParallelOperations` yöntem `Program.cs`ekle:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

Kullanılacak `SetNumberOfParallelOperations` `ExecuteChoice` yöntemi değiştirin:

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
    int choice = int.Parse(Console.ReadLine());

    SetNumberOfParallelOperations();

    if(choice == 1)
    {
        TransferLocalFileToAzureBlob(account).Wait();
    }
    else if(choice == 2)
    {
        TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
    }
    else if(choice == 3)
    {
        TransferUrlToAzureBlob(account).Wait();
    }
    else if(choice == 4)
    {
        TransferAzureBlobToAzureBlob(account).Wait();
    }
}
```

Zamanlayıcı `TransferLocalFileToAzureBlob` kullanmak için yöntemi değiştirin:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="track-transfer-progress"></a>Aktarım ilerlemesini izleme

Verilerin aktarılmasının ne kadar sürdüğünü bilmek yararlıdır. Ancak transfer işlemi *sırasında* transferin gidişatını görebilmek daha da iyi olacaktır. Bu senaryoyu gerçekleştirmek için bir `TransferContext` nesne oluşturmamız gerekir. Nesne `TransferContext` iki şekilde `SingleTransferContext` gelir: `DirectoryTransferContext`ve . Eski tek bir dosya aktarmak için ve ikinci dosyaların bir dizin aktarmak içindir.

Yöntemleri `GetSingleTransferContext` ekleyin `GetDirectoryTransferContext` ve `Program.cs`için:

```csharp
public static SingleTransferContext GetSingleTransferContext(TransferCheckpoint checkpoint)
{
    SingleTransferContext context = new SingleTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });

    return context;
}

public static DirectoryTransferContext GetDirectoryTransferContext(TransferCheckpoint checkpoint)
{
    DirectoryTransferContext context = new DirectoryTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });

    return context;
}
```

Kullanılacak `GetSingleTransferContext` `TransferLocalFileToAzureBlob` yöntemi değiştirin:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    Console.WriteLine("\nTransfer started...\n");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob, null, context);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="resume-a-canceled-transfer"></a>İptal edilen aktarıma devam etme

Veri Hareketi kitaplığı tarafından sunulan bir diğer kullanışlı özellik de iptal edilmiş bir aktarıma devam edebilme yeteneğidir. Bize geçici olarak yazarak `c`transferi iptal etmek için izin veren bazı kod ekleyelim , ve sonra 3 saniye sonra aktarım devam edelim.

Değiştir `TransferLocalFileToAzureBlob`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.UploadAsync(localFilePath, blob, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadAsync(localFilePath, blob, null, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Şimdiye kadar, `checkpoint` değerimiz her zaman `null`. Şimdi, transferi iptal edersek, transferimizin son kontrol noktasını geri alacağız ve bu yeni kontrol noktasını transfer bağlamımızda kullanacağız.

## <a name="transfer-a-local-directory-to-blob-storage"></a>Yerel bir dizini Blob depolama alanına aktarma

Veri Hareketi kitaplığı aynı anda yalnızca bir dosya aktarabilseydi hayal kırıklığına uğratıcı olurdu. Neyse ki durum böyle değil. Veri Hareketi kitaplığı, dosyaların ve tüm alt dizinlerinin dizini aktarımı sağlar. Bunu yapmamızı sağlayan bazı kodlar ekleyelim.

İlk olarak, `GetBlobDirectory` yöntemi `Program.cs`ekleyin:

```csharp
public static CloudBlobDirectory GetBlobDirectory(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container. This can be a new or existing Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    CloudBlobDirectory blobDirectory = container.GetDirectoryReference("");

    return blobDirectory;
}
```

Sonra, `TransferLocalDirectoryToAzureBlobDirectory`değiştirin:

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account);
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    UploadDirectoryOptions options = new UploadDirectoryOptions()
    {
        Recursive = true
    };

    try
    {
        task = TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetDirectoryTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Bu yöntem ve tek bir dosya yükleme yöntemi arasında birkaç fark vardır. Şimdi daha önce `TransferManager.UploadDirectoryAsync` oluşturduğumuz `getDirectoryTransferContext` yöntemi kullanıyoruz. Buna ek olarak, `options` yükleme işlemimize bir değer sağlıyoruz ve bu da yüklememize alt dizinler eklemek istediğimizi belirtmemizi sağlıyor.

## <a name="copy-a-file-from-url-to-a-blob"></a>Dosyayı URL'den blob'a kopyalama

Şimdi, bir dosyayı URL'den Azure Blob'a kopyalamamızı sağlayan kod ekleyelim.

Değiştir `TransferUrlToAzureBlob`:

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Bu özellik için önemli bir kullanım örneği, verileri başka bir bulut hizmetinden (örn. AWS) Azure'a taşımanız gerektiğidir. Kaynağa erişmenizi sağlayan bir URL'ye sahip olduğunuz sürece, `TransferManager.CopyAsync` yöntemi kullanarak bu kaynağı kolayca Azure Blobs'a taşıyabilirsiniz. Bu yöntem aynı zamanda yeni bir boolean parametresi tanır. Bu parametreyi, `true` sunucu tarafında eşzamanlı bir kopya yapmak istediğimizi gösterir şekilde ayarlamak. Bu parametreyi `false` eşzamanlı bir kopyayı gösterir şekilde ayarlamak, yani kaynak önce yerel makinemize indirilir, sonra Azure Blob'a yüklenir. Ancak, senkron kopya şu anda yalnızca bir Azure Depolama kaynağından diğerine kopyalamak için kullanılabilir.

## <a name="copy-a-blob"></a>Bir leke kopyalama

Veri Hareketi kitaplığı tarafından benzersiz olarak sağlanan bir diğer özellik de, bir Azure Depolama kaynağından diğerine kopyalama yeteneğidir.

Değiştir `TransferAzureBlobToAzureBlob`:

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(sourceBlob, destinationBlob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(sourceBlob, destinationBlob, false, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Bu örnekte, senkron bir kopya `TransferManager.CopyAsync` `false` yapmak istediğimizi belirtmek için boolean parametresini ayarlıyoruz. Bu, kaynağın önce yerel makinemize indirilir, sonra Azure Blob'a yüklenir. Senkron kopyalama seçeneği, kopyalama işleminizin tutarlı bir hıza sahip olmasını sağlamanın harika bir yoludur. Buna karşılık, bir eşzamanlı sunucu tarafı kopyanın hızı, sunucudaki kullanılabilir ağ bant genişliğine bağlıdır ve bu da dalgalanabilir. Ancak, senkron kopya, eşzamanlı kopyayla karşılaştırıldığında ek çıkış maliyeti oluşturabilir. Önerilen yaklaşım, çıkış maliyetini önlemek için kaynak depolama hesabınızla aynı bölgede bulunan bir Azure VM'de eşzamanlı kopya kullanmaktır.

Veri hareketi uygulaması tamamlandı. [Tam kod örneği GitHub'da mevcuttur.](https://github.com/azure-samples/storage-dotnet-data-movement-library-app)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Depolama Veri Hareketi kitaplığı başvuru belgeleri.](https://azure.github.io/azure-storage-net-data-movement)

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]
