---
title: Dijital ikizleri yönetme
titleSuffix: Azure Digital Twins
description: Bkz. ayrı ayrı TWINS ve ilişkileri alma, güncelleştirme ve silme.
author: baanders
ms.author: baanders
ms.date: 4/10/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 707cfb2e9bea3286daa92ea54f7bb9659a455caf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390525"
---
# <a name="manage-digital-twins"></a>Dijital ikizleri yönetme

Ortamınızdaki varlıklar [dijital TWINS](concepts-twins-graph.md)tarafından temsil edilir. Dijital iklerinizi yönetmek oluşturma, değiştirme ve kaldırma işlemini içerebilir. Bu işlemleri yapmak için, [**Digitaltwins API 'lerini**](how-to-use-apis-sdks.md), [.net (C#) SDK 'Sını](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)veya [Azure dijital TWINS CLI](how-to-use-cli.md)' yi kullanabilirsiniz.

Bu makale, dijital TWINS yönetimine odaklanır; ilişkiler ve [ikizi Graf](concepts-twins-graph.md) bir bütün olarak çalışmak için bkz. [nasıl yapılır: ilişkiler Ile ikizi grafiğini yönetme](how-to-manage-graph.md).

> [!TIP]
> Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

## <a name="create-a-digital-twin"></a>Dijital ikizi oluşturma

Bir ikizi oluşturmak için, `CreateDigitalTwin` hizmet istemcisinde aşağıdaki gibi yöntemi kullanın:

```csharp
await client.CreateDigitalTwinAsync("myNewTwinID", initData);
```

Dijital bir ikizi oluşturmak için şunları sağlamanız gerekir:
* Dijital ikizi için istenen KIMLIK
* Kullanmak istediğiniz [model](concepts-models.md) 

İsteğe bağlı olarak, dijital ikizi tüm özellikleri için başlangıç değerleri sağlayabilirsiniz. 

> [!TIP]
> GetDigitalTwin ile ikizi aldığınızda yalnızca en az bir kez ayarlanan özellikler döndürülür.  

Model ve ilk özellik değerleri `initData` , ilgili verileri içeren BIR JSON dizesi olan parametresi aracılığıyla sağlanır.

### <a name="initialize-properties"></a>Özellikleri Başlat

İkizi oluşturma API 'SI, ikizi özelliklerinin geçerli bir JSON açıklamasında seri hale getirilebilir bir nesne kabul eder. Bir ikizi için JSON biçiminin açıklaması için bkz. [Kavramlar: dijital TWINS ve ikizi Graph](concepts-twins-graph.md) .

Bir parametre nesnesini el ile ya da bir belirtilen yardımcı sınıfı kullanarak oluşturabilirsiniz. Her birine bir örnek aşağıda verilmiştir.

#### <a name="create-twins-using-manually-created-data"></a>El ile oluşturulan verileri kullanarak TWINS oluşturma

```csharp
// Define the model type for the twin to be created
Dictionary<string, object> meta = new Dictionary<string, object>()
{
    { "$model", "dtmi:com:contoso:Room;1" }
};
// Initialize the twin properties
Dictionary<string, object> twin = new Dictionary<string, object>()
{
    { "$metadata", meta },
    { "Temperature", temperature},
    { "Humidity", humidity},
};
client.CreateDigitalTwin("myNewRoomID", JsonSerializer.Serialize<Dictionary<string, object>>(twin));
```

#### <a name="create-twins-with-the-helper-class"></a>Yardımcı sınıfla TWINS oluşturma

```csharp
BasicDigitalTwin twin = new BasicDigitalTwin();
twin.Metadata = new DigitalTwinMetadata();
twin.Metadata.ModelId = "dtmi:example:Room;1";
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("Temperature", 25.0);
props.Add("Humidity", 50.0);
twin.CustomProperties = props;

client.CreateDigitalTwin("myNewRoomID", JsonSerializer.Serialize<BasicDigitalTwin>(twin));
```

## <a name="get-data-for-a-digital-twin"></a>Dijital ikizi için veri al

Herhangi bir dijital ikizi tam verilerine şunu çağırarak erişebilirsiniz:

```csharp
object result = await client.GetDigitalTwin(id);
```

Bu çağrı, ikizi verilerini JSON dizesi olarak döndürür. 

Tek bir API çağrısını kullanarak birden çok TWINS almak için, sorgu API 'SI örneklerine bkz. [nasıl yapılır: ikizi grafiğini sorgulama](how-to-query-graph.md).

Bir *ay*tanımlayan aşağıdaki modeli ( [dijital TWINS tanım dili (dtdl)](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL)olarak yazılmış) göz önünde bulundurun:

```json
{
    "@id": " dtmi:com:contoso:Moon;1",
    "@type": "Interface",
    "@context": "dtmi:dtdl:context;2",
    "contents": [
        {
            "@type": "Property",
            "name": "radius",
            "schema": "double",
            "writable": true
        },
        {
            "@type": "Property",
            "name": "mass",
            "schema": "double",
            "writable": true
        }
    ]
}
```

`object result = await client.DigitalTwins.GetByIdAsync("my-moon");`Bir *ay*türü ikizi çağırma sonucu şöyle görünebilir:

```json
{
  "$dtId": "myMoon-001",
  "radius": 1737.1,
  "mass": 0.0734,
  "$metadata": {
    "$model": "dtmi:com:contoso:Moon;1",
    "radius": {
      "desiredValue": 1737.1,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "mass": {
      "desiredValue": 0.0734,
      "desiredVersion": 8,
      "ackVersion": 8,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

Dijital ikizi tanımlı özellikleri, Digital ikizi üzerinde en üst düzey özellikler olarak döndürülür. DTDL tanımının parçası olmayan meta veriler veya sistem bilgileri bir `$` ön ek ile döndürülür. Meta veri özellikleri şunları içerir:
* Bu Azure dijital TWINS örneğindeki dijital ikizi KIMLIĞI (as) `$dtId` .
* Bir bölümdeki diğer özellikler `$metadata` . Buna aşağıdakiler dahildir:
    - Dijital ikizi modelinin DTMı 'ı.
    - Her yazılabilir özellik için eşitleme durumu. Bu, hizmetin ve cihazın ayrılan durumlar (örneğin, bir cihaz çevrimdışı olduğunda) olduğu durumlarda, cihazlar için en yararlı seçenektir. Şu anda bu özellik yalnızca IoT Hub bağlı fiziksel cihazlara uygulanır. Meta veriler bölümündeki verilerle, bir özelliğin tam durumunun yanı sıra son değiştirilme zaman damgalarını anlamak mümkündür. Eşitleme durumu hakkında daha fazla bilgi için bkz. cihaz durumunu eşitlemeye yönelik [bu IoT Hub öğreticisi](../iot-hub/tutorial-device-twins.md) .
    - IoT Hub veya Azure dijital TWINS gibi hizmete özgü meta veriler. 

İkizi için döndürülen JSON 'ı, tercih ettiğiniz bir JSON ayrıştırma kitaplığı kullanarak ayrıştırtırabilirsiniz `System.Text.Json` .

Ayrıca, SDK 'da bulunan serileştirme Yardımcısı sınıfını da kullanabilirsiniz `BasicDigitalTwin` ; Bu, önceden ayrıştırılmış form içindeki Core ikizi meta verilerini ve özelliklerini döndürür. Örnek aşağıda verilmiştir:

```csharp
Response<string> res = client.GetDigitalTwin(twin_id);
BasicDigitalTwin twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
foreach (string prop in twin.CustomProperties.Keys)
{
    if (twin.CustomProperties.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

Serileştirme yardımcı sınıfları hakkında daha fazla bilgi [Için bkz. nasıl yapılır: Azure dijital TWINS API 'leri ve SDK 'Larını kullanma](how-to-use-apis-sdks.md).

## <a name="update-a-digital-twin"></a>Dijital ikizi güncelleştirme

Bir Digital ikizi özelliklerini güncelleştirmek için, değiştirmek istediğiniz bilgileri [JSON yama](http://jsonpatch.com/) biçiminde yazarsınız. Bu şekilde, birden çok özelliği aynı anda değiştirebilirsiniz. Ardından JSON Patch belgesini bir `Update` yönteme geçirirsiniz:

`await client.UpdateDigitalTwin(id, patch);`.

JSON yama kodu örneği aşağıda verilmiştir. Bu belge, uygulandığı dijital ikizi 'in *kütle* ve *Radius* özellik değerlerini değiştirir.

```json
[
  {
    "op": "replace",
    "path": "/mass",
    "value": 0.0799
  },
  {
    "op": "replace",
    "path": "/radius",
    "value": 0.800
  }
]
```

Düzeltme eklerini el ile veya [SDK](how-to-use-apis-sdks.md)'daki bir serileştirme Yardımcısı sınıfını kullanarak oluşturabilirsiniz. Her birine bir örnek aşağıda verilmiştir.

#### <a name="create-patches-manually"></a>Düzeltme eklerini el ile oluşturma
```csharp
List<object> twinData = new List<object>();
twinData.Add(new Dictionary<string, object>() {
    { "op", "add"},
    { "path", "/Temperature"},
    { "value", 25.0}
});

await client.UpdateDigitalTwinAsync(twinId, JsonConvert.SerializeObject(twinData));
```

#### <a name="create-patches-using-the-helper-class"></a>Yardımcı sınıfını kullanarak yama oluşturma

```csharp
UpdateOperationsUtility uou = new UpdateOperationsUtility();
uou.AppendAddOp("/Temperature", 25.0);
await client.UpdateDigitalTwinAsync(twinId, uou.Serialize());
```

### <a name="update-properties-in-digital-twin-components"></a>Dijital ikizi bileşenlerinde güncelleştirme özellikleri

Bir modelin bileşen içerebileceğini ve diğer modellerden sonra yapılmasına izin vermesini geri çekin. 

Bir Digital ikizi bileşenlerindeki özellikleri yama yapmak için JSON yaması içinde yol söz dizimini kullanacaksınız:

```json
[
  {
    "op": "replace",
    "path": "/mycomponentname/mass",
    "value": 0.0799
  }
]
```

### <a name="update-a-digital-twins-model"></a>Digital ikizi 'in modelini güncelleştirme

`Update`İşlevi, dijital bir ikizi farklı bir modele geçirmek için de kullanılabilir. 

Örneğin, Digital ikizi 'ın meta veri alanının yerini alan aşağıdaki JSON Patch belgesini göz önünde bulundurun `$model` :

```json
[
  {
    "op": "replace",
    "path": "/$metadata/$model",
    "value": "dtmi:com:contoso:foo;1"
  }
]
```

Bu işlem yalnızca düzeltme eki tarafından değiştirilen dijital ikizi yeni modelle uyumlu olduğunda başarılı olur. 

Aşağıdaki örneği inceleyin:
1. *Foo_old*modeliyle dijital bir ikizi düşünün. *foo_old* gerekli bir özellik *kütle*tanımlar.
2. Yeni model *foo_new* bir özellik kütle tanımlar ve yeni bir gerekli özellik *sıcaklığını*ekler.
3. Düzeltme ekiyle sonra, dijital ikizi hem bir kütle hem de sıcaklık özelliği olmalıdır. 

Bu durumun düzeltme ekinin hem model hem de ikizi 'ın sıcaklık özelliğini güncelleştirmesi gerekir, örneğin:

```json
[
  {
    "op": "replace",
    "path": "$metadata.$model",
    "value": "dtmi:com:contoso:foo_new"
  },
  {
    "op": "add",
    "path": "temperature",
    "value": 60
  }
]
```

## <a name="delete-a-digital-twin"></a>Dijital ikizi silme

Kullanarak TWINS 'yi silebilirsiniz `DeleteDigitalTwin(ID)` . Ancak, daha fazla ilişki olmadığında yalnızca bir ikizi silebilirsiniz. Önce tüm ilişkileri silmeniz gerekir. 

Bunun için kodun bir örneği aşağıda verilmiştir:

```csharp
static async Task DeleteTwin(string id)
{
    await FindAndDeleteOutgoingRelationshipsAsync(id);
    await FindAndDeleteIncomingRelationshipsAsync(id);
    try
    {
        await client.DeleteDigitalTwin(id);
    } catch (RequestFailedException exc)
    {
        Console.WriteLine($"*** Error:{exc.Error}/{exc.Message}");
    }
}

public async Task FindAndDeleteOutgoingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin

    try
    {
        // GetRelationshipsAsync will throw an error if a problem occurs
        AsyncPageable<string> relsJson = client.GetRelationshipsAsync(dtId);

        await foreach (string relJson in relsJson)
        {
            var rel = System.Text.Json.JsonSerializer.Deserialize<BasicRelationship>(relJson);
            await client.DeleteRelationshipAsync(dtId, rel.Id).ConfigureAwait(false);
            Log.Ok($"Deleted relationship {rel.Id} from {dtId}");
        }
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving or deleting relationships for {dtId} due to {ex.Message}");
    }
}

async Task FindAndDeleteIncomingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin

    try
    {
        // GetRelationshipssAsync will throw an error if a problem occurs
        AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

        await foreach (IncomingRelationship incomingRel in incomingRels)
        {
            await client.DeleteRelationshipAsync(incomingRel.SourceId, incomingRel.RelationshipId).ConfigureAwait(false);
            Log.Ok($"Deleted incoming relationship {incomingRel.RelationshipId} from {dtId}");
        }
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving or deleting incoming relationships for {dtId} due to {ex.Message}");
    }
}
```

### <a name="delete-all-digital-twins"></a>Tüm dijital TWINS 'i Sil

Tüm TWINS sürümlerini aynı anda silme hakkında bir örnek için öğreticide kullanılan örnek uygulamayı indirin [: örnek bir istemci uygulamasıyla ilgili temel bilgileri araştırma](tutorial-command-line-app.md). *CommandLoop.cs* dosyası bunu bir `CommandDeleteAllTwins` işlevde yapar.

## <a name="manage-twins-with-cli"></a>CLı ile TWINS 'i yönetme

TWINS, Azure Digital TWINS CLı kullanılarak da yönetilebilir. Komutları [nasıl yapılır: Azure dijital TWINS CLI 'Sını kullanma](how-to-use-cli.md)bölümünde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. dijital TWINS 'niz arasında ilişkiler oluşturma ve yönetme:
* [Nasıl yapılır: ikizi grafiğini ilişkilerle yönetme](how-to-manage-graph.md)