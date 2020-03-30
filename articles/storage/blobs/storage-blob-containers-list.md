---
title: Blob kaplarını .NET ile listele - Azure Depolama
description: .NET istemci kitaplığını kullanarak Azure Depolama hesabınızdaki blob kapsayıcılarını nasıl listeleyiş süreceğinizi öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 155b8f5d50c7b106daff8dab4df17200b844c988
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79135913"
---
# <a name="list-blob-containers-with-net"></a>Blob kapları .NET ile listele

Kodunuzdan bir Azure Depolama hesabındaki kapsayıcıları listelediğinizde, sonuçların Azure Depolama'dan nasıl döndürülebileceğini yönetmek için bir dizi seçenek belirtebilirsiniz. Bu makalede, [.NET için Azure Depolama istemci kitaplığını](/dotnet/api/overview/azure/storage?view=azure-dotnet)kullanarak kapsayıcıların nasıl listelenebildiğini gösterir.  

## <a name="understand-container-listing-options"></a>Konteyner listeleme seçeneklerini anlama

Depolama hesabınızdaki kapsayıcıları listelemek için aşağıdaki yöntemlerden birini arayın:

- [ListeContainersSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listcontainerssegmented)
- [ListeContainersSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listcontainerssegmentedasync)

Bu yöntemleriçin aşırı yüklemeler, kapsayıcıların listeleme işlemi tarafından nasıl döndürülür şekilde döndürülmesini yönetmek için ek seçenekler sağlar. Bu seçenekler aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-how-many-results-are-returned"></a>Kaç sonuç döndürülür yönetme

Varsayılan olarak, bir listeleme işlemi bir defada en fazla 5000 sonuç döndürür. Daha küçük bir sonuç kümesi döndürmek `maxresults` **için, ListContainerSegmented** yöntemlerinden birini ararken parametre için sıfır olmayan bir değer sağlayın.

Depolama hesabınızda 5000'den fazla kapsayıcı varsa veya kayıt `maxresults` işleminin depolama hesabında kapsayıcıların bir alt kümesini döndürecek bir değer belirtmişseniz, Azure Depolama kapsayıcılar listesiyle birlikte bir *devam belirteci* döndürür. Devam belirteci, Azure Depolama'dan bir sonraki sonuç kümesini almak için kullanabileceğiniz opak bir değerdir.

Kodunuzda, null olup olmadığını belirlemek için devam belirteci değerini denetleyin. Devam belirteci null olduğunda, sonuç kümesi tamamlanır. Devam belirteci null değilse, daha sonra **listContainersSegmented** veya **ListContainersSegmentedAsync** tekrar, devamı belirteci sonraki sonuç kümesini almak için geçen, devamı belirteci null kadar arayın.

### <a name="filter-results-with-a-prefix"></a>Sonuçları önek ile filtreleme

Kapsayıcılar listesini filtrelemek için parametre `prefix` için bir dize belirtin. Önek dize bir veya daha fazla karakter içerebilir. Azure Depolama daha sonra yalnızca adları bu önek ile başlayan kapsayıcıları döndürür.

### <a name="return-metadata"></a>Meta verileri döndürme

Kapsayıcı meta verilerini sonuçlarla döndürmek [için, ContainerListingDetails](/dotnet/api/microsoft.azure.storage.blob.containerlistingdetails) numaralandırması için **Meta veri** değerini belirtin. Azure Depolama, döndürülen her kapsayıcıyla birlikte meta veriler içerir, bu nedenle kapsayıcı meta verilerini almak için **Alma Nitelikleri** yöntemlerinden birini de aramanız gerekmez.

## <a name="example-list-containers"></a>Örnek: Kapsayıcıları listele

Aşağıdaki örnek, belirtilen bir önek ile başlayan bir depolama hesabındakapsayıcıları eş senkronize olarak listeler. Örnek, kapsayıcıları bir seferde 5 sonuç artışlı olarak listeler ve sonuçların bir sonraki bölümünü almak için devamı belirteci kullanır. Örnek, kapsayıcı meta verilerini de sonuçlarla birlikte döndürür.

```csharp
private static async Task ListContainersWithPrefixAsync(CloudBlobClient blobClient,
                                                        string prefix)
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

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

[List Containers](/rest/api/storageservices/list-containers2)
[Blob Kaynaklarını Listele Konteynerleri](/rest/api/storageservices/enumerating-blob-resources) Listele
