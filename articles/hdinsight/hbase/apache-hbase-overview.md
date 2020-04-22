---
title: Azure HDInsight'ta Apache HBase nedir?
description: Hadoop’ta oluşturulan bir NoSQL veritabanı olan HDInsight’ta Apache HBase’e giriş. Kullanım örnekleri hakkında bilgi edinin ve HBase’i diğer Hadoop kümeleriyle karşılaştırın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 04/20/2020
ms.openlocfilehash: e977d4d68a330f57c4b53da4270e4515d4e69ee0
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81729817"
---
# <a name="what-is-apache-hbase-in-azure-hdinsight"></a>Azure HDInsight'ta Apache HBase nedir

[Apache HBase,](https://hbase.apache.org/) Apache Hadoop üzerine inşa edilmiş ve [Google BigTable'dan](https://cloud.google.com/bigtable/)sonra modellenen açık kaynak kodlu bir NoSQL veritabanıdır. HBase, şemasız bir veritabanında büyük miktarda veri için rasgele erişim ve güçlü tutarlılık sağlar. Veritabanı sütun aileler tarafından düzenlenir.

Kullanıcı açısından bakıldığında, HBase bir veritabanına benzer. Veriler tablonun satır ve sütunlarında depolanır ve satır içindeki veriler sütun ailesine göre gruplanır. HBase şemasız bir veritabanıdır. Sütunlar ve veri türleri kullanmadan önce tanımsız olabilir. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. Hadoop ortamında dağıtılmış uygulamalar tarafından sağlanan veri artıklığı, toplu işleme ve diğer özelliklere güvenebilir.

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Azure HDInsight'ta Apache HBase nasıl uygulanır?

HDInsight HBase, Azure ortamına tümleştirilmiş yönetilen bir küme olarak sunulur. Kümeler, düşük gecikme ve performans ve maliyet seçimlerinde daha fazla esneklik sağlayan verileri doğrudan [Azure Depolama'da](./../hdinsight-hadoop-use-blob-storage.md)depolamak üzere yapılandırılır. Bu özellik, müşterilerin büyük veri kümeleriyle çalışan etkileşimli web siteleri oluşturmasına olanak tanır. Milyonlarca uç noktadan sensör ve telemetri verilerini depolayan hizmetler oluşturmak için. Ve hadoop işleri ile bu verileri analiz etmek. HBase ve Hadoop, Azure'daki büyük veri projesi için iyi bir başlangıç noktasıdır. Hizmetler, gerçek zamanlı uygulamaların büyük veri kümeleriyle çalışmasını sağlayabilir.

HDInsight uygulaması, tabloların otomatik olarak parçalanması için HBase'in ölçeklendirme mimarisini kullanır. Ve okuma ve yazma için güçlü tutarlılık ve otomatik failover. Performans, okumalar için bellek içi önbelleğe alma ve yazmalar için yüksek verimlilikli akış tarafından geliştirilmiştir. HBase kümesi sanal ağda oluşturulabilir. Ayrıntılar için bkz. [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Veriler HDInsight HBase’de nasıl yönetilir?

Veriler HBase kabuğunda `create`, `get`, `put` ve `scan` komutları kullanılarak HBase tarafından yönetilebilir. Veriler `put` kullanılarak veritabanına yazılır ve `get` kullanarak okunur. `scan` komutu, bir tablodaki birden çok satırdaki verileri almak için kullanılır. Veriler, HBase REST API’sinin üstünde bir istemci kitaplığı sağlayan HBase C# API’si kullanılarak da yönetilebilir. Bir HBase veritabanı da [Apache Hive](https://hive.apache.org/)kullanılarak sorgulanabilir. Bu programlama modellerine giriş yapmak için, [HDInsight'ta Apache Hadoop ile Apache HBase kullanmaya başlayın](./apache-hbase-tutorial-get-started-linux.md)bakın. Yardımcı işlemciler de kullanılabilir, veritabanını barındıran düğümlerde veri işlemeye izin verir.

> [!NOTE]  
> Thrift, HDInsight’ta HBase tarafından desteklenmez.

## <a name="use-cases-for-apache-hbase"></a>Apache HBase için kılıfları kullanma

BigTable (ve uzantısı olarak, HBase) web arama oluşturulan kanonik kullanım örneği. Arama motorları terimleri bunları içeren web siteleriyle eşleştiren dizinler oluşturur. Ancak HBase için uygun olan diğer birçok kullanım örneği vardır; bunların birkaçı bu bölümde listelenmektedir.

|Senaryo |Açıklama |
|---|---|
|Anahtar değeri deposu|HBase anahtar değeri deposu olarak kullanılabilir ve ileti sistemlerini yönetmek için uygundur. Facebook, mesajlaşma sistemi için HBase'i kullanır ve Internet iletişimlerini depolamak ve yönetmek için idealdir. WebTable web sayfalarından çıkarılan tabloları aramak ve yönetmek için HBase kullanır.|
|Algılayıcı verileri|HBase çeşitli kaynaklardan artımlı olarak toplanan verileri yakalamak için yararlıdır. Bu veriler sosyal analizve zaman serilerini içerir. Ve etkileşimli panoları trendler ve sayaçlarla güncel tutmak ve denetim günlüğü sistemlerini yönetmek. Örnekler Bloomberg tüccar terminali ve Open Time Serisi Veritabanı (OpenTSDB) içerir. OpenTSDB, sunucu sistemlerinin durumu hakkında toplanan ölçümleri saklar ve sağlar.|
|Gerçek zamanlı sorgu|[Apache Phoenix, Apache](https://phoenix.apache.org/) HBase için bir SQL sorgu motorudur. JDBC sürücüsü olarak erişilir ve SQL kullanarak HBase tablolarını sorgulamayı ve yönetmeyi sağlar.|
|Bir platform olarak HBase|Uygulamalar, bir veri deposu olarak kullanarak HBase’in üstünde çalışabilir. Örnekler Phoenix, OpenTSDB `Kiji`ve Titan içerir. Uygulamalar HBase ile de tümleştirebilir. Örnekler şunlardır: [Apache Hive](https://hive.apache.org/), Apache Pig, [Solr](https://lucene.apache.org/solr/), Apache Storm, Apache Flume, [Apache Impala](https://impala.apache.org/), Apache Spark, `Ganglia`ve Apache Matkap.|

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Apache Hadoop ile Apache HBase'i kullanmaya başlayın](./apache-hbase-tutorial-get-started-linux.md)
* [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md)
* [HDInsight'ta Apache HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
