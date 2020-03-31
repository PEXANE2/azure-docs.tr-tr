---
title: .NET ile depolama hesabı türü ve SKU adı alın
titleSuffix: Azure Storage
description: .NET istemci kitaplığını kullanarak Azure Depolama hesap türünü ve SKU adını nasıl alacağınızı öğrenin.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/06/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: 1495ed55c24a0f94b911d60d1db0f32940ea134a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137067"
---
# <a name="get-storage-account-type-and-sku-name-with-net"></a>.NET ile depolama hesabı türü ve SKU adı alın

Bu makalede, [.NET için Azure](/dotnet/api/overview/azure/storage?view=azure-dotnet)Depolama istemci kitaplığını kullanarak bir blob için Azure Depolama hesap türü ve SKU adı nasıl alınır gösterilmektedir.

Hesap bilgileri, 2018-03-28 sürümüyle başlayan hizmet sürümlerinde mevcuttur.

## <a name="about-account-type-and-sku-name"></a>Hesap türü ve SKU adı hakkında

**Hesap türü**: Geçerli `BlobStorage` `BlockBlobStorage`hesap `FileStorage` `Storage`türleri `StorageV2`, , , , ve . [Azure depolama hesabına genel bakış,](storage-account-overview.md) çeşitli depolama hesaplarının açıklamaları da dahil olmak üzere daha fazla bilgiye sahiptir.

**SKU adı**: Geçerli `Premium_LRS`SKU adları `Standard_RAGRS`, `Standard_RAGZRS`, `Standard_ZRS` `Premium_ZRS` `Standard_GRS` `Standard_GZRS`, `Standard_LRS`, , , ve . SKU adları büyük/küçük harf duyarlıdır ve [SkuName Sınıfında](/dotnet/api/microsoft.azure.management.storage.models.skuname?view=azure-dotnet)dize alanlarıdır.

## <a name="retrieve-account-information"></a>Hesap bilgilerini alma

Bir blob ile ilişkili depolama hesabı türünü ve SKU adını almak için GetAccountProperties veya [GetAccountPropertiesAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountpropertiesasync?view=azure-dotnet) yöntemini arayın. [GetAccountProperties](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountproperties?view=azure-dotnet)

Aşağıdaki kod örneği, salt okunur hesap özelliklerini alır ve görüntüler.

```csharp
private static async Task GetAccountInfoAsync(CloudBlob blob)
{
    try
    {
        // Get the blob's storage account properties.
        AccountProperties acctProps = await blob.GetAccountPropertiesAsync();

        // Display the properties.
        Console.WriteLine("Account properties");
        Console.WriteLine("  AccountKind: {0}", acctProps.AccountKind);
        Console.WriteLine("      SkuName: {0}", acctProps.SkuName);
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

## <a name="next-steps"></a>Sonraki adımlar

[Azure portalı](https://portal.azure.com) ve Azure REST API'si aracılığıyla bir depolama hesabında gerçekleştirebileceğiniz diğer işlemler hakkında bilgi edinin.

- [Hesap Bilgileri alma işlemi (REST)](/rest/api/storageservices/get-account-information)
