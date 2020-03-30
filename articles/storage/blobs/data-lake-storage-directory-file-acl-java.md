---
title: Dosyaları & ACD'ler için Azure Veri Gölü Depolama Gen2 Java SDK
description: Hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinleri ve dosya ve dizin erişim denetim listelerini (ACL) yönetmek için Java için Azure Depolama kitaplıklarını kullanın.
author: normesta
ms.service: storage
ms.date: 03/20/2020
ms.author: normesta
ms.topic: conceptual
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 45870dd7d3035b6b49340fd6e8016794088e775a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061569"
---
# <a name="use-java-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Java'yı kullanın

Bu makalede, hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinler, dosyalar ve izinler oluşturmak ve yönetmek için Java'yı nasıl kullanacağınızı gösterir. 

[Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) | [Örnekleri](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake) | [API referans](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.0.1/index.html) | [Gen1 Gen2 haritalama](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md) | [Geribildirim ver](https://github.com/Azure/azure-sdk-for-java/issues)

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkinleştirilmiş bir depolama hesabı. Oluşturmak için [bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Başlamak için [bu sayfayı](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) açın ve Java kitaplığın en son sürümünü bulun. Ardından, metin düzenleyicinizdeki *pom.xml* dosyasını açın. Bu sürüme başvuran bir bağımlılık öğesi ekleyin.

Azure Etkin Dizini (AD) kullanarak istemci uygulamanızın kimliğini doğrulamayı planlıyorsanız, Azure Gizli İstemci Kitaplığı'na bir bağımlılık ekleyin. Bkz. [Projenize Gizli İstemci Kitaplığı paketini ekleme.](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#adding-the-package-to-your-project)

Ardından, bu içeri alma deyimlerini kod dosyanıza ekleyin.

```java
import com.azure.core.credential.TokenCredential;
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

## <a name="connect-to-the-account"></a>Hesaba bağlanın 

Bu makaledeki parçacıkları kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı nı kullanarak bağlanma

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir **DataLakeServiceClient** örneği oluşturur.

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

### <a name="connect-by-using-azure-active-directory-azure-ad"></a>Azure Etkin Dizin (Azure AD) kullanarak bağlanma

Uygulamanızın kimliğini Azure AD ile doğrulamak [için Java için Azure kimlik istemcisi kitaplığını](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) kullanabilirsiniz.

Bu örnek, bir istemci kimliği, istemci sırrı ve kiracı kimliği kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için, [istemci uygulamasından gelen istekleri yetkilendirmek için Azure AD'den bir belirteç edinin'e](../common/storage-auth-aad-app.md)bakın.

```java
static public DataLakeServiceClient GetDataLakeServiceClient
    (String accountName, String clientId, String ClientSecret, String tenantID){

    String endpoint = "https://" + accountName + ".dfs.core.windows.net";
        
    ClientSecretCredential clientSecretCredential = new ClientSecretCredentialBuilder()
    .clientId(clientId)
    .clientSecret(ClientSecret)
    .tenantId(tenantID)
    .build();
           
    DataLakeServiceClientBuilder builder = new DataLakeServiceClientBuilder();
    return builder.credential(clientSecretCredential).endpoint(endpoint).buildClient();
 } 
```

> [!NOTE]
> Daha fazla örnek için, Java belgeleri [için Azure kimlik istemcisi kitaplığına](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) bakın.


## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi, dosyalarınız için bir kapsayıcı görevi görür. **DataLakeServiceClient.createFileSystem** yöntemini arayarak bir tane oluşturabilirsiniz.

Bu örnek, .. `my-file-system` 

```java
static public DataLakeFileSystemClient CreateFileSystem
(DataLakeServiceClient serviceClient){

    return serviceClient.createFileSystem("my-file-system");
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

**DataLakeFileSystemClient.createDirectory** yöntemini arayarak bir dizin başvurusu oluşturun.

Bu örnek, dosya `my-directory` sistemine bir dizin ekler ve sonra adlı `my-subdirectory`bir alt dizin ekler. 

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

**DataLakeDirectoryClient.rename** yöntemini arayarak bir dizini yeniden adlandırın veya taşıyın. İstenilen dizin bir parametre yolunu geçirin. 

Bu örnek, bir alt dizinin adını `my-subdirectory-renamed`yeniden adlandırır.

```java
static public DataLakeDirectoryClient
    RenameDirectory(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory");

    return directoryClient.rename(
        fileSystemClient.getFileSystemName(),"my-subdirectory-renamed");
}
```

Bu örnek, adlı `my-subdirectory-renamed` `my-directory-2`bir dizinin alt dizinine adlı bir dizin taşır. 

```java
static public DataLakeDirectoryClient MoveDirectory
(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory-renamed");

    return directoryClient.rename(
        fileSystemClient.getFileSystemName(),"my-directory-2/my-subdirectory-renamed");                
}
```

## <a name="delete-a-directory"></a>Bir dizini silme

**DataLakeDirectoryClient.deleteWithResponse** yöntemini arayarak bir dizini silin.

Bu örnek, adlı `my-directory`bir dizini siler.   

```java
static public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient){
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    directoryClient.deleteWithResponse(true, null, null, null);
}
```

## <a name="manage-a-directory-acl"></a>Bir dizin ACL yönetme

Bu örnek alır ve sonra adlı `my-directory`bir dizinin ACL ayarlar. Bu örnek, sahibi kullanıcıya okuma, yazma ve izinleri yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine okuma erişimi sağlar.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

```java
static public void ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        directoryClient.getAccessControl();

    List<PathAccessControlEntry> pathPermissions = directoryAccessControl.getAccessControlList();
       
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));
             
    RolePermissions groupPermission = new RolePermissions();
    groupPermission.setExecutePermission(true).setReadPermission(true);
  
    RolePermissions ownerPermission = new RolePermissions();
    ownerPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(true);
  
    RolePermissions otherPermission = new RolePermissions();
    otherPermission.setReadPermission(true);
  
    PathPermissions permissions = new PathPermissions();
  
    permissions.setGroup(groupPermission);
    permissions.setOwner(ownerPermission);
    permissions.setOther(otherPermission);

    directoryClient.setPermissions(permissions, null, null);

    pathPermissions = directoryClient.getAccessControl().getAccessControlList();
     
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}

```

## <a name="upload-a-file-to-a-directory"></a>Dosyayı dizine yükleme

İlk olarak, **DataLakeFileClient** sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. **DataLakeFileClient.append** yöntemini arayarak dosya yükleyin. **DataLakeFileClient.FlushAsync** yöntemini arayarak yüklemeyi tamamladığından emin olun.

Bu örnek, bir metin dosyasını `my-directory`adlı bir dizine yükler.

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

> [!TIP]
> Dosya boyutunuz büyükse, kodunuz **DataLakeFileClient.append** yöntemine birden çok arama yapmak zorunda kalacaktır. Bunun yerine **DataLakeFileClient.uploadFromFile** yöntemini kullanmayı düşünün. Bu şekilde, tüm dosyayı tek bir aramada yükleyebilirsiniz. 
>
> Örnek için sonraki bölüme bakın.

## <a name="upload-a-large-file-to-a-directory"></a>Büyük bir dosyayı dizine yükleme

**DataLakeFileClient.uploadFromFile** yöntemini kullanarak **DataLakeFileClient.append** yöntemine birden fazla arama yapmak zorunda kalmadan büyük dosyaları yükleyin.

```java
static public void UploadFileBulk(DataLakeFileSystemClient fileSystemClient) 
    throws FileNotFoundException{
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = directoryClient.getFileClient("uploaded-file.txt");

    fileClient.uploadFromFile("C:\\mytestfile.txt");

    }

```


## <a name="manage-a-file-acl"></a>Dosyayı ACL yönetme

Bu örnek alır ve sonra adlı `upload-file.txt`bir dosyanın ACL ayarlar. Bu örnek, sahibi kullanıcıya okuma, yazma ve izinleri yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine okuma erişimi sağlar.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

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
           
    RolePermissions groupPermission = new RolePermissions();
    groupPermission.setExecutePermission(true).setReadPermission(true);

    RolePermissions ownerPermission = new RolePermissions();
    ownerPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(true);

    RolePermissions otherPermission = new RolePermissions();
    otherPermission.setReadPermission(true);

    PathPermissions permissions = new PathPermissions();

    permissions.setGroup(groupPermission);
    permissions.setOwner(ownerPermission);
    permissions.setOther(otherPermission);

    fileClient.setPermissions(permissions, null, null);

    pathPermissions = fileClient.getAccessControl().getAccessControlList();
   
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}
```

## <a name="download-from-a-directory"></a>Bir dizinden indirin

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir **DataLakeFileClient** örneği oluşturun. Dosyayı okumak için **DataLakeFileClient.read** yöntemini kullanın. Akıştan bir dosyaya bayt kaydetmek için herhangi bir .NET dosya işleme API'sini kullanın. 

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
      
}

```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Bu örnek, adlı `my-directory`bir dizinde bulunan her dosyanın adlarını yazdırır.

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

* [API başvuru belgeleri](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.0.1/index.html)
* [Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake)
* [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)
* [Gen1 - Gen2 haritalama](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde](https://github.com/Azure/azure-sdk-for-java/issues)