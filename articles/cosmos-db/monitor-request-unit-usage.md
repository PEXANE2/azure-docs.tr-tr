---
title: Azure Cosmos DB'de bir işlemin iş letiniş kullanımını izleme
description: Azure Cosmos DB'deki bir işlemin birim kullanımını nasıl izleyeceğinizi veya birim kullanımını nasıl isteyeceğinizi öğrenin. Azure Cosmos DB hesabının sahipleri, hangi işlemlerin daha fazla İstek birimi aldığını anlayabilir.
ms.service: cosmos-db
ms.topic: conceptual
author: kanshiG
ms.author: govindk
ms.date: 04/09/2020
ms.openlocfilehash: bc39ef199a5d40d3eaa75023277d3e00b93e131a
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115436"
---
# <a name="how-to-monitor-throughput-or-request-unit-usage-of-an-operation-in-azure-cosmos-db"></a>Azure Cosmos DB'de bir işlemin iş verimi veya birim kullanımını isteme izleme

Azure Cosmos DB için Azure Monitörü, hesabınızı izlemek ve panolar oluşturmak için bir ölçüm görünümü sağlar. Azure Cosmos DB ölçümleri varsayılan olarak toplanır, bu özellik herhangi bir şeyi açıkça etkinleştirmenizi veya yapılandırmanızı gerektirmez. **Toplam İstek Birimleri** ölçümü, farklı işlem türleri için istek birimlerinin kullanımını almak için kullanılır. Daha sonra, hangi işlemlerin en çok kullanıldığını analiz edebilirsiniz. Varsayılan olarak, iş verileri bir dakika aralığında toplanır. Ancak, zaman taneciklilik seçeneğini değiştirerek toplama birimini değiştirebilirsiniz.

İstek birimi kullanım verilerini çözümlemenin iki yolu vardır:

* Verilen zaman aralığında hangi işlemler daha fazla istek birimi alıyor.
* Hangi işlemler genel olarak daha fazla istek birimi tüketerek iş yükünüze hakimdir.
Bu çözümleme, ekleme, yükseltme ve dizin oluşturma gibi işlemlere odaklanmanızı sağlar. Belirli alanları dizinoluşturma nın üzerinde/altında olup olmadığını öğrenebilir ve [dizin oluşturma ilkesini](index-policy.md#include-exclude-paths) yolları içerecek veya hariç tutmak için değiştirebilirsiniz.

Belirli sorguların daha fazla istek birimi aldığını fark ederseniz, şu gibi eylemlerde bulunabilirsiniz:

* Doğru miktarda veri talep ediyorsanız yeniden gözden geçirin.
* Filtre yan tümcesi ile dizin kullanmak için sorguyu değiştirin.
* Daha az pahalı UDF işlev çağrıları gerçekleştirin.
* Farklı bölümlere sorgu dışında fan en aza indirmek için bölüm tuşlarını tanımlayın.
* Ayrıca, arama yanıtında döndürülen sorgu ölçümlerini, tanılama günlüğü ayrıntılarını kullanabilir ve sorgu yürütmesi hakkında daha fazla bilgi edinmek için [sorgu performansı alamı](sql-api-query-metrics.md) makalesine başvurabilirsiniz.
* Toplamdan başlayıp doğru boyutu kullanarak avg kullanımına bakabilirsiniz.

## <a name="view-the-total-request-unit-usage-metric"></a>Toplam istek birimi kullanım ölçüm

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Sol navigasyon çubuğundan **Monitör'ü** seçin ve **Ölçümler'i**seçin.

   ![Azure Monitör'de Ölçümler bölmesi](./media/monitor-request-unit-usage/monitor-metrics-blade.png)

1. **Ölçümler** bölmesinden > Gerekli **aboneliği**ve **kaynak grubunu**seçmek > bir **kaynak** seçin. Kaynak **türü**için **Azure Cosmos DB hesaplarını**seçin, varolan Azure Cosmos hesaplarınızdan birini seçin ve **Uygula'yı**seçin.

   ![Ölçümleri görüntülemek için Azure Cosmos DB hesabını seçin](./media/monitor-request-unit-usage/select-cosmos-db-account.png)

1. Ardından, kullanılabilir ölçümler listesinden **Toplam İstek Birimleri** metrikünü seçin. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek [için, kategori](monitor-cosmos-db-reference.md) makalesine göre Ölçümler'e bakın. Bu örnekte, toplama değeri olarak **Toplam İstek Birimleri** ve **Avg'yi** seçelim. Bu ayrıntılara ek olarak, ölçümlerin **Zaman aralığı** ve **Zaman parçalı lığını** da seçebilirsiniz. Maksimum olarak, son 30 gün için ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra, filtrenize göre bir grafik görüntülenir. Seçili dönem için dakika başına tüketilen ortalama istek birimi sayısını görebilirsiniz.  

   ![Azure portalından bir metrik seçin](./media/monitor-request-unit-usage/request-unit-usage-metric.png)

## <a name="filters-for-request-unit-usage"></a>İstek birimi kullanımı için filtreler

Ayrıca ölçümlere filtre uygulayabilir ve belirli bir **CollectionName, DatabaseName,** **OperationType**, **Region**, Status , **Status**Code ve **StatusCode**tarafından görüntülenen grafikleri alabilirsiniz. **DatabaseName** **Filtre Ekle** ve **Uygula bölme** seçenekleri, istek birimi kullanımını filtrelemenize ve ölçümleri gruplandırmanıza olanak tanır.

Her işlemin istek birimi kullanımını toplam(toplam) veya ortalamaya göre almak için, **Splitting uygula'yı** seçin ve Aşağıdaki resimde gösterildiği gibi **İşlem türünü** ve filtre değerini seçin:

   ![Azure monitöründeki işlemler için Cosmos DB İstek birimleri](./media/monitor-request-unit-usage/request-unit-usage-operations.png)

İstek birimi kullanımını koleksiyona göre görmek istiyorsanız, **Bölme uygula'yı** seçin ve filtre olarak koleksiyon adını seçin. Pano içinde çeşitli koleksiyonseçenekleriyle aşağıdaki gibi bir sohbet göreceksiniz. Daha sonra daha fazla ayrıntı görüntülemek için belirli bir koleksiyon adı seçebilirsiniz:

   ![Azure monitöründeki koleksiyona göre tüm işlemler için Cosmos DB İstek birimleri](./media/monitor-request-unit-usage/request-unit-usage-collection.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure'daki [tanı lama ayarlarını](cosmosdb-monitor-resource-logs.md) kullanarak Azure Cosmos DB verilerini izleyin.
* [Azure Cosmos DB denetim düzlemi işlemlerini denetleme](audit-control-plane-logs.md)