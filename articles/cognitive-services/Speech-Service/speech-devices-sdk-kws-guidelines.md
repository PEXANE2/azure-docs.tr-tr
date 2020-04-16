---
title: Anahtar kelime adlandırma yönergeleri - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Etkili bir anahtar kelime oluşturmak, cihazınızın tutarlı ve doğru bir şekilde yanıt vermesi için hayati önem taşır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: c00d27e5e7b7a8d614ce94ea4024b6093669757c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399837"
---
# <a name="guidelines-for-creating-an-effective-keyword"></a>Etkili bir anahtar kelime oluşturma yönergeleri

Etkili bir anahtar kelime oluşturmak, cihazınızın tutarlı ve doğru bir şekilde yanıt vermesi için hayati önem taşır. Anahtar kelimenizi özelleştirmek, cihazınızı farklılaştırmanın ve markanızı güçlendirmenin etkili bir yoludur. Bu makalede, etkili bir anahtar kelime oluşturmak için bazı yol gösterici ilkeler öğrenirsiniz.

## <a name="choose-an-effective-keyword"></a>Etkili bir anahtar kelime seçin

Bir anahtar kelime seçerken aşağıdaki yönergeleri göz önünde bulundurun:

> [!div class="checklist"]
> * Anahtar kelimeniz İngilizce bir sözcük veya tümcecik olmalıdır.
> * Bunu söylemek iki saniyeden fazla sürmez.
> * 4 ile 7 hecekelimeleri en iyi şekilde çalışır. Örneğin, "Hey, Bilgisayar" iyi bir anahtar kelimedir. Sadece "Hey" zavallı bir tane.
> * Anahtar kelimeler yaygın İngilizce telaffuz kurallarına uymalıdır.
> * Ortak İngilizce telaffuz kurallarına uyan benzersiz veya hatta uydurma bir sözcük yanlış pozitifliği azaltabilir. Örneğin, "computerama" iyi bir anahtar kelime olabilir.
> * Ortak bir sözcük seçmeyin. Örneğin, "yemek" ve "gitmek" insanların sıradan konuşmasık sık söylediği kelimelerdir. Bunlar cihazınız için yanlış tetikleyiciler olabilir.
> * Alternatif telaffuzları olabilecek bir anahtar kelime kullanmaktan kaçının. Kullanıcıların cihazlarının yanıt vermesi için "doğru" telaffuzunu bilmeleri gerekir. Örneğin, "509" "beş sıfır dokuz", "beş oh dokuz" veya "beş yüz dokuz" olarak telaffuz edilebilir. "R.E.I." "r-e-i" veya "Ray" olarak telaffuz edilebilir. "Live" "/līv/" veya "/liv/" olarak telaffuz edilebilir.
> * Özel karakterler, semboller veya basamaklar kullanmayın. Örneğin, "Go#" ve "20 + cats" sorunlu anahtar kelimeler olabilir. Ancak, "keskin gitmek" veya "yirmi artı kedi" işe yarayabilir. Markanızdaki sembolleri kullanmaya devam edebilir ve uygun telaffuzu güçlendirmek için pazarlama ve belgeleri kullanabilirsiniz.

> [!NOTE]
> Anahtar kelimeniz olarak ticari markalı bir sözcük seçerseniz, söz sahibinin sahibi olduğunuzdan veya ticari marka sahibinden bu kelimeyi kullanmak için izin aldığınızdan emin olun. Microsoft, seçtiğiniz anahtar kelimeden kaynaklanabilir herhangi bir yasal sorundan sorumlu değildir.

## <a name="next-steps"></a>Sonraki adımlar

[Speech Studio'u kullanarak özel bir anahtar kelimeoluşturmayı](speech-devices-sdk-create-kws.md)öğrenin.
