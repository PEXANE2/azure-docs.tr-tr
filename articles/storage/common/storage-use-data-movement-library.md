---
title: .NET için veri taşıma kitaplığıyla veri aktarma
titleSuffix: Azure Storage
description: Veri taşıma kitaplığını kullanarak blob ve dosya içeriklerine veri taşıyın veya kopyalayın. Yerel dosyalardaki verileri Azure depolama 'ya kopyalayın veya depolama hesapları içinde veya arasında verileri kopyalayın. Verilerinizi kolayca Azure Storage 'a geçirin.
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5fe1e45f2ff80c1a212009291ab880a57f347fdf
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75978386"
---
# <a name="transfer-data-with-the-data-movement-library"></a>Veri Taşıma kitaplığıyla veri aktarma

Azure depolama veri taşıma kitaplığı, Blobları ve dosyaları karşıya yüklemek, indirmek ve kopyalamak için tasarlanan platformlar arası açık kaynaklı bir kitaplıktır. Bu kitaplık [AzCopy](../storage-use-azcopy.md)'i destekleyen temel veri taşıma çerçevesidir. Veri taşıma kitaplığı, .NET için Azure Storage istemci kitaplığı 'nda kullanılamayan kullanışlı yöntemler sunar. Bu yöntemler paralel işlemlerin sayısını ayarlamanıza, aktarım ilerlemesini izlemenize, iptal edilen bir aktarımı kolayca sürdürmeye ve çok daha fazlasına olanak sağlar.

Bu kitaplık ayrıca .NET Core kullanır, bu da Windows, Linux ve macOS için .NET uygulamaları oluştururken kullanabileceğiniz anlamına gelir. .NET Core hakkında daha fazla bilgi edinmek için [.NET Core belgelerine](https://dotnet.github.io/)bakın. Bu kitaplık, Windows için geleneksel .NET Framework uygulamaları için de geçerlidir.

Bu belgede, Windows, Linux ve macOS 'ta çalışan bir .NET Core konsol uygulamasının nasıl oluşturulacağı ve aşağıdaki senaryolar gerçekleştirilir:

- Dosya ve dizinleri blob depolamaya yükleyin.
- Veri aktarımı sırasında paralel işlem sayısını tanımlayın.
- Veri aktarımı ilerlemesini izleyin.
- Veri aktarımını iptal et.
- Dosyayı URL 'den blob depolamaya kopyalayın.
- Blob depolamadan blob depolamaya kopyalama.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio Code](https://code.visualstudio.com/)
- Bir [Azure Storage hesabı](storage-account-create.md)

## <a name="setup"></a>Kurulum

1. .NET Core yüklemek için [.NET Core yükleme kılavuzunu](https://www.microsoft.com/net/core) ziyaret edin. Ortamınızı seçerken komut satırı seçeneğini belirleyin.
2. Komut satırından projeniz için bir dizin oluşturun. Bu dizine gidin ve `dotnet new console -o <sample-project-name>` yazarak bir C# konsol projesi oluşturun.
3. Bu dizini Visual Studio Code açın. Bu adım, Windows 'ta `code .` yazarak komut satırı aracılığıyla hızla yapılabilir.
4. Uzantıyı Visual Studio Code marketi 'nden yüklersiniz. [ C# ](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio Code yeniden başlatın.
5. Bu noktada, iki istem görmeniz gerekir. Biri, "derlemek ve hata ayıklamak için gerekli varlıklar" eklemek içindir. "Evet" e tıklayın. Çözümlenmemiş bağımlılıkları geri yüklemek için başka bir istem vardır. "Geri yükle" ye tıklayın.
6. `.vscode` altındaki `launch.json`, bir konsol olarak dış Terminal kullanmak üzere değiştirin. Bu ayar `"console": "externalTerminal"` olarak okunmalıdır
7. Visual Studio Code, .NET Core uygulamalarında hata ayıklamanıza olanak tanır. Uygulamanızı çalıştırmak ve kurulumun çalıştığını doğrulamak için `F5` isabet edin. "Merhaba Dünya!" görmeniz gerekir konsola yazdırılır.

## <a name="add-the-data-movement-library-to-your-project"></a>Veri taşıma kitaplığını projenize ekleyin

1. Veri taşıma kitaplığının en son sürümünü `<project-name>.csproj` dosyanızın `dependencies` bölümüne ekleyin. Bu sürüm, yazma sırasında `"Microsoft.Azure.Storage.DataMovement": "0.6.2"`
2. Projenizi geri yüklemek için bir istem görüntülenmelidir. "Geri yükle" düğmesine tıklayın. Ayrıca, proje dizininizin köküne komut `dotnet restore` yazarak projenizi komut satırından da geri yükleyebilirsiniz.

`<project-name>.csproj`Değiştir:

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

## <a name="set-up-the-skeleton-of-your-application"></a>Uygulamanızın iskektlerini ayarlayın

Yaptığımız ilk şey, uygulamamız için "iskelet" kodunu ayarlamuyoruz. Bu kod, bir depolama hesabı adı ve hesap anahtarı için bize sorar ve bu kimlik bilgilerini bir `CloudStorageAccount` nesnesi oluşturmak için kullanır. Bu nesne, tüm aktarım senaryolarında depolama hesabınızla etkileşim kurmak için kullanılır. Kod ayrıca, çalıştırmak istediğimiz aktarım işlemi türünü seçmenizi ister.

`Program.cs`Değiştir:

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

## <a name="upload-a-local-file-to-a-blob"></a>Blob 'a yerel bir dosya yükleme

`Program.cs``GetSourcePath` ve `GetBlob` yöntemleri ekleyin:

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

`TransferLocalFileToAzureBlob` yöntemini değiştirin:

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

Bu kod, yerel bir dosyanın yolunu, yeni veya var olan bir kapsayıcının adını ve yeni bir Blobun adını ister. `TransferManager.UploadAsync` yöntemi, bu bilgileri kullanarak karşıya yüklemeyi gerçekleştirir.

Uygulamanızı çalıştırmak için `F5` tuşuna basın. [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)ile depolama hesabınızı görüntüleyerek karşıya yükleme işleminin gerçekleştiğini doğrulayabilirsiniz.

## <a name="set-the-number-of-parallel-operations"></a>Paralel işlem sayısını ayarla

Veri taşıma kitaplığı tarafından sunulan bir özellik, veri aktarımı aktarım hızını artırmak için paralel işlem sayısını ayarlamanıza olanak sağlar. Varsayılan olarak, veri taşıma kitaplığı, paralel işlem sayısını, makinenizde bulunan çekirdek sayısı olarak 8 * olarak ayarlar.

Düşük bant genişliğine sahip bir ortamdaki birçok paralel işlemin ağ bağlantısını tükendiğini ve gerçekten işlemlerin tamamen tamamlanmasını önlemeyeceğini aklınızda bulundurun. Kullanılabilir ağ bant genişliğinizi temel alarak neyin en iyi şekilde çalışacağını belirlemek için bu ayarla denemeler yapmanız gerekir.

Paralel işlemlerin sayısını ayarlayabileceğimizi sağlayan bir kod ekleyelim. Ayrıca, aktarımın tamamlanma süresini belirten bir kod ekleyelim.

`Program.cs`için bir `SetNumberOfParallelOperations` yöntemi ekleyin:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

`ExecuteChoice` yöntemi `SetNumberOfParallelOperations`kullanacak şekilde değiştirin:

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

`TransferLocalFileToAzureBlob` yöntemini bir Zamanlayıcı kullanacak şekilde değiştirin:

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

Verilerin aktarım için ne kadar sürdüğünü bilme yararlı olur. Ancak, aktarım işlemi *sırasında* aktarımın ilerlemesini görmek daha da iyidir. Bu senaryoya ulaşmak için `TransferContext` nesnesi oluşturmanız gerekir. `TransferContext` nesnesi iki şekilde gelir: `SingleTransferContext` ve `DirectoryTransferContext`. İlki tek bir dosyayı aktarmaya yöneliktir ve ikincisi bir dosya dizinini aktarmaya yöneliktir.

`Program.cs``GetSingleTransferContext` ve `GetDirectoryTransferContext` yöntemleri ekleyin:

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

`TransferLocalFileToAzureBlob` yöntemi `GetSingleTransferContext`kullanacak şekilde değiştirin:

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

## <a name="resume-a-canceled-transfer"></a>İptal edilen bir aktarımı sürdürür

Veri taşıma kitaplığı tarafından sunulan başka bir kullanışlı özellik, iptal edilen bir aktarımı sürdürmeye olanak sağlar. `c`yazarak, aktarımı geçici olarak iptal etmemizi sağlayan bir kod ekleyelim ve sonra 3 saniye sonra aktarıma geri dönelim.

`TransferLocalFileToAzureBlob`Değiştir:

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

Şimdi `checkpoint` değeri her zaman `null`olarak ayarlanmıştır. Artık aktarımı iptal etmemiz durumunda, aktarımımız son denetim noktasını aldık ve ardından bu yeni denetim noktasını aktarma bağlamımızda kullanacağız.

## <a name="transfer-a-local-directory-to-blob-storage"></a>Yerel bir dizini blob depolamaya aktarma

Veri taşıma kitaplığı tek seferde yalnızca bir dosya aktaracağından, bu durum söz konusu olabilir. Luckily, bu durum değildir. Veri taşıma kitaplığı, bir dosya dizinini ve tüm alt dizinlerini aktarma yeteneği sağlar. Yalnızca bunu yapmamızı sağlayan bir kod ekleyelim.

İlk olarak, `Program.cs``GetBlobDirectory` yöntemi ekleyin:

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

Sonra `TransferLocalDirectoryToAzureBlobDirectory`değiştirin:

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

Bu yöntem ve tek bir dosyayı karşıya yüklemek için yöntemi arasında birkaç fark vardır. Artık `TransferManager.UploadDirectoryAsync` ve daha önce oluşturduğumuz `getDirectoryTransferContext` yöntemi kullandık. Ayrıca, karşıya yükleme operasyonimize bir `options` değer sağlıyoruz. Bu, karşıya yükleme işlemi için alt dizinler eklemek istediğimizi göstermemizi sağlar.

## <a name="copy-a-file-from-url-to-a-blob"></a>URL 'den blob 'a dosya kopyalama

Şimdi de bir URL 'den Azure Blobuna bir dosya kopyalamamızı sağlayan bir kod ekleyelim.

`TransferUrlToAzureBlob`Değiştir:

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

Bu özellik için önemli bir kullanım durumu, başka bir bulut hizmetinden (ör. AWS) Azure 'a veri taşımanız gerektiğinde olur. Kaynağa erişim sağlayan bir URL olduğu sürece, `TransferManager.CopyAsync` yöntemini kullanarak bu kaynağı kolayca Azure Bloblarına taşıyabilirsiniz. Bu yöntem, yeni bir Boolean parametresi de tanıtır. Bu parametreyi `true` olarak ayarlamak, zaman uyumsuz bir sunucu tarafı kopyası yapmak istediğdiğimiz anlamına gelir. Bu parametrenin `false` olarak ayarlanması, zaman uyumlu bir kopya olduğunu belirtir-kaynağın önce yerel makinenize indirildiğini ve sonra Azure Blob 'a yüklendiğini gösterir. Ancak zaman uyumlu kopyalama Şu anda yalnızca bir Azure Storage kaynağından diğerine kopyalama için kullanılabilir.

## <a name="copy-a-blob"></a>Blob kopyalama

Veri taşıma kitaplığı tarafından benzersiz şekilde sağlanmış olan başka bir özellik, bir Azure Storage kaynağından diğerine kopyalama yeteneğidir.

`TransferAzureBlobToAzureBlob`Değiştir:

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

Bu örnekte, zaman uyumlu bir kopya yapmak istediğdiğimiz göstermek için `TransferManager.CopyAsync` Boole parametresini `false` olarak ayarlayacağız. Bu, kaynağın önce yerel makinenize indirileceği ve sonra Azure Blob 'a yüklendiği anlamına gelir. Zaman uyumlu kopyalama seçeneği, kopyalama işlemi için tutarlı bir hız olduğundan emin olmanın harika bir yoludur. Buna karşılık, zaman uyumsuz bir sunucu tarafı kopyasının hızı sunucudaki kullanılabilir ağ bant genişliğine bağımlıdır ve bu da dalgalanmasına sahip olabilir. Ancak zaman uyumlu kopya, zaman uyumsuz kopyaya kıyasla ek çıkış maliyeti oluşturabilir. Önerilen yaklaşım, çıkış maliyetini önlemek için kaynak depolama hesabınızla aynı bölgedeki bir Azure VM 'de zaman uyumlu kopyalama kullanmaktır.

Veri taşıma uygulaması artık tamamlanmıştır. [Tam kod örneği GitHub ' da kullanılabilir](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).

## <a name="next-steps"></a>Sonraki adımlar

[Azure depolama veri taşıma kitaplığı başvuru belgeleri](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]
