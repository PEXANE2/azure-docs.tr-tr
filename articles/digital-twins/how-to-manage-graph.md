---
title: İlişkileri olan ikiz grafiğini yönetme
titleSuffix: Azure Digital Twins
description: Bkz. bir dijital TWINS grafiğini ilişkiler ile bağlayarak yönetme.
author: baanders
ms.author: baanders
ms.date: 10/21/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 947a0c9a0af3c38d6c4d6f66da691d62530a69e7
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93279497"
---
# <a name="manage-a-graph-of-digital-twins-using-relationships"></a>İlişkileri kullanarak dijital TWINS grafiğini yönetme

Azure dijital TWINS 'in kalbi, tüm ortamınızı temsil eden [ikizi grafiktir](concepts-twins-graph.md) . İkizi Graph, **ilişkiler** aracılığıyla bağlı tek tek dijital TWINS 'ten oluşur. 

Çalışan bir [Azure dijital TWINS örneğinden](how-to-set-up-instance-portal.md) ve istemci uygulamanızda [kimlik doğrulama](how-to-authenticate-client.md) kodu ayarladıktan sonra, Azure dijital TWINS örneğindeki dijital TWINS ve bunların ilişkilerini oluşturmak, değiştirmek ve silmek Için [**digitaltwins API 'lerini**](/rest/api/digital-twins/dataplane/twins) kullanabilirsiniz. [.Net (C#) SDK 'sını](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true)veya [Azure DIGITAL TWINS CLI](how-to-use-cli.md)'yi de kullanabilirsiniz.

Bu makale, ilişkilerin ve grafiğin tamamını yönetmeye odaklanır. tek tek dijital TWINS ile çalışmak için bkz. [*nasıl yapılır: dijital TWINS 'ı yönetme*](how-to-manage-twin.md).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]
    
[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

## <a name="create-relationships"></a>İlişki oluştur

İlişkiler, ikizi grafiğinin temelini oluşturan farklı dijital TWINS 'in birbirlerine nasıl bağlandığını tanımlar.

İlişkiler, çağrı kullanılarak oluşturulur `CreateRelationship()` . 

Bir ilişki oluşturmak için şunu belirtmeniz gerekir:
* Kaynak ikizi KIMLIĞI ( `srcId` Aşağıdaki kod örneğinde): ilişkinin kaynaklandığı IKIZI kimliği.
* Target ikizi ID ( `targetId` Aşağıdaki kod örneğinde): ilişkinin ulaştığı IKIZI kimliği.
* Bir ilişki adı ( `relName` Aşağıdaki kod örneğinde): bunun gibi bir genel tür ilişkisi _vardır_.
* İlişki KIMLIĞI ( `relId` Aşağıdaki kod örneğinde): Bu ilişkinin özel adı, _Relationship1_ gibi bir şeydir.

İlişki KIMLIĞI, belirtilen kaynak ikizi içinde benzersiz olmalıdır. Genel olarak benzersiz olması gerekmez.
Örneğin, ikizi *foo* için, her bir özel ilişki kimliği benzersiz olmalıdır. Ancak, başka bir ikizi *çubuğu* , bir *foo* ilişkisinin aynı kimliğiyle eşleşen bir giden ilişkiye sahip olabilir.

Aşağıdaki kod örneğinde, Azure dijital TWINS Örneğinizde nasıl ilişki oluşturacağınız gösterilmektedir.

```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId, string relName)
        {
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = relName
            };

            try
            {
                string relId = $"{srcId}-{relName}->{targetId}";
                await client.CreateOrReplaceRelationshipAsync(srcId, relId, relationship);
                Console.WriteLine($"Created {relName} relationship successfully");
            }
            catch (RequestFailedException rex)
            {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }
            
        }
```
Ana yönteinizde artık aşağıdaki `CreateRelationship()` gibi bir ilişki _içeren_ bir ilişki oluşturmak için işlevini çağırabilirsiniz: 

```csharp
await CreateRelationship(client, srcId, targetId, "contains");
```
Birden çok ilişki oluşturmak isterseniz, farklı ilişki türlerini bağımsız değişkene geçirerek aynı yönteme yapılan çağrıları tekrarlayabilirsiniz. 

Yardımcı sınıfı hakkında daha fazla bilgi için `BasicRelationship` bkz. [*nasıl yapılır: Azure dijital TWINS API 'Leri ve SDK 'larını kullanma*](how-to-use-apis-sdks.md#serialization-helpers).

### <a name="create-multiple-relationships-between-twins"></a>TWINS arasında birden çok ilişki oluşturma

İlişkiler aşağıdakilerden biri olarak sınıflandırılabilir: 

* Giden ilişkiler: Bu ikizi 'e ait olan Ilişkiler, diğer TWINS 'e bağlamak için dışarıya işaret eden bir Ilişki. `GetRelationshipsAsync()`Yöntemi, bir ikizi giden ilişkisini almak için kullanılır.
* Gelen ilişkiler: bir "gelen" bağlantısı oluşturmak için bu ikizi 'e işaret eden diğer TWINS 'e ait Ilişkiler. `GetIncomingRelationshipsAsync()`Yöntemi, bir ikizi gelen ilişkilerini almak için kullanılır.

İki TWINS arasında sahip olduğunuz ilişki sayısında herhangi bir kısıtlama yoktur; TWINS arasında istediğiniz sayıda ilişkiye sahip olabilirsiniz. 

Bu, aynı anda iki farklı ilişki türünü tek seferde ifade ettiğiniz anlamına gelir. Örneğin, *Ikizi A* , *depolanan* bir ilişki ve *ikizi B* ile *üretilmiş* ilişkiye sahip olabilir.

İsterseniz aynı türde ilişki türünün aynı ilişki türünde birden fazla örneğini de oluşturabilirsiniz. Bu örnekte, ilişkilerin farklı ilişki kimliklerine sahip olduğu sürece *Ikizi A* , *ikizi B* ile iki farklı *saklı* ilişkiye sahip olabilir.

## <a name="list-relationships"></a>Liste ilişkileri

Grafikteki belirli bir ikizi **giden** ilişkiler listesine erişmek için `GetRelationships()` yöntemini şöyle kullanabilirsiniz:

```csharp
await client.GetRelationships()
```

Bu, `Azure.Pageable<T>` `Azure.AsyncPageable<T>` çağrının zaman uyumlu veya zaman uyumsuz sürümünü kullanıp kullanmayacağınızı bağlı olarak bir veya döndürür.

İlişkilerin bir listesini alan bir örnek aşağıda verilmiştir:

```csharp
public static async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            try
            {
                // GetRelationshipsAsync will throw if an error occurs
                AsyncPageable<BasicRelationship> rels = client.GetRelationshipsAsync<BasicRelationship>(dtId);
                List<BasicRelationship> results = new List<BasicRelationship>();
                await foreach (BasicRelationship rel in rels)
                {
                    results.Add(rel);
                    Console.WriteLine($"Found relationship-{rel.Name}->{rel.TargetId}");
                }

                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

```
Şu şekilde, TWINS 'in giden ilişkilerini şu şekilde görmek için bu yöntemi çağırabilirsiniz:

```csharp
await FindOutgoingRelationshipsAsync(client, twin_Id);
```
Alınan ilişkileri kullanarak, grafiğinizde diğer TWINS 'ye gidebilirsiniz. Bunu yapmak için, `target` döndürülen ilişkiden alanı okuyun ve bir sonraki çağrınızdan kimlik olarak kullanın `GetDigitalTwin()` .

### <a name="find-incoming-relationships-to-a-digital-twin"></a>Dijital ikizi gelen ilişkilerini bulma

Azure dijital TWINS 'in Ayrıca belirli bir ikizi tüm _ *gelen* * ilişkilerini bulması için bir API 'si vardır. Bu, genellikle ters gezinmede veya bir ikizi silinirken yararlıdır.

Önceki kod örneği, bir ikizi giden ilişkilerini bulmaya odaklanılmıştır. Aşağıdaki örnek benzer şekilde yapılandırılmıştır, ancak bunun yerine ikizi *gelen* ilişkileri bulur.

`IncomingRelationship`Çağrıların ilişkinin tam gövdesini döndürmediğini unutmayın.

```csharp
public static async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            try
            {
                // GetRelationshipsAsync will throw an error if a problem occurs
                AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

                List<IncomingRelationship> results = new List<IncomingRelationship>();
                await foreach (IncomingRelationship incomingRel in incomingRels)
                {
                    results.Add(incomingRel);
                    Console.WriteLine($"Found incoming relationship-{incomingRel.RelationshipId}");

                }
                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"*** Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }
```

Şu şekilde, TWINS 'in gelen ilişkilerini şu şekilde görmek için bu yöntemi çağırabilirsiniz:

```csharp
await FindIncomingRelationshipsAsync(client, twin_Id);
```
### <a name="list-all-twin-properties-and-relationships"></a>Tüm ikizi özelliklerini ve ilişkilerini listeleyin

Bir ikizi giden ve gelen ilişkilerini listelemek için yukarıdaki yöntemleri kullanarak, ikizi özellikleri ve ilişkilerinin her iki türü de dahil olmak üzere tam ikizi bilgilerini yazdıran bir yöntem oluşturabilirsiniz. Bunun nasıl yapılacağını gösteren adlı örnek bir yöntem aşağıda verilmiştir `FetchAndPrintTwinAsync()` .

```csharp  
private static async Task FetchAndPrintTwinAsync(DigitalTwinsClient client, string twin_Id)
        {
            BasicDigitalTwin twin;
            Response<BasicDigitalTwin> res = client.GetDigitalTwin(twin_Id);
            
            await FindOutgoingRelationshipsAsync(client, twin_Id);
            await FindIncomingRelationshipsAsync(client, twin_Id);

            return;
        }
```

Artık bu işlevi şu şekilde Main yönteinizde çağırabilirsiniz: 

```csharp
await FetchAndPrintTwinAsync(client, targetId);
```
## <a name="delete-relationships"></a>İlişkileri Sil

İlk parametre kaynak ikizi belirtir (ilişkinin kaynaklandığı ikizi). Diğer parametre ilişki KIMLIĞIDIR. İlişki kimlikleri yalnızca bir ikizi kapsamı içinde benzersiz olduğundan, hem ikizi ID hem de ilişki KIMLIĞI gereklidir.

```csharp
private static async Task DeleteRelationship(DigitalTwinsClient client, string srcId, string relId)
        {
            try
            {
                Response response = await client.DeleteRelationshipAsync(srcId, relId);
                await FetchAndPrintTwinAsync(srcId, client);
                Console.WriteLine("Deleted relationship successfully");
            }
            catch (RequestFailedException Ex)
            {
                Console.WriteLine($"Error {Ex.ErrorCode}");
            }
        }
```

Artık şöyle bir ilişki silmek için bu yöntemi çağırabilirsiniz:

```csharp
await DeleteRelationship(client, srcId, relId);
```
## <a name="create-a-twin-graph"></a>İkizi grafiği oluşturma 

Aşağıdaki yürütülebilir kod parçacığı, dijital TWINS ve ilişkilerden ikizi bir grafik oluşturmak için bu makaledeki ilişki işlemlerini kullanır.

### <a name="set-up-the-runnable-sample"></a>Çalıştırılabilir örneği ayarlama

Kod parçacığı öğreticiden model tanımlarında [*Room.js*](https://github.com/Azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Room.json) ve [*Floor.js*](https://github.com/azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Floor.json) kullanır [*: örnek bir Istemci uygulamasıyla Azure dijital TWINS 'i araştırma*](tutorial-command-line-app.md). Bu bağlantıları doğrudan dosyalara gitmek veya tam uçtan uca [örnek projenin bir](/samples/azure-samples/digital-twins-samples/digital-twins-samples/)parçası olarak indirmek için kullanabilirsiniz. 

Örneği çalıştırmadan önce aşağıdakileri yapın:
1. Model dosyalarını indirin, projenize yerleştirin ve `<path-to>` aşağıdaki koddaki yer tutucuları değiştirip programınıza onları nerede bulacağınızı söyleyin.
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
            DigitalTwinsClient client = createDTClient();
            Console.WriteLine($"Service client created – ready to go");
            Console.WriteLine();

            //Upload models
            Console.WriteLine($"Upload models");
            Console.WriteLine();
            string dtdl = File.ReadAllText("<path-to>/Room.json");
            string dtdl1 = File.ReadAllText("<path-to>/Floor.json");
            var typeList = new List<string>();
            typeList.Add(dtdl);
            typeList.Add(dtdl1);
            // Upload the models to the service
            await client.CreateModelsAsync(typeList);

            //Create new (Floor) digital twin
            BasicDigitalTwin floorTwin = new BasicDigitalTwin();
            string srcId = "myFloorID";
            floorTwin.Metadata = new DigitalTwinMetadata();
            floorTwin.Metadata.ModelId = "dtmi:example:Floor;1";
            //Floor twins have no properties, so nothing to initialize
            //Create the twin
            await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(srcId, floorTwin);
            Console.WriteLine("Twin created successfully");

            //Create second (Room) digital twin
            BasicDigitalTwin roomTwin = new BasicDigitalTwin();
            string targetId = "myRoomID";
            roomTwin.Metadata = new DigitalTwinMetadata();
            roomTwin.Metadata.ModelId = "dtmi:example:Room;1";
            // Initialize properties
            Dictionary<string, object> props = new Dictionary<string, object>();
            props.Add("Temperature", 35.0);
            props.Add("Humidity", 55.0);
            roomTwin.Contents = props;
            //Create the twin
            await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(targetId, roomTwin);
            
            //Create relationship between them
            await CreateRelationship(client, srcId, targetId, "contains");
            Console.WriteLine();

            //Print twins and their relationships
            Console.WriteLine("--- Printing details:");
            Console.WriteLine("Outgoing relationships from source twin:");
            await FetchAndPrintTwinAsync(srcId, client);
            Console.WriteLine();
            Console.WriteLine("Incoming relationships to target twin:");
            await FetchAndPrintTwinAsync(targetId, client);
            Console.WriteLine("--------");
            Console.WriteLine();

            //Delete the relationship
            Console.WriteLine("Deleting the relationship");
            await DeleteRelationship(client, srcId, $"{srcId}-contains->{targetId}");
            Console.WriteLine();

            //Print twins and their relationships again
            Console.WriteLine("--- Printing details:");
            Console.WriteLine("Outgoing relationships from source twin:");
            await FetchAndPrintTwinAsync(srcId, client);
            Console.WriteLine();
            Console.WriteLine("Incoming relationships to target twin:");
            await FetchAndPrintTwinAsync(targetId, client);
            Console.WriteLine("--------");
            Console.WriteLine();
        }

        private static DigitalTwinsClient createDTClient()
        {
            string adtInstanceUrl = "https://<your-instance-hostname>";
            var credentials = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
            return client;
        }
        private async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId, string relName)
        {
            // Create relationship between twins
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = relName
            };

            try
            {
                string relId = $"{srcId}-{relName}->{targetId}";
                await client.CreateOrReplaceRelationshipAsync(srcId, relId, relationship);
                Console.WriteLine($"Created {relName} relationship successfully");
            }
            catch (RequestFailedException rex)
            {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }

        }

        private static async Task FetchAndPrintTwinAsync(string twin_Id, DigitalTwinsClient client)
        {
            BasicDigitalTwin twin;
            Response<BasicDigitalTwin> res = client.GetDigitalTwin(twin_Id);
            await FindOutgoingRelationshipsAsync(client, twin_Id);
            await FindIncomingRelationshipsAsync(client, twin_Id);

            return;
        }

        private static async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            
            try
            {
                // GetRelationshipsAsync will throw if an error occurs
                AsyncPageable<BasicRelationship> rels = client.GetRelationshipsAsync<BasicRelationship>(dtId);
                List<BasicRelationship> results = new List<BasicRelationship>();
                await foreach (BasicRelationship rel in rels)
                {
                    results.Add(rel);
                    Console.WriteLine($"Found relationship-{rel.Name}->{rel.TargetId}");
                }

                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"*** Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

        private static async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            
            try
            {
                // GetRelationshipsAsync will throw an error if a problem occurs
                AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

                List<IncomingRelationship> results = new List<IncomingRelationship>();
                await foreach (IncomingRelationship incomingRel in incomingRels)
                {
                    results.Add(incomingRel);
                    Console.WriteLine($"Found incoming relationship-{incomingRel.RelationshipId}");
                }
                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

        private static async Task DeleteRelationship(DigitalTwinsClient client, string srcId, string relId)
        {
            try
            {
                Response response = await client.DeleteRelationshipAsync(srcId, relId);
                await FetchAndPrintTwinAsync(srcId, client);
                Console.WriteLine("Deleted relationship successfully");
            }
            catch (RequestFailedException Ex)
            {
                Console.WriteLine($"Error {Ex.ErrorCode}");
            }
        }
    }
}
```

Yukarıdaki programın konsol çıktısı aşağıda verilmiştir: 

:::image type="content" source="./media/how-to-manage-graph/console-output-twin-graph.png" alt-text="İkizi ayrıntılarını, gelen ve giden TWINS ilişkilerini gösteren konsol çıkışı." lightbox="./media/how-to-manage-graph/console-output-twin-graph.png":::

> [!TIP]
> İkizi Graph, TWINS arasında ilişki oluşturma kavramıdır. İkizi grafiğinin görsel temsilini görüntülemek istiyorsanız, bu makalenin [_Visualization *](how-to-manage-graph.md#visualization) bölümüne bakın. 

### <a name="create-a-twin-graph-from-a-spreadsheet"></a>Elektronik tablodan ikizi grafik oluşturma

Pratik kullanım durumlarında, ikizi hiyerarşileri genellikle farklı bir veritabanında depolanan verilerden veya belki de bir elektronik tabloda oluşturulur. Bu bölümde bir elektronik tablonun nasıl ayrıştırılabileceği gösterilmektedir.

Oluşturulacak bir dizi dijital TWINS ve ilişki tanımlayan aşağıdaki veri tablosunu göz önünde bulundurun.

| Model Kimliği| İkizi KIMLIĞI (benzersiz olmalıdır) | İlişki adı | Hedef ikizi KIMLIĞI | İkizi init verileri |
| --- | --- | --- | --- | --- |
| dtmı: örnek: kat; 1 | Floor1 |  contains | Room1 |{"Sıcaklık": 80, "nem": 60}
| dtmı: örnek: kat; 1 | Floor0 |  silindi      | Room0 |{"Sıcaklık": 70, "nem": 30}
| dtmı: örnek: Oda; 1  | Room1 | 
| dtmı: örnek: Oda; 1  | Room0 |

Aşağıdaki kod, bir elektronik tabloyu okumak ve sonuçlardan bir Azure dijital TWINS ikizi grafiği oluşturmak için [MICROSOFT Graph API](/graph/overview) kullanır.

```csharp
var range = msftGraphClient.Me.Drive.Items["BuildingsWorkbook"].Workbook.Worksheets["Building"].usedRange;
JsonDocument data = JsonDocument.Parse(range.values);
List<BasicRelationship> RelationshipRecordList = new List<BasicRelationship>();
foreach (JsonElement row in data.RootElement.EnumerateArray())
{
    string modelId = row[0].GetString();
    string sourceId = row[1].GetString();
    string relName = row[2].GetString();
    string targetId = row[3].GetString();
    string initData = row[4].GetString();
    
    // Parse spreadsheet extra data into a JSON string to initialize the digital twin
    // Left out for compactness
    Dictionary<string, object> initData = new Dictionary<string, object>() { ... };

    if (sourceId != null)
    {
        BasicRelationship br = new BasicRelationship()
        {
            SourceId = sourceId,
            TargetId = targetId,
            Name = relName
        };
        RelationshipRecordList.Add(br);
    }

    BasicDigitalTwin twin = new BasicDigitalTwin();
    twin.Contents = initData;
    // Set the type of twin to be created
    twin.Metadata = new DigitalTwinMetadata() { ModelId = modelId };
    
    try
    {
        await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(sourceId, twin);
    }
    catch (RequestFailedException e)
    {
       Console.WriteLine($"Error {e.Status}: {e.Message}");
    }
    foreach (BasicRelationship rec in RelationshipRecordList)
    { 
        try { 
            await client.CreateOrReplaceRelationshipAsync(rec.sourceId, Guid.NewGuid().ToString(), rec);
        }
        catch (RequestFailedException e)
        {
            Console.WriteLine($"Error {e.Status}: {e.Message}");
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