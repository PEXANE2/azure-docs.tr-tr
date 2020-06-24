---
title: "Öğretici: Azure SYNAPSE Analytics 'i kullanmaya başlama"
description: Bu öğreticide, Azure SYNAPSE Analytics 'i ayarlamaya ve kullanmaya yönelik temel adımları öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 05/19/2020
ms.openlocfilehash: daa8b594b06203c7de9a9b462be469dd71ed2e49
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84791869"
---
# <a name="get-started-with-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'i kullanmaya başlama

Bu öğretici, Azure SYNAPSE Analytics 'i ayarlamaya ve kullanmaya yönelik temel adımlarda size rehberlik eder.

## <a name="prepare-a-storage-account"></a>Depolama hesabı hazırlama

1. [Azure Portal](https://portal.azure.com)açın.
1. Aşağıdaki ayarlara sahip yeni bir depolama hesabı oluşturun:

    |Tab|Ayar | Önerilen değer | Description |
    |---|---|---|---|
    |Temel Bilgiler|**Depolama hesabı adı**| Herhangi bir ad verebilirsiniz.|Bu belgede, bu belgeye **contosolake**olarak başvuracağız.|
    |Temel Bilgiler|**Hesap türü**|**StorageV2**olarak ayarlanmalıdır.||
    |Temel Bilgiler|**Konum**|Herhangi bir konum seçin.| Azure SYNAPSE Analytics çalışma alanınızı ve Azure Data Lake Storage 2. hesabınızın aynı bölgede olmasını öneririz.|
    |Gelişmiş|**Data Lake Storage 2. Nesil**|**Etkin**| Azure SYNAPSE, yalnızca bu ayarın etkinleştirildiği depolama hesaplarıyla birlikte kullanılabilir.|
    |||||

1. Depolama hesabını oluşturduktan sonra sol bölmede **erişim denetimi (IAM)** seçeneğini belirleyin. Ardından, aşağıdaki rolleri atayın veya zaten atanmış olduklarından emin olun:
    1. Kendinizi **sahip** rolüne atayın.
    1. Kendinizi **Depolama Blobu veri sahibi** rolüne atayın.
1. Sol bölmede **kapsayıcılar** ' ı seçin ve bir kapsayıcı oluşturun.
1. Kapsayıcıya bir ad verebilirsiniz. Bu belgede kapsayıcı **kullanıcılarını**adı vereceğiz.
1. Varsayılan ayar olan **genel erişim düzeyini**kabul edin ve **Oluştur**' u seçin.

Aşağıdaki adımda, Azure SYNAPSE çalışma alanınızı, bu depolama hesabını "birincil" depolama hesabı ve çalışma alanı verilerini depolayacak kapsayıcı olarak kullanacak şekilde yapılandıracaksınız. Çalışma alanı, verileri Apache Spark tablolarında depolar. Spark uygulama günlüklerini **/SYNAPSE/WorkspaceName**adlı bir klasörde depolar.

## <a name="create-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

1. [Azure Portal](https://portal.azure.com)açın ve en üstteki **SYNAPSE**için arama yapın.
1. Arama sonuçlarında **Hizmetler**altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** öğesini seçin.
1. Bu ayarları kullanarak bir çalışma alanı oluşturmak için **Ekle** ' yi seçin:

    |Tab|Ayar | Önerilen değer | Description |
    |---|---|---|---|
    |Temel Bilgiler|**Çalışma alanı adı**|Her şeyi çağırabilirsiniz.| Bu belgede, **MyWorkspace**kullanacağız.|
    |Temel Bilgiler|**Geli**|Depolama hesabının bölgesiyle eşleştirin.|

1. **Data Lake Storage Gen 2**' yi seçin altında, daha önce oluşturduğunuz hesabı ve kapsayıcıyı seçin.
1. **Gözden geçir +** oluştur oluştur ' u seçin  >  **Create**. Çalışma alanınız birkaç dakika içinde hazırlanıyor.

## <a name="verify-access-to-the-storage-account"></a>Depolama hesabına erişimi doğrulama

Azure SYNAPSE çalışma alanınız için Yönetilen kimlikler zaten depolama hesabına erişebilmiş olabilir. Aşağıdakileri sağlamak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) ve çalışma alanınız için seçilen birincil depolama hesabını açın.
1. Sol bölmeden **erişim denetimi (IAM)** seçeneğini belirleyin.
1. Aşağıdaki rolleri atayın veya zaten atandıklarından emin olun. Çalışma alanı kimliği ve çalışma alanı adı için aynı adı kullanıyoruz.
    1. Depolama hesabındaki **Depolama Blobu veri katılımcısı** rolü için çalışma alanı kimliği olarak **MyWorkspace** ' i atayın.
    1. Çalışma alanı adı olarak **MyWorkspace** atayın.

1. **Kaydet**’i seçin.

## <a name="open-synapse-studio"></a>SYNAPSE Studio 'Yu açın

Azure SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:

* [Azure Portal](https://portal.azure.com)SYNAPSE çalışma alanınızı açın. **Genel bakış** bölümünün üst kısmında, **SYNAPSE Studio 'yu Başlat**' ı seçin.
* https://web.azuresynapse.netÇalışma alanınıza gidin ve oturum açın.

## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **SQL havuzlarını**Yönet ' i seçin.
1. **Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**SQL havuzu adı**| **SQLDB1**|
    |**Performans düzeyi**|**DW100C**|
    |||

1. **Gözden geçir +** oluştur oluştur ' u seçin  >  **Create**. SQL havuzunuz birkaç dakika içinde hazırlanacaktır. SQL havuzunuz, **SQLDB1**olarak da BILINEN bir SQL havuzu veritabanıyla ilişkilendirilir.

Bir SQL havuzu, etkin olduğu sürece faturalanabilir kaynakları tüketir. Daha sonra maliyetleri azaltmak için havuzu duraklatabilirsiniz.

## <a name="create-an-apache-spark-pool"></a>Apache Spark havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki bölmede **Manage**  >  **Apache Spark havuzlarını**Yönet ' i seçin.
1. **Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|---|
    |**Apache Spark havuzu adı**|**Spark1**
    |**Düğüm boyutu**| **Küçük**|
    |**Düğüm sayısı**| En az 3 ve en fazla 3 olarak ayarlayın|

1. **Gözden geçir +** oluştur oluştur ' u seçin  >  **Create**. Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

> [!NOTE]
> Ada karşın, bir Apache Spark havuzu SQL havuzu gibi değildir. Azure SYNAPSE çalışma alanına Spark ile nasıl etkileşim kuracağınızı söylemek için kullandığınız bazı temel meta veriler.

Meta veriler olduklarından Spark havuzları başlatılamaz veya durdurulamaz.

Azure SYNAPSE 'te Spark etkinliği gerçekleştirdiğinizde, kullanmak için bir Spark havuzu belirtirsiniz. Havuz, Azure SYNAPSE 'in kaç Spark kaynağı kullandığını söyler. Yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Havuzu kullanmayı etkin bir şekilde durdurduğunuzda, kaynakların otomatik olarak zaman aşımına uğrar ve geri dönüştürülür.

> [!NOTE]
> Spark veritabanları Spark havuzlarından bağımsız olarak oluşturulur. Çalışma alanı her zaman **varsayılan**olarak adlandırılan bir Spark veritabanına sahiptir. Ek Spark veritabanları oluşturabilirsiniz.

## <a name="the-sql-on-demand-pool"></a>SQL isteğe bağlı havuzu

Her çalışma alanı, **SQL isteğe**bağlı olarak adlandırılan önceden oluşturulmuş bir havuz ile gelir. Bu havuz silinemiyor. SQL isteğe bağlı havuzu, Azure SYNAPSE 'de bir SQL havuzu oluşturmak veya yönetmek zorunda kalmadan SQL ile çalışmanıza olanak sağlar.

Diğer havuz türlerinden farklı olarak, isteğe bağlı SQL için faturalandırma, sorguyu yürütmek için kullanılan kaynak sayısını değil, sorguyu çalıştırmak için taranan veri miktarına bağlıdır.

* İsteğe bağlı SQL, tüm SQL isteğe bağlı havuzlarından bağımsız olarak bulunan kendi SQL isteğe bağlı veritabanlarına sahiptir.
* Bir çalışma alanının her zaman **isteğe bağlı SQL**ADLı bir SQL isteğe bağlı havuzu vardır.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>NYC TAXI örnek verilerini SQLDB1 veritabanına yükleme

1. SYNAPSE Studio 'da, en üstteki mavi menüsünde **?** seçeneğini belirleyin. simge.
1. Başlarken **Getting started**  >  **hub 'ını**seçin.
1. **Sorgu örnek verisi**etiketli kartta, **SQLDB1**adlı SQL havuzunu seçin.
1. **Sorgu verileri**' ni seçin. "Örnek verileri yükleme" bildirimi kısaca görüntülenir. SYNAPSE Studio 'nun en üstünde yer alan açık mavi bir durum çubuğu, verilerin SQLDB1 'e yüklendiğini gösterir.
1. Durum çubuğu yeşile dönüşdikten sonra kapatın.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>SQL havuzundaki NYC TAXI verilerini keşfet

1. SYNAPSE Studio 'da **veri** merkezine gidin.
1. **SQLDB1**  >  **Tables**bölümüne gidin. Birkaç tablo yüklendiğini görürsünüz.
1. Dbo öğesine sağ tıklayın **. Seyahat** tablosu ve **Yeni SQL betiği**Seç  >  **ilk 100 satır seçin**.
1. Yeni bir SQL betiği oluşturulup çalışırken bekleyin.
1. SQL **komut dosyasının en üstünde,** **SQLDB1**adlı SQL havuzuna otomatik olarak ayarlandığını unutmayın.
1. SQL komut dosyasının metnini bu kodla değiştirin ve çalıştırın.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    Bu sorgu, toplam seyahat mesafeleri ve ortalama seyahat mesafesinin, pascların sayısıyla ilişkisini gösterir.
1. SQL komut dosyası sonucu penceresinde, sonuçların bir çizgi grafik olarak görselliğini görmek için **görünümü** **grafik** olarak değiştirin.

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>NYC TAXI verilerini Spark nyctaxi veritabanına yükleme

**SQLDB1**' deki bir tabloda veri mevcuttur. **Nyctaxi**adlı bir Spark veritabanına yükleyin.

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin.
1. **+**  >  **Not defteri**seçin.
1. Not defterinin en üstünde, **Attach** değerini **Spark1**olarak ayarlayın.
1. Bir not defteri kod hücresi eklemek için **Kod Ekle** ' yi seçin ve ardından şu metni yapıştırın:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. **Veri** merkezine gidin, **veritabanları**' na sağ tıklayın ve ardından **Yenile**' yi seçin. Şu veritabanlarını görmeniz gerekir:

    - **SQLDB1** (SQL havuzu)
    - **nyctaxi** (Spark)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

1. Not defterinize geri dönün.
1. Yeni bir kod hücresi oluşturun ve aşağıdaki metni girin. Ardından, **nyctaxi** Spark veritabanına YÜKLEDIĞIMIZ NYC TAXI verilerini göstermek için hücreyi çalıştırın.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Daha önce SQL Pool **SQLDB1**ile yaptığımız analizi gerçekleştirmek için aşağıdaki kodu çalıştırın. Bu kod, çözümlemenin sonuçlarını **nyctaxi. passengercountstats** adlı bir tabloya kaydeder ve sonuçları görselleştirir.

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

1. Hücre sonuçlarında, görselleştirilen verileri görmek için **grafik** ' i seçin.

## <a name="customize-data-visualization-with-spark-and-notebooks"></a>Spark ve Not defterleri ile veri görselleştirmesini özelleştirme

Grafiklerin not defterlerini kullanarak nasıl işleneceğini kontrol edebilirsiniz. Aşağıdaki kod basit bir örnek gösterir. **Matplotlib** ve **Seaborn**popüler kitaplıklarını kullanır. Kod, daha önce çalıştırdığımız SQL sorgularıyla aynı tür çizgi grafiğini işler.

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

Daha önce, **SQLDB1. dbo. SEYAHATSQL** havuz tablosundan verileri Spark tablosu **nyctaxi. seyahat**içine kopyaladık. Daha sonra Spark kullanarak, verileri Spark tablosuna **nyctaxi. passengercountstats**olarak toplandık. Artık **nyctaxi. passengercountstats** Içindeki verileri **SQLDB1. dbo. passengercountstats**adlı bir SQL havuzu tablosuna kopyalayacağız.

Not defterinizde aşağıdaki hücreyi çalıştırın. Toplanan Spark tablosunu SQL havuz tablosuna geri kopyalar.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>İstek üzerine SQL kullanarak Spark veritabanlarında NYC TAXI verilerini çözümleyin

Spark veritabanlarındaki tablolar otomatik olarak görünür ve isteğe bağlı olarak SQL tarafından sorgulanırlar.

1. SYNAPSE Studio 'da, **geliştirme** merkezine gidin ve yenı bir SQL betiği oluşturun.
1. **İsteğe bağlı SQL**'e **bağlanma** ayarı yapın.
1. Aşağıdaki metni betiğe yapıştırın ve betiği çalıştırın.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > İsteğe bağlı SQL kullanan bir sorguyu ilk kez çalıştırdığınızda, SQL istek üzerine sorguları çalıştırmak için gereken SQL kaynaklarını toplamak için yaklaşık 10 saniye sürer. Sonraki sorgular çok daha hızlı olacaktır.
  
## <a name="orchestrate-activities-with-pipelines"></a>İşlem hatları ile etkinlikleri düzenleme

Azure SYNAPSE 'te çok çeşitli görevleri düzenleyebilirsiniz.

1. SYNAPSE Studio **'da, düzenleme merkezine gidin** .
1. **+**  >  Yeni bir işlem hattı oluşturmak için işlem **hattı** ' nı seçin.
1. **Geliştirme** merkezine gidin ve daha önce oluşturduğunuz Not defterini bulun.
1. Bu Not defterini ardışık düzene sürükleyin.
1. İşlem hattında, yeni **tetikleyici Ekle**  >  **/Düzenle**' yi seçin.
1. **Tetikleyiciyi seçin**' de, **Yeni**' yi seçin ve ardından **yineleme** ' de tetikleyiciyi her 1 saatte bir çalışacak şekilde ayarlayın.
1. **Tamam**’ı seçin.
1. **Tümünü Yayımla**. İşlem hattı her saat çalışır.
1. İşlem hattını bir sonraki saatte bir süre beklemeden çalıştırmak için, yeni **tetikleyici Ekle**  >  **/Düzenle**' yi seçin.

## <a name="work-with-data-in-a-storage-account"></a>Bir depolama hesabındaki verilerle çalışma

Şimdiye kadar, çalışma alanındaki veritabanlarında verilerin bulunduğu senaryolar kapsandık. Artık depolama hesaplarında dosyalarla nasıl çalışacağız gösterilmektedir. Bu senaryoda, çalışma alanının ve çalışma alanını oluştururken belirttiğimiz kapsayıcının birincil depolama hesabını kullanacağız.

* Depolama hesabının adı: **contosolake**
* Depolama hesabındaki kapsayıcının adı: **Kullanıcılar**

### <a name="create-csv-and-parquet-files-in-your-storage-account"></a>Depolama hesabınızda CSV ve Parquet dosyaları oluşturma

Aşağıdaki kodu bir not defterinde çalıştırın. Depolama hesabında bir CSV dosyası ve bir Parquet dosyası oluşturur.

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyze-data-in-a-storage-account"></a>Depolama hesabındaki verileri analiz etme

1. SYNAPSE Studio 'da **veri** merkezine gidin ve **bağlı**' yı seçin.
1. **Depolama hesapları**  >  **MyWorkspace (birincil-contosolake)** sayfasına gidin.
1. **Kullanıcıları (birincil)** seçin. **NYCTaxi** klasörünü görmeniz gerekir. İçinde **PassengerCountStats.csv** ve **Passengercountstats. Parquet**adlı iki klasör görmeniz gerekir.
1. **Passengercountstats. Parquet** klasörünü açın. İçinde, *bölüm-00000-2638e00c-0790-496b-a523-578da9a15019-C000. Snappy. Parquet*gibi bir ada sahip bir Parquet dosyası görürsünüz.
1. **. Parquet**öğesine sağ tıklayın ve ardından **Yeni Not defteri**' ni seçin. Şöyle bir hücre içeren bir not defteri oluşturur:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Hücreyi çalıştırın.
1. İçindeki Parquet dosyasına sağ tıklayın ve ardından **Yeni SQL betiği**  >  **en üstteki 100 satırları**Seç ' i seçin. Şöyle bir SQL betiği oluşturur:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```

     Betikte, **Attach** alanı **isteğe bağlı SQL**olarak ayarlanır.

1. Betiği çalıştırın.

## <a name="visualize-data-with-power-bi"></a>Power BI ile verileri görselleştirme

NYC TAXI verilerinden, toplanan veri kümelerini iki tabloda oluşturduk:
- **nyctaxi. passengercountstats**
- **SQLDB1. dbo. PassengerCountStats**

Power BI çalışma alanını Azure SYNAPSE çalışma alanınıza bağlayabilirsiniz. Bu sayede Power BI çalışma alanınıza kolayca veri alabilirsiniz. Power BI raporlarınızı doğrudan Azure SYNAPSE çalışma alanınızda düzenleyebilirsiniz.

### <a name="create-a-power-bi-workspace"></a>Power BI çalışma alanı oluşturma

1. [Powerbi.Microsoft.com](https://powerbi.microsoft.com/)'de oturum açın.
1. **NYCTaxiWorkspace1**adlı yeni bir Power BI çalışma alanı oluşturun.

### <a name="link-your-azure-synapse-workspace-to-your-new-power-bi-workspace"></a>Azure SYNAPSE çalışma alanınızı yeni Power BI çalışma alanınıza bağlama

1. SYNAPSE Studio 'da **Manage**  >  **bağlı hizmetleri**Yönet ' e gidin.
1. Power BI **Yeni**  >  **Bağlan**' ı seçin ve ardından bu alanları ayarlayın:

    |Ayar | Önerilen değer | 
    |---|---|
    |**Adı**|**NYCTaxiWorkspace1**|
    |**Çalışma alanı adı**|**NYCTaxiWorkspace1**|

1. **Oluştur**'u seçin.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-azure-synapse-workspace"></a>Azure SYNAPSE çalışma alanınızdaki verileri kullanan bir Power BI veri kümesi oluşturma

1. SYNAPSE Studio 'da Power BI **geliştirme**bölümüne gidin  >  **Power BI**.
1. **NYCTaxiWorkspace1**  >  **Power BI veri kümelerine** gidin ve **Yeni Power BI veri kümesi ' ni**seçin.
1. **SQLDB1** veritabanının üzerine gelin ve **. pbıd dosyasını indir**' i seçin.
1. İndirilen **. pbıds** dosyasını açın. Power BI Masaüstü açılır ve Azure SYNAPSE çalışma alanınızda **SQLDB1** 'e otomatik olarak bağlanır.
1. **SQL Server veritabanı**adında bir iletişim kutusu görünürse:
    1. **Microsoft hesabı**seçin.
    1. **Oturum aç** ' ı seçin ve hesabınızda oturum açın.
    1. **Bağlan**'ı seçin.
1. **Gezgin** iletişim kutusu açıldıktan sonra, **Passengercountstats** tablosunu denetleyip **Yükle**' yi seçin.
1. **Bağlantı ayarları** iletişim kutusu göründükten sonra **DirectQuery**  >  **Tamam**' ı seçin.
1. Sol taraftaki **rapor** düğmesini seçin.
1. Raporunuza **çizgi grafik** ekleyin.
    1. **Passengercount** sütununu **görsel öğeler**  >  **eksenine**sürükleyin.
    1. **Sumüçlü uzaklığı** ve **avgüçlü uzaklığı** sütunlarını **görselleştirmeler**  >  **değerlerine**sürükleyin.
1. **Giriş** sekmesinde **Yayımla**' yı seçin.
1. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.
1. **Passengeranalysis. pbix**dosya adını seçin ve ardından **Kaydet**' i seçin.
1. **Bir hedef seçin**bölümünde **NYCTaxiWorkspace1**öğesini seçin ve ardından **Seç**' e tıklayın.
1. Yayımlamanın bitmesini bekleyin.

### <a name="configure-authentication-for-your-dataset"></a>Veri kümeniz için kimlik doğrulamasını yapılandırma

1. [Powerbi.Microsoft.com](https://powerbi.microsoft.com/) açın ve **oturum açın**.
1. Sol tarafta, **çalışma alanları**altında **NYCTaxiWorkspace1** çalışma alanını seçin.
1. Bu çalışma alanının içinde, **Passenger Analizi** adlı bir veri kümesini ve **Passenger Analizi**adlı bir raporu bulun.
1. **Passengeranalysis** veri kümesinin üzerine gelin, üç nokta (...) düğmesini seçin ve ardından **Ayarlar**' ı seçin.
1. **Veri kaynağı kimlik bilgileri**' nde, **kimlik doğrulama yöntemini** **OAuth2**olarak ayarlayın ve **oturum aç**' ı seçin.

### <a name="edit-a-report-in-synapse-studio"></a>SYNAPSE Studio 'da rapor düzenleme

1. SYNAPSE Studio 'ya dönün ve **Kapat ve Yenile '** yi seçin.
1. **Geliştirme** merkezine gidin.
1. **Power BI** üzerine gelin ve **Power BI Reports** düğümünü Yenile düğümünü seçin.
1. **Power BI** altında şunları görmeniz gerekir:
    1. **NYCTaxiWorkspace1**  >  **Power BI veri kümeleri**altında, **passengeranalysis**adlı yeni bir veri kümesi.
    1. **NYCTaxiWorkspace1**  >  **Power BI raporlarında**, **passengeranalysis**adlı yeni bir rapor.
1. **Passengeranalysis** raporunu seçin. Rapor açılır ve doğrudan SYNAPSE Studio içinde düzenleyebilirsiniz.

## <a name="monitor-activities"></a>Etkinlikleri izleme

1. SYNAPSE Studio 'da **izleyici** hub 'ına gidin.
1. Bu konumda, çalışma alanında yer alan tüm etkinliklerin geçmişini görebilir ve bunların şimdi etkin olduğunu görebilirsiniz.
1. Çalışma alanında daha önce ne yaptığını görmek için işlem **hattı çalıştırmalarını**, **Apache Spark uygulamaları**ve **SQL isteklerini** inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE Analytics (çalışma alanları Önizleme)](overview-what-is.md)hakkında daha fazla bilgi edinin.

