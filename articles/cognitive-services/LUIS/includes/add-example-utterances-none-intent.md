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
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77279653"
---
İstemci uygulamanın, uygulamanın anlamlı veya uygulama için uygun olup olmadığını bilmeleri gerekir. Oluşturma işleminin bir parçası olarak, istemci uygulama tarafından yanıtlanmaması gerekip gerekmediğini tespit etmek için **hiçbiri** amacı her uygulamaya eklenir.

LUSıS, utterance için **hiçbiri** amacını döndürürse, istemci uygulamanız kullanıcının konuşmayı sonlandırmasını isteyip istemediğini veya konuşmaya devam etmek için daha fazla yol vermesini ister.

**Hiçbiri** hedefini boş bırakırsanız, konu etki alanı dışında tahmin edilecek bir söylenişi, mevcut konu etki alanı amaçlarınında tahmin edilir. Sonuç olarak, bir sohbet bot gibi istemci uygulamasının yanlış bir tahmine göre hatalı işlemler gerçekleştirmesi gerekir.

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

1. **None** (Yok) amacını seçin. Kullanıcılarınızın girebileceği ancak pizza sıralama uygulamanız ile ilgili olmayan üç tür ekleme:

    |`None` örnek söylenme|
    |--|
    |`Barking dogs are annoying`|
    |`Penguins in the ocean`|

    Bu örneklerde, `pizza`, `cheese`, `crust`, `pickup` `deliver`gibi konu etki alanında beklediğinizi bir sözcük kullanmamalısınız.