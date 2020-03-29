---
title: .NET - Azure Depolama ile Blob depolamasına yapılan istekte müşteri tarafından sağlanan bir anahtar belirtin
description: .NET'i kullanarak Blob depolamasına yapılan bir istekte müşteri tarafından sağlanan anahtarı nasıl belirteceklerini öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 11/26/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: ea8254f108aed9d40e6970a27409035b1e10ab41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74807009"
---
# <a name="specify-a-customer-provided-key-on-a-request-to-blob-storage-with-net"></a>.NET ile Blob depolama sına istek te müşteri tarafından sağlanan bir anahtar belirtin

Azure Blob depolamasına karşı istekte bulunduran istemciler, tek bir istekte şifreleme anahtarı sağlama seçeneğine sahiptir. İstekteki şifreleme anahtarının eklenmesi, Blob depolama işlemleri için şifreleme ayarları üzerinde parçalı denetim sağlar. Müşteri tarafından sağlanan anahtarlar (önizleme) Azure Key Vault'ta veya başka bir anahtar deposunda depolanabilir.

Bu makalede, .NET ile bir istek te müşteri tarafından sağlanan anahtarın nasıl belirtilen gösterir.

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

Azure Depolama'dan Azure Identity istemci kitaplığıyla kimlik doğrulama hakkında daha fazla bilgi edinmek [için, Azure Etkin Dizini ve Azure Kaynakları için yönetilen kimliklerle blob'lara ve kuyruklara erişimi Yetkilendirme'de](../common/storage-auth-aad-msi.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library) **Azure Kimlik kitaplığıyla Kimlik Doğrulama** başlıklı bölüme bakın.

## <a name="example-use-a-customer-provided-key-to-upload-a-blob"></a>Örnek: Bir blob yüklemek için müşteri tarafından sağlanan bir anahtar kullanın

Aşağıdaki örnek, müşteri tarafından sağlanan bir anahtar oluşturur ve bir blob yüklemek için bu anahtarı kullanır. Kod bir blok yükler, ardından blob'u Azure Depolama'ya yazmak için blok listesini işler.

```csharp
async static Task UploadBlobWithClientKey(string accountName, string containerName,
    string blobName, Stream data, byte[] key)
{
    const string blobServiceEndpointSuffix = ".blob.core.windows.net";
    Uri accountUri = new Uri("https://" + accountName + blobServiceEndpointSuffix);

    // Specify the customer-provided key on the options for the client.
    BlobClientOptions options = new BlobClientOptions()
    {
        CustomerProvidedKey = new CustomerProvidedKey(key)
    };

    // Create a client object for the Blob service, including options.
    BlobServiceClient serviceClient = new BlobServiceClient(accountUri, 
        new DefaultAzureCredential(), options);

    // Create a client object for the container.
    // The container client retains the credential and client options.
    BlobContainerClient containerClient = serviceClient.GetBlobContainerClient(containerName);

    // Create a new block blob client object.
    // The blob client retains the credential and client options.
    BlobClient blobClient = containerClient.GetBlobClient(blobName);

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload the data using the customer-provided key.
        await blobClient.UploadAsync(data);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Veriler için Azure Depolama şifrelemesi](../common/storage-service-encryption.md)
- [Azure Etkin Dizini ve Yönetilen kimliklerle Azure Kaynakları için blob'lara ve kuyruklara erişimi yetkilendirme](../common/storage-auth-aad-msi.md)
