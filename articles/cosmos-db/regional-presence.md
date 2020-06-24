---
title: Azure Cosmos DB bölgesel varlık
description: Bu makalede, Azure Cosmos DB ve farklı bulut ortamlarının bölgesel varlığı açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.custom: seodec18
ms.openlocfilehash: 78f02c07932c6a058ba4fa27289640b7efd56169
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85113679"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Azure Cosmos DB bölgesel varlık

Azure Cosmos DB, Azure 'da temel bir hizmettir ve varsayılan olarak, Azure 'un kullanılabildiği tüm bölgelerde her zaman kullanılabilir. Azure Şu anda dünya çapındaki [54 bölgede](https://azure.microsoft.com/global-infrastructure/regions/) kullanılabilir. 

:::image type="content" source="./media/regional-presence/regional-presence.png" alt-text="Azure Cosmos DB kullanılabildiği bölgeler" lightbox="./media/regional-presence/regional-presence.png" border="false":::

Cosmos DB, müşteriler tarafından kullanılabilen beş farklı Azure bulut ortamında kullanılabilir:

* Genel olarak kullanılabilen **Azure genel** bulutu.

* **Azure Çin 21Vianet** , Microsoft ve 21Vianet arasındaki, ülkenin en büyük İnternet sağlayıcılarından biri Çin 'deki benzersiz bir iş ortaklığı aracılığıyla sunulmaktadır.

* **Azure Almanya** , bir veri güvenliği modeli altında hizmetler sağlar ve bu da müşteri verilerinin, Almanya veri emanetini gören Deutsche Telekoası olan T-Systems International GmbH denetimi altındaki Almanya 'da kalmasını sağlar.

* **Azure Kamu** , ABD kamu kurumlarına ve iş ortaklarına Birleşik Devletler dört bölgede kullanılabilir. 

* **Savunma Bakanlığı (DOD) Için Azure Kamu** , ABD savunma Bakanlığı Birleşik Devletler iki bölgede sunulmaktadır.

## <a name="regional-presence-with-global-distribution"></a>Genel dağıtım ile bölgesel varlık

Azure Cosmos DB (SQL, MongoDB, Cassandra, Gremlin ve Table dahil) tarafından sunulan tüm API 'Ler varsayılan olarak tüm Azure bölgelerinde kullanılabilir. Örneğin, yalnızca genel Azure bölgelerinde değil, aynı zamanda Çin, Almanya, devlet ve Savunma Bakanlığı (DoD) bölgeleri gibi bağımsız bulutlarda Azure Cosmos DB tarafından sunulan MongoDB ve Cassandra API 'Lerine sahip olabilirsiniz.

Azure Cosmos DB, [küresel olarak dağıtılmış](distribute-data-globally.md) bir veritabanı hizmetidir. Azure Cosmos hesabınızla istediğiniz sayıda Azure bölgesini ilişkilendirebilirsiniz ve verileriniz otomatik olarak ve saydam olarak çoğaltılır. İstediğiniz zaman Azure Cosmos hesabınıza bir bölge ekleyebilir veya kaldırabilirsiniz. Anahtar temelli küresel dağıtım özelliği ve çoklu ana kopyalı çoğaltma protokolüyle, Azure Cosmos DB 99. yüzdebirlik 99,999 ' den daha az okuma ve yazma gecikme süresi, okuma ve yazma kullanılabilirliği ve Azure Cosmos hesabınızla ilişkili tüm bölgelerde okuma ve yazma işlemleri için sağlanan aktarım hızını ölçeklendirmeye esnek olanağı sunar. Azure Cosmos DB, iyi tanımlanmış beş tutarlılık modeli de sunar ve verilerinize belirli bir tutarlılık modeli uygulamayı tercih edebilirsiniz. Son olarak Azure Cosmos DB, sektördeki tek veritabanı hizmetidir ve sağlanan aktarım hızı, 99. yüzdelik gecikme süresi, yüksek kullanılabilirlik ve tutarlılık dahil olmak üzere kapsamlı bir [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) sağlar. Yukarıdaki yetenekler tüm Azure bulutlarında kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık aşağıdaki makalelerle Azure Cosmos DB temel kavramları hakkında bilgi edinebilirsiniz:

* [Genel veri dağıtımı](distribute-data-globally.md)
* [Azure Cosmos DB hesabını yönetme](manage-account.md)
* [Azure Cosmos kapsayıcıları ve veritabanları için sağlama işleme](set-throughput.md)
* [SLA Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
