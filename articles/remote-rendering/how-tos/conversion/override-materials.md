---
title: Model dönüştürme sırasında malzemeleri geçersiz kılma
description: Dönüşüm zamanında ki iş akışını geçersiz kılan malzemeyi açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/13/2020
ms.topic: how-to
ms.openlocfilehash: 90653db4c572877a728964851a99beebf2e823a4
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681486"
---
# <a name="override-materials-during-model-conversion"></a>Model dönüştürme sırasında malzemeleri geçersiz kılma

Dönüştürme sırasında, kaynak modeldeki malzeme ayarları, işleyici tarafından kullanılan [PBR malzemelerini](../../overview/features/pbr-materials.md) tanımlamak için kullanılır.
Bazen [varsayılan dönüştürme](../../reference/material-mapping.md) istenilen sonuçları vermez ve değişiklik yapmanız gerekir.
Bir model Azure Uzaktan İşleme'de kullanılmak üzere dönüştürüldüğünde, malzeme dönüştürmenin malzeme başına nasıl yapıldığını özelleştirmek için bir malzeme geçersiz kılma dosyası sağlayabilirsiniz.
[Model dönüştürme yapılandırma](configure-model-conversion.md) bölümü, malzeme geçersiz dosyası nın dosya adını bildirmek için yönergeler vardır.

## <a name="the-override-file-used-during-conversion"></a>Dönüştürme sırasında kullanılan geçersiz kılma dosyası

Basit bir örnek olarak, bir kutu modelinin "Varsayılan" adı verilen tek bir malzemeye sahip olduğunu varsayalım. Albedo rengi ARR kullanım için ayarlanması gerekir.
Bu durumda, `box_materials_override.json` bir dosya aşağıdaki gibi oluşturulabilir:

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

Dosya `box_materials_override.json` giriş kapsayıcısına yerleştirilir ve `ConversionSettings.json` bir yanı, `box.fbx`dönüşüm geçersiz kılma dosyasını nerede bulacağını söyler (bkz. model dönüştürme [yapılandırma:](configure-model-conversion.md)

```json
{
    "material-override" : "box_materials_override.json"
}
```

Model dönüştürüldüğünde, yeni ayarlar uygulanır.

### <a name="color-materials"></a>Renkli malzemeler

[Renk malzemesi](../../overview/features/color-materials.md) modeli, aydınlatmadan bağımsız olarak sürekli gölgelenmiş bir yüzeyi tanımlar.
Bu, örneğin Fotogrametri algoritmaları tarafından yapılan varlıklar için yararlıdır.
Malzeme geçersiz kılma dosyalarında, bir malzeme ' ye `unlit` ayarlayarak renk malzemesi olarak ilan `true`edilebilir.

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

### <a name="ignore-specific-texture-maps"></a>Belirli doku eşlemlerini yoksay

Bazen dönüştürme işleminin belirli doku eşlemlerini yok saymasını isteyebilirsiniz. Bu durum, modeliniz işleyici tarafından doğru anlaşılmayan özel haritalar oluşturan bir araç tarafından oluşturulduğunda olabilir. Örneğin, opaklık dışında bir şeyi tanımlamak için kullanılan bir "OpacityMap" veya "NormalMap"in "BumpMap" olarak depolandığı bir model. (İkinci durumda dönüştürücü "NormalMap" olarak "BumpMap" kullanmak neden olur "NormalMap", göz ardı etmek istiyorum.)

Prensip basittir. Sadece adlı `ignoreTextureMaps` bir özellik ekleyin ve yoksaymak istediğiniz herhangi bir doku eşlemi ekleyin:

```json
[
    {
        "name": "Default",
        "ignoreTextureMaps": ["OpacityMap", "NormalMap"]
    }
]
```

Yoksayabileceğiniz doku haritalarının tam listesi için aşağıdaki JSON şemasına bakın.

## <a name="json-schema"></a>JSON şema

Malzeme dosyaları için tam JSON şema burada verilmiştir. Renk malzemesi `unlit` ve `ignoreTextureMaps` [PBR](../../overview/features/pbr-materials.md) [malzeme](../../overview/features/color-materials.md) modellerinde bölümlerde açıklanan özelliklerin bir alt kümesi ve mevcut özellikleri dışında.

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

* [Renkli malzemeler](../../overview/features/color-materials.md)
* [PBR malzemeleri](../../overview/features/pbr-materials.md)