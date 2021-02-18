---
title: Azure ölçümleri Gezgininde birden çok kaynağı görüntüleme
description: Azure ölçümleri gezginini kullanarak birden çok kaynağı görselleştirmeyi öğrenin.
author: ritaroloff
services: azure-monitor
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: riroloff
ms.subservice: metrics
ms.openlocfilehash: 176acea34dead929986acb807b465593f9ffda5a
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100622328"
---
# <a name="view-multiple-resources-in-the-azure-metrics-explorer"></a>Azure ölçümleri Gezgininde birden çok kaynağı görüntüleme

Kaynak kapsamı seçici, aynı abonelik ve bölge içindeki birden fazla kaynak arasında ölçümleri görüntülemenize olanak sağlar. Bu makalede, Azure Izleyici 'nin Azure ölçümleri gezgin özelliğini kullanarak birden çok kaynağın nasıl görüntüleneceği açıklanır. 

## <a name="select-a-resource"></a>Bir kaynak seçin 

**Azure izleyici** menüsünde veya bir kaynağın menüsünün **izleme** bölümünden **ölçümler** ' i seçin. Ardından kapsam seçiciyi açmak için **kapsam seçin** öğesini seçin. 

Ölçümlerini görmek istediğiniz kaynakları seçmek için kapsam seçiciyi kullanın. Ölçüm Gezginini bir kaynağın menüsünden açtıysanız kapsam doldurulmalıdır. 

![Kaynak kapsamı seçicinin nasıl açılacağını gösteren ekran görüntüsü.](./media/metrics-dynamic-scope/019.png)

## <a name="select-multiple-resources"></a>Birden çok kaynak seçin 

Bazı kaynak türleri, birden fazla kaynak üzerinde ölçümleri sorgulayabilir. Ölçümler aynı abonelik ve konum dahilinde olmalıdır. **Kaynak türleri** menüsünün en üstünde bu kaynak türlerini bulun.

![Birden çok kaynakla uyumlu kaynakların bir menüsünü gösteren ekran görüntüsü.](./media/metrics-dynamic-scope/020.png)

> [!WARNING] 
> Birden çok kaynakta, kaynak grubunda veya abonelikte ölçümleri görselleştirmek için abonelik düzeyinde Izleme okuyucusu izninizin olması gerekir. Daha fazla bilgi için, bkz. [Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](../../role-based-access-control/role-assignments-portal.md).

Birden çok kaynak üzerinde ölçümleri görselleştirmek için kaynak kapsamı seçicisinden birden çok kaynak seçerek başlayın. 

![Birden çok kaynağın nasıl seçdiğinin gösterildiği ekran görüntüsü.](./media/metrics-dynamic-scope/021.png)

> [!NOTE]
> Seçtiğiniz kaynaklar aynı kaynak türü, konum ve abonelik içinde olmalıdır. Bu ölçütlere uymayan kaynaklar seçilebilir değildir. 

Bitirdiğinizde Seçimlerinizi kaydetmek için **Uygula** ' yı seçin. 

## <a name="select-a-resource-group-or-subscription"></a>Kaynak grubu veya abonelik seçin 

> [!WARNING]
> Birden çok kaynakta, kaynak grubunda veya abonelikte ölçümleri görselleştirmek için abonelik düzeyinde Izleme okuyucusu izninizin olması gerekir. 

Birden çok kaynakla uyumlu olan türler için bir abonelik veya birden çok kaynak grubu genelinde ölçümleri sorgulayabilirsiniz. Bir abonelik veya bir veya daha fazla kaynak grubu seçerek başlayın: 

![Birden çok kaynak grubunda nasıl sorgu yapılacağını gösteren ekran görüntüsü.](./media/metrics-dynamic-scope/022.png)

Kaynak türü ve konum seçin. 

![Seçili kaynak gruplarını gösteren ekran görüntüsü.](./media/metrics-dynamic-scope/023.png)

Seçimlerinizin uygulandığı kaynakları doğrulamak için seçilen kapsamları genişletebilirsiniz.

![Gruplar içindeki seçili kaynakları gösteren ekran görüntüsü.](./media/metrics-dynamic-scope/024.png)

Kapsamları seçmeyi tamamladığınızda **Uygula**' yı seçin. 

## <a name="split-and-filter-by-resource-group-or-resources"></a>Kaynak grubu veya kaynaklara göre bölme ve filtreleme

Kaynaklarınızı doldurduktan sonra, verilerinize daha fazla öngörü kazanmak için bölme ve filtrelemeyi kullanabilirsiniz. 

Bölme, ölçümün farklı segmentlerinin birbirleriyle nasıl karşılaştırılacağını görselleştirmenize olanak tanır. Örneğin, birden çok kaynak için bir ölçüm çizerseniz, kaynak KIMLIĞINE veya kaynak grubuna göre bölmek için **bölmeyi Uygula** ' yı seçebilirsiniz. Bölünmüş, tek bir ölçümü birden çok kaynak veya kaynak grubu genelinde karşılaştırmanıza olanak sağlar.  

Örneğin, aşağıdaki grafik dokuz VM arasında CPU yüzdesini gösterir. Kaynak KIMLIĞINE göre böldüğünüz zaman CPU 'nun VM tarafından ne kadar farklı olduğunu görürsünüz. 

![VM 'lerde CPU yüzdesini görmek için bölmenin nasıl kullanılacağını gösteren ekran görüntüsü.](./media/metrics-dynamic-scope/026.png)

Bölme ile birlikte, yalnızca görmek istediğiniz kaynak gruplarını görüntülemek için filtrelemeyi kullanabilirsiniz.  Örneğin, belirli bir kaynak grubuna yönelik VM 'Lerin CPU yüzdesini görüntülemek için, kaynak grubuna göre filtrelemek için **Filtre Ekle** ' yi seçebilirsiniz. 

Bu örnekte, bir Uyarspintoysdemo tarafından filtreliyoruz. Burada, filtre,,,,,, bir,,,,,,,, 

![Kaynak grubuna göre nasıl filtreleneceğini gösteren ekran görüntüsü.](./media/metrics-dynamic-scope/027.png)

## <a name="pin-multiple-resource-charts"></a>Birden çok Kaynak Grafiği sabitleme 

Kaynak grupları ve abonelikler arasında ölçümleri görselleştirerek çoklu kaynak grafikleri, kullanıcının abonelik düzeyinde *Izleme okuyucusu* iznine sahip olmasını gerektirir. Birden çok kaynak grafiği sabitlemeyi planladığınız panolar için tüm kullanıcıların yeterli izinlere sahip olduğundan emin olun. Daha fazla bilgi için, bkz. [Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](../../role-based-access-control/role-assignments-portal.md).

Çoklu kaynak grafiğinizi bir panoya sabitlemek için bkz. [panolara sabitleme](../essentials/metrics-charts.md#pinning-to-dashboards). 

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçüm Gezgini sorunlarını giderme](../essentials/metrics-troubleshoot.md)
* [Azure hizmetleri için kullanılabilir ölçümlerin listesine bakın](../platform/metrics-supported.md)
* [Yapılandırılmış grafik örneklerine bakın](../essentials/metric-chart-samples.md)