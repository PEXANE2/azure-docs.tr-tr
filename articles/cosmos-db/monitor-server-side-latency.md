---
title: Azure Cosmos DB işlemler için sunucu tarafı gecikme süresini izleme
description: Azure Cosmos DB hesabındaki veya bir kapsayıcıdaki işlemler için sunucu gecikmesini nasıl izleyeceğinizi öğrenin. Azure Cosmos DB hesabının sahipleri, Azure Cosmos hesaplarınızla sunucu tarafı gecikme sorunlarını anlayabilir.
ms.service: cosmos-db
ms.topic: how-to
author: kanshiG
ms.author: govindk
ms.date: 04/07/2020
ms.openlocfilehash: ec6a9db63504958640137fcd0fcfc904eb01afa5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93074738"
---
# <a name="how-to-monitor-the-server-side-latency-for-operations-in-an-azure-cosmos-db-container-or-account"></a>Bir Azure Cosmos DB kapsayıcısı veya hesabındaki işlemler için sunucu tarafı gecikme süresini izleme
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB için Azure Izleyici, hesabınızı izlemek ve panolar oluşturmak için bir ölçüm görünümü sağlar. Azure Cosmos DB ölçümleri varsayılan olarak toplanır, bu özellik açıkça her şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez. Sunucu tarafı gecikme ölçümü, bir işlemin sunucu tarafı gecikme süresini görüntülemek için kullanılır. Azure Cosmos DB, doğrudan bağlantı ile nokta okuma/yazma işlemleri için 10 MS 'den daha az SLA sağlar. Nokta okuma ve yazma işlemleri için [SLA 'lar belgesinde](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/)ayrıntılı olarak hesaplanır.

Nokta işlemi için olağandışı yüksek gecikme süresi görürseniz, sunucu tarafı gecikmesini izleyebilirsiniz:

* Doğrudan bağlantı modundaki bölüm anahtarı ve KIMLIKLI bir GET veya SET işlemi
* Bir okuma veya yazma işlemi veya
* Bir sorgu

Döndürülen verilerin boyutunu görmek için tanılama günlüğüne bakabilirsiniz. Sorgu işlemleri için sürekli yüksek gecikme görürseniz, daha yüksek [Aktarım hızı veya ru/sn](cosmosdb-monitor-resource-logs.md#diagnostic-queries) için tanılama günlüğüne bakmalısınız. Sunucu tarafı gecikmesi, veriler istemciye döndürülmeden önce arka uç altyapısında harcanan süreyi gösterir. Her arka uç gecikme sorunlarını gidermek için bu ölçüme bakmak önemlidir.

## <a name="view-the-server-side-latency-metric"></a>Sunucu tarafı gecikme ölçüsünü görüntüleme

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **ölçümler**' i seçin.

   :::image type="content" source="./media/monitor-server-side-latency/monitor-metrics-blade.png" alt-text="Azure Izleyici 'de ölçümler bölmesi":::

1. **Ölçümler** bölmesinden > **bir kaynak seçin** > gerekli **aboneliği** ve **kaynak grubunu** seçin. **Kaynak türü** için **Azure Cosmos DB hesapları**' nı seçin, mevcut Azure Cosmos hesaplarınızdan birini seçin ve **Uygula**' yı seçin.
   
   :::image type="content" source="./media/monitor-server-side-latency/select-cosmos-db-account.png" alt-text="Ölçümleri görüntülemek için Azure Cosmos DB hesabı seçin":::

1. Ardından, kullanılabilir ölçümler listesinden **sunucu tarafı gecikme süresi**  ölçüsünü seçin. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için [kategoriye göre ölçümler](monitor-cosmos-db-reference.md) makalesine bakın. Bu örnekte, toplama değeri olarak **sunucu tarafı gecikme süresi** ve **Ort** ' i seçelim. Bu ayrıntılara ek olarak, ölçümlerin **zaman aralığını** ve **zaman parçalı yapısını** da seçebilirsiniz. En fazla, son 30 güne ait ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra filtreniz temelinde bir grafik görüntülenir. Seçili dönem için dakika başına sunucu tarafı gecikme süresini görebilirsiniz.  

   :::image type="content" source="./media/monitor-server-side-latency/server-side-latency-metric.png" alt-text="Azure portal Server-Side gecikme ölçümünü seçin":::

## <a name="filters-for-server-side-latency"></a>Sunucu tarafı gecikme süresi için filtreler

Ayrıca ölçümleri filtreleyebilir ve belirli bir **CollectionName**, **connectionmode**, **DatabaseName**, **OperationType**, **Region** ve **publicapıtype** tarafından görünen grafikleri alabilirsiniz. 

Ölçümleri filtrelemek için, **Filtre Ekle** ' yi seçin ve **Publicapittype** gibi gerekli özelliği seçin ve **SQL** değerini seçin. **OperationType** için başka bir filtre ekleyin. Daha sonra grafik, seçilen dönemde farklı işlemler için sunucu tarafı gecikme süresini görüntüler. Saklı yordam aracılığıyla yürütülen işlemler, OperationType ölçümü altında kullanılamayacak şekilde günlüğe kaydedilmez.

Her bir işlemin **sunucu tarafı gecikme süresi** ölçümleri, aşağıdaki görüntüde gösterildiği gibi görüntülenir:

:::image type="content" source="./media/monitor-server-side-latency/server-side-latency-filters.png" alt-text="Sunucu tarafı gecikme ölçümleri için filtreler":::

Ayrıca, **bölmeyi Uygula** seçeneğini kullanarak ölçümleri gruplandırabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

* Azure 'da [tanılama ayarlarını](cosmosdb-monitor-resource-logs.md) kullanarak Azure Cosmos DB verileri izleyin.
* [Denetim düzlemi işlemlerini denetim Azure Cosmos DB](audit-control-plane-logs.md)
