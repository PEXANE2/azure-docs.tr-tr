---
title: Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri
description: Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri.
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/12/2020
ms.reviewer: sngun
ms.openlocfilehash: f640ad85fd34dd5a4803e7dd96f1c0283f0c859a
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93100441"
---
# <a name="apache-cassandra-and-azure-cosmos-db-consistency-levels"></a>Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Azure Cosmos DB aksine Apache Cassandra kesin olarak tanımlanmış tutarlılık garantisi sağlamaz. Bunun yerine Apache Cassandra, yüksek kullanılabilirlik, tutarlılık ve gecikme süresi avantajları sağlamak için bir yazma tutarlılığı düzeyi ve bir okuma tutarlılığı düzeyi sağlar. Azure Cosmos DB Cassandra API kullanırken:

* Apache Cassandra 'ın yazma tutarlılığı düzeyi, Azure Cosmos hesabınızda yapılandırılmış varsayılan tutarlılık düzeyiyle eşleştirilir. Bir yazma işlemi (CL) tutarlılığı, istek başına temelinde değiştirilemez.

* Azure Cosmos DB, Cassandra istemci sürücüsüyle belirtilen okuma tutarlılığı düzeyini, bir okuma isteği üzerinde dinamik olarak yapılandırılmış Azure Cosmos DB tutarlılık düzeylerinden birine dinamik olarak eşler.

## <a name="mapping-consistency-levels"></a>Tutarlılık düzeylerini eşleme

Aşağıdaki tabloda, Cassandra API kullanılırken yerel Cassandra tutarlılık düzeylerinin Azure Cosmos DB tutarlılık düzeylerine nasıl eşlendiği gösterilmektedir:  

:::image type="content" source="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" alt-text="Cassandra tutarlılık modeli eşleme" lightbox="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" :::

Azure Cosmos hesabınız, güçlü tutarlılık dışında bir tutarlılık düzeyiyle yapılandırıldıysa, olasılık açısından *olasılığa sınırlı bir Stalet* (PBS) ölçüsüne bakarak istemcilerinizin iş yükleriniz için güçlü ve tutarlı okumalar alabilir olasılığını fark edebilirsiniz. Bu ölçüm Azure portal kullanıma sunulur, daha fazla bilgi edinmek için bkz. [Izleme olasılığa yönelik sınırlı Stalet (PBS) ölçümü](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

Dayalı sınırlanmış stalet, nihai tutarlılığın ne kadar olduğunu gösterir. Bu ölçüm, Azure Cosmos hesabınızda halen yapılandırdığınız tutarlılık düzeyinden daha güçlü bir tutarlılık elde etme konusunda bir fikir sağlar. Diğer bir deyişle, yazma ve okuma bölgelerinin bir birleşimi için kesin olarak tutarlı okuma elde etme olasılığını (milisaniye olarak ölçülür) görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB için genel dağıtım ve tutarlılık düzeyleri hakkında daha fazla bilgi edinin:

* [Genel dağıtıma genel bakış](distribute-data-globally.md)
* [Tutarlılık düzeyine genel bakış](consistency-levels.md)
* [Yüksek kullanılabilirlik](high-availability.md)
