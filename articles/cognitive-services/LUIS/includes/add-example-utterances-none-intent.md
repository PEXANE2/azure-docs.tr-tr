---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.author: diberry
ms.openlocfilehash: 956aa308bf1cb3736c491031239661ec6b295ddb
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77279653"
---
İstemci uygulamanın, uygulamanın anlamlı veya uygulama için uygun olup olmadığını bilmeleri gerekir. Oluşturma işleminin bir parçası olarak, istemci uygulama tarafından yanıtlanmaması gerekip gerekmediğini tespit etmek için **hiçbiri** amacı her uygulamaya eklenir.

LUSıS, utterance için **hiçbiri** amacını döndürürse, istemci uygulamanız kullanıcının konuşmayı sonlandırmasını isteyip istemediğini veya konuşmaya devam etmek için daha fazla yol vermesini ister.

**Hiçbiri** hedefini boş bırakırsanız, konu etki alanı dışında tahmin edilecek bir söylenişi, mevcut konu etki alanı amaçlarınında tahmin edilir. Sonuç olarak, bir sohbet bot gibi istemci uygulamasının yanlış bir tahmine göre hatalı işlemler gerçekleştirmesi gerekir.

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

1. **None** (Yok) amacını seçin. Kullanıcılarınızın girebileceği ancak pizza sıralama uygulamanız ile ilgili olmayan üç tür ekleme:

    |`None`örnek söyleyceler|
    |--|
    |`Barking dogs are annoying`|
    |`Penguins in the ocean`|

    Bu `pizza`örnekler `cheese` `crust` `pickup` ,,,,,,,,,,,,,,,,,,, `deliver`