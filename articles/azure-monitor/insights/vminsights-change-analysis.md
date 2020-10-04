---
title: VM'ler için Azure İzleyici analiz değişikliği
description: Uygulama değişiklik Analizi tümleştirmesiyle VM'ler için Azure İzleyici tümleştirme, bir sanal makinede, BT performansını etkileyebilecek değişiklikleri görüntülemenize olanak sağlar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/23/2020
ms.openlocfilehash: 59799a09436d5968a441f6f17655d3138a2d84d8
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91711661"
---
# <a name="change-analysis-in-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici analiz değişikliği
[Uygulama değişiklik Analizi](../app/change-analysis.md) tümleştirmesiyle VM'ler için Azure izleyici tümleştirme, bir sanal makinede, BT performansını etkileyebilecek değişiklikleri görüntülemenize olanak sağlar.

## <a name="overview"></a>Genel Bakış
Yavaş çalışan bir sanal makinenizin olduğunu ve yapılandırmasındaki son değişikliklerin performansını etkileip etkilemediğini araştırmak istediğinizi varsayalım. VM'ler için Azure İzleyici kullanarak VM 'nin performansını görüntüleyebilir ve Son saatteki bellek kullanımında bir artış olduğunu bulabilirsiniz. Değişiklik analizi, bu sürenin sonunda yapılan herhangi bir yapılandırma değişikliğinin bu artışın nedeni olup olmadığını belirlemenize yardımcı olabilir.

Uygulama değişikliği analiz hizmeti, [Azure Kaynak grafının](../../governance/resource-graph/how-to/get-resource-changes.md) yanı sıra Azure Resource Manager ağ güvenliği kuralları gibi iç içe geçmiş Özellikler değişikliklerini toplar. 

## <a name="enabling-change-analysis"></a>Değişiklik analizini etkinleştirme
VM'ler için Azure İzleyici değişiklik analizini eklemek için, *Microsoft. changeanalysis* kaynak sağlayıcısını kaydetmeniz gerekir. Azure portal VM'ler için Azure İzleyici veya uygulama değişikliği analizini ilk kez başlattığınızda, bu kaynak sağlayıcı sizin için otomatik olarak kaydedilir. Uygulama değişikliği analizi, kaynaklarda performans yükü olmayan ücretsiz bir hizmettir.

## <a name="view-change-analysis"></a>Değişiklik analizini görüntüle
Değişiklik analizi, VM'ler için Azure İzleyici **değişiklik** seçeneği belirlenerek **performans** veya **eşleme** sekmesinden kullanılabilir. 

[![Değişiklikleri araştır](media/vminsights-change-analysis/investigate-changes-screenshot.png)](media/vminsights-change-analysis/investigate-changes-screenshot-zoom.png#lightbox)


VM için filtrelenmiş uygulama değişikliği Analizi sayfasını başlatmak için **değişiklikleri araştır** düğmesine tıklayın. Soruna neden olmuş olabilecek bir sorun olup olmadığını görmek için listelenen değişiklikleri gözden geçirebilirsiniz. Belirli bir değişikliği bilmiyorsanız, değişikliği yapan kişiyi belirleyebilmek için **değişikliğe göre değişiklik** yapabilirsiniz.

[![Değişiklik ayrıntıları](media/vminsights-change-analysis/change-details-screenshot.png)](media/vminsights-change-analysis/change-details-screenshot.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar
- [Uygulama değişikliği Analizi](../app/change-analysis.md)hakkında daha fazla bilgi edinin.
- [Azure Kaynak Grafiği](../../governance/resource-graph/how-to/get-resource-changes.md)hakkında daha fazla bilgi edinin. 

