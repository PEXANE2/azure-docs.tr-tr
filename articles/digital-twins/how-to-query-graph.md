---
title: İkizi grafiğini sorgulama
titleSuffix: Azure Digital Twins
description: Bilgi için bkz. Azure dijital TWINS ikizi grafiğini sorgulama.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 93043874db6076b26d0fefe447db7acd83547442
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84612883"
---
# <a name="query-the-azure-digital-twins-twin-graph"></a>Azure Digital TWINS ikizi grafiğini sorgulama

Bu makale, bilgi için [ikizi grafiğini](concepts-twins-graph.md) sorgulamak üzere [Azure Digital TWINS sorgu deposu dilinin](concepts-query-language.md) kullanılmasıyla ilgili ayrıntılı bilgi sağlar. Azure Digital TWINS [**sorgu API 'lerini**](how-to-use-apis-sdks.md)kullanarak grafikteki sorguları çalıştırırsınız.

## <a name="query-syntax"></a>Sorgu söz dizimi

Sorgu dili yapısını gösteren ve olası sorgu işlemlerini gerçekleştiren bazı örnek sorgular aşağıda verilmiştir.

Özelliklere göre [dijital TWINS](concepts-twins-graph.md) al (kimlik ve meta veriler dahil):
```sql
SELECT  * 
FROM DigitalTwins T  
WHERE T.firmwareVersion = '1.1'
AND T.$dtId in ['123', '456']
AND T.Temperature = 70
```

[Modele](concepts-models.md) göre dijital TWINS al
```sql
SELECT  * 
FROM DigitalTwins T  
WHERE IS_OF_MODEL(T , 'dtmi:com:contoso:Space;3')
AND T.roomSize > 50
```

> [!TIP]
> Dijital bir ikizi KIMLIĞI, meta veri alanı kullanılarak sorgulanır `$dtId` .

## <a name="run-queries-with-an-api-call"></a>Sorguları bir API çağrısıyla çalıştırma

Bir sorgu dizesine karar verdikten sonra **sorgu API**'sine bir çağrı yaparak onu yürütün.
Aşağıdaki kod parçacığı, istemci uygulamasından bu çağrıyı gösterir:

```csharp
var client = new AzureDigitalTwinsAPIClient(<your-credentials>);
client.BaseUri = new Uri(<your-Azure-Digital-Twins-instance-URL>);

QuerySpecification spec = new QuerySpecification("SELECT * FROM digitaltwins");
QueryResult result = await client.Query.QueryTwinsAsync(spec);
```

Bu çağrı sorgu sonuçlarını bir QueryResult nesnesi biçiminde döndürür. 

Sorgu çağrıları sayfalama destekler. Hata işleme ve sayfalama içeren bir örnek aşağıda verilmiştir:

```csharp
string query = "SELECT * FROM digitaltwins";
try
{
    AsyncPageable<string> qresult = client.QueryAsync(query);
    await foreach (string item in qresult) 
    {
        // Do something with each result
    }
}
catch (RequestFailedException e)
{
    Log.Error($"Error {e.Status}: {e.Message}");
    return null;
}
```

## <a name="query-based-on-relationships"></a>İlişkileri temel alan sorgu

Dijital TWINS ' ilişkilerine göre sorgulama yaparken, Azure Digital TWINS sorgu deposu dilinin özel bir sözdizimi vardır.

İlişkiler, yan tümcesindeki sorgu kapsamına çekilir `FROM` . "Klasik" SQL-Type dillerinden önemli bir ayrım, bu `FROM` yan tümcedeki her bir ifadenin bir tablo olmaması değildir; Bunun yerine `FROM` yan tümce bir çapraz varlık ilişki geçişini ifade eder ve Azure Digital TWINS sürümü ile yazılır `JOIN` . 

Azure dijital TWINS [modeli](concepts-models.md) özellikleri ile, ilişkilerin, TWINS 'den bağımsız olarak mevcut olmadığını geri çekin. Bu, Azure Digital TWINS sorgu deposu dilinin `JOIN` Genel SQL 'den biraz farklı olduğu anlamına gelir `JOIN` , burada ilişkiler bağımsız olarak sorgulanamaz ve bir ikizi bağlı olması gerekir.
Bu farkı eklemek için, `RELATED` `JOIN` yan tümcesinde ikizi 'in ilişki kümesine başvurmak için kullanılır. 

Aşağıdaki bölümde bunun nasıl göründüğü hakkında birkaç örnek verilmiştir.

> [!TIP]
> Kavramsal olarak, bu özellik, CosmosDB 'nin belge merkezli işlevselliğini taklit eder ve burada `JOIN` bir belge içindeki alt nesneler üzerinde gerçekleştirilebilir. CosmosDB, `IN` `JOIN` geçerli bağlam belgesi içindeki dizi öğelerini yinelemek için tasarlanan anahtar sözcüğünü kullanır.

### <a name="relationship-based-query-examples"></a>İlişki tabanlı sorgu örnekleri

İlişkiler içeren bir veri kümesini almak için, bir deyimi ve `FROM` ardından N deyimlerini kullanın `JOIN` , burada `JOIN` deyimler bir Previous veya deyimin sonucu üzerinde ilişki alır `FROM` `JOIN` .

Örnek bir ilişki tabanlı sorgu aşağıda verilmiştir. Bu kod parçacığı, ' ABC ' öğesinin *ID* özelliğine sahip tüm dijital TWINS 'leri ve bu dijital TWINS ile ilgili tüm dijital TWINS 'leri, bir *içerir* ilişki aracılığıyla seçer. 

```sql
SELECT T, CT
FROM DIGITALTWINS T
JOIN CT RELATED T.contains
WHERE T.$dtId = 'ABC' 
```

>[!NOTE] 
> Geliştiricinin `JOIN` yan tümcesindeki anahtar değeriyle ilişkilendirilmesi gerekmez `WHERE` (veya tanımıyla birlikte satır içi bir anahtar değeri belirtebilirsiniz `JOIN` ). İlişki özelliklerinin kendisi hedef varlığı tanımlarında, bu bağıntı sistem tarafından otomatik olarak hesaplanır.

### <a name="query-the-properties-of-a-relationship"></a>Bir ilişkinin özelliklerini sorgulama

Benzer şekilde, dijital TWINS 'nin DTDL aracılığıyla tanımlanan özellikleri vardır, ilişkilerin de özellikleri olabilir. Azure Digital TWINS sorgu deposu dili, yan tümce içindeki ilişkiye bir diğer ad atayarak ilişkilerin filtrelenmesini ve projeksiyonunu sağlar `JOIN` . 

Örnek olarak, *Reportedcondition* özelliği olan bir *servicedBy* ilişkisini göz önünde bulundurun. Aşağıdaki sorguda, özelliğine başvurmak için bu ilişkiye ' R ' diğer adı verilir.

```sql
SELECT T, SBT, R
FROM DIGITALTWINS T
JOIN SBT RELATED T.servicedBy R
WHERE T.$dtId = 'ABC' 
AND R.reportedCondition = 'clean'
```

Yukarıdaki örnekte, *Reportedcondition* 'ın *servicedBy* ilişkisinin kendisini ( *servicedBy* ilişkisine sahip bir Digital ikizi değil) nasıl bir özelliği olduğunu not edin.

### <a name="query-limitations"></a>Sorgu sınırlamaları

Örneğinizdeki değişiklikler sorgularda yansıtılmadan önce 10 saniyeye kadar bir gecikme olabilir. Örneğin, Digitaltwıns API 'SI ile TWINS oluşturma veya silme gibi bir işlemi tamamladıysanız, sonuç sorgu API 'si isteklerinde hemen yansıtılmayabilir. Bir kısa dönemin beklenmesi, çözülmesi için yeterli olmalıdır.

Önizleme sırasında kullanmayla ilgili ek sınırlamalar vardır `JOIN` .
* İfadesinde hiçbir alt sorgu desteklenmez `FROM` .
* `OUTER JOIN`semantikler desteklenmez, yani ilişkinin sıfır sıralaması varsa, tüm "satır" çıkış sonuç kümesinden kaldırılır.
* Genel Önizleme sırasında grafik çapraz geçiş derinliği kısıtlıdır: `JOIN` sorgu başına yalnızca birine izin verilir.
* `JOIN`İşlem kaynağı kısıtlanmış: sorgu, sorgunun başladığı TWINS 'i bildirmelidir.

## <a name="query-best-practices"></a>En iyi sorgu yöntemleri

Azure dijital TWINS ile sorgulama için bazı ipuçları aşağıda verilmiştir.

* Model tasarım aşamasında sorgu modelini göz önünde bulundurun. Tek bir sorguda yanıtlanması gereken ilişkilerin tek düzeyli ilişki olarak modellendirildiğinden emin olun.
* Özellikleri grafik çapraz geçişinin büyük sonuç kümelerinden kaçınacak şekilde tasarlayın.
* Bir TWINS dizisi oluşturup işleçle sorgulama yaparak ihtiyacınız olan sorguların sayısını önemli ölçüde azaltabilirsiniz `IN` . Örneğin, *binaların* *katlara* ve *katlara* *Oda*içerdiği bir senaryoyu düşünün. Sık kullanılan bir bina içindeki odaları aramak için şunları yapabilirsiniz:

    1. İlişkiye göre binaları bulun `contains`
        ```sql
        SELECT Floor
        FROM DIGITALTWINS Building
        JOIN Floor RELATED Building.contains
        WHERE Building.$dtId = @buildingId
        ``` 
    2. Odaları bulmak için, tek tek bir kat katınızı göz önünde bulundurarak ve `JOIN` her birinin odasını bulmak üzere bir sorgu çalıştırırken, binadaki katların bir koleksiyonu ile sorgulayabilirsiniz (aşağıdaki sorguda yer alarak adlandırılmış *taban* ).

        İstemci uygulaması:
        ```csharp
        var floors = "['floor1','floor2', ..'floorn']"; 
        ```
        Sorguda:
        ```sql
        SELECT Room
        FROM DIGITALTWINS Floor
        JOIN Room RELATED Floor.contains
        WHERE Floor.$dtId IN ['floor1','floor2', ..'floorn']
        AND Room. Temperature > 72
        AND IS_OF_MODEL(Room, 'dtmi:com:contoso:Room;1')
        ```
* Özellik adları ve değerleri büyük/küçük harfe duyarlıdır, bu nedenle modellerdeki tanımlı tam adları kullanın. Özellik adları yanlış yazılmıştır veya yanlış bir şekilde ayarlandıysa, sonuç kümesi hiçbir hata döndürülmeden boştur.


## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki sorguları çalıştırmak için kullanılan sorgu API 'SI de dahil olmak üzere [Azure Digital TWINS API 'leri ve SDK 'ları](how-to-use-apis-sdks.md)hakkında daha fazla bilgi edinin.
