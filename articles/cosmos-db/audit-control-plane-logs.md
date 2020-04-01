---
title: Azure Cosmos DB denetim düzlemi işlemleri nasıl denetler?
description: Azure Cosmos DB'de bölge ekleme, iş ortasını güncelleştirme, bölge başarısızlığı, vNet vb. ekleme gibi denetim düzlemi işlemlerini nasıl denetleyeceğinizi öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: sngun
ms.openlocfilehash: 64ad8e6b1101d8486268c857b3a7752e1801f52c
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420268"
---
# <a name="how-to-audit-azure-cosmos-db-control-plane-operations"></a>Azure Cosmos DB denetim düzlemi işlemleri nasıl denetler?

Denetim düzlemi işlemleri, Azure Cosmos hesabında veya kapsayıcıda yapılan değişiklikleri içerir. Örneğin, bir Azure Cosmos hesabı oluşturmak, bir bölge eklemek, iş buzunu güncelleştirmek, bölge başarısızlığı, VNet vb. eklemek, denetim düzlemi işlemlerinden bazılarıdır. Bu makalede, Azure Cosmos DB'deki denetim düzlemi işlemlerinin nasıl denetlenecekleri açıklanmaktadır.

## <a name="disable-key-based-metadata-write-access"></a>Anahtar tabanlı meta veri yazma erişimini devre dışı
 
Azure Cosmos DB'deki denetim düzlemi işlemlerini denetlemeden önce, hesabınızdaki anahtar tabanlı meta veri yazma erişimini devre dışı düşürün. Anahtar tabanlı meta veri yazma erişimi devre dışı bırakıldığında, hesap anahtarları aracılığıyla Azure Cosmos hesabına bağlanan istemcilerin hesaba erişmesi engellenir. `disableKeyBasedMetadataWriteAccess` Özelliği doğru olarak ayarlayarak yazma erişimini devre dışı kullanabilirsiniz. Bu özelliği ayarladıktan sonra, herhangi bir kaynakta değişiklikler yalnızca uygun Rol tabanlı erişim denetimi (RBAC) rolü ve kimlik bilgilerine sahip bir kullanıcıdan gerçekleşebilir. Bu özelliğinasıl ayarleyeceğiniz hakkında daha fazla bilgi edinmek için [SDK'lar makalesindeki Değişiklikleri Önleme makalesine](role-based-access-control.md#preventing-changes-from-cosmos-sdk) bakın.

 Meta veri yazma erişimini kapatırken aşağıdaki noktaları göz önünde bulundurun:

* Uygulamalarınızın Yukarıdaki kaynakları değiştiren meta veri çağrıları yapmamasını değerlendirin ve emin olun (Örneğin, SDK veya hesap anahtarlarını kullanarak koleksiyon oluşturma, güncelleme iş bölümü, ...).

* Şu anda Azure portalı meta veri işlemleri için hesap anahtarlarını kullanır ve bu nedenle bu işlemler engellenir. Alternatif olarak, bu tür işlemleri gerçekleştirmek için Azure CLI, SDK veya Kaynak Yöneticisi şablon dağıtımlarını kullanın.

## <a name="enable-diagnostic-logs-for-control-plane-operations"></a>Kontrol düzlemi işlemleri için tanılama günlüklerini etkinleştirme

Azure portalını kullanarak denetim düzlemi işlemleri için tanılama günlüklerini etkinleştirebilirsiniz. Denetim düzlemi işlemlerinde günlüğe kaydetmeyi etkinleştirmek için aşağıdaki adımları kullanın:

1. Azure [portalında](https://portal.azure.com) oturum açın ve Azure Cosmos hesabınıza gidin.

1. **Tanılama ayarları** bölmesini açın, oluşturulacak günlükler için bir **Ad** sağlayın.

1. Günlük türü için **ControlPlaneRequests'i** seçin ve **Günlük Analitiğine Gönder** seçeneğini seçin.

Günlükleri bir depolama hesabında veya bir olay hub'ına akışta da depolayabilirsiniz. Bu makalede, günlükleri günlükleri günlük leri analiz göndermek ve sonra onları sorgulamak nasıl gösterir. Etkinleştirdikten sonra, tanılama günlüklerinin etkili olması birkaç dakika sürer. Bu noktadan sonra gerçekleştirilen tüm kontrol uçağı işlemleri izlenebilir. Aşağıdaki ekran görüntüsü, denetim düzlemgünlüklerinin nasıl etkinleştirilen gösterir:

![Denetim düzlemi istekleri günlüğe kaydetmeyi etkinleştirme](./media/audit-control-plane-logs/enable-control-plane-requests-logs.png)

## <a name="view-the-control-plane-operations"></a>Kontrol düzlemi işlemlerini görüntüleme

Günlüğe kaydetmeyi açtıktan sonra, belirli bir hesabın işlemlerini izlemek için aşağıdaki adımları kullanın:

1. [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol daki gezinmeden **Monitör** sekmesini açın ve ardından **Günlükler** bölmesini seçin. Kapsamda belirli bir hesapla sorguları kolayca çalıştırabileceğiniz bir UI açılır. Denetim düzlemgünlüklerini görüntülemek için aşağıdaki sorguyu çalıştırın:

   ```kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="ControlPlaneRequests"
   | where TimeGenerated >= ago(1h)
   ```

Bir Azure Cosmos hesabına Bir VNET eklendiğinde aşağıdaki ekran görüntüleri günlükleri yakalar:

![VNet eklendiğinde düzlem günlüklerini denetleme](./media/audit-control-plane-logs/add-ip-filter-logs.png)

Cassandra tablosunun iş bölümü güncelleştirildiğinde aşağıdaki ekran görüntüleri günlükleri yakalar:

![Elde iş güncelleştirme olduğunda düzlem günlüklerini denetleme](./media/audit-control-plane-logs/throughput-update-logs.png)

## <a name="identify-the-identity-associated-to-a-specific-operation"></a>Belirli bir işlemle ilişkili kimliği belirleme

Daha fazla hata ayıklamak istiyorsanız, Etkinlik Kimliği'ni kullanarak veya işlemin zaman damgası ile **Etkinlik günlüğündeki** belirli bir işlemi tanımlayabilirsiniz. Zaman damgası, etkinlik kimliğinin açıkça geçirilen olmadığı bazı Kaynak Yöneticisi istemcileri için kullanılır. Etkinlik günlüğü, işlemin başlatıldığı kimlik hakkında ayrıntılı bilgi verir. Aşağıdaki ekran görüntüsü, etkinlik kimliğinin nasıl kullanılacağını ve etkinlik günlüğünde onunla ilişkili işlemleri nasıl bulacağını gösterir:

![Etkinlik kimliğini kullanın ve işlemleri bulun](./media/audit-control-plane-logs/find-operations-with-activity-id.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB için Azure Monitörünü keşfedin](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json)
* [Azure Cosmos DB'deki ölçümlerle izleme ve hata ayıklama](use-metrics.md)