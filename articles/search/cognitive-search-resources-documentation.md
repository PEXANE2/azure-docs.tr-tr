---
title: AI zenginleştirme için belge bağlantıları
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de AI zenginleştirme iş yükleri ile ilgili makalelerin, öğreticilerin, örneklerin ve blog gönderilerinin açıklamalı bir listesi.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/12/2020
ms.openlocfilehash: 6199b2a322bc22f85d6fb8b422ea49c5f35f080e
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84753001"
---
# <a name="documentation-resources-for-ai-enrichment-in-azure-cognitive-search"></a>Azure Bilişsel Arama AI zenginleştirme için belge kaynakları

AI zenginleştirme, Dizin Oluşturucu tabanlı dizin oluşturma işleminin, metin olmayan kaynaklardaki ve ayırt edilmemiş metinlerin, Azure Bilişsel Arama 'de tam metin aranabilir içeriğe dönüştüren bir eklentisi. 

Yerleşik işleme için bilişsel hizmetler 'deki önceden eğitilen AI modelleri analizler gerçekleştirmek için dahili olarak çağırılır. Ayrıca, Azure Machine Learning, Azure Işlevleri veya diğer yaklaşımlar kullanarak özel modelleri tümleştirebilirsiniz.

Aşağıda AI zenginleştirme belgelerinin birleştirilmiş bir listesi verilmiştir.

## <a name="concepts"></a>Kavramlar

+ [AI zenginleştirme](cognitive-search-concept-intro.md)
+ [Beceri kümeleri](cognitive-search-working-with-skillsets.md)
+ [Hata ayıklama oturumları](cognitive-search-debug-session.md)
+ [Bilgi depoları](knowledge-store-concept-intro.md)
+ [Yansıtmalar](knowledge-store-projection-overview.md)
+ [Artımlı zenginleştirme (önbelleğe alınmış zenginleştirilmiş belgeyi yeniden kullanma)](cognitive-search-incremental-indexing-conceptual.md)

## <a name="hands-on-walkthroughs"></a>Uygulamalı izlenecek yollar

+ [Hızlı başlangıç: Azure portal Bilişsel Beceri oluşturma](cognitive-search-quickstart-blob.md)
+ [Öğretici: AI ile zenginleştirilmiş dizin oluşturma](cognitive-search-tutorial-blob.md)
+ [Öğretici: hata ayıklama oturumlarıyla beceri 'de yapılan değişiklikleri Tanıla, Onar ve Kaydet](cognitive-search-tutorial-debug-sessions.md)

## <a name="knowledge-stores"></a>Bilgi depoları

+ [Hızlı başlangıç: Azure portal bilgi deposu oluşturma](knowledge-store-create-portal.md)
+ [REST ve Postman kullanarak bilgi deposu oluşturma](knowledge-store-create-rest.md)
+ [Depolama Gezgini bir bilgi deposu görüntüleme](knowledge-store-view-storage-explorer.md)
+ [Bilgi deposunu Power BI bağlama](knowledge-store-connect-power-bi.md)
+ [Projeksiyon örnekleri (zenginleştirme ve dışarı aktarma)](knowledge-store-projections-examples.md)

## <a name="custom-skills-advanced"></a>Özel yetenekler (Gelişmiş)

+ [Özel yetenekler arabirimi tanımlama](cognitive-search-custom-skill-interface.md)
+ [Örnek: Azure Işlevleri (ve Bing Varlık Arama API 'Leri) kullanarak özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
+ [Örnek: Python kullanarak özel bir yetenek oluşturma](cognitive-search-custom-skill-python.md)
+ [Örnek: form tanıyıcı kullanarak özel bir yetenek oluşturma](cognitive-search-custom-skill-form.md) 
+ [Örnek: Azure Machine Learning kullanarak özel bir yetenek oluşturma](cognitive-search-tutorial-aml-custom-skill.md) 

## <a name="how-to-guidance"></a>Nasıl yapılır Kılavuzu

+ [Bilişsel Hizmetler kaynağı iliştirme](cognitive-search-attach-cognitive-services.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [bir beceri başvuru ek açıklamaları](cognitive-search-concept-annotations-syntax.md)
+ [Alanları bir dizine eşleyin](cognitive-search-output-field-mapping.md)
+ [Görüntülerdeki bilgileri işleme ve ayıklama](cognitive-search-concept-image-scenarios.md)
+ [Artımlı zenginleştirme için önbelleğe almayı yapılandırın](search-howto-incremental-index.md)
+ [Azure Bilişsel Arama dizinini yeniden oluşturma](search-howto-reindex.md)
+ [Tasarım ipuçları](cognitive-search-concept-troubleshooting.md)
+ [Sık karşılaşılan hatalar ve uyarılar](cognitive-search-common-errors-warnings.md)

## <a name="skills-reference"></a>Beceriler başvurusu

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
  + [Microsoft. yetenekler. Text. KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft. yetenekler. Text. LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft. yetenekler. Text. Entityrecognitionbeceri](cognitive-search-skill-entity-recognition.md)
  + [Microsoft. yetenekler. Text. Mergeskıll](cognitive-search-skill-textmerger.md)
  + [Microsoft. yetenekler. Text. PIIDetectionSkill](cognitive-search-skill-pii-detection.md)
  + [Microsoft. yetenekler. Text. Splitbeceri](cognitive-search-skill-textsplit.md)
  + [Microsoft. yetenekler. Text. Sentimentbecerisi](cognitive-search-skill-sentiment.md)
  + [Microsoft. yetenekler. Text. Translationbeceri](cognitive-search-skill-text-translation.md)
  + [Microsoft. yetenekler. Vision. ımageanalysisbeceri](cognitive-search-skill-image-analysis.md)
  + [Microsoft. yetenekler. Vision. Ocrbeceri](cognitive-search-skill-ocr.md)
  + [Microsoft. yetenekler. util. Conditionalbeceri](cognitive-search-skill-conditional.md)
  + [Microsoft.Skills.Util.DocUmentextractionbeceri](cognitive-search-skill-document-extraction.md)
  + [Microsoft. yetenekler. util. Shaperbeceri](cognitive-search-skill-shaper.md)

+ Özel beceriler
  + [Microsoft. yetenekler. Custom. Amlbeceri](cognitive-search-aml-skill.md)
  + [Microsoft. yetenekler. Custom. WebApiSkill](cognitive-search-custom-skill-web-api.md)

+ [Kullanım dışı bırakılmış beceriler](cognitive-search-skill-deprecated.md)
  + [Microsoft. yetenekler. Text. Namedentityrecognitionbeceri](cognitive-search-skill-named-entity-recognition.md)

## <a name="apis"></a>API'ler

+ [REST API](https://docs.microsoft.com/rest/api/searchservice/)
  + [Beceri oluşturma (api-Version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [Dizin Oluşturucu oluştur (api-Version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/)
+ [Azure Bilişsel Arama'daki Dizin Oluşturucular](search-indexer-overview.md)
+ [Azure Bilişsel Arama nedir?](search-what-is-azure-search.md)
