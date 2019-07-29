---
title: İçerik çevirisini önleme-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Translator Metin Çevirisi API'si içerik çevirisini önleyin.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: f9b48b094713f6ee141c5c15d5636ca965ad61b9
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595230"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Translator Metin Çevirisi API'si içerik çevirisini önleme

Translator Metin Çevirisi API'si, içeriği çevrilemeyecek şekilde etiketleyebilmenizi sağlar. Örneğin, kod, marka adı veya yerelleştirmeden anlamlı olmayan bir sözcük/tümcecik etiketlemek isteyebilirsiniz.

## <a name="methods-for-preventing-translation"></a>Çeviriyi önlemek için Yöntemler
1. Twitter etiketine @somethingtopassthrough veya #somethingtopassthrough kaçış. Çeviri sonrasında kaçış işaretini kaldır.

2. İçeriğinizi ile `notranslate`etiketleyin.

   Örnek:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Belirli bir çeviri yapmak için [dinamik sözlüğü](dynamic-dictionary.md) kullanın.

4. Dizeyi çeviri için Translator Metin Çevirisi API'si iletmeyin.

5. Özel çevirici: % 100 olasılığa sahip bir tümceciğin çevirisini yapmak için [özel çevirmende bir sözlük](custom-translator/what-is-dictionary.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API çağrısındaki çeviriyi önleyin](reference/v3-0-translate.md)
