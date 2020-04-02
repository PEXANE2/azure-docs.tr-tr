---
title: "Azure Databricks'ten farklı veri kaynaklarına bağlanma "
description: Azure Veri Tuğlaları'ndan Azure SQL Veritabanı, Azure Veri Gölü Deposu, blob depolama, Cosmos DB, Etkinlik Hub'ları ve Azure SQL Veri Ambarı'na nasıl bağlanışları öğrenin.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mamccrea
ms.openlocfilehash: 79a821a4c8fe4cb2d048f0dcb0a6e091462a1779
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548803"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Azure Veri Tuğlaları'ndan veri kaynaklarına bağlanma

Bu makalede, Azure'daki Azure Veri Tuğlaları'na bağlanabilen tüm farklı veri kaynaklarına bağlantılar sağlanmaktadır. Azure veri kaynaklarından (örneğin, Azure Blob Depolama, Azure Etkinlik Hub'ları, vb.) bir Azure Veri Tuğlaları kümesine veri ayıklamak ve üzerlerinde analitik işler yürütmek için bu bağlantılardaki örnekleri izleyin. 

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure Databricks çalışma alanınız ve Bir Kıvılcım kümeniz olmalıdır. [Azure Databricks ile başlayın'daki](quickstart-create-databricks-workspace-portal.md)yönergeleri izleyin.

## <a name="data-sources-for-azure-databricks"></a>Azure Veri Tuğlaları için veri kaynakları

Aşağıdaki liste, Azure'da Azure Veri Tuğlaları ile kullanabileceğiniz veri kaynaklarını sağlar. Azure Veri Tuğlaları ile kullanılabilecek veri kaynaklarının tam listesi [için Azure Veri Tuğlaları için Veri kaynaklarına](/azure/databricks/data/data-sources/index)bakın.

- [Azure SQL veritabanı](/azure/databricks/data/data-sources/sql-databases)

    Bu bağlantı, JDBC kullanarak SQL veritabanlarına bağlanmak ve JDBC arabirimi üzerinden okumaların paralelliğini denetlemek için DataFrame API'sini sağlar. Bu konu, scala API kullanarak ayrıntılı örnekler sağlar, sonunda kısaltılmış Python ve Spark SQL örnekleri.
- [Azure Data Lake Storage](/azure/databricks/data/data-sources/azure/azure-datalake-gen2)

    Bu bağlantı, Azure Veri Gölü Depolama ile kimlik doğrulaması yapmak için Azure Etkin Dizin hizmet ilkesinin nasıl kullanılacağına ilişkin örnekler sağlar. Ayrıca, Azure Veri Verileri Depolama'daki verilere Azure Veri Tuğlaları'ndan nasıl erişilene ilişkin talimatlar da sağlar.

- [Azure Blob Depolama](/azure/databricks/data/data-sources/azure/azure-storage)

    Bu bağlantı, belirli bir kapsayıcı için erişim anahtarını veya SAS'ı kullanarak Azure Veri Tuğlaları'ndan Azure Blob Depolamasına doğrudan nasıl erişileceğiniz hakkında örnekler sağlar. Bağlantı ayrıca RDD API'sini kullanarak Azure Veri Tuğlaları'ndan Azure Blob Depolamasına nasıl erişilene ilişkin bilgiler de sağlar.

- [Azure Cosmos DB](/azure/databricks/data/data-sources/azure/cosmosdb-connector)

    Bu bağlantı, Azure Cosmos DB'deki verilere erişmek için Azure Databricks'teki [Azure Cosmos DB Spark konektörünü](https://github.com/Azure/azure-cosmosdb-spark) nasıl kullanacağımız hakkında yönergeler sağlar.

- [Azure Event Hubs](/azure/event-hubs/event-hubs-spark-connector)

    Bu bağlantı, Azure Etkinlik Hub'larında verilere erişmek için Azure Veri Tuğlaları'ndaki [Azure Etkinlik Hub'larının Kıvılcım bağlayıcısının](https://github.com/Azure/azure-event-hubs-spark) nasıl kullanılacağına ilişkin yönergeler sağlar.

- [Azure SQL Veri Ambarı](/azure/synapse-analytics/sql-data-warehouse/)

    Bu bağlantı, Azure Veri Tuğlaları'ndan bağlanmak için Azure SQL Veri Ambarı bağlayıcısının nasıl kullanılacağına ilişkin yönergeler sağlar.
    

## <a name="next-steps"></a>Sonraki adımlar

Verileri Azure Databricks'e aktarabileceğiniz kaynaklar hakkında bilgi edinmek [için Azure Veri Tuğlaları için Veri kaynaklarına](/azure/databricks/data/data-sources/index)bakın.


