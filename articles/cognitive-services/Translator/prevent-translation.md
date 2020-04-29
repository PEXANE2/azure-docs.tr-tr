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
ms.date: 03/20/2020
ms.author: swmachan
ms.openlocfilehash: c7be4a0ea1a9d24a8b262132632a0bbb63ae1b96
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80052479"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Translator Metin Çevirisi API'si içerik çevirisini önleme

Translator Metin Çevirisi API'si, içeriği çevrilemeyecek şekilde etiketleyebilmenizi sağlar. Örneğin, kod, marka adı veya yerelleştirmeden anlamlı olmayan bir sözcük/tümcecik etiketlemek isteyebilirsiniz.

## <a name="methods-for-preventing-translation"></a>Çeviriyi önlemek için Yöntemler

1. İçeriğinizi ile `notranslate`etiketleyin. Bu, yalnızca Input textType 'ın HTML olarak ayarlandığı durumlarda çalıştığını tasarlayabilmesini sağlamaktır

   Örnek:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

2. Belirli bir çeviri yapmak için [dinamik sözlüğü](dynamic-dictionary.md) kullanın.

3. Dizeyi çeviri için Translator Metin Çevirisi API'si iletmeyin.

4. Özel çevirici: bir tümceciği %100 olasılığa sahip bir ifadenin çevirisi yapmak için [özel çevirmende bir sözlük](custom-translator/what-is-dictionary.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API çağrısındaki çeviriyi önleyin](reference/v3-0-translate.md)
