---
title: .NET - Azure Depolama'da bir blob anlık görüntüsü oluşturma ve yönetme
description: Belirli bir anda blob verilerini yedeklemek için bir blob'un salt okunur anlık görüntüsünü nasıl oluşturup oluşturabilirsiniz öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/06/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 9bf5eea55002814f461d375b3db43a37fe4f7aa9
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474088"
---
# <a name="create-and-manage-a-blob-snapshot-in-net"></a>.NET'te bir blob anlık görüntüsü oluşturma ve yönetme

Anlık görüntü, bir noktada çekilen bir lekenin salt okunur halidir. Anlık görüntüler lekeleri yedeklemek için yararlıdır. Bu makalede, [.NET için Azure Depolama istemci kitaplığını](/dotnet/api/overview/azure/storage?view=azure-dotnet)kullanarak blob anlık görüntüleri nasıl oluşturulup yönetilir gösterilmektedir.

## <a name="about-blob-snapshots"></a>Blob anlık görüntüleri hakkında

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

Bir blob anlık görüntüsü, anlık görüntünün çekildiği zamanı belirtmek için URI blob'una eklenen bir **DateTime** değeri olması dışında, taban blob'uyla aynıdır. Örneğin, bir sayfa blob `http://storagesample.core.blob.windows.net/mydrives/myvhd`URI ise, anlık `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`URI benzer.

> [!NOTE]
> Tüm anlık görüntüler temel blob'un URI'sini paylaşır. Temel blob ve anlık görüntü arasındaki tek ayrım eklenen **DateTime** değeridir.
>

Bir blob anlık görüntü herhangi bir sayı olabilir. Anlık görüntüler açıkça silinene kadar devam eder, bu da anlık görüntünün temel lekeden daha uzun yaşayamaması anlamına gelir. Geçerli anlık görüntülerinizi izlemek için temel lekeyle ilişkili anlık görüntüleri güncelleyebilirsiniz.

Bir blob anlık görüntüsü oluşturduğunuzda, blob sistem özellikleri aynı değerlerle anlık görüntü kopyalanır. Oluşturduğunuzda anlık görüntü için ayrı meta veriler belirtmediğiniz sürece, temel blob'un meta verileri de anlık görüntüye kopyalanır. Anlık görüntü oluşturduktan sonra okuyabilir, kopyalayabilir veya silebilirsiniz, ancak değiştiremezsiniz.

Temel blob ile ilişkili tüm kiralama anlık görüntü etkilemez. Anlık görüntü de kira elde edemezsiniz.

VHD dosyası, vm diskin geçerli bilgilerini ve durumunu depolamak için kullanılır. Bir diski VM'nin içinden ayırabilir veya VM'yi kapatabilir ve ardından VHD dosyasının anlık görüntüsünü alabilirsiniz. Bu anlık görüntü dosyasını daha sonra, vhd dosyasını bu noktada almak ve VM'yi yeniden oluşturmak için kullanabilirsiniz.

## <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Bir blok blob anlık görüntü oluşturmak için aşağıdaki yöntemlerden birini kullanın:

- [Anlık Görüntü Oluşturma](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshot)
- [OluşturmaSnapshotAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshotasync)

Aşağıdaki kod örneği, anlık görüntünün nasıl oluşturulabildiğini gösterir. Bu örnek, oluşturulduğunda anlık görüntü için ek meta verileri belirtir.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="delete-snapshots"></a>Anlık görüntüleri silme

Bir blob silmek için, öncelikle bu blob herhangi bir anlık silmeniz gerekir. Anlık bir fotoğrafı tek tek silebilir veya kaynak blob silindiğinde tüm anlık görüntünün silineceğini belirtebilirsiniz. Anlık görüntüleri olan bir blob silmeye çalışırsanız, bir hata sonuçlanır.

Blob anlık görüntülerini silmek için aşağıdaki blob silme yöntemlerinden birini kullanın ve [DeleteSnapshotsOption](/dotnet/api/microsoft.azure.storage.blob.deletesnapshotsoption) enum'u ekleyin.

- [Sil](/dotnet/api/microsoft.azure.storage.blob.cloudblob.delete)
- [Deleteasync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexistsasync)

Aşağıdaki kod örneği, [CloudBlockBlob][dotnet_CloudBlockBlob]türündeki bir nesnenin `blockBlob` bulunduğu .NET'teki bir blob ve anlık görüntüleri nasıl silinir gösterir:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Mutlak URI'yi anlık görüntüye döndürme

Aşağıdaki kod örneği bir anlık görüntü oluşturur ve birincil konum için mutlak URI yazar.

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Anlık görüntülerinin nasıl ücret tahakkuk ettirmelerini anlama

Bir blob'un salt okunur kopyası olan anlık görüntü oluşturmak, hesabınıza ek veri depolama ücretleri neden olabilir. Uygulamanızı tasarlarken, maliyetleri en aza indirmek için bu ücretlerin nasıl tahakkuk edebileceğini bilmeniz önemlidir.

### <a name="important-billing-considerations"></a>Önemli faturalandırma konuları

Aşağıdaki liste, anlık görüntü oluştururken göz önünde bulundurulması gereken önemli noktaları içerir.

- Depolama hesabınız, ister blob'da ister anlık görüntüde olsun, benzersiz bloklar veya sayfalar için ücret ekleri oluşur. Hesabınızda bulunan blob'u güncelleştirene kadar bir lekeyle ilişkili anlık görüntüler için ek ücret almaz. Taban lekesini güncelledikten sonra anlık görüntülerinden uzaklaşır. Bu durumda, her blob veya anlık görüntü benzersiz bloklar veya sayfalar için ücretlendirilir.
- Bir blok blob içinde bir blok değiştirdiğinizde, bu blok daha sonra benzersiz bir blok olarak ücretlendirilir. Bu, blok, anlık görüntüdekiyle aynı blok kimliğine ve aynı veriye sahip olsa bile geçerlidir. Blok yeniden işlendikten sonra, herhangi bir anlık görüntüdeki muadilinden uzaklaşır ve verileri için ücretlendirilirsiniz. Aynı durum, aynı verilerle güncelleştirilen bir sayfa blob'undaki bir sayfa için de geçerlidir.
- [UploadFromFile,][dotnet_UploadFromFile] [UploadText,][dotnet_UploadText] [UploadFromStream][dotnet_UploadFromStream]veya [UploadFromByteArray][dotnet_UploadFromByteArray] yöntemini arayarak bir blok blob'u değiştirmek, blob'daki tüm blokların yerini alır. Bu lekeyle ilişkili bir anlık görüntün varsa, temel blob ve anlık görüntüdeki tüm bloklar şimdi ayrılır ve her iki blobdaki tüm bloklar için ücretlendirilirsiniz. Bu, temel blob ve anlık görüntü deki veriler aynı kalsa bile geçerlidir.
- Azure Blob hizmeti, iki bloğun aynı verileri içerip içermediğini belirlemek için bir araç içermez. Yüklenen ve işlenen her blok, aynı veriye ve aynı blok kimliğine sahip olsa bile benzersiz olarak kabul edilir. Benzersiz bloklar için ücretler tahakkuk ettiğiiçin, anlık görüntü yesahip bir blob'un güncelleştirilmesinin ek benzersiz bloklar ve ek ücretlerle sonuçlandığı göz önünde bulundurulması önemlidir.

### <a name="minimize-cost-with-snapshot-management"></a>Anlık görüntü yönetimi yle maliyeti en aza indirin

Ek ücretlendirmelerden kaçınmak için anlık görüntülerinizi dikkatle yönetmenizi öneririz. Anlık görüntülerinizin depolanmasından kaynaklanan maliyetleri en aza indirmeye yardımcı olmak için aşağıdaki en iyi uygulamaları takip edebilirsiniz:

- Uygulama tasarımınız anlık görüntüleri korumanızı gerektirmediği sürece, aynı verilerle güncelleştirseniz bile, blob'u her güncellediğinizde blob ile ilişkili anlık görüntüleri silin ve yeniden oluşturun. Blob'un anlık görüntülerini silerek ve yeniden oluşturarak, blob ve anlık görüntünün birbirinden sapmadığından emin olabilirsiniz.
- Bir blob için anlık görüntüler koruyorsanız, blob'u güncellemek için [UploadFromFile,][dotnet_UploadFromFile] [UploadText,][dotnet_UploadText] [UploadFromStream][dotnet_UploadFromStream]veya [UploadFromByteArray'i][dotnet_UploadFromByteArray] aramaktan kaçının. Bu yöntemler, taban lekenizin ve anlık görüntülerinin önemli ölçüde farklılaşmasına neden olarak blob'daki tüm blokların yerini alır. Bunun yerine, [PutBlock][dotnet_PutBlock] ve [PutBlockList][dotnet_PutBlockList] yöntemlerini kullanarak mümkün olan en az sayıda bloğu güncelleştirin.

### <a name="snapshot-billing-scenarios"></a>Anlık faturalandırma senaryoları

Aşağıdaki senaryolar, bir blok blob ve anlık görüntüleri için ücretlerin nasıl tahakkuk edeceğini gösterir.

#### <a name="scenario-1"></a>Senaryo 1

Senaryo 1'de, anlık görüntü alındıktan sonra temel blob güncelleştirilmemiştir, bu nedenle yalnızca benzersiz bloklar 1, 2 ve 3 için ücretler tahakkuk ettirilir.

![Azure Depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

#### <a name="scenario-2"></a>Senaryo 2

Senaryo 2'de, temel blob güncelleştirildi, ancak anlık görüntü güncellenmedi. Blok 3 güncelleştirildi ve aynı verileri ve aynı kimliği içermesine rağmen, anlık görüntüdeki blok 3 ile aynı değildir. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure Depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

#### <a name="scenario-3"></a>3. Senaryo

Senaryo 3'te, temel blob güncelleştirildi, ancak anlık görüntü güncellenmedi. Blok 3 temel blob blok 4 ile değiştirildi, ancak anlık görüntü hala blok 3 yansıtır. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure Depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

#### <a name="scenario-4"></a>4. Senaryo

Senaryo 4'te, temel blob tamamen güncelleştirildi ve orijinal blokların hiçbirini içermiş. Sonuç olarak, hesap sekiz benzersiz blok için ücretlendirilir. Bu yöntemler bir blob içeriğinin tümünün yerini aldığı için [UploadFromFile,][dotnet_UploadFromFile] [UploadText,][dotnet_UploadText] [UploadFromStream][dotnet_UploadFromStream]veya [UploadFromByteArray][dotnet_UploadFromByteArray]gibi bir güncelleştirme yöntemi kullanıyorsanız bu senaryo oluşabilir.

![Azure Depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Sonraki adımlar

- Sanal makine (VM) disk anlık görüntüleriyle çalışma hakkında daha fazla bilgiyi, [artımlı anlık görüntülerle Azure'un yönetilmeyen VM disklerini Yedekle'de](../../virtual-machines/windows/incremental-snapshots.md) bulabilirsiniz

- Blob depolama alanını kullanarak ek kod örnekleri için [Azure Kod Örnekleri'ne](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob)bakın. Örnek bir uygulama indirebilir ve çalıştırabilirsiniz veya GitHub'daki koda göz atabilirsiniz.

[dotnet_AccessCondition]: https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.accesscondition
[dotnet_CloudBlockBlob]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblockblob
[dotnet_CreateSnapshotAsync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.createsnapshotasync
[dotnet_HTTPStatusCode]: https://docs.microsoft.com/java/api/com.microsoft.store.partnercenter.network.httpstatuscode
[dotnet_PutBlockList]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblocklist
[dotnet_PutBlock]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblock
[dotnet_UploadFromByteArray]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblob.uploadfrombytearray
[dotnet_UploadFromFile]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblob.uploadfromfile
[dotnet_UploadFromStream]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadfromstream
[dotnet_UploadText]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadtext
