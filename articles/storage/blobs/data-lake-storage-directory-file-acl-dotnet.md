---
title: Azure Data Lake Storage 2. & ACL 'Ler için Azure .NET kullanma (Önizleme)
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Azure Storage istemci kitaplığı 'nı kullanın.
author: normesta
ms.service: storage
ms.date: 11/24/2019
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: fb69e0b797243a3403e8899d3f4ef23a9be9451d
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534269"
---
# <a name="use-net-for-files--acls-in-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake Storage 2. & ACL 'Leri için .NET kullanma (Önizleme)

Bu makalede, .NET kullanarak hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında Dizin, dosya ve izinleri oluşturma ve bunları yönetme işlemi gösterilmektedir. 

> [!IMPORTANT]
> Bu makalede tanıtılan [Azure. Storage. Files. DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/12.0.0-preview.6) NuGet paketi şu anda genel önizleme aşamasındadır.

[Paket (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/12.0.0-preview.6) | [örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake) | [API başvurusu](https://azuresdkdocs.blob.core.windows.net/$web/dotnet/Azure.Storage.Files.DataLake/12.0.0-preview.6/api/index.html) | [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md) | [geri bildirim verme](https://github.com/Azure/azure-sdk-for-net/issues)

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Başlamak için [Azure. Storage. Files. DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/12.0.0-preview.6) NuGet paketini yükledikten sonra.

NuGet paketlerinin nasıl yükleneceğine ilişkin daha fazla bilgi için bkz. [NuGet Paket Yöneticisi 'ni kullanarak Visual Studio 'da paketleri yüklemek ve yönetmek](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio).

Ardından, bu using deyimlerini kod dosyanızın en üstüne ekleyin.

```csharp
using Azure.Storage.Files.DataLake;
using Azure.Storage.Files.DataLake.Models;
using System.IO;
using Azure;
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. Bir tane almanın en kolay yolu, hesap anahtarı kullanmaktır. 

Bu örnek, bir hesap anahtarı kullanarak **DataLakeServiceClient** örneği oluşturur.

```cs
public void GetDataLakeServiceClient(ref DataLakeServiceClient dataLakeServiceClient,
    string accountName, string accountKey)
{
    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    string dfsUri = "https://" + accountName + ".dfs.core.windows.net";

    dataLakeServiceClient = new DataLakeServiceClient
        (new Uri(dfsUri), sharedKeyCredential);
}
```

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. **Filesystemclient. Createfilesystemmasync** yöntemini çağırarak bir tane oluşturabilirsiniz.

Bu örnek `my-file-system`adlı bir dosya sistemi oluşturur. 

```cs
public async Task<DataLakeFileSystemClient> CreateFileSystem
    (DataLakeServiceClient serviceClient)
{
        return await serviceClient.CreateFileSystemAsync("my-file-system");
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

**Filesystemclient. CreateDirectoryAsync** yöntemini çağırarak bir dizin başvurusu oluşturun.

Bu örnek, bir dosya sistemine `my-directory` adlı bir dizin ekler ve sonra `my-subdirectory`adlı bir alt dizin ekler. 

```cs
public async Task<DataLakeDirectoryClient> CreateDirectory
    (DataLakeServiceClient serviceClient, string fileSystemName)
{
    DataLakeFileSystemClient fileSystemClient =
        serviceClient.GetFileSystemClient(fileSystemName);

    DataLakeDirectoryClient directoryClient =
        await fileSystemClient.CreateDirectoryAsync("my-directory");

    return await directoryClient.CreateSubDirectoryAsync("my-subdirectory");
}
```

## <a name="rename-or-move-a-directory"></a>Dizini yeniden adlandırma veya taşıma

**Directoryclient. RenameAsync** yöntemini çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

Bu örnek, `my-subdirectory-renamed`adı için bir alt dizini yeniden adlandırır.

```cs
public async Task<DataLakeDirectoryClient> 
    RenameDirectory(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory/my-subdirectory");

    return await directoryClient.RenameAsync("my-directory/my-subdirectory-renamed");
}
```

Bu örnek, `my-subdirectory-renamed` adlı bir dizini `my-directory-2`adlı bir dizinin alt dizinine taşır. 

```cs
public async Task<DataLakeDirectoryClient> MoveDirectory
    (DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
            fileSystemClient.GetDirectoryClient("my-directory/my-subdirectory-renamed");

    return await directoryClient.RenameAsync("my-directory-2/my-subdirectory-renamed");                
}
```

## <a name="delete-a-directory"></a>Bir dizini silme

**Directoryclient. Delete** yöntemini çağırarak bir dizini silin.

Bu örnek, `my-directory`adlı bir dizini siler.  

```cs
public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    directoryClient.Delete();
}
```

## <a name="manage-a-directory-acl"></a>Dizin ACL 'sini yönetme

**Directoryclient. GetAccessControlAsync** yöntemini çağırarak bir dizinin erişim denetim LISTESINI (ACL) alın ve **Directoryclient. SETACCESSCONTROL** metodunu çağırarak ACL 'yi ayarlayın.

Bu örnek `my-directory`adlı bir dizinin ACL 'sini alır ve ayarlar. `user::rwx,group::r-x,other::rw-` dize, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma ve yazma izinlerini verir.

```cs
public async Task ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        await directoryClient.GetAccessControlAsync();

    Console.WriteLine(directoryAccessControl.Acl);

    directoryClient.SetAccessControl("user::rwx,group::r-x,other::rw-");

}

```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

İlk olarak, **Datalakefileclient** sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. **Datalakefileclient. Appendadsync** yöntemini çağırarak bir dosyayı karşıya yükleyin. **Datalakefileclient. FlushAsync** yöntemini çağırarak karşıya yüklemeyi tamamladığınızdan emin olun.

Bu örnek, `my-directory`adlı bir dizine bir metin dosyası yükler.    

```cs
public async Task UploadFile(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    DataLakeFileClient fileClient = await directoryClient.CreateFileAsync("uploaded-file.txt");

    FileStream fileStream = 
        File.OpenRead("C:\\file-to-upload.txt");

    long fileSize = fileStream.Length;

    await fileClient.AppendAsync(fileStream, offset: 0);

    await fileClient.FlushAsync(position: fileSize);

}
```

## <a name="manage-a-file-acl"></a>Dosya ACL 'sini yönetme

**Datalakefileclient. GetAccessControlAsync** yöntemini çağırarak bir dosyanın erişim denetim LISTESINI (ACL) alın ve **Fileclient. SetAccessControl** metodunu çağırarak ACL 'yi ayarlayın.

Bu örnek `my-file.txt`adlı bir dosyanın ACL 'sini alır ve ayarlar. `user::rwx,group::r-x,other::rw-` dize, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma ve yazma izinlerini verir.

```cs
public async Task ManageFileACLs(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.GetFileClient("hello.txt");

    PathAccessControl FileAccessControl =
        await fileClient.GetAccessControlAsync();

    Console.WriteLine(FileAccessControl.Acl);

    fileClient.SetAccessControl("user::rwx,group::r-x,other::rw-");
}
```

## <a name="download-from-a-directory"></a>Bir dizinden indir 

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir **Datalakefileclient** örneği oluşturun. **Fileclient. ReadAsync** yöntemini kullanın ve bir [Stream](https://docs.microsoft.com/dotnet/api/system.io.stream) nesnesi elde etmek için dönüş değerini ayrıştırın. Akıştan bir dosyaya bayt kaydetmek için herhangi bir .NET dosya işleme API 'sini kullanın. 

Bu örnek, bir dosyaya bayt kaydetmek için bir [BinaryReader](https://docs.microsoft.com/dotnet/api/system.io.binaryreader) ve [FILESTREAM](https://docs.microsoft.com/dotnet/api/system.io.filestream) kullanır. 

```cs
public async Task DownloadFile(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.GetFileClient("my-image.png");

    Response<FileDownloadInfo> downloadResponse = await fileClient.ReadAsync();

    BinaryReader reader = new BinaryReader(downloadResponse.Value.Content);

    FileStream fileStream = 
        File.OpenWrite("C:\\my-image-downloaded.png");

    int bufferSize = 4096;

    byte[] buffer = new byte[bufferSize];

    int count;

    while ((count = reader.Read(buffer, 0, buffer.Length)) != 0)
    {
        fileStream.Write(buffer, 0, count);
    }

    await fileStream.FlushAsync();

    fileStream.Close();
}
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

**Filesystemclient. ListPathsAsync** yöntemini çağırarak ve sonra sonuçlar arasında sıralama yaparak dizin içeriğini listeleyin.

Bu örnek, `my-directory`adlı bir dizinde bulunan her bir dosyanın adını yazdırır.

```cs
public async Task ListFilesInDirectory(DataLakeFileSystemClient fileSystemClient)
{
    IAsyncEnumerator<PathItem> enumerator = 
        fileSystemClient.ListPathsAsync("my-directory").GetAsyncEnumerator();

    await enumerator.MoveNextAsync();

    PathItem item = enumerator.Current;

    while (item != null)
    {
        Console.WriteLine(item.Name);

        if (!await enumerator.MoveNextAsync())
        {
            break;
        }
                
        item = enumerator.Current;
    }

}
```

## <a name="see-also"></a>Ayrıca bkz.

* [API başvuru belgeleri](https://azuresdkdocs.blob.core.windows.net/$web/dotnet/Azure.Storage.Files.DataLake/12.0.0-preview.6/api/index.html)
* [Paket (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/12.0.0-preview.6)
* [Örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake)
* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde bulunun](https://github.com/Azure/azure-sdk-for-net/issues)

