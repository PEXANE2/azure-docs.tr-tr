---
title: Hadoop kümesindeki verileri keşfedin - Ekip Veri Bilimi Süreci
description: Bir model oluşturmak ve dağıtmak için bir HDInsight Hadoop kümesi kullanarak, uça bir dizi senaryo için Takım Veri Bilimi İşlemi'ni kullanma.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: bf69786f56f52874bd9358ae44a6b88b466e77f4
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677470"
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Ekip Veri Bilimi Süreci iş başında: Azure HDInsight Hadoop kümelerini kullanın
Bu izlenme de, [takım veri bilimi işlemini (TDSP)](overview.md) uçtan uca bir senaryoda kullanırız. Kamuya açık [NYC Taksi Gezileri](https://www.andresmh.com/nyctaxitrips/) veri setinden verileri depolamak, keşfetmek ve özellik belirleme yapmak ve verileri aşağıdan tatmak için bir Azure [HDInsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/) kullanıyoruz. İkili ve çok sınıflı sınıflandırma ve regresyon tahmin görevlerini işlemek için Azure Machine Learning ile verilerin modellerini oluşturuyoruz. 

Daha büyük bir veri kümesinin nasıl işleyeceğini gösteren bir yol için, [1-TB veri kümesinde Azure HDInsight Hadoop Kümelerini kullanma - Takım Veri Bilimi Süreci'ne](hive-criteo-walkthrough.md)bakın.

Ayrıca, 1-TB veri kümesini kullanan izlenebilirlikte sunulan görevleri gerçekleştirmek için bir IPython dizüstü bilgisayarı da kullanabilirsiniz. Daha fazla bilgi için [Hive ODBC bağlantısını kullanarak Criteo walkthrough'a](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb)bakın.

## <a name="nyc-taxi-trips-dataset-description"></a><a name="dataset"></a>NYC Taksi Gezileri dataset açıklama
NYC Taksi Gezisi verileri sıkıştırılmış virgülayrılmış değerler (CSV) dosyaları (~ 48 GB sıkıştırılmamış) yaklaşık 20 GB olduğunu. Bu 173 milyondan fazla bireysel geziler vardır ve her gezi için ödenen ücretler içerir. Her seyahat kaydı pick-up ve bırakma yeri ve saati, anonim hack (sürücü) lisans numarası ve madalyon numarası (taksinin benzersiz kimliği) içerir. Veriler 2013 yılındaki tüm gezileri kapsar ve her ay için aşağıdaki iki veri kümesinde sağlanır:

- trip_data CSV dosyaları seyahat bilgilerini içerir: yolcu sayısı, kalkış noktaları, seyahat süresi ve seyahat uzunluğu. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
- trip_fare CSV dosyaları, her seyahat için ödenen ücretin ayrıntılarını içerir: ödeme türü, ücret tutarı, ek ücret ve vergiler, ipuçları ve geçiş ücretleri ve ödenen toplam tutar. Aşağıda birkaç örnek kayıt veörnekleri verebleri verebleri bulabilirsiniz:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat\_verilerine ve seyahat\_ücretine katılmak için benzersiz anahtar\_alanlardan oluşur: madalyon, hack lisansı ve teslim alma\_tarihi. Belirli bir seyahatle ilgili tüm ayrıntıları almak için bu üç tuşa katılmak yeterlidir.

## <a name="examples-of-prediction-tasks"></a><a name="mltasks"></a>Tahmin görevleriörnekleri
Gerekli işlem görevlerini netleştirmeye yardımcı olmak için veri çözümlemesi temel alınabilmek için yapmak istediğiniz tahmin türünü belirleyin. Bu izbarada ele aldığımız tahmin sorunlarına üç örnek *verilmiştir:\_*

- **İkili sınıflandırma**: Bir seyahat için bahşiş ödenip ödenmediğini tahmin edin. Diğer bir sonuç *\_tutarı* 0 TL'den büyük bir örnek olumlu bir örnek, 0 TL'lik bir *ipucu\_tutarı* ise olumsuz bir örnektir.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
- **Çok sınıflı sınıflandırma**: Yolculuk için ödenen bahşiş tutarlarının aralığını tahmin edin. *\_Bahşiş tutarını* beş sınıfa böleriz:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
- **Regresyon görevi**: Bir yolculuk için ödenen bahşiş miktarını tahmin edin.  

## <a name="set-up-an-hdinsight-hadoop-cluster-for-advanced-analytics"></a><a name="setup"></a>Gelişmiş analitik için bir HDInsight Hadoop kümesi ayarlama
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Bir HDInsight kümesini üç adımda kullanan gelişmiş analitik için bir Azure ortamı ayarlayabilirsiniz:

1. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md): Bu depolama hesabı, Verileri Azure Blob depolama alanında depolamak için kullanılır. HDInsight kümelerinde kullanılan veriler de burada bulunur.
2. [Gelişmiş Analitik Süreci ve Teknolojisi için Azure HDInsight Hadoop kümelerini özelleştirin.](customize-hadoop-cluster.md) Bu adım, tüm düğümlere 64 bit Anaconda Python 2.7 yüklü bir HDInsight Hadoop kümesi oluşturur. HDInsight kümenizi özelleştirirken hatırlanması gereken iki önemli adım vardır.
   
   * 1. adımda oluşturulan depolama hesabını oluştururken HDInsight kümenize bağlamayı unutmayın. Bu depolama hesabı küme içinde işlenen verilere erişir.
   * Kümeyi oluşturduktan sonra, kümenin baş düğümüne Uzaktan Erişim'i etkinleştirin. **Yapılandırma** sekmesine göz atın ve **Uzaktan'yı etkinleştir'i**seçin. Bu adım, uzaktan oturum açma için kullanılan kullanıcı kimlik bilgilerini belirtir.
3. [Azure Machine Learning çalışma alanı oluşturun](../studio/create-workspace.md): Bu çalışma alanını makine öğrenimi modelleri oluşturmak için kullanırsınız. Bu görev, HDInsight kümesini kullanarak ilk veri arama ve aşağı örnekleme tamamlandıktan sonra ele alınmıştır.

## <a name="get-the-data-from-a-public-source"></a><a name="getdata"></a>Verileri genel bir kaynaktan alma
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

[NYC Taksi Gezileri](https://www.andresmh.com/nyctaxitrips/) veri kümesini genel konumundan makinenize kopyalamak için, Azure [Blob depolamasına ve bu depolamadan veri taşı'nda](move-azure-blob.md)açıklanan yöntemlerden herhangi birini kullanın.

Burada, veri içeren dosyaları aktarmak için AzCopy'nin nasıl kullanılacağını açıklıyoruz. AzCopy'i indirmek ve yüklemek için [AzCopy komut satırı yardımcı programı ile başlarken](../../storage/common/storage-use-azcopy.md)talimatları izleyin.

1. Komut istemi penceresinden, * \<path_to_data_folder>* istediğiniz hedefle değiştirerek aşağıdaki AzCopy komutlarını çalıştırın:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Kopya tamamlandığında, seçilen veri klasöründe toplam 24 sıkıştırılmış dosya görürsünüz. İndirilen dosyaları yerel makinenizdeki aynı dizine boşaltın. Sıkıştırılmamış dosyaların bulunduğu klasöre not alın. Bu klasör, aşağıdaki * \<\_dosyalarunzipped_data\_\_\> yol* olarak adlandırılır.

## <a name="upload-the-data-to-the-default-container-of-the-hdinsight-hadoop-cluster"></a><a name="upload"></a>Verileri HDInsight Hadoop kümesinin varsayılan kapsayıcısına yükleyin
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Aşağıdaki AzCopy komutlarında, Hadoop kümesini oluştururken ve veri dosyalarını açarken aşağıdaki parametreleri belirttiğiniz gerçek değerlerle değiştirin.

* *** \<path_to_data_folder>*** Makinenizde, günönce günönce günönce günönce günönce veri dosyalarını içeren dizin (yol ile birlikte) bulunur.  
* ***Hadoop küme nin depolama hesap adı>\<*** HDInsight kümenizle ilişkili depolama hesabı.
* ***Hadoop küme>varsayılan konteyner \<*** Kümeniz tarafından kullanılan varsayılan kapsayıcı. Varsayılan kapsayıcının adı genellikle kümenin kendisiyle aynı addır. Örneğin, küme "abc123.azurehdinsight.net" olarak adlandırılırsa, varsayılan kapsayıcı abc123'dür.
* depolama hesabı anahtarı>*** \<*** Kümeniz tarafından kullanılan depolama hesabının anahtarı.

Komut istemi veya Windows PowerShell penceresinden aşağıdaki iki AzCopy komutunu çalıştırın.

Bu komut, Hadoop kümesinin varsayılan kapsayıcısındaki ***nyctaxitripraw*** dizinine gezi verilerini yükler.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Bu komut, hadoop kümesinin varsayılan kapsayıcısındaki ***nyctaxifareraw*** dizinine ücret verilerini yükler.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Veriler artık Blob depolama alanında olmalı ve HDInsight kümesi içinde tüketilmeye hazır olmalıdır.

## <a name="sign-in-to-the-head-node-of-hadoop-cluster-and-prepare-for-exploratory-data-analysis"></a><a name="#download-hql-files"></a>Hadoop kümesinin baş düğümünde oturum açın ve araştırmacı veri analizine hazırlanın
> [!NOTE]
> Bu genellikle bir yönetici görevidir.
> 
> 

Keşif veri analizi ve verilerin aşağı örneklemesi için kümenin baş düğümüne erişmek için [Hadoop Cluster'ın baş düğümüne eriş'te](customize-hadoop-cluster.md)özetlenen yordamı izleyin.

Bu iznizde, ön veri incelemeleri gerçekleştirmek için öncelikle SQL benzeri bir sorgu dili olan [Hive'da](https://hive.apache.org/)yazılmış sorguları kullanırız. Hive sorguları '.hql' dosyalarında depolanır. Daha sonra, modeller oluşturmak için Machine Learning içinde kullanılacak bu verileri aşağıdan örnekliyoruz.

Kümeyi araştırmacı veri çözümlemesi için hazırlamak için, ilgili Hive komut dosyalarını içeren '.hql' dosyalarını [GitHub'dan](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) baş düğümündeki yerel bir dizine (C:\temp) indirin. Kümenin baş düğümünün içinden komut istemini açın ve aşağıdaki iki komutu çalıştırın:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Bu iki komut, bu walkthrough'da gereken tüm '.hql' dosyalarını baş düğümündeki yerel dizin ***C:\temp&#92;*** indirin.

## <a name="create-hive-database-and-tables-partitioned-by-month"></a><a name="#hive-db-tables"></a>Kovan veritabanı ve tablolar aya göre bölümlenmiş oluşturma
> [!NOTE]
> Bu görev genellikle bir yönetici içindir.
> 
> 

Şimdi NYC taksi veri seti için Hive tablolar oluşturmak için hazırsınız.
Hadoop kümesinin baş düğümünde, baş düğümünün masaüstündeki Hadoop komut satırını açın. Aşağıdaki komutu çalıştırarak Kovan dizinini girin:

    cd %hive_home%\bin

> [!NOTE]
> Bu walkthrough tüm Hive komutları çalıştırın/ dizin istemi. Bu, tüm yol sorunlarını otomatik olarak işler. Bu kılavuzda "Hive directory prompt", "Hive bin/ directory prompt" ve "Hadoop komut satırı" terimlerini birbirinin yerine kullanıyoruz.
> 
> 

Hive dizin isteminden, Hive veritabanı nı ve tablolarını oluşturan baş düğümünün Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Burada **C\_içeriği:\temp\örnek kovan\_oluşturmak\_db\_\_ve tables.hql** dosyası hive veritabanı **nyctaxidb**oluşturur , ve tablolar **gezi** ve **ücret**.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Bu Hive komut dosyası iki tablo oluşturur:

* **Gezi** tablosu her sürüşün seyahat ayrıntılarını (sürücü ayrıntıları, karşılama süresi, yolculuk mesafesi ve süreleri) içerir.
* **Ücret** tablosu ücret ayrıntılarını (ücret tutarı, bahşiş tutarı, geçiş ücretleri ve ek ücretler) içerir.

Bu yordamlarla ilgili ek yardıma ihtiyacınız varsa veya alternatif yordamları araştırmak istiyorsanız, [doğrudan Hadoop komut satırından Kovan gönder sorgularını gönder bölümüne](move-hive-tables.md#submit)bakın.

## <a name="load-data-to-hive-tables-by-partitions"></a><a name="#load-data"></a>Verileri bölümlere göre Kovan tablolarına yükleme
> [!NOTE]
> Bu görev genellikle bir yönetici içindir.
> 
> 

NYC taksi veri seti biz daha hızlı işleme ve sorgu süreleri sağlamak için kullandığınız ay, doğal bir bölümleme vardır. Aşağıdaki PowerShell komutları (Hadoop komut satırı kullanılarak Hive dizininden verilir) ay tarafından bölümlenen gezi ve ücret Hive tablolarına veri yükler.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

**Partitions.hql\_\_dosyasına\_göre örnek\_\_kovan yük verileri** aşağıdaki **LOAD** komutlarını içerir:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Burada arama işleminde kullanılan Hive sorgularının bir kısmı yalnızca bir veya iki bölüme bakmayı içerir. Ancak bu sorgular tüm veri kümesinde çalıştırılabilir.

### <a name="show-databases-in-the-hdinsight-hadoop-cluster"></a><a name="#show-db"></a>HDInsight Hadoop kümesinde veritabanlarını göster
Hadoop komut satırı penceresinde HDInsight Hadoop kümesinde oluşturulan veritabanlarını göstermek için Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show databases;"

### <a name="show-the-hive-tables-in-the-nyctaxidb-database"></a><a name="#show-tables"></a>**Nyctaxidb** veritabanında Hive tablolarını göster
**Nyctaxidb** veritabanındaki tabloları göstermek için Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show tables in nyctaxidb;"

Tabloların aşağıdaki komutu çalıştırarak bölümlendiğini doğrulayabiliriz:

    hive -e "show partitions nyctaxidb.trip;"

İşte beklenen çıktı:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Benzer şekilde, ücret tablosunun aşağıdaki komutu çalıştırarak bölümlendiğinden emin olabiliriz:

    hive -e "show partitions nyctaxidb.fare;"

İşte beklenen çıktı:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="data-exploration-and-feature-engineering-in-hive"></a><a name="#explore-hive"></a>Hive'da veri arama ve özellik mühendisliği
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

Hive tablolarına yüklenen veriler için veri arama ve özellik mühendisliği görevlerini gerçekleştirmek için Kovan sorgularını kullanabilirsiniz. Bu tür görevlere örnekler aşağıda verilmiştir:

* Her iki tablodaki en iyi 10 kaydı görüntüleyin.
* Değişen zaman pencerelerinde birkaç alanın veri dağılımlarını keşfedin.
* Boylam ve enlem alanlarının veri kalitesini araştırın.
* Uç miktarına göre ikili ve çok sınıflı sınıflandırma etiketleri oluşturun.
* Doğrudan yolculuk mesafelerini hesaplayarak özellikler oluşturun.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Keşif: Tablo gezisinde en iyi 10 kaydı görüntüleyin
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

Verilerin neye benzediğini görmek için her tablodan 10 kaydı inceleyin. Kayıtları incelemek için, Hadoop komut satırı konsolundaki Hive dizininden ayrı olarak aşağıdaki iki sorguyu çalıştırın.

İlk aydan itibaren gezi tablosunda ilk 10 kaydı almak için:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

İlk aydan itibaren ücret tablosunda ilk 10 kaydı almak için:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Önceki sorguda küçük bir değişiklikle uygun görüntüleme için kayıtları bir dosyaya kaydedebilirsiniz:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Keşif: 12 bölümün her birinde kayıt sayısını görüntüleyin
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

İlgi çekici, seyahat sayısının takvim yılı içinde nasıl değiştiğidir. Aya göre gruplandırma, gezilerin dağılımını gösterir.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Bu komut aşağıdaki çıktıyı üretir:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Burada, ilk sütun ay, ikinci o ay için geziler sayısıdır.

Ayrıca, Hive dizinindeki aşağıdaki komutu çalıştırarak gezi veri kümemizdeki toplam kayıt sayısını da sayabiliriz:

    hive -e "select count(*) from nyctaxidb.trip;"

Bu komut verir:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Gezi veri kümesinde gösterilenlere benzer komutları kullanarak, kayıt sayısını doğrulamak için ücret veri kümesi için Hive dizininden Hive sorguları verebiliriz.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Bu komut bu çıktıyı üretir:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Her iki veri kümesi için de ayda aynı sayıda yolculuk döndürülür ve verilerin doğru yüklendiğinin ilk doğrulanması sağlanır.

Kovan dizin isteminden aşağıdaki komutu kullanarak ücret veri kümesindeki toplam kayıt sayısını sayabilirsiniz:

    hive -e "select count(*) from nyctaxidb.fare;"

Bu komut verir:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Her iki tablodaki toplam kayıt sayısı da aynıdır ve verilerin doğru yüklendiğini ikinci bir doğrulama sağlar.

### <a name="exploration-trip-distribution-by-medallion"></a>Keşif: Madalyon ile gezi dağılımı
> [!NOTE]
> Bu analiz genellikle bir veri bilim adamı görevidir.
> 
> 

Bu örnek, belirli bir süre içinde 100'den fazla seyahat ile madalyonlar (taksi numaraları) tanımlar. Bölüm değişken **ayı**tarafından koşullandırıldığından, sorgu bölümlenmiş tablo erişiminden yararlanır. Sorgu sonuçları yerel bir dosyaya yazılır, **queryoutput.tsv**, baş `C:\temp` düğümünde.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Burada inceleme için **madalyon.hql\_dosyası tarafından örnek\_\_kovan gezi\_sayısı\_** içeriğidir.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

NYC taksi veri setindema eşsiz bir taksi tanımlar. Belirli bir zaman diliminde hangi taksilerin belirli sayıdan fazla seyahat yaptığını sorarak hangi taksilerin nispeten meşgul olduğunu belirleyebilirsiniz. Aşağıdaki örnek, ilk üç ay içinde yüzden fazla yolculuk yapan taksileri tanımlar ve sorgu sonuçlarını **C:\temp\queryoutput.tsv**adlı yerel bir dosyaya kaydeder.

Burada inceleme için **madalyon.hql\_dosyası tarafından örnek\_\_kovan gezi\_sayısı\_** içeriğidir.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Hive dizin isteminden aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Keşif: Madalyon ve hack lisansı ile Trip dağıtım
> [!NOTE]
> Bu görev genellikle bir veri bilim adamı içindir.
> 
> 

Bir veri kümesini araştırırken, sık sık değer gruplarının dağılımlarını incelemek isteriz. Bu bölümde, taksiler ve sürücüler için bu analizin nasıl yapılacılınağı

**\_Madalyon\_\_license.hql dosyasına göre\_\_örnek kovan gezi\_sayısı** **madalyon** ve **hack_license**ücret veri seti gruplandırın ve her kombinasyonun sayımlarını döndürür. İşte içeriği şunlardır:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Bu sorgu, azalan sayıda yolculukla sipariş edilen taksi ve sürücü kombinasyonlarını döndürür.

Hive dizin istemi, çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Sorgu sonuçları yerel bir dosyaya yazılır, **C:\temp\queryoutput.tsv**.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Keşif: Geçersiz boylam veya enlem kayıtlarını kontrol ederek veri kalitesinin değerlendirilmesi
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

Araştırmacı veri analizinin ortak amacı, geçersiz veya kötü kayıtları niçin oyalamaktır. Bu bölümdeki örnek, boylam veya enlem alanlarının NYC alanının çok dışında bir değer içerip içermediğini belirler. Bu tür kayıtların hatalı boylam-enlem değerine sahip olması muhtemel olduğundan, bunları modelleme için kullanılacak tüm verilerden ortadan kaldırmak istiyoruz.

Burada örnek **\_kovan\_kalite\_değerlendirmesi.hql** dosyasının içeriği denetim için.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Hive dizin istemi, çalıştırın:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Bu komutta yer alan *-S* bağımsız değişkeni, Kovan Haritası/Azalt işlerinin durum ekranı çıktısını bastırır. Hive sorgusu çıktısının ekran baskısını daha okunabilir hale getirir, bu komut yararlıdır.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Keşif: Gezi ipuçlarıikili sınıf dağılımları
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

[Tahmin görevleri örnekleri](hive-walkthrough.md#mltasks) bölümünde özetlenen ikili sınıflandırma sorunu için, bir ipucuverilip verilmediğini bilmek yararlıdır. İpuçlarının bu dağılımı ikilidir:

* verilen bahşiş (Sınıf\_1, uç miktarı > $0)  
* ipucu yok (Sınıf\_0, uç miktarı = $0)

Aşağıdaki **örnek\_kovan\_\_uçlu frekanslar.hql** dosyası çalıştırmak için komutu gösterir:

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Hive dizin istemi, çalıştırın:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Eksplors: Çok sınıflı ayarda sınıf dağılımları
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

[Tahmin görevleri Örnekleri](hive-walkthrough.md#mltasks) bölümünde özetlenen çok sınıflı sınıflandırma sorunu için, bu veri kümesi verilen ipuçlarının miktarını tahmin etmek için doğal bir sınıflandırmaya da kendini ödünç verir. Sorgudaki ipucu aralıklarını tanımlamak için kutuları kullanabiliriz. Çeşitli uç aralıkları için sınıf dağılımlarını almak için **\_örnek\_\_kovan\_ucu aralığı frekansları.hql** dosyasını kullanın. İşte içeriği.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Hadoop komut satırı konsolundan aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>Keşif: İki boylam-enlem konumu arasındaki doğrudan mesafeyi hesaplama
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

İki konum arasındaki doğrudan mesafe ile taksinin gerçek seyahat mesafesi arasında bir fark olup olmadığını bilmek isteyebilirsiniz. Bir yolcu, sürücünün onları kasıtlı olarak daha uzun bir yoldan aldığını öğrenirse bahşiş verme olasılığı daha düşük olabilir.

Gerçek yolculuk mesafesi ile iki boylam enlem noktası ("büyük daire" mesafesi) arasındaki [Haversine mesafesi](https://en.wikipedia.org/wiki/Haversine_formula) arasındaki karşılaştırmayı görmek için, Hive içinde bulunan trigonometrik işlevleri kullanabilirsiniz:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

Bir önceki sorguda, R mil olarak Dünya'nın yarıçapı ve pi radyanlar dönüştürülür. Boylam enlem noktaları, NYC alanından uzak değerleri kaldırmak için filtrelenir.

Bu durumda, sonuçları **queryoutputdir**adlı bir dizine yazarız. Aşağıdaki komutların sırası önce bu çıktı dizini oluşturur ve sonra Kovan komutunu çalıştırır.

Hive dizin istemi, çalıştırın:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Sorgu sonuçları, Hadoop kümesinin varsayılan kapsayıcısı altında dokuz Azure blobs **(queryoutputdir/000000 0\_** to **queryoutputdir/000008\_0)** yazılır.

Tek tek lekelerin boyutunu görmek için, Kovan dizininden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///queryoutputdir

Belirli bir dosyanın içeriğini görmek için, diyelim ki **000000\_0**, Hadoop `copyToLocal` komutunu kullanın.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`büyük dosyalar için çok yavaş olabilir ve onlarla kullanılması tavsiye edilmez.  
> 
> 

Bu verilerin bir Azure blob'unda yer almanın önemli bir avantajı, Alma [Verileri][import-data] modülasyonlarını kullanarak Machine Learning içindeki verileri inceleyebiliyor olmamızdır.

## <a name="down-sample-data-and-build-models-in-machine-learning"></a><a name="#downsample"></a>Machine Learning'de alt örnek verileri ve yapı modelleri
> [!NOTE]
> Bu genellikle bir veri bilim adamı görevidir.
> 
> 

Araştırmacı veri analizi aşamasından sonra, Machine Learning'de modeller oluşturmak için verileri aşağıdan taramaya hazırız. Bu bölümde, verileri örneklemek için Hive sorgusunun nasıl kullanılacağını gösteriyoruz. Machine Learning daha sonra [Alma Verisi][import-data] modülünden erişir.

### <a name="down-sampling-the-data"></a>Verilerin aşağı örneklemesi
Bu yordamın iki adımı vardır. Önce tüm kayıtları mevcut üç tuşları **nyctaxidb.trip** ve **nyctaxidb.fare** tablolar katılmak: **madalyon**, **hack\_lisans**, ve **pickup\_datetime**. Daha sonra bir ikili sınıflandırma etiketi, **uçlu**ve çok sınıflı sınıflandırma etiketi, **uç\_sınıfı**oluştururuz.

Machine Learning'de alt örneklenmiş verileri doğrudan [Alma Verisi][import-data] modülünden kullanabilmek için, önceki sorgunun sonuçlarını dahili bir Kovan tablosunda depolamanız gerekir. Aşağıda, bir iç Hive tablosu oluşturur ve içeriğini birleştirilmiş ve aşağı örneklenmiş verilerle doldururuz.

Sorgu, **alma\_tarihi** alanından aşağıdaki zaman parametrelerini oluşturmak için standart Hive işlevlerini doğrudan uygular:
- günün saati
- yılın haftası
- hafta içi ('1' Pazartesi ve '7' Pazar anlamına gelir)

Sorgu ayrıca teslim alma ve bırakma konumları arasındaki doğrudan mesafeyi de oluşturur. Bu tür işlevlerin tam listesi [için, Bkz. LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF).

Sorgu daha sonra sorgu sonuçlarının Azure Machine Learning Studio'ya sığabilmesi için verileri aşağı yıkır. Orijinal veri kümesinin yalnızca yaklaşık yüzde 1'i stüdyoya aktarılır.

Burada **\_örnek kovan\_\_\_\_** makineleri öğrenme modeli oluşturma için veri hazırlar aml full.hql dosyası için hazırlamak içeriği şunlardır:

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Bu sorguyu Kovan dizini komut isteminden çalıştırmak için:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Şimdi bir iç tablo var, **nyctaxidb.nyctaxi_downsampled_dataset**, Hangi Machine Learning [İthalat Veri][import-data] modülü kullanılarak erişilebilir. Ayrıca, bu veri kümesini Machine Learning modellerini oluşturmak için kullanabiliriz.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>Aşağıda örneklenmiş verilere erişmek için Machine Learning'de Alma Verisi modüllerini kullanın
Machine Learning'in Alma [Verisi][import-data] modülünde Kovan sorguları vermek için bir Makine Öğrenimi çalışma alanına erişmeniz gerekir. Ayrıca kümenin kimlik bilgilerine ve ilişkili depolama hesabına da erişmeniz gerekir.

[Alma Verileri][import-data] modülü ve giriş parametreleri hakkında bazı ayrıntılar aşağıda verilmiştir:

**HCatalog sunucu URI**: Küme adı **abc123**ise,\/o zaman kullanın: https: /abc123.azurehdinsight.net.

**Hadoop kullanıcı hesabı adı**: Küme için seçilen kullanıcı adı (uzaktan erişim kullanıcı adı değil).

**Hadoop kullanıcı hesabı parolası**: Küme için seçilen parola (uzaktan erişim parolası değil).

**Çıktı verilerinin konumu**: Azure olarak seçildi.

**Azure Depolama hesap adı**: Kümeyle ilişkili varsayılan depolama hesabının adı.

**Azure kapsayıcı adı**: Kümenin varsayılan kapsayıcı adıdır ve genellikle küme adı ile aynıdır. **Abc123**adlı bir küme için, adı abc123 olduğunu.

> [!IMPORTANT]
> Makine Öğreniminde [Alma Verisi][import-data] modülasyonlarını kullanarak sorgulamak istediğimiz herhangi bir tablo dahili tablo olmalıdır.
> 
> 

Db veritabanındaki tablo **T'nin** **D.db** dahili bir tablo olup olmadığını şu şekilde belirleyebilirsiniz. Hive dizin isteminden aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///D.db/T

Tablo bir iç tabloysa ve doldurulursa, içeriği burada gösterilmelidir.

Tablonun dahili tablo olup olmadığını belirlemenin başka bir yolu da Azure Depolama Gezgini kullanmaktır. Kümenin varsayılan kapsayıcı adına gitmek için kullanın ve ardından tablo adına göre filtre uygulayın. Tablo ve içeriği gösterirse, bu tablonun bir iç tablo olduğunu doğrular.

Hive sorgusunun ve [Alma Verisi][import-data] modülünün ekran görüntüsü aşağıda ve

![İthalat Veri modülü için Hive sorgusunun ekran görüntüsü](./media/hive-walkthrough/1eTYf52.png)

Aşağıdan örneklenmiş verilerimiz varsayılan kapsayıcıda bulunduğundan, Machine Learning'in ortaya çıkan Hive sorgusu basittir. Sadece bir **SELECT * FROM\_nyctaxidb.nyctaxi\_downsampled veri**.

Veri kümesi artık Machine Learning modellerini oluşturmak için başlangıç noktası olarak kullanılabilir.

### <a name="build-models-in-machine-learning"></a><a name="mlmodel"></a>Makine Öğreniminde modeller oluşturun
Artık [Machine Learning'de](https://studio.azureml.net)model oluşturma ve model dağıtımına geçebilirsiniz. Veriler, daha önce tanımlanan tahmin sorunlarını ele almada kullanmamız için hazırdır:

- **İkili sınıflandırma**: Bir seyahat için bahşiş ödenip ödenmediğini tahmin etmek.

  **Öğrenci kullanılır:** İki sınıflı lojistik regresyon

  a. Bu sorun için, hedef (veya sınıf) etiketi **uçlu.** Orijinal aşağı örneklenmiş veri kümesinde, bu sınıflandırma denemesi için hedef sızıntıları olan birkaç sütun vardır. Özellikle, **ipucu\_sınıfı,** **\_uç miktarı**ve **\_toplam tutar,** test zamanında kullanılamayan hedef etiket hakkındaki bilgileri ortaya çıkarır. [Dataset modülündeki Sütunları Seç'i][select-columns] kullanarak bu sütunları dikkate alarak kaldırıyoruz.

  Aşağıdaki diyagram, belirli bir seyahat için bir ipucunun ödenip ödenmediğini tahmin etmek için denememizi gösterir:

  ![İpucunun ödenip ödenmediğini tahmin etmek için deneme diyagramı](./media/hive-walkthrough/QGxRz5A.png)

  b. Bu deney için hedef etiket dağılımlarımız kabaca 1:1 idi.

   Aşağıdaki grafik, ikili sınıflandırma sorunu için ipucu sınıfı etiketlerinin dağılımını gösterir:

  ![İpucu sınıfı etiketlerinin dağılım grafiği](./media/hive-walkthrough/9mM4jlD.png)

    Sonuç olarak, aşağıdaki şekilde gösterildiği gibi, 0,987 eğrisi (AUC) altında bir alan elde:

  ![AUC değeri grafiği](./media/hive-walkthrough/8JDT0F8.png)

- **Çok sınıflı sınıflandırma**: Daha önce tanımlanmış sınıfları kullanarak, yolculuk için ödenen bahşiş tutarlarının aralığını tahmin etmek.

  **Öğrenci kullanılır:** Çok sınıflı lojistik regresyon

  a. Bu sorun için, hedef (veya sınıf) etiketi, beş değerden birini (0,1,2,3,4) alabilen **\_uç sınıfıdır.** İkili sınıflandırma durumunda olduğu gibi, bu deney için hedef sızıntıları olan birkaç sütun var. Özellikle, **uçlu,** **bahşiş\_miktarı**ve **\_toplam tutar,** test zamanında kullanılamayan hedef etiket hakkındaki bilgileri ortaya çıkarır. Bu sütunları [Dataset modülündeki Sütunları Seç'i][select-columns] kullanarak kaldırıyoruz.

  Aşağıdaki diyagram, bir ipucunun hangi depo da düşeceğini tahmin etmek için deneyi gösterir. Kutu kutuları şunlardır: Sınıf 0: uç = $0, Sınıf 1: ipucu > $0 ve ipucu <$ 5, Sınıf 2: uç > $5 ve ipucu <= $10, Sınıf 3: ipucu > $10 ve ipucu <= 20 $ ve Sınıf 4: ipucu > 20 TL.

  ![İpucu için depo tahmin etmek için deney diyagramı](./media/hive-walkthrough/5ztv0n0.png)

  Şimdi gerçek test sınıfı dağılımının neye benzediğini gösteriyoruz. Sınıf 0 ve Sınıf 1 yaygındır ve diğer sınıflar nadirdir.

  ![Test sınıfı dağılım grafiği](./media/hive-walkthrough/Vy1FUKa.png)

  b. Bu deneme için, burada gösterildiği gibi tahmin doğrularını görmek için bir karışıklık matrisi kullanırız:

  ![Karışıklık matrisi](./media/hive-walkthrough/cxFmErM.png)

  Yaygın sınıflarda sınıf doğrulukları iyi olsa da, model nadir sınıflarda "öğrenme" iyi bir iş yapmaz.

- **Regresyon görevi**: Bir yolculuk için ödenen bahşiş miktarını tahmin etmek.

  **Öğrenci kullanılır:** Artırılmış karar ağacı

  a. Bu sorun için, hedef (veya sınıf) etiketi **\_uç miktarıdır.** Bu durumda hedef sızıntıları şunlardır: **uçlu**, **uç\_sınıfı**, ve **\_toplam tutar**. Tüm bu değişkenler, genellikle test zamanında kullanılamayan bahşiş miktarı hakkında bilgi ortaya çıkarır. Bu sütunları [Dataset modülündeki Sütunları Seç'i][select-columns] kullanarak kaldırıyoruz.

  Aşağıdaki diyagram, verilen ipucunun miktarını tahmin etmek için deneyi gösterir:

  ![İpucu miktarını tahmin etmek için deneme diyagramı](./media/hive-walkthrough/11TZWgV.png)

  b. Regresyon sorunları için, tahminlerdeki karehataya ve belirleme katsayısına bakarak tahminin doğruluğunu ölçeriz:

  ![Tahmin istatistiklerinin ekran görüntüsü](./media/hive-walkthrough/Jat9mrz.png)

  Burada, tespit katsayısı 0,709 olup, varyansın yaklaşık yüzde 71'inin model katsayıları ile açıklandığını ima eder.

> [!IMPORTANT]
> Machine Learning ve buna nasıl erişip kullanılacağı hakkında daha fazla bilgi edinmek için [Machine Learning nedir'](../studio/what-is-machine-learning.md)e bakın. Buna ek olarak, [Azure AI Galerisi](https://gallery.cortanaintelligence.com/) bir dizi deneyi kapsar ve Makine Öğrenimi'nin çeşitli özelliklerine ayrıntılı bir giriş sağlar.
> 
> 

## <a name="license-information"></a>Lisans bilgileri
Bu örnek gözden geçirme ve beraberindeki komut dosyaları Microsoft tarafından MIT lisansı altında paylaşılır. Daha fazla bilgi için GitHub'daki örnek kodun dizinindeki **LICENSE.txt** dosyasına bakın.

## <a name="references"></a>Başvurular
• [Andrés Monroy NYC Taksi Gezileri İndirme Sayfası](https://www.andresmh.com/nyctaxitrips/)  
• [Chris Whong tarafından NYC's Taksi Trip Veri FOILing](https://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taksi ve Limuzin Komisyonu Araştırma ve İstatistik](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/



