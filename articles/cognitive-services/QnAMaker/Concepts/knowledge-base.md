---
title: Bilgi Bankası - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma Bilgi Bankası, her QnA çiftiyle ilişkili bir soru-cevap (QnA) çiftleri ve isteğe bağlı meta veri kümesi içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 78fc9fe34eb3463021dae69990fe1d30668d453f
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300502"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma Bilgi Bankası nedir?

Soru-Cevap Oluşturma Bilgi Bankası, her QnA çiftiyle ilişkili bir soru-cevap (QnA) çiftleri ve isteğe bağlı meta veri kümesi içerir.

## <a name="key-knowledge-base-concepts"></a>Bilgi Bankası anahtar kavramlar

* **Sorular**: Bir soru, Kullanıcı sorgusunu en iyi şekilde temsil eden metni içerir. 
* **Yanıtlar**: Yanıt, bir Kullanıcı sorgusu ilişkili soru ile eşleştiğinde döndürülen yanıttır.  
* **Meta veriler**: Meta veriler bir QnA çiftiyle ilişkili olan ve anahtar-değer çiftleri olarak temsil edilen etiketlerdir. Meta veri etiketleri, QnA çiftlerini filtrelemek ve sorgu eşleştirmesinin gerçekleştirileceği kümeyi sınırlamak için kullanılır.

Sayısal bir soru-cevap kimliği tarafından temsil edilen bir tek soru-cevap, soru (diğer Sorular) tümünü tek bir yanıt harita birden çok çeşitlemesi vardır. Buna ek olarak, her bir çiftin ilişkili birden fazla meta veri alanı olabilir: bir anahtar ve bir değer.

![Soru-cevap Oluşturucu bilgi bankalarından](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Bilgi Bankası içerik biçimi

Bilgi Bankası zengin içerik içe alma, içerik markdown biçimine dönüştürmek soru-cevap Oluşturucu çalışır. Çoğu sohbet istemcisi tarafından anlaşılabilen markın biçimleri hakkında bilgi edinmek için [Bu blogu](https://aka.ms/qnamaker-docs-markdown-support) okuyun.

Meta veri alanları, ürün: Shredder gibi iki noktayla ayrılmış anahtar-değer çiftlerinden oluşur. Hem anahtar hem de değer salt metin olmalıdır. Meta verileri anahtar boşluk içermemelidir. Meta veriler anahtar başına yalnızca bir değeri destekler.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Soru-Cevap Oluşturma en iyi yanıtı seçmek için Kullanıcı sorgusunu nasıl işler?

Eğitilen ve [yayınlanan](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) soru-cevap oluşturma Bilgi Bankası, bir bot veya başka bir Istemci uygulamasından [generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sindeki bir Kullanıcı sorgusu alır. Aşağıdaki diyagramda, Kullanıcı sorgusu alındığında işlem gösterilmektedir.

![Bir Kullanıcı sorgusu için derecelendirme işlemi](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

### <a name="ranker-process"></a>Ranker işlemi

İşlem aşağıdaki tabloda açıklanmıştır.

|Adım|Amaç|
|--|--|
|1\.|İstemci uygulaması, Kullanıcı sorgusunu [Generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sine gönderir.|
|2|Soru-Cevap Oluşturma dil algılama, yazım ve sözcük ayırıcılarını kullanarak Kullanıcı sorgusunu önceden işler.|
|3|Bu ön işleme, en iyi arama sonuçları için Kullanıcı sorgusunu değiştirmek üzere alınır.|
|4|Değiştirilen sorgu Azure Search dizine gönderilir ve bu da sonuçların `top` sayısını alır. Bu sonuçlarda doğru yanıt yoksa, `top` biraz değerini artırın. Genellikle, için `top` 10 değeri sorguların% 90 ' de işe yarar.|
|5|Soru-Cevap Oluşturma, Kullanıcı sorgusunun getirilen Azure Search sonuçlarının doğruluğunu belirlemede gelişmiş özellikler uygular. |
|6|Eğitilen su modeli, Azure Search sonuçlarını derecelendirmek için 5. adımdaki Özellik Puanını kullanır.|
|7|Yeni sonuçlar, istemci uygulamasına derecelendirilir sırada döndürülür.|
|||

Kullanılan özellikler arasında şunlar yer alır, ancak sözcük düzeyi semantikleri, bir Corpus içindeki terim düzeyi önem derecesi ve derin öğrenilen anlam modelleri, iki metin dizesi arasında benzerlik ve ilgi belirleme açısından sınırlı değildir.

## <a name="http-request-and-response-with-endpoint"></a>Uç nokta ile HTTP isteği ve yanıtı
Bilgi bankanızı yayımladığınızda, hizmet, uygulama ile tümleştirilen bir sohbet bot ile tümleştirilebilen REST tabanlı bir HTTP uç noktası oluşturur. 

### <a name="the-user-query-request-to-generate-an-answer"></a>Bir yanıt oluşturmak için Kullanıcı sorgulama isteği

Kullanıcı sorgusu, son kullanıcının bilgi bankasını `How do I add a collaborator to my app?`sorduğu sorudır. Sorgu genellikle doğal dil biçiminde veya soruyu `help with collaborators`temsil eden birkaç anahtar sözcüğe (gibi) sahiptir. Sorgu, istemci uygulamanızdaki bir HTTP isteğinden bilgi tabanınızdan gönderilir.

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

[Scorethreshold](./confidence-score.md#choose-a-score-threshold), [top](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)ve [strictfilters](../how-to/metadata-generateanswer-usage.md#filter-results-with-strictfilters-for-metadata-tags)gibi özellikleri ayarlayarak yanıtı kontrol edersiniz.

Doğru ve nihai yanıtı bulmak için konuşmayı, soruları ve yanıtları belirginleştirebilmek için [Çoklu açma işleviyle](../how-to/multiturn-conversation.md) birlikte [konuşma bağlamını](../how-to/metadata-generateanswer-usage.md#use-question-and-answer-results-to-keep-conversation-context) kullanın.

### <a name="the-response-from-a-call-to-generate-an-answer"></a>Yanıt oluşturmak için bir çağrıdan yanıt

HTTP yanıtı, belirli bir Kullanıcı sorgusuna en iyi eşleşme temelinde Bilgi Bankası 'ndan alınan yanıttır. Yanıt, yanıtı ve tahmin Puanını içerir. `top` Özelliği ile birden fazla en iyi yanıt sorulursa, her biri puanı olan birden fazla top yanıtı alırsınız. 

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

### <a name="test-and-production-knowledge-base"></a>Test ve üretim Bilgi Bankası
Bilgi Bankası, Soru-Cevap Oluşturma ile oluşturulan, tutulan ve kullanılan soruların ve yanıtların deposudur. Her Soru-Cevap Oluşturma katmanı, birden fazla bilgi tabanı için kullanılabilir.

Bilgi Bankası 'nda iki durum vardır: *Test* ve *yayımlandı*.

*Test bilgi tabanı* , doğruluk ve yanıtların eksiksiz olması için düzenlenen, kaydedilen ve sınanan sürümdür. Test Bilgi Bankası 'nda yapılan değişiklikler uygulamanızın son kullanıcısını veya sohbet bot 'ı etkilemez. Test bilgi tabanı, http isteğinde olarak `test` bilinir. 

*Yayımlanan bilgi tabanı* , sohbet bot veya uygulamanızda kullanılan sürümdür. Bilgi Bankası yayımlama eylemi, bilgi bankasındaki yayımlanmış sürüme test bilgi tabanı içeriğini koyar. Yayımlanan bilgi tabanı uygulamanın uç nokta aracılığıyla kullandığı sürüm olduğundan, içeriğin doğru ve iyi test edildiğinden emin olun. Yayımlanan bilgi tabanı, http isteğinde olarak `prod` bilinir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)

İle bir Bilgi Bankası oluşturun ve düzenleyin: 
* [REST API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [.NET SDK](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Şunu kullanarak bir yanıt oluşturun: 
* [REST API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [.NET SDK](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
