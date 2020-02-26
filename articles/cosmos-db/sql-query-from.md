---
title: Azure Cosmos DB FROM yan tümcesi
description: SQL söz dizimi ve Azure Cosmos DB for FROM yan tümcesinin örneği hakkında bilgi edinin. Bu makalede ayrıca, FROM yan tümcesini kullanarak, kapsam sonuçları için örnekler gösterilmektedir ve alt öğeleri elde edebilirsiniz.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 3939594064b63c567720378b9d316acca64d3266
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587694"
---
# <a name="from-clause-in-azure-cosmos-db"></a>Azure Cosmos DB FROM yan tümcesi

Kaynak filtrelenmez veya sorgu daha sonra yansıtılmamışsa FROM (`FROM <from_specification>`) yan tümcesi isteğe bağlıdır. `SELECT * FROM Families` gibi bir sorgu, tüm `Families` kapsayıcısının üzerinde numaralandırılır. Kapsayıcı adını kullanmak yerine kapsayıcı için özel tanımlayıcı kökünü de kullanabilirsiniz.

FROM yan tümcesi sorgu başına aşağıdaki kuralları uygular:

* Kapsayıcı `SELECT f.id FROM Families AS f` veya yalnızca `SELECT f.id FROM Families f`gibi diğer ad olabilir. Burada `f` `Families`diğer addır. , Tanımlayıcı [diğer adı](sql-query-aliasing.md) için isteğe bağlı bir anahtar sözcüktür.  

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
  
  Bir veri kaynağı olan veya olmayan bir diğer ad belirtir. Diğer ad belirtilmemişse, aşağıdaki kurallar kullanılarak `<container_expression>` çıkarslacaktır:  
  
  -  İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
  -  İfade `<container_expression>`, property_name daha sonra property_name bir diğer ad olarak kullanılır. İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
- `input_alias` olarak  
  
  `input_alias`, temel alınan kapsayıcı ifadesinin döndürdüğü bir değer kümesi olduğunu belirtir.  
 
- `input_alias`  
  
  `input_alias`, temel alınan kapsayıcı ifadesi tarafından döndürülen her bir dizinin tüm dizi öğelerinde yineleme yaparak elde edilen değer kümesini temsil etmelidir. Bir dizi değil temel alınan kapsayıcı ifadesi tarafından döndürülen herhangi bir değer yoksayılır.  
  
- `<container_expression>`  
  
  Belgeleri almak için kullanılacak kapsayıcı ifadesini belirtir.  
  
- `ROOT`  
  
  Bu belge, varsayılan, bağlı durumda kapsayıcı alınması gerektiğini belirtir.  
  
- `container_name`  
  
  Bu belgede sağlanan kapsayıcıdan alınacağını belirtir. Kapsayıcının adı şu anda bağlı kapsayıcısının adı eşleşmelidir.  
  
- `input_alias`  
  
  Bu belgede sağlanan diğer ad tarafından tanımlanan diğer kaynaktan alınması gerektiğini belirtir.  
  
- `<container_expression> '.' property_name`  
  
  `property_name` özelliğine erişerek belgenin alınması gerektiğini belirtir.  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  Belirtilen kapsayıcı ifadesi tarafından alınan tüm belgeler için `property_name` özelliğine veya array_index dizi öğesine erişerek belgenin alınması gerektiğini belirtir.  
  
## <a name="remarks"></a>Açıklamalar
  
`<from_source>(`s) için belirtilen veya çıkarılan tüm diğer adlar benzersiz olmalıdır. `<container_expression>.`property_name sözdizimi `<container_expression>' ['"property_name"']'`ile aynıdır. Ancak, bir özellik adı bir tanımlayıcı olmayan karakter içeriyorsa, ikinci sözdizimi kullanılabilir.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>Dizi öğeleri yanı sıra, tanımsız değerler eksik özellikler eksik işleme
  
Bir kapsayıcı ifadesi özellikleri veya dizi öğeleri ve değeri yok erişirse, bu değer yoksayılır ve daha fazla işlenmedi.  
  
### <a name="container-expression-context-scoping"></a>Kapsayıcı bağlamı ifadesi kapsamı  
  
Bir kapsayıcı ifade kapsayıcı kapsamlı veya belge kapsamlı olabilir:  
  
-   Kapsayıcı ifadesinin temeldeki kaynağı kök veya `container_name`ise, bir ifade kapsayıcı kapsamıdır. Böyle bir ifade, bir kapsayıcıdan doğrudan alınan belge kümesini temsil eder ve diğer kapsayıcı ifadeler işleme bağımlı değildir.  
  
-   Bir ifade, kapsayıcı ifadesinin temel alınan kaynağı sorguda daha önce tanıtılan `input_alias`, belge kapsamıdır. Böyle bir ifade, diğer adlı kapsayıcı ile ilişkili bir kümeye ait her bir belgenin kapsamında kapsayıcı ifadesinin değerlendirilmesi elde belgeleri kümesini temsil eder.  Sonuç kümesi, her bir temel alınan belgeler için kapsayıcı ifadesinin değerlendirilmesi elde edilen kümeleri birleşimi olacaktır. 

## <a name="examples"></a>Örnekler

### <a name="get-subitems-by-using-the-from-clause"></a>FROM yan tümcesini kullanarak alt öğeleri Al

FROM yan tümcesi, kaynağı daha küçük bir alt kümeye düşürebilir. Her bir öğedeki yalnızca bir alt ağacı numaralandırmak için, aşağıdaki örnekte gösterildiği gibi alt kök kaynak olabilir:

```sql
    SELECT *
    FROM Families.children
```

Sonuçlar şunlardır:

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

Sonuçlar şunlardır:

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
