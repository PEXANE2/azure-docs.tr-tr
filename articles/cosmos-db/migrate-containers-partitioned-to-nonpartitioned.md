---
title: Bölümlenmemiş Azure Cosmos kapsayıcılarını bölümlenmiş kapsayıcılara geçirme
description: Var olan tüm bölümlenmemiş kapsayıcıları bölümlenmiş kapsayıcılara geçirmeyi öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: d51c200ebff0d92b1bcdf2c8e3e0325103e214b7
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69615032"
---
# <a name="migrate-non-partitioned-containers-to-partitioned-containers"></a>Bölümlendirilmemiş kapsayıcıları bölümlenmiş kapsayıcılara geçirme

Azure Cosmos DB, bölüm anahtarı olmayan kapsayıcılar oluşturmayı destekler. Şu anda, 2. x sürümüne eşit veya daha küçük bir sürüme sahip olan Azure CLı ve Azure Cosmos DB SDK 'Ları (.net, Java, NodeJs) kullanarak bölümlenmemiş kapsayıcılar oluşturabilirsiniz. Azure portal kullanarak bölümlenmemiş kapsayıcılar oluşturamazsınız. Ancak, bu tür bölümlenmemiş kapsayıcılar elastik değildir ve 10.000 RU/sn 'lik depolama kapasitesi olan 10 GB ve verimlilik limiti vardır.

Bölümlenmemiş kapsayıcılar eski ve depolama ve aktarım hızını ölçeklendirmek için mevcut bölümlenmemiş Kapsayıcılarınızı bölümlenmiş kapsayıcılara geçirmeniz gerekir. Azure Cosmos DB, bölümlenmemiş Kapsayıcılarınızı bölümlenmiş kapsayıcılara geçirmek için sistem tarafından tanımlanan bir mekanizma sağlar. Bu belgede, var olan tüm bölümlenmemiş kapsayıcıların bölümlenmiş kapsayıcılara otomatik olarak geçirilmesi açıklanmaktadır. Yalnızca tüm dillerde SDK 'ların v3 sürümünü kullanıyorsanız otomatik geçiş özelliğinden yararlanabilirsiniz.

> [!NOTE] 
> Şu anda, bu belgede açıklanan adımları kullanarak MongoDB ve Gremlin API hesaplarını Azure Cosmos DB geçiremezsiniz. 

## <a name="migrate-container-using-the-system-defined-partition-key"></a>Sistem tanımlı bölüm anahtarını kullanarak kapsayıcıyı geçirme

Azure Cosmos DB geçişi desteklemek için, bölüm anahtarı olmayan tüm kapsayıcılarda adlı `/_partitionkey` sistem tanımlı bir bölüm anahtarını tanımlar. Kapsayıcılar geçirildikten sonra bölüm anahtarı tanımını değiştiremezsiniz. Örneğin, bölümlenmiş bir kapsayıcıya geçirilmiş bir kapsayıcının tanımı aşağıdaki gibi olacaktır: 

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
 
Kapsayıcı geçirildikten sonra, `_partitionKey` özelliği belgenin diğer özellikleriyle birlikte doldurarak belgeler oluşturabilirsiniz. Özelliği `_partitionKey` , belgelerinizin bölüm anahtarını temsil eder. 

Doğru bölüm anahtarının seçilmesi, sağlanan aktarım hızını en iyi şekilde kullanmak için önemlidir. Daha fazla bilgi için bkz. [bölüm anahtarını seçme](partitioning-overview.md) makalesi. 

> [!NOTE]
> Yalnızca tüm dillerde SDK 'ların en son/v3 sürümünü kullanıyorsanız sistem tanımlı bölüm anahtarından yararlanabilirsiniz.

Aşağıdaki örnek, sistem tanımlı bölüm anahtarı ile bir belge oluşturmak için örnek bir kod gösterir ve belgeyi okur:

**Belgenin JSON temsili**

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

    [JsonProperty(PropertyName = "_partitionKey")]
    public string PartitionKey {get {return this.DeviceId; set; }
}

CosmosContainer migratedContainer = database.Containers["testContainer"];

DeviceInformationItem deviceItem = new DeviceInformationItem() {
  Id = "1234", 
  DeviceId = "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

CosmosItemResponse<DeviceInformationItem > response = 
  await migratedContainer.Items.CreateItemAsync(
    deviceItem.PartitionKey, 
    deviceItem
  );

// Read back the document providing the same partition key
CosmosItemResponse<DeviceInformationItem> readResponse = 
  await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
    partitionKey:deviceItem.PartitionKey, 
    id: device.Id
  ); 

```

Tüm örnek için bkz. [.NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) GitHub deposu. 
                      
## <a name="migrate-the-documents"></a>Belgeleri geçirme

Kapsayıcı tanımı bir bölüm anahtarı özelliği ile geliştirirken, kapsayıcıdaki belgeler otomatik olarak geçirilmez. Bu, sistem bölümü anahtar özelliği `/_partitionKey` yolunun varolan belgelere otomatik olarak eklenmeyeceği anlamına gelir. Bir bölüm anahtarı olmadan oluşturulmuş belgeleri okuyarak ve bunları belgelerdeki özelliği ile `_partitionKey` yeniden yazarak, varolan belgeleri yeniden bölümlemeniz gerekir. 

## <a name="access-documents-that-dont-have-a-partition-key"></a>Bölüm anahtarı olmayan belgelere erişin

Uygulamalar, "CosmosContainerSettings. NonePartitionKeyValue" adlı özel sistem özelliğini kullanarak bölüm anahtarı olmayan mevcut belgelere erişebilir, bu, geçirilmeyen belgelerin değeridir. Tüm CRUD ve sorgu işlemlerinde bu özelliği kullanabilirsiniz. Aşağıdaki örnek, NonePartitionKey öğesinden tek bir belgeyi okumak için bir örnek gösterir. 

```csharp
CosmosItemResponse<DeviceInformationItem> readResponse = 
await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
  partitionKey: CosmosContainerSettings.NonePartitionKeyValue, 
  id: device.Id
); 

```

Belgeleri yeniden bölümleme hakkında tüm örnek için bkz. [.net Samples](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) GitHub deposu. 

## <a name="compatibility-with-sdks"></a>SDK 'lar ile uyumluluk

V2. x. x ve v1. x. x gibi Azure Cosmos DB SDK 'ların daha eski sürümleri sistem tarafından tanımlanan bölüm anahtarı özelliğini desteklemez. Bu nedenle, eski bir SDK 'dan kapsayıcı tanımını okuduğunuzda, hiçbir bölüm anahtarı tanımı içermez ve bu kapsayıcılar tamamen daha önce olduğu gibi davranır. SDK 'ların eski sürümüyle oluşturulan uygulamalar hiçbir değişiklik yapılmadan bölümlenmemiş olarak çalışmaya devam eder. 

Geçirilen bir kapsayıcı SDK 'nın en son/v3 sürümü tarafından tüketilediyse ve sistem tanımlı bölüm anahtarını yeni belgeler içinde doldurmaya başlatırsanız, artık daha eski SDK 'lardan bu belgelere erişemezsiniz (okuma, güncelleştirme, silme, sorgulama).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [Azure Cosmos DB'de İstek birimleri](request-units.md)
* [Kapsayıcılar ve veritabanları üzerinde üretilen iş sağlama](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](account-overview.md)