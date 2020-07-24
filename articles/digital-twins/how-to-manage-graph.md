---
title: İlişkileri olan ikiz grafiğini yönetme
titleSuffix: Azure Digital Twins
description: Bkz. bir dijital TWINS grafiğini ilişkiler ile bağlayarak yönetme.
author: baanders
ms.author: baanders
ms.date: 4/10/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 7f7239e0c13478af712d8e8d9dad8fda23fe42c7
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87125541"
---
# <a name="manage-a-graph-of-digital-twins-using-relationships"></a>İlişkileri kullanarak dijital TWINS grafiğini yönetme

Azure dijital TWINS 'in kalbi, tüm ortamınızı temsil eden [ikizi grafiktir](concepts-twins-graph.md) . İkizi Graph, **ilişkiler**aracılığıyla bağlanan tek tek dijital TWINS 'ten oluşur.

Çalışan bir [Azure dijital TWINS örneğinden](how-to-set-up-instance-scripted.md) ve istemci uygulamanızda [kimlik doğrulama](how-to-authenticate-client.md) kodu ayarladıktan sonra, Azure dijital TWINS örneğindeki dijital TWINS ve bunların ilişkilerini oluşturmak, değiştirmek ve silmek Için [**digitaltwins API 'lerini**](how-to-use-apis-sdks.md) kullanabilirsiniz. [.Net (C#) SDK 'sını](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)veya [Azure DIGITAL TWINS CLI](how-to-use-cli.md)'yi de kullanabilirsiniz.

Bu makale, ilişkilerin ve grafiğin tamamını yönetmeye odaklanır. tek tek dijital TWINS ile çalışmak için bkz. [*nasıl yapılır: dijital TWINS 'ı yönetme*](how-to-manage-twin.md).

[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

## <a name="create-relationships"></a>İlişki oluştur

İlişkiler, ikizi grafiğinin temelini oluşturan farklı dijital TWINS 'in birbirlerine nasıl bağlandığını tanımlar.

İlişkiler, çağrı kullanılarak oluşturulur `CreateRelationship` . 

Bir ilişki oluşturmak için şunu belirtmeniz gerekir:
* Kaynak ikizi KIMLIĞI (ilişkinin kaynaklandığı ikizi)
* Target ikizi ID (ilişkinin ulaştığı ikizi)
* İlişki adı
* İlişki KIMLIĞI

İlişki KIMLIĞI, belirtilen kaynak ikizi içinde benzersiz olmalıdır. Genel olarak benzersiz olması gerekmez.
Örneğin, ikizi *foo*için, her bir özel ilişki kimliği benzersiz olmalıdır. Ancak, başka bir ikizi *çubuğu* , bir *foo* ilişkisinin aynı kimliğiyle eşleşen bir giden ilişkiye sahip olabilir. 

Aşağıdaki kod örneği, Azure dijital TWINS örneğinizle nasıl bir ilişki ekleneceğini göstermektedir.

```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
{
    var relationship = new BasicRelationship
    {
        TargetId = targetId,
        Name = "contains"
    };

    try
    {
        string relId = $"{srcId}-contains->{targetId}";
        await client.CreateRelationshipAsync(srcId, relId, JsonSerializer.Serialize(relationship));
        Console.WriteLine("Created relationship successfully");
    }
    catch (RequestFailedException rex) {
        Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
    }
}
```

Yardımcı sınıfı hakkında daha fazla bilgi için `BasicRelationship` bkz. [*nasıl yapılır: Azure dijital TWINS API 'Leri ve SDK 'larını kullanma*](how-to-use-apis-sdks.md).

## <a name="list-relationships"></a>Liste ilişkileri

Grafikteki belirli bir ikizi ilişki listesine erişmek için şunu kullanabilirsiniz:

```csharp
await client.GetRelationshipsAsync(id);
```

Bu, `Azure.Pageable<T>` çağrının zaman `Azure.AsyncPageable<T>` uyumlu veya zaman uyumsuz sürümünü kullanıyorsanız öğesine bağlı olarak bir veya döndürür.

İlişkilerin bir listesini alan tam bir örnek aşağıda verilmiştir:

```csharp
public async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin
    try
    {
        // GetRelationshipsAsync will throw if an error occurs
        AsyncPageable<string> relsJson = client.GetRelationshipsAsync(dtId);
        List<BasicRelationship> results = new List<BasicRelationship>();
        await foreach (string relJson in relsJson)
        {
            var rel = System.Text.Json.JsonSerializer.Deserialize<BasicRelationship>(relJson);
            results.Add(rel);
        }
        return results;
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
        return null;
    }
}
```

Alınan ilişkileri kullanarak, grafiğinizde diğer TWINS 'ye gidebilirsiniz. Bunu yapmak için, `target` döndürülen ilişkiden alanı okuyun ve bir sonraki çağrınızdan kimlik olarak kullanın `GetDigitalTwin` . 

### <a name="find-relationships-to-a-digital-twin"></a>Dijital ikizi ilişkiler bulma

Azure dijital TWINS 'in Ayrıca belirli bir ikizi gelen tüm ilişkileri bulması için bir API 'SI vardır. Bu, genellikle ters gezinmede veya bir ikizi silinirken yararlıdır.

Önceki kod örneği giden ilişkileri bulmaya odaklanır. Aşağıdaki örnek benzerdir, ancak bunun yerine gelen ilişkileri bulur. Ayrıca, bunları bulduktan sonra da siler.

`IncomingRelationship`Çağrıların ilişkinin tam gövdesini döndürmediğini unutmayın.

```csharp
async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin
    try
    {
        // GetRelationshipsAsync will throw an error if a problem occurs
        AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

        List<IncomingRelationship> results = new List<IncomingRelationship>();
        await foreach (IncomingRelationship incomingRel in incomingRels)
            results.Add(incomingRel);
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
    }
}
```

## <a name="delete-relationships"></a>İlişkileri Sil

Kullanarak ilişkileri silebilirsiniz `DeleteRelationship(source, relId);` .

İlk parametre kaynak ikizi belirtir (ilişkinin kaynaklandığı ikizi). Diğer parametre ilişki KIMLIĞIDIR. İlişki kimlikleri yalnızca bir ikizi kapsamı içinde benzersiz olduğundan, hem ikizi ID hem de ilişki KIMLIĞI gereklidir.

## <a name="create-a-twin-graph"></a>İkizi grafiği oluşturma 

Aşağıdaki kod parçacığı, dijital TWINS ve ilişkilerden ikizi bir grafik oluşturmak için bu makaledeki ilişki işlemlerini kullanır.

```csharp
static async Task CreateTwins()
{
    // Create twins - see utility functions below 
    await CreateRoom("Room01", 68, 50, false, "");
    await CreateRoom("Room02", 70, 66, true, "EId-00124");
    await CreateFloorOrBuilding("Floor01", makeFloor:true);

    // Create relationships
    await AddRelationship("Floor01", "contains", "Floor-to-Room01", "Room01");
    await AddRelationship("Floor01", "contains", "Floor-to-Room02", "Room02");
}

static async Task<bool> AddRelationship(string source, string relationship, string id, string target)
{
    var relationship = new BasicRelationship
    {
        TargetId = target,
        Name = relationship
    };

    try
    {
        string relId = $"{source}-contains->{target}";
        await client.CreateRelationshipAsync(source, relId, JsonSerializer.Serialize(relationship));
        Console.WriteLine("Created relationship successfully");
        return true;
    }
    catch (RequestFailedException rex) {
        Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
        return false;
    }
}

static async Task<bool> CreateRoom(string id, double temperature, double humidity)
{
    BasicDigitalTwin twin = new BasicDigitalTwin();
    twin.Metadata = new DigitalTwinMetadata();
    twin.Metadata.ModelId = "dtmi:com:contoso:Room;2";
    // Initialize properties
    Dictionary<string, object> props = new Dictionary<string, object>();
    props.Add("Temperature", temperature);
    props.Add("Humidity", humidity);
    twin.CustomProperties = props;
    
    try
    {
        client.CreateDigitalTwin(id, JsonSerializer.Serialize<BasicDigitalTwin>(twin)); 
        return true;       
    }
    catch (ErrorResponseException e)
    {
        Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}"); 
        return false;
    }
}

static async Task<bool> CreateFloorOrBuilding(string id, bool makeFloor=true)
{
    string type = "dtmi:com:contoso:Building;3";
    if (makeFloor==true)
        type = "dtmi:com:contoso:Floor;2";
    BasicDigitalTwin twin = new BasicDigitalTwin();
    twin.Metadata = new DigitalTwinMetadata();
    twin.Metadata.ModelId = type;
    // Initialize properties
    Dictionary<string, object> props = new Dictionary<string, object>();
    props.Add("AverageTemperature", 0);
    twin.CustomProperties = props;
    
    try
    {
        client.CreateDigitalTwin(id, JsonSerializer.Serialize<BasicDigitalTwin>(twin));  
        return true;      
    }
    catch (ErrorResponseException e)
    {
        Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}"); 
        return false;
    }
}
```

### <a name="create-a-twin-graph-from-a-spreadsheet"></a>Elektronik tablodan ikizi grafik oluşturma

Pratik kullanım durumlarında, ikizi hiyerarşileri genellikle farklı bir veritabanında depolanan verilerden veya belki de bir elektronik tabloda oluşturulur. Bu bölümde bir elektronik tablonun nasıl ayrıştırılabileceği gösterilmektedir.

Oluşturulacak bir dizi dijital TWINS ve ilişki tanımlayan aşağıdaki veri tablosunu göz önünde bulundurun.

| Modelleme    | ID | Üst | İlişki adı | Diğer veriler |
| --- | --- | --- | --- | --- |
| sını    | Floor01 | | | … |
| Oda    | Room10 | Floor01 | contains | … |
| Oda    | Room11 | Floor01 | contains | … |
| Oda    | Room12 | Floor01 | contains | … |
| sını    | Floor02 | | | … |
| Oda    | Room21 | Floor02 | contains | … |
| Oda    | Room22 | Floor02 | contains | … |

Aşağıdaki kod, bir elektronik tabloyu okumak ve sonuçlardan bir Azure dijital TWINS ikizi grafiği oluşturmak için [MICROSOFT Graph API](https://docs.microsoft.com/graph/overview) kullanır.

```csharp
var range = msftGraphClient.Me.Drive.Items["BuildingsWorkbook"].Workbook.Worksheets["Building"].usedRange;
JsonDocument data = JsonDocument.Parse(range.values);
List<BasicRelationship> RelationshipRecordList = new List<BasicRelationship>();
foreach (JsonElement row in data.RootElement.EnumerateArray())
{
    string type = row[0].GetString();
    string id = row[1].GetString();
    string relSource = row[2].GetString();
    string relName = row[3].GetString();
    // Parse spreadsheet extra data into a JSON string to initialize the digital twin
    // Left out for compactness
    Dictionary<string, object> initData = new Dictionary<string, object>() { ... };

    if (relSource != null)
    {
        BasicRelationship br = new BasicRelationship()
        {
            SourceId = relSource,
            TargetId = id,
            Name = relName
        };
        RelationshipRecordList.Add(br);
    }

    BasicDigitalTwin twin = new BasicDigitalTwin();
    twin.CustomProperties = initData;
    // Set the type of twin to be created
    switch (type)
    {
        case "room":
            twin.Metadata = new DigitalTwinMetadata() { ModelId = "dtmi:com:contoso:Room;2" };
            break;
        case "floor":
            twin.Metadata = new DigitalTwinMetadata() { ModelId = "dtmi:com:contoso:Floor;2" };
            break;
        ... handle additional types
    }
    try
    {
        client.CreateDigitalTwin(id, JsonSerializer.Serialize<BasicDigitalTwin>(twin));
    }
    catch (RequestFailedException e)
    {
        Log.Error($"Error {e.Status}: {e.Message}");
    }
    foreach (BasicRelationship rec in RelationshipRecordList)
    { 
        try { 
            client.CreateRelationship(rec.SourceId, Guid.NewGuid().ToString(), JsonSerializer.Serialize<BasicRelationship>(rec));
        }
        catch (RequestFailedException e)
        {
            Log.Error($"Error {e.Status}: {e.Message}");
        }
    }
}
```
## <a name="manage-relationships-with-cli"></a>CLı ile ilişkileri yönetme

Azure dijital TWINS CLı kullanılarak aynı zamanda ve ilişkileri de yönetilebilir. Komutları [*nasıl yapılır: Azure dijital TWINS CLI 'Sını kullanma*](how-to-use-cli.md)bölümünde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS ikizi grafiğini sorgulama hakkında bilgi edinin:
* [*Kavramlar: sorgu dili*](concepts-query-language.md)
* [*Nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)