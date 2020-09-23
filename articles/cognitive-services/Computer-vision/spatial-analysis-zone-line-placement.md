---
title: Uzamsal analiz bölgesi ve satır yerleşimi
titleSuffix: Azure Cognitive Services
description: Uzamsal analizler ile bölgeleri ve satırları ayarlamayı öğrenin
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: aahi
ms.openlocfilehash: 7e2a64c14d7d7a1d20b64b746969aca1e60ab218
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941678"
---
# <a name="zone-and-line-placement-guide"></a>Bölge ve satır yerleştirme Kılavuzu

Bu makalede, bir alanda Peoples hareketlerinin doğru analizini elde etmek için uzamsal analiz işlemlerine yönelik bölgelerin ve satırların nasıl tanımlanacağı hakkında yönergeler sunulmaktadır. Bu tüm işlemler için geçerlidir. 

Bölgeler ve satırlar JSON SPACEANALYSIS_CONFIG parametresi kullanılarak tanımlanır. Daha fazla bilgi için bkz. [uzamsal analiz işlemleri](spatial-analysis-operations.md) makalesi.

## <a name="guidelines-for-drawing-zones"></a>Çizim bölgeleri için yönergeler

Her alanın farklı olduğunu unutmayın; gereksinimlerinize bağlı olarak konum veya boyut güncelleştirmeniz gerekir.

Kamera görünümlerinizin belirli bir bölümünü görmek isterseniz, ilgilendiğiniz en büyük bölgeyi oluşturun, ancak ilgilendiğiniz diğer alanlara dahil olmamak üzere, ilgilendiğiniz belirli bir kat alanını kapsayan. Bu, toplanan verilerin doğruluğunu artırır ve izlemek istemediğiniz alanlardan hatalı pozitif sonuçlar vermez. Poligonun köşelerini yerleştirirken, izlemek istediğiniz alanın dışında olmadığından emin olun.  

### <a name="example-of-a-well-shaped-zone"></a>İyi şekillendirilmiş bir bölgenin örneği

Bölge, her bir kenar üzerinde duran ve ilgilendiğiniz alana odaklanmış üç kişiyi kapsayacak kadar büyük olmalıdır. Uzamsal analizler, fit 'in bölgeye yerleştirilmiş olduğu kişileri belirler. bu nedenle, 2B görüntüye bölge çizilirken, bölgeyi kata yerleştirmekte olan bir carevcil hayvan olarak düşünün.

![İyi şekillendirilmiş bölge](./media/spatial-analysis/zone-good-example.png)

### <a name="examples-of-zones-that-arent-well-shaped"></a>İyi şekillendirilmiş bölge örnekleri

Aşağıdaki örneklerde kötü şekillendirilmiş bölgeler gösterilmektedir. Bu örneklerde ilgilendiğiniz alan, *oyunun oyun zamanının* önünde bulunduğu alandır.

**Bölge, zemin üzerinde değil.**

![Kötü şekillendirilmiş bölge](./media/spatial-analysis/zone-not-on-floor.png) 

**Bölge çok küçük.**

![bölge çok küçük](./media/spatial-analysis/zone-too-small.png)

**Bölge, ekran çevresindeki alanı tamamen yakalamaz.**

![bölge, son Cap etrafındaki alanı tamamen yakalamaz](./media/spatial-analysis/zone-bad-capture.png)

**Bölge, Kamera görüntüsünün kenarına çok yakın ve doğru görüntüyü yakalamaz.**

![bölge, Kamera görüntüsünün kenarına çok yakın ve doğru görüntüyü yakalamaz](./media/spatial-analysis/zone-edge.png)

**Bölge, raf tarafından kısmen engellenerek insanlar ve kat tamamen görünmez.**

![bölge kısmen engellenmiştir, bu nedenle insanlar tamamen görünmez](./media/spatial-analysis/zone-partially-blocked.png)

### <a name="example-of-a-well-shaped-line"></a>İyi şekillendirilmiş bir çizgi örneği

Satır, tüm giriş sığacak kadar uzun olmalıdır. Uzamsal analiz, satır içindeki kişileri belirler. bu nedenle, 2B görüntüde çizgiler çizilirken bunları tabanda göründükleri gibi çizdiğinizi düşünelim. 

Mümkünse, satırı gerçek giriş değerinden daha geniş bir şekilde genişletin. Bu, ek çapraz çizgiler (satır bir duvarta olduğunda aşağıdaki görüntüde olduğu gibi) ile sonuçlanmıyorsa onu genişletir.

![İyi şekillendirilmiş çizgi](./media/spatial-analysis/zone-line-good-example.png)

### <a name="examples-of-lines-that-arent-well-shaped"></a>İyi şekillendirilmiş olmayan satır örnekleri

Aşağıdaki örneklerde kötü tanımlanmış satırlar gösterilmektedir.

**Çizgi, tabandaki tüm giriş yolunu kapsamıyor.**

![Satır, tabandaki tüm giriş yolunu kapsamıyor](./media/spatial-analysis/zone-line-bad-coverage.png)

**Satır çok yüksek ve kapıdan tamamen kapsamıyor.**

![Satır çok yüksek ve kapıdan tamamen kapsamıyor](./media/spatial-analysis/zone-line-too-high.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Web uygulaması sayımını yapan bir kişi dağıtın](spatial-analysis-web-app.md)
* [Uzamsal analiz işlemlerini yapılandırma](./spatial-analysis-operations.md)
* [Günlüğe kaydetme ve sorun giderme](spatial-analysis-logging.md)
* [Kamera Yerleştirme Kılavuzu](spatial-analysis-camera-placement.md)
