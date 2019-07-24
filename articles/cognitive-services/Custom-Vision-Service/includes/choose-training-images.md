---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: ee2c1fd1fc1cad07b14a2c99318be20be30db9c5
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423552"
---
En azından, ilk eğitim kümesinde etiket başına en az 30 görüntü kullanmanızı öneririz. Ayrıca, modelinizi eğitilen bir kez test etmek için birkaç ekstra görüntü toplamak isteyeceksiniz.

Modelinizi verimli bir şekilde eğitebilmek için, görüntüleri görsel ile birlikte kullanın. Değişen görüntüleri seçin:
* kamera açısı
* aydınlatma
* arka plan
* görsel stil
* bireysel/gruplanmış konu (ler)
* size
* türü

Ayrıca, tüm eğitim görüntülerinizin aşağıdaki ölçütleri karşıladığından emin olun:
* . jpg,. png veya. bmp biçimi
* boyutu 6MB 'tan büyük değil (tahmin görüntüleri için 4MB)
* en kısa kenarından 256 pikselden az değildir; Bundan kısa tüm görüntüler Özel Görüntü İşleme Hizmeti göre otomatik olarak ölçeklendirilir