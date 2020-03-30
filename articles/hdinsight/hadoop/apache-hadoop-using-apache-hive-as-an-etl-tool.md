---
title: Apache Hive'ı ETL Aracı Olarak Kullanma - Azure HDInsight
description: Azure HDInsight'ta (ETL) verileri ayıklamak, dönüştürmek ve yüklemek için Apache Hive'ı kullanın.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: be331f36a6305b05ce83a2b2d5fdfb73a154ce3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623110"
---
# <a name="use-apache-hive-as-an-extract-transform-and-load-etl-tool"></a>Apache Hive'ı Ayıklama, Dönüştürme ve Yükleme (ETL) aracı olarak kullanma

Genellikle gelen verileri analiz için uygun bir hedefe yüklemeden önce temizlemeniz ve dönüştürmeniz gerekir. Verileri hazırlamak ve bir veri hedefine yüklemek için ayıklama, dönüştürme ve yükleme (ETL) işlemleri kullanılır.  HDInsight'taki Apache Hive yapılandırılmamış verileri okuyabilir, verileri gerektiği gibi işleyebilir ve ardından karar destek sistemleri için verileri ilişkisel bir veri ambarına yükleyebilir. Bu yaklaşımda, veriler kaynaktan ayıklanır ve Azure Depolama lekeleri veya Azure Veri Gölü Depolama gibi ölçeklenebilir depolamada depolanır. Veriler daha sonra Hive sorguları bir dizi kullanılarak dönüştürülür ve nihayet hedef veri deposuna toplu yükleme için hazırlık Hive içinde sahnelenir.

## <a name="use-case-and-model-overview"></a>Büyük/küçük harf ve modele genel bakış kullanın

Aşağıdaki şekil, ETL otomasyonu için kullanım örneği ve modeline genel bir bakış gösterir. Girdi verileri uygun çıktıyı oluşturmak için dönüştürülür.  Bu dönüştürme sırasında, veriler şekli, veri türünü ve hatta dili değiştirebilir.  ETL süreçleri Imperial'i metrik'e dönüştürebilir, saat dilimlerini değiştirebilir ve hedefteki varolan verilerle düzgün şekilde hizalanması için hassasiyeti artırabilir.  ETL süreçleri, raporlamayı güncel tutmak veya varolan veriler hakkında daha fazla bilgi sağlamak için yeni verileri mevcut verilerle birleştirebilir.  Raporlama araçları ve hizmetleri gibi uygulamalar daha sonra bu verileri istenilen biçimde tüketebilir.

![ETL mimarisi olarak Apache Hive](./media/apache-hadoop-using-apache-hive-as-an-etl-tool/hdinsight-etl-architecture.png)

Hadoop genellikle çok sayıda metin dosyası (CSV gibi) veya daha küçük ama sık değişen metin dosyaları veya her ikisini birden içe aktaran ETL işlemlerinde kullanılır.  Kovan veri hedefe yüklemeden önce verileri hazırlamak için kullanmak için harika bir araçtır.  Hive, CSV üzerinde bir şema oluşturmanıza ve verilerle etkileşimde olan MapReduce programları oluşturmak için SQL benzeri bir dil kullanmanıza olanak tanır.

ETL gerçekleştirmek için Hive'ı kullanmanın tipik adımları aşağıdaki gibidir:

1. Verileri Azure Veri Gölü Depolama'ya veya Azure Blob Depolama'ya yükleyin.
2. Şemalarınızı depolamak için Hive tarafından kullanılmak üzere bir Metadata Store veritabanı (Azure SQL Veritabanı'nı kullanarak) oluşturun.
3. Bir HDInsight kümesi oluşturun ve veri deposunu bağlayın.
4. Veri deposundaki veriler üzerinden okuma zamanında uygulanacak şemayı tanımlayın:

    ```
    DROP TABLE IF EXISTS hvac;

    --create the hvac table on comma-separated sensor data stored in Azure Storage blobs

    CREATE EXTERNAL TABLE hvac(`date` STRING, time STRING, targettemp BIGINT,
        actualtemp BIGINT,
        system BIGINT,
        systemage BIGINT,
        buildingid BIGINT)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    STORED AS TEXTFILE LOCATION 'wasbs://{container}@{storageaccount}.blob.core.windows.net/HdiSamples/SensorSampleData/hvac/';
    ```

5. Verileri dönüştürün ve hedefe yükleyin.  Dönüştürme ve yükleme sırasında Hive'ı kullanmanın birkaç yolu vardır:

    * Hive kullanarak verileri sorgulayın ve hazırlayın ve Azure Veri Gölü Depolama'da veya Azure blob depolamasında CSV olarak kaydedin.  Ardından, bu CSV'leri elde etmek ve verileri SQL Server gibi bir hedef ilişkisel veritabanına yüklemek için SQL Server Integration Services (SSIS) gibi bir araç kullanın.
    * Hive ODBC sürücüsünü kullanarak verileri doğrudan Excel veya C# adresinden sorgula.
    * Hazırlanan düz CSV dosyalarını okumak ve hedef ilişkisel veritabanına yüklemek için [Apache Sqoop](apache-hadoop-use-sqoop-mac-linux.md) kullanın.

## <a name="data-sources"></a>Veri kaynakları

Veri kaynakları genellikle veri deponuzdaki varolan verilerle eşleştirilebilen dış verilerdir( örneğin:

* Sosyal medya verileri, günlük dosyaları, sensörler ve veri dosyaları oluşturan uygulamalar.
* Hava durumu istatistikleri veya satıcı satış numaraları gibi veri sağlayıcılardan alınan veri kümeleri.
* Uygun bir araç veya çerçeve üzerinden yakalanan, filtrelenen ve işlenen verileri akış.

<!-- TODO: (see Collecting and loading data into HDInsight). -->

## <a name="output-targets"></a>Çıkış hedefleri

Hive'ı, veri çıktısını çeşitli hedeflere kullanmak ta:

* SQL Server veya Azure SQL Veritabanı gibi ilişkisel bir veritabanı.
* Azure SQL Veri Ambarı gibi bir veri ambarı.
* Excel.
* Azure tablo ve blob depolama.
* Verilerin belirli biçimlerde işlenmesini gerektiren uygulamalar veya hizmetler veya belirli bilgi yapısı türleri içeren dosyalar olarak.
* [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)gibi bir JSON Belge Mağazası .

## <a name="considerations"></a>Dikkat edilmesi gerekenler

ETL modeli genellikle aşağıdakileri yapmak istediğinizde kullanılır:

* Akış verilerini veya dış kaynaklardan gelen büyük hacimli yarı yapılandırılmış veya yapılandırılmamış verileri varolan bir veritabanına veya bilgi sistemine yükleyin.
* Verileri yüklemeden önce, kümeden birden fazla dönüşüm geçirerek temizleyin, dönüştürün ve doğrulayın.
* Düzenli olarak güncelleştirilen raporlar ve görselleştirmeler oluşturun. Örneğin, raporun gün içinde oluşturulması çok uzun sürüyorsa, raporu gece çalışacak şekilde zamanlayabilirsiniz. Bir Kovan sorgusunu otomatik olarak çalıştırmak için [Azure Logic Apps](../../logic-apps/logic-apps-overview.md) ve PowerShell'i kullanabilirsiniz.

Verilerin hedefi bir veritabanı değilse, sorgu içinde uygun biçimde bir dosya oluşturabilirsiniz, örneğin bir CSV. Bu dosya daha sonra Excel veya Power BI'ye aktarılabilir.

ETL işleminin bir parçası olarak veriler üzerinde çeşitli işlemler gerçekleştirmeniz gerekiyorsa, bunları nasıl yönettiğinizi göz önünde bulundurun. İşlemler çözüm içinde bir iş akışı olarak değil, harici bir program tarafından denetlenirse, bazı işlemlerin paralel olarak yürütülüp yürütülemeyeceğine karar vermeniz ve her işin ne zaman tamamlanacağına karar vermeniz gerekir. Hadoop içinde Oozie gibi bir iş akışı mekanizması kullanarak dış komut dosyaları veya özel programlar kullanarak bir dizi işlem düzenlemeye çalışırken daha kolay olabilir. Oozie hakkında daha fazla bilgi için [İş Akışı ve iş orkestrasyonu](https://msdn.microsoft.com/library/dn749829.aspx)hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçekte ETL](apache-hadoop-etl-at-scale.md)
* [Bir veri ardışık hattını operasyonel hale](../hdinsight-operationalize-data-pipeline.md)

<!-- * [ETL Deep Dive](../hdinsight-etl-deep-dive.md) -->
