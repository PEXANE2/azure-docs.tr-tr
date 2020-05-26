---
title: "Öğretici: Azure SYNAPSE Analytics 'i kullanmaya başlama"
description: Azure 'da temel kavramları hızlı bir şekilde anlamak için adımlara göre adımlar SYNAPSE
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: quickstart
ms.date: 05/19/2020
ms.openlocfilehash: e826075d2f0032f796ebe6d2c8648130e5b453b7
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83800899"
---
# <a name="getting-started-with-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'i kullanmaya başlama

Bu öğretici, Azure SYNAPSE Analytics 'i kurmak ve kullanmak için gereken tüm temel adımlarda size kılavuzluk eder.

## <a name="prepare-a-storage-account-for-use-with-a-synapse-workspace"></a>Bir depolama hesabını bir Synapse çalışma alanıyla kullanılmak üzere hazırlama

* [Azure Portal](https://portal.azure.com) açın
* Aşağıdaki ayarlarla yeni bir depolama hesabı oluşturun:
    * **Temel bilgiler** sekmesinde

    |Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    |**Depolama hesabı adı**| Herhangi bir ad verebilirsiniz.|Bu belgede, bu belgeye olarak başvuracağız `contosolake` .
    |**Hesap türü**|Şu şekilde ayarlanmalıdır`StorageV2`||
    |**Konum**|Herhangi bir konum seçebilirsiniz| SYNAPSE çalışma alanınızın ve Azure Data Lake Storage (ADLS) Gen2 hesabının aynı bölgede olması önerilir.|
    ||||

    * **Gelişmiş** sekmesinde

    |Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    |**Data Lake Storage Gen2**|`Enabled`| Azure SYNAPSE, yalnızca bu ayarın etkinleştirildiği depolama hesaplarıyla birlikte kullanılabilir.|
    ||||

* Depolama hesabı oluşturulduktan sonra, bu rol atamalarını yapın veya zaten atandıklarından emin olun. Depolama hesabında, sol gezinti bölmesinde **erişim denetimi (IAM)** seçeneğini belirleyin.
    * Depolama hesabındaki **sahibi** rolüne kendinizi atayın
    * Depolama hesabındaki **Depolama Blobu veri sahibi** rolüne kendinizi atayın
* Sol gezinti bölmesinde **kapsayıcılar** ' ı seçin ve bir kapsayıcı oluşturun. Herhangi bir ad verebilirsiniz. Varsayılan **genel erişim düzeyini**kabul edin. Bu belgede, kapsayıcıyı çağıracağız `users` . **Oluştur**’u seçin. 

## <a name="create-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

* [Azure Portal](https://portal.azure.com) açın ve için en üstteki arama ' yı açın `Synapse` .
* Arama sonuçlarında **Hizmetler**altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** seçeneğini belirleyin.
* **+ Ekle** ' yi seçin
* **Temel bilgiler** sekmesi:

    |Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    |**Çalışma alanı adı**|Her şeyi çağırabilirsiniz.| Bu belgede, şunu kullanacağız`myworkspace`
    |**Bölge**|Depolama hesabının bölgesiyle Eşleştir||
    |||

* **Data Lake Storage Gen 2** ' yi seçin altında, daha önce oluşturduğunuz hesabı ve kapsayıcıyı seçin

> [!NOTE]
> Burada seçilen depolama hesabı, SYNAPSE çalışma alanının "birincil" depolama hesabı olarak anılacaktır.

* **İncele ve oluştur**’u seçin. **Oluştur**’u seçin. Çalışma alanınız birkaç dakika içinde hazırlanacaktır.

## <a name="verify-the-synapse-workspace-msi-has-access-to-the-storage-account"></a>SYNAPSE çalışma alanı MSI ' nin depolama hesabına erişimi olduğunu doğrulayın

Bu, sizin için zaten yapılmış olabilir. Herhangi bir durumda, doğrulamanız gerekir.

* Çalışma alanınız için seçilen birincil depolama hesabını açmak [Azure Portal](https://portal.azure.com) açın.
* Aşağıdaki atamanın mevcut olduğundan emin olun veya yoksa oluşturun
    * Depolama hesabındaki Depolama Blobu veri katılımcısı rolü çalışma alanınıza.
    * Bu rolü çalışma alanına atamak için, Depolama Blobu veri katılımcısı rolünü seçin, varsayılan **erişim ata** ' yı seçin ve **Seç** kutusuna çalışma alanınızın adını yazın. **Kaydet**’i seçin.
    
## <a name="launch-synapse-studio"></a>SYNAPSE Studio 'Yu Başlat

SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:
* [Azure Portal](https://portal.azure.com) SYNAPSE çalışma alanınızı açın ve **genel bakış** bölümünün üst kısmında **SYNAPSE Studio 'yu Başlat** ' ı seçin.
* Doğrudan https://web.azuresynapse.net çalışma alanınıza gidin ve oturum açın.

## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma

* SYNAPSE Studio 'da, sol taraftaki **> SQL havuzlarını Yönet** ' e gidin
* NOTE: tüm SYNAPSE çalışma alanları, **SQL isteğe bağlı**olarak adlandırılan önceden oluşturulmuş bir havuz ile gelir.
* **+ Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**SQL havuzu adı**| `SQLDB1`|
    |**Performans düzeyi**|`DW100C`|
* **Gözden geçir + oluştur** ' u ve ardından **Oluştur**' u seçin.
* Havuzunuz birkaç dakika içinde hazırlanacaktır.

> [!NOTE]
> Bir Synapse SQL havuzu, "Azure SQL veri ambarı" olarak adlandırılabilecek şekilde karşılık gelir

* Bir SQL havuzu, çalıştığı sürece faturalanabilir kaynakları tüketir. Bu nedenle, maliyetleri azaltmak için gerektiğinde havuzu duraklatabilirsiniz.
* SQL havuzunuz oluşturulduğunda, **SQLDB1**ADLı bir SQL havuzu veritabanı ile ilişkilendirilir.

## <a name="create-an-apache-spark-pool-for-azure-synapse-analytics"></a>Azure SYNAPSE Analytics için Apache Spark havuzu oluşturma

* SYNAPSE Studio 'da, sol taraftaki **> yönet Apache Spark havuzlar** ' ı seçin.
* **+ Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**Apache Spark havuzu adı**|`Spark1`
    |**Düğüm boyutu**| `Small`|
    |**Düğüm sayısı**| En az 3 ve en fazla 3 olarak ayarlayın|
    |||

* **Gözden geçir + oluştur** ' u ve ardından **Oluştur**' u seçin.
* Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

> [!NOTE]
> Ada karşın, bir Apache Spark havuzu SQL havuzu gibi değildir. SYNAPSE çalışma alanını Spark ile nasıl etkileşim kuracağınızı bilgilendirmek için kullandığınız bir temel meta veriler vardır. 

* Meta veri Spark havuzları olduklarından başlatılamaz veya durdurulamaz. 
* SYNAPSE içinde herhangi bir Spark etkinliği gerçekleştirdiğinizde, kullanmak için bir Spark havuzu belirtirsiniz. Havuz, kaç Spark kaynağı kullanılacağını SYNAPSE bildirir. Yalnızca kullanılan kaynaklar için ödeme yaparsınız. Havuzu kullanmayı etkin bir şekilde durdurduğunuzda kaynaklar otomatik olarak zaman aşımına uğrar ve geri dönüştürülür.
> [!NOTE]
> Spark veritabanları Spark havuzlarından bağımsız olarak oluşturulur. Çalışma alanı her zaman **varsayılan** olarak adlandırılan BIR Spark veritabanına sahiptir ve ek Spark veritabanları oluşturabilirsiniz.

## <a name="sql-on-demand-pools"></a>İsteğe bağlı SQL havuzları

İsteğe bağlı SQL, her zaman bir Synapse çalışma alanıyla kullanılabilen özel bir tür SQL havuzudur. SYNAPSE SQL havuzu oluşturmak veya yönetmek zorunda kalmadan SQL ile çalışmanıza olanak sağlar.

> [!NOTE]
> Diğer havuzların aksine, isteğe bağlı SQL için faturalama, sorguyu yürütmek için kullanılan kaynak sayısını değil, sorguyu çalıştırmak için taranan veri miktarına bağlıdır.

* İsteğe bağlı SQL, tüm SQL isteğe bağlı havuzlarından bağımsız olarak mevcut olan SQL isteğe bağlı veritabanlarına sahiptir.
* Şu anda bir çalışma alanı her zaman **isteğe bağlı SQL**ADLı bir SQL isteğe bağlı havuzu içerir.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>NYC TAXI örnek verilerini SQLDB1 veritabanına yükleme

* SYNAPSE Studio 'da, en üstteki mavi menüsünde **?** seçeneğini belirleyin. simgesini seçin.
* Kullanmaya başlama **> başlangıç hub 'ını** seçin
* **Sorgu örnek verileri** etiketli kartta, adlı SQL havuzunu seçin`SQLDB1`
* **Sorgu verileri**' ni seçin. "Örnek verileri yükleme" diyerek görüntülenen ve sonra kaybolan bir bildirim görürsünüz.
* SYNAPSE Studio 'nun en üstünde, verilerin SQLDB1 'e yüklendiğini belirten açık mavi bir bildirim çubuğu göreceksiniz. Yeşil dönüşene kadar bekleyin ve ardından kapatın.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>SQL havuzundaki NYC TAXI verilerini keşfet

* SYNAPSE Studio 'da **veri** merkezine gidin
* **SQLDB1 > tablolarına**gidin. Birkaç tablo yüklendiğini görürsünüz.
* Dbo öğesine sağ tıklayın **. Seyahat** tablosu ve **Yeni SQL betiği Seç > Ilk 100 satır seçin**
* Yeni bir SQL betiği oluşturulup otomatik olarak çalıştırılacak.
* SQL **komut dosyasının en üstünde, SQLDB1** adlı SQL havuzuna otomatik olarak ayarlandığını unutmayın.
* SQL komut dosyasının metnini bu kodla değiştirin ve çalıştırın.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

* Bu sorgu, toplam seyahat mesafeleri ve ortalama seyahat mesafesinin, pascların sayısıyla ilişkisini gösterir
* SQL komut dosyası sonucu penceresinde, sonuçların bir çizgi grafik olarak görselliğini görmek için **görünümü** **grafik** olarak değiştirin

## <a name="create-a-spark-database-and-load-the-nyc-taxi-data-into-it"></a>Spark veritabanı oluşturun ve bu veritabanına NYC TAXI verilerini yükleyin

Bir SQL havuzu veritabanında kullanılabilir veri var. Şimdi bunu bir Spark veritabanına yükledik.

* SYNAPSE Studio 'da, * * geliştirme merkezi ' ne gidin.
* Seçin **+** ve **Not defteri** seçin
* Not defterinin en üstünde, **Ekle** değerini olarak ayarlayın`Spark1`
* **Kod Ekle** ' yi seçerek bir not defteri kod hücresi ekleyin ve aşağıdaki metni yapıştırın:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

 * Veri merkezine gidin, veritabanlarına sağ tıklayın ve **Yenile** ' yi seçin.
 * Şimdi şu veritabanlarını görmeniz gerekir:
     * SQLDB (SQL havuzu)
     * nyctaxi (Spark)
      
 ## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

 * Not defterinize geri dön
 * Yeni bir kod hücresi oluşturun, aşağıdaki metni girin ve hücreyi çalıştırın

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

 * Daha önce SQL havuzuyla yaptığımız analizi gerçekleştirmek için bu kodu çalıştırın

   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
    """) 
    display(df)
    df.write.saveAsTable("nyctaxi.passengercountstats")
    ```

 * Hücre sonuçlarında, görselleştirilen verileri görmek için **grafik** ' i seçin
 
## <a name="customize-data-visualization-data-with-spark-and-notebooks"></a>Spark ve Not defterleri ile veri görselleştirme verilerini özelleştirme

Spark Not defterleri sayesinde, tam olarak işleme grafiklerini denetleyebilirsiniz. Aşağıdaki kod, Matplotlib ve Sea-doğumu popüler kitaplıklarını kullanarak basit bir örnek gösterir. SQL sorgularını daha önce çalıştırırken gördüğünüz grafiği de işleyecek şekilde işleyebilir.

```py
%%pyspark
import matplotlib.pyplot
import seaborn

seaborn.set(style = "whitegrid")
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.toPandas()
seaborn.lineplot(x="PassengerCount", y="SumTripDistance" , data = df)
seaborn.lineplot(x="PassengerCount", y="AvgTripDistance" , data = df)
matplotlib.pyplot.show()
```
    
## <a name="load-data-from-a-spark-table-into-a-sql-pool-table"></a>Spark tablosundan SQL havuz tablosuna veri yükleme

Daha önce bir SQL havuzu veritabanından Spark DB 'ye veri kopyaladık. Spark kullanarak, verileri nyctaxi. passengercountstats içine toplandık. Artık aşağıdaki hücreyi bir not defterinde çalıştırın ve toplanan tabloyu SQL havuzu veritabanına geri kopyalayacaktır.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>SQL-on talebini kullanarak Spark veritabanlarında NYC TAXI verilerini çözümleme 

* Spark veritabanlarındaki tablolar, isteğe bağlı SQL tarafından otomatik olarak görünür ve sorgulanabilir
* SYNAPSE Studio 'da geliştirme merkezine gidin ve yeni bir SQL betiği oluşturun
* **İsteğe bağlı SQL** 'e **bağlanma** ayarla 
* Aşağıdaki metni betiğe yapıştırın:

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

* **Çalıştır** 'ı seçin
* NOTE: Bu işlem ilk kez çalıştırıldığında, sorguları çalıştırmak için gereken SQL kaynaklarını toplamak üzere SQL isteğe bağlı olarak 10 saniye sürer. Sonraki sorgularda bu süre gerekli olmayacaktır.
  
## <a name="use-pipelines-to-orchestrate-activities"></a>Etkinlikleri düzenlemek için işlem hatlarını kullanma

Azure SYNAPSE 'te çok çeşitli görevleri düzenleyebilirsiniz. Bu bölümde, ne kadar kolay olduğunu göreceksiniz.

* SYNAPSE Studio 'da, Orchestrate hub 'ına gidin.
* **+** Sonra Işlem **hattı**' nı seçin. Yeni işlem hattı oluşturulur.
* Geliştirme merkezine gidin ve daha önce oluşturduğunuz not defterlerini bulun.
* Bu Not defterini ardışık düzene sürükleyin.
* İşlem hattında **tetikleyici ekle > yeni/Düzenle**' yi seçin.
* **Tetikleyici Seç** ' de **Yeni**' yi seçin ve ardından yineleme ' de tetikleyiciyi her 1 saatte bir çalışacak şekilde ayarlayın.
* **Tamam**’ı seçin.
* **Tümünü Yayımla** ' yı seçtiğinizde işlem hattı her saat çalışacaktır.
* İşlem hattını bir sonraki saat beklemek zorunda kalmadan şimdi çalıştırmak istiyorsanız, **Yeni/düzenle > tetikleyici Ekle**' yi seçin.

## <a name="working-with-data-in-a-storage-account"></a>Depolama hesabındaki verilerle çalışma

Şimdiye kadar, senaryolar veritabanlarında yer alan senaryolar ele alınmıştır. Azure SYNAPSE 'in bir depolama hesabındaki basit dosyaları nasıl çözümleyebildiğimiz gösterilmektedir. Bu senaryoda, çalışma alanını bağladığımız depolama hesabını ve kapsayıcıyı kullanacağız.

Depolama hesabının adı: contosolake depolama hesabındaki kapsayıcının adı: kullanıcılar

### <a name="creating-csv-and-parquet-files-in-your-storage-account"></a>Depolama hesabınızda CSV ve Parquet dosyaları oluşturma

Aşağıdaki kodu bir not defterinde çalıştırın. Depolama hesabında bir CSV ve Parquet verileri oluşturur

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyzing-data-in-a-storage-account"></a>Depolama hesabındaki verileri analiz etme

* SYNAPSE Studio 'da **veri** merkezine gidin
* **Bağlı** seçin
* **> çalışmaalanıadı (birincil-contosolake) depolama hesaplarına** gidin
* **Kullanıcıları Seç (birincil) "**
* ' NYCTaxi ' adlı bir klasör görmeniz gerekir. İçinde ' PassengerCountStats. csv ' ve ' PassengerCountStats. parquet ' adlı iki klasör görmeniz gerekir.
* ' PassengerCountStats. parquet ' klasörüne gidin.
* İçindeki Parquet dosyasına sağ tıklayın ve yeni not defteri ' ni seçin, şöyle bir hücre içeren bir not defteri oluşturur:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

* Parquet dosyasını Spark ile çözümlemek için hücreyi çalıştırın.
* İçindeki Parquet dosyasına sağ tıklayın ve yeni SQL betiği ' ni seçin **> en üstteki 100 satırları seçmek**için şöyle bir hücre içeren bir not defteri oluşturulur:

    ```py
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```
    
* Betik, isteğe bağlı olarak **SQL** 'e eklenecektir betiği çalıştırır. Parquet dosyasından şemayı içerdiğine dikkat edin.

## <a name="visualize-data-with-power-bi"></a>Power BI ile verileri görselleştirme

Verileriniz artık Power BI ' de kolayca analiz edilebilir ve görselleştirilir. SYNAPSE, Power BI çalışma alanını size SYNAPSE çalışma alanına bağlayabilmeniz için benzersiz bir tümleştirme sunar. Başlamadan önce, Power BI çalışma alanınızı bağlamak için öncelikle bu [hızlı](quickstart-power-bi.md) başlangıçta bulunan adımları izleyin.

### <a name="create-a-power-bi-workspace-and-link-it-to-your-synapse-workspace"></a>Power BI çalışma alanı oluşturun ve SYNAPSE çalışma alanınıza bağlayın

* [Powerbi.Microsoft.com](https://powerbi.microsoft.com/)'de oturum açın.
* Adlı yeni bir Power BI çalışma alanı oluşturun `NYCTaxiWorkspace1` .
* SYNAPSE Studio 'da **> bağlı hizmetleri Yönet**' e gidin.
* **+ Yeni** ' yi seçin ve **Power BI Bağlan** ' ı seçin ve bu alanları ayarlayın:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**Adı**|`NYCTaxiWorkspace1`|
    |**Çalışma alanı adı**|`NYCTaxiWorkspace1`|
    |||
    
* **Oluştur**’u seçin.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-synapse-workspace"></a>SYNAPSE çalışma alanınızdaki verileri kullanan bir Power BI veri kümesi oluşturma

* SYNAPSE Studio 'da **geliştirme > Power BI**gidin.
* **NYCTaxiWorkspace1 > Power BI veri kümelerine** gidin ve **Yeni Power BI veri kümesi ' ni**seçin.
* SQLDB1 veritabanının üzerine gelin ve **. pbıd dosyasını indir**' i seçin.
* İndirilen dosyayı açın `.pbids` . Bu, masaüstü Power BI başlatacaktır ve SYNAPSE çalışma alanınızda SQLDB1 'e otomatik olarak bağlanır.
* **SQL Server veritabanı**olarak adlandırılan bir iletişim kutusu görürseniz:
    * **Microsoft hesabı**seçin. 
    * **Oturum aç** ' ı seçin ve oturum açın.
    * **Bağlan**'ı seçin.
* **Gezgin** iletişim kutusu açılır. Bu, **Passengercountstats** tablosuna göz atın ve **Yükle**' yi seçin.
* **Bağlantı ayarları** iletişim kutusu görüntülenir. **DirectQuery** 'yi seçin ve **Tamam 'ı** seçin
* Sol taraftaki **rapor** düğmesini seçin.
* Raporunuza **çizgi grafik** ekleyin.
    * **Passsengercount** sütununu, **eksen > görselleştirmelere** sürükleyin
    * **Sumüçlü mesafeyi** ve **avgüçlü uzaklığı** sütunlarını, **değerler > görselleştirmelere**sürükleyin.
* **Giriş** sekmesinde **Yayımla**' yı seçin.
* Değişikliklerinizi kaydetmek isteyip istemediğinizi sorar. **Kaydet**’i seçin.
* Bir dosya adı seçmeniz istenir. Seçin `PassengerAnalysis.pbix` ve **Kaydet**' i seçin.
* **Bir hedef** seçip Seç ' i seçmenizi ister `NYCTaxiWorkspace1` . **Select**
* Yayımlamanın bitmesini bekleyin.

### <a name="configure-authentication-for-your-dataset"></a>Veri kümeniz için kimlik doğrulamasını yapılandırma

* [Powerbi.Microsoft.com](https://powerbi.microsoft.com/) açın ve **oturum açın**
* Sol tarafta, **çalışma alanları** ' nın altında, `NYCTaxiWorkspace1` yayımladığınız çalışma alanını seçin.
* Bu çalışma alanının içinde adlı bir veri kümesi `Passenger Analysis` ve adlı bir rapor görmeniz gerekir `Passenger Analysis` .
* `PassengerAnalysis`Veri kümesinin üzerine gelin ve üç nokta ile simgeyi seçin ve **Ayarlar**' ı seçin.
* **Veri kaynağı kimlik bilgileri** ' nde kimlik doğrulama yöntemini **OAuth2** olarak ayarlayın ve **oturum aç '** ı seçin.

### <a name="edit-a-report-report-in-synapse-studio"></a>SYNAPSE Studio 'da rapor raporu düzenleme

* SYNAPSE Studio 'ya dönün ve şimdi **Kapat ve Yenile '** yi seçerek şunları görmeniz gerekir:
    * **Power BI veri kümeleri**altında, **Passengeranalysis**adlı yeni bir veri kümesi.
    * **Power BI veri kümeleri**altında, **Passengeranalysis**adlı yeni bir rapor.
* **Passengeranalysis** raporuna tıklayın. 
    * Yine de veri kümesi için kimlik doğrulaması yapılandırmanız gerektiğinden hiçbir şey gösterilmez.
* SynapseStudio ' de, **çalışma alanınızın adı > Power BI raporlar > > geliştirme Power BI ' a**gidin.
* Power BI raporunu gösteren tüm pencereleri kapatın.
* **Power BI Reports** düğümünü yenileyin.
* Raporu seçin ve artık raporu doğrudan SYNAPSE Studio içinde düzenleyebilirsiniz.

## <a name="monitor-activities"></a>Etkinlikleri izleme

* SYNAPSE Studio 'da izleyici hub 'ına gidin.
* Bu konumda, çalışma alanında gerçekleşen tüm etkinliklerin geçmişini ve bunların şimdi etkin olduğunu görebilirsiniz.
* İşlem **hattı çalıştırmalarını**, **Apache Spark uygulamalarını**ve **SQL isteklerini** inceleyin ve çalışma alanında daha önce ne yaptığını görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE Analytics (Önizleme)](overview-what-is.md) hakkında daha fazla bilgi edinin

