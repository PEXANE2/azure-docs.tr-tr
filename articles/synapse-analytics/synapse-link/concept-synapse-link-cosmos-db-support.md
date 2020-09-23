---
title: Desteklenen Azure Cosmos DB özellikler için Azure SYNAPSE link (Önizleme)
description: Azure Cosmos DB için Azure SYNAPSE bağlantısı tarafından desteklenen eylemlerin güncel listesini anlayın
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: synapse-link
ms.date: 09/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 336409b8b6f804b224b87d5fb11fded0654b8619
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90895523"
---
# <a name="azure-synapse-link-preview-for-azure-cosmos-db-supported-features"></a>Desteklenen Azure Cosmos DB özellikler için Azure SYNAPSE link (Önizleme)

Bu makalede Azure Cosmos DB için Azure Synapse Link'te desteklenen işlevler anlatılmıştır.

## <a name="azure-synapse-support"></a>Azure SYNAPSE desteği

Azure Cosmos DB iki tür kapsayıcı vardır:
* HTAP kapsayıcısı-SYNAPSE bağlantısı etkinleştirilmiş bir kapsayıcı. Bu kapsayıcıda hem işlem deposu hem de analitik depo vardır. 
* OLTP kapsayıcısı-Synaspe bağlantısı olan bir kapsayıcı etkin değil. Bu kapsayıcıda yalnızca işlem deposu vardır ve analitik depo yoktur.

> [!IMPORTANT]
> Azure Cosmos DB için Azure SYNAPSE bağlantısı şu anda yönetilen sanal ağ etkinleştirilmemiş SYNAPSE çalışma alanlarında destekleniyor. 

SYNAPSE bağlantısını etkinleştirmeden bir Azure Cosmos DB kapsayıcısına bağlanabilirsiniz, bu durumda yalnızca işlem deposuna okuma/yazma yapabilirsiniz. Aşağıda, Azure Cosmos DB için SYNAPSE link içindeki şu anda desteklenen özelliklerin listesi verilmiştir. 

| Kategori              | Açıklama |[Spark](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) | [SQL sunucusuz](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- | ----------------------------------------------------------- |
| **Çalışma zamanı desteği** |Azure Cosmos DB erişmek için desteklenen Azure SYNAPSE çalışma zamanı| ✓ | [Bizimle iletişime geçin](mailto:cosmosdbsynapselink@microsoft.com?subject=[Enable%20Preview%20Feature]%20SQL%20serverless%20for%20Cosmos%20DB) |
| **Azure Cosmos DB API desteği** | Desteklenen Azure Cosmos DB API türü | SQL/MongoDB | SQL/MongoDB |
| **Nesne**  |Oluşturulabilen ve Azure Cosmos DB kapsayıcıya işaret eden bir tablo gibi nesneler| Veri çerçevesi, görünüm, tablo | Görüntüle |
| **Okuyamaz**    | Okunabilecek Azure Cosmos DB kapsayıcısı türü | OLTP/HTAP | HTAP  |
| **Yazarken**   | Azure Cosmos DB kapsayıcısına veri yazmak için Azure SYNAPSE çalışma zamanı kullanılabilir | Yes | Hayır |

* Spark 'tan bir Azure Cosmos DB kapsayıcısına veri yazarsanız, bu, Azure Cosmos DB işlem deposundan oluşur ve Azure Cosmos DB ve Istek birimlerindeki işlem iş yüklerinin performansını etkiler.
* Dış tablolar aracılığıyla SYNAPSE SQL havuzu tümleştirmesi Şu anda desteklenmiyor.

## <a name="supported-code-generated-actions-for-spark"></a>Spark için desteklenen kod tarafından oluşturulan eylemler

| Hareket              | Açıklama |OLTP |HTAP  |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **DataFrame 'e yükle** |Spark DataFrame 'e veri yükleme ve verileri okuma |✓| ✓ |
| **Spark tablosu oluşturma** |Azure Cosmos DB kapsayıcısına işaret eden tablo oluşturma|✓| ✓ |
| **Veri çerçevesini kapsayıcıya yaz** |Bir kapsayıcıya veri yazma|✓| ✓ |
| **Kapsayıcıdan akış veri çerçevesini yükleme** |Azure Cosmos DB değişiklik akışını kullanarak veri akışı|✓| ✓ |
| **Akış veri çerçevesini kapsayıcıya yaz** |Azure Cosmos DB değişiklik akışını kullanarak veri akışı|✓| ✓ |



## <a name="supported-code-generated-actions-for-sql-serverless"></a>SQL sunucusuz için desteklenen kod tarafından oluşturulan eylemler

| Hareket              | Açıklama |OLTP |HTAP |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **Verileri inceleme** |Tanıdık T-SQL söz dizimi ve otomatik Şema çıkarımı ile bir kapsayıcıdaki verileri araştırma|X| ✓ |
| **Görünümler oluşturma ve bı raporları oluşturma** |BI ile SYNAPSE SQL sunucusuz için bir kapsayıcıya doğrudan erişim sağlamak üzere bir SQL görünümü oluşturun |X| ✓ |
| **Cosmos DB verilerle birlikte farklı veri kaynaklarını birleştirin** | Azure Blob depolama 'daki verilerle birlikte Cosmos DB kapsayıcılarından ve CETAS kullanarak Azure Data Lake Storage veri okuma sonuçlarını depolayın |X| ✓ |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB için bkz. SYNAPSE bağlantısına bağlanma](../quickstart-connect-synapse-link-cosmos-db.md)
* [Spark ile analitik depoyu sorgulama hakkında bilgi edinin](how-to-query-analytical-store-spark.md)
