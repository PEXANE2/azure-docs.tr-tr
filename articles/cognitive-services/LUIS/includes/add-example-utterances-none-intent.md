---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: e4147fbb27c8538f801f6c49f8b535a283faf50f
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325892"
---
İstemci uygulamanın, uygulamanın anlamlı veya uygulama için uygun olup olmadığını bilmeleri gerekir. Oluşturma işleminin bir parçası olarak, istemci uygulama tarafından yanıtlanmaması gerekip gerekmediğini tespit etmek için **hiçbiri** amacı her uygulamaya eklenir.

LUSıS, utterance için **hiçbiri** amacını döndürürse, istemci uygulamanız kullanıcının konuşmayı sonlandırmasını isteyip istemediğini veya konuşmaya devam etmek için daha fazla yol vermesini ister. 

**Hiçbiri** hedefini boş bırakırsanız, konu etki alanı dışında tahmin edilecek bir söylenişi, mevcut konu etki alanı amaçlarınında tahmin edilir. Sonuç olarak, bir sohbet bot gibi istemci uygulamasının yanlış bir tahmine göre hatalı işlemler gerçekleştirmesi gerekir. 

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

1. **None** (Yok) amacını seçin. Kullanıcılarınızın girebileceği ancak pizza sıralama uygulamanız ile ilgili olmayan üç tür ekleme:

    |`None` örnek söylenme|
    |--|
    |`Barking dogs are annoying`|
    |`Order a pizza for me`|
    |`Penguins in the ocean`|

    Bu örneklerde, `pizza`, `cheese`, `crust`, `pickup` `deliver`gibi konu etki alanında beklediğinizi bir sözcük kullanmamalısınız.