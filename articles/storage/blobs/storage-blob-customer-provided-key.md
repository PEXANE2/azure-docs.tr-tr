---
title: .NET-Azure depolama ile BLOB depolama için istekte bir müşteri tarafından sağlanmış anahtar belirtin
description: .NET kullanarak BLOB depolama için bir istekte müşteri tarafından sağlanmış anahtar belirtmeyi öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 11/26/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: ea8254f108aed9d40e6970a27409035b1e10ab41
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74807009"
---
# <a name="specify-a-customer-provided-key-on-a-request-to-blob-storage-with-net"></a>.NET ile BLOB depolama için istekte müşteri tarafından sağlanmış bir anahtar belirtin

Azure Blob depolamada istek yapan istemcilerin, tek bir istekte şifreleme anahtarı sağlama seçeneği vardır. İstek üzerine şifreleme anahtarı dahil olmak üzere, BLOB depolama işlemleri için şifreleme ayarları üzerinde ayrıntılı denetim sağlar. Müşteri tarafından sunulan anahtarlar (Önizleme), Azure Key Vault veya başka bir anahtar deposunda depolanabilir.

Bu makalede, .NET ile bir istekte müşteri tarafından sağlanmış bir anahtarın nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

Azure depolama 'dan Azure kimlik istemci kitaplığı ile kimlik doğrulama hakkında daha fazla bilgi edinmek için bkz. Azure **kimlik kitaplığıyla kimlik doğrulama** başlıklı Bölüm [Azure Active Directory ve Azure kaynakları için Yönetilen kimlikler ile blob 'lara ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad-msi.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library).

## <a name="example-use-a-customer-provided-key-to-upload-a-blob"></a>Örnek: bir blobu karşıya yüklemek için müşteri tarafından sağlanmış bir anahtar kullanın

Aşağıdaki örnek, bir müşteri tarafından sağlanmış anahtar oluşturur ve bir blobu karşıya yüklemek için bu anahtarı kullanır. Kod bir blok yükler ve ardından blok listesini kaydeder ve BLOB 'u Azure depolama 'ya yazar.

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

- [Bekleyen veriler için Azure depolama şifrelemesi](../common/storage-service-encryption.md)
- [Azure kaynakları için Azure Active Directory ve yönetilen kimlikler ile bloblara ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad-msi.md)
