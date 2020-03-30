---
title: Bölümlenmemiş Azure Cosmos kapsayıcılarını bölümlenmiş kapsayıcılara geçirme
description: Varolan tüm bölümlenmemiş kapsayıcıları bölümlenmiş kapsayıcılara nasıl geçirteceklerini öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: mjbrown
ms.openlocfilehash: 742ef62895f3ef64e8fa22ab21d2947bee57776b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623361"
---
# <a name="migrate-non-partitioned-containers-to-partitioned-containers"></a>Bölümlenmemiş kapsayıcıları bölümlenmiş kapsayıcılara geçirme

Azure Cosmos DB, bölme anahtarı olmadan kapsayıcı oluşturmayı destekler. Şu anda, 2,x'ten daha az veya eşit sürümü olan Azure CLI ve Azure Cosmos DB SDK'ları (.Net, Java, NodeJs) kullanarak bölümlenmemiş kapsayıcılar oluşturabilirsiniz. Azure portalını kullanarak bölümlenmemiş kapsayıcılar oluşturamazsınız. Ancak, bu tür bölümlenmemiş kaplar elastik değildir ve 20 GB sabit depolama kapasitesine ve 10K RU/s'lik iş verme limitine sahiptir.

Bölümlenmemiş kapsayıcılar eskidir ve depolama ve iş ortasını ölçeklendirmek için varolan bölümlenmemiş kapsayıcıları bölümlenmiş kapsayıcılara geçirmelisiniz. Azure Cosmos DB, bölümlenmemiş kapsayıcılarınızı bölümlenmiş kapsayıcılara geçirmek için sistem tanımlı bir mekanizma sağlar. Bu belge, varolan tüm bölümlenmemiş kapsayıcıların nasıl otomatik olarak bölümlenmiş kapsayıcılara geçirilir olduğunu açıklar. Otomatik geçiş özelliğinden yalnızca tüm dillerde SDK'ların V3 sürümünü kullanıyorsanız yararlanabilirsiniz.

> [!NOTE]
> Şu anda, bu belgede açıklanan adımları kullanarak Azure Cosmos DB MB MongoDB ve Gremlin API hesaplarını geçiremezsiniz.

## <a name="migrate-container-using-the-system-defined-partition-key"></a>Sistem tanımlı bölüm anahtarını kullanarak kapsayıcıyı geçirin

Geçişi desteklemek için Azure Cosmos DB, bölüm `/_partitionkey` anahtarı olmayan tüm kapsayıcılarda adında sistem tanımlı bir bölüm anahtarı sağlar. Kapsayıcılar geçirildikten sonra bölüm anahtar tanımını değiştiremezsiniz. Örneğin, bölümlenmiş bir kapsayıcıya geçirilen bir kapsayıcının tanımı aşağıdaki gibi olacaktır:

```json
{
    "Id": "CollId" 
  "partitionKey": {
    "paths": [
      "/_partitionKey"
    ],
    "kind": "Hash"
  },
}
```

Kapsayıcı geçirildikten sonra, özelliği belgenin `_partitionKey` diğer özellikleriyle birlikte doldurarak belgeler oluşturabilirsiniz. Özellik, `_partitionKey` belgelerinizin bölüm anahtarını temsil eder.

Doğru bölüm anahtarını seçmek, sağlanan iş çıktısını en iyi şekilde kullanmak için önemlidir. Daha fazla bilgi için [bölüm anahtarı makalesini nasıl seçeceğinizi](partitioning-overview.md) görün.

> [!NOTE]
> Sistem tanımlı bölüm anahtarından ancak tüm dillerde SDK'ların en son/V3 sürümünü kullanıyorsanız yararlanabilirsiniz.

Aşağıdaki örnek, sistem tanımlı bölüm anahtarıyla bir belge oluşturmak ve bu belgeyi okumak için örnek bir kod gösterir:

**Belgenin JSON gösterimi**

```csharp
DeviceInformationItem = new DeviceInformationItem
{
   "id": "elevator/PugetSound/Building44/Floor1/1",
   "deviceId": "3cf4c52d-cc67-4bb8-b02f-f6185007a808",
   "_partitionKey": "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

public class DeviceInformationItem
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }

    [JsonProperty(PropertyName = "deviceId")]
    public string DeviceId { get; set; }

    [JsonProperty(PropertyName = "_partitionKey", NullValueHandling = NullValueHandling.Ignore)]
    public string PartitionKey {get {return this.DeviceId; set; }
}

CosmosContainer migratedContainer = database.Containers["testContainer"];

DeviceInformationItem deviceItem = new DeviceInformationItem() {
  Id = "1234",
  DeviceId = "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
}

ItemResponse<DeviceInformationItem > response = 
  await migratedContainer.CreateItemAsync<DeviceInformationItem>(
    deviceItem.PartitionKey, 
    deviceItem
  );

// Read back the document providing the same partition key
ItemResponse<DeviceInformationItem> readResponse = 
  await migratedContainer.ReadItemAsync<DeviceInformationItem>( 
    partitionKey:deviceItem.PartitionKey, 
    id: device.Id
  );

```

Tam örnek için [.Net örnekleri][1] GitHub deposuna bakın.
                      
## <a name="migrate-the-documents"></a>Belgeleri geçirme

Kapsayıcı tanımı bir bölüm anahtar özelliği ile geliştirilmiş olsa da, kapsayıcı içindeki belgeler otomatik olarak geçirilmiş değildir. Bu da sistem bölümü `/_partitionKey` anahtar özelliği yolunun varolan belgelere otomatik olarak eklenmediğini gösterir. Bir bölüm anahtarı olmadan oluşturulan belgeleri okuyarak varolan belgeleri yeniden bölmeniz `_partitionKey` ve belgelerdeki özellik ile yeniden yazmanız gerekir.

## <a name="access-documents-that-dont-have-a-partition-key"></a>Bölüm anahtarı olmayan belgelere erişin

Uygulamalar "PartitionKey.None" adlı özel sistem özelliğini kullanarak bir bölüm anahtarı olmayan varolan belgelere erişebilir, bu geçirilmeyen belgelerin değeridir. Bu özelliği tüm CRUD ve sorgu işlemlerinde kullanabilirsiniz. Aşağıdaki örnek, NonePartitionKey'den tek bir Belgeyi okumak için bir örnek gösterir. 

```csharp
CosmosItemResponse<DeviceInformationItem> readResponse = 
await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
  partitionKey: PartitionKey.None, 
  id: device.Id
); 

```

Belgelerin nasıl yeniden bölümleyeceğime ilişkin tam örnek için [.Net örnekleri][1] GitHub deposuna bakın. 

## <a name="compatibility-with-sdks"></a>SDK'larla uyumluluk

V2.x.x ve V1.x.x gibi Azure Cosmos DB SDK'larının eski sürümü, sistem tanımlı bölüm anahtarı özelliğini desteklemez. Yani, eski bir SDK'dan kapsayıcı tanımını okuduğunuzda, herhangi bir bölüm anahtarı tanımı içermez ve bu kapsayıcılar tam olarak eskisi gibi davranacaktır. SDK'ların eski sürümüyle oluşturulmuş uygulamalar, herhangi bir değişiklik olmadan bölümlenmemiş olarak çalışmaya devam eder. 

Geçirilen bir kapsayıcı SDK'nın en son/V3 sürümü tarafından tüketilirse ve sistem tanımlı bölüm anahtarını yeni belgeler içinde doldurmaya başlarsanız, artık eski SDK'lardan bu tür belgelere erişemezsiniz (okuma, güncelleme, silme, sorgulayamazsınız).

## <a name="known-issues"></a>Bilinen sorunlar

**V3 SDK kullanılarak bölüm anahtarı olmadan eklenen öğelerin sayısının sorgulanması daha yüksek iş elde etme tüketimi içerebilir**

V2 SDK kullanılarak eklenen maddeler veya parametreli `PartitionKey.None` V3 SDK kullanılarak eklenen maddeler için V3 SDK'dan sorgu larsanız, `PartitionKey.None` parametre FeedOptions'da parametre sağlanıyorsa sayım sorgusu daha fazla RU/s tüketebilir. Başka bir öğe bir bölme `PartitionKey.None` anahtarı yla eklenmemişse, parametreyi sağlamamanızı öneririz.

Bölüm anahtarı için farklı değerlerle yeni öğeler eklenirse, uygun anahtarı geçerek `FeedOptions` bu tür öğe sayımları için sorgu herhangi bir sorun olmayacaktır. Bölüm tuşu ile yeni belgeler ekledikten sonra, yalnızca belge sayısını bölüm anahtarı değeri olmadan sorgulamanız gerekiyorsa, bu sorgu yine normal bölümlenmiş koleksiyonlara benzer daha yüksek RU/s'lere neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [Azure Cosmos DB'de İstek Birimleri](request-units.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](account-overview.md)

[1]: https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/NonPartitionContainerMigration