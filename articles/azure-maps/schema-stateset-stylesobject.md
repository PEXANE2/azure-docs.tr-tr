---
title: Dinamik Azure haritaları için StylesObject
description: JSON şemasına yönelik başvuru kılavuzu ve dinamik Azure haritalar 'da oluşturma bölümünde kullanılan StylesObject için sözdizimi.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/19/2020
ms.topic: reference
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 4b085fbc6e330d38b59fce0c494f672b00c712b7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85120530"
---
# <a name="stylesobject-schema-reference-guide-for-dynamic-maps"></a>Dinamik haritalar için StylesObject şeması başvuru kılavuzu

Bu makale, için JSON şemasına ve sözdizimine yönelik başvuru kılavuzudur `StylesObject` . , `StylesObject` `StyleObject` Stateset stillerini temsil eden bir dizidir. Stateset stillerinizi ınkapımap veri özelliklerine uygulamak için Azure Maps Oluşturucu [özelliği durum hizmetini](https://docs.microsoft.com/rest/api/maps/featurestate) kullanın. Stateset stillerinizi oluşturduktan ve bunları ınkapımap özellikleriyle ilişkilendirdikten sonra, bunları dinamik ınkapılı haritalar oluşturmak için kullanabilirsiniz. Dinamik ınkapılı haritalar oluşturma hakkında daha fazla bilgi için bkz. [creator ınkapımaps için dinamik stil uygulama](indoor-map-dynamic-styling.md).

## <a name="styleobject"></a>StyleObject

`StyleObject`, Ya da olarak olur [`BooleanTypeStyleRule`](#booleantypestylerule) [`NumericTypeStyleRule`](#numerictypestylerule) .

Aşağıdaki JSON `BooleanTypeStyleRule` adlı bir adlandırılmış `occupied` ve adı gösterir `NumericTypeStyleRule` `temperature` .

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
    }
]
```

## <a name="numerictypestylerule"></a>NumericTypeStyleRule

 `NumericTypeStyleRule`, Bir olur [`StyleObject`](#styleobject) ve aşağıdaki özelliklerden oluşur:

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `keyName` | string | *Durum* veya dinamik özellik adı. Bir, `keyName` dizi içinde benzersiz olmalıdır `StyleObject` .| Evet |
| `type` | string | Değer "numeric". | Evet |
| `rules` | [`NumberRuleObject`](#numberruleobject)[]| İlişkili renklerle bir sayısal stil aralığı dizisi. Her Aralık, *durum* değeri aralığa uygun olduğunda kullanılacak bir renk tanımlar.| Evet |

### <a name="numberruleobject"></a>NumberRuleObject

, `NumberRuleObject` Bir [`RangeObject`](#rangeobject) ve `color` özelliğinden oluşur. *Durum* değeri aralığa denk geliyorsa, görüntüleme rengi özellikte belirtilen renkte olacaktır `color` .

Birden çok çakışan Aralık tanımlarsanız, seçilen renk, karşıladığınız ilk aralıkta tanımlanan renk olur.

Aşağıdaki JSON örneğinde, *durum* değeri 50-60 arasındaysa her iki Aralık da true olarak kalır. Ancak, kullanılacak olan renk, listedeki uygulanan ilk Aralık olduğu için kullanılır `#343deb` .

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
| `range` | [RangeObject](#rangeobject) | [Rangeobject](#rangeobject) , bir mantıksal Aralık koşulları kümesi tanımlar, bu, `true` *durumun* görüntüleme rengini özelliğinde belirtilen renkle değiştirir `color` . `range`Belirtilmemişse, özellikte tanımlanan renk `color` her zaman kullanılacaktır.   | Hayır |
| `color` | string | Durum değeri aralığa düştüğünde kullanılacak renk. `color`Özelliği, aşağıdaki biçimlerden herhangi birindeki BIR JSON dizesidir: <ul><li> HTML stili onaltılı değerler </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, %50, 50%)")</li><li> HSLA ("HSLA (100, %50, 50%, 1)")</li><li> Önceden tanımlanmış, sarı ve mavi gibi HTML renkleri adları.</li></ul> | Evet |

### <a name="rangeobject"></a>RangeObject

, `RangeObject` Bir sayısal Aralık değeri tanımlar [`NumberRuleObject`](#numberruleobject) . *Durum* değerinin aralığa dönebilmesi için, tüm tanımlı koşulların doğru olması gerekir. 

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `minimum` | double | X ≥ kadar olan tüm x `minimum`| Hayır |
| `maximum` | double | X 'in x ≤ tüm sayısı `maximum` . | Hayır |
| `exclusiveMinimum` | double | X 'in tüm x > sayısı `exclusiveMinimum` .| Hayır |
| `exclusiveMaximum` | double | X 'in tüm x < sayısı `exclusiveMaximum` .| Hayır |

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

## <a name="booleantypestylerule"></a>BooleanTypeStyleRule

`BooleanTypeStyleRule`, Bir olur [`StyleObject`](#styleobject) ve aşağıdaki özelliklerden oluşur:

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `keyName` | string |  *Durum* veya dinamik özellik adı.  `keyName`Stil dizisi içinde benzersiz olmalıdır.| Evet |
| `type` | string |Değer "Boolean" değeridir. | Evet |
| `rules` | [`BooleanRuleObject`](#booleanruleobject)1| Renkleri `true` ve `false` *durum* değerlerini içeren bir Boole çifti.| Evet |

### <a name="booleanruleobject"></a>BooleanRuleObject

`BooleanRuleObject` `true` , Ve değerleri için renkleri tanımlar `false` .

| Özellik | Tür | Açıklama | Gerekli |
|-----------|----------|-------------|-------------|
| `true` | string | *Durum* değeri olduğunda kullanılacak renk `true` . `color`Özelliği, aşağıdaki biçimlerden herhangi birindeki BIR JSON dizesidir: <ul><li> HTML stili onaltılı değerler </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, %50, 50%)")</li><li> HSLA ("HSLA (100, %50, 50%, 1)")</li><li> Önceden tanımlanmış, sarı ve mavi gibi HTML renkleri adları.</li></ul>| Evet |
| `false` | string | *Durum* değeri olduğunda kullanılacak renk `false` . | Evet |

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
