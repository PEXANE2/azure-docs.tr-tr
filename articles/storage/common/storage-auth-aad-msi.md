---
title: Azure kaynakları için Azure Active Directory ve yönetilen kimlikler ile bloblara ve kuyruklara erişim yetkisi verme-Azure depolama
description: Azure Blob ve kuyruk depolama, Azure kaynakları için Azure Active Directory ve yönetilen kimlikler ile kaynaklara erişim yetkisi verme desteği sağlar. Azure sanal makinelerinde, işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer uygulamalarda çalışan uygulamalardan bloblara ve kuyruklara erişim yetkisi vermek için Azure kaynakları için Yönetilen kimlikler kullanabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/18/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 0f1c66126a1aa9a6ebf6f78ac6fb1ba37ba41829
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68985421"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Azure kaynakları için Azure Active Directory ve yönetilen kimlikler ile bloblara ve kuyruklara erişim yetkisi verme

Azure Blob ve kuyruk depolama, [Azure kaynakları için yönetilen kimliklerle](../../active-directory/managed-identities-azure-resources/overview.md)Azure Active Directory (Azure AD) kimlik doğrulamasını destekler. Azure kaynakları için Yönetilen kimlikler, Azure sanal makinelerinde (VM), işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer hizmetlerde çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak blob ve kuyruk verilerine erişim yetkisi verebilir. Azure AD kimlik doğrulamasıyla birlikte Azure kaynakları için Yönetilen kimlikler kullanarak, kimlik bilgilerini bulutta çalışan uygulamalarınızla depolamaktan kaçınabilirsiniz.  

Bu makalede, Azure VM 'den yönetilen bir kimlikle blob veya kuyruk verilerine erişim yetkisi verme işlemleri gösterilmektedir.

## <a name="enable-managed-identities-on-a-vm"></a>VM 'de yönetilen kimlikleri etkinleştirme

VM 'nizden blob 'lara ve kuyruklara erişim yetkisi vermek üzere Azure kaynakları için Yönetilen kimlikler kullanabilmeniz için önce VM 'de Azure kaynakları için yönetilen kimlikleri etkinleştirmeniz gerekir. Azure kaynakları için yönetilen kimliklerin nasıl etkinleştireceğinizi öğrenmek için şu makalelerden birine bakın:

- [Azure portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager istemci kitaplıkları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-an-azure-ad-managed-identity"></a>Azure AD yönetilen kimliği için izin verme

Blob veya Kuyruk hizmeti bir isteği Azure Storage uygulamanızda yönetilen bir kimlikten yetkilendirmek için, önce bu yönetilen kimlik için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama, blob ve kuyruk verileri için izinleri çevreleyen RBAC rollerini tanımlar. RBAC rolü yönetilen bir kimliğe atandığında, yönetilen kimliğe bu izinler uygun kapsamdaki blob veya kuyruk verilerine verilir.

RBAC rolleri atama hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:

- [Azure portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-cli.md)
- [PowerShell kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-powershell.md)

## <a name="azure-storage-resource-id"></a>Azure depolama kaynak KIMLIĞI

[!INCLUDE [storage-resource-id-include](../../../includes/storage-resource-id-include.md)]

## <a name="net-code-example-create-a-block-blob"></a>.NET kod örneği: Blok Blobu oluşturma

Kod örneği, Azure AD 'den bir OAuth 2,0 belirtecinin nasıl alınacağını ve bir blok blobu oluşturma isteğine yetki vermek için nasıl kullanılacağını gösterir. Bu örneğin çalışmasını sağlamak için, önce önceki bölümlerde özetlenen adımları izleyin.

[!INCLUDE [storage-app-auth-lib-include](../../../includes/storage-app-auth-lib-include.md)]

### <a name="add-the-callback-method"></a>Geri çağırma yöntemini ekleyin

Geri arama yöntemi, belirtecin sona erme zamanını denetler ve gerektiği şekilde yeniler:

```csharp
private static async Task<NewTokenAndFrequency> TokenRenewerAsync(Object state, CancellationToken cancellationToken)
{
    // Specify the resource ID for requesting Azure AD tokens for Azure Storage.
    // Note that you can also specify the root URI for your storage account as the resource ID.
    const string StorageResource = "https://storage.azure.com/";  

    // Use the same token provider to request a new token.
    var authResult = await ((AzureServiceTokenProvider)state).GetAuthenticationResultAsync(StorageResource);

    // Renew the token 5 minutes before it expires.
    var next = (authResult.ExpiresOn - DateTimeOffset.UtcNow) - TimeSpan.FromMinutes(5);
    if (next.Ticks < 0)
    {
        next = default(TimeSpan);
        Console.WriteLine("Renewing token...");
    }

    // Return the new token and the next refresh time.
    return new NewTokenAndFrequency(authResult.AccessToken, next);
}
```

### <a name="get-a-token-and-create-a-block-blob"></a>Belirteç alın ve Blok Blobu oluşturun

Uygulama kimlik doğrulama Kitaplığı **AzureServiceTokenProvider** sınıfını sağlar. Bu sınıfın bir örneği, belirteç alan bir geri aramaya geçirilebilir ve sonra belirteci süresi dolmadan önce yeniler.

Aşağıdaki örnek bir belirteci alır ve yeni bir blob oluşturmak için onu kullanır ve ardından blobu okumak için aynı belirteci kullanır.

```csharp
const string blobName = "https://storagesamples.blob.core.windows.net/sample-container/blob1.txt";

// Get the initial access token and the interval at which to refresh it.
AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
var tokenAndFrequency = await TokenRenewerAsync(azureServiceTokenProvider,CancellationToken.None);

// Create storage credentials using the initial token, and connect the callback function
// to renew the token just before it expires
TokenCredential tokenCredential = new TokenCredential(tokenAndFrequency.Token,
                                                        TokenRenewerAsync,
                                                        azureServiceTokenProvider,
                                                        tokenAndFrequency.Frequency.Value);

StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a blob using the storage credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName),
                                            storageCredentials);

// Upload text to the blob.
await blob.UploadTextAsync(string.Format("This is a blob named {0}", blob.Name));

// Continue to make requests against Azure Storage.
// The token is automatically refreshed as needed in the background.
do
{
    // Read blob contents
    Console.WriteLine("Time accessed: {0} Blob Content: {1}",
                        DateTimeOffset.UtcNow,
                        await blob.DownloadTextAsync());

    // Sleep for ten seconds, then read the contents of the blob again.
    Thread.Sleep(TimeSpan.FromSeconds(10));
} while (true);
```

Uygulama kimlik doğrulaması Kitaplığı hakkında daha fazla bilgi için bkz. [.NET kullanarak Azure Key Vault Için hizmetten hizmete kimlik doğrulaması](../../key-vault/service-to-service-authentication.md).

Erişim belirteci alma hakkında daha fazla bilgi edinmek için bkz. bir [Azure VM 'de Azure kaynakları için Yönetilen kimlikler kullanarak erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

> [!NOTE]
> Azure AD ile blob veya kuyruk verilerinde istekleri yetkilendirmek için bu istekler için HTTPS kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure depolama için RBAC rolleri hakkında daha fazla bilgi edinmek için bkz. [RBAC ile depolama verileri için erişim haklarını yönetme](storage-auth-aad-rbac.md).
- Depolama uygulamalarınızın içindeki kapsayıcılara ve kuyruklara erişimi yetkilendirmeyi öğrenmek için bkz. [Azure AD 'yi depolama uygulamalarıyla kullanma](storage-auth-aad-app.md).
- Azure AD kimlik bilgileriyle Azure CLı ve PowerShell komutlarının nasıl çalıştırılacağını öğrenmek için bkz. [BLOB veya kuyruk verilerine erişmek için Azure AD kimlik bilgileriyle Azure CLI veya PowerShell komutlarını çalıştırma](storage-auth-aad-script.md).
