---
title: Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetleri | Microsoft Dokümanlar
description: Hangi Azure hizmetlerinin Azure Veri Gölü Depolama Gen2 ile tümleştirildiği hakkında bilgi edinin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: edfcb1d0bfc87c84620b13ed26ec681fef32f1e3
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878332"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'yi destekleyen Azure hizmetleri

Azure hizmetlerini veri almak, analitik gerçekleştirmek ve görsel gösterimler oluşturmak için kullanabilirsiniz. Bu makale, desteklenen Azure hizmetlerinin bir listesini sağlar, destek düzeylerini açıklar ve azure veri gölü depolama gen2 ile bu hizmetleri kullanmanıza yardımcı olan makalelere bağlantılar sağlar.

## <a name="supported-azure-services"></a>Desteklenen Azure hizmetleri

Bu tablo, Azure Veri Gölü Depolama Gen2 ile kullanabileceğiniz Azure hizmetlerini listeler. Destek genişlemeye devam ettikçe, bu tablolarda görünen öğeler zaman içinde değişecektir.

> [!NOTE]
> Destek düzeyi yalnızca hizmetin Data Lake Storage Gen 2 ile nasıl desteklendirilebildiğini ifade eder.

|Azure hizmeti |Destek düzeyi |İlgili makaleler: |
|---------------|-------------------|---|
|Azure Data Factory|Genel kullanıma sunuldu|[Azure Veri Fabrikası ile Verileri Azure Veri Gölü Depolama Gen2'ye yükleyin](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|Genel kullanıma sunuldu|[Azure Databricks ile kullanma](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [Hızlı başlatma: Azure Veri Tuğlaları kullanarak Azure Veri Gölü Depolama Gen2'deki verileri analiz edin](data-lake-storage-quickstart-create-databricks-account.md) <br>[Öğretici: Azure Databricks'i kullanarak verileri ayıklayın, dönüştürün ve yükleyin](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse) <br>[Öğretici: Spark kullanarak Azure Databricks ile Veri Gölü Depolama Gen2 verilerine erişin](data-lake-storage-use-databricks-spark.md)|
|Azure Event Hub|Genel kullanıma sunuldu|[Azure Blob Depolama veya Azure Veri Gölü Depolamasında Azure Etkinlik Hub'ları aracılığıyla olayları yakalama](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Azure Event Grid|Genel kullanıma sunuldu|[Öğretici: Databricks Delta tablosunu güncelleştirmek için veri gölü yakalama deseni uygulayın](data-lake-storage-events.md)|
|Azure Logic Apps|Genel kullanıma sunuldu|[Genel Bakış - Azure Mantık Uygulamaları Nedir?](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)|
|Azure Machine Learning|Genel kullanıma sunuldu|[Azure depolama hizmetlerinde verilere erişin](https://docs.microsoft.com/azure/machine-learning/how-to-access-data)|
|Azure Stream Analytics|Genel kullanıma sunuldu|[Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Azure Veri Gölü Gen2'ye Çıkış](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|Data Box|Genel kullanıma sunuldu|[Verileri şirket içi BIR HDFS deposundan Azure Depolama'ya geçirmek için Azure Veri Kutusu'ni kullanma](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |Genel kullanıma sunuldu|[Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[Veri Gölü Depolama Gen2 ile HDFS CLI kullanma](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Öğretici: Azure HDInsight'ta Apache Hive kullanarak verileri ayıklayın, dönüştürün ve yükleyin](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT Hub |Genel kullanıma sunuldu|[Aygıttan buluta iletileri farklı uç noktalara göndermek için IoT Hub ileti yönlendirmesini kullanma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|
|Power BI|Genel kullanıma sunuldu|[Power BI kullanarak Veri Gölü Depolama Gen2'deki verileri analiz edin](https://docs.microsoft.com/power-query/connectors/datalakestorage)|
|SQL Veri Ambarı|Genel kullanıma sunuldu|[Azure SQL Veri Ambarı ile kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-sql-data-warehouse-polybase)|
|SQL Server Integration Services (SSIS)|Genel kullanıma sunuldu|[Azure Depolama bağlantı yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|
|Azure Bilişsel Arama|Önizleme|[Azure Veri Gölü Depolama Gen2 belgelerini dizine ve aramasına (önizleme)](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|
|Azure Veri Gezgini|Önizleme|[Azure Veri Gezgini'ni kullanarak Azure Veri Gölü'ndeki verileri sorgula](https://docs.microsoft.com/azure/data-explorer/data-lake-query-data)|
|Azure Content Delivery Network|Henüz desteklenmedi|[Azure Veri Gölü Depolama Gen2 belgelerini dizine ve aramasına (önizleme)](https://docs.microsoft.com/azure/cdn/cdn-overview)|


## <a name="see-also"></a>Ayrıca bkz.

- [Azure Veri Gölü Depolama Gen2 ile bilinen sorunlar](data-lake-storage-known-issues.md)
- [Blob depolama özellikleri Azure Veri Gölü Depolama Gen2'de kullanılabilir](data-lake-storage-supported-blob-storage-features.md)
- [Azure Veri Gölü Depolama Gen2'yi destekleyen açık kaynak platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Veri Gölü Depolama'da çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)