---
title: Dijital ikizler ve ikiz grafiği
titleSuffix: Azure Digital Twins
description: Dijital ikizi kavramını ve ilişkilerinin bir grafiği nasıl yaptığını anlayın.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: c4cdfc35d39be827109c56a16d68f6e51b56ef43
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85483355"
---
# <a name="understand-digital-twins-and-their-twin-graph"></a>Dijital TWINS ve ikizi graflarını anlayın

Bir Azure dijital TWINS çözümünde ortamınızdaki varlıklar Azure **dijital TWINS**tarafından temsil edilir. Dijital ikizi, özel olarak tanımlanan [modellerden](concepts-models.md)birinin bir örneğidir. Diğer dijital TWINS 'e, **ikizi grafiği**oluşturmak için **ilişkiler** aracılığıyla bağlanabilir: Bu ikizi Graph, ortamınızın tamamının gösterimidir.

> [!TIP]
> "Azure dijital TWINS", bu Azure hizmetine bir bütün olarak başvurur. "Digital ikizi (s)" veya yalnızca "ikizi (s)", hizmet örneğinizin içindeki bireysel ikizi düğümlerine başvurur.

## <a name="creating-digital-twins"></a>Dijital TWINS oluşturma

Azure dijital TWINS Örneğinizde dijital bir ikizi oluşturabilmeniz için önce hizmete bir *modelin* yüklenmiş olması gerekir. Model, diğer şeyler arasında belirli bir ikizi sahip olduğu özellikler, telemetri iletileri ve ilişkiler kümesini açıklar. Bir modelde tanımlanan bilgi türleri için bkz. [Kavramlar: özel modeller](concepts-models.md).

Bir modeli oluşturup karşıya yükledikten sonra, istemci uygulamanız türün bir örneğini oluşturabilir; Bu bir dijital ikizi. Örneğin, bir *kat*modeli oluşturduktan sonra bu türü kullanan bir veya birkaç dijital TWINS oluşturabilirsiniz ( *Groundfloor*adlı bir *taban*-tür ikizi gibi, başka bir *Floor2*vb.). 

Aşağıda, *Oda*türünde bir ikizi örneği oluşturmak Için [Digitaltwins API 'lerini](how-to-use-apis-sdks.md) kullanan istemci kodu kod parçacığı verilmiştir.

Azure Digital TWINS 'in geçerli önizlemede, ikizi oluşturulmadan önce bir ikizi öğesinin tüm özellikleri başlatılmalıdır. Bu, gerekli başlatma değerlerini sağlayan bir JSON belgesi oluşturularak yapılır.

```csharp
public Task<boolean> CreateRoom(string id, double temperature, double humidity) 
{
    // Define the model for the twin to be created
    Dictionary<string, object> meta = new Dictionary<string, object>()
    {
      { "$model", "dtmi:com:contoso:Room;2" }
    };
    // Initialize the twin properties
    Dictionary<string, object> initData = new Dictionary<string, object>()
    {
      { "$metadata", meta },
      { "Temperature", temperature},
      { "Humidity", humidity},
    };
    try
    {
      await client.DigitalTwins.AddAsync(id, initData);
      return true;
    }
    catch (ErrorResponseException e)
    {
      Console.WriteLine($"*** Error creating twin {id}: {e.Response.StatusCode}");
      return false;
    }
}
```

## <a name="relationships-creating-a-graph-of-digital-twins"></a>İlişkiler: dijital bir TWINS grafiği oluşturma

TWINS, ilişkileri tarafından bir ikizi grafiğine bağlanır. Bir ikizi 'in sahip olduğu ilişkiler, modelinin bir parçası olarak tanımlanmıştır.  

Örneğin, model *tabanı* bir, *Oda*türü olarak İKI hedefleyen bir *ilişki içerebilir.* Azure dijital TWINS, bu tanım ile *herhangi bir* *tabandan* ( *Oda* alt türlerinden oluşan TWINS dahil) herhangi bir kat *ikizi ilişki oluşturmanıza* olanak sağlayacak. 

Burada, *Groundfloor* adlı bir *taban*-tür dijital ikizi ve *Cafe*adlı bir *Oda*türü dijital Ikizi arasında bir ilişki oluşturmak için [digitaltwins API 'lerini](how-to-use-apis-sdks.md) kullanan bazı örnek istemci kodları verilmiştir.

```csharp
// Create Twins, using functions similar to the previous sample
await CreateRoom("Cafe", 70, 66);
await CreateFloor("GroundFloor", averageTemperature=70);
// Create relationships
Dictionary<string, object> targetrec = new Dictionary<string, object>()
{
    { "$targetId", "Cafe" }
};
try
{
    await client.DigitalTwins.AddEdgeAsync("GroundFloor", "contains", "GF-to-Cafe", targetrec);
} catch(ErrorResponseException e)
{
    Console.WriteLine($"*** Error creating relationship: {e.Response.StatusCode}");
}
```

Bu işlemin sonucu, bir grafikteki kenarlar (bunların ilişkileri) aracılığıyla bağlanan bir düğüm kümesidir (dijital TWINS).

## <a name="json-representations-of-graph-elements"></a>Grafik öğelerinin JSON gösterimleri

Dijital ikizi verileri ve ilişki verileri her ikisi de JSON biçiminde depolanır. Bu, [ikizi grafiğini](how-to-query-graph.md) Azure dijital TWINS Örneğinizde sorguladığınızda, sonucun dijital TWINS ve oluşturduğunuz ilişkilerin JSON gösterimi olacağı anlamına gelir.

### <a name="digital-twin-json-format"></a>Digital ikizi JSON biçimi

JSON nesnesi olarak temsil edildiğinde, dijital bir ikizi aşağıdaki alanları görüntüler:

| Alan adı | Açıklama |
| --- | --- |
| `$dtId` | Dijital ikizi KIMLIĞINI temsil eden kullanıcı tarafından sağlanmış dize |
| `$conformance` | Bu dijital ikizi uygunluk durumunu içeren bir sabit listesi (*uyumlu*, *uyumlu değil*, *bilinmiyor*) |
| `{propertyName}` | JSON ( `string` , sayı türü veya nesne) içindeki bir özelliğin değeri |
| `$relationships` | İlişki koleksiyonu yolunun URL 'SI. Bu alan, dijital ikizi giden ilişki kenarları yoksa yok olur. |
| `$metadata.$model` | Seçim Bu dijital ikizi karakterleştirir model arabiriminin KIMLIĞI |
| `$metadata.{propertyName}.desiredValue` | [Yalnızca yazılabilir özellikler için] Belirtilen özelliğin istenen değeri |
| `$metadata.{propertyName}.desiredVersion` | [Yalnızca yazılabilir özellikler için] İstenen değerin sürümü |
| `$metadata.{propertyName}.ackVersion` | Dijital ikizi uygulayan cihaz uygulaması tarafından kabul edilen sürüm |
| `$metadata.{propertyName}.ackCode` | [Yalnızca yazılabilir özellikler için] `ack`Dijital ikizi uygulayan cihaz uygulaması tarafından döndürülen kod |
| `$metadata.{propertyName}.ackDescription` | [Yalnızca yazılabilir özellikler için] `ack`Dijital ikizi uygulayan cihaz uygulaması tarafından döndürülen açıklama |
| `{componentName}` | Kök nesnesine benzer şekilde bileşenin özellik değerlerini ve meta verilerini içeren bir JSON nesnesi. Bileşenin özelliği olmasa bile bu nesne vardır. |
| `{componentName}.{propertyName}` | JSON ( `string` , sayı türü veya nesne) içindeki bileşenin özelliğinin değeri |
| `{componentName}.$metadata` | Bileşene ait meta veri bilgileri, kök düzeyine benzer`$metadata` |

JSON nesnesi olarak biçimlendirilen bir dijital ikizi örneği aşağıda verilmiştir:

```json
{
  "$dtId": "Cafe",
  "Temperature": 72,
  "Location": {
    "x": 101,
    "y": 33
  },
  "component": {
    "TableOccupancy": 1,
    "$metadata": {
      "TableOccupancy": {
        "desiredValue": 1,
        "desiredVersion": 3,
        "ackVersion": 2,
        "ackCode": 200,
        "ackDescription": "OK"
      }
    }
  },
  "$metadata": {
    "$model": "dtmi:com:contoso:Room;1",
    "Temperature": {
      "desiredValue": 72,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "Location": {
      "desiredValue": {
        "x": 101,
        "y": 33,
      },
      "desiredVersion": 8,
      "ackVersion": 8,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

### <a name="relationship-json-format"></a>İlişki JSON biçimi

Bir JSON nesnesi olarak temsil edildiğinde, dijital bir ikizi bir ilişki aşağıdaki alanları görüntüler:

| Alan adı | Açıklama |
| --- | --- |
| `$relationshipId` | Bu ilişkinin KIMLIĞINI temsil eden kullanıcı tarafından sağlanmış bir dize. Bu dize, kaynak dijital ikizi bağlamında benzersizdir, bu da `sourceId`  +  `relationshipId` Azure dijital TWINS örneği bağlamında benzersiz anlamına gelir. |
| `$sourceId` | Kaynak dijital ikizi KIMLIĞI |
| `$targetId` | Hedef dijital ikizi KIMLIĞI |
| `$relationshipName` | İlişkinin adı |
| `{propertyName}` | Seçim Bu ilişkinin bir özelliğinin değeri, JSON ( `string` , sayı türü veya nesne) |

JSON nesnesi olarak biçimlendirilen bir ilişki örneği aşağıda verilmiştir:

```json
{
  "$relationshipId": "relationship-01",
  "$etag": "W/\"506e8391-2b21-4ac9-bca3-53e6620f6a90\"",
  "$sourceId": "GroundFloor",
  "$relationshipName": "contains",
  "$targetId": "Cafe",
  "startDate": "2020-02-04"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bkz. Azure Digital Ikizi API 'Leri ile Graf öğelerini yönetme:
* [Nasıl yapılır: dijital TWINS 'i yönetme](how-to-manage-twin.md)
* [Nasıl yapılır: ikizi grafiğini ilişkilerle yönetme](how-to-manage-graph.md)

Ya da, Azure Digital TWINS ikizi grafiğini sorgulama hakkında bilgi edinin:
* [Kavramlar: sorgu dili](concepts-query-language.md)