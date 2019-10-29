---
title: Azure Time Series Insights ortamınızı ölçeklendirme | Microsoft Docs
description: Bu makalede Azure Time Series Insights ortamınızın nasıl ölçeklendiriyapılacağı açıklanır. Fiyatlandırma SKU 'sunda kapasite eklemek veya çıkarmak için Azure portal kullanın.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: 3f03f5ed75c720c9b0daf30d721ef4d2aee9749c
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991148"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Time Series Insights ortamınızı ölçeklendirme

Bu makalede, [Azure Portal](https://portal.azure.com)kullanarak Time Series Insights ortamınızın kapasitesinin nasıl değiştirileceği açıklanır. Kapasite, giriş oranına, depolama kapasitesine ve seçtiğiniz SKU 'la ilişkili maliyete uygulanan çarpandır.

Belirli bir fiyatlandırma SKU 'SU içindeki kapasiteyi artırmak veya azaltmak için Azure portal kullanabilirsiniz.

Ancak, fiyatlandırma katmanı SKU 'sunda değişiklik yapılmasına izin verilmez. Örneğin, S1 fiyatlandırma SKU 'SU olan bir ortam S2 'e dönüştürülemez veya tam tersi de geçerlidir.

## <a name="ga-limits"></a>GA sınırları

[!INCLUDE [Azure Time Series Insights GA limits](../../includes/time-series-insights-ga-limits.md)]

## <a name="change-the-capacity-of-your-environment"></a>Ortamınızın kapasitesini değiştirme

1. Azure portal, Time Series Insights ortamınızı bulun ve seçin.

1. Time Series Insights ortamınızın menüsünde **Yapılandır**' ı seçin.

   [![. png 'yi yapılandırma](media/scale-your-environment/configure.png)](media/scale-your-environment/configure.png#lightbox)

1. Giriş hızlarınız ve depolama kapasiteniz için gereksinimleri karşılayan kapasiteyi seçmek için **Kapasite** kaydırıcısını ayarlayın. Değişikliğin etkisini göstermek için, giriş **oranına**, **depolama kapasitesine**ve **Tahmini maliyet** güncelleştirmesine dinamik olarak dikkat edin.

   [![kaydırıcı](media/scale-your-environment/slider.png)](media/scale-your-environment/slider.png#lightbox)

   Alternatif olarak, kaydırıcının sağ tarafındaki metin kutusuna kapasite çarpanı sayısını yazabilirsiniz.

1. Ortamı ölçeklendirmek için **Kaydet** ' i seçin. İlerleme göstergesi, değişiklik kaydedilene kadar anlık olarak görüntülenir.

1. [Azaltmayı engellemek için](time-series-insights-diagnose-and-solve-problems.md)yeni kapasitenin yeterli olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Time Series Insights ' de saklama süresini anlama](time-series-insights-concepts-retention.md)konusunu gözden geçirin.

- [Azure Time Series Insights 'da veri bekletmeyi yapılandırma](time-series-insights-how-to-configure-retention.md)hakkında bilgi edinin.

- [Ortamınızı planlama](time-series-insights-environment-planning.md)hakkında bilgi edinin.