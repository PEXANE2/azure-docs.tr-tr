---
title: "& ACL 'Ler için Java SDK Azure Data Lake Storage 2. (Önizleme)"
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Java için Azure depolama kitaplıklarını kullanın.
author: normesta
ms.service: storage
ms.date: 11/24/2019
ms.author: normesta
ms.topic: conceptual
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 41e93ad19e21ae81843992b1b190d1bd37585c2e
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75835088"
---
# <a name="use-java-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake Storage 2. (Önizleme) içinde dizinleri, dosyaları ve ACL 'Leri yönetmek için Java 'Yı kullanma

Bu makalede, Java kullanarak hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında Dizin, dosya ve izinleri oluşturma ve bunları yönetme işlemi gösterilmektedir. 

> [!IMPORTANT]
> Bu makalede tanıtılan Java kitaplığı şu anda genel önizleme aşamasındadır.

[Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) | [örnekleri](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake) | [API başvurusu](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.0.0-preview.6/index.html) | [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | [geri bildirim verme](https://github.com/Azure/azure-sdk-for-java/issues)

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Başlamak için [Bu sayfayı](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) açın ve Java kitaplığı 'nın en son sürümünü bulun. Sonra, metin düzenleyicinizde *pom. xml* dosyasını açın. Bu sürüme başvuran bir bağımlılık öğesi ekleyin.

Ardından, bu Imports deyimlerini kod dosyanıza ekleyin.

```java
import com.azure.storage.common.StorageSharedKeyCredential;
import com.azure.storage.file.datalake.DataLakeDirectoryClient;
import com.azure.storage.file.datalake.DataLakeFileClient;
import com.azure.storage.file.datalake.DataLakeFileSystemClient;
import com.azure.storage.file.datalake.DataLakeServiceClient;
import com.azure.storage.file.datalake.DataLakeServiceClientBuilder;
import com.azure.storage.file.datalake.models.ListPathsOptions;
import com.azure.storage.file.datalake.models.PathAccessControl;
import com.azure.storage.file.datalake.models.PathAccessControlEntry;
import com.azure.storage.file.datalake.models.PathItem;
import com.azure.storage.file.datalake.models.PathPermissions;
import com.azure.storage.file.datalake.models.RolePermissions;
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan 

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. Bir tane almanın en kolay yolu, hesap anahtarı kullanmaktır. 

Bu örnek, bir hesap anahtarı kullanarak **DataLakeServiceClient** örneği oluşturur.

```java

static public DataLakeServiceClient GetDataLakeServiceClient
(String accountName, String accountKey){

    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    DataLakeServiceClientBuilder builder = new DataLakeServiceClientBuilder();

    builder.credential(sharedKeyCredential);
    builder.endpoint("https://" + accountName + ".dfs.core.windows.net");

    return builder.buildClient();
}      

```
## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. **DataLakeServiceClient. createFileSystem** metodunu çağırarak bir tane oluşturabilirsiniz.

Bu örnek `my-file-system`adlı bir dosya sistemi oluşturur. 

```java
static public DataLakeFileSystemClient CreateFileSystem
(DataLakeServiceClient serviceClient){

    return serviceClient.createFileSystem("my-file-system");
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

**Datalakefilesystemclient. createDirectory** metodunu çağırarak bir dizin başvurusu oluşturun.

Bu örnek, bir dosya sistemine `my-directory` adlı bir dizin ekler ve sonra `my-subdirectory`adlı bir alt dizin ekler. 

```java
static public DataLakeDirectoryClient CreateDirectory
(DataLakeServiceClient serviceClient, String fileSystemName){
    
    DataLakeFileSystemClient fileSystemClient =
    serviceClient.getFileSystemClient(fileSystemName);

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.createDirectory("my-directory");

    return directoryClient.createSubDirectory("my-subdirectory");
}
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

**Datalakedirectoryclient. Rename** metodunu çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

Bu örnek, `my-subdirectory-renamed`adı için bir alt dizini yeniden adlandırır.

```java
static public DataLakeDirectoryClient
    RenameDirectory(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory");

    return directoryClient.rename("my-directory/my-subdirectory-renamed");
}
```

Bu örnek, `my-subdirectory-renamed` adlı bir dizini `my-directory-2`adlı bir dizinin alt dizinine taşır. 

```java
static public DataLakeDirectoryClient MoveDirectory
(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory-renamed");

    return directoryClient.rename("my-directory-2/my-subdirectory-renamed");                
}
```

## <a name="delete-a-directory"></a>Bir dizini silme

**Datalakedirectoryclient. deleteWithResponse** yöntemini çağırarak bir dizini silin.

Bu örnek, `my-directory`adlı bir dizini siler.   

```java
static public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient){
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    directoryClient.deleteWithResponse(true, null, null, null);
}
```

## <a name="manage-a-directory-acl"></a>Dizin ACL 'sini yönetme

Bu örnek, `my-directory`adlı bir dizinin ACL 'sini alır ve ayarlar. Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

```java
static public void ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        directoryClient.getAccessControl();

    List<PathAccessControlEntry> pathPermissions = directoryAccessControl.getAccessControlList();
       
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));
             
    PathPermissions permissions = new PathPermissions()

      .group(new RolePermissions().execute(true).read(true))
      .owner(new RolePermissions().execute(true).read(true).write(true))
      .other(new RolePermissions().read(true));

    directoryClient.setPermissions(permissions, null, null);

    pathPermissions = directoryClient.getAccessControl().getAccessControlList();
     
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}

```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

İlk olarak, **Datalakefileclient** sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. **Datalakefileclient. Append** metodunu çağırarak bir dosyayı karşıya yükleyin. **Datalakefileclient. FlushAsync** yöntemini çağırarak karşıya yüklemeyi tamamladığınızdan emin olun.

Bu örnek `my-directory`. ' adlı dizine bir metin dosyası yükler.

```java
static public void UploadFile(DataLakeFileSystemClient fileSystemClient) 
    throws FileNotFoundException{
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = directoryClient.createFile("uploaded-file.txt");

    File file = new File("C:\\mytestfile.txt");

    InputStream targetStream = new FileInputStream(file);

    long fileSize = file.length();

    fileClient.append(targetStream, 0, fileSize);

    fileClient.flush(fileSize);
}
```

## <a name="manage-a-file-acl"></a>Dosya ACL 'sini yönetme

Bu örnek, `upload-file.txt`adlı bir dosyanın ACL 'sini alır ve ayarlar. Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

```java
static public void ManageFileACLs(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.getFileClient("uploaded-file.txt");

    PathAccessControl fileAccessControl =
        fileClient.getAccessControl();

    List<PathAccessControlEntry> pathPermissions = fileAccessControl.getAccessControlList();
     
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));
           
    PathPermissions permissions = new PathPermissions()

        .group(new RolePermissions().execute(true).read(true))
        .owner(new RolePermissions().execute(true).read(true).write(true))
        .other(new RolePermissions().read(false));

    fileClient.setPermissions(permissions, null, null);

    pathPermissions = fileClient.getAccessControl().getAccessControlList();
   
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}
```

## <a name="download-from-a-directory"></a>Bir dizinden indir

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir **Datalakefileclient** örneği oluşturun. Dosyayı okumak için **Datalakefileclient. Read** metodunu kullanın. Akıştan bir dosyaya bayt kaydetmek için herhangi bir .NET dosya işleme API 'sini kullanın. 

**Datalakefileclient. Flush** metodunu çağırarak indirmeyi tamamladığınızdan emin olun.

```java
static public void DownloadFile(DataLakeFileSystemClient fileSystemClient)
    throws FileNotFoundException, java.io.IOException{

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.getFileClient("uploaded-file.txt");

    File file = new File("C:\\downloadedFile.txt");

    OutputStream targetStream = new FileOutputStream(file);

    fileClient.read(targetStream);

    targetStream.close();

    fileClient.flush(file.length());
        
}

```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Bu örnek, `my-directory`adlı bir dizinde bulunan her bir dosyanın adını yazdırır.

```java
static public void ListFilesInDirectory(DataLakeFileSystemClient fileSystemClient){
        
    ListPathsOptions options = new ListPathsOptions();
    options.setPath("my-directory");
     
    PagedIterable<PathItem> pagedIterable = 
    fileSystemClient.listPaths(options, null);

    java.util.Iterator<PathItem> iterator = pagedIterable.iterator();
       
    PathItem item = iterator.next();

    while (item != null)
    {
        System.out.println(item.getName());


        if (!iterator.hasNext())
        {
            break;
        }
            
        item = iterator.next();
    }

}
```

## <a name="see-also"></a>Ayrıca bkz.

* [API başvuru belgeleri](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.0.0-preview.6/index.html)
* [Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake/12.0.0-preview.6/jar)
* [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)
* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde bulunun](https://github.com/Azure/azure-sdk-for-java/issues)