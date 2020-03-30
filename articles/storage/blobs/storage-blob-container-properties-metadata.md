---
title: Blob kapsayıcısı için özellikleri ve meta verileri yönetmek için .NET'i kullanın
titleSuffix: Azure Storage
description: .NET istemci kitaplığını kullanarak sistem özelliklerini nasıl ayarlayıp alabildiğini ve Azure Depolama hesabınızdaki blob kapsayıcılarında özel meta verileri nasıl depolayacağım öğren.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.openlocfilehash: c66b521b5cd75825fcafe07b24d5d527c45f5153
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79135930"
---
# <a name="manage-container-properties-and-metadata-with-net"></a>.NET ile kapsayıcı özelliklerini ve meta verileri yönetme

Blob kapsayıcıları, içerdikleri verilere ek olarak sistem özelliklerini ve kullanıcı tanımlı meta verileri destekler. Bu makalede, [.NET için Azure Depolama istemci kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)ile sistem özellikleri nin ve kullanıcı tanımlı meta verilerin nasıl yönetilengösterilen

## <a name="about-properties-and-metadata"></a>Özellikler ve meta veriler hakkında

- **Sistem özellikleri**: Her Blob depolama kaynağında sistem özellikleri bulunur. Bazıları okunabilir veya ayarlanabilir, diğerleri ise salt okunur. Kapakların altında, bazı sistem özellikleri belirli standart HTTP üstbilgilerine karşılık gelir. .NET için Azure Depolama istemci kitaplığı bu özellikleri sizin için korur.

- **Kullanıcı tanımlı meta veriler**: Kullanıcı tanımlı meta veriler, Blob depolama kaynağı için belirttiğiniz bir veya daha fazla ad değeri çiftinden oluşur. Kaynakla ek değerler depolamak için meta verileri kullanabilirsiniz. Meta veri değerleri yalnızca kendi amaçlarınız içindir ve kaynağın nasıl etkilediğini etkilemez.

Blob depolama kaynağı için özellik ve meta veri değerlerini almak iki adımlı bir işlemdir. Bu değerleri okuyamadan **önce, FetchAttributes** veya **FetchAttributesAsync** yöntemini arayarak bunları açıkça getirmeniz gerekir. Bu kuralın istisnası, Var ve **ExistsAsync** yöntemlerinin kapakların altında uygun **Exists** **FetchAttributes** yöntemini çağırmasıdır. Bu yöntemlerden birini aradiğinizde, **FetchAttributes'ı**da aramanız gerekmez.

> [!IMPORTANT]
> Depolama kaynağının özelliğinin veya meta veri değerlerinin doldurulmadığını fark ederseniz, kodunuzu FetchAttributes veya **FetchAttributesAsync** yöntemini çağırıp çağırmadığını denetleyin. **FetchAttributes**

Meta veri adı/değer çiftleri geçerli HTTP üstbilgileridir ve bu nedenle HTTP üstbilgileri yöneten tüm kısıtlamalara uymalıdır. Meta veri adları geçerli http üstbilgi adları ve geçerli C# tanımlayıcıları olmalıdır, yalnızca ASCII karakterleri içerebilir ve büyük/küçük harf duyarsız olarak ele alınmalıdır. ASCII olmayan karakterler içeren meta veri değerleri Base64 kodlanmış veya URL kodlanmış olmalıdır.

## <a name="retrieve-container-properties"></a>Kapsayıcı özelliklerini alma

Kapsayıcı özelliklerini almak için aşağıdaki yöntemlerden birini arayın:

- [FetchAttributes](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.fetchattributes)
- [FetchAttributesAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.fetchattributesasync)

Aşağıdaki kod örneği bir kapsayıcının sistem özelliklerini getirir ve konsol penceresine bazı özellik değerleri yazar:

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

## <a name="set-and-retrieve-metadata"></a>Meta verileri ayarlama ve alma

Bir blob veya kapsayıcı kaynağında meta verileri bir veya daha fazla ad değeri çifti olarak belirtebilirsiniz. Meta verileri ayarlamak için kaynaktaki Meta **veri** koleksiyonuna ad değeri çiftleri ekleyin ve değerleri yazmak için aşağıdaki yöntemlerden birini arayın:

- [SetMeta veri](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setmetadata)
- [SetMetadataAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setmetadataasync)

Meta verilerinizin adı C# tanımlayıcılarının adlandırma kurallarına uygun olmalıdır. Meta veri adları oluşturuldukları durumu korur, ancak ayarlandığında veya okunduğunda büyük/küçük harf duyarsızdır. Bir kaynak için aynı ada sahip iki veya daha fazla meta veri üstbilgisi gönderilirse, Blob depolama http hata kodu 400 (Kötü İstek) döndürür.

Aşağıdaki kod örneği, bir kapsayıcı üzerinde meta verileri ayarlar. Bir değer, koleksiyonun **Ekle** yöntemi kullanılarak ayarlanır. Diğer değer örtülü anahtar/değer sözdizimi kullanılarak ayarlanır. Her ikisi de geçerlidir.

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

Meta verileri almak için, **Meta veri** koleksiyonunu doldurmak için blob veya kapsayıcınızdaki **FetchAttributesAsync** yöntemini arayın ve ardından aşağıdaki örnekte gösterildiği gibi değerleri okuyun. **FetchAttributes**

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

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Konteyner Özellikleri işlemini alın](/rest/api/storageservices/get-container-properties)
- [Kapsayıcı Meta veri işlemini ayarla](/rest/api/storageservices/set-container-metadata)
- [Kapsayıcı Meta veri işlemini alın](/rest/api/storageservices/set-container-metadata)
