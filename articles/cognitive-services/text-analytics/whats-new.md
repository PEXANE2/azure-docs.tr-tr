---
title: Metin Analizi API'si yenilikler
titlesuffix: Text Analytics - Azure Cognitive Services
description: Metin Analizi hizmetiyle ilgili yeni gelişmeleri öğrenin
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: aahi
ms.openlocfilehash: 44ef6fb118be4d1110a693faded6c57bc8b4e2fd
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499952"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Metin Analizi API’sindeki yenilikler

Metin Analizi API'si, sürekli olarak güncelleştirilir. Son geliştirmelerin güncel kalması için, bu makalede yeni yayınlar ve özellikler hakkında bilgi verilmektedir.

## <a name="named-entity-recognition-v3-public-preview---october-2019"></a>Adlandırılmış varlık tanıma v3 genel önizlemesi-Ekim 2019

Adlandırılmış varlık tanıma 'nın (NER) sonraki sürümü artık genel önizlemeye sunuldu ve metinde bulunan varlıkların genişletilmiş algılanması ve kategorilerini sağlar. Şu olanakları sunar:

* Aşağıdaki yeni varlık türlerinin tanınması:
    * Telefon numarası
    * IP adresi

* Kişisel bilgi varlık türlerini (Yalnızca Ingilizce) tanıma için yeni bir uç nokta
* Varlık tanıma ve varlık bağlama için ayrı uç noktalar.

Varlık bağlama, Ingilizce ve Ispanyolca 'yi destekler. NER dil desteği varlık türüne göre farklılık gösterir. Daha fazla bilgi için aşağıdaki bağlantıya bakın. 

> [!div class="nextstepaction"]
> [Adlandırılmış varlık tanıma v3 hakkında daha fazla bilgi edinin](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-v3-public-preview)

## <a name="sentiment-analysis-v3-public-preview---october-2019"></a>Yaklaşım Analizi v3 genel önizleme-Ekim 2019

Yaklaşım Analizi sonraki sürümü artık genel önizlemeye sunuldu ve API 'nin metin kategorisi ve puanlaması için doğruluk ve ayrıntı açısından önemli geliştirmeler sağlar. Ayrıca şunları sağlar:

* Metindeki farklı sentiler için otomatik etiketleme.
* Bir belge ve tümce düzeyinde yaklaşım Analizi ve çıktısı. 

Ingilizce (`en`), Japonca (`ja`), Çince Basitleştirilmiş (`zh-Hans`), geleneksel Çince (`zh-Hant`), Fransızca (`fr`), Italyanca (`it`), Ispanyolca (`es`), Felemenkçe (`nl`), Portekizce (`pt`) ve Almanca (`de`) destekler. ve şu bölgelerde kullanılabilir: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe`ve `West US 2`.

> [!div class="nextstepaction"]
> [Yaklaşım Analizi v3 hakkında daha fazla bilgi edinin](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-v3-public-preview)

## <a name="next-steps"></a>Sonraki Adımlar

* [Metin Analizi API'si nedir?](overview.md)  
* [Örnek kullanıcı senaryoları](text-analytics-user-scenarios.md)
* [Yaklaşım Analizi](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Dil algılama](how-tos/text-analytics-how-to-language-detection.md)
* [Varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md)
* [Anahtar tümceciği ayıklama](how-tos/text-analytics-how-to-keyword-extraction.md)
