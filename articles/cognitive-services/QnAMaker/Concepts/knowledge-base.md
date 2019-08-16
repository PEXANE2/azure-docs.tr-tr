---
title: Bilgi Bankası - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru/yanıt (soru-cevap) çifti ve isteğe bağlı meta veriler her soru-cevap çifti ile ilişkili bir dizi soru-cevap Oluşturucu Bilgi Bankası oluşur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 8cd63913c0e96d496aa617369601c1dd121b4b46
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69542852"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Temel bir soru-cevap Oluşturucu bilgi nedir?

Soru/yanıt (soru-cevap) çifti ve isteğe bağlı meta veriler her soru-cevap çifti ile ilişkili bir dizi soru-cevap Oluşturucu Bilgi Bankası oluşur.

## <a name="key-knowledge-base-concepts"></a>Bilgi Bankası anahtar kavramlar

* **Sorular** -soru kullanıcı sorgusu en iyi temsil eden metin içeriyor. 
* **Yanıtlar** -kullanıcı sorgusu ile ilişkili soru eşleştiğinde, döndürülen yanıt cevaptır.  
* **Meta veri** -meta verileri bir soru-cevap çifti ile ilişkili etiket ve anahtar-değer çiftleri olarak temsil edilir. Meta veri etiketleri, QnA çiftlerini filtrelemek ve sorgu eşleştirmesinin gerçekleştirileceği kümeyi sınırlamak için kullanılır.

Sayısal bir soru-cevap kimliği tarafından temsil edilen bir tek soru-cevap, soru (diğer Sorular) tümünü tek bir yanıt harita birden çok çeşitlemesi vardır. Ayrıca, her bir çiftin ilişkili birden fazla meta veri alanı olabilir: bir anahtar ve bir değer.

![Soru-cevap Oluşturucu bilgi bankalarından](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Bilgi Bankası içerik biçimi

Bilgi Bankası zengin içerik içe alma, içerik markdown biçimine dönüştürmek soru-cevap Oluşturucu çalışır. Okuma [bu](https://aka.ms/qnamaker-docs-markdown-support) markdown anlamak için blog çoğu sohbet istemciler tarafından anlaşılır biçimlendirir.

Meta veri alanları, anahtar-değer çiftleri virgül ile ayrılmış oluşur **(ürün: öğütücü)** . Hem anahtar hem de değer salt metin olmalıdır. Meta verileri anahtar boşluk içermemelidir. Meta veriler anahtar başına yalnızca bir değeri destekler.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Soru-Cevap Oluşturma en iyi yanıtı seçmek için Kullanıcı sorgusunu nasıl işler?

Eğitilen ve [yayınlanan](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) soru-cevap oluşturma Bilgi Bankası, bir bot veya başka bir Istemci uygulamasından [generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sindeki bir Kullanıcı sorgusu alır. Aşağıdaki diyagramda, Kullanıcı sorgusu alındığında işlem gösterilmektedir.

![Bir Kullanıcı sorgusu için derecelendirme işlemi](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

İşlem aşağıdaki tabloda açıklanmıştır:

|Adım|Amaç|
|--|--|
|1\.|İstemci uygulaması, Kullanıcı sorgusunu [Generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)'sine gönderir.|
|2|QNA yapıcısı, dil algılama, yazım ve sözcük ayırıcılarını içeren Kullanıcı sorgusunu ön işleme.|
|3|Bu ön işleme, en iyi arama sonuçları için Kullanıcı sorgusunu değiştirmek üzere alınır.|
|4|Değiştirilen sorgu Azure Search dizine gönderilir ve sonuçların `top` sayısı alınıyor. Bu sonuçlarda doğru yanıt yoksa, `top` biraz değerini artırın. Genellikle 10 `top` ' un bir değeri, sorguların% 90 ' de işe yarar.|
|5|Soru-Cevap Oluşturma, Kullanıcı sorgusunun getirilen Azure Search sonuçlarının doğruluğunu belirlemede gelişmiş özellikler uygular. |
|6|Eğitilen su modeli, Azure Search sonuçlarını derecelendirmek için 5. adımdaki Özellik Puanını kullanır.|
|7|Yeni sonuçlar, istemci uygulamasına derecelendirilir sırada döndürülür.|
|||

Kullanılan özellikler arasında şunlar yer alır, ancak sözcük düzeyi semantikleri, bir Corpus içindeki terim düzeyi önem derecesi ve derin öğrenilen anlam modelleri, iki metin dizesi arasında benzerlik ve ilgi belirleme açısından bunlarla sınırlı değildir.

## <a name="http-request-and-response-with-endpoint"></a>Uç nokta ile HTTP isteği ve yanıtı
Bilgi bankanızı yayımladığınızda, hizmet, uygulama ile tümleştirilen bir sohbet bot ile tümleştirilebilen REST tabanlı bir HTTP **uç noktası** oluşturur. 

### <a name="the-user-query-request-to-generate-an-answer"></a>Bir yanıt oluşturmak için Kullanıcı sorgulama isteği

**Kullanıcı sorgusu** , son kullanıcının bilgi bankasından, örneğin, `How do I add a collaborator to my app?`bir sorduğu sorudır. Sorgu genellikle doğal bir dil biçiminde veya soruyu temsil eden birkaç anahtar kelimedir, örneğin, `help with collaborators`. Sorgu, istemci uygulamanızdaki bir HTTP **isteğinden** bilginiz için gönderilir.

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

[Scorethreshold](./confidence-score.md#choose-a-score-threshold), [top](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)ve [stringfilters](../how-to/metadata-generateanswer-usage.md#filter-results-with-strictfilters-for-metadata-tags)gibi özellikleri ayarlayarak yanıtı kontrol edersiniz.

Doğru ve nihai yanıtı bulmak için konuşmayı, soruları ve yanıtları belirginleştirebilmek için [Çoklu açma işleviyle](../how-to/multiturn-conversation.md) birlikte [konuşma içeriğini](../how-to/metadata-generateanswer-usage.md#use-question-and-answer-results-to-keep-conversation-context) kullanın.

### <a name="the-response-from-a-call-to-generate-answer"></a>Yanıt oluşturmak için bir çağrıdan yanıt

HTTP **yanıtı** , belirli bir Kullanıcı sorgusuna en iyi eşleşme temelinde Bilgi Bankası 'ndan alınan yanıttır. Yanıt, yanıtı ve tahmin Puanını içerir. `top` Özelliği ile birden fazla en iyi yanıt sorulursa, her biri puanı olan birden fazla en iyi yanıt alırsınız. 

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
Bilgi Bankası, sorular deposudur ve soru-cevap Oluşturucu kullanılan oluşturulan ve tutulan yanıtlar. Her bir soru-cevap Oluşturucu katman birden çok bilgi bankaları için kullanılabilir.

Bilgi Bankası iki durumlu - Test sahiptir ve yayımladınız. 

**Test bilgi tabanı** , düzenlenen, kaydedilen ve yanıtların doğruluğu ve bütünlüğü açısından test edilmekte olan sürümdür. Test Bilgi Bankası'na yapılan değişiklikler, uygulama/sohbet Robotu son kullanıcısına etkilemez. Test bilgi tabanı, http isteğinde olarak `test` bilinir. 

**Yayımlanan bilgi tabanı** , sohbet bot/uygulamanızda kullanılan sürümdür. Bilgi Bankası Yayımlama eylemi Bilgi Bankası yayımlanan sürümünü Test Bilgi Bankası içeriği yerleştirir. Yayımlanan Bilgi Bankası uç noktası aracılığıyla uygulamanın kullandığı sürüm olduğundan, içeriği doğru ve test edilmiş iyi olduğundan emin olmak için dikkatli olunması. Yayımlanan bilgi tabanı, http isteğinde olarak `prod` bilinir. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)

İle bilgi tabanı oluştur ve Düzenle: 
* [REST API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [.NET SDK](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Yanıt oluştur: 
* [REST API](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [.NET SDK](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
