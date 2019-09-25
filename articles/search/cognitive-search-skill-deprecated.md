---
title: Kullanım dışı bilişsel yetenekler-Azure Search
description: Bu sayfa kullanım dışı kabul edilen bilişsel arama becerileri listesini içerir ve yakın gelecekte desteklenecektir.
services: search
manager: nitinme
author: luiscabrer
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.openlocfilehash: 1e78852ec8b92f1a9e37a4dbcbbcb371c0ac0f97
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265430"
---
# <a name="deprecated-cognitive-search-skills"></a>Kullanım dışı bilişsel arama becerileri

Bu belge kullanım dışı kabul edilen bilişsel becerileri açıklar. İçerik için aşağıdaki kılavuzu kullanın:

* Beceri adı: Kullanım dışı bırakılacak yeteneğin adı, @odata.type özniteliğiyle eşlenir.
* Son kullanılabilir API sürümü: Azure Search ortak API 'sinin, karşılık gelen kullanım dışı bırakılmış beceriye sahip becerileri ile oluşturulabileceği/güncelleştirilebilen son sürümü.
* Destek sonu: Karşılık gelen yeteneğin desteklenmeyen kabul edildiği son gün. Daha önce oluşturulan becerileri hala çalışmaya devam etmelidir, ancak kullanıcıların kullanım dışı bir beceriye geçirilmesi önerilir.
* Öneri Geçiş yolu, desteklenen bir yetenek kullanacak şekilde ilet. Kullanıcıların destek almaya devam etmek için önerileri izlemesi önerilir.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft. yetenekler. Text. Namedentityrecognitionbeceri

### <a name="last-available-api-version"></a>Son kullanılabilir API sürümü

2017-11-11-Önizleme

### <a name="end-of-support"></a>Destek sonu

15 Şubat 2019

### <a name="recommendations"></a>Öneriler 

Bunun yerine [Microsoft. beceriler. Text. Entityrecognitionbeceri](cognitive-search-skill-entity-recognition.md) kullanın. Daha yüksek bir kalitede Namedentityrecognitionbeceri işlevlerinin çoğunu sağlar. Ayrıca, karmaşık çıkış alanlarında daha zengin bilgiler içerir.

[Varlık tanıma beceriye](cognitive-search-skill-entity-recognition.md)geçiş yapmak için, yetenek Tanımınızda aşağıdaki değişikliklerden birini veya daha fazlasını gerçekleştirmeniz gerekir. Yetenek tanımını [Update BECERI API](https://docs.microsoft.com/rest/api/searchservice/update-skillset)'sini kullanarak güncelleştirebilirsiniz.

> [!NOTE]
> Şu anda, kavram olarak Güvenirlik puanı desteklenmez. `minimumPrecision` Parametresi ,`EntityRecognitionSkill` daha sonra kullanılmak üzere ve geriye dönük uyumluluk için ' de bulunur.

1. *(Gerekli)* `@odata.type` Öğesini`"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` olarak değiştirin`"#Microsoft.Skills.Text.EntityRecognitionSkill"`.

2. *(Isteğe bağlı)* `entities` Çıktıyı kullanıyorsanız, `EntityRecognitionSkill` bunun yerine `namedEntities` karmaşık koleksiyon çıktısını kullanın. Yetenek tanımında öğesini kullanarak `targetName` , adlı `entities`bir ek açıklamayla eşleme yapabilirsiniz.

3. *(Isteğe bağlı)* Açıkça belirtmezseniz `categories` `EntityRecognitionSkill` ,, tarafından `NamedEntityRecognitionSkill`desteklenenlerden farklı türde Kategoriler döndürebilir. Bu davranış istenmeyen ise, `categories` `["Person", "Location", "Organization"]`parametresini açıkça ayarladığınızdan emin olun.

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

+ [Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Varlık tanıma yeteneği](cognitive-search-skill-entity-recognition.md)
