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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77279653"
---
İstemci uygulaması, bir söyleyiş anlamlı veya uygulama için uygun olup olmadığını bilmek gerekir. Bir sözcük istemci uygulaması tarafından yanıtlanması gerekip gerekmeden karar vermek için oluşturma işleminin bir parçası olarak her uygulamaya **Yok** amacı eklenir.

LUIS bir söz için **Yok** niyetini döndürürse, istemci uygulamanız kullanıcının konuşmayı sonlandırmak veya konuşmaya devam etmek için daha fazla yol tarifi vermek isteyip istemediğini sorabilir.

**Yok** niyetini boş bırakırsanız, konu etki alanı dışında tahmin edilmesi gereken bir söyleyiş varolan konu etki alanı niyetlerinden birinde tahmin edilir. Sonuç olarak, sohbet botu gibi istemci uygulaması, yanlış bir tahmine dayalı olarak yanlış işlemler gerçekleştirecektir.

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

1. **None** (Yok) amacını seçin. Kullanıcınızın girebileceği ancak Pizza sipariş uygulamanızla ilgili olmayan üç kelime ekleyin:

    |`None`örnek söz|
    |--|
    |`Barking dogs are annoying`|
    |`Penguins in the ocean`|

    Bu örnekler, konu etki alanınızda beklediğiniz `pizza`kelimeleri `cheese` `crust`, `pickup` `deliver`, , .