---
title: Azure Cosmos DB hesabını başka bir bölgeye taşımayı öğrenin
description: Azure Cosmos DB hesabını başka bir bölgeye taşımayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 09/12/2020
ms.author: mjbrown
ms.openlocfilehash: 60c28a96008355491c058cd08dbbb3a1cbffad98
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059453"
---
# <a name="how-to-move-an-azure-cosmos-db-account-to-another-region"></a>Azure Cosmos DB hesabını başka bir bölgeye taşıma

Bu makalede, verilerin Azure Cosmos DB çoğaltılacağı bir bölgenin nasıl taşınacağı veya hesabın (Azure Resource Manager) meta verilerinin yanı sıra bir bölgeden diğerine nasıl geçirileceği açıklanır.

## <a name="move-data-from-one-region-to-another"></a>Verileri bir bölgeden diğerine taşıma

Azure Cosmos DB veri çoğaltmasını yerel olarak destekler, böylece verileri bir bölgeden diğerine taşımak basittir ve Azure portal, Azure PowerShell veya Azure CLı kullanılarak gerçekleştirilebilir ve aşağıdaki adımları içerir:

1. Hesaba yeni bir bölge ekleyin

    Bir Azure Cosmos DB hesabına yeni bir bölge eklemek için bkz. bir [Azure Cosmos DB hesabına bölge ekleme/kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)

1. Yeni bölgeye el ile yük devretme gerçekleştirme

    Kaldırılmakta olan bölgenin şu anda hesap için yazma bölgesi olduğu durumlarda, yukarıda eklenen yeni bölgeye bir yük devretme başlatmanız gerekir. Bu, sıfır kesinti süresi işlemidir. Çok bölgeli bir hesapta bir okuma bölgesi taşıyorsanız, bu adımı atlayabilirsiniz. Yük devretmeyi başlatmak için bkz. [Azure Cosmos hesabında el ile yük devretme gerçekleştirme](how-to-manage-database-account.md#manual-failover)

1. Özgün bölgeyi kaldır

    Bir Azure Cosmos DB hesabından bir bölgeyi kaldırmak için bkz. bir [Azure Cosmos DB hesabına bölge ekleme/kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)

## <a name="migrate-azure-cosmos-db-account-meta-data"></a>Azure Cosmos DB hesabı meta verilerini geçirme

Azure Cosmos DB, hesap meta verilerinin bir bölgeden diğerine geçirilmesini yerel olarak desteklemez. Hesap meta verilerini ve müşteri verilerini bir bölgeden diğerine geçirmek için, istenen bölgede yeni bir hesap oluşturulması ve ardından verilerin el ile kopyalanması gerekir. SQL API 'sinin neredeyse sıfır kapalı kalma süresi geçişi, [değişiklik akışı](change-feed.md) veya onu kullanan bir araç gerektirir. MongoDB API 'SI, Cassandra veya diğer API 'yi geçiriyorsanız veya hesaplar arasında veri geçirirken seçenekler hakkında daha fazla bilgi edinmek için, Şirket [içi veya bulut verilerinizi Azure Cosmos DB 'ye geçirme seçenekleri](cosmosdb-migrationchoices.md)konusuna bakın. Aşağıdaki adımlarda, SQL API için bir Azure Cosmos DB hesabının ve verilerinin bir bölgeden diğerine nasıl geçirileceği gösterilmektedir:

1. İstenen bölgede yeni bir Azure Cosmos DB hesabı oluşturun

    Azure portal, PowerShell veya CLı ile yeni bir hesap oluşturmak için, [bir Azure Cosmos DB hesabı oluşturun](how-to-manage-database-account.md#create-an-account).

1. Yeni bir veritabanı ve kapsayıcı oluşturma

    Yeni bir veritabanı ve kapsayıcı oluşturmak için bkz. [Azure Cosmos kapsayıcısı oluşturma](how-to-create-container.md)

1. Azure Cosmos DB Live Data Migrator aracıyla veri geçirme

    Neredeyse sıfır kapalı kalma süresine sahip verileri geçirmek için bkz. [Azure Cosmos DB canlı veri Migrator aracı](https://github.com/Azure-Samples/azure-cosmosdb-live-data-migrator)

1. Uygulama bağlantı dizesini Güncelleştir

    Canlı Migrator aracı çalışmaya devam ediyorsa, uygulamalarınızın yeni dağıtımında bağlantı bilgilerini güncelleştirin. Uygulamanıza yönelik uç noktalar ve anahtarlar Azure portal alınabilir.

    :::image type="content" source="./media/secure-access-to-data/nosql-database-security-master-key-portal.png" alt-text="Azure portal-gösteren NoSQL veritabanı güvenliğine erişim denetimi (ıAM)":::

1. İstekleri yeni uygulamaya yönlendir

    Yeni uygulama Azure Cosmos DB bağlandığında, istemci isteklerini yeni dağıtımınıza yeniden yönlendirebilirsiniz.

1. Artık gerekli olmayan kaynakları Sil

    Artık yeni örneğe tamamen yeniden yönlendirilen isteklerle, eski Azure Cosmos DB hesabını ve canlı veri Migrator aracını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabının yanı sıra veritabanı ve kapsayıcıları yönetme hakkında daha fazla bilgi ve örnek için aşağıdaki makaleleri okuyun:

* [Azure Cosmos hesabını yönetme](how-to-manage-database-account.md)
* [Azure Cosmos DB akışı değiştirme](change-feed.md)
