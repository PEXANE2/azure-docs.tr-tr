---
title: .NET-Azure depolama ile blob kapsayıcılarını listeleme
description: .NET istemci kitaplığını kullanarak Azure Storage hesabınızdaki blob kapsayıcılarını listeleyeceğinizi öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/10/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 6812ad879427a814206ef1400fcff5f3c4af0e75
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235034"
---
# <a name="list-blob-containers-with-net"></a>.NET ile blob kapsayıcılarını listeleme

Kodlarınızdan bir Azure depolama hesabındaki kapsayıcıları listelürsünüz, sonuçların Azure depolama 'dan nasıl döndürüleceğini yönetmek için bir dizi seçenek belirtebilirsiniz. Bu makalede, [.net Için Azure Storage istemci kitaplığı](/dotnet/api/overview/azure/storage/client)kullanılarak kapsayıcıların nasıl listeyapılacağı gösterilmektedir.  

## <a name="understand-container-listing-options"></a>Kapsayıcı listesi seçeneklerini anlama

Depolama hesabınızdaki kapsayıcıları listelemek için aşağıdaki yöntemlerden birini çağırın:

- [Listcontainerskesimli](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listcontainerssegmented)
- [ListContainersSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listcontainerssegmentedasync)

Bu yöntemlerin aşırı yüklemeleri, kapsayıcıların listeleme işlemi tarafından nasıl döndürüleceğini yönetmek için ek seçenekler sağlar. Bu seçenekler aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-how-many-results-are-returned"></a>Kaç sonuç döndürüldüğünü yönetin

Varsayılan olarak, bir listeleme işlemi bir seferde en fazla 5000 sonuç döndürür. Daha küçük bir sonuç kümesi döndürmek için, **listcontainerkesimli** metotlardan birini `maxresults` çağırırken parametresi için sıfır dışında bir değer sağlayın.

Depolama hesabınızda 5000 ' den fazla kapsayıcı varsa veya liste işlemi depolama hesabındaki kapsayıcıların bir alt kümesini döndürdüğünde `maxresults` bu şekilde bir değer belirttiyseniz Azure Storage, bir *devamlılık belirteci* döndürür. kapsayıcıların listesi. Devamlılık belirteci, Azure depolama 'nın bir sonraki sonuç kümesini almak için kullanabileceğiniz donuk bir değerdir.

Kodunuzda, null olup olmadığını anlamak için devamlılık belirtecinin değerini denetleyin. Devamlılık belirteci null olduğunda, sonuç kümesi tamamlanır. Devamlılık belirteci null değilse, devam belirteci null olana kadar, sonraki sonuç kümesini almak için **Listcontainerskesimli** veya **ListContainersSegmentedAsync** yeniden çağırın.

### <a name="filter-results-with-a-prefix"></a>Sonuçları bir ön eke göre filtrele

Kapsayıcılar listesini filtrelemek için, `prefix` parametre için bir dize belirtin. Ön ek dizesi bir veya daha fazla karakter içerebilir. Daha sonra Azure Storage yalnızca adları bu önek ile başlayan kapsayıcıları döndürür.

### <a name="return-container-metadata"></a>Kapsayıcı meta verilerini döndür

Sonuçlarla kapsayıcı meta verileri döndürmek için [Containerlistdetails](/dotnet/api/microsoft.azure.storage.blob.containerlistingdetails) numaralandırması Için **meta veri** değerini belirtin. Azure depolama, her kapsayıcı tarafından döndürülen meta verileri içerir, bu nedenle kapsayıcı meta verilerini almak için **Fetchattributes** yöntemlerinden birini çağırmanız gerekmez.

## <a name="example-list-containers"></a>Örnek: Kapsayıcıları listeleme

Aşağıdaki örnek, belirtilen bir önek ile başlayan bir depolama hesabındaki kapsayıcıları zaman uyumsuz olarak listeler. Örnek, kapsayıcıları tek seferde 5 sonuçtan oluşan artışlarla listeler ve sonraki sonuç segmentini almak için devamlılık belirtecini kullanır. Örnek ayrıca, sonuçlarla birlikte kapsayıcı meta verileri döndürür.

```csharp
private static async Task ListContainersWithPrefixAsync(CloudBlobClient blobClient, string prefix)
{
    Console.WriteLine("List all containers beginning with prefix {0}, plus container metadata:", prefix);

    try
    {
        ContainerResultSegment resultSegment = null;
        BlobContinuationToken continuationToken = null;

        do
        {
            // List containers beginning with the specified prefix, returning segments of 5 results each.
            // Passing null for the maxResults parameter returns the max number of results (up to 5000).
            // Requesting the container's metadata with the listing operation populates the metadata,
            // so it's not necessary to also call FetchAttributes() to read the metadata.
            resultSegment = await blobClient.ListContainersSegmentedAsync(
                prefix, ContainerListingDetails.Metadata, 5, continuationToken, null, null);

            // Enumerate the containers returned.
            foreach (var container in resultSegment.Results)
            {
                Console.WriteLine("\tContainer:" + container.Name);

                // Write the container's metadata keys and values.
                foreach (var metadataItem in container.Metadata)
                {
                    Console.WriteLine("\t\tMetadata key: " + metadataItem.Key);
                    Console.WriteLine("\t\tMetadata value: " + metadataItem.Value);
                }
            }

            // Get the continuation token. If not null, get the next segment.
            continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0} : {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources](../../../includes/storage-blob-dotnet-resources.md)]

## <a name="see-also"></a>Ayrıca bkz.

[](/rest/api/storageservices/list-containers2)
[BLOB kaynaklarını listeler](/rest/api/storageservices/enumerating-blob-resources) halinde listeleme kapsayıcıları
