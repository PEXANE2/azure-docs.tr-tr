---
title: Metin Analizi API'si metin uzaklıkları
titleSuffix: Azure Cognitive Services
description: Çok dilli ve Emoji kodlamalarının neden olduğu uzaklıklar hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 03/09/2020
ms.author: aahi
ms.reviewer: jdesousa
ms.openlocfilehash: 6e404c710a244f06676edf50c3f5c95a7d681e35
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79219241"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>Metin Analizi API'si çıkışındaki metin uzaklıkları

Çok dilli ve Emoji desteği, grapheme olarak adlandırılan tek bir görüntülenen karakteri temsil etmek için birden fazla [kod noktası](https://wikipedia.org/wiki/Code_point) kullanan Unicode kodlamalara yol açmıştır. Örneğin, 🌷 ve 👍 gibi emojıs, şekil kaplama tonu gibi görsel öznitelikler için ek karakterlerle, şekli oluşturmak için birkaç karakteri kullanabilir. Benzer şekilde, Hintçe Word `अनुच्छेद` beş harf ve üç Birleşik işaret olarak kodlanır.

Olası çok dilli ve Emoji kodlamaları farklı uzunluklarıyla, Metin Analizi API'si yanıttaki uzaklıkları döndürebilir.

## <a name="offsets-in-the-api-response"></a>API yanıtında uzaklıklar. 

Her kaydırın, [adlandırılmış varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md) veya [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)gibi API yanıtı döndürüldüğünde, aşağıdakileri unutmayın:

* Yanıttaki öğeler, çağrılan uç noktaya özgü olabilir. 
* HTTP POST/GET yükleri [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp)olarak kodlanır, bu, istemci tarafı derleyicinizdeki veya işletim sisteminizde varsayılan karakter kodlaması olabilir veya olmayabilir.
* Uzaklıklar, karakter sayımlarını değil [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0) standardına göre grafem sayılarına başvurur.

## <a name="extracting-substrings-from-text-with-offsets"></a>Uzaklıkları olan metinden alt dizeleri ayıklama

Uzaklıklar, .NET [substring ()](https://docs.microsoft.com/dotnet/api/system.string.substring?view=netframework-4.8) yöntemi gibi karakter tabanlı alt dize yöntemleri kullanılırken sorunlara yol açabilir. Bir sorun, bir kaydırın bir alt dize yönteminin, son yerine çok karakterli grafem kodlamasının ortasında sonlanmasına neden olabilir.

.NET ' te, dize, tek tek karakter nesneleri yerine bir dizi metin öğesi olarak çalışmanıza olanak sağlayan [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) sınıfını kullanmayı düşünün. Ayrıca, tercih ettiğiniz yazılım ortamınızda grafem Bölümlendirici kitaplıklarını da arayabilirsiniz. 

Metin Analizi API'si kolaylık sağlamak için bu metinsel öğeleri de döndürür.

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
* [Dili algıla](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Dil tanıma](../how-tos/text-analytics-how-to-language-detection.md)
