---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: f0761847c3677b324ef16c5987eb9a1561dbcbe0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75379309"
---
Düğüm `"confidence"` altında her anahtar/değer sonucuiçin `"pageResults"` değerleri inceleyin. `"readResults"` Düğümdeki güven puanlarına da bakmalısınız, bu da metin okuma işlemine karşılık gelir. Okuma sonuçlarının güveni anahtar/değer çıkarma sonuçlarının güvenini etkilemez, bu nedenle her ikisini de kontrol etmelisiniz.
* Okuma işlemi için güven puanları düşükse, giriş belgelerinizin kalitesini yükseltmeye çalışın (bkz. [Giriş gereksinimleri).](../overview.md#input-requirements)
* Anahtar/değer çıkarma işleminin güven puanları düşükse, analiz edilen belgelerin eğitim kümesinde kullanılan belgelerle aynı türde olduğundan emin olun. Eğitim kümesindeki belgelerin görünüm varyasyonları varsa, bunları farklı klasörlere bölmeyi ve her varyasyon için bir modeli eğitmeyi düşünün.

Hedeflediğiniz güven puanları kullanım durumunuza bağlıdır, ancak genellikle %80 veya üzerinde bir puanı hedeflemek iyi bir uygulamadır. Tıbbi kayıtların okunması veya faturalandırma ekstrelerinin okunması gibi daha hassas vakalar için %100 puan önerilir.