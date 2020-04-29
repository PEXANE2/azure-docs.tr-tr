---
title: Anahtar sözcük adlandırma yönergeleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Etkin anahtar sözcük oluşturma, cihazınızın sürekli olarak ve doğru şekilde yanıt vermesini sağlamak için önemlidir.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: c00d27e5e7b7a8d614ce94ea4024b6093669757c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81399837"
---
# <a name="guidelines-for-creating-an-effective-keyword"></a>Etkin anahtar sözcük oluşturma yönergeleri

Etkin anahtar sözcük oluşturma, cihazınızın sürekli olarak ve doğru şekilde yanıt vermesini sağlamak için önemlidir. Anahtar keliinizi özelleştirmek, cihazınızı ayırt etmenin ve markanızı güçlendirmenin etkili bir yoludur. Bu makalede, etkin anahtar sözcük oluşturmak için bazı temel ilkeler öğrenirsiniz.

## <a name="choose-an-effective-keyword"></a>Etkin bir anahtar sözcük seçin

Bir anahtar sözcük seçerken aşağıdaki yönergeleri göz önünde bulundurun:

> [!div class="checklist"]
> * Anahtar kelimelisiniz Ingilizce bir sözcük veya tümcecik olmalıdır.
> * İki saniyeden daha uzun sürmemelidir.
> * 4 ile 7 hece hecelere en iyi şekilde çalışan sözcükler. Örneğin, "Hey, Computer" iyi bir anahtar sözcüktür. Yalnızca "Hey" kötü bir alışkanlıktır.
> * Anahtar sözcükler, yaygın Ingilizce Söyleniş kurallarını izlemelidir.
> * Yaygın Ingilizce telaffuz kuralları takip eden, benzersiz veya hatta oluşturulmuş bir sözcük, hatalı pozitif sonuçları düşürebilir. Örneğin, "computerama" iyi bir anahtar sözcük olabilir.
> * Ortak bir sözcük seçemezsiniz. Örneğin, "yemek" ve "Go", insanların normal konuşmada sıkça söyledikleri sözcüklerdir. Cihazınız için yanlış Tetikleyiciler olabilir.
> * Alternatif söylenişleri olabilecek bir anahtar sözcük kullanmaktan kaçının. Kullanıcıların, cihazlarını yanıt vermesini sağlamak için "sağ" söylenişini bilmeleri gerekir. Örneğin, "509", "5 0 9", "5 0 9" veya "509" olarak kullanılabilir. "R.E.I." "r-e-i" veya "Ray" olarak kullanılabilir. "Canlı", "/līv/" veya "/Liv/" ile yapılabilir.
> * Özel karakterler, semboller veya basamaklar kullanmayın. Örneğin, "Go #" ve "20 + kediler" sorunlu anahtar sözcüklerdir. Bununla birlikte, "keskin" veya "yirmi Plus kediler" işe devam edebilir. Markanızdaki sembolleri kullanmaya devam edebilir ve uygun telaffuz zorlamak için pazarlama ve belgeleri kullanabilirsiniz.

> [!NOTE]
> Anahtar kelime olarak bir trademarked sözcüğü seçerseniz, bu ticari marka sahibi olduğunuzdan veya ticari marka sahibinden, kelimeyi kullanmak için izninizin olduğundan emin olun. Microsoft, sizin tercih ettiğiniz anahtar kelimeden çıkabilecek herhangi bir yasal sorun için tabi değildir.

## <a name="next-steps"></a>Sonraki adımlar

[Konuşma Studio kullanarak özel bir anahtar sözcük oluşturmayı](speech-devices-sdk-create-kws.md)öğrenin.
