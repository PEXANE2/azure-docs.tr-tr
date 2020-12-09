---
title: Dinamik Azure haritaları için StylesObject şeması başvuru kılavuzu
description: Dinamik Azure Maps StylesObject şeması ve söz dizimi için başvuru kılavuzu.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: reference
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 08379e66c97d34eea53410190475e90e156a58e2
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96903352"
---
# <a name="stylesobject-schema-reference-guide-for-dynamic-maps"></a>Dinamik haritalar için StylesObject şeması başvuru kılavuzu

> [!IMPORTANT]
> Azure haritalar Creator Hizmetleri şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 , `StylesObject` `StyleObject` Stateset stillerini temsil eden bir dizidir. Stateset stillerinizi ınkapımap veri özelliklerine uygulamak için Azure haritalar Oluşturucu (Önizleme) [özelliği durum hizmetini](/rest/api/maps/featurestate) kullanın. Stateset stillerinizi oluşturduktan ve onları ınkapımap özellikleriyle ilişkilendirdikten sonra, bunları dinamik ınkapılı haritalar oluşturmak için kullanabilirsiniz. Dinamik ınkapılı haritalar oluşturma hakkında daha fazla bilgi için bkz. [creator ınkapımaps için dinamik stil uygulama](indoor-map-dynamic-styling.md).

## <a name="styleobject"></a>StyleObject

`StyleObject`, Aşağıdaki stil kurallarından biridir:

 * [`BooleanTypeStyleRule`](#booleantypestylerule)
 * [`NumericTypeStyleRule`](#numerictypestylerule)
 * [`StringTypeStyleRule`](#stringtypestylerule)

Aşağıdaki JSON, her üç stil türünün örnek kullanımını gösterir.  `BooleanTypeStyleRule`Özelliği true ve false olan özelliklerin dinamik stilini belirlemekte kullanılır `occupied` .  `NumericTypeStyleRule` `temperature` Özelliği belirli bir Aralık içinde olan özelliklerin stilini belirlemekte kullanılır. Son olarak, `StringTypeStyleRule` belirli stilleri ile eşleştirmek için kullanılır `meetingType` .



```json
 "styles": [
     {
        "keyname": "occupied",
        "type": "boolean",
        "rules": [
            {
                "true": "#FF0000",
                "false": "#00FF00"
            }
        ]
    },
    {
        "keyname": "temperature",
        "type": "number",
        "rules": [
             {
                "range": {
                "minimum": 50,
                "exclusiveMaximum": 70
                },
                "color": "#343deb"
            },
            {
                "range": {
                "maximum": 70,
                "exclusiveMinimum": 30
              },
              "color": "#eba834"
            }
        ]
    },
    {
      "keyname": "meetingType",
      "type": "string",
      "rules": [
        {
          "private": "#FF0000",
          "confidential": "#FF00AA",
          "allHands": "#00FF00",
          "brownBag": "#964B00"
        }
      ]
    }
]
```

## <a name="numerictypestylerule"></a>NumericTypeStyleRule

 `NumericTypeStyleRule`, Bir olur [`StyleObject`](#styleobject) ve aşağıdaki özelliklerden oluşur:

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `keyName` | string | *Durum* veya dinamik özellik adı. Bir `keyName` , dizi içinde benzersiz olmalıdır `StyleObject` .| Yes |
| `type` | string | Değer "numeric". | Yes |
| `rules` | [`NumberRuleObject`](#numberruleobject)[]| İlişkili renklerle bir sayısal stil aralığı dizisi. Her Aralık, *durum* değeri aralığa uygun olduğunda kullanılacak bir renk tanımlar.| Yes |

### <a name="numberruleobject"></a>NumberRuleObject

, `NumberRuleObject` Bir [`RangeObject`](#rangeobject) ve `color` özelliğinden oluşur. *Durum* değeri aralığa denk geliyorsa, görüntüleme rengi özellikte belirtilen renkte olacaktır `color` .

Birden çok çakışan Aralık tanımlarsanız, seçilen renk, karşıladığınız ilk aralıkta tanımlanan renk olur.

Aşağıdaki JSON örneğinde, *durum* değeri 50-60 arasındaysa her iki Aralık da true olarak kalır. Ancak, kullanılan renk, `#343deb` Bu, listedeki ilk Aralık olduğu için kullanılır.

```json

    {
        "rules":[
            {
                "range": {
                "minimum": 50,
                "exclusiveMaximum": 70
                },
                "color": "#343deb"
            },
            {
                "range": {
                "minimum": 50,
                "maximum": 60
                },
                "color": "#eba834"
            }
        ]
    }
]
```

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `range` | [RangeObject](#rangeobject) | [Rangeobject](#rangeobject) , bir mantıksal Aralık koşulları kümesi tanımlar, bu, `true` *durumun* görüntüleme rengini özelliğinde belirtilen renkle değiştirir `color` . `range`Belirtilmemişse, özellikte tanımlanan renk `color` her zaman kullanılacaktır.   | No |
| `color` | string | Durum değeri aralığa düştüğünde kullanılacak renk. `color`Özelliği, aşağıdaki biçimlerden herhangi birindeki BIR JSON dizesidir: <ul><li> HTML stili onaltılı değerler </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, %50, 50%)")</li><li> HSLA ("HSLA (100, %50, 50%, 1)")</li><li> Önceden tanımlanmış, sarı ve mavi gibi HTML renkleri adları.</li></ul> | Yes |

### <a name="rangeobject"></a>RangeObject

, `RangeObject` Bir sayısal Aralık değeri tanımlar [`NumberRuleObject`](#numberruleobject) . *Durum* değerinin aralığa dönebilmesi için, tüm tanımlı koşulların doğru olması gerekir.

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `minimum` | double | X ≥ kadar olan tüm x `minimum`| No |
| `maximum` | double | X 'in x ≤ tüm sayısı `maximum` . | No |
| `exclusiveMinimum` | double | X 'in tüm x > sayısı `exclusiveMinimum` .| No |
| `exclusiveMaximum` | double | X 'in tüm x < sayısı `exclusiveMaximum` .| No |

### <a name="example-of-numerictypestylerule"></a>NumericTypeStyleRule örneği

Aşağıdaki JSON adlı bir `NumericTypeStyleRule` *durumu* gösterir `temperature` . Bu örnekte, [`NumberRuleObject`](#numberruleobject) iki tanımlı sıcaklık aralığı ve bunların ilişkili renk stillerini içerir. Sıcaklık aralığı 50-69 ise, ekran rengini kullanmalıdır `#343deb` .  Sıcaklık aralığı 31-70 ise, ekran rengini kullanmalıdır `#eba834` .

```json
{
    "keyname": "temperature",
    "type": "number",
    "rules":[
        {
            "range": {
            "minimum": 50,
            "exclusiveMaximum": 70
            },
            "color": "#343deb"
        },
        {
            "range": {
            "maximum": 70,
            "exclusiveMinimum": 30
            },
            "color": "#eba834"
        }
    ]
}
```

## <a name="stringtypestylerule"></a>StringTypeStyleRule

`StringTypeStyleRule`, Bir olur [`StyleObject`](#styleobject) ve aşağıdaki özelliklerden oluşur:

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `keyName` | string |  *Durum* veya dinamik özellik adı.  Bir `keyName` , dizi içinde benzersiz olmalıdır  `StyleObject` .| Yes |
| `type` | string |Değer "String" değeridir. | Yes |
| `rules` | [`StringRuleObject`](#stringruleobject)[]| N sayıda *durum* değeri dizisi.| Yes |

### <a name="stringruleobject"></a>StringRuleObject

, Bir `StringRuleObject` özelliğin özelliğinin olası dize değerleri olan en fazla N sayıda durum değeri içerir. Özelliğin özellik değeri tanımlı durum değerleriyle eşleşmezse, bu özelliğin dinamik bir stili olmayacaktır. Yinelenen durum değerleri verilirse, ilki öncelik alır.

Eşleşen dize değeri büyük/küçük harfe duyarlıdır.

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `stateValue1` | string | Değer dizesinin stateValue1 olduğu renk. | No |
| `stateValue2` | string | Değer dizesinin stateValue olduğu renk. | No |
| `stateValueN` | string | Değer dizesinin stateValueN ve olduğu renk. | No |

### <a name="example-of-stringtypestylerule"></a>StringTypeStyleRule örneği

Aşağıdaki JSON, `StringTypeStyleRule` belirli toplantı türleriyle ilişkili stilleri tanımlayan bir gösterir.

```json
    {
      "keyname": "meetingType",
      "type": "string",
      "rules": [
        {
          "private": "#FF0000",
          "confidential": "#FF00AA",
          "allHands": "#00FF00",
          "brownBag": "#964B00"
        }
      ]
    }

```

## <a name="booleantypestylerule"></a>BooleanTypeStyleRule

`BooleanTypeStyleRule`, Bir olur [`StyleObject`](#styleobject) ve aşağıdaki özelliklerden oluşur:

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `keyName` | string |  *Durum* veya dinamik özellik adı.  Bir `keyName` , dizi içinde benzersiz olmalıdır `StyleObject`  .| Yes |
| `type` | string |Değer "Boolean" değeridir. | Yes |
| `rules` | [`BooleanRuleObject`](#booleanruleobject)1| Renkleri `true` ve `false` *durum* değerlerini içeren bir Boole çifti.| Yes |

### <a name="booleanruleobject"></a>BooleanRuleObject

`BooleanRuleObject` `true` , Ve değerleri için renkleri tanımlar `false` .

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `true` | string | *Durum* değeri olduğunda kullanılacak renk `true` . `color`Özelliği, aşağıdaki biçimlerden herhangi birindeki BIR JSON dizesidir: <ul><li> HTML stili onaltılı değerler </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, %50, 50%)")</li><li> HSLA ("HSLA (100, %50, 50%, 1)")</li><li> Önceden tanımlanmış, sarı ve mavi gibi HTML renkleri adları.</li></ul>| Yes |
| `false` | string | *Durum* değeri olduğunda kullanılacak renk `false` . | Yes |

### <a name="example-of-booleantypestylerule"></a>BooleanTypeStyleRule örneği

Aşağıdaki JSON adlı bir `BooleanTypeStyleRule` *durumu* gösterir `occupied` . , [`BooleanRuleObject`](#booleanruleobject) Ve değerleri için renkleri tanımlar `true` `false` .

```json
{
    "keyname": "occupied",
    "type": "boolean",
    "rules": [
    {
        "true": "#FF0000",
        "false": "#00FF00"
    }
    ]
}
```