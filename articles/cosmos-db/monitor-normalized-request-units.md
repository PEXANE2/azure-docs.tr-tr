---
title: Azure Cosmos kapsayıcısı veya bir hesap için normalleştirilmiş RU/s 'yi izleme
description: Azure Cosmos DB bir işlemin normalleştirilmiş istek birimi kullanımını nasıl izleyeceğinizi öğrenin. Bir Azure Cosmos DB hesabının sahipleri, hangi işlemlerin daha fazla istek birimi tükettiğini anlayabilir.
ms.service: cosmos-db
ms.topic: how-to
author: kanshiG
ms.author: govindk
ms.date: 06/25/2020
ms.openlocfilehash: 8709389208ba1320685b1834b20893f08ef33ed7
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85482913"
---
# <a name="how-to-monitor-normalized-rus-for-an-azure-cosmos-container-or-an-account"></a>Azure Cosmos kapsayıcısı veya bir hesap için normalleştirilmiş RU/s 'yi izleme

Azure Cosmos DB için Azure Izleyici, hesabınızı izlemek ve panolar oluşturmak için bir ölçüm görünümü sağlar. Azure Cosmos DB ölçümleri varsayılan olarak toplanır, bu özellik açıkça her şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez.

**NORMALLEŞTIRILMIŞ ru tüketim** ölçümü, çoğaltmaların, bölüm anahtarı aralıkları genelinde istek birimleri tüketimine göre ne kadar iyi doygun olduğunu görmek için kullanılır. Azure Cosmos DB, üretilen işi tüm fiziksel bölümler arasında eşit olarak dağıtır. Bu ölçüm, bir çoğaltma kümesi içindeki maksimum üretilen iş kullanımının saniye başına bir görünümünü sağlar. Bu ölçümü, belirtilen kapsayıcının bölümleri genelinde RU/s kullanımını hesaplamak için kullanın. Bu ölçümü kullanarak, istek birimi kullanımının yüksek yüzdesini görürseniz, iş yükünüzün ihtiyaçlarını karşılamak için aktarım hızını artırmanız gerekir.

## <a name="what-to-expect-and-do-when-normalized-rus-is-higher"></a>Normalleştirilmiş RU/s daha yüksek olduğunda beklenmeli ve yapılacak

Normalleştirilmiş RU/sn tüketimi %100 ' e ulaştığında, istemci hız sınırlandırma hatalarını alır. İstemci bekleme süresini dikkate almalıdır ve yeniden dener. %100 kullanımına ulaşan kısa bir ani artış varsa, çoğaltma üzerindeki üretilen iş, en yüksek performans sınırına ulaştı. Örneğin, bir çoğaltmada tüm RU/s 'leri tüketen saklı yordam gibi tek bir işlem, normalleştirilmiş RU/sn tüketimine göre kısa bir ani artış oluşmasına neden olur. Bu gibi durumlarda, istek hızı düşük olursa hiçbir anında hız sınırlama hatası olmayacaktır. Bunun nedeni, Azure Cosmos DB isteklerin belirli bir istek için sağlanan RU/s 'den daha fazla ücret almasına izin verdiğinden ve bu süre içindeki diğer isteklerin hız sınırlı olmasına olanak sağlar.

Azure Izleyici ölçümleri, **toplam istek** ölçümünü kullanarak durum kodu başına işlem bulmanıza yardımcı olur. Daha sonra bu isteklere 429 durum kodu ile filtre uygulayabilir ve bunları **Işlem türüne**göre bölebilirsiniz.

Oran sınırlı olan istekleri bulmak için, bu bilgileri tanılama günlükleri aracılığıyla almanız önerilir.

%100 normalleştirilmiş RU/sn tüketimi için sürekli en yüksek değer varsa veya %100 ' e yakınsa, üretilen işi artırmanız önerilir. Azure izleyici ölçümlerini ve Azure izleyici günlüklerini kullanarak hangi işlemlerin ağır olduğunu ve en yüksek kullanımlarını öğrenebilirsiniz.

**NORMALLEŞTIRILMIŞ ru tüketim** ölçümü, kullanım açısından hangi bölüm anahtarı aralığının daha sıcak olduğunu görmek için de kullanılır; Böylece, bir bölüm anahtar aralığına doğru aktarım hızını çarpıklığı sağlar. Daha sonra, kullanım açısından hangi mantıksal bölüm anahtarlarının etkin olduğu hakkında bilgi almak için Azure Izleyici günlüklerinde **Partitionkeyrutüketim** günlüğünü görmek üzere daha sonra izleyebilirsiniz.

## <a name="view-the-normalized-request-unit-consumption-metric"></a>Normalleştirilmiş istek birimi tüketim ölçümünü görüntüleyin

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **ölçümler**' i seçin.

   :::image type="content" source="./media/monitor-normalized-request-units/monitor-metrics-blade.png" alt-text="Azure Izleyici 'de ölçümler bölmesi":::

3. **Ölçümler** bölmesinden > **bir kaynak seçin** > gerekli **aboneliği**ve **kaynak grubunu**seçin. **Kaynak türü**için **Azure Cosmos DB hesapları**' nı seçin, mevcut Azure Cosmos hesaplarınızdan birini seçin ve **Uygula**' yı seçin.

   :::image type="content" source="./media/monitor-normalized-request-units/select-cosmos-db-account.png" alt-text="Ölçümleri görüntülemek için bir Azure Cosmos hesabı seçin":::

4. Ardından, kullanılabilir ölçümler listesinden bir ölçüm seçebilirsiniz. Talep birimleri, depolama, gecikme, kullanılabilirlik, Cassandra ve diğer kullanıcılara özgü ölçümleri seçebilirsiniz. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için [kategoriye göre ölçümler](monitor-cosmos-db-reference.md) makalesine bakın. Bu örnekte, **NORMALLEŞTIRILMIŞ ru tüketim** ölçümünü ve **en fazla** toplama değeri olarak bir değer seçelim.

   Bu ayrıntılara ek olarak, ölçümlerin **zaman aralığını** ve **zaman parçalı yapısını** da seçebilirsiniz. En fazla, son 30 güne ait ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra filtreniz temelinde bir grafik görüntülenir.

   :::image type="content" source="./media/monitor-normalized-request-units/normalized-request-unit-usage-metric.png" alt-text="Azure portal bir ölçüm seçin":::

### <a name="filters-for-normalized-request-unit-consumption"></a>Normalleştirilmiş istek birimi tüketimi için filtreler

Ayrıca ölçümleri ve belirli bir **CollectionName**, **DatabaseName**, **Partitionkeyrangeıd**ve **bölge**tarafından görünen grafiği filtreleyebilirsiniz. Ölçümleri filtrelemek için **Filtre Ekle** ' yi seçin ve **KoleksiyonAdı** ve ilgilendiğiniz karşılık gelen değer gibi gerekli özelliği seçin. Daha sonra grafik, seçili dönem için kapsayıcı için tüketilen normalleştirilmiş RU tüketim birimlerini görüntüler.  

**Bölmeyi Uygula** seçeneğini kullanarak ölçümleri gruplandırabilirsiniz.  

Her kapsayıcı için normalleştirilmiş istek birimi tüketim ölçümü, aşağıdaki görüntüde gösterildiği gibi görüntülenir:

:::image type="content" source="./media/monitor-normalized-request-units/normalized-request-unit-usage-filters.png" alt-text="Normalleştirilmiş istek birimi tüketim ölçümüne filtre uygulayın":::

## <a name="next-steps"></a>Sonraki adımlar

* Azure 'da [tanılama ayarlarını](cosmosdb-monitor-resource-logs.md) kullanarak Azure Cosmos DB verileri izleyin.
* [Denetim düzlemi işlemlerini denetim Azure Cosmos DB](audit-control-plane-logs.md)
