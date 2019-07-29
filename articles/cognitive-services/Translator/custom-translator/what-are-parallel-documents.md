---
title: Paralel belgeler nelerdir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Paralel belgeler, birinin diğerinin çevirisi olduğu belge çiftleridir. Çiftteki bir belge kaynak dilde cümleler içeriyor ve diğer belge, hedef dile çevrilmiş olan bu cümleleri içeriyor.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: fb54df2e1eb89d30e62ae80355635356343994ee
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595452"
---
# <a name="what-are-parallel-documents"></a>Paralel belgeler nelerdir?

Paralel belgeler, birinin diğerinin çevirisi olduğu belge çiftleridir. Çiftteki bir belge kaynak dilde cümleler içeriyor ve diğer belge, hedef dile çevrilmiş olan bu cümleleri içeriyor.
Hangi dilin "kaynak" olarak işaretlendiğinden ve hangi dilin "hedef" olarak işaretlendiğinden bağımsız olarak, bir paralel belge, bir çeviri sisteminin her iki yönde de eğilerek kullanılabilir.

## <a name="requirements"></a>Gereksinimler

Bir sistemi eğitebilmeniz için en az 10.000 benzersiz paralel cümle gerekir. En iyi uygulama olarak, çeviri sisteminizin kalitesini artırmak için sürekli olarak daha fazla paralel içerik ve yeniden eğitme ekleyebilirsiniz.

Microsoft, özel çevirmene yüklenen belgelerin üçüncü tarafın telif hakkı veya fikri mülkiyet özelliklerini ihlal etmesidir. Daha fazla bilgi için lütfen [kullanım koşullarına](https://azure.microsoft.com/support/legal/cognitive-services-terms/)bakın.
Portalı kullanarak bir belgeyi karşıya yüklemek, belgede fikri mülkiyet özelliğinin sahipliğini değiştirmez.

## <a name="use-of-parallel-documents"></a>Paralel belge kullanımı

Paralel belgeler sistem tarafından kullanılır:

1.  Sözcüklerin, deyimlerin ve Tümcelerin iki dil arasında yaygın olarak nasıl eşlenildiğini öğrenmek için.

2.  Çevreleyen tümceciklere bağlı olarak uygun bağlamı nasıl işleyeceğini öğrenmek için. Bir sözcük her zaman diğer dildeki aynı sözcüğe çevrilemeyebilir.

En iyi uygulama olarak, belgelerin kaynak ve hedef dil sürümleri arasında bir 1:1 cümlesi bulunduğundan emin olun.

Projeniz, etki alanına (kategori) özel ise, belgeleriniz söz konusu kategori içindeki terminoloji bakımından tutarlı olmalıdır. Elde edilen çeviri sisteminin kalitesi, belge kümesi içindeki tümce sayısına ve Tümcelerin kalitesine bağlıdır. Belgelerinize özgü bir sözcüğe ait bir sözcük için farklı kullanımlar içeren belgeleriniz daha fazla örnek, çeviri sırasında sistemin daha iyi bir şekilde yapabildiği iş.

Karşıya yüklenen belgeler her çalışma alanı için özeldir ve istediğiniz sayıda proje veya seyahat halinde kullanılabilir. Belgelerinizden ayıklanan cümleler, deponuzda düz Unicode metin dosyaları olarak ayrı olarak depolanır ve silmeniz için kullanılabilir. Özel çeviriciyi bir belge deposu olarak kullanmayın, karşıya yüklediğiniz belgeleri karşıya yüklediğiniz biçimde indiremeyeceksiniz.



## <a name="next-steps"></a>Sonraki adımlar

- Özel çevirmende bir [sözlüğü](what-is-dictionary.md) nasıl kullanacağınızı öğrenin.
