---
title: BD izlemesini kapsayıcı öngörüleri ile yapılandırma | Microsoft Docs
description: Bu makalede, kapsayıcı öngörüleri ile kalıcı birimler ile Kubernetes kümelerini izlemeyi nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: 0afbeab49a6909a0011cd75a0419f7325ca68132
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713737"
---
# <a name="configure-pv-monitoring-with-container-insights"></a>BD izlemesini kapsayıcı öngörüleri ile yapılandırma

*Ciprod10052020* aracı sürümü ile başlayarak, Container Insights tümleşik ARACıSı artık BD (kalıcı birim) kullanımını izlemeyi destekliyor.

## <a name="pv-metrics"></a>BD ölçümleri

Kapsayıcı öngörüleri, 60sec aralıklarında aşağıdaki ölçümleri toplayarak ve bunları **ınsijizmetrik** tablosunda depolayarak, otomatik olarak izlemeyi başlatır.

|Ölçüm adı |Ölçüm boyutu (Etiketler) |Açıklama |
|------------|------------------------|------------|
| `pvUsedBytes`|`container.azm.ms/pv`|Belirli bir pod tarafından kullanılan talebe sahip belirli bir kalıcı birim için bayt cinsinden kullanılan alan. `pvCapacityBytes` , veri alma maliyetini azaltmak ve sorguları basitleştirmek için Etiketler alanında bir boyut olarak ikiye katlanır.|

## <a name="monitor-persistent-volumes"></a>Kalıcı birimleri izleme

Kapsayıcı öngörüleri, her küme için bir çalışma kitabında bu ölçüm için önceden yapılandırılmış grafikler içerir. Sol bölmedeki çalışma kitaplarını seçerek ve Öngörüler içindeki **çalışma kitaplarını görüntüle** açılır listesinden grafikleri doğrudan bir aks kümesinden, **iş yükü ayrıntıları** çalışma kitabının kalıcı birim sekmesinde bulabilirsiniz. Ayrıca, BD kullanımı için önerilen uyarıyı etkinleştirebilir ve bu ölçümleri Log Analytics olarak sorgulayın.  

![Azure Izleyici BD iş yükü çalışma kitabı örneği](./media/container-insights-persistent-volumes/pv-workload-example.PNG)

## <a name="next-steps"></a>Sonraki adımlar

- Toplanan [BD ölçümleri hakkında](./container-insights-agent-config.md)daha fazla bilgi edinin.
