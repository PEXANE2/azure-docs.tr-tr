---
title: Kaynaklardaki dizi özellikleri için yazar ilkeleri
description: Dizi parametreleri ve dizi dil ifadeleriyle çalışmayı öğrenin, [*] takma adını değerlendirin ve Azure İlkesi tanım kurallarıyla öğeleri ekleyin.
ms.date: 11/26/2019
ms.topic: how-to
ms.openlocfilehash: 991d159f6444133d902382bc9ca43bc2acd201e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280670"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Azure kaynaklarındaki dizi özellikleri için yazar ilkeleri

Azure Kaynak Yöneticisi özellikleri genellikle dizeleri ve booleans olarak tanımlanır. Bir-çok ilişkisi varsa, karmaşık özellikler bunun yerine diziler olarak tanımlanır. Azure İlkesi'nde diziler birkaç farklı şekilde kullanılır:

- Bir tanım [parametresi](../concepts/definition-structure.md#parameters)türü , birden fazla seçenek sağlamak için
- **In'deki** veya **olmayan** koşulları kullanarak bir [ilke kuralının](../concepts/definition-structure.md#policy-rule) bir parçası
- Diğer [ \[ \* \] adı](../concepts/definition-structure.md#understanding-the--alias) değerlendiren bir ilke kuralının bir bölümü:
  - **Yok**, **Yok**veya **Tümü** gibi senaryolar
  - **Sayım** lı karmaşık senaryolar
- Varolan bir diziyi değiştirmek veya eklemek için [ek efektinde](../concepts/effects.md#append)

Bu makale, Azure İlkesi'nin her kullanımını kapsar ve birkaç örnek tanım sağlar.

## <a name="parameter-arrays"></a>Parametre dizileri

### <a name="define-a-parameter-array"></a>Parametre dizilimi tanımlama

Bir parametrenin dizi olarak tanımlanması, birden fazla değer gerektiğinde ilke esnekliği sağlar.
Bu ilke tanımı, _eastus2_için parametre **izin Konumlar** ve varsayılanlar için herhangi bir tek bir konuma izin verir:

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

**Türü** _dize_olduğu gibi, ilke atarken yalnızca bir değer ayarlanabilir. Bu ilke atanmışsa, kapsamdaki kaynaklara yalnızca tek bir Azure bölgesinde izin verilir. Çoğu ilke tanımları _eastus2_, _eastus_, ve _westus2_izin gibi onaylanmış seçeneklerin bir listesi için izin vermek gerekir.

Birden çok seçeneğe izin vermek için ilke tanımını oluşturmak _için dizi_ türünü kullan **type** Aynı ilke aşağıdaki gibi yeniden yazılabilir:

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
> İlke tanımı kaydedildikten sonra, parametredeki **tür** özelliği değiştirilemez.

Bu yeni parametre tanımı ilke ataması sırasında birden fazla değer alır. Dizi özelliği **izin Verilen Değerler** tanımlandığı için, atama sırasında kullanılabilen değerler önceden tanımlanmış seçenekler listesiyle daha da sınırlıdır. İzin **verilen Değerlerin** kullanımı isteğe bağlıdır.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Atama sırasında değerleri parametre dizilimine geçir

İlkeyi Azure portalı üzerinden atarken, **tür** _dizisi_ parametresi tek bir textbox olarak görüntülenir. İpucu diyor ki "Kullanın ; değerleri ayırmak için. (örneğin Londra; New York)". _Eastus2_, _eastus_ve _westus2'nin_ izin verilen konum değerlerini parametreye geçirmek için aşağıdaki dizeyi kullanın:

`eastus2;eastus;westus2`

Azure CLI, Azure PowerShell veya REST API'yi kullanırken parametre değerinin biçimi farklıdır. Değerler, parametrenin adını da içeren bir JSON dizesinde geçirilir.

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

- Azure CLI: Parametre **paramlarıyla** [az ilke atamasını oluşturma](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) komutu
- Azure PowerShell: Parametre **PolitikasıParametre** ile Cmdlet [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment)
- REST API: _PUT_ [PUT'ta,](/rest/api/resources/policyassignments/create) request body'nin bir parçası olarak özelliklerin değeri olarak oluşturma **işlemi.parametreler** özelliği

## <a name="policy-rules-and-arrays"></a>İlke kuralları ve diziler

### <a name="array-conditions"></a>Dizi koşulları

Bir _dizi_
parametrenin**type** kullanılabileceğini belirten ilke kuralı `in` [koşulları](../concepts/definition-structure.md#conditions) sınırlıdır ve `notIn`. Bir örnek olarak koşul `equals` ile aşağıdaki ilke tanımı nı ele alalım:

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

Bu ilke tanımını Azure portalı üzerinden oluşturmaya çalışmak, bu hata iletisi gibi bir hataya yol açar:

- "'{GUID}' ilkesi doğrulama hataları nedeniyle parametreye alınamadı. Lütfen ilke parametrelerinin doğru tanımlanmış olup olmadığını kontrol edin. İç özel durum 'Dil ifadesinin değerlendirme sonucu '[parameters('allowedLocations')]' türü 'Dizi', beklenen tür 'String'dir.'."

Beklenen **type** koşul `equals` türü _dizedir._ **AllowedLocations** **tür** _dizisi_olarak tanımlandığından, ilke altyapısı dil ifadesini değerlendirir ve hatayı atar. `in` Ve `notIn` koşulile, ilke altyapısı dil ifadesinde **tür** _dizisini_ bekler. Bu hata iletisini `equals` gidermek `in` için, ya da `notIn`.

### <a name="evaluating-the--alias"></a>[*] takma adını değerlendirme

Adlarına ** \[ \* ** eklenmiş diğer adlar, **türün** bir _dizi_olduğunu gösterir. Dizinin tamamının değerini değerlendirmek yerine, ** \[ \* ** dizinin her öğesini mantıksal ve aralarında tek tek değerlendirmeyi mümkün kılar. Madde başına bu değerlendirmenin yararlı olduğu üç standart senaryo vardır: _Yok,_ _Any_, veya _Tüm_ öğeler eşleşir. Karmaşık senaryolar [için,](../concepts/definition-structure.md#count)sayı kullanın.

İlke **altyapısı, yalnızca** **then** **if** kuralı doğru olarak değerlendirildiğinde efekti tetikler.
Bu gerçeği dizi her bir unsuru ** \[ \* ** değerlendirir şekilde bağlamında anlamak önemlidir.

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

**ipRules** dizisi aşağıdaki senaryo tablosu için aşağıdaki gibidir:

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

Aşağıdaki her koşul örneği `<field>` `"field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value"`için , .

Aşağıdaki sonuçlar, yukarıdaki koşul ve örnek ilke kuralı ve varolan değerlerin dizisini biraraya gelen bir sonucudur:

|Koşul |Sonuç | Senaryo |Açıklama |
|-|-|-|-|
|`{<field>,"notEquals":"127.0.0.1"}` |Nothing |Hiçbiri eşleşmez |Bir dizi öğesi yanlış olarak değerlendirir (127.0.0.1 != 127.0.0.1) ve bir doğru (127.0.0.1 != 192.168.1.1), böylece **notEquals** durumu _yanlış_ tır ve etkisi tetiklenmez. |
|`{<field>,"notEquals":"10.0.4.1"}` |İlke etkisi |Hiçbiri eşleşmez |Her iki dizi öğesi de doğru olarak değerlendirir (10.0.4.1 != 127.0.0.1 ve 10.0.4.1 != 192.168.1.1.1) böylece **notEquals** koşulu _doğrudur_ ve etkisi tetiklenir. |
|`"not":{<field>,"notEquals":"127.0.0.1" }` |İlke etkisi |Bir veya daha fazla eşleşme |Bir dizi öğesi yanlış olarak değerlendirir (127.0.0.1 != 127.0.0.1) ve bir doğru (127.0.0.1 != 192.168.1.1), böylece **notEquals** durumu _yanlıştır_. Mantıksal işleç doğru olarak değerlendirir _(yanlış_**değil),** böylece etkisi tetiklenir. |
|`"not":{<field>,"notEquals":"10.0.4.1"}` |Nothing |Bir veya daha fazla eşleşme |Her iki dizi öğesi de doğru olarak değerlendirilir (10.0.4.1 != 127.0.0.1 ve 10.0.4.1 != 192.168.1.1) olarak değerlendirilir, böylece **notEquals** koşulu _doğrudur_. Mantıksal işleç yanlış olarak değerlendirir _(doğru_**değil),** böylece etkisi tetiklenmez. |
|`"not":{<field>,"Equals":"127.0.0.1"}` |İlke etkisi |Tüm eşleşmeler |Bir dizi öğesi doğru olarak değerlendirir (127.0.0.1 == 127.0.0.1) ve bir yanlış (127.0.0.1 == 192.168.1.1), böylece **Eşittir** koşulu _yanlıştır._ Mantıksal işleç doğru olarak değerlendirir _(yanlış_**değil),** böylece etkisi tetiklenir. |
|`"not":{<field>,"Equals":"10.0.4.1"}` |İlke etkisi |Tüm eşleşmeler |Her iki dizi öğesi de false olarak değerlendirilir (10.0.4.1 == 127.0.0.1 ve 10.0.4.1 == 192.168.1.1) yani **Eşittir** koşulu _yanlıştır._ Mantıksal işleç doğru olarak değerlendirir _(yanlış_**değil),** böylece etkisi tetiklenir. |
|`{<field>,"Equals":"127.0.0.1"}` |Nothing |Tüm maç |Bir dizi öğesi doğru olarak değerlendirir (127.0.0.1 == 127.0.0.1) ve bir yanlış olarak (127.0.0.1 == 192.168.1.1), böylece **Eşittir** koşulu _yanlış_ tır ve efekt tetiklenmez. |
|`{<field>,"Equals":"10.0.4.1"}` |Nothing |Tüm maç |Her iki dizi öğesi de yanlış olarak değerlendirir (10.0.4.1 == 127.0.0.1 ve 10.0.4.1 == 192.168.1.1) bu nedenle **Eşittir** koşulu _yanlıştır_ ve efekt tetiklenmez. |

## <a name="the-append-effect-and-arrays"></a>Ek efekti ve diziler

[Ek efekti,](../concepts/effects.md#append) **details.field'ın** takma ad ** \[ \* ** olup olmadığına bağlı olarak farklı şekilde değişir.

- ** \[ Takma \* ** ad olmadığında, ek tüm diziyi **değer** özelliğiyle değiştirir
- ** \[ Bir \* ** diğer ad, ek **değer** özelliğini varolan diziye ekler veya yeni dizioluşturur

Daha fazla bilgi için [ek örneklere](../concepts/effects.md#append-examples)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları](remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
