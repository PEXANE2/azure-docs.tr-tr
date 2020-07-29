---
title: Bulunamayan Azure Cosmos DB özel durum sorunlarını giderme
description: Bulunamadı özel durumu tanılama ve çözme
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: f391772672904a1e2bdfa0a741d9f85a6edeea8b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294622"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-not-found"></a>Tanılama ve sorun giderme Azure Cosmos DB bulunamadı
HTTP durum kodu 404, kaynağın artık mevcut olmadığını gösterir.

## <a name="expected-behavior"></a>Beklenen davranış
Bir uygulamanın 404 beklediği ve senaryoyu doğru işlediği birçok geçerli senaryo vardır.

## <a name="not-found-was-returned-for-an-item-that-should-exist-or-does-exist"></a>Bulunmaması veya var olması gereken bir öğe için bulunamadı
Aşağıda, bir durum kodu 404, öğe veya çıkış durumunda döndürülmek için olası sebeplerdir.

### <a name="1-race-condition"></a>1. yarış durumu
Birden çok SDK istemci örneği vardır ve yazma işleminden önce okuma gerçekleşti.

#### <a name="solution"></a>Çözüm:
1. Cosmos DB için varsayılan hesap tutarlılığı, oturum tutarlılığı olur. Bir öğe oluşturulduğunda veya güncelleştirilirken, yanıt, okuma isteğinin bu değişikliğe sahip bir yinelemeden okumasını sağlamak için SDK örnekleri arasında geçirilemeyen bir oturum belirteci döndürür.
2. [Tutarlılık düzeyini](consistency-levels-choosing.md) [daha güçlü bir düzeye](consistency-levels-tradeoffs.md) değiştirme

### <a name="2-invalid-partition-key-and-id-combination"></a>2. geçersiz bölüm anahtarı ve KIMLIK birleşimi
Bölüm anahtarı ve KIMLIK bileşimi geçerli değil.

#### <a name="solution"></a>Çözüm:
Yanlış kombinasyona neden olan uygulama mantığını düzeltemedi. 

### <a name="3-invalid-character-in-item-id"></a>3. öğe KIMLIĞINDE geçersiz karakter
Öğe KIMLIĞINDE [geçersiz bir karakterle](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.resource.id?view=azure-dotnet#remarks) Cosmos DB bir öğe eklenir.

#### <a name="solution"></a>Çözüm:
Kullanıcıların KIMLIĞI özel karakterler içermeyen farklı bir değere değiştirmesi önerilir. KIMLIĞI değiştirmek bir seçenek değilse, özel karakterlerin kaçış KIMLIĞINI Base64 olarak kodlayabilirsiniz.

Kapsayıcıda zaten eklenmiş olan öğeler, ad tabanlı başvurular yerine RID değerleri kullanılarak değiştirilebilir.
```c#
// Get a container reference that use RID values
ContainerProperties containerProperties = await this.Container.ReadContainerAsync();
string[] selfLinkSegments = containerProperties.SelfLink.Split('/');
string databaseRid = selfLinkSegments[1];
string containerRid = selfLinkSegments[3];
Container containerByRid = this.cosmosClient.GetContainer(databaseRid, containerRid);

// List of invalid characters are listed here.
//https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.resource.id?view=azure-dotnet#remarks
FeedIterator<JObject> invalidItemsIterator = this.Container.GetItemQueryIterator<JObject>(
    @"select * from t where CONTAINS(t.id, ""/"") or CONTAINS(t.id, ""#"") or CONTAINS(t.id, ""?"") or CONTAINS(t.id, ""\\"") ");
while (invalidItemsIterator.HasMoreResults)
{
    foreach (JObject itemWithInvalidId in await invalidItemsIterator.ReadNextAsync())
    {
        // It recommend to chose a new ID that does not contain special characters, but
        // if that is not possible then it can be Base64 encoded to escape the special characters
        byte[] plainTextBytes = Encoding.UTF8.GetBytes(itemWithInvalidId["id"].ToString());
        itemWithInvalidId["id"] = Convert.ToBase64String(plainTextBytes);

        // Update the item with the new ID value using the rid based container reference
        JObject item = await containerByRid.ReplaceItemAsync<JObject>(
            item: itemWithInvalidId,
            ID: itemWithInvalidId["_rid"].ToString(),
            partitionKey: new Cosmos.PartitionKey(itemWithInvalidId["status"].ToString()));

        // Validate the new ID can be read using the original name based contianer reference
        await this.Container.ReadItemAsync<ToDoActivity>(
            item["id"].ToString(),
            new Cosmos.PartitionKey(item["status"].ToString())); ;
    }
}
```

### <a name="4-time-to-live-ttl-purge"></a>4. yaşam süresi (TTL) Temizleme
Öğe, [yaşam süresi (TTL)](https://docs.microsoft.com/azure/cosmos-db/time-to-live) özellik kümesine sahipti. Yaşam süresi dolmadığı için öğe temizlendi.

#### <a name="solution"></a>Çözüm:
Öğenin temizlenme süresini engellemek için zaman etkin olarak değiştirin.

### <a name="5-lazy-indexing"></a>5. Lazy dizin oluşturma
[Yavaş dizin oluşturma](index-policy.md#indexing-mode) işlemi yakalanmadı.

#### <a name="solution"></a>Çözüm:
Dizin oluşturmanın dizin oluşturma ilkesini yakalayıp değiştirmesini bekleyin

### <a name="6-parent-resource-deleted"></a>6. üst kaynak silindi
İçinde öğenin bulunduğu veritabanı ve/veya kapsayıcı silinmiş.

#### <a name="solution"></a>Çözüm:
1. Üst kaynağı [geri yükleyin](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore#backup-retention-period) veya kaynakları yeniden oluşturun.
2. Silinen kaynağı değiştirmek için yeni bir kaynak oluşturun

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB .NET SDK kullanırken sorunları [Tanılama ve giderme](troubleshoot-dot-net-sdk.md)
* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin