---
title: Anahtar sözcük adlandırma yönergeleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Etkin anahtar sözcük oluşturma, cihazınızın sürekli olarak ve doğru şekilde yanıt vermesini sağlamak için önemlidir.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: dapine
ms.openlocfilehash: 3c3943f7269fa2d0ed25acf2215549635b5f16ac
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79219571"
---
# <a name="guidelines-for-creating-an-effective-keyword"></a>Etkin anahtar sözcük oluşturma yönergeleri

Etkin anahtar sözcük oluşturma, cihazınızın sürekli olarak ve doğru şekilde yanıt vermesini sağlamak için önemlidir. Anahtar keliinizi özelleştirmek, cihazınızı ayırt etmenin ve markanızı güçlendirmenin etkili bir yoludur. Bu makalede, etkin anahtar sözcük oluşturmak için bazı temel ilkeler öğrenirsiniz.

## <a name="choose-an-effective-keyword"></a>Etkin bir anahtar sözcük seçin

Bir anahtar sözcük seçerken aşağıdaki yönergeleri göz önünde bulundurun:

* Anahtar kelimelisiniz Ingilizce bir sözcük veya tümcecik olmalıdır.

* Söyleyin iki saniyeden daha uzun sürer.

* 4 ila 7 hece sözcükleri en iyi çalışır. Örneğin, "Hey, Computer" iyi bir anahtar sözcüktür. Yalnızca "Merhaba" zayıf bir paroladır.

* Anahtar sözcükler, yaygın Ingilizce Söyleniş kurallarını izlemelidir.

* Benzersiz bir veya daha yaygın İngilizce telaffuz kurallarını izleyen made-up sözcük hatalı pozitif sonuçları azaltmak. Örneğin, "computerama" iyi bir anahtar sözcük olabilir.

* Ortak bir sözcük seçmeyin. Örneğin, "yemek" ve "kişiler sık sıradan konuşmada söyleyin sözcükler Git". Bunlar, cihazınız için false Tetikleyiciler olabilir.

* Alternatif söylenişleri olabilecek bir anahtar sözcük kullanmaktan kaçının. Kullanıcılar, cihazlarını yanıt almak için "doğru" telaffuz bilmesi gerekir. Örneğin, "509" "beş sıfır dokuz" bildirilebilir "beş oh dokuz," veya "beş yüz ve dokuz." "R.E.I." "r-e-i" veya "ışın" bildirilebilir "Canlı" "/līv/" veya "/liv/" bildirilebilir.

* Özel karakterler, simgeler veya basamak kullanmayın. Örneğin, "Go #" ve "20 + kediler" sorunlu anahtar sözcüklerdir. Ancak, "NET Git" veya "yirmi kediler artı" işe yarayabilir. Yine de, marka simgeleri kullanın ve uygun telaffuz güçlendirmek için pazarlama ve belgeleri kullanın.

> [!NOTE]
> Anahtar kelime olarak bir trademarked sözcüğü seçerseniz, bu ticari marka sahibi olduğunuzdan veya ticari marka sahibinden, kelimeyi kullanmak için izninizin olduğundan emin olun. Microsoft, sizin tercih ettiğiniz anahtar kelimeden çıkabilecek herhangi bir yasal sorun için tabi değildir.

## <a name="next-steps"></a>Sonraki adımlar

[Konuşma Studio kullanarak özel bir anahtar sözcük oluşturmayı](speech-devices-sdk-create-kws.md)öğrenin.
