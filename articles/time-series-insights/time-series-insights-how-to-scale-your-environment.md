---
title: Azure Time Series Insights ortamınızı ölçeklendirme | Microsoft Docs
description: Bu makalede Azure Time Series Insights ortamınızın nasıl ölçeklendiriyapılacağı açıklanır. Fiyatlandırma SKU 'sunda kapasite eklemek veya çıkarmak için Azure portal kullanın.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/27/2019
ms.custom: seodec18
ms.openlocfilehash: 13fa2b892013cf4a3fb96220c901030c6b0aee0b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129101"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Time Series Insights ortamınızı ölçeklendirme

Bu makalede, Azure portal kullanarak ortamınızın Time Series Insights ortamınızın kapasitesini nasıl değiştirileceği açıklanır. Kapasite, giriş oranına, depolama kapasitesine ve seçtiğiniz SKU 'la ilişkili maliyete uygulanan çarpandır.

Belirli bir fiyatlandırma SKU 'SU içindeki kapasiteyi artırmak veya azaltmak için Azure portal kullanabilirsiniz.

Ancak, fiyatlandırma katmanı SKU 'sunda değişiklik yapılmasına izin verilmez. Örneğin, S1 fiyatlandırma SKU 'SU olan bir ortam S2 'e dönüştürülemez veya tam tersi de geçerlidir.

## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU giriş fiyatları ve kapasiteleri

| S1 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1\. | 1 GB (1.000.000 olay) | ayda 30 GB (30.000.000 olay) |
| 10 | 10 GB (10.000.000 olay) | ayda 300 GB (300.000.000 olay) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU giriş fiyatları ve kapasiteleri

| S2 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1\. | 10 GB (10.000.000 olay) | ayda 300 GB (300.000.000 olay) |
| 10 | 100 GB (100.000.000 olay) | ayda 3 TB (3.000.000.000 olay) |

Kapasitelerin ölçeği doğrusal olarak, kapasite 2 ' ye sahip bir S1 SKU 'SU her gün giriş oranı ve 60 GB (60.000.000 olay) için 2 GB (2.000.000) olay destekler.

## <a name="change-the-capacity-of-your-environment"></a>Ortamınızın kapasitesini değiştirme

1. Azure portal, Time Series Insights ortamınızı bulun ve seçin.

1. Time Series Insights ortamınızın menüsünde **Yapılandır**' ı seçin.

   [![. png 'yi yapılandırma](media/scale-your-environment/configure.png)](media/scale-your-environment/configure.png#lightbox)

1. Giriş hızlarınız ve depolama kapasiteniz için gereksinimleri karşılayan kapasiteyi seçmek için **Kapasite** kaydırıcısını ayarlayın. Değişikliğin etkisini göstermek için, giriş **oranına**, **depolama kapasitesine**ve **Tahmini maliyet** güncelleştirmesine dinamik olarak dikkat edin.

   [![Sürgü](media/scale-your-environment/slider.png)](media/scale-your-environment/slider.png#lightbox)

   Alternatif olarak, kaydırıcının sağ tarafındaki metin kutusuna kapasite çarpanı sayısını yazabilirsiniz.

1. Ortamı ölçeklendirmek için **Kaydet** ' i seçin. İlerleme göstergesi, değişiklik kaydedilene kadar anlık olarak görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azaltmayı engellemek için](time-series-insights-diagnose-and-solve-problems.md)yeni kapasitenin yeterli olduğunu doğrulayın.