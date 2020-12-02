---
title: Azure Data Lake Storage 2. destekleyen Azure hizmetleri | Microsoft Docs
description: Azure Data Lake Storage 2. ile hangi Azure hizmetlerinin tümleştirileceğini öğrenin
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: e988b88f222750bc7de505a24eb44877eea4d966
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452442"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. destekleyen Azure hizmetleri

Azure hizmetlerini kullanarak verileri alabilir, analiz yapabilir ve görsel gösterimler oluşturabilirsiniz. Bu makale, desteklenen Azure hizmetlerinin bir listesini sağlar ve destek düzeylerini kapatır ve bu hizmetleri Azure Data Lake Storage 2. ile kullanmanıza yardımcı olan makalelere bağlantılar sağlar.

## <a name="supported-azure-services"></a>Desteklenen Azure hizmetleri

Bu tabloda Azure Data Lake Storage 2. ile kullanabileceğiniz Azure hizmetleri listelenmektedir. Destek genişlemeye devam ettiğinden, Bu tablolarda görüntülenen öğeler zaman içinde değişir.

> [!NOTE]
> Destek düzeyi yalnızca hizmetin Data Lake Storage Gen 2 ile nasıl desteklendiğini gösterir.

|Azure hizmeti |Destek düzeyi |Azure AD |Paylaşılan Anahtar| İlgili makaleler: |
|---------------|-------------------|---|---|---|
|Azure Data Factory|Genel kullanıma sunuldu|Evet|Evet|[Azure Data Factory ile Azure Data Lake Storage 2. verileri yükleme](../../data-factory/load-azure-data-lake-storage-gen2.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|Genel kullanıma sunuldu|Evet|Evet|[Azure Databricks ile kullanma](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [Hızlı başlangıç: Azure Databricks kullanarak Azure Data Lake Storage 2. verileri çözümleme](data-lake-storage-quickstart-create-databricks-account.md) <br>[Öğretici: Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](/azure/databricks/scenarios/databricks-extract-load-sql-data-warehouse) <br>[Öğretici: Spark kullanarak Azure Databricks Data Lake Storage 2. verilere erişme](data-lake-storage-use-databricks-spark.md)|
|Azure Event Hub|Genel kullanıma sunuldu|Hayır|Evet|[Azure Blob depolama veya Azure Data Lake Storage Azure Event Hubs aracılığıyla olayları yakalama](../../event-hubs/event-hubs-capture-overview.md)|
|Azure Event Grid|Genel kullanıma sunuldu|Evet|Evet|[Öğretici: bir Databricks Delta tablosunu güncelleştirmek için Data Lake Capture modelini uygulama](data-lake-storage-events.md)|
|Azure Logic Apps|Genel kullanıma sunuldu|Hayır|Evet|[Genel Bakış-Azure Logic Apps nedir?](../../logic-apps/logic-apps-overview.md)|
|Azure Machine Learning|Genel kullanıma sunuldu|Evet|Evet|[Azure Storage hizmetlerindeki verilere erişme](../../machine-learning/how-to-access-data.md)|
|Azure Stream Analytics|Genel kullanıma sunuldu|Evet|Evet|[Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](../../stream-analytics/stream-analytics-quick-create-portal.md) <br> [Azure Data Lake Gen2 çıkış](../../stream-analytics/stream-analytics-define-outputs.md)|
|Data Box|Genel kullanıma sunuldu|Hayır|Evet|[Şirket içi bir, Azure depolama 'ya veri geçirmek için Azure Data Box kullanma](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |Genel kullanıma sunuldu|Evet|Evet|[Azure HDInsight kümeleriyle Azure Data Lake Storage 2. Nesil hizmetini kullanma](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[Data Lake Storage 2. ile](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Öğretici: Azure HDInsight üzerinde Apache Hive kullanarak verileri ayıklama, dönüştürme ve yükleme](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT Hub |Genel kullanıma sunuldu|Hayır|Evet|[Farklı uç noktalara cihazdan buluta iletiler göndermek için IoT Hub ileti yönlendirmeyi kullanma](../../iot-hub/iot-hub-devguide-messages-d2c.md)|
|Power BI|Genel kullanıma sunuldu|Evet|Evet|[Power BI kullanarak Data Lake Storage 2. verileri çözümleme](/power-query/connectors/datalakestorage)|
|Azure Synapse Analytics (eski adı SQL Veri Ambarı)|Genel kullanıma sunuldu|Evet|Evet|[Azure SYNAPSE Analytics ile kullanma](../../azure-sql/database/vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|SQL Server Integration Services (SSIS)|Genel kullanıma sunuldu|Evet|Evet|[Azure depolama bağlantı Yöneticisi](/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|
|Azure Veri Gezgini|Genel kullanıma sunuldu|Evet|Evet|[Azure Data Lake Azure Veri Gezgini kullanarak verileri sorgulama](/azure/data-explorer/data-lake-query-data)|
|Azure Bilişsel Arama|Önizleme|Evet|Evet|[Azure Data Lake Storage 2. belgelerini dizin ve arama (Önizleme)](../../search/search-howto-index-azure-data-lake-storage.md)|
|Azure Content Delivery Network|Henüz desteklenmiyor|Geçerli değil|Geçerli değil|[Azure Data Lake Storage 2. belgelerini dizin ve arama (Önizleme)](../../cdn/cdn-overview.md)|

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. ile ilgili bilinen sorunlar](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage 2. bulunan BLOB depolama özellikleri](data-lake-storage-supported-blob-storage-features.md)
- [Azure Data Lake Storage 2. destekleyen açık kaynaklı platformlar](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage çoklu protokol erişimi](data-lake-storage-multi-protocol-access.md)