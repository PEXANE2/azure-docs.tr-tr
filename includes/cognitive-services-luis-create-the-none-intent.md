---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 355fe134939b26c51d6e03368f782845628a6b96
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188679"
---
İstemci uygulaması, bir söyleyiş anlamlı veya uygulama için uygun olup olmadığını bilmek gerekir. Bir sözcük istemci uygulaması tarafından yanıtlanamaz olup olmadığını belirlemek için oluşturma sürecinin bir parçası olarak her uygulama için **Yok** niyeti eklenir.

LUIS bir söz için **Yok** niyetini döndürürse, istemci uygulamanız kullanıcının konuşmayı sonlandırmak veya konuşmaya devam etmek için daha fazla yol tarifi vermek isteyip istemediğini sorabilir. 

> [!CAUTION] 
> **Yok** niyetini boş bırakmayın. 

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

2. **None** (Yok) amacını seçin. Kullanıcınızın girebileceği ancak İnsan Kaynakları uygulamanızla ilgili olmayan üç kelime ekleyin:

    | Örnek konuşmalar|
    |--|
    |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
    |Order a pizza for me (Bana bir pizza söyle)|
    |Penguins in the ocean (Okyanustaki penguenler)|
