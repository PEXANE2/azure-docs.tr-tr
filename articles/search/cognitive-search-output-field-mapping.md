---
title: Bilişsel arama zenginleştirme giriş alanlarını çıkış alanlarına eşleyin-Azure Search
description: Kaynak veri alanlarını ayıklar ve zenginleştirin ve bir Azure Search dizinde çıkış alanlarıyla eşleyin.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.subservice: cognitive-search
ms.openlocfilehash: 1d1ef7be83601a1ccf09ac52f87d0fcc953c605c
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69639183"
---
# <a name="how-to-map-enriched-fields-to-a-searchable-index"></a>Zenginleştirilmiş alanları aranabilir bir dizinle eşleme

Bu makalede, aranabilir bir dizindeki çıkış alanlarına zenginleştirilmiş giriş alanlarını eşlemeyi öğreneceksiniz. [Bir beceri tanımladıktan](cognitive-search-defining-skillset.md)sonra, arama dizininizdeki belirli bir alana doğrudan değer katkıda bulunan herhangi bir yeteneğin çıkış alanlarını eşlemeniz gerekir. İçeriği zenginleştirilmiş belgelerden dizine taşımak için alan eşlemeleri gereklidir.


## <a name="use-outputfieldmappings"></a>OutputFieldMappings kullanma
Alanları eşlemek için, Dizin `outputFieldMappings` Oluşturucu tanımınıza aşağıda gösterildiği gibi ekleyin:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

İsteğin gövdesi aşağıdaki şekilde yapılandırılır:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions"
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```
Her bir çıkış alanı eşlemesi için, zenginleştirme alanının adını (sourceFieldName) ve alanın adı dizinde (targetFieldName) başvurulduğu şekilde ayarlayın.

Bir sourceFieldName içindeki yol bir öğeyi veya birden çok öğeyi temsil edebilir. Yukarıdaki örnekte, ```/document/content/sentiment``` tek bir sayısal değeri temsil ederken ```/document/content/organizations/*/description``` , çeşitli kuruluş açıklamalarını temsil eder. Çeşitli öğelerin olduğu durumlarda, öğelerin her birini içeren bir dizi içinde "düzleştirilmez". Daha fazla iyal ```/document/content/organizations/*/description``` , örneğin, *açıklamalar* alanındaki veriler, Dizin oluşturulmadan önce düz bir açıklama dizisi gibi görünür:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```
## <a name="next-steps"></a>Sonraki adımlar
Zenginleştirilmiş alanları aranabilir alanlara eşleştirdikten sonra, aranabilir alanların her biri için alan özniteliklerini [Dizin tanımının bir parçası olarak](search-what-is-an-index.md)ayarlayabilirsiniz.

Alan eşleme hakkında daha fazla bilgi için bkz. [Azure Search Dizin oluşturucularda alan eşlemeleri](search-indexer-field-mappings.md).
