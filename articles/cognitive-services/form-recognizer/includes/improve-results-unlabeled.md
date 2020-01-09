---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: f0761847c3677b324ef16c5987eb9a1561dbcbe0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75379309"
---
`"pageResults"` düğümü altındaki her bir anahtar/değer sonucunun `"confidence"` değerlerini inceleyin. Ayrıca, metin okuma işlemine karşılık gelen `"readResults"` düğümündeki güven puanlarını de göz atalım. Okuma sonuçlarının güvenilirliği, anahtar/değer ayıklama sonuçlarının güvenini etkilemez, bu nedenle her ikisini de denetlemeniz gerekir.
* Okuma işleminin güven puanları düşükse, giriş belgelerinizin kalitesini geliştirmeyi deneyin (bkz. [giriş gereksinimleri](../overview.md#input-requirements)).
* Anahtar/değer ayıklama işleminin güven puanları düşükse, çözümlenmekte olan belgelerin Eğitim kümesinde kullanılan belgelerle aynı türde olduğundan emin olun. Eğitim kümesindeki belgeler görünümde Çeşitlemeler içeriyorsa, bunları farklı klasörlere bölmeyi ve her varyasyon için bir modeli eğitmek için bir modele göz önünde bulundurun.

Hedeflediğiniz güven puanları kullanım örneğine bağlıdır, ancak genellikle %80 veya üzeri bir puanı hedeflemek iyi bir uygulamadır. Tıbbi kayıtları veya faturalandırma deyimlerini okumak gibi daha hassas durumlar için %100 puanı önerilir.