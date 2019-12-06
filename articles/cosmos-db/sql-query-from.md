---
title: Azure Cosmos DB FROM yan tümcesi
description: SQL söz dizimi ve Azure Cosmos DB for FROM yan tümcesinin örneği hakkında bilgi edinin. Bu makalede ayrıca, FROM yan tümcesini kullanarak, kapsam sonuçları için örnekler gösterilmektedir ve alt öğeleri elde edebilirsiniz.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 4f6d7580ea7ff0e8968c0c3ce4b3ca6111c86ac8
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873378"
---
# <a name="from-clause-in-azure-cosmos-db"></a>Azure Cosmos DB FROM yan tümcesi

Kaynak filtrelenmez veya sorgu daha sonra yansıtılmamışsa FROM (`FROM <from_specification>`) yan tümcesi isteğe bağlıdır. `SELECT * FROM Families` gibi bir sorgu, tüm `Families` kapsayıcısının üzerinde numaralandırılır. Kapsayıcı adını kullanmak yerine kapsayıcı için özel tanımlayıcı kökünü de kullanabilirsiniz.

FROM yan tümcesi sorgu başına aşağıdaki kuralları uygular:

* Kapsayıcı gibi diğer adı, olabilir `SELECT f.id FROM Families AS f` ya da yalnızca `SELECT f.id FROM Families f`. Burada `f` `Families`diğer addır. , Tanımlayıcı [diğer adı](sql-query-aliasing.md) için isteğe bağlı bir anahtar sözcüktür.  

* Diğer ad eklendikten sonra özgün kaynak adı bağlanamaz. Örneğin, tanımlayıcı `Families` başka bir ad olduğu ve artık çözümlenemediği için `SELECT Families.id FROM Families f` sözdizimsel olarak geçersizdir.  

* Kesin şema bağlılığı yokluğunda belirsiz bağlamalardan kaçınmak için, tüm başvurulan özellikler tam olarak nitelenmelidir. Örneğin, özellik `id` bağlanmadığından `SELECT id FROM Families f` sözdizimsel olarak geçersizdir.

## <a name="syntax"></a>Sözdizimi
  
```sql  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <container_expression> [[AS] input_alias]  
        | input_alias IN <container_expression>  
  
<container_expression> ::=   
        ROOT   
     | container_name  
     | input_alias  
     | <container_expression> '.' property_name  
     | <container_expression> '[' "property_name" | array_index ']'  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<from_source>`  
  
  Bir veri kaynağı olan veya olmayan bir diğer ad belirtir. Diğer ad belirtilmezse, içinden gösterilen `<container_expression>` kuralları kullanarak:  
  
  -  İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
  -  İfade ise `<container_expression>`, property_name sonra property_name diğer ad olarak kullanılır. İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
- FARKLI `input_alias`  
  
  Belirten `input_alias` temel alınan kapsayıcı ifadesi tarafından döndürülen değerler kümesidir.  
 
- `input_alias` GİRİŞ  
  
  Belirten `input_alias` temel alınan kapsayıcı ifadesi tarafından döndürülen her dizinin tüm dizi öğeleri üzerinde yineleme tarafından alınan değerler kümesini temsil etmelidir. Bir dizi değil temel alınan kapsayıcı ifadesi tarafından döndürülen herhangi bir değer yoksayılır.  
  
- `<container_expression>`  
  
  Belgeleri almak için kullanılacak kapsayıcı ifadesini belirtir.  
  
- `ROOT`  
  
  Bu belge, varsayılan, bağlı durumda kapsayıcı alınması gerektiğini belirtir.  
  
- `container_name`  
  
  Bu belgede sağlanan kapsayıcıdan alınacağını belirtir. Kapsayıcının adı şu anda bağlı kapsayıcısının adı eşleşmelidir.  
  
- `input_alias`  
  
  Bu belgede sağlanan diğer ad tarafından tanımlanan diğer kaynaktan alınması gerektiğini belirtir.  
  
- `<container_expression> '.' property_`  
  
  Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen kapsayıcı ifade.  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen kapsayıcı ifade.  
  
## <a name="remarks"></a>Açıklamalar
  
Tüm diğer adları içinde çıkarımı yapılan veya sağlanan `<from_source>(`s) benzersiz olmalıdır. Söz dizimi `<container_expression>.`property_name aynıdır `<container_expression>' ['"property_name"']'`. Ancak, bir özellik adı bir tanımlayıcı olmayan karakter içeriyorsa, ikinci sözdizimi kullanılabilir.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>Dizi öğeleri yanı sıra, tanımsız değerler eksik özellikler eksik işleme
  
Bir kapsayıcı ifadesi özellikleri veya dizi öğeleri ve değeri yok erişirse, bu değer yoksayılır ve daha fazla işlenmedi.  
  
### <a name="container-expression-context-scoping"></a>Kapsayıcı bağlamı ifadesi kapsamı  
  
Bir kapsayıcı ifade kapsayıcı kapsamlı veya belge kapsamlı olabilir:  
  
-   Kapsayıcı-kapsayıcı ifadenin temel alınan kaynak ya da kök ise kapsamı, bir ifade veya `container_name`. Böyle bir ifade, bir kapsayıcıdan doğrudan alınan belge kümesini temsil eder ve diğer kapsayıcı ifadeler işleme bağımlı değildir.  
  
-   Bir ifade belge temel alınan kaynak kapsayıcı ifadenin ise kapsamlı, `input_alias` sorgu daha önce sunulan. Böyle bir ifade, diğer adlı kapsayıcı ile ilişkili bir kümeye ait her bir belgenin kapsamında kapsayıcı ifadesinin değerlendirilmesi elde belgeleri kümesini temsil eder.  Sonuç kümesi, her bir temel alınan belgeler için kapsayıcı ifadesinin değerlendirilmesi elde edilen kümeleri birleşimi olacaktır. 

## <a name="examples"></a>Örnekler

### <a name="get-subitems-by-using-the-from-clause"></a>FROM yan tümcesini kullanarak alt öğeleri Al

FROM yan tümcesi, kaynağı daha küçük bir alt kümeye düşürebilir. Her bir öğedeki yalnızca bir alt ağacı numaralandırmak için, aşağıdaki örnekte gösterildiği gibi alt kök kaynak olabilir:

```sql
    SELECT *
    FROM Families.children
```

Sonuçlar:

```json
    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
       {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Yukarıdaki sorgu kaynak olarak bir dizi kullandı, ancak kaynak olarak bir nesnesi de kullanabilirsiniz. Sorgu, sonuca eklenmek üzere kaynakta geçerli, tanımlı herhangi bir JSON değerini dikkate alır. Aşağıdaki örnek, bir `address.state` değeri olmayan `Families` dışarıda bırakır.

```sql
    SELECT *
    FROM Families.address.state
```

Sonuçlar:

```json
    [
      "WA",
      "NY"
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [WHERE yan tümcesi](sql-query-where.md)