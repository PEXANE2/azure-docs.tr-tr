---
title: Azure Cosmos DB ile bölgesel varlık
description: Bu makalede, Azure Cosmos DB'nin ve farklı bulut ortamlarının bölgesel varlığı açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.custom: seodec18
ms.openlocfilehash: 95b9a9804fe1a9473d226912f178284f701a1547
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72753222"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Azure Cosmos DB ile bölgesel varlık

Azure Cosmos DB, Azure'da temel bir hizmettir ve varsayılan olarak Azure'un kullanılabildiği tüm bölgelerde her zaman kullanılabilir. Azure şu anda dünya çapında [54 bölgede](https://azure.microsoft.com/global-infrastructure/regions/) kullanılabilir. 

[![Azure Cosmos DB'nin kullanılabildiği bölgeler](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

Cosmos DB, müşterilerin kullanabileceği beş farklı Azure bulut ortamının tümlerinde kullanılabilir:

* Küresel olarak kullanılabilen **Azure genel** bulutu.

* **Azure China 21Vianet,** Microsoft ile Ülkenin Çin'deki en büyük internet sağlayıcılarından biri olan 21Vianet arasındaki benzersiz bir ortaklık aracılığıyla kullanılabilir.

* **Azure Almanya,** Deutsche Telecom'un bir yan kuruluşu olan ve Alman veri mütevellisi olarak görev yapan T-Systems International GmbH'nin kontrolü altında müşteri verilerinin Almanya'da kalmasını sağlayan bir veri mütevelli modeli altında hizmet sunmaktadır.

* **Azure Kamu,** ABD'deki dört bölgede ABD devlet kurumları ve ortakları tarafından kullanılabilir. 

* **Azure Savunma Bakanlığı Hükümeti (DoD),** ABD Savunma Bakanlığı'nın ABD'de iki bölgesinde kullanılabilir.

## <a name="regional-presence-with-global-distribution"></a>Küresel dağılım ile bölgesel varlık

Azure Cosmos DB tarafından açığa çıkarılan tüm API'ler (SQL, MongoDB, Cassandra, Gremlin ve Table dahil) varsayılan olarak tüm Azure bölgelerinde kullanılabilir. Örneğin, MongoDB ve Cassandra API'lerinin Azure Cosmos DB tarafından yalnızca tüm küresel Azure bölgelerinde değil, Çin, Almanya, Hükümet ve Savunma Bakanlığı (DoD) bölgeleri gibi egemen bulutlarda da açığa çıkmalarını sağlayabilirsiniz.

Azure Cosmos DB, [dünya çapında dağıtılmış](distribute-data-globally.md) bir veritabanı hizmetidir. İstediğiniz sayıdaAzure bölgesini Azure Cosmos hesabınızla ilişkilendirebilirsiniz ve verileriniz otomatik olarak ve saydam bir şekilde çoğaltılır. Azure Cosmos hesabınıza istediğiniz zaman bir bölge ekleyebilir veya kaldırabilirsiniz. Anahtar teslimküresel dağıtım yeteneği ve çok hakim çoğaltma protokolü ile Azure Cosmos DB, 99 yüzdelik, 99.999 okuma ve yazma kullanılabilirliğinde 10'dan az okuma ve yazma gecikmesi ve elastik olarak sağlanan ölçeklendirme olanağı sunar Azure Cosmos hesabınızla ilişkili tüm bölgelerde okuma ve yazma için iş için iş için. Azure Cosmos DB, ayrıca beş iyi tanımlanmış tutarlılık modeli sunar ve verilerinize belirli bir tutarlılık modeli uygulamayı seçebilirsiniz. Son olarak, Azure Cosmos DB, sektördeki, sağlanan iş ortası, yüzde 99'luk gecikme, yüksek kullanılabilirlik ve tutarlılık içeren kapsamlı bir [Hizmet Düzeyi Sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) sağlayan tek veritabanı hizmetidir. Yukarıdaki özellikler tüm Azure bulutlarında kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'nin temel kavramları hakkında şu makaleleri öğrenebilirsiniz:

* [Küresel veri dağıtımı](distribute-data-globally.md)
* [Azure Cosmos DB hesabı nasıl yönetilir?](manage-account.md)
* [Azure Cosmos kapsayıcıları ve veritabanları için sağlama iş ortası](set-throughput.md)
* [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
