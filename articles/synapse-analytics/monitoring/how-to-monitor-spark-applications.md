---
title: Apache Spark uygulamalarını izleyin
description: Apache Spark uygulamalarınızı izlemek için Azure Synapse Studio'yu kullanın.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: f231693fdcf3519e29eed38e47db52d92acc00fa
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430753"
---
# <a name="use-the-azure-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Apache Spark uygulamalarınızı izlemek için Azure Synapse Studio'yu (önizleme) kullanın

Azure Synapse Analytics ile, çalışma alanınızda Spark havuzlarınızda dizüstü bilgisayarları, işleri ve diğer türuygulamaları çalıştırmak için Spark'ı kullanabilirsiniz.

Bu makalede, Spark uygulamalarınızı nasıl izleyeceğiniz açıklanır ve en son durumu, sorunları ve ilerlemeyi takip edebilirsiniz.

## <a name="accessing-the-list-of-spark-applications"></a>Kıvılcım uygulamaları listesine erişim

Çalışma alanınızdaki Kıvılcım uygulamalarının listesini görmek için önce [Azure Synapse Studio'yu açın](https://web.azuresynapse.net/) ve çalışma alanınızı seçin.

  > [!div class="mx-imgBorder"]
  > ![Çalışma alanına giriş yapın](./media/common/login-workspace.png)

Çalışma alanınızı açtıktan sonra soldaki **Monitör** bölümünü seçin.

  > [!div class="mx-imgBorder"]
  > ![Monitör merkezini seçin](./media/common/left-nav.png)

Kıvılcım uygulamaları listesini görüntülemek için **Spark uygulamalarını** seçin.

  > [!div class="mx-imgBorder"]
  > ![Spark uygulamalarını seçin](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-spark-applications"></a>Kıvılcım uygulamalarınızı filtreleme

Kıvılcım uygulamalarının listesini ilgilendiğiniz uygulamalara filtre leyebilirsiniz. Ekranın üst kısmındaki filtreler, filtrelemek istediğiniz bir alan belirtmenize olanak tanır.

Örneğin, görünümü yalnızca "satış" adını içeren Kıvılcım uygulamalarını görmek için filtreleyebilirsiniz:

  > [!div class="mx-imgBorder"]
  > ![Filtre düğmesi](./media/common/filter-button.png)

  > [!div class="mx-imgBorder"]
  > ![Örnek filtre](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-spark-application"></a>Belirli bir Kıvılcım uygulamasıyla ilgili ayrıntıları görüntüleme

Kıvılcım uygulamalarınızdan biriyle ilgili ayrıntıları görüntülemek için Spark uygulamasını seçin ve ayrıntıları görüntüleyin. Spark uygulaması hala çalışıyorsa, ilerlemeyi izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Denetim hattı çalıştırmaları hakkında daha fazla bilgi [için, Monitör ardışık hattının Azure Synapse Studio makalesini çalıştırışlarına](how-to-monitor-pipeline-runs.md) bakın.  
