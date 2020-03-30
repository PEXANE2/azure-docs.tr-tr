---
title: Küçümsülen bilişsel beceriler
titleSuffix: Azure Cognitive Search
description: Bu sayfa, amortismana tabi olduğu düşünülen ve yakın gelecekte Azure Bilişsel Arama beceri lerinde desteklenmeyen bilişsel becerilerin bir listesini içerir.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 5f3587e4398be28cbaa2372be720258196bb48ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792028"
---
# <a name="deprecated-cognitive-skills-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da azalan bilişsel beceriler

Bu belge, amortismana ermiş olarak kabul edilen bilişsel becerileri açıklar. İçerikler için aşağıdaki kılavuzu kullanın:

* Beceri Adı: Küçümsülecek becerinin adı, özniteliğe eşlenir. @odata.type
* Kullanılabilir son api sürümü: Azure Bilişsel Arama genel API'sinin, ilgili amortismana uygun becerileri içeren becerilerin oluşturulabileceği/güncelleştirilebileceği son sürümü.
* Desteğin sonu: İlgili becerinin desteklenmediği son gün. Daha önce oluşturulan beceri kümeleri çalışmaya devam etmelidir, ancak kullanıcıların azat edilmiş bir beceriden uzaklaşmaları önerilir.
* Öneriler: Desteklenen bir beceriyi kullanmak için geçiş yolu ileri. Kullanıcıların destek almaya devam etmek için önerileri izlemeleri tavsiye edilir.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityTanıma Beceri

### <a name="last-available-api-version"></a>Kullanılabilir son api sürümü

2017-11-11-Önizleme

### <a name="end-of-support"></a>Destek sonu

15 Şubat 2019

### <a name="recommendations"></a>Öneriler 

Bunun yerine [Microsoft.Skills.Text.EntityRecognitionSkill'i](cognitive-search-skill-entity-recognition.md) kullanın. NamedEntityRecognitionSkill'in işlevselliğinin çoğunu daha yüksek kalitede sağlar. Ayrıca karmaşık çıktı alanlarında daha zengin bilgilere sahiptir.

[Varlık Tanıma Becerisine](cognitive-search-skill-entity-recognition.md)geçiş yapmak için, beceri tanımınızda aşağıdaki değişikliklerden birini veya birkaçını gerçekleştirmeniz gerekir. [Beceri tanımını Güncelleme Skillset API'sini](https://docs.microsoft.com/rest/api/searchservice/update-skillset)kullanarak güncelleştirebilirsiniz.

> [!NOTE]
> Şu anda, bir kavram olarak güven puanı desteklenmez. `minimumPrecision` Parametre gelecekteki kullanım `EntityRecognitionSkill` ve geriye dönük uyumluluk için vardır.

1. *(Gerekli)* 'den `@odata.type` `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` ' `"#Microsoft.Skills.Text.EntityRecognitionSkill"`e değiştirin.

2. *(İsteğe bağlı)* Çıktıyı `entities` kullanıyorsanız, bunun yerine `namedEntities` karmaşık koleksiyon çıktısını `EntityRecognitionSkill` kullanın. Beceri `targetName` tanımındaki sözcük tanımını kullanarak onu ' `entities`adlı bir ek açıklamayla eşleyebilirsiniz.

3. *(İsteğe bağlı)* Açıkça `categories`belirtmezseniz, tarafından desteklenenkategorilerin yanı sıra farklı türde kategoriler `NamedEntityRecognitionSkill`de `EntityRecognitionSkill` döndürülebilir. Bu davranış istenmiyorsa, parametreyi `categories` açıkça `["Person", "Location", "Organization"]`'ye ayarladık.

    _Örnek Geçiş Tanımları_

    * Basit geçiş

        _(Daha önce) NamedEntityTanıma beceri tanımı_
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
        _(Sonra) EntityTanıma beceri tanımı_
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

        _(Daha önce) NamedEntityTanıma beceri tanımı_
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
        _(Sonra) EntityTanıma beceri tanımı_
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
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Varlık Tanıma Becerisi](cognitive-search-skill-entity-recognition.md)
