---
title: AI zenginleştirme için dokümantasyon bağlantıları
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da AI zenginleştirme iş yüklerine ilişkin makalelerin, öğreticilerin, örneklerin ve blog gönderilerinin açıklamalı listesi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: d2b25fb93a1e35ffa82cf49c60d181b841b1692d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616188"
---
# <a name="documentation-resources-for-ai-enrichment-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da AI zenginleştirme için dokümantasyon kaynakları

AI zenginleştirme, Azure Bilişsel Arama dizinoluşturma yeteneğidir ve metin dışı kaynaklarda ve farklılaşmamış metinde gizli bilgileri bularak Azure Bilişsel Arama'da tam metin aranabilir içeriğe dönüştürür.

Aşağıdaki makaleler AI zenginleştirme için tam belgelerdir.

## <a name="getting-started"></a>Başlarken
+ [Azure Bilişsel Arama'da AI'ye Giriş](cognitive-search-concept-intro.md)
+ [Hızlı başlangıç: Azure portalında bilişsel beceriler oluşturun](cognitive-search-quickstart-blob.md)
+ [Öğretici: AI ile zenginleştirilmiş indeksleme](cognitive-search-tutorial-blob.md)
+ [Örnek: AI zenginleştirme için özel bir beceri oluşturma](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Nasıl kılavuz
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Beceri kümesindeki ek açıklamalara başvurma](cognitive-search-concept-annotations-syntax.md)
+ [Alanları dizinle nasıl eşlersin?](cognitive-search-output-field-mapping.md)
+ [Görüntülerden bilgi işleme ve ayıklama](cognitive-search-concept-image-scenarios.md)
+ [Azure Bilişsel Arama dizinini yeniden oluşturma](search-howto-reindex.md)
+ [Özel beceri arabirimi nasıl tanımlanır?](cognitive-search-custom-skill-interface.md)
+ [Sorun giderme ipuçları](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Başvuru

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.EntityTanıma Beceri](cognitive-search-skill-entity-recognition.md)
  + [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.PIIDetectionSkill](cognitive-search-skill-pii-detection.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Text.TranslationSkill](cognitive-search-skill-text-translation.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ConditionalSkill](cognitive-search-skill-conditional.md)
  + [Microsoft.Skills.Util.DocumentExtractionSkill](cognitive-search-skill-document-extraction.md)
  + [Microsoft.Skills.Util.ShaperBeceri](cognitive-search-skill-shaper.md)

+ Özel beceriler
  + [Microsoft.Skills.Custom.WebApiSkill](cognitive-search-custom-skill-web-api.md)

+ [Kullanım dışı bırakılmış beceriler](cognitive-search-skill-deprecated.md)
  + [Microsoft.Skills.Text.NamedEntityTanıma Beceri](cognitive-search-skill-named-entity-recognition.md)

+ [REST API](https://docs.microsoft.com/rest/api/searchservice/)
  + [Skillset oluştur (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [Indexer Oluştur (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/)
+ [Azure Bilişsel Arama'daki Dizin Oluşturucular](search-indexer-overview.md)
+ [Azure Bilişsel Arama nedir?](search-what-is-azure-search.md)
