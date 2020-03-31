---
title: Veri Gölü Depolama Gen1 içeren veri senaryoları | Microsoft Dokümanlar
description: Veri Gölü Depolama Gen1'de (önceden Azure Veri Gölü Deposu olarak bilinir) hangi verilerin yutulabileceğini, işlenebilir, indirilebilir ve görüntülenebileceğini kullanarak farklı senaryoları ve araçları anlama
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: a8f9b66bf9a301888f2371fb1c58a4845c2232b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536165"
---
# <a name="using-azure-data-lake-storage-gen1-for-big-data-requirements"></a>Büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen1'i kullanma

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Büyük veri işlemede dört temel aşama vardır:

* Büyük miktarda veriyi gerçek zamanlı veya toplu olarak veri deposuna alma
* Verileri işleme
* Verileri indirme
* Verileri görselleştirme

Bu makalede, büyük veri ihtiyaçlarınızı karşılamak için mevcut seçenekleri ve araçları anlamak için Azure Veri Gölü Depolama Gen1 ile ilgili olarak bu aşamalara bakıyoruz.

## <a name="ingest-data-into-data-lake-storage-gen1"></a>Verileri Veri Gölü Depolama Gen1'e alma
Bu bölümde, farklı veri kaynakları ve bu verilerin bir Veri Gölü Depolama Gen1 hesabına alınabileceği farklı yollar vurgulanır.

![Verileri Veri Gölü Depolama Gen1'e alma](./media/data-lake-store-data-scenarios/ingest-data.png "Verileri Veri Gölü Depolama Gen1'e alma")

### <a name="ad-hoc-data"></a>Geçici veriler
Bu, büyük bir veri uygulamasını prototiplemek için kullanılan daha küçük veri kümelerini temsil eder. Verilerin kaynağına bağlı olarak geçici veri sindirerek farklı yolları vardır.

| veri kaynağı | Kullanarak yutma |
| --- | --- |
| Yerel bilgisayar |<ul> <li>[Azure Portalı](data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure CLI](data-lake-store-get-started-cli-2.0.md)</li> <li>[Visual Studio için Veri Gölü Araçlarını Kullanma](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure Depolama Blob |<ul> <li>[Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)</li> <li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[HDInsight kümesinde çalışan DistCp](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Akışlı veriler
Bu, uygulamalar, aygıtlar, sensörler vb. gibi çeşitli kaynaklar tarafından oluşturulabilen verileri temsil eder. Bu veriler çeşitli araçlarla Veri Gölü Depolama Gen1'e sindirilebilir. Bu araçlar genellikle verileri gerçek zamanlı olarak olay bazında yakalar ve işlenir ve daha sonra daha fazla işlenebilecek şekilde olayları toplu olarak Veri Gölü Depolama Gen1'e yazar.

Kullanabileceğiniz araçlar şunlardır:

* [Azure Akış Analizi](../stream-analytics/stream-analytics-data-lake-output.md) - Etkinlik Hub'larına aktarılan etkinlikler, Azure Veri Gölü Depolama Gen1 çıktısı kullanılarak Azure Veri Gölü Depolama Gen1'e yazılabilir.
* [Azure HDInsight Storm](../hdinsight/storm/apache-storm-write-data-lake-store.md) - Fırtına kümesinden veri doğrudan Veri Gölü Depolama Gen1'e yazabilirsiniz.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – Olay Hub'larından olayları alabilir ve ardından Veri Gölü [Depolama Gen1 .NET SDK'yı](data-lake-store-get-started-net-sdk.md)kullanarak Veri Gölü Depolama Gen1'e yazabilirsiniz.

### <a name="relational-data"></a>İlişkisel veriler
İlişkisel veritabanlarından da veri kaynağı olabilirsiniz. Bir süre içinde, ilişkisel veritabanları büyük miktarda veri toplar ve bu veriler büyük bir veri ardışık hattı aracılığıyla işlenirse önemli öngörüler sağlayabilir. Bu verileri Veri Gölü Depolama Gen1'e taşımak için aşağıdaki araçları kullanabilirsiniz.

* [Apaçi Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web sunucusu günlük verileri (özel uygulamaları kullanarak yükleme)
Web sunucusu günlük verilerinin analizi büyük veri uygulamaları için yaygın bir kullanım örneği olduğundan ve Büyük hacimli günlük dosyalarının Veri Gölü Depolama Gen1'e yüklenmesini gerektirdiğinden, bu tür veri kümesi özellikle çağrılır. Bu tür verileri yüklemek için kendi komut dosyalarınızı veya uygulamalarınızı yazmak için aşağıdaki araçlardan herhangi birini kullanabilirsiniz.

* [Azure CLI](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Veri Gölü Depolama Gen1 .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

Web sunucusu günlük verilerini yüklemek ve diğer veri türlerini (örn. sosyal duyarlılık verileri) yüklemek için, size daha büyük büyük veri uygulamanızın bir parçası olarak veri yükleme bileşeninizi ekleme esnekliği sağladığından, kendi özel komut dosyalarınızı/uygulamalarınızı yazmak için iyi bir yaklaşımdır. Bazı durumlarda bu kod bir komut dosyası veya basit komut satırı yardımcı programı şeklinde alabilir. Diğer durumlarda, kod büyük veri işlemeyi bir iş uygulamasına veya çözümüne entegre etmek için kullanılabilir.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleriyle ilişkili veriler
Çoğu HDInsight küme türü (Hadoop, HBase, Storm) veri depolama deposu olarak Veri Gölü Depolama Gen1'i destekler. HDInsight, Azure Depolama Blobs'tan (WASB) verilere erişim kümeleri. Daha iyi performans için WASB'deki verileri kümeyle ilişkili bir Veri Gölü Depolama Gen1 hesabına kopyalayabilirsiniz. Verileri kopyalamak için aşağıdaki araçları kullanabilirsiniz.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy Hizmeti](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Şirket içinde veya IaaS Hadoop kümelerinde depolanan veriler
Büyük miktarda veri, hdfs kullanan makinelerde yerel olarak, varolan Hadoop kümelerinde depolanabilir. Hadoop kümeleri şirket içi dağıtımda olabilir veya Azure'daki bir IaaS kümesi içinde olabilir. Bu tür verileri tek seferlik bir yaklaşım için veya yinelenen bir şekilde Azure Veri Gölü Depolama Gen1'e kopyalamak için gereksinimler olabilir. Bunu başarmak için kullanabileceğiniz çeşitli seçenekler vardır. Aşağıda alternatifler ve ilişkili trade-off listesidir.

| Yaklaşım | Ayrıntılar | Yararları | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Verileri doğrudan Hadoop kümelerinden Azure Veri Gölü Depolama Gen1'e kopyalamak için Azure Veri Fabrikası'nı (ADF) kullanın |[ADF, veri kaynağı olarak HDFS'yi destekler](../data-factory/connector-hdfs.md) |ADF, HDFS ve birinci sınıf uçtan uca yönetim ve izleme için hazır destek sağlar |Veri Yönetimi Ağ Geçidi'nin şirket içinde veya IaaS kümesinde dağıtılmasını gerektirir |
| Hadoop'tan gelen verileri dosya olarak dışa aktarın. Ardından dosyaları uygun mekanizmayı kullanarak Azure Veri Gölü Depolama Gen1'e kopyalayın. |Dosyaları Azure Veri Gölü Depolama Gen1'e aşağıdakileri kullanarak kopyalayabilirsiniz: <ul><li>[Windows işletim sistemi için Azure PowerShell](data-lake-store-get-started-powershell.md)</li><li>[Azure CLI](data-lake-store-get-started-cli-2.0.md)</li><li>Herhangi bir Veri Gölü Depolama Gen1 SDK kullanarak özel uygulama</li></ul> |Çabuk başlayın. Özelleştirilmiş yüklemeler yapabilir |Birden çok teknoloji içeren çok aşamalı işlem. Yönetim ve izleme araçların özelleştirilmiş doğası göz önüne alındığında zaman içinde bir meydan okuma olarak büyüyecek |
| Hadoop'tan Azure Depolama'ya verileri kopyalamak için Distcp'yi kullanın. Ardından verileri Azure Depolama'dan Veri Gölü Depolama Gen1'e uygun mekanizmayı kullanarak kopyalayın. |Aşağıdakileri kullanarak Azure Depolama'dan Veri Gölü Depolama Gen1'e verileri kopyalayabilirsiniz: <ul><li>[Azure Data Factory](../data-factory/copy-activity-overview.md)</li><li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[HDInsight kümelerinde çalışan Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Açık kaynak araçlarını kullanabilirsiniz. |Birden çok teknoloji içeren çok aşamalı işlem |

### <a name="really-large-datasets"></a>Gerçekten büyük veri kümeleri
Birkaç terabayt aralığında veri kümeleri yüklemek için, yukarıda açıklanan yöntemleri kullanarak bazen yavaş ve maliyetli olabilir. Bu gibi durumlarda, aşağıdaki seçenekleri kullanabilirsiniz.

* **Azure ExpressRoute'u kullanma**. Azure ExpressRoute, binalarınızda Azure veri merkezleri ve altyapı arasında özel bağlantılar oluşturmanıza olanak tanır. Bu, büyük miktarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için Azure [ExpressRoute belgelerine](../expressroute/expressroute-introduction.md)bakın.
* **"Çevrimdışı" veri yükleme**. Azure ExpressRoute'u kullanmak herhangi bir nedenle mümkün değilse, verilerinizle birlikte sabit disk sürücülerini bir Azure veri merkezine aktarmak için [Azure İçe Aktarma/Verme hizmetini](../storage/common/storage-import-export-service.md) kullanabilirsiniz. Verileriniz ilk olarak Azure Depolama Blobs'a yüklenir. Daha sonra Azure Depolama Blobs'tan Veri Gölü Depolama Gen1'e veri kopyalamak için [Azure Veri Fabrikası](../data-factory/connector-azure-data-lake-store.md) veya [AdlCopy aracını](data-lake-store-copy-data-azure-storage-blob.md) kullanabilirsiniz.

  > [!NOTE]
  > Dışa Aktarma/Dışa Aktarma hizmetini kullanırken, Azure veri merkezine sevk ettiğiniz disklerde dosya boyutları 195 GB'dan büyük olmamalıdır.
  >
  >

## <a name="process-data-stored-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'de depolanan işlem verileri
Veriler Veri Gölü Depolama Gen1'de kullanılabilir hale geldikten sonra desteklenen büyük veri uygulamalarını kullanarak bu veriler üzerinde çözümleme çalıştırabilirsiniz. Şu anda, Veri Gölü Depolama Gen1'de depolanan verilerüzerinde veri analizi işlerini çalıştırmak için Azure HDInsight ve Azure Veri Gölü Analizi'ni kullanabilirsiniz.

![Veri Gölü Depolama Gen1'deki verileri analiz edin](./media/data-lake-store-data-scenarios/analyze-data.png "Veri Gölü Depolama Gen1'deki verileri analiz edin")

Aşağıdaki örneklere bakabilirsiniz.

* [Depolama alanı olarak Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-storage-gen1"></a>Data Lake Storage Gen1'den veri indirin
Ayrıca, şu gibi senaryolar için Azure Veri Gölü Depolama Gen1'den veri indirmek veya taşımak da isteyebilirsiniz:

* Verileri, varolan veri işleme ardışık hatlarınızla ara birim olarak diğer depolara taşıyın. Örneğin, verileri Veri Gölü Depolama Gen1'den Azure SQL Veritabanı'na veya şirket içi SQL Server'a taşımak isteyebilirsiniz.
* Uygulama prototipleri inşa ederken IDE ortamlarında işlenmek üzere verileri yerel bilgisayarınıza indirin.

![Veri Gölü Depolama Gen1'den çıkış verileri](./media/data-lake-store-data-scenarios/egress-data.png "Veri Gölü Depolama Gen1'den çıkış verileri")

Bu gibi durumlarda, aşağıdaki seçeneklerden birini kullanabilirsiniz:

* [Apaçi Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Veri Gölü Depolama Gen1'den veri indirmek için kendi komut dosyanızı/uygulamanızı yazmak için aşağıdaki yöntemleri de kullanabilirsiniz.

* [Azure CLI](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Veri Gölü Depolama Gen1 .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'deki verileri görselleştirin
Veri Gölü Depolama Gen1'de depolanan verilerin görsel gösterimlerini oluşturmak için bir hizmet karışımık kullanabilirsiniz.

![Veri Gölü Depolama Gen1'deki verileri görselleştirin](./media/data-lake-store-data-scenarios/visualize-data.png "Veri Gölü Depolama Gen1'deki verileri görselleştirin")

* [Verileri Veri Gölü Depolama Gen1'den Azure SQL Veri Ambarı'na taşımak için Azure Veri Fabrikası'nı](../data-factory/copy-activity-overview.md) kullanarak başlayabilirsiniz
* Bundan sonra, verilerin görsel gösterimini oluşturmak için [Power BI'yi Azure SQL Veri Ambarı ile tümleştirebilirsiniz.](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md)
