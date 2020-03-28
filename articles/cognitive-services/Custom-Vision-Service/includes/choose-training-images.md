---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: ce67062f83ad10703dfb1f0ee1f3741e698fb450
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70130124"
---
En az olarak, ilk eğitim kümesinde etiket başına en az 30 resim kullanmanızı öneririz. Ayrıca, modelinizi eğitildikten sonra test etmek için birkaç ekstra resim toplamak isteyeceksiniz.

Modelinizi etkili bir şekilde eğitmek için görsel çeşitli görüntüler kullanın. Göre değişen görüntüleri seçin:
* kamera açısı
* Aydınlatma
* arka plan
* görsel stil
* bireysel/gruplanmış konu(lar)
* size
* type

Ayrıca, tüm eğitim görsellerinizin aşağıdaki ölçütleri karşıladığından emin olun:
* .jpg, .png, .bmp veya .gif biçimi
* boyutu 6MB 'dan büyük değil (tahmin görüntüleri için 4MB)
* en kısa kenarda en az 256 piksel; bundan daha kısa olan tüm görüntüler, Özel Vizyon Hizmeti tarafından otomatik olarak ölçeklendirilir
