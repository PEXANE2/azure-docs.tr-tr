---
title: Ekleri Azure Cosmos DB
description: Bu makalede Azure Cosmos DB eklerine genel bakış sunulmaktadır.
author: aliuy
ms.author: andrl
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 08/07/2020
ms.reviewer: sngun
ms.openlocfilehash: cb5dbe5101231963f45132fca58279b934c5e49c
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90055376"
---
# <a name="azure-cosmos-db-attachments"></a>Ekleri Azure Cosmos DB

Azure Cosmos DB ekler, bir dış blob veya medya dosyası ile ilişkili meta verilere yönelik başvuruları içeren özel öğelerdir.

Azure Cosmos DB iki tür eki destekler:

* **Yönetilmeyen ekler** , bir dış hizmette (örneğin, Azure depolama, OneDrive, vb.) depolanan bir blob 'A yönelik URI başvurusu etrafında bir sarmalayıcıdır. Bu yaklaşım, bir URI özelliğini standart Azure Cosmos DB öğesinde depolamaya benzer.
* **Yönetilen ekler** , Azure Cosmos DB tarafından dahili olarak yönetilen ve depolanan ve sistem tarafından oluşturulan bir Medialink aracılığıyla sunulan bloblardır.


> [!NOTE]
> Ek eski bir özelliktir. Bu özelliği zaten kullanıyorsanız, desteği devam eden işlevselliği sunmaya yönelik kapsamlardır.
> 
> Ekleri kullanmak yerine, blob verilerini depolamak için Azure Blob Storage 'ı amaç tarafından oluşturulan bir BLOB depolama hizmeti olarak kullanmanızı öneririz. Blob 'larla ilgili meta verileri, başvuru URI bağlantılarıyla birlikte Azure Cosmos DB öğe özellikleri olarak depolamaya devam edebilirsiniz. Bu verilerin Azure Cosmos DB ' de depolanması, Azure Blob depolamada depolanan Blobların meta verilerini ve bağlantılarını sorgulama yeteneği sağlar.
> 
> Microsoft, ekleri tamamen kullanımdan kaldıramadan önce, daha fazla tarihte duyurulacak en az 36 aylık bir bildirim sağlamaya kararlıdır.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Azure Cosmos DB yönetilen ekler standart öğe desteğinden farklıdır. bu sayede, sınırsız ölçeklenebilirlik, küresel dağıtım ve diğer Azure hizmetleriyle tümleştirme sunar.

- Ekler, Azure Cosmos DB SDK 'larının tüm sürümlerinde desteklenmez.
- Yönetilen ekler veritabanı hesabı başına 2 GB depolama ile sınırlıdır.
- Yönetilen ekler Azure Cosmos DB genel dağıtımı ile uyumlu değildir ve bölgeler arasında çoğaltılmaz.

## <a name="migrating-attachments-to-azure-blob-storage"></a>Ekleri Azure Blob depolamaya geçirme

Aşağıdaki adımları izleyerek Azure Cosmos DB eklerini Azure Blob depolamaya geçirmeyi öneririz:

1. Kaynak Azure Cosmos DB kapsayıcıınızdan ek verileri hedef Azure Blob depolama kapsayıcınıza kopyalayın.
2. Hedef Azure Blob depolama kapsayıcısında karşıya yüklenen blob verilerini doğrulayın.
3. Uygulanabiliyorsa, Azure Cosmos DB veri kümeniz içinde dize özellikleri olarak Azure Blob depolamada bulunan bloblara URI başvuruları ekleyin.
4. Yeni Azure Blob depolama kapsayıcısından blob 'ları okumak ve yazmak için uygulama kodunuzu yeniden düzenleyin.

Aşağıdaki DotNet kod örneği, Azure Cosmos DB .NET SDK v2 ve Azure Blob Storage .NET SDK V12 kullanarak bir geçiş akışının parçası olarak Azure Cosmos DB eklerin Azure Blob depolama alanına nasıl kopyalanacağını gösterir. `<placeholder values>`Kaynak Azure Cosmos DB hesabı ve hedef Azure Blob depolama kapsayıcısını değiştirdiğinizden emin olun.

```csharp

using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;

namespace attachments
{
    class Program
    {
        private static string cosmosAccount = "<Your_Azure_Cosmos_account_URI>";
        private static string cosmosKey = "<Your_Azure_Cosmos_account_PRIMARY_KEY>";
        private static string cosmosDatabaseName = "<Your_Azure_Cosmos_database>";
        private static string cosmosCollectionName = "<Your_Azure_Cosmos_collection>";
        private static string storageConnectionString = "<Your_Azure_Storage_connection_string>";
        private static string storageContainerName = "<Your_Azure_Storage_container_name>";
        private static DocumentClient cosmosClient = new DocumentClient(new Uri(cosmosAccount), cosmosKey);
        private static BlobServiceClient storageClient = new BlobServiceClient(storageConnectionString);
        private static BlobContainerClient storageContainerClient = storageClient.GetBlobContainerClient(storageContainerName);

        static void Main(string[] args)
        {
            CopyAttachmentsToBlobsAsync().Wait();
        }

        private async static Task CopyAttachmentsToBlobsAsync()
        {
            Console.WriteLine("Copying Azure Cosmos DB Attachments to Azure Blob Storage ...");

            int totalCount = 0;
            string docContinuation = null;

            // Iterate through each item (document in v2) in the Azure Cosmos DB container (collection in v2) to look for attachments.
            do
            {
                FeedResponse<dynamic> response = await cosmosClient.ReadDocumentFeedAsync(
                    UriFactory.CreateDocumentCollectionUri(cosmosDatabaseName, cosmosCollectionName),
                    new FeedOptions
                    {
                        MaxItemCount = -1,
                        RequestContinuation = docContinuation
                    });
                docContinuation = response.ResponseContinuation;

                foreach (Document document in response)
                {
                    string attachmentContinuation = null;
                    PartitionKey docPartitionKey = new PartitionKey(document.Id);

                    // Iterate through each attachment within the item (if any).
                    do
                    {
                        FeedResponse<Attachment> attachments = await cosmosClient.ReadAttachmentFeedAsync(
                            document.SelfLink,
                            new FeedOptions
                            {
                                PartitionKey = docPartitionKey,
                                RequestContinuation = attachmentContinuation
                            }
                        );
                        attachmentContinuation = attachments.ResponseContinuation;

                        foreach (var attachment in attachments)
                        {
                            // Download the attachment in to local memory.
                            MediaResponse content = await cosmosClient.ReadMediaAsync(attachment.MediaLink);

                            byte[] buffer = new byte[content.ContentLength];
                            await content.Media.ReadAsync(buffer, 0, buffer.Length);

                            // Upload the locally buffered attachment to blob storage
                            string blobId = String.Concat(document.Id, "-", attachment.Id);

                            Azure.Response<BlobContentInfo> uploadedBob = await storageContainerClient.GetBlobClient(blobId).UploadAsync(
                                new MemoryStream(buffer, writable: false),
                                true
                            );

                            Console.WriteLine("Copied attachment ... Item Id: {0} , Attachment Id: {1}, Blob Id: {2}", document.Id, attachment.Id, blobId);
                            totalCount++;

                            // Clean up attachment from Azure Cosmos DB.
                            // Warning: please verify you've succesfully migrated attachments to blog storage prior to cleaning up Azure Cosmos DB.
                            // await cosmosClient.DeleteAttachmentAsync(
                            //     attachment.SelfLink,
                            //     new RequestOptions { PartitionKey = docPartitionKey }
                            // );

                            // Console.WriteLine("Cleaned up attachment ... Document Id: {0} , Attachment Id: {1}", document.Id, attachment.Id);
                        }

                    } while (!string.IsNullOrEmpty(attachmentContinuation));
                }
            }
            while (!string.IsNullOrEmpty(docContinuation));

            Console.WriteLine("Finished copying {0} attachments to blob storage", totalCount);
        }
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Blob depolamayı](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet) kullanmaya başlama
- [Azure Cosmos DB .NET SDK v2](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.attachment?view=azure-dotnet) aracılığıyla ekleri kullanmaya yönelik başvurular alın
- [Azure Cosmos DB Java SDK v2](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.attachment?view=azure-java-stable) aracılığıyla ekleri kullanmaya yönelik başvurular alın
- [Azure Cosmos DB REST API](https://docs.microsoft.com/rest/api/cosmos-db/attachments) aracılığıyla ekleri kullanmaya yönelik başvurular alın
