---
title: Azure Storage 'da Blobun salt okunurdur bir anlık görüntü oluşturma | Microsoft Docs
description: Blob verilerini belirli bir anda yedeklemek için bir Blobun anlık görüntüsünü oluşturmayı öğrenin. Anlık görüntülerin nasıl faturalandırıldığını ve kapasite ücretlerini en aza indirmek için nasıl kullanılacağını anlayın.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 0da3373ba2c13bd6a00a92a6b38bead86fc9a5ea
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897023"
---
# <a name="create-a-blob-snapshot"></a>Blob anlık görüntüsü oluşturma

Anlık görüntü, bir Blobun zaman içinde alınmış bir salt okunurdur. Anlık görüntüler Blobları yedeklemek için faydalıdır. Anlık görüntü oluşturduktan sonra, bunu okuyabilir, kopyalayabilir veya silebilirsiniz, ancak değiştiremezsiniz.

Blob URI 'si, blob URI 'sinin, anlık görüntünün alındığı saati belirtmek için blob URI 'sine eklenmiş bir **Tarih saat** değeri olması dışında, kendi temel blobuna benzer. Örneğin, bir Sayfa Blobu URI 'si ise `http://storagesample.core.blob.windows.net/mydrives/myvhd`, anlık görüntü URI 'si öğesine `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`benzerdir.

> [!NOTE]
> Tüm anlık görüntüler, temel Blobun URI 'sini paylaşır. Temel blob ve anlık görüntü arasındaki tek ayrım, eklenen **Tarih saat** değeridir.
>

Blob herhangi bir sayıda anlık görüntüye sahip olabilir. Anlık görüntüler, açıkça silinene kadar kalır. Anlık görüntü, kendi temel blobunu kullanamaz. Geçerli anlık görüntülerinizi izlemek için temel bloba ilişkili anlık görüntüleri numaralandırabilirsiniz.

Bir Blobun anlık görüntüsünü oluşturduğunuzda, Blobun sistem özellikleri aynı değerlerle anlık görüntüye kopyalanır. Temel Blobun meta verileri, oluşturduğunuz sırada anlık görüntü için ayrı meta veriler belirtmediğiniz müddetçe anlık görüntüye de kopyalanır.

Temel blob ile ilişkili tüm kiralamalar anlık görüntüyü etkilemez. Anlık görüntü üzerinde kira elde edilemez.

Bir VHD dosyası, bir VM diskinin geçerli bilgilerini ve durumunu depolamak için kullanılır. VM 'nin içinden bir diski ayırabilir veya VM 'yi kapatabilir ve sonra VHD dosyasının anlık görüntüsünü alabilirsiniz. Bu anlık görüntü dosyasını daha sonra bu noktada VHD dosyasını almak ve VM 'yi yeniden oluşturmak için kullanabilirsiniz.

## <a name="create-a-snapshot"></a>Anlık görüntü oluşturma
Aşağıdaki kod örneğinde, [.net Için Azure Storage Istemci kitaplığı](/dotnet/api/overview/azure/storage/client)kullanılarak nasıl anlık görüntü oluşturacağınız gösterilmektedir. Bu örnek, bir anlık görüntü oluşturulduğunda diğer meta verileri belirtir.

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
        // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
        // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
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

## <a name="copy-snapshots"></a>Anlık görüntüleri Kopyala
Blob 'ları ve anlık görüntüleri içeren kopyalama işlemleri şu kurallara uyar:

* Bir anlık görüntüyü kendi temel blobundan kopyalayabilirsiniz. Bir anlık görüntüyü temel Blobun konumuna yükselterek bir blob 'un önceki bir sürümünü geri yükleyebilirsiniz. Anlık görüntü kalır, ancak anlık görüntünün yazılabilir kopyasıyla temel Blobun üzerine yazılır.
* Bir anlık görüntüyü hedef bloba farklı bir adla kopyalayabilirsiniz. Elde edilen hedef blobu anlık görüntü değil yazılabilir bir BLOB.
* Bir kaynak blobu kopyalandığında, kaynak Blobun herhangi bir anlık görüntüsü hedefe kopyalanmaz. Hedef Blobun bir kopya ile üzerine yazıldığında, özgün hedef blobu ile ilişkili tüm anlık görüntüler değişmeden kalır.
* Bir blok Blobun anlık görüntüsünü oluşturduğunuzda, Blobun yürütülen blok listesi de anlık görüntüye kopyalanır. Kaydedilmemiş bloklar kopyalanmaz.

## <a name="specify-an-access-condition"></a>Erişim koşulu belirtin
[CreateSnapshot tasync][dotnet_CreateSnapshotAsync]çağırdığınızda, anlık görüntünün yalnızca bir koşul karşılanırsa oluşturulması için bir erişim koşulu belirtebilirsiniz. Erişim koşulu belirtmek için [accesscondition][dotnet_AccessCondition] parametresini kullanın. Belirtilen koşul karşılanmazsa, anlık görüntü oluşturulmaz ve BLOB hizmeti [HttpStatusCode][dotnet_HTTPStatusCode]durum kodunu döndürür. PreconditionFailed.

## <a name="delete-snapshots"></a>Anlık görüntüleri Sil
Anlık görüntüler de silinmedikleri takdirde bir blobu anlık görüntülerle silemezsiniz. Bir anlık görüntüyü tek tek silebilir veya kaynak blobu silindiğinde tüm anlık görüntülerin silineceğini belirtebilirsiniz. Anlık görüntülere sahip olan bir blobu silmeye çalışırsanız hata oluşur.

Aşağıdaki kod örneği, .net 'teki bir Blobun ve anlık görüntülerinin nasıl silineceğini gösterir; burada `blockBlob` , [cloudblockblob][dotnet_CloudBlockBlob]türünde bir nesnedir:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Azure Premium Depolama ile anlık görüntüler
Premium Depolama ile anlık görüntüler kullanırken aşağıdaki kurallar geçerlidir:

* Bir Premium Depolama hesabında Sayfa Blobu başına en fazla anlık görüntü sayısı 100 ' dir. Bu sınır aşılırsa, anlık görüntü blobu işlemi 409 (`SnapshotCountExceeded`) hata kodunu döndürür.
* Her 10 dakikada bir Premium Depolama hesabındaki bir sayfa blobunun anlık görüntüsünü alabilirsiniz. Bu oran aşılırsa, anlık görüntü blobu işlemi 409 (`SnapshotOperationRateExceeded`) hata kodunu döndürür.
* Bir anlık görüntüyü okumak için, bir anlık görüntüyü hesaptaki başka bir sayfa blobuna kopyalamak üzere blobu Kopyala işlemini kullanabilirsiniz. Kopyalama işleminin hedef blobu mevcut bir anlık görüntü içermemelidir. Hedef Blobun anlık görüntüler varsa, blob kopyalama işlemi 409 (`SnapshotsPresent`) hata kodunu döndürür.

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Mutlak URI 'yi bir anlık görüntüye döndürün
Bu C# kod örneği bir anlık görüntü oluşturur ve birincil konum IÇIN mutlak URI 'yi yazar.

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

## <a name="understand-how-snapshots-accrue-charges"></a>Anlık görüntülerin ücretleri nasıl tahakkuk ettir öğrenin
Bir Blobun salt okunurdur kopyası olan bir anlık görüntü oluşturmak, hesabınıza ek veri depolama ücretleri oluşmasına neden olabilir. Uygulamanızı tasarlarken, maliyetleri en aza indirmek için bu ücretlerin nasıl tahakkuk edebileceğini bilmeniz önemlidir.

### <a name="important-billing-considerations"></a>Önemli faturalandırma konuları
Aşağıdaki liste, bir anlık görüntü oluştururken dikkate alınması gereken önemli noktaları içerir.

* Depolama hesabınız, blob 'ta mi yoksa anlık görüntüde olsun, benzersiz bloklar veya sayfalar için ücretler uygular. Hesabınız, temel aldıkları blobu güncelleştirene kadar bir blob ile ilişkili anlık görüntüler için ek ücret uygulamaz. Temel blobu güncelleştirdikten sonra, anlık görüntülerinden ayrılan olur. Bu durumda, her blob veya anlık görüntüde benzersiz bloklar veya sayfalar için ücretlendirilirsiniz.
* Blok Blobu içindeki bir bloğu değiştirdiğinizde, bu blok daha sonra benzersiz bir blok olarak ücretlendirilir. Bu, blok aynı blok KIMLIĞINE ve anlık görüntüdeki verilerle aynı verilere sahip olsa da geçerlidir. Blok yeniden gerçekleştirildikten sonra, herhangi bir anlık görüntüde onun karşılığından ayrılan bir işlem olur ve veriler için ücretlendirilirsiniz. Aynı verilerle güncelleştirilmiş bir sayfa blobunun bir sayfası için de aynı değer geçerlidir.
* [Uploadfromfile][dotnet_UploadFromFile], [Uploadtext][dotnet_UploadText], [Uploadfromstream][dotnet_UploadFromStream]veya [uploadfrombrivtearray][dotnet_UploadFromByteArray] yöntemi çağırarak bir blok blobunun değiştirilmesi, blobdaki tüm blokları değiştirir. Bu bloba ilişkili bir anlık görüntü varsa, temel blob ve anlık görüntüdeki tüm bloklar artık birbirinden kalır ve her iki blobdaki tüm bloklar için ücretlendirilirsiniz. Bu, temel bloba ve anlık görüntüdeki veriler aynı kalabilse bile geçerlidir.
* Azure Blob hizmeti, iki Blobun aynı verileri içerip içermediğini belirleme anlamına gelir. Karşıya yüklenen ve yürütülen her bir blok, aynı verilere ve aynı blok KIMLIĞINE sahip olsa bile benzersiz olarak değerlendirilir. Ücretler benzersiz bloklar için tahakkuk ettiğinden, anlık görüntü sonucu olan bir Blobun ek benzersiz bloklar ve ek ücretler ile güncelleştirilmesini göz önünde bulundurmanız önemlidir.

### <a name="minimize-cost-with-snapshot-management"></a>Anlık görüntü yönetimiyle maliyeti en aza indirme

Ek ücretlerden kaçınmak için anlık görüntülerinizi dikkatle yönetmeniz önerilir. Anlık görüntülerinizin depolanması tarafından tahakkuk eden maliyetleri en aza indirmeye yardımcı olması için bu en iyi yöntemleri izleyebilirsiniz:

* Aynı verilerle güncelleştirseniz bile blob 'u her güncelleştirdiğinizde bir blob ile ilişkili anlık görüntüleri silin ve yeniden oluşturun, çünkü uygulama tasarımınız anlık görüntüleri korumanızı gerektirmez. Blobun anlık görüntülerini silip yeniden oluşturarak Blobun ve anlık görüntülerin birbirinden yönlendirilmemesini sağlayabilirsiniz.
* Bir Blobun anlık görüntülerini korumak istiyorsanız, blobu güncelleştirmek için [Uploadfromfile][dotnet_UploadFromFile], [Uploadtext][dotnet_UploadText], [Uploadfromstream][dotnet_UploadFromStream]veya [uploadfrombrivtearray][dotnet_UploadFromByteArray] çağrılmasını önleyin. Bu yöntemler, blobdaki tüm blokları değiştirerek temel Blobun ve anlık görüntülerinin önemli ölçüde ayrılmış olmasına neden olur. Bunun yerine, [Putblock][dotnet_PutBlock] ve [putblocklist][dotnet_PutBlockList] yöntemlerini kullanarak mümkün olan en az sayıda blok güncelleştirin.

### <a name="snapshot-billing-scenarios"></a>Anlık görüntü faturalandırma senaryoları
Aşağıdaki senaryolarda, giderlerin bir Blok Blobu ve anlık görüntüleri için nasıl tahakkuk olduğu gösterilmektedir.

**Senaryo 1**

Senaryo 1 ' de, anlık görüntü alındıktan sonra temel blob güncelleştirilmedi, bu nedenle ücretler yalnızca 1, 2 ve 3 benzersiz bloklar için ücretlendirilir.

![Azure depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**Senaryo 2**

Senaryo 2 ' de, temel blob güncelleştirildi, ancak anlık görüntü yok. 2\. blok güncelleştirildi ve aynı verileri ve aynı KIMLIĞI içerse de, anlık görüntüde blok 3 ile aynı değildir. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**Senaryo 3**

Senaryo 3 ' te, temel blob güncelleştirildi, ancak anlık görüntü yok. Blok 3, temel Blobun içindeki blok 4 ile değiştirilmiştir, ancak anlık görüntü hala blok 3 ' ü yansıtır. Sonuç olarak, hesap dört blok için ücretlendirilir.

![Azure depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**Senaryo 4**

Senaryo 4 ' te, temel blob tamamen güncelleştirilmiştir ve özgün bloklarından hiçbirini içermez. Sonuç olarak, hesap tüm sekiz benzersiz blok için ücretlendirilir. Bu senaryo, bu yöntemler bir blob 'un tüm içeriğini aştığından [Uploadfromfile][dotnet_UploadFromFile], [Uploadtext][dotnet_UploadText], [Uploadfromstream][dotnet_UploadFromStream]veya [uploadfrombrivtearray][dotnet_UploadFromByteArray]gibi bir Update yöntemi kullanıyorsanız oluşabilir.

![Azure depolama kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Artımlı anlık görüntülerle Azure YÖNETILMEYEN VM disklerini yedekleme](../../virtual-machines/windows/incremental-snapshots.md) bölümünde sanal makıne (VM) disk anlık görüntüleriyle çalışma hakkında daha fazla bilgi edinebilirsiniz

* Blob depolamayı kullanan ek kod örnekleri için bkz. [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Örnek bir uygulamayı indirebilir, çalıştırabilir veya GitHub 'daki koda gidebilirsiniz.

[dotnet_AccessCondition]: https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.accesscondition
[dotnet_CloudBlockBlob]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob
[dotnet_CreateSnapshotAsync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.createsnapshotasync
[dotnet_HTTPStatusCode]: https://docs.microsoft.com/java/api/com.microsoft.store.partnercenter.network.httpstatuscode
[dotnet_PutBlockList]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblocklist
[dotnet_PutBlock]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblock
[dotnet_UploadFromByteArray]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.uploadfrombytearray
[dotnet_UploadFromFile]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.uploadfromfile
[dotnet_UploadFromStream]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadfromstream
[dotnet_UploadText]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadtext