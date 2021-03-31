---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 9f8eadea198bbae3de2ffc1b3aaac48925719586
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "100105764"
---
En azından, ilk eğitim kümesinde etiket başına en az 30 görüntü kullanmanızı öneririz. Ayrıca, modelinizi eğitilen bir kez test etmek için birkaç ekstra görüntü toplamak isteyeceksiniz.

Modelinizi verimli bir şekilde eğitebilmek için, görüntüleri görsel ile birlikte kullanın. Değişen görüntüleri seçin:
* kamera açısı
* aydınlatma
* arka plan
* görsel stil
* bireysel/gruplanmış konu (ler)
* boyut
* tür

Ayrıca, tüm eğitim görüntülerinizin aşağıdaki ölçütleri karşıladığından emin olun:
* . jpg,. png,. bmp veya. gif biçimi
* boyutu 6MB 'tan büyük değil (tahmin görüntüleri için 4MB)
* en kısa kenarından 256 pikselden az değildir; Bundan kısa tüm görüntüler Özel Görüntü İşleme Hizmeti göre otomatik olarak ölçeklendirilir

> [!NOTE]
> Öğreticinizi tamamlamaya yönelik daha geniş bir görüntü kümesine mi ihtiyacınız var? Microsoft garaj projesi olan Trove, eğitim amaçlarıyla görüntü kümeleri toplamanıza ve satın almanıza olanak tanır. Görüntülerinizi topladıktan sonra bunları indirebilir ve ardından Özel Görüntü İşleme projenize her zamanki şekilde aktarabilirsiniz. Daha fazla bilgi edinmek için [Trove sayfasını](https://www.microsoft.com/ai/trove?activetab=pivot1:primaryr3) ziyaret edin.