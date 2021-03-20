---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 07e7cc991f127bf4bb4f466c0108962786e45bce
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96018393"
---
**Performans** sekmesinin sol bölmesindeki **olasılık eşiği** kaydırıcısını aklınızda edin. Bu, bir tahmine uygun kabul edilmesi için gereken güven düzeyidir (duyarlık ve geri çekmeyi hesaplama amacıyla). 

Tahmin çağrılarını yüksek bir olasılık eşiğine göre yorumladığınızda, &mdash; algılanan sınıflandırmaların doğru olduğu, ancak çok fazla algılanamayan sınıflandırmalar için yüksek duyarlıklı sonuçları döndürür. Düşük olasılık eşiği, &mdash; gerçek sınıflandırmaların büyük bir kısmının algılanmadığına karşın bu küme içinde daha fazla hatalı pozitif sonuç vardır. Bu şekilde, olasılık eşiğini projenizin özel ihtiyaçlarına göre ayarlamanız gerekir. Daha sonra, istemci tarafında tahmin sonuçları aldığınızda, burada kullandığınız olasılık eşiği değerini kullanmanız gerekir.