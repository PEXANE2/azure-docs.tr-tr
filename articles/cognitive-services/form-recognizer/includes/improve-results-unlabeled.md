---
author: laujan
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: lajanuar
ms.openlocfilehash: 89b035397ea2050ae7e61f2a19310b6a7fb4192c
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467236"
---
`"confidence"`Düğüm altındaki her bir anahtar/değer sonucunun değerlerini inceleyin `"pageResults"` . Ayrıca `"readResults"` , düğümdeki, metin okuma işlemine karşılık gelen güven puanlarını de göz atalım. Okuma sonuçlarının güvenilirliği, anahtar/değer ayıklama sonuçlarının güvenini etkilemez, bu nedenle her ikisini de denetlemeniz gerekir.
* Okuma işleminin güven puanları düşükse, giriş belgelerinizin kalitesini geliştirmeyi deneyin (bkz. [giriş gereksinimleri](../overview.md#input-requirements)).
* Anahtar/değer ayıklama işleminin güven puanları düşükse, çözümlenmekte olan belgelerin Eğitim kümesinde kullanılan belgelerle aynı türde olduğundan emin olun. Eğitim kümesindeki belgeler görünümde Çeşitlemeler içeriyorsa, bunları farklı klasörlere bölmeyi ve her varyasyon için bir modeli eğitmek için bir modele göz önünde bulundurun.

Hedeflediğiniz güven puanları kullanım örneğine bağlıdır, ancak genellikle %80 veya üzeri bir puanı hedeflemek iyi bir uygulamadır. Tıbbi kayıtları veya faturalandırma deyimlerini okumak gibi daha hassas durumlar için %100 puanı önerilir.