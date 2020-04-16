---
title: Monitör ardışık hattı Azure Synapse Studio çalışır (önizleme)
description: Çalışma alanı ardışık hat lar denetimini izlemek için Azure Synapse Studio'yu kullanın.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 712dc62e29229f03dec12afdf18edbf55667dbdf
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430792"
---
# <a name="use-azure-synapse-studio-to-monitor-your-workspace-pipeline-runs"></a>Çalışma alanı ardışık hat lar denetimini izlemek için Azure Synapse Studio'u kullanın

Azure Synapse Analytics ile, çözümünüzün içinde veri hareketinizi, veri dönüşümünüzü ve bilgi işlem etkinliklerinizi otomatikleştirebilen ve düzenleyebilen karmaşık ardışık hatlar oluşturabilir ve oluşturabilirsiniz. Bu ardışık hatları Azure Synapse Studio 'yı (önizleme) kullanarak yazar ve izleyebilirsiniz.

Bu makalede, ardışık hatlar ınızın en son durumunu, sorunlarını ve ilerlemelerini izlemenize olanak tanıyan ardışık işlem yapılarını nasıl izleyeceğiniz açıklanmaktadır.

## <a name="access-the-list-of-pipeline-runs"></a>Boru hattı çalıştırmaları listesine erişin

Çalışma alanınızda çalışan ardışık hatlar listesini görmek için önce [Azure Synapse Studio'yu açın](https://web.azuresynapse.net/) ve çalışma alanınızı seçin.

![Çalışma alanına giriş yapın](./media/common/login-workspace.png)

Çalışma alanınızı açtıktan sonra soldaki **Monitör** bölümünü seçin.

![Monitör merkezini seçin](./media/common/left-nav.png)

**Boru hattı çalıştırmaları** listesini görüntülemek için Pipeline çalışır'ı seçin.

![Boru hattı çalışanlarını seçin](./media/how-to-monitor-pipeline-runs/monitor-hub-nav-pipelineruns.png)

## <a name="filtering-your-pipeline-runs"></a>Boru hattı çalışanlarınızı filtreleme

Boru hattı çalışır listesini ilgilendiğiniz kişilere filtreleyebilirsiniz. Ekranın üst kısmındaki filtreler, filtrelemek istediğiniz bir alan belirtmenize olanak tanır.

Örneğin, yalnızca "tatil" adlı ardışık iş akdinin çalışanlarını görmek için görünümü filtreleyebilirsiniz:

![Filtre düğmesi](./media/common/filter-button.png)

![Örnek filtre](./media/how-to-monitor-pipeline-runs/filter-example.png)

## <a name="viewing-details-about-a-specific-pipeline-run"></a>Belirli bir boru hattı çalışmasıyla ilgili ayrıntıları görüntüleme

Ardışık hatlar hattı çalıştırınla ilgili ayrıntıları görüntülemek için, ardışık hatlar çalışmasını seçin. Ardından, ardışık hatlar çalışmasıyla ilişkili etkinlik çalışır görüntüle. Ardışık hatlar hala çalışıyorsa, ilerlemeyi izleyebilirsiniz. 
  
## <a name="next-steps"></a>Sonraki adımlar

Uygulamaları izleme hakkında daha fazla bilgi edinmek için [Monitor Apache Spark uygulamaları](how-to-monitor-spark-applications.md) makalesine bakın. 
