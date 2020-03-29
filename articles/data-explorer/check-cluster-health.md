---
title: Azure Veri Gezgini kümesinin sistem durumunu denetleme
description: Bu makalede, Azure Veri Gezgini kümenizin durumunu izlemek için adımlar açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a280d8869a3790444a97c38f792a3d9eeb6bde1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60861327"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinin sistem durumunu denetleme

CPU, bellek ve disk alt sistemi gibi bir Azure Veri Gezgini kümesinin sistem durumunu etkileyen çeşitli etkenler vardır. Bu makalede, bir kümenin durumunu ölçmek için atabileceğiniz bazı temel adımlar gösterilmektedir.

1. Oturum [https://dataexplorer.azure.com](https://dataexplorer.azure.com)aç.

1. Sol bölmede, kümenizi seçin ve aşağıdaki komutu çalıştırın.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    1'in çıktısı sağlıklıdır; 0 çıktısı sağlıksız.

1. [Azure portalında](https://portal.azure.com)oturum açın ve kümenize gidin.

1. **İzleme**altında, **Ölçümler'i**seçin, ardından aşağıdaki resimde gösterildiği gibi **Canlı Tutun'u**seçin. 1'e yakın bir çıkış, sağlıklı bir küme anlamına gelir.

    ![Küme Canlı Tutun metrik](media/check-cluster-health/portal-metrics.png)

1. Grafiğe başka ölçümler eklemek mümkündür. Grafiği seçin ve **ardından metrik ekle.** Başka bir metrik seçin - bu örnek **CPU**gösterir.

    ![Ölçüm ekleme](media/check-cluster-health/add-metric.png)

1. Bir kümenin durumuyla ilgili sorunları tanılama konusunda yardıma ihtiyacınız varsa, lütfen [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)bir destek isteği açın.