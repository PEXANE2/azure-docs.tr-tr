---
title: Dönüştürülmüş bir model hakkında bilgi alın
description: Tüm model dönüştürme parametrelerinin açıklaması
author: malcolmtyrrell
ms.author: matyrr
ms.date: 03/05/2020
ms.topic: how-to
ms.openlocfilehash: d5f843add0649682bae8c472bc50b6beea33bf93
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681525"
---
# <a name="get-information-about-a-converted-model"></a>Dönüştürülmüş bir model hakkında bilgi alın

Dönüştürme hizmeti tarafından üretilen arrAsset dosyası yalnızca işleme hizmeti tarafından tüketim için tasarlanmıştır. Ancak, bir işleme oturumu başlatmadan bir model hakkındaki bilgilere erişmek istediğiniz zamanlar olabilir. Bu nedenle, dönüştürme hizmeti çıktı kapsayıcısında arrAsset dosyasının yanına bir JSON dosyası yerleştirir. Örneğin, bir dosya `buggy.gltf` dönüştürülürse, çıktı kapsayıcısı `buggy.info.json` dönüştürülen varlığın `buggy.arrAsset`yanında çağrılan bir dosya içerir. Kaynak model, dönüştürülen model ve dönüştürmenin kendisi hakkında bilgi içerir.

## <a name="example-info-file"></a>Örnek *bilgi* dosyası

Burada adlı `buggy.gltf`bir dosya dönüştürerek üretilen bir örnek *bilgi* dosyası:

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

Bu bölümde sağlanan dosya adları içerir.

* `input`: Kaynak dosyanın adı.
* `output`: Kullanıcı varsayılan olmayan bir ad belirttiğinde çıktı dosyasının adı.

### <a name="the-conversionsettings-section"></a>*Dönüşüm Ayarları* bölümü

Bu bölümde, model dönüştürüldüğünde belirtilen [Dönüşüm Ayarları'nın](configure-model-conversion.md#settings-file) bir kopyası yer almaktadır.

### <a name="the-inputinfo-section"></a>*GirişBilgi* bölümü

Bu bölümde kaynak dosya biçimi yle ilgili bilgiler kaydedilir.

* `sourceAssetExtension`: Kaynak dosyanın dosya uzantısı.
* `sourceAssetFormat`: Kaynak dosya biçiminin açıklaması.
* `sourceAssetFormatVersion`: Kaynak dosya biçiminin sürümü.
* `sourceAssetGenerator`: Varsa, kaynak dosyayı oluşturan aracın adı.

### <a name="the-inputstatistics-section"></a>*Girişİstatistikler* bölümü

Bu bölümde kaynak sahne hakkında bilgi sağlar. Genellikle bu bölümdeki değerler le kaynak modeli oluşturan araçtaki eşdeğer değerler arasında tutarsızlıklar olacaktır. Model dışa aktarma ve dönüştürme adımları sırasında değiştirildiğinden, bu tür farklılıklar beklenmektedir.

* `numMeshes`: Her parçanın tek bir malzemeye başvurulabileceği kafes parçalarının sayısı.
* `numFaces`: Tüm modeldeki _toplam üçgen_ sayısı. Mesh in dönüştürme sırasında üçgenlenmiş olduğunu unutmayın.
* `numVertices`: Tüm modeldeki toplam tepe sayısı.
* `numMaterial`: Tüm modeldeki toplam malzeme sayısı.
* `numFacesSmallestMesh`: Modelin en küçük örgüsündeki üçgen sayısı.
* `numFacesBiggestMesh`: Modelin en büyük örgüsündeki üçgen sayısı.
* `numNodes`: Modelin sahne grafiğindeki düğüm sayısı.
* `numMeshUsagesInScene`: Düğümlerin kaç kez başvuru meshes sayısı. Birden fazla düğüm aynı kafese başvurabilir.
* `maxNodeDepth`: Sahne grafiğindeki düğümlerin maksimum derinliği.

### <a name="the-outputinfo-section"></a>*OutputInfo* bölümü

Bu bölümde, oluşturulan çıktı hakkında genel bilgiler kaydedilir.

* `conversionToolVersion`: Model dönüştürücü sürümü.
* `conversionHash`: ArrAsset içindeki verilerin bir karma render katkıda bulunabilir. Dönüşüm hizmetinin aynı dosyada yeniden çalıştırıldığında farklı bir sonuç üretip üretmediğini anlamak için kullanılabilir.

### <a name="the-outputstatistics-section"></a>*Çıktılarİstatistikbölümü*

Bu bölümde dönüştürülen varlıktan hesaplanan bilgiler kaydedilir.

* `numMeshPartsCreated`: ArrAsset'deki meshesayısı. Instancing `numMeshes` dönüşüm `inputStatistics` işleminden etkilendiği için, bölümdekinden farklı olabilir.
* `numMeshPartsInstanced`: ArrAsset'de yeniden kullanılan meshe sayısı.
* `recenteringOffset`: Dönüşüm `recenterToOrigin` [Ayarları'ndaki](configure-model-conversion.md) seçenek etkinleştirildiğinde, bu değer dönüştürülen modeli özgün konumuna geri döndürecek çeviridir.
* `boundingBox`: Modelin sınırları.

## <a name="next-steps"></a>Sonraki adımlar

* [Model dönüştürme](model-conversion.md)
* [Model dönüşümyapılandırma](configure-model-conversion.md)
