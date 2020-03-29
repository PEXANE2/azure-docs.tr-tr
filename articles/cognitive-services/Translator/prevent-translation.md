---
title: İçerik çevirisini önleme - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API ile içeriğin çevrilmesini engelleyin. Çevirmen Metin API'si, içeriğin çevrilmemesi için etiketlemenize olanak tanır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: swmachan
ms.openlocfilehash: c7be4a0ea1a9d24a8b262132632a0bbb63ae1b96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80052479"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Çevirmen Metin API ile içerik çevirisi nasıl engellenir?

Çevirmen Metin API'si, içeriğin çevrilmemesi için etiketlemenize olanak tanır. Örneğin, kod, bir marka adı veya yerelleştirildiğinde anlamlı olmayan bir sözcük/tümcecik etiketlemek isteyebilirsiniz.

## <a name="methods-for-preventing-translation"></a>Çevirinin önlenmesi için yöntemler

1. İçeriğinizi ' `notranslate`le etiketle. Bu yalnızca giriş textType HTML olarak ayarlandığında bu çalışır tasarım gereğidir

   Örnek:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

2. Belirli bir çeviri reçete etmek için [dinamik sözlüğü](dynamic-dictionary.md) kullanın.

3. Çeviri için dizeyi Çevirmen Metin API'sine geçirmeyin.

4. Özel Çevirmen: %100 olasılıklı bir ifadenin çevirisini reçete etmek için [Özel Çevirmen'de sözlük](custom-translator/what-is-dictionary.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API aramanızda çeviriden kaçının](reference/v3-0-translate.md)
