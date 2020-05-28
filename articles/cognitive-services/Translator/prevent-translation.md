---
title: İçerik çevirisini engelleme-çevirmen
titleSuffix: Azure Cognitive Services
description: Çevirmenle içerik çevirisini önleyin. Çevirmen, içeriği çevrilmeyecek şekilde etiketleyebilmenizi sağlar.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 37d2e4c0131569ab50ebf49ff73b6adf7a420713
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996185"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator"></a>Çevirmenle içerik çevirisini önleme

Çevirmen, içeriği çevrilmeyecek şekilde etiketleyebilmenizi sağlar. Örneğin, kod, marka adı veya yerelleştirmeden anlamlı olmayan bir sözcük/tümcecik etiketlemek isteyebilirsiniz.

## <a name="methods-for-preventing-translation"></a>Çeviriyi önlemek için Yöntemler

1. İçeriğinizi ile etiketleyin `notranslate` . Bu, yalnızca Input textType 'ın HTML olarak ayarlandığı durumlarda çalıştığını tasarlayabilmesini sağlamaktır

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

3. Dizeyi çeviri için çevirmene iletmeyin.

4. Özel çevirici: bir tümceciği %100 olasılığa sahip bir ifadenin çevirisi yapmak için [özel çevirmende bir sözlük](custom-translator/what-is-dictionary.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Çevirici çağrın çevirisinde kaçının](reference/v3-0-translate.md)
