---
title: Kullanım dışı bilişsel yetenekler
titleSuffix: Azure Cognitive Search
description: Bu sayfa kullanım dışı kabul edilen bilişsel yeteneklerin bir listesini içerir ve Azure Bilişsel Arama becerileri 'in yakın bir tarafında desteklenmeyecektir.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 85f3b9862bd8155c1a4b11860dc82d92a2f9e810
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88936104"
---
# <a name="deprecated-cognitive-skills-in-azure-cognitive-search"></a>Azure Bilişsel Arama kullanım dışı bilişsel yetenekler

Bu belge kullanım dışı kabul edilen bilişsel becerileri açıklar. İçerik için aşağıdaki kılavuzu kullanın:

* Beceri adı: kullanım dışı bırakılacak yeteneğin adı, @odata.type özniteliğiyle eşlenir.
* Son kullanılabilir API sürümü: karşılık gelen kullanım dışı beceriye sahip becerileri ile Azure Bilişsel Arama genel API 'sinin son sürümü oluşturulabilir/güncelleştirilir.
* Destek sonu: ilgili yeteneğin desteklenmeyen kabul edildiği son gün. Daha önce oluşturulan becerileri hala çalışmaya devam etmelidir, ancak kullanıcıların kullanım dışı bir beceriye geçirilmesi önerilir.
* Öneriler: geçiş yolu, desteklenen bir yetenek kullanacak şekilde ilet. Kullanıcıların destek almaya devam etmek için önerileri izlemesi önerilir.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft. yetenekler. Text. Namedentityrecognitionbeceri

### <a name="last-available-api-version"></a>Son kullanılabilir API sürümü

2017-11-11-Önizleme

### <a name="end-of-support"></a>Destek sonu

15 Şubat 2019

### <a name="recommendations"></a>Öneriler 

Bunun yerine [Microsoft. beceriler. Text. Entityrecognitionbeceri](cognitive-search-skill-entity-recognition.md) kullanın. Daha yüksek bir kalitede Namedentityrecognitionbeceri işlevlerinin çoğunu sağlar. Ayrıca, karmaşık çıkış alanlarında daha zengin bilgiler içerir.

[Varlık tanıma beceriye](cognitive-search-skill-entity-recognition.md)geçiş yapmak için, yetenek Tanımınızda aşağıdaki değişikliklerden birini veya daha fazlasını gerçekleştirmeniz gerekir. Yetenek tanımını [Update BECERI API](/rest/api/searchservice/update-skillset)'sini kullanarak güncelleştirebilirsiniz.

> [!NOTE]
> Şu anda, kavram olarak Güvenirlik puanı desteklenmez. `minimumPrecision`Parametresi, `EntityRecognitionSkill` daha sonra kullanılmak üzere ve geriye dönük uyumluluk için ' de bulunur.

1. *(Gerekli)* Öğesini `@odata.type` olarak değiştirin `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` `"#Microsoft.Skills.Text.EntityRecognitionSkill"` .

2. *(Isteğe bağlı)* Çıktıyı kullanıyorsanız, `entities` `namedEntities` bunun yerine karmaşık koleksiyon çıktısını kullanın `EntityRecognitionSkill` . Yetenek tanımında öğesini kullanarak, `targetName` adlı bir ek açıklamayla eşleme yapabilirsiniz `entities` .

3. *(Isteğe bağlı)* Açıkça belirtmezseniz,, `categories` `EntityRecognitionSkill` tarafından desteklenenlerden farklı türde Kategoriler döndürebilir `NamedEntityRecognitionSkill` . Bu davranış istenmeyen ise, parametresini açıkça ayarladığınızdan emin olun `categories` `["Person", "Location", "Organization"]` .

    _Örnek geçiş tanımları_

    * Basit geçiş

        _Eklenmeden Namedentitytanımayı yetenek tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _Sonra Entitytanıması yetenek tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * Biraz karmaşık geçiş

        _Eklenmeden Namedentitytanımayı yetenek tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _Sonra Entitytanıması yetenek tanımı_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Varlık tanıma yeteneği](cognitive-search-skill-entity-recognition.md)