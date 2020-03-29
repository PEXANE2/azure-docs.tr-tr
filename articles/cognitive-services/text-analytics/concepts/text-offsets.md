---
title: Metin Analizi API'sinde metin uzaklıkları
titleSuffix: Azure Cognitive Services
description: Çok dilli ve emoji kodlamalarının neden olduğu uzaklıklar hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219241"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>Metin Analizi API çıkışında metin uzaklıkları

Çok dilli ve emoji desteği, grafeme adı verilen tek bir görüntülenen karakteri temsil etmek için birden fazla [kod noktası](https://wikipedia.org/wiki/Code_point) kullanan Unicode kodlamalarına yol açmıştır. Örneğin, 🌷 gibi emojiler ve 👍 cilt tonu gibi görsel öznitelikler için ek karakterler ile şekil oluşturmak için birkaç karakter kullanabilirsiniz. Benzer şekilde, Hintçe kelime `अनुच्छेद` beş harf ve üç birleştirme işaretleri olarak kodlanır.

Olası çok dilli ve emoji kodlamalarının farklı uzunlukları nedeniyle, Text Analytics API yanıtta uzaklıkları döndürebilir.

## <a name="offsets-in-the-api-response"></a>API yanıtında uzaklıklar. 

Uzaklıklar, [Adlandırılmış Varlık Tanıma](../how-tos/text-analytics-how-to-entity-linking.md) veya [Duyarlılık Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)gibi API yanıtı döndürüldüğünde aşağıdakileri hatırlayın:

* Yanıttaki öğeler çağrılan bitiş noktasına özgü olabilir. 
* HTTP POST/GET yükleri [UTF-8'de](https://www.w3schools.com/charsets/ref_html_utf8.asp)kodlanır ve istemci tarafı derleyicinizde veya işletim sisteminizde varsayılan karakter kodlaması olabilir veya olmayabilir.
* Uzaklıklar, karakter sayılarını değil, [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0) standardına dayalı grafim sayılarını ifade eder.

## <a name="extracting-substrings-from-text-with-offsets"></a>Uzaklıklı metinden alt dizeleri ayıklama

Uzaklıklar, karakter tabanlı substring yöntemlerini kullanırken sorunlara neden olabilir, örneğin .NET [substring()](https://docs.microsoft.com/dotnet/api/system.string.substring?view=netframework-4.8) yöntemi. Bir sorun, bir ofset sonu yerine çok karakterli grafme kodlama ortasında sona bir substring yöntemi neden olabilir.

.NET'te, tek tek karakter nesneleri yerine bir dizi metin öğesi olarak bir dize yle çalışmanızı sağlayan [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) sınıfını kullanmayı düşünün. Tercih ettiğiniz yazılım ortamında grafeme ayırıcı kitaplıklarını da inceleyebilirsiniz. 

Text Analytics API kolaylık sağlamak için bu metin öğelerini de döndürür.

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
* [Dili algılama](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Dil tanıma](../how-tos/text-analytics-how-to-language-detection.md)
