---
title: Blob sürüm oluşturmayı etkinleştirme ve yönetme (Önizleme)
titleSuffix: Azure Storage
description: Azure portal blob sürümü oluşturmayı nasıl etkinleştireceğinizi veya bir Azure Resource Manager şablonu kullanarak öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/05/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 0e24bcb54fd26d4a3d983681b3348ef736b277cf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82884351"
---
# <a name="enable-and-manage-blob-versioning"></a>Blob sürüm oluşturmayı etkinleştirme ve yönetme

Depolama hesabı için blob sürüm oluşturma 'yı (Önizleme) Azure portal veya Azure Resource Manager şablonunu kullanarak istediğiniz zaman etkinleştirebilir veya devre dışı bırakabilirsiniz.

## <a name="enable-blob-versioning"></a>Blob sürümü oluşturmayı etkinleştirme

# <a name="azure-portal"></a>[Azure portalındaki](#tab/portal)

Azure portal blob sürümü oluşturmayı etkinleştirmek için:

1. Portalda depolama hesabınıza gidin.
1. **BLOB hizmeti**altında **veri koruma**' yı seçin.
1. **Sürüm oluşturma** bölümünde, **etkin**' i seçin.

:::image type="content" source="media/versioning-enable/portal-enable-versioning.png" alt-text="Azure portal blob sürümü oluşturmayı nasıl etkinleştireceğinizi gösteren ekran görüntüsü":::

# <a name="template"></a>[Şablon](#tab/template)

Bir şablonla blob sürümü oluşturmayı etkinleştirmek için, **ısversioningenabled** özelliği **true**olan bir şablon oluşturun. Aşağıdaki adımlar Azure portal bir şablonun nasıl oluşturulacağını açıklamaktadır.

1. Azure portal **kaynak oluştur**' u seçin.
1. **Market 'Te ara**' te, **şablon dağıtımı**yazın ve ardından **ENTER**tuşuna basın.
1. **Şablon dağıtımı**öğesini seçin, **Oluştur**' u seçin ve ardından **düzenleyicide kendi şablonunuzu oluştur**' u seçin.
1. Şablon Düzenleyicisi 'nde aşağıdaki JSON öğesine yapıştırın. `<accountName>`Yer tutucusunu depolama hesabınızın adıyla değiştirin.
1. Şablonu kaydedin.
1. Hesabın kaynak grubunu belirtin ve ardından şablonu dağıtmak ve BLOB sürümü oluşturmayı etkinleştirmek için **satın al** düğmesini seçin.

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts/blobServices",
                "apiVersion": "2019-06-01",
                "name": "<accountName>/default",
                "properties": {
                    "IsVersioningEnabled": true
                }
            }
        ]
    }
    ```

Azure portal şablonlar ile kaynak dağıtma hakkında daha fazla bilgi için bkz. [Azure Portal ile kaynak dağıtma](../../azure-resource-manager/templates/deploy-portal.md).

---

## <a name="modify-a-blob-to-trigger-a-new-version"></a>Yeni bir sürüm tetiklemek için blobu değiştirme

Aşağıdaki kod örneği, .NET için Azure depolama istemci kitaplığı ile yeni bir sürüm oluşturma 'nın nasıl tetikleneceğini göstermektedir. Bu örneği çalıştırmadan önce, depolama hesabınız için sürüm oluşturmayı etkinleştirdiğinizden emin olun.

Örnek bir Blok Blobu oluşturur ve ardından Blobun meta verilerini günceller. Blob 'un meta verilerini güncelleştirme, yeni bir sürümün oluşturulmasını tetikler. Örnek, ilk sürümü ve geçerli sürümü alır ve yalnızca geçerli sürümün meta verileri içerdiğini gösterir.

```csharp
public static async Task UpdateVersionedBlobMetadata(string containerName, string blobName)
{
    // Create a new service client from the connection string.
    BlobServiceClient blobServiceClient = new BlobServiceClient(ConnectionString);

    // Create a new container client.
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(containerName);

    try
    {
        // Create the container.
        await containerClient.CreateIfNotExistsAsync();

        // Upload a block blob.
        BlockBlobClient blockBlobClient = containerClient.GetBlockBlobClient(blobName);

        string blobContents = string.Format("Block blob created at {0}.", DateTime.Now);
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        string initalVersionId;
        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            Response<BlobContentInfo> uploadResponse = await blockBlobClient.UploadAsync(stream, null, default);

            // Get the version ID for the current version.
            initalVersionId = uploadResponse.Value.VersionId;
        }

        // Update the blob's metadata to trigger the creation of a new version.
        Dictionary<string, string> metadata = new Dictionary<string, string>
        {
            { "key", "value" },
            { "key1", "value1" }
        };

        Response<BlobInfo> metadataResponse = await blockBlobClient.SetMetadataAsync(metadata);

        // Get the version ID for the new current version.
        string newVersionId = metadataResponse.Value.VersionId;

        // Request metadata on the previous version.
        BlockBlobClient initalVersionBlob = blockBlobClient.WithVersion(initalVersionId);
        Response<BlobProperties> propertiesResponse = await initalVersionBlob.GetPropertiesAsync();
        PrintMetadata(propertiesResponse);

        // Request metadata on the current version.
        BlockBlobClient newVersionBlob = blockBlobClient.WithVersion(newVersionId);
        Response<BlobProperties> newPropertiesResponse = await newVersionBlob.GetPropertiesAsync();
        PrintMetadata(newPropertiesResponse);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
    finally
    {
        await containerClient.DeleteAsync();
    }
}

static void PrintMetadata(Response<BlobProperties> propertiesResponse)
{
    if (propertiesResponse.Value.Metadata.Count > 0)
    {
        Console.WriteLine("Metadata values for version {0}:", propertiesResponse.Value.VersionId);
        foreach (var item in propertiesResponse.Value.Metadata)
        {
            Console.WriteLine("Key:{0}  Value:{1}", item.Key, item.Value);
        }
    }
    else
    {
        Console.WriteLine("Version {0} has no metadata.", propertiesResponse.Value.VersionId);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)
- [Azure Depolama blobları için geçici silme](soft-delete-overview.md)
