---
title: Dönüştürülmüş bir model hakkında bilgi alma
description: Tüm model dönüştürme parametrelerinin açıklaması
author: malcolmtyrrell
ms.author: matyrr
ms.date: 03/05/2020
ms.topic: how-to
ms.openlocfilehash: 722d3e218272202074820db442ab1592042c7011
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84805002"
---
# <a name="get-information-about-a-converted-model"></a>Dönüştürülmüş bir model hakkında bilgi alma

Dönüştürme hizmeti tarafından üretilen arrAsset dosyası yalnızca işleme hizmeti tarafından tüketimine yöneliktir. Ancak, bir işleme oturumu başlatmadan bir modelle ilgili bilgilere erişmek istediğinizde bazı zamanlar olabilir. Bu nedenle, dönüştürme hizmeti çıkış kapsayıcısındaki arrAsset dosyasının yanına bir JSON dosyası koyar. Örneğin, bir dosya `buggy.gltf` dönüştürülürse, çıkış kapsayıcısı, dönüştürülmüş varlığın yanında adlı bir dosya içerir `buggy.info.json` `buggy.arrAsset` . Kaynak modeli, dönüştürülmüş model ve dönüştürmenin kendisi hakkında bilgiler içerir.

## <a name="example-info-file"></a>Örnek *bilgi* dosyası

Aşağıda adlı bir dosya dönüştürülürken oluşturulan örnek bir *bilgi* dosyası verilmiştir `buggy.gltf` :

```JSON
{
    "files": {
        "input": "Buggy.gltf"
    },
    "conversionSettings": {
        "recenterToOrigin": true
    },
    "inputInfo": {
        "sourceAssetExtension": ".gltf",
        "sourceAssetFormat": "glTF2 Importer",
        "sourceAssetFormatVersion": "2.0",
        "sourceAssetGenerator": "COLLADA2GLTF"
    },
    "inputStatistics": {
        "numMeshes": 148,
        "numFaces": 308306,
        "numVertices": 245673,
        "numMaterial": 149,
        "numFacesSmallestMesh": 2,
        "numFacesBiggestMesh": 8764,
        "numNodes": 206,
        "numMeshUsagesInScene": 236,
        "maxNodeDepth": 3
    },
    "outputInfo": {
        "conversionToolVersion": "3b28d840de9916f9d628342f474d38c3ab949590",
        "conversionHash": "CCDB1F7A4C09F565"
    },
    "outputStatistics": {
        "numMeshPartsCreated": 236,
        "numMeshPartsInstanced": 88,
        "recenteringOffset": [
            -24.1,
            -50.9,
            -16.5974
        ],
        "boundingBox": {
            "min": [
                -43.52,
                -61.775,
                -79.6416
            ],
            "max": [
                43.52,
                61.775,
                79.6416
            ]
        }
    }
}
```

## <a name="information-in-the-info-file"></a>Bilgi dosyasındaki bilgiler

### <a name="the-files-section"></a>*Dosyalar* bölümü

Bu bölüm, belirtilen dosya adlarını içerir.

* `input`: Kaynak dosyanın adı.
* `output`: Kullanıcı varsayılan olmayan bir ad belirtmiştir, çıkış dosyasının adı.

### <a name="the-conversionsettings-section"></a>*Conversionsettings* bölümü

Bu bölüm, model dönüştürüldüğünde belirtilen [Conversionsettings](configure-model-conversion.md#settings-file) 'in bir kopyasını içerir.

### <a name="the-inputinfo-section"></a>*Inputınfo* bölümü

Bu bölüm, kaynak dosya biçimiyle ilgili bilgileri kaydeder.

* `sourceAssetExtension`: Kaynak dosyanın dosya uzantısı.
* `sourceAssetFormat`: Kaynak dosya biçiminin açıklaması.
* `sourceAssetFormatVersion`: Kaynak dosya biçiminin sürümü.
* `sourceAssetGenerator`: Varsa, kaynak dosyayı oluşturan aracın adı.

### <a name="the-inputstatistics-section"></a>*Inputstatistics* bölümü

Bu bölüm, kaynak sahneyi hakkında bilgi sağlar. Bu bölümdeki değerler ve kaynak modeli oluşturan araçtaki eşdeğer değerler arasında genellikle tutarsızlıklar olur. Bu farklar beklenmektedir, çünkü model dışa aktarma ve dönüştürme adımları sırasında değiştirilir.

* `numMeshes`: Her parçanın tek bir malzemeye başvurmasına neden olan kafes bölümlerinin sayısı.
* `numFaces`: Tüm modeldeki _üçgenin_ toplam sayısı. Ağ dönüştürme sırasında triangulated olduğunu unutmayın. Bu sayı, [Standart Işleme VM boyutundaki](../../reference/vm-sizes.md#how-the-renderer-evaluates-the-number-of-polygons)Çokgen sınırına katkıda bulunur.
* `numVertices`: Tüm modeldeki köşelerin toplam sayısı.
* `numMaterial`: Tüm modeldeki Toplam malzeme sayısı.
* `numFacesSmallestMesh`: Modelin en küçük ağı içindeki üçgenin sayısı.
* `numFacesBiggestMesh`: Modelin en büyük kafesteki üçgenler sayısı.
* `numNodes`: Modelin sahne grafiğindeki düğüm sayısı.
* `numMeshUsagesInScene`: Düğümlerin kafeslerin kaç kez başvurulacağını. Birden fazla düğüm aynı kafesde başvurabilir.
* `maxNodeDepth`: Sahne grafiği içindeki düğümlerin maksimum derinliği.

### <a name="the-outputinfo-section"></a>*Outputınfo* bölümü

Bu bölüm, oluşturulan çıkış hakkındaki genel bilgileri kaydeder.

* `conversionToolVersion`: Model dönüştürücünün sürümü.
* `conversionHash`: ArrAsset içinde işleme katkıda bulunan verilerin karması. Aynı dosya üzerinde yeniden çalıştırıldığında, dönüştürme hizmetinin farklı bir sonuç üretmediğini anlamak için kullanılabilir.

### <a name="the-outputstatistics-section"></a>*Outputstatistics* bölümü

Bu bölüm, dönüştürülmüş varlık tarafından hesaplanan bilgileri kaydeder.

* `numMeshPartsCreated`: ArrAsset içindeki kafeslerin sayısı. `numMeshes` `inputStatistics` Örnek, dönüştürme işleminden etkilendiğinden, bölümünde farklılık gösterebilir.
* `numMeshPartsInstanced`: ArrAsset içinde yeniden kullanılan kafeslerin sayısı.
* `recenteringOffset`: `recenterToOrigin` [Conversionsettings](configure-model-conversion.md) seçeneği etkin olduğunda, bu değer dönüştürülen modeli özgün konumuna geri taşıyabilecek çevirmadır.
* `boundingBox`: Modelin sınırları.

## <a name="next-steps"></a>Sonraki adımlar

* [Model dönüştürme](model-conversion.md)
* [Model dönüştürmeyi yapılandırma](configure-model-conversion.md)
