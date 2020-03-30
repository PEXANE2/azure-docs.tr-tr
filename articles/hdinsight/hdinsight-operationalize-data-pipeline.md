---
title: Veri analitiği ardışık hattını operasyonel hale getirir - Azure
description: Yeni veriler tarafından tetiklenen ve kısa sonuçlar üreten bir örnek veri ardışık alanı ayarlayın ve çalıştırın.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/25/2019
ms.openlocfilehash: 16c7af4d66bd550eb4a286de7c86c436b1fe10e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75922656"
---
# <a name="operationalize-a-data-analytics-pipeline"></a>Veri analizi işlem hattını kullanıma hazır hale getirme

*Veri boru hatları,* birçok veri analizi çözümlerini alt eder. Adından da anlaşılacağı gibi, bir veri ardışık ham veri alır, temizler ve gerektiği gibi yeniden şekillendirir ve daha sonra genellikle işlenen verileri depolamadan önce hesaplamalar veya toplamalar gerçekleştirir. İşlenen veriler istemciler, raporlar veya API'ler tarafından tüketilir. Veri aktarıcısı, ister zamanlamada ister yeni veriler tarafından tetiklendiğinde yinelenebilir sonuçlar sağlamalıdır.

Bu makalede, HDInsight Hadoop kümelerinde çalışan Oozie'yi kullanarak veri ardışık yollarınızın tekrarlanabilirlik için nasıl işlevselhale leştirilen bir şekilde çalıştırılabildiğini açıklanmaktadır. Örnek senaryo, havayolu uçuş zaman serisi verilerini hazırlayan ve işleyen bir veri ardışık dizisinde size yol alar.

Aşağıdaki senaryoda, giriş verileri bir ay boyunca bir yığın uçuş verisi içeren düz bir dosyadır. Bu uçuş verileri, kaynak ve varış havaalanı, uçuşlar, kalkış ve varış saatleri vb. gibi bilgileri içerir. Bu boru hattı ile amaç günlük havayolu performansı özetlemektir, her havayolu dakika içinde ortalama kalkış ve varış gecikmeleri ile her gün için bir satır vardır, ve toplam mil o gün uçtu.

| YEAR | MONTH | DAY_OF_MONTH | Taşıyıcı |AVG_DEP_DELAY | AVG_ARR_DELAY |TOTAL_DISTANCE |
| --- | --- | --- | --- | --- | --- | --- |
| 2017 | 1 | 3 | Acar | 10.142229 | 7.862926 | 2644539 |
| 2017 | 1 | 3 | AS | 9.435449 | 5.482143 | 572289 |
| 2017 | 1 | 3 | Dl | 6.935409 | -2.1893024 | 1909696 |

Örnek boru hattı, yeni bir zaman döneminin uçuş verileri gelene kadar bekler ve daha sonra bu ayrıntılı uçuş bilgilerini uzun vadeli analizler için Apache Hive veri ambarınızda saklar. Boru hattı aynı zamanda sadece günlük uçuş verilerini özetleyen çok daha küçük bir veri kümesi oluşturur. Bu günlük uçuş özeti verileri, bir web sitesi gibi raporlar sağlamak üzere bir SQL Veritabanına gönderilir.

Aşağıdaki diyagram örnek ardışık gösteriş.

![HDI uçuş örneği veri boru hattına genel bakış](./media/hdinsight-operationalize-data-pipeline/flight-pipeline-overview.png)

## <a name="apache-oozie-solution-overview"></a>Apache Oozie çözüme genel bakış

Bu ardışık işlem, HDInsight Hadoop kümesi üzerinde çalışan Apache Oozie kullanır.

Oozie *eylemler,* *iş akışları*ve *koordinatörleri*açısından boru hatları açıklar. Eylemler, Kovan sorgusu nu çalıştırmak gibi gerçek çalışmayı belirler. İş akışları eylem sırasını tanımlar. Koordinatörler, iş akışının ne zaman çalıştırılmasına yönelik zamanlamayı tanımlar. Koordinatörler, iş akışının bir örneğini başlatmadan önce yeni verilerin kullanılabilirliğini de bekleyebilirler.

Aşağıdaki diyagram, bu örnek Oozie ardışık üst düzey tasarımını gösterir.

![Oozie Uçuş örneği Veri Boru Hattı](./media/hdinsight-operationalize-data-pipeline/pipeline-overview-oozie.png)

## <a name="provision-azure-resources"></a>Azure kaynaklarını sağlama

Bu ardışık işlem için bir Azure SQL Veritabanı ve aynı konumda bir HDInsight Hadoop kümesi gerektirir. Azure SQL Veritabanı, hem ardışık hatlar tarafından üretilen özet verileri hem de Oozie Meta data Store'u depolar.

### <a name="provision-azure-sql-database"></a>Sağlama Azure SQL Veritabanı

1. Azure SQL Veritabanı oluşturun. Bkz. [Azure portalında bir Azure SQL Veritabanı Oluşturma.](../sql-database/sql-database-single-database-get-started.md)

1. HDInsight kümenizin bağlı Azure SQL Veritabanı'na erişebilmesini sağlamak için Azure hizmetlerinin ve kaynaklarının sunucuya erişmesine izin verecek şekilde Azure SQL Veritabanı güvenlik duvarı kurallarını yapılandırın. Bu seçeneği Azure portalında **Sunucu Güvenlik Duvarını Ayarla'yı**seçerek ve Azure'a İzin Ver hizmetlerinin ve kaynaklarının altındaki **A.B.'yi** seçerek Azure SQL Veritabanı sunucusu veya veritabanı için bu **sunucuya erişebilirsiniz.** Daha fazla bilgi için IP [güvenlik duvarı kuralları oluştur ve yönet'](../sql-database/sql-database-firewall-configure.md#use-the-azure-portal-to-manage-server-level-ip-firewall-rules)e bakın.

1. Ardışık her çalışmadan özetverileri depolayacak `dailyflights` tabloyu oluşturmak için aşağıdaki SQL deyimlerini yürütmek için Sorgu [düzenleyicisini](../sql-database/sql-database-single-database-get-started.md#query-the-database) kullanın.

    ```sql
    CREATE TABLE dailyflights
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER CHAR(2),
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    GO

    CREATE CLUSTERED INDEX dailyflights_clustered_index on dailyflights(YEAR,MONTH,DAY_OF_MONTH,CARRIER)
    GO
    ```

Azure SQL Veritabanınız artık hazır.

### <a name="provision-an-apache-hadoop-cluster"></a>Bir Apache Hadoop Kümesi sağlama

Özel bir metastore ile bir Apache Hadoop kümesi oluşturun. Portaldan küme oluşturma sırasında, **Depolama** sekmesinden, **Metastore ayarları**altında SQL Veritabanınızı seçtiğinizden emin olun. Metastore seçme hakkında daha fazla bilgi için [bkz.](./hdinsight-use-external-metadata-stores.md#select-a-custom-metastore-during-cluster-creation) Küme oluşturma hakkında daha fazla bilgi için, [Linux'ta HDInsight ile Başlayın'a](hadoop/apache-hadoop-linux-tutorial-get-started.md)bakın.

## <a name="verify-ssh-tunneling-set-up"></a>SSH tünel kurulumlarını doğrulayın

Koordinatörünüzün durumunu ve iş akışı örneklerini görüntülemek için Oozie Web Konsolu'nu kullanmak için HDInsight kümenize bir SSH tüneli ayarlayın. Daha fazla bilgi için [Bkz. SSH Tüneli.](hdinsight-linux-ambari-ssh-tunnel.md)

> [!NOTE]  
> SSH tünelinde kümenizin web kaynaklarına göz atmak için [Foxy Proxy](https://getfoxyproxy.org/) uzantısı ile Chrome'u da kullanabilirsiniz. Tünelin 9876 portundaki ana bilgisayar `localhost` aracılığıyla tüm isteği vekil olarak yapılandırın. Bu yaklaşım, Windows 10'da Bash olarak da bilinen Linux için Windows Alt Sistemi ile uyumludur.

1. Kümenize bir SSH tüneli açmak için aşağıdaki `CLUSTERNAME` komutu çalıştırın, kümenizin adı nerededir:

    ```cmd
    ssh -C2qTnNf -D 9876 sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Aşağıdakilere göz atarak kafa düğümünüzde Ambari'ye doğru yol alarak tünelin çalışır durumda olduğunu doğrulayın:

    `http://headnodehost:8080`

1. Ambari içinden **Oozie Web Konsolu** erişmek için, **Oozie** > **Hızlı Bağlantılar** > [Aktif sunucu] > **Oozie Web UI**gidin.

## <a name="configure-hive"></a>Hive'i Yapılandır

### <a name="upload-data"></a>Karşıya veri yükleme

1. Bir ay boyunca uçuş verilerini içeren örnek bir CSV dosyasını indirin. ZIP dosyasını `2017-01-FlightData.zip` [HDInsight GitHub deposundan](https://github.com/hdinsight/hdinsight-dev-guide) indirin ve CSV dosyasına `2017-01-FlightData.csv`indirin.

1. Bu CSV dosyasını HDInsight kümenize bağlı Azure Depolama hesabına `/example/data/flights` kopyalayın ve klasöre yerleştirin.

    1. Dosyaları yerel makinenizden HDInsight küme kafa düğümünüzün yerel depolama alanına kopyalamak için SCP'yi kullanın.

        ```cmd
        scp ./2017-01-FlightData.csv sshuser@CLUSTERNAME-ssh.azurehdinsight.net:2017-01-FlightData.csv
        ```

    1. Kümenize bağlanmak için [ssh komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. Kümenizin adını değiştirerek `CLUSTERNAME` aşağıdaki komutu düzenleme ve ardından komutu girin:

        ```cmd
        ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
        ```

    1. Ssh oturumunuzdan, dosyayı kafa düğümü yerel depolama alanınızdan Azure Depolama'ya kopyalamak için HDFS komutunu kullanın.

        ```bash
        hadoop fs -mkdir /example/data/flights
        hdfs dfs -put ./2017-01-FlightData.csv /example/data/flights/2017-01-FlightData.csv
        ```

### <a name="create-tables"></a>Tablo oluşturma

Örnek veriler artık kullanılabilir. Ancak, ardışık işlem için iki Hive tabloları, bir`rawFlights`gelen veriler için (`flights`) ve özetlenen veriler için bir ( ) gerektirir. Ambari'de bu tabloları aşağıdaki gibi oluşturun.

1. Ambari'ye giriş yaparak `http://headnodehost:8080`.

2. Hizmetler listesinden **Hive'ı**seçin.

    ![Apache Ambari hizmetleri listesi Hive seçerek](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive.png)

3. Kovan Görünümü 2.0 etiketinin yanında **Görünüme Git'i** seçin.

    ![Ambari Apache Hive özet listesi](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-summary.png)

4. Sorgu metin alanında, tablo oluşturmak için aşağıdaki `rawFlights` ifadeleri yapıştırın. Tablo, `rawFlights` Azure Depolama klasöründeki `/example/data/flights` CSV dosyaları için okundu üzerinde şema sağlar.

    ```sql
    CREATE EXTERNAL TABLE IF NOT EXISTS rawflights (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    )
    LOCATION '/example/data/flights'
    ```

5. Tablooluşturmak için Yürüt''ün'u seçin. **Execute**

    ![hdi ambari hizmetleri kovan sorgusu](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-query.png)

6. Tabloyu `flights` oluşturmak için, sorgu metni alanındaki metni aşağıdaki ifadelerle değiştirin. Tablo, yıla, `flights` aya ve ayın gününe göre yüklenen verileri bölümlere aktaran Hive tarafından yönetilen bir tablodur. Bu tablo, uçuş başına bir satırın kaynak verilerinde bulunan en düşük parçalılıkla tüm geçmiş uçuş verilerini içerir.

    ```sql
    SET hive.exec.dynamic.partition.mode=nonstrict;

    CREATE TABLE flights
    (
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT
    )
    PARTITIONED BY (YEAR INT, MONTH INT, DAY_OF_MONTH INT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES 
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    );
    ```

7. Tablooluşturmak için Yürüt''ün'u seçin. **Execute**

## <a name="create-the-oozie-workflow"></a>Oozie iş akışını oluşturma

Ardışık hatlar genellikle belirli bir zaman aralığına göre toplu olarak verileri işler. Bu durumda, boru hattı uçuş verilerini günlük olarak işler. Bu yaklaşım, giriş CSV dosyalarının günlük, haftalık, aylık veya yıllık olarak ulaşmasını sağlar.

Örnek iş akışı, uçuş verilerini üç ana adımda gün gün işler:

1. Tablo tarafından temsil edilen kaynak CSV dosyasından o günün tarih aralığına ait verileri ayıklamak için bir Hive sorgusu çalıştırın `rawFlights` ve verileri tabloya `flights` ekleyin.
2. Gün ve taşıyıcı tarafından özetlenen uçuş verilerinin bir kopyasını içeren gün için Kovan'da dinamik olarak bir evreleme tablosu oluşturmak için bir Hive sorgusu çalıştırın.
3. Hive'daki günlük evreleme tablosundaki tüm verileri Azure SQL Veritabanı'ndaki `dailyflights` hedef tabloya kopyalamak için Apache Sqoop'u kullanın. Sqoop, Azure Depolama'da bulunan Hive tablosunun arkasındaki verilerden gelen kaynak satırları okur ve bunları JDBC bağlantısı kullanarak SQL Veritabanına yükler.

Bu üç adım, Bir Oozie iş akışı tarafından koordine edilir.

1. Yerel iş istasyonunuzdan, '' adlı `job.properties`bir dosya oluşturun. Aşağıdaki metni dosyanın başlangıç içeriği olarak kullanın.
Ardından, özel ortamınız için değerleri güncelleştirin. Metnin altındaki tablo özelliklerin her birini özetler ve kendi ortamınız için değerleri nerede bulabileceğinizi gösterir.

    ```text
    nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
    jobTracker=[ACTIVERESOURCEMANAGER]:8050
    queueName=default
    oozie.use.system.libpath=true
    appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
    oozie.wf.application.path=${appBase}/load_flights_by_day
    hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
    hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
    hiveDailyTableName=dailyflights${year}${month}${day}
    hiveDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/${year}/${month}/${day}
    sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
    sqlDatabaseTableName=dailyflights
    year=2017
    month=01
    day=03
    ```

    | Özellik | Değer kaynağı |
    | --- | --- |
    | nameNode | HDInsight kümenize bağlı Azure Depolama Kapsayıcısı'na tam yol. |
    | jobTracker | Etkin kümenizin İplik baş düğümünün iç ana adı. Ambari ana sayfasında, hizmetler listesinden İpLik'i seçin ve ardından Etkin Kaynak Yöneticisi'ni seçin. Ana bilgisayar adı URI sayfanın üst kısmında görüntülenir. Bağlantı noktası 8050'yi tamamla. |
    | queueName | Hive eylemlerini zamanlarken kullanılan İplik sırasının adı. Varsayılan olarak bırakın. |
    | oozie.use.system.libpath | Doğru olarak bırak. |
    | appBase | Oozie iş akışını ve destekleyici dosyaları dağıttığınız Azure Depolama'daki alt klasöre giden yol. |
    | oozie.wf.application.path | Çalışacak Oozie iş akışının `workflow.xml` konumu. |
    | kovanScriptLoadPartition | Azure Depolama'daki Hive sorgu `hive-load-flights-partition.hql`dosyasına giden yol. |
    | kovanScriptCreateDailyTable | Azure Depolama'daki Hive sorgu `hive-create-daily-summary-table.hql`dosyasına giden yol. |
    | kovanDailyTableName | Evreleme tablosu için kullanılacak dinamik olarak oluşturulan ad. |
    | kovanDataFolder | Azure Depolama'daki yol, hazırlama tablosunun içerdiği verilere. |
    | sqlDatabaseConnectionString | Azure SQL Veritabanınıza JDBC sözdizimi bağlantı dizesi. |
    | sqlDatabaseTableName | Özet satırlarının eklendiği Azure SQL Veritabanı'ndaki tablonun adı. Olarak `dailyflights`bırakın. |
    | yıl | Uçuş özetlerinin hesaplandığı günün yıl bileşeni. Olduğu gibi git. |
    | ay | Uçuş özetlerinin hesaplandığı günün ay bileşeni. Olduğu gibi git. |
    | gün | Uçuş özetlerinin hesaplandığı günün ay bileşeni. Olduğu gibi git. |

1. Yerel iş istasyonunuzdan, '' adlı `hive-load-flights-partition.hql`bir dosya oluşturun. Aşağıdaki kodu dosyanın içeriği olarak kullanın.

    ```sql
    SET hive.exec.dynamic.partition.mode=nonstrict;

    INSERT OVERWRITE TABLE flights
    PARTITION (YEAR, MONTH, DAY_OF_MONTH)
    SELECT 
        FL_DATE,
        CARRIER,
        FL_NUM,
        ORIGIN,
        DEST,
        DEP_DELAY,
        ARR_DELAY,
        ACTUAL_ELAPSED_TIME,
        DISTANCE,
        YEAR,
        MONTH,
        DAY_OF_MONTH
    FROM rawflights
    WHERE year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    Oozie değişkenleri sözdizimini `${variableName}`kullanır. Bu değişkenler `job.properties` dosyada ayarlanır. Oozie çalışma zamanında gerçek değerleri yerine geçer.

1. Yerel iş istasyonunuzdan, '' adlı `hive-create-daily-summary-table.hql`bir dosya oluşturun. Aşağıdaki kodu dosyanın içeriği olarak kullanın.

    ```sql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER STRING,
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName}
    SELECT  year, month, day_of_month, carrier, avg(dep_delay) avg_dep_delay, 
            avg(arr_delay) avg_arr_delay, sum(distance) total_distance 
    FROM flights
    GROUP BY year, month, day_of_month, carrier 
    HAVING year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    Bu sorgu, yalnızca özetlenen verileri bir gün boyunca depolayacak, ortalama gecikmeleri ve taşıyıcı tarafından gün tarafından uçurulan mesafenin toplamını hesaplayan SELECT deyimini dikkate alacak bir hazırlama tablosu oluşturur. Bilinen bir konumda depolanan bu tabloya eklenen veriler (kovanDataFolder değişkeninin gösterdiği yol) böylece bir sonraki adımda Sqoop'un kaynağı olarak kullanılabilir.

1. Yerel iş istasyonunuzdan, '' adlı `workflow.xml`bir dosya oluşturun. Aşağıdaki kodu dosyanın içeriği olarak kullanın. Yukarıdaki bu adımlar, Oozie iş akışı dosyasında ayrı eylemler olarak ifade edilir.

    ```xml
    <workflow-app name="loadflightstable" xmlns="uri:oozie:workflow:0.5">
        <start to = "RunHiveLoadFlightsScript"/>
        <action name="RunHiveLoadFlightsScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
                </configuration>
                <script>${hiveScriptLoadPartition}</script>
                <param>year=${year}</param>
                <param>month=${month}</param>
                <param>day=${day}</param>
            </hive>
            <ok to="RunHiveCreateDailyFlightTableScript"/>
            <error to="fail"/>
        </action>
    
        <action name="RunHiveCreateDailyFlightTableScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
                </configuration>
                <script>${hiveScriptCreateDailyTable}</script>
                <param>hiveTableName=${hiveDailyTableName}</param>
                <param>year=${year}</param>
                <param>month=${month}</param>
                <param>day=${day}</param>
                <param>hiveDataFolder=${hiveDataFolder}/${year}/${month}/${day}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>
    
        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.compress.map.output</name>
                    <value>true</value>
                </property>
                </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveDataFolder}/${year}/${month}/${day}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>
        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>
        <end name="end"/>
    </workflow-app>
    ```

İki Hive sorgusuna Azure Depolama'daki yollarından erişilir ve kalan değişken `job.properties` değerleri dosya tarafından sağlanır. Bu dosya, iş akışını 3 Ocak 2017 tarihi için çalışacak şekilde yapılandırır.

## <a name="deploy-and-run-the-oozie-workflow"></a>Oozie iş akışını dağıtın ve çalıştırın

Oozie`workflow.xml`iş akışınızı (), Hive sorgularınızı ( ve`hive-load-flights-partition.hql` `hive-create-daily-summary-table.hql`iş yapılandırmanızı )`job.properties`dağıtmak için bash oturumunuzdaki SCP'yi kullanın.  Oozie'de, `job.properties` kafa düğümünün yerel depolamasında yalnızca dosya bulunabilir. Diğer tüm dosyaların HDFS'de depolanması gerekir, bu durumda Azure Depolama. İş akışı tarafından kullanılan Sqoop eylemi, kafa düğümünden HDFS'ye kopyalanmalıdır SQL Veritabanı ile iletişim kurmak için bir JDBC sürücüsüne bağlıdır.

1. Kafa `load_flights_by_day` düğümünün yerel deposunda kullanıcı yolunun altındaki alt klasörü oluşturun. Açık ssh oturumunuzdan aşağıdaki komutu uygulayın:

    ```bash
    mkdir load_flights_by_day
    ```

1. Geçerli dizindeki (ve `workflow.xml` `job.properties` dosyalardaki) tüm dosyaları `load_flights_by_day` alt klasöre kopyalayın. Yerel iş istasyonunuzdan aşağıdaki komutu uygulayın:

    ```cmd
    scp ./* sshuser@CLUSTERNAME-ssh.azurehdinsight.net:load_flights_by_day
    ```

1. İş akışı dosyalarını HDFS'ye kopyalayın. Açık ssh oturumunuzdan aşağıdaki komutları uygulayın:

    ```bash
    cd load_flights_by_day
    hadoop fs -mkdir -p /oozie/load_flights_by_day
    hdfs dfs -put ./* /oozie/load_flights_by_day
    ```

1. Yerel `mssql-jdbc-7.0.0.jre8.jar` baş düğümünden HDFS'deki iş akışı klasörüne kopyalayın. Kümenizde farklı bir kavanoz dosyası varsa, komutu gerektiği gibi gözden geçirin. Farklı `workflow.xml` bir kavanoz dosyasını yansıtmak için gerektiği gibi gözden geçirin. Açık ssh oturumunuzdan aşağıdaki komutu uygulayın:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc*.jar /oozie/load_flights_by_day
    ```

1. İş akışını çalıştırın. Açık ssh oturumunuzdan aşağıdaki komutu uygulayın:

    ```bash
    oozie job -config job.properties -run
    ```

1. Oozie Web Konsolu'nu kullanarak durumu gözlemleyin. Ambari içinden, **Oozie**seçin , **Hızlı Bağlantılar**, ve sonra **Oozie Web Konsolu**. İş **Akışı İşler** sekmesialtında, **Tüm İşler'i**seçin.

    ![hdi oozie web konsolu iş akışları](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-workflows.png)

1. Durum BAŞARILI OLDUĞUNDA, eklenen satırları görüntülemek için SQL Veritabanı tablosunu sorgula. Azure portalını kullanarak, SQL Veritabanınız için bölmeye gidin, **Araçlar'ı**seçin ve **Sorgu Düzenleyicisi'ni**açın.

        SELECT * FROM dailyflights

Artık iş akışı tek bir test günü için çalıştığına göre, bu iş akışını günlük çalışacak şekilde iş akışını zamanlayan bir koordinatörle sarabilirsiniz.

## <a name="run-the-workflow-with-a-coordinator"></a>İş akışını bir koordinatörle çalıştırma

Bu iş akışını günlük (veya tarih aralığındaki tüm günler) çalışacak şekilde zamanlamak için bir koordinatör kullanabilirsiniz. Bir koordinatör, örneğin `coordinator.xml`bir XML dosyası tarafından tanımlanır:

```xml
<coordinator-app name="daily_export" start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" timezone="UTC" xmlns="uri:oozie:coordinator:0.4">
    <datasets>
        <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
            <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
            <done-flag></done-flag>
        </dataset>
    </datasets>
    <input-events>
        <data-in name="event_input1" dataset="ds_input1">
            <instance>${coord:current(0)}</instance>
        </data-in>
    </input-events>
    <action>
        <workflow>
            <app-path>${appBase}/load_flights_by_day</app-path>
            <configuration>
                <property>
                    <name>year</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'yyyy')}</value>
                </property>
                <property>
                    <name>month</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'MM')}</value>
                </property>
                <property>
                    <name>day</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveScriptLoadPartition</name>
                    <value>${hiveScriptLoadPartition}</value>
                </property>
                <property>
                    <name>hiveScriptCreateDailyTable</name>
                    <value>${hiveScriptCreateDailyTable}</value>
                </property>
                <property>
                    <name>hiveDailyTableNamePrefix</name>
                    <value>${hiveDailyTableNamePrefix}</value>
                </property>
                <property>
                    <name>hiveDailyTableName</name>
                    <value>${hiveDailyTableNamePrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}${coord:formatTime(coord:nominalTime(), 'MM')}${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveDataFolderPrefix</name>
                    <value>${hiveDataFolderPrefix}</value>
                </property>
                <property>
                    <name>hiveDataFolder</name>
                    <value>${hiveDataFolderPrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}/${coord:formatTime(coord:nominalTime(), 'MM')}/${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>sqlDatabaseConnectionString</name>
                    <value>${sqlDatabaseConnectionString}</value>
                </property>
                <property>
                    <name>sqlDatabaseTableName</name>
                    <value>${sqlDatabaseTableName}</value>
                </property>
            </configuration>
        </workflow>
    </action>
</coordinator-app>
```

Gördüğünüz gibi, koordinatörün çoğunluğu yalnızca iş akışı örneğine yapılandırma bilgilerini geçiriyor. Ancak, dikkat etmek için birkaç önemli öğeler vardır.

* Nokta 1: `start` `end` Öğenin `coordinator-app` kendisindeki ve öznitelikleri koordinatörün üzerinde çalıştığı zaman aralığını denetler.

    ```
    <coordinator-app ... start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" ...>
    ```

    Bir koordinatör, öznitelik tarafından `start` `end` `frequency` belirtilen aralıka göre, tarih aralığı içinde eylemleri planlamaktan sorumludur. Sırayla zamanlanan her eylem, iş akışını yapılandırılmış olarak çalıştırAr. Yukarıdaki koordinatör tanımında koordinatör, 1 Ocak 2017 ile 5 Ocak 2017 tarihleri arasında eylemleri yürütecek şekilde yapılandırılmıştır. Frekans, [Oozie Expression Language](https://oozie.apache.org/docs/4.2.0/CoordinatorFunctionalSpec.html#a4.4._Frequency_and_Time-Period_Representation) frekans ifadesi `${coord:days(1)}`ile bir güne ayarlanır. Bu, koordinatöre günde bir kez bir eylem (ve dolayısıyla iş akışı) zamanlanmasıyla sonuçlanır. Bu örnekte olduğu gibi, geçmişte olan tarih aralıkları için eylem gecikmeden çalışacak şekilde zamanlanır. Bir eylemin çalıştırılması için zamanlandığı tarihin başlangıcına *nominal saat*denir. Örneğin, 1 Ocak 2017 verilerini işlemek için koordinatör 2017-01-01T00:00:00 GMT nominal zaman ile eylem zamanlayacaktır.

* Nokta 2: İş akışının tarih aralığında, `dataset` öğe belirli bir tarih aralığı için veriler için HDFS'de nerede görünüleceklerini belirtir ve Oozie'nin verilerin işleme için henüz kullanılabilir olup olmadığını nasıl belirleyip belirlemediğini yapılandırır.

    ```xml
    <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
        <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
        <done-flag></done-flag>
    </dataset>
    ```

    HDFS'deki verilere giden `uri-template` yol, öğede sağlanan ifadeye göre dinamik olarak oluşturulur. Bu koordinatörde, veri kümesiyle bir günlük bir sıklık da kullanılır. Eylemlerin zamanlandığı (ve nominal sürelerini tanımladığı) koordinatör öğesindeki başlangıç ve `initial-instance` bitiş `frequency` tarihleri denetlenirken, veri kümesinde ve veri `uri-template`kümesinde , ' ın oluşturulmasında kullanılan tarihin hesaplanması kontrol edilir. Bu durumda, ilk günün (1/1/2017) değerindeki verileri almasını sağlamak için ilk örneği koordinatörün başlamasından bir gün öncesine ayarlayın. Dataset'in tarih hesaplaması, koordinatör `initial-instance` tarafından belirlenen nominal süreyi geçmeyen en son tarihi bulana kadar (31/12/2016) veri kümesi sıklığı artışlarında (bir gün) ilerleyen değerinden (ilk eylem için 2017-01-01T00:00:00 GMT) itibaren ileri ye doğru ilerler.

    Boş `done-flag` öğe, Oozie girinveri varlığını atanan zamanda denetlediğinde, Oozie'nin verileri bir dizin veya dosya nın varlığına göre kullanıp kullanılmadığını belirlediğini gösterir. Bu durumda, bir csv dosyasının varlığıdır. Bir csv dosyası varsa, Oozie verilerin hazır olduğunu varsayar ve dosyayı işlemek için bir iş akışı örneği başlatır. CSV dosyası yoksa, Oozie verilerin henüz hazır olmadığını ve iş akışının çalışmasının bekleme durumuna geçtiğini varsayar.

* Nokta 3: `data-in` Öğe, ilişkili veri kümesi `uri-template` için değerleri değiştirirken kullanılacak belirli zaman damgasını nominal zaman olarak belirtir.

    ```xml
    <data-in name="event_input1" dataset="ds_input1">
        <instance>${coord:current(0)}</instance>
    </data-in>
    ```

    Bu durumda, örneğini, eylemin `${coord:current(0)}`nominal zamanını koordinatör tarafından planlandığı gibi kullanmaya çeviren ifadeye ayarlayın. Başka bir deyişle, koordinatör 01/01/2017 nominal bir saat ile çalışacak eylemi zamanladığında, 01/01/2017 URI şablonundaki YIL (2017) ve AY (01) değişkenlerinin yerine geçmek için kullanılan şeydir. URI şablonu bu örnek için hesaplandıktan sonra, Oozie beklenen dizinin veya dosyanın kullanılabilir olup olmadığını denetler ve iş akışının bir sonraki çalışmasını buna göre zamanlar.

Önceki üç nokta, koordinatör tarafından günlük olarak kaynak verilerin işlenmesini zamanladığı bir durum elde etmek için biraraya gelir.

* 1. Nokta: Koordinatör 2017-01-01 nominal tarihi ile başlar.

* Nokta 2: Oozie, 'de `sourceDataFolder/2017-01-FlightData.csv`mevcut olan verileri arar.

* 3. Nokta: Oozie bu dosyayı bulduğunda, 2017-01-01 verilerini işleyecek iş akışının bir örneğini zamanlar. Oozie daha sonra 2017-01-02 için işleme devam ediyor. Bu değerlendirme 2017-01-05 hariç tekrarlar.

İş akışlarında olduğu gibi, iş akışı `job.properties` tarafından kullanılan ayarların bir üst kümesine sahip bir dosyada da bir koordinatör yapılandırması tanımlanır.

```text
nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
jobTracker=[ACTIVERESOURCEMANAGER]:8050
queueName=default
oozie.use.system.libpath=true
appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
oozie.coord.application.path=${appBase}
sourceDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/
hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
hiveDailyTableNamePrefix=dailyflights
hiveDataFolderPrefix=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/
sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
sqlDatabaseTableName=dailyflights
```

Bu `job.properties` dosyada tanıtılan tek yeni özellikler şunlardır:

| Özellik | Değer kaynağı |
| --- | --- |
| oozie.coord.application.path | Çalışacak Oozie koordinatörünün `coordinator.xml` bulunduğu dosyanın konumunu gösterir. |
| kovanDailyTableNamePrefix | Evreleme tablosunun tablo adını dinamik olarak oluştururken kullanılan önek. |
| kovanDataFolderPrefix | Tüm evreleme tablolarının depolandığı yolun öneki. |

## <a name="deploy-and-run-the-oozie-coordinator"></a>Oozie Koordinatörü'nün dağıtımı ve çalıştırı

Bir koordinatörle birlikte ardışık hattı çalıştırmak için, iş akışınızı içeren klasörün bir üst katındabir klasörden çalışmanız dışında, iş akışıyla ilgili olarak benzer bir şekilde ilerleyin. Bu klasör kuralı koordinatörleri diskteki iş akışlarından ayırır, böylece bir koordinatörü farklı alt iş akışlarıyla ilişkilendirebilirsiniz.

1. Koordinatör dosyalarını kümenizin baş düğümünün yerel depolama alanına kopyalamak için yerel makinenizdeki SCP'yi kullanın.

    ```bash
    scp ./* sshuser@CLUSTERNAME-ssh.azurehdinsight.net:~
    ```

2. Kafa düğümüne SSH.

    ```bash
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Koordinatör dosyalarını HDFS'ye kopyalayın.

    ```bash
    hdfs dfs -put ./* /oozie/
    ```

4. Koordinatörü çalıştırın.

    ```bash
    oozie job -config job.properties -run
    ```

5. Oozie Web Konsolu'nu kullanarak durumu doğrulayın, bu kez **Koordinatör İşler** sekmesini ve ardından **tüm işleri**seçin.

    ![Oozie Web Konsolu Koordinatör İşleri](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-jobs.png)

6. Zamanlanan eylemlerin listesini görüntülemek için bir koordinatör örneği seçin. Bu durumda, 1/1/2017 ile 1/4/2017 aralığında nominal süreleri olan dört eylem görmeniz gerekir.

    ![Oozie Web Konsolu Koordinatör İş](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-instance.png)

    Bu listedeki her eylem, bir günlük verileri işleyen ve o günün başlangıcının nominal saatle gösterildiği iş akışı örneğine karşılık gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Apache Oozie Dokümantasyon](https://oozie.apache.org/docs/4.2.0/index.html)
