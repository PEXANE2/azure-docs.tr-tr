---
title: Bilgi tabanını sorgula - QnA Maker
description: Bir bilgi tabanı yayınlanmalıdır. Yayımlandıktan sonra, bilgi tabanı, generateAnswer API kullanılarak çalışma zamanı tahmin bitiş noktasında sorgulanır.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: cb777aa16fada50811cce1bbf49f28662c62b49b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220726"
---
# <a name="query-the-knowledge-base-for-answers"></a>Yanıtlar için bilgi tabanını sorgula

Bir bilgi tabanı yayınlanmalıdır. Yayımlandıktan sonra, bilgi tabanı, generateAnswer API kullanılarak çalışma zamanı tahmin bitiş noktasında sorgulanır. Sorgu, QnA Maker'ın bir yanıtla mümkün olan en iyi eşleşmeyi seçmesine yardımcı olmak için soru metnini ve diğer ayarları içerir.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>QnA Maker en iyi yanıtı seçmek için kullanıcı sorgusunu nasıl işler?

Eğitilmiş ve [yayınlanan](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) QnA Maker bilgi [bankası, GenerateAnswer API'de](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)bir bot veya başka bir istemci uygulamasından bir kullanıcı sorgusu alır. Aşağıdaki diyagram, kullanıcı sorgusu nun alındığı işlemi göstermektedir.

![Kullanıcı sorgusu için sıralama modeli işlemi](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

### <a name="ranker-process"></a>Ranker işlemi

İşlem aşağıdaki tabloda açıklanmıştır.

|Adım|Amaç|
|--|--|
|1|İstemci uygulaması kullanıcı sorgusunu [GenerateAnswer API'sine](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)gönderir.|
|2|QnA Maker, kullanıcı sorgusunu dil algılama, yazım layıcılar ve sözcük ayırıcılarla önceden işler.|
|3|Bu ön işleme, en iyi arama sonuçları için kullanıcı sorgusunu değiştirmek için alınır.|
|4|Bu değiştirilmiş sorgu, sonuç sayısını alan bir Azure `top` Bilişsel Arama Dizini'ne gönderilir. Bu sonuçlarda doğru yanıt yoksa, değerini biraz `top` artırın. Genellikle, sorguların% 90 `top` çalışır için 10 değeri.|
|5|QnA Maker, kullanıcı sorgusu ile getirilen QnA sonuçları arasındaki benzerliği belirlemek için sözdizimve anlamsal tabanlı featurization kullanır.|
|6|Makineden öğrenilen sıralamacı modeli, güven puanlarını ve yeni sıralama sırasını belirlemek için adım 5'ten farklı özellikleri kullanır.|
|7|Yeni sonuçlar sıralı sırada istemci uygulamasına döndürülür.|
|||

Kullanılan özellikler, sözcük düzeyindesemantik, bir korpusta terim düzeyi önemi ve iki metin dizeleri arasındaki benzerlik ve alaka düzeyini belirlemek için derin öğrenilen semantik modelleri içerir, ancak bunlarla sınırlı değildir.

## <a name="http-request-and-response-with-endpoint"></a>UÇ NOKTAİlE HTTP isteği ve yanıtı
Bilgi tabanınızı yayımladığınızda, hizmet genellikle bir sohbet botu olan uygulamanıza entegre edilebilen REST tabanlı bir HTTP bitiş noktası oluşturur.

### <a name="the-user-query-request-to-generate-an-answer"></a>Yanıt oluşturmak için kullanıcı sorgusu isteği

Kullanıcı sorgusu, son kullanıcının bilgi tabanını sorduğu sorudur. `How do I add a collaborator to my app?` Sorgu genellikle doğal dil biçiminde veya soruyu temsil eden birkaç `help with collaborators`anahtar kelimededir, örneğin. Sorgu, istemci uygulamanızdaki bir HTTP isteğinden bilgi tabanınıza gönderilir.

```json
{
    "question": "How do I add a collaborator to my app?",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "QuestionType",
        "value": "Support"
    }],
    "userId": "sd53lsY="
}
```

[Yanıtı scoreThreshold](./confidence-score.md#choose-a-score-threshold), [top](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)ve strictFilters gibi özellikleri ayarlayarak [denetlersiniz.](../how-to/metadata-generateanswer-usage.md#filter-results-with-strictfilters-for-metadata-tags)

Doğru ve son yanıtı bulmak için, konuşmanın soruları ve yanıtları hassaslaştırmasına devam etmek için [çok döndürme işlevine](../how-to/multiturn-conversation.md) sahip [konuşma bağlamını](../how-to/metadata-generateanswer-usage.md#use-question-and-answer-results-to-keep-conversation-context) kullanın.

### <a name="the-response-from-a-call-to-generate-an-answer"></a>Yanıt oluşturmak için bir çağrının yanıtı

HTTP yanıtı, belirli bir kullanıcı sorgusu için en iyi eşleşmeyi temel alan bilgi tabanından alınan yanıttır. Yanıt, yanıtı ve tahmin puanını içerir. `top` Özellik ile birden fazla üst yanıt istediyseniz, her biri bir puana sahip birden fazla üst yanıt alırsınız.

```json
{
    "answers": [
        {
            "questions": [
                "How do I add a collaborator to my app?",
                "What access control is provided for the app?",
                "How do I find user management and security?"
            ],
            "answer": "Use the Azure portal to add a collaborator using Access Control (IAM)",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "QuestionType",
                    "value": "Support"
                },
                {
                    "name": "ToolDependency",
                    "value": "Azure Portal"
                }
            ]
        }
    ]
}
```


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenilirlik puanı](./confidence-score.md)
