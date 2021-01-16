---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: d07a5da3b9013700694f6c20102ef2e8c5066087
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2021
ms.locfileid: "98256442"
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
> Microsoft garaj projesi olan Trove, eğitim amaçlarıyla görüntü kümeleri toplamanıza ve satın almanıza olanak tanır. Görüntülerinizi topladıktan sonra bunları indirebilir ve ardından Özel Görüntü İşleme projenize her zamanki şekilde aktarabilirsiniz. Daha fazla bilgi edinmek için [Trove sayfasını](https://www.microsoft.com/en-us/ai/trove?activetab=pivot1:primaryr3) ziyaret edin.