---
title: Azure purview 'da verileriniz hakkındaki öngörüleri tarayın
description: Bu nasıl yapılır kılavuzunda, verileriniz üzerinde purview Insights tarama raporlaması 'nın nasıl görüntüleneceği ve kullanılacağı açıklanmaktadır.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/20/2020
ms.openlocfilehash: ea12bf8a8d93f14c5364864b97d1173fe8602765
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96554425"
---
# <a name="scan-insights-on-your-data-in-azure-purview"></a>Azure purview 'da verileriniz hakkındaki öngörüleri tarayın

Bu nasıl yapılır kılavuzunda verileriniz için Azure purview tarama öngörüleriyle ilgili nasıl erişme, görüntüleme ve filtreleme açıklanmaktadır.

Bu nasıl yapılır kılavuzunda şunları yapmayı öğreneceksiniz:

* Purview hesabınızdan öngörüleri görüntüleyin.
* Taramalarınızın bir kuşbakışı görünümünü alın.

## <a name="prerequisites"></a>Ön koşullar

Purview Insights 'ı kullanmaya başlamadan önce aşağıdaki adımları tamamladığınızdan emin olun:

* Azure kaynaklarınızı ayarlayın ve hesabı verilerle doldurun.
* Veri kaynağındaki bir taramayı ayarlayın ve doldurun.

Daha fazla bilgi için bkz. [Azure purview 'ta veri kaynaklarını yönetme (Önizleme)](manage-data-sources.md).

## <a name="use-purview-scan-insights"></a>Purview tarama öngörülerini kullanma

Azure purview ' de kaynak türlerini kaydedebilir ve tarayabilirsiniz. Tarama öngörülerine zaman içinde tarama durumunu görüntüleyebilirsiniz. Öngörüler, belirli bir süre içinde başarısız olan, başarılı veya alma işlemlerinin kaç kez iptal edildiğini söyler.

### <a name="view-scan-insights"></a>Tarama öngörülerini görüntüleme

1. Azure portal **Azure purview** örneği ekranına gidin ve purview hesabınızı seçin.

1. **Genel bakış** sayfasında, **Başlarken** bölümünde, **purview Studio 'yu aç** kutucuğunu seçin.

   :::image type="content" source="./media/scan-insights/portal-access.png" alt-text="Azure portal Takiview 'ı başlatın":::

1. Takip görünümü **giriş** sayfasında, Öngörüler alanına erişmek için **öngörüleri görüntüle** kutucuğunu seçin **Insights** :::image type="icon" source="media/scan-insights/ico-insights.png" border="false"::: .

   :::image type="content" source="./media/scan-insights/view-insights.png" alt-text="Azure portal öngörülerinizi görüntüleyin":::

1. **Öngörüler** :::image type="icon" source="media/scan-insights/ico-insights.png" border="false"::: alanında **taramalar** ' i seçerek purview **tarama öngörüleri** raporunu görüntüleyin.

### <a name="view-high-level-kpis-to-show-count-of-scans-by-status"></a>Duruma göre tarama sayısını göstermek için üst düzey KPI 'Ları görüntüle
 
Üst düzey KPI 'ler, bir dönem içinde çalıştırılan toplam tarama gösterir. Zaman aralığı, son 30 gün için varsayılan olarak ayarlanır. Ancak, son yedi gün ' yı da seçebilirsiniz. Zaman filtresine bağlı olarak, KPI değerleri tarama sayısını uygun şekilde yansıtır.


Seçilen zaman filtresi değerine bağlı olarak, başarılı, başarısız ve iptal edilmiş taramaların haftaya göre veya grafikteki güne göre dağıtımını görebilirsiniz.

   :::image type="content" source="./media/scan-insights/scan-insights.png" alt-text="Tarama öngörülerini görüntüleme":::

## <a name="next-steps"></a>Sonraki adımlar

[Varlık öngörüleri](./asset-insights.md) Ile Azure purview Insight Reports hakkında daha fazla bilgi edinin
