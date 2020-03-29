---
title: Keşif - Personalizer
titleSuffix: Azure Cognitive Services
description: Keşif le, Personalizer kullanıcı davranışı değişse bile iyi sonuçlar sunmaya devam edebiliyor. Bir keşif ayarı seçmek, modeli geliştirmek için, araştırılabilmek için kullanıcı etkileşimlerinin oranı hakkında bir iş kararıdır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 0b69c1fb070431ad61858322dce461f6496c35d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73490812"
---
# <a name="exploration-and-exploitation"></a>Keşif ve sömürü

Keşif le, Personalizer kullanıcı davranışı değişse bile iyi sonuçlar sunmaya devam edebiliyor.

Personalizer bir Rank çağrısı aldığında, aşağıdakileri yapan bir RewardActionID döndürür:
* Geçerli makine öğrenimi modeline dayalı olarak en olası kullanıcı davranışı yla eşleşecek şekilde yararlanma yı kullanır.
* Sıralamada en yüksek olasılık olan eyleme uymayan keşif kullanır.

Personalizer şu anda keşfetmek için *açgözlü epsilon* denilen bir algoritma kullanır. 

## <a name="choosing-an-exploration-setting"></a>Keşif ayarı seçme

Azure portalının Kisizer için **Yapılandırma** sayfasında keşif için kullanılacak trafik yüzdesini yapılandırırsınız. Bu ayar, arama yapan Rank çağrılarının yüzdesini belirler. 

Personalizer, her bir rütbe çağrısında bu olasılığı keşfedip keşfetmeyin veya bundan yararlanıp yararlanmayacağını belirler. Bu, belirli kullanıcı işllerinde bir davranışı kilitleyen bazı A/B çerçevelerinde kullanıcı davranışından farklıdır.

## <a name="best-practices-for-choosing-an-exploration-setting"></a>Bir keşif ayarı seçmek için en iyi uygulamalar

Bir keşif ayarı seçmek, modeli geliştirmek için, araştırılabilmek için kullanıcı etkileşimlerinin oranı hakkında bir iş kararıdır. 

Sıfır bir ayar Personalizer birçok yararları inkar edecektir. Bu ayar sayesinde, Personalizer daha iyi kullanıcı etkileşimlerini keşfetmek için kullanıcı etkileşimi kullanmaz. Bu model durgunluk yol açar, sürüklenme, ve sonuçta daha düşük performans.

Çok yüksek bir ayar, kullanıcı davranışından öğrenmenin faydalarını inkâr edecektir. %100'e ayarlamak sabit bir rasgeleleştirme anlamına gelir ve kullanıcılardan öğrenilen herhangi bir davranış sonucu etkilemez.

Personalizer'ın keşfedip keşfetmediğini veya sömürdüğünü görüp görmediğininize bağlı olarak uygulama davranışını değiştirmemeniz önemlidir. Bu sonuçta potansiyel performansı azaltacak öğrenme önyargıları yol açacak.

## <a name="next-steps"></a>Sonraki adımlar

[Pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) 