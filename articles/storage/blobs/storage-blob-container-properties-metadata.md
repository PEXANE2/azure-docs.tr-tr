---
title: Bir blob kapsayıcısının özelliklerini ve meta verilerini yönetmek için .NET kullanın
titleSuffix: Azure Storage
description: .NET istemci kitaplığı 'nı kullanarak, sistem özelliklerini ayarlama ve alma ve Azure Depolama hesabınızdaki blob kapsayıcılarında özel meta verileri depolama hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/01/2020
ms.author: tamram
ms.openlocfilehash: 455595a2e41ecc05f7064044e09df8efcd9d4548
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85833409"
---
# <a name="manage-container-properties-and-metadata-with-net"></a>.NET ile kapsayıcı özelliklerini ve meta verileri yönetme

Blob kapsayıcıları, içerdikleri verilere ek olarak sistem özelliklerini ve Kullanıcı tanımlı meta verileri destekler. Bu makalede, [.net Için Azure Storage istemci kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)ile sistem özelliklerinin ve Kullanıcı tanımlı meta verilerin nasıl yönetileceği gösterilmektedir.

## <a name="about-properties-and-metadata"></a>Özellikler ve meta veriler hakkında

- **Sistem Özellikleri**: her BLOB depolama kaynağında sistem özellikleri bulunur. Bazıları okunabilir veya ayarlanabilir, diğerleri salt okunurdur. Kapakların altında bazı sistem özellikleri bazı standart HTTP üst bilgilerine karşılık gelir. .NET için Azure depolama istemci kitaplığı, bu özellikleri sizin için korur.

- **Kullanıcı tanımlı meta veriler**: Kullanıcı tanımlı meta veriler, bir BLOB depolama kaynağı için belirlediğiniz bir veya daha fazla ad-değer çiftinden oluşur. Kaynak ile ek değerleri depolamak için meta verileri kullanabilirsiniz. Meta veri değerleri yalnızca kendi amacınıza yöneliktir ve kaynağın nasıl davranacağını etkilemez.

Meta veri adı/değer çiftleri geçerli HTTP başlıklardır ve bu nedenle HTTP üst bilgilerini yöneten tüm kısıtlamalara uymalıdır. Meta veri adları geçerli HTTP üst bilgisi adları ve geçerli C# tanımlayıcıları olmalıdır, yalnızca ASCII karakterler içerebilir ve büyük/küçük harfe duyarsız olarak değerlendirilmelidir. ASCII olmayan karakterler içeren meta veri değerleri Base64 kodlamalı veya URL kodlu olmalıdır.

## <a name="retrieve-container-properties"></a>Kapsayıcı özelliklerini al

# <a name="net-v12-sdk"></a>[.NET V12 SDK](#tab/dotnet)

Kapsayıcı özelliklerini almak için aşağıdaki yöntemlerden birini çağırın:

- [GetProperties](/dotnet/api/azure.storage.blobs.blobcontainerclient.getproperties)
- [GetPropertiesAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getpropertiesasync)

Aşağıdaki kod örneği bir kapsayıcının sistem özelliklerini getirir ve bazı özellik değerlerini konsol penceresine yazar:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_ReadContainerProperties":::

# <a name="net-v11-sdk"></a>[.NET v11 SDK](#tab/dotnet11)

BLOB depolama kaynağı için özellik ve meta veri değerlerinin alınması, iki adımlı bir işlemdir. Bu değerleri okuyabilmeniz için, **Fetchattributes** veya **Fetchattributesasync** yöntemini çağırarak açıkça bunları almanız gerekir. Bu kuralın özel durumu **var ve mevcut** **tsasync** yöntemlerinin, kapsamakta olan uygun **fetchattributes** metodunu çağırmasını sağlar. Bu yöntemlerden birini çağırdığınızda, **Fetchattributes**' i de çağırmanız gerekmez.

> [!IMPORTANT]
> Bir depolama kaynağı için özellik veya meta veri değerlerinin doldurulmadığını fark ederseniz, kodunuzun **Fetchattributes** veya **Fetchattributesasync** yöntemini çağırmadığını kontrol edin.

Kapsayıcı özelliklerini almak için aşağıdaki yöntemlerden birini çağırın:

- [FetchAttributes](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.fetchattributes)
- [FetchAttributesAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.fetchattributesasync)

Aşağıdaki kod örneği bir kapsayıcının sistem özelliklerini getirir ve bazı özellik değerlerini konsol penceresine yazar:

```csharp
private static async Task ReadContainerPropertiesAsync(CloudBlobContainer container)
{
    try
    {
        // Fetch some container properties and write out their values.
        await container.FetchAttributesAsync();
        Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri);
        Console.WriteLine("Public access level: {0}", container.Properties.PublicAccess);
        Console.WriteLine("Last modified time in UTC: {0}", container.Properties.LastModified);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

---

## <a name="set-and-retrieve-metadata"></a>Meta verileri ayarlama ve alma

# <a name="net-v12-sdk"></a>[.NET V12 SDK](#tab/dotnet)

Meta verileri bir blob veya kapsayıcı kaynağında bir veya daha fazla ad-değer çifti olarak belirtebilirsiniz. Meta verileri ayarlamak için, bir [IDictionary](/dotnet/api/system.collections.idictionary) nesnesine ad-değer çiftleri ekleyin ve ardından değerleri yazmak için aşağıdaki yöntemlerden birini çağırın:

- [SetMetadata](/dotnet/api/azure.storage.blobs.blobcontainerclient.setmetadata)
- [SetMetadataAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.setmetadataasync)

Meta verilerinizin adı, C# tanımlayıcıları için adlandırma kurallarına uygun olmalıdır. Meta veri adları, oluşturulduğu durumu korur, ancak ayarlandığında veya okurken büyük/küçük harf duyarsız olur. Bir kaynak için aynı ada sahip iki veya daha fazla meta veri üstbilgisi gönderilirse, BLOB depolama alanı virgülle ayırın ve iki değeri ayırır ve HTTP yanıt kodu 200 (Tamam) döndürün.

Aşağıdaki kod örneği bir kapsayıcıda meta verileri ayarlar.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_AddContainerMetadata":::

Meta verileri almak için aşağıdaki yöntemlerden birini çağırın:

- [GetProperties](/dotnet/api/azure.storage.blobs.blobcontainerclient.getproperties)
- [GetPropertiesAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getpropertiesasync).

Ardından, aşağıdaki örnekte gösterildiği gibi değerleri okuyun.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Metadata.cs" id="Snippet_ReadContainerMetadata":::

# <a name="net-v11-sdk"></a>[.NET v11 SDK](#tab/dotnet11)

Meta verileri bir blob veya kapsayıcı kaynağında bir veya daha fazla ad-değer çifti olarak belirtebilirsiniz. Meta verileri ayarlamak için kaynaktaki **meta veri** koleksiyonuna ad-değer çiftleri ekleyin ve ardından değerleri yazmak için aşağıdaki yöntemlerden birini çağırın:

- [SetMetadata](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setmetadata)
- [SetMetadataAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setmetadataasync)

Meta verilerinizin adı, C# tanımlayıcıları için adlandırma kurallarına uygun olmalıdır. Meta veri adları, oluşturulduğu durumu korur, ancak ayarlandığında veya okurken büyük/küçük harf duyarsız olur. Bir kaynak için aynı ada sahip iki veya daha fazla meta veri üstbilgisi gönderilirse, BLOB depolama alanı virgülle ayırın ve iki değeri ayırır ve HTTP yanıt kodu 200 (Tamam) döndürün.

Aşağıdaki kod örneği bir kapsayıcıda meta verileri ayarlar. Bir değer, koleksiyonun **Add** yöntemi kullanılarak ayarlanır. Diğer değer örtük anahtar/değer sözdizimi kullanılarak ayarlanır. Her ikisi de geçerlidir.

```csharp
public static async Task AddContainerMetadataAsync(CloudBlobContainer container)
{
    try
    {
        // Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        // Set the container's metadata.
        await container.SetMetadataAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Meta **verileri almak** için blob veya kapsayıcınızda **Fetchattributes** veya **fetchattributesasync** yöntemini çağırın ve ardından aşağıdaki örnekte gösterildiği gibi değerleri okuyun.

```csharp
public static async Task ReadContainerMetadataAsync(CloudBlobContainer container)
{
    try
    {
        // Fetch container attributes in order to populate the container's properties and metadata.
        await container.FetchAttributesAsync();

        // Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

---

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Kapsayıcı özelliklerini al işlemi](/rest/api/storageservices/get-container-properties)
- [Kapsayıcı meta verileri ayarlama işlemi](/rest/api/storageservices/set-container-metadata)
- [Kapsayıcı meta verilerini al işlemi](/rest/api/storageservices/get-container-metadata)
