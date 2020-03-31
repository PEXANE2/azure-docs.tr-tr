---
title: 'Quickstart: Azure Blob depolama kitaplığı v12 - .NET'
description: Bu hızlı başlangıçta, Blob (nesne) depolama alanında bir kapsayıcı ve bir blob oluşturmak için .NET için Azure Blob depolama istemcisi kitaplık 12 sürümünü nasıl kullanacağınızı öğrenirsiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobların listesini görüntülemeyi öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/05/2019
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: 5cfb0430bc94d347afd75bc01170a71a7ad53565
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240509"
---
# <a name="quickstart-azure-blob-storage-client-library-v12-for-net"></a>Quickstart: .NET için Azure Blob depolama istemcisi kitaplığı v12

.NET için Azure Blob depolama istemcisi kitaplığı v12 ile başlayın. Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Paketi yüklemek ve temel görevler için örnek kodu denemek için adımları izleyin. Blob depolama, çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir.

> [!NOTE]
> Önceki SDK sürümüyle başlamak [için Quickstart: .NET için Azure Blob depolama istemcisi kitaplığına](storage-quickstart-blobs-dotnet-legacy.md)bakın.

.NET için Azure Blob depolama istemcisi kitaplığını v12'yi kullanın:

* Bir kapsayıcı oluşturma
* Azure Depolama alanına blob yükleme
* Tüm lekeleri bir kapta listele
* Blob'u yerel bilgisayarınıza indirin
* Kapsayıcı silme

[API başvuru belgeleri](/dotnet/api/azure.storage.blobs) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs) | [Paketi (NuGet)](https://www.nuget.org/packages/Azure.Storage.Blobs) | [Örnekleri](https://docs.microsoft.com/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-samples)

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı - [bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için geçerli [.NET Core SDK.](https://dotnet.microsoft.com/download/dotnet-core) SDK değil, çalışma zamanı almak için emin olun.

## <a name="setting-up"></a>Ayarlama

Bu bölüm, .NET için Azure Blob depolama istemcisi kitaplığı v12 ile çalışmak üzere bir proje hazırlamakonusunda size yol sunar.

### <a name="create-the-project"></a>Proje oluşturma

*BlobQuickstartV12*adında bir .NET Core uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` *BlobQuickstartV12*adında yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: *Program.cs*.

   ```console
   dotnet new console -n BlobQuickstartV12
   ```

1. Yeni oluşturulan *BlobQuickstartV12* dizinine geçin.

   ```console
   cd BlobQuickstartV12
   ```

1. *BlobQuickstartV12* dizininin yan tarafında, *veri*adı verilen başka bir dizin oluşturun. Burada blob veri dosyaları oluşturulacak ve depolanır.

    ```console
    mkdir data
    ```

### <a name="install-the-package"></a>Paketi yükleyin

Uygulama dizinindeyken ,komutu kullanarak .NET paketi için Azure Blob `dotnet add package` depolama istemcisi kitaplığını yükleyin.

```console
dotnet add package Azure.Storage.Blobs
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Düzenleyicinizdeki *Program.cs* dosyasını açma
1. İfadeyi `Console.WriteLine("Hello World!");` kaldırma
1. `using` Yönergeekleme
1. Async `Main` kodunu desteklemek için yöntem bildirimini güncelleştirme

İşte kod:

```csharp
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using System;
using System.IO;
using System.Threading.Tasks;

namespace BlobQuickstartV12
{
    class Program
    {
        static async Task Main()
        {
        }
    }
}
```

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Blob depolama, büyük miktarda yapılandırılmamış veri depolamak için optimize edilebiyi sunar. Yapılandırılmamış veriler, metin veya ikili veriler gibi belirli bir veri modeline veya tanıma bağlı olmayan verilerdir. Blob depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabındaki bir kapsayıcı
* Konteynerde bir leke

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Blob depolama mimarisi diyagramı](./media/storage-blobs-introduction/blob1.png)

Bu kaynaklarla etkileşimkurmak için aşağıdaki .NET sınıflarını kullanın:

* [BlobServiceClient](/dotnet/api/azure.storage.blobs.blobserviceclient): `BlobServiceClient` Sınıf, Azure Depolama kaynaklarını ve blob kapsayıcılarını işlemenizi sağlar.
* [BlobContainerClient](/dotnet/api/azure.storage.blobs.blobcontainerclient): `BlobContainerClient` Sınıf, Azure Depolama kapsayıcılarını ve bunların lekelerini işlemenizi sağlar.
* [BlobClient](/dotnet/api/azure.storage.blobs.blobclient): `BlobClient` Sınıf, Azure Depolama lekelerini işlemenizi sağlar.
* [BlobDownloadInfo](/dotnet/api/azure.storage.blobs.models.blobdownloadinfo): `BlobDownloadInfo` Sınıf, bir blob indirmeden döndürülen özellikleri ve içeriği temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, .NET için Azure Blob depolama istemcisi kitaplığı yla aşağıdakileri nasıl gerçekleştireceklerini gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kapsayıcı oluşturma](#create-a-container)
* [Blob'ları bir kapsayıcıya yükleme](#upload-blobs-to-a-container)
* [Blob’ları bir kapsayıcıda listeleme](#list-the-blobs-in-a-container)
* [Blob’ları indirme](#download-blobs)
* [Kapsayıcı silme](#delete-a-container)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, [depolama bağlantı dizesi bölümüyapılanınoluşturulan](#configure-your-storage-connection-string) ortam değişkeninden depolama hesabının bağlantı dizesini alır.

Bu kodu yöntemin `Main` içine ekleyin:

```csharp
Console.WriteLine("Azure Blob storage v12 - .NET quickstart sample\n");

// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called AZURE_STORAGE_CONNECTION_STRING. If the
// environment variable is created after the application is launched in a
// console or with Visual Studio, the shell or application needs to be closed
// and reloaded to take the environment variable into account.
string connectionString = Environment.GetEnvironmentVariable("AZURE_STORAGE_CONNECTION_STRING");
```

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Yeni kapsayıcı için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için kapsayıcı adına bir GUID değeri ekler.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcıları ve blobları adlandırma hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

[BlobServiceClient](/dotnet/api/azure.storage.blobs.blobserviceclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızdaki kapsayıcıyı oluşturmak için [CreateBlobContainerAsync](/dotnet/api/azure.storage.blobs.blobserviceclient.createblobcontainerasync) yöntemini arayın.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
// Create a BlobServiceClient object which will be used to create a container client
BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

//Create a unique name for the container
string containerName = "quickstartblobs" + Guid.NewGuid().ToString();

// Create the container and return a container client object
BlobContainerClient containerClient = await blobServiceClient.CreateBlobContainerAsync(containerName);
```

### <a name="upload-blobs-to-a-container"></a>Blob'ları bir kapsayıcıya yükleme

Aşağıdaki kod parçacığı:

1. Yerel *veri* dizininde bir metin dosyası oluşturur.
1. [Kapsayıcı](#create-a-container) daktankiler üzerindeki [GetBlobClient](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobclient) yöntemini arayarak [BlobClient](/dotnet/api/azure.storage.blobs.blobclient) nesnesine başvuru alır.
1. [UploadAsync](/dotnet/api/azure.storage.blobs.blobclient.uploadasync#Azure_Storage_Blobs_BlobClient_UploadAsync_System_IO_Stream_System_Boolean_System_Threading_CancellationToken_) yöntemini arayarak yerel metin dosyasını blob'a yükler. Bu yöntem, daha önce oluşturulmadıysa bir blob oluşturur, aksi takdirde üzerine yazar.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
// Create a local file in the ./data/ directory for uploading and downloading
string localPath = "./data/";
string fileName = "quickstart" + Guid.NewGuid().ToString() + ".txt";
string localFilePath = Path.Combine(localPath, fileName);

// Write text to the file
await File.WriteAllTextAsync(localFilePath, "Hello, World!");

// Get a reference to a blob
BlobClient blobClient = containerClient.GetBlobClient(fileName);

Console.WriteLine("Uploading to Blob storage as blob:\n\t {0}\n", blobClient.Uri);

// Open the file and upload its data
using FileStream uploadFileStream = File.OpenRead(localFilePath);
await blobClient.UploadAsync(uploadFileStream, true);
uploadFileStream.Close();
```

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[GetBlobsAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsasync) yöntemini arayarak kabın içinde lekeleri listeleyin. Bu durumda, kapsayıcıya yalnızca bir blob eklenmiştir, bu nedenle listeleme işlemi yalnızca bir blob döndürür.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
Console.WriteLine("Listing blobs...");

// List all blobs in the container
await foreach (BlobItem blobItem in containerClient.GetBlobsAsync())
{
    Console.WriteLine("\t" + blobItem.Name);
}
```

### <a name="download-blobs"></a>Blob’ları indirme

[DownloadAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.downloadasync) yöntemini arayarak daha önce oluşturulmuş blob'u indirin. Örnek kod, yerel dosya sisteminde her iki dosyayı görebilmeniz için dosya adına "DOWNLOADED" eki ekler.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
// Download the blob to a local file
// Append the string "DOWNLOAD" before the .txt extension 
// so you can compare the files in the data directory
string downloadFilePath = localFilePath.Replace(".txt", "DOWNLOAD.txt");

Console.WriteLine("\nDownloading blob to\n\t{0}\n", downloadFilePath);

// Download the blob's contents and save it to a file
BlobDownloadInfo download = await blobClient.DownloadAsync();

using (FileStream downloadFileStream = File.OpenWrite(downloadFilePath))
{
    await download.Content.CopyToAsync(downloadFileStream);
    downloadFileStream.Close();
}
```

### <a name="delete-a-container"></a>Kapsayıcı silme

Aşağıdaki [kod, DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteasync)kullanarak tüm kapsayıcıyı silerek oluşturulan kaynakları temizler. Ayrıca uygulama tarafından oluşturulan yerel dosyaları da siler.

Uygulama, blob, kapsayıcı ve `Console.ReadLine` yerel dosyaları silmeden önce arayarak kullanıcı girişi için duraklar. Bu, kaynakların silinmeden önce doğru oluşturulduğunu doğrulamak için iyi bir şanstır.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
// Clean up
Console.Write("Press any key to begin clean up");
Console.ReadLine();

Console.WriteLine("Deleting blob container...");
await containerClient.DeleteAsync();

Console.WriteLine("Deleting the local source and downloaded files...");
File.Delete(localFilePath);
File.Delete(downloadFilePath);

Console.WriteLine("Done");
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama yerel *veri* klasörünüzde bir test dosyası oluşturur ve blob depolama ya yükler. Örnek daha sonra kapsayıcıdaki lekeleri listeler ve eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla karşıdan yükler.

Uygulama dizininize gidin, ardından uygulamayı oluşturun ve çalıştırın.

```console
dotnet build
```

```console
dotnet run
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Blob storage v12 - .NET quickstart sample

Uploading to Blob storage as blob:
         https://mystorageacct.blob.core.windows.net/quickstartblobs60c70d78-8d93-43ae-954d-8322058cfd64/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Listing blobs...
        quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Downloading blob to
        ./data/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31DOWNLOADED.txt

Press any key to begin clean up
Deleting blob container...
Deleting the local source and downloaded files...
Done
```

Temizleme işlemine başlamadan önce, iki dosya için *veri* klasörünüzü denetleyin. Dosyaları açarak aynı olduklarını görebilirsiniz.

Dosyaları doğruladıktan sonra, test dosyalarını silmek ve demoyu bitirmek için **Enter** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta .NET kullanarak blobları karşıya yükleme, indirme ve listeleme hakkında bilgi edindiniz.

Blob depolama örnek uygulamalarını görmek için şunları yapmaya devam edin:

> [!div class="nextstepaction"]
> [Azure Blob depolama SDK v12 .NET örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples)

* Öğreticiler, örnekler, hızlı başlangıçlar ve diğer belgeler [için .NET ve .NET Core geliştiricileri için Azure'u ziyaret edin.](/dotnet/azure/)
* .NET Core hakkında daha fazla bilgi için bkz. [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/).
