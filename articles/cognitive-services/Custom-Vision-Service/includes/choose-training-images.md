---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: ce67062f83ad10703dfb1f0ee1f3741e698fb450
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "70130124"
---
En azından, ilk eğitim kümesinde etiket başına en az 30 görüntü kullanmanızı öneririz. Ayrıca, modelinizi eğitilen bir kez test etmek için birkaç ekstra görüntü toplamak isteyeceksiniz.

Modelinizi verimli bir şekilde eğitebilmek için, görüntüleri görsel ile birlikte kullanın. Değişen görüntüleri seçin:
* kamera açısı
* aydınlatma
* arka plan
* görsel stil
* bireysel/gruplanmış konu (ler)
* size
* type

Ayrıca, tüm eğitim görüntülerinizin aşağıdaki ölçütleri karşıladığından emin olun:
* . jpg,. png,. bmp veya. gif biçimi
* boyutu 6MB 'tan büyük değil (tahmin görüntüleri için 4MB)
* en kısa kenarından 256 pikselden az değildir; Bundan kısa tüm görüntüler Özel Görüntü İşleme Hizmeti göre otomatik olarak ölçeklendirilir
