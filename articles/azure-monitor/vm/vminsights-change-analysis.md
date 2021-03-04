---
title: VM öngörülerinin değişiklik Analizi
description: Uygulama değişikliği Analizi tümleştirmesi ile VM öngörüleri tümleştirmesi, bir sanal makinede, BT performansını etkileyebilecek değişiklikleri görüntülemenize olanak sağlar.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/23/2020
ms.openlocfilehash: 375812813d704eb9b48d0ed8fbbc65dd5e47da49
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102046780"
---
# <a name="change-analysis-in-vm-insights"></a>VM öngörülerinin değişiklik Analizi
[Uygulama değişikliği Analizi](../app/change-analysis.md) TÜMLEŞTIRMESI ile VM öngörüleri tümleştirmesi, bir sanal makinede, BT performansını etkileyebilecek değişiklikleri görüntülemenize olanak sağlar.

## <a name="overview"></a>Genel Bakış
Yavaş çalışan bir sanal makinenizin olduğunu ve yapılandırmasındaki son değişikliklerin performansını etkileip etkilemediğini araştırmak istediğinizi varsayalım. VM öngörülerini kullanarak VM 'nin performansını görüntüleyebilir ve Son saatteki bellek kullanımında bir artış olduğunu bulabilirsiniz. Değişiklik analizi, bu sürenin sonunda yapılan herhangi bir yapılandırma değişikliğinin bu artışın nedeni olup olmadığını belirlemenize yardımcı olabilir.

Uygulama değişikliği analiz hizmeti, [Azure Kaynak grafının](../../governance/resource-graph/how-to/get-resource-changes.md) yanı sıra Azure Resource Manager ağ güvenliği kuralları gibi iç içe geçmiş Özellikler değişikliklerini toplar. 

## <a name="enabling-change-analysis"></a>Değişiklik analizini etkinleştirme
VM öngörülerine değişiklik Analizi eklemek için *Microsoft. ChangeAnalysis* kaynak sağlayıcısını kaydetmeniz gerekir. Azure portal VM öngörülerini veya uygulama değişikliği analizini ilk kez başlattığınızda, bu kaynak sağlayıcı sizin için otomatik olarak kaydedilir. Uygulama değişikliği analizi, kaynaklarda performans yükü olmayan ücretsiz bir hizmettir.

## <a name="view-change-analysis"></a>Değişiklik analizini görüntüle
Değişiklik analizi, sanal makine öngörülerinin **performans** veya **eşleme** sekmesinden **değişiklik** seçeneği belirlenerek kullanılabilir. 

[![Değişiklikleri araştır](media/vminsights-change-analysis/investigate-changes-screenshot.png)](media/vminsights-change-analysis/investigate-changes-screenshot-zoom.png#lightbox)


VM için filtrelenmiş uygulama değişikliği Analizi sayfasını başlatmak için **değişiklikleri araştır** düğmesine tıklayın. Soruna neden olmuş olabilecek bir sorun olup olmadığını görmek için listelenen değişiklikleri gözden geçirebilirsiniz. Belirli bir değişikliği bilmiyorsanız, değişikliği yapan kişiyi belirleyebilmek için **değişikliğe göre değişiklik** yapabilirsiniz.

[![Değişiklik ayrıntıları](media/vminsights-change-analysis/change-details-screenshot.png)](media/vminsights-change-analysis/change-details-screenshot.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar
- [Uygulama değişikliği Analizi](../app/change-analysis.md)hakkında daha fazla bilgi edinin.
- [Azure Kaynak Grafiği](../../governance/resource-graph/how-to/get-resource-changes.md)hakkında daha fazla bilgi edinin. 

