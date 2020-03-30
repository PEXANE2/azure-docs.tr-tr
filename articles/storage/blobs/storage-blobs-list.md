---
title: .NET - Azure Depolama ile liste lekeleri
description: .NET istemci kitaplığını kullanarak Azure Depolama hesabınızdaki bir kapsayıcıdaki lekeleri nasıl listeleyişsüreceğinizi öğrenin. Kod örnekleri, düz bir listedeki lekelerin nasıl listeleniyi veya dizinveya klasörler halinde düzenlenmiş gibi blob'ları hiyerarşik olarak nasıl listelediklerini gösterir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: eb62883859a3efeb1c05deb38d8a40fba76e9cdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137929"
---
# <a name="list-blobs-with-net"></a>.NET ile blobs listele

Kodunuzdaki lekeleri listelediğinizde, sonuçların Azure Depolama'dan nasıl döndürülebileceğini yönetmek için bir dizi seçenek belirtebilirsiniz. Her sonuç kümesinde döndürülecek sonuç sayısını belirtebilir ve ardından sonraki kümeleri alabilirsiniz. Adları bu karakter veya dize yle başlayan lekeleri döndürmek için bir önek belirtebilirsiniz. Ve düz bir liste yapısında veya hiyerarşik olarak lekeleri listeleyebilirsiniz. Hiyerarşik bir liste, blobs'ları klasörler halinde düzenlenmiş gibi döndürür. 

Bu makalede, [.NET için Azure Depolama istemci kitaplığını](/dotnet/api/overview/azure/storage?view=azure-dotnet)kullanarak lekelernasıl listelenebilen gösterir.  

## <a name="understand-blob-listing-options"></a>Blob listeleme seçeneklerini anlama

Depo hesabındaki lekeleri listelemek için aşağıdaki yöntemlerden birini arayın:

- [CloudBlobClient.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobs)
- [CloudBlobClient.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmented)
- [CloudBlobClient.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmentedasync)

Bir kapsayıcıdaki lekeleri listelemek için aşağıdaki yöntemlerden birini arayın:

- [CloudBlobContainer.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobs)
- [CloudBlobContainer.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmented)
- [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync)

Bu yöntemleriçin aşırı yüklemeler, lekelerin listeleme işlemi yle nasıl döndürülür olduğunu yönetmek için ek seçenekler sağlar. Bu seçenekler aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-how-many-results-are-returned"></a>Kaç sonuç döndürülür yönetme

Varsayılan olarak, bir listeleme işlemi bir defada en fazla 5000 sonuç döndürür. Daha küçük bir sonuç kümesi döndürmek için `maxresults` **ListBlobs** yöntemlerinden birini ararken parametre için sıfır olmayan bir değer sağlayın.

Bir giriş işlemi 5000'den fazla blob döndürürse `maxresults` veya giriş işleminin depolama hesabında kapsayıcıların bir alt kümesini döndürecek bir değer belirtmişseniz, Azure Depolama blob listesiyle birlikte bir *devam belirteci* döndürür. Devam belirteci, Azure Depolama'dan bir sonraki sonuç kümesini almak için kullanabileceğiniz opak bir değerdir.

Kodunuzda, null olup olmadığını belirlemek için devam belirteci değerini denetleyin. Devam belirteci null olduğunda, sonuç kümesi tamamlanır. Devam belirteci null değilse, devamı belirteci null olana kadar, sonraki sonuç kümesialmak için devamı belirteci geçen, yeniden arama listeleme işlemi.

### <a name="filter-results-with-a-prefix"></a>Sonuçları önek ile filtreleme

Kapsayıcılar listesini filtrelemek için parametre `prefix` için bir dize belirtin. Önek dize bir veya daha fazla karakter içerebilir. Azure Depolama daha sonra yalnızca adları bu önek ile başlayan lekeleri döndürür.

### <a name="return-metadata"></a>Meta verileri döndürme

Sonuçlarla blob meta verilerini döndürmek [için, BlobListingDetails](/dotnet/api/microsoft.azure.storage.blob.bloblistingdetails) numaralandırması için **Meta veri** değerini belirtin. Azure Depolama, döndürülen her blob ile meta veriler içerir, bu nedenle blob meta verilerini almak için bu bağlamda **Alma Öznitelikleri** yöntemlerinden birini aramanız gerekmez.

### <a name="flat-listing-versus-hierarchical-listing"></a>Düz listeleme karşı hiyerarşik listeleme

Azure Depolama'daki blob'lar hiyerarşik bir paradigma (klasik dosya sistemi gibi) yerine düz bir paradigma yla düzenlenir. Ancak, bir klasör yapısını taklit etmek için blob'ları *sanal dizinler* halinde düzenleyebilirsiniz. Sanal dizin blob adının bir parçasını oluşturur ve delimiter karakteri ile gösterilir.

Blob'ları sanal dizinler halinde düzenlemek için, blob adında bir sınır dışılayıcı karakteri kullanın. Varsayılan sınır layıcı karakter bir forward slash (/), ancak delimiter olarak herhangi bir karakter belirtebilirsiniz.

Lekelerinize bir delimiter kullanarak isim atarsanız, lekeleri hiyerarşik olarak listelemeyi seçebilirsiniz. Hiyerarşik bir listeleme işlemi için Azure Depolama, ana nesnenin altındaki sanal dizinleri ve lekeleri döndürür. Hiyerarşide geçiş yapmak için giriş işlemini özyinelemeli olarak çağırabilirsiniz, benzer şekilde klasik bir dosya sisteminden programlı olarak geçebilirsiniz.

## <a name="use-a-flat-listing"></a>Düz bir liste kullanma

Varsayılan olarak, bir listeleme işlemi düz bir girişteki lekeleri döndürür. Düz bir liste, blobs sanal dizin tarafından organize edilmez.

Aşağıdaki örnekte, belirtilen kapsayıcıdaki lekeler, isteğe bağlı bir segment boyutu belirtilmiş olarak düz bir liste kullanarak listeler ve blob adını bir konsol penceresine yazar.

```csharp
private static async Task ListBlobsFlatListingAsync(CloudBlobContainer container, int? segmentSize)
{
    BlobContinuationToken continuationToken = null;
    CloudBlob blob;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned
        // and execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(string.Empty,
                true, BlobListingDetails.Metadata, segmentSize, continuationToken, null, null);

            foreach (var blobItem in resultSegment.Results)
            {
                // A flat listing operation returns only blobs, not virtual directories.
                blob = (CloudBlob)blobItem;

                // Write out some blob properties.
                Console.WriteLine("Blob name: {0}", blob.Name);
            }

            Console.WriteLine();

           // Get the continuation token and loop until it is null.
           continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

Örnek çıktıaşağıdakilere benzer:

```
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

## <a name="use-a-hierarchical-listing"></a>Hiyerarşik bir liste kullanma

Bir listeleme işlemini hiyerarşik olarak çağırdığınızda, Azure Depolama hiyerarşinin ilk düzeyinde sanal dizinleri ve lekeleri döndürür. Her sanal dizinin [Önek](/dotnet/api/microsoft.azure.storage.blob.cloudblobdirectory.prefix) özelliği, bir sonraki dizini almak için özyinelemeli bir çağrıda önek geçirebilmeniz için ayarlanır.

Blobs'u hiyerarşik olarak `useFlatBlobListing` listelemek için, listeleme yönteminin parametresini **false**olarak ayarlayın.

Aşağıdaki örnekte, belirtilen kapsayıcıdaki lekeler, isteğe bağlı bir segment boyutu belirtilmiş olarak düz bir liste kullanarak listeler ve blob adını konsol penceresine yazar.

```csharp
private static async Task ListBlobsHierarchicalListingAsync(CloudBlobContainer container, string prefix)
{
    CloudBlobDirectory dir;
    CloudBlob blob;
    BlobContinuationToken continuationToken = null;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned and
        // execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(prefix,
                false, BlobListingDetails.Metadata, null, continuationToken, null, null);
            foreach (var blobItem in resultSegment.Results)
            {
                // A hierarchical listing may return both virtual directories and blobs.
                if (blobItem is CloudBlobDirectory)
                {
                    dir = (CloudBlobDirectory)blobItem;

                    // Write out the prefix of the virtual directory.
                    Console.WriteLine("Virtual directory prefix: {0}", dir.Prefix);

                    // Call recursively with the prefix to traverse the virtual directory.
                    await ListBlobsHierarchicalListingAsync(container, dir.Prefix);
                }
                else
                {
                    // Write out the name of the blob.
                    blob = (CloudBlob)blobItem;

                    Console.WriteLine("Blob name: {0}", blob.Name);
                }
                Console.WriteLine();
            }

            // Get the continuation token and loop until it is null.
            continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

Örnek çıktıaşağıdakilere benzer:

```
Virtual directory prefix: FolderA/
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt

Virtual directory prefix: FolderA/FolderB/
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt

Virtual directory prefix: FolderA/FolderB/FolderC/
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

> [!NOTE]
> Blob anlık görüntüleri hiyerarşik bir listeleme işleminde listelenemez.

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Blobs listesi](/rest/api/storageservices/list-blobs)
- [Blob Kaynaklarını Derecelendirme](/rest/api/storageservices/enumerating-blob-resources)
