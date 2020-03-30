---
title: Azure HDInsight'ta Apache HBase nedir?
description: Hadoop’ta oluşturulan bir NoSQL veritabanı olan HDInsight’ta Apache HBase’e giriş. Kullanım örnekleri hakkında bilgi edinin ve HBase’i diğer Hadoop kümeleriyle karşılaştırın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 03/03/2020
ms.openlocfilehash: 97814f4d22629fd74f395887a7361a3aabe55012
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78271846"
---
# <a name="what-is-apache-hbase-in-azure-hdinsight"></a>Azure HDInsight'ta Apache HBase nedir

[Apache HBase,](https://hbase.apache.org/) [Apache Hadoop](https://hadoop.apache.org/) üzerine inşa edilmiş ve [Google BigTable'dan](https://cloud.google.com/bigtable/)sonra modellenen açık kaynak kodlu bir NoSQL veritabanıdır. HBase, sütun aileleri tarafından veritabanında büyük miktarlardaki yapılandırılmamış ve yarı yapılandırılmış veriler için rasgele erişim ve güçlü tutarlılık sağlar.

Kullanıcı açısından bakıldığında, HBase bir veritabanına benzer. Veriler tablonun satır ve sütunlarında depolanır ve satır içindeki veriler sütun ailesine göre gruplanır. HBase, kullanılmadan önce sütunların ya da bunlarda depolanan veri türünün tanımlanmasına gerek duyulmayan, şemasız bir veritabanıdır. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. Veri yedekleme, toplu işleme ve Hadoop ekosistemindeki dağıtılmış uygulamalar tarafından sağlanan diğer özelliklere dayanabilir.

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Azure HDInsight'ta Apache HBase nasıl uygulanır?

HDInsight HBase, Azure ortamına tümleştirilmiş yönetilen bir küme olarak sunulur. Kümeler, düşük gecikme ve performans ve maliyet seçimlerinde daha fazla esneklik sağlayan verileri doğrudan [Azure Depolama'da](./../hdinsight-hadoop-use-blob-storage.md)depolamak üzere yapılandırılır. Bu, müşterilerin milyonlarca uç noktadan gelen algılayıcı ve telemetri verilerini depolayan hizmetleri oluşturmak ve bu verileri Hadoop işleriyle çözümlemek üzere etkileşimli web siteleri oluşturmalarını sağlar. HBase ve Hadoop Azure’da büyük veri projeleri için yi başlangıç noktalarıdır; bunlar özellikle büyük veri kümeleriyle çalışmak üzere gerçek zamanlı uygulamaları etkinleştirebilir.

HDInsight uygulaması, tabloların otomatik parçalanmasını, okumalar ve yazmalar için yüksek tutarlılık ve otomatik yük devretme sağlamak için HBase’in ölçek genişletmeli mimarisinden yararlanır. Performans, okumalar için bellek içi önbelleğe alma ve yazmalar için yüksek verimlilikli akış tarafından geliştirilmiştir. HBase kümesi sanal ağda oluşturulabilir. Ayrıntılar için bkz. [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Veriler HDInsight HBase’de nasıl yönetilir?

Veriler HBase kabuğunda `create`, `get`, `put` ve `scan` komutları kullanılarak HBase tarafından yönetilebilir. Veriler `put` kullanılarak veritabanına yazılır ve `get` kullanarak okunur. `scan` komutu, bir tablodaki birden çok satırdaki verileri almak için kullanılır. Veriler, HBase REST API’sinin üstünde bir istemci kitaplığı sağlayan HBase C# API’si kullanılarak da yönetilebilir. Bir HBase veritabanı da [Apache Hive](https://hive.apache.org/)kullanılarak sorgulanabilir. Bu programlama modellerine giriş yapmak için, [HDInsight'ta Apache Hadoop ile Apache HBase kullanmaya başlayın](./apache-hbase-tutorial-get-started-linux.md)bakın. Yardımcı işlemciler de kullanılabilir, veritabanını barındıran düğümlerde veri işlemeye izin verir.

> [!NOTE]  
> Thrift, HDInsight’ta HBase tarafından desteklenmez.

## <a name="use-cases-for-apache-hbase"></a>Apache HBase için kılıfları kullanma

BigTable (ve uzantısı olarak, HBase) web arama oluşturulan kanonik kullanım örneği. Arama motorları terimleri bunları içeren web siteleriyle eşleştiren dizinler oluşturur. Ancak HBase için uygun olan diğer birçok kullanım örneği vardır; bunların birkaçı bu bölümde listelenmektedir.

|Senaryo |Açıklama |
|---|---|
|Anahtar değeri deposu|HBase anahtar değeri deposu olarak kullanılabilir ve ileti sistemlerini yönetmek için uygundur. Facebook, mesajlaşma sistemi için HBase'i kullanır ve Internet iletişimlerini depolamak ve yönetmek için idealdir. WebTable web sayfalarından çıkarılan tabloları aramak ve yönetmek için HBase kullanır.|
|Algılayıcı verileri|HBase çeşitli kaynaklardan artımlı olarak toplanan verileri yakalamak için yararlıdır. Buna sosyal analiz, zaman serileri, etkileşimli panoları trendler ve sayaçlarla güncel tutma ve denetim günlüğü sistemlerini yönetme dahildir. Örnek olarak, Bloomberg tüccar terminali ve sunucu sistemlerinin durumuna ilişkin toplanan ölçümleri toplayan ve bunlara erişim imkanı sağlayan Open Time Series Veritabanı (OpenTSDB) verilebilir.|
|Gerçek zamanlı sorgu|[Apache Phoenix, Apache](https://phoenix.apache.org/) HBase için bir SQL sorgu motorudur. JDBC sürücüsü olarak erişilir ve SQL kullanarak HBase tablolarını sorgulamayı ve yönetmeyi sağlar.|
|Bir platform olarak HBase|Uygulamalar, bir veri deposu olarak kullanarak HBase’in üstünde çalışabilir. Örnekler Phoenix, [OpenTSDB,](http://opentsdb.net/)Kiji ve Titan içerir. Uygulamalar HBase ile de tümleştirebilir. Örnekler [Apache Hive](https://hive.apache.org/), [Apache Pig](https://pig.apache.org/), [Solr](https://lucene.apache.org/solr/), [Apache Storm](https://storm.apache.org/), [Apache Flume](https://flume.apache.org/), [Apache Impala](https://impala.apache.org/), [Apache Spark](https://spark.apache.org/) , [Ganglia](http://ganglia.info/), ve [Apache Matkap](https://drill.apache.org/)içerir .|

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Apache Hadoop ile Apache HBase'i kullanmaya başlayın](./apache-hbase-tutorial-get-started-linux.md)
* [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md)
* [HDInsight'ta Apache HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
