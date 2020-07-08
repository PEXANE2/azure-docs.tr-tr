---
title: Kaynaklardaki dizi özellikleri için yazma ilkeleri
description: Dizi parametreleri ve dizi dili ifadeleriyle çalışmayı öğrenin, [*] diğer adını değerlendirin ve Azure Ilke tanımı kuralları ile öğeleri ekleyin.
ms.date: 05/20/2020
ms.topic: how-to
ms.openlocfilehash: f3d30f76d555386e5ab8041a0b8cc82b5b60e28e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83684244"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Azure kaynaklarında dizi özellikleri için yazma ilkeleri

Azure Resource Manager özellikler genellikle dizeler ve Boole değerleri olarak tanımlanır. Bire çok ilişkisi olduğunda, karmaşık özellikler bunun yerine diziler olarak tanımlanır. Azure Ilkesinde diziler birkaç farklı şekilde kullanılır:

- Birden çok seçenek sağlamak için bir [tanım parametresinin](../concepts/definition-structure.md#parameters)türü
- **Ya da** **notın** koşullarını kullanan bir [ilke kuralının](../concepts/definition-structure.md#policy-rule) bir parçası
- Değerlendirmek için [ \[ \* \] diğer adı](../concepts/definition-structure.md#understanding-the--alias) değerlendiren bir ilke kuralının parçası:
  - **Hiçbiri**, **Any**veya **Tümü** gibi senaryolar
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

**Tür** _dize_olduğu için, ilke atanırken yalnızca bir değer ayarlanabilir. Bu ilke atanırsa, kapsamdaki kaynaklara yalnızca tek bir Azure bölgesi içinde izin verilir. Çoğu ilke tanımlarının, _eastus2_, _eastus_ve _westus2_gibi onaylanan seçenekler listesi için izin verilmesi gerekir.

Birden çok seçeneğe izin vermek üzere ilke tanımı oluşturmak için _dizi_ **türünü**kullanın. Aynı ilke aşağıdaki gibi yeniden yazılabilir:

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

İlke Azure portal aracılığıyla atanırken, _dizi_ **türünde** bir parametre tek bir metin kutusu olarak görüntülenir. İpucu "kullanım" diyor. değerlerini ayırmak için. (örneğin, Londra; New York) ". _Eastus2_, _eastus_ve _westus2_ izin verilen konum değerlerini parametreye geçirmek için aşağıdaki dizeyi kullanın:

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

- Azure CLı: komut [az Policy atama Create](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) parametresini parametre **params**
- Azure PowerShell: cmdlet [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) with Parameter **PolicyParameter**
- REST API: _PUT_ [oluşturma](/rest/api/resources/policyassignments/create) işleminde, **Properties. Parameters** özelliğinin değeri olarak istek gövdesinin bir parçası olarak

## <a name="policy-rules-and-arrays"></a>İlke kuralları ve diziler

### <a name="array-conditions"></a>Dizi koşulları

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

Beklenen koşul **türü** `equals` _dize_. **Allowedlocations** **türü** _dizi_olarak tanımlandığından, ilke altyapısı dil ifadesini değerlendirir ve hata oluşturur. `in`Ve `notIn` koşuluyla, ilke altyapısı dil ifadesinde **tür** _dizisini_ bekler. Bu hata iletisini çözümlemek için ya `equals` da ' ya geçin `in` `notIn` .

### <a name="evaluating-the--alias"></a>[*] Diğer adı değerlendiriliyor

Adına eklenmiş olan diğer adlar **\[\*\]** **türün** bir _dizi_olduğunu gösterir. Tüm dizinin değerini değerlendirmek yerine, **\[\*\]** dizideki her öğeyi MANTıKSAL ve aralarında tek tek değerlendirmek mümkün hale gelir. Her öğe değerlendirmesi için bu üç standart senaryo vardır: _none_, _Any_veya _All_ öğeleri eşleşir. Karmaşık senaryolar için [Count](../concepts/definition-structure.md#count)kullanın.

İlke altyapısı **, yalnızca** **IF** kuralı doğru olarak değerlendirildiğinde **etkisini** tetikler.
Bu olgu, **\[\*\]** dizinin her bir öğesini değerlendiren şekilde anlamak için önemlidir.

Aşağıdaki senaryo tablosu için örnek ilke kuralı:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            <-- Condition (see table below) -->
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

**Iprules** dizisi aşağıdaki senaryo tablosu için aşağıdaki gibidir:

```json
"ipRules": [
    {
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Aşağıdaki her koşul örneği için ile değiştirin `<field>` `"field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value"` .

Aşağıdaki sonuçlar, koşulun birleşiminin ve yukarıdaki mevcut değerlerden oluşan örnek ilke kuralının sonucudur:

|Koşul |Sonuç | Senaryo |Açıklama |
|-|-|-|-|
|`{<field>,"notEquals":"127.0.0.1"}` |Nothing |Hiçbiri eşleşmiyor |Tek bir dizi öğesi yanlış (127.0.0.1! = 127.0.0.1) ve diğeri doğru (127.0.0.1! = 192.168.1.1) olarak değerlendirilir, bu nedenle **Not alalals** koşulu _false_ olur ve etki tetiklenmez. |
|`{<field>,"notEquals":"10.0.4.1"}` |İlke etkisi |Hiçbiri eşleşmiyor |Her iki dizi öğesi de true olarak değerlendirilir (10.0.4.1! = 127.0.0.1 ve 10.0.4.1! = 192.168.1.1), bu nedenle **Not al** koşulu _true_ ve etki tetiklenir. |
|`"not":{<field>,"notEquals":"127.0.0.1" }` |İlke etkisi |Bir veya daha fazla eşleşme |Tek bir dizi öğesi yanlış (127.0.0.1! = 127.0.0.1) ve diğeri doğru (127.0.0.1! = 192.168.1.1) olarak değerlendirilir ve bu nedenle **Not al** koşulu _false_olur. Mantıksal işleç doğru olarak değerlendirilir (false**değil** _false_), bu nedenle etki tetiklenir. |
|`"not":{<field>,"notEquals":"10.0.4.1"}` |Nothing |Bir veya daha fazla eşleşme |Her iki dizi öğesi de true olarak değerlendirilir (10.0.4.1! = 127.0.0.1 ve 10.0.4.1! = 192.168.1.1), bu nedenle **Not al** koşulu _doğrudur_. Mantıksal işleç yanlış olarak değerlendirilir ( _true_değil), bu nedenle etki**tetiklenmez** . |
|`"not":{<field>,"Equals":"127.0.0.1"}` |İlke etkisi |Tüm eşleşme değil |Bir Array öğesi true (127.0.0.1 = = 127.0.0.1) ve diğeri false (127.0.0.1 = = 192.168.1.1) olarak değerlendirilir ve bu nedenle **eşittir** koşulu _false_olur. Mantıksal işleç doğru olarak değerlendirilir (false**değil** _false_), bu nedenle etki tetiklenir. |
|`"not":{<field>,"Equals":"10.0.4.1"}` |İlke etkisi |Tüm eşleşme değil |Her iki dizi öğesi de false olarak değerlendirilir (10.0.4.1 = = 127.0.0.1 ve 10.0.4.1 = = 192.168.1.1), bu nedenle **eşittir** koşulu _false_olur. Mantıksal işleç doğru olarak değerlendirilir (false**değil** _false_), bu nedenle etki tetiklenir. |
|`{<field>,"Equals":"127.0.0.1"}` |Nothing |Tüm eşleşme |Bir Array öğesi true (127.0.0.1 = = 127.0.0.1) ve diğeri false (127.0.0.1 = = 192.168.1.1) olarak değerlendirilir; bu nedenle **eşittir** koşulu _false_ olur ve etki tetiklenmez. |
|`{<field>,"Equals":"10.0.4.1"}` |Nothing |Tüm eşleşme |Her iki dizi öğesi de false olarak değerlendirilir (10.0.4.1 = = 127.0.0.1 ve 10.0.4.1 = = 192.168.1.1), bu nedenle **eşittir** koşulu _false_ olur ve etki tetiklenmez. |

## <a name="the-append-effect-and-arrays"></a>Ekleme efekti ve dizileri

**Ayrıntılar. alanın** bir diğer ad olmasına bağlı olarak, [ekleme efekti](../concepts/effects.md#append) farklı şekilde davranır **\[\*\]** .

- **\[\*\]** Diğer ad olmadığında, Append **değeri** tüm diziyi değer özelliği ile değiştirir
- Bir **\[\*\]** diğer ad olduğunda, append değeri, var olan diziye **değer** özelliğini ekler veya yeni diziyi oluşturur

Daha fazla bilgi için bkz. [append örnekleri](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
