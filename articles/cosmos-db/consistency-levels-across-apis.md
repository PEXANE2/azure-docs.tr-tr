---
title: Tutarlılık düzeyleri ve Azure Cosmos DB API’leri
description: Azure Cosmos DB ve Apache Cassandra, MongoDB'deki farklı API'ler arasındaki tutarlılık düzeyi eşlemesini anlama
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.reviewer: sngun
ms.openlocfilehash: ef7d032d37105549ff7b05f85b953cd420954602
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131465"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Azure Cosmos DB API’leri

Azure Cosmos DB, popüler veritabanları için iletişim protokolü uyumlu API'ler için yerel destek sağlar. Bunlar arasında MongoDB, Apache Cassandra, Gremlin ve Azure Tablo depolama alanı yer almaktadır. Bu veritabanları, tutarlılık düzeyleri için tam olarak tanımlanmış tutarlılık modelleri veya SLA destekli garantiler sunmuyor. Bunlar genellikle Azure Cosmos DB tarafından sunulan beş tutarlılık modelinin yalnızca bir alt kümesini sağlar. 

SQL API, Gremlin API ve Table API kullanılırken, Azure Cosmos hesabında yapılandırılan varsayılan tutarlılık düzeyi kullanılır. 

Cassandra API veya Azure Cosmos DB'nin MongoDB api'sini kullanırken, uygulamalar apache Cassandra ve MongoDB tarafından sırasıyla daha güçlü tutarlılık ve dayanıklılık garantileriyle sunulan tam bir tutarlılık düzeyi setine sahip olur. Bu belge, Apache Cassandra ve MongoDB tutarlılık düzeyleri için ilgili Azure Cosmos DB tutarlılık düzeylerini gösterir.

## <a name="mapping-between-apache-cassandra-and-azure-cosmos-db-consistency-levels"></a><a id="cassandra-mapping"></a>Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Azure Cosmos DB'nin aksine, Apache Cassandra yerel olarak tam olarak tanımlanmış tutarlılık garantileri sağlamaz.  Bunun yerine, Apache Cassandra yüksek kullanılabilirlik, tutarlılık ve gecikme dengelerini etkinleştirmek için bir yazma tutarlılık düzeyi ve okuma tutarlılığı düzeyi sağlar. Azure Cosmos DB'nin Cassandra API'si kullanırken: 

* Apache Cassandra'nın yazma tutarlılık düzeyi, Azure Cosmos hesabınızda yapılandırılan varsayılan tutarlılık düzeyine göre eşlenir. Yazma işlemi (CL) için tutarlılık istek başına olarak değiştirilemez.

* Azure Cosmos DB, Cassandra istemci sürücüsü tarafından belirtilen okuma tutarlılık düzeyini, okuma isteğinde dinamik olarak yapılandırılan Azure Cosmos DB tutarlılık düzeylerinden biriyle dinamik olarak eşler. 

Aşağıdaki tablo, cassandra API kullanırken yerel Cassandra tutarlılık düzeylerinin Azure Cosmos DB'nin tutarlılık düzeylerine nasıl eşlenerek eşlendirilebildiğini göstermektedir:  

[![Cassandra tutarlılık modeli haritalama](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png)](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png#lightbox)

## <a name="mapping-between-mongodb-and-azure-cosmos-db-consistency-levels"></a><a id="mongo-mapping"></a>MongoDB ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Azure Cosmos DB'nin aksine, yerel MongoDB tam olarak tanımlanmış tutarlılık garantileri sağlamaz. Bunun yerine, yerel MongoDB kullanıcıların aşağıdaki tutarlılık garantilerini yapılandırmalarına olanak tanır: yazma endişesi, okuma endişesi ve isMaster yönergesi - istenen tutarlılık düzeyine ulaşmak için okuma işlemlerini birincil veya ikincil yinelemelere yönlendirmek için. 

MongoDB için Azure Cosmos DB'nin API'sini kullanırken, MongoDB sürücüsü yazma bölgenizi birincil yineleme olarak ele alır ve diğer tüm bölgeler yineleme okunur. Birincil yineleme olarak Azure Cosmos hesabınızla ilişkili hangi bölgeyi seçebilirsiniz. 

MongoDB için Azure Cosmos DB'nin API'si kullanırken:

* Yazma endişesi, Azure Cosmos hesabınızda yapılandırılan varsayılan tutarlılık düzeyine eşlenir.
 
* Azure Cosmos DB, MongoDB istemci sürücüsü tarafından belirtilen okuma endişesini, okuma isteğinde dinamik olarak yapılandırılan Azure Cosmos DB tutarlılık düzeylerinden biriyle dinamik olarak eşler.  

* Azure Cosmos hesabınızla ilişkili belirli bir bölgeye, bölgeyi ilk yazılabilir bölge haline getirerek "Asıl" olarak ek açıklama yapabilirsiniz. 

Aşağıdaki tablo, Azure Cosmos DB'nin MongoDB için API'sini kullanırken yerel MongoDB yazma/okuma endişelerinin Azure Cosmos tutarlılık düzeylerine nasıl eşlenerek eşlendirilebildiğini göstermektedir:

[![MongoDB tutarlılık modeli eşleme](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png)](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB API'leri ile açık kaynak API'leri arasındaki tutarlılık düzeyleri ve uyumluluk hakkında daha fazla bilgi edinin. Aşağıdaki makalelere bakın:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md)
* [MongoDB özellikleri Azure Cosmos DB'nin MongoDB api'si tarafından desteklenir](mongodb-feature-support.md)
* [Azure Cosmos DB Cassandra API tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)
