---
title: Azure Data Lake Storage 2. destekleyen Azure hizmetleri | Microsoft Docs
description: Azure Data Lake Storage 2. ile hangi Azure hizmetlerinin tümleştirileceğini öğrenin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: edfcb1d0bfc87c84620b13ed26ec681fef32f1e3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80878332"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. destekleyen Azure hizmetleri

Azure hizmetlerini kullanarak verileri alabilir, analiz yapabilir ve görsel gösterimler oluşturabilirsiniz. Bu makale, desteklenen Azure hizmetlerinin bir listesini sağlar ve destek düzeylerini kapatır ve bu hizmetleri Azure Data Lake Storage 2. ile kullanmanıza yardımcı olan makalelere bağlantılar sağlar.

## <a name="supported-azure-services"></a>Desteklenen Azure hizmetleri

Bu tabloda Azure Data Lake Storage 2. ile kullanabileceğiniz Azure hizmetleri listelenmektedir. Destek genişlemeye devam ettiğinden, Bu tablolarda görüntülenen öğeler zaman içinde değişir.

> [!NOTE]
> Destek düzeyi yalnızca hizmetin Data Lake Storage Gen 2 ile nasıl desteklendiğini gösterir.

|Azure hizmeti |Destek düzeyi |İlgili makaleler: |
|---------------|-------------------|---|
|Azure Data Factory|Genel kullanıma sunuldu|[Azure Data Factory ile Azure Data Lake Storage 2. verileri yükleme](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|Genel kullanıma sunuldu|[Azure Databricks ile kullanma](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [Hızlı başlangıç: Azure Databricks kullanarak Azure Data Lake Storage 2. verileri çözümleme](data-lake-storage-quickstart-create-databricks-account.md) <br>[Öğretici: Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse) <br>[Öğretici: Spark kullanarak Azure Databricks Data Lake Storage 2. verilere erişme](data-lake-storage-use-databricks-spark.md)|
|Azure Event Hub|Genel kullanıma sunuldu|[Azure Blob depolama veya Azure Data Lake Storage Azure Event Hubs aracılığıyla olayları yakalama](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Azure Event Grid|Genel kullanıma sunuldu|[Öğretici: bir Databricks Delta tablosunu güncelleştirmek için Data Lake Capture modelini uygulama](data-lake-storage-events.md)|
|Azure Logic Apps|Genel kullanıma sunuldu|[Genel Bakış-Azure Logic Apps nedir?](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)|
|Azure Machine Learning|Genel kullanıma sunuldu|[Azure Storage hizmetlerindeki verilere erişme](https://docs.microsoft.com/azure/machine-learning/how-to-access-data)|
|Azure Stream Analytics|Genel kullanıma sunuldu|[Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Azure Data Lake Gen2 çıkış](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|Data Box|Genel kullanıma sunuldu|[Şirket içi bir, Azure depolama 'ya veri geçirmek için Azure Data Box kullanma](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |Genel kullanıma sunuldu|[Azure HDInsight kümeleri ile Azure Data Lake Storage 2. kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[Data Lake Storage 2. ile](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Öğretici: Azure HDInsight üzerinde Apache Hive kullanarak verileri ayıklama, dönüştürme ve yükleme](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT Hub |Genel kullanıma sunuldu|[Farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|
|Power BI|Genel kullanıma sunuldu|[Power BI kullanarak Data Lake Storage 2. verileri çözümleme](https://docs.microsoft.com/power-query/connectors/datalakestorage)|
|SQL Veri Ambarı|Genel kullanıma sunuldu|[Azure SQL Veri Ambarı ile kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-sql-data-warehouse-polybase)|
|SQL Server Integration Services (SSIS)|Genel kullanıma sunuldu|[Azure depolama bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|
|Azure Bilişsel Arama|Önizleme|[Azure Data Lake Storage 2. belgelerini dizin ve arama (Önizleme)](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|
|Azure Veri Gezgini|Önizleme|[Azure Data Lake Azure Veri Gezgini kullanarak verileri sorgulama](https://docs.microsoft.com/azure/data-explorer/data-lake-query-data)|
|Azure Content Delivery Network|Henüz desteklenmiyor|[Azure Data Lake Storage 2. belgelerini dizin ve arama (Önizleme)](https://docs.microsoft.com/azure/cdn/cdn-overview)|


## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri](data-lake-storage-supported-blob-storage-features.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)