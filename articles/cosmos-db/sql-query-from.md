---
title: Azure Cosmos DB'deki FROM yan tümcesi
description: SQL sözdizimi hakkında bilgi edinin ve Azure Cosmos DB için FROM yan tümcesi için örnek olun. Bu makalede, kapsam sonuçlarına örnekler de gösterilmektedir ve FROM yan tümcesini kullanarak alt öğeleri alın.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 3939594064b63c567720378b9d316acca64d3266
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77587694"
---
# <a name="from-clause-in-azure-cosmos-db"></a>Azure Cosmos DB'deki FROM yan tümcesi

KAYNAK filtre`FROM <from_specification>`uygulanmadığı veya sorguda daha sonra yansıtılmadığı sürece FROM ( ) yan tümcesi isteğe bağlıdır. Gibi bir `SELECT * FROM Families` sorgu tüm `Families` kapsayıcı üzerinde sayısallar. Kapsayıcı adını kullanmak yerine kapsayıcı için özel tanımlayıcı ROOT'u da kullanabilirsiniz.

FROM yan tümcesi sorgu başına aşağıdaki kuralları uygular:

* Kapsayıcı gibi `SELECT f.id FROM Families AS f` veya basitçe `SELECT f.id FROM Families f`diğer adlar olabilir. Burada `f` diğer adı `Families`. AS tanımlayıcı [diğer adı](sql-query-aliasing.md) için isteğe bağlı bir anahtar kelimedir.  

* Takma ad landıktan sonra, özgün kaynak adı bağlanamaz. Örneğin, `SELECT Families.id FROM Families f` tanımlayıcı diğer adı olduğundan ve artık çözülemediğinden sözdizimi geçersizdir. `Families`  

* Kesin şema yapışması yokluğunda belirsiz ciltlemeleri önlemek için, başvurulan tüm özelliklerin tam olarak nitelikli olması gerekir. Örneğin, `SELECT id FROM Families f` özellik `id` bağlı olmadığından sözdizimsel olarak geçersizdir.

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
  
  Takma adlı veya takma adsız bir veri kaynağını belirtir. Diğer ad belirtilmemişse, aşağıdaki `<container_expression>` kurallardan çıkarılır:  
  
  -  İfade container_name ise, container_name takma ad olarak kullanılır.  
  
  -  İfade `<container_expression>`, property_name ise, property_name takma ad olarak kullanılacaktır. İfade container_name ise, container_name takma ad olarak kullanılır.  
  
- Olarak`input_alias`  
  
  Alttaki kapsayıcı `input_alias` ifadesi tarafından döndürülen bir değer kümesi olduğunu belirtir.  
 
- `input_alias`Inç  
  
  Temel kapsayıcı ifadesi `input_alias` tarafından döndürülen her dizinin tüm dizi öğeleri üzerinde yineleyerek elde edilen değerler kümesini temsil etmesi gerektiğini belirtir. Bir dizi olmayan altbilgi ifadesi tarafından döndürülen herhangi bir değer yoksayılır.  
  
- `<container_expression>`  
  
  Belgeleri almak için kullanılacak kapsayıcı ifadesini belirtir.  
  
- `ROOT`  
  
  Bu belgenin varsayılan olarak bağlı olan kapsayıcıdan alınması gerektiğini belirtir.  
  
- `container_name`  
  
  Bu belgenin sağlanan kapsayıcıdan alınması gerektiğini belirtir. Kapsayıcının adı, şu anda bağlı olan kapsayıcının adıyla eşleşmelidir.  
  
- `input_alias`  
  
  Bu belgenin sağlanan diğer ad tarafından tanımlanan diğer kaynaktan alınması gerektiğini belirtir.  
  
- `<container_expression> '.' property_name`  
  
  Bu belgenin `property_name` özelliğe erişilerek alınması gerektiğini belirtir.  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  Belirtilen kapsayıcı ifadesi tarafından alınan tüm belgeler `property_name` için özellik veya array_index dizi öğesierişerek bu belgenin alınması gerektiğini belirtir.  
  
## <a name="remarks"></a>Açıklamalar
  
Sağlanan veya `<from_source>(`s) çıkarılan tüm diğer adlar benzersiz olmalıdır. Sözdizimi `<container_expression>.`property_name ile `<container_expression>' ['"property_name"']'`aynıdır. Ancak, bir özellik adı tanımlayıcı olmayan bir karakter içeriyorsa, ikinci sözdizimi kullanılabilir.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>Eksik özellikleri, eksik dizi öğelerini ve tanımlanmamış değerleri işleme
  
Bir kapsayıcı ifadesi özelliklere veya dizi öğelerine erişiyorsa ve bu değer yoksa, bu değer yoksayılacak ve daha fazla işlenmez.  
  
### <a name="container-expression-context-scoping"></a>Kapsayıcı ifade bağlamı kapsam  
  
Kapsayıcı ifadesi kapsayıcı kapsamında veya belge kapsamı içinde olabilir:  
  
-   Kapsayıcı ifadesinin altında yatan kaynak ROOT veya `container_name`. Böyle bir ifade, doğrudan kapsayıcıdan alınan bir belge kümesini temsil eder ve diğer kapsayıcı ifadelerinin işlenmesine bağlı değildir.  
  
-   Kapsayıcı ifadesinin altında yatan kaynak sorguda daha önce `input_alias` tanıtılırsa, bir ifade belge kapsamındadır. Böyle bir ifade, diğer adla eklenen kapsayıcıyla ilişkili kümeye ait her belgenin kapsamındaki kapsayıcı ifadesini değerlendirerek elde edilen bir belge kümesini temsil eder.  Elde edilen küme, temel kümedeki her belge için kapsayıcı ifadesi değerlendirilerek elde edilen kümeler birleşimi olacaktır. 

## <a name="examples"></a>Örnekler

### <a name="get-subitems-by-using-the-from-clause"></a>FROM yan tümcesini kullanarak alt öğeleri alın

FROM yan tümcesi kaynağı daha küçük bir alt kümeye indirebilir. Her öğede yalnızca bir alt ağacı sıralamak için, alt kök aşağıdaki örnekte gösterildiği gibi kaynak olabilir:

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

Önceki sorgu kaynak olarak bir dizi kullanılır, ancak kaynak olarak bir nesne de kullanabilirsiniz. Sorgu, sonuca eklenmesi için kaynaktaki geçerli, tanımlanmış JSON değerini dikkate alır. Aşağıdaki örnek, `Families` değeri olmayanları `address.state` dışlar.

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
