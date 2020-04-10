---
title: Azure Cosmos DB'deki işlemler için sunucu tarafı gecikmesi nasıl izlenir?
description: Azure Cosmos DB hesabındaki işlemler veya kapsayıcıiçin sunucu gecikmesini nasıl izleyeceğinizi öğrenin. Azure Cosmos DB hesabının sahipleri, Azure Cosmos hesaplarınızla ilgili sunucu tarafındaki gecikme sorunlarını anlayabilir.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 04/07/2020
ms.openlocfilehash: 62c10a2ada9ff7d3bf7090028dd9684192517d02
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991438"
---
# <a name="how-to-monitor-the-server-side-latency-for-operations-in-an-azure-cosmos-db-container-or-account"></a>Azure Cosmos DB kapsayıcısındaveya hesabındaki işlemler için sunucu tarafı gecikmesi nasıl izlenir?

Azure Cosmos DB için Azure Monitörü, hesabınızı izlemek ve panolar oluşturmak için bir ölçüm görünümü sağlar. Azure Cosmos DB ölçümleri varsayılan olarak toplanır, bu özellik herhangi bir şeyi açıkça etkinleştirmenizi veya yapılandırmanızı gerektirmez. Sunucu tarafı gecikme syon ölçümü, bir işlemin sunucu tarafı gecikmesini görüntülemek için kullanılır. Azure Cosmos DB, doğrudan bağlantı yla nokta okuma/yazma işlemleri için 10 ms'den az SLA sağlar. Nokta okuma ve yazma işlemleri için SLA'lar [SLA belgesinde](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/)ayrıntılı olarak hesaplanır.

Nokta işlemleri için alışılmadık derecede büyük bir gecikme görürseniz:

* Doğrudan modda bölüm tuşu ve kimliği ile bir alma veya ayarlama işlemi
* Okuma veya yazma işlemi veya
* Bir sorgu

Döndürülen verilerin boyutunu görmek için tanılama günlüğüne bakabilirsiniz. Sorgu işlemleri için sürekli bir yüksek gecikme süresi görürseniz, döndürülen verilerin boyutu, kullanılan veri veya [RU/s'ler veya](cosmosdb-monitor-resource-logs.md#diagnostic-queries) belirli bir dönemdeki bu tür işlemlerin sayısı için tanılama günlüğüne bakabilirsiniz. Bu şekilde, sunucu tarafındaki gecikme sorunlarını hata ayıklayabilirsiniz.

## <a name="view-server-side-latency-metric"></a>Sunucu tarafı gecikme ölçüsü

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Sol navigasyon çubuğundan **Monitör'ü** seçin ve **Ölçümler'i**seçin.

   ![Azure Monitör'de Ölçümler bölmesi](./media/monitor-server-side-latency/monitor-metrics-blade.png)

1. **Ölçümler** bölmesinden > Gerekli **aboneliği**ve **kaynak grubunu**seçmek > bir **kaynak** seçin. Kaynak **türü**için **Azure Cosmos DB hesaplarını**seçin, varolan Azure Cosmos hesaplarınızdan birini seçin ve **Uygula'yı**seçin.
   
   ![Ölçümleri görüntülemek için bir Cosmos DB hesabı seçin](./media/monitor-server-side-latency/select-cosmos-db-account.png)

1. Ardından, kullanılabilir ölçümler listesinden **Server Side Gecikme sonu** ölçümünü seçin. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek [için, kategori](monitor-cosmos-db-reference.md) makalesine göre Ölçümler'e bakın. Bu örnekte, toplama değeri olarak **Server Side Latency** ve **Avg'yi** seçelim. Bu ayrıntılara ek olarak, ölçümlerin **Zaman aralığı** ve **Zaman parçalı lığını** da seçebilirsiniz. Maksimum olarak, son 30 gün için ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra, filtrenize göre bir grafik görüntülenir. Seçili dönem için dakika başına tüketilen ortalama istek birimi sayısını görebilirsiniz.  

   ![Azure portalından Sunucu Tarafı Gecikme Sonu ölçüm'ini seçin](./media/monitor-server-side-latency/server-side-latency-metric.png)

## <a name="filters-for-server-side-latency"></a>Sunucu tarafı gecikmesi için filtreler

Ayrıca ölçümleri ve belirli bir **CollectionName,** **ConnectionMode,** **DatabaseName,** **OperationType,** **Region**ve **PublicAPIType**tarafından görüntülenen grafiğe de filtre uygulayabilirsiniz. 

Ölçümlere filtre uygulayın **Add filter** ve **PublicAPIType** gibi gerekli özelliği seçin ve **sql**değerini seçin. **OperationType**için başka bir filtre ekleyin. Grafik daha sonra, seçili dönemdeki farklı işlemler için sunucu tarafı gecikmesini görüntüler. Saklı yordam ı ile yürütülen işlemler günlüğe kaydedilmez, bu nedenle OperationType ölçümü altında kullanılamaz.

Her işlem için **Server Side Gecikme sonu** ölçümleri aşağıdaki resimde gösterildiği gibi görüntülenir:

![Sunucu tarafı gecikme ölçümleri için filtreler](./media/monitor-server-side-latency/server-side-latency-filters.png)

Ayrıca, **Uygula bölme** seçeneğini kullanarak ölçümleri gruplandırmayapabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

* Azure'da [tanılama ayarlarını](cosmosdb-monitor-resource-logs.md) kullanarak Azure Cosmos DB verilerini izleme
* [Azure Cosmos DB denetim düzlemi işlemlerini denetleme](audit-control-plane-logs.md)