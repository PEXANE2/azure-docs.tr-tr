---
title: Dizin oluşturma sırasında yerleşik metin ve görüntü işleme
titleSuffix: Azure Cognitive Search
description: Veri çıkarma, doğal dil, görüntü işleme bilişsel becerileri, Azure Bilişsel Arama boru hattındaham içeriğe anlamsallık ve yapı ekler.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: b1cf1750924ab8ea8afff6ac788683565433866b
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81618024"
---
# <a name="built-in-cognitive-skills-for-text-and-image-processing-during-indexing-azure-cognitive-search"></a>Dizin oluşturma sırasında metin ve görüntü işleme için yerleşik bilişsel beceriler (Azure Bilişsel Arama)

Bu makalede, Azure Bilişsel Arama ile sağlanan ve içeriği ve yapıyı ayıklamak için bir beceriye dahil edebilirsiniz bilişsel beceriler hakkında bilgi edinin. *Bilişsel beceri,* içeriği bir şekilde dönüştüren bir modül veya işlemdir. Genellikle, veri ayıklar veya çıkars yapısı bir bileşenidir ve bu nedenle giriş verileri anlayışımızı genişletir. Hemen hemen her zaman, çıktı metin tabanlıdır. *Beceri,* zenginleştirme boru hattını tanımlayan beceriler topluluğudur. 

> [!NOTE]
> İşleme sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla Bilgi Al algoritması ekleyerek kapsamı genişlettikçe, [faturalandırılabilir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir. Bilişsel Hizmetler'de API'leri ararken ve Azure Bilişsel Arama'da belge çözme aşamasının bir parçası olarak görüntü ayıklama için ücretler tahakkuk ettirilir. Belgelerden metin çıkarma için herhangi bir ücret yoktur.
>
> Yerleşik becerilerin yürütülmesi, mevcut [Bilişsel Hizmetler ödeme-as-you gitmek fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/)tahsil edilir. Görüntü çıkarma fiyatlandırması [Azure Bilişsel Arama fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmıştır.
>
> [Artımlı zenginleştirme (önizleme)](cognitive-search-incremental-indexing-conceptual.md) özelliği, dizinleyicinin gelecekte becerilerinizi değiştirip zamandan ve paradan tasarruf etmenizi sağlarsa yalnızca gerekli olan bilişsel becerileri çalıştırmada daha verimli olmasını sağlayan bir önbellek sağlamanıza olanak tanır.


## <a name="built-in-skills"></a>Yerleşik yetenekler

Çeşitli beceriler, tükettikleri veya ürettikleri şeyde esnektir. Genel olarak, çoğu beceri önceden eğitilmiş modellere dayanır, bu da modeli kendi eğitim verilerinizi kullanarak eğitemeyeceğiniz anlamına gelir. Aşağıdaki tablo, Microsoft tarafından sağlanan becerileri sıralar ve açıklar. 

| Beceri | Açıklama |
|-------|-------------|
|[Microsoft.Skills.Text.CustomEntityLookupSkill](cognitive-search-skill-custom-entity-lookup.md)| Özel, kullanıcı tarafından tanımlanan sözcük ve tümcecikler listesinden metin arar.|
| [Microsoft.Skills.Text.KeyphraseSkill](cognitive-search-skill-keyphrases.md) | Bu beceri, terim yerleşimine, dil kurallarına, diğer terimlere yakınlığı ve terimin kaynak veriler içinde ne kadar olağandışı olduğuna bağlı olarak önemli ifadeleri algılamak için önceden eğitilmiş bir model kullanır. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Bu beceri, hangi dilin kullanıldığını (belge başına bir dil kimliği) algılamak için önceden eğitilmiş bir model kullanır. Aynı metin segmentleri içinde birden çok dil kullanıldığında, çıktı ağırlıklı olarak kullanılan dilin LCID'sidir.|
| [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md) | Alanlar koleksiyonundan gelen metni tek bir alanda birleştirir.  |
| [Microsoft.Skills.Text.EntityTanıma Beceri](cognitive-search-skill-entity-recognition.md) | Bu beceri, belirli kategoriler kümesi için varlıklar oluşturmak için önceden eğitilmiş bir model kullanır: kişiler, konum, kuruluş, e-postalar, URL'ler, tarih alanları. |
| [Microsoft.Skills.Text.PIIDetectionSkill](cognitive-search-skill-pii-detection.md)  | Bu beceri, belirli bir metinden kişisel olarak tanımlanabilir bilgileri ayıklamak için önceden eğitilmiş bir model kullanır. Bu beceri, metinde algılanan KIŞISEL bilgiler varlıklarını maskeleme için çeşitli seçenekler de sunar.  |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Bu beceri, kayıt bazında olumlu veya olumsuz duygular elde etmek için önceden eğitilmiş bir model kullanır. Skor 0 ile 1 arasındadır. Nötr puanları, hem algılanamadığı zaman hem de nötr olarak kabul edilen metin için null durum için oluşur.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | İçeriği artımlı olarak zenginleştirebilmeniz veya zenginleştirebilmeniz için metni sayfalara böler. |
| [Microsoft.Skills.Text.TranslationSkill](cognitive-search-skill-text-translation.md) | Bu beceri, giriş metnini normalleştirme veya yerelleştirme kullanım örnekleri için çeşitli dillere çevirmek için önceden eğitilmiş bir model kullanır. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Bu beceri, görüntünün içeriğini tanımlamak ve metin açıklaması oluşturmak için bir görüntü algılama algoritması kullanır. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optik karakter tanıma. |
| [Microsoft.Skills.Util.ConditionalSkill](cognitive-search-skill-conditional.md) | Filtreleme, varsayılan değer atama ve bir koşula göre verileri birleştirme sağlar.|
| [Microsoft.Skills.Util.DocumentExtractionSkill](cognitive-search-skill-document-extraction.md) | Zenginleştirme ardışık alanı içindeki bir dosyadan içerik ayıklar. |
| [Microsoft.Skills.Util.ShaperBeceri](cognitive-search-skill-shaper.md) | Çıktıyı karmaşık bir türe eşler (tam ad, çok satırlı adres veya soyadı ve kişisel tanımlayıcının birleşimi için kullanılabilecek çok parçalı veri türü.) |
| [Microsoft.Skills.Custom.WebApiSkill](cognitive-search-custom-skill-web-api.md) | Özel bir Web API'sine HTTP çağrısı yaparak bir AI zenginleştirme boru hattının genişletilebilirliğini sağlar |


Özel bir [beceri](cognitive-search-custom-skill-web-api.md)oluşturma kılavuzu için, [bkz.](cognitive-search-custom-skill-interface.md) [Example: Creating a custom skill for AI enrichment](cognitive-search-create-custom-skill-example.md)

## <a name="see-also"></a>Ayrıca bkz.

+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Özel Beceriler arayüz tanımı](cognitive-search-custom-skill-interface.md)
+ [Öğretici: AI ile zenginleştirilmiş indeksleme](cognitive-search-tutorial-blob.md)
