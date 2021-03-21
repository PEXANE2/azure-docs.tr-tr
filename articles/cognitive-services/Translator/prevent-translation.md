---
title: İçerik çevirisini engelleme-çevirmen
titleSuffix: Azure Cognitive Services
description: Çevirmenle içerik çevirisini önleyin. Çevirmen, içeriği çevrilmeyecek şekilde etiketleyebilmenizi sağlar.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: e89ff3f4c1f54d9e137d3dd51e325b908c826b03
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98897485"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator"></a>Çevirmenle içerik çevirisini önleme

Çevirmen, içeriği çevrilmeyecek şekilde etiketleyebilmenizi sağlar. Örneğin kodları, marka adını veya yerelleştirildiğinde bir anlam ifade etmeyen bir sözcüğü/tümceciği etiketlemek isteyebilirsiniz.

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

2. İçeriğinizi ile etiketleyin `translate="no"` . Bu yalnızca Input textType, HTML olarak ayarlandığında geçerlidir

   Örnek:

   ```html
   <span translate="no">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div translate="no">This will not be translated.</div>
   <div>This will be translated. </div>
   ```
   
3. Belirli bir çeviri yapmak için [dinamik sözlüğü](dynamic-dictionary.md) kullanın.

4. Dizeyi çeviri için çevirmene iletmeyin.

5. Özel çevirici: bir tümceciği %100 olasılığa sahip bir ifadenin çevirisi yapmak için [özel çevirmende bir sözlük](custom-translator/what-is-dictionary.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Metni çevirmek için Çevir işlemini kullanma](reference/v3-0-translate.md)
