---
title: 'Öğretici: Azure HDInsight kullanarak verileri ayıklayın, dönüştürün ve yükleyin'
description: Bu eğitimde, ham bir CSV veri setinden veri ayıklamayı, Azure HDInsight'ta Apache Hive kullanarak nasıl dönüştüreceğinizi ve ardından dönüştürülmüş verileri Sqoop kullanarak Azure SQL Veritabanı'na yüklemeyi öğreneceksiniz.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: c9ed675dc970b093f6407d15b3db2ac2668c626b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74327569"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-hdinsight"></a>Öğretici: Azure HDInsight kullanarak verileri ayıklayın, dönüştürün ve yükleyin

Bu öğreticide, bir ETL işlemi gerçekleştirin: verileri ayıklamak, dönüştürmek ve yüklemek. Ham bir CSV veri dosyanı alır, Azure HDInsight kümesine aktarın, Apache Hive ile dönüştürün ve Apache Sqoop ile Azure SQL veritabanına yüklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Verileri bir HDInsight kümesine ayıklayın ve yükleyin.
> * Apache Hive kullanarak verileri dönüştürün.
> * Sqoop kullanarak verileri Azure SQL veritabanına yükleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

* **HDInsight için yapılandırılan bir Azure Veri Gölü Depolama Gen2 depolama hesabı**

    Bkz. [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanın.](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)

* **HDInsight'ta Linux tabanlı Hadoop kümesi**

    [Hızlı Başlangıç:Azure portalını kullanarak Azure HDInsight'ta Apache Hadoop ve Apache Hive ile başlayın.](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-linux-create-cluster-get-started-portal)

* **Azure SQL Veritabanı**: Hedef veri deposu olarak Azure SQL veritabanını kullanıyorsunuz. SQL veritabanınız yoksa bkz. [Azure portalında Azure SQL veritabanı oluşturma](../../sql-database/sql-database-get-started.md).

* **Azure CLI**: Azure CLI'yi yüklemediyseniz, [Install the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)bkz.

* **Güvenli Kabuk (SSH) istemcisi**: Daha fazla bilgi [için SSH kullanarak HDInsight'a (Hadoop) bağlan'a](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="download-the-flight-data"></a>Uçuş verilerini indirme

1. [Research and Innovative Technology Administration, Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time) (Araştırma ve Yenilikçi Teknolojiler İdaresi, Ulaşım İstatistikleri Bürosu) sayfasına göz atın.

2. Sayfada aşağıdaki değerleri seçin:

   | Adı | Değer |
   | --- | --- |
   | Yıl Filtresi |2013 |
   | Dönem Filtresi |Ocak |
   | Alanlar |Yıl, FlightDate, Reporting_Airline, IATA_CODE_Reporting_Airline, Flight_Number_Reporting_Airline, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, DestAirportID, DestState, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |
   
   Diğer tüm alanları temizleyin.

3. **Download** (İndir) seçeneğini belirleyin. Seçtiğiniz veri alanlarını içeren bir .zip dosyası alırsınız.

## <a name="extract-and-upload-the-data"></a>Verileri ayıklayın ve yükleyin

Bu bölümde, HDInsight kümenize veri yükler siniz ve ardından bu verileri Veri Gölü Depolama Gen2 hesabınıza kopyalayaceksiniz.

1. Bir komut istemi açın ve .zip dosyasını HDInsight küme başlığı düğümüne yüklemek için aşağıdaki Secure Copy (Scp) komutunu kullanın:

   ```bash
   scp <file-name>.zip <ssh-user-name>@<cluster-name>-ssh.azurehdinsight.net:<file-name.zip>
   ```

   * Yer `<file-name>` tutucuyu .zip dosyasının adı ile değiştirin.
   * YER `<ssh-user-name>` TUTUCUyu HDInsight kümesi için SSH girişi ile değiştirin.
   * Yer `<cluster-name>` tutucuyu HDInsight kümesinin adı ile değiştirin.

   SSH oturum açma bilgilerinizi doğrulamak için bir parola kullanıyorsanız parola girmeniz istenir.

   Ortak anahtar kullanıyorsanız, eşleşen özel anahtarın yolunu belirtmek için `-i` parametresini kullanmanız gerekebilir. Örneğin, `scp -i ~/.ssh/id_rsa <file_name>.zip <user-name>@<cluster-name>-ssh.azurehdinsight.net:`.

2. Karşıya yükleme tamamlandıktan sonra SSH kullanarak kümeye bağlanın. Komut istemine aşağıdaki komutu girin:

   ```bash
   ssh <ssh-user-name>@<cluster-name>-ssh.azurehdinsight.net
   ```

3. .zip dosyasını açmak için aşağıdaki komutu kullanın:

   ```bash
   unzip <file-name>.zip
   ```

   Komut bir **.csv** dosyasını ayıklar.

4. Veri Gölü Depolama Gen2 kapsayıcısını oluşturmak için aşağıdaki komutu kullanın.

   ```bash
   hadoop fs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/
   ```

   Yer `<container-name>` tutucuyu kabınıza vermek istediğiniz adla değiştirin.

   Yer `<storage-account-name>` tutucuyu depolama hesabınızın adı ile değiştirin.

5. Bir dizin oluşturmak için aşağıdaki komutu kullanın.

   ```bash
   hdfs dfs -mkdir -p abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data
   ```

6. *.csv* dosyasını dizine kopyalamak için aşağıdaki komutu kullanın:

   ```bash
   hdfs dfs -put "<file-name>.csv" abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data/
   ```

   Dosya adı boşluklar veya özel karakterler içeriyorsa, dosya adının etrafındaki tırnak tırnaklarını kullanın.

## <a name="transform-the-data"></a>Verileri dönüştürme

Bu bölümde, bir Apache Hive iş çalıştırmak için Beeline kullanın.

Apache Hive işinin bir parçası olarak, .csv dosyasındaki verileri **gecikmeler**adlı bir Apache Hive tablosuna aktarın.

1. HDInsight kümesi için zaten sahip olduğunuz SSH komutundan **flightdelays.hql**adlı yeni bir dosya oluşturmak ve bunları güncellemek için aşağıdaki komutu kullanın:

   ```bash
   nano flightdelays.hql
   ```

2. Yer tutucuları konteyner `<container-name>` ve `<storage-account-name>` depolama hesabı adınız ile değiştirerek aşağıdaki metni değiştirin. Ardından, sağ fare tıklama düğmesiyle birlikte SHIFT tuşuna basarak metni nano konsola kopyalayıp yapıştırın.

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION 'abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
    CREATE TABLE delays
    LOCATION 'abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/processed'
    AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

3. CTRL+X kullanarak dosyayı kaydedin `Y` ve istendiğinde yazın.

4. Hive’ı başlatmak ve **flightdelays.hql** dosyasını çalıştırmak için aşağıdaki komutu kullanın:

   ```bash
   beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
   ```

5. __flightdelays.hql__ betiği çalışmayı tamamladıktan sonra aşağıdaki komutu kullanarak etkileşimli bir Beeline oturumu açın:

   ```bash
   beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
   ```

6. `jdbc:hive2://localhost:10001/>` istemini aldığınızda, içeri aktarılan uçuş gecikme verilerini almak için aşağıdaki sorguyu kullanın:

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

   Bu sorgu, hava durumundan kaynaklanan gecikmeler yaşayan şehirlerin bir listesini, ortalama gecikme süresi ile birlikte alır ve `abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output` konumuna kaydeder. Daha sonra, Sqoop bu konumdaki verileri okur ve Azure SQL Veritabanına aktarır.

7. Beeline’dan çıkmak için isteme `!quit` girin.

## <a name="create-a-sql-database-table"></a>SQL veritabanı tablosu oluşturma

Bu işlem için SQL veritabanınızdaki sunucu adı gerekir. Sunucu adınızı bulmak için bu adımları tamamlayın.

1. [Azure portalına](https://portal.azure.com)gidin.

2. **SQL Veritabanları'nı**seçin.

3. Kullanmayı seçtiğiniz veritabanının adına filtre uygulayın. Sunucu adı, **Sunucu adı** sütununda listelenir.

4. Kullanmak istediğiniz veritabanının adına filtre uygulayın. Sunucu adı, **Sunucu adı** sütununda listelenir.

    ![Azure SQL sunucu ayrıntılarını alın](./media/data-lake-storage-tutorial-extract-transform-load-hive/get-azure-sql-server-details.png "Azure SQL sunucu ayrıntılarını alın")

    SQL Veritabanına bağlanıp tablo oluşturmanın çok sayıda yolu vardır. Aşağıdaki adımlarda HDInsight kümesinden [FreeTDS](https://www.freetds.org/) kullanılır.

5. FreeTDS yüklemek için küme ile kurulan bir SSH bağlantısından aşağıdaki komutu kullanın:

   ```bash
   sudo apt-get --assume-yes install freetds-dev freetds-bin
   ```

6. Yükleme tamamlandıktan sonra, SQL Veritabanı sunucusuna bağlanmak için aşağıdaki komutu kullanın.

   ```bash
   TDSVER=8.0 tsql -H '<server-name>.database.windows.net' -U '<admin-login>' -p 1433 -D '<database-name>'
    ```
   * Yer `<server-name>` tutucuyu SQL Veritabanı sunucu adı ile değiştirin.

   * Yer `<admin-login>` tutucuyu SQL Veritabanı için yönetici girişi ile değiştirin.

   * Yer `<database-name>` tutucuyu veritabanı adı ile değiştirme

   Size istendiğinde, SQL Database yönetici girişi için parolayı girin.

   Aşağıdakine benzer bir çıktı alırsınız:

   ```
   locale is "en_US.UTF-8"
   locale charset is "UTF-8"
   using default charset "UTF-8"
   Default database being set to sqooptest
   1>
   ```

7. İsteyerek, `1>` aşağıdaki ifadeleri girin:

   ```hiveql
   CREATE TABLE [dbo].[delays](
   [OriginCityName] [nvarchar](50) NOT NULL,
   [WeatherDelay] float,
   CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED
   ([OriginCityName] ASC))
   GO
   ```

8. `GO` deyimi girildiğinde önceki deyimler değerlendirilir.

   Sorgu, kümelenmiş dizin içeren **gecikmeler**adlı bir tablo oluşturur.

9. Tablonun oluşturulduğunu doğrulamak için aşağıdaki sorguyu kullanın:

   ```hiveql
   SELECT * FROM information_schema.tables
   GO
   ```

   Çıktı aşağıdaki metne benzer:

   ```
   TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
   databaseName       dbo             delays        BASE TABLE
   ```

10. Tsql yardımcı programından çıkış yapmak için `1>` istemine `exit` girin.

## <a name="export-and-load-the-data"></a>Verileri dışa aktarma ve yükleme

Önceki bölümlerde, dönüştürülmüş verileri konumdaki `abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output`verileri kopyaladınız. Bu bölümde, verileri Azure SQL veritabanında oluşturduğunuz `abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output` tabloya aktarmak için Sqoop'u kullanırsınız.

1. Sqoop’un SQL veritabanınızı görebildiğini doğrulamak için aşağıdaki komutu kullanın:

   ```bash
   sqoop list-databases --connect jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433 --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD>
   ```

   Komut, **gecikmeler** tablosunu oluşturduğunuz veritabanı da dahil olmak üzere veritabanlarının listesini döndürür.

2. **Kovan örnekleme tablosundan** **gecikmeler** tablosuna veri aktarmak için aşağıdaki komutu kullanın:

   ```bash
   sqoop export --connect 'jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433;database=<DATABASE_NAME>' --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD> --table 'delays' --export-dir 'abfs://<container-name>@.dfs.core.windows.net/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
   ```

   Sqoop, **gecikmeler** tablosunu içeren veritabanına bağlanır ve verileri `/tutorials/flightdelays/output` dizinden **gecikmeler** tablosuna aktarın.

3. `sqoop` Komut bittikten sonra, veritabanına bağlanmak için tsql yardımcı programını kullanın:

   ```bash
   TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -P <ADMIN_PASSWORD> -p 1433 -D <DATABASE_NAME>
   ```

4. Verilerin **gecikmeler** tablosuna aktarıldığını doğrulamak için aşağıdaki ifadeleri kullanın:

   ```sql
   SELECT * FROM delays
   GO
   ```

   Tabloda verilerin listesini görürsünüz. Tablo, şehir adını ve bu şehre ait ortalama uçuş gecikme süresini içerir.

5. Tsql yardımcı programını çıkmak için girin. `exit`

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide kullanılan tüm kaynaklar önceden var olandır. Temizlik gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta verilerle çalışmanın daha fazla yolunu öğrenmek için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
