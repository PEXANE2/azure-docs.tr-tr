---
title: Azure HDInsight 'ta Apache HBase nedir?
description: Hadoop’ta oluşturulan bir NoSQL veritabanı olan HDInsight’ta Apache HBase’e giriş. Kullanım örnekleri hakkında bilgi edinin ve HBase’i diğer Hadoop kümeleriyle karşılaştırın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 03/03/2020
ms.openlocfilehash: 97814f4d22629fd74f395887a7361a3aabe55012
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78271846"
---
# <a name="what-is-apache-hbase-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache HBase nedir?

[Apache HBase](https://hbase.apache.org/) , [Apache Hadoop](https://hadoop.apache.org/) oluşturulan ve [Google BigTable](https://cloud.google.com/bigtable/)'dan sonra Modellenen açık kaynaklı, NoSQL veritabanıdır. HBase, sütun aileleri tarafından veritabanında büyük miktarlardaki yapılandırılmamış ve yarı yapılandırılmış veriler için rasgele erişim ve güçlü tutarlılık sağlar.

Kullanıcı perspektifinden HBase veritabanına benzer. Veriler bir tablonun satırları ve sütunlarında depolanır ve bir satırdaki veriler sütun ailesine göre gruplandırılır. HBase, kullanılmadan önce sütunların ya da bunlarda depolanan veri türünün tanımlanmasına gerek duyulmayan, şemasız bir veritabanıdır. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. Veri yedekleme, toplu işleme ve Hadoop ekosistemindeki dağıtılmış uygulamalar tarafından sağlanan diğer özelliklere dayanabilir.

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache HBase nasıl uygulanır?

HDInsight HBase, Azure ortamına tümleştirilmiş yönetilen bir küme olarak sunulur. Kümeler, düşük gecikme süresi ve performans ve maliyet seçeneklerinde artan esneklik sağlayan verileri doğrudan [Azure Storage](./../hdinsight-hadoop-use-blob-storage.md)'da depolayacak şekilde yapılandırılmıştır. Bu, müşterilerin milyonlarca uç noktadan gelen algılayıcı ve telemetri verilerini depolayan hizmetleri oluşturmak ve bu verileri Hadoop işleriyle çözümlemek üzere etkileşimli web siteleri oluşturmalarını sağlar. HBase ve Hadoop Azure’da büyük veri projeleri için yi başlangıç noktalarıdır; bunlar özellikle büyük veri kümeleriyle çalışmak üzere gerçek zamanlı uygulamaları etkinleştirebilir.

HDInsight uygulaması, tabloların otomatik parçalanmasını, okumalar ve yazmalar için yüksek tutarlılık ve otomatik yük devretme sağlamak için HBase’in ölçek genişletmeli mimarisinden yararlanır. Performans, okumalar için bellek içi önbelleğe alma ve yazmalar için yüksek verimlilikli akış tarafından geliştirilmiştir. HBase kümesi sanal ağda oluşturulabilir. Ayrıntılar için bkz. [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Veriler HDInsight HBase’de nasıl yönetilir?

Veriler HBase kabuğunda `create`, `get`, `put` ve `scan` komutları kullanılarak HBase tarafından yönetilebilir. Veriler `put` kullanılarak veritabanına yazılır ve `get` kullanarak okunur. `scan` komutu, bir tablodaki birden çok satırdaki verileri almak için kullanılır. Veriler, HBase REST API’sinin üstünde bir istemci kitaplığı sağlayan HBase C# API’si kullanılarak da yönetilebilir. HBase veritabanı, [Apache Hive](https://hive.apache.org/)kullanılarak da sorgulanabilir. Bu programlama modellerine giriş için bkz. [HDInsight 'ta Apache Hadoop Ile Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md). Aynı zamanda, veritabanını barındıran düğümlerde veri işlemeye izin veren de mevcuttur.

> [!NOTE]  
> Thrift, HDInsight’ta HBase tarafından desteklenmez.

## <a name="use-cases-for-apache-hbase"></a>Apache HBase için kullanım örnekleri

Web aramadan BigTable (ve uzantısı ile HBase) oluşturulan kurallı kullanım durumu. Arama motorları terimleri bunları içeren web siteleriyle eşleştiren dizinler oluşturur. Ancak HBase için uygun olan diğer birçok kullanım örneği vardır; bunların birkaçı bu bölümde listelenmektedir.

|Senaryo |Açıklama |
|---|---|
|Anahtar değeri deposu|HBase anahtar-değer deposu olarak kullanılabilir ve ileti sistemlerini yönetmek için uygundur. Facebook, kendi mesajlaşma sistemleri için HBase kullanır ve Internet iletişimlerini depolamak ve yönetmek için idealdir. WebTable web sayfalarından çıkarılan tabloları aramak ve yönetmek için HBase kullanır.|
|Algılayıcı verileri|HBase çeşitli kaynaklardan artımlı olarak toplanan verileri yakalamak için yararlıdır. Bu, sosyal analizleri, zaman serisini, eğilimleri ve sayaçlarla etkileşimli panoları güncel tutmaya ve denetim günlüğü sistemlerini yönetmeye dahildir. Örnek olarak, Bloomberg tüccar terminali ve sunucu sistemlerinin durumuna ilişkin toplanan ölçümleri toplayan ve bunlara erişim imkanı sağlayan Open Time Series Veritabanı (OpenTSDB) verilebilir.|
|Gerçek zamanlı sorgu|[Apache Phoenix](https://phoenix.apache.org/) , Apache HBase IÇIN bir SQL sorgu altyapısıdır. Bir JDBC sürücüsü olarak erişilir ve SQL kullanarak HBase tablolarının sorgulanmasına ve yönetilmesine izin vermez.|
|Bir platform olarak HBase|Uygulamalar, bir veri deposu olarak kullanarak HBase’in üstünde çalışabilir. Phoenix, [Opentsdb](http://opentsdb.net/), Kiji ve Titan örnekleri sayılabilir. Uygulamalar HBase ile de tümleştirebilir. Örnekler şunlardır [Apache Hive](https://hive.apache.org/), [Apache Pig](https://pig.apache.org/), [Solr](https://lucene.apache.org/solr/), [Apache Storm](https://storm.apache.org/), [Apache flome](https://flume.apache.org/), [Apache Impala](https://impala.apache.org/), [Apache Spark](https://spark.apache.org/) , [ganbir](http://ganglia.info/)ve [Apache detaya gitme](https://drill.apache.org/).|

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 'ta Apache Hadoop ile Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md)
* [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md)
* [HDInsight 'ta Apache HBase çoğaltmasını yapılandırma](apache-hbase-replication.md)
