---
title: Java ile Azure Dosyaları için geliştirin | Microsoft Dokümanlar
description: Dosya verilerini depolamak için Azure Dosyaları kullanan Java uygulamalarını ve hizmetlerini nasıl geliştireceklerini öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 28a280ea7c3bf9ef84a1fff05da5090ed526fb12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73837469"
---
# <a name="develop-for-azure-files-with-java"></a>Java ile Azure Dosyaları için geliştirme
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>Bu öğretici hakkında
Bu öğretici, dosya verilerini depolamak için Azure Dosyaları kullanan uygulamalar veya hizmetler geliştirmek için Java kullanmanın temellerini gösterecektir. Bu eğitimde, bir konsol uygulaması oluşturacağız ve Java ve Azure Dosyaları ile temel işlemlerin nasıl gerçekleştirileceğini göstereceğiz:

* Azure dosya paylaşımlarını oluşturma ve silme
* Dizin oluşturma ve silme
* Azure dosya paylaşımındaki dosyaları ve dizinleri sayısalolarak kaydedin
* Dosyayükleme, indirme ve silme

> [!Note]  
> Azure Dosyalarına Kobİ üzerinden erişilebildiği için, standart Java G/Ç sınıflarını kullanarak Azure dosya paylaşımına erişen uygulamalar yazmak mümkündür. Bu makalede, Azure Dosyaları yla konuşmak için Azure Files REST [API'sini](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) kullanan Azure Depolama Java SDK'sını kullanan uygulamaların nasıl yazıldığı açıklanmaktadır.

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
Örnekleri oluşturmak için Java Geliştirme Kiti (JDK) ve [Java için Azure Depolama SDK'sına](https://github.com/Azure/azure-storage-java)ihtiyacınız olacaktır. Bir Azure depolama hesabı da oluşturmuş olmalısınız.

## <a name="set-up-your-application-to-use-azure-files"></a>Uygulamanızı Azure Dosyalarını kullanacak şekilde ayarlama
Azure depolama API'lerini kullanmak için, depolama hizmetine erişmek istediğiniz Java dosyasının en üstüne aşağıdaki ifadeyi ekleyin.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Azure depolama bağlantı dizesi ayarlama
Azure Dosyaları'nı kullanmak için Azure depolama hesabınıza bağlanmanız gerekir. İlk adım, depolama hesabınıza bağlanmak için kullanacağımız bir bağlantı dizesini yapılandırmak olacaktır. Bunu yapmak için statik bir değişken tanımlayalım.

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> your_storage_account_name ve your_storage_account_key depolama hesabınıziçin gerçek değerlerle değiştirin.
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a>Azure depolama hesabına bağlanma
Depolama hesabınıza bağlanmak için, **ayrışdırıcı** yöntemine bir bağlantı dizesi geçirerek **CloudStorageAccount** nesnesini kullanmanız gerekir.

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

**CloudStorageAccount.parse** bir InvalidKeyException atar, bu nedenle deneme/yakalama bloğuna koymanız gerekir.

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Azure Dosyaları'ndaki tüm dosyalar ve dizinler **Paylaşım**adı verilen bir kapsayıcıda bulunur. Depolama hesabınız, hesap kapasitenizin izin verdiği kadar hisseye sahip olabilir. Bir paylaşıma ve içeriğine erişim elde etmek için bir Azure Files istemcisi kullanmanız gerekir.

```java
// Create the Azure Files client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Azure Dosyaları istemcisini kullanarak, bir paylaşıma başvuru alabilirsiniz.

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

Paylaşımı gerçekten oluşturmak için CloudFileShare nesnesinin **createIfNotExists** yöntemini kullanın.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

Bu noktada, **hisse** **sampleshare**adlı bir paya başvuruda bulundu.

## <a name="delete-an-azure-file-share"></a>Azure dosya paylaşımını silme
Bir paylaşımı silme, CloudFileShare nesnesindeki **deleteIfExists** yöntemini arayarak yapılır. İşte bunu yapan örnek kod.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a>Dizin oluşturma
Ayrıca, dosyaları kök dizininde olması yerine alt dizinlerin içine koyarak depolama alanını da düzenleyebilirsiniz. Azure Dosyaları, hesabınızın izin verdiği kadar dizin oluşturmanıza olanak tanır. Aşağıdaki kod kök dizinin altında **sampledir** adlı bir alt dizini oluşturur.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a>Bir dizini silme
Bir dizin silmek basit bir görevdir, ancak hala dosya veya diğer dizinler içeren bir dizini silemeyeceğiniz unutulmamalıdır.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Azure dosya paylaşımındaki dosyaları ve dizinleri sayısalolarak kaydedin
Bir paylaşım içindeki dosyaların ve dizinlerin listesini almak, CloudFileDirectory başvurusunda **listFilesAndDirectories'i** arayarak kolayca yapılır. Yöntem, üzerinde eritebileceğiniz ListFileItem nesnelerinin listesini döndürür. Örnek olarak, aşağıdaki kod, kök dizinin içindeki dosyaları ve dizinleri listeler.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme
Bu bölümde, yerel depolama dan bir paylaşımın kök dizinine dosya yüklemeyi öğreneceksiniz.

Bir dosyayı yüklemenin ilk adımı, bulunduğu dizin için bir başvuru elde etmektir. Bunu, paylaşım nesnesinin **getRootDirectoryReference** yöntemini arayarak yaparsınız.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Artık paylaşımın kök dizinine bir başvurunuz olduğuna göre, aşağıdaki kodu kullanarak dosyaya bir dosya yükleyebilirsiniz.

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Dosya indirme
Azure Dosyaları'na karşı gerçekleştireceğiniz en sık işlemlerden biri dosyaları indirmektir. Aşağıdaki örnekte, kod SampleFile.txt indirir ve içeriğini görüntüler.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a>Dosyayı silme
Başka bir yaygın Azure Dosyaları işlemi dosya silme olduğunu. Aşağıdaki kod SampleFile.txt **adlı**bir dosyayı siler.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure depolama API'leri hakkında daha fazla bilgi edinmek istiyorsanız, bu bağlantıları izleyin.

* [Java geliştiricileri için Azure](/java/azure)/)
* [Java için Azure Depolama SDK'sı](https://github.com/azure/azure-storage-java)
* [Android için Azure Depolama SDK](https://github.com/azure/azure-storage-android)
* [Azure Depolama İstemcisi SDK Başvurusu](https://javadoc.io/doc/com.microsoft.azure/azure-core/0.8.0/index.html)
* [Azure Storage Hizmetleri REST API’si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blogu](https://blogs.msdn.com/b/windowsazurestorage/)
* [AzCopy Command-Line Utility ile veri aktarımı](../common/storage-use-azcopy.md)
* [Azure Dosyaları sorunlarını giderme - Windows](storage-troubleshoot-windows-file-connection-problems.md)
