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
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 022b16669791b9b9cce066b3dd17c70b33569cc0
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955243"
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


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası geliştirme yaşam döngüsü](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz.

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
