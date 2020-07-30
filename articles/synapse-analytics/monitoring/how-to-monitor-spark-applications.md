---
title: SYNAPSE Studio 'da Apache Spark uygulamalarını izleme
description: Apache Spark uygulamalarınızı izlemek için SYNAPSE Studio kullanın.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: b5ad9e6a448747ac8f33829b1137b1af434e7a9c
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87385594"
---
# <a name="use-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Apache Spark uygulamalarınızı izlemek için SYNAPSE Studio 'Yu (Önizleme) kullanın

Azure SYNAPSE Analytics ile, çalışma alanınızdaki Spark Havuzlarınızda not defterlerini, işleri ve diğer uygulama türlerini çalıştırmak için Spark 'ı kullanabilirsiniz.

Bu makalede, Apache Spark uygulamalarınızın nasıl izleneceği açıklanmaktadır. Bu, en son durum, sorun ve ilerlemeyi göz önünde tutmanıza olanak sağlar.

## <a name="access-apache-spark-applications-list"></a>Apache Spark uygulamalar listesine erişin

Çalışma alanınızdaki Apache Spark uygulamalarının listesini görmek için önce [SYNAPSE Studio 'yu açın](https://web.azuresynapse.net/) ve çalışma alanınızı seçin.

![Çalışma alanında oturum aç](./media/common/login-workspace.png)

Çalışma alanınızı açtıktan sonra, soldaki **izleyici** bölümünü seçin.

![Izleme hub 'ı seçin](./media/common/left-nav.png)

Apache Spark uygulamalarının listesini görüntülemek için **Apache Spark uygulamalar** ' ı seçin.

 ![Spark uygulamalarını seçin](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filter-your-apache-spark-applications"></a>Apache Spark uygulamalarınızı filtreleyin

Apache Spark uygulamalarının listesini ilgilendiğiniz olanlarla filtreleyebilirsiniz. Ekranın üst kısmındaki filtreler, filtrelemek istediğiniz bir alan belirtmenize izin verir.

Örneğin, yalnızca "Sales" adını içeren Apache Spark uygulamalarını görmek için görünümü filtreleyebilirsiniz:

![Filtre düğmesi](./media/common/filter-button.png)

![Örnek filtre](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="view-details-about-a-specific-apache-spark-application"></a>Belirli bir Apache Spark uygulamayla ilgili ayrıntıları görüntüleme

Apache Spark uygulamalarınızdan biri hakkındaki ayrıntıları görüntülemek için Apache Spark uygulamasını seçin ve ayrıntıları görüntüleyin. Apache Spark uygulama hala çalışıyorsa, ilerlemeyi izleyebilirsiniz. [Daha fazla bilgi edinin](apache-spark-applications.md).

## <a name="next-steps"></a>Sonraki adımlar

İşlem hattı çalıştırmalarını izleme hakkında daha fazla bilgi için bkz. işlem [hattı çalıştırmalarını Izleme SYNAPSE Studio](how-to-monitor-pipeline-runs.md) makalesi. 

Apache Spark uygulamada hata ayıklama hakkında daha fazla bilgi için bkz. [SYNAPSE Studio 'da uygulamaları izleme Apache Spark](apache-spark-applications.md) .