---
title: Azure Cosmos DB FROM yan tümcesi
description: SQL söz dizimi ve Azure Cosmos DB for FROM yan tümcesinin örneği hakkında bilgi edinin. Bu makalede ayrıca, FROM yan tümcesini kullanarak, kapsam sonuçları için örnekler gösterilmektedir ve alt öğeleri elde edebilirsiniz.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: tisande
ms.openlocfilehash: e4bbb27a2f49027ed5a456ad824f54b9c92a899c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83005868"
---
# <a name="from-clause-in-azure-cosmos-db"></a>Azure Cosmos DB FROM yan tümcesi

`FROM <from_specification>`Kaynak filtrelenmez veya sorgu daha sonra yansıtılmamışsa from () yan tümcesi isteğe bağlıdır. `SELECT * FROM Families`Kapsayıcının tamamına göre numaralandırmış bir sorgu `Families` . Kapsayıcı adını kullanmak yerine kapsayıcı için özel tanımlayıcı kökünü de kullanabilirsiniz.

`FROM`Yan tümce sorgu başına aşağıdaki kuralları uygular:

* Kapsayıcı, veya gibi diğer ad olabilir `SELECT f.id FROM Families AS f` `SELECT f.id FROM Families f` . `f`Diğer adı aşağıda verilmiştir `Families` . , Tanımlayıcı [diğer adı](sql-query-working-with-json.md#aliasing) için isteğe bağlı bir anahtar sözcüktür.  

* Diğer ad eklendikten sonra özgün kaynak adı bağlanamaz. Örneğin, `SELECT Families.id FROM Families f` tanımlayıcı `Families` başka bir ad olduğu ve artık çözümlenemediği için sözdizimsel olarak geçersiz.  

* Kesin şema bağlılığı yokluğunda belirsiz bağlamalardan kaçınmak için, tüm başvurulan özellikler tam olarak nitelenmelidir. Örneğin, `SELECT id FROM Families f` özellik bağlanmadığından sözdizimsel olarak geçersiz `id` .

## <a name="syntax"></a>Söz dizimi
  
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
  
## <a name="arguments"></a>Bağımsız değişkenler
  
- `<from_source>`  
  
  Diğer adı olan veya olmayan bir veri kaynağını belirtir. Diğer ad belirtilmemişse, `<container_expression>` aşağıdaki kurallar kullanılarak çıkarsedilir:  
  
-  İfade bir container_name ise, container_name bir diğer ad olarak kullanılacaktır.  
  
-  İfade ise `<container_expression>` property_name, property_name bir diğer ad olarak kullanılır. İfade bir container_name ise, container_name bir diğer ad olarak kullanılacaktır.  
  
- GEREKTIĞI`input_alias`  
  
  `input_alias`Öğesinin, temel alınan kapsayıcı ifadesinin döndürdüğü bir değer kümesi olduğunu belirtir.  
 
- `input_alias`'NDAKI  
  
  `input_alias`Öğesinin, temel alınan kapsayıcı ifadesi tarafından döndürülen her bir dizinin tüm dizi öğeleri üzerinde yinelenerek elde edilen değer kümesini temsil etmesi gerektiğini belirtir. Bir dizi olmayan temeldeki kapsayıcı ifadesi tarafından döndürülen herhangi bir değer yok sayılır.  
  
- `<container_expression>`  
  
  Belgeleri almak için kullanılacak kapsayıcı ifadesini belirtir.  
  
- `ROOT`  
  
  Belgenin varsayılan, bağlı olan kapsayıcıdan alınması gerektiğini belirtir.  
  
- `container_name`  
  
  Belgenin, belirtilen kapsayıcıdan alınması gerektiğini belirtir. Kapsayıcının adı, şu anda bağlı olan kapsayıcının adıyla eşleşmelidir.  
  
- `input_alias`  
  
  Belgenin, belirtilen diğer ad tarafından tanımlanan diğer kaynaktan alınması gerektiğini belirtir.  
  
- `<container_expression> '.' property_name`  
  
  Özelliğe erişerek belgenin alınması gerektiğini belirtir `property_name` .  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  `property_name`Belirtilen kapsayıcı ifadesi tarafından alınan tüm belgeler için özelliğe veya array_index dizi öğesine erişerek belgenin alınması gerektiğini belirtir.  
  
## <a name="remarks"></a>Açıklamalar
  
İçinde belirtilen veya gösterilen tüm diğer adlar `<from_source>(` benzersiz olmalıdır. Property_name sözdizimi, `<container_expression>.` ile aynıdır `<container_expression>' ['"property_name"']'` . Ancak, bir özellik adı bir tanımlayıcı olmayan karakter içeriyorsa, ikinci sözdizimi kullanılabilir.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>Eksik özellikleri işleme, eksik dizi öğeleri ve tanımsız değerler
  
Bir kapsayıcı ifadesi özelliklere veya dizi öğelerine erişirse ve bu değer yoksa, bu değer yok sayılır ve daha fazla işlenmez.  
  
### <a name="container-expression-context-scoping"></a>Kapsayıcı ifadesi bağlam kapsamı  
  
Kapsayıcı ifadesi kapsayıcı kapsamlı veya belge kapsamlı olabilir:  
  
- Kapsayıcı ifadesinin temeldeki kaynağı kök veya varsa, bir ifade kapsayıcı kapsamıdır `container_name` . Böyle bir ifade, doğrudan kapsayıcıdan alınan bir belge kümesini temsil eder ve diğer kapsayıcı ifadelerinin işlenmesine bağlı değildir.  
  
- Bir ifade, kapsayıcı ifadesinin temel alınan kaynağı `input_alias` sorguda daha önce tanıtıldığında belge kapsamlı bir ifadedir. Böyle bir ifade, diğer ad kapsayıcında ilişkili olan her belge kapsamındaki kapsayıcı ifadesi hesaplanarak elde edilen bir belge kümesini temsil eder. Sonuç kümesi, temel alınan küme içindeki her belge için kapsayıcı ifadesi hesaplanarak elde edilen bir küme birleşimidir.

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

Yukarıdaki sorgu kaynak olarak bir dizi kullandı, ancak kaynak olarak bir nesnesi de kullanabilirsiniz. Sorgu, sonuca eklenmek üzere kaynakta geçerli, tanımlı herhangi bir JSON değerini dikkate alır. Aşağıdaki örnek, `Families` bir değeri olmayan dışında tutulacak `address.state` .

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
