---
title: Azure Veri Gölü Depolama Gen2 ile ilgili veri senaryoları | Microsoft Dokümanlar
description: Veri Gölü Depolama Gen2'de (önceden Azure Veri Gölü Deposu olarak bilinir) hangi verilerin yutulabileceğini, işlenebilir, indirilebilir ve görüntülenebileceğini kullanarak farklı senaryoları ve araçları anlama
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: b0ebe6cb505fa2a145dd3cbb94398912f2933a4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77369703"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen2'yi kullanma

Büyük veri işlemede dört temel aşama vardır:

> [!div class="checklist"]
> * Büyük miktarda veriyi gerçek zamanlı veya toplu olarak veri deposuna alma
> * Verileri işleme
> * Verileri indirme
> * Verileri görselleştirme

Bu makalede, her işlem aşaması için seçenekler ve araçlar vurgulanır.

Azure Veri Gölü Depolama Gen2 ile kullanabileceğiniz Azure hizmetlerinin tam listesi için [bkz.](data-lake-storage-integrate-with-azure-services.md)

## <a name="ingest-the-data-into-data-lake-storage-gen2"></a>Verileri Veri Gölü Depolama Gen2'ye alma

Bu bölümde, farklı veri kaynakları ve bu verilerin bir Veri Gölü Depolama Gen2 hesabına alınabileceği farklı yollar vurgulanır.

![Verileri Veri Gölü Depolama Gen2'ye alma](./media/data-lake-storage-data-scenarios/ingest-data.png "Verileri Veri Gölü Depolama Gen2'ye alma")

### <a name="ad-hoc-data"></a>Geçici veriler

Bu, büyük bir veri uygulamasını prototiplemek için kullanılan daha küçük veri kümelerini temsil eder. Verilerin kaynağına bağlı olarak geçici veri sindirerek farklı yolları vardır. 

Geçici verileri yutmak için kullanabileceğiniz araçların bir listesi aşağıda veda edebilirsiniz.

| veri kaynağı | Kullanarak yutma |
| --- | --- |
| Yerel bilgisayar |[Azure PowerShell](data-lake-storage-directory-file-acl-powershell.md)<br><br>[Azure CLI](data-lake-storage-directory-file-acl-cli.md)<br><br>[Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)<br><br>[AzCopy aracı](../common/storage-use-azcopy-v10.md)|
| Azure Depolama Blob |[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)<br><br>[AzCopy aracı](../common/storage-use-azcopy-v10.md)<br><br>[HDInsight kümesinde çalışan DistCp](data-lake-storage-use-distcp.md)|

### <a name="streamed-data"></a>Akışlı veriler

Bu, uygulamalar, aygıtlar, sensörler vb. gibi çeşitli kaynaklar tarafından oluşturulabilen verileri temsil eder. Bu veriler çeşitli araçlarla Veri Gölü Depolama Gen2'ye sindirilebilir. Bu araçlar genellikle verileri gerçek zamanlı olarak olay bazında yakalar ve işlenir ve daha sonra daha fazla işlenebilecek şekilde olayları toplu olarak Veri Gölü Depolama Gen2'ye yazar.

Aşağıda, akışlı verileri yutmak için kullanabileceğiniz araçların bir listesi verebilirsiniz.

|Araç | Rehber |
|---|--|
|Azure Stream Analytics|[Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Azure Veri Gölü Gen2'ye Çıkış](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|Azure HDInsight Fırtınası | [HdInsight'ta Apache Storm'dan Apache Hadoop HDFS'ye yazın](https://docs.microsoft.com/azure/hdinsight/storm/apache-storm-write-data-lake-store) |

### <a name="relational-data"></a>İlişkisel veriler

İlişkisel veritabanlarından da veri kaynağı olabilirsiniz. Bir süre içinde, ilişkisel veritabanları büyük miktarda veri toplar ve bu veriler büyük bir veri ardışık hattı aracılığıyla işlenirse önemli öngörüler sağlayabilir. Bu verileri Veri Gölü Depolama Gen2'ye taşımak için aşağıdaki araçları kullanabilirsiniz.

İlişkisel verileri yutmak için kullanabileceğiniz araçların bir listesi aşağıda veda edebilirsiniz.

|Araç | Rehber |
|---|--|
|Azure Data Factory | [Azure Data Factory’de Kopyalama Etkinliği](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web sunucusu günlük verileri (özel uygulamaları kullanarak yükleme)

Web sunucusu günlük verilerinin analizi büyük veri uygulamaları için yaygın bir kullanım örneği olduğundan ve Büyük hacimli günlük dosyalarının Veri Gölü Depolama Gen2'ye yüklenmesini gerektirdiğinden, bu tür veri kümesi özellikle çağrılır. Bu tür verileri yüklemek için kendi komut dosyalarınızı veya uygulamalarınızı yazmak için aşağıdaki araçlardan herhangi birini kullanabilirsiniz.

Web sunucusu günlük verilerini yutmak için kullanabileceğiniz araçların bir listesi aşağıda veda edebilirsiniz.

|Araç | Rehber |
|---|--|
|Azure Data Factory | [Azure Data Factory’de Kopyalama Etkinliği](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)  |
|Azure CLI|[Azure CLI](data-lake-storage-directory-file-acl-cli.md)|
|Azure PowerShell|[Azure PowerShell](data-lake-storage-directory-file-acl-powershell.md)|

Web sunucusu günlük verilerini yüklemek ve diğer veri türlerini (örn. sosyal duyarlılık verileri) yüklemek için, size daha büyük büyük veri uygulamanızın bir parçası olarak veri yükleme bileşeninizi ekleme esnekliği sağladığından, kendi özel komut dosyalarınızı/uygulamalarınızı yazmak için iyi bir yaklaşımdır. Bazı durumlarda bu kod bir komut dosyası veya basit komut satırı yardımcı programı şeklinde alabilir. Diğer durumlarda, kod büyük veri işlemeyi bir iş uygulamasına veya çözümüne entegre etmek için kullanılabilir.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleriyle ilişkili veriler

Çoğu HDInsight küme türü (Hadoop, HBase, Storm) veri depolama deposu olarak Veri Gölü Depolama Gen2'yi destekler. HDInsight, Azure Depolama Blobs'tan (WASB) verilere erişim kümeleri. Daha iyi performans için WASB'deki verileri kümeyle ilişkili bir Veri Gölü Depolama Gen2 hesabına kopyalayabilirsiniz. Verileri kopyalamak için aşağıdaki araçları kullanabilirsiniz.

Aşağıda, HDInsight kümeleriyle ilişkili verileri yutmak için kullanabileceğiniz araçların bir listesi verebilirsiniz.

|Araç | Rehber |
|---|--|
|Apache DistCp | [Azure Depolama Blobs ve Azure Veri Gölü Depolama Gen2 arasındaki verileri kopyalamak için DistCp'yi kullanın](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|AzCopy aracı | [AzCopy ile veri aktarımı](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10) |
|Azure Data Factory | [Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Gölü Depolama Gen2'ye veya Azure Veri Depolama Gen2'den kopyalama](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2) |

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Şirket içinde veya IaaS Hadoop kümelerinde depolanan veriler

Büyük miktarda veri, hdfs kullanan makinelerde yerel olarak, varolan Hadoop kümelerinde depolanabilir. Hadoop kümeleri şirket içi dağıtımda olabilir veya Azure'daki bir IaaS kümesi içinde olabilir. Bu tür verileri tek seferlik bir yaklaşım için veya yinelenen bir şekilde Azure Veri Gölü Depolama Gen2'ye kopyalamak için gereksinimler olabilir. Bunu başarmak için kullanabileceğiniz çeşitli seçenekler vardır. Aşağıda alternatifler ve ilişkili trade-off listesidir.

| Yaklaşım | Ayrıntılar | Yararları | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Verileri doğrudan Hadoop kümelerinden Azure Veri Gölü Depolama Gen2'ye kopyalamak için Azure Veri Fabrikası'nı (ADF) kullanın |[ADF, veri kaynağı olarak HDFS'yi destekler](../../data-factory/connector-hdfs.md) |ADF, HDFS ve birinci sınıf uçtan uca yönetim ve izleme için hazır destek sağlar |Veri Yönetimi Ağ Geçidi'nin şirket içinde veya IaaS kümesinde dağıtılmasını gerektirir |
| Hadoop'tan Azure Depolama'ya verileri kopyalamak için Distcp'yi kullanın. Ardından verileri Azure Depolama'dan Veri Gölü Depolama Gen2'ye uygun mekanizmayı kullanarak kopyalayın. |Aşağıdakileri kullanarak Azure Depolama'dan Veri Gölü Depolama Gen2'ye verileri kopyalayabilirsiniz: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[AzCopy aracı](../common/storage-use-azcopy-v10.md)</li><li>[HDInsight kümelerinde çalışan Apache DistCp](data-lake-storage-use-distcp.md)</li></ul> |Açık kaynak araçlarını kullanabilirsiniz. |Birden çok teknoloji içeren çok aşamalı işlem |

### <a name="really-large-datasets"></a>Gerçekten büyük veri kümeleri

Birkaç terabayt aralığında veri kümeleri yüklemek için, yukarıda açıklanan yöntemleri kullanarak bazen yavaş ve maliyetli olabilir. Bu gibi durumlarda Azure ExpressRoute'u kullanabilirsiniz.  

Azure ExpressRoute, binalarınızda Azure veri merkezleri ve altyapı arasında özel bağlantılar oluşturmanıza olanak tanır. Bu, büyük miktarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için [Azure ExpressRoute belgelerine](../../expressroute/expressroute-introduction.md)bakın.

## <a name="process-the-data"></a>Verileri işleme

Veriler Veri Gölü Depolama Gen2'de kullanılabilir hale geldikten sonra desteklenen büyük veri uygulamalarını kullanarak bu veriler üzerinde çözümleme çalıştırabilirsiniz. 

![Veri Gölü Depolama Gen2'deki verileri analiz edin](./media/data-lake-storage-data-scenarios/analyze-data.png "Veri Gölü Depolama Gen2'deki verileri analiz edin")

Burada, Veri Gölü Depolama Gen2'de depolanan verilerüzerinde veri analizi işlerini çalıştırmak için kullanabileceğiniz araçların bir listesi verebilirsiniz.

|Araç | Rehber |
|---|--|
|Azure HDInsight | [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Databricks | [Azure Data Lake Storage 2. Nesil](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)<br><br>[Hızlı başlatma: Azure Veri Tuğlaları kullanarak Azure Veri Gölü Depolama Gen2'deki verileri analiz edin](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br><br>[Öğretici: Azure Databricks'i kullanarak verileri ayıklayın, dönüştürün ve yükleyin](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="visualize-the-data"></a>Verileri görselleştirme

Data Lake Storage Gen2'de depolanan verilerin görsel temsillerini oluşturmak için Power BI konektörünü kullanın. Bkz. [Power BI kullanarak Azure Veri Gölü Depolama Gen2'deki verileri analiz edin.](https://docs.microsoft.com/power-query/connectors/datalakestorage)

## <a name="download-the-data"></a>Verileri indirin

Ayrıca, şu gibi senaryolar için Azure Veri Gölü Depolama Gen2'den veri indirmek veya taşımak da isteyebilirsiniz:

* Verileri, varolan veri işleme ardışık hatlarınızla ara birim olarak diğer depolara taşıyın. Örneğin, verileri Veri Gölü Depolama Gen2'den Azure SQL Veritabanı'na veya şirket içi SQL Server'a taşımak isteyebilirsiniz.

* Uygulama prototipleri inşa ederken IDE ortamlarında işlenmek üzere verileri yerel bilgisayarınıza indirin.

![Veri Gölü Depolama Gen2'den çıkış verileri](./media/data-lake-storage-data-scenarios/egress-data.png "Veri Gölü Depolama Gen2'den çıkış verileri")

Burada, Data Lake Storage Gen2'den veri indirmek için kullanabileceğiniz araçların bir listesi verebilirsiniz.

|Araç | Rehber |
|---|--|
|Azure Data Factory | [Azure Data Factory’de Kopyalama Etkinliği](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |
|Apache DistCp | [Azure Depolama Blobs ve Azure Veri Gölü Depolama Gen2 arasındaki verileri kopyalamak için DistCp'yi kullanın](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|Azure Storage Gezgini|[Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Azure Depolama Gezgini'ni kullanın](data-lake-storage-explorer.md)|
|AzCopy aracı|[AzCopy ve Blob depolama ile veri aktarımı](../common/storage-use-azcopy-blobs.md)|
