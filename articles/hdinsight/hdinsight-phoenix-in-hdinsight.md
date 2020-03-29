---
title: HDInsight'ta Apache Phoenix - Azure HDInsight
description: Apache Phoenix'e Genel Bakış
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: b1d81296c996ab09cb6482cb970496779ccf8bd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435490"
---
# <a name="apache-phoenix-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Phoenix

[Apache Phoenix](https://phoenix.apache.org/) açık kaynak kodlu, kitlesel paralel ilişkisel veritabanı katmanı [Apache HBase](hbase/apache-hbase-overview.md)üzerine inşa edilmiştir. Phoenix, HBase üzerinden SQL benzeri sorgular kullanmanıza olanak tanır. Phoenix, kullanıcıların SQL tablolarını, dizinleri, görünümleri ve dizileri oluşturmasını, silmesini, değiştirmesine ve satırları tek tek ve toplu olarak yükseltmesine olanak sağlamak için altında JDBC sürücülerini kullanır. Phoenix, sorguları derlemek için MapReduce'ı kullanmak yerine noSQL yerel derlemesini kullanarak HBase'in üstünde düşük gecikmeli uygulamalar oluşturulmasını sağlar. Phoenix, sunucunun adres alanında istemci tarafından sağlanan kodu çalıştırmayı desteklemek için yardımcı işlemciler ekler ve verilerle birlikte bulunan kodu çalıştırılır. Bu yaklaşım istemci/sunucu veri aktarımını en aza indirir.

Apache Phoenix, programlama yerine SQL benzeri sözdizimi kullanabilen geliştiricilere büyük veri sorguları açar. Phoenix, [Apache Hive ve Apache](hadoop/hdinsight-use-hive.md) Spark SQL gibi diğer araçların aksine HBase için son derece optimize edilse de, bu tür araçlariçin son derece iyileştirilmiştir. Geliştiriciler için yararı çok daha az kod ile yüksek performanslı sorgular yazmaktır.

Bir SQL sorgusu gönderdiğinizde, Phoenix sorguyu HBase yerel çağrılarına derler ve en iyi duruma getirilmesi için paralel olarak taramayı (veya planı) çalıştırır. Bu soyutlama katmanı, geliştiriciyi MapReduce işlerini yazmaktan kurtarır, bunun yerine iş mantığına ve Phoenix'in büyük veri depolaması etrafındaki uygulamalarının iş akışına odaklanır.

## <a name="query-performance-optimization-and-other-features"></a>Performans optimizasyonu ve diğer özellikleri sorgula

Apache Phoenix, HBase sorgularına çeşitli performans geliştirmeleri ve özellikler ekler.

### <a name="secondary-indexes"></a>İkincil dizinler

HBase'in birincil satır anahtarında lexicographically sıralanmış tek bir dizini vardır. Bu kayıtlara yalnızca satır tuşu ile erişilebilir. Satır anahtarı dışındaki herhangi bir sütundan kayıtlara erişmek, gerekli filtreyi uygularken tüm verilerin taranması gerekir. İkincil dizinde, dizine eklenmiş sütunlar veya ifadeler alternatif bir satır anahtarı oluşturur ve bu dizinde aramave aralık taramalarına izin verir.

Komutu `CREATE INDEX` ile ikincil bir dizin oluşturun:

```sql
CREATE INDEX ix_purchasetype on SALTEDWEBLOGS (purchasetype, transactiondate) INCLUDE (bookname, quantity);
```

Bu yaklaşım, tek dizine eklenmiş sorguları yürütme üzerinde önemli bir performans artışı sağlayabilir. Bu tür ikincil dizin, sorguda yer alan tüm sütunları içeren bir **kaplama dizinidir.** Bu nedenle, tablo arama gerekli değildir ve dizin tüm sorguyu karşılar.

### <a name="views"></a>Görünümler

Phoenix görünümleri, yaklaşık 100'den fazla fiziksel tablo oluşturduğunuzda performansın bozulmaya başladığı bir HBase sınırlamasını aşmanın bir yolunu sağlar. Phoenix görünümleri, birden çok *sanal tablonun* altında yatan bir fiziksel HBase tablosunu paylaşmasını sağlar.

Phoenix görünümü oluşturmak, standart SQL görünüm sözdizimini kullanmaya benzer. Bir fark, temel tablosundan devralınan sütunlara ek olarak, görünümünüz için sütunları tanımlayabilmektir. Yeni `KeyValue` sütunlar da ekleyebilirsiniz.

Örneğin, aşağıda tanımla birlikte `product_metrics` fiziksel bir tablo verilmiştir:

```sql
CREATE  TABLE product_metrics (
    metric_type CHAR(1),
    created_by VARCHAR,
    created_date DATE,
    metric_id INTEGER
    CONSTRAINT pk PRIMARY KEY (metric_type, created_by, created_date, metric_id));
```

Ek sütunlarla bu tablonun üzerinde bir görünüm tanımlayın:

```sql
CREATE VIEW mobile_product_metrics (carrier VARCHAR, dropped_calls BIGINT) AS
SELECT * FROM product_metrics
WHERE metric_type = 'm';
```

Daha sonra daha fazla `ALTER VIEW` sütun eklemek için deyimi kullanın.

### <a name="skip-scan"></a>Atlama tonu

Atlama tonu, farklı değerler bulmak için bileşik dizinin bir veya daha fazla sütununu kullanır. Aralık taramasından farklı olarak, atlama taraması satır içi tarama yı uygulayarak [gelişmiş performans](https://phoenix.apache.org/performance.html#Skip-Scan)sağlar. Tarama sırasında, bir sonraki değer bulunana kadar ilk eşleşen değer dizinle birlikte atlanır.

Atlama tarama hbase filtresinumararasyon `SEEK_NEXT_USING_HINT` kullanır. Atlama, `SEEK_NEXT_USING_HINT`atlama tonu kullanılarak her sütunda hangi anahtar kümesinin veya anahtar aralıklarının arandığını izler. Atlama tarama, filtre değerlendirmesi sırasında ona geçirilen bir anahtarı alır ve kombinasyonlardan biri olup olmadığını belirler. Değilse, atlama tarak atlamak için bir sonraki en yüksek tuşu değerlendirir.

### <a name="transactions"></a>İşlemler

HBase satır düzeyinde işlemler sağlarken, Phoenix tam [ACID](https://en.wikipedia.org/wiki/ACID) semantiği ile çapraz satır ve çapraz tablo işlem desteği eklemek için [Tephra](https://tephra.io/) ile tümleştirir.

Geleneksel SQL işlemlerinde olduğu gibi, Phoenix işlem yöneticisi aracılığıyla sağlanan hareketler, bir atomik veri biriminin başarıyla yükseltilmesini ve yukarı ekleme işleminin işlem etkinleştirilmiş herhangi bir tabloda başarısız olması durumunda hareketi geri almanızı sağlar.

Phoenix işlemlerini etkinleştirmek için [Apache Phoenix işlem belgelerine](https://phoenix.apache.org/transactions.html)bakın.

Etkin leştirilmiş hareketler içeren yeni bir `TRANSACTIONAL` tablo `true` oluşturmak `CREATE` için, özelliği bir deyimde ayarlayın:

```sql
CREATE TABLE my_table (k BIGINT PRIMARY KEY, v VARCHAR) TRANSACTIONAL=true;
```

Varolan bir tabloyu işlem amaçlı olarak değiştirmek `ALTER` için, bir deyimde aynı özelliği kullanın:

```sql
ALTER TABLE my_other_table SET TRANSACTIONAL=true;
```

> [!NOTE]  
> İşlem tablosunu işlem dışı olarak geri değiştiremezsiniz.

### <a name="salted-tables"></a>Tuzlu Tablolar

*Bölge sunucusu etkin noktalama* HBase için sıralı anahtarları ile kayıtları yazarken oluşabilir. Kümenizde birden çok bölge sunucunuz olsa da, yazmalarınızın tümü tek bir sunucuda oluşur. Bu konsantrasyon, yazma iş yükünüzün kullanılabilir bölge sunucularının tümüne dağıtıldığı etkin nokta sorununu oluşturur, yalnızca bir tanesi yükü işletmektedir. Her bölge önceden tanımlanmış bir maksimum boyuta sahip olduğundan, bir bölge bu boyut sınırına ulaştığında, iki küçük bölgeye bölünür. Bu durumda, bu yeni bölgelerden biri tüm yeni kayıtları alır ve yeni etkin nokta haline gelir.

Bu sorunu azaltmak ve daha iyi performans elde etmek için, tüm bölge sunucularının eşit şekilde kullanılması için tabloları önceden böldü. Phoenix *tuzlu tablolar*sağlar , şeffaf belirli bir tablo için satır tuşuna tuzlu bayt ekleyerek. Tablo, tablonun ilk aşamasında bölge sunucuları arasında eşit yük dağılımı sağlamak için tuz bayt sınırları üzerinde önceden bölünmüştür. Bu yaklaşım, yazma ve okuma performansını artırarak, yazma iş yükünü tüm kullanılabilir bölge sunucularına dağıtır. Bir tabloyu tuzlamak `SALT_BUCKETS` için, tablo oluşturulduğunda tablo özelliğini belirtin:

```sql
CREATE TABLE Saltedweblogs (
    transactionid varchar(500) Primary Key,
    transactiondate Date NULL,
    customerid varchar(50) NULL,
    bookid varchar(50) NULL,
    purchasetype varchar(50) NULL,
    orderid varchar(50) NULL,
    bookname varchar(50) NULL,
    categoryname varchar(50) NULL,
    invoicenumber varchar(50) NULL,
    invoicestatus varchar(50) NULL,
    city varchar(50) NULL,
    state varchar(50) NULL,
    paymentamount DOUBLE NULL,
    quantity INTEGER NULL,
    shippingamount DOUBLE NULL) SALT_BUCKETS=4;
```

## <a name="enable-and-tune-phoenix-with-apache-ambari"></a>Apache Ambari ile Phoenix'i etkinleştirin ve ayarlayın

HDInsight HBase kümesi, yapılandırma değişiklikleri yapmak için [Ambari UI'yi](hdinsight-hadoop-manage-ambari.md) içerir.

1. Phoenix'i etkinleştirmek veya devre dışı kalmak ve Phoenix'in sorgu zaman sonu ayarlarını denetlemek için Hadoop kullanıcı kimlik bilgilerinizi kullanarak Ambari Web UI'de oturum`https://YOUR_CLUSTER_NAME.azurehdinsight.net`açın.

2. Sol menüdeki hizmetler listesinden **HBase'i** seçin ve ardından **Configs** sekmesini seçin.

    ![Apache Ambari HBase yapılandırmaları](./media/hdinsight-phoenix-in-hdinsight/ambari-hbase-config1.png)

3. Phoenix'i etkinleştirmek veya devre dışı kalmak için **Phoenix SQL** yapılandırma bölümünü bulun ve sorgu zaman arasını ayarlayın.

    ![Ambari Phoenix SQL yapılandırma bölümü](./media/hdinsight-phoenix-in-hdinsight/apache-ambari-phoenix.png)

## <a name="see-also"></a>Ayrıca bkz.

* [HDInsight'ta Linux tabanlı HBase kümeleriyle Apache Phoenix'i kullanma](hbase/apache-hbase-query-with-phoenix.md)

* [Azure HDInsight'ta Apache HBase üzerinden Apache Phoenix sorgularını çalıştırmak için Apache Zeppelin'i kullanın](./hbase/apache-hbase-phoenix-zeppelin.md)
