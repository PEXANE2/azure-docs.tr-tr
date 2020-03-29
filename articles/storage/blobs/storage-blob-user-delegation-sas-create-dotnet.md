---
title: Bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturmak için .NET'i kullanın
titleSuffix: Azure Storage
description: Azure Depolama için .NET istemci kitaplığını kullanarak Azure Active Directory kimlik bilgileriyle bir kullanıcı delegasyonu SAS nasıl oluşturabilirsiniz öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/18/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: 385d2c3b88bc2e4d653dae2dc9670cb9e9388faf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75371845"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-net"></a>.NET ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturma

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Bu makalede, .NET için Azure Depolama istemci kitaplığı ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturmak için Azure Active Directory (Azure AD) kimlik bilgilerini nasıl kullanılacağı nı gösterir.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="assign-rbac-roles-for-access-to-data"></a>Verilere erişim için RBAC rollerini atama

Bir Azure REKLAM güvenlik ilkesi blob verilerine erişmeye çalıştığında, bu güvenlik ilkesinin kaynak için izinleri olmalıdır. Güvenlik ilkesi ister Azure'da yönetilen bir kimlik olsun ister geliştirme ortamında kod çalıştıran bir Azure AD kullanıcı hesabı olsun, güvenlik ilkesine Azure Depolama'daki blob verilerine erişim sağlayan bir RBAC rolü atanmalıdır. RBAC üzerinden izin atama hakkında daha fazla bilgi için, Azure Etkin [Dizini'ni kullanarak Azure blob'larına ve kuyruklarına erişim yetkisindeki](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights) **erişim hakları için RBAC rolleri ata** başlıklı bölüme bakın.

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

Azure Depolama'dan Azure Identity istemci kitaplığıyla kimlik doğrulama hakkında daha fazla bilgi edinmek [için, Azure Etkin Dizini ve Azure Kaynakları için yönetilen kimliklerle blob'lara ve kuyruklara erişimi Yetkilendirme'de](../common/storage-auth-aad-msi.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library) **Azure Kimlik kitaplığıyla Kimlik Doğrulama** başlıklı bölüme bakın.

## <a name="add-using-directives"></a>Using yönergeleri ekleme

Azure Kimliği `using` ve Azure Depolama istemci kitaplıklarını kullanmak için kodunuza aşağıdaki yönergeleri ekleyin.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure;
using Azure.Identity;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

## <a name="get-an-authenticated-token-credential"></a>Kimlik doğrulaması belirteç kimlik bilgisi alma

Kodunuzu Azure Depolama'ya istekleri yetkilendirmek için kullanabileceği bir belirteç kimlik bilgisi almak için [Varsayılan Azure Credential](/dotnet/api/azure.identity.defaultazurecredential) sınıfının bir örneğini oluşturun.

Aşağıdaki kod snippet nasıl kimlik doğrulaması belirteç kimlik almak ve Blob depolama için bir hizmet istemcisi oluşturmak için kullanabilirsiniz gösterir:

```csharp
// Construct the blob endpoint from the account name.
string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

// Create a new Blob service client with Azure AD credentials.
BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint),
                                                     new DefaultAzureCredential());
```

## <a name="get-the-user-delegation-key"></a>Kullanıcı delegasyonu anahtarını alın

Her SAS bir anahtar la imzalanır. Bir kullanıcı delegasyonu SAS oluşturmak için önce bir kullanıcı delegasyonu anahtarı istemeniz gerekir, daha sonra SAS imzalamak için kullanılır. Kullanıcı delegasyonu anahtarı, Azure REKLAM kimlik bilgilerinize dayanması dışında bir hizmet SAS'ı veya hesap SAS'ı imzalamak için kullanılan hesap anahtarına benzer. Bir istemci OAuth 2.0 belirteci kullanarak bir kullanıcı delegasyonu anahtarı istediğinde, Azure Depolama kullanıcı temsilciliği anahtarını kullanıcı adına döndürür.

Kullanıcı delegasyonu anahtarına sahip olduktan sonra, bu anahtarı n için anahtar ömrü boyunca herhangi bir sayıda kullanıcı delegasyonu paylaşılan erişim imzaları oluşturmak için kullanabilirsiniz. Kullanıcı delegasyonu anahtarı, onu elde etmek için kullanılan OAuth 2.0 belirtecinden bağımsızdır, bu nedenle anahtar hala geçerli olduğu sürece belirteci yenilemek gerekmez. Anahtarın 7 güne kadar geçerli olduğunu belirtebilirsiniz.

Kullanıcı temsilciliği anahtarını istemek için aşağıdaki yöntemlerden birini kullanın:

- [GetUserDelegationKey](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkey)
- [GetUserDelegationKeyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkeyasync)

Aşağıdaki kod snippet kullanıcı delegasyonu anahtarı alır ve özelliklerini yazar:

```csharp
// Get a user delegation key for the Blob service that's valid for seven days.
// You can use the key to generate any number of shared access signatures over the lifetime of the key.
UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                   DateTimeOffset.UtcNow.AddDays(7));

// Read the key's properties.
Console.WriteLine("User delegation key properties:");
Console.WriteLine("Key signed start: {0}", key.SignedStartsOn);
Console.WriteLine("Key signed expiry: {0}", key.SignedExpiresOn);
Console.WriteLine("Key signed object ID: {0}", key.SignedObjectId);
Console.WriteLine("Key signed tenant ID: {0}", key.SignedTenantId);
Console.WriteLine("Key signed service: {0}", key.SignedService);
Console.WriteLine("Key signed version: {0}", key.SignedVersion);
```

## <a name="create-the-sas-token"></a>SAS belirteci oluşturma

Aşağıdaki kod snippet yeni bir [BlobSasBuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) oluşturmak ve kullanıcı delegasyonu SAS için parametreleri sağlar gösterir. Snippet sonra SAS belirteç dizealmak için [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) çağırır. Son olarak, kod kaynak adresi ve SAS belirteci de dahil olmak üzere tam URI oluşturur.

```csharp
// Create a SAS token that's valid for one hour.
BlobSasBuilder sasBuilder = new BlobSasBuilder()
{
    BlobContainerName = containerName,
    BlobName = blobName,
    Resource = "b",
    StartsOn = DateTimeOffset.UtcNow,
    ExpiresOn = DateTimeOffset.UtcNow.AddHours(1)
};

// Specify read permissions for the SAS.
sasBuilder.SetPermissions(BlobSasPermissions.Read);

// Use the key to get the SAS token.
string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

// Construct the full URI, including the SAS token.
UriBuilder fullUri = new UriBuilder()
{
    Scheme = "https",
    Host = string.Format("{0}.blob.core.windows.net", accountName),
    Path = string.Format("{0}/{1}", containerName, blobName),
    Query = sasToken
};
```

## <a name="example-get-a-user-delegation-sas"></a>Örnek: Bir kullanıcı delegasyonu SAS alın

Aşağıdaki örnek yöntem, güvenlik ilkesinin kimliğini doğrulamak ve kullanıcı delegasyonu SAS oluşturmak için tam kodu gösterir:

```csharp
async static Task<Uri> GetUserDelegationSasBlob(string accountName, string containerName, string blobName)
{
    // Construct the blob endpoint from the account name.
    string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

    // Create a new Blob service client with Azure AD credentials.  
    BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint), 
                                                            new DefaultAzureCredential());

    // Get a user delegation key for the Blob service that's valid for seven days.
    // You can use the key to generate any number of shared access signatures over the lifetime of the key.
    UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow, 
                                                                        DateTimeOffset.UtcNow.AddDays(7));

    // Read the key's properties.
    Console.WriteLine("User delegation key properties:");
    Console.WriteLine("Key signed start: {0}", key.SignedStartsOn);
    Console.WriteLine("Key signed expiry: {0}", key.SignedExpiresOn);
    Console.WriteLine("Key signed object ID: {0}", key.SignedObjectId);
    Console.WriteLine("Key signed tenant ID: {0}", key.SignedTenantId);
    Console.WriteLine("Key signed service: {0}", key.SignedService);
    Console.WriteLine("Key signed version: {0}", key.SignedVersion);
    Console.WriteLine();

    // Create a SAS token that's valid for one hour.
    BlobSasBuilder sasBuilder = new BlobSasBuilder()
    {
        BlobContainerName = containerName,
        BlobName = blobName,
        Resource = "b",
        StartsOn = DateTimeOffset.UtcNow,
        ExpiresOn = DateTimeOffset.UtcNow.AddHours(1)
    };

    // Specify read permissions for the SAS.
    sasBuilder.SetPermissions(BlobSasPermissions.Read);

    // Use the key to get the SAS token.
    string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

    // Construct the full URI, including the SAS token.
    UriBuilder fullUri = new UriBuilder()
    {
        Scheme = "https",
        Host = string.Format("{0}.blob.core.windows.net", accountName),
        Path = string.Format("{0}/{1}", containerName, blobName),
        Query = sasToken
    };

    Console.WriteLine("User delegation SAS URI: {0}", fullUri);
    Console.WriteLine();
    return fullUri.Uri;
}
```

## <a name="example-read-a-blob-with-a-user-delegation-sas"></a>Örnek: SAS kullanıcı delegasyonu yla bir leke okuyun

Aşağıdaki örnek, simüle edilmiş bir istemci uygulamasından önceki örnekte oluşturulan kullanıcı delegasyonu SAS'ı sınar. SAS geçerliyse, istemci uygulaması lekenin içeriğini okuyabilir. SAS geçersizse (örneğin süresi dolmuşsa, Azure Depolama hata kodu 403'ü (Yasak) döndürür.

```csharp
private static async Task ReadBlobWithSasAsync(Uri sasUri)
{
    // Try performing blob operations using the SAS provided.

    // Create a blob client object for blob operations.
    BlobClient blobClient = new BlobClient(sasUri, null);

    // Download and read the contents of the blob.
    try
    {
        // Download blob contents to a stream and read the stream.
        BlobDownloadInfo blobDownloadInfo = await blobClient.DownloadAsync();
        using (StreamReader reader = new StreamReader(blobDownloadInfo.Content, true))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                Console.WriteLine(line);
            }
        }

        Console.WriteLine();
        Console.WriteLine("Read operation succeeded for SAS {0}", sasUri);
        Console.WriteLine();
    }
    catch (RequestFailedException e)
    {
        // Check for a 403 (Forbidden) error. If the SAS is invalid,
        // Azure Storage returns this error.
        if (e.Status == 403)
        {
            Console.WriteLine("Read operation failed for SAS {0}", sasUri);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni verme](../common/storage-sas-overview.md)
- [Kullanıcı Delegasyonu Anahtarı işlemi alın](/rest/api/storageservices/get-user-delegation-key)
- [Bir kullanıcı delegasyonu SAS (REST API) oluşturma](/rest/api/storageservices/create-user-delegation-sas)
