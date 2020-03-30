---
title: Birden çok belge içeren dizin lekeleri
titleSuffix: Azure Cognitive Search
description: Her blob'un bir veya daha fazla arama dizin belgesi verebileceği Azure Congitive Search Blob dizinini kullanarak metin içeriği için Azure bloblarını tarayın.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 1840bda0ecc9462a5d8f796b616d728d0bb412f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112274"
---
# <a name="indexing-blobs-to-produce-multiple-search-documents"></a>Birden çok arama belgesi oluşturmak için blobs dizini
Varsayılan olarak, bir blob dizinleyici tek bir arama belgesi olarak bir blob içeriğini ele alır. Belirli **ayrıştırmaModu** değerleri, tek bir blob'un birden çok arama belgesine neden olabileceği senaryoları destekler. Bir dizin leyicinin bir blob'dan birden fazla arama belgesi ayıklamasını sağlayan farklı **ayrışma Modu** türleri şunlardır:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

## <a name="one-to-many-document-key"></a>Bir-çok belge anahtarı
Azure Bilişsel Arama dizininde görünen her belge, bir belge anahtarıyla benzersiz olarak tanımlanır. 

Ayrıştırma modu belirtilmediğinde ve dizindeki anahtar alanı için açık bir eşleme `metadata_storage_path` yoksa Azure Bilişsel Arama özelliği otomatik olarak anahtar olarak [eşler.](search-indexer-field-mappings.md) Bu eşleme, her blob'un ayrı bir arama belgesi olarak görünmesini sağlar.

Yukarıda listelenen ayrıştırma modlarından herhangi birini kullanırken, bir blob eşlerini "çok" arama belgelerine göre eşler ve yalnızca blob meta verilerini temel alan bir belge anahtarı yapmak uygun olmayan bir belge dir. Bu kısıtlamayı aşmak için Azure Bilişsel Arama, bir lekeden çıkarılan her bir varlık için "birden çok'a" bir belge anahtarı oluşturma yeteneğine sahiptir. Bu özellik `AzureSearch_DocumentKey` adlandırılmış ve blob çıkarılan her bir varlık eklenir. Bu özelliğin değeri _blobs genelinde_ her bir varlık için benzersiz olması garanti edilir ve varlıklar ayrı arama belgeleri olarak gösterecektir.

Varsayılan olarak, anahtar dizin alanı için açık alan `AzureSearch_DocumentKey` eşlemeleri belirtilmediğinde, `base64Encode` alan eşleme işlevini kullanarak eşlenir.

## <a name="example"></a>Örnek
Aşağıdaki alanları içeren bir dizin tanımınız olduğunu varsayalım:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Ve blob konteyner aşağıdaki yapı ile lekeler vardır:

_Blob1.json_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2.json_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Bir dizin oluşturup **parsingMode'u** ayarladığınızda `jsonLines` - anahtar alanı için açık alan eşlemeleri belirtmeden, aşağıdaki eşleme örtülü olarak uygulanır
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Bu kurulum, aşağıdaki bilgileri içeren Azure Bilişsel Arama dizinine neden olur (kısalık için kısaltılmış base64 kodlanmış kimlik)

| id | sıcaklık | basınç | timestamp |
|----|-------------|----------|-----------|
| aHR0 ... YjEuanNvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... YjEuanNvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... YjIuanNvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0 ... YjIuanNvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Dizin anahtar alanı için özel alan eşleme

Önceki örnekle aynı dizin tanımıvarsayarsak, blob kapsayıcı aşağıdaki yapı ile lekeler olduğunu söylüyorlar:

_Blob1.json_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2.json_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

`delimitedText` **ParsingMode**ile bir dizin oluştursanız, aşağıdaki gibi anahtar alana bir alan eşleme işlevi kurmak doğal hissedebilirsiniz:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Ancak, alan _lekeler arasında_benzersiz olmadığından, `recordid` bu eşleme 4 belgenin dizinde gösterilmesine neden _olmaz._ Bu nedenle, `AzureSearch_DocumentKey` özellikten "bir-çok" ayrıştırma modları için anahtar dizin alanına uygulanan örtük alan eşlemesini kullanmanızı öneririz.

Açık bir alan eşlemesi ayarlamak istiyorsanız, _kaynak Alanının_ **tüm lekelerdeki**her bir varlık için farklı olduğundan emin olun.

> [!NOTE]
> Ayıklanan varlık `AzureSearch_DocumentKey` başına benzersizlik sağlama nın sağladığı yaklaşım değişebilir ve bu nedenle uygulamanızın gereksinimleri için bunun değerine güvenmemelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

Blob dizin oluşturmanın temel yapısını ve iş akışını zaten bilmiyorsanız, önce [Azure Bilişsel Arama ile Azure Blob Depolamasını Dizine Ekleyerek'i](search-howto-index-json-blobs.md) gözden geçirmelisiniz. Farklı blob içerik türleri için ayrıştma modları hakkında daha fazla bilgi için aşağıdaki makaleleri inceleyin.

> [!div class="nextstepaction"]
> [Dizin oluşturma CSV blobs](search-howto-index-csv-blobs.md)
> [Indeksleme JSON lekeler](search-howto-index-json-blobs.md)
