---
title: Azure Cosmos kapsayıcısı veya bir hesap için normalleştirilmiş RU/s 'yi izleme
description: Azure Cosmos DB bir işlemin normalleştirilmiş istek birimi kullanımını nasıl izleyeceğinizi öğrenin. Bir Azure Cosmos DB hesabının sahipleri, hangi işlemlerin daha fazla istek birimi tükettiğini anlayabilir.
ms.service: cosmos-db
ms.topic: how-to
author: kanshiG
ms.author: govindk
ms.date: 06/25/2020
ms.openlocfilehash: e7005a3786bb2d538450b076c113e159c766d72e
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88642087"
---
# <a name="how-to-monitor-normalized-rus-for-an-azure-cosmos-container-or-an-account"></a>Azure Cosmos kapsayıcısı veya bir hesap için normalleştirilmiş RU/s 'yi izleme

Azure Cosmos DB için Azure Izleyici, hesabınızı izlemek ve panolar oluşturmak için bir ölçüm görünümü sağlar. Azure Cosmos DB ölçümleri varsayılan olarak toplanır, bu özellik açıkça her şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez.

**NORMALLEŞTIRILMIŞ ru tüketim** ölçümü, bölüm anahtarı aralıklarının trafiğe göre ne kadar iyi doymuş olduğunu görmek için kullanılır. Azure Cosmos DB üretilen işi tüm bölüm anahtarı aralıklarında eşit olarak dağıtır. Bu ölçüm, bölüm anahtar aralığı için en yüksek aktarım hızı kullanımının saniye başına bir görünümünü sağlar. Bu ölçüyü, belirtilen kapsayıcı için bölüm anahtar aralığı genelinde RU/s kullanımını hesaplamak için kullanın. Bu ölçümü kullanarak, Azure izleyici 'deki tüm bölüm anahtarı aralıklarında istek birimi kullanımının yüksek yüzdesini görürseniz, iş yükünüzün ihtiyaçlarını karşılamak için aktarım hızını artırmanız gerekir. 

## <a name="what-to-expect-and-do-when-normalized-rus-is-higher"></a>Normalleştirilmiş RU/s daha yüksek olduğunda beklenmeli ve yapılacak

Normalleştirilmiş RU/sn tüketimi, belirtilen bölüm anahtarı aralığı için %100 ' e ulaştığında ve bir istemci bu zaman penceresinde belirli bir bölüm anahtarı aralığına istek yapıyorsa, bu hız sınırlı bir hata alır. İstemci önerilen bekleme süresini dikkate almalıdır ve isteği yeniden dener. SDK, uygun bir şekilde bekleyerek, önceden yapılandırılmış süreleri yeniden denemeden bu durumu daha kolay hale getirir.  Normalleştirilmiş RU %100 değerine ulaştığından, RU oranı sınırlandırma hatasını görmeniz gerekli değildir. Bunun nedeni, normalleştirilmiş RU 'nın tüm bölüm anahtarı aralıkları üzerinde en fazla kullanımı temsil eden tek bir değer olmasından kaynaklanır, bir bölüm anahtar aralığı meşgul olabilir ancak diğer bölüm anahtarı aralıkları sorun olmadan isteklere sunabilir. Örneğin, bir bölüm anahtar aralığındaki tüm RU/s 'leri tüketen saklı yordam gibi tek bir işlem, normalleştirilmiş RU/s tüketimine göre kısa bir ani artış oluşmasına neden olur. Bu gibi durumlarda, istek hızı düşük olduğunda veya farklı bölüm anahtarı aralıklarında diğer bölümlere istekler yapılırsa, hiçbir anında hız sınırlama hatası olmayacaktır. 

Azure Izleyici ölçümleri, **toplam istek** ÖLÇÜMÜNÜ kullanarak SQL API için durum kodu başına işlemleri bulmanıza yardımcı olur. Daha sonra bu isteklere 429 durum kodu ile filtre uygulayabilir ve bunları **Işlem türüne**göre bölebilirsiniz.  

Oran sınırlı olan istekleri bulmak için, bu bilgileri tanılama günlükleri aracılığıyla almanız önerilir.

En fazla %100 normalleştirilmiş RU/sn tüketimi veya birden çok bölüm anahtar aralığı genelinde %100 ' e kapatırsanız, üretilen işi arttırmanız önerilir. Azure izleyici ölçümlerini ve Azure izleyici tanılama günlüklerini kullanarak hangi işlemlerin ağır olduğunu ve en yüksek kullanımlarını öğrenebilirsiniz.

Özet bölümünde, hangi bölüm anahtarı aralığının kullanım açısından daha sıcak olduğunu görmek için **NORMALLEŞTIRILMIŞ ru tüketim** ölçümü kullanılır. Böylece, bir bölüm anahtar aralığına doğru aktarım hızını çarpıklığı sağlar. Daha sonra, kullanım açısından hangi mantıksal bölüm anahtarlarının etkin olduğu hakkında bilgi almak için Azure Izleyici günlüklerinde **Partitionkeyrutüketim** günlüğünü görmek üzere daha sonra izleyebilirsiniz. Bu, bölüm anahtarı seçiminde değişikliği ya da uygulama mantığındaki değişikliği işaret eder. Hız sınırlamasını çözümlemek için, verilerin yükünü birden çok bölüme dağıtın veya gerçekten gerekli olduğu gibi aktarım hızını artırın. 



## <a name="view-the-normalized-request-unit-consumption-metric"></a>Normalleştirilmiş istek birimi tüketim ölçümünü görüntüleyin

1. [Azure portalında](https://portal.azure.com/) oturum açın.

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
