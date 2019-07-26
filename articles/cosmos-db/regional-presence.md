---
title: Azure Cosmos DB ile bölgesel varlığı
description: Bu makalede, Azure Cosmos DB ile farklı bulut ortamları hakkında bölgesel varlığını açıklanmaktadır.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: 775ca8c57a9a444df2f7d0a4df4224b7eba9d677
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68384033"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Azure Cosmos DB ile bölgesel varlığı

Azure Cosmos DB, Azure 'da temel bir hizmettir ve varsayılan olarak, Azure 'un kullanılabildiği tüm bölgelerde her zaman kullanılabilir. Şu anda Azure kullanılabilir [54 bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/) dünya çapında. 

[![Azure Cosmos DB kullanılabildiği bölgeler](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

Cosmos DB, tüm beş farklı Azure bulut ortamlarında müşterilerine kullanılabilir:

* **Azure genel** bulut, dünya çapında kullanılabilir.

* **Azure Çin 21Vianet** , Microsoft ve 21Vianet arasındaki, ülkenin en büyük İnternet sağlayıcılarından biri Çin 'deki benzersiz bir iş ortaklığı aracılığıyla sunulmaktadır.

* **Azure Almanya** Alman veri güvenilen kişisi olarak davranan Deutsche Telekom'ın bir yan kuruluşu olan T-Systems International GmbH denetimindeki Almanya'daki veri kalır, müşteri sağlayan bir veri Emanetçisi modeli altında hizmetleri sağlar.

* **Azure kamu** ABD'de ABD devlet kurumları ile bunların iş ortakları için dört bölgelerinde kullanılabilir. 

* **Savunma Bakanlığı (DOD) Için Azure Kamu** , ABD savunma Bakanlığı Birleşik Devletler iki bölgede sunulmaktadır.

## <a name="regional-presence-with-global-distribution"></a>Bölgesel varlığı ile genel dağıtım

Azure Cosmos DB (SQL, MongoDB, Cassandra, Gremlin ve Table dahil) tarafından sunulan tüm API 'Ler varsayılan olarak tüm Azure bölgelerinde kullanılabilir. Örneğin, yalnızca genel Azure bölgelerinde değil, aynı zamanda Çin, Almanya, devlet ve Savunma Bakanlığı (DoD) bölgeleri gibi bağımsız bulutlarda Azure Cosmos DB tarafından sunulan MongoDB ve Cassandra API 'Lerine sahip olabilirsiniz.

Azure Cosmos DB, [küresel olarak dağıtılmış](distribute-data-globally.md) bir veritabanı hizmetidir. Dilediğiniz sayıda Azure bölgesinde Azure Cosmos hesabınızla ilişkilendirebilirsiniz ve verilerinizin otomatik ve şeffaf şekilde çoğaltılır. Ekleyebilir veya bir bölge herhangi bir zamanda Azure Cosmos hesabınızı kaldırın. Anahtar teslim küresel dağıtım özelliği ve birden çok yönetilen çoğaltma protokolü ile Azure Cosmos DB 10 MS'den kısa okuyup 99. yüzdebirlik dilimde gecikme süreleri 99,999 okuma ve yazma kullanılabilirliğini ve sağlanan esnek bir şekilde ölçeklendirme olanağı sunar. aktarım hızı için okur ve Azure Cosmos hesabınızla ilişkili tüm bölgeler arasında yazar. Azure Cosmos DB, ayrıca beş iyi tanımlanmış tutarlılık modeli sunar ve belirli bir tutarlılık modelini, verilere uygulanacak seçebilirsiniz. Son olarak Azure Cosmos DB, sektördeki tek veritabanı hizmetidir ve sağlanan aktarım hızı, 99. yüzdelik gecikme süresi, yüksek kullanılabilirlik ve tutarlılık dahil olmak üzere kapsamlı bir [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) sağlar. Yukarıdaki yetenekler tüm Azure bulutlarında kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık aşağıdaki makalelerle Azure Cosmos DB temel kavramları hakkında bilgi edinebilirsiniz:

* [Genel veri dağıtımı](distribute-data-globally.md)
* [Bir Azure Cosmos DB hesabını yönetme](manage-account.md)
* [Azure Cosmos kapsayıcılar ve veritabanları için sağlama aktarım hızı](set-throughput.md)
* [SLA Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
