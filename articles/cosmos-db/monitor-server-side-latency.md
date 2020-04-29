---
title: Azure Cosmos DB işlemler için sunucu tarafı gecikme süresini izleme
description: Azure Cosmos DB hesabındaki veya bir kapsayıcıdaki işlemler için sunucu gecikmesini nasıl izleyeceğinizi öğrenin. Azure Cosmos DB hesabının sahipleri, Azure Cosmos hesaplarınızla sunucu tarafı gecikme sorunlarını anlayabilir.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 04/07/2020
ms.openlocfilehash: 0f1e6d07afb3b7b4d26081bc9e34ac257b280d0f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81113918"
---
# <a name="how-to-monitor-the-server-side-latency-for-operations-in-an-azure-cosmos-db-container-or-account"></a>Bir Azure Cosmos DB kapsayıcısı veya hesabındaki işlemler için sunucu tarafı gecikme süresini izleme

Azure Cosmos DB için Azure Izleyici, hesabınızı izlemek ve panolar oluşturmak için bir ölçüm görünümü sağlar. Azure Cosmos DB ölçümleri varsayılan olarak toplanır, bu özellik açıkça her şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez. Sunucu tarafı gecikme ölçümü, bir işlemin sunucu tarafı gecikme süresini görüntülemek için kullanılır. Azure Cosmos DB, doğrudan bağlantı ile nokta okuma/yazma işlemleri için 10 MS 'den daha az SLA sağlar. Nokta okuma ve yazma işlemleri için [SLA 'lar belgesinde](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/)ayrıntılı olarak hesaplanır.

Nokta işlemleri için olağandışı büyük gecikme süresi görürseniz:

* Doğrudan modda bölüm anahtarı ve KIMLIĞI olan bir get veya set işlemi
* Bir okuma veya yazma işlemi veya
* Bir sorgu

Döndürülen verilerin boyutunu görmek için tanılama günlüğüne bakabilirsiniz. Sorgu işlemleri için sürekli yüksek gecikme görürseniz, tanılama günlüğünü döndürülen verilerin boyutu, [üretilen iş veya ru/sn](cosmosdb-monitor-resource-logs.md#diagnostic-queries) veya belirli bir dönemdeki işlem sayısı için arayabilirsiniz. Bu şekilde, sunucu tarafı gecikme sorunlarını ayıklayabilirsiniz.

## <a name="view-the-server-side-latency-metric"></a>Sunucu tarafı gecikme ölçüsünü görüntüleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **ölçümler**' i seçin.

   ![Azure Izleyici 'de ölçümler bölmesi](./media/monitor-server-side-latency/monitor-metrics-blade.png)

1. **Ölçümler** bölmesinden > **bir kaynak seçin** > gerekli **aboneliği**ve **kaynak grubunu**seçin. **Kaynak türü**için **Azure Cosmos DB hesapları**' nı seçin, mevcut Azure Cosmos hesaplarınızdan birini seçin ve **Uygula**' yı seçin.
   
   ![Ölçümleri görüntülemek için Azure Cosmos DB hesabı seçin](./media/monitor-server-side-latency/select-cosmos-db-account.png)

1. Ardından, kullanılabilir ölçümler listesinden **sunucu tarafı gecikme süresi** ölçüsünü seçin. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için [kategoriye göre ölçümler](monitor-cosmos-db-reference.md) makalesine bakın. Bu örnekte, toplama değeri olarak **sunucu tarafı gecikme süresi** ve **Ort** ' i seçelim. Bu ayrıntılara ek olarak, ölçümlerin **zaman aralığını** ve **zaman parçalı yapısını** da seçebilirsiniz. En fazla, son 30 güne ait ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra filtreniz temelinde bir grafik görüntülenir. Seçili dönem için dakika başına sunucu tarafı gecikme süresini görebilirsiniz.  

   ![Azure portal sunucu tarafı gecikme ölçümünü seçin](./media/monitor-server-side-latency/server-side-latency-metric.png)

## <a name="filters-for-server-side-latency"></a>Sunucu tarafı gecikme süresi için filtreler

Ayrıca ölçümleri filtreleyebilir ve belirli bir **CollectionName**, **connectionmode**, **DatabaseName**, **OperationType**, **Region**ve **publicapıtype**tarafından görünen grafikleri alabilirsiniz. 

Ölçümleri filtrelemek için, **Filtre Ekle** ' yi seçin ve **Publicapittype** gibi gerekli özelliği seçin ve **SQL**değerini seçin. **OperationType**için başka bir filtre ekleyin. Daha sonra grafik, seçilen dönemde farklı işlemler için sunucu tarafı gecikme süresini görüntüler. Saklı yordam aracılığıyla yürütülen işlemler, OperationType ölçümü altında kullanılamayacak şekilde günlüğe kaydedilmez.

Her bir işlemin **sunucu tarafı gecikme süresi** ölçümleri, aşağıdaki görüntüde gösterildiği gibi görüntülenir:

![Sunucu tarafı gecikme ölçümleri için filtreler](./media/monitor-server-side-latency/server-side-latency-filters.png)

Ayrıca, **bölmeyi Uygula** seçeneğini kullanarak ölçümleri gruplandırabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

* Azure 'da [tanılama ayarlarını](cosmosdb-monitor-resource-logs.md) kullanarak Azure Cosmos DB verileri izleyin.
* [Denetim düzlemi işlemlerini denetim Azure Cosmos DB](audit-control-plane-logs.md)