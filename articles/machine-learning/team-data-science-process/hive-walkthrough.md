---
title: Hadoop kümesinde verileri araştırma-takım veri bilimi Işlemi
description: Bir modeli derlemek ve dağıtmak için bir HDInsight Hadoop kümesi kullanan uçtan uca bir senaryo için ekip veri bilimi sürecini kullanma.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81677470"
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Ekip veri bilimi Işlemi sürüyor: Azure HDInsight Hadoop kümelerini kullanma
Bu kılavuzda, [ekip veri bilimi işlemini (TDSP)](overview.md) uçtan uca bir senaryoda kullanırız. Genel kullanıma açık [NYC Taxi](https://www.andresmh.com/nyctaxitrips/) veri kümesinden verileri depolamak, araştırmak ve özellik mühendislerini ve verileri aşağı [örneklemek için bir Azure HDInsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/) kullanırız. İkili ve çok sınıflı sınıflandırmayı ve gerileme tahmine dayalı görevleri işlemek için Azure Machine Learning ile veri modelleri oluşturacağız. 

Daha büyük bir veri kümesini nasıl işleyeceğinizi gösteren bir anlatım için, bkz. [Team Data Science Process-1-TB veri kümesinde Azure HDInsight Hadoop kümeleri kullanma](hive-criteo-walkthrough.md).

Ayrıca, 1 TB 'lık veri kümesini kullanan yönergede sunulan görevleri gerçekleştirmek için bir IPython Not defteri de kullanabilirsiniz. Daha fazla bilgi için bkz. [HIVE ODBC bağlantısı kullanan Criteo anlatımı](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb).

## <a name="nyc-taxi-trips-dataset-description"></a><a name="dataset"></a>NYC TAXI gidiş veri kümesi açıklaması
NYC TAXI seyahat verileri yaklaşık 20 GB sıkıştırılmış virgülle ayrılmış değerler (CSV) dosyası (~ 48 GB sıkıştırılmamış) olur. 173.000.000 ' den fazla ayrı geziye sahiptir ve her seyahat için ücretli olan Fares 'yi içerir. Her seyahat kaydı için alma ve bırakma konumu ve saati, anonimleştirilmiş Hack (sürücü) lisans numarası ve numara (TAXI 'nin benzersiz KIMLIĞI) bulunur. Veriler, 2013 yılında yapılan tüm döngüleri kapsamakta ve her ay için aşağıdaki iki veri kümelerinde sunulmaktadır:

- Trip_data CSV dosyaları seyahat ayrıntıları içeriyor: pascuların sayısı, seçme ve açılan noktaları, seyahat süresi ve seyahat uzunluğu. Aşağıda birkaç örnek kayıt verilmiştir:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
- Trip_fare CSV dosyaları her seyahat için ödenen tarifeli havayolu ayrıntılarını içerir: ödeme türü, tarifeli havayolu miktarı, ek ücret, vergiler, ipuçları ve Tolls ve ödenen toplam tutar. Aşağıda birkaç örnek kayıt verilmiştir:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Seyahat\_verilerine ve seyahat\_tarifeli havayolu katılacak benzersiz anahtar alanlardan oluşur: medtalon, Hack\_lisansı ve toplama\_tarih saati. Belirli bir yolculuğa uygun tüm ayrıntıları almak için bu üç anahtardan birine katılmanız yeterlidir.

## <a name="examples-of-prediction-tasks"></a><a name="mltasks"></a>Tahmin görevlerinin örnekleri
Gerekli işlem görevlerinin açıklanmasına yardımcı olmak için veri analizinden göre yapmak istediğiniz tahmine yönelik türü belirleme. İşte bu kılavuzda, tüm *tıp\_miktarına*bağlı olarak ele aldığımız tahmin sorunlarına yönelik üç örnek verilmiştir:

- **İkili sınıflandırma**: bir tıp bir seyahat için ödenip ödenmediğini tahmin edin. Diğer bir deyişle, $0 'den büyük bir *İpucu\_miktarı* pozitif bir örnektir, ancak bir $0 *İpucu\_miktarı* negatif bir örnektir.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
- **Birden çok Lass sınıflandırması**: seyahat için ödenen ipucu tutarlarının aralığını tahmin edin. *İpucu\_miktarını* beş sınıfa bölyoruz:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
- **Regresyon görevi**: seyahat için ödenen ipucunun miktarını tahmin edin.  

## <a name="set-up-an-hdinsight-hadoop-cluster-for-advanced-analytics"></a><a name="setup"></a>Gelişmiş analiz için bir HDInsight Hadoop kümesi ayarlama
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Üç adımda HDInsight kümesini kullanan gelişmiş analizler için bir Azure ortamı ayarlayabilirsiniz:

1. [Depolama hesabı oluştur](../../storage/common/storage-account-create.md): Bu depolama hesabı, verileri Azure Blob depolama alanında depolamak için kullanılır. HDInsight kümelerinde kullanılan veriler de burada bulunur.
2. [Gelişmiş analiz işlemi ve teknoloji için Azure HDInsight Hadoop kümelerini özelleştirin](customize-hadoop-cluster.md). Bu adım, tüm düğümlerde yüklü 64 bit Anaconda Python 2,7 ile bir HDInsight Hadoop kümesi oluşturur. HDInsight kümenizi özelleştirirken hatırlanması gereken iki önemli adım vardır.
   
   * Adım 1 ' de oluşturulan depolama hesabını, oluşturduğunuz zaman HDInsight kümeniz ile bağlamayı unutmayın. Bu depolama hesabı, küme içinde işlenen verilere erişir.
   * Kümeyi oluşturduktan sonra, kümenin baş düğümüne uzaktan erişimi etkinleştirin. **Yapılandırma** sekmesine gidin ve **Uzaktan Etkinleştir**' i seçin. Bu adım, uzaktan oturum açma için kullanılan Kullanıcı kimlik bilgilerini belirtir.
3. [Azure Machine Learning çalışma alanı oluştur](../studio/create-workspace.md): Machine Learning modelleri oluşturmak için bu çalışma alanını kullanın. Bu görev, HDInsight kümesi kullanılarak ilk veri araştırması ve aşağı örnekleme tamamlandıktan sonra karşılanır.

## <a name="get-the-data-from-a-public-source"></a><a name="getdata"></a>Verileri ortak bir kaynaktan al
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

[NYC TAXI gidiş](https://www.andresmh.com/nyctaxitrips/) veri kümesini makinenize ortak konumundan kopyalamak Için [verileri Azure Blob depolama alanına taşıma](move-azure-blob.md)bölümünde açıklanan yöntemlerden herhangi birini kullanın.

Burada, AzCopy kullanarak verileri içeren dosyaları nasıl aktaracağımız açıklanmıştır. AzCopy indirmek ve yüklemek için [AzCopy komut satırı yardımcı programı ile çalışmaya](../../storage/common/storage-use-azcopy.md)başlama bölümündeki yönergeleri izleyin.

1. Komut istemi penceresinde, aşağıdaki AzCopy komutlarını çalıştırarak * \<path_to_data_folder>* istenen hedefle değiştirin:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Kopyalama tamamlandığında, seçilen veri klasöründe toplam 24 daraltılmış dosya görürsünüz. İndirilen dosyaları yerel makinenizde aynı dizine ayıklayın. Sıkıştırılmamış dosyaların bulunduğu klasörü bir yere göz önünde alın. Bu klasör, aşağıdaki * \<\_\_\_unzipped_data\> dosyaların yolu* olarak adlandırılır.

## <a name="upload-the-data-to-the-default-container-of-the-hdinsight-hadoop-cluster"></a><a name="upload"></a>HDInsight Hadoop kümesinin varsayılan kapsayıcısına verileri yükleme
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Aşağıdaki AzCopy komutlarında, aşağıdaki parametreleri Hadoop kümesini oluştururken belirttiğiniz gerçek değerlerle değiştirin ve veri dosyalarının sıkıştırmasını geri gönderin.

* *** \<path_to_data_folder>*** Makinenizde, ZIP üzerinde olmayan veri dosyalarını içeren dizin (yol ile birlikte).  
* ***Hadoop kümesi>depolama hesabı adı \<*** HDInsight kümeniz ile ilişkili depolama hesabı.
* ***Hadoop kümesi>varsayılan kapsayıcısı \<*** Kümeniz tarafından kullanılan varsayılan kapsayıcı. Varsayılan kapsayıcının adı genellikle kümenin kendisiyle aynı addır. Örneğin, küme "abc123.azurehdinsight.net" olarak adlandırıldıysanız, varsayılan kapsayıcı abc123 olur.
* depolama hesabı anahtarı>*** \<*** Kümeniz tarafından kullanılan depolama hesabı için anahtar.

Bir komut isteminden veya bir Windows PowerShell penceresinde aşağıdaki iki AzCopy komutunu çalıştırın.

Bu komut, seyahat verilerini Hadoop kümesinin varsayılan kapsayıcısındaki ***nyctaxitripraw*** dizinine yükler.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Bu komut, tarifeli havayolu verilerini Hadoop kümesinin varsayılan kapsayıcısındaki ***nyctaxifareraw*** dizinine yükler.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Verilerin artık BLOB depolama alanında olması ve HDInsight kümesi içinde tüketilmeye hazır olması gerekir.

## <a name="sign-in-to-the-head-node-of-hadoop-cluster-and-prepare-for-exploratory-data-analysis"></a><a name="#download-hql-files"></a>Hadoop kümesinin baş düğümünde oturum açın ve araştırmacı veri analizi hazırlayın
> [!NOTE]
> Bu genellikle bir yönetim görevidir.
> 
> 

Keşif veri analizi ve verilerin aşağı örneklemesini sağlamak üzere kümenin baş düğümüne erişmek için, [Hadoop kümesinin baş düğümüne erişme](customize-hadoop-cluster.md)bölümünde özetlenen yordamı izleyin.

Bu kılavuzda, aslında SQL benzeri bir sorgu dili olan [Hive](https://hive.apache.org/)içinde yazılmış sorguları, ön veri araştırmalarını gerçekleştirecek şekilde kullanacağız. Hive sorguları '. HQL ' dosyalarında depolanır. Daha sonra bu verileri modeller oluşturmak için Machine Learning içinde kullanılacak şekilde örnekliyoruz.

Kümeyi araştırmacı veri analizi için hazırlamak üzere [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) 'Dan ilgili Hive betiklerini içeren '. HQL ' dosyalarını baş düğümde bir yerel dizine (C:\Temp) indirin. Kümenin baş düğümü içinden komut istemi ' ni açın ve aşağıdaki iki komutu çalıştırın:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Bu iki komut, bu kılavuzda gereken tüm '. HQL ' dosyalarını baş düğümdeki ***C:\temp&#92;*** yerel dizinine indirir.

## <a name="create-hive-database-and-tables-partitioned-by-month"></a><a name="#hive-db-tables"></a>Aya göre bölümlenmiş Hive veritabanı ve tabloları oluşturma
> [!NOTE]
> Bu görev genellikle yönetici içindir.
> 
> 

Şimdi NYC TAXI veri kümesi için Hive tabloları oluşturmaya hazırsınız.
Hadoop kümesinin baş düğümünde, baş düğümün masaüstündeki Hadoop komut satırını açın. Aşağıdaki komutu çalıştırarak Hive dizinini girin:

    cd %hive_home%\bin

> [!NOTE]
> Bu izlenecek yolda bulunan tüm Hive komutlarını Hive bin/Dizin isteminden çalıştırın. Bu, tüm yol sorunlarını otomatik olarak işler. Bu izlenecek yolda "Hive Dizin istemi", "Hive bin/Dizin istemi" ve "Hadoop komut satırı" terimlerini kullanıyoruz.
> 
> 

Hive Dizin isteminde, Hive veritabanını ve tablolarını oluşturan baş düğümün Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Burada, **Nyctaxidb**Hive veritabanını oluşturan **\_DB\_ve\_Tables\_. HQL\_** dosyasının içeriği ve **seyahat** ve **tarifeli havayolu**tabloları oluşturulur.

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

Bu Hive betiği iki tablo oluşturur:

* **Seyahat** tablosu her bir arttırıldığında (sürücü ayrıntıları, seçme saati, seyahat uzaklığı ve saatler) için seyahat ayrıntılarını içerir.
* **Tarifeli havayolu** tablosu tarifeli havayolu ayrıntılarını (tarifeli havayolu Amount, tıp Amount, Tolls ve surcharges) içerir.

Bu yordamlarla ilgili ek yardıma ihtiyacınız varsa veya alternatif olanları araştırmak isterseniz, [Hive sorgularını doğrudan Hadoop komut satırından gönderme](move-hive-tables.md#submit)bölümüne bakın.

## <a name="load-data-to-hive-tables-by-partitions"></a><a name="#load-data"></a>Bölümlere göre Hive tablolarına veri yükleme
> [!NOTE]
> Bu görev genellikle yönetici içindir.
> 
> 

NYC TAXI veri kümesinde, daha hızlı işleme ve sorgu sürelerini etkinleştirmek için kullandığımız ayda bir doğal bölümlendirme vardır. Aşağıdaki PowerShell komutları (Hadoop komut satırı kullanılarak Hive dizininden verilir), aya göre bölümlenen seyahat ve tarifeli havayolu Hive tablolarına veri yükler.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

**Sample\_Hive\_, Partitions\_.\_HQL dosyasına göre\_yükleme verileri** aşağıdaki **yükleme** komutlarını içerir:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Araştırma sürecinde burada kullanılan Hive sorgularının sayısı yalnızca bir veya iki bölüme bakmayı içerir. Ancak bu sorgular tüm veri kümesi genelinde çalıştırılabilir.

### <a name="show-databases-in-the-hdinsight-hadoop-cluster"></a><a name="#show-db"></a>HDInsight Hadoop kümesindeki veritabanlarını gösterme
HDInsight Hadoop kümesinde oluşturulan veritabanlarını Hadoop komut satırı penceresinde göstermek için, Hadoop komut satırı içinde aşağıdaki komutu çalıştırın:

    hive -e "show databases;"

### <a name="show-the-hive-tables-in-the-nyctaxidb-database"></a><a name="#show-tables"></a>**Nyctaxidb** veritabanında Hive tablolarını gösterme
**Nyctaxidb** veritabanındaki tabloları göstermek Için, Hadoop komut satırında aşağıdaki komutu çalıştırın:

    hive -e "show tables in nyctaxidb;"

Aşağıdaki komutu çalıştırarak tabloların bölümlendiğini doğrulayabiliriz:

    hive -e "show partitions nyctaxidb.trip;"

Beklenen çıkış şu şekildedir:

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

Benzer şekilde, aşağıdaki komutu çalıştırarak tarifeli havayolu tablosunun bölümlenmiş olduğundan emin olabilirsiniz:

    hive -e "show partitions nyctaxidb.fare;"

Beklenen çıkış şu şekildedir:

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

## <a name="data-exploration-and-feature-engineering-in-hive"></a><a name="#explore-hive"></a>Hive 'de veri araştırması ve özellik Mühendisliği
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

Hive tablolarına yüklenen veriler için veri keşif ve özellik mühendisliği görevlerini gerçekleştirmek üzere Hive sorgularını kullanabilirsiniz. Bu tür görevlere örnekler aşağıda verilmiştir:

* Her iki tabloda da ilk 10 kaydı görüntüleyin.
* Değişen zaman pencereleri içinde birkaç alanın veri dağıtımlarını araştırma.
* Boylam ve enlem alanlarının veri kalitesini araştırın.
* Tıp miktarına göre ikili ve çoklu sınıf sınıflandırma etiketleri oluşturun.
* Doğrudan seyahat mesafelerini hesaplayarak Özellikler oluşturun.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Araştırma: tablo yolculuğuna ilk 10 kaydı görüntüleme
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

Verilerin nasıl göründüğünü görmek için, her tablodan 10 kayıt inceleyin. Kayıtları incelemek için aşağıdaki iki sorguyu Hadoop komut satırı konsolundaki Hive Dizin isteminden ayrı olarak çalıştırın.

İlk ayın seyahat tablosunda ilk 10 kaydı almak için:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

İlk ayın tarifeli havayolu tablosundaki ilk 10 kaydı almak için:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Önceki sorguda küçük bir değişiklik ile uygun bir şekilde görüntülemek için kayıtları bir dosyaya kaydedebilirsiniz:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Araştırma: 12 bölümden her birinde kayıt sayısını görüntüleme
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

Bu, gün sayısının takvim yılı sırasında değişiklik gösterdiği bir sayıdır. Aya göre gruplandırma, gidiş dağılımını gösterir.

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

Burada ilk sütun month, ikincisi ise o aya ait gidiş dönüş sayısıdır.

Ayrıca, Hive Dizin isteminde aşağıdaki komutu çalıştırarak seyahat veri kümizdeki toplam kayıt sayısını da saybiliriz:

    hive -e "select count(*) from nyctaxidb.trip;"

Bu komut şunları verir:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Seyahat veri kümesi için gösterilenlere benzer komutları kullanarak, kayıt sayısını doğrulamak için tarifeli havayolu veri kümesinin Hive Dizin isteminden Hive sorguları yayımlayabiliriz.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Bu komut bu çıktıyı oluşturur:

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

Her iki veri kümesi için de tam olarak aynı gidiş dönüş sayısı döndürülür ve verilerin doğru şekilde yüklendiği ilk doğrulamayı sağlar.

Hive Dizin isteminde aşağıdaki komutu kullanarak tarifeli havayolu veri kümesindeki toplam kayıt sayısını saymanız gerekir:

    hive -e "select count(*) from nyctaxidb.fare;"

Bu komut şunları verir:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Her iki tablodaki kayıtların toplam sayısı aynı zamanda, verilerin doğru şekilde yüklendiğinden ikinci bir doğrulama sağlamak için de aynıdır.

### <a name="exploration-trip-distribution-by-medallion"></a>Araştırma: medtalon tarafından seyahat dağılımı
> [!NOTE]
> Bu analiz genellikle bir veri bilimi görevidir.
> 
> 

Bu örnek, belirli bir süre içinde 100 ' den fazla dönüşle birlikte medalons (TAXI numaraları) tanımlar. Bölümlenmiş tablo erişiminizden sorgu avantajları, Bölüm değişkeninin **aya**göre belirlenir. Sorgu sonuçları, ' de `C:\temp` baş düğümdeki querbir yerel dosyaya, **quergı put. tsv**dosyasına yazılır.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

İnceleme için **medtalon. HQL\_dosyasına\_göre\_\_örnek\_Hive seyahat sayısı** içeriği aşağıda verilmiştir.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

NYC TAXI veri kümesindeki medalon, benzersiz bir cab tanımlıyor. Belirli bir süre içinde belirli sayıda gidiş 'tan daha fazlasını yaptığını isteyerek hangi Cabs 'nin karşılaştırılanmasının ne olduğunu belirleyebilirsiniz. Aşağıdaki örnek, ilk üç ayda yüz 'dan fazla bir dönüşten daha fazla olan Cabs 'yi tanımlar ve sorgu sonuçlarını **C:\temp\quer47put.exe**yerel dosyasına kaydeder.

İnceleme için **medtalon. HQL\_dosyasına\_göre\_\_örnek\_Hive seyahat sayısı** içeriği aşağıda verilmiştir.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Hive Dizin isteminde aşağıdaki komutu çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Araştırma: medtalon ve Hack lisansınıza göre seyahat dağılımı
> [!NOTE]
> Bu görev genellikle bir veri bilimcisini için kullanılır.
> 
> 

Bir veri kümesini araştırırken, genellikle değer gruplarının dağıtımlarını incelemek istiyoruz. Bu bölüm, cab ve sürücüler için bu çözümlemenin nasıl yapılacağını gösteren bir örnek sağlar.

License. **hack_license**HQL dosyası tarafından **medallion** **\_\_\_\_\_, tarifeli havayolu veri kümesini ve hack_license ve her birleşimin sayısını döndüren örnek Hive seyahat sayısı.\_** İçerik şu şekildedir:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Bu sorgu, sıralı dönüşlere göre sıralanan cab ve sürücü kombinasyonlarını döndürür.

Hive Dizin isteminde şunu çalıştırın:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Sorgu sonuçları, **C:\temp\quer, put.exe**adlı yerel bir dosyaya yazılır.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Araştırma: geçersiz Boylam veya enlem kayıtlarını denetleyerek veri kalitesini değerlendirme
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

Araştırmacı veri analizinin ortak amacı, geçersiz veya hatalı kayıtları almak için kullanılır. Bu bölümdeki örnek, Boylam ya da Enlem alanlarının NYC alanının dışından bir değer içerip içermediğini belirler. Bu tür kayıtlar hatalı Boylam-Enlem değerine sahip olduğundan, bunları modelleme için kullanılacak verilerden ortadan kaldırmak istiyoruz.

İnceleme için **örnek\_Hive\_Quality\_Assessment. HQL** dosyasının içeriği aşağıda verilmiştir.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Hive Dizin isteminde şunu çalıştırın:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Bu komutta yer alan *-S* bağımsız değişkeni Hive harita/azaltma işlerinin durum ekranı çıktısını bastırır. Bu komut, Hive sorgu çıkışının ekran yazdırmasını daha okunaklı hale getiren yararlı olur.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Araştırma: geçiş ipuçları için Ikili sınıf dağıtımları
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

[Tahmin görevleri örnekleri](hive-walkthrough.md#mltasks) bölümünde özetlenen ikili sınıflandırma sorunu için, bir ipucunun verilip verilmediğini bilmemiz yararlı olur. Bu ipuçlarının dağıtımı ikili bir değer:

* İpucu verildi (sınıf 1, ipucu\_miktarı > $0)  
* İpucu yok (Sınıf 0, ipucu\_tutarı = $0)

Aşağıdaki **örnek\_Hive\_eğimli\_frekansları. HQL** dosyası çalıştırılacak komutu gösterir:

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Hive Dizin isteminde şunu çalıştırın:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Araştırma: birden çok Lass ayarında sınıf dağıtımları
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

[Tahmin görevleri örnekleri](hive-walkthrough.md#mltasks) bölümünde özetlenen birden çok Lass sınıflandırma sorunu için, bu veri kümesi aynı zamanda verilen ipuçlarının miktarını tahmin etmek için kendisini doğal bir sınıflandırmayla da özetler. Sorgudaki tıp aralıklarını tanımlamak için depo gözleri kullanabiliriz. Çeşitli tıp aralıklarına yönelik sınıf dağıtımlarını almak için, **\_örnek Hive\_İpucu\_aralığı\_frekansları. HQL** dosyasını kullanın. İçeriği burada bulabilirsiniz.

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

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>Araştırma: iki Boylam-Enlem konumu arasındaki doğrudan mesafeyi hesaplama
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

İki konum arasındaki doğrudan mesafe ile TAXI 'nin gerçek seyahat uzaklığı arasında bir fark olup olmadığını bilmeniz gerekebilir. Bir pastger, sürücünün kasıtlı olarak daha uzun bir yol ile alınmış olduğunu anlarsa ipucu daha az olabilir.

Gerçek seyahat mesafesi ile iki boylam-Latitude noktaları arasındaki [Haversinüsü](https://en.wikipedia.org/wiki/Haversine_formula) arasındaki karşılaştırmayı görmek için ("harika daire" mesafesini), Hive içinde bulunan trigonometrik işlevleri kullanabilirsiniz:

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

Yukarıdaki sorguda, R, mil 'deki dünya yarıçapdır ve Pi, radyana dönüştürülür. Boylam-Enlem noktaları, NYC alanından uzak değerleri kaldıracak şekilde filtrelenmiştir.

Bu durumda, sonuçları **querbir putdir**adlı dizine yazdık. Aşağıdaki komutların sırası önce bu çıkış dizinini oluşturur ve Hive komutunu çalıştırır.

Hive Dizin isteminde şunu çalıştırın:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Sorgu sonuçları, Hadoop kümesinin varsayılan kapsayıcısı altında dokuz Azure Blob 'a (**quer0,0,putdir/000000 yazın\_0-quer0,0,putdir** **/000008\_0**) yazılır.

Ayrı Blobların boyutunu görmek için Hive Dizin isteminde aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///queryoutputdir

Belirli bir dosyanın içeriğini görmek için, **000000 yazın\_0**deyin, Hadoop 'ın `copyToLocal` komutunu kullanın.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`büyük dosyalar için çok yavaş olabilir ve bunlarla birlikte kullanılması önerilmez.  
> 
> 

Bu verilerin bir Azure blobuna sahip olmasının önemli bir avantajı, [verileri Içeri aktarma][import-data] modülünü kullanarak Machine Learning içindeki verileri keşfedebiliriz.

## <a name="down-sample-data-and-build-models-in-machine-learning"></a><a name="#downsample"></a>Machine Learning 'de aşağı örnek veri ve derleme modelleri
> [!NOTE]
> Bu genellikle bir veri bilimi görevidir.
> 
> 

Araştırmacı veri analizi aşamasından sonra, Machine Learning model oluşturmaya yönelik verileri şimdi kullanıma hazır hale getirdik. Bu bölümde, verileri aşağı örneklemek için Hive sorgusunun nasıl kullanılacağını göstereceğiz. Machine Learning [verileri Içeri aktarma][import-data] modülünden erişir.

### <a name="down-sampling-the-data"></a>Verileri aşağı örnekleme
Bu yordamda iki adım vardır. İlk olarak, **nyctaxidb. seyahat** ve **nyctaxidb. tarifeli havayolu** tablolarını tüm kayıtlarda bulunan üç anahtara katıyoruz: **medtalon**, **Hack\_lisansı**ve **toplama\_tarih saati**. Daha sonra bir ikili sınıflandırma etiketi, **eğimli**ve çok sınıflı bir sınıflandırma etiketi, **tıp\_sınıfı**oluşturacağız.

Machine Learning ' deki [verileri Içeri aktarma][import-data] modülünden doğrudan örnek olarak kullanabilmek için, önceki sorgunun sonuçlarını bir iç Hive tablosuna depolamanız gerekir. Aşağıdaki şekilde, bir iç Hive tablosu oluşturur ve içeriğini birleştirilmiş ve aşağı örneklenir verilerle doldurduk.

Sorgu, **\_toplama tarih/** saati alanından aşağıdaki zaman parametrelerini oluşturmak için standart Hive işlevlerini doğrudan uygular:
- Günün saati
- yılın haftası
- hafta içi (' 1 ' Pazartesi için gelir ve ' 7 ' Pazar günü temsil eder)

Sorgu ayrıca, çekme ve açılan konumlar arasında doğrudan mesafe oluşturur. Bu işlevlerin tüm listesi için bkz. [Languagemanual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF).

Sorgu daha sonra sorgu sonuçlarının Azure Machine Learning Studio sığması için verileri aşağı örnekler. Özgün veri kümesinin yalnızca yüzde 1 ' i Studio 'ya aktarılır.

Machine Learning ' de model oluşturmaya yönelik verileri hazırlayan **\_AML\_Full\_. HQL\_\_** dosyasının içeriği aşağıda verilmiştir:

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

Bu sorguyu Hive Dizin isteminden çalıştırmak için:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Artık, Machine Learning [veri alma][import-data] modülü kullanılarak erişilebilen **nyctaxidb. nyctaxi_downsampled_dataset**iç tablosuna sahipsiniz. Ayrıca, bu veri kümesini Machine Learning modelleri oluşturmak için de kullanabiliriz.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>Aşağı örneklenir verilere erişmek için Machine Learning veri alma modülünü kullanın
Machine Learning [veri Içeri aktarma][import-data] modülünde Hive sorguları vermek için, bir Machine Learning çalışma alanına erişmeniz gerekir. Ayrıca kümenin kimlik bilgilerine ve ilişkili depolama hesabına erişmeniz gerekir.

[Veri Içeri aktarma][import-data] modülü ve giriş için parametreler hakkında bazı ayrıntılar aşağıda verilmiştir:

**Hcatalog sunucusu URI 'si**: küme adı **abc123**ise, şunu kullanın: https:\//abc123.azurehdinsight.net.

**Hadoop Kullanıcı hesabı adı**: küme için seçilen Kullanıcı adı (Uzaktan Erişim Kullanıcı adı değil).

**Hadoop Kullanıcı hesabı parolası**: küme için seçilen parola (uzaktan erişim parolası değil).

**Çıkış verilerinin konumu**: Azure olarak seçildi.

**Azure depolama hesabı adı**: kümeyle ilişkili varsayılan depolama hesabının adı.

**Azure Container Name**: küme için varsayılan kapsayıcı adı ve genellikle küme adıyla aynıdır. **Abc123**adlı bir küme için ad abc123 olur.

> [!IMPORTANT]
> Machine Learning [veri alma][import-data] modülünü kullanarak sorgulamak istediğimiz herhangi bir tablo, iç tablo olmalıdır.
> 
> 

Burada, **D. db** veritabanındaki **bir tablonun iç tablo olup** olmadığı nasıl saptanamıyor. Hive Dizin isteminde aşağıdaki komutu çalıştırın:

    hdfs dfs -ls wasb:///D.db/T

Tablo bir iç tablo ise ve doldurulduğundan, içeriğinin burada gösterilmesi gerekir.

Bir tablonun iç tablo olup olmadığını belirlemenin başka bir yolu Azure Depolama Gezgini kullanmaktır. Kümenin varsayılan kapsayıcı adına gidip tablo adına göre filtre uygulamak için bunu kullanın. Tablo ve içeriği görünmüyorsa bu, bir iç tablo olduğunu onaylar.

Hive sorgusunun ve [veri Içeri aktarma][import-data] modülünün bir ekran görüntüsü aşağıda verilmiştir:

![Veri Içeri aktarma modülünün Hive sorgusunun ekran görüntüsü](./media/hive-walkthrough/1eTYf52.png)

Aşağı örneklerimizin verileri varsayılan kapsayıcıda bulunduğundan, Machine Learning elde edilen Hive sorgusu basittir. Yalnızca bir **select * from nyctaxidb. nyctaxi\_downörneklenmiş\_veri**.

Veri kümesi artık Machine Learning modelleri oluşturmak için başlangıç noktası olarak kullanılabilir.

### <a name="build-models-in-machine-learning"></a><a name="mlmodel"></a>Machine Learning modelleri derleme
Artık [Machine Learning](https://studio.azureml.net)' de model oluşturma ve model dağıtımına devam edebilirsiniz. Veriler, daha önce tanımlanan tahmin sorunlarını ele almak için kullanıma yöneliktir:

- **İkili sınıflandırma**: bir ipucunun seyahat için ödenip ödenmediğini tahmin etmek için.

  **Öğrenner:** İki sınıf Lojistik gerileme

  a. Bu sorun için, hedef (veya sınıf) etiketi **eğimli**olur. Orijinal aşağı örneklenir veri kümesinde, bu sınıflandırma denemesi için hedef sızıntı olan birkaç sütun vardır. Özellikle, **\_tıp sınıfı**, **tıp\_miktarı**ve **Toplam\_tutar** , test sırasında kullanılamayan hedef etiketle ilgili bilgileri açığa çıkarır. Bu sütunları, [veri kümesindeki sütunları seçme][select-columns] modülünde kullanarak göz önünde çıkardık.

  Aşağıdaki diyagramda, belirli bir seyahat için bir ipucunun ödenip ödenmediğini tahmin etmeye yönelik denediğimiz bir ipucu gösterilmektedir:

  ![İpucunun ücretli olduğunu tahmin etmek için deneme diyagramı](./media/hive-walkthrough/QGxRz5A.png)

  b. Bu deneme için, hedef etiket dağıtımlarımız kabaca 1:1.

   Aşağıdaki grafikte, ikili sınıflandırma sorunu için tip sınıfı etiketlerinin dağılımı gösterilmektedir:

  ![İpucu sınıfı etiketlerinin dağılımı grafiği](./media/hive-walkthrough/9mM4jlD.png)

    Sonuç olarak, aşağıdaki şekilde gösterildiği gibi 0,987 eğrisi (AUC) altında bir alan elde ediyoruz:

  ![AUC değeri grafiği](./media/hive-walkthrough/8JDT0F8.png)

- **Birden çok Lass sınıflandırması**: daha önce tanımlanan sınıfları kullanarak seyahat için ödenen ipucu tutarlarının aralığını tahmin etmek için.

  **Öğrenner:** Birden çok Lass Lojistik gerileme

  a. Bu sorun için, hedef (veya sınıf) etiketimiz **\_, beş**değerden birini (0, 1, 2, 3, 4) alabilir. İkili sınıflandırma durumunda olduğu gibi, bu deneme için hedef sızıntı olan birkaç sütunumuz vardır. Özellikle, **eğimli**, **tıp\_miktarı**ve **Toplam\_tutar** , test sırasında kullanılamayan hedef etiketle ilgili bilgileri açığa çıkarır. Bu sütunları, [veri kümesindeki sütunları seç][select-columns] modülünde kullanarak kaldırdık.

  Aşağıdaki diyagramda, bir ipucunun ne kadar ağır olduğunu tahmin etmeye yönelik deneme gösterilmektedir. Sepetler: Sınıf 0: İpucu = $0, sınıf 1: İpucu > $0 ve ipucu <= $5, sınıf 2: İpucu > $5 ve ipucu <= $10, Class 3: İpucu > $10 ve ipucu <= $20 ve sınıf 4: İpucu > $20.

  ![İpucu için bin 'i tahmin etmek için deneme diyagramı](./media/hive-walkthrough/5ztv0n0.png)

  Artık gerçek test sınıfı dağıtımının nasıl göründüğünü göstereceğiz. Sınıf 0 ve sınıf 1 yaygındır ve diğer sınıflar nadir olarak bulunur.

  ![Test sınıfı dağıtımının grafiği](./media/hive-walkthrough/Vy1FUKa.png)

  b. Bu deneme için, burada gösterildiği gibi tahmin accuracies bakmak için bir karışıklık matrisi kullanırız:

  ![Karışıklık matrisi](./media/hive-walkthrough/cxFmErM.png)

  Yaygın sınıfların accuracies sınıfı iyi olsa da, model, rarer sınıflarında "öğrenme" işleminin iyi bir işini yapmaz.

- **Regresyon görevi**: seyahat için ödenen ipucu miktarını tahmin etmek için.

  **Öğrenner:** Artırılmış karar ağacı

  a. Bu sorun için, hedef (veya sınıf) etiketi **tıp\_miktarıdır**. Bu durumda hedef sızıntıları: **eğimli**, **tıp\_sınıfı**ve **Toplam\_miktar**. Tüm bu değişkenler, genellikle test sırasında kullanılamayan ipucu miktarı hakkındaki bilgileri açığa çıkarır. Bu sütunları, [veri kümesindeki sütunları seç][select-columns] modülünde kullanarak kaldırdık.

  Aşağıdaki diyagramda verilen ipucunun miktarını tahmin etmeye yönelik deneme gösterilmektedir:

  ![İpucu miktarını tahmin etmek için deneme diyagramı](./media/hive-walkthrough/11TZWgV.png)

  b. Gerileme sorunları için tahminlerde kare içine girerek tahmin accuracies ölçüyoruz ve belirleme katsayısı:

  ![Tahmin istatistiklerinin ekran görüntüsü](./media/hive-walkthrough/Jat9mrz.png)

  Burada, belirleme katsayısı 0,709 ' dir; farkın yüzde 71 ' i, model katsayısının altında açıklanmıştır.

> [!IMPORTANT]
> Machine Learning hakkında daha fazla bilgi edinmek ve nasıl erişebileceğiniz ve kullanılacağı hakkında daha fazla bilgi için bkz. [Machine Learning](../studio/what-is-machine-learning.md). Ayrıca, [Azure yapay zeka Galerisi](https://gallery.cortanaintelligence.com/) denemeleri bir gamutu kapsamakta ve Machine Learning özellikleri hakkında kapsamlı bir giriş sağlar.
> 
> 

## <a name="license-information"></a>Lisans bilgileri
Bu örnek adım adım ve ilgili betikler, Microsoft tarafından MıT lisansı kapsamında paylaşılır. Daha fazla bilgi için GitHub 'daki örnek kodun dizinindeki **LICENSE. txt** dosyasına bakın.

## <a name="references"></a>Başvurular
• [Andr, Monroy NYC Taxi seyahatler Indirme sayfası](https://www.andresmh.com/nyctaxitrips/)  
• [Cwhong 'e göre NYC 'Nin TAXI veri yolculuğuna çıkılıyor](https://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi ve Limousine Komisyonu araştırması ve istatistikleri](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/



