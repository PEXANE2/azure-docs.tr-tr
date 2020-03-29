---
title: Proje Akustik Bake Çözünürlük
titlesuffix: Azure Cognitive Services
description: Bu kavramsal bakış, akustik pişirirken kaba ve ince çözünürlükler arasındaki farkı açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: d8eb3b2cbaf7b4e842d8338eefde756f6d381111
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68854359"
---
# <a name="project-acoustics-bake-resolution"></a>Proje Akustik Bake Çözünürlük
Bu kavramsal bakış, akustik pişirirken kaba ve ince çözünürlükler arasındaki farkı açıklar. Pişirme iş akışının Problar adımı nda bu ayarı seçersiniz.

## <a name="coarse-vs-fine-resolution"></a><a name="Coarse-vs-Fine-Resolution"></a>Kaba vs ince çözünürlük

Kaba ve ince çözünürlük ayarları arasındaki tek fark simülasyonun gerçekleştirilme sıklığıdır. Fine, kabafrekansın iki katı kadar yüksek bir frekans kullanır. Bunun akustik simülasyon üzerinde bir dizi etkisi vardır:

* Kaba için dalga boyu inceden iki kat daha uzundur ve bu nedenle voxel'ler iki kat daha büyüktür.
* Simülasyon süresi doğrudan voxel boyutu ile ilgilidir, kaba fırında yaklaşık 16 kat daha hızlı ince bir fırında n için yapılır.
* Voxel boyutundan daha küçük portallar (örneğin, kapı veya pencereler) simüle edilemez. Kaba ayar, bu küçük portalların bazılarının simüle edilmemelerine neden olabilir; bu nedenle, çalışma zamanında ses geçirilmez. Voxel'leri görüntüleyerek bunun olup olmadığını görebilirsiniz.
* Düşük simülasyon frekansı, köşeler ve kenarlar etrafında daha az kırınım sağlar.
* Ses kaynakları "dolu" voxel'lerin (yani geometri içeren voxel'lerin) içinde bulunamaz. Bu hiçbir ses ile sonuçlanır. Ses kaynaklarını yerleştirmek, böylece ince ayarı kullanırken olduğundan daha büyük kaba voxels içinde değildir yerleştirmek daha zordur.
* Daha büyük voxels portalları içine daha fazla izinsiz olacak, aşağıda gösterildiği gibi. İlk görüntü kaba kullanılarak oluşturulurken, ikincisi ince çözünürlük kullanılarak aynı kapıdır. Kırmızı işaretler ile belirtildiği gibi, ince ayar kullanarak kapı içine çok daha az saldırı vardır. Mavi çizgi geometri ile tanımlandığı gibi kapı, kırmızı çizgi voxel boyutu ile tanımlanan etkili akustik portal ise. Bu saldırının belirli bir durumda nasıl sonuçlandığı tamamen voxel'lerin, sahnedeki nesnelerin izine ve konumuna göre belirlenen portalın geometrisiyle nasıl hizalandığına bağlıdır.

![Unreal bir kapı doldurma kaba voxels Ekran görüntüsü](media/unreal-coarse-bake.png)

![Unreal bir kapı da ince voxels Ekran görüntüsü](media/unreal-fine-bake.png)

## <a name="next-steps"></a>Sonraki adımlar

[Unreal](unreal-baking.md) veya [Unity](unity-baking.md) eklentilerimizi kullanarak kaba ve ince çözünürlük ayarlarını kendiniz deneyin.
