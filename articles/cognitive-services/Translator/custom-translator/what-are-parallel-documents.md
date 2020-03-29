---
title: Paralel belgeler nelerdir? - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Paralel belgeler, birinin diğerinin çevirisi olduğu belge çiftleridir. Çiftteki bir belge kaynak dilde cümleler, diğer belge ise hedef dile çevrilmiş bu cümleleri içerir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: d7c38a44e3111a319e4146b3c9b71a22b0d31bfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72675476"
---
# <a name="what-are-parallel-documents"></a>Paralel belgeler nelerdir?

Paralel belgeler, birinin diğerinin çevirisi olduğu belge çiftleridir. Çiftteki bir belge kaynak dilde cümleler, diğer belge ise hedef dile çevrilmiş bu cümleleri içerir.
Hangi dilin "kaynak" olarak işaretlendiği ve hangi dilin "hedef" olarak işaretlendiği önemli değildir – paralel bir belge her iki yönde de bir çeviri sistemini eğitmek için kullanılabilir.

## <a name="requirements"></a>Gereksinimler

Bir sistemi eğitmek için en az 10.000 benzersiz hizalanmış paralel cümleye ihtiyacınız olacaktır. Bu sınırlama, paralel cümlelerinizin bir çeviri modelini başarılı bir şekilde eğitecek kadar benzersiz kelime bilgisi içerdiğinden emin olmak için bir güvenlik ağıdır. En iyi uygulama olarak, çeviri sisteminizin kalitesini artırmak için sürekli olarak daha fazla paralel içerik ekleyin ve yeniden eğitin. Lütfen [Cümle Hizalama'ya](https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/sentence-alignment)bakın.

Microsoft, Özel Çevirmen'e yüklenen belgelerin üçüncü bir tarafın telif hakkını veya fikri özelliklerini ihlal etmediğini şart başvurur. Daha fazla bilgi için lütfen [Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/cognitive-services-terms/)bakın.
Portalı kullanarak belge yüklemek, belgedeki fikri mülkiyetin mülkiyetini değiştirmez.

## <a name="use-of-parallel-documents"></a>Paralel belgelerin kullanımı

Paralel belgeler sistem tarafından kullanılır:

1.  Sözcüklerin, tümceciklerin ve cümlelerin genellikle iki dil arasında nasıl eşlendirilmelerini öğrenmek için.

2.  Çevredeki tümceciklere bağlı olarak uygun bağlamı nasıl işlenir öğrenmeyi. Bir sözcük her zaman diğer dilde aynı kelimeye çevrilmeyebilir.

En iyi uygulama olarak, belgelerin kaynak ve hedef dil sürümleri arasında 1:1 cümle yazışması olduğundan emin olun.

Projeniz etki alanına (kategoriye) özelse, belgeleriniz bu kategorideki terminolojide tutarlı olmalıdır. Ortaya çıkan çeviri sisteminin kalitesi, belge setinizdeki cümle sayısına ve cümlelerin kalitesine bağlıdır. Kategorinize özgü bir sözcük için belgelerinizin farklı kullanımlarla birlikte ne kadar çok örnek içerdiğine, sistemin çeviri sırasında yapabileceği o kadar iyi bir iş olabilir.

Yüklenen belgeler her çalışma alanına özeldir ve istediğiniz kadar proje veya eğitimde kullanılabilir. Belgelerinizden çıkarılan cümleler, düz Unicode metin dosyaları olarak deponuzda ayrı olarak saklanır ve silmeniz için kullanılabilir. Özel Çevirmen'i belge deposu olarak kullanmayın, yüklediğiniz belgeleri yüklediğiniz biçimde indiremeyeceksiniz.



## <a name="next-steps"></a>Sonraki adımlar

- Özel Çevirmen'de [sözlük](what-is-dictionary.md) kullanmayı öğrenin.
