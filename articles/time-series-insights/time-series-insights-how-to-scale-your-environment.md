---
title: Ortamınızı ölçeklendirme - Azure Time Series Öngörüleri| Microsoft Dokümanlar
description: Azure portalını kullanarak Azure Zaman Serisi Öngörüleri ortamınızı nasıl ölçeklendireceklerini öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/17/2020
ms.custom: seodec18
ms.openlocfilehash: 9604f0c6eeb9d0ac4b7d07f1ab1ba03285fd60fe
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640078"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Zaman Serisi Öngörüleri ortamınızı ölçeklendirme

Bu makalede, [Azure portalını](https://portal.azure.com)kullanarak Zaman Serisi Öngörüler ortamınızın kapasitesini nasıl değiştireceğiniz açıklanmaktadır. Kapasite, seçtiğiniz SKU ile ilişkili giriş hızına, depolama kapasitesine ve maliyete uygulanan çarpandır.

Belirli bir fiyatlandırma SKU'su içindeki kapasiteyi artırmak veya azaltmak için Azure portalını kullanabilirsiniz.

Ancak, fiyatlandırma katmanı SKU'yu değiştirmeye izin verilmez. Örneğin, S1 fiyatlandırmaSKU'su olan bir ortam S2'ye dönüştürülemez veya tam tersi.

## <a name="ga-limits"></a>GA sınırları

[!INCLUDE [Azure Time Series Insights GA limits](../../includes/time-series-insights-ga-limits.md)]

## <a name="change-the-capacity-of-your-environment"></a>Ortamınızın kapasitesini değiştirme

1. Azure portalında Time Series Öngörüleri ortamınızı bulun ve seçin.

1. Time Series Insights ortamınızın menüsünde **Depolama Yapılandırması'nı**seçin.

   [![Zaman Serisi Öngörüleri kapasitenizi yapılandırın](media/scale-your-environment/scale-your-environment-configure.png)](media/scale-your-environment/scale-your-environment-configure.png#lightbox)

1. Giriş hızlarınız ve depolama kapasiteniz için gereksinimleri karşılayan kapasiteyi seçmek için **Kapasite** kaydırıcısını ayarlayın. Değişikliğin etkisini göstermek için **Giriş hızına,** **Depolama kapasitesine**ve **Tahmini maliyet** güncelleştirmesine dinamik olarak dikkat edin.

   [![Kapasite kaydırıcısını kullanarak ortamınızı yapılandırın](media/scale-your-environment/scale-your-environment-slider.png)](media/scale-your-environment/scale-your-environment-slider.png#lightbox)

   Alternatif olarak, kaydırıcının sağındaki metin kutusuna kapasite çarpanı sayısını yazabilirsiniz.

1. Ortamı ölçeklendirmek için **Kaydet'i** seçin. İlerleme göstergesi, değişiklik kısa bir süre için yapılana kadar görüntülenir.

1. Yeni kapasitenin [azaltmayı önlemek için yeterli](time-series-insights-diagnose-and-solve-problems.md)olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için, [Zaman Serisi Öngörüleri'nde anlama tutmayı gözden geçirin.](time-series-insights-concepts-retention.md)

- Azure [Zaman Serisi Öngörüleri'nde veri saklama](time-series-insights-how-to-configure-retention.md)yı yapılandırma hakkında bilgi edinin.

- [Çevrenizi planlama](time-series-insights-environment-planning.md)hakkında bilgi edinin.
