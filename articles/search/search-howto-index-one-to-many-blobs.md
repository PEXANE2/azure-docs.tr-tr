---
title: Tam metin araması için Azure Blob Dizin oluşturucudan çok sayıda arama dizini belgesine bir blob 'u dizine ekleyin
titleSuffix: Azure Cognitive Search
description: Azure Congitive arama blobu Dizin oluşturucuyu kullanarak Azure bloblarını metin içeriği için gezin. Her blob bir veya daha fazla arama dizini belgesi sağlayabilir.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 863d38f6ea0f071a1c1a6678d025ec5b37a306dc
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73466429"
---
# <a name="indexing-blobs-to-produce-multiple-search-documents"></a>Birden çok arama belgesi üretmek için Blobları dizinleme
Varsayılan olarak, bir blob Dizin Oluşturucu bir Blobun içeriğini tek bir arama belgesi olarak değerlendirir. Belirli **Parsingmode** değerleri, tek bir Blobun birden çok arama belgesi ile sonuçlanabileceğinden senaryolar destekler. Bir dizin oluşturucunun bir Blobun birden fazla arama belgesi ayıklamasına izin veren farklı türde **Parsingmode** 'lar şunlardır:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

## <a name="one-to-many-document-key"></a>Bire çok belge anahtarı
Bir Azure Bilişsel Arama dizininde görüntülenen her belge, bir belge anahtarı tarafından benzersiz şekilde tanımlanır. 

Hiçbir ayrıştırma modu belirtilmediğinde ve Azure Bilişsel Arama dizindeki anahtar alanı için açık eşleme yoksa, `metadata_storage_path` özelliğini anahtar olarak otomatik olarak [eşler](search-indexer-field-mappings.md) . Bu eşleme, her Blobun ayrı bir arama belgesi olarak görünmesini sağlar.

Yukarıda listelenen ayrıştırma modlarından herhangi birini kullanırken, bir blob "birçok" arama belgesiyle eşlenir ve bir belge anahtarı yalnızca blob meta verilerini uygun olmayan şekilde temel alır. Azure Bilişsel Arama, bu kısıtlamayı aşmak için bir bloba ayıklanan her bir varlık için "bire çok" belge anahtarı oluşturma yeteneğine sahiptir. Bu özellik `AzureSearch_DocumentKey` olarak adlandırılır ve bloba ayıklanan her bir varlığa eklenir. Bu özelliğin değeri, _Bloblar genelinde_ her bir varlık için benzersiz olarak garanti edilir ve varlıklar ayrı arama belgeleri olarak görünür.

Varsayılan olarak, anahtar dizin alanı için hiçbir açık alan eşlemesi belirtilmediğinde `AzureSearch_DocumentKey`, `base64Encode` alan eşleme işlevi kullanılarak onunla eşleştirilir.

## <a name="example"></a>Örnek
Aşağıdaki alanlarla bir dizin tanımı olduğunu varsayalım:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Blob kabınızda aşağıdaki yapıyla blob 'lar vardır:

_Blob1. JSON_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2. JSON_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Bir Dizin Oluşturucu oluşturup, anahtar alanı için herhangi bir açık alan eşlemelerini belirtmeden `jsonLines` **-olarak ayarladığınızda** , aşağıdaki eşleme örtük olarak uygulanır
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Bu kurulum, aşağıdaki bilgileri içeren Azure Bilişsel Arama dizinine neden olur (breçekimi için Base64 kodlamalı kimlik kısaltıldı)

| id | sıcaklık | basınç | timestamp |
|----|-------------|----------|-----------|
| aHR0 ... Yıljeuannvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... Yıljeuannvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... Yıljluayla Annvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0 ... Yıljluayla Annvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Dizin anahtarı alanı için özel alan eşleme

Önceki örnekle aynı dizin tanımının olduğunu varsayarsak, blob kapsayıcınızda aşağıdaki yapıya sahip Bloblar olduğunu varsayalım:

_Blob1. JSON_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2. JSON_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

`delimitedText` **Parsingmode**ile bir Dizin Oluşturucu oluşturduğunuzda, anahtar alana aşağıdaki şekilde bir alan eşleme işlevi ayarlamak doğal olabilir:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Ancak, `recordid` alanı _bloblarda_benzersiz olmadığından, bu eşleme dizinde gösterilen 4 belge ile _sonuçlanmaz_ . Bu nedenle, `AzureSearch_DocumentKey` özelliğinden uygulanan örtük alan eşlemesini "bire çok" ayrıştırma modları için anahtar Dizin alanına kullanmanızı öneririz.

Açık alan eşlemesi ayarlamak istiyorsanız, _SourceField_ 'ın **Tüm Bloblar genelinde**her bir varlık için benzersiz olduğundan emin olun.

> [!NOTE]
> Ayıklanan varlık başına benzersizlik sağlamak `AzureSearch_DocumentKey` tarafından kullanılan yaklaşım değişikliğe tabidir ve bu nedenle uygulamanızın gereksinimlerine göre bu değere dayanmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Blob dizin oluşturma işleminin temel yapısı ve iş akışı hakkında bilgi sahibi değilseniz, ilk olarak [Azure Search Azure Blob Storage Dizin oluşturmayı](search-howto-index-json-blobs.md) gözden geçirmeniz gerekir. Farklı blob içerik türleri için ayrıştırma modları hakkında daha fazla bilgi için aşağıdaki makaleleri gözden geçirin.

> [!div class="nextstepaction"]
> [JSON Bloblarını dizine](search-howto-index-json-blobs.md) alma
> [CSV Blobları](search-howto-index-csv-blobs.md)
