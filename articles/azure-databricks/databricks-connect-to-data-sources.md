---
title: 'Azure Databricks farklı veri kaynaklarına bağlanma '
description: Azure Databricks Azure SQL veritabanı, Azure Data Lake Store, BLOB depolama, Cosmos DB, Event Hubs ve Azure SQL veri ambarı 'na nasıl bağlanacağınızı öğrenin.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mamccrea
ms.openlocfilehash: 9b44db3e8ffc02d211f7f97404f0cdd8d319fe03
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72597501"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Azure Databricks veri kaynaklarına bağlanma

Bu makalede, Azure 'daki Azure Databricks bağlı olabilecek tüm farklı veri kaynaklarına bağlantılar sağlanmaktadır. Bu bağlantılardaki örnekleri izleyerek Azure veri kaynaklarından (örneğin, Azure Blob depolama, Azure Event Hubs, vb.) verileri bir Azure Databricks kümesine ayıklayın ve analitik işleri bunlar üzerinde çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Databricks çalışma alanınız ve Spark kümeniz olmalıdır. [Azure Databricks kullanmaya başlama](quickstart-create-databricks-workspace-portal.md)yönergelerini izleyin.

## <a name="data-sources-for-azure-databricks"></a>Azure Databricks için veri kaynakları

Aşağıdaki liste, Azure Databricks ile kullanabileceğiniz Azure 'daki veri kaynaklarını sağlar. Azure Databricks ile kullanılabilecek veri kaynaklarının tamamen listesi için bkz. [Azure Databricks Için veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- [Azure SQL veritabanı](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    Bu bağlantı, JDBC kullanarak SQL veritabanlarına bağlanmak için veri çerçevesi API 'sini ve JDBC arabirimi aracılığıyla okumaların paralelliğini denetleme olanağı sağlar. Bu konu, sonunda kısaltılmış Python ve Spark SQL örnekleri ile Scala API 'sini kullanarak ayrıntılı örnekler sağlar.
- [Azure Data Lake Storage](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)

    Bu bağlantı, Azure Data Lake Storage kimlik doğrulamak için Azure Active Directory hizmet sorumlusunu kullanma hakkında örnekler sağlar. Ayrıca, Azure Databricks Azure Data Lake Storage verilere nasıl erişileceğini gösteren yönergeler de sağlar.

- [Azure Blob Depolama](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    Bu bağlantı, belirli bir kapsayıcı için erişim anahtarını veya SAS 'yi kullanarak Azure Databricks Azure Blob depolamaya doğrudan erişme hakkında örnekler sağlar. Bağlantı Ayrıca, RDD API kullanarak Azure Databricks Azure Blob depolamaya nasıl erişeceğimiz hakkında bilgi de sağlar.

- [Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    Bu bağlantı, Azure Cosmos DB verilere erişmek için Azure Databricks [Azure Cosmos DB Spark bağlayıcısının](https://github.com/Azure/azure-cosmosdb-spark) nasıl kullanılacağına ilişkin yönergeler sağlar.

- [Azure Event Hubs](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    Bu bağlantı, Azure Event Hubs 'deki verilere erişmek için Azure Databricks [azure Event Hubs Spark bağlayıcısının](https://github.com/Azure/azure-event-hubs-spark) nasıl kullanılacağına ilişkin yönergeler sağlar.

- [Azure SQL Veri Ambarı](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    Bu bağlantı, Azure Databricks bağlantı kurmak için Azure SQL veri ambarı bağlayıcısının nasıl kullanılacağına ilişkin yönergeler sağlar.
    

## <a name="next-steps"></a>Sonraki adımlar

Azure Databricks verileri içeri aktarabileceğiniz kaynaklar hakkında bilgi edinmek için bkz. [Azure Databricks Için veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#).


