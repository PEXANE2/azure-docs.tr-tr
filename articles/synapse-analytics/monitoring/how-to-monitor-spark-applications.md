---
title: Apache Spark uygulamalarını Izleme
description: Apache Spark uygulamalarınızı izlemek için Azure SYNAPSE Studio 'Yu kullanın.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: e646f1bc1a25f58dd54437cbd77750479ac9ae99
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970917"
---
# <a name="use-the-azure-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Apache Spark uygulamalarınızı izlemek için Azure SYNAPSE Studio 'Yu (Önizleme) kullanın

Azure SYNAPSE Analytics ile, çalışma alanınızdaki Spark Havuzlarınızda not defterlerini, işleri ve diğer uygulama türlerini çalıştırmak için Spark 'ı kullanabilirsiniz.

Bu makalede, Apache Spark uygulamalarınızın nasıl izleneceği açıklanmaktadır. Bu, en son durum, sorun ve ilerlemeyi göz önünde tutmanıza olanak sağlar.

## <a name="accessing-the-list-of-apache-spark-applications"></a>Apache Spark uygulamalar listesine erişme

Çalışma alanınızdaki Apache Spark uygulamalarının listesini görmek için önce [Azure SYNAPSE Studio 'yu açın](https://web.azuresynapse.net/) ve çalışma alanınızı seçin.

![Çalışma alanında oturum aç](./media/common/login-workspace.png)

Çalışma alanınızı açtıktan sonra, soldaki **izleyici** bölümünü seçin.

![Izleme hub 'ı seçin](./media/common/left-nav.png)

Apache Spark uygulamalarının listesini görüntülemek için **Apache Spark uygulamalar** ' ı seçin.

 ![Spark uygulamalarını seçin](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-apache-spark-applications"></a>Apache Spark uygulamalarınızın filtrelenmesi

Apache Spark uygulamalarının listesini ilgilendiğiniz olanlarla filtreleyebilirsiniz. Ekranın üst kısmındaki filtreler, filtrelemek istediğiniz bir alan belirtmenize izin verir.

Örneğin, yalnızca "Sales" adını içeren Apache Spark uygulamalarını görmek için görünümü filtreleyebilirsiniz:

![Filtre düğmesi](./media/common/filter-button.png)

![Örnek filtre](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-apache-spark-application"></a>Belirli bir Apache Spark uygulamayla ilgili ayrıntıları görüntüleme

Apache Spark uygulamalarınızdan biri hakkındaki ayrıntıları görüntülemek için Apache Spark uygulamasını seçin ve ayrıntıları görüntüleyin. Apache Spark uygulama hala çalışıyorsa, ilerlemeyi izleyebilirsiniz. [Daha fazla bilgi edinin](apache-spark-applications.md).

## <a name="next-steps"></a>Sonraki adımlar

İşlem hattı çalıştırmalarını izleme hakkında daha fazla bilgi için bkz. [izleyici işlem hattı Azure SYNAPSE Studio 'yu çalıştırma](how-to-monitor-pipeline-runs.md) makalesi. 

Apache Spark uygulamasında hata ayıklama hakkında daha fazla bilgi için bkz. [Azure 'da Apache Spark uygulamaları Izleme SYNAPSE Studio](apache-spark-applications.md) makalesi.