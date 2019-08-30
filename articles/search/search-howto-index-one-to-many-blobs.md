---
title: Tam metin araması için Azure Blob Dizin oluşturucudan birden çok arama dizini belgesi içeren dizin Blobları-Azure Search
description: Azure Search blob Dizin oluşturucuyu kullanarak metin içeriği için Azure Blob 'larını gezin. Her blob bir veya daha fazla Azure Search Dizin belgesi içerebilir.
ms.date: 05/02/2019
author: arv100kri
manager: nitinme
ms.author: arjagann
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seofeb2018
ms.openlocfilehash: 2c2a17d006f65854a89b9fac1818fcec420c07dc
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70182319"
---
# <a name="indexing-blobs-producing-multiple-search-documents"></a>Birden çok arama belgesi üreten Blobların dizinini oluşturma
Varsayılan olarak, bir blob Dizin Oluşturucu bir Blobun içeriğini tek bir arama belgesi olarak değerlendirir. Belirli **Parsingmode** değerleri, tek bir Blobun birden çok arama belgesi ile sonuçlanabileceğinden senaryolar destekler. Bir dizin oluşturucunun bir Blobun birden fazla arama belgesi ayıklamasına izin veren farklı türde **Parsingmode** 'lar şunlardır:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

## <a name="one-to-many-document-key"></a>Bire çok belge anahtarı
Bir Azure Search dizininde görüntülenen her belge, bir belge anahtarı tarafından benzersiz şekilde tanımlanır. 

Hiçbir ayrıştırma modu belirtilmediğinde ve dizindeki anahtar alanı için açık eşleme yoksa Azure Search, `metadata_storage_path` özelliği otomatik olarak anahtar olarak [eşler](search-indexer-field-mappings.md) . Bu eşleme, her Blobun ayrı bir arama belgesi olarak görünmesini sağlar.

Yukarıda listelenen ayrıştırma modlarından herhangi birini kullanırken, bir blob "birçok" arama belgesiyle eşlenir ve bir belge anahtarı yalnızca blob meta verilerini uygun olmayan şekilde temel alır. Bu kısıtlamayı aşmak için Azure Search bloba ayıklanan her bir varlık için "bire çok" belge anahtarı oluşturma yeteneğine sahiptir. Bu özellik adlandırılır `AzureSearch_DocumentKey` ve bloba ayıklanan her bir varlığa eklenir. Bu özelliğin değeri, _Bloblar genelinde_ her bir varlık için benzersiz olarak garanti edilir ve varlıklar ayrı arama belgeleri olarak görünür.

Varsayılan olarak, anahtar dizin alanı için hiçbir açık alan eşlemesi belirtilmediğinde `AzureSearch_DocumentKey` , `base64Encode` alan eşleme işlevi kullanılarak onunla eşleştirilir.

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

Bir Dizin Oluşturucu oluşturup, anahtar alanı için herhangi bir açık alan `jsonLines` eşlemesi belirtmeden **parsingmode** 'u olarak ayarlarsanız, aşağıdaki eşleme örtük olarak uygulanır
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Bu kurulum, aşağıdaki bilgileri içeren Azure Search dizinine neden olur (breçekimi için Base64 kodlamalı kimlik kısaltıldı)

| id | sıcaklık | basınç | timestamp |
|----|-------------|----------|-----------|
| aHR0 ... Yıljeuannvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... Yıljeuannvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... Yıljluayla Annvbjsx | 1\. | 1\. | 2018-01-12T00:00:00Z |
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

`delimitedText` **Parsingmode**ile bir Dizin Oluşturucu oluşturduğunuzda, anahtar alanına aşağıdaki şekilde bir alan eşleme işlevi ayarlamak doğal olabilir:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Ancak, bu eşleme, Dizin _Bloblar genelinde_benzersiz olmadığından, dizinde gösterilen 4 belge ile `recordid` _sonuçlanmaz_ . Bu nedenle, `AzureSearch_DocumentKey` özellikten "bire çok" ayrıştırma modları için anahtar Dizin alanına uygulanan örtük alan eşlemesini kullanmanızı öneririz.

Açık alan eşlemesi ayarlamak istiyorsanız, _SourceField_ 'ın **Tüm Bloblar genelinde**her bir varlık için benzersiz olduğundan emin olun.

> [!NOTE]
> Ayıklanan varlık başına benzersizlik `AzureSearch_DocumentKey` sağlamak için kullanılan yaklaşım değişikliğe tabidir ve bu nedenle uygulamanızın gereksinimlerine göre bu değere dayanmamalıdır.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search Dizin oluşturucular](search-indexer-overview.md)
+ [Azure Blob depolamayı Azure Search ile dizinleme](search-howto-index-json-blobs.md)
+ [Azure Search blob Indexer ile CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
+ [JSON bloblarını Azure Search blob Indexer ile dizinleme](search-howto-index-json-blobs.md)

## <a name="NextSteps"></a>Sonraki adımlar
* Azure Search hakkında daha fazla bilgi için [Arama hizmeti sayfasına](https://azure.microsoft.com/services/search/)bakın.