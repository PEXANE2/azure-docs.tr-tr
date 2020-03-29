---
title: Dosyalar & ACD'ler için Azure Veri Gölü Depolama Gen2 .NET SDK
description: Hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinleri ve dosya ve dizin erişim denetim listelerini (ACL) yönetmek için Azure Depolama istemci kitaplığını kullanın.
author: normesta
ms.service: storage
ms.date: 03/20/2020
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: b83d0d2d765b60585832f1a3e7c610f05eac075c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061575"
---
# <a name="use-net-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için .NET'i kullanın

Bu makalede, hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinler, dosyalar ve izinler oluşturmak ve yönetmek için .NET'i nasıl kullanacağınızı gösterir. 

[Paket (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake) | [Örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake) | [API referans](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake) | [Gen1 Gen2 haritalama](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md) | [Geribildirim ver](https://github.com/Azure/azure-sdk-for-net/issues)

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkinleştirilmiş bir depolama hesabı. Oluşturmak için [bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Başlamak için [Azure.Storage.Files.DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/) NuGet paketini yükleyin.

NuGet paketlerinin nasıl yüklenirhakkında daha fazla bilgi için, [NuGet Paket Yöneticisi'ni kullanarak Visual Studio'da paketleri yükleyin ve yönetin.](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio)

Ardından, bunları kod dosyanızın en üstüne kullanarak ifadeler ekleyin.

```csharp
using Azure.Storage.Files.DataLake;
using Azure.Storage.Files.DataLake.Models;
using Azure.Storage;
using System.IO;
using Azure;
```

## <a name="connect-to-the-account"></a>Hesaba bağlanın

Bu makaledeki parçacıkları kullanmak için depolama hesabını temsil eden bir [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient) örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı nı kullanarak bağlanma

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?) örneği oluşturur.

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

### <a name="connect-by-using-azure-active-directory-ad"></a>Azure Etkin Dizin (AD) kullanarak bağlanın

Uygulamanızın kimliğini Azure AD ile doğrulamak [için .NET için Azure kimlik istemcisi kitaplığını](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) kullanabilirsiniz.

Bu örnek, bir istemci kimliği, istemci sırrı ve kiracı kimliği kullanarak bir [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?) örneği oluşturur.  Bu değerleri almak için, [istemci uygulamasından gelen istekleri yetkilendirmek için Azure AD'den bir belirteç edinin'e](../common/storage-auth-aad-app.md)bakın.

```cs
public void GetDataLakeServiceClient(ref DataLakeServiceClient dataLakeServiceClient, 
    String accountName, String clientID, string clientSecret, string tenantID)
{

    TokenCredential credential = new ClientSecretCredential(
        tenantID, clientID, clientSecret, new TokenCredentialOptions());

    string dfsUri = "https://" + accountName + ".dfs.core.windows.net";

    dataLakeServiceClient = new DataLakeServiceClient(new Uri(dfsUri), credential);
}

```

> [!NOTE]
> Daha fazla örnek için [,.NET belgeleri için Azure kimlik istemcisi kitaplığına](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) bakın.

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi, dosyalarınız için bir kapsayıcı görevi görür. [DataLakeServiceClient.CreateFileSystem](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient.createfilesystemasync) yöntemini arayarak bir tane oluşturabilirsiniz.

Bu örnek, .. `my-file-system` 

```cs
public async Task<DataLakeFileSystemClient> CreateFileSystem
    (DataLakeServiceClient serviceClient)
{
        return await serviceClient.CreateFileSystemAsync("my-file-system");
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

[DataLakeFileSystemClient.CreateDirectoryAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefilesystemclient.createdirectoryasync) yöntemini arayarak bir dizin başvurusu oluşturun.

Bu örnek, dosya `my-directory` sistemine bir dizin ekler ve sonra adlı `my-subdirectory`bir alt dizin ekler. 

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

[DataLakeDirectoryClient.RenameAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.renameasync) yöntemini arayarak bir dizini yeniden adlandırın veya taşıyın. İstenilen dizin bir parametre yolunu geçirin. 

Bu örnek, bir alt dizinin adını `my-subdirectory-renamed`yeniden adlandırır.

```cs
public async Task<DataLakeDirectoryClient> 
    RenameDirectory(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory/my-subdirectory");

    return await directoryClient.RenameAsync("my-directory/my-subdirectory-renamed");
}
```

Bu örnek, adlı `my-subdirectory-renamed` `my-directory-2`bir dizinin alt dizinine adlı bir dizin taşır. 

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

[DataLakeDirectoryClient.Delete](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.delete) yöntemini arayarak bir dizini silin.

Bu örnek, adlı `my-directory`bir dizini siler.  

```cs
public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    directoryClient.Delete();
}
```

## <a name="manage-a-directory-acl"></a>Bir dizin ACL yönetme

[DataLakeDirectoryClient.GetAccessControlAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.getaccesscontrolasync) yöntemini arayarak bir dizinin erişim denetim listesini (ACL) alın ve [DataLakeDirectoryClient.SetAccessControlList](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.setaccesscontrollist) yöntemini arayarak ACL'yi ayarlayın.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın. 

Bu örnek, adlı `my-directory`bir dizinin ACL'sini alır ve ayarlar. Dize, `user::rwx,group::r-x,other::rw-` sahibi kullanıcıya okuma, yazma ve izin yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve diğerlerine okuma ve yazma izni verir.

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

## <a name="upload-a-file-to-a-directory"></a>Dosyayı dizine yükleme

İlk olarak, [DataLakeFileClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient) sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. [DataLakeFileClient.AppendAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.appendasync) yöntemini arayarak dosya yükleyin. [DataLakeFileClient.FlushAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.flushasync) yöntemini arayarak yüklemeyi tamamladığından emin olun.

Bu örnek, bir metin dosyasını `my-directory`adlı bir dizine yükler.    

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

> [!TIP]
> Dosya boyutunuz büyükse, kodunuz [DataLakeFileClient.AppendAsync'e](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.appendasync)birden çok arama yapmak zorunda kalacaktır. Bunun yerine [DataLakeFileClient.UploadAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.uploadasync?view=azure-dotnet#Azure_Storage_Files_DataLake_DataLakeFileClient_UploadAsync_System_IO_Stream_) yöntemini kullanmayı düşünün. Bu şekilde, tüm dosyayı tek bir aramada yükleyebilirsiniz. 
>
> Örnek için sonraki bölüme bakın.

## <a name="upload-a-large-file-to-a-directory"></a>Büyük bir dosyayı dizine yükleme

[DataLakeFileClient.UploadAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.uploadasync?view=azure-dotnet#Azure_Storage_Files_DataLake_DataLakeFileClient_UploadAsync_System_IO_Stream_) yöntemini kullanarak [DataLakeFileClient.AppendAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.appendasync) yöntemine birden fazla arama yapmak zorunda kalmadan büyük dosyaları yükleyin.

```cs
public async Task UploadFileBulk(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    DataLakeFileClient fileClient = directoryClient.GetFileClient("uploaded-file.txt");

    FileStream fileStream =
        File.OpenRead("C:\\file-to-upload.txt");

    await fileClient.UploadAsync(fileStream);

}

```

## <a name="manage-a-file-acl"></a>Dosyayı ACL yönetme

[DataLakeFileClient.GetAccessControlAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.getaccesscontrolasync) yöntemini arayarak bir dosyanın erişim kontrol listesini (ACL) alın ve [DataLakeFileClient.SetAccessControlList](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.setaccesscontrollist) yöntemini arayarak ACL'yi ayarlayın.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın. 

Bu örnek, adlı `my-file.txt`bir dosyanın ACL'sini alır ve ayarlar. Dize, `user::rwx,group::r-x,other::rw-` sahibi kullanıcıya okuma, yazma ve izin yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve diğerlerine okuma ve yazma izni verir.

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

## <a name="download-from-a-directory"></a>Bir dizinden indirin 

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir [DataLakeFileClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient) örneği oluşturun. [DataLakeFileClient.ReadAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.readasync) yöntemini kullanın ve [Akış](https://docs.microsoft.com/dotnet/api/system.io.stream) nesnesi elde etmek için iade değerini ayrıştın. Akıştan bir dosyaya bayt kaydetmek için herhangi bir .NET dosya işleme API'sini kullanın. 

Bu örnek, bir dosyaya bayt kaydetmek için Bir [BinaryReader](https://docs.microsoft.com/dotnet/api/system.io.binaryreader) ve [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream) kullanır. 

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

[FileSystemClient.GetPathsAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefilesystemclient.getpathsasync) yöntemini arayarak ve ardından sonuçları listeleyerek dizin içeriğini listele.

Bu örnek, adlı `my-directory`bir dizinde bulunan her dosyanın adlarını yazdırır.

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
* [Gen1 - Gen2 haritalama](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde](https://github.com/Azure/azure-sdk-for-net/issues)

