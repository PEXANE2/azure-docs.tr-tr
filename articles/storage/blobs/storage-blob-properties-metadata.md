---
title: .NET - Azure Depolama ile bir blob için özellikleri ve meta verileri yönetme
description: .NET istemci kitaplığını kullanarak sistem özelliklerini nasıl ayarlayıp alabildiğini ve Azure Depolama hesabınızdaki blob'larda özel meta verileri nasıl depolayacağım öğren.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: b4abd7e29dec67ddc1be50a2a6703da2a25551d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137671"
---
# <a name="manage-blob-properties-and-metadata-with-net"></a>Blob özelliklerini ve meta verileri .NET ile yönetme

İçerdikleri verilere ek olarak, blobs destek sistem özellikleri ve kullanıcı tanımlı meta veriler. Bu makalede, [.NET için Azure Depolama istemci kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)ile sistem özellikleri nin ve kullanıcı tanımlı meta verilerin nasıl yönetilengösterilen

## <a name="about-properties-and-metadata"></a>Özellikler ve meta veriler hakkında

- **Sistem özellikleri**: Her Blob depolama kaynağında sistem özellikleri bulunur. Bazıları okunabilir veya ayarlanabilir, diğerleri ise salt okunur. Kapakların altında, bazı sistem özellikleri belirli standart HTTP üstbilgilerine karşılık gelir. .NET için Azure Depolama istemci kitaplığı bu özellikleri sizin için korur.

- **Kullanıcı tanımlı meta veriler**: Kullanıcı tanımlı meta veriler, Blob depolama kaynağı için belirttiğiniz bir veya daha fazla ad değeri çiftinden oluşur. Kaynakla ek değerler depolamak için meta verileri kullanabilirsiniz. Meta veri değerleri yalnızca kendi amaçlarınız içindir ve kaynağın nasıl etkilediğini etkilemez.

Blob depolama kaynağı için meta veri ve özellik değerlerini almak iki adımlı bir işlemdir. Bu değerleri okuyamadan önce, bu değerleri `FetchAttributes` çağırarak `FetchAttributesAsync` veya yönteme çağırarak açıkça getirmeniz gerekir. Bu kuralın `Exists` istisnası, `ExistsAsync` ve yöntemlerin `FetchAttributes` kapakların altında uygun yöntemi çağırmasıdır. Bu yöntemlerden birini aradiğinizde, `FetchAttributes`''

> [!IMPORTANT]
> Depolama kaynağının özelliğinin veya meta veri değerlerinin doldurulmadığını fark ederseniz, `FetchAttributes` kodunuzu veya `FetchAttributesAsync` yöntemi çağırıp çağırmadığını denetleyin.

## <a name="set-and-retrieve-properties"></a>Özellikleri ayarlama ve alma

Aşağıdaki kod örneği `ContentType` bir `ContentLanguage` blob üzerinde ve sistem özelliklerini ayarlar.

```csharp
public static async Task SetBlobPropertiesAsync(CloudBlob blob)
{
    try
    {
        Console.WriteLine("Setting blob properties.");

        // You must explicitly set the MIME ContentType every time
        // the properties are updated or the field will be cleared.
        blob.Properties.ContentType = "text/plain";
        blob.Properties.ContentLanguage = "en-us";

        // Set the blob's properties.
        await blob.SetPropertiesAsync();
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

Blob özelliklerini almak `FetchAttributes` için, `FetchAttributesAsync` `Properties` özelliği doldurmak için blob veya yöntem arayın. Aşağıdaki kod örneği bir blob sistem özelliklerini alır ve bazı değerleri görüntüler:

```csharp
private static async Task GetBlobPropertiesAsync(CloudBlob blob)
{
    try
    {
        // Fetch the blob properties.
        await blob.FetchAttributesAsync();

        // Display some of the blob's property values.
        Console.WriteLine(" ContentLanguage: {0}", blob.Properties.ContentLanguage);
        Console.WriteLine(" ContentType: {0}", blob.Properties.ContentType);
        Console.WriteLine(" Created: {0}", blob.Properties.Created);
        Console.WriteLine(" LastModified: {0}", blob.Properties.LastModified);
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

Bir blob veya kapsayıcı kaynağında meta verileri bir veya daha fazla ad değeri çifti olarak belirtebilirsiniz. Meta verileri ayarlamak için, kaynaktaki `Metadata` koleksiyona ad değeri çiftleri ekleyin. Ardından, değerleri yazmak için aşağıdaki yöntemlerden birini arayın:

- [SetMeta veri](/dotnet/api/microsoft.azure.storage.blob.cloudblob.setmetadata)
- [SetMetadataAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.setmetadataasync)

Meta veri adı/değer çiftleri geçerli HTTP üstbilgileridir ve HTTP üstbilgileri yle ilgili tüm kısıtlamalara uymalıdır. Meta veri adları geçerli http üstbilgi adları ve geçerli C# tanımlayıcıları olmalıdır, yalnızca ASCII karakterleri içerebilir ve büyük/küçük harf duyarsız olarak ele alınmalıdır. ASCII olmayan karakterler içeren meta veri değerlerini [temel 64 kodlayın](https://docs.microsoft.com/dotnet/api/system.convert.tobase64string) veya [URL kodlayın.](https://docs.microsoft.com/dotnet/api/system.web.httputility.urlencode)

Meta verilerinizin adı C# tanımlayıcılarının adlandırma kurallarına uygun olmalıdır. Meta veri adları oluşturulduklarında kullanılan servis talebinin korunmasını, ancak ayarlandığında veya okunduğunda büyük/küçük harf duyarsızlığı dır. Bir kaynak için aynı adı kullanan iki veya daha fazla meta veri üstbilgisi gönderilirse, Azure Blob depolama alanı HTTP hata kodu 400 (Kötü İstek) döndürür.

Aşağıdaki kod örneği, meta verileri bir blob üzerinde ayarlar. Bir değer, koleksiyonun `Add` yöntemi kullanılarak ayarlanır. Diğer değer örtülü anahtar/değer sözdizimi kullanılarak ayarlanır.

```csharp
public static async Task AddBlobMetadataAsync(CloudBlob blob)
{
    try
    {
        // Add metadata to the blob by calling the Add method.
        blob.Metadata.Add("docType", "textDocuments");

        // Add metadata to the blob by using key/value syntax.
        blob.Metadata["category"] = "guidance";

        // Set the blob's metadata.
        await blob.SetMetadataAsync();
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

Meta verileri almak `FetchAttributes` için, `FetchAttributesAsync` `Metadata` koleksiyonu doldurmak için blob veya kapsayıcınızdaki veya yöntemi arayın ve aşağıdaki örnekte gösterildiği gibi değerleri okuyun.

```csharp
public static async Task ReadBlobMetadataAsync(CloudBlob blob)
{
    try
    {
        // Fetch blob attributes in order to populate 
        // the blob's properties and metadata.
        await blob.FetchAttributesAsync();

        Console.WriteLine("Blob metadata:");

        // Enumerate the blob's metadata.
        foreach (var metadataItem in blob.Metadata)
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

- [Blob Properties işlemini ayarla](/rest/api/storageservices/set-blob-properties)
- [Blob Properties işlemini alın](/rest/api/storageservices/get-blob-properties)
- [Blob Metaveri işlemini ayarlama](/rest/api/storageservices/set-blob-metadata)
- [Blob Metadata işlemini alın](/rest/api/storageservices/get-blob-metadata)
