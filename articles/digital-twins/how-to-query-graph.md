---
title: İkiz grafiğini sorgulama
titleSuffix: Azure Digital Twins
description: Bilgi için bkz. Azure dijital TWINS ikizi grafiğini sorgulama.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: e6236d9ed5ed75b6b5e10914e668de545c48fc2c
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89055643"
---
# <a name="query-the-azure-digital-twins-twin-graph"></a>Azure Digital TWINS ikizi grafiğini sorgulama

Bu makalede, bilgi için [ikizi grafiğini](concepts-twins-graph.md) sorgulamak üzere [Azure Digital TWINS sorgu dilinin](concepts-query-language.md) kullanımıyla ilgili örnekler ve daha ayrıntılı bilgiler sunulmaktadır. Azure Digital TWINS [**sorgu API 'lerini**](how-to-use-apis-sdks.md)kullanarak grafikteki sorguları çalıştırırsınız.

[!INCLUDE [digital-twins-query-operations.md](../../includes/digital-twins-query-operations.md)]

Bu makalenin geri kalanında, bu işlemlerin nasıl kullanılacağına ilişkin örnekler verilmektedir.

## <a name="query-syntax"></a>Sorgu söz dizimi

Bu bölüm, sorgu dili yapısını gösteren örnek sorgular içerir ve [dijital TWINS](concepts-twins-graph.md)üzerinde olası sorgu işlemleri gerçekleştirir.

### <a name="select-top-items"></a>En üstteki öğeleri seç

Yan tümcesini kullanarak bir sorgudaki birkaç "üst" öğeyi seçebilirsiniz `Select TOP` .

```sql
SELECT TOP (5)
FROM DIGITALTWINS
WHERE ...
```

### <a name="query-by-property"></a>Özelliğe göre sorgu

**Özelliklere** göre dijital TWINS al (kimlik ve meta veriler dahil):
```sql
SELECT  * 
FROM DigitalTwins T  
WHERE T.firmwareVersion = '1.1'
AND T.$dtId in ['123', '456']
AND T.Temperature = 70
```

> [!TIP]
> Dijital bir ikizi KIMLIĞI, meta veri alanı kullanılarak sorgulanır `$dtId` .

Ayrıca, **belirli bir özelliğin tanımlanıp tanımlanmadığına**göre TWINS de alabilirsiniz. Tanımlı bir *konum* özelliği olan TWINS 'i alan bir sorgu aşağıda verilmiştir:

```sql
SELECT *
FROM DIGITALTWINS WHERE IS_DEFINED(Location)
```

Bu, [dijital TWINS 'e etiket ekleme](how-to-use-tags.md)başlığı altında açıklandığı gibi, kendi *etiket* özelliklerine göre TWINS 'leri almanıza yardımcı olabilir. *Kırmızı*ile etiketlenmiş tüm TWINS 'leri alan bir sorgu aşağıda verilmiştir:

```sql
select * from digitaltwins where is_defined(tags.red) 
```

Ayrıca, **bir özelliğin türüne**göre TWINS de edinebilirsiniz. *Sıcaklık* özelliği bir sayı olan TWINS 'i alan bir sorgu aşağıda verilmiştir:

```sql
SELECT * FROM DIGITALTWINS T
WHERE IS_NUMBER(T.Temperature)
```

### <a name="query-by-model"></a>Modele göre sorgu

`IS_OF_MODEL`İşleci, ikizi 'in [**modeline**](concepts-models.md)göre filtrelemek için kullanılabilir. Devralmayı destekler ve birkaç aşırı yükleme seçeneği içerir.

En basit kullanımı `IS_OF_MODEL` yalnızca bir parametre alır `twinTypeName` : `IS_OF_MODEL(twinTypeName)` .
Bu parametreye bir değer geçiren sorgu örneği aşağıda verilmiştir:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:sample:thing;1')
```

Birden fazla olduğunda (örneğin kullanıldığında) arama yapılacak bir ikizi koleksiyonu belirtmek için `JOIN` , `twinCollection` parametresini ekleyin: `IS_OF_MODEL(twinCollection, twinTypeName)` .
Bu parametre için bir değer ekleyen bir sorgu örneği aşağıda verilmiştir:

```sql
SELECT * FROM DIGITALTWINS DT WHERE IS_OF_MODEL(DT, 'dtmi:sample:thing;1')
```

Tam eşleşme yapmak için şu `exact` parametreyi ekleyin: `IS_OF_MODEL(twinTypeName, exact)` .
Bu parametre için bir değer ekleyen bir sorgu örneği aşağıda verilmiştir:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:sample:thing;1', exact)
```

Üç bağımsız değişkeni de bir araya geçirebilirsiniz: `IS_OF_MODEL(twinCollection, twinTypeName, exact)` .
Aşağıda, üç parametre için bir değer belirten bir sorgu örneği verilmiştir:

```sql
SELECT ROOM FROM DIGITALTWINS DT WHERE IS_OF_MODEL(DT, 'dtmi:sample:thing;1', exact)
```

### <a name="query-based-on-relationships"></a>İlişkileri temel alan sorgu

Dijital TWINS ' **ilişkilerine**göre sorgulama yaparken Azure Digital TWINS sorgu dilinin özel bir sözdizimi vardır.

İlişkiler, yan tümcesindeki sorgu kapsamına çekilir `FROM` . "Klasik" SQL-Type dillerinden önemli bir ayrım, bu `FROM` yan tümcedeki her bir ifadenin bir tablo olmaması değildir; Bunun yerine `FROM` yan tümce bir çapraz varlık ilişki geçişini ifade eder ve Azure Digital TWINS sürümü ile yazılır `JOIN` . 

Azure dijital TWINS [modeli](concepts-models.md) özellikleri ile, ilişkilerin, TWINS 'den bağımsız olarak mevcut olmadığını geri çekin. Bu, Azure Digital TWINS sorgu dilinin `JOIN` Genel SQL 'den biraz farklı olduğu anlamına gelir `JOIN` , burada ilişkiler bağımsız olarak sorgulanamaz ve bir ikizi bağlı olması gerekir.
Bu farkı eklemek için, `RELATED` `JOIN` yan tümcesinde ikizi 'in ilişki kümesine başvurmak için kullanılır. 

Aşağıdaki bölümde bunun nasıl göründüğü hakkında birkaç örnek verilmiştir.

> [!TIP]
> Kavramsal olarak, bu özellik, CosmosDB 'nin belge merkezli işlevselliğini taklit eder ve burada `JOIN` bir belge içindeki alt nesneler üzerinde gerçekleştirilebilir. CosmosDB, `IN` `JOIN` geçerli bağlam belgesi içindeki dizi öğelerini yinelemek için tasarlanan anahtar sözcüğünü kullanır.

#### <a name="relationship-based-query-examples"></a>İlişki tabanlı sorgu örnekleri

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

#### <a name="query-the-properties-of-a-relationship"></a>Bir ilişkinin özelliklerini sorgulama

Benzer şekilde, dijital TWINS 'nin DTDL aracılığıyla tanımlanan özellikleri vardır, ilişkilerin de özellikleri olabilir. , **İlişkilerinin özelliklerine göre**TWINS 'i sorgulayabilirsiniz.
Azure Digital TWINS sorgu dili, yan tümce içindeki ilişkiye bir diğer ad atayarak ilişkilerin filtrelenmesini ve projeksiyonunu sağlar `JOIN` . 

Örnek olarak, *Reportedcondition* özelliği olan bir *servicedBy* ilişkisini göz önünde bulundurun. Aşağıdaki sorguda, özelliğine başvurmak için bu ilişkiye ' R ' diğer adı verilir.

```sql
SELECT T, SBT, R
FROM DIGITALTWINS T
JOIN SBT RELATED T.servicedBy R
WHERE T.$dtId = 'ABC' 
AND R.reportedCondition = 'clean'
```

Yukarıdaki örnekte, *Reportedcondition* 'ın *servicedBy* ilişkisinin kendisini ( *servicedBy* ilişkisine sahip bir Digital ikizi değil) nasıl bir özelliği olduğunu not edin.

### <a name="query-with-multiple-joins"></a>Birden çok birleştirme içeren sorgu

Şu anda önizleme aşamasında olan en fazla beş `JOIN` sn tek bir sorguda desteklenir. Bu, aynı anda birden çok ilişki seviyelerine çapraz geçiş yapmanıza olanak sağlar.

Burada, Oda 1 ve 2 ' deki açık panellerde bulunan tüm hafif bultların yer aldığı çok sayıda JOIN sorgusuna bir örnek verilmiştir.

```sql
SELECT LightBulb 
FROM DIGITALTWINS Room 
JOIN LightPanel RELATED Room.contains 
JOIN LightBulb RELATED LightPanel.contains 
WHERE IS_OF_MODEL(LightPanel, 'dtmi:contoso:com:lightpanel;1') 
AND IS_OF_MODEL(LightBulb, 'dtmi:contoso:com:lightbulb ;1') 
AND Room.$dtId IN ['room1', 'room2'] 
```

### <a name="other-compound-query-examples"></a>Diğer bileşik sorgu örnekleri

Tek bir sorguda daha fazla ayrıntı dahil etmek için, birleşim işleçlerini kullanarak yukarıdaki sorgu türlerinden herhangi birini **birleştirebilirsiniz** . Aynı anda birden fazla ikizi tanımlayıcısı için sorgu oluşturan bileşik sorguların bazı ek örnekleri aşağıda verilmiştir.

| Açıklama | Sorgu |
| --- | --- |
| *Oda 123* ' nin sahip olduğu cihazların dışında, işleç rolüne sunan mxyonga cihazlarını döndürün | `SELECT device`<br>`FROM DigitalTwins space`<br>`JOIN device RELATED space.has`<br>`WHERE space.$dtid = 'Room 123'`<br>`AND device.$metadata.model = 'dtmi:contosocom:DigitalTwins:MxChip:3'`<br>`AND has.role = 'Operator'` |
| KIMLIĞI *ID1* olan başka bir Ikizi ile *Contains* adlı bir ilişkiye sahip olan TWINS 'i alma | `SELECT Room`<br>`FROM DIGITIALTWINS Room`<br>`JOIN Thermostat ON Room.Contains`<br>`WHERE Thermostat.$dtId = 'id1'` |
| Bu oda modelinin *floor11* tarafından bulunan tüm odalarına ulaşın | `SELECT Room`<br>`FROM DIGITALTWINS Floor`<br>`JOIN Room RELATED Floor.Contains`<br>`WHERE Floor.$dtId = 'floor11'`<br>`AND IS_OF_MODEL(Room, 'dtmi:contosocom:DigitalTwins:Room;1')` |

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

## <a name="query-limitations"></a>Sorgu sınırlamaları

Örneğinizdeki değişiklikler sorgularda yansıtılmadan önce 10 saniyeye kadar bir gecikme olabilir. Örneğin, Digitaltwıns API 'SI ile TWINS oluşturma veya silme gibi bir işlemi tamamladıysanız, sonuç sorgu API 'si isteklerinde hemen yansıtılmayabilir. Bir kısa dönemin beklenmesi, çözülmesi için yeterli olmalıdır.

Önizleme sırasında kullanmayla ilgili ek sınırlamalar vardır `JOIN` .
* İfadesinde hiçbir alt sorgu desteklenmez `FROM` .
* `OUTER JOIN` semantikler desteklenmez, yani ilişkinin sıfır sıralaması varsa, tüm "satır" çıkış sonuç kümesinden kaldırılır.
* Önizleme süresince grafik çapraz geçiş derinliği `JOIN` sorgu başına beş düzey ile kısıtlıdır.
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
