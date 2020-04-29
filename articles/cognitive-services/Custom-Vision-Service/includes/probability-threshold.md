---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 07e7cc991f127bf4bb4f466c0108962786e45bce
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "68423594"
---
**Performans** sekmesinin sol bölmesindeki **olasılık eşiği** kaydırıcısını aklınızda edin. Bu, bir tahmine uygun kabul edilmesi için gereken güven düzeyidir (duyarlık ve geri çekmeyi hesaplama amacıyla). 

Tahmin çağrılarını yüksek bir olasılık eşiğine göre yorumladığınızda, algılanan sınıflandırmaların doğru olduğu, ancak çok fazla algılanamayan sınıflandırmalar için yüksek&mdash;duyarlıklı sonuçları döndürür. Düşük olasılık eşiği, gerçek sınıflandırmaların&mdash;büyük bir kısmının algılanmadığına karşın bu küme içinde daha fazla hatalı pozitif sonuç vardır. Bu şekilde, olasılık eşiğini projenizin özel ihtiyaçlarına göre ayarlamanız gerekir. Daha sonra, istemci tarafında tahmin sonuçları aldığınızda, burada kullandığınız olasılık eşiği değerini kullanmanız gerekir.