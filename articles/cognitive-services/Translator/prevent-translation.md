---
title: İçerik çevirisini önleme-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si içerik çevirisini önleyin. Translator Metin Çevirisi API'si, içeriği çevrilemeyecek şekilde etiketleyebilmenizi sağlar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: swmachan
ms.openlocfilehash: 15a36451c18d65df6667f24284f3f69f3d1c06b8
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326768"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Translator Metin Çevirisi API'si içerik çevirisini önleme

Translator Metin Çevirisi API'si, içeriği çevrilemeyecek şekilde etiketleyebilmenizi sağlar. Örneğin, kod, marka adı veya yerelleştirmeden anlamlı olmayan bir sözcük/tümcecik etiketlemek isteyebilirsiniz.

## <a name="methods-for-preventing-translation"></a>Çeviriyi önlemek için Yöntemler
1. Twitter etiketine kaçış @somethingtopassthrough veya #somethingtopassthrough. Çeviri sonrasında kaçış işaretini kaldır. Bu, geçerli Twitter etiketleri için normal ifadedir: `\B@[A-Za-z]+[A-Za-z0-9_]+)`. Bir etiket bir "@" işareti ile başlamalı, ardından bir karakter ve ardından bir veya daha fazla karakter, rakam veya alt çizgi gelmelidir. Etiketlerin kısa tutulması ve açılış etiketinin önünde bir boşluk olması önerilir.

2. İçeriğinizi `notranslate`etiketleyin. Bu, yalnızca Input textType 'ın HTML olarak ayarlandığı durumlarda çalıştığını tasarlayabilmesini sağlamaktır

   Örnek:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Belirli bir çeviri yapmak için [dinamik sözlüğü](dynamic-dictionary.md) kullanın.

4. Dizeyi çeviri için Translator Metin Çevirisi API'si iletmeyin.

5. Özel çevirici: bir tümceciği %100 olasılığa sahip bir ifadenin çevirisi yapmak için [özel çevirmende bir sözlük](custom-translator/what-is-dictionary.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API çağrısındaki çeviriyi önleyin](reference/v3-0-translate.md)
