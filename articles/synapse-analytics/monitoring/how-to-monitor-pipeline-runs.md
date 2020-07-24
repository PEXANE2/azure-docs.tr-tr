---
title: SYNAPSE Studio kullanarak işlem hattı çalıştırmalarını izleme
description: Çalışma alanı işlem hattı çalıştırmalarını izlemek için SYNAPSE Studio 'Yu kullanın.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 385b06dd7756c3bebf6557f9ea0b518473a37d49
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87075898"
---
# <a name="use-synapse-studio-to-monitor-your-workspace-pipeline-runs"></a>Çalışma alanı işlem hattı çalıştırmalarını izlemek için SYNAPSE Studio 'Yu kullanma

Azure SYNAPSE Analytics ile çözümünüzdeki veri taşıma, veri dönüştürme ve işlem etkinliklerini otomatikleştirebilen ve düzenleyen karmaşık işlem hatları oluşturabilirsiniz. Bu işlem hatlarını SYNAPSE Studio (Önizleme) kullanarak yazabilir ve izleyebilirsiniz.

Bu makalede, işlem hatlarınızın en son durumunu, sorunlarını ve ilerlemesini görmenizi sağlayan işlem hattı çalıştırmalarınızın nasıl izleneceği açıklanır.

## <a name="access-the-list-of-pipeline-runs"></a>İşlem hattı çalıştırmaları listesine erişin

Çalışma alanınızdaki işlem hattı çalıştırmalarının listesini görmek için önce [SYNAPSE Studio 'yu açın](https://web.azuresynapse.net/) ve çalışma alanınızı seçin.

![Çalışma alanında oturum aç](./media/common/login-workspace.png)

Çalışma alanınızı açtıktan sonra, soldaki **izleyici** bölümünü seçin.

![Izleme hub 'ı seçin](./media/common/left-nav.png)

İşlem hattı çalıştırmaları listesini görüntülemek için işlem **hattı çalıştırmaları** ' nı seçin.

![İşlem hattı çalıştırmalarını seçin](./media/how-to-monitor-pipeline-runs/monitor-hub-nav-pipelineruns.png)

## <a name="filtering-your-pipeline-runs"></a>İşlem hattı çalıştırmalarınız filtreleniyor

İşlem hattı çalıştırmalarının listesini ilgilendiğiniz olanlarla filtreleyebilirsiniz. Ekranın üst kısmındaki filtreler, filtrelemek istediğiniz bir alan belirtmenize izin verir.

Örneğin, "tatil" adlı işlem hattı için yalnızca işlem hattı çalıştırmalarını görmek üzere görünümü filtreleyebilirsiniz:

![Filtre düğmesi](./media/common/filter-button.png)

![Örnek filtre](./media/how-to-monitor-pipeline-runs/filter-example.png)

## <a name="viewing-details-about-a-specific-pipeline-run"></a>Belirli bir işlem hattı çalıştırmasıyla ilgili ayrıntıları görüntüleme

İşlem hattı çalıştırınızla ilgili ayrıntıları görüntülemek için işlem hattı çalıştırmasını seçin. Sonra işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntüleyin. İşlem hattı çalışmaya devam ediyorsa, ilerlemeyi izleyebilirsiniz. 
  
## <a name="next-steps"></a>Sonraki adımlar

Uygulamaları izleme hakkında daha fazla bilgi edinmek için [Apache Spark uygulamaları izleme](how-to-monitor-spark-applications.md) makalesine bakın. 
