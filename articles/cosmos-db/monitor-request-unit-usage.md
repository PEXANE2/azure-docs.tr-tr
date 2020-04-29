---
title: Azure Cosmos DB bir işlemin üretilen iş kullanımını izleme
description: Azure Cosmos DB bir işlemin aktarım hızını veya istek birimi kullanımını nasıl izleyeceğinizi öğrenin. Bir Azure Cosmos DB hesabının sahipleri, hangi işlemlerin daha fazla Istek birimi aldığını anlayabilir.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 04/09/2020
ms.openlocfilehash: bc39ef199a5d40d3eaa75023277d3e00b93e131a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81115436"
---
# <a name="how-to-monitor-throughput-or-request-unit-usage-of-an-operation-in-azure-cosmos-db"></a>Azure Cosmos DB bir işlemin üretilen iş veya istek birimi kullanımını izleme

Azure Cosmos DB için Azure Izleyici, hesabınızı izlemek ve panolar oluşturmak için bir ölçüm görünümü sağlar. Azure Cosmos DB ölçümleri varsayılan olarak toplanır, bu özellik açıkça her şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez. **Toplam Istek birimleri** ölçümü, farklı işlem türleri için istek birimleri kullanımını almak için kullanılır. Daha sonra, üretilen işlemin büyük bir bölümünü analiz edebilirsiniz. Varsayılan olarak, aktarım hızı verileri tek dakikalık bir aralıkta toplanır. Ancak, toplama birimini zaman ayrıntı düzeyi seçeneğini değiştirerek değiştirebilirsiniz.

İstek birimi kullanım verilerini çözümlemenin iki yolu vardır:

* Verilen zaman aralığı içinde, hangi işlemlerin daha fazla istek birimi aldığını.
* Genel olarak hangi işlemler, daha fazla istek birimi tüketerek iş yükünüzü ortadan kaldırma.
Bu analiz, INSERT, upsert ve dizinlerine bakma gibi işlemlere odaklanabilmenizi sağlar. Belirli alanların dizinini oluşturma/kapatma altında olup olmadığını ve [Dizin oluşturma ilkesini](index-policy.md#include-exclude-paths) , yolları dahil etmek veya hariç tutmak için değiştirip değiştirebileceğinizi öğrenebilirsiniz.

Bazı sorgulara daha fazla istek birimi olduğunu fark ederseniz, şu gibi eylemler gerçekleştirebilirsiniz:

* Doğru miktarda veri isteğinde bulunduğunuzu yeniden düşünün.
* Sorguyu Filter yan tümcesiyle birlikte kullanmak üzere değiştirin.
* Daha ucuz UDF işlev çağrıları gerçekleştirin.
* Farklı bölümlere sorgu dışı olan fanı azaltmak için bölüm anahtarlarını tanımlayın.
* Sorgu yürütme hakkında daha fazla bilgi edinmek için çağrı yanıtında döndürülen sorgu ölçümlerini, tanılama günlüğü ayrıntılarını da kullanabilirsiniz ve [sorgu performansı ayarlama](sql-api-query-metrics.md) makalesine başvurabilirsiniz.
* Sum 'dan başlayıp, ardından sağ boyutu kullanarak ortalama kullanım bölümüne bakabilirsiniz.

## <a name="view-the-total-request-unit-usage-metric"></a>Toplam istek birimi kullanım ölçümünü görüntüleyin

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **ölçümler**' i seçin.

   ![Azure Izleyici 'de ölçümler bölmesi](./media/monitor-request-unit-usage/monitor-metrics-blade.png)

1. **Ölçümler** bölmesinden > **bir kaynak seçin** > gerekli **aboneliği**ve **kaynak grubunu**seçin. **Kaynak türü**için **Azure Cosmos DB hesapları**' nı seçin, mevcut Azure Cosmos hesaplarınızdan birini seçin ve **Uygula**' yı seçin.

   ![Ölçümleri görüntülemek için Azure Cosmos DB hesabı seçin](./media/monitor-request-unit-usage/select-cosmos-db-account.png)

1. Ardından, kullanılabilir ölçümler listesinden **Toplam Istek birimleri** ölçüsünü seçin. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için [kategoriye göre ölçümler](monitor-cosmos-db-reference.md) makalesine bakın. Bu örnekte **Toplam Istek birimlerini** ve toplama değeri olarak **Ort** ' i seçelim. Bu ayrıntılara ek olarak, ölçümlerin **zaman aralığını** ve **zaman parçalı yapısını** da seçebilirsiniz. En fazla, son 30 güne ait ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra filtreniz temelinde bir grafik görüntülenir. Seçili dönem için dakika başına tüketilen ortalama istek birimi sayısını görebilirsiniz.  

   ![Azure portal bir ölçüm seçin](./media/monitor-request-unit-usage/request-unit-usage-metric.png)

## <a name="filters-for-request-unit-usage"></a>İstek birimi kullanımı için filtreler

Ayrıca ölçümleri filtreleyebilir ve belirli bir **CollectionName**, **DatabaseName**, **OperationType**, **bölge**, **durum**ve **StatusCode**tarafından görünen grafikleri alabilirsiniz. **Filtre Ekle** ve **bölmeyi Uygula** seçenekleri, istek birimi kullanımını filtrelemenizi ve ölçümleri gruplandıreklemenizi sağlar.

Her bir işlemin istek birimi kullanımını toplam (Toplam) veya ortalamaya göre almak için, **bölmeyi Uygula** ' yı seçin ve aşağıdaki görüntüde gösterildiği gibi **işlem türünü** ve filtre değerini seçin:

   ![Azure izleyici 'de işlemler için Cosmos DB Isteği birimleri](./media/monitor-request-unit-usage/request-unit-usage-operations.png)

İstek birim kullanımını koleksiyona göre görmek isterseniz, **bölmeyi Uygula** ' yı seçin ve koleksiyon adını filtre olarak seçin. Panoda bulunan koleksiyonlar arasından aşağıdaki gibi bir sohbet görürsünüz. Daha sonra daha fazla ayrıntı görüntülemek için belirli bir koleksiyon adı seçebilirsiniz:

   ![Azure izleyici 'de koleksiyon tarafından tüm işlemler için Istek birimleri Cosmos DB](./media/monitor-request-unit-usage/request-unit-usage-collection.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure 'da [tanılama ayarlarını](cosmosdb-monitor-resource-logs.md) kullanarak Azure Cosmos DB verileri izleyin.
* [Denetim düzlemi işlemlerini denetim Azure Cosmos DB](audit-control-plane-logs.md)