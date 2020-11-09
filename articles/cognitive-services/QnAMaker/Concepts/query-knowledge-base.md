---
title: Bilgi Bankası Soru-Cevap Oluşturma sorgulama-
description: Bilgi Bankası 'nın yayımlanması gerekir. Bilgi Bankası, yayımlandıktan sonra, generateAnswer API kullanılarak çalışma zamanı tahmin uç noktasında sorgulanır.
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: e8dd056a7b6357b8342d3059e17baa88db92b404
ms.sourcegitcommit: 051908e18ce42b3b5d09822f8cfcac094e1f93c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94376735"
---
# <a name="query-the-knowledge-base-for-answers"></a>Bilgi Bankası yanıtlarını yanıtlar için sorgulama

Bilgi Bankası 'nın yayımlanması gerekir. Bilgi Bankası, yayımlandıktan sonra, generateAnswer API kullanılarak çalışma zamanı tahmin uç noktasında sorgulanır. Sorgu, soru metnini ve diğer ayarları içerir. Bu, bir yanıta en iyi eşleşmeyi Soru-Cevap Oluşturma seçmenize yardımcı olabilir.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Soru-Cevap Oluşturma en iyi yanıtı seçmek için Kullanıcı sorgusunu nasıl işler?

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Eğitilen ve [yayınlanan](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) soru-cevap oluşturma Bilgi Bankası, bir bot veya başka bir Istemci uygulamasından [generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sindeki bir Kullanıcı sorgusu alır. Aşağıdaki diyagramda, Kullanıcı sorgusu alındığında işlem gösterilmektedir.

![Bir Kullanıcı sorgusu için derecelendirme modeli işlemi](../media/qnamaker-concepts-knowledgebase/ranker-v1.png)

### <a name="ranker-process"></a>Ranker işlemi

İşlem aşağıdaki tabloda açıklanmıştır.

|Adım|Amaç|
|--|--|
|1|İstemci uygulaması, Kullanıcı sorgusunu [Generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sine gönderir.|
|2|Soru-Cevap Oluşturma dil algılama, yazım ve sözcük ayırıcılarını kullanarak Kullanıcı sorgusunu önceden işler.|
|3|Bu ön işleme, en iyi arama sonuçları için Kullanıcı sorgusunu değiştirmek üzere alınır.|
|4|Değiştirilen Bu sorgu, sonuçların sayısını alan bir Azure Bilişsel Arama dizinine gönderilir `top` . Bu sonuçlarda doğru yanıt yoksa, biraz değerini artırın `top` . Genellikle, için 10 değeri `top` sorguların %90 ' de işe yarar.|
|5|Soru-Cevap Oluşturma, kullanıcı sorgusuyla getirilen QnA sonuçları arasındaki benzerliği belirlemede anlamlı ve anlamsal tabanlı bir şekilde kullanır.|
|6|Makine tarafından öğrenilen derecelendiricisini modeli, güven puanlarını ve yeni Derecelendirme sırasını öğrenmek için 5. adımdan farklı özellikleri kullanır.|
|7|Yeni sonuçlar, istemci uygulamasına derecelendirilir sırada döndürülür.|
|||

Kullanılan özellikler arasında şunlar yer alır, ancak sözcük düzeyi semantikleri, bir Corpus içindeki terim düzeyi önem derecesi ve derin öğrenilen anlam modelleri, iki metin dizesi arasında benzerlik ve ilgi belirleme açısından sınırlı değildir.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Eğitilen ve [yayınlanan](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) soru-cevap oluşturma Bilgi Bankası, bir bot veya başka bir Istemci uygulamasından [generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sindeki bir Kullanıcı sorgusu alır. Aşağıdaki diyagramda, Kullanıcı sorgusu alındığında işlem gösterilmektedir.

![Kullanıcı sorgu önizlemesi için derecelendirme modeli işlemi](../media/qnamaker-concepts-knowledgebase/ranker-v2.png)

### <a name="ranker-process"></a>Ranker işlemi

İşlem aşağıdaki tabloda açıklanmıştır.

|Adım|Amaç|
|--|--|
|1|İstemci uygulaması, Kullanıcı sorgusunu [Generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sine gönderir.|
|2|Soru-Cevap Oluşturma dil algılama, yazım ve sözcük ayırıcılarını kullanarak Kullanıcı sorgusunu önceden işler.|
|3|Bu ön işleme, en iyi arama sonuçları için Kullanıcı sorgusunu değiştirmek üzere alınır.|
|4|Değiştirilen Bu sorgu, sonuçların sayısını alan bir Azure Bilişsel Arama dizinine gönderilir `top` . Bu sonuçlarda doğru yanıt yoksa, biraz değerini artırın `top` . Genellikle, için 10 değeri `top` sorguların %90 ' de işe yarar.|
|5|Soru-Cevap Oluşturma, Kullanıcı sorgusu ile Azure Bilişsel Arama getirilen aday QnA sonuçları arasındaki benzerliği tespit etmek için, resim durumu dönüştürücü tabanlı modeli kullanır. Transformatör tabanlı model, tüm dillerin güven puanlarını ve yeni Derecelendirme sırasını belirlemesi için yatay olarak çalışarak, derinlemesine bir öğrenme çok dilli modelidir.|
|6|Yeni sonuçlar, istemci uygulamasına derecelendirilir sırada döndürülür.|
|||

Derecelendiricisini, Kullanıcı sorgusunun en iyi eşleşen QNA çiftlerini bulmak için diğer tüm sorular ve yanıtlar üzerinde çalışmaktadır. Kullanıcıların derecelendiricisini 'ı yalnızca derecelendiricisini 'a Yapılandırma esnekliği vardır. 

---

## <a name="http-request-and-response-with-endpoint"></a>Uç nokta ile HTTP isteği ve yanıtı
Bilgi bankanızı yayımladığınızda, hizmet, uygulama ile tümleştirilen bir sohbet bot ile tümleştirilebilen REST tabanlı bir HTTP uç noktası oluşturur.

### <a name="the-user-query-request-to-generate-an-answer"></a>Bir yanıt oluşturmak için Kullanıcı sorgulama isteği

Kullanıcı sorgusu, son kullanıcının bilgi bankasını sorduğu sorudır `How do I add a collaborator to my app?` . Sorgu genellikle doğal dil biçiminde veya soruyu temsil eden birkaç anahtar sözcüğe (gibi) sahiptir `help with collaborators` . Sorgu, istemci uygulamanızdaki bir HTTP isteğinden bilgi tabanınızdan gönderilir.

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

[Scorethreshold](./confidence-score.md#choose-a-score-threshold), [top](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)ve [strictfilters](../how-to/metadata-generateanswer-usage.md#filter-results-with-strictfilters-for-metadata-tags)gibi özellikleri ayarlayarak yanıtı kontrol edersiniz.

Doğru ve nihai yanıtı bulmak için konuşmayı, soruları ve yanıtları belirginleştirebilmek için [Çoklu açma işleviyle](../how-to/multiturn-conversation.md) birlikte [konuşma bağlamını](../how-to/metadata-generateanswer-usage.md#use-question-and-answer-results-to-keep-conversation-context) kullanın.

### <a name="the-response-from-a-call-to-generate-an-answer"></a>Yanıt oluşturmak için bir çağrıdan yanıt

HTTP yanıtı, belirli bir Kullanıcı sorgusuna en iyi eşleşme temelinde Bilgi Bankası 'ndan alınan yanıttır. Yanıt, yanıtı ve tahmin Puanını içerir. Özelliği ile birden fazla en iyi yanıt sorulursa, her biri `top` puanı olan birden fazla top yanıtı alırsınız.

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