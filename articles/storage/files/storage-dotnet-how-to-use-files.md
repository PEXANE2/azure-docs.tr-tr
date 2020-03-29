---
title: .NET ile Azure Dosyaları için geliştirme | Microsoft Docs
description: Dosya verilerini depolamak için Azure Dosyaları'nı kullanan .NET uygulamaları ve hizmetlerini geliştirmeyi öğrenin.
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 10/7/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 4d8be13a75e276d5be6ec71141a13f95601869f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301446"
---
# <a name="develop-for-azure-files-with-net"></a>.NET ile Azure Dosyaları için geliştirme

[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

Bu öğretici, dosya verilerini depolamak için [Azure Dosyaları](storage-files-introduction.md) kullanan uygulamalar geliştirmek üzere .NET kullanmayla ilgili temel bilgileri gösterir. Bu öğretici, .NET ve Azure Dosyaları ile temel işlemleri yapmak için basit bir konsol uygulaması oluşturur:

* Bir dosyanın içeriğini alın.
* Dosya paylaşımı için en büyük boyutu veya *kotayı* ayarlayın.
* Paylaşımda tanımlanan depolanmış erişim ilkesini kullanan bir dosya için paylaşılan erişim imzası (SAS anahtarı) oluşturun.
* Bir dosyayı aynı depolama hesabındaki başka bir dosyaya kopyalama.
* Bir dosyayı aynı depolama hesabındaki bir bloba kopyalama.
* Sorun giderme için Azure Depolama Ölçümleri'ni kullanın.

Azure Dosyaları hakkında daha fazla bilgi edinmek için Azure [Dosyaları nedir?](storage-files-introduction.md)

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="understanding-the-net-apis"></a>.NET API'lerini anlama

Azure Dosyaları istemci uygulamalarına iki geniş yaklaşım sağlar: Sunucu İleti Bloğu (SMB) ve REST. .NET içinde, `System.IO` `WindowsAzure.Storage` ve API'ler bu yaklaşımları soyutlar.

API | Kullanılması gereken durumlar | Notlar
----|-------------|------
[System.IO](https://docs.microsoft.com/dotnet/api/system.io) | Uygulamanız: <ul><li>Kobİ kullanarak dosyaları okuma/yazma gerekiyor</li><li>Azure Dosyaları hesabınıza 445 bağlantı noktası üzerinden erişimi olan bir cihazda çalışıyor</li><li>Dosya paylaşımının yönetim ayarlarından herhangi birini yönetmesi gerekmiyor</li></ul> | Azure Dosyaları ile Kobİ üzerinden uygulanan Dosya G/Ç, genellikle herhangi bir ağ dosyası paylaşımı veya yerel depolama aygıtıyla G/Ç ile aynıdır. .NET'te dosya G/Ç de dahil olmak üzere bir dizi özelliğe giriş yapmak için [Konsol Uygulaması](https://docs.microsoft.com/dotnet/csharp/tutorials/console-teleprompter) öğreticisine bakın.
[Microsoft.Azure.Storage.File](/dotnet/api/overview/azure/storage?view=azure-dotnet#version-11x) | Uygulamanız: <ul><li>Güvenlik duvarı veya ISS kısıtlamaları nedeniyle 445 bağlantı noktasında SMB kullanarak Azure Dosyalarına erişemiyorum</li><li>Bir dosya paylaşımının kotasını ayarlama veya paylaşılan bir erişim imzası oluşturma gibi yönetim işlevleri gerektiriyor</li></ul> | Bu makalede, SMB `Microsoft.Azure.Storage.File` yerine REST kullanarak dosya G/Ç'nin kullanımı ve dosya paylaşımının yönetimi gösteriş göstermektedir.

## <a name="create-the-console-application-and-obtain-the-assembly"></a>Konsol uygulaması oluşturma ve derleme alma

Visual Studio'da yeni bir Windows konsol uygulaması oluşturun. Aşağıdaki adımlar Visual Studio 2019'da nasıl bir konsol uygulaması oluşturabileceğinizi gösterir. Adımlar Visual Studio’nun diğer sürümlerinde de benzerdir.

1. Visual Studio'yı başlatın ve **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da,** C#için Konsol **Uygulaması'nı (.NET Framework)** seçin ve ardından **İleri'yi**seçin.
1. **Yeni projenizi Yapılandır'da,** uygulama için bir ad girin ve **Oluştur'u**seçin.

Bu öğreticideki tüm kod örneklerini `Main()` konsol uygulamanızın `Program.cs` dosyasının yöntemine ekleyebilirsiniz.

Azure Depolama istemci kitaplığını herhangi bir .NET uygulamasında kullanabilirsiniz. Bu türler arasında Azure bulut hizmeti veya web uygulaması ile masaüstü ve mobil uygulamalar yer almaktadır. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

## <a name="use-nuget-to-install-the-required-packages"></a>Gereken paketleri yüklemek için NuGet kullanma

Bu öğreticiyi tamamlamak için projenizdeki bu paketlere bakın:

* [.NET için Microsoft Azure Depolama ortak kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/)
  
  Bu paket, depolama hesabınızdaki ortak kaynaklara programlı erişim sağlar.
* [.NET için Microsoft Azure Depolama Blob kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/)

  Bu paket, depolama hesabınızdaki blob kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure Depolama Dosyası kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.File/)

  Bu paket, depolama hesabınızdaki dosya kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure Configuration Manager kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/)

  Bu paket, uygulamanız nerede çalışırsa çalışırsa çalışırsa çalışırsa, bir yapılandırma dosyasında bağlantı dizesini ayrıştama için bir sınıf sağlar.

Her iki paketi de almak için NuGet kullanabilirsiniz. Şu adımları uygulayın:

1. **Solution Explorer'da**projenizi sağ tıklatın ve **NuGet Paketlerini Yönet'i**seçin.
1. **NuGet Paket Yöneticisi'nde** **Gözat'ı**seçin. Ardından **Microsoft.Azure.Storage.Blob'u**arayın ve seçin ve ardından **Yükle'yi**seçin.

   Bu adım, paketi ve bağımlılıklarını yükler.
1. Bu paketleri arayın ve yükleyin:

   * **Microsoft.Azure.Storage.Common**
   * **Microsoft.Azure.Storage.File**
   * **Microsoft.Azure.ConfigurationManager**

## <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a>Depolama hesabı kimlik bilgilerinizi App.config dosyasına kaydedin

Ardından, kimlik bilgilerinizi projenizin `App.config` dosyasına kaydedin. **Çözüm Gezgini'nde,** aşağıdaki örneğe benzer şekilde dosyayı çift tıklatın `App.config` ve edin. Depolama `myaccount` hesabı adınız `mykey` ve depolama hesabı anahtarınızla değiştirin.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> Azure depolama öykünücüsünün en son sürümü Azure Dosyaları'nı desteklemez. Bağlantı dizeniz, Azure Dosyaları ile çalışmak için buluttaki bir Azure Depolama hesabını hedeflemelidir.

## <a name="add-using-directives"></a>Using yönergeleri ekleme

**Çözüm Gezgini'nde,** dosyayı `Program.cs` açın ve dosyanın üst üstüne yönergeleri kullanarak aşağıdaki yönergeleri ekleyin.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.Azure.Storage; // Namespace for Storage Client Library
using Microsoft.Azure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.Azure.Storage.File; // Namespace for Azure Files
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-the-file-share-programmatically"></a>Dosya paylaşımına programlamayla erişme

Ardından, bağlantı dizesini `Main()` almak için yukarıda gösterilen koddan sonra yönteme aşağıdaki içeriği ekleyin. Bu kod, daha önce oluşturduğumuz dosyaya bir başvuru alır ve içeriğini çıkarır.

```csharp
// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Çıkışı görmek konsol uygulamasını çalıştırın.

## <a name="set-the-maximum-size-for-a-file-share"></a>Dosya paylaşımı için boyut üst sınırını ayarlama

Azure Depolama İstemci Kitaplığı sürümü 5.x ile başlayarak, bir dosya paylaşımı için kotayı (maksimum boyut) ayarlayabilirsiniz. Paylaşımda halihazırda ne kadar verinin depolandığını da kontrol edebilirsiniz.

Bir paylaşım için kota ayarlanması, paylaşımda depolanan dosyaların toplam boyutunu sınırlar. Paylaşımdaki dosyaların toplam boyutu, paylaşımda ayarlanan kotayı aşarsa, istemciler varolan dosyaların boyutunu artıramaz. İstemciler, bu dosyalar boş olmadığı sürece yeni dosyalar oluşturamaz.

Aşağıdaki örnekte, paylaşımdaki mevcut kullanımını nasıl kontrol edileceği veya paylaşım için nasıl kota ayarlanacağı gösterilmiştir.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.Azure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Dosya veya dosya paylaşımı için paylaşılan erişim imzası oluşturma

Azure Storage İstemci Kitaplığı’nın 5.x sürümünden başlayarak, bir dosya paylaşımı veya yalnızca dosya için paylaşılan erişim imzası (SAS) oluşturabilirsiniz. Paylaşılan erişim imzalarını yönetmek için dosya paylaşımında depolanmış bir erişim ilkesi de oluşturabilirsiniz. SAS'ı tehlikeye girerse iptal etmenizi sağlar, çünkü depolanmış bir erişim ilkesi oluşturmanızı öneririz.

Aşağıdaki örnek, bir paylaşım üzerinde depolanmış bir erişim ilkesi oluşturur. Örnek, paylaşımdaki bir dosyadaki Bir SAS için kısıtlamaları sağlamak için bu ilkeyi kullanır.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new stored access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the stored access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authorized via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

Paylaşılan erişim imzaları oluşturma ve kullanma hakkında daha fazla bilgi için [bkz.](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#how-a-shared-access-signature-works)

## <a name="copy-files"></a>Dosyaları kopyalama

Azure Storage İstemci Kitaplığı’nın 5.x sürümünden başlayarak, bir dosyayı başka bir dosyaya, bir dosyayı başka bir bloba veya bir blobu bir dosyaya kopyalayabilirsiniz. Sonraki bölümlerde, bu kopyalama işlemlerinin programlı olarak nasıl yapılacağını gösteriyoruz.

Ayrıca, bir dosyayı diğerine kopyalamak veya bir dosyaya veya başka bir şekilde bir blob kopyalamak için AzCopy'yi de kullanabilirsiniz. Bkz. [AzCopy ile başlayın.](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

> [!NOTE]
> Bir blobu dosyaya veya bir dosyayı bloba kopyalamak için aynı depolama hesabında kopyalama yapıyor olsanız da kaynak nesnesi erişimini yetkilendirmek amacıyla bir paylaşılan erişim imzası (SAS) kullanmanız gerekir.
>

### <a name="copy-a-file-to-another-file"></a>Dosyayı başka bir dosyaya kopyalama

Aşağıdaki örnekte, bir dosya aynı paylaşımdaki başka bir dosyaya kopyalanır. Bu kopyalama işlemi aynı depolama hesabındaki dosyalar arasında kopyalandığından, kopyayı yapmak için Paylaşılan Anahtar kimlik doğrulamasını kullanabilirsiniz.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

### <a name="copy-a-file-to-a-blob"></a>Dosyayı bir bloba kopyalama

Aşağıdaki örnekte, bir dosya oluşturulur ve aynı depolama hesabındaki bir bloba kopyalanır. Örnekte, kaynak dosya için hizmetin kopyalama sırasında kaynak dosyaya erişimi yetkilendirmek üzere kullandığı bir SAS oluşturulur.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure Files.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authorize access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

Aynı şekilde, bir blobu bir dosyaya kopyalayabilirsiniz. Kaynak dosya bir blob ise, kopyalama sırasında bu bloba erişimi yetkilendirmesi için bir SAS oluşturun.

## <a name="share-snapshots"></a>Anlık görüntüleri paylaşma

Azure Depolama İstemci Kitaplığı'nın 8.5 sürümünden başlayarak, paylaşım anlık görüntüsü oluşturabilirsiniz. Ayrıca paylaşım anlık görüntülerini listeleyebilir, onlara göz atabilir ve paylaşım anlık görüntülerini silebilirsiniz. Paylaşım anlık görüntüleri salt okunur olduğundan paylaşım anlık görüntüleri üzerinde yazma işlemi gerçekleştirilemez.

### <a name="create-share-snapshots"></a>Paylaşım anlık görüntüsü oluşturma

Aşağıdaki örnekte dosya paylaşım anlık görüntüsü oluşturulmaktadır.

```csharp
storageAccount = CloudStorageAccount.Parse(ConnectionString); 
fClient = storageAccount.CreateCloudFileClient(); 
string baseShareName = "myazurefileshare"; 
CloudFileShare myShare = fClient.GetShareReference(baseShareName); 
var snapshotShare = myShare.Snapshot();

```

### <a name="list-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme

Aşağıdaki örnekte bir paylaşım üzerindeki paylaşım anlık görüntüleri listelenmektedir.

```csharp
var shares = fClient.ListShares(baseShareName, ShareListingDetails.All);
```

### <a name="browse-files-and-directories-within-share-snapshots"></a>Paylaşım anlık görüntüleri içindeki dosyalara ve dizinlere göz atma

Aşağıdaki örnekte paylaşım anlık görüntüleri içindeki dosyalara ve dizinlere göz atılmaktadır.

```csharp
CloudFileShare mySnapshot = fClient.GetShareReference(baseShareName, snapshotTime); 
var rootDirectory = mySnapshot.GetRootDirectoryReference(); 
var items = rootDirectory.ListFilesAndDirectories();
```

### <a name="list-shares-and-share-snapshots-and-restore-file-shares-or-files-from-share-snapshots"></a>Paylaşımları ve paylaşım anlık görüntülerini listeleme ve dosya paylaşımlarını veya paylaşım anlık görüntülerindeki dosyaları geri yükleme

Bir dosya paylaşımının anlık görüntüsünü alarak daha sonra içindeki dosyaları veya dosya paylaşımının tamamını kurtarabilirsiniz.

Bir dosya paylaşımının paylaşım anlık görüntülerini sorgulayarak bir dosya paylaşım anlık görüntüsündeki dosyayı geri yükleyebilirsiniz. Daha sonra belirli bir paylaşım anlık görüntüsüne ait bir dosyayı alabilirsiniz. Bu sürümü doğrudan okumak ve karşılaştırmak veya geri yüklemek için kullanın.

```csharp
CloudFileShare liveShare = fClient.GetShareReference(baseShareName);
var rootDirOfliveShare = liveShare.GetRootDirectoryReference();
var dirInliveShare = rootDirOfliveShare.GetDirectoryReference(dirName);
var fileInliveShare = dirInliveShare.GetFileReference(fileName);

CloudFileShare snapshot = fClient.GetShareReference(baseShareName, snapshotTime);
var rootDirOfSnapshot = snapshot.GetRootDirectoryReference();
var dirInSnapshot = rootDirOfSnapshot.GetDirectoryReference(dirName);
var fileInSnapshot = dir1InSnapshot.GetFileReference(fileName);

string sasContainerToken = string.Empty;
SharedAccessFilePolicy sasConstraints = new SharedAccessFilePolicy();
sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
sasConstraints.Permissions = SharedAccessFilePermissions.Read;

//Generate the shared access signature on the container, setting the constraints directly on the signature.
sasContainerToken = fileInSnapshot.GetSharedAccessSignature(sasConstraints);

string sourceUri = (fileInSnapshot.Uri.ToString() + sasContainerToken + "&" + fileInSnapshot.SnapshotTime.ToString()); ;
fileInliveShare.StartCopyAsync(new Uri(sourceUri));
```

### <a name="delete-share-snapshots"></a>Paylaşım anlık görüntülerini silme

Aşağıdaki örnekte dosya paylaşım anlık görüntüsü silinmektedir.

```csharp
CloudFileShare mySnapshot = fClient.GetShareReference(baseShareName, snapshotTime); mySnapshot.Delete(null, null, null);
```

## <a name="troubleshoot-azure-files-by-using-metrics"></a>Ölçümleri kullanarak Azure Dosyaları sorun giderme<a name="troubleshooting-azure-files-using-metrics"></a>

Artık Azure Depolama Analizi, Azure Dosyaları için ölçümleri destekliyor. Ölçüm verilerini kullanarak istekleri ve tanılama sorunlarını izleyebilirsiniz.

Azure dosyaları için ölçümleri [Azure portalından](https://portal.azure.com)etkinleştirebilirsiniz. Ayrıca, REST API ile Ayarla Dosya Hizmeti Özellikleri işlemini veya Depolama İstemci Kitaplığı'ndaki analoglarından birini çağırarak ölçümleri programlanabilir şekilde etkinleştirebilirsiniz.

Aşağıdaki kodda, Azure Dosyaları için ölçümleri etkinleştirmek üzere .NET için Depolama İstemcisi Kitaplığı'nı nasıl kullanacağınız gösterilmiştir.

İlk olarak, `using` dosyanıza `Program.cs` yukarıda eklediğiniz yönergelerin yanı sıra aşağıdaki yönergeleri ekleyin:

```csharp
using Microsoft.Azure.Storage.File.Protocol;
using Microsoft.Azure.Storage.Shared.Protocol;
```

Azure Blobs, Azure Tabloları ve Azure `ServiceProperties` Kuyrukları `Microsoft.Azure.Storage.Shared.Protocol` ad alanında paylaşılan türü kullansa `FileServiceProperties` da, `Microsoft.Azure.Storage.File.Protocol` Azure Dosyaları ad alanında kendi türünü kullanır. Ancak, aşağıdaki kodun derlemesi için kodunuzun her iki ad alanlarına da başvurmanız gerekir.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.Azure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read the metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

Herhangi bir sorunla karşılaşırsanız, [Windows'daki Azure Dosyaları sorunlarını gidermeye](storage-troubleshoot-windows-file-connection-problems.md)başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosyaları hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

### <a name="conceptual-articles-and-videos"></a>Kavramsal makaleler ve videolar

* [Azure Dosyaları: Windows ve Linux için uyumlu bulut SMB dosya sistemi](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Azure Dosyaları'nı Linux ile kullanma](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>File Storage için araç desteği

* [AzCopy’yi kullanmaya başlama](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Windows’ta Azure Dosyalar sorunlarını giderme](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Başvuru

* [.NET için Azure Depolama API'leri](/dotnet/api/overview/azure/storage)
* [Dosya Hizmeti REST API'si](/rest/api/storageservices/File-Service-REST-API)

### <a name="blog-posts"></a>Blog yazıları

* [Azure Dosya Depolama, artık genel olarak kullanılabilir](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Azure Dosya Depolama İçinde](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Microsoft Azure Dosyaları Hizmeti Tanıtımı](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Microsoft Azure Dosyaları ile kalıcı bağlantılar](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
