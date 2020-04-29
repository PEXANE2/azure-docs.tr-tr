---
title: Model dönüştürme sırasında malzemeleri geçersiz kılma
description: Dönüştürme sırasında iş akışını geçersiz kılan malzemeleri açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/13/2020
ms.topic: how-to
ms.openlocfilehash: 90653db4c572877a728964851a99beebf2e823a4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681486"
---
# <a name="override-materials-during-model-conversion"></a>Model dönüştürme sırasında malzemeleri geçersiz kılma

Dönüştürme sırasında, kaynak modeldeki malzeme ayarları, işleyici tarafından kullanılan [PBR malzemelerini](../../overview/features/pbr-materials.md) tanımlamak için kullanılır.
Bazen [varsayılan dönüştürme](../../reference/material-mapping.md) istenen sonuçlara izin vermez ve değişiklik yapmanız gerekir.
Bir model Azure uzaktan Işlemede kullanılmak üzere dönüştürüldüğünde, malzeme dönüştürmesinin malzeme başına olarak nasıl yapılacağını özelleştirmek için bir malzeme geçersiz kılma dosyası sağlayabilirsiniz.
[Model dönüştürmeyi yapılandırma](configure-model-conversion.md) bölümünde malzeme geçersiz kılma dosya adını bildirmek için yönergeler vardır.

## <a name="the-override-file-used-during-conversion"></a>Dönüştürme sırasında kullanılan geçersiz kılma dosyası

Basit bir örnek olarak, bir kutu modelinin "varsayılan" olarak adlandırılan tek bir malzemenin olduğunu varsayalım. Albedo renginin ARR 'de kullanılmak üzere ayarlanması gerekir.
Bu durumda, bir `box_materials_override.json` dosya aşağıdaki gibi oluşturulabilir:

```json
[
    {
        "name": "Default",
        "albedoColor": {
            "r": 0.33,
            "g": 0.33,
            "b": 0.33,
            "a": 1.0
        }
    }
]
```

`box_materials_override.json` Dosya, giriş kapsayıcısına yerleştirilir ve `ConversionSettings.json` `box.fbx`üzerine, geçersiz kılma dosyasını nerede bulacağını söyleyen dönüştürmeye bildirir (bkz [. model dönüştürmeyi yapılandırma](configure-model-conversion.md)):

```json
{
    "material-override" : "box_materials_override.json"
}
```

Model dönüştürüldüğünde yeni ayarlar uygulanır.

### <a name="color-materials"></a>Renk malzemeleri

[Renk malzemesi](../../overview/features/color-materials.md) modeli ışıklanmaya bağımsız olan sürekli gölgeli bir yüzey tanımlar.
Bu, örneğin, Photogrammetri algoritmalarının yaptığı varlıklar için yararlıdır.
Malzeme geçersiz kılma dosyalarında, bir malzeme, olarak `unlit` `true`ayarlanarak bir renk malzemesi olarak bildirilebilecek.

```json
[
    {
        "name": "Photogrametry_mat1",
        "unlit" : true
    },
    {
        "name": "Photogrametry_mat2",
        "unlit" : true
    }
]
```

### <a name="ignore-specific-texture-maps"></a>Belirli doku eşlemelerini yoksay

Bazen dönüştürme işleminin belirli doku eşlemelerini yoksaymasını isteyebilirsiniz. Bu, modelinize göre doğru anlaşılmayan özel eşlemeler üreten bir araç tarafından oluşturulduysa, bu durum olabilir. Örneğin, opaklık dışında bir şeyi tanımlamak için kullanılan bir "OpacityMap" veya "NormalMap" in "Tampmap" olarak depolandığı bir model. (İkinci durumda, "NormalMap" olarak dönüştürücünün "NormalMap" olarak kullanılmasına neden olacak "NormalMap" öğesini yok sayabilirsiniz.)

İlke basittir. Yalnızca adlı `ignoreTextureMaps` bir özellik ekleyin ve yoksaymak istediğiniz doku haritasını ekleyin:

```json
[
    {
        "name": "Default",
        "ignoreTextureMaps": ["OpacityMap", "NormalMap"]
    }
]
```

Göz ardı ettiğiniz doku eşlemelerinin tam listesi için aşağıdaki JSON şemasına bakın.

## <a name="json-schema"></a>JSON şeması

Malzemeler dosyaları için tam JSON şeması burada verilmiştir. `unlit` Ve `ignoreTextureMaps`dışında, kullanılabilir özellikler, [renk malzemesi](../../overview/features/color-materials.md) ve [PBR malzeme](../../overview/features/pbr-materials.md) modelleriyle ilgili bölümlerde açıklanan özelliklerin bir alt kümesidir.

```json
{
    "definitions" :
    {
        "color":
        {
            "type" : "object",
            "description" : "Color as 4 components vector",
            "properties":
            {
                "r": {"type":"number"},
                "g": {"type":"number"},
                "b": {"type":"number"},
                "a": {"type":"number"}
            },
            "required": ["r", "g", "b"]
        },
        "alpha":
        {
            "type" : "object",
            "description" : "Alpha channel for color",
            "properties":
            {
                "a": {"type":"number"}
            },
            "required": ["a"]
        },
        "colorOrAlpha":
        {
            "anyOf": [
                {"$ref": "#/definitions/color"},
                {"$ref": "#/definitions/alpha"}
            ]
        },
        "listOfMaps":
        {
            "type": "array",
            "items": {
                "type": "string",
                "enum": ["AlbedoMap",
                            "EmissiveMap",
                            "NormalMap",
                            "OcclusionMap",
                            "RoughnessMap",
                            "MetalnessMap",
                            "ReflectivityMap",
                            "BumpMap",
                            "OpacityMap",
                            "DiffuseMap",
                            "SpecularMap",
                            "ShininessMap",
                            "MetallicRoughnessMap",
                            "SpecularGlossinessMap"]
            }
        }
    },
    "type" : "array",
    "description" : "List of materials to override",
    "items":
    {
        "type" : "object",
        "description" : "List of parameters to override",
        "properties":
        {
            "name": { "type" : "string"},
            "unlit": { "type" : "boolean" },
            "albedoColor": { "$ref": "#/definitions/colorOrAlpha" },
            "roughness": { "type": "number" },
            "metalness": { "type": "number" },
            "transparent": { "type" : "boolean" },
            "alphaClipEnabled": { "type" : "boolean" },
            "alphaClipThreshold": { "type": "number" },
            "useVertexColor": { "type" : "boolean" },
            "isDoubleSided": { "type" : "boolean" },
            "ignoreTextureMaps": { "$ref" : "#/definitions/listOfMaps" }
        },
        "required": ["name"],
        "additionalProperties" : false
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Renk malzemeleri](../../overview/features/color-materials.md)
* [PBR malzemeleri](../../overview/features/pbr-materials.md)