---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 07e7cc991f127bf4bb4f466c0108962786e45bce
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68423594"
---
**Performans** sekmesinin sol bölmesindeki **Olasılık Eşik** kaydırıcısını not edin. Bu, bir tahminin doğru kabul edilebilmesi için (kesinlik ve geri çağırma hesaplama amacıyla) sahip olması gereken güven düzeyidir. 

Tahmin çağrılarını yüksek olasılık eşiğiyle yorumladığınızda, algılanan sınıflandırmaların doğru olduğunu&mdash;geri çağırmak pahasına sonuçları yüksek hassasiyetle döndürme eğilimindedirler, ancak çoğu algılanmayadevam eder. Düşük olasılık eşiği,&mdash;gerçek sınıflandırmaların çoğunun tam tersi algılanır, ancak bu küme içinde daha fazla yanlış pozitif vardır. Bunu göz önünde bulundurarak, olasılık eşiğini projenizin özel ihtiyaçlarına göre ayarlamanız gerekir. Daha sonra, istemci tarafında tahmin sonuçları alırken, burada kullandığınız olasılık eşik değerini kullanmanız gerekir.