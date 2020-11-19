---
title: Kaynaklardaki dizi özellikleri için yazma ilkeleri
description: Dizi parametreleri ve dizi dili ifadeleriyle çalışmayı öğrenin, [*] diğer adını değerlendirin ve Azure Ilke tanımı kuralları ile öğeleri ekleyin.
ms.date: 10/22/2020
ms.topic: how-to
ms.openlocfilehash: 60044d4a599c14088ea923a6a14cb46543646995
ms.sourcegitcommit: 03c0a713f602e671b278f5a6101c54c75d87658d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94920466"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Azure kaynaklarında dizi özellikleri için yazma ilkeleri

Azure Resource Manager özellikler genellikle dizeler ve Boole değerleri olarak tanımlanır. Bire çok ilişkisi olduğunda, karmaşık özellikler bunun yerine diziler olarak tanımlanır. Azure Ilkesinde diziler birkaç farklı şekilde kullanılır:

- Birden çok seçenek sağlamak için bir [tanım parametresinin](../concepts/definition-structure.md#parameters)türü
- **Ya da** **notın** koşullarını kullanan bir [ilke kuralının](../concepts/definition-structure.md#policy-rule) bir parçası
- Değerlendirmek için [ \[ \* \] diğer adı](../concepts/definition-structure.md#understanding-the--alias) değerlendiren bir ilke kuralının parçası:
  - **Hiçbiri**, **Any** veya **Tümü** gibi senaryolar
  - **Count** ile karmaşık senaryolar
- Var olan bir diziyi değiştirmek veya eklemek için [ekleme efekti](../concepts/effects.md#append)

Bu makalede Azure Ilkesi tarafından kullanılan her kullanım ele alınmaktadır ve birkaç örnek tanım sunulmaktadır.

## <a name="parameter-arrays"></a>Parametre dizileri

### <a name="define-a-parameter-array"></a>Parametre dizisi tanımlama

Bir parametreyi dizi olarak tanımlamak, birden fazla değer gerektiğinde ilke esnekliği sağlar.
Bu ilke tanımı, **Allowedlocations** parametresi için tek bir konuma izin verir ve varsayılan olarak _eastus2_:

```json
"parameters": {
    "allowedLocations": {
        "type": "string",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2"
    }
}
```

**Tür** _dize_ olduğu için, ilke atanırken yalnızca bir değer ayarlanabilir. Bu ilke atanırsa, kapsamdaki kaynaklara yalnızca tek bir Azure bölgesi içinde izin verilir. Çoğu ilke tanımlarının, _eastus2_, _eastus_ ve _westus2_ gibi onaylanan seçenekler listesi için izin verilmesi gerekir.

Birden çok seçeneğe izin vermek üzere ilke tanımı oluşturmak için _dizi_ **türünü** kullanın. Aynı ilke aşağıdaki gibi yeniden yazılabilir:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2",
        "allowedValues": [
            "eastus2",
            "eastus",
            "westus2"
        ]

    }
}
```

> [!NOTE]
> Bir ilke tanımı kaydedildikten sonra, bir parametresindeki **tür** özelliği değiştirilemez.

Bu yeni parametre tanımı, ilke ataması sırasında birden fazla değer alır. Dizi özelliği tarafından tanımlanan, atama sırasında kullanılabilir olan değerler **önceden tanımlanmış seçenek** listesiyle daha da sınırlıdır. **AllowedValues** kullanımı isteğe bağlıdır.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Atama sırasında değerleri bir parametre dizisine geçirme

İlke Azure portal aracılığıyla atanırken, _dizi_ **türünde** bir parametre tek bir metin kutusu olarak görüntülenir. İpucu "kullanım" diyor. değerlerini ayırmak için. (örneğin, Londra; New York) ". _Eastus2_, _eastus_ ve _westus2_ izin verilen konum değerlerini parametreye geçirmek için aşağıdaki dizeyi kullanın:

`eastus2;eastus;westus2`

Azure CLı, Azure PowerShell veya REST API kullanılırken parametre değerinin biçimi farklıdır. Değerler, parametrenin adını da içeren bir JSON dizesi aracılığıyla geçirilir.

```json
{
    "allowedLocations": {
        "value": [
            "eastus2",
            "eastus",
            "westus2"
        ]
    }
}
```

Bu dizeyi her SDK ile kullanmak için aşağıdaki komutları kullanın:

- Azure CLı: komut [az Policy atama Create](/cli/azure/policy/assignment#az_policy_assignment_create) parametresini parametre **params**
- Azure PowerShell: cmdlet [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) with Parameter **PolicyParameter**
- REST API: _PUT_ [oluşturma](/rest/api/resources/policyassignments/create) işleminde, **Properties. Parameters** özelliğinin değeri olarak istek gövdesinin bir parçası olarak

## <a name="array-conditions"></a>Dizi koşulları

Parametresi _dizi_ [conditions](../concepts/definition-structure.md#conditions) 
 **türünün** birlikte kullanılabileceği ilke kuralı koşulları ve ile sınırlıdır `in` `notIn` . Aşağıdaki ilke tanımını `equals` bir örnek olarak koşulla gerçekleştirin:

```json
{
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "equals": "[parameters('allowedLocations')]"
      }
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "description": "The list of allowed locations for resources.",
        "displayName": "Allowed locations",
        "strongType": "location"
      }
    }
  }
}
```

Azure portal aracılığıyla bu ilke tanımını oluşturmaya çalışmak, bu hata iletisi gibi bir hataya yol açar:

- "' {GUID} ' ilkesi doğrulama hataları nedeniyle parametreleştirimedi. İlke parametrelerinin doğru tanımlanıp tanımlanmadığından emin olun. ' [Parameters (' allowedLocations ')] ' dil ifadesinin değerlendirme sonucu, ' Array ' türü, beklenen tür ' String '. '. "

Beklenen koşul **türü** `equals` _dize_. **Allowedlocations** **türü** _dizi_ olarak tanımlandığından, ilke altyapısı dil ifadesini değerlendirir ve hata oluşturur. `in`Ve `notIn` koşuluyla, ilke altyapısı dil ifadesinde **tür** _dizisini_ bekler. Bu hata iletisini çözümlemek için ya `equals` da ' ya geçin `in` `notIn` .

## <a name="referencing-array-resource-properties"></a>Başvuru dizisi kaynak özellikleri

Birçok kullanım çalışması, değerlendirilen kaynaktaki dizi özellikleriyle çalışmayı gerektirir. Bazı senaryolar bir dizinin tamamına başvurulmayı gerektirir (örneğin, uzunluğunun denetlenmesi). Diğerleri her bir dizi üyesine bir koşul uygulanmasını gerektirir (örneğin, tüm güvenlik duvarı kuralının internet 'ten erişimi engellemediğinden emin olun). Azure Ilkesinin kaynak özelliklerine başvurabileceği farklı yolları ve bu başvuruların dizi özelliklerine başvurduklarında nasıl davranacağını anlamak, bu senaryoları kapsayan koşullar yazmak için kullanılan anahtardır.

### <a name="referencing-resource-properties"></a>Kaynak özelliklerine başvurma
Kaynak özelliklerine [diğer adlar](../concepts/definition-structure.md#aliases) kullanılarak Azure ilkesi tarafından başvurulabilir. Azure Policy içindeki bir kaynak özelliğinin değerlerine başvurmak için iki yol vardır:

- **Tüm** seçili kaynak özelliklerinin bir koşula uyup uymadığını denetlemek için [alan](../concepts/definition-structure.md#fields) koşulunu kullanın. Örnek:

  ```json
  {
    "field" : "Microsoft.Test/resourceType/property",
    "equals": "value"
  }
  ```

- `field()`Bir özelliğin değerine erişmek için işlevini kullanın. Örnek:

  ```json
  {
    "value": "[take(field('Microsoft.Test/resourceType/property'), 7)]",
    "equals": "prefix_"
  }
  ```

Alan koşulunun örtük bir "tümü" davranışı vardır. Diğer ad bir değer koleksiyonunu temsil ediyorsa, tüm bireysel değerlerin koşulu karşılayıp karşılamadığını denetler. `field()`İşlevi diğer şablon işlevleri tarafından işlenebilen, diğer adıyla temsil edilen değerleri döndürür.

### <a name="referencing-array-fields"></a>Başvuru dizisi alanları

Dizi kaynağı özellikleri genellikle iki farklı diğer ad türü tarafından temsil edilir. Kendisine eklenmiş bir ' normal ' diğer ad ve [dizi diğer](../concepts/definition-structure.md#understanding-the--alias) adı `[*]` :

- `Microsoft.Test/resourceType/stringArray`
- `Microsoft.Test/resourceType/stringArray[*]`

#### <a name="referencing-the-array"></a>Diziye başvurma

İlk diğer ad, istek içeriğinden özelliğin değerini tek bir değer temsil eder `stringArray` . Bu özelliğin değeri bir dizi olduğundan, ilke koşullarında çok yararlı değildir. Örnek:

```json
{
  "field": "Microsoft.Test/resourceType/stringArray",
  "equals": "..."
}
```

Bu koşul, tüm `stringArray` diziyi tek bir dize değerine karşılaştırır. Çoğu koşul, `equals` yalnızca dize değerlerini kabul eder, bu nedenle bir diziyi bir dizeye karşılaştırırken çok daha fazla kullanılmaz. Dizi özelliğine başvuran ana senaryo, var olup olmadığı denetlenirken yararlı olur:

```json
{
  "field": "Microsoft.Test/resourceType/stringArray",
  "exists": "true"
}
```

İşlevi ile `field()` döndürülen değer, istek içeriğindeki dizidir, daha sonra dizi bağımsız değişkenlerini kabul eden [desteklenen şablon işlevleri](../concepts/definition-structure.md#policy-functions) ile birlikte kullanılabilir. Örneğin, aşağıdaki koşul uzunluğunun 0 ' dan büyük olup olmadığını denetler `stringArray` :

```json
{
  "value": "[length(field('Microsoft.Test/resourceType/stringArray'))]",
  "greater": 0
}
```

#### <a name="referencing-the-array-members-collection"></a>Dizi üyeleri koleksiyonuna başvurma

Söz dizimini kullanan diğer adlar, Array özelliğinin `[*]` kendisini seçmekten farklı olan **bir Array özelliğinden seçilen özellik değerlerinin bir koleksiyonunu** temsil eder. Durumunda `Microsoft.Test/resourceType/stringArray[*]` , tüm üyelerini içeren bir koleksiyon döndürür `stringArray` . Daha önce belirtildiği gibi, bir `field` koşul tüm seçili kaynak özelliklerinin koşulu karşıladığını denetlediğinde, bu nedenle aşağıdaki koşul yalnızca **Tüm** üyeleri `stringArray` ' "değeri" ' değerine eşitse true olur.

```json
{
  "field": "Microsoft.Test/resourceType/stringArray[*]",
  "equals": "value"
}
```

Dizi nesneler içeriyorsa, `[*]` her bir dizi üyesinden belirli bir özelliğin değerini seçmek için bir diğer ad kullanılabilir. Örnek:

```json
{
  "field": "Microsoft.Test/resourceType/objectArray[*].property",
  "equals": "value"
}
```

`property`İçindeki tüm özelliklerin değerleri değerine eşitse bu koşul doğrudur `objectArray` `"value"` .

`field()`Bir dizi diğer adına başvurmak için işlevini kullanırken, döndürülen değer seçilen tüm değerlerin bir dizisidir. Bu davranış, işlevin ortak kullanım durumunun `field()` , kaynak özelliği değerlerine şablon işlevleri uygulayabilme özelliğinin çok sınırlı olduğunu gösterir. Bu durumda kullanılabilen tek şablon işlevleri, dizi bağımsız değişkenlerini kabul eden bir durumdur. Örneğin, ile dizi uzunluğunu almak mümkündür `[length(field('Microsoft.Test/resourceType/objectArray[*].property'))]` . Ancak, her dizi üyesine şablon işlevi uygulamak ve bunu istenen değerle karşılaştırmak gibi daha karmaşık senaryolar yalnızca ifade kullanılırken mümkündür `count` . Daha fazla bilgi için bkz. [Count ifadesi](#count-expressions).

Özetlemek gerekirse, aşağıdaki örnek kaynak içeriğine ve çeşitli diğer adlarla döndürülen seçili değerlere bakın:

```json
{
  "tags": {
    "env": "prod"
  },
  "properties":
  {
    "stringArray": [ "a", "b", "c" ],
    "objectArray": [
      {
        "property": "value1",
        "nestedArray": [ 1, 2 ]
      },
      {
        "property": "value2",
        "nestedArray": [ 3, 4 ]
      }
    ]
  }
}
```

Örnek kaynak içeriğinde alan koşulunu kullanırken, sonuçlar aşağıdaki gibidir:

| Diğer ad | Seçili değerler |
|:--- |:---|
| `Microsoft.Test/resourceType/missingArray` | `null` |
| `Microsoft.Test/resourceType/missingArray[*]` | Boş değerler koleksiyonu. |
| `Microsoft.Test/resourceType/missingArray[*].property` | Boş değerler koleksiyonu. |
| `Microsoft.Test/resourceType/stringArray` | `["a", "b", "c"]` |
| `Microsoft.Test/resourceType/stringArray[*]` | `"a"`, `"b"`, `"c"` |
| `Microsoft.Test/resourceType/objectArray[*]` |  `{ "property": "value1", "nestedArray": [ 1, 2 ] }`,<br/>`{ "property": "value2", "nestedArray": [ 3, 4 ] }`|
| `Microsoft.Test/resourceType/objectArray[*].property` | `"value1"`, `"value2"` |
| `Microsoft.Test/resourceType/objectArray[*].nestedArray` | `[ 1, 2 ]`, `[ 3, 4 ]` |
| `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` | `1`, `2`, `3`, `4` |

`field()`İşlevi örnek kaynak içeriğinde kullanırken, sonuçlar aşağıdaki gibidir:

| Expression | Döndürülen değer |
|:--- |:---|
| `[field('Microsoft.Test/resourceType/missingArray')]` | `""` |
| `[field('Microsoft.Test/resourceType/missingArray[*]')]` | `[]` |
| `[field('Microsoft.Test/resourceType/missingArray[*].property')]` | `[]` |
| `[field('Microsoft.Test/resourceType/stringArray')]` | `["a", "b", "c"]` |
| `[field('Microsoft.Test/resourceType/stringArray[*]')]` | `["a", "b", "c"]` |
| `[field('Microsoft.Test/resourceType/objectArray[*]')]` |  `[{ "property": "value1", "nestedArray": [ 1, 2 ] }, { "property": "value2", "nestedArray": [ 3, 4 ] }]`|
| `[field('Microsoft.Test/resourceType/objectArray[*].property')]` | `["value1", "value2"]` |
| `[field('Microsoft.Test/resourceType/objectArray[*].nestedArray')]` | `[[ 1, 2 ], [ 3, 4 ]]` |
| `[field('Microsoft.Test/resourceType/objectArray[*].nestedArray[*]')]` | `[1, 2, 3, 4]` |

## <a name="count-expressions"></a>Sayı ifadeleri

[Count](../concepts/definition-structure.md#count) ifadeleri, kaç dizi üyesinin bir koşula uymasını ve sayıyı bir hedef değerle karşılaştırmasını sayar. `Count` koşullara kıyasla dizileri değerlendirmek için daha sezgisel ve çok yönlüdür `field` . Söz dizimi aşağıdaki gibidir:

```json
{
  "count": {
    "field": <[*] alias>,
    "where": <optional policy condition expression>
  },
  "equals|greater|less|any other operator": <target value>
}
```

' WHERE ' koşulu olmadan kullanıldığında, `count` yalnızca bir dizinin uzunluğunu döndürür. Önceki bölümde bulunan örnek kaynak içeriğiyle, `count` `true` bu yana aşağıdaki ifade `stringArray` üç üyeye sahiptir:

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]"
  },
  "equals": 3
}
```

Bu davranış, iç içe diziler ile de kullanılabilir. Örneğin, `count` `true` dizilerde dört dizi üyesi olduğundan, aşağıdaki ifade olarak değerlendirilir `nestedArray` :

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]"
  },
  "greaterOrEquals": 4
}
```

Öğesinin gücü `count` `where` koşulnda. Belirtildiğinde, Azure Ilkesi dizi üyelerini numaralandırır ve her bir koşula göre değerlendirilen dizi üyelerinin sayısını sayarak değerlendirir `true` . Özellikle, koşul değerlendirmesinin her yinelemesinde `where` , Azure ilkesi tek bir dizi üyesi ***i** _ seçer ve kaynak içeriğini * `where` i array_ 'nin tek üyesi * _*_i_*_ . Her yinelemede yalnızca bir dizi üyesinin kullanılabilir olması, her bir dizi üyesine karmaşık koşullar uygulamak için bir yol sağlar.

Örnek:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]",
    "where": {
      "field": "Microsoft.Test/resourceType/stringArray[*]",
      "equals": "a"
    }
  },
  "equals": 1
}
```
İfadeyi değerlendirmek için `count` Azure ilkesi, `where` her üye için bir kez koşulu 3 kez değerlendirir `stringArray` ve kaç kez değerlendirildiğini Sayın `true` . `where`Koşul `Microsoft.Test/resourceType/stringArray[*]` dizi üyelerini başvurduğunda, öğesinin tüm üyelerini seçmek yerine `stringArray` yalnızca tek bir dizi üyesini her seferinde seçer:

| Yineleme | Seçili `Microsoft.Test/resourceType/stringArray[*]` değerler | `where` Değerlendirme sonucu |
|:---|:---|:---|
| 1 | `"a"` | `true` |
| 2 | `"b"` | `false` |
| 3 | `"c"` | `false` |

Ve bu nedenle `count` döndürülür `1` .

Daha karmaşık bir ifade aşağıda verilmiştir:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*]",
    "where": {
      "allOf": [
        {
          "field": "Microsoft.Test/resourceType/objectArray[*].property",
          "equals": "value2"
        },
        {
          "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]",
          "greater": 2
        }
      ]
    }
  },
  "equals": 1
}
```

| Yineleme | Seçili değerler | `where` Değerlendirme sonucu |
|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value1"` </br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1`, `2` | `false` |
| 2 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value2"` </br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3`, `4`| `true` |

Ve bu nedenle `count` döndürülür `1` .

`where`İfadenin **Tüm** istek içeriğine karşı değerlendirilme (yalnızca şu anda numaralandırılmakta olan dizi üyesine yapılan değişikliklerle), `where` koşulun dizi dışındaki alanlara de başvurabileceği anlamına gelir:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*]",
    "where": {
      "field": "tags.env",
      "equals": "prod"
    }
  }
}
```

| Yineleme | Seçili değerler | `where` Değerlendirme sonucu |
|:---|:---|:---|
| 1 | `tags.env` => `"prod"` | `true` |
| 2 | `tags.env` => `"prod"` | `true` |

İç içe geçmiş sayısı ifadelerine da izin verilir:
```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/objectArray[*]",
    "where": {
      "allOf": [
        {
          "field": "Microsoft.Test/resourceType/objectArray[*].property",
          "equals": "value2"
        },
        {
          "count": {
            "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]",
            "where": {
              "field": "Microsoft.Test/resourceType/objectArray[*].nestedArray[*]",
              "equals": 3
            },
            "greater": 0
          }
        }
      ]
    }
  }
}
```
 
| Dış döngü yinelemesi | Seçili değerler | İç döngü yinelemesi | Seçili değerler |
|:---|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value1`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1`, `2` | 1 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1` |
| 1 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value1`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `1`, `2` | 2 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `2` |
| 2 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value2`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3`, `4` | 1 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3` |
| 2 | `Microsoft.Test/resourceType/objectArray[*].property` => `"value2`</br> `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `3`, `4` | 2 | `Microsoft.Test/resourceType/objectArray[*].nestedArray[*]` => `4` |

### <a name="the-field-function-inside-where-conditions"></a>`field()`Koşul içindeki işlev `where`

`field()`Bir koşulun içindeyken işlevlerin davranış şekli `where` aşağıdaki kavramları temel alır:
1. Dizi diğer adları, tüm dizi üyelerinden seçilen bir değer koleksiyonuna çözümlenir.
1. `field()` dizi diğer adlarına başvuran işlevler, seçili değerlere sahip bir dizi döndürür.
1. Koşul içinde sayılan dizi diğer adına başvurmak, `where` geçerli yinelemede değerlendirilen dizi üyesinden tek bir değer seçilmiş bir koleksiyon döndürür.

Bu davranış, koşul içindeki bir işlevle sayılan dizi üyesine başvurulduğunda `field()` `where` , **tek bir üyeye sahip bir dizi döndürdüğünü** gösterir. Bu sezgisel olmayabilir, dizi diğer adlarının her zaman seçili özelliklerin bir koleksiyonunu döndürdüğü fikrle tutarlıdır. Aşağıda bir örnek verilmiştir:

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]",
    "where": {
      "field": "Microsoft.Test/resourceType/stringArray[*]",
      "equals": "[field('Microsoft.Test/resourceType/stringArray[*]')]"
    }
  },
  "equals": 0
}
```

| Yineleme | İfade değerleri | `where` Değerlendirme sonucu |
|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/stringArray[*]` => `"a"` </br>  `[field('Microsoft.Test/resourceType/stringArray[*]')]` => `[ "a" ]` | `false` |
| 2 | `Microsoft.Test/resourceType/stringArray[*]` => `"b"` </br>  `[field('Microsoft.Test/resourceType/stringArray[*]')]` => `[ "b" ]` | `false` |
| 3 | `Microsoft.Test/resourceType/stringArray[*]` => `"c"` </br>  `[field('Microsoft.Test/resourceType/stringArray[*]')]` => `[ "c" ]` | `false` |

Bu nedenle, sayılan dizi diğer adının değerine bir işlevle erişmeniz gerektiğinde `field()` , bunu yapmanın yolu bir şablon işlevi ile sarmalıdır `first()` :

```json
{
  "count": {
    "field": "Microsoft.Test/resourceType/stringArray[*]",
    "where": {
      "field": "Microsoft.Test/resourceType/stringArray[*]",
      "equals": "[first(field('Microsoft.Test/resourceType/stringArray[*]'))]"
    }
  }
}
```

| Yineleme | İfade değerleri | `where` Değerlendirme sonucu |
|:---|:---|:---|
| 1 | `Microsoft.Test/resourceType/stringArray[*]` => `"a"` </br>  `[first(field('Microsoft.Test/resourceType/stringArray[*]'))]` => `"a"` | `true` |
| 2 | `Microsoft.Test/resourceType/stringArray[*]` => `"b"` </br>  `[first(field('Microsoft.Test/resourceType/stringArray[*]'))]` => `"b"` | `true` |
| 3 | `Microsoft.Test/resourceType/stringArray[*]` => `"c"` </br>  `[first(field('Microsoft.Test/resourceType/stringArray[*]'))]` => `"c"` | `true` |

Faydalı örnekler için bkz. [Count örnekleri](../concepts/definition-structure.md#count-examples).

## <a name="modifying-arrays"></a>Dizileri değiştirme

Oluşturma veya güncelleştirme sırasında bir kaynaktaki [değiştirme ve](../concepts/effects.md#append) [değiştirme](../concepts/effects.md#modify) özellikleri. Dizi özellikleriyle çalışırken, bu etkilerin davranışı işlemin diğer adı değiştirmeye çalışıp çalışmadığına göre değişir  **\[\*\]** .

> [!NOTE]
> `modify`Diğer adlarla efektin kullanılması şu anda **önizlemededir**.

|Diğer ad |Etki | Sonuç |
|-|-|-|
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `append` | Azure Ilkesi, eksik olursa, efekt ayrıntılarında belirtilen tüm diziyi ekler. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `modify``add`işlem ile | Azure Ilkesi, eksik olursa, efekt ayrıntılarında belirtilen tüm diziyi ekler. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `modify``addOrReplace`işlem ile | Azure Ilkesi, varsa, efekt ayrıntılarında belirtilen tüm diziyi ekler veya mevcut diziyi değiştirir. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `append` | Azure Ilkesi, efekt ayrıntılarında belirtilen dizi üyesini ekler. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `modify``add`işlem ile | Azure Ilkesi, efekt ayrıntılarında belirtilen dizi üyesini ekler. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `modify``addOrReplace`işlem ile | Azure Ilkesi, var olan tüm dizi üyelerini kaldırır ve efekt ayrıntılarında belirtilen dizi üyesini ekler. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `append` | Azure Ilkesi, `action` her dizi üyesinin özelliğine bir değer ekler. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `modify``add`işlem ile | Azure Ilkesi, `action` her dizi üyesinin özelliğine bir değer ekler. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `modify``addOrReplace`işlem ile | Azure Ilkesi, her dizi üyesinin var olan özelliğini ekler veya değiştirir `action` . |

Daha fazla bilgi için bkz. [append örnekleri](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
