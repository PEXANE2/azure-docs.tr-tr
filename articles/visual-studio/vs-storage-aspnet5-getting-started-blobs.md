---
title: Visual Studio (ASP.NET Core) kullanarak blob depolama ile başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabı oluşturduktan sonra Visual Studio ASP.NET Core projesinde Azure Blob depolama alanını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 81df41470c893f569fd17345e8bdf4b29641ec64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298838"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Azure Blob depolama ve Visual Studio bağlantılı hizmetler (ASP.NET Core) ile başlayın

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

Bu makalede, Visual Studio Connected Services özelliğini kullanarak bir ASP.NET Core projesinde bir Azure depolama hesabı oluşturduktan veya başvurulan sonra Visual Studio'da Azure Blob depolama alanını kullanmaya nasıl başları nız **açıklanmaktadır.** **Bağlı Hizmetler** işlemi, projenizdeki Azure depolama alanına erişmek için uygun NuGet paketlerini yükler ve depolama hesabının bağlantı dizesini proje yapılandırma dosyalarınıza ekler. (Azure Depolama hakkında genel bilgi için [Depolama belgelerine](https://azure.microsoft.com/documentation/services/storage/) bakın.)

Azure Blob depolama, dünyanın herhangi bir yerinden http veya HTTPS üzerinden erişilebilen büyük miktarlarda yapılandırılmamış veri depolamak için bir hizmettir. Tek bir leke herhangi bir boyutta olabilir. Blobs görüntüler, ses ve video dosyaları, ham veri ve belge dosyaları gibi şeyler olabilir. Bu makalede, bir ASP.NET Core projesinde Visual Studio **Connected Services'ı** kullanarak bir Azure depolama hesabı oluşturduktan sonra blob depolama alanına nasıl başlanırsınız.

Dosyalar klasörlerde yaşadığı gibi, depolama lekeleri de kapsayıcılarda yaşar. Bir leke oluşturduktan sonra, bu blob bir veya daha fazla kapsayıcı lar oluşturun. Örneğin, "Not Defteri" adı verilen bir blob'da, resimleri depolamak için "görüntüler" adı verilen kapsayıcılar ve ses dosyalarını depolamak için "ses" adı verilen başka bir kapsayıcı oluşturabilirsiniz. Kapsayıcıları oluşturduktan sonra, tek tek dosyaları yükleyebilirsiniz. Bkz. [Quickstart: .NET'i kullanarak blob'ları yükleyin, indirin ve listeleyerek](../storage/blobs/storage-quickstart-blobs-dotnet.md) blob'ları programlı olarak manipüle etme hakkında daha fazla bilgi edin.

Azure Depolama API'lerinden bazıları eşzamanlıdır ve bu makaledeki kod, async yöntemlerinin kullanıldığını varsayar. Daha fazla bilgi için [Asynchronous programlamaya](https://docs.microsoft.com/dotnet/csharp/async) bakın.

## <a name="access-blob-containers-in-code"></a>Blob kaplarını kodda erişin

ASP.NET Core projelerindeki blob'lara programlı olarak erişmek için, zaten mevcut değilse aşağıdaki kodu eklemeniz gerekir:

1. Gerekli `using` ifadeleri ekleyin:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Extensions.Logging.LogLevel;
    ```

1. Depolama `CloudStorageAccount` hesabı bilgilerinizi temsil eden bir nesne alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerinizi almak için aşağıdaki kodu kullanın:

    ```cs
     CloudStorageAccount storageAccount = new CloudStorageAccount(
        new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
        "<storage-account-name>",
        "<access-key>"), true);
    ```

1. Depolama `CloudBlobClient` hesabınızdaki varolan bir kapsayıcıya başvuru almak için bir `CloudBlobContainer` nesne kullanın:

    ```cs
    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "mycontainer."
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    ```

## <a name="create-a-container-in-code"></a>Kodda kapsayıcı oluşturma

Ayrıca arayarak `CloudBlobClient` depolama hesabınızda bir kapsayıcı oluşturmak `CreateIfNotExistsAsync`için kullanabilirsiniz:

```cs
// Create a blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Get a reference to a container named "my-new-container."
CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

// If "mycontainer" doesn't exist, create it.
await container.CreateIfNotExistsAsync();
```

Kapsayıcıiçindeki dosyaları herkesin kullanımına açmak için kapsayıcıyı herkese açık olarak ayarlayın:

```cs
await container.SetPermissionsAsync(new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
});
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme

Bir blob dosyasını bir kapsayıcıya yüklemek için, bir kapsayıcı başvurusu alın ve blob başvurusu almak için kullanın. Ardından, yöntemi arayarak herhangi bir `UploadFromStreamAsync` veri akışını bu başvuruya yükleyin. Bu işlem, zaten orada değilse blob oluşturur ve varolan bir blob üzerine yazar. 

```cs
// Get a reference to a blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite the "myblob" blob with the contents of a local file
// named "myfile".
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    await blockBlob.UploadFromStreamAsync(fileStream);
}
```

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Bir kapsayıcıdaki lekeleri listelemek için, önce bir kapsayıcı `ListBlobsSegmentedAsync` başvurusu alın, ardından içindeki lekeleri ve/veya dizinleri almak için yöntemini arayın. `IListBlobItem`İade edilen bir nesne için zengin özellik ve yöntem `CloudBlockBlob` `CloudPageBlob`kümesine `CloudBlobDirectory` erişmek için, onu bir , veya nesneye atın. Blob türünü bilmiyorsanız, hangisini kullanacağınızı belirlemek için bir tür denetimi kullanın.

```cs
BlobContinuationToken token = null;
do
{
    BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
    token = resultSegment.ContinuationToken;

    foreach (IListBlobItem item in resultSegment.Results)
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
        }

        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
        }

        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }
} while (token != null);
```

Bkz. Hızlı Başlangıç: Blob kapsayıcısının içeriğini listeletmenin başka yolları için [.NET'i kullanarak blob'ları yükleyin, indirin ve listeleyin.](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container)

## <a name="download-a-blob"></a>Blob indirme

Bir blob indirmek için, ilk blob bir referans `DownloadToStreamAsync` almak, sonra yöntemi arayın. Aşağıdaki örnek, `DownloadToStreamAsync` blob içeriğini daha sonra yerel bir dosya olarak kaydedebilirsiniz bir akış nesnesine aktarmak için yöntemi kullanır.

```cs
// Get a reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save the blob contents to a file named "myfile".
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    await blockBlob.DownloadToStreamAsync(fileStream);
}
```

Bkz. Hızlı Başlangıç: Blobları dosya olarak kaydetmenin başka yolları için [.NET'i kullanarak blob'ları yükleyin, indirin ve listeleyin.](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs)

## <a name="delete-a-blob"></a>Blob silme

Bir blob silmek için, ilk blob bir `DeleteAsync` başvuru almak, sonra yöntemi arayın:

```cs
// Get a reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
await blockBlob.DeleteAsync();
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
