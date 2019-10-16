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
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: a899de22137decc1eb1578369a2751710c17abda
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72332869"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Time Series Insights ortamınızı ölçeklendirme

Bu makalede, [Azure Portal](https://portal.azure.com)kullanarak Time Series Insights ortamınızın kapasitesinin nasıl değiştirileceği açıklanır. Kapasite, giriş oranına, depolama kapasitesine ve seçtiğiniz SKU 'la ilişkili maliyete uygulanan çarpandır.

Belirli bir fiyatlandırma SKU 'SU içindeki kapasiteyi artırmak veya azaltmak için Azure portal kullanabilirsiniz.

Ancak, fiyatlandırma katmanı SKU 'sunda değişiklik yapılmasına izin verilmez. Örneğin, S1 fiyatlandırma SKU 'SU olan bir ortam S2 'e dönüştürülemez veya tam tersi de geçerlidir.

## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU giriş fiyatları ve kapasiteleri

| S1 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | 1 GB (1.000.000 olay) | ayda 30 GB (30.000.000 olay) |
| 10 | 10 GB (10.000.000 olay) | ayda 300 GB (300.000.000 olay) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU giriş fiyatları ve kapasiteleri

| S2 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | 10 GB (10.000.000 olay) | ayda 300 GB (300.000.000 olay) |
| 10 | 100 GB (100.000.000 olay) | ayda 3 TB (3.000.000.000 olay) |

Kapasitelerin ölçeği doğrusal olarak, kapasite 2 ' ye sahip bir S1 SKU 'SU her gün giriş oranı ve 60 GB (60.000.000 olay) için 2 GB (2.000.000) olay destekler.

## <a name="change-the-capacity-of-your-environment"></a>Ortamınızın kapasitesini değiştirme

1. Azure portal, Time Series Insights ortamınızı bulun ve seçin.

1. Time Series Insights ortamınızın menüsünde **Yapılandır**' ı seçin.

   [@no__t -1. png 'yi Yapılandır](media/scale-your-environment/configure.png)](media/scale-your-environment/configure.png#lightbox)

1. Giriş hızlarınız ve depolama kapasiteniz için gereksinimleri karşılayan kapasiteyi seçmek için **Kapasite** kaydırıcısını ayarlayın. Değişikliğin etkisini göstermek için, giriş **oranına**, **depolama kapasitesine**ve **Tahmini maliyet** güncelleştirmesine dinamik olarak dikkat edin.

   [![Kaydırıcı](media/scale-your-environment/slider.png)](media/scale-your-environment/slider.png#lightbox)

   Alternatif olarak, kaydırıcının sağ tarafındaki metin kutusuna kapasite çarpanı sayısını yazabilirsiniz.

1. Ortamı ölçeklendirmek için **Kaydet** ' i seçin. İlerleme göstergesi, değişiklik kaydedilene kadar anlık olarak görüntülenir.

1. [Azaltmayı engellemek için](time-series-insights-diagnose-and-solve-problems.md)yeni kapasitenin yeterli olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Time Series Insights ' de saklama süresini anlama](time-series-insights-concepts-retention.md)konusunu gözden geçirin.

- [Azure Time Series Insights 'da veri bekletmeyi yapılandırma](time-series-insights-how-to-configure-retention.md)hakkında bilgi edinin.

- [Ortamınızı planlama](time-series-insights-environment-planning.md)hakkında bilgi edinin.