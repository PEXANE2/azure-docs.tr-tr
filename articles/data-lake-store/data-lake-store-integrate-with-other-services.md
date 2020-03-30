---
title: Azure Veri Gölü Depolama Gen1'i diğer Azure Hizmetleriyle Tümleştirme | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1'in diğer Azure hizmetleriyle nasıl tümleştirinceklerini anlama
documentationcenter: ''
services: data-lake-store
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 43024b63a355646f607adbb6623cc6c349374ea8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535604"
---
# <a name="integrating-azure-data-lake-storage-gen1-with-other-azure-services"></a>Azure Veri Gölü Depolama Gen1'i diğer Azure hizmetleriyle tümleştirme
Azure Veri Gölü Depolama Gen1, daha geniş bir senaryo aralığı sağlamak için diğer Azure hizmetleriyle birlikte kullanılabilir. Aşağıdaki makalede, Data Lake Storage Gen1 ile entegre edilebilen hizmetler listelenebilmiştir.

## <a name="use-data-lake-storage-gen1-with-azure-hdinsight"></a>Azure HDInsight ile Veri Gölü Depolama Gen1'i kullanma
HDFS uyumlu depolama alanı olarak Veri Gölü Depolama Gen1'i kullanan bir [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) kümesi sağlayabilirsiniz. Bu sürüm için, Windows ve Linux'taki Hadoop ve Storm kümeleri için Data Lake Storage Gen1'i yalnızca ek depolama alanı olarak kullanabilirsiniz. Bu tür kümeler varsayılan depolama alanı olarak Azure Depolama'yı (WASB) kullanmaya devam eder. Ancak, Windows ve Linux'taki HBase kümeleri için Varsayılan depolama alanı veya ek depolama alanı veya her ikisini de olarak Veri Gölü Depolama Gen1'i kullanabilirsiniz.

Data Lake Storage Gen1 ile hdinsight kümesinin nasıl sağlanacak ile ilgili talimatlar için bkz:

* [Azure Portalı kullanarak Veri Gölü Depolama Gen1 ile hdinsight kümesi sağlama](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure PowerShell'i kullanarak varsayılan depolama alanı olarak Veri Gölü Depolama Gen1 ile hdinsight kümesini sağlama](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Azure PowerShell kullanarak ek depolama alanı olarak Data Lake Storage Gen1 ile hdinsight kümesini sağlama](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-storage-gen1-with-azure-data-lake-analytics"></a>Azure Veri Gölü Analizi ile Veri Gölü Depolama Gen1'i kullanma
[Azure Veri Gölü Analizi,](../data-lake-analytics/data-lake-analytics-overview.md) Bulut ölçeğinde Büyük Verilerle çalışmanıza olanak tanır. Kaynakları dinamik olarak karşılar ve terabaytlar ve hatta eksabaytlarca veri eksabaytları üzerinde analiz yapmanızı sağlar ve bunlardan biri Data Lake Storage Gen1 olmak üzere desteklenen veri kaynaklarında depolanabilir. Data Lake Analytics, siz büyük veri iş yükleri için en yüksek performans, iş ortası ve paralelleştirme sağlayan Data Lake Storage Gen1 ile çalışmak üzere özel olarak optimize edecektir.

Data Lake Storage Gen1 ile Data Lake Analytics'in nasıl kullanılacağına ilişkin talimatlar için [bkz.](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="use-data-lake-storage-gen1-with-azure-data-factory"></a>Azure Veri Fabrikası ile Veri Gölü Depolama Gen1'i kullanma
Azure tablolarından, Azure SQL Veritabanı'ndan, Azure SQL Veri Ambarından, Azure Depolama Blob'larından ve şirket içi veritabanlarından veri almak için [Azure Veri Fabrikası'nı](https://azure.microsoft.com/services/data-factory/) kullanabilirsiniz. Azure veri fabrikasında birinci sınıf bir vatandaş olan Azure Veri Fabrikası, bu kaynaktan Veri Gölü Depolama Gen1'e veri alımını düzenlemek için kullanılabilir.

Veri Gölü Depolama Gen1 ile Azure Veri Fabrikası'nın nasıl kullanılacağına ilişkin talimatlar için, [Veri Fabrikası'nı kullanarak Verileri Veri Gölü Depolama Gen1'e taşıyın'](../data-factory/connector-azure-data-lake-store.md)a bakın.

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-storage-gen1"></a>Azure Depolama Blobs'daki verileri Veri Gölü Depolama Gen1'e kopyalama
Azure Veri Gölü Depolama Gen1, Azure Blob Depolama'daki verileri bir Veri Gölü Depolama Gen1 hesabına kopyalamanızı sağlayan komut satırı aracı AdlCopy'i sağlar. Daha fazla bilgi için bkz: [Azure Depolama Blobs'tan Veri Gölü Depolama Gen1'e veri kopyalama.](data-lake-store-copy-data-azure-storage-blob.md)

## <a name="copy-data-between-azure-sql-database-and-data-lake-storage-gen1"></a>Azure SQL Veritabanı ve Veri Gölü Depolama Gen1 arasındaki verileri kopyalama
Azure SQL Veritabanı ve Veri Gölü Depolama Gen1 arasında veri almak ve aktarmak için Apache Sqoop'u kullanabilirsiniz. Daha fazla bilgi için [bkz.](data-lake-store-data-transfer-sql-sqoop.md)

## <a name="use-data-lake-storage-gen1-with-stream-analytics"></a>Stream Analytics ile Veri Gölü Depolama Gen1'i kullanma
Azure Akış Analitiği kullanılarak aktarılan verileri depolamak için veri gölü gen1'i çıktılardan biri olarak kullanabilirsiniz. Daha fazla bilgi için Azure [Depolama Blob'undaki Akış verilerini Azure Akış Analitiği'ni kullanarak Veri Gölü Depolama Gen1'e](data-lake-store-stream-analytics.md)bakın.

## <a name="use-data-lake-storage-gen1-with-power-bi"></a>Power BI ile Veri Gölü Depolama Gen1 kullanın
Verileri analiz etmek ve görselleştirmek için Bir Veri Gölü Depolama Gen1 hesabından veri almak için Power BI'yi kullanabilirsiniz. Daha fazla bilgi için bkz. [Power BI kullanarak Veri Gölü Depolama Gen1'deki verileri analiz](data-lake-store-power-bi.md)edin.

## <a name="use-data-lake-storage-gen1-with-data-catalog"></a>Veri Kataloğu ile Veri Gölü Depolama Gen1 kullanın
Verileri kuruluş genelinde bulunabilir hale getirmek için Veri Gölü Depolama Gen1'deki verileri Azure Veri Kataloğu'na kaydedebilirsiniz. Daha fazla bilgi için Azure [Veri Kataloğu'nda Veri Gölü Depolama Gen1'den veri kaydedin.](data-lake-store-with-data-catalog.md)

## <a name="use-data-lake-storage-gen1-with-sql-server-integration-services-ssis"></a>SQL Server Entegrasyon Hizmetleri (SSIS) ile Veri Gölü Depolama Gen1'i Kullanma
SSIS'teki Data Lake Storage Gen1 bağlantı yöneticisini kullanarak Bir SSIS paketini Data Lake Storage Gen1'e bağlayabilirsiniz. Daha fazla bilgi için Bkz. [SSIS ile Veri Gölü Depolama Gen1 kullanın.](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager)

## <a name="use-data-lake-storage-gen1-with-sql-data-warehouse"></a>SQL Veri Ambarı ile Veri Gölü Depolama Gen1'i Kullanma
Veri Gölü Depolama Gen1'den verileri SQL Veri Ambarı'na yüklemek için PolyBase'i kullanabilirsiniz. Daha fazla bilgi için sql [veri ambarı ile Veri Gölü Depolama Gen1 kullanın](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md)bakın.

## <a name="use-data-lake-storage-gen1-with-azure-event-hubs"></a>Azure Etkinlik Hub'larıyla Veri Gölü Depolama Gen1'i kullanma
Azure Etkinlik Hub'ları tarafından alınan verileri arşivlemek ve yakalamak için Azure Veri Gölü Depolama Gen1'i kullanabilirsiniz. Daha fazla bilgi için Azure [Etkinlik Hub'ları ile Veri Gölü Depolama Gen1'i kullanın.](data-lake-store-archive-eventhub-capture.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
* [Portal kullanarak Veri Gölü Depolama Gen1 ile başlayın](data-lake-store-get-started-portal.md)
* [PowerShell kullanarak Data Lake Storage Gen1 ile başlayın](data-lake-store-get-started-powershell.md)  

