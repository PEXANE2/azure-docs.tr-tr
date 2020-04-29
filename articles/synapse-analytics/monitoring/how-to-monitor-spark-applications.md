---
title: Apache Spark uygulamalarını izleme
description: Apache Spark uygulamalarınızı izlemek için Azure SYNAPSE Studio 'Yu kullanın.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: f231693fdcf3519e29eed38e47db52d92acc00fa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81430753"
---
# <a name="use-the-azure-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Apache Spark uygulamalarınızı izlemek için Azure SYNAPSE Studio 'Yu (Önizleme) kullanın

Azure SYNAPSE Analytics ile, çalışma alanınızdaki Spark Havuzlarınızda not defterlerini, işleri ve diğer uygulama türlerini çalıştırmak için Spark 'ı kullanabilirsiniz.

Bu makalede Spark uygulamalarınızın nasıl izleneceği açıklanmakta ve en son durum, sorun ve ilerleme durumunu göz önünde tutmanız sağlanır.

## <a name="accessing-the-list-of-spark-applications"></a>Spark uygulamaları listesine erişme

Çalışma alanınızdaki Spark uygulamalarının listesini görmek için önce [Azure SYNAPSE Studio 'yu açın](https://web.azuresynapse.net/) ve çalışma alanınızı seçin.

  > [!div class="mx-imgBorder"]
  > ![Çalışma alanında oturum aç](./media/common/login-workspace.png)

Çalışma alanınızı açtıktan sonra, soldaki **izleyici** bölümünü seçin.

  > [!div class="mx-imgBorder"]
  > ![Izleme hub 'ı seçin](./media/common/left-nav.png)

Spark uygulamalarının listesini görüntülemek için **Spark uygulamaları** ' nı seçin.

  > [!div class="mx-imgBorder"]
  > ![Spark uygulamalarını seçin](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-spark-applications"></a>Spark uygulamalarınızın filtrelenmesi

Spark uygulamalarının listesini ilgilendiğiniz olanlarla filtreleyebilirsiniz. Ekranın üst kısmındaki filtreler, filtrelemek istediğiniz bir alan belirtmenize izin verir.

Örneğin, yalnızca "Sales" adını içeren Spark uygulamalarını görmek için görünümü filtreleyebilirsiniz:

  > [!div class="mx-imgBorder"]
  > ![Filtre düğmesi](./media/common/filter-button.png)

  > [!div class="mx-imgBorder"]
  > ![Örnek filtre](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-spark-application"></a>Belirli bir Spark uygulamasının ayrıntılarını görüntüleme

Spark uygulamalarınızdan biri hakkındaki ayrıntıları görüntülemek için Spark uygulamasını seçin ve ayrıntıları görüntüleyin. Spark uygulaması hala çalışıyorsa, ilerlemeyi izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

İşlem hattı çalıştırmalarını izleme hakkında daha fazla bilgi için bkz. [izleyici işlem hattı Azure SYNAPSE Studio 'yu çalıştırma](how-to-monitor-pipeline-runs.md) makalesi.  
