---
title: "& ACL 'Ler için Azure Data Lake Storage 2. .NET SDK (Önizleme)"
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Azure Storage istemci kitaplığı 'nı kullanın.
author: normesta
ms.service: storage
ms.date: 01/09/2020
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 76fab93543310252bb9003029573f9d3f1ff62b6
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834989"
---
# <a name="use-net-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake Storage 2. (Önizleme) içinde dizinleri, dosyaları ve ACL 'Leri yönetmek için .NET kullanın

Bu makalede, .NET kullanarak hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında Dizin, dosya ve izinleri oluşturma ve bunları yönetme işlemi gösterilmektedir. 

> [!IMPORTANT]
> Bu makalede tanıtılan [Azure. Storage. Files. DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake) NuGet paketi şu anda genel önizleme aşamasındadır.

[Paket (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake) | [örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake) | [API başvurusu](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake) | [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md) | [geri bildirim verme](https://github.com/Azure/azure-sdk-for-net/issues)

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Başlamak için [Azure. Storage. Files. DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/) NuGet paketini yükledikten sonra.

NuGet paketlerinin nasıl yükleneceğine ilişkin daha fazla bilgi için bkz. [NuGet Paket Yöneticisi 'ni kullanarak Visual Studio 'da paketleri yüklemek ve yönetmek](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio).

Ardından, bu using deyimlerini kod dosyanızın en üstüne ekleyin.

```csharp
using Azure.Storage.Files.DataLake;
using Azure.Storage.Files.DataLake.Models;
using System.IO;
using Azure;
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient) örneği oluşturmanız gerekir. Bir tane almanın en kolay yolu, hesap anahtarı kullanmaktır. 

Bu örnek, bir hesap anahtarı kullanarak [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?) örneği oluşturur.

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

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. [DataLakeServiceClient. CreateFileSystem](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient.createfilesystemasync) metodunu çağırarak bir tane oluşturabilirsiniz.

Bu örnek `my-file-system`adlı bir dosya sistemi oluşturur. 

```cs
public async Task<DataLakeFileSystemClient> CreateFileSystem
    (DataLakeServiceClient serviceClient)
{
        return await serviceClient.CreateFileSystemAsync("my-file-system");
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

[Datalakefilesystemclient. CreateDirectoryAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefilesystemclient.createdirectoryasync) yöntemini çağırarak bir dizin başvurusu oluşturun.

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

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

[Datalakedirectoryclient. RenameAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.renameasync) yöntemini çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

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

[Datalakedirectoryclient. Delete](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.delete) yöntemini çağırarak bir dizini silin.

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

[Datalakedirectoryclient. GetAccessControlAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.getaccesscontrolasync) yöntemini çağırarak bir dizinin erişim denetim LISTESINI (ACL) alın ve [Datalakedirectoryclient. SetAccessControlList](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.setaccesscontrollist) metodunu çağırarak ACL 'yi ayarlayın.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın. 

Bu örnek `my-directory`adlı bir dizinin ACL 'sini alır ve ayarlar. `user::rwx,group::r-x,other::rw-` dize, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma ve yazma izinlerini verir.

```cs
public async Task ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        await directoryClient.GetAccessControlAsync();

    Console.WriteLine(directoryAccessControl.AccessControlList);

    IList<PathAccessControlItem> accessControlList
        = PathAccessControlExtensions.ParseAccessControlList
        ("user::rwx,group::r-x,other::rw-");

    directoryClient.SetAccessControlList(accessControlList);

}

```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

İlk olarak, [Datalakefileclient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient) sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. [Datalakefileclient. Appendadsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.appendasync) yöntemini çağırarak bir dosyayı karşıya yükleyin. [Datalakefileclient. FlushAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.flushasync) yöntemini çağırarak karşıya yüklemeyi tamamladığınızdan emin olun.

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

[Datalakefileclient. GetAccessControlAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.getaccesscontrolasync) yöntemini çağırarak bir dosyanın erişim denetim LISTESINI (ACL) alın ve [Datalakefileclient. SetAccessControlList](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.setaccesscontrollist) metodunu çağırarak ACL 'yi ayarlayın.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın. 

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

    Console.WriteLine(FileAccessControl.AccessControlList);

    IList<PathAccessControlItem> accessControlList
        = PathAccessControlExtensions.ParseAccessControlList
        ("user::rwx,group::r-x,other::rw-");

    fileClient.SetAccessControlList(accessControlList);
}
```

## <a name="download-from-a-directory"></a>Bir dizinden indir 

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir [Datalakefileclient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient) örneği oluşturun. [Datalakefileclient. ReadAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.readasync) yöntemini kullanın ve bir [Stream](https://docs.microsoft.com/dotnet/api/system.io.stream) nesnesi elde etmek için dönüş değerini ayrıştırın. Akıştan bir dosyaya bayt kaydetmek için herhangi bir .NET dosya işleme API 'sini kullanın. 

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

[Filesystemclient. GetPathsAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefilesystemclient.getpathsasync) yöntemini çağırarak ve sonra sonuçlar arasında sıralama yaparak dizin içeriğini listeleyin.

Bu örnek, `my-directory`adlı bir dizinde bulunan her bir dosyanın adını yazdırır.

```cs
public async Task ListFilesInDirectory(DataLakeFileSystemClient fileSystemClient)
{
    IAsyncEnumerator<PathItem> enumerator = 
        fileSystemClient.GetPathsAsync("my-directory").GetAsyncEnumerator();

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

* [API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake)
* [Paket (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake)
* [Örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake)
* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde bulunun](https://github.com/Azure/azure-sdk-for-net/issues)

