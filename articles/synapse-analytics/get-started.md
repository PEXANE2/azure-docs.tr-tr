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
ms.openlocfilehash: 24a34ae6f00eca7154021162184f5e71503da06b
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248337"
---
# <a name="getting-started-with-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'i kullanmaya başlama

Bu belge, Azure SYNAPSE Analytics 'i kurmak ve kullanmak için gereken tüm temel adımlarda size rehberlik eder.

## <a name="prepare-a-storage-account-for-use-with-a-synapse-workspace"></a>Bir depolama hesabını bir Synapse çalışma alanıyla kullanılmak üzere hazırlama

* [Azure Portal](https://portal.azure.com) açın
* Aşağıdaki ayarlarla yeni bir depolama hesabı oluşturun:

    |Tab|Ayar | Önerilen değer | Açıklama |
    |---|---|---|---|
    |Temel bilgiler|**Depolama hesabı adı**| Herhangi bir ad verebilirsiniz.|Bu belgede, bu belgeye olarak başvuracağız `contosolake` .|
    |Temel bilgiler|**Hesap türü**|Şu şekilde ayarlanmalıdır`StorageV2`||
    |Temel bilgiler|**Konum**|Herhangi bir konum seçebilirsiniz| SYNAPSE çalışma alanınızın ve Azure Data Lake Storage (ADLS) Gen2 hesabının aynı bölgede olması önerilir.|
    |Gelişmiş|**Data Lake Storage 2. Nesil**|`Enabled`| Azure SYNAPSE, yalnızca bu ayarın etkinleştirildiği depolama hesaplarıyla birlikte kullanılabilir.|

1. Depolama hesabı oluşturulduktan sonra sol gezinmede **erişim denetimi (IAM)** seçeneğini belirleyin. Ardından, aşağıdaki rolleri atayın veya zaten atanmış olduklarından emin olun. 

    a. * Depolama hesabı b 'deki **sahip** rolüne kendiniz atayın. * Depolama hesabındaki **Depolama Blobu veri sahibi** rolüne kendinizi atayın

1. Sol gezinti bölmesinde **kapsayıcılar** ' ı seçin ve bir kapsayıcı oluşturun. Herhangi bir ad verebilirsiniz. Varsayılan **genel erişim düzeyini**kabul edin. Bu belgede, kapsayıcıyı çağıracağız `users` . **Oluştur**’u seçin. 

Aşağıdaki adımda, SYNAPSE çalışma alanınızı bu depolama hesabını "birincil" depolama hesabı ve çalışma alanı verilerini depolayacak kapsayıcı olarak kullanacak şekilde yapılandıracaksınız. Çalışma alanı, verileri bu hesapta Apache Spark tablolarında ve Spark uygulama günlüklerinde adlı bir klasörde depolar `/synapse/workspacename` .

## <a name="create-a-synapse-workspace"></a>SYNAPSE çalışma alanı oluşturma

* [Azure Portal](https://portal.azure.com) açın ve için en üstteki arama ' yı açın `Synapse` .
* Arama sonuçlarında **Hizmetler**altında **Azure SYNAPSE Analytics (çalışma alanları Önizleme)** seçeneğini belirleyin.
* Bu ayarları kullanarak çalışma alanı oluşturmak için **+ Ekle** ' yi seçin

    |Tab|Ayar | Önerilen değer | Açıklama |
    |---|---|---|---|
    |Temel bilgiler|**Çalışma alanı adı**|Her şeyi çağırabilirsiniz.| Bu belgede, şunu kullanacağız`myworkspace`|
    |Temel bilgiler|**Bölge**|Depolama hesabının bölgesiyle Eşleştir|

1. **Data Lake Storage Gen 2**' yi seçin altında, daha önce oluşturduğunuz hesabı ve kapsayıcıyı seçin.

1. **İncele ve oluştur**’u seçin. **Oluştur**’u seçin. Çalışma alanınız birkaç dakika içinde hazırlanacaktır.

## <a name="verify-the-synapse-workspace-msi-has-access-to-the-storage-account"></a>SYNAPSE çalışma alanı MSI ' nin depolama hesabına erişimi olduğunu doğrulayın

Bu, sizin için zaten yapılmış olabilir. Herhangi bir durumda, doğrulamanız gerekir.

1. [Azure Portal](https://portal.azure.com) açın ve çalışma alanınız için seçilen birincil depolama hesabını açın.
1. Sol gezinmede **erişim denetimi (IAM)** seçeneğini belirleyin. Ardından, aşağıdaki rolleri atayın veya zaten atanmış olduklarından emin olun. 
    a. Depolama hesabındaki **Depolama Blobu veri katılımcısı** rolüne çalışma alanı kimliğini atayın. Çalışma alanı kimliği, çalışma alanıyla aynı ada sahip. Bu belgede, çalışma alanı `myworkspace` kimliği`myworkspaced`
1. **Kaydet**’i seçin.
    
## <a name="launch-synapse-studio"></a>Synapse Studio'yu başlatma

SYNAPSE çalışma alanınız oluşturulduktan sonra, SYNAPSE Studio 'Yu açmak için iki yol vardır:
* [Azure Portal](https://portal.azure.com) SYNAPSE çalışma alanınızı açın ve **genel bakış** bölümünün üst kısmında **SYNAPSE Studio 'yu Başlat** ' ı seçin.
* Doğrudan çalışma alanınıza gidin https://web.azuresynapse.net ve oturum açın.

## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki gezinmede **> SQL havuzlarını Yönet** ' i seçin.
1. **+ Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|
    |**SQL havuzu adı**| `SQLDB1`|
    |**Performans düzeyi**|`DW100C`|

1. **Gözden geçir + oluştur** ' u ve ardından **Oluştur**' u seçin.
1. SQL havuzunuz birkaç dakika içinde hazırlanacaktır. SQL havuzunuz oluşturulduğunda, **SQLDB1**ADLı bir SQL havuzu veritabanı ile ilişkilendirilir.

Bir SQL havuzu, etkin olduğu sürece faturalanabilir kaynakları tüketir. Daha sonra maliyetleri azaltmak için havuzu duraklatabilirsiniz.

## <a name="create-an-apache-spark-pool"></a>Apache Spark havuzu oluşturma

1. SYNAPSE Studio 'da, sol taraftaki **> yönet Apache Spark havuzlar** ' ı seçin.
1. **+ Yeni** ' yi seçin ve şu ayarları girin:

    |Ayar | Önerilen değer | 
    |---|---|
    |**Apache Spark havuzu adı**|`Spark1`
    |**Düğüm boyutu**| `Small`|
    |**Düğüm sayısı**| En az 3 ve en fazla 3 olarak ayarlayın|

1. **Gözden geçir + oluştur** ' u ve ardından **Oluştur**' u seçin.
1. Apache Spark havuzunuz birkaç saniye içinde hazırlanacaktır.

> [!NOTE]
> Ada karşın, bir Apache Spark havuzu SQL havuzu gibi değildir. SYNAPSE çalışma alanını Spark ile nasıl etkileşim kuracağınızı bilgilendirmek için kullandığınız bir temel meta veriler vardır. 

Meta veriler olduklarından Spark havuzları başlatılamaz veya durdurulamaz. 

SYNAPSE içinde herhangi bir Spark etkinliği gerçekleştirdiğinizde, kullanmak için bir Spark havuzu belirtirsiniz. Havuz, kaç Spark kaynağı kullanılacağını SYNAPSE bildirir. Yalnızca kullanılan kaynaklar için ödeme yaparsınız. Havuzu kullanmayı etkin bir şekilde durdurduğunuzda, kaynaklar otomatik olarak zaman aşımına uğrar ve geri dönüştürülür.

> [!NOTE]
> Spark veritabanları Spark havuzlarından bağımsız olarak oluşturulur. Çalışma alanı her zaman **varsayılan** adlı bir Spark veritabanına sahiptir ve ek Spark veritabanları oluşturabilirsiniz.

## <a name="the-sql-on-demand-pool"></a>SQL isteğe bağlı havuzu

Her çalışma alanı, **SQL isteğe bağlı**olarak adlandırılmış ve önceden oluşturulmuş ve silinmek üzere olmayan bir havuz ile gelir. SQL isteğe bağlı havuzu, SYNAPSE bir SQL havuzu oluşturmak veya yönetmek zorunda kalmadan SQL ile çalışmanıza olanak sağlar. Diğer havuzların aksine, isteğe bağlı SQL için faturalama, sorguyu yürütmek için kullanılan kaynak sayısını değil, sorguyu çalıştırmak için taranan veri miktarına bağlıdır.

* İsteğe bağlı SQL, tüm SQL isteğe bağlı havuzlarından bağımsız olarak bulunan kendi SQL isteğe bağlı veritabanlarına da sahiptir.
* Şu anda bir çalışma alanı her zaman **isteğe bağlı SQL**ADLı bir SQL isteğe bağlı havuzu içerir.

## <a name="load-the-nyc-taxi-sample-data-into-the-sqldb1-database"></a>NYC TAXI örnek verilerini SQLDB1 veritabanına yükleme

1. SYNAPSE Studio 'da, en üstteki mavi menüsünde **?** seçeneğini belirleyin. simge.
1. Kullanmaya başlama **> başlangıç hub 'ını** seçin
1. **Sorgu örnek verileri**etiketli kartta, adlı SQL havuzunu seçin`SQLDB1`
1. **Sorgu verileri**' ni seçin. "Örnek verileri yükleme" diyerek görüntülenen ve sonra kaybolan bir bildirim görürsünüz.
1. SYNAPSE Studio 'nun en üstünde, verilerin SQLDB1 'e yüklendiğini belirten açık mavi bir bildirim çubuğu göreceksiniz. Yeşil dönüşene kadar bekleyin ve ardından kapatın.

## <a name="explore-the-nyc-taxi-data-in-the-sql-pool"></a>SQL havuzundaki NYC TAXI verilerini keşfet

1. SYNAPSE Studio 'da **veri** merkezine gidin
1. **SQLDB1 > tablolarına**gidin. Birkaç tablo yüklendiğini görürsünüz.
1. Dbo öğesine sağ tıklayın **. Seyahat** tablosu ve **Yeni SQL betiği Seç > Ilk 100 satır seçin**
1. Yeni bir SQL betiği oluşturulup otomatik olarak çalıştırılacak.
1. SQL komut dosyasının en üst **kısmında, ADLı** SQL havuzuna otomatik olarak ayarlandığına dikkat edin `SQLDB1` .
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

1. Bu sorgu, toplam seyahat mesafeleri ve ortalama seyahat mesafesinin, pascların sayısıyla ilişkisini gösterir
1. SQL komut dosyası sonucu penceresinde, sonuçların bir çizgi grafik olarak görselliğini görmek için **görünümü** **grafik** olarak değiştirin

## <a name="load-the-nyc-taxi-sample-data-into-the-spark-nyctaxi-database"></a>NYC TAXI örnek verilerini Spark nyctaxi veritabanına yükleme

İçindeki bir tabloda verileriniz var `SQLDB1` . Şimdi bunu adlı bir Spark veritabanına yükledik `nyctaxi` .

1. SYNAPSE Studio 'da **geliştirme** merkezine gidin
1. Seçin **+** ve **Not defteri** seçin
1. Not defterinin en üstünde, **Ekle** değerini olarak ayarlayın`Spark1`
1. **Kod Ekle** ' yi seçerek bir not defteri kod hücresi ekleyin ve aşağıdaki metni yapıştırın:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. **Veri** merkezine gidin, **veritabanlarına** sağ tıklayın ve **Yenile**' yi seçin.
1. Şimdi şu veritabanlarını görmeniz gerekir:
    - SQLDB (SQL havuzu)
    - nyctaxi (Spark)
      
## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Spark ve not defterlerini kullanarak NYC TAXI verilerini çözümleme

1. Not defterinize geri dön
1. Yeni bir kod hücresi oluşturun, aşağıdaki metni girin ve Spark veritabanına yüklediğimiz NYC TAXI verilerini örnek olarak çalıştırın `nyctaxi` .

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Daha önce SQL havuzuyla yaptığımız analizi gerçekleştirmek için aşağıdaki kodu çalıştırın `SQLDB1` . Bu kod ayrıca analiz sonuçlarını adlı bir tabloya kaydeder `nyctaxi.passengercountstats` ve sonuçları görselleştirir.

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

1. Hücre sonuçlarında, görselleştirilen verileri görmek için **grafik** ' i seçin
 
## <a name="customize-data-visualization-data-with-spark-and-notebooks"></a>Spark ve Not defterleri ile veri görselleştirme verilerini özelleştirme

Not defterleri ile, işleme grafiklerinin nasıl çalıştığını kontrol edebilirsiniz. Aşağıdaki kod, popüler kitaplıkları ve kullanarak basit bir örnek gösterir `matplotlib` `seaborn` . SQL sorgularını daha önce çalıştırırken gördüğünüz çizgi grafik türünü de işleyebilir.

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

Daha önce bir SQL havuzu tablosundan Spark tablosuna veri kopyaladık `SQLDB1.dbo.Trip` `nyctaxi.trip` . Daha sonra Spark kullanarak verileri Spark tablosuna toplandık `nyctaxi.passengercountstats` . Şimdi, verileri `nyctaxi.passengercountstats` adlı BIR SQL havuzu tablosuna kopyalayacağız `SQLDB1.dbo.PassengerCountStats` . 

Aşağıdaki hücreyi Not defterinizde çalıştırın. Toplanan Spark tablosunu SQL havuz tablosuna geri kopyalayacaktır.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>İstek üzerine SQL kullanarak Spark veritabanlarında NYC TAXI verilerini çözümleyin 

Spark veritabanlarındaki tablolar otomatik olarak görünür ve isteğe bağlı SQL tarafından sorgulanabilir.

1. SYNAPSE Studio 'da, **geliştirme** merkezine gidin ve yenı bir SQL betiği oluşturun
1. **İsteğe bağlı SQL** 'e **bağlanma** ayarla 
1. Aşağıdaki metni betiğe yapıştırın ve betiği çalıştırın.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```
    > [!NOTE]
    > İsteğe bağlı SQL kullanan bir sorguyu ilk kez çalıştırdığınızda, sorguları çalıştırmak için gereken SQL kaynaklarını toplamak üzere SQL isteğe bağlı SQL için yaklaşık 10 saniye sürer. Sonraki sorgular bu süreyi gerektirmez ve çok daha hızlı olur.
  
## <a name="orchestrate-activities-with-pipelines"></a>İşlem hatları ile etkinlikleri düzenleme

Azure SYNAPSE 'te çok çeşitli görevleri düzenleyebilirsiniz. Bu bölümde, ne kadar kolay olduğunu göreceksiniz.

1. SYNAPSE Studio 'da, **orchestrate** hub 'ına gidin.
1. **+** Sonra Işlem **hattı**' nı seçin. Yeni işlem hattı oluşturulur.
1. Geliştirme merkezine gidin ve daha önce oluşturduğunuz Not defterini bulun.
1. Bu Not defterini ardışık düzene sürükleyin.
1. İşlem hattında **tetikleyici ekle > yeni/Düzenle**' yi seçin.
1. **Tetikleyici Seç** ' de **Yeni**' yi seçin ve ardından yineleme ' de tetikleyiciyi her 1 saatte bir çalışacak şekilde ayarlayın.
1. **Tamam**’ı seçin.
1. **Tümünü Yayımla** ' yı seçtiğinizde işlem hattı her saat çalışacaktır.
1. İşlem hattını bir sonraki saati beklemeden hemen çalıştırmak istiyorsanız, **Yeni/düzenle > tetikleyici Ekle**' yi seçin.

## <a name="working-with-data-in-a-storage-account"></a>Depolama hesabındaki verilerle çalışma

Şimdiye kadar, senaryolar çalışma alanındaki veritabanlarında yer verdik. Artık depolama hesaplarındaki dosyalarla nasıl çalışacağız. Bu senaryoda, çalışma alanını oluştururken belirttiğimiz çalışma alanının ve kapsayıcının birincil depolama hesabını kullanacağız.

* Depolama hesabının adı:`contosolake`
* Depolama hesabındaki kapsayıcının adı:`users`

### <a name="creating-csv-and-parquet-files-in-your-storage-account"></a>Depolama hesabınızda CSV ve Parquet dosyaları oluşturma

Aşağıdaki kodu bir not defterinde çalıştırın. Depolama hesabında bir CSV dosyası ve bir Parquet dosyası oluşturur

```py
%%pyspark
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.repartition(1) # This ensure we'll get a single file during write()
df.write.mode("overwrite").csv("/NYCTaxi/PassengerCountStats.csv")
df.write.mode("overwrite").parquet("/NYCTaxi/PassengerCountStats.parquet")
```

### <a name="analyzing-data-in-a-storage-account"></a>Depolama hesabındaki verileri analiz etme

1. SYNAPSE Studio 'da **veri** merkezine gidin
1. **Bağlı** seçin
1. **Çalışma alanım > depolama hesaplarına gidin (birincil-contosolake)**
1. **Kullanıcıları Seç (birincil) "**
1. Adlı bir klasör görmeniz gerekir `NYCTaxi` . İçinde iki klasör görmeniz gerekir `PassengerCountStats.csv` `PassengerCountStats.parquet` .
1. `PassengerCountStats.parquet`Klasörüne gidin.
1. İçindeki Parquet dosyasına sağ tıklayın ve **Yeni Not defteri**' ni seçin, şöyle bir hücre içeren bir not defteri oluşturur:

    ```py
    %%pyspark
    data_path = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet', format='parquet')
    data_path.show(100)
    ```

1. Hücreyi çalıştırın.
1. İçindeki Parquet dosyasına sağ tıklayın ve **yenı SQL betiği ' ni seçin > en üstteki 100 satırları seçin**, şöyle bir SQL betiği oluşturur:

    ```sql
    SELECT TOP 100 *
    FROM OPENROWSET(
        BULK 'https://contosolake.dfs.core.windows.net/users/NYCTaxi/PassengerCountStats.parquet/part-00000-1f251a58-d8ac-4972-9215-8d528d490690-c000.snappy.parquet',
        FORMAT='PARQUET'
    ) AS [r];
    ```
    
1. Betikte, **Attach** alanı **isteğe bağlı SQL**olarak ayarlanır.
1. Betiği çalıştırın.

## <a name="visualize-data-with-power-bi"></a>Power BI ile verileri görselleştirme

NYX TAXI verilerinden, iki tabloda toplanmış veri kümeleri oluşturduk:
* `nyctaxi.passengercountstats`
* `SQLDB1.dbo.PassengerCountStats`

Bir Power BI çalışma alanını SYNAPSE çalışma alanınıza bağlayabilirsiniz. Bu, Power BI çalışma alanınıza kolayca veri almanıza olanak sağlar ve Power BI raporlarınızı doğrudan SYNAPSE çalışma alanınızda düzenleyebilirsiniz.

### <a name="create-a-power-bi-workspace"></a>Power BI çalışma alanı oluşturma

1. [Powerbi.Microsoft.com](https://powerbi.microsoft.com/)'de oturum açın.
1. Adlı yeni bir Power BI çalışma alanı oluşturun `NYCTaxiWorkspace1` .

### <a name="link-your-synapse-workspace-to-your-new-power-bi-workspace"></a>SYNAPSE çalışma alanınızı yeni Power BI çalışma alanınıza bağlama

1. SYNAPSE Studio 'da **> bağlı hizmetleri Yönet**' e gidin.
1. **+ Yeni** ' yi seçin ve **Power BI Bağlan** ' ı seçin ve bu alanları ayarlayın:

    |Ayar | Önerilen değer | 
    |---|---|
    |**Adı**|`NYCTaxiWorkspace1`|
    |**Çalışma alanı adı**|`NYCTaxiWorkspace1`|
        
1. **Oluştur**’u seçin.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-synapse-workspace"></a>SYNAPSE çalışma alanınızdaki verileri kullanan bir Power BI veri kümesi oluşturma

1. SYNAPSE Studio 'da **geliştirme > Power BI**gidin.
1. **NYCTaxiWorkspace1 > Power BI veri kümelerine** gidin ve **Yeni Power BI veri kümesi ' ni**seçin.
1. Veritabanının üzerine gelin `SQLDB1` ve **. pbıd dosyasını indir**' i seçin.
1. İndirilen dosyayı açın `.pbids` . 
1. Bu işlem masaüstü Power BI başlatacaktır ve `SQLDB1` SYNAPSE çalışma alanınızda otomatik olarak bağlanır.
1. **SQL Server veritabanı**: a olarak adlandırılan bir iletişim kutusu görürseniz. **Microsoft hesabı**seçin. 
    b. **Oturum aç** ' ı seçin ve oturum açın.
    c. **Bağlan**'ı seçin.
1. **Gezgin** iletişim kutusu açılır. Ne zaman, **Passengercountstats** tablosunu denetleyip **Yükle**' yi seçin.
1. **Bağlantı ayarları** iletişim kutusu görüntülenir. **DirectQuery** 'yi seçin ve **Tamam 'ı** seçin
1. Sol taraftaki **rapor** düğmesini seçin.
1. Raporunuza **çizgi grafik** ekleyin.
    a. **Passsengercount** sütununu b **ekseni > görselleştirmelere** sürükleyin. **Sumüçlü mesafeyi** ve **avgüçlü uzaklığı** sütunlarını, **değerler > görselleştirmelere**sürükleyin.
1. **Giriş** sekmesinde **Yayımla**' yı seçin.
1. Değişikliklerinizi kaydetmek isteyip istemediğinizi sorar. **Kaydet**’i seçin.
1. Bir dosya adı seçmeniz istenir. Seçin `PassengerAnalysis.pbix` ve **Kaydet**' i seçin.
1. **Bir hedef** seçip Seç ' i seçmenizi ister `NYCTaxiWorkspace1` . **Select**
1. Yayımlamanın bitmesini bekleyin.

### <a name="configure-authentication-for-your-dataset"></a>Veri kümeniz için kimlik doğrulamasını yapılandırma

1. [Powerbi.Microsoft.com](https://powerbi.microsoft.com/) açın ve **oturum açın**
1. Sol tarafta, çalışma **alanları** ' nın altında `NYCTaxiWorkspace1` çalışma alanını seçin.
1. Bu çalışma alanının içinde adlı bir veri kümesi `Passenger Analysis` ve adlı bir rapor görmeniz gerekir `Passenger Analysis` .
1. `PassengerAnalysis`Veri kümesinin üzerine gelin ve üç nokta ile simgeyi seçin ve **Ayarlar**' ı seçin.
1. **Veri kaynağı kimlik bilgileri**' nde, **kimlik doğrulama yöntemini** **OAuth2** olarak ayarlayın ve **oturum aç '** ı seçin.

### <a name="edit-a-report-in-synapse-studio"></a>SYNAPSE Studio 'da rapor düzenleme

1. SYNAPSE Studio 'ya geri dönün ve **Kapat ve Yenile '** yi seçin. 
1. **Geliştirme** merkezine gidin 
1. **Power BI** üzerine gelin ve **Power BI Reports** düğümünü Yenile düğümüne tıklayın.
1. Artık **Power BI** altında şunları görmeniz gerekir: a. * **NYCTaxiWorkspace1 >** altında, **Passengeranalysis**adlı yeni bir veri kümesi Power BI veri kümeleri.
    b. * NYCTaxiWorkspace1 > altında, **Passengeranalysis**adlı yeni bir rapor **Power BI raporlar**.
1. **Passengeranalysis** raporunu seçin. 
1. Rapor açılır ve artık raporu doğrudan SYNAPSE Studio içinde düzenleyebilirsiniz.

## <a name="monitor-activities"></a>Etkinlikleri izleme

1. SYNAPSE Studio 'da izleyici hub 'ına gidin.
1. Bu konumda, çalışma alanında yer alan tüm etkinliklerin geçmişini görebilir ve bunların şimdi etkin olduğunu görebilirsiniz.
1. İşlem **hattı çalıştırmalarını**, **Apache Spark uygulamalarını**ve **SQL isteklerini** inceleyin ve çalışma alanında daha önce ne yaptığını görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE Analytics (Önizleme)](overview-what-is.md) hakkında daha fazla bilgi edinin

