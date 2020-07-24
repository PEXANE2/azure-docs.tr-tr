---
title: Ortamınızı ölçeklendirme-Azure Time Series Insights | Microsoft Docs
description: Azure portal kullanarak Azure Time Series Insights ortamınızı ölçeklendirmeyi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/30/2020
ms.custom: seodec18
ms.openlocfilehash: a552f03c8a8fa05ed7d2c6eb87374d4e7e17838d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87070087"
---
# <a name="how-to-scale-your-azure-time-series-insights-gen1-environment"></a>Azure Time Series Insights Gen1 ortamınızı ölçeklendirme

Bu makalede, [Azure Portal](https://portal.azure.com)kullanarak Azure Time Series Insights ortamınızın kapasitesinin nasıl değiştirileceği açıklanır. Kapasite, giriş oranına, depolama kapasitesine ve seçtiğiniz SKU 'la ilişkili maliyete uygulanan çarpandır.

Belirli bir fiyatlandırma SKU 'SU içindeki kapasiteyi artırmak veya azaltmak için Azure portal kullanabilirsiniz.

Ancak, fiyatlandırma katmanı SKU 'sunda değişiklik yapılmasına izin verilmez. Örneğin, S1 fiyatlandırma SKU 'SU olan bir ortam S2 'e dönüştürülemez veya tam tersi de geçerlidir.

## <a name="ga-limits"></a>GA sınırları

[!INCLUDE [Azure Time Series Insights GA limits](../../includes/time-series-insights-ga-limits.md)]

## <a name="change-the-capacity-of-your-environment"></a>Ortamınızın kapasitesini değiştirme

1. Azure portal, Azure Time Series Insights ortamınızı bulun ve seçin.

1. Azure Time Series Insights ortamınızın menüsünde **depolama yapılandırması**' nı seçin.

   [![Azure Time Series Insights kapasitenizi yapılandırın](media/scale-your-environment/scale-your-environment-configure.png)](media/scale-your-environment/scale-your-environment-configure.png#lightbox)

1. Giriş hızlarınız ve depolama kapasiteniz için gereksinimleri karşılayan kapasiteyi seçmek için **Kapasite** kaydırıcısını ayarlayın. Değişikliğin etkisini göstermek için, giriş **oranına**, **depolama kapasitesine**ve **Tahmini maliyet** güncelleştirmesine dinamik olarak dikkat edin.

   [![Kapasite kaydırıcısını kullanarak ortamınızı yapılandırma](media/scale-your-environment/scale-your-environment-slider.png)](media/scale-your-environment/scale-your-environment-slider.png#lightbox)

   Alternatif olarak, kaydırıcının sağ tarafındaki metin kutusuna kapasite çarpanı sayısını yazabilirsiniz.

1. Ortamı ölçeklendirmek için **Kaydet** ' i seçin. İlerleme göstergesi, değişiklik kaydedilene kadar anlık olarak görüntülenir.

1. [Azaltmayı engellemek için](time-series-insights-diagnose-and-solve-problems.md)yeni kapasitenin yeterli olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Azure Time Series Insights ' de saklama süresini anlama](time-series-insights-concepts-retention.md)konusunu gözden geçirin.

- [Azure Azure Time Series Insights 'da veri bekletmeyi yapılandırma](time-series-insights-how-to-configure-retention.md)hakkında bilgi edinin.

- [Ortamınızı planlama](time-series-insights-environment-planning.md)hakkında bilgi edinin.
