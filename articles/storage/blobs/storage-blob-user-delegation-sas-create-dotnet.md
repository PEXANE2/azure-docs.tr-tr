---
title: Bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturmak için .NET kullanın
titleSuffix: Azure Storage
description: Azure depolama için .NET istemci kitaplığı 'nı kullanarak Azure Active Directory kimlik bilgileriyle Kullanıcı temsili SAS (Önizleme) oluşturmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: 915f2895fbc1e543fbbfbb408ba27eb758a40515
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74892541"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-net-preview"></a>.NET ile bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturma (Önizleme)

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Bu makalede, .NET için Azure depolama istemci kitaplığı ile bir kapsayıcı veya blob için Kullanıcı temsili SAS (Önizleme) oluşturmak üzere Azure Active Directory (Azure AD) kimlik bilgilerinin nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="assign-rbac-roles-for-access-to-data"></a>Verilere erişmek için RBAC rolleri atama

Bir Azure AD güvenlik sorumlusu blob verilerine erişmeyi denediğinde, bu güvenlik sorumlusunun kaynak için izinleri olması gerekir. Güvenlik sorumlusunun Azure 'da yönetilen bir kimlik veya geliştirme ortamında kod çalıştıran bir Azure AD Kullanıcı hesabı olup olmadığı, güvenlik sorumlusu Azure Storage 'daki blob verilerine erişim izni veren bir RBAC rolü atanmalıdır. RBAC aracılığıyla izin atama hakkında daha fazla bilgi için, [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights)konusundaki **ERIŞIM hakları Için RBAC rolleri atama** başlıklı bölüme bakın.

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

Azure depolama 'dan Azure kimlik istemci kitaplığı ile kimlik doğrulama hakkında daha fazla bilgi edinmek için bkz. Azure **kimlik kitaplığıyla kimlik doğrulama** başlıklı Bölüm [Azure Active Directory ve Azure kaynakları için Yönetilen kimlikler ile blob 'lara ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad-msi.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library).

## <a name="add-using-directives"></a>Using yönergeleri ekleme

Azure kimlik ve Azure depolama istemci kitaplıklarını kullanmak için aşağıdaki `using` yönergelerini kodunuza ekleyin.

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

## <a name="get-an-authenticated-token-credential"></a>Kimliği doğrulanmış belirteç kimlik bilgisi al

Kodunuzun Azure depolama 'ya istekleri yetkilendirmek için kullanabileceği bir belirteç kimlik bilgisi almak için, [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) sınıfının bir örneğini oluşturun.

Aşağıdaki kod parçacığı kimliği doğrulanmış belirteç kimlik bilgisinin nasıl alınacağını ve BLOB depolaması için bir hizmet istemcisi oluşturmak için nasıl kullanılacağını gösterir:

```csharp
// Construct the blob endpoint from the account name.
string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

// Create a new Blob service client with Azure AD credentials.
BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint),
                                                     new DefaultAzureCredential());
```

## <a name="get-the-user-delegation-key"></a>Kullanıcı temsili anahtarını al

Her SAS bir anahtarla imzalanır. Bir Kullanıcı temsili SAS oluşturmak için, önce SAS imzalamak için kullanılan bir Kullanıcı temsili anahtarı istemeniz gerekir. Kullanıcı temsili anahtarı, Azure AD kimlik bilgilerinizi temel aldığından, bir hizmet SAS 'sini veya bir hesap SAS 'sini imzalamak için kullanılan hesap anahtarına benzer. İstemci bir OAuth 2,0 belirteci kullanarak bir Kullanıcı temsili anahtarı istediğinde, Azure depolama Kullanıcı adına Kullanıcı temsili anahtarını döndürür.

Kullanıcı temsilciliğini aldıktan sonra, anahtarın ömrü boyunca herhangi bir sayıda kullanıcı temsili paylaşılan erişim imzası oluşturmak için bu anahtarı kullanabilirsiniz. Kullanıcı temsili anahtarı, bunu elde etmek için kullanılan OAuth 2,0 belirtecinden bağımsızdır, bu nedenle anahtar hala geçerli olduğu sürece belirtecin yenilenmesi gerekmez. Anahtarın 7 güne kadar bir dönem için geçerli olduğunu belirtebilirsiniz.

Kullanıcı temsili anahtarını istemek için aşağıdaki yöntemlerden birini kullanın:

- [GetUserDelegationKey](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkey)
- [GetUserDelegationKeyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkeyasync)

Aşağıdaki kod parçacığı, Kullanıcı temsili anahtarını alır ve özelliklerini Yazar:

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

Aşağıdaki kod parçacığı, yeni bir [Blobsasbuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) oluşturma ve Kullanıcı temsili SAS için parametreler sağlama gösterilmektedir. Kod parçacığı daha sonra SAS belirteç dizesini almak için [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) çağırır. Son olarak, kod kaynak adresi ve SAS belirteci dahil olmak üzere tüm URI 'yi oluşturur.

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

## <a name="example-get-a-user-delegation-sas"></a>Örnek: Kullanıcı temsilciliğini al SAS

Aşağıdaki örnek yöntemi, güvenlik sorumlusunun kimliğini doğrulamak ve Kullanıcı temsili SAS oluşturmak için tüm kodu gösterir:

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

## <a name="example-read-a-blob-with-a-user-delegation-sas"></a>Örnek: Kullanıcı temsilciliğini içeren bir blobu okuma

Aşağıdaki örnek, bir önceki örnekte oluşturulan kullanıcı temsili SAS 'ı sanal bir istemci uygulamasından sınar. SAS geçerliyse, istemci uygulama Blobun içeriğini okuyabilir. SAS geçersizse, örneğin süresi dolmuşsa Azure Storage 403 (yasak) hata kodunu döndürür.

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

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../common/storage-sas-overview.md)
- [Kullanıcı temsilciyi anahtar işlemi al](/rest/api/storageservices/get-user-delegation-key)
- [Kullanıcı temsilciliğini oluşturma SAS (REST API)](/rest/api/storageservices/create-user-delegation-sas)
