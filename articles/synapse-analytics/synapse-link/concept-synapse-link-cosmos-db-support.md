---
title: Desteklenen Azure Cosmos DB özellikler için Azure SYNAPSE link (Önizleme)
description: Azure Cosmos DB için Azure SYNAPSE bağlantısı tarafından desteklenen eylemlerin güncel listesini anlayın
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/21/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 2e0b1ee5584420ab38fda8897ef610794b09c29a
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83658847"
---
# <a name="azure-synapse-link-preview-for-azure-cosmos-db-supported-features"></a>Desteklenen Azure Cosmos DB özellikler için Azure SYNAPSE link (Önizleme)

Bu makalede, şu anda Azure Cosmos DB için Azure SYNAPSE bağlantısında desteklenen işlevler açıklanmaktadır. 

## <a name="azure-synapse-support"></a>Azure SYNAPSE desteği

Azure Cosmos DB iki tür kapsayıcı vardır:
* HTAP kapsayıcısı-SYNAPSE bağlantısı etkinleştirilmiş bir kapsayıcı. Bu kapsayıcıda hem işlem deposu hem de analitik depo vardır. 
* OLTP kapsayıcısı-yalnızca işlem deposuna sahip bir kapsayıcı; SYNAPSE bağlantısı etkin değil. 

SYNAPSE bağlantısını etkinleştirmeden bir Azure Cosmos DB kapsayıcısına bağlanabilirsiniz, bu durumda yalnızca işlem deposuna okuma/yazma yapabilirsiniz. Azure Cosmos DB için SYNAPSE link içindeki şu anda desteklenen özelliklerin bir listesi aşağıdadır. 

| Kategori              | Açıklama |[Spark](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) | [SQL sunucusuz](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------- |
| **Çalışma zamanı desteği** |Azure SYNAPSE çalışma zamanına göre okuma veya yazma desteği| ✓ | [Bize Ulaşın](mailto:AskSynapse@microsoft.com?subject=[Enable%20Preview%20Feature]%20SQL%20serverless%20for%20Cosmos%20DB)|
| **Azure Cosmos DB API desteği** |SYNAPSE bağlantısı olarak API desteği| SQL/MongoDB | SQL/MongoDB |
| **Nesne**  |Oluşturulabilen ve Azure Cosmos DB kapsayıcıya işaret eden bir tablo gibi nesneler| Görünüm, tablo | Görüntüle |
| **Okuyamaz**    |Azure Cosmos DB kapsayıcısından verileri okuma| OLTP/HTAP | HTAP  |
| **Yazarken**   |Çalışma zamanından Azure Cosmos DB kapsayıcısına veri yazma| OLTP | yok |

* Spark 'tan bir Azure Cosmos DB kapsayıcısına veri yazarsanız, bu işlem Azure Cosmos DB işlem deposu aracılığıyla gerçekleşir ve Istek birimleri tüketerek Azure Cosmos DB işlem performansını etkiler.
* Dış tablolar aracılığıyla SQL havuzu tümleştirmesi Şu anda desteklenmiyor.

## <a name="supported-code-generated-actions-for-spark"></a>Spark için desteklenen kod tarafından oluşturulan eylemler

| Hareket              | Açıklama |OLTP |HTAP  |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **DataFrame 'e yükle** |Spark DataFrame 'e veri yükleme ve verileri okuma |X| ✓ |
| **Spark tablosu oluşturma** |Azure Cosmos DB kapsayıcısına işaret eden tablo oluşturma|X| ✓ |
| **Veri çerçevesini kapsayıcıya yaz** |Bir kapsayıcıya veri yazma|✓| ✓ |
| **Kapsayıcıdan akış veri çerçevesini yükleme** |Azure Cosmos DB değişiklik akışını kullanarak veri akışı|✓| ✓ |
| **Akış veri çerçevesini kapsayıcıya yaz** |Azure Cosmos DB değişiklik akışını kullanarak veri akışı|✓| ✓ |



## <a name="supported-code-generated-actions-for-sql-serverless"></a>SQL sunucusuz için desteklenen kod tarafından oluşturulan eylemler

| Hareket              | Açıklama |OLTP |HTAP |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **En üstteki 100 seçin** |Bir kapsayıcıdan ilk 100 öğe önizleme|X| ✓ |
| **Görünüm Oluştur** |SYNAPSE SQL aracılığıyla doğrudan bir kapsayıcıda bı erişimine sahip olmak için bir görünüm oluşturma|X| ✓ |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB için bkz. SYNAPSE bağlantısına bağlanma](../quickstart-connect-synapse-link-cosmos-db.md)
* [Spark ile analitik depoyu sorgulama hakkında bilgi edinin](how-to-query-analytical-store-spark.md)