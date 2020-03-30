---
title: Çıktı alanlarına harita girişi
titleSuffix: Azure Cognitive Search
description: Kaynak veri alanlarını ayıklayın ve zenginleştirin ve Bir Azure Bilişsel Arama dizinindeki çıktı alanlarıyla eşleşin.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f0537af684632a08a39e3e681900d62238365073
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74280977"
---
# <a name="how-to-map-ai-enriched-fields-to-a-searchable-index"></a>AI ile zenginleştirilmiş alanların aranabilir bir dizinle eşlenme

Bu makalede, zenginleştirilmiş giriş alanlarını kullanılabilir bir dizinde çıktı alanlarıyla nasıl eşlendirdiğinizi öğrenirsiniz. [Bir skillset tanımladıktan](cognitive-search-defining-skillset.md)sonra, arama dizininizdeki belirli bir alana doğrudan değer katan herhangi bir becerinin çıktı alanlarını haritalamalısınız. 

Zenginleştirilmiş belgelerden içeriğin dizin içine taşınması için Çıktı Alanı Eşlemeleri gereklidir.  Zenginleştirilmiş belge gerçekten bir bilgi ağacıdır ve dizideki karmaşık türler için destek olsa da, bazen zenginleştirilmiş ağaçtaki bilgileri daha basit bir türe (örneğin, bir dizi dize) dönüştürmek isteyebilirsiniz. Çıktı alanı eşlemeleri, bilgileri düzleştirmek le veri şekli dönüşümleri gerçekleştirmenize olanak sağlar.

## <a name="use-outputfieldmappings"></a>OutputFieldMappings kullanın
Alanları eşlemek `outputFieldMappings` için, aşağıda gösterildiği gibi dizin leyici tanımınıza ekleyin:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

İsteğin gövdesi aşağıdaki gibi yapılandırılmıştır:

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

Her çıktı alanı eşleme için, zenginleştirilmiş belge ağacındaki (sourceFieldName) verilerin konumunu ve dizinde (targetFieldName) başvurulan alanın adını ayarlayın.

## <a name="flattening-information-from-complex-types"></a>Karmaşık Türlerden Bilgileri Düzleştirmek 

SourceFieldName'deki yol bir öğeyi veya birden çok öğeyi temsil edebilir. Yukarıdaki örnekte, ```/document/content/sentiment``` tek bir sayısal değeri ```/document/content/organizations/*/description``` temsil ederken, birkaç kuruluş açıklamasını temsil eder. 

Birkaç öğenin bulunduğu durumlarda, öğelerin her birini içeren bir diziiçine "düzleştirilmiş" dir. 

Daha somut olarak, ```/document/content/organizations/*/description``` örneğin, *açıklamalar* alanındaki veriler dizine girmeden önce düz bir açıklama dizisi gibi görünür:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```

Bu önemli bir ilkedir, bu yüzden başka bir örnek verecektir. Zenginleştirme ağacının bir parçası olarak karmaşık türleri bir dizi olduğunu düşünün. Aşağıda açıklanan gibi karmaşık türleri bir dizi var customEntities adlı bir üye olduğunu varsayalım.

```json
"document/customEntities": 
[
    {
        "name": "heart failure",
        "matches": [
            {
                "text": "heart failure",
                "offset": 10,
                "length": 12,
                "matchDistance": 0.0
            }
        ]
    },
    {
        "name": "morquio",
        "matches": [
            {
                "text": "morquio",
                "offset": 25,
                "length": 7,
                "matchDistance": 0.0
            }
        ]
    }
    //...
]
```

Dizinizinizin, varlıkların adlarının her birini depolamak istediğiniz Toplama (Edm.String) türünde 'hastalıklar' adı verilen bir alana sahip olduğunu varsayalım. 

Bu, aşağıdaki gibi "\*" sembolü kullanılarak kolayca yapılabilir:

```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/customEntities/*/name",
            "targetFieldName": "diseases"
        }
    ]
```

Bu işlem, özel varlıklar öğelerinin adlarının her birinin "düzleştirilme" örneğini aşağıdaki gibi tek bir dize dizisinde n için ayıracaktır:

```json
  "diseases" : ["heart failure","morquio"]
```

## <a name="next-steps"></a>Sonraki adımlar
Zenginleştirilmiş alanlarınızı aranabilir alanlara eşledikten sonra, [dizin tanımının](search-what-is-an-index.md)bir parçası olarak aranabilir alanların her biri için alan özniteliklerini ayarlayabilirsiniz.

Alan eşleme hakkında daha fazla bilgi için [Azure Bilişsel Arama dizinleyicilerinde Alan eşlemelerine](search-indexer-field-mappings.md)bakın.
