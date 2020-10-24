---
title: Kapsayıcılar için Azure Izleyici ile BD izlemesini yapılandırma | Microsoft Docs
description: Bu makalede, kapsayıcıların Azure Izleyici ile kalıcı birimlerle izleme Kubernetes kümelerini nasıl yapılandırabileceğiniz açıklanmaktadır.
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: 30e99c2abbc66de257f9623dedc901fca51976c1
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92492206"
---
# <a name="configure-pv-monitoring-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici ile BD izlemeyi yapılandırma

Aracı sürümü *ciprod10052020*ile başlayarak, kapsayıcılar için Azure izleyici tümleşik aracı artık BD (kalıcı birim) kullanımını izlemeyi destekliyor.

## <a name="pv-metrics"></a>BD ölçümleri

Kapsayıcılar için Azure Izleyici, aşağıdaki ölçümleri 60sec aralıklarında toplayarak ve bunları **ınsi\ölçüm** tablosunda depolayarak, otomatik olarak izlemeyi başlatır.

|Ölçüm adı |Ölçüm boyutu (Etiketler) |Açıklama |
|------------|------------------------|------------|
| `pvUsedBytes`|`container.azm.ms/pv`|Belirli bir pod tarafından kullanılan talebe sahip belirli bir kalıcı birim için bayt cinsinden kullanılan alan. `pvCapacityBytes` , veri alma maliyetini azaltmak ve sorguları basitleştirmek için Etiketler alanında bir boyut olarak ikiye katlanır.|

## <a name="monitor-persistent-volumes"></a>Kalıcı birimleri izleme

Kapsayıcılar için Azure Izleyici, her küme için bir çalışma kitabında bu ölçüm için önceden yapılandırılmış grafikler içerir. Sol bölmedeki çalışma kitaplarını seçerek ve Öngörüler içindeki **çalışma kitaplarını görüntüle** açılır listesinden grafikleri doğrudan bir aks kümesinden, **iş yükü ayrıntıları** çalışma kitabının kalıcı birim sekmesinde bulabilirsiniz. Ayrıca, BD kullanımı için önerilen uyarıyı etkinleştirebilir ve bu ölçümleri Log Analytics olarak sorgulayın.  

![Azure Izleyici BD iş yükü çalışma kitabı örneği](./media/container-insights-persistent-volumes/pv-workload-example.PNG)

## <a name="next-steps"></a>Sonraki adımlar

- Toplanan [BD ölçümleri hakkında](https://aka.ms/ci/pvconfig)daha fazla bilgi edinin.
