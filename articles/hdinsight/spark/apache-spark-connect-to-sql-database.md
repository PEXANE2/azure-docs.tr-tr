---
title: Azure SQL Veritabanına veri okumak ve yazmak için Apache Spark'ı kullanma
description: HDInsight Spark kümesi ile Azure SQL Veritabanı arasında nasıl bağlantı kurup kurmayı öğrenin. Verileri okumak, veri yazmak ve verileri SQL veritabanına aktarmak için
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/20/2020
ms.openlocfilehash: 4e783a233bd35e012c02fbbbdc7f4223552fc734
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686856"
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>Azure SQL Veritabanına veri okumak ve yazmak için HDInsight Spark kümesini kullanın

Azure HDInsight'taki Bir Apache Spark kümesini Azure SQL Veritabanı'na nasıl bağlayabilirsiniz öğrenin. Ardından verileri SQL veritabanına okuyun, yazın ve aktarın. Bu makaledeki yönergeler, Scala kod parçacıklarını çalıştırmak için bir Jupyter Notebook kullanır. Ancak, Scala veya Python'da bağımsız bir uygulama oluşturabilir ve aynı görevleri yapabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure HDInsight Spark kümesi.  [HDInsight'ta Apache Spark kümesi oluştur'daki](apache-spark-jupyter-spark-sql.md)yönergeleri izleyin.

* Azure SQL Veritabanı çözümünü karşılaştırabilirsiniz. Azure SQL [Veritabanı Oluştur'daki](../../sql-database/sql-database-get-started-portal.md)yönergeleri izleyin. Örnek **AdventureWorksLT** şeması ve verileri içeren bir veritabanı oluşturduğunuzdan emin olun. Ayrıca, istemcinizin IP adresinin sunucudaki SQL veritabanına erişmesine izin vermek için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturduğunuzdan emin olun. Güvenlik duvarı kuralını ekleme yönergeleri aynı makalede kullanılabilir. Azure SQL Veritabanınızı oluşturduktan sonra, aşağıdaki değerleri kullanışlı tuttuğunızdan emin olun. Bir Kıvılcım kümesinden veritabanına bağlanmak için bunlara ihtiyacınız vardır.

    * Azure SQL Veritabanı'nı barındıran sunucu adı.
    * Azure SQL Veritabanı adı.
    * Azure SQL Database admin kullanıcı adı / şifre.

* SQL Server Management Studio (SSMS). Verileri bağlamak [ve sorgulamak için SSMS'i kullan yönergelerini](../../sql-database/sql-database-connect-query-ssms.md)izleyin.

## <a name="create-a-jupyter-notebook"></a>Jupyter Notebook oluşturma

Kıvılcım kümesiyle ilişkili bir Jupyter Notebook oluşturarak başlayın. Bu not defterini, bu makalede kullanılan kod parçacıklarını çalıştırmak için kullanırsınız.

1. Azure [portalından](https://portal.azure.com/)kümenizi açın.
1. Sağ taraftaki Küme **panolarının** altındaki **Jupyter dizüstü bilgisayarını** seçin.  **Küme panolarını**görmüyorsanız, sol menüden **Genel Bakış'ı** seçin. İstenirse, küme için yönetici kimlik bilgilerini girin.

    ![Apache Spark üzerinde Jupyter dizüstü bilgisayar](./media/apache-spark-connect-to-sql-database/hdinsight-spark-cluster-dashboard-jupyter-notebook.png "Kıvılcım üzerinde Jupyter dizüstü bilgisayar")

   > [!NOTE]  
   > Ayrıca tarayıcınızda aşağıdaki URL'yi açarak Spark kümesindeki Jupyter dizüstü bilgisayarına da erişebilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

1. Jupyter dizüstü bilgisayarında, sağ üst köşeden **Yeni'yi**tıklatın ve ardından Bir Scala dizüstü bilgisayar oluşturmak için **Spark'ı** tıklatın. HDInsight Spark kümesindeki jupyter dizüstü bilgisayarlar da Python2 uygulamaları için **PySpark** çekirdeğini ve Python3 uygulamaları için **PySpark3** çekirdeğini sağlar. Bu makale için bir Scala dizüstü bilgisayar oluşturuyoruz.

    ![Kıvılcım jupyter dizüstü için Çekirdekler](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "Kıvılcım jupyter dizüstü için Çekirdekler")

    Çekirdekler hakkında daha fazla bilgi için bkz. [HDInsight’ta Apache Spark kümeleri ile Jupyter not defterleri kullanma](apache-spark-jupyter-notebook-kernels.md).

   > [!NOTE]  
   > Bu makalede, Spark'tan SQL veritabanına veri akışı yalnızca Şu anda Yalnızca Scala ve Java'da desteklendirilmediği için bir Spark (Scala) çekirdeği kullanıyoruz. Bu makalede tutarlılık için Python kullanılarak SQL'den okuma ve SQL'e yazma işlemi yapılabilse de, her üç işlem için de Scala'yı kullanıyoruz.

1. Yeni bir not defteri varsayılan adı ile açılır, **Adsız**. Not defteri adını tıklatın ve seçtiğiniz bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "Not defteri adını belirtme")

Artık uygulamanızı oluşturmaya başlayabilirsiniz.

## <a name="read-data-from-azure-sql-database"></a>Azure SQL Veritabanı'ndan verileri okuma

Bu bölümde, AdventureWorks veritabanında bulunan bir tablodaki verileri (örneğin **SalesLT.Address)** okuyun.

1. Yeni bir Jupyter not defterinde, bir kod hücresinde, aşağıdaki snippet'i yapıştırın ve yer tutucu değerlerini Azure SQL Veritabanınızın değerleriyle değiştirin.

    ```scala
    // Declare the values for your Azure SQL database

    val jdbcUsername = "<SQL DB ADMIN USER>"
    val jdbcPassword = "<SQL DB ADMIN PWD>"
    val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
    val jdbcPort = 1433
    val jdbcDatabase ="<AZURE SQL DB NAME>"
    ```

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.  

1. Spark veri çerçevesi API'lerine geçirebileceğiniz bir JDBC URL'si oluşturmak için aşağıdaki snippet'i kullanın. Kod parametreleri `Properties` tutmak için bir nesne oluşturur. Bir kod hücresine snippet yapıştırın ve çalıştırmak için **SHIFT + ENTER** tuşuna basın.

    ```scala
    import java.util.Properties

    val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
    val connectionProperties = new Properties()
    connectionProperties.put("user", s"${jdbcUsername}")
    connectionProperties.put("password", s"${jdbcPassword}")
    ```

1. Azure SQL Veritabanınızdaki bir tablodaki verilerle birlikte bir veri çerçevesi oluşturmak için aşağıdaki snippet'i kullanın. Bu snippet'te, `SalesLT.Address` **AdventureWorksLT** veritabanının bir parçası olarak kullanılabilen bir tablo kullanıyoruz. Bir kod hücresine snippet yapıştırın ve çalıştırmak için **SHIFT + ENTER** tuşuna basın.

    ```scala
    val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)
    ```

1. Artık veri şemasını almak gibi veri çerçevesi üzerinde işlemler yapabilirsiniz:

    ```scala
    sqlTableDF.printSchema
    ```

    Aşağıdaki görüntüye benzer bir çıktı görürsünüz:

    ![şema çıktısı](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "şema çıktısı")

1. En iyi 10 satırı almak gibi işlemler de yapabilirsiniz.

    ```scala
    sqlTableDF.show(10)
    ```

1. Veya veri kümesinden belirli sütunları alın.

    ```scala
    sqlTableDF.select("AddressLine1", "City").show(10)
    ```

## <a name="write-data-into-azure-sql-database"></a>Azure SQL Veritabanına veri yazma

Bu bölümde, Azure SQL Veritabanı'nda bir tablo oluşturmak ve verilerle doldurmak için kümede kullanılabilen örnek bir CSV dosyası kullanıyoruz. Örnek CSV dosyası **(HVAC.csv)** tüm HDInsight kümelerinde `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`mevcuttur.

1. Yeni bir Jupyter not defterinde, bir kod hücresinde, aşağıdaki snippet'i yapıştırın ve yer tutucu değerlerini Azure SQL Veritabanınızın değerleriyle değiştirin.

    ```scala
    // Declare the values for your Azure SQL database

    val jdbcUsername = "<SQL DB ADMIN USER>"
    val jdbcPassword = "<SQL DB ADMIN PWD>"
    val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
    val jdbcPort = 1433
    val jdbcDatabase ="<AZURE SQL DB NAME>"
    ```

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.  

1. Aşağıdaki parçacık, Spark veri çerçevesi API'lerine geçirebileceğiniz bir JDBC URL'si oluşturur. Kod parametreleri `Properties` tutmak için bir nesne oluşturur. Bir kod hücresine snippet yapıştırın ve çalıştırmak için **SHIFT + ENTER** tuşuna basın.

    ```scala
    import java.util.Properties

    val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
    val connectionProperties = new Properties()
    connectionProperties.put("user", s"${jdbcUsername}")
    connectionProperties.put("password", s"${jdbcPassword}")
    ```

1. HVAC.csv'deki verilerin şemasını ayıklamak ve csv'deki verileri bir veri çerçevesiiçinde yüklemek için şemayı kullanmak `readDf`için aşağıdaki snippet'i kullanın. Bir kod hücresine snippet yapıştırın ve çalıştırmak için **SHIFT + ENTER** tuşuna basın.

    ```scala
    val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
    val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    ```

1. Geçici `readDf` bir tablo oluşturmak için `temphvactable`veri çerçevesini kullanın. Sonra bir kovan tablosu oluşturmak için `hvactable_hive`geçici tablo kullanın.

    ```scala
    readDf.createOrReplaceTempView("temphvactable")
    spark.sql("create table hvactable_hive as select * from temphvactable")
    ```

1. Son olarak, Azure SQL Veritabanı'nda bir tablo oluşturmak için kovan tablosunu kullanın. Aşağıdaki parçacık Azure SQL `hvactable` Veritabanı'nda oluşturulur.

    ```scala
    spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)
    ```

1. SSMS kullanarak Azure SQL Veritabanı'na bağlanın `dbo.hvactable` ve orada bir veritabanı gördüğünüzden doğrulayın.

    a. Aşağıdaki ekran görüntüsünde gösterildiği gibi bağlantı ayrıntılarını sağlayarak SSMS'i başlatın ve Azure SQL Veritabanı'na bağlanın.

    ![SSMS1'i kullanarak SQL veritabanına bağlanın](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "SSMS1'i kullanarak SQL veritabanına bağlanın")

    b. **Object**Explorer'dan, oluşturulan **dbo.hvactable'ı** görmek için Azure SQL Veritabanı'nı ve Tablo düğümlerini genişletin.

    ![SSMS2'yi kullanarak SQL veritabanına bağlanın](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "SSMS2'yi kullanarak SQL veritabanına bağlanın")

1. Tablodaki sütunları görmek için SSMS'te bir sorgu çalıştırın.

    ```sql
    SELECT * from hvactable
    ```

## <a name="stream-data-into-azure-sql-database"></a>Verileri Azure SQL Veritabanına aktarın

Bu bölümde, verileri `hvactable` önceki bölümde Azure SQL Veritabanı'nda oluşturduğunuz veri akışı.

1. İlk adım olarak, `hvactable`hiçbir kayıt olduğundan emin olun. SSMS kullanarak, tabloda aşağıdaki sorguyu çalıştırın.

    ```sql
    TRUNCATE TABLE [dbo].[hvactable]
    ```

1. HDInsight Spark kümesinde yeni bir Jupyter dizüstü bilgisayar oluşturun. Bir kod hücresinde, aşağıdaki snippet yapıştırın ve sonra **SHIFT + ENTER**tuşuna basın :

    ```scala
    import org.apache.spark.sql._
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    import org.apache.spark.sql.streaming._
    import java.sql.{Connection,DriverManager,ResultSet}
    ```

1. **Biz HVAC.csv** içine veri `hvactable`akışı . HVAC.csv dosyası kümede ' `/HdiSamples/HdiSamples/SensorSampleData/HVAC/`de mevcuttur. Aşağıdaki snippet' te, önce verilerin şemasını akışlı olarak elde ediyoruz. Daha sonra, bu şema kullanarak bir akış veri çerçevesi oluştururuz. Bir kod hücresine snippet yapıştırın ve çalıştırmak için **SHIFT + ENTER** tuşuna basın.

    ```scala
    val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
    val readStreamDf = spark.readStream.schema(userSchema).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
    readStreamDf.printSchema
    ```

1. Çıktı **HVAC.csv**şema gösterir. Aynı `hvactable` şema da var. Çıktı, tablodaki sütunları listeler.

    !['hdinsight Apache Kıvılcım şeması tablosu'](./media/apache-spark-connect-to-sql-database/hdinsight-schema-table.png "Tablo şeması")

1. Son olarak, HVAC.csv'deki verileri okumak ve Azure SQL `hvactable` Veritabanı'nda aktarmak için aşağıdaki snippet'i kullanın. Bir kod hücresine parçacık yapıştırın, yer tutucu değerlerini Azure SQL Veritabanınızın değerleriyle değiştirin ve çalıştırmak için **SHIFT + ENTER** tuşuna basın.

    ```scala
    val WriteToSQLQuery  = readStreamDf.writeStream.foreach(new ForeachWriter[Row] {
        var connection:java.sql.Connection = _
        var statement:java.sql.Statement = _

        val jdbcUsername = "<SQL DB ADMIN USER>"
        val jdbcPassword = "<SQL DB ADMIN PWD>"
        val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
        val jdbcPort = 1433
        val jdbcDatabase ="<AZURE SQL DB NAME>"
        val driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
        val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"

        def open(partitionId: Long, version: Long):Boolean = {
        Class.forName(driver)
        connection = DriverManager.getConnection(jdbc_url, jdbcUsername, jdbcPassword)
        statement = connection.createStatement
        true
        }

        def process(value: Row): Unit = {
        val Date  = value(0)
        val Time = value(1)
        val TargetTemp = value(2)
        val ActualTemp = value(3)
        val System = value(4)
        val SystemAge = value(5)
        val BuildingID = value(6)  

        val valueStr = "'" + Date + "'," + "'" + Time + "'," + "'" + TargetTemp + "'," + "'" + ActualTemp + "'," + "'" + System + "'," + "'" + SystemAge + "'," + "'" + BuildingID + "'"
        statement.execute("INSERT INTO " + "dbo.hvactable" + " VALUES (" + valueStr + ")")
        }

        def close(errorOrNull: Throwable): Unit = {
        connection.close
        }
        })

    var streamingQuery = WriteToSQLQuery.start()
    ```

1. SQL Server Management Studio'da `hvactable` (SSMS) aşağıdaki sorguyu çalıştırarak verilerin aktarıldığını doğrulayın. Sorguyu her çalıştırdığınızda, tablodaki satır sayısının arta geldiğini gösterir.

    ```sql
    SELECT COUNT(*) FROM hvactable
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri Gölü Depolama'daki verileri analiz etmek için HDInsight Spark kümesini kullanın](apache-spark-use-with-data-lake-store.md)
* [Azure HDInsight'ta apache Spark kümesinde veri yükleme ve sorgu çalıştırma](apache-spark-load-data-run-query.md)
* [HDInsight'ta Apache Kafka ile Apache Spark Structured Streaming'i kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
