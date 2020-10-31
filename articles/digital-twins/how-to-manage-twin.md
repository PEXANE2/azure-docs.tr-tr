---
title: Dijital ikizleri yönetme
titleSuffix: Azure Digital Twins
description: Bkz. ayrı ayrı TWINS ve ilişkileri alma, güncelleştirme ve silme.
author: baanders
ms.author: baanders
ms.date: 10/21/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 4962116b683d648f8f2d229b5113d2c8a01e15f8
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93131012"
---
# <a name="manage-digital-twins"></a>Dijital ikizleri yönetme

Ortamınızdaki varlıklar [dijital TWINS](concepts-twins-graph.md)tarafından temsil edilir. Dijital iklerinizi yönetmek oluşturma, değiştirme ve kaldırma işlemini içerebilir. Bu işlemleri yapmak için, [**Digitaltwins API 'lerini**](/rest/api/digital-twins/dataplane/twins), [.net (C#) SDK 'Sını](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet-preview&preserve-view=true)veya [Azure dijital TWINS CLI](how-to-use-cli.md)' yi kullanabilirsiniz.

Bu makale, dijital TWINS yönetimine odaklanır; ilişkiler ve [ikizi Graf](concepts-twins-graph.md) bir bütün olarak çalışmak için bkz. [*nasıl yapılır: ilişkiler Ile ikizi grafiğini yönetme*](how-to-manage-graph.md).

> [!TIP]
> Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

## <a name="create-a-digital-twin"></a>Dijital ikizi oluşturma

Bir ikizi oluşturmak için, `CreateDigitalTwin()` hizmet istemcisinde aşağıdaki gibi yöntemi kullanın:

```csharp
await client.CreateDigitalTwinAsync("myTwinId", initData);
```

Dijital bir ikizi oluşturmak için şunları sağlamanız gerekir:
* Dijital ikizi için istenen KIMLIK
* Kullanmak istediğiniz [model](concepts-models.md)

İsteğe bağlı olarak, dijital ikizi tüm özellikleri için başlangıç değerleri sağlayabilirsiniz. Özellikler isteğe bağlı olarak değerlendirilir ve daha sonra ayarlanabilir, ancak **ayarlanana kadar bir ikizi parçası olarak gösterilmez.**

>[!NOTE]
>İkizi özelliklerinin **başlatılması gerekmiyorsa, ikizi oluşturulduğunda ikizi 'in** tüm [bileşenlerinin](concepts-models.md#elements-of-a-model) ayarlanması gerekir. Bunlar boş nesneler olabilirler, ancak bileşenlerin kendisi mevcut olmalıdır.

Model ve herhangi bir başlangıç özelliği değeri `initData` , ilgili verileri içeren BIR JSON dizesi olan parametresi aracılığıyla sağlanır. Bu nesneyi yapılandırma hakkında daha fazla bilgi için sonraki bölüme geçin.

> [!TIP]
> Bir ikizi oluşturduktan veya güncelleştirdikten sonra değişiklikler [sorgularda](how-to-query-graph.md)yansıtılmadan önce 10 saniyeye kadar gecikme olabilir. `GetDigitalTwin`API ( [Bu makalede daha sonra](#get-data-for-a-digital-twin)açıklanan) Bu gecikmeyle karşılaşmaz, bu nedenle anlık bir yanıt gerekiyorsa, yeni oluşturduğunuz TWINS 'nizi görmek IÇIN sorgulamak yerine API çağrısını kullanın. 

### <a name="initialize-model-and-properties"></a>Modeli ve özellikleri Başlat

İkizi oluşturulduğu sırada bir ikizi özelliklerini başlatabilirsiniz. 

İkizi oluşturma API 'SI, ikizi özelliklerinin geçerli bir JSON açıklamasına serileştirilmiş bir nesne kabul eder. Bir ikizi için JSON biçiminin açıklaması için bkz. [*Kavramlar: dijital TWINS ve ikizi Graph*](concepts-twins-graph.md) . 

İlk olarak, ikizi ve özellik verilerini temsil eden bir veri nesnesi oluşturabilirsiniz. Daha sonra, bu `JsonSerializer` nesnenin serileştirilmiş bir sürümünü parametresi IÇIN API çağrısına geçirmek için kullanabilirsiniz `initdata` , örneğin:

```csharp
await client.CreateDigitalTwinAsync(srcId, JsonSerializer.Serialize<BasicDigitalTwin>(twin));
```
Bir parametre nesnesini el ile ya da bir belirtilen yardımcı sınıfı kullanarak oluşturabilirsiniz. Her birine bir örnek aşağıda verilmiştir.

#### <a name="create-twins-using-manually-created-data"></a>El ile oluşturulan verileri kullanarak TWINS oluşturma

Herhangi bir özel yardımcı sınıf kullanmadan, bir ikizi özelliklerini temsil edebilirsiniz `Dictionary<string, object>` ; burada, `string` özelliğin adı, `object` özelliği ve değerini temsil eden bir nesnedir.

[!INCLUDE [Azure Digital Twins code: create twin](../../includes/digital-twins-code-create-twin.md)]

#### <a name="create-twins-with-the-helper-class"></a>Yardımcı sınıfla TWINS oluşturma

Yardımcı sınıfı, `BasicDigitalTwin` Özellik alanlarını doğrudan bir "ikizi" nesnesinde depolamanıza olanak tanır. `Dictionary<string, object>`Daha sonra doğrudan ikizi nesnesine eklenebilen bir kullanarak özellik listesini oluşturmak isteyebilirsiniz `CustomProperties` .

```csharp
BasicDigitalTwin twin = new BasicDigitalTwin();
twin.Metadata = new DigitalTwinMetadata();
twin.Metadata.ModelId = "dtmi:example:Room;1";
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("Temperature", 25.0);
props.Add("Humidity", 50.0);
twin.CustomProperties = props;

client.CreateDigitalTwinAsync("myRoomId", JsonSerializer.Serialize<BasicDigitalTwin>(twin));
Console.WriteLine("The twin is created successfully");
```

>[!NOTE]
> `BasicDigitalTwin` nesneler bir alanla gelir `Id` . Bu alanı boş bırakabilirsiniz, ancak bir KIMLIK değeri eklerseniz, çağrıya geçirilen ID parametresiyle eşleşmesi gerekir `CreateDigitalTwin()` . Örneğin:
>
>```csharp
>twin.Id = "myRoomId";
>```

## <a name="get-data-for-a-digital-twin"></a>Dijital ikizi için veri al

Aşağıdaki gibi bir yöntemi çağırarak herhangi bir dijital ikizi ayrıntılarına erişebilirsiniz `GetDigitalTwin()` :

```csharp
object result = await client.GetDigitalTwin(id);
```
Bu çağrı, ikizi verilerini JSON dizesi olarak döndürür. Bu, ikizi ayrıntılarını görüntülemek için nasıl kullanılacağına ilişkin bir örnek aşağıda verilmiştir:

```csharp
Response<string> res = client.GetDigitalTwin("myRoomId");
twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
foreach (string prop in twin.CustomProperties.Keys)
{
  if (twin.CustomProperties.TryGetValue(prop, out object value))
  Console.WriteLine($"Property '{prop}': {value}");
}
```
Yöntemi ile bir ikizi aldığınızda, yalnızca en az bir kez ayarlanmış olan özellikler döndürülür `GetDigitalTwin()` .

>[!TIP]
>`displayName`Bir ikizi için, model meta verilerinin bir parçası olduğundan, ikizi örneği için veri alınırken gösterilmez. Bu değeri görmek için [modelden geri](how-to-manage-model.md#retrieve-models)alabilirsiniz.

Tek bir API çağrısını kullanarak birden çok TWINS almak için, sorgu API 'SI örneklerine bkz. [*nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md).

Bir *ay* tanımlayan aşağıdaki modeli ( [dijital TWINS tanım dili (dtdl)](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL)olarak yazılmış) göz önünde bulundurun:

```json
{
    "@id": "dtmi:example:Moon;1",
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
`object result = await client.GetDigitalTwinAsync("my-moon");`Bir *ay* türü ikizi çağırma sonucu şöyle görünebilir:

```json
{
  "$dtId": "myMoon-001",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "radius": 1737.1,
  "mass": 0.0734,
  "$metadata": {
    "$model": "dtmi:example:Moon;1",
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
* `$etag`, Web sunucusu tarafından atanan standart bir HTTP alanı.
* Bir bölümdeki diğer özellikler `$metadata` . Bunlar:
    - Dijital ikizi modelinin DTMı 'ı.
    - Her yazılabilir özellik için eşitleme durumu. Bu, hizmetin ve cihazın ayrılan durumlar (örneğin, bir cihaz çevrimdışı olduğunda) olduğu durumlarda, cihazlar için en yararlı seçenektir. Şu anda bu özellik yalnızca IoT Hub bağlı fiziksel cihazlara uygulanır. Meta veriler bölümündeki verilerle, bir özelliğin tam durumunun yanı sıra son değiştirilme zaman damgalarını anlamak mümkündür. Eşitleme durumu hakkında daha fazla bilgi için bkz. cihaz durumunu eşitlemeye yönelik [bu IoT Hub öğreticisi](../iot-hub/tutorial-device-twins.md) .
    - IoT Hub veya Azure dijital TWINS gibi hizmete özgü meta veriler. 

İkizi için döndürülen JSON 'ı, tercih ettiğiniz bir JSON ayrıştırma kitaplığı kullanarak ayrıştırtırabilirsiniz `System.Text.Json` .

Ayrıca, SDK 'da bulunan serileştirme Yardımcısı sınıfını da kullanabilirsiniz `BasicDigitalTwin` ; Bu, önceden ayrıştırılmış form içindeki Core ikizi meta verilerini ve özelliklerini döndürür. Aşağıda bir örnek verilmiştir:

```csharp
Response<string> res = client.GetDigitalTwin(twin_Id);
BasicDigitalTwin twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
foreach (string prop in twin.CustomProperties.Keys)
{
    if (twin.CustomProperties.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

Serileştirme yardımcı sınıfları hakkında daha fazla bilgi [*Için bkz. nasıl yapılır: Azure dijital TWINS API 'leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md).

## <a name="update-a-digital-twin"></a>Dijital ikizi güncelleştirme

Dijital bir ikizi özelliklerini güncelleştirmek için, değiştirmek istediğiniz bilgileri [JSON yama](http://jsonpatch.com/) biçiminde yazarsınız. Bu şekilde, birden çok özelliği aynı anda değiştirebilirsiniz. Ardından JSON Patch belgesini bir `UpdateDigitalTwin()` yönteme geçirirsiniz:

```csharp
await client.UpdateDigitalTwin(id, patch);
```

Bir yama çağrısı, tek bir ikizi üzerinde dilediğiniz kadar özelliği güncelleştirebilir (hatta bunların hepsi de). Birden çok TWINS genelinde özellikleri güncelleştirmeniz gerekiyorsa, her bir ikizi için ayrı bir güncelleştirme çağrısının olması gerekir.

> [!TIP]
> Bir ikizi oluşturduktan veya güncelleştirdikten sonra değişiklikler [sorgularda](how-to-query-graph.md)yansıtılmadan önce 10 saniyeye kadar gecikme olabilir. `GetDigitalTwin`API ( [Bu makalede daha önce](#get-data-for-a-digital-twin)açıklanan) Bu gecikmeyle karşılaşmaz, bu nedenle anlık bir yanıt gerekirse yeni güncellenen TWINS 'nizi görmek IÇIN sorgulamak yerine API çağrısını kullanın. 

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

await client.UpdateDigitalTwinAsync(twin_Id, JsonSerializer.Serialize(twinData));
Console.WriteLine("Updated twin properties");
FetchAndPrintTwin(twin_Id, client);
}
```

#### <a name="create-patches-using-the-helper-class"></a>Yardımcı sınıfını kullanarak yama oluşturma

```csharp
UpdateOperationsUtility uou = new UpdateOperationsUtility();
uou.AppendAddOp("/Temperature", 25.0);
await client.UpdateDigitalTwinAsync(twin_Id, uou.Serialize());
```

### <a name="update-properties-in-digital-twin-components"></a>Dijital ikizi bileşenlerinde güncelleştirme özellikleri

Bir modelin bileşen içerebileceğini ve diğer modellerden sonra yapılmasına izin vermesini geri çekin. 

Bir Digital ikizi bileşenlerindeki özellikler için düzeltme eki uygulamak üzere, JSON yaması içinde yol söz dizimini kullanabilirsiniz:

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

`UpdateDigitalTwin()`İşlevi, dijital bir ikizi farklı bir modele geçirmek için de kullanılabilir. 

Örneğin, Digital ikizi 'ın meta veri alanının yerini alan aşağıdaki JSON Patch belgesini göz önünde bulundurun `$model` :

```json
[
  {
    "op": "replace",
    "path": "/$metadata/$model",
    "value": "dtmi:example:foo;1"
  }
]
```

Bu işlem yalnızca düzeltme eki tarafından değiştirilen dijital ikizi yeni modelle uyumlu olduğunda başarılı olur. 

Aşağıdaki örneği inceleyin:
1. *Foo_old* modeliyle dijital bir ikizi düşünün. *foo_old* gerekli bir özellik *kütle* tanımlar.
2. Yeni model *foo_new* bir özellik kütle tanımlar ve yeni bir gerekli özellik *sıcaklığını* ekler.
3. Düzeltme ekiyle sonra, dijital ikizi hem bir kütle hem de sıcaklık özelliği olmalıdır. 

Bu durumun düzeltme ekinin hem model hem de ikizi 'ın sıcaklık özelliğini güncelleştirmesi gerekir, örneğin:

```json
[
  {
    "op": "replace",
    "path": "$metadata.$model",
    "value": "dtmi:example:foo_new"
  },
  {
    "op": "add",
    "path": "temperature",
    "value": 60
  }
]
```

### <a name="handle-conflicting-update-calls"></a>Çakışan güncelleştirme çağrılarını işle

Azure dijital TWINS, tüm gelen isteklerin diğer bir tarihten sonra işlenmesini sağlar. Bu, birden çok işlev aynı anda bir ikizi üzerinde aynı özelliği güncelleştirmeye çalışırsa bile, çakışmayı işlemek için açık kilitleme kodu yazmanıza **gerek yoktur** .

Bu davranış, ikizi esasına göre yapılır. 

Örnek olarak, bu üç çağrının aynı anda ulaştığını gösteren bir senaryo düşünün: 
*   *Twin1* üzerinde yazma özelliği
*   *Twin1* üzerinde B özelliği yazma
*   *Twin2* üzerinde yazma özelliği

*Twin1* değiştiren iki çağrı diğerinden sonra yürütülür ve değişiklik iletileri her değişiklik için oluşturulur. *Twin2* değiştirme çağrısı, bir çakışma olmadan eşzamanlı olarak yürütülemeyebilir.

## <a name="delete-a-digital-twin"></a>Dijital ikizi silme

Yöntemi kullanarak ikizlerini 'yi silebilirsiniz `DeleteDigitalTwin()` . Ancak, daha fazla ilişki olmadığında yalnızca bir ikizi silebilirsiniz. Bu nedenle, önce ikizi 'in gelen ve giden ilişkilerini silin.

Aşağıda, TWINS ve bunların ilişkilerini silmenin bir kodu örneği verilmiştir:

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
        Log.Error($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving or deleting relationships for {dtId} due to {ex.Message}");
    }
}

async Task FindAndDeleteIncomingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin

    try
    {
        // GetRelationshipsAsync will throw an error if a problem occurs
        AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

        await foreach (IncomingRelationship incomingRel in incomingRels)
        {
            await client.DeleteRelationshipAsync(incomingRel.SourceId, incomingRel.RelationshipId).ConfigureAwait(false);
            Log.Ok($"Deleted incoming relationship {incomingRel.RelationshipId} from {dtId}");
        }
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"_*_ Error {ex.Status}/{ex.ErrorCode} retrieving or deleting incoming relationships for {dtId} due to {ex.Message}");
    }
}
```
### <a name="delete-all-digital-twins"></a>Tüm dijital TWINS 'i Sil

Tüm TWINS sürümlerini aynı anda silme hakkında bir örnek için, _Tutorial kullanılan örnek uygulamayı indirin [: örnek bir istemci uygulamasıyla ilgili temel bilgileri keşfet *](tutorial-command-line-app.md). *CommandLoop.cs* dosyası bunu bir `CommandDeleteAllTwins()` işlevde yapar.

## <a name="manage-twins-using-runnable-code-sample"></a>Runzorla kod örneğini kullanarak TWINS 'i yönetme

Bir ikizi oluşturmak, ayrıntılarını güncelleştirmek ve ikizi silmek için aşağıdaki çalıştırılabilir kod örneğini kullanabilirsiniz. 

### <a name="set-up-the-runnable-sample"></a>Çalıştırılabilir örneği ayarlama

Kod parçacığı öğreticiden model tanımındaki [Room.js](https://github.com/Azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Room.json) kullanır [*: örnek bir Istemci uygulamasıyla Azure dijital TWINS 'i araştırma*](tutorial-command-line-app.md). Bu bağlantıyı doğrudan dosyaya gitmek veya tam uçtan uca [örnek projenin bir](/samples/azure-samples/digital-twins-samples/digital-twins-samples/)parçası olarak indirmek için kullanabilirsiniz.

Örneği çalıştırmadan önce aşağıdakileri yapın:
1. Model dosyasını indirin, projenize yerleştirin ve `<path-to>` aşağıdaki koddaki yer tutucuyu değiştirip, programınıza onu nerede bulacağınızı söyleyin.
2. Yer tutucusunu `<your-instance-hostname>` Azure dijital TWINS örneğinizin ana bilgisayar adı ile değiştirin.
3. Bu paketleri projenize ekleyin:
    ```cmd/sh
    dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
    dotnet add package Azure.identity
    ```

Ayrıca, örneği doğrudan çalıştırmak istiyorsanız yerel kimlik bilgilerini de ayarlamanız gerekir. Sonraki bölümde bu adım gösterilmektedir.
[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

### <a name="run-the-sample"></a>Örneği çalıştırma

Yukarıdaki adımları tamamladıktan sonra, aşağıdaki örnek kodu doğrudan çalıştırabilirsiniz.

```csharp
using System;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
using Azure.DigitalTwins.Core.Serialization;
using System.Text.Json;

namespace minimal
{
    class Program
    {

        public static async Task Main(string[] args)
        {
            Console.WriteLine("Hello World!");

            //Create the Azure Digital Twins client for API calls
            string adtInstanceUrl = "https://<your-instance-hostname>";
            var credentials = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
            Console.WriteLine($"Service client created – ready to go");
            Console.WriteLine();

            //Upload models
            Console.WriteLine($"Upload a model");
            Console.WriteLine();
            string dtdl = File.ReadAllText("<path-to>/Room.json");
            var typeList = new List<string>();
            typeList.Add(dtdl);
            // Upload the model to the service
            await client.CreateModelsAsync(typeList);

            //Create new digital twin
            BasicDigitalTwin twin = new BasicDigitalTwin();
            string twin_Id = "myRoomId";
            twin.Metadata = new DigitalTwinMetadata();
            twin.Metadata.ModelId = "dtmi:example:Room;1";
            // Initialize properties
            Dictionary<string, object> props = new Dictionary<string, object>();
            props.Add("Temperature", 35.0);
            props.Add("Humidity", 55.0);
            twin.CustomProperties = props;
            await client.CreateDigitalTwinAsync(twin_Id, JsonSerializer.Serialize<BasicDigitalTwin>(twin));
            Console.WriteLine("Twin created successfully");
            Console.WriteLine();

            //Print twin
            Console.WriteLine("--- Printing twin details:");
            twin = FetchAndPrintTwin(twin_Id, client);
            Console.WriteLine("--------");
            Console.WriteLine();

            //Update twin data
            List<object> twinData = new List<object>();
            twinData.Add(new Dictionary<string, object>() 
            {
                { "op", "add"},
                { "path", "/Temperature"},
                { "value", 25.0}
            });
            await client.UpdateDigitalTwinAsync(twin_Id, JsonSerializer.Serialize(twinData));
            Console.WriteLine("Twin properties updated");
            Console.WriteLine();

            //Print twin again
            Console.WriteLine("--- Printing twin details (after update):");
            FetchAndPrintTwin(twin_Id, client);
            Console.WriteLine("--------");
            Console.WriteLine();

            //Delete twin
            await DeleteTwin(client, twin_Id);
        }

        private static BasicDigitalTwin FetchAndPrintTwin(string twin_Id, DigitalTwinsClient client)
        {
            BasicDigitalTwin twin;
            Response<string> res = client.GetDigitalTwin(twin_Id);
            twin = JsonSerializer.Deserialize<BasicDigitalTwin>(res.Value);
            Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
            foreach (string prop in twin.CustomProperties.Keys)
            {
                if (twin.CustomProperties.TryGetValue(prop, out object value))
                    Console.WriteLine($"Property '{prop}': {value}");
            }

            return twin;
        }
        private static async Task DeleteTwin(DigitalTwinsClient client, string id)
        {
            await FindAndDeleteOutgoingRelationshipsAsync(client, id);
            await FindAndDeleteIncomingRelationshipsAsync(client, id);
            try
            {
                await client.DeleteDigitalTwinAsync(id);
                Console.WriteLine("Twin deleted successfully");
            }
            catch (RequestFailedException exc)
            {
                Console.WriteLine($"*** Error:{exc.Message}");
            }
        }

        private static async Task FindAndDeleteOutgoingRelationshipsAsync(DigitalTwinsClient client, string dtId)
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
                    Console.WriteLine($"Deleted relationship {rel.Id} from {dtId}");
                }
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving or deleting relationships for {dtId} due to {ex.Message}");
            }
        }

       private static async Task FindAndDeleteIncomingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin

            try
            {
                // GetRelationshipsAsync will throw an error if a problem occurs
                AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

                await foreach (IncomingRelationship incomingRel in incomingRels)
                {
                    await client.DeleteRelationshipAsync(incomingRel.SourceId, incomingRel.RelationshipId).ConfigureAwait(false);
                    Console.WriteLine($"Deleted incoming relationship {incomingRel.RelationshipId} from {dtId}");
                }
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"_*_ Error {ex.Status}/{ex.ErrorCode} retrieving or deleting incoming relationships for {dtId} due to {ex.Message}");
            }
        }

    }
}

```
Yukarıdaki programın konsol çıktısı aşağıda verilmiştir: 

:::image type="content" source="./media/how-to-manage-twin/console-output-manage-twins.png" alt-text="İkizi oluşturulduğunu, güncelleştirildiğini ve silindiğini gösteren konsol çıkışı" lightbox="./media/how-to-manage-twin/console-output-manage-twins.png":::

## <a name="manage-twins-with-cli"></a>CLı ile TWINS 'i yönetme

TWINS, Azure Digital TWINS CLı kullanılarak da yönetilebilir. Komutların [_How-to: Azure Digital TWINS CLI * kullanma](how-to-use-cli.md)bölümünde bulabilirsiniz.

## <a name="view-all-digital-twins"></a>Tüm dijital TWINS 'i görüntüle

Örneğinizdeki dijital TWINS 'in tamamını görüntülemek için bir [sorgu](how-to-query-graph.md)kullanın. Sorgu [API 'leri](/rest/api/digital-twins/dataplane/query) veya [CLI komutlarıyla](how-to-use-cli.md)bir sorgu çalıştırabilirsiniz.

Örnekteki tüm dijital TWINS listesini döndürecek temel sorgunun gövdesi aşağıda verilmiştir:

```sql
SELECT *
FROM DIGITALTWINS
``` 

## <a name="next-steps"></a>Sonraki adımlar

Bkz. dijital TWINS 'niz arasında ilişkiler oluşturma ve yönetme:
* [*Nasıl yapılır: ikizi grafiğini ilişkilerle yönetme*](how-to-manage-graph.md)